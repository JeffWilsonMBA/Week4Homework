

```python
import os
import pandas as pd
import numpy as np
```


```python
# Open the schools data file
schools = os.path.join("raw_data", "schools_complete.csv")
schools_df = pd.read_csv(schools)
# schools_df.head()
```


```python
# Open the students data file
students = os.path.join("raw_data", "students_complete.csv")
students_df = pd.read_csv(students)
# students_df.head()
```


```python
# schools_df.dtypes
```


```python
# students_df.dtypes
```


```python
## Part One - District Summary

# Key district metrics for distric summary table

total_schools  = len(schools_df)

total_students = schools_df["size"].sum()

total_budget = schools_df["budget"].sum()

avg_math_score = students_df["math_score"].mean()

avg_reading_score = students_df["reading_score"].mean()

# % Passing Math based on 70
math_pass = students_df.loc[(students_df["math_score"] >= 70)]
math_count = math_pass["math_score"].count()

# Need to pull in total students by school
passing_math = math_count/total_students*100

# % Passing Math based on 70
read_pass = students_df.loc[(students_df["reading_score"] >= 70)]
read_count = read_pass["reading_score"].count()
passing_reading = read_count/total_students*100

overall_passing = (passing_math + passing_reading)/2
```


```python
# Create district summary table
district_summary_table = pd.DataFrame({"Total Schools": [total_schools],
                                      "Total Students": [total_students],
                                      "Total Budget": [total_budget,],
                                      "Average Math Score": [avg_math_score],
                                      "Average Reading Score": [avg_reading_score],
                                      "% Passing Math": [passing_math],
                                      "% Passing Reading": [passing_reading],
                                      "Overall Passing Score": [overall_passing],
                                      })
district_summary_table = district_summary_table[["Total Schools",
                                                 "Total Students",
                                                 "Total Budget",
                                                 "Average Math Score",
                                                 "Average Reading Score",
                                                 "% Passing Math",
                                                 "% Passing Reading",
                                                 "Overall Passing Score",
                                                 ]]
district_summary_table = district_summary_table.round(2)

# Format table
district_summary_table["Total Students"] = district_summary_table["Total Students"].map("{0:,.0f}".format)
district_summary_table["Total Budget"] = district_summary_table["Total Budget"].map("{0:,.0f}".format)
district_summary_table["% Passing Math"] = district_summary_table["% Passing Math"].map("{0:,.2f}%".format)
district_summary_table["% Passing Reading"] = district_summary_table["% Passing Reading"].map("{0:,.2f}%".format)
district_summary_table["Overall Passing Score"] = district_summary_table["Overall Passing Score"].map("{0:,.2f}%".format)

##Part One Answer
#Print Table
print("District Summary")
district_summary_table
```

    District Summary
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>Overall Passing Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39,170</td>
      <td>24,649,428</td>
      <td>78.99</td>
      <td>81.88</td>
      <td>74.98%</td>
      <td>85.81%</td>
      <td>80.39%</td>
    </tr>
  </tbody>
</table>
</div>




```python
##Part Two - School Summary

# Add per student budget to schools_df
schools_df["Per Student Budget"] = schools_df["budget"]/schools_df["size"]

# Renames "name" to "school"
schools_df = schools_df.rename(columns={"name": "school"})

# Merge student and school tables
district_df = pd.merge(students_df, schools_df, on="school")
district_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>School ID</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>Per Student Budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
      <td>0</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Get the mean test scores by school and merge into schools_df
average_scores_table = district_df.groupby(["school"])['reading_score', 'math_score'].mean().reset_index()

schools_df = schools_df.merge(average_scores_table, on = 'school', how = "outer")
```


