.. DeepStack documentation master file, created by
   sphinx-quickstart on Wed Dec 12 17:30:35 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Traffic Recognition
====================

The traffic recognition API classifies an image into one of the following ::

    Sparse traffic

    Dense traffic

    Accident

    Fire

With this, from images of live traffic, you can tell if an accident has occured,
if their is traffic gridlock or if a vehicle is on fire.


To use this API, you need to set **VISION-TRAFFIC=True** when starting DeepStack ::

    sudo docker run -e VISION-TRAFFIC=True -v localstorage:/datastore \
    -p 80:5000 deepquestai/deepstack

If using the GPU Version, run ::

    sudo docker run --rm --runtime=nvidia -e VISION-TRAFFIC=True -v localstorage:/datastore \
    -p 80:5000 deepquestai/deepstack:gpu

*Note also that you can have multiple endpoints activated, for example, both traffic and object detection are activated below* ::

    sudo docker run -e VISION-TRAFFIC=True  -e VISION-DETECTION=True -v localstorage:/datastore \
    -p 80:5000 deepquestai/deepstack



**Example**

.. figure:: test-image4.jpg
    :align: center

::

    import requests
    
    image_data = open("test-image4.jpg","rb").read()
    
    response = requests.post("http://localhost:80/v1/vision/traffic",files={"image":image_data}).json()
    print("Label",response["label"])
    print(response)

Result ::

    Label accident
    {'success': True, 'label': 'accident', 'confidence': 0.9488776}

