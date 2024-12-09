import sqlite3

# Database Initialization
def initialize_database():
    connection = sqlite3.connect('ice_cream_parlor.db')
    cursor = connection.cursor()
    
    # Create tables
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS flavors (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            is_seasonal BOOLEAN NOT NULL
        )
    ''')
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS ingredients (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL UNIQUE
        )
    ''')
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS allergens (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL UNIQUE
        )
    ''')
    cursor.execute('''
        CREATE TABLE IF NOT EXISTS cart (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            flavor_id INTEGER NOT NULL,
            FOREIGN KEY(flavor_id) REFERENCES flavors(id)
        )
    ''')
    connection.commit()
    connection.close()

# Add a New Flavor
def add_flavor(name, is_seasonal):
    connection = sqlite3.connect('ice_cream_parlor.db')
    cursor = connection.cursor()
    cursor.execute("INSERT INTO flavors (name, is_seasonal) VALUES (?, ?)", (name, is_seasonal))
    connection.commit()
    connection.close()

# Add a New Allergen
def add_allergen(name):
    connection = sqlite3.connect('ice_cream_parlor.db')
    cursor = connection.cursor()
    try:
        cursor.execute("INSERT INTO allergens (name) VALUES (?)", (name,))
        connection.commit()
    except sqlite3.IntegrityError:
        print("Allergen already exists.")
    connection.close()

# Search and Filter Flavors
def search_flavors(keyword="", only_seasonal=False):
    connection = sqlite3.connect('ice_cream_parlor.db')
    cursor = connection.cursor()
    query = "SELECT id, name, is_seasonal FROM flavors WHERE name LIKE ?"
    params = [f"%{keyword}%"]
    if only_seasonal:
        query += " AND is_seasonal = 1"
    cursor.execute(query, params)
    results = cursor.fetchall()
    connection.close()
    return results

# Add Flavor to Cart
def add_to_cart(flavor_id):
    connection = sqlite3.connect('ice_cream_parlor.db')
    cursor = connection.cursor()
    cursor.execute("INSERT INTO cart (flavor_id) VALUES (?)", (flavor_id,))
    connection.commit()
    connection.close()

# View Cart Contents
def view_cart():
    connection = sqlite3.connect('ice_cream_parlor.db')
    cursor = connection.cursor()
    cursor.execute('''
        SELECT flavors.name 
        FROM cart 
        JOIN flavors ON cart.flavor_id = flavors.id
    ''')
    items = cursor.fetchall()
    connection.close()
    return items

# Main Functionality
def main():
    initialize_database()
    
    while True:
        print("\n--- Ice Cream Parlor Management ---")
        print("1. Add a New Flavor")
        print("2. Search & Filter Flavors")
        print("3. Add a New Allergen")
        print("4. Add Flavor to Cart")
        print("5. View Cart")
        print("6. Exit")
        
        choice = input("Select an option: ")
        
        if choice == "1":
            name = input("Enter flavor name: ")
            is_seasonal = input("Is it seasonal? (yes/no): ").strip().lower() == "yes"
            add_flavor(name, is_seasonal)
            print("Flavor added successfully!")
        elif choice == "2":
            keyword = input("Enter search term (leave blank for all): ")
            only_seasonal = input("Filter seasonal flavors only? (yes/no): ").strip().lower() == "yes"
            flavors = search_flavors(keyword, only_seasonal)
            print("Available Flavors:")
            for flavor in flavors:
                print(f"ID: {flavor[0]}, Name: {flavor[1]}, Seasonal: {'Yes' if flavor[2] else 'No'}")
        elif choice == "3":
            allergen = input("Enter allergen name: ")
            add_allergen(allergen)
            print("Allergen added successfully!")
        elif choice == "4":
            flavor_id = input("Enter the flavor ID to add to cart: ")
            add_to_cart(flavor_id)
            print("Flavor added to cart!")
        elif choice == "5":
            cart_items = view_cart()
            print("Your Cart:")
            for item in cart_items:
                print(f"- {item[0]}")
        elif choice == "6":
            print("Goodbye!")
            break
        else:
            print("Invalid option. Please try again.")

if __name__ == "__main__":
    main()
