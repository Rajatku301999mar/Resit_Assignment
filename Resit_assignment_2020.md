# Resit Take-Home Assignment - Data Analysis and Quantitative trading 

**This is an individual assignment. It has been solved by:** 

|     Name       | Student number    | Email           |
| :------------: | :---------------: | :-------------: | 
| [name] |       [student number]    |    [email]      |

Please fill your credentials in the table above by double clicking on the text and replacing the current text in brackets by your own name, student number and email. 

By submitting this assignment you consent to the course policy on cheating and UvA policy on plagiarism.

# Part 1. Theoretical questions

Objective: In this section, you must explain a few key concepts from the course in your own words.

To Complete:
- Submit answers to questions below. 

**Step 1(a)**    
Explain in general terms how quantiative trading firms can use finance theory to identify convergence arbitrage opportunities. Also explain why theory does not offer guidance to firms on how long the arbitrage trade must be kept open.

<div class="alert alert-block alert-warning">
Click on this box and type your answer. It is not necessary to fill out any tables.
</div>

**Step 1(b)**    
The CDS-Bond basis examples and pricing formulas presented in the course focus on the case where the bond trades at par. Briefly explain in words why the formula does not work exactly when the bond trades above par, i.e., the price is greater than the face value.

Highlight the problem by filling out the below table, which is based on Slide 24 of Lecture 2. In the example, assume a 5-year bond has face value of 1,000, price of 1,200, and a recovery rate of 70%. 

<div class="alert alert-block alert-warning">
Answer by double clicking this window and filling out the table below. You may not need to use all the rows in the table.
    <br><br>
    
| Position <br>      | Initial <br> Payment     | Settlement, <br> No Default in Year 5    | Settlement, <br> Default in Year 5|
|:-------------------|:---------------:|:------------:|:----------------:|
|Position 1          |[answer]         |[answer]      |[answer]          |
|Position 2          |[answer]         |[answer]      |[answer]          |
|Position 3          |[answer]         |[answer]      |[answer]          |
|Position 4          |[answer]         |[answer]      |[answer]          |
|Total               |[answer]         |[answer]      |[answer]          |
    
</div>

**Step 1(c)**    
Define the SMB factor, i.e., how is it measured and what does it represent. Explain why asset pricing researchers decided to add the factor to the CAPM model, and what is their argument for why the factor helps explain stock returns. 

<div class="alert alert-block alert-warning">
Click on this box and type your answer. It is not necessary to fill out any tables.
</div>

# Part 2. Determinants of the CDS-Bond Basis

**Objective:** In this section, you will examine how deviations from the CDS-Bond basis relate to various firm characteristics. The goal is to understand what type of firms experience deviations in the CDS-Bond basis---this can be useful to determine whether those deviations are due to market inefficiency or could be explained by other factors such as lack of liquidity. 

In the steps below, you need to combine data on the CDS-Bond basis with firm fundamentals from the Compustat database. You then need to estimate linear regressions relating CDS-Bond basis deviations to the firm fundamentals.

**Getting started**: The output file from Step #4 of Homework 2 is necessary to complete this part of the assignment. Throughout these instructions, I will refer to this file as “HW2_output.txt”. You also need the file "firm_fundamentals.txt" with data from Compustat. 

**To Complete:**
- Fill in all code boxes 
- Fill in all tables and answer boxes below.

**Note on writing code:** To solve the individual steps of this part of the assignment, you can use either the "input-output" method of reading in files line-by-line or the Pandas module (or both). Some steps may be significantly easier to complete using Pandas. 

**Step 2(a)**    
Read in the file HW2_output.txt and briefly describe the data that it contains. Explain at what level the observations are defined, and the time period that the sample covers. 

<div class="alert alert-block alert-warning">
Click on this box and type your answer. It is not necessary to fill out any tables.
</div>


```python
import pandas as pd
df = pd.read_csv("HW2_output.txt", error_bad_lines=False,sep="\t")
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>company</th>
      <th>gvkey</th>
      <th>date</th>
      <th>market_cds_spread</th>
      <th>bond_yield</th>
      <th>swap_fixed_rate</th>
      <th>implied_CDS_spread</th>
      <th>basis_deviation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20040102</td>
      <td>10.0</td>
      <td>NaN</td>
      <td>3.79</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20040105</td>
      <td>10.8</td>
      <td>NaN</td>
      <td>3.79</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20040106</td>
      <td>10.8</td>
      <td>NaN</td>
      <td>3.67</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20040107</td>
      <td>10.8</td>
      <td>NaN</td>
      <td>3.62</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20040108</td>
      <td>10.5</td>
      <td>NaN</td>
      <td>3.62</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.shape
```




    (1374147, 8)




```python
df.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gvkey</th>
      <th>date</th>
      <th>market_cds_spread</th>
      <th>bond_yield</th>
      <th>swap_fixed_rate</th>
      <th>implied_CDS_spread</th>
      <th>basis_deviation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1.374147e+06</td>
      <td>1.374147e+06</td>
      <td>1.096650e+06</td>
      <td>76914.000000</td>
      <td>1.299383e+06</td>
      <td>63687.000000</td>
      <td>63687.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>3.155164e+04</td>
      <td>2.010277e+07</td>
      <td>2.771288e+02</td>
      <td>3.664372</td>
      <td>2.694218e+00</td>
      <td>169.641668</td>
      <td>-6.434030</td>
    </tr>
    <tr>
      <th>std</th>
      <td>4.988769e+04</td>
      <td>3.699396e+04</td>
      <td>1.274475e+03</td>
      <td>1.961050</td>
      <td>1.471940e+00</td>
      <td>181.899404</td>
      <td>203.391909</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.045000e+03</td>
      <td>2.004010e+07</td>
      <td>1.000000e+00</td>
      <td>0.024789</td>
      <td>7.300000e-01</td>
      <td>-500.700000</td>
      <td>-542.200000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>5.073000e+03</td>
      <td>2.007073e+07</td>
      <td>5.000000e+01</td>
      <td>2.131615</td>
      <td>1.490000e+00</td>
      <td>41.990000</td>
      <td>-50.855000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>9.459000e+03</td>
      <td>2.010102e+07</td>
      <td>1.026800e+02</td>
      <td>2.819478</td>
      <td>2.180000e+00</td>
      <td>93.700000</td>
      <td>-7.220000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>2.534000e+04</td>
      <td>2.013113e+07</td>
      <td>2.471100e+02</td>
      <td>5.130947</td>
      <td>4.080000e+00</td>
      <td>248.955000</td>
      <td>21.685000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>2.603290e+05</td>
      <td>2.016123e+07</td>
      <td>6.111711e+04</td>
      <td>12.271930</td>
      <td>5.760000e+00</td>
      <td>1029.190000</td>
      <td>5047.930000</td>
    </tr>
  </tbody>
</table>
</div>



**Step 2(b)**     
In this step you will begin to prepare a dataset for estimating linear regressions. Read through HW2_output.txt and complete the following:
- Retain only observations with a non-missing value for the variable *basis_deviation*. (Missing values are expressed as "NA" in the file.)
- Linear regression estimates are very sensitive to the presence of outliers in the data. Therefore omit some observations that are potential outliers: Those with *market_cds_spread* of 2,000 or higher, *bond_yield* of 20 or higher, and *basis_deviation* of 2,000 or higher. **Note: Write a single logical expression that executes this.**
- Create a new variable called *negative_deviation* that equals 1 for observations with a negative value of *basis_deviation*, and 0 for observations with a non-negative value. 
- Create a new variable called *pre_crisis_period* that equals 1 for observations from January 1, 2004 though August 31, 2008, and 0 for all observations from April 1, 2009 though December 31, 2015. Omit other observations from the height of the financial crisis. 

After completing the above, save only the variables *gvkey*, *date*, *basis_deviation*, *pre_crisis_period*, and *negative_deviation* in a separate output file or Pandas data frame.


```python
## ENTER YOUR CODE IN THIS BOX
df.dropna(subset = ["basis_deviation"], inplace=True)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>company</th>
      <th>gvkey</th>
      <th>date</th>
      <th>market_cds_spread</th>
      <th>bond_yield</th>
      <th>swap_fixed_rate</th>
      <th>implied_CDS_spread</th>
      <th>basis_deviation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110926</td>
      <td>52.259998</td>
      <td>1.551084</td>
      <td>1.17</td>
      <td>38.11</td>
      <td>14.15</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110927</td>
      <td>52.270000</td>
      <td>1.504463</td>
      <td>1.25</td>
      <td>25.45</td>
      <td>26.82</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110928</td>
      <td>51.230000</td>
      <td>1.451097</td>
      <td>1.27</td>
      <td>18.11</td>
      <td>33.12</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110929</td>
      <td>50.240002</td>
      <td>1.437551</td>
      <td>1.27</td>
      <td>16.76</td>
      <td>33.48</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110930</td>
      <td>51.259998</td>
      <td>1.392073</td>
      <td>1.25</td>
      <td>14.21</td>
      <td>37.05</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1374077</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20160926</td>
      <td>156.089996</td>
      <td>3.840031</td>
      <td>1.16</td>
      <td>268.00</td>
      <td>-111.91</td>
    </tr>
    <tr>
      <th>1374078</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20160927</td>
      <td>156.580002</td>
      <td>3.544972</td>
      <td>1.15</td>
      <td>239.50</td>
      <td>-82.92</td>
    </tr>
    <tr>
      <th>1374079</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20160928</td>
      <td>153.710007</td>
      <td>3.593457</td>
      <td>1.14</td>
      <td>245.35</td>
      <td>-91.64</td>
    </tr>
    <tr>
      <th>1374098</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20161025</td>
      <td>143.690002</td>
      <td>3.749937</td>
      <td>1.29</td>
      <td>245.99</td>
      <td>-102.30</td>
    </tr>
    <tr>
      <th>1374100</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20161027</td>
      <td>142.259995</td>
      <td>3.648401</td>
      <td>1.38</td>
      <td>226.84</td>
      <td>-84.58</td>
    </tr>
  </tbody>
