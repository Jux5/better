# Double Pendulum Simulator

## UPTDATE!!! rk45
I've implemented a Runge-Kutta-Fehlber thing
Idk, where this guy got the coefficients from but this thing works like butter

## Overview

This simulator models the motion of a double pendulum system using numerical integration techniques. It generates simulation data, preserves energy accuracy, and supports adaptive step-size integration for improved precision.

---

## Requirements

- **Python 3.x**
- **NumPy** (for numerical calculations)

---

## Features

### **Data Generation**

- **`gen_test`**
  - Generates a CSV file (`test<i>.csv`) with simulation data based on initial conditions.
  - The file contains the following columns:
    - `t`: Time
    - `phi1`: Angle of the first pendulum
    - `v1`: Angular velocity of the first pendulum
    - `phi2`: Angle of the second pendulum
    - `v2`: Angular velocity of the second pendulum

### **Initial Condition Generator**

- **`gen_y0`**
  - Creates random initial states (`y0`) with angular velocities (`v1` and `v2`) set to zero.

### **Simulation Methods**

1. **`gen(y0)`**
   - Simulates the motion of the double pendulum using the classical RK4 method with a fixed time step of `0.01`.
   - Outputs:
     - `phi1`, `v1`, `phi2`, `v2`: Angles and angular velocities
     - `t`: Time

2. **`gen_energy(y0)`**
   - Similar to `gen(y0)`, but applies a correction to ensure energy conservation:
     - If the energy deviates significantly from the initial energy, the system scales velocities to restore the energy balance.

3. **`gen_energy_adaptive(y0)`** *(Flagship Method)*  
   - Uses adaptive RK4 integration for precise energy preservation:
     - Dynamically adjusts the time step (`dt`) based on error thresholds.
     - Ensures smooth integration even in chaotic regions where energy deviation is more likely.
   - Outputs the same results as `gen_energy(y0)` but is more accurate at the cost of computation time.

---

## Core Functions

### **`rk4_adaptive_step`**
- Implements an adaptive Runge-Kutta method for a single time step:
  1. Computes a **full RK4 step** from `t` to `t + dt`.
  2. Computes **two half RK4 steps** from `t` to `t + dt/2` and `t + dt/2` to `t + dt`.
  3. Compares results to estimate the error and evaluates the energy deviation.
  4. Adjusts `dt` dynamically:
     - If the error or energy deviation exceeds a threshold, the time step is reduced, and the function recurses.
     - If the error is very low, the time step increases for efficiency.
- This ensures energy preservation without relying on manual corrections.

---

