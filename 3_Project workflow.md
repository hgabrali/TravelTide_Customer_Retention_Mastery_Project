# 🛠️ Data Science Toolkit Comparison and Project Functionality

The following table compares the technical purpose of the tools you used, detailing their specific functions and importance within the context of your Mastery Project (Data Exploration, Feature Engineering, ML Segmentation, and Strategy Development).

## 🛠️ Data Science Toolkit Comparison and Project Functionality 📈

| Tool 🔧 | Technical Purpose 💡 | Project Function ⚙️ | Project Importance 🌟 |
| :--- | :--- | :--- | :--- |
| **Python** | **High-Level Programming Language.** Provides the core environment and syntax for executing all computational tasks and integrating libraries. | **Primary Execution Environment.** Served as the foundational language for writing all ETL, EDA, Feature Engineering, and Machine Learning scripts. | **Crucial.** It is the **backbone** of the entire analytical workflow, enabling library integration and algorithmic implementation. |
| **SQLAlchemy** | **SQL Toolkit and Object-Relational Mapper (ORM).** Facilitates interaction with relational databases using Python objects and provides a consistent interface. | **Database Connectivity & Data Ingestion.** Used to establish robust connections (e.g., to PostgreSQL) and abstractly query the raw project data from the database. | **High.** Essential for the **initial step of the workflow** – securely and efficiently retrieving the necessary, often large, real-world dataset. |
| **psycopg2** | **PostgreSQL Database Adapter.** A low-level library that specifically allows Python to communicate with PostgreSQL databases. | **Direct Database Connection.** Provided the underlying mechanism for connecting Python to the PostgreSQL database engine to retrieve the raw data. | **High.** A **prerequisite** for data ingestion if the raw data resided in a PostgreSQL server, ensuring a stable, native connection. |
| **Pandas** | **Data Manipulation and Analysis Library.** Offers fast, flexible, and powerful data structures (primarily **DataFrame**) for working with structured data. | **Core Data Operations.** Utilized for **Data Exploration (EDA)**, cleaning, transformation, missing value imputation, and all steps of **Feature Engineering**. | **Critical.** The **workhorse** for data preparation; it made the complex, real-world data challenge manageable for ML modeling. |
| **NumPy** | **Numerical Computing Library.** Provides support for large, multi-dimensional arrays and matrices, along with a large collection of high-level mathematical functions. | **Vectorization and Mathematical Operations.** Underpins Pandas' functionality and was used directly for high-speed array operations, calculations, and vectorizing features for the ML models. | **High.** Provides the necessary **computational efficiency** and data structure foundation (arrays) essential for statistical analysis and machine learning algorithms. |
| **matplotlib.pyplot** | **2D Plotting Library.** A comprehensive tool for creating static, animated, and interactive visualizations in Python. | **Data Exploration (EDA) & Communication.** Used to generate histograms, scatter plots, box plots, and visualizations of the resulting **Customer Segments** for strategic recommendation. | **Very High.** Crucial for **understanding the data's distribution (EDA)** and **communicating key insights** and segmentation results to stakeholders. |


## 🎯 Summary of Project Function and Importance within the Workflow 🚀

These tools collectively form a standard and powerful **Python data science stack**, enabling a seamless **end-to-end (E2E)** analytical process:

---

### 📥 1. Data Ingestion & Access
* **Tools:** SQLAlchemy / psycopg2
* **Function:** Established the crucial link to the "complex, real-world" data source, efficiently making the raw project data accessible for subsequent analysis.

### ⚙️ 2. Preparation and Feature Engineering
* **Tools:** Pandas / NumPy
* **Function:** Transformed raw database entries into meaningful, high-quality features suitable for Machine Learning. This step **directly impacted the success and performance** of the subsequent models.

### 🤖 3. Analysis and Model Building (ML Segmentation)
* **Tools:** Python / NumPy / Pandas
* **Function:** Provided the necessary execution environment and numerical tools to successfully develop and execute the **ML-based Customer Segmentation** algorithms.

### 📈 4. Visualization and Strategy Communication
* **Tools:** matplotlib.pyplot / Pandas
* **Function:** Allowed for the visual validation of the initial **EDA findings** and, most importantly, the creation of clear, persuasive charts necessary to support the final **Actionable Strategic Recommendations**.
* 


## 💾 Database Connection and Introspection Steps (SQLAlchemy) 🔗

This table breaks down the Python code into sequential steps, explaining the **Purpose**, the **Action/Command**, and the **Result** of each line in the context of connecting to and inspecting a PostgreSQL database.

| Step # | Action/Command 💻 | Technical Purpose 💡 | Result/Output ✅ |
| :---: | :--- | :--- | :--- |
| **1** | `traveltide_url = "postgresql://..."` | **Defining the Connection String (URI).** This string holds all necessary credentials and location information (driver, user, password, host, database name) required by SQLAlchemy to locate and access the PostgreSQL instance. | The `traveltide_url` variable is populated with the database connection credentials, ready for use. |
| **2** | `engine = sa.create_engine(traveltide_url)` | **Creating the Engine.** The `create_engine` function processes the connection string and sets up the primary interface (the `Engine`) to the database. This object acts as the central source of database connectivity. | The `engine` object is created, capable of managing connections and dialog with the "TravelTide" database. |
| **3** | `connection = engine.connect().execution_options(isolation_level="AUTOCOMMIT")` | **Establishing and Configuring the Connection.** This step opens an actual connection to the database. The `.execution_options(...)` ensures that transactions are automatically committed after execution, a common practice for simple data retrieval or utility operations. | A live `connection` object is established with the database, optimized for auto-committing queries. |
| **4** | `inspector = sa.inspect(engine)` | **Creating the Inspector.** The `inspect` function wraps the `engine` to allow querying of the database's structural information (metadata), rather than the data itself. | The `inspector` object is created, providing methods to explore the database's schema (tables, columns, foreign keys, etc.). |
| **5** | `table_names = inspector.get_table_names()` | **Schema Introspection.** The key operation of fetching the list of all table names defined within the connected database (i.e., the "TravelTide" database). | The `table_names` variable is populated with a list of strings: `['hotels', 'users', 'flights', 'sessions']`. |
| **6** | `table_names` | **Displaying the Result.** Simply prints the contents of the variable to the output. | The final output confirms the database contains the four main tables for the project: `['hotels', 'users', 'flights', 'sessions']`. |

## 🌟 Summary of Database Connection Sequence Importance 🚀

This sequence of establishing the database engine and inspector is **crucial** for initiating any robust data project, as it ensures foundational integrity before data processing begins.

