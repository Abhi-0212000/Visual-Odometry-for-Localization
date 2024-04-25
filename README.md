## Visual-Odometry-for-Localization

Visual Odometry for Localization in Autonomous Driving


Visual odometry is a technique used to estimate the trajectory of a camera by analyzing images captured by the camera over time. In this project, we implement visual odometry using images obtained from a monocular camera.

### Loading and Visualizing the Data

The dataset used in this project is obtained from the CARLA simulator. It consists of grayscale images, RGB images, and depth maps. However, for our purpose of determining the camera trajectory, we only utilize grayscale images. RGB images are used for visualization purposes.

### Step 1: Feature Extraction

The first step in visual odometry is feature extraction. Features are distinct points or regions in an image that can be reliably tracked over time. We extract features from each image in the dataset and compute descriptors for each feature.

We can use various feature extraction algorithms such as ORB, SIFT, or SURF.

![feature_descriptor](https://github.com/Abhi-0212000/Visual-Odometry-for-Localization/assets/70425157/2ee03f7d-25cb-4f1d-8f99-4bfd592507cb)


### Step 2: Feature Matching

In this step, we match features between subsequent pairs of images. The goal is to find corresponding features between two consecutive frames.

We can employ algorithms such as brute-force matching or FLANN (Fast Library for Approximate Nearest Neighbors) to find matches.

![matched_features_img1_2](https://github.com/Abhi-0212000/Visual-Odometry-for-Localization/assets/70425157/49009b46-93a2-4a1a-be89-e2f1b11f84ef)


### Step 3: Trajectory Estimation

#### 3.1 Estimating Camera Motion

To estimate the motion of the camera between subsequent frames, we need to determine the rotation matrix (extrinsic camera matrix) and translation vector.

This can be achieved by decomposing the essential matrix obtained from the matched features using methods like essential matrix decomposition or PnP RANSAC.

![Camera_movement_visualization](https://github.com/Abhi-0212000/Visual-Odometry-for-Localization/assets/70425157/883338d3-e8a2-4321-bb03-4bfc079b1de0)


#### 3.2 Camera Trajectory Estimation

The camera trajectory is estimated by considering that the origin of the trajectory coordinate system is located at the camera position when the first image (index 0) was taken.

**Camera Pose Estimation Pipeline**

$$
I_{0:n}=\left\(I_0,\ldots,I_n\right\)
$$

- `I` : Images

$$
T_{k,k-1}=
\begin{bmatrix}
R_{k,k-1}&t_{k,k-1}\\
0&1
\end{bmatrix}
$$

$$T_{k,k-1}\in\mathbb{R}^{4\times4}
\ \ \ \ \ \ \ \ \, \ \ \ \ \ \ \ \ \
R_{k,k-1}\in\mathbb{R}^{3\times3}
\ \ \ \ \ \ \ \ \, \ \ \ \ \ \ \ \ \
t_{k,k-1}\in\mathbb{R}^{3\times1}
$$

- `T` : Rigid Body Transformation
- `R` : Rotation matrix
- `t` : Translation vector

$$T_{1:n}=\left\(T_1,\ldots,T_n\right\)$$

$$C_{0:n}=\left\(C_0,\ldots,C_n\right\)$$

- `C` : Camera Pose

$$C_0=I_{4\times4}$$

- `C_0` : Initial Camera Pose

$$C_n=C_{n-1}T_n^{-1}$$


- The position of the camera at time \( k \) relative to the initial position is given by the last column of \( C_k \), where the values represent the \( x \), \( y \), and \( z \) coordinates.

### Results

![trajectory](https://github.com/Abhi-0212000/Visual-Odometry-for-Localization/assets/70425157/9713083f-f968-466a-b206-018b4552400d)


---

This pipeline provides a structured approach to implementing visual odometry for camera localization. Each step builds upon the previous one, ultimately leading to the estimation of the camera trajectory.
