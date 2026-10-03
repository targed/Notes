# Lecture 28: Electromagnetic Induction
  
  This lecture explores the phenomenon where a changing magnetic environment can create, or *induce*, an electromotive force (emf) and a current in a conductor.
## Induced Current
  
  *   **Fundamental Principle:** If the magnetic field passing through a loop or coil of wire changes over time, an electromotive force (emf) is generated, and if the loop forms a closed circuit, a current is *induced* in the coil.
  *   **Key Idea:** It's the *change* in the magnetic field (or the magnetic environment) that matters, not just the presence of a field.
## Motional EMF
  
  *   **Setup:** Consider a straight conductor (e.g., a metal rod) of length *L* moving with a constant velocity $\vec{v}$ perpendicular to a uniform magnetic field $\vec{B}$. Assume $\vec{v}$ is perpendicular to the length of the rod.
  *   **Force on Charges:** The charge carriers (usually electrons) within the conductor are moving along with the rod. Since they are charges moving in a magnetic field, they experience a magnetic force:
    $\vec{F}_B = q(\vec{v} \times \vec{B})$
    The magnitude is $F_B = qvB$ (since $\vec{v} \perp \vec{B}$).
  *   **Charge Separation:** Using the Right-Hand Rule, positive charges would be pushed towards one end of the rod, and negative charges towards the other. This separation of charge continues until the electric field ($\vec{E}$) created by the separated charges exerts an electric force ($F_E = qE$) that exactly balances the magnetic force.
  *   **Equilibrium:** $F_E = F_B \implies qE = qvB \implies E = vB$
  *   **Induced EMF (Potential Difference):** The electric field creates a potential difference across the ends of the rod. This potential difference is the *motional emf* (ℰ):
    $ℰ = \Delta V = EL = (vB)L$
  
    $ℰ = vLB$
  
    This potential difference exists whether or not the rod is part of a complete circuit. It acts like a battery created by motion in a magnetic field.
## Induced Current in a Circuit
  
  *   **Setup:** Now consider the moving conductor sliding on stationary conducting rails that form a closed circuit with a resistor *R*.
  *   **Current Flow:** The motional emf (ℰ = vLB) generated in the moving rod acts as the voltage source for the circuit. According to Ohm's Law, an induced current (I) will flow through the circuit:
  
    $I = \frac{ℰ}{R} = \frac{vLB}{R}$
  
  *   **Direction:** The direction of the induced current can be determined by considering the direction of the motional emf (positive charges pushed to one end) or later by Lenz's Law.
## Force on the Current-Carrying Rod
  
  *   **Back Force:** Since the moving rod now carries an induced current *I* and is still in the magnetic field *B*, it experiences a magnetic force given by:
    $\vec{F}_{wire} = I(\vec{L} \times \vec{B})$
  *   **Magnitude:** $F_{wire} = ILB$ (since $\vec{L} \perp \vec{B}$)
  *   **Direction:** Using the Right-Hand Rule for the force on a current, this force opposes the motion of the rod. To keep the rod moving at a constant velocity $\vec{v}$, an external agent must apply an opposing force ($F_{app} = F_{wire}$) in the direction of motion.
## Energy Conservation
  
  *   **Work Done:** The external agent applying the force $F_{app}$ does work at a rate (Power):
    $P_{app} = F_{app}v = (ILB)v$
  *   **Substituting Induced Current:**
    $P_{app} = (\frac{vLB}{R})LBv = \frac{(vLB)^2}{R}$
  *   **Power Dissipated:** The electrical power dissipated as heat in the resistor is:
    $P_{dissipated} = I^2R = (\frac{vLB}{R})^2 R = \frac{(vLB)^2}{R}$
  *   **Conclusion:** $P_{app} = P_{dissipated}$. The rate at which the external agent does work to move the rod is exactly equal to the rate at which energy is dissipated as heat in the resistor. Energy is conserved. The mechanical energy input is converted into electrical energy, which is then dissipated as thermal energy.
## Magnetic Flux ($\Phi_B$)
  
  To generalize beyond motional emf, we need the concept of magnetic flux.
  
  *   **Analogy:** Similar to electric flux, magnetic flux measures the "amount" of magnetic field passing through a given surface area.
  *   **Definition:** For a uniform magnetic field $\vec{B}$ passing through a flat area *A*, the magnetic flux is:
  
    $\Phi_B = B A \cos\theta$
  
    Where:
    *   $\Phi_B$ is the magnetic flux.
    *   $B$ is the magnitude of the magnetic field.
    *   $A$ is the area of the surface.
    *   $\theta$ is the angle between the **magnetic field vector ($\vec{B}$)** and the **normal vector ($\hat{n}$)** to the surface area.
  
  *   **Special Cases:**
    *   Maximum Flux: When $\vec{B}$ is perpendicular to the surface ($\theta = 0^\circ$), $\Phi_B = BA$.
    *   Zero Flux: When $\vec{B}$ is parallel to the surface ($\theta = 90^\circ$), $\Phi_B = 0$.
  *   **Units:** The SI unit of magnetic flux is the Weber (Wb).
    *   1 Weber = 1 Tesla-meter<sup>2</sup> (1 Wb = 1 T⋅m<sup>2</sup>)
