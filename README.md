# node-red-contrib-ui-time-scheduler-support-functions-
Possibly handy functions for use with node-red-contrib-ui-time-scheduler

Here's some support functions I wrote for the Node-RED timer module *node-red-contrib-ui-time-scheduler*, which can be found here: https://flows.nodered.org/node/node-red-contrib-ui-time-scheduler.

These add additional functionality to the timer without any modifications to the core code, so they should be compatible with any changes made by the timer's author.

To use these functions, the timer node needs to retain a copy of its settings in a saved file, as explained here: [https://flows.nodered.org/node/node-red-contrib-ui-time-scheduler](https://github.com/fellinga/node-red-contrib-ui-time-scheduler?tab=readme-ov-file#restoring-schedules-after-a-reboot), however one additional change to the flow is needed because the top output only sends out the current config if the timer's settings are changed via the dashboard, not if they are injected into the node. The change is as below, where we write the new config to the save file directly from the output of any of the new functions used from here:

![node structure](https://github.com/linker3000/node-red-contrib-ui-time-scheduler-support-functions-/assets/19429471/c21a7b90-1388-4333-bf2d-7babd3673cc1)


