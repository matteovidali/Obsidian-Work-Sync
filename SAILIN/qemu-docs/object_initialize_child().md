# Location & Definition
Comes from: **qemu/include/[[QOM]]/[[object.h]]**
and is defined in: **qemu/[[QOM]]/[[object.c]]** 

## Prototype
```c
void object_initialize_child_internal(Object *parent, const char *propname,
                                      void *child, size_t size,
                                      const char *type);

#define object_initialize_child(parent, propname, child, type) \

object_initialize_child_internal((parent), (propname), \
								 (child), sizeof(*(child)), (type))
``` 

## Definition

The function **`object_initialize_child`** is actually defined in the [header](object.h.md), and exists to call the function **`object_initialize_child_internal`**. 
Why this is done is currently a mystery to me.

The function that is actually called **`object_initialize_child_internal`** is defined in the [main file](object.c.md)



