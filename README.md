import csv
from pathlib import Path

def analyze_weather(filename):
    path = Path(filename)

    if not path.exists():
        print("File not found!")
        return

    temperatures = []

    try:
        with path.open(encoding="utf-8") as file:
            reader = csv.DictReader(file)

            if not reader.fieldnames or "temperature" not in reader.fieldnames:
                print("CSV must contain a temperature column.")
                return

            for row in reader:
                try:
                    temperature = float(row["temperature"])
                    day = row.get("date", "Unknown")
                    temperatures.append((day, temperature))
                except (ValueError, TypeError):
                    continue

        if not temperatures:
            print("No valid temperature data found.")
            return

        values = [item[1] for item in temperatures]

        hottest = max(temperatures, key=lambda x: x[1])
        coldest = min(temperatures, key=lambda x: x[1])
        average = sum(values) / len(values)

        print("\n===== WEATHER REPORT =====")
        print(f"Recorded days: {len(values)}")
        print(f"Average temperature: {average:.2f}°C")
        print(f"Hottest day: {hottest[0]} ({hottest[1]}°C)")
        print(f"Coldest day: {coldest[0]} ({coldest[1]}°C)")

    except OSError as error:
        print("Error:", error)

def main():
    filename = input("Enter CSV file path: ")
    analyze_weather(filename)

if __name__ == "__main__":
    main()
