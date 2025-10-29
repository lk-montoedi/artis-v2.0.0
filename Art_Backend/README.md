# Project Overview: Artis Backend

This project is the backend for an online marketplace called "Artis At Auction". It is a WCF (Windows Communication Foundation) service written in C# using the .NET Framework.

## Key Technologies

*   **.NET Framework 4.7.2**
*   **WCF (Windows Communication Foundation):** The core technology for building the service.
*   **C#:** The programming language used.
*   **LINQ to SQL:** Used for data access to a local SQL Server database file (`.mdf`).
*   **SQL Server (LocalDB):** The database used for storing data.

## Architecture

The project follows a typical WCF service architecture:

*   **`IService1.cs`:** Defines the service contract, which is the public interface of the service. It specifies the operations that clients can call.
*   **`Service1.svc.cs`:** Implements the service contract and contains the business logic for each operation.
*   **`Database.dbml`:** The LINQ to SQL data model that maps C# classes to database tables. The actual data context is generated in `Database.designer.cs`.
*   **Data Transfer Objects (DTOs):** A number of classes (e.g., `CustomerDT`, `ProductDT`, `BidDT`) are used to transfer data between the service and its clients.
*   **`Web.config`:** The configuration file for the service, containing the database connection string and service model settings.

## Features

The service provides a comprehensive set of features for an online marketplace:

*   **User Management:** Registering, retrieving, and managing customers and sellers.
*   **Product Management:** Adding, removing, editing, and retrieving products, which can be categorized as Art, Clothing, or Accessories.
*   **Bidding System:** Creating, editing, and retrieving bids for products.
*   **Shopping Cart:** Adding and removing items from a shopping cart.
*   **Invoicing:** Creating invoices from the shopping cart, calculating VAT, and handling order fulfillment.
*   **Analytics:** Providing data for graphs and reports, such as revenue, product statistics, and user demographics.

# Building and Running

To build and run this project, you will need Visual Studio with the ".NET desktop development" and "ASP.NET and web development" workloads installed.

1.  **Open the solution:** Open the `Art_Backend.sln` file in Visual Studio.
2.  **Build the solution:** Build the solution by pressing `Ctrl+Shift+B` or by selecting "Build > Build Solution" from the menu.
3.  **Run the service:**
    *   Set the `Art_Backend` project as the startup project.
    *   Press `F5` to start the service. This will launch the WCF Test Client, which you can use to test the service's operations.
    *   Alternatively, you can publish the service to a web server like IIS.

# Development Conventions

*   **Data Access:** Data access is performed using LINQ to SQL. The `DatabaseDataContext` class is used to query and update the database.
*   **Service Operations:** All service operations are defined in the `IService1` interface and implemented in the `Service1` class.
*   **Data Transfer:** DTOs are used to pass data between the client and the service. This helps to decouple the service's internal data model from the data model exposed to clients.
*   **Error Handling:** The current implementation has basic error handling using `try-catch` blocks, but it could be improved with more specific exception handling and fault contracts.
*   **Configuration:** The database connection string and other service settings are stored in the `Web.config` file.

# Models

This section details the Data Transfer Objects (DTOs) used in the service.

### BidDT
Represents the data of a bid on a product, including the current amount and expiration time.
- `Id` (int)
- `CustomerId` (int?)
- `Amount` (decimal?)
- `ProductId` (int?)
- `Expired` (string)
- `ExpiresAt` (DateTime?)
- `Created` (DateTime?)

### BidEditDT
Used to edit an existing bid, containing the new amount and the customer placing the bid.
- `CustomerId` (int)
- `ProductId` (int)
- `Amount` (decimal)

### BidNewDT
Used to create a new bid, containing the initial amount and the customer placing the bid.
- `CustomerId` (int)
- `ProductId` (int)
- `Amount` (decimal)

