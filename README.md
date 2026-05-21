# Delivery Sync

Automates syncing delivery confirmations from **Team Global Express (TGE)** into **Tall Emu** invoices.

## What it does

1. Logs into the TGE portal and pulls all shipments within a given date range
2. Checks each shipment's delivery status (Milestone field + Actual Delivery date)
3. For delivered shipments, extracts: delivery date, delivery time, and who signed for it
4. Logs into Tall Emu and finds the matching invoice by invoice number
5. Pastes the delivery details into the invoice's Description field

## Tech Stack

- Python 3.11+
- Playwright (async, Chromium) — browser automation
- python-dotenv — credentials management
- asyncio — async/await throughout

## Usage

```bash
python main.py
```

You'll be prompted to enter a date range:

```
Enter start date (DD/MM/YYYY): 01/05/2025
Enter end date (DD/MM/YYYY): 14/05/2025
```

The script runs in headed mode so you can watch it work. At the end it prints a summary: shipments found, delivered count, and how many invoices were updated successfully vs failed.

## Project Structure

```
├── main.py        # Entry point — date range input, orchestrates the flow
├── tge.py         # TGE portal automation (login, fetch shipments, check delivery)
├── tallemu.py     # Tall Emu automation (login, find invoice, update description)
├── models.py      # Shared data classes
└── .env           # Credentials (not committed)
```

## Setup

Install dependencies:

```bash
pip install playwright python-dotenv
playwright install chromium
```

Copy `.env.example` to `.env` and fill in your credentials:

```
TGE_URL=https://www.myteamge.com/
TGE_USERNAME=
TGE_PASSWORD=

TALLEMU_URL=
TALLEMU_USERNAME=
TALLEMU_PASSWORD=
```

## Notes

- Only shipments with a clear delivered Milestone status are processed — ambiguous statuses (in transit, pending, etc.) are skipped and logged
- Credentials are loaded from `.env` and never hardcoded
- Built for internal use — not intended for public distribution
