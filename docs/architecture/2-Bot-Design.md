# Bot Design

This part of the documentation describes the execution flow of the bot. How it is connect to exchange , How it is execute the back test and how it is execute the live trading.

## Bot execution flow chart

``` mermaid
graph TD
   A[Start] --> B[Get BarSeries]
   B --> C["`For *index* in *BarSeries.length*`"]
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

