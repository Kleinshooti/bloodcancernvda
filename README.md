# Blood Cancer image recognition (Acute Lymphoblastic Leukemia only) for nVidia Jetson
A simple nVidia Jetson compatible ML image recognition AI model for detecting Acute Lymphoblastic Leukemia.

![image](https://github.com/Kleinshooti/bloodcancernvda/assets/110417405/656ff225-46db-4055-877a-d481ef72d645)


## The Algorithm

The model used was Resnet-18, a free and open-source ML model. I trained it with a dataset that had 5 classes: Non-cancerous cells, Benign Blood cancer, early Pre-B ALL, Pre-B ALL, and Pro-B ALL. I created this dataset using 2 datasets: one that consisted of images of cells confirmed to be cancerous, and one that consisted of images of cells confirmed to be non-cancerous.
I retrained the model and converted it into ONNX format.
imagenet.py was used to visually determine results.
this is the dataset. https://www.kaggle.com/datasets/harischuth/blood-cancer-dataset-cancer-noncancer

## Running this project (steps are in order)

Download the jetson-inference container from github to a Jetson: https://github.com/dusty-nv/jetson-inference

Change directories into jetson-inference/python/training/classification/data

Run this command in the terminal to download the data
wget https://www.kaggle.com/datasets/harischuth/blood-cancer-dataset-cancer-noncancer/download?datasetVersionNumber=1 -O bloodcancer.tar.gz

run this command in the Terminal.
tar xvzf bloodcancer.tar.gz

Change directory to jetson-inference/

run the docker container by running
./docker/run.sh

I have done file operation for you to save a precious hour of your time as the tar archive is 2GB big.

Make sure to create a labels.txt file in your bloodcancer directory, and save it so that it has all the folder names on separate lines in their order.

From inside the Docker container, change directories so you are in jetson-inference/python/training/classification

Run this command
python3 train.py --model-dir=models/bloodcancer data/bloodcancer --epochs=X
X is the amount of epochs you want.

Wait patiently and do not turn off your Jetson.
(I'm really sorry to tell you that this will take absolutely ages if you are using a Jetson Nano. It's not my fault. I had to wait a night for this to even get to epoch 125, which is good enough. If you want it quicker, get a better Jetson. Don't touch your Jetson Nano heatsink either. You will burn yourself. It will hurt.)

Once you wait and hopefully go outside and touch grass for the first time in a decade if you haven't already, sit back down at your desk, with your Jetson still running and export the model by running this script
python3 onnx_export.py --model-dir=models/bloodcancer

Exit docker by pressing Ctrl + D

Change directories to jetson-inference/python/training/classification

In the terminal enter in NET=models/bloodcancer and DATASET=data/bloodcancer as separate commands on separate lines

finally, enter this in the terminal
imagenet.py --model=$NET/resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=$DATASET/labels.txt $DATASET/test/benign/Snap_114.jpg output.jpg
You can change the output.jpg to anything you want. and you can change the Benign directory to any other one, and change the image name too.

##Video

https://youtu.be/eplIUt4ekrY
