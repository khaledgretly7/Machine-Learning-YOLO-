# Machine-Learning-YOLO-
# YOLO++ Research Project Master Documentation

# Project Title

Real-Time Object Detection for Autonomous Vehicles Using Enhanced YOLOv8 Architecture (YOLO++) and Future Hybrid Fusion Framework

---

# Team Member

Khaled Elgreitly

---

# Project Goal

Develop a real-time object detection framework for autonomous vehicles capable of:

* Detecting road objects accurately.
* Maintaining high FPS for real-time deployment.
* Improving small object detection.
* Remaining lightweight for edge deployment.
* Providing a research contribution beyond standard YOLOv8.

---

# Initial Baseline

The project started from:

YOLOv8n (Ultralytics)

Reasons:

* Lightweight
* Real-time
* Strong baseline
* Widely used in autonomous driving research

Baseline Architecture:

Backbone:

* Conv
* C2f Blocks
* SPPF

Neck:

* PAN-FPN

Head:

* Anchor-Free Detect Head

---

# Dataset Selection Process

Several datasets were considered:

## KITTI

Advantages:

* Autonomous driving focused

Disadvantages:

* Small size
* Limited classes
* Mostly daytime

Result:
Not selected.

---

## Open Images

Advantages:

* Huge dataset

Disadvantages:

* Inconsistent annotations
* Very large

Result:
Not selected.

---

## COCO 2017

Advantages:

* Large scale
* High quality annotations
* Industry standard benchmark
* Contains road-related classes

Result:
Selected.

---

# Final Dataset

Original COCO Classes Filtered

Selected Road-Relevant Classes:

1. Person
2. Bicycle
3. Car
4. Motorcycle
5. Bus
6. Train
7. Truck
8. Traffic Light
9. Stop Sign
10. Parking Meter
11. Bench
12. Dog
13. Cat

Validation Images:

3327

Instances:

16029

---

# Initial Baseline Training

Model:

YOLOv8n

Training Environment:

Server GPU:
NVIDIA RTX A6000

Framework:

Ultralytics YOLOv8

PyTorch

CUDA

---

# YOLO++ Modifications

The enhanced model was called:

YOLO++

Later final version:

YOLOv8_allin3

---

# Modification 1

SE Attention Block

Research Origin:

Squeeze-and-Excitation Networks
(Hu et al., CVPR 2018)

Purpose:

Improve channel-wise feature selection.

Problem in Baseline:

All channels treated equally.

Solution:

Global Average Pooling

↓

Channel Importance Calculation

↓

Feature Reweighting

---

Implementation

Added custom SE block.

Inserted after final neck feature stage.

Code:

Global Average Pooling

FC1

SiLU

FC2

Sigmoid

Feature Scaling

---

Benefits

* Better feature selection
* Better small object representation
* Minimal computational overhead

---

# Modification 2

Neck-Only Attention

Purpose:

Enhance feature fusion stage without increasing backbone complexity.

Reason:

Backbone already extracts features efficiently.

Most object-scale information is fused inside the neck.

Applied:

Feature Fusion Region

Instead of Backbone

Benefits:

* Better multi-scale fusion
* Improved small object handling
* Preserved inference speed

---

# Modification 3

Feature Refinement

Enhanced PAN Structure

Purpose:

Preserve fine-grained details.

Problem:

Small object features often disappear during repeated downsampling.

Solution:

Refined neck fusion strategy.

Benefits:

* Better traffic light detection
* Better bicycle detection
* Better distant object recognition

---

# Modification 4

Loss Tuning

Purpose:

Improve convergence.

Changes:

Learning Rate Scheduling

Cosine Learning Rate

Close Mosaic

Training Parameter Optimization

Benefits:

* More stable training
* Better recall
* Better final mAP

---

# Transfer Learning Experiment

Objective:

Leverage pretrained COCO knowledge.

Training:

pretrained=yolov8n.pt

freeze=10

Result:

Transfer learning did not improve final metrics significantly.

Performance decreased slightly.

Conclusion:

YOLO++ architecture already adapted well.

Transfer learning was not selected as final model.

---

# Final YOLO++ Results

Overall Metrics

Precision:

0.756

Recall:

0.562

mAP50:

0.632

mAP50-95:

0.456

Inference Time:

1.0 ms

FPS:

~120 FPS

GFLOPs:

8.1

Parameters:

3.02 Million

---

# Baseline Comparison

YOLOv8 Baseline

Precision:
0.750

Recall:
0.549

mAP50:
0.625

