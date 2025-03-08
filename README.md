# Medical-Dataset-EDA
Project completed in pursuit of Master's of Science in Data Analytics.

## RESEARCH QUESTION

I suspect that patients that live in geographically different areas will have very different levels of Vitamin D. Vitamin D deficiencies in patients could be a cause for why patients are being readmitted. This could be useful information for the hospital to know to help them better treat their patients and lower their risk of fines due to readmissions. For this reason, I would like to answer the question: 

***“Do patients from rural areas have different levels of Vitamin D than patients from suburban areas?”***

## BENEFIT FROM ANALYSIS

The stakeholders in the hospital organization could know that patients from different geographical areas have significantly different Vitamin D levels. Vitamin D levels are provided in our dataset and could also prove to have a determining factor in a patient being readmitted to the hospital. Therefore, by learning if certain geographical areas have lower Vitamin D levels, then you could conclude that these patients could benefit from having additional Vitamin D supplements given to them during their initial stay. 

## DATA IDENTIFICATION

In order to answer the question that I have posed, I need to use the columns ‘Area’ and ‘VitD_levels’ from this dataset to run this test. I will need to exclude urban from the Area variable as I am only focusing my analysis on suburban and rural. I will need to use the Pandas DataFrame to import the csv file so that I can analyze the data in this dataframe (Pandas, 2023).

## T-TEST 
```python
#Perform a 2-sample T-test comparing Vit_D levels of rural/suburban patients
rural = med_data[med_data.Area == 'Rural'].VitD_levels
suburban = med_data[med_data.Area == 'Suburban'].VitD_levels

t_result = stats.ttest_ind(rural, suburban)

alpha = 0.05

if (t_result[1] < alpha):
    print("Vitamin D levels in rural areas and suburban areas are significantly different!")
    print("")
    print("Test Statistic:", round(t_result[0],5))
    print("P-Value:", round(t_result[1],5))
else: 
    print("Vitamin D levels in rural areas and suburban areas are not significantly different.")
    print("")
    print("Test Statistic:", round(t_result[0],5))
    print("P-Value:", round(t_result[1],5))
```

> Vitamin D levels in rural areas and suburban areas are not significantly different.
>
> Test Statistic: -0.36873
> P-Value: 0.71234



## JUSTIFICATION

I chose the t-test method because I wanted to test the differences in the means of the Vitamin D levels of rural and suburban patients. This test is the most effective hypothesis test given the variables I chose. I wanted to find the likelihood of these variables’ differences being caused by random chance. This technique will be able to determine if there is any statistical or significant difference in the Vitamin D levels of rural and suburban patients. 

## UNIVARIATE STATISTICS 

The first continuous variable I chose is the Income variable. 
The distribution is a skewed distribution that is positively skewed to the right.  

