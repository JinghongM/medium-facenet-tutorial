# EL-9123 Final Project : Facial Recognition

In the project, we built a Convolution Neural Networking with a SVM classification network to implement facial recognition. The environment is built in the Docker. The dataset is LFW. The preprocessing is used from CMU. And the highest accuracy rate is 91%.

In this repository, you can find 
```
1. All the code that is required to do this project 
```
```
2. result.txt: the result of the project
```
```
3. ML_final_project.pdf: the report for this project. So in this readme file, instruction about downloading dataset and configuration of Docker will be provided.
```


# Instruction for downloading dataset and setup the environment
## The project structure
```
├── Dockerfile
├── etc
│ ├── 20170511–185253
│ │ ├── 20170511–185253.pb
├── data
├── medium_facenet_tutorial
│ ├── align_dlib.py
│ ├── download_and_extract_model.py
│ ├── __init__.py
│ ├── lfw_input.py
│ ├── preprocess.py
│ ├── shape_predictor_68_face_landmarks.dat
│ └── train_classifier.py
├── requirements.txt


```
## Downlaod Lfw dataset
The lfw(Labeled Faces in the Wild) is a database of face photographs designed for face recognition which contains more than 13,000 images of faces collected from the lab. The label of each image is the name of folder, which is the same we did in Lab 8.
```
# Make sure to put all the image folder under <data> folder. A <data> folder can be created by yourself.
curl -O http://vis-www.cs.umass.edu/lfw/lfw.tgz
tar -xzvf lfw.tgz
```

## Docker environment
Because of the huge amount of packets we need in the project, we use a docker to finish the prediction. The docker image can be found online.
```
# Make sure you assign enough cores to the VM so that this process can be faster.
curl https://get.docker.com | sh
docker build -t colemurray/medium-facenet-tutorial -f Dockerfile .
```

## Download Dlib landmark predictor
```
curl -O http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2
bzip2 -d shape_predictor_68_facelandmarks.dat.bz2
```

## Preprocess
We use the downloaded landmark predictor before to preprocessing the image.
Before passing images into the CNN model, we use a preprocessing filter from CMU to detect a face in the image and find the largest face part in the image and center the filtered part. If a face can't be found in the image, a logger will be displayed in the console.
```
docker run -v $PWD:/medium-facenet-tutorial \
-e PYTHONPATH=$PYTHONPATH:/medium-facenet-tutorial \
-it colemurray/medium-facenet-tutorial python3 /medium-facenet-tutorial/medium_facenet_tutorial/preprocess.py \
--input-dir /medium-facenet-tutorial/data \
--output-dir /medium-facenet-tutorial/output/intermediate \
--crop-dim 180
```
```
docker run -v $PWD:/medium-facenet-tutorial \
-e PYTHONPATH=$PYTHONPATH:/medium-facenet-tutorial \
-it colemurray/medium-facenet-tutorial python3 /medium-facenet-tutorial/medium_facenet_tutorial/download_and_extract_model.py \
--model-dir /medium-facenet-tutorial/etc
```

## Train the classifier
Before the images per figure in the dataset are only 1-5 pictures. The classifier will randomly create multiply training sets by flipping left and right, changing the brightness and contrast of the original images in order to avoid the overfitting.
We also add another processing by random flip the training set up and down.
```
docker run -v $PWD:/medium-facenet-tutorial \
-e PYTHONPATH=$PYTHONPATH:/medium-facenet-tutorial \
-it colemurray/medium-facenet-tutorial \
python3 /medium-facenet-tutorial/medium_facenet_tutorial/train_classifier.py \
--input-dir /medium-facenet-tutorial/output/intermediate \
--model-path /medium-facenet-tutorial/etc/20170511-185253/20170511-185253.pb \
--classifier-path /medium-facenet-tutorial/output/classifier.pkl \
--num-threads 16 \
--num-epochs 25 \
--min-num-images-per-class 10 \
--is-train 
```

## Test the classifier
In this project, we use the original training dataset as the test sets. The best accuracy rate is 91% by using 5 epochs, 256 batches
```
docker run -v $PWD:/medium-facenet-tutorial \
-e PYTHONPATH=$PYTHONPATH:/medium-facenet-tutorial \
-it colemurray/medium-facenet-tutorial \
python3 /medium-facenet-tutorial/medium_facenet_tutorial/train_classifier.py \
--input-dir /medium-facenet-tutorial/output/intermediate \
--model-path /medium-facenet-tutorial/etc/20170511-185253/20170511-185253.pb \
--classifier-path /medium-facenet-tutorial/output/classifier.pkl \
--num-threads 16 \
--num-epochs 25 \
--min-num-images-per-class 10 \
```


# Reference
The foundation of this project and some of the parameters tuned in this project is based on 
* [FaceNet](http://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=7298682)
