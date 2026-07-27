# 🚦 ReWaIn-MTS-Cap
## From Detection to Explanation: A Vision–Language Framework for Traffic Sign Semantic Captioning

> A two-phase Deep Learning framework that combines Traffic Sign Detection and Vision-Language Models to automatically generate human-readable semantic descriptions for Mexican traffic signs.

---

## 📌 Project Information

**Project Type:** Mini Project

**Institute:** Indian Institute of Information Technology, Nagpur

**Project Supervisor:** Dr. Jagdish Chakole

### Team Members

- Rohan Kumar
- Jwalit Lal
- Nishant Singh
- Ujjwal Prakash

---

# 📖 Introduction

Traffic signs play a vital role in road safety by communicating essential information to drivers. They regulate vehicle movement, provide warnings, enforce traffic rules, and help maintain organized transportation systems.

Modern Advanced Driver Assistance Systems (ADAS) and autonomous vehicles rely heavily on computer vision techniques to detect these traffic signs. Recent developments in deep learning have significantly improved the accuracy of traffic sign detection systems.

Although these systems are highly effective at identifying the presence and location of traffic signs, they still suffer from a fundamental limitation.

Current systems answer two questions:

- What is the traffic sign?
- Where is it located?

However, they cannot answer an equally important question:

> **What does this traffic sign actually mean?**

A detected class label such as **"Speed Limit 30"** or **"No Parking"** provides very limited information to humans and intelligent driving systems.

A driver or an autonomous system benefits much more from a semantic explanation such as:

> "A speed limit sign restricting vehicles to a maximum speed of 30 km/h."

or

> "A no parking sign indicating that vehicles are prohibited from stopping or parking in this designated area."

Generating such natural-language descriptions bridges the gap between visual recognition and semantic understanding.

This project aims to build an end-to-end Vision-Language Framework capable of performing both tasks:

1. Detect traffic signs from road images.
2. Generate meaningful natural language explanations describing the detected signs.

---

# 💡 Motivation

Traffic sign detection has become a mature research problem due to significant advances in deep learning and object detection architectures.

However, the presentation identifies several important limitations in existing systems.

## Current Situation

Existing systems can:

- Detect traffic signs
- Draw bounding boxes
- Predict traffic sign classes

But they cannot:

- Explain the purpose of the traffic sign
- Describe what action the driver should take
- Produce human-readable descriptions

In other words,

**Detection ≠ Understanding**

This gap becomes increasingly important for intelligent transportation systems, explainable AI, and future driver assistance technologies.

---

## Dataset Gap

The presentation also highlights another major challenge.

Most publicly available traffic sign datasets are designed only for detection or classification.

Examples include:

- GTSRB (German Traffic Sign Dataset)
- TT100K (Chinese Traffic Sign Dataset)

These datasets do not generalize well to Mexican or Latin American traffic signs.

Furthermore, the existing **ReWaIn-MTS** dataset focuses exclusively on traffic sign detection and contains no natural-language annotations.

As a result, there was no benchmark available for traffic sign caption generation on Mexican traffic signs.

---

# 🎯 Project Objectives

The project was designed with several key objectives.

### 1. Create a Vision-Language Dataset

Extend the existing ReWaIn-MTS dataset by adding structured natural language captions for every traffic sign instance.

---

### 2. Compare Multiple Detection Architectures

Instead of selecting a single detector, evaluate multiple YOLO architectures under identical training conditions to determine the most suitable model.

The evaluated architectures include:

- YOLOv8x
- YOLO26-L
- YOLOv11x
- YOLOv9e

---

### 3. Generate Semantic Captions

Fine-tune a Vision-Language model capable of producing meaningful textual explanations for detected traffic signs.

---

### 4. Evaluate Caption Quality

Measure caption quality using multiple evaluation metrics including:

- BLEU
- CIDEr
- METEOR
- ROUGE-L

---

### 5. Compare Vision-Language Models

Perform an architectural comparison between:

- BLIP
- ViT-GPT2

to understand their effectiveness for traffic sign caption generation.

---

# 🌎 Problem Statement

Although traffic sign detection models achieve excellent localization accuracy, they still provide outputs that are difficult for humans to interpret directly.

For example,

Traditional detection systems produce outputs such as

