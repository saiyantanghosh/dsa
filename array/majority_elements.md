# Problem
Given an arrays of integer find out majority element which appears more than n/2.
# Constraints 
* No of elements -10^9 <= nums[i] <= 10^9
* value inside array - 1 <= n <= 5 * 10^4
* There should be one majority element
* If more than one majority element , then return any of numbers
* Return -1 if no majority found 
* Empty or single return -1
# Approaches 
## Approach1 - Brute force
* build frequency map and get num
```java
import java.util.*;

class Solution {
    public int majorityElement(int[] nums) {
       Map<Integer,Integer> majority = new HashMap<>();
        for(int num:nums){
            majority.put(num,majority.getOrDefault(num,0)+1);
            if(majority.get(num) > nums.length / 2){
                return num;
            }
        }
        return -1;
    }

}

```
### Comprexity
* T.C. - O(n)
* S.C. - O(n)
## Approach2 - Sort and get mddile
* sort array and get middle index element as majority will expect to be present in middle. arr[n/2]
### Comprexity 
* T.C. - O(nlogn)
* S.C. - O(1)
## Approach3 - using Boyer-Moore voting algorithm
* get possible majority elements by cancelling each pair.
* This approach will provide majority candidate if any majority elemnts exists. If no majority elements exists then it will return arbitary element as majorty element. 
* We need go for 2nd pass to recheck and get recheck on majority element

## Psedo 
First pass
* if same element increment count 
* different element decrease count 
* if count is zero , set current elemnt as possible candidate

Second Pass
* after getting possible majority candidate , traverse and check frequency of this element and frequency should be > n/2

```java
class Solution {
    public int majorityElement(int[] nums) {
       int possibleCandidate = 0;
       int count=0;
       int noOfElement = nums.length;
        for(int num:nums){
            if(count==0){
               possibleCandidate= num;
            }
            if(num == possibleCandidate){
                count++;
            }else{
                count--;
            }
        }
        int frequency = 0;
        for(int num:nums){
            if(possibleCandidate==num){
               frequency++;
            }
            if(frequency>=noOfElement/2){
             return possibleCandidate;
            }
        }
        return -1;
    }

}

```
## Comprexity
* T.C. - O(n)
* S.C. - O(1)
