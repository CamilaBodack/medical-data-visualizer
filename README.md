# Medical Data Visualizer

[FCC Data Analysis Challenge - Medical Data Visualizer](https://www.freecodecamp.org/learn/data-analysis-with-python/data-analysis-with-python-projects/medical-data-visualizer)

- Adapted from FCC recomendations :
  - In this project, you will visualize and make calculations from medical examination data using matplotlib, seaborn, and pandas. The dataset values were collected during medical examinations.

    Data description
    The rows in the dataset represent patients and the columns represent information like body measurements, results from various blood tests, and lifestyle choices. You will use the dataset to explore the relationship between cardiac disease, body measurements, blood markers, and lifestyle choices.

    File name: medical_examination.csv

    Create a chart similar to examples/Figure_1.png, where we show the counts of good and bad outcomes for the cholesterol, gluc, alco, active, and smoke variables for patients with cardio=1 and cardio=0 in different panels.

    Use the data to complete the following tasks in medical_data_visualizer.py:

    - Add an overweight column to the data. To determine if a person is overweight, first calculate their BMI by dividing their weight in kilograms by the square of their height in meters. If that value is > 25 then the person is overweight. Use the value 0 for NOT overweight and the value 1 for overweight.
    - Normalize the data by making 0 always good and 1 always bad. If the value of cholesterol or gluc is 1, make the value 0. If the value is more than 1, make the value 1.
    - Convert the data into long format and create a chart that shows the value counts of the categorical features using seaborn's catplot(). The dataset should be split by Cardio so there is one chart for each cardio value. The chart should look like examples/Figure_1.png.
    - Clean the data. Filter out the following patient segments that represent incorrect data:
        - diastolic pressure is higher than systolic (Keep the correct data with (df['ap_lo'] <= df['ap_hi']))
        - height is less than the 2.5th percentile (Keep the correct data with (df['height'] >= df['height'].quantile(0.025)))
        - height is more than the 97.5th percentile
        - weight is less than the 2.5th percentile
        - weight is more than the 97.5th percentile
    - Create a correlation matrix using the dataset. Plot the correlation matrix using seaborn's heatmap(). 
    - Mask the upper triangle. The chart should look like examples/Figure_2.png.
    - Any time a variable is set to None, make sure to set it to the correct code.

    - Unit tests are written for you under test_module.py.

    - Instructions
        - By each number in the medical_data_visualizer.py file, add the code from the associated instruction number below.

        - Import the data from medical_examination.csv and assign it to the df variable
        - Create the overweight column in the df variable
        - Normalize data by making 0 always good and 1 always bad. If the value of cholesterol or gluc is 1, set the value to 0. If the value is more than 1, set the value to 1.
        - Draw the Categorical Plot in the draw_cat_plot function
        - Create a DataFrame for the cat plot using pd.melt with values from cholesterol, gluc, smoke, alco, active, and overweight in the df_cat variable.
        - Group and reformat the data in df_cat to split it by cardio.
        -  Show the counts of each feature. You will have to rename one of the columns for the catplot to work correctly.
        - Convert the data into long format and create a chart that shows the value counts of the categorical features using the following method provided by the seaborn library import : sns.catplot()
        - Get the figure for the output and store it in the fig variable
        - Do not modify the next two lines
        - Draw the Heat Map in the draw_heat_map function
        - Clean the data in the df_heat variable by filtering out the following patient segments that represent incorrect data:
            - height is less than the 2.5th percentile (Keep the correct data with (df['height'] >= df['height'].quantile(0.025)))
            - height is more than the 97.5th percentile
            - weight is less than the 2.5th percentile
            - weight is more than the 97.5th percentile
            - Calculate the correlation matrix and store it in the corr variable
            - Generate a mask for the upper triangle and store it in the mask variable
            - Set up the matplotlib figure
            - Plot the correlation matrix using the method provided by the seaborn library import: sns.heatmap()
            - Do not modify the next two lines
    - Development
        - Write your code in medical_data_visualizer.py. 
        For development, you can use main.py to test your code.

    - Testing
        - The unit tests for this project are in test_module.py. We imported the tests from test_module.py to main.py for your convenience.

--- 

## Hints ⛑

- Main topics to study for better understanding:
  - Data cleaning
  - Normalization 
  - Heatmap creation and interpretation
  - pd.melt() how it works and its parameters

---

- Expected different values in heat map.

```sh
Traceback (most recent call last):
  File "./fcc-data-analysis-with-python/boilerplate-medical-data-visualizer/test_module.py", line 49, in test_heat_map_values
    self.assertEqual(actual, expected, "Expected different values in heat map.")
AssertionError: Lists differ: ['0.0[585 chars] '-0.2', '0.7', '0.0', '0.2', '0.1', '0.1', '-[22 chars]0.1'] != ['0.0[585 chars] '-0.1', '0.7', '0.0', '0.2', '0.1', '0.1', '-[22 chars]0.1']

First differing element 81:
'-0.2'
'-0.1'
```

Will cause error:

```py
df['overweight'] = (round((df['weight']/((df['height']/100) ** 2) > 25), 1)).astype(int)
```

Solution: remove the round() function and only check for the recommended condition (BMI over 25)

Works fine:

```py
df['overweight'] = (df['weight']/((df['height']/100) ** 2) > 25).astype(int)
```

Also probably helps debugging similar problens:

in test_module.py

add attribute **maxDiff** in class:

```py
class HeatMapTestCase(unittest.TestCase):

    def setUp(self):
        self.fig = medical_data_visualizer.draw_heat_map()
        self.ax = self.fig.axes[0]
   👉   self.maxDiff = None
```
