[![MCHP](images/microchip.png)](https://www.microchip.com)

# PIC18F16Q41 Analog-Serial-Read and LED Toggle

In this step-by-step example, an LED will be toggled every 500ms and an analog value will be sent over serial UART and read using MPLAB® Data Visualizer. Both demos were featured in the Future Electronics webinar titled “Reduce Your Time to Market with the MPLAB® Ecosystem of Software and Hardware Tools” from November 3rd, 2021 which can be found [here](https://www.futureelectronics.com/resources/events/microchip-8-bit-technology-webinar-series).

## Related Documentation

- PIC18F16Q41 Product Page found [here.](https://www.microchip.com/en-us/product/PIC18F16Q41)

- PIC18F06/16Q41 DataSheet found [here.](https://ww1.microchip.com/downloads/en/DeviceDoc/PIC18F06-16Q41-DataSheet-40002214C.pdf)

- PIC18F16Q41 Hardware User Guide found [here.](https://ww1.microchip.com/downloads/en/DeviceDoc/PIC18F16Q41-Curiosity-Nano-Hardware-User-Guide-DS50003048A.pdf)

- Webinar video link from Future Electronics can be found [here.](https://www.futureelectronics.com/resources/events/microchip-8-bit-technology-webinar-series)


## Software Used

- MPLAB X IDE 5.50.0 or newer [(MPLAB® X IDE 5.50)](https://www.microchip.com/en-us/development-tools-tools-and-software/mplab-x-ide?utm_source=GitHub&utm_medium=TextLink&utm_campaign=MCU8_MMTCha_MPAE_Examples&utm_content=pic18f16q41-analog-serial-read-mplab-mcc-github)
- MPLAB XC8 2.32.0 or newer compiler [(MPLAB® XC8 2.32)](https://www.microchip.com/en-us/development-tools-tools-and-software/mplab-xc-compilers?utm_source=GitHub&utm_medium=TextLink&utm_campaign=MCU8_MMTCha_MPAE_Examples&utm_content=pic18f16q41-analog-serial-read-mplab-mcc-github)
- MPLAB Code Configurator 5.0.3 or newer [(MPLAB® MCC 5.0.3)](https://www.microchip.com/en-us/development-tools-tools-and-software/embedded-software-center/mplab-code-configurator?utm_source=GitHub&utm_medium=TextLink&utm_campaign=MCU8_MMTCha_pic18q41&utm_content=pic18f16q41-analog-serial-read-mplab-mcc-github)
- MPLAB Data Visualizer 1.3.1113 or newer [(MPLAB® Data Visualizer 1.3.1113)](https://www.microchip.com/en-us/development-tools-tools-and-software/embedded-software-center/mplab-data-visualizer?utm_source=GitHub&utm_medium=TextLink&utm_campaign=MCU8_MMTCha_pic18q41&utm_content=pic18f16q41-analog-serial-read-mplab-mcc-github)

## Hardware Used

- PIC18F16Q41 Curiosity Nano Development Board ([EV26Q64A](https://www.microchip.com/en-us/development-tool/EV26Q64A?utm_source=GitHub&utm_medium=TextLink&utm_campaign=MCU8_MMTCha_pic18q41&utm_content=pic18f16q41-adcc-temp-sensor-mplab-mcc-github))
- Breadboard
- Wire
- 1x, 10k Ohm potentiometer

## Setup #1: LED Toggle
#### Step #1: Creating the Project
- On the tool bar, click on New Project
- Microchip Embedded; Standalone Project
- Enter the Device
  - For this Project: pic18f16q41
- Enter a name for this project, such as “LED_Toggle”
  - Name: “LED_Toggle”
   - Note: The project name cannot have any empty spaces

#### Step #2: MPLAB Code Configurator (MCC)
- Open MPLAB Code Configurator by clicking the blue “MCC” shield in the top toolbar

![MCC Button on Toolbar](images/MCC_Button_on_Toolbar.png)

- When MCC opens, select “MCC Melody” and click “Finish”

![Opening MCC Melody](images/Opening_Melody.png)

- Modify the Clock Control under “Project Resources” in the top left panel
  - Set “Clock Source” to High Frequency Internal Oscillator (HFINTOSC)
  - Set “HF Internal Clock” to 4_MHz
  - Set “Clock Divider” to 4

![LED Blink - Clock Control](images/LEDBlinkClockControl.png)


- Set Configuration Bits under “Project Resources” in the top left panel
  - Set “External Oscillator Mode Selection” to “Oscillator not enabled”
  - Set "Power-up Default Value for COSC" to "HFINTOSC with HFFRQ= 4MHz and CDIV= 4:1"

![LED Blink - Config Bits](images/LEDBlinkConfigBits.png)

#### Step #3: Configure the Pins

- LED0 is connected to pin RC1
  - Set this pin as a digital GPIO output

![LED Pin Grid](images/LEDPinGridView.png)

- Assign pin RC1 a custom name of “LED” and ensure all other configurations are as seen below

![LED Pin Settings](images/LEDPinSettings.png)

#### Step #4: Generate the project
- Click the generate button in MCC to create the appropriate header and source files for this configuration
- Close MCC by clicking the blue “MCC” shield again

#### Step #5: Modifying main.c
- Upon the generation being completed, the new MCC generated header and source files will be in the project window. Select the main.c file found within your source files and you will see an empty while(1) loop where you can add your application code
- Follow this path under “Projects”
  - LED_Toggle -> Header Files -> MCC Generated Files -> System -> pins.h
- Open “pins.h” and scroll down to find the defined function “LED_Toggle()”
  - Copy and paste this function into your main.c, ``while(1)`` loop
  - Below this function, add a delay of 500 milliseconds using ``__delay_ms(500);``

```
int main(void)
{
    SYSTEM_Initialize();

    while(1)
    {
        LED_Toggle();
        __delay_ms(500);
    }    
}
```

- Make and program the device to observe the toggling LED

![Make and Program](images/MakeAndProgram.png)

- Result:

![LED Blinking Demo GIF](images/LEDBlinkDemoGif.gif)


## Setup #2: Analog-Serial-Read
#### Step #1: Setting Up the Breadboard
- Follow the image below to set up your bread Breadboard
  - VTG from Curiosity Nano to positive rail
  - GND from Curiosity Nano to negative rail
  - Jumper wire from positive rail to Vcc pin on potentiometer
  - Jumper wire from negative rail to GND pin on potentiometer
  - Jumper wire from pin RA5 on Curiosity Nano to Output pin on potentiometer

![Wiring Schematic](images/ASR_Schematic.png)

![Breadboard Setup](images/BreadboardSetup.jpg)

#### Step #2: Creating the Project

- On the tool bar, click on New Project
- Microchip Embedded; Standalone Project
- Enter the Device
  - For this Project: PIC18F16Q41
- Enter a name for this project, such as “Analog_Serial_Read”
  - Name: “Analog_Serial_Read”
    - Note: The project name cannot have any empty spaces

#### Step #3: MPLAB Code Configurator (MCC)
- Open MPLAB Code Configurator by clicking the blue “MCC” shield in the top toolbar

![MCC Button on Toolbar](images/MCC_Button_on_Toolbar.png)

- When MCC opens, select “MCC Melody” and click “Finish” on the next page

![Opening MCC Melody](images/ASROpeningMelody.png)

- Modify the Clock Control under “Project Resources” in the top left panel
  - Set “Clock Source” to High Frequency Internal Oscillator (HFINTOSC)
  - Set “HF Internal Clock” to 4_MHz
  - Set “Clock Divider” to 1

![ASR - Clock Control](images/ASRClockControl.png)

- Set Configuration Bits under “Project Resources” in the top left panel
  - Set “External Oscillator Mode Selection” to “Oscillator not enabled”
  - Set "Power-up Default Value for COSC" to "HFINTOSC with HFFRQ= 4MHz and CDIV= 4:1"

![ASR - Config Bits](images/LEDBlinkConfigBits.png)

#### Step #4: Add ADCC and UART Peripherals
- In Device Resources:
  - Drivers → ADCC → ADCC
  - Drivers → UART → UART1

![Analog Serial Read Peripherals](images/ASRPeripheralsADCCUART.png)

###### Once the peripherals are added, modify the peripherals:
- ADCC
  - Hardware Settings to change while the rest can be left as default
    - Operating Mode = Basic Mode
    - Result Alignment = right
    - Positive Reference = VDD
    - Negative Reference = VSS
    - Acquisition Count = 2

![ADC Settings](images/ADCSettings.png)

- UART1
  - Software Settings:
    - Enable "Redirect STDIO to UART"
  - Hardware Settings:
    - Enable UART box should be checked
    - Set the Baud Rate to 19200
    - Enable Transmit and Receive should be checked
    - Everything else can be left as default settings

![UART Settings](images/UARTSettings.png)

#### Step #5: Configure the Pins
- UART1 TX1 is connected to pin RB7
- UART1 RX1 is connected to pin RB5
- ADCC “ANx” is connected to pin RA5
  - This is the pin on the Curiosity Nano that the potentiometer is connected to

![Analog-Serial-Read Pin Grid Settings](images/ASRPinGridSettings.png)

- Assign pin RA5 a custom name of “POT” and ensure all other configurations are as seen below

![Analog-Serial-Read Pin Configuration](images/ASRPinConfiguration2.png)

#### Step #6: Generate the Project
- Click the generate button in MCC to create the appropriate header and source files for this configuration

#### Step #7: Modifying main.c
- Upon the generation being completed, the new MCC generated header and source files will be in the project window. Select the main.c file found within your source files and you will see an empty ``while(1)`` loop where you can add your application code
- Follow this path under “Projects”
  - Analog_Serial_Read → Header Files → MCC Generated Files → ADC → adcc.h
- Open “adcc.h” and scroll down to find the defined function ``adc_result_t ADCC_GetSingleConversion(adcc_channel_t channel);``
  - Copy and paste this function into your main.c, ``while(1)`` loop.
- Pass your custom pin name through the function within a ``printf()`` statement to read the current value from the potentiometer

  ```
  int main(void)
  {
      SYSTEM_Initialize();

      while(1)
      {
          printf("%d\r\n",ADCC_GetSingleConversion(POT));
      }    
  }
  ```

- Make and program the device

![Make and Program](images/MakeAndProgram.png)

#### Step #8: Terminal Emulator
- For this project, the terminal program that is being used is MPLAB Data Visualizer
  - Open Data Visualizer by clicking the green “DV” shield in the top toolbar

![Opening MPLAB Data Visualizer](images/MPLABDataVisualizer.png)

- Click on your Curiosity Nano COM port and set:
  - Baud Rate to: 19200
  - Char Length: 8 bits
  - Parity: None
  - Stop Bits: 1 bit
- Click “Apply” to save these settings

![COM Port Settings](images/COMPortSettings.png)

- On the right side of the terminal window, ensure “Display As: 8-bit ASCII” is selected

![Terminal Settings](images/TerminalSettings.png)

- Press the drop-down carrot next to your Curiosity Nano’s COM port and select “Send to Terminal”

![Sending Data to Serial Terminal](images/DataToSerialTerminal.png)

- While twisting the potentiometer’s dial, you will be able to see the changing values within the Data Visualizer terminal window

![Serial Terminal Output](images/SerialTerminalOutput.png)



## Summary
This application demonstrates how to toggle a LED on and off at a rate of 500 milliseconds as well as how to configure an analog-serial-read using a potentiometer.