```
Traffic Sign:
Speed Limit 60

Confidence:
98%
```

While accurate, this prediction provides only the traffic sign category.

The proposed framework instead generates semantic descriptions such as:

```
A speed limit sign indicating that
vehicles should not exceed
60 km/h on this road.
```

The objective is therefore to transform object detection into semantic understanding using Vision-Language Models.

---

# 📂 Dataset Overview

The project is built upon the existing **ReWaIn-MTS** dataset.

## Original Dataset

The original dataset contains:

| Property | Value |
|----------|--------|
| Dataset | ReWaIn-MTS |
| Published | IEEE Latin America Transactions (2025) |
| Road Images | 1,439 |
| Mexican Cities | 8 |
| Traffic Sign Instances | 2,283 |
| Traffic Sign Classes | 37 |
| Annotation Format | YOLO Bounding Boxes |

The original dataset contains only detection annotations.

No captions or semantic descriptions are available.

---

## Extended Dataset

To support Vision-Language learning, the dataset was extended into a new dataset called:

# ReWaIn-MTS-Cap

The extended dataset preserves the same road images and traffic sign annotations while introducing structured semantic captions.

Dataset statistics:

| Property | Value |
|----------|--------|
| Road Images | 1,439 |
| Traffic Sign Instances | 2,283 |
| Traffic Sign Classes | 37 |
| Captions per Sign | 5 |
| Total Caption Pairs | ~11,415 |

This extension transforms the dataset from a pure object detection benchmark into a Vision-Language dataset suitable for traffic sign caption generation.

---

# 📝 Five Caption Templates

Each traffic sign is associated with five structured captions.

Rather than writing multiple random descriptions, each caption focuses on a different semantic perspective.

The presentation defines five caption templates:

### Template 1 — Sign Identification and Meaning

Describes:

- traffic sign type
- regulatory meaning

Example:

> A no parking sign prohibiting vehicles from stopping or parking in this designated area.

---

### Template 2 — Scene Context and Mounting

Describes:

- surrounding environment
- physical placement of the sign

---

### Template 3 — Driver Instruction

Explains:

- what action the driver should perform
- expected driving behavior

---

### Template 4 — Regulatory Framing

Explains:

- traffic rule enforced
- applicable road users

---

### Template 5 — Sign Category and Visual Characteristics

Describes:

- sign appearance
- traffic sign category
- visual properties

Using multiple caption perspectives increases semantic diversity and provides richer supervision for Vision-Language training.

---

# ⚠ Dataset Challenges

The presentation identifies several engineering challenges encountered while building the dataset.

These challenges significantly influenced the final system design.

---

## Challenge 1 — Tiny Traffic Signs

Approximately **64%** of traffic signs occupy only a very small portion of the original road image.

Such tiny objects provide insufficient visual information for image captioning models.

### Solution

Instead of training on the complete road image,

each detected traffic sign is cropped individually and resized to **224 × 224 pixels**, which matches the input resolution used during caption generation.

---

## Challenge 2 — Severe Class Imbalance

The dataset contains highly imbalanced traffic sign classes.

Some classes contain hundreds of examples, while others have only a few instances.

### Solution

A class balancing strategy is applied by:

- Downsampling majority classes
- Upsampling minority classes until each class contains approximately 50 samples

This ensures more balanced learning across traffic sign categories.

---

## Challenge 3 — Overfitting on Upsampled Images

Simply duplicating rare class images increases the risk of overfitting.

### Solution

Albumentations-based data augmentation is applied only to duplicated crops, introducing visual diversity while preserving class balance.

---

## Challenge 4 — Single Reference Evaluation

Evaluating generated captions against only one reference description leads to lower evaluation scores.

### Solution

All five captions associated with each traffic sign are used as reference captions during evaluation.

---

## Challenge 5 — Caption Memorization

Repeatedly training on the same caption can cause the model to memorize sentence structures instead of learning semantics.

### Solution

During training, one of the five captions is randomly selected as the target caption in each epoch.

This encourages the model to understand traffic sign semantics rather than memorize templates.

---

## Challenge 6 — Limited Training Data

Compared to general image captioning datasets, the caption dataset is relatively small.

### Solution

A class-balanced augmented training pool is created to improve gradient diversity and enable more stable model training.

