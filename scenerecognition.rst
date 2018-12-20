.. DeepStack documentation master file, created by
   sphinx-quickstart on Wed Dec 12 17:30:35 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Scene Recognition
====================

The traffic recognition api classifies an image into one of 365 scenes


**Example**

.. figure:: test-image5.jpg
    :align: center

::

    import requests
    
    image_data = open("test-image5.jpg","rb").read()
    
    response = requests.post("http://localhost:80/v1/vision/scene",files={"image":image_data}).json()
    
    for prediction in response["predictions"]:
        print(prediction["label"])
    
    print(response)

Result ::

    conference_room
    {'success': True, 'predictions': [{'label': 'conference_room', 'confidence': 73.73981475830078}]}
