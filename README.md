# Coffee-Maker

# Everyone it would seem has been working on a quarantine project to keep them busy and sane these last months. Not to be left out, I am sharing a portion of what I have been working on.
# I have been interested in making a “pour over” coffee maker for several years now but didn’t want to just throw together some 3D printer looking contraption that would eat up all my precious one-bedroom apartment counter space. Instead I opted to go for the impractical, dream shot coffee maker that would (and this was non-negotiable since all my ex-barista friends would tell me so) make a cup of coffee at exactly the right temperature (measured at the filter), exactly the right bean-to-water ratio (measured to the nearest gram), exactly the right pour pattern (no sprinklers or concentric circles… had to be a spiral), and at exactly the right flow rate (measured to the nearest ml/s). Additionally, the water flow I determined must be laminar when it exited the pour spout just so that it all looked very nice.¶
# This was to my naive mind totally do-able. And to an extent it still is, but 1000 dollars and 100+ hrs. later (and still counting) I can say with certainty that it is now unlikely I will ever do better (financially and sanity-wise) than if I had just gone to my friend’s shop (difficult in quarantine) and bought that 5 dollar cup of coffee every day for the rest of my life. But here I am. Soon to have an excellent, hands-free coffee maker but still using my french press every morning...
# This portion is what I came up with to eliminate a flow meter (not enough room to package in the 2”x4” space available in the tower) from the hot water supply line. It was initially written in Python for easy design visualization. It is, however, totally executable on a C++ Teensy 4.0 (no exclusive NumPy or other non-portable methods used) as it will be implemented in the final coffee maker.
