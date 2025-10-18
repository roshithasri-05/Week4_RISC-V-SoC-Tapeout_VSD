
# 📘 NMOS Drain Current (Id) vs Drain-to-Source Voltage (Vds) 

## 1. 🔌 Introduction to CMOS Circuit Design

### 1.1 NMOS Transistors: The Building Blocks

At the heart of all CMOS digital logic lies the synergy between **NMOS and PMOS** transistors. While PMOS handles the pull-up path (to VDD), **NMOS devices form the pull-down path** (to GND).

**Key Characteristics of NMOS:**

* A **voltage-controlled device**: current between **Drain ↔ Source** is modulated by the **Gate-to-Source voltage (V<sub>GS</sub>)**.
* Operation is determined by:
  `V_GS`, `V_DS`, and the **threshold voltage (V<sub>T</sub>)**.

**Design Tip:**
The **W/L ratio** (Width/Length of the transistor) is critical:

* 🔹 Larger W/L → Higher drive strength → Faster switching
* 🔹 Smaller W/L → Lower power, slower switching

This trade-off directly affects **timing** and **power** behavior, especially when visualized using SPICE simulations for **VTC curves** and **delay profiles**.

---

### 1.2 Why SPICE Matters in Digital Design

Every standard cell you use in ASICs or digital libraries has been characterized using **SPICE** — a precise transistor-level simulation tool.

🔍 **SPICE Provides:**

* Accurate **I–V**, **C–V**, and transient response curves.
* Basis for LUTs (Lookup Tables) used in `.lib` files for STA tools.

🧠 These simulations form the backbone of:

* **Input Slew**: Slope of the incoming signal.
* **Output Load**: Capacitance at the output.
* **Delay**: Measured between 50% points of input and output waveforms.


![image](https://github.com/user-attachments/assets/81d96781-4030-4e72-9a5c-f7cf27662cf1)

Different cell versions like `BUFX2`, `INVX1`, etc., get individually simulated to extract realistic, physics-based performance metrics — ensuring signoff integrity.

---

## 1.3 🧩 NMOS Construction & Threshold Behavior

**NMOS (N-type MOSFET)** is built on a **P-type substrate** with heavily doped N<sup>+</sup> regions for **source** and **drain**. The **gate** lies above a thin oxide layer, forming the conductive channel when activated.

| Gate Voltage                       | Channel State                      |
| ---------------------------------- | ---------------------------------- |
| V<sub>GS</sub> = 0                 | No channel formed                  |
| 0 < V<sub>GS</sub> < V<sub>T</sub> | Weak depletion, no conduction      |
| V<sub>GS</sub> ≥ V<sub>T</sub>     | Inversion layer → channel conducts |


![image](https://github.com/user-attachments/assets/e339cc39-4a56-4fde-9109-5353b5ddd274)

**What determines V<sub>T</sub>?**

* Substrate doping
* Oxide thickness (t<sub>ox</sub>)
* Body bias (V<sub>SB</sub>), which induces the **body effect**

---

### 1.4 When NMOS Conducts: Strong Inversion

Once **V<sub>GS</sub> >> V<sub>T</sub>**, a strong inversion channel is formed. The drain current increases almost linearly with V<sub>DS</sub> — until saturation.

💡 **Strong inversion ensures:**

* Stable current flow
* Predictable digital/analog performance

---

### 1.5 Body Effect and V<sub>T</sub> Shift

If the **substrate (body)** is at a higher potential than the source, it **raises the threshold voltage**, reducing conduction.

🔬 SPICE models this via:

```
VTH = VTO + γ(√(2φF + VSB) - √(2φF))
```

Where:

* `γ` = body effect coefficient
* `VSB` = source-to-body bias

📌 Tip: To avoid V<sub>T</sub> variability in digital designs, tie the NMOS body to **ground**.

---

## 2. ⚙️ NMOS Operating Modes

### 2.1 Operating Regions Based on V<sub>GS</sub> and V<sub>DS</sub>

🧭 The NMOS works in two main regions:

* **Linear (Triode) Region** →
  If `V_DS < (V_GS - V_T)`, the device behaves like a **resistor**.

* **Saturation Region** →
  If `V_DS ≥ (V_GS - V_T)`, the channel is pinched off at the drain end.

---

### 2.2 Drain Current in Linear Region

At small V<sub>DS</sub>, the NMOS behaves like a controlled resistor:

```
I_DS ≈ μ_n C_ox (W/L) [(V_GS − V_T)V_DS − (V_DS²/2)]
```

This expression is key to:

* Analog amplifier design
* Accurate low-voltage SPICE modeling

---

### 2.3 Transport by Drift Current

Electron motion is dominated by **drift**:

```
J = q × n × μ × E
```

High electric fields near the drain lead to **mobility reduction** — captured in detailed SPICE models like BSIM.

---

### 2.4 Saturation Current Model

In saturation, the current no longer increases linearly with V<sub>DS</sub>:

```
I_DS = (1/2) μ_n C_ox (W/L) (V_GS − V_T)² (1 + λV_DS)
```

* `λ` models **channel length modulation**, giving a small slope to the otherwise flat I–V curve.

---

## 3. 🧪 SPICE Simulation for NMOS Devices

### 3.1 SPICE Essentials

🔧 SPICE needs two main components:

* **Netlist** → List of devices, connections
* **Model File** → Technology-specific `.lib.spice` file (e.g., `sky130.lib.spice`)


![image](https://github.com/user-attachments/assets/19dc0c16-9bca-4554-aa34-3fbedccf7b41)

---

### 3.2 Inside SPICE Models

SPICE supports accurate physics-based transistor models like **BSIM3** and **BSIM4**, which include:

* Body effect
* Velocity saturation
* Short-channel effects

---

### 3.3 Example SPICE Inputs

🧾 Typical parameters:

* **VTO** (threshold voltage)
* **KP** (transconductance factor)
* **λ**, **γ**, **C<sub>ox</sub>**, etc.

🔍 Sample netlist:

```spice
M1 vdd n1 0 0 nmos W=1.8u L=1.2u
R1 in n1 55
Vdd vdd 0 2.5
Vin in 0 2.5
```


![image](https://github.com/user-attachments/assets/cd7e17af-7efa-49b4-8f6d-d698c3f1c24a)

---

### 3.4 SPICE Netlist Syntax

Each component follows a simple syntax:

* `M` → MOSFET
* `R` → Resistor
* `V` → Voltage source

SPICE interprets these lines and performs simulations accordingly.


![SPICE Syntax](https://github.com/user-attachments/assets/6419babc-2765-4fab-973c-1f31f8f7ad61)

---

### 3.5 Running SPICE Labs with sky130

📦 Clone the design repo:

```bash
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
cd sky130CircuitDesignWorkshop
```

▶️ Run the simulation:

```bash
ngspice day1_nfet_idvds_L2_W5.spice
plot -vdd#branch
```


📊 Analyze real-world I–V curves and validate your theoretical knowledge.


---

## ✅ Summary and Learnings

* ✅ SPICE enables **precise modeling** of NMOS behavior.
* ✅ Understand NMOS I–V curves across **linear** and **saturation** regions.
* ✅ Use the **sky130 PDK** for real-device simulation and learning.
* ✅ Mastering SPICE helps you make **
