# Designing a Scalable Database for Greenspot Grocer Using MySQL

## Business Task
In my previous SQL project, [Optimising Inventory for Strategic Warehouse Closure at Mint Classics Company](https://github.com/jef-fortunahamid/MintClassicsCo/blob/main/README.md) using basic SQL syntax to answer the business task, to close one storage facility by exploring the company's current inventory and make data-driven recommendations. This time around, I have a more complex challenge - designing a relational database for Greenspot Grocer, a rapidly growing online grocery store. The existing data, stored in a `.csv` file, is becoming difficult to manage. My goal is to restructure this data into a scalable MySQL database.

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
- `SELECT`
- `JOIN`
- `TRIGGER`

## Data
The following is the sample screenshot of the data provided for our analysis.
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/f58c021f-cfda-43c4-a49f-3448d8f70c70)

## Analysis
### Step 1: Examine the Data
The first thing to do is to understand the existing data. As we analyse our given data, in our case, the `.csv` file has 13 columns with various attributes.
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
After examining the data, the next step is to create a robust relational database plan that allows for scalability and detailed tracking. Looking at the following image we can base our tables on this:
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/c770f742-dfb8-4e67-8d7b-f1b19a89d8bf)
Based on our requirements, the following tables are proposed:
1. `Products` Table: This table will serve as the central repository for all products sold by Greenspot Grocer. Initially using `item num`, this column has been renamed to `item_id` for standardisation. The table will include:
  - `Item num`: The unique identifier for each product. 
  - `description`: A brief description of the product.
  - `item_type`: The category to which the product belongs.
  - `unit`: The unit in which the product is sold.
  - `location`: Where the product is located in the warehouse or store.
  - `cost`: The cost of acquiring the product.
  - `vendor_id`: A reference to the `Vendor` table for vendor details.
2. `Inventory` Table: This table will keep track of the available stock for each product. Given that we might have multiple records for the same item_id (for tracking different batches, purchase dates, etc.), a separate id column will be used as the primary key. It will include:
  - `id`: A unique identifier for each inventory record.
  - `item_id`: A reference to the `Products` table.
  - `quantity_on_hand`: The current stock level of the product.
  - `purchase_date`: The date the product was last purchased or restocked.
  - `expiry_date`: The date of expiration of products (if applicable).
3. `Vendor` Table: This table will store details about the vendors from whom Greenspot Grocer purchases products. It will include:
  - `vendor_id`: A unique identifier for each vendor.
  - `vendor_name`: The name of the vendor.
  - `vendor_address`: The address of the vendor.
4. `Sales` Table: This table will capture sales transactions.
  - `sales_id`: A unique identifier for each sales transaction.
  - `item_id`: A reference to the `Products` table (renamed from item_num).
  - `purchase_date`: The date of the sale (replaces the date sold column).
  - `customer_id`: A reference to the Customer table (replaces the cust column).
  - `quantity`: The quantity sold in the transaction.
  - `price`: The price at which each item sold
  - `total_amount`: The total amount of the transaction of each item, calculated as `price * quantity`.
5. `Customer` Table: A new table created to replace the cust column in the Sales table. This table will hold customer information.
  - `customer_id`: A unique identifier for each customer (this replaces the cust column in the Sales table).
  - `name`: The name of the customer.
  - `address`: The physical address of the customer.
  - `email`: The email address of the customer.
  - `phone`: The contact number of the customer.

### Step 3: Design the Enhanced Entity-Relationship (EER) Diagram and Establish Relationships
After carefully examining the data and identifying the key entities, we utilized MySQL Workbench's EER Diagram Builder to create an Enhanced Entity-Relationship (EER) Diagram. This graphical tool allowed us to drag and drop tables, fields, and establish relationships using crow's foot notation (one of the ways to show relationships), offering a visual blueprint for our database structure. The EER Diagram serves as a comprehensive yet easy-to-understand representation of our tables and the relationships between them.
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/dc0ee13d-a69d-4aaa-b18c-86a7128aa887)

