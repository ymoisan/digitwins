# Post-Mortem: Digital Twin IoT Architecture Exploration

## Summary

Explored various architectures for integrating IoT sensor data into a Digital Twin Common Operating Picture using modern Rust-backed Python libraries and event-driven patterns. Also evaluated multiple 3D visualization platforms before settling on USD/Omniverse approach.

## Previous Explorations

### **Web-Based 3D Visualization Platforms**

**Cesium** - Struggled with feature alignment issues where 3D buildings and trees floated above ground level. Created [community discussion](https://community.cesium.com/t/floating-features/36942/3) seeking solutions. Found workaround but feels like a hack that shouldn't be necessary for proper terrain alignment.

**iTowns** - Better terrain alignment than Cesium, with buildings properly grounded. Easy localhost deployment was a plus. However, heavily dependent on French OGC web services (mainly WMS) with hardcoded server instances. Created [discussion about PMTiles](https://github.com/iTowns/itowns/discussions/2542) as alternative to OGC services for simpler basemap creation.

**Giro3D/Piero** - Very difficult localhost installation process, never successfully deployed. Once set up might be functional, but initial barrier too high.

### **Assessment of Web Platforms**
- Both iTowns and Giro3D/Piero appeared "France-centric" in design and data sources
- Unclear how these platforms could enable stakeholder interaction needed for true Common Operating Picture
- Limited flexibility for integrating diverse sensor data and supporting various use cases
- Led to decision to explore USD/Omniverse for better stakeholder collaboration tools

## What We Tested

### **Technology Stack Evaluation**
- **obstore** as boto3 replacement for S3/MinIO access
- **Polars** as pandas replacement for DataFrame processing  
- **delta-rs** for DeltaLake operations
- **MQTT 5** features for IoT data ingestion
- **Event-driven serverless** vs permanent service architectures

### **Architecture Patterns**
- MQTT-first vs DeltaLake-first data flow
- Event sourcing with append-only storage
- Object storage events triggering Omniverse updates
- Direct sensor writes to DeltaLake tables

### **Infrastructure Setup**
- Pixi dependency management in corporate environment
- SSL certificate configuration for conda/pip
- MinIO bucket permissions and access policies
- CityEngine USD export workflow (12-hour base scene generation)

## What Worked ✅

- **NRCAN SSL Certificate**: Successfully configured pixi to work with corporate SSL cert
- **Dependency Management**: Pixi resolved all required packages (obstore, polars, deltalake)
- **Architecture Decisions**: Settled on DeltaLake-first approach over complex MQTT infrastructure
- **MinIO Integration**: Designed appropriate bucket policies for sensor access
- **Simplified Planning**: Recognized initial planning was over-engineered
- **Base USD Scene**: CityEngine successfully exported 12-hour USD scene (acceptable for one-time base generation)

## What Didn't Work ❌

### **Over-Engineering**
- Multiple architectural options when simpler approach was better; e.g.  complex Python class hierarchies not needed when using cloud "goodies" (like events on object storage)

### **Technical Issues**
- **SSL Certificate Problems**: Blocked progress until corporate cert was identified
- **obstore API Confusion**: Linter errors and unclear documentation slowed testing
- **Incomplete Testing**: Never completed MinIO bucket connection verification
- **USD Visualization**: Current software setup cannot view/validate the exported USD scene

## Key Learnings

1. **Start Simple**: Don't over-engineer architecture upfront - build minimal working version first
2. **Corporate Environment**: SSL certificates and firewalls require specific handling in enterprise settings  
3. **Event-Driven is Cleaner**: Serverless object storage events better than permanent MQTT infrastructure
4. **Modern Tooling**: Rust-backed Python libraries (obstore, polars, delta-rs) offer significant performance gains
5. **Focus on Objective**: Common Operating Picture goal was clear, but got distracted by implementation details
6. **Base Scene Generation**: Long USD export times (12 hours) acceptable for one-time base infrastructure scenes
