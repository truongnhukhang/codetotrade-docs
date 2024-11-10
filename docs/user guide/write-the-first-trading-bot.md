# Write the first trading bot
In this guide, i will show you how to easily to create your own trading bot.

## Step 1 : Find the trading idea

Here’s a simple algorithmic trading strategy for BTC/USDT using a Moving Average Convergence Divergence (MACD) strategy with a Relative Strength Index (RSI) filter. The idea is to use the MACD indicator to identify buy/sell signals and use the RSI as a confirmation filter to reduce false signals.

![MACD](../assets/images/example/macd-crossover.jpg)

```
# Buy if MACD crosses above the signal line and RSI is below 70
if macd_line > signal_line and previous_macd_line <= previous_signal_line:
    if rsi < 70:
        return True
return False
```

```
# Sell if MACD crosses below the signal line and RSI is above 30
if macd_line < signal_line and previous_macd_line >= previous_signal_line:
    if rsi > 30:
        return True
return False
```

## Step 2 : Implement the trading idea

### Step 2.1 : Create a new bot

Firstly, You need to chose your language to start with. The extend the BaseBot class 
=== "Java"
    ```Java
    public class MyMacdBot extends BaseBot {
    }
    ```

=== "Python"
    ```Python
    class MyMacdBot(BaseBot):
    ```

### Step 2.2 : Create indicator in your bot
In this step, you need to create Macd indicator and Rsi indicator.

=== "Java"
    ```Java
    ```

=== "Python"
    ```Python
    ```

