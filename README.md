#rt3573sta
=========

Patched version of this ralink driver to handle sk_buff structures properly on 64-bit systems

*Forked from [ashaffer/rt3573sta](https://github.com/ashaffer/rt3573sta) this version should fix some errors when compiling on recent kernels*


##Installation

Use `git clone` to clone this repository to a directory of your choosing. Example:
```
mkdir drivers/
cd drivers/
git clone https://github.com/RD777/rt3573sta/
```

Once the directory has been cloned, procede into compiling and installing the module with `make`:
```
cd rt3573sta/
sudo make
```

If there were no errors and the compilation went smoothly, install:
```
sudo make install
```

Afterwards, load the module:
``` 
sudo modprobe rt3573sta
```
