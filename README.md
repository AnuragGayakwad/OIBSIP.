# OIBSIP.
Create an ATM Interface using Java Programming. ("user id= Oasis Infobyte    user pin=1234")

Here created an ATM Interface using Java Programming
#Oasis Infobyte

import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        User user = new User("oasis Infobyte", "1234");
        ATMOperations atm = new ATMOperations(user);

        System.out.println("====== Welcome to ATM Interface ======");
        System.out.print("Enter User ID: ");
        String enteredId = scanner.nextLine();
        System.out.print("Enter PIN: ");
        String enteredPin = scanner.nextLine();

        if (user.authenticate(enteredId, enteredPin)) {
            System.out.println("\nLogin successful!\n");
            atm.start();
        } else {
            System.out.println("Invalid credentials. Exiting...");
        }

        scanner.close();
    }
}


class User {
    private String userId;
    private String pin;

    public User(String userId, String pin) {
        this.userId = userId;
        this.pin = pin;
    }

    public boolean authenticate(String userId, String pin) {
        return this.userId.equals(userId) && this.pin.equals(pin);
    }
}


class BankAccount {
    private double balance;
    private List<Transaction> history;

    public BankAccount() {
        this.balance = 0.0;
        this.history = new ArrayList<>();
    }

    public void deposit(double amount) {
        balance += amount;
        history.add(new Transaction("Deposit", amount));
        System.out.println("Amount deposited: $" + amount);
    }

    public void withdraw(double amount) {
        if (amount > balance) {
            System.out.println("Insufficient balance.");
        } else {
            balance -= amount;
            history.add(new Transaction("Withdraw", amount));
            System.out.println("Amount withdrawn: $" + amount);
        }
    }

    public void transfer(double amount, String recipientId) {
        if (amount > balance) {
            System.out.println("Insufficient balance.");
        } else {
            balance -= amount;
            history.add(new Transaction("Transfer to " + recipientId, amount));
            System.out.println("Amount $" + amount + " transferred to " + recipientId);
        }
    }

    public void printTransactionHistory() {
        if (history.isEmpty()) {
            System.out.println("No transactions found.");
        } else {
            System.out.println("Transaction History:");
            for (Transaction t : history) {
                System.out.println(t);
            }
        }
    }

    public double getBalance() {
        return balance;
    }
}


class Transaction {
    private String type;
    private double amount;
    private String timestamp;

    public Transaction(String type, double amount) {
        this.type = type;
        this.amount = amount;
        this.timestamp = java.time.LocalDateTime.now()
                .format(java.time.format.DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
    }

    @Override
    public String toString() {
        return "[" + timestamp + "] " + type + ": $" + amount;
    }
}


class ATMOperations {
    private BankAccount account;
    private User user;
    private Scanner scanner;

    public ATMOperations(User user) {
        this.user = user;
        this.account = new BankAccount();
        this.scanner = new Scanner(System.in);
    }

    public void start() {
        int choice;
        do {
            System.out.println("\n====== ATM Menu ======");
            System.out.println("1. Transaction History");
            System.out.println("2. Withdraw");
            System.out.println("3. Deposit");
            System.out.println("4. Transfer");
            System.out.println("5. Quit");
            System.out.print("Choose an option: ");
            choice = getIntInput();

            switch (choice) {
                case 1:
                    account.printTransactionHistory();
                    break;
                case 2:
                    System.out.print("Enter amount to withdraw: $");
                    account.withdraw(getDoubleInput());
                    break;
                case 3:
                    System.out.print("Enter amount to deposit: $");
                    account.deposit(getDoubleInput());
                    break;
                case 4:
                    System.out.print("Enter recipient ID: ");
                    String recipient = scanner.nextLine();
                    System.out.print("Enter amount to transfer: $");
                    account.transfer(getDoubleInput(), recipient);
                    break;
                case 5:
                    System.out.println("Thank you for using the ATM. Goodbye!");
                    break;
                default:
                    System.out.println("Invalid option. Try again.");
            }

        } while (choice != 5);
    }

    private int getIntInput() {
        while (true) {
            try {
                return Integer.parseInt(scanner.nextLine());
            } catch (NumberFormatException e) {
                System.out.print("Enter a valid number: ");
            }
        }
    }

    private double getDoubleInput() {
        while (true) {
            try {
                return Double.parseDouble(scanner.nextLine());
            } catch (NumberFormatException e) {
                System.out.print("Enter a valid amount: ");
            }
        }
    }
}