</table>
<p>63687 rows × 8 columns</p>
</div>




```python
df = df.drop(df[df.market_cds_spread >= 2000].index)
df = df.drop(df[df.bond_yield >= 20].index)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>company</th>
      <th>gvkey</th>
      <th>date</th>
      <th>market_cds_spread</th>
      <th>bond_yield</th>
      <th>swap_fixed_rate</th>
      <th>implied_CDS_spread</th>
      <th>basis_deviation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110926</td>
      <td>52.259998</td>
      <td>1.551084</td>
      <td>1.17</td>
      <td>38.11</td>
      <td>14.15</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110927</td>
      <td>52.270000</td>
      <td>1.504463</td>
      <td>1.25</td>
      <td>25.45</td>
      <td>26.82</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110928</td>
      <td>51.230000</td>
      <td>1.451097</td>
      <td>1.27</td>
      <td>18.11</td>
      <td>33.12</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110929</td>
      <td>50.240002</td>
      <td>1.437551</td>
      <td>1.27</td>
      <td>16.76</td>
      <td>33.48</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110930</td>
      <td>51.259998</td>
      <td>1.392073</td>
      <td>1.25</td>
      <td>14.21</td>
      <td>37.05</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1374077</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20160926</td>
      <td>156.089996</td>
      <td>3.840031</td>
      <td>1.16</td>
      <td>268.00</td>
      <td>-111.91</td>
    </tr>
    <tr>
      <th>1374078</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20160927</td>
      <td>156.580002</td>
      <td>3.544972</td>
      <td>1.15</td>
      <td>239.50</td>
      <td>-82.92</td>
    </tr>
    <tr>
      <th>1374079</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20160928</td>
      <td>153.710007</td>
      <td>3.593457</td>
      <td>1.14</td>
      <td>245.35</td>
      <td>-91.64</td>
    </tr>
    <tr>
      <th>1374098</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20161025</td>
      <td>143.690002</td>
      <td>3.749937</td>
      <td>1.29</td>
      <td>245.99</td>
      <td>-102.30</td>
    </tr>
    <tr>
      <th>1374100</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20161027</td>
      <td>142.259995</td>
      <td>3.648401</td>
      <td>1.38</td>
      <td>226.84</td>
      <td>-84.58</td>
    </tr>
  </tbody>
</table>
<p>63582 rows × 8 columns</p>
</div>




```python
df["negative_deviation"] = ""
df['negative_deviation'][df['basis_deviation'] < 0] = 1
df['negative_deviation'][df['basis_deviation'] >= 0] = 0
df
```

    <ipython-input-6-e6ea8ed448f8>:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['negative_deviation'][df['basis_deviation'] < 0] = 1
    <ipython-input-6-e6ea8ed448f8>:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['negative_deviation'][df['basis_deviation'] >= 0] = 0
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>company</th>
      <th>gvkey</th>
      <th>date</th>
      <th>market_cds_spread</th>
      <th>bond_yield</th>
      <th>swap_fixed_rate</th>
      <th>implied_CDS_spread</th>
      <th>basis_deviation</th>
      <th>negative_deviation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110926</td>
      <td>52.259998</td>
      <td>1.551084</td>
      <td>1.17</td>
      <td>38.11</td>
      <td>14.15</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110927</td>
      <td>52.270000</td>
      <td>1.504463</td>
      <td>1.25</td>
      <td>25.45</td>
      <td>26.82</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110928</td>
      <td>51.230000</td>
      <td>1.451097</td>
      <td>1.27</td>
      <td>18.11</td>
      <td>33.12</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110929</td>
      <td>50.240002</td>
      <td>1.437551</td>
      <td>1.27</td>
      <td>16.76</td>
      <td>33.48</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110930</td>
      <td>51.259998</td>
      <td>1.392073</td>
      <td>1.25</td>
      <td>14.21</td>
      <td>37.05</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1374077</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20160926</td>
      <td>156.089996</td>
      <td>3.840031</td>
      <td>1.16</td>
      <td>268.00</td>
      <td>-111.91</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1374078</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20160927</td>
      <td>156.580002</td>
      <td>3.544972</td>
      <td>1.15</td>
      <td>239.50</td>
      <td>-82.92</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1374079</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20160928</td>
      <td>153.710007</td>
      <td>3.593457</td>
      <td>1.14</td>
      <td>245.35</td>
      <td>-91.64</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1374098</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20161025</td>
      <td>143.690002</td>
      <td>3.749937</td>
      <td>1.29</td>
      <td>245.99</td>
      <td>-102.30</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1374100</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20161027</td>
      <td>142.259995</td>
      <td>3.648401</td>
      <td>1.38</td>
      <td>226.84</td>
      <td>-84.58</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>63582 rows × 9 columns</p>
</div>




```python
df["pre_crisis_period"] = ""
df['pre_crisis_period'][(df['date'] <= 20080831) & (df['date']>= 20040101)] = 1
df['pre_crisis_period'][(df['date'] <= 20151231) & (df['date'] >= 20090104)] = 0
df = df.drop(df[df.date > 20151231].index)
df
```

    <ipython-input-7-cf064df15ab1>:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['pre_crisis_period'][(df['date'] <= 20080831) & (df['date']>= 20040101)] = 1
    <ipython-input-7-cf064df15ab1>:3: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      df['pre_crisis_period'][(df['date'] <= 20151231) & (df['date'] >= 20090104)] = 0
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>company</th>
      <th>gvkey</th>
      <th>date</th>
      <th>market_cds_spread</th>
      <th>bond_yield</th>
      <th>swap_fixed_rate</th>
      <th>implied_CDS_spread</th>
      <th>basis_deviation</th>
      <th>negative_deviation</th>
      <th>pre_crisis_period</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110926</td>
      <td>52.259998</td>
      <td>1.551084</td>
      <td>1.17</td>
      <td>38.11</td>
      <td>14.15</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110927</td>
      <td>52.270000</td>
      <td>1.504463</td>
      <td>1.25</td>
      <td>25.45</td>
      <td>26.82</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110928</td>
      <td>51.230000</td>
      <td>1.451097</td>
      <td>1.27</td>
      <td>18.11</td>
      <td>33.12</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110929</td>
      <td>50.240002</td>
      <td>1.437551</td>
      <td>1.27</td>
      <td>16.76</td>
      <td>33.48</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>3M COMPANY</td>
      <td>7435</td>
      <td>20110930</td>
      <td>51.259998</td>
      <td>1.392073</td>
      <td>1.25</td>
      <td>14.21</td>
      <td>37.05</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1373876</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20151218</td>
      <td>206.979996</td>
      <td>4.103321</td>
      <td>1.67</td>
      <td>243.33</td>
      <td>-36.35</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1373877</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20151221</td>
      <td>207.910004</td>
      <td>4.001055</td>
      <td>1.67</td>
      <td>233.11</td>
      <td>-25.20</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1373878</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20151222</td>
      <td>207.899994</td>
      <td>3.940443</td>
      <td>1.69</td>
      <td>225.04</td>
      <td>-17.14</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1373879</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20151223</td>
      <td>206.929993</td>
      <td>4.005939</td>
      <td>1.71</td>
      <td>229.59</td>
      <td>-22.66</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1373882</th>
      <td>YUM! BRANDS</td>
      <td>65417</td>
      <td>20151228</td>
      <td>205.500000</td>
      <td>4.046410</td>
      <td>1.69</td>
      <td>235.64</td>
      <td>-30.14</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>57670 rows × 10 columns</p>
</div>




```python
df2 = df[['gvkey', 'date', 'basis_deviation', 'pre_crisis_period', 'negative_deviation']].copy()
df2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gvkey</th>
      <th>date</th>
      <th>basis_deviation</th>
      <th>pre_crisis_period</th>
      <th>negative_deviation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016</th>
      <td>7435</td>
      <td>20110926</td>
      <td>14.15</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>7435</td>
      <td>20110927</td>
      <td>26.82</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>7435</td>
      <td>20110928</td>
      <td>33.12</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>7435</td>
      <td>20110929</td>
      <td>33.48</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>7435</td>
      <td>20110930</td>
      <td>37.05</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1373876</th>
      <td>65417</td>
      <td>20151218</td>
      <td>-36.35</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1373877</th>
      <td>65417</td>
      <td>20151221</td>
      <td>-25.20</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1373878</th>
      <td>65417</td>
      <td>20151222</td>
      <td>-17.14</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1373879</th>
      <td>65417</td>
      <td>20151223</td>
      <td>-22.66</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1373882</th>
      <td>65417</td>
      <td>20151228</td>
      <td>-30.14</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>57670 rows × 5 columns</p>
</div>



**Step 2(c)**    
Read in the file firm_fundamentals.txt and briefly describe the data that it contains. Explain at what level the observations are defined and the time period that the sample covers. Also briefly list three variables which represent firm characteristics that you think may affect the CDS-Bond basis.

<div class="alert alert-block alert-warning">
Click on this box and type your answer. It is not necessary to fill out any tables.
</div>


