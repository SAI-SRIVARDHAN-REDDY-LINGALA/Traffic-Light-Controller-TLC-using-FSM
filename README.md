# 🚦 Traffic Light Controller (TLC) using FSM with Delay Counters

**By: Sai Srivardhan Lingala 🧠✨**

---

## 📌 Overview

This project implements a **Finite State Machine (FSM)**-based **Traffic Light Controller** for a highway and a country road intersection. It uses an **internal delay counter** to manage transition delays **without inserting extra wait states**.

The design prioritizes the **highway traffic**, only allowing the **country road** green access if a vehicle is detected via a sensor (`X`).

---

## 💡 Problem Statement

### 🛣️ Scenario:

* **Two Roads:**

  * `Highway (hwy)` – Main road with default priority.
  * `Country Road (cntry)` – Side road with conditional priority.
* **Goal:**
  Efficiently control traffic lights using an FSM that transitions through:

  1. Green → Yellow → Red (for each direction)
  2. Internal delay counters for realistic light transitions.
  3. A sensor input (`X`) to detect waiting vehicles on the country road.

---

## 🧠 Key Features

* ✅ **Priority-based FSM Design**
* ⏱️ **Built-in Delay Handling** using a 4-bit counter (no external wait states)
* 💡 **Sensor-based Activation** for side-road traffic
* 🧪 **Fully Simulated Testbench** with waveform support (GTKWave)
* 🧾 **Comprehensive VCD Dump for Debugging**
* 🧼 Clean Verilog Best Practices (blocking/non-blocking, parameterized states)

---

## ⚙️ FSM Architecture

```text
       (X=1)                                         (X=1)
  S0 ─────────► S1 ──[Y2RDELAY]──► S2 ──[R2GDELAY]──► S3 ──┐
   ▲                                                       │
   │                                            [X=0]      │
   └──────────────[Y2RDELAY]◄──── S4 ◄─────────────────────
```

| State | Highway | Country | Description             |
| ----- | ------- | ------- | ----------------------- |
| `S0`  | GREEN   | RED     | Default highway flow    |
| `S1`  | YELLOW  | RED     | Prepare highway to stop |
| `S2`  | RED     | RED     | All stop (intermediate) |
| `S3`  | RED     | GREEN   | Country road gets green |
| `S4`  | RED     | YELLOW  | Prepare country to stop |

---

## 🔧 Delay Configuration

| Name       | Macro      | Value (Clock Cycles) |
| ---------- | ---------- | -------------------- |
| Yellow→Red | `Y2RDELAY` | 2                    |
| Red→Green  | `R2GDELAY` | 2                    |

---

## 📦 Files Included

| File Name                      | 📌 Icon | Description                                                                 |
|-------------------------------|:--------:|-----------------------------------------------------------------------------|
| `TLC.v`                       | 🔧       | Main FSM-based traffic light controller design (Verilog HDL)               |
| `test_tb.v`                   | 🧪       | Testbench to simulate and validate the FSM behavior                        |
| `Output in terminal.png`      | 📸       | Snapshot of terminal output during simulation showing signal/state activity|
| `State definition.png`        | 🧾       | Descriptive table showing state encodings and light mappings               |
| `Traffic light controller.png`| 🖼️       | System-level layout of the traffic light logic                             |
| `State transition diagram.png`| 🔁       | FSM state transition diagram for visualizing the controller behavior       |
| `README.md`                   | 📘       | Project documentation and instructions (this file!)                        |

---

## 🧪 Testbench Details

### ✅ What It Tests:

* Default no-car case: highway stays green.
* Sensor X triggers side-road transition.
* Delay logic validation for yellow/red switches.
* Multiple car events (entry & exit) handled.

### 📽️ Simulation Highlights:

```verilog
$monitor("Time=%0t | X=%b | HWY=%b | CNTRY=%b | STATE=%0d | DELAY=%0d", 
         $time, X, hwy, cntry, uut.state, uut.delay_counter);
```

🧠 **Monitor output includes state, sensor input, and delay counters** for maximum visibility.

---

## 🌊 GTKWave Setup (Optional but Recommended)

To view waveforms:

1. Run your simulation (using `iverilog` + `vvp`):

   ```bash
   iverilog -o test.out TLC.v test_tb.v
   vvp test.out
   ```

2. Open waveform:

   ```bash
   gtkwave test.vcd
   ```

---


## 🚀 Future Improvements (for Advanced Learners)

* 🔄 **Programmable delay values** via inputs
* 📦 Add AXI/UART interfaces for SoC integration
* 🛑 Emergency vehicle interrupt logic
* 🧠 AI edge detection for smart traffic management

---

## 👨‍💻 Author

**Sai Srivardhan Lingala**
🧠 Passionate about Hardware Design | Verilog | System Design
📫 [LinkedIn](https://www.linkedin.com/in/sai-srivardhan-lingala) | ✉️ [saisrivardhanlingala@gmail.com](mailto:saisrivardhanlingala@gmail.com)

---

## 📜 License

MIT License. Free to use, modify, and share for learning or projects.

---

