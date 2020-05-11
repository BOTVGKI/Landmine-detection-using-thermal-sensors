# Landmine-detection-using-thermal-sensors
We tried to detect landmine from thermal images using image processing and deep learning techniques.

We tried using multi-scale template matching for detection of landmines.The idea of using this technique strikes into our mind due to the nature of landmines having regular shape in different sizes.Template matching (TM) is a straightforward and simple object detection method in image analysis. It is based on a model of the object that we search - e.g. model images of letters (a,b,c..) in text recognition. It is possible to use TM for the detection of Landmines in aerial images. However, since the appearance of Landmines change continuously over the image we need model images (templates) that apply to this variation. 

Such templates have been generated using methods of computer graphics, or, it is possible to use the real images and copy templates from them. Using this technique , we can apply template matching even if dimensions of template does not match to the dimensions of region in the image.We have to just loop over the images having multiple scales and have to keep track of the match with the largest correlation coefﬁcient (along with the x, y-coordinates of the region with the largest correlation coefﬁcient).After looping over all scales,we will take the region with the largest correlation coefﬁcient and use that as our “matched” region.We are using Canny edge detection for detecting the edges as matching using the edges rather than the raw image gives us a substantial boost in accuracy for template matching. We are using np.linspace function for looping through multiple images.This function accepts three arguments, the starting value, the ending value, and the number of equal chunk slices in between. In this example, we’ll start from 100 percent of the original size of the image and work our way down to 20 percent of the original size in 20 equally sized percent chunks.We then resize the image image according to the current scale and compute the ratio of the old width to the new width.We make a check to ensure that the input image is larger than our template matching. If the template is larger, then our cv2.matchTemplate call will throw an error, so we just break from the loop if this is the case.
Fig. 2: Template
Fig. 3: Tested Image
B. Approach 2: We wanted to use statistical model to detect our region of interest. We had two options: either we could extract the features of image and fed it into a classiﬁer such as support vector machines or we could use a deep learning model.We have used Mask R-CNN which is simple, ﬂexible, and general framework for object instance segmentation.We were able to detect each landmine in the processed images along with a high quality segmentation mask for each instance.The reason why we used this model is because of its ease of generalisation to another tasks.

Mask R-CNN Architecture
We have used tensorﬂow version 1.15, Keras version 2.24 and python version 3.7.3 for running this model.We are adding our weights to train this model.Bounding boxes are generated for each landmine based on Feature Pyramid Network (FPN) and a ResNet101 backbone.We are using coco model to train it on our own weights.We have extended two classes : Conﬁg class which contained the default conﬁguration and dataset class which provided us to work with our landmine dataset with a little change int he code.We can also load multiple dataset using this class which is our plan for future work.We have used the following loss function for our R-CNN model:
L(pi,ti) =
1 NclsX i
Lcls(pi,p∗i)+γ
1 NclsX i
p∗ iR(ti−t∗ i)

Where, i is the index of an another in a mini-batch and pi is the predicted probability of another i being an object. p∗ i is 0 when anchor is negative and 1 when is positive. The vector representing is ti. Log loss over two classes is represented in Lreg. Finally, R is robust loss function. For bounding box regression, the parametrizations of 4 coordinates following equation:
tx =
x−xa Wa
,ty =
y−ya ha

Where the x,y denote the box’s coordinates.
