# Ball Tracking Robot Using Computer Vision

My project is a ball-tracking robot that implements two fundamental sensors, the ultrasonic sensor and the PiCamera, to detect the ball's color and park in front of it. It runs on a Raspberry Pi 4, which is a microcomputer, and uses OpenCV, a computer vision library for the PiCamera that allows you to create color masks for any object. It also utilizes an ultrasonic sensor that emits soundwaves and measures the time it takes for them to return, determining the distance of an object from the sensor.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Dominic R | Mountain View High School | Software Engineering | Incoming Sophomore |

# Final Milestone: Coding the logic

<iframe width="560" height="315" src="https://www.youtube.com/embed/JE0mSJVivUM?si=rRrgV_1lsp3SeP-V" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

# Summary
From my second milestone, I've gone through an iterative process of reorienting parts around the chassis and coding basic logic for the robot to track the ball. I reoriented my breadboard and battery pack following the addition of the portable charger, which allows the robot to become wireless and ready for testing, and I also went through multiple orientations for the ultrasonic sensor and PiCamera. I learned how to use Pulse Width Modulation, a function to help control the speed of DC Motors, to my advantage so that the robot could turn at a slower speed to more effectively find the ball. Once my hardware was settled, I started coding some basic logic, but was stuck at turning the various ideas I had into effective code. As a result, I used the help of ChatGPT to make substantial changes, which allowed my robot to unlock smart/smooth steering and detect the ball more frequently. A few key topics I learned about during my time at BSE were soldering, breadboard connections, the power equation, Ohm's Law, PWM, and the oscilloscope. I understood different solder joints and which onces to avoid, how the underside of a breadboard looks like and how to use one effecetively, how to calculate the necessary resistance for a project using the voltage divider equation (V_out = V_in * (R2 / (R1 + R2))), how Ohm's Law (V = IR) ties into my project, how to use PWM to control the speed of DC Motors, and the usefulness of an oscilloscope for visualizing connections. After everything I've learned at BSE, I hope to use my knowledge in soldering and breadboard connections for competition projects within school clubs, and I hope to understand at a deeper level how to use the power equation and Ohm's Law in complex electrical projects.
# Challenges
One minor challenge I encountered was deciding what orientation of the PiCamera and ultrasonic sensor I would use. I went through multiple orientations, which involved assembling and subsequently disassembling the PiCamera Mount, since its angle made ball detection difficult. I ended up taking two ultrasonic mounts and using double-sided tape to fix the two objects in optimal positions for detection. One major challenge I had was that one of my motors suddenly stopped working one day, so I learned how to use an oscilloscope so I could identify the problem, which was that I was misusing PWM due to my primitive understanding of the function at the time.
# Reflection
Reflecting back on the project, I would have invested more time in understanding PWM before integrating it into the project, since my superficial understanding of it early on caused a motor to fail and cost me time that I had to spend on debugging. I also would have planned my sensor and camera orientations more deliberately from the start, rather than going through multiple iteration cycles on assembly. If I were to continue this project, I would explore fully incorporate the LED strip code to add night vision, and I would finish designing a 3D printed shell for the robot. Overall, this project gave me a strong foundation in computer vision, embedded systems, and iterative hardware design that I plan to build on in future engineering projects.

# Second Milestone: Setting up Hardware and Electrical Connections

<iframe width="560" height="315" src="https://www.youtube.com/embed/SG6JKxGKYbI?si=zcIaGNO3x1XRtLHC" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

# Summary
Since my first milestone, I have constructed my robot chassis and attached all my electronic components to it using a strong double-sided adhesive. I was able to successfully test my DC Motors and Ultrasonic Sensor using code I found on the official Raspberry Pi website. They contribute to the final goal since the motors will move the robot towards the ball. The ultrasonic sensor can identify how far away different objects are, including the ball. I also solidified my electrical connections by either soldering or wrapping wire around electrical points. Something surprising that has occurred was that I found some code for calibrating and sensing a certain color on **Seeed Studio**, and it worked perfectly without any problems at all. Normally, I have found that using code online to work on a project always has its own, unique kinks to work out, but the lack of those kinks surprised me. Before my final milestone, I have to create the main software for the robot, which entails creating a color mask for the ball and writing logic for the robot to track and move towards the ball.

# Challenges
One challenge from Milestone 1 that was able to be overcome was the loss of the SSH Key connection, and as I mentioned, the instructors created a new WiFi network that made handling the Pi much easier since I could use Tiger VNC, a better visualization software compared to OBS Capture. Although this milestone didn't bring up any serious challenges, a minor challenge that I faced was effectively attaching all my hardware components to the chassis, which I solved by purchasing a strong double-sided tape and sticking all my components to the chassis.

# First Milestone: Setting up Raspberry Pi

<iframe width="560" height="315" src="https://www.youtube.com/embed/qotKq9lGOao?si=t-YUARO0Oc2g_OL_" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

# Summary
My project is a ball-tracking robot, which means that it needs some way to receive visual input and take action based on that input. The robot will use ultrasonic sensors to identify where different objects are, a Raspberry Pi paired with a PiCamera to detect the ball's color, and DC Motors so it has a means of moving towards the ball. For my first milestone, I set up my Raspberry Pi with an SSH Key, which is like a passkey to secure, password-free access to your Raspberry Pi. I also connected my PiCamera to the Raspberry Pi and took a successful photo using the test code provided by Bluestamp. About my upcoming milestones, I plan first to assemble the robot with all its necessary components and make sure electrical connections are working properly, then I will code the robot to detect the ball's color and track it.

# Challenges
Although setting up the Raspberry Pi was smooth at first, when I tried filming the first milestone video, the SSH Key failed, and from then on, it hasn't shown sign of being reestablished. However, I plan to solve this problem by directly coding on the Raspberry Pi using a software called OBS Capture instead of connecting to the Pi and coding it remotely. This software uses an HDMI cable to visualize the Pi as a computer on your laptop screen. The instructors are also talking of creating a new WiFi network for each classroom that allows SSH Key connections since the school WiFi has restrictions on these connections.

# Schematics (Main Project)

