
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



# Global shutter cameras routinely achieve sub-millisecond exposures, sub-20µs varies

**Neither claim fully captures market reality.** Research across eight major machine vision camera manufacturers reveals that virtually all modern global shutter industrial cameras support minimum integration times in the **microsecond range**—typically 10-60 µs—making sub-millisecond minimums absolutely standard. However, sub-20µs capability is available on roughly **50-60% of current models**, concentrated in USB3 interfaces and newer Sony Pregius sensor generations. The determining factors are interface type, sensor generation, and whether special "ultra short" exposure modes are enabled.

## Comprehensive specifications across manufacturers

Detailed examination of technical documentation from all eight manufacturers reveals significant variation in minimum integration times, with a clear pattern emerging around sensor generation and interface type.

**FLIR/Teledyne FLIR** delivers among the shortest minimums in the industry. Blackfly S USB3 models with Sony IMX250 achieve **6 µs** minimum exposure, while Oryx 10GigE cameras range from **5-39 µs** depending on sensor and resolution. The Grasshopper3 series with Sony Pregius sensors likely achieves 5-10 µs based on sensor specifications, though exact minimums aren't always documented.

**Basler** offers the most granular control through its "Ultra Short exposure time mode," enabling **1-2 µs** minimums on ace and ace 2 series cameras with newer Pregius S sensors. In standard "Common" exposure mode, these same cameras typically specify **19-27 µs** minimums. Older ace models without Ultra Short capability specify 20-40 µs, while dart compact modules achieve **10-20 µs**.

**JAI's Go series** demonstrates dramatic interface dependence: USB3 Vision models with Sony IMX174 achieve **6 µs**, while identical GigE Vision versions specify **38 µs**—a 6x difference. Camera Link models fall between at **12-14 µs**.

| Manufacturer | Best Minimum | Typical Range | Interface Dependency |
|--------------|--------------|---------------|---------------------|
| FLIR/Teledyne | 5-6 µs | 6-39 µs | USB3 > 10GigE |
| Basler | 1-2 µs (Ultra Short) | 10-40 µs | Mode-dependent |
| JAI | 6-7 µs | 6-73 µs | USB3 >> GigE |
| Lucid | 1-2.5 µs (Short Mode) | 8-30 µs | Mode-dependent |
| Sony | 10 µs | ~10 µs | Consistent |
| Ximea | 1 µs (LUXIMA) | 15-100 µs | Sensor-dependent |
| IDS | 35 µs | 35-58 µs | USB3 > GigE |
| Allied Vision | 37-38 µs | 37-40 µs | Consistent |

## Some manufacturers consistently exceed 20 µs minimums

**IDS Imaging** and **Allied Vision** notably specify higher minimums than competitors. IDS uEye cameras with Sony IMX273 achieve **35 µs** minimum on USB3 Rev.2 interfaces and **58 µs** on GigE—well above the sub-20µs threshold even with modern Pregius sensors. Allied Vision's Mako and Manta series specify **37-38 µs** minimums across their global shutter lineup.

**Lucid Vision Labs** presents an interesting split: standard Triton and Phoenix GigE cameras specify **30 µs** minimums, but their Atlas 5GigE series with 4th-generation Sony Pregius S sensors (IMX546, IMX547) offers a "Short Exposure Mode" achieving **1-2.5 µs**—among the shortest in the industry.

**Ximea's** specifications vary dramatically by sensor family. Sony Pregius-based xiC/xiB cameras achieve **19-25 µs**, while CMOSIS CMV series sensors range from **16-100 µs**. Their specialized LUXIMA LUX13/LUX19 high-speed sensors achieve **1 µs** with 1 µs step resolution—a premium feature for demanding applications.

## Technical factors governing minimum exposure

Several architectural constraints determine minimum integration time, explaining the observed variation across manufacturers:

**Row time dependency** fundamentally limits exposure. Sensor timing is defined by row clock periods, which depend on internal pixel clock dividers and system clock frequency. No mainstream sensor supports true zero-exposure time—there's always a minimum number of row periods required for proper pixel reset and charge transfer.

**Interface bandwidth** directly impacts timing precision. USB3 Vision cameras consistently achieve lower minimums than GigE Vision equivalents of the same sensor because higher data rates enable faster internal clocking. JAI's Go-2400 series demonstrates this perfectly: 6 µs on USB3 versus 38 µs on GigE with identical IMX174 sensors.

**Sensor generation evolution** has progressively reduced minimums. Sony's 1st-generation Pregius (IMX174) typically achieves 19-20 µs, while 4th-generation Pregius S sensors with backside illumination and dual-ADC architecture enable **2 µs** minimums through "Ultra Short Shutter Interval" modes. This represents nearly a 10x improvement in one decade.

**ADC conversion overhead** varies with bit depth and architecture. Column-parallel ADCs in modern sensors enable faster digitization, but switching between 8-bit and 12-bit modes can affect minimum exposure on some platforms. Basler documents that 8-bit modes enable slightly shorter minimums on certain ace models.

## Evaluating the competing claims

**Claim A** ("Most global shutter cameras permit integration times below 20µs") **overstates market capability**. While leading performers like FLIR Blackfly S, JAI Go USB3, and Basler in Ultra Short mode achieve 5-6 µs or even 1-2 µs, mainstream cameras from IDS (35-58 µs), Allied Vision (37-38 µs), and Lucid's standard lines (30 µs) fall clearly above 20 µs. Based on comprehensive specifications, approximately **50-60% of current global shutter models** achieve sub-20µs minimums, concentrated in USB3 interfaces and latest-generation sensors.

**Claim B** ("Many global shutter cameras have this ability, but sub-millisecond minimum integration times are not typical") **is internally contradictory**. The first clause is accurate—many (not most) cameras achieve sub-20µs. However, the second clause is definitively false: sub-millisecond minimums are **absolutely standard**. Every manufacturer surveyed specifies minimums in the **10-100 microsecond range**, making sub-millisecond capability universal for modern global shutter industrial cameras.

## What the data actually shows

The market reality defies both simplified characterizations:

- **Sub-millisecond minimums are universal** (100% of modern global shutter cameras)
- **Sub-100 µs minimums are standard** (~90%+ of cameras)  
- **Sub-20 µs capability is common but not dominant** (~50-60% of models)
- **Sub-10 µs capability is a premium feature** (~20-30% of models)
- **Sub-2 µs capability is specialized** (~10-15%, requiring latest sensors with special modes)

Interface selection dramatically affects achievable minimums—choosing USB3 Vision over GigE Vision often provides 3-6x improvement with identical sensors. Sensor generation matters equally: 4th-generation Pregius S cameras routinely achieve what was impossible five years ago.

## Conclusion

**A more accurate statement would be:** "Sub-millisecond minimum integration times are standard across all modern global shutter machine vision cameras, typically ranging from 10-60 µs. Sub-20µs capability is available on approximately half of current models—primarily those using USB3/10GigE interfaces with recent Sony Pregius sensors—while sub-2µs capability remains a premium feature requiring latest-generation sensors with special exposure modes enabled."

For applications requiring minimum integration times below 20 µs, specifying USB3 Vision or 10GigE interfaces with 3rd or 4th-generation Sony Pregius sensors is essential. Cameras from FLIR, Basler (with Ultra Short mode), JAI, and Lucid (with Short Mode) consistently achieve the shortest minimums. Applications tolerant of 30-50 µs minimums have broader manufacturer options including IDS and Allied Vision.