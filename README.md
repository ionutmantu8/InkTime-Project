## Block Diagram
The system is built around the nRF52840 microcontroller, powered from a LiPo battery with USB-C charging, and integrates an e-paper display, IMU, fuel gauge, haptic driver, and three user buttons.

> If the block diagram does not render in your local Markdown viewer, open this file in VS Code and install the **Markdown Preview Mermaid Support** extension.

```mermaid
flowchart LR
    subgraph Power
        USB[USB-C]
        CHG[BQ25180<br/>LiPo Charger]
        BAT[LiPo Battery]
        DCDC[RT6160<br/>DC/DC]
        FG[MAX17048<br/>Fuel Gauge]
    end

    subgraph Core
        MCU[nRF52840]
        ANT[2450AT18B100E<br/>Antenna]
        SWD[SWD Header]
        IMU[BMA421 IMU]
    end

    subgraph Display
        EPDDRV[E-Paper Drive Circuit]
        EPDCONN[503480-2400<br/>E-Paper Connector]
        EPD[E-Paper Display]
    end

    subgraph UI
        BTN[3 Buttons]
        HAPTICDRV[DRV2605<br/>Haptic Driver]
        VIB[Shaker]
    end

    USB --> CHG --> BAT
    BAT --> DCDC
    BAT --> FG
    USB --> MCU
    DCDC --> MCU

    MCU --> ANT
    MCU --> SWD
    MCU <-->|I2C| IMU
    MCU <-->|I2C| FG

    MCU -->|SPI + EPD_CTRL| EPDDRV
    EPDDRV --> EPDCONN --> EPD

    MCU -->|GPIO| BTN
    MCU -->|I2C + HAPTIC_EN| HAPTICDRV
    HAPTICDRV --> VIB
```

## Bill of Materials (BOM)

