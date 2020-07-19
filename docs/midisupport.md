<link rel="stylesheet" href="./docstyle.css">

<script>
  WebAudioControlsOptions={

  };
</script>

<script src="../webaudio-controls.js"></script>

Basic Usage :
<div style="display:flex;width:100%;flex-wrap:wrap">
<div class="item"><a href="./index.html">Overview</a></div>
<div class="item"><a href="./install.html">Install</a></div>
<div class="item"><a href="./specs.html">Attributes, Functions, Events</a></div>
<div class="item"><a href="./options.html">WebAudioControlsOptions</a></div>
<div class="item"><a href="./knobimage.html">Creating Knob Images</a></div>
<div class="item"><a href="./defstyle.html">Default Style of Controls</a></div>
<div class="item"><a href="./example.html">Examples of Various Attributes</a></div>
</div>
<br/>

Advanced Usage and Application Note :
<div style="display:flex;width:100%;flex-wrap:wrap">
<div class="item"><a href="./knobsamples.html">Knob Samples from KnobGallery</a></div>
<div class="item"><a href="./keyboard.html">Working Keyboard Demo</a></div>
<div class="item"><a href="./knobsize.html">Determining Knob Size</a></div>
<div class="item"><a href="./tracking.html">Slider tracking "rel" and "abs"</a></div>
<div class="item"><a href="./nonlinear.html">Non-Linear Knobs / Sliders</a></div>
<div class="item"><a href="./multifader.html">Multi-Touch Device Support</a></div>
<div class="item cur"><a href="./midisupport.html">MIDI Support</a></div>
<div class="item"><a href="./resizetest.html">Resizing After Creation</a></div>
</div>

---

# MIDI Support

**knobs, sliders and switches have midilearn/midicc support built-in**  

To enable MIDI related functions, specify `useMidi:1` in `WebAudioControlsOptions` before loading `webaudio-controls.js`. 

```html
<script>WebAudioControlsOptions = {
  useMidi:1,
}</script>
<script src="webaudio-controls.js"></script>
```

**Midilearn right click menu** :  
 Add a `midilearn=1` attribute to the `<webaudio-knob>`,  `<webaudio-slider>` and  `<webaudio-switch>` elements. Then right click on the element in the GUI, a midi learn menu should appear. Then, operate one of your midi controller and it should start actionning the webaudio-controls widget in the HTML page. You can hot plug/unplug midi devices, they will be detected.

![Midi Learn Menu](../img/midilearn.png)

**Declarative association between a midi controller and a GUI webaudiocontrol** :  
 There is also an HTML `midicc="channel.cc#"` attribute that works like this:  
 `midicc="3.2"` means "listen to a CC event on channel 3, CC number 2". If you don't know the channel/CC number of your controller:  
  1) add a `midilearn=1` attribute so that a right click on the GUI widget will display the midilearn menu,  
  2) select "learn" in the menu,  
  3) operate your knob/slider/switch, normally the midi controller and the GUI object are in sync.  
  4) look at the devtool console, there is a message indicating the channel and CC number, for example "channel 0, cc 28". Then if you add the attribute `midicc="0.28"` to the HTML of your knob/slider/switch, the midi mapping between your GUI webaudio-controls and your midi controller will be automatic. Follow the links at the end of this section and look at the HTML source code to see some examples.

Example: associate a knob with a controller on channel 7, cc number 7:

```html
<webaudio-knob midilearn=true midicc="7.7" ...></webaudio-knob>
```

**External midi event listener (hook)** :  
 You can also declare in your HTML file your own midi event listener (for example for listening to program changes events): use the `webAudioControlsMidiManager` object, that comes with an `addMidiListener` method. Like that you will benefit from the MIDI code included in the webaudio-controls. Here is an example (also, look at the source code of the [overview](./index.html) demo, and open the devtool console to see midi messages received by the hook at the end of the HTML file).

```html
<script>
// add this to your html page that uses webaudiocontrols
webAudioControlsMidiManager.addMidiListener(function(event) {
    var data = event.data;
    var channel = data[0] & 0xf;
    var controlNumber = data[1];

    console.log("Midi event hook: data:[" + data + "] channel:" + channel + " cc:"+controlNumber);

    // do whatever you want with the event
    // ...
});
</script>
```

Demo at: [https://wasabi.i3s.unice.fr/AmpSimFA/](https://wasabi.i3s.unice.fr/AmpSimFA/) and at [https://wasabi.i3s.unice.fr/AmpSimFA/sample1.html](https://wasabi.i3s.unice.fr/AmpSimFA/sample1.html)


---