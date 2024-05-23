# YOLOv4 model to predict the presence of pneumonia in x-ray chest images.

### The dataset was obtained from https://academictorrents.com/details/a0d80e1bb03ef8357d71e058ef9471b4468cd18e

## Conversion
The original data was in DICOM format, so after installing and importing the necessary libraries, I implemented a convert() function to handle the conversion process. This function constructs the file path for each DICOM file, checks if the file exists, reads the DICOM file, retrieves the pixel array, resizes the image to 224x224 pixels, converts the image to RGB format, and saves it as a JPG file in the specified output path.

## Creating files
We start by loading the CSV file containing image labels and separates the data into two DataFrames based on the target values 1 and 0. For each JPEG image in the training images directory, it extracts the patientId from the filename and checks if there are corresponding rows in the DataFrame for target=1. If found, it creates a text file with bounding box data for each image where the target is 1. If the patientId has a target of 0, it creates an empty text file. 

The code then generates train.txt and test.txt files containing the paths to the training and testing images. We also create a classes.names file listing the class names and an image_data.data file specifying the number of classes, paths to the training and validation text files, class names file, and backup directory. 

## Clone the darknet repository
We clone the darknet repository from https://github.com/AlexeyAB/darknet and run the !make command after navigating to the darknet directory. Inside darknet/makefile: we change GPU, CUDNN, and OPENCV to 1.
We then download yolov4.conv.137 file, which contains pretrained weights for the convolutuon layers.

Inside the darknet/cfg folder, we create a new file named yolov4_custom.cfg, then we copy the contents of the yolov4.cfg file from the darknet/cfg folder and paste it to the new yolov4_custom.cfg file we created. We then change the following parameters in yolov4_custom.cfg: batch=64, subdivision=16, max_batches= 4000, steps= 3200, 3600, filter=21, width=224, height=224, classes=2.

## Training
We call the train command for the model and give it the images_data.data file path, the yolov4_custom.cfg file, and the yolov4.conv.137 weights file. In case of disconnection during training, we will continue training using a similar command, with the modification of the weights file path to be the path of the last weights of the training before it disconnected.

![chart](https://github.com/sottohy/Pneumonia-Detection/assets/91037437/bc4b9f29-6bee-4900-a10c-eda3708a9206)


## Testing (Prediction)
We change the following parameters in yolov4_custom.cfg to prepare for testing: batch=1, subdivision=1, batch_normalize=0. Then we run the test command where we give it the image_data.data file path, the yolov4_custom.cfg file path, the yolov4_custom_final.weights file path, and the path to an image from the test folder. We specify the result ouput file so that the results of the test would be saved in a txt file named result.txt.

Lastly, we use OpenCV to load the predicted image, read detection results from the result.txt file, and annotate the image with bounding boxes for class "Pneumonia". We extract the (class name, confidence, x_min, y_min, x_max, y_max) then store them in a list. The annotated image is displayed using a window and saved to a file named annotated_image.jpg.


