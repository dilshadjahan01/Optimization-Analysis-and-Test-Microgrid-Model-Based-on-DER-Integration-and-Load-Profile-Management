import numpy as np
import matplotlib.pyplot as plt

# Time in hours (24 hours for a full day simulation)
time = np.arange(0, 24, 1)

# Simulated solar power generation (kW)
# Assuming a peak during midday (12:00)
solar_generation = np.maximum(0, 10 * np.sin(np.pi * (time - 6) / 12))

# Industrial load demand (kW)
# Assume peak demand occurs between 9:00 and 17:00
load_demand = 5 + 3 * np.where((time >= 9) & (time <= 17), 1, 0)

# Initialize storage (battery) parameters
battery_capacity = 50  # in kWh
battery_charge = 20  # initial charge in kWh
battery_charge_rate = 5  # kW (max charge/discharge rate per hour)

# Initialize arrays to store results
battery_storage = np.zeros_like(time)
grid_import = np.zeros_like(time)
grid_export = np.zeros_like(time)

# Simulation
for t in range(len(time)):
    # Calculate excess or shortfall of energy
    excess_energy = solar_generation[t] - load_demand[t]

    # Update battery storage
    if excess_energy > 0:  # excess energy, try to charge battery
        charge_amount = min(excess_energy, battery_charge_rate, battery_capacity - battery_charge)
        battery_charge += charge_amount
        excess_energy -= charge_amount
        grid_export[t] = excess_energy  # export excess to the grid
    else:  # shortfall of energy, try to discharge battery
        discharge_amount = min(-excess_energy, battery_charge_rate, battery_charge)
        battery_charge -= discharge_amount
        grid_import[t] = -excess_energy - discharge_amount  # import shortfall from the grid

    # Store battery state
    battery_storage[t] = battery_charge

# Plotting the results
plt.figure(figsize=(12, 8))

plt.subplot(4, 1, 1)
plt.plot(time, solar_generation, label='Solar Generation (kW)', color='orange')
plt.ylabel('kW')
plt.legend()

plt.subplot(4, 1, 2)
plt.plot(time, load_demand, label='Load Demand (kW)', color='red')
plt.ylabel('kW')
plt.legend()

plt.subplot(4, 1, 3)
plt.plot(time, battery_storage, label='Battery Storage (kWh)', color='blue')
plt.ylabel('kWh')
plt.legend()

plt.subplot(4, 1, 4)
plt.plot(time, grid_import, label='Grid Import (kW)', color='green')
plt.plot(time, grid_export, label='Grid Export (kW)', color='purple')
plt.ylabel('kW')
plt.xlabel('Time (hours)')
plt.legend()

plt.tight_layout()
plt.show()

