# CombSUM
final score is calculated by adding the individual scores it was given in each of the input result sets

**High scores** in the input result set are carried to the **fused result set**, so the **Skimming Effect** is in use.

because the scores are added, documents appearing in multiple result sets will also receive a boost, exploiting the **Chorus Effect**

> Example

- Example 1: 
	- Document #1 is given a score of 0.45 by System A 
	- Document #1 is given a score of 0.3 by System B 
	- Document #1 is given a score of 0.35 by System C 
	- Document #1's CombSUM score is 0.45 + 0.3 + 0.35 = 1.1 
- Example 2: 
	- Document #2 is given a score of 0.55 by System A 
	- Document #2 is not returned by System B 
	- Document #2 is given a score of 0.65 by System C  
	- Document #2's CombSUM score is 0.55 + 0 + 0.65 = 1.2
## Score Normalisation
Consider the situation where different IR systems may calculate scores in different ranges: 
- System A: Assigns scores between 0 and 1000 
- System B: Assigns scores between 0 and 1

standard normalisation:
$$
\text{normalised\_score} = \frac{\text{unnormalised\_score} - \text{min\_score}}{\text{max\_score} - \text{min\_score}}
$$
## CombSUM
归一化之后加起来
![](/assets/10a%20Fusion%20Score%20Based/file-20260518210851936.png)
![](/assets/10a%20Fusion%20Score%20Based/file-20260518210900920.png)

# CombMNZ
Although CombSUM makes use of the **Chorus Effect** by adding the normalised scores, CombMNZ **emphasises this even further**.
It does this by **multiplying** each document's **CombSUM** score by the **number of result sets** it was contained in
MNZ stands for "**Multiply by Non Zero**".

- Example 1:
	- Document #1 is given a score of 0.45 by System A
	- Document #1 is given a score of 0.3 by System B
	- Document #1 is given a score of 0.35 by System C
	- Document #1's CombMNZ score is (0.45 + 0.3 + 0.35) x 3 = 3.3
- Example 2:
	- Document #2 is given a score of 0.55 by System A
	- Document #2 is not returned by System B
	- Document #2 is given a score of 0.65 by System C
	- Document #2's CombMNZ score is (0.55 + 0 + 0.65) x 2 = 2.4

![](/assets/10a%20Fusion%20Score%20Based/file-20260518211328689.png)


CombMNZ should only be used for **data fusion tasks**
# Linear Combination
每个IR给不同的权重
CombMNZ and CombSUM: every **input result set** is assumed to be of **equal** quality
Linear Combination Model apply a **weighting**

Example 1 (System A, B and C have weights of 1, 2, 3 respectively): 
- Document #1 is given a score of 0.45 by System A 
- Document #1 is given a score of 0.3 by System B 
- Document #1 is given a score of 0.35 by System C 
- Document #1's overall score is: (0.45 x 1) + (0.3 x 2) +(0.35 x 3) = 2.1

## Weights
1. CORI, which relies on having access to **details about the document collection** each input system is using (generally used for Distributed IR).
	- Document Frequency
	- Inverse Collection Freqency
2. LMS(using result Length)
	- **longer** result set results in **higher** weight. Worked surprisingly well in practice.