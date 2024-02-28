import ccxt
import datetime
import numpy as np
from sklearn.linear_model import LinearRegression
import matplotlib.pyplot as plt

def predict_future_prices(symbol):
    exchange = ccxt.binance({
        'apiKey': 'ТВОЯТ_API_KEY',
        'secret': 'ТВОЯТ_API_SECRET',
    })

 # Извличане на исторически данни
  ohlcv = exchange.fetch_ohlcv(symbol, '1h', limit=1000)
    
   # Преобразуване на данните в подходящ формат
  timestamps = np.array([entry[0] for entry in ohlcv]).reshape(-1, 1)
    closing_prices = np.array([entry[4] for entry in ohlcv])

   # Обучение на модела
  model = LinearRegression()
    model.fit(timestamps, closing_prices)

   # Предсказване на бъдещи цени
   future_timestamps = np.array([timestamps[-1] + i * 3600000 for i in range(1, 11)]).reshape(-1, 1)
    future_prices = model.predict(future_timestamps)

   # Визуализация
   plt.plot(timestamps, closing_prices, label='Historical Prices')
    plt.plot(future_timestamps, future_prices, label='Predicted Prices', linestyle='dashed')
    plt.title(f'{symbol} Historical and Predicted Prices')
    plt.xlabel('Timestamp')
    plt.ylabel('Closing Price (USDT)')
    plt.legend()
    plt.show()

# Въвеждане на символ от потребителя
symbol_input = input('Въведете символ за анализ (например, BTC/USDT): ')
predict_future_prices(symbol_input)
<!---Prekata/Prekata is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
