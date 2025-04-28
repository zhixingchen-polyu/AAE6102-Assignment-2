# AAE6102_Assignment2: Lonosphere mapping based GNSS ground station

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

## 1. DGNSS: Basic enhancement with communication dependence

Differential GNSS calculates the spatially correlated errors of satellite signals (such as ionospheric delays and ephemeris errors) using ground reference stations, generating differential correction data broadcasted to user terminals. Smartphones receive this correction information via mobile networks or satellite links, enhancing positioning accuracy to within 1 meter. The advantages of this technology include a mature infrastructure, compatibility with existing GNSS chip architectures, and relatively low computational complexity, making it suitable for resource-constrained mobile devices. However, its accuracy is limited by the spatial correlation between the reference station and the user terminal—when the distance exceeds 50 kilometers, the spatial decorrelation of ionospheric delays significantly reduces the effectiveness of corrections. Additionally, frequent signal obstructions in urban environments can lead to delayed updates of correction data, inadvertently introducing positioning errors. Wide-area DGNSS based on cloud servers can mitigate spatial correlation limitations by fusing data from multiple reference stations, but its reliance on continuous cellular network connectivity poses challenges in areas with weak signal coverage.

## 2. RTK: The cost of centimeter-level accuracy

RTK technology achieves centimeter-level real-time positioning by transmitting the carrier phase observations from reference stations to the mobile device and using a double difference algorithm to eliminate common errors. After equipping smartphones with dual-frequency GNSS receivers, they can theoretically support RTK positioning, but actual performance is significantly constrained by hardware limitations. Smartphone antennas are less capable of multipath suppression compared to survey-grade equipment, and signal reflections caused by metal bodies can lead to phase jumps, necessitating dynamic compensation via kinematic ambiguity resolution algorithms. Furthermore, RTK has stringent requirements for communication link stability; delays exceeding 100 milliseconds between the reference station and the mobile device can result in resolution failures. To address this issue, NTRIP services based on Internet Protocol have been integrated into smartphone RTK applications, but fluctuations in service quality on public networks still limit reliability. Notably, Virtual Reference Station (VRS) technology extends the RTK service range by generating virtual observations, but it requires continuous generation of megabits per second correction data streams in the cloud, posing challenges regarding data costs and power consumption for smartphones.


