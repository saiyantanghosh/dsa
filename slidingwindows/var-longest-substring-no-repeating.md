# Problem
Length of Longest Substring without any Repeating Character
# Requirements
* input is string 
* contains ASCII or unicode
* empty string retrun 0 
* single string 1
* size - 10^7
* return length substring 
* return first occurrance of substring
* input is one word in string , not multiple words
# Example
""/null=> 0
"a"=> 1
abca => 3
abcab => 3
# Approaches
## Brute force
* consider all substring and two for loops 
* create a maxLength = Integer.MIN
* per substring build a duplicate boolean array
* if any duplicate chars present then ignore 
* get length and perform Math.max(current.length,maxLength);
* return max length
## Pattern Recongnization
* substring - contiguous array
* variable window
  - contiguous valid region
  - it can expand and shrink based on next string 
  - maintain incrementally
## Approach 1 
```java

public class StringUtils{
    private int getLongestSubstring(String input){
        if(input==null || input.length() ==0){
            return 0;
        }
        int lengthOfInput =  input.length();
        if(lengthOfInput==1){
            return 1;
        }
        int right=1;
        int left=0;
        int maxLength = 1;
        while(right<lengthOfInput){
            char currentChar = input.charAt(right);
            String current = input.substring(left,right);
           if(current.indexOf(currentChar)!=-1){
            left++;
            
           }else{
            maxLength = Math.max(maxLength,right-left+1);
            right++;
            
           }
        } 
        return maxLength;
    }
}

```
* this version is traversing array multiple times - not optimal O(N^2)

## Approach2  
* using int seen[] - holding last seen array
```java

public class StringUtils{
    private int getLongestSubstring(String input){
        if(input==null || input.length() ==0){
            return 0;
        }
        int lengthOfInput =  input.length();
        if(lengthOfInput==1){
            return 1;
        }

        int left=0;
        int maxLength = 0;
        int[] lastSeen = new int[128];
        Arrays.fill(lastSeen, -1);

        for (int right = 0; right < input.length(); right++) {
            char currentChar = input.charAt(right);
            // prevent moving left backward
            if(lastSeen[currentChar] >= left){
                // one step AFTER previous duplicate
               left = lastSeen[currentChar]+1;
            }
            lastSeen[currentChar]=right;
            maxLength = Math.max(
                    maxLength,
                    right - left + 1
            );
        } 
        return maxLength;
    }
}

```
* T.C. - O(N) , S.C. - O(128) - O(1)