```python
## Get passing counts so we can calculate the percentage
# Get math counts
student_math_pass = district_df[district_df['math_score'] >=70]
math_pass_count = student_math_pass.groupby(["school"])['math_score'].count().reset_index()
math_pass_count.rename_axis({'math_score' : 'Math Count'}, axis=1, inplace=True)

# Get reading counts
student_reading_pass = district_df[district_df['reading_score'] >=70]
reading_pass_count = student_reading_pass.groupby(["school"])['reading_score'].count().reset_index()
reading_pass_count.rename_axis({'reading_score' : 'Reading Count'}, axis=1, inplace=True)

#Merge counts into one table
pass_count = math_pass_count.merge(reading_pass_count, on="school", how='inner')
pass_count

# Now merge into schools_df for calucalations
schools_df = schools_df.merge(pass_count, on = 'school', how = "outer")

schools_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>Per Student Budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>Math Count</th>
      <th>Reading Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>655.0</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>1916</td>
      <td>2372</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>639.0</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>1946</td>
      <td>2381</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>600.0</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1653</td>
      <td>1688</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>652.0</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>3094</td>
      <td>3748</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>625.0</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1371</td>
      <td>1426</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>578.0</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>2143</td>
      <td>2204</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>582.0</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>1749</td>
      <td>1803</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>Bailey High School</td>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>628.0</td>
      <td>81.033963</td>
      <td>77.048432</td>
      <td>3318</td>
      <td>4077</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>581.0</td>
      <td>83.814988</td>
      <td>83.803279</td>
      <td>395</td>
      <td>411</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>609.0</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>910</td>
      <td>923</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>583.0</td>
      <td>83.955000</td>
      <td>83.682222</td>
      <td>1680</td>
      <td>1739</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>637.0</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>2654</td>
      <td>3208</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>650.0</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>3145</td>
      <td>3867</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>Ford High School</td>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>644.0</td>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>1871</td>
      <td>2172</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>638.0</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>1525</td>
      <td>1591</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Calculate the percent passing for math and reading, and then the overall passing
schools_df["% Passing Math"] = schools_df["Math Count"]/schools_df['size']*100
schools_df["% Passing Reading"] = schools_df["Reading Count"]/schools_df['size']*100
schools_df["% Overall Passing"] = (schools_df["% Passing Math"] + schools_df["% Passing Reading"])/2

# Delect math and reading counts no longer needed
del schools_df['Math Count']
del schools_df['Reading Count']
del schools_df['School ID']

# making a copy of schools_df before formatting it
copy_schools_df = schools_df.copy(deep=True)
```


