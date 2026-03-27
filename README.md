# SMART TEMPERATURE MONITORING FOR CRITICAL SERVER ENVIRONMENT

A real-time environmental monitoring solution designed to track and record temperature and humidity data across multiple server rooms in a college environment.

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [System Architecture](#system-architecture)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Database Structure](#database-structure)
- [API Endpoints](#api-endpoints)
- [Contributing](#contributing)
- [License](#license)

## 📖 Overview

This project provides an intelligent monitoring system for critical server room environments. It continuously tracks temperature and humidity levels across three server rooms, stores the data in a MySQL database, and provides hourly aggregated analytics to optimize storage and improve performance monitoring.

The system ensures data integrity, reduces redundancy, and enables real-time alerts for temperature anomalies that could affect server performance.

## ✨ Features

- **Real-time Monitoring**: Continuous tracking of temperature and humidity sensors
- **Multi-room Support**: Monitor up to three server rooms simultaneously
- **Data Aggregation**: Automatic hourly aggregation to optimize storage
- **MySQL Database**: Robust data persistence with XAMPP integration
- **Web Dashboard**: User-friendly interface to visualize sensor data
- **Alert System**: Automated notifications for temperature threshold violations
- **Historical Analytics**: Access to aggregated data for trend analysis
- **Responsive Design**: Works seamlessly on desktop and mobile devices

## 🛠️ Tech Stack

| Technology | Purpose | Percentage |
|-----------|---------|-----------|
| JavaScript | Interactive functionality & client-side logic | 42.5% |
| PHP | Server-side processing & data handling | 39.4% |
| HTML | Page structure & markup | 16.1% |
| CSS | Styling & responsive design | 2% |

**Database**: MySQL (XAMPP)

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────┐
│         Temperature & Humidity Sensors              │
│    (Server Room 1, 2, 3)                           │
└────────────────────┬────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────┐
│        Data Collection Module (JavaScript)          │
│   - Read sensor inputs                             │
│   - Validate data                                  │
│   - Queue for transmission                         │
└────────────────────┬────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────┐
│      Backend API (PHP)                             │
│   - Process incoming data                          │
│   - Store raw sensor readings                      │
│   - Compute hourly aggregates                      │
│   - Handle alert triggers                          │
└────────────────────┬────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────┐
│      MySQL Database (XAMPP)                        │
│   - Raw sensor data                                │
│   - Hourly aggregated data                         │
│   - Alert history                                  │
│   - Configuration parameters                       │
└─────────────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────┐
│      Web Dashboard (HTML/CSS/JavaScript)           │
│   - Real-time monitoring display                   │
│   - Historical charts                              │
│   - Alert notifications                            │
│   - System configuration                           │
└─────────────────────────────────────────────────────┘
```

## 📦 Installation

### Prerequisites

- XAMPP (PHP 7.2+ and MySQL 5.7+)
- Web browser (Chrome, Firefox, Safari, Edge)
- Git

### Steps

1. **Clone the repository**
   ```bash
   git clone https://github.com/Bhushandk/SMART-TEMPERATURE-MONITORING-FOR-CRITICAL-SERVER-ENVIRONMENT.git
   cd SMART-TEMPERATURE-MONITORING-FOR-CRITICAL-SERVER-ENVIRONMENT
   ```

2. **Start XAMPP**
   - Open XAMPP Control Panel
   - Start Apache and MySQL services

3. **Copy project to htdocs**
   ```bash
   cp -r . /path/to/xampp/htdocs/temperature-monitoring
   ```

4. **Create database**
   - Open phpMyAdmin (http://localhost/phpmyadmin)
   - Create a new database named `server_monitoring`

5. **Import database schema**
   - Import the provided SQL schema file into the `server_monitoring` database

6. **Configure database connection**
   - Update database credentials in `config/db.php`:
   ```php
   $host = 'localhost';
   $user = 'root';
   $password = '';
   $database = 'server_monitoring';
   ```

7. **Access the application**
   - Open http://localhost/temperature-monitoring in your web browser

## ⚙️ Configuration

### Sensor Configuration

Configure sensor settings in the web dashboard:

- **Temperature Threshold**: Set alert triggers (default: 28°C for warning, 35°C for critical)
- **Humidity Threshold**: Set acceptable humidity range (default: 30-70%)
- **Polling Interval**: Adjust sensor read frequency (default: 5 minutes)
- **Aggregation Period**: Set hourly aggregation window (default: 1 hour)

### Email Alerts (Optional)

Configure email notifications for critical events:

```php
// In config/alerts.php
define('ALERT_EMAIL', 'admin@college.edu');
define('SMTP_HOST', 'your-smtp-server');
define('SMTP_PORT', 587);
```

## 🚀 Usage

### Viewing Real-time Data

1. Log in to the web dashboard
2. Select the server room from the dropdown
3. View live temperature and humidity readings
4. Monitor current status indicators

### Generating Reports

1. Navigate to the "Reports" section
2. Select date range and server room
3. Choose aggregation level (hourly, daily, weekly)
4. Generate and download reports as CSV

### Configuring Alerts

1. Go to "Settings" → "Alert Configuration"
2. Set temperature thresholds
3. Define alert recipients
4. Save preferences

## 🗄️ Database Structure

### Raw Sensor Data Table
```sql
CREATE TABLE sensor_readings (
  id INT PRIMARY KEY AUTO_INCREMENT,
  room_id INT NOT NULL,
  temperature DECIMAL(5,2),
  humidity DECIMAL(5,2),
  timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (room_id) REFERENCES rooms(id)
);
```

### Hourly Aggregates Table
```sql
CREATE TABLE hourly_aggregates (
  id INT PRIMARY KEY AUTO_INCREMENT,
  room_id INT NOT NULL,
  hour DATETIME,
  avg_temperature DECIMAL(5,2),
  max_temperature DECIMAL(5,2),
  min_temperature DECIMAL(5,2),
  avg_humidity DECIMAL(5,2),
  FOREIGN KEY (room_id) REFERENCES rooms(id)
);
```

### Rooms Table
```sql
CREATE TABLE rooms (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(100),
  location VARCHAR(255),
  status ENUM('active', 'inactive')
);
```

## 🔌 API Endpoints

### GET /api/readings.php
Retrieve current sensor readings
```
Parameters:
  - room_id: Server room ID (1-3)
  - limit: Number of recent readings (default: 10)

Response:
  {
    "success": true,
    "data": [
      {"timestamp": "2026-03-27 10:30:00", "temp": 24.5, "humidity": 52.3},
      ...
    ]
  }
```

### POST /api/readings.php
Submit new sensor reading
```
Parameters:
  - room_id: Server room ID
  - temperature: Temperature value
  - humidity: Humidity value

Response:
  {"success": true, "id": 12345}
```

### GET /api/aggregates.php
Get hourly aggregated data
```
Parameters:
  - room_id: Server room ID
  - start_date: YYYY-MM-DD format
  - end_date: YYYY-MM-DD format

Response:
  {
    "success": true,
    "data": [
      {"hour": "2026-03-27 10:00:00", "avg_temp": 24.2, "max_temp": 25.1, "min_temp": 23.8, "avg_humidity": 52.5}
      ...
    ]
  }
```

## 🤝 Contributing

Contributions are welcome! Please follow these guidelines:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/YourFeatureName`)
3. Commit your changes (`git commit -m 'Add YourFeatureName'`)
4. Push to the branch (`git push origin feature/YourFeatureName`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 📧 Contact & Support

For issues, questions, or suggestions:
- Open an issue on GitHub
- Contact: Bhushan DK
- Repository: https://github.com/Bhushandk/SMART-TEMPERATURE-MONITORING-FOR-CRITICAL-SERVER-ENVIRONMENT

---

**Last Updated**: March 27, 2026