# Singleton

```python
class BankAccount:
    def __init__(self, balance=0):
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount

    def withdraw(self, amount):
        if amount > self.balance:
            raise ValueError("Insufficient balance")
        self.balance -= amount


class BankAccountFactory:
    _instance = None

    def __new__(cls, *args, **kwargs):
        if cls._instance is None:
            cls._instance = super().__new__(cls)
            cls._instance.accounts = {}
        return cls._instance

    def create_account(self, account_number, balance=0):
        if account_number not in self.accounts:
            account = BankAccount(balance)
            self.accounts[account_number] = account
        else:
            account = self.accounts[account_number]
        return account
```
