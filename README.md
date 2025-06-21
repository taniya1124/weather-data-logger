import pandas as pd
from datetime import datetime

# Data storage
weather_data = []
date_set = set()  # ✅ Store unique dates to avoid duplicates

# ✅ Function to validate date format
def is_valid_date(date_str):
    try:
        datetime.strptime(date_str, "%Y-%m-%d")
        return True
    except ValueError:
        return False

# ✅ Function to add new weather entry
def add_entry():
    while True:
        date = input("Enter date (YYYY-MM-DD): ")
        if not is_valid_date(date):
            print("❌ Invalid date format. Please use YYYY-MM-DD.")
            continue
        if date in date_set:
            print("⚠️ Entry for this date already exists. Duplicate not allowed.")
            return
        break

    while True:
        try:
            temperature = float(input("Enter temperature (in °C): "))
            break
        except ValueError:
            print("❌ Please enter a valid number.")

    condition = input("Enter weather condition (e.g., Sunny, Rainy): ")

    entry = {
        "Date": date,
        "Temperature": temperature,
        "Condition": condition
    }

    weather_data.append(entry)
    date_set.add(date)
    print("✅ Entry added successfully!")

# ✅ Function to view all entries
def view_entries():
    if not weather_data:
        print("📭 No weather data available.")
        return
    df = pd.DataFrame(weather_data)
    print("\n📄 Weather Data:")
    print(df.to_string(index=False))

# ✅ Function to show summary
def summarize_data():
    if not weather_data:
        print("📭 No data to summarize.")
        return
    df = pd.DataFrame(weather_data)
    print(f"\n📊 Average Temperature: {df['Temperature'].mean():.2f} °C")
    print("\n☁️ Condition Counts:")
    print(df['Condition'].value_counts())

# ✅ Function to export to CSV
def export_to_csv():
    if not weather_data:
        print("📭 No data to export.")
        return
    df = pd.DataFrame(weather_data)
    df.to_csv("weather_data.csv", index=False)
    print("✅ Data exported to weather_data.csv")

# ✅ Menu display
def display_menu():
    print("\n📋 Weather Data Menu")
    print("1. Add Entry")
    print("2. View Entries")
    print("3. Show Summary")
    print("4. Export to CSV")
    print("5. Exit")

# ✅ Main loop
while True:
    display_menu()
    choice = input("Enter choice (1–5): ")

    if choice == '1':
        add_entry()
    elif choice == '2':
        view_entries()
    elif choice == '3':
        summarize_data()
    elif choice == '4':
        export_to_csv()
    elif choice == '5':
        print("👋 Exiting. Goodbye!")
        break
    else:
        print("❌ Invalid choice. Please enter 1 to 5.")