---

# 📌 Key Takeaways

The first phase of the project focuses on transforming a traditional traffic sign detection dataset into a Vision-Language dataset suitable for semantic caption generation.

The presentation introduces the motivation behind the work, extends the ReWaIn-MTS dataset with approximately **11,415 caption pairs**, and addresses several engineering challenges through crop-based preprocessing, class balancing, augmentation, and structured caption templates.

These design decisions establish the foundation for the two-stage framework, where Phase 1 performs accurate traffic sign detection and Phase 2 generates natural-language explanations.

# 🏗️ Overall System Architecture

The proposed framework follows a **two-phase pipeline** that transforms traditional object detection into semantic understanding.

Instead of directly generating captions from complete road images, the system first detects traffic signs, extracts individual sign regions, and finally generates human-readable descriptions for each detected sign.

The complete workflow is illustrated below.

```
                     Road Scene Image
                             │
                             ▼
                 Input Dataset (ReWaIn-MTS)
                             │
                             ▼
          ┌─────────────────────────────────┐
          │         PHASE 1                 │
          │    Traffic Sign Detection       │
          │                                 │
          │        YOLO Detection Model     │
          │                                 │
          │  • Bounding Boxes               │
          │  • Sign Class                   │
          │  • Confidence Score             │
          └─────────────────────────────────┘
                             │
                             ▼
                Sign Region Cropping
                             │
                             ▼
       Vision-Language Dataset Creation
                             │
                             ▼
      Cropped Traffic Sign + Semantic Captions
                             │
                             ▼
          ┌─────────────────────────────────┐
          │         PHASE 2                 │
          │    BLIP Caption Generation      │
          │                                 │
          │ Generates Natural Language      │
          │ Semantic Descriptions           │
          └─────────────────────────────────┘
                             │
                             ▼
          Final Semantic Traffic Sign Caption
```

The architecture separates the problem into two independent stages.

This modular design allows improvements in detection and caption generation without affecting the other phase.

---

# 🚀 Phase 1 – Traffic Sign Detection

The primary objective of Phase 1 is to accurately detect traffic signs from road scene images.

Instead of immediately selecting a single object detector, multiple YOLO architectures were evaluated under identical training conditions to identify the most suitable model.

This process is referred to as **Architecture Ablation**.

---

# 🎯 Why Perform Architecture Ablation?

Different YOLO models have different strengths.

Some provide:

- Higher precision
- Better recall
- Faster inference
- Improved localization
- Better small-object detection

Since traffic signs occupy only a very small portion of the road image, selecting the appropriate detector is a critical design decision.

Rather than assuming one model would perform best, the project experimentally compared multiple architectures under identical conditions.

---

# 📂 Dataset Split

The presentation uses the following dataset split.

| Dataset | Images |
|----------|--------:|
| Total Images | 1,439 |
| Training | 805 |
| Validation | 201 |
| Test | 144 |

The split is **stratified**, ensuring that traffic sign classes remain well distributed across training, validation, and testing.

All images are resized to **640 × 640** pixels before training.

---

# ⚖️ Class Balancing Strategy

One of the major challenges of the dataset is severe class imbalance.

Some traffic sign categories contain hundreds of examples, whereas others contain only a few samples.

To address this issue, the presentation proposes a **three-pronged class balancing strategy**.

---

## 1. Oversampling

Minority classes are duplicated during training.

According to the presentation:

- Classes containing fewer than 30 instances are duplicated.
- 26 out of 37 classes required oversampling.
- The rarest traffic sign classes received up to 15 duplicate copies.

This increases the representation of underrepresented traffic sign categories during training.

---

## 2. Offline and Online Data Augmentation

Instead of simply duplicating identical images, augmentation techniques are applied.

The presentation mentions:

- Albumentations
- HSV augmentation
- Blur
- Rotation
- Variations for duplicated samples

YOLO mosaic augmentation and flip transformations are also applied during training.

These augmentations introduce additional diversity while preserving the semantic meaning of each traffic sign.

---

## 3. Weighted Classification Loss

The third component of the balancing strategy focuses on the training loss.

Instead of treating every class equally,

rare traffic sign classes receive higher weights during optimization.

The presentation mentions:

