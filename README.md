# Railway System Database Schema

This project contains a **PostgreSQL schema** for managing a **Railway Booking & Management System**.

## 📂 Schema Overview
The schema `railway_system` is designed to store:
- User login data
- Passenger profiles
- Train types and details
- Station information
- Train routes
- Journey bookings

---

## 🗂 Tables

### 1. `user_login`
Stores login credentials and basic user info.
| Column        | Type   | Description |
|--------------|--------|-------------|
| user_id (PK) | TEXT   | Unique user identifier |
| user_password| TEXT   | Login password (should be hashed) |
| first_name   | TEXT   | First name |
| last_name    | TEXT   | Last name |
| sign_up_on   | DATE   | Registration date |
| email_id     | TEXT   | Email address |

---

### 2. `passenger`
Stores passenger profile details.
| Column        | Type   | Description |
|--------------|--------|-------------|
| passenger_id (PK) | TEXT | Unique passenger ID |
| user_password| TEXT   | Login password (should be hashed) |
| first_name   | TEXT   | First name |
| last_name    | TEXT   | Last name |
| sign_up_on   | DATE   | Registration date |
| email_id     | TEXT   | Email address |
| contact      | TEXT   | Contact number |

---

### 3. `train_type`
Stores categories of trains.
| Column        | Type   | Description |
|--------------|--------|-------------|
| train_type_id (PK) | TEXT | Unique type ID |
| train_type   | TEXT   | Train category (Express, Superfast, Local, etc.) |
| coaches_count| TEXT   | Number of coaches |
| passenger_strength| TEXT | Capacity |
| train_count  | DATE   | (Should ideally be INT for number of trains) |

---

### 4. `stations`
Stores station details.
| Column        | Type   | Description |
|--------------|--------|-------------|
| station_id (PK) | TEXT | Station code |
| station_name| TEXT   | Station name |
| city        | TEXT   | City |
| state       | TEXT   | State |

---

### 5. `train_details`
Stores details of individual trains.
| Column        | Type   | Description |
|--------------|--------|-------------|
| train_id (PK) | TEXT | Unique train ID |
| train_type_id (FK) | TEXT | Linked to `train_type` |
| source_station_id (FK) | TEXT | Linked to `stations` |
| destination_station_id (FK) | TEXT | Linked to `stations` |
| duration_minutes| INT  | Total journey time |
| journey_start | TIMESTAMP | Departure time |
| journey_end   | TIMESTAMP | Arrival time |
| passenger_strength | INT | Capacity |
| is_available  | BOOLEAN | Train operational status |

---

### 6. `journey`
Stores booking information.
| Column        | Type   | Description |
|--------------|--------|-------------|
| journey_id (PK) | TEXT | Journey ID |
| passenger_id (FK) | TEXT | Linked to `passenger` |
| train_id (FK) | TEXT | Linked to `train_details` |
| booking_id   | TEXT   | Booking reference |
| payment_id   | TEXT   | Payment reference |
| payment_status| TEXT  | Status of payment |
| paid_on      | TIMESTAMP | Payment date/time |
| booking_status| TEXT | Status of booking |
| booked_on    | TIMESTAMP | Booking date/time |
| seat_alloted | TEXT  | Allocated seat number |
| meal_booked  | BOOLEAN | Whether meal is booked |

---

### 7. `train_routes`
Stores the routes for trains.
| Column        | Type   | Description |
|--------------|--------|-------------|
| row_id (PK)  | SERIAL | Auto-increment ID |
| route_id     | TEXT   | Route identifier |
| train_id (FK)| TEXT   | Linked to `train_details` |
| station_id (FK) | TEXT | Linked to `stations` |
| order_number | INT    | Stop order |
| halt_duration_minutes | INT | Time stopped at station |
| estimated_arrival | TIME | Arrival time |
| estimated_departure| TIME | Departure time |

---

## 🔗 Relationships
- **One train type → Many trains**
- **One station → Can be part of many routes**
- **One train → Many stops in `train_routes`**
- **One passenger → Many journeys**

---

## 💡 Notes
- Passwords should be stored in hashed format.
- The `train_count` column in `train_type` is currently `DATE`, which should ideally be `INT` for counting trains.
- The schema enforces **referential integrity** using `FOREIGN KEY` constraints.
