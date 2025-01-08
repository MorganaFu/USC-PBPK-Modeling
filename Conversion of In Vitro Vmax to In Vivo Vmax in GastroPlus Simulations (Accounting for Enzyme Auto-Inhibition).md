## <span style="color:rgb(0, 176, 240)"><span style="color:rgb(0, 176, 240)">Abstract</span></span>

This document outlines the systematic approach for converting in vitro enzyme kinetic parameters to in vivo values, specifically accounting for enzyme auto-inhibition in GastroPlus simulations. The process involves scaling up in vitro experimental Vmax, auto-inhibition adjustments of Km, and refinements of each involving enzyme's clearance contribution .

## 1.  Calculation of Auto-Inhibition Factor and Modified Km,app

A substrate's auto-inhibition indicates that itself acts as both a substrate as well as an inhibitor of an enzyme. In our model, we only consider the mechanism of substrate itself competitively inhibit the enzyme. Thus, the equation of competitive inhibition effect on Km could be utilized to our auto-inhibition model:
![[Pasted image 20241113002510.png]]

Notably, the inhibitor is the substrate itself. The observed Cmax value was used as the concentration of the inhibitor, for worst-case scenario predictions. Through the following formula, auto-inhibition factor and adjusted Km can be calculated.

![[Pasted image 20241113133154.png]]
## 2.  Converting the In vitro Vmax to In Vivo Vmax by Multiplying the Scaling Factor

Use relative enzyme expression levels to scale up to whole organ levels:
![[Pasted image 20241112232720.png]]

The liver weight is typically about **2.25% of total body weight**. Microsomal Protein per Gram of Liver (MPPGL) is typically 38 mg/g liver. Using the equation above, the total microsomal protein (mg) can be calculated. 

The common unit of Vmax in vitro is pmol/min per mg microsomal protein, the following equation allows us to calculate the the Vmax in vivo, based on the total amount of microsomal protein contained in the liver on individual's level.

![[Pasted image 20241112233852.png]]

## 3. Calculating the Simulated (Observed) Clint

By dividing the **Vmax** values obtained from the previous step by the corresponding **Km** values provided in the literature (using the formula **Clint = Vmax / Km**), we calculate the intrinsic clearance (**Clint**) for each enzyme. Summing these values gives us the **total Clint**. We refer to these as "simulated" or "observed" Clint values because we use them as constants in our modeling process, aligning them with the Clint values used in **GastroPlus**.

This alignment ensures consistency between our calculations and the GastroPlus model. By matching our calculated Clint values with those in GastroPlus, we can back-calculate and determine the actual **Vmax** values that users should input into GastroPlus for accurate pharmacokinetic simulations.

![[Pasted image 20241112234416.png]]

## 4. Assign total Clint to Each Participating Enzyme's Own Clint

![[Pasted image 20241113011838.png]]

### How the equation is built:

**Enzyme Kinetics Principles**:

- Lower Km = Higher affinity = More contribution
- Higher Km = Lower affinity = Less contribution

The apparent Km (Km,app) has an inverse relationship with enzyme contribution - as Km,app increases, the enzyme's contribution decreases. Therefore, Km,app is placed in the denominator of our equation. To calculate a relative contribution factor that considers each enzyme's role within the entire metabolic system, we place the sum of all participating enzymes' Km,app values in the numerator. This ratio (ΣKm,app / Km,app) effectively captures both the individual enzyme's affinity and its relative importance in the overall metabolic pathway. Log is employed to smooth out large differences. 

### Identify refined percentage of contribution:

The percentage of contribution of each enzyme in the metabolism process provided in literatures, multiplies by our derived contribution factor, we now can obtain the refined percentage contribution:

![[Pasted image 20241113020125.png]]

To ensure all contributions sum to 100% and preserve relative proportions after contribution factor refinement, the following normalization need to be done:

![[Pasted image 20241113020853.png]]

### Calculate in vivo Clint and in vivo Vmax:

Previously, we have obtained the observed/simulated Clint value for each participating enzyme, as well as the total Clint. Now, by multiplying the observed total Clint with each enzyme percentage of contribution, we can finally identify the in vivo Clint value of each enzyme.

![[Pasted image 20241113130547.png]]

Finally, multiplying the in vivo Clint of each enzyme by apparent Km, we can derive the in vivo Vmax as the input of GastroPlus. 

![[Pasted image 20241113131933.png]]