#### Establishing Relationships
We used the Crow's Foot notation to establish the following relationships between our tables:
1. **Products to Invetory**: A *one-to-many* relationship. One product can appear in multiple inventory records, but each inventory record relates to one and only one product. This relationship is implemented through the `item_id` foreign key in the `Inventory` table.
2. **Vendor to Products**: A *one-to-many* relationship. One vendor can supply multiple products, but each product is supplied by one vendor. This relationship is established through the `vendor_id` foreign key in the `Products` table.
3. **Products to Sales**: A *one-to-many* relationship. One product can be part of multiple sales transactions, but each sales transaction involves one and only one product. The `item_id` in the `Sales` table serves as the foreign key linking back to the `Products` table.
4. **Customer to Sales**: A *one-to-many* relationship. A customer can make multiple purchases, but each sales transaction is made by one and only one customer. This is managed through the `customer_id` foreign key in the `Sales` table.

These relationships are accurately represented in the EER Diagram, which ensures that our database will have referential integrity. This design will also make it easier to write SQL queries to retrieve and manipulate data, as it clearly defines how data in various tables is interconnected.
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/d5d3064d-a778-4b6b-b21a-acb2905c4da2)

### Step 4: Create and Populate the Tables
After successfully designing the EER Diagram, the next step is to implement this database structure in MySQL Workbench. Here's how to go about it.

**Create a New Schema**: This is simply creating our database, we will name it as `GreenspotGrocerDB`. Right-click on the "Schemas" pane (left side of the workbench) and choose "Create Schema" and this will pop out:
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/fc44f9d9-9b19-4d08-8c48-565fdc10522e)
Press "Apply". You can easily see this new database under the "Schema" pane.

![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/e774e2d6-d62a-4ef6-93fc-30b04c1575ee)

**Create Tables**: Now, we'll proceed to run a series of queries to create our five tables. We need to take note of our database structure that we used to built our EER Diagram. However, before we proceed to run `CREATE TABLE` queries, we need to run this query:
```sql
USE GreenspotGrocerDB;
```
This is to ensure that we are running our queries on the correct database, this is especially required if we have other databases installed in our server.

Now we run the following queries to create our table:
```sql
-- Vendor Table
DROP TABLE IF EXISTS Vendor;
CREATE TABLE Vendor (
    vendor_id INT PRIMARY KEY AUTO_INCREMENT
  , vendor_name VARCHAR(255) NOT NULL
  , vendor_address VARCHAR(255) NOT NULL
);

-- Products Table
DROP TABLE IF EXISTS Products;
CREATE TABLE Products (
    item_id INT PRIMARY KEY
  , description VARCHAR(255) NOT NULL
  , item_type VARCHAR(50) NOT NULL
  , unit VARCHAR(50) NOT NULL
  , location VARCHAR(50) NOT NULL
  , cost FLOAT NOT NULL
  , vendor_id INT NOT NULL
  , FOREIGN KEY (vendor_id) REFERENCES Vendor(vendor_id)
);

-- Inventory Table
DROP TABLE IF EXISTS Inventory;
CREATE TABLE Inventory (
    id INT PRIMARY KEY AUTO_INCREMENT
  , item_id INT NOT NULL
  , quantity_on_hand INT NOT NULL
  , purchase_date DATE NOT NULL
  , expiry_date DATE
  , FOREIGN KEY (item_id) REFERENCES Products(item_id)
);

-- Customer Table
DROP TABLE IF EXISTS Customer;
CREATE TABLE Customer (
    customer_id INT PRIMARY KEY
  , name VARCHAR(255) NOT NULL
  , address VARCHAR(255) NOT NULL
  , email VARCHAR(255) NOT NULL UNIQUE
  , phone VARCHAR(20) NOT NULL
);

-- Sales Table
DROP TABLE IF EXISTS Sales;
CREATE TABLE Sales (
    sales_id INT PRIMARY KEY AUTO_INCREMENT
  , item_id INT
  , date_sold DATE
  , customer_id INT
  , quantity INT
  , price FLOAT
  , total_amount FLOAT
  , FOREIGN KEY (item_id) REFERENCES Products(item_id)
  , FOREIGN KEY (customer_id) REFERENCES Customer(customer_id)
);
```
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/6a4fb3a8-45e3-4d70-bcfd-632b1f316f4a)

Once the tables are created, the next step is to populate them with the sample data. We can do this using INSERT INTO statements.