```python
# Format table
schools_df["size"] = schools_df["size"].map("{0:,.0f}".format)
schools_df["budget"] = schools_df["budget"].map("{0:,.0f}".format)
schools_df["Per Student Budget"] = schools_df["Per Student Budget"].map("{0:,.0f}".format)
schools_df["reading_score"] = schools_df["reading_score"].map("{0:,.2f}".format)
schools_df["math_score"] = schools_df["math_score"].map("{0:,.2f}".format)
schools_df["% Passing Math"] = schools_df["% Passing Math"].map("{0:,.2f}%".format)
schools_df["% Passing Reading"] = schools_df["% Passing Reading"].map("{0:,.2f}%".format)
schools_df["% Overall Passing"] = schools_df["% Overall Passing"].map("{0:,.2f}%".format)

print("School Summary")
schools_df
```

    School Summary
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>Per Student Budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2,917</td>
      <td>1,910,635</td>
      <td>655</td>
      <td>81.18</td>
      <td>76.63</td>
      <td>65.68%</td>
      <td>81.32%</td>
      <td>73.50%</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2,949</td>
      <td>1,884,411</td>
      <td>639</td>
      <td>81.16</td>
      <td>76.71</td>
      <td>65.99%</td>
      <td>80.74%</td>
      <td>73.36%</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1,761</td>
      <td>1,056,600</td>
      <td>600</td>
      <td>83.73</td>
      <td>83.36</td>
      <td>93.87%</td>
      <td>95.85%</td>
      <td>94.86%</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4,635</td>
      <td>3,022,020</td>
      <td>652</td>
      <td>80.93</td>
      <td>77.29</td>
      <td>66.75%</td>
      <td>80.86%</td>
      <td>73.81%</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1,468</td>
      <td>917,500</td>
      <td>625</td>
      <td>83.82</td>
      <td>83.35</td>
      <td>93.39%</td>
      <td>97.14%</td>
      <td>95.27%</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2,283</td>
      <td>1,319,574</td>
      <td>578</td>
      <td>83.99</td>
      <td>83.27</td>
      <td>93.87%</td>
      <td>96.54%</td>
      <td>95.20%</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1,858</td>
      <td>1,081,356</td>
      <td>582</td>
      <td>83.98</td>
      <td>83.06</td>
      <td>94.13%</td>
      <td>97.04%</td>
      <td>95.59%</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Bailey High School</td>
      <td>District</td>
      <td>4,976</td>
      <td>3,124,928</td>
      <td>628</td>
      <td>81.03</td>
      <td>77.05</td>
      <td>66.68%</td>
      <td>81.93%</td>
      <td>74.31%</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>427</td>
      <td>248,087</td>
      <td>581</td>
      <td>83.81</td>
      <td>83.80</td>
      <td>92.51%</td>
      <td>96.25%</td>
      <td>94.38%</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>585,858</td>
      <td>609</td>
      <td>84.04</td>
      <td>83.84</td>
      <td>94.59%</td>
      <td>95.95%</td>
      <td>95.27%</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1,800</td>
      <td>1,049,400</td>
      <td>583</td>
      <td>83.95</td>
      <td>83.68</td>
      <td>93.33%</td>
      <td>96.61%</td>
      <td>94.97%</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3,999</td>
      <td>2,547,363</td>
      <td>637</td>
      <td>80.74</td>
      <td>76.84</td>
      <td>66.37%</td>
      <td>80.22%</td>
      <td>73.29%</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4,761</td>
      <td>3,094,650</td>
      <td>650</td>
      <td>80.97</td>
      <td>77.07</td>
      <td>66.06%</td>
      <td>81.22%</td>
      <td>73.64%</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Ford High School</td>
      <td>District</td>
      <td>2,739</td>
      <td>1,763,916</td>
      <td>644</td>
      <td>80.75</td>
      <td>77.10</td>
      <td>68.31%</td>
      <td>79.30%</td>
      <td>73.80%</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1,635</td>
      <td>1,043,130</td>
      <td>638</td>
      <td>83.85</td>
      <td>83.42</td>
      <td>93.27%</td>
      <td>97.31%</td>
      <td>95.29%</td>
    </tr>
  </tbody>
</table>
</div>




```python
sorted_schools = schools_df.sort_values("% Overall Passing", ascending=False)
print("Top Performing Schools")
sorted_schools.head()
```

    Top Performing Schools
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>Per Student Budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1,858</td>
      <td>1,081,356</td>
      <td>582</td>
      <td>83.98</td>
      <td>83.06</td>
      <td>94.13%</td>
      <td>97.04%</td>
      <td>95.59%</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1,635</td>
      <td>1,043,130</td>
      <td>638</td>
      <td>83.85</td>
      <td>83.42</td>
      <td>93.27%</td>
      <td>97.31%</td>
      <td>95.29%</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1,468</td>
      <td>917,500</td>
      <td>625</td>
      <td>83.82</td>
      <td>83.35</td>
      <td>93.39%</td>
      <td>97.14%</td>
      <td>95.27%</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>585,858</td>
      <td>609</td>
      <td>84.04</td>
      <td>83.84</td>
      <td>94.59%</td>
      <td>95.95%</td>
      <td>95.27%</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2,283</td>
      <td>1,319,574</td>
      <td>578</td>
      <td>83.99</td>
      <td>83.27</td>
      <td>93.87%</td>
      <td>96.54%</td>
      <td>95.20%</td>
    </tr>
  </tbody>
</table>
</div>




