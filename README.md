# final_python_project
## Going to be using one of the project ideas from the pdf form. I created an interactive application that allows a user to visually compare release speed and pitch movement between two pitchers. 

## My research question specifically is "How much does Paul Skenes differ from Jared Jones in pitch speed and movement?"
## Used an interactive app in Python that the visitor can choose with pitchers to compare pitch velocity and along with pitch movement. It is super easy to use. 
## Author: Tucker Ganley

from pybaseball import statcast
import pandas as pd

start_date = "2024-06-01"
end_date = "2024-06-07"

# Collect pitch data from Statcast between the given dates
df = statcast(start_dt=start_date, end_dt=end_date)

# Show the first few rows of the data
df.head()


# Filter the relevant columns
pitch_data = df[['player_name', 'release_speed', 'api_break_z_with_gravity', 'api_break_x_arm', 'pitch_type']]

# Drop rows with missing values for the relevant columns
pitch_data = pitch_data.dropna()

# Show the first few rows of the filtered data
pitch_data.head()

# Comparing Justin Steele and Shota Imanaga
pitcher_names = ['Skenes, Paul', 'Jones, Jared']  # Example names
filtered_pitch_data = pitch_data[pitch_data['player_name'].isin(pitcher_names)]

# Display the filtered data
filtered_pitch_data.head()


# Create a scatter plot for pitch movement
plt.figure(figsize=(10, 6))
sns.scatterplot(x='api_break_x_arm', y='api_break_z_with_gravity', hue='player_name', data=filtered_pitch_data)
plt.title('Pitch Movement Comparison (Horizontal vs Vertical)')
plt.xlabel('Horizontal Movement (inches)')
plt.ylabel('Vertical Movement (inches)')
plt.legend(title='Player Name')
plt.show()


# Plot pitch type distribution for each player
plt.figure(figsize=(8, 6))
sns.countplot(x='pitch_type', hue='player_name', data=filtered_pitch_data)
plt.title('Pitch Type Distribution by Player')
plt.xlabel('Pitch Type')
plt.ylabel('Count')

# Save the plot
plt.savefig('pitch_type_distribution.png')
plt.close()

import ipywidgets as widgets
from ipywidgets import interact

@interact
def compare_pitchers(pitcher1=widgets.Dropdown(options=unique_player_names),
                     pitcher2=widgets.Dropdown(options=unique_player_names)):
    # Filter data for selected pitchers
    selected_data = df[df['player_name'].isin([pitcher1, pitcher2])]
    
    # Plot release speed comparison
    sns.boxplot(x='player_name', y='release_speed', data=selected_data)
    plt.title('Release Speed Comparison')
    plt.xlabel('Player Name')
    plt.ylabel('Release Speed (mph)')
    plt.show()

# Display unique player names for selection
print("Select two pitchers from the following list:\n")
print(pitch_movement_data['player_name'].unique())

# Allow the user to input two player names
pitcher_1 = input("\nEnter the first player's name: ")
pitcher_2 = input("Enter the second player's name: ")

# Filter data for the selected players
filtered_data = pitch_movement_data[pitch_movement_data['player_name'].isin([pitcher_1, pitcher_2])]

# Check if the input players are valid
if pitcher_1 not in pitch_movement_data['player_name'].unique() or pitcher_2 not in pitch_movement_data['player_name'].unique():
    print("One or both player names are invalid. Please try again.")
else:
    print(f"\nSelected players: {pitcher_1} and {pitcher_2}")
    
    # Create the plot
    import matplotlib.pyplot as plt
    import seaborn as sns

    plt.figure(figsize=(10, 6))
    sns.scatterplot(x='api_break_x_arm', y='api_break_z_with_gravity', hue='player_name', data=filtered_data)
    plt.title('Pitch Movement Comparison (Horizontal vs Vertical)')
    plt.xlabel('Horizontal Movement (inches)')
    plt.ylabel('Vertical Movement (inches)')
    plt.legend(title='Player Name')

    # Save the plot
    plt.savefig('interactive_pitch_movement_comparison.png')
    plt.show()
