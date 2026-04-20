# Query Optimisation
- Process in order of increasing frequency
	- Start with the smallest set
		- 从最小的开始，到达最后就可以停，因此更省时间
- This is why we record document frequency in the dictionary
![](assets/1c%20Query%20Optimisation/file-20260420203323040.png)
## AND and OR
> (madding OR crowd) AND (ignoble OR strife)

- **Estimate** the size of each **OR** operation
	- **Maximum possible size** is the **sum** of the frequencies of the terms
- Process in increasing order of OR size
# skip pointer
