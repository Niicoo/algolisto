Video Capture
=============

Webcam Capture
##############

Basic example showing how to open the webcam, show the video to the user and saving it to an output file.

.. code-block:: python

    import cv2

    # Capture video from webcam
    cap = cv2.VideoCapture(0)

    # Output video
    writer = cv2.VideoWriter("webcam.mp4", cv2.VideoWriter_fourcc(*'mp4v'), 25, (640, 480))

    while(True):
        # Capture frame
        ret, frame = cap.read()
        # Write frame to the output file
        writer.write(frame)
        # Show the frame
        cv2.imshow("Webcam Capture", frame)
        # Break the loop when pressing "x" key
        if cv2.waitKey(1) &0XFF == ord('x'):
            break

    cap.release()
    writer.release()
    cv2.destroyAllWindows()


There are a lot of problems in this code:

- The device port number is set manually (:code:`0`), in case of multiple devices, we need to be able to chose the webcam desired.
- We don't know the true native resolution of the webcam, it is set by default to 640x480. Also, we don't know which resolutions are available/valid for this device.
- The input framerate from the webcam is variable whereas we set it manually for the output video file.

List available devices
**********************

.. code-block:: python

    import cv2

    ports_availables = []
    for no_port in range(1000):
        cap = cv2.VideoCapture(no_port)
        if not cap.isOpened(): continue
        ret, frame = cap.read()
        if ret:
            ports_availables.append(no_port)

    print("Ports available: ", ports_availables)


List available resolutions
**************************

One way to do it is to manually test every possible resolution:

.. code-block:: python

    import cv2

    # Data got from https://en.wikipedia.org/wiki/List_of_common_resolutions
    RESOLUTIONS_LIST = [
        (16, 16), (42, 11), (32, 32), (40, 30), (42, 32), (48, 32), (60, 40),
        (84, 48), (64, 64), (72, 64), (128, 36), (75, 64), (150, 40), (96, 64),
        (96, 64), (128, 48), (96, 65), (102, 64), (101, 80), (96, 96), (240, 64),
        (160, 102), (128, 128), (160, 120), (160, 144), (144, 168), (160, 152),
        (160, 160), (140, 192), (160, 200), (224, 144), (208, 176), (240, 160),
        (220, 176), (160, 256), (208, 208), (256, 192), (280, 192), (256, 212),
        (432, 128), (256, 224), (240, 240), (256, 240), (320, 192), (320, 192),
        (320, 200), (256, 256), (256, 256), (320, 208), (320, 224), (320, 240),
        (320, 256), (384, 224), (368, 240), (376, 240), (272, 340), (400, 240),
        (512, 192), (448, 224), (320, 320), (432, 240), (560, 192), (400, 270),
        (512, 212), (384, 288), (480, 234), (400, 300), (480, 250), (312, 390),
        (512, 240), (320, 400), (640, 200), (480, 272), (512, 256), (512, 256),
        (416, 352), (480, 320), (640, 240), (640, 240), (640, 256), (512, 342),
        (368, 480), (496, 384), (800, 240), (512, 384), (640, 320), (640, 350),
        (640, 360), (480, 500), (512, 480), (720, 348), (720, 350), (640, 400),
        (720, 364), (800, 352), (600, 480), (640, 480), (640, 512), (768, 480),
        (800, 480), (848, 480), (854, 480), (800, 600), (960, 540), (832, 624),
        (960, 544), (1024, 576), (960, 640), (1024, 600), (1024, 640), (960, 720),
        (1136, 640), (1138, 640), (1024, 768), (1024, 800), (1152, 720), (1152, 768),
        (1280, 720), (1120, 832), (1280, 768), (1152, 864), (1334, 750), (1280, 800),
        (1152, 900), (1024, 1024), (1366, 768), (1280, 854), (1280, 960), (1600, 768),
        (1080, 1200), (1440, 900), (1440, 900), (1280, 1024), (1440, 960), (1600, 900),
        (1400, 1050), (1440, 1024), (1440, 1080), (1600, 1024), (1680, 1050),
        (1776, 1000), (1600, 1200), (1600, 1280), (1920, 1080), (1440, 1440),
        (2048, 1080), (1920, 1200), (2048, 1152), (1792, 1344), (1920, 1280),
        (2280, 1080), (2340, 1080), (1856, 1392), (2400, 1080), (1800, 1440),
        (2880, 900), (2160, 1200), (2048, 1280), (1920, 1400), (2520, 1080),
        (2436, 1125), (2538, 1080), (1920, 1440), (2560, 1080), (2160, 1440),
        (2048, 1536), (2304, 1440), (2256, 1504), (2560, 1440), (2304, 1728),
        (2560, 1600), (2880, 1440), (2960, 1440), (2560, 1700), (2560, 1800),
        (2880, 1620), (2560, 1920), (3440, 1440), (2736, 1824), (2880, 1800),
        (2560, 2048), (2732, 2048), (3200, 1800), (2800, 2100), (3072, 1920),
        (3000, 2000), (3840, 1600), (3200, 2048), (3240, 2160), (5120, 1440),
        (3200, 2400), (3840, 2160), (4096, 2160), (3840, 2400), (4096, 2304),
        (5120, 2160), (4480, 2520), (4096, 3072), (4500, 3000), (5120, 2880),
        (5120, 3200), (5120, 4096), (6016, 3384), (6400, 4096), (6400, 4800),
        (6480, 3240), (7680, 4320), (7680, 4800), (8192, 4320), (8192, 4608),
        (10240, 4320), (8192, 8192), (15360, 8640)
    ]

    cap = cv2.VideoCapture(0)
    resolutions_available = set()
    for width, height in RESOLUTIONS_LIST:
        cap.set(cv2.CAP_PROP_FRAME_WIDTH, width)
        cap.set(cv2.CAP_PROP_FRAME_HEIGHT, height)
        width = cap.get(cv2.CAP_PROP_FRAME_WIDTH)
        height = cap.get(cv2.CAP_PROP_FRAME_HEIGHT)
        resolutions_available.add((int(width), int(height)))

    print(resolutions_available)