# Main Code
```python
import cv2
import numpy as np
from picamera2 import Picamera2
import RPi.GPIO as gpio
from gpiozero import DistanceSensor
import time
 
Your_color = "Red"
pwmA = None
pwmB = None
ball_last_seen = None
stable_ball_counter = 0
ball_required_stability = 3

# Define your color range 163,179,121, 255, 111, 255
my_color_lower = np.array([163, 121, 111], np.uint8)
my_color_upper = np.array([179, 255, 255], np.uint8)
gpio.setmode(gpio.BCM)

def init():
    global pwmA, pwmB
    # Motor 1
    gpio.setup(23, gpio.OUT) #IN1
    gpio.setup(22, gpio.OUT) #IN2
    gpio.setup(8, gpio.OUT) #EN1
    # Motor 2
    gpio.setup(5, gpio.OUT) #IN3
    gpio.setup(6, gpio.OUT) #IN4
    gpio.setup(7, gpio.OUT) #EN2
    #LED
    gpio.setup(16, gpio.OUT) #RED
    gpio.setup(20, gpio.OUT) #GREEN
    gpio.setup(26, gpio.OUT) #BLUE
   
    pwmA = gpio.PWM(8, 1000)
    pwmB = gpio.PWM(7, 1000)
    pwmA.start(0)
    pwmB.start(0)
   
def light_red():
    gpio.output(16, True)
    gpio.output(20, False)
    gpio.output(26, False)
   
def light_green():
    gpio.output(16, False)
    gpio.output(20, True)
    gpio.output(26, False)
   
def light_blue():
    gpio.output(16, False)
    gpio.output(20, False)
    gpio.output(26, True)

def move_forward(duration, speed):
    gpio.output(23, True)
    gpio.output(22, False)
    gpio.output(5, False)
    gpio.output(6, True)
   
    pwmA.ChangeDutyCycle(100)
    pwmB.ChangeDutyCycle(100)
    time.sleep(0.05)
    pwmA.ChangeDutyCycle(speed)
    pwmB.ChangeDutyCycle(speed+7)
   
    time.sleep(duration)
    pwmA.ChangeDutyCycle(0)
    pwmB.ChangeDutyCycle(0)
   
def move_backward(duration, speed):
    gpio.output(23, False)
    gpio.output(22, True)
    gpio.output(5, True)
    gpio.output(6, False)
   
    pwmA.ChangeDutyCycle(100)
    pwmB.ChangeDutyCycle(100)
    time.sleep(0.05)
    pwmA.ChangeDutyCycle(speed)
    pwmB.ChangeDutyCycle(speed+7)
   
    time.sleep(duration)
    pwmA.ChangeDutyCycle(0)
    pwmB.ChangeDutyCycle(0)

def rotate_left(duration, speed):
    gpio.output(23, True)
    gpio.output(22, False)
    gpio.output(5, True)
    gpio.output(6, False)
   
    pwmA.ChangeDutyCycle(100)
    pwmB.ChangeDutyCycle(100)
    time.sleep(0.05)
    pwmA.ChangeDutyCycle(speed)
    pwmB.ChangeDutyCycle(speed)
   
    time.sleep(duration)
    pwmA.ChangeDutyCycle(0)
    pwmB.ChangeDutyCycle(0)
   
def rotate_left_soft(duration):
    print("rotate_left_soft CALLED")
    gpio.output(23, True)
    gpio.output(22, False)
    gpio.output(5, False)
    gpio.output(6, False)
   
    pwmA.ChangeDutyCycle(100)
    pwmB.ChangeDutyCycle(100)
    time.sleep(0.05)
    pwmA.ChangeDutyCycle(0)
    pwmB.ChangeDutyCycle(50)
    time.sleep(duration)

    pwmA.ChangeDutyCycle(0)
    pwmB.ChangeDutyCycle(0)

def rotate_right(duration, speed):
    gpio.output(23, False)
    gpio.output(22, True)
    gpio.output(5, False)
    gpio.output(6, True)
   
    pwmA.ChangeDutyCycle(100)
    pwmB.ChangeDutyCycle(100)
    time.sleep(0.05)
    pwmA.ChangeDutyCycle(speed)
    pwmB.ChangeDutyCycle(speed)
   
    time.sleep(duration)
    pwmA.ChangeDutyCycle(0)
    pwmB.ChangeDutyCycle(0)
   
def rotate_right_soft(duration):
    print("rotate_right_soft CALLED")
    gpio.output(23, False)
    gpio.output(22, False)
    gpio.output(5, False)
    gpio.output(6, True)
   
    pwmA.ChangeDutyCycle(100)
    pwmB.ChangeDutyCycle(100)
    time.sleep(0.05)
    pwmA.ChangeDutyCycle(25)
    pwmB.ChangeDutyCycle(0)
   
    time.sleep(duration)
    pwmA.ChangeDutyCycle(0)
    pwmB.ChangeDutyCycle(0)
   
def stop():
    gpio.output(23, False)
    gpio.output(22, False)
    gpio.output(5, False)
    gpio.output(6, False)
    pwmA.ChangeDutyCycle(0)
    pwmB.ChangeDutyCycle(0)
   
def is_ball_centered(ball_x, frame_width, tolerance = 30):
    center_x = frame_width//2
    return abs(ball_x-center_x) < tolerance

def set_motor_speed(left_speed, right_speed):
    if left_speed >= 0:
        gpio.output(23, True)
        gpio.output(22, False)
    else:
        gpio.output(23, False)
        gpio.output(22, True)
    pwmA.ChangeDutyCycle(abs(left_speed))

    if right_speed >= 0:
        gpio.output(5, False)
        gpio.output(6, True)
    else:
        gpio.output(5, True)
        gpio.output(6, False)
    pwmB.ChangeDutyCycle(abs(right_speed))

def smooth_steering(ball_x, frame_width):
    center_x = frame_width // 2
    offset = ball_x - center_x
    tolerance = 30
    max_speed = 60
    min_speed = 30
    if abs(offset) < tolerance:
        left_speed = right_speed = max_speed
    else:
        steering_factor = offset / center_x
        steering_factor = max(min(steering_factor, 1), -1)

        if steering_factor > 0:
            right_speed = max_speed * (1 - abs(steering_factor) * 0.5)
            left_speed = max_speed
        else:
            left_speed = max_speed * (1 - abs(steering_factor) * 0.5)
            right_speed = max_speed

    set_motor_speed(left_speed, right_speed)

def search_for_ball(timeout=10, min_area=100000):
    start_time = time.time()
    direction = -1 if ball_last_seen == "left" else 1
    while time.time() - start_time < timeout:
        if direction == -1:
            rotate_left_soft(0.15)
        else:
            rotate_right_soft(0.15)
        im = picam2.capture_array()
        hsvFrame = cv2.cvtColor(im, cv2.COLOR_BGR2HSV)
        color_mask = cv2.inRange(hsvFrame, my_color_lower, my_color_upper)
        contours, _ = cv2.findContours(color_mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)

        if contours:
            largest = max(contours, key=cv2.contourArea)
            area = cv2.contourArea(largest)

            if area >= min_area:
                print("Ball found during search!")
                return True
    return False

# Initialize PiCamera
init()
picam2 = Picamera2()
picam2.preview_configuration.main.size = (1280, 720)
picam2.preview_configuration.main.format = "RGB888"
picam2.preview_configuration.align()
picam2.configure("preview")
picam2.start()
ultrasonic = DistanceSensor(echo = 17, trigger = 10)

while True:
    im = picam2.capture_array()
    hsvFrame = cv2.cvtColor(im, cv2.COLOR_BGR2HSV)
    color_mask = cv2.inRange(hsvFrame, my_color_lower, my_color_upper)
    contours, _ = cv2.findContours(color_mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    if contours:
        light_blue()
        largest = max(contours, key=cv2.contourArea)
        area = cv2.contourArea(largest)
        if area > 120000:
            stable_ball_counter = 0
            x,y,w,h = cv2.boundingRect(largest)
            ball_x = x+w//2
            frame_center = im.shape[1]//2      
            offset = ball_x - frame_center
            print(f"Offset: {offset}, ball_last_seen set to: {ball_last_seen}")
            if abs(offset) < 40:
                ball_last_seen = "center"
            elif offset < 0:
                ball_last_seen = "left"
            else:
                ball_last_seen = "right"
               
            print("Ball found")
           
            if is_ball_centered(ball_x, im.shape[1]):
                print("Ball centered")
                if cv2.countNonZero(color_mask) < 150000 and ultrasonic.distance > 0.1:
                    light_blue()
                    move_forward(0.2, 55)
                    print("Approaching ball")
                else:
                    stop()
                    light_green()
                    print("Parked in front of ball")
            else:
                light_blue()
                smooth_steering(ball_x, im.shape[1])
                print(f"Ball off-center, smoothly steering {ball_last_seen}")
            cv2.rectangle(im, (x, y), (x + w, y + h), (0, 255, 0), 3)
            cv2.line(im, (frame_center, 0), (frame_center, im.shape[0]), (255, 0, 0), 2)
            cv2.circle(im, (ball_x, y + h // 2), 10, (0, 0, 255), -1)
            
            cv2.putText(im, f"Offset: {offset}", (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1, (255,255,255), 2)
            cv2.putText(im, f"Last seen: {ball_last_seen}", (10, 60), cv2.FONT_HERSHEY_SIMPLEX, 1, (255,255,255), 2)
            cv2.putText(im, f"Distance: {ultrasonic.distance:.2f}m", (10, 90), cv2.FONT_HERSHEY_SIMPLEX, 1, (255,255,255), 2)
        else:
            light_red()
            print("Contour too small, ignoring")
            stable_ball_counter += 1
            if stable_ball_counter > 5:
                ball_last_seen = None
                print("Clearing ball_last_seen due to instability")
            found = search_for_ball()

    else:
        light_red()
        print(f"[DEBUG] ball_last_seen = {ball_last_seen}")
        if ball_last_seen == "left":
            print("Main: ball_last_seen = left ? calling rotate_left_soft")
            rotate_left_soft(0.3)
            print("Ball lost, rotating left to reacquire")
        elif ball_last_seen == "right":
            rotate_right_soft(0.3)
            print("Ball lost, rotating right to reacquire")
        elif ball_last_seen == "center":
            rotate_right_soft(0.2)
            print("Ball was centered but now lost")
        else:
            print("Ball completely lost. Running search...")
            found = search_for_ball()
            if found:
                light_blue()
                print("Ball reacquired. Returning to tracking...")
            else:
                stop()
                print("Ball not found after search. Waiting or retrying...")
                time.sleep(0.5)

    display = im.copy()
    text_color = (255, 255, 255)
    cv2.putText(display, f"Ball Last Seen: {ball_last_seen}", (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1, text_color, 2)
    cv2.putText(display, f"Ultrasonic: {ultrasonic.distance:.2f}m", (10, 70), cv2.FONT_HERSHEY_SIMPLEX, 1, text_color, 2)
                
    cv2.imshow("PiCam View", im)

    if cv2.waitKey(1) & 0xFF == ord('q'):
        picam2.stop()
        cv2.destroyAllWindows()
        break
'''
while True:
    im = picam2.capture_array()
    hsvFrame = cv2.cvtColor(im, cv2.COLOR_BGR2HSV)
    color_mask = cv2.inRange(hsvFrame, my_color_lower, my_color_upper)
    contours, _ = cv2.findContours(color_mask, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
    print("Color pixels:", cv2.countNonZero(color_mask))
    print("Ultrasonic distance:", ultrasonic.distance)
   
# Start a while loop
while True:
    im = picam2.capture_array()
    hsvFrame = cv2.cvtColor(im, cv2.COLOR_BGR2HSV)
    color_mask = cv2.inRange(hsvFrame, my_color_lower, my_color_upper)
    color_pixel_count = cv2.countNonZero(color_mask)
    #result_frame = detect_single_color(im, Your_color, my_color_lower, my_color_upper, (0, 255, 0))
    #cv2.imshow("Single Color Detection in Real-Time", result_frame)
    #print("Matching color pixels:", color_pixel_count)
    if cv2.waitKey(10) & 0xFF == ord('q'):
        picam2.stop()
        cv2.destroyAllWindows()
        break
'''
```

