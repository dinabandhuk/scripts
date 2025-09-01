# Scripts
Helper scripts and retrofits
# Vivado FPGA design suite on Debian 13 trixie
Since I'm confused on the licensing terms of the original script,
I'll only provide my own steps to adapt the install on "Linux 6.12.41+deb13-amd64 Debian 13 Trixie"
Steps:
- create a custom install script installLibsDebian.sh from the original installLibs.sh
- ```bash
  sudo cp installLibs.sh installLibsDebian.sh
  sudo chown $(echo "$USER") installLibsDebian.sh
  ```
- then modify the installLibsDebian.sh by adding these lines to the if then sections
- ```bash
  if [[ $OSDIST == "debian"* ]]; then
  doDebian
  fi
  ```
- and also create the function in the same file installLibsDebian.sh in appropriate location with similar functions
- ```bash
  doDebian()
\{
\### AIE Tools prerequisite libraries
   apt-get update | tee -a $logFile
   apt-get install -y libc6-dev-i386 net-tools | tee -a $logFile
   apt-get install -y graphviz | tee -a $logFile
   apt-get install -y make | tee -a $logFile
\### Vitis Tools prerequisite libraries
   apt-get install -y unzip | tee -a $logFile
   apt-get install -y zip | tee -a $logFile
   apt-get install -y g++ | tee -a $logFile
   apt-get install -y libtinfo5 | tee -a $logFile
   apt-get install -y xvfb | tee -a $logFile
   apt-get install -y git | tee -a $logFile
   #apt-get install -y libncurses5 | tee -a $logFile
   apt-get install -y libncurses5-dev | tee -a $logFile
   apt-get install -y libc6-dev-i386 | tee -a $logFile
   apt-get install -y libnss3-dev | tee -a $logFile
   apt-get install -y libgdk-pixbuf2.0-dev | tee -a $logFile
   apt-get install -y libgtk-3-dev | tee -a $logFile
   apt-get install -y libxss-dev  | tee -a $logFile
   apt-get install -y libasound2   | tee -a $logFile
   apt-get install -y compat-openssl10  | tee -a $logFile
   apt-get install -y fdisk  | tee -a $logFile
   apt-get install -y  libsecret-1-dev | tee -a $logFile
\}
```
- execute ```bash
./installLibsDebian.sh
```
- if your system doesn't have utf-8 en locale install it as [shown](https://serverfault.com/questions/54591/how-to-install-change-locale-on-debian)
- ```bash
  sudo apt install locales-all
  ```
- If libtinfo5 is missing, manually install it by downloading .deb file [as shown](https://askubuntu.com/questions/1531760/how-to-install-libtinfo5-on-ubuntu24-04)
- Do the same for other missing libraries if they are not on official repository and
  ensure there are no binary/package conflicts, otherwise your system will be rendered unusable.
- Inspect the logs of the install script for other possible issues.
- You might also have to create symlinks to some libraries, relevant information can be looked up on Xilinx Support Website.
