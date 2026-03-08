# 🗄️ PostgreSQL Database Projects

A collection of three relational database projects built with PostgreSQL, created as part of the freeCodeCamp Relational Database curriculum. Each project demonstrates core database concepts including table design, relationships, constraints, and querying.

---

## 📦 Projects

| # | Project | Database Name | Description |
|---|---|---|---|
| 1 | 🌌 Celestial Bodies | `universe` | Models the structure of the universe |
| 2 | ⚽ World Cup | `worldcup` | Tracks World Cup matches and teams |
| 3 | 🧪 Periodic Table | `periodic_table` | Stores chemical elements and properties |

---

---

# 🌌 Project 1 — Celestial Bodies Database

A database that models the structure of the universe, including galaxies, stars, planets, moons, and comets.

## Database Structure

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

## Table Details

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

## Setup

```bash
psql --username=freecodecamp --dbname=postgres
```
```sql
CREATE DATABASE universe;
\c universe
```

## Save / Restore

```bash
# Save
pg_dump -cC --inserts -U freecodecamp universe > universe.sql

# Restore
psql -U freecodecamp < universe.sql
```

---

---

# ⚽ Project 2 — World Cup Database

A database that tracks FIFA World Cup matches, teams, and results.

## Database Structure

The database is named `worldcup` and contains **2 tables**:

| Table | Description |
|---|---|
| `teams` | Stores each team that participated |
| `games` | Stores each match played, linked to two teams |

### Relationships
- Each **game** has a `winner_id` and `opponent_id` that both reference a row in `teams`

## Table Details

### `teams`
| Column | Type | Constraints |
|---|---|---|
| `team_id` | SERIAL | PRIMARY KEY |
| `name` | VARCHAR(100) | NOT NULL, UNIQUE |

### `games`
| Column | Type | Constraints |
|---|---|---|
| `game_id` | SERIAL | PRIMARY KEY |
| `year` | INT | NOT NULL |
| `round` | VARCHAR(50) | NOT NULL |
| `winner_id` | INT | FOREIGN KEY → teams |
| `opponent_id` | INT | FOREIGN KEY → teams |
| `winner_goals` | INT | NOT NULL |
| `opponent_goals` | INT | NOT NULL |

## Inserting Data from a CSV

The project uses a shell script (`insert_data.sh`) to read match data from a `games.csv` file and insert it into the database:

```bash
#!/bin/bash

cat games.csv | while IFS="," read YEAR ROUND WINNER OPPONENT WINNER_GOALS OPPONENT_GOALS
do
  # Insert team if it doesn't exist
  WINNER_ID=$(psql --username=freecodecamp --dbname=worldcup -t --no-align -c \
    "SELECT team_id FROM teams WHERE name='$WINNER'")
  if [[ -z $WINNER_ID ]]; then
    psql --username=freecodecamp --dbname=worldcup -c \
      "INSERT INTO teams(name) VALUES('$WINNER')"
    WINNER_ID=$(psql --username=freecodecamp --dbname=worldcup -t --no-align -c \
      "SELECT team_id FROM teams WHERE name='$WINNER'")
  fi

  # Insert game
  psql --username=freecodecamp --dbname=worldcup -c \
    "INSERT INTO games(year, round, winner_id, opponent_id, winner_goals, opponent_goals)
     VALUES($YEAR, '$ROUND', $WINNER_ID, $OPPONENT_ID, $WINNER_GOALS, $OPPONENT_GOALS)"
done
```

## Setup

```bash
psql --username=freecodecamp --dbname=postgres
```
```sql
CREATE DATABASE worldcup;
\c worldcup
```

## Save / Restore

```bash
# Save
pg_dump -cC --inserts -U freecodecamp worldcup > worldcup.sql

# Restore
psql -U freecodecamp < worldcup.sql
```

---

---

# 🧪 Project 3 — Periodic Table Database

A database that stores chemical elements and their properties.

## Database Structure

The database is named `periodic_table` and contains **3 tables**:

