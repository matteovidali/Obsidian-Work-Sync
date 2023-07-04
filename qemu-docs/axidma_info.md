# [[TypeInfo]] axidma_info

axidma_info is the [[TypeInfo]] structure of the [[xilinx_axidma]] module


## Definition

```c
static const TypeInfo axidma_info = {
    .name          = TYPE_XILINX_AXI_DMA,
    .parent        = TYPE_SYS_BUS_DEVICE,
    .instance_size = sizeof(XilinxAXIDMA),
    .class_init    = axidma_class_init,
    .instance_init = xilinx_axidma_init,
};```

