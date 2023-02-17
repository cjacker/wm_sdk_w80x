# About 

SDK for WinnerMicro W800/W801

For W806, please use [wm-sdk-w806](https://github.com/IOsetting/wm-sdk-w806)

# File Structure

```
wm-sdk-w80x
├── app         # User application code
├── bin         
├── demo        # Demos
├── doc         # API documents
├── include
├── ld          # Link scripts
├── lib         # Libraries release in binary
├── Makefile
├── platform    # SDK source code
├── README.md
├── src
└── tools       # Utilities
```

# For Linux Users

## Download Toolchain
I upload the toolchain to github, you can download it from: https://github.com/cjacker/wm_sdk_w80x/releases/download/init/csky-elfabiv2-tools-x86_64-minilibc-20210423.tar.gz

Or download it from https://occ.t-head.cn/community/download.

## Installation

Extract the toolchains to proper folder -- be careful that the tar ball use `./` as top level path, move it to a seperate folder or specify a target folder for the uncompressing.
```bash
mkdir csky-elfabiv2-tools-x86_64-minilibc-20210423
tar xvf csky-elfabiv2-tools-x86_64-minilibc-20210423.tar.gz  -C csky-elfabiv2-tools-x86_64-minilibc-20210423/
```
Then move it to somewhere, e.g. /opt/toolchains, set it read-only to normal users
```bash
cd /opt/toolchains/
sudo mv ~/Download/csky-elfabiv2-tools-x86_64-minilibc-20210423/ .
sudo chown -R root:root csky-elfabiv2-tools-x86_64-minilibc-20210423/
```
You don't need to add it to the system PATH variable.

## Building

Checkout this project
```bash
git clone https://github.com/cjacker/wm_sdk_w80x.git
```

Then build the project
```bash
cd wm-sdk-w80x
make
```

## Configuration
Run menuconfig
```bash
cd wm-sdk-w80x
make menuconfig
```

## Help
```bash
make help
```

## Download To Development Board

Connect the development board to PC, get the USB port name by commands `dmesg`, `lsusb` and `ls /dev/tty*`.

Run menuconfig to set the download port
```bash
cd wm-sdk-w80x
make menuconfig
```
In menuconfig, navigate to `Download Configuration`
* Download port: Input the USB port name, e.g. `ttyUSB0`;
* Down rate: Input the UART baud rate, the higher rate the higher download speed. The availabe options are `115200`, `460800`, `921600`, `1000000` and `2000000`.

Then save and exit menuconfig, download the hex file to development board
```bash
make flash
```

Press the `Reset` key to start the downloading. If previously downloaded hex was built with `USE_UART0_AUTO_DL` enabled, the board will start downloading automatically.
```
build finished!
connecting serial...
serial connected.
wait serial sync.........         <--- Press the Reset key here
please manually reset the device. <--- (Or here)
.....
serial sync sucess.
mac CC-CC-CC-CC-CC-CC.
start download.
0% [###] 100%
download completed.
reset command has been sent.
```
When downloding finishes, the board will be reset automatically to run the new program. In case the auto-reset fails, you need to press the reset key manually to make it run.

### More Download Options

Show serial ports
```bash
make list
```
Build, download and start serial monitor
```bash
make run
```
Start serial monitor only
```bash
make monitor
```

