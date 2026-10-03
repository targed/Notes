# Lecture 10: Capacitor Networks
  
  This lecture covers how to analyze circuits containing multiple capacitors connected in series and parallel.
## Capacitors in Parallel
  
  *   **Connection:** Capacitors are connected in parallel when their "top" plates are connected together by a wire and their "bottom" plates are connected together by another wire.  This means they share the same potential difference.
  
  *   **Equivalent Capacitance (C_eq):** The equivalent capacitance of capacitors in parallel is the *sum* of the individual capacitances:
  
    $C_{eq} = C_1 + C_2 + C_3 + ...$
  
  *   **Reasoning:**
    *   **Same Voltage:**  All capacitors in parallel have the same potential difference (ΔV) across them.
    *   **Total Charge:** The total charge stored on the equivalent capacitor (Q_eq) is the sum of the charges on the individual capacitors:
        $Q_{eq} = Q_1 + Q_2 + Q_3 + ...$
    *   **Derivation:**  Since Q = C|ΔV|, we have:
        $Q_{eq} = C_{eq}|\Delta V|$
        $Q_1 = C_1|\Delta V|$
        $Q_2 = C_2|\Delta V|$
        ...
        Substituting into the total charge equation:
        $C_{eq}|\Delta V| = C_1|\Delta V| + C_2|\Delta V| + ...$
        Dividing both sides by |ΔV| gives:
        $C_{eq} = C_1 + C_2 + ...$
  
  *   **Key Idea:**  Connecting capacitors in parallel effectively increases the total plate area, allowing more charge to be stored at the same voltage.
## Capacitors in Series
  
  *   **Connection:** Capacitors are connected in series when they are connected end-to-end, so the "bottom" plate of one capacitor is connected to the "top" plate of the next.  This means they share the same charge.
  
  *   **Equivalent Capacitance (C_eq):** The reciprocal of the equivalent capacitance of capacitors in series is the sum of the reciprocals of the individual capacitances:
  
    $\frac{1}{C_{eq}} = \frac{1}{C_1} + \frac{1}{C_2} + \frac{1}{C_3} + ...$
  
  *   **Reasoning:**
    *   **Same Charge:** All capacitors in series have the same magnitude of charge (Q) on their plates. This is because the charge on one plate of a capacitor induces an equal and opposite charge on the connected plate of the next capacitor in the series.
    *   **Total Voltage:** The total potential difference across the series combination (ΔV_eq) is the sum of the potential differences across the individual capacitors:
       $\Delta V_{eq} = \Delta V_1 + \Delta V_2 +  \Delta V_3 + ...$
  
    *   **Derivation:**  Since |ΔV| = Q/C, we have:
      $\Delta V_{eq} = \frac{Q}{C_{eq}}$
      $\Delta V_1 = \frac{Q}{C_1}$
       $\Delta V_2 = \frac{Q}{C_2}$
        ...
  
        Substituting into the total voltage equation:
       $\frac{Q}{C_{eq}} = \frac{Q}{C_1} + \frac{Q}{C_2} + ...$
       Dividing by Q:
       $\frac{1}{C_{eq}} = \frac{1}{C_1} + \frac{1}{C_2} + ...$
  
  *   **Key Idea:** Connecting capacitors in series effectively increases the total separation distance between the plates, reducing the overall capacitance.
## Example Problem (Ex. 23.10)
  
  Find the equivalent capacitances of the four capacitor combinations shown in the figure (not provided, but we can analyze combinations).
  
  **Given:**
  
  *   $C_1 = 2 µF$
  *   $C_2 = 4 µF$
  *   $C_3 = 8 µF$
  *   $C_4 = 16 µF$
  *    ΔV = 60 V (This voltage is relevant for calculating charge and energy, but not for finding equivalent capacitance).
  
  We need the "figure" to give the specific combinations, but we can solve some example combinations:
  
  **Example Combination 1: C_1 and C_2 in Parallel**
  
  $C_{eq} = C_1 + C_2 = 2 \mu F + 4 \mu F = 6 \mu F$
  
  **Example Combination 2: C_1 and C_2 in Series**
  
  $\frac{1}{C_{eq}} = \frac{1}{C_1} + \frac{1}{C_2} = \frac{1}{2 \mu F} + \frac{1}{4 \mu F} = \frac{2}{4 \mu F} + \frac{1}{4 \mu F} = \frac{3}{4 \mu F}$
  
  $C_{eq} = \frac{4}{3} \mu F \approx 1.33 \mu F$
  
  **Example Combination 3: C_1 and C_2 in Parallel, then in Series with C_3**
  
  1.  **Parallel Combination (C_12):**
    $C_{12} = C_1 + C_2 = 2 \mu F + 4 \mu F = 6 \mu F$
  
  2.  **Series Combination (C_123):**
    $\frac{1}{C_{123}} = \frac{1}{C_{12}} + \frac{1}{C_3} = \frac{1}{6 \mu F} + \frac{1}{8 \mu F} = \frac{4}{24 \mu F} + \frac{3}{24 \mu F} = \frac{7}{24 \mu F}$
  
    $C_{123} = \frac{24}{7} \mu F \approx 3.43 \mu F$
  
  **Example Combination 4: C_1 and C_2 in Series, then in Parallel with C_3**
  
  1. **Series Combination (C12):**
     $\frac{1}{C_{12}} = \frac{1}{C_1} + \frac{1}{C_2} = \frac{1}{2 \mu F} + \frac{1}{4 \mu F}  = \frac{3}{4 \mu F}$
    $C_{12} = \frac{4}{3} \mu F$
  2. **Parallel Combination:**
  
  $C_{eq} = C_{12} + C_3 =  \frac{4}{3} \mu F + 8\mu F = \frac{4}{3} \mu F + \frac{24}{3} \mu F = \frac{28}{3} \mu F \approx 9.33 \mu F$
  
  **General Strategy for Complex Networks:**
  
  1.  **Identify Series and Parallel Combinations:** Look for groups of capacitors that are connected purely in series or purely in parallel.
  2.  **Simplify:**  Replace each series or parallel combination with its equivalent capacitance.
  3.  **Repeat:** Continue simplifying the circuit by identifying and combining series and parallel combinations until you are left with a single equivalent capacitance.
## Summary
  
  This lecture explained how to calculate the equivalent capacitance of capacitors connected in series and parallel.  The key formulas are:
  
  *   **Parallel:**  $C_{eq} = C_1 + C_2 + C_3 + ...$
  *   **Series:**  $\frac{1}{C_{eq}} = \frac{1}{C_1} + \frac{1}{C_2} + \frac{1}{C_3} + ...$
  
  Understanding these concepts is crucial for analyzing and designing circuits containing capacitors.