[![MIT License](https://img.shields.io/badge/License-MIT-lightgray.svg)](LICENSE)
![Python Version](https://img.shields.io/badge/Python-3.8.5-blue.svg)
<!-- Add in additional badges as appropriate -->

![Banner of NHS AI Lab Skunkworks ](docs/banner.png)

# NHS AI Lab Skunkworks project: C359 - NHS Ambulance Handover Delay Predictor

> A pilot project for the NHS AI (Artificial Intelligence) Lab Skunkworks team, C359 - NHS Ambulance Handover Delay Predictor, will use statistical analysis and machine learning to understand whether AI approaches can be used to create a proactive response to the redirect of ambulances between hospitals on a given day in order to minimise the time spent waiting for patient handover and thereby maximise the time available to respond to patient calls for help
C359 - NHS Ambulance Handover Delay Predictorwas selected as a project in Q2 2022 following a succesful pitch to the AI Skunkworks problem-sourcing programme.

## Intended Use

This proof of concept ([TRL 4](https://en.wikipedia.org/wiki/Technology_readiness_level)) is intended to demonstrate the technical validity of applying a Random Forest modelling technique to numerous datasets in order to solve the problem of ambulance delays while handing patients over to the emergency departments at hospitals. The data that were available for the project were:
1. ambulance assignments data
2. incidents data
3. patient transfers data
It is not intended for deployment in a clinical or non-clinical setting without further development and compliance with the [UK Medical Device Regulations 2002](https://www.legislation.gov.uk/uksi/2002/618/contents/made) where the product qualifies as a medical device.


## Data Protection

This project was subject to a Data Protection Impact Assessment (DPIA), ensuring the protection of the data used in line with the [UK Data Protection Act 2018](https://www.legislation.gov.uk/ukpga/2018/12/contents/enacted) and [UK GDPR](https://ico.org.uk/for-organisations/dp-at-the-end-of-the-transition-period/data-protection-and-the-eu-in-detail/the-uk-gdpr/). No data or trained models are shared in this repository.


## Background

South Central Ambulance Service (SCAS) has around 2.5 million total patient contacts per year across the services of Hampshire, Thames Valley, Surrey and Sussex.  To ensure the delivery of an effective and efficient service, SCAS need to make informed decisions, drawing upon the best possible management information available.
The decision on where to take ambulance patients is complex taking in factors such as severity of illness, urgency of the situation, geography, travel time and handover/queuing time.  With national targets on handover times and NHS trusts publishing their ability to meet these targets, there is an opportunity to determine if the large volume of electronic data available from patients who have been taken to hospital can be used within Artificial intelligence (AI)/Machine Learning (ML) to predict where patients should be sent to minimise waiting times.  Such techniques are commonly used across industries that provide services or sell goods through, but not limited to: optimising staffing schedules, reducing waiting times (e.g. to answer calls or to be physically seen) and increasing the robustness of a queuing system to the inevitable variation in demand for a service.  
Looking across an NHS trust, knowing where ambulances can handover patients to inform balancing for care outcomes could potentially improve the performance and safety for both the ambulance trusts and hospitals, whilst also: 
- reducing the stress and improve the overall experience for the patient
- reducing the overall clinical risk as handover from the ambulance to the hospital will happen as quickly as possible
- reducing operational pressures on the ambulance service provider by reducing the amount of reactive management, thereby also reducing staffing stress levels
- increasing both the hospital and ambulance efficiency – for every patient waiting to be admitted to the hospital, there is an ambulance crew that is not able to attend another call


## Model selection

The data available for this project included:
1. details about incidents which ambulances had to attend to, 
2. details about ambulances that arrive at hospitals to hand over patients to emergency departments, and
3. details about patient transfers in, and between hospitals

The additional datasets that were used for the project included:
1. UK Bank holidays data
2. Weather data - Minimum temperature, Maximum temperature, Rainfall (mm)

After the cleaning stage, the available data was first manipulated to get some useful metrics for the analysis. Some of the metrics derived were the handover time in minutes (and consequently handover delay), the euclidean distance between the hospitals, and the past delays in handing over patients. The correlations and trends in the data were checked into. Different modelling techniques were investigated into, namely:
1. Regression techniques
2. Time Series Analysis
3. Naive Bayes technique
4. Decision Tree algorithm
5. Random Forest algorithm

First we investigated into the correlation between the variables. Due to the minimal correlation, the regression techniques could not be carried forward. Then some time series analysis were done on the data. It was seen that there were some pattern in the data. But due to the nature of the project, where both the handover delays (in minutes) and the reasons causing the delays were to be predicted, the time series analysis could not be carried forward. For the Naive Bayes approach, due to the response variable being non-normal, it could not be carried forward as this is one of the main assumptions of the model. 

The Decision Tree was a better fit for the model to be used for the project. It can produce both numerical and categorical (non-numerical) predictions. It is basically a machine learning algorithm that helps make decisions based on previous experiences. At every step, based on the different features, the model answers questions using which it ultimately comes to a conclusion, which is the prediction. Also, the most important features which contributed to the predition being made can be extracted from the model. This was used to obtain the potential reasons for the delay. But sometimes, decision trees may be prone to overfitting, which is the case when the model performs well during training, but fails to produce reliable predictions in production. For this reason, the Decision Tree was not carried forward.

Finally, we decided to go with the Random Forest algorithm. The algorithm basically works just like a Decision Tree, except that it consists of multiple trees. We can get predictions for the handover delays, and by extracting the most important features contributing to the predictions, the potential reasons for the delays can be obtained. Since the Random Forest algorithm consists of multiple Decision Trees, it reduces the risks of overfitting as well.

## Known limitations

There were multiple limitations that were faced over the course of this project. Some of them were:
1. No data was available about the state of the hospitals themselves at the times available in the data.
This made it hard to identify whether there were some factors in the hospitals which were contributing to the ambulance delays.
2. The pre-processing stages of the notebooks are designed to work on the format that the `Assignments dataset`, `Incidents dataset`, and `PTS dataset` were for the project. 
If a different dataset is to be used, or if the structure of the dataset has changed, the code will need modifying so as to ensure the smooth running of the code.
3. The secure paths where the different datasets will be saved need to be added to the scripts to make it work. 
4. The exclusion of non-emergency incidents from the dataset reduced the available data by more than 50%.
This can be reviewed so that more data is available for analysis in the future.
5. Disparity in the number of data points for the different hospitals.
The number of data points available for the different hospitals varied a lot in the hospitals. A subset of the most significant hospitals was used for the project.


## Data pipeline

![Data Pipeline for the modelling](docs/data_pipeline_c359.png)
The data dictionary is found [in the data dictionary notebook.](notebooks/00-data_dictionary_notebook.ipynb)


## Directory structure

The directory structure of this project includes data stored outside of the git tree. This is to ensure that, when coding in the open, no data can accidentally be committed to the repository through either the use of `git push -f` to override a `.gitignore` file, or through ignoring the `pre-commit` hooks.

A `project-directory` must first be created, inside of which this repository can be cloned (into e.g. `repo-directory`).

`data` and `models` folders will be stored at the highest level, outside the git tree, and must be created manually first, including the subdirectories `data/interim`, `data/processed` and `data/raw`:

![PNG of Directory Structure](docs/to-add-directory-structure.bays)

```
e.g., (Needs to be changed)
project-directory
├── repo-directory
│   ├── .git
│   ├── .github
│   ├── config
│   ├── docs
│   ├── notebooks
│   ├── outputs
│   └── src
├── data
│   ├── interim
│   ├── processed
│   └── raw
└── models
```


## Getting Started

1. Create a folder for this project, and clone this repository (`https://github.com/nhsx/skunkworks-ambulance-queueing-prediction`) as a subfolder of that folder e.g. `repo-directory` as above
2. Switch to the `develop` branch: `git branch develop`
3. Create a new virtual environment e.g., `pyenv virtualenv 3.8.13 ambulance-delays-project`
4. Activate your environment e.g., `pyenv activate ambulance-delays-project`
5. Install required packages: `pip install -r requirements.txt`
6. __Activate the git pre commit hook: pre-commit install__
7. Execute notebooks in [notebooks/](notebooks)

Please note that `python v3.9.12` was used for this project.

## NHS AI Lab Skunkworks
The project is supported by the NHS AI Lab Skunkworks, which exists within the NHS AI Lab at NHSX to support the health and care community to rapidly progress ideas from the conceptual stage to a proof of concept.

Find out more about the [NHS AI Lab Skunkworks](https://www.nhsx.nhs.uk/ai-lab/ai-lab-programmes/skunkworks/).
Join our [Virtual Hub](https://future.nhs.uk/connect.ti/system/text/register) to hear more about future problem-sourcing event opportunities.
Get in touch with the Skunkworks team at [aiskunkworks@nhsx.nhs.uk](aiskunkworks@nhsx.nhs.uk).


## Licence

Unless stated otherwise, the codebase is released under [the MIT Licence][mit].
This covers both the codebase and any sample code in the documentation.

The documentation is [© Crown copyright][copyright] and available under the terms
of the [Open Government 3.0][ogl] licence.

[mit]: LICENCE
[copyright]: http://www.nationalarchives.gov.uk/information-management/re-using-public-sector-information/uk-government-licensing-framework/crown-copyright/
[ogl]: http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/


## End to End Testing

If you want to run through this project end to end, you can do so by executing the following commands. Please note you must ensure your environment is set up as described in Getting Started and all commands below assume the working directory is `skunkworks-ambulance-queueing-prediction/notebooks`

`jupyter nbconvert --to notebook --execute 01-fake-data-generation.ipynb --ExecutePreprocessor.kernel_name=python3`

`jupyter nbconvert --to notebook --execute 02-preparing-data-for-modelling.ipynb --ExecutePreprocessor.kernel_name=python3`

`jupyter nbconvert --to notebook --execute 03-modelling.ipynb --ExecutePreprocessor.kernel_name=python3`