```python
print("Bottom Performing Schools")
sorted_schools.tail()
```

    Bottom Performing Schools
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>Per Student Budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>13</th>
      <td>Ford High School</td>
      <td>District</td>
      <td>2,739</td>
      <td>1,763,916</td>
      <td>644</td>
      <td>80.75</td>
      <td>77.10</td>
      <td>68.31%</td>
      <td>79.30%</td>
      <td>73.80%</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4,761</td>
      <td>3,094,650</td>
      <td>650</td>
      <td>80.97</td>
      <td>77.07</td>
      <td>66.06%</td>
      <td>81.22%</td>
      <td>73.64%</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2,917</td>
      <td>1,910,635</td>
      <td>655</td>
      <td>81.18</td>
      <td>76.63</td>
      <td>65.68%</td>
      <td>81.32%</td>
      <td>73.50%</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2,949</td>
      <td>1,884,411</td>
      <td>639</td>
      <td>81.16</td>
      <td>76.71</td>
      <td>65.99%</td>
      <td>80.74%</td>
      <td>73.36%</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3,999</td>
      <td>2,547,363</td>
      <td>637</td>
      <td>80.74</td>
      <td>76.84</td>
      <td>66.37%</td>
      <td>80.22%</td>
      <td>73.29%</td>
    </tr>
  </tbody>
</table>
</div>




```python
##Part Five - Rading Score by Grade
math_scores_grade = pd.pivot_table(district_df, index = ["school"],
                                   values = ("math_score"), 
                                   columns = ["grade"], aggfunc=np.mean)

# Clean up header and reorder columns
math_flattened = pd.DataFrame(math_scores_grade.to_records())
math_flattened = math_flattened[["school", "9th", "10th", "11th", "12th"]]

print("Math Scores by Grade")
math_flattened
```

    Math Scores by Grade
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hernandez High School</td>
      <td>77.438495</td>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Holden High School</td>
      <td>83.787402</td>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Huang High School</td>
      <td>77.027251</td>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Johnson High School</td>
      <td>77.187857</td>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>83.625455</td>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rodriguez High School</td>
      <td>76.859966</td>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Shelton High School</td>
      <td>83.420755</td>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Thomas High School</td>
      <td>83.590022</td>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wilson High School</td>
      <td>83.085578</td>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Wright High School</td>
      <td>83.264706</td>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>




```python
##Part Six - Rading Score by Grade
reading_scores_grade = pd.pivot_table(district_df, index = ["school"],
                                   values = ("reading_score"), 
                                   columns = ["grade"], aggfunc=np.mean)

# Clean up header and reorder columns
reading_flattened = pd.DataFrame(reading_scores_grade.to_records())
reading_flattened = reading_flattened[["school", "9th", "10th", "11th", "12th"]]

print("Reading Scores by Grade")
reading_flattened
```

    Reading Scores by Grade
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cabrera High School</td>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Figueroa High School</td>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Ford High School</td>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Hernandez High School</td>
      <td>80.866860</td>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Holden High School</td>
      <td>83.677165</td>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Huang High School</td>
      <td>81.290284</td>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Johnson High School</td>
      <td>81.260714</td>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>83.807273</td>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rodriguez High School</td>
      <td>80.993127</td>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Shelton High School</td>
      <td>84.122642</td>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Thomas High School</td>
      <td>83.728850</td>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Wilson High School</td>
      <td>83.939778</td>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Wright High School</td>
      <td>83.833333</td>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create scores by spending table
bins = [0,585,615,645,1000]
group_labels = ["0-585", "585 to 615", "615 to 645", "645+"]

# Making a copy of copy_schools_df for use in the size exercise
size_schools_df = copy_schools_df.copy(deep=True)
pd.cut(copy_schools_df["Per Student Budget"],bins,labels = group_labels).head()
```




    0          645+
    1    615 to 645
    2    585 to 615
    3          645+
    4    615 to 645
    Name: Per Student Budget, dtype: category
    Categories (4, object): [0-585 < 585 to 615 < 615 to 645 < 645+]




```python
copy_schools_df["Budget Group"] = pd.cut(copy_schools_df["Per Student Budget"],bins,labels=group_labels)
del copy_schools_df['size']
del copy_schools_df['budget']
del copy_schools_df['Per Student Budget']
copy_schools_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>type</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
      <th>Budget Group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
      <td>645+</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
      <td>615 to 645</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
      <td>585 to 615</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
      <td>645+</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
      <td>615 to 645</td>
    </tr>
  </tbody>
