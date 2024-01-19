The object definition file for QEMU/[[QOM]]

## object_initialize_child_internal

The 'internal' function calls the 'with_props' function. It just adds a reference to `error_abort` in to the function call so that the internal function is not required to do so.

```c
	void object_initialize_child_internal(Object *parent,
                                      const char *propname,
                                      void *child, size_t size,
                                      const char *type)
{
    object_initialize_child_with_props(parent, propname, child, size, type,
                                       &error_abort, NULL);
}
```

## object_initialize_child_with_props

The 'with_props' function calls the 'with_propsv' function. This is simply to parse the `va_list vargs` from the variables left in the Error variable.

```c
bool object_initialize_child_with_props(Object *parentobj,
                                        const char *propname,
                                        void *childobj, size_t size,
                                        const char *type,
                                        Error **errp, ...)
{
    va_list vargs;
    bool ok;

    va_start(vargs, errp);
    ok = object_initialize_child_with_propsv(parentobj, propname,
                                             childobj, size, type, errp,
                                             vargs);
    va_end(vargs);
    return ok;
}

```

## object_initialize_child_with_propsv

This function actually calls the object_initialize function. It requires a parent object, a property name, and a child object as well as some other parameters.

This will attempt to create the child objects and set the correct properties for it. 
```c
bool object_initialize_child_with_propsv(Object *parentobj,
                                         const char *propname,
                                         void *childobj, size_t size,
                                         const char *type,
                                         Error **errp, va_list vargs)
{
    bool ok = false;
    Object *obj;
    UserCreatable *uc;

    object_initialize(childobj, size, type);
    obj = OBJECT(childobj);

    if (!object_set_propv(obj, errp, vargs)) {
        goto out;
    }

    object_property_add_child(parentobj, propname, obj);

    uc = (UserCreatable *)object_dynamic_cast(obj, TYPE_USER_CREATABLE);
    if (uc) {
        if (!user_creatable_complete(uc, errp)) {
            object_unparent(obj);
            goto out;
        }
    }

    ok = true;

out:
    /*
     * We want @obj's reference to be 1 on success, 0 on failure.
     * On success, it's 2: one taken by object_initialize(), and one
     * by object_property_add_child().
     * On failure in object_initialize() or earlier, it's 1.
     * On failure afterwards, it's also 1: object_unparent() releases
     * the reference taken by object_property_add_child().
     */
    object_unref(obj);
    return ok;
}
```

## object_initialize

This function is a wrapper for `object_initialize_with_type`. It will find the type from the name using a helper function, and then ensure that the type is either in the `CONFIG_MODULES` definitions, or that the type exists.

```c
void object_initialize(void *data, size_t size, const char *typename)
{
    TypeImpl *type = type_get_by_name(typename);

#ifdef CONFIG_MODULES
    if (!type) {
        int rv = module_load_qom(typename, &error_fatal);
        if (rv > 0) {
            type = type_get_by_name(typename);
        } else {
            error_report("missing object type '%s'", typename);
            exit(1);
        }
    }
#endif
    if (!type) {
        error_report("missing object type '%s'", typename);
        abort();
    }

    object_initialize_with_type(data, size, type);
}
```

## object_initialize_with_type

This is the actual object initialization function. it will initialize a type, and check to ensure size, not abstract and size $\geq$ type->instance_size.
Then it will do the `memset`, and obj struct filling required, and then call the object initializer and post initializer 

```c
static void object_initialize_with_type(Object *obj, size_t size, TypeImpl *type)
{
    type_initialize(type);

    g_assert(type->instance_size >= sizeof(Object));
    g_assert(type->abstract == false);
    g_assert(size >= type->instance_size);

    memset(obj, 0, type->instance_size);
    obj->class = type->class;
    object_ref(obj);
    object_class_property_init_all(obj);
    obj->properties = g_hash_table_new_full(g_str_hash, g_str_equal,
                                            NULL, object_property_free);
    object_init_with_type(obj, type);
    object_post_init_with_type(obj, type);
}
````