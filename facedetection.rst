.. DeepStack documentation master file, created by
   sphinx-quickstart on Wed Dec 12 17:30:35 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

.. _facedetection:

Face Detection
==============

The face detection API detects faces and returns their coordinates.
It functions similarly to the face recognition API except that it does not 
perform recognition. 


**Example**

.. figure:: family.jpg
    :align: center

::

    import requests
    
    image_data = open("family.jpg","rb").read()
    
    response = requests.post("http://localhost:80/v1/vision/face",files={"image":image_data}).json()
    
    print(response)

Result ::

    {'predictions': [{'x_max': 712, 'y_max': 261, 'x_min': 626, 'confidence': 0.99990666, 'y_min': 145}, {'x_max': 620, 'y_max': 288, 'x_min': 543, 'confidence': 0.99986553, 'y_min': 174}, {'x_max': 810, 'y_max': 242, 'x_min': 731, 'confidence': 0.99986434, 'y_min': 163}, {'x_max': 542, 'y_max': 279, 'x_min': 477, 'confidence': 0.99899536, 'y_min': 197}], 'success': True}

We can use the coordinates returned to extract the faces from the image

::

    import requests
    from PIL import Image

    image_data = open("family.jpg","rb").read()
    image = Image.open("family.jpg").convert("RGB")

    response = requests.post("http://localhost:80/v1/vision/face",files={"image":image_data}).json()
    i = 0
    for face in response["predictions"]:
        
        y_max = int(face["y_max"])
        y_min = int(face["y_min"])
        x_max = int(face["x_max"])
        x_min = int(face["x_min"])
        cropped = image.crop((x_min,y_min,x_max,y_max))
        cropped.save("image{}.jpg".format(i))

        i += 1

Result

.. figure:: image0_female.jpg
    :align: center

.. figure:: image1_male.jpg
    :align: center

.. figure:: image2_male.jpg
    :align: center

.. figure:: image3_female.jpg
    :align: center


**Performance**

DeepStack offers three modes allowing you to tradeoff speed for peformance. 
During startup, you can specify performance mode to be , **"High" , "Medium" and "Low"**

The default mode is "Medium"

You can speciy a different mode as seen below ::

    sudo docker run -e MODE=High -e VISION-FACE=True -v localstorage:/datastore \
    -p 80:5000 deepquestai/deepstack

Note the -**e MODE=High** above 


**Setting Minimum Confidence**

By default, the minimum confidence for detecting faces is 0.45. The confidence ranges between 0 and 1.
If the confidence level for a face falls below the min_confidence, no face is detected.

The min_confidence parameter allows you to increase or reduce the minimum confidence.

We lower the confidence allowed below.

Example ::

    import requests

    image_data = open("family.jpg","rb").read()

    response = requests.post("http://localhost:80/v1/vision/face",
    files={"image":image_data},data={"min_confidence":0.30}).json()