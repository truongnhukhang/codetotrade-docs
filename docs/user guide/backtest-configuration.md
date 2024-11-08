# Back Test configuration

## Exchange configuration

<table>
  <tr>
    <th>Config</th>
    <th>Explanation</th>
  </tr>
  <tr>
    <td>Start date</td>
    <td>The beginning date for your backtest period</td>
  </tr>
  <tr>
    <td>End date</td>
    <td>The ending date for your backtest period</td>
  </tr>
  <tr>
    <td>Exchange</td>
    <td>The exchange to simulate trading on</td>
  </tr>
  <tr>
    <td>Maker fee</td>
    <td>Fee charged when you add liquidity to the order book (limit orders)</td>
  </tr>
  <tr>
    <td>Taker fee</td>
    <td>Fee charged when you remove liquidity from the order book (market orders)</td>
  </tr>
  <tr>
    <td>Coin</td>
    <td>The trading pair/symbol to backtest (e.g. BTC/USDT)</td>
  </tr>
  <tr>
    <td>Main candle interval</td>
    <td>Primary timeframe for strategy execution (e.g. 1m, 5m, 15m, 1h)</td>
  </tr>
  <tr>
    <td>Support candle intervals</td>
    <td>Additional timeframes available for analysis. Example: If your strategy run on 5m time frame but you only want to buy when 1h time frame is bullish, you should add 1h interval here</td>
  </tr>
  <tr>
    <td>BTC.Dom candle intervals</td>
    <td>Bitcoin dominance chart intervals (Binance Futures only)</td>
  </tr>
</table>

## Bot configuration

<table>
  <tr>
    <th>Config</th>
    <th>Explanation</th>
  </tr>
  <tr>
    <td>Bot name</td>
    <td>Name of the bot</td>
  </tr>
  <tr>
    <td>Bot path</td>
    <td>The address of the bot client (e.g. localhost:8888)</td>
  </tr>
  <tr>
    <td>Prefetch bar</td>
    <td>Number of bars to prefetch (e.g. If your algorthim will buy when rsi(14) > 70, you should set this value to be greater than 14)</td>
  </tr>
  <tr>
    <td>Initiation balance</td>
    <td>Initial fiat balance for the bot (e.g. 1000USDT )</td>
  </tr>
  <tr>
    <td>Number worker allocate to this backtest</td>
    <td>How many threads to run the backtest, please keep in mind that the more threads you use, the more memory and CPU it will consume</td>
  </tr>
  <tr>
    <td>Indicator params</td>
    <td>Indicator parameters (e.g. period, source) . The BotClient will receive this params as [key,value] type when it init. example : </br> 

     ```python
     def init(self, config: Dict[str, str]) -> None: 
        self.short_ema_period = int(config.get('s_ema')) 
        self.long_ema_period = int(config.get('l_ema')) 
        self.adx_period = int(config.get('adx')) 
     ```
      the indicator params will be </br> s_ema=30 </br> l_ema=50 </br> adx=14
    </td>
  </tr>
</table>
