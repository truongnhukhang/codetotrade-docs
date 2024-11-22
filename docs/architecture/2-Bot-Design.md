# Bot Design

This part of the documentation describes the execution flow of the bot. How it is connect to exchange , How it is execute the back test and how it is execute the live trading.

## Bot execution flow chart

``` mermaid
graph TD
   A[Start] --> B[Get BarSeries]
   B --> Init["`init()`"]
   Init["`init()`"] --> C["`For *index* in *BarSeries.length*`"]
   C --> IndexLoop
   subgraph IndexLoop
        D{{BotStatus==IN_POSITION}} --> |"True"| E{{"`isHitTP(index) || 
        isHitSL(index) || 
        isClosePosition(index)`"}}
        D --> |"False"| F{{"`isBuy(index) || isSell(index)`"}}
        F --> |"True"| G["`buy(index) || sell(index)`"]
        F --> |"False"| END@{ shape: circle, label: "END" }
        G --> |"tradeMetadata"| H["`executeTrade(tradeMetadata)`"]
        H --> I["`BotStatus=IN_POSITION`"]
        E --> |"False"| END
        E --> |"True"| L["`BotStatus=WAITING_FOR_ORDER`"]
        I --> END
        L --> END
   end
```

## BaseBot Abstract Class

This an abstract class that provide
    - empty body (abstract) methods that need to be filled in (implemented) by the child classes.
    - BarSeries,Coin information all other attribute need for your algorithm to work.

We will go though each method in detail in the following sections

### Attributes

#### BarSeries

This is the main candlestick series that the bot is working on. We will loop the BarSeries and execute the trading algorithm for each candlestick. 

This Candle interval of the BarSeries was configured in the `Main candle interval` input on UI

#### OtherBarSeries

This is a map of BarSeries by candle interval . This object will be created when you put value in the `Support candle intervals` on the UI .

*How to get the BarSeries of 1 hour interval from the map?*

=== "Java"
```java
import java.time.Duration;

BarSeries barSeries1Hour = otherBarSeries.get(Duration.ofMinutes(60));
```
=== "Python"
```python
bar_series_1_hour = other_bar_series['60']
```

#### CoinInfo

This object contain coin information that the bot is trading on. We will use this object to know exactly the price precision of the coin , and minimum quantity that the exchange allow to buy and sell in this coin

### Methods

#### Init(config : Map<String,String> | Dict[str,str>)

This method is called when the bot is initialized, allowing you to configure your indicators with parameters from `Indicator params` input on the UI.

#### isBuy(index : int) -> bool

This method is called for each Bar (CandleStick) to determine if the bot should buy at this Bar . 

*How to get Current Bar by index?*

=== "Java"
    ```java
    @Override
    public boolean isBuy(int i) {
        Bar currentBar = barSeries.getBar(index);
        double closePrice = bar.getClosePrice().doubleValue();
        long openTime = bar.getBeginTime().toInstant().toEpochMilli();
    }
    ```

=== "Python"
    ```python
    def is_buy(self, idx: int) -> bool:
        current_bar = self.bar_series.bars[idx]
        close_price = current_bar.close
        open_time = current_bar.start_time
    ```
*How to get Previous Bar by index?*

=== "Java"
    ```java
    @Override
    public boolean isBuy(int i) {
        Bar prevBar = barSeries.getBar(index-1);
    }
    ```

=== "Python"
    ```python
    def is_buy(self, idx: int) -> bool:
        prev_bar = self.bar_series.bars[idx-1]
    ```
*How to get last n Bar from index? (Sometime you need to know price of last 3 candles)*
 
=== "Java"
    ```java
    @Override
    public boolean isBuy(int i) {
        Bar nLastBar = barSeries.getBar(index-3);
    }
    ```

=== "Python"
    ```python
    def is_buy(self, idx: int) -> bool:
        current_bar = self.bar_series.bars[idx-3]
    ```
*How to get Bar (CandleStick) of 1 hour interval BarSeries by index?*

In order to get the Bar of 1 hour interval BarSeries by main index, you can use the following code:

=== "Java"
    ```java
    @Override
    public boolean isBuy(int i) {
        Bar currentMainBar = barSeries.getBar(index);
        long openTimeOfCurrentMainBar = currentMainBar.getBeginTime().toInstant().toEpochMilli();
        BarSeries barSeries1Hour = otherBarSeries.get(Duration.ofMinutes(60));
        
        int idx1h = getIndexOfBarSeriesByTime(barSeries1Hour, openTimeOfCurrentMainBar);
        Bar current1hBar = barSeries1Hour.getBar(idx1h);
        double closePrice = current1hBar.getClosePrice().doubleValue();
    }
    ```
=== "Python"
    ```python
    def is_buy(self, idx: int) -> bool:
        current_main_bar = self.bar_series.bars[idx]
        open_time_of_current_main_bar = current_main_bar.start_time
        bar_series_1_hour = other_bar_series['60']
        
        idx_1h = get_index_of_bar_series_by_time(bar_series_1_hour, open_time_of_current_main_bar)
        current_1h_bar = bar_series_1_hour.bars[idx_1h]
    ```

#### isSell(index : int) -> bool

Same isBuy but for selling.