# LED Strip Code (Modifications)

```python
# sudo ~/ball_tracking_robot/venv/bin/python /home/dreouk/Documents/Led_test.py
import board
import neopixel
import RPi.GPIO as GPIO
import time

# Pin setup
LDR_PIN = 21
LED_PIN = board.D18
NUM_PIXELS = 5

pixels = neopixel.NeoPixel(LED_PIN, NUM_PIXELS, auto_write=False)

GPIO.setmode(GPIO.BCM)

def rc_time(pin):
    count = 0

    GPIO.setup(pin, GPIO.OUT)
    GPIO.output(pin, False)
    time.sleep(0.1)

    GPIO.setup(pin, GPIO.IN)
    while GPIO.input(pin) == 0:
        count += 1
        if count > 10000:
            break

    return count

try:
    while True:
        light_level = rc_time(LDR_PIN)
        print("Light level:", light_level)

        threshold = 45  

        if light_level > threshold:
            pixels.fill((255, 255, 255))
            pixels.show()
        else:
            pixels.fill((0, 0, 0))
            pixels.show()

        time.sleep(0.2)

except KeyboardInterrupt:
    print("Exiting program")

finally:
    pixels.fill((0, 0, 0))
    pixels.show()
    GPIO.cleanup()
```

# Color Calibration Code

