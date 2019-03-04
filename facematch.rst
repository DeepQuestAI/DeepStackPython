.. DeepStack documentation master file, created by
   sphinx-quickstart on Wed Dec 12 17:30:35 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. _facematch:

Face Match
===========

The face detection api compares faces in two different pictures and tells the similarity between them.
A typical use of this is matching identity documents with pictures of a person.


**Example**

Here we shall compare two pictures of obama

.. figure:: test-image6.jpeg
    :align: center

.. figure:: test-image7.jpg
    :align: center

::

    import requests
    
    image_data1 = open("test-image6.jpeg","rb").read()
    image_data2 = open("test-image7.jpg","rb").read()
    
    response = requests.post("http://localhost:80/v1/vision/face/match",files={"image1":image_data1,"image2":image_data2}).json()
    
    print(response)

Result ::

    {'similarity': 0.73975885, 'success': True}

Here we shall compare a picture of Obama with that of Bradley Cooper

.. figure:: test-image6.jpeg
    :align: center

.. figure:: test-image8.jpg
    :align: center

::

    import requests
    
    image_data1 = open("test-image6.jpeg","rb").read()
    image_data2 = open("test-image8.jpg","rb").read()
    
    response = requests.post("http://localhost:80/v1/vision/face/match",files={"image1":image_data1,"image2":image_data2}).json()
    
    print(response)

Result ::

    {{'similarity': 0.4456827, 'success': True}

As seen above, the match for two different pictures of Obama was very high while the match for Obama and Bradley Cooper was very low.

**Performance**

DeepStack offers three modes allowing you to tradeoff speed for peformance. 
During startup, you can specify performance mode to be , **"High" , "Medium" and "Low"**

The default mode is "Medium"

You can speciy a different mode as seen below ::

    sudo docker run -e MODE=High -e VISION-FACE=True -v localstorage:/datastore \
    -p 80:5000 deepquestai/deepstack

Note the -**e MODE=High** above 