# Smart Football Player Analytics

## Project Overview

**Smart Football Player Analytics** is a computer vision project developed as part of the **Computer Vision training program at SDAIA Academy**.

The project demonstrates practical applications of computer vision in football match analysis using **Python, YOLOv8, OpenCV, ByteTrack, and Supervision**.

The system analyzes football match videos to detect and track players, classify players into teams, visualize player movement, identify tactical zones, detect crowding situations, and combine the results into an integrated football analytics system.

The project focuses on applying computer vision techniques to a real-world scenario, with an emphasis on **object detection, object tracking, image processing, movement analysis, and automated tactical analysis**.

---

## Training Program

**SDAIA Academy – Computer Vision Training Program**

This project was developed as part of the practical projects and exercises completed during the Computer Vision training program.

### SDAIA Academy

[GitHub – SDAIA Academy](https://github.com/SDAIAAcademy)

---

## Technologies

* **Python**
* **YOLOv8**
* **OpenCV**
* **ByteTrack**
* **Supervision**
* **NumPy**
* **SciPy**
* **PyTorch**
* **FFmpeg**
* **Google Colab**

---

## Project Objectives

The main objectives of the project are:

* Detect football players using YOLOv8.
* Track players across video frames using ByteTrack.
* Assign a unique ID to each tracked player.
* Classify players into two teams based on shirt color.
* Generate a player movement heatmap.
* Visualize player movement trails.
* Divide the football field into tactical zones.
* Detect crowding situations between players.
* Integrate all analysis components into a single football analytics system.

---

## System Workflow

```text
Football Match Video
        │
        ▼
Player Detection
        │
        ▼
Player Tracking
     ByteTrack
        │
        ├──────────────► Team Classification
        │
        ├──────────────► Movement Heatmap
        │
        ├──────────────► Movement Trails
        │
        ├──────────────► Tactical Zones
        │
        └──────────────► Crowding Detection
                              │
                              ▼
                 Integrated Analytics Video
```

---

# Tasks

## Task 1 — Player Detection and Tracking

This task detects football players in each video frame using **YOLOv8** and assigns a unique tracking ID to each player using **ByteTrack**.

### Main Features

* Detects people using YOLOv8.
* Filters unrealistic detections.
* Tracks players across consecutive frames.
* Assigns a unique ID to each player.
* Stores bounding box coordinates.
* Stores player center positions.
* Generates an annotated tracking video.

### Model

```python
model = YOLO("yolov8m.pt")
```

### Tracker

```python
tracker = sv.ByteTrack()
```

### Tracking Data

Each detected player is stored with:

```python
{
    "id": track_id,
    "bbox": (x1, y1, x2, y2),
    "center": (cx, cy)
}
```

### Output

```text
task1_player_tracking_fixed.mp4
```

---

## Task 2 — Team Classification

This task classifies detected players into two teams based on the dominant color of their shirts.

The tracking information generated in Task 1 is reused to locate each player throughout the video.

### Classification Process

1. Extract the player's bounding box.
2. Crop the player's body region.
3. Extract the shirt region.
4. Convert the image from BGR to HSV.
5. Analyze the dominant colors.
6. Assign the player to a team.

### Team Labels

```text
Team A
Team B
Unknown
```

Red shirt regions are classified as **Team A**, while sufficiently white shirt regions are classified as **Team B**.

### Output

```text
task2_team_classification_fixed.mp4
```

---

## Task 3 — Player Movement Heatmap

This task generates a heatmap showing the areas most frequently occupied by players throughout the video.

The center position of each tracked player is accumulated to identify areas of high and low player activity.

### Process

* Collect player center positions.
* Accumulate positions on a heatmap.
* Apply Gaussian blur.
* Normalize the heatmap.
* Apply a color map.
* Overlay the heatmap on the original video.

### Example

```python
cx, cy = player["center"]

cv2.circle(
    heatmap,
    (cx, cy),
    25,
    1.0,
    -1
)
```

### Output

```text
task3_heatmap_fixed.mp4
```

---

## Task 4 — Movement Trails and Tactical Zones

This task visualizes player movement trails and divides the football field into three tactical zones.

```text
Defense | Midfield | Attack
```

Each player is assigned to a tactical zone based on the horizontal position of their bounding-box center.

### Tactical Zone Logic

```python
if cx < zone_w:
    zone = "DEFENSE"

elif cx < zone_w * 2:
    zone = "MIDFIELD"

else:
    zone = "ATTACK"
```

### Movement Trails

The system stores the most recent **40 positions** for each player and connects these positions to visualize their movement.

### Zone Statistics

The output displays the number of players currently located in:

* Defense
* Midfield
* Attack

### Output

```text
task4_trails_zones_fixed.mp4
```

---

## Task 5 — Crowding Detection

This task detects situations where multiple players are positioned close to one another.

The system calculates the pairwise distances between player center positions.

A crowding situation is detected when at least **four players** are located within the defined distance threshold.

### Parameters

```python
CROWD_DISTANCE = min(W, H) * 0.12
MIN_PLAYERS = 4
```

### Distance Calculation

```python
distances = cdist(
    points,
    points
)
```

The system counts a continuous crowding situation as a single event. A new event is counted only after the players separate and another crowding situation occurs.

### Output

```text
task5_crowding_fixed.mp4
```

---

# Final Integrated System

The final stage combines all previous tasks into one complete football analytics system.

The integrated system includes:

* Player detection
* Player tracking
* Unique player IDs
* Team classification
* Movement heatmap
* Movement trails
* Tactical zones
* Crowding detection
* Tactical dashboard

Each player is analyzed using:

```text
Player ID
    │
    ├── Bounding Box
    ├── Center Position
    ├── Team
    ├── Tactical Zone
    └── Movement Trail
```

The final video also displays a tactical dashboard containing:

```text
SMART FOOTBALL ANALYTICS

Players: XX

DEF: X | MID: X | ATT: X

Crowding Events: X

STATUS: NORMAL
```

When crowding is detected:

```text
TACTICAL ALERT: HIGH CROWDING
```

### Final Output

```text
smart_football_final_fixed.mp4
```

---

# Project Outputs

| Task                             | Output                                |
| -------------------------------- | ------------------------------------- |
| Player Detection & Tracking      | `task1_player_tracking_fixed.mp4`     |
| Team Classification              | `task2_team_classification_fixed.mp4` |
| Player Movement Heatmap          | `task3_heatmap_fixed.mp4`             |
| Movement Trails & Tactical Zones | `task4_trails_zones_fixed.mp4`        |
| Crowding Detection               | `task5_crowding_fixed.mp4`            |
| Integrated Analytics System      | `smart_football_final_fixed.mp4`      |

---

# Project Structure

```text
Smart-Football-Player-Analytics/
│
├── README.md
│
├── notebooks/
│   └── smart_football_analytics.ipynb
│
├── models/
│   └── yolov8m.pt
│
├── data/
│   └── shot1.mp4
│
├── outputs/
│   ├── task1_player_tracking_fixed.mp4
│   ├── task2_team_classification_fixed.mp4
│   ├── task3_heatmap_fixed.mp4
│   ├── task4_trails_zones_fixed.mp4
│   ├── task5_crowding_fixed.mp4
│   └── smart_football_final_fixed.mp4
│
└── requirements.txt
```

---

# Installation

Install the required Python libraries:

```bash
pip install -U ultralytics supervision opencv-python lapx scipy
```

FFmpeg is also required for video conversion and playback.

---

# How to Run

The project can be executed using **Google Colab** or a compatible Python environment.

### 1. Open the Notebook

Open:

```text
notebooks/smart_football_analytics.ipynb
```

in Google Colab.

### 2. Prepare the Input Video

Place the football match video in the required location.

For Google Drive:

```python
video_path = "/content/drive/MyDrive/shot1.mp4"
```

### 3. Run the Tasks in Order

The tasks should be executed in the following order:

```text
Task 1
  ↓
Task 2
  ↓
Task 3
  ↓
Task 4
  ↓
Task 5
  ↓
Final Integrated System
```

Later tasks reuse the tracking information generated during Task 1.

---

# Requirements

* Python 3.x
* Google Colab or compatible Python environment
* GPU recommended for faster YOLOv8 inference
* Football match video
* YOLOv8 model
* FFmpeg
* Required Python libraries

---

# Technical Documentation

The project uses several computer vision techniques:

### YOLOv8

YOLOv8 is used for detecting players in individual video frames.

### ByteTrack

ByteTrack is used to associate player detections between consecutive frames and maintain a unique tracking ID.

### HSV Color Analysis

HSV color space is used to analyze the dominant shirt colors for team classification.

### Heatmap Analysis

Player center positions are accumulated and blurred to visualize areas of frequent player activity.

### Tactical Zone Analysis

The video frame is divided into three vertical zones to provide a simplified tactical positioning analysis.

### Distance-Based Crowding Detection

Pairwise Euclidean distances between players are calculated to identify situations where multiple players are positioned close together.

---

# Version Control

Git was used to manage the project development and maintain a structured version history.

The repository follows basic Git best practices, including:

* Meaningful commit messages.
* Organized project structure.
* Separate folders for notebooks, models, data, and outputs.
* Documentation through `README.md`.
* Tracking project changes using Git.
* Avoiding unnecessary files in the repository.

Example commit structure:

```text
feat: add player detection and tracking
feat: add team classification
feat: add player movement heatmap
feat: add tactical zones and movement trails
feat: add crowding detection
feat: integrate final football analytics system
docs: update project README
```

---

# Applications

This project can be applied to:

* Football match analysis.
* Player movement analysis.
* Tactical analysis.
* Team positioning analysis.
* Sports analytics.
* Player tracking.
* Automated video analysis.
* Computer vision research.
* Performance analysis.

---

# Future Improvements

Potential future improvements include:

* Training a dedicated team classification model.
* Automatic football pitch detection.
* Perspective transformation to real-world coordinates.
* Player speed estimation.
* Distance covered by each player.
* Ball detection and tracking.
* Possession estimation.
* Pass detection.
* Formation recognition.
* Advanced tactical event detection.
* Player performance statistics.
* Real-time analytics dashboard.

---

# Conclusion

The **Smart Football Player Analytics** project demonstrates how computer vision techniques can be combined to analyze football match footage.

By integrating **YOLOv8, ByteTrack, OpenCV, Supervision, NumPy, and SciPy**, the system can detect and track players, classify teams, visualize player movement, identify tactical zones, detect crowding situations, and present the results through an integrated analytics video.

The project provides a foundation for developing more advanced automated football tactical analysis systems.

---

## Training Program

**SDAIA Academy – Computer Vision**

### SDAIA Academy GitHub

https://github.com/SDAIAAcademy

---

## Project Workflow

Football Video  
↓  
Player Detection  
↓  
Player Tracking  
↓  
Team Classification  
↓  
Movement Analysis  
↓  
Tactical Zone Analysis  
↓  
Crowding Detection  
↓  
Integrated Football Analytics
