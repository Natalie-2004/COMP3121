# COMP3121

## Week 2 - Divide And Conquer (DAC)

We met this kind of algo before: the Merge sort! The idea is to split the array into two, sort the two parts recursively and then merge the two sorted array.

#### Problem 1: We are given 27 coins of the same denomination; we know that one of them is counterfeit and that it is lighter than the others. Find the counterfeit coin by weighting coins on a pan balance only three times. 

Solution: 
- Divide the coins into three groups of nine, say A, B and C.
- Weight group A against group B
	- If one group is lighter than the other, it contains counterfeit coin.
	- If instead both groups have equal weight, the group C contains the counterfeit coin.

 #### Problem 2: Assume that you have m users ranking the same set of n movies. You want to determine for any two users A and B how similar their tastes are. How should we measure the degree of similarity of two users A and B?

 For example B = [1, 2, 3, 4, 5, ... 11] and A = [1, 11, 9, 12, 7, ... 5]. Between Movie 1 and 2 B more prefer to 1 and A also prefer 1. Hence Movie 1 and 2 won't count as inversion. However, B more prefers to Movie 4 between 4 and 7 but A prefers 7, this pair counts as an inversion.

The target is find the number of inversion pairs between A and B.
