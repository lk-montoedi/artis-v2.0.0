# Project Overview

This project is an ASP.NET Web Forms application that serves as an e-commerce platform for artisans. It allows artisans to register, list their products (art, accessories, clothing), and sell them through an auction-based system. Users can browse products, place bids, and purchase items. The application also includes features like a shopping cart, wishlist, user profiles, and order management.

The application follows a client-server architecture, where the ASP.NET front-end communicates with a WCF service for all data operations.

## Key Technologies

*   **Frontend:** ASP.NET Web Forms, HTML, CSS, JavaScript, jQuery, Bootstrap
*   **Backend:** C#, WCF (Windows Communication Foundation)

# Building and Running

1.  **Open in Visual Studio:** This is a Visual Studio project. Open the `Project_Backup.sln` file in Visual Studio.
2.  **WCF Service Dependency:** The application requires a running instance of the backend WCF service. The service endpoint is configured in `Web.config` to be `http://localhost:14011/Service1.svc`. Make sure the service is running at this address before launching the web application.
3.  **Run the Application:** Once the solution is open and the WCF service is running, you can start the application by pressing `F5` or clicking the "Start Debugging" button in Visual Studio. This will build the project and open the website in your default browser.

# Development Conventions

*   **Web Forms:** The project uses the ASP.NET Web Forms model, with `.aspx` files for the user interface and corresponding `.aspx.cs` code-behind files for server-side logic.
*   **Master Page:** The overall site layout and structure are defined in the `Site.Master` file. All other pages inherit from this master page.
*   **WCF Service Client:** The application communicates with the backend WCF service using a generated client proxy (`Service1Client`). All data access is performed through this client.
*   **Static Assets:** CSS, JavaScript, and images are located in the `assets` directory.
