# Coffee-Maker

#### Everyone it would seem has been working on a quarantine project to keep them busy and sane these last months. Not to be left out, I am sharing a portion of what I have been working on.

#### I have been interested in making a “pour over” coffee maker for several years now but didn’t want to just throw together some 3D printer looking contraption that would eat up all my precious one-bedroom apartment counter space. Instead I opted to go for the impractical, dream shot coffee maker that would (and this was non-negotiable since all my ex-barista friends would tell me so) make a cup of coffee at exactly the right temperature (measured at the filter), exactly the right bean-to-water ratio (measured to the nearest gram), exactly the right pour pattern (no sprinklers or concentric circles… had to be a spiral), and at exactly the right flow rate (measured to the nearest ml/s). Additionally, the water flow I determined must be laminar when it exited the pour spout just so that it all looked very nice.

![Coffee-Maker-Layout](/coffee_maker_layout_annotated.png)

#### This was to my naive mind totally do-able. And to an extent it still is, but $1000 and 100+ hrs. later (and still counting) I can say with certainty that it is now unlikely I will ever do better (financially and sanity-wise) than if I had just gone to my friend’s shop (difficult in quarantine) and bought that $5 cup of coffee every day for the rest of my life. But here I am. Soon to have an excellent, hands-free coffee maker but still using my french press every morning...

#### This portion is what I came up with to eliminate a flow meter (not enough room to package in the 2”x4” space available in the tower) from the hot water supply line. It was initially written in Python for easy design visualization. It is, however, totally executable on a C++ Teensy 4.0 (no exclusive NumPy or other non-portable methods used) as it will be implemented in the final coffee maker. Feel free to download and try out the sizing a new tank with the code snippets I have provided here to see how the design process works!

### Description of Coffee Maker’s Core Functions:

#### 1. Based on the weight of beans ground, a total water need is determined by the machine (this satisfies requirement regarding coffee to water ratio)

#### 2. Next the machine will determine the fractions of water to be delivered to the filter at each of the 4 stages of coffee filtering. Please see Blue Bottle tips (https://bluebottlecoffee.com/preparation-guides/pour-over).

#### 3. The next step solves for the required water column height (head pressure potential) in the hot water tank such that the desired water volume will flow to the coffee filter within the time frame specified in the above instructions.

   ###### Note: several design constraints exist at this point as the heating element must be completely submerged while the water added is kept to a minimum in order to ensure complete heating to the desired brew temperature (198°F as measured by type k thermocouple) while the coffee filters during the rest period. In addition this must be weighed with how much head pressure will be developed to flow the desired water volume. No other flow control valve exists (except for a binary on/off solenoid). This last bit has certainly made my life more difficult.

#### 4. In order to determine the required water column height in the hot water tank, an iterative solver will compute (theoretically) and converge to the time required to flow the desired water volume. This is done over discrete time steps as each time step solution requires convergence due to the underlying friction and shape factor equations' dependence on Reynolds number (the previous time step's solution is used to initialize current time step and to speed up convergence). The head friction losses are of course dependent on Reynolds number, and hence flow rate, and must converge with each time step to produce and smooth and stable flow rate (see notes on challenges at end for why this is important). The result of this is integrated and a final “pour solution” including total flow volume and time is produced (shown below).

![Head-Height](/instantaneous_head_height.png)

![Pour-Total](/instantaneous_pour_total.png)

#### Residuals are shown for a converged time step within the final solution.

![Residuals](/residual.png)

### Convergence Inconvenience:

#### Convergence of the friction losses in the tubing posed the largest difficulty in getting a working (convergent) code as it would often diverge at certain critical flow rates. Previously a digitally converted PDF of the Moody diagram was used to tabulate and interpolate friction factor coefficients at each Reynolds number for the relative pipe roughness of silicone tubing. This posed issues in the circled transition region where large jumps in friction factor would prevent code from converging within a discrete time step solution and in fact prompted several flow rates to diverge when they encountered the transitional pipe flow region.

![Moody-Diagram](/moody_diagram.jpg)

###### source: https://www.researchgate.net/figure/Moody-diagram-for-the-determination-of-flow-regimes-with-regard-to-internal-friction_fig1_235735436

This was overcome by using a continuous Darcy-Weisbach equation (Cheng, 2008) friction factor calculation between the laminar and turbulent Reynolds numbers.

![Darcy-Weisbach-Formula](/DarcyWeisbachEqn_main.png)

#### where:

![Darcy-Weisbach-Coef-a](/DarcyWeisbachCoef_a.png)  

![Darcy-Weisbach-Coef-b](/DarcyWeisbachCoef_b.png)

###### source: https://en.wikipedia.org/wiki/Darcy_friction_factor_formulae


