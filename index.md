# LED Matrix Audio Visualizer
I used a 8x32 LED matrix, controlled by an Arduino microcontroller, to graph an FFT from live sound frequencies taken from a microphone. The entire system is powered through an AC power outlet which allows the user to always display sound data without replacing batteries. The LED matrix also has multiple animation modes that can be changed using a button. 

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Charles Shireman | Birch Wathen Lenox School | Computer Science | Incoming Senior

![Headstone Image](https://osbornegroupcre.com/wp-content/uploads/2016/02/missing-image-640x360.png)
  
# Final Milestone
Not completed

[![Final Milestone](https://osbornegroupcre.com/wp-content/uploads/2016/02/missing-image-640x360.png)

# Second Milestone
My second milestone was the completion of my audio visualization code with live sound from my computer. I used the Processing IDE and its sound library to send computed FFT data through the arduino serial port. The use of the serial port was difficult and needed unconventional data transfer methods. Using a string instead of conventional number data types, my program was able to send large amounts of data, such as a sound array, in one data transfer. The out string was formed in the processing code, sent to the arduino IDE, and put back into its original form with a getVals method.

[![Second Milestone](https://osbornegroupcre.com/wp-content/uploads/2016/02/missing-image-640x360.png)

# First Milestone
  

My first milestone was setting up and controlling my LED matrix with an Arduino microcontroller. Using the Adafruit NeoMatrix library for the Arduino IDE, I was able to control each LED specifically as well as create custom light animations. The LED matrix was wired directly into the Arduino without the use of a breadboard to simplify input. One issue I faced in this project was wiring the screen to the Arduino. I went through multiple iterations of wiring but eventually figured out the only the screen input connections were needed while the rest can be ignored. 

[![Milestone #1](https://res.cloudinary.com/marcomontalbano/image/upload/v1624455381/video_to_markdown/images/youtube--I3flct1JG6U-c05b58ac6eb4c4700831b2b3070cd403.jpg)](https://www.youtube.com/watch?v=I3flct1JG6U "Milestone #1"){:target="_blank" rel="noopener"}

### Rainbow Wave Code

Using the Adafruit NeoMatrix library, I was able to apply my previous coding skills to my arduino project. My rainbow wave function uses an int varible, count, to determine the starting color for every one of the function initializations. Since the starting color changed, the for loop then changes every other line color accordingly. Another outside variable, temp, allows for a change in color based on the last line color inside the for loop.
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