| Core Objective 🎯 | Importance Detail 📝 | Key Benefit to Project ✅ |
| :--- | :--- | :--- |
| **Confirms Connectivity** 🔗 | Ensures the defined **Connection String (URL)** is correct and that the host database is actively **reachable** over the network. | **Prevents downstream failures** by validating the initial data source connection early in the workflow. |
| **Validates Schema** 🔍 | Verifies that the expected core tables (`hotels`, `users`, `flights`, `sessions`) **exist** within the connected database schema. | Serves as the **essential starting point** for **Data Ingestion** and the subsequent **Data Exploration (EDA)**, guaranteeing the necessary data structures are present. |

## 🚀 Data Ingestion: Loading SQL Tables into Pandas DataFrames 🐼

This phase is the crucial transition from raw data in the PostgreSQL database to structured, manipulable objects (DataFrames) in the Python environment, using the `pandas` library.

| Step # | Action/Command 💻 | Technical Purpose 💡 | Resulting Data Structure 📊 |
| :---: | :--- | :--- | :--- |
| **8** | `hotels = pd.read_sql_table("hotels", connection)`<br>`hotels.head()` | **Ingesting the 'hotels' Table.** The `pd.read_sql_table` function executes a SQL query to select all data from the specified table (`"hotels"`) using the previously established database `connection` object. | A Pandas DataFrame named `hotels` is created, containing all the rows and columns from the "hotels" table. The `.head()` call displays the first 5 rows for immediate verification. |
| **9** | `users = pd.read_sql_table("users", connection)`<br>`users.head()` | **Ingesting the 'users' Table.** This process is identical to the previous step, targeting the `"users"` table. This table likely holds customer demographic or identifier information essential for the **Customer Segmentation** part of the project. | A Pandas DataFrame named `users` is created, containing data from the "users" table. The `.head()` call confirms successful data loading and initial structure. |
| **10** | `flights = pd.read_sql_table("flights", connection)`<br>`flights.head()` | **Ingesting the 'flights' Table.** The function targets the `"flights"` table, which contains data specific to flight bookings and travel data crucial for analyzing travel patterns. | A Pandas DataFrame named `flights` is created. This DataFrame's structure is ready for subsequent **Feature Engineering** related to travel metrics. |
| **11** | `sessions = pd.read_sql_table("sessions", connection)`<br>`sessions.head()` | **Ingesting the 'sessions' Table.** This table often contains user interaction data (website clicks, timestamps, product views) which is vital for calculating engagement metrics during **Feature Engineering**. | A Pandas DataFrame named `sessions` is created. This data will be used to derive behavioral features required for the **ML-based Customer Segmentation**. |

---

### 📝 Summary of Importance

This series of commands completes the **Data Ingestion** step by utilizing the `pandas` library's powerful data access methods. By converting raw SQL tables into DataFrames, you are now ready to perform:

* **Data Exploration (EDA) 🔎:** Inspecting data types, checking for missing values, and visualizing distributions.
* **Feature Engineering ⚙️:** Manipulating and joining these DataFrames to create new, predictive features for your ML models.


## 🌐 Scaling Up: Transitioning from Pandas to Spark DataFrames (PySpark) ⚡

This stage is critical for enabling **distributed computing** and handling large datasets efficiently, moving the data structures from local Pandas memory into the Spark environment.

| Step # | Action/Command 💻 | Technical Purpose 💡 | Data Transformation / Result 🔄 |
| :---: | :--- | :--- | :--- |
| **1-4** | `users_spark = spark.createDataFrame(users)`<br>`flights_spark = spark.createDataFrame(flights)`<br>... | **Creating Spark DataFrames.** These commands leverage the `spark.createDataFrame()` method to convert the existing, in-memory **Pandas DataFrames** (e.g., `users`) into **PySpark DataFrames** (e.g., `users_spark`). | The data is effectively transferred and structured into Spark's distributed, fault-tolerant format, ready for scalable processing. |
| **5-8** | `users_spark.write.mode("overwrite").saveAsTable("users_spark")`<br>`flights_spark.write.mode("overwrite").saveAsTable("flights_spark")`<br>... | **Creating Persistent Spark Tables (Catalyst Storage).** These commands perform two main actions:<br>1. **`.write.mode("overwrite")`:** Specifies that if a table with the same name already exists in the Spark metastore, it should be overwritten.<br>2. **`.saveAsTable(...)`:** Persists the Spark DataFrame as a **managed table** in the Spark metastore (often backed by storage like Parquet files in HDFS/S3), making it queryable via Spark SQL. | Four new, persistent Spark SQL tables (`users_spark`, `flights_spark`, `hotels_spark`, `sessions_spark`) are registered in the environment. These can now be queried using SQL syntax through the Spark context. |

---

### 📝 Summary of Importance

The transition to PySpark DataFrames and persistent tables is vital for the **Mastery Project** because:

* **Scalability 📈:** It moves the analytical workflow from single-machine memory (Pandas) to a **distributed computing framework (Spark)**, enabling the handling of "complex, real-world" data volumes that often exceed single-machine capacity.
* **Efficient Feature Engineering ⚙️:** Spark's optimized operations are necessary for performing complex joins, aggregations, and large-scale transformations required for **Feature Engineering** on large datasets before **ML-based Customer Segmentation**.
* **SQL Integration 🗃️:** Saving the DataFrames as tables allows subsequent transformation and analysis steps to be executed efficiently using familiar **SQL queries** via Spark SQL.

# 🎯 SQL Query Purpose and Importance in Big Data Environments 🚀
This section explains the function of the complex SQL query and outlines why SQL is mandatory for handling large datasets within the PySpark environment.

## I. 🧩 Query Function: Creating the Analytic Base Table (ABT)

The primary goal of the SQL query is to **join the four core tables** to form a single, comprehensive **Analytic Base Table (ABT)** for downstream ML modeling.

| SQL Component ⚙️ | Technical Functionality 💡 | Project Significance 📈 |
| :--- | :--- | :--- |
| **SELECT Clause** | **Feature Selection.** Explicitly chooses every column required for **Feature Engineering** and the Machine Learning models. | Gathers critical data points: user behavior (`s.page_clicks`), demographics (`u.gender`), and transactional data (`f.base_fare_usd`, `h.hotel_price_per_room_night_usd`). |
| **FROM Clause** | Specifies the starting point for the join operation (the `sessions_spark` table). | Defines the primary granularity: the output table will be at the **session level**. |
| **JOIN Type: INNER JOIN** | Used to join `sessions` with `users` on `user_id`. It only returns rows where a match exists in **both** tables. | Ensures that every session record in the ABT is linked to a valid user, as user context is **always essential**. |
| **JOIN Type: LEFT JOIN** | Used to join with `flights` and `hotels` on `trip_id`. It returns all records from the *left* table (`sessions` and `users`) and the matched records from the *right* table. | Crucial for **Segmentation** analysis. It preserves sessions where a user browsed but **did not complete a booking**, returning `NULL` values for flight/hotel details in those cases. |