- Inverse class-frequency weighting
- Maximum weight capped at 10
- Rare class errors receive greater penalty

This encourages the detector to learn minority classes more effectively.

---

# ⚙️ Training Configuration

All four YOLO architectures were trained under identical conditions.

This ensures that performance differences arise from architectural improvements rather than different hyperparameters.

Training configuration:

| Parameter | Value |
|-----------|-------|
| Optimizer | AdamW |
| Learning Rate | 0.001 |
| Batch Size | 16 |
| Epochs | 40 |
| Early Stopping | Patience = 10 |
| GPU | NVIDIA T4 |

Using identical training settings allows a fair comparison across all architectures.

---

# 🤖 YOLO Architectures Evaluated

Four object detection architectures were compared.

---

## YOLOv8x

YOLOv8x serves as one of the strongest modern object detectors.

The presentation reports:

- C2f Backbone
- Anchor-free detector
- mAP ≈ 96.89%
- Recall = 0.9398

---

## YOLO26-L

YOLO26-L introduces improvements in feature aggregation.

Reported results:

- BiC Neck
- Reparameterized architecture
- mAP ≈ 94.78%
- Recall ≈ 0.86

---

## YOLOv11x

YOLOv11x further improves detection performance.

Reported results:

- C3k2 architecture
- C2PSA attention
- mAP ≈ 96.72%
- Recall ≈ 0.87

---

## YOLOv9e

YOLOv9e incorporates:

- PGI (Programmable Gradient Information)
- GELAN Backbone

Reported results:

- mAP ≈ 96.83%
- Recall ≈ 0.89

---

# 📊 Detection Results

The presentation compares previous work with the proposed implementation.

| Source | Model | mAP@50 |
|---------|--------|--------:|
| Previous Work | YOLOv5 | 84.82% |
| Previous Work | YOLOv8 | 96.21% |
| Previous Work | YOLOv11 | 92.64% |
| Previous Work | RT-DETR | 93.12% |
| Ours | YOLO26-L | 94.78% |
| Ours | YOLOv11x | 96.72% |
| Ours | YOLOv9e | 96.83% |
| **Ours** | **YOLOv8x** | **96.89%** |

The presentation shows that all four trained models achieve detection accuracy greater than **94.78% mAP@50**, demonstrating the effectiveness of the proposed class balancing strategy.

---

# 📈 Performance Analysis

The comparison demonstrates several important observations.

### YOLOv5

Acts as the weakest baseline with approximately **84.82% mAP@50**.

---

### YOLO26-L

Improves substantially over YOLOv5 while maintaining strong localization performance.

---

### YOLOv11x

Produces excellent detection accuracy and significantly improves over previous YOLOv11 results.

---

### YOLOv9e

Delivers competitive detection performance through the combination of PGI and GELAN architecture.

---

### YOLOv8x

Achieves the highest overall detection accuracy among all evaluated models in the presentation.

---

# 🎯 Why YOLOv8x Was Selected

Instead of selecting the model with only the highest precision,

the presentation follows a **Recall-First Principle**.

The motivation is straightforward.

In traffic sign captioning,

**missing a traffic sign is far more harmful than detecting an additional false positive.**

If a sign is not detected,

the caption generation stage never receives that traffic sign.

Consequently,

no caption can be generated.

The presentation summarizes this idea as:

> **Missed Detection > False Positive**

A false positive may generate an unnecessary caption.

A missed detection completely removes the traffic sign from the Vision-Language pipeline.

---

# 📉 Precision vs Recall

The presentation compares precision and recall for the evaluated detectors.

YOLOv8x achieves the highest recall among all evaluated architectures.

Reported recall:

**0.9398**

This means YOLOv8x successfully detects more traffic signs than the other evaluated models.

Higher recall directly improves the caption generation stage because more detected traffic signs become available for semantic description.

---

# 🔍 Why Recall Matters

The second phase of the framework depends entirely on the output produced by Phase 1.

If detection quality decreases,

caption quality also decreases.

The presentation summarizes this relationship as:

```
Detection Quality
        ↓
Crop Quality
        ↓
Caption Quality
```

Accurate localization produces higher-quality traffic sign crops.

Higher-quality crops provide better visual information for the BLIP Vision Transformer.

