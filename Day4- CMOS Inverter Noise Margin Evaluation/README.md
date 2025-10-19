
# 📐 CMOS Inverter Noise Margin Evaluation using Sky130 & Ngspice

This repository provides a detailed evaluation of **CMOS inverter noise margin robustness** with simulations powered by the **Sky130 PDK** and **Ngspice**.

We explore how inverter noise margins behave under different transistor sizing configurations, and how to **extract key noise parameters from the Voltage Transfer Characteristic (VTC)**.

---

## ⚙️ Section 1: Static Analysis of CMOS Inverter Noise Margins

### 🔍 1.1 What Is a Noise Margin?

Noise margin indicates how immune a logic gate (like a CMOS inverter) is to electrical noise—an essential factor in reliable digital circuit design, especially at submicron technologies.

#### Comparing Ideal vs Real-World CMOS Inverters

* **Ideal Behavior**

  * Sharp transition at Vin = VDD/2
  * Almost infinite slope at the threshold point

* **Actual Behavior**

  * Due to resistive and capacitive elements, transition is gradual
  * The slope near the transition is ≈ –1
  * Output approaches, but never perfectly reaches, 0 V or VDD

In summary:

* Input from 0 → VIL → Output ~ VOH (High)
* Input from VIH → VDD → Output ~ VOL (Low)
* The range VIL to VIH represents the **transition zone**

> ![image](https://github.com/user-attachments/assets/926f7cba-df7a-469d-8ba2-aac09db38ea4)

---

### 📈 1.2 Understanding the VTC Curve

A **Voltage Transfer Characteristic (VTC)** graph shows how Vout changes with Vin for a CMOS inverter.

* **X-axis:** Input voltage (Vin)
* **Y-axis:** Output voltage (Vout)

Observations:

* Vout ≈ VDD when Vin is low
* Rapid fall in Vout near Vin ≈ VDD/2
* Vout ≈ 0 V when Vin is high

> ![image](https://github.com/user-attachments/assets/54f63d85-d471-41ca-bd99-03e4809723b9)

---

### 🧮 1.3 Defining the Noise Margins

To ensure **stable logic levels between cascaded gates**, two key margins are defined:

[
\text{NM}*\text{H} = V*\text{OH} - V_\text{IH}
]
[
\text{NM}*\text{L} = V*\text{IL} - V_\text{OL}
]

* **NMH** – High-level noise immunity
* **NML** – Low-level noise immunity

Higher values imply **greater resilience to input disturbances**.

> ![image](https://github.com/user-attachments/assets/e642928a-4161-41a9-8687-8fcfed8486b0)

---

### 🧪 1.4 Impact of PMOS Width on Noise Margins

Changing the **Wp/Wn ratio** influences:

* **Switching threshold**
* **Noise tolerance**
* **Speed and stability**

**Design Guidelines:**

* Ensure **VOL < VIL** so logic 0 is detected correctly
* Ensure **VOH > VIH** to maintain valid logic 1
* Keep slope ≈ –1 at threshold for maximum gain and noise immunity

> ![PMOS Width Effect](https://github.com/user-attachments/assets/77896404-fa7a-4ba8-bd7b-3f351e802b4a)

---

## 🧰 Section 2: Sky130 Simulations with Ngspice

### ⚡ 2.1 Running the Simulation

To simulate the inverter circuit and view the VTC:

```bash
ngspice day4_inv_noisemargin_wp1_wn036.spice
plot out vs in
```

Outputs:

<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/c4ed6ba0-27a6-495c-8717-a4d6e18f7232" />
<img width="1210" height="773" alt="Image" src="https://github.com/user-attachments/assets/e05c198b-f2d8-45c9-a4f8-c50f67e3ef1b" />

---

### 📍 2.2 Extracting Key Voltage Points

To determine VIL, VIH, VOH, and VOL from the simulation:

1. Select **point on PMOS curve (top left)** → Terminal displays `x0 = VIL`, `y0 = VOH`
2. Select **point on NMOS curve (bottom right)** → Terminal displays `x1 = VIH`, `y1 = VOL`

These values are then used in the noise margin formulas.

---

## ✅ Summary

| Concept              | Description                                             |
| -------------------- | ------------------------------------------------------- |
| **Noise Margin**     | Measure of inverter’s resistance to input disturbances  |
| **VTC Curve**        | Visualizes Vin-Vout behavior of a CMOS inverter         |
| **NMH & NML**        | High and low noise margin equations, respectively       |
| **Wp/Wn Tuning**     | Adjusts switching point to balance speed and robustness |
| **Ngspice + Sky130** | Used for simulation and accurate VTC extraction         |

