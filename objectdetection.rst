.. DeepStack documentation master file, created by
   sphinx-quickstart on Wed Dec 12 17:30:35 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Object Detection
=================

The object detection API locates and classifies 80 different kinds of objects in a single image.

To use this API, you need to set **VISION-DETECTION=True** when starting DeepStack ::

    sudo docker run -e VISION-DETECTION=True -v localstorage:/datastore \
    -p 80:5000 deepquestai/deepstack

If using the GPU Version, run ::

    sudo docker run --rm --runtime=nvidia -e VISION-DETECTION=True -v localstorage:/datastore \
    -p 80:5000 deepquestai/deepstack:gpu

*Note also that you can have multiple endpoints activated, for example, both face and object detection are activated below* ::

    sudo docker run -e VISION-DETECTION=True  -e VISION-FACE=True -v localstorage:/datastore \
    -p 80:5000 deepquestai/deepstack



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


**CLASSES**

The following are the classes of objects DeepStack can detect in images ::

    person,   bicycle,   car,   motorcycle,   airplane,
    bus,   train,   truck,   boat,   traffic light,   fire hydrant,   stop_sign,
    parking meter,   bench,   bird,   cat,   dog,   horse,   sheep,   cow,   elephant,  
    bear,   zebra, giraffe,   backpack,   umbrella,   handbag,   tie,   suitcase,   
    frisbee,   skis,   snowboard, sports ball,   kite,   baseball bat,   baseball glove, 
    skateboard,   surfboard,   tennis racket, bottle,   wine glass,   cup,   fork,   
    knife,   spoon,   bowl,   banana,   apple,   sandwich,   orange, broccoli,   carrot,   
    hot dog,   pizza,   donot,   cake,   chair,   couch,   potted plant,   bed, dining table,   
    toilet,   tv,   laptop,   mouse,   remote,   keyboard,   cell phone,   microwave,
    oven,   toaster,   sink,   refrigerator,   book,   clock,   vase,   scissors,   teddy bear,
    hair dryer, toothbrush.
