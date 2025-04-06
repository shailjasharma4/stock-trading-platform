import java.util.*;

class Stock {
    String symbol;
    double price;

    Stock(String symbol, double price) {
        this.symbol = symbol;
        this.price = price;
    }
}

class Portfolio {
    Map<String, Integer> holdings = new HashMap<>();
    double cashBalance;

    Portfolio(double initialCash) {
        this.cashBalance = initialCash;
    }

    void buyStock(String symbol, int quantity, double price) {
        double totalCost = quantity * price;
        if (cashBalance >= totalCost) {
            holdings.put(symbol, holdings.getOrDefault(symbol, 0) + quantity);
            cashBalance -= totalCost;
            System.out.println("Bought " + quantity + " shares of " + symbol);
        } else {
            System.out.println("Not enough balance to complete purchase.");
        }
    }

    void sellStock(String symbol, int quantity, double price) {
        if (holdings.getOrDefault(symbol, 0) >= quantity) {
            holdings.put(symbol, holdings.get(symbol) - quantity);
            cashBalance += quantity * price;
            System.out.println("Sold " + quantity + " shares of " + symbol);
        } else {
            System.out.println("Not enough shares to sell.");
        }
    }

    void showPortfolio(Map<String, Stock> marketData) {
        System.out.println("\n--- Portfolio ---");
        System.out.println("Cash Balance: $" + String.format("%.2f", cashBalance));
        double totalValue = cashBalance;
        for (String symbol : holdings.keySet()) {
            int qty = holdings.get(symbol);
            double stockValue = qty * marketData.get(symbol).price;
            totalValue += stockValue;
            System.out.println(symbol + ": " + qty + " shares @ $" + marketData.get(symbol).price + " = $" + String.format("%.2f", stockValue));
        }
        System.out.println("Total Portfolio Value: $" + String.format("%.2f", totalValue));
    }
}

public class StockTradingPlatform {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Map<String, Stock> marketData = new HashMap<>();
        marketData.put("AAPL", new Stock("AAPL", 180.25));
        marketData.put("GOOGL", new Stock("GOOGL", 2750.75));
        marketData.put("AMZN", new Stock("AMZN", 3300.55));
        marketData.put("TSLA", new Stock("TSLA", 700.10));

        Portfolio portfolio = new Portfolio(10000); // Start with $10,000

        int choice;
        do {
            System.out.println("\n--- Stock Trading Platform ---");
            System.out.println("1. View Market Data");
            System.out.println("2. Buy Stock");
            System.out.println("3. Sell Stock");
            System.out.println("4. View Portfolio");
            System.out.println("5. Exit");
            System.out.print("Enter choice: ");
            choice = scanner.nextInt();
            scanner.nextLine(); // clear buffer

            switch (choice) {
                case 1:
                    System.out.println("\n--- Market Data ---");
                    for (Stock stock : marketData.values()) {
                        System.out.println(stock.symbol + ": $" + stock.price);
                    }
                    break;
                case 2:
                    System.out.print("Enter stock symbol to buy: ");
                    String buySymbol = scanner.nextLine().toUpperCase();
                    if (marketData.containsKey(buySymbol)) {
                        System.out.print("Enter quantity: ");
                        int qty = scanner.nextInt();
                        scanner.nextLine();
                        portfolio.buyStock(buySymbol, qty, marketData.get(buySymbol).price);
                    } else {
                        System.out.println("Stock not found.");
                    }
                    break;
                case 3:
                    System.out.print("Enter stock symbol to sell: ");
                    String sellSymbol = scanner.nextLine().toUpperCase();
                    if (marketData.containsKey(sellSymbol)) {
                        System.out.print("Enter quantity: ");
                        int qty = scanner.nextInt();
                        scanner.nextLine();
                        portfolio.sellStock(sellSymbol, qty, marketData.get(sellSymbol).price);
                    } else {
                        System.out.println("Stock not found.");
                    }
                    break;
                case 4:
                    portfolio.showPortfolio(marketData);
                    break;
                case 5:
                    System.out.println("Exiting platform. Goodbye!");
                    break;
                default:
                    System.out.println("Invalid choice. Try again.");
            }

        } while (choice != 5);

        scanner.close();
    }
}
