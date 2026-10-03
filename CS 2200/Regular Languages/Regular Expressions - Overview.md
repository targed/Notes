- **1. Introduction**
  
  *   **Purpose:** Regular Expressions (REs) are a formal algebraic notation used to describe **patterns** or **sets of strings** (languages). They serve as a textual, declarative way to specify the members of a regular language.
  *   **Equivalence:** Regular Expressions are **equivalent in expressive power** to Finite Automata (DFAs and NFAs). This means any language that can be described by an RE can be recognized by an FA, and vice-versa.
  *   **Applications:** Widely used in:
    *   Programming languages (Perl, Python, Java, etc.) for string searching and manipulation.
    *   Text editors (vi, Emacs) and command-line tools (grep) for pattern matching.
    *   Lexical analysis in compilers to define tokens.
    *   Database query languages.
    *   Bioinformatics for sequence analysis.
  
  **2. Regular Sets**
  
  *   **Definition 2:** Any set of strings that can be represented by a regular expression is called a **regular set**.
  *   Essentially, "regular set" is another term for a **regular language** when discussed in the context of REs.
  
  **3. Definition of a Regular Expression (Recursive)**
  
  *   Regular Expressions over an alphabet $\Sigma$ are defined recursively as follows:
  
    *   **Base Cases (Primitive REs):**
        1.  **Empty Set:** $\emptyset$ (or sometimes $\Phi$) is a regular expression denoting the empty language $L(\emptyset) = \{\}$. (The language containing *no strings*).
        2.  **Empty String:** $\epsilon$ (or sometimes $\lambda$) is a regular expression denoting the language containing only the empty string: $L(\epsilon) = \{\epsilon\}$.
        3.  **Symbol:** For any symbol $a \in \Sigma$, $a$ is a regular expression denoting the language containing only that single symbol: $L(a) = \{a\}$.
  
    *   **Inductive Cases (Operations):** If $R_1$ and $R_2$ are regular expressions representing languages $L(R_1)$ and $L(R_2)$ respectively, then the following are also regular expressions:
        4.  **Union (Alternation):** $(R_1 + R_2)$ or $(R_1 | R_2)$ or $(R_1 \cup R_2)$ is an RE representing the union of the languages: $L(R_1 + R_2) = L(R_1) \cup L(R_2)$. (Strings matching $R_1$ *or* $R_2$).
        5.  **Concatenation:** $(R_1 \cdot R_2)$ or simply $(R_1 R_2)$ is an RE representing the concatenation of the languages: $L(R_1 R_2) = \{xy \mid x \in L(R_1) \text{ and } y \in L(R_2)\}$. (Strings formed by taking a string from $L(R_1)$ followed by a string from $L(R_2)$).
        6.  **Kleene Star (Closure):** $(R_1^*)$ is an RE representing the Kleene closure of the language: $L(R_1^*) = \bigcup_{i=0}^{\infty} L(R_1)^i = L(R_1)^0 \cup L(R_1)^1 \cup L(R_1)^2 \cup \dots$, where $L(R_1)^0 = \{\epsilon\}$. (Strings formed by concatenating zero or more strings from $L(R_1)$).
  
  *   **Precedence:** Similar to arithmetic operations, RE operators have precedence to reduce the need for parentheses:
    1.  Kleene Star (*) has the highest precedence.
    2.  Concatenation ($\cdot$) has the next highest precedence.
    3.  Union (+) has the lowest precedence.
    *   Example: $a + b \cdot c^*$ is interpreted as $a + (b \cdot (c^*))$.
  
  **4. Examples of REs and the Languages (Regular Sets) They Represent**
  
  *(Based on examples in the text, over $\Sigma = \{0, 1\}$ unless specified)*
  
  *   `101`: Represents the language $\{101\}$. (Base case concatenation).
  *   `ε + a`: Represents the language $\{\epsilon, a\}$. (Union).
  *   `(a + b)*`: Represents the language $\{\epsilon, a, b, aa, ab, ba, bb, aaa, \dots\}$. All possible strings over $\{a, b\}$, including the empty string. (Union and Kleene Star).
  *   `ab + ba`: Represents the language $\{ab, ba\}$. (Concatenation and Union).
  *   `a*`: Represents the language $\{\epsilon, a, aa, aaa, \dots\}$. Zero or more 'a's.
  *   `(0 + 1)*`: All possible strings of 0s and 1s, including $\epsilon$.
  *   `(0 + 1)*00`: All strings of 0s and 1s that end with "00".
  *   `0(0 + 1)*1`: All strings that start with '0' and end with '1'.
  *   `(11)*`: Strings consisting of an even number of 1s (including zero 1s, i.e., $\epsilon$). Language: $\{\epsilon, 11, 1111, 111111, \dots\}$.
  *   `1(11)*` or `(11)*1`: Strings consisting of an odd number of 1s. Language: $\{1, 111, 11111, \dots\}$.
  *   `(0 + 1)*00(0 + 1)*`: Strings containing "00" as a substring.
  *   `(1 + 10)*`: Strings not containing "00" as a substring (every 0 must be followed by a 1 if not the end).
  *   `(0 + ε)(1 + 10)*`: Strings not containing "00", possibly starting with 0.
  *   `(0 + 1)*011`: Strings ending in "011".
  *   `0+1+2+`: Strings containing at least one '0', followed by at least one '1', followed by at least one '2'. Example: `012, 0012, 0112, 0122, ...`. (Note: `+` here means one or more, Kleene Plus, where $R^+ = RR^*$).
  *   `(0 + 1)*(00 + 11)`: Strings whose last two symbols are the same.
  *   `(1 + 011)*`: Strings where every '0' is immediately followed by "11". Includes strings of only '1's.
  
  **5. Identity Rules and Algebraic Laws (Brief Mention)**
  
  *   The overview introduces the idea that there are rules for simplifying or manipulating REs while preserving the language they represent.
  *   Examples mentioned (detailed in Sec 3.3, 3.4):
    *   Associativity: $(r+s)+t = r+(s+t)$, $r(st)=(rs)t$.
    *   Commutativity of Union: $r+s = s+r$.
    *   Distributivity: $r(s+t) = rs + rt$, $(r+s)t = rt + st$.
    *   Identity elements: $R+\emptyset = R$, $R\epsilon = \epsilon R = R$.
    *   Idempotence of Union: $R+R = R$.
    *   Rules involving Kleene Star: $RR^* = R^*R = R^+$, $(R^*)^* = R^*$, $\epsilon + RR^* = R^*$.
  
  ---
  
  **Helpful Resources:**
  
  1.  **Wikipedia - Regular expression:** [https://en.wikipedia.org/wiki/Regular_expression](https://en.wikipedia.org/wiki/Regular_expression) (Covers both theoretical and practical aspects).
  2.  **Regular-Expressions.info:** [https://www.regular-expressions.info/](https://www.regular-expressions.info/) (Excellent site for practical regex usage and tutorials).
  3.  **Regex101:** [https://regex101.com/](https://regex101.com/) (Online tool to build, test, and debug regular expressions).
  4.  **JFLAP Tutorial - Regular Expressions:** [https://www.jflap.org/tutorial/re/definition/index.html](https://www.jflap.org/tutorial/re/definition/index.html)