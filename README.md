# Sea-Level-Predictor

import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import linregress

    # Import data
df = pd.read_csv('epa-sea-level.csv')

def draw_plot():
    # Create scatter plot
    fig, ax = plt.subplots(figsize=(12, 6))
    ax.scatter(df['Year'], df['CSIRO Adjusted Sea Level'])

    # First line of best fit (all data)
    res1 = linregress(df['Year'], df['CSIRO Adjusted Sea Level'])
    x_pred1 = pd.Series(range(1880, 2051))
    y_pred1 = res1.intercept + res1.slope * x_pred1
    ax.plot(x_pred1, y_pred1, 'r', label='Fit: All Data')

    # Second line of best fit (2000 onwards)
    df_recent = df[df['Year'] >= 2000]
    res2 = linregress(df_recent['Year'], df_recent['CSIRO Adjusted Sea Level'])
    x_pred2 = pd.Series(range(2000, 2051))
    y_pred2 = res2.intercept + res2.slope * x_pred2
    ax.plot(x_pred2, y_pred2, 'g', label='Fit: 2000+ Data')

    # Labels and title
    ax.set_title('Rise in Sea Level')
    ax.set_xlabel('Year')
    ax.set_ylabel('Sea Level (inches)')
    ax.legend()

    # Save and return plot
    plt.tight_layout()
    plt.savefig('sea_level_plot.png')
    return fig
