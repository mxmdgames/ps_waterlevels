# San Juan River Safety Indicators

This project fetches and displays river safety information for tubing and kayaking/rafting on the San Juan River near Pagosa Springs, CO. It uses the USGS (United States Geological Survey) water services API to obtain real-time flow rate data and displays safety indicators based on predefined thresholds. Additionally, it includes a local weather report widget.

## How It Works

### Data Source

The code fetches river flow data from the USGS water services API. The specific site being monitored is identified by the `SITE_CODE` '09342500', which corresponds to the San Juan River near Pagosa Springs, CO.

### Fetching Data

The `fetchRiverData()` function makes an asynchronous HTTP request to the USGS API using the `fetch()` method. The URL includes the `SITE_CODE` and requests the current stream flow data (parameter code `00060`).

### Parsing Data

Once the data is received, it's parsed from JSON format. The code extracts the current flow rate value from the response and converts it to a floating-point number.

### Determining Safety

There are two functions to determine safety levels:

- **Tubing Safety (`determineTubingSafety`)**:
    - Below 200 cfs: "Too Low"
    - 200-399 cfs: "Ideal"
    - 400-799 cfs: "Good"
    - 800-1199 cfs: "Caution"
    - 1200 cfs and above: "Too High"

- **Kayaking/Rafting Safety (`determineRaftingSafety`)**:
    - Below 400 cfs: "Too low to raft"
    - 400-799 cfs: "Low"
    - 800-1499 cfs: "Medium"
    - 1500-2399 cfs: "Medium/High"
    - 2400-3599 cfs: "High"
    - 3600 cfs and above: "Really High"

Each safety level is associated with a color and a message.

### Updating Indicators

The `updateSafetyIndicators()` function calls `fetchRiverData()`, then uses `determineTubingSafety()` and `determineRaftingSafety()` to get the safety levels for the current flow rate. It then updates the visual indicators for both tubing and kayaking/rafting.

### Display

The safety level is displayed in circular indicators on the webpage. The color and message in these indicators change based on the current river conditions.

### Flow Rate Display

The current flow rate in cubic feet per second (cfs) is displayed below each indicator.

### Periodic Updates

The code sets up an interval to call `updateSafetyIndicators()` every 15 minutes (900,000 milliseconds), ensuring the displayed information stays current.

### Weather Widget

The page also includes a weather widget for Pagosa Springs, which is loaded from a third-party service (weatherwidget.io). This provides additional context about local weather conditions.
