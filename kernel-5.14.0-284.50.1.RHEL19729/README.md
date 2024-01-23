# RHCOS layering

## How to build the container-base images:

```bash
$ export OCP_VERSION="4.14.5"
$ export OCP_KERNEL="kernel-5.14.0-284.50.1.rhel19729.2.el9_2.x86_64"
$ VARIABLE_NAME=$(curl -s https://mirror.openshift.com/pub/openshift-v4/clients/ocp/$OCP_VERSION/release.txt | grep -m1 'rhel-coreos' | awk -F ' ' '{print $2}')
$ podman build -t ${OCP_KERNEL}:${OCP_VERSION} --no-cache --build-arg rhel_coreos_release=${VARIABLE_NAME} .
$ podman tag localhost/${OCP_KERNEL}:${OCP_VERSION} quay.io/jclaret/${OCP_KERNEL}:${OCP_VERSION} 
$ podman login -u="<username>" -p="<password>" quay.io
$ podman push quay.io/jclaret/${OCP_KERNEL}:${OCP_VERSION}
```

## How to apply the kernel to OCP using MachineConfig:

- for `master` nodes:

```bash
$ oc create -f 99-kernel-5.14.0-284.50.1.RHEL19729.2.el9_2-master.yaml
```

- for `worker` nodes:

```bash
$ oc create -f 99-kernel-5.14.0-284.50.1.RHEL19729.2.el9_2-worker.yaml
```
