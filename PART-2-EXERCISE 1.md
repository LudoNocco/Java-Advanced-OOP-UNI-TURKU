# EXERCISE 1: analysis of array average function

## What is the result when “nums = [1, 2]”
Here, the avg function calculates the mean of the array elements. The sum calculation is equal to 1 + 2 = 3, but as the Array size is 2 and an integer division is occurring, such that 3 ÷ 2, the result ends up being 1 because the fractional part is discarded. Then, a type conversion takes place whereby 1 is cast to a float, giving a final result of 1.0.

## What is the result with “empty nums array”
When nums is empty, the avg function detects the empty array and so an exception is thrown. In main, the exception is caught but not handled, meaning that result remains uninitialized. If result is an object type, it will be null. If it is a primitive type, it will have a default value. An example for a float would be 0.0 as a default value.

## what is my proposed solution?
I would modify the avg function to handle non-negative numbers exclusively, that is to only accept  arrays with non-negative integers (x ≥ 0). For arrays that do not meet these requirements (so containing negative numbers), the function should identify all indices with negative values and report these indices to the caller.

## Potential implementation

```java
class NegativeElementException extends Exception {
    private Map<Integer, Integer> negativeElements;

    public NegativeElementException(Map<Integer, Integer> negativeElements) {
        this.negativeElements = negativeElements;
    }

    public Map<Integer, Integer> getNegativeElements() {
        return negativeElements;
    }
}

public class ArrayAverageCalculator {
    public static double calculateAverage(int[] sequence) throws IllegalArgumentException, NegativeElementException {
        if (sequence == null || sequence.length == 0) {
            throw new IllegalArgumentException("Input sequence is null or empty");
        }

        Map<Integer, Integer> negativeElements = new HashMap<>();
        long sum = 0;
        for (int i = 0; i < sequence.length; i++) {
            if (sequence[i] < 0) {
                negativeElements.put(i, sequence[i]);
            }
            sum += sequence[i];
        }

        if (!negativeElements.isEmpty()) {
            throw new NegativeElementException(negativeElements);
        }

        return (double) sum / sequence.length;
    }

    public static void main(String[] args) {
        int[] testSequence1 = {1, 2};
        int[] testSequence2 = {};
        int[] testSequence3 = {1, -2, -3, 4};

        try {
            double result = calculateAverage(testSequence1);
            System.out.printf("Average of testSequence1: %.2f%n", result);
        } catch (IllegalArgumentException | NegativeElementException e) {
            System.out.println("Error with testSequence1: " + e.getMessage());
        }

        try {
            double result = calculateAverage(testSequence2);
            System.out.printf("Average of testSequence2: %.2f%n", result);
        } catch (IllegalArgumentException | NegativeElementException e) {
            System.out.println("Error with testSequence2: " + e.getMessage());
        }

        try {
            double result = calculateAverage(testSequence3);
            System.out.printf("Average of testSequence3: %.2f%n", result);
        } catch (IllegalArgumentException e) {
            System.out.println("Error with testSequence3: " + e.getMessage());
        } catch (NegativeElementException e) {
            System.out.println("Negative elements found in testSequence3:");
            e.getNegativeElements().forEach((index, value) -> 
                System.out.printf("The %d-th number %d in your array is invalid%n", index + 1, value));
        }
    }
}
