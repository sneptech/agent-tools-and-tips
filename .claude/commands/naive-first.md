# Naive First

For algorithmic or complex logic: implement the obviously-correct naive version first, verify correctness, THEN optimize. Never skip the naive step.

## Task

$ARGUMENTS

## Process

### Step 1: Naive Implementation
Write the simplest, most obviously-correct implementation. Prioritize clarity over performance. This version should be:
- Easy to read and verify by inspection
- Correct for all inputs including edge cases
- Potentially slow, and that's fine

### Step 2: Verify Correctness
- Write tests against the naive implementation
- Cover edge cases, boundary conditions, empty inputs, large inputs
- Confirm everything passes
- This naive version is now your correctness oracle

### Step 3: Optimize (only if needed)
- Identify the actual performance bottleneck (don't guess — measure if possible)
- Implement the optimized version
- Run the SAME tests against the optimized version
- Verify identical behavior to the naive version

### Step 4: Present Both

```
NAIVE VERSION: [code]
- Complexity: O(?) time, O(?) space
- Status: all tests passing

OPTIMIZED VERSION: [code]
- Complexity: O(?) time, O(?) space
- Status: all tests passing
- Improvement: [quantify the gain]

CORRECTNESS CHECK:
- Both versions produce identical output for all test cases: [yes/no]
```

Correctness first. Performance second. A fast wrong answer is worse than a slow right one.
