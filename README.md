import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.println("=== Simple Calculator ===");
        while (true) {
            System.out.print("Enter first number (or type 0 to quit): ");
            double num1 = sc.nextDouble();

            if (num1 == 0) {
                System.out.println("Calculator closed.");
                break;
            }

            System.out.print("Enter operator (+, -, *, /): ");
            char op = sc.next().charAt(0);

            System.out.print("Enter second number: ");
            double num2 = sc.nextDouble();

            double result = 0;

            switch (op) {
                case '+':
                    result = num1 + num2;
                    break;
                case '-':
                    result = num1 - num2;
                    break;
                case '*':
                    result = num1 * num2;
                    break;
                case '/':
                    if (num2 != 0) {
                        result = num1 / num2;
                    } else {
                        System.out.println("Error: Division by zero!");
                        continue;
                    }
                    break;
                default:
                    System.out.println("Invalid operator!");
                    continue;
            }

            System.out.println("Result: " + result);
        }
    }
}
