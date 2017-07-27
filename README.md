# OpenSatelliteProject
Project Information
TODO: Write this page.

# Compiling Guide (Ubuntu 16.04)

That compile guide is a adapted version from @hdoverobinson that was published on gist and OSP Rocket chat. He also created a script to automatically download deps and build OSP! You can check it here: [https://gist.github.com/hdoverobinson/82e3f08c34052d36c92e8db5a027d129](https://gist.github.com/hdoverobinson/82e3f08c34052d36c92e8db5a027d129)

1. Download needed dependencies
```bash
# Create working dir
mkdir -p OSP/deps
mkdir -p OSP/bin
cd OSP/deps
# Add upstream drivers and gnuradio modules from myriadrf
sudo add-apt-repository ppa:myriadrf/drivers -y
sudo add-apt-repository ppa:myriadrf/gnuradio -y
# Add Mono Upstream key
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
echo "deb http://download.mono-project.com/repo/debian xenial main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list
# Update APT Cache
sudo apt-get update
# Install required deps
sudo apt-get -y install libairspy-dev libusb-1.0-0-dev libhackrf-dev libhackrf0 libaec0 libaec-dev mono-complete monodevelop nuget libopenal-dev referenceassemblies-pcl ttf-mscorefonts-installer gtk-sharp3 build-essential git
# Install MonoGame (Not required for Web Version)
wget http://www.monogame.net/releases/v3.5.1/monogame-sdk.run
chmod +x monogame-sdk.run
sudo ./monogame-sdk.run
# Get back to root of OSP folder
cd .. 
```

2. Compiling Demodulator and Decoder
```bash
# Clone xritdemod repository
git clone https://github.com/opensatelliteproject/xritdemod.git
# Enter on folder
cd xritdemod
# Compile / Install libcorrect
make libcorrect
sudo make libcorrect-install
# Compile / Install libSatHelper
make libSatHelper
sudo make libSatHelper-install 
# Compile / Install upstream librtlsdr
make librtlsdr
sudo make librtlsdr-install 
# Compile / Test xritdemod
make
make test
# Copy binares to bin folder
cp decoder/build/xritDecoder demodulator/build/xritDemodulator ../bin
# Back to OSP folder
cd ..
```

3. Compiling Decompressor Lib
That library is needed for decompressing LRIT/HRIT Files. It's a wrapper to `libaec`.
```bash
# Clone Decompressor repository
git clone https://github.com/opensatelliteproject/decompressor.git
cd decompressor
# Create build folder and makefile scripts
mkdir -p build
cd build
cmake ..
# Build
make
# Install
sudo make install
sudo ldconfig
# Back to OSP Folder
cd ../..
```

4. Compiling GOES Dump
```bash
# Clone goesdump repository
git clone https://github.com/opensatelliteproject/goesdump.git
cd goesdump
# Restore Nuget dependencies
nuget restore goesdump.sln
# Build GOES Dump
mdtool build goesdump.sln -c:Release
# Copy Binaries to binary folder
cp goesdump/bin/Release/* ../bin
# Back to OSP Dir
cd ..
```

5. Get the Web Interface

The web interface package can be downloaded here: https://github.com/opensatelliteproject/goesdump/releases/tag/1.0.2-beta named as `goes-web.zip`. Just extract all content under a `web` folder inside `OSP/bin`.

```bash
# Download it
wget https://github.com/opensatelliteproject/goesdump/releases/download/1.0.2-beta/goesdump-web.zip
mkdir OSP/bin/web -p

# Extract it
unzip goesdump-web.zip -d OSP/bin/web
```

Now you should have all needed binaries under `OSP/bin` folder.

# Support
There is a RocketChat page where you can ask questions and suggest features: [https://osp.teske.net.br/](https://osp.teske.net.br/) , there is also a forum that you can ask your questions and see tutorials / setups from other members: [http://hearsat.online/viewforum.php?f=18](http://hearsat.online/viewforum.php?f=18)

# GOES Dump Configuration File

Now GOES Dump config file is a SQLite database named `config.db`. All configs can be changed on WebUI and you can also open the `config.db` with [SQLite Browser](http://sqlitebrowser.org/)

# Configuring Syslog for GOES Dump Headless

Edit the file `/etc/rsyslog.conf` and find this lines:
```
# provides UDP syslog reception
#module(load="imudp")
#input(type="imudp" port="514")
```

Uncomment the last two save and restart rsyslog:
```
# provides UDP syslog reception
module(load="imudp")
input(type="imudp" port="514")
```

`service rsyslog restart`
