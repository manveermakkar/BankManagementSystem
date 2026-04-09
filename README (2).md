# 🏦 Bank Management System

A console-based **Bank Management System** built in C++ that simulates core banking operations. It supports account creation, deposits, withdrawals, balance enquiry, and account closure, with persistent data storage using file I/O.

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Project Structure](#project-structure)
- [Classes & Functions](#classes--functions)
- [How It Works](#how-it-works)
- [Getting Started](#getting-started)
- [Usage](#usage)
- [Data Persistence](#data-persistence)
- [Limitations & Known Issues](#limitations--known-issues)
- [Future Improvements](#future-improvements)

---

## Overview

This project demonstrates object-oriented programming in C++ by simulating a simple banking system. It uses two primary classes — `Account` and `Bank` — to manage customer accounts. All account data is saved to and loaded from a local binary text file (`Bank.data`), ensuring that records persist between program sessions.

---

## Features

- ✅ Open a new bank account
- ✅ Check account balance (Balance Enquiry)
- ✅ Deposit money into an account
- ✅ Withdraw money with minimum balance enforcement
- ✅ Close an existing account
- ✅ View all accounts
- ✅ Persistent data storage via file I/O (`Bank.data`)
- ✅ Auto-incrementing unique account numbers
- ✅ Exception handling for insufficient funds

---

## Project Structure

```
bank-management-system/
│
├── bankmanagementsystem.cpp   # Main source file (all classes and logic)
└── Bank.data                  # Auto-generated data file for account persistence
```

---

## Classes & Functions

### `class Account`

Represents an individual bank account.

| Member | Type | Description |
|---|---|---|
| `accountNumber` | `long` (private) | Unique auto-incremented account ID |
| `firstName` | `string` (private) | Account holder's first name |
| `lastName` | `string` (private) | Account holder's last name |
| `balance` | `float` (private) | Current account balance |
| `NextAccountNumber` | `static long` | Tracks the last assigned account number |

#### Methods

| Method | Description |
|---|---|
| `Account(fname, lname, balance)` | Constructor — creates a new account with auto-incremented number |
| `getAccNo()` | Returns the account number |
| `getFirstName()` | Returns the first name |
| `getLastName()` | Returns the last name |
| `getBalance()` | Returns the current balance |
| `Deposit(amount)` | Adds the given amount to the balance |
| `Withdraw(amount)` | Deducts the amount; throws `InsufficientFunds` if balance falls below minimum |
| `setLastAccountNumber(n)` | Static — sets the account number counter (used during file load) |
| `getLastAccountNumber()` | Static — retrieves the current counter value |

#### Operator Overloads

| Operator | Description |
|---|---|
| `operator<<` (ofstream) | Serializes account data to a file |
| `operator>>` (ifstream) | Deserializes account data from a file |
| `operator<<` (ostream) | Prints account details to the console |

---

### `class Bank`

Manages all accounts using an in-memory `std::map` keyed by account number.

#### Methods

| Method | Description |
|---|---|
| `Bank()` | Constructor — loads all accounts from `Bank.data` on startup |
| `OpenAccount(fname, lname, balance)` | Creates a new account, saves to file, returns the new `Account` |
| `BalanceEnquiry(accountNumber)` | Returns the `Account` object for the given account number |
| `Deposit(accountNumber, amount)` | Deposits amount into the specified account, returns updated `Account` |
| `Withdraw(accountNumber, amount)` | Withdraws amount from the specified account, returns updated `Account` |
| `CloseAccount(accountNumber)` | Removes the account from the map |
| `ShowAllAccounts()` | Prints details of all accounts to the console |
| `~Bank()` | Destructor — saves all accounts back to `Bank.data` on exit |

---

### `class InsufficientFunds`

A lightweight exception class thrown when a withdrawal would cause the balance to drop below `MIN_BALANCE` (500).

---

## How It Works

1. **Startup:** The `Bank` constructor reads `Bank.data` (if it exists) and populates the in-memory `map<long, Account>` with stored accounts. The account number counter is restored from the last saved account.

2. **Operations:** The user selects from a menu. Each operation interacts with the `Bank` object which modifies the in-memory map.

3. **Persistence:** 
   - When an account is **opened**, the full map is written to `Bank.data` immediately.
   - On **program exit**, the `Bank` destructor flushes all accounts back to `Bank.data`.

4. **Minimum Balance:** Withdrawals are blocked if the resulting balance would go below `500` (defined by `MIN_BALANCE`). An `InsufficientFunds` exception is thrown in this case.

---

## Getting Started

### Prerequisites

- A C++ compiler (g++, clang++, MSVC, etc.)
- C++11 or later

### Compilation

```bash
g++ bankmanagementsystem.cpp -o bank
```

### Run

```bash
./bank
```

On Windows:
```bash
bank.exe
```

---

## Usage

Once running, you'll see the following menu:

```
***Banking System***

    Select one option below
    1 Open an Account
    2 Balance Enquiry
    3 Deposit
    4 Withdrawal
    5 Close an Account
    6 Show All Accounts
    7 Quit
Enter your choice:
```

### Example Walkthrough

**Opening an account:**
```
Enter your choice: 1
Enter First Name: John
Enter Last Name: Doe
Enter initial Balance: 1000

Congratulation Account is Created
First Name: John
Last Name: Doe
Account Number: 1
Balance: 1000
```

**Depositing money:**
```
Enter your choice: 3
Enter Account Number: 1
Enter Balance: 500

Amount is Deposited
First Name: John
Last Name: Doe
Account Number: 1
Balance: 1500
```

**Withdrawing money:**
```
Enter your choice: 4
Enter Account Number: 1
Enter Balance: 800

Amount Withdrawn
First Name: John
Last Name: Doe
Account Number: 1
Balance: 700
```

---

## Data Persistence

Account data is stored in a plain-text file called `Bank.data` in the same directory as the executable. The file is automatically created on first use and updated on every account opening and on program exit.

**File format (one account per 4 lines):**
```
1
John
Doe
1000
```

---

## Limitations & Known Issues

- **No error handling for invalid account numbers** — looking up a non-existent account with `map::find` and dereferencing the end iterator causes undefined behavior.
- **Typo in output** — "Congradulation" should be "Congratulation"; "initil" should be "initial".
- **Missing `break` in `case 5`** — after closing an account, execution falls through to `case 6` and shows all accounts unintentionally.
- **`float` used for balance** — floating-point precision can cause rounding errors in financial calculations; `double` or integer-based cents would be more appropriate.
- **No authentication** — any user can access any account by knowing the account number.
- **No transaction history** — individual deposits/withdrawals are not logged.

---

## Future Improvements

- [ ] Add input validation and error handling for invalid account numbers
- [ ] Replace `float` with `double` or integer (paise/cents) for accurate balance storage
- [ ] Add PIN/password-based authentication per account
- [ ] Log transaction history per account
- [ ] Add account types (savings, current)
- [ ] Implement interest calculation
- [ ] Build a GUI or web front-end
- [ ] Use a database (SQLite) instead of flat file storage

---

## License

This project is open source and available for educational purposes.
