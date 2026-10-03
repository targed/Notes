# Lecture 9: Applications of Electric Potential, Capacitors, and Dielectrics
## 1. Application of Electric Potential: Electrocardiogram (ECG/EKG)
  
  *   **Principle:** An electrocardiogram measures the electrical activity of the heart. The heart's contractions are triggered by electrical signals, and these signals create potential differences on the surface of the body.
  *   **Measurement:** Electrodes placed on the skin detect these small potential differences. The ECG records the time variation of these potentials, providing information about the heart's rhythm and health. Different parts of the ECG waveform correspond to different phases of the heart's cycle (atrial depolarization, ventricular depolarization, ventricular repolarization).
  *   **Clinical Use:** ECGs are used to diagnose a wide range of heart conditions, including arrhythmias (irregular heartbeats), heart attacks, and other cardiac problems.
## 2. Capacitors
  
  *   **Definition:** A capacitor is a device that stores electrical energy by accumulating electric charge on two conductive surfaces (usually plates) separated by an insulating material (a dielectric).
  *   **Basic Operation:** When a voltage is applied across the capacitor, charge accumulates on the plates – positive charge on one plate and an equal amount of negative charge on the other.
  * **Symbol**: The circuit symbol uses two parallel lines, indicating the parallel plates in a capacitor.
## Capacitance (C)
  
  *   **Definition:** Capacitance (C) is a measure of a capacitor's ability to store charge for a given voltage. It is defined as the ratio of the magnitude of the charge (Q) on either conductor to the magnitude of the potential difference (ΔV) between the conductors:
  
    $C = \frac{Q}{|\Delta V|}$
  
  *   **Units:** The SI unit of capacitance is the Farad (F), where 1 Farad = 1 Coulomb/Volt (1 F = 1 C/V).  The Farad is a very large unit; typical capacitors have capacitances in the microfarad (µF = 10<sup>-6</sup> F), nanofarad (nF = 10<sup>-9</sup> F), or picofarad (pF = 10<sup>-12</sup> F) range.
  
  *   **Parallel Plate Capacitor (Review):** For a parallel plate capacitor with plate area *A* and separation *d*, and vacuum/air between plates:
    $E = \frac{\sigma}{\epsilon_0} = \frac{Q}{\epsilon_0 A}$
  
  And the potential difference is
    $|\Delta V| = Ed = \frac{Qd}{\epsilon_0 A}$
    Therefore,
    $C = \frac{Q}{|\Delta V|} = \frac{\epsilon_0 A}{d}$
  *   **Capacitance Depends Only on Geometry:** The capacitance of a parallel plate capacitor depends *only* on the area of the plates (A) and the distance between them (d), and the permittivity of free space if there is no dielectric.  It does *not* depend on the charge or the voltage.
  
  *   **Example:**  A demonstration capacitor has a diameter of 30 cm (radius = 15 cm = 0.15 m) and a plate separation of 2 mm (0.002 m). Calculate its capacitance.
  
    $A = \pi r^2 = \pi (0.15 \text{ m})^2 \approx 0.0707 \text{ m}^2$
  
    $C = \frac{\epsilon_0 A}{d} = \frac{(8.85 \times 10^{-12} \text{ C}^2/\text{N}\cdot\text{m}^2)(0.0707 \text{ m}^2)}{0.002 \text{ m}} \approx 3.13 \times 10^{-10} \text{ F} = 313 \text{ pF}$
  
  *   **Key Point:** Changing the voltage applied to a capacitor *does not* change its capacitance. Capacitance is a *physical property* of the capacitor itself. Capacitance is often printed on the physical device.
## Effect of Plate Separation
  
  *   **Demonstration (Conceptual):** If the plate separation of a charged capacitor is increased (while keeping it isolated so the charge Q remains constant), the potential difference (ΔV) *increases*.
    *   Since C = Q/|ΔV|, and Q is constant, if |ΔV| increases, C *decreases*.
    *   This is consistent with  $C = ε_0A/d$.  Increasing *d* decreases *C*.
