# KavinV_RISC-V-SoC-Tapeout-Program_VSD

## 🧪 Week 0 Tasks – Tools Installation

In this week, I installed the following tools required for digital design and verification:

## 🔧 Installed Tools

1. 📂 Virtual box :
      install virtual box and create a virtual space with 6 gb ram and 50 gb hdd and 4 cpu.if you can't access the root then use this commands
    ```
             su
            usermod -aG sudo username
      ```

after this you can access the root. 
   
2.📂 install Yosys  
   ```
      sudo apt-get update 
      git clone https://github.com/YosysHQ/yosys.git 
      cd yosys 
      sudo apt install make (If make is not installed please install it)  
      sudo apt-get install build-essential clang bison flex \ 
        libreadline-dev gawk tcl-dev libffi-dev git \ 
        graphviz xdot pkg-config python3 libboost-system-dev \ 
        libboost-python-dev libboost-filesystem-dev zlib1g-dev 
      make config-gcc 
      make  
      sudo make install
   ```
### Yosys
![yosys installation]()


3.📂 Icarus Verilog (iverilog)*  
  
      ```
      sudo apt-get update 
      sudo apt-get install iverilog
      ```

### Icarus Verilog
![iverilog installation]()

4. 📂 *GTKWave*  
   ```
      sudo apt-get update 
      sudo apt-get install gtkwave
      ```
### GTKWave
![gtkwave installation]()
---







