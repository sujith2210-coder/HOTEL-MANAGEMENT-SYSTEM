# HOTEL-MANAGEMENT-SYSTEM
A Python-based Hotel Management System that streamlines hotel operations such as room booking, customer management, check-in/check-out, room availability tracking, and bill generation. The project provides an easy-to-use interface for managing hotel records efficiently and reducing manual work.
import sqlite3

# Create database connection
conn = sqlite3.connect("hotel.db")
cursor = conn.cursor()

# Create customer table
cursor.execute("""
CREATE TABLE IF NOT EXISTS customers (
    customer_id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT,
    phone TEXT,
    room_no INTEGER,
    days INTEGER
)
""")

conn.commit()

# Add Customer
def add_customer(name, phone, room_no, days):
    cursor.execute(
        "INSERT INTO customers (name, phone, room_no, days) VALUES (?, ?, ?, ?)",
        (name, phone, room_no, days)
    )
    conn.commit()
    print("Customer Added Successfully")

# View Customers
def view_customers():
    cursor.execute("SELECT * FROM customers")
    records = cursor.fetchall()

    for record in records:
        print(record)

# Generate Bill
def generate_bill(customer_id):
    cursor.execute(
        "SELECT name, room_no, days FROM customers WHERE customer_id=?",
        (customer_id,)
    )

    customer = cursor.fetchone()

    if customer:
        room_charge = 1000
        total = customer[2] * room_charge

        print("\n----- BILL -----")
        print("Customer:", customer[0])
        print("Room No:", customer[1])
        print("Days Stayed:", customer[2])
        print("Total Amount:", total)
    else:
        print("Customer Not Found")

# Delete Customer (Check-Out)
def checkout(customer_id):
    cursor.execute(
        "DELETE FROM customers WHERE customer_id=?",
        (customer_id,)
    )
    conn.commit()
    print("Check-Out Successful")

# Main Menu
while True:
    print("\nHOTEL MANAGEMENT SYSTEM")
    print("1. Add Customer")
    print("2. View Customers")
    print("3. Generate Bill")
    print("4. Check-Out")
    print("5. Exit")

    choice = input("Enter Choice: ")

    if choice == "1":
        name = input("Enter Name: ")
        phone = input("Enter Phone: ")
        room = int(input("Enter Room Number: "))
        days = int(input("Enter Number of Days: "))
        add_customer(name, phone, room, days)

    elif choice == "2":
        view_customers()

    elif choice == "3":
        cid = int(input("Enter Customer ID: "))
        generate_bill(cid)

    elif choice == "4":
        cid = int(input("Enter Customer ID: "))
        checkout(cid)

    elif choice == "5":
        break

    else:
        print("Invalid choice")

conn.close()
        print("Invalid Choice")

conn.close()
