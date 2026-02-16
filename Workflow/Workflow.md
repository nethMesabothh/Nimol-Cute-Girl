# POS System Workflow - Activity Diagrams

## 1. Staff Authentication & Authorization Workflow

```mermaid
flowchart TD
    Start([Staff Login]) --> EnterCredentials[Enter Username/Password]
    EnterCredentials --> Validate{Valid Credentials?}
    Validate -->|No| ShowError[Show Error Message]
    ShowError --> EnterCredentials
    Validate -->|Yes| CheckRole{Check Role}
    CheckRole -->|Manager| ManagerAccess[Full System Access]
    CheckRole -->|Staff| StaffAccess[Limited Access: POS and View Products]
    ManagerAccess --> Dashboard[Access Dashboard]
    StaffAccess --> Dashboard
    Dashboard --> End([Session Active])
```

## 2. Product CRUD Workflow

```mermaid
flowchart TD
    Start([Product Management]) --> CheckRole{User Role?}
    CheckRole -->|Staff| ViewOnly[View Products Only]
    CheckRole -->|Manager| FullAccess[Full CRUD Access]

    ViewOnly --> Browse[Browse Product Catalog]
    Browse --> Search[Search/Filter Products]
    Search --> ViewDetails[View Product Details]
    ViewDetails --> End1([End])

    FullAccess --> SelectAction{Select Action}
    SelectAction -->|Create| CreateProduct[Create New Product]
    SelectAction -->|Read| Browse
    SelectAction -->|Update| UpdateProduct[Update Product]
    SelectAction -->|Delete| DeleteProduct[Delete Product]

    CreateProduct --> EnterDetails["Enter Product Details: Name, SKU, Barcode, Category, Price, Cost, Stock, Supplier"]
    EnterDetails --> UploadImage[Upload Product Images]
    UploadImage --> SetVariants[Set Product Variants]
    SetVariants --> Save[Save Product]
    Save --> End2([Product Created])

    UpdateProduct --> SelectProduct[Select Product to Update]
    SelectProduct --> EditDetails[Edit Product Information]
    EditDetails --> UpdateStock[Update Stock Quantities]
    UpdateStock --> SaveUpdate[Save Changes]
    SaveUpdate --> End3([Product Updated])

    DeleteProduct --> SelectDelete[Select Product to Delete]
    SelectDelete --> CheckDependencies{Has Dependencies?}
    CheckDependencies -->|Yes| ShowWarning[Show Warning Message]
    CheckDependencies -->|No| ConfirmDelete{Confirm Deletion?}
    ShowWarning --> ConfirmDelete
    ConfirmDelete -->|No| SelectDelete
    ConfirmDelete -->|Yes| Delete[Delete Product]
    Delete --> LogAudit[Log Audit Trail]
    LogAudit --> End4([Product Deleted])
```

## 3. POS Sales Transaction Workflow

```mermaid
flowchart TD
    Start([Start POS Transaction]) --> Login{Staff Logged In?}
    Login -->|No| Auth[Authenticate Staff]
    Auth --> Login
    Login -->|Yes| NewTransaction[Start New Transaction]

    NewTransaction --> AddItem[Add Item to Cart]
    AddItem --> CheckStock{Stock Available?}
    CheckStock -->|No| ShowOutOfStock[Show Out of Stock Message]
    ShowOutOfStock --> AddItem
    CheckStock -->|Yes| CheckExpiry[Check Expiry Date]

    CheckExpiry --> CheckDaysLeft{Days Until Expiry?}
    CheckDaysLeft -->|"<= 1 day"| ApplyDiscount1[Apply 20% Auto Discount]
    CheckDaysLeft -->|"<= 3 days"| ApplyDiscount2[Apply 10% Auto Discount]
    CheckDaysLeft -->|"> 3 days"| NoDiscount[No Discount]

    ApplyDiscount1 --> FIFOCheck[Check FIFO - Use Oldest Stock First]
    ApplyDiscount2 --> FIFOCheck
    NoDiscount --> FIFOCheck

    FIFOCheck --> AddToCart[Add to Cart with Expiry Info]
    AddToCart --> CartAction{More Items?}
    CartAction -->|Yes| AddItem
    CartAction -->|No| ViewCart[View Cart Summary]

    ViewCart --> ModifyCart{Modify Cart?}
    ModifyCart -->|Yes| CartOptions{Action?}
    CartOptions -->|Remove Item| RemoveItem[Remove Item]
    CartOptions -->|Change Quantity| ChangeQty[Change Quantity]
    CartOptions -->|Apply Discount| ApplyManualDiscount[Apply Manual Discount]
    RemoveItem --> ViewCart
    ChangeQty --> ViewCart
    ApplyManualDiscount --> ViewCart
    ModifyCart -->|No| CalculateTotal[Calculate Total, Tax, Discounts]

    CalculateTotal --> PaymentMethod{Select Payment Method}
    PaymentMethod -->|Cash| CashPayment[Process Cash Payment]
    PaymentMethod -->|QR Code| QRPayment[Process QR Payment]
    PaymentMethod -->|Split| SplitPayment[Process Split Payment]

    CashPayment --> EnterCash[Enter Cash Amount]
    EnterCash --> CalculateChange[Calculate Change]
    CalculateChange --> OpenDrawer[Open Cash Drawer]
    OpenDrawer --> ProcessPayment1[Process Payment]

    QRPayment --> GenerateQR[Generate QR Code]
    GenerateQR --> ScanQR[Customer Scans QR Code]
    ScanQR --> VerifyPayment{Payment Verified?}
    VerifyPayment -->|No| RetryQR[Retry Payment]
    RetryQR --> ScanQR
    VerifyPayment -->|Yes| ProcessPayment2[Process Payment]

    SplitPayment --> SelectMethods[Select Multiple Payment Methods]
    SelectMethods --> ProcessSplit[Process Each Payment]
    ProcessSplit --> ProcessPayment3[Process Payment]

    ProcessPayment1 --> UpdateInventory[Update Inventory Stock]
    ProcessPayment2 --> UpdateInventory
    ProcessPayment3 --> UpdateInventory

    UpdateInventory --> GenerateReceipt[Generate Receipt]
    GenerateReceipt --> PrintReceipt{Print Receipt?}
    PrintReceipt -->|Yes| Print[Print Receipt]
    PrintReceipt -->|No| SkipPrint[Skip Printing]
    Print --> SaveTransaction[Save Transaction]
    SkipPrint --> SaveTransaction

    SaveTransaction --> UpdateStock[Update Stock Levels]
    UpdateStock --> LogActivity[Log Staff Activity]
    LogActivity --> End([Transaction Complete])
```

