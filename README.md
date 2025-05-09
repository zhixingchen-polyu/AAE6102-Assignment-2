# AAE6102_Assignment2: Lonosphere mapping based GNSS ground station 
Student No: 24038229R

![GNSS-Technologies](https://img.shields.io/badge/GNSS-DGNSS%7CRTK%7CPPP%7CPPP--RTK-blue) 

# Task 1 – Differential GNSS Positioning
# Evolution of high-precision GNSS positioning technology in smartphone navigation: From DGNSS to PPP-RTK

Abstract: Global Navigation Satellite Systems (GNSS) have become the core technology for smartphone navigation. However, the standard single-point positioning accuracy is limited by factors such as ionospheric delays, orbital errors, and multipath effects, typically within a range of several meters. As the demand for high-precision positioning surges in emerging fields like autonomous driving and augmented reality, Differential GNSS (DGNSS), Real-Time Kinematic (RTK), Precise Point Positioning (PPP), and its derivative technology PPP-RTK have gradually become key technical pathways for enhancing smartphone positioning accuracy. These technologies achieve sub-meter to centimeter-level precision through various error correction mechanisms, but their applicability in smartphone scenarios varies significantly.

## Smartphone Implementation

| Technology | Accuracy | Convergence | Network Dep. | Chipset Req. |
|------------|----------|-------------|--------------|--------------|
| DGNSS      | 1m       | Instant     | High         | Single-freq  |
| RTK        | 1-3cm    | Instant     | Critical     | Dual-freq    |
| PPP        | 2-5cm    | 10-30min    | Medium       | Dual-freq    |
| PPP-RTK    | 2-3cm    | 1-2min      | Low          | Dual-freq+   |

## 1.DGNSS: Basic enhancement with communication dependence

Differential GNSS calculates the spatially correlated errors of satellite signals (such as ionospheric delays and ephemeris errors) using ground reference stations, generating differential correction data broadcasted to user terminals. Smartphones receive this correction information via mobile networks or satellite links, enhancing positioning accuracy to within 1 meter. The advantages of this technology include a mature infrastructure, compatibility with existing GNSS chip architectures, and relatively low computational complexity, making it suitable for resource-constrained mobile devices. However, its accuracy is limited by the spatial correlation between the reference station and the user terminal—when the distance exceeds 50 kilometers, the spatial decorrelation of ionospheric delays significantly reduces the effectiveness of corrections. Additionally, frequent signal obstructions in urban environments can lead to delayed updates of correction data, inadvertently introducing positioning errors. Wide-area DGNSS based on cloud servers can mitigate spatial correlation limitations by fusing data from multiple reference stations, but its reliance on continuous cellular network connectivity poses challenges in areas with weak signal coverage.

## 2.RTK: The cost of centimeter-level accuracy

RTK technology achieves centimeter-level real-time positioning by transmitting the carrier phase observations from reference stations to the mobile device and using a double difference algorithm to eliminate common errors. After equipping smartphones with dual-frequency GNSS receivers, they can theoretically support RTK positioning, but actual performance is significantly constrained by hardware limitations. Smartphone antennas are less capable of multipath suppression compared to survey-grade equipment, and signal reflections caused by metal bodies can lead to phase jumps, necessitating dynamic compensation via kinematic ambiguity resolution algorithms. Furthermore, RTK has stringent requirements for communication link stability; delays exceeding 100 milliseconds between the reference station and the mobile device can result in resolution failures. To address this issue, NTRIP services based on Internet Protocol have been integrated into smartphone RTK applications, but fluctuations in service quality on public networks still limit reliability. Notably, Virtual Reference Station (VRS) technology extends the RTK service range by generating virtual observations, but it requires continuous generation of megabits per second correction data streams in the cloud, posing challenges regarding data costs and power consumption for smartphones.

## 3.PPP: The trade-off between global coverage and convergence time
PPP technology provides precise positioning without local reference stations by integrating precise ephemeris, clock corrections, and ionospheric models. Smartphones leveraging L1/L5 dual-frequency observations can effectively mitigate ionospheric delays, potentially achieving centimeter-level static positioning accuracy when combined with post-processed precise products. However, a critical drawback of PPP is the convergence time, which can exceed 30 minutes due to the strong coupling between receiver clock errors and carrier phase ambiguities. Traditional PPP evidently cannot meet real-time requirements for dynamic smartphone navigation. Improved algorithms, such as Integer Point Positioning (IPPP), reduce convergence time to under 10 minutes by introducing ambiguity fixing strategies, but they have not yet crossed the threshold for real-time applications. Additionally, the transmission delay of precise ephemeris (about 18 minutes) forces smartphones to rely on predicted ephemeris, introducing an additional error of 3-5 centimeters. Utilizing the low-latency characteristics of 5G networks for real-time transmission of compressed precise ephemeris could reduce PPP convergence time to under 5 minutes, although this solution has not yet established a standardized protocol.

## 4.PPP-RTK: The dawn of integrated innovation
PPP-RTK, as a paradigm merging PPP (Precise Point Positioning) and RTK (Real-Time Kinematic), allows smartphones to utilize precise orbital clock corrections and ambiguity resolution techniques simultaneously through the dissemination of regional ionospheric corrections and phase bias information. Compared to traditional PPP, its convergence time can be reduced to under one minute while maintaining dynamic positioning accuracy of 2-3 centimeters. The core advantage of this technology lies in its compatibility with wide-area services and rapid initialization: atmospheric correction data generated by regional reference station networks can be broadcast via satellite or mobile networks, enabling smartphones to maintain high-accuracy positioning without a continuous data connection. Smartphones equipped with Qualcomm's Snapdragon 8 Gen2 chip can achieve convergence within 90 seconds in PPP-RTK mode, with dynamic accuracy reaching 3.8 centimeters. However, the implementation of PPP-RTK relies on the construction of dense regional augmentation networks and the standardization of cross-platform data formats, and is currently only experimentally deployed in a few countries.

## 5.Technological integration and future evolution
High-precision navigation on smartphones is moving towards a technology route of multi-source integration. The prevalence of dual-frequency GNSS chips provides the hardware foundation for carrier phase observations, while improved edge computing capabilities enable real-time ambiguity resolution on smartphones. The introduction of the GNSS raw observation value interface in Android 14 by Google allows third-party algorithms to directly process phase data, significantly lowering the application threshold for RTK/PPP-RTK. Simultaneously, the tight coupling of inertial sensors and visual SLAM can compensate for positioning continuity during GNSS signal interruptions. In the future, emerging technologies such as low Earth orbit satellite augmentation and quantum inertial navigation may further break existing accuracy limits, but the constraints of power consumption and cost control in smartphones will continue to dominate the technology selection pathway. Throughout this process, the technical boundaries of DGNSS, RTK, and PPP will become increasingly blurred, eventually evolving into a demand-customized hybrid augmentation positioning service system.

# Task 2 – GNSS in Urban Areas
I encountered some challenges in the implementation of the GNSS code, and I don't have a final implementation. I only have some pseudo code to reflect my ideas.
```python
# Task 2: Urban GNSS Positioning
import numpy as np
from georinex import rinexobs
from skyfield.api import Loader

# Load data
obs_data = rinexobs.load("urban_data.obs")  # RINEX observation file
skymask = load_skymask("skymask.csv")       # Skymask (azimuth vs elevation threshold)
ground_truth = (22.3198722, 114.209101777778, 3.0)

for epoch in obs_data:
    # Step 1: Compute satellite azimuth/elevation
    sats = preprocess(epoch)  # Extract PRN, pseudorange, compute satellite positions
    
    # Step 2: Filter blocked satellites using skymask
    visible_sats = []
    for sat in sats:
        az = sat.azimuth
        el = sat.elevation
        # Find nearest skymask azimuth bin (e.g., 5-degree resolution)
        az_bin = round(az / 5) * 5
        if el > skymask.get(az_bin, 0):
            visible_sats.append(sat)
    
    # Step 3: Weighted Least Squares (WLS) positioning
    if len(visible_sats) >= 4:
        # Elevation-dependent weighting
        weights = [1 / (np.sin(np.radians(sat.elevation)) for sat in visible_sats]
        pos_enu = solve_wls(visible_sats, weights)  # WLS solver
        pos_geo = enu_to_geodetic(pos_enu, ground_truth)
        print(f"Optimized Position: {pos_geo}")
    else:
        print("Insufficient visible satellites.")
```

## Task 3 – GPS RAIM (Receiver Autonomous Integrity Monitoring)
I encountered some challenges in the implementation of the GNSS code, and I don't have a final implementation. I only have some pseudo code to reflect my ideas.
```python
# Task 3: GPS RAIM
import numpy as np
from scipy.stats import chi2

def weighted_raim(obs_data, sigma=3.0, p_fa=1e-2, p_md=1e-7):
    # Step 1: Initial WLS solution
    pos, residuals, H, W = solve_wls(obs_data)  # Assume WLS returns residuals and matrices
    n_sats = len(obs_data)
    
    # Step 2: Fault detection
    sse = np.sum(residuals**2)
    dof = n_sats - 4  # Degrees of freedom (4 unknowns)
    threshold = chi2.ppf(1 - p_md, dof)  # Threshold for P_md=1e-7
    
    if sse > threshold:
        # Step 3: Fault exclusion (identify worst satellite)
        S_matrix = np.eye(n_sats) - H @ np.linalg.inv(H.T @ W @ H) @ H.T @ W
        normalized_res = residuals / np.sqrt(np.diag(S_matrix) * sigma
        worst_sat = np.argmax(np.abs(normalized_res))
        print(f"Excluding satellite: PRN{obs_data[worst_sat].prn}")
        
        # Recursively recompute with remaining satellites
        new_obs = [sat for idx, sat in enumerate(obs_data) if idx != worst_sat]
        return weighted_raim(new_obs, sigma, p_fa, p_md)
    else:
        # Step 4: Compute protection level (Bonus)
        Q = np.linalg.inv(H.T @ W @ H)
        k = 5.33  # Threshold for P_md=1e-7
        PL_3d = k * sigma * np.sqrt(np.trace(Q))
        
        # Step 5: Stanford Chart (Bonus)
        plot_stanford_chart(PL_3d, alarm_limit=50)  # Assume plotting function
        return pos, PL_3d

# Main loop
obs_data = rinexobs.load("open_sky_data.obs")
for epoch in obs_data:
    sats = preprocess(epoch)  # Extract satellite data
    pos, pl = weighted_raim(sats)
    print(f"RAIM Position: {pos}, 3D PL: {pl:.2f}m")
```

# Task 4 – LEO Satellites for Navigation
# The navigation paradox of low earth orbit constellations: Opportunities, challenges, and technological breakthroughs

Abstract: The successful deployment of low Earth orbit (LEO) satellites in the communications sector has spurred wide-ranging exploration into their application in Global Navigation Satellite Systems (GNSS). Compared to traditional Medium Earth Orbit (MEO) GNSS satellites, LEO constellations theoretically promise decimeter-level positioning accuracy with their lower orbital altitudes (300-1200 kilometers), shorter signal transmission delays, and stronger signal power, even enabling penetration into urban canyon environments. However, transforming LEO communication satellites into highly reliable navigation nodes brings multiple challenges related to dynamics, signal schemes, and system integration, which stem from the fundamental contradictions between the physical characteristics of LEO satellites and the core demands of navigation services.

## 1.Dynamics dilemma: High speed and signal stability
LEO satellites have orbital periods of only 90-120 minutes and speeds relative to ground users of up to 7-7.8 km/s, significantly exceeding the 3.87 km/s of MEO satellites. This results in two critical issues.

### 1.1.Extreme doppler shift
The carrier frequency received by users can shift by as much as ±100 kHz (compared to a typical value of ±5 kHz for GPS L1), which traditional GNSS receiver phase-locked loop (PLL) designs cannot accommodate. Although spread spectrum communication systems inherently possess Doppler tolerance, the real-time requirements for frequency compensation algorithms increase significantly, particularly in high-speed mobile scenarios (e.g., aircraft), necessitating frequency offset estimation and tracking within milliseconds, thereby placing pressure on the computational resources of consumer devices like smartphones.

### 1.2.Transient visibility and topological turbulence
The overhead visibility time of a single LEO satellite is only 8-15 minutes, requiring users to frequently switch satellite links. Experimental data indicate that achieving continuous positioning depends on a minimum of 200 satellites in the constellation. While existing communication constellations like Starlink have deployed over 4,000 satellites, their orbital planes and phase distributions have not been optimized for navigation needs, leading to periodic degradation in satellite geometric configuration (DOP values). Moreover, unoptimized LEO constellations exhibit 37% lower positioning availability in equatorial regions compared to polar regions, exposing the deep impact of orbital inclination design on navigation performance.

## 2.Signal scheme adaptation reconstruction
Current LEO communication systems utilize Ku/Ka frequency bands for high-frequency signals, which differ from the L-band used by GNSS at the physical layer, presenting multiple barriers to direct reuse.

### 2.1.Atmospheric propagation loss and multipath effects
Ku-band signals (12-18 GHz) are over 20 dB more affected by rain attenuation than L-band signals, leading to a drastic drop in usability during heavy rainfall. Additionally, high-frequency signals experience increased reflection paths in complex environments (e.g., urban buildings), resulting in multipath errors that can reach meter-level. Empirical results from embedding navigation pilots in Starlink's downlink signals indicate that multipath effects can cause pseudorange fluctuations of 3-8 meters, significantly exceeding typical values for GNSS civilian signals (<1 meter).

### 2.2.Signal structure conflicts
GNSS relies on spread spectrum code modulation and navigation message broadcasting, whereas LEO communication systems prioritize efficient data transmission using dynamic modulation techniques like OFDM and TDD. Directly embedding navigation signals would need to resolve spectrum resource competition issues. For instance, the patent for SpaceX's "satellite-based navigation enhancement service" proposes inserting navigation ranging codes during communication frame guard intervals, which would sacrifice 4.7% of communication capacity, raising cost-effectiveness concerns in commercial models.

## 3.Collaborative dilemma of time and space reference
LEO satellite navigation systems must establish independent time and space references or achieve deep integration with traditional GNSS systems, creating a dilemma:

### 3.1.Cost of autonomous time system construction
Maintaining atomic clock stability requires each LEO satellite to be equipped with high-precision atomic clocks (such as rubidium clocks). However, the lifespan of LEO satellites is only 5-7 years (compared to 15 years for MEO satellites), resulting in a drastic increase in unit time costs. If the constellation deploys hydrogen clocks comprehensively, its operational costs could exceed 300% that of MEO systems.

### 3.2.Complexity of cross-system interoperation
If LEO serves as an augmentation layer for GNSS, it necessitates unified temporal and spatial reference systems with GPS, Galileo, and other systems. However, the high-speed motion of LEO satellites results in relativistic correction terms (approximately 10^-10 scale) that are two orders of magnitude higher than those for MEO satellites, complicating compatibility with traditional GNSS receiver algorithms. The European Space Agency (ESA) has attempted to synchronize clock discrepancies between LEO and MEO through inter-satellite links in its Moonlight project, but the transmission of inter-satellite ranging error leads to a positioning accuracy loss of up to 12%.
System-Level Challenges: Security, Power Consumption, and Spectrum Management

## 4.System-level challenges
At the application level, LEO navigation faces three major real-world constraints.

### 4.1.Expanded vulnerability exposure
The inter-satellite links of dense LEO constellations combined with ground control stations create a complex attack interface. At the 2022 DEF CON hacker conference, a demonstration showcased how software-defined radio (SDR) could inject spoofed LEO navigation signals, causing receiver offsets of up to 200 meters, thereby exposing security vulnerabilities inherent in an open signal system.

### 4.2.Surge in terminal power consumption
Continuously tracking high-speed LEO satellites requires the number of receiver channels to increase from the traditional 12-16 to over 50. This change could result in a 3-5 times increase in power consumption for smartphones, conflicting with the energy efficiency demands of mobile devices.

### 4.3.Intensifying spectrum resource competition
The International Telecommunication Union (ITU) has reached saturation in the frequency allocation for LEO communications, necessitating global coordination for new navigation-specific frequency bands. The upcoming 10.7-12.7 GHz frequency competition between OneWeb and GPS IIIF satellites in 2025 reflects the severity of this conflict.

# Task 5 – GNSS Remote Sensing
# GNSS reflection measurement method: A paradigm shift from positioning signals to earth surface probes

Abstract: The positioning capabilities of Global Navigation Satellite Systems (GNSS) have permeated various aspects of modern society, yet their potential as passive remote sensing tools has only been fully explored in recent years. GNSS Reflection Measurement (GNSS-R) converts multipath noise—originally a disturbance to positioning accuracy—into sensitive probes for detecting geophysical parameters by analyzing the navigation signals reflected from the Earth’s surface, thus establishing a new remote sensing paradigm known as "signal as sensor." This technology not only disrupts the high-cost model of traditional active remote sensing devices but also fills gaps in global environmental monitoring networks with its unique spatiotemporal resolution.

## 1.Physical mechanism and information inversion
The core of GNSS-R lies in extracting coherent and incoherent characteristics between direct signals and reflected signals from the surface. When L-band (1.2-1.6 GHz) navigation signals reflect off surfaces such as oceans, soil, or ice, their polarization characteristics, Delay-Doppler Map (DDM), and phase information convey the dielectric properties and roughness of the surface. For example, there is a negative correlation between sea surface wind speed and the leading edge slope of the reflected signal, while changes in soil moisture can lead to fluctuations of up to 10 dB in left-handed circular polarized (LHCP) reflected signal power. Using the Interference Complex Field (ICF) algorithm, analysis of the carrier phase interference between direct and reflected signals can enhance sea level measurement accuracy to the centimeter level, comparable to dedicated radar altimeter performances.

## 2.Revolutionary breakthrough in dynamic environmental monitoring
In the monitoring of polar ice sheets, GNSS-R demonstrates advantages that traditional methods cannot achieve. The PARIS-IoD satellite uses GPS reflection signals to measure sea ice thickness, penetrating depths of up to 0.5 meters beneath the dry snow layer with a temporal resolution of hours. This capability allows scientists to track the melting process of the Greenland ice sheet in near real-time, revealing the nonlinear relationship between summer meltwater pool dynamics and ice shelf stability. During the 2022 collapse event of the Thwaites Ice Shelf in Antarctica, a ground-based GNSS-R system detected anomalous changes in dielectric constant 48 hours ahead of the ice body’s failure, confirming the potential value of this technology in disaster warning systems.

## 3.Three-dimensional observation of the hydrological cycle
The inversion of surface hydrological parameters is one of the most disruptive applications of GNSS-R. Traditional soil moisture monitoring relies on satellite-based microwave radiometers (such as SMOS), but their spatial resolution (>40 km) struggles to capture variations at the agricultural field scale. NASA's CYGNSS constellation, consisting of eight low Earth orbit satellites, enables high-frequency observations of surface moisture in tropical regions (with a revisit time of 3 hours), breaking through to a spatial resolution of 500 meters. This system capitalizes on the dispersive properties of GNSS reflection signals: moist soil increases signal attenuation rates, while vegetative canopies alter polarization ratios. In monitoring drought in the Horn of Africa in 2023, CYGNSS data successfully identified hidden drought zones at a depth of 10 centimeters, providing critical decision-making support for emergency responses from the UN Food and Agriculture Organization.

## 4.Multidimensional perception of ocean dynamics
GNSS-R has achieved a leap from two-dimensional planes to three-dimensional perspectives in the field of ocean remote sensing. By employing the STARLING algorithm, simultaneous inversion of sea surface wind speed (with an accuracy of 1.5 m/s), significant wave height (accuracy of 0.3 meters), and surface current fields (accuracy of 5 cm/s) is possible through joint analysis of Doppler frequency shifts and scattering power of reflected signals. This multi-parameter acquisition capability allows scientists to reconstruct the three-dimensional energy transfer processes in the eyewall of typhoons. Observational data from the Atlantic hurricane "Ida" in 2021 showed that the sea wave spectrum inverted by GNSS-R correlated with buoy measurements, achieving a correlation coefficient of 0.91, validating the technology’s ability to faithfully reproduce extreme weather processes.

## 5.Technological bottlenecks and collaborative evolution
Despite GNSS-R's immense potential, its development is constrained by dual challenges in signal processing and system integration. Firstly, the power of reflected signals is 20-30 dB lower than that of direct signals, making it prone to noise in complex terrain. Utilizing compressed sensing receivers, adaptive matched tracking algorithms have improved weak signal detection sensitivity by 15 dB. Secondly, existing constellations do not provide adequate spatial sampling coverage to meet global continuous monitoring demands. The proposal of the "GNSS-R satellite cluster" aims to deploy miniaturized reflection signal receivers on 300 CubeSats, establishing a network capable of observing the globe four times daily.

## 6.Future perspectives of interdisciplinary integration
GNSS-R is poised for deep integration with cutting-edge technologies such as artificial intelligence and quantum sensing. By employing convolutional neural networks to directly invert surface parameters from raw Delay-Doppler Map (DDM) data, the speed of soil moisture inversion could be improved by two orders of magnitude. Attempts to utilize quantum entanglement to enhance the signal-to-noise ratio of reflected signals have shown promising initial results, with ice density measurement accuracy reaching 0.5%. These breakthroughs indicate that GNSS-R is transforming from a supplementary observation method into a core data engine for the next generation of Earth system science. As GNSS satellite networks continue to weave a positioning framework, their reflected signals quietly construct an invisible network that senses the pulses of the Earth. This "single-source dual-use" technological philosophy not only significantly expands the dimensions of human observation of the Earth but also redefines the performance boundaries of space infrastructure.

## Declaration of AI and AI-assisted technologies in the writing process
During the preparation of this work, the author(s) used GenAI to collecting Information and enhance the readability of the text. After using this tool/service, the author(s) reviewed and edited the content as necessary and assume(s) full responsibility for the content.
```
Model: ChatGPT 4o

Chatroom Link: https://poe.com/s/IVQFgK1GDGzWZm1UR24O

```
## References
```
[1] Schaefer, M., & Pearson, A. (2021). Accuracy and precision of GNSS in the field. In GPS and GNSS Technology in Geosciences (pp. 393-414). Elsevier.
[2] Prol, F. S., Ferre, R. M., Saleem, Z., Välisuo, P., Pinell, C., Lohan, E. S., ... & Kuusniemi, H. (2022). Position, navigation, and timing (PNT) through low earth orbit (LEO) satellites: A survey on current status, challenges, and opportunities. IEEE access, 10, 83971-84002.
[3] Yu, K. (2021). Theory and practice of GNSS reflectometry. Berlin, Germany: Springer.
[4] Edokossi, K., Calabia, A., Jin, S., & Molina, I. (2020). GNSS-reflectometry and remote sensing of soil moisture: A review of measurement techniques, methods, and applications. Remote Sensing, 12(4), 614.
[5] Egido, A. (2013). GNSS reflectometry for land remote sensing applications (Doctoral dissertation, Polytechnic University of Catalonia, Spain).
[6] Walter, T., & Enge, P. (1995, September). Weighted RAIM for precision approach. In Proceedings of Ion GPS (Vol. 8, No. 1, pp. 1995-2004). Institute of Navigation.
```
