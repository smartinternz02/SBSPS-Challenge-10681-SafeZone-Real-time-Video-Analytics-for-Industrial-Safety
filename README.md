# Object-Detection
import math
from ultralytics import YOLO
import cv2
import cvzone

#input from webcam
cap = cv2.VideoCapture(0) #for webcam
cap.set(3, 1280)
cap.set(4, 720)

model = YOLO('../YOLO-Weights/yolov8n.pt')
classNames = ["person", "bicycle", "car", "motorbike", "aeroplane", "bus", "train", "truck", "beat", "traffic Light", "fire hydrant", "stop sign", "parking meter", "bench", "bird", "cat", "dog", "horse", "sheep", "cow", "elephant", "bear", "zebra", "giraffe", "backpack", "umbrella", "handbag", "tie", "suitcase", "frisbee", "skis", "snowboard", "sports ball", "kite", "baseball bat", "baseball glove", "skateboard", "surfboard", "tennis racket", "bottle", "wine glass", "cup", "fork", "knife", "spoon", "bowl", "banana", "apple", "sandwich", "orange", "broccoli", "carrot", "hot dog", "pizza", "donut", "cake", "chair", "sofa", "potted plant", "bed", "dining table", " toilet", "tv monitor", "laptop", "mouse", "remote", "keyboard", "call phone", "microwave", "oven", "toaster", "sink", "refrigerator", "book", "clock", "vase", "scissors", " teddy bear", "hair driers", "toothbrush"]

while True:
    success, img = cap.read()
    results = model(img, stream=True)
    for r in results:
        boxes = r.boxes
        for box in boxes:

            # bounding box
            x1, y1, x2, y2 = box.xyxy[0]
            x1, y1, x2, y2 = int(x1), int(y1), int(x2), int(y2)
            w, h = x2-x1, y2-y1
            cvzone.cornerRect(img, (x1, y1, w, h), l=15)

            # confidence
            conf = math.ceil((box.conf[0]*100))/100

            # class name
            cls = int(box.cls[0])

            cvzone.putTextRect(img, f'{classNames[cls]} {conf}', (max(0, x1), max(40, y1)), scale=1, thickness=1)


    cv2.imshow("Image", img)
    cv2.waitKey(1)