```sql
-- Insert data into Vendor Table
INSERT INTO Vendor (vendor_name, vendor_address) VALUES
  ('Bennet Farms', 'Rt. 17 Evansville, IL 55446')
, ('Freshness, Inc.', '202 E. Maple St., St. Joseph, MO 45678')
, ('Ruby Redd Produce, LLC', '1212 Milam St., Kenosha, AL, 34567')
;

-- Insert data into Products Table
INSERT INTO Products (item_id, description, item_type, unit, location, cost, vendor_id) VALUES
  (1000, 'Bennet Farm free-range eggs', 'Dairy', 'dozen', 'D12', 2.35, 1)
, (2000, "Ruby's Kale", 'Produce', 'bunch', 'p12', 1.29, 3)
, (1100, 'Freshness White beans', 'Canned', '12 ounce can', 'a2', 0.69, 2)
, (1222, 'Freshness Green beans', 'Canned', '12 ounce can', 'a3', 0.59, 2)
, (1223, 'Freshness Green beans', 'Canned', '36 ounce can', 'a7', 1.75, 2)
, (1224, 'Freshness Wax beans', 'Canned', '12 ounce can', 'a3', 0.65, 2)
, (2001, "Ruby's Organic Kale", 'Produce', 'bunch', 'po2', 2.19, 3)
;

-- Insert data into Inventory Table
INSERT INTO Inventory (item_id, quantity_on_hand, purchase_date, expiry_date) VALUES
  (1000, 29, '2022-02-01', NULL)
, (1100, 53, '2022-02-02', NULL)
, (2000, 3, '2022-02-12', NULL)
, (1222, 59, '2022-02-10', NULL)
, (1223, 12, '2022-02-10', NULL)
, (1224, 31, '2022-02-10', NULL)
, (2000, 28, '2022-01-12', NULL)
, (2001, 20, '2022-02-12', NULL)
, (1223, 17, '2022-02-15', NULL)
;

-- Insert data into Sales Table
INSERT INTO Sales (item_id, date_sold, customer_id, quantity, price, total_amount) VALUES
  (1000, '2022-02-02', 198765, 2, 5.49, 10.98)
, (1100, '2022-02-02', 202900, 2, 1.49, 2.98)
, (1000, '2022-02-04', 196777, 2, 5.99, 11.98)
, (1000, '2022-02-11', 277177, 4, 5.49, 10.98)
, (1222, '2022-02-12', 111000, 12, 1.29, 15.48)
, (1223, '2022-02-13', 198765, 5, 3.49, 17.45)
, (2001, '2022-02-13', 100988, 1, 6.99, 6.99)
, (2001, '2022-02-14', 202900, 12, 6.99,83.88)
, (2000, '2022-02-15', 111000, 2, 3.99, 7.98)
;

-- Insert data into Customer Table
INSERT INTO Customer (customer_id, name, address, email, phone) VALUES
  (111000, 'John Bailes', 'Bridgetown, WA 6255', 'john.b@google.com', 0897665511)
, (198765, 'Gary Smith', 'Perth, WA 6100', 'gary.smith@yahoo.com', 0477558114)
, (202900, 'Laura Gale', 'Mandurah, WA 6210', 'l.gale01@google.com', 0422111555)
, (196777, 'Ian Walker', 'Perth, Wa 6100', 'iwalker10@hotmail.com', 0499885512)
, (277177, 'Belinda Dole', 'Mandurah, WA 6210', 'beldole@google.com', 0897112258)
, (100988, 'Sandra Park', 'Perth, WA 6100', 's.park@yahoo.com', 0897075454)
;
```
We'll check each table.
```sql
SELECT *
FROM vendor;
```
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/0e3b4aed-e09c-45bc-9b74-30beca63e3a7)

```sql
SELECT *
FROM products;
```
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/0702fa66-0a59-4cd8-91b6-e59f471f786e)

```sql
SELECT *
FROM inventory;
```
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/3b6748a8-4ee0-4b31-9d96-0fb57d45c80d)

```sql
SELECT *
FROM sales;
```
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/234ae010-42c4-40b8-8fa3-38bfc165b262)

```sql
SELECT *
FROM customer;
```
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/3dbd0a99-336e-4bd9-a896-7061c3e857d9)


