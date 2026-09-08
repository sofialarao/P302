# My Dashboard - Project Brief

## What is this?
This is an interactive dashboard for a caregiver user. Rosa is a curse that’s taking care of Elena, an eldery with inicial Alzheimer diagnosis. Through a tablet Application, Rosa is able to fill in daily Information about Elena, and the Family and Dr is able to check in the progress of the patient. Rosa logs in daily to check ai smart suggestions and Analysis from everyday data she entres. Think of a healthcare Application 101, to easily understand progress and behaviors.

## Data
Generate a fake dataset as a JSON file (src/data/metrics.json).
Daily, Monthly, 6 and 12 months of data (Jan-Dec 2025).
Metrics showing:
- daily events/actions of the elderly
-list of behavior patterns to compare across 12 weeks
- Time Analysis of duration of daily tasks with observations from caregiver
- Line chart for 12 months to analyze the elderly independency across time

## Layout (Vuetify)
- v-app-bar at the top with the dashboard title, patient name, caregiver name, and date.
- A CTA button in the nav bar to see patient history
- An avatar to see patient information and family contact info
- Below the app bar:
	- Top section: KPIs with patient date of diagnosis and diagnosis name; observations logged for the last 12 weeks and a CTA if some of them need action, patient independency status)
	- 1st section: Latest most repetitive patterns and observations. When user click show an ai smart analysis to guide the caregiver for next actions regarding that pattern.
	- 2nd section:  data visualization for Weekly analysis. Full width for the user to see data Evolution as they interact with it. This highlights core behaviors for elderlys with this diagnosis. To analyze disease progress.
	- 3rd section: Progress bar time analysis of everyday actions the caregiver fills out daily, this gives a progress time analysis of the elderly capacity and Independecy foresight across alzheimer levels.
- Bottom section: a space to showcase the ups and lows of the patient across months, highlighting the good patterns and suggestions of how to take this information to the Family, pros and con.

## Interactions
- The data visualizations should be interactive for the user to swipe and top across data to discover more information.


## Style
- dark theme by default (vuetify darkk theme)
- Clean, minimal, lots of whitespace
- charts should use a cohesive color BLUE - GRAY palette 
- mobile responsive - cards stack on small screens and charts resize or adapt to smaller versions with CTA for user to open a full screen visual.
## Tech
- Vue 3 + TypeScript + Vuetify 3
- Chart.js via vue-chartsjs for all charts
- fake data from a local JSON file (no API calls)
- Single page - no routing for this app