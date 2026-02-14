# Raptor Project: Geospatial Data Processing for Machine Learning

## Overview

The **Raptor Project** is a high-performance big data processing pipeline built with **Scala** and **Apache Spark**. It is designed to ingest, process, and analyze massive geospatial datasets ("Big Input") to generate features for downstream Machine Learning models.

In the context of Machine Learning, data preparation and feature engineering are often the most computationally expensive steps. This project solves the challenge of integrating unstructured **Raster data** (satellite imagery, elevation models) with structured **Vector data** (administrative boundaries) at scale.

## The "Big Input": Machine Learning Context

This project specifically targets the **"Big Input"** problem in training ML models for geospatial analytics. 
Training accurate models (e.g., for crop yield prediction, flood risk assessment, or land use classification) requires processing terabytes of raw data.

### 1. Data Volume & Variety
The system handles two distinct types of massive inputs:
*   **Raster Data (Big Data)**: High-resolution `.tif` files (e.g., USGS DEMs) representing continuous fields like elevation or temperature. Each file can be hundreds of megabytes, covering vast geographic areas.
*   **Vector Data (Structured Data)**: Complex `.zip` Shapefiles representing political or physical boundaries (e.g., US Counties).

### 2. Feature Engineering Pipeline
The core value of this project for ML is **Feature Extraction**. It transforms raw pixels into meaningful statistical features (e.g., "Average Elevation per County") that can be fed directly into:
*   **Regression Models**: Predicting climate trends based on terrain attributes.
*   **Classification Models**: Categorizing regions based on topographic profiles.

## Key Algorithms & Computational Methods

To handle the "Big Input" efficiently, the project implements several distributed spatial algorithms:

### 1. Aggregate QuadTree
*   **Purpose**: Rapid approximation and aggregation.
*   **Mechanism**: Constructs a **QuadTree** spatial index to hierarchically group raster pixels. This allows for fast aggregation of data (e.g., sum, count) over large areas without iterating through every single pixel for every query.
*   **ML Use Case**: Coarse-grained feature generation for initial model training.

### 2. Point in Polygon (Ray Casting)
*   **Purpose**: Precise spatial join.
*   **Mechanism**: Uses the **Ray Casting algorithm** to determine exactly which pixels fall within a specific irregular polygon (Result of `PointInPolygon.scala`).
*   **ML Use Case**: Generating ground-truth labels for pixel-level classification tasks.

### 3. Geometric Clipping
*   **Purpose**: Data reduction and focusing.
*   **Mechanism**: Filters and clips raster datasets to the bounding box of vector regions before performing expensive geometric checks (`Clipping.scala`).
*   **ML Use Case**: Creating training chips/patches for Convolutional Neural Networks (CNNs).

## Project Structure

```
RaptorProject/
├── build.sbt               # Scala build configuration & dependencies
├── run.sh                  # Execution script
├── src/main/scala/         # Source Code
│   ├── Main.scala          # Entry point
│   ├── AggregateQuadTree.scala # spatial aggregation logic
│   ├── Clipping.scala      # Geometric clipping logic
│   ├── PointInPolygon.scala # Ray casting algorithm
│   ├── RasterLoader.scala  # Utilities for loading .tif data
│   └── VectorLoader.scala  # Utilities for loading .shp data
└── data/                   # The "Big Input" directory
    ├── Raster/             # Large TIF files
    └── Vector/             # Shapefiles (US States/Counties)
```

## Prerequisites

*   **Java 8+**
*   **Scala 2.12.18**
*   **Apache Spark 3.5.5**
*   **sbt** (Scala Build Tool)

## Installation & Usage

1.  **Clone the repository:**
    ```bash
    git clone <repository-url>
    cd RaptorProject
    ```

2.  **Prepare Data:**
    Ensure your "Big Input" data is placed in the `data/` directory:
    *   Place `.tif` files in `data/Raster/`
    *   Place `.zip` shapefiles in `data/Vector/`

3.  **Run the Pipeline:**
    Use the provided shell script to compile and submit the Spark job.
    ```bash
    ./run.sh
    ```
    *   This script runs `sbt package` to build the JAR.
    *   It then submits the job to Spark using `spark-submit`.
    *   Output logs are directed to `output.log`.

## Performance

The system utilizes Spark's distributed computing capabilities (`RDD`s and `Datasets`) to parallelize operations across multiple cores. 
*   **Optimization**: Includes spatial indexing (QuadTree) and bounding-box filtering to minimize expensive geometric tests.
*   **Scalability**: Designed to scale horizontally with the size of the "Big Input".
