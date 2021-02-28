# Python Metatrader DataFrame API 2.0

## Instalation for docker Metatrader 5 Server API

###### first make sure you have docker installed on your pc

###### run this command on your terminal or powershell

```
docker volume create ejtraderMT
docker run -d --restart=always -p 5900:5900 -p 15555:15555 -p 15556:15556 -p 15557:15557 -p 15558:15558 --name ejtraderapi_server -v ejtraderMT:/data sostrader/ejtraderapi_server:stable
```

or if you dont want to use docker you can download the expert and install on your Metatrader 5
simple download the folder MQL5 from the link below and install it on the Metatrader
https://github.com/traderpedroso/ejtraderMTServer

## Installation for Python API

```
# for last stable use pip
pip install ejtraderMT

or

#for developers attention may contain countless bugs
git clone https://github.com/traderpedroso/ejtraderMT
cd ejtraderMT
python setup.py install

```

### import

```python
from ejtraderMT import Metatrader


```

### Connect Metatrader 5

make sure thesisAPI expert are load on the chart

```python
'''
to change the host IP example Metatrader("192.168.1.100") ou
you can use doman example  "metatraderserverdomain.com"

for you local Time on the dataFrame Metatrader(localtime=True)
attention broker time is the default for Dataframe index "date"

for real volume for active like WIN futures ou centralized market use Metatrader(real_volume=True)
attention tick volume is the default


to use more than one option just use , example Metatrader(host='hostIP',localtime=True)
'''
api = Metatrader()

```

#### Account information

```python
accountInfo = api.accountInfo()
print(accountInfo)
print(accountInfo['broker'])
print(accountInfo['balance'])
```

### You can create market or pending order with the commands.

#### Market Orders

```python
# symbol, volume, stoploss, takeprofit, deviation
api.buy("EURUSD", 0.01, 1.18, 1.19, 5)
api.sell("EURUSD", 0.01, 1.18, 1.19, 5)
```

#### Limit Orders

```python
# symbol, volume, stoploss, takeprofit, price, deviation
api.buyLimit("EURUSD", 0.01, 1.17, 1.19, 1.18, 5)
api.sellLimit("EURUSD", 0.01, 1.20, 1.17, 1.19, 5)
```

#### Stop Orders

```python
#symbol, volume, stoploss, takeprofit, price, deviation
api.buyStop("EURUSD", 0.01, 1.18, 1.20, 1.19, 5)
api.sellStop("EURUSD", 0.01, 1.19, 1.17, 1.18, 5)
```

#### Positions & Manipulation

```python
positions = api.positions()


if 'positions' in positions:
    for position in positions['positions']:
        api.CloseById(position['id'])


```

#### Orders & Manipulation

```python
orders = api.order()

if 'orders' in orders:
    for order in orders['orders']:
        api.CancelById(order['id'])

```

#### Modify possition

```python
api.positionModify( id, stoploss, takeprofit)

```

#### Modify order

```python
api.orderModify( id, stoploss, takeprofit, price)

```

#### close by symbol

```python
api.CloseBySymbol("EURUSD")

```

#### close particial

```python
# id , volume
api.ClosePartial( id, volume)

```

#### If you want to cancel all Orders

```python
api.cancel_all()
```

#### if you want to close all positions

```python
api.close_all()
```

# History Dataframe Ready

#### History from Date to Date

```python
# you can add unlimited actives to list  ["EURUSD","GBPUSD","AUDUSD"]
symbol = ["EURUSD"]
timeframe = "M1"
fromDate = "20/02/2021"
toDate = "24/02/2021"

history = api.history(symbol,timeframe,fromDate,toDate)
print(history)
                        open     high      low    close  volume  spread
date
2021-02-21 23:00:00  1.21135  1.21138  1.21131  1.21134     7.0      35
2021-02-21 23:01:00  1.21130  1.21135  1.21130  1.21135     6.0      43
2021-02-21 23:04:00  1.21150  1.21184  1.21134  1.21184    13.0      31
2021-02-21 23:05:00  1.21163  1.21207  1.21148  1.21181    39.0      42
2021-02-21 23:06:00  1.21189  1.21193  1.21182  1.21182    17.0      64
...                      ...      ...      ...      ...     ...     ...
2021-02-24 02:56:00  1.21629  1.21629  1.21590  1.21594    51.0       5
2021-02-24 02:57:00  1.21592  1.21592  1.21574  1.21574    34.0       5
2021-02-24 02:58:00  1.21574  1.21579  1.21572  1.21575    35.0       5
2021-02-24 02:59:00  1.21576  1.21588  1.21573  1.21582    55.0       5
2021-02-24 03:00:00  1.21583  1.21601  1.21578  1.21598    80.0       5

[3104 rows x 6 columns]

```

#### History by period unit like 27 candles