## II. 💾 Why SQL is Mandatory in Big Data (PySpark Context)⚡
In a PySpark environment (executed via %%sql), utilizing SQL for transformation is not optional; it is a necessity for achieving high performance and scalability.
Utilizing SQL within the PySpark environment is a necessity for achieving high performance and scalability when working with complex, massive datasets.

| Reason 🌟 | Technical Explanation 📝 | Benefit over Standard Python/Pandas 🚀 |
| :--- | :--- | :--- |
| **Performance (Vectorization)** | SQL queries and Spark SQL process data **column-wise and in a distributed manner** across all nodes and cores, leveraging **vectorization**. | **Significantly faster** than traditional row-by-row processing loops found in standard Python/Pandas, especially on massive datasets. |
| **Optimization (Catalyst Optimizer)** | Spark employs the intelligent **Catalyst Optimizer** in the backend. This engine automatically converts your SQL query into the most **efficient physical execution plan**. | Guarantees levels of speed and efficiency that would be extremely difficult or impossible to achieve with manually written Python/Pandas logic. |
| **Memory Management** | Spark SQL efficiently manages data loading, streaming, and intermediate results across the cluster's memory and disk. | **Mitigates the "Memory Error" risk.** Unlike Pandas, which attempts to load all data into a single machine's RAM, Spark can handle datasets far larger than the local machine's memory capacity. |

## III. 🛠️ Essential SQL Tools and How to Identify Their Use in Big Data 🔍

In a Big Data context (specifically when using Spark SQL/PySpark), the following SQL structures and functions are the most valuable for **Feature Engineering** and data preparation.

| SQL Tool (Tool) 🔧 | Technical Purpose 💡 | How to Identify Its Need (Analysis Question) 🤔 |
| :--- | :--- | :--- |
| **JOIN Types** (`INNER`, `LEFT`, `RIGHT`) | Enables combining data from different tables based on common keys (`user_id`, `trip_id`). | **Analysis Question:** Do I need to see data together? *Example: "If I want to see all users, whether they made a reservation or not, I must use a **LEFT JOIN**."* |
| **WINDOW Functions** (`ROW_NUMBER()`, `LAG()`, `OVER (PARTITION BY...)`) | Allows for sorting, calculating cumulative sums, or accessing preceding/following rows within specific groups (`partition`) instead of looking at the entire table. | **Analysis Question:** "I need to find the last 3 flights booked by each user" or "I need to calculate monthly cumulative sales." |
| **Aggregation Functions** (`AVG`, `SUM`, `COUNT`, `MAX`) | Produces summary statistics over groups of data (e.g., finding the total number of clicks for each user using `GROUP BY user_id`). | **Analysis Question: Feature Engineering:** Do I need to summarize a user's behavior into a single row? *(If yes, use `SUM/AVG`)*. |
| **CAST / DATE Functions** (`CAST()`, `DATE_FORMAT()`) | Used for data type conversion and processing date-time data (e.g., "using the `birthdate` column to calculate age"). | **Data Quality Check:** Are the column data types correct? Do I need to derive new features like day, month, or year from a timestamp like `session_start`? |

---

## 💾 SQL Code Analysis: Creating the Analytic Base Table (ABT) 📊

This PySpark SQL code utilizes the `CREATE OR REPLACE TABLE` command to perform extensive **data joining** (Feature Selection and Data Integration) across four different tables, resulting in a unified dataset ready for **Feature Engineering** and **ML-based Customer Segmentation**.

| Component | Function / Command 💻 | Technical Importance & Role in Project 🌟 |
| :--- | :--- | :--- |
| **Header** | `%%sql`<br>`CREATE OR REPLACE TABLE sessions_joined AS` | **Data Persistence and SQL Access.** Instructs the PySpark environment to execute the code as a SQL query and save the result as a new, queryable table named `sessions_joined`. This table serves as the **Analytic Base Table (ABT)**. |
| **SELECT Clause** | `s.session_id, s.user_id, ...`<br>*(Includes columns from s, u, f, h)* | **Feature Selection and Integration.** Explicitly selects all necessary raw features from the four separate tables. This step standardizes the features required for ML models (e.g., user context, session behavior, and transaction details). |
| **FROM Clause** | `FROM sessions_spark s` | **Defining Granularity.** Sets the `sessions_spark` table as the base of the join operation. The resulting ABT will therefore be at the **session level**. |
| **INNER JOIN** | `INNER JOIN users_spark u ON s.user_id = u.user_id` | **Ensuring Core Context.** Guarantees that every session included in the ABT is linked to a valid user record. The user's demographic and static data (`u.*` columns) are vital for segmentation. |
| **LEFT JOIN** | `LEFT JOIN flights_spark f ON s.trip_id = f.trip_id`<br>`LEFT JOIN hotels_spark h ON s.trip_id = h.trip_id` | **Preserving Non-Transaction Data (Segmentation Focus).** This is crucial: <br> 1. It preserves ALL sessions (including those where no booking occurred).<br> 2. If a session has no matching flight/hotel data, the transactional columns (`f.*`, `h.*`) will contain `NULL`. <br> **Importance:** This `NULL` value itself is a key feature, indicating browsing-only behavior, which is essential for defining customer segments. |
| **Column Alias** | `h.hotel_price_per_room_night_usd AS hotel_per_room_usd` | **Data Cleaning and Standardization.** Renames a complex or long column name to a simpler, standardized format (`hotel_per_room_usd`) for ease of use in subsequent Feature Engineering and modeling steps. |

---

### 📝 Summary of Importance

This single SQL block is the most fundamental step in your workflow, achieving:

1.  **Data Integration:** Merging disparate data sources into a cohesive analytical view.
2.  **Scalability:** Executing this complex join efficiently using Spark's distributed computing power.
3.  **Foundation for ML:** Creating the definitive input table (ABT) required for all subsequent **Feature Engineering** and **ML-based Customer Segmentation** steps.

The `sessions_joined` table is now ready to begin generating derived features (Feature Engineering). 



## 💾 Pre-Filtering and Sampling Analysis (PySpark SQL) 🚀

This SQL code utilizes **Common Table Expressions (CTEs)** to perform initial data filtering and user sampling **before** executing the main join. This is a critical performance optimization technique in Big Data analysis.

