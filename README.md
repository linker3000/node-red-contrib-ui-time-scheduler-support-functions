# node-red-contrib-ui-time-scheduler Support Functions
Possibly handy functions and code for use with node-red-contrib-ui-time-scheduler.

Everything here is offered 'as is' with no claims made about fitness for any specific purpose. Feel free to offer suggestions for improvements.

Here's some support functions I wrote for the Node-RED timer module *node-red-contrib-ui-time-scheduler*, which can be found here: https://flows.nodered.org/node/node-red-contrib-ui-time-scheduler. These add additional functionality to the timer without any modifications to the core code, so they should be compatible with any changes made by the timer's author.

### Updating the timer save file

**This applies to node-red-contrib-ui-time-scheduler V1.17.2 and below.**

To use functions or code that modify the timer's settings, the timer node needs to keep a copy of its settings in a saved file, as explained here: [https://flows.nodered.org/node/node-red-contrib-ui-time-scheduler](https://github.com/fellinga/node-red-contrib-ui-time-scheduler?tab=readme-ov-file#restoring-schedules-after-a-reboot), however one additional change to the flow is needed because the top output only sends out the current config if the timer's settings are changed via the dashboard, not if they are injected into the node. The change is as below, where we write the new config to the save file directly from the output of any of the new functions used from here:

![node structure](https://github.com/linker3000/node-red-contrib-ui-time-scheduler-support-functions/assets/19429471/7a75c216-51e3-4c52-9ee6-ae4d8b641327)

*Versions of node-red-contrib-ui-time-scheduler greater than V1.17.2 send the timer status info to the top node output for both dashboard **and** injected changes.*

## Cascade (Duplicate first timer)
Reads the settings for timer 0 and duplicates them to all other timers, overwriting all other settings. See the function's code and adjust the loop value for 
to match your number of outputs.

## Check Tomorrow

When fed with the contents of the saved timer config, it outputs a true/false if any timers are set to come on 'tomorrow'. The first output covers all timers and there's also separate outputs for each timer so you can, for example, have 'LED' indicators for them. *This function does not need to re-save the setup config. Adjust the number of function outputs and the code to suit your setup.* 

## Clear All

This code is designed to be used as the payload for a push button; it loads an empty setup into the node, effectively erasing all timers.

``{"timers":[],"settings":{"disabledDevices":[],"overviewFilter":"enabled"}}``
