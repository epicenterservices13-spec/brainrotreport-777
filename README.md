# Brainrot Report 777 (`brainrotreport-777`)
### Autonomous AI Construction Intelligence & Spatial MEP Coordination Platform

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![AWS Powered](https://img.shields.io/badge/Cloud-AWS%20Spatial%20Foundry-orange?logo=amazon-aws)](https://aws.amazon.com/)
[![PostGIS 3D](https://img.shields.io/badge/Spatial-PostgreSQL%20%2B%20PostGIS-336791?logo=postgresql)](https://postgis.net/)
[![Python 3.11+](https://img.shields.io/badge/Python-3.11%2B-blue?logo=python)](https://python.org)

---

## 🏗️ Overview

**Brainrot Report 777** is an autonomous AI-powered Architecture, Engineering, and Construction (AEC) intelligence platform. Traditional takeoff and plan management tools (Bluebeam, Procore, Togal.AI) treat drawings as flat, static pixels. **Brainrot Report 777** dissolves this abstraction, transforming 2D architectural and MEP (Mechanical, Electrical, Plumbing) drawings—whether digital PDFs or mobile camera field captures—into coordinated 2.5D/3D spatial intelligence.

By combining computer vision, vector operator extraction, domain-specific AI agents, and deterministic 3D geometric collision engines, the platform delivers:
- **Instant Multi-Discipline Takeoffs**: Material lengths, fixture counts, duct surface areas, and circuit totals.
- **True 3D Clash Detection**: Elevation-aware spatial clash analysis between disciplines before physical installation.
- **Code Compliance Verification**: Automated validation against building codes (e.g., NEC 110.26 electrical clearance zones, UPC drainage fixture unit slopes).
- **Contractual RFI Generation**: Autonomous drafting of field Requests for Information (RFIs) with localized drawing callout crops.

---

## ⚡ System Architecture

```
Photo / PDF Drawing Upload (Mobile / Web)
                  │
                  ▼
   [ S3 Direct Multipart Ingest Bucket ]
                  │
                  ▼
     Plan Normalizer & Scanner Agent
  (PyMuPDF Vector Extraction / OpenCV Homography)
                  │
                  ▼
     High-Resolution Tiling Engine
     (1024x1024 Coordinate-Indexed Tiles)
                  │
 ┌────────────────┼────────────────┐
 ▼                ▼                ▼
Plumbing      Mechanical       Electrical
 Agent          Agent            Agent
 ▼                ▼                ▼
Hydro-Graph    Ductwork 3D     Circuit & Load
 (DFU / Invert) (BOD / CFM)     (NEC 110.26)
 ▼                ▼                ▼
Takeoff &      Equipment &      Panels &
 Quantities     Diffusers        Clearances
 └────────────────┬────────────────┘
                  │
                  ▼
    [ Amazon Aurora PostGIS 3D ]
    (GiST R-Tree Spatial Indexing)
                  │
                  ▼
      Spatial Coordination Agent
   (Deterministic Collision Matrix)
                  │
         ┌────────┴────────┐
         ▼                 ▼
   3D Clash Report    Contractual RFIs
   & Cost Takeoff     (Claude 3.5 Sonnet)
```

---

## 🤖 The Agent Roster

### 1. Plan Scanner & Normalizer Agent
- **Vector vs. Raster Separation**: Parses digital PDFs directly via vector operator streams (`m`, `l`, `c`, `re`, `Tj`) preserving millimeter-accurate lines and text without raster loss.
- **Homography Correction**: Warps distorted jobsite mobile photographs into planar alignment using OpenCV 4-point perspective transforms.
- **Scale Calibration**: Automatically identifies graphic bar scales and cross-validates against structural column grid dimensions.
- **High-Res Tiling**: Slices large-format $E$-size sheets ($36" \times 48"$ at $300\text{ DPI} \approx 155\text{ MP}$) into sliding-window coordinate-indexed tiles.

### 2. Plumbing Agent
- **Extraction**: Sanitary sewer, domestic cold/hot water, vent stacks, fixture units, and pipe diameters.
- **Topological Logic**: Models piping as directed acyclic graphs (DAGs) tracking gravity slopes ($1/8"$ or $1/4"$ per foot) and invert elevations.
- **Code Engine**: Evaluates pipe capacities against Uniform Plumbing Code (UPC) and International Plumbing Code (IPC) Drainage Fixture Unit (DFU) tables.

### 3. Mechanical (HVAC) Agent
- **Extraction**: Supply, return, and exhaust ductwork, VAV boxes, RTUs, diffusers, and fire/smoke dampers.
- **3D Spatial Reconstruction**: Combines duct dimensions (e.g., `24x12`) with Bottom-of-Duct (`BOD`) callouts and Reflected Ceiling Plans (`RCP`) to extrude 3D Axis-Aligned Bounding Boxes (AABB).
- **Checks**: Verifies airflow (CFM) velocities, aspect ratios, and missing equipment tag callouts.

### 4. Electrical Agent
- **Extraction**: Switchboards, distribution panels, transformers, lighting fixtures, receptacles, and homeruns.
- **Schedule Parsing**: Extracts panel schedules (breaker ratings, phase loads, wire sizes) via OCR table extraction.
- **NEC 110.26 Clearance Zone Enforcement**: Extrudes an unyielding 3D spatial exclusion zone in front of electrical equipment ($3\text{--}4\text{ ft}$ depth, $30\text{ in}$ width, $6.5\text{ ft}$ height). Any foreign piping or ductwork entering this volume triggers a critical clash.

### 5. Spatial Coordination Agent
- **Deterministic Collision Detection**: Loads all extruded discipline volumes into a PostGIS 3D GiST / R-Tree spatial index to evaluate true 3D spatial intersections:
  $$\text{Intersection}(A, B) = A_{\text{box}} \cap B_{\text{box}} \neq \emptyset$$
- **Filters**: Distinguishes valid wall penetrations from hard physical collisions and maintenance accessibility blocks.
- **RFI Synthesizer**: Leverages Amazon Bedrock (Claude 3.5 Sonnet) to draft formal, professional RFI documents citing specific sheet numbers, grid coordinates, and recommended engineering solutions.

---

## ☁️ AWS Cloud Foundry Architecture

```
[ Client / Web App ]
         │
         │ 1. Request Presigned URL
         ▼
[ API Gateway + Lambda ]
         │
         │ 2. S3 Presigned Multipart Upload
         ▼
[ Amazon S3 Ingestion Bucket ]
         │
         │ 3. s3:ObjectCreated Event
         ▼
[ Amazon EventBridge / SQS ]
         │
         │ 4. Orchestration Trigger
         ▼
[ AWS Step Functions (Distributed Map) ]
         │
         ├──────────────────────────────────────────────┐
         ▼                                              ▼
[ ECS Fargate Normalizer ]                      [ AWS Textract ]
• PyMuPDF Vector Split                          • Panel Schedules
• OpenCV Homography                             • Equipment Tables
• Coordinate Tiling                             • Key-Value OCR
         │
         ├──────────────────────────────────────────────┐
         ▼                                              ▼
[ Parallel Discipline Workers ]                 [ Amazon Bedrock ]
• Plumbing Worker (DFU / Invert)                • Claude 3.5 Sonnet
• Mechanical Worker (BOD / Duct AABB)           • Multimodal Analysis
• Electrical Worker (NEC 110.26)                • RFI Formulation
         │
         ▼
[ Amazon Aurora Serverless v2 PostgreSQL + PostGIS (3D) ]
• Spatial Indexing (GiST / R-Tree)
• ST_3DIntersects Collision Queries
         │
         ▼
[ Coordination Engine Worker ]
• Clash Matrix Generation
• Takeoff Bill of Materials (BOM)
```

---

## 🗄️ Database Schema (PostgreSQL + PostGIS)

```sql
-- Core Projects
CREATE TABLE projects (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name TEXT NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Plan Sheets & Spatial Calibration
CREATE TABLE plan_sheets (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    project_id UUID REFERENCES projects(id) ON DELETE CASCADE,
    sheet_number TEXT NOT NULL,         -- e.g. 'M101', 'P101', 'E101'
    discipline TEXT NOT NULL,           -- 'Mechanical', 'Electrical', 'Plumbing'
    scale_ratio DOUBLE PRECISION,       -- Drawing units to millimeters
    origin_x DOUBLE PRECISION DEFAULT 0.0,
    origin_y DOUBLE PRECISION DEFAULT 0.0,
    elevation_aff DOUBLE PRECISION DEFAULT 0.0 -- Floor elevation AFF (mm)
);

-- Extracted MEP Entities in 3D Space
CREATE TABLE mep_entities (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    sheet_id UUID REFERENCES plan_sheets(id) ON DELETE CASCADE,
    discipline TEXT NOT NULL,
    entity_type TEXT NOT NULL,          -- 'duct', 'pipe', 'panel', 'vav', 'fixture'
    tag TEXT,                           -- 'VAV-1', 'LP-1A', 'WC-1'
    elevation_bottom DOUBLE PRECISION,  -- Millimeters AFF
    elevation_top DOUBLE PRECISION,     -- Millimeters AFF
    geometry_2d GEOMETRY(Geometry, 0),  -- 2D footprint or centerline
    bounding_box_3d BOX3D,              -- Extruded 3D envelope for collision detection
    attributes JSONB                    -- Sizing, CFM, voltage, material specs
);

-- Detected Clashes & Coordination Issues
CREATE TABLE clashes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    project_id UUID REFERENCES projects(id) ON DELETE CASCADE,
    entity_a_id UUID REFERENCES mep_entities(id),
    entity_b_id UUID REFERENCES mep_entities(id),
    clash_type TEXT NOT NULL,           -- 'HARD_COLLISION', 'NEC_CLEARANCE', 'ACCESSIBILITY'
    severity TEXT NOT NULL,             -- 'CRITICAL', 'WARNING', 'INFORMATIONAL'
    description TEXT NOT NULL,
    suggested_fix TEXT,
    rfi_draft TEXT,
    resolved BOOLEAN DEFAULT FALSE
);
```

---

## 🗺️ 90-Day Implementation Roadmap

| Phase | Milestone | Core Deliverables |
|:---|:---|:---|
| **Phase 1 (Days 1–30)** | **Ground Truth Ingestion** | S3 Multipart Ingest, PyMuPDF vector extractor, OpenCV 4-point homography pipeline, automated scale calibration. |
| **Phase 2 (Days 31–60)** | **The MEP Triad** | Plumbing DFU tracer, Mechanical duct 3D AABB builder, Electrical panel schedule parser & NEC 110.26 clearance engine. |
| **Phase 3 (Days 61–90)** | **Spatial Clash & RFIs** | PostGIS 3D collision matrix, automated RFI drafting via Claude 3.5 Sonnet, contractor takeoff export (CSV/JSON/PDF). |
| **Phase 4 (Horizon)** | **BIM & Ecosystem** | IFC-Lite / Autodesk Revit export, subcontractor vendor procurement matching, building energy simulation loops. |

---

## 🚀 Quickstart

### Prerequisites
- Python 3.11+
- PostgreSQL 16+ with PostGIS 3.4+ extension
- AWS CLI configured with Bedrock & S3 permissions

```bash
# Clone the repository
git clone https://github.com/epicenterservices13-spec/brainrotreport-777.git
cd brainrotreport-777

# Set up virtual environment
python3 -m venv .venv
source .venv/bin/activate

# Install core dependencies
pip install -r requirements.txt
```

---

## 📜 License

Licensed under the [Apache License, Version 2.0](LICENSE).