Consequently,

better crops lead to better semantic captions.

---

# ✅ Phase 1 Summary

The first phase focuses on building a robust traffic sign detector through systematic architecture comparison.

Rather than relying on a single detector, four YOLO architectures are trained under identical conditions using class balancing, oversampling, augmentation, and weighted loss.

The experiments demonstrate that all evaluated models achieve strong detection performance, while YOLOv8x is selected based on its high detection accuracy and superior recall.

The output of this stage consists of:

- Traffic sign bounding boxes
- Traffic sign class labels
- Confidence scores
- Cropped traffic sign regions

These cropped traffic sign images serve as the direct input to the Vision-Language caption generation stage described in Phase 2.

# 🤖 Phase 2 – Vision-Language Caption Generation

After successfully detecting traffic signs in Phase 1, the framework enters the second stage, where the detected traffic sign crops are converted into meaningful natural-language descriptions.

Unlike conventional image captioning tasks that describe an entire scene, this project focuses on **semantic caption generation for individual traffic signs**.

Each detected sign is treated as an independent visual object and is paired with multiple structured captions describing its meaning, driver instructions, regulatory purpose, and visual characteristics.

The objective of this phase is not merely to recognize a traffic sign, but to **generate a human-readable explanation** that conveys its semantic meaning.

---

# Why a Two-Phase Framework?

The presentation separates the complete pipeline into two independent stages.

```
Road Image
      │
      ▼
Traffic Sign Detection
      │
      ▼
Traffic Sign Crop
      │
      ▼
Vision-Language Model
      │
      ▼
Semantic Caption
```

This modular design provides several advantages.

Instead of training one large end-to-end model, each phase can be optimized independently.

Advantages include:

- Better debugging
- Improved modularity
- Independent model improvements
- Easier experimentation
- Higher caption quality through better detection

---

# Why BLIP?

Several Vision-Language Models exist for image caption generation.

However, according to the presentation, BLIP was selected because it is specifically designed for learning strong visual-language relationships.

BLIP stands for

**Bootstrapping Language Image Pre-training**

Its architecture enables the model to understand both:

- visual content
- textual semantics

simultaneously.

Rather than generating generic captions, BLIP learns to associate specific image regions with meaningful language.

This makes it well suited for traffic sign caption generation.

---

# BLIP Architecture

The presentation describes BLIP as a Vision-Language architecture consisting of three major components.

```
Traffic Sign Crop
        │
        ▼
 Vision Transformer Encoder
        │
Extract Visual Features
        │
        ▼
Cross-Attention Module
        │
Align Image + Text Features
        │
        ▼
Language Decoder
        │
Generate Caption
```

Each component performs a different task.

---

## Vision Encoder

The Vision Transformer (ViT) receives the cropped traffic sign image.

Its responsibilities include:

- extracting visual features
- learning traffic sign appearance
- identifying geometric patterns
- encoding semantic information

The encoder transforms the image into feature embeddings that can be interpreted by the language generation module.

---

## Cross-Attention

One of BLIP's strongest characteristics is the use of cross-attention.

Instead of treating image and language independently,

cross-attention aligns visual features directly with textual representations.

This allows the model to generate descriptions that are strongly grounded in the detected traffic sign.

---

## Language Decoder

The decoder receives aligned visual-language embeddings.

It then generates the final caption word by word.

The output is a complete natural-language description explaining the detected traffic sign.

---

# Crop-Based Training Strategy

One of the most important engineering decisions highlighted in the presentation is **crop-based training**.

Instead of providing the entire road scene,

only the detected traffic sign crop is used during caption generation.

```
Road Image
     │
     ▼
Traffic Sign Detection
     │
     ▼
Crop Traffic Sign
     │
     ▼
Resize (224 × 224)
     │
     ▼
BLIP
```

---

# Why Crop-Based Training?

The presentation explains that approximately **64% of traffic signs occupy only a tiny region of the complete image**.

If the complete road scene is passed directly into BLIP,

the Vision Transformer struggles to focus on such small objects.

As a result,

caption quality decreases significantly.

By cropping each detected sign,

the model receives a high-resolution view of the traffic sign.

This allows the Vision Transformer to learn:

- sign shape
- color
- symbols
- semantic meaning

