#Face Recognition with OpenCV and Python
##Introduction
What is face recognition? Or what is recognition? When you look at an apple fruit, your mind immediately tells you that this is an apple fruit. This process, your mind telling you that this is an apple fruit is recognition in simple words. So what is face recognition then? I am sure you have guessed it right. When you look at your friend walking down the street or a picture of him, you recognize that he is your friend Paulo. Interestingly when you look at your friend or a picture of him you look at his face first before looking at anything else. Ever wondered why you do that? This is so that you can recognize him by looking at his face. Well, this is you doing face recognition.

But the real question is how does face recognition works? It is quite simple and intuitive. Take a real life example, when you meet someone first time in your life you don't recognize him, right? While he talks or shakes hands with you, you look at his face, eyes, nose, mouth, color and overall look. This is your mind learning or training for the face recognition of that person by gathering face data. Then he tells you that his name is Paulo. At this point your mind knows that the face data it just learned belongs to Paulo. Now your mind is trained and ready to do face recognition on Paulo's face. Next time when you will see Paulo or his face in a picture you will immediately recognize him. This is how face recognition work. The more you will meet Paulo, the more data your mind will collect about Paulo and especially his face and the better you will become at recognizing him.

Now the next question is how to code face recognition with OpenCV, after all this is the only reason why you are reading this article, right? OK then. You might say that our mind can do these things easily but to actually code them into a computer is difficult? Don't worry, it is not. Thanks to OpenCV, coding face recognition is as easier as it feels. The coding steps for face recognition are same as we discussed it in real life example above.

Training Data Gathering: Gather face data (face images in this case) of the persons you want to recognize
Training of Recognizer: Feed that face data (and respective names of each face) to the face recognizer so that it can learn.
Recognition: Feed new faces of the persons and see if the face recognizer you just trained recognizes them.
OpenCV comes equipped with built in face recognizer, all you have to do is feed it the face data. It's that simple and this how it will look once we are done coding it.
# OpenCV Face Recognizers
OpenCV has three built in face recognizers and thanks to OpenCV's clean coding, you can use any of them by just changing a single line of code. Below are the names of those face recognizers and their OpenCV calls.

EigenFaces Face Recognizer Recognizer - cv2.face.createEigenFaceRecognizer()
FisherFaces Face Recognizer Recognizer - cv2.face.createFisherFaceRecognizer()
Local Binary Patterns Histograms (LBPH) Face Recognizer - cv2.face.createLBPHFaceRecognizer()
We have got three face recognizers but do you know which one to use and when? Or which one is better? I guess not. So why not go through a brief summary of each, what you say? I am assuming you said yes :) So let's dive into the theory of each.
EigenFaces Face Recognizer
This algorithm considers the fact that not all parts of a face are equally important and equally useful. When you look at some one you recognize him/her by his distinct features like eyes, nose, cheeks, forehead and how they vary with respect to each other. So you are actually focusing on the areas of maximum change (mathematically speaking, this change is variance) of the face. For example, from eyes to nose there is a significant change and same is the case from nose to mouth. When you look at multiple faces you compare them by looking at these parts of the faces because these parts are the most useful and important components of a face. Important because they catch the maximum change among faces, change the helps you differentiate one face from the other. This is exactly how EigenFaces face recognizer works.

# EigenFaces face recognizer
looks at all the training images of all the persons as a whole and try to extract the components which are important and useful (the components that catch the maximum variance/change) and discards the rest of the components. This way it not only extracts the important components from the training data but also saves memory by discarding the less important components. These important components it extracts are called principal components. Below is an image showing the principal components extracted from a list of faces.
# Local Binary Patterns Histograms (LBPH) Face Recognizer
I wrote a detailed explaination on Local Binary Patterns Histograms in my previous article on face detection using local binary patterns histograms. So here I will just give a brief overview of how it works.

We know that Eigenfaces and Fisherfaces are both affected by light and in real life we can't guarantee perfect light conditions. LBPH face recognizer is an improvement to overcome this drawback.

Idea is to not look at the image as a whole instead find the local features of an image. LBPH alogrithm try to find the local structure of an image and it does that by comparing each pixel with its neighboring pixels.

Take a 3x3 window and move it one image, at each move (each local part of an image), compare the pixel at the center with its neighbor pixels. The neighbors with intensity value less than or equal to center pixel are denoted by 1 and others by 0. Then you read these 0/1 values under 3x3 window in a clockwise order and you will have a binary pattern like 11100011 and this pattern is local to some area of the image. You do this on whole image and you will have a list of local binary patterns.

# Coding Face Recognition with OpenCV
The Face Recognition process in this tutorial is divided into three steps.

Prepare training data: In this step we will read training images for each person/subject along with their labels, detect faces from each image and assign each detected face an integer label of the person it belongs to.
Train Face Recognizer: In this step we will train OpenCV's LBPH face recognizer by feeding it the data we prepared in step 1.
Testing: In this step we will pass some test images to face recognizer and see if it predicts them correctly.
[There should be a visualization diagram for above steps here]

To detect faces, I will use the code from my previous article on face detection. So if you have not read it, I encourage you to do so to understand how face detection works and its Python coding.
Import Required Modules
Before starting the actual coding we need to import the required modules for coding. So let's import them first.

cv2: is OpenCV module for Python which we will use for face detection and face recognition.

Face Recognition is a fascinating idea to work on and OpenCV has made it extremely simple and easy for us to code it. It just takes a few lines of code to have a fully working face recognition application and we can switch between all three face recognizers with a single line of code change. It's that simple.

Although EigenFaces, FisherFaces and LBPH face recognizers are good but there are even better ways to perform face recognition like using Histogram of Oriented Gradients (HOGs) and Neural Networks. So the more advanced face recognition algorithms are now a days implemented using a combination of OpenCV and Machine learning. I have plans to write some articles on those more advanced methods as well, so stay tuned!
os: We will use this Python module to read our training directories and file names.
numpy: We will use this module to convert Python lists to numpy arrays as OpenCV face recognizers accept numpy arrays.
# Training Data
The more images used in training the better. Normally a lot of images are used for training a face recognizer so that it can learn different looks of the same person, for example with glasses, without glasses, laughing, sad, happy, crying, with beard, without beard etc. To keep our tutorial simple we are going to use only 12 images for each person.

So our training data consists of total 2 persons with 12 images of each person. All training data is inside training-data folder. training-data folder contains one folder for each person and each folder is named with format sLabel (e.g. s1, s2) where label is actually the integer label assigned to that person. For example folder named s1 means that this folder contains images for person 1. The directory structure tree for training data is as follows:
The test-data folder contains images that we will use to test our face recognizer after it has been successfully trained.

As OpenCV face recognizer accepts labels as integers so we need to define a mapping between integer labels and persons actual names so below I am defining a mapping of persons integer labels and their respective names.

Note: As we have not assigned label 0 to any person so the mapping for label 0 is empty.
