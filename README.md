# Fufu Republic Data Modeling

## Task
Case Study: Fufu Republic
Overview:
Fufu Republic is a popular restaurant chain in Nigeria with multiple outlets nationwide. While the
core menu is standardized, some items vary by location (e.g., the Agege branch may sell
Chinese Rice, while the Lekki branch might not). Customers can order online through the
website or visit outlets for dine-in or take-out.
Payment Methods:
The restaurant accepts:
● Cash
● Debit card payments via Nomba POS terminals at outlets
● Online payments processed through gateways like Nomba Web Checkout, Paystack,
Interswitch etc.
Challenges:
1. Inventory Management:
Variations in customer demand and menu items across branches make it challenging to
maintain optimal stock levels.
2. Customer Experience:
The restaurant aims to improve the customer experience by offering personalized
promotions based on purchasing behavior.
Objective:
Fufu Republic wants to leverage data to:
● Understand sales trends across locations, payment methods, and dining options
(dine-in, take-out, online).
● Manage stock levels efficiently, reducing waste and ensuring availability.
● Enhance customer experience by analyzing purchasing habits and tailoring promotions
accordingly.

As a recently hired data engineer at Fufu Republic, you have been tasked with developing a
dimensional model to address the business's needs for data-driven decision-making.
1. Map out the necessary entities ,relationships and constraints: This should be a
model (Any degree of abstraction is okay)
2. Create a dimensional model:
○ Identify a business process of your choice
○ List the business question under the business process you selected
○ Identify the grain, dimensions and fact.


## Solution

### Entities

#### Product:

Description: This dimension holds information about the products or menu items offered by Fufu Republic. Each product has attributes that describe its type and price, which are key for sales analysis and inventory management.

##### Attributes:
    - ProductID (Primary Key): A unique identifier for each product/menu item.
    - Name: The name of the product (e.g., Fufu, Chinese Rice).
    - Product_type: The category of the product (e.g., Main Dish, Appetizer).
    - is_standard: Used to specify a standard menu
    - Description: A short description of the product.
    - Unit_price: The price of the product.

#### Branch:

Description: This entity represents the different restaurant branches of Fufu Republic across Nigeria. 
  ##### Attributes:
    - LocationID (Primary Key): A unique identifier for each branch.
    - name: The name of the branch (e.g., Lekki, Agege).
    - Address: Physical address of the branch.
    - City: the city the branch operates in (e.g., Ikeja)
    - State: The state in Nigeria the branch operates in (e.g Lagos, Abuja)

#### Customer:

Description: This dimension captures details about the customers of Fufu Republic. It is crucial for analyzing customer behavior, creating personalized promotions, and tracking sales based on customer demographics.
##### Attributes:
    - CustomerID (Primary Key): A unique identifier for each customer.
    - Title: The title of the customer (e.g., Mr, Mrs, Miss).
    - First_name: The first name of the customer.
    - Last_name: The last name of the customer.
    - Email_address: The email address of the customer for contact and marketing purposes.
    - Phone_no: The phone number of the customer.

#### ProductLocation:

Description: This dimension acts as a link between specific products and the locations where they are available. Since not all menu items are available in all branches, this table helps track the availability of each product in each branch.
##### Attributes:
    - ProductLocationID (Primary Key): A unique identifier for the availability of a product at a specific branch.
    - ProductID (Foreign Key): Links to the product being offered.
    - BranchID (Foreign Key): Links to the branch offering the product.
    - Is_available: A boolean flag indicating whether the product is available at that specific location (True or False).

#### Payment :

Description: This dimension stores the different payment methods that Fufu Republic accepts. It is used to track how customers are paying for their orders (e.g., cash, card, or online).
#####  Attributes:
    - PaymentID (Primary Key): A unique identifier for each payment method.
    - Payment_method: The method of payment (e.g., Cash, Debit Card, Online Payment).
    - Description: Additional details about the payment method (e.g., Nomba POS, Paystack).

#### Order:

Description: The central fact table that captures each sales transaction at Fufu Republic. It links to all the dimensions.
##### Attributes:
    - Order_id (Primary Key): A unique identifier for each sales transaction.
    - CustomerID (Foreign Key): Links to the customer who made the purchase.
    - BranchID (Foreign Key): Links to the branch where the sale occurred.
    - PaymentLocationID (Foreign Key): Links to the branch where the payment was processed.
    - PaymentID (Foreign Key): Links to the method of payment used in the transaction.
    - ProductID (Foreign Key): Links to the specific product sold in the transaction.
    - Dining_option: Indicates whether the order was dine-in, take-out, or an online order.
    - Order_date: The date and time when the order was placed.
    - Quantity_sold: The number of units of the product sold in the transaction.
    - Sub_total: The total cost of the order before taxes.
    - Tax_amt: The amount of tax applied to the order.
    - Total_due: The final total amount due after taxes.


#### Entity Relationships:
Branch_dim is related to Order_fact and ProductLocation_dim. Each branch can have multiple sales and manage different inventories of products.
Customer_dim is related to Order_fact, meaning each sale can be linked to a specific customer for analysis.
Product_dim is connected to both Order_fact and ProductLocation_dim, which allows tracking product sales and availability by location.
Payment_dim is related to Order_fact, allowing analysis of sales by different payment methods.
