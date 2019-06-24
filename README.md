# ndnSIM

## Installation guide - Ubuntu 16.04
In June 2019, installing ndnSIM caused problems since there was a problem with the latest commit from `git clone https://github.com/named-data-ndnSIM/ns-3-dev.git ns-3`

Some of the errors faced: 
```
  visualizer/base.py", line 134
  print("Could not load plugin %r: %s" % (filename, str(ex)),
  file=sys.stderr)
```
Which can be fixed by editing the file in visualizer/base.py in line 134 from `file=sys.stderr` to `file(sys.stderr)`
After fixing this error, another error saying:
``` 
visualizer/core.py", line 1875, in start
import sys
RuntimeError: maximum recursion depth exceeded while calling a Python object
This method of installation will not cause such errors. 
```
Follow the steps below to get rid of these problems.

#### Installing dependencies
```
sudo apt install build-essential libsqlite3-dev libboost-all-dev libssl-dev git python-setuptools castxml
sudo apt install python-dev python-pygraphviz python-kiwi python-gnome2 ipython libcairo2-dev python3-gi libgirepository1.0-dev python-gi python-gi-cairo gir1.2-gtk-3.0 gir1.2-goocanvas-2.0 python-pip
sudo pip install pygraphviz pycairo PyGObject pygccxml
```
#### Downloading ndnSIM source
```
git clone https://github.com/named-data/ndn-cxx.git ndn-cxx
git clone https://github.com/named-data-ndnSIM/pybindgen.git pybindgen
git clone https://github.com/named-data-ndnSIM/ns-3-dev.git ns-3
```
Since there are problems from the current commit, we need to clone from a specific commit.

```
cd ndnSIM/ns-3
git reset --hard 7c4662ff285290a9a10a27b6f6682e0678c29907
```
```
cd ndnSIM
git clone --recursive https://github.com/named-data-ndnSIM/ndnSIM.git ns-3/src/ndnSIM
```
#### Compiling and running ndnSIM
```
cd ndnSIM/ndn-cxx
./waf configure --enable-shared --disable-static
./waf
sudo ./waf install
sudo ldconfig
cd ndnSIM/ns-3
./waf configure --enable-examples
./waf
```
#### Simulating using ndnSIM
Using the examples already included
`./waf --run=ndn-simple`
`./waf --run=ndn-simple --vis` - run with visualizer
`NS_LOG=ndn.Consumer:ndn.Producer ./waf --run=ndn-simple --vis` - run with visualizer & Logging
`NS_LOG=ndn.Consumer:ndn.Producer ./waf --run=ndn-simple --vis > debug.out 2>&1` - run with visualizer & Logging in separate file (debug.out)

Error encountered (unsolved-did not use vis):
```
Could not load icon applets-screenshooter due to missing gnomedesktop Python module ...... AttributeError: 'X11Display' object has no attribute 'get_primary_monitor'
```

# References
* http://ndnsim.net/current/getting-started.html
* https://atifurrehman.blogspot.com/2018/07/ndnsim-installation-guide-ubuntu-1604.html
* https://www.lists.cs.ucla.edu/pipermail/ndnsim/2019-May/005319.html
* https://github.com/named-data-ndnSIM/ns-3-dev/commits/ndnSIM-ns-3.29