```python
# you can add unlimited actives to list  ["EURUSD","GBPUSD","AUDUSD"]
symbol = ["EURUSD"]
timeframe = "M1"
fromDate = 27

history = api.history(symbol,timeframe,fromDate)
print(history)

                        open     high      low    close  volume  spread
date
2021-02-26 19:23:00  1.20846  1.20857  1.20837  1.20856    84.0       5
2021-02-26 19:24:00  1.20855  1.20858  1.20842  1.20847    71.0       5
2021-02-26 19:25:00  1.20846  1.20849  1.20832  1.20845    69.0       5
2021-02-26 19:26:00  1.20844  1.20845  1.20823  1.20833    64.0       5
2021-02-26 19:27:00  1.20833  1.20836  1.20821  1.20834    53.0       5
...                      ...      ...      ...      ...     ...     ...
2021-02-26 22:55:00  1.20721  1.20730  1.20718  1.20719    46.0      13
2021-02-26 22:56:00  1.20718  1.20738  1.20718  1.20731    39.0      12
2021-02-26 22:57:00  1.20730  1.20731  1.20716  1.20717    45.0      18
2021-02-26 22:58:00  1.20716  1.20731  1.20694  1.20704    77.0      16
2021-02-26 22:59:00  1.20702  1.20705  1.20702  1.20704    16.0      37
```

#### History for lastest period gread for predict

```python
# you can add unlimited actives to list  ["EURUSD","GBPUSD","AUDUSD"]
symbol = ["EURUSD"]
timeframe = "M1"
fromDate = 27

history = api.history(symbol,timeframe)
print(history)

                        open     high      low    close  volume  spread
date
2021-02-26 19:23:00  1.20846  1.20857  1.20837  1.20856    84.0       5
2021-02-26 19:24:00  1.20855  1.20858  1.20842  1.20847    71.0       5
2021-02-26 19:25:00  1.20846  1.20849  1.20832  1.20845    69.0       5
2021-02-26 19:26:00  1.20844  1.20845  1.20823  1.20833    64.0       5
2021-02-26 19:27:00  1.20833  1.20836  1.20821  1.20834    53.0       5

```

#### History for multiple symbols merged dataframe

```python
# you can add unlimited actives to list  ["EURUSD","GBPUSD","AUDUSD"] etc
symbol = ["EURUSD","GBPUSD"]
timeframe = "M1"
fromDate = "20/02/2021"
toDate = "24/02/2021"


history = api.history(symbol,timeframe,fromDate,toDate)
print(history)


                        open     high      low    close  volume  spread  gbpusd_open  gbpusd_high  gbpusd_low  gbpusd_close  gbpusd_volume  gbpusd_spread
date
2021-02-21 23:00:00  1.21135  1.21138  1.21131  1.21134     7.0      35      1.40113      1.40113     1.40110       1.40110            2.0            130
2021-02-21 23:04:00  1.21150  1.21184  1.21134  1.21184    13.0      31      1.40119      1.40119     1.40119       1.40119            1.0            102
2021-02-21 23:05:00  1.21163  1.21207  1.21148  1.21181    39.0      42      1.40174      1.40174     1.40167       1.40168           11.0             61
2021-02-21 23:06:00  1.21189  1.21193  1.21182  1.21182    17.0      64      1.40156      1.40170     1.40132       1.40155           10.0             46
2021-02-21 23:07:00  1.21181  1.21182  1.21180  1.21182     4.0      82      1.40156      1.40156     1.40156       1.40156            1.0             63
...                      ...      ...      ...      ...     ...     ...          ...          ...         ...           ...            ...            ...
2021-02-24 02:56:00  1.21629  1.21629  1.21590  1.21594    51.0       5      1.41833      1.41835     1.41786       1.41800           62.0              8
2021-02-24 02:57:00  1.21592  1.21592  1.21574  1.21574    34.0       5      1.41798      1.41801     1.41765       1.41766           54.0              8
2021-02-24 02:58:00  1.21574  1.21579  1.21572  1.21575    35.0       5      1.41767      1.41789     1.41767       1.41768           64.0              8
2021-02-24 02:59:00  1.21576  1.21588  1.21573  1.21582    55.0       5      1.41769      1.41782     1.41764       1.41769           42.0              9
2021-02-24 03:00:00  1.21583  1.21601  1.21578  1.21598    80.0       5      1.41770      1.41797     1.41746       1.41784           95.0              8

[3097 rows x 12 columns]
```

# Live data and streaming events

```python
from ejtraderMT import Metatrader
from threading import Thread




api = Metatrader()

symbols = ["EURUSD","GBPUSD","AUDUSD"]
timeframe = "TICK"


# Live data stream subscribe
api.live(symbols,timeframe)




def price():
    connect = api.live_price
    while True:
        price = connect.recv_json()
        print(price)


def event():
    connect = api.live_event
    while True:
        event = connect.recv_json()
        print(event)




t = Thread(target=price, daemon=True)
t.start()

t = Thread(target=event, daemon=True)
t.start()




while True:
    pass

```

# Project Based and reference thanks for

Ding Li @dingmaotu
https://github.com/dingmaotu/mql-zmq

Nikolai khramkov @khramkov
https://github.com/khramkov/MQL5-JSON-API

### Future add comming soon

```

```
