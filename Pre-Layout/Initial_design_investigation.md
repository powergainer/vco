# Initial design investigation
This section covers the first design investigation for the High Speed VCO in Skywater 130nm.


Design scope
====
The scope of the work is to design a high speed VCO that can run in the GHz range (1.6 – 3.2GHz) with relatively low power consumption and low area.

A Phase Locked Loop (PLL) using this VCO in combination with appropriate frequency divider ratios can  generate a range of output frequencies up to 3.2GHz.

The design is to be implemented in 130nm CMOS technology.

![3p2GHz_PLL_diagram](https://user-images.githubusercontent.com/95447782/159557928-4e36689b-07c1-43bc-99af-7f9a7e779c6d.png)



VCO architecture
====
A detailed [literature survey](/Pre-Layout/VCO_PLL_Literature_Survey.docx) was conducted where we analyzed a large body of relevant papers in the VCO and PLL space.

Two of the main topologies used for VCO design are LC-VCO and Ring Oscillators. LC-VCOs tend to have low noise/jitter performance, but usually require more area due to the passive integrated inductors, and they have a narrower tuning range. Ring oscillators usually require less area, less power and have a wider tuning range at the expense of worse phase noise and jitter. [\[1\]](https://www.researchgate.net/publication/228864712_Comparison_of_LC_and_Ring_VCOs_for_PLLs_in_a_90_nm_Digital_CMOS_Process)
For this design we chose to implement a ring VCO. Delay stages can be either current starved inverters [\[2\]](https://ieeexplore.ieee.org/document/7755299), differential to single ended [\[3\]](https://ieeexplore.ieee.org/document/7755348) or differential cross-coupled delay stages [\[5\]](https://ieeexplore.ieee.org/document/9407468).


Current-starved VCO
====
We decide to go for current-starved single ended delay cells VCO topology due to design simplicity, potential for lower current consumption (less switching devices compared to differential topology) and the fact that we don't need quadrature outputs.

![image](https://user-images.githubusercontent.com/95447782/159561923-18572ea0-3958-446a-80d0-dfd113818a06.png)
Current-starved ring VCO [\[2\]](https://ieeexplore.ieee.org/document/7755299)


Design methodology
====
After deciding on a circuit topology we perform initial **design space exploration** and we **systematically narrow down the design variables** to get to our target performance. If during the design process we were to encounter limitations with this topology that prevented us from reaching the target specs, we would consider going back and evaluating another circuit topology.

This type of **bottom-up design approach** is an effective way to build a good understanding of the circuit and the relationships that govern its behaviour. From here, a systematic **top-down** design approach can be derived, eventually deriving what could be considered a **design recipe** that would allow the effective design and optimization of the VCO or PLL circuit to a certain performance specification.


Effect of Nstages: 5-stage vs 3-stage
------
Higher frequency can be achieved with 3 stages rather than 5 stages.

Fvco vs Vctrl for 3 vs 5 stages:
![image](https://user-images.githubusercontent.com/95447782/152638575-39170f98-0a9e-4087-888b-266762a9a17b.png)

Supply current (rms) vs Vctrl for 3 vs 5 stages:
![image](https://user-images.githubusercontent.com/95447782/152638600-cf312c5a-065f-4a95-95e7-a39406e051e8.png)


3 stages yields higher frequency at not much current increase.
From now on we focus on 3 stages.


VCO frequency vs current mirror ratio:
------
Mirror ratio increased from 1.29 to 1.6 to 2.4

Starting point: 1.29u/1.01u = x1.277 ratio

DP1: 1.6u/1.01u = x1.584 ratio

DP2: 2.4u/1.01u = x2.376 ratio

We can gain some speed at not much current increase.
But duty cycle starts to distort.
Not much current increase.

* Blue = 3-stage CS-VCO, mirrors 1.29u/.18u, inverters 0.5u/.18u (1.462GHz @ 0.9V Vctrl)
* Purple = 3-stage CS-VCO, mirrors 1.6u/.18u, inverters 0.5u/.18u (1.695GHz @ 0.9V Vctrl)
* Pink = 3-stage CS-VCO, mirrors 2.4u/.18u, inverters 0.5u/.18u (2.068GHz @ 0.9V Vctrl)

![image](https://user-images.githubusercontent.com/95447782/152638679-bf99ab79-a2ef-4ce9-96f7-176f2dee13d7.png)


|Current mirror multiplication factor	|Fvco @ 0.9V Vctrl (Hz)|	Kvco (Hz/V)|
|------------|------------|------------|
|1.277	|1.462E+09	|5.27E+09 Hz/V|
|1.584	|1.695E+09	|5.62E+09 Hz/V|
|2.376	|2.068E+09	|5.65E+09 Hz/V|

![image](https://user-images.githubusercontent.com/95447782/152638733-b8a1b642-7250-406c-ba6e-2241208b3f1d.png)

![image](https://user-images.githubusercontent.com/95447782/152638736-9f5acc31-cad4-4307-8fc7-ea8dd6436e4b.png)


Takeaway:
Hence increasing current mirror ratio can help increase frequency and Kvco.




Sensitivity to delay cell inverter drive strength:
-----
For a fixed mirror ratio, we show the effect of changing delay cell inverter drive strength (0.18um L vs 0.15um L).

Increasing delay cell inverters drive strength (reducing L from 0.18u to 0.15u)

* Blue = 3-stage CS-VCO, mirrors 1.6u/.18u, inverters 0.5u/.15u (2.16GHz)
* Green = 3-stage CS-VCO, mirrors 2.4u/.18u, inverters 0.5u/.15u (2.64GHz)

![image](https://user-images.githubusercontent.com/95447782/152638828-c01e81c4-7ec4-4877-a6c4-71a90fd05a42.png)


Takeaway:
Making inverters minimum length can benefit high frequency operation.
The reason is low capacitance on critical nodes of ring oscillator.
Frequency goes up significantly.
But duty cycle starts to distort.
And peak to peak swing goes down significantly (specially if mirror ratio also increased).

Leakage not much of a concern since top and bottom stacked mirror devices are not minimum length.




Kvco:
--------
Table of Kvco for each Design point.

|Design Point|			Kvco|
|------------|------------|
|mirrors 1.29u/.18u|			5.27E+09 Hz/V|
|mirrors 1.6u/.18u|			5.62E+09 Hz/V|
|mirrors 2.4u/.18u|			5.65E+09 Hz/V|
|mirrors 1.6u/.18u, invs 0.5u/.15u|			6.73E+09 Hz/V|
|mirrors 2.4u/.18u, invs 0.5u/.15u|			6.63E+09 Hz/V|

![image](https://user-images.githubusercontent.com/95447782/152638918-b112174d-7f43-41e1-9e05-8159bac787ab.png)




Design comparison at this point:
--------
F vs Vctrl for various design points so far:
![image](https://user-images.githubusercontent.com/95447782/152638931-341f046e-7443-44dc-8cd2-3050ec599e56.png)

Current consumption vs Vctrl:
![image](https://user-images.githubusercontent.com/95447782/152638934-b6bda38d-de88-48fe-9ace-bc7f6dfe32f5.png)


* Looks like design with mirrors at 1.6u/0.18u and min length inverters may be an ok tradeoff for relatively high speed, and oscillator internal phases not too distorted.
* Possibly reduce mirror ratio to help reduce duty cycle distortion a bit without losing too much speed.


Further optimization:
===
We tried to increase oscillation speed by trying to optimize the following aspects of the circuit:

Fixing the output swing:
----
Rising edge is slower than falling edge, look into making PMOS stronger than NMOS in delay cells and/or output buffer. --> This was fixed by making the PMOS on the last stage of the output buffer stronger than the NMOS.

![image](https://user-images.githubusercontent.com/95447782/153831511-d61e6ae6-783c-46fe-a107-447aa74bc398.png)


Optimization of delay cell dimensions:
---
Can we increase oscillation speed with larger W on delay cells (more drive strength, able to charge capacitance faster)? But then drain capacitance will increase thus reducing speed? So probably the way to go is to reduce inverter W, since that may reduce drain cap, if that dominates speed then speed may go up.

**Sweep of delay cell pmos & nmos dimensions:**
![image](https://user-images.githubusercontent.com/95447782/153833100-61898cc3-aed9-4b16-8373-251d48693439.png)

So, Fvco goes up with smaller delay cell devices, as expected, due to lower capacitance in the internal nodes of the ring oscillator.

Also skewing the PMOS versus the NMOS helps improve speed.

Best speed is achieved at *0.5um/0.15um PMOS and 0.36um/0.15um NMOS* device dimensions for the ring oscillator delay cells.



Output buffer optimization:
---
The load capacitance presented by the output buffer to the ring oscillator reduces operating speed. Can the ring oscillator drive the output buffer fast enough? Maybe reduce output buffer 1st stage. Investigate effect of output buffer on speed. More strength better? But more capacitance? So what's the best trade off?

This is the output buffer topology before optimization:
![image](https://user-images.githubusercontent.com/95447782/153834407-630b7a6f-19c6-4860-a134-a0bce4a906a5.png)

We find out best trade-off for 1st stage of output buffer, since that's what loads the ring oscillator core.
**Fvco Vs size of 1st stage of output buffer**
![image](https://user-images.githubusercontent.com/95447782/153834112-7c89af89-1522-45b2-9abf-036fa8603061.png)

We can see the speed gain reducing the gate area attached to the VCO output. But it gets to a point that the 1st stage of the output buffer can't drive the 2nd stage so we loose peak to peak swing.

This is what the waveform looks like for the near-3 GHz design points:

* Red (3.129 GHz): M21 1.6u/0.15u, M22 0.8u/0.15u, delay cell pmos and nmos are kept as 0.5u/0.15u, mirrors 2.4u/.18u.

* Blue (2.971 GHz): M21 2.0u/0.15u, M22 1.0u/0.15u, delay cell pmos and nmos are kept as 0.5u/0.15u, mirrors 2.4u/.18u.

* Green (2.824 GHz): M21 2.4u/0.15u, M22 1.2u/0.15u, delay cell pmos and nmos are kept as 0.5u/0.15u, mirrors 2.4u/.18u.

![image](https://user-images.githubusercontent.com/95447782/153835006-5d7288dd-e287-4573-a5d9-e00535ec2256.png)

But so far we can't reduce M21 and M22 under 1.2um PMOS and 0.6um NMOS width without losing the output swing.

So, in order to increase speed as much as possible, we insert an intermediate stage in the output buffer, hence we can keep reducing the 1st stage while at the same time recovering the peak to peak swing at the output.
![image](https://user-images.githubusercontent.com/95447782/153835200-34cb885b-65c9-4766-95bd-37859dc6f3ba.png)

With this change, we can keep reducing M21 and M22 in the 1st stage of the output buffer. We can go as low as 0.58um PMOS and 0.36um NMOS width.
We call this design point **DP5 --> 4.11GHz oscillating frequency at 0.9Vctrl, typical corner, VDD = 1.8V.**
	
![image](https://user-images.githubusercontent.com/95447782/153836151-d83eb80c-111b-47a5-89bb-4dce3eb79970.png)


Output waveform for DP5 at 4.11GHz at 0.9V Vctrl:
![image](https://user-images.githubusercontent.com/95447782/153836337-e5964f33-1def-4320-ae21-2ef2d1c5443d.png)


Fvco Vs Vctrl for DP5:
===
This is Fvco Vs Vctrl for the design point called DP5, versus previous design iterations (to see the evolution through the design optimization process). *Please note these results are pre-layout results without parasitic capacitances and are therefore expected to drop after we implement the layout*, however these serve as the initial guidance.
![image](https://user-images.githubusercontent.com/95447782/153836578-7e8d3f68-c3d2-4058-b8eb-e5fe2080424e.png)

Supply current:
![image](https://user-images.githubusercontent.com/95447782/153836609-8784f1f0-6e8e-4350-86d3-edc6ca1d1710.png)

Kvco:
![image](https://user-images.githubusercontent.com/95447782/153836661-e5ed1312-3dc4-438f-bde6-545ac66fa0d4.png)


**Notes / Further work:**
* *These are pre-layout numbers*. Will come down with post-layout extraction.
* This frequency is achieved with a load on the output of the VCO that represents a divide-by-two circuit that would be present as part of the feedback in a PLL system as well as clock buffers that would buffer the clock downstream.
* In current design, oscillator needs Vctrl higher than 0.8V to oscillate. Limiting factor here is vco core internal node voltage swing too low when delay cells are current starved.



Extending oscillation range to lower Vctrl values:
---
At Vctrl < 0.8V, oscillation lost. Limiting factor here is vco core internal node voltage swing too low when delay cells are current starved.

These are net10, net11, net12:

![image](https://user-images.githubusercontent.com/95447782/153843104-77f62832-470c-443b-82ba-4ff68a1fbfad.png)

This is Vctrl = 0.8V, output swing ok, internal net10, net11, net12 swing ok. Net10 swings from 0.08V to 1.27V, hence it can trigger the 1st inverter of the output buffer.

![image](https://user-images.githubusercontent.com/95447782/153843171-a8fbdd4a-fc78-4a65-b0ab-7c5ead731542.png)


This is Vctrl = 0.7V, node10 voltage swing too low to trigger next inverter (net10 only reaches 0.96V briefly at its peak).

![image](https://user-images.githubusercontent.com/95447782/153843214-0d22e43b-cc9d-4999-b172-34aa5bf6c411.png)



How to fix (to recover oscillation at Vctrl < 0.8V):
* More current in current mirrors
* Less resistive current mirror devices
* Swing recovery techniques (circuit topology change)

How to hack/workaround:
* Voltage shifting before Vctrl: Create Vctrl_2 = Vctrl + delta,  or Vctrl_2 = a * Vctrl + b
* Just live with it? This may be acceptable as a trade-off depending on system requirements

Next steps
---
* Create preliminary VCO  layout, extract and run simulation with parasitic components. See frequency drop due to parasitics, back-annotate parasitic caps into schematic sims, iterate design.
* Action completed - See [Post-layout simulation results](/Post-Layout/Post-Layout.md)
