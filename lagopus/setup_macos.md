Integration Test VM on MacOS
============================

Checkout
--------

```
git clone https://github.com/rakshasa/lagopus.git
#git clone git@github.com:rakshasa/lagopus.git

cd lagopus/
git checkout fix-macos
```


Update Submodule
----------------

*DO NOT USE.*

Updating DPDK version. Note that lagopus' VM doesn't work with newer DPDK.

```
git submodule update --init src/dpdk/

cd src/dpdk/
git checkout v2.2.0
cd ../../

git add src/dpdk/
git commit -m 'Update dpdk submodule.'
git submodule deinit --all
```


Dependencies
------------

```
cd test/integration_test/vm/

rvm use system

vagrant plugin install vagrant-mutate
vagrant plugin install vagrant-reload

rvm install 2.2
rvm gemset create lagopus
rvm use 2.2@lagopus

gem install nokogiri

vagrant box add lagopus_ubuntu_14.04s \
   https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box
```


Config Files
------------

Edit the following in 'conf/ansible_conf.yml':

```
lagopus:
  git: https://github.com/rakshasa/lagopus.git

...

proxy_env:
  http_proxy: http://192.168.1.foo:8888/
  https_proxy: https://192.168.1.foo:8888/
```

Use the IP address of your internet-connected interface.


TinyProxy Setup
---------------

Install and run 'tinyproxy' from macports.

```
cp /opt/local/etc/tinyproxy.conf.default /opt/local/etc/tinyproxy.conf
```

Edit 'tinyproxy.conf' and add the following:

```
LogFile "/tmp/tinyproxy.log"
PidFile "/tmp/tinyproxy.pid"

Allow 192.168.1.foo
```

Run tinyproxy.


Create VM
---------

```
vagrant up
```

