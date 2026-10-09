# BatteryPulse: Battery Digital Twin Platform

BatteryPulse is a comprehensive battery digital twin platform that provides real-time monitoring, prediction, and simulation capabilities for lithium-ion battery packs. The platform combines physics-based models with machine learning to estimate State of Health (SOH), Remaining Useful Life (RUL), and detect anomalies in battery performance.

## Features

- **Real-time Monitoring**: Live telemetry data visualization
- **SOH Prediction**: Machine learning-based State of Health estimation
- **RUL Prediction**: Remaining Useful Life forecasting
- **Anomaly Detection**: Real-time anomaly scoring for early fault detection
- **What-If Simulation**: Simulate battery behavior under different conditions
- **Data Validation**: CSV upload and validation for external data
- **RESTful API**: Comprehensive API for integration with other systems
- **Interactive Dashboard**: React-based UI for easy data visualization

## Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌────────────────────┐
│   Frontend      │    │    Backend API   │    │     ML Models      │
│ (React/Vite/TS) │◄─►│ (FastAPI/Python) │◄─►│ (Scikit-learn/NumPy) │
└─────────────────┘    └──────────────────┘    └────────────────────┘
        │                         │                         │
        │                         ▼                         │
        │              ┌────────────────────┐                │
        │              │   Synthetic Data   │                │
        │              │   Generator        │                │
        │              └────────────────────┘                │
        │                         │                         │
        ▼                         ▼                         ▼
┌─────────────────┐    ┌──────────────────┐    ┌────────────────────┐
│   Docker        │    │   Kubernetes     │    │      CI/CD         │
│   Compose       │    │   Manifests      │    │ (GitHub Actions)   │
└─────────────────┘    └──────────────────┘    └────────────────────┘
```

## Getting Started

### Prerequisites

- Docker and Docker Compose
- Git (for cloning the repository)

### Installation

1. Clone the repository:
   ```bash
   git clone <repository-url>
   cd batterypulse-digital-twin
   ```

2. Start the services:
   ```bash
   make up
   ```

3. Access the applications:
   - Frontend Dashboard: http://localhost:3000
   - Backend API: http://localhost:8000
   - API Documentation: http://localhost:8000/docs

### Development

```bash
# Start backend in development mode
make dev-backend

# Start frontend in development mode
make dev-frontend

# Run tests
make test

# Lint code
make lint

# Format code
make format
```

## API Endpoints

### Battery Management
- `POST /packs/` - Create a new battery pack
- `GET /packs/{pack_id}` - Get battery pack details
- `POST /packs/{pack_id}/cells/` - Add a cell to a pack
- `GET /cells/{cell_id}` - Get cell details

### Telemetry and Cycles
- `POST /telemetry/` - Add telemetry data point
- `POST /cells/{cell_id}/cycles/` - Add cycle data
- `GET /cells/{cell_id}/soh` - Get State of Health prediction
- `GET /cells/{cell_id}/rul` - Get Remaining Useful Life prediction
- `GET /cells/{cell_id}/anomaly-score` - Get anomaly score

### Simulation and Analysis
- `POST /what-if-simulation/` - Run what-if simulation
- `POST /validate-csv/` - Validate and process CSV upload

## Model Descriptions

### State of Health (SOH) Model
Estimates the current capacity relative to nominal capacity using:
- Voltage characteristics
- Current profiles
- Temperature effects
- Cycle count

### Remaining Useful Life (RUL) Model
Predicts the number of cycles remaining until end-of-life (80% SOH) based on:
- Historical degradation trends
- Operating conditions (temperature, current)
- Usage patterns

### Anomaly Detection Model
Identifies abnormal battery behavior using:
- Voltage thresholds (2.5V - 4.2V)
- Current thresholds (-5A to 5A)
- Temperature thresholds (-20°C to 60°C)
- Statistical deviation from normal patterns

## Data Models

### Battery Pack
- `name`: Pack identifier
- `nominal_voltage`: Total pack voltage
- `nominal_capacity`: Total pack capacity (Ah)
- `cell_count`: Number of cells
- `configuration`: Series/parallel configuration (e.g., "4S2P")

### Battery Cell
- `pack_id`: Associated pack identifier
- `cell_number`: Cell number within pack
- `nominal_voltage`: Nominal cell voltage (V)
- `nominal_capacity`: Nominal cell capacity (Ah)
- `chemistry`: Battery chemistry (default: Li-ion)

### Cycle Data
- `cell_id`: Associated cell identifier
- `cycle_index`: Cycle number
- `voltage_mean/min/max`: Voltage statistics (V)
- `current_mean/min/max`: Current statistics (A)
- `temperature_mean/min/max`: Temperature statistics (°C)
- `capacity`: Discharge capacity (Ah)
- `energy`: Energy throughput (J)

### Telemetry Data
- `cell_id`: Associated cell identifier
- `voltage`: Instantaneous voltage (V)
- `current`: Instantaneous current (A)
- `temperature`: Instantaneous temperature (°C)
- `capacity`: Current capacity (Ah)
- `soc`: State of Charge (0-1)

## Deployment

### Docker Compose
The platform can be deployed using Docker Compose for development and testing:
```bash
docker-compose up -d
```

### Kubernetes
Production deployment manifests are available in the `k8s/` directory:
```bash
kubectl apply -f k8s/
```

## Sample Data Generation

Generate sample data for testing and demonstration:
```bash
python sample_data/generate_sample_data.py
```

This creates sample JSON and CSV files in the `sample_data/` directory.

## License

MIT License - see the LICENSE file for details.

## Contact

For questions and support, please open an issue on the GitHub repository.