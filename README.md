# CinemaMicroservice

---

# 🎬 Cinema Microservice System

A **Cinema Management Microservice Application** with an integrated **ticketing system**, developed with scalability, modularity, and modern best practices in mind.

## 📌 Overview

This application allows users to:

- Register and verify accounts via email
- Browse film screenings by date
- View seat availability
- Book tickets with support for discounts and multi-currency pricing
- Receive a PDF ticket with a QR code via email

Admins can:

- Add new films
- Create new screenings by selecting date, time, and associated films

---

## ⚙️ Architecture & Technologies

| Component | Technology |
|----------|------------|
| **Core Framework** | Java, Spring Boot |
| **Inter-service Communication** | Feign Client over HTTP |
| **Configuration Management** | Spring Cloud Config Server |
| **Service Discovery** | Eureka Server |
| **Gateway** | API Gateway (Spring Cloud Gateway) |
| **Asynchronous Messaging** | Apache Kafka |
| **Database** | PostgreSQL (Dockerized) |
| **Monitoring** | Zipkin (for distributed tracing) |
| **Containerization** | Docker |
| **Testing** | JUnit |

---

## 🛠️ Features

### 🔐 User Registration & Verification
- Users register and receive an activation link via email.
- After activation, users can proceed to book tickets.

### 🎟️ Ticket Booking
- Select a screening date and film
- Check seat availability
- Choose seat (row and number)
- Select ticket type:
  - `NORMAL`
  - `REDUCE` (for students)
- Apply discounts:
  - **Event Discounts** (20% off on Tuesdays & Fridays)
  - **Student Discount** (10% off)
- Select ticket currency from 34 supported options (via NBP API)
- Receive ticket as PDF with QR code

### 💰 Currency Exchange
- Integrated with **National Bank of Poland API**
- Fetches exchange rates for 34 currencies every 24 hours

### 📧 Email Service
- Ticket microservice sends Kafka message upon successful booking
- EmailSender service consumes the message and emails the ticket (PDF + QR code)

### 🎥 Admin Functionality
- Add new films
- Create screenings with date and time

---

## 🧪 How to Run

### ✅ Prerequisites

- Install [IntelliJ IDEA](https://www.jetbrains.com/idea/)
- Install [Docker Desktop](https://www.docker.com/products/docker-desktop)

### ▶️ Running the Application

1. Clone the repository:
   ```bash
   git clone https://github.com/Gimi818/CinemaMicroService
   ```

2. Start Docker Desktop

3. Run Docker containers:
   ```bash
   docker-compose up
   ```

4. In IntelliJ, run the following services in order:
   - ConfigServer
   - DiscoveryServer (Eureka)
   - API Gateway
   - Microservices: `user`, `ticket`, `film`, `screening`, `currencies`

---

## 📬 API Usage with Postman

### 1. Register User
```http
POST localhost:8222/api/v1/users/registration
```

**JSON Body:**
```json
{
  "firstName":"Wojciech",
  "lastName":"Gmiterek",
  "email":"cinemaemailtest@gmail.com",
  "password":"password",
  "repeatedPassword":"password"
}
```

### 2. Activate Account
Click the email activation link sent to your inbox.

---

### 3. Browse Screenings
```http
GET localhost:8222/api/v1/screenings?date=2024-04-23
```
View screenings available between `2024-10-11` and `2024-10-31`.

---

### 4. Check Seat Availability
```http
GET localhost:8222/api/v1/screenings/1
```
Replace `1` with the desired screening ID.

---

### 5. View Available Currencies
```http
GET localhost:8222/api/v1/codes
```

---

### 6. Book Ticket
```http
POST localhost:8222/api/v1/book/1/1
```

**URL Parameters:**
- First `1`: User ID
- Second `1`: Film ID

**JSON Body:**
```json
{
  "ticketType":"REDUCE",
  "currency":"USD",
  "rowsNumber":5,
  "seatInRow": 8
}
```

---

### 7. Receive PDF Ticket
Check your email inbox for a PDF ticket with a QR code confirming the reservation.

---

## 🎬 Admin APIs

### Add New Film
```http
POST localhost:8222/api/v1/films
```

**JSON Body:**
```json
{
  "title": "BAD BOYS 3",
  "category": "ACTION",
  "durationFilmInMinutes": 130
}
```

---

### Create Screening
```http
POST localhost:8222/api/v1/screenings/10
```
Replace `10` with the Film ID.

**JSON Body:**
```json
{
  "date": "2024-02-01",
  "time": "22:30"
}
```

---

