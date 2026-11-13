# Mobile Wallet Fraud Detection System

A **desktop application** built with Python (Tkinter) and **MySQL** (XAMPP / MariaDB compatible).
(Optional Flask web version is also included as `app.py`.)

## Prerequisites
1. **XAMPP MySQL** (or standard MySQL server) running on port `3306`.
2. Install Python dependencies:
```bash
py -m pip install -r requirements.txt
```

## How to run
```bash
py desktop_app.py
```
* The database `wallet_fraud_db` and all necessary tables (`users`, `wallets`, `devices`, `transactions`, `fraud_alerts`) are created automatically on launch.
* (Optional web version: `py app.py`, then open `http://127.0.0.1:5000` in your browser).

## MySQL Configuration ([database.py](file:///c:/Users/Hp/Downloads/wallet_fraud_project/wallet_fraud_project/database.py))
Default settings work automatically with XAMPP:
* **Host**: `localhost`
* **Port**: `3306`
* **User**: `root`
* **Password**: `""` (empty)
* **Database**: `wallet_fraud_db`

*(You can override these with environment variables: `MYSQL_HOST`, `MYSQL_PORT`, `MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_DB`)*

## Login & Registration
| Role | Username | Password |
|:---|:---|:---|
| **Admin** | `admin` | `admin123` |
| **Customers** | Click **New account (Register)** in the app to create fresh Sender & Receiver accounts | Custom |

## Fraud detection rules ([fraud.py](file:///c:/Users/Hp/Downloads/wallet_fraud_project/wallet_fraud_project/fraud.py))
| Rule | Score |
|:---|:---|
| Amount above 50,000 Tk | +40 |
| 4 transactions within 10 minutes | +35 |
| Transaction from a new device | +30 |

* Score 0–29 -> **LOW**: transaction completes normally
* Score 30–59 -> **MEDIUM**: transaction completes + creates an alert for Admin review (Mark Safe/Fraud)
* Score 60+ -> **HIGH**: transaction is held; money does not move until Admin approves or rejects

## How to test
1. Run `py desktop_app.py`.
2. Click **New account (Register)** to create a Sender account (e.g. `karim`).
3. Click **New account (Register)** to create a Receiver account (e.g. `hasan`).
4. Log in as `karim` -> Dashboard -> use **Add Money** (e.g. `100,000` Tk).
5. Go to **Send Money** -> send `60,000` Tk to `hasan` with a new device name -> triggers **HIGH (70)** risk score (held for review).
6. Log in as `admin` -> go to **Fraud Alerts** tab to Approve or Reject the held transaction.
7. Go to **Transaction Report** tab to filter and export the full report as CSV.

## File structure
```
desktop_app.py        -> Desktop UI (Tkinter: Login, Wallet, Admin, Report, CSV)
services.py           -> Wallet business logic & database queries
fraud.py              -> Fraud detection engine & risk scoring
database.py           -> MySQL connection manager (PyMySQL) & auto-initialization
security.py           -> Password hashing (PBKDF2 SHA-256)
sql/mysql_schema.sql  -> Complete MySQL schema & admin seed
app.py                -> Optional Flask web version
templates/            -> Web UI templates
static/style.css      -> Web styling
```