## Faraday's Law of Induction
  
  This is the general law governing induced emf.
  
  *   **Statement:** The magnitude of the induced emf (ℰ) in a closed loop is equal to the rate of change of the magnetic flux ($\Phi_B$) through the loop.
  *   **Formula (Magnitude):**
  
    $|ℰ| = |\frac{\Delta \Phi_B}{\Delta t}|$ (for average emf over time interval Δt)
  
    $|ℰ| = |\frac{d \Phi_B}{dt}|$ (for instantaneous emf)
  
  *   **For a Coil with N Turns:** If the coil has N identical turns, the emf is multiplied by N:
  
    $|ℰ| = N |\frac{d \Phi_B}{dt}|$
  
  *   **Ways to Change Flux (and induce emf):**
    1.  Change the **magnetic field strength (B)**.
    2.  Change the **area (A)** of the loop.
    3.  Change the **angle (θ)** between the field and the normal to the loop (i.e., rotate the loop).
    4.  Move the loop into or out of a magnetic field region.
## Lenz's Law (Determining Direction)
  
  Faraday's Law gives the magnitude of the induced emf, while Lenz's Law determines the direction of the induced current (and thus the polarity of the induced emf).
  
  *   **Statement:** The direction of the induced current in a loop is such that the magnetic field created by the induced current **opposes** the **change** in magnetic flux that caused it.
  *   **"Opposing the Change":**
    *   **If the flux is increasing:** The induced current creates a magnetic field pointing in the *opposite* direction to the original field to try and counteract the increase.
    *   **If the flux is decreasing:** The induced current creates a magnetic field pointing in the *same* direction as the original field to try and replenish the decreasing flux.
  
  *   **Procedure using Lenz's Law:**
    1.  **Determine the direction of the original magnetic field ($\vec{B}_{orig}$) through the loop.**
    2.  **Determine if the magnetic flux ($\Phi_B$) through the loop is increasing, decreasing, or constant.**
    3.  **Determine the direction of the induced magnetic field ($\vec{B}_{ind}$) needed to oppose the change:**
        *   If flux is increasing, $\vec{B}_{ind}$ opposes $\vec{B}_{orig}$.
        *   If flux is decreasing, $\vec{B}_{ind}$ is in the same direction as $\vec{B}_{orig}$.
        *   If flux is constant, there is no induced field (no induced current).
    4.  **Use the Right-Hand Rule (for loops):** Point your thumb in the direction of the required $\vec{B}_{ind}$. Your fingers will curl in the direction of the **induced current (I<sub>ind</sub>)**.
## Examples (Conceptual, based on typical slide diagrams)
  
  *   **Magnet moving towards a loop:**
    *   Original field (e.g., North pole approaching) points through the loop and is *increasing*.
    *   Induced field must oppose the original field (point away from the approaching N pole).
    *   Use RHR to find the current direction that creates this opposing field.
  *   **Magnet moving away from a loop:**
    *   Original field points through the loop and is *decreasing*.
    *   Induced field must be in the *same* direction as the original field to try and maintain it.
    *   Use RHR to find the current direction.
  *   **Loop changing area in a constant field:**
    *   If area increases, flux increases. Induced B opposes original B.
    *   If area decreases, flux decreases. Induced B reinforces original B.
  *   **Loop rotating in a constant field:**
    *   The angle θ changes, causing the flux $\Phi_B = BA\cos\theta$ to change. Apply Lenz's law based on whether the magnitude of the flux is increasing or decreasing at that instant.
## Summary
  
  Electromagnetic induction is the production of an emf (and possibly current) due to a changing magnetic flux. Motional emf ($ℰ = vLB$) is a specific case where a conductor moves through a magnetic field. Magnetic flux ($\Phi_B = BA\cos\theta$) quantifies the field passing through an area. Faraday's Law ($|ℰ| = |d\Phi_B/dt|$) gives the magnitude of the induced emf, while Lenz's Law determines the direction of the induced current by stating that the induced magnetic field opposes the change in flux.