## 4. Expiry Data Management Workflow

```mermaid
flowchart TD
    Start([Expiry Management System]) --> MonitorProducts[Monitor All Products with Expiry Dates]
    MonitorProducts --> CheckExpiryDates[Check Expiry Dates Daily]

    CheckExpiryDates --> EvaluateProduct{Evaluate Each Product}
    EvaluateProduct --> CalculateDays[Calculate Days Until Expiry]

    CalculateDays --> CheckThreshold{Check Alert Threshold}
    CheckThreshold -->|Expired| ExpiredAlert[Trigger Expired Alert]
    CheckThreshold -->|"<= 1 day"| CriticalAlert["Trigger Critical Alert - Apply 20% Auto Discount"]
    CheckThreshold -->|"<= 3 days"| WarningAlert["Trigger Warning Alert - Apply 10% Auto Discount"]
    CheckThreshold -->|"<= 7 days"| EarlyAlert[Trigger Early Alert]
    CheckThreshold -->|"> 7 days"| NoAlert[No Action Needed]

    ExpiredAlert --> NotifyManager[Notify Manager via Dashboard/Email/SMS]
    CriticalAlert --> NotifyManager
    WarningAlert --> NotifyManager
    EarlyAlert --> NotifyManager

    NotifyManager --> FIFOProcess[FIFO Processing]
    FIFOProcess --> SortByExpiry[Sort Stock by Expiry Date]
    SortByExpiry --> PrioritizeOld[Prioritize Oldest Stock for Sales]
    PrioritizeOld --> UpdatePriority[Update Stock Priority in System]

    UpdatePriority --> ApplyDiscountRules[Apply Auto Discount Rules]
    ApplyDiscountRules --> UpdatePricing[Update Product Pricing at POS]
    UpdatePricing --> GenerateReport[Generate Expiry Report]

    GenerateReport --> End([End])
    NoAlert --> End
```

## 5. Payment Processing Workflow

