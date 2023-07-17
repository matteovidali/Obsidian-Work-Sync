Selected Micro Controller is: [**STM32F401RET6**](https://www.st.com/en/microcontrollers-microprocessors/stm32f401re.html?rt=db&id=DB2196) 
Purchase: [Digikey](https://www.digikey.com/en/products/detail/stmicroelectronics/STM32F401RET6/5051337) [Mouser](https://www.mouser.com/ProductDetail/STMicroelectronics/STM32F401RET6?qs=s5SkPsIz108eLldhTu8INg%3D%3D)

## Power System
### Decoupling Capacitors
This device will be powered with $3.3V$ power, and has 4 *VDD* Pins and 1 *VBAT* pin. For this, we will have 1 $100nF$ ceramic capacitor per power input pin, and 1 $10\mu F$ ceramic capacitor for the whole device decoupling

### NRST
The NRST pin will be tied to ground through a capacitor to avoid unexpected reset behavior

### BOOT0
Boot zero pin will be tied to a switch through a $10k\ohm$ resistor, so that the device can be quickly switched on or off

### VREF+
The VREF+ pin will be shorted to $3.3V$ Power, and then decoupled with a $10nF$ ceramic capacitor to ground to obtain a decently filtered signal for VREF

### VCAP1
The VCAP1 pin will be dealt with in line with section *6.3.2 VCAP1/VCAP2 external capacitors* of the [STM32F401 Datasheet](https://www.st.com/resource/en/datasheet/stm32f401re.pdf) 

