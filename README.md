# 3GHz High Speed VCO
High Speed VCO in Google-Skywater 130nm process

![image](https://user-images.githubusercontent.com/95447782/159545975-644caf80-5cb7-410d-9a74-c7f3799e71a4.png)

![image](https://user-images.githubusercontent.com/95447782/159536411-3104660e-a312-41c9-ad33-6c2830ce608c.png)

![image](https://user-images.githubusercontent.com/95447782/159129021-774e9976-ce00-4699-9d40-47be3756df81.png)



Key features
===
* \> 3GHz VCO output frequency
* 1.8V supply
* Programmable bias current modes
* Custom VCO layout to minimize parasitics and achieve high frequency (link to post-layout)
* Custom high speed frequency divider for >3.5GHz operation
* Area 0.3mm^2 (VCO) + 0.5mm^2 (10 Frequency dividers) 
* Taped out in Google-Skywater 130nm process




|		|	min	|	typ	|	max	|	Units	|	Conditions	|
|	------------	|	------------	|	------------	|	------------	|	------------	|	------------	|
|	Supply voltage	|	1.62	|	1.8	|	1.98	|	V	|		|
|		|		|		|		|		|		|
|	Fvco	|		|	2.68	|		|	GHz	|	@ 0.9V Vctrl, x0.125 bias mode	|
|		|		|	3.21	|		|	GHz	|	@ 0.9V Vctrl, x0.25 bias mode	|
|		|		|	3.46	|		|	GHz	|	@ 0.9V Vctrl, x0.5 bias mode	|
|		|		|	3.72	|		|	GHz	|	@ 0.9V Vctrl, x1 bias mode	|
|		|		|	3.94	|		|	GHz	|	@ 0.9V Vctrl, x2 bias mode	|
|		|		|		|		|		|		|
|	Supply current (VCO)	|		|	329.8	|		|	uA rms	|	@ 0.9V Vctrl, x0.125 bias mode	|
|		|		|	393.6	|		|	uA rms	|	@ 0.9V Vctrl, x0.25 bias mode	|
|		|		|	425.9	|		|	uA rms	|	@ 0.9V Vctrl, x0.5 bias mode	|
|		|		|	449.4	|		|	uA rms	|	@ 0.9V Vctrl, x1 bias mode	|
|		|		|	464.0	|		|	uA rms	|	@ 0.9V Vctrl, x2 bias mode	|
|		|		|		|		|		|		|
|	Kvco	|		|	9.31	|		|	GHz/V	|	@ 0.9V Vctrl, x0.125 bias mode	|
|		|		|	9.94	|		|	GHz/V	|	@ 0.9V Vctrl, x0.25 bias mode	|
|		|		|	8.78	|		|	GHz/V	|	@ 0.9V Vctrl, x0.5 bias mode	|
|		|		|	8.78	|		|	GHz/V	|	@ 0.9V Vctrl, x1 bias mode	|
|		|		|	6.79	|		|	GHz/V	|	@ 0.9V Vctrl, x2 bias mode	|
|		|		|		|		|		|		|
|	Area	|		|	0.3	|		|	mm^2	|	VCO only	|
|		|		|	0.5	|		|	mm^2	|	Frequency dividers (10 instances)	|
|		|		|		|		|		|		|
|	Noise	|		|		|		|		|		|
|		|		|		|		|		|		|



Design files
====
* [Schematics](https://github.com/powergainer/caravel_user_project_analog_vco/tree/main/xschem)
* [Layout](https://github.com/powergainer/caravel_user_project_analog_vco/tree/main/mag)
* [GDS](https://github.com/powergainer/caravel_user_project_analog_vco/tree/main/gds)

Simulation results
====
* [Pre-layout](/Pre-Layout/Initial_design_investigation.md)
* [Post-layout](/Post-Layout/Post-Layout.md)


Introduction to Skywater 130nm technology and PDK:
====
The design methodology used in this projects is based on the Skywater 130nm PDK and open-source EDA toolflow.

More information on details about Google-Skywater 130nm technology and PDK and the open-source EDA toolflow can be found here:

[VSDIAT Skywater 130nm Physical Verification Workshop](https://github.com/powergainer/VSDIAT_Physical_Verification_Sky130)


Project plan and MPW shuttle schedules:
===



