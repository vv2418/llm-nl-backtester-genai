# Backtest Chat Copilot

A Streamlit-based web application that uses LLMs to translate natural language trading strategy descriptions into executable backtests. Built with an agentic pipeline architecture using LangGraph for robust state management, error recovery, and human-in-the-loop confirmation.

## Features

- **Natural Language to Strategy**: Describe trading strategies in plain English
- **Human-in-the-Loop**: Review and confirm strategy interpretation before backtesting
- **Automatic Backtesting**: Execute strategies with comprehensive performance metrics
- **LLM Explanations**: Get human-readable explanations of strategy performance
- **Trade-by-Trade Analysis**: Detailed breakdown of every trade executed
- **Multiple LLM Models**: Choose from gpt-4o-mini, gpt-4o, gpt-4-turbo, or gpt-3.5-turbo
- **Error Recovery**: Automatic retries with exponential backoff for transient failures
- **State Persistence**: Resume from checkpoints, track execution history

## Quick Start

### Prerequisites

- Python 3.10+
- OpenAI API key

### Installation

1. **Clone the repository** (or navigate to project directory)

2. **Create a virtual environment**:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   pip install langgraph langchain langchain-openai
   ```

4. **Set up environment variables**:
   Create a `.env` file in the project root:
   ```bash
   OPENAI_API_KEY=your_openai_api_key_here
   ```

   ⚠️ **IMPORTANT**: Replace `your_openai_api_key_here` with your actual OpenAI API key. The application requires this to function.

5. **Run the application**:
   ```bash
   streamlit run app.py
   ```

6. **Open your browser** to `http://localhost:8501`

## Usage

1. **Enter Strategy**: Describe your trading strategy in natural language
   ```
   Example: "Backtest AAPL from 2018-01-01 to 2024-01-01.
   Go long when the 10-day moving average crosses above the 50-day moving average.
   Exit when the 10-day moving average crosses back below the 50-day.
   Only enter new positions when 20-day realized volatility is below its 1-year median."
   ```

2. **Select Model**: Choose your preferred OpenAI model (gpt-4o-mini is fastest/cheapest)

3. **Review Interpretation**: The system will show how it interpreted your strategy

4. **Confirm & Proceed**: Review the interpretation and click "Confirm & Proceed"

5. **View Results**: See performance metrics, equity curve, trade details, and LLM explanation

## Supported Strategy Types

- **Single-asset strategies** (one ticker at a time)
- **Long-only** (no shorting, no leverage)
- **Moving Average Crossovers**: Fast MA vs Slow MA
- **Volatility Filters**: Realized volatility vs 1-year median
- **Temporal Constraints**: Lookahead windows, duration requirements, sequential entry logic

## Project Structure

```
Project/
├── app.py                 # Streamlit web application
├── pipeline/              # Agentic pipeline (LangGraph)
│   ├── graph.py          # Pipeline workflow definition
│   ├── nodes.py          # Individual pipeline nodes
│   ├── state.py          # State schema
│   ├── checkpoints.py   # Checkpoint management
│   └── errors.py         # Error handling & retries
├── llm/                  # LLM integration
│   ├── translator.py     # Natural language → StrategySpec
│   ├── interpreter.py    # Strategy interpretation explanation
│   └── explainer.py      # Results explanation
├── core/                 # Backtesting engine
│   ├── strategy_spec.py  # Strategy data structures
│   ├── data.py          # Data fetching & features
│   ├── backtester.py    # Backtest execution
│   ├── metrics.py       # Performance metrics
│   ├── validator.py     # Strategy validation
│   └── plotting.py     # Visualizations
└── utils/               # Utilities
    └── metrics_tracker.py  # LLM metrics logging
```

## Architecture

The application uses an **agentic pipeline architecture** built with LangGraph:

- **State Management**: Persistent state across pipeline stages
- **Human-in-the-Loop**: Checkpoint mechanism for user confirmation
- **Error Recovery**: Automatic retries with exponential backoff
- **Conditional Routing**: Smart flow based on validation results
- **Observability**: Comprehensive logging and metrics tracking

See [ARCHITECTURE_DIAGRAM.md](ARCHITECTURE_DIAGRAM.md) for detailed architecture documentation.

## Metrics Tracking

The system automatically tracks:
- Token usage (input/output/total) for each LLM call
- API costs based on model pricing
- Latency for each operation
- Success/failure rates

Metrics are logged to `metrics_log.csv`. Run `python analysis/analyze_metrics.py` to generate summary statistics.

## Configuration

- **API Keys**: Set `OPENAI_API_KEY` in `.env` file
- **Model Selection**: Choose from dropdown in UI
- **Logging**: Configured in `app.py` and pipeline modules

## Limitations

- Single-asset strategies only
- Long-only (no shorting or leverage)
- Daily timeframe (close-to-close)
- Limited to moving average crossovers and volatility filters

## Troubleshooting

- **"OPENAI_API_KEY is not set"**: Create `.env` file with your API key
- **Import errors**: Ensure all dependencies are installed (`pip install -r requirements.txt`)
- **Pipeline errors**: Check terminal logs for detailed error messages
- **Data fetch failures**: Verify ticker symbol and date range are valid
  
## Setup tutorial - https://drive.google.com/file/d/1ko90REr-hpD2-H6zjs2LfWaB-lHo1b0t/view?usp=sharing

## License

This project is for educational purposes.

## Acknowledgments

Built as part of the LLM Gen AI course at Columbia University.
