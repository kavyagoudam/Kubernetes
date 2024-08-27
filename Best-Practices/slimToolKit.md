# SlimToolKit
slimtoolkit is a open source tool that helps to optimize docker images by removing unnecessary layers and files.


Docker Image size 500 MB --> 80 MB ⚡ 

slimtoolKit does this by analyzing the Docker image and identifying the unwanted layers and files that will not affect the image's ability to run properly even if they are not included in the image.

The slimtoolKit can reduce the image size by upto 30% of the image size

The image size may be reduced even more for applications run on compiled languages such c, c++, Java etc.

refer [slimtoolkit blog](https://devopscube.com/slimtoolkit-to-shrink-docker-images/)
