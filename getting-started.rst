.. DeepStack documentation master file, created by
   sphinx-quickstart on Wed Dec 12 17:30:35 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Getting Started with DeepStack
===============================

DeepStack is distributed as a docker image. In this tutorial, we shall 
go through the complete process of setting up docker and DeepStack and using it
to build a Face Recognition system.

**Setting Up Docker** 

Docker is a container platform that allows developers to distribute applications
as self-contained packages that ships every dependency from the
operating system to the app dependences. It is similar to virtual machines
but is more lightweight and easier to manage. 

You can learn more about Docker on `Docker's Website <https://docker.io />`_
Visit  `Docker Getting Started <https://docs.docker.com/get-started />`_ for instructions on setting up and using Docker for the first time.

**Installing DeepStack**

DeepStack is an AI Server that provides AI features as APIs consumable via basic web requests.
It works entirely offline and can be installed anywhere docker runs both on premise and in the cloud.

DeepStack is developed and maintained by `DeepQuest AI <https://deepquestai.com />`_

Run the command below to install DeepStack ::
    
    docker pull deepquestai/deepstack

To install the GPU Accelerated Version, follow :ref:`gpuinstall`

**Starting DeepStack**

Below we shall run DeepStack with only the FACE features enabled ::

    sudo docker run -e VISION-FACE=True -v localstorage:/datastore -p 80:5000 deepquestai/deepstack

*Basic Parameters*

    **-e VISION-FACE=True** This enables the face recognition APIs, all apis are disabled by default.

    **-v localstorage:/datastore** This specifies the local volume where deepstack will store all data.

    **-p 80:5000** This makes deepstack accessible via port 80 of the machine.

**NOTE FOR THE GPU VERSION**

    If you installed the GPU Version, remmember to add the args args **--rm --runtime=nvidia** 
    The equivalent run command for the gpu version is ::

        sudo docker run --rm --runtime=nvidia -e VISION-FACE=True -v localstorage:/datastore \
        -p 80:5000 deepquestai/deepstack:gpu
    


**Face Recognition**
    
    Think of a software that can identity known people by their names. Face Recognition does exactly that. Register a picture of a number of people
    and the system will be able to recognize them again anytime.
    Face Recognition is a two step process: The first is to register a known face and second is to recognize unknown faces.

**REGISTERING A FACE**
    
    Here we are building an application that can tell the names of a number of popular celebrities.
    First we collect pictures of a number of celebrities and we register them with deepstack


    .. figure:: cruise.jpg
        :align:  left

    .. figure:: adele.jpg
        :align:  left

    .. figure:: elba.jpg
        :align:  left

    .. figure:: perri.jpg
        :align:  left

Below we will register the faces with their names ::
    
    import requests

    tom_cruise = open("cruise.jpg","rb").read()
    adele = open("adele.jpg","rb").read()
    elba = open("elba.jpg","rb").read()
    perri = open("perri.jpg","rb").read()
    
    requests.post("http://localhost:80/v1/vision/face/register",files={"image":tom_cruise}, data={"userid":"Tom Cruise"})
    requests.post("http://localhost:80/v1/vision/face/register",files={"image":adele}, data={"userid":"Adele"})
    requests.post("http://localhost:80/v1/vision/face/register",files={"image":elba}, data={"userid":"Idris Elba"})
    requests.post("http://localhost:80/v1/vision/face/register",files={"image":perri}, data={"userid":"Christina Perri"})

**RECOGNITION**

Now we shall attempt to recognize any of these celebrities using DeepStack.
Below we will send in a whole new picture of Adele and DeepStack will attempt to
predict the name.

.. figure:: test-image.jpg
    :align:  left
    

Prediction code ::

    import requests

    test_image = open("test-image.jpg","rb").read()
   
    res = requests.post("http://localhost:80/v1/vision/face/recognize",files={"image":test_image}).json()

    for user in res["predictions"]:
        print(user["userid"])

Result ::

    Adele
 
.. toctree::
   :maxdepth: 2
   :caption: Contents:

We have just created a face recognition system. You can try with different people and test on different pictures of them.

The next tutorial is dedicated to the full power of the face recognition api as well as best practices to make the best out of it.

