# Simulating Custom RRAM Devices with CrossSim

This guide explains how to simulate your **own fabricated RRAM device** using [CrossSim](https://github.com/sandialabs/cross-sim), Sandia Labs' analog crossbar simulator. It walks through generating LTP/LTD lookup tables from real device data and integrating them into CrossSim.

---

## 🔧 Step 1: Setup CrossSim

1. Clone the CrossSim repository:

   ```bash
   git clone https://github.com/sandialabs/cross-sim.git
   cd cross-sim
   ```

2. Install the required Python packages:

   ```bash
   pip install -r requirements.txt
   ```

> Recommended: Use Python 3.8 or higher.

---

## 📊 Step 2: Prepare Measurement Data

You should have `.xls` files for your RRAM measurements.

**Required columns:**

* `VMeasCh1` (Measured voltage)
* `IMeasCh1` (Measured current)

---

## 👁️ Step 3: Generate LTP/LTD Data CSVs

Use the [Colab preprocessing script](LINK_TO_YOUR_COLAB_SCRIPT) to process your `.xls` files into two CSVs: one for LTP (SET) and one for LTD (RESET).

### What It Does:

* Filters LTP data: `0.5V ≤ VMeasCh1 ≤ 1.0V`
* Filters LTD data: `-1.0V ≤ VMeasCh1 ≤ -0.5V`
* Each `.xls` file contributes a column named `rmp1`, `rmp2`, ... to the output CSVs.
* Outputs:

  * `set.csv`  ➔ Contains LTP current data
  * `reset.csv` ➔ Contains LTD current data

### How to Use:

1. Open the Colab notebook
2. Upload your `.xls` files
3. It will process and download `set.csv` and `reset.csv`

---

## 📂 Step 4: Move CSVs to CrossSim Folder

Copy both `set.csv` and `reset.csv` into:

```
cross-sim/examples/lookup_table_generation/lookup_table_example/
```

---

## ⚙️ Step 5: Generate the Lookup Table

Navigate to the `lookup_table_example` folder and run:

```bash
python create_lookup_table.py
```

The script uses:

```python
folder = 'lookup_table_example'
set_file = 'set.csv'
reset_file = 'reset.csv'
```

It will produce:

* `lut_set.npy`
* `lut_reset.npy`

These are the LUT files used by CrossSim in neural network simulations.

---

## 🫠 Why This Matters

This process enables:

* Modeling real, fabricated RRAM devices
* Evaluating neuromorphic behavior
* Integrating hardware-accurate LUTs into ML pipelines

---

## ✨ Bonus: Custom Tools

You can customize the preprocessing script or CrossSim LUT scripts to fit your device's voltage windows or behavior.

Feel free to fork and modify as needed!

---

Made with ❤️ by \[Your Name]
