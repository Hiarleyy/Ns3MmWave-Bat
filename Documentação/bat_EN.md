# Batman Algorithm  🦇 🦇 🦇

### Language | Idioma
**English** | [Português](bat.md)

The [code](https://github.com/Hiarleyy/Ns-3-simulations/blob/main/batman.py) implements a simplified version of the BAT Algorithm for optimizing user positioning in an internet network, considering the presence of an antenna and user movement. Let's analyze what each part of the code does:

### Imports and Constant Definition
```python
import numpy as np
import matplotlib.pyplot as plt
```

#### Defining constants
```python
NUM_USERS = 2
DIMENSIONS = 2  # x, y
BAT_POPULATION = 30
MAX_ITER = 1000
```
Here the necessary libraries are imported and constants are defined for the number of users, space dimensions (2D), bat population size, and maximum number of iterations.

### Auxiliary Functions

#### Signal Intensity Function
```python
def signal_intensity(antenna_pos, user_pos):
    distance = np.linalg.norm(antenna_pos - user_pos, axis=1)
    return 1 / (distance ** 2)
```
This function calculates signal intensity based on the distance between the antenna and user positions.

#### Application Priority Function
```python
def application_priority(app_type):
    priorities = {'video': 1.0, 'voice': 0.8, 'data': 0.5}
    return priorities.get(app_type, 0.5)
```

This function assigns a priority to each application type (video, voice, data).

#### Fitness Function
```python
def fitness(antenna_pos, user_positions, user_apps):
    intensities = signal_intensity(antenna_pos, user_positions)
    priorities = np.array([application_priority(app) for app in user_apps])
    return np.sum(intensities * priorities)
```

The fitness function evaluates the quality of user positions based on signal intensity and application priorities.

### Bat Initialization
```python
def initialize_bats(population_size, num_users, dimensions, min_dist=50, max_dist=450):
    bats = []
    for _ in range(population_size):
        bat = []
        for _ in range(num_users):
            angle = np.random.uniform(0, 2 * np.pi)
            distance = np.random.uniform(min_dist, max_dist)
            x = distance * np.cos(angle)
            y = distance * np.sin(angle)
            bat.append([x, y])
        bats.append(np.array(bat))
    return bats
```

This function initializes the bat population, where each bat represents a possible solution for user positioning around the antenna.

### Position Update
```python
def update_positions(bats, velocities, frequencies, best_solution, loudness, pulse_rate):
    for i in range(len(bats)):
        frequencies[i] = np.random.uniform(0, 2)
        velocities[i] = velocities[i] + (bats[i] - best_solution) * frequencies[i]
        new_position = bats[i] + velocities[i]
        
        if np.random.rand() > pulse_rate[i]:
            new_position = best_solution + np.random.uniform(-1, 1, new_position.shape) * np.mean(loudness)
        
        bats[i] = new_position
    return bats, velocities
```

This function updates bat positions based on the algorithm's movement equations.

### Main BAT Algorithm
```python
def bat_algorithm(antenna_pos, initial_user_positions, user_apps):
    bats = initialize_bats(BAT_POPULATION, NUM_USERS, DIMENSIONS)
    velocities = [np.zeros_like(bat) for bat in bats]
    frequencies = np.zeros(BAT_POPULATION)
    loudness = np.random.uniform(1, 2, BAT_POPULATION)
    pulse_rate = np.random.uniform(0, 1, BAT_POPULATION)
    
    best_fitness = -np.inf
    best_solution = initial_user_positions.copy()
    fitness_history = []
    
    for iteration in range(MAX_ITER):
        for i in range(BAT_POPULATION):
            current_fitness = fitness(antenna_pos, bats[i], user_apps)
            
            if current_fitness > best_fitness and np.random.rand() < loudness[i]:
                best_solution = bats[i].copy()
                best_fitness = current_fitness
                loudness[i] *= 0.95
                pulse_rate[i] = pulse_rate[i] * (1 - np.exp(-2 * iteration))
        
        bats, velocities = update_positions(bats, velocities, frequencies, best_solution, loudness, pulse_rate)
        fitness_history.append(best_fitness)
    
    return best_solution, fitness_history
```

The main algorithm that coordinates all operations and searches for the optimal solution.

### Plotting and Usage Examples
```python
def plot_results(antenna_pos, original_positions, optimized_positions):
    plt.figure(figsize=(12, 5))
    
    plt.subplot(1, 2, 1)
    plt.scatter(antenna_pos[0], antenna_pos[1], c='red', s=100, marker='^', label='Antenna')
    plt.scatter(original_positions[:, 0], original_positions[:, 1], c='blue', s=50, label='Original Users')
    plt.title('Original Positions')
    plt.xlabel('X Position')
    plt.ylabel('Y Position')
    plt.legend()
    plt.grid(True)
    
    plt.subplot(1, 2, 2)
    plt.scatter(antenna_pos[0], antenna_pos[1], c='red', s=100, marker='^', label='Antenna')
    plt.scatter(optimized_positions[:, 0], optimized_positions[:, 1], c='green', s=50, label='Optimized Users')
    plt.title('Optimized Positions')
    plt.xlabel('X Position')
    plt.ylabel('Y Position')
    plt.legend()
    plt.grid(True)
    
    plt.tight_layout()
    plt.show()
```

This function creates visualizations comparing original and optimized positions.

# Usage Example
```python
antenna_pos = np.array([250, 250])
user_positions = initialize_bats(1, NUM_USERS, DIMENSIONS, min_dist=50, max_dist=450)[0]
user_apps = np.random.choice(['low', 'medium', 'high'], NUM_USERS)

new_user_positions, fitness_history = bat_algorithm(antenna_pos, user_positions, user_apps)
plot_results(antenna_pos, user_positions, new_user_positions)

print("Antenna Position:", antenna_pos)
print("Original User Positions:")
for i, (pos, app) in enumerate(zip(user_positions, user_apps)):
    print(f"User {i+1}: Position: {pos}, Application Priority: {app}")
print("Optimized User Positions:")
for i, (pos, app) in enumerate(zip(new_user_positions, user_apps)):
    print(f"User {i+1}: Position: {pos}, Application Priority: {app}")
print("Optimized user displacement:")
for i in range(NUM_USERS):
    displacement = np.linalg.norm(new_user_positions[i] - user_positions[i])
    print(f"User {i+1}: {displacement:.2f}")
```

This usage example demonstrates how to use the algorithm to optimize user positioning, showing original positions, optimized positions, and the displacement of each user.