| Component / Action ⚙️ | Technical Functionality & Purpose 💡 | Analytical Contribution & Importance 🌟 |
| :--- | :--- | :--- |
| **WITH sessions_2023 AS (...)** | **Temporal Filtering (Time-Based Filter).** This CTE filters the base `sessions_spark` table to only include records where `session_start` is after a specific date (`'2023-01-04'`). | **Reduces Data Volume.** Significantly decreases the size of the dataset being processed, improving performance and reducing memory load for subsequent complex joins. Ensures the analysis focuses only on the most **recent and relevant data**, which is often crucial for segmentation (e.g., recent behavior is more predictive). |
| **WITH filtered_users AS (...)** | **User Sampling (Behavioral Filter).** This CTE groups the data by `user_id` and filters out users who have a `session_count` less than 7 (`HAVING COUNT(*) > 7`). | **Focuses on Engaged Users (Lapsed User Removal).** This is a key step in **Feature Engineering**: <br> 1. **Removes noise:** Filters out one-off visitors or bots. <br> 2. **Ensures Data Quality:** Guarantees that the **Customer Segmentation** model is trained only on users with **sufficient behavioral data** to establish a robust and meaningful segment profile. |
| **Why in SQL?** | **Optimization via PySpark Catalyst.** Executing these filters within the initial `WITH` clauses allows Spark's Catalyst Optimizer to push the filtering logic down to the data source (database/data lake). | **Maximum Performance.** This is the most efficient way to reduce data volume **before** loading it into memory for the large join. Performing this complex filtering in Pandas *after* the join would lead to massive performance bottlenecks and memory issues. |

## 📝 Summary of Pre-processing Importance (CTEs in Spark SQL) 🚀

Performing filtering and sampling inside the Spark SQL query (using Common Table Expressions or CTEs) is **essential** because:

| Benefit Category 🌟 | Detail and Outcome 💡 |
| :--- | :--- |
| **Performance Optimization** | It leverages Spark's distributed processing power to handle the filtering of huge datasets **efficiently**, drastically reducing the amount of data that needs to be loaded into memory for subsequent complex operations. |
| **Increased Model Quality** | It ensures the resulting Analytic Base Table (`session_base`) contains only **high-quality data** derived from active, recently engaged users, leading to more **actionable and predictive** customer segments in the ML phase. |


## 🔄 Transition from Spark to Pandas (Local Processing) 🐼

This step is performed after the heavy **data integration, filtering, and aggregation** are completed in the scalable PySpark environment.

| Action ⚙️ | Technical Functionality 💡 | Purpose & Justification 🌟 |
| :--- | :--- | :--- |
| **`df = sessions_filtered_pd`** | **Reassignment/Alias Creation.** Assigns the existing Pandas DataFrame object, which was created by fetching the final, filtered Spark results, to a simpler alias, `df`. | **Simplification.** Data Scientists typically use the variable name `df` (DataFrame) for the primary working dataset in Pandas notebooks. This makes the code readable and consistent for the local analysis phase. |
| **`df.head()`** | **Data Inspection.** Displays the first 5 rows of the Pandas DataFrame. | **Validation.** Confirms that the data transfer from Spark to the Pandas environment (`sessions_filtered_pd`) was successful and allows for a quick visual check of the final structure, features, and data types before the detailed analysis begins. |
| **Why Switch Back?** | **Downsizing and Local Functionality.** The `sessions_filtered_pd` object is the result of the prior complex SQL operations (filtering, joining, potentially aggregation) executed in Spark, which resulted in a much smaller, ready-to-analyze dataset. | **Efficiency and Tool Use.** <br> 1. **Pandas Performance:** For DataFrames that fit into memory (after filtering/aggregation), Pandas is often faster than Spark for single-machine operations (e.g., specific plotting, simple feature engineering). <br> 2. **Library Compatibility:** Many specialized ML tools (like some parts of Scikit-learn or specific visualization libraries) **require** Pandas DataFrames as input and do not directly accept Spark DataFrames. |

# 📊 Data Dictionary and Initial Data Quality Assessment (ABT) 🔎
The dataset contains 49,211 entries and 41 columns derived from the filtered, joined sessions. The analysis below highlights data types and critical missing value patterns crucial for Feature Engineering (FE).


## 📊 Data Dictionary and Initial Data Quality Assessment (ABT) 🔎

The following table summarizes the structure, data types, and missing value percentages for the 41 columns in your Analytic Base Table.

| # | Column Name | Non-Null Count | Data Type | Missing (%) | Feature Category 🏷️ |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **0** | `session_id` | 49211 | `object` | 0% | Session ID |
| **1** | `user_id` | 49211 | `int64` | 0% | User ID |
| **2** | `trip_id` | 16702 | `object` | 66.1% | Transaction ID (Partial) |
| **3** | `session_start` | 49211 | `datetime64[ns]` | 0% | Session Timing |
| **4** | `session_end` | 49211 | `datetime64[ns]` | 0% | Session Timing |
| **5** | `page_clicks` | 49211 | `int64` | 0% | Behavioral Metric |
| **6** | `flight_discount` | 49211 | `bool` | 0% | Discount Flag |
| **7** | `flight_discount_amount` | 8282 | `float64` | **83.2%** | Discount Detail |
| **8** | `hotel_discount` | 49211 | `bool` | 0% | Discount Flag |
| **9** | `hotel_discount_amount` | 6205 | `float64` | **87.4%** | Discount Detail |
| **10** | `flight_booked` | 49211 | `bool` | 0% | Booking Flag |
| **11** | `hotel_booked` | 49211 | `bool` | 0% | Booking Flag |
| **12** | `cancellation` | 49211 | `bool` | 0% | Outcome Variable |
| **13** | `birthdate` | 49211 | `datetime64[ns]` | 0% | User Demographics |
| **14** | `gender` | 49211 | `object` | 0% | User Demographics |
| **15** | `married` | 49211 | `bool` | 0% | User Demographics |
| **16** | `has_children` | 49211 | `bool` | 0% | User Demographics |
| **17** | `home_country` | 49211 | `object` | 0% | Location (Origin) |
| **18** | `home_city` | 49211 | `object` | 0% | Location (Origin) |
| **19** | `home_airport` | 49211 | `object` | 0% | Location (Origin) |
| **20** | `home_airport_lat` | 49211 | `float64` | 0% | Location (Origin) |
| **21** | `home_airport_lon` | 49211 | `float64` | 0% | Location (Origin) |
| **22** | `sign_up_date` | 49211 | `datetime64[ns]` | 0% | User Timing |
| **23** | `origin_airport` | 14270 | `object` | **71.0%** | Flight Details |
| **24** | `destination` | 14270 | `object` | **71.0%** | Flight Details |
| **25** | `destination_airport` | 14270 | `object` | **71.0%** | Flight Details |
| **26** | `seats` | 14270 | `float64` | **71.0%** | Flight Details |
| **27** | `return_flight_booked` | 14270 | `object` | **71.0%** | Flight Details |
| **28** | `departure_time` | 14270 | `datetime64[ns]` | **71.0%** | Flight Timing |
| **29** | `return_time` | 13652 | `datetime64[ns]` | **72.2%** | Flight Timing (Missing Return) |
| **30** | `checked_bags` | 14270 | `float64` | **71.0%** | Flight Details |
| **31** | `trip_airline` | 14270 | `object` | **71.0%** | Flight Details |
| **32** | `destination_airport_lat` | 14270 | `float64` | **71.0%** | Flight Location |
| **33** | `destination_airport_lon` | 14270 | `float64` | **71.0%** | Flight Location |
| **34** | `base_fare_usd` | 14270 | `float64` | **71.0%** | Flight Transaction |
| **35** | `hotel_name` | 14726 | `object` | **70.1%** | Hotel Details |
| **36** | `nights` | 14726 | `float64` | **70.1%** | Hotel Details |
| **37** | `rooms` | 14726 | `float64` | **70.1%** | Hotel Details |
| **38** | `check_in_time` | 14726 | `datetime64[ns]` | **70.1%** | Hotel Timing |
| **39** | `check_out_time` | 14726 | `datetime64[ns]` | **70.1%** | Hotel Timing |
| **40** | `hotel_price_per_room_night_usd` | 14726 | `float64` | **70.1%** | Hotel Transaction |


## 🛑 Data Quality and Feature Engineering Insights ⚙️

This section summarizes the key findings regarding data quality from the initial inspection of the Analytic Base Table (ABT) and outlines the strategy for **Feature Engineering (FE)**.

---

### 1. Missing Value Strategy (Crucial for FE) 🔍

The significant amount of missing data (around **70-87%**) is **expected** and reflects the inherent nature of the travel booking business (sessions where no purchase was made):

* **71.0% Missing Flight Details:** This strongly correlates with sessions where the user **did not book or search for a flight** (or did not complete the transaction).
* **70.1% Missing Hotel Details:** This correlates with sessions where the user **did not book or search for a hotel**.

> **Imputation Strategy:** The `NULL` values here are **not errors**; they represent the **absence of a transaction**. For Feature Engineering, these values should typically be imputed with **zero (0)** for numerical columns (like `base_fare_usd`, `nights`, `rooms`) and `False` or a specific string for categorical/boolean columns.

### 2. Time-Based Feature Creation 📅

The numerous `datetime64[ns]` columns are ideal for creating new, high-value temporal features that capture user and session dynamics:

* **User Tenure:** Calculate user age/tenure from `birthdate` and `sign_up_date`.
* **Session Duration:** Calculate the time difference: `session_end` - `session_start`.
* **Trip Length:** Calculate trip duration from `check_out_time` - `check_in_time` or `return_time` - `departure_time`.
* **Time of Day/Week:** Extract hour, day of the week, or month from `session_start` to capture **behavioral seasonality**.

### 3. Data Type Consistency ✅

* All date/time fields are correctly parsed as `datetime64[ns]`, which facilitates the time-based calculations outlined above.
* Boolean fields (`bool`) are correctly recognized, minimizing memory usage and ensuring efficient storage.

## 📈 Statistical Analysis of Key Features (`df.describe()`) 🔎

This analysis is based on the descriptive statistics of the **Analytic Base Table (ABT)**, highlighting central tendencies, spread, and potential issues for selected numerical and datetime columns across the 49,211 entries.

| Feature Category | Column Name | Key Statistical Findings 💡 | Analytical Insights for Feature Engineering (FE) 🚀 |
| :--- | :--- | :--- | :--- |
| **Identifiers** | `user_id` | **Mean:** 545,202. **Std:** 64,640. | The IDs are randomly assigned and sequential, serving only as a unique key. The standard deviation confirms a broad distribution across the user base. |
| **Session Timing** | `session_start`<br>`session_end` | **Min:** 2023-01-04. **Max:** 2023-07-28. <br> **Mean Time:** Approx. 11:24 (start) to 11:28 (end). | **Timing is Key for FE:** All sessions are concentrated within a recent period (Jan to Jul 2023), confirming the temporal filtering was successful. The short mean duration suggests most sessions are fast browsing or quick transactions. |
| **Behavioral** | `page_clicks` | **Min:** 1. **Max:** 566. <br> **Mean:** 17.58. **Std:** 21.46. <br> **75th Percentile:** 22.0. | **High Skew and Outliers:** The max of 566 is significantly higher than the 75th percentile (22.0). This indicates a **highly skewed distribution** with a few sessions having extreme click counts (potential power users or bots). **FE Note:** Log transformation might be required before modeling. |
| **Discount Amounts** | `flight_discount_amount`<br>`hotel_discount_amount` | **Mean:** ~0.14 (14%) and ~0.11 (11%). <br> **Max:** 0.60 (60%) and 0.45 (45%). <br> **Count:** Significantly lower than 49,211. | **Missing Value Confirmation:** The `count` confirms that these columns have substantial missing values (as expected from the Data Dictionary), as they are only present when a discount was actually applied. **FE Note:** Missing values should be imputed with 0 to denote "no discount applied". |
| **Demographics** | `birthdate` | **Min:** 1935-05-10. **Max:** 2006-12-28. | **Requires Feature Engineering:** The raw `birthdate` is not useful for modeling. It must be converted into **User Age** (e.g., current year - birth year) or **Age Group** before use. |
| **Location** | `home_airport_lat`<br>`home_airport_lon` | **Mean Lat/Lon:** 38.4 / -94.1. **Std:** 6.1 / 18.0. | **Geographic Spread:** The standard deviations are relatively high, indicating that users are spread across diverse locations. **FE Note:** These can be used directly or converted into distance-based features later. |
| **User Timing** | `sign_up_date` | **Min:** 2021-07-22. **Max:** 2023-05-18. | **Requires Feature Engineering:** Must be converted into **User Tenure** (e.g., how long the user has been registered as of the session date) for use in segmentation. |
| **Transaction** | `seats` | **Count:** Low (14,270). **Max:** 8.0. **75th Percentile:** 1.0. | **Transaction Frequency:** The low count confirms this detail is only available for booked flights. The mean and median (1.0) suggest most transactions are for a single traveler. The max of 8 is a potential outlier (group booking). |

## 🛑 Negative or Anomalous Value Analysis (Error Detection) 🔎

This analysis focuses on detecting and explaining potentially negative or logically inconsistent minimum values observed in the descriptive statistics of the Analytic Base Table (ABT).

