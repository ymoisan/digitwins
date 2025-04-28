# Urban Digital Twin (UDT) Project Plan

## 1. Goal

Develop an Urban Digital Twin (UDT) for a Canadian government organization. The initial phase will focus on integrating existing 3D building data, with subsequent phases incorporating tree data, demonstrating real-time data integration (**e.g., sensor data, camera orientation**), and exploring physics-based simulation capabilities. **A key objective is to establish and demonstrate collaborative, non-destructive workflows leveraging OpenUSD and Nvidia Omniverse Nucleus.** The UDT aims for interoperability using the OpenUSD format, with Nvidia Omniverse as the target platform for interactive visualization, analysis, simulation integration (**including dynamic visualization of camera FOVs**), **and collaborative development**. Data processing and storage will leverage Open Table Formats and cloud/object storage platforms, while exploring ESRI CityEngine for conversion, authoring, and enrichment workflows.

## 2. Scope

*   **Phase 1:** (Focus: Static Data Integration & Visualization Setup)
    *   Ingest and process existing 3D building data (GeoPackage, Cesium/glTF).
    *   Evaluate GeoParquet 1.1 for vector/attribute storage.
    *   Evaluate and establish pipelines to convert/import source formats into OpenUSD (including CityEngine workflows).
    *   Set up data handling workflows (Delta Lake on FSDH/MinIO).
    *   Set up Omniverse (Nucleus) environment.
    *   Evaluate visual fidelity and data integrity.
*   **Phase 2:** (Focus: Enrichment, Procedural Content, **& Basic Collaboration**)
    *   Investigate methods for sourcing or generating point locations for trees.
    *   Utilize ArcGIS CityEngine's built-in vegetation libraries and procedural rules to place 3D tree models.
    *   Integrate the procedurally generated vegetation into the OpenUSD scene.
    *   Explore and implement interactive features within Omniverse (data querying, basic scenario simulation).
    *   Integrate outputs from newer processing tools like Roofer.
    *   **Define and implement a strategy for USD scene composition (layering, referencing) to enable non-destructive updates.**
    *   **Demonstrate a non-destructive workflow for updating building data (add/replace) within the UDT scene using USD layers/references and Omniverse Nucleus.**
*   **Phase 3 (Future):** (Focus: Dynamic Data, Simulation Integration, **& Advanced Collaboration**)
    *   **Set up the Omniverse Kit SDK development environment.**
    *   Develop proofs-of-concept for integrating simulated real-time data with the UDT in Omniverse:
        *   **IoT Sensor Data:** Update building visualization (e.g., color) based on simulated temperature data.
        *   **Camera Field of View (FOV):** Dynamically visualize camera FOV (e.g., a cone) based on simulated orientation data streams.
    *   Develop a proof-of-concept for integrating physics-based simulation results (flood analysis using SimScale).
    *   Prepare necessary UDT data for simulations (terrain, simplified buildings, camera locations/specs).
    *   Run basic simulation scenarios (flood, camera orientation) and visualize results/updates within the Omniverse UDT scene.
    *   Further explore and refine collaborative workflows.
    *   Research and document potential architectures for scaling real-time data handling, simulation workflows, **and collaborative scene management.**

## 3. Technology Stack

*   **Input Data Formats:**
    *   3D Buildings: 3D GeoPackage, Cesium tiles (containing glTF v2.0)
    *   Building Footprints: GeoPackage, Shapefile
    *   Point Clouds: LAS/LAZ
    *   **Terrain Data:** (Required for flood simulation - format TBD, e.g., DEM from Lidar)
    *   Tree Data (Phase 2): Point locations
    *   **Camera Data (Phase 3):** Point locations (with height/initial orientation), Camera specifications (potentially URL refs)
    *   Simulated Real-Time Data (Phase 3): JSON, MQTT (for temperature, camera orientation)
*   **Intermediate / Component Formats:**
    *   glTF (v2.0)
*   **3D Authoring / Conversion / Generation Tools:**
    *   ArcGIS CityEngine
    *   GDAL/OGR, Custom Python Scripts
    *   Geoflow3D, Roofer
*   **Intermediate/Storage Formats:**
    *   GeoParquet (v1.1), Delta Lake
    *   OpenUSD
