# Definition

From Wikipedia:

*Phase Correlation is an approach to estimate the relative translative offset between two similar images (digital image correlation) or other data sets. It is commonly used in image registration and relies on a frequency-domain representation of the data, usually calculated by fast Fourier Transforms ([[FFT]]). The term is applied particularly to a subset of [[cross-correlation]] techniques that isolate the phase information from the Fourier-space representation of the cross-correlogram*

# Principle

Begin with two input signals (with dimension $D_n$) - For most of DPA, this dimension will be 1, but this is equivalent for any dimensional signals.


### 1. Input Signals
*Input signals $s_a$ and $s_b$*

### 2a. Window Function
To reduce [[Edge Effects]] one can apply a [[Window Function]] on both signals. 


### 2b. DFT
Then apply the relevant $n-Dimensional$ [[Discrete Fourier Transform]]. 
$$\mathbf{S_n} = \mathcal{F_n}\{s_n\}$$
Where $\mathcal{F_n}$ represents the particular n-Dimensional DFT.


### 3. Cross-Power-Spectrum

Now, on this frequency domain signal, we may compute the [[Cross-Power Spectrum]], taking the [[Complex Conjugate]] (represented by \*) of the second signal DFT result, and multiplying the FTs together *elementwise* and normalizing elementwise as well.

$$R = \frac{\mathbf{S_a}\circ\mathbf{S^*_b}}{|\mathbf{S_a}\circ\mathbf{S^*_b}|}$$
Here, ($\circ$) represents the [[Hadamard Product]]. Element wise, this looks like:

$$R_{j...k} = \frac{\mathbf{S_{a,j...k}}\cdot\mathbf{S^*_{b,j...k}}}{|\mathbf{S_{a,j...k}}\cdot\mathbf{S^*_{b,j...k}}|}$$
### Inverse DFT

It is now possible to obtain the normalized cross-correlation by applying the inverse Fourier transform.

$$\mathbf{r} = \mathcal{F^{-1}}\{R\}$$
And finally, obtain the location of the signal peak - which spells the resulting shift in the second signal ($s_b$). 

$$(\Delta{x}, ..., \Delta{y}) = \underset{(x,...,y)}{\mathrm{argmax}}\{x\}$$
