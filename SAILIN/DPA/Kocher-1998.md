# Intro

Cryptographic devices are implemented using semiconductor logic gates (Not exactly FPGA, but a starting point), which are constructed from transistors.
Electrons flow across the silicon substrate when a charge is applied to (or removed from) a transistor's gate, consuming power and producing electromagnetic radiation.

Using a shunt resistor in series with power or ground input, this power consumption can be measured in analog to the current as $\frac{V}{R}=I$. 

Thus we may obtain a [[trace]], measuring the current over time - capturing a cryptographic operation.

## SPA

### Summary:
Simple power analysis is a method of recording the power consumption of a device, and through clever observation of the trace, deduce when a 1 or a 0 is being used in a simplistic XOR or RSA cipher.

### More Detail:
[[SPA]] or [[DPA/SPA|Simple Power Analysis]] refers to the idea that you can interpret the power consumption measurements collected during the cryptographic operation.

When doing this on a standard gate array processor, the trace shows hints at the cryptographic algorithm the chip is utilizing.

It is even possible to see the clock cycles and the operations happening in the clock cycles.

Things that could allow for this type of power analysis are:

**[[Data Encryption Standard - Wikipedia|DES]] Key rotations**: when the key is rotated, a conditional branch is commonly used to check the bit shifted off the end so that 1 bits can be wrapped around to the front again. The resulting power consumption traces for a 1 bit and a 0 bit will contain different [[DPA/SPA|SPA]] features if the execution paths take different branches for each.

**[[Data Encryption Standard - Wikipedia|DES]] permutations**: Conditional branching can be caused by permutation implementation causing different power profiles for 1 and 0

**Comparisons**: String comparisons typically branch when an error is found, and this causes a large power consumption characteristic changes. 

**Multipliers**: Modular multiplications, and multiplier circuits leak quite a lot of power information on their operands. This can depend on the operand values and [[Hamming Weights]]. 

**Exponentiators**: The square and multiply algorithm typically used to do modular exponentiation: for every 1 in the exponent, the algorithm uses one square and one multiply where as for the 0 bits, the algorithm simply multiplies. 

"*Most (but not all) hard-wired hardware implementations of symmetric cryptographic algorithms have suciently small power consumption variations that SPA do es not yield key material.*"

## [[DPA]]

The idea of [[DPA]] is to use a statistical function tailored to the target algorithm ([[Data Encryption Standard - Wikipedia|DES]] in this case), and to utilize the statistical modeling capabilities even when measurement errors get in the way (Seemingly [[Baysian statistics]]).


### DES Background Overview

- 16 Rounds of DES
- 8 S-box lookup operations per round
- (6 key bits) $\oplus$ (6 bits of *R* register) $\rightarrow$ S-box lookup
- Each S-box gets above x-or'd input, and produces 4 out bits
- $4 \times 8 = 32$ output bits per round
- 32 output bits are reordered
- (32 reordered bits) $\oplus$ (*L* register)
- *L* and *R* are exchanged

### Summary of DPA

The idea behind DPA is to use statistics to infer the key in chunks rather than as a whole. This is done using several statistical meathods.
1. **Averaging multiple traces to eliminate noise**
	1. This principle means to take multiple measurements of power consumption for the same exact cryptographic operation, and average them together thereby attenuating the independant noise on each signal
2. **Using A Selection function to split different averaged traces**
	1. This refers to, once enough *different* traces have been gathered, and averaged using principle 1 to reduce noise, a selection function is defined to split those traces with regard to weather some feature is present or not
3. **Computing the difference in averages of the two groups of traces**
	1. This means that once we have gotten many average traces, and divided them into groups depending on some property, we then find the difference of the averages of those two groups, to see if there is significant statistical difference. If there is, our property is useful, and if not, the property is unuseful and we must try again.

#### What is that property???
In general, this is the question. The property that we seek for is weather a particular bit changes in accordance with our guess of the secret key. To do this, we will use a selection function as defined below.

### Selection Function

#### Summary:
The selection function $D(C,b,K_s)$ is simply a function ($D$) which takes as arguments:
- $C$ - The Cipher Text
- $b$ - the particular bit of interest, and
- $K_s$ - the key guess

and returns exactly the value of bit $b$ with our key guess. 
We can then use this as a splitting function for our traces.

#### Definition

Selection function: $D(C,b,K_s)$

Computes the value of bit $0 \leq b < 32$ of the DES intermediate L at the beginning of the 16th round for a cipher-text $C$, where the 6 key bits entering the S-box corresponding to bit $b$ are represented by $0 \leq K_s < 2^6$.