more effectively.

---

# Training Dataset

The caption generation stage uses the newly created **ReWaIn-MTS-Cap** dataset.

Dataset statistics include:

| Property | Value |
|----------|--------|
| Images | 1,439 |
| Sign Instances | 2,283 |
| Classes | 37 |
| Captions per Sign | 5 |
| Total Caption Pairs | ~11,415 |

Each traffic sign crop is paired with five structured semantic captions.

These captions become the training targets for BLIP.

---

# Caption Selection Strategy

Rather than always using the same caption,

the presentation adopts **caption shuffling**.

During each training epoch,

one of the five available captions is randomly selected.

Advantages include:

- prevents memorization
- increases language diversity
- improves semantic understanding
- exposes the model to multiple sentence structures

This encourages BLIP to learn the underlying meaning of traffic signs rather than simply memorizing template sentences.

---

# Class Balancing

The presentation emphasizes that class balancing is equally important during caption generation.

Majority classes are downsampled.

Minority classes are augmented until approximately **50 instances per class** are available.

This ensures that BLIP receives balanced supervision across all traffic sign categories.

---

# Training Configuration

The presentation specifies the following configuration for BLIP fine-tuning.

| Parameter | Value |
|-----------|-------|
| Optimizer | AdamW |
| Learning Rate | 5 × 10⁻⁵ |
| Batch Size | 16 |
| Mixed Precision | FP16 |
| Early Stopping | Enabled |
| Beam Search | 5 |

Mixed precision training reduces memory consumption while improving training efficiency.

Beam Search is used during inference to produce more accurate captions.

---

# Caption Evaluation

Generated captions are evaluated using multiple standard image-captioning metrics.

---

## BLEU

BLEU measures n-gram overlap between generated captions and reference captions.

Higher BLEU scores indicate better lexical similarity.

The presentation reports:

- BLEU-1
- BLEU-2
- BLEU-3
- BLEU-4

---

## METEOR

METEOR measures semantic similarity while considering synonym matching.

Compared to BLEU,

it provides a better indication of linguistic quality.

---

## ROUGE-L

ROUGE-L measures the longest common subsequence between generated and reference captions.

It evaluates structural similarity between sentences.

---

## CIDEr

CIDEr is highlighted in the presentation as the **primary evaluation metric**.

Unlike BLEU,

CIDEr rewards captions that contain domain-specific vocabulary.

This makes it particularly suitable for traffic sign caption generation.

---

# BLIP Captioning Results

The presentation reports the following evaluation results.

| Metric | Score |
|---------|-------|
| BLEU-1 | 0.875 |
| BLEU-2 | 0.821 |
| BLEU-3 | 0.784 |
| BLEU-4 | **0.755** |
| METEOR | **0.472** |
| ROUGE-L | **0.737** |
| CIDEr | **1.799** |

These results demonstrate that BLIP successfully learns both the visual appearance and semantic meaning of traffic signs.

The high CIDEr score indicates strong domain-specific caption generation.

---

# BLIP vs ViT-GPT2

To further evaluate the effectiveness of the proposed approach,

the presentation compares BLIP with another popular Vision-Language model:

**ViT-GPT2**

Both models are trained using the same traffic sign dataset.

---

## ViT-GPT2 Results

| Metric | Score |
|---------|-------|
| BLEU-4 | 0.496 |
| CIDEr | 0.233 |

---

## BLIP Results

| Metric | Score |
|---------|-------|
| BLEU-4 | **0.755** |
| CIDEr | **1.799** |

---

# Performance Comparison

The presentation highlights a key observation.

Although ViT-GPT2 produces reasonably fluent captions,

its CIDEr score remains significantly lower than BLIP.

BLIP achieves approximately

**7.7× higher CIDEr**

than ViT-GPT2.

This demonstrates that BLIP captures traffic sign semantics much more effectively.

---

# Why Does BLIP Perform Better?

According to the presentation,

the primary architectural difference lies in **cross-attention**.

BLIP directly aligns:

- image regions
- language representations

before generating captions.

ViT-GPT2, in contrast, relies on an autoregressive decoder that produces fluent language but struggles to generate specialized traffic sign terminology.

As a result,

BLIP generates captions that are both semantically richer and more relevant to the traffic sign domain.