### CartDT
Represents a customer's shopping cart, including the total price and a list of items.
- `Total` (decimal?)
- `Items` (List<CartItemDT>)
- `CreatedAt` (DateTime?)
- `ModifiedAt` (DateTime?)

### CartItemDT
Represents an item within a shopping cart.
- `Id` (int)
- `ProductId` (int?)
- `Name` (string)
- `Price` (decimal)
- `Images` (string)

### CustomerDT
Represents the data of a customer, including personal details and purchase history.
- `Name` (string)
- `Surname` (string)
- `Email` (string)
- `ContactNumber` (string)
- `StreetAddress` (string)
- `City` (string)
- `Province` (string)
- `Image` (string)
- `CreatedAt` (DateTime)
- `NoPurchases` (int?)

### InvoiceDT
Represents an invoice, including the total amount, VAT, and a list of purchased items.
- `Id` (int)
- `Date` (DateTime)
- `Total` (decimal)
- `GrandTotal` (decimal)
- `VAT` (decimal)
- `Shipping` (decimal)
- `Items` (List<InvoiceItemDT>)

### InvoiceItemDT
Represents an item within an invoice.
- `ProductName` (string)
- `Price` (decimal?)

### ProductDT
Represents a product, including its details, price, and seller information.
- `Id` (int)
- `Name` (string)
- `Description` (string)
- `Price` (decimal)
- `IsActive` (string)
- `Quantity` (int?)
- `Added` (DateTime?)
- `Color` (string)
- `SoldAt` (DateTime?)
- `SellerId` (int)
- `Images` (string)
- `ExtraAttributes` (List<string>)

### ProductEdit
Used to edit an existing product's details.
- `Id` (int)
- `Name` (string)
- `Description` (string)
- `Price` (decimal)

### ProductReg
Used to register a new product.
- `Name` (string)
- `Description` (string)
- `Price` (decimal)
- `Quantity` (int)
- `Color` (string)
- `SellerId` (int)
- `Images` (string[])
- `Category` (string)

### SellerDT
Represents a seller, including their business details and sales history.
- `Id` (int)
- `Name` (string)
- `Surname` (string)
- `Type` (string)
- `Email` (string)
- `ContactNumber` (string)
- `StreetAddress` (string)
- `City` (string)
- `Province` (string)
- `Image` (string)
- `CreatedAt` (DateTime)
- `BusinessName` (string)
- `NoSale` (int?)
- `Description` (string)

# Endpoints

This section lists all the service endpoints defined in `IService1.cs`, with explanations of their functionality.

### Login

- `string Login(string username, string password)`: Authenticates a user based on email and password. Returns the user's ID and type if successful, otherwise null.
- `string GetUserRole(int id)`: Retrieves the user's role (e.g., "Customer", "Seller") based on their user ID.
- `User GetUser(int id)`: Retrieves a user's details by their ID.

### Customer

- `bool credit(decimal funds, int id)`: Adds funds to a user's account balance.
- `bool debit(decimal funds, int id)`: Deducts funds from a user's account balance.
- `string RegisterCustomer(User user)`: Registers a new customer, creates a cart for them, and returns a success or error message.
- `bool RemoveCustomer(int customerId)`: Removes a customer from the database.
- `bool EditCustomer(CustomerDT newCustomer)`: (Not Implemented) Edits a customer's details.
- `CustomerDT GetCustomer(int customerId)`: Retrieves a customer's details by their ID.
- `List<CustomerDT> GetCustomers()`: Retrieves a list of all customers.
- `List<Notification> GetNotifications(int userId)`: Retrieves a list of notifications for a specific user.

### Seller

- `bool RegisterSeller(int id, string description, string business, string image)`: Upgrades a customer to a seller, adding business details.
- `bool RemoveSeller(int sellerId)`: Removes a seller from the database.
- `bool EditSeller(SellerDT newSeller)`: (Not Implemented) Edits a seller's details.
- `SellerDT GetSeller(int sellerId)`: Retrieves a seller's details by their ID.
- `List<SellerDT> GetSellers()`: Retrieves a list of all sellers.

