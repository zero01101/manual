# Bridge

Rack is a standalone DAW-like application and not a VST/AU plugin because of the major limitations of these formats.
It is common to think of physical modular synthesizers as entire self-contained DAWs, so many people use Rack as a complete DAW to compose music and build patches without other software.

However, *VCV Bridge* allows audio, MIDI, DAW transport, and DAW clocks to be transferred between Rack and your DAW through the included VST/AU instrument/effect Bridge plugins.

The setup order between Rack and your DAW does not matter.

## Setting up Bridge in Rack

- Add an Audio or MIDI module to Rack from the [Core](Core.html) plugin, and select "Bridge" from the driver dropdown list.
- Open the device menu to select the Bridge port.

Up to 8 channels of audio entering the Bridge effect plugin are routed to the INPUT section of the Audio module in Rack and then back to the effect plugin.

The 16 automation parameters in the VST/AU Bridge plugin simply generate MIDI-CC messages 0-15, so you can use a [Core MIDI-CC](Core.html#midi-cc) interface to convert them to 0-10 V signals in Rack.

## Setting up Bridge in your DAW

- Make sure the VST or AU Bridge plugin is installed, and launch your DAW. See the [installation instructions](https://vcvrack.com/manual/Installing.html#installing-rack) for more information about installing the VST on your platform.
- Add the "VCV Bridge" instrument or "VCV Bridge fx" effect plugin to a track.
	- The instrument plugin is easier for sending MIDI to Rack, although it also supports audio input if supported by your DAW.
	- The effect plugin is easier for sending audio to Rack, although it also supports MIDI input if supported by your DAW.
- Open the plugin parameters to reveal the Bridge port setting and 16 automation parameters.

### Ableton Live

Add a "VCV-Bridge" plugin to a MIDI track and open the automation parameters by clicking the triangle icon next to the plugin's name.
*Bridge* will send MIDI and receive audio from Rack.

To send audio to Rack, select the Bridge's track under the "Audio To" menu on another track, and optionally select the channel pair (1/2, 3/4, 5/6, or 6/7).

To record audio from Rack, create a new audio track and select the Bridge's track and optionally the channel pair under the "Audio From" menu.
Make sure "Monitor" is set to "In" on the Bridge's track to enable audio output even when it is not record-enabled.

![Ableton Live VCV Bridge](images/BridgeLive.png)

### Cubase
TODO

### FL Studio
In FL Studio Add a "VCV-Bridge" Channel. Inside Channel settings, navigate to Processing tab and check "Make bridged" and "Use fixed size buffers" (this is to be fixed sooner or later). For each output in VCV you will need separate Channel listening to separate port, as there's no option to handle many Wrapper inputs in FL Studio. 
VCV needs to be started *after* adding initial bridge in FL Studio, otherwise you won't hear any sound nor send any notes.
To send notes - in VCV - add MIDI-1 mapped to respective Port inside Channel in FL Studio.
To receive sound - in VCV - add SOUND, mapped to respective Port, Inputs 1-2 will send stereo sound to Wrapper in FL Studio.

Rendering notice: It was noticed, that it is impossible to render with ASIO (Presonus AudioBox ASIO) enabled, as it renders with tons of overloads and spikes. It is advised to try rendering with default audio driver with maximal possible latency.

### Propellerhead Reason
Add a VCV-Bridge or VCV-Bridge-fx from within Reason's browser or device menu to the rack.  The added device will default to receiving from Bridge port 1; you can change this by opening the CV Programmer panel on the VST rack device, setting any of the assignable parameters to the "Port" option, and changing that parameter's Base Value.  (Note that the Base Value is scaled from 0-100, not 1-16 as port selection is.)  After the Bridge device is added, open VCV Rack and ensure that the Fundamental Audio module's output is set to Bridge, and the correct Port is chosen.

You can then use Reason's MIDI input to "play" Rack via any of the Fundamental MIDI devices, assuming the MIDI device is set to receive from Bridge and the appropriate Port.  You can also use Reason's CV sequencers (e.g. Matrix Pattern Sequencer) as long as the Note or CV and Gate outputs are connected to the appropriate CV and Gate inputs of the VST device in Reason's rack in the same manner.

![Reason and VCV Bridge](images/BridgeReason.png)

Additionally, you can of course assign any of the available parameter slots to any of Bridge's CC inputs to pass CV modulation data to Rack to be used through MIDI-CC or similar devices.  The "Learn" option within Reason's VST device is not usable due to the nature of VCV Bridge acting as an intermediary between Reason and VCV Rack, as Reason cannot communicate with Rack directly to observe CC parameter changes.

![Pulsar modulating Fundamental VCO-1 and VCF](images/BridgeReason2.png)

You can also use audio-rate signals from within Reason to VCV Rack or vice versa for a truly modular approach, but since VCV Rack and Reason do not use compatible voltage standards, you must use interpreting devices.  Pitch signals can be translated between the two using Expert Sleepers' Silent Way voice controller, which is available for both [VCV Rack](https://vcvrack.com/plugins.html#expert%20sleepers%20silent), and [Reason](https://www.propellerheads.com/shop/rack-extension/silent-way-voice-controller/) as an add-on purchase.  Control signals can be interpreted bidirectionally within Reason by using Robotic Bean's CV-I (Audio to CV) and CV-O (CV to Audio) Rack Extensions, also available as a [separate purchase](https://www.propellerheads.com/shop/product_bundle/cv-io-bundle/).

*Note: The third-party devices listed above are used as examples, not endorsements.*

### REAPER
Right-click in the REAPER Track Control Panel (underneath the main toolbar, to the left of the Arrange area). Select "Insert virtual instrument on new track...". Locate "VCV Bridge" in your VST or VSTi folders. Select the 32- or 64-bit version of VCV Bridge per your own preferences and REAPER flavor. (If desired, you can also insert the VCV Bridge into an existing track by clicking the FX button for the track.)

When REAPER brings up the "Build Routing Confirmation" dialog, click "No" if you just want the bridge to make a typical set of stereo outputs available to REAPER, and "Yes" if you want to create eight discrete mono audio channels. REAPER will create eight additional audio tracks labeled "Output 1-8" if you select "Yes" at this dialog. Otherwise, it will only create a single track for MIDI and audio. 

(You can also manually create additional tracks / channels, and route the audio from the track containing the bridge insert into these additional tracks. This enables simultaneous live capture of VCV inbound MIDI and outbound audio on separate REAPER tracks.)

The bridge insert defaults to port 1 in REAPER. Make sure your MIDI and audio modules in Rack are all set to communicate with Bridge, and that the port settings in those Rack modules also match the intended port number from your REAPER bridge track or channel. 

You should now be able to play Rack via your MIDI controller from the armed bridge channel in REAPER. REAPER auto-maps and -arms the track for MIDI input, and also enables record monitoring, when the track is created with "Insert virtual instrument on new track." If you added VCV Bridge to your REAPER project another way, you may need to arm the track and turn record monitoring on. 

To record audio from Rack, right-click on the record arm button for the track that you want to capture Rack's output audio from the bridge, and select "Record: output" --> "Record: (whatever_audio_format_is_desired)". 

*Note:* If REAPER is already open with a VCV bridge instance created, and VCV is opened second with a patch that already contains MIDI and/or audio modules set to anything other than Bridge send/receive, VCV Rack may crash on startup or patch load, even if the VCV Bridge insert is currently bypassed in REAPER. You may need to close REAPER and specifically select Bridge mode in your Rack audio / MIDI setup first before attempting to bridge. 
