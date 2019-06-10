<p><span style="text-decoration: underline; font-size: 12pt;"><strong>NUT UPS Monitor</strong></span></p>
<p>Please see this Hubitat Community thread for more details and help on this integration:</p>
<p><a title="https://community.hubitat.com/t/qnap-nas-integrations-for-ups-monitoring-nodered-home-bridge-etc/17086" href="https://community.hubitat.com/t/qnap-nas-integrations-for-ups-monitoring-nodered-home-bridge-etc/17086">https://community.hubitat.com/t/qnap-nas-integrations-for-ups-monitoring-nodered-home-bridge-etc/17086</a></p>
<p>This is a telnet based integration between your Hubitat Elevation (HE) and your NAS.&nbsp; The&nbsp;NAS automatically drops the telnet connection after 20-30 seconds so this integration creates a new telnet connection each time and then disconnects.&nbsp; The following attributes are gathered from the NAS NUT Server:</p>
<ul style="list-style-position: inside;">
<li>Battery - % charged</li>
<li>Battery Run Time - will report "mains" while the UPS is on mains power but will change to the number of minutes remaining as reported by the UPS once on battery power.</li>
<li>UPS Alarm - will report any alarms reported by the UPS such as Change Battery</li>
<li>Power Source - mains or battery</li>
<li>UPS Status - Online, Offline, On Battery, Low Battery, Battery needs replacing, Alarm</li>
</ul>
<p>&nbsp;</p>
<p>NAS Settings (based on using a QNAP NAS using QTS4.3 or later:</p>
<ul style="list-style-position: inside;">
<li>In order for the HE hub to communicate with the UPS, you must enable the network UPS service and include the IP address of your HE hub running this driver</li>
<li>In QTS navigate to Control Panel, External Device, UPS tab:
<ul>
<li>Make sure that Enable Network UPS master is checked</li>
<li>In one of the IP address fields, enter the IP address of your HE hub</li>
<li>Click apply</li>
</ul>
</li>
<li>Note: entering the IP address into the list of IPs will allow communication between the UPS and the HE hub.&nbsp; A username and password are not required.</li>
</ul>
<p>&nbsp;</p>
<p>Hubitat Setup:</p>
<ul style="list-style-position: inside;">
<li>Download the <a title="nut_ups_driver code" href="https://raw.githubusercontent.com/mlritchie/Hubitat/master/NUT%20UPS%20Monitor/nut_ups_driver">nut_ups_driver code</a> and navigate to Drivers Code and create a new driver with this code in your HE hub</li>
<li>Create a new Virtual device
<ul>
<li>Navigate to&nbsp;Devices and click Add&nbsp;Virtual Device</li>
<li>Set the name and device network ID</li>
<li>Set type to your custom driver NUT UPS&nbsp;Device which will be found in the&nbsp;User drivers list at the very bottom</li>
<li>Click Save Device</li>
</ul>
</li>
<li>On your new device:<br />
<ul>
<li>Set&nbsp;NUT server hostname to the IP address of your NAS</li>
<li>The default NUT server port number value of 3493 should work (at least for QNAP)</li>
<li>Set the polling interval to the number of seconds you want your HE hub to check for UPS updates.&nbsp; Keep in mind that longer intervals will delay knowing that the UPS is on battery.</li>
<li>Click Save Preferences</li>
</ul>
</li>
<li>Other Preferences:
<ul>
<li>Display all variables: The NUT server will report quite a few attributes about your UPS that are mostly unnecessary.&nbsp; In order to streamline this integration and reduce the number of updates, only a few important attributes are gathered and stored as mentioned above.&nbsp; Feel free to enable this preference to see all the other attributes which will be stored in a State variable.</li>
<li>Pause Updates:&nbsp;Turning on this preference will turn off the polling of your UPS.</li>
<li>Connection Failure Threshold: After this threshold is met the driver will automatically pause updates.&nbsp; Connection failures can happen&nbsp;if your NAS is offline, restarting, or upgrading firmware.&nbsp; Some NAS devices are faster than others so you may need to tweak this setting depending on how fast your NAS restarts or applies firmware.&nbsp; Turning on debugging will log the number of failed attempts.&nbsp; It is important to note that once the threshold is met you will need to manually turn off pause updates.</li>
<li>Enable debug logging: Debugging is disabled by default however feel free to enable it.&nbsp; This does generate a ton of logging so make sure you turn it off when you are done debugging.</li>
</ul>
</li>
</ul>
