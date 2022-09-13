Video Editing
=============

Below an example:

- opening a video file
- process the frames one-by-one 
- write a video file using the processed frames


.. code-block:: python

    import cv2

    videopath_in = "video_in.mp4"
    videopath_out = "video_out.mp4"

    # Input video
    cap = cv2.VideoCapture(videopath_in)

    # Input video parameters
    FPS = cap.get(cv2.CAP_PROP_FPS)
    WIDTH = int(cap.get(cv2.CAP_PROP_FRAME_WIDTH))
    HEIGHT = int(cap.get(cv2.CAP_PROP_FRAME_HEIGHT))

    # Output video
    writer = cv2.VideoWriter(videopath_out, cv2.VideoWriter_fourcc(*'mp4v'), FPS, (WIDTH, HEIGHT))

    no_frame = 0
    while(cap.isOpened()):
        # Get input frame
        ret, frame = cap.read()
        # frame shape: (height, width, channels = 3)
        # frame dtype: uint8
        # frame channels: BGR

        if ret == False:
            break

        # Do whatever you want with the frame here
        frame[200:600, : , :] = (255, 0, 0)

        # Write the frame to the output file
        writer.write(frame)

        # Progression
        print("\rNo frame = %d" % no_frame, end="")

    writer.release()
    cap.release()
    cv2.destroyAllWindows()