```python
import cv2
import numpy as np

def empty(a):
  pass

def stackImages(scale,imgArray):
  rows = len(imgArray)
  cols = len(imgArray[0])
  rowsAvailable = isinstance(imgArray[0], list)
  width = imgArray[0][0].shape[1]
  height = imgArray[0][0].shape[0]
  if rowsAvailable:
      for x in range ( 0, rows):
          for y in range(0, cols):
              if imgArray[x][y].shape[:2] == imgArray[0][0].shape [:2]:
                  imgArray[x][y] = cv2.resize(imgArray[x][y], (0, 0), None, scale, scale)
              else:
                  imgArray[x][y] = cv2.resize(imgArray[x][y], (imgArray[0][0].shape[1], imgArray[0][0].shape[0]), None, scale, scale)
              if len(imgArray[x][y].shape) == 2: imgArray[x][y]= cv2.cvtColor( imgArray[x][y], cv2.COLOR_GRAY2BGR)
      imageBlank = np.zeros((height, width, 3), np.uint8)
      hor = [imageBlank]*rows
      hor_con = [imageBlank]*rows
      for x in range(0, rows):
          hor[x] = np.hstack(imgArray[x])
      ver = np.vstack(hor)
  else:
      for x in range(0, rows):
          if imgArray[x].shape[:2] == imgArray[0].shape[:2]:
              imgArray[x] = cv2.resize(imgArray[x], (0, 0), None, scale, scale)
          else:
              imgArray[x] = cv2.resize(imgArray[x], (imgArray[0].shape[1], imgArray[0].shape[0]), None,scale, scale)
          if len(imgArray[x].shape) == 2: imgArray[x] = cv2.cvtColor(imgArray[x], cv2.COLOR_GRAY2BGR)
      hor= np.hstack(imgArray)
      ver = hor
  return ver



path = 'captured_image.jpg'
cv2.namedWindow("TrackBars")
cv2.resizeWindow("TrackBars",640,240)
cv2.createTrackbar("Hue Min","TrackBars",0,179,empty)
cv2.createTrackbar("Hue Max","TrackBars",19,179,empty)
cv2.createTrackbar("Sat Min","TrackBars",110,255,empty)
cv2.createTrackbar("Sat Max","TrackBars",240,255,empty)
cv2.createTrackbar("Val Min","TrackBars",153,255,empty)
cv2.createTrackbar("Val Max","TrackBars",255,255,empty)

while True:
  img = cv2.imread(path)
  img= cv2.resize(img, (300, 300))
  imgHSV = cv2.cvtColor(img,cv2.COLOR_BGR2HSV)
  h_min = cv2.getTrackbarPos("Hue Min","TrackBars")
  h_max = cv2.getTrackbarPos("Hue Max", "TrackBars")
  s_min = cv2.getTrackbarPos("Sat Min", "TrackBars")
  s_max = cv2.getTrackbarPos("Sat Max", "TrackBars")
  v_min = cv2.getTrackbarPos("Val Min", "TrackBars")
  v_max = cv2.getTrackbarPos("Val Max", "TrackBars")
  print(h_min,h_max,s_min,s_max,v_min,v_max)
  lower = np.array([h_min,s_min,v_min])
  upper = np.array([h_max,s_max,v_max])
  mask = cv2.inRange(imgHSV,lower,upper)
  imgResult = cv2.bitwise_and(img,img,mask=mask)


  cv2.imshow("Original",img)
  cv2.imshow("HSV",imgHSV)
  cv2.imshow("Mask", mask)
  cv2.imshow("Result", imgResult)

  #imgStack = stackImages(0.6,([img,imgHSV],[mask,imgResult]))
  #cv2.imshow("Stacked Images", imgStack)

  cv2.waitKey(1)
```

# Motor Test Code

```python
# Motor test
import RPi.GPIO as gpio
from gpiozero import DistanceSensor
import time
gpio.setmode(gpio.BCM)
pwmA = None
pwmB = None

def init():
    # Motor 1
    gpio.setup(23, gpio.OUT) #IN1
    gpio.setup(22, gpio.OUT) #IN2
    gpio.setup(8, gpio.OUT) #EN1
    # Motor 2
    gpio.setup(5, gpio.OUT) #IN3
    gpio.setup(6, gpio.OUT) #IN4
    gpio.setup(7, gpio.OUT) #EN2


def move_forward(duration):
    gpio.output(8, True)
    gpio.output(7, True)
    gpio.output(23, True)
    gpio.output(22, False)
    gpio.output(5, False)
    gpio.output(6, True)
    time.sleep(duration)
    
def move_backward(duration):
    gpio.output(8, True)
    gpio.output(7, True)
    gpio.output(23, False)
    gpio.output(22, True)
    gpio.output(5, True)
    gpio.output(6, False)
    time.sleep(duration)
    
init()
print("Moving forward")
move_forward(1)
gpio.cleanup()
#Ultrasonic
#ultrasonic = DistanceSensor(echo = 17, trigger = 10)
#while True:
    #print(ultrasonic.distance)

```

