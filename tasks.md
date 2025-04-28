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

## Phase 1: Static Data Integration & Visualization Setup

1.  [ ] **Task: Data Assessment & Inventory**
    *   Objective: Fully understand the structure, attributes, coordinate systems, and quality of the existing 3D building GeoPackage and Cesium tiles datasets. Assess suitability for conversion to GeoParquet 1.1 and/or direct import (GeoPackage, glTF) into CityEngine.
    *   Actions:
        *   [ ] Gather sample datasets for both formats.
        *   [ ] Use tools (e.g., QGIS, GDAL/OGR, Cesium Inspector, CityEngine) to inspect file structure, metadata, CRS, and attribute tables.
        *   [ ] Analyze 3D geometry representation (e.g., MultiSurface, PolyhedralSurface) in GeoPackage.
        *   [ ] Specifically examine Cesium Tiles: Identify version (e.g., 1.0 with b3dm, 1.1 with direct glTF). If b3dm, investigate tools/methods for extracting embedded glTF 2.0 content. Evaluate the structure and content (geometry, materials) of the extracted/direct glTF.
        *   [ ] Evaluate how data maps to potential GeoParquet representations or CityEngine import capabilities (GeoPackage import, glTF drag-and-drop).
        *   [ ] Document findings, noting suitability for different pipelines (GeoParquet, CityEngine import) and potential issues (CRS, attributes, materials, geometry complexity).
        *   [ ] **Identify potential terrain data sources**.
        *   [ ] **Identify potential camera location data sources**.

