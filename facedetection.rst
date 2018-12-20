.. DeepStack documentation master file, created by
   sphinx-quickstart on Wed Dec 12 17:30:35 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Face Detection!
==================

The face detection api detects faces and returns their coordinates as well as the gender.
It functions similarly to the face recognition api except that it does not 
perform recognition. 
Also note that the recognition api does not return gender predictions.

**Example**

.. figure:: family.jpg
    :align: center

::

    import requests
    
    image_data = open("family.jpg","rb").read()
    
    response = requests.post("http://localhost:80/v1/vision/face",files={"image":image_data}).json()
    
    for person in response["predictions"]:
        print(person["gender"])
    
    print(response)

Result ::

    female
    male
    male
    female
    {'predictions': [{'y_max': 303, 'gender': 'female', 'confidence': 100, 'x_min': 534, 'x_max': 629, 'y_min': 174}, {'y_max': 275, 'gender': 'male', 'confidence': 99, 'x_min': 616, 'x_max': 711, 'y_min': 146}, {'y_max': 259, 'gender': 'male', 'confidence': 98, 'x_min': 729, 'x_max': 811, 'y_min': 147}, {'y_max': 290, 'gender': 'female', 'confidence': 99, 'x_min': 471, 'x_max': 549, 'y_min': 190}], 'success': True}

We can use the coordinates returned to extract the faces from the image

::

    import requests
    from PIL import Image

    image_data = open("family.jpg","rb").read()
    image = Image.open("family.jpg").convert("RGB")

    response = requests.post("http://localhost:80/v1/vision/face",files={"image":image_data}).json()
    i = 0
    for face in response["predictions"]:
        
        gender = face["gender"]
        y_max = int(face["y_max"])
        y_min = int(face["y_min"])
        x_max = int(face["x_max"])
        x_min = int(face["x_min"])
        cropped = image.crop((x_min,y_min,x_max,y_max))
        cropped.save("image{}_{}.jpg".format(i,gender))

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