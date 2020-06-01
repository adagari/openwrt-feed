# OpenWrt Feed
## Packages

* [`goeap_proxy`](https://github.com/pyther/goeap_proxy) (Go: Proxy EAP packets between network interfaces)
* [`eap_proxy`](https://github.com/jaysoffian/eap_proxy) (Python: Proxy EAP packets between network interfaces)

## How to Use (Overview)

Add the following lines to feeds.conf in OpenWrt buildroot:
```
src-git adagari git://github.com/adagari/openwrt-feed
```

Update the feed:
```
./scripts/feeds update adagari
```

Activate the package:
```
./scripts/feeds install PACKAGE_NAME
```

## Building Packages
Instructions based on OpenWrt's wiki [Using the SDK](https://openwrt.org/docs/guide-developer/using_the_sdk).

1. Download the SDK: [Insturctions](https://openwrt.org/docs/guide-developer/using_the_sdk#obtain_the_sdk). 
    ```
   wget https://downloads.openwrt.org/releases/18.06.4/targets/x86/64/openwrt-sdk-18.06.4-x86-64_gcc-7.3.0_musl.Linux-x86_64.tar.xz
   ```

2. Extract SDK, enter directory
    ```
    $ tar xf openwrt-sdk-18.06.4-x86-64_gcc-7.3.0_musl.Linux-x86_64.tar.xz
    $ cd openwrt-sdk-18.06.4-x86-64_gcc-7.3.0_musl.Linux-x86_64/
    ```

3. Update feeds.conf
    ```
    $ cp feeds.conf.default feeds.conf
    $ echo 'src-git adagari git://github.com/adagari/openwrt-feed' >> feeds.conf
    ```

4. Update and install the feeds
    ```
    $ ./scripts/feeds update -a
    $ ./scripts/feeds install -a
    ```

5. Run menuconfig
    ```
    $ make menuconfig
    ```
     Unselect
     - Cryptographically sign package lists

6. Build Package
    ```
    $ make package/goeap_proxy/download
    $ make package/goeap_proxy/prepare
    $ make package/goeap_proxy/compile
    ```

7. Package should be built and located in `./bin/packages/x86_64/adagari/goeap_proxy`

## Building OpenWrt Image

1. Download the OpenWrt Image Builder: [Instructions: Using the Image Builder](https://openwrt.org/docs/guide-user/additional-software/imagebuilder)
    ```
    $ wget https://downloads.openwrt.org/releases/18.06.4/targets/x86/64/openwrt-imagebuilder-18.06.4-x86-64.Linux-x86_64.tar.xz
    ```

2. Extract Image Builder and enter directory
    ```
    $ tar xf openwrt-imagebuilder-18.06.4-x86-64.Linux-x86_64.tar.xz
    $ cd openwrt-imagebuilder-18.06.4-x86-64.Linux-x86_64
    ```

3. Copy built package into `./packages`
    ```
    $ cp ~/build/openwrt-sdk-18.06.4-x86-64_gcc-7.3.0_musl.Linux-x86_64/bin/packages/x86_64/adagari/goeap_proxy_0.200502.3-1_x86_64.ipk ./packages
    ```

4. Build Image
    ```
    $ make image PACKAGES="goeap_proxy"
    ```

    Note: other platforms (non-x86) you mant to to specify `PROFILE=XXX`

    List of profiles can be obtained with
    ```
    $ make info
    ```
