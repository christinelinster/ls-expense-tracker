# ⚡️ Expense Tracker CLI

A lightweight command-line expense tracking application built with Python and PostgreSQL. Provides a fast, terminal-based interface for logging expenses, searching transactions, and tracking spending.

## 🎥 Quick Demo
https://github.com/user-attachments/assets/cba9ed1a-5f90-4926-90e5-0d401abb5ae6


## Features

- **Quick Entry** - Add expenses with simple command syntax
- **Automatic Timestamps** - Each expense records creation date automatically
- **Search** - Case-insensitive keyword search across memo fields
- **Formatted Display** - Clean tabular output with automatic totals
- **Safe Deletion** - Remove individual expenses or clear all with confirmation
- **Persistent Storage** - PostgreSQL database for reliable data management

## Tech Stack

- **Backend**: Python 3.10+, psycopg2
- **Database**: PostgreSQL
- **Architecture**: Context managers, DictCursor, parameterized queries

## Installation

### Prerequisites

- Python 3.10+
- PostgreSQL 12+

### Setup

1. Clone the repository
```bash
git clone https://github.com/christinelinster/ls-expense-tracker.git
cd ls-expense-tracker
```

2. Install dependencies
```bash
pip install psycopg2-binary
```

3. Set up database
```bash
psql postgres
CREATE DATABASE expense_tracker;
\q

psql -d expense_tracker -f schema.sql
```

4. Make executable (Unix/Linux/Mac)
```bash
chmod +x expense
```

## Usage

### Commands

**View help**
```bash
./expense
```

**Add expense**
```bash
./expense add 45.50 "Grocery shopping"
```

**List all expenses**
```bash
./expense list
```
Output:
```
There are 3 expenses.
  1 | 2024-12-09 |        45.50 | Grocery shopping
  2 | 2024-12-09 |        12.00 | Coffee
  3 | 2024-12-08 |        30.00 | Gas
--------------------------------------------------
Total                            87.50
```

**Search expenses**
```bash
./expense search coffee
```

**Delete expense**
```bash
./expense delete 2
```

**Clear all expenses**
```bash
./expense clear
```

## Database Schema

```sql
CREATE TABLE expenses (
    id SERIAL PRIMARY KEY,
    amount DECIMAL(10, 2) NOT NULL,
    memo TEXT NOT NULL,
    created_on DATE NOT NULL
);

CREATE INDEX idx_expenses_date ON expenses(created_on);
```

## Architecture

### Project Structure

```
ls-expense-tracker/
├── expense            # Main CLI application
├── schema.sql        # Database schema
└── README.md         # Documentation
```

### Design Patterns

**Context Managers** - Ensures proper database connection cleanup and automatic transaction management

```python
@contextmanager
def _database_connect(self):
    connection = psycopg2.connect(dbname='expense_tracker')
    try:
        with connection:
            yield connection
    finally: 
        connection.close()
```

**Separation of Concerns** - CLI layer (`CLI` class) handles command routing while data layer (`ExpenseData` class) manages database operations

**Parameterized Queries** - All SQL queries use parameter binding for security

```python
cursor.execute("SELECT * FROM expenses WHERE id = %s", id)
```

## Technical Highlights

- **Python 3.10+ match-case** syntax for clean command routing
- **psycopg2 DictCursor** for column name access
- **Context managers** for resource management
- **ILIKE operator** for case-insensitive search
- **Formatted output** with right-aligned numbers and separator lines

## Roadmap

- [ ] Date range filtering for expense queries
- [ ] Category support (food, transport, entertainment)
- [ ] Edit command to update existing expenses
- [ ] CSV export functionality
- [ ] Budget tracking with alerts
- [ ] Configuration file for database credentials
- [ ] Comprehensive test suite with pytest

## License

MIT License

## Contact

Christine Lin  
[GitHub](https://github.com/christinelinster) | [LinkedIn](https://linkedin.com/in/christinelin19/) | [Portfolio](https://christine-lin.vercel.app/)  

Built with ⚡️ for developers who live in the terminal.
