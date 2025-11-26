# Klipper G-Code Macros for Qidi Printers

Optimized G-code macros for Klipper firmware (v1.1.0) designed for Qidi Q2 3D printers with multi-material box support.

## Features

- ⚡ **Optimized print start sequence** - Reduced startup time by 1.5-2.5 minutes
- 🔥 **Early bed heating** - Bed and chamber heating starts immediately, parallel to other operations
- 📦 **Multi-material box support** - Full support for Qidi box system (BOX_PRINT_START, EXTRUSION_AND_FLUSH) (!!! NOT TESTED !!!)
- ✅ **Parameter validation** - Temperature and parameter validation before execution 
- 🎯 **Dual homing option** - Optional second homing after filament cutting for maximum precision

### PRINT_START Macros

1. **PRINT_START_basic.cfg** - Basic version without box support (Recommended)
   - Execution time: ~3.5-4.5 minutes
   - Recommended for: Single material printing

2. **PRINT_START_with_boxes.cfg** - Full version with box support (!!! NOT TESTED !!!)
   - Execution time: ~3.5-5.5 minutes
   - Recommended for: Multi-material printing with box system
   - Includes: BOX_PRINT_START, EXTRUSION_AND_FLUSH, BUFFER_MONITORING

3. **PRINT_START_fast.cfg** - Fast version without box support 
   - Execution time: ~3-4 minutes
   - Single homing (G28) instead of dual
   - Recommended for: When maximum precision is not critical

4. **PRINT_START_with_boxes_fast.cfg** - Fast version with box support (!!! NOT TESTED !!!)
   - Execution time: ~3.5-5 minutes
   - Single homing (G28) instead of dual
   - Recommended for: Multi-material printing when speed is priority

## Installation

1. Open `gcode_macro.cfg` in printer settings. 
2. Replace section `PRINT_START` with content of a desired config.
3. Restart Klipper

#### OR

1. Copy the desired macro file(s) to your Klipper config directory
2. Include the file in your `printer.cfg`:
   ```ini
   [include PRINT_START_with_boxes.cfg]
   ```
4. Ensure `gcode_macro.cfg` does not contains `PRINT_START` macro.
3. Restart Klipper

## Usage

### PRINT_START Parameters

The macro accepts the following parameters:

- `BED` (required) - Bed temperature (0-150°C)
- `HOTEND` (required) - Hotend temperature (0-350°C)
- `CHAMBER` (optional, default: 0) - Chamber temperature (0-100°C)
- `EXTRUDER` (optional, default: 0) - Extruder number for box system

### Example

```gcode
PRINT_START BED=60 HOTEND=220 CHAMBER=50 EXTRUDER=0
```

## Key Optimizations

### Early Bed Heating
- Bed (M140) and chamber (M141) heating starts immediately after parameter validation
- Heating occurs in parallel with:
  - AUTOTUNE_SHAPERS
  - G28 (homing)
  - Nozzle cleaning
  - Filament cutting
  - Calibration operations
- **Time savings: 30-90 seconds** per print

### Optimized Delays
- Reduced unnecessary pauses
- G4 P30000 → G4 P15000 (15 seconds saved)
- G4 P3000 → G4 P2000 (1 second saved)
- Runout loop optimization: G4 P100 → G4 P10 (1.4 seconds saved)

### Parallel Operations
- Bed and chamber heating in parallel
- Initialization operations during heating
- Temperature waiting optimized

## File Structure

```
.
├── README.md
├── PRINT_START_basic.cfg              # Basic version without box support
├── PRINT_START_with_boxes.cfg         # Full version with box support
├── PRINT_START_fast.cfg               # Fast version without box support
├── PRINT_START_with_boxes_fast.cfg    # Fast version with box support

### Box System Not Working
Ensure:
- `box_count >= 1` in save_variables
- `enable_box == 1` in save_variables
- `box_extras` macro is defined

### Homing Issues
- Dual homing (two G28) is intentional for precision after filament cutting
- Use FAST versions if single homing is sufficient

## Contributing

Contributions are welcome! Please ensure:
- Code follows Klipper macro conventions
- Comments are in English
- Performance optimizations are tested
- Backward compatibility is maintained

## License

This project is provided as-is for use with Klipper firmware and Qidi printers.

## Support

For issues and questions, please open an issue in the repository.

---

**Note:** These macros are optimized for Qidi Q2 printers with Klipper firmware 1.1.0. Adjustments may be needed for other printer configurations.
