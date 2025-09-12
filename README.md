# SIH2025
# CGWB-Compliant Rainwater Harvesting Recommendation System

## Overview

This system provides automated, scientifically-backed recommendations for rainwater harvesting structures based on Central Ground Water Board (CGWB) guidelines. The system integrates multiple geospatial datasets to analyze location-specific conditions and recommend optimal harvesting solutions.

## Key Features

- **Automated Rooftop Area Detection**: Uses satellite imagery analysis to calculate catchment areas
- **Climate-Aware Rainfall Estimation**: Automatically estimates regional rainfall patterns
- **CGWB Compliance**: All recommendations follow official groundwater board standards
- **Multi-tier Recommendations**: Budget, Standard, and Premium system options
- **Soil-Specific Analysis**: Incorporates soil infiltration rates and porosity data
- **Groundwater Assessment**: Analyzes local aquifer conditions and water table depth

## System Architecture

### Data Sources
- **Groundwater Data**: District-level groundwater recharge information (`GWR_District2020.shp`)
- **Aquifer Information**: Major aquifer systems mapping (`Major_Aquifers.shp`)
- **Land Use Classification**: LULC data for urban/rural classification (`LULCDATA.tif`)
- **Soil Properties**: Multi-layer soil type mapping with infiltration characteristics
  - Sandy soil (`fsandy.asc`)
  - Clay soil (`fclayey.asc`)
  - Loamy soil (`floamy.asc`)
  - Clay-skeletal soil (`fclayskeletal.asc`)

### Core Components

#### 1. Rooftop Area Calculation
```python
def calculate_rooftop_area_from_satellite(lat, lon):
    """
    Automatically extracts rooftop area from satellite imagery
    
    Process:
    1. Acquire high-resolution satellite imagery for coordinates
    2. Apply building footprint detection algorithms
    3. Calculate total rooftop catchment area
    4. Adjust for roof slope and material efficiency
    
    Returns: Effective catchment area in m²
    """
```

**Implementation Notes:**
- Uses computer vision techniques on satellite imagery
- Applies building segmentation models
- Accounts for roof material and slope in catchment calculations
- Provides confidence intervals for area estimates

#### 2. Automated Rainfall Estimation
```python
def estimate_regional_rainfall(lat, lon):
    """
    Estimates annual and seasonal rainfall patterns
    
   
    
    Enhanced Features:
    - Historical rainfall data analysis
    - Seasonal distribution patterns
    - Climate change projections
    """
```

## Installation & Setup

### Prerequisites
```bash
pip install geopandas shapely rasterio pyproj scipy numpy
```

### Data Preparation
1. Ensure all shapefile components (.shp, .shx, .dbf, .prj) are present
2. Verify coordinate reference systems are properly defined
3. Run soil data processing to generate combined soil map:
```python
# Soil processing is automated in the system
combined_soil_map = process_soil_layers()
```

## Usage

### Basic Recommendation Generation
```python
# Automated analysis with satellite-derived rooftop area
recommendation = recommend_recharge_structure_cgwb(
    lat=26.1445,  # Latitude
    lon=91.7362,  # Longitude (Guwahati coordinates)
    roof_area="auto_calculate",     # System calculates from satellite
    open_space_area=50,             # m² of available open space
    num_dwellers=4,                 # Household size
    budget_range="medium",          # "low", "medium", "high"
    usage_intent="household",       # "drinking", "household", "irrigation"
    annual_rainfall="auto_estimate" # System estimates from coordinates
)
```

### Runoff Calculation
```python
runoff_data = calculate_runoff(
    lat=26.1445, 
    lon=91.7362,
    roof_type="tilted",           # "flat", "tilted", "sloping"
    roof_area="auto_calculate",   # Calculated from satellite imagery
    rainfall_intensity=50,        # mm/hr (or auto-estimated)
    rainfall_duration=2           # hours
)
```

## Output Structure

### Location Analysis
```json
{
  "location_analysis": {
    "coordinates": {"latitude": 26.1445, "longitude": 91.7362},
    "land_use": "Rural",
    "soil_type": "Loamy soil",
    "soil_infiltration_rate": "10.0 mm/hr",
    "groundwater_depth": "10 meters",
    "aquifer_type": "Local aquifer",
    "annual_rainfall": "2200 mm"
  }
}
```

