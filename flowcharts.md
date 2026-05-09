# Pharmacy Management System - Complete System Flowchart

I have remade the flowchart into a **single, unified, and color-coded diagram**. This provides a holistic view of the entire application, starting from the login screen and branching out based on the user's role (Admin vs. User).

```mermaid
flowchart TD
    %% Styling and Color Coding
    classDef startEnd fill:#2ecc71,stroke:#27ae60,stroke-width:2px,color:#fff;
    classDef login fill:#3498db,stroke:#2980b9,stroke-width:2px,color:#fff;
    classDef dashboard fill:#9b59b6,stroke:#8e44ad,stroke-width:2px,color:#fff;
    classDef action fill:#f1c40f,stroke:#f39c12,stroke-width:2px,color:#333;
    classDef process fill:#ecf0f1,stroke:#bdc3c7,stroke-width:2px,color:#333;
    classDef database fill:#e74c3c,stroke:#c0392b,stroke-width:2px,color:#fff;

    Start([Start System]) ::: startEnd --> LoginPage{Login Page} ::: login
    
    LoginPage -- "Invalid" --> LoginPage
    LoginPage -- "Success" --> RoleCheck{Check Role} ::: login
    
    %% =======================
    %%      USER FLOW
    %% =======================
    RoleCheck -- "Role: User/Staff" --> UserDash[User Dashboard] ::: dashboard
    UserDash --> UserMenu{User Menu} ::: action
    
    %% User: Medicines
    UserMenu -->|View Medicines| ViewMed[Browse Medicines List] ::: process
    ViewMed --> Buy{Add to Cart?} ::: action
    Buy -- "Yes" --> Cart[Proceed to Cart] ::: process
    Cart --> Checkout[Checkout & Pay] ::: process
    Checkout --> DB_Sale[(Save Sale to DB)] ::: database
    DB_Sale --> DB_MedStock[(Deduct Stock)] ::: database
    DB_MedStock --> Receipt[View Receipt / Bill] ::: process
    Receipt --> UserDash
    Buy -- "No" --> UserDash
    
    %% User: Logs
    UserMenu -->|Activity Logs| ViewUserLogs[View Own Activity] ::: process
    ViewUserLogs --> DB_UserLogs[(Fetch Own Logs)] ::: database
    DB_UserLogs --> DisplayUserLogs[Display User Logs] ::: process
    DisplayUserLogs --> UserDash
    
    %% User: Logout
    UserMenu -->|Logout| LogoutUser[Logout] ::: login
    LogoutUser --> EndSession([End Session]) ::: startEnd
    
    
    %% =======================
    %%      ADMIN FLOW
    %% =======================
    RoleCheck -- "Role: Admin" --> AdminDash[Admin Dashboard] ::: dashboard
    AdminDash --> AdminMenu{Admin Menu} ::: action
    
    %% Admin: Medicines
    AdminMenu -->|Manage Medicines| AdminMedMenu{Medicines Action} ::: action
    AdminMedMenu -->|Add| AddMed[Enter New Medicine Data] ::: process
    AdminMedMenu -->|Delete| DelMed[Select Medicine to Delete] ::: process
    AdminMedMenu -->|Restock| Restock[Admin Purchase / Restock] ::: process
    AddMed --> DB_AdminMed[(Update Medicines DB)] ::: database
    DelMed --> DB_AdminMed
    Restock --> DB_AdminMed
    DB_AdminMed --> AdminDash
    
    %% Admin: Users
    AdminMenu -->|Manage Users| AdminUserMenu{Users Action} ::: action
    AdminUserMenu -->|Create| AddUser[Enter New User Details] ::: process
    AdminUserMenu -->|Delete| DelUser[Select User to Delete] ::: process
    AddUser --> DB_AdminUser[(Update Users DB)] ::: database
    DelUser --> DB_AdminUser
    DB_AdminUser --> AdminDash
    
    %% Admin: Reports
    AdminMenu -->|View Reports| AdminReportMenu{Reports Action} ::: action
    AdminReportMenu -->|Sales| AdminSales[Request Sales Data] ::: process
    AdminSales --> DB_AdminSales[(Fetch All Sales)] ::: database
    DB_AdminSales --> ShowAdminSales[Display All Sales] ::: process
    ShowAdminSales --> AdminDash
    
    AdminReportMenu -->|Logs| AdminLogs[Request All Logs] ::: process
    AdminLogs --> DB_AdminLogs[(Fetch All Logs)] ::: database
    DB_AdminLogs --> ShowAdminLogs[Display System Logs] ::: process
    ShowAdminLogs --> AdminDash
    
    %% Admin: Logout
    AdminMenu -->|Logout| LogoutAdmin[Logout] ::: login
    LogoutAdmin --> EndSession
```

### Color Legend:
* **Green `(Pills)`**: Start and End of the session.
* **Blue `(Diamonds/Rectangles)`**: Authentication events (Login, Role Check, Logout).
* **Purple `(Rectangles)`**: Main Dashboard Hubs.
* **Yellow `(Diamonds)`**: Decision points or Menu selections.
* **Light Gray `(Rectangles)`**: Standard processes or page views.
* **Red `(Cylinders)`**: Database interactions (fetching or updating data).
