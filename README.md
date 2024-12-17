# NiftyFuturesOptionsMaster

## **Overview**

**NiftyFuturesOptionsMaster** is a comprehensive data processing pipeline designed to consolidate NIFTY Futures and Put Options data, calculate essential financial Greeks (Delta, Gamma, Vega, Theta), and integrate Open Interest metrics. This tool ensures accurate and continuous data analysis across different trading days, even when Futures prices experience significant fluctuations.

## **Features**

- **Data Consolidation:** Combines minute-by-minute Futures and Put Options data into a unified master file.
- **Greeks Calculation:** Computes Delta, Gamma, Vega, and Theta for each Put Option using the Black-Scholes model.
- **Open Interest Integration:** Incorporates Open Interest data to assess market liquidity and sentiment.
- **Dynamic Categorization:** Automatically classifies Put Options as In-The-Money (ITM) or Out-of-The-Money (OTM) based on the current Futures price.
- **Data Continuity:** Ensures accurate categorization and referencing of options even when Futures prices change significantly across trading days.
- **Comprehensive Logging:** Tracks processing steps, warnings, and errors for easy monitoring and debugging.

## **Data Flow**

1. **Data Sources:**
   - **Futures Data:** `mock_futures.csv`
   - **Options Data:** `mock_options.csv`

2. **Processing Steps:**
   - **Data Cleaning:** Removes unnecessary columns and ensures data integrity.
   - **Timestamp Alignment:** Combines `Date` and `Time` columns into a unified `Timestamp` for accurate matching.
   - **Categorization:** Classifies Put Options as ITM or OTM based on the current Futures price at each timestamp.
   - **Greeks Calculation:** Computes financial Greeks for each categorized Put Option.
   - **Open Interest Integration:** Adds Open Interest metrics to the master file.
   - **Master File Generation:** Compiles all data into `master_file_enhanced.csv` ready for analysis.

## **Handling Dynamic Futures Price Changes**

One of the core challenges addressed by this project is maintaining data continuity and accurate categorization of Put Options when the Futures price changes significantly across trading days. Here's how the script handles this:

1. **Unified Timestamp Creation:**
   - Both Futures and Options data are combined into a single `Timestamp` column, representing each trading minute.
   - This ensures that each minute's Futures price is accurately paired with its corresponding Put Options data.

2. **Per-Timestamp Processing:**
   - For each Futures timestamp, the script:
     - Retrieves the current Futures price.
     - Categorizes Put Options as ITM if their strike price is **above** the current Futures price.
     - Categorizes Put Options as OTM if their strike price is **below** the current Futures price.
   - **Dynamic Adjustment:** Since this categorization happens **within the loop for each timestamp**, any change in the Futures price (e.g., from 18,000 to 22,000) automatically triggers a reclassification of options based on the new price.

3. **New Trading Day Detection:**
   - The script detects when a new trading day begins by comparing the current timestamp's date with the previously processed date.
   - Upon detecting a new day, it updates the reference Futures price, ensuring that option categorizations are based on the latest price.

4. **Consistent Option Referencing:**
   - By processing each timestamp independently and basing categorizations on the current Futures price, the script avoids issues related to multi-indexing.
   - This approach ensures that ITM and OTM Put Options are always correctly referenced relative to the current Futures price, regardless of price changes across days.

## **Installation**

1. **Clone the Repository:**

   ```bash
   git clone https://github.com/your-username/NiftyFuturesOptionsMaster.git
   cd NiftyFuturesOptionsMaster