---

# Comparison with Previous Image Captioning Methods

The presentation compares the proposed BLIP model against several previous captioning approaches.

The compared methods include:

- Show & Tell
- Show, Attend & Tell
- Bottom-Up Attention
- Oscar
- BLIP Base (COCO)
- Rule-Based Templates
- ViT-GPT2

Among all compared methods,

the proposed BLIP model achieves the strongest overall performance.

Key observations reported in the presentation include:

- Highest BLEU-4 score
- Highest CIDEr score
- Strong ROUGE-L performance
- Significant improvement over the BLIP COCO baseline
- Superior domain adaptation for traffic sign caption generation

These results demonstrate that fine-tuning BLIP on the ReWaIn-MTS-Cap dataset substantially improves semantic caption quality.

---

# Key Findings from Phase 2

The second phase transforms traffic sign crops into human-readable descriptions using BLIP.

Several important design decisions contribute to its success:

- Crop-based training improves visual focus.
- Structured captions improve semantic diversity.
- Caption shuffling prevents memorization.
- Class balancing ensures fair learning across all traffic sign categories.
- BLIP significantly outperforms ViT-GPT2 in domain-specific caption generation.

The evaluation results indicate that the proposed Vision-Language pipeline successfully bridges the gap between object detection and semantic understanding, enabling traffic signs to be described in natural language rather than represented solely as class labels.

# 📈 Per-Class Caption Performance

In addition to reporting overall captioning metrics, the presentation analyzes the caption generation performance across different traffic sign classes using the **CIDEr** metric.

This analysis provides insight into how well the model performs on individual categories rather than only evaluating overall dataset performance.

## Why Per-Class Analysis?

Overall evaluation metrics summarize the model's average performance, but they do not reveal whether certain traffic sign categories are easier or more difficult to describe.

Per-class evaluation helps identify:

- Traffic sign categories with consistently accurate captions.
- Classes where the model struggles to generate meaningful descriptions.
- The effect of class diversity and visual complexity on caption quality.
- Opportunities for improving future versions of the model.

The presentation illustrates these differences using a per-class CIDEr comparison chart.

Although caption quality varies across categories, the proposed BLIP model demonstrates strong performance across the majority of traffic sign classes, indicating that the model generalizes well beyond a small subset of signs.

---

# 📝 Sample Caption Generation

The presentation includes qualitative examples demonstrating the captions generated by the trained BLIP model.

The generated captions are designed to provide semantic explanations instead of simply predicting a traffic sign class.

Instead of producing outputs such as:

```
Stop Sign
```

the model generates descriptive captions explaining the meaning of the sign.

Examples shown in the presentation illustrate that the model can describe:

- the type of traffic sign,
- the regulation it represents,
- the action expected from the driver,
- and the overall semantic meaning of the sign.

These examples demonstrate that the framework successfully moves from **object detection** to **semantic understanding**, allowing traffic signs to be interpreted in natural language.

---

# 📊 Overall Project Pipeline

The complete workflow implemented in this project can be summarized as follows.

```
                     Road Image
                          │
                          ▼
              Traffic Sign Detection
                    (YOLOv8x)
                          │
                          ▼
          Bounding Boxes + Class Labels
                          │
                          ▼
             Crop Traffic Sign Regions
                          │
                          ▼
              Resize to 224 × 224 Pixels
                          │
                          ▼
          BLIP Vision-Language Model
                          │
                          ▼
         Natural Language Caption Generation
                          │
                          ▼
      Human-Readable Semantic Explanation
```

The framework separates detection and language generation into two modular stages, allowing each component to be optimized independently.

---

# 🏆 Project Contributions

The presentation highlights several important contributions of this work.

## 1. Extension of the ReWaIn-MTS Dataset

The existing ReWaIn-MTS traffic sign detection dataset is extended with structured semantic captions.

This new dataset, **ReWaIn-MTS-Cap**, enables research on traffic sign caption generation for Mexican traffic signs.

---

## 2. Vision-Language Framework

A complete two-stage Vision-Language pipeline is proposed.

The framework combines:

- object detection,
- image preprocessing,
- and natural-language caption generation

into a unified workflow capable of producing semantic explanations for detected traffic signs.

