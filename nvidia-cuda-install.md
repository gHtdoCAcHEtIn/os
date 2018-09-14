I reinstalled the OS and re-ran everything same as before adding the --no-opengl-libs flag. This is the critical part. I would like to add something else though! I did not install have to install the NVIDIA driver explicitly first. I ran the cuda.run file instead, and said yes to install the NVIDIA driver when it prompted me to. I feel like doing this in one straight run made everything way easier. Thanks for everything!

Post of final script and solution

0. Download your relevant CUDA.run file: mine was: `cuda_7.0.28_linux.run`

Note, that once again this install is if you purely want to use your graphics card (Titan X) for GPU/CUDA purposes and not for rendering.

Also run: `sudo apt-get install build-essential`

1. I start off with the regular GUI and Ubuntu working with no login problems.
2. No need to create an `xorg.conf` file. If you have one, remove it (assuming you ahve a fresh OS install). 
`sudo rm /etc/X11/xorg.conf`

3. Create the `/etc/modprobe.d/blacklist-nouveau.conf` file with :
```bash
blacklist nouveau
options nouveau modeset=0
```
Then 
```bash
sudo update-initramfs -u
```

4. Reboot computer. Nothing should have changed in loading up menu. You should be taken to the login screen. Once there type: Ctrl + Alt + F1, and login to your user.

5. Go to the directory where you have the CUDA driver, and run 
`chmod a+x . `

6. Now, run 
```bash
sudo service lightdm stop
```
The top line is a necessary step for installing the driver.

7. I run the CUDA driver run file. Notice that I explicitly don't want the OpenGL flags to be installed:
```bash
sudo bash cuda-7.0.28_linux.run --no-opengl-libs
```

or
```bash
sudo bash ./NVIDIA-Linux-x86_64-384.66.run --no-opengl-files
sudo bash ./cuda_8.0.61_375.26_linux.run --no-opengl-libs
```

8. During the install: 
* Accept EULA conditions
* Say NO to installing the NVIDIA driver
* SAY YES to installing CUDA Toolkit + Driver
* Say YES/NO (optional) to installing CUDA Samples
* Say NO to rebuilding any Xserver configurations with Nvidia.

9. Installation should be complete. Now check if device nodes are present:
Check if /dev/nvidia* files exist. If they don't, do :
```bash
sudo modprobe nvidia
```

10. Set Environment path variables:
```bash
export PATH=/usr/local/cuda-7.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-7.0/lib64:$LD_LIBRARY_PATH
```
Change 7.0 to your CUDA version

11. Verify the driver version:
```bash
cat /proc/driver/nvidia/version
```

12. Check nvcc (CUDA compiler) version:
```bash
nvcc -V
```
13. Check CUDA driver version:
```bash
nvidia-smi
```

14. [Optional] At this point you can switch the lightdm back on again by doing:
```bash
sudo service lightdm start. 
```

You should be able to login to your session through the GUI without any problems or login-loops.

15. Create CUDA Samples. Go to your `NVIDIA_CUDA-7.5_Samples folder` and type `make`.

16. Go to `NVIDIA_CUDA-7.5_Samples/bin/x86_64/linux/release/` for the demos, and do the two standard checks:
```bash
./deviceQuery
```
to see your graphics card specs and
```bash
./bandwidthTest
```
to check if its operating correctly.

Both tests should ultimately output a 'PASS' in your terminal.

17. Reboot. Everything should be ok.

Versions:
* Ubuntu 16.04.3 LTS Desktop amd64
* GCC 5.4.0
* CUDA 8.0
* NVIDIA driver 384.66


------------------------------------------------------
2018/09/11 [u102016]

Upgrading from nvidia driver 387.34 to 390.12

* nvidia-install --uninstall
* cat /proc/driver/nvidia/version
  - still shows NVRM kernel module (387.34) loaded
  - GCC version 5.4.x loaded

* reboot
* sudo service lightdm stop
* sudo ./NVIDIA-...390.12.run 
* 32-bit compatiblity - skip
* update nvidia-xconfig - skip
* test nvidia-smi 
  - works
  - shows TITAN V
