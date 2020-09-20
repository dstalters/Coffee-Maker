# Coffee-Maker

#### Everyone it would seem has been working on a quarantine project to keep them busy and sane these last months. Not to be left out, I am sharing a portion of what I have been working on.

#### I have been interested in making a “pour over” coffee maker for several years now but didn’t want to just throw together some 3D printer looking contraption that would eat up all my precious one-bedroom apartment counter space. Instead I opted to go for the impractical, dream shot coffee maker that would (and this was non-negotiable since all my ex-barista friends would tell me so) make a cup of coffee at exactly the right temperature (measured at the filter), exactly the right bean-to-water ratio (measured to the nearest gram), exactly the right pour pattern (no sprinklers or concentric circles… had to be a spiral), and at exactly the right flow rate (measured to the nearest ml/s). Additionally, the water flow I determined must be laminar when it exited the pour spout just so that it all looked very nice.

#### This was to my naive mind totally do-able. And to an extent it still is, but $1000 and 100+ hrs. later (and still counting) I can say with certainty that it is now unlikely I will ever do better (financially and sanity-wise) than if I had just gone to my friend’s shop (difficult in quarantine) and bought that $5 cup of coffee every day for the rest of my life. But here I am. Soon to have an excellent, hands-free coffee maker but still using my french press every morning...

#### This portion is what I came up with to eliminate a flow meter (not enough room to package in the 2”x4” space available in the tower) from the hot water supply line. It was initially written in Python for easy design visualization. It is, however, totally executable on a C++ Teensy 4.0 (no exclusive NumPy or other non-portable methods used) as it will be implemented in the final coffee maker.

### Description of Coffee Maker’s Core Functions:

#### 1. Based on the weight of beans ground, a total water need is determined by the machine (this satisfies requirement regarding coffee to water ratio)

#### 2. Next the machine will determine the fractions of water to be delivered to the filter at each of the 4 stages of coffee filtering. Please see Blue Bottle tips (https://bluebottlecoffee.com/preparation-guides/pour-over).

#### 3. The machine will then use the shape and height of the hot water tank as well as the heating curve (assuming no losses at this point in the code but losses coming soon) to determine how much water must be pumped into the tank and heated during each rest period to satisfy head pressure needs to flow the needed amount of water in the specified time during each active pouring phase.

##### Discussion: This is required because geometry constraints and the need to fit a thermos under the filter while keeping the height of the coffee maker reasonable and fitting under cabinets (18” clearance) have limited the available potential head pressure in the line to 2”-3”. Actual temperature of the water in the tank is measured using a type K thermocouple and will be the final determinator of when the water is at the proper temperature (aka 198°F) - code calculations are mainly part of “smart management” system to ensure the correct head pressure is achieved in the tank before each pour and that heating of that added water is sure to occur in the allotted rest period.

#### 4. Head pressure requirement is determined from iteratively converging over each time step in an pour phase to determine how quickly water will actually flow through the plumbing system considering the use of flow control (on/off) solenoid, pipe fittings, and small diameter tubing all contributing some amount of head loss through friction and shape. These losses are of course dependent on the flow rate at that time step and thus necessitate the convergence of the system at each time step. Once this has all been completed and integrated over the entire pour period, the code will return the amount of water poured at each time step. If the flow rate of water is greater than needed, a shorter time period will be reported to pour the required volume of water. If the flow rate of water is less than needed, a modified period will be reported. If the flow rate is so reduced that the needed amount of water will never flow, this too will be reported.

### Challenges:

#### Convergence of the friction losses in the tubing posed the largest difficulty in getting a working (convergent) code as it would often diverge at certain critical flow rates. This was eliminated by using a continuous (and therefore no step change) friction calculation between the laminar and turbulent Reynolds numbers in calculating friction factors (lambda in graph below). Previously a digitally converted PDF of the Moody diagram was used to tabulate and interpolate friction factors at each Reynolds number for the relative pipe roughness of silicone tubing. This posed issues in the circled transition region where large jumps in friction factor would prevent code from converging and in fact prompted several flow rates to diverge.


![Moody Diagram](/moody_diagram.jpg)
