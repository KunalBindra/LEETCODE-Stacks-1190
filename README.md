# LEETCODE-Stacks-1190
Let's walk through the `reverseParentheses` method step by step:

### Initial Setup
1. **StringBuilder `sb`**: This will be used to build the final string without the parentheses.
2. **Deque `stack`**: This will be used to keep track of the indices of the opening parentheses.
3. **Map `pair`**: This will map each parenthesis to its corresponding matching parenthesis.

### Step 1: Identify Matching Parentheses
The first for-loop iterates over each character in the string `s` to find and pair matching parentheses:
- When an opening parenthesis `(` is found, its index is pushed onto the `stack`.
- When a closing parenthesis `)` is found, the index of its matching opening parenthesis is popped from the `stack`. Both indices are stored in the `pair` map to facilitate easy lookup.

### Step 2: Build the Final String
The second for-loop constructs the resulting string by iterating through the characters in `s`:
- The variable `d` controls the direction of iteration (forward or backward).
- If the current character is a parenthesis, the loop jumps to its matching parenthesis and reverses the direction of iteration by negating `d`.
- If the current character is not a parenthesis, it is appended to `sb`.

### Code Breakdown
Here's the code with some comments for clarity:

```java
class Solution {
  public String reverseParentheses(String s) {
    StringBuilder sb = new StringBuilder(); // To build the resulting string
    Deque<Integer> stack = new ArrayDeque<>(); // To track the indices of '('
    Map<Integer, Integer> pair = new HashMap<>(); // To store matching parentheses

    // Step 1: Pair the parentheses
    for (int i = 0; i < s.length(); ++i) {
      if (s.charAt(i) == '(') {
        stack.push(i); // Push index of '(' onto the stack
      } else if (s.charAt(i) == ')') {
        final int j = stack.pop(); // Pop the matching '(' index
        pair.put(i, j); // Map ')' to '('
        pair.put(j, i); // Map '(' to ')'
      }
    }

    // Step 2: Build the final string
    for (int i = 0, d = 1; i < s.length(); i += d) {
      if (s.charAt(i) == '(' || s.charAt(i) == ')') {
        i = pair.get(i); // Jump to the matching parenthesis
        d = -d; // Reverse the direction
      } else {
        sb.append(s.charAt(i)); // Append non-parenthesis characters
      }
    }

    return sb.toString(); // Return the result
  }
}
```

### Example
Let's dry run the example input `"a(bcdefghijkl(mno)p)q"`:

1. **Pairing parentheses**:
   - `(` at index 1 matches `)` at index 17.
   - `(` at index 12 matches `)` at index 16.

2. **Building the final string**:
   - Start with `i = 0`, `d = 1`: Append `a`.
   - `i = 1`, `d = 1`: Jump to `i = 17`, `d = -1`.
   - `i = 16`, `d = -1`: Append `p`.
   - `i = 15`, `d = -1`: Append `o`.
   - `i = 14`, `d = -1`: Append `n`.
   - `i = 13`, `d = -1`: Append `m`.
   - `i = 12`, `d = -1`: Jump to `i = 16`, `d = 1`.
   - Continue appending in reverse, then forward again until the final string is built.

The resulting string will be `"apmnolkjihgfedcbq"`.
