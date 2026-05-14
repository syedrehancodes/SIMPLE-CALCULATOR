import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.println("=== Simple Calculator ===");
        System.out.println("Type 'exit' at any prompt to quit.");

        while (true) {
            System.out.print("Enter first number: ");
            String s1 = sc.next();
            if (s1.equalsIgnoreCase("exit")) break;

            double num1;
            try {
                num1 = Double.parseDouble(s1);
            } catch (NumberFormatException e) {
                System.out.println("Invalid number. Try again.");
                continue;
            }

            System.out.print("Enter operator (+, -, *, /): ");
            String opStr = sc.next();
            if (opStr.equalsIgnoreCase("exit")) break;
            if (opStr.length() != 1 || "+-*/".indexOf(opStr.charAt(0)) == -1) {
                System.out.println("Invalid operator. Use + - * or /.");
                continue;
            }
            char op = opStr.charAt(0);

            System.out.print("Enter second number: ");
            String s2 = sc.next();
            if (s2.equalsIgnoreCase("exit")) break;

            double num2;
            try {
                num2 = Double.parseDouble(s2);
            } catch (NumberFormatException e) {
                System.out.println("Invalid number. Try again.");
                continue;
            }

            double result;
            switch (op) {
                case '+': result = num1 + num2; break;
                case '-': result = num1 - num2; break;
                case '*': result = num1 * num2; break;
                case '/':
                    if (num2 == 0) {
                        System.out.println("Error: Division by zero!");
                        continue;
                    }
                    result = num1 / num2;
                    break;
                default:
                    System.out.println("Unexpected operator.");
                    continue;
            }

            System.out.println("Result: " + result);
        }

        System.out.println("Calculator closed.");
        sc.close();
    }
}
