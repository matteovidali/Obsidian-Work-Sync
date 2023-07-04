# The QEMU file xilinx axidma

## Init functions

The **main** [[axidma_init function]] seems to be:

```c
	static void xilinx_axidma_init(Object *obj)
```


There seem to be a few others:

```c
	static void axidma_class_init(ObjectClass *klass, void *data)

	static void xilinx_axidma_stream_class_init(ObjectClass *klass, void *data)
```

But these seem to be more internal.

## [[TypeInfo]] [[axidma_info]]

The [axidma_info](qemu-docs/axidma_info) [[TypeInfo]] struct seems to be the main info struct for the [[QOM]] object.