# Bill of Materials (Main Project)

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Buzzer | Make sounds to provide enhanced experience | $7 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Gikfun-Terminals-Passive-Electronic-Arduino/dp/B01GJLE5BS/ref=sr_1_1_sspa?crid=2QM1MJDNNAQCA&dib=eyJ2IjoiMSJ9.OyPyJJ4xtWru7f8xcsfDi7R03NiW0MPkZStq2OCgkR1o4VGlFrUNNqNTzJyiVZVdk_wUMmt6O_38yzxecOcAzyz_rCYUsdSXR1avywEMk8IU9K3ojIEHaW-YDrSIzRR-a-ALkkqQhkfI9C70N65oew-bd2T9sxWGmhJpcucFmK3Y6noTTw-8Tjh_4tLth_U9hI3mRgBnSTtbLmFUdyJqIItVViPqW00ZtYBzmg2n7mU._n9TNI6lrWkczy6P113YSB7VdYavuhuaU-rBRfkV__E&dib_tag=se&keywords=arduino+buzzer&qid=1752793393&sprefix=arduino+buzze%2Caps%2C132&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)"> Link </a> |
| Electric Capacitor | Stores electrical energy in an energy field | $5 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Tegg-Capacitor-Aluminium-Electrolytic-Capacitors/dp/B07TC7Z26J/ref=sxin_15_pa_sp_search_thematic_sspa?content-id=amzn1.sym.70fcaece-2dd2-4653-bf00-fb6af1af1b93%3Aamzn1.sym.70fcaece-2dd2-4653-bf00-fb6af1af1b93&crid=TPG2H4VAARCM&cv_ct_cx=arduino+electric+capacitor&keywords=arduino+electric+capacitor&pd_rd_i=B07TC7Z26J&pd_rd_r=1658ab37-b84e-445d-a4fc-44075e824e1f&pd_rd_w=NkNue&pd_rd_wg=QO4Gb&pf_rd_p=70fcaece-2dd2-4653-bf00-fb6af1af1b93&pf_rd_r=4W8P821CAH8N3VS2BKAC&qid=1752793420&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=arduino+electric+capacito%2Caps%2C127&sr=1-2-e169343e-09af-4d41-85b1-8335fe8f32d0-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&psc=1)"> Link </a> |
| Micro USB | Allows for testing of the electronics | $6 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Charging-Transfer-Android-Trustable-MYFON/dp/B098DW7485/ref=sr_1_3?crid=1Q5DXZRO06QG6&dib=eyJ2IjoiMSJ9.XI5TAoOgPcN3rs36qvgQ-9f4CK96ZoUlcBAknhleVfee6NdvqB8_1dBe0hy5JyUj-glKMbqws9QLom9Y-WS24x38iQawHgDdvNGM8b2IiH5UeanDoCPaQX6VxRm7cUMACu-6k8ZFa3otnJgwRhlJQQE4fIjv8G5OuxMvzSrVUhkjy3UPRB4n9_foz_tJ_X6FylQk81Y20492dT33IpKDntRzkkVPzk9LWsmobrjt6v7PDhTrluBJsn1v3MnCJX0rlZ37F5LkGXYM497VrV77iIFNLGmPNg2olhLO6Shiac4.F_tQmgsb6R9-lyCsXNfpJDm7y4WlApPFl_1mkDQfvlY&dib_tag=se&keywords=arduino%2Bmicrousb%2Bcable&qid=1752793459&s=industrial&sprefix=arduino%2Bmicrousb%2Bcable%2Cindustrial%2C112&sr=1-3&th=1)"> Link </a> |
| Power Cable | Allows for testing of the electronics | $6 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/DKARDU-Connector-Solderable-Extension-Charger/dp/B09WTWSG1W/ref=sr_1_1?crid=17DEQS0T42MPJ&dib=eyJ2IjoiMSJ9.SUHJzrZza5pkgNlk3sDzxIQ8HFe81xrdz6WzvUUxpMlvAU69hIJAr5xKXDHutD1dGa9D1vMoc3oT3GQ1EwnkpJK7Y5z2nBt9df9m0c9X1Sy7DkbKus25TDZXgau7pzIaenH2Zs2iTldhgh7_R885LU6h1RZ8uWa8LqMn0M1E9THx1LY_O1IKbogI-W-vlaA93gaUugb3mZDVzC7_RvL_Xh0ipFGWBp1VXIj68UuKPl-CSgeBunTop6Vp8eOSYYgrh0CBNzhA89lE7FOb5af2TTXCjACABDTWARgFRAe8dlg.aDRCKe64i-C8yNlGWTabFgDAXEELJzi90sWjaG7cXk4&dib_tag=se&keywords=PCB%2Bpower%2Bcable&qid=1752793585&s=industrial&sprefix=pcb%2Bpower%2Bcable%2Cindustrial%2C120&sr=1-1&th=1)"> Link </a> |
| Self-switch | Turns on and off the device | $7 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Latching-Button-Circuit-0-23inch-Self-Lock/dp/B0DFQ7MCCS/ref=sr_1_27?crid=382HA7ZCS59N0&dib=eyJ2IjoiMSJ9.4qtR5AUXFYsumY2sRQU5HwnlBqFcKdLqbBBWEY627Ep5418x9WgCoenRBdJSZN0KWZW3e5pdEvl7eRMOB6VzhgxPjoyM0ElDUiXCvkCBzD29QMO3z5eMgJIfg6mKeO6CukSrHmbKTAu7SOZPPJNUBxJLMIA1z0uqZpLPAA-SeHcJ8Aqeghn8EINQX84xC-XBfGonQpjrWj7COtv5j-Hpj8qxhmtO1AzvV5g1ba5SS9UhcxqufLukZ23EytoAi8-FCHdTvwrDYQkfTU3zUXymjkhTMXHR3zO4gBt5FKhcp0g.GV2810EcR9TFLvJsnGEt98PggigthasCZZeA9sLtzHE&dib_tag=se&keywords=arduino%2Bself%2Bswitch&qid=1752793627&s=industrial&sprefix=arduino%2Bself%2Bswitc%2Cindustrial%2C121&sr=1-27&th=1)"> Link </a> |
| Self-switch cap | Makes it easy for the user to turn on and off the device | $2 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Latching-Button-Circuit-0-23inch-Self-Lock/dp/B0DFQ7MCCS/ref=sr_1_27?crid=382HA7ZCS59N0&dib=eyJ2IjoiMSJ9.4qtR5AUXFYsumY2sRQU5HwnlBqFcKdLqbBBWEY627Ep5418x9WgCoenRBdJSZN0KWZW3e5pdEvl7eRMOB6VzhgxPjoyM0ElDUiXCvkCBzD29QMO3z5eMgJIfg6mKeO6CukSrHmbKTAu7SOZPPJNUBxJLMIA1z0uqZpLPAA-SeHcJ8Aqeghn8EINQX84xC-XBfGonQpjrWj7COtv5j-Hpj8qxhmtO1AzvV5g1ba5SS9UhcxqufLukZ23EytoAi8-FCHdTvwrDYQkfTU3zUXymjkhTMXHR3zO4gBt5FKhcp0g.GV2810EcR9TFLvJsnGEt98PggigthasCZZeA9sLtzHE&dib_tag=se&keywords=arduino%2Bself%2Bswitch&qid=1752793627&s=industrial&sprefix=arduino%2Bself%2Bswitc%2Cindustrial%2C121&sr=1-27&th=1)"> Link </a> |
| Digitron Display | Displays score for different games | $4 | <a href="[[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Display-MAX7219-Compatible-Arduino-Raspberry/dp/B0DHGL5CPK/ref=sr_1_1?crid=ADGA434DUG8S&dib=eyJ2IjoiMSJ9.EGCMFKkVXIubAhKllu5Vf3zhLKcxhkpB-EJKDxdgiUY4cI03aoScOxyDOxz1JhBQty9zOjS0epDaP7GadPxfwM3n507SAAGqXC4DfyHX5SGtTFb-2u30P-8JiAJKITBS5HDVPDLlNlqH9JLg1ZQNgkpvfYmCPaXPXymxvKTByg_T_9vTFe17b_pd4K-GEQwF6DAqjCOOkwSo4I-7ZFwWtGu3rKZQ4WSu3n7j4zMU1mgz8102zLLeETqk3e-MC9SBV5Af2wUAggxQhImkX5_yP56yaP8Hxh9YfwPC8vQ5g0c.QIodDmjBi7EKN_yHaPqSogn9kq_4dWsyJev_SScT9JQ&dib_tag=se&keywords=arduino%2Bdot%2Bmatrix&qid=1752793714&s=industrial&sprefix=arduino%2Bdot%2Bmatri%2Cindustrial%2C113&sr=1-1&th=1)](https://www.amazon.com/WWZMDiB-Module%EF%BC%8CLED-Brightness-Adjustable-Accessories/dp/B0BFQNFX6D/ref=sr_1_1_sspa?crid=1YMZV306BE1YO&dib=eyJ2IjoiMSJ9.TOsWOwmSqpc7D4XwEk7P_XBC6HxBkTymLi__VoaZfZ50UK9bSvJZP4ENypND_i1244lomhyYAdljT04Da6Lt6fUsXjGc_s4ldKP75_HGOAZhAQ-wdJEW2K319cS9KVnoSgjJqd2X1xDz81tl_oH5Un11NPtc7uuakIncKdksXpOWHGeScC2wrET9nmK5V8y0JPwgO6p7udIj6XXpkQAHQ2uCKFrzoXgerpPw5QV-utdvxGyw3vcVEzDEgQBCtNFCQhDxhanez03-P6eeLO0x593c21eVY2Xz7jYSQ4qb9p8.YqLwkZ6IVXYDgcIW5cIUwXzB57apCFW_kTMvZBy4gnY&dib_tag=se&keywords=arduino+digitron+display&qid=1752793784&s=industrial&sprefix=arduino+digitron+display%2Cindustrial%2C114&sr=1-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)"> Link </a> |
| IC Chip | Stores software for device | $7 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Original-Atmel-Dip-8-ATTINY85-20PU-Tiny85-20Pu/dp/B06W9JBJJ6/ref=sr_1_6?crid=1514X4NKXZVEP&dib=eyJ2IjoiMSJ9.eCIJiJ20gCOskUWoTQhr2uaflCeA8xq0ssoZ9UQ_utmUetz4pBM1HUHahQ0CEJLz78PLGDz18ToVBgwLr8aa7DLGcn919bUxd1Fco29kqRNjbtM9TIWHRaNqhnYx5H0I9O86kz534JvF2pNJNmZQNXxbmDjB8324WrEIbViCWhdtKya5PFXyi8LeTVUngJlZgp4tO5Okp5PEhQuZ3cQ9Ss4ZnRuQpwlpWfZDr1Bo1LU78laMuDXe9qsIa0Kb9jznzzgW1PCJAWAnfndnWx7GFje-vr0lGmkwb-9xMNUPyOw._yAFGytF1lcyEr9Fbbja-3htCap5NhbZkyCPOSLoIFY&dib_tag=se&keywords=arduino+ic+chip&qid=1752793756&s=industrial&sprefix=arduino+ic+chip%2Cindustrial%2C112&sr=1-6)"> Link </a> |
| LED dot matrix module | Acts as a screen for the device | $2 | <a href="[[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Display-MAX7219-Compatible-Arduino-Raspberry/dp/B0DHGL5CPK/ref=sr_1_1?crid=ADGA434DUG8S&dib=eyJ2IjoiMSJ9.EGCMFKkVXIubAhKllu5Vf3zhLKcxhkpB-EJKDxdgiUY4cI03aoScOxyDOxz1JhBQty9zOjS0epDaP7GadPxfwM3n507SAAGqXC4DfyHX5SGtTFb-2u30P-8JiAJKITBS5HDVPDLlNlqH9JLg1ZQNgkpvfYmCPaXPXymxvKTByg_T_9vTFe17b_pd4K-GEQwF6DAqjCOOkwSo4I-7ZFwWtGu3rKZQ4WSu3n7j4zMU1mgz8102zLLeETqk3e-MC9SBV5Af2wUAggxQhImkX5_yP56yaP8Hxh9YfwPC8vQ5g0c.QIodDmjBi7EKN_yHaPqSogn9kq_4dWsyJev_SScT9JQ&dib_tag=se&keywords=arduino%2Bdot%2Bmatrix&qid=1752793714&s=industrial&sprefix=arduino%2Bdot%2Bmatri%2Cindustrial%2C113&sr=1-1&th=1)how to](https://www.amazon.com/HiLetgo-MAX7219-Arduino-Microcontroller-Display/dp/B07FFV537V/ref=sr_1_1_sspa?crid=QP9W0P687B1Y&dib=eyJ2IjoiMSJ9.VRKpp8C6VgfQkqpQFRO_jeBBfeEZAtZUFxfCN7yShsnqYhcOSrylvknMi3p6bjor04Ch5FvFEHMGG6gJSXtzoKbBp0K8WfmFY41h_Xp0-pHdJYtwXJa-_9tuGTqOvnt75hhl_LLocxdr4XUHyAqjzwLdx4WhO-djdmJMuuaam5W4jjG_wc-aAO5it6j_vANC6AgJjL9K2GmhyKL90mjR1WmTcFnirylYOEeL9rPoLlGmT6C8Ob54Zf3zHpmlbSMiTltgvdu1Qel-B_xcy36bzom7mb9ssVnjv893lFELQwE.-TPWNj6t1CmdKIGagGakiBad8K6Hqb6ou0j5y0KMjfo&dib_tag=se&keywords=arduino+led+dot+matrix+module&qid=1752794866&s=industrial&sprefix=arduino+led+dot+matrix+modul%2Cindustrial%2C127&sr=1-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)"> Link </a> |
| Button | Allows for manual input by user | $4.50 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Gikfun-12x12x7-3-Tactile-Momentary-Arduino/dp/B01E38OS7K/ref=sr_1_2_sspa?crid=TPTDC9JWQ7G3&dib=eyJ2IjoiMSJ9.K8ztKL3l65wCk2uoh4BBBDaBNwwJroXwb13SQ7K1QH-pdbYclQOVIkI-M9Bo_Wp4FHX7PbrIBSN4GNRv2hSj9DsdXWTdqPskNgPd2egJZ0qISiU1x0lnJhCklfyBunv8KyJJuEq1givx6M-IbiN8QMDB9XsU6VT7yUc0hbWR-LiA4n7E_Zv33eYpA-FYx69RNwjneMFmIJBeaY_K9IRpEljQPpif5L6TyyPWEOJ2sfgsf7LdCfKBqzziBkdyTw4tTNunNf2Rhwm3F91KDYv2FcJ0tATs1seFWlM3NFxMA8U.f2hOouuigHSgvy7zVo83cZWBmJ7Gv_V21vwr3PXoKYQ&dib_tag=se&keywords=arduino+tactile+button&qid=1752794889&s=industrial&sprefix=arduino+tactile+button%2Cindustrial%2C123&sr=1-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)"> Link </a> |
| Button cap | Makes it easy for user to provide manual input | $4.50 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Gikfun-12x12x7-3-Tactile-Momentary-Arduino/dp/B01E38OS7K/ref=sr_1_2_sspa?crid=TPTDC9JWQ7G3&dib=eyJ2IjoiMSJ9.K8ztKL3l65wCk2uoh4BBBDaBNwwJroXwb13SQ7K1QH-pdbYclQOVIkI-M9Bo_Wp4FHX7PbrIBSN4GNRv2hSj9DsdXWTdqPskNgPd2egJZ0qISiU1x0lnJhCklfyBunv8KyJJuEq1givx6M-IbiN8QMDB9XsU6VT7yUc0hbWR-LiA4n7E_Zv33eYpA-FYx69RNwjneMFmIJBeaY_K9IRpEljQPpif5L6TyyPWEOJ2sfgsf7LdCfKBqzziBkdyTw4tTNunNf2Rhwm3F91KDYv2FcJ0tATs1seFWlM3NFxMA8U.f2hOouuigHSgvy7zVo83cZWBmJ7Gv_V21vwr3PXoKYQ&dib_tag=se&keywords=arduino+tactile+button&qid=1752794889&s=industrial&sprefix=arduino+tactile+button%2Cindustrial%2C123&sr=1-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)"> Link </a> |
| PCB | Connects all hardware and software and acts as backbone of device | $4 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Rindion-Compatible-Prototype-Electronics-Soldering/dp/B0DBZ1BXFZ/ref=sr_1_1_sspa?crid=3PYEBNJRYRFYE&dib=eyJ2IjoiMSJ9.hZ7yUlqIa0lR5GczvXnkXchid0p8q1tf9dz-Aqsd1VTo0-DN9ihE9bPrpMDJUOae8S0tJ5m_FAMKqX0l5ROXgIZYe6Am_YxuRjGugXfdV6QH2Y9mgWhy86Mvo1us1cC8JcoQWwO7uDJFzU__0blnkWL-8peu20H5gpbs7sU1ON6inudm8vdy6ROibt6HRJiQiSvMr5C0ZLt8pzzwWJd0ibiZVpQa7pv_174Sfl6AAxzCronWV1de5-Utii-c1PiC52rt8Seanlua1NPEc6VXzHLqKl3C9W5JxIzaIuO9dXc.EtfYCCagBF4xFfEIjneQFbqt9n5JubLNnl0I4cAbo_I&dib_tag=se&keywords=PCB&qid=1752794913&s=industrial&sprefix=pcb%2Cindustrial%2C128&sr=1-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)"> Link </a> |
| Screw | Hold acrylic shell and PCB together | $0.10 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/WZHUIDA-Screws-Assortment-M5x6mm-Washers/dp/B0DRYD9LWQ/ref=sr_1_1_sspa?content-id=amzn1.sym.918a99dd-4826-4c0a-be33-a6705d69c4cf%3Aamzn1.sym.918a99dd-4826-4c0a-be33-a6705d69c4cf&crid=2W0BXKSPDWK6E&dib=eyJ2IjoiMSJ9.lJQGGNO-WO_GctF05dAhhRgZAAnXh3RbqLTH8lxqZXkJFdBvnhJ85-AVMSqsXyWMhDaz-zujjuriIj--BbdszcB1Un6LFVjePjcZLgBpMmT9MqJ1To3Cc_LJ1jCdFlLvABAbaGWvs48uZ9qQGDt0kjBHyQVzNKr6TIll0QhjbMvyvopxVpST1si1VNodVi4RBbw34ik0rw5c2DIopcpK7s7jiTc4q-ZcdI2xNnpVfkFIUDfeYiXIy3EXGS9I1_PpRfDdkpUtSd8k-tR89DskWlmDXjRpKXIvYrt2UMz_3aE.NDqrHbbyVJ_o26F-IFC2rLB1BuXu1YW-UP0_1lS_sr4&dib_tag=se&keywords=Screws&pd_rd_r=2108f754-bc40-4771-90e9-c2c124f595fc&pd_rd_w=h10Vb&pd_rd_wg=NGaZT&pid=uicVlaC&qid=1752795015&s=industrial&sprefix=non%2Bself%2Bthreading%2Bscrew%2Cindustrial%2C122&sr=1-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1)"> Link </a> |
| Column | Acts as standoff between PCB and acrylic shell | $0.30 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Csdtylh-Male-Female-Standoff-Stainless-Assortment/dp/B06Y5TJXY1/ref=sr_1_2?crid=3NU4Q8QHEIBNY&dib=eyJ2IjoiMSJ9.SROPdtLRZ0wppidkVVHZbAy1Lqsyy7BDI3O1XRygzX06JOdLomeO6nn9j8nJkPkeWWP3bZEF4WcxFbcQ26u8ulTkPJiv3xYMv3mUmF4MIR9kF55mwA29Z4w6ys1Q76HHyNhah3vMNzhIMhZyO980zg4s3BcJRHUNFXIhGU7opYC9Kaf6lIIA8xbJaJCNsX4ATW2CgkX1XWOd4P4-L10o7ln18ZUh4dHF0bIOLuhnG4uMmYqV_HmtPXR6bKONafNsu_oUxyNT4-EgibB-UvkECkMyyd-hzuIYbfvvKfHYbj4.dXo8Fa7RkMtwFUG9Ih_G9P-IEiW1tX1bTvvCgsC-fu8&dib_tag=se&keywords=arduino%2Bmetal%2Bstandoff&qid=1752795037&s=industrial&sprefix=arduino%2Bmetal%2Bstandof%2Cindustrial%2C129&sr=1-2&th=1)"> Link </a> |
| Battery Case | Allows for batteries to be placed in and provide long-term power | $7 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/Battery-Holder-Barrel-Connector-Arduino/dp/B07T65WWCR/ref=sr_1_1?crid=1OYXN0JTMLBQ1&dib=eyJ2IjoiMSJ9.ghtrwrmtGzNZNMN4_tCMJiIaiC29HwmDKl2OB7D2rZWC4ziKD6XJnm1cpNA6pR21hdaTsmF7jr1qq8MQtj129bHnusbMBy7K8ggKaApevF0gcZGxNB6PfzNw3ICqhJ9dht_CNasG6ROTdpBxEhHUmoqCFWxbwhDnkzqjwVbR1SqJ0q7looK4T22CQ0xGjpDxz5-kojyVjiuJrNXw4-KqgwGc4txp1LMnk9MB4CR0nAQ.4Pq7-z8RyRvipV96QvhQPgZDZtMUKh_YOreMBiFtCfM&dib_tag=se&keywords=arduino+battery+case&qid=1752795059&s=industrial&sprefix=arduino+battery+cas%2Cindustrial%2C123&sr=1-1)"> Link </a> |
| Acrylic shell | Allows for device to be more wieldy | $10 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/SunFounder-Transparent-Compatible-Original-Included/dp/B0CGDTD5PJ/ref=sr_1_3?crid=288PH8MAWLA9M&dib=eyJ2IjoiMSJ9.n0BJ_rb3yZ0sHqcY7KsFTNb4Sxyn6906z4y52jSf_GpUkV90aelkNJys4U9PWc6jkSLuls7eHEjk9Sq2z8e25bMVabogobhV5hpZ1QiNdNnYMF76B8r6CLC9pqQ-vJBPSjzFkCzTKXQ2pIMdd_i18zCw8zX6Gwr8nknWOvIIG29hWqJKvAM8dzPKbvtKXkFjwE-MzRvVX246ZRmHn4ilbHNdofGX5pYZFA0cXB3byu8.e9o5mTvvtq0UR2M51Np-ozBihJJP_Qw_xsTAz5sgE1U&dib_tag=se&keywords=acrylic+shell+arduino&qid=1752795114&sprefix=acrylic+shell+arduino%2Caps%2C121&sr=8-3)"> Link </a> |

