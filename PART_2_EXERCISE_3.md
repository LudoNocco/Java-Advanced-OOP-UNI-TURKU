# What is my proposed solution?

To achieve the requirements of this exercise, I believe that we need to separate the mortgage data and calculations from the user interaction logic. 

We can use encapsulation principles to ensure that implementation details are not visible to the client. We can achieve this by having a “LoanAccount Class” to manages loan details and perform calculations and a separate “LoanSimulator Class” to handle user input and the overall flow. 

The advantages of this approach are that the LoanAccount class encapsulates and protects loan data, and modularity is achieved by hiding the business logic from user interface. 

However, as for disadvantages, my solution relies on command-line input in the main method, and as we know, users can make mistakes, especially when, due to encapsulation, the details of how the implementation works are hidden from them. Also, the facts that objects are created in the program may require additional memory, so may be more resource intensive compared to other approaches.

# Potential implementation

## LoanAccount Class

```java
public class LoanAccount {
    private final double borrowedAmount;
    private final int repaymentPeriod;

    public LoanAccount(double borrowedAmount, int repaymentPeriod) {
        validateLoanParameters(borrowedAmount, repaymentPeriod);
        this.borrowedAmount = borrowedAmount;
        this.repaymentPeriod = repaymentPeriod;
    }

    private void validateLoanParameters(double amount, int period) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Loan amount must be positive.");
        }
        if (period <= 0 || period > 300) {
            throw new IllegalArgumentException("Loan period must be between 1 and 300 months.");
        }
    }

    public double computeMonthlyPayment() {
        return borrowedAmount / repaymentPeriod + borrowedAmount / 240;
    }

    public double getBorrowedAmount() { return borrowedAmount; }
    public int getRepaymentPeriod() { return repaymentPeriod; }
}
```

## LoanSimulator Class

```java
import java.util.Scanner;
public class LoanSimulator {
    public static void main(String[] args) {
        double loanAmount = promptForInput("Enter loan amount: ");
        int loanTerm = (int) promptForInput("Enter loan term (months, 1-300): ");

        try {
            LoanAccount loan = new LoanAccount(loanAmount, loanTerm);
            double monthlyPayment = loan.computeMonthlyPayment();
            System.out.printf("Monthly payment: $%.2f%n", monthlyPayment);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }

    private static double promptForInput(String prompt) {
        System.out.print(prompt);
        return new Scanner(System.in).nextDouble();
    }
}