```mermaid
flowchart TD
    Start([Payment Processing]) --> SelectMethod{Select Payment Method}

    SelectMethod -->|Cash| CashFlow[Cash Payment Flow]
    SelectMethod -->|QR Code| QRFlow[QR Code Payment Flow]
    SelectMethod -->|Split| SplitFlow[Split Payment Flow]

    CashFlow --> EnterAmount[Enter Cash Amount Received]
    EnterAmount --> ValidateAmount{Amount >= Total?}
    ValidateAmount -->|No| ShowInsufficient[Show Insufficient Amount Error]
    ShowInsufficient --> EnterAmount
    ValidateAmount -->|Yes| CalculateChange[Calculate Change Amount]
    CalculateChange --> OpenDrawer[Open Cash Drawer]
    OpenDrawer --> CompleteCash[Complete Cash Payment]

    QRFlow --> GenerateQR[Generate Payment QR Code]
    GenerateQR --> DisplayQR[Display QR Code to Customer]
    DisplayQR --> WaitPayment[Wait for Customer Payment]
    WaitPayment --> CheckStatus{Payment Status?}
    CheckStatus -->|Pending| WaitPayment
    CheckStatus -->|Failed| ShowFailed[Show Payment Failed]
    ShowFailed --> Retry{Retry Payment?}
    Retry -->|Yes| GenerateQR
    Retry -->|No| SelectMethod
    CheckStatus -->|Success| VerifyPayment[Verify Payment Confirmation]
    VerifyPayment --> CompleteQR[Complete QR Payment]

    SplitFlow --> SelectMethods[Select Multiple Payment Methods]
    SelectMethods --> EnterSplit[Enter Amount for Each Method]
    EnterSplit --> ValidateSplit{Total = Transaction Total?}
    ValidateSplit -->|No| ShowError[Show Amount Mismatch Error]
    ShowError --> EnterSplit
    ValidateSplit -->|Yes| ProcessEach[Process Each Payment Method]
    ProcessEach --> CompleteSplit[Complete Split Payment]

    CompleteCash --> SavePayment[Save Payment Record]
    CompleteQR --> SavePayment
    CompleteSplit --> SavePayment

    SavePayment --> GenerateReceipt[Generate Payment Receipt]
    GenerateReceipt --> End([Payment Complete])
```

## 6. Staff Management Workflow

```mermaid
flowchart TD
    Start([Staff Management]) --> CheckRole{User Role?}
    CheckRole -->|Staff| Denied[Access Denied]
    CheckRole -->|Manager| ManagerAccess[Manager Access Granted]

    Denied --> End1([End])

    ManagerAccess --> SelectAction{Select Action}
    SelectAction -->|Create Staff| CreateStaff[Create New Staff Account]
    SelectAction -->|View Staff| ViewStaff[View Staff List]
    SelectAction -->|Update Staff| UpdateStaff[Update Staff Information]
    SelectAction -->|View Activity| ViewActivity[View Activity Logs]

    CreateStaff --> EnterStaffInfo["Enter Staff Information: Name, Contact, Position"]
    EnterStaffInfo --> AssignRole{Assign Role}
    AssignRole -->|Staff| SetStaffRole[Set as Staff Role]
    AssignRole -->|Manager| SetManagerRole[Set as Manager Role]
    SetStaffRole --> SetPermissions[Set Permissions]
    SetManagerRole --> SetPermissions
    SetPermissions --> CreateAccount[Create Account with Password]
    CreateAccount --> End2([Staff Created])

    ViewStaff --> DisplayList[Display Staff List]
    DisplayList --> FilterSearch[Filter/Search Staff]
    FilterSearch --> ViewDetails[View Staff Details]
    ViewDetails --> End3([End])

    UpdateStaff --> SelectStaff[Select Staff Member]
    SelectStaff --> EditInfo[Edit Staff Information]
    EditInfo --> UpdateRole{Update Role?}
    UpdateRole -->|Yes| ChangeRole[Change Role]
    ChangeRole --> UpdatePermissions[Update Permissions]
    UpdateRole -->|No| SaveChanges[Save Changes]
    UpdatePermissions --> SaveChanges
    SaveChanges --> LogChange[Log Change in Audit Trail]
    LogChange --> End4([Staff Updated])

    ViewActivity --> SelectStaffActivity[Select Staff Member]
    SelectStaffActivity --> DisplayLogs[Display Activity Logs]
    DisplayLogs --> FilterLogs[Filter by Date/Action]
    FilterLogs --> ExportLogs{Export Logs?}
    ExportLogs -->|Yes| Export[Export to CSV/PDF]
    ExportLogs -->|No| End5([End])
    Export --> End5
```

## 7. Complete POS System Main Workflow

```mermaid
flowchart TD
    Start([System Start]) --> Login[Staff Login]
    Login --> Authenticate{Authentication Success?}
    Authenticate -->|No| Login
    Authenticate -->|Yes| CheckRole{User Role?}

    CheckRole -->|Manager| ManagerDashboard["Manager Dashboard: Product Management, Staff Management, Reports, Settings, POS"]
    CheckRole -->|Staff| StaffDashboard["Staff Dashboard: POS, View Products"]

    ManagerDashboard --> SelectFeature{Select Feature}
    StaffDashboard --> SelectFeature

    SelectFeature -->|Product Management| ProductWorkflow[Product CRUD Workflow]
    SelectFeature -->|Staff Management| StaffWorkflow[Staff Management Workflow]
    SelectFeature -->|POS| POSWorkflow[POS Sales Transaction]
    SelectFeature -->|Reports| ReportsWorkflow[View Reports]
    SelectFeature -->|Settings| SettingsWorkflow[System Settings]

    ProductWorkflow --> SelectFeature
    StaffWorkflow --> SelectFeature
    POSWorkflow --> SelectFeature
    ReportsWorkflow --> SelectFeature
    SettingsWorkflow --> SelectFeature

    SelectFeature -->|Logout| Logout[Logout]
    Logout --> End([System End])
```
