
# Infrared Camera Exposure Calculations

This worked example walks through the radiometric signal chain starting from an extended IR source produced by the reflection of a photo-epilation OHT laser light on the surface of the skin, to the electron generation at a silicon sensor.

## Geometrical Setup

### 1. main info
- **Light source**: We assume an IR source emitting in the NIR, as example 850nm. For this worked example we assume a total emitted power in the range 10 to 20 W over cm² area on the target. This is the 
- 
- 
- **Irradiance at lens**: 1 W/cm² 
- **Sensor**: BSI silicon sensor with 2.6 µm pixel pitch, 35% QE at 850nm
- **Two lens configurations** to compare

###  Fundamental constants
h = 6.626e-34       # Planck's constant [J·s]
c = 3.0e8           # Speed of light [m/s]
wavelength = 850e-9 # Wavelength [m]

#### Photon energy at 850nm
E_photon = (h * c) / wavelength = Photon energy: 2.339e-19 J (≈ 0.00146 eV)


### 2. Sensor specifications
we assume a global shutter with typical parameters for a modern BSI sensor:
pixel_pitch = 2.6e-6          # Pixel size [m]
pixel_area = pixel_pitch ** 2  # Pixel area [m²]
quantum_efficiency = 0.35      # QE at 850nm [dimensionless]
full_well_capacity = 6000      # Typical for 2.6µm BSI pixel [electrons]

### 3. Lens Parameters

We'll compare two lenses:
- **Lens A**: Standard wide-angle (65° FOV, f/1.8)
- **Lens B**: Fisheye (175° FOV, f/2.35)

We can calculate entrance pupil diameters and light-gathering geometry.

#### Lens Parameters

**Lens A (f=3.6mm, F/1.8):**
$$D_A = \frac{3.6\text{mm}}{1.8} = 2.0\text{mm}$$

**Lens B (f=1.2mm, F/2.35):**
$$D_B = \frac{1.2\text{mm}}{2.35} = 0.51\text{mm}$$



### Step 5.4: Electron Generation Rate

$$\dot{n}_e = QE \times \Phi_{pixel}$$

Units: electrons per second per pixel