```python
import pandas as pd
dffirm = pd.read_csv("firm_fundamentals.txt", error_bad_lines=False,sep="\t")
dffirm.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gvkey</th>
      <th>year</th>
      <th>fiscal_year_end_date</th>
      <th>company_name</th>
      <th>sp1500_firm</th>
      <th>country</th>
      <th>total_assets</th>
      <th>book_value_equity</th>
      <th>market_value_equity</th>
      <th>cash_assets</th>
      <th>total_debt</th>
      <th>debt_due_next_year</th>
      <th>profitability</th>
      <th>annual_stock_return</th>
      <th>stock_volatility</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1010</td>
      <td>2000</td>
      <td>20001231</td>
      <td>ACF INDUSTRIES HOLDING CORP</td>
      <td>0</td>
      <td>USA</td>
      <td>3794.500</td>
      <td>985.200</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2042.900</td>
      <td>0.092819</td>
      <td>0.180036</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1010</td>
      <td>2001</td>
      <td>20011231</td>
      <td>ACF INDUSTRIES HOLDING CORP</td>
      <td>0</td>
      <td>USA</td>
      <td>3723.100</td>
      <td>954.200</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1905.500</td>
      <td>0.062851</td>
      <td>0.397544</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1010</td>
      <td>2002</td>
      <td>20021231</td>
      <td>ACF INDUSTRIES HOLDING CORP</td>
      <td>0</td>
      <td>USA</td>
      <td>3702.500</td>
      <td>892.600</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1968.700</td>
      <td>0.106415</td>
      <td>0.254389</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1010</td>
      <td>2003</td>
      <td>20031231</td>
      <td>ACF INDUSTRIES HOLDING CORP</td>
      <td>0</td>
      <td>USA</td>
      <td>4832.100</td>
      <td>1655.000</td>
      <td>NaN</td>
      <td>0.134641</td>
      <td>1860.100</td>
      <td>0.065148</td>
      <td>0.150699</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1019</td>
      <td>2000</td>
      <td>20001231</td>
      <td>AFA PROTECTIVE SYSTEMS INC</td>
      <td>0</td>
      <td>USA</td>
      <td>28.638</td>
      <td>13.184</td>
      <td>30.378</td>
      <td>0.063482</td>
      <td>1.786</td>
      <td>0.045778</td>
      <td>0.041455</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1019</td>
      <td>2001</td>
      <td>20011231</td>
      <td>AFA PROTECTIVE SYSTEMS INC</td>
      <td>0</td>
      <td>USA</td>
      <td>30.836</td>
      <td>12.026</td>
      <td>39.721</td>
      <td>0.057173</td>
      <td>3.129</td>
      <td>0.100337</td>
      <td>0.028635</td>
      <td>0.382514</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1034</td>
      <td>2000</td>
      <td>20001231</td>
      <td>ALPHARMA INC  -CL A</td>
      <td>1</td>
      <td>USA</td>
      <td>1610.435</td>
      <td>847.887</td>
      <td>1764.389</td>
      <td>0.045287</td>
      <td>525.121</td>
      <td>0.025677</td>
      <td>0.061621</td>
      <td>NaN</td>
      <td>0.1339</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1034</td>
      <td>2001</td>
      <td>20011231</td>
      <td>ALPHARMA INC  -CL A</td>
      <td>1</td>
      <td>USA</td>
      <td>2390.008</td>
      <td>891.616</td>
      <td>1172.211</td>
      <td>0.006232</td>
      <td>1060.592</td>
      <td>0.023443</td>
      <td>-0.038887</td>
      <td>-0.397151</td>
      <td>0.1440</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1034</td>
      <td>2002</td>
      <td>20021231</td>
      <td>ALPHARMA INC  -CL A</td>
      <td>1</td>
      <td>USA</td>
      <td>2296.924</td>
      <td>1001.843</td>
      <td>612.710</td>
      <td>0.010393</td>
      <td>895.858</td>
      <td>0.033603</td>
      <td>-0.080975</td>
      <td>-0.549716</td>
      <td>0.1724</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1034</td>
      <td>2003</td>
      <td>20031231</td>
      <td>ALPHARMA INC  -CL A</td>
      <td>1</td>
      <td>USA</td>
      <td>2329.268</td>
      <td>1131.989</td>
      <td>1045.883</td>
      <td>0.025168</td>
      <td>817.156</td>
      <td>0.025894</td>
      <td>0.010663</td>
      <td>0.687658</td>
      <td>0.1723</td>
    </tr>
  </tbody>
</table>
</div>




```python
dffirm.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gvkey</th>
      <th>year</th>
      <th>fiscal_year_end_date</th>
      <th>sp1500_firm</th>
      <th>total_assets</th>
      <th>book_value_equity</th>
      <th>market_value_equity</th>
      <th>cash_assets</th>
      <th>total_debt</th>
      <th>debt_due_next_year</th>
      <th>profitability</th>
      <th>annual_stock_return</th>
      <th>stock_volatility</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>125847.000000</td>
      <td>125847.000000</td>
      <td>1.258470e+05</td>
      <td>125847.000000</td>
      <td>1.046710e+05</td>
      <td>104364.000000</td>
      <td>1.088530e+05</td>
      <td>102534.000000</td>
      <td>1.043040e+05</td>
      <td>99003.000000</td>
      <td>98158.000000</td>
      <td>94755.000000</td>
      <td>44081.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>84950.693040</td>
      <td>2009.130325</td>
      <td>2.009253e+07</td>
      <td>0.126988</td>
      <td>1.238911e+04</td>
      <td>1877.717501</td>
      <td>3.739485e+03</td>
      <td>0.148393</td>
      <td>3.530921e+03</td>
      <td>1.271861</td>
      <td>-10.523709</td>
      <td>2.346061</td>
      <td>0.162944</td>
    </tr>
    <tr>
      <th>std</th>
      <td>71790.977679</td>
      <td>5.766511</td>
      <td>5.766511e+04</td>
      <td>0.332960</td>
      <td>1.005272e+05</td>
      <td>9471.282221</td>
      <td>1.844686e+04</td>
      <td>0.211071</td>
      <td>4.262721e+04</td>
      <td>46.974933</td>
      <td>277.867929</td>
      <td>255.978049</td>
      <td>0.107595</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1010.000000</td>
      <td>2000.000000</td>
      <td>2.000123e+07</td>
      <td>0.000000</td>
      <td>0.000000e+00</td>
      <td>-139965.000000</td>
      <td>0.000000e+00</td>
      <td>-0.825622</td>
      <td>-1.100000e+02</td>
      <td>-0.068244</td>
      <td>-32545.000000</td>
      <td>-1.000000</td>
      <td>0.016000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>19440.000000</td>
      <td>2004.000000</td>
      <td>2.004123e+07</td>
      <td>0.000000</td>
      <td>5.635900e+01</td>
      <td>12.943250</td>
      <td>3.565000e+01</td>
      <td>0.017294</td>
      <td>1.304000e+00</td>
      <td>0.000000</td>
      <td>-0.113518</td>
      <td>-0.249708</td>
      <td>0.089300</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>63081.000000</td>
      <td>2009.000000</td>
      <td>2.009123e+07</td>
      <td>0.000000</td>
      <td>4.454530e+02</td>
      <td>115.351500</td>
      <td>2.099847e+02</td>
      <td>0.057857</td>
      <td>5.838950e+01</td>
      <td>0.025824</td>
      <td>0.036737</td>
      <td>0.011213</td>
      <td>0.135200</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>158587.000000</td>
      <td>2014.000000</td>
      <td>2.014123e+07</td>
      <td>0.000000</td>
      <td>2.494132e+03</td>
      <td>689.302000</td>
      <td>1.219183e+03</td>
      <td>0.184653</td>
      <td>6.682430e+02</td>
      <td>0.104159</td>
      <td>0.119577</td>
      <td>0.268387</td>
      <td>0.206400</td>
    </tr>
    <tr>
      <th>max</th>
      <td>330942.000000</td>
      <td>2019.000000</td>
      <td>2.019123e+07</td>
      <td>1.000000</td>
      <td>3.771200e+06</td>
      <td>348703.000000</td>
      <td>1.819782e+06</td>
      <td>1.000000</td>
      <td>3.391920e+06</td>
      <td>7516.000000</td>
      <td>5617.383000</td>
      <td>71014.120000</td>
      <td>2.029400</td>
    </tr>
  </tbody>
</table>
</div>




```python
dffirm.columns
```




    Index(['gvkey', 'year', 'fiscal_year_end_date', 'company_name', 'sp1500_firm',
           'country', 'total_assets', 'book_value_equity', 'market_value_equity',
           'cash_assets', 'total_debt', 'debt_due_next_year', 'profitability',
           'annual_stock_return', 'stock_volatility'],
          dtype='object')



**Step 2(d)**     
Read through firm_fundamentals.txt and retain only the observations that are relevant for the CDS-Bond basis. Specifically:
- Retain only firms that are in the S&P 1500 index.
- Retain only firms with non-missing values of *total_assets*, *total_debt*, and *book_value_equity*. Missing values are expressed as empty strings instead of "NA". **Note: Write a single logical expression that executes this.**
- Retain only firms whose country of operation is the USA and only fiscal years that end after January 1, 2004. **Note: Write a single logical expression that executes this.**


