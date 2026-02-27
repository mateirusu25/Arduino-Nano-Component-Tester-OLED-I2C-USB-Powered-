# Arduino Nano Component Tester (OLED I2C Version)

This is a custom-configured version of the popular **Transistor Tester / Component Tester** (m-firmware by Markus Reschke), specifically adapted to run on a standard **Arduino Nano (ATmega328)** with a **0.96" I2C OLED display (SSD1306)**.

Unlike typical battery-powered builds, this configuration is heavily modified for **Desktop USB Power (Continuous/Auto-Hold Mode)**. It disables all battery monitoring and auto-sleep functions, preventing the dreaded "Bye!" shutdown loop when powered directly via USB. It stores all data in the Flash memory, meaning you only need to upload the `.hex` file.

## 🛠️ Components Required

1. **1x Arduino Nano** (ATmega328P)
2. **1x 0.96" OLED Display** (SSD1306, I2C, 4-pin)
3. **3x 680 Ω Resistors** (1% tolerance highly recommended for accuracy)
4. **3x 470 kΩ Resistors** (1% tolerance highly recommended)
5. **1x 10 kΩ Resistor** (Pull-up resistor for the button)
6. **1x Push Button** (Normally Open)
7. Breadboard and jumper wires

---

## 🔌 Hardware Wiring & Pinout

### A. OLED Display 0.96" (I2C - 4 Wires)
* **GND** -> Arduino **GND**
* **VCC** -> Arduino **5V** (or 3.3V)
* **SDA** -> Arduino **A4**
* **SCL** (or SCK) -> Arduino **A5**

### B. Test Probes (Where you insert the component to test)
* **TP1** (Test Pin 1) -> Arduino **A0**
* **TP2** (Test Pin 2) -> Arduino **A1**
* **TP3** (Test Pin 3) -> Arduino **A2**

### C. Resistor Matrix (Connected to D2 - D7)
* Between **D2** and **A0** -> **680 Ω** resistor
* Between **D3** and **A0** -> **470 kΩ** resistor
* Between **D4** and **A1** -> **680 Ω** resistor
* Between **D5** and **A1** -> **470 kΩ** resistor
* Between **D6** and **A2** -> **680 Ω** resistor
* Between **D7** and **A2** -> **470 kΩ** resistor

### D. Control Button
* Button Pin 1 -> Arduino **A3**
* Button Pin 2 -> Arduino **GND**
* **IMPORTANT:** Connect a **10 kΩ pull-up resistor** between Arduino **A3** and Arduino **5V** to prevent floating pin issues.

---

## 💻 Software & Tools Required

If you want to compile the code yourself or flash it to the Arduino, you will need:
1. **WinAVR**: For compiling the C code using the Makefile.
2. **XLoader**: A tiny utility to upload the compiled `.hex` file to the Arduino Nano directly (since we are bypassing the Arduino IDE).

---

## 🔨 How to Compile and Flash

### 1. Compile the Firmware (Optional)
If you made changes to `config.h` or `config_328.h`, you need to recompile:
1. Open Command Prompt (`cmd`) and navigate to the project folder.
2. Run the command: `make clean`
3. Run the command: `make`
4. [cite_start]This will generate a fresh `ComponentTester.hex` file[cite: 1, 8].

*(Note for Windows 10/11 users: WinAVR might throw a "child died before initialization" or "Resource temporarily unavailable" error at the very end of the `make` process. You can safely ignore this if the `.hex` file was generated successfully).*

### 2. Flash to Arduino Nano
1. Connect your Arduino Nano via USB.
2. Open **XLoader**.
3. Select the generated `ComponentTester.hex` file.
4. Set the Device to **Duemilanove/Nano(ATmega328)**.
5. Select your COM Port and set the Baud Rate to **57600** (or 115200 for newer bootloaders).
6. Click **Upload**.

---

## ⚙️ Usage & Calibration (Crucial Step!)

Because the tester runs in **Auto-Hold mode**, it will measure a component, display its value on the OLED, and freeze the screen until you press the button again.

### Navigation:
* **Short Click:** Measure the connected component (or move to the next menu item).
* **Multiple Short Clicks:** Enter the Main Menu.
* **Long Press (> 1.5s):** Select an option in the menu (or power off/sleep).

### First-Time Calibration (Adjustment)
To get accurate readings (and prevent resistors from showing up as capacitors due to breadboard capacitance), you MUST calibrate the tester:
1. Use jumper wires to **short-circuit all 3 test probes together** (A0, A1, and A2).
2. Do multiple short clicks on the button to open the Menu.
3. Click short until you highlight **Adjustment** (or Selftest).
4. Long-press the button to select it.
5. The tester will begin calibration. When the screen says **"Isolate Probes!"**, quickly remove the wires shorting A0, A1, and A2.
6. Let it finish. It will save the breadboard capacitance and wire resistance profiles to memory. 

Your Component Tester is now highly accurate and ready to use! Enjoy!
