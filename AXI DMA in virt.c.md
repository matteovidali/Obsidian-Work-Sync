# The goal
Is to put the xilinx_axi_dma.c dma into the virt machine. When doing this naiively by just adding it to the device call, yeilds the error:

`qemu-system-arm: -device xlnx.axi-dma: Parameter 'driver' expects a pluggable device type` 

Which refers to the fact that the virt machine does not know how to *plug* this device into itself.

# Requirements

The things I believe that I need to implement to get this to work are:

- [ ] AXI_DMA header file with needed types and prototypes in it
- [ ] Adding the AXI_DMA to the Machine State of the machine (`virt.h`) 
- [ ] Adding a function in `virt.c` to instantiate and realize the DMA engine
