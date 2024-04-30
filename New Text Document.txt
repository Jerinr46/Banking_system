import random

class BankAccount:
    def __init__(self, account_number, pin, balance=0, name="", address="", dob="", profession="", account_type="", aadhar_available=False, pan_available=False, username="", guardian_name="", guardian_address="", guardian_mobile=""):
        self.account_number = account_number
        self.pin = pin
        self.balance = balance
        self.name = name
        self.address = address
        self.dob = dob
        self.profession = profession
        self.account_type = account_type
        self.aadhar_available = aadhar_available
        self.pan_available = pan_available
        self.username = username
        self.guardian_name = guardian_name
        self.guardian_address = guardian_address
        self.guardian_mobile = guardian_mobile
        self.ifsc_code = f"ENTR{random.randint(1000, 9999)}"

    def login(self, username, pin):
        return self.username == username and self.pin == pin

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            print(f"Deposit of {amount} successful.")
        else:
            print("Invalid deposit amount.")

    def withdraw(self, amount):
        if 2000 < self.balance - amount:
            self.balance -= amount
            print(f"Withdrawal of {amount} successful.")
        else:
            print("Insufficient balance or invalid withdrawal amount.")

def main():
    print("WELCOME TO ENTRI BANK")

    # Check if the customer is an existing customer or new
    is_existing_customer = input("Are you an existing customer? (yes/no): ").lower() == "yes"

    if is_existing_customer:
        # Example accounts for testing
        accounts = [
            BankAccount("1234567890", "1234", name="John Doe", address="123 Main St", dob="1990-01-01", profession="Engineer", account_type="Savings", aadhar_available=True, pan_available=True, username="john_doe_1990"),
            BankAccount("0987654321", "4321", name="Jane Smith", address="456 Elm St", dob="1985-05-15", profession="Teacher", account_type="Current", aadhar_available=True, pan_available=True, username="jane_smith_1985")
        ]

        # Prompt for login details
        entered_username = input("Enter username to login: ")
        entered_pin = input("Enter PIN to login: ")

        # Find the account matching the entered details
        logged_in_account = None
        for account in accounts:
            if account.login(entered_username, entered_pin):
                logged_in_account = account
                break

        if logged_in_account:
            print("Login successful.")
            print(f"Welcome, {logged_in_account.name}!")
            # Deposit
            deposit_amount = float(input("Enter amount to deposit: "))
            logged_in_account.deposit(deposit_amount)

            # Withdrawal
            withdrawal_amount = float(input("Enter amount to withdraw: "))
            logged_in_account.withdraw(withdrawal_amount)

            # Displaying balance
            print(f"Current balance: {logged_in_account.balance}")
        else:
            print("Invalid username or PIN. Login failed.")
    else:
        # Creating a new account
        name = input("Enter your full name: ")
        address = input("Enter your address: ")
        dob = input("Enter your date of birth (YYYY-MM-DD): ")
        profession = input("Enter your profession: ")
        mobile_number = input("Enter your mobile number: ")
        print("Select account type:")
        print("1. Savings")
        print("2. Current")
        account_type_choice = input("Enter your choice: ")
        account_type = "Savings" if account_type_choice == "1" else "Current"
        aadhar_available = input("Do you have Aadhar card? (yes/no): ").lower() == "yes"
        pan_available = input("Do you have Pan card? (yes/no): ").lower() == "yes"

        # Generate a username and password based on name, age, and mobile number
        username = f"{name.lower().replace(' ', '_')}_{dob.split('-')[0]}_{mobile_number[-4:]}"
        password = f"{name.lower().replace(' ', '_')}_{dob.split('-')[0]}_{mobile_number[-4:]}".replace('-', '')

        # If age is less than 18, ask for guardian details
        if int(dob.split('-')[0]) >= 18:
            guardian_name = ""
            guardian_address = ""
            guardian_mobile = ""
        else:
            guardian_name = input("Enter guardian's full name: ")
            guardian_address = input("Enter guardian's address: ")
            guardian_mobile = input("Enter guardian's mobile number: ")

        # Creating a new account with default PIN '1234'
        new_account_number = str(random.randint(1000000000, 9999999999))
        new_account = BankAccount(new_account_number, "1234", name=name, address=address, dob=dob, profession=profession, account_type=account_type, aadhar_available=aadhar_available, pan_available=pan_available, username=username, guardian_name=guardian_name, guardian_address=guardian_address, guardian_mobile=guardian_mobile)

        print("Account created successfully.")
        print(f"Your account number is: {new_account_number}")
        print(f"Your IFSC code is: {new_account.ifsc_code}")
        print(f"Your username is: {username}")
        print(f"Your password is: {password}")

if __name__ == "__main__":
    main()