```python
## ENTER YOUR CODE IN THIS BOX
import numpy as np
dffirm = dffirm.drop(dffirm[dffirm.sp1500_firm == 0].index)
dffirm['total_assets'].replace('', np.nan, inplace=True)
dffirm['total_debt'].replace('', np.nan, inplace=True)
dffirm['book_value_equity'].replace('', np.nan, inplace=True)
dffirm.dropna(subset=['total_assets'], inplace=True)
dffirm.dropna(subset=['total_debt'], inplace=True)
dffirm.dropna(subset=['book_value_equity'], inplace=True)
dffirm = dffirm.drop(dffirm[dffirm.country != 'USA'].index)
dffirm = dffirm.drop(dffirm[dffirm.fiscal_year_end_date >= 20040101].index)
dffirm.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gvkey</th>
      <th>year</th>
      <th>fiscal_year_end_date</th>
      <th>company_name</th>
      <th>sp1500_firm</th>
      <th>country</th>
      <th>total_assets</th>
      <th>book_value_equity</th>
      <th>market_value_equity</th>
      <th>cash_assets</th>
      <th>total_debt</th>
      <th>debt_due_next_year</th>
      <th>profitability</th>
      <th>annual_stock_return</th>
      <th>stock_volatility</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>1034</td>
      <td>2000</td>
      <td>20001231</td>
      <td>ALPHARMA INC  -CL A</td>
      <td>1</td>
      <td>USA</td>
      <td>1610.435</td>
      <td>847.887</td>
      <td>1764.3890</td>
      <td>0.045287</td>
      <td>525.121</td>
      <td>0.025677</td>
      <td>0.061621</td>
      <td>NaN</td>
      <td>0.1339</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1034</td>
      <td>2001</td>
      <td>20011231</td>
      <td>ALPHARMA INC  -CL A</td>
      <td>1</td>
      <td>USA</td>
      <td>2390.008</td>
      <td>891.616</td>
      <td>1172.2110</td>
      <td>0.006232</td>
      <td>1060.592</td>
      <td>0.023443</td>
      <td>-0.038887</td>
      <td>-0.397151</td>
      <td>0.1440</td>
    </tr>
    <tr>
      <th>8</th>
      <td>1034</td>
      <td>2002</td>
      <td>20021231</td>
      <td>ALPHARMA INC  -CL A</td>
      <td>1</td>
      <td>USA</td>
      <td>2296.924</td>
      <td>1001.843</td>
      <td>612.7100</td>
      <td>0.010393</td>
      <td>895.858</td>
      <td>0.033603</td>
      <td>-0.080975</td>
      <td>-0.549716</td>
      <td>0.1724</td>
    </tr>
    <tr>
      <th>9</th>
      <td>1034</td>
      <td>2003</td>
      <td>20031231</td>
      <td>ALPHARMA INC  -CL A</td>
      <td>1</td>
      <td>USA</td>
      <td>2329.268</td>
      <td>1131.989</td>
      <td>1045.8830</td>
      <td>0.025168</td>
      <td>817.156</td>
      <td>0.025894</td>
      <td>0.010663</td>
      <td>0.687658</td>
      <td>0.1723</td>
    </tr>
    <tr>
      <th>58</th>
      <td>1075</td>
      <td>2000</td>
      <td>20001231</td>
      <td>PINNACLE WEST CAPITAL CORP</td>
      <td>1</td>
      <td>USA</td>
      <td>7149.151</td>
      <td>2382.714</td>
      <td>4039.7910</td>
      <td>0.001449</td>
      <td>2501.327</td>
      <td>0.141235</td>
      <td>0.081929</td>
      <td>NaN</td>
      <td>0.0780</td>
    </tr>
    <tr>
      <th>59</th>
      <td>1075</td>
      <td>2001</td>
      <td>20011231</td>
      <td>PINNACLE WEST CAPITAL CORP</td>
      <td>1</td>
      <td>USA</td>
      <td>7981.748</td>
      <td>2499.323</td>
      <td>3549.9260</td>
      <td>0.003586</td>
      <td>3204.980</td>
      <td>0.082443</td>
      <td>0.068587</td>
      <td>-0.121260</td>
      <td>0.0821</td>
    </tr>
    <tr>
      <th>60</th>
      <td>1075</td>
      <td>2002</td>
      <td>20021231</td>
      <td>PINNACLE WEST CAPITAL CORP</td>
      <td>1</td>
      <td>USA</td>
      <td>8425.806</td>
      <td>2686.153</td>
      <td>3110.8830</td>
      <td>0.009222</td>
      <td>3264.901</td>
      <td>0.078833</td>
      <td>0.056652</td>
      <td>-0.185424</td>
      <td>0.0909</td>
    </tr>
    <tr>
      <th>61</th>
      <td>1075</td>
      <td>2003</td>
      <td>20031231</td>
      <td>PINNACLE WEST CAPITAL CORP</td>
      <td>1</td>
      <td>USA</td>
      <td>9536.378</td>
      <td>2829.779</td>
      <td>3653.3460</td>
      <td>0.023990</td>
      <td>3409.286</td>
      <td>0.098260</td>
      <td>0.085377</td>
      <td>0.173951</td>
      <td>0.0901</td>
    </tr>
    <tr>
      <th>78</th>
      <td>1076</td>
      <td>2002</td>
      <td>20021231</td>
      <td>AARON'S INC</td>
      <td>1</td>
      <td>USA</td>
      <td>483.648</td>
      <td>280.545</td>
      <td>475.1242</td>
      <td>0.000199</td>
      <td>73.265</td>
      <td>0.001146</td>
      <td>0.042829</td>
      <td>0.342331</td>
      <td>0.1067</td>
    </tr>
    <tr>
      <th>79</th>
      <td>1076</td>
      <td>2003</td>
      <td>20031231</td>
      <td>AARON'S INC</td>
      <td>1</td>
      <td>USA</td>
      <td>555.292</td>
      <td>320.186</td>
      <td>659.7607</td>
      <td>0.000171</td>
      <td>79.570</td>
      <td>0.051368</td>
      <td>0.047504</td>
      <td>0.380028</td>
      <td>0.1018</td>
    </tr>
  </tbody>
</table>
</div>



**Step 2(e)**     
In this step you will modify the firm fundamentals dataset to create variables that are useful for linear regression analysis.
- Create a variable *ME_BE* defined as the market value of equity defined by the book value of equity. Omit outliers with a *ME_BE* ratio above 20.
- Create a variable *leverage_ratio* defined as total debt divided by total assets. Omit outliers with a leverage ratio above 1.
- The distribution of firm size is highly skewed. This means that the market contains a few very large firms, and many medium or small firms. Skewed distributions can affect regression estimates. Researchers account for this by transforming skewed variables using natural logarithms. In Python, this can be done by importing the Numpy module and using the code "ln_X = np.log(X)", where X represents the variable that you want to transform. Use this procedure to create the variables *ln_total_assets*, *ln_total_debt*, and *ln_debt_due_next_year*.
- Omit observations with *profitability* values above 1 or below -1, or  *annual_stock_return* values above 2. **Note: Write a single logical expression that executes this.**

After completing the above, save the modified dataset in a separate output file or Pandas data frame. You can choose to combine steps 2(d) and 2(e) if you like.



```python
## ENTER YOUR CODE IN THIS BOX
dffirm2 = pd.read_csv("firm_fundamentals.txt", error_bad_lines=False,sep="\t")
dffirm2['ME_BE'] = dffirm2.apply(lambda row: row.market_value_equity + row.book_value_equity, axis=1)
dffirm2 = dffirm2.drop(dffirm2[dffirm2.ME_BE > 20].index)
dffirm2['leverage_ratio'] = dffirm2.apply(lambda row: row.total_debt + row.total_assets, axis=1)
dffirm2 = dffirm2.drop(dffirm2[dffirm2.leverage_ratio > 1].index)
dffirm2['ln_total_assets'] = ""
dffirm2['ln_total_assets'] = np.log(dffirm2['total_assets'])
dffirm2['ln_total_debt'] = ""
dffirm2['ln_total_debt'] = np.log(dffirm2['total_debt'])
dffirm2['ln_debt_due_next_year'] = ""
dffirm2['ln_debt_due_next_year'] = np.log(dffirm2['debt_due_next_year'])
dffirm2 = dffirm2.drop(dffirm2[(dffirm2.profitability > 1) & (dffirm2.profitability < -1) & (dffirm2.annual_stock_return > 2)].index)
dffirm2
```

    C:\Users\Rajat Kumar\AppData\Local\Programs\Python\Python38\lib\site-packages\pandas\core\series.py:679: RuntimeWarning: divide by zero encountered in log
      result = getattr(ufunc, method)(*inputs, **kwargs)
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gvkey</th>
      <th>year</th>
      <th>fiscal_year_end_date</th>
      <th>company_name</th>
      <th>sp1500_firm</th>
      <th>country</th>
      <th>total_assets</th>
      <th>book_value_equity</th>
      <th>market_value_equity</th>
      <th>cash_assets</th>
      <th>total_debt</th>
      <th>debt_due_next_year</th>
      <th>profitability</th>
      <th>annual_stock_return</th>
      <th>stock_volatility</th>
      <th>ME_BE</th>
      <th>leverage_ratio</th>
      <th>ln_total_assets</th>
      <th>ln_total_debt</th>
      <th>ln_debt_due_next_year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>120</th>
      <td>1084</td>
      <td>2008</td>
      <td>20081231</td>
      <td>WORLDS INC</td>
      <td>0</td>
      <td>USA</td>
      <td>0.174</td>
      <td>-3.538</td>
      <td>10.477600</td>
      <td>0.0</td>
      <td>0.773</td>
      <td>4.442529</td>
      <td>-8.537635</td>
      <td>0.000000</td>
      <td>NaN</td>
      <td>6.939600</td>
      <td>0.947</td>
      <td>-1.748700</td>
      <td>-0.257476</td>
      <td>1.491224</td>
    </tr>
    <tr>
      <th>121</th>
      <td>1084</td>
      <td>2009</td>
      <td>20091231</td>
      <td>WORLDS INC</td>
      <td>0</td>
      <td>USA</td>
      <td>0.004</td>
      <td>-3.864</td>
      <td>4.829761</td>
      <td>0.0</td>
      <td>0.953</td>
      <td>238.250000</td>
      <td>-7.198020</td>
      <td>-0.550000</td>
      <td>NaN</td>
      <td>0.965761</td>
      <td>0.957</td>
      <td>-5.521461</td>
      <td>-0.048140</td>
      <td>5.473321</td>
    </tr>
    <tr>
      <th>198</th>
      <td>1119</td>
      <td>2000</td>
      <td>20001231</td>
      <td>ADAMS DIVERSIFIED EQUITY FD</td>
      <td>0</td>
      <td>USA</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1662.906000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>199</th>
      <td>1119</td>
      <td>2001</td>
      <td>20011231</td>
      <td>ADAMS DIVERSIFIED EQUITY FD</td>
      <td>0</td>
      <td>USA</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1160.665000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-0.322857</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>200</th>
      <td>1119</td>
      <td>2002</td>
      <td>20021231</td>
      <td>ADAMS DIVERSIFIED EQUITY FD</td>
      <td>0</td>
      <td>USA</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>875.069200</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>-0.256681</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>125833</th>
      <td>327451</td>
      <td>2015</td>
      <td>20151231</td>
      <td>GRINDROD SHIPPING</td>
      <td>0</td>
      <td>SGP</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>125840</th>
      <td>328795</td>
      <td>2013</td>
      <td>20131231</td>
      <td>ARCOSA INC</td>
      <td>0</td>
      <td>USA</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>125841</th>
      <td>328795</td>
      <td>2014</td>
      <td>20141231</td>
      <td>ARCOSA INC</td>
      <td>0</td>
      <td>USA</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>125842</th>
      <td>328795</td>
      <td>2015</td>
      <td>20151231</td>
      <td>ARCOSA INC</td>
      <td>0</td>
      <td>USA</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>125846</th>
      <td>330942</td>
      <td>2019</td>
      <td>20191231</td>
      <td>JPMORGAN BTBULDRS US EQU ETF</td>
      <td>0</td>
      <td>USA</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>63.822000</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>24149 rows × 20 columns</p>
</div>



**Step 2(f)**    
Before estimating any regressions, you must combine the firm fundamental dataset (created in Step 2(e)) with the CDS-Bond basis dataset (created in Step 2(b)). For each firm *F*, you should merge all CDS-Bond basis deviations from year *T* with the firm's fundamentals from year *T-1*. Describe in words each step of this merging process. Make sure to explain what variables uniquely identify the observations in each dataset, and whether each observation in the firm fundamental dataset is matched to a single or multiple observations in the CDS-Bond basis dataset. 

<div class="alert alert-block alert-warning">
Click on this box and type your answer. It is not necessary to fill out any tables.
</div>



**Step 2(g)**    
Now executive the merge described in Step 2(f). Make sure to look over the merged dataset to ensure that observations are matched correctly. 


```python
## ENTER YOUR CODE IN THIS BOX
df10=pd.concat([df2,dffirm2])
df10
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gvkey</th>
      <th>date</th>
      <th>basis_deviation</th>
      <th>pre_crisis_period</th>
      <th>negative_deviation</th>
      <th>year</th>
      <th>fiscal_year_end_date</th>
      <th>company_name</th>
      <th>sp1500_firm</th>
      <th>country</th>
      <th>...</th>
      <th>total_debt</th>
      <th>debt_due_next_year</th>
      <th>profitability</th>
      <th>annual_stock_return</th>
      <th>stock_volatility</th>
      <th>ME_BE</th>
      <th>leverage_ratio</th>
      <th>ln_total_assets</th>
      <th>ln_total_debt</th>
      <th>ln_debt_due_next_year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016</th>
      <td>7435</td>
      <td>20110926.0</td>
      <td>14.15</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>7435</td>
      <td>20110927.0</td>
      <td>26.82</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>7435</td>
      <td>20110928.0</td>
      <td>33.12</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>7435</td>
      <td>20110929.0</td>
      <td>33.48</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>7435</td>
      <td>20110930.0</td>
      <td>37.05</td>
      <td>0</td>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>125833</th>
      <td>327451</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2015.0</td>
      <td>20151231.0</td>
      <td>GRINDROD SHIPPING</td>
      <td>0.0</td>
      <td>SGP</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>125840</th>
      <td>328795</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2013.0</td>
      <td>20131231.0</td>
      <td>ARCOSA INC</td>
      <td>0.0</td>
      <td>USA</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>125841</th>
      <td>328795</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2014.0</td>
      <td>20141231.0</td>
      <td>ARCOSA INC</td>
      <td>0.0</td>
      <td>USA</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>125842</th>
      <td>328795</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2015.0</td>
      <td>20151231.0</td>
      <td>ARCOSA INC</td>
      <td>0.0</td>
      <td>USA</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>125846</th>
      <td>330942</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2019.0</td>
      <td>20191231.0</td>
      <td>JPMORGAN BTBULDRS US EQU ETF</td>
      <td>0.0</td>
      <td>USA</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>81819 rows × 24 columns</p>
