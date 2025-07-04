# TOP POSTING : this document is overkill.  Will delete soon.

# UDT Project Tasks

## Plan Items

Below is a summary of the task items. 

Legend of annotations: (based on the VSCode GitHub conventions)

| Mark | Description              |
| ---- | ------------------------ |
| 🏃    | work in progress         |
| ✋    | blocked task             |
| 💪    | stretch goal             |
| 🔴    | missing issue reference  |
| 🔵    | more investigation needed |
| ⚫    | under discussion         |
| ⬛    | large work item          |
| ✅    | completed                |
| ❌    | won't do / cancelled     |

---

## Project Setup & Environment

1.  [ ] **Task: Setup Pixi Environment**
    *   Objective: Initialize the project using Pixi for reproducible dependency management.
    *   Actions:
        *   [ ] Install Pixi ([https://pixi.sh/](https://pixi.sh/)).
        *   [ ] Run `pixi init` to create `pixi.toml`.
        *   [ ] Define initial project channels (e.g., `conda-forge`) and target platforms.
        *   [ ] Add basic Python version and initial core libraries (e.g., `python`, `marimo`).
        *   [ ] Run `pixi install` to resolve and install dependencies.
        *   [ ] Document Pixi usage conventions for the project (adding dependencies, running scripts).

2.  [ ] **Task: Setup Marimo & Notebook Workflow**
    *   Objective: Establish Marimo as the notebook environment and define the notebook/sidecar coding pattern.
    *   Actions:
        *   [ ] Ensure Marimo is installed via Pixi.
        *   [ ] Create a sample Marimo notebook (`.py`) demonstrating basic usage.
        *   [ ] Create a corresponding sample sidecar Python module (`.py`) with a simple function.
        *   [ ] Demonstrate importing and using the sidecar function within the Marimo notebook.
        *   [ ] Document the preferred workflow: Core logic in `.py` modules, interactive exploration/presentation in Marimo notebooks.

## Phase 1: Static Data Integration & Visualization Setup

3.  [ ] **Task: Data Assessment & Inventory**
    *   Objective: Fully understand the structure, attributes, coordinate systems, and quality of the existing **2D footprint datasets (e.g., GeoPackage, Shapefile) and (optionally) existing 3D building datasets (GeoPackage, Cesium tiles)**. Assess suitability for GeoParquet 1.1 storage, attribute enrichment, and use in direct conversion vs. procedural generation pipelines.
    *   Actions:
        *   [ ] Gather sample datasets for both 2D and 3D formats.
        *   [ ] Use tools (e.g., QGIS, GDAL/OGR, **Marimo/Python**) to inspect file structure, metadata, CRS, and attribute tables for **footprint data**. Check for potential GERS ID usage or linkable identifiers.
        *   [ ] **Assess availability and quality of attributes relevant to procedural generation** (height, floors, roof type etc.) within the footprint data or linkable sources. Document alignment with Overture `building`/`building_part` schemas.
        *   [ ] (If pursuing direct 3D conversion) Analyze 3D geometry representation (e.g., MultiSurface, PolyhedralSurface) in GeoPackage.
        *   [ ] (If pursuing direct 3D conversion) Specifically examine Cesium Tiles: Identify version (e.g., 1.0 with b3dm, 1.1 with direct glTF). If b3dm, investigate tools/methods for extracting embedded glTF 2.0 content. Evaluate the structure and content (geometry, materials) of the extracted/direct glTF.
        *   [ ] Evaluate how **footprint data and attributes map to GeoParquet 1.1 representations**. Evaluate how **3D data** maps to potential CityEngine import capabilities.
        *   [ ] Document findings, noting suitability for different pipelines (**Procedural Generation from Footprints/Attributes**, Direct 3D Conversion, CityEngine import) and potential issues (CRS mismatches between Lidar/Footprints, attribute gaps, geometry complexity).
        *   [ ] **Identify potential terrain data sources**.
        *   [ ] **Identify potential camera location data sources**.

4.  [ ] **Task: Conversion & Pipeline Tool Evaluation**
    *   Objective: Identify and evaluate potential tools/libraries for the different pipeline stages: Source -> Intermediate (GeoParquet/Attributes/CityEngine), Intermediate -> USD (via direct conversion OR procedural generation), and Delta Lake management.
    *   Actions:
        *   [ ] Research tools for Source Footprints/Attributes -> GeoParquet: GDAL, GeoPandas, **Python scripts (using sidecar pattern)**, FME.
        *   [ ] (If using direct 3D) Research tools for Cesium Tiles -> glTF.
        *   [ ] Research tools for Source 3D/glTF -> CityEngine: CityEngine's importers.
        *   [ ] Research tools/methods for **Procedural Generation (Footprints + Attributes -> USD):**
            *   [ ] Custom Python scripts (**Marimo/sidecar pattern**) using libraries like Shapely (for 2D), potentially PyVista/Trimesh (for 3D mesh generation), and USD Python bindings.
            *   [ ] CityEngine using CGA rules driven by attributes.
        *   [ ] Research tools for GeoParquet/CityEngine/glTF -> USD (Direct Conversion): CityEngine USD Exporter, CityEngine Omniverse Connector, Custom Python scripts (USD bindings reading GeoParquet/glTF), Omniverse connectors.
        *   [ ] Investigate Delta Lake interaction methods: **Marimo notebooks (calling sidecar functions)**, `delta-rs` library.
        *   [ ] Evaluate based on: Format support, attribute handling (**especially for procedural logic**), CRS reprojection, **procedural generation capabilities**, performance, ease of use, licensing, platform compatibility.
        *   [ ] Document pros and cons of top candidates for each potential pipeline step **within Marimo notebooks**.

5.  [ ] **Task: Define Attribute Requirements & Schema Mapping (Procedural Generation)**
    *   Objective: Define the specific attributes needed for procedural generation at target LoDs and map them to Overture schemas.
    *   Actions:
        *   [ ] Analyze Overture `building` and `building_part` schemas in detail ([https://docs.overturemaps.org/schema/reference/buildings/building/](https://docs.overturemaps.org/schema/reference/buildings/building/), [https://docs.overturemaps.org/schema/reference/buildings/building_part/](https://docs.overturemaps.org/schema/reference/buildings/building_part/)).
        *   [ ] Define target Levels of Detail (LoD) for procedural models (e.g., LoD1: Extrusion, LoD2: Extrusion + Basic Roof Shapes).
        *   [ ] Determine the minimum and desired set of attributes required for each target LoD (e.g., `height` or `num_floors` for LoD1; `roof_shape`, `roof_height`, `min_height` etc. for LoD2).
        *   [ ] Document the mapping between required attributes and the Overture schema fields.
        *   [ ] Investigate and document potential strategies for handling missing attributes (e.g., default values, estimations).

6.  [ ] **Task: FSDH (Databricks) Setup & Familiarization**
    *   Objective: Gain access to and basic proficiency with the Federal Science DataHub (FSDH) Azure Databricks environment. (Note: Marimo/Pixi may not be directly usable here, stick to Databricks notebooks/standard Python envs).
    *   Actions:
        *   [ ] Obtain necessary credentials/access for FSDH. [https://poc.fsdh-dhsf.science.cloud-nuage.canada.ca/](https://poc.fsdh-dhsf.science.cloud-nuage.canada.ca/)
        *   [ ] Explore the Databricks workspace interface.
        *   [ ] Run basic Spark SQL / PySpark commands.
        *   [ ] Learn how to mount/access storage.
        *   [ ] Test creating a simple Delta Lake table.
        *   [ ] Document setup, access procedures, and initial findings.
        *   [ ] **Test basic Delta operations**.

7.  [ ] **Task: Delta-rs & MinIO Setup & Familiarization**
    *   Objective: Set up `delta-rs` and test basic Delta Lake operations against an internal MinIO object storage instance **using Marimo and Pixi**. 
    *   Actions:
        *   [ ] Ensure MinIO instance is accessible.
        *   [ ] Install `delta-rs` Python library **via Pixi**.
        *   [ ] Configure `delta-rs` to connect to MinIO **within a Python module**.
        *   [ ] Test writing a simple DataFrame to a Delta table on MinIO **using a Marimo notebook calling the module**.
        *   [ ] Test reading the Delta table back **using a Marimo notebook calling the module**.
        *   [ ] Document setup, configuration for MinIO, and basic read/write operations **in a Marimo notebook**.

8.  [ ] **Task: CityEngine Setup & Familiarization**
    *   Objective: Install ArcGIS CityEngine, confirm licensing, and gain basic proficiency in data import (GeoPackage, glTF), **attribute-driven procedural generation (CGA basics)**, USD export/connection, and vegetation placement.
    *   Actions:
        *   ✋ Verify CityEngine license availability and terms. <b><font color="red">Waiting for free trial license.</font></b>
        *   [ ] Install CityEngine and the Omniverse Connector. [https://chr11115.github.io/cityengine/omniverse](https://chr11115.github.io/cityengine/omniverse)
        *   [ ] Follow introductory tutorials (interface, scene setup, data import).
        *   [ ] **Follow basic CGA tutorials focusing on attribute-driven extrusion and roof forms.**
        *   [ ] Test importing sample 3D GeoPackage data.
        *   [ ] Test importing sample extracted/direct glTF 2.0 files (assess fidelity, material handling).
        *   [ ] Test exporting a simple **procedurally generated** scene to USD.
        *   [ ] Explore the ESRI.lib Vegetation library: Browse available models, understand placement methods (e.g., point-based, procedural rules using CGA).
        *   [ ] Test placing sample vegetation using points or simple rules.
        *   [ ] Document setup, licensing, import/export findings, **CGA capabilities for procedural generation**, and initial vegetation library assessment.

9.  [ ] **Task: Nvidia Omniverse, Nucleus, & Kit SDK Setup & Familiarization**
    *   Objective: Install Omniverse core applications, set up Nucleus, **install the Kit SDK,** and gain basic proficiency in importing/visualizing USD assets, connecting CityEngine, **understanding Nucleus collaboration features, and running a basic Kit SDK sample.**
    *   Actions:
        *   ✋ Review Omniverse/Nucleus/Kit SDK system requirements and install necessary components (including Nucleus server and Kit SDK). [https://docs.omniverse.nvidia.com/nucleus/latest/index.html](https://docs.omniverse.nvidia.com/nucleus/latest/index.html), [https://docs.omniverse.nvidia.com/kit/docs/kit-manual/latest/overview.html](https://docs.omniverse.nvidia.com/kit/docs/kit-manual/latest/overview.html) <b><font color="red">Waiting for an adequate GPU for tests.</font></b>
        *   [ ] Configure Nucleus.
        *   [ ] Follow introductory tutorials (Omniverse UI, USD import, navigation).
        *   [ ] **Explore Nucleus features: Version control (checkpoints), permissions, live sync capabilities.**
        *   [ ] Test connecting CityEngine to Nucleus.
        *   [ ] **Follow Kit SDK introductory tutorials for setting up a development environment (e.g., configuring Python environment).**
        *   [ ] **Build and run a basic "hello world" style extension or script within Kit.**
        *   [ ] Investigate other Omniverse connectors.
        *   [ ] Document setup process (including Nucleus **and Kit SDK**) and initial impressions.

10. [ ] **Task: Proof of Concept (Pipeline Option 1: Procedural Generation via Python)**
    *   Objective: Test the Footprint/Attributes (GeoParquet/Delta) -> Procedural Python Script -> USD -> Omniverse pipeline end-to-end with a small sample **using Marimo/Pixi/Sidecar pattern**.
    *   Actions:
        *   [ ] Prepare sample footprint data with relevant attributes (aligned with Task 5) stored in GeoParquet/Delta Lake (MinIO/delta-rs) **using Marimo/module**. Ensure GERS ID or linkable ID is present.
        *   [ ] Develop/run **Python module script** (using e.g., Shapely, PyVista, USD bindings) to:
            *   [ ] Read footprint geometry and Overture-structured attributes from Delta Lake.
            *   [ ] Generate 3D geometry (e.g., LoD1 extrusion) based on attributes.
            *   [ ] Write the resulting geometry and attributes to a USD file.
        *   [ ] Import the resulting USD into Omniverse.
        *   [ ] Validate successful generation, alignment with basemap (if available), and visual representation **within a Marimo notebook**.
        *   [ ] Document process, code snippets, results, and any challenges **in a Marimo notebook**.
        *   [ ] **Noting USD structure for layering**.

11. [ ] **Task: Proof of Concept (Pipeline Option 2: Direct 3D Conversion - Optional)**
    *   Objective: (If pursuing) Test the Source 3D (GeoPackage or Cesium/glTF) -> Intermediate -> USD -> Omniverse pipeline end-to-end with a small sample. Compare results (fidelity, alignment) with procedural approach.
    *   Actions:
        *   [ ] Convert sample source 3D data (e.g., single building/tile) -> Intermediate (e.g., glTF, GeoParquet if attributes extracted).
        *   [ ] Convert Intermediate -> USD (e.g., using Python script or CityEngine export).
        *   [ ] Import the resulting USD into Omniverse.
        *   [ ] Validate data integrity and visual fidelity. **Critically assess vertical alignment issues.**
        *   [ ] Document process, results, and challenges (alignment, conversion fidelity).
        *   [ ] **Noting USD structure for layering**.

12. [ ] **Task: Proof of Concept (Pipeline Option 3: Procedural Generation via CityEngine)**
    *   Objective: Test the Footprint/Attributes -> CityEngine (CGA) -> USD (Export or Connector) -> Omniverse pipeline end-to-end with a small sample.
    *   Actions:
        *   [ ] Import sample footprint data with relevant attributes into CityEngine.
        *   [ ] Develop basic CGA rule(s) to read attributes (mapped to Overture schema) and generate 3D geometry (e.g., LoD1/LoD2).
        *   [ ] Generate models in CityEngine.
        *   [ ] Export the model/scene from CityEngine as a USD file OR use the Omniverse Connector to send to Nucleus.
        *   [ ] Import/Open the resulting USD in Omniverse.
        *   [ ] Validate successful generation, alignment, and visual representation. Compare ease of rule writing vs. Python scripting.
        *   [ ] Document process, results, connector performance, and any challenges.
        *   [ ] **Noting USD structure for layering**.

## Phase 2: Enrichment, Procedural Content, & Basic Collaboration

13. [ ] **Task: Phase 2 Vegetation Workflow Definition**
    *   Objective: Define and test the specific workflow for integrating 3D trees using CityEngine's vegetation library.
    *   Actions:
        *   [ ] Identify/acquire sample point data sources for tree locations.
        *   [ ] Define required attributes for tree points (e.g., species identifier, height).
        *   [ ] Develop/select basic CGA rules or utilize CityEngine tools for placing vegetation from ESRI.lib based on input points/attributes.
        *   [ ] Test exporting/connecting the combined building+vegetation scene to Omniverse.
        *   [ ] Document the chosen workflow and steps for larger-scale implementation.

14. [ ] **Task: Roofer / Geoflow3D Evaluation (as Attribute Source?)**
    *   Objective: Evaluate Roofer/Geoflow3D not just for direct 3D output, but as a potential **source of geometric attributes** (e.g., accurate height, roof type estimation) that could enrich footprint data for the procedural pipeline.
    *   Actions:
        *   [ ] Review Roofer/Geoflow3D documentation regarding attribute output or analysis capabilities.
        *   [ ] Install tools **(document environment needs)** and run test reconstruction on sample data.
        *   [ ] Analyze outputs to determine if reliable attributes (height, roof form identifiers) can be extracted and linked back to footprints (e.g., via spatial join or common IDs).
        *   [ ] Assess feasibility of integrating this attribute extraction into the pipeline **(document in Marimo)**.
        *   [ ] Document findings and recommendations **in a Marimo notebook**.

15. [ ] **Task: Initial Interactive Feature Exploration (Omniverse)**
    *   Objective: Explore basic interactive capabilities within Omniverse using the integrated UDT data.
    *   Actions:
        *   [ ] Research Omniverse APIs/tools for object selection and attribute querying (e.g., using Python scripting, UI elements).
        *   [ ] Implement a simple PoC for selecting a building and displaying its attributes (read from USD or linked data).
        *   [ ] Document potential methods for more advanced UDT interactions.

16. [ ] **Task: USD Layering/Referencing Strategy Definition**
    *   Objective: Define a strategy for structuring the UDT scene using USD layers and references to facilitate non-destructive updates and collaboration.
    *   Actions:
        *   [ ] Research USD best practices for large scene composition (layering, referencing, variants, payloads).
        *   [ ] Propose a layering structure (e.g., base terrain layer, regional building layers, update layers, vegetation layers, simulation layers).
        *   [ ] Define how building updates (add/replace) will be managed within this structure (e.g., overriding prims in session layers, referencing updated asset files in dedicated layers).
        *   [ ] Consider how different data sources/pipelines feed into this structure.
        *   [ ] Document the proposed composition strategy.

17. [ ] **Task: Proof of Concept (Non-Destructive Building Update)**
    *   Objective: Demonstrate adding or replacing building data in the UDT scene using the defined USD composition strategy and Nucleus, **considering both procedurally generated and potentially direct 3D model workflows**.
    *   Actions:
        *   [ ] Prepare a sample "update" dataset (e.g., one new/modified building as a USD file).
        *   [ ] Load the base UDT scene (from one of the Phase 1 PoCs) in Omniverse via Nucleus.
        *   [ ] Implement the update according to the strategy (e.g., create/edit an "update" or "session" layer in Omniverse, add references/overrides for the new/modified building).
        *   [ ] Save the update layer back to Nucleus.
        *   [ ] Verify that the composed scene correctly displays the updated building(s) while base layers remain unchanged (check file structure/timestamps in Nucleus if possible).
        *   [ ] (Optional) Simulate a second user making a different, non-conflicting update in a separate layer.
        *   [ ] Document the update process, illustrating the non-destructive workflow using layers/references.

## Phase 3: Dynamic Data, Simulation Integration, & Advanced Collaboration

*   **Real-Time IoT Simulation:**
    18. [ ] **Task: Real-Time Integration Research (Omniverse/USD/Kit)**
        *   Objective: Understand how real-time data can be connected to and visualized within an Omniverse USD scene **using Kit SDK**.
        *   Actions:
            *   [ ] Research Omniverse Kit SDK documentation specifically for real-time data pipelines, event handling, UI creation, and Python scripting for dynamic updates.
            *   [ ] Investigate how external data streams (e.g., MQTT, WebSockets, APIs) can be ingested by a Kit-based application/extension.
            *   [ ] Explore USD schema possibilities for linking assets to external data or storing dynamic attributes.
            *   [ ] Research methods for modifying material properties (e.g., color) or other visual aspects of USD prims dynamically using Kit APIs.
    19. [ ] **Task: IoT Sensor Simulation Design**
        *   Objective: Design a simple simulation for the IoT temperature sensor PoC **using Python**.
        *   Actions:
            *   [ ] Define the simulated data format (e.g., JSON payload with building ID, sensor location ID, timestamp, temperature value).
            *   [ ] Choose a simulation method (e.g., simple **Python script/module** generating data, potentially publishing to MQTT or a basic web endpoint).
            *   [ ] Define the target building and sensor location within the sample UDT scene.
            *   [ ] Specify the desired visualization logic (e.g., map temperature range to a color gradient [blue -> white -> red]).
    20. [ ] **Task: Proof of Concept (Real-Time Temperature Visualization Update)**
        *   Objective: Implement the PoC to dynamically update a building's color in Omniverse based on simulated sensor data **using Kit SDK scripting and Python simulation module**.
        *   Actions:
            *   [ ] Develop the **Python simulation module** (from Task 18) to generate/stream temperature data.
            *   [ ] Develop an Omniverse script/extension **using Kit SDK APIs** that:
                *   [ ] Connects to the simulated data source.
                *   [ ] Identifies the target building prim in the USD stage.
                *   [ ] Applies the visualization logic: Reads temperature, calculates color, modifies the building's displayColor material attribute (or similar mechanism).
            *   [ ] Run the simulation and the Omniverse application together.
            *   [ ] Verify that the building color changes dynamically in response to simulated temperature fluctuations.
            *   [ ] Document the implementation approach, code snippets, and challenges encountered.
*   **Camera FOV Simulation:**
    21. [ ] **Task: Research USD Camera Representation & FOV Visualization**
        *   Objective: Understand how to represent cameras and their FOVs within a USD scene for dynamic updates **using Kit SDK**.
        *   Actions:
            *   [ ] Investigate `UsdGeomCamera` schema.
            *   [ ] Explore methods for visualizing the FOV dynamically.
            *   [ ] Research how to efficiently update camera orientation and the corresponding FOV visualization primitive via **Omniverse Kit SDK scripting**.
            *   [ ] Review examples or best practices.
    22. [ ] **Task: Camera Orientation Simulation Design**
        *   Objective: Design a simple simulation for the camera orientation update PoC **using Python**.
        *   Actions:
            *   [ ] Define camera locations/initial orientation based on sample data or hypothetical placement.
            *   [ ] Gather or define representative camera specifications (e.g., FOV angles from URL/docs).
            *   [ ] Define the simulated data format (e.g., JSON payload with camera ID, timestamp, new orientation [e.g., quaternion or Euler angles]).
            *   [ ] Choose a simulation method (e.g., **Python script/module** generating orientation changes over time, publishing to MQTT/endpoint).
            *   [ ] Specify the target camera(s) within the sample UDT scene.
    23. [ ] **Task: Proof of Concept (Dynamic Camera FOV Visualization)**
        *   Objective: Implement the PoC to dynamically update a camera's FOV visualization in Omniverse based on simulated orientation data **using Kit SDK scripting and Python simulation module**.
        *   Actions:
            *   [ ] Develop the **Python simulation module** (from Task 21) to generate/stream orientation data.
            *   [ ] Add camera representations (`UsdGeomCamera`) to the USD scene.
            *   [ ] Develop an Omniverse script/extension **using Kit SDK APIs** that:
                *   [ ] Connects to the simulated orientation data source.
                *   [ ] Identifies the target `UsdGeomCamera` prim and associated FOV visualization prim.
                *   [ ] Updates the camera prim's orientation based on incoming data.
                *   [ ] Updates the transform/shape of the FOV visualization prim.
            *   [ ] Run the simulation and the Omniverse application together.
            *   [ ] Verify that the camera's FOV visualization updates dynamically.
            *   [ ] Document the implementation approach, code snippets, and challenges.
*   **Flood Simulation (SimScale Integration):**
    24. [ ] **Task: SimScale & Omniverse Extension Research**
        *   Objective: Understand the capabilities and workflow of the Omniverse SimScale Converter Extension.
        *   Actions:
            *   [ ] Review SimScale documentation on supported analysis types (especially fluid dynamics relevant to flooding).
            *   [ ] Review documentation for the Omniverse SimScale Converter Extension (installation, workflow, data requirements, limitations).
            *   [ ] Assess SimScale account requirements/access options.
    25. [ ] **Task: Prepare Data for Flood Simulation PoC**
        *   Objective: Prepare a simplified dataset suitable for a basic flood simulation PoC **using procedurally generated (simplified) buildings where appropriate**. 
        *   Actions:
            *   [ ] Acquire or generate sample terrain data (DEM) for a small area of interest.
            *   [ ] Process terrain data into a format suitable for import via the Omniverse extension (likely needs to be part of the USD scene) **(use Python modules)**.
            *   [ ] Select/generate simplified building models (USD prims using procedural approach) within the area of interest **(potentially using Python USD bindings in modules)**.
            *   [ ] Define basic simulation parameters (e.g., inflow boundary, water level).
    26. [ ] **Task: Proof of Concept (SimScale Flood Simulation & Visualization)**
        *   Objective: Execute a basic flood simulation using SimScale via the Omniverse extension and visualize results in Omniverse.
        *   Actions:
            *   [ ] Install the Omniverse SimScale Converter Extension.
            *   [ ] Use the extension to send the prepared terrain and building geometry (USD) to SimScale.
            *   [ ] Set up a simple transient flood simulation scenario in SimScale.
            *   [ ] Run the simulation in SimScale.
            *   [ ] Use the extension to import simulation results (e.g., water surface elevation, flow velocity fields) back into the Omniverse USD scene.
            *   [ ] Configure visualization of the results within Omniverse (e.g., coloring surfaces by water depth, displaying flow vectors).
            *   [ ] Document the workflow, results, challenges, and performance.
*   **Advanced Collaboration (Placeholder):**
    27. [ ] **Task: Explore Advanced Collaboration Scenarios**
        *   Objective: Investigate more complex collaborative workflows enabled by USD and Nucleus.
        *   Actions:
            *   [ ] Research handling conflicting edits in USD layers.
            *   [ ] Explore branching/merging concepts if supported by Nucleus or external tools.
            *   [ ] Investigate permission models for controlling access to different parts of the UDT scene/layers.
            *   [ ] Document potential advanced collaboration patterns relevant to the organization. 