# ansible-role-rtpengine [![Build Status](https://secure.travis-ci.org/davehorton/ansible-role-rtpengine.png)](http://travis-ci.org/davehorton/ansible-role-rtpengine)

This is an ansible role for building rtpengine. 

## Role variables

Available variables are listed below, along with default values (see defaults/main.yml):
```
rtp_engine_version: master
```
branch to check out

```
rtp_engine_recording_dir: /tmp
```
directory into which to write files
```
rtp_engine_recording_method: pcap
```
type of files to write
```
rtp_engine_recording_format: eth
```
recording format
```
rtp_engine_udp_port: 12222
```
udp port
```
rtp_engine_NG_port: 22222
```
port to listen on for ng commands
```
rtp_engine_cli_port: 127.0.0.1:9900
```
cli port
```
rtp_engine_log_level: 6
```
log level
```
rtp_engine_min_port: 40000
rtp_engine_max_port: 60000
```
rtp port range
```
homer_protocol: udp
```
protocol to use for homer

```

## Example playbook
```
- hosts: all
  become: yes
  roles:
    - ansible-role-rtpengine
```


## Troubleshooting

### CMAKE can be outdate and need remove and reinstall as workaround on Debian11, this need for G729 build in `/usr/local/src/bcg729`

```
  wget https://github.com/Kitware/CMake/releases/download/v3.29.1/cmake-3.29.1.tar.gz
  tar -zxvf cmake-3.29.1.tar.gz
  sudo apt-get remove cmake
  cd cmake-3.29.1/
  ./configure
  gmake
  sudo make install
```

build with with-kernel can crashed with Debian11, workaround can be applied to pass though by
check whether there any `bpf/resolve_btfids` for your current linux build

```bash
ls -lt /usr/src/linux-headers-`uname -r`/tools/bpf/resolve_btfids
```

reinstall linux-headers could fix issue

```bash
  sudo apt install --reinstall linux-headers-$(uname -r)
  apt install libwebsockets-dev
  find linux-headers-6.1.0-0.deb11.13-cloud-amd64 | grep resolve_btfids
  ls -lta /lib/modules/$(uname -r)/build/tools/bpf/resolve_btfids
```

fetch from repos for extension tools use with kernel linux of your current linux kernel `https://mirrors.edge.kernel.org/pub/linux/kernel/v6.x/`


```bash
cd /usr/src
wget https://mirrors.edge.kernel.org/pub/linux/kernel/v6.x/linux-6.1.tar.gz
tar -zxvf linux-6.1.tar.gz
cd /usr/src/linux-6.1/tools/bpf/resolve_btfids/
make
file tools/bpf/resolve_btfids/resolve_btfids
mkdir -p /usr/src/linux-headers-`uname -r`/tools/bpf/resolve_btfids
ln -s $(realpath resolve_btfids) /usr/src/linux-headers-`uname -r`/tools/bpf/resolve_btfids
```



