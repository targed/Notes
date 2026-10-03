## Introduction
  
  This lecture introduces the concepts of heat, temperature, and heat transfer.  We will cover:
  
  * 0th Law of Thermodynamics
  * Heat and temperature change
  * Heat transfer mechanisms
## Zero-th Law of Thermodynamics
  
  * **Thermal contact:**  Two objects are in thermal contact if they can exchange energy with each other due to a temperature difference.
  * **Thermal equilibrium:** When two objects in thermal contact (and isolated from their surroundings) reach the same temperature, they are in thermal equilibrium. No further net energy transfer occurs.
  * **Zero-th Law:** If two objects are in thermal equilibrium with a third object, they are in thermal equilibrium with each other.
  * **Basis for temperature measurement:** The zero-th law allows us to define temperature and use a thermometer to measure the temperature of an object.
## Energy Transfer
  
  * **Heat:** When two objects at different temperatures are in contact, energy is transferred between them. This energy transfer due solely to a temperature difference is called **heat**.
  * **Direction of heat flow:** Heat always flows from the hotter object to the colder object.
  * **Temperature change:**  When heat is added to an object, its temperature typically increases. When heat is removed, its temperature typically decreases.
  * **Specific heat:**  The amount of heat required to raise the temperature of 1 kg of a substance by 1 degree Celsius (or 1 Kelvin).  It is a property of the material.
  * **Formula:** $\Delta T = \frac{Q}{mc_{substance}}$ (More about this in next slide)
## Heat and Temperature Change
  
  * **Heat ($Q$):** The amount of heat energy transferred to or from an object.
  * **Formula:** $Q = mc\Delta T$
    * $m$: Mass of the object
    * $c$: Specific heat of the substance
    * $\Delta T$: Change in temperature
  * **Sign convention:**
    * $Q > 0$: Heat flows *into* the system (temperature increases).
    * $Q < 0$: Heat flows *out of* the system (temperature decreases).
## Heat in Phase Changes
  
  * **Phase changes (phase transitions):**  Transitions between solid, liquid, and gas phases.
  * **Latent heat:** Phase changes require or release a certain amount of heat even though the temperature remains constant during the transition.
  * **Latent heat of fusion ($L_f$):**  Heat required to melt 1 kg of a solid at its melting point.
  * **Latent heat of vaporization ($L_v$):** Heat required to vaporize 1 kg of a liquid at its boiling point.
  * **Formulas:**
    * Solid to liquid: $Q = +mL_f$ (heat absorbed)
    * Liquid to solid: $Q = -mL_f$ (heat released)
    * Liquid to gas: $Q = +mL_v$ (heat absorbed)
    * Gas to liquid: $Q = -mL_v$ (heat released)
## Example 1: Mixing Tea and Cup
  
  **Problem:** 300 g of tea at 70°C are poured into a 120 g cup made of aluminum that is at a temperature of 20°C. What is the final temperature?
  
  **(Solution will be discussed in the lecture. This involves using the equation $Q = mc\Delta T$ for both the tea and the cup, and the principle of conservation of energy (the heat lost by the tea equals the heat gained by the cup).  You'll need the specific heat values for tea (assume it's the same as water) and aluminum.)**
## Example 2: Ice and Water Mixture
  
  **Problem:** You have 0.25 kg of water at 25°C, and you are adding ice to it that has a temperature of -20°C. How much ice is needed so that the final temperature of the mixture is 0°C and all the ice is melted? Neglect the container.
  
  **(Solution will be discussed in the lecture. This is a multi-step problem involving heating the ice, melting the ice, and cooling the water.)**
## Three Mechanisms of Heat Transfer
  
  Heat can be transferred through three different mechanisms:
  
  * **Conduction:**  Heat transfer through direct contact between materials.
  * **Convection:** Heat transfer through the movement of fluids (liquids or gases).
  * **Radiation:** Heat transfer through electromagnetic waves.
## Heat Conduction
  
  * **Steady state:** A situation where the temperature at each point in the material remains constant over time.
  * **Rate of heat flow ($H$):**  In steady state, the rate of heat energy flow through an area $A$ is given by:
    * $H = \frac{dQ}{dt} = kA \frac{T_{hot} - T_{cold}}{L}$
    * $k$: Thermal conductivity, a material property. Higher $k$ means the material conducts heat more readily.
    * $A$: Cross-sectional area
    * $L$: Length or thickness of the material
    * $T_{hot}$ and $T_{cold}$: Temperatures at the ends of the material.
  
  **(Diagram illustrating heat conduction through a rod)**
## Material Boundaries
  
  * **Multiple layers:** Consider heat conduction through layers of different materials with different thermal conductivities ($k_n$) and thicknesses ($L_n$).
  * **Heat flow:** The rate of heat flow ($H$) is the same through each layer in steady state.
  * **Temperature at the boundary:** The temperature at the boundary between two materials is the same for both materials.
  * **Thermal resistance ($R$):** Analogous to electrical resistance, thermal resistance can be defined for each layer as $R_n = \frac{L_n}{k_n A}$.
  * **Total thermal resistance:** For multiple layers, the total thermal resistance is the sum of the individual resistances: $R_{tot} = \sum_n R_n$.
  * **Heat flow in terms of thermal resistance:** $H = \frac{A(T_{hot} - T_{cold})}{R_{tot}}$
  
  **(Diagram of heat conduction through multiple layers of materials)**
## Example: Composite Rod
  
  **Problem:** A rod of uniform cross-section has its left end placed in water at 10°C and its right end at 40°C. The left half of the rod consists of material A with thermal conductivity 400 W/m°C, and the right half of material B with thermal conductivity 200 W/m°C. What is the temperature in the middle of the rod?
  
  **(Diagram of the rod)**
  
  **(Solution will be discussed in lecture, using the concept of steady-state heat flow and thermal resistance.)**