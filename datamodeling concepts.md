# Data Modeling Interview Topics

> Ordered by priority for Senior Data Engineer interview preparation

---

## Tier 1 - Must Know (Focus First)

### Core Dimensional Modeling
- ✅ **dimensional modeling** - Design methodology for data warehouses using facts (measures) and dimensions (context). Optimized for read-heavy analytics. [→ Details](#dimensional-modeling)
- ✅ **star schema** - Central fact table surrounded by denormalized dimension tables. Simple queries, fast performance, BI-friendly. [→ Details](#star-schema)
- ✅ **snowflake schema** - Normalized dimension tables with hierarchies. Less redundancy, more joins, slower queries. [→ Details](#snowflake-schema)
- ✅ **fact tables** - Store quantitative business events (sales, clicks, transactions). Contains FKs to dimensions + measures. [→ Details](#fact-tables)
- ✅ **additive vs non-additive facts** - Additive: SUM across all dimensions (revenue). Non-additive: Cannot SUM (ratios, percentages). [→ Details](#additive-vs-non-additive-facts)
- ✅ **semi-additive facts** - SUM across some dimensions only. Example: account balance (SUM across accounts, not time). [→ Details](#semi-additive-facts)
- ✅ **factless fact tables** - Fact table with no measures. Row existence = event occurred. Example: student attendance, driver availability. [→ Details](#factless-fact-tables)

### Slowly Changing Dimensions
- ✅ **slowly changing dimensions** - Dimension attributes that change over time (customer address, product price). Requires history tracking strategy. [→ Details](#slowly-changing-dimensions)
- ✅ **SCD types** - Different strategies to handle changes: Type 0 (retain original), Type 1 (overwrite), Type 2 (history rows), Type 3 (previous column), Type 4 (mini-dimension), Type 6 (hybrid 1+2+3). [→ Details](#scd-types)
- ✅ **type 0, type 1, type 2, type 3, type 4, type 6** - Type 2 most common (valid_from, valid_to, is_current). Type 6 for "current + history" needs. [→ Details](#scd-types)

### Foundational Concepts
- ✅ **data warehousing** - Centralized repository for integrated data from multiple sources, optimized for analytics. Subject-oriented, time-variant, non-volatile. [→ Details](#data-warehousing)
- ✅ **data marts** - Subset of data warehouse for specific business area (Sales Mart, Finance Mart). Faster to build, department-focused. [→ Details](#data-marts)
- ✅ **OLTP vs OLAP** - OLTP: Transactional, normalized, row-oriented (PostgreSQL). OLAP: Analytical, denormalized, columnar (Snowflake, Databricks). [→ Details](#oltp-vs-olap)
- ✅ **normalization vs denormalization** - Normalize (3NF) to reduce redundancy in OLTP. Denormalize for query performance in OLAP. [→ Details](#normalization-vs-denormalization)
- ✅ **surrogate keys** - System-generated keys (auto-increment, hash) vs natural business keys. Enable SCD tracking, join performance. [→ Details](#surrogate-keys)
- ✅ **conformed dimensions** - Shared dimensions across multiple fact tables/marts. Ensures consistent analysis (same dim_date, dim_customer everywhere). [→ Details](#conformed-dimensions)
- ✅ **entity-relationship modeling** - Design technique using entities, attributes, relationships. Crow's foot notation. For OLTP/source systems. [→ Details](#entity-relationship-modeling)

### Modern Architecture (Critical for Senior Role)
- ✅ **medallion architecture (bronze/silver/gold layers)** - Lakehouse pattern: Bronze (raw), Silver (cleaned/conformed), Gold (business-ready/aggregated). [→ Details](#medallion-architecture)
- ✅ **partitioning and bucketing strategies** - Partition by date/region for filter pruning. Bucket by join keys (customer_id) for shuffle-free joins. [→ Details](#partitioning-and-bucketing)
- ✅ **schema evolution and schema registry** - Handle schema changes (add columns, type widening). Registry for Kafka/streaming compatibility enforcement. [→ Details](#schema-evolution-and-registry)

---

## Tier 2 - Senior Level Differentiators

### Advanced Dimensional Modeling
- ✅ **aggregate tables** - Pre-computed summaries at higher grain (daily→monthly). Speeds up dashboards by 100x. [→ Details](#aggregate-tables)
- ✅ **cumulative tables** - Track running totals over time (lifetime orders, YTD revenue). One row per entity, accumulates metrics. [→ Details](#cumulative-tables)
- ✅ **accumulating snapshots** - Track process lifecycle with milestone timestamps (order→ship→deliver). Measures are lag times between milestones. [→ Details](#accumulating-snapshots)
- ✅ **bridge tables** - Resolve M:N relationships between fact and dimension (customer↔multiple addresses). Contains weighting factors. [→ Details](#bridge-tables)
- ✅ **role-playing dimensions** - Same dimension used multiple ways in one fact (order_date, ship_date, delivery_date all → dim_date). [→ Details](#role-playing-dimensions)
- ✅ **junk dimensions** - Combine low-cardinality flags (is_gift, payment_type) into single dimension. Reduces fact table width. [→ Details](#junk-dimensions)
- ✅ **degenerate dimensions** - Dimension attribute stored in fact table (order_id, invoice_number). No separate dimension table needed. [→ Details](#degenerate-dimensions)
- ✅ **outrigger dimensions** - Secondary dimension hanging off another dimension (dim_geography → dim_customer). Shared reference data. [→ Details](#outrigger-dimensions)

### Data Vault & Advanced Patterns
- ✅ **data vault modeling** - Enterprise DW methodology using Hubs (business keys), Links (relationships), Satellites (attributes+history). Agile, auditable, scalable. [→ Details](#data-vault-modeling)
- ✅ **idempotent data processing** - Running same operation multiple times = same result. Patterns: DELETE+INSERT, MERGE, partition replacement. Critical for retry safety. [→ Details](#idempotent-data-processing)
- ✅ **temporal cardinality explosion** - Joining SCD Type 2 without date constraints creates massive row multiplication. Always use `fact.date BETWEEN dim.valid_from AND dim.valid_to`. [→ Details](#temporal-cardinality-explosion)

### Design Tradeoffs
- ✅ **knowing your data consumers** - Design for your audience: analysts need simple star schema, data scientists need wide tables, engineers need modular normalized. [→ Details](#knowing-your-data-consumers)
- ✅ **compactness vs usability tradeoffs** - Codes (TINYINT) save space but need lookups. Strings are self-documenting. Use strings in Gold layer. [→ Details](#compactness-vs-usability)
- ✅ **wide vs narrow tables tradeoffs** - Narrow (normalized): flexible, more joins. Wide (denormalized): simple queries, columnar-friendly. [→ Details](#wide-vs-narrow-tables)
- ✅ **one big table (OBT) pattern** - Single denormalized table with all facts+dimensions. Zero joins, fastest queries, but harder to maintain. [→ Details](#one-big-table-pattern)

### Data Models
- **logical vs physical data models** - Logical: platform-independent design with attributes and keys. Physical: implementation-specific with exact types, indexes, partitions. [→ Details](#logical-vs-physical-data-models)
- **conceptual data models** - High-level business view with entities and relationships only. No attributes. For stakeholder communication. [→ Details](#conceptual-data-models)

---

## Tier 3 - Advanced/Specialized

### Specialized Modeling Patterns
- graph data modeling
- hierarchical data modeling
- data modeling for real-time/streaming
- modeling for ML feature stores
- activity schema / event modeling

### Technical Optimizations
- run length encoding (RLE) compression
- datelist data structure
- power of enums
- little book of enums

### Fact Table Concepts
- facts
- reduced facts
- additive vs non additive measures

### Cloud-Native Patterns
- data modeling for cloud data warehouses (Snowflake, BigQuery, Redshift, Databricks)

---

## Tier 4 - Analytics Patterns

### Business Analytics Modeling
- growth accounting
- survival analysis patterns
- customer lifetime value (CLV)
- churn analysis
- cohort analysis

### Analytics Types
- predictive analytics
- prescriptive analytics
- descriptive analytics
- diagnostic analytics

---

## Tier 5 - Nice to Have

### Alternative Methodologies
- anchor modeling
- data modeling best practices

---

## Interview Tips

- Be ready to **design a schema on whiteboard** (e.g., "Design a data model for an e-commerce platform")
- Know **tradeoffs** - when to use star vs snowflake, normalized vs denormalized
- Understand **why** behind each concept, not just definitions
- Prepare examples from **your experience** for each major concept
- Focus on Tier 1 & 2 first - these cover 80% of interview questions

---

## 📚 Completed Examples & Deep Dives

### Example 1: Uber Ride Analytics - Complete Dimensional Model

**Business Questions:**
- What's the total revenue by city, day, hour?
- What's the average trip duration and distance?
- How does surge pricing affect demand?
- How long does it take from request to pickup?

#### Star Schema Overview

```
                                    ┌─────────────────┐
                                    │   dim_rider     │
                                    │ (SCD Type 2)    │
                                    ├─────────────────┤
                                    │ rider_key (SK)  │
                                    │ rider_id        │
                                    │ rider_tier      │
                    ┌───────────────│ home_city       │
                    │               └─────────────────┘
                    │
                    │               ┌─────────────────┐
                    │               │   dim_driver    │
                    │               │ (SCD Type 2)    │
                    │               ├─────────────────┤
                    │               │ driver_key (SK) │
                    │               │ driver_tier     │
                    ├───────────────│ avg_rating      │
                    │               └─────────────────┘
                    │
                    ▼               ┌─────────────────┐
┌─────────────────────────────────┐ │  dim_location   │
│         fact_ride               │ │ (Role-Playing)  │
├─────────────────────────────────┤ ├─────────────────┤
│ ride_key (SK)                   │ │ location_key    │
│ ride_id (Degenerate Dim)        │ │ city, zone      │
│                                 │ │ neighborhood    │
│ rider_key (FK)                  │ │ is_airport      │
│ driver_key (FK)                 │ └─────────────────┘
│ vehicle_key (FK)                │
│ pickup_location_key (FK)        │ ┌─────────────────┐
│ dropoff_location_key (FK)       │ │  dim_date       │
│ ride_type_key (FK)              │ │ (Role-Playing)  │
│ request_date_key (FK)           │ ├─────────────────┤
│ pickup_date_key (FK)            │ │ date_key        │
│ dropoff_date_key (FK)           │ │ day_of_week     │
│                                 │ │ is_weekend      │
│ ── ADDITIVE MEASURES ──         │ │ is_holiday      │
│ fare_amount                     │ └─────────────────┘
│ surge_amount                    │
│ tip_amount                      │ ┌─────────────────┐
│ total_amount                    │ │  dim_time       │
│ distance_miles                  │ │ (Role-Playing)  │
│ duration_minutes                │ ├─────────────────┤
│                                 │ │ time_key        │
│ ── NON-ADDITIVE MEASURES ──     │ │ hour_24         │
│ surge_multiplier (use AVG)      │ │ is_rush_hour    │
│ driver_rating_given (use AVG)   │ │ time_of_day     │
│ rider_rating_given (use AVG)    │ └─────────────────┘
└─────────────────────────────────┘
```

#### Key Table Definitions

**fact_ride (Transaction Fact)**
```sql
CREATE TABLE fact_ride (
    ride_key                    BIGINT PRIMARY KEY,
    ride_id                     STRING,          -- Degenerate dimension
    rider_key                   BIGINT,          -- FK
    driver_key                  BIGINT,          -- FK
    vehicle_key                 BIGINT,          -- FK
    pickup_location_key         BIGINT,          -- Role-playing dim
    dropoff_location_key        BIGINT,          -- Role-playing dim
    request_date_key            INT,             -- Role-playing dim
    pickup_date_key             INT,             -- Role-playing dim
    
    -- ADDITIVE MEASURES
    fare_amount                 DECIMAL(10,2),
    surge_amount                DECIMAL(10,2),
    tip_amount                  DECIMAL(10,2),
    total_amount                DECIMAL(10,2),
    distance_miles              DECIMAL(8,2),
    duration_minutes            INT,
    
    -- NON-ADDITIVE MEASURES
    surge_multiplier            DECIMAL(3,2),
    driver_rating_given         DECIMAL(2,1),
    rider_rating_given          DECIMAL(2,1)
);
```

**fact_ride_lifecycle (Accumulating Snapshot)**
```sql
CREATE TABLE fact_ride_lifecycle (
    ride_key                    BIGINT PRIMARY KEY,
    
    -- MILESTONE TIMESTAMPS (NULL until reached)
    request_datetime            TIMESTAMP,
    driver_assigned_datetime    TIMESTAMP,
    driver_arrived_datetime     TIMESTAMP,
    trip_started_datetime       TIMESTAMP,
    trip_completed_datetime     TIMESTAMP,
    
    -- LAG MEASURES (calculated as milestones hit)
    request_to_match_seconds    INT,
    match_to_arrival_minutes    INT,
    trip_duration_minutes       INT,
    total_lifecycle_minutes     INT,
    
    current_status              STRING  -- requested/matched/in_progress/completed
);
```

**fact_driver_availability (Factless Fact)**
```sql
CREATE TABLE fact_driver_availability (
    date_key            INT,
    time_key            INT,
    driver_key          BIGINT,
    location_key        BIGINT,
    -- NO MEASURES! Row existence = driver was available
    PRIMARY KEY (date_key, time_key, driver_key)
);
```

**bridge_driver_vehicle (Bridge Table)**
```sql
CREATE TABLE bridge_driver_vehicle (
    driver_key          BIGINT,
    vehicle_key         BIGINT,
    is_primary_vehicle  BOOLEAN,
    effective_from      DATE,
    effective_to        DATE
);
```

#### Concepts Applied in This Example

| Concept | Where Used |
|---------|------------|
| **Star Schema** | fact_ride at center with dimensions |
| **Surrogate Keys** | All `*_key` columns |
| **Degenerate Dimension** | ride_id in fact table |
| **Role-Playing Dimensions** | dim_location (pickup/dropoff), dim_date, dim_time |
| **SCD Type 2** | dim_rider, dim_driver (tier changes over time) |
| **Conformed Dimensions** | dim_location, dim_date shared across facts |
| **Additive Facts** | fare, distance, duration |
| **Non-Additive Facts** | surge_multiplier, ratings |
| **Factless Fact** | fact_driver_availability |
| **Accumulating Snapshot** | fact_ride_lifecycle |
| **Bridge Table** | bridge_driver_vehicle |

#### Sample Interview Queries

```sql
-- Revenue by City and Ride Type
SELECT pl.city, rt.ride_type_name,
       SUM(f.total_amount) as revenue,
       AVG(f.surge_multiplier) as avg_surge
FROM fact_ride f
JOIN dim_location pl ON f.pickup_location_key = pl.location_key
JOIN dim_ride_type rt ON f.ride_type_key = rt.ride_type_key
GROUP BY pl.city, rt.ride_type_name;

-- Wait Time Analysis (Accumulating Snapshot)
SELECT pl.neighborhood,
       AVG(fl.request_to_match_seconds) as avg_match_time,
       AVG(fl.match_to_arrival_minutes) as avg_eta
FROM fact_ride_lifecycle fl
JOIN fact_ride f ON fl.ride_key = f.ride_key
JOIN dim_location pl ON f.pickup_location_key = pl.location_key
WHERE fl.current_status = 'completed'
GROUP BY pl.neighborhood;

-- Driver Availability vs Actual Rides (Factless Fact)
SELECT COUNT(DISTINCT da.driver_key) as available_drivers,
       COUNT(DISTINCT fr.driver_key) as drivers_with_rides
FROM fact_driver_availability da
LEFT JOIN fact_ride fr ON da.driver_key = fr.driver_key
    AND da.date_key = fr.pickup_date_key
JOIN dim_location l ON da.location_key = l.location_key
WHERE l.is_downtown = TRUE;
```

---

### Example 2: Star Schema vs Snowflake Schema

#### Visual Comparison

**Star Schema (Denormalized):**
```
                         dim_product (FLAT)
                         ┌─────────────────────┐
                         │ product_key         │
                         │ product_name        │
                         │ brand_name          │◄── embedded
                         │ category_name       │◄── embedded
                         │ department_name     │◄── embedded
                         └──────────┬──────────┘
                                    │
┌─────────────┐          ┌──────────┴──────────┐          ┌─────────────┐
│ dim_store   │──────────│     fact_sales      │──────────│ dim_customer│
└─────────────┘          └─────────────────────┘          └─────────────┘
                                    │
                         ┌──────────┴──────────┐
                         │     dim_date        │
                         └─────────────────────┘
```

**Snowflake Schema (Normalized):**
```
dim_brand ──► dim_product ──► dim_subcategory ──► dim_category ──► dim_department
                   │
                   ▼
              fact_sales
```

#### Key Differences

| Aspect | Star Schema | Snowflake Schema |
|--------|-------------|------------------|
| **Dimensions** | Denormalized (flat) | Normalized (multiple tables) |
| **Joins** | 1 join to dimension | Multiple joins through hierarchy |
| **Query Speed** | Faster | Slower |
| **Storage** | More (duplication) | Less |
| **BI Tools** | Preferred | Supported |

#### Query Comparison

**Star Schema (1 join):**
```sql
SELECT p.brand_name, p.category_name, SUM(f.amount)
FROM fact_sales f
JOIN dim_product p ON f.product_key = p.product_key
GROUP BY p.brand_name, p.category_name;
```

**Snowflake (4 joins):**
```sql
SELECT b.brand_name, c.category_name, SUM(f.amount)
FROM fact_sales f
JOIN dim_product p ON f.product_key = p.product_key
JOIN dim_brand b ON p.brand_key = b.brand_key
JOIN dim_subcategory sc ON p.subcategory_key = sc.subcategory_key
JOIN dim_category c ON sc.category_key = c.category_key
GROUP BY b.brand_name, c.category_name;
```

#### When to Use Each

**Use Star Schema:**
- Query performance is critical
- BI tools (Tableau, Power BI)
- Business users write queries
- Storage cost not a concern

**Use Snowflake Schema:**
- Very large dimensions with sparse attributes
- Many-to-many relationships
- Strict data integrity requirements
- Dimensions update frequently

#### Denormalization Truth

**You DON'T lose data** - you duplicate it!

| Risk | Mitigation |
|------|------------|
| Update anomalies | Full dimension reload in ETL |
| Inconsistency | Single ETL source of truth |
| Storage | Modern cloud storage is cheap |

**Interview Answer:**
> "I default to star schema for modern data warehouses because query performance and BI tool compatibility outweigh storage costs. I use snowflake only for very large, sparse dimension attributes or many-to-many relationships."

---

# 📖 Detailed Concept Reference

> Full explanations for each concept discussed. Click the ↑ Back link to return to the topic list.

---

## Tier 1: Core Dimensional Modeling

### Dimensional Modeling

**Definition:** A design methodology for data warehouses that organizes data into **facts** (measurable events) and **dimensions** (descriptive context). Pioneered by Ralph Kimball as the foundation of the "bottom-up" data warehouse approach.

**Key Components:**
- **Fact Tables:** Store quantitative business metrics (revenue, quantity, clicks)
- **Dimension Tables:** Store descriptive attributes (customer name, product category, date)
- **Grain:** The level of detail in the fact table (one row per transaction, per day, per customer)

---

**Example 1: E-Commerce Sales**
```sql
-- Fact table: What happened (measures)
CREATE TABLE fact_sales (
    sale_key        BIGINT PRIMARY KEY,
    date_key        INT,
    customer_key    BIGINT,
    product_key     BIGINT,
    store_key       INT,
    quantity        INT,
    unit_price      DECIMAL(10,2),
    discount_amt    DECIMAL(10,2),
    total_amount    DECIMAL(12,2)
);

-- Dimension tables: Who, What, When, Where
dim_customer: customer_key, name, email, segment, city, state
dim_product: product_key, name, category, brand, supplier
dim_date: date_key, date, day_of_week, month, quarter, year, is_holiday
dim_store: store_key, store_name, region, manager
```

**Example 2: Healthcare Visits**
```sql
-- Grain: One row per patient visit
CREATE TABLE fact_patient_visit (
    visit_key           BIGINT,
    date_key            INT,
    patient_key         BIGINT,
    provider_key        BIGINT,
    diagnosis_key       BIGINT,
    visit_duration_min  INT,
    copay_amount        DECIMAL(8,2),
    billed_amount       DECIMAL(10,2)
);
```

**Example 3: Streaming Analytics**
```sql
-- Grain: One row per song play
CREATE TABLE fact_song_play (
    play_key        BIGINT,
    timestamp_key   BIGINT,
    user_key        BIGINT,
    song_key        BIGINT,
    device_key      INT,
    play_duration_sec   INT,
    skip_flag       BOOLEAN
);
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Intuitive** | Business users understand "what happened" + "context" |
| **Query Performance** | Predictable join patterns, BI tools optimize well |
| **Flexibility** | Add new dimensions without changing fact table |
| **Scalability** | Fact tables partition well by date |
| **BI Tool Friendly** | Tableau, Power BI, Looker work natively |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Redundancy** | Denormalized dimensions duplicate data |
| **ETL Complexity** | Must handle SCDs, surrogate keys |
| **Not for OLTP** | Designed for reads, not transactional writes |
| **Upfront Design** | Grain decisions are hard to change later |

---

**Common Grain Mistakes:**
```sql
-- TOO COARSE: Can't answer "what products sold?"
fact_daily_sales (date_key, store_key, total_revenue)  -- Lost product detail!

-- TOO FINE: Explodes storage, slow queries
fact_clickstream (user_key, timestamp_ms, x_coord, y_coord)  -- Every mouse move!

-- JUST RIGHT: Answer business questions without waste
fact_order_line (order_key, product_key, quantity, amount)  -- Per line item
```

**Interview Tip:** "The grain is the most critical decision. Get it wrong and you can't answer business questions or you explode storage. I always document the grain statement explicitly: 'One row represents one order line item for one product.'"

[↑ Back to Topics](#core-dimensional-modeling)

---

### Star Schema

**Definition:** A dimensional model where a central fact table connects directly to denormalized dimension tables, forming a star pattern. The "gold standard" for analytical data warehouses.

**Characteristics:**
- Dimension tables are flat (no hierarchies split out)
- Simple queries (one join per dimension)
- Optimized for BI tools and OLAP queries
- Each dimension contains ALL its descriptive attributes

**Visual:**
```
                    dim_customer
                    (customer_key, name, city, state, country, segment)
                          │
    dim_date ────────── fact_sales ────────── dim_product
    (date_key,            │                   (product_key, name,
     month, quarter,      │                    category, subcategory,
     year, is_weekend)    │                    brand, supplier)
                          │
                    dim_store
                    (store_key, name, region, manager, sq_feet)
```

---

**Example 1: Retail Sales Star**
```sql
-- Query: Monthly revenue by customer segment
SELECT 
    d.month_name,
    c.segment,
    SUM(f.total_amount) as revenue
FROM fact_sales f
JOIN dim_date d ON f.date_key = d.date_key
JOIN dim_customer c ON f.customer_key = c.customer_key
WHERE d.year = 2025
GROUP BY d.month_name, c.segment
ORDER BY d.month_name;
```

**Example 2: Product Performance Analysis**
```sql
-- Query: Top 10 products by category with YoY growth
SELECT 
    p.category,
    p.product_name,
    SUM(CASE WHEN d.year = 2025 THEN f.quantity END) as qty_2025,
    SUM(CASE WHEN d.year = 2024 THEN f.quantity END) as qty_2024,
    ROUND(100.0 * (SUM(CASE WHEN d.year = 2025 THEN f.quantity END) - 
                   SUM(CASE WHEN d.year = 2024 THEN f.quantity END)) / 
          NULLIF(SUM(CASE WHEN d.year = 2024 THEN f.quantity END), 0), 2) as yoy_growth_pct
FROM fact_sales f
JOIN dim_date d ON f.date_key = d.date_key
JOIN dim_product p ON f.product_key = p.product_key
GROUP BY p.category, p.product_name
ORDER BY qty_2025 DESC
LIMIT 10;
```

**Example 3: Multi-Dimensional Slice**
```sql
-- Query: Weekend vs Weekday sales by region and product category
SELECT 
    s.region,
    p.category,
    d.is_weekend,
    COUNT(*) as num_transactions,
    SUM(f.total_amount) as revenue
FROM fact_sales f
JOIN dim_date d ON f.date_key = d.date_key
JOIN dim_store s ON f.store_key = s.store_key
JOIN dim_product p ON f.product_key = p.product_key
GROUP BY s.region, p.category, d.is_weekend;
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Simple Queries** | One join per dimension, intuitive SQL |
| **Fast Performance** | Fewer joins = faster execution |
| **BI Tool Native** | Tableau, Power BI auto-detect star schemas |
| **Easy to Understand** | Business users can write their own queries |
| **Columnar Storage** | Compresses dimension attributes efficiently |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Data Redundancy** | "California" repeated millions of times in dim_customer |
| **Large Dimensions** | Very wide tables if many attributes |
| **Update Anomalies** | Changing "California" to "CA" requires full dimension reload |
| **Storage Cost** | More bytes than snowflake (but storage is cheap) |

---

**When Star Schema Shines:**
- Interactive dashboards needing sub-second response
- Business analysts writing ad-hoc queries
- Standard BI tool deployments
- Medium-sized dimensions (< 10M rows)

**Interview Tip:** "Star schema is my default choice for analytics because join simplicity and BI tool compatibility outweigh the storage cost. Modern columnar storage compresses the redundancy efficiently."

[↑ Back to Topics](#core-dimensional-modeling)

---

### Snowflake Schema

**Definition:** Normalized dimension tables where hierarchies are split into separate tables. Named because the diagram resembles a snowflake with branches extending from each dimension.

---

**Visual Comparison:**
```
STAR SCHEMA (flat dimensions):
┌─────────────────────────────────┐
│ dim_product                     │
│ product_key, name, brand,       │
│ subcategory, category, dept     │  ← All in ONE table
└─────────────────────────────────┘

SNOWFLAKE SCHEMA (normalized hierarchy):
┌──────────────┐    ┌────────────────┐    ┌─────────────┐    ┌──────────────┐
│ dim_product  │───►│ dim_subcategory│───►│ dim_category│───►│ dim_department│
│ product_key  │    │ subcategory_key│    │ category_key│    │ department_key│
│ name, brand  │    │ subcat_name    │    │ cat_name    │    │ dept_name     │
└──────────────┘    └────────────────┘    └─────────────┘    └──────────────┘
```

---

**Example 1: Product Hierarchy**
```sql
-- Snowflake: 4 tables for product dimension
CREATE TABLE dim_department (
    department_key  INT PRIMARY KEY,
    dept_name       VARCHAR(50)   -- 10 rows: Electronics, Clothing, etc.
);

CREATE TABLE dim_category (
    category_key    INT PRIMARY KEY,
    department_key  INT REFERENCES dim_department,
    category_name   VARCHAR(50)   -- 50 rows: Laptops, TVs, Shirts, etc.
);

CREATE TABLE dim_subcategory (
    subcategory_key INT PRIMARY KEY,
    category_key    INT REFERENCES dim_category,
    subcat_name     VARCHAR(50)   -- 200 rows: Gaming Laptops, Smart TVs, etc.
);

CREATE TABLE dim_product (
    product_key     INT PRIMARY KEY,
    subcategory_key INT REFERENCES dim_subcategory,
    product_name    VARCHAR(100),
    brand           VARCHAR(50)   -- 50,000 rows: actual products
);
```

**Example 2: Geography Hierarchy**
```sql
-- Snowflake for location
CREATE TABLE dim_country (
    country_key     INT PRIMARY KEY,
    country_name    VARCHAR(50),
    continent       VARCHAR(20)
);

CREATE TABLE dim_state (
    state_key       INT PRIMARY KEY,
    country_key     INT REFERENCES dim_country,
    state_name      VARCHAR(50)
);

CREATE TABLE dim_city (
    city_key        INT PRIMARY KEY,
    state_key       INT REFERENCES dim_state,
    city_name       VARCHAR(100),
    population      INT
);
```

---

**Query Comparison:**

```sql
-- STAR SCHEMA: 1 join to get department
SELECT p.department, SUM(f.amount)
FROM fact_sales f
JOIN dim_product p ON f.product_key = p.product_key
GROUP BY p.department;

-- SNOWFLAKE SCHEMA: 4 joins to get department
SELECT dept.dept_name, SUM(f.amount)
FROM fact_sales f
JOIN dim_product p ON f.product_key = p.product_key
JOIN dim_subcategory sc ON p.subcategory_key = sc.subcategory_key
JOIN dim_category c ON sc.category_key = c.category_key
JOIN dim_department dept ON c.department_key = dept.department_key
GROUP BY dept.dept_name;
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Storage Efficiency** | "Electronics" stored once, not 50K times |
| **Data Integrity** | FK constraints prevent orphan records |
| **Easy Updates** | Rename department in 1 row, not 50K |
| **Sparse Attributes** | Null-heavy columns isolated in sub-tables |
| **M:N Relationships** | Naturally supports product → multiple categories |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **More Joins** | 4+ joins for simple queries |
| **Slower Queries** | Join overhead hurts performance |
| **BI Tool Issues** | Some tools don't auto-detect snowflake |
| **Complex ETL** | Maintain referential integrity across tables |
| **User Confusion** | Business users struggle with multi-table joins |

---

**When to Use Snowflake:**
- Dimensions with 100+ million rows
- Attributes that are 90%+ NULL (sparse)
- Many-to-many dimension relationships
- Strict referential integrity requirements (regulatory)
- Dimension updates are frequent

**When to Avoid:**
- Interactive dashboards (latency matters)
- Self-service BI for business users
- Modern columnar storage (compression handles redundancy)

**Interview Tip:** "I use snowflake schema sparingly - only for very large dimensions with sparse attributes or when referential integrity is mandated. For most analytics, star schema's query simplicity wins."

[↑ Back to Topics](#core-dimensional-modeling)

---

### Fact Tables

**Definition:** Central tables in dimensional models that store measurable business events. The "what happened" of your data warehouse.

---

**Types of Fact Tables:**

| Type | Description | Example | Row Behavior |
|------|-------------|---------|---------------|
| **Transaction** | One row per event | Each sale, each click | INSERT only |
| **Periodic Snapshot** | One row per time period | Daily inventory levels | INSERT (new periods) |
| **Accumulating Snapshot** | One row per lifecycle | Order fulfillment | UPDATE as milestones hit |
| **Factless** | No measures, just relationships | Student attendance | INSERT only |

---

**Example 1: Transaction Fact (Most Common)**
```sql
-- Grain: One row per order line item
CREATE TABLE fact_order_line (
    order_line_key      BIGINT PRIMARY KEY,
    order_key           BIGINT,          -- Degenerate dimension
    date_key            INT,
    customer_key        BIGINT,
    product_key         BIGINT,
    store_key           INT,
    -- Measures (additive)
    quantity            INT,
    unit_price          DECIMAL(10,2),
    discount_amount     DECIMAL(10,2),
    line_total          DECIMAL(12,2),
    cost                DECIMAL(10,2),
    profit              DECIMAL(10,2)
);

-- Query: Daily sales by product category
SELECT d.date, p.category, SUM(f.line_total) as revenue
FROM fact_order_line f
JOIN dim_date d ON f.date_key = d.date_key
JOIN dim_product p ON f.product_key = p.product_key
GROUP BY d.date, p.category;
```

**Example 2: Periodic Snapshot**
```sql
-- Grain: One row per product per warehouse per day
CREATE TABLE fact_inventory_daily (
    snapshot_date_key   INT,
    product_key         BIGINT,
    warehouse_key       INT,
    -- Measures (semi-additive - can't sum across time)
    quantity_on_hand    INT,
    quantity_reserved   INT,
    quantity_available  INT,      -- on_hand - reserved
    days_of_supply      DECIMAL(5,1),
    PRIMARY KEY (snapshot_date_key, product_key, warehouse_key)
);

-- Query: Current inventory levels (point-in-time)
SELECT w.warehouse_name, SUM(f.quantity_on_hand) as total_units
FROM fact_inventory_daily f
JOIN dim_warehouse w ON f.warehouse_key = w.warehouse_key
WHERE f.snapshot_date_key = 20250103  -- Today's snapshot
GROUP BY w.warehouse_name;
```

**Example 3: Accumulating Snapshot**
```sql
-- Grain: One row per order (updated as order progresses)
CREATE TABLE fact_order_fulfillment (
    order_key               BIGINT PRIMARY KEY,
    customer_key            BIGINT,
    
    -- Milestone dates (NULL until reached)
    order_date_key          INT NOT NULL,
    payment_date_key        INT,
    pick_date_key           INT,
    ship_date_key           INT,
    delivery_date_key       INT,
    
    -- Lag measures (calculated when milestones complete)
    order_to_payment_days   INT,
    payment_to_ship_days    INT,
    ship_to_delivery_days   INT,
    total_fulfillment_days  INT,
    
    -- Status
    current_status          VARCHAR(20)  -- 'ordered','paid','shipped','delivered'
);

-- Query: Average fulfillment time by status
SELECT current_status, AVG(total_fulfillment_days) as avg_days
FROM fact_order_fulfillment
WHERE delivery_date_key IS NOT NULL
GROUP BY current_status;
```

---

**✅ Pros of Each Type:**

| Type | Best For | Advantage |
|------|----------|----------|
| Transaction | Detailed analysis | Full fidelity, drill-down to event level |
| Periodic Snapshot | Point-in-time state | Simple trend analysis, consistent grain |
| Accumulating | Process tracking | Lifecycle visibility, bottleneck identification |

**❌ Cons of Each Type:**

| Type | Drawback | Mitigation |
|------|----------|------------|
| Transaction | Can be HUGE (billions of rows) | Partition by date, aggregate tables |
| Periodic Snapshot | Storage grows daily | Archive old snapshots, keep N days |
| Accumulating | Requires UPDATEs | Use Delta Lake MERGE, careful CDC |

---

**Grain Statement Best Practices:**
```
✅ GOOD: "One row represents one order line item for one product purchased by one customer at one store on one day."

❌ BAD: "One row per sale."  (Ambiguous - sale = order or line item?)
```

**Interview Tip:** "I always document the grain statement before building. For transaction facts, I ensure the grain supports drill-down to answer 'which specific transaction?' For snapshots, I define 'as of when?' clearly."

[↑ Back to Topics](#core-dimensional-modeling)

---

### Additive vs Non-Additive Facts

**Definition:** Classification of measures based on how they can be aggregated across dimensions.

---

| Type | Can SUM Across | Examples | Aggregation Strategy |
|------|----------------|----------|----------------------|
| **Additive** | All dimensions | Revenue, Quantity, Cost, Clicks | `SUM()` always valid |
| **Semi-Additive** | Some dimensions | Account Balance, Inventory | `SUM()` across some, `AVG/LAST` for time |
| **Non-Additive** | None | Ratios, Percentages, Unit Price | Store components, calculate at query time |

---

**Example 1: Additive Facts (Easy)**
```sql
-- Sales amount is additive - SUM works across ALL dimensions
SELECT 
    d.year,                            -- ✓ SUM across time
    c.region,                          -- ✓ SUM across geography
    p.category,                        -- ✓ SUM across products
    SUM(f.quantity) as total_units,    -- ✓ Meaningful
    SUM(f.revenue) as total_revenue    -- ✓ Meaningful
FROM fact_sales f
JOIN dim_date d ON f.date_key = d.date_key
JOIN dim_customer c ON f.customer_key = c.customer_key
JOIN dim_product p ON f.product_key = p.product_key
GROUP BY d.year, c.region, p.category;
```

**Example 2: Semi-Additive Facts**
```sql
-- Account balance: SUM across accounts, NOT across time

-- ✅ CORRECT: Total deposits across all accounts on Jan 1
SELECT SUM(balance) as total_deposits
FROM fact_account_balance
WHERE snapshot_date = '2025-01-01';  -- Point in time!

-- ❌ WRONG: SUM(balance) across dates is meaningless
-- Jan 1 balance + Jan 2 balance ≠ anything useful

-- ✅ CORRECT: Average daily balance over time
SELECT account_id, AVG(balance) as avg_daily_balance
FROM fact_account_balance
WHERE snapshot_date BETWEEN '2025-01-01' AND '2025-01-31'
GROUP BY account_id;

-- ✅ CORRECT: Latest balance (most common need)
SELECT account_id, balance
FROM fact_account_balance
WHERE snapshot_date = (SELECT MAX(snapshot_date) FROM fact_account_balance);
```

**Example 3: Non-Additive Facts**
```sql
-- Unit price, margins, ratios CANNOT be summed

-- ❌ WRONG: SUM(unit_price) = $500 (meaningless!)
SELECT SUM(unit_price) FROM fact_sales;

-- ❌ WRONG: SUM(margin_pct) = 250% (nonsense!)
SELECT SUM(margin_pct) FROM fact_sales;

-- ✅ CORRECT: Weighted average price
SELECT 
    p.category,
    SUM(f.quantity * f.unit_price) / SUM(f.quantity) as weighted_avg_price
FROM fact_sales f
JOIN dim_product p ON f.product_key = p.product_key
GROUP BY p.category;

-- ✅ CORRECT: Calculate ratio from components
SELECT 
    p.category,
    SUM(f.profit) / NULLIF(SUM(f.revenue), 0) * 100 as margin_pct
FROM fact_sales f
JOIN dim_product p ON f.product_key = p.product_key
GROUP BY p.category;
```

---

**Best Practice: Store Components, Not Ratios**

```sql
-- ❌ BAD: Storing calculated ratio
fact_sales (
    margin_pct DECIMAL(5,2)  -- Can't aggregate!
)

-- ✅ GOOD: Store components, calculate at query time
fact_sales (
    revenue    DECIMAL(12,2),  -- Additive
    cost       DECIMAL(12,2),  -- Additive
    profit     DECIMAL(12,2)   -- Additive (revenue - cost)
    -- Calculate margin_pct = profit/revenue in SQL
)
```

---

**Common Semi-Additive Measures:**
- Account balances
- Inventory levels
- Headcount
- Open orders
- Active subscriptions

**Common Non-Additive Measures:**
- Unit price, unit cost
- Percentages, ratios
- Rates ($/hour, conversion rate)
- Scores, rankings

**Interview Tip:** "I always store additive components and calculate ratios at query time. This ensures correct aggregation at any level. For semi-additive facts like inventory, I use snapshot tables and always query with a point-in-time filter."

[↑ Back to Topics](#core-dimensional-modeling)

---

### Semi-Additive Facts

**Definition:** Measures that can be summed across some dimensions but not others. The classic trap for data analysts!

---

**The Rule:**
- ✅ SUM across: accounts, products, locations, customers (entity dimensions)
- ❌ SUM across: time (makes no sense for balances/levels)

---

**Example 1: Bank Account Balance**
```sql
-- Fact table: Daily balance snapshot
CREATE TABLE fact_account_balance (
    snapshot_date   DATE,
    account_key     BIGINT,
    branch_key      INT,
    balance         DECIMAL(15,2),
    PRIMARY KEY (snapshot_date, account_key)
);

-- ✅ VALID: Total deposits across all accounts on a specific day
SELECT SUM(balance) as total_deposits
FROM fact_account_balance
WHERE snapshot_date = '2025-01-03';  -- $10 billion

-- ❌ INVALID: SUM across dates
SELECT SUM(balance) FROM fact_account_balance;  -- $300 billion (counts same $ 30 times!)

-- ✅ VALID: Average balance over time (for one account)
SELECT account_key, AVG(balance) as avg_balance
FROM fact_account_balance
WHERE snapshot_date BETWEEN '2025-01-01' AND '2025-01-31'
GROUP BY account_key;

-- ✅ VALID: End-of-month balance (common reporting need)
SELECT b.branch_name, SUM(f.balance) as eom_deposits
FROM fact_account_balance f
JOIN dim_branch b ON f.branch_key = b.branch_key
WHERE f.snapshot_date = '2025-01-31'  -- Last day of month
GROUP BY b.branch_name;
```

**Example 2: Inventory Levels**
```sql
-- Grain: One row per product per warehouse per day
CREATE TABLE fact_inventory_snapshot (
    snapshot_date_key   INT,
    product_key         BIGINT,
    warehouse_key       INT,
    quantity_on_hand    INT,
    quantity_on_order   INT
);

-- ✅ VALID: Total inventory across all warehouses TODAY
SELECT SUM(quantity_on_hand) as total_inventory
FROM fact_inventory_snapshot
WHERE snapshot_date_key = 20250103;

-- ✅ VALID: Average inventory level (for capacity planning)
SELECT 
    p.product_name,
    AVG(f.quantity_on_hand) as avg_daily_inventory,
    MAX(f.quantity_on_hand) as peak_inventory
FROM fact_inventory_snapshot f
JOIN dim_product p ON f.product_key = p.product_key
WHERE f.snapshot_date_key BETWEEN 20250101 AND 20250131
GROUP BY p.product_name;
```

**Example 3: Employee Headcount**
```sql
-- Grain: One row per department per month
CREATE TABLE fact_headcount_monthly (
    month_key           INT,
    department_key      INT,
    headcount           INT,
    avg_tenure_months   DECIMAL(5,1)
);

-- ✅ VALID: Company-wide headcount for January
SELECT SUM(headcount) as total_employees
FROM fact_headcount_monthly
WHERE month_key = 202501;

-- ❌ INVALID: SUM across months = double counting
SELECT SUM(headcount) FROM fact_headcount_monthly;  -- Wrong!
```

---

**Aggregation Strategies for Time:**

| Need | Function | Example |
|------|----------|--------|
| Current value | `LAST` / Filter to MAX date | Today's balance |
| Average over period | `AVG()` | Avg daily inventory |
| Peak/trough | `MAX()` / `MIN()` | Highest headcount |
| Beginning vs End | Filter to specific dates | Jan 1 vs Jan 31 |
| Monthly trend | Filter per month | One value per month |

---

**BI Tool Trap:**
```
Tableau/Power BI user drags:
  - Account Balance to Values (SUM by default!)
  - Month to Columns
  - Branch to Rows

Result: SUM of balances for each month = WRONG!

Fix: Change aggregation to LAST or filter to end-of-month date
```

---

**✅ Pros of Semi-Additive Design:**
- Captures point-in-time state accurately
- Enables trend analysis when used correctly
- Storage efficient (one row per period)

**❌ Cons:**
- Requires careful aggregation logic
- BI tools default to SUM (common errors)
- User education needed

**Interview Tip:** "I always flag semi-additive measures in documentation and column naming. For BI tools, I create calculated fields with proper aggregation logic so users can't accidentally SUM account balances across time."

[↑ Back to Topics](#core-dimensional-modeling)

---

### Factless Fact Tables

**Definition:** A fact table with no numeric measures - the existence of a row IS the fact. Used to track events, relationships, or conditions.

---

**Two Types:**

| Type | Purpose | Example |
|------|---------|--------|
| **Event Tracking** | "Something happened" | Student attended class, employee clocked in |
| **Coverage** | "What was possible/available" | Products on promotion, drivers on shift |

---

**Example 1: Student Attendance (Event Tracking)**
```sql
-- Grain: One row per student per class per day they attended
CREATE TABLE fact_class_attendance (
    date_key        INT,
    student_key     BIGINT,
    class_key       INT,
    instructor_key  INT,
    room_key        INT,
    -- NO MEASURES! Row exists = student attended
    PRIMARY KEY (date_key, student_key, class_key)
);

-- Query: Attendance rate by class
SELECT 
    c.class_name,
    COUNT(DISTINCT a.student_key) as students_attended,
    e.enrolled_count,
    ROUND(100.0 * COUNT(DISTINCT a.student_key) / e.enrolled_count, 1) as attendance_pct
FROM fact_class_attendance a
JOIN dim_class c ON a.class_key = c.class_key
JOIN (
    SELECT class_key, COUNT(*) as enrolled_count
    FROM dim_enrollment
    GROUP BY class_key
) e ON a.class_key = e.class_key
WHERE a.date_key = 20250103
GROUP BY c.class_name, e.enrolled_count;
```

**Example 2: Product Promotions (Coverage)**
```sql
-- Grain: One row per product per store per day on promotion
CREATE TABLE fact_promotion_coverage (
    date_key        INT,
    product_key     BIGINT,
    store_key       INT,
    promotion_key   INT,
    -- NO MEASURES! Row exists = product was on promotion
    PRIMARY KEY (date_key, product_key, store_key, promotion_key)
);

-- Query: Which products were on promotion but didn't sell?
SELECT 
    p.product_name,
    s.store_name,
    pr.promotion_name
FROM fact_promotion_coverage pc
JOIN dim_product p ON pc.product_key = p.product_key
JOIN dim_store s ON pc.store_key = s.store_key
JOIN dim_promotion pr ON pc.promotion_key = pr.promotion_key
LEFT JOIN fact_sales f ON pc.date_key = f.date_key
    AND pc.product_key = f.product_key
    AND pc.store_key = f.store_key
WHERE pc.date_key = 20250103
  AND f.sale_key IS NULL;  -- No sales!
```

**Example 3: Driver Availability (Coverage)**
```sql
-- Grain: One row per driver per 15-min slot per zone where available
CREATE TABLE fact_driver_availability (
    date_key        INT,
    time_slot_key   INT,        -- 96 slots per day (15-min intervals)
    driver_key      BIGINT,
    zone_key        INT,
    -- NO MEASURES!
    PRIMARY KEY (date_key, time_slot_key, driver_key)
);

-- Query: Supply/demand analysis
WITH supply AS (
    SELECT date_key, time_slot_key, zone_key, COUNT(*) as drivers_available
    FROM fact_driver_availability
    GROUP BY date_key, time_slot_key, zone_key
),
demand AS (
    SELECT date_key, time_slot_key, zone_key, COUNT(*) as ride_requests
    FROM fact_ride_request
    GROUP BY date_key, time_slot_key, zone_key
)
SELECT 
    z.zone_name,
    t.time_slot,
    s.drivers_available,
    d.ride_requests,
    CASE WHEN d.ride_requests > s.drivers_available THEN 'Undersupplied' ELSE 'OK' END as status
FROM supply s
JOIN demand d ON s.date_key = d.date_key 
    AND s.time_slot_key = d.time_slot_key 
    AND s.zone_key = d.zone_key
JOIN dim_zone z ON s.zone_key = z.zone_key
JOIN dim_time_slot t ON s.time_slot_key = t.time_slot_key;
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Captures events without measures** | Attendance, eligibility, coverage |
| **Enables coverage analysis** | What was available vs what sold |
| **Supports negative analysis** | "Why didn't X happen?" queries |
| **Simple grain** | Binary: row exists or doesn't |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Can grow large** | High-cardinality combinations |
| **Confusing to users** | "Where are the numbers?" |
| **Harder to query** | Requires JOINs with other facts |

---

**Common Use Cases:**
- ✅ Class/event attendance
- ✅ Employee schedule/shift coverage
- ✅ Product promotion periods
- ✅ Marketing campaign exposure
- ✅ Eligibility/availability windows

**Interview Tip:** "Factless facts are powerful for coverage analysis - answering 'What was possible but didn't happen?' For example, products on promotion that didn't sell, or drivers available but no rides requested. The row's existence is the measure."

[↑ Back to Topics](#core-dimensional-modeling)

---

## Tier 1: Slowly Changing Dimensions

### Slowly Changing Dimensions

**Definition:** Dimension attributes that change over time (customer address changes, product price changes, employee promotion). The challenge: How do we track historical values while supporting current-state queries?

---

### SCD Types Comparison

| Type | Strategy | History | Storage | ETL Complexity | Use When |
|------|----------|---------|---------|----------------|----------|
| **Type 0** | Never change | None | Lowest | Simplest | Immutable facts (birth date, SSN) |
| **Type 1** | Overwrite | None | Low | Simple | Corrections, history not needed |
| **Type 2** | New row + dates | Full | High | Complex | Full audit trail needed |
| **Type 3** | Previous column | Last only | Medium | Medium | Need current + previous |
| **Type 4** | History table | Full | Medium | Medium | Frequent changes, separate query |
| **Type 6** | Hybrid 1+2+3 | Full + current | Highest | Most complex | Best of all worlds |

---

**Example 1: Type 1 (Overwrite) - Typo Correction**
```sql
-- Customer name was misspelled, just fix it
-- Before: John Smithh
-- After: John Smith

UPDATE dim_customer
SET customer_name = 'John Smith'
WHERE customer_id = 'C001';

-- No history preserved - old name is gone forever
-- Use for: typos, data quality fixes, non-critical attributes
```

**Example 2: Type 2 (New Row with Date Range) - Most Common**
```sql
CREATE TABLE dim_customer_scd2 (
    customer_key    BIGINT PRIMARY KEY,      -- Surrogate (unique per version)
    customer_id     VARCHAR(50),              -- Natural key (same across versions)
    name            VARCHAR(100),
    email           VARCHAR(200),
    segment         VARCHAR(20),              -- The attribute that changes!
    city            VARCHAR(50),
    valid_from      DATE NOT NULL,
    valid_to        DATE NOT NULL,            -- '9999-12-31' = current
    is_current      BOOLEAN DEFAULT FALSE
);

-- Customer upgrades from Bronze to Gold on Jan 5, 2025
-- BEFORE (one row):
-- | key | customer_id | segment | valid_from | valid_to   | is_current |
-- |-----|-------------|---------|------------|------------|------------|
-- | 101 | C001        | Bronze  | 2020-01-01 | 9999-12-31 | TRUE       |

-- AFTER (two rows):
-- | key | customer_id | segment | valid_from | valid_to   | is_current |
-- |-----|-------------|---------|------------|------------|------------|
-- | 101 | C001        | Bronze  | 2020-01-01 | 2025-01-04 | FALSE      |
-- | 102 | C001        | Gold    | 2025-01-05 | 9999-12-31 | TRUE       |

-- Query: Current state (for operational reports)
SELECT * FROM dim_customer_scd2 WHERE is_current = TRUE;

-- Query: Historical analysis (join fact with point-in-time dimension)
SELECT f.*, d.segment
FROM fact_orders f
JOIN dim_customer_scd2 d ON f.customer_id = d.customer_id
    AND f.order_date >= d.valid_from
    AND f.order_date < d.valid_to;
```

**Example 3: Type 3 (Previous Value Column)**
```sql
CREATE TABLE dim_customer_scd3 (
    customer_key        BIGINT PRIMARY KEY,
    customer_id         VARCHAR(50),
    current_segment     VARCHAR(20),
    previous_segment    VARCHAR(20),       -- Only stores ONE previous value
    segment_change_date DATE
);

-- After upgrade:
-- | key | customer_id | current_segment | previous_segment | change_date |
-- |-----|-------------|-----------------|------------------|-------------|
-- | 101 | C001        | Gold            | Bronze           | 2025-01-05  |

-- Limitation: If customer changes again (Gold → Platinum), Bronze is lost!
```

**Example 4: Type 6 (Hybrid 1+2+3) - Full Flexibility**
```sql
CREATE TABLE dim_customer_scd6 (
    customer_key        BIGINT PRIMARY KEY,
    customer_id         VARCHAR(50),
    
    -- Type 2: Historical tracking
    segment             VARCHAR(20),       -- Value at this version
    valid_from          DATE,
    valid_to            DATE,
    
    -- Type 1: Current value (denormalized for easy access)
    current_segment     VARCHAR(20),       -- Always reflects latest
    
    -- Type 3: Previous value
    previous_segment    VARCHAR(20)
);

-- Both rows show current_segment = 'Gold' for easy filtering
-- | key | customer_id | segment | valid_from | valid_to   | current_segment | previous_segment |
-- |-----|-------------|---------|------------|------------|-----------------|------------------|
-- | 101 | C001        | Bronze  | 2020-01-01 | 2025-01-04 | Gold            | Bronze           |
-- | 102 | C001        | Gold    | 2025-01-05 | 9999-12-31 | Gold            | Bronze           |
```

---

**✅ Pros and ❌ Cons by Type:**

**Type 1:**
- ✅ Simple ETL, low storage
- ❌ No history, audit trail lost

**Type 2:**
- ✅ Full history, accurate point-in-time analysis
- ❌ Table grows, complex joins, requires surrogate keys

**Type 3:**
- ✅ Easy comparison (current vs previous)
- ❌ Only one version of history, schema changes for new attributes

**Type 6:**
- ✅ Maximum flexibility, easy current-state queries
- ❌ Most complex ETL, highest storage, maintenance overhead

---

**Decision Matrix:**

| Requirement | Recommended Type |
|-------------|------------------|
| No history needed, corrections only | Type 1 |
| Full audit trail required | Type 2 |
| Compare current to previous only | Type 3 |
| Regulatory compliance (finance, healthcare) | Type 2 or 6 |
| Most attributes change rarely | Type 2 |
| Some attributes change frequently | Type 4 (mini-dimension) |

**Interview Tip:** "I default to Type 2 for any attribute where business might ask 'what was the value when this transaction happened?' For corrections and non-critical attributes, Type 1 is fine. Type 6 adds convenience but increases ETL complexity - I use it when both historical analysis AND current-state reporting are frequent."

[↑ Back to Topics](#slowly-changing-dimensions)

---

## Tier 1: Foundational Concepts

### Data Warehousing

**Definition:** A centralized repository that stores integrated data from multiple sources, optimized for analytical queries. The foundation of business intelligence.

---

**Bill Inmon's Four Characteristics:**

| Characteristic | Description | Example |
|----------------|-------------|--------|
| **Subject-oriented** | Organized by business subjects | Sales, Finance, HR (not by source system) |
| **Integrated** | Data unified with consistent formats | All dates as YYYY-MM-DD, all currencies as USD |
| **Time-variant** | Historical data preserved | 10 years of transaction history |
| **Non-volatile** | Data is stable, read-mostly | No operational UPDATEs, only batch loads |

---

**Inmon vs Kimball Approaches:**

| Aspect | Inmon (Top-Down) | Kimball (Bottom-Up) |
|--------|------------------|---------------------|
| **Build Order** | Enterprise DW first, then data marts | Data marts first, enterprise bus later |
| **Schema** | Normalized (3NF) | Dimensional (Star Schema) |
| **Time to Value** | Longer (12-24 months) | Shorter (3-6 months) |
| **Flexibility** | More flexible, less redundant | Faster queries, more redundant |
| **ETL** | ETL to central DW | ETL to each data mart |
| **When to Use** | Large enterprises, strict governance | Faster delivery, departmental needs |

---

**Example 1: Inmon Architecture**
```
Source Systems                  Enterprise DW (3NF)           Data Marts
┌───────────┐                   ┌─────────────────┐         ┌───────────┐
│ ERP       │─────────────────►│ Customers       │────────►│ Sales     │
└───────────┘                   │ Orders          │         │ Mart      │
┌───────────┐                   │ Products        │         └───────────┘
│ CRM       │─────────────────►│ Employees       │         ┌───────────┐
└───────────┘                   │ Transactions    │────────►│ Finance   │
┌───────────┐                   │ (Normalized)    │         │ Mart      │
│ Web Logs  │─────────────────►└─────────────────┘         └───────────┘
└───────────┘
```

**Example 2: Kimball Architecture**
```
Source Systems                  Dimensional Data Marts          Enterprise Bus
┌───────────┐                   ┌─────────────────┐
│ ERP       │─────────────────►│ Sales Mart      │──────┐
└───────────┘                   │ fact_sales      │      │   Conformed
┌───────────┐                   │ dim_customer*   │      │   Dimensions:
│ CRM       │─────────────────►│ dim_product*    │      ├───► dim_date
└───────────┘                   └─────────────────┘      ├───► dim_customer
                               ┌─────────────────┐      ├───► dim_product
                               │ Inventory Mart  │      │
                               │ fact_inventory  │──────┘
                               │ dim_product*    │
                               └─────────────────┘
* = Conformed dimension (shared across marts)
```

**Example 3: Modern Lakehouse Architecture**
```
Source Systems         Bronze         Silver           Gold (Data Marts)
┌──────────┐        ┌────────┐     ┌─────────┐     ┌────────────┐
│ Databases │──────►│ Raw    │───►│ Cleaned │───►│ Sales Mart │
│ APIs      │        │ JSON   │     │ Typed   │     │ Finance    │
│ Files     │        │ CSV    │     │ Joined  │     │ Marketing  │
│ Streams   │        │ Parquet│     │ Delta   │     │ (Star)     │
└──────────┘        └────────┘     └─────────┘     └────────────┘
                          │            │                   │
                          └────────────┴───────────────────┘
                               Data Lake (Delta Lake)
```

---

**✅ Pros of Data Warehousing:**
| Benefit | Description |
|---------|-------------|
| **Single Source of Truth** | Consistent data across all reports |
| **Optimized for Analytics** | Columnar storage, aggregations |
| **Historical Analysis** | Time-series, trend analysis |
| **Decoupled from Operations** | Queries don't impact production systems |
| **Data Quality** | Cleaned, validated, transformed |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Cost** | Infrastructure, ETL development, maintenance |
| **Latency** | Batch loads = stale data (hours/days) |
| **Complexity** | Requires specialized skills |
| **Rigidity** | Schema changes are painful |
| **Time to Build** | Months of development before value |

---

**Modern Evolution:**

| Era | Technology | Characteristics |
|-----|------------|-----------------|
| 1990s | On-prem DW (Teradata, Oracle) | Expensive, proprietary, scaled-up |
| 2010s | Cloud DW (Redshift, BigQuery, Snowflake) | Elastic, pay-per-query, managed |
| 2020s | Lakehouse (Databricks, Synapse) | Unified batch+streaming, open formats |

**Interview Tip:** "Traditional DW vs Lakehouse? I see lakehouse as the evolution - you get the structure and governance of a warehouse with the flexibility and cost-efficiency of a data lake. The medallion architecture gives you both raw data preservation (audit trail) and business-ready marts."

[↑ Back to Topics](#foundational-concepts)

---

### Data Marts

**Definition:** A subset of the data warehouse focused on a specific business area, department, or use case. Think of it as a "mini data warehouse" optimized for a particular group of users.

---

**Types of Data Marts:**

| Type | Sources From | Pros | Cons |
|------|--------------|------|------|
| **Dependent** | Enterprise Data Warehouse | Consistent, governed, single truth | Longer setup, DW required first |
| **Independent** | Source systems directly | Fast to build, departmental autonomy | Data silos, inconsistency, duplication |
| **Hybrid** | Both DW and sources | Flexibility | Complexity, potential conflicts |

---

**Example 1: Sales Data Mart**
```sql
-- Star schema optimized for sales analysis
CREATE TABLE sales_mart.fact_sales (
    sale_key        BIGINT,
    date_key        INT,
    customer_key    BIGINT,
    product_key     BIGINT,
    store_key       INT,
    salesperson_key BIGINT,
    quantity        INT,
    revenue         DECIMAL(12,2),
    cost            DECIMAL(12,2),
    profit          DECIMAL(12,2)
);

-- Pre-aggregated summary for dashboards
CREATE TABLE sales_mart.agg_daily_sales (
    date_key        INT,
    store_key       INT,
    total_revenue   DECIMAL(15,2),
    total_orders    INT,
    avg_order_value DECIMAL(10,2)
);
```

**Example 2: Finance Data Mart**
```sql
-- Optimized for financial reporting
CREATE TABLE finance_mart.fact_gl_transactions (
    transaction_key     BIGINT,
    posting_date_key    INT,
    account_key         INT,
    cost_center_key     INT,
    amount              DECIMAL(15,2),
    transaction_type    VARCHAR(10)  -- 'DR' or 'CR'
);

CREATE TABLE finance_mart.dim_account (
    account_key         INT,
    account_number      VARCHAR(20),
    account_name        VARCHAR(100),
    account_type        VARCHAR(20),  -- Asset, Liability, Expense, Revenue
    parent_account_key  INT
);
```

**Example 3: Marketing Data Mart**
```sql
-- Customer analytics and campaign performance
CREATE TABLE marketing_mart.dim_customer_360 (
    customer_key            BIGINT,
    customer_id             VARCHAR(50),
    email                   VARCHAR(200),
    segment                 VARCHAR(20),
    acquisition_channel     VARCHAR(50),
    first_purchase_date     DATE,
    lifetime_value          DECIMAL(12,2),
    recency_days            INT,
    frequency_score         INT,
    churn_risk_score        DECIMAL(5,2)
);
```

---

**In Lakehouse Architecture:**
```
Bronze (Raw)  →  Silver (Cleaned)  →  Gold (Data Marts)
                                         ├── sales_mart/
                                         ├── finance_mart/
                                         ├── marketing_mart/
                                         └── hr_mart/
```

---

**✅ Pros of Data Marts:**
| Benefit | Description |
|---------|-------------|
| **Performance** | Smaller, focused, faster queries |
| **Simplicity** | Users only see relevant data |
| **Security** | Department-specific access control |
| **Autonomy** | Teams can iterate independently |
| **Cost** | Smaller compute for specific queries |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Silos** | Independent marts = inconsistent metrics |
| **Duplication** | Same customer data in 5 marts |
| **Maintenance** | ETL jobs for each mart |
| **Cross-functional** | Hard to join across marts |

---

**Best Practices:**
1. **Use conformed dimensions** - Same `dim_customer` across all marts
2. **Dependent preferred** - Source from central DW/Silver layer
3. **Clear ownership** - One team owns each mart
4. **Document metrics** - Centralized definitions (data catalog)

**Interview Tip:** "I prefer dependent data marts sourcing from a Silver layer with conformed dimensions. This gives departments the autonomy and performance they need while maintaining enterprise consistency. Independent marts create silos - I've seen companies with 5 different 'customer count' numbers."

[↑ Back to Topics](#foundational-concepts)

---

### OLTP vs OLAP

**Definition:** Two fundamentally different database workloads with opposite optimization goals.

---

**Comprehensive Comparison:**

| Aspect | OLTP (Transactional) | OLAP (Analytical) |
|--------|----------------------|-------------------|
| **Purpose** | Run the business | Analyze the business |
| **Operations** | INSERT, UPDATE, DELETE | SELECT (aggregations) |
| **Query Pattern** | Single row by key | Scan millions of rows |
| **Users** | 1000s concurrent (customers, apps) | 10s concurrent (analysts) |
| **Response Time** | Milliseconds | Seconds to minutes |
| **Schema** | Normalized (3NF) | Denormalized (Star) |
| **Storage** | Row-oriented | Column-oriented |
| **Data Currency** | Real-time | Periodic refresh |
| **History** | Current state only | Years of history |
| **Indexes** | Many B-tree indexes | Few (rely on scans) |
| **Examples** | PostgreSQL, MySQL, Oracle | Snowflake, BigQuery, Databricks |

---

**Example 1: OLTP Query (PostgreSQL)**
```sql
-- Get customer's current order status
-- Pattern: Point lookup by key, return few rows

SELECT o.order_id, o.status, o.total_amount
FROM orders o
WHERE o.customer_id = 12345
  AND o.created_at > NOW() - INTERVAL '30 days'
ORDER BY o.created_at DESC
LIMIT 10;

-- Execution: Uses index on customer_id, returns in < 10ms
-- Rows scanned: ~10
```

**Example 2: OLAP Query (Snowflake)**
```sql
-- Revenue trend by category and region
-- Pattern: Full table scan, aggregate millions of rows

SELECT 
    d.year,
    d.quarter,
    p.category,
    c.region,
    SUM(f.revenue) as total_revenue,
    COUNT(DISTINCT f.customer_key) as unique_customers,
    SUM(f.revenue) / COUNT(DISTINCT f.customer_key) as revenue_per_customer
FROM fact_sales f
JOIN dim_date d ON f.date_key = d.date_key
JOIN dim_product p ON f.product_key = p.product_key
JOIN dim_customer c ON f.customer_key = c.customer_key
WHERE d.year IN (2024, 2025)
GROUP BY d.year, d.quarter, p.category, c.region
ORDER BY d.year, d.quarter, total_revenue DESC;

-- Execution: Scans 500M rows, columnar pruning, returns in 5 seconds
-- Rows scanned: 500,000,000 (but only 4 columns read)
```

---

**Why Column-Oriented for OLAP?**

```
ROW-ORIENTED (OLTP):                 COLUMN-ORIENTED (OLAP):
┌────────────────────────────┐      ┌────────┐┌───────┐┌───────┐
│ id, name, amount, date      │      │ id     ││ name  ││amount │
│ id, name, amount, date      │      │ 1      ││ Alice ││ 100   │
│ id, name, amount, date      │      │ 2      ││ Bob   ││ 200   │
└────────────────────────────┘      └────────┘└───────┘└───────┘

• Read whole row: Great for OLTP   • Read one column: Great for OLAP
• Fast INSERT: Append row          • Fast SUM(amount): Read only amount column
• Compression: Limited             • Compression: Excellent (similar values)
```

---

**✅ When to Use OLTP:**
- E-commerce checkout
- Banking transactions
- User authentication
- Real-time inventory updates
- Any write-heavy workload

**✅ When to Use OLAP:**
- Business dashboards
- Monthly/quarterly reports
- Ad-hoc analysis
- Machine learning feature stores
- Any read-heavy analytical workload

---

**Common Anti-Patterns:**

```sql
-- ❌ WRONG: Running analytics on OLTP database
-- This will lock tables and slow down production!
SELECT category, SUM(amount)
FROM production.orders  -- Don't do this!
GROUP BY category;

-- ✅ RIGHT: Replicate to OLAP warehouse for analytics
SELECT category, SUM(amount)
FROM analytics.fact_orders  -- Separate system
GROUP BY category;
```

**Interview Tip:** "OLTP is for running the business - fast transactions, ACID guarantees, normalized schema. OLAP is for understanding the business - aggregated queries over historical data, columnar storage, denormalized for joins. Never run analytical queries on production OLTP systems."

[↑ Back to Topics](#foundational-concepts)

---

### Normalization vs Denormalization

**Definition:** Two opposing strategies for organizing data - normalization reduces redundancy, denormalization optimizes for read performance.

---

**Normalization Levels:**

| Form | Rule | Example Violation |
|------|------|-------------------|
| **1NF** | Atomic values, no repeating groups | `phone: "555-1234, 555-5678"` |
| **2NF** | 1NF + no partial dependencies | Composite key but some columns depend on only one part |
| **3NF** | 2NF + no transitive dependencies | `zip` → `city` (city depends on zip, not the key) |
| **BCNF** | Every determinant is a candidate key | Rare edge cases beyond 3NF |

---

**Example 1: Normalization (OLTP)**
```sql
-- UNNORMALIZED (violates 1NF - repeating groups)
CREATE TABLE orders_bad (
    order_id     INT,
    customer_name VARCHAR(100),
    customer_email VARCHAR(200),
    product1_name VARCHAR(100),
    product1_qty  INT,
    product2_name VARCHAR(100),  -- What if 3 products? 10?
    product2_qty  INT
);

-- NORMALIZED (3NF)
CREATE TABLE customers (
    customer_id   INT PRIMARY KEY,
    name          VARCHAR(100),
    email         VARCHAR(200)
);

CREATE TABLE orders (
    order_id      INT PRIMARY KEY,
    customer_id   INT REFERENCES customers,
    order_date    DATE
);

CREATE TABLE order_lines (
    order_id      INT REFERENCES orders,
    product_id    INT REFERENCES products,
    quantity      INT,
    unit_price    DECIMAL(10,2),
    PRIMARY KEY (order_id, product_id)
);

CREATE TABLE products (
    product_id    INT PRIMARY KEY,
    name          VARCHAR(100),
    category      VARCHAR(50)
);
```

**Example 2: Denormalization (OLAP)**
```sql
-- DENORMALIZED for analytics
CREATE TABLE fact_order_line (
    order_line_key      BIGINT,
    order_id            INT,               -- Degenerate dimension
    order_date          DATE,
    
    -- Customer (embedded, not joined)
    customer_id         INT,
    customer_name       VARCHAR(100),
    customer_email      VARCHAR(200),
    customer_segment    VARCHAR(20),
    customer_city       VARCHAR(50),
    customer_state      VARCHAR(50),
    
    -- Product (embedded)
    product_id          INT,
    product_name        VARCHAR(100),
    product_category    VARCHAR(50),
    product_brand       VARCHAR(50),
    
    -- Measures
    quantity            INT,
    unit_price          DECIMAL(10,2),
    line_total          DECIMAL(12,2)
);

-- Query without any JOINs!
SELECT customer_segment, product_category, SUM(line_total)
FROM fact_order_line
GROUP BY customer_segment, product_category;
```

---

**Comparison:**

| Aspect | Normalized (3NF) | Denormalized (Star) |
|--------|------------------|---------------------|
| **Redundancy** | Minimal | High (intentional) |
| **Storage** | Lower | Higher |
| **Joins** | Many (5-10+) | Few (1-3) |
| **Query Speed** | Slower | Faster |
| **Write Speed** | Faster | Slower |
| **Update Anomalies** | None | Possible (but managed by ETL) |
| **Flexibility** | High | Lower |
| **Best For** | OLTP | OLAP |

---

**✅ Pros of Normalization:**
- No update anomalies (change name once, affects everywhere)
- Minimal storage (no duplication)
- Flexible schema changes
- Data integrity enforced by constraints

**❌ Cons of Normalization:**
- Many joins = slower queries
- Complex SQL for simple questions
- Not optimized for read-heavy analytics

**✅ Pros of Denormalization:**
- Fast queries (minimal joins)
- Simple SQL for analytics
- Columnar compression handles redundancy
- BI tool friendly

**❌ Cons of Denormalization:**
- Update anomalies (customer name in million rows)
- ETL complexity (must sync changes)
- Schema changes require backfill

---

**Modern Best Practice:**
```
┌─────────────┐      ┌──────────────────┐      ┌─────────────────┐
│ Source OLTP │      │ Silver Layer     │      │ Gold Layer      │
│ (Normalized)│ ───► │ (Normalized-ish) │ ───► │ (Denormalized)  │
│ 3NF         │      │ Cleaned, typed   │      │ Star Schema     │
└─────────────┘      └──────────────────┘      └─────────────────┘
```

**Interview Tip:** "I normalize at the source/Silver layer for flexibility and integrity, then denormalize in the Gold layer for query performance. Modern columnar storage compresses redundant values efficiently, so the storage trade-off is minimal. ETL handles update propagation."

[↑ Back to Topics](#foundational-concepts)

---

### Surrogate Keys

**Definition:** System-generated identifiers (auto-increment integers, hashes, UUIDs) that serve as primary keys instead of natural business keys.

---

**Surrogate vs Natural Keys:**

| Aspect | Surrogate Key | Natural Key |
|--------|---------------|-------------|
| **Source** | System-generated | Business-provided |
| **Examples** | `customer_key = 12345` | `customer_id = 'CUST-2025-001'` |
| **Stability** | Never changes | May change (SSN corrections, mergers) |
| **Size** | Small (INT/BIGINT) | Variable (VARCHAR) |
| **Join Speed** | Faster | Slower (string comparison) |
| **SCD Support** | Required for Type 2 | Cannot support multiple versions |

---

**Example 1: Why Surrogate Keys Are Essential for SCD Type 2**
```sql
-- WITHOUT surrogate key (broken!)
-- Customer upgrades from Bronze to Gold
-- Natural key only - can't have two rows with same customer_id!

CREATE TABLE dim_customer_broken (
    customer_id     VARCHAR(50) PRIMARY KEY,  -- Natural key
    segment         VARCHAR(20)
);
-- ❌ Can't insert second row for same customer!

-- WITH surrogate key (correct)
CREATE TABLE dim_customer (
    customer_key    BIGINT PRIMARY KEY,       -- Surrogate (unique per version)
    customer_id     VARCHAR(50),              -- Natural key (repeats for each version)
    segment         VARCHAR(20),
    valid_from      DATE,
    valid_to        DATE
);

-- Now we can have multiple versions:
-- | customer_key | customer_id | segment | valid_from | valid_to   |
-- |--------------|-------------|---------|------------|------------|
-- | 101          | CUST-001    | Bronze  | 2020-01-01 | 2025-01-04 |
-- | 102          | CUST-001    | Gold    | 2025-01-05 | 9999-12-31 |
```

**Example 2: Types of Surrogate Keys**
```sql
-- 1. Auto-increment (traditional DW)
customer_key BIGINT GENERATED ALWAYS AS IDENTITY

-- 2. Hash-based (for idempotent processing)
customer_key = MD5(customer_id || '|' || valid_from)  -- Deterministic!

-- 3. UUID (distributed systems)
customer_key = uuid_generate_v4()  -- 550e8400-e29b-41d4-a716-446655440000

-- 4. Sequence-based (Oracle style)
customer_key = customer_seq.NEXTVAL
```

**Example 3: Handling NULL Natural Keys**
```sql
-- Source data has NULL customer_id for anonymous purchases
-- Natural key approach: FAIL (NULL = NULL is false in SQL)
-- Surrogate key approach: Works!

INSERT INTO dim_customer (customer_key, customer_id, name)
VALUES 
    (1, NULL, 'Anonymous 1'),  -- Different surrogate keys
    (2, NULL, 'Anonymous 2');  -- Same NULL natural key is OK
```

---

**When to Use Each Key Type:**

| Use Case | Key Type | Rationale |
|----------|----------|-----------|
| Dimensional modeling (star schema) | Surrogate | SCD support, join performance |
| Fact table FK references | Surrogate | Compact, fast joins |
| Source system tracking | Natural (as attribute) | Traceability |
| Idempotent ETL matching | Natural | Stable across runs |
| Distributed ingestion | Hash-based surrogate | Deterministic, no coordination |
| Microservices | UUID | No central sequence needed |

---

**✅ Pros of Surrogate Keys:**
| Benefit | Description |
|---------|-------------|
| **SCD Type 2** | Multiple rows per business entity |
| **Performance** | Integers join faster than strings |
| **Stability** | Insulated from source system key changes |
| **NULL handling** | Multiple NULL natural keys possible |
| **Mergers/acquisitions** | Remap keys without changing facts |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Extra column** | Both surrogate + natural key stored |
| **ETL complexity** | Lookup or generate on every load |
| **Debugging harder** | "What is customer_key 12345?" |
| **No business meaning** | Users prefer customer_id |

---

**Best Practice: Keep Both**
```sql
CREATE TABLE dim_customer (
    customer_key    BIGINT PRIMARY KEY,      -- For joins (surrogate)
    customer_id     VARCHAR(50) NOT NULL,    -- For humans (natural)
    -- ... other columns
    UNIQUE (customer_id, valid_from)         -- Business key constraint
);

-- ETL uses natural key to find/update records
-- Queries use surrogate key for joins
-- Reports display natural key for users
```

**Interview Tip:** "Surrogate keys are mandatory for SCD Type 2 and recommended for all dimensions. I always keep the natural key as an attribute for traceability and user-facing reports. For distributed ETL, I use hash-based surrogates for idempotency."

[↑ Back to Topics](#foundational-concepts)

---

### Conformed Dimensions

**Definition:** Dimensions shared across multiple fact tables and data marts with **identical structure, values, and meaning**. The key to enterprise-wide consistency.

---

**Why Conformance Matters:**

```
Without Conformed Dimensions:              With Conformed Dimensions:
┌───────────────┐  ┌───────────────┐   ┌───────────────┐  ┌───────────────┐
│ Sales Mart    │  │ Inventory Mart│   │ Sales Mart    │  │ Inventory Mart│
│               │  │               │   │               │  │               │
│ dim_product_A │  │ dim_product_B │   │   fact_sales  │  │fact_inventory │
│ (50K products)│  │ (48K products)│   │       │       │  │       │       │
└───────────────┘  └───────────────┘   └───────┬───────┘  └───────┬───────┘
"Why don't numbers match?"                         └───────┬───────┘
                                                         │
                                                 ┌───────┴───────┐
                                                 │  dim_product  │ ← SHARED!
                                                 │ (50K products)│
                                                 └───────────────┘
                                                 Consistent everywhere!
```

---

**Example 1: Conformed Date Dimension**
```sql
-- THE most important conformed dimension
-- Used by: Sales, Inventory, HR, Finance, Marketing, Operations...

CREATE TABLE dim_date (
    date_key            INT PRIMARY KEY,      -- YYYYMMDD format
    full_date           DATE NOT NULL,
    
    -- Calendar attributes
    day_of_week         TINYINT,              -- 1-7
    day_name            VARCHAR(10),          -- 'Monday'
    day_of_month        TINYINT,
    day_of_year         SMALLINT,
    
    -- Week
    week_of_year        TINYINT,
    iso_week            TINYINT,
    
    -- Month
    month_number        TINYINT,
    month_name          VARCHAR(10),
    month_short         CHAR(3),              -- 'Jan'
    
    -- Quarter/Year
    quarter             TINYINT,
    quarter_name        CHAR(2),              -- 'Q1'
    year                SMALLINT,
    year_month          CHAR(7),              -- '2025-01'
    
    -- Fiscal calendar (company-specific)
    fiscal_year         SMALLINT,
    fiscal_quarter      TINYINT,
    
    -- Flags
    is_weekend          BOOLEAN,
    is_holiday          BOOLEAN,
    is_last_day_of_month BOOLEAN
);

-- Pre-populate for 20 years
-- Used across ALL fact tables in ALL marts
```

**Example 2: Conformed Customer Dimension**
```sql
-- Shared across: Sales, Support, Marketing, Finance
CREATE TABLE dim_customer (
    customer_key        BIGINT PRIMARY KEY,
    customer_id         VARCHAR(50),          -- Natural key
    
    -- Core attributes (agreed across all marts)
    customer_name       VARCHAR(200),
    email               VARCHAR(200),
    phone               VARCHAR(20),
    
    -- Segmentation (marketing + sales agreed on these!)
    customer_segment    VARCHAR(20),          -- 'Enterprise', 'SMB', 'Consumer'
    industry            VARCHAR(50),
    company_size        VARCHAR(20),
    
    -- Geography (conformed with dim_geography)
    city                VARCHAR(100),
    state               VARCHAR(50),
    country             VARCHAR(50),
    region              VARCHAR(20),
    
    -- SCD Type 2
    valid_from          DATE,
    valid_to            DATE,
    is_current          BOOLEAN
);
```

**Example 3: Drill-Across Query (Power of Conformance)**
```sql
-- Compare sales vs inventory vs returns for same products
-- Only possible with conformed dim_product and dim_date!

SELECT 
    d.year_month,
    p.category,
    sales.total_revenue,
    inv.avg_inventory_value,
    returns.return_rate
FROM dim_date d
CROSS JOIN dim_product p
LEFT JOIN (
    SELECT date_key, product_key, SUM(revenue) as total_revenue
    FROM fact_sales
    GROUP BY date_key, product_key
) sales ON d.date_key = sales.date_key AND p.product_key = sales.product_key
LEFT JOIN (
    SELECT date_key, product_key, AVG(inventory_value) as avg_inventory_value
    FROM fact_inventory
    GROUP BY date_key, product_key
) inv ON d.date_key = inv.date_key AND p.product_key = inv.product_key
LEFT JOIN (
    SELECT date_key, product_key, 
           SUM(return_qty) * 100.0 / NULLIF(SUM(sold_qty), 0) as return_rate
    FROM fact_returns
    GROUP BY date_key, product_key
) returns ON d.date_key = returns.date_key AND p.product_key = returns.product_key
WHERE d.year = 2025;
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Consistency** | Same customer_key = same customer everywhere |
| **Drill-across** | Combine facts from different marts |
| **Single truth** | One definition of "Enterprise customer" |
| **Easier ETL** | Build dimension once, use many times |
| **Governance** | Enforced business definitions |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Coordination** | Requires cross-team agreement |
| **Slower changes** | Must update all consumers |
| **Compromise** | Some teams may want different attributes |
| **Upfront work** | Define before building marts |

---

**Conformance Levels:**

| Level | Description | Example |
|-------|-------------|--------|
| **Identical** | Exact same table, same keys | Both marts reference same dim_date |
| **Subset** | One is subset of another | Finance uses 3 of 20 dim_date columns |
| **Rollup** | Aggregated version | dim_month derived from dim_date |
| **Shrunken** | Filtered version | US-only dim_customer for US mart |

**Interview Tip:** "Conformed dimensions are the backbone of the Kimball bus architecture. Without them, you get 'multiple versions of the truth' - Sales says 100K customers, Marketing says 95K. I always start with dim_date, dim_customer, and dim_product as enterprise-wide conformed dimensions."

[↑ Back to Topics](#foundational-concepts)

---

### Entity-Relationship Modeling

**Definition:** A graphical technique to design databases by identifying entities, attributes, and relationships. The foundation of OLTP database design.

---

**Core Components:**

| Component | Description | Notation |
|-----------|-------------|----------|
| **Entity** | Real-world object or concept | Rectangle |
| **Attribute** | Property of an entity | Oval (or listed in rectangle) |
| **Relationship** | Connection between entities | Diamond (or line) |
| **Cardinality** | How many of each entity | Crow's foot, Chen, UML notation |

---

**Cardinality Types:**

```
1:1 (One-to-One)           1:N (One-to-Many)           M:N (Many-to-Many)
┌───────┐    ┌────────┐   ┌────────┐    ┌───────┐   ┌───────┐    ┌───────┐
│Person │────│Passport│   │Customer│────┴╤┴─Order│   │Student│─┬──┬─│Course │
└───────┘    └────────┘   └────────┘        └───────┘   └───────┘ ┴  ┴ └───────┘
  1           1            1              N             M         N

Each person has             One customer can            Students take many courses
exactly one passport        place many orders           Courses have many students
```

---

**Example 1: E-Commerce ER Model**
```
┌────────────┐       ┌────────────┐       ┌────────────┐
│  CUSTOMER  │       │   ORDER    │       │ORDER_LINE │
├────────────┤       ├────────────┤       ├────────────┤
│ PK cust_id │───1──N──│ PK order_id│───1──N──│ PK line_id │
│ name       │       │ FK cust_id │       │ FK order_id│
│ email      │       │ order_date │       │ FK prod_id │
│ phone      │       │ status     │       │ quantity   │
└────────────┘       └────────────┘       │ unit_price │
                                         └──────┴─────┘
                                                │
                      ┌────────────┐              │
                      │  PRODUCT   │───1──────N──┘
                      ├────────────┤
                      │ PK prod_id │
                      │ name       │
                      │ price      │
                      │ FK cat_id  │
                      └────────────┘
```

**Example 2: SQL from ER Model**
```sql
-- Translating ER to DDL

CREATE TABLE customer (
    customer_id     INT PRIMARY KEY,
    name            VARCHAR(100) NOT NULL,
    email           VARCHAR(200) UNIQUE,
    phone           VARCHAR(20)
);

CREATE TABLE product (
    product_id      INT PRIMARY KEY,
    name            VARCHAR(100) NOT NULL,
    price           DECIMAL(10,2),
    category_id     INT REFERENCES category(category_id)
);

CREATE TABLE orders (  -- 'order' is reserved word
    order_id        INT PRIMARY KEY,
    customer_id     INT NOT NULL REFERENCES customer(customer_id),
    order_date      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status          VARCHAR(20) DEFAULT 'pending'
);

CREATE TABLE order_line (
    line_id         INT PRIMARY KEY,
    order_id        INT NOT NULL REFERENCES orders(order_id),
    product_id      INT NOT NULL REFERENCES product(product_id),
    quantity        INT NOT NULL CHECK (quantity > 0),
    unit_price      DECIMAL(10,2) NOT NULL
);
```

**Example 3: M:N Resolution (Junction Table)**
```sql
-- Many-to-Many: Students <-> Courses
-- Resolved with junction/bridge table

CREATE TABLE student (
    student_id      INT PRIMARY KEY,
    name            VARCHAR(100)
);

CREATE TABLE course (
    course_id       INT PRIMARY KEY,
    course_name     VARCHAR(100)
);

CREATE TABLE enrollment (       -- Junction table
    student_id      INT REFERENCES student,
    course_id       INT REFERENCES course,
    enrolled_date   DATE,
    grade           CHAR(2),
    PRIMARY KEY (student_id, course_id)
);
```

---

**ER vs Dimensional Modeling:**

| Aspect | ER Modeling | Dimensional Modeling |
|--------|-------------|----------------------|
| **Purpose** | OLTP (transactions) | OLAP (analytics) |
| **Optimization** | Writes (INSERT/UPDATE) | Reads (SELECT) |
| **Schema** | 3NF (normalized) | Star (denormalized) |
| **Redundancy** | Avoided | Embraced |
| **Joins** | Many (follows relationships) | Few (star pattern) |
| **Flexibility** | Schema changes easy | Schema changes hard |

---

**✅ Pros of ER Modeling:**
- No data redundancy
- Clear business rule representation
- Easy schema evolution
- ACID compliance natural

**❌ Cons:**
- Many joins for analytical queries
- Not optimized for aggregations
- Complex for business users to query

**Interview Tip:** "I use ER modeling for source OLTP systems where data integrity and transactions matter. For analytics, I transform ER models into dimensional models - entities become dimensions, transactions become facts. The ER model is the source; the dimensional model is the analytical view."

[↑ Back to Topics](#foundational-concepts)

---

## Tier 1: Modern Architecture

### Medallion Architecture

**Definition:** Lakehouse pattern organizing data into three layers based on quality and purpose.

| Layer | Purpose | Characteristics |
|-------|---------|-----------------|
| **Bronze** | Raw ingestion | Source format, append-only, full fidelity |
| **Silver** | Cleaned/Conformed | Deduplicated, typed, validated, joined |
| **Gold** | Business-ready | Aggregated, denormalized, use-case specific |

**Data Flow:**
```
Sources → Bronze (raw) → Silver (cleaned) → Gold (business marts)
```

**Key Principle:** Each layer is independently queryable and recoverable.

[↑ Back to Topics](#modern-architecture-critical-for-senior-role)

---

### Partitioning and Bucketing

**Definition:** Physical data organization strategies to optimize query performance by reducing data scanned.

---

**Partitioning vs Bucketing:**

| Aspect | Partitioning | Bucketing |
|--------|--------------|----------|
| **Mechanism** | Separate folders per value | Hash into N fixed files |
| **Cardinality** | Low (< 1000 partitions) | High (millions of values OK) |
| **Query benefit** | Partition pruning | Shuffle-free joins |
| **Best for** | Date, region, status | Customer_id, user_id |
| **Physical layout** | `/date=2025-01-01/`, `/date=2025-01-02/` | `part-0000.parquet`, `part-0001.parquet` |

---

**Example 1: Partitioning by Date (Most Common)**
```sql
-- Create partitioned table
CREATE TABLE fact_orders (
    order_id        BIGINT,
    customer_id     BIGINT,
    order_date      DATE,
    amount          DECIMAL(12,2)
)
USING DELTA
PARTITIONED BY (order_date);

-- Physical layout:
-- /fact_orders/order_date=2025-01-01/part-0000.parquet
-- /fact_orders/order_date=2025-01-02/part-0000.parquet
-- /fact_orders/order_date=2025-01-03/part-0000.parquet

-- Query with partition pruning:
SELECT SUM(amount) 
FROM fact_orders 
WHERE order_date = '2025-01-03';  -- Only reads ONE folder!

-- Spark will show: "Partition Filters: [order_date = 2025-01-03]"
```

**Example 2: Multi-Level Partitioning**
```sql
-- Partition by year, then month (hierarchical)
CREATE TABLE fact_events (
    event_id        BIGINT,
    event_type      STRING,
    event_timestamp TIMESTAMP,
    year            INT,
    month           INT
)
USING DELTA
PARTITIONED BY (year, month);

-- Physical layout:
-- /fact_events/year=2025/month=1/
-- /fact_events/year=2025/month=2/
-- /fact_events/year=2024/month=12/

-- Query January 2025:
SELECT * FROM fact_events WHERE year = 2025 AND month = 1;
```

**Example 3: Bucketing for Join Optimization**
```sql
-- Hive/Spark bucketing
CREATE TABLE fact_orders_bucketed (
    order_id        BIGINT,
    customer_id     BIGINT,
    amount          DECIMAL(12,2)
)
CLUSTERED BY (customer_id) INTO 256 BUCKETS;

CREATE TABLE dim_customer_bucketed (
    customer_id     BIGINT,
    name            STRING,
    segment         STRING
)
CLUSTERED BY (customer_id) INTO 256 BUCKETS;

-- Join is shuffle-free! (sort-merge join)
SELECT f.*, d.name, d.segment
FROM fact_orders_bucketed f
JOIN dim_customer_bucketed d ON f.customer_id = d.customer_id;

-- Both tables bucketed on customer_id with same # buckets
-- Data is pre-shuffled, no exchange needed at query time
```

---

**Z-Ordering (Multi-Dimensional Clustering):**
```sql
-- Delta Lake Z-Order
OPTIMIZE fact_orders
ZORDER BY (customer_id, product_id);

-- Co-locates data for multiple filter columns
-- Good for: WHERE customer_id = X AND product_id = Y
-- Uses space-filling curve (Z-order) to cluster

-- Limitation: Full rewrite on each OPTIMIZE
```

**Liquid Clustering (Delta Lake 3.1+):**
```sql
-- Modern alternative to Z-Order
CREATE TABLE fact_orders (
    order_id        BIGINT,
    customer_id     BIGINT,
    order_date      DATE,
    amount          DECIMAL(12,2)
)
USING DELTA
CLUSTER BY (customer_id, order_date);  -- Liquid clustering

-- Benefits over Z-Order:
-- • Incremental clustering (only new data)
-- • No full rewrite needed
-- • Can change clustering columns without rewrite
-- • Better for streaming + batch hybrid
```

---

**Anti-Patterns:**

```sql
-- ❌ TOO MANY PARTITIONS (high cardinality)
PARTITIONED BY (customer_id)  -- 10 million partitions! Slow metadata.

-- ❌ TOO FEW ROWS PER PARTITION
PARTITIONED BY (order_date, hour, store_id)  -- 1000 files with 100 rows each

-- ✅ RIGHT-SIZED
PARTITIONED BY (order_date)  -- 365 partitions/year, 1M rows each
```

---

**✅ Pros:**

| Strategy | Benefit |
|----------|--------|
| Partitioning | Partition pruning (10x-100x fewer files scanned) |
| Bucketing | Shuffle-free joins (no network I/O) |
| Z-Order | Multi-column filter optimization |
| Liquid Clustering | Incremental, flexible clustering |

**❌ Cons:**

| Strategy | Drawback |
|----------|----------|
| Partitioning | Too many small files if high cardinality |
| Bucketing | Must match bucket count for join benefit |
| Z-Order | Full table rewrite on optimize |
| Liquid Clustering | Only Delta Lake, newer feature |

---

**Decision Guide:**

| Scenario | Recommendation |
|----------|----------------|
| Filter by date frequently | Partition by date |
| Join two large tables on same key | Bucket both on that key |
| Filter by multiple columns | Z-Order or Liquid Clustering |
| Streaming + batch hybrid | Liquid Clustering |
| < 1M rows per partition | Consider coarser partitioning |

**Interview Tip:** "I partition by date for most fact tables - it's the most common filter and aligns with incremental processing. For large join-heavy workloads, I co-bucket both tables on the join key. Liquid Clustering is my new go-to for multi-column filtering because it's incremental and doesn't require periodic OPTIMIZE runs."

[↑ Back to Topics](#modern-architecture-critical-for-senior-role)

---

### Schema Evolution and Registry

**Definition:** The ability to modify table/message schemas over time while maintaining compatibility with existing consumers and data.

---

**Schema Evolution Types:**

| Change Type | Risk Level | Example |
|-------------|------------|--------|
| **Add nullable column** | ✅ Safe | `ALTER TABLE ADD COLUMN notes STRING` |
| **Add column with default** | ✅ Safe | `ALTER TABLE ADD COLUMN status STRING DEFAULT 'active'` |
| **Widen numeric type** | ✅ Safe | `INT → BIGINT` |
| **Rename column** | ⚠️ Breaking | `customer_name → cust_name` |
| **Remove column** | ⚠️ Breaking | `DROP COLUMN address` |
| **Narrow type** | ❌ Breaking | `BIGINT → INT` (data loss!) |
| **Change nullability** | ⚠️ Breaking | `NOT NULL → NULL` (safe), `NULL → NOT NULL` (breaks if NULLs exist) |

---

**Example 1: Safe Schema Evolution (Delta Lake)**
```sql
-- Original table
CREATE TABLE customers (
    customer_id     BIGINT,
    name            STRING,
    email           STRING
);

-- SAFE: Add nullable column
ALTER TABLE customers ADD COLUMN phone STRING;

-- SAFE: Add column with default
ALTER TABLE customers ADD COLUMN created_at TIMESTAMP DEFAULT current_timestamp();

-- SAFE: Widen type (enable schema evolution first)
SET spark.databricks.delta.schema.autoMerge.enabled = true;
ALTER TABLE customers ALTER COLUMN customer_id TYPE DECIMAL(20,0);

-- Old Parquet files still readable - Delta handles type coercion
```

**Example 2: Breaking Change Mitigation**
```sql
-- BREAKING: Rename column
-- Don't: ALTER TABLE RENAME COLUMN (breaks downstream)

-- DO: Add new column, populate, deprecate old
ALTER TABLE customers ADD COLUMN customer_name STRING;
UPDATE customers SET customer_name = name;
-- Notify consumers to switch
-- Later: ALTER TABLE customers DROP COLUMN name;

-- OR: Use column mapping (Delta Lake)
ALTER TABLE customers SET TBLPROPERTIES (
    'delta.columnMapping.mode' = 'name'
);
ALTER TABLE customers RENAME COLUMN name TO customer_name;
```

**Example 3: Schema Registry (Kafka/Avro)**
```python
# Schema Registry ensures producers can't break consumers

# Version 1: Original schema
schema_v1 = {
    "type": "record",
    "name": "Customer",
    "fields": [
        {"name": "customer_id", "type": "long"},
        {"name": "name", "type": "string"}
    ]
}

# Version 2: Add optional field (BACKWARD compatible)
schema_v2 = {
    "type": "record",
    "name": "Customer",
    "fields": [
        {"name": "customer_id", "type": "long"},
        {"name": "name", "type": "string"},
        {"name": "email", "type": ["null", "string"], "default": None}  # Optional!
    ]
}

# Registry validates compatibility before accepting new schema
# Modes: BACKWARD (new can read old), FORWARD (old can read new), FULL (both)
```

---

**Compatibility Modes:**

| Mode | Description | Use Case |
|------|-------------|----------|
| **BACKWARD** | New schema can read old data | Consumer upgrades first |
| **FORWARD** | Old schema can read new data | Producer upgrades first |
| **FULL** | Both directions compatible | Safest, most restrictive |
| **NONE** | No validation | Development only |

---

**✅ Pros of Schema Evolution:**
| Benefit | Description |
|---------|-------------|
| **Agility** | Evolve schema without breaking consumers |
| **Zero downtime** | No need to rewrite all data |
| **Incremental** | Add fields as requirements emerge |
| **Time travel** | Query historical data with current schema |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Complexity** | Must track compatibility rules |
| **Technical debt** | Old columns accumulate |
| **Testing burden** | Must test backward/forward compatibility |
| **Documentation** | Must track schema versions |

---

**Best Practices:**
1. **Always add nullable** - Never add NOT NULL without default
2. **Never delete immediately** - Deprecate, then remove after grace period
3. **Use schema registry** - For streaming/Kafka pipelines
4. **Version schemas** - Track changes in version control
5. **Test compatibility** - CI/CD validation before deployment

**Interview Tip:** "I treat schema evolution as a contract with consumers. Safe changes (add nullable, widen types) can deploy immediately. Breaking changes (rename, remove, narrow) require a deprecation period, consumer notification, and dual-write strategy. For streaming, Schema Registry with BACKWARD compatibility is mandatory."

[↑ Back to Topics](#modern-architecture-critical-for-senior-role)

---

## Tier 2: Advanced Dimensional Modeling

### Aggregate Tables

**Definition:** Pre-computed summary tables at a higher grain than the base fact table. Trading storage for query performance.

---

**The Problem They Solve:**
```sql
-- Dashboard query on 1 BILLION row fact table
SELECT d.month, SUM(f.revenue)
FROM fact_sales f  -- 1,000,000,000 rows
JOIN dim_date d ON f.date_key = d.date_key
GROUP BY d.month;
-- Time: 45 seconds 🐌

-- Same query on 1,200 row aggregate
SELECT month, SUM(revenue)
FROM agg_monthly_sales  -- 1,200 rows (100 months × 12 categories)
GROUP BY month;
-- Time: 50 milliseconds 🚀
```

---

**Example 1: Simple Time-Based Aggregate**
```sql
-- Base fact: Transaction grain (1B rows)
CREATE TABLE fact_sales (
    sale_key        BIGINT,
    date_key        INT,
    customer_key    BIGINT,
    product_key     BIGINT,
    store_key       INT,
    quantity        INT,
    revenue         DECIMAL(12,2),
    cost            DECIMAL(12,2)
);

-- Aggregate: Monthly grain (12K rows)
CREATE TABLE agg_monthly_sales (
    year_month      CHAR(7),        -- '2025-01'
    category        VARCHAR(50),
    region          VARCHAR(20),
    
    -- Pre-aggregated measures
    total_orders    BIGINT,
    total_quantity  BIGINT,
    total_revenue   DECIMAL(15,2),
    total_cost      DECIMAL(15,2),
    total_profit    DECIMAL(15,2),
    unique_customers BIGINT,
    
    -- Aggregate refresh tracking
    last_updated    TIMESTAMP
);

-- ETL to populate aggregate
INSERT OVERWRITE agg_monthly_sales
SELECT 
    DATE_FORMAT(d.full_date, 'yyyy-MM') as year_month,
    p.category,
    s.region,
    COUNT(*) as total_orders,
    SUM(f.quantity) as total_quantity,
    SUM(f.revenue) as total_revenue,
    SUM(f.cost) as total_cost,
    SUM(f.revenue - f.cost) as total_profit,
    COUNT(DISTINCT f.customer_key) as unique_customers,
    CURRENT_TIMESTAMP() as last_updated
FROM fact_sales f
JOIN dim_date d ON f.date_key = d.date_key
JOIN dim_product p ON f.product_key = p.product_key
JOIN dim_store s ON f.store_key = s.store_key
GROUP BY DATE_FORMAT(d.full_date, 'yyyy-MM'), p.category, s.region;
```

**Example 2: Multi-Level Aggregate Hierarchy**
```sql
-- Fact: 1B rows (transaction)
-- Agg Level 1: 10M rows (daily)
-- Agg Level 2: 100K rows (weekly)
-- Agg Level 3: 12K rows (monthly)

-- Level 1: Daily aggregate
CREATE TABLE agg_daily_sales AS
SELECT 
    date_key,
    product_key,
    store_key,
    SUM(revenue) as revenue,
    SUM(quantity) as quantity,
    COUNT(*) as transaction_count
FROM fact_sales
GROUP BY date_key, product_key, store_key;

-- Level 2: Weekly (built from daily agg!)
CREATE TABLE agg_weekly_sales AS
SELECT 
    YEAR(d.full_date) as year,
    WEEKOFYEAR(d.full_date) as week,
    a.product_key,
    a.store_key,
    SUM(a.revenue) as revenue,
    SUM(a.quantity) as quantity
FROM agg_daily_sales a
JOIN dim_date d ON a.date_key = d.date_key
GROUP BY YEAR(d.full_date), WEEKOFYEAR(d.full_date), a.product_key, a.store_key;
```

**Example 3: Aggregate with BI Tool Navigation**
```sql
-- Aggregate-aware BI tools (SSAS, Tableau, Looker) automatically choose
-- the right aggregate based on query grain

-- User query: "Show me monthly revenue by region"
-- Tool detects: matches agg_monthly_region_sales (no need to scan fact!)

-- Aggregate definition includes "navigation" metadata:
CREATE TABLE agg_monthly_region_sales (
    year_month      CHAR(7),
    region          VARCHAR(20),
    total_revenue   DECIMAL(15,2)
    -- Metadata: grain = [dim_date.month, dim_store.region]
    -- Covers: dim_date.month, dim_store.region, fact_sales.revenue
);
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Query Speed** | 10x-1000x faster for aggregate queries |
| **Dashboard Performance** | Sub-second response for BI tools |
| **Cost Savings** | Less compute for repeated queries |
| **User Experience** | Real-time feel for historical analysis |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Storage Cost** | Additional tables to maintain |
| **Freshness Lag** | Must wait for aggregate refresh |
| **ETL Complexity** | More jobs to orchestrate |
| **Consistency Risk** | Aggregate can drift from base fact |
| **Drill-through Limits** | Can't drill to transaction detail |

---

**Design Decisions:**

| Decision | Recommendation |
|----------|----------------|
| Which dimensions to include | Most common filter/group-by columns |
| Refresh frequency | Match SLA (hourly for ops, daily for reporting) |
| How many levels | 2-3 max (diminishing returns) |
| Distinct counts | Pre-calculate (expensive at query time) |
| Materialized view vs table | MV for auto-refresh; table for complex logic |

**Interview Tip:** "Aggregate tables are the first optimization I reach for when dashboard performance is slow. I identify the top 10 dashboard queries, determine their grain, and create aggregates to match. The key is balancing freshness requirements with query performance - I use incremental refresh for near-real-time needs."

[↑ Back to Topics](#advanced-dimensional-modeling)

---

### Cumulative Tables

**Definition:** Tables that maintain running totals or state that accumulates over time. Each row represents the cumulative state as of a specific point in time.

---

**Cumulative vs Event Tables:**

| Aspect | Event/Transaction Table | Cumulative Table |
|--------|------------------------|------------------|
| **Row meaning** | One event happened | State as of date |
| **Updates** | Append only | Update existing + append new |
| **Example** | "User placed order #123" | "User's lifetime orders = 47" |
| **Query pattern** | SUM(amount) for totals | Direct read for current state |

---

**Example 1: User Lifetime Metrics (Most Common)**
```sql
-- Cumulative table: One row per user, updated daily
CREATE TABLE cumulative_user_metrics (
    user_id                 BIGINT,
    as_of_date              DATE,           -- When this snapshot was taken
    
    -- Cumulative counts
    lifetime_orders         INT,            -- Running total
    lifetime_revenue        DECIMAL(12,2),  -- Running total
    lifetime_returns        INT,
    
    -- First/last events (set once, then static or updated)
    first_order_date        DATE,           -- Never changes after set
    last_order_date         DATE,           -- Updated with each new order
    first_login_date        DATE,
    last_login_date         DATE,
    
    -- Derived metrics
    days_since_first_order  INT,
    days_since_last_order   INT,
    avg_order_value         DECIMAL(10,2),
    
    PRIMARY KEY (user_id, as_of_date)
);

-- Daily ETL pattern:
MERGE INTO cumulative_user_metrics t
USING (
    SELECT 
        user_id,
        CURRENT_DATE as as_of_date,
        SUM(order_count) as lifetime_orders,
        SUM(revenue) as lifetime_revenue,
        MIN(first_order) as first_order_date,
        MAX(last_order) as last_order_date
    FROM (
        -- Previous cumulative + new events
        SELECT user_id, lifetime_orders as order_count, lifetime_revenue as revenue,
               first_order_date as first_order, last_order_date as last_order
        FROM cumulative_user_metrics
        WHERE as_of_date = CURRENT_DATE - INTERVAL 1 DAY
        UNION ALL
        SELECT user_id, COUNT(*), SUM(amount), MIN(order_date), MAX(order_date)
        FROM fact_orders
        WHERE order_date = CURRENT_DATE - INTERVAL 1 DAY
        GROUP BY user_id
    )
    GROUP BY user_id
) s
ON t.user_id = s.user_id AND t.as_of_date = s.as_of_date
WHEN MATCHED THEN UPDATE SET *
WHEN NOT MATCHED THEN INSERT *;
```

**Example 2: Product Cumulative Sales**
```sql
-- Cumulative YTD sales by product
CREATE TABLE cumulative_product_ytd (
    product_id              BIGINT,
    year                    INT,
    as_of_date              DATE,
    
    ytd_units_sold          BIGINT,
    ytd_revenue             DECIMAL(15,2),
    ytd_returns             BIGINT,
    ytd_net_revenue         DECIMAL(15,2),
    
    -- Running average
    avg_daily_units         DECIMAL(10,2),
    days_in_year_so_far     INT
);

-- Query: YTD performance comparison
SELECT 
    p.product_name,
    c2025.ytd_revenue as ytd_2025,
    c2024.ytd_revenue as ytd_2024,
    (c2025.ytd_revenue - c2024.ytd_revenue) / c2024.ytd_revenue * 100 as yoy_growth
FROM cumulative_product_ytd c2025
JOIN cumulative_product_ytd c2024 
    ON c2025.product_id = c2024.product_id
    AND c2025.as_of_date = '2025-01-03' 
    AND c2024.as_of_date = '2024-01-03'  -- Same day last year
JOIN dim_product p ON c2025.product_id = p.product_id;
```

**Example 3: Subscription State Cumulative**
```sql
-- Track subscription state over time
CREATE TABLE cumulative_subscription (
    customer_id             BIGINT,
    as_of_date              DATE,
    
    -- Current state
    subscription_status     STRING,         -- 'active', 'churned', 'paused'
    subscription_tier       STRING,         -- 'basic', 'premium', 'enterprise'
    mrr                     DECIMAL(10,2),  -- Monthly recurring revenue
    
    -- Cumulative history
    months_active           INT,
    total_payments          DECIMAL(12,2),
    times_churned           INT,
    times_upgraded          INT,
    
    -- First/last events
    subscription_start_date DATE,
    last_renewal_date       DATE,
    last_churn_date         DATE
);
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Query Performance** | No SUM() over billions of rows |
| **Point-in-Time Analysis** | "What was their lifetime value on Jan 1?" |
| **ML Features** | Pre-computed features for models |
| **Dashboard Speed** | User profile pages load instantly |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Storage Growth** | One row per entity per day |
| **ETL Complexity** | Must handle incremental + catch-up |
| **Backfill Challenges** | Rebuilding history is expensive |
| **Drift Risk** | Can diverge from source events |

---

**Storage Strategies:**

| Strategy | Description | Use Case |
|----------|-------------|----------|
| Daily snapshots | One row per entity per day | ML features, point-in-time |
| Monthly snapshots | One row per entity per month | Long-term trends |
| Latest only | One row per entity (overwritten) | Current-state dashboards |
| SCD Type 2 | New row only when values change | Dimension tracking |

**Interview Tip:** "Cumulative tables are essential for user/customer analytics and ML feature stores. Instead of computing 'lifetime value' by scanning all orders at query time, I pre-compute it daily. The key is designing the ETL to be incremental - merge yesterday's cumulative with today's events."

[↑ Back to Topics](#advanced-dimensional-modeling)

---

### Accumulating Snapshots

**Definition:** A fact table that tracks a process or workflow lifecycle with multiple milestone dates. The row is updated as the process progresses through stages.

---

**Key Characteristics:**
- One row per "thing being tracked" (order, loan, patient visit)
- Multiple date/timestamp columns for milestones
- NULL values indicate "not yet reached"
- Row is UPDATED (unlike transaction facts which are INSERT only)
- Lag measures calculated as milestones complete

---

**Example 1: Order Fulfillment Lifecycle**
```sql
CREATE TABLE fact_order_fulfillment (
    order_key               BIGINT PRIMARY KEY,
    customer_key            BIGINT,
    
    -- Milestone dates (NULLuntil reached)
    order_placed_dt         TIMESTAMP NOT NULL,  -- Always set
    payment_confirmed_dt    TIMESTAMP,           -- NULL until paid
    warehouse_picked_dt     TIMESTAMP,           -- NULL until picked
    shipped_dt              TIMESTAMP,           -- NULL until shipped
    delivered_dt            TIMESTAMP,           -- NULL until delivered
    returned_dt             TIMESTAMP,           -- NULL unless returned
    
    -- Lag measures (calculated as milestones complete)
    order_to_payment_hrs    INT,                 -- SET when payment_confirmed_dt populated
    payment_to_ship_hrs     INT,                 -- SET when shipped_dt populated
    ship_to_deliver_hrs     INT,                 -- SET when delivered_dt populated
    total_fulfillment_hrs   INT,                 -- SET when delivered_dt populated
    
    -- Current status
    current_status          VARCHAR(20),         -- 'placed','paid','shipped','delivered','returned'
    
    -- Measures at completion
    order_total             DECIMAL(12,2),
    shipping_cost           DECIMAL(8,2)
);

-- Row lifecycle:
-- Day 1: INSERT (order_placed_dt, current_status='placed')
-- Day 2: UPDATE (payment_confirmed_dt, order_to_payment_hrs, current_status='paid')
-- Day 3: UPDATE (shipped_dt, payment_to_ship_hrs, current_status='shipped')
-- Day 5: UPDATE (delivered_dt, ship_to_deliver_hrs, total_fulfillment_hrs, current_status='delivered')
```

**Example 2: Loan Application Process**
```sql
CREATE TABLE fact_loan_application (
    application_key         BIGINT PRIMARY KEY,
    applicant_key           BIGINT,
    loan_type_key           INT,
    
    -- Milestones
    application_submitted_dt    TIMESTAMP NOT NULL,
    documents_received_dt       TIMESTAMP,
    credit_check_completed_dt   TIMESTAMP,
    underwriting_completed_dt   TIMESTAMP,
    approval_decision_dt        TIMESTAMP,
    closing_dt                  TIMESTAMP,
    
    -- Outcomes at each stage
    credit_score                INT,             -- SET at credit_check
    approved_amount             DECIMAL(12,2),   -- SET at approval
    interest_rate               DECIMAL(5,3),    -- SET at approval
    final_decision              VARCHAR(20),     -- 'approved', 'denied', 'withdrawn'
    
    -- Lag measures
    submit_to_decision_days     INT,
    decision_to_close_days      INT,
    total_process_days          INT
);

-- Analysis: Where are the bottlenecks?
SELECT 
    CASE 
        WHEN closing_dt IS NOT NULL THEN 'Closed'
        WHEN approval_decision_dt IS NOT NULL THEN 'Pending Close'
        WHEN underwriting_completed_dt IS NOT NULL THEN 'In Underwriting'
        WHEN credit_check_completed_dt IS NOT NULL THEN 'Awaiting Underwriting'
        ELSE 'Early Stage'
    END as stage,
    COUNT(*) as applications,
    AVG(submit_to_decision_days) as avg_days_to_decision
FROM fact_loan_application
GROUP BY 1;
```

**Example 3: Patient Visit Workflow**
```sql
CREATE TABLE fact_patient_visit (
    visit_key               BIGINT PRIMARY KEY,
    patient_key             BIGINT,
    provider_key            BIGINT,
    facility_key            INT,
    
    -- Milestones
    scheduled_dt            TIMESTAMP,
    checked_in_dt           TIMESTAMP,
    nurse_assessment_dt     TIMESTAMP,
    doctor_seen_dt          TIMESTAMP,
    treatment_complete_dt   TIMESTAMP,
    checked_out_dt          TIMESTAMP,
    
    -- Wait time measures
    wait_for_nurse_min      INT,
    wait_for_doctor_min     INT,
    total_visit_min         INT,
    
    -- Visit outcome
    diagnosis_key           INT,
    prescription_count      INT,
    follow_up_scheduled     BOOLEAN
);
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Lifecycle Visibility** | See exactly where each item is in process |
| **Bottleneck Analysis** | Identify slow stages (avg time in underwriting) |
| **SLA Monitoring** | Track against service level agreements |
| **Funnel Analysis** | How many make it through each stage? |
| **Compact Storage** | One row per item (not one per event) |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Requires UPDATEs** | Not append-only like transaction facts |
| **ETL Complexity** | Must identify and update existing rows |
| **SCD Conflict** | Can't use Type 2 (one row = one item) |
| **Late Data** | Out-of-order events require re-processing |

---

**Implementation with Delta Lake:**
```sql
-- Use MERGE for idempotent updates
MERGE INTO fact_order_fulfillment target
USING staging_order_events source
ON target.order_key = source.order_key
WHEN MATCHED THEN UPDATE SET
    -- Only update milestone if not already set and source has value
    payment_confirmed_dt = COALESCE(target.payment_confirmed_dt, source.payment_confirmed_dt),
    shipped_dt = COALESCE(target.shipped_dt, source.shipped_dt),
    delivered_dt = COALESCE(target.delivered_dt, source.delivered_dt),
    -- Recalculate lags
    order_to_payment_hrs = CASE 
        WHEN source.payment_confirmed_dt IS NOT NULL AND target.payment_confirmed_dt IS NULL
        THEN TIMESTAMPDIFF(HOUR, target.order_placed_dt, source.payment_confirmed_dt)
        ELSE target.order_to_payment_hrs 
    END,
    current_status = source.current_status
WHEN NOT MATCHED THEN INSERT *;
```

**Interview Tip:** "Accumulating snapshots are my go-to for process/workflow tracking. Unlike transaction facts, they give a single-row view of each item's journey. The key design decision is identifying all possible milestones upfront. I use Delta Lake MERGE for updates, with COALESCE to ensure milestones are only set once."

[↑ Back to Topics](#advanced-dimensional-modeling)

---

### Bridge Tables

**Definition:** A table that resolves many-to-many relationships between a fact table and a dimension. The "connector" between entities that can't have a single foreign key.

---

**When You Need Bridge Tables:**
- Customer has multiple addresses (billing, shipping, work)
- Employee belongs to multiple departments
- Product has multiple categories
- Patient has multiple diagnoses per visit
- Order has multiple promotions applied

---

**Example 1: Customer-Address Bridge**
```sql
-- Problem: Customer can have multiple addresses
-- Can't put address_key directly in fact_orders (which one?)

CREATE TABLE dim_customer (
    customer_key    BIGINT PRIMARY KEY,
    customer_id     VARCHAR(50),
    name            VARCHAR(200)
    -- NO address here!
);

CREATE TABLE dim_address (
    address_key     BIGINT PRIMARY KEY,
    street          VARCHAR(200),
    city            VARCHAR(100),
    state           VARCHAR(50),
    zip             VARCHAR(20),
    country         VARCHAR(50)
);

CREATE TABLE bridge_customer_address (
    customer_key    BIGINT,
    address_key     BIGINT,
    address_type    VARCHAR(20),    -- 'billing', 'shipping', 'home', 'work'
    is_primary      BOOLEAN,
    effective_from  DATE,
    effective_to    DATE,
    PRIMARY KEY (customer_key, address_key, address_type)
);

-- Query: Get customer's shipping address
SELECT c.name, a.*
FROM fact_orders f
JOIN dim_customer c ON f.customer_key = c.customer_key
JOIN bridge_customer_address b ON c.customer_key = b.customer_key 
    AND b.address_type = 'shipping'
    AND b.is_primary = TRUE
JOIN dim_address a ON b.address_key = a.address_key;
```

**Example 2: Weighting Factor Bridge (Allocation)**
```sql
-- Problem: Split sales credit across multiple salespeople
-- Order $100 should be attributed 60% to Alice, 40% to Bob

CREATE TABLE bridge_order_salesperson (
    order_key           BIGINT,
    salesperson_key     BIGINT,
    weight_factor       DECIMAL(5,4),  -- Must sum to 1.0 per order
    role                VARCHAR(20),   -- 'primary', 'support', 'manager'
    PRIMARY KEY (order_key, salesperson_key)
);

-- Data:
-- order_key=1, salesperson_key=100 (Alice), weight=0.60
-- order_key=1, salesperson_key=101 (Bob), weight=0.40

-- Query: Revenue by salesperson (with proper attribution)
SELECT 
    s.salesperson_name,
    SUM(f.order_total * b.weight_factor) as attributed_revenue
FROM fact_orders f
JOIN bridge_order_salesperson b ON f.order_key = b.order_key
JOIN dim_salesperson s ON b.salesperson_key = s.salesperson_key
GROUP BY s.salesperson_name;

-- Alice: $60 (not $100!)
-- Bob: $40
```

**Example 3: Product-Promotion Bridge**
```sql
-- Problem: Multiple promotions can apply to same product
CREATE TABLE bridge_product_promotion (
    product_key         BIGINT,
    promotion_key       INT,
    discount_percent    DECIMAL(5,2),
    start_date          DATE,
    end_date            DATE,
    stacks_with_others  BOOLEAN,
    PRIMARY KEY (product_key, promotion_key, start_date)
);

-- Query: What promotions applied to each sale?
SELECT 
    f.sale_key,
    p.product_name,
    STRING_AGG(pr.promotion_name, ', ') as applied_promotions,
    SUM(b.discount_percent) as total_discount_pct
FROM fact_sales f
JOIN dim_product p ON f.product_key = p.product_key
JOIN bridge_product_promotion b ON f.product_key = b.product_key
    AND f.sale_date BETWEEN b.start_date AND b.end_date
JOIN dim_promotion pr ON b.promotion_key = pr.promotion_key
GROUP BY f.sale_key, p.product_name;
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Handles M:N** | Properly models complex relationships |
| **Allocation** | Weight factors enable proper attribution |
| **Flexibility** | Can have multiple relationship types |
| **History** | Can track relationship changes over time |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Extra Joins** | Queries become more complex |
| **Fanout Risk** | Can multiply rows unexpectedly |
| **Weight Management** | Must ensure weights sum to 1.0 |
| **BI Tool Challenges** | Some tools don't handle bridges well |

---

**Fanout Warning:**
```sql
-- Without weight factor, you'll double-count!
-- Order $100 with 2 salespeople
SELECT SUM(f.order_total) FROM fact_orders f
JOIN bridge_order_salesperson b ON f.order_key = b.order_key;
-- Result: $200 (WRONG! Counted twice)

-- With weight factor:
SELECT SUM(f.order_total * b.weight_factor)...
-- Result: $100 (CORRECT)
```

**Interview Tip:** "Bridge tables solve M:N relationships that can't be modeled with simple FKs. The critical design element is the weight factor for any revenue/quantity allocation - without it, you'll double-count. I also add validation to ensure weights sum to 1.0 per parent entity."

[↑ Back to Topics](#advanced-dimensional-modeling)

---

### Role-Playing Dimensions

**Definition:** A single dimension table that is referenced multiple times by the same fact table, each reference representing a different "role" or context.

---

**Most Common Example: Date Dimension**
```sql
-- Fact table with 3 date references
CREATE TABLE fact_order (
    order_key           BIGINT,
    
    order_date_key      INT,        -- When order was placed
    ship_date_key       INT,        -- When order shipped
    delivery_date_key   INT,        -- When order was delivered
    
    customer_key        BIGINT,
    amount              DECIMAL(12,2)
);

-- All three FKs reference the SAME dim_date table
-- But each plays a different "role"
```

---

**Example 1: Multiple Date Roles**
```sql
-- Query with aliases for each role
SELECT 
    order_date.month_name as order_month,
    ship_date.month_name as ship_month,
    delivery_date.month_name as delivery_month,
    AVG(DATEDIFF(delivery_date.full_date, order_date.full_date)) as avg_fulfillment_days
FROM fact_order f
JOIN dim_date order_date ON f.order_date_key = order_date.date_key
JOIN dim_date ship_date ON f.ship_date_key = ship_date.date_key
JOIN dim_date delivery_date ON f.delivery_date_key = delivery_date.date_key
WHERE order_date.year = 2025
GROUP BY order_date.month_name, ship_date.month_name, delivery_date.month_name;
```

**Example 2: Location/Geography Roles**
```sql
-- Flight fact with origin and destination (same airport dimension)
CREATE TABLE fact_flight (
    flight_key              BIGINT,
    departure_date_key      INT,
    
    origin_airport_key      INT,      -- Role: Origin
    destination_airport_key INT,      -- Role: Destination
    
    passengers              INT,
    revenue                 DECIMAL(12,2)
);

-- Query: Traffic between cities
SELECT 
    origin.city as from_city,
    destination.city as to_city,
    SUM(f.passengers) as total_passengers
FROM fact_flight f
JOIN dim_airport origin ON f.origin_airport_key = origin.airport_key
JOIN dim_airport destination ON f.destination_airport_key = destination.airport_key
GROUP BY origin.city, destination.city
ORDER BY total_passengers DESC;
```

**Example 3: Employee Roles**
```sql
-- Sales fact with multiple employee roles
CREATE TABLE fact_sale (
    sale_key                BIGINT,
    date_key                INT,
    
    salesperson_key         BIGINT,   -- Who sold it
    manager_key             BIGINT,   -- Who approved it
    support_rep_key         BIGINT,   -- Who provides support
    
    amount                  DECIMAL(12,2)
);

-- All three reference same dim_employee!
SELECT 
    sales.name as salesperson,
    mgr.name as manager,
    SUM(f.amount) as total_sales
FROM fact_sale f
JOIN dim_employee sales ON f.salesperson_key = sales.employee_key
JOIN dim_employee mgr ON f.manager_key = mgr.employee_key
GROUP BY sales.name, mgr.name;
```

---

**Implementation Options:**

| Approach | Pros | Cons |
|----------|------|------|
| **Table aliases in queries** | Simple, no extra objects | Users must remember aliases |
| **Views for each role** | Self-documenting | More database objects |
| **BI tool semantic layer** | Best UX | Tool-specific configuration |

---

**View Approach (Recommended):**
```sql
-- Create views for each role
CREATE VIEW dim_order_date AS 
SELECT * FROM dim_date;

CREATE VIEW dim_ship_date AS 
SELECT * FROM dim_date;

CREATE VIEW dim_delivery_date AS 
SELECT * FROM dim_date;

-- Now queries are self-documenting
SELECT * FROM fact_order f
JOIN dim_order_date d ON f.order_date_key = d.date_key;
-- Clear what 'd' represents!
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **No Duplication** | One table, multiple uses |
| **Consistent Attributes** | All dates have same calendar logic |
| **Maintenance** | Update once, applies to all roles |
| **Storage Efficient** | Don't store dim_date three times |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Query Complexity** | Multiple self-joins with aliases |
| **BI Tool Configuration** | Must define each role in tool |
| **User Confusion** | "Which date dimension is which?" |

---

**Common Role-Playing Dimensions:**
- `dim_date`: order_date, ship_date, due_date, payment_date
- `dim_employee`: salesperson, manager, approver, creator
- `dim_location`: origin, destination, billing, shipping
- `dim_customer`: buyer, seller, payer, shipper
- `dim_account`: debit_account, credit_account

**Interview Tip:** "Role-playing dimensions are a space-saver that avoids duplicating dimension tables. dim_date is the classic example - I create views like dim_order_date and dim_ship_date to make queries self-documenting. In BI tools, I configure each role as a separate logical dimension pointing to the same physical table."

[↑ Back to Topics](#advanced-dimensional-modeling)

---

### Junk Dimensions

**Definition:** A dimension that combines multiple low-cardinality flags, indicators, and codes into a single table. Cleans up the fact table by consolidating miscellaneous attributes.

---

**The Problem It Solves:**
```sql
-- BEFORE: Cluttered fact table with 6 flag columns
CREATE TABLE fact_order_cluttered (
    order_key           BIGINT,
    date_key            INT,
    customer_key        BIGINT,
    product_key         BIGINT,
    
    -- Clutter! Low-value, low-cardinality flags
    is_gift             BOOLEAN,        -- 2 values
    is_expedited        BOOLEAN,        -- 2 values
    is_first_order      BOOLEAN,        -- 2 values
    payment_type        VARCHAR(10),    -- 4 values: 'credit','debit','paypal','crypto'
    channel             VARCHAR(10),    -- 3 values: 'web','mobile','store'
    order_priority      VARCHAR(10),    -- 3 values: 'low','medium','high'
    
    amount              DECIMAL(12,2)
);
-- 6 extra columns = wider fact table, more storage
```

---

**Example 1: Order Flags Junk Dimension**
```sql
-- AFTER: Clean fact table + junk dimension
CREATE TABLE dim_order_flags (
    order_flags_key     INT PRIMARY KEY,  -- Small: 2×2×2×4×3×3 = 288 rows max
    
    is_gift             BOOLEAN,
    is_expedited        BOOLEAN,
    is_first_order      BOOLEAN,
    payment_type        VARCHAR(10),
    channel             VARCHAR(10),
    order_priority      VARCHAR(10)
);

-- Pre-populate with all combinations
INSERT INTO dim_order_flags VALUES
(1, TRUE, TRUE, TRUE, 'credit', 'web', 'high'),
(2, TRUE, TRUE, TRUE, 'credit', 'web', 'medium'),
(3, TRUE, TRUE, TRUE, 'credit', 'web', 'low'),
-- ... all 288 combinations
(288, FALSE, FALSE, FALSE, 'crypto', 'store', 'low');

-- Clean fact table
CREATE TABLE fact_order (
    order_key           BIGINT,
    date_key            INT,
    customer_key        BIGINT,
    product_key         BIGINT,
    order_flags_key     INT,              -- Single FK replaces 6 columns!
    amount              DECIMAL(12,2)
);
```

**Example 2: Transaction Codes Junk**
```sql
-- Banking transaction codes
CREATE TABLE dim_transaction_codes (
    txn_codes_key       INT PRIMARY KEY,
    
    transaction_type    VARCHAR(20),  -- 'deposit','withdrawal','transfer','fee'
    channel_type        VARCHAR(20),  -- 'atm','online','mobile','branch'
    is_international    BOOLEAN,
    is_recurring        BOOLEAN,
    fee_category        VARCHAR(10)   -- 'none','standard','premium'
);

-- Only ~160 possible combinations (4×4×2×2×5)
-- Much cleaner than 5 columns on billion-row fact table!
```

**Example 3: Generating the Junk Dimension**
```sql
-- Auto-generate all combinations
INSERT INTO dim_order_flags
WITH 
gift AS (SELECT * FROM (VALUES (TRUE), (FALSE)) t(is_gift)),
exp AS (SELECT * FROM (VALUES (TRUE), (FALSE)) t(is_expedited)),
first AS (SELECT * FROM (VALUES (TRUE), (FALSE)) t(is_first_order)),
pay AS (SELECT * FROM (VALUES ('credit'),('debit'),('paypal'),('crypto')) t(payment_type)),
chan AS (SELECT * FROM (VALUES ('web'),('mobile'),('store')) t(channel)),
prio AS (SELECT * FROM (VALUES ('low'),('medium'),('high')) t(priority))
SELECT 
    ROW_NUMBER() OVER () as order_flags_key,
    g.is_gift, e.is_expedited, f.is_first_order,
    p.payment_type, c.channel, r.priority
FROM gift g
CROSS JOIN exp e
CROSS JOIN first f
CROSS JOIN pay p
CROSS JOIN chan c
CROSS JOIN prio r;
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Cleaner Fact Table** | Replace 5-10 columns with 1 FK |
| **Smaller Facts** | 1 INT vs 5 columns = storage savings |
| **Reusability** | Same junk dim can apply to multiple facts |
| **Queryability** | Can filter/group on any flag |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Extra Join** | Must join to see flag values |
| **Combination Explosion** | Too many flags = millions of combos |
| **Maintenance** | New flag value = update junk dim |
| **Not Intuitive** | "What's order_flags_key 47?" |

---

**Guidelines:**

| Guideline | Reason |
|-----------|--------|
| Max 6-8 attributes | Avoid combination explosion |
| Each < 10 distinct values | Keep table small |
| No NULLs | Every combination pre-populated |
| Include "Unknown" row | For missing/unmapped values |
| Group logically | Order flags separate from shipping flags |

**Interview Tip:** "Junk dimensions are great for consolidating miscellaneous flags that would otherwise clutter the fact table. I pre-generate all combinations upfront - typically a few hundred rows max. During ETL, I lookup the junk key based on the flag values. It's especially valuable for wide fact tables with many low-cardinality attributes."

[↑ Back to Topics](#advanced-dimensional-modeling)

---

### Degenerate Dimensions

**Definition:** A dimension attribute that lives directly in the fact table instead of a separate dimension table. The value IS the dimension - there's nothing else to say about it.

---

**Key Characteristics:**
- No separate dimension table
- Stored in the fact table itself
- Used primarily for drill-through and identification
- No descriptive attributes (if there were, it would be a real dimension)

---

**Example 1: Order Number (Classic)**
```sql
-- Order number has no descriptive attributes
-- It's just an identifier for grouping/drill-through

CREATE TABLE fact_order_line (
    order_line_key      BIGINT,
    order_number        VARCHAR(20),    -- DEGENERATE! No dim_order table
    
    date_key            INT,
    customer_key        BIGINT,
    product_key         BIGINT,
    
    quantity            INT,
    amount              DECIMAL(12,2)
);

-- Query: Get all line items for a specific order
SELECT * FROM fact_order_line WHERE order_number = 'ORD-2025-001234';

-- Query: Group by order (degenerate dimension acts as grouping key)
SELECT 
    order_number,
    COUNT(*) as line_count,
    SUM(amount) as order_total
FROM fact_order_line
GROUP BY order_number;
```

**Example 2: Invoice Number**
```sql
CREATE TABLE fact_invoice_line (
    invoice_line_key    BIGINT,
    invoice_number      VARCHAR(20),    -- Degenerate: no dim_invoice
    invoice_line_number INT,            -- Degenerate: just a sequence
    
    date_key            INT,
    customer_key        BIGINT,
    product_key         BIGINT,
    
    quantity            INT,
    unit_price          DECIMAL(10,2),
    line_total          DECIMAL(12,2)
);
```

**Example 3: Transaction ID / Confirmation Number**
```sql
CREATE TABLE fact_payment (
    payment_key             BIGINT,
    transaction_id          VARCHAR(50),    -- Degenerate: unique identifier
    confirmation_number     VARCHAR(30),    -- Degenerate: receipt reference
    
    date_key                INT,
    customer_key            BIGINT,
    payment_method_key      INT,
    
    amount                  DECIMAL(12,2)
);

-- Used for: Support tickets, refund lookups, audit trails
-- "Customer called about transaction ABC123 - what happened?"
SELECT * FROM fact_payment WHERE transaction_id = 'ABC123';
```

---

**Degenerate vs Real Dimension:**

| Aspect | Degenerate | Real Dimension |
|--------|------------|----------------|
| **Table** | In fact table | Separate table |
| **Attributes** | Just the ID | Multiple descriptive attributes |
| **Example** | order_number | customer (name, email, segment) |
| **Purpose** | Drill-through, grouping | Slicing, filtering, analysis |

---

**When It SHOULD Be a Real Dimension:**
```sql
-- If order has attributes beyond the ID...
-- Order date? → That's dim_date
-- Order status? Order priority? Shipping method?
-- Then you NEED dim_order!

CREATE TABLE dim_order (
    order_key           BIGINT PRIMARY KEY,
    order_number        VARCHAR(20),        -- Natural key
    order_status        VARCHAR(20),        -- Attribute
    order_priority      VARCHAR(10),        -- Attribute
    shipping_method     VARCHAR(20),        -- Attribute
    created_timestamp   TIMESTAMP           -- Attribute
);

-- Now fact_order_line references dim_order
CREATE TABLE fact_order_line (
    order_line_key      BIGINT,
    order_key           BIGINT,             -- FK to dim_order
    -- order_number moved to dim_order
    ...
);
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Simplicity** | No extra table to maintain |
| **No Joins** | Direct access for lookups |
| **Storage** | Avoids dimension table overhead |
| **Natural Grouping** | Easy to aggregate by order |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **No Attributes** | Can't add order-level info later |
| **Redundancy** | Order number repeated per line |
| **Indexing** | May need index on fact table |
| **Design Lock-in** | Hard to convert to dimension later |

---

**Common Degenerate Dimensions:**
- Order number / ID
- Invoice number
- Transaction ID
- Confirmation number
- Receipt number
- Shipment tracking number
- Ticket number
- Check number

**Interview Tip:** "Degenerate dimensions are identifiers without attributes - they exist solely for drill-through and grouping. Order number is the classic example. If I later need order-level attributes (status, priority), I'll promote it to a real dimension. The key question: 'Does this ID have any descriptive attributes?' If no, degenerate. If yes, real dimension."

[↑ Back to Topics](#advanced-dimensional-modeling)

---

### Outrigger Dimensions

**Definition:** A secondary dimension that is referenced by another dimension (not directly by the fact table). It "hangs off" a primary dimension like an outrigger on a canoe.

---

**Visual:**
```
                    Standard Star Schema:
                    ┌──────────────┐
                    │ dim_customer │
                    │ city         │
                    │ state        │  ← Geography embedded
                    │ country      │
                    └──────┬───────┘
                           │
                    ┌──────┴───────┐
                    │  fact_sales  │
                    └──────────────┘

                    With Outrigger:
                    ┌──────────────┐    ┌──────────────┐
                    │ dim_customer │───►│dim_geography │  ← Outrigger!
                    │ geography_key│    │ city         │
                    └──────┬───────┘    │ state        │
                           │         │ country      │
                    ┌──────┴───────┐    └──────────────┘
                    │  fact_sales  │
                    └──────────────┘
```

---

**Example 1: Shared Geography Outrigger**
```sql
-- Outrigger: Shared geography for multiple dimensions
CREATE TABLE dim_geography (
    geography_key       INT PRIMARY KEY,
    city                VARCHAR(100),
    state               VARCHAR(50),
    state_abbrev        CHAR(2),
    country             VARCHAR(50),
    region              VARCHAR(20),
    timezone            VARCHAR(30),
    lat                 DECIMAL(10,6),
    lng                 DECIMAL(10,6)
);

-- Customer references geography
CREATE TABLE dim_customer (
    customer_key        BIGINT PRIMARY KEY,
    customer_id         VARCHAR(50),
    name                VARCHAR(200),
    email               VARCHAR(200),
    geography_key       INT REFERENCES dim_geography  -- Outrigger!
);

-- Store references SAME geography
CREATE TABLE dim_store (
    store_key           INT PRIMARY KEY,
    store_id            VARCHAR(20),
    store_name          VARCHAR(100),
    manager_name        VARCHAR(100),
    geography_key       INT REFERENCES dim_geography  -- Same outrigger!
);

-- Employee references SAME geography
CREATE TABLE dim_employee (
    employee_key        BIGINT PRIMARY KEY,
    employee_id         VARCHAR(20),
    name                VARCHAR(100),
    department          VARCHAR(50),
    geography_key       INT REFERENCES dim_geography  -- Same outrigger!
);
```

**Example 2: Date Outrigger for SCD**
```sql
-- Some dimensions have date attributes that could reference dim_date
CREATE TABLE dim_employee (
    employee_key        BIGINT PRIMARY KEY,
    employee_id         VARCHAR(20),
    name                VARCHAR(100),
    hire_date_key       INT REFERENCES dim_date,      -- Outrigger to date!
    termination_date_key INT REFERENCES dim_date,
    birth_date_key      INT REFERENCES dim_date
);

-- Now you can analyze by hire_date calendar attributes
SELECT 
    hire_date.year as hire_year,
    hire_date.quarter as hire_quarter,
    COUNT(*) as employees_hired
FROM dim_employee e
JOIN dim_date hire_date ON e.hire_date_key = hire_date.date_key
GROUP BY hire_date.year, hire_date.quarter;
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Shared Reference** | Same geography for customer, store, employee |
| **Consistency** | All dimensions use same geography values |
| **Storage Savings** | Don't duplicate city/state across dimensions |
| **Single Update** | Fix city name once, applies everywhere |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Extra Join** | Fact → Dim → Outrigger = 2 joins |
| **Query Complexity** | Users must know about outrigger |
| **BI Tool Issues** | Some tools don't traverse outriggers |
| **Snowflaking** | Moves away from pure star schema |

---

**When to Use vs When to Denormalize:**

| Use Outrigger When | Denormalize When |
|-------------------|------------------|
| Attribute is shared across 3+ dimensions | Used by only 1-2 dimensions |
| Frequent updates to shared attributes | Rarely changes |
| Storage is a concern | Query simplicity matters more |
| Strict data governance | Self-service BI users |

---

**Query with Outrigger:**
```sql
-- Extra join needed to get geography
SELECT 
    g.state,
    g.region,
    SUM(f.amount) as revenue
FROM fact_sales f
JOIN dim_customer c ON f.customer_key = c.customer_key
JOIN dim_geography g ON c.geography_key = g.geography_key  -- Outrigger join!
GROUP BY g.state, g.region;

-- Compare to denormalized (no outrigger):
SELECT c.state, c.region, SUM(f.amount)
FROM fact_sales f
JOIN dim_customer c ON f.customer_key = c.customer_key
GROUP BY c.state, c.region;
-- One less join!
```

**Interview Tip:** "Outriggers are useful when the same attributes (like geography) are shared across multiple dimensions. Rather than duplicating city/state/country in dim_customer, dim_store, and dim_employee, I extract to dim_geography and reference it. The trade-off is an extra join at query time, so I only use outriggers when the sharing benefit outweighs the join cost."

[↑ Back to Topics](#advanced-dimensional-modeling)

---

## Tier 2: Data Vault & Advanced Patterns

### Data Vault Modeling

**Definition:** Enterprise DW methodology with three components:

| Component | Purpose | Characteristics |
|-----------|---------|-----------------|
| **Hub** | Business keys | customer_id, product_id (immutable) |
| **Link** | Relationships | customer↔order connection |
| **Satellite** | Attributes + History | Full history of all changes |

**When to Use:** Multiple sources, regulatory audit needs, agile/unknown requirements.

[↑ Back to Topics](#data-vault--advanced-patterns)

---

### Idempotent Data Processing

**Definition:** Processing that produces the same result regardless of how many times it runs. If you run the same ETL job twice, you get the same output - not double the data.

---

**Why It Matters:**
```
Without Idempotency:                    With Idempotency:
Run 1: 100 rows inserted                Run 1: 100 rows inserted
Run 2: 100 rows inserted (duplicate!)   Run 2: 0 rows (already exists) ✓
Run 3: 100 rows inserted (triplicate!)  Run 3: 0 rows (already exists) ✓
Result: 300 rows (WRONG!)               Result: 100 rows (CORRECT!)
```

---

**Pattern 1: DELETE + INSERT by Partition**
```sql
-- IDEMPOTENT: Delete existing data for time period, then insert fresh
-- Safe to re-run for same date

-- Step 1: Delete existing
DELETE FROM fact_sales 
WHERE date_partition = '2025-01-03';

-- Step 2: Insert fresh
INSERT INTO fact_sales
SELECT * FROM staging_sales 
WHERE sale_date = '2025-01-03';

-- Running twice? First delete removes data, insert adds same data back
-- Net result: Same 100 rows, no duplicates!
```

**Pattern 2: MERGE (Upsert) on Business Key**
```sql
-- IDEMPOTENT: Update if exists, insert if not
MERGE INTO dim_customer target
USING staging_customer source
ON target.customer_id = source.customer_id  -- Business key!
WHEN MATCHED THEN 
    UPDATE SET 
        name = source.name,
        email = source.email,
        updated_at = current_timestamp()
WHEN NOT MATCHED THEN 
    INSERT (customer_id, name, email, created_at)
    VALUES (source.customer_id, source.name, source.email, current_timestamp());

-- Running twice? Second run just updates with same values
-- No duplicates possible!
```

**Pattern 3: Delta Lake replaceWhere**
```sql
-- IDEMPOTENT: Atomic replace of partition
INSERT INTO fact_sales
SELECT * FROM staging_sales
WHERE sale_date = '2025-01-03';

-- Using Delta Lake replaceWhere (transactional)
df.write.format("delta") \
  .mode("overwrite") \
  .option("replaceWhere", "date_partition = '2025-01-03'") \
  .save("/path/to/fact_sales")

-- Atomic: Either fully succeeds or fully fails
-- Running twice? Replaces with identical data
```

**Pattern 4: Deterministic Surrogate Keys**
```sql
-- ❌ NOT IDEMPOTENT: Auto-increment creates different keys each run
INSERT INTO fact_sales (sale_key, ...)
VALUES (NEXTVAL('sale_seq'), ...);
-- Run 1: sale_key = 1001
-- Run 2: sale_key = 1002 (duplicate row with different key!)

-- ✅ IDEMPOTENT: Hash-based key is deterministic
INSERT INTO fact_sales (sale_key, ...)
SELECT 
    MD5(CONCAT(order_id, '|', line_number)) as sale_key,  -- Same input = same key!
    ...
FROM staging_sales;
-- Run 1: sale_key = 'abc123'
-- Run 2: sale_key = 'abc123' (MERGE can detect duplicate!)
```

---

**Idempotency Patterns Summary:**

| Pattern | Best For | How It Works |
|---------|----------|-------------|
| DELETE + INSERT | Fact tables with partitions | Clear partition, reload fresh |
| MERGE (Upsert) | Dimensions, any keyed table | Update existing, insert new |
| replaceWhere | Delta Lake partitioned tables | Atomic partition replacement |
| Deterministic keys | All tables | Same input always produces same key |

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Retry Safety** | Job fails? Just re-run, no cleanup needed |
| **Backfill** | Re-run historical dates without fear |
| **Debugging** | Run same job in dev/prod, compare results |
| **Orchestration** | Airflow/Dagster can retry automatically |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Performance** | MERGE slower than raw INSERT |
| **Complexity** | Must design for idempotency upfront |
| **Key Design** | Requires stable business keys |

---

**Common Mistakes:**
```sql
-- ❌ Using auto-increment in ETL (not idempotent)
INSERT INTO fact (id, ...) VALUES (NEXTVAL, ...);

-- ❌ Appending without dedup check
INSERT INTO fact SELECT * FROM staging;  -- Duplicates on re-run!

-- ❌ Relying on current_timestamp() in keys
MD5(CONCAT(order_id, current_timestamp()));  -- Different each run!
```

**Interview Tip:** "Every ETL job I write must be idempotent. My go-to patterns: DELETE+INSERT by partition for facts, MERGE for dimensions, and hash-based surrogate keys instead of auto-increment. This makes retries, backfills, and debugging trivial. If Airflow retries a failed task, I want zero data quality issues."

[↑ Back to Topics](#data-vault--advanced-patterns)

---

### Temporal Cardinality Explosion

**Definition:** An unintended row multiplication that occurs when joining a fact table to an SCD Type 2 dimension without proper date constraints. Each fact row matches EVERY historical version of the dimension.

---

**The Problem Visualized:**
```
Fact Table (1M orders):              SCD Type 2 Dimension:
┌───────────┬──────────────┐      ┌───────────┬────────┬──────────┐
│ order_id  │ customer_id  │      │customer_id│ segment│valid_from│
├───────────┼──────────────┤      ├───────────┼────────┼──────────┤
│ 1         │ C001         │      │ C001      │ Bronze │ 2020-01  │
│ 2         │ C001         │      │ C001      │ Silver │ 2022-01  │
│ ...       │ ...          │      │ C001      │ Gold   │ 2024-01  │
└───────────┴──────────────┘      └───────────┴────────┴──────────┘

WRONG JOIN (no date constraint):
Order 1 (C001) × 3 versions = 3 rows
Order 2 (C001) × 3 versions = 3 rows
...
1M orders × 3 versions = 3M rows!  (EXPLODED!)
```

---

**Example 1: The Bug**
```sql
-- ❌ WRONG: Joins to ALL historical versions
SELECT 
    f.order_id,
    f.amount,
    d.segment
FROM fact_orders f
JOIN dim_customer d ON f.customer_id = d.customer_id;

-- Result: 10M rows instead of 1M!
-- Each order appears multiple times with different segments
-- SUM(amount) is now 10x the actual total!
```

**Example 2: The Fix**
```sql
-- ✅ CORRECT: Point-in-time join with date constraints
SELECT 
    f.order_id,
    f.order_date,
    f.amount,
    d.segment  -- Segment as of order date
FROM fact_orders f
JOIN dim_customer d ON f.customer_id = d.customer_id
    AND f.order_date >= d.valid_from
    AND f.order_date < d.valid_to;  -- Or: f.order_date < COALESCE(d.valid_to, '9999-12-31')

-- Result: Exactly 1M rows (one per order)
-- Each order joined to the CORRECT segment at that time
```

**Example 3: Using is_current for Current State Only**
```sql
-- If you only need current dimension values (not point-in-time):
SELECT 
    f.order_id,
    f.amount,
    d.segment  -- Current segment (may differ from order-time segment)
FROM fact_orders f
JOIN dim_customer d ON f.customer_id = d.customer_id
    AND d.is_current = TRUE;  -- Only join to current version

-- Result: 1M rows, but segment reflects TODAY's value
-- Use for: current customer status, not historical analysis
```

---

**Detection:**
```sql
-- Check for explosion by comparing counts
SELECT 'Fact only', COUNT(*) FROM fact_orders
UNION ALL
SELECT 'Joined', COUNT(*) FROM fact_orders f
JOIN dim_customer d ON f.customer_id = d.customer_id;

-- If joined count >> fact count, you have explosion!
```

---

**✅ Correct Patterns:**

| Scenario | Join Condition |
|----------|---------------|
| Point-in-time analysis | `fact.date >= dim.valid_from AND fact.date < dim.valid_to` |
| Current state only | `dim.is_current = TRUE` |
| Specific date | `dim.valid_from <= '2025-01-01' AND dim.valid_to > '2025-01-01'` |
| Latest version | `dim.valid_to = '9999-12-31'` |

---

**❌ Common Mistakes:**
- Forgetting date constraints in ad-hoc queries
- BI tools auto-generating joins without temporal logic
- Testing with small data (explosion not noticed)
- Using `<=` instead of `<` for valid_to (double-counts boundary day)

---

**Prevention Strategies:**
1. **Views:** Create `v_fact_orders_with_customer` that includes correct join
2. **BI Layer:** Configure Tableau/Looker relationships with date constraints
3. **Documentation:** Comment all SCD2 dimensions clearly
4. **Testing:** Always verify row counts before and after join

**Interview Tip:** "Temporal cardinality explosion is a silent killer in DW queries. I always add point-in-time join constraints for SCD2 dimensions: `fact.date >= dim.valid_from AND fact.date < dim.valid_to`. In BI tools, I preconfigure these relationships so users can't accidentally create fanouts."

[↑ Back to Topics](#data-vault--advanced-patterns)

---

## Tier 2: Design Tradeoffs

### Knowing Your Data Consumers

**Definition:** The most critical design decision - understanding WHO will use your data and HOW determines every modeling choice.

---

**Consumer Profiles:**

| Consumer | Skill Level | Query Style | Needs | Best Model |
|----------|------------|-------------|-------|------------|
| **Business Analysts** | SQL basics | Ad-hoc, exploratory | Simple queries, self-service | Star schema, wide tables |
| **Data Scientists** | Advanced | Complex, iterative | Features, full history | Flat/wide tables, raw data |
| **Executives** | None | Dashboards only | KPIs, trends | Pre-aggregated, materialized |
| **Applications/APIs** | N/A (code) | Key-value, real-time | Low latency, caching | Denormalized, indexed |
| **Data Engineers** | Expert | Complex transformations | Raw data, flexibility | Normalized, partitioned |

---

**Example 1: Business Analyst Optimization**
```sql
-- What analysts need: Simple queries, self-documenting columns
-- They write queries like this:
SELECT 
    month,
    product_category,
    customer_segment,
    SUM(revenue) as total_revenue
FROM sales_summary  -- Wide, denormalized table
WHERE year = 2025
GROUP BY month, product_category, customer_segment;

-- NOT like this (too many joins):
SELECT d.month, p.category, c.segment, SUM(f.amount)
FROM fact_sales f
JOIN dim_date d ON f.date_key = d.date_key
JOIN dim_product p ON f.product_key = p.product_key
JOIN dim_customer c ON f.customer_key = c.customer_key
WHERE d.year = 2025
GROUP BY d.month, p.category, c.segment;
```

**Example 2: Data Science Optimization**
```sql
-- What data scientists need: Flat tables for feature engineering
CREATE TABLE ml_customer_features AS
SELECT 
    customer_id,
    -- Behavioral features
    lifetime_orders,
    lifetime_revenue,
    avg_order_value,
    days_since_first_order,
    days_since_last_order,
    order_frequency_30d,
    order_frequency_90d,
    -- Engagement features
    website_visits_30d,
    email_opens_30d,
    support_tickets_90d,
    -- Labels
    churned_next_30d
FROM customer_360;  -- One row per customer, all features

-- They'll load into Python: df = spark.table("ml_customer_features").toPandas()
```

**Example 3: Executive Dashboard Optimization**
```sql
-- What executives need: Pre-computed KPIs, instant refresh
CREATE TABLE exec_daily_kpis AS
SELECT 
    report_date,
    -- Top-line metrics
    total_revenue,
    total_orders,
    new_customers,
    -- Comparisons
    revenue_vs_yesterday_pct,
    revenue_vs_last_week_pct,
    revenue_vs_last_year_pct,
    -- Goals
    revenue_mtd,
    revenue_mtd_target,
    revenue_mtd_vs_target_pct
FROM compute_daily_kpis();

-- Dashboard just reads single row: SELECT * WHERE report_date = TODAY
```

---

**Key Questions to Ask:**

| Question | Why It Matters |
|----------|----------------|
| Who is the primary user? | SQL skill level, tool preferences |
| What questions do they ask? | Defines grain and dimensions needed |
| How often do they query? | Performance requirements |
| What latency is acceptable? | Real-time vs batch decisions |
| Will they drill through? | Need transaction-level detail? |
| Do they need history? | SCD Type 2 vs Type 1 |
| What tools do they use? | Tableau/Power BI vs Jupyter |

---

**✅ Pros of Consumer-Centric Design:**
| Benefit | Description |
|---------|-------------|
| **Adoption** | Users actually use the data |
| **Fewer Tickets** | Self-service reduces support burden |
| **Performance** | Optimized for actual query patterns |
| **Trust** | Users get correct answers easily |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Multiple Tables** | Different consumers may need different views |
| **Maintenance** | More models to keep in sync |
| **Scope Creep** | "Can you add one more column?" requests |

**Interview Tip:** "I always start with 'Who will use this?' before designing a data model. A model for business analysts (star schema, simple queries) looks very different from one for ML engineers (flat features, raw access). I often build a Silver layer for flexibility and multiple Gold layers optimized for each consumer type."

[↑ Back to Topics](#design-tradeoffs)

---

### Compactness vs Usability

**Definition:** The tradeoff between storage-efficient coded values and human-readable descriptive values.

---

**Comparison:**

| Aspect | Compact (Codes) | Usable (Strings) |
|--------|-----------------|------------------|
| **Storage** | 1-4 bytes | 10-100 bytes |
| **Example** | `payment_type_code = 1` | `payment_type = 'Credit Card'` |
| **GROUP BY** | Faster (integer comparison) | Slower (string comparison) |
| **Readability** | Needs lookup | Self-documenting |
| **BI Tools** | Requires mapping | Works directly |
| **Compression** | Already small | Compresses well in columnar |

---

**Example 1: Payment Type**
```sql
-- COMPACT: Codes in fact table
CREATE TABLE fact_transactions_compact (
    txn_id              BIGINT,
    payment_type_code   TINYINT,  -- 1=Credit, 2=Debit, 3=ACH, 4=Wire
    amount              DECIMAL(12,2)
);

-- Lookup table
CREATE TABLE lkp_payment_type (
    payment_type_code   TINYINT PRIMARY KEY,
    payment_type_name   VARCHAR(20)
);

-- Query requires join:
SELECT l.payment_type_name, SUM(f.amount)
FROM fact_transactions_compact f
JOIN lkp_payment_type l ON f.payment_type_code = l.payment_type_code
GROUP BY l.payment_type_name;
```

```sql
-- USABLE: Strings directly in table
CREATE TABLE fact_transactions_usable (
    txn_id              BIGINT,
    payment_type        VARCHAR(20),  -- 'Credit Card', 'Debit Card', etc.
    amount              DECIMAL(12,2)
);

-- Query is simpler:
SELECT payment_type, SUM(amount)
FROM fact_transactions_usable
GROUP BY payment_type;
```

**Example 2: Status Codes**
```sql
-- In Bronze (from source): status_code = 'A', 'P', 'C', 'X'
-- In Gold (for users): order_status = 'Active', 'Pending', 'Completed', 'Cancelled'

-- Transformation in Silver:
CASE source.status_code
    WHEN 'A' THEN 'Active'
    WHEN 'P' THEN 'Pending'
    WHEN 'C' THEN 'Completed'
    WHEN 'X' THEN 'Cancelled'
    ELSE 'Unknown'
END as order_status
```

---

**Modern Reality: Columnar Compression**
```
In columnar storage (Parquet, Delta, ORC):
• Repeated strings compress extremely well
• 'Credit Card' repeated 1M times ≈ stored once + dictionary
• Storage difference is often negligible
• Query speed similar after compression

Result: Usability often wins in modern warehouses!
```

---

**✅ When to Use Codes:**
- Source system uses codes (preserve in Bronze)
- Extreme storage constraints (rare today)
- High-frequency updates on codes
- Internal technical values (not user-facing)

**✅ When to Use Strings:**
- User-facing tables (Gold layer)
- Self-service BI
- Report columns
- Anything business users will see

---

**Best Practice: Layer-Based Approach**
```
Bronze: status_code = 'A'           (preserve source format)
Silver: status_code = 'A'           (validated, but still codes)
Gold:   order_status = 'Active'     (human-readable for BI)
```

**Interview Tip:** "With modern columnar storage, the storage argument for codes is largely obsolete - strings compress well. I prefer human-readable values in Gold layer for self-service BI. I keep codes in Bronze/Silver for flexibility, but transform to readable strings for business users."

[↑ Back to Topics](#design-tradeoffs)

---

### Wide vs Narrow Tables

**Definition:** The fundamental tradeoff between normalized (narrow) tables with many joins vs denormalized (wide) tables with few joins.

---

**Comparison:**

| Aspect | Narrow (Normalized) | Wide (Denormalized) |
|--------|---------------------|---------------------|
| **Structure** | Many tables, 5-20 columns each | Few tables, 50-500+ columns |
| **Joins Required** | Multiple | None/Few |
| **Schema Changes** | Add table/column easily | Schema evolution complex |
| **Redundancy** | None | High (intentional) |
| **Write Performance** | Fast (update one place) | Slow (update many columns) |
| **Read Performance** | Slower (joins) | Fast (no joins) |
| **Columnar Storage** | Less efficient | Very efficient |

---

**Example 1: Order Data**
```sql
-- NARROW: 5 normalized tables
-- fact_orders (order_key, customer_key, date_key, amount)
-- dim_customer (customer_key, name, email, segment, region_key)
-- dim_region (region_key, region_name, country)
-- dim_product (product_key, product_name, category_key)
-- dim_category (category_key, category_name)

-- Query requires 4+ joins:
SELECT c.name, r.region_name, p.product_name, cat.category_name, f.amount
FROM fact_orders f
JOIN dim_customer c ON f.customer_key = c.customer_key
JOIN dim_region r ON c.region_key = r.region_key
JOIN dim_product p ON f.product_key = p.product_key
JOIN dim_category cat ON p.category_key = cat.category_key;
```

```sql
-- WIDE: Single denormalized table
CREATE TABLE orders_wide (
    order_id, order_date, order_amount,
    customer_name, customer_email, customer_segment,
    region_name, country,
    product_name, category_name,
    -- 50+ more columns...
);

-- Query is simple:
SELECT customer_name, region_name, product_name, category_name, order_amount
FROM orders_wide;
```

**Example 2: Customer 360**
```sql
-- WIDE: Customer 360 view (ML-ready)
CREATE TABLE customer_360 AS
SELECT 
    -- Identity
    c.customer_id, c.name, c.email, c.segment,
    -- Location
    c.city, c.state, c.country, c.region,
    -- Lifetime metrics
    SUM(o.amount) as lifetime_revenue,
    COUNT(o.order_id) as lifetime_orders,
    AVG(o.amount) as avg_order_value,
    -- Recency
    MAX(o.order_date) as last_order_date,
    MIN(o.order_date) as first_order_date,
    DATEDIFF(CURRENT_DATE, MAX(o.order_date)) as days_since_last_order,
    -- Frequency
    COUNT(CASE WHEN o.order_date >= CURRENT_DATE - 30 THEN 1 END) as orders_30d,
    COUNT(CASE WHEN o.order_date >= CURRENT_DATE - 90 THEN 1 END) as orders_90d
FROM dim_customer c
LEFT JOIN fact_orders o ON c.customer_key = o.customer_key
GROUP BY c.customer_id, c.name, c.email, c.segment, c.city, c.state, c.country, c.region;
```

---

**Decision Guide:**

| If... | Then Use |
|-------|----------|
| Data changes frequently | Narrow (update one place) |
| Read-heavy workload | Wide (no joins) |
| Unknown future queries | Narrow (flexible) |
| Specific dashboard | Wide (optimized) |
| BI self-service | Wide (simple queries) |
| ML feature store | Wide (denormalized features) |

---

**✅ Pros of Wide Tables:**
| Benefit | Description |
|---------|-------------|
| **Query Speed** | No joins = faster queries |
| **Simplicity** | Business users can self-serve |
| **Columnar Optimization** | Parquet/Delta compress well |
| **Predictable Performance** | No join explosion |

**❌ Cons of Wide Tables:**
| Drawback | Description |
|----------|-------------|
| **Schema Evolution** | Adding columns requires rebuild |
| **Data Freshness** | Must rebuild entire table on source updates |
| **Storage** | Redundant data (compressed, but still) |
| **Write Latency** | Updates are expensive |

**Interview Tip:** "I use narrow tables in Silver for flexibility and wide tables in Gold for performance. The Medallion architecture gives you both - normalized for engineering, denormalized for analytics."

[↑ Back to Topics](#design-tradeoffs)

---

### One Big Table Pattern (OBT)

**Definition:** A single, fully denormalized table containing all facts and dimension attributes in one place. Zero joins, maximum query simplicity.

---

**Architecture:**
```
Traditional (Star):              One Big Table (OBT):
┌───────────┐                      ┌────────────────────────┐
│ dim_date  │                      │  orders_obt             │
└─────┬─────┘                      ├────────────────────────┤
      │    ┌───────────┐           │ order_id               │
      └────┤fact_sales├────       │ order_date, year, month│
           └────┬──────┘           │ customer_id, name, seg │
┌─────────┬───┘                    │ product_id, name, cat  │
│dim_cust│                         │ store_id, store_name   │
└─────────┘  ┌───────────┐           │ amount, quantity, cost │
              │dim_product│           │ ... (100+ columns)     │
              └───────────┘           └────────────────────────┘
(4 tables, 3 joins)               (1 table, 0 joins)
```

---

**Example 1: E-commerce OBT**
```sql
CREATE TABLE orders_obt AS
SELECT 
    -- Fact measures
    f.order_id,
    f.order_date,
    f.quantity,
    f.unit_price,
    f.discount_amount,
    f.net_amount,
    f.shipping_cost,
    
    -- Date dimension (flattened)
    d.year, d.quarter, d.month, d.week_of_year,
    d.day_of_week, d.is_weekend, d.is_holiday,
    
    -- Customer dimension (flattened)
    c.customer_id, c.customer_name, c.customer_email,
    c.segment, c.tier, c.lifetime_value,
    c.city, c.state, c.country, c.region,
    
    -- Product dimension (flattened)
    p.product_id, p.product_name, p.brand,
    p.category, p.subcategory, p.department,
    p.unit_cost, p.supplier_id,
    
    -- Store dimension (flattened)
    s.store_id, s.store_name, s.store_type,
    s.store_city, s.store_state
    
FROM fact_orders f
JOIN dim_date d ON f.date_key = d.date_key
JOIN dim_customer c ON f.customer_key = c.customer_key
JOIN dim_product p ON f.product_key = p.product_key
JOIN dim_store s ON f.store_key = s.store_key;
```

**Example 2: Querying OBT (Zero Joins!)**
```sql
-- All queries are simple scans
SELECT 
    year, month, region, category,
    SUM(net_amount) as revenue,
    COUNT(DISTINCT customer_id) as unique_customers
FROM orders_obt
WHERE year = 2025
GROUP BY year, month, region, category;

-- Filters push down efficiently in columnar storage
```

**Example 3: ML Feature Table (OBT Variant)**
```sql
-- Feature store as OBT
CREATE TABLE customer_features_obt AS
SELECT 
    customer_id,
    -- Static attributes
    customer_segment, customer_tier, customer_region,
    -- Aggregated features
    lifetime_orders, lifetime_revenue, avg_order_value,
    orders_30d, orders_90d, orders_365d,
    days_since_first_order, days_since_last_order,
    favorite_category, favorite_brand,
    -- Engagement
    website_visits_30d, email_opens_30d, support_tickets_90d,
    -- Labels (for training)
    churned_next_30d, ltv_next_365d
FROM customer_360_calculation;

-- Data scientists can directly: df = spark.table('customer_features_obt').toPandas()
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Zero Joins** | Fastest possible query performance |
| **Simple Queries** | Business users can self-serve |
| **Columnar Optimized** | Parquet/Delta compression works great |
| **Predictable** | No join fanouts or explosions |
| **BI Friendly** | Drag-and-drop in Tableau/Power BI |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Stale Data** | Must fully rebuild when dimensions change |
| **Schema Changes** | Adding columns requires full rebuild |
| **Storage** | Redundant data (mitigated by compression) |
| **Granularity Lock** | Fixed to one grain, can't drill to different levels |
| **Update Latency** | SCD changes require full OBT refresh |

---

**When to Use OBT:**
- High-performance dashboards with known query patterns
- ML feature stores (one row per entity)
- Self-service BI for non-technical users
- Reporting layers where latency matters

**When NOT to Use OBT:**
- Rapidly changing dimension attributes
- Need for multiple granularities
- Ad-hoc analysis requiring flexibility
- Real-time data requirements

**Interview Tip:** "OBT is the ultimate denormalization - zero joins, maximum speed. I use it for specific high-performance dashboards and ML feature stores. But I always maintain the source star schema in Silver for flexibility. OBT is a Gold layer optimization, not a replacement for dimensional modeling."

[↑ Back to Topics](#design-tradeoffs)

---

## Tier 2: Data Models

### Logical vs Physical Data Models

**Definition:** Logical models define WHAT data to store (business perspective), while Physical models define HOW to store it (technical implementation).

---

**Comparison:**

| Aspect | Logical Model | Physical Model |
|--------|---------------|----------------|
| **Audience** | Business analysts, architects | DBAs, data engineers |
| **Data Types** | Generic (Text, Number, Date) | Specific (VARCHAR(255), BIGINT, TIMESTAMP) |
| **Keys** | Business keys (customer_id) | Surrogate keys, foreign keys |
| **Relationships** | Cardinality (1:M, M:N) | JOIN conditions, constraints |
| **Indexes** | Not specified | Defined for performance |
| **Partitioning** | Not specified | Partition keys, bucketing |
| **Platform** | Database-agnostic | Platform-specific (Snowflake, Delta Lake) |
| **Normalization** | Conceptual (3NF goal) | May denormalize for performance |

---

**Example 1: Customer Orders**

**Logical Model:**
```
CUSTOMER                           ORDER
────────────                           ────────────
customer_id (PK)                   order_id (PK)
name                               customer_id (FK)
email                              order_date
segment                            total_amount

Relationship: Customer 1:M Order (one customer, many orders)
```

**Physical Model (PostgreSQL):**
```sql
CREATE TABLE dim_customer (
    customer_key    BIGSERIAL PRIMARY KEY,
    customer_id     VARCHAR(50) NOT NULL UNIQUE,
    name            VARCHAR(255) NOT NULL,
    email           VARCHAR(320),
    segment         VARCHAR(20) CHECK (segment IN ('Bronze','Silver','Gold','Platinum')),
    created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
CREATE INDEX idx_customer_segment ON dim_customer(segment);

CREATE TABLE fact_orders (
    order_key       BIGSERIAL PRIMARY KEY,
    order_id        VARCHAR(50) NOT NULL UNIQUE,
    customer_key    BIGINT REFERENCES dim_customer(customer_key),
    order_date      DATE NOT NULL,
    total_amount    DECIMAL(12,2) NOT NULL,
    created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP
) PARTITION BY RANGE (order_date);
```

**Physical Model (Delta Lake/Databricks):**
```sql
CREATE TABLE dim_customer (
    customer_key    BIGINT GENERATED ALWAYS AS IDENTITY,
    customer_id     STRING NOT NULL,
    name            STRING NOT NULL,
    email           STRING,
    segment         STRING
)
USING DELTA
TBLPROPERTIES ('delta.autoOptimize.optimizeWrite' = 'true');

CREATE TABLE fact_orders (
    order_key       BIGINT GENERATED ALWAYS AS IDENTITY,
    order_id        STRING NOT NULL,
    customer_key    BIGINT,
    order_date      DATE NOT NULL,
    total_amount    DECIMAL(12,2) NOT NULL
)
USING DELTA
PARTITIONED BY (order_date)
CLUSTER BY (customer_key);  -- Liquid Clustering in Databricks
```

---

**Example 2: Design Process Flow**
```
Step 1: Logical Model (Business Requirements)
────────────────────────────────────────
"We need to track customers, their orders, and products"
  ↓
Entities: Customer, Order, Product, Order_Line
Relationships: Customer 1:M Order, Order 1:M Order_Line, Product 1:M Order_Line

Step 2: Physical Model (Technical Implementation)
────────────────────────────────────────
- dim_customer: Surrogate key, indexes on segment/region
- fact_orders: Partitioned by date, clustered by customer
- fact_order_lines: Partitioned by date, grain is order+product
- Columnar format: Delta Lake
- Retention: 7 years hot, archive to cold storage
```

---

**✅ Pros of Separating Logical/Physical:**
| Benefit | Description |
|---------|-------------|
| **Communication** | Business understands logical; tech owns physical |
| **Flexibility** | Change platform without changing business model |
| **Documentation** | Logical model is living documentation |
| **Optimization** | Physical can deviate for performance |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Maintenance** | Two models to keep in sync |
| **Drift** | Physical may diverge from logical over time |
| **Overhead** | Extra documentation effort |

**Interview Tip:** "Logical models are for business alignment - they answer 'what data do we need?' Physical models are for engineering - they answer 'how do we store and optimize it?' I always start with logical to ensure business requirements are captured, then translate to physical with platform-specific optimizations."

[↑ Back to Topics](#data-models)

---

### Conceptual Data Models

**Definition:** The highest-level data model showing only entities and their relationships - no attributes, no data types, no technical details. A business-readable map of "what things exist and how they connect."

---

**Three-Tier Model Hierarchy:**
```
                    CONCEPTUAL
                    \u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500
          Entities + Relationships only
          Audience: Executives, Business
                        \u2502
                        \u25bc
                      LOGICAL
                    \u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500
          + Attributes, keys, cardinality
          Audience: Analysts, Architects
                        \u2502
                        \u25bc
                     PHYSICAL
                    \u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500
          + Data types, indexes, partitions
          Audience: DBAs, Engineers
```

---

**Example 1: E-commerce Conceptual Model**
```
\u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510     places     \u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510    contains    \u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510
\u2502  CUSTOMER \u2502\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u25ba\u2502   ORDER   \u2502\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u25ba\u2502  PRODUCT  \u2502
\u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518             \u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518               \u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518
      \u2502                        \u2502                           \u2502
      \u2502 writes                 \u2502 ships to                  \u2502 belongs to
      \u25bc                        \u25bc                           \u25bc
\u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510            \u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510              \u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510
\u2502  REVIEW   \u2502            \u2502  ADDRESS  \u2502              \u2502 CATEGORY  \u2502
\u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518            \u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518              \u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518

No columns, no data types - just "what exists" and "how connected"
```

**Example 2: Healthcare Conceptual Model**
```
\u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510     treats     \u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510     has      \u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510
\u2502 DOCTOR  \u2502\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u25ba\u2502 PATIENT \u2502\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u25ba\u2502  DIAGNOSIS  \u2502
\u2514\u2500\u2500\u2500\u2500\u252c\u2500\u2500\u2500\u2500\u2518             \u2514\u2500\u2500\u2500\u2500\u252c\u2500\u2500\u2500\u2500\u2518             \u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518
     \u2502                     \u2502
     \u2502 works at            \u2502 takes
     \u25bc                     \u25bc
\u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510           \u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510
\u2502HOSPITAL \u2502           \u2502 MEDICATION  \u2502
\u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518           \u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518
```

**Example 3: Financial Services Conceptual Model**
```
\u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510     owns      \u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510    records    \u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510
\u2502 CUSTOMER \u2502\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u25ba\u2502 ACCOUNT \u2502\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u25ba\u2502 TRANSACTION \u2502
\u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518            \u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518              \u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518
                            \u2502
                            \u2502 applies for
                            \u25bc
                      \u250c\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2510
                      \u2502  LOAN  \u2502
                      \u2514\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2500\u2518
```

---

**Purpose:**

| Use Case | Description |
|----------|-------------|
| **Stakeholder Alignment** | Validate understanding with business before technical work |
| **Scope Definition** | Define what's in/out of the data domain |
| **Communication Tool** | Non-technical presentation for executives |
| **Project Kickoff** | Starting point for data initiatives |
| **Documentation** | Living reference for domain understanding |

---

**\u2705 Pros:**
| Benefit | Description |
|---------|-------------|
| **Simple** | Anyone can understand it |
| **Fast** | Create in hours, not days |
| **Business Aligned** | Uses business language |
| **Technology Agnostic** | No platform decisions needed |

**\u274c Cons:**
| Drawback | Description |
|----------|-------------|
| **Incomplete** | Not enough detail for implementation |
| **Ambiguous** | Relationships may be interpreted differently |
| **Stale** | Often not maintained after project starts |

---

**Best Practice: Create All Three Levels**
```
1. Conceptual: Align with business (boxes and lines)
2. Logical: Define attributes, keys, cardinality
3. Physical: Implement with platform-specific DDL
```

**Interview Tip:** "Conceptual models are my starting point for any new data initiative. They're the handshake between business and technical teams - simple enough for executives to review, precise enough to prevent misunderstandings. I typically whiteboard a conceptual model in the first stakeholder meeting, then evolve it to logical and physical as we progress."

[\u2191 Back to Topics](#data-models)

---

# Tier 3: Advanced/Specialized Detailed Explanations

## Specialized Modeling Patterns

### Graph Data Modeling

**Definition:** A data modeling approach where entities are represented as nodes and relationships as edges, optimized for traversing connections rather than aggregating rows.

---

**When to Use Graph vs Relational:**

| Use Case | Graph | Relational |
|----------|-------|------------|
| Social networks (friends of friends) | ✅ Optimal | ❌ Recursive CTEs slow |
| Fraud detection (connection patterns) | ✅ Optimal | ❌ Multiple self-joins |
| Recommendation engines | ✅ Fast traversal | ❌ Complex joins |
| Aggregations (SUM, COUNT, AVG) | ❌ Not designed for | ✅ Optimal |
| OLAP analytics | ❌ Not designed for | ✅ Use star schema |

---

**Example 1: Social Network**
```
Graph Structure:
    (Alice)──follows──►(Bob)──follows──►(Carol)
       │                  │
       └──follows──►(David)──works_at──►(TechCorp)
```

```sql
-- Relational approach (complex, slow):
WITH RECURSIVE friends AS (
    SELECT friend_id FROM follows WHERE user_id = 'Alice'
    UNION ALL
    SELECT f.friend_id FROM follows f
    JOIN friends fr ON f.user_id = fr.friend_id
    WHERE depth < 3
)
SELECT DISTINCT company FROM employees 
WHERE user_id IN (SELECT friend_id FROM friends);

-- Graph approach (Cypher - Neo4j):
MATCH (alice:User {name: 'Alice'})-[:FOLLOWS*1..3]->(friend)-[:WORKS_AT]->(company)
RETURN DISTINCT company.name
```

**Example 2: Fraud Detection Ring**
```cypher
-- Find circular money flows within 5 hops
MATCH path = (a:Account)-[:TRANSFER*2..5]->(a)
WHERE ALL(t IN relationships(path) WHERE t.amount > 10000)
RETURN path
```

**Example 3: Product Recommendations**
```cypher
-- "Customers who bought X also bought Y"
MATCH (c:Customer)-[:PURCHASED]->(p1:Product {name: 'Laptop'})
MATCH (c)-[:PURCHASED]->(p2:Product)
WHERE p2.name <> 'Laptop'
RETURN p2.name, COUNT(*) as co_purchases
ORDER BY co_purchases DESC LIMIT 10
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Relationship Traversal** | O(1) hop vs O(n) join |
| **Pattern Matching** | Find complex connection patterns |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Aggregations** | Not optimized for SUM/AVG/COUNT |
| **Batch Analytics** | Poor for OLAP workloads |

**Interview Tip:** "I use graph databases when the query is about relationships - 'friends of friends', 'shortest path', 'connected components'. For analytics and aggregations, I stick with relational/dimensional models."

[↑ Back to Topics](#specialized-modeling-patterns)

---

### Hierarchical Data Modeling

**Definition:** Techniques for modeling tree structures (org charts, product categories, bill of materials) where entities have parent-child relationships.

---

**Four Main Approaches:**

| Method | Structure | Query Speed | Update Speed | Best For |
|--------|-----------|-------------|--------------|----------|
| **Adjacency List** | parent_id FK | Slow (recursive) | Fast | Shallow, changing trees |
| **Path Enumeration** | path string | Fast reads | Slow updates | Read-heavy, stable trees |
| **Nested Sets** | left/right bounds | Fast subtree | Very slow updates | Static hierarchies |
| **Closure Table** | ancestor/descendant pairs | Fast all queries | Moderate updates | Balanced read/write |

---

**Example 1: Adjacency List**
```sql
CREATE TABLE categories (
    category_id     INT PRIMARY KEY,
    category_name   VARCHAR(100),
    parent_id       INT REFERENCES categories(category_id)
);

-- Query subtree (requires recursive CTE):
WITH RECURSIVE tree AS (
    SELECT category_id, category_name, 0 as depth
    FROM categories WHERE category_id = 1
    UNION ALL
    SELECT c.category_id, c.category_name, t.depth + 1
    FROM categories c
    JOIN tree t ON c.parent_id = t.category_id
)
SELECT * FROM tree;
```

**Example 2: Path Enumeration**
```sql
CREATE TABLE categories (
    category_id     INT PRIMARY KEY,
    category_name   VARCHAR(100),
    path            VARCHAR(500)  -- '/1/2/5/' format
);

-- Query all descendants (fast!):
SELECT * FROM categories WHERE path LIKE '/1/%';
```

**Example 3: Closure Table**
```sql
CREATE TABLE category_closure (
    ancestor_id     INT,
    descendant_id   INT,
    depth           INT,
    PRIMARY KEY (ancestor_id, descendant_id)
);

-- Query all descendants:
SELECT c.* FROM categories c
JOIN category_closure cc ON c.category_id = cc.descendant_id
WHERE cc.ancestor_id = 1;
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Natural Fit** | Matches org/category structures |
| **Drill-Down** | Supports expand/collapse in UI |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Complexity** | No single best approach |
| **Performance** | Deep hierarchies expensive |

**Interview Tip:** "For hierarchies, I evaluate read/write ratio first. Adjacency list for changing trees, closure table for balanced workloads, path enumeration when reads dominate."

[↑ Back to Topics](#specialized-modeling-patterns)

---

### Data Modeling for Real-Time/Streaming

**Definition:** Designing data models for continuous event streams with millisecond latency requirements.

---

**Key Concepts:**

| Concept | Description |
|---------|-------------|
| **Event Time** | When event occurred (in source) |
| **Processing Time** | When event is processed |
| **Watermark** | How late events are tolerated |
| **Window** | Time bucket (tumbling, sliding, session) |

---

**Example 1: Event Schema**
```sql
CREATE TABLE events (
    event_id        STRING,
    event_type      STRING,
    event_time      TIMESTAMP,
    processing_time TIMESTAMP,
    user_id         STRING,
    properties      MAP<STRING, STRING>
)
USING DELTA
PARTITIONED BY (DATE(event_time));
```

**Example 2: Windowed Aggregations**
```sql
-- Tumbling window: Count purchases per minute
SELECT 
    window(event_time, '1 minute') as time_window,
    COUNT(*) as purchase_count
FROM events WHERE event_type = 'purchase'
GROUP BY window(event_time, '1 minute');

-- Session window: Events per session (30-min gap)
SELECT 
    user_id,
    session_window(event_time, '30 minutes') as session,
    COUNT(*) as events_in_session
FROM events
GROUP BY user_id, session_window(event_time, '30 minutes');
```

**Example 3: Lambda vs Kappa**
```
Lambda: Batch + Speed layers → Merged view (two codebases)
Kappa:  Stream-only → Materialized views (one codebase, replay)
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Low Latency** | Sub-second insights |
| **Event Sourcing** | Full audit trail |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Complexity** | Late arrivals, out-of-order events |
| **Cost** | Always-on infrastructure |

**Interview Tip:** "For streaming, I separate event_time from processing_time, use watermarks for late data, and choose Kappa when I can replay from Kafka."

[↑ Back to Topics](#specialized-modeling-patterns)

---

### Modeling for ML Feature Stores

**Definition:** Data models optimized for ML, providing consistent features for training (batch) and inference (real-time).

---

**Example 1: Feature Table**
```sql
CREATE TABLE customer_features (
    customer_id         STRING NOT NULL,
    feature_timestamp   TIMESTAMP NOT NULL,
    lifetime_orders     INT,
    days_since_last_order INT,
    orders_last_30d     INT,
    is_active           BOOLEAN
)
USING DELTA
PARTITIONED BY (DATE(feature_timestamp));
```

**Example 2: Point-in-Time Join (Avoid Leakage)**
```sql
SELECT l.customer_id, l.churned_next_30d, f.orders_last_30d
FROM labels l
ASOF JOIN customer_features f
    ON l.customer_id = f.customer_id
    AND f.feature_timestamp <= l.label_date;
```

---

**✅ Pros:** Consistency, reusability, point-in-time correctness
**❌ Cons:** Dual stores, freshness trade-offs, complexity

**Interview Tip:** "Feature stores solve training/serving skew and prevent data leakage via ASOF joins."

[↑ Back to Topics](#specialized-modeling-patterns)

---

### Activity Schema / Event Modeling

**Definition:** Capturing user actions as immutable events for flexible analytics.

---

**Example 1: Activity Stream**
```sql
CREATE TABLE activity_stream (
    activity_id     STRING PRIMARY KEY,
    user_id         STRING,
    activity_type   STRING,
    activity_time   TIMESTAMP,
    properties      MAP<STRING, STRING>
);
```

**Example 2: Funnel Analysis**
```sql
WITH funnel AS (
    SELECT user_id,
        MAX(CASE WHEN activity_type = 'viewed_product' THEN 1 ELSE 0 END) as viewed,
        MAX(CASE WHEN activity_type = 'purchased' THEN 1 ELSE 0 END) as purchased
    FROM activity_stream
    WHERE activity_time >= CURRENT_DATE - 7
    GROUP BY user_id
)
SELECT SUM(viewed) as viewers, SUM(purchased) as buyers,
       ROUND(100.0 * SUM(purchased) / SUM(viewed), 1) as conversion_pct
FROM funnel;
```

---

**✅ Pros:** Flexibility, complete history, sequence analysis
**❌ Cons:** Query complexity, storage growth

**Interview Tip:** "Activity schemas capture raw user journeys. I materialize aggregates for reporting."

[↑ Back to Topics](#specialized-modeling-patterns)

---

## Technical Optimizations

### Run Length Encoding (RLE) Compression

**Definition:** A compression technique that stores consecutive identical values as a single value plus count. Highly effective for sorted, low-cardinality columns in columnar storage.

---

**How It Works:**
```
Raw Data:     [A, A, A, A, B, B, B, C, C, C, C, C, C]
RLE Encoded:  [(A, 4), (B, 3), (C, 6)]

Storage: 13 values → 6 values (54% reduction)
```

---

**Example 1: Status Column Optimization**
```sql
-- Maximize RLE by sorting before write:
INSERT INTO optimized_table
SELECT * FROM source_table
ORDER BY status, region, date;  -- Low-cardinality columns first

-- Result: 'Active' repeated 800K times → stored as ('Active', 800000)
```

**Example 2: Delta Lake Z-ORDER**
```sql
-- Z-ORDER clusters data for RLE benefits
OPTIMIZE my_table ZORDER BY (region, status);

-- Queries now scan fewer files
SELECT * FROM my_table WHERE region = 'US' AND status = 'Active';
```

**Example 3: Parquet Column Ordering**
```python
# Sort before writing to enable RLE
df.sort_values(['region', 'product_type']).to_parquet(
    'output.parquet',
    compression='snappy'
)
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Compression** | 10-100x for sorted low-cardinality |
| **Scan Speed** | Read one value, apply to N rows |
| **Automatic** | Parquet/ORC apply it automatically |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Sort Required** | Must cluster data for best results |
| **High Cardinality** | Ineffective for unique values |

**Interview Tip:** "RLE is why column order in CLUSTER BY matters - low cardinality columns first maximize compression. I always sort data before writing to Parquet."

[↑ Back to Topics](#technical-optimizations)

---

### Datelist Data Structure

**Definition:** A compact representation of date ranges or activity patterns as an array or bitmap, reducing storage for time-series presence data.

---

**Example 1: Active Days as Array**
```sql
-- Instead of one row per active day (365 rows/user/year):
-- Use array: one row with list of active days

CREATE TABLE user_activity_monthly (
    user_id         STRING,
    year_month      STRING,  -- '2025-01'
    active_days     ARRAY<INT>,  -- [1, 2, 3, 5, 8, 13]
    active_count    INT
);

-- Query: Users active on Jan 3rd
SELECT user_id FROM user_activity_monthly
WHERE year_month = '2025-01' AND ARRAY_CONTAINS(active_days, 3);
```

**Example 2: Date Range Storage**
```sql
-- Single row vs 90 rows
CREATE TABLE subscription_periods (
    user_id     STRING,
    start_date  DATE,
    end_date    DATE,
    days_active INT GENERATED ALWAYS AS (DATEDIFF(end_date, start_date) + 1)
);
```

**Example 3: Weekly Pattern Bitmap**
```sql
-- 7-bit bitmap for weekly pattern
-- User active Mon-Fri = 0111110 binary = 62 decimal
CREATE TABLE user_weekly_pattern (
    user_id         STRING,
    activity_bitmap INT  -- 0-127
);

-- Check if user is active on Monday (bit 1):
SELECT user_id FROM user_weekly_pattern WHERE activity_bitmap & 2 > 0;
```

---

**✅ Pros:** Compact storage, fast aggregation (array length = count)
**❌ Cons:** Query complexity, harder to join on individual dates

**Interview Tip:** "Datelists trade query simplicity for storage. For user activity, instead of 365 rows per user, I store one row with an array. Great for DAU/MAU."

[↑ Back to Topics](#technical-optimizations)

---

### Power of Enums / Little Book of Enums

**Definition:** Using enumerated types or integer codes instead of strings for categorical data, providing type safety, storage efficiency, and performance.

---

**Example 1: Native Enum (PostgreSQL)**
```sql
CREATE TYPE order_status AS ENUM ('pending', 'shipped', 'delivered', 'cancelled');

CREATE TABLE orders (
    order_id BIGINT,
    status order_status  -- Stored as integer internally
);

-- Benefits: Type safety (can't insert 'pendng'), 1-4 bytes vs 10+ bytes
```

**Example 2: Enum Pattern in Data Warehouses**
```sql
-- Delta/Spark (no native enum) - use TINYINT
CREATE TABLE orders (
    order_id BIGINT,
    status_code TINYINT  -- 1=pending, 2=shipped, 3=delivered, 4=cancelled
);

-- Lookup for reporting:
CREATE VIEW orders_readable AS
SELECT order_id,
    CASE status_code
        WHEN 1 THEN 'Pending'
        WHEN 2 THEN 'Shipped'
        WHEN 3 THEN 'Delivered'
        WHEN 4 THEN 'Cancelled'
    END as status
FROM orders;
```

**Example 3: Bitmap Flags**
```sql
-- Multiple permissions in single INT
-- 1=read, 2=write, 4=delete, 8=admin
CREATE TABLE user_permissions (
    user_id BIGINT,
    permissions INT  -- read+write = 3, all = 15
);

-- Check write permission:
SELECT user_id FROM user_permissions WHERE permissions & 2 > 0;
```

---

**✅ Pros:** Type safety, 10x storage reduction, faster comparisons
**❌ Cons:** Schema changes complex, less readable in raw data

**Interview Tip:** "I use enums or TINYINT+lookup for any categorical column with <50 values. It's a 10x storage reduction and enables efficient bitmap indexes."

[↑ Back to Topics](#technical-optimizations)

---

## Fact Table Concepts (Advanced)

### Reduced Facts

**Definition:** Pre-aggregated fact tables at a coarser grain to improve query performance for common aggregate queries.

---

**Example 1: Daily to Monthly Reduction**
```sql
-- Source: fact_daily_sales (3.65M rows/year for 10K products)
-- Reduced: fact_monthly_sales (120K rows/year - 97% reduction)

CREATE TABLE fact_monthly_sales AS
SELECT 
    product_key,
    DATE_TRUNC('month', sale_date) as sale_month,
    SUM(quantity) as total_quantity,
    SUM(revenue) as total_revenue,
    COUNT(DISTINCT customer_key) as unique_customers
FROM fact_daily_sales
GROUP BY product_key, DATE_TRUNC('month', sale_date);
```

**Example 2: Multi-Level Rollups**
```sql
CREATE TABLE fact_sales_reduced AS
SELECT 
    COALESCE(product_category, 'ALL') as category,
    COALESCE(region, 'ALL') as region,
    sale_month,
    SUM(revenue) as revenue
FROM fact_sales
GROUP BY GROUPING SETS (
    (product_category, region, sale_month),
    (product_category, sale_month),
    (region, sale_month),
    (sale_month)
);
```

---

**✅ Pros:** 10-100x faster queries, reduced compute costs
**❌ Cons:** Data latency, storage overhead, sync complexity

**Interview Tip:** "I create reduced facts for dashboard queries that always aggregate to week/month. The base fact stays for drill-through; reduced facts serve 90% of queries."

[↑ Back to Topics](#fact-table-concepts-advanced)

---

## Cloud-Native Patterns

### Data Modeling for Cloud Data Warehouses

**Definition:** Adapting data modeling practices for cloud-native platforms (Snowflake, BigQuery, Redshift, Databricks) that have different performance characteristics than traditional RDBMS.

---

**Platform Comparison:**

| Aspect | Snowflake | BigQuery | Redshift | Databricks |
|--------|-----------|----------|----------|------------|
| **Storage** | Columnar, auto-compressed | Columnar, Capacitor | Columnar | Delta Lake (Parquet) |
| **Partitioning** | Micro-partitions (auto) | Date partitioning | Distribution keys | Partition columns |
| **Clustering** | Cluster keys | Clustering | Sort keys | Z-ORDER, Liquid Clustering |
| **Joins** | Optimized | Broadcast/Shuffle | Collocated | Broadcast/Sort-Merge |

---

**Example 1: Snowflake Optimization**
```sql
-- Cluster keys for common filters
CREATE TABLE fact_orders (
    order_id BIGINT,
    order_date DATE,
    customer_id STRING,
    amount DECIMAL(12,2)
)
CLUSTER BY (order_date, customer_id);

-- Search optimization for point lookups
ALTER TABLE dim_customer ADD SEARCH OPTIMIZATION ON EQUALITY(customer_id);
```

**Example 2: BigQuery Partitioning + Clustering**
```sql
CREATE TABLE fact_orders
PARTITION BY DATE(order_date)
CLUSTER BY customer_id, product_id
AS SELECT * FROM staging_orders;

-- Queries filter on partition first, then cluster columns
SELECT * FROM fact_orders 
WHERE order_date BETWEEN '2025-01-01' AND '2025-01-31'
  AND customer_id = 'C001';
```

**Example 3: Databricks Liquid Clustering**
```sql
-- Modern alternative to partitioning + Z-ORDER
CREATE TABLE fact_orders (
    order_id BIGINT,
    order_date DATE,
    customer_id STRING,
    region STRING
)
USING DELTA
CLUSTER BY (order_date, customer_id, region);

-- Auto-optimizes on write, no manual OPTIMIZE needed
```

---

**Cloud-Native Best Practices:**

| Practice | Why |
|----------|-----|
| Avoid small files | Merge on write, use OPTIMIZE |
| Denormalize more | Joins are expensive at scale |
| Use native types | VARIANT/JSON for semi-structured |
| Partition by date | Most queries filter by time |
| Monitor costs | Pay per query/storage |

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Auto-scaling** | Compute scales with workload |
| **Separation** | Storage and compute independent |
| **Managed** | No infrastructure maintenance |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Cost** | Can be expensive at scale |
| **Vendor Lock-in** | Platform-specific features |
| **Learning Curve** | Different optimization patterns |

**Interview Tip:** "Cloud warehouses favor wide, denormalized tables because storage is cheap and joins are expensive. I partition by date, cluster by common filter columns, and use platform-specific features like Snowflake's search optimization or Databricks' Liquid Clustering."

[↑ Back to Topics](#cloud-native-patterns)

---

## Business Analytics Modeling

### Growth Accounting

**Definition:** A framework for decomposing user/revenue growth into constituent parts: new, retained, resurrected, and churned users, providing actionable growth insights.

---

**Growth Accounting Formula:**
```
End Period Users = Start Users + New + Resurrected - Churned

Net Growth = New + Resurrected - Churned
```

**Example 1: User Growth Accounting**
```sql
WITH user_states AS (
    SELECT 
        user_id,
        month,
        CASE 
            WHEN LAG(month) OVER (PARTITION BY user_id ORDER BY month) IS NULL THEN 'new'
            WHEN LAG(month) OVER (PARTITION BY user_id ORDER BY month) = ADD_MONTHS(month, -1) THEN 'retained'
            WHEN LAG(month) OVER (PARTITION BY user_id ORDER BY month) < ADD_MONTHS(month, -1) THEN 'resurrected'
        END as user_state
    FROM active_users_monthly
)
SELECT 
    month,
    COUNT(CASE WHEN user_state = 'new' THEN 1 END) as new_users,
    COUNT(CASE WHEN user_state = 'retained' THEN 1 END) as retained_users,
    COUNT(CASE WHEN user_state = 'resurrected' THEN 1 END) as resurrected_users
FROM user_states
GROUP BY month;
```

**Example 2: MRR Growth Accounting**
```sql
SELECT 
    month,
    SUM(CASE WHEN prev_mrr IS NULL AND curr_mrr > 0 THEN curr_mrr END) as new_mrr,
    SUM(CASE WHEN prev_mrr > 0 AND curr_mrr > prev_mrr THEN curr_mrr - prev_mrr END) as expansion_mrr,
    SUM(CASE WHEN prev_mrr > 0 AND curr_mrr < prev_mrr AND curr_mrr > 0 THEN prev_mrr - curr_mrr END) as contraction_mrr,
    SUM(CASE WHEN prev_mrr > 0 AND curr_mrr = 0 THEN prev_mrr END) as churned_mrr
FROM (
    SELECT 
        user_id, 
        month, 
        mrr as curr_mrr,
        LAG(mrr) OVER (PARTITION BY user_id ORDER BY month) as prev_mrr
    FROM subscription_mrr
);
```

---

**✅ Pros:** Actionable insights, identifies growth levers, executive-friendly
**❌ Cons:** Requires clean cohort tracking, definition consistency

**Interview Tip:** "Growth accounting tells you WHERE growth comes from. If net growth is flat but new users are high, you have a retention problem. I build dashboards that show new, retained, resurrected, and churned separately."

[↑ Back to Topics](#business-analytics-modeling)

---

### Customer Lifetime Value (CLV)

**Definition:** The predicted total revenue a customer will generate over their entire relationship, used for acquisition investment decisions and customer segmentation.

---

**CLV Formulas:**
```
Simple CLV = Average Revenue per Customer × Average Lifespan

Contractual CLV = (ARPU × Gross Margin) / Churn Rate

Probabilistic CLV = Σ (Margin × P(Active at time t) × Discount Factor)
```

**Example 1: Historical CLV**
```sql
SELECT 
    customer_id,
    first_purchase_date,
    COUNT(DISTINCT order_id) as total_orders,
    SUM(revenue) as lifetime_revenue,
    SUM(revenue - cost) as lifetime_profit,
    DATEDIFF(MAX(order_date), first_purchase_date) as customer_tenure_days
FROM orders o
JOIN customers c USING (customer_id)
GROUP BY customer_id, first_purchase_date;
```

**Example 2: Predictive CLV (BG/NBD Model Inputs)**
```sql
-- RFM metrics for probabilistic models
SELECT 
    customer_id,
    DATEDIFF(CURRENT_DATE, MAX(order_date)) as recency_days,
    COUNT(*) as frequency,
    DATEDIFF(MAX(order_date), MIN(order_date)) as T,  -- customer age
    AVG(order_value) as monetary_value
FROM orders
WHERE order_date >= '2020-01-01'
GROUP BY customer_id
HAVING COUNT(*) >= 2;  -- Need 2+ purchases for model
```

---

**✅ Pros:** Informs CAC limits, segments high-value customers
**❌ Cons:** Prediction uncertainty, assumes stable behavior

**Interview Tip:** "I calculate historical CLV first, then build predictive models. Historical shows what customers have spent; predictive estimates future value for acquisition decisions."

[↑ Back to Topics](#business-analytics-modeling)

---

### Cohort Analysis

**Definition:** Grouping users by shared characteristics (acquisition date, first action) and tracking their behavior over time to identify trends and compare performance.

---

**Example 1: Retention Cohort**
```sql
WITH cohorts AS (
    SELECT 
        user_id,
        DATE_TRUNC('month', first_activity_date) as cohort_month
    FROM users
),
activity AS (
    SELECT 
        user_id,
        DATE_TRUNC('month', activity_date) as activity_month
    FROM user_activity
)
SELECT 
    cohort_month,
    DATEDIFF(month, cohort_month, activity_month) as months_since_signup,
    COUNT(DISTINCT a.user_id) as active_users,
    COUNT(DISTINCT a.user_id) * 100.0 / 
        FIRST_VALUE(COUNT(DISTINCT a.user_id)) OVER (PARTITION BY cohort_month ORDER BY activity_month) as retention_pct
FROM cohorts c
JOIN activity a USING (user_id)
GROUP BY cohort_month, activity_month;
```

**Example 2: Revenue Cohort**
```sql
SELECT 
    cohort_month,
    months_since_signup,
    SUM(revenue) as cohort_revenue,
    SUM(revenue) / COUNT(DISTINCT user_id) as revenue_per_user,
    SUM(SUM(revenue)) OVER (PARTITION BY cohort_month ORDER BY months_since_signup) as cumulative_revenue
FROM (
    SELECT 
        u.user_id,
        DATE_TRUNC('month', u.signup_date) as cohort_month,
        DATEDIFF(month, u.signup_date, o.order_date) as months_since_signup,
        o.revenue
    FROM users u
    JOIN orders o USING (user_id)
)
GROUP BY cohort_month, months_since_signup;
```

---

**Cohort Matrix Visualization:**
```
                 Month 0   Month 1   Month 2   Month 3
Jan 2025 Cohort   100%      45%       30%       25%
Feb 2025 Cohort   100%      50%       35%       28%
Mar 2025 Cohort   100%      55%       40%       --
Apr 2025 Cohort   100%      52%       --        --
```

---

**✅ Pros:** Isolates time effects, compares cohort quality, tracks improvement
**❌ Cons:** Small cohorts = noisy data, requires consistent definitions

**Interview Tip:** "Cohort analysis isolates the effect of when users joined. If the Feb cohort retains better than Jan, something improved. I build both retention and revenue cohort views."

[↑ Back to Topics](#business-analytics-modeling)

---

### Churn Analysis

**Definition:** Analyzing customer attrition patterns to predict and prevent future churn through early warning indicators and intervention strategies.

---

**Example 1: Churn Rate Calculation**
```sql
-- Monthly churn rate
SELECT 
    month,
    COUNT(DISTINCT CASE WHEN churned = 1 THEN customer_id END) as churned_customers,
    COUNT(DISTINCT customer_id) as total_customers,
    ROUND(100.0 * COUNT(DISTINCT CASE WHEN churned = 1 THEN customer_id END) / 
          COUNT(DISTINCT customer_id), 2) as churn_rate_pct
FROM (
    SELECT 
        customer_id,
        month,
        CASE WHEN LEAD(month) OVER (PARTITION BY customer_id ORDER BY month) IS NULL 
                  AND month < CURRENT_DATE - INTERVAL 30 DAY THEN 1 ELSE 0 END as churned
    FROM active_customers_monthly
);
```

**Example 2: Churn Predictors**
```sql
-- Features for churn prediction
SELECT 
    customer_id,
    -- Usage decline
    (usage_30d - usage_60_to_90d) / NULLIF(usage_60_to_90d, 0) as usage_trend,
    -- Engagement drop
    days_since_last_login,
    -- Support tickets (friction indicator)
    support_tickets_30d,
    -- Feature adoption
    features_used_pct,
    -- Actual churn (label)
    churned_next_month
FROM customer_health_scores;
```

---

**✅ Pros:** Enables proactive retention, identifies at-risk customers
**❌ Cons:** Requires accurate churn definition, behavior changes over time

**Interview Tip:** "I define churn based on business context - contractual (cancellation) or behavioral (90 days inactive). Then I track leading indicators like usage decline and support tickets."

[↑ Back to Topics](#business-analytics-modeling)

---

## Analytics Types

### Descriptive Analytics

**Definition:** Analytics that describe what happened in the past through summarization, aggregation, and visualization of historical data.

---

**Example 1: Sales Summary**
```sql
SELECT 
    region,
    product_category,
    SUM(revenue) as total_revenue,
    COUNT(DISTINCT order_id) as order_count,
    AVG(order_value) as avg_order_value,
    SUM(revenue) / SUM(SUM(revenue)) OVER () * 100 as revenue_pct
FROM sales
WHERE order_date BETWEEN '2025-01-01' AND '2025-03-31'
GROUP BY region, product_category;
```

**Use Cases:** Dashboards, KPI reporting, trend visualization

**Interview Tip:** "Descriptive analytics is the foundation - you must understand what happened before predicting what will happen."

[↑ Back to Topics](#analytics-types)

---

### Diagnostic Analytics

**Definition:** Analytics that explain why something happened by identifying patterns, correlations, and root causes in historical data.

---

**Example 1: Revenue Drop Investigation**
```sql
-- Drill down: Which dimension explains the drop?
SELECT 
    'By Region' as dimension,
    region as value,
    SUM(CASE WHEN period = 'current' THEN revenue END) as current_revenue,
    SUM(CASE WHEN period = 'prior' THEN revenue END) as prior_revenue,
    SUM(CASE WHEN period = 'current' THEN revenue END) - 
    SUM(CASE WHEN period = 'prior' THEN revenue END) as delta
FROM sales_with_period
GROUP BY region
ORDER BY delta
LIMIT 5;  -- Top 5 declining regions
```

**Use Cases:** Root cause analysis, anomaly investigation, A/B test analysis

**Interview Tip:** "Diagnostic analytics answers 'why'. When a metric drops, I segment by dimensions to isolate the root cause."

[↑ Back to Topics](#analytics-types)

---

### Predictive Analytics

**Definition:** Analytics that forecast what is likely to happen using statistical models, machine learning, and historical patterns.

---

**Example 1: Churn Probability**
```sql
-- Score customers with pre-trained model
SELECT 
    customer_id,
    model_predict(
        'churn_model',
        days_since_last_login,
        usage_30d,
        support_tickets_30d,
        months_as_customer
    ) as churn_probability
FROM customer_features
WHERE churn_probability > 0.7;  -- High risk
```

**Example 2: Demand Forecasting**
```sql
-- Time series features for forecasting
SELECT 
    product_id,
    sale_date,
    daily_sales,
    AVG(daily_sales) OVER (PARTITION BY product_id ORDER BY sale_date ROWS 7 PRECEDING) as ma_7d,
    AVG(daily_sales) OVER (PARTITION BY product_id ORDER BY sale_date ROWS 28 PRECEDING) as ma_28d,
    LAG(daily_sales, 7) OVER (PARTITION BY product_id ORDER BY sale_date) as sales_last_week,
    LAG(daily_sales, 365) OVER (PARTITION BY product_id ORDER BY sale_date) as sales_last_year
FROM daily_product_sales;
```

**Use Cases:** Churn prediction, demand forecasting, lead scoring

**Interview Tip:** "Predictive analytics requires clean feature engineering. I build feature tables with historical aggregates, then train models on labeled outcomes."

[↑ Back to Topics](#analytics-types)

---

### Prescriptive Analytics

**Definition:** Analytics that recommend actions by optimizing for specific outcomes, combining predictions with business rules and constraints.

---

**Example 1: Dynamic Pricing**
```sql
-- Recommend price based on demand prediction and inventory
SELECT 
    product_id,
    current_price,
    predicted_demand,
    inventory_level,
    CASE 
        WHEN inventory_level < predicted_demand * 0.5 THEN current_price * 1.15  -- Low stock: raise price
        WHEN inventory_level > predicted_demand * 2 THEN current_price * 0.9    -- Excess: discount
        ELSE current_price
    END as recommended_price
FROM demand_predictions
JOIN inventory USING (product_id);
```

**Example 2: Next Best Action**
```sql
SELECT 
    customer_id,
    CASE 
        WHEN churn_probability > 0.8 AND ltv > 1000 THEN 'Personal outreach'
        WHEN churn_probability > 0.8 THEN 'Discount offer'
        WHEN upgrade_probability > 0.6 THEN 'Upsell campaign'
        WHEN engagement_score < 30 THEN 'Re-engagement email'
        ELSE 'No action'
    END as recommended_action
FROM customer_scores;
```

**Use Cases:** Dynamic pricing, inventory optimization, personalized recommendations

**Interview Tip:** "Prescriptive analytics takes predictions and adds business logic to recommend actions. It's the bridge between data science and business operations."

[↑ Back to Topics](#analytics-types)

---

## Alternative Methodologies

### Anchor Modeling

**Definition:** A highly normalized modeling technique where each attribute is stored in a separate table, enabling schema evolution without breaking existing structures.

---

**Core Components:**
- **Anchor:** Entity identifier (like customer_id)
- **Attribute:** Single property in separate table
- **Tie:** Relationship between anchors
- **Knot:** Shared lookup values

**Example: Customer Entity in Anchor Model**
```sql
-- Anchor: Just the ID
CREATE TABLE A_Customer (
    customer_id BIGINT PRIMARY KEY,
    created_at TIMESTAMP
);

-- Attributes: Separate tables with history
CREATE TABLE A_Customer_Name (
    customer_id BIGINT,
    name STRING,
    valid_from TIMESTAMP,
    valid_to TIMESTAMP
);

CREATE TABLE A_Customer_Email (
    customer_id BIGINT,
    email STRING,
    valid_from TIMESTAMP,
    valid_to TIMESTAMP
);

-- Query: Reconstruct customer at point in time
SELECT 
    a.customer_id,
    n.name,
    e.email
FROM A_Customer a
LEFT JOIN A_Customer_Name n ON a.customer_id = n.customer_id
    AND '2025-01-01' BETWEEN n.valid_from AND n.valid_to
LEFT JOIN A_Customer_Email e ON a.customer_id = e.customer_id
    AND '2025-01-01' BETWEEN e.valid_from AND e.valid_to;
```

---

**✅ Pros:**
| Benefit | Description |
|---------|-------------|
| **Additive Schema** | Add attributes without altering existing tables |
| **Full History** | Each attribute tracked independently |
| **No Nulls** | Attributes only exist when populated |

**❌ Cons:**
| Drawback | Description |
|----------|-------------|
| **Query Complexity** | Many joins for simple queries |
| **Performance** | Join overhead at query time |
| **Tooling** | Limited tool support |

**Interview Tip:** "Anchor modeling is overkill for most projects but valuable when schema changes frequently and you need attribute-level history. I've seen it in insurance and healthcare where attributes evolve constantly."

[↑ Back to Topics](#alternative-methodologies)

---

### Data Modeling Best Practices Summary

**Definition:** Consolidated guidelines for effective data modeling across all methodologies, ensuring maintainable, performant, and understandable data structures.

---

**1. Naming Conventions:**
```
Tables: snake_case, singular or plural consistently
  - dim_customer, fact_order, stg_raw_events

Columns: snake_case, descriptive
  - customer_id (not cust_id or id)
  - order_created_at (not created)
  - is_active (boolean prefix)

Keys: table_id pattern
  - customer_id, order_id, product_key
```

**2. Documentation Standards:**
```yaml
# Column-level documentation
columns:
  - name: customer_lifetime_value
    description: Total revenue from customer since first order
    data_type: DECIMAL(12,2)
    business_owner: Finance
    pii: false
    
  - name: email
    description: Primary customer email address
    pii: true
    masked_in: [dev, staging]
```

**3. Testing Checklist:**
```sql
-- Primary key uniqueness
SELECT customer_id, COUNT(*) FROM dim_customer GROUP BY customer_id HAVING COUNT(*) > 1;

-- Referential integrity
SELECT f.* FROM fact_orders f 
LEFT JOIN dim_customer c ON f.customer_id = c.customer_id
WHERE c.customer_id IS NULL;

-- Not null constraints
SELECT * FROM dim_customer WHERE customer_name IS NULL;

-- Accepted values
SELECT DISTINCT status FROM orders WHERE status NOT IN ('pending', 'shipped', 'delivered', 'cancelled');
```

**4. Performance Guidelines:**

| Layer | Partitioning | Clustering | Materialization |
|-------|--------------|------------|-----------------|
| **Staging** | Ingestion date | None | View/Ephemeral |
| **Intermediate** | Business date | Join keys | Table |
| **Marts** | Business date | Filter columns | Table + Aggregate |

**5. Change Management:**
```
- Version control all models
- Use migrations for DDL changes
- Document breaking changes in CHANGELOG
- Deprecate before removing columns (add _deprecated suffix)
- Test in dev before prod deployment
```

---

**Interview Tip:** "Good data modeling is 70% naming and documentation. If consumers can't understand your model without asking you, it's not done. I always add column descriptions and maintain a data dictionary."

[↑ Back to Topics](#alternative-methodologies)

---

## Quick Reference: Decision Guide

| Scenario | Recommended Approach |
|----------|---------------------|
| Fast dashboards for business users | Star Schema / Wide denormalized tables |
| Slowly changing reference data | SCD Type 2 with surrogate keys |
| Real-time event data | Activity Schema + streaming aggregation |
| Frequently changing schema | Data Vault or Anchor Modeling |
| ML feature serving | Feature Store with point-in-time joins |
| Graph relationships (fraud, social) | Graph database or adjacency tables |
| High-cardinality time series | Partitioned by date, clustered by entity |
| Multi-source integration | Data Vault with business keys |

---

## Interview Preparation Tips

1. **Know your trade-offs:** Every design decision has pros and cons
2. **Speak in examples:** "At my last company, we chose X because..."
3. **Ask clarifying questions:** "What's the query pattern? How often does data change?"
4. **Draw diagrams:** Visualize your solution during the interview
5. **Mention tools:** "I'd implement this in dbt with incremental models..."
6. **Discuss evolution:** "We'd start simple and iterate based on usage patterns"

---

[↑ Back to Table of Contents](#table-of-contents)