# Retro Arcade Gaming Console Starter Project

<img src = "DominicR.jpg" width = "450" height = "600">

<iframe width="560" height="315" src="https://www.youtube.com/embed/cbm1Ko4pPN8?si=BsYM3Q0l8w3hW4iN" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
My project was the Retro Arcade Gaming Console. It came with a PCB with all the software already installed, and what I had to do was solder all the components onto the board and hook up a battery pack to power it. One challenge I faced was that I accidentally bridged the capacitor when soldering it, and I had to use desoldering wick and soldering paste to remove the solder so I could redo the solder connection. I'm excited to continue on to the intensive project, where I will be creating a ball-tracking robot.

# Starter Project Kit

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Vogurtime DIY Solder Project Game Kit | Used this kit and followed its instructions to create starter project | $18.99 | <a href="(https://www.amazon.com/Soldering-ElectronicsPracticing-Learning-Comfortable-VOGURTIME/dp/B094QRRHC2?th=1)"> Link </a> |

# Schematics (Starter Project)
<!--
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 
-->
![Headstone Image](schematics-_WNfuLqZO8t.jpg)

# Other Resources/Examples
- [Raspberry Pi Pin Layout]([https://trashytuber.github.io/YimingJiaBlueStamp/](https://www.raspberrypi-spy.co.uk/2012/06/simple-guide-to-the-rpi-gpio-header-and-pins/))
- [Example Ball-Tracking Robot Project]([https://sviatil0.github.io/Sviatoslav_BSE/](https://deringur.github.io/BSE_Derin_Portfolio/))
