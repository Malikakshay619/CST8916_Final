# CST8916_Final_Project
Real-time Monitoring System for Rideau Canal Skateway
The Rideau Canal Skateway, located in Ottawa, Canada, is one of the world’s largest outdoor skating rinks. Given its historical and iconic status, ensuring the safety of skaters is of utmost importance. The ice conditions and weather factors such as temperature and snow accumulation must be monitored regularly to ensure that the skateway remains safe for users.

To address this, I have designed a real-time monitoring system that simulates IoT sensors deployed at key locations along the Rideau Canal (Dow’s Lake, Fifth Avenue, NAC). These sensors collect data about the following every 10 seconds:

Ice Thickness (in cm): Critical for determining whether the ice is thick enough to support skaters.
Surface Temperature (in °C): Helps identify the potential for ice melting, which could be dangerous for skaters.
Snow Accumulation (in cm): Impacts the strength and usability of the ice surface.
External Temperature (in °C): Provides weather context and indicates whether external weather conditions might be a risk.
This system continuously monitors the data from these sensors and processes it in real-time to detect unsafe conditions. The data is then stored in Azure Blob Storage for further analysis.

The key challenge that this system addresses is the real-time monitoring and detection of unsafe ice conditions, which helps to take timely actions to ensure safety along the Rideau Canal Skateway.
The architecture of the Real-time Monitoring System for Rideau Canal Skateway consists of the following components:

Simulated IoT Sensors: Deployed at Dow’s Lake, Fifth Avenue, and NAC. They collect real-time data (ice thickness, surface temperature, snow accumulation, and external temperature) every 10 seconds.

Azure IoT Hub: Acts as a central hub, receiving data from the sensors and streaming it in real-time to Azure Stream Analytics.

Azure Stream Analytics: Processes data in 5-minute tumbling windows to calculate:

Average ice thickness
Maximum snow accumulation Aggregates are grouped by location.
Azure Blob Storage: Stores the processed output, organized by location and timestamp for easy access and analysis.

Data Flow
Sensors → Azure IoT Hub
IoT Hub → Azure Stream Analytics
Stream Analytics → Azure Blob Storage