Capture video with a fixed framerate
************************************

There is no easy and reliable way to set the framerate of the input video capture as it depends on:

- Your camera's capture capabilities
- Your computer's system resources
- Whether the current capture backend you're using supports changing frame rates

See https://stackoverflow.com/questions/52068277/change-frame-rate-in-opencv-3-4-2 and https://stackoverflow.com/questions/12652401/how-to-set-framerate-with-opencv-camera-capture for more info.

The best easy solution is to use a method as below:

.. code-block:: python

    import cv2
    import time

    # Capture video from webcam
    cap = cv2.VideoCapture(0)

    FPS = 15

    time_prev = time.perf_counter()
    while(True):
        # Capture frame
        time_elapsed = time.perf_counter() - time_prev
        ret, frame = cap.read()
        
        if time_elapsed > 1./FPS:
            time_prev = time.perf_counter()
            # Process frame
            # ...
            cv2.imshow("Webcam Capture", frame)
        # Break the loop when pressing "x" key
        if cv2.waitKey(1) &0XFF == ord('x'):
            break

    cap.release()
    cv2.destroyAllWindows()


Screen Capture
##############

If you want to capture your screen, you can use the (:code:`PIL.ImageGrab.grab`) method, however it doesn't work on Linux systems (at least for me).
Alternatively, you can use the package **mss** (:code:`conda install -c conda-forge python-mss`).

More info here: https://stackoverflow.com/questions/24129253/screen-capture-with-opencv-and-python-2-7

.. code-block:: python

    import cv2
    import numpy as np
    from mss import mss

    mon = {'top': 0, 'left': 0, 'width': 1920, 'height': 1200}
    with mss() as sct:
        while True:
            img = sct.grab(mon)
            cv2.imshow('Screen Capture', np.array(img))
            if cv2.waitKey(25) & 0xFF == ord('q'):
                break
        cv2.destroyAllWindows()


------------------------------------------------------------

**Sources**:

- Capturing screen: https://stackoverflow.com/questions/24129253/screen-capture-with-opencv-and-python-2-7
- Find possible resolutions: https://www.learnpythonwithrune.org/find-all-possible-webcam-resolutions-with-opencv-in-python/
- Set a fixed framerate: https://stackoverflow.com/questions/52068277/change-frame-rate-in-opencv-3-4-2