---

## 3. YOLO Architecture Comparison

Multiple YOLO architectures are evaluated under identical training conditions.

The comparison enables the selection of the most appropriate detector for the caption generation pipeline.

---

## 4. BLIP Fine-Tuning

The BLIP Vision-Language model is fine-tuned using the newly created caption dataset.

The model learns to generate meaningful descriptions for detected traffic signs rather than simple class labels.

---

## 5. Comparison with ViT-GPT2

The project evaluates two different Vision-Language models on the same dataset.

The experimental results demonstrate that BLIP provides substantially better caption generation performance.

---

# ⚠️ Limitations

The presentation identifies several limitations of the current implementation.

## 1. No OCR Integration

The framework focuses on visual recognition and semantic caption generation.

Traffic signs containing textual information are not explicitly processed using Optical Character Recognition (OCR).

As a result, text appearing on traffic signs is not interpreted separately.

---

## 2. Crop-Based Processing

Caption generation operates only on cropped traffic sign regions.

The surrounding road scene is not incorporated during caption generation.

Consequently, contextual information that might influence interpretation is not considered.

---

## 3. English-Only Captions

All generated captions are produced in English.

Support for multilingual caption generation is not included in the current implementation.

---

# 🚀 Future Work

The presentation proposes several directions for extending the current framework.

## OCR Integration

Future versions can incorporate Optical Character Recognition to interpret textual information present on traffic signs.

This would enable richer and more informative semantic descriptions.

---

## Context-Aware Captioning

Instead of relying only on cropped traffic sign images, future systems may also consider the surrounding road scene.

Combining local sign information with global scene context could improve caption quality.

---

## Detection-Guided Captioning

The presentation suggests tighter integration between the detection and caption generation stages.

Rather than treating them as separate modules, future work may explore detection-guided Vision-Language architectures.

---

## Spanish Caption Generation

Since the dataset represents Mexican traffic signs, generating captions in Spanish would improve accessibility and better align with the target domain.

---

## Larger Vision-Language Models

The presentation also proposes investigating larger and more advanced Vision-Language models to further improve caption quality and semantic understanding.

---

# 🎯 Conclusion

This project presents a two-phase Vision-Language framework for semantic traffic sign caption generation.

Instead of limiting the system to object detection, the framework extends traffic sign understanding by producing meaningful natural-language descriptions.

The project begins by extending the existing ReWaIn-MTS dataset with structured captions, creating the ReWaIn-MTS-Cap dataset.

Multiple YOLO architectures are evaluated for traffic sign detection, with YOLOv8x selected based on its strong detection performance and high recall.

The detected traffic sign crops are then processed using a fine-tuned BLIP Vision-Language model to generate semantic captions.

Experimental evaluation demonstrates strong caption generation performance across multiple metrics, and comparison with ViT-GPT2 shows that BLIP provides significantly better semantic descriptions for this task.

Overall, the project successfully bridges the gap between traffic sign detection and semantic understanding by integrating computer vision and natural language generation into a unified pipeline.

---

# 🔑 Key Takeaways

- Developed a two-stage Vision-Language framework for traffic sign semantic captioning.
- Extended the ReWaIn-MTS dataset by introducing multiple semantic captions for each traffic sign.
- Evaluated four YOLO architectures under identical training settings.
- Selected YOLOv8x based on its strong detection performance and high recall.
- Fine-tuned the BLIP Vision-Language model for semantic caption generation.
- Achieved strong captioning performance using BLEU, METEOR, ROUGE-L, and CIDEr metrics.
- Demonstrated that BLIP outperforms ViT-GPT2 for traffic sign caption generation.
- Identified future research opportunities including OCR integration, multilingual captioning, context-aware caption generation, and larger Vision-Language models.

---

# 📚 References

This project is based on the work presented in:

> **"From Detection to Explanation: A Vision–Language Framework for Traffic Sign Semantic Captioning"**

Additional references included in the presentation are listed on the final reference slide.

---

# 🙏 Acknowledgements

We express our sincere gratitude to our project supervisor for their guidance and support throughout the development of this project.

We also acknowledge all team members whose collaborative efforts contributed to the successful completion of this work.

---

## ⭐ If you found this project useful, consider giving it a star on GitHub!
