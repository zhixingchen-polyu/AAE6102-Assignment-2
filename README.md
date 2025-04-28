# AAE6102_Assignment2: Lonosphere mapping based GNSS ground station

![GNSS-Technologies](https://img.shields.io/badge/GNSS-DGNSS%7CRTK%7CPPP%7CPPP--RTK-blue) 

# Evolution of High-Precision GNSS Positioning Technology in Smartphone Navigation: From DGNSS to PPP-RTK

Global Navigation Satellite Systems (GNSS) have become the core technology for smartphone navigation. However, the standard single-point positioning accuracy is limited by factors such as ionospheric delays, orbital errors, and multipath effects, typically within a range of several meters. As the demand for high-precision positioning surges in emerging fields like autonomous driving and augmented reality, Differential GNSS (DGNSS), Real-Time Kinematic (RTK), Precise Point Positioning (PPP), and its derivative technology PPP-RTK have gradually become key technical pathways for enhancing smartphone positioning accuracy. These technologies achieve sub-meter to centimeter-level precision through various error correction mechanisms, but their applicability in smartphone scenarios varies significantly.

## 1. DGNSS: Basic Enhancement with Communication Dependence

Differential GNSS calculates the spatially correlated errors of satellite signals (such as ionospheric delays and ephemeris errors) using ground reference stations, generating differential correction data broadcasted to user terminals. Smartphones receive this correction information via mobile networks or satellite links, enhancing positioning accuracy to within 1 meter. The advantages of this technology include a mature infrastructure, compatibility with existing GNSS chip architectures, and relatively low computational complexity, making it suitable for resource-constrained mobile devices. 

## Smartphone Implementation

| Technology | Accuracy | Convergence | Network Dep. | Chipset Req. |
|------------|----------|-------------|--------------|--------------|
| DGNSS      | 1m       | Instant     | High         | Single-freq  |
| RTK        | 1-3cm    | Instant     | Critical     | Dual-freq    |
| PPP        | 2-5cm    | 10-30min    | Medium       | Dual-freq    |
| PPP-RTK    | 2-3cm    | 1-2min      | Low          | Dual-freq+   |


