# IDC Classifier CaffeNet

![IDC Classifier](../../images/IDC-Classification.jpg)

## Introduction

**IDC Classifier CaffeNet** uses **Caffe** (**CaffeNet**) to provide a way to train a neural network with labelled **breast cancer histology images** to detect **Invasive Ductal Carcinoma (IDC)** in unseen/unlabelled images.

For training a custom trained **CaffeNet model** for detecting **Invasive Ductal Carcinoma (IDC)** trained on the **Intel AI DevCloud** is used and for classification the project uses the **Intel® Movidius**.

## Intel AI DevCloud 

This tutorial uses **Intel AI DevCloud** for training. Access is free and you can request access by visiting the [Intel AI DevCloud](https://software.intel.com/en-us/ai-academy/devcloud "Intel AI DevCloud") area on Intel's website. 

## Cloning The Repo

You will need to clone this repository to a location on your development terminal. Navigate to the directory you would like to download it to and issue the following commands.

```
  $ git clone https://github.com/BreastCancerAI/IDC-Classifier.git
```

Once you have the repo, you will need to find the files in this folder located in [BreastCancerAI/IDC-Classifier/Caffe/CaffeNet](https://github.com/BreastCancerAI/IDC-Classifier/Caffe/CaffeNet "BreastCancerAI/IDC-Classifier/Caffe/CaffeNet").

## Preparing Your IDC Training Data

For this tutorial, I used a dataset from **Kaggle** ( [Breast Histopathology Images](https://www.kaggle.com/paultimothymooney/breast-histopathology-images "Breast Histopathology Images") ), but you are free to use any dataset you like. Once you decide on your dataset you need to arrange your data into the **model/train** directory. Each subdirectory should be named with integers, I used 0 and 1 to represent positive and negative. In my training/testing I used 4400 positive and 4400 negative examples using 60% for training data, 20% for validation data and 20% for testing. The model gave an overall training accuracy of 0.8596 (See Training Results below) and an average confidence of 0.96 on correct identifications. The data provided is 50px x 50px, as CaffeNet use images of size 227px x 227px, the images are resized to 227px x 227px, ideally the images would be that size already so you may want to try different datasets and see how your results vary.

## Upload Training Code & Data To Intel® AI DevCloud

Now you are ready to upload the required files and folders to AI DevCloud. You need to create a zip of some of the directories and files from [BreastCancerAI/IDC-Classifier/Caffe/CaffeNet](https://github.com/BreastCancerAI/IDC-Classifier/Caffe/CaffeNet "BreastCancerAI/IDC-Classifier/Caffe/CaffeNet"), ensuring you have included your training data as outlined above. The zip should include the following:

```
components
data
model
Trainer.ipynb
Trainer.py
```

Create a directory in the root of your Intel AI DevCloud account called **IDC-Classifier** and a subdirectory **CaffeNet** then upload the zip and unzip on the server using terminal. 

## Modify Paths In Prototxt Files

Before you can train the model, you need to update the prototxt files in [IDC-Classifier/CaffeNet/model](https://github.com/BreastCancerAI/IDC-Classifier/Caffe/CaffeNet/model "IDC-Classifier/CaffeNet/model"). Search [IDC-Classifier/CaffeNet/model/solver.prototxt](https://github.com/BreastCancerAI/IDC-Classifier/blob/master/Caffe/CaffeNet/model/solver.prototxt "IDC-Classifier/CaffeNet/model/solver.prototxt") and [IDC-Classifier/CaffeNet/model/train.prototxt](https://github.com/BreastCancerAI/IDC-Classifier/blob/master/Caffe/CaffeNet/model/train.prototxt "IDC-Classifier/CaffeNet/model/train.prototxt") for the word **YourUser** and replace it with your AI DevCloud username.

## Start Training

Next you can complete the training process by following the steps in the Notebook: [IDC-Classifier/CaffeNet/Trainer.ipynb](https://github.com/BreastCancerAI/IDC-Classifier/blob/master/Caffe/CaffeNet/Trainer.ipynb "IDC-Classifier/CaffeNet/Trainer.ipynb").

By following the training Notebook you will accomplish the following:

- Sorting the data
- Creating the LMDB databases for training and validation
- Computing the mean of your training data
- Training your model

## Sorting The Data

The first task of the training Notebook is to sort the training data. In the **Create DataSorter Job** section of the Notebook, we create a job script that will submit a job to run **Trainer.py** on the DevCloud.

```
%%%%writefilewritefil  IDC-Classifier-Trainer
cd $PBS_O_WORKDIR
echo "* Hello world from compute server `hostname` on the A.I. DevCloud!"
echo "* The current directory is ${PWD}."
echo "* Compute server's CPU model and number of logical CPUs:"
lscpu | grep 'Model name\\|^CPU(s)'
echo "* Python available to us:"
export PATH=/glob/intel-python/python3/bin:$PATH;
which python
python --version 
echo "* Starting IDC-Classifier-DataSorter job"
echo "* This job sorts the data for the IDC Classifier"
python Trainer.py
sleep 10
echo "*Adios"
# Remember to have an empty line at the end of the file; otherwise the last command will not run
```

This job will use the **sortData**, **createLMDB** and **computeMean** functions of the **Trainer** class in **Trainer.py**. The final steps for training is to follow the steps in the **Create Training Job** section to complete the training of your model.

# DISCLAIMER

The purpose of the tutorial and source code for **IDC Classifier** is to help people learn how to create computer vision projects and for people interested in the medical use case evaluate if it may help them and to expand upon. Although the the program is fairly accurate in testing, this project is not meant to be an alternative for use instead of seeking professional help. I am a developer not a doctor or expert on cancer.

- **Acknowledgement:** Uses code from Intel® **movidius/ncsdk** ([movidius/ncsdk Github](https://github.com/movidius/ncsdk "movidius/ncsdk Github"))
- **Acknowledgement:** Uses code from chesterkuo **imageclassify-movidius** ([imageclassify-movidius Github](https://github.com/chesterkuo/imageclassify-movidius "imageclassify-movidius Github"))

## Bugs/Issues

Please feel free to create issues for bugs and general issues you come across whilst using this or any other BreastCancerAI repo issues: [BreastCancerAI Github Issues](https://github.com/BreastCancerAI/IDC-Classifier/issues "BreastCancerAI Github Issues")

## Contributors

[![Adam Milton-Barker, Intel® Software Innovator](../../images/Intel-Software-Innovator.jpg)](https://github.com/AdamMiltonBarker)

