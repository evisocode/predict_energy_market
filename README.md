# Intro and Disclaimer

This page describes an exercise to Optimize Results in Italian Energy Market

The source data is public and made available via GME
https://www.mercatoelettrico.org/

This Page, including all Algorithms and Descriptions are Confidential Information, property of Eviso S.R.L. 

Responsible/Contact: data_admin@eviso.it

# Rules and Context
**SIMPLIFIED DESCRIPTION**
Authorized ENTITIES can submit BIDs to purchase or sell energy in the SPOT MARKET for each of the day 24 hours.
The fit between all purchase and the selling bids for an hour of a day defines the price for that day.
The SPOT MARKET is divided into 8 SUB-MARKETS that close and open at different times, creating temporal windows for entities to bid multiple times for the same specific hour of a specific day.
The Italian market is divided into multiple REGIONS, each with its own 8 SUB-MARKETS.

The SPOT MARKET represents the reservation of energy. No real energy is exchanged here.
The difference between the real energy produced or consumed by each entity and the energy reserved in SPOT MARKET will be handled by the DISPATCH MARKET that will charge entities for that.
Both energy producers and consumers make bids in the same SPOT MARKETS.
A consumer submits mainly purchase bids but can also submit selling bids if their forecast changed and they bought too much energy.

The goals of each entity in the SPOT market are:**
1- forecast its energy consumption or production in order to reserve the correct amount in the SPOT MARKET and minimize cost of the difference in the DISPATCH MARKET.
2- reserve the above energy at the best possible price by bidding in any of the 8 SUB-MARKETS in the SPOT MARKET or even transacting previously reserved energy in subsequent SUB-MARKETS.


**EXAMPLES OF POSSIBLE ACTIONS**
1.	On 12th May, a consumer forecasts that will consume 12MWh of energy for the next day at hour number 17 (17:00 to 18:00) in the NORD region.
2.	He bids for those 12MWh in the MGP sub-market that closes on at 12:00 of 12th May with two separate bids: one with 8MWh at price 55€/MWH and a second with 4MWh at 25€/MWh.
3.	At 12:55 the consumers receive the result that only the fist bid was accepted because it was higher than the price defined by the offer and demand crossing of that session: 50€/MWH. The second bid was rejected because the price was lower than 50€/MWh.
4.	The consumer than submits a new bid of 4MWH at 35€/MWH for the MI1 sub-market that closes at 15:00 of the 12th May.
5.	At 15:30 the consumer receive the result that the bid was accepted since the price of the session was settled at 30€/MWh, which was lower than the price of the bid.
6.	At 08:00 of the 13th May the consumer forecasts a different value of needed energy for the hour number 17 (17:00 to 18:00) for that same day. Now instead of 12MWh, the forecast is 10MWH.
7.	At that time the MI6 sub-market is open until 11:15 and the consumer submits the bid to sell 2MWH at a price of 15€/MWH.
8.	At 11:45 the consumer receive the result that the bid was accepted because the session price was settled at 25€/MWh, higher than the submitted selling bid price.
9.	After the 13th day ends, the consumer receives the information that the real consumption the hour number 17 (17:00 to 18:00) of the 13th of May was 11MWh and that the extra 1MWh resulted in a charge in the DISPATCH MARKET at 60 €/MWh.
The total energy cost for that our can be computed by:
+ 8 MWh x 50 €/MWh = 440 €
+ 4 MWh x 30 €/MWh = 220 €
- 2 MWh x 30 €/MWh

**RULES FOR EACH BID**
- submitted for a specific HOUR of a specific DAY
- include energy QUANTITY (MWh) 
- include min and max PRICE (€) willing to sell/purchase.
- single PURPOSE: either purchase or sell
- target one single MARKET and one single ZONE
- target an HOUR inside the interval of the used MARKET
- be submitted inside the Start and End time of the used MARKET
- ENTITIES can submit multiple BIDs for the same HOUR, MARKET, ZONE and PURPOSE.

**ACCEPTANCE OR REFUSAL OF BIDS**
- For each HOUR, MARKET and ZONE, the SYSTEM will intersect the purchase and sell cumulative curves of all ENTITIES.
- The PRICE of the BIDS at the intersection defined the AWARDED PRICE
- All purchase BIDS with BID PRICE above the AWARDED PRICE are accepted, others are refused.
- All sell BIDS with BID PRICE bellow the AWARDED PRICE are accepted, others are refused.

