# GPS-based-toll-systems-Project
This project aims to simulate a toll-based system using GPS coordinates. Vehicles are tracked as they move through a road network, and tolls are deducted from their balances when they enter predefined toll zones. The simulation uses Python libraries for geospatial data handling and visualization.
**GPS Toll-Based System Simulation**
Table of Contents
1.	Introduction
2.	Project Overview
3.	Libraries and Dependencies
4.	Data Preparation
5.	Simulation Design
6.	Implementation
7.	Results
8.	Visualization
9.	Conclusion
10.	References
________________________________________
**1. Introduction**
This project aims to simulate a toll-based system using GPS coordinates. Vehicles are tracked as they move through a road network, and tolls are deducted from their balances when they enter predefined toll zones. The simulation uses Python libraries for geospatial data handling and visualization.
________________________________________
**2. Project Overview**
Objectives
•	Simulate vehicle movements in a road network.
•	Deduct toll fees when vehicles enter toll zones.
•	Track and update vehicle balances.
Scope
•	Create a simple geospatial model with vehicles, toll zones, and road networks.
•	Visualize the toll zones and vehicle routes.
________________________________________
**3. Libraries and Dependencies**
The project utilizes several Python libraries:
•	SimPy: For simulation.
•	GeoPandas: For geospatial data manipulation.
•	Shapely: For geometric operations.
•	GeoPy: For geospatial calculations.
•	Pandas: For data handling.
•	Matplotlib: For plotting.
•	Folium: For interactive maps.
Installation
Install the required libraries using the following command:
python
Copy code
!pip install simpy geopandas shapely geopy pandas matplotlib folium
________________________________________
**4. Data Preparation**
Toll Zones and Road Network
Define toll zones and the road network using GeoPandas. Here’s an example with two toll zones and a single road:
python
Copy code
import geopandas as gpd
from shapely.geometry import Point, LineString

toll_zones = gpd.GeoDataFrame({
    'geometry': [Point(12.9716, 77.5946), Point(13.0827, 80.2707)]
}, crs="EPSG:4326")

road_network = gpd.GeoDataFrame({
    'geometry': [LineString([(12.9716, 77.5946), (13.0827, 80.2707)])]
}, crs="EPSG:4326")
Vehicle Data
Initialize vehicle data including IDs, start and end coordinates, and account balances:
python
Copy code
import pandas as pd

vehicles = pd.DataFrame({
    'id': [1, 2],
    'start': [(12.9716, 77.5946), (12.2958, 76.6394)],
    'end': [(13.0827, 80.2707), (12.9716, 77.5946)],
    'balance': [1000, 800]
})
________________________________________
**5. Simulation Design**
Vehicle Movement
Vehicles move from their start to end coordinates. They enter toll zones if their current position falls within the zone’s geometry.
Toll Calculation
A toll fee is deducted from the vehicle's balance each time it enters a toll zone.
________________________________________
**6. Implementation**
Define Functions
Implement the functions to simulate vehicle movement and toll calculations:
python
Copy code
import simpy
from shapely.geometry import Point

def vehicle_movement(env, vehicle_id, start, end, toll_zones, road_network):
    current_position = start
    while current_position != end:
        current_position = end
        for _, zone in toll_zones.iterrows():
            if Point(current_position).within(zone['geometry']):
                print(f"Vehicle {vehicle_id} entered toll zone at {env.now}")
                env.process(calculate_toll(env, vehicle_id))
        yield env.timeout(1)

def calculate_toll(env, vehicle_id):
    toll_rate = 5
    vehicles.loc[vehicles['id'] == vehicle_id, 'balance'] -= toll_rate
    print(f"Toll deducted for vehicle {vehicle_id} at {env.now}")
    yield env.timeout(0)
Run Simulation
Setup the simulation environment and run it:
python
Copy code
env = simpy.Environment()
for _, vehicle in vehicles.iterrows():
    env.process(vehicle_movement(env, vehicle['id'], vehicle['start'], vehicle['end'], toll_zones, road_network))
env.run(until=10)
________________________________________
**7. Results**
Updated Vehicle Balances
After running the simulation, the updated vehicle balances are:
python
Copy code
print(vehicles)
Output:
scss
Copy code
   id              start                end  balance
0   1  (12.9716, 77.5946)  (13.0827, 80.2707)      995
1   2  (12.2958, 76.6394)  (12.9716, 77.5946)      795
Simulation Logs
The simulation logs show toll deductions and vehicle entries into toll zones.
________________________________________
**8. Visualization**
Matplotlib Plot
Visualize toll zones and the road network:
python
Copy code
import matplotlib.pyplot as plt

base = toll_zones.plot(color='red', markersize=50)
road_network.plot(ax=base, color='blue')
plt.show()
Folium Map
Create an interactive map using Folium:
python
Copy code
import folium

m = folium.Map(location=[12.9716, 77.5946], zoom_start=10)
for _, row in toll_zones.iterrows():
    folium.Marker([row['geometry'].y, row['geometry'].x], popup='Toll Zone').add_to(m)
m.save('toll_system_map.html')
m
________________________________________
**9. Conclusion**
The GPS Toll-Based System Simulation effectively models vehicle movement through a road network with toll zones. It demonstrates the integration of geospatial data and simulation techniques to manage toll collection dynamically. This approach can be extended to more complex scenarios with additional vehicle dynamics and varied toll structures.
________________________________________
**10. References**
•	SimPy: SimPy Documentation
•	GeoPandas: GeoPandas Documentation
•	Shapely: Shapely Documentation
•	GeoPy: GeoPy Documentation
•	Folium: Folium Documentation

Vidhi
Rollno.2204995
Khalsa college of Engineering and Technology
Year:3rd(Semster:5th)

:During my 45-day training program with the Intel Unnati Program, I developed a project titled "GPS Toll-based System Simulation." This project focuses on simulating a toll collection system using Python, leveraging GPS data to calculate toll fees based on the distance traveled by vehicles. The system comprises several core components, including a GPS module that utilizes the Haversine formula to compute the distance between two geographical coordinates, a TollBooth class that represents toll booths with specific locations and toll rates, and a Vehicle class that manages vehicle information, balance, and trip details. As vehicles pass through toll booths, the system dynamically calculates toll fees based on the distance traveled. Additionally, the project includes a GPS Toll System class that oversees the overall management of vehicles and toll booths, enabling the simulation of trips and the recording of toll fees and trip history. This project provided me with practical experience in Python programming and system design, aligning with the practical learning objectives of the Intel Unnati Program. The hands-on development process helped me gain a deeper understanding of how GPS-based toll systems operate and how to simulate such systems effectively.