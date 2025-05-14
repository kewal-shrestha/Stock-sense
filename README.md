

# StockSense Web Application

![Laravel](https://img.shields.io/badge/Laravel-v12.x-red.svg)  
![PHP](https://img.shields.io/badge/PHP-8.4.5+-blue.svg)  
![Docker](https://img.shields.io/badge/Docker-Supported-blue.svg)  
![License](https://img.shields.io/badge/License-MIT-green.svg)

A stocksense web application developed as an internship project by a team of 7 interns over 15-20 days. This Laravel-based backend powers a full-stack application designed to provide real-time stock data, portfolio management, simulated trading, and secure user authentication. The project emphasizes collaborative development, security, and practical full-stack experience, with Docker support for easy setup.

## Table of Contents
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
  - [Docker Installation (Linux)](#docker-installation)
- [Configuration](#configuration)
- [API Endpoints](#api-endpoints)
- [Database Schema](#database-schema)
- [Usage](#usage)
- [Testing](#testing)
- [Team Structure](#team-structure)
- [Contributing](#contributing)
- [License](#license)

## Features

### Core Features
- **User Authentication:**
  - Registration, login, logout, and password reset.
  - Secure JWT-based authentication with token expiration and refresh.
  - Optional 2FA (Time-Based One-Time Password).
- **Stock Data:**
  - Real-time stock quotes via WebSocket integration.
  - Historical stock data with charts.
  - Stock search functionality.
- **Portfolio Management:**
  - Add/remove stocks to/from portfolio.
  - View portfolio and track performance.
- **Simulated Trading:**
  - Buy/sell stocks with virtual money.
  - Transaction history tracking.
- **Watchlist:**
  - Add/remove stocks to/from watchlist.
  - View watchlist.
- **IPO Features:**
  - View IPO details.
  - Apply for IPOs.
- **Holdings:**
  - Track user holdings.

### Security Features
- JWT for authentication and authorization.
- Password hashing with bcrypt.
- Input validation and rate limiting.
- Optional 2FA implementation.

## Requirements

- Git for version control.
- PHP >= 8.4.5 (for standard installation without Docker).
- Composer >= 2.x (for standard installation without Docker).
- Laravel >= 12.x (for standard installation without Docker).
- Docker (for containerized setup, recommended):
  - Docker Engine and Docker Compose for running the application.
  - The Docker setup uses Laravel Sail for PHP 8.4 (sail-8.4/app image).
  - Required services:
    - PostgreSQL 12 (postgres:12 image) for the database.
    - pgAdmin (dpage/pgadmin4:latest image) for database management.
    - Redis (redis:alpine image) for caching and queue management.
    - Meilisearch (getmeili/meilisearch:latest image) for search functionality.
    - Mailpit (axllent/mailpit:latest image) for email testing.
  - Ports used (ensure they are free on your system):
    - Application: ${APP_PORT:-80} (default 80, mapped to 8080 in your setup).
    - Vite (if used): ${VITE_PORT:-5173} (default 5173).
    - PostgreSQL: ${FORWARD_DB_PORT:-5432} (default 5432).
    - pgAdmin: ${PGADMIN_PORT} (default 8081 as per your .env).
    - Redis: ${FORWARD_REDIS_PORT:-6379} (default 6379).
    - Meilisearch: ${FORWARD_MEILISEARCH_PORT:-7700} (default 7700).
    - Mailpit SMTP: ${FORWARD_MAILPIT_PORT:-1025} (default 1025).
    - Mailpit Dashboard: ${FORWARD_MAILPIT_DASHBOARD_PORT:-8025} (default 8025).
    - WebSockets (if configured): 6001.

## Installation

### Standard Installation
1. **Clone the repository:**
   ```bash
   git clone https://github.com/username/stock-market-web-app.git
   cd stock-market-web-app
   ```

2. **Install dependencies:**
   ```bash
   composer install
   ```

3. **Set up the environment file:**
   ```bash
   cp .env.example .env
   ```

4. **Generate an application key:**
   ```bash
   php artisan key:generate
   ```

5. **Configure the database:**
   - Update `.env` with your database credentials:
     ```
     DB_CONNECTION=pgsql
     DB_HOST=postgres
     DB_PORT=5432
     DB_DATABASE=stock-sense
     DB_USERNAME=myuser
     DB_PASSWORD=mypassword
     PGADMIN_PORT=8081
     PGADMIN_EMAIL=admin@example.com
     PGADMIN_PASSWORD=adminpassword
     ```
   - Run migrations:
     ```bash
     php artisan migrate
     ```

6. **(Optional) Seed the database:**
   ```bash
   php artisan db:seed
   ```

7. **Start the Laravel server:**
   ```bash
   php artisan serve
   ```
   The backend will be available at `http://localhost:8000`.

8. **Set up WebSockets (if applicable):**
   - Install Laravel WebSockets:
     ```bash
     composer require beyondcode/laravel-websockets
     php artisan websockets:serve
     ```

### Docker Installation (Linux)
1. **Install Docker and Docker Compose:**
   - Update package list:
     ```bash
     sudo apt update
     sudo apt install docker.io docker-compose -y
     ```
   - Start and enable Docker:
     ```bash
     sudo systemctl start docker
     sudo systemctl enable docker
     ```

2. **Clone the repository:**
   ```bash
   git clone https://github.com/username/stock-market-web-app.git
   cd stock-market-web-app
   ```

3. **Set up the environment file:**
   ```bash
   cp .env.example .env
   ```
   - Update `.env` with Docker-specific database settings:
     ```
     DB_CONNECTION=pgsql
     DB_HOST=postgres
     DB_PORT=5432
     DB_DATABASE=stock-sense
     DB_USERNAME=myuser
     DB_PASSWORD=mypassword
     PGADMIN_PORT=8081
     PGADMIN_EMAIL=admin@example.com
     PGADMIN_PASSWORD=adminpassword
     ```

4. **Build and run the containers:**
   ```bash
   docker-compose up -d --build
   ```
   - This starts the app, database, and WebSocket services (assuming a `docker-compose.yml` is included).

5. **Run migrations inside the container:**
   ```bash
   docker-compose exec app php artisan migrate
   ```

6. **Access the application:**
   - Backend: `http://localhost:8080`
   - WebSockets (if configured): `ws://localhost:6001`

7. **Stop the containers:**
   ```bash
   docker-compose down
   ```

### Docker Installation (Windows)
1. **Install Docker Desktop:**
   - Download and install [Docker Desktop](https://www.docker.com/products/docker-desktop) for Windows.
   - Launch Docker Desktop and ensure it’s running.

2. **Clone the repository:**
   - Open a terminal (e.g., PowerShell or Command Prompt):
     ```bash
     git clone https://github.com/username/stock-market-web-app.git
     cd stock-market-web-app
     ```

3. **Set up the environment file:**
   ```bash
   copy .env.example .env
   ```
   - Edit `.env` with Docker-specific database settings:
     ```
     DB_CONNECTION=pgsql
     DB_HOST=postgres
     DB_PORT=5432
     DB_DATABASE=stock-sense
     DB_USERNAME=myuser
     DB_PASSWORD=mypassword
     PGADMIN_PORT=8081
     PGADMIN_EMAIL=admin@example.com
     PGADMIN_PASSWORD=adminpassword
     ```

4. **Build and run the containers:**
   ```bash
   docker-compose up -d --build
   ```

5. **Run migrations inside the container:**
   ```bash
   docker-compose exec app php artisan migrate
   ```

6. **Access the application:**
   - Backend: `http://localhost:8080`
   - WebSockets (if configured): `ws://localhost:6001`

7. **Stop the containers:**
   ```bash
   docker-compose down
   ```

## Configuration
- Update `.env` with:
  - `APP_URL` for the application base URL.
  - `JWT_SECRET` for JWT authentication (generate via `php artisan jwt:secret` if using tymon/jwt-auth).
  - WebSocket settings (e.g., `PUSHER_APP_*` or Laravel WebSockets config).
  - Mail settings (`MAIL_*`) for password reset emails.
- Configure queues for real-time updates:
  ```bash
  php artisan queue:work
  ```

## API Endpoints

### Authentication API
| Method | Endpoint                              |     Description            | Parameters                                                                                      |
|--------|---------------------------------------|----------------------------|-------------------------------------------------------------------------------------------------|
| POST   | `/api/v1/auth/register`               | Register a new user        | `username`, `email`, `password`                                                                 |
| POST   | `/api/v1/auth/email/resend`           | Resend a new mail          |  `email`                                                                                        |
| POST   | `/api/v1/auth/login`                  | Log in a user              | `email`, `password`                                                                             |
| POST   | `/api/v1/auth/logout`                 | Log out a user             | `Authorization: Bearer <token>`                                                                 |
| POST   | `/api/v1/auth/change-password`        | change-password            | `Authorization: Bearer <token>` , `recent_password`, `new_password`, `new_password_confirmation`|
| POST   | `/api/v1/auth/forgot-password`        | Forget password            | `email`                                                                                         |
| PUT    | `/api/v1/auth/reset-password/{token}` | Reset password             | `password` `<token>`                                                                            |
| POST   | `/api/v1/auth/2fa/enable`             | Enable 2FA                 | `Authorization: Bearer <token>`                                                                 |
| POST   | `/api/v1/auth/verify-otp`             | Verify 2FA code            | `Private token` (TOTP code)                                                                     |
| POST   | `/api/v1/auth/2fa/disable`            | Disable 2FA                | `Authorization: Bearer <token>`                                                                 |     



### Stock Data API
| Method | Endpoint                        |    Description             |     Parameters                     |
|--------|---------------------------------|----------------------------|------------------------------------|
| GET    | `/api/v1/stocks/{id}`           | Get real-time stock quote  | `symbol`                           |
| GET    | `/api//v1stocks/{id}/history`   | Get historical data        | `from`, `to` (query)               |
| GET    | `/api/v1/stocks/`               | get all stocks             | `Authorization: Bearer<token>`     |
| POST   | `/api/v1/stocks/`               | Add new stock              | `symbol`,`company_name`,`sector_id`|
| PUT    | `/api/v1/stocks/{id}`           | Edit a stock               | `symbol`,`company_name`,`sector_id`|
| DELETE | `/api/v1/stocks/{id}`           | Delete a stock             | `Authorization: Bearer<token>`     |




### Portfolio API
| Method | Endpoint                     |    Description             |     Parameters                  |
|--------|------------------------------|----------------------------|---------------------------------|
| GET    | `/api/v1/users/portfolios`   | Get user’s portfolio       | `Authorization: Bearer <token>` |




### Transaction API
| Method | Endpoint                     |    Description             |     Parameters                                                |
|--------|------------------------------|----------------------------|---------------------------------------------------------------|
| GET    | `/api/v1/transactions`       | fetched all transactionst  | `Authorization: Bearer <token>`                               |
| POST   | `/api/v1/transactions`       | Add stock to watchlist     | `Authorization: Bearer <token>`,`stock_id`,`type`, `quantity` |  



### Holdings API
| Method | Endpoint                     |    Description             |     Parameters                  |
|--------|------------------------------|----------------------------|---------------------------------|
| GET    | `/api/v1/users/holdings`     | fetched user holdings      | `Authorization: Bearer <token>` |



### IPO Details API
| Method | Endpoint                     |    Description             |     Parameters                                                                                                         |
|--------|------------------------------|----------------------------|------------------------------------------------------------------------------------------------------------------------|
| GET    | `/api/v1/ipo-details`        | fetched all ipo details    | `Authorization: Bearer <token>`                                                                                        |
| POST   | `/api/watchlist/add`         | Add New IPO                | `Authorization: Bearer <token>`, ` stock_id`, `issue_price`, `total_shares`, `open_date`, `close_date`, `listing_date` |
| GET    |`/api/v1/ipo-details/{Id}`    | Fetch IPO by ID            | `Authorization: Bearer <token>`, `Id`                                                                                  |
| PUT    |`/api/v1/ipo-details/{Id}`    | Update IPO by ID           | `Authorization: Bearer <token>`, `Id`                                                                                  |
| DEL    |`/api/v1/ipo-details/{Id}`    | Delete IPO by ID           | `Authorization: Bearer <token>`, `Id`                                                                                  |




### IPO Application API
| Method | Endpoint                     |    Description             |     Parameters                                               | 
|--------|------------------------------|----------------------------|--------------------------------------------------------------|
| GET    | `/api/v1/ipo-applications`   | all user ipo applications  | `Authorization: Bearer <token>`                              |
| POST   | `/api/v1/ipo-applications`   | Add New IPO                | `Authorization: Bearer <token>`, ` ipo_id` , `applied_shares`|
| GET    |`/api/v1/ipo-details/{Id}`    | Fetch IPO by ID            | `Authorization: Bearer <token>`, `Id`                        |




### Watchlist API    
| Method | Endpoint                             |    Description               |     Parameters                             |
|--------|--------------------------------------|------------------------------|--------------------------------------------|
| GET    | `/api/v1/users/watchlist`            | fetched all user’s watchlist | `Authorization: Bearer <token>`            |
| POST   | `/api/v1/users/watchlist`            | Add stock to watchlist       | `Authorization: Bearer <token>``Stock_id`  |
| DEL    | `/api/v1/users/watchlist/{stock_id}` | Remove stock from watchlist  | `Authorization: Bearer <token>`, `stock_id`|

*Note:* All endpoints requiring authentication expect an `Authorization: Bearer <token>` header.


---

## Usage
- Access the API at `http://localhost:8080/api/`.
- Use tools like Postman or cURL to test endpoints. Example:
  ```bash
  curl -X POST http://localhost:8000/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email": "user@example.com", "password": "Password@123"}'
  ```

