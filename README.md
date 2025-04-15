# CST8916_Final_Project
Rideau Canal Skateway in Ottawa, Canada is one of the largest outdoor skating rinks globally. With its historic and iconic nature, ensuring skaters' safety is most important. Ice conditions and weather parameters like temperature and snow accumulation need to be checked periodically so that the skateway remains safe for usage.

To address this, I have designed a real-time monitoring system that simulates IoT sensors deployed at key locations along the Rideau Canal (Dow's Lake, Fifth Avenue, NAC). The sensors record data about the following every 10 seconds:

Ice Thickness (in cm): Critical in determining whether the ice is thick enough to support skaters.

Surface Temperature (in °C): Helps determine the likelihood of ice melting, which can be dangerous for skaters.
Snow cover (in cm): Impacts the usability and strength of the ice cover.
External temperature (in °C): Gives environmental context and indicates whether exterior weather conditions can be dangerous. This solution constantly monitors the input from these sensors and processes this in real-time to see whether it has moved into an unsafe state. That information is written out to Azure Blob Storage where it can be further processed.
The significant challenge faced by this system is the detection and monitoring in real-time of unsafe ice conditions, enabling swift action to allow for safety on the Rideau Canal Skateway. The components of the Real-time Monitoring System for Rideau Canal Skateway architecture are:

Simulated IoT Sensors: Deployed at Dow’s Lake, Fifth Avenue, and NAC. They collect real-time data (ice thickness, surface temperature, snow accumulation, and external temperature) every 10 seconds.

Azure IoT Hub: Acts as a central hub, receiving data from the sensors and streaming it in real-time to Azure Stream Analytics.

Azure Stream Analytics: Processes data in 5-minute tumbling windows to calculate:

Average ice thickness
Maximum snow accumulation Aggregates are grouped by location.
Azure Blob Storage: Stores the processed output, organized by location and timestamp for easy access and analysis.
![image](https://github.com/user-attachments/assets/470969f0-4422-45a2-9b2b-07e45a8d6636)

Data Flow
Sensors → Azure IoT Hub
IoT Hub → Azure Stream Analytics
Stream Analytics → Azure Blob Storage

# Rideau Canal IoT Simulation – Implementation Details

## 1. IoT Sensor Simulation

Simulated IoT sensors generate data for three key locations along the Rideau Canal Skateway:
- **Dow's Lake**
- **Fifth Avenue**
- **National Arts Centre (NAC)**

### Data Collected:
- Ice Thickness (in cm)
- Surface Temperature (in °C)
- Snow Accumulation (in cm)
- External Temperature (in °C)

**Frequency:** Every 10 seconds  
**Format:** JSON  
**Python Scripts:**  
- `DowsLake_Simulator.py`  
- `FifthAvenue_Simulator.py`  
- `NAC_Simulator.py`

### Sample JSON Payload:
```json
{
  "location": "Dow's Lake",
  "iceThickness": 33,
  "surfaceTemperature": 3,
  "snowAccumulation": 8,
  "externalTemperature": 0,
  "timestamp": "2025-04-04T16:51:00.745122Z"
}

SELECT
  location,
  AVG(iceThickness) AS avgIceThickness,
  MAX(snowAccumulation) AS maxSnowAccumulation,
  System.Timestamp AS timestamp
INTO
  ridueoutput
FROM
  ridueinput
GROUP BY
  location,
  TumblingWindow(minute, 5)
