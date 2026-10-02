# Workstation setup

## Host

Fedora 44, kernel 7.2, 24 GB RAM on an ASUS Vivobook. Fedora is my daily
driver. It is close to RHEL, which is common in enterprise, and keeps me
working on the command line.

## The Sleuth Kit

* **Version:** 4.15.0
* **Installed with:** `sudo dnf install sleuthkit`
* **What it does:** command line tools for file system analysis
  (`mmls`, `fsstat`, `fls`, `icat`, `istat`).
* **Why:** it is the engine under Autopsy, and using it directly shows
  exactly what the GUI is doing. Output is plain text, so it goes straight
  into case notes.

## Autopsy

* **Version:** 4.22.1
* **Installed with:** the official snap, `sudo snap install autopsy`,
  then connecting its interfaces so it can read disk images.
* **What it does:** GUI case management over The Sleuth Kit, with ingest
  modules for hashing, file type detection, EXIF, keyword search and
  carving.
* **Why:** free, widely used in training, and similar in workflow to
  commercial tools.
* **Problems I hit:** Autopsy is not packaged for Fedora. The Fedora
  `sleuthkit` package has no Java bindings (`libtsk_jni`), so Autopsy could
  not talk to it. The snap bundles a matched Java and Sleuth Kit, which
  fixed it. I kept the dnf Sleuth Kit for command line work.

## ewftools (libewf)

* **Version:** 20140608
* **Installed with:** `sudo dnf install ewftools`
* **What it does:** `ewfinfo` reads E01 metadata, `ewfverify` checks the
  acquisition hash stored inside the image.
* **Why:** E01 is the standard evidence format, and verifying an image
  before analysis is basic integrity practice.

## GHex

* **Installed with:** `sudo dnf install ghex`
* **Why:** a simple hex editor for checking file signatures and raw
  bytes by hand.

## KVM and virt-manager

* **Version:** libvirt 12.0.0
* **Why:** built into the Linux kernel, so no third party kernel modules.
  Snapshots let me roll a VM back to a clean state after each exercise.

## Kali Linux VM

* **Version:** Kali Linux Rolling
* **Specs:** 5 vCPUs, 8 GB RAM, 128 GB thin provisioned qcow2 disk
  (about 40 GB used)
* **Network:** NAT through the libvirt default network, virtio adapter
* **Why:** a disposable environment for tools I do not want on the host.

## Windows VM

* **Version:** Windows 11 25H2
* **Specs:** 4 vCPUs, 8 GB RAM, 128 GB thin provisioned qcow2 disk
  (about 21 GB used)
* **Network:** NAT through the libvirt default network, e1000e adapter
* **Why:** most artefacts I will examine are Windows. Used for Windows only
  tools and for generating test artefacts.

## Hashing

`sha256sum` and `md5sum` from coreutils. SHA256 is my primary hash. MD5 is
recorded because older images and tools still publish it.
