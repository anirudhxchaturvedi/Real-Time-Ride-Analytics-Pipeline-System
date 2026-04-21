# Real-Time Ride Analytics Pipeline System

A comprehensive data engineering solution for processing and analyzing Uber-like ride data using a modern **Medallion Architecture** (Bronze → Silver → Gold layers) with real-time streaming capabilities.

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Architecture](#-architecture)
- [Technology Stack](#-technology-stack)
- [Project Structure](#-project-structure)
- [Getting Started](#-getting-started)
- [Installation](#-installation)
- [Configuration](#-configuration)
- [Usage](#-usage)
- [Data Pipeline](#-data-pipeline)
- [API Endpoints](#-api-endpoints)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🎯 Project Overview

This project demonstrates a **production-ready data analytics pipeline** that ingests ride data, performs quality validation, transforms it into business-ready formats, and exposes insights through a modern REST API.

### Key Capabilities

- ✅ **Real-time Data Ingestion** - Stream processing from Azure Event Hubs
- ✅ **Data Quality Validation** - Automated cleansing and standardization
- ✅ **Medallion Architecture** - Enterprise-grade data layering
- ✅ **Dimension & Fact Modeling** - Star schema for analytics
- ✅ **Change Data Capture** - Track data modifications over time
- ✅ **REST API** - FastAPI endpoints for data access
- ✅ **Web Dashboard** - HTML templates for visualization
- ✅ **Scalable Processing** - Apache Spark integration

---

## 🏗️ Architecture

### Data Flow

```
Raw Data (JSON)
    ↓
[BRONZE LAYER] - Raw Data Vault
    ├─ uber.bronze.bulk_rides
    └─ uber.bronze.silver_obt
    ↓
[SILVER LAYER] - Cleaned & Validated
    ├─ Data quality checks
    ├─ Duplicate removal
    ├─ Reference data mapping
    └─ OBT (One Big Table) format
    ↓
[GOLD LAYER] - Business-Ready Analytics
    ├─ Dimensions
    │  ├─ dim_payment_view
    │  ├─ dim_booking_view
    │  └─ dim_location_view
    └─ Facts
       └─ fact_view
    ↓
[API & WEB] - Exposure Layer
    ├─ FastAPI REST endpoints
    ├─ HTML templates
    └─ Real-time dashboards
```

### Medallion Layers

| Layer | Purpose | Key Tables |
|-------|---------|-----------|
| **Bronze** | Raw data storage | bulk_rides, silver_obt |
| **Silver** | Validated & cleaned | Transformed OBT tables |
| **Gold** | Business analytics | Dimensions & Facts |

---

## 💻 Technology Stack

### Core Technologies

| Component | Technology | Version |
|-----------|-----------|---------|
| **Cloud Platform** | Microsoft Azure | - |
| **Data Storage** | Azure Blob Storage, Delta Lake | Latest |
| **Stream Processing** | Azure Event Hubs | 5.15.1 |
| **Distributed Computing** | Apache Spark | 3.x |
| **Web Framework** | FastAPI | 0.133.1 |
| **Data Validation** | Pydantic | 2.12.5 |
| **Notebooks** | Jupyter/Databricks | - |

### Python Dependencies

```
fastapi==0.133.1
pydantic==2.12.5
azure-eventhub==5.15.1
azure-core==1.38.2
pandas
sqlalchemy
faker==40.5.1
pyyaml==6.0.3
requests==2.32.5
uvicorn
```

---

## 📁 Project Structure

```
Real-Time Ride Analytics Pipeline System/
│
├── Code_Files/
│   ├── bronze_adls.ipynb ............. Initial data ingestion
│   ├── ingest.py ..................... Data extraction script
│   ├── silver.py ..................... Data cleansing
│   ├── silver_obt.ipynb .............. OBT transformation
│   ├── silver_obt.sql ................ SQL transformations
│   └── model.py ...................... Dimension & Fact definitions
│
├── Data/
│   ├── bulk_rides.json ............... 5000+ ride records
│   ├── map_cities.json ............... City mapping
│   ├── map_payment_methods.json ...... Payment methods
│   ├── map_ride_statuses.json ........ Ride statuses
│   ├── map_vehicle_types.json ........ Vehicle types
│   ├── map_vehicle_makes.json ........ Manufacturers
│   └── map_cancellation_reasons.json . Cancellation codes
│
├── templates/
│   ├── home.html ..................... Landing page
│   └── confirmation.html ............. Confirmation page
│
├── api.py ............................ FastAPI application
├── connection.py ..................... Database connectivity
├── data.py ........................... Data models
├── requirements.txt .................. Python dependencies
├── pyproject.toml .................... Project config
└── README.md ......................... This file
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.8+
- Azure subscription (for cloud components)
- Databricks workspace (optional, for Spark jobs)
- Git

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd "Real-Time Ride Analytics Pipeline System"
   ```

2. **Create virtual environment**
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Verify installation**
   ```bash
   pip list | grep -E "fastapi|pydantic|azure"
   ```

---

## ⚙️ Configuration

### Environment Variables

Create a `.env` file in the project root:

```env
# Azure Configuration
AZURE_BLOB_CONNECTION_STRING=<your-connection-string>
AZURE_BLOB_CONTAINER=rides-data
AZURE_EVENT_HUB_CONNECTION_STRING=<your-eventhub-connection>
AZURE_EVENT_HUB_NAME=rides-events

# Database Configuration
DATABASE_HOST=<your-host>
DATABASE_PORT=5432
DATABASE_NAME=uber_analytics
DATABASE_USER=<your-user>
DATABASE_PASSWORD=<your-password>

# API Configuration
API_HOST=0.0.0.0
API_PORT=8000
DEBUG=False
```

### Database Connection

Edit `connection.py` with your database credentials:

```python
DATABASE_URL = "postgresql://user:password@localhost/uber_analytics"
```

---

## 📊 Usage

### 1. Data Ingestion

**Run the ingestion pipeline:**

```bash
python Code_Files/ingest.py
```

This will:
- Extract data from Azure Blob Storage
- Load to Bronze layer (Delta tables)
- Create initial data catalog

### 2. Data Transformation

**Run Jupyter notebooks in order:**

```bash
# Step 1: Load to Bronze layer
jupyter notebook Code_Files/bronze_adls.ipynb

# Step 2: Transform to Silver layer
jupyter notebook Code_Files/silver_obt.ipynb
```

Or use CLI:

```bash
python Code_Files/silver.py
```

### 3. Data Modeling

**Create Dimensions & Facts:**

```bash
python Code_Files/model.py
```

Creates:
- `dim_payment_view` - Payment dimension
- `dim_booking_view` - Booking dimension
- `dim_location_view` - Location dimension
- `fact_view` - Ride facts table

### 4. Start API Server

**Launch FastAPI application:**

```bash
uvicorn api:app --host 0.0.0.0 --port 8000 --reload
```

Access API at: `http://localhost:8000`

---

## 🔌 API Endpoints

### Available Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/` | Home page |
| `GET` | `/rides` | List all rides |
| `GET` | `/rides/{ride_id}` | Get ride details |
| `GET` | `/analytics` | Analytics dashboard |
| `GET` | `/metrics` | Aggregate metrics |
| `GET` | `/status` | System health check |
| `POST` | `/rides` | Create new ride |
| `GET` | `/docs` | Interactive API documentation (Swagger) |

### Example API Calls

**Get all rides:**
```bash
curl http://localhost:8000/rides
```

**Get ride metrics:**
```bash
curl http://localhost:8000/metrics
```

**View interactive docs:**
```bash
# Open in browser
http://localhost:8000/docs
```

---

## 📈 Data Pipeline Details

### Bronze Layer (Raw)
- **Purpose**: Store raw data as-is
- **Tables**: 
  - `uber.bronze.bulk_rides` - Original ride records
  - `uber.bronze.silver_obt` - Pre-transformed OBT
- **Update**: Append-only

### Silver Layer (Validated)
- **Purpose**: Clean, deduplicated data
- **Transformations**:
  - Remove duplicates
  - Handle null values
  - Validate schemas
  - Map reference data
  - Data quality checks
- **Update**: Merge/Upsert

### Gold Layer (Analytics)

**Dimensions** (SCD Type 1 & 2):
- `dim_payment_view` - Payment methods (Type 1)
- `dim_booking_view` - Ride details (Type 1)
- `dim_location_view` - Cities/locations (Type 2)

**Facts** (SCD Type 1):
- `fact_view` - Ride transactions with metrics
  - Measures: fare, distance, duration, rating, tip
  - Foreign keys: ride_id, city_id, payment_id, driver_id

---

## 🔍 Sample Data

### Ride Record Example

```json
{
  "ride_id": "ride_12345",
  "passenger_id": "pass_001",
  "driver_id": "driv_001",
  "pickup_location": "Downtown",
  "dropoff_location": "Airport",
  "pickup_datetime": "2026-04-21T10:30:00Z",
  "distance_miles": 15.5,
  "duration_minutes": 25,
  "base_fare": 5.00,
  "distance_fare": 31.00,
  "time_fare": 12.50,
  "surge_multiplier": 1.2,
  "tip_amount": 5.00,
  "total_fare": 54.30,
  "payment_method": "credit_card",
  "rating": 4.8,
  "ride_status": "completed"
}
```

---

## 📝 Key Data Dimensions

### Cities
- New York, Los Angeles, Chicago, Houston, Phoenix, Philadelphia, San Antonio, San Diego, Dallas, Austin

### Payment Methods
- Credit Card, Debit Card, Digital Wallet, Cash

### Vehicle Types
- UberX, UberXL, UberEats, Uber Black, Uber Green

### Ride Status
- Completed, Cancelled, No-Show, Scheduled

---

## 🛠️ Troubleshooting

### Common Issues

**Connection to Azure fails:**
```bash
# Verify credentials
echo $AZURE_BLOB_CONNECTION_STRING
# Check network connectivity
ping blob.core.windows.net
```

**FastAPI port already in use:**
```bash
# Kill process on port 8000
lsof -ti:8000 | xargs kill -9
# Or use different port
uvicorn api:app --port 8001
```

**Pydantic validation errors:**
```bash
# Verify data schemas
python -c "from data import RideModel; print(RideModel.schema())"
```

---

## 📚 Documentation

- **Spark Docs**: https://spark.apache.org/
- **FastAPI**: https://fastapi.tiangolo.com/
- **Azure Services**: https://docs.microsoft.com/azure/
- **Delta Lake**: https://delta.io/

---

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit changes (`git commit -m 'Add AmazingFeature'`)
4. Push to branch (`git push origin feature/AmazingFeature`)
5. Open Pull Request