mAP50-95:
0.451

Inference:
1.1 ms

---

YOLO++

Precision:
0.756

Recall:
0.562

mAP50:
0.632

mAP50-95:
0.456

Inference:
1.0 ms

---

Observed Improvements

Precision:
+0.6%

Recall:
+2.3%

mAP50:
+1.1%

mAP50-95:
+1.1%

Inference:
~9% Faster

---

# Class-wise Results

Person:
mAP50 = 0.749

Bicycle:
mAP50 = 0.443

Car:
mAP50 = 0.571

Motorcycle:
mAP50 = 0.648

Bus:
mAP50 = 0.737

Truck:
mAP50 = 0.448

Traffic Light:
mAP50 = 0.424

Stop Sign:
mAP50 = 0.692

Fire Hydrant:
mAP50 = 0.785

Bench:
mAP50 = 0.302

Parking Meter:
mAP50 = 0.622

Dog:
mAP50 = 0.750

Cat:
mAP50 = 0.877

---

# Performance Analysis

Strong Classes:

* Person
* Cat
* Dog
* Bus
* Fire Hydrant

Weak Classes:

* Traffic Light
* Bicycle
* Bench

Reason:

Small Object Detection Limitation

---

# Streamlit Deployment

Implemented:

Image Upload

Video Upload

USB Camera

IP Camera

Confidence Slider

IoU Slider

Start Detection

Stop Detection

Real-Time Visualization

---

# Current Limitation

Although YOLO++ is very fast:

120 FPS

Small object localization still needs improvement.

Examples:

* Traffic Lights
* Distant Bicycles
* Far Stop Signs

---

# New Research Direction

Hybrid Detection Framework

Inspired By:

Hybrid YOLOv9 + FasterRCNN Research Paper

IEEE Document:
11077147

---

# Proposed Research Title

YOLO++-RCNN

A Hybrid Adaptive Fusion Framework for Real-Time Autonomous Driving Object Detection

---

# Motivation

YOLO++

Strengths:

* High speed
* Real-time
* Lightweight

Weaknesses:

* Small object localization

FasterRCNN

Strengths:

* Strong localization
* Better small object performance

Weaknesses:

* Slow

---

# Proposed Hybrid Architecture

Image

↓

YOLO++

↓

Feature Extraction

↓

FasterRCNN Feature Extraction

↓

Adaptive Feature Fusion

↓

Detection Head

↓

Final Prediction

---

# Adaptive Feature Fusion

Goal:

Fuse strengths of both detectors.

Possible Fusion:

Fusion = αYOLO + (1-α)RCNN

Advanced Version:

Attention-Guided Fusion

weights = sigmoid(conv(features))

fusion = weights * YOLO
+ (1-weights) * RCNN

---

# Planned Research Stages

Stage 1

Train FasterRCNN

on same 13 classes dataset.

---

Stage 2

Evaluate FasterRCNN.

Metrics:

* Precision
* Recall
* mAP50
* mAP50-95
* FPS

---

Stage 3

Extract Feature Maps

YOLO++

FasterRCNN

---

Stage 4

Implement Adaptive Feature Fusion

---

Stage 5

Train Hybrid Framework

---

Stage 6

Ablation Study

Models:

1. FasterRCNN

2. YOLOv8 Baseline

3. YOLO++

4. YOLO++ + Fusion

5. YOLO++ + Adaptive Fusion

6. Full Hybrid

---

# Future Work

Potential Extension:

YOLO++ + RT-DETR

Reasons:

* Transformer-based
* Better global context
* Modern architecture
* Strong small-object performance
* Real-time capable

Potential Publication Direction:

Q1 Journal / IEEE Conference

---

# Key Discussion Points

Why YOLOv8?

* Stable
* Lightweight
* Mature ecosystem

Why not YOLOv9?

* Higher complexity
* Harder deployment
* Focus was lightweight real-time systems

Why SE?

* Lightweight attention
* Minimal GFLOPs increase

Why Neck Attention?

* Better multi-scale fusion

Why Loss Tuning?

* Better convergence

Why Hybrid?

* Combine speed and localization

Why FasterRCNN?

* Strong small-object detection

Why Adaptive Fusion?

* Learn optimal detector contribution dynamically

---

# Current Status

Completed:

YOLO++
Training
Evaluation
Deployment
Presentation
Report

In Progress:

Hybrid YOLO++ + FasterRCNN

Next Step:

Train FasterRCNN on same dataset and begin Adaptive Feature Fusion implementation.
