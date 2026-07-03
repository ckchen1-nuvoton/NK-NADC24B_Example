# Nuvoton M251 with NADC24B Weighting Sensor Application

This project demonstrates how to interface the **Nuvoton M251 microcontroller** with the **NADC24B (24-bit Delta-Sigma ADC)** external module via SPI. It implements a high-precision, low-latency analog data acquisition system designed for electronic scale applications (e.g., weighing scales, counting scales, and refrigerant scales).

## 🚀 Key Features
* **High-Precision 24-bit Conversion:** Handles 24-bit signed analog data using full-range voltage calculations (2.4V reference).
* **Interrupt-Driven Ready Detection:** Utilizes a GPIO falling-edge interrupt on `PA.4` linked to the NADC24B data-ready pin, ensuring zero CPU polling overhead.
* **Efficient SPI Link:** Configured in SPI Master Mode 0 running at a high-speed clock of **10 MHz** to instantly retrieve ADC sample packets.

---

## 📊 Hardware & Timing Architecture

The timing diagram and hardware setup below illustrate the low-overhead data fetching routine implemented in this repository:

<img width="1680" height="726" alt="image" src="https://github.com/user-attachments/assets/5fd964ae-7fc4-4f8f-9bf5-f5cb21b17b2f" />

### Pin Mapping

| MCU Pin | Peripheral | Connected To (NADC24B) | Signal Description |
| :--- | :--- | :--- | :--- |
| **PA.0** | SPI0_MOSI | SDI / MOSI | Serial Data Input |
| **PA.1** | SPI0_MISO | SDO / MISO | Serial Data Output |
| **PA.2** | SPI0_CLK  | SCLK | Serial Clock (10 MHz, Schmitt Trigger enabled) |
| **PA.3** | SPI0_SS   | /CS / SS | Chip Select Line |
| **PA.4** | GPIO Input| /DRDY | **Data Ready Pin** (Falling-edge triggers `GPA_IRQHandler`) |
| **PB.12/13**| UART0 | Debug Console | Serial Log output (`115200, 8-N-1`) |

---

## 🛠️ Data Processing Logic

The data flow is decoupled into an asynchronous hardware-to-software routine:

1. **Hardware Signaling:** The NADC24B completes an internal conversion cycle and pulls the `/DRDY` line (`PA.4`) **LOW**.
2. **Interrupt Handshake (`GPA_IRQHandler`):** The MCU catches the falling edge, clears the flag, and triggers an 8-bit SPI transaction to retrieve the raw 24-bit sample.
3. **Sign Extension & Conversion:** * Formats the 24-bit raw stream into a standard 32-bit signed integer (`int32_t`).
   * Calculates the true analog voltage using the ratio:

$$\text{Volt} = \frac{\text{s32AdcData}}{\text{FULL\_RANGE}} \times 2.4\text{V}$$



# Nuvoton M251 with NADC24B Weighting Sensor Application

This project demonstrates how to interface the **Nuvoton M251 microcontroller** with the **NADC24B (24-bit Delta-Sigma ADC)** external module via SPI. It implements a high-precision, low-latency analog data acquisition system designed for electronic scale applications (e.g., weighing scales, counting scales, and refrigerant scales).

## 🚀 Key Features
* **High-Precision 24-bit Conversion:** Handles 24-bit signed analog data using full-range voltage calculations (2.4V reference).
* **Interrupt-Driven Ready Detection:** Utilizes a GPIO falling-edge interrupt on `PA.4` linked to the NADC24B data-ready pin, ensuring zero CPU polling overhead.
* **Efficient SPI Link:** Configured in SPI Master Mode 0 running at a high-speed clock of **10 MHz** to instantly retrieve ADC sample packets.

---

## 📊 Hardware & Timing Architecture

The timing diagram and hardware setup below illustrate the low-overhead data fetching routine implemented in this repository:

<img width="1680" height="726" alt="image" src="https://github.com/user-attachments/assets/5fd964ae-7fc4-4f8f-9bf5-f5cb21b17b2f" />

### Pin Mapping

| MCU Pin | Peripheral | Connected To (NADC24B) | Signal Description |
| :--- | :--- | :--- | :--- |
| **PA.0** | SPI0_MOSI | SDI / MOSI | Serial Data Input |
| **PA.1** | SPI0_MISO | SDO / MISO | Serial Data Output |
| **PA.2** | SPI0_CLK  | SCLK | Serial Clock (10 MHz, Schmitt Trigger enabled) |
| **PA.3** | SPI0_SS   | /CS / SS | Chip Select Line |
| **PA.4** | GPIO Input| /DRDY | **Data Ready Pin** (Falling-edge triggers `GPA_IRQHandler`) |
| **PB.12/13**| UART0 | Debug Console | Serial Log output (`115200, 8-N-1`) |

---

## 🛠️ Data Processing Logic

The data flow is decoupled into an asynchronous hardware-to-software routine:

1. **Hardware Signaling:** The NADC24B completes an internal conversion cycle and pulls the `/DRDY` line (`PA.4`) **LOW**.
2. **Interrupt Handshake (`GPA_IRQHandler`):** The MCU catches the falling edge, clears the flag, and triggers an 8-bit SPI transaction to retrieve the raw 24-bit sample.
3. **Sign Extension & Conversion:** * Formats the 24-bit raw stream into a standard 32-bit signed integer (`int32_t`).
   * Calculates the true analog voltage using the ratio:

$$
\text{Volt} = \frac{\text{s32\_AdcData}}{\text{FULL\_RANGE}} \times 2.4\text{V}
$$

## 💻 Environment
* **MCU:** Nuvoton NuMicro M251 Series (Cortex-M23 core)
* **External ADC:** NADC24B Driver Board
* **IDE:** Keil MDK uVision
* **Clock:** 48MHz Internal High-Speed RC Oscillator (HIRC) as HCLK

## 💻 Environment
* **MCU:** Nuvoton NuMicro M251 Series (Cortex-M23 core)
* **External ADC:** NADC24B Driver Board
* **IDE:** Keil MDK uVision
* **Clock:** 48MHz Internal High-Speed RC Oscillator (HIRC) as HCLK