</div>



**Step 2(h)**    
Calculate summary statistics (number of observations, mean, median, and standard deviation) for the following variables, separately for the pre-crisis and post-crisis periods: *basis_deviation*, *ln_total_assets*, *ME_BE*, *leverage_ratio*, *profitability*, *annual_stock_return*, and *stock_volatility*. Report the statistics in the table below.


```python
## ENTER YOUR CODE IN THIS BOX
df2.describe().T
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>gvkey</th>
      <td>57670.0</td>
      <td>2.215677e+04</td>
      <td>39000.991809</td>
      <td>1045.0</td>
      <td>5.046000e+03</td>
      <td>8530.00</td>
      <td>14477.00</td>
      <td>176760.0</td>
    </tr>
    <tr>
      <th>date</th>
      <td>57670.0</td>
      <td>2.011758e+07</td>
      <td>26057.957569</td>
      <td>20060623.0</td>
      <td>2.009090e+07</td>
      <td>20120703.00</td>
      <td>20140902.00</td>
      <td>20151231.0</td>
    </tr>
    <tr>
      <th>basis_deviation</th>
      <td>57670.0</td>
      <td>-9.891590e+00</td>
      <td>108.908968</td>
      <td>-542.2</td>
      <td>-4.556750e+01</td>
      <td>-3.83</td>
      <td>23.26</td>
      <td>1591.9</td>
    </tr>
  </tbody>
</table>
</div>




```python
dffirm2.describe().T
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>gvkey</th>
      <td>24149.0</td>
      <td>1.142669e+05</td>
      <td>74470.828938</td>
      <td>1.084000e+03</td>
      <td>2.704100e+04</td>
      <td>1.605970e+05</td>
      <td>1.789180e+05</td>
      <td>3.309420e+05</td>
    </tr>
    <tr>
      <th>year</th>
      <td>24149.0</td>
      <td>2.011989e+03</td>
      <td>5.427818</td>
      <td>2.000000e+03</td>
      <td>2.008000e+03</td>
      <td>2.013000e+03</td>
      <td>2.017000e+03</td>
      <td>2.019000e+03</td>
    </tr>
    <tr>
      <th>fiscal_year_end_date</th>
      <td>24149.0</td>
      <td>2.012113e+07</td>
      <td>54278.182739</td>
      <td>2.000123e+07</td>
      <td>2.008123e+07</td>
      <td>2.013123e+07</td>
      <td>2.017123e+07</td>
      <td>2.019123e+07</td>
    </tr>
    <tr>
      <th>sp1500_firm</th>
      <td>24149.0</td>
      <td>1.780612e-03</td>
      <td>0.042161</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>1.000000e+00</td>
    </tr>
    <tr>
      <th>total_assets</th>
      <td>2973.0</td>
      <td>7.217349e+01</td>
      <td>1217.987453</td>
      <td>0.000000e+00</td>
      <td>1.000000e-02</td>
      <td>1.170000e-01</td>
      <td>3.830000e-01</td>
      <td>4.255705e+04</td>
    </tr>
    <tr>
      <th>book_value_equity</th>
      <td>2964.0</td>
      <td>4.464435e+00</td>
      <td>421.824366</td>
      <td>-8.836697e+03</td>
      <td>-8.270000e-01</td>
      <td>-1.835000e-01</td>
      <td>1.300000e-02</td>
      <td>1.570300e+04</td>
    </tr>
    <tr>
      <th>market_value_equity</th>
      <td>20035.0</td>
      <td>6.048795e+02</td>
      <td>3282.454434</td>
      <td>0.000000e+00</td>
      <td>9.868640e+00</td>
      <td>6.177613e+01</td>
      <td>2.726449e+02</td>
      <td>1.378832e+05</td>
    </tr>
    <tr>
      <th>cash_assets</th>
      <td>2559.0</td>
      <td>3.596731e-01</td>
      <td>0.374192</td>
      <td>-1.686747e-01</td>
      <td>2.457810e-02</td>
      <td>1.965318e-01</td>
      <td>6.871538e-01</td>
      <td>1.000000e+00</td>
    </tr>
    <tr>
      <th>total_debt</th>
      <td>2906.0</td>
      <td>1.385375e-01</td>
      <td>0.203973</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>3.200000e-02</td>
      <td>2.080000e-01</td>
      <td>9.680000e-01</td>
    </tr>
    <tr>
      <th>debt_due_next_year</th>
      <td>2524.0</td>
      <td>8.494671e+00</td>
      <td>49.508951</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>8.358280e-02</td>
      <td>1.391787e+00</td>
      <td>9.669999e+02</td>
    </tr>
    <tr>
      <th>profitability</th>
      <td>1286.0</td>
      <td>-1.105863e+02</td>
      <td>1113.420699</td>
      <td>-3.254500e+04</td>
      <td>-1.186133e+01</td>
      <td>-1.991979e+00</td>
      <td>-2.301677e-01</td>
      <td>1.024000e+03</td>
    </tr>
    <tr>
      <th>annual_stock_return</th>
      <td>16973.0</td>
      <td>1.192128e+00</td>
      <td>99.820318</td>
      <td>-9.999281e-01</td>
      <td>-1.414320e-01</td>
      <td>2.569700e-03</td>
      <td>1.551821e-01</td>
      <td>1.249900e+04</td>
    </tr>
    <tr>
      <th>stock_volatility</th>
      <td>222.0</td>
      <td>2.801072e-01</td>
      <td>0.162854</td>
      <td>1.730000e-02</td>
      <td>1.707250e-01</td>
      <td>2.555000e-01</td>
      <td>3.493000e-01</td>
      <td>9.522000e-01</td>
    </tr>
    <tr>
      <th>ME_BE</th>
      <td>2369.0</td>
      <td>1.449315e+00</td>
      <td>38.902018</td>
      <td>-1.154825e+03</td>
      <td>7.625000e-02</td>
      <td>1.219560e+00</td>
      <td>4.280280e+00</td>
      <td>1.997160e+01</td>
    </tr>
    <tr>
      <th>leverage_ratio</th>
      <td>2906.0</td>
      <td>3.539126e-01</td>
      <td>0.301138</td>
      <td>0.000000e+00</td>
      <td>7.000000e-02</td>
      <td>2.950000e-01</td>
      <td>5.895000e-01</td>
      <td>1.000000e+00</td>
    </tr>
    <tr>
      <th>ln_total_assets</th>
      <td>2973.0</td>
      <td>-inf</td>
      <td>NaN</td>
      <td>-inf</td>
      <td>-4.605170e+00</td>
      <td>-2.145581e+00</td>
      <td>-9.597203e-01</td>
      <td>1.065860e+01</td>
    </tr>
    <tr>
      <th>ln_total_debt</th>
      <td>2906.0</td>
      <td>-inf</td>
      <td>NaN</td>
      <td>-inf</td>
      <td>-inf</td>
      <td>-3.442019e+00</td>
      <td>-1.570217e+00</td>
      <td>-3.252319e-02</td>
    </tr>
    <tr>
      <th>ln_debt_due_next_year</th>
      <td>2524.0</td>
      <td>-inf</td>
      <td>NaN</td>
      <td>-inf</td>
      <td>-inf</td>
      <td>-2.481922e+00</td>
      <td>3.305879e-01</td>
      <td>6.874198e+00</td>
    </tr>
  </tbody>
