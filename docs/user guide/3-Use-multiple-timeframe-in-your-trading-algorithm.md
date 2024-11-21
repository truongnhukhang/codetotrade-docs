# Use multiple timeframe in your trading algorithm

## Overview

The ability to implement trading algorithms using multiple candle intervals is a powerful feature in algorithmic trading. This functionality allows traders to combine data from different timeframes, providing a more comprehensive analysis of market trends and conditions. By leveraging this feature, traders can create sophisticated strategies that are more adaptive to market dynamics, improving decision-making and potential profitability.

## Why Multi-Timeframe Analysis Matters

Markets exhibit behaviors that vary across timeframes. A trend visible in a 1-hour interval might not align with conditions seen in a 15-minute or 5-minute timeframe. Using multiple candle intervals enables traders to:
- **Capture broader market trends** using higher timeframes.
- **Refine entry and exit points** with detailed analysis from lower timeframes.
- **Avoid false signals** by confirming conditions across multiple intervals.
- **Improve strategy robustness** by considering multi-dimensional data.

## Example Strategy

Consider a trading strategy where:

- **Buy Signal**:
    - When the **MACD** on the 1-hour timeframe satisfies `MACD > MACD_Signal`, and
    - The **RSI** on the 5-minute timeframe indicates oversold conditions (`RSI < 30`).

- **Sell Signal**:
    - When the **MACD** on the 1-hour timeframe satisfies `MACD < MACD_Signal`, and
    - The **RSI** on the 5-minute timeframe indicates overbought conditions (`RSI > 70`).

This approach combines a broader market perspective (1-hour MACD) with refined details from shorter timeframes (15-minute and 5-minute RSI) to enhance precision and reduce noise in decision-making.

## How This Feature Works

TBD

## Conclusion

The multi-timeframe support feature empowers traders to build strategies that are both dynamic and reliable. By integrating data from different intervals, it opens the door to more sophisticated analyses and better trading outcomes, making it an essential tool for algorithmic trading. Whether you're a novice or an experienced trader, this feature can elevate your strategy to new levels of performance.