**NOTE**: If $K_s$ is incorrect, evaluating $D(C,b,K_s)$ will yield the correct value for bit $b$ with probability $P \approx \frac{1}{2}$ for each cipher-text.

### Required Observations

To implement, the Attacker must observe:
1. $m$ encryption operations
2. Capture power traces $\textbf{T}_{1..m}[1..k]$ containing $k$ samples each.
3. Record cipher-texts $C_{1..m}$

**NOTE**: No knowledge of the plain-text is required

### DPA Attack - Implementation

Power consumption measurements determine weather a key block guess $K_s$ is correct.

#### Differential Trace: 

- $\Delta_D [1..k]$  

Is defined to be the *DIFFERENCE* between:
1. *AVERAGE* of the traces for which $D(C,b,K_s)$ is *1* 
2. *AVERAGE* of the traces for which $D(C,b,K_s)$ is *0* 

Thus:

- $\Delta_D [j]$ is the average over $C_{1..m}$ of the effect due to the value represented by the selection function $D$ on the power consumption measurements at point $j$.

In different words, $\Delta_D[j]$ is a point in the differential trace which is the representation of the effect of the value given by $D$ - in essence, the differential trace shows the contribution of power that the particular bit $b$ makes as a 0 or a 1. This is then indexed at point $j$ arbitrarily for the following definition:

$$
\begin{equation}
\begin{split}

\Delta_D[j] = \frac{\sum_{i=1}^m D(C_i, b, K_s)\textbf{T}_i[j]}{\sum_{i=1}^m D(C_i, b, K_s)} - \frac{\sum_{i=1}^m (1-D(C_i, b, K_s))\textbf{T}_i[j]}{\sum_{i=1}^m (1-D(C_i, b, K_s))} \\ \\
\approx 2(\frac{\sum_{i=1}^m D(C_i, b, K_s)\textbf{T}_i[j]}{\sum_{i=1}^m D(C_i, b, K_s)} - \frac{\sum_{i=1}^m\textbf{T}_i[j]}{m})

\end{split}
\end{equation}
$$

### Explanation of the above horrifying summation notation
This looks a little scary, but its not too bad:

The first line is the actual precise definition. It has two terms, which are both very similar.

The *first term* is the sum of the power traces multiplied by the selection function (which is 1 or 0) which sums *ALL TRACES WHICH ARE 1 AT BIT b*

This is then Divided by the sum of the selection function over all cipher texts $C_i$. This is literally just the number of traces which are 1 @ bit $b$

The *second term* is exactly the opposite - the $1 - D(..)$ term is exactly 1 when the selection function outputs a 0 @ bit $b$. Thus it is essentially the sum of *ALL TRACES WHICH ARE 0 AT BIT b$

This is then also divided by the sum of the selection function over all cipher texts $C_i$ which is again just the number of cipher texts whose $b$ bit were 0.


#TODO: CHECK THIS LINE IN EQUATION
The second line is just an approximation of the first part, and I do not yet fully understand this one as it simply subtracts the traces where the selection function was 1 with the sum of all traces averaged, which does not seem exactly true, and could be inaccurate...

Now for the fun part...


### Incorrect $K_s$

If $K_s$ is *incorrect*, then the value of bit $b$ computed using $D$ will differ from the value of bit $b$ computed by the device for $\approx \frac{1}{2}$ of the cipher-texts $C_i$. This means that $D$ is *uncorrelated* with the outputs of the device.

"*If a Random function is used to divide a set into two subsets, the difference in the averages of the subsets should approach zero as the subset sizes approach infinity.*"

Thus, if $K_s$ is incorrect:

$$
	\lim_{m\to\infty} \Delta_D[j] \approx 0
$$
**NOTE**: Trace components which are uncorrelated to $D$ will diminish proportional to $\frac{1}{\sqrt{m}}$, meaning the differential trace should become much flatter with incorrect $K_s$.

(This can be untrue for the case in which the selected $K_s$ and resulting $D$ are weakly correlated with $\textbf{T}_i$


### Correct $K_s$

If $K_s$ IS correct, the computed value for $D(C_i, b, K_s)$ will match the *actual* value of bit $b$ with probability 1.

Thus, $D$ is *STRONGLY* correlated with $\textbf{T}_i$, and the value of the bit manipulated in the 16th round. 

The plot of $\Delta_D$ will be flat with spikes in regions where $D$ is correlated to the values being processed. 


