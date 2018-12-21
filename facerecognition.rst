.. DeepStack documentation master file, created by
   sphinx-quickstart on Wed Dec 12 17:30:35 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Face Recognition
=================

In the Getting Started, we had an overview of the face recognition api. In this section, we shall explore all the functionalities 
of the api.

**Face Registeration** 

The face registeration endpoint allows you to register pictures of person and associate it with a userid.

You can specify multiple pictures per person during registeration.

Example ::

    import requests
    
    user_image = open("image1.jpg","rb").read()
    
    response = requests.post("http://localhost:80/v1/vision/face/register",files={"image":user_image},data={"userid":"User Name"}).json()

    print(response)

Result ::

    {'predictions': {'message': 'face added'}, 'success': True}

The response above indicates the call was successful. You should always check for the "success" status.
If their is an error in your request, you will receive a response like ::

    {'error': 'user id not specified', 'success': False}

This indicates that you ommited the userid in your request.
If you ommited the image, the response will be ::

    {'error': 'No valid image file found', 'success': False}



**FACE RECOGNITION**
    
The face registeration endpoint detects all faces in an image and returns the USERID for each face. Note that the USERID was specified
during the registeration phase. If a new face is encountered, the USERID will be unknown. 

We shall test this on the image below.

.. figure:: test-image2.jpg
    :align: center
    


    
::
    
    import requests

    image_data = open("test-image2.jpg","rb").read()

    response = requests.post("http://localhost:80/v1/vision/face/recognize",files={"image":image_data}).json()
    
    for user in response["predictions"]:
        print(user["userid"])

    print("Full Response: ",response)

Result  ::

    unknown
    Idris Elba
    Full Response:  {'predictions': [{'userid': 'unknown', 'y_max': 827, 'x_min': 850, 'y_min': 233, 'x_max': 1290}, {'x_min': 1577, 'confidence': 0.6002725154510684, 'userid': 'Idris Elba', 'y_max': 767, 'y_min': 160, 'x_max': 2041}], 'success': True}

As you can see above, the first user is unknown since we did not previously register her, however, Idris Elba was detected as we
registered a picture of his in the previous tutorial.
Note also that the full response contains the coordinates of the faces.


**Setting Face Distance**

DeepStack recognizes faces by computing the distance between a new face and set of embeddings of previously registered faces.
By default, if the distance is as low as 0.45, the user will be recognized, however, you can increase or decrease the distance
using the "min_distance" parameter.

Example ::

    import requests

    image_data = open("test-image2.jpg","rb").read()

    response = requests.post("http://localhost:80/v1/vision/face/recognize",files={"image":image_data},data={"min_distance":0.8}).json()
    
    for user in response["predictions"]:
        print(user["userid"])

    print("Full Response: ",response)

Result ::

    Christina Perri
    Idris Elba
    Full Response:  {'predictions': [{'y_min': 233, 'x_max': 1290, 'userid': 'Christina Perri', 'x_min': 850, 'y_max': 827, 'confidence': 0.2758451809953383}, {'y_min': 160, 'x_max': 2041, 'userid': 'Idris Elba', 'x_min': 1577, 'y_max': 767, 'confidence': 0.6002725154510684}], 'success': True}

By increasing the distance allowed, the system detects the first user as "Christina Perri".
The lower the distance, the more accurate the system is. However, if the distance is too low, the system may not detect any user.


**USER MANAGEMENT**

The face recognition api allows you to retrieve and delete faces
that has been previously registered with DeepStack.

Listing faces ::

    import requests
    faces = requests.post("http://localhost:80/v1/vision/face/list").json()

    print(faces)

Result ::

    {'success': True, 'faces': ['Tom Cruise', 'Adele', 'Idris Elba', 'Christina Perri']}


Deleting a face ::

    import requests

    response = requests.post("http://localhost:80/v1/vision/face/delete",data={"userid":"Idris Elba"}).json()

    print(response)

Result ::

    {'success': True, 'message': 'user deleted successfully'}

Having deleted Idris Elba from our database, we shall now attempt to recognize him
in our test image.


:: 

    import requests

    image_data = open("test-image2.jpg","rb").read()

    response = requests.post("http://localhost:80/v1/vision/face/recognize",files={"image":image_data}).json()
    
    for user in response["predictions"]:
        print(user["userid"])

Result ::

    unknown
    unknown
