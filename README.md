# Reinvent 2018 - ARC329 - Massively Parallel Data Processing at Scale

## Overview

In this session we make use of a project called [pywren](http://pywren.io/) to run Python code in parallel at massive scale across [AWS Lambda](https://aws.amazon.com/lambda/) functions. We will be using [Landsat 8](https://aws.amazon.com/public-datasets/landsat/) satellite imagery to calculate a [Normalized Difference Vegetation Index (NDVI)](https://en.wikipedia.org/wiki/Normalized_Difference_Vegetation_Index) across multiple points of interests in the world, using the GeoTIFF data across multiple light spectrum bands using an [embarrassingly parallel](https://en.wikipedia.org/wiki/Embarrassingly_parallel) NVDI calculation function written in Python.


This session will use a [Jupyter Notebook](http://jupyter.org/), an open-source web application that allows to create and share documents that contain live code, equations, visualizations and narrative text. This session also assumes the participant has a functional AWS account with at least one IAM(AWS Identity Access Management) user configured for CLI(AWS Command Line Interface) access.
 Please follow the below setup instructions first, before proceeding with this session.

## Setup Environment

 We will need to have Jupyter Notebook running locally for this session, and ensure we have all the necessary Python libraries downloaded to visualize outputs.

1\. Install Jupyter Notebook on your laptop. _(Note: You can safely skip this step, if you have Jupyter Notebook with kernel of Python 2.7 installed on your machine)_ To do so, follow the instructions on <http://jupyter.org/install.html>. If you have `pip` installed, the easiest way to install Jupyter is by using pip. Please make sure to use **Python 2.7** and not Python 3+ for this workshop.

```
python -m pip install --upgrade pip
python -m pip install jupyter
```

2\. In addition to this, we need to install a variety of Python libraries. These libraries are used for visualization, processing of datasets and to also talk to AWS straight from Python using the [AWS SDK for Python](https://aws.amazon.com/sdk-for-python/). To do so, please execute the following commands:

```
python -m pip install boto3 warcio matplotlib numpy wordcloud nltk rasterio scipy seaborn awscli tldextract bs4 rio_toa folium pandas
```

3\. In the last step, let's install the dependencies for PyWren and set it up locally on our machine.

```
python -m pip install pywren
```

4\. Pywren comes with an interactive setup process as of `v0.2`, let's use this to setup Pywren: **Take note of the AWS Region(us-west-2 recommended), S3 Bucket Name and Function Name as we will need these values to configure our notebook parameters later.**

```
$ pywren-setup

This is the PyWren interactive setup script
Your AWS configuration appears to be set up, and your account ID is 123456789
This interactive script will set up your initial PyWren configuration.
If this is your first time using PyWren then accepting the defaults should be fine.
What is your default aws region? [us-west-2]:
Location for config file:  [~/.pywren_config]:
PyWren requires an s3 bucket to store intermediate data. What s3 bucket would you like to use? [jonas-pywren-604]:
Bucket does not currently exist, would you like to create it? [Y/n]: Y
PyWren prefixes every object it puts in S3 with a particular prefix.
PyWren s3 prefix:  [pywren.jobs]:
Would you like to configure advanced PyWren properties? [y/N]:
PyWren standalone mode uses dedicated AWS instances to run PyWren tasks. This is more flexible, but more expensive with fewer simultaneous workers.
Would you like to enable PyWren standalone mode? [y/N]:
Creating config /Users/pauvince/.pywren_config
new default file created in ~/.pywren_config
lambda role is pywren_exec_role_1
Creating bucket pywren-604.
Creating role.
Deploying lambda.
Pausing for 5 seconds for changes to propagate.
Pausing for 5 seconds for changes to propagate.
Successfully created function.
Pausing for 10 sec for changes to propagate.
function returned: Hello world
```

5\. PyWren uses the default Python logger to communicate progress back. Let's raise our log level in the current terminal session to INFO:

```
export PYWREN_LOGLEVEL=INFO
```

6\. Time to test our Pywren function and see if our laptop can communicate appropriately with our AWS environment and use PyWren in the AWS Lambda function:

```
pywren test_function
```

You should now see an output similar to the following one:

```
2018-10-26 09:30:35,072 [INFO] pywren.executor: using serializer with meta-supplied preinstalls
2018-10-26 09:30:40,403 [INFO] pywren.executor: map 780d4829-7ff5-42b9-80f3-73e020aca329 00000 apply async
2018-10-26 09:30:40,405 [INFO] pywren.executor: call_async 780d4829-7ff5-42b9-80f3-73e020aca329 00000 lambda invoke
2018-10-26 09:30:40,880 [INFO] pywren.executor: call_async 780d4829-7ff5-42b9-80f3-73e020aca329 00000 lambda invoke complete
2018-10-26 09:30:40,910 [INFO] pywren.executor: map invoked 780d4829-7ff5-42b9-80f3-73e020aca329 00000 pool join
2018-10-26 09:30:49,215 [INFO] pywren.future: ResponseFuture.result() 780d4829-7ff5-42b9-80f3-73e020aca329 00000 call_success True
function returned: Hello world

```
7\. The last step we need to copy the function file we will be using in Lambda to the S3 bucket created by the pyWren setup.  At the CMD line, navigate to the root of the cloned repository and type the following.
```
aws s3 cp lambda_function.zip s3://<your S3 Bucket Name>/lambda_function.zip
```

## Run the session

We will use Jupyter Notebooks to locally run our session examples with Pywren and have the workload executed remotely. To do so, we first need to clone this repository to our local machine:

```
git clone https://github.com/<name>/ARC329.git
```

Now enter the newly create `ARC329` folder and start a Jupyter Notebook instance by typing the following command:

```
jupyter notebook
```

This will launch a Jupyter Notebook instance and open your web browser.  Please click onto the folder and launch the  Notebook by clicking on the file named **ReInvent_ARC_329.ipynb**. The notebook will open and provide you with the instructions to complete the session.

## Outcomes

### NDVI Calculation on Satellite Imagery

We will query various locations from the Landsat 8 satellite imagery and analyze them. Example scene:

![Scene Example](Images/napa.jpg)

We will then use the GeoTIFF imagery and it's different information to calculate the NDVI index and analyze how much cloud coverage vs the NVDI we had on certain days:

![NDVI and Cloud Table](Images/TABLE.png)

Lastly we will plot NDVI changes over time for certain areas of interest of the world:

![NDVI Timeseries](Images/PLOT.png)
