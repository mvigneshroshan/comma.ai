# comma.ai
Hardware Challenge - Harness Tester Challenge

This document outlines the show-stopping bugs identified in the schematic and PCB layout of the Harness Tester, per the design goals. The issues are categorized clearly and do not include suggestions or improvements, as instructed.

Bugs & Issues in the Schematic
1.	Missing Bypass Capacitor for Teensy 4.1 (U2): The Teensy’s 3.3V line powers other components like the CY8C9560A and RGB LED, but there's no capacitor nearby to stabilize voltage.
2.	I2C Pull-up Mistake on CY8C9560A Bus: CY_SDA is pulled to GND (should be 3.3V). CY_SCL is correctly pulled to 3.3V. This would break communication.
3.	I2C Pull-ups Missing for GPS Module (U3): The SDA and SCL lines going to the GPS have no pull-up resistors, which are needed for the I2C bus to function properly.
4.	RGB LED Lacks Current-Limiting Resistors: The red, green, and blue LED lines are connected directly to GPIOs with no resistors. This could damage the LED or the microcontroller.
5.	Voltage Regulator Pins May Be Reversed (U1 – L7805): It looks like the input and output pins of the 7805 are swapped, which can prevent it from working or even damage it.
6.	No Reverse Polarity Protection on 12V Input: There's a diode for voltage spikes, but not one that blocks reverse polarity. Reversing the 12V supply could damage the board.
7.	MicroSD Card Circuit Missing: The design mentions using microSD to save test data, but there is no microSD socket or connection shown in the schematic.
8.	USB Lines Possibly Conflicting: USB D+/D− from the Teensy are connected to both the USB port and the GPS, which could cause communication problems.
9.	MOSFET (Q1) Gate Control Missing: There is a pull-down resistor, but no clear signal to turn the MOSFET on. It might not switch at all.
10.	Teensy VBAT Pin Left Floating: This pin is used for keeping the clock running when power is off. It floats in the design and may cause undefined behavior.
Bugs & Issues in Layout
1.	Unrouted Net Present: KiCad reports 1 unrouted net in the PCB layout.
2.	No Current-Limiting Resistors for RGB LED (D3): LED_R, LED_G, and LED_B are routed directly to the LED pins without any resistors.
3.	MicroSD Socket on PCB but Missing in Schematic: The MicroSD connector is placed in the layout, but there’s no corresponding symbol or net in the schematic.
4.	Board Edge Clearance Violations (U2 Pins 1 and 48): Violates 0.5 mm minimum edge clearance rule (actual = 0.48 mm).
5.	No GND Stitching Vias Around GPS & Antenna (U3, AE1)
6.	The antenna trace (RF_IN) lacks surrounding ground vias and a proper RF return path.
7.	Power Trace Widths on +5V, +3.3V, GND Are 0.8128 mm (32 mil): Acceptable for ~1 A, but if current exceeds this or trace length increases, voltage drop or heat may occur.
8.	Courtyard Overlap Between C6 and U5: Component courtyards overlap, violating IPC layout standards.
9.	Silkscreen Over Pads (example J3, D3): Some silkscreen elements overlap with component pads.
10.	Long Parallel Traces on Cable Header (J3): High-density area under J3 shows long, closely spaced parallel signals.
11.	Silkscreen Label “START TEST” Touches Switch Outline: The label slightly overlaps SW1’s boundary.
12.	Uneven Fill Connections to Pads/Vias: Some pads (around U3) are connected to the fill with minimal neck width.
