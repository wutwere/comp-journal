# Competitive Programming Journal
This is where I track problems that take significant thinking (> ~30 min) to solve and have interesting ideas worth remembering. Sorted most recent to oldest.

## [CF 1967D](https://codeforces.com/contest/1967/problem/D), 5/2/2024

**Summary:** Given arrays A and B of N [1, 1e6] integers that are [1, 1e6], find the minimum number of operations to make A non-decreasing. Each operation, you can choose any set of A[i] and replace each with B[A[i]].

**Solution:** Array B can be thought of as a functional graph. Create a way to determine whether it's possible to reach some node v from node u in this graph in <= k steps in O(1) using precomputed information like DFS order, subtree size, cycle size, etc. Then, binary search on the answer. Each binary search step can be verified in O(n) by working left to right, keeping track of the current desired integer, and incrementing it until it's reachable (or impossible).

## [CF 1967C](https://codeforces.com/contest/1967/problem/C), 5/2/2024

**Summary:** Given N [1, 2e5] integers, K [1, 1e9], and an array B which is the K-th Fenwick Tree of some unknown array A, find any possible A. 

**Solution:** In the K-th Fenwick Tree, find how much each node contributes to its ancestors. It turns out that for any node u, any node that is x levels above it adds it (x + K - 1 choose x) times. So, starting from nodes with the lowest bit and working up to the highest bit, traverse upwards and subtract the necessary amount from its ancestors. Then, you end up with A.

## [CF 1967B2](https://codeforces.com/contest/1967/problem/B2), 5/2/2024

**Summary:** Given N [1, 2e6] and M [1, 2e6], count the number of pairs (a, b) such that a is in [1, N], b is in [1, M], and b * gcd(a, b) is a multiple of a + b.

**Solution:** First, decompose a and b into coprime parts by letting g = gcd(a, b), so now we know a = a' * g and b = b' * g, where a' and b' are two coprime integers. a + b = g(a' + b') and we want g(a' + b') | b' *  g * g, so we want a' + b' | b' * g. No matter what b' is, a' + b' can not divide it, so we can simplify this to a' + b' | g. Therefore, we can just count the number of coprime integers a' and b' and let g be any multiple of a' + b'. One way to do this is iterating over each a in [1, N], each b in [1, M] while a * (a + b) <= N and b * (a + b) <= M, and adding min(N / a, M / b) / (a + b) to the answer if a and b are coprime. It is also easy to see a' is bounded by sqrt(N) and b' is bounded by sqrt(M), so this is fast enough.