# STA
Timing analysis is integral part of ASIC/VLSI design flow. Anything else can be compromised but not timing!
[Source: VLSI Expert – Static Timing Analysis](https://www.vlsi-expert.com/p/static-timing-analysis.html)



---

**System Setup Time** ≥ **T\_setup\_ff** + **Tpd DIN (max)** − **Tpd Clk (min)**

* **T\_setup\_ff**: This is the internal setup time of the flip-flop itself. It's the non-negotiable requirement from the component's datasheet. Your calculation is incomplete without it.
* **Tpd DIN (max)**: The longest possible delay for the data path.
* **Tpd Clk (min)**: The shortest possible delay for the clock path.


**System Hold Time** ≥ **T\_hold\_ff** + **Tpd Clk (max)** − **Tpd DIN (min)**


---

Step-by-step process to calculate the **System Setup Time** using that formula.

This calculation tells you the setup time requirement for your circuit's input port, as seen from the outside world. It's the value you would publish in a datasheet.

### ## Step 1: Find `T_setup_ff` (Internal Flip-Flop Setup Time)

This value is **not calculated**. It is a fixed physical property of the flip-flop you are using.

* **Action:** Look up this value in the **datasheet** or the **standard cell library** provided by the manufacturer for your specific technology.

### ## Step 2: Find `Tpd DIN (max)` (Maximum Data Path Delay)

This is the longest possible time it takes for a signal to travel from the external `DIN` pin to the D-input of the first flip-flop inside your chip.

* **Action:** Use a Static Timing Analysis (STA) tool to report the path delay. The tool calculates this by summing the **maximum** delays of all components on the path:
    * Input buffer delay
    * Combinational logic gate delays
    * Net and wire delays

### ## Step 3: Find `Tpd Clk (min)` (Minimum Clock Path Delay)

This is the shortest possible time it takes for the clock signal to travel from the external `CLK` pin to the clock input of that same flip-flop.

* **Action:** Use the STA tool to report this path delay. The tool calculates this by summing the **minimum** delays of the buffers and wires along the clock path.

### ## Step 4: Calculate the System Setup Time

Now, plug the values from the previous steps into the formula.

**System Setup Time = ( `T_setup_ff` ) + ( `Tpd DIN (max)` ) − ( `Tpd Clk (min)` )**

The result is the setup time requirement for any external device that needs to send data *to your circuit*. For example, if the result is 2 ns, an external device must provide data to your `DIN` pin at least 2 ns before it sends the clock pulse to your `CLK` pin.

---

## ## Different Paths for Different Analyses

### 1. **Input-to-FF Path**
* **Purpose:** To define the **System Setup Time** for the chip's input ports.
* **Formula Used:** The one you asked about: `System Setup Time = T_setup_ff + Tpd DIN(max) - Tpd Clk(min)`.

### 2. **FF-to-FF Path**
* **Purpose:** To determine the **maximum internal operating frequency** of your chip. This is the most common analysis for the chip's core logic.
* **Formula Used:** The analysis checks if `T_logic_delay(max) + T_clk-q + T_setup_ff <= Clock Period`.

### 3. **FF-to-Output Path**
* **Purpose:** To define the **Clock-to-Output Time (`T_co`)** for the chip's output ports. This tells an external device how long it has to wait after a clock edge for the output data to be valid.

In short, you don't use the FF-to-FF path to calculate the "System Setup Time" because that's not what it's for. You analyze the FF-to-FF path separately to find your maximum clock speed. Each path type gives you a different, critical piece of information about your design's overall performance.

