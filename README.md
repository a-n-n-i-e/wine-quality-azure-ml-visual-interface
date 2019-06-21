# Is that wine good or bad? A tutorial on how to build a binary classification for red wine quality using Azure Machine Learning Visual Interface

## Tools and Data
### Create Resource in Azure
1. Go to [Azure Portal](https://azure.portal.com/) and login or [Create an Account](https://azure.microsoft.com/en-us/free/)
2. Click "Create resource"
3. Select "AI + Machine Learning" then "Machine Learning service workspace"
4. Fill in requried fields and select "Review + Create" then select "Create"
 </br> ![createamlresource][createamlresource]
5. It will take a few minutes to create the resources needed for your workspace. Below is a list of all the resources that are created:
</br> ![workspaceresourcelist][workspaceresourcelist]

### Launch AML VI
1. Navigate to your resource group that you created the workspace under
2. Click the "Machine Learning Service Workspace" resource listed in the resource group
3. In the left nav click on "Visual Interface"
4. Then click "Launch visual interface"
5. This will open a new tab for the Visual interface for Azure Machine Learning Service
</br> ![launchamlvi][launchamlvi]

### We need data!
1. I used a dataset I found on Kaggle. Kaggle is an online community of data scientists and machine learners. 
2. Download the dataset from this repo because I have added an additional field (quality bool) to the dataset.
* [Wine Dataset from Repo](https://github.com/cassieview/IntroToAzureMLInterface/blob/master/dataset/winequality-red.csv)
* [Kaggle Dataset](https://www.kaggle.com/uciml/red-wine-quality-cortez-et-al-2009) 
</br> _Relevant publication: P. Cortez, A. Cerdeira, F. Almeida, T. Matos and J. Reis. Modeling wine preferences by data mining from physicochemical properties. In Decision Support Systems, Elsevier, 47(4):547-553, 2009._

### Getting data into AML VI
There are a few different ways to import data into VI. You can use the [Import Data Module](https://docs.microsoft.com/en-us/azure/machine-learning/algorithm-module-reference/import-data) to import data from Azure Blob Storage or a Web URL via HTTP. In this tutorial we are going to upload our data into the "My Datasets" in AML VI.
1. Select "New" from the bottom left corner of the browser
2. From the left nav bar Select "Datasets"
3. Select "Upload from Local file"
4. Navigate to downloaded data and select it to be uploaded
5. Update the name and add a description (its helpful to have detailed description once there are lots of datasets uploaded)
</br>![uploaddataset][uploaddataset]

### Create New Experiment
1. Select "New" from the bottom left corner of the browser
2. Select "Blank Experiment"
3. In the top left hand of the workspace select the experiment name text "Expirment created on xx/xx/xxxx" and edit the name of your expirnment.
4. Go to My Datasets to find the data uploaded OR use the import module to import from the github csv link
5. Drag data onto workspace
</br> ![createexpadddata][createexpadddata]

## Build the Model
### Assign the label attribute to the dataset
We now have created an experiment and have imported the data. Lets build the model. In the left hand nav there are different modules that you can drag and drop onto the workspace to build the model.
1. Under Data Transformation > Manipulation drag and drop the "Edit Metadata" module onto the workspace
2. Connect the modules together be clicking and dragging on the circles like a visio diagram.
3. Click on the "Edit Metadata" and select "Edit Columns" from the right hand side of the workspace
4. Leave the default configuration and type `qualityBool` into the textbox and click "Ok"

### The First Run of the Experiment
1. Select "Run" from the button of the workspace
2. Select "Create new" to create a new compute target
3. Enter a name for the new compute target
4. Select "Run"
</br>![runexperiment][runexperiment]

### Select Feature Columns
1. Under Data Transformation > Manipulation drag and drop the "Edit Metadata" module onto the workspace
2. Connect the modules together be clicking and dragging on the circles like a visio diagram.
3. Click on the "Edit Metadata" and select "Edit Columns" from the right hand side of the workspace
4. Select all the columns except for `quality` and `qualityBool`
5. Select the arrow to move the highlighted features into the "Selected Columns" box and click "Ok".

### Split the Data
When you train the model the standard practice is to split your data to train and score your model. 70% trains the model and 30% scores the model to see how well the training went. Understand that true model accuracy should be tested on unseen data outside of this 30% score.  This score gives you an idea of how the model is performing but is not law and sometimes misleading.

1. In the left nav type "Split Data" in the textbox at the top
2. Drag and drop the module onto the workspace and connect it to the existing modules
3. Select the "Split Data" module and change the split from `0.5` to `0.7`

### Train, Score and Evaluate the Model
Now we have prepared our data by select features, assigning labels, cleaning and preprocessing. Its time to train the model.

1. There are many different aglorthms to choose from when building a model. Many professional data scientists try a few different ones to see which provides a better accuracy score. [Here is a cheatsheet for choosing an algorithm](https://docs.microsoft.com/en-us/azure/machine-learning/studio/algorithm-cheat-sheet). For this model we are going to use a `Two-Class Logistic Regression`.
2. Add the following modules to the workspace: `Two-Class Logistic Regression`, `Train Model`, `Score Model`, `Evaluate Model`
</br> _hint: if you have questions about modules or concepts, click on the module and in the lower right corner of the workspace you will see a "more help" link. Click the link to get infomration about how the module works and help with data science terms_
3. Connect them together as displayed below
4. Select the `Train Model` module and click "Edit Columns" in the right side of the workspace
5. Type `qualityBool` into the textbox to indicate the dataset label
6. Run the Experiment

### Check Accuracy of model
We now have a trained model in Azure Machine Learning Visual Interface. Lets visualize our results to see how it performed.

1. Right click on the button circle of the `Evaluate Model` module.
2. [How to understand metrics for classification models](https://docs.microsoft.com/en-us/azure/machine-learning/algorithm-module-reference/evaluate-model#bkmk_classification)

### Deploy the Web Service
Once the model has an acceptable or "good enough" accuracy its time to deploy your model to a web service.

1. Click "Create predictive experiment" in the bottom nav of the workspace
2. Click "Run" on the predictive experiment, select the compute and click "Run"
3. Now the model you created will show up under "Trained Models" in the left nav of the workspace. This allows you to import trained models into different experiments
4. Click "Deploy Web Service" in the bottom nav of the workspace
5. Now we need to create a web service compute target (if you dont already have one)
6. Click "Create new" and then click the "Go to azure portal link" think will open a new tab and bring you to the azure machine learning workspace resource with compute selected from the left hand nav. Follow the instructions in the pane to create the compute target for the web service.
7. Once you have created the compute target, click refresh in the corner of the pane to show the newly created compute target
8. Select the Compute target and click "Deploy"

### Test the Web Service

1. Select the "Web Service" icon on the left nav of the workspace. The web service that was created will be listed.
2. Click the web service that was created
3. Here you can test and get the information needed to consume the API created.

### You have now created a machine learning model using Azure Machine Learning Visual Interface! 🎉✨

## Helpful Machine Learning Concepts and Best Practices

### What features are needed to train the model
In this example we used all the attributes in the datasets as features. When building a model is it important to think about what features actually help make a decision to train a model. 

### we have 100% accuracy which doesnt mean its awesome.. it could just mean its overfitted
1. Overfitting a model means you dont have enough data for it to actual "learn" so it will do great on your data but as soon as it put out into the real world. It will fail. This is why you want to always test with unseen data. Lets grab some data from another source to see if we actually have a realy good model or if its overfitted.
2. If data is very imbalanced meaning you have lots of one label and little of another - this can also create overfitting and models that look like they are performing really well when they are actually not good models. There are different techniques to work with imbalanced data.
* sampling
* SMOTE
* WHATELSE??

## Terms

Feature:
Label:

## Helpful Links
[Machine Learning Visual Interface Overview Doc](https://docs.microsoft.com/en-us/azure/machine-learning/service/ui-concept-visual-interface)


[create-resource]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/createresource.png "Create Resource"
[select-workspace]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/selectworkspace.PNG "Select Workspace"
[workspaceresourcelist]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/workspaceresourcelist.PNG

[viportal]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/viportal.PNG
[navvisualinterface]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/navvisualinterface.PNG
[launchvisualinterface]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/launchvisualinterface.PNG
[runexperiment]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/runexperiment.PNG
[datasetuploadfields]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/datasetuploadfields.PNG
[newdatasetupload]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/newdatasetupload.png
[createexpadddata]: https://github.com/cassieview/IntroToAzureMLInterface/blob/master/doc-imgs/createexpadddata.gif
[createamlresource]: https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/doc-imgs/createamlresource.gif
[launchamlvi]:https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/doc-imgs/launchamlvi.gif
[uploaddataset]: https://github.com/cassieview/wine-quality-azure-ml-visual-interface/blob/master/doc-imgs/uploaddataset.gif
