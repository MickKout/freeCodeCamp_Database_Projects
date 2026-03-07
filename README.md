# 🌌 Universe Database — PostgreSQL Project

A relational database project built with PostgreSQL that models the structure of the universe, including galaxies, stars, planets, moons, and comets.

---

## 📋 Project Overview

This project was built as part of a PostgreSQL learning exercise. It demonstrates the use of relational database concepts such as primary keys, foreign keys, constraints, data types, and multi-table joins.

---

## 🗄️ Database Structure

The database is named `universe` and contains **5 tables**:

| Table | Description |
|---|---|
| `galaxy` | Contains data about galaxies |
| `star` | Contains data about stars, linked to a galaxy |
| `planet` | Contains data about planets, linked to a star |
| `moon` | Contains data about moons, linked to a planet |
| `comet` | Contains data about comets |

### Relationships
- Each **star** belongs to a **galaxy** via `galaxy_id`
- Each **planet** belongs to a **star** via `star_id`
- Each **moon** belongs to a **planet** via `planet_id`

---

## 🧱 Table Details

### `galaxy`
| Column | Type | Constraints |
|---|---|---|
| `galaxy_id` | SERIAL | PRIMARY KEY |
| `name` | VARCHAR(100) | NOT NULL, UNIQUE |
| `age_in_millions_of_years` | INT | NOT NULL |
| `distance_from_earth` | NUMERIC | |
| `is_spherical` | BOOLEAN | |
| `description` | TEXT | |

### `star`
| Column | Type | Constraints |
|---|---|---|
| `star_id` | SERIAL | PRIMARY KEY |
| `name` | VARCHAR(100) | NOT NULL, UNIQUE |
| `age_in_millions_of_years` | INT | NOT NULL |
| `is_spherical` | BOOLEAN | |
| `distance_from_earth` | NUMERIC | |
| `galaxy_id` | INT | FOREIGN KEY → galaxy |

### `planet`
| Column | Type | Constraints |
|---|---|---|
| `planet_id` | SERIAL | PRIMARY KEY |
| `name` | VARCHAR(100) | NOT NULL, UNIQUE |
| `has_life` | BOOLEAN | NOT NULL |
| `is_spherical` | BOOLEAN | NOT NULL |
| `distance_from_star` | NUMERIC | |
| `age_in_millions_of_years` | INT | |
| `star_id` | INT | FOREIGN KEY → star |

### `moon`
| Column | Type | Constraints |
|---|---|---|
| `moon_id` | SERIAL | PRIMARY KEY |
| `name` | VARCHAR(100) | NOT NULL, UNIQUE |
| `is_spherical` | BOOLEAN | NOT NULL |
| `has_atmosphere` | BOOLEAN | NOT NULL |
| `distance_from_planet` | NUMERIC | |
| `age_in_millions_of_years` | INT | |
| `planet_id` | INT | FOREIGN KEY → planet |

### `comet`
| Column | Type | Constraints |
|---|---|---|
| `comet_id` | SERIAL | PRIMARY KEY |
| `name` | VARCHAR(100) | NOT NULL, UNIQUE |
| `age_in_millions_of_years` | INT | NOT NULL |
| `is_spherical` | BOOLEAN | NOT NULL |
| `distance_from_earth` | NUMERIC | |

---

## 🚀 Getting Started

### Prerequisites
- PostgreSQL installed on your machine
- Access to a terminal

### Setup

1. **Connect to PostgreSQL:**
```bash
psql --username=freecodecamp --dbname=postgres
```

2. **Create the database:**
```sql
CREATE DATABASE universe;
\c universe
```

3. **Run the SQL file** (if restoring from a saved file):
```bash
psql -U freecodecamp < universe.sql
```

---

## 💾 Saving the Database

To dump/export the database to a `.sql` file:
```bash
pg_dump -cC --inserts -U freecodecamp universe > universe.sql
```

---

## 🛠️ Key SQL Concepts Used

- `CREATE DATABASE` / `CREATE TABLE`
- `PRIMARY KEY` with `SERIAL` (auto-increment)
- `FOREIGN KEY` with `REFERENCES`
- Data types: `VARCHAR`, `INT`, `NUMERIC`, `TEXT`, `BOOLEAN`
- Constraints: `NOT NULL`, `UNIQUE`
- `INSERT INTO` with multiple rows
- `JOIN`, `FULL JOIN` across multiple tables
- `SELECT`, `WHERE`, `ORDER BY`
- `ALTER TABLE` to add/modify columns
- `pg_dump` to save the database

---

## 📁 File Structure

```
universe/
├── universe.sql       # Full database dump
└── README.md          # Project documentation
```

---

## 📚 Resources

- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [freeCodeCamp Relational Database Curriculum](https://www.freecodecamp.org/learn/relational-database/)
