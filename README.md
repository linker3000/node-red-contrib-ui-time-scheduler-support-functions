# node-red-contrib-ui-time-scheduler Support Functions
Possibly handy functions and code for use with node-red-contrib-ui-time-scheduler.

Everything here is offered 'as is' with no claims made about fitness for any specific purpose. Feel free to offer suggestions for improvements.

Here's some support functions I wrote for the Node-RED timer module *node-red-contrib-ui-time-scheduler*, which can be found here: https://flows.nodered.org/node/node-red-contrib-ui-time-scheduler. These add additional functionality to the timer without any modifications to the core code, so they should be compatible with any changes made by the timer's author.

## Notes

### Ensuring config changes are saved
To use functions or code that modify the timer's settings (like the ones here), the timer node needs to keep a copy of its settings in a saved file, as explained here: [https://flows.nodered.org/node/node-red-contrib-ui-time-scheduler](https://github.com/fellinga/node-red-contrib-ui-time-scheduler?tab=readme-ov-file#restoring-schedules-after-a-reboot), however one additional change to the flow is needed because (at the time of writing) the top output only sends out the current config if the timer's settings are changed via the dashboard, not if they are injected into the node. The change is as below, where we write the new config to the save file directly from the output of any of the new functions used from here:

![node structure](https://github.com/linker3000/node-red-contrib-ui-time-scheduler-support-functions/assets/19429471/7a75c216-51e3-4c52-9ee6-ae4d8b641327)

*Versions of node-red-contrib-ui-time-scheduler greater than V1.17.2 should (do check - it was a planned change) send the timer status info to the top node output for both dashboard **and** injected changes.*

### Ensuring injected changes are noticed

To make sure that injected changes are processed by the timer node and the output/s changed accordingly, you MUST unset the option highlighted below. If this isn't done, the timer node will not re-evaluate new
'off' conditions on the fly.

<img width="434" alt="single off setting" src="https://github.com/linker3000/node-red-contrib-ui-time-scheduler-support-functions/assets/19429471/25e7721d-7e5d-4192-b4a3-63d01f370724">

### Evaluation of settings

If you find that your timers do not respond quickly to changes, adjust the refresh period - for example, in the above image, the refresh is set to 5 second instead of the default 60.

## Functions and code

### Cascade (Duplicate first timer)
Reads the settings for timer 0 and duplicates them to all other timers, overwriting all other settings. See the function's code and adjust the loop value for 
to match your number of outputs.

### Check Tomorrow

When fed with the contents of the saved timer config, it outputs a true/false if any timers are set to turn on 'tomorrow'. The first output covers all timers and there's also separate outputs for each timer so you can, for example, have 'LED' indicators for them. *This function does not need to re-save the setup config. Adjust the number of function outputs and the code to suit your setup.* 

### Boost on and off

Adds or removes a 'boost' period to the specified timer's settings, starting from 'now' - for example, you could trigger a heater to come on (boost) for 2 hours (the default value in the 'on' function - this can be changed to suit your needs).

NB: The 'on' function will not re-enable a disabled timer.

Use as follows:

<img width="656" alt="boost examples" src="https://github.com/linker3000/node-red-contrib-ui-time-scheduler-support-functions/assets/19429471/c5d8a139-443c-443f-82c0-d9cd9d47aed7">

* The trigger/s (buttons etc.) send a msg.topic containing the timer number.
* The trigger causes the config file for the timers to be read.
* The read file function outputs the config file as the msg.payload, with the timer number as the msg.topic.
* The boost on or off function processes the config file, makes changes and these are injected into the timer.
* The new config is saved back to the target file (see notes above about making sure this happens).

### Enable / Disable Settings

Enable or disable all the settings for every timer. See the function comments for more info

### Enable / Disable Timer

Enable or disable one or more timer outputs (not the settings therein). See the function comments for more info.

### Who's Disabled

Returns true/false values to indicate whether timer outputs are disabled - handy because the timer module does not have indicators for this.

### Clear All

This code is designed to be used as the payload for a push button; it loads an empty setup into the node, effectively erasing all timers.

``{"timers":[],"settings":{"disabledDevices":[],"overviewFilter":"enabled"}}``
