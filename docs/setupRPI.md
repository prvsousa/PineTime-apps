# Build RIOT-OS Firmware for Raspberry PI

## Download PineTime-apps and build
```
git clone --recursive https://github.com/bosmoment/PineTime-apps
cd PineTime-apps/apps/pinetime
make all
```

## Install tools RPI
```
rm -rf ~/pinetime-rust-newt
rm -rf ~/openocd-spi
```
```
sudo apt install -y wget p7zip-full
cd ~
wget https://github.com/lupyuen/pinetime-rust-mynewt/releases/download/v2.0.5/pinetime-rust-mynewt.7z
7z x pinetime-rust-mynewt.7z
rm pinetime-rust-mynewt.7z
```
```
cd ~/pinetime-rust-mynewt
scripts/install-pi.sh
```
```
source $HOME/.cargo/env
rustup default nightly-2020-02-16
rustup update
rustup target add thumbv7em-none-eabihf
```
## Use OpenOCD for flashing with the app build of Pinetime-apps  
```
$HOME/openocd-spi/src/openocd \
	-s $HOME/openocd-spi/tcl \
	-f $HOME/pinetime-rust-mynewt/scripts/nrf52-pi/swd-pi.ocd \
	-f $HOME/pinetime-rust-mynewt/scripts/nrf52/flash-boot.ocd
```

You should edit this part of the code of flash-boot.ocd with the path of the file generated in the first step:


```
# Bootloader address must sync with hw/bsp/nrf52/bsp.yml
echo "Flashing Bootloader..."
program /PATH/TO/BIN/FILE verify 0x00000000
echo ""
```

