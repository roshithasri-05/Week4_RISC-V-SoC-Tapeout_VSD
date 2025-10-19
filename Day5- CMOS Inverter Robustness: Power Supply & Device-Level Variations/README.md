
# ⚡ CMOS Inverter Robustness: Power Supply & Device-Level Variations

This module explores how CMOS inverter performance is impacted by two critical factors: **power supply variation** and **device-level fabrication inconsistencies**. Using **SmartSPICE simulations** with the **Sky130 PDK**, we analyze the impact on **VTC**, **noise margins**, and **switching behavior**.

---

## 🧪 1. Power Supply Variation Analysis

### 🔌 1.1 Simulating Inverter at Multiple Supply Voltages

We examine the effect of fluctuating V<sub>DD</sub> (e.g., **1.6 V**, **1.8 V**, and **2.0 V**) on CMOS inverter performance via SmartSPICE simulations.

**Key Observations:**

* **Lower V<sub>DD</sub> → Reduced Noise Margins**
* **Switching threshold (V<sub>m</sub>) shifts with V<sub>DD</sub>**
* **Slower rise/fall times** under low-voltage operation
* Performance degrades due to weaker drive strength and delayed transitions.

 ![image](https://github.com/user-attachments/assets/b48afb11-c7b3-42a1-8c75-b10adb36fac4)

---

### ✅ 1.2 Benefits vs Limitations of Low Supply Voltage

#### 🌟 Benefits

* Significant **dynamic power reduction** (P ∝ V<sub>DD</sub><sup>2</sup>)
* Minimizes **thermal hotspots** and **electromigration risk**
* Ideal for **portable, battery-operated** applications

#### ⚠️ Limitations

* **Shrinking noise margins**
* **Increased delay** due to slower transitions
* **Vulnerability to process/temperature variations**

Low voltage design requires careful balance between **power savings** and **robust logic behavior**.

---

### 🧪 1.3 Sky130 Lab Results: Supply Variation

Simulation of inverter circuits at various V<sub>DD</sub> values shows differing output responses and VTC shapes:


```bash
Gain = (y0 - y1) / (x0 - x1)
```

---

## 🏗️ 2. Device-Level Variations and Their Effects

### 🧵 2.1 Process Variability: Etching Impact

Fabrication irregularities such as **etching non-uniformity** result in deviations in transistor **channel length (L)** and **width (W)**.

**Consequences:**

* Alters drive currents and threshold voltages
* Causes **VTC asymmetry** and mismatch in PMOS/NMOS response

 ![image](https://github.com/user-attachments/assets/63a2fe26-c3c1-44b7-b079-947353b51bee)

---

### 🧪 2.2 Process Variability: Oxide Thickness Changes

Variability in **gate oxide thickness (t<sub>ox</sub>)** leads to inconsistency in **gate capacitance** and **V<sub>th</sub>**:

* **Thin oxide:** Faster switching, higher leakage, lower reliability
* **Thick oxide:** Slower response, higher delay, reduced gain

Maintaining **uniform oxide thickness** is crucial for predictable inverter behavior.

 ![image](https://github.com/user-attachments/assets/5433c90d-0c74-41ed-9638-794b386f15aa)

---

### 🧠 2.3 Device Variation Simulations in SmartSPICE

Simulation of device-level variations involves tuning parameters such as:

* **W/L ratio**
* **Threshold voltage (V<sub>th</sub>)**
* **Oxide thickness (t<sub>ox</sub>)**

The results show noticeable shifts in:

* Switching thresholds
* VTC curve symmetry
* Noise margin robustness

These highlight the **sensitivity of inverter behavior** to device mismatches.

 ![image](https://github.com/user-attachments/assets/0e302e64-5899-4a1a-b8dc-8f9850157af3)

---

### 📌 2.4 Design Takeaways

✅ **Power Supply Variation**

* Decreases logic level differentiation
* Slows down switching
* Reduces margin for error

✅ **Device-Level Variation**

* Causes mismatch and offset in VTC
* Impacts inverter balance and gain
* Requires **layout matching** and **tight process control**

Together, they represent two major reliability challenges in CMOS circuit design.

---

### 🧪 2.5 Sky130 Labs – Device Variation Results

Below are lab simulations using Sky130 for observing inverter robustness under device variation scenarios:


---

## 📊 Final Summary

This study highlights how **supply voltage scaling** and **fabrication inconsistencies** influence the stability and performance of CMOS inverters.

| Parameter             | Effect                                                         |
| --------------------- | -------------------------------------------------------------- |
| Low V<sub>DD</sub>    | Reduces power but weakens speed and noise immunity             |
| Etching Variation     | Alters transistor dimensions → mismatch and VTC asymmetry      |
| Oxide Thickness Shift | Affects gate capacitance, speed, and leakage behavior          |
| Simulation Tools      | SmartSPICE + Sky130 PDK used for analysis and curve evaluation |

Understanding these behaviors is essential to **design resilient digital circuits** in nanometer-scale technologies.

