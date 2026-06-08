# Flood-Segmentation-Using-DeepLearning-and-Sentinel-1-Images
Flood Segmentation Using Deep Learning and Sentinel-1 SAR Imagery
Overview

Floods are among the most destructive natural disasters, making rapid and accurate flood mapping essential for disaster response and risk assessment. Traditional optical satellite imagery is often ineffective during flood events due to cloud cover and poor visibility.

This project presents a deep learning-based flood segmentation framework using Sentinel-1 Synthetic Aperture Radar (SAR) imagery from the Sen1Floods11 dataset. Unlike optical sensors, SAR imagery operates under all weather and lighting conditions, making it highly suitable for flood monitoring.

The project explores progressively advanced architectures, culminating in a novel Cross Polarization Feature Gating Network (CPFG-Net) that explicitly models interactions between VV and VH polarization channels while incorporating physics-based SAR features.

Features
Flood segmentation using Sentinel-1 SAR imagery
Physics-informed feature engineering
ResNet34 U-Net baseline implementation
Custom CPFG-Net architecture
Cross-polarization feature gating
Multi-scale feature fusion
Class imbalance handling
MixUp augmentation
DropBlock regularization
Comprehensive evaluation using segmentation metrics
Dataset
Sen1Floods11

The project uses the Sen1Floods11 dataset containing:

Sentinel-1 SAR images
Flood segmentation masks
Multiple geographic flood events

Dataset split:

Split	Images
Train	285
Validation	72
Test	89

Each image contains:

VV Polarization
VH Polarization

Dataset paper:

Bonafilia et al., Sen1Floods11: A Georeferenced Dataset to Train and Test Deep Learning Flood Algorithms for Sentinel-1 (CVPRW 2020)

Data Preprocessing
SAR Normalization

Backscatter values are clipped and normalized.

VV range:

[-25, 5]

VH range:

[-32, -5]

Normalization:

X
norm
	​

=
X
max
	​

−X
min
	​

X−X
min
	​

	​

Physics-Based SAR Features

To improve flood discrimination, additional SAR descriptors are generated:

Polarization Ratio
PR=
VV
VH
	​

Polarization Difference
Difference=VV−VH
Joint Scattering Feature
Joint=VV×VH
Radar Vegetation Index (RVI)
RVI=
VV+VH
4VH
	​

NDSI
NDSI=
VV+VH
VV−VH
	​


Final feature representation:

VV
VH
Polarization Ratio
Polarization Difference
Joint Scattering
RVI
NDSI
Model Architectures
1. ResNet34 U-Net Baseline

Input:

512 × 512 × 2

Channels:

VV
VH

Features:

ResNet34 encoder
U-Net decoder
Skip connections
Dropout regularization
2. Physics-Based ResNet34 U-Net

Input:

512 × 512 × 5

Additional channels:

Polarization Ratio
RVI
NDSI

Uses transfer learning from baseline model.

3. CPFG-Net

Proposed architecture containing:

Dual ResNet34 encoders
Cross Polarization Feature Gating
Multi-scale feature fusion
Physics feature injection
U-Net decoder
Cross Polarization Feature Gating

Instead of directly merging VV and VH channels, CPFG-Net learns polarization-specific interactions.

Dual Encoders
F
VV
	​

=Encoder
VV
	​

(X
VV
	​

)
F
VH
	​

=Encoder
VH
	​

(X
VH
	​

)
Adaptive Gating
A
VV
	​

=σ(Conv(F
VH
	​

))
A
VH
	​

=σ(Conv(F
VV
	​

))
Gated Features
G
VV
	​

=F
VV
	​

⊙A
VV
	​

G
VH
	​

=F
VH
	​

⊙A
VH
	​


This allows the network to learn meaningful cross-polarization relationships.

Handling Class Imbalance

Flood datasets are highly imbalanced.

Strategies used:

Weighted sampling
Positive class weighting
Positive Class Weight = 11.67
BCE + Dice Loss
L=0.5×BCE+0.5×Dice
Data Augmentation

Applied augmentations:

Horizontal Flip
Vertical Flip
Rotation
MixUp
Random Brightness Shift
SAR Speckle Noise Simulation
Training Configuration
Optimizer
AdamW
Scheduler
Cosine Annealing Learning Rate
Regularization
Dropout2D
DropBlock
MixUp
Evaluation Metrics

Models are evaluated using:

IoU
Dice Score
Precision
Recall
F1 Score
Accuracy
Results
Model	IoU	Dice	Recall
ResNet34 U-Net	0.3436	0.4153	0.5072
Physics U-Net	0.3924	0.4667	0.5943
CPFG-Net v2	0.3720	0.4489	0.5267
CPFG-Net v2 + DropBlock	0.3873	0.4671	0.6038
Best Model

CPFG-Net v2 + DropBlock

Performance:

IoU      : 0.3873
Dice     : 0.4671
Recall   : 0.6038
Accuracy : 0.8878
Key Contributions
Developed a novel Cross Polarization Feature Gating Network (CPFG-Net)
Incorporated SAR physics-based descriptors into deep learning models
Improved flood segmentation through polarization-aware feature learning
Demonstrated the effectiveness of dual-stream SAR processing
Achieved improved IoU and Dice scores over baseline U-Net architectures
Technologies Used
Python
PyTorch
NumPy
OpenCV
Albumentations
segmentation_models_pytorch
Scikit-learn
Matplotlib
Future Work
Transformer-based SAR segmentation
Multi-temporal flood mapping
Domain adaptation across geographic regions
Real-time flood monitoring systems
Lightweight edge deployment models
