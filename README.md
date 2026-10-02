# Fedora Forensics Workstation

A documented digital forensics workstation built on Fedora Linux, used for
learning disk forensics with open source tools on public test images.

## Purpose

I am a second year Computer Forensics and Security student at SETU Waterford.
This repo records how I built my forensics workstation, why I chose each tool,
and worked examples on public datasets, written up the way I would write up
a real case.

## Hardware and host

| Item | Detail |
| --- | --- |
| Laptop | ASUS Vivobook, 24 GB RAM |
| Host OS | Fedora 44 |
| Virtualisation | KVM/virt-manager |
| Case storage | Internal SSD, ~/evidence/, kept outside this repo |
| Test media | 16 GB USB sticks, wiped and used only for practice imaging |

## What is in this repo

| Path | Contents |
| --- | --- |
| `docs/setup.md` | Every tool installed, how, and why |
| `case-001/` | Worked example on a public SD card image |
| `screenshots/` | Screenshots referenced in the write ups |

## Rules I work to

* Public datasets only. No real case data, ever.
* Every image is hashed before and after analysis.
* Evidence files are never committed to this repo.

## Next steps

* Document my write blocking approach and test it on a USB stick
* Add evidence handling notes: naming, hashing, storage
* Case 002: image and analyse a practice USB stick
* Case 003: a NIST CFReDS image
* Add a Windows artefact example from the Windows VM
