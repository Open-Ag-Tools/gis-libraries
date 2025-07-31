# Vector GIS File Formats: Pros and Cons

## 1. Shapefile (.shp)
**Pros:**
- Very widely supported across almost all GIS software (industry standard for decades)
- Simple format, easy to handle for small to medium datasets
- Human-readable attribute table (DBF)

**Cons:**
- Multiple files required (.shp, .shx, .dbf, etc.)
- Limited to 2 GB per file
- Attribute name limit (10 characters) and no native support for UTF-8
- No topology or advanced data model

**Performance:**
- **File size:** Moderate (separate files)  
- **Speed:** Good for small datasets, slower on very large datasets (no modern indexing)

**Best For:** Legacy projects and maximum interoperability, but not recommended for new development.

---

## 2. GeoJSON (.geojson / .json)
**Pros:**
- Human-readable, simple JSON structure
- Natively supported by web mapping frameworks (Leaflet, Mapbox, OpenLayers)
- Easily transferable over the web (REST APIs, web services)
- Open standard, no licensing restrictions

**Cons:**
- Larger file size compared to binary formats
- No built-in spatial indexing → slower for very large datasets
- Limited attribute storage and data types
- Precision issues for very detailed geometries

**Performance:**
- **File size:** Larger than binary (text-based)  
- **Speed:** Fast for small datasets, can lag with very large datasets due to lack of indexing

**Best For:** Web mapping, lightweight spatial data sharing, APIs.

---

## 3. GeoPackage (GPKG)
**Pros:**
- Single file (SQLite-based database)
- Supports **both vector and raster** in one file
- Open standard (OGC), widely adopted and modern
- Supports large datasets efficiently (up to multiple terabytes)
- Can store multiple layers and spatial indexes for fast querying

**Cons:**
- Slightly more complex than simple text formats (like GeoJSON)
- Not as ubiquitous as Shapefile (older software may not support it well)
- Editing in parallel (multiple users) is tricky compared to databases

**Performance:**
- **File size:** Efficient (binary SQLite)  
- **Speed:** High performance due to indexing and database optimization

**Best For:** Modern offline GIS projects, mobile apps, multi-layer portable GIS datasets.

---

## 4. FlatGeobuf (.fgb)
**Pros:**
- Very fast (streamable binary format with spatial indexing)
- Designed for **web + cloud streaming** use cases
- Open format, good for very large datasets
- Supported by modern GIS libraries (GDAL, QGIS, etc.)

**Cons:**
- Newer format → not universally supported yet
- Binary format (not human-readable like GeoJSON)
- Limited tooling compared to Shapefile and GeoPackage

**Performance:**
- **File size:** Small (binary + compression)  
- **Speed:** Excellent (built-in indexing and streaming support)

**Best For:** Web/cloud mapping with very large vector datasets and fast queries.

---

## 5. File Geodatabase (.gdb)
**Pros:**
- Handles massive datasets efficiently
- Supports advanced data types, topology, and relationships
- Supported by ArcGIS and QGIS (read/write via GDAL)
- Good for enterprise workflows

**Cons:**
- Proprietary ESRI format (not fully open)
- Some features only fully supported in ArcGIS
- Complex structure (folder with multiple files)

**Performance:**
- **File size:** Compact for large datasets  
- **Speed:** Very high performance in ESRI software, good in others

**Best For:** ESRI-based enterprise workflows where performance and advanced GIS capabilities are required.

---

## 6. Well-Known Text (WKT)
**Pros:**
- Simple text representation of geometries (e.g., `POINT (30 10)`, `POLYGON ((30 10, 40 40, 20 40, 10 20, 30 10))`)
- Human-readable and easy to embed in databases or text files
- Widely supported across GIS and databases (PostGIS, Oracle Spatial, etc.)
- Easy to generate and parse programmatically

**Cons:**
- Purely geometric (no attribute schema or styling information)
- Larger file sizes than binary for big datasets
- No inherent spatial indexing
- Slower to parse compared to binary formats (WKB)

**Performance:**
- **File size:** Large for big datasets (text overhead)  
- **Speed:** Slower parsing vs. binary (WKB)

**Best For:** Geometry exchange in text-based systems, debugging spatial data, database geometry fields.

---

## 7. Well-Known Binary (WKB)
**Pros:**
- Compact binary representation of geometries
- Faster to parse and store compared to WKT
- Standardized (OGC) and widely supported by spatial databases (e.g., PostGIS)
- Efficient for storage and transmission

**Cons:**
- Not human-readable
- Purely geometric (no attributes, no styling)
- Requires software to interpret (less transparent than WKT)

**Performance:**
- **File size:** Smaller than WKT (binary)  
- **Speed:** Faster than WKT, excellent for storage and transport

**Best For:** Internal database storage, high-performance spatial data exchange, APIs requiring binary payloads.

---

## Summary Table

| Format          | File Type | Max Size  | Human Readable | Multi-Layer | Indexing | Web-Friendly | Open Standard | Performance |
|-----------------|-----------|-----------|----------------|-------------|----------|---------------|----------------|--------------|
| **Shapefile**   | Multiple  | 2 GB      | Partial (DBF)  | No          | Basic    | No            | Partially      | Moderate     |
| **GeoJSON**     | Single    | ~GBs      | Yes (JSON)     | No          | No       | Yes           | Yes            | Good small, slow large |
| **GeoPackage**  | Single    | TBs       | No (SQLite DB) | Yes         | Yes      | Moderate      | Yes            | High         |
| **FlatGeobuf**  | Single    | TBs       | No (Binary)    | Yes         | Yes      | Yes           | Yes            | Very High    |
| **File GDB**    | Folder    | TBs       | No             | Yes         | Yes      | No            | No             | High (ESRI)  |
| **WKT**         | Text      | Depends   | Yes            | No          | No       | Limited       | Yes            | Slow         |
| **WKB**         | Binary    | Depends   | No             | No          | No       | Limited       | Yes            | Fast         |

---

## Recommended Usage by Scenario

### **Web Applications**
- **Best:** GeoJSON (for small/medium data), FlatGeobuf (for large datasets, streaming)
- **Alternative:** GeoPackage for downloadable data packages

### **Enterprise GIS**
- **Best:** File Geodatabase (for ArcGIS users), GeoPackage (for open standard approach)
- **Alternative:** Shapefile (legacy support)

### **Open Data Portals**
- **Best:** GeoJSON (easy access, human-readable), GeoPackage (single file, robust)
- **Alternative:** Shapefile for legacy support, FlatGeobuf for large datasets

### **Mobile & Offline GIS**
- **Best:** GeoPackage (all-in-one database)
- **Alternative:** Shapefile (if legacy support required)

### **Databases & APIs**
- **Best:** WKT (simple debugging, text-based exchange), WKB (high-performance binary storage)
- **Alternative:** GeoJSON for JSON-based REST APIs

---
