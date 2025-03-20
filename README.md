# COMP3121

## Week 2 - Divide And Conquer (DAC)

We have met this kind of algo before: the merge sort! The idea is to split the array into two, sort the two parts recursively and then merge the two sorted array.

Main Idea: Break down a complex problem into subproblems, and then subproblems into smaller subproblems...all the way until there's a simple direct solution (i.e. the base case). The solution of the origin complex problem is exactly the 
Steps:
1. Divide: Break the given problem into smaller non-overlapping problems.
2. Conquer: Solve Smaller Problems
3. Combine: Use the Solutions of Smaller Problems to find the overall result.

#### Problem 1: We are given 27 coins of the same denomination; we know that one of them is counterfeit and that it is lighter than the others. Find the counterfeit coin by weighting coins on a pan balance only three times. 

Solution: 
- Divide the coins into three groups of nine, say A, B and C.
- Weight group A against group B
	- If one group is lighter than the other, it contains counterfeit coin.
	- If instead both groups have equal weight, the group C contains the counterfeit coin.

#### Problem 2: Assume that you have m users ranking the same set of n movies. You want to determine for any two users A and B how similar their tastes are. How should we measure the degree of similarity of two users A and B?

For example B = [1, 2, 3, 4, 5, ... 11] and A = [1, 11, 9, 12, 7, ... 5]. Between Movie 1 and 2 B more prefer to 1 and A also prefer 1. Hence Movie 1 and 2 won't count as inversion. However, B more prefers to Movie 4 between 4 and 7 but A prefers 7, this pair counts as an inversion.

The target is find the number of inversion pairs between A and B.

Suppose we have n size of array. We can split the array into half, forming Alo = A[1..m] and Ahi = A[m + 1, n], where m = [n/2]. Note that the total number of inversions in array A is equal to the sum of the number of inversions in Alo + Ahi + A(lo, hi -> across the two halves). Alo and Ahi are the numbers of inversion, which can be calculated recursively. The hard point is to count how many inversions across the half. 

Given numerical example:
Alo = [1, 5, 9, 12, 7, 10]; Ahi = [3, 4, 6, 8, 2, 11]
How many inversions involve the 8? It's the number of elem in Alo that are greater than 8. 

However, in order to solve more efficiently, we not only count the inversions across the partition but also sort them. We can assume that the subarrays Alo and Ahi are sorted in the process of counting I(Alo) and I(Ahi): Alo = [1, 5, 7, 9, 10, 12]; Ahi = [2, 3, 4, 6, 8, 11]. Now, we're merging them together and as we merge, we count how many inversions we get. 

- Compare the ith elem in Alo and Ahi:
	- if Alo is smaller, insert into the array. 
 	- if Ahi is smaller, insert into the array. Since Alo[i] > Ahi[i], and is sorted, everything after ith elem at Alo is greater than Ahi. This means these two forms an inversion with everything else remaining at Alo.

Whenever there're cases that Alo[i] > Ahi[i-x] elem, this should counts as inversions but we had already counted them. The double-counted is disregarded. 
The total inversions should be: 5 + 5 + 5 + 4 + 3 + 1, with A sorted as [1, 2, 3, 4, 5, 8, 9, 10, 11, 12].

This modified merge sort still takes linear time, i.e O(n log n).

### Recurrences T(n) = aT(n/b) + cn
It's estimates time complexity of DAC. That is:

- reduces a problem of size n to a many subproblems
- of a smaller size n/b
- with overhead cost of f(n) to split up the problem and combine solution
<img width="1040" alt="Screenshot 2025-03-09 at 10 20 18 pm" src="https://github.com/user-attachments/assets/dc675dff-b2de-4d72-8827-c5d1a08a9914" />

### Master Theorem
See wk2 lec slides.

<img width="974" alt="Screenshot 2025-03-09 at 10 23 27 pm" src="https://github.com/user-attachments/assets/f4e58380-eea7-443f-90b3-7bc3f1d9fe51" />

- Case 1: the amount of work to split up and re-combine is small, dominated by the number of subproblems to the total time taken than the actual individual split up and re-combine. **f(n) SMALLER THAN n^{logb a}**
- Case 2: they contribute equally
- Case 3: the amount of work to split up and re-combine is large. **f(n) LARGER THAN n^{logb a}**

If none of these contributions hold, the master theorem will not be applied.

#### Example 1: Let T(n) = 4T(n/2) + n.

The critical exp is logb a = log 2 4 = 2 -> the critical poly is n^2.

Compare n with n^2, which case does it fit? Case 1. f(n) = n = Omega(n^(2 - epsilon)) for small epsilon i.e. 0.1.

Hence, T(n) = O(n^2). 

#### Example 2: Let T(n) = 2T(n/2) + 5n.

The critical exp is logb a = log2 2 = 1 -> the critical poly is n.

Now, f(n) = 5n = Theta(n). It falls into case 2, where T(n) = Theta(n log n).

#### Example 3: Let T(n) = 3T(n/4) + n.

The critical exp is logb a = log4 3 = 0.7925... -> the critical poly is n^(log4 3).

Now, f(n) = n = Bit Omega(n^(log4 3 + epsilon)) for small e as 0.1. Also, af(n/b) = 3f(n/4) = 3/4n < cn = cf(n) for c = .9 < 1. # Note Case 3 has two condition to satisfy. 

It falls into case 3, where T(n) = Theta(f(n)) = Theta(n).

