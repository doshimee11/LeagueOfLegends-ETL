# Analyzing League of Legends Ranked Games: An End-to-End ETL Process


## Table of Contents
- [Introduction](#introduction)
- [Objective](#objective)
- [About the Dataset](#about-the-dataset)
- [Methods Used](#methods-used)
- [Steps](#steps)
- [Technologies](#technologies)
- [Results](#results)
- [Conclusion](#conclusion)
- [Future Work](#future-work)


## Introduction
In the ever-evolving realm of online gaming, data has become the cornerstone of strategic advancement and player enhancement. This project embarks on a comprehensive journey to dissect and analyze ranked games from League of Legends (LoL), one of the world's most popular multiplayer online battle arena (MOBA) games. By crafting an end-to-end Extract, Transform, Load (ETL) process, we aim to unearth actionable insights that could empower players with winning strategies and deepen our understanding of the game's dynamics.


## Objective
The primary goal of this project is to develop a robust ETL pipeline that processes LoL match data to:
* **Extract:** Gather raw game data from reliable sources.
* **Transform:** Cleanse and manipulate the data to enhance its quality and usability.
* **Load:** Store the refined data into an accessible database for analysis.

Ultimately, this project seeks to provide a solid foundation for game analytics that can suggest strategies to increase winning probabilities, benefiting both casual players and competitive enthusiasts.


## About the Dataset
### Data Sources
1. ***League of Legends (LoL) - Ranked Games 2020***
    * **Source:** Kaggle Dataset by gyejr95
    * **Description:** A comprehensive dataset containing detailed information on 10,800 ranked matches from the Korea (KR) server in 2020.
    * **Key Files:**
        * *match_df:* Participant details for both winning and losing teams.
        * *winner_df:* In-game statistics and objectives for the winning team.
        * *loser_df:* In-game statistics and objectives for the losing team.

2. ***LoL Data Dragon - Patch 10.15.1***
    * **Source:** Data Dragon by Riot Games
    * **Description:** Official game data and assets up to patch 10.15.1, including champions, items, and other game elements.
    * **Key Files:**
        * *meta_champs.json:* Metadata for all champions.
        * *meta_items.json:* Information on all purchasable in-game items.

### Dataset Characteristics
* **Volume:** High-volume datasets suitable for extensive analysis.
* **Variety:** Includes both structured match data and unstructured metadata.
* **Relevance:** Focused on the competitive scene (ranked games) in one of the most active regions (KR server).


## Methods Used
* ***Data Extraction:***
    * Kaggle API: Downloaded datasets programmatically.
    * File Handling: Used pickle and json libraries to read various file formats.
    * DataFrames: Loaded data into pandas for efficient manipulation.
* ***Data Transformation:***
    * Data Cleaning:
        * Addressed missing values in the win column of `loser_df`.
        * Filtered data to include only 'CLASSIC' game modes.
        * Removed remade games (matches ≤ 15 minutes) that lack strategic value.
    * Data Integration:
        * Merged datasets to establish relationships between matches, champions, and items.
        * Mapped champion and item IDs to their names for better readability.
* ***Data Loading:***
    * AWS RDS PostgreSQL:
        * Created a relational database to store cleaned data.
        * Ensured data integrity with primary and foreign keys.


## Steps
1. Project Scope and Data Gathering:
    * Defined clear objectives to construct an ETL pipeline for League of Legends ranked games.
    * Gathered data using the Kaggle API and downloaded Data Dragon assets from Riot Games.
2. Exploring and Assessing the Data:
    * Issue 1: Addressed NaN values in `loser_df` by removing tutorial games.
    * Issue 2: Filtered out non-'CLASSIC' game modes to focus on standard ranked matches.
    * Issue 3: Excluded remade games with durations ≤ 15 minutes to ensure meaningful analysis.
3. Defining the Data Model:
    * Adopted a star schema for efficient querying.
    * **`Fact Table`:**
        * `matches`: Contains match-level data like `gameId`, `gameDuration`, and performance indicators.
    * **`Dimension Tables`:**
        * champions: Details champions used in matches.
        * items: Information on items purchased.
        * players: Player-specific data.
4. Mapping Out Data Pipelines:
    * Database Setup:
        * Created a PostgreSQL database on AWS RDS with secure access.
    * Schema Definition:
        * Established tables with appropriate data types and constraints.
    * Data Loading Sequence:
        * Loaded dimension tables first to set reference data.
        * Loaded the fact table, linking it to dimensions via foreign keys.
    * Testing and Validation:
        * Ran queries to verify correct data population.
        * Checked for anomalies or discrepancies to ensure data integrity.


## Technologies
* Programming Language: Python 3.x
* Data Manipulation:
    * pandas: For DataFrame operations.
    * numpy: For numerical computations.
* APIs and Data Access:
    * kaggle: For dataset downloads.
    * psycopg2: PostgreSQL database adapter.
    * sqlalchemy: For ORM and database handling.
* Data Formats:
    * pickle: For serializing and deserializing Python objects.
    * json: For handling JSON files, particularly metadata.
* Cloud Services:
    * AWS RDS: Hosted the PostgreSQL database for scalable and reliable storage.


## Results
#### 1. ***What Impact Do Data Quality Issues Have on Analysis?***
* **Understanding Missing Values:** Recognized that NaN values in key columns indicate non-standard matches (e.g., tutorials) which could skew analysis.
* **Game Mode Relevance:** By filtering to 'CLASSIC' games only, ensured that the analysis reflects the standard competitive environment.

#### 2. ***How Do Remade Games Affect Data Integrity?***
* **Early Termination Matches:** Remade games don't provide full match data, potentially misleading statistical insights.
* **Data Cleaning Importance:** Excluding these matches improved the reliability of win/loss ratios and performance metrics.

#### 3. ***How Can the ETL Process Be Optimized for Large Datasets?***
* **Data Chunking:** For massive datasets, processing in chunks could prevent memory overloads.
* **Parallel Processing:** Utilizing multi-threading or distributed computing frameworks to speed up data transformation.
* **Incremental Loading:** Implementing change data capture (CDC) techniques for updating the database efficiently.


## Conclusion
This project lays the groundwork for an in-depth exploration of League of Legends gameplay through data. By meticulously constructing an ETL pipeline and addressing data quality issues, I have created a robust platform for generating valuable insights.


## Future Work
* **Real-Time Data Integration:** Incorporate live match data to provide up-to-date analytics.
* **Advanced Predictive Models:** Develop machine learning models to predict match outcomes based on champion select and item builds.
* **User Interface:** Create a dashboard for interactive data exploration.
* **Community Collaboration:** Opening the project to contributions from the community to enrich the dataset and analysis.
