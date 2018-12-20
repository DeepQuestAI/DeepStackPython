.. DeepStack documentation master file, created by
   sphinx-quickstart on Wed Dec 12 17:30:35 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Object Detection!
==================

The object detection api locates and classifies 80 different kinds of objects in a single image.

**Example**

.. figure:: test-image3.jpg
    :align: center

::

    import requests
    
    image_data = open("test-image3.jpg","rb").read()
    
    response = requests.post("http://localhost:80/v1/vision/detection",files={"image":image_data}).json()
    
    for object in response["predictions"]:
        print(object["label"])
    
    print(response)

Result ::

    dog
    person
    person
    {'predictions': [{'x_max': 819, 'x_min': 633, 'y_min': 354, 'confidence': 99, 'label': 'dog', 'y_max': 546}, {'x_max': 601, 'x_min': 440, 'y_min': 116, 'confidence': 99, 'label': 'person', 'y_max': 516}, {'x_max': 445, 'x_min': 295, 'y_min': 84, 'confidence': 99, 'label': 'person', 'y_max': 514}], 'success': True}

We can use the coordinates returned to extract the objects

::

    import requests
    from PIL import Image

    image_data = open("test-image3.jpg","rb").read()
    image = Image.open("test-image3.jpg").convert("RGB")

    response = requests.post("http://localhost:80/v1/vision/detection",files={"image":image_data}).json()
    i = 0
    for object in response["predictions"]:

        label = object["label"]
        y_max = int(object["y_max"])
        y_min = int(object["y_min"])
        x_max = int(object["x_max"])
        x_min = int(object["x_min"])
        cropped = image.crop((x_min,y_min,x_max,y_max))
        cropped.save("image{}_{}.jpg".format(i,label))

        i += 1

Result

.. figure:: image0_dog.jpg
    :align: center

.. figure:: image1_person.jpg
    :align: center

.. figure:: image2_person.jpg
    :align: center

**DETECTION SPEED**

DeepStack offers three modes for object detection, the default mode is 
"LOW" offering good performance at maximum speed, the second mode is "MEDIUM"
while the third is "HIGH". The HIGH mode is much slower but offers the best 
performance. It is able to detect smaller details which the "LOW" and "MEDIUM" 
modes may failt to detect.

You can specify the mode in the run command for DeepStack has seen below ::

    sudo docker run -e MODE=HIGH -v localstorage:/datastore -p 80:5000 deepquestai/deepstack 
