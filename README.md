# Capstone Project - Azure Machine Learning Engineer Nanodegree program
In this project, two models were created one using Automated ML and the other a customized model whose hyperparameters are tuned using HyperDrive on an external dataset.

## Project Set Up and Installation
To set up this project,
1. Create a compute instance in AzureML studio
2. Provide a link to your dataset or register your dataset on AzureML
3. Model Building (AutoML):
    1. Set up AutoML configuration
    2. Run the AutoML configuration and get your best model and best run
4. Model Building (Hyperdrive):
    1. Set up Hyperdrive configuration
    2. Run the Hyperdrive configuration and save your best model and best run
4. Model Deployment: Deploy your best model as a webservice
5. Consume the endpoint using the rest uri and primary key.
 
## Dataset

### Overview
The data used in this project is the Heart falure dataset available on kaggle. The dataset contains 12 features that can be used to predict mortality by heart failure.
These features includes behavioural lifestyles that could increase the risk of heart failure such as tobacco use, unhealthy diet and obesity, physical inactivity and harmful use of alcohol. 

### Task
The heart failure dataset will be used to build a classification model to predict either an occurence of death(1) or not(0) based on the following features: 
age, anaemia, creatinine_phosphokinase, diabetes,	ejection_fraction,	high_blood_pressure,	platelets	serum_creatinine,	serum_sodium,	sex,	smoking and time


### Access

A link to the dataset is provided in the notebook and it was accessed using the tabulardatasetfactory function

## Automated ML

This project is a classification task and accuracy was used as the primary metric, number of cross validation was set to 5.
The detailed settings used are in screenshot below:

![image](https://user-images.githubusercontent.com/86358182/130730662-babee264-7636-4bf3-8be8-09bae7550650.png)



### Results

*TODO* Remeber to provide screenshots of the `RunDetails` widget as well as a screenshot of the best model trained with it's parameters.
The best model was votingEsemble which is a combination of other good performing models with an accuracy of 0.8791

![image](https://user-images.githubusercontent.com/86358182/130728824-f830a3dc-e370-4cfb-bd36-512f5aa04cf4.png)

Parameters of the best model

![image](https://user-images.githubusercontent.com/86358182/130869228-fec2587b-d8ae-4462-ad92-0f21c21fcf78.png)

Run Details widget

## Hyperparameter Tuning
*TODO*: What kind of model did you choose for this experiment and why? Give an overview of the types of parameters and their ranges used for the hyperparameter search

Considering the simplisity of this dataset, Logistic regression was used for the hyperparameter tuning because it provides good accuracy and it is less inclined to overfitting especially with low dimentional dataset like this (299 rows x 12 columns).

This model can be improved by trying out other algorithms, for example, tree-based models like xgboost and lightgbm. 
Perfoming some feature transformation on the data set like cutting the continous variables into bins of categories could help improve the model performance.

### Results
*TODO*: What are the results you got with your model? What were the parameters of the model? How could you have improved it?
The best accuracy for hyperparameter tuning was approximately 0.88

![image](https://user-images.githubusercontent.com/86358182/130730344-a313a0f6-965f-41db-b3be-411c6ae33224.png)

![image](https://user-images.githubusercontent.com/86358182/130865794-1a2ebe03-5ee4-4309-b5ef-abbbe1ea5e57.png)

![image](https://user-images.githubusercontent.com/86358182/130865823-e8d3f31a-0b32-4856-8e8a-dde556085b15.png)

Picture of the RunDetails widget

## Model Deployment
*TODO*: Give an overview of the deployed model and instructions on how to query the endpoint with a sample input.
The AutoML Model was deployed as a webservice using ACI webservice because it performed better on the test set.

![image](https://user-images.githubusercontent.com/86358182/130869008-1343cadf-a369-4e8c-b1f7-33e5b10bb419.png)

Deployment Code

![image](https://user-images.githubusercontent.com/86358182/130867511-32fdb602-6b69-400a-a576-c40680b5920b.png)

Model endpoint


Sample call to the endoint

To query the endpoint, here is a sample data;

import requests

import json

scoring_uri = "http://1677aa14-2086-4ca7-bcf8-045a08c4e2ce.southcentralus.azurecontainer.io/score"

key = "7Xxr6kaBqQKQfqbTeto8LJGnmuC88MuG"

data = {
    "data": [
    
        {
        "age":75.0,
"anaemia":0,
"creatinine_phosphokinase": 582,
"diabetes":0,
"ejection_fraction":20,
"high_blood_pressure":1,
"platelets":265000.00,
"serum_creatinine":1.9,
"serum_sodium":130,
"sex":1,
"smoking":0,
"time":4                       
            },
        {
        "age":55.0,
"anaemia":0,
"creatinine_phosphokinase": 7861,
"diabetes":0,
"ejection_fraction":38,
"high_blood_pressure":0,
"platelets":263358.03,
"serum_creatinine":1.1,
"serum_sodium":136,
"sex":1,
"smoking":0,
"time":6   
        
            },
        ]
    }


input_data = json.dumps(data)
with open("data.json", "w") as _f:
    _f.write(input_data)


headers = {"Content-Type": "application/json"}

headers["Authorization"] = f"Bearer {key}"


resp = requests.post(scoring_uri, input_data, headers=headers)
print(resp.json())


## Screen Recording
A screencast recording for this project is available here https://youtu.be/8LqSgYSe0DM

