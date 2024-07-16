# Overview
[[Segmentation in OS]]
# Types
1. Virtual Memory Segmentation
	Divides `processes` in `n` segments
	> Not divided at the same time
 
	_May or may not_ take place at **runtime** of the program
2. Simple segmentation
	Divides `processes` in `n` segments
	> Divides at once 

	_Takes place_ at `runtime` of the program
	Causes segments to be scattered in non contiguous locations in memory
# Why segmentation?
To overcome the drawbacks of Page faults