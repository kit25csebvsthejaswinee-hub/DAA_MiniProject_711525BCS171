import java.util.Scanner;
public class Main {
static long[][] dp;
static int[][] split;
 static int[] dimensions;
 * Recursively prints the optimal parenthesization.
    static void printOptimalOrder(int i, int j) {
      if (i == j) {
      System.out.print("A" + i);
    return;
        }
        System.out.print("(");
        printOptimalOrder(i, split[i][j]);
        printOptimalOrder(split[i][j] + 1, j);
        System.out.print(")");
    }
    static long matrixChainOrder(int n) {
        dp = new long[n + 1][n + 1];
        split = new int[n + 1][n + 1];
        for (int length = 2; length <= n; length++) {
            for (int i = 1; i <= n - length + 1; i++) {
           int j = i + length - 1;
           dp[i][j] = Long.MAX_VALUE;
            for (int k = i; k < j; k++) {
              long cost = dp[i][k]
              + dp[k + 1][j]
              + (long) dimensions[i - 1]
            * dimensions[k]
            * dimensions[j]
             if (cost < dp[i][j]) {
                 dp[i][j] = cost;
                 split[i][j] = k;
                    }
                }
            }
        }
        return dp[1][n];
    }
    static void printCostTable(int n) {
        System.out.println("\nDP Cost Table:");
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if (i > j) {
                    System.out.print("0\t");
                } else {
                    System.out.print(dp[i][j] + "\t");
                }
            }
            System.out.println();
        }
    }
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("======================================");
        System.out.println("       MATRIX CHAIN OPTIMIZER");
        System.out.println("======================================");
        System.out.print("Enter number of matrices: ");
        int n = scanner.nextInt();
        if (n <= 0) {
            System.out.println("Error: Number of matrices must be positive.");
            scanner.close();
            return;
        }
        dimensions = new int[n + 1];
        System.out.println("\nEnter " + (n + 1)
                + " dimensions:");
        for (int i = 0; i <= n; i++) {
            dimensions[i] = scanner.nextInt();
            if (dimensions[i] <= 0) {
                System.out.println(
                        "Error: Matrix dimensions must be positive."
                );
                scanner.close();
                return;
            }
        }
        long minimumCost = matrixChainOrder(n);
        System.out.println("\n======================================");
        System.out.println("RESULT");
        System.out.println("======================================");
        System.out.println(
                "Minimum scalar multiplications: " + minimumCost
        );
        System.out.print("Optimal multiplication order: ");
        printOptimalOrder(1, n);
        System.out.println();
        printCostTable(n);
        System.out.println("======================================");
        scanner.close();
    }
}
