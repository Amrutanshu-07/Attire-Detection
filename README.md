# Attire Detection

**Turning fashion videos into searchable outfits, products, and style
insights.**

Attire Detection is a fashion-video understanding and discovery project.
It processes selected frames from a fashion video, detects visible
clothing items, retrieves visually similar products from a catalogue,
and assigns a fashion-vibe label using text metadata.

> **Project:** Group 23\
> **Core technologies:** YOLOv8n, CLIP (ViT-B/32), FAISS, keyword-based
> NLP

## Table of Contents

-   [Overview](#overview)
-   [Key Features](#key-features)
-   [How It Works](#how-it-works)
-   [Technology Stack](#technology-stack)
-   [Dataset and Project Assets](#dataset-and-project-assets)
-   [Configuration](#configuration)
-   [Output](#output)
-   [Reported Results](#reported-results)
-   [Limitations](#limitations)
-   [Future Improvements](#future-improvements)
-   [Team](#team)

## Overview

A fashion video may show an outfit only briefly, with multiple garments
and accessories appearing together. Finding each item and searching for
similar catalogue products manually can be time-consuming. Describing
the overall aesthetic can also be subjective.

This project brings three stages together:

1.  **Attire detection** --- identify clothing and accessories in
    sampled video frames.
2.  **Product discovery** --- retrieve visually similar catalogue items
    using image embeddings.
3.  **Fashion-vibe classification** --- assign an aesthetic category
    using captions, hashtags, and descriptions.

The project is designed as a fashion discovery tool, not as a dress-code
or uniform-compliance checker.

## Key Features

-   **Video frame sampling:** Selects five keyframes from each input
    video.
-   **Attire detection:** Uses Ultralytics YOLOv8n to locate attire
    items and return class names, bounding-box coordinates, and
    confidence scores.
-   **Visual product retrieval:** Encodes detected clothing crops with
    CLIP ViT-B/32 and searches catalogue embeddings using FAISS.
-   **Fashion-vibe classification:** Applies keyword-based rules to text
    metadata to assign a supported vibe label.
-   **Structured results:** Combines attire labels, product matches,
    similarity scores, and the predicted fashion vibe.

## How It Works

The processing pipeline follows these steps:

``` text
Fashion Video
     |
     v
Sample 5 Keyframes
     |
     v
YOLOv8n Attire Detection
     |
     v
Detected Clothing Regions
     |
     +------------------------------+
     |                              |
     v                              v
CLIP ViT-B/32                  Captions, Hashtags,
Visual Embeddings              and Descriptions
     |                              |
     v                              v
FAISS Similarity Search        Keyword-based NLP
     |                              |
     v                              v
Similar Catalogue Items        Fashion-vibe Label
     |                              |
     +---------------+--------------+
                     |
                     v
       Attire + Product Matches + Vibe
```

### Stage 1: Attire Detection

YOLOv8n processes each selected frame and predicts attire classes. For
every detected item, the output includes:

-   Class name
-   Bounding-box coordinates
-   Confidence score

The configured detection confidence threshold is **0.25**. Detected
regions are passed to the product-retrieval stage so matching can focus
on individual clothing items rather than the complete video frame.

### Stage 2: Similar Product Retrieval

Each detected clothing crop is encoded using **CLIP ViT-B/32** to create
a visual embedding. FAISS performs nearest-neighbour vector search
against catalogue embeddings and returns ranked candidate products.

-   Catalogue records available: **11,000+**
-   Optimised search subset: **1,500+ items**
-   Configured similarity threshold: **0.35**

A similarity match indicates that catalogue items are visually related.
It does **not** guarantee that the result is the identical brand, model,
or product.

### Stage 3: Fashion-Vibe Classification

The vibe module scans available captions, hashtags, and product/video
descriptions for category-related keywords. Keyword rules map the text
to one of the supported labels.

Supported labels:

-   Clean Girl
-   Coquette
-   Cottagecore
-   Streetcore
-   Y2K
-   Boho
-   Party Glam

This is a text-based, keyword-rule classification module---not a
separate deep-learning image classifier. Its output therefore depends on
the quality and availability of the text metadata.

## Technology Stack

  -----------------------------------------------------------------------
  Component               Technology              Purpose
  ----------------------- ----------------------- -----------------------
  Attire detection        Ultralytics YOLOv8n     Detect clothing and
                                                  accessories in sampled
                                                  frames

  Image representation    CLIP ViT-B/32           Generate visual
                                                  embeddings for detected
                                                  crops

  Similarity search       FAISS                   Retrieve nearest
                                                  catalogue vectors

  Vibe classification     Keyword-based NLP       Assign a fashion-vibe
                                                  label from text
                                                  metadata

  Video processing        Keyframe sampling       Select five
                                                  representative frames
                                                  per video
  -----------------------------------------------------------------------

## Dataset and Project Assets

The project presentation describes the following inputs and assets:

  -----------------------------------------------------------------------
  Asset                               Description
  ----------------------------------- -----------------------------------
  `data/videos`                       Folder containing fashion video
                                      clips

  `images.csv`                        Product catalogue metadata
                                      containing 11K+ catalogue items

  Optimised catalogue subset          1,500+ items used for faster
                                      similarity search

  `vibeslist.json`                    Metadata supporting the
                                      fashion-vibe categories
  -----------------------------------------------------------------------

Five keyframes are sampled from each video and used as the visual input
for attire detection.

*Note: The exact dataset download links, complete repository directory
structure, and data-licensing details are not specified in the project
presentation.*

## Configuration

The values reported for the project pipeline are:

  Setting                                        Value
  ----------------------------------- ----------------
  YOLO detection confidence                     `0.25`
  Product similarity threshold                  `0.35`
  Frames sampled per video                         `5`
  Optimised catalogue search subset     `1,500+` items

## Output

For an input fashion video, the workflow produces:

-   Detected attire labels and their bounding boxes on sampled frames
-   Similar catalogue product candidates with similarity scores
-   A predicted fashion-vibe category
-   A structured result combining the detection, retrieval, and
    vibe-classification stages

## Reported Results

The project presentation reports the following observed results:

  Metric                                               Reported result
  ------------------------------ -------------------------------------
  Processing time                  Approximately 8.3 seconds per video
  Attire detections                                             20--24
  High-quality product matches                                      10
  Reported vibe accuracy                                          80%+
  Successful test videos                                    6 out of 6

These are the results reported for the project's tests and should be
interpreted in the context of the tested videos and evaluation setup.

## Limitations

-   Sampling only five frames may miss clothing details that appear
    briefly.
-   Occlusion and lighting conditions can affect attire detection.
-   Product retrieval returns visually related candidates, not
    guaranteed exact product identities.
-   Vibe classification depends on the quality of captions, hashtags,
    and descriptions.
-   The reported evaluation covers six successful test videos; broader
    validation is still needed.

## Future Improvements

Potential next steps identified for the project include:

-   Expand the number and variety of video tests.
-   Improve product-retrieval ranking.
-   Validate fashion-vibe labels against human ratings.

## Team

**Group 23**

  Name                  Student ID
  ------------------- ------------
  Awi Kumari               2330371
  Amrutanshu Nanda         2330362
  Aditya Kumar             2330132
  Abhishek Karmakar        2330281

------------------------------------------------------------------------

**Attire Detection --- See it. Find it. Style it.**