![IMG_1582](https://github.com/user-attachments/assets/802258c1-ba46-484f-acd6-60a60697ffb9)

The second continuous variable I chose is the TotalCharge variable. 
This distribution is a bimodal distribution where both modes are asymmetrical. 

![IMG_1583](https://github.com/user-attachments/assets/c37c29b9-b227-43ae-92ff-c688f4dfb118)

The next variable I chose is the Marital variable, which is categorical. 
The distribution of the Marital variable is uniform.

![IMG_1584](https://github.com/user-attachments/assets/a5be95b6-dbce-4e1f-8d10-af8bd1383aa5)

Lastly, for the final categorical variable, I chose the Complication_risk variable. 
This shows a skewed distribution that is positively skewed to the right. 

![IMG_1585](https://github.com/user-attachments/assets/5e72f7ef-f2f5-4d0e-ac42-b9f2fbd77e26)

## BIVARIATE STATISTICS

The two categorical variables I chose are Suburban Area Vitamin D levels and Rural Area Vitamin D levels. The bivariate plot of these variables has a normal distribution (Kibirige, 2023). The two continuous variables I chose are VitD_levels and Initial_days. The bivariate plot of these variables shows a bimodal distribution with no significant correlation (Waskom, 2012-2022). 

```python
#Bivariate Density Plot - Suburban vs. Rural Vitamin D levels

#remove Suburban values from Area variable
area_med_data = med_data[(med_data['Area'] == 'Suburban') | (med_data['Area'] == 'Rural')]

print(p9.ggplot(area_med_data) 
      + p9.aes(x='VitD_levels', fill='Area') 
      + p9.geom_density(alpha=0.5))
```

![IMG_1586](https://github.com/user-attachments/assets/544e285a-98fe-4d6b-ac43-ce89f3473052)

```python
#Bivariate Scatter Plot - VitD_levels vs. Initial_days

subset = med_data.sample(n=1000, random_state=500)

sns.scatterplot(x='VitD_levels', y='Initial_days', data=subset)
plt.show()

#Run a Pearson-R correlation test
pearson = stats.pearsonr(med_data.VitD_levels, med_data.Initial_days)
print(pearson)
print("")
# Test if p-value is bigger or smaller than alpha
alpha = 0.05
if pearson[1] < alpha:
    print("How long a patient stays in the hospital and their Vitamin D levels are significantly correlated")
else:
    print("No significant correlation found between how many days a patient stays at the hospital and their Vitamin D levels.")
```

![IMG_1587](https://github.com/user-attachments/assets/6afc8de6-9dab-4cb2-9746-18480b854d91)

> PearsonRResult(statistic=-0.00260027203914892, pvalue=0.7948675238809281)
>
> No significant correlation found between how many days a patient stays at the hospital and their Vitamin D levels.

## RESULTS OF ANALYSIS

The results of the hypothesis test that I ran are that we must accept the null hypothesis that both variables are too similar to provide any significant statistical differences (Longe, 2023). The critical value in my test was 0.71234 which is a value much greater than the alpha of 0.05. This means that *the Vitamin D levels of patients from rural areas are not significantly different than the Vitamin D levels of patients from suburban areas* (SciPy, 2008-2023). 

## LIMITATIONS OF ANALYSIS

The limitations of my data analysis are that we are using a sample size of 10,000 patients. This does provide some good analysis, but we do not really know how much of the total population in these areas this represents. So, we need to understand these limitations when generalizing these results on a much larger scale. We are also limited to understanding the differences of two areas, but there are a lot of layers that we may be missing in the analysis due to only using two variables. If there are outliers in the dataset, then this could sway the results of the analysis. I treated any outliers in the dataset prior to running this test (Nigam, 2022). 

## RECOMMENDED COURSE OF ACTION
  	
Since there is no significant difference in the Vitamin D levels of rural and suburban patients, my recommended course of action would be to treat both groups of patients with the same level of treatment regarding Vitamin D levels. There is no reason to use any special means of treatment for a rural patient over a suburban patient, or vice versa. 

## SUPPORTING DOCUMENTATION

#### SOURCES FOR THIRD-PARTY CODE

The SciPy community (2008-2023). Retrieved, September 27, 2023, from https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_ind.html.

Pandas (2023, June 28). Retrieved September 27, 2023, from https://pandas.pydata.org/docs/reference/index.html.

Waskom, M. (2012-2022). Seaborn Statistical Data Visualization. Retrieved September 27, 2023, from https://seaborn.pydata.org/index.html.

Kibirige, H. (2023). Retrieved on September 27, 2023, from https://plotnine.readthedocs.io/en/v0.12.3/.


#### SOURCES 

Longe, B. (2023, July 27). T-testing: Definition, Formula & Interpretations. Retrieved September 27, 2023, from https://www.formpl.us/blog/t-testing

Nigam, V. (2022, February 8). Statistical Tests: When to Use T-Test, Chi-Square and More. Retrieved September 27, 2023, from https://builtin.com/data-science/t-test-vs-chi-square

Kumar, A. (2022, February 18). A Quick Guide to Bivariate Analysis in Python. Retrieved September 27, 2023, from https://www.analyticsvidhya.com/blog/2022/02/a-quick-guide-to-bivariate-analysis-in-python/.