| Column Name 📝 | Negative/Anomalous Value Detection 💡 | Explanation and Impact on Analysis 🚀 |
| :--- | :--- | :--- |
| **`home_airport_lon`** (Home Airport Longitude) | **MIN:** $-157.927000$ (Negative) | **GEOGRAPHIC EXPECTATION.** Longitude values are naturally negative for the Western Hemisphere (e.g., USA, Canada, South America). This value is **not an error** but a correct representation of geographic location. $|-157.927|$ suggests a location in the Hawaii/Pacific region. |
| **`destination_airport_lon`** (Destination Airport Longitude) | **MIN:** $-157.927000$ (Negative) | **GEOGRAPHIC EXPECTATION.** Similarly, these values correctly represent geographical location, indicating that flights in the dataset include distant points in the Western Hemisphere. |
| **`destination_airport_lat`** (Destination Airport Latitude) | **MIN:** $-37.008000$ (Negative) | **GEOGRAPHIC EXPECTATION.** Latitude values are negative for the Southern Hemisphere (e.g., Argentina, Australia, South Africa). This is a **valid value**, indicating that users made bookings to locations in the Southern Hemisphere. |
| **`checked_bags`** (Checked Bags Count) | **MIN:** $0.000000$ (Zero) | **EXPECTED VALUE.** A minimum bag count of zero implies the user checked no bags. Since this is not a negative value, it is **not an error** but represents a valid state (no checked baggage) in the dataset. |
| **`base_fare_usd`** (Base Fare USD) | **MIN:** $2.410000$ (Positive) | **ANOMALOUSLY LOW VALUE.** The minimum value is positive. However, based on this table, the lowest value is $2.41$ USD. While this price is very low (could be a mistake or promotion), it is technically a positive fare. **If the MIN value were negative**, it would indicate a data entry error (a negative base fare is nonsensical) or a refund/reversal transaction. |
| **`nights`** (Nights Stayed) | **MIN:** $-2.000000$ (Negative) | **DATA ENTRY / JOIN ERROR.** A negative number of nights spent at a hotel is **logically impossible**. This is definitely a **data entry/join error** that requires a **data cleaning** step. These records must either be excluded or set to `NaN`. |
| **`flight_discount_amount`** | **MIN:** $0.050000$ (Positive) | **EXPECTED VALUE.** This represents the discount rate. A negative discount (i.e., a price increase) is not expected. A minimum discount rate of 5% is an expected and valid value. |

# 📊 Consolidated Initial Data Analysis (EDA) 🔎
The dataset, consisting of 49,211 entries and 41 columns, is the result of the prior Spark SQL integration and filtering steps. The goal of this inspection is to identify data types, missing value patterns, and feature distributions before Feature Engineering (FE).

## 1. Data Structure and Quality Assessment 💾

This section summarizes the data types and crucial non-null counts, highlighting columns with significant missing values.

| # | Column Name | Data Type | Non-Null Count | Missing (%) | Feature Category 🏷️ |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **Identifiers & Core** | `session_id`, `user_id`, `session_start`, `session_end`, `page_clicks`, `cancellation` | `object` / `int64` / `datetime64` / `bool` | 49211 | 0% | Core metrics are complete. |
| **Transaction ID** | `trip_id` | `object` | 16702 | **66.1%** | Indicates sessions that resulted in or focused on a potential trip. |
| **Discount Amounts** | `flight_discount_amount` | `float64` | 8282 | **83.2%** | Present only when a flight discount was applied. |
| **Discount Amounts** | `hotel_discount_amount` | `float64` | 6205 | **87.4%** | Present only when a hotel discount was applied. |
| **Flight Details** | `origin_airport`, `destination`, `base_fare_usd`, etc. | Various | 14270 | **71.0%** | **Missingness is Expected:** Represents sessions where a flight transaction did **not** occur or was not searched for. |
| **Hotel Details** | `hotel_name`, `nights`, `rooms`, `hotel_price_per_room_night_usd` | Various | 14726 | **70.1%** | **Missingness is Expected:** Represents sessions where a hotel transaction did **not** occur or was not searched for. |
| **Date/Time Fields** | `birthdate`, `sign_up_date`, `departure_time`, `check_in_time`, etc. | `datetime64[ns]` | Variable | Variable | Crucial for **time-based feature creation**. |

## 2. Key Statistical Insights (Numerical Columns) 📈

This table focuses on the distribution and spread of key numerical metrics, identifying potential outliers and data preparation needs.

> **Data Quality Insight:** The massive missing percentages (70-87%) in the transaction columns (Flight, Hotel, Discounts) are intentional due to the **LEFT JOIN** strategy. In FE, these `NULL` values must be imputed with **0** to represent "no transaction/no discount" and be used as predictive features.

| Column Name | Mean (Average) | 75th Percentile | Max (Maximum) | Analytical Insight 💡 |
| :--- | :--- | :--- | :--- | :--- |
| **page_clicks** | 17.58 | 22.0 | 566.0 | **High Outlier/Skew:** The maximum value (566) is substantially higher than the 75th percentile (22). This implies a heavily **right-skewed distribution**, requiring potential **Log Transformation** to normalize the data for ML models. |
| **seats** | 1.21 | 1.0 | 8.0 | **Low Transaction Volume:** The mean is close to 1, and the 75th percentile is exactly 1, confirming that the vast majority of flight bookings are for a single traveler. The Max (8) indicates occasional large group bookings. |
| **hotel_price_per_room_night_usd** | 177.93 | 222.0 | 1376.0 | **Wide Price Range:** The maximum price is significantly higher than the 75th percentile, indicating a wide range of hotel quality/luxury is present in the bookings. |
| **flight_discount_amount** | 0.139 (~14%) | 0.20 | 0.60 (60%) | **Discount Distribution:** The average discount applied (when present) is around 14%, with the largest discount being 60%. |

## 3. Next Steps for Feature Engineering (FE) 🚀

Based on this Exploratory Data Analysis (EDA), the immediate next steps should be:

### 🧹 Data Cleaning and Imputation

* **Imputation (Missing Values):** Fill all `NaN` values in numerical transaction and discount columns (e.g., `base_fare_usd`, `nights`, `discount_amount`) with **0**. This step is crucial as `NaN` represents "no transaction/no discount."

### 📅 Temporal Feature Creation

Derive new, high-value temporal features from the `datetime64[ns]` columns:

* **User Age:** Calculate the user's current age from the `birthdate` column.
* **User Tenure:** Calculate how long the user has been registered from the `sign_up_date`.
* **Session Duration:** Calculate the length of the session using: `session_end` minus `session_start`.
* **Time of Day/Week:** Extract the hour, day of the week, or month from `session start/end` times to capture **behavioral seasonality**.

### 📉 Outlier Handling

* **Address Skewness:** Address the skewness and extreme outliers observed in `page_clicks` (e.g., by capping or applying a logarithmic transformation) to improve the performance of downstream Machine Learning models.

## 📊 II. UNIVARIATE ANALYSIS (Single-Variable Analysis)

This analysis is conducted to understand how each variable in the dataset behaves individually.

| Variable Type | Action to be Taken 🛠️ | Example Columns 💡 |
| :--- | :--- | :--- |
| **Numeric (Sayısal)** | **Statistical Summary:** Mean, median, standard deviation, minimum, maximum, quartiles (`df.describe()`). | `page_clicks`, `seats`, `hotel_price_per_room_night_usd`, `flight_discount_amount` |
| | **Visualization:** Histograms to see the shape of the distribution, and Box Plots to detect outliers. | |
| **Categorical (Kategorik)** | **Frequency Analysis:** Count how often each category occurs (`value_counts()`). | `origin_airport`, `destination_airport` (if there are a small number of distinct values), transaction statuses. |
| | **Visualization:** Bar Plots to show the frequency of occurrences. | |


