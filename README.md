# Pyre Wildfire Radio Alert System

**The Pyre Wildfire Radio Alert System** is a life-saving communication tool designed to protect vulnerable communities in wildfire-prone areas. This non-digital, satellite-enabled alert system ensures that high-risk populations—including those with limited access to internet or cellular networks—receive timely and critical evacuation notifications.

## Project Overview

In California and other regions prone to wildfires, residents in rural, low-income, and elderly communities are disproportionately affected due to barriers in accessing reliable digital communication channels. Pyre seeks to bridge this gap by using satellite-based alerts to deliver evacuation warnings directly to homes, leveraging existing fire alarms and a compact radio transmitter. When activated by local authorities, the system alerts residents using the fire alarm siren, making evacuation notices unmistakable.

## Features

- **Non-Digital Alert Transmission**: Utilizes satellite communication to reach residents without relying on internet or cellular networks.
- **Automatic Integration with Fire Alarms**: Connects directly to existing fire alarm systems to activate loud evacuation alerts.
- **Dashboard for Monitoring and Management**: A centralized dashboard for emergency services to trigger alerts, track wildfire status, and monitor responses.
- **Real-Time Wildfire Tracking**: Integrates data such as wildfire perimeters, active incidents, red flag warnings, and air quality forecasts through the [California Department of Forestry and Fire Protection API (CAL FIRE)](https://www.fire.ca.gov/).

## Key Data on the Dashboard

1. **Wildfire Perimeters**
2. **Active Air Assets**
3. **Fire Incidents by Acre Size**
4. **County Alerts**
5. **Red Flag Warnings**
6. **Smoke and Haze Forecast**
7. **Prescribed Fires**
8. **Currently Active Incidents**

## How It Works

1. **Evacuation Alert Trigger**: Local authorities monitor wildfire progression using the Pyre dashboard. When a wildfire reaches “danger status,” an evacuation alert is triggered.
2. **Notification Transmission**: A satellite signal activates the Pyre radio transmitters, prompting the connected fire alarms to sound a distinct evacuation warning.
3. **Response Monitoring**: Residents acknowledge receipt of the alert via a green ("safe") or red ("evacuate") button. If no response is received within an hour, authorities are notified to check on the household.

## Getting Started

To run the Pyre dashboard locally or deploy it for official use, follow the installation instructions in the [Installation Guide](./installation.md) and the API setup instructions in [CAL FIRE API Setup](./api_setup.md).

## Community Engagement and Support

Pyre emphasizes community safety and resilience, actively engaging with local residents to build trust, offer training, and gather feedback. Regular community sessions ensure the system meets user needs and supports inclusivity in emergency preparedness.

## License

Distributed under the MIT License. See [LICENSE](./LICENSE) for more information.

--- 

For questions or to contribute, please contact the Pyre team at [pyre.support@example.com](mailto:pyre.support@example.com).
