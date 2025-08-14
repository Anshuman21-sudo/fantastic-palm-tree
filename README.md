import json
from datetime import datetime

DATA_FILE = "expenses.json"

# Load existing data
def load_data():
    try:
        with open(DATA_FILE, "r") as file:
            return json.load(file)
    except FileNotFoundError:
        return {"expenses": [], "budget": 0}

# Save data to file
def save_data(data):
    with open(DATA_FILE, "w") as file:
        json.dump(data, file, indent=4)

# Add an expense
def add_expense(data):
    amount = float(input("Enter expense amount: "))
    category = input("Enter category (Food, Transport, Bills, etc.): ")
    description = input("Enter short description: ")
    date = datetime.now().strftime("%Y-%m-%d")
    
    expense = {
        "date": date,
        "amount": amount,
        "category": category,
        "description": description
    }
    data["expenses"].append(expense)
    save_data(data)
    print("✅ Expense added successfully!")

# View all expenses
def view_expenses(data):
    if not data["expenses"]:
        print("No expenses recorded yet.")
        return
    print("\n--- All Expenses ---")
    for i, exp in enumerate(data["expenses"], start=1):
        print(f"{i}. {exp['date']} | ₹{exp['amount']} | {exp['category']} | {exp['description']}")

# Set monthly budget
def set_budget(data):
    budget = float(input("Enter your monthly budget: "))
    data["budget"] = budget
    save_data(data)
    print(f"✅ Monthly budget set to ₹{budget}")

# View budget status
def view_budget_status(data):
    total_spent = sum(exp["amount"] for exp in data["expenses"])
    budget = data["budget"]
    print(f"Monthly Budget: ₹{budget}")
    print(f"Total Spent: ₹{total_spent}")
    if budget > 0:
        remaining = budget - total_spent
        if remaining >= 0:
            print(f"✅ You have ₹{remaining} left this month.")
        else:
            print(f"⚠ You have exceeded your budget by ₹{abs(remaining)}!")

# Menu
def menu():
    data = load_data()
    while True:
        print("\n--- Personal Expense Tracker ---")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. Set Monthly Budget")
        print("4. View Budget Status")
        print("5. Exit")
        choice = input("Enter your choice: ")

        if choice == "1":
            add_expense(data)
        elif choice == "2":
            view_expenses(data)
        elif choice == "3":
            set_budget(data)
        elif choice == "4":
            view_budget_status(data)
        elif choice == "5":
            print("💾 Saving and exiting...")
            save_data(data)
            break
        else:
            print("Invalid choice. Try again!")

# Run the program
if __name__ == "__main__":
    menu()
