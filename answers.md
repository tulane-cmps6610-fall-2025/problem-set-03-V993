# CMPS 6610 Problem Set 03

## Answers

**Name:** Leonardo Matone

Place all written answers from `problemset-03.md` here for easier grading.

- **1b.** 
    - $W(N) = O(n)$, we always iterate through all of the list, 
    - $S(N) = O(n)$, this is iterative, and thereby the span is the same
- **1d.** 
    - $W(N) = O(n)$, we are still checking each item in the list
    - $S(N) = O(\log n)$, because we divide and conquer

- **1e.**
    - $W(N) = O(n)$, we are still checking each item in the list
    - $S(N) = S(\frac 23 n) + c$, as we only care about the maximum of the two splits. This is asymptotically dominated by $O(n\log n)$ as a leaf dominated recurrence  [CHECK THIS]

- **2a**


I'm not sure if I did a great job writing this SPARC code. This is what I tried to write: 
1. Iterate the dedup function over the input_list
2. Keep a hash of the seen items
3. update our result array *iff* the item is not in the hash.
4. Update the hash *iff* the item is not in the hash.

```
dedup ((hash, result), item) =
    if item ∈ hash then
        (hash, result) # Do not update the hash or the result arrays
    else
        (hash ++ item, result ++ item)

let (hash, result) = iterate dedup ((⟨ ⟩, [ ]), input_list) 
# I'm denoting the hash as "⟨ ⟩" to denote that it is a set, order insensitive, and the result as an ordered list with "[ ]".
in result
```

Operating under the assumption that hashing is $O(1)$:
$W(N) = O(n)$
$S(N) = O(n)$

- **2b**

The main idea here is the same, we just need to iterate over multiple lists instead of a single list. It's actually a little easier as we are now order-insensitive and can rely entirely on a hash. There is a cop-out solution here where we can simply concatenate the lists into a single list and use the same algorithm as before, but instead I'll come at this from scrath:
1. Iterate over each list in A_lists
2. For each list, iterate over each item and update the hash
3. Return the composite hash over all lists

```
updateHash (hash, item) =
    if item ∈ hash then
        hash
    else
        hash ++ item

processList (hash, A_list) =
    iterate addFromList hash A_list

let result = iterate processList ⟨ ⟩ A_lists
```

Operating under the assumption that hashing is $O(1)$:
$W(N) = O(N)$ (where n=num_lists and m=num_items, N=n*m)
$S(N) = O(N)$

- **2c**

Yes, iterate (used above) is most straightforward here and pretty plug-and-play (unless I have made a catastrophic mistake). But I can envision a few other (less straightforward) approaches. 

Reduce: Reduce is not ideal for this problem. Due to the nature of its recursion, it would be very difficult to keep track of all of the information necessary to determine whether or not there are duplicates in an unsorted list. Consider the case where we have a "1" at the start of the list and a "1" at the very end. When we come to the merge step, we could remove the 1 from the right side recursion, but it might be buried between other inputs. While it could be possible with enough contorting of reduce, it's nontrivial to do so- applying this operation in our problem and would be less useful. 

Filter/Tabulate/Map: These operations are not ideal for our application due to the sequential nature of our problem- applying a function independently over all indices of our input would not allow us to keep track of duplicates as easily.

Scan/prefix sum: Because it allows for intermediate computations, we would be able to implement this using scan and a similar hashing strategy. This would allow us to get the speedup of reduce, and also remain applicable to our problem.

- **3b.**

Recurrence/asymptotic expressions: 
$W(N) = W(n) + c: O(n)$
$S(N) = S(n) + c: O(n)$

- **3d.**

Recurrence/asymptotic expressions: 

$W(n) = 2W(n/2) + n: O(n)$
$S(N) = S(n/2) + c: O(\log n)$

- **3f.**

Recurrence/asymptotic expressions: 
$W(N) = 2W(n/2) + c: O(n)$
$S(N) = S(n/2) + c: O(\log n)$

