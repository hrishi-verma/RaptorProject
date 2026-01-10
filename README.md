# Raptor Project

## Overview
**Raptor Project** is a high-performance distributed spatial analysis engine built using **Apache Spark** and **Scala**. Designed to handle large-scale geospatial datasets, the system seamlessly integrates raster data (elevation models) with vector data (administrative regions) to perform complex spatial computations.

The project demonstrates efficient parallel processing of geospatial queries, including QuadTree aggregation, geometry clipping, and point-in-polygon analysis.

## Features
- **Distributed Raster Loading**: Efficiently parses and loads GeoTIFF elevation data into Spark Datasets.
- **Vector Data Integration**: Ingests GeoJSON files (e.g., state/county boundaries) for spatial joins.
- **Advanced Spatial Algorithms**:
    - **Aggregate QuadTree**: Spatial indexing and aggregation for optimized querying.
    - **Clipping**: geometric clipping of raster data against vector boundaries.
    - **Point-in-Polygon**: Massively parallel verification of point containment within regional polygons.
- **Performance Benchmarking**: Built-in metrics to measure execution time for each spatial operation.

## Tech Stack
- **Language**: Scala 2.12
- **Framework**: Apache Spark 3.5.5 (Spark SQL)
- **Data Processing**: GDAL (Geospatial Data Abstraction Library)
- **Containerization**: Docker
- **Build Tool**: sbt

## Authors
- **Rishi**
- **Liam D. Healey**
- **Colton Simmons**

## Getting Started

### Prerequisites
- **Docker** (Recommended)
- OR **Java 8+**, **Scala 2.12**, and **Apache Spark** installed locally.

### Installation & Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-repo/raptor-project.git
   cd raptor-project
   ```

2. **Data Preparation:**
   Ensure your raw GeoTIFF data is in `data/Raster_geotiff`. Run the conversion script to prepare the standard TIFF format:
   ```bash
   ./geotifConvert.sh
   ```

3. **Running with Docker:**
   Build and run the container:
   ```bash
   docker build -t raptor-project .
   docker run -it raptor-project
   ```

4. **Running Locally:**
   Execute the run script, which handles compilation (sbt package) and spark-submit:
   ```bash
   ./run.sh
   ```

## Project Structure
```
raptor-project/
├── src/main/scala/       # Source code (Spark jobs, Loaders, Algorithms)
├── data/                 # Data directory (Raster, GeoJSON assets)
├── build.sbt             # Scala Build Tool configuration
├── run.sh                # Compilation and execution script
├── geotifConvert.sh      # Data preprocessing script
└── Dockerfile            # Container definition
```

## Performance
The application logs execution times for each major operation (Load, QuadTree, Clipping, Point-in-Polygon) to help analyze the efficiency of distributed spatial queries.