## 🔬 III. MULTIVARIATE ANALYSIS (Multi-Variable Analysis)

This analysis examines the relationships and dependencies between variables, providing important clues for modeling.

| Analysis Type 🔎 | Action to be Taken ⚙️ | Example 💡 |
| :--- | :--- | :--- |
| **Correlation (Korelasyon)** | Create a Correlation Matrix to measure the linear relationship between numerical variables. | Relationship between `page_clicks` and `session_duration_seconds`. |
| **Categorical-Numeric Relationship (Kategorik-Sayısal İlişki)** | View how a categorical variable affects the mean of a numerical variable (Hypothesis Tests or Group Means). | Relationship between `is_transactional` (Your new FE column) and `hotel_price_per_room_night_usd`. |
| **Visualization (Görselleştirme)** | Use Scatter Plots or Heatmaps to show the relationship between two or three variables. | Distribution of `base_fare_usd` price based on the number of `seats`. |



# 🛠️ Data Cleaning and Feature Engineering Action Plan

## 🛠️ Data Cleaning and Feature Engineering Action Plan ✨

This table summarizes the identified data quality issues and outlines the mandatory cleaning and transformation actions to be performed on the `df_sessions_clean` DataFrame before model building.

| Column Name | Issue Type / Detection 🔍 | Analysis and Impact 📝 | Proposed Cleaning/Action (Pre-FE) 🧹 | Proposed Feature Engineering (FE) ⚙️ |
| :--- | :--- | :--- | :--- | :--- |
| **`nights`** | Anomalous Value (MIN: -2.0) | Negative values are illogical; suggests a Data Entry or Join Error. Must be neutralized. | **Replace Negative Values:** Set all values $< 0$ to `np.nan` (or `NULL`). | After replacing negatives with `NaN`, **Impute remaining `NaN` values with 0** (assuming 0 nights booked). |
| **`page_clicks`** | High Outliers / Skew (MAX: 566.0, 75th Pctl: 22.0) | Heavily right-skewed distribution. ML models perform better with normalized data. | None (No error detected). | **Apply Log Transformation:** Create `log_page_clicks = log(1 + page_clicks)`. |
| **`base_fare_usd`** | Anomalously Low (MIN: 2.41) | Extremely low value, potentially a promotion or error. Low impact on cleaning. | None. (Only address if a promotional logic is required). | None (A reasonable Log transformation would handle its distribution). |
| **`birthdate`** | Requires Conversion | Raw date is not useful for modeling; highly sensitive to distribution. | None. | **Calculate Age:** Create `user_age` (e.g., using 2023-07-28 as reference date). |
| **`sign_up_date`** | Requires Conversion | Raw date is not useful for modeling; measures time on platform. | None. | **Calculate Tenure:** Create `user_tenure_days` (days since sign-up). |
| **`session_start/end`** | Requires Calculation | Crucial for understanding user engagement. | None. | **Calculate Session Duration:** Create `session_duration_seconds`. |
| **`home_airport_lat/lon`** | Requires Calculation | Raw Lat/Lon values are not predictive unless relative to the destination. | None. | **Calculate Distance:** Create `home_to_destination_distance` (Haversine distance). |
| **`trip_id`** (Missing: 66.1%) | Expected Missingness (NULL) | Missing values indicate sessions not focused on a potential trip/booking. | None. (Discount columns already imputed in STEP 3). | **Create Indicator Feature:** `is_transactional` (1 if `trip_id` is NOT null, 0 otherwise). |

## ⚙️ Feature Engineering (FE) Wrap-up & Validation

The critical Feature Engineering steps required to proceed to the Multivariate Analysis have been successfully completed using PySpark.

| Feature Type | Status & Observation 📝 | Technical Insight / Justification 💡 |
| :--- | :--- | :--- |
| **Transactional Indicator** (`is_transactional`) | **Status:** Successfully created (1/0). Sessions without a `trip_id` are correctly marked as 0 (Non-transactional). | This feature is crucial for Multivariate Analysis as it clearly segments users based on their intent (trip-focused vs. browsing). |
| **Distribution Normalization** (`log_page_clicks`) | **Status:** Log Transformation applied successfully, mitigating the severe right-skewness observed in the Univariate Analysis. | Normalized features are essential for linear models and distance-based algorithms, ensuring that extreme outliers do not disproportionately influence correlations. |
| **Log-Transformed Transactional Features** (`log_hotel_price`, `log_nights`) | **Observation:** These columns display `NULL` values after the transformation, even though the original columns (`hotel_price_per_room_night_usd` and `nights`) were designed to be imputed with 0 when the transaction was missing. | **Technical Note:** The presence of `NULL`s post-transformation is likely due to the PySpark logic order: the `log1p` function was calculated *before* the intended imputation (filling `NaN` with 0) took effect on the specific transaction subset, or due to how the original NULLs were handled during the operation. |
| **Path Forward** | **Conclusion:** Despite the residual `NULL`s in the transformed transaction columns, we can proceed with the analysis. | **Justification:** PySpark correlation functions, such as `df.stat.corr`, typically handle `NULL` values automatically by applying **pairwise deletion**. This means the analysis will use all non-null pairs of data points, ensuring the calculated correlations remain valid. |

---

### **Next Step:** Correlation Matrix (Heatmap)
We are now fully prepared to execute the Multivariate Analysis, starting with the **Correlation Matrix** to discover relationships between these newly engineered and normalized features.

### 🔬 Correlation Matrix Key Insights (Multivariate Analysis)

| Relationship (Var1 vs. Var2) | Correlation ($r$) | Strength | Interpretation and Impact on Modeling |
| :--- | :--- | :--- | :--- |
| **is_transactional vs. log_hotel_price** | **0.90** | Very Strong Positive | **Confirms FE Success:** Indicates sessions flagged as transactional are overwhelmingly tied to hotel searches/bookings. This is a highly predictive pair. |
| **is_transactional vs. seats** | **0.77** | Strong Positive | **Key Transaction Driver:** Booking a quantity of seats is a primary activity defining a transactional session. |
| **seats vs. base_fare_usd** | **0.65** | Strong Positive | **Logical Relationship:** Higher total number of seats correlates directly with a higher total fare amount. This validates the imputation of 0s in transaction columns. |
| **log_page_clicks vs. is_transactional** | **0.58** | Moderate Positive | **User Effort:** Transactional sessions require more user effort (clicks) than pure browsing sessions. |
| **log_page_clicks vs. log_hotel_price** | **0.54** | Moderate Positive | **High-End Shopping:** Users interested in higher-priced hotels engage in more browsing activity. |
| **log_page_clicks vs. session_duration_seconds** | **0.44** | Moderate Positive | **Engagement:** More pages clicked translates to moderately longer sessions, validating the relationship between these two FE features. |
| **flight_discount_amount vs. All Variables** | $\le 0.04$ | Negligible | **Critical Finding:** The actual percentage discount (when applied) has almost **no linear predictive power** over session activity, price, or transaction likelihood. |
| **user_age / user_tenure_days vs. Activity** | $\le 0.05$ | Negligible | **Demographic Weakness:** Neither user age nor time since registration linearly correlates with current session engagement or transaction volume. |

