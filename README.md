# EL-9123 Final Project : Facial Recognition


In this repository, you can find 
```
1. All the code that is required to do this project 
```
```
2. result.txt: the result of the project
```
```
3. ML_final_project.pdf: the report for this project. So in this readme file, on instruction about downloading dataset and configuration of Docker will be provided.
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
```
# Make sure to put all the image folder under <data> folder. A <data> folder can be created by yourself.
curl -O http://vis-www.cs.umass.edu/lfw/lfw.tgz
tar -xzvf lfw.tgz
```