*   **Target Visualization, Simulation, & Collaboration Platform:** Nvidia Omniverse
    *   **Omniverse Nucleus:** Collaboration server, **essential for version control, permissions, and non-destructive layering workflows.** [https://docs.omniverse.nvidia.com/nucleus/latest/index.html](https://docs.omniverse.nvidia.com/nucleus/latest/index.html)
    *   Omniverse CityEngine Connector
    *   **Omniverse Kit SDK:** For developing custom extensions/scripts (Phase 3 setup required).
    *   Potential Omniverse Microservices
    *   **Omniverse SimScale Converter Extension** (Phase 3)
    *   **Omniverse USD Primitives:** (e.g., `UsdGeomCamera` for camera representation, `UsdGeomCone` or custom geometry for FOV visualization)
*   **Cloud Simulation Platform:**
    *   **SimScale:** For CFD/FEA simulations (Phase 3).
*   **Data Platforms / Storage:**
    *   Azure Databricks (FSDH)
    *   MinIO
*   **Potential Supporting Tools/Libraries:**
    *   **Geospatial:** PDAL, Fiona, Shapely, rasterio (for terrain)
    *   **Parquet/Delta:** `delta-rs`, PyArrow
    *   **3D/USD/glTF:** USD Python Bindings, Cesium libraries, CityJSON libraries, glTF libraries
    *   **Real-Time Simulation (Phase 3):** Python libraries for data generation/streaming, Omniverse Python scripting.
    *   **Data Handling/Processing:** Python, Spark (Databricks), TOML
    *   **Camera Data Parsing:** Libraries for parsing camera specifications

## 4. Data Sources

*   **Primary:** Internally generated 3D Buildings (GeoPackage, Cesium Tiles/glTF).
*   **Supporting/Reference:** Footprints (GeoAI, Auto Buildings, ODB), Lidar Point Clouds.
*   **Phase 2:** Potential Tree Point Locations.
*   **Phase 3:** Simulated IoT Sensor Data, **Camera location/spec data**, **Terrain data for simulation**, **Simulation results from SimScale**.

## 5. High-Level Direction

1.  **Data Conversion & Storage Strategy:** (As before - GeoPackage/glTF -> CityEngine/GeoParquet -> Delta Lake).
2.  **USD Pipeline Evaluation:** (As before - Compare multiple pathways).
3.  **Platform Integration:** (As before - Omniverse, CityEngine, FSDH, MinIO).
4.  **USD Scene Architecture:** Define a robust scene composition strategy using USD layering and referencing early on to facilitate non-destructive updates and collaboration through Omniverse Nucleus.
5.  **Scalability & Performance:** (Adjusted from 4 - Leverage platforms for scale).
6.  **Interoperability & Enrichment:** (Adjusted from 5 - Adhere to standards, leverage CityEngine).
7.  **Phased Approach:** Tackle buildings (Phase 1), then trees **and basic non-destructive workflows** (Phase 2), then demonstrate real-time data links, simulation, **and potentially more advanced collaboration** (Phase 3).

## 6. Risks & Challenges

*   (Existing risks remain regarding format conversion, tooling, scalability, learning curve, licensing, data consistency, Delta Lake management, FSDH environment)
*   **Real-Time Integration Complexity (Phase 3):** Developing custom Omniverse extensions/scripts **using Kit SDK** requires specific knowledge. Ensuring performance with potentially high-frequency data streams needs investigation.
*   **Simulation vs. Reality (Phase 3):** The initial PoC will use simulated data. Bridging to actual real-world IoT data sources involves additional complexities (protocols, security, data infrastructure).
*   **USD Schema for Real-Time Data/Cameras:** Defining how real-time data links, camera parameters (`UsdGeomCamera`), and dynamic FOV geometry are represented within USD.
*   **Simulation Data Preparation (Phase 3):** Preparing accurate and suitable geometry (terrain, simplified buildings) and boundary conditions for CFD/FEA simulation in SimScale can be complex and time-consuming.
*   **Simulation Complexity & Expertise (Phase 3):** Setting up, running, and interpreting CFD simulations requires specific domain knowledge and familiarity with SimScale platform.
*   **Omniverse-SimScale Integration (Phase 3):** Reliance on the SimScale Converter Extension; potential limitations in data transfer (geometry complexity, result types), performance, or version compatibility.
*   **USD Composition Complexity:** Managing a potentially complex structure of USD layers and references for a large-scale UDT requires careful planning and adherence to conventions.
*   **Collaboration Workflow Management:** Defining clear protocols for how different users/teams contribute updates via Nucleus to avoid conflicts and maintain scene integrity.
*   **Performance with Deep Composition/Dynamic Updates:** Highly layered scenes **or scenes with many dynamically updated elements (like camera FOVs)** can impact performance. 