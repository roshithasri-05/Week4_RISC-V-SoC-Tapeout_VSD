
# Velocity Saturation and Basics of CMOS Inverter VTC



# 1. SPICE Simulation for Lower Nodes and Velocity Saturation Effect

## 1.1 SPICE Simulation for Lower Nodes

**Goal:**
Investigate how drain current responds to varying gate and drain voltages for both long- and short-channel MOSFETs using SPICE simulations. This helps visualize advanced effects like velocity saturation, especially in short-channel devices.

**Device Configurations:**

* **Long Channel:** L = 1.2 μm, W = 1.8 μm
* **Short Channel:** L = 0.25 μm, W = 0.375 μm

---

## 1.2 Drain Current vs Gate Voltage for Long and Short Channel Devices

**Long-channel characteristics:**

* With a fixed drain voltage, long-channel devices follow the **square-law** model:
  I<sub>D</sub> increases with (V<sub>GS</sub> − V<sub>T</sub>)².
* This behavior matches classical MOSFET theory and is valid across a wide V<sub>GS</sub> range.

**Short-channel behavior:**

* When the channel is short, current no longer scales quadratically at higher V<sub>GS</sub>.
* Instead, the curve starts to **flatten**, indicating **velocity saturation**—a result of carriers reaching their maximum drift velocity.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/20f18463-de10-42d7-b5a2-d690c03493b9" />

---

## 1.3 Velocity Saturation at Lower and Higher Electric Fields

**Carrier motion regimes:**

* **Low electric field:**
  Velocity (v) increases linearly with electric field (E), i.e., v = μ·E.

* **High electric field:**
  Carrier velocity **saturates** at a maximum value, v<sub>sat</sub>, even if E increases.

**Effect on current:**

* In short-channel devices, high internal electric fields push carriers into velocity saturation.
* This transition suppresses the usual current growth, leading to a **linear** I<sub>D</sub>–V<sub>GS</sub> relation instead of quadratic.

**Design takeaway:**

* Short-channel scaling boosts speed but introduces non-ideal behavior that must be accurately modeled.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/111423b4-72c9-4aa0-81c0-3d80b0aada7a" />

---

## 1.4 Velocity Saturation Drain Current Model

**Generalized I<sub>D</sub> expression:**

```
I_D = K_n [ V_GT · V_min - (V_min² / 2) ] (1 + λ V_DS)
```

Where:

* V<sub>GT</sub> = V<sub>GS</sub> − V<sub>T</sub>
* V<sub>min</sub> = minimum of (V<sub>GT</sub>, V<sub>DS</sub>, V<sub>DSAT</sub>)
* V<sub>DSAT</sub> = saturation voltage due to velocity limits
* K<sub>n</sub> = process constant (μ<sub>n</sub>C<sub>ox</sub>W/L)
* λ = channel-length modulation parameter

**Interpretation:**

* At lower voltages, the model behaves like the ideal quadratic law.
* When velocity saturation dominates, current increases **linearly** with V<sub>GS</sub>.
* The (1 + λV<sub>DS</sub>) term captures output resistance in saturation.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/d82c840c-b0dd-454c-8af6-6bbf51dad0a2" />

---

## 1.5 Labs — Sky130 Id–V<sub>GS</sub> Simulation

Run this simulation to observe I<sub>D</sub> vs V<sub>GS</sub> for a short-channel NMOS device using sky130 models:

```bash
ngspice day2_nfet_idvgs_L015_W039.spice
plot -vdd#branch
```


---

## 1.6 Labs — Sky130 Id–V<sub>DS</sub> / V<sub>T</sub> Extraction

Use this simulation to extract threshold voltage and plot I<sub>D</sub> vs V<sub>DS</sub>:

```bash
ngspice day2_nfet_idvds_L015_W039.spice
plot -vdd#branch
```


---

# 2. CMOS Voltage Transfer Characteristics (VTC)

## 2.1 MOSFET as a Switch

MOSFETs function like switches in digital logic:

* **NMOS** turns ON when V<sub>GS</sub> > V<sub>T</sub>
* **PMOS** turns ON when V<sub>GS</sub> < −V<sub>T</sub>

In an inverter:

* V<sub>IN</sub> = 0 → PMOS ON → V<sub>OUT</sub> = V<sub>DD</sub>
* V<sub>IN</sub> = V<sub>DD</sub> → NMOS ON → V<sub>OUT</sub> = 0

