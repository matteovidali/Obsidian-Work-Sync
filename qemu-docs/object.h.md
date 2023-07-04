The header file for qemu [[qom]] object.

## object_initialize_child

```c
void object_initialize_child_internal(Object *parent, const char *propname,
                                      void *child, size_t size,
                                      const char *type);

#define object_initialize_child(parent, propname, child, type) \

object_initialize_child_internal((parent), (propname), \
								 (child), sizeof(*(child)), (type))
``` 