# ORB-SLAM3 Docker Setup

Docker container with **CUDA + X11(GUI) Support + Ubuntu 22.04**

| ORB-SLAM3 Dockerized |
| :---: |
| <img src="/resources/image.png" width="100%" alt="ORB-SLAM3 running with EuRoC dataset"> |

This guide will walk you through setting up ORB-SLAM3 in a Docker container, running it with a EuRoC dataset, and testing it with different configurations like Monocular, Monocular-Inertial, and Stereo.

> [!important]
> Communication between ORB-SLAM3 is done with sockets in this example. Real world applications usually use ROS communication protocol.

## Prerequisites

- Docker
- NVIDIA GPU and drivers with CUDA support for GPU acceleration
- X11 server running for displaying graphical applications

## Getting Started

### Step 1: Running the Docker Container

Build the Docker image:

```bash
docker build -t orb_slam3:latest .
```

Run the container with GPU support and X11 forwarding for graphical output:

```bash
docker run -it --gpus all --env="DISPLAY" \
  --env="QT_X11_NO_MITSHM=1" \
  --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
  --name orb_slam3_container \
  orb_slam3:latest
```

### Step 2: Dataset Setup

Inside the container download and set up the EuRoC MAV dataset:

```bash
cd /opt/orb_slam3

# Create directory for dataset
mkdir -p Datasets/EuRoc
cd Datasets/EuRoc/

# Download the dataset
wget -c http://robotics.ethz.ch/~asl-datasets/ijrr_euroc_mav_dataset/machine_hall/MH_01_easy/MH_01_easy.zip

# Create a directory for MH_01_easy dataset and extract it
mkdir MH01
unzip MH_01_easy.zip -d MH01/
```

### Step 3: Installing Additional Packages

To run graphical applications inside the container:

```bash
apt-get update && apt-get install -y x11-apps
```

Test the X11 setup:

```bash
xclock
```

> [!check]
> You should see a clock window appear on your screen. If it works, your X11 forwarding is configured correctly.

### Step 4: Running ORB-SLAM3 Examples

Navigate to the ORB-SLAM3 directory and run the examples:

#### Stereo Example

```bash
./Examples/Stereo/stereo_euroc ./Vocabulary/ORBvoc.txt \
  ./Examples/Stereo/EuRoC.yaml \
  ../Datasets/EuRoc/MH01 \
  ./Examples/Stereo/EuRoC_TimeStamps/MH01.txt dataset-MH01_stereo
```

Start X11 Server to forward video feed.
