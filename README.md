# Star Tracker Simulated Image Generator
## Tool to generate simulated images for star tracker centroid testing

### Background
A star tracker is a type of attitude determination sensor flown on spacecraft. It operates under the principle of capturing an image of the "sky" (AKA the celestial sphere), identifying stars in the image, relating the pointing direction in an absolute reference frame, and returning a quaternion representing the attitude. One of the processes is called the **centroiding** process which determines the center of all stars in a given image.

### Motivation
In order to properly test the centroiding phase, images need to be generated and passed through. These images can be created by taking real images of the night sky (which is susceptible to environmental/sky conditions and a function of the camera) or creating realistic images. This generator create images based on a given (or random) pointing direction and camera properties (e.g., FOV, sensor quality, etc.) with an associated CSV containing true centroid information. This can be used to test the centroiding step and measure accuracy and precision.

### Projection Generator in Action
The **generateProjection** script makes use of the *argparse* library to ingest user data (or use default values) to create images
![HelpMenu](media/HelpMenu.png)


The tool can be run without any arguments to point to a random direction (e.g., as seen below when where we can find the Big Dipper)
![BigDipper](media/BigDipper.png)
The tool also prints out the relevant information of stars in the image e.g., their camera and ECI vectors
![BigDipperFrame](media/BigDipperDataFrame.png)

The tool can also ingest specific data in case specific pointing directions want to be viewed
![ManualSet](media/ManualSet.png)
![ManualSetFrame](media/ManualSetFrame.png)

The generateProjections tool creates a CSV containing image information that can be used as the ground truth data against the results of the centroiding algorithm

It should be noted that these analysis plots do not contain the actual image the star tracker would see, but a representation of where the stars should be located. The actual images are generated by calling a different script, **generateImage**.

### Image Generator in Action
To generate simulated (and utility images), **generateImage** can be called. Similar to **generateProjection**, this tool uses *argparse* to ingest user data such as the PKL file of a previously generated projection. If no PKL file is given, the script will call **generateProjection** with a random pointing direction. It also has the ability to create *n* images as a flag.
![generateImageHelpMenu](media/genImgHelp.png)

The images generated include a true-to-size image (right) based on the camera pixel and a MATPLOTLIB utility image (left) to show where the stars should be. These images file name have the RA, DEC and ROLL used to generate them for easier use later on.
|                                     |                                     |
| ----------------------------------- | ----------------------------------- |
| ![](media/UTIL_-75.14933065298278_-77.92631917270793_11.490861906531109.png) | ![](media/SIM_-75.14933065298278_-77.92631917270793_11.490861906531109.png) |

The tool aims to be as realistic as possible by generating the star brightness and size according to the Airy Disk equations; notice the small and dim ring around the star below. The tool assumes the image is **5 magnitudes** over exposed to increase the star size and brightness.
![airyDisk](media/AiryDisk.png)

No noise (e.g., Dark Current or ADC noise) has been simulated but are the next steps in sim generation.
