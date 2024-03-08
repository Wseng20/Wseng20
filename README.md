
<!---
Wseng20/Wseng20 is a ✨ special ✨ repository because its `README.md` (this file) appears 


import yfinance as yf

def find_stocks_with_high_volume_and_breaking_pre_market_highs():
    # Get current date
    import datetime
    today = datetime.date.today()

    # Define the market opening hours (in UTC)
    market_open_time = datetime.datetime(today.year, today.month, today.day, 13, 30)  # 9:30 AM Eastern Time
    market_close_time = datetime.datetime(today.year, today.month, today.day, 20, 0)  # 4:00 PM Eastern Time

    # Get pre-market data
    pre_market_data = yf.download(tickers="^GSPC", start=market_open_time.strftime('%Y-%m-%d'), end=market_close_time.strftime('%Y-%m-%d'), interval="1m")

    # Filter stocks with over 100 million volume and breaking pre-market highs
    filtered_stocks = []
    for symbol in pre_market_data.columns.levels[1]:
        stock_data = yf.Ticker(symbol)
        stock_info = stock_data.info
        if 'volume' in stock_info and stock_info['volume'] > 100000000:
            pre_market_high = pre_market_data['High'][symbol].max()
            if pre_market_high > stock_info['regularMarketPreviousClose']:
                filtered_stocks.append(symbol)

    return filtered_stocks

# Example usage:
filtered_stocks = find_stocks_with_high_volume_and_breaking_pre_market_highs()
print("Stocks with over 100 million volume and breaking pre-market highs:", filtered_stocks)
