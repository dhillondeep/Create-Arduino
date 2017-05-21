# Create Arduino Projects

## About

[Bare Arduino Project](https://github.com/ladislas/Bare-Arduino-Project) let's you create Arduino projects without using Arduino IDE. You can use your favorite text editor such as Vim or SublimeText to do so. For further information, check their [README.md](https://github.com/ladislas/Bare-Arduino-Project/blob/master/README.md).

This repository contains a shell script that let's you create Arduino projects using Bare-Arduino-Project very easily. The focus of this repository is to create single working project while Bare-Arduino-Project focuses on creating a folder to hold bunch of arduino projects and they will only work if they are in the folder. So, Create-Arduino removes unnecessary files and gives you a simple project up and running in no time.


## How to Use

### Getting Ready - Tools to install

#### OS X

Before starting we need to install `git`, `arduino` and `Homebrew`:

*   [Arduino IDE 1.0.x or 1.6.x](http://arduino.cc/en/main/software#toc2) - Download the app from the website
*   [Homebrew](http://mxcl.github.io/homebrew/) - Follow the instructions on their website
*   [Git](http://git-scm.com/) - use `brew install git` to install the latest version

#### Linux

Before starting we need to install `git` and `arduino`:

```Bash
# First add the git-core ppa
$ sudo add-apt-repository ppa:git-core/ppa

# Update list
$ sudo apt-get update && sudo apt-get upgrade

# Install git 2.x.x and Arduino 1.0.x
$ sudo apt-get install git arduino
```

### 1. Install `avr-gcc`, `binutils`, `avr-libc` and `avrdude`

#### OS X

```Bash
$ brew tap osx-cross/avr
$ brew install avr-libc
$ brew install avrdude
```

Check that everything has been installed properly by running `avr-gcc -v` and `avrdude -v`.

#### Linux

```Bash
$ sudo apt-get install gcc-avr binutils avr-libc avrdude
```

Make sure everything is up and running by running `avr-gcc -v` and `avrdude -v`.

### 2. Clone `Create-Arduino` repository from Github

Simply clone the repo to your desired location:

```Bash
$ git clone https://github.com/dhillondeep/Create-Arduino
```

Go to Create-Arduino folder and run create-arduino shell script

This script takes in two parameters. The first parameter is the name of the project and the second parameter is the location of the project.

```Bash
$ ./create-arduino PROJECT_NAME PROJECT_LOCATION
```

### 3. Install `pySerial`

To upload the program, we need to reset the Arduino board. This is done using a `python script` stored in `Arduino-Makefile/bin` which is inside the project you just created

First, if you don't already have Python, you can install it

#### OS X
```Bash
$ brew install python
```

Then install `pySerial`:

```Bash
$ pip install pyserial
```

### 4. Copy and edit a `Makefile`

To compile arduino code, we need to have a Makefile for the project we want to compile

`cd` to `src` folder of your project:

```Bash
$ cd src
```

Then copy the `Makefile-Example.mk`:

```Bash
# if you are on OS X
$ cp ../Makefile-OSX.mk ./Makefile

# or if you're running Linux
$ cp ../../Makefile-Linux.mk ./Makefile
```

`Makefile` has a `PROJECT_DIR` which defines the full path of the root project folder. The way it has been setup from default will not work in our case. You have to edit it to following:

```Bash
PROJECT_DIR       = $(abspath $(shell basename $(CURDIR))/../..)
```

You can modify the `Makefile` to suit your needs:

* `PROJECT_DIR` is the full path to the root project folder
.
* `BOARD_TAG` & `BOARD_SUB` define the target board you are compiling to. `BOARD_SUB` is only used in the most recent versions of the IDE (and not in Arduino 1.0.x). To find the proper values:
  * Open the board definitions file (see `BOARDS_TXT` path when `make` is launched)
  * Find for the board used (for example *"Arduino Pro or Pro Mini (3.3V, 8 MHz) w/ ATmega328"*)
  * Look at the config keys for this board (in this case `pro.menu.cpu.8MHzatmega328=ATmega...`)
  * So `BOARD_TAG = pro` and `BOARD_SUB = 8MHzatmega328`
* `MONITOR_PORT` is the device full path (required if you want to upload to the board). An example is `/dev/tty.usbserial-A20356BI`

### 6. Write Arduino Code

`Main.cpp` inside `src/Main` folder of your project is empty and does not contain any code. You have to write Arduino code in order for the project to compile.

Sample code

```c++
#include <Arduino.h>

void setup() {
	Serial.begin(115200);
	delay(1000);
}

void loop() {
	Serial.println("Hello world");
}

```  

### 7. Compile and upload your code

Go to the folder where Makefile is stored and compile the project

```Bash
$ make
$ make upload
```

So, every time you want to create an arduino project, just call the script with appropriate parameters and it will create an Arduino project for you.

This install guide is inspired by the install guide of Bare-Arduino-Project. For the full procedure, please check [INSTALL.md](https://github.com/ladislas/Bare-Arduino-Project/blob/master/INSTALL.md).