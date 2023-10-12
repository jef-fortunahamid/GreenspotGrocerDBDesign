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
After examining the data, ythe next step is tocreate a relationship database plan. The following tables come to mind:
1. Products Table: Will include columns like `Item num`, `description`, `item type`, `Unit`, `Location`, and `cost`.
2. Inventory Table: Will store `Item num`, `quantity on-hand`, and `purchase date`.
3. Sales Table: Will capture sales transactions with columns like `Item num`, `date sold`, `customer`, and q`uantity`.
4. Vendor Table: Will store vendor details like `vendor name` and `vendor address`.
![image](https://github.com/jef-fortunahamid/GreenspotGrocerDBDesign/assets/125134025/c770f742-dfb8-4e67-8d7b-f1b19a89d8bf)