### Product

- `int AddProduct(ProductReg product)`: Adds a new product to the database and returns the new product's ID.
- `bool RemoveProduct(int productId)`: Removes a product and any associated bids from the database.
- `bool EditProduct(ProductEdit product)`: Edits the name, description, and price of a product.
- `ProductDT GetProduct(int productId)`: Retrieves a single product's details, including extra attributes based on its category.
- `List<ProductDT> GetProducts()`: Retrieves a list of all products.
- `bool MakeActive(int productId)`: Sets a product's status to "active".
- `bool MakeInactive(int productId)`: Sets a product's status to "inactive".
- `bool AddAccessory(int productId, Accessory accessory)`: Adds accessory-specific details to a product.
- `bool AddArt(int productId, Art art)`: Adds art-specific details to a product.
- `bool AddClothing(int productId, Clothing clothing)`: Adds clothing-specific details to a product.

### Cart

- `bool AddToCart(int? customerId, int productId)`: Adds a product to a customer's cart after a successful bid. Handles bid resolution logic.
- `bool RemoveCartItem(int customerId, int itemId)`: Removes an item from a customer's cart.
- `CartDT GetCart(int customerId)`: Retrieves the contents of a customer's cart.
- `bool ClearCart(int customerId)`: Removes all items from a customer's cart.
- `bool isInBid(int productId)`: Checks if a product is currently in a bid.

### Bid

- `BidDT AddBid(BidNewDT bid)`: Creates a new bid for a product.
- `BidDT EditBid(BidEditDT bid)`: Updates an existing bid with a new amount and customer.
- `BidDT GetBid(int productId)`: Retrieves the current bid for a product.
- `bool CloseBid(int bidId)`: Marks a bid as closed.
- `bool IsOpen(int productId)`: Checks if a bid is currently open for a product.
- `bool IsExpired(int productId)`: Checks if a bid for a product has expired.
- `List<BidDT> GetBids()`: Retrieves a list of all bids.

### Invoice

- `void setAddress(int customerId, string streetAddress, string city, string province)`: Sets the address for a customer.
- `InvoiceDT MakeInvoice(int customerId)`: Creates an invoice from the items in a customer's cart, processes the payment, and clears the cart.
- `List<InvoiceDT> GetInvoices(int customerId)`: Retrieves a list of all invoices for a customer.
- `InvoiceDT GetInvoice(int invoiceId)`: Retrieves a single invoice by its ID.

### Filtering and Searching

- `List<ProductDT> GetClothing()`: Retrieves all products in the "Clothing" category.
- `List<ProductDT> GetAccessories()`: Retrieves all products in the "Accessory" category.
- `List<ProductDT> GetArts()`: Retrieves all products in the "Art" category.
- `List<BidDT> GetBasedPrice(string type, decimal min, decimal max)`: Retrieves a list of bids based on product type and price range.
- `List<BidDT> Search(string query)`: Searches for bids based on a product name query.

### Analytics

- `Dictionary<string, decimal> GetRevenue()`: Retrieves total revenue from sales and registrations.
- `Dictionary<string, int> GetActiveInactiveProductCounts()`: Retrieves the count of active and inactive products.
- `Dictionary<string, int> GetUserRegistrationsByMonth()`: Retrieves the number of user registrations per month.
- `Dictionary<string, int> GetUserLocationsByProvince()`: Retrieves the number of users per province.
- `Dictionary<string, int> GetArtSoldUnsoldCounts()`: Retrieves the count of sold and unsold art products.
- `Dictionary<string, int> GetAccessoriesSoldUnsoldCounts()`: Retrieves the count of sold and unsold accessory products.
- `Dictionary<string, int> GetClothingSoldUnsoldCounts()`: Retrieves the count of sold and unsold clothing products.