AWS MQTT Demo
=============

This demo application connects to [AWS IoT](https://aws.amazon.com/iot/) 
through MQTT, subscribes to topics and publishes messages.

It requires an active and properly configured [thing](https://www2.keil.com/iot/aws).

You can use the AWS IoT MQTT client in the AWS IoT console to watch the 
MQTT messages sent and received by AWS IoT.

The following describes the various components and the configuration settings.

Once the application is configured you can:
 - Build the application
 - Connect the debugger
 - Run the application and view messages in a debug printf or terminal window


AWS IoT Client
--------------
The file `iot_config.h` configures the connection to AWS IoT with these settings:
 - IOT_DEMO_SERVER:      Remote Host
 - IOT_DEMO_ROOT_CA:     Trusted Server Root Certificate
 - IOT_DEMO_CLIENT_CERT: Client Certificate
 - IOT_DEMO_PRIVATE_KEY: Client Private Key
 - IOT_DEMO_IDENTIFIER:  Thing Identifier

Note: These settings need to be configured by the user!


FreeRTOS Real-Time Operating System
-----------------------------------
The [FreeRTOS RTOS](https://github.com/ARM-software/CMSIS-FreeRTOS) 
implements the resource management. It is configured with the following settings:

- Global Dynamic Memory size: 24000 bytes
- Default Thread Stack size: 3072 bytes


WiFi IoT Socket
---------------
This implementation uses an IoT socket layer that connects to a 
[CMSIS-Driver WiFi](https://arm-software.github.io/CMSIS_5/Driver/html/index.html).

The file `socket_startup.c` configures the WiFi connection with these settings:
 - SSID:          network identifier
 - PASSWORD:      network password
 - SECURITY_TYPE: network security

Note: These settings need to be configured by the user!


ESP8266 WiFi Module
-------------------
The [ESP8266 WiFi Module](https://www2.keil.com/iot/shields/wrl13287) 
is connected via an Arduino connector using a USART interface.
It exposes a **CMSIS-Driver WiFi**.


NXP MIMXRT1064-EVK Target Board
-------------------------------
The Board layer contains the following configured interface drivers:

**CMSIS-Driver ETH_MAC0** controlling the KSZ8081RNB ethernet PHY (U16):
 - MDC:       GPIO_EMC_40 (A7)
 - MDIO:      GPIO_EMC_41 (C7)
 - REF_CLK:   GPIO_B1_10  (B13)
 - RX_DATA,0: GPIO_B1_04  (E12)
 - RX_DATA,1: GPIO_B1_05  (D12)
 - RX_EN:     GPIO_B1_06  (C12)
 - RX_ER:     GPIO_B1_11  (C13)
 - TX_DATA,0: GPIO_B1_07  (B12)
 - TX_DATA,1: GPIO_B1_08  (A12)
 - TX_EN:     GPIO_B1_09  (A13)

**CMSIS-Driver ETH_PHY0** routed to RJ45 connector (J19)

**CMSIS-Driver MCI0** routed to memory card socket (J39):
 - DATA3: GPIO_SD_B0_05 (J2)
 - DATA2: GPIO_SD_B0_04 (H2)
 - DATA1: GPIO_SD_B0_03 (K1)
 - DATA0: GPIO_SD_B0_02 (J1)
 - CMD:   GPIO_SD_B0_00 (J4)
 - CLK:   GPIO_SD_B0_01 (J3)

**CMSIS-Driver USART1** routed to Virtual COM port (J41):
 - RX: GPIO_AD_B0_13 (L14)
 - TX: GPIO_AD_B0_12 (K14)

**CMSIS-Driver USART3** routed to Arduino UNO R3 connector (J22):
 - RX: D0/UART_RX - GPIO_AD_B1_07 (K10)
 - TX: D1/UART_TX - GPIO_AD_B1_06 (J12)

**CMSIS-Driver VIO** with the following board hardware mapping:
 - vioBUTTON0: USER_BUTTON (L6)
 - vioLED0:    USER_LED    (F14)

**STDIO** routed to Virtual COM port (DAP-Link, baudrate = 115200)

The board configuration can be modified using 
[MCUxpresso](https://www.keil.com/nxp) 
and is stored in the file `MIMXRT1064-EVK.mex`.
