erDiagram
    %% Auth & RBAC
    ROLES ||--o{ USERS : "has"
    USERS ||--o| CUSTOMER_PROFILES : "is"
    USERS ||--o| STAFF_PROFILES : "is"
    USERS ||--o{ AUDIT_LOGS : "acts"

    %% HR & KPI
    DEPARTMENTS ||--o{ STAFF_PROFILES : "belongs_to"
    DEPARTMENTS ||--o{ KPI_METRICS : "defines"
    STAFF_PROFILES ||--o{ STAFF_KPI_RECORDS : "evaluates"
    KPI_METRICS ||--o{ STAFF_KPI_RECORDS : "scores"
    STAFF_PROFILES ||--o{ PAYROLL_RECORDS : "receives"
    BONUS_FUNDS ||--o{ PAYROLL_RECORDS : "funds"

    %% Customer & Deposits
    CUSTOMER_PROFILES ||--o{ DEPOSIT_CONTRACTS : "signs"
    STAFF_PROFILES ||--o{ DEPOSIT_CONTRACTS : "manages"
    CUSTOMER_PROFILES ||--o{ BANK_ACCOUNTS : "owns"

    %% Fund & Investment
    STAFF_PROFILES ||--o{ INVESTMENT_FUNDS : "manages_fund"
    INVESTMENT_FUNDS ||--o{ PORTFOLIO_ASSETS : "holds"
    PORTFOLIO_ASSETS ||--o{ FUND_TRANSACTIONS : "trades"

    %% Ledger Links
    DEPOSIT_CONTRACTS ..o{ GENERAL_LEDGER : "references"
    FUND_TRANSACTIONS ..o{ GENERAL_LEDGER : "references"
    PAYROLL_RECORDS ..o{ GENERAL_LEDGER : "references"

    USERS {
        uuid user_id PK
        int role_id FK
        string username
        string email
        string status
    }

    CUSTOMER_PROFILES {
        uuid customer_id PK, FK
        string full_name
        string identity_card_number
        string kyc_status
    }

    STAFF_PROFILES {
        uuid staff_id PK, FK
        int dept_id FK
        string staff_code
        decimal base_salary
    }

    DEPOSIT_CONTRACTS {
        uuid contract_id PK
        uuid customer_id FK
        uuid staff_assigned_id FK
        decimal principal_amount
        decimal interest_rate
        timestamp maturity_date
        string status
    }

    INVESTMENT_FUNDS {
        uuid fund_id PK
        string fund_name
        uuid manager_id FK
        decimal current_nav
    }

    PORTFOLIO_ASSETS {
        uuid asset_id PK
        uuid fund_id FK
        string asset_code
        decimal quantity
        decimal current_market_price
    }

    GENERAL_LEDGER {
        uuid entry_id PK
        string transaction_type
        decimal amount
        uuid reference_id
    }
