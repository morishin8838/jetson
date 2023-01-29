# Tensorflow
    
## Tensorflow

```
$apt update
$apt-get install git
$git clone https://github.com/tensorflow/models.git

cd /models/research/object_detection/
git clone https://github.com/EdjeElectronics/TensorFlow-Object-Detection-API-Tutorial-Train-Multiple-Objects-Windows-10.git
cp -r TensorFlow-Object-Detection-API-Tutorial-Train-Multiple-Objects-Windows-10/* .
rm -rf TensorFlow-Object-Detection-API-Tutorial-Train-Multiple-Objects-Windows-10/  


alias python=python3
wget https://hs.forecr.io/hubfs/tf_mask_model.zip
apt update 
apt install unzip
unzip tf_mask_model.zip


mv frozen_inference_graph.pb ./inference_graph/
mv labelmap.pbtxt ./training/

wget https://hs.forecr.io/hubfs/video.mp4

sed -i ‘s/test.mov/video.mp4/g’ Object_detection_video.py


```

