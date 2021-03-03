# Titanic_dataset
School project on the titanic survivor dataset 
import pandas as pd
df = pd.read_csv('titanic_train.csv')
print(df.head())
print(df.columns)
pd.set_option('display.max_columns', None)

#Q1: How many people are in titanic and how many survivors?

#total number of passengers#
total_passenger_count = len(df)
print(total_passenger_count)

#number of survivors#
survivors_count = (df['Survived']).sum()
print(survivors_count)

# Q2: How many that survived were female and how many that died were female?#

#-------------------------------Solution 1------------------------------------

#Using the code below, we'll get the count of survivors by gender
table = pd.crosstab(df['Survived'],df['Sex'])
print(table)

#-------------------------------Solution 2------------------------------------

#number of female survivors#
female_survivors = df.loc[(df.Survived == 1)&(df.Sex=='female')]
print(len(female_survivors))

#number of female deaths#
female_deaths = df.loc[(df.Survived == 0)&(df.Sex=='female')]
print(len(female_deaths))



#Q3: How many children were on the titanic?#
#Children defined as younger than 18 years old#

children_count = (df['Age'] < 18).sum()
print(children_count)



#Q4: How many children died that were on the ship?#


#-------------------------------Solution 1------------------------------------

table_1 = pd.crosstab(df['Survived'],df['Age'] < 18)
print(table_1)

#-------------------------------Solution 2------------------------------------

children_deaths = df.loc[(df.Survived == 0)&(df.Age < 18)]
print(len(children_deaths))




# Q5 : How many people had families with them?#

df['passengers_with_families'] = df['SibSp'] + df['Parch']
family_count= (df['passengers_with_families'] >= 1).sum()
print(family_count)


# Q6 : What is the ratio of female to male?#
female_count = (df['Sex']=='female').sum()
male_count = (df['Sex']=='male').sum()
ratio = female_count/male_count
print(ratio)


# Q7 : What contributed to the survival of those who survived?#

#Data cleaning#
df['isMale'] = df.Sex.map(lambda x: 1 if x == 'male' else 0)

obj_df = df.select_dtypes(include=['object']).copy()
cleanup_nums = {'Embarked': {'S': 0, 'C': 1, 'Q':2}}
obj_df = obj_df.replace(cleanup_nums)
df['Embarked1']= obj_df['Embarked']

data_selection = df[['Survived', 'Pclass', 'isMale', 'Age', 'SibSp','Parch', 'Fare', 'Embarked1']]


#Correlation analysis#
corrMatrix = data_selection.corr()
print(corrMatrix)
