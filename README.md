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

## Docker environment
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
* [Facenet](http://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=7298682)
