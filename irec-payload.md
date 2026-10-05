# IREC Rocketry Payload (2025-2026)

**Skills:** Systems Integration, Thermodynamics, Electromechanical Actuation, Fluid Mechanics
**Mission:** Maintain a biologically viable environment (< 42°C) for *Bacillus Subtilis* bacteria during a 30,000-foot apogee flight while surviving high-G launch forces.

## Electromechanical Architecture
![Sabot Isometric View](Sabot_Isometric.png)
![Internal Payload Assembly](Payload_Assembly.png)

The payload was designed with a modular, tiered 4U CubeSat architecture to optimize space and simplify assembly within the strict diameter of the rocket airframe. Components were stacked vertically to minimize the footprint, securely housed within a custom-modeled sabot geometry.

## Actuation & Physics (Upper Compartment)
![Upper Compartment](Payload_Upper_Compartment.png)

To perform the metabolic experiment during flight, the payload required a highly robust, high-torque fluid deployment system driven by linear actuators. 
* **Eliminating Off-Axis Torque:** The dual linear actuators drive a custom Polycarbonate-Carbon Fiber (PC-CF) unified interface plate. This unified plate distributes the normal force evenly, completely eliminating off-axis torque and preventing the syringe plungers from binding under high-G ascent vibrations.
* **Friction Mitigation:** Utilized two-part silicone-free syringes for the primary driving and metabolic receiving to minimize static friction and ensure smooth actuator translation.

## Fluidics & Active Cooling (Middle Compartment)
![Middle Compartment](Payload_Middle_Compartment.png)

The middle compartment houses the core biological experiment and the primary thermal bridge.
* **Isobaric Fluid Expansion:** To guarantee a strict anaerobic (0% oxygen) environment, the system utilizes a syringe-to-syringe expansion method. The injection volume perfectly translates to the receiving volume ($\Delta V = 0$), relying on isobaric expansion ($P_1 V_1 = P_2 V_2$) so the internal pressure remains constant at 1 atm.
* **Conductive Thermal Bus:** The cold side of the Peltier system is mounted directly to a custom aluminum "Cold Block," which interfaces physically with the syringes to maximize conductive thermal transfer to the fluid.

## Fluorometer Integration & Optical Detection
![DIYNAFLUOR Baseline](traulab_DIYNAFLUOR_Exploded_View.png)
**Caption:** *Baseline DIYNAFLUOR open-source architecture by Traulab.*

![Custom Sensor Enclosure](Fluorometer_Ruggedized_Casing.png)
**Caption:** *Baseline DIYNAFLUOR optical layout adapted into my custom ruggedized enclosure designed for high-G flight.*

The core scientific objective relied on a fluorometer to measure bacterial metabolic rates. Rather than reinventing the optical detection principles, I leveraged the open-source **traulab/DIYNAFLUOR** project as our baseline architecture. 

* **Ruggedized Enclosure Design:** I designed a custom, thick-walled housing in Fusion 360 to adapt this lab-bench device for rocket flight. The deliberate, simple geometry prioritized structural rigidity and rapid 3D printing over complex aesthetics. 
* **Mechanical Immobilization:** The internal cavity was tolerance-fit to the optical block, while the lid utilized a four-point screw mount to ensure the intense launch vibrations would not shift the focal point or disrupt continuous data logging.

## Thermal Exhaust & Shielding (Sabot)
![Sabot Cross-Section](Sabot_Isometric_Section.png)

Aerodynamic heating at high velocities threatened the biological samples, necessitating an active cooling loop and strategic insulation.
* **Forced Convection Exhaust:** To dissipate the waste heat from the Peltier's hot side, dual fans drive forced convection ($Q = hA\Delta T$) across a finned heatsink. This heat is actively vented out of the airframe through targeted exhaust holes in the sabot corridor.
* **Aerodynamic Shielding:** The sabot housing features rectangular wall recesses filled with high R-value insulation to passively combat aerodynamic heating during ascent.

## Avionics Packaging & Integration (Lower Compartment)
![Lower Compartment](Payload_Lower_Compartment.png)

The physical arrangement of the control electronics and power systems was dictated by rocket flight dynamics and safety constraints.
* **Z-Axis CG Optimization:** The high-mass NiMH battery arrays were deliberately placed at the lowest possible Z-axis coordinate. This lowered the Center of Gravity (CG), dynamically stabilizing the payload within the airframe.
* **Physical Avionics Isolation:** The lower electronics compartment is physically walled off from the middle fluidics compartment. This structural isolation ensures the microcontrollers and SD data loggers are protected from potential leaks or aerosolized biological media during extreme launch vibrations.

![Primary Avionics](Payload_Electronics_PCB1.png)
![Fluorometer Operations](Payload_Electronics_PCB2.png)
![Payload Software Flowchart](Payload_Software_Flowchart.png)

* **Distributed Control Systems:** The architecture splits processing between two distinct boards. The primary PCB handles the Raspberry Pi Pico, real-time clock (RTC), altimeter, and actuator/fan power distribution. A secondary Arduino Uno board is dedicated strictly to the continuous TSL2591 light sensor readings for the fluorometer experiment. 
* **Packaging Constraints:** Integrating standard breakout boards and the Arduino footprint required careful standoff placement and wire routing within the lower sabot to ensure reliable connections without interfering with the battery mass. The custom firmware logic (detailed in the flowchart above) ensures these isolated systems coordinate precise timing for temperature control and fluid injection before and after launch.
