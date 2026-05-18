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
