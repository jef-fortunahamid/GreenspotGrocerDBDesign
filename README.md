# Designing a Scalable Database for Greenspot Grocer Using MySQL

## Business Task
In my previous SQL project, [Optimising Inventory for Strategic Warehouse Closure at Mint Classics Company](https://github.com/jef-fortunahamid/MintClassicsCo/blob/main/README.md) using basic SQL syntax to answer the business task, to close one storage facility by exploring the company's current inventory and make data-driven recommendations. This time around, I have a more complex challenge - designing a relational database for Greenspot Grocer, a rapidly growing online grocery store. the existing data, stored in a `.csv` file, is becoming difficult to manage. My goal is to restucture this data into a scalable MySQL database.

During this project, I'll be answering the following questions and more:
- How should the existing data be structured ainto tables?
- What relationships exist between these tables?
- Can the design handle future scaling needs?
- How can the design be verified?

## Techniques
To accomplish this tasks, I will use various SQL features, including:
- `CREATE TABLE`
- `PRIMARY KEY` and `FOREIGN KEY`
- `INSERT INTO`
-  `SELECT`
-  `JOIN`

## Data
The following is the sample screenshot of the data provided for our analysis.
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/f58c021f-cfda-43c4-a49f-3448d8f70c70)

## Analysis
### Step 1: Examine the Data
The first thing to do is to understand the existing data. As we anlyse our given data, in our case, the `.csv` file has 13 columns with various attributes.
1. Item num: A numerical identifier for each item (e.g., 1000, 2000).
2. sdscription: Describes the product (e.g., "Bennet Farm free-range eggs").
3. quantity on-hand: The amount of the product currently available (e.g., 29, 3).
4. cost: The cost to purchase the item (e.g., 2.35).
5. purchase date: The date the item was purchased (e.g., "2/1/2022").
6. vendor: Information about the vendor from whom the product was purchased (e.g., "Bennet Farms, Rt. 17 Evansville, IL 55446").
7. price: The selling price of the item (e.g., 5.49).
8. date sold: The date the item was sold (e.g., "2/2/2022").
9. cust: Customer ID or information (e.g., 198765, 202900).
10. Quantity: Quantity sold (e.g., 25, 2).
11. item type: The category of the item (e.g., "Dairy", "Produce").
12. Location: Location in the store or warehouse (e.g., "D12", "p12").
13. Unit: The unit in which the item is sold (e.g., "dozen", "bunch").

These columns can be grouped into different tables based on their relationships.

### Step 2: Plan the Database Structure
After examining the data, the next step is to create a robust relational database plan that allows for scalability and detailes tracking. Looking at the following image we can base our tables on this:
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/c770f742-dfb8-4e67-8d7b-f1b19a89d8bf)
Based on our requirements, the following tables are propsed:
1. `Products` Table: This table will serve as the central repository for all products sold by Greenspot Grocer. Initially using `item num`, this column has been renamed to `item_id` for standardisation. The table will include:
  - `Item num`: The unique identifier for each product. 
  - `description`: A brief description of the product.
  - `item_type`: The category to which the product belongs.
  - `unit`: The unit in which the product is sold.
  - `location`: Where the product is located in the warehouse or store.
  - `cost`: The cost of acquiring the product.
  - `vendor_id`: A reference to the Vendor table for vendor details.
2. `Inventory` Table: This table will keep track of the available stock for each product. Given that we might have multiple records for the same item_id (for tracking different batches, purchase dates, etc.), a separate id column will be used as the primary key. It will include:
  - `id`: A unique identifier for each inventory record.
  - `item_id`: A reference to the Products table.
  - `quantity_on_hand`: The current stock level of the product.
  - `purchase_date`: The date the product was last purchased or restocked.
3. `Vendor` Table: This table will store details about the vendors from whom Greenspot Grocer purchases products. It will include:
  - `vendor_id`: A unique identifier for each vendor.
  - `vendor_name`: The name of the vendor.
  - `vendor_address`: The address of the vendor.
4. `Sales` Table: This table will capture sales transactions.
  - `sales_id`: A unique identifier for each sales transaction.
  - `item_id`: A reference to the Products table (renamed from item_num).
  - `date_sold`: The date of the sale.
  - `customer_id`: A reference to the Customer table (replaces the cust column).
  - `quantity`: The quantity sold in the transaction.
5. `Customer` Table: A new table created to replace the cust column in the Sales table. This table will hold customer information.
  - `customer_id`: A unique identifier for each customer (this replaces the cust column in the Sales table).
  - `name`: The name of the customer.
  - `address`: The physical address of the customer.
  - `email`: The email address of the customer.
  - `phone`: The contact number of the customer.

Step 3: Populate the Tables
Once the tables are created, the next step is to populate them with the sample data. You can do this using INSERT INTO statements.

Step 4: Test the Design
To verify the validity of our design, we can write SQL queries using JOIN operations to fetch data across tables:

sql
Copy code
-- Fetch details of items sold on a particular date
SELECT Products.description, Sales.date_sold, Sales.quantity
FROM Products
JOIN Sales ON Products.item_num = Sales.item_num
WHERE Sales.date_sold = '2022-02-02';
My Insights

Restructured the messy .csv data into four well-defined tables.
Established relationships between these tables through PRIMARY and FOREIGN KEYS.
Created a database design that not only accommodates the current data but is also scalable for future growth.
By utilizing MySQL and SQL queries, I've designed a robust and scalable database for Greenspot Grocer. This will help them immensely as they continue to grow.

Thank you for reading my SQL Database Design project! If you found this helpful and want to know more about my work, feel free to check out other projects in my LinkedIn featured section.
