# COMP3121

Based on lec 22T2, lecture by Ravaan: https://www.youtube.com/playlist?list=PLVly8g0-6d_fT3YiFeK4TgQzaukR4BC6-

- [Mod 2 - Divide And Conquer (DAC)](#mod-2---divide-and-conquer-dac)
- [Mod 3 - The Greedy Algorithms](#mod-3---the-greedy-algorithms)
- [Mod 4 - The Flow Networks](#mod-4---the-flow-networks)
- [Mod 5 - Dynamic Programming](#mod-5---dynamic-programming)

## Mod 2 - Divide And Conquer (DAC)

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

## Mod 3 - The Greedy Algorithms

Main Idea: Solves a problem by dividing it into stages(choices), which looks the best at the moment, with the hope that these local optimal choices will lead to a global optimal solution. Rather than exhaustively searching all the ways to get from one stage to the next.

Note that Greedy is not always always correct, especially you can disprove it's best optimal using a counter-example. The way to prove its correctness are:

- <ins>Greedy stays ahead</ins>: prove that at every stage, no other sequence of choices could do better than our proposed algo.

Structure: similar to proof by induction: 
	1. Identify a base case
 	2. Raise an assumption for some k.
  	3. Prove that at ... condition at the k + 1^{th} or other things by something.

- <ins>Exchange argument</ins>: consider an alternative solution, and gradually transform it to the solution found by our proposed algo without making it any worse.

Structure: give two expressions such that Greedy = {G_1, G_2... G_k} and Optimal = {O_1, O_2...O_k} for some k. Assume that first (i-1) elements are the same. As we known g_i != o_i. If you replace o_i with g_i, then O becomes better.



### Optimal Selection:
#### Example 1: Activity Selection
<img width="600" alt="image" src="https://github.com/user-attachments/assets/e5def2da-4df3-484d-8c05-3ebb1bf020f1" />

Let's try different attempts:  
```
Attempt 1. Always choose the shortest activity which does not conflict with the previously chosen activities, then remove the conflicting activities and repeat. 
```
<img width="600" alt="image" src="https://github.com/user-attachments/assets/4e95b3fd-f875-4892-9836-a9dbe30b075f" />

```
Attempt 2. Always choose an activity which conflicts with the fewest possible number of the remaining activities. It may
appear that in this way we minimally restrict our next choice:
```
<img width="600" alt="image" src="https://github.com/user-attachments/assets/ea5a498c-5faa-4667-ac0b-d31d8a769d9f" />

Solution:
Instead of working from middle part, work through them from left to right, i.e. sort the activities by n time and always pick that activity whose n time is the earliest. This guarantee the first activity in our schedule and we remove any activity that's conflicts with it. If there are several activities finished with the earliest n time, randomly pick one. 

<img width="603" alt="image" src="https://github.com/user-attachments/assets/75c8b869-05bf-400c-a3ed-b3e7b480f1c4" />

Looking at the first different letter: B:
- Is the sequence ABEG as good as the sequence ACEG? 
- Is it even a legal sequence? Can C changed to B? Does new activity B can fit with A, E and G?

B can fit with A, as greedy choses the one that finish first. Given that B finishes earlier than C, B can fit with EG. Hence, the sequence ABEG is legal, with the same size of the sequence ACEG. 

Repeat this chunk for the three remaining combinations. 

No matter what alternative solution is considered, the greedy solution is always at least as good, thereby proving its correctness and optimality for the problem. Hence, the greedy algo is correct at this case. 

Time complexity:

<img width="638" alt="image" src="https://github.com/user-attachments/assets/8e8c65d7-7246-4189-8fa2-8511e66cd8eb" />

#### Example 2: Cell Towers

<img width="600" alt="image" src="https://github.com/user-attachments/assets/69b3f4e7-69e4-4761-9171-8277f78c8637" />

Let's attempt a greedy algo, processing the houses west to east. The first house must be covered by some tower, which we place 5km to the east of this house. This tower may cover some other houses, but eventually we should reach a house that is again out of range of this tower. We then place a second tower 5km apart to the east of the house. Continue this step until all houses get covered. 

At each house, we need to decide whether to place a new tower. This is done in constant time. 

Therefore, this algo runs in O(n) time if the houses are provided in order, and O(n log n) time otherwise. 

Then prove the correctness of greedy. 


### Optimal Ordering:
#### Example 3: Minimising Job Lateness

<img width="600" alt="Screenshot 2025-03-17 at 11 03 41 pm" src="https://github.com/user-attachments/assets/afa24bcb-1220-4c01-9558-e48718fdff24" />

<img width="600" alt="image" src="https://github.com/user-attachments/assets/21626fc8-aadd-4b20-8179-8a55943e78f2" />
<img width="600" alt="image" src="https://github.com/user-attachments/assets/b70284fe-0da9-41c4-a6ea-1ce1c016e908" />
<img width="600" alt="image" src="https://github.com/user-attachments/assets/05524533-f913-46e5-a21b-eaa0f15ce7b9" />

#### Example 4: Tape Storage
A list of n files of lengths Li which have to be stored on a tape. Each file is equally likely to be needed. To retrieve a file, one must start from the beginning of the tape and scan it until the file is found and read.

Task: Order the files on the tape so that the average retrieval time is minimised. 

We start by sorting the tapes so that the smallest retrieval time lines up the first, all the way until the largest one. This is because we try to minimise to time of transmission by not letting large tape block at the front position. Therefore, we get the expression:
$Sum_of_time = l1 + (l1 + l2) + (l1+l2+l3) + (l1+l2+l3+l4) + ... + (l1 + ... + ln)$

And the probability we want to read any one of the files is 1/n. Therefore,

$E = \sum_{i=1}^{n} \frac{1}{n} \cdot (l_1 + ... + l_i)\\
  = \frac{1}{n} \sum_{i=1}^{n} (l_1 + ... + l_i)\\
  = \frac{1}{n} (n l_1 + (n - 1) l_2 + ... + 2l_{n-1} + l_n)$

This is minimised if l1 <= l2 <= l3 <=... <= ln, so we simply sorted in increasing order that takes O(n log n) time.

<ins>Proof by Optimally<\ins>

<img width="697" alt="image" src="https://github.com/user-attachments/assets/b17787ea-4726-4d9a-8dcc-0c0dae4c2807" />

Advanced qn: 

<img width="752" alt="image" src="https://github.com/user-attachments/assets/b785a9fe-cf33-41a3-a461-a51a9b8735ee" />

If we go by decreasing $\frac{P_i}{l_i} priorities files with larger probability would comes to the front, and the files with smaller l_i also come to the front. Therefore, the expected value is the sum of probability * the length of all files.

$E = l1p1 + (l1+l2)p2 + ... + (l1 + l2 + ... + ln)pn$

We now show that this is minimised if the files are ordered in a decreasing order of values of the ratio pi / li. 

<ins>Proof by Optimally<\ins>

Again suppose we have some ordering l1 ... ln and suppose they're out of order what happens if i swap two things that are adjacent and out of order, that is, two things that re in the wrong order of this ratio. 

Let us see what happens if we swap two adjacent files, denoted as k and k+1. The expected time before the swapping and after the swap are, respectively:  
<img width="450" alt="image" src="https://github.com/user-attachments/assets/abeb9085-e9c7-4ce0-8c54-057bac983dc1" />  
<img width="450" alt="image" src="https://github.com/user-attachments/assets/c02c2e2e-56fc-4d1a-becd-29d39138ea2c" />  
It's clear that th previous files $1 .. k_{n-1}$ are unaffected, the probability and the time taken are the same as we walk through files that are unaffected by the swap. The same is true of the subsequent files k + 2 .. n. 

To access the file k + 1 with probability $p_{k + 1}$ we need to go through all the previous k - 1 files and then file k + 1 which is now being stored in the kth position. Conversely to get the file k we need to go through the first k - 1 files and then file k + 1 and finally file k. We observe that the terms $lk * pk$ and $l_{k+1} * p_{k+1}$ appear in E but also in E', leaving the only difference is the red highlighted in E and blue highlighted in E'. 
<img width="605" alt="image" src="https://github.com/user-attachments/assets/cb5408d7-04a1-4c4f-8ee0-ca04243dc6f4" /> <h>

Thus, $E - E'$ = $l_kp_{k+1} - l_{k+1}p_{k}$, which is a positive whenever $l_kp_{k+1} > l_{k+1}p_{k}$, i.e. when $\frac{p_k}{l_k} < \frac{p_{k + 1}}{l_{k+1}}$. Similar like the bubble sort process. 

Consequently, E > E' if and only if $\frac{p_k}{l_k} < \frac{p_{k + 1}}{l_{k+1}}$, which means that the swap decreaese the expected time whenver $\frac{p_k}{l_k} < \frac{p_{k + 1}}{l_{k+1}}$, i.e. if there is an inversion: file k + 1 with a larger ratio $\frac{p_{k + 1}}{l_{k+1}}$ has been put after file k with a smaller ratio $\frac{p_k}{l_k}$. As long as the sequence isn't already sorted, there will be inversions of consecutive files, and swapping will reduce the expected time. Consequently, the optimal solution is the one with no inversions. 

#### Example 5: Interval Stabbing (also at mod 3 quiz): Let X be a set of n intervals on the real line, described by two arrays $X_l[1...n]$ and X_r[1...n], representing their left and right endpoints. We say that a set P of points stabs X if every interval in X contains at least one point in P. Describe and analyse an efficient algorithm where given the interval set we can work out the smallest set of points that stabs X. 

<img width="432" alt="Screenshot 2025-03-22 at 8 26 16 pm" src="https://github.com/user-attachments/assets/d7365250-8559-406b-8cd4-8e66493270b1" />

Consider these ideas:

- It is a good idea to stab the largest possible number of intervals?
- <img width="419" alt="Screenshot 2025-03-22 at 8 30 08 pm" src="https://github.com/user-attachments/assets/8c720774-aaa3-4a3f-aabb-8ba0956fa72e" />
- This is a counterexample. It requires you to stab three times. The optimal should be two times.  

Hint: the interval that ends the earliest has to be stabbed somewhere. What is the best place to stab it? 

#### Example 6: Fractional Knapsack (Optional)


### Optimal Merging:
#### Example 7: Array Merging
Assume you are given n sorted arrays of different sizes. You are allowed to merge to any two arrays into a single new sorted array and proceed in this manner until only array is left. 

Design an algorithm which ahieves this task and moves array elements as few times as possible. Give an informal justification why your algorithm is optimal. 

<ins>The huffman Code</ins>  
Assume you are given a set of symbols, for example the english alphabet plus punctuation marks and a blank space (between words). You want to encode these symbols using binary strings, so that sequence of such symbols can be decoded in an unambiguous way. # Unambiguous means there's only one way to decode the binary string. 

One way of doing so is to reserve bit strings of equal and sufficient length, given the number of istinct symbols to be encoded. For example, if you have 26 letters and up to 6 punctuation symbols, you could use strings of 5 bits, as $2^5 = 32 (31 symbols < 32)$. To decode a piece of text you would partition the bit stream into groups of 5 bits and use a lookup table to decode the text. 

Notably, common symbols and rare symbols have the same length of binary coding, but their frequent are not equal. One way to improve performance is to write frequent symbols such as vowels into short codes while infrequent ones, such as 'q', 'x' can have longer codes. 

However, if the codes are of variable length, them how can we decoding it?
Suppose we have this to interpret:
```
a = 10, b = 0, c = 101, d = 11
text = 0111010
```
The first zero must correspond to letter b. The second part '11' must be letter d. But we how know the next part is 'aa' or 'cb'?

One way of ensuring unqiue readability of codes from a single bitsteam is to ensure that no of a symbol is a prefix of a code for another symbol. Codes with such property are called <ins>prefix codes</ins>. For example:

<img width="878" alt="Screenshot 2025-03-22 at 9 55 21 pm" src="https://github.com/user-attachments/assets/e7d11985-d833-44d9-b9a6-205aea476d71" />  

The **Huffman code** uses greedy method to construct an optimal prefix code. This makes sure that the average number of bits in an encoding of an “average” text is as small as possible.  
<img width="600" alt="Screenshot 2025-03-22 at 9 57 14 pm" src="https://github.com/user-attachments/assets/5377dca8-b8b0-4636-a31f-513593109366" />  
<img width="400" alt="image" src="https://github.com/user-attachments/assets/c1c036be-3ec2-4f5a-af7b-361510099a71" /><img width="400" alt="image" src="https://github.com/user-attachments/assets/c9dca43e-f0a5-4b87-b241-a040c149c3ad" />


**Application to graphs**
#### Directed graph structure Example 8: Tsunami Warning  
<img width="400" alt="Screenshot 2025-03-23 at 4 02 58 pm" src="https://github.com/user-attachments/assets/4cac8364-c295-45fb-819a-a38bcd4306fa" /><img width="400" alt="image" src="https://github.com/user-attachments/assets/42eb0b88-00f6-4827-ba4f-6e164367ea4b" />

Key insights: Location (x_i, y_i), radius(r_i); A tower i can indirectly activate other towers j if there's a sequence of activations from i to j.

```
Attempt 1: Find the unactivated tower with the largest radius and place a sensor at this tower. Find and remove all activated towers. Repeat this process.

Counterexample:

Suppose we have towers:
Towers:
Tower A: (0, 0), radius 1
Tower B: (1, 0), radius 1
Tower C: (3, 0), radius 1

You would first activate Tower A or B since they have the largest radius (1).
Activating either A or B will only activate one other tower (C), requiring two sensors in total.
The optimal solution is to activate Tower C first, which will activate both A and B due to their proximity, requiring only one sensor.
```

```
Attempt 2: Find the unactivated tower with the largest number of towers within its range. If there's none, place a sensor at the leftmost tower. Repeat.

Reusing the same example, you would activate either Tower A or B first since they both have one tower within their range.
Activating A or B will not activate all towers, requiring two sensors in total.
The optimal solution is to activate Tower B, which will activate both A and C, requiring only one sensor.
```

Thinking of this as a directed graph. Suppose that activating tower a causes tower b to also be activated, and vice versa. Then we never want to place sensors at
both towers; indeed, placing a sensor at a is equivalent to placing a
sensor at b. We can extend this idea into forming a cycle: A -> B, B -> C/D, C -> A. All four towers can be activated by placing just one sensor at a or b or c.

Let S be a subset of towers such that activating any tower in S causes the activation of all towers in S. We never want to place more than one sensor in S, and if we place one, then it doesn't matter where we put it. (Either direct or indirect). And we need every vertex to reach everyone, so-called **strongly connected components**. 

<ins>Definition: Given a directed graph G = (V, E) and a vertex v, the strongly connected component of G containing v consists of all vertices u ∈ V such that there is a directed path in G from v to u and a directed path from u to v. We will denote it by Cv.</ins>
_  

How do we find the strongly connected component Cv ⊆ V containing v?

Construct another graph Grev = (V, Erev) consisting of the same set of vertices V but with the set of edges Erev obtained by reversing the direction of all edges E of G. u is in Cv if and only if u is reachable from v and v is reachable from u. Equivalently, u is reachable from v in both G and Grev.

<img width="400" alt="image" src="https://github.com/user-attachments/assets/91502db6-26d0-4780-883f-17aabb913377" /><img width="400" alt="image" src="https://github.com/user-attachments/assets/6a0e0861-2152-469a-846f-e1ded4237b9a" />  

Suppose we have Grev: {a <- b <- d <- e}, and there's a path from e to a Grev. This corresponds to the G: {a -> b -> d -> e}, and there's a path from a to e.  
<img width="600" alt="image" src="https://github.com/user-attachments/assets/81bef819-60a7-43f0-a230-d6ce4366a726" />  
We compute the path using BFS at G and Grev. The strongly connected component of G containing v is given by Cv = Rv ∩ R′v. Therefore, the strong connected component are: {a, b, c}, {d, e, f}, {g, h}. Finding all strongly connected components in this way is O(V) traversals of the graph. Each of these traversals is a BFS requiring O(V + E) time. The overall time complexity is O(V(V + E)). 

We can improve this:  
<img width="400" alt="image" src="https://github.com/user-attachments/assets/ca884415-0b25-463f-ba45-4e46c8ebe3fc" /><img width="400" alt="image" src="https://github.com/user-attachments/assets/85a06cfc-5432-4a9c-a707-16d088e4c653" />

We continue our solution to the tsunami warning problem. Now we have the set of super-towers (strongly connected components), and we know for each super-tower (SCC) which others it can activate. The condensation graph shows the relationship between different supertowers. Our task is to decide which super towers need a sensor in order to propagate. 

The correct greedy strategy is to only place a sensor in each super tower without incoming edges in the condensation graph, as they will not be directly or indirectly activated. 

Proof Correctness:
These supertowers cannot be activated by another supertower, so they each require a sensor. This shows that there's no solution using fewer sensors. We still have to prove that this solution activates all supertowers. Consider a supertower with one or more incoming edges. Follow any of these edges backward, and continue backtracking in this way. Since the condensation graph is _acyclic_, this path must end at destinations, i.e. some supertower without incoming edges. The sensor placed here will then activate all supertowers along our path. Therefore, all supertowers are activated as required.  

<img width="400" alt="image" src="https://github.com/user-attachments/assets/18385904-6263-4eae-8476-22dc0d230f16" /><img width="400" alt="Screenshot 2025-03-23 at 8 23 55 pm" src="https://github.com/user-attachments/assets/0c6f4c14-4ed9-46ac-a18b-b10a32e66007" />  


#### Example 9: Topological Sorting  
In the special case where a directed graph G has no cycles (e.g.
when G is a condensation graph), then we can order the vertices of
G by the following rule: Let G = (V, E ) be a directed graph, and let n = |V |. A
topological sort of G is a linear ordering (enumeration) of its
vertices σ : V → {1, . . . , n} such that if there exists an edge
(v, w ) ∈ E then v precedes w in the ordering, i.e., σ(v) < σ(w).

A directed acyclic graph permits a topological sort of its vertices.
Note that the topological sort is not necessarily unique, i.e., there
may be more than one valid topological ordering of the vertices.  

<img width="600" alt="image" src="https://github.com/user-attachments/assets/c0c33635-2aba-4cb8-a67d-33021e29701b" />  

How do we topologically sort the vertices?  

We have:  
- a list L of vertices, initially empty,
- an array D consisting of the in-degrees of the vertices, and
- a set S of vertices with no incoming edges.

While set S is non-empty, select a vertex u of with D[u] = 0 in the set.  
- remove it from the set S and append to L
- Then for every outgoing edge e = (u, v) from this vertex, remove the edge from the graph, and decrement D[v] accordingly.
	- if D[v] is now zero, insert v into S.
- If there are no edges remaining, then L is a topological ordering.
Otherwise, the graph has a cycle.  

<img width="600" alt="image" src="https://github.com/user-attachments/assets/007b06cd-879f-4bc7-9742-dccfe5be4d53" />  

This algo runs O(V + E) time overall, that is, linear time. Once again, we can run this algorithm “for free” as it is asymptotically no slower than reading the graph. In problems involving directed acyclic graphs, it is often useful to start with a topological sort and then think about the actual problem!  

#### Single Source Shortest Paths Example 10: Dijkstra
A directed graph G = (V, E) with non-negative weight w(e), and a designated source vertex s ∈ V. We will assume that for every v ∈ V there is a path from s to v. Task: find the weight of the shortest path from s to v for every v ∈ V.

To find shortest paths from s in an undirected graph, simply replace each undirected edge with two directed edges in opposite directions. There isn’t necessarily a unique shortest path from s to each vertex.

Observe that this problem makes the following assumptions:
- There is exactly one source vertex s.
- The graph is directed.
- The edge weights are non-negative.

This task is accomplished by a greedy algo called Dijkstra. 
```
Algorithm Outline

Maintain a set S of vertices for which the shortest path weight has been found, initially empty. S is represented by a boolean array.
For every vertex v, maintain a value d_v which is the weight of the shortest ‘known’ path from s to v, i.e. the shortest path using only intermediate vertices in S. Initially d_s = 0 and d_v = ∞ for all other vertices.

At each stage, we greedily add to S the vertex v ∈ V \ S which has the smallest d_v value. Record this value as the length of the shortest path from s to v, and update other d_z values as necessary.
```

In this course, we keep exainme on dijkstra on:
- Correctness: why is it correct to always add the vertex outside S with the smallest d_v value?
- Update: when v is added to S, for which vertices z must we update d_z, and how do we do these updates?
- Data Structure: What data structure should we use to represent the dv values?
- Time complexity. What is the time complexity of this algorithm, and how is it impacted by our choice of data structure?

Claim: Suppose v is the next vertex to be added to S. Then dv is the length of the shortest path from s to v. 

Proof of Correctness: 
- dv is the length of the shortest path from s to v using only intermediate vertices in S. Let’s call this path p.
- If this were not the shortest path from s to v , there must be some shorter path p' which first leaves S at some vertex y before later reaching v.
- Now, the portion of p' up to y is a path from s to y using only intermediate vertices in S.
- Therefore, this portion of p' has weight at least dy.
- Since all edge weights are non-negative, p′ itself has weight at least dy.
- But v was chosen to have smallest d -value among all vertices outside S!
- So we know that dv ≤ dy , and hence the weight of path p is at most that of p'.
- Therefore, dv is indeed the weight of the shortest path from s to v.
- <img width="500" alt="image" src="https://github.com/user-attachments/assets/1650f22c-d3e3-4652-9592-4023cef6d307" />

Claim: Earlier, we said that when we add a vertex v to S, we may have to update some dz values. What updates could be required?

Proof of Update:
- If there is an edge from v to z with weight w (v , z), the shortest known path to z may be improved by taking the shortest path to v followed by this edge. Therefore we check whether dz > dv + w (v, z), and if so we update dz to the value dv + w (v, z). As it turns out, these are the only updates we should consider!

Claim: If dz changes as a result of adding v to S, the new shortest known path to z must have penultimate vertex v, i.e., the last edge must go from v to z.

Proof:
- Suppose that adding v to S allows for a new shortest path through S from s to z with penultimate vertex u ̸= v.
- Such a path must include v , or else it would not be new. Thus the path is of the form p = s → · · · → v → · · · → u → z.
- Since u was added to S before v was, we know that there is a shortest path p' from s to u which does not pass through v.
- Appending the edge − → uz to p' produces a path through S from s to z which is no longer than p.
- This path was already a candidate for dz, so the weight of p is greater than or equal to the existing dz value.
- This is a contradiction, so the proof is complete.
- <img width="500" alt="image" src="https://github.com/user-attachments/assets/dd399149-8199-45ed-8d7b-0b427dbba02b" />

Now, we are ready to consider data structures to maintain the dv values. We need to support two operations: find the vertex v ∈ V \ S with smallest dv value, and for each of its outgoing edges (v, z), update dz if necessary.

We start on the simplest Data Structure: Array. This gives us some limitations such as the time complexity $O(n^2 + m)$ -> in simply graph is $O(n^2)$.

Thinking the two main operations:
- find the vertex v ∈ V \ S with smallest dv value, and
- for each of its outgoing edges (v , z), update dz if necessary.

So far, we have done the first operation in O(n) using linear search. We not just a pure find minimum because we have to skip over vertices already in S. Instead, when we add a vertex v to S, we could try deleting d_v from the data structure altogether:
- find minimum,
- delete minimum,
- update any.

Thus augmented heap is more suitable. We will use a heap represented by an array A[1..n]; the left child of A[j] is stored in A[2j] and the right child in A[2j + 1]. Every element of A is of the form A[j] = (i, di) for some vertex i. The min-heap property is maintained with respect to the d-values only. We will also maintain another array P[1..n] which stores the position of elements in the heap. Whenever A[j ] refers to vertex i, we record P[i ] = j, so that we can look up vertex i using the property A[P[i]] = (i, di).

<img width="500" alt="image" src="https://github.com/user-attachments/assets/4691fc85-4d94-47c9-9f1b-b26eab06e688" />

In this way, we can store things in an array while implicitly maintains the structure of the full binary tree, aka. a heap. 

The first table is a regular heap and the second table just a position array that allow us to quickly lookup which entry of that array corresponds to some vertex. In the heap array, we simply list of each layer of the tree from left to right, so 1 is the root of the tree that has vertex 7 with distance 1, forming the first col at {A[j], j}.  
<img width="400" alt="image" src="https://github.com/user-attachments/assets/5d408e80-5cd9-4c4d-ade5-3ec368c1ff44" /><img width="400" alt="image" src="https://github.com/user-attachments/assets/4cff71ff-23fb-4c07-b370-fa17048b700d" />  

Updating the d-value of vertex i is now an O(log n) operation. 
First, look up vertex i in the position array P. This gives us P[i], the index in A where the pair (i, di ) is stored. 
Next, we update di by changing the second entry of A[P[i]]. Finally, it may be necessary to bubble up or down to restore the min-heap property. 
In this algorithm, d-values are only ever reduced, so only bubbling up is applicable.

<img width="650" alt="image" src="https://github.com/user-attachments/assets/1ea80f38-2f50-4e71-a965-5db5daa39c71" />  

Time Complexity: Each of n stages requires a deletion from the heap (when vertex is added to S), which takes O(log n) many steps. Each edge causes at most one update of a key in the heap, also taking O(log n) many steps. Thus, in total, the algo runs in time O((n + m) log n). But since we know $m >= n-1$ as there's a path from v to every other vertex, we can simply to O(m log n).  


#### Example 11: Minimum Spanning Tree
<ins>Definition: G = (V, E ) is a connected graph and each edge e in E has a non-negative length. A spanning tree T of G is any tree which is a subgraph of G on the same vertex set V.
The weight of a tree T is the sum of all edge lengths in T. A minimum spanning tree T of a connected graph G is a spanning tree of G with the smallest weight.  

Let say we have S and S's compliments outside. Then of all the edges crossing the boundary between S and not S, the cheapest of the edge e must belong to every single minimum spanning tree. Why e' (the greater weight) is not chosen?  
<img width="600" alt="Screenshot 2025-03-27 at 4 52 44 pm" src="https://github.com/user-attachments/assets/d5374b46-06d2-4541-8dd9-06b61791e6e8" />  

<img width="400" alt="image" src="https://github.com/user-attachments/assets/acf6d6a1-bd17-4d01-8f57-e72d435f1807" /><img width="400" alt="image" src="https://github.com/user-attachments/assets/3e5333b5-ca6a-494b-9214-35295e124698" />  

See the proof of Prim's and Kruskal's algo at week 3 slides.


## Mod 4 - The Flow Networks
```
Definition:

A flow network G = (V, E) is a directed graph in which each edge e = (u, v) ∈ E has a positive integer capacity c(u, v) > 0.

There are two distinguished vertices: a source s and a sink k;
no edge leaves the sink and no edge enters the source. 
```
<img width="400" alt="image" src="https://github.com/user-attachments/assets/6f7abe88-e4cd-4136-aa57-018c6de32d81" />

A flow in G is a function f : E -> [0, ∞], f(u, v) >= 0, which satisfies:
1. **Capacity constraint**: for all edges e(u, v) ∈ E we require $\[ f(u, v) <= c(u, v)\]$. i.e. the flow through any edge does not exceed its capacity.
2. **Flow conservation**: for all vertices v ∈ V - {s, t} we require $\sum_{(u,v) \in E} f(u,v) = \sum_{(v,w) \in E} f(v,w)$ i.e. the flow into any vertex (other than the source and the sink) equals the flow out of that vertex.  
<img width="600" alt="image" src="https://github.com/user-attachments/assets/e6c00a5b-e55c-4f0e-bd58-6e285f8e80f4" />  

```
Integrality Theorem

If all capacities are integers (as we assumed earlier),
then there's a flow of max value such that f(u, v) is an integer for each edge (u, v) ∈ E.

This means that there's always at least one solution entirely in integers.
We will only consider integer solutions hereafter. 
```  

#### Example 1: Maximum Flow: Navie algo
<img width="600" alt="image" src="https://github.com/user-attachments/assets/bb40a267-ce6f-42b5-b8ed-9c04f68abf79" />  

The pictured flow has a value of 19 units, and it doesn't appear possible to send another unit of flow. The biggest flow is indeed 23 units.
<img width="600" alt="image" src="https://github.com/user-attachments/assets/94b7b089-9ce0-4731-901e-494e63a33801" />  

This example demonstrates that the most basic greedy algorithm - send one unit of flow at a time along arbitrarily chosen paths - does not always achieve the maximum flow. In the first attempt we sent 19 units of flow to vertex v3, only to send four units back to v2, which is incorrect. It would have been better to send those four units of flow to t directly, but this may not have been obvious at the time this decision was made. We need a way to correct mistakes! We would like to send flow from v2 back to v3 so as to “cancel out” the earlier allocation.

Given a flow in a flow network, the _residual flow network_ is the network made up of the leftover capacities. (Noted that initially, the G and G' are the same as there's no flowing.)

<img width="600" alt="image" src="https://github.com/user-attachments/assets/122ecfae-689c-4a7e-a902-d7bbbe3b4a99" />  

For example, the pic on the RHS is the residual flow network encoded for each pair how much more flow can we send through the corresponds. The pipe from s to v1 has a capacity of 16 and we use 11 currently, which means we can send another 5 units of flow from s to v1. However, we may also realise at some point those 11 of flow being sent to v1 is a mistake, therefore, we should also include the residual edge (the back edge) which has the capacity of 11 saying that of those 11 units flows being send forwarded we might later need to send any of those 11 backward to cancel out. 

<img width="600" alt="image" src="https://github.com/user-attachments/assets/be29fef0-1ef9-4160-84b7-40b898f77371" />  

Suppose the original flow network has an edge from v to w with capacity c_1 and flow f_1 units, and an edge from w to v with capacity c_2 and flow f_2 units. What are the corresponding edges in the residual graph? And how much flow can be sent from v to w (and vice versa)?

The forward edge allows c_1 - f_1 additional units of flow, and we can also send up to f_2 units to cancel the flow through the reverse edge. Thus we create edges from v to w with capacity c_1 - f_1 + f2 and similarly from w to v with capacity c_2 - f_2 + f1. 

_An augmenting path is a path from s to t in the residual flow network. We can achieve this path using any algo (DFS/BFS)._  
The residual flow network below corresponds to the earlier example of a flow of value 19 units. An augmenting path is pictured in red. 
<img width="273" alt="image" src="https://github.com/user-attachments/assets/c430dcba-94d7-467d-aa5f-9e9f99cb73b6" />  

The capacity of an augmenting path is the capacity of its bottleneck (i.e. the smallest value on the path). For example 4 here. Now we need to re-calculate the residual capacities to: 1, 0, 1. (Suppose we now sending f=4 from s to t).  The edge with residual capacity becomes 0 is called **Saturated**, which means we need to remove them. Then we update the non-zero residual capacity. Now we enter the next iteration starting by finding a new augmenting path, and if we can't find an augmenting path from s to t, we terminate the program. 

For example:
<img width="600" alt="image" src="https://github.com/user-attachments/assets/6c9ab8f3-c3ac-4309-8c6d-974612d48ba9" />  

Suppose we have an augmenting path(this is arbitrary) of capacity f, including an edge from v to w. We should:
- Cancel up to f units of flow being sent from w to v,
- add the remainder of these f units to the flow being sent from v to w,
- increase the residual capacity from w to v by f, and
- reduce the residual capacity from v to w by f.

Note that this naive algo may fail something and it **is not guaranteed** to always find the max flow. As sometimes it comes up with 'blocking flow' and this algo is not allowed to 'regret' the path it has chosen. 

#### Ford-Fulkerson Algorithm
Similarly, FF algorithm also needs a residual graph. 

It has one more step when doing iteration: add a backward('reverse') path. For example:  
<img width="600" alt="image" src="https://github.com/user-attachments/assets/ce000725-27cf-4cd0-a305-24eccd552347" />
<img width="600" alt="image" src="https://github.com/user-attachments/assets/8afcddea-e03e-4ffe-922f-7dc606be24b7" />
<img width="600" alt="image" src="https://github.com/user-attachments/assets/4878c21f-dfb7-443f-b4ff-d85bdabc3406" />

At the end of the procedure:  
<img width="600" alt="image" src="https://github.com/user-attachments/assets/511e148e-8c2e-4d6f-8e6d-a76c17f5e82b" />  
The max flow is 5.  

<img width="600" alt="image" src="https://github.com/user-attachments/assets/13530226-2ba5-4ca5-b81d-4e81a2faccc7" />  

Time complexity:  
- The number of augmenting paths can be up to the value of the max flow, denoted as f.
- Each argumenting path is found at O(E) time by DFS/BFS.
- The overall time complexity is O(f * E). It's exponential compared with the same of input, which is very slow.  

<img width="400" alt="image" src="https://github.com/user-attachments/assets/89ca10c6-d3fb-4c03-851b-32e022e0a9a5" />
<img width="400" alt="image" src="https://github.com/user-attachments/assets/50b22ff2-91d5-48ed-be84-f43bd0b5fd8b" />  

#### Edmonds-Karp Algorithm
A special case of FFA. They are almost the same except EKA uses the shortest (argumenting) path from source to sink. (Apply weight 1 to all edges of the residual graph).

Time Complexity: $O(V * E^2)$
#### Minimum Cut 

Suppose we have Graph G = (V, E), source and sink ∈ V. 

At first we run any algo that compute the final residual graph and remove edges that's not coming from s to t. The S-T Cut splits V into two subsets: S and T, where:
- S <- all the vertices that has finite distances (reachable from s).
- T <- all other remaining vertices (unreachable from s).

They also have these relationships:
- S ∪ T = V and S ∩ T = ∅ (intersection is empty)
- s ∈ S, t ∈ T

The capacity c(S, T) = sum of weights of the edges leaving S and forwarding to T.

```
The definition of Min-Cut is the S-T cut (S, T) that has the minimise capacity.
```  

<img width="600" alt="image" src="https://github.com/user-attachments/assets/ffee554d-b4dd-461a-94ce-444c08d1e964" />  

**Max-Flow Min-Cut Theorem**  
- In a flow network, the maximum amount of flow from s to t is equal to the capacity of the minimum s-t cut.
- In short, the amount of max-flow = capacity of min-cut
<img width="600" alt="image" src="https://github.com/user-attachments/assets/5e93573f-7d72-444f-93e8-0c785bb260cd" />  

Proof of correctness:
- Assume that the Ford-Fulkerson algorithm has terminated, so there no more augmenting paths from the source s to the sink t in the last residual flow network.
- Define S to be the source s and all vertices u such that there is a (directed) path in the _residual flow network_ from the source s to that vertex u.
- Define T to be the set of all vertices for which there is no such path.
- Since there are no more augmenting paths from s to t, clearly the sink t belongs to T.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/3c9e2e3f-18ed-4bab-a097-8d0bcc11df2e" />  

Note: flow network also requires proof of correctness. For detailed check 7.01. 

#### Example 2: Movie Rental
Instance: Suppose you have a movie rental agency.

You have k movies in stock, with mi copies of movie i. There are n customers, who have each specified which subset of
the k movies they are willing to see. However, no customer can rent out more than 5 movies at a time.

Task: Design an algorithm which runs in O(n^2 k) time and dispatches the largest possible number of movies.  

We construct a flow network with:
- source s and sink t
- a vertex $u_i$ for each customer i and a vertex $v_j$ for each movie j,
- for each i, an edge from s to $u_i$ with capacity 5,
- for each customer i, for each movie j that they are willing to see, an edge from $u_i$ to $v_j$ with capacity 1.
- for each j, an edge from $v_j$ to t with capacity $m_j$.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/1c6bbd68-ed43-467d-823f-4e500b681a60" />  

The max flow in this network is exactly the max amount of movies that can be dispatched. Consider a flow in this graph, each customer-movie edge has capacity 1, so we will interpret a flow of 1 from $u_i$ to $v_j$ as assigning movie j to customer i. 

Each customer only receives movies that they want to see. By flow conservation, the amount of flow sent along the edge from s to $u_i$ is equal to the total flow sent from $u_i$ to all movie vertices $v_j$, so it represents the number of movies received by customer i. Again, the capacity constraint ensures that this does not exceed 5 as required. Similarly, the movie stock levels $m_j$ are also respected.

Therefore, any flow in this graph corresponds to a valid allocation of movies to customers.  
<img width="600" alt="image" src="https://github.com/user-attachments/assets/7a0eded9-39f0-4732-b781-ad500ee932b1" />  

#### Example 3: Cargo Allocation
The storage space of a ship is in the form of a rectangular grid of cells with n rows and m columns. Some of the
cells are taken by support pillars and cannot be used for storage, so they have 0 capacity.

You are given the capacity of every cell; the cell in row i and column j has capacity C (i, j). To ensure the stability of the ship, the total weight in each row i must not exceed $C_r(i)$ and the total weight in each column j must not exceed $C_c(j)$. 

Task: Design an algorithm which runs in O((n + m)(nm)2) time and allocates the maximum possible weight of cargo without exceeding any of the cell, row or column capacities.

At first, we map this scenario into a graph problem. Let's constrct a flow network with:
- source S and sink T
- Vertex $r_i$, for each row i and a vertex $c_j$ for each column j
- for each i, an edge from s to $r_i$ with capacity $C_r(i)$
- for each cell (i, j) which is not a pillar, an edge from $r_i$ to $c_j$ with capacity $C(i, j)$.
- for each j, and edge from $c_j$ to t with capacity $C_c(j)$.

<img width="400" alt="image" src="https://github.com/user-attachments/assets/d71a7c65-52c9-4318-97c5-cc91104490a6" /><img width="400" alt="image" src="https://github.com/user-attachments/assets/9a831915-40c3-4884-9766-889186ea35f7" />  

Overall time complexity using EKA. We have n + m + 2 vertices and up to nm + n + m edges. By only taking the dominated terms, we get $O(V + E^2) = O((n + m + 2)(nm)^2)$. 


#### Bipartise and Matchinges
```
A graph G = (V, E) is said to be bipartite if its vertices can be divided into two disjoint sets A and B such that every edge e ∈ E has one endpoint in the set A and the other in the set B.

A matching in a graph G = (V, E) is a subset M ⊆ E such that each vertex of the graph belongs to at most one edge in M. A maximum matching in G is a matching containing the largest possible number of edges. 
```

#### Example 4: Max Bipartite Matching
Given a bipartite graph G, find the size (i.e. the number of pairs matched) in a maximum matching. How can we turn a Maximum Bipartite Matching problem into a Maximum Flow problem?

Create two new vertices, s and t (the source and sink). Construct an edge from s to each vertex in A, and from each vertex in B to t. Orient the existing edges from A to B. Assign capacity 1 to all edges. 

<img width="500" alt="image" src="https://github.com/user-attachments/assets/f59704ca-3239-432e-882e-d38ac3241746" />  

<img width="500" alt="image" src="https://github.com/user-attachments/assets/9cfb34bd-ac12-495e-9e3a-d5e8f0e8864f" />

This is how the residual graph maps solutions:  
<img width="400" alt="image" src="https://github.com/user-attachments/assets/a116a505-06cf-409a-904e-4ef72ed4b541" /><img width="400" alt="image" src="https://github.com/user-attachments/assets/e11256e6-ba05-4243-9d0f-b716dcba1771" />  

And if there's an augmenting path involving backward edges:  
<img width="600" alt="image" src="https://github.com/user-attachments/assets/01d73d20-86db-40e1-bf84-550a31bc0f70" />

We had sent flow from a3 to b3, and now we sending flow backward from b3 to a3. This cancels out the edge a3 to b3 in the original graph. 

Eventually, we get the final residual graph that has no augmenting path.

## Mod 5 - Dynamic Programming
The main idea to to solve a large problem recursively by building from (carefully chosen) subproblems of smaller size. Notably, the subproblems are OVERLAPPING in DP, whereas they're disjoint at DAC. It can be solved from top-to-down or from down-to-top. 

```
Optimal Substructure Property

We must choose subproblems in such a way that optimal solutions to subproblems can be combined into an optimal solution for full problem,
and the same subproblem occur several times in the recursion tree.

When we choose a subproblem, we store the result so that subsequent instances of the same subproblem can be answered by just looking up a value in a table.
```

In short, it try to avoid repeated calculations. 

Three key elements:
1. Define the subproblems
2. A recurrence relation, which determines how the solutions to smaller subproblems are combined to solve a larger subproblem, and
3. Any base cases (not every subproblem need to solve by recurrence).

**Understanding the original problem is crucial**
It may be one of our subproblems, or it may solved by combining results from several subproblems, in which case we must also describe this process. 

#### Recursion Revision
There are three key elements of recursion:
1. Clearly define what the function is meant to do. For example:
   ```
   	int f(int n) {

	}
   ```

2. Determine the base case (i.e. when recursion ends)
   ```
   	int f(int n){
	    if(n <= 2){
	        return 1;
	    }
	}
   ```

3. Find the recursive relationship (equivalent expression). This step is about **reducing** the problem size step-by-step.
   ```
   	int f(int n){
	    if(n <= 2){
	        return 1;
	    }

		return f(n-1) + f(n - 2);
	}
   ```

We have $f(n) = f(n-1) + f(n-2)$. It's base case is $f(0) = f(1) = 1$. For $f(7)$, we need to know two terms: $f(5)$ (which needs another f4 and f3)and $f(6)$ (which needs another f5 and f4) and so on. This is the way we use to build recursion tree. However, it is not efficient with time complexity of $O(2^n)$.

Recalling our Dp, we can store values in a table, so that next time we need to lookup f(5) directly, instead going directly into the recursion. This lets us trim the entire right subtree down to a single node $f(5)$.  
<img width="400" alt="image" src="https://github.com/user-attachments/assets/b6fac0c8-9ad4-43c0-81a0-59ed2e37f7cd" />  

#### Example 1 Linear Recurrence: Longest Increasing Subsequence
Instance: Given a sequence of n real numbers $A[1...n]$.

Task: determine a subsequence of maximum length, in which the values in the subsequence are strictly increasing. 

A natural choice for the subproblems is as follows: for each $1 <= i <= n$, let P(i) be the problem of determining the length of the longest increasing subsequence of $A[1..i]. 

However, it is not immediately obvious how to relate these subproblems to each other.

Usually, we look for $Q(i)$, the problem of determining opt(i), the length of the longest increasing subsequence of $A[1..i]$ that includes last element $A[i]$. 

Note that the overall solution is recovered by taking the best of the answers to all the subproblems, i.e., the longest increasing subsequence ending at any index.

We will try to solve $Q(i)$ by extending the sequence which solves $Q(j)$ for some $j < i$. 
<img width="600" alt="image" src="https://github.com/user-attachments/assets/167fc634-6b0a-4f74-88dd-bdbae245ebf7" />  

Supposing we have already solved all of these earlier subproblems, we now look for all indices $j < i$ such that $A[j] < A[i]$.
Among those we pick m so that opt(m) is maximum, and extend that sequence with $A[i]$. This forms the basis of recurrence. When at $i=1$ is our base case as there are no previous indices to consider. By doing so, we can filled up this table, with the $opt(i)$ refers to the max out of all the previous opt values where $j$ and $A[j]$ are both less than i:  
<img width="600" alt="image" src="https://github.com/user-attachments/assets/e71c263e-4e40-4502-9015-977634ae839d" />  

The total time complexity is $O(n^2)$. We have $n$ subproblems each taking $O(n)$.  

Noted: if the question is asking for returning the longest subsequence instead, we could use an array or hashtable to record the $prev(i)$ in our table. After all subproblems have been solved, the longest increasing subsequence can be recovered by backtracking through the table.  

#### Example 2: Maximum Earns
<img width="600" alt="image" src="https://github.com/user-attachments/assets/acb0a559-4c35-4b44-954a-e2ed50bea05c" />  

Given this chart, it has a list of sorted tasks. For each task $i$ consists of: starting time $s_i$, finishing time $f_i$ and earnings $w_i$. No overlapping tasks are allowed. Your task is to find a solution that maximises the earnings.

Suppose for each $1 <= i <= n$, $OPT(i)$ is the optimal earnings when up to $ith$ task. 

Let's imagine this task into a tree structure, and we choose the best:
- Choose (the current task): $w_i + OPT(prev[i])$
- No Choose (the current task): $OPT([i-1])$

The recurrance is then: $DP(i) = max(DP(i+1), w_i + DP(prev(i)))$    
<img width="600" alt="image" src="https://github.com/user-attachments/assets/fc41fc32-c584-4194-a5ee-92a267dd0d29" />  

The overall time complexity is $O(log n)$  

#### Example 3: Making Change
<img width="600" alt="Screenshot 2025-04-09 at 4 09 31 pm" src="https://github.com/user-attachments/assets/fe8cbafb-125f-4ed4-bd31-cd48bfb26de6" />  

We will try to find the optimal solution for not only C, but every amount up to C. Assume we have found optimal solutions for every amount $j < i$ and now want to find an optimal solution for amount i. We consider each coin vk as part of the solution for amount i, and make up the remaining amount $i − v_k$ with the previously computed optimal solution.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/f9c6ba64-59ef-431d-b72f-b5fc67cd42ac" />  

Among all of these optimal solutions, which we find in the table we are constructing recursively, we pick one which uses the fewest number of coins. Supposing we choose coin m, we obtain an optimal solution $opt(i)$ for amount i by adding one coin of denomination $v_m$ to $opt(i − v_m)$. If the amount is zero, then the solution is trivial: use no coins. Hence, these are the solutions:
<img width="400" alt="image" src="https://github.com/user-attachments/assets/c80a3ad4-0c0f-4da0-90e9-ccbb807956e8" /><img width="400" alt="image" src="https://github.com/user-attachments/assets/8773cc5a-4283-414d-8695-6b4c4fcb79f7" />  

The overall time complexity is $OPT(nC)$. Each of C subproblems is solved in $O(n)$ time. This is considered to be a **pseudopolynomial time algorithm**, not a polynomial time algorithm. We always determine whether an algorithm runs in polynomial time with reference to the length of the input.
In this case, the input is n, the coin values and C, so the number of bits required to communicate it is O(n log C).
On the other hand, the algorithm runs in O(nC), so the running time is exponential in the length of the input. 
Noted that small changes in the amount of input required to communicate with C cause large changes in runtime.  

Proof of Correctness:  
<img width="400" alt="image" src="https://github.com/user-attachments/assets/40587d8c-77fc-48ed-a28b-bfaee375d203" /><img width="400" alt="image" src="https://github.com/user-attachments/assets/c0bd408a-f205-489d-b20c-ab40ddd5f660" />  









