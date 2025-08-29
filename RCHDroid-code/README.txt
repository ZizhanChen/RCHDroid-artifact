1. The repo is arranged as follows:
------
  |----benchmark-app/: contains the benchmark app source code.
  |----source-code/: contains the source code of RCHDroid.
  
2. About:
The artifact provides the source code of RCH-Droid, along with the instructions to generate the results.  
The artifact allows users to reproduce key results from the paper, including Figure 7, Figure 8, Figure 9, Figure 10, and Figure 14. 
The hardware must contain the rk3588-lubancat-5-v2 development board connected to a screen. Users can also build images from the source code.


3. Hardware Dependencies
RCHDroid works on Android platforms. We have tested RCHDroid’s correctness and performance on the rk3588-lubancat-5-v2 development board. To reproduce the results in the paper, the same development board is suggested to be utilized.

4. software Dependencies
- Apps installation and communication with the development board: ADB 1.0.41.
- CPU and memory profiler: Android Studio Arctic Fox profiler.

5. Installation}
Users can follow the following steps to build the image from the source code.
- Download the Android 14 source code.
- Replace parts of the code of Android 14 with the code inside ./source-code to build RCHDroid. README inside ./source-code presents which parts should be replaced.
- Compile the image.
- When the image is ready, users can flash the image to the development board.

6. Experiment Workflow
Users can reproduce the runtime change handling time with 27 apps in Figure 7 and memory usage in Figure 8, and reproduce the runtime change handling time and memory usage with the Top 100 Android Apps in Figure 14 as follows:
- For each app, users should first start it in landscape mode (i.e., 1920x1080), and then wait a few seconds for the app to start up and enter a stable state. 
- Users can measure the memory usage of the app before runtime changes by the following command: \verb|dumpsys meminfo|. Users can find the memory usage of the app in the \verb|Total PSS| \verb|by process| list. 
- Users can trigger the first runtime change by the command through ADB:
    \verb|wm size 1080x1920|.
- Users can trigger the second runtime change by the command through ADB:
    \verb|wm size reset|.
- The run time change handling timestamps will be saved in the log. Users can print the related logs by the command through ADB: \verb+logcat | grep "zizhan"+. The runtime change handling time is the time between the \verb|configura-| \verb|tion change happened| and the \verb|activity resumed|.
- Users can measure the memory usage after runtime changes by \verb|dumpsys meminfo|. Users can find the memory usage of the app in the \verb|Total PSS by process| list. 
- Users should close the app to release the resources for the next round of the experiment. Also, users should clear the logs by the command through ADB: \verb|logcat -c|.

Users can reproduce the CPU and memory usage over time with the benchmark app in Figure 9 as follows:
- Users can load the benchmark app source code into the Android Studio. The app can run on the development board through the ADB.
- Users can use the profiler tool of Android Studio to monitor memory usage and CPU usage over time. 
- Users can trigger the first runtime change by the command through ADB:
    \verb|wm size 1080x1920|.
- Users can touch the button to update the views. The views updating is asynchronous, during which, users should trigger the second runtime change by the command through ADB:
    \verb|wm size reset|.


Users can reproduce the runtime change handling time with different numbers of views of RCHDroid in Figure 10 as follows:
- Users can load the benchmark app source code into the Android Studio. The app can run on the development board through the ADB.
- The number of views can be changed. Users can modify src/main/java/.../MainActivity.java, src/main/res/layout-land\\/activity\_main.xml, and src/main/res/layout-port/activity\_\\main.xml to change the view number.
- Users can trigger the runtime change by the commands through ADB:
    \verb|wm size 1080x1920| and \verb|wm size reset|. The runtime change handling time will be saved in the log.
-Users can print the related logs by the command through ADB: \verb+logcat | grep "zizhan"+. The runtime change handling time is the time between the \verb|configuration change| \verb|happened| and the \verb|activity resumed|.

7. Evaluation and Expected Results

We expect the reproduced results for Figure 7, Figure 8, Figure 9, Figure 10, and Figure 14 to match the results in the paper.