</table>
</div>




```python
df10.describe().T
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>mean</th>
      <th>std</th>
      <th>min</th>
      <th>25%</th>
      <th>50%</th>
      <th>75%</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>gvkey</th>
      <td>81819.0</td>
      <td>4.934320e+04</td>
      <td>66888.128157</td>
      <td>1.045000e+03</td>
      <td>6.502000e+03</td>
      <td>1.214200e+04</td>
      <td>6.237400e+04</td>
      <td>3.309420e+05</td>
    </tr>
    <tr>
      <th>date</th>
      <td>57670.0</td>
      <td>2.011758e+07</td>
      <td>26057.957569</td>
      <td>2.006062e+07</td>
      <td>2.009090e+07</td>
      <td>2.012070e+07</td>
      <td>2.014090e+07</td>
      <td>2.015123e+07</td>
    </tr>
    <tr>
      <th>basis_deviation</th>
      <td>57670.0</td>
      <td>-9.891590e+00</td>
      <td>108.908968</td>
      <td>-5.422000e+02</td>
      <td>-4.556750e+01</td>
      <td>-3.830000e+00</td>
      <td>2.326000e+01</td>
      <td>1.591900e+03</td>
    </tr>
    <tr>
      <th>year</th>
      <td>24149.0</td>
      <td>2.011989e+03</td>
      <td>5.427818</td>
      <td>2.000000e+03</td>
      <td>2.008000e+03</td>
      <td>2.013000e+03</td>
      <td>2.017000e+03</td>
      <td>2.019000e+03</td>
    </tr>
    <tr>
      <th>fiscal_year_end_date</th>
      <td>24149.0</td>
      <td>2.012113e+07</td>
      <td>54278.182739</td>
      <td>2.000123e+07</td>
      <td>2.008123e+07</td>
      <td>2.013123e+07</td>
      <td>2.017123e+07</td>
      <td>2.019123e+07</td>
    </tr>
    <tr>
      <th>sp1500_firm</th>
      <td>24149.0</td>
      <td>1.780612e-03</td>
      <td>0.042161</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>1.000000e+00</td>
    </tr>
    <tr>
      <th>total_assets</th>
      <td>2973.0</td>
      <td>7.217349e+01</td>
      <td>1217.987453</td>
      <td>0.000000e+00</td>
      <td>1.000000e-02</td>
      <td>1.170000e-01</td>
      <td>3.830000e-01</td>
      <td>4.255705e+04</td>
    </tr>
    <tr>
      <th>book_value_equity</th>
      <td>2964.0</td>
      <td>4.464435e+00</td>
      <td>421.824366</td>
      <td>-8.836697e+03</td>
      <td>-8.270000e-01</td>
      <td>-1.835000e-01</td>
      <td>1.300000e-02</td>
      <td>1.570300e+04</td>
    </tr>
    <tr>
      <th>market_value_equity</th>
      <td>20035.0</td>
      <td>6.048795e+02</td>
      <td>3282.454434</td>
      <td>0.000000e+00</td>
      <td>9.868640e+00</td>
      <td>6.177613e+01</td>
      <td>2.726449e+02</td>
      <td>1.378832e+05</td>
    </tr>
    <tr>
      <th>cash_assets</th>
      <td>2559.0</td>
      <td>3.596731e-01</td>
      <td>0.374192</td>
      <td>-1.686747e-01</td>
      <td>2.457810e-02</td>
      <td>1.965318e-01</td>
      <td>6.871538e-01</td>
      <td>1.000000e+00</td>
    </tr>
    <tr>
      <th>total_debt</th>
      <td>2906.0</td>
      <td>1.385375e-01</td>
      <td>0.203973</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>3.200000e-02</td>
      <td>2.080000e-01</td>
      <td>9.680000e-01</td>
    </tr>
    <tr>
      <th>debt_due_next_year</th>
      <td>2524.0</td>
      <td>8.494671e+00</td>
      <td>49.508951</td>
      <td>0.000000e+00</td>
      <td>0.000000e+00</td>
      <td>8.358280e-02</td>
      <td>1.391787e+00</td>
      <td>9.669999e+02</td>
    </tr>
    <tr>
      <th>profitability</th>
      <td>1286.0</td>
      <td>-1.105863e+02</td>
      <td>1113.420699</td>
      <td>-3.254500e+04</td>
      <td>-1.186133e+01</td>
      <td>-1.991979e+00</td>
      <td>-2.301677e-01</td>
      <td>1.024000e+03</td>
    </tr>
    <tr>
      <th>annual_stock_return</th>
      <td>16973.0</td>
      <td>1.192128e+00</td>
      <td>99.820318</td>
      <td>-9.999281e-01</td>
      <td>-1.414320e-01</td>
      <td>2.569700e-03</td>
      <td>1.551821e-01</td>
      <td>1.249900e+04</td>
    </tr>
    <tr>
      <th>stock_volatility</th>
      <td>222.0</td>
      <td>2.801072e-01</td>
      <td>0.162854</td>
      <td>1.730000e-02</td>
      <td>1.707250e-01</td>
      <td>2.555000e-01</td>
      <td>3.493000e-01</td>
      <td>9.522000e-01</td>
    </tr>
    <tr>
      <th>ME_BE</th>
      <td>2369.0</td>
      <td>1.449315e+00</td>
      <td>38.902018</td>
      <td>-1.154825e+03</td>
      <td>7.625000e-02</td>
      <td>1.219560e+00</td>
      <td>4.280280e+00</td>
      <td>1.997160e+01</td>
    </tr>
    <tr>
      <th>leverage_ratio</th>
      <td>2906.0</td>
      <td>3.539126e-01</td>
      <td>0.301138</td>
      <td>0.000000e+00</td>
      <td>7.000000e-02</td>
      <td>2.950000e-01</td>
      <td>5.895000e-01</td>
      <td>1.000000e+00</td>
    </tr>
    <tr>
      <th>ln_total_assets</th>
      <td>2973.0</td>
      <td>-inf</td>
      <td>NaN</td>
      <td>-inf</td>
      <td>-4.605170e+00</td>
      <td>-2.145581e+00</td>
      <td>-9.597203e-01</td>
      <td>1.065860e+01</td>
    </tr>
    <tr>
      <th>ln_total_debt</th>
      <td>2906.0</td>
      <td>-inf</td>
      <td>NaN</td>
      <td>-inf</td>
      <td>-inf</td>
      <td>-3.442019e+00</td>
      <td>-1.570217e+00</td>
      <td>-3.252319e-02</td>
    </tr>
    <tr>
      <th>ln_debt_due_next_year</th>
      <td>2524.0</td>
      <td>-inf</td>
      <td>NaN</td>
      <td>-inf</td>
      <td>-inf</td>
      <td>-2.481922e+00</td>
      <td>3.305879e-01</td>
      <td>6.874198e+00</td>
    </tr>
  </tbody>
</table>
</div>



<div class="alert alert-block alert-warning">
Answer by double clicking this window and filling out the table below.
    <br><br>
    
**Pre-Crisis Summary Statistics**    
    
|                        | No. of <br> Observations     | Mean   | Median | Standard <br> Deviation| 
|:-----------------------|:----------------------------:|:------:|:------:|:----------------------:|
|*basis_deviation*       |[answer]                      |[answer]|[answer]|[answer]                |
|*negative_basis*        |[answer]                      |[answer]|[answer]|[answer]                |
|*ln_total_assets*       |[answer]                      |[answer]|[answer]|[answer]                |
|*ME_BE*                 |[answer]                      |[answer]|[answer]|[answer]                |
|*leverage_ratio*        |[answer]                      |[answer]|[answer]|[answer]                |
|*profitability*         |[answer]                      |[answer]|[answer]|[answer]                |
|*annual_stock_return*   |[answer]                      |[answer]|[answer]|[answer]                |
|*stock_volatility*      |[answer]                      |[answer]|[answer]|[answer]                |

**Post-Crisis Summary Statistics**
    
|                        | No. of <br> Observations     | Mean   | Median | Standard <br> Deviation| 
|:-----------------------|:----------------------------:|:------:|:------:|:----------------------:|
|*basis_deviation*       |[answer]                      |[answer]|[answer]|[answer]                |
|*negative_basis*        |[answer]                      |[answer]|[answer]|[answer]                |
|*ln_total_assets*       |[answer]                      |[answer]|[answer]|[answer]                |
|*ME_BE*                 |[answer]                      |[answer]|[answer]|[answer]                |
|*leverage_ratio*        |[answer]                      |[answer]|[answer]|[answer]                |
|*profitability*         |[answer]                      |[answer]|[answer]|[answer]                |
|*annual_stock_return*   |[answer]                      |[answer]|[answer]|[answer]                |
|*stock_volatility*      |[answer]                      |[answer]|[answer]|[answer]                |
    
</div>

**Step 2(i)**    
Estimate four regression models testing determinants of the CDS-Bond basis deviations. The dependent variable in each model should be *basis_deviation*. The explanatory variables should be:
- *ln_total_assets* and *leverage_ratio* in Model 1
- *ln_total_assets*, *leverage_ratio*, *annual_stock_return*, and *stock_volatility* in Model 2
- *ln_total_assets*, *leverage_ratio*, *ME_BE*, and *profitability* in Model 3 
- *ln_total_assets* and three other variables of your choice in Model 4 (at least one of the additional variables should not be in models 1 through 3)

Estimate each of the regression models separately for the pre- and post-crisis period (so eight regressions total). Report the regression coefficients and p-values in the table below. Also briefly interpret the results in one paragraph.

**Note:** If you have limited background with regressions, you should read the document "Introduction to Regression" listed under the Canvas module "Reference Materials".

<div class="alert alert-block alert-warning">
Answer by double clicking this window and filling out the tables below.
    <br><br>
    
**Table 1. Regression estimates from pre-crisis period**   
    
|                       | Model 1 <br> Coefficient | Model 1 <br> P-value | Model 2 <br> Coefficient | Model 2 <br> P-value | Model 3 <br> Coefficient | Model 3 <br> P-value | Model 4 <br> Coefficient | Model 4 <br> P-value |
|:----------------------|:------------------------:|:--------------------:|:------------------------:|:----------------------:|:------------------------:|:--------------------:|:------------------------:|:--------------------:| 
|*ln_total_assets*      |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | 
|*leverage_ratio*       |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | 
|*annual_stock_return*  |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | 
|*stock_volatility*     |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              |
|*ME_BE*                |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | |*profitability*        |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]                
|Additional Variable 1  |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | 
|Additional Variable 2  |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | 
|Additional Variable 3  |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | 
<br> 
    
**Table 2. Regression estimates from post-crisis period**   
    
    
|                       | Model 1 <br> Coefficient | Model 1 <br> P-value | Model 2 <br> Coefficient | Model 2 <br> P-value | Model 3 <br> Coefficient | Model 3 <br> P-value | Model 4 <br> Coefficient | Model 4 <br> P-value |
|:----------------------|:------------------------:|:--------------------:|:------------------------:|:----------------------:|:------------------------:|:--------------------:|:------------------------:|:--------------------:| 
|*ln_total_assets*      |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | 
|*leverage_ratio*       |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | 
|*annual_stock_return*  |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | 
|*stock_volatility*     |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              |
|*ME_BE*                |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | |*profitability*        |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]                
|Additional Variable 1  |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | 
|Additional Variable 2  |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | 
|Additional Variable 3  |[answer]                  |[answer]              |[answer]                  |[answer]                 |[answer]                  |[answer]              |[answer]                  |[answer]              | 
    
</div>

**Challenge Exercises**    
The below exercises are intended for students who wish to earn a grade above 8,0 on the resit assignment. They should be possible to complete with the computing knowledge and syntax taught in the course. Students who do not attempt the challenge exercises can receive a grade of up to 8,0 on their resit assignment. Students who make a serious attempt at the challenge exercises will receive a bonus of up to 2,0 points on the resit assignment. The bonus is awarded even to students who do not correctly complete all of the main steps of the resit assignment. However, no bonus is awarded to students who do not attempt at least half of the main steps.

For this exercise, you need the file "crsp_data_hw5.txt" with data from CRSP, that was also used in Homework 4 and the Final Assignment.  

The exercises are:
- Read in the file crsp_data_hw5.txt. For each firm *F* and year *T*, calculate the average of *trading_volume* (measured as the number of shares traded in each month, in millions on shares). Then for each firm *F*, match the average trading volume from year *T* with the value of shares_outstanding from year *T-1* (the latter variable is in firm_fundamentals.txt). Create the variable *scaled_trading_volume* which equals the ratio of average trading volume/shares outstanding.
- Merge *scaled_trading_volume* into the data frame created in Step 2(g). Then estimate a new regression Model 5 with *ln_total_assets*, *leverage_ratio*, *annual_stock_return*, *stock_volatility*, and *scaled_trading_volume*, separately before and after the financial crisis. Explain why you may expect *scaled_trading_volume* to be related to deviations to the CDS-Bond basis. Also interpret the coefficients on *scaled_trading_volume*.



```python
## ENTER YOUR CODE FOR CHALLENGE EXERCISE 1 IN THIS BOX
```


```python
## ENTER YOUR CODE FOR CHALLENGE EXERCISE 2 IN THIS BOX
```

<div class="alert alert-block alert-warning">
Answer by double clicking this window and filling out the tables below.
    <br><br>
    
**Table 1. Regression estimates from additional regression model**   
    
|                       | Model 5 Coefficient, <br> Pre-crisis Period |  Model 5 P-value, <br> Pre-crisis Period | Model 5 Coefficient, <br> Post-crisis Period |  Model 5 P-value, <br> Post-crisis Period |
|:----------------------|:-------------------------------------------:|:----------------------------------------:|:-------------------------------------------:|:-----------------------------------------:|
|*ln_total_assets*      |[answer]                                     |[answer]                                  | [answer]                                     |[answer]                                   |
|*annual_stock_return*  |[answer]                                     |[answer]                                  | [answer]                                     |[answer]                                   |
|*stock_volatility*     |[answer]                                     |[answer]                                  | [answer]                                     |[answer]                                   |
|*scaled_trading_volume*|[answer]                                     |[answer]                                  | [answer]                                     |[answer]                                   |    
     
</div>

# Part 3. Creating and Testing a CDS-Bond Basis Factor 

**Objective:** Some quantitative trading firms seek to make a profit by identifying firms with mispriced stocks. There are many different variables that could be related to stock mispricing, so quant firms may construct a wide variety of factors when looking for profitable trading strategies. In this section, you will attempt to replicate this procedure by constructing your own risk factor based on deviations from the CDS-Bond basis.

In the steps below, you need to combine data on the CDS-Bond basis with stock prices from the CRSP database. You then need to construct portfolios based on CDS-Bond basis deviations and calculate the stock returns for each portfolios.


**Getting Started**: The output file from Step #4 of Homework 2 is necessary to complete this part of the assignment. Throughout these instructions, I will refer to this file as “HW2_output.txt”. You also need the file "crsp_data_hw5.txt" with data from CRSP, that was also used in Homework 4 and the Final Assignment.  

**To Complete:**
- Fill in all code boxes
- Fill in all tables and answer boxes below.
- Submit the output file created in step 3(d). 


**Note on writing code:** To streamline the grading process, please use Pandas throughout this part of the assignment. 

**Step 3(a)**     
Start with the file HW2_output.txt that you used in Step 2(a). Read in the file using Pandas. Keep only observations from January 1, 2007 or later, and omit any observations with a missing value for *basis_deviation*. For each firm *F* and year *T* in the file, calculate the average value of *basis_deviation* and store it in a column called *avg_basis_deviation*. Create a new data frame with just the variables *gvkey*, *year*, *avg_basis_deviation*. Make sure observations are unique at the firm and year level.


```python
# ENTER YOUR CODE IN THIS BOX
import pandas as pd
df3 = pd.read_csv("HW2_output.txt", error_bad_lines=False,sep="\t")
df3 = df3.drop(df3[df3.date < 20070101].index)
df3.dropna(subset = ["basis_deviation"], inplace=True)
df3['avg_basis_deviation']=""
df3['avg_basis_deviation']=df3['basis_deviation']
df4 = df3[['gvkey', 'date', 'avg_basis_deviation']].copy()
df4
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gvkey</th>
      <th>date</th>
      <th>avg_basis_deviation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2016</th>
      <td>7435</td>
      <td>20110926</td>
      <td>14.15</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>7435</td>
      <td>20110927</td>
      <td>26.82</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>7435</td>
      <td>20110928</td>
      <td>33.12</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>7435</td>
      <td>20110929</td>
      <td>33.48</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>7435</td>
      <td>20110930</td>
      <td>37.05</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1374077</th>
      <td>65417</td>
      <td>20160926</td>
      <td>-111.91</td>
    </tr>
    <tr>
      <th>1374078</th>
      <td>65417</td>
      <td>20160927</td>
      <td>-82.92</td>
    </tr>
    <tr>
      <th>1374079</th>
      <td>65417</td>
      <td>20160928</td>
      <td>-91.64</td>
    </tr>
    <tr>
      <th>1374098</th>
      <td>65417</td>
      <td>20161025</td>
      <td>-102.30</td>
    </tr>
    <tr>
      <th>1374100</th>
      <td>65417</td>
      <td>20161027</td>
      <td>-84.58</td>
    </tr>
  </tbody>
