.. DeepStack documentation master file, created by
   sphinx-quickstart on Wed Dec 12 17:30:35 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Scene Recognition
====================

The traffic recognition api classifies an image into one of 365 scenes


To use this API, you need to set **VISION-SCENE=True** when starting DeepStack ::

    sudo docker run -e VISION-SCENE=True -v localstorage:/datastore \
    -p 80:5000 deepquestai/deepstack

If using the GPU Version, run ::

    sudo docker run --rm --runtime=nvidia -e VISION-SCENE=True -v localstorage:/datastore \
    -p 80:5000 deepquestai/deepstack:gpu

*Note also that you can have multiple endpoints activated, for example, both traffic and scene recognition are activated below* ::

    sudo docker run -e VISION-SCENE=True  -e VISION-TRAFFIC=True -v localstorage:/datastore \
    -p 80:5000 deepquestai/deepstack


**Example**

.. figure:: test-image5.jpg
    :align: center

::

    import requests
    
    image_data = open("test-image5.jpg","rb").read()
    
    response = requests.post("http://localhost:80/v1/vision/scene",files={"image":image_data}).json()
    print("Label:",response["label"])
    print(response)

Result ::

    {'confidence': 0.73739815, 'label': 'conference_room', 'success': True}


