# MAP-Elites Hexapod Experiments

*Sferes2 experiment*: MAP-Elites code for developing hexapod experiments similar to [Cully et al. (2015), Nature](https://github.com/resibots/cully_2015_nature). This code is not replicating the exact experiments of the Nature paper, but can be seen as a more modern implementation of the underlying algorithm.

### Citing this code

If you use our code for a scientific paper, please cite:

Antoine Cully, Jeff Clune, Danesh Tarapore, and Jean-Baptiste Mouret. **"Robots that can adapt like animals."** *Nature 521, no. 7553 (2015): 503-507*.

In BibTex:
  
    @article{cully_robots_2015,
        title = {Robots that can adapt like animals},
        volume = {521},
        pages = {503--507},
        number = {7553},
        journal = {Nature},
        author = {Cully, Antoine and Clune, Jeff and Tarapore, Danesh and Mouret, Jean-Baptiste},
        year = {2015}
    }

## Authors
- Author/Maintainer: Konstantinos Chatzilygeroudis
- Other contributors: Vassilis Vassiliades, Jean-Baptiste Mouret

## How to compile

### Dependencies

#### Setting up the ResiBots environment

We use the environmental `RESIBOTS_DIR` variable for easy configuration and library finding (it basically points to one folder where every local installation happens that is related to the project). Thus, before installing/compiling anything, make sure that you add this line to your `~/.bashrc` or `~/.zshrc` file:

```bash
export RESIBOTS_DIR=/path/to/resibots/installation/folder
```

From now on, we assume that the `RESIBOTS_DIR` variable is set.

#### Installing DART

To simulate our hexapod we are using the [DART] simulator. To install use the following procedure:

```bash
sudo apt-add-repository ppa:libccd-debs/ppa
sudo apt-add-repository ppa:fcl-debs/ppa
sudo apt-add-repository ppa:dartsim/ppa
sudo apt-get update

sudo apt-get install build-essential cmake pkg-config git
sudo apt-get install libeigen3-dev libassimp-dev libccd-dev libfcl-dev libboost-regex-dev libboost-system-dev libode-dev
sudo apt-get install libopenscenegraph-dev

sudo apt-get install libtinyxml-dev libtinyxml2-dev
sudo apt-get install liburdfdom-dev liburdfdom-headers-dev

cd /path/to/tmp/folder
git clone git://github.com/dartsim/dart.git
cd dart
git checkout v6.3.0

mkdir build
cd build
cmake -DDART_ENABLE_SIMD=ON ..
make -j4
sudo make install
```

#### Installing the hexapod common files

In order to simulate the hexapod you nead to get the URDF file and the controller library:

```bash
cd /path/to/tmp/folder
git clone https://github.com/resibots/hexapod_common.git
cd hexapod_common/hexapod_models
./waf configure --prefix=$RESIBOTS_DIR
./waf install
cd ../hexapod_controller
./waf configure --prefix=$RESIBOTS_DIR
./waf
./waf install
```

#### Installing the DART wrapper

To facilitate the simulation we have created a simple wrapper over the DART simulator that is specific to our hexapod robot:

```bash
cd /path/to/tmp/folder
git clone https://github.com/resibots/hexapod_simu.git
cd hexapod_simu/hexapod_dart
./waf configure --prefix=$RESIBOTS_DIR
./waf
./waf install
```

#### Installing sferes's dependencies

```bash
sudo apt-get update
sudo apt-get install libeigen3-dev libboost-serialization-dev libboost-filesystem-dev libboost-test-dev libboost-program-options-dev libboost-thread-dev libboost-regex-dev libboost-graph-dev
sudo apt-get install libtbb-dev
```

### Compiling the experiment

- Get [Sferes2]: `git clone https://github.com/sferes2/sferes2.git`
- Go to your `sferes2` root directory
- Go to the modules folder: `cd modules`
- Get the map elites module: `git clone https://github.com/sferes2/map_elites.git`
- Go back to your `sferes` directory
- Configure for compilation: `./waf configure`
    - You can add other options if you want, like: `--cpp11=yes` to force C++11 compilation
- Compile sferes2 with: `./waf`
- Create an experiment folder (if there's none) and cd to it: `mkdir exp && cd exp`
- Clone map_elites_hexapod_v2: `git clone https://github.com/resibots/map_elites_hexapod_v2.git`
- Go back to your `sferes2` root directory
- Configure the experiment: `./waf configure --exp map_elites_hexapod_v2`
- Compile the experiment: `./waf --exp map_elites_hexapod_v2`

## How to run

- Compile the experiment (as shown above)
- Run it (assuming you are on sferes2 root dir and the `RESIBOTS_DIR` folder is set properly):
    - `./build/exp/map_elites_hexapod_v2/hexa_duty_text` for text archives
    - `./build/exp/map_elites_hexapod_v2/hexa_duty_binary` for binary archives

## Funding

This work has been funded by the European Research Council (ERC) under the European Union’s Horizon 2020 research and innovation programme (grant agreement number 637972 - ResiBots).

## LICENSE

[CeCILL]

[CeCILL]: http://www.cecill.info/index.en.html
[DART]: https://github.com/dartsim/dart
[Sferes2]: https://github.com/sferes2/sferes2
