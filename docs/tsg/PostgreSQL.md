## PostgreSQL

## Purpose

This document introduces PostgreSQL and explains why it was selected as the primary database for Your Fantasy.

By the end of this guide you should understand:

1. What PostgreSQL is
2. What problems it solves
3. Why PostgreSQL was selected
4. What relational databases are
5. The core concepts used throughout relational database design
6. How PostgreSQL fits into Your Fantasy's backend architecture

---

## Terminology Help:

Database: A database is a system used to organize, store, retrieve, and manage data. Unlike a spreadsheet or a text file, a database is designed to efficiently store miillions (or even billions) of records while allowing applications to quickly search, update, and relate that information.

# 1. What is PostgreSQL?

PostgreSQL is an open-source Relational Database Management System (RDBMS)

    - Relational:
        - Data is stored in tables
        - Tables can be connected (related) to one another.
    - Database:
        - Stores data permanently.
    - Management System:
        - Provides tools for creating, querying, updating, securing, and organizing the data.

Real World Example: 

Log in to Meta/Facebook > Username and Password are stored into a database and not in Meta's code > The database stores: User credentials, Birthdate, Friends > The application only asks the database for the information.

# 2. What problem does PostgreSQL solve?

PostrgeSQL solves one of the biggest problems in software-- Persistence.

    - Persistence means:
        - Data survives after the application closes.
        - Allows Your Fantasy to save player data and world changes such as items, monsters, guild changes, etc


# 3. Use Case

For Your Fantasy, PostgreSQL will eventually store:
- Accounts
- Characters
- Guilds
- Friends
- Housing
- Inventory
- Market Listings
- Recipes
- Crafting Materials
- Quest Progress
- Achievements
- Player Economy
- Etc..

# 4. Why PostgreSQL over MySQL?

PostgreSQL and MySQL are both capable relational databases.

PostgreSQL was selected because it offers stronger support for complex relational data, advanced SQL features, and long-term scalability.

Since Your Fantasy will eventually contain interconnected systems such as characters, guilds, housing, crafting, economies, and player relationships, PostgreSQL provides a strong foundation for future expansion.

# 5. What are relational databases?

A Relational Database stores structured data in organized table of rows and columns. Tables are linked (or related) using unique identifierscalled keys, which allow applications to query and connect data across multiple tables wihtout duplicating information.

## Important Concepts

        Tables: A Table is a collection of related information. 
            - Each table should have one responsibility
            - Accounts
            - Characters
            - Inventory
            - ETc

            Ex: Table: Account
                ID | Username | Email

                Table: Character
                ID | Name | Level | Gold

            The Account Table is responsible for the Account | The Character Table is responsible for the Character

        Rows: A Row represents one record inside a table.

            Ex: Characters

                ID | Name | Level
                -----------------
                1  | Shinxee | 15
                2  | Luna    | 42
            
            Each horizontal line is a row. Each row represents one character.

        Columns: Columns describe the properties of a record

            Ex:     Characters

                ID | Name | Level | Gold
            
            Columns are ID, Name, Level, Gold

            Columns would describe the player

        Primary Keys: A Primary Key uniquely identifies every row in a table. No two rows can have the same Primary Key.

            Ex:

                    Characters

                CharacterID | Name
                ------------------
                1           | Shinxee
                2           | Luna
                3           | Bob

            CharacterID is the Primary Key. Shinxee is widely known as CharacterID number 1. This Primary Key never changes even if there are two Shinxee's. Even if the player name changes, the characterID doesn't.

        Foreign Keys: A Foreign Key creates a relationship between two tables. Instead of storing duplicate information, we story the Primary Key from anotehr table.

            Ex:
                Characters

                    CharacterID | Name
                    ---------------------
                    1           | Shinxee

                Inventory

                    CharacterID | ItemID | Quantity
                    -------------------------------
                    1           | 52     | 5
                    1           | 83     | 1
            
            Notice the inventory doesn't store Shinxee, it stores 1 instead. The database knows CharacterID 1 belongs to Shinxee

        Relationships: A relationship is a logical connection establish between different tables. It associates data across tables linking records that belong together which eliminate data duplication.

            Ex: Relationships describe how tables connect to one another.

                Example:

                One Account
                ↓

                Many Characters

                One Character
                ↓

                Many Inventory Items

                One Item
                ↓

                Many Characters may own it

        Indexes: An index is a data structure that improves data retrieval speeds by avoiding full scale scans.

            Ex: Instead of checking the entire phonebook from the beginning to get a certain number you jump straight to it.

                SELECT *
                FROM Characters
                WHERE Name = 'Shinxee';
            
            Indexes exist to improve search performance

        Transactions: A Transaction groups mulitple operations into one safe operation. Transactions provide an all-or-nothing guarantee, meaning every modification in the group succeeds and is permanently saved, or the entire group is canceled and the database rolls back to it's original state/

            EX: Player 1 gives Player 2 Sword and Potion, if server crashes mid transaction Player 1 keeps sword and potion since the transaction never succeeded.
                - If the transaction succeeds then Player 2 gets sword and potion.
        
        Example of database flow:

        Account
        │
        └── AccountID (Primary Key)
                │
                ▼
        Character
        │
        ├── CharacterID (Primary Key)
        └── AccountID (Foreign Key)
                │
                ▼
        Inventory
        │
        ├── InventoryID (Primary Key)
        ├── CharacterID (Foreign Key)
        └── ItemID (Foreign Key)
                │
                ▼
        Item
        │
        └── ItemID (Primary Key)