</table>
<p>63435 rows × 3 columns</p>
</div>



**Step 3(b)**    
Using the data frame from Step 3(a), in each year form 5 portfolios based on *avg_basis_deviation*. Specifically, in each year calculate the quintiles of *avg_basis_deviation* across all individual firms in the data frame in that year. Then in each year, sort firms into portfolios numbered 1 (lowest value of *avg_basis_deviation*) through 5 (highest value of *avg_basis_deviation*). Your data frame should now have the variables *gvkey*, *year*, *avg_basis_deviation*, and a portfolio identifier with values between 1 and 5.


```python
## ENTER YOUR CODE IN THIS BOX
df4['portfolio']= pd.qcut(df4['avg_basis_deviation'],  q = 5, labels = False) 
df4.sort_values(by =['avg_basis_deviation'], inplace = True)
df4
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>gvkey</th>
      <th>date</th>
      <th>avg_basis_deviation</th>
      <th>portfolio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1067438</th>
      <td>8272</td>
      <td>20081224</td>
      <td>-542.20</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1062820</th>
      <td>9555</td>
      <td>20130501</td>
      <td>-511.46</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1028457</th>
      <td>9155</td>
      <td>20150716</td>
      <td>-508.81</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1097051</th>
      <td>15521</td>
      <td>20090212</td>
      <td>-493.67</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1067413</th>
      <td>8272</td>
      <td>20081119</td>
      <td>-474.95</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>36913</th>
      <td>1045</td>
      <td>20131217</td>
      <td>5028.70</td>
      <td>4</td>
    </tr>
    <tr>
      <th>36914</th>
      <td>1045</td>
      <td>20131218</td>
      <td>5029.76</td>
      <td>4</td>
    </tr>
    <tr>
      <th>36916</th>
      <td>1045</td>
      <td>20131220</td>
      <td>5033.66</td>
      <td>4</td>
    </tr>
    <tr>
      <th>36925</th>
      <td>1045</td>
      <td>20140102</td>
      <td>5046.54</td>
      <td>4</td>
    </tr>
    <tr>
      <th>36921</th>
      <td>1045</td>
      <td>20131227</td>
      <td>5047.93</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
<p>63435 rows × 4 columns</p>
</div>



**Step 3(c)**    
Now read in the file crsp_data_hw5.txt, which contains monthly stock returns for a large sample of U.S. firms. For each firm *F* and year *T*, merge in the portfolio identifier from year *T-1* that you created in Step 3(b). During the merge you should keep only firms that are in both the CDS-Bond basis dataset and the CRSP dataset.


```python
## ENTER YOUR CODE IN THIS BOX
dfcrsp = pd.read_csv("crsp_data_hw5.txt", error_bad_lines=False,sep="\t")
pd.merge(dfcrsp, df4, on='gvkey', how='inner')
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date_x</th>
      <th>sic_industry_code</th>
      <th>comnam</th>
      <th>stock_price</th>
      <th>trading_volume</th>
      <th>stock_return</th>
      <th>bid_price</th>
      <th>ask_price</th>
      <th>gvkey</th>
      <th>year</th>
      <th>date_y</th>
      <th>avg_basis_deviation</th>
      <th>portfolio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>19630131.0</td>
      <td>4511.0</td>
      <td>AMERICAN AIRLS INC</td>
      <td>2.210165</td>
      <td>7988.0</td>
      <td>0.102041</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1045</td>
      <td>1963</td>
      <td>20101020</td>
      <td>488.17</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>19630131.0</td>
      <td>4511.0</td>
      <td>AMERICAN AIRLS INC</td>
      <td>2.210165</td>
      <td>7988.0</td>
      <td>0.102041</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1045</td>
      <td>1963</td>
      <td>20100922</td>
      <td>546.49</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>19630131.0</td>
      <td>4511.0</td>
      <td>AMERICAN AIRLS INC</td>
      <td>2.210165</td>
      <td>7988.0</td>
      <td>0.102041</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1045</td>
      <td>1963</td>
      <td>20101014</td>
      <td>554.44</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>19630131.0</td>
      <td>4511.0</td>
      <td>AMERICAN AIRLS INC</td>
      <td>2.210165</td>
      <td>7988.0</td>
      <td>0.102041</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1045</td>
      <td>1963</td>
      <td>20101013</td>
      <td>562.65</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>19630131.0</td>
      <td>4511.0</td>
      <td>AMERICAN AIRLS INC</td>
      <td>2.210165</td>
      <td>7988.0</td>
      <td>0.102041</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1045</td>
      <td>1963</td>
      <td>20101015</td>
      <td>570.88</td>
      <td>4</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>29287944</th>
      <td>20151231.0</td>
      <td>2621.0</td>
      <td>DOMTAR CORP</td>
      <td>36.950000</td>
      <td>110177.0</td>
      <td>-0.091020</td>
      <td>36.95</td>
      <td>36.96</td>
      <td>176760</td>
      <td>2015</td>
      <td>20090924</td>
      <td>-91.95</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29287945</th>
      <td>20151231.0</td>
      <td>2621.0</td>
      <td>DOMTAR CORP</td>
      <td>36.950000</td>
      <td>110177.0</td>
      <td>-0.091020</td>
      <td>36.95</td>
      <td>36.96</td>
      <td>176760</td>
      <td>2015</td>
      <td>20090923</td>
      <td>-88.98</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29287946</th>
      <td>20151231.0</td>
      <td>2621.0</td>
      <td>DOMTAR CORP</td>
      <td>36.950000</td>
      <td>110177.0</td>
      <td>-0.091020</td>
      <td>36.95</td>
      <td>36.96</td>
      <td>176760</td>
      <td>2015</td>
      <td>20090928</td>
      <td>-86.36</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29287947</th>
      <td>20151231.0</td>
      <td>2621.0</td>
      <td>DOMTAR CORP</td>
      <td>36.950000</td>
      <td>110177.0</td>
      <td>-0.091020</td>
      <td>36.95</td>
      <td>36.96</td>
      <td>176760</td>
      <td>2015</td>
      <td>20091005</td>
      <td>-75.29</td>
      <td>0</td>
    </tr>
    <tr>
      <th>29287948</th>
      <td>20151231.0</td>
      <td>2621.0</td>
      <td>DOMTAR CORP</td>
      <td>36.950000</td>
      <td>110177.0</td>
      <td>-0.091020</td>
      <td>36.95</td>
      <td>36.96</td>
      <td>176760</td>
      <td>2015</td>
      <td>20090916</td>
      <td>-47.87</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>29287949 rows × 13 columns</p>
