# 🌞 SDO Real-Time Data Fetcher

A lightweight Python tool for fetching real-time Solar Dynamics Observatory (SDO) data, designed to complement the Surya foundation model's data pipeline.

## Overview

While Surya uses preprocessed SDO data from HuggingFace, this tool enables researchers to:
- Fetch the **latest real-time** SDO observations (updated every 12 seconds)
- Download specific wavelengths for custom analysis
- Monitor solar activity in real-time
- Create custom training datasets from recent observations

## Why This Tool?

The Surya foundation model is trained on SDO data spanning 2011-2019. This tool bridges the gap by providing:
- **Real-time observations** for current solar activity monitoring
- **Quick data exploration** without downloading large preprocessed datasets
- **Custom data collection** for fine-tuning on recent events
- **Educational purposes** for understanding SDO instruments

## 🚀 Quick Start

### Installation

```bash
cd tools/sdo_data_fetcher
pip install -r requirements.txt
```

### Basic Usage

```bash
# Fetch latest AIA 171Å image (most common for Surya)
python sdo_fetcher_v2.py --source AIA_171

# Fetch multiple wavelengths used in Surya training
python sdo_fetcher_v2.py --multiple

# List all available SDO sources
python sdo_fetcher_v2.py --list
```

### Integration with Surya

```python
from sdo_fetcher_v2 import SDOFetcher

# Fetch the 8 AIA channels used in Surya
aia_channels = ["AIA_94", "AIA_131", "AIA_171", "AIA_193", 
                "AIA_211", "AIA_304", "AIA_335", "AIA_1600"]

fetcher = SDOFetcher(output_dir="surya_inference_data")
results = fetcher.download_multiple(aia_channels)

# Images are now ready for preprocessing and Surya inference
```

## 📡 Available Data Sources

The tool provides access to all SDO/AIA and HMI channels:

### AIA Channels (used in Surya)
- **AIA 94, 131, 171, 193, 211, 304, 335, 1600** - The 8 AIA channels Surya was trained on
- **AIA 1700** - Additional AIA channel

### HMI Channels (used in Surya)
- **HMI Magnetogram** - Magnetic field measurements (5 channels in Surya model)
- **HMI Continuum** - Visible light solar surface

## 🔬 Use Cases with Surya

### 1. Real-Time Solar Activity Monitoring
```bash
# Monitor current solar activity with Surya's primary wavelengths
python sdo_advanced_examples.py  # Select option 3: Space Weather Check
```

### 2. Custom Inference on Latest Data
```python
# Fetch latest multi-channel data
fetcher = SDOFetcher(output_dir="latest_obs")
fetcher.download_multiple(["AIA_171", "AIA_193", "AIA_211", "HMI_Magnetogram"])

# Preprocess for Surya (user would add their preprocessing pipeline)
# Run Surya inference on current solar conditions
```

### 3. Fine-tuning Dataset Creation
```bash
# Continuous monitoring to build recent event datasets
python sdo_advanced_examples.py  # Select option 5: Continuous monitoring
```

### 4. Validation Data Collection
- Fetch observations from specific dates for model validation
- Compare Surya forecasts with actual observations
- Track model performance on recent solar events

## 📊 Output Format

Each download produces:
- **JPG image** (1024×1024) - Ready for quick visualization
- **JSON metadata** - Observation timestamp and source info

For Surya integration, images can be:
1. Loaded and preprocessed to match Surya's input format (4096×4096, normalized)
2. Aligned temporally for multi-channel input
3. Used for inference or fine-tuning

## 🎯 Advanced Features

The `sdo_advanced_examples.py` script includes:
- Multi-wavelength comparison sets
- Active region monitoring (useful for flare forecasting tasks)
- Space weather assessment
- Continuous monitoring for time-series data collection

## 🔄 Data Pipeline Integration

```
Real-Time SDO          This Tool           Preprocessing       Surya Model
Observations    →    sdo_fetcher_v2.py  →  (user's code)  →    Inference
(12s cadence)        (download JPGs)       (resize, align)     (forecasting)
```

## 📖 Documentation

- **Surya Model**: See main repository README for model architecture and capabilities
- **SDO Mission**: [https://sdo.gsfc.nasa.gov/](https://sdo.gsfc.nasa.gov/)
- **Data Specs**: Images at 1024×1024 (can be upsampled to Surya's 4096×4096)

## 🤝 Contributing

This tool is designed to be lightweight and focused on data acquisition. For preprocessing pipelines specific to Surya, please contribute to the main model repository.

## ⚖️ License

MIT License - Free for research and educational purposes.

## 🌟 Acknowledgments

- **NASA/SDO** for open solar data
- **NASA-IMPACT Surya Team** for the foundation model
- Data sourced from NASA's SDO mission servers

---

**Note**: This tool fetches 1024×1024 images for quick access. For full-resolution 4096×4096 SDO data as used in Surya training, refer to the HuggingFace datasets in the main repository.