### Water Balance Assessment
```json
{
  "water_balance": {
    "household_size": 4,
    "daily_water_need": "600 liters",
    "annual_water_demand": "219,000 liters",
    "roof_catchment": "120 m²",
    "annual_runoff_potential": "224,400 liters",
    "surplus_deficit_ratio": 0.025,
    "collection_months": "6-7 months (June-December)"
  }
}
```

### CGWB-Compliant Recommendations

#### Storage Systems
- **Budget Option**: Basic HDPE tank systems (₹25,000-₹40,000)
- **Standard Option**: Ferrocement systems (₹50,000-₹75,000)
- **Premium Option**: Smart stainless steel systems (₹90,000-₹1,20,000)

#### Recharge Systems
- **Budget Option**: Basic recharge pits (₹15,000 per pit)
- **Standard Option**: Engineered recharge pits (₹25,000 per pit)
- **Premium Option**: Smart recharge networks (₹40,000 per pit)

#### Hybrid Systems
- Integrated storage-recharge combinations
- AI-optimized water distribution
- Seasonal operation strategies

## Satellite Integration Features

### Rooftop Detection Pipeline
1. **Image Acquisition**: High-resolution satellite imagery retrieval
2. **Building Segmentation**: AI-powered building footprint extraction
3. **Roof Classification**: Material and slope analysis
4. **Area Calculation**: Precise catchment area computation
5. **Validation**: Ground-truth verification where possible

### Rainfall Integration
1. **Coordinate-Based Estimation**: Regional climate data analysis
2. **Historical Patterns**: Multi-year rainfall trend analysis
3. **Seasonal Distribution**: Monthly precipitation patterns
4. **Future Projections**: Climate change impact assessment

## Compliance & Standards

### CGWB Requirements
-  First-flush diverter mandatory
-  ISI marked materials for permanent installations
-  Proper filtration systems for storage
-  Overflow management systems
-  Regular maintenance protocols

### Regulatory Compliance
- **Urban Areas**: NOC from groundwater authority required
- **All Areas**: Building plan approval for new constructions
- **Recharge Systems**: Soil percolation testing mandatory



## Maintenance Schedule

### Monthly Tasks
- Visual inspection of gutters and downpipes
- First-flush diverter operation check
- Tank/pit cover damage assessment

### Annual Maintenance
- **Cost**: ₹5,000-₹15,000 for complete service
- Professional system cleaning and calibration
- Component replacement as needed

## Technical Specifications

### Soil Properties Integration
```python
soil_properties = {
    1: {"porosity": 0.40, "infiltration": 10.0},    # Sandy soil
    2: {"porosity": 0.55, "infiltration": 0.5},     # Clay soil
    3: {"porosity": 0.45, "infiltration": 10.0},    # Loamy soil
    4: {"porosity": 0.50, "infiltration": 2.0}      # Clay-skeletal soil
}
```

### Runoff Coefficient Calculation
- **Base Coefficients**: Flat (0.85), Tilted (0.90), Sloping (0.95)
- **Urban Adjustment**: +10% efficiency
- **Soil Infiltration Factor**: High infiltration reduces coefficient
- **Final Range**: 0.70-1.00 (automatically clipped)

## Error Handling & Fallbacks

### Data Availability Issues
- **Missing Soil Data**: Defaults to Loamy soil properties
- **No Groundwater Info**: Uses regional average depth (10m)
- **LULC Data Unavailable**: Defaults to Rural classification
- **Aquifer Data Missing**: Assumes local aquifer system

### Satellite Integration Fallbacks
- **Cloud Cover**: Uses most recent clear imagery
- **Resolution Limitations**: Provides confidence intervals
- **Processing Errors**: Falls back to manual input option

## Future Enhancements

### Planned Features
1. **Real-time Weather Integration**: Live rainfall data incorporation
2. **IoT Sensor Integration**: Automated monitoring capabilities
3. **Mobile App Interface**: Field data collection and monitoring
4. **Machine Learning Optimization**: Predictive maintenance scheduling

### Research Integrations
- Climate change impact modeling
- Groundwater depletion trend analysis
- Community-scale water balance optimization

## Support & Documentation

### Technical Support
- System logs available for debugging
- Comprehensive error messaging
- Validation checks for all inputs

### Scientific Validation
- All recommendations based on peer-reviewed research
- CGWB technical manual compliance
- Regional calibration with field studies

---

**Note**: This system is designed for the Indian subcontinent, with specific calibration for the Assam region. For deployment in other regions, recalibration of rainfall patterns and soil property databases may be required.