</table>
</div>




```python
district_budget = copy_schools_df.groupby("Budget Group")
pd.DataFrame(district_budget.size().reset_index())
print("Scores by Spending")
district_budget.mean()
```

    Scores by Spending
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
    <tr>
      <th>Budget Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0-585</th>
      <td>83.933814</td>
      <td>83.455399</td>
      <td>93.460096</td>
      <td>96.610877</td>
      <td>95.035486</td>
    </tr>
    <tr>
      <th>585 to 615</th>
      <td>83.885211</td>
      <td>83.599686</td>
      <td>94.230858</td>
      <td>95.900287</td>
      <td>95.065572</td>
    </tr>
    <tr>
      <th>615 to 645</th>
      <td>81.891436</td>
      <td>79.079225</td>
      <td>75.668212</td>
      <td>86.106569</td>
      <td>80.887391</td>
    </tr>
    <tr>
      <th>645+</th>
      <td>81.027843</td>
      <td>76.997210</td>
      <td>66.164813</td>
      <td>81.133951</td>
      <td>73.649382</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Scores by size
bins = [0,1000,2000,5000]
size_group_labels = ["<1000", "1000 to 2000", "2000 and up"]

# Making a copy of copy_schools_df for use in the size exercise
pd.cut(size_schools_df["size"],bins,labels = size_group_labels).head()
```




    0     2000 and up
    1     2000 and up
    2    1000 to 2000
    3     2000 and up
    4    1000 to 2000
    Name: size, dtype: category
    Categories (3, object): [<1000 < 1000 to 2000 < 2000 and up]




```python
size_schools_df["Size Group"] = pd.cut(size_schools_df["size"],bins,labels=size_group_labels)
del size_schools_df['budget']
del size_schools_df['Per Student Budget']
size_schools_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
      <th>Size Group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
      <td>2000 and up</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
      <td>2000 and up</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
      <td>1000 to 2000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
      <td>2000 and up</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
      <td>1000 to 2000</td>
    </tr>
  </tbody>
</table>
</div>




```python
district_size = size_schools_df.groupby("Size Group")
pd.DataFrame(district_size.size().reset_index())
print("Scores by School Size")
district_size.mean()
```

    Scores by School Size
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>size</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
    <tr>
      <th>Size Group</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;1000</th>
      <td>694.500</td>
      <td>83.929843</td>
      <td>83.821598</td>
      <td>93.550225</td>
      <td>96.099437</td>
      <td>94.824831</td>
    </tr>
    <tr>
      <th>1000 to 2000</th>
      <td>1704.400</td>
      <td>83.864438</td>
      <td>83.374684</td>
      <td>93.599695</td>
      <td>96.790680</td>
      <td>95.195187</td>
    </tr>
    <tr>
      <th>2000 and up</th>
      <td>3657.375</td>
      <td>81.344493</td>
      <td>77.746417</td>
      <td>69.963361</td>
      <td>82.766634</td>
      <td>76.364998</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Scores by type of school
del size_schools_df['Size Group']
by_type = size_schools_df.groupby("type")
pd.DataFrame(by_type.size().reset_index())
print("Scores by School Type")
by_type.mean()
```

    Scores by School Type
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>size</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>1524.250000</td>
      <td>83.896421</td>
      <td>83.473852</td>
      <td>93.620830</td>
      <td>96.586489</td>
      <td>95.103660</td>
    </tr>
    <tr>
      <th>District</th>
      <td>3853.714286</td>
      <td>80.966636</td>
      <td>76.956733</td>
      <td>66.548453</td>
      <td>80.799062</td>
      <td>73.673757</td>
    </tr>
  </tbody>
</table>
</div>