**DEFINITIONS OF SUB-MARKETS TARGETED IN THIS EXERCISE**
| Market|      Description  |    Start   |    End     |  Results   |Interval|
|:-----:|:-----------------:|------------|------------|------------|--------|
| MPG   |  Day-Ahead        |08:00 of D-9|12:00 of D-1|12:55 of D-1|01 to 24|
| MI1   |     Intra-day     |12:55 of D-1|15:00 of D-1|15:30 of D-1|01 to 24|
| MI2   |     Intra-day     |12:55 of D-1|16:30 of D-1|17:00 of D-1|01 to 24|
| MI3   |     Intra-day     |17:30 of D-1|23:45 of D-1|00:15 of D  |05 to 24|
| MI4   |     Intra-day     |17:30 of D-1|03:45 of D  |04:15 of D  |09 to 24|
| MI5   |     Intra-day     |05:30 of D  |07:45 of D  |08:15 of D  |13 to 24|
| MI6   |     Intra-day     |05:30 of D  |11:15 of D  |11:45 of D  |17 to 24|
| MI7   |     Intra-day     |05:30 of D  |15:45 of D  |16:15 of D  |21 to 24|

Interval = hours of D in which that specific Market can BID

More details about SUB-MARKETS
- https://www.mercatoelettrico.org/En/Mercati/MercatoElettrico/MPE.aspx

**DEFINITIONS OF ZONES TARGETED IN THIS EXERCISE**

The energy consumed or produced in each ZONE must be traded in the SPOT market of that specific ZONE. This means every zone has its own 8 SUB-MARKET sessions.
For PRICE purposes, the zones can share the BIDs and create MEGA ZONES or even a single national ZONE. This behavior is very complex and not in the scope of this study.
Zone Map
<img src="https://www.researchgate.net/profile/Iea_Pvps2/publication/324727266/figure/fig17/AS:631598269620258@1527596170404/5-2-Market-zones-of-the-Italian-power-system.png" width="400" height="400">

| **Name** | **Acronym** | **Type of zone** | **Areas included** |
| --- | --- | --- | --- |
| Central-Northern Italy | CNOR | geographical | Toscana, Umbria, Marche |
| Central-Southern Italy | CSUD | geographical | Lazio, Abruzzo, Campania |
| Northern Italy | NORD | geographical | Val D&#39;Aosta, Piemonte, Liguria, Lombardia, Trentino, Veneto, Friuli Venezia Giulia, Emilia Romagna |
| Sardegna | SARD | geographical | Sardegna  |
| Sicilia | SICI | geographical |  Sicilia |
| Southern Italy | SUD | geographical | Molise, Puglia, Basilicata, Calabria |
| Rossano | ROSN | pole of limited production |   |
| Austria | AUST | foreign virtual |   |
| Corsica | CORS | foreign virtual |   |
| Corsica AC | COAC | foreign virtual |   |
| France | FRAN | foreign virtual |   |
| Greece | GREC | foreign virtual |   |
| Slovenia | SLOV | foreign virtual |   |
| Switzerland | SVIZ | foreign virtual |   |
| Malta | MALT | foreign virtual |   |
| Montenegro | MONT | foreign virtual |   |
| France Coupling | XFRA | foreign virtual | interconnection France |
| Austria Coupling | XAUS | foreign virtual | interconnection Austria |
| Slovenia Coupling | BSP | foreign virtual | interconnection Slovenia |
| Switzerland Coupling | XSVI | foreign virtual | interconnection Switzerland |


More details about ZONES
- https://www.mercatoelettrico.org/En/Mercati/MercatoElettrico/Zone.aspx

**DATA EXAMPLES*

Example of AWARDED PRICES per MARKET for ZONE = 'NORD' at 19 March 2020