| Table | Description |
|---|---|
| `elements` | Stores basic element info like symbol and name |
| `properties` | Stores physical/chemical properties of each element |
| `types` | Stores element type categories (metal, nonmetal, etc.) |

### Relationships
- Each row in **properties** is linked to `elements` via `atomic_number`
- Each row in **properties** has a `type_id` that references `types`

## Table Details

### `elements`
| Column | Type | Constraints |
|---|---|---|
| `atomic_number` | INT | PRIMARY KEY |
| `symbol` | VARCHAR(2) | NOT NULL, UNIQUE |
| `name` | VARCHAR(40) | NOT NULL, UNIQUE |

### `properties`
| Column | Type | Constraints |
|---|---|---|
| `atomic_number` | INT | FOREIGN KEY → elements |
| `atomic_mass` | NUMERIC | NOT NULL |
| `melting_point_celsius` | NUMERIC | NOT NULL |
| `boiling_point_celsius` | NUMERIC | NOT NULL |
| `type_id` | INT | FOREIGN KEY → types |

### `types`
| Column | Type | Constraints |
|---|---|---|
| `type_id` | SERIAL | PRIMARY KEY |
| `type` | VARCHAR(50) | NOT NULL |

## Element Query Script

The project includes an `element.sh` bash script that accepts an atomic number, symbol, or name and returns info about that element:

```bash
#!/bin/bash

if [[ -z $1 ]]; then
  echo "Please provide an element as an argument."
  exit
fi

RESULT=$(psql --username=freecodecamp --dbname=periodic_table --no-align -t -c \
  "SELECT e.atomic_number, e.name, e.symbol, t.type, p.atomic_mass,
          p.melting_point_celsius, p.boiling_point_celsius
   FROM elements e
   JOIN properties p USING(atomic_number)
   JOIN types t USING(type_id)
   WHERE e.atomic_number='$1' OR e.symbol='$1' OR e.name='$1'")

if [[ -z $RESULT ]]; then
  echo "I could not find that element in the database."
else
  echo "$RESULT" | while IFS="|" read NUM NAME SYMBOL TYPE MASS MELT BOIL
  do
    echo "The element with atomic number $NUM is $NAME ($SYMBOL). It's a $TYPE, with a mass of $MASS amu. $NAME has a melting point of $MELT celsius and a boiling point of $BOIL celsius."
  done
fi
```

## Setup

```bash
psql --username=freecodecamp --dbname=postgres
```
```sql
CREATE DATABASE periodic_table;
\c periodic_table
```

## Save / Restore

```bash
# Save
pg_dump -cC --inserts -U freecodecamp periodic_table > periodic_table.sql

# Restore
psql -U freecodecamp < periodic_table.sql
```

---

---

## 🛠️ Key SQL Concepts Used Across All Projects

- `CREATE DATABASE` / `CREATE TABLE`
- `PRIMARY KEY` with `SERIAL` (auto-increment)
- `FOREIGN KEY` with `REFERENCES`
- Data types: `VARCHAR`, `INT`, `NUMERIC`, `TEXT`, `BOOLEAN`
- Constraints: `NOT NULL`, `UNIQUE`
- `INSERT INTO` with single and multiple rows
- `JOIN`, `FULL JOIN` across multiple tables
- `SELECT`, `WHERE`, `ORDER BY`
- `ALTER TABLE` to add/modify/rename columns
- `pg_dump` to save and restore databases
- Bash scripting to automate data insertion and querying

---

## 📁 File Structure

```
postgresql-projects/
├── universe/
│   └── universe.sql          # Full database dump
├── worldcup/
│   ├── worldcup.sql          # Full database dump
│   ├── games.csv             # Raw match data
│   └── insert_data.sh        # Script to insert CSV data
├── periodic_table/
│   ├── periodic_table.sql    # Full database dump
│   └── element.sh            # Script to query elements
└── README.md                 # Project documentation
```

---

## 📚 Resources

- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
- [freeCodeCamp Relational Database Curriculum](https://www.freecodecamp.org/learn/relational-database/)
