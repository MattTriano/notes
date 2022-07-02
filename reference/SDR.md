# Software Defined Radio

## RTL-SDR on Raspberry Pi

There are a number of software options that might work on the ARM chipset (which is used by Raspberry Pi boards), and none of the featureful ones seemed to be trivially easy to install, but I can confirm that [SDR++](https://github.com/AlexandreRouma/SDRPlusPlus) is pretty straightforward to set up, and it works with the very inexpensive [RTL-SDR](https://www.amazon.com/RTL-SDR-Blog-RTL2832U-Software-Defined/dp/B011HVUEME/ref=sr_1_3?crid=EOTH3R0TC3GS&keywords=rtl+sdr&qid=1656736943&sprefix=rtl+sdr%2Caps%2C459&sr=8-3) that I bought and played with a few years ago before putting it in a drawer. 

Also, this was on a freshly set up Raspberry Pi OS (version 5.15.32-v8, the 2022/03/31 aarch64 image).  

```bash
git clone https://github.com/AlexandreRouma/SDRPlusPlus.git
cd SDRPlusPlus/
```

Then you'll essentially run the commands in [`rpi_install.sh`](https://github.com/AlexandreRouma/SDRPlusPlus/blob/master/rpi_install.sh), although I ran lines commands manually (I found I had to change `libvolk1-dev` to `libvolk2-dev`, and there aren't many commands)

```bash
sudo apt update
sudo apt install -y build-essential cmake git libfftw3-dev libglfw3-dev libvolk2-dev libzstd-dev libsoapysdr-dev libairspyhf-dev libairspy-dev libiio-dev libad9361-dev librtaudio-dev libhackrf-dev librtlsdr-dev libbladerf-dev liblimesuite-dev p7zip-full wget

mkdir -p build
cd build
cmake .. -DOPT_BUILD_LIMESDR_SOURCE=ON
make
sudo make install
```

Then plug in the RTL-SDR dongle and SDR++ can be opened from the start menu, in the **Other** program group.

In the program, under the **Source** banner, select the **RTL-SDR** and then scan through frequencies. For reference, here are the frequencies for [Chicago Police radio zones](https://chicagocop.com/resources/radio-frequencies/) (as well as CFD and other Chicago public safety channels). 


