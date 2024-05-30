# Competitive Programming Journal
This is where I track problems that took me significant thinking (> ~30 min) to solve and have interesting ideas worth remembering. Sorted most recent to oldest.

## [CF 1976D](https://codeforces.com/contest/1976/problem/D), 5/30/2024

**Summary:** Given a balanced bracket string of length N [1, 2e5], count the number of substrings such that the bracket string is still balanced after inverting the substring. Inverting means replacing '(' with ')', and vice versa.

**Solution:** Only invert substrings with an equal number of '(' and ')', otherwise we unbalance the string. In the balance array, if we invert some substring at [l, r], the balance at each index i in this subarray becomes bal[l - 1] - (bal[i] - bal[l - 1]) = 2 * bal[l - 1] - bal[i]. The bracket string will be valid if all balances are non-negative, so our answer is to count all pairs of positions with equal balance x such that all balances between the positions are at most two times x.

## [ABC 348F](https://atcoder.jp/contests/abc348/tasks/abc348_f), 5/15/2024

**Summary:** Given N [1, 2000], M [1, 2000], and N sequences of length M which we will refer to as a 2D grid A, where any element in A is an integer [1, 999], count the number of pairs (i, j) such that 1 <= i < j <= N and sequence i and sequence j are similar. Sequences are similar if there are only an odd number of indices 1 <= k <= M such that A[i][k] = A[j][k].

**Solution:** Keep N bitsets of size N to represent which sequences share an odd number of equal integers. Solve one column at a time. Keep 1000 bitsets of size N, where the i-ith bitset has bit j set if the j-th sequence is i in our current column. Then, iterate over each sequence and xor its answer bitset with the bitset of whatever integer is in its current column. This may seem expensive, but both the initialization and usage of bitsets are 64x quicker. Finally, sum up bits to get the answer (avoid double counting pairs or self pairs).

## [ABC 349F](https://atcoder.jp/contests/abc349/tasks/abc349_f), 5/9/2024

**Summary:** Given an array A of N [1, 2e5] integers that are [1, 1e16], and M [1, 1e16], count the number of subsequences in A such that the LCM of the subsequence is M.

**Solution:** Remove all integers from A which are not factors of M. Prime factorize M. Replace each integer in A with a bitmask representing which max power of prime factor it shares with M. Count all bitmasks, then do 2D DP over bitmasks to count the number of subsequences such that their bitwise OR is all 1s.

## [ABC 350G](https://atcoder.jp/contests/abc350/tasks/abc350_g), 5/7/2024

**Summary:** Given N [1, 1e5] and Q [1, 1e5], you need to process Q queries. Each query is either 1. add an edge between nodes u [1, N] and v [1, N] which are in different components, or 2. if a node is adjacent to both node u and v, print it, otherwise print 0. You are forced to process queries online.

**Solution:** In query type 2, there are only two possible cases. The node is a parent of both or the node is a parent of one and a child of the other. So, keep track of parents in the tree and the answer becomes trivial. To make the solution run in time, merge smaller trees into larger trees and you'll only need to recompute the parents in the smaller tree.

## [ABC 350E](https://atcoder.jp/contests/abc350/tasks/abc350_e), 5/5/2024

**Summary:** Given N [1, 1e18], X [1, 1e9], Y [1, 1e9], and A [2, 6], you can either pay X to replace N with floor(N / A), or pay Y to roll a die giving [1, 6] uniformly randomly where b is the result and replace N with floor(N / b). Calculate the minimum expected cost to make N become 0 with optimal decisions.

**Solution:** Let E(n) be the expected cost for n to reach 0. If we could only choose the random die, then E(n) = Y + (E(n) + E(n / 2) + ... + E(n / 6)) / 6, so E(n) = Y * 6 / 5 + (E(n / 2) + E(n / 3) + ... + E(n / 6)) / 5. Now, we don't need to worry about infinite loops, and we can just do a memoized recursion to calculate this. But, since we have the option to pay X or Y, our real E(n) is min(X + E(n / A), Y * 6 / 5 + (E(n / 2) + E(n / 3) + ... + E(n / 6)) / 5).

## [CF 1967D](https://codeforces.com/contest/1967/problem/D), 5/2/2024

**Summary:** Given arrays A and B of N [1, 1e6] integers that are [1, 1e6], find the minimum number of operations to make A non-decreasing. Each operation, you can choose any set of A[i] and replace each with B[A[i]].

**Solution:** Array B can be thought of as a functional graph. Create a way to determine whether it's possible to reach some node v from node u in this graph in <= k steps in O(1) using precomputed information like DFS order, subtree size, cycle size, etc. Then, binary search on the answer. Each binary search step can be verified in O(n) by working left to right, keeping track of the current desired integer, and incrementing it until it's reachable (or impossible).

## [CF 1967C](https://codeforces.com/contest/1967/problem/C), 5/2/2024

**Summary:** Given N [1, 2e5] integers, K [1, 1e9], and an array B which is the K-th Fenwick Tree of some unknown array A, find any possible A. 

**Solution:** In the K-th Fenwick Tree, find how much each node contributes to its ancestors. It turns out that for any node u, any node that is x levels above it adds it (x + K - 1 choose x) times. So, starting from nodes with the lowest bit and working up to the highest bit, traverse upwards and subtract the necessary amount from its ancestors. Then, you end up with A.

## [CF 1967B2](https://codeforces.com/contest/1967/problem/B2), 5/2/2024

**Summary:** Given N [1, 2e6] and M [1, 2e6], count the number of pairs (a, b) such that a is in [1, N], b is in [1, M], and b * gcd(a, b) is a multiple of a + b.

**Solution:** First, decompose a and b into coprime parts by letting g = gcd(a, b), so now we know a = a' * g and b = b' * g, where a' and b' are two coprime integers. a + b = g(a' + b') and we want g(a' + b') | b' *  g * g, so we want a' + b' | b' * g. No matter what b' is, a' + b' can not divide it, so we can simplify this to a' + b' | g. Therefore, we can just count the number of coprime integers a' and b' and let g be any multiple of a' + b'. One way to do this is iterating over each a in [1, N], each b in [1, M] while a * (a + b) <= N and b * (a + b) <= M, and adding min(N / a, M / b) / (a + b) to the answer if a and b are coprime. It is also easy to see a' is bounded by sqrt(N) and b' is bounded by sqrt(M), so this is fast enough.
