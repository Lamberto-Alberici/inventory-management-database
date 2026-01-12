# inventory-management-database
PostgreSQL database system for inventory management with a normalized schema, stored procedures, and custom functions

# Inventory Management Database System

A comprehensive PostgreSQL database designed for retail inventory management operations.

## Overview
Group project (4 members) creating a normalized relational database with advanced features including stored procedures, user-defined functions, views, and transaction management.

**Technologies:** PostgreSQL, SQL, pgAdmin, Database Design

**Course:** Database & File Systems - DePauw University

## Database Architecture

**[View Entity Relationship Diagram (ERD)](./erd.pdf)**

The database consists of 13 interconnected tables designed to support complete inventory management operations:

### Core Tables
- **Customer_Accounts**: User information and account management
- **Customer_Rewards**: Loyalty program with tiered benefits (Bronze/Silver/Gold/Platinum)
- **Products**: Product catalog with pricing and category organization
- **Categories**: Product categorization system
- **Orders**: Order tracking and management
- **Order_Product**: Junction table linking orders to products
- **Customer_Returns**: Return processing and tracking
- **Suppliers** (if applicable): Vendor management

## Key Features

### 1. Custom Functions

**Customer Reward Tier Calculation**
```sql
fn_customer_reward_tier(customer_id) 
-- Returns reward tier based on accumulated points:
-- Bronze: < 150 points
-- Silver: 150-399 points
-- Gold: 400-799 points
-- Platinum: 800+ points
```

**Order Weight Calculator**
```sql
calculate_order_weight_f(order_id)
-- Calculates total weight of all products in an order
-- Returns: DECIMAL representing total weight in standardized units
```

**Product Count by Category**
```sql
count_products_in_category(category_id)
-- Returns count of products in specified category
-- Useful for inventory analysis and category management
```

### 2. Views for Efficient Querying

**Customer Rewards Status**
```sql
fn_customer_rewards_status()
-- Returns customer_id, full_name, points_total, status (Active/Inactive)
-- Provides quick overview of loyalty program participation
```

**High-Value Orders Analysis**
```sql
fn_orders_over_100_with_products()
-- Returns detailed breakdown of orders exceeding $100
-- Includes customer info, order details, and line items
```

**Customer Activity Tracking**
```sql
getcustomeractivity()
-- Comprehensive view of customer orders and returns
-- Links customer accounts with their transaction history
```

### 3. Database Design Principles

- **Normalized to Third Normal Form (3NF)** - Eliminates data redundancy  
- **Referential Integrity** - Foreign key constraints ensure data consistency  
- **Indexing Strategy** - Optimized query performance on frequently accessed columns  
- **Transaction Management** - ACID properties for reliable operations  
- **Data Validation** - Constraints and triggers for business rule enforcement  

## Technical Highlights

- **8+ interconnected tables** with proper relationships
- **6+ custom functions and stored procedures** for business logic
- **Multiple views** for complex reporting needs
- **Robust constraints** including CHECK, NOT NULL, and UNIQUE
- **Performance optimization** through strategic indexing

## Use Cases

This database supports:
- Customer account management and authentication
- Product catalog maintenance
- Order processing and fulfillment
- Returns and refund handling
- Loyalty rewards program administration
- Inventory tracking and reporting
- Sales analytics and reporting

## Installation & Setup

### Prerequisites
- PostgreSQL 12+ installed
- pgAdmin (recommended) or psql command line

### Steps
1. Create database:
```bash
   createdb inventory_db
```

2. Load schema:
```bash
   psql inventory_db < database_schema.sql
```

3. Verify installation:
```sql
   \dt -- List all tables
   \df -- List all functions
```

## Sample Queries

**Get all Platinum tier customers:**
```sql
SELECT customer_id, 
       fn_customer_reward_tier(customer_id) as tier
FROM customer_rewards
WHERE fn_customer_reward_tier(customer_id) = 'Platinum';
```

**Analyze high-value orders:**
```sql
SELECT * FROM fn_orders_over_100_with_products()
ORDER BY order_total DESC
LIMIT 10;
```

## Project Context

Completed as the final project for **Database & File Systems (CS 480)** at DePauw University. The project demonstrates understanding of:
- Relational database design
- SQL programming (DDL, DML, TCL)
- Stored procedures and functions
- Database normalization
- Query optimization
- Real-world application development

## Contributors

Group project completed collaboratively with 3 teammates, Fall 2024.

## Future Enhancements

Potential improvements for production deployment:
- Add user authentication and authorization
- Implement audit logging for transactions
- Create backup and recovery procedures
- Add API layer for application integration
- Expand analytics capabilities with materialized views

---

**For questions or collaboration opportunities, feel free to reach out!**
