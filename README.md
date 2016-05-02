# simple line charts

# http://finance.yahoo.com/q/hp?s=FB&a=00&b=1&c=2011&d=00&e=1&f=2016&g=d
# http://finance.yahoo.com/q/hp?s=GOOG&a=00&b=1&c=2011&d=00&e=1&f=2016&g=d
# http://finance.yahoo.com/q/hp?s=YHOO&a=00&b=1&c=2011&d=00&e=1&f=2016&g=d
# http://finance.yahoo.com/q/hp?s=AAPL&a=00&b=1&c=2011&d=00&e=1&f=2016&g=d
# http://finance.yahoo.com/q/hp?s=AMZN&a=00&b=1&c=2011&d=00&e=1&f=2016&g=d
# http://finance.yahoo.com/q/hp?s=MSFT&a=00&b=1&c=2011&d=00&e=1&f=2016&g=d
import requests
TICKERS = ['FB', 'GOOG', 'YHOO', 'AAPL', 'AMZN', 'MSFT']
BASEURL = 'http://real-chart.finance.yahoo.com/table.csv?s={ticker}&a=00&b=1&c=2011&d=00&e=1&f=2016&g=d&ignore=.csv'

for t in TICKERS:
    resp = requests.get(BASEURL.format(ticker=t))
    with open(t + '.csv', 'w') as f:
        f.write(resp.text)
