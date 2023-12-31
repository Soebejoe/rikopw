# rikopw
// program yang mengimplementasikan - Inheritence - Polimorfism - Asosiasi, Agregasi, Komposisi - class diagram - harus ada minimal class 10 class - Program console
  
  
  import java.util.ArrayList;
  import java.util.HashMap;
  import java.util.Scanner;

  // Abstract class for Account
  abstract class Account {
      protected String accountNumber;
      protected String accountHolder;
      protected double balance;
      protected ArrayList<String> transactions;

      public Account(String accountNumber, String accountHolder) {
          this.accountNumber = accountNumber;
          this.accountHolder = accountHolder;
          this.balance = 0;
          this.transactions = new ArrayList<>();
      }

      public String getAccountHolder() {
          return accountHolder;
      }

      public abstract void displayInfo();

      public void deposit(double amount) {
          this.balance += amount;
          this.transactions.add("Deposit: +" + amount);
      }

      public void withdraw(double amount) {
          if (amount <= this.balance) {
              this.balance -= amount;
              this.transactions.add("Withdrawal: -" + amount);
          } else {
              System.out.println("Insufficient funds!");
          }
      }

      public void transfer(Account targetAccount, double amount) {
          if (amount <= this.balance) {
              this.balance -= amount;
              targetAccount.deposit(amount);
              this.transactions.add("Transfer to " + targetAccount.accountHolder + ": -" + amount);
          } else {
              System.out.println("Insufficient funds!");
          }
      }
  }

  // Class for User
  class User extends Account {
      public User(String accountNumber, String accountHolder) {
          super(accountNumber, accountHolder);
      }

      @Override
      public void displayInfo() {
          System.out.println("Account Holder: " + this.accountHolder);
          System.out.println("Account Number: " + this.accountNumber);
          System.out.println("Balance: " + this.balance);
          System.out.println("Transaction History:");
          for (String transaction : this.transactions) {
              System.out.println(transaction);
          }
      }
  }

  // Class for Admin
  class Admin {
      private Bank bank;

      public Admin(Bank bank) {
          this.bank = bank;
      }

      public void viewAllAccounts() {
          for (Account account : this.bank.getAccounts()) {
              account.displayInfo();
          }
      }
  }

  // Class for Bank
  class Bank {
      private ArrayList<Account> accounts;

      public Bank() {
          this.accounts = new ArrayList<>();
      }

      public void addAccount(Account account) {
          this.accounts.add(account);
      }

      public ArrayList<Account> getAccounts() {
          return this.accounts;
      }
  }

  // Class for UserManager
  class UserManager {
      private HashMap<String, String> userCredentials;

      public UserManager() {
          this.userCredentials = new HashMap<>();
          this.userCredentials.put("admin", "admin");
          this.userCredentials.put("user1", "11111");
          this.userCredentials.put("user2", "22222");
      }

      public String authenticate() {
          Scanner scanner = new Scanner(System.in);

          System.out.println("Enter username:");
          String username = scanner.next();

          System.out.println("Enter password:");
          String password = scanner.next();

          if (userCredentials.containsKey(username) && userCredentials.get(username).equals(password)) {
              return username;
          } else {
              System.out.println("Login failed. Exiting program.");
              return null;
          }
      }
  }


  // Main class
  public class Main {
      public static void main(String[] args) {
          UserManager userManager = new UserManager();
          String loggedInUser = userManager.authenticate();

          Bank bank = new Bank();
          Admin admin = new Admin(bank);

          User user1 = new User("123", "Handoko");
          User user2 = new User("456", "Meli");

          bank.addAccount(user1);
          bank.addAccount(user2);

          // Sample transactions
          user1.deposit(1250);
          user2.deposit(750);
          user1.transfer(user2, 300);

          if ("admin".equals(loggedInUser)) {
              // Admin view
              admin.viewAllAccounts();
          } else {
              // User view
              for (Account account : bank.getAccounts()) {
                  if (account.getAccountHolder().equals(loggedInUser)) {
                      account.displayInfo();
                      break;
                  }
              }
          }
      }
  }



//ANALISA:

// Abstract class Account: Menyediakan dasar untuk akun dengan properti dasar seperti nomor akun, pemilik akun, saldo, dan riwayat transaksi.
Memiliki metode deposit, withdraw, transfer, dan abstract method displayInfo.

//Class User extends Account:Merupakan turunan dari kelas Account yang mengimplementasikan metode displayInfo sesuai dengan kebutuhan akun pengguna.
Digunakan untuk membuat objek akun pengguna dengan nomor akun, pemilik akun, saldo awal, dan riwayat transaksi.

//Class Admin: Memiliki referensi ke Bank dan dapat melihat informasi semua akun di bank.
Digunakan untuk membuat objek admin dengan referensi ke bank.

//Class Bank: Menyimpan daftar akun dan menyediakan metode untuk menambah akun.
Memiliki metode untuk mendapatkan daftar semua akun.

//Class UserManager:Mengelola otentikasi pengguna dengan menggunakan HashMap untuk menyimpan kredensial pengguna (username dan password).
Memiliki metode authenticate yang meminta pengguna untuk memasukkan username dan password.

//Main class:Menjalankan aplikasi utama dengan membuat objek UserManager untuk otentikasi.
Membuat objek Bank dan Admin.
Membuat beberapa objek User, menambahkannya ke Bank, dan melakukan beberapa transaksi (deposit dan transfer).
Jika pengguna yang masuk adalah admin, tampilkan informasi semua akun. Jika pengguna biasa, tampilkan informasi akun pengguna tersebut.

//Poin Penting:Program menggunakan konsep OOP (Object-Oriented Programming) dengan pewarisan (inheritance) untuk memodelkan hierarki kelas.
Bank menyimpan daftar akun, dan Admin dapat melihat informasi semua akun.
UserManager mengelola otentikasi pengguna dengan menggunakan username dan password.
Setiap transaksi (deposit, withdraw, transfer) dicatat dalam riwayat transaksi setiap akun.
Pengguna dapat melakukan deposit, withdraw, dan transfer melalui objek User.
Program memberikan tampilan informasi akun sesuai peran pengguna (admin atau pengguna biasa).
