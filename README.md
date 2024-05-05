# pycromanage
Microscope control through pycromanager: https://pycro-manager.readthedocs.io/en/latest/index.html 

So far I have been doing this through anaconda and jupyter notebook. Create a conda env (eg pycromanage) and then follow the installation instructions: 

    Install pycro-manager: pip install pycromanager

    Download newest version of micro-manager 2.0

    Open Micro-Manager, select tools-options, and check the box that says Run server on port 4827 (you only need to do this once)

Install jupyter notebook, and probably numpy and any other convenient things, then your good to go. Start a jupyter notebook and test the connection to uManager:

    from pycromanager import Core

    core = Core()
    print(core)

The 'core' object allows interfacing more or less directly with the micromanager API (here). For example, to get the current autofocus device and check its offset:

    afd = core.get_auto_focus_device()
    print(core.get_auto_focus_offset())

# Setting up uManager configuration:

**Camera**

Most commercial cameras have uManager adapters which are well documented on the uManager site. Create an instance of your camera after moving the correct files into the uManager installation folder.

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