### The Karatsuba trick - Dealing with Big integer multiplications

(Karatsuba algorithm for fast multiplication using Divide and Conquer algorithm)[https://www.geeksforgeeks.org/karatsuba-algorithm-for-fast-multiplication-using-divide-and-conquer-algorithm/]

<img width="521" alt="Screenshot 2025-03-11 at 9 49 37 pm" src="https://github.com/user-attachments/assets/3a233c95-38fd-4029-acd1-a3960f7c08be" />

Suppose there're two integers A and B which consist n bits.

The base case is n = 1, where we simply evaluate the product. 
Otherwise, we let:

- A1 and A0 be the most and least significant parts of A respectively
- B1 and B0 likewise

<img width="662" alt="Screenshot 2025-03-12 at 3 22 05 pm" src="https://github.com/user-attachments/assets/2ad2589f-6678-4f43-a095-4d2e336ce0b8" />
<img width="700" alt="Screenshot 2025-03-12 at 3 19 36 pm" src="https://github.com/user-attachments/assets/4b2e44a2-a461-4513-b49f-7efd14f9d310" />

After cancelation, we end up 3 multiplications, saving 1 multiplications at each recurrence.

Therefore, the time complexity is O(n^(log_2 3)).

### The FFT 

Idea: Mapping arrays into binary format, dividing into odd and even arrays and thereby recursively computing FFT(as a black box) to get the coefficient of the polynomial multiplications.
Time Complexity: O(n log n), Case 2.

## Week 3 - The Greedy Algorithms

Main Idea: Solves a problem by dividing it into stages(choices), which looks the best at the moment, with the hope that these local optimal choices will lead to a global optimal solution. Rather than exhaustively searching all the ways to get from one stage to the next.

Note that Greedy is not always always correct, especially you can disprove it's best optimal using a counter-example. The way to prove its correctness are:

- <ins>Greedy stays ahead</ins>: prove that at every stage, no other sequence of choices could do better than our proposed algo.
- <ins>Exchange argument</ins>: consider an alternative solution, and gradually transform it to the solution found by our proposed algo without making it any worse. 

Optimal Selection: 
#### Example 1: Activity Selection
<img width="600" alt="image" src="https://github.com/user-attachments/assets/e5def2da-4df3-484d-8c05-3ebb1bf020f1" />

Let's try different attempts: 
Attempt 1. Always choose the shortest activity which does not conflict with the previously chosen activities, then remove the conflicting activities and repeat. 
<img width="560" alt="image" src="https://github.com/user-attachments/assets/4e95b3fd-f875-4892-9836-a9dbe30b075f" />

Attempt 2. Always choose an activity which conflicts with the fewest possible number of the remaining activities. It may
appear that in this way we minimally restrict our next choice:
<img width="558" alt="image" src="https://github.com/user-attachments/assets/ea5a498c-5faa-4667-ac0b-d31d8a769d9f" />

Solution:
Instead of working from middle part, work through them from left to right, i.e. sort the activities by n time and always pick that activity whose n time is the earliest. This guarantee the first activity in our schedule and we remove any activity that's conflicts with it. If there are several activities finished with the earliest n time, randomly pick one. 

<img width="603" alt="image" src="https://github.com/user-attachments/assets/75c8b869-05bf-400c-a3ed-b3e7b480f1c4" />

Looking at the first differnt letter: B:
- Is the sequence ABEG as good as the sequence ACEG? 
- Is it even a legal sequence? Can C changed to B? Does new activity B can fit with A, E and G?

B can fit with A, as greedy choses the one that finish first. Given that B finishes earlier than C, B can fit with EG. Hence, the sequence ABEG is legal, with the same size of the sequence ACEG. 

Repeat this chunk for the three remaining combinations. 

No matter what alternative solution is considered, the greedy solution is always at least as good, thereby proving its correctness and optimality for the problem. Hence, the greedy algo is correct at this case. 

Time complexity:
<img width="638" alt="image" src="https://github.com/user-attachments/assets/8e8c65d7-7246-4189-8fa2-8511e66cd8eb" />

#### Example 2: Cell Towers

<img width="572" alt="image" src="https://github.com/user-attachments/assets/69b3f4e7-69e4-4761-9171-8277f78c8637" />

Let's attempt a greedy algo, processing the houses west to east. The first house must be covered by some tower, which we place 5km to the east of this house. This tower may cover some other houses, but eventually we should reach a house that is again out of range of this tower. We then place a second tower 5km apart to the east of the house. Continue this step until all houses get covered. 

At each house, we need to decide whether to place a new tower. This is done in constant time. 

Therefore, this algo runs in O(n) time if the houses are provided in order, and O(n log n) time otherwise. 

Then prove the correctness of greedy. 

Optimal Ordering:
#### Example 3: Minimising Job Lateness

<img width="600" alt="Screenshot 2025-03-17 at 11 03 41 pm" src="https://github.com/user-attachments/assets/afa24bcb-1220-4c01-9558-e48718fdff24" />

<img width="600" alt="image" src="https://github.com/user-attachments/assets/21626fc8-aadd-4b20-8179-8a55943e78f2" />
<img width="600" alt="image" src="https://github.com/user-attachments/assets/b70284fe-0da9-41c4-a6ea-1ce1c016e908" />
<img width="573" alt="image" src="https://github.com/user-attachments/assets/05524533-f913-46e5-a21b-eaa0f15ce7b9" />