| Hour | MGP   | MI1   | MI2   | MI3   | MI4    | MI5   | MI6   | MI7   |
|:----:|-------|-------|-------|-------|--------|-------|-------|-------|
| 01   | 26,67 | 26,67 | 27,06 |       |        |       |       |       |
| 02   | 24,00 | 22,56 | 24,90 |       |        |       |       |       |
| 03   | 23,00 | 20,70 | 21,29 |       |        |       |       |       |
| 04   | 22,16 | 19,95 | 22,06 |       |        |       |       |       |
| 05   | 22,26 | 20,04 | 21,04 | 25,85 |        |       |       |       |
| 06   | 26,00 | 27,90 | 26,08 | 27,90 |        |       |       |       |
| 07   | 30,25 | 30,25 | 30,41 | 31,70 |        |       |       |       |
| 08   | 33,98 | 30,98 | 32,56 | 31,00 |        |       |       |       |
| 09   | 32,06 | 32,15 | 32,17 | 29,55 | 32,16  |       |       |       |
| 10   | 28,75 | 28,18 | 26,88 | 25,75 | 28,85  |       |       |       |
| 11   | 25,04 | 25,00 | 25,00 | 22,55 | 26,04  |       |       |       |
| 12   | 24,93 | 24,63 | 24,90 | 22,43 | 25,03  |       |       |       |
| 13   | 22,83 | 20,83 | 20,83 | 22,73 | 22,82  | 22,73 |       |       |
| 14   | 22,99 | 22,99 | 22,99 | 22,89 | 23,37  | 22,89 |       |       |
| 15   | 24,92 | 25,00 | 25,00 | 26,00 | 27,90  | 24,82 |       |       |
| 16   | 27,90 | 28,14 | 28,14 | 30,10 | 29,90  | 27,80 |       |       |
| 17   | 32,00 | 32,15 | 32,16 | 31,90 | 32,01  | 31,90 | 25,00 |       |
| 18   | 35,04 | 44,59 | 40,04 | 37,90 | 34,94  | 35,73 | 30,00 |       |
| 19   | 37,82 | 60,00 | 54,00 | 75,49 | 60,82  | 51,90 | 63,44 |       |
| 20   | 51,62 | 75,61 | 68,06 | 77,22 | 97,22  | 64,93 | 69,01 |       |
| 21   | 36,88 | 75,00 | 61,23 | 75,49 | 105,49 | 59,00 | 57,11 | 66,33 |
| 22   | 34,50 | 57,68 | 41,50 | 44,08 | 32,00  | 34,01 | 38,07 | 59,00 |
| 23   | 34,50 | 31,00 | 29,50 | 28,20 | 29,28  | 28,20 | 25,26 | 28,20 |
| 24   | 27,10 | 24,76 | 27,10 | 27,10 | 27,90  | 26,16 | 25,00 | 27,20 |


# Goals of the exercise

**GOAL 1: MAX & MIN PRICES**
- For each HOUR, DAY and ZONE, predict the MARKET with lower PRICE 
- For each HOUR, DAY and ZONE, predict the MARKET with higher PRICE 

**GOAL 2: BEST STRATEGY WITH 1X PURCHASE & 1X SELL**

RULES:
- For each HOUR, DAY and ZONE, purchase 1MWh in just one MARKET and sell 1MWh at another MARKET.
- Can be use any MARKETS in any order of PURSCHARE / SELL
- VALUE = PRICE SELL - PRICE PURCHASE
- VALUE is defined for each HOUR, DAY and ZONE
- GOD STRATEGY is the perfect strategy.
- GOD_RATIO = VALUE of the STRATEGY / VALUE of the GOD STRATEGY 
- To rank your Strategies use VALUE and GOD_RATIO

GOAL:
- Create a strategy that maximizes VALUE over an long period of time (several days)

# Raw Data Description

* Raw Data is present in TABLE 'raw_market_data'
* Each line corresponds to a single HOUR of a single DAY of a single MARKET.
* The TABLE *keys* are 'data_hora' and 'Mercato'
* The TABLE is hosted in a MYSQL engine and accessible via direct Query (see next section)
* Below are listed all the columns present in 'raw_market_data'

