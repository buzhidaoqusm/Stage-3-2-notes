#exam Briefly describe two ways in which the process of running Boolean queries can be optimised so that they can be processed more efficiently.
# Query Optimisation
- Process in order of increasing frequency
	- Start with the smallest set
		- 从最小的开始，到达最后就可以停，因此更省时间
- This is why we record document frequency in the dictionary
![](/assets/1c%20Query%20Optimisation/file-20260420203323040.png)
## AND and OR
> (madding OR crowd) AND (ignoble OR strife)

- **Estimate** the size of each **OR** operation
	- **Maximum possible size** is the **sum** of the frequencies of the terms
- Process in increasing order of OR size
# skip pointer
- 上下都走到8时
- 41>11，并且11的skip pointer是31，它也小于41，所以可以直接跳过中间所有的部分
this only works if the list is **sorted**
![](/assets/1c%20Query%20Optimisation/file-20260420204704593.png)
## Placing skips
Simple heuristic: for postings of length L, use `根号L` 
Easy if the **index is relatively static**; **harder** if L keeps **changing** because of updates.

This **definitely** used to **help**; with **modern hardware** it may **not** unless you have a memory-based index
The **I/O cost of loading a bigger postings** list can **outweigh** the gains from quicker in **memory merging**!