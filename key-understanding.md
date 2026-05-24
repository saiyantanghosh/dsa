# Requirement Clarifications
* Input size
  - what is the upper bound of input
  - Influence data structure like if N=10^8 then int [] = 4 byte* 10^8 ~ 400 MB but hashmap has Key,Value,Object overhead mostly 32-64 byte per entry ~ 10^8 * 40 ~ 40 GB , so HM will be costly.
  - Arrays memory contiguous and cache local and HM bucket,nodes,resize overhead.
  - int[] vs ArrayList<Integer>
    - int[] - contiguous memory,cache local,faster iteration
    - Arraylist - storing reference , worse cache locality, autoboxing CPU overhead, Resizeing behavior (new capacity=old capacity* 1.5,allocate new array and copy elements)
* Duplicate
  - can input containts duplicates?
  - input is sorted? 
    - yes , we can use in-place deduplication,two pointer , binary search
    - no, hashset but memory use more
  - preserve order ?
    - Hashmap no ordering and heavy duplicate
    - use LinkedHashMap for ordering as insert( more memory overhead because of prev,next pointers)
    - use TreeMap - storted entry - O(log N) - redblack tree
  - if input is bound and dense(input size ~ range) then use BitSet ( 1 bit per value) rather than boolean[] (1 byte per value~ 8x). if sparse , then use hashset with less entries.
* for input string 
  - input type as string or char[]
  - case sensitive or ignored ?
  - ASCII or Unicode
    - ASCII - letters and digits,symbols - 128 - int[] freq = new int[128]; less space O(1)
    - Unicode - all languages,emojis.HashMap<Character,Integer> , more memory O(N)
  - Return substring or length  
* Negative values
  - all -ve, mixed of +ve and -ve , zero+ -ve 
  - Positive Numbers Often Enable
     - sliding window
     - greedy shrinking/expanding
     - two pointers
  - Negative Numbers Often Require
     - prefix sums
     - hashmap tracking
     - DP
* check Overflow 
   - java int range -2^31 to 2^31-1 if overflow use long
   - watch for Prefix Sum let say N=10^5 and num[i]=10^9 then total 10^14 beyound int 
   - Binary Search Mid Calculation use mid = left + (right - left) / 2;
   - Multiplication : long area = width * height;
   - Common DP Problems Needing Modulo
        - Unique Paths
        - Coin Change combinations
        - Distinct Subsequences
        - Decode Ways
        - Count BSTs
        - Catalan numbers
* Streaming
* can input mutate?
* Online vs offline
* Distribute consideration
* For Graphs, always clarify:
  - direction?
  - weights?
  - cycles?
  - connectivity?
  - scale?
* For Trees:
  - binary?
  - BST?
  - balanced?
  - duplicates?
  - mutable?
# Edge Cases
## Arrays
* empty
* single element
* duplicates
* all same
* max values
## Graphs
* disconnected
* cycle
* self-loop
## Strings
* unicode
* repeated chars
* large input

# Complexity Intuition Cheat Sheet
| Complexity | Rough Limit |
| ---------- | ----------- |
| O(N!)      | N ≤ 10      |
| O(2ᴺ)      | N ≤ 20      |
| O(N³)      | N ≤ 500     |
| O(N²)      | N ≤ 10⁴     |
| O(N log N) | N ≤ 10⁶     |
| O(N)       | N ≥ 10⁷     |

