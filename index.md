
The goal of this github organtization is to:
1. Create tools to analyze historical financial data in a way that is logical and produces referencable data  
2. Encourage collaboration and spread ideas
3. Use historically inferred statistical data to make good future financial decisions 

# Programs

## [Earnings Call Toolkit (BETA)](https://github.com/stoxrisk/EarningsCallToolkit)

### Objective 
To provide tools that make it easy to add more code to analyze intraday and daily data around earnings calls

### Description
Currently there are two strategies that have run through this program. 

One of them, which is further described here in this [Reddit Post](https://www.reddit.com/r/algotrading/comments/d57hbq/2019_earnings_call_percentage_change_analysis/)
This strategy described in this reddit pulls intraday data from sp500 companies during 2019 surrounding earnings calls (due to lack of data I could only manage to get pre/post market data for 2019). It analyzes percentage differences in price among given timeframes for after market earnings calls, the set ones are intraday of earnings call, post market, pre market next day, and intraday next day. The line graph visualization of the results is [HERE](https://raw.githubusercontent.com/stoxrisk/EarningsCallToolkit/master/line_graph_output.png)
and the most useful section of the data, the bucketed statistical data is [HERE](https://docs.google.com/spreadsheets/d/15W2TGEMyI30cGp9kIFp1qzh10FJr2HnNhz1tK0wJ_Ak/edit?usp=sharing) (which is also included as raw data in the repo itself)

The second strategy I use to predict future eanings calls outcomes and evaluate the value of selling or buying options. It works by using daily data from Yahoo Finance (instead of intraday data from TD Ameritrade) and comparing the day before the earnings call and the day after or during the earnings call, this strategy works on both pre and post earnings calls unlike the previous strategy. The end result of the program is a CSV "profile" for each symbol that describes the rises or falls over time through earnings calls and the average AVERAGE of absolute, negative, and positive changes.

With it there is also a helper program [HERE](https://github.com/stoxrisk/EarningsCallToolkit/blob/master/estimate_strikes.py) that will estimate good strike prices for selling options based on historical data, when using it to make a move on Costco's upcoming earnings call the output looks like this:

```markdown
-----------------------------------------
Earnings Date 20121212: -0.600139%
Earnings Date 20130312: 1.278795%
Earnings Date 20130530: -0.947322%
Earnings Date 20131009: 2.121021%
Earnings Date 20131211: -1.224593%
Earnings Date 20140529: -0.087534%
Earnings Date 20141008: 2.762033%
Earnings Date 20141210: -1.950499%
Earnings Date 20150305: 2.717945%
Earnings Date 20150527: -0.797692%
Earnings Date 20151208: -5.418366%
Earnings Date 20160302: -0.778838%
Earnings Date 20160929: 3.403613%
Earnings Date 20161207: 2.430933%
Earnings Date 20170302: -4.337567%
Earnings Date 20171005: -5.973550%
Earnings Date 20171214: 3.323860%
Earnings Date 20180307: -0.891331%
Earnings Date 20180531: -0.559928%
Earnings Date 20181004: -5.550754%
Earnings Date 20181213: -8.586816%
Earnings Date 20190307: 5.087880%
Earnings Date 20190530: -0.811456%
-----------------------------------------
Chance of Rising: 34.782609%
Chance of Falling: 65.217391%
-----------------------------------------
Absolute Difference Average: 2.680107%
Positive Difference Average: 2.890760%
Negative Difference Average: -2.567759%
-----------------------------------------
Current Price: 288.510010
Recommended Lower Strike: 281.000000
Recommended Higher Strike: 297.000000
-----------------------------------------
```
Being bullish on this I'd probably sell a credit spread with strikes around -5% and buy a debit spread with strikes around current price given this current data.

You can use the [editor on GitHub](https://github.com/stoxrisk/stoxrisk.github.io/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Usage
* Be sure to first install all that is required in requirements.txt *

To run the first strategy:
python ./analyze.py 1

To run the second strategy for all SP500 companies
python ./analyze.py 2

To run the second strategy for a specific symbol
python ./analyze.py 2 TSLA

To estimate Strikes: 
python ./estimate_strikes.py COST

## [Percentage Difference Analysis (ALPHA)](https://github.com/stoxrisk/StockPriceDiffAnalysis)

### Objective 
To get an overview of the percentage of change over a given set of days within a particular timeframe.

### Description
This program is one of the first in conception and is used to sell options with the highest probability of success for any given equity symbol.
For example imagine if you wanted to sell some options and make some money with really high probibility on SPY with a 30 day expiration. What should your strike prices be? 
We can run the progm with the following command: 

python ./visualize_change.py SPY 30 2019-01-01

Which will analyze data from the beginning of 2019 until now, and compare every 30 day difference period and generate the following output:
```markdown
Standard Deviation: 3.5216235067092305
Mean: 2.073248407643312
Median: 2.9
***********************
-5.0 - -1.4 : 95.4%
-1.4 - 2.1 : 68.2%
2.1 - 5.6 : 68.2%
5.6 - 9.1 : 95.4%
***********************
<-5.0 : 1 : 0.64%
<-4.5 : 4 : 2.55%
<-4.0 : 3 : 1.91%
<-3.5 : 4 : 2.55%
<-3.0 : 1 : 0.64%
<-2.5 : 6 : 3.82%
<-2.0 : 9 : 5.73%
<-1.5 : 8 : 5.10%
<-1.0 : 8 : 5.10%
<-0.5 : 4 : 2.55%
<0.0 : 2 : 1.27%
<0.5 : 1 : 0.64%
<1.0 : 2 : 1.27%
<1.5 : 6 : 3.82%
<2.0 : 8 : 5.10%
<2.5 : 9 : 5.73%
<3.0 : 17 : 10.83%
<3.5 : 9 : 5.73%
<4.0 : 20 : 12.74%
<4.5 : 7 : 4.46%
```

The visual histogram of data can be viewed [HERE](https://raw.githubusercontent.com/stoxrisk/StockPriceDiffAnalysis/master/outputs/spy30-2019.png)

From this we can gather the following:
- Setting a bearish strike price between -1.4% - 2.1% of the current price will provide profit with 68.2% certainty
- For almost guarenteed odds of 95.4% we can put a strike price at -5% or +9.1%

I think selling some bearish options would be a smart move here, pending a fragile and odd world economy

### Usage:

python ./visualize_change.py [SYMBOL] [DAYS To Measure] [Date to have data begin at]

## [Configurable Backtester (Early In-Progress)](https://github.com/stoxrisk/Backtesting-Engine)

### Objective
The objective of this program is to provide a super easy interface for backtesting using configuration files. The end goal of this program will be to create or modify configuration files that describe a strategy to backtest on, analyze the data, and then autotrade that strategy for you on current data.

An example of the configuration file would be like this:
[Engulfing Strategy Configuration](https://github.com/stoxrisk/Backtesting-Engine/blob/master/configs/engulfing.txt)


# Additional Notes:

- These programs are only configured to work for windows machines, easy configurations can be made to have this compatible with UNIX machines but no such need currently exists
- Programs are always in development and welcome for collaboration or suggestions


Contact:
Please reach out to me at: worldchuba@gmail.com
