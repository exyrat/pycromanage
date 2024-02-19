# pycromanage
Microscope control through pycromanager: https://pycro-manager.readthedocs.io/en/latest/index.html 

Setting up uManager configuration:

**Camera**
For our Andor EMCCD, follow: Andor
Important: Install Andor driver pack into uManager installation folder
**Shutters**
We control our shutters with a 3-channel shutter box controlled through an NI DAQ. The easiest way I’ve found is:
First, connect the NIDAQ. Most likely you will be using Dev1.
Then create an instance of a State Device Shutter (in Utilities). You will need to choose the correct port on the NIDAQ, for us this is Dev1 → port0. This allows us to set the state of the device for each channel: 001 (Ch1), 010 (Ch2), etc… You can create groups of these e.g. if you want to simultaneously open only Ch1 and Ch3.
Set the Master Shutter as the state device shutter, which allows uManager to automatically open/close the desired shutters.
**XYZ Fine Stage**
We use the PI E-727. This is controlled through the PI_GCS_2 adapter.
First, move the proper dll ( PI_GCS2_DLL_x64.dll ) into the uManager installation folder.
Create an instance of the PI_GCSController_DLL. For the inputs, choose a proper name (e.g., E-727). The interface parameter is the SN# found in PIMikroMove and the interface type is USB (for our case).
Then, create an instance of the desired stage. E.g. for Z control, create a PIZStage. Select the controller (E-727) and choose axis 3. The process is similar for a PIXYStage (but axes 1 and 2).
