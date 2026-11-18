# ANOVA on the seaborn dataset "penguins"
The aim of this study is to investigate whether body mass in penguins differs across species and sex, and whether the effect of species on body mass depends on sex.
The dependent variable was the body weight, and the independent variables were species and sex.

# Description
The main goals were:
* assess whether body mass differs across species
* assess whether body mass differs across species and sex
* assess whether the effect of species depends on sex

*Dataset* = data obtained from the dataset "penguins" from the Python library "seaborn"


*Tools* = Python

# Example analysis
## Python
Custom function, to display the main issues with the data structure, before Data Cleaning
```python
def data_structure(df):
    display(Markdown("## Dataset head"))
    display(df.head())
    display(Markdown("## Shape"))
    print(df.shape)
    display(Markdown("## Info"))
    print(df.info())
    display(Markdown("## Describe"))
    display(df.describe())
    display(Markdown("## Unique values"))
    print(df.nunique())
    display(Markdown("## Null"))
    print(df.isnull().sum())
    display(df[df.isna().any(axis=1)])
    display(Markdown("## Duplicates"))
    print(df.duplicated().sum())
    display(df[df.duplicated()])
```

Data Cleaning (dropping rows with no information on the sex)
```python
print("Shape before cleaning", df.shape)
df_sex = df[~df["sex"].isna()]
print("Shape after cleaning (new dataset)", df_sex.shape)
```

Descriptive statistics and boxplot to identify outliers and for an Exploratory Data Analysis
```python
display(df.groupby("species")["body_mass_g"].describe())
sns.boxplot(y='body_mass_g', hue='species', data=df)
```





Performing One-way ANOVA
```python
df_anova = df[["species", "body_mass_g"]]
model = ols(
    "body_mass_g ~ C(species)",
    data=df_anova
).fit()

anova_results = anova_lm(model, typ=2)

print(anova_results)
```



# Insights
The main results (summarized in "Summary of results.pdf") include: 
* when only species is accounted for, the average body weight of Gentoo differs from the other two species (Adelie and Chinstrap), and the effect size is very high
* when both species and sex are accounted for,
  * within species, males are always bigger than females
  * within sexes, the statistical difference between male Adelie and male Chinstrap is not significant
  * within sexes, Adelie males are bigger than Chinstrap males (even if the difference is not statistically significant), while Adelie females are smaller than Chinstrap females

# How to
A first interpretation of data is in "documentation.xlsx".

There are no raw data, since the data are imported with the library "seaborn". 

Jupyter Notebooks "One-way ANOVA (species).ipynb" and "Two-way ANOVA (species and sex).ipynb" can run the tests again.

An overview of the project and the corresponding results is in "Summary of results.pdf", and in the Word version "Summary of results.docx".
