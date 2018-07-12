# razerblade15

Razer Blade 15 (2018)

## Recompiling Kernel for touchpad support

Download kernel tarball

``` bash
wget https://git.kernel.org/torvalds/t/linux-4.18-rc4.tar.gz
```

Extract archive

``` bash
tar xvfs linux-4.18-rc4.tar.gz
```

Enter directory

``` bash
cd linux-4.18-rc4
```

  Ubuntu-specific patches

  http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.18-rc4/

  ```
  wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.18-rc4/0001-base-packaging.patch;
  patch -f < 0001-base-packaging.patch
  ```

  ```
  wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.18-rc4/0002-UBUNTU-SAUCE-add-vmlinux.strip-to-BOOT_TARGETS1-on-p.patch;
  patch -f < 0002-UBUNTU-SAUCE-add-vmlinux.strip-to-BOOT_TARGETS1-on-p.patch
  ```

  ```
  wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.18-rc4/0003-UBUNTU-SAUCE-tools-hv-lsvmbus-add-manual-page.patch;
  patch -f < 0003-UBUNTU-SAUCE-tools-hv-lsvmbus-add-manual-page.patch
  ```

  ```
  wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.18-rc4/0004-debian-changelog.patch;
  patch -f < 0004-debian-changelog.patch
  ```

  ```
  wget http://kernel.ubuntu.com/~kernel-ppa/mainline/v4.18-rc4/0005-configs-based-on-Ubuntu-4.18.0-1.2.patch;
  patch -f < 0005-configs-based-on-Ubuntu-4.18.0-1.2.patch
  ```

Apply trackpad patches

```
wget https://raw.githubusercontent.com/jbdrthapa/razerblade15/master/razerfiles/touchpad/gpiolib.patch;
patch drivers/gpio/gpiolib.c < gpiolib.patch
```

```
wget https://raw.githubusercontent.com/jbdrthapa/razerblade15/master/razerfiles/touchpad/pinctrl-intel.patch;
patch drivers/pinctrl/intel/pinctrl-intel.c < pinctrl-intel.patch
```

Install compilation dependencies

```
sudo apt-get install git build-essential kernel-package fakeroot libncurses5-dev libssl-dev ccache flex
```

Copy config from current kernel

```
cp /boot/config-`uname -r` .config
```

```
make clean
```

Compile kernel

```
make -j `getconf _NPROCESSORS_ONLN` deb-pkg LOCALVERSION=-custom
```

Go to parent directory

```
cd ../
```

Install newly built kernel

```
sudo dpkg -i *.deb
```