## Dielectrics
  
  *   **Definition:** A dielectric is an insulating material placed between the plates of a capacitor.
  *   **Effect:** Inserting a dielectric *increases* the capacitance of the capacitor.
  *   **Demonstration (Conceptual):** If a Teflon sheet is inserted between the plates of a fully charged capacitor (connected to an electroscope to measure potential difference), the deflection of the electroscope *decreases*. This indicates that the potential difference (ΔV) *decreases*.
    *   Since C = Q/|ΔV|, and Q is constant (the capacitor is isolated), if |ΔV| decreases, C *increases*.
  
  *   **Dielectric Constant (κ):** The dielectric constant (κ, also sometimes represented by *K*) is a dimensionless quantity that represents the factor by which the capacitance increases when a dielectric is inserted.  κ is always greater than 1.
    *   κ = 1 for vacuum (and approximately 1 for air)
    *   κ = 2 for Teflon
    *   κ = 80 for water
  
  *   **Capacitance with Dielectric:** The capacitance of a parallel plate capacitor with a dielectric is:
  
    $C = \kappa \frac{\epsilon_0 A}{d} = \kappa C_0$
  
    Where C_0 is the capacitance without the dielectric (i.e., with vacuum or air).
  
  *   **Molecular Explanation:**  The dielectric material becomes *polarized* in the electric field. The molecules within the dielectric align themselves with the field, creating an internal electric field that *opposes* the external field.  This reduces the net electric field between the plates, allowing more charge to be stored for a given voltage.
## Energy Stored in a Capacitor
  
  *   **Work Done Charging:**  Work must be done to transfer charge from one plate of a capacitor to the other. This work is stored as electric potential energy in the capacitor.
  *   **Derivation:**
  Let q be the charge during some moment of the transfer process, and $\Delta V$ be the potential difference.
  Then $\Delta V = \frac{q}{C}$
  The work required to add a charge $dq$ is:
  $dW = \Delta V dq = \frac{q}{C}dq$
  The total work, is the sum of the work for all the small changes in charge:
  $W = \int_0^Q \frac{q}{C}dq = \frac{1}{C} \int_0^Q qdq = \frac{1}{C}[\frac{1}{2}q^2]_0^Q = \frac{1}{2}\frac{Q^2}{C}$
  *   **Energy Formulas:** The energy (U) stored in a capacitor can be expressed in several equivalent forms:
  
    $U = \frac{1}{2} \frac{Q^2}{C} = \frac{1}{2} Q |\Delta V| = \frac{1}{2} C |\Delta V|^2$
  * **Video Example**: A charged capacitor is discharged through a light bulb, showing the stored energy as light and heat.
  
  *   **Example:** Calculate the energy stored in a 22,000 µF capacitor charged to 64 V.
  
    $U = \frac{1}{2} C |\Delta V|^2 = \frac{1}{2} (22,000 \times 10^{-6} \text{ F})(64 \text{ V})^2 \approx 45.1 \text{ J}$
## Advantages of Capacitors
  
  *   **Fast Energy Release:** Capacitors can release their stored energy very quickly, much faster than batteries.
  *   **Applications:**
    *   **Camera Flash:**  A capacitor stores energy slowly from a battery and then releases it rapidly to power the flash.
    *   **Defibrillator:**  A capacitor stores a large amount of energy and delivers a brief, high-voltage shock to the heart to restore a normal rhythm.  A battery stores more energy, but it's slower to be retrieved.
## Summary
  
  This lecture covered the application of electric potential to electrocardiograms. It defined capacitance and explored the factors that affect it, including the presence of dielectrics.  Finally, it derived and applied formulas for the energy stored in a capacitor, highlighting the advantages of capacitors for rapid energy release and applications like camera flashes and defibrillators.