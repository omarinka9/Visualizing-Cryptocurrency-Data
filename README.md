# Visualizing-Cryptocurrency-Data
Visualizing Cryptocurrency Data

import matplotlib.pyplot as plt
import pandas as pd
import requests

def get_historical_data(start_date, end_date):
    url = f'https://api.coindesk.com/v1/bpi/historical/close.json?start={start_date}&end={end_date}'
    response = requests.get(url)
    if response.status_code == 200:
        data = response.json()
        df = pd.DataFrame.from_dict(data['bpi'], orient='index', columns=['Price'])
        df.index = pd.to_datetime(df.index)
        return df
    else:
        print(f'Ошибка при получении исторических данных: {response.status_code}')
        return None

if __name__ == '__main__':
    start_date = '2020-01-01'
    end_date = '2020-12-31'
    df = get_historical_data(start_date, end_date)
    if df is not None:
        ax = df.plot(title=f'Цена Биткойна с {start_date} по {end_date}', figsize=(15, 8))
        ax.set_xlabel('Дата')
        ax.set_ylabel('Цена ($)')
        plt.show()
