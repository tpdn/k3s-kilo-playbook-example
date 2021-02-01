Build a secure [K3s](https://github.com/k3s-io/k3s) cluster over public network with [Kilo](https://github.com/squat/kilo)
===========================================

Straightforward playbooks, for reference, not production-ready

---

 * https://blog.tpdn.kim/entry/build-a-secure-k3s-cluster-over-public-network-ja (ja)

System requirements
-------------------
* Ansible 2.10.5+
* Ubuntu 20.04 hosts
  * Maybe it works on ubuntu 18.04
* A server(master) node
  * Not support multiple server(master) nodes
* One or more agent(worker) nodes
* Passwordless SSH access

Usage
===========
Edit configurations
-------------
* hosts.ini
* group_vars/all.yml
* id_rsa

Run ansible-playbook
---------------------
Start provisioning of the cluster using the following command:
```
ansible-playbook playbook.yml -i hosts.ini
```

See if it worked
===============
```
KUBECONFIG=files/k3s.yaml kubectl get nodes
```
```
# Log into your server node
server-node:~# wg
```


Example of expected results
------------------------
```
(ansible-venv) tpdn@tpdn-VirtualBox:~/k3s-kilo-playbook-example$ KUBECONFIG=files/k3s.yaml kubectl get nodes
NAME           STATUS   ROLES                  AGE   VERSION
agent-node-3   Ready    <none>                 25h   v1.20.2+k3s1
agent-node-1   Ready    <none>                 25h   v1.20.2+k3s1
agent-node-2   Ready    <none>                 25h   v1.20.2+k3s1
agent-node-4   Ready    <none>                 25h   v1.20.2+k3s1
server-node    Ready    control-plane,master   25h   v1.20.2+k3s1
agent-node-5   Ready    <none>                 25h   v1.20.2+k3s1
```
```
root@server-node:~# wg
interface: kilo0
  public key: 0kKfArnPxNBblekTLPHm1BMhzrAbVcTqbh9EudDa51k=
  private key: (hidden)
  listening port: 51820

peer: Jddm35Ig1PsX2RwI3i2bYvDzSAZ3kVjmqJexbnY4Dp8=
  endpoint: agent-node-1.example.com:51820
  allowed ips: 10.42.4.0/24, 10.4.0.3/32
  latest handshake: 1 minute, 9 seconds ago
  transfer: 319.90 KiB received, 480.49 KiB sent

peer: N7GH4CO4BNmwcInVDC3TNWbV7/gd3UuriDHgV75LqVR=
  endpoint: agent-node-2.example.com:51820
  allowed ips: 10.42.1.0/24, 10.4.0.5/32
  latest handshake: 2 hours, 47 minutes, 46 seconds ago
  transfer: 1.35 MiB received, 83.46 KiB sent

peer: RrCuDQhtxPr20tbR3qPmVvWFvbG9QrME5vRFWAL+0nh=
  endpoint: agent-node-3.example.com:51820
  allowed ips: 10.42.5.0/24, 10.4.0.1/32

peer: e1dsyWfUyz2PnCwKF7s2NJM5RoSP1s8b2cCM78VuguU=
  endpoint: agent-node-4.example.com:51820
  allowed ips: 10.42.3.0/24, 10.4.0.2/32

peer: TxZuEMbIXsKy07jhf3LfaSjTSEvSqEw1q67QD3uGIKx=
  endpoint: agent-node-5.example.com:51820
  allowed ips: 10.42.2.0/24, 10.4.0.4/32
```

