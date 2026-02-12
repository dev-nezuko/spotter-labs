# Fuel Optimized Route Planner (Django + OpenRouteService)

This project is a Django REST API that calculates an optimized
driving route between two locations and determines cost efficient fuel
stops along the route based on:

-   Vehicle fuel efficiency (MPG)
-   Tank range
-   Fuel station prices (loaded from CSV)
-   Real driving routes from OpenRouteService (ORS)

------------------------------------------------------------------------

## Features

-   Uses OpenRouteService Directions API for accurate driving routes
-   Calculates total distance using ORS data
-   Finds cheapest fuel stations near the route
-   Proactive refueling using a safety buffer
-   CSV-based fuel station ingestion
-   Secure configuration using `.env`

------------------------------------------------------------------------

## Tech Stack

-   Python
-   Django
-   Django REST Framework
-   PostgreSQL
-   OpenRouteService API
-   python-dotenv
-   requests
-   polyline

------------------------------------------------------------------------


# Setup Instructions

## Create Virtual Environment

### macOS / Linux

python -m venv venv 
source venv/bin/activate

### Windows

python -m venv venv 
venv\Scripts\activate

------------------------------------------------------------------------

## Install Dependencies

pip install -r requirements.txt

------------------------------------------------------------------------

# Environment Configuration

Create a `.env` file in the project root:

DB_NAME=fuel_routes DB_USER=postgres DB_PASSWORD=your_password
DB_HOST=localhost DB_PORT=5432

ORS_API_KEY=your_openrouteservice_key


------------------------------------------------------------------------

# Database Setup (PostgreSQL)

## Create Database

CREATE DATABASE fuel_routes;

## Apply Migrations

python manage.py makemigrations python manage.py migrate

------------------------------------------------------------------------

# Get OpenRouteService API Key

1.  Go to https://openrouteservice.org
2.  Create an account
3.  Generate a Directions API key
4.  Add it to `.env`

------------------------------------------------------------------------

# Fuel Stations CSV

Place your CSV file at root level same as the manage.py:

Example CSV format:

station_name,city,state,zip_code,latitude,longitude,fuel_type,price_per_gallon
Shell,Los Angeles,CA,90001,34.0522,-118.2437,Regular,4.59


------------------------------------------------------------------------

# Load Fuel Stations into Database

python manage.py load_fuel_data

------------------------------------------------------------------------

# Run Development Server

python manage.py runserver

Server will run at: http://127.0.0.1:8000/

------------------------------------------------------------------------

# API Endpoint

POST /api/route/

Request Body:

{ "start_lat": 34.05233, "start_lng": -118.24356, "end_lat": 40.71278,
"end_lng": -74.0060}

Response Example:

{ "route": \[\[lat, lng\], ...\], "fuel_stops": \[ { "station_name": "Exxon", "latitude": 39.7392, "longitude": -104.9903, "price_per_gallon": 4.58 } \], "total_distance_miles": 2794.41, "total_fuel_cost": 1075.25 }




