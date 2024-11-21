# Run the Deep (Overfit) Backtesting

## Overview

Deep backtesting, also known as overfit backtesting, is a technique used to evaluate and optimize trading algorithms by exploring a wide range of parameter combinations. This approach allows traders and developers to gain insights into how robust their strategies are under various conditions, helping them identify potential improvements or weaknesses.

### Why Use Deep Backtesting?

When designing a trading algorithm, fixed parameters may not fully capture the algorithm's potential or limitations. By systematically testing multiple combinations of parameters, deep backtesting helps:

   - Uncover the best-performing parameter sets.

   - Assess the robustness of the trading strategy across different market conditions.

   - Detect overfitting by evaluating consistency in performance.

   - Identify opportunities to refine the algorithm for better outcomes.

## Example Trading Algorithm

For demonstration, consider the following simple trading algorithm:

1. **Buy Signal**:
    - When the Moving Average Convergence Divergence (MACD) indicator satisfies `MACD(fast=12, slow=26) > MACD_Signal(signal=9)`, and
    - The Relative Strength Index (RSI) is below 70 (`RSI(14) < 70`).

2. **Sell Signal**:
    - When the MACD indicator satisfies `MACD(fast=12, slow=26) < MACD_Signal(signal=9)`, and
    - The RSI is above 30 (`RSI(14) > 30`).

3. **Risk Management**:
    - A stop-loss percentage of 2%.
    - A take-profit percentage of 3%.

## Parameterization for Deep Backtesting

In deep backtesting, the algorithm's parameters are varied systematically to create a comprehensive set of test cases. The parameters and their respective possible values for this algorithm are as follows:

- **MACD Fast Period (`fast`)**: `9,12,17,19,25,34`
- **MACD Slow Period (`slow`)**: `34,49,43,55,89`
- **MACD Signal Period (`signal`)**: `9,11,17,23`
- **RSI Period (`rsi`)**: `14,33,21,43`
- **Stop-Loss Percentage (`sl`)**: `2,3,4,5,6,7`
- **Take-Profit Percentage (`tp`)**: `2,3,4,5,6,7`


## How It Works

### Run a deep back testing

**Exchange** : Binance Future

**Coin** : BTCUSDT 

**Main** candle interval : 15m 

**Date from**: 2024-01-01 

**Date to**: 2024-11-21 .

Users provide the parameter lists through  `indicator params` input field. The application automatically generates and tests all combinations.
![overfit-test](../assets/images/deepbacktest.png)

**Important** : You need to get the right value for each parameter in the `init` function.
```java
@Override
public void init(Map<String, String> config) {
   BarSeries series = getBarSeries(); 
   ClosePriceIndicator closePrice = new ClosePriceIndicator(series);
   
   int rsiPeriod = Integer.parseInt(config.get("rsi"));
   int slow = Integer.parseInt(config.get("slow"));
   int fast = Integer.parseInt(config.get("fast"));
   int signalPeriod = Integer.parseInt(config.get("signal"));
   this.tp = Double.parseDouble(config.get("tp"));
   this.sl = Double.parseDouble(config.get("sl"));
   
   this.rsi = new RSIIndicator(closePrice, rsiPeriod);
   this.macd = new MACDIndicator(closePrice, fast, slow);
   this.signal = new EMAIndicator(macd, signalPeriod);
}
```

> The deep backtesting application generates all possible combinations of the given parameter values. For instance:
>
1. **Combination 1**: `fast=9, slow=34, rsi=14, sl=2, tp=3`
2. **Combination 2**: `fast=9, slow=34, rsi=14, sl=2, tp=3`
3. **Combination 3**: `fast=9, slow=34, rsi=14, sl=2, tp=4`
4. ...
5. **Combination N**: `fast=34, slow=89, rsi=43, sl=2, tp=6`
>
The application runs a backtest for each combination and records the results, including metrics such as profit and loss (PNL), win rate, drawdowns, and more.
>

### Interpretation of Results

Running over 17,000 backtest cases gives us a clear picture of how well the algorithm performs in different situations.

If most backtests show positive results, like good profits and manageable losses, the algorithm is likely strong and reliable. But if the results are mostly negative or inconsistent, it may need changes or further testing to improve.

![overfit-test-rs](../assets/images/deepbacktest-rs.png)

### Performance
How long does it take to run 17,000 backtests? On my machine (Core i9 12900K, 32GB RAM) with 6 workers allocated, it took about 10 minutes to complete.

The performance depends on how efficiently the algorithm is designed and the programming language used for implementation. Typically, Java performs better than Python in terms of execution speed.

## Benefits of Deep Backtesting

- **Comprehensive Analysis**: Evaluates the algorithm under a variety of conditions.
- **Optimization**: Identifies parameter combinations that maximize performance.
- **Risk Mitigation**: Helps detect overfitting and ensure real-world applicability.
- **Data-Driven Decisions**: Empowers traders to refine strategies based on evidence.

## Conclusion

Deep backtesting is a powerful tool for traders looking to maximize the potential of their trading algorithms. By systematically exploring various parameter combinations, it ensures a robust and well-optimized strategy, ready to tackle the dynamic nature of financial markets.

## Appendix
### Source code
=== "Java"
[MacdOverfitTestBot](https://github.com/truongnhukhang/codetotrade-java-example/blob/master/src/main/java/coin/algorithm/example/bot/MyMacdOverfitTestBot.java)