# 🎟️ Secure Your Seat – Movie Ticket Booking System (DBMS Project)

A fully normalized relational database system for a movie ticket booking platform, built using **PostgreSQL**. This project models real-world entities such as customers, cinemas, movies, payments, and shows — and supports advanced querying for analytics and reporting.

---

## 📌 Features

- ✅ Schema designed in **Boyce-Codd Normal Form (BCNF)** with formal proofs
- ✅ Models complex real-world entities: movies, customers, cinemas, shows, seats, and payments
- ✅ Supports multilingual movies and multiple genres per movie
- ✅ Seat-level booking and ticket tracking
- ✅ Integrated payment and ticketing system with referential integrity
- ✅ Analytical SQL queries for operational insights:
  - City-wise revenue
  - Top customers by spending
  - Seat occupancy rates
  - Popular genres and booking patterns

---

## 🏗️ Database Design

- 12+ tables covering all core entities and relations
- Normalized to **BCNF** to eliminate redundancy and anomalies
- Foreign keys with **ON DELETE CASCADE** and **ON UPDATE CASCADE**
- Composite primary keys in many-to-many relations (e.g., genres, languages)

📄 See [`docs/Normalization_Proofs.pdf`](docs/Normalization_Proofs.pdf) for detailed normalization proofs and functional dependencies.

---

## 🧠 Sample Queries

- **Top 3 revenue-generating cities:**
```sql
SELECT c.name AS city_name, SUM(p.amount) AS total_revenue
FROM city c
JOIN cinema ci ON c.cityid = ci.cityid
JOIN cinemaHall h ON ci.cinemaid = h.cinemaid
JOIN shows s ON h.hallid = s.hallid
JOIN showseat ss ON s.showid = ss.showid
JOIN eticket e ON ss.ticketno = e.ticketno
JOIN payment p ON e.paymentid = p.paymentid
GROUP BY c.name
ORDER BY total_revenue DESC
LIMIT 3;
````

* **Top 5 customers by total spend:**

```sql
SELECT c.uid, c.fname, c.lname, SUM(p.amount) AS total_amount_spent
FROM customer c
NATURAL JOIN eticket e
NATURAL JOIN payment p
GROUP BY c.uid, c.fname, c.lname
ORDER BY total_amount_spent DESC
LIMIT 5;
```

* **Find customers who booked shows with runtime > 100 mins:**

```sql
SELECT DISTINCT c.*
FROM customer c
JOIN payment p ON c.uid = p.uid
JOIN eticket e ON p.paymentid = e.paymentid
JOIN showseat ss ON e.ticketno = ss.ticketno
JOIN shows s ON ss.showid = s.showid
JOIN movie m ON s.cbfcno = m.cbfcno
WHERE m.runtime > 100;
```

And many more in [`queries/SQL_Queries.sql`](queries/SQL_Queries.sql)

---

## 🛠️ Tech Stack

* **Database:** PostgreSQL 15+
* **Query Language:** SQL
* **Design Tools:** ERD + Functional Dependency Normalization

---

## 🧪 Sample Entities

* 🎬 **Movies** with multiple genres and languages
* 🧑 **Customers** with personal and contact details
* 🏢 **Cinemas** mapped to cities and halls
* ⏱️ **Shows** tied to movies, halls, and schedules
* 💳 **Payments** linked to tickets and users
* 💺 **Seats** with booking status and pricing

---

## 📈 Analytical Capabilities

* Customer segmentation and behavior
* Revenue and booking insights by city or cinema
* Seat utilization tracking per hall and per show
* Top genres, shows, and high-spending customers

## 📄 License

This project is open-source and intended for academic and learning purposes. Feel free to fork or use with attribution.

---

## 🚀 Future Improvements

* Integrate with backend (e.g., Flask, Node.js) to build APIs for booking and user login
* Develop a UI for ticket search, booking, and payments
* Add triggers and stored procedures for business rules (e.g., auto-cancel unpaid tickets)