## Architecture Integration

        For Your Fantasy, PostgreSQL will not communicate directly with Unreal Engine.

        Instead:

        Player

        ↓

        Unreal Client

        ↓

        Login API

        ↓

        PostgreSQL

        The API becomes the only service allowed to communicate directly with the database.

        This improves security and allows business logic to remain centralized.

## How does Docker host PostgreSQL?

If Docker isn't directly on Ubuntu it lives inside a Docker container.

Architecture:
```
                                    Azure VM (Ubuntu)
                                    │
                                    ├── Docker Engine
                                    │
                                    ├── PostgreSQL Image
                                    │        │
                                    │        ▼
                                    │   PostgreSQL Container
                                    │
                                    └── Docker Volume
                                            │
                                            ▼
                                    Persistent Database Files
```

## Installation
```bash
mkdir -p ~/yourfantasy/database/postgres # Creates a directory that stores the Docker Compose project
# -p Creates as parent directories of they don't exist.

cd ~/yourfantasy/database/postgres #Change directory to Postgres

nano docker-compose.yml #Creates a YAML configuration file that tells Docker to create a Container for postgreSQL
    # nano = Notepad / VScode but for Linux
    # YAML = YAML is a human-readable configuration language commonly used to define infrastructure and application settings. Docker Compose uses YAML files to describe the containers, networking, storage, and configuration required to run an application.
#
#Paste the following into the YML configuration:

            services: # We're saying everything below is a service
            postgres: # We're calling one service postgres
                image: postgres:17 # Docker asks Docker Hub "Download PostgreSQL version 17"
                container_name: yourfantasy-postgres # Name the container
                restart: unless-stopped # If the VM reboots, restart PostgreSQL automatically
                environment: # Environment Variables
                POSTGRES_USER: yourfantasy_admin #Creates Admin Account
                POSTGRES_PASSWORD: # Create a Password
                POSTGRES_DB: yourfantasy_dev # Creates first DB
                ports: # Connects the following ports
                - "5432:5432" # Connects port 5432 inside container to 5432 on Ubuntu
                volumes:  # Tells Docker don't store data inside the container, store it here:
                - postgres_data:/var/lib/postgresql/data # Storage path

            volumes:
            postgres_data:
    
docker compose up -d #Start PostgreSQL
# docker = run docker
# compose = Use Docker Compose
# up = Bring everything online
# -d = Detached mode (This gives back the terminal)

docker ps # Show me running containers (Verification)
# ps = Shows running processes
# can add -a to show ALL containers

docker logs yourfantasy-postgres # Confirmation that PostgreSQL didn't just start but also initialized successfully to accept connections.
```

The following commands create a directory for the database in our project, then creates a Docker Compose YAML configuration file describing the PostgreSQL service. Docker Compose reads this file and instructs Docker Engine to download the PostgreSQL image (if needed), create a container from that image, configure networking, volumes, and environment variables, and start the database. It then checks running services on docker to confirm the container creation.


## Takeaways
- PostgreSQL stores persistent game data.
- Data is organized into related tables.
- Primary Keys uniquely identify records.
- Foreign Keys connect related tables.
- Transactions prevent partial updates.
- Indexes improve search performance.
- PostgreSQL is hosted inside Docker.
- Unreal Engine will access PostgreSQL through the Backend API.

## Revision History

### v1.0 Created on 7/9/26
- Initial PostgreSQL setup
- Docker Compose Deployment
- Verification Process