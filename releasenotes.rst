.. DeepStack documentation master file, created by
   sphinx-quickstart on Wed Dec 12 17:30:35 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

DeepStack Beta 2.0 - Release Notes
==================================

DeepStack Beta 2.0 features a new face detection engine, significantly improving the face detection and recognition APIS.

Improvements
-------------

    * **New Face Detection Engine**

        The face detection APIs have been improved to detect faces even when occluded.

    * **Improved Face Recognition**

        The face recognition APIs have been improved significantly. Recognized faces now report confidence over 70%.
        The default min_confidence is now set to 0.67
    
    * **New Face Match API**

        The new :ref:`facematch` API allows you to compute the similarity score on two images containing two faces.
    
    * **Speed Modes**

        Speed modes have been introduced to allow you easily tradeoff performance for accuracy.

        There are three speed modes, **"High" , "Medium" and "Low"**
        
        You can specify your speed mode has exemplified below. ::

            sudo docker run -e MODE=High -e VISION-DETECTION=True -v localstorage:/datastore \
            -p 80:5000 deepquestai/deepstack

            Note the -**e MODE=High** above 

    * **Minimum Confidence**

        The "min_confidence" parameter allows you to control the level of confidence of results for :ref:`objectdetection` , :ref:`facedetection` and :ref:`facerecognition`


Breaking Changes
----------------

    * **TRAFFIC API REMOVED**

        The traffic api has been removed, a more improved version maybe re-introduced in future versions of DeepStack.
        
    * **GENDER API REMOVED**

        The face detection api no longer return gender information, only bounding boxes are now returned. Gender prediction maybe 
        re-introduced in future versions of DeepStack..


DeepStack Beta - Release Notes
==============================

.. _releasenotes:

DeepStack Beta is more optimized for storage, memory, compute and is more accurate.

Improvements
-------------

    * **New Fast GPU Version**

        We have released the GPU Version of DeepStack, accelerating responses by orders of magnitude if you have
        an Nvidia GPU. See :ref:`gpuinstall` for setup instructions
    
    * **Improved Face Recognition Engine**

        The face recognition API is now powered by state-of-the-art face recognition engine.

    * **Better error handling and crash recovery**

        DeepStack now gracefully handles invalid images and reports errors better.

    * **50% Reduction in Install Size**

        The total install size DeepStack is now half the original size.
    
Breaking Changes
----------------

    * **ENDPOINT ACTIVATION**

        To avoid unneccesary memory usage. Only endpoints you activate are loaded.

        For example, to use any face API, you should run deepstack as below ::

            sudo docker run -e VISION-FACE=True -v localstorage:/datastore \
            -p 80:5000 deepquestai/deepstack

        In this example, you can query all face related APIs, however, you cannot query the object detection API and other endpoints.

        You can activate multiple endpoints at once as below ::

            sudo docker run -e VISION-FACE=True -e VISION-SCENE=True -e VISION-TRAFFIC=True \
            -v localstorage:/datastore -p 80:5000 deepquestai/deepstack
    
    * **FACE RECOGNITION API-MINIMUM CONFIDENCE**

        The *min_distance* parameter has been replaced by *min_confidence*
        See :ref:`facerecognition` for usage instructions

    * **FACE RECOGNITION API - DATA INCOMPATIBILITY**

        Any faces registered with the Alpha Version is incompatible with this Version.
        You need to re-register previously registered faces.
        The new face recognition engine is stable and future releases will remain compatible with
        this Version.

    * **FACE RECOGNITION API - RESPONSES**

        Face registeration response has been changed from ::

            {'predictions': {'message': 'face added'}, 'success': True}

        To ::

            {'message': 'face added', 'success': True}

        
        Face delete response has been changed from ::

            {'success': True, 'message': 'user deleted successfully'}

        To ::

            {'success': True}

    * **SCENE AND TRAFFIC API - RESPONSES** 

        Responses has been changed from ::

            {'success': True, 'predictions': [{'label': 'conference_room', 'confidence': 73.73981475830078}]}

        To ::

            {'success': True, 'confidence': 73.73981, 'label': 'conference_room'}

    * **OBJECT DECTION API - SPEED MODES**

        Speed Modes have been deprecated.


