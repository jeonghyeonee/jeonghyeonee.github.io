---
layout: post
title: Video Play
categories: OpenCV
tags:  [Video, video, OpenCV, Python]
---

## Videop Play using OpenCV with Python

### When you use OpenCV
When you want to play the video using Python, you can use OpenCV.
(It is just a part of OpenCV's function)

### How to play Video using OpenCV

```
import cv2

# Open a Video file
# VideoCapture('<file_name>')
video = cv2.VideoCapture('test.mp4')
```

##### References
- [Getting Started with Videos](https://docs.opencv.org/4.x/dd/d43/tutorial_py_video_display.html)
- [Video Capture Function](https://docs.opencv.org/3.4/d8/dfe/classcv_1_1VideoCapture.html)
- [Python | Play a video using OpenCV](https://www.geeksforgeeks.org/python-play-a-video-using-opencv/)
- [OpenCV 동영상 파일 재생하기기](https://scribblinganything.tistory.com/491)
- [Read, Write and Display a video using OpenCV](https://learnopencv.com/read-write-and-display-a-video-using-opencv-cpp-python/)