| Column             | Datatype     | Example          | Description        |
|--------------------|--------------|------------------|----------------|
| data_hora          | INT(13)      | 2017030101       | Date and hour of BID SESSION = 8 Date Digits + 2 Hour Digits |
| inserted           | DATETIME     | 2020/03/26 08:26 | Date value was inserted in this table                        |
| Data               | DATE         | 2017/03/01       | Date of BID SESSION                                          |
| Mercato            | VARCHAR(3)   | MGP              | MARKET                                                       |
| Ora                | INT(2)       | 01               | HOUR                                                         |
| TOTALE_ACQUISTI    | FLOAT(20,6)  | 28047,36         | QUANTITY (MWh) of all accepted purchase BIDs                 |
| NAT_ACQUISTI       | FLOAT(20,6)  | 28047,36         | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| CNOR_ACQUISTI      | FLOAT(20,6)  | 3089,955         | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| CSUD_ACQUISTI      | FLOAT(20,6)  | 4198,906         | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| NORD_ACQUISTI      | FLOAT(20,6)  | 15717,26         | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| SARD_ACQUISTI      | FLOAT(20,6)  | 836,08           | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| SICI_ACQUISTI      | FLOAT(20,6)  | 1597,486         | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| SUD_ACQUISTI       | FLOAT(20,6)  | 2096,675         | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| AUST_ACQUISTI      | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| BRNN_ACQUISTI      | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| COAC_ACQUISTI      | FLOAT(20,6)  | 70               | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| CORS_ACQUISTI      | FLOAT(20,6)  | 49               | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| FOGN_ACQUISTI      | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| FRAN_ACQUISTI      | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| GREC_ACQUISTI      | FLOAT(20,6)  | 250              | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| MFTV_ACQUISTI      | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| PRGP_ACQUISTI      | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| ROSN_ACQUISTI      | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| SLOV_ACQUISTI      | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| SVIZ_ACQUISTI      | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| BSP_ACQUISTI       | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| MALT_ACQUISTI      | FLOAT(20,6)  | 142              | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| XAUS_ACQUISTI      | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| XFRA_ACQUISTI      | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted purchase BIDs       |
| TOTALE_VENDITE     | FLOAT(20,6)  | 23941,36         | QUANTITY (MWh) of all accepted sell BIDs                     |
| NAT_VENDITE        | FLOAT(20,6)  | 28047,36         | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| CNOR_VENDITE       | FLOAT(20,6)  | 1876,865         | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| CSUD_VENDITE       | FLOAT(20,6)  | 3153,855         | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| NORD_VENDITE       | FLOAT(20,6)  | 9122,995         | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| SARD_VENDITE       | FLOAT(20,6)  | 1243,176         | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| SICI_VENDITE       | FLOAT(20,6)  | 932,997          | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| SUD_VENDITE        | FLOAT(20,6)  | 3063,155         | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| AUST_VENDITE       | FLOAT(20,6)  | 25               | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| BRNN_VENDITE       | FLOAT(20,6)  | 546,821          | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| COAC_VENDITE       | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| CORS_VENDITE       | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| FOGN_VENDITE       | FLOAT(20,6)  | 284,601          | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| FRAN_VENDITE       | FLOAT(20,6)  | 45               | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| GREC_VENDITE       | FLOAT(20,6)  | 231              | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| MFTV_VENDITE       | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| PRGP_VENDITE       | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| ROSN_VENDITE       | FLOAT(20,6)  | 1093,895         | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| SLOV_VENDITE       | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| SVIZ_VENDITE       | FLOAT(20,6)  | 2322             | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| BSP_VENDITE        | FLOAT(20,6)  | 663              | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| MALT_VENDITE       | FLOAT(20,6)  | 0                | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| XAUS_VENDITE       | FLOAT(20,6)  | 285              | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| XFRA_VENDITE       | FLOAT(20,6)  | 3158             | QUANTITY (MWh) of specific ZONE accepted sell BIDs           |
| TOTITABSP_VENDITE  | FLOAT(20,6)  | 24604,36         |                                                              |
| TOTITABSP_ACQUISTI | FLOAT(20,6)  | 28047,36         |                                                              |
| PUN                | DOUBLE(20,6) | 43,36            | PRICE                                                        |
| NAT                | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| CNOR               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| CSUD               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| NORD               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| SARD               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| SICI               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| SUD                | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| BRNN               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| AUST               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| COAC               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| CORS               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| FOGN               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| FRAN               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| GREC               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| MFTV               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| PRGP               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| ROSN               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| SLOV               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| SVIZ               | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |
| BSP                | FLOAT(20,6)  | 43,36            | AWARDED PRICE (euro) of specific ZONE                        |

# Access to Server Data
* Data is hosted in a MySQL Server
* The data can be accessed via direct query.
* See below and example using PyMySQL

```
!apt-get install mysql-server > /dev/null
!service mysql start
!pip -q install PyMySQL
%load_ext sql
%config SqlMagic.feedback=False 
%config SqlMagic.autopandas=True
%sql mysql+pymysql://colab_energy_market:PASSWORD@HOST:PORT/energy_market
# query using %sql or %%sql
df = %sql SELECT * FROM raw_market_data where Mercato = 'MI1' and data > '2020-03-30 00:00:00'
df
```