### Step 5: Test the Database Design
To verify the validity of our design, we can write SQL queries using JOIN operations to fetch data across tables:
```sql
-- Fetch details of total sales from each customer
SELECT
    c.customer_id
  , c.name
  , ROUND(SUM(s.total_amount), 2) AS total_amount_purchase
FROM sales s 
  JOIN customer c
    ON s.customer_id = c.customer_id
GROUP BY
    c.customer_id
  , c.name
;
```
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/27f7596a-97e1-4da7-9037-ef51faa4ae5f)

```sql
-- Fetch details of items sold at which date
SELECT
    p.item_id
  , p.description
  , s.date_sold
  , s.quantity
FROM products p 
  JOIN sales s 
    ON p.item_id = s.item_id
ORDER BY
  date_sold
;
```
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/cc942d65-d11e-4f88-85ca-6566bb30bc19)

```sql
-- Fetch details about the latest purchased order from each vendor(supplier) on each item
SELECT
    v.vendor_name
  , p.item_id
  , p.description
  , i.quantity_on_hand
  , MAX(i.purchase_date) AS latest_order_date
FROM products p
  JOIN vendor v
    ON p.vendor_id = v.vendor_id
  JOIN inventory i 
    ON p.item_id = i.item_id
GROUP BY
    v.vendor_name
  , p.item_id
  , p.description
  , i.quantity_on_hand
;
```
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/5a70e6e6-3c4b-4ecf-937c-c8c93d77f0a1)

These queries proved that our database design is sound, that the tables and table relationships we have selected can allow then retrieval of data from all the tables in the database.

While checking this database, we ran into two main issues:
- How can the database automatically update the number of items left in stock when we sell something?
- What can we do when we have more than one set of the same item (like from different shipments or batches) in our stock? How do we make sure sales are counted correctly for each one?

To solve these problems, let's discuss one problem at a time.

First problem, the database structure we've designed will support the basic operations of storing and tracking sales and inventory, but we woulkd need additional logic to automatically update the `quantity_on_hand` in the `Inventory` table when a sale is made. We can do this using SQL Triggers.

Here's an example:
```sql
DELIMITER //
CREATE TRIGGER after_sales_insert
AFTER INSERT
ON Sales FOR EACH ROW
BEGIN
  UPDATE Inventory
  SET quantity_on_hand = quantity_on_hand - NEW.quantity
  WHERE item_id - NEW.item_id;
END;
//
DELIMITER ;
```
This trigger will execute  after a new record is inserted into the `Sales` table. It will automatically decrease the `quantity_on_hand` for the corresponding `iten_id` in the `Inventory` table by the `quantity` sold.

The second problem, this is particularly important for perishable items where we might want to sell the oldest stock first (FIFO - First In, First Out). We'll need more advanced logic to handle sales transactions. We need a Trigger with a loop through the inventory records for the sold item.

### Conclusion
**The Process**
The task at hand was to design and implement a database for Greenspot Grocer, a rapidly growing, family-owned online grocery store. The journey started with an understanding of the business requirements and the current data structure,followed by database planning, design, and finally, implementation using MySQL workbench. One of the unique aspects of this project was the use of MySQL Workbench's EER Diagram builder to visually design the database structure. It's an invaluable tool for quickly sketching out relationships and ensuring that the tables are well-organised.

**Challenges and Solutions**
One of the most significant challenges was dealing with inventory management, especially whan handling multiple batches of the same item. The challenge was to ensure that the sales transactions correctly updated  the inventory, accounting for various purchase and expiry dates. After some brainstorming, we concluded that an SQL Trigger would be the most efficient way to handle this, ensuring that the items are sold in a FIFO manner. This is especially critical for perishable items.

Another challenge was data normalisation and ensuring the database is scalable. A `Customer` table was intoduced to replace the customer column in the `Sales` table. The item identifiers were standardised across tables, and additional columns were added to capture sales transactions more comprehensively.

**Sugestions for Future Improvements**
For future business endeavours, a dedicated table could be introduced to capture promotional offers and discounts. Additionally, data analytics tools could be integrated for real-time sales and inventory tracking. Furthermore, a column to track the 'source' of the sales (e.g. online, in-store, promotional event) could provide valuable insights for marketing strategies.

By combining technical prowess with a deep understanding of the business requirements, this project serves as a robust solution for Greenspot Grocer's database needs. Thank you for taking the time to review this project. 
