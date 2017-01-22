# CUDA-Instructions
Steps to install CUDA on Ubuntu 16.04 (LTS) and configuring NVIDIA Graphics Card

## Steps

(Keep these instructions handy in another machine as later on you'll be typing commands in tty mode. Also, take backup of your system before initiating.)

1. Install build essentials.

  ```
  sudo apt-get install build-essential
  ```
2. Download the CUDA toolkit.

  ```
  wget https://developer.nvidia.com/compute/cuda/8.0/prod/local_installers/cuda_8.0.44_linux-run
  ```
3. Extract installers (path in extract argument must be absolute).

  ```
  ./cuda_8.0.44_linux.run -extract=<absolute_path>
  ```
4. Remove all traces of nvidia in ubuntu repositories.

  ```
  sudo apt-get --purge remove nvidia-*
  ```
5. Remove xorg config file if present.

  ```
  sudo rm /etc/X11/xorg.conf
  ```
6. Stop the default nouveau driver.

  ```
  sudo nano /etc/modprobe.d/blacklist-nouveau.conf
  ```
  * Add the following lines:
  
    ```
    blacklist nouveau
    options nouveau modeset=0
    ```
7. Update the initramfs image and reboot.

  ```
  sudo update-initramfs -u
  sudo reboot
  ```
8. Switch to tty with <kbd>CTRL</kbd>+<kbd>ALT</kbd>+<kbd>F1</kbd> and login to your user. You can exit the tty mode with <kbd>ALT</kbd>+<kbd>F7</kbd>
9. Kill the x-server. (This is necessary !)

  ```
  sudo service lightdm stop
  ```
10. Run the following commands. (Flag "--no-opengl-files" is necessary otherwise you will get stuck in a login loop forever !)

  ```
  sudo ./NVIDIA-Linux-x86_64-367.48.run --no-opengl-files
  sudo ./cuda-linux64-rel-8.0.44-21122537.run
  sudo ./cuda-samples-linux-8.0.44-21122537.run
  ```
11. Set the environment variables.

  ```
  export PATH=/usr/local/cuda-8.0/bin:$PATH
  export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:$LD_LIBRARY_PATH
  ```
12. Verify the install.

  ```
  cat /proc/driver/nvidia/version
  nvcc -V
  ```
13. Now you can switch back to normal mode. Run ```sudo service lightdm start```
14. Check your gcc version ```gcc --version```. CUDA doesn't support gcc version > 5. So either you can install a lower version of gcc or do the following(works!):
  * ```sudo nano /usr/local/cuda/include/host_config.h```
  * Comment out the line 119 (//#error -- unsupported GNU version! gcc versions later than 5 are not supported!)

15. Run the following commands.

  ```
  cd /usr/local/cuda/samples/1_Utilities/deviceQuery
  sudo make
  ./deviceQuery
  ```
  You Should get the following output:
  ```
  ./deviceQuery Starting...

   CUDA Device Query (Runtime API) version (CUDART static linking)

    Detected 1 CUDA Capable device(s)

    Device 0: "GeForce 940MX"
      CUDA Driver Version / Runtime Version          8.0 / 8.0
      CUDA Capability Major/Minor version number:    5.0
      Total amount of global memory:                 2002 MBytes (2099642368 bytes)
      ( 3) Multiprocessors, (128) CUDA Cores/MP:     384 CUDA Cores
      GPU Max Clock rate:                            1242 MHz (1.24 GHz)
      Memory Clock rate:                             1001 Mhz
      Memory Bus Width:                              64-bit
      L2 Cache Size:                                 1048576 bytes
      Maximum Texture Dimension Size (x,y,z)         1D=(65536), 2D=(65536, 65536), 3D=(4096, 4096, 4096)
      Maximum Layered 1D Texture Size, (num) layers  1D=(16384), 2048 layers
      Maximum Layered 2D Texture Size, (num) layers  2D=(16384, 16384), 2048 layers
      Total amount of constant memory:               65536 bytes
      Total amount of shared memory per block:       49152 bytes
      Total number of registers available per block: 65536
      Warp size:                                     32
      Maximum number of threads per multiprocessor:  2048
      Maximum number of threads per block:           1024
      Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
      Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
      Maximum memory pitch:                          2147483647 bytes
      Texture alignment:                             512 bytes
      Concurrent copy and kernel execution:          Yes with 1 copy engine(s)
      Run time limit on kernels:                     No
      Integrated GPU sharing Host Memory:            No
      Support host page-locked memory mapping:       Yes
      Alignment requirement for Surfaces:            Yes
      Device has ECC support:                        Disabled
      Device supports Unified Addressing (UVA):      Yes
      Device PCI Domain ID / Bus ID / location ID:   0 / 1 / 0
      Compute Mode:
         < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >

    deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 8.0, CUDA Runtime Version = 8.0, NumDevs = 1, Device0 = GeForce 940MX
    Result = PASS
  ```
