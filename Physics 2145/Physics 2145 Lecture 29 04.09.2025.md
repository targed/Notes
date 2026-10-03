# Lecture 29: Faraday's Law
  
  This lecture revisits and applies Faraday's Law of Induction and Lenz's Law to understand induced emf and current, and explores several practical applications.
## Review: Lenz's Law
  
  Lenz's Law provides the direction of the induced current in a conducting loop when the magnetic flux through that loop changes.
  
  *   **Statement:** The direction of the induced current is such that the magnetic field created by the induced current **opposes** the **change** in magnetic flux that caused it.
  *   **How Flux Changes:** The magnetic flux ($\Phi_B = BA\cos\theta$) can change if:
    1.  The magnetic field strength **B changes** (increases or decreases).
    2.  The **area A** of the loop changes (expands or contracts).
    3.  The **angle θ** between the field and the normal to the loop changes (loop rotates).
    4.  The loop moves into or out of a region with a magnetic field.
  
  *   **Applying Lenz's Law (Reminder):**
    1.  Find the direction of the original magnetic field ($\vec{B}_{orig}$).
    2.  Determine if the flux ($\Phi_B$) is increasing or decreasing.
    3.  Determine the direction of the induced magnetic field ($\vec{B}_{ind}$) needed to oppose the change (opposite to $\vec{B}_{orig}$ if flux increases, same direction as $\vec{B}_{orig}$ if flux decreases).
    4.  Use the Right-Hand Rule (curl fingers in current direction, thumb points in $\vec{B}_{ind}$ direction) to find the direction of the induced current ($I_{ind}$).
## Review: Faraday's Law of Induction
  
  Faraday's Law quantifies the magnitude of the induced emf.
  
  *   **Statement:** An emf is induced in a conducting loop if the magnetic flux through the loop changes. The magnitude of the induced emf is equal to the rate of change of the magnetic flux through the loop.
  *   **Formula (Magnitude - Average):** If the flux changes by ΔΦ<sub>B</sub> during a time interval Δt, the average induced emf is:
  
    $|ℰ| = |\frac{\Delta \Phi_B}{\Delta t}|$
  
  *   **Formula (Magnitude - Instantaneous):**
  
    $|ℰ| = |\frac{d \Phi_B}{dt}|$
  
  *   **For a Coil with N Turns:** If the coil consists of N identical turns, the total induced emf is N times the emf induced in a single turn:
  
    $|ℰ| = N |\frac{d \Phi_B}{dt}|$
  
  *   **Combining with Lenz's Law (Sign):** Faraday's Law is often written with a negative sign to incorporate Lenz's Law, indicating the direction of the emf (which way it drives current):
  
    $ℰ = -N \frac{d \Phi_B}{dt}$
  
    The negative sign signifies that the induced emf opposes the change in flux.
## Examples (Conceptual, based on slide image)
  
  The slide shows several scenarios applying Lenz's Law:
  
  1.  **Scenario 1 (e.g., North pole moving towards loop):**
    *   $\vec{B}_{orig}$ points through the loop (e.g., to the right).
    *   Flux is increasing (magnet moving closer).
    *   $\vec{B}_{ind}$ must oppose the change, so it points opposite to $\vec{B}_{orig}$ (e.g., to the left).
    *   Using RHR, the induced current flows in the direction that produces $\vec{B}_{ind}$.
  2.  **Scenario 2 (e.g., North pole moving away from loop):**
    *   $\vec{B}_{orig}$ points through the loop (e.g., to the right).
    *   Flux is decreasing (magnet moving away).
    *   $\vec{B}_{ind}$ must reinforce the original field, so it points in the same direction as $\vec{B}_{orig}$ (e.g., to the right).
    *   Using RHR, the induced current flows in the direction that produces $\vec{B}_{ind}$.
  3.  **Scenario 3 (e.g., Loop area decreasing in constant field):**
    *   Assume $\vec{B}_{orig}$ points into the page.
    *   Area A is decreasing, so flux $\Phi_B = BA\cos\theta$ is decreasing.
    *   $\vec{B}_{ind}$ must reinforce $\vec{B}_{orig}$, so it also points into the page.
    *   Using RHR (thumb into page), the induced current flows clockwise.
  4.  **Scenario 4 (e.g., Loop rotating):** The direction depends on whether the flux is increasing or decreasing at that specific moment of rotation. If $\theta$ is changing such that $\cos\theta$ is increasing, flux increases; if $\cos\theta$ is decreasing, flux decreases.
## Application: Electric Generator
  
  *   **Principle:** Converts mechanical energy into electrical energy using electromagnetic induction.
  *   **Mechanism:**
    *   A coil of wire (N turns, area A) is rotated (by mechanical means, e.g., a turbine) in a uniform magnetic field B.
    *   As the coil rotates, the angle θ between the magnetic field and the normal to the coil changes.
    *   The magnetic flux through the coil changes: $\Phi_B = BA\cos\theta$. If the coil rotates with angular velocity ω, then $\theta = \omega t$. So, $\Phi_B = BA\cos(\omega t)$.
    *   According to Faraday's Law, an emf is induced:
        $ℰ = -N \frac{d \Phi_B}{dt} = -N \frac{d}{dt}(BA\cos(\omega t))$
        $ℰ = -N BA (-\sin(\omega t) \cdot \omega) = NBA\omega \sin(\omega t)$
  *   **Output:** The induced emf is sinusoidal (alternating voltage), which drives an alternating current (AC) if connected to a circuit. The maximum emf is $ℰ_{max} = NBA\omega$.
## Application: Eddy Currents
  
  *   **Definition:** Induced currents that circulate within the bulk of a solid conductor when it is exposed to a changing magnetic flux.
  *   **Cause:** When a conductor moves through a non-uniform magnetic field, or when the magnetic field through a stationary conductor changes, different parts of the conductor experience a changing flux. This induces circulating currents (eddies) within the material itself according to Faraday's and Lenz's Laws.
  *   **Effects:**
    *   **Heating:** Eddy currents dissipate energy as heat (I<sup>2</sup>R loss) within the conductor due to its resistance. This is used in induction heating.
    *   **Magnetic Braking:** According to Lenz's Law, eddy currents create magnetic fields that oppose the change in flux causing them. This results in a braking force that opposes the motion of the conductor relative to the magnetic field. This is used in brakes for trains or roller coasters.
    *   Can be undesirable in transformers and motors, leading to energy loss. Laminated cores are used to reduce eddy currents in these devices.
## Application: Transcranial Magnetic Stimulation (TMS)
  
  *   **Principle:** A non-invasive technique used to stimulate or inhibit specific regions of the brain.
  *   **Mechanism:**
    *   A coil (often shaped like a figure-eight) is placed near the scalp.
    *   A brief, strong pulse of current is passed through the coil.
    *   This rapidly changing current creates a rapidly changing magnetic field that penetrates the scalp and skull.
    *   According to Faraday's Law, the changing magnetic field induces weak electric currents (eddy currents) in the conductive brain tissue beneath the coil.
    *   These induced currents can depolarize neurons in the targeted brain region, triggering or modulating neural activity.
  *   **Uses:** Research tool to study brain function, potential therapeutic applications for depression, stroke rehabilitation, and other neurological/psychiatric conditions.
## Summary
  
  Faraday's Law ($ℰ = -N d\Phi_B/dt$) and Lenz's Law are fundamental principles describing how a changing magnetic flux induces an emf and current in a conductor. These principles explain the operation of electric generators, the formation and effects of eddy currents (used in braking and heating, but also a source of energy loss), and modern neuroscience techniques like Transcranial Magnetic Stimulation (TMS).