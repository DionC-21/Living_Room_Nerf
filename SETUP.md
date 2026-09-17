# Setup Instructions

## Google Colab Training

### Requirements
- Google Colab account
- Google Drive with input images
- Images stored at: Drive/living_room_nerf/

### Steps
1. Run colab/train.ipynb cells in order
2. Output saved to Drive/living_room_nerf/gs_output/

## Jetson Nano Deployment

### Requirements
- NVIDIA Jetson Nano
- JetPack 4.6 (CUDA 10.2)
- Python 3.6

### Installation
```bash
# Install system dependencies
sudo apt-get install -y python3.6-venv libopenmpi-dev libopenblas-dev

# Create virtual environment
python3.6 -m venv ~/gsplat_env36
source ~/gsplat_env36/bin/activate

# Install PyTorch for JetPack 4.6
wget https://nvidia.box.com/shared/static/fjtbno0vpo676a25cgvuqc1wty0fkkg6.whl -O torch-1.10.0-cp36-cp36m-linux_aarch64.whl
python -m pip install torch-1.10.0-cp36-cp36m-linux_aarch64.whl
python -m pip install numpy==1.19.4 plyfile tqdm Pillow scipy glfw pyopengl imgui pyglm imageio

# Fix MPI library
sudo ln -s /usr/lib/aarch64-linux-gnu/libmpi_cxx.so.20 /usr/lib/aarch64-linux-gnu/libmpi_cxx.so.40
sudo ln -s /usr/lib/aarch64-linux-gnu/libmpi.so.20 /usr/lib/aarch64-linux-gnu/libmpi.so.40
sudo ldconfig

# Clone viewer
git clone https://github.com/limacv/GaussianSplattingViewer.git ~/gsviewer
```

### Running the Viewer
```bash
source ~/gsplat_env36/bin/activate
export LD_LIBRARY_PATH=/usr/local/cuda-10.2/targets/aarch64-linux/lib:$LD_LIBRARY_PATH
export DISPLAY=:1
cd ~/gsviewer
python main.py
```
Press O to open file dialog and load your .ply file.

### Fan Control
```bash
# Full speed (recommended during rendering)
sudo sh -c 'echo 255 > /sys/devices/pwm-fan/target_pwm'
```
