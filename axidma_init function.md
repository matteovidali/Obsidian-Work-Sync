# xilinx [dma](Work/DMA)

The file `xilinx_axidma.c` should implement the xilinx DMA object, and allow me to just insert it into my code and run the QEMU machine with the hardware for DMA.  

We can only hope.

# The main init code of xilinx_axidma.c

### The Source Code
```c
static void xilinx_axidma_init(Object *obj)
{
    XilinxAXIDMA *s = XILINX_AXI_DMA(obj);
    SysBusDevice *sbd = SYS_BUS_DEVICE(obj);

    object_initialize_child(OBJECT(s), "axistream-connected-target",
                            &s->rx_data_dev, TYPE_XILINX_AXI_DMA_DATA_STREAM);
    object_initialize_child(OBJECT(s), "axistream-control-connected-target",
                            &s->rx_control_dev,
                            TYPE_XILINX_AXI_DMA_CONTROL_STREAM);
    object_property_add_link(obj, "dma", TYPE_MEMORY_REGION,
                             (Object **)&s->dma_mr,
                             qdev_prop_allow_set_link_before_realize,
                             OBJ_PROP_LINK_STRONG);

    sysbus_init_irq(sbd, &s->streams[0].irq);
    sysbus_init_irq(sbd, &s->streams[1].irq);

    memory_region_init_io(&s->iomem, obj, &axidma_ops, s,
                          "xlnx.axi-dma", R_MAX * 4 * 2);
    sysbus_init_mmio(sbd, &s->iomem);
}
```

## Breakdown

1. The init function begins by establishing a `XilinxAXIDMA *s` to the object passed. This is consistent with my own development as there must be a reference to the current *object* 

2. There is also a `SysBusDevice *sbd` pointer to the system bus. This is also grabbed from the object.

3. Then the function runs two [[object_initialize_child()]] which is an external function responsible for creating '*child*' devices - the controlled target and the controller target.
4. The `object_property_add_link` function adds the required properties to the object
5. The `sysbus_init_irq` function initializes the IRQ interrupts on the system bus
6. The `memory_region_init_io` function actually prepares the system memory regions required by the axi-dma.
7. Finally, the `sysbus_init_mmio` call actually inits the memory mapped io region into the emulation.