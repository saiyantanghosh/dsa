# Reverse String with no extra space 
## Constraints
* input string seperated by spaces

## Approach1 - extra space using Stack
```java
import java.util.*;
class Main {
    public static void main(String[] args) {
    String input = "my name is saiyantan";  
    String[] words = input.trim().split("\\s++");
    Deque<String> stack = new ArrayDeque<>(); 
    reverseWords(words,stack);
    StringBuilder reversedWord = new StringBuilder();
    while(!stack.isEmpty()){
        reversedWord.append(stack.pop());
        reversedWord.append(" ");
    }
    System.out.print(reversedWord.toString());
    }
    private static void reverseWords(String[] words,Deque<String> stack){
        for(String word: words){
            stack.push(word);
        }
    }
 
}
```
## Approach2 - No extra space same input string
* reverse String
* Reverse each words till string 

```java 

class Main {
    public static void main(String[] args) {
    String input = "my name is saiyantan";  
    char[] charOfWords = input.toCharArray();
    int noOfWords = charOfWords.length;
    reverse(charOfWords,0,noOfWords-1);
    reverseWords(charOfWords,noOfWords);
    System.out.println(charOfWords);
    }
    private static void reverseWords(char[] input,int noOfWords){
        int left =0;
        for(int right=0;right<=noOfWords;right++){
         
            if (right == noOfWords || input[right] == ' ') {
                reverse(input,left,right-1);
                left=right+1;
            }
        }
        
    }
    private static void reverse(char[] input, int left,int right){
        while(left<right){
            swap(input,left,right);
            left ++;
            right--;
        }
        
    }
    private static void swap(char[] inputChars, int i, int j){
        char temp = inputChars[i];
        inputChars[i] = inputChars[j];
        inputChars[j] =temp; 
    }
    
}



```
