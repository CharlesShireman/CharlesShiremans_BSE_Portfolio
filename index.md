# LED Matrix Audio Visualizer
I used a 8x32 LED matrix, controlled by an Arduino microcontroller, to graph an FFT from live sound frequencies taken from a microphone. The entire system is powered through an AC power outlet which allows the user to always display sound data without replacing batteries. The LED matrix also has multiple animation modes that can be changed using a button. 

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Charles Shireman | Birch Wathen Lenox School | Computer Science | Incoming Senior

![Headstone Image](https://osbornegroupcre.com/wp-content/uploads/2016/02/missing-image-640x360.png)
  
# Final Milestone
My final milestone was the implementation of both a microphone and a button to my audio visualizer. To connect my Arduino to the microphone, I first soldered three braided wires to the microphone component and then connected them to a 5V port, a ground port and an analog input port on my Arduino. To connect my button to my Arduino, I connected it to a digital port and a ground port. I then set the digital port to input pull up which pulls up the voltage across the port to the maximum whenever there is no input voltage across the port unlike a regular input port which pulls down the voltage to 0V every time there is no voltage is detected across the port. Since I no longer connected my Arduino to my computer, I needed to create new code using Arduino FFT libraries rather than processing libraries. I used a FixFFT library to easily convert my analog sound into an FFT graph. Using this library is was able to create both a horizontal and vertical FFT graph.

![Final Milestone](https://osbornegroupcre.com/wp-content/uploads/2016/02/missing-image-640x360.png)
<table>
<tr>
<th>Audio Visualizer Code</th>
<th>Button Code</th>
</tr>
<tr>
<td>
<pre>
<pre>
<font color="#00979c">void</font> <font color="#000000">microAudioVisual</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">{</font>
 &nbsp;<font color="#5e6d03">for</font> <font color="#000000">(</font><font color="#000000">i</font> <font color="#434f54">=</font> <font color="#000000">0</font><font color="#000000">;</font> <font color="#000000">i</font> <font color="#434f54">&lt;</font> <font color="#000000">32</font><font color="#000000">;</font> <font color="#000000">i</font><font color="#434f54">++</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">val</font> <font color="#434f54">=</font> <font color="#d35400">analogRead</font><font color="#000000">(</font><font color="#000000">A5</font><font color="#000000">)</font><font color="#000000">;</font><font color="#434f54">&#47;&#47;get audio from Analog 0</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">data</font><font color="#000000">[</font><font color="#000000">i</font><font color="#000000">]</font> <font color="#434f54">=</font> <font color="#000000">val</font> <font color="#434f54">&#47;</font> <font color="#000000">6</font><font color="#434f54">-</font><font color="#000000">40</font><font color="#000000">;</font> <font color="#434f54">&#47;&#47;optimize sound input for better graph representation</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">im</font><font color="#000000">[</font><font color="#000000">i</font><font color="#000000">]</font> <font color="#434f54">=</font> <font color="#000000">0</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#000000">fix_fft</font><font color="#000000">(</font><font color="#000000">data</font><font color="#434f54">,</font> <font color="#000000">im</font><font color="#434f54">,</font> <font color="#000000">7</font><font color="#434f54">,</font> <font color="#000000">0</font><font color="#000000">)</font><font color="#000000">;</font><font color="#434f54">&#47;&#47;fix ftt library requires both input and output arrays for FFT calculation</font>
 &nbsp;<font color="#000000">matrix</font><font color="#434f54">.</font><font color="#000000">fillScreen</font><font color="#000000">(</font><font color="#000000">0</font><font color="#000000">)</font><font color="#000000">;</font><font color="#434f54">&#47;&#47;reset screen</font>
 &nbsp;<font color="#5e6d03">for</font> <font color="#000000">(</font><font color="#000000">i</font> <font color="#434f54">=</font> <font color="#000000">0</font><font color="#000000">;</font> <font color="#000000">i</font> <font color="#434f54">&lt;</font> <font color="#000000">32</font><font color="#000000">;</font> <font color="#000000">i</font><font color="#434f54">++</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#00979c">int</font> <font color="#000000">dat</font> <font color="#434f54">=</font> <font color="#d35400">sqrt</font><font color="#000000">(</font><font color="#000000">data</font><font color="#000000">[</font><font color="#000000">i</font><font color="#000000">]</font> <font color="#434f54">*</font> <font color="#000000">data</font><font color="#000000">[</font><font color="#000000">i</font><font color="#000000">]</font> <font color="#434f54">+</font> <font color="#000000">im</font><font color="#000000">[</font><font color="#000000">i</font><font color="#000000">]</font> <font color="#434f54">*</font> <font color="#000000">im</font><font color="#000000">[</font><font color="#000000">i</font><font color="#000000">]</font><font color="#000000">)</font><font color="#000000">;</font><font color="#434f54">&#47;&#47;filter out noise and hum</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">i</font><font color="#434f54">==</font><font color="#000000">1</font> <font color="#434f54">||</font> <font color="#000000">i</font><font color="#434f54">==</font><font color="#000000">2</font> <font color="#434f54">||</font> <font color="#000000">i</font><font color="#434f54">==</font><font color="#000000">3</font><font color="#000000">)</font> <font color="#000000">dat</font><font color="#434f54">&#47;=</font><font color="#000000">2</font><font color="#000000">;</font><font color="#434f54">&#47;&#47;lower first three band values</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">getColor</font><font color="#000000">(</font><font color="#000000">i</font><font color="#434f54">,</font><font color="#000000">dat</font><font color="#000000">)</font><font color="#000000">;</font><font color="#434f54">&#47;&#47;change color of light row using global matrixColor variable</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">matrix</font><font color="#434f54">.</font><font color="#000000">drawLine</font><font color="#000000">(</font><font color="#000000">i</font><font color="#434f54">,</font> <font color="#000000">0</font><font color="#434f54">,</font> <font color="#000000">i</font><font color="#434f54">,</font><font color="#000000">dat</font><font color="#434f54">,</font> <font color="#000000">matrixColor</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#434f54">&#47;&#47; draw bar graphics</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#000000">matrix</font><font color="#434f54">.</font><font color="#d35400">show</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">}</font>
</pre>
</td>
<td>

<pre>
<font color="#00979c">void</font> <font color="#5e6d03">loop</font><font color="#000000">(</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;<b><font color="#d35400">button</font></b><font color="#434f54">.</font><font color="#5e6d03">loop</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>

 &nbsp;<font color="#5e6d03">if</font> <font color="#000000">(</font><b><font color="#d35400">button</font></b><font color="#434f54">.</font><font color="#d35400">isPressed</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">buttonPress</font> <font color="#434f54">=</font> <font color="#00979c">HIGH</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>

 &nbsp;<font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">buttonPress</font> <font color="#434f54">==</font> <font color="#00979c">HIGH</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">lastMethod</font> <font color="#434f54">==</font> <font color="#005c5f">&#34;textScroll&#34;</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">lastMethod</font> <font color="#434f54">=</font> <font color="#005c5f">&#34;audio&#34;</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">}</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">else</font> <font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">lastMethod</font> <font color="#434f54">==</font> <font color="#005c5f">&#34;audio&#34;</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">lastMethod</font> <font color="#434f54">=</font> <font color="#005c5f">&#34;rainbow&#34;</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">}</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">else</font> <font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">lastMethod</font> <font color="#434f54">==</font> <font color="#005c5f">&#34;rainbow&#34;</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">lastMethod</font> <font color="#434f54">=</font> <font color="#005c5f">&#34;vert&#34;</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">}</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">else</font> <font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">lastMethod</font><font color="#434f54">==</font><font color="#005c5f">&#34;vert&#34;</font><font color="#000000">)</font><font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">lastMethod</font><font color="#434f54">=</font><font color="#005c5f">&#34;textScroll&#34;</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">}</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">buttonPress</font> <font color="#434f54">=</font> <font color="#00979c">LOW</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>

 &nbsp;<font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">lastMethod</font> <font color="#434f54">==</font> <font color="#005c5f">&#34;textScroll&#34;</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">textDisplay</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#5e6d03">else</font> <font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">lastMethod</font> <font color="#434f54">==</font> <font color="#005c5f">&#34;audio&#34;</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">serialAudioVisual</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#5e6d03">else</font> <font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">lastMethod</font> <font color="#434f54">==</font> <font color="#005c5f">&#34;rainbow&#34;</font><font color="#000000">)</font> <font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">rainbowWave</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#5e6d03">else</font> <font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">lastMethod</font><font color="#434f54">==</font><font color="#005c5f">&#34;vert&#34;</font><font color="#000000">)</font><font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">vertAudioVisual</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font> 
 &nbsp;&nbsp;<font color="#000000">}</font>

<font color="#000000">matrix</font><font color="#434f54">.</font><font color="#d35400">show</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">}</font>
</pre>

</td>
</tr>
</table>
# Second Milestone
My second milestone was the completion of my audio visualization code with live sound from my computer. I used the Processing IDE and its sound library to send computed FFT data through the Arduino serial port. The use of the serial port was difficult and needed unconventional data transfer methods. Using a string instead of conventional number data types, my program was able to send large amounts of data, such as a sound array, in one data transfer. The output string was formed in the processing code, sent to the arduino IDE, and put back into its original form with a getVals method.

![Second Milestone][![Charlie S Milestone #2](https://res.cloudinary.com/marcomontalbano/image/upload/v1625232831/video_to_markdown/images/youtube--nbdAxFHW8wU-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=nbdAxFHW8wU "Charlie S Milestone #2")
### Processing Code
I used the Processing IDE's sound, serial and FFT libraries to take microphone input from my computer, find 32 bands of FFT data and send it to the Arduino IDE.



# First Milestone
  

My first milestone was setting up and controlling my LED matrix with an Arduino microcontroller. Using the Adafruit NeoMatrix library for the Arduino IDE, I was able to control each LED specifically as well as create custom light animations. The LED matrix was wired directly into the Arduino without the use of a breadboard to simplify input. One issue I faced in this project was wiring the screen to the Arduino. I went through multiple iterations of wiring but eventually figured out the only the screen input connections were needed while the rest can be ignored. 

[![Charlie S Milestone #1](https://res.cloudinary.com/marcomontalbano/image/upload/v1624455381/video_to_markdown/images/youtube--I3flct1JG6U-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=I3flct1JG6U "Charlie S Milestone #1")

### Rainbow Wave Code

Using the Adafruit NeoMatrix library, I was able to apply my previous coding skills to my Arduino project. My rainbow wave function uses an int varible, count, to determine the starting color for every one of the function initializations. Since the starting color changed, the for loop then changes every other line color accordingly. Another outside variable, temp, allows for a change in color based on the last line color inside the for loop.

![Wave GIF](https://github.com/CharlesShireman/CharlesShiremans_BSE_Portfolio/raw/gh-pages/rainbowWave.gif)
```
void rainbowWave() {
  if (count == 0) {
    temp = "r";
    int color = colors[0];
    count = 1;
  }
  else if (count == 1) {
    temp = "g";
    int color = colors[1];
    count = 2;
  }
  else if (count == 2) {
    temp = "b";
    int color = colors[2];
    count = 0;
  }
  for (int i = 0; i <= matrix.width(); i++) {
    if (temp == "r") {
      temp = "g";
      color = colors[1];
    }
    else if (temp == "g") {
      temp = "b";
      color = colors[2];
    }
    else if (temp == "b") {
      temp = "r";
      color = colors[0];
    }
    matrix.drawFastVLine(x + i, y, 8, color);
  }
}
```
#### Adafruit Libraries
Additional information about the LED control libaries used can be found at:

https://learn.adafruit.com/adafruit-gfx-graphics-library

https://github.com/adafruit/Adafruit_NeoMatrix
