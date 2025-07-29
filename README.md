# STA
Timing analysis is integral part of ASIC/VLSI design flow. Anything else can be compromised but not timing!



---

**System Setup Time** ≥ **T\_setup\_ff** + **Tpd DIN (max)** − **Tpd Clk (min)**

* **T\_setup\_ff**: This is the internal setup time of the flip-flop itself. It's the non-negotiable requirement from the component's datasheet. Your calculation is incomplete without it.
* **Tpd DIN (max)**: The longest possible delay for the data path.
* **Tpd Clk (min)**: The shortest possible delay for the clock path.


**System Hold Time** ≥ **T\_hold\_ff** + **Tpd Clk (max)** − **Tpd DIN (min)**

---


