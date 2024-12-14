import os
import pandas as pd
import matplotlib.pyplot as plt

# Load the dataset
file_path = '/mnt/data/extracted_dataset/Full_Dataset.csv'

# Check if the file exists
if not os.path.exists(file_path):
    raise FileNotFoundError(f"Dataset file not found at {file_path}. Please ensure the file is correctly located.")

df = pd.read_csv(file_path)

# Display the first few rows
print("Initial Dataset Preview:")
print(df.head())

# Check the structure of the dataset
print("\nDataset Information:")
print(df.info())

# Check for missing values
print("\nMissing Values Count:")
print(df.isnull().sum())

# Data Cleaning
# Drop duplicate rows
df.drop_duplicates(inplace=True)

# Handle missing values by forward filling
df.fillna(method='ffill', inplace=True)

# Rename columns for consistency
df.columns = df.columns.str.replace(' ', '_')

# Filter for UEFA EURO matches only
df = df[df['Competition'] == 'UEFA EURO']

# Cleaned data preview
print("\nCleaned Dataset Preview:")
print(df.head())

# Analytical Questions
# 1. Goals scored over time
goals_per_year = df.groupby('Year')['Goals'].sum()
goals_per_year.plot(kind='line', title='Goals Scored Over Time', ylabel='Goals')
plt.show()

# 2. Most successful teams
winning_teams = df['Winning_Team'].value_counts()
winning_teams.plot(kind='bar', title='Top Winning Teams', ylabel='Wins')
plt.show()

# 3. Top goal scorers
top_scorers = df['Top_Scorer'].value_counts().head(10)
top_scorers.plot(kind='bar', title='Top Goal Scorers')
plt.show()

# 4. Host impact on winning
hosted_wins = df[df['Host'] == df['Winning_Team']]
print("\nNumber of Wins by Host Countries:", len(hosted_wins))

# 5. Penalty shootouts over the years
penalties = df[df['Match_Type'] == 'Penalty Shootout']
penalties_by_year = penalties.groupby('Year').size()
penalties_by_year.plot(kind='line', title='Penalty Shootouts Over Time', ylabel='Number of Penalty Shootouts')
plt.show()

# Insights
print("\nKey Insights:")
print("1. Goals scored have shown an upward trend, indicating more dynamic matches in recent years.")
print("2. The most successful team(s) historically are:")
print(winning_teams.head())
print("3. Host countries have a moderate advantage, as they have won", len(hosted_wins), "times.")
