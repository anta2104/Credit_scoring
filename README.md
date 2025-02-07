# Trip Risk Assessment API

## Overview
This API provides trip risk assessment based on trip data retrieved from an external endpoint. It analyzes missing data, calculates safety scores, and detects harsh driving events to determine the overall risk level of a trip.

## Requirements
- Python 3.10
- Flask
- Requests
- NumPy
- pytz

## Installation
1. Install dependencies:
   ```sh
   pip install -r requirements.txt
   ```
2. Run the API:
   ```sh
   python api.py
   ```

## API Endpoints
### 1. Trip Risk Assessment
**Endpoint:**
```
POST /api/trip-risk
```

**Request Body:**
```json
{
  "endpoint": "<external_trip_data_url>",
  "trip_id": "trip_id"
}
```

**Response:**
```json
{
  "trip_id": "<trip_id>",
  "flag": {
    "data_integrity_score": <score>,
    "warning": "<warning_level>"
  },
  "harsh_events": {
    "harsh_braking": <count>,
    "harsh_acceleration": <count>
  },
  "risk_score": {
    "score": <numeric_score>,
    "level": "<risk_level>"
  }
}
```

**Risk Levels:**
- 90-100: Low Risk
- 70-89: Moderate Risk
- 50-69: Elevated Risk
- 30-49: High Risk
- 0-29: Extreme Risk

**Warning Levels:**
- **EXCLUDE**: Trip data is insufficient or missing completely, making risk assessment impossible.
- **PARTIALLY UNRELIABLE**: More than 10% of the trip data is missing, reducing confidence in the assessment.
- **CAUTION**: Either consecutive missing data points exceed 5 or between 5% and 10% of data is missing, requiring careful interpretation.
- **RELIABLE**: Minimal or no missing data, ensuring a high-confidence risk assessment.

## Functional Breakdown

### 1. `detect_missing_data(trip_data)`
- Checks for missing trip points and calculates a data integrity score.
- Returns a score and warning level based on data gaps.

### 2. `calculate_harsh_events(trip_points)`
- Detects harsh braking and harsh acceleration events based on acceleration thresholds.
- Harsh Braking: Acceleration < -0.8 m/s²
- Harsh Acceleration: Acceleration > 0.7 m/s²

### 3. `calculate_safety_score(trip_data)`
- Computes a safety score based on speeding violations.
- Factors in speed limit violations and distance traveled while speeding.

## Deployment
To run the API using docker:
```sh
    sudo bash build.sh
```




