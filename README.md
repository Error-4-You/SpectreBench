# SpectreBench





# <h3>By Error-4-You(E4U)</h3>

## 😒😐

![C++](https://img.shields.io/badge/c++-%2300599C.svg?style=for-the-badge&logo=c%2B%2B&logoColor=white)    ![HTML5](https://img.shields.io/badge/html5-%23E34F26.svg?style=for-the-badge&logo=html5&logoColor=white)   ![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)   ![Java](https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=java&logoColor=white)   ![PHP](https://img.shields.io/badge/php-%23777BB4.svg?style=for-the-badge&logo=php&logoColor=white)    ![Shell Script](https://img.shields.io/badge/shell_script-%23121011.svg?style=for-the-badge&logo=gnu-bash&logoColor=red)
  
# 😏😏

[![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=Error-4-You&langs_count=8)](https://github.com/Error-4-You/github-readme-stats)

# 🤬🤬


<p align="center">
<a href="https://github.com/Error-4-You"><img title="YouTube" src="https://img.shields.io/badge/Error-4You-brightgreen?style=for-the-badge&logo=github"></a>
<a href="https://youtube.com/channel/UCfjJgu6-VQPvcgRaygLyhqQ"><img title="YouTube" src="https://img.shields.io/badge/YouTube-Error 4 You-red?style=for-the-badge&logo=Youtube"></a>
</p>

<p align="center">
<a href="https://t.me/h4ck199"><img title="Telegram" src="https://img.shields.io/badge/Telegram-black?style=for-the-badge&logo=Telegram"></a>
<a href="https://chat.whatsapp.com/FkcQ98SucwgK1TbTfGWzsR"><img title="whatsapp" src="https://img.shields.io/badge/whatsapp-blue?style=for-the-badge&logo=whatsapp"></a>

<a href="https://chat.whatsapp.com/FkcQ98SucwgK1TbTfGWzsR">![TikTok](https://img.shields.io/badge/H4CK.LK-%23000000.svg?style=for-the-badge&logo=TikTok&logoColor=FF0F00)</a>
</p>




## LICENSE:
This software is free to distribute, modify and use with the condition that credit is provided to the creator (Error-4-You) and is not for commercial use.

This repository contains a collection of scripts and code used to measure the impact of Spectre mitigations on a variety of workloads.

## Linux Command Line Options

To toggle specific mitigations, use the relevent [Linux kernel command line options](https://www.kernel.org/doc/html/v5.15/admin-guide/kernel-parameters.html). Some specific options that may be relevant:

 * `mitigations=auto` (run with default mitigations)
 * `mitigations=off` (run with no mitigations)
 * `nospec_store_bypass_disable` (disable the "speculative store bypass disable" mitigation)
 * `spec_store_bypass_disable=on` (force-enable "speculative store bypass disable" even in cases it wouldn't be enabled by default)

If your system is using grub, you can set the boot parameters by editing `/etc/default/grub` and changing the `GRUB_CMDLINE_LINUX_DEFAULT` variable. Afterwards be sure to run `sudo update-grub` or equivalent and then reboot.

Verify the currently used mitigations with:

```bash
# Print kernel command line
cat /proc/cmdline

# print the mitigations in use for each vulnerability
tail /sys/devices/system/cpu/vulnerabilities/*
```

## Benchmarks

### LEBench

Run:
```
./lebench.py
```

After every iteration, the script will print the running mean and standard deviation, plus an estimate of the 95%-confidence interval (only valid after 30 iterations). Stop the script with Ctrl-C.

### Octane 2

The benchmark uses selenium to run many iterations of the Octane 2 benchmark and aggregate the results. To install:

```bash
sudo apt-get update
sudo apt-get install -y python-is-python3 python3-selenium python3-numpy firefox git
wget https://github.com/mozilla/geckodriver/releases/download/v0.30.0/geckodriver-v0.30.0-linux64.tar.gz
tar -xzf geckodriver-v0.30.0-linux64.tar.gz
sudo mv geckodriver /usr/local/bin/
```

Run:

```bash
./bench.py 2> warnings.txt
```

By default this runs 100 iterations of each configuration. The resulting scores will be written to the terminal in the form 

> n = _num_iterations_
>
> No mitigations: mean_score (standard_deviation)
>
> Just index_masking: mean_score2 (standard_deviation2)
>
> index_masking + object_mitigations: mean_score3 (standard_deviation3)
>
> All mitigations: mean_score4 (standard_deviation4)


If you don't redirect stderr, you may get a bunch of "DeprecationWarning: firefox_profile has been deprecated, please pass in a Service object" warnings printed to the terminal.

### PARSEC
Download the PARSEC benchmarks:
```bash
wget http://parsec.cs.princeton.edu/download/2.1/parsec-2.1.tar.gz && \
wget http://parsec.cs.princeton.edu/download/2.1/binaries/parsec-2.1-amd64-linux.tar.gz && \
tar -xzf parsec-2.1-amd64-linux.tar.gz && \
tar -xzf parsec-2.1.tar.gz -C parsec-2.1 --strip-components=1
```

Fixup a couple of directories:
```bash
mv parsec-2.1/pkgs/apps/facesim/inst/amd64-linux.gcc.pre parsec-2.1/pkgs/apps/facesim/inst/amd64-linux.gcc && \
mv parsec-2.1/pkgs/apps/swaptions/inst/amd64-linux.gcc.pre parsec-2.1/pkgs/apps/swaptions/inst/amd64-linux.gcc && \
mv parsec-2.1/pkgs/apps/bodytrack/inst/amd64-linux.gcc.pre parsec-2.1/pkgs/apps/bodytrack/inst/amd64-linux.gcc 
```
Then `cd` into the parsec-2.1 directory and run this script:
```bash
#!/bin/bash

export EXP="auto"

./bin/parsecmgmt -a run -p facesim -i native -s "time -p" > ${EXP}.facesim.0
./bin/parsecmgmt -a run -p facesim -i native -s "time -p" > ${EXP}.facesim.1
./bin/parsecmgmt -a run -p facesim -i native -s "time -p" > ${EXP}.facesim.2

./bin/parsecmgmt -a run -p bodytrack -i native -s "time -p" > ${EXP}.bodytrack.0
./bin/parsecmgmt -a run -p bodytrack -i native -s "time -p" > ${EXP}.bodytrack.1
./bin/parsecmgmt -a run -p bodytrack -i native -s "time -p" > ${EXP}.bodytrack.2

./bin/parsecmgmt -a run -p swaptions -i native -s "time -p" > ${EXP}.swaptions.0
./bin/parsecmgmt -a run -p swaptions -i native -s "time -p" > ${EXP}.swaptions.1
./bin/parsecmgmt -a run -p swaptions -i native -s "time -p" > ${EXP}.swaptions.2
```

### VM Benchmarks
Install more dependencies (replacing _myusername_ as appropriate):
```bash
sudo apt-add-repository multiverse
sudo apt-get install vagrant virtualbox
sudo usermod -a -G kvm myusername    
```

Run:
```bash
bash filetest.sh
```
And:
```bash
./vm.sh 
```

# ⚠️Warning️⚠️

<p>This tool is only for educational purpose. If you use this tool for other purposes except education we will not be responsible in such cases.</p>

 2022 © - Error-4-You