To analyze switching behavior, use I–V load lines of each transistor and find where they intersect.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/64680935-b53d-4ee0-a274-6c0794fa86a4" />

---

## 2.2 Introduction to Standard MOS Voltage–Current Parameters

**Mapping parameters for inverter analysis:**

* NMOS:
  V<sub>GSN</sub> = V<sub>IN</sub>, V<sub>DSN</sub> = V<sub>OUT</sub>

* PMOS:
  V<sub>GSP</sub> = V<sub>IN</sub> − V<sub>DD</sub>,
  V<sub>DSP</sub> = V<sub>OUT</sub> − V<sub>DD</sub>

**Plotting note:**
To compare both devices on the same V<sub>IN</sub>–V<sub>OUT</sub> axes, shift PMOS characteristics to align with NMOS conventions.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/b6b9da15-5cde-4a40-80ad-2a6b88d623ea" />

---

## 2.3 PMOS/NMOS Drain Current vs Drain Voltage (Load Curves)

**Load curve insights:**

* NMOS curve: I<sub>D</sub> rises and then saturates; cutoff point = V<sub>IN</sub> − V<sub>T</sub>
* PMOS curve: mirrored version, shifted for P-type device parameters

**Intersection point = inverter output voltage** for a given V<sub>IN</sub>

Plot these intersections across a V<sub>IN</sub> sweep to build the inverter’s Voltage Transfer Curve (VTC).

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/42110191-9803-4600-98d9-2986cf4cc43d" />

---

## 2.4 Step 1 — Convert PMOS Gate-Source Voltage to V<sub>IN</sub>

**Transformation needed:**

* Convert PMOS parameters to match inverter input-output space:

  * V<sub>GSP</sub> = V<sub>IN</sub> − V<sub>DD</sub>
  * V<sub>DSP</sub> = V<sub>OUT</sub> − V<sub>DD</sub>

This lets you overlay PMOS and NMOS curves on the same plot.

<img width="1256" height="740" alt="Image" src="https://github.com/user-attachments/assets/1714654e-b562-4f68-ac72-ad2abe43671e" />

---

## 2.5 Step 2 & 3 — Convert PMOS and NMOS Drain-Source Voltages to V<sub>OUT</sub>

**PMOS transformation:**

* V<sub>OUT</sub> = V<sub>DD</sub> + V<sub>DSP</sub>

So:

* V<sub>OUT</sub> = 0 → maximum PMOS current
* V<sub>OUT</sub> = V<sub>DD</sub> → PMOS turns off

**NMOS remains direct:**

* V<sub>DSN</sub> = V<sub>OUT</sub>

These steps help align both I–V curves for intersection analysis.

<img width="1259" height="610" alt="Image" src="https://github.com/user-attachments/assets/2e82a2ea-9cfa-449f-8f37-74d751c32aab" />

---

## 2.6 Step 4 — Merge Load Curves and Plot VTC

**Steps:**

1. Sweep V<sub>IN</sub>, and for each point, plot I<sub>D</sub> vs V<sub>OUT</sub> for both PMOS and NMOS.
2. Find where their currents are equal → that’s the stable V<sub>OUT</sub> for that input.
3. Collect these points to build the inverter’s VTC.

**Typical VTC features (V<sub>DD</sub> = 2 V):**

* Low V<sub>IN</sub> → V<sub>OUT</sub> ≈ 2 V (logic high)
* High V<sub>IN</sub> → V<sub>OUT</sub> ≈ 0 V (logic low)
* Transition near V<sub>DD</sub>/2 with steep slope

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/3d98f010-44ce-4983-9870-a3b517e6173d" />

---

# 3. Summary

* **Velocity saturation** causes the drain current in short-channel devices to scale linearly (not quadratically) with gate voltage at high fields.
* **Simulations with Sky130** show this deviation clearly — especially when comparing I<sub>D</sub> vs V<sub>GS</sub> in long vs short channels.
* **CMOS inverter behavior** is understood by combining load curves of NMOS and PMOS and identifying their intersections.
* **VTC plots** reveal key switching behavior, gain region, and output logic levels.
* **Designers** must account for short-channel and high-field effects in modern CMOS circuits to ensure proper functionality and performance.
