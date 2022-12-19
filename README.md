# LabViewCodesPilatusDetector
This program shows how to manipulate Pilatus images using LabView and contains simple LabView program to send commands to Pilatus detector and display Pilatus images in real time. 

The programs with name simple_image_viewer are able to open in one folder all Pilatus images inside it. The programs open Pilatus images from 3 types of Pilatus detectors: Pilatus 100K, Pilatus 200K and Pilatus 300K. The size of image of Piltus 100K is 487 x 195 pixels (H x V). The size of Pilatus 200K image is 487 x 407 pixels (H x V). The size of Pilatus 300K image is 487 x 619 pixels (H x V). The simple_image_viewer opens Pilatus images as binary files. The Pilatus images contain header with length of 4096 bytes. Therefore, you need to set offset in bytes to skip header length. The LabView programs use swap bytes function to swap low and high bytes. The Pilatus images are encoded as 20 bits unsigned integer numbers (the maximum number of photons is 1048575 photons per pixel). 

The most valuable LabView codes in this repositary are Pilatus_Live.vi in Pilatus_Live.llb and Pilatus_200K_Live.vi in Pilatus_200K_Live.llb. The programs were written in LabView 2012. The programs use LabVIEW Internet Toolkit toolbox from LabView. You need to download it and instal within LabView 2012. The LabVIEW Internet Toolkit contains Telnet library, as you can send commands to camserver of Pilatus detctor through Telnet communication protocol. The mentioned programs send for example these commands to the Pilatus dettector:
ni 1000
expt 1
expp 1.1
expo live.tif
The commands mean do number of images of 1000 with exposure time 1 s and exposure period 1.1 s and name images as live.tif. The exposure period will be derived from exposure time plus small extra time for creation of image and its soring on the memory disk. If number of images is 1000, the first image will be names as live_00000.tif and the last image will be live_00999.tif. 
The programs are written as the Queued state machine. When you press Start button you send comamnds to the Pilatus detector as mentioned above. Click CTRL+E and go to the block digram of LabView code and then go to Start_Pilatus state. Here, in StartPilatus state, you see Piltus expp function, which has input expt + 0.005 seconds. This 0.005 seconds or 5 ms is the minimum time for Pilatus detector to create image and store it in the detector electronics. Owerwrite value 0.005 seconds with another larger value if necessarry. For example 0.1 s = 100 ms or other value. 
The Pilatus_live program looks on the Z:\ drive if last image exists. If last image for example live_00999.tif exists, then the program goes to StopPilatus state. If not, the program looks at Z:\ drive, which is the last image with live name in the stack and displays it in PlotImage state. 
The Piltus detector runs under Linux system. You need to share \p2_det\images directory through Samba sharing with Windows. Create drive Z:\ on your Windows system, which points to the shared image directory on the Linux Pilatus computer. 
The last thing you need is that you need install X-Win32, which allows remote display of UNIX windows on Windows machines in a normal window alongside the other Windows applications. Then you can through ssh shell open \p2_det\ directory and start camserver of Pilatus detector by camonly command. Once, camserver is open, you can send comamnds to detector through camserver. The camserver establish commumincation between Pilatus detctor and Linux computer, X-Win32 opens communication between Windows and Linux computer. The LabView program sends commands to the IP address and port of camserver. 