|Component|JLC Part #|Package|Description|Datasheet|
| :--- | :--- | :--- | :--- | :--- |
|SJ1|N/A|Solder Jumper (Copper feature - leave open)|SMD solder JUMPER|N/A|
|R2, R3, R4|[C3920633](https://www.lcsc.com/product-detail/C3920633.html)|0201|7.68k 0201 Thin Film Surface Mount Fixed Resistor +/-0.5% 0.031W CPF0201D7K68C1|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2404081048_TE-Connectivity-CPF0201B511RE1_C3920633.pdf)|
|C23, C27, C34, C42|[C21012218](https://www.lcsc.com/product-detail/C21012218.html)|-|Check availability|[datasheet](https://jlc-prod-smt.oss-eu-central-1.aliyuncs.com/smtDataManualFile/8603520985945550848-C21012218.pdf?response-content-disposition=attachment%3B%20filename%3DC21012218.pdf%3B%20filename%2A%3DUTF-8%27%27C21012218.pdf&x-oss-date=20260407T205848Z&x-oss-expires=1800&x-oss-security-token=CAISgAN1q6Ft5B2yfSjIr5r9Dd2HhJt1xpCRZnzhgHQ0Psp9nrTKiTz2IHhMdHJsAOodtv0%2FmmhT6PkclqRLcbhpcmfjV%2BZHzLB8qYoRtS1%2F4J7b16cNrbH4M4H6aXeirtuwDsz9SNTCALjPD3nPii50x5bjaDymRCbLGJaViJlhHLN1Ow6jdmhpCctxLAlvo9NgFxm3D%2Fu2NQPwiWf9FVdhvhEG6Vly8qOi2MaRmFy8yFTx0b0SvJ%2BjYMrmPctoN9JnSdC5mfdzau3a1TJ84gRD0a5wkaVA1zbDs5bfISEIuUzebreLqY03dV4mOvdqIcMe8qigz88fk%2FfIioH6xyxKOexoSCnFTOiiupCcQLPyao9jLu6iayqViY7QaIOTqQohZmkAMwVOasAsI3Ngh4zF97Qt0cVNkXO9gWfLI8DtuMleWoolCMBMoNHD0eS19jklBdzSlusJRAJJUVBflCeKEaRNSAd3WGhEfM2%2BBt4QT30w5N2u00S8OSMIfXAg5qKWD5sagAGsz%2FZUSUxHdlmxVPmipF%2FTcZWs78wlHh10XRuVSQAFwkjbrT5Hik%2Bt6g43igoBe9p%2FcAdU6beRw%2F0OV8Ul08RsL45K5wTZwHFsbzFcwqB3eAqV0VruhFdx%2FHGC73AK3f%2BS3e6Qh1%2F1xoEQbmetO%2F%2BEcdyt%2BmwKiZrCa5Agq9keuCAA&x-oss-signature-version=OSS4-HMAC-SHA256&x-oss-credential=STS.NYHFg3iDTqRzdZPdta2EQqqak%2F20260407%2Feu-central-1%2Foss%2Faliyun_v4_request&x-oss-signature=20ddec61a1b664f2d44439cb342bbd932d8702b658f2816e0dfb7eb77563430a)|
|EPD_C5|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|R1_EP_DR|[C3920633](https://www.lcsc.com/product-detail/C3920633.html)|0201|7.68k 0201 Thin Film Surface Mount Fixed Resistor +/-0.5% 0.031W CPF0201D7K68C1|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2404081048_TE-Connectivity-CPF0201B511RE1_C3920633.pdf)|
|C15|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|C5, C7, C8, C12, C19|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|C11|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|R2_EP_DR, R9, R_PWR_EPD|[C3920633](https://www.lcsc.com/product-detail/C3920633.html)|0201|7.68k 0201 Thin Film Surface Mount Fixed Resistor +/-0.5% 0.031W CPF0201D7K68C1|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2404081048_TE-Connectivity-CPF0201B511RE1_C3920633.pdf)|
|R5, R7, R8|[C3920633](https://www.lcsc.com/product-detail/C3920633.html)|0201|7.68k 0201 Thin Film Surface Mount Fixed Resistor +/-0.5% 0.031W CPF0201D7K68C1|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2404081048_TE-Connectivity-CPF0201B511RE1_C3920633.pdf)|
|C1-EP-DR|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|C24, C39|[C9900179830](https://www.lcsc.com/product-detail/C9900179830.html)|402|0402 (1005 Metric)|N/A|
|L2|[C12669](https://www.lcsc.com/product-detail/C12669.html)|402|Generic chip inductor|[datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_2304140030_Murata-Electronics-LQG15HS27NJ02D_C12669.pdf)|
|C1, C2, C17, C18|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|L3|[C12669](https://www.lcsc.com/product-detail/C12669.html)|402|Generic chip inductor|[datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_2304140030_Murata-Electronics-LQG15HS27NJ02D_C12669.pdf)|
|C3, C4|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|C29, C30, C31, C32, C37, C38|[C3920633](https://www.lcsc.com/product-detail/C3920633.html)|0201|7.68k 0201 Thin Film Surface Mount Fixed Resistor +/-0.5% 0.031W CPF0201D7K68C1|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2404081048_TE-Connectivity-CPF0201B511RE1_C3920633.pdf)|
|EPD_C1, EPD_C2, EPD_C6, EPD_C7, EPD_C8, EPD_C9, EPD_C10, EPD_C11, EPD_C12|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|R_TYPE_SEL|[C3920633](https://www.lcsc.com/product-detail/C3920633.html)|0201|7.68k 0201 Thin Film Surface Mount Fixed Resistor +/-0.5% 0.031W CPF0201D7K68C1|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2404081048_TE-Connectivity-CPF0201B511RE1_C3920633.pdf)|
|SW_DN, SW_ENT, SW_UP|[C569760](https://www.lcsc.com/product-detail/C569760.html)|SMD,3.9x2.9mm|-40℃~+85℃ 1.6N 1.6mm 15V 2.9mm 20mA 3.9mm 500,000 Cycles IP67 J-Lead Rectangular Button SPST Surface Mount,Vertical White With Bracket SMD,3.9x2.9mm Tactile Switches ROHS|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2301111010_PANASONIC-EVPAKE31A_C569760.pdf)|
|Q1|[C2564](https://www.lcsc.com/product-detail/C2564.html)|TO-220AB|-55℃~+175℃ 1 P-Channel 180nC@10V 200W 20mΩ@10V 3.4nF 4V 55V 640pF 74A P-Channel TO-220AB MOSFETs ROHS|[datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_1809041724_Infineon-Technologies-IRF4905PBF_C2564.pdf)|
|C25, C33|[C9900179830](https://www.lcsc.com/product-detail/C9900179830.html)|402|0402 (1005 Metric)|N/A|
|ANT1|[C2917717](https://www.lcsc.com/product-detail/C2917717.html)|1206|-45℃~+125℃ 0.5dBi 1.3mm 1.6mm 100MHz 2.45GHz 2W 3.2mm 50Ω Patch Antenna 1206 Antennas ROHS|[datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_2404021210_Johanson-Dielectrics-2450AT18B100E_C2917717.pdf)|
|L1|[C12669](https://www.lcsc.com/product-detail/C12669.html)|402|Generic chip inductor|[datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_2304140030_Murata-Electronics-LQG15HS27NJ02D_C12669.pdf)|
|X2|[C32346](https://www.lcsc.com/product-detail/C32346.html)|SMD3215-2P|-40℃~+85℃ 12.5pF 32.768kHz 70kΩ Crystal Oscillator ±20ppm SMD3215-2P Crystals ROHS|[datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_2404180925_Seiko-Epson-Q13FC13500004_C32346.pdf)|
|X1|[C9009](https://www.lcsc.com/product-detail/C9009.html)|SMD3225-4P|-40℃~+85℃ 12pF 32MHz Crystal Oscillator ±10ppm ±20ppm SMD3225-4P Crystals ROHS|[datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_2403291504_YXC-Crystal-Oscillators-X322532MOB4SI_C9009.pdf)|
|R17, R18|[C3920633](https://www.lcsc.com/product-detail/C3920633.html)|0201|7.68k 0201 Thin Film Surface Mount Fixed Resistor +/-0.5% 0.031W CPF0201D7K68C1|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2404081048_TE-Connectivity-CPF0201B511RE1_C3920633.pdf)|
|C43|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|C2-EP-DR|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|C6, C14, C20, C21|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|C16|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|J1|[C122434](https://www.lcsc.com/product-detail/C122434.html)|SMD,P=0.5mm,Surface Mount，Right Angle|FFC & FPC Connectors 0.5mm FPC RA SMT Dual Contact 24Ckt|[datasheet](https://www.molex.com/content/dam/molex/molex-dot-com/products/automated/en-us/salesdrawingpdf/503/503480/5034802400_sd.pdf)|
|R1_USB, R2_USB|[C3920633](https://www.lcsc.com/product-detail/C3920633.html)|0201|7.68k 0201 Thin Film Surface Mount Fixed Resistor +/-0.5% 0.031W CPF0201D7K68C1|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2404081048_TE-Connectivity-CPF0201B511RE1_C3920633.pdf)|
|L5|[C1329646](https://www.lcsc.com/product-detail/C1329646.html)|SMD,4.8x4.8mm|1.6A 1.6A 4.7uH 41.4mΩ AEC-Q200 ±30% SMD,4.8x4.8mm Power Inductors ROHS|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2304140030_BOURNS-SRR4828A-4R7Y_C1329646.pdf)|
|C9|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|IC3|[C189517](https://www.lcsc.com/product-detail/C189517.html)|LGA-12(2x2)|Accelerometers Triaxial low-g 12bit Acceleration Sensor|[datasheet](https://www.lcsc.com/datasheet/C189517.pdf)|
|IC1|[C3682423](https://www.lcsc.com/product-detail/C3682423.html)|DSBGA-8(1.1x1.6)|Charger IC Lithium Ion/Polymer, Lithium Iron Phosphate 8-DSBGA (1.6x1.1)|[datasheet](https://www.ti.com/cn/lit/ds/symlink/bq25180.pdf?ts=1775594237116)|
|IC2|[C81079](https://www.lcsc.com/product-detail/C81079.html)|DSBGA-9|Haptic Driver for ERM/LRA with Built-In Library and Smart Loop Architecture|[datasheet](https://www.ti.com/cn/lit/gpn/drv2605)|
|L7|[C5832368](https://www.lcsc.com/product-detail/C5832368.html)|1008|13mΩ 470nH 6.5A 7.5A ±20% 1008 Power Inductors ROHS|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2306021632_cjiang--Changjiang-Microelectronics-Tech-FTC252012SR47MBCA_C5832368.pdf)|
|TP (Test Pads)|N/A|N/A|Test pad|N/A|
|J4|[C709357](https://www.lcsc.com/product-detail/C709357.html)|SMD|-40℃~+85℃ 1 10,000 cycles 16P 30V 3A 7.81mm Female Surface Mount, Right Angle Type-C SMD USB Connectors ROHS|[datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_2404191039_Shenzhen-Kinghelm-Elec-KH-TYPE-C-16P_C709357.pdf)|
|U2|[C2682616](https://www.lcsc.com/product-detail/C2682616.html)|DFN-8-EP(2x2)|-40℃~+85℃ 1 2.5V~4.5V 3uA I2C Lithium Battery DFN-8-EP(2x2) Battery Management ROHS|[datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_2410121738_Analog-Devices-Inc--Maxim-Integrated-MAX17048G-T10_C2682616.pdf)|
|D2, D4, D5|[C82046](https://www.lcsc.com/product-detail/C82046.html)|SOD-123|ON SEMICONDUCTOR - MBR0530 - DIODE, SCHOTTKY, 0.5A, 30V, SOD-123|[datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_2304140030_onsemi-MBR0530T1G_C82046.pdf)|
|C10, C13, C22|[C9900156064](https://www.lcsc.com/product-detail/C9900156064.html)|201|Generic chip capacitor|[datasheet](https://ds.yuden.co.jp/TYCOMPAS/or/download?pn=MLAST063SCG681JFNA01&fileType=CA)|
|U1|[C3606653](https://www.lcsc.com/product-detail/C3606653.html)|QFN-48(6x6)|nRF52840|[datasheet](https://www.lcsc.com/datasheet/C3606653.pdf)|
|IC9|[C7065276](https://www.lcsc.com/product-detail/C7065276.html)|WLCSP-15B(2.3x1.4)|Buck-Boost Regulator Positive Output Step-Up/Step-Down I2C DC-DC Controller IC 15-WL-CSP (BSC) (1.4x2.3)|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2312271436_Richtek-Tech-RT6160AWSC_C7065276.pdf)|
|Q3|[C469327](https://www.lcsc.com/product-detail/C469327.html)|SOT-323|MOSFET N-Ch 30V 1.5A TrenchFET SC70 Vishay Si1308EDL-T1-GE3 N-channel MOSFET Transistor, 1.5 A, 30 V, 3-Pin SC-70|[datasheet](https://www.lcsc.com/datasheet/lcsc_datasheet_1912202016_Vishay-Intertech-SI1308EDL-T1-GE3_C469327.pdf)|
|J2|[C90533](https://www.lcsc.com/product-detail/C90533.html)|P=1mm|CABLE ADAPTER 6 POS|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/1810141506_LX-FFC6P1-0mm7CM_C90533.pdf)|
|D3|[C2969755](https://www.lcsc.com/product-detail/C2969755.html)|SOT-23-6L|Low Cap. ESD Protection Auto SOT-23-6 STMicroelectronics USBLC6-2SC6Y, Dual Uni-Directional TVS Diode Array, 6-Pin SOT-23|[datasheet](https://wmsc.lcsc.com/wmsc/upload/file/pdf/v2/lcsc/2211080730_STMicroelectronics-USBLC6-2SC6Y_C2969755.pdf)|


## Hardware Functionality

The hardware platform is built around the **Nordic nRF52840** microcontroller, which acts as the central processing and communication unit of the device. The system integrates power-management circuitry, an inertial sensor, an e-paper display subsystem, a haptic feedback subsystem, USB connectivity, user input buttons, and a debug interface.

### Main hardware blocks

#### 1. Microcontroller – nRF52840
The nRF52840 is the core of the system. It was chosen because it provides:
- low-power operation, suitable for battery-powered wearable devices;
- a rich set of GPIOs;
- integrated USB device support;
- SPI and I2C peripherals for connecting multiple external components;
- SWD debug support;
- BLE capability for future wireless extensions.

The MCU is clocked using:
- a **32 MHz crystal** for the main system clock;
- a **32.768 kHz crystal** for accurate low-power timing.

It also includes the RF matching network and a **2.4 GHz chip antenna** for wireless communication.

---

#### 2. Power subsystem
The device is powered from a **single-cell LiPo battery** and charged through a **USB-C connector**.

The power architecture contains:
- **USB-C connector** for power input and USB communication;
- **USBLC6-2SC6Y** ESD protection for the USB data lines;
- **BQ25180YBGR** battery charger / PMIC for LiPo charging and battery power management;
- **RT6160AWSC** DC/DC converter for generating the regulated supply rail;
- decoupling capacitors and inductors used for filtering and rail stabilization;
- **MAX17048G+T10** fuel gauge for battery level monitoring.

This architecture allows the system to be powered both from USB and from battery, while also monitoring the battery state and keeping energy consumption low.

---

#### 3. E-paper display subsystem
The user interface is based on a **1.54-inch e-paper display**, connected through an FPC connector (**503480-2400**).  
Because the display requires dedicated bias voltages, the design also includes an **e-paper drive circuit** built from passive components, Schottky diodes, MOSFETs and filtering capacitors.

The display subsystem uses:
- **SPI signals** for communication with the MCU;
- several digital control signals:
  - `EPD_CS`
  - `EPD_DC`
  - `EPD_RST`
  - `EPD_BUSY`

The e-paper display was chosen because it has very low static power consumption and is well suited for wearable devices that only update the screen occasionally.

---

#### 4. IMU subsystem
Motion sensing is implemented using the **BMA421 IMU**.  
This sensor communicates with the MCU over **I2C** and provides interrupt outputs:
- `IMU_INT1`
- `IMU_INT2`

The IMU is used for motion/activity awareness and can wake or notify the MCU when specific events occur.

---

#### 5. Battery monitoring
Battery state measurement is handled by the **MAX17048 fuel gauge**, which communicates over **I2C** and provides an `ALERT` line to the MCU.  
This allows the firmware to estimate battery percentage and detect low-battery conditions.

---

#### 6. Haptic feedback
The haptic subsystem is built around the **DRV2605YZFR** haptic driver, connected to the MCU over **I2C**.  
The driver controls the vibration motor (shaker) through:
- `SDA`
- `SCL`
- `HAPTIC_EN`

This solution was chosen because it offloads waveform generation from the microcontroller and allows reliable control of the vibration motor.

---

#### 7. User buttons
The device includes **three buttons**:
- `SW_UP`
- `SW_ENT`
- `SW_DN`

These are connected to dedicated GPIO inputs of the MCU and provide the basic user interface for navigation and interaction.

---

#### 8. Debug and programming
For firmware upload and debugging, the board includes a **TC2030-IDC SWD header**.  
The exposed debug signals are:
- `SWDIO`
- `SWDCLK`
- `SWO`
- `RESET`
- `3.3V`
- `GND`

This header is used during development and testing.

---

## Interfaces used in the design

The design uses the following communication interfaces:

- **USB**
  - used for wired connectivity and battery charging path integration;
  - signals used: `VBUS`, `D+`, `D-`.

- **I2C**
  - shared bus used for:
    - BMA421 IMU
    - MAX17048 fuel gauge
    - DRV2605 haptic driver

- **SPI**
  - used for communication with the e-paper display subsystem

- **GPIO**
  - used for:
    - buttons
    - display control lines
    - interrupt lines
    - enable lines

- **SWD**
  - used for debugging and programming the nRF52840

---

## nRF52840 Pin Usage

Based on the schematic, the following nRF52840 pins are used for the main peripherals:

| nRF52840 Pin | Signal | Connected block | Purpose |
|---|---|---|---|
| P0.13 | D+ | USB-C | USB data line |
| P0.14 | D- | USB-C | USB data line |
| VBUS pin | VBUS | USB-C / charger | USB voltage detection |
| P0.06 | SDA | I2C bus | Shared data line for IMU, fuel gauge and haptic driver |
| P0.07 | SCL | I2C bus | Shared clock line for IMU, fuel gauge and haptic driver |
| P1.13 | MOSI | E-paper display | SPI data output to display |
| P1.15 | SCK | E-paper display | SPI clock |
| P0.17 | EPD_RST | E-paper display | Display reset |
| P0.15 | EPD_BUSY | E-paper display | Busy/status feedback from display |
| P0.16 | EPD_DC | E-paper display | Data/command select |
| P0.05 | EPD_CS | E-paper display | SPI chip select |
| P0.08 | IMU_INT1 | IMU | Interrupt from IMU |
| P1.08 | IMU_INT2 | IMU | Second interrupt from IMU |
| P0.11 | PMIC_INT | BQ25180 | Power-management interrupt |
| P0.24 | ALERT | MAX17048 | Battery fuel gauge alert |
| P0.12 | HAPTIC_EN | DRV2605 | Haptic driver enable/control |
| SWDIO | SWDIO | SWD header | Debug/programming |
| SWDCLK | SWDCLK | SWD header | Debug/programming |
| P0.18 / RESET | RESET | SWD header | Hardware reset |
| P1.09 | SWO | SWD header | Debug trace output |
| ANT pin | RF | Antenna matching network | 2.4 GHz RF connection |

### Why these pins were chosen
The chosen pin mapping follows the functional requirements of the peripherals:
- the dedicated **USB pins** of the nRF52840 are used for `D+` and `D-`;
- the e-paper display uses a mix of **SPI pins** and additional GPIO control lines;
- the sensors and monitoring ICs share the same **I2C bus**, which reduces routing complexity and saves GPIOs;
- interrupt-capable GPIOs are used for `IMU_INT1`, `IMU_INT2`, `PMIC_INT`, and `ALERT`;
- `SWDIO`, `SWDCLK`, `RESET`, and `SWO` are routed to the debug header for easier firmware development.

---

## Power Consumption Considerations

The board was designed with low-power operation in mind.

### Main power-saving choices
- **nRF52840** is a low-power MCU suitable for battery-powered applications.
- **E-paper display** only consumes significant power during refresh; static content does not require continuous display power like an LCD.
- **BQ25180** integrates battery charging and PMIC functionality, reducing the need for discrete always-on circuitry.
- **MAX17048** provides battery estimation with low overhead.
- **I2C shared bus** reduces pin count and simplifies routing.
- The **haptic motor** is driven only when needed.
- Buttons are connected as low-duty-cycle GPIO inputs.
- The external DC/DC stage improves efficiency compared to a purely linear power architecture.

### Qualitative consumption profile
The highest instantaneous consumption occurs during:
- battery charging over USB;
- e-paper refresh;
- vibration motor activation;
- radio activity of the nRF52840.

The lowest consumption mode is expected when:
- the display content is static,
- the MCU is in sleep mode,
- haptic feedback is off,
- no USB power is connected,
- only low-power monitoring remains active.

This makes the platform appropriate for a compact wearable device where energy efficiency matters more than continuous high-performance operation.


## ERC / Schematic Checks

During schematic verification, the project generated a small number of ERC warnings and one input-pin related message. These were reviewed manually before approval.

### Input-pin / single-pin style warnings
Warnings such as:
- `Only INPUT pins on net ...`
- `Only one pin on net ...`

were approved because, according to the project requirements and the lab guidance, these checks can be accepted when the designer has manually verified that the connections are intentional and that no required schematic connections are missing. In this design, the affected nets were inspected and no missing functional connection was identified.

### Power-pin warnings
Warnings of the form:
- `POWER pin ... connected to ...`

were also reviewed manually. After checking the schematic against the intended design, no missing supply rails or broken power connections were found. In addition, this was discussed during the lab, and we were told that such warnings can be approved when the power connectivity is intentional and complete.


## ERC Notes
Some ERC warnings were approved after manual verification. The `Only INPUT pins on net ...` and similar messages were accepted because the affected nets were intentionally connected and no functional connection was missing from the schematic. The power-pin warnings were also checked manually and approved after confirming that the supply rails and power connections were correct.

## DRC Notes
Some DRC warnings were approved in the PCB layout, mainly overlap-related messages such as SMD-via or pad-via overlaps. These were caused intentionally by using via-in-pad / escape routing in dense areas of the board, where this approach was necessary to complete routing in the available space.