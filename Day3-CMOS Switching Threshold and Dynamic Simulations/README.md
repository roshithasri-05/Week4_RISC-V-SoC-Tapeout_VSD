
# CMOS Switching Threshold and Dynamic Simulations

## **1. Voltage Transfer Characteristics – SPICE Simulations**

### **1.1 SPICE Deck Creation for CMOS Inverter**

* A SPICE netlist was generated for a CMOS inverter using the Sky130 PDK.
* The inverter includes one PMOS and one NMOS transistor arranged in a complementary pair.
* The **PMOS** is connected to **VDD**, the **NMOS** to **GND**, and both gates are tied together to form the **input**.
* The **output** is taken from the node where the drains of both transistors meet.
* Appropriate **width-to-length ratios (W/L)** were chosen to achieve balanced switching behavior.
* This configuration acts as a foundation for all further simulations and characterizations.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/319db6fe-2664-48b8-826e-e9397aa04bba" />

---

### **1.2 SPICE Simulation for CMOS Inverter**

* The inverter netlist was simulated using **Ngspice**.
* The **Voltage Transfer Characteristic (VTC)** was obtained by sweeping the input voltage from 0 V to VDD.
* The simulation illustrates how the output voltage sharply switches near the **threshold point**.
* At this point, **V<sub>in</sub> = V<sub>out</sub>**, known as the **switching threshold (V<sub>m</sub>)**.
* The results confirm the expected inverter operation with a clean transition and well-defined logic levels.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/58b46102-0306-4db6-b9fc-83c838992227" />

---

### **1.3 Labs Sky130 SPICE Simulation for CMOS**

#### Voltage Transfer Characteristics


* Run the following commands in Ngspice:

```spice
ngspice day3_inv_vtc_Wp084_Wn036.spice
plot out vs in
```


---

* Run and plot the transient behavior:

```spice
ngspice day3_inv_tran_Wp084_Wn036.spice
plot out vs time in
```


---

## **2. Static Behavior Evaluation – CMOS Inverter Robustness (Switching Threshold)**

### **2.1 Switching Threshold, Vm**

* The **switching threshold (V<sub>m</sub>)** represents the input voltage where the inverter output equals the input.
* Simulations were carried out with various **PMOS and NMOS W/L ratios**.
* Key findings:

  * Increasing (W/L)<sub>p</sub> causes the switching point to shift higher.
  * V<sub>m</sub> values observed ranged approximately from **0.99 V to 1.4 V**.
* Inverter **rise and fall delays** were also measured to analyze how sizing affects timing.
* **Conclusion:**

  * Larger PMOS improves pull-up strength but increases delay during falling transitions.
  * A balanced inverter ensures symmetrical switching — ideal for use in **data path logic** where timing balance is critical.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/5d477e0f-45e0-4227-a1ad-be5b5b98a746" />

---

### **2.2 Analytical Expression of Vm as a Function of (W/L)p and (W/L)n**

#### Switching Threshold Voltage Expression

During the switching event, both NMOS and PMOS can be in saturation. The **switching threshold voltage (V<sub>m</sub>)** corresponds to the input voltage where the output falls to half the supply, i.e., **V<sub>DD</sub>/2**. This is critical for defining **noise margins** and **logic level stability**.


<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/c7446e70-2da0-4777-b869-8ccb767995e0" />

---

### **2.3 Analytical Expression of (W/L)p and (W/L)n as a Function of Vm**

* The inverse relation was derived to **determine required W/L ratios** for a given switching threshold.
* Once the desired V<sub>m</sub> is known, designers can compute the appropriate (W/L)<sub>p</sub> and (W/L)<sub>n</sub> values.
* This is essential in **optimizing inverter design** for trade-offs in **delay**, **area**, and **power**.
* It also provides a guideline for achieving **symmetrical switching characteristics**.

<img width="1920" height="1080" alt="Image" src="https://github.com/user-attachments/assets/289cb2db-5bf3-4652-b196-5372d5d99178" />  


---

### **2.4 Static and Dynamic Simulation of CMOS Inverter**

* This section includes both **DC and transient analysis** of the CMOS inverter.

* The static simulation confirmed a valid logic-level transition and output swing.

* For dynamic analysis, a **pulsed input** was applied to evaluate timing performance.

* Observations:

  * Delay depends on sizing and the effective capacitive load.
  * When both transistors are sized appropriately, rise and fall delays are nearly equal.

* This allows for assessment of inverter performance under real operating conditions.

---

### **2.5 Extended Static and Dynamic Simulation**

* Additional simulations explored the effects of **varying load capacitance**.

* The results confirm:

  * **Larger capacitance → longer propagation delay**
  * Transition slope becomes gentler with higher loads

* These outcomes align with delay models used in circuit design and emphasize the importance of accounting for load conditions.

---

## **3. Applications of CMOS Inverter in Clock Network and STA**


### **3.1 Applications in Clock Network**

* CMOS inverters are essential for **clock signal distribution**, especially in **H-tree** clock networks.
* They serve as **buffers** to restore and drive signals across different branches of the clock tree.
* Key design metrics for clock networks include:

  1. **Clock Skew** – Time mismatch between clock arrival points
  2. **Pulse Width** – Ensures consistent high/low time intervals
  3. **Duty Cycle** – Ideally maintained at 50%
  4. **Latency** – Total delay from clock source to sink
  5. **Power Consumption** – Must be optimized to reduce dynamic losses
  6. **Signal Integrity & Crosstalk** – Prevented by proper spacing and shielding

---

### **3.2 Static Timing Analysis (STA)**

* CMOS inverter delay directly affects **Static Timing Analysis (STA)**, which is vital for verifying timing closure in digital designs.

* STA evaluates whether each signal path meets timing constraints.

  * **Setup and Hold Time** checks
  * **Data Arrival vs Required Time** comparisons
  * **Slack Calculation:**

    ```
    Slack = Required Time − Arrival Time
    ```

    Slack must be **non-negative** to ensure reliable operation.

* **Example case:**

  * Clock Period: 1 ns
  * Clock Skew: 10 ps
  * Uncertainty: 90 ps

* Accurate inverter modeling and sizing are crucial for minimizing clock skew and maintaining timing margins in synchronous systems.

---

## **4. Summary**

* The **switching threshold voltage (V<sub>m</sub>)** of a CMOS inverter is influenced by the ratio of PMOS and NMOS sizing.
* Both simulation and analytical models confirm this dependency, aiding in precise design.
* **Dynamic behavior** is strongly impacted by capacitive load, with increased load causing longer delays.
* CMOS inverters are integral to **clock networks** and play a critical role in **Static Timing Analysis**, influencing synchronization across the chip.
* This study provides a comprehensive understanding of CMOS inverter characteristics using **Sky130 technology**, useful for both academic and practical design scenarios.

