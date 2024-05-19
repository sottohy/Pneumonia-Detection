# YOLOv4 model to predict the presence of pneumonia in chest images.

### The dataset was obtained from https://academictorrents.com/details/a0d80e1bb03ef8357d71e058ef9471b4468cd18e

## Conversion
The original data was in DICOM format, so after installing and importing the necessary libraries, I implemented a convert() function to handle the conversion process. This function constructs the file path for each DICOM file, checks if the file exists, reads the DICOM file, retrieves the pixel array, resizes the image to 224x224 pixels, converts the image to RGB format, and saves it as a JPG file in the specified output path.

## txt files
We start by loading the CSV file containing image labels and separates the data into two DataFrames based on the target values 1 and 0. For each JPEG image in the training images directory, it extracts the patientId from the filename and checks if there are corresponding rows in the DataFrame for target=1. If found, it creates a text file with bounding box data for each image where the target is 1. If the patientId has a target of 0, it creates an empty text file. 

The code then generates train.txt and test.txt files containing the paths to the training and testing images. We also create a classes.names file listing the class names and an image_data.data file specifying the number of classes, paths to the training and validation text files, class names file, and backup directory. 
