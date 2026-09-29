# User-Centric AI Models for Assisting the Blind

Capstone project by **Chisava Tshwarano Lekoana** .

This project trains a YOLO object detection model to identify 19 everyday object classes. A MATLAB prototype displays detections and provides spoken feedback. It supports photos, folders of photos, questions about a photo, and live object search through a phone stream.

## Object classes

Person, bicycle, car, motorcycle, bus, truck, traffic light, stop sign, bench, dog, cat, chair, couch, dining table, bed, toilet, cell phone, bottle, and refrigerator.

## Data and training

A selected subset of COCO 2017 contains **21,000 images**: 18,000 training, 1,500 validation, and 1,500 test images. Training used Ultralytics YOLO on a Kaggle T4 GPU. Images contain class labels and bounding boxes.

One reported validation run produced **66.2% precision**, **56.2% recall**, **62.0% mAP50**, and **42.1% mAP50–95**. These figures describe that validation run; the live `best.pt` checkpoint should be evaluated separately before attributing the same figures to it.

## Prototype features

| MATLAB menu option | Feature |
| --- | --- |
| 1 | Detect objects in one photo |
| 2 | Step through a folder of photos |
| 3 | Ask a question about a photo |
| 10 | Search for an object in a live phone stream |

The photo workflow uses an ONNX model imported into MATLAB. The live phone workflow uses a Python worker to run `best.pt` and send detections to MATLAB. The proximity beep is based on the object's apparent size in the image; it does not measure physical distance.

## Run the live demonstration

1. Install Python packages: `py -m pip install ultralytics opencv-python`.
2. Put `phone_worker.py` beside your `best.pt` file.
3. Start your phone's video-streaming app and connect the phone and laptop to the same network. Use the stream URL shown by the app.
4. In PowerShell, start the worker (replace the example URL):

   ```powershell
   py phone_worker.py --model best.pt --source "http://192.168.0.62:8080/video"
   ```

5. Leave PowerShell running. In MATLAB, open the project folder, run `main_capstone`, and select **option 10**. Enter an object class such as `cell phone`, `bottle`, or `chair`.

For photo features, the MATLAB project also needs the relevant `.m` files and the exported `models/best.onnx`. The exact MATLAB toolboxes and support packages depend on the installed MATLAB release. Check the project configuration before running it on another laptop.

## Project files

- `main_capstone.m`: MATLAB menu.
- `phoneSearchAssistant.m`: live object search and feedback in MATLAB.
- `phone_worker.py`: YOLO inference service for the live stream.
- MATLAB detection and speech `.m` files: photo analysis, questions, and audio output.
- Kaggle notebook: data preparation, training, and evaluation (add the notebook to this repository if available).
- `best.pt` and `best.onnx`: model checkpoints/exports, supplied separately if too large for GitHub.

## Limitations

The model is limited to the 19 listed classes. Lighting, motion, occlusion, and small objects may reduce detection quality. The prototype has not been validated as an independent navigation aid or tested with intended users.
