# Weather Forecasting Web Application

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-3.0-green)](https://flask.palletsprojects.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A Flask web application for weather forecasting using OpenWeatherMap API, featuring an interactive dashboard with weather predictions and analytics.

## 🌟 Features

- **Real-Time Weather Data**: Integration with OpenWeatherMap API
- **5-Day Forecasting**: Detailed weather predictions
- **Interactive Dashboard**: Visualization of weather patterns and model performance
- **RESTful API**: Complete API for programmatic access
- **Weather Analytics**: Historical weather data tracking

## 📋 Table of Contents

- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [API Documentation](#api-documentation)
- [Project Structure](#project-structure)
- [License](#license)

## 🚀 Installation

### Prerequisites

- Python 3.8 or higher
- pip package manager
- OpenWeatherMap API key (free tier available)

### Step 1: Clone the Repository

```bash
git clone <repository-url>
cd "Weather Forecasting App"
```

### Step 2: Create Virtual Environment

```bash
# On macOS/Linux
python3 -m venv venv
source venv/bin/activate

# On Windows
python -m venv venv
venv\Scripts\activate
```

### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 4: Set Up Environment Variables

Create a `.env` file in the project root:

```env
OPENWEATHER_API_KEY=your_openweathermap_api_key_here
SECRET_KEY=your_secret_key_here
DEBUG=True
PORT=5000
```

To get an OpenWeatherMap API key:
1. Sign up at [OpenWeatherMap](https://openweathermap.org/api)
2. Navigate to API keys section
3. Generate a new API key (free tier available)

### Step 5: Initialize Database

```bash
python -c "from app import create_app, db; app = create_app(); app.app_context().push(); db.create_all()"
```

## ⚙️ Configuration

The application can be configured through `config.py` or environment variables:

```python
# Weather Event Thresholds
HEATWAVE_THRESHOLD = 35  # Celsius
STORM_WIND_THRESHOLD = 63  # km/h
FLOOD_PRECIPITATION_THRESHOLD = 50  # mm per day

# Application Settings
DEBUG = True  # Set to False in production
PORT = 5000
HOST = '0.0.0.0'
```

## 🎯 Usage

### Running the Application

#### Development Mode

```bash
python app.py
```

The application will be available at `http://localhost:5000`

#### Production Mode

```bash
export FLASK_ENV=production
gunicorn -w 4 -b 0.0.0.0:5000 "app:create_app()"
```

### Using the Web Interface

1. **Home Page** (`/`): Overview of the weather forecasting system
2. **Predict Page** (`/predict`): Enter location coordinates for 5-day forecast
3. **Dashboard** (`/dashboard`): View weather analytics and model performance
4. **About Page** (`/about`): Information about the application

### Quick Start Example

1. Navigate to `/predict`
2. Enter coordinates:
   - Latitude: 40.7128
   - Longitude: -74.0060
   - Location Name: New York City (optional)
3. Click "Generate Prediction"
4. View the 5-day weather forecast

## 📡 API Documentation

### Base URL
```
http://localhost:5000/api
```

### Endpoints

#### 1. Health Check
```http
GET /api/health
```

**Response:**
```json
{
  "status": "healthy",
  "timestamp": "2025-10-23T12:00:00Z"
}
```

#### 2. Generate 5-Day Forecast
```http
POST /api/predict
Content-Type: application/json

{
  "latitude": 40.7128,
  "longitude": -74.0060,
  "location_name": "New York City"
}
```

**Response:**
```json
{
  "success": true,
  "location": {
    "latitude": 40.7128,
    "longitude": -74.0060,
    "name": "New York",
    "country": "US",
    "sunrise": "06:45:30",
    "sunset": "18:20:15"
  },
  "predictions": [
    {
      "day_offset": 0,
      "date": "Wednesday, Oct 23",
      "temperature": 22.5,
      "feels_like": 21.8,
      "temp_min": 18.3,
      "temp_max": 24.7,
      "humidity": 65,
      "pressure": 1013,
      "wind_speed": 5.2,
      "wind_deg": 180,
      "cloudiness": 40,
      "visibility": 10000,
      "weather_description": "Partly Cloudy",
      "weather_icon": "02d",
      "precipitation": 0
    }
  ],
  "generated_at": "2025-10-23T12:00:00Z"
}
```

#### 3. Get Current Weather
```http
GET /api/weather/current?latitude=40.7128&longitude=-74.0060
```

**Response:**
```json
{
  "success": true,
  "data": {
    "timestamp": "2025-10-23T12:00:00Z",
    "latitude": 40.7128,
    "longitude": -74.006,
    "location_name": "New York",
    "temperature": 22.5,
    "humidity": 65,
    "pressure": 1013,
    "wind_speed": 5.2,
    "weather_description": "clear sky",
    "weather_icon": "01d"
  }
}
```

#### 4. Get Historical Weather Data
```http
GET /api/weather/historical?latitude=40.7128&longitude=-74.0060&start_date=2025-10-01&end_date=2025-10-22
```

#### 5. Get Prediction History
```http
GET /api/predictions/history?limit=50
```

#### 6. Get Model Performance Metrics
```http
GET /api/models/performance
```

## 📁 Project Structure

```
Vijender Weather Forecasting App/
├── app.py                      # Main Flask application entry point
├── config.py                   # Configuration settings
├── routes.py                   # Flask routes and API endpoints
├── models.py                   # Database models (SQLAlchemy)
├── requirements.txt            # Python dependencies
├── .env                        # Environment variables (not in git)
├── .env.example               # Environment variables template
├── .gitignore                 # Git ignore rules
│
├── templates/                  # HTML templates
│   ├── base.html              # Base template with navbar
│   ├── index.html             # Home page
│   ├── predict.html           # Weather prediction interface
│   ├── dashboard.html         # Analytics dashboard
│   └── about.html             # About page
│
├── static/                     # Static files
│   └── images/                # Visualization images
│       ├── weather_frequency.png
│       ├── correlation_heatmap.png
│       └── confusion_matrix.png
│
├── instance/                   # Instance-specific files
│   └── weather_forecast.db    # SQLite database
│
└── logs/                       # Application logs
    └── app.log
```

## 🔧 Development

### Running Tests

```bash
# Add your test commands here
python -m pytest tests/
```

### Database Management

#### View Database
```bash
sqlite3 instance/weather_forecast.db
```

#### Reset Database
```bash
rm instance/weather_forecast.db
python -c "from app import create_app, db; app = create_app(); app.app_context().push(); db.create_all()"
```

#### Check Database Tables
```python
from app import create_app, db
app = create_app()
with app.app_context():
    print(db.metadata.tables.keys())
```

## 🐛 Troubleshooting

### Common Issues

**1. API Key Errors**
```
Error: OpenWeatherMap API key not configured
Solution: Ensure OPENWEATHER_API_KEY is set in .env file
```

**2. Database Errors**
```
Error: No such table: weather_data
Solution: Initialize the database using Step 5 in Installation
```

**3. Import Errors**
```
Error: ModuleNotFoundError
Solution: Activate virtual environment and reinstall requirements
```

**4. Port Already in Use**
```
Error: Address already in use
Solution: Change PORT in .env or kill the process using port 5000
```

### Debug Mode

View application logs:
```bash
tail -f logs/app.log
```

Enable debug mode in `.env`:
```env
DEBUG=True
```

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/NewFeature`)
3. Commit your changes (`git commit -m 'Add NewFeature'`)
4. Push to the branch (`git push origin feature/NewFeature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- [OpenWeatherMap](https://openweathermap.org/) for weather data API
- Flask framework and its extensions
- Bootstrap for UI components

## 📧 Contact

**Developer**: Vijender  
**Email**: your.email@example.com

## 🗺️ Future Enhancements

- [ ] User authentication and saved locations
- [ ] Email/SMS alerts for severe weather
- [ ] Mobile responsive design improvements
- [ ] Additional weather data sources
- [ ] Historical weather data visualization
- [ ] Export predictions to CSV/PDF

---

**Last Updated: October 23, 2025**


## 🚀 Installation

### Prerequisites

- Python 3.8 or higher
- pip package manager
- Virtual environment (recommended)

### Step 1: Clone the Repository

```bash
git clone <repository-url>
cd "Vijender Weather Forecasting App"
```

### Step 2: Create Virtual Environment

```bash
# On macOS/Linux
python3 -m venv venv
source venv/bin/activate

# On Windows
python -m venv venv
venv\Scripts\activate
```

### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 4: Set Up Environment Variables

```bash
cp .env.example .env
```

Edit `.env` file with your API keys:

```env
OPENWEATHER_API_KEY=your_openweathermap_key
NOAA_API_KEY=your_noaa_key
ECMWF_API_KEY=your_ecmwf_key
OPENAI_API_KEY=your_openai_key
SECRET_KEY=your_secret_key
```

### Step 5: Initialize Database

```bash
python -c "from app import create_app, db; app = create_app(); app.app_context().push(); db.create_all()"
```

## ⚙️ Configuration

### API Keys

#### OpenWeatherMap API
1. Sign up at [OpenWeatherMap](https://openweathermap.org/api)
2. Get your free API key
3. Add to `.env` file

#### NOAA API
1. Register at [NOAA](https://www.ncdc.noaa.gov/cdo-web/token)
2. Request an API token
3. Add to `.env` file

### Model Configuration

Edit `config.py` to customize:

```python
# Model Parameters
LSTM_UNITS = 128
LSTM_LAYERS = 2
DROPOUT_RATE = 0.2
BATCH_SIZE = 32
EPOCHS = 100

# Weather Thresholds
HEATWAVE_THRESHOLD = 35  # Celsius
STORM_WIND_THRESHOLD = 63  # km/h
FLOOD_PRECIPITATION_THRESHOLD = 50  # mm per day
```

## 🎯 Usage

### Running the Application

#### Development Mode

```bash
python app.py
```

The application will be available at `http://localhost:5000`

#### Production Mode

```bash
export FLASK_ENV=production
gunicorn -w 4 -b 0.0.0.0:5000 app:app
```

### Using the Web Interface

1. **Home Page**: Overview of the system
2. **Predict Page**: Enter location coordinates for predictions
3. **Dashboard**: View analytics and model performance
4. **About Page**: Research information and methodology

### Quick Start Example

1. Navigate to `/predict`
2. Enter coordinates:
   - Latitude: 40.7128
   - Longitude: -74.0060
   - Location: New York City
3. Select forecast days (1-7)
4. Click "Generate Prediction"

## 📡 API Documentation

### Base URL
```
http://localhost:5000/api
```

### Endpoints

#### 1. Health Check
```http
GET /api/health
```

**Response:**
```json
{
  "status": "healthy",
  "timestamp": "2025-10-22T12:00:00Z"
}
```

#### 2. Generate Predictions
```http
POST /api/predict
```

**Request Body:**
```json
{
  "latitude": 40.7128,
  "longitude": -74.0060,
  "location_name": "New York City",
  "days": 7
}
```

**Response:**
```json
{
  "success": true,
  "location": {
    "latitude": 40.7128,
    "longitude": -74.0060,
    "name": "New York City"
  },
  "predictions": [
    {
      "date": "2025-10-23",
      "primary_event": "heatwave",
      "event_probability": 0.85,
      "confidence": 0.82,
      "events": {
        "heatwave": {
          "probability": 0.85,
          "risk_level": "high"
        },
        "storm": {
          "probability": 0.15,
          "risk_level": "low"
        },
        "flood": {
          "probability": 0.10,
          "risk_level": "minimal"
        }
      },
      "weather_forecast": {
        "temperature": 36.5,
        "humidity": 65,
        "wind_speed": 15.2,
        "precipitation": 0.0,
        "description": "clear sky"
      }
    }
  ]
}
```

#### 3. Get Current Weather
```http
GET /api/weather/current?latitude=40.7128&longitude=-74.0060
```

#### 4. Get Prediction History
```http
GET /api/predictions/history?limit=50
```

#### 5. Get Model Performance
```http
GET /api/models/performance?model_name=lightgbm
```

#### 6. Train Models
```http
POST /api/train
```

**Request Body:**
```json
{
  "model_type": "ensemble",
  "event_type": "heatwave"
}
```

## 🧠 Model Architecture

### 1. Bi-LSTM (Bidirectional LSTM)
- **Purpose**: Capture temporal dependencies in both directions
- **Architecture**: 2 layers, 128 units, 0.2 dropout
- **Use Case**: Sequential pattern recognition

### 2. LightGBM
- **Purpose**: Fast gradient boosting for classification
- **Features**: Efficient training, feature importance
- **Use Case**: Event classification

### 3. Extra Trees
- **Purpose**: Ensemble learning with randomized trees
- **Parameters**: 200 estimators, max depth 20
- **Use Case**: Robust classification

### 4. LSTM
- **Purpose**: Standard time series forecasting
- **Architecture**: 2 layers, 128 units
- **Use Case**: Temporal pattern learning

### 5. Temporal Fusion Transformer (TFT)
- **Purpose**: Multi-horizon forecasting with attention
- **Features**: Interpretable attention mechanism
- **Use Case**: Long-term predictions

### Ensemble Strategy

The system uses weighted ensemble predictions:
- LightGBM: 25%
- Extra Trees: 20%
- Bi-LSTM: 20%
- LSTM: 20%
- TFT: 15%

## 📁 Project Structure

```
Vijender Weather Forecasting App/
├── app.py                      # Main Flask application
├── config.py                   # Configuration settings
├── requirements.txt            # Python dependencies
├── .env.example               # Environment variables template
├── .gitignore                 # Git ignore rules
│
├── data/                      # Data processing modules
│   ├── __init__.py
│   ├── data_collector.py     # API data collection
│   └── preprocessor.py       # Data preprocessing
│
├── ml/                        # Machine learning models
│   ├── __init__.py
│   ├── models.py             # Model implementations
│   └── predictor.py          # Prediction orchestrator
│
├── models/                    # Saved model files
│   └── saved_models/         # Trained model checkpoints
│
├── routes.py                  # Flask routes
├── models.py                  # Database models
│
├── templates/                 # HTML templates
│   ├── base.html             # Base template
│   ├── index.html            # Home page
│   ├── predict.html          # Prediction interface
│   ├── dashboard.html        # Analytics dashboard
│   └── about.html            # About page
│
├── utils/                     # Utility functions
│   ├── __init__.py
│   ├── evaluation.py         # Metrics and evaluation
│   └── visualization.py      # Plotting functions
│
├── logs/                      # Application logs
└── data/                      # Data storage
    ├── raw/                  # Raw data files
    └── processed/            # Processed data
```

## 🔧 Advanced Usage

### Training Custom Models

```python
from ml.predictor import ModelTrainer
from data.data_collector import WeatherDataCollector
import pandas as pd

# Collect training data
collector = WeatherDataCollector()
# ... collect data

# Train models
trainer = ModelTrainer()
models = trainer.train_all_models(training_data)
```

### Batch Predictions

```python
from ml.predictor import WeatherPredictor

predictor = WeatherPredictor()

locations = [
    (40.7128, -74.0060, "New York"),
    (34.0522, -118.2437, "Los Angeles"),
    (51.5074, -0.1278, "London")
]

for lat, lon, name in locations:
    predictions = predictor.predict(lat, lon, 7, name)
    print(f"Predictions for {name}:")
    print(predictions)
```

### Custom Evaluation

```python
from utils.evaluation import EvaluationReport, evaluate_classification

# Create evaluation report
report = EvaluationReport("My Model")
report.add_event_evaluation("heatwave", y_true, y_pred)
report.add_event_evaluation("storm", y_true_storm, y_pred_storm)

# Generate and save report
print(report.generate_report())
report.save_report("evaluation_report.txt")
```

## 📊 Performance Metrics

### Current Model Performance

| Model | Accuracy | Precision | Recall | F1-Score |
|-------|----------|-----------|--------|----------|
| LightGBM | 0.87 | 0.85 | 0.89 | 0.87 |
| Extra Trees | 0.85 | 0.83 | 0.86 | 0.84 |
| Bi-LSTM | 0.82 | 0.80 | 0.84 | 0.82 |
| Ensemble | 0.89 | 0.87 | 0.91 | 0.89 |

*Note: These are example metrics. Actual performance depends on training data.*

## 🐛 Troubleshooting

### Common Issues

#### 1. API Key Errors
```
Error: Missing API key
Solution: Ensure all API keys are set in .env file
```

#### 2. Database Errors
```
Error: No such table
Solution: Run database initialization command
```

#### 3. Model Loading Errors
```
Error: Model file not found
Solution: Train models first or download pre-trained models
```

### Debug Mode

Enable debug logging:

```python
# In config.py
DEBUG = True
```

Check logs:
```bash
tail -f logs/app.log
```

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### Code Style

- Follow PEP 8 guidelines
- Add docstrings to all functions
- Write unit tests for new features
- Update documentation

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- OpenWeatherMap for weather data API
- NOAA for historical climate data
- ECMWF for atmospheric data
- World Meteorological Organization for extreme weather standards

## 📧 Contact

**Project Maintainer**: Vijender  
**Email**: your.email@example.com  
**Research Advisor**: [Advisor Name]

## 📚 References

1. G. Camps-Valls et al. (2025). "Deep learning methods for extreme weather predictions."
2. B. Jiménez-Esteve et al. (2024). "AI attribution methods for climate extremes."
3. L. Xu et al. (2024). "Pangu-Weather: Hybrid AI-numerical models for extreme precipitation."
4. World Meteorological Organization. "Standards for extreme weather event classification."

## 🗺️ Roadmap

### Version 1.0 (Current)
- ✅ Basic prediction system
- ✅ Multi-model ensemble
- ✅ Web interface
- ✅ API endpoints

### Version 2.0 (Planned)
- ⬜ Real-time alert system
- ⬜ Mobile application
- ⬜ Advanced visualization
- ⬜ Multi-region support
- ⬜ AI-augmented pattern analysis using GPT

### Version 3.0 (Future)
- ⬜ Long-term climate projections
- ⬜ Microclimate modeling
- ⬜ Integration with IoT sensors
- ⬜ Blockchain-based data verification

---

**Built with ❤️ for Climate Science Research**

*Last Updated: October 22, 2025*