![image](https://github.com/user-attachments/assets/4ca6f9a9-586c-4762-8f36-b1c9ec7557c0)
import numpy as np
import matplotlib.pyplot as plt

# Time in hours (24 hours for a full day simulation)
time = np.arange(0, 24, 1)

# Simulated solar power generation (kW)
solar_generation = np.maximum(0, 10 * np.sin(np.pi * (time - 6) / 12))

# Simulated wind power generation (kW)
# Assuming more variability and possibly more power during the night
wind_generation = 3 + 2 * np.sin(np.pi * (time) / 24 + np.pi/4) + np.random.normal(0, 0.5, len(time))

# Load demand profiles (kW)
# Base load (constant)
base_load = np.full_like(time, 4)

# Variable load (simulates industrial activity, fluctuating during working hours)
variable_load = 2 * np.where((time >= 8) & (time <= 18), np.sin(np.pi * (time - 8) / 10), 0)

# Peak load (additional peak load in the evening)
peak_load = np.where((time >= 18) & (time <= 21), 3, 0)

# Total load demand
load_demand = base_load + variable_load + peak_load

# Initialize storage (battery) parameters
battery_capacity = 50  # in kWh
..........................................
.............................................
# Plotting the results
plt.figure(figsize=(12, 10))

plt.subplot(5, 1, 1)
plt.plot(time, solar_generation, label='Solar Generation (kW)', color='orange')
plt.ylabel('kW')
plt.legend()

plt.subplot(5, 1, 2)
plt.plot(time, wind_generation, label='Wind Generation (kW)', color='cyan')
plt.ylabel('kW')
plt.legend()

plt.subplot(5, 1, 3)
plt.plot(time, load_demand, label='Load Demand (kW)', color='red')
plt.ylabel('kW')
plt.legend()

plt.subplot(5, 1, 4)
plt.plot(time, battery_storage, label='Battery Storage (kWh)', color='blue')
plt.ylabel('kWh')
plt.legend()

plt.subplot(5, 1, 5)
plt.plot(time, grid_import, label='Grid Import (kW)', color='green')
plt.plot(time, grid_export, label='Grid Export (kW)', color='purple')
plt.ylabel('kW')
plt.xlabel('Time (hours)')
plt.legend()

plt.tight_layout()
plt.show()
![image](https://github.com/user-attachments/assets/7f60eddd-bf77-4277-bebd-f5fd92e62ea8)
import numpy as np
import matplotlib.pyplot as plt

# Function to simulate microgrid operation
def simulate_microgrid(time, solar_gen, wind_gen, load_demand, battery_capacity, battery_charge_rate):
    battery_charge = 20  # initial charge in kWh
    battery_storage = np.zeros_like(time)
    grid_import = np.zeros_like(time)
    grid_export = np.zeros_like(time)
    ............................................
    ..............................................
    ..................................................
    
# Plot results for each test case
plot_results(time, battery_storage, grid_import, grid_export, 'Test Case 1: Normal Operation')
plot_results(time, battery_storage_high_renewable, grid_import_high_renewable, grid_export_high_renewable, 'Test Case 2: High Renewable Generation')
plot_results(time, battery_storage_low_renewable, grid_import_low_renewable, grid_export_low_renewable, 'Test Case 3: Low Renewable Generation')
plot_results(time, battery_storage_peak_load, grid_import_peak_load, grid_export_peak_load, 'Test Case 4: Peak Load Demand')
plot_results(time, battery_storage_failure, grid_import_failure, grid_export_failure, 'Test Case 5: Battery Failure')
plot_results(time, battery_storage_increased_activity, grid_import_increased_activity, grid_export_increased_activity, 'Test Case 6: Increased Industrial Activity')

# For the extended time test case, we need to adjust the plot function slightly for 72 hours of data:
plt.figure(figsize=(12, 8))
plt.subplot(3, 1, 1)
plt.plot(extended_time, battery_storage_extended, label='Battery Storage (kWh)', color='blue')
plt.ylabel('kWh')
plt.legend()
plt.title('Test Case 7: Extended Cloudy Days')

plt.subplot(3, 1, 2)
plt.plot(extended_time, grid_import_extended, label='Grid Import (kW)', color='green')
plt.ylabel('kW')
plt.legend()

plt.subplot(3, 1, 3)
plt.plot(extended_time, grid_export_extended, label='Grid Export (kW)', color='purple')
plt.ylabel('kW')
plt.xlabel('Time (hours)')
plt.legend()

plt.tight_layout()
plt.show()

![image](https://github.com/user-attachments/assets/31a81dee-b743-49c5-8df0-d316608b33dd)

![image](https://github.com/user-attachments/assets/0c040fd5-5b1d-4f73-b3c6-561a13fe95d6)

![image](https://github.com/user-attachments/assets/fc13a1a7-7791-4488-8235-39b7a71edfce)
![image](https://github.com/user-attachments/assets/701d36fb-ce77-4236-80db-6495551a00a2)
![image](https://github.com/user-attachments/assets/4c0a3fc7-fd0f-4f0c-a4d3-0136d4bc4624)
![image](https://github.com/user-attachments/assets/c866843c-ee5d-4165-a914-1525e35d3536)
![image](https://github.com/user-attachments/assets/5cdc7fff-6a93-4ad5-8d69-931d9829e3c9)


*Optimization and Analysys *

![image](https://github.com/user-attachments/assets/460c838e-f5d3-496a-9072-4d63479bffc3)
![image](https://github.com/user-attachments/assets/9e8c066e-1a51-428d-8b2d-ffe7f82cb1f2)
![image](https://github.com/user-attachments/assets/0229be85-5e3a-4ea0-853e-652cc9dd76f6)

![image](https://github.com/user-attachments/assets/0bea1306-aef3-4b5f-8b5e-8e202e2349aa)













