## Kernel
- The Intel® DSA Driver (IDXD) with DWQ
support was introduced in kernel version 5.6.
- The IDXD driver with SWQ support is available in Linux
upstream Kernel versions 5.18 and beyond.

-link: https://cdrdv2-public.intel.com/759709/data-streaming-accelerator-user-guide.pdf

## Prepare SWQ
### Install idxd-config
```
git clone https://github.com/intel/idxd-config.git
./autogen.sh
./configure CFLAGS='-g -O2' --prefix=/usr --sysconfdir=/etc --libdir=/usr/lib64
make
make check
make install
```

### Configure SWQ and validate function
1. Edit /etc/default/grub to add "intel_iommu=sm_on iommu=on".
2. reboot:
```
$grub-update
$reboot
```
2. Compile dsa-micro and configure SWQ
```
git clone https://github.com/intel-sandbox/dsa-perf-micros.git
./autogen.sh
./configure CFLAGS='-g -O2' --prefix=/usr --sysconfdir=/etc --libdir=/usr/lib64
make
./scripts/setup_dsa.sh -d dsa0 -w 1 -m s -e 4
./src/dsa_perf_micros -n128 -s4k -j -f -i1000 -k5-8 -w1 -zF,F -o3
```
- link: https://wiki.ith.intel.com/pages/viewpage.action?spaceKey=CRPnP&title=DSA+Micros
- link: https://github.com/intel-sandbox/dsa-perf-micros/blob/master/doc/sample_command_lines.rst

## DML

### Source code
```
git clone --recursive https://github.com/intel/DML.git
```
Note: the branch is **"develop"** and commit is **"Add UMWAIT support for C API"**.

## Compile and Run
1. change high level api example like below:
```
    --- a/examples/high_level_api/mem_move.cpp
    +++ b/examples/high_level_api/mem_move.cpp
    @@ -11,7 +11,7 @@
    
    constexpr auto size = 1024u;  // 1 KB
    
    -using execution_path = dml::software;
    +using execution_path = dml::hardware;
```
2. Compile DML
```
cd DML
mkdir build
cd build
cmake -DLOG_HW_INIT=ON ..
cmake --build . --target install -j10
```
3. Run high level api example
```
./build/examples/high_level_api/dmlhl_mem_move_example
```