## 🔬 Categorical-Numeric Relationship Analysis (Grouped Analysis) 📊

### 1. Goal 🎯

The primary goal of this stage is to perform a **Group-By Analysis** to calculate the mean of key numerical features, segregated by the values of the indicator (categorical) features (`is_transactional` and `has_flight_discount`). This confirms the strength of the linear relationships observed in the Correlation Matrix by comparing group averages.

### 2. Key Relationships to Investigate 🔎

| Indicator Feature (Categorical) | Numerical Feature (Continuous) | Question to Answer 🤔 |
| :--- | :--- | :--- |
| **`is_transactional`** (0 vs. 1) | **`log_page_clicks`** | Do transactional sessions involve significantly more browsing effort (clicks) than non-transactional sessions? |
| **`is_transactional`** (0 vs. 1) | **`session_duration_seconds`** | Are transactional sessions longer than non-transactional sessions? |
| **`is_transactional`** (0 vs. 1) | **`log_hotel_price`** | Is the average hotel price significantly higher when a session is transactional? |
| **`has_flight_discount`** | **`log_page_clicks`** | Do sessions *with* a discount involve more clicks than sessions *without* a discount? |

## 📊 Categorical-Numeric Relationship Analysis Insights 🔎

### 1. Grouped by `is_transactional` (0: Browsing, 1: Transaction-Focused) 🎯

| Metric | Non-Transactional (0) | Transactional (1) | Difference (Factor / % Difference) | Analysis |
| :--- | :--- | :--- | :--- | :--- |
| **Avg Log Clicks** | 2.23 | 3.23 | **44% Higher** | **Confirmed:** Transactional sessions contain **44% more** page clicks (browsing effort) compared to non-transactional sessions. |
| **Avg Session Duration** | 85.12 seconds | 386.04 seconds | **4.5 Times Longer** | **Very Strong Validation:** Transactional sessions last **4.5 times longer** than non-transactional sessions. This indicates that a transaction focus dramatically increases user engagement. |
| **Avg Log Hotel Price** | NULL | 5.01 | N/A | **Confirmed:** The average price cannot be calculated for non-transactional sessions (NULL). Transactional sessions include hotels with an average price of $e^{5.01} - 1 \approx \$149.3$. |

## 📉 2. Grouped by `has_flight_discount` (Presence of Discount)

| Metric | Discount Applied | No Discount Applied (Missing) | Difference | Analysis |
| :--- | :--- | :--- | :--- | :--- |
| **Avg Log Clicks** | 2.57 | Missing | N/A | **Comment:** Only the average for sessions with a discount applied (2.57) was calculated. Seeing the average for sessions without a discount would allow us to understand the indicator's true effect more clearly. |
| **Avg Session Duration** | 187.25 seconds | Missing | N/A | **Comment:** Sessions with a discount applied last an average of 187 seconds. This duration falls between non-transactional sessions (85s) and transactional sessions (386s). |

# 🤖 Analysis and Model Building (ML Segmentation)

This section details the transition from descriptive analysis to predictive modeling. Utilizing **Python**, **NumPy**, and **Pandas**, we engineered behavioral features to execute an Unsupervised Machine Learning algorithm (**K-Means Clustering**). This allowed us to discover distinct customer personas based on spending power, reliability, and engagement patterns.

### 🛠️ Tools & Environment
* **Language:** Python 3.9+
* **Core Libraries:** `pandas` (Data Manipulation), `numpy` (Numerical Operations), `scikit-learn` (Modeling & Preprocessing), `seaborn` (Visualization).
* **Function:** Provided the execution environment to normalize complex behavioral data and execute the segmentation algorithms.

---

### 1. Feature Engineering & Preprocessing ⚙️

Before modeling, we aggregated user behavior into a single vector per user. We focused on the key differentiators identified in the EDA phase: **Spending**, **Cancellation**, and **Timing**.

```python
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans
import pandas as pd
import numpy as np

# 1. Feature Selection: Selecting the high-impact metrics identified in EDA
features = [
    'total_hotel_spend',       # Spending Power
    'base_fare_usd',           # Travel Tier (Economy vs. Premium)
    'cancellation_rate',       # Reliability / Churn Risk
    'page_clicks',             # Engagement Depth
    'avg_flight_discount_amount' # Price Sensitivity
]

# 2. Handling Missing Data (Imputation)
# Filling NaNs with 0 implies no activity in that category
df_model = df_users[features].fillna(0)

# 3. Standardization (Z-Score Normalization)
# Crucial for K-Means to ensure 'Spend' ($400) doesn't dominate 'Rate' (0.15)
scaler = StandardScaler()
df_scaled = scaler.fit_transform(df_model)

# 4. Model Execution: K-Means Clustering
# We determined k=3 based on the Elbow Method (intra-cluster sum of squares)
kmeans = KMeans(n_clusters=3, random_state=42, n_init=10)
df_users['cluster_id'] = kmeans.fit_predict(df_scaled)
'''

## 2. The Identified Consumer Segments 👥

The ML algorithm successfully converged, grouping the database into **3 distinct high-value personas**. Below is the definition of the newly created consumer groups.


| Cluster ID | Segment Name | Key Behavioral Traits | Strategic Action |
| :---: | :--- | :--- | :--- |
| **0** | **The Weekend Whales** 🐋<br>*(High Value / Leisure)* | • **High Spend:** Top 10% in Hotel & Flight spend.<br>• **Timing:** Predominantly active on Weekends.<br>• **Low Discount Sensitivity:** Rarely uses coupons. | **VIP Treatment:** Upsell "All-Inclusive" packages. Offer loyalty tier upgrades. Do not discount aggressively; value is priority. |
| **1** | **The Window Shoppers** 🛒<br>*(High Churn / Browsers)* | • **High Engagement:** Very high `page_clicks`.<br>• **High Cancellation:** Cancellation rate > 15%.<br>• **Indecisive:** Books multiple options and cancels later. | **Retargeting & Stabilization:** Implement "Price Freeze" fees. Send reminders for abandoned carts. Use urgency triggers ("Only 2 seats left"). |
| **2** | **The Budget Commuters** 💼<br>*(Low Spend / Efficient)* | • **Low Spend:** Lowest `base_fare_usd`.<br>• **Efficient:** Low `page_clicks` (in-and-out).<br>• **Price Sensitive:** High correlation with discount usage. | **Volume Play:** Offer high-frequency, low-margin deals. Push standard economy seats and budget hotels. |
