# GNR Visualizer (Telecommunications Traffic Analyzer)

A Python application designed to visualize telecommunications traffic load and calculate the **GNR** (*Godzina Największego Ruchu* - Busy Hour).

The application uses **FreeSimpleGUI** for the interface and **Matplotlib** for plotting, allowing users to analyze system load based on call turnaround times and call intensity data using standard industry algorithms (TCBH and FDMH).

## 🚀 Features

* **Interactive Visualization:** Plots system load over time (minutes) and visually highlights the calculated Busy Hour.
* **Dual Algorithms:** Switch instantly between **TCBH** (Time Consistent Busy Hour) and **FDMH** (Fixed Daily Measurement Hour) calculations.
* **Automatic Data Simulation:** If no external data is provided, the system automatically generates a 7-day dataset based on a default seed file (`int.txt`) with random daily shifts.
* **Custom Data Import:** Load your own datasets via the GUI to analyze specific traffic patterns.
* **Metric Calculation:** automatically calculates Traffic Load (Erlangs) based on intensity and average holding times.

## 📋 Requirements

* **Python 3.11+** (Required due to the usage of `StrEnum`).
* **OS:** Windows, macOS, or Linux.

### Python Dependencies

Install the required packages via pip:

```bash
pip install pandas numpy FreeSimpleGUI matplotlib
```
## 🛠️ Usage

### **Preparation**

Ensure the default data files are present in the script directory if you want to use the simulation mode:

* czas.txt (Turnaround times)  
* int.txt (Base intensity pattern)

### **Running the App**

Execute the main script:

```bash
python main.py
```
### **Using the Interface**

* **Tabs (TCBH / FDMH):** Click the tabs at the top to switch between calculation algorithms. The graph updates automatically.  
* **Menu \> Plik \> Otwórz (File \> Open):** Load custom data. You will be asked to select a file for turnaround times and a folder for intensity logs.  
* **Menu \> Plik \> Zamknij (File \> Close):** Closes the custom data view and reverts to the default simulated data.  
* **Menu \> Pomoc (Help):** View descriptions of the algorithms and file formats.

## **📂 Data File Specifications**

To analyze your own data, you must provide files adhering to the following formats:

### **1\. Turnaround Time File (.txt)**

Used to calculate the Average Holding Time (AHT).

* **Format:** A single column of numbers representing call duration in seconds.  
* **Example:**  
  120  
  45  
  300  
  15

### **2\. Intensity Files (Directory)**

You must select a folder containing .txt files. Each file represents one day of traffic.

* **Filename:** Any .txt file (e.g., day1.txt, monday.txt).  
* **Format:** Two columns separated by whitespace.  
  * Col 1: Minute of the day (integer).  
  * Col 2: Intensity value (float).  
* **Important:** The decimal separator must be a comma (,).  
* **Example:**  
  Plaintext  
  1       0,002  
  2       0,005  
  3       0,012  
  ...

## **🧮 Algorithms Explained**

The application calculates Load for every minute using the formula:

Load \= Intensity × Avg Turnaround Time

### **TCBH (Time Consistent Busy Hour)**

* **Goal:** Find the sliding 60-minute window that has the highest average load across all measured days.  
* **Methodology:**  
  1. Calculates the mean load for every specific minute (e.g., mean of 10:01 AM across all days).  
  2. Applies a rolling sum with a window size of 60 minutes.  
  3. Selects the window with the maximum sum.  
* **Use Case:** Network capacity planning.

### **FDMH (Fixed Daily Measurement Hour)**

* **Goal:** Find the specific clock hour (e.g., 10:00–11:00) with the highest total load.  
* **Methodology:**  
  1. Aggregates data into fixed hourly buckets (Hour 0, Hour 1... Hour 23).  
  2. Sums the load for each hour across all days.  
  3. Identifies the hour with the peak volume.  
* **Use Case:** Reporting and long-term trend analysis.

## **📝 Localization & Terminology**

The User Interface is localized in Polish. Here is a glossary for non-Polish speakers:

| Polish Term | English Translation |
| :---- | :---- |
| **GNR** | Busy Hour (Godzina Największego Ruchu) |
| **Obciążenie** | Load |
| **Czas obsługi** | Turnaround Time / Service Time |
| **Intensywność** | Intensity |
| **Plik** | File |
| **Otwórz / Zamknij** | Open / Close |
| **Zatwierdź** | Submit / Confirm |

## **⚙️ Technical Notes**

* **Temporary Files:** Upon launch, the script creates a folder named int\_files to store simulated daily data. This folder is automatically deleted (shutil.rmtree) when the application closes.  
* **Error Handling:** The application includes pop-ups for malformed data (e.g., wrong decimal separators or missing columns).

## **⚖️ License**

This project is open-source. Feel free to modify, distribute, or use it for educational purposes outlined under MIT License
