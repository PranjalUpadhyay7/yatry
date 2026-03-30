# Yatry 🛺

**Hassle-Free Ride Sharing powered by Advanced Optimization Algorithms**

[![Project Report](https://img.shields.io/badge/Project--Report-Read_PDF-blue.svg)](./dse311_yatry.pdf)
[![Project Presentation](https://img.shields.io/badge/Project--Presentation-View_Slides-orange.svg)](./yatry_presentation.pdf)

> Developed by Arpan Jain, Pranjal Upadhyay, Rugved Upaddhye, Sattwik Sahu (IISER Bhopal Student Community).

## 📖 Overview

In urban environments, optimizing shared transportation is crucial for reducing costs and improving efficiency, especially for students. **Yatry** approaches this by transforming the city's frequently visited locations and estimated fares into a weighted tree structure. Passengers' destinations and preferred timings are analyzed to compute trip similarity scores, facilitating the clustering of individuals into compatible shared rides. Dynamic programming techniques are utilized to determine optimal seating arrangements when group sizes exceed vehicle capacity. Finally, an optimal departure time is calculated for each grouped vehicle by integrating individual time convenience functions.

## ✨ Key Features & Methodology

Our pipeline consists of several distinct algorithmic optimization stages:

1. **Map Modeling & Routing (Tree Structure)** 🗺️
   - The city map (focused on IISER Bhopal and surrounding areas) is modeled as a weighted tree based on frequent locations and inter-location estimated fares.
   - Routing algorithms optimally extract shared segments crossing the campus paths.

2. **Passenger Affinity Matrix Construction** 🔗
   - **Time Affinity (`$\tau$`)**: Scores how closely two passengers' preferred departure time window distributions align.
   - **Route Affinity (`$\rho$`)**: Scores how much route overlap two passengers share, maximizing shared savings.
   - A combined affinity matrix ($\tau \times \rho$) is utilized to measure the overall compatibility between any pair of passengers.

3. **Clustering via Affinity Propagation** 🧩
   - Passengers are intuitively clustered using Scikit-Learn's `AffinityPropagation` model to group those with compatible spatial routes and temporal schedules.
   - The algorithm runs iteratively to guarantee stable group combinations without manual thresholds.

4. **Fair Cost Allocation & Vehicle Assignment (Dynamic Programming)** ⚖️
   - If a resulting group size exceeds the vehicle’s maximum capacity (e.g. 3 or 4 people for a standard rickshaw), an advanced vehicle assignment model employing dynamic programming optimally splits the group into smaller subgroups. 
   - The split minimizes the worst-case (maximum) fare any single individual pays. Costs for each shared route segment are explicitly split proportionally among the co-passengers of that given segment.

5. **Optimal Departure Time Calculation** ⏱️
   - Integrates the individual time convenience functions of all passengers assigned to a specific Rickshaw.
   - It calculates a precise, single optimal time of departure that minimizes collective wait times and maximizes group convenience.

## 🚀 Getting Started

### Prerequisites

1. [Install `uv`](https://docs.astral.sh/uv/getting-started/installation/) for dependency management:
   ```bash
   curl -LsSf https://astral.sh/uv/install.sh | sh
   ```

### Setup & Running the Pipeline

1. Fork the repo and clone your fork:
   ```bash
   git clone https://github.com/\<your_username\>/yatry
   ```
2. Create the virtual environment and install dependencies:
   ```bash
   uv sync
   ```
   > `uv` will install the required Python version automatically from `pyproject.toml`.

3. Run the complete optimization pipeline locally:
   ```bash
   python src/yatry/utils/pipeline.py
   ```

---

## 📂 Project Structure Overview

The underlying methodology corresponds directly to the optimized modules mapped out under `src/yatry/`:

```text
src/yatry/
├── utils/
│   ├── pipeline.py       # Main optimization pipeline orchestrating the entire flow
│   ├── optim/
│   │   ├── clustering.py # Aggregates passengers using customized AffinityPropagation
│   │   ├── assign.py     # DP algorithm for optimal capacity-constrained auto assignments
│   │   ├── time.py       # Differential equations calculating precise departure schedules
│   │   ├── fare.py       # Fair cost allocation dividing segmented costs proportionally 
│   ├── data/
│   │   ├── map.py        # Graph instantiation of the city transport network (BHOPAL)
│   │   ├── locations.py  # Constants for landmark locations (IISERB, BAIRAGARH, etc.)
│   ├── models/
│   │   ├── tree.py       # Tree operations finding structural path queries
│   │   ├── map.py        # Higher-level abstraction resolving routes
```

## 🤝 Contributing

1. Set up remote to sync changes with your fork:
   ```bash
   git remote add origin https://github.com/\<your_username\>/yatry
   ```
2. Create a new branch in your fork:
   ```bash
   git checkout -b <your_branch_name>
   ```
3. Commit your changes:
   ```bash
   git add .
   git commit -m "<your_commit_message>"
   ```
   > To know more about meaningful commit messages, follow [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/#summary).
4. Push your changes to your fork and create a pull request.

---

Made with ❤️ at IISER Bhopal. For a deeper theoretical understanding of the math and algorithm proofs, refer to the attached detailed [Project Report](./dse311_yatry.pdf) and the comprehensive [Project Presentation](./yatry_presentation.pdf).
