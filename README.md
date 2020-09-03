# quickOCP4.4

RM/all VMS (take note of mac addresses)

## Get Binaries | CLI

```bash

## Copy append-boostrap.ign oout & create dir
cd ocp-dev
cp append-bootstrap.ign ../
cd ..
rm -rf ocp-dev
mkdir ocp-dev

##get binaries
wget -c https://mirror.openshift.com/pub/openshift-v4/clients/ocp/4.4.16/openshift-client-linux-4.4.16.tar.gz
wget -c https://mirror.openshift.com/pub/openshift-v4/clients/ocp/4.4.16/openshift-install-linux-4.4.16.tar.gz
tar -xvf openshift-install-linux-4.4.16.tar.gz
tar -xvf openshift-client-linux-4.4.16.tar.gz
sudo cp oc /usr/local/bin/
sudo cp kubectl /usr/local/bin/
```
## install-config + hosting
```bash
## copy install-config.yaml to dir
cp install-config.yaml ocp-dev/
./openshift-install create-manifests --dir=./ocp-dev
sed -i "s/true/false/g" ocp-dev/manifest/cluster-scheduler-02-config.yml
./openshift-install create ignition-configs --dir=./ocp-dev
cp append-bootstrap.ign ocp-dev/append-bootstrap.ign
cd ocp-dev
base64 -w0 append-bootstrap.ign > append-bootstrap.base64
base64 -w0 master.ign > master.base64
base64 -w0 worker.ign > worker.base64
cat append-bootstrap.base64
cat master.base64
cat worker.base64

```
## boot nodes with ign file

## after
```bash
./openshift-install --dir=./ocp-dev wait-for bootstrap-complete --log-level debug
pwd
export KUBECONFIG=
oc get nodes
oc get csr -o json | jq -r '.items[] | select(.status == {} ) | .metadata.name' | xargs oc adm certificate approve
```