2.  [ ] **Task: Conversion & Pipeline Tool Evaluation**
    *   Objective: Identify and evaluate potential tools/libraries for the different pipeline stages: Source -> Intermediate (GeoParquet/glTF/CityEngine), Intermediate -> USD, and Delta Lake management.
    *   Actions:
        *   [ ] Research tools for Source -> GeoParquet: GDAL, GeoPandas, custom Python scripts, FME.
        *   [ ] Research tools for Cesium Tiles -> glTF: Libraries or scripts for extracting glTF from b3dm (if applicable).
        *   [ ] Research tools for Source/glTF -> CityEngine: CityEngine's importers (verify GeoPackage 3D, test glTF import fidelity). [https://doc.arcgis.com/en/cityengine/latest/help/help-import-gltf.htm](https://doc.arcgis.com/en/cityengine/latest/help/help-import-gltf.htm)
        *   [ ] Research tools for GeoParquet/CityEngine/glTF -> USD: CityEngine USD Exporter, CityEngine Omniverse Connector, Custom Python scripts (USD bindings reading GeoParquet/glTF), Omniverse connectors.
        *   [ ] Investigate Delta Lake interaction methods: Databricks notebooks, `delta-rs` library. [https://delta-io.github.io/delta-rs/](https://delta-io.github.io/delta-rs/)
        *   [ ] Evaluate based on: Format support, attribute/material handling, CRS reprojection, performance, ease of use, licensing, platform compatibility.
        *   [ ] Document pros and cons of top candidates for each potential pipeline step.

3.  [ ] **Task: FSDH (Databricks) Setup & Familiarization**
    *   Objective: Gain access to and basic proficiency with the Federal Science DataHub (FSDH) Azure Databricks environment.
    *   Actions:
        *   [ ] Obtain necessary credentials/access for FSDH. [https://poc.fsdh-dhsf.science.cloud-nuage.canada.ca/](https://poc.fsdh-dhsf.science.cloud-nuage.canada.ca/)
        *   [ ] Explore the Databricks workspace interface.
        *   [ ] Run basic Spark SQL / PySpark commands.
        *   [ ] Learn how to mount/access storage.
        *   [ ] Test creating a simple Delta Lake table.
        *   [ ] Document setup, access procedures, and initial findings.
        *   [ ] **Test basic Delta operations**.

4.  [ ] **Task: Delta-rs & MinIO Setup & Familiarization**
    *   Objective: Set up `delta-rs` and test basic Delta Lake operations against an internal MinIO object storage instance.
    *   Actions:
        *   [ ] Ensure MinIO instance is accessible.
        *   [ ] Install `delta-rs` Python library.
        *   [ ] Configure `delta-rs` to connect to MinIO.
        *   [ ] Test writing a simple DataFrame to a Delta table on MinIO.
        *   [ ] Test reading the Delta table back.
        *   [ ] Document setup, configuration for MinIO, and basic read/write operations.

5.  [ ] **Task: CityEngine Setup & Familiarization**
    *   Objective: Install ArcGIS CityEngine, confirm licensing, and gain basic proficiency in data import (GeoPackage, glTF), USD export/connection, and vegetation placement.
    *   Actions:
        *   ✋ Verify CityEngine license availability and terms. <b><font color="red">Waiting for free trial license.</font></b>
        *   [ ] Install CityEngine and the Omniverse Connector. [https://chr11115.github.io/cityengine/omniverse](https://chr11115.github.io/cityengine/omniverse)
        *   [ ] Follow introductory tutorials (interface, scene setup, data import).
        *   [ ] Test importing sample 3D GeoPackage data.
        *   [ ] Test importing sample extracted/direct glTF 2.0 files (assess fidelity, material handling).
        *   [ ] Test exporting a simple scene to USD.
        *   [ ] Explore the ESRI.lib Vegetation library: Browse available models, understand placement methods (e.g., point-based, procedural rules using CGA).
        *   [ ] Test placing sample vegetation using points or simple rules.
        *   [ ] Document setup, licensing, import/export findings, and initial vegetation library assessment.

6.  [ ] **Task: Nvidia Omniverse, Nucleus, & Kit SDK Setup & Familiarization**
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

7.  [ ] **Task: Proof of Concept (Pipeline Option 1: GeoParquet/Delta)**
    *   Objective: Test the Source -> GeoParquet -> Delta -> USD -> Omniverse pipeline end-to-end with a small sample.
    *   Actions:
        *   [ ] Convert sample source data (e.g., single building/tile) to GeoParquet 1.1.
        *   [ ] Write GeoParquet to Delta Lake (Databricks or MinIO/delta-rs).
        *   [ ] Develop/run script to read from Delta and convert GeoParquet to USD.
        *   [ ] Import the resulting USD into Omniverse.
        *   [ ] Validate data integrity and visual fidelity at each step.
        *   [ ] Document process, results, and any challenges.
        *   [ ] **Noting USD structure for layering**.

8.  [ ] **Task: Proof of Concept (Pipeline Option 2: CityEngine Direct Import/Export)**
    *   Objective: Test the Source (GeoPackage or Cesium/glTF) -> CityEngine -> Export USD -> Omniverse pipeline end-to-end with a small sample.
    *   Actions:
        *   [ ] Import sample source data (GeoPackage or extracted/direct glTF) into CityEngine.
        *   [ ] Export the model/scene from CityEngine as a USD file.
        *   [ ] Import the resulting USD into Omniverse.
        *   [ ] Validate data integrity and visual fidelity (including materials from glTF if applicable).
        *   [ ] Document process, results, and any challenges (import fidelity, material translation).
        *   [ ] **Noting USD structure for layering**.

9.  [ ] **Task: Proof of Concept (Pipeline Option 3: CityEngine Connector)**
    *   Objective: Test the Source (GeoPackage or Cesium/glTF) -> CityEngine -> Omniverse Connector -> Nucleus -> Omniverse pipeline end-to-end with a small sample.
    *   Actions:
        *   [ ] Import sample source data (GeoPackage or extracted/direct glTF) into CityEngine.
        *   [ ] Use the Omniverse Connector to send the model/scene to Nucleus.
        *   [ ] Open/Import the scene from Nucleus within Omniverse.
        *   [ ] Validate data integrity and visual fidelity.
        *   [ ] Test the update mechanism.
        *   [ ] Document process, results, connector performance, and any challenges.
        *   [ ] **Noting USD structure for layering**.

## Phase 2: Enrichment, Procedural Content, & Basic Collaboration

10. [ ] **Task: Phase 2 Vegetation Workflow Definition**
    *   Objective: Define and test the specific workflow for integrating 3D trees using CityEngine's vegetation library.
    *   Actions:
        *   [ ] Identify/acquire sample point data sources for tree locations.
        *   [ ] Define required attributes for tree points (e.g., species identifier, height).
        *   [ ] Develop/select basic CGA rules or utilize CityEngine tools for placing vegetation from ESRI.lib based on input points/attributes.
        *   [ ] Test exporting/connecting the combined building+vegetation scene to Omniverse.
        *   [ ] Document the chosen workflow and steps for larger-scale implementation.

11. [ ] **Task: Roofer Evaluation**
    *   Objective: Evaluate Roofer for 3D building generation and its integration potential.
    *   Actions:
        *   [ ] Review Roofer documentation.
        *   [ ] Investigate CityJSON -> GeoParquet/CityEngine import workflows.
        *   [ ] Install Roofer and run test reconstruction on sample data.
        *   [ ] Compare results (quality, performance) with Geoflow3D.
        *   [ ] Assess output compatibility with planned pipelines (GeoParquet/Delta or CityEngine).
        *   [ ] Document findings and recommendations.

12. [ ] **Task: Initial Interactive Feature Exploration (Omniverse)**
    *   Objective: Explore basic interactive capabilities within Omniverse using the integrated UDT data.
    *   Actions:
        *   [ ] Research Omniverse APIs/tools for object selection and attribute querying (e.g., using Python scripting, UI elements).
        *   [ ] Implement a simple PoC for selecting a building and displaying its attributes (read from USD or linked data).
        *   [ ] Document potential methods for more advanced UDT interactions.

13. [ ] **Task: USD Layering/Referencing Strategy Definition**
    *   Objective: Define a strategy for structuring the UDT scene using USD layers and references to facilitate non-destructive updates and collaboration.
    *   Actions:
        *   [ ] Research USD best practices for large scene composition (layering, referencing, variants, payloads).
        *   [ ] Propose a layering structure (e.g., base terrain layer, regional building layers, update layers, vegetation layers, simulation layers).
        *   [ ] Define how building updates (add/replace) will be managed within this structure (e.g., overriding prims in session layers, referencing updated asset files in dedicated layers).
        *   [ ] Consider how different data sources/pipelines feed into this structure.
        *   [ ] Document the proposed composition strategy.

14. [ ] **Task: Proof of Concept (Non-Destructive Building Update)**
    *   Objective: Demonstrate adding or replacing building data in the UDT scene using the defined USD composition strategy and Nucleus.
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
    15. [ ] **Task: Real-Time Integration Research (Omniverse/USD/Kit)**
        *   Objective: Understand how real-time data can be connected to and visualized within an Omniverse USD scene **using Kit SDK**.
        *   Actions:
            *   [ ] Research Omniverse Kit SDK documentation specifically for real-time data pipelines, event handling, UI creation, and Python scripting for dynamic updates.
            *   [ ] Investigate how external data streams (e.g., MQTT, WebSockets, APIs) can be ingested by a Kit-based application/extension.
            *   [ ] Explore USD schema possibilities for linking assets to external data or storing dynamic attributes.
            *   [ ] Research methods for modifying material properties (e.g., color) or other visual aspects of USD prims dynamically using Kit APIs.
    16. [ ] **Task: IoT Sensor Simulation Design**
        *   Objective: Design a simple simulation for the IoT temperature sensor PoC.
        *   Actions:
            *   [ ] Define the simulated data format (e.g., JSON payload with building ID, sensor location ID, timestamp, temperature value).
            *   [ ] Choose a simulation method (e.g., simple Python script generating data, potentially publishing to MQTT or a basic web endpoint).
            *   [ ] Define the target building and sensor location within the sample UDT scene.
            *   [ ] Specify the desired visualization logic (e.g., map temperature range to a color gradient [blue -> white -> red]).
    17. [ ] **Task: Proof of Concept (Real-Time Temperature Visualization Update)**
        *   Objective: Implement the PoC to dynamically update a building's color in Omniverse based on simulated sensor data **using Kit SDK scripting**.
        *   Actions:
            *   [ ] Develop the simulation script (from Task 16) to generate/stream temperature data.
            *   [ ] Develop an Omniverse script/extension **using Kit SDK APIs** that:
                *   [ ] Connects to the simulated data source.
                *   [ ] Identifies the target building prim in the USD stage.
                *   [ ] Applies the visualization logic: Reads temperature, calculates color, modifies the building's displayColor material attribute (or similar mechanism).
            *   [ ] Run the simulation and the Omniverse application together.
            *   [ ] Verify that the building color changes dynamically in response to simulated temperature fluctuations.
            *   [ ] Document the implementation approach, code snippets, and challenges encountered.
*   **Camera FOV Simulation:**
    18. [ ] **Task: Research USD Camera Representation & FOV Visualization**
        *   Objective: Understand how to represent cameras and their FOVs within a USD scene for dynamic updates **using Kit SDK**.
        *   Actions:
            *   [ ] Investigate `UsdGeomCamera` schema.
            *   [ ] Explore methods for visualizing the FOV dynamically.
            *   [ ] Research how to efficiently update camera orientation and the corresponding FOV visualization primitive via **Omniverse Kit SDK scripting**.
            *   [ ] Review examples or best practices.
    19. [ ] **Task: Camera Orientation Simulation Design**
        *   Objective: Design a simple simulation for the camera orientation update PoC.
        *   Actions:
            *   [ ] Define camera locations/initial orientation based on sample data or hypothetical placement.
            *   [ ] Gather or define representative camera specifications (e.g., FOV angles from URL/docs).
            *   [ ] Define the simulated data format (e.g., JSON payload with camera ID, timestamp, new orientation [e.g., quaternion or Euler angles]).
            *   [ ] Choose a simulation method (e.g., Python script generating orientation changes over time, publishing to MQTT/endpoint).
            *   [ ] Specify the target camera(s) within the sample UDT scene.
    20. [ ] **Task: Proof of Concept (Dynamic Camera FOV Visualization)**
        *   Objective: Implement the PoC to dynamically update a camera's FOV visualization in Omniverse based on simulated orientation data **using Kit SDK scripting**.
        *   Actions:
            *   [ ] Develop the simulation script (from Task 19) to generate/stream orientation data.
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
    21. [ ] **Task: SimScale & Omniverse Extension Research**
        *   Objective: Understand the capabilities and workflow of the Omniverse SimScale Converter Extension.
        *   Actions:
            *   [ ] Review SimScale documentation on supported analysis types (especially fluid dynamics relevant to flooding).
            *   [ ] Review documentation for the Omniverse SimScale Converter Extension (installation, workflow, data requirements, limitations).
            *   [ ] Assess SimScale account requirements/access options.
    22. [ ] **Task: Prepare Data for Flood Simulation PoC**
        *   Objective: Prepare a simplified dataset suitable for a basic flood simulation PoC.
        *   Actions:
            *   [ ] Acquire or generate sample terrain data (DEM) for a small area of interest.
            *   [ ] Process terrain data into a format suitable for import via the Omniverse extension (likely needs to be part of the USD scene).
            *   [ ] Select/simplify building models (USD prims) within the area of interest.
            *   [ ] Define basic simulation parameters (e.g., inflow boundary, water level).
    23. [ ] **Task: Proof of Concept (SimScale Flood Simulation & Visualization)**
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
    24. [ ] **Task: Explore Advanced Collaboration Scenarios**
        *   Objective: Investigate more complex collaborative workflows enabled by USD and Nucleus.
        *   Actions:
            *   [ ] Research handling conflicting edits in USD layers.
            *   [ ] Explore branching/merging concepts if supported by Nucleus or external tools.
            *   [ ] Investigate permission models for controlling access to different parts of the UDT scene/layers.
            *   [ ] Document potential advanced collaboration patterns relevant to the organization. 