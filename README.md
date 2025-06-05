# Neural Network Error Rate Analysis Script

## 1. Overview

This Python script simulates a simplified model of a neural network to analyze its error rates under different conditions. It specifically investigates how the network's ability to correctly represent input stimuli is affected by:

* **Set Size:** The number of active input stimuli.
* **Stimuli Proximity:** Whether the input stimuli are spatially close or far apart on a grid.
* **Receptive Field (RF) Size:** A parameter influencing the range of inhibitory connections within the network (categorized as low RF and high RF).

The script translates an original Octave implementation into Python, using NumPy for numerical computations and Matplotlib for generating plots. The primary error metric used is the **Percentage Hamming Distance**, which measures the discrepancy between the network's final state and the initial input pattern, normalized by the number of active inputs.

## 2. How it Works

The simulation models a grid of `N` nodes (typically 64, forming an 8x8 grid). The core logic involves:

1.  **Input Generation:**
    * `generate_close_inputs`: Creates input patterns where a specified number of active nodes are located near the center of the grid.
    * `generate_far_inputs`: Creates input patterns where active nodes are located further away from the center.
2.  **Network Dynamics:**
    * The state of each node (`x`) evolves over discrete time steps (`dt`) for a total simulation time (`T_total`).
    * The update rule for each node's state includes:
        * Decay term (`-x`).
        * Self-excitation via an activation function (`alpha * F_x`).
        * Lateral inhibition from other nodes, where the strength of inhibition depends on the Euclidean distance between nodes and the `receptive_field_size` (`beta_matrix * F_x`).
        * The external input pattern (`current_input`), which is applied for an initial transient period (`T_input`).
        * Gaussian noise (`noise`).
3.  **Error Calculation:**
    * After the simulation period, the final state of the network is binarized (nodes with activity > 0.5 are considered 'on').
    * The Percentage Hamming Distance is calculated by comparing this binarized final output to the original input pattern. This distance is normalized by the number of 'on' bits in the input pattern and then multiplied by 100.
4.  **Averaging:**
    * For each combination of parameters (set size, stimuli proximity, RF size), the simulation is run multiple times (`num_simulations`), and the average percentage error is computed.
5.  **Plotting:**
    * The script generates two plots:
        * One for low receptive field sizes.
        * One for high receptive field sizes.
    * Each plot shows the average Hamming Distance error as a function of the set size, with separate lines for 'close' and 'far' stimuli. Both plots share the same y-axis scale (Hamming Distance) for direct comparison.

## 3. Key Parameters

Editable parameters are located at the beginning of the `error_rate_analysis_py()` function:

* `N`: Total number of nodes in the grid (e.g., 64 for an 8x8 grid).
* `dt`: Time step for the simulation.
* `T_total`: Total duration of each simulation run.# Neural Network Error Rate Analysis Script