</div>



**Step 3(d)**    
For each month in the data frame from Step 3(c), calculate the equal-weighted average stock return across firms in each of the 5 portfolios. Also, for each month calculate the difference between the return of portfolio 5 and portfolio 1, i.e., the monthly value of (equal-weighted average stock return across firms in Portfolio 5) - (equal-weighted average stock return across firms in Portfolio 1). Label this difference in portfolio returns as the *basis_factor*. Briefly explain what this variable represents.

At the end of this step, your data frame should contain a variable for the month, 5 variables for the average monthly returns of each portfolio, and the variable *basis_factor*. It should have unique observations for each month. Store this data frame in an output file.


```python
## ENTER YOUR CODE IN THIS BOX
```

<div class="alert alert-block alert-warning">
Click on this box and type your answer about what basis_factor represents.
</div>



**Step 3(e)**    
Calculate the average value of *basis_factor* across all months before September 2008. Also calculate the average value of *basis_factor* across all months after March 2009. Report the averages below.


```python
## ENTER YOUR CODE IN THIS BOX
df3d1=df3d['date'][df['date'] < 20080901].mean()
df3d2=df3d['date'][df['date'] < 20090331].mean()
```

<div class="alert alert-block alert-warning">
Click on this box and type your answer about the averages of basis_factor.
</div>



**Challenge Exercises**    
The below exercises are intended for students who wish to earn a grade above 8,0 on the resit assignment. They should be possible to complete with the computing knowledge and syntax taught in the course. Students who do not attempt the challenge exercises can receive a grade of up to 8,0 on their resit assignment. Students who make a serious attempt at the challenge exercises will receive a bonus of up to 2,0 points on the resit assignment. The bonus is awarded even to students who do not correctly complete all of the main steps of the resit assignment. However, no bonus is awarded to students who do not attempt at least half of the main steps.

For these exercises, you need the monthly factor data in the file "factor_data.txt".

The exercises are:
- Combine the data frame from Step 3(d) with monthly data on the SMB and HML factors, the monthly equity risk premium ERP, and the risk-free rate. Also, subtract the risk-free rate from each of the 5 monthly portfolio returns (i.e., the portfolios based on quintiles of the CDS-Bond basis).
- Estimate a 4-factor model separately for each of the 5 portfolios using an OLS regression. Use the monthly portfolio returns as the dependent variable. The explanatory variables should include the standard Fama-French factors along with *basis_factor*. For each portfolio estimate the regressions using all months in the dataset.  Record the factor loadings, alpha, associated p-values, and R<sup>2</sup> in a table. 
- Explain why a quant trading firm may want to test a stock return factor model that includes a risk factor based on deviations from the CDS-Bond basis. Also interpret the results from tables 1 and 2 statistically and discuss what they mean about the *basis_factor* in 1-2 paragraphs. 


```python
## ENTER YOUR CODE FOR CHALLENGE EXERCISE 1 IN THIS BOX
```


```python
## ENTER YOUR CODE FOR CHALLENGE EXERCISE 2 IN THIS BOX
```

<div class="alert alert-block alert-warning">
Answer by double clicking this window and filling out the tables below. Write your interpretation of the regression models under Table 2 in this box.
    <br><br>
    
**Table 1. Factor loadings and p-values**    
    
|             | Portfolio 1 <br> Loading | Portfolio 1 <br> P-value | Portfolio 2 <br> Loading | Portfolio 2 <br> P-value | Portfolio 3 <br> Loading | Portfolio 3 <br> P-value | Portfolio 4 <br> Loading | Portfolio 4 <br> P-value | Portfolio 5 <br> Loading | Portfolio 5 <br> P-value |
|:------------|:------------------------:|:------------------------:|:------------------------:|:------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:---------------------------:|:-------------------------:|:-------------------------:|
|SMB          |[answer]                  |[answer]                  |[answer]                  |[answer]                   |[answer]                   |[answer]                   |[answer]                   |[answer]                     |[answer]                   |[answer]                   |
|HML          |[answer]                  |[answer]                  |[answer]                  |[answer]                   |[answer]                   |[answer]                   |[answer]                   |[answer]                     |[answer]                   |[answer]                   |
|ERP          |[answer]                  |[answer]                  |[answer]                  |[answer]                   |[answer]                   |[answer]                   |[answer]                   |[answer]                     |[answer]                   |[answer]                   |
    
<br> 
    
**Table 2. Regression R<sup>2</sup> values**    

|                         | Portfolio 1 | Portfolio 2 | Portfolio 3  | Portfolio 4  | Portfolio 5  |
|:------------------------|:-----------:|:-----------:|:------------:|:------------:|:------------:|
|Regression R<sup>2</sup> |[answer]     |[answer]     |[answer]      |[answer]      |[answer]      |
    
</div>

<div class="alert alert-block alert-warning">
Click on this box and type your answer to Challenge Exercise 3. 
</div>


