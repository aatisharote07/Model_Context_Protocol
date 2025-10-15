# Expense Tracker MCP Server

A Model Context Protocol (MCP) server that enables LLMs to interact with a personal expense tracking system. This server allows you to manage your expenses through natural language conversations with AI assistants like Claude.

## Overview

This project implements an MCP server that connects your expense tracking database to AI assistants. Instead of manually entering data or using a traditional UI, you can simply tell the AI about your expenses, ask for summaries, or request insights - and the AI will interact with your expense database directly.

## Features

- **Add Expenses**: Record expenses with date, amount, category, subcategory, and notes through natural conversation
- **List Expenses**: Query expenses within any date range
- **Summarize by Category**: Get spending summaries organized by category
- **Comprehensive Categories**: Pre-defined categories covering food, transport, housing, utilities, health, entertainment, and more
- **Flexible Subcategories**: Detailed subcategories for precise expense tracking

## Architecture

The server uses:
- **FastMCP**: Framework for building MCP servers
- **SQLite**: Lightweight database for storing expense records
- **JSON Categories**: Configurable category structure

## Categories Supported

The system includes 20 main categories with detailed subcategories:

- 🍔 **Food**: groceries, dining out, coffee, snacks, delivery
- 🚗 **Transport**: fuel, public transport, cabs, parking, tolls
- 🏠 **Housing**: rent, maintenance, repairs, furnishing
- 💡 **Utilities**: electricity, water, internet, phone
- 🏥 **Health**: medicines, doctor visits, fitness, insurance
- 📚 **Education**: books, courses, subscriptions, workshops
- 👨‍👩‍👧 **Family & Kids**: school fees, daycare, toys, clothes
- 🎬 **Entertainment**: movies, streaming, games, outings
- 🛍️ **Shopping**: clothing, electronics, accessories, appliances
- 📱 **Subscriptions**: SaaS tools, cloud services, streaming
- 💅 **Personal Care**: salon, grooming, cosmetics
- 🎁 **Gifts & Donations**: personal gifts, charity, festivals
- 💰 **Finance & Fees**: bank charges, interest, brokerage
- 💼 **Business**: software, hosting, marketing, travel
- ✈️ **Travel**: flights, hotels, transportation, food
- 🏡 **Home**: household supplies, cleaning, kitchenware
- 🐾 **Pet**: food, vet, grooming, supplies
- 📋 **Taxes**: income tax, GST, filing fees
- 📈 **Investments**: mutual funds, stocks, crypto
- 📦 **Miscellaneous**: uncategorized items

## Installation

### Prerequisites

- Python 3.11 or higher
- An MCP-compatible client (like Claude Desktop)

### Setup

1. Clone or download this project:
```bash
git clone <your-repo-url>
cd expense-tracker-mcp-server
```

2. Install dependencies:
```bash
pip install -e .
```

3. The database will be automatically initialized on first run.

## Usage

### Running the Server

```bash
python main.py
```

### Configuring with Claude Desktop

Add this server to your Claude Desktop configuration file:

**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`

**Windows**: `%APPDATA%/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "expense-tracker": {
      "command": "python",
      "args": ["/absolute/path/to/expense-tracker-mcp-server/main.py"]
    }
  }
}
```

Replace `/absolute/path/to/` with the actual path to your project directory.

### Example Conversations

Once connected, you can interact with your expense tracker naturally:

**Adding Expenses:**
> "I spent ₹1,200 on groceries today"
> 
> "Add expense: ₹500 for dinner at restaurant on October 10th"
>
> "Uber ride cost me ₹250 yesterday"

**Querying Expenses:**
> "Show me all my expenses from last week"
>
> "What did I spend in September?"
>
> "List my food expenses this month"

**Getting Summaries:**
> "Summarize my spending for October by category"
>
> "How much did I spend on transport this month?"
>
> "Give me a breakdown of all my expenses this year"

## Database Schema

The SQLite database contains a single `expenses` table:

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary key (auto-increment) |
| date | TEXT | Date of expense (YYYY-MM-DD format) |
| amount | REAL | Expense amount |
| category | TEXT | Main category |
| subcategory | TEXT | Detailed subcategory |
| note | TEXT | Additional notes |

## Available Tools

The MCP server exposes three tools to the LLM:

### 1. `add_expense`
Adds a new expense entry to the database.

**Parameters:**
- `date` (required): Date in YYYY-MM-DD format
- `amount` (required): Expense amount
- `category` (required): Main category
- `subcategory` (optional): Subcategory
- `note` (optional): Additional notes

### 2. `list_expenses`
Lists expenses within a date range.

**Parameters:**
- `start_date` (required): Start date (inclusive)
- `end_date` (required): End date (inclusive)

### 3. `summarize`
Summarizes total spending by category within a date range.

**Parameters:**
- `start_date` (required): Start date (inclusive)
- `end_date` (required): End date (inclusive)
- `category` (optional): Filter by specific category

## Resources

The server provides a `expense://categories` resource that exposes the full category structure in JSON format. The LLM can access this to suggest appropriate categories and subcategories.

## Customization

### Adding Custom Categories

Edit `categories.json` to add or modify categories:

```json
{
  "your_category": [
    "subcategory1",
    "subcategory2",
    "other"
  ]
}
```

The server reads this file fresh on each request, so no restart is needed.

## Benefits of MCP Integration

- **Natural Language Interface**: Talk to your expense tracker instead of filling forms
- **Context-Aware**: The LLM understands your spending patterns and can provide insights
- **Automated Categorization**: The AI can suggest appropriate categories based on your description
- **Smart Queries**: Ask complex questions about your spending without writing SQL
- **Multi-Modal**: Combine expense tracking with other tasks in the same conversation

## File Structure

```
expense-tracker-mcp-server/
├── main.py              # MCP server implementation
├── categories.json      # Category definitions
├── pyproject.toml      # Project configuration
├── expenses.db         # SQLite database (created on first run)
└── README.md           # This file
```

## Troubleshooting

**Server not connecting:**
- Verify the absolute path in your MCP configuration
- Ensure Python 3.11+ is installed and accessible
- Check that FastMCP is installed: `pip list | grep fastmcp`

**Database errors:**
- The database is automatically created in the project directory
- Ensure write permissions for the directory

**Category not found:**
- Check `categories.json` for valid categories
- The LLM can read this file to suggest valid options

## Future Enhancements

Potential features to add:
- Recurring expense tracking
- Budget limits and alerts
- Multi-currency support
- Export to CSV/Excel
- Visualization generation
- Receipt image storage
- Split expenses
- Income tracking

## License

[Add your license here]

## Contributing

Contributions are welcome! Please feel free to submit issues or pull requests.

## Acknowledgments

Built with [FastMCP](https://github.com/jlowin/fastmcp) - A Python framework for building Model Context Protocol servers.
