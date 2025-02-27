# energy-tracker

Developing an energy-tracking Python program involves obtaining and processing energy consumption data, analyzing it, and providing insights or recommendations for optimization. Below is a simple outline of what such a program might look like. This would include reading energy data from a CSV file, analyzing it, and providing basic insights.

Please note, to run this program you need to have access to household energy consumption data in a suitable format, such as a CSV file.

```python
import csv
from datetime import datetime
import matplotlib.pyplot as plt

class EnergyTracker:
    def __init__(self, filename):
        self.filename = filename
        self.data = []

    def read_data(self):
        """Reads energy consumption data from a CSV file"""
        try:
            with open(self.filename, newline='') as csvfile:
                reader = csv.DictReader(csvfile)
                for row in reader:
                    date = datetime.strptime(row['date'], '%Y-%m-%d')
                    consumption = float(row['consumption'])
                    self.data.append({'date': date, 'consumption': consumption})
            print("Data reading complete.")
        except FileNotFoundError:
            print(f"Error: The file {self.filename} was not found.")
        except KeyError:
            print("Error: Missing necessary column in the data file.")
        except ValueError:
            print("Error: Data format is incorrect.")
        except Exception as e:
            print(f"Unexpected error: {e}")

    def analyze_data(self):
        """Analyzes energy consumption data"""
        try:
            if not self.data:
                raise ValueError("No data to analyze.")

            total_consumption = sum(item['consumption'] for item in self.data)
            daily_consumption = total_consumption / len(self.data)

            max_consumption = max(self.data, key=lambda x: x['consumption'])
            min_consumption = min(self.data, key=lambda x: x['consumption'])

            print(f"Total consumption: {total_consumption:.2f} kWh")
            print(f"Average daily consumption: {daily_consumption:.2f} kWh")
            print(f"Highest daily consumption: {max_consumption['consumption']:.2f} kWh on {max_consumption['date']}")
            print(f"Lowest daily consumption: {min_consumption['consumption']:.2f} kWh on {min_consumption['date']}")

            # Create a plot to visualize energy consumption over time
            self.create_plot()

        except ValueError as e:
            print(e)
        except Exception as e:
            print(f"Unexpected error during analysis: {e}")

    def create_plot(self):
        """Create a plot to visualize energy consumption over time"""
        try:
            dates = [item['date'] for item in self.data]
            consumptions = [item['consumption'] for item in self.data]

            plt.figure(figsize=(10, 5))
            plt.plot(dates, consumptions, marker='o')
            plt.title('Daily Energy Consumption')
            plt.xlabel('Date')
            plt.ylabel('Energy Consumption (kWh)')
            plt.grid(True)
            plt.xticks(rotation=45)
            plt.tight_layout()
            plt.show()
        except Exception as e:
            print(f"Error creating plot: {e}")

def main():
    filename = 'energy_data.csv'  # Change to your file path
    tracker = EnergyTracker(filename)
    tracker.read_data()
    tracker.analyze_data()

if __name__ == "__main__":
    main()
```

### Features of the Code:

1. **CSV Reading**: The code reads a CSV file containing dates and energy consumption data.

2. **Data Analysis**:
   - Calculates total energy consumption.
   - Computes average daily consumption.
   - Finds the highest and lowest daily energy usages.

3. **Visual Representation**:
   - It uses matplotlib to plot the consumption data over time.

4. **Error Handling**:
   - Handles file not found errors, data format issues, and unpredicted exceptions.

5. **Scalability and Customization**:
   - Easily scalable by expanding the data analysis with more sophisticated analytics or machine learning models.
   - Supports potential customization by defining reduction strategies based on the results.

For a more comprehensive tool, additional data sources, such as real-time meters or integration with any APIs providing energy usage, might be considered, especially for dynamic tracking and recommendations.