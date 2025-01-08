## <span style="color:rgb(0, 176, 240)"><span style="color:rgb(0, 176, 240)">Abstract</span></span>

This document outlines the systematic approach for converting in vitro enzyme kinetic parameters to in vivo values, specifically accounting for enzyme auto-inhibition in GastroPlus simulations. The process involves scaling up in vitro experimental Vmax, auto-inhibition adjustments of Km, and refinements of each involving enzyme's clearance contribution .

## 1.  Calculation of Auto-Inhibition Factor and Modified Km,app

A substrate's auto-inhibition indicates that itself acts as both a substrate as well as an inhibitor of an enzyme. In our model, we only consider the mechanism of substrate itself competitively inhibit the enzyme. Thus, the equation of competitive inhibition effect on Km could be utilized to our auto-inhibition model:

![Pasted image 20241113002510](https://github.com/user-attachments/assets/2c29e4d2-b92a-42ae-9929-305f120a6859)

Notably, the inhibitor is the substrate itself. The observed Cmax value was used as the concentration of the inhibitor, for worst-case scenario predictions. Through the following formula, auto-inhibition factor and adjusted Km can be calculated.

![Pasted image 20241113133154](https://github.com/user-attachments/assets/73bcdda4-c103-4a7f-8424-a7f62d5751ff)


## 2.  Converting the In vitro Vmax to In Vivo Vmax by Multiplying the Scaling Factor

Use relative enzyme expression levels to scale up to whole organ levels:
![Pasted image 20241112232720](https://github.com/user-attachments/assets/46a0d44f-ff47-455b-92e5-377b828530e9)

The liver weight is typically about **2.25% of total body weight**. Microsomal Protein per Gram of Liver (MPPGL) is typically 38 mg/g liver. Using the equation above, the total microsomal protein (mg) can be calculated. 

The common unit of Vmax in vitro is pmol/min per mg microsomal protein, the following equation allows us to calculate the the Vmax in vivo, based on the total amount of microsomal protein contained in the liver on individual's level.
![Pasted image 20241112233852](https://github.com/user-attachments/assets/04f33085-ca1a-4d25-aaef-d897638ea764)


## 3. Calculating the Simulated (Observed) Clint

By dividing the **Vmax** values obtained from the previous step by the corresponding **Km** values provided in the literature (using the formula **Clint = Vmax / Km**), we calculate the intrinsic clearance (**Clint**) for each enzyme. Summing these values gives us the **total Clint**. We refer to these as "simulated" or "observed" Clint values because we use them as constants in our modeling process, aligning them with the Clint values used in **GastroPlus**.

This alignment ensures consistency between our calculations and the GastroPlus model. By matching our calculated Clint values with those in GastroPlus, we can back-calculate and determine the actual **Vmax** values that users should input into GastroPlus for accurate pharmacokinetic simulations.

![Pasted image 20241112234416](https://github.com/user-attachments/assets/7a3593e9-9315-4e1c-b89c-b3c3cedbd7fa)


## 4. Assign total Clint to Each Participating Enzyme's Own Clint

![Pasted image 20241113011838](https://github.com/user-attachments/assets/974d6fac-95d4-4bc6-9fa2-3a496e83ed42)

### How the equation is built:

**Enzyme Kinetics Principles**:

- Lower Km = Higher affinity = More contribution
- Higher Km = Lower affinity = Less contribution

The apparent Km (Km,app) has an inverse relationship with enzyme contribution - as Km,app increases, the enzyme's contribution decreases. Therefore, Km,app is placed in the denominator of our equation. To calculate a relative contribution factor that considers each enzyme's role within the entire metabolic system, we place the sum of all participating enzymes' Km,app values in the numerator. This ratio (ΣKm,app / Km,app) effectively captures both the individual enzyme's affinity and its relative importance in the overall metabolic pathway. Log is employed to smooth out large differences. 

### Identify refined percentage of contribution:

The percentage of contribution of each enzyme in the metabolism process provided in literatures, multiplies by our derived contribution factor, we now can obtain the refined percentage contribution:

![Pasted image 20241113020125](https://github.com/user-attachments/assets/cd16ee42-5eb4-4518-a37a-e669d07f9736)

To ensure all contributions sum to 100% and preserve relative proportions after contribution factor refinement, the following normalization need to be done:

![Pasted image 20241113020853](https://github.com/user-attachments/assets/e62db57e-eca6-48e3-b523-e1d8581cbb46)

### Calculate in vivo Clint and in vivo Vmax:

Previously, we have obtained the observed/simulated Clint value for each participating enzyme, as well as the total Clint. Now, by multiplying the observed total Clint with each enzyme percentage of contribution, we can finally identify the in vivo Clint value of each enzyme.

![Pasted image 20241113130547](https://github.com/user-attachments/assets/f43e6bc4-18fd-416f-ad38-1285cb8df24c)

Finally, multiplying the in vivo Clint of each enzyme by apparent Km, we can derive the in vivo Vmax as the input of GastroPlus. 

![Pasted image 20241113131933](https://github.com/user-attachments/assets/0c8db7b9-6eca-4fa9-8f85-956e741c740e)
