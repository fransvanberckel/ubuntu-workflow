# Tinkerbell Ubuntu workflow

```
ubuntu-workflow/
├── LICENSE
├── README.md
└── ubuntu
    ├── 00-base
    │   ├── debian-cleanup.sh
    │   ├── Dockerfile
    │   ├── functions.sh
    │   ├── get-package-list.sh
    │   └── yq.list
    ├── 01-disk-wipe
    │   ├── Dockerfile
    │   └── wipe.sh
    ├── 02-disk-partition
    │   ├── cpr.sh
    │   ├── Dockerfile
    │   └── partition.sh
    ├── 03-install-root-fs
    │   ├── chroot-script
    │   ├── config
    │   ├── Dockerfile
    │   ├── packages
    │   └── root-fs.sh
    ├── 04-cloud-init
    │   ├── cloud-init.sh
    │   └── Dockerfile
    ├── build_and_push_images.sh
    ├── create_tink_workflow.sh
    ├── focal.yml
    ├── hardware.json
    ├── pack_grml_tar_gz.sh
    └── verify_json_tweaks.sh
```

About the Tinkerbell Ubuntu workflow it's based on Grml-debootstrap. It doesn't try to process disk images any more. Just installing old plain deb packages. Because of the need to setup the same packages over and over again, (especifically in cace of installing multiple Ubuntu instances) this workflow has a dependency on apt-cacher-ng, which is installed as a Docker container. It doesn't work without! It uses the default port 3142. And cache all the deb packages nicely.

There's one available writen for [Debian](https://github.com/fransvanberckel/debian-workflow) as well.

## Compatibility

This configuration has been tested with the following hardware and OS combinations:

- HP ProBook 6560b and Ubuntu 18.04 (64-bit)
- HP ProLiant DL2000 Node Rack Server module and Debian Bullseye (64-bit)

I am open for cloud instances to test, when they become available.

## Usage

First, before you start running the workers workflow, you need to make adjustments to, where needed ...

- ubuntu/focal.yml
- ubuntu/hardware.json
- ubuntu/build_and_push_images.sh
- ubuntu/create_tink_workflow.sh

Next verify, build, pack & create the environment.
```
$ ./verify_json_tweaks.sh
$ sudo ./build_and_push_images.sh
$ sudo ./pack_grml_tar_gz.sh
$ sudo cp ./grml-ubuntu.tar.gz /var/tinkerbell/state/webroot/misc/osie/current/
$ sudo ./create_tink_workflow.sh

Enter new root password:
Enter new user password:
Enter new password salt:
Creating new Tinkerbell worker environment
2020/08/07 05:56:07 Hardware data pushed successfully
Created Template: 95f948b6-cf87-4d64-bb56-1f5087ae6588
Created Workflow: 508569a3-0275-4f50-b957-51d4de6c21ae
```

## Notes

* All workflow actions run as `--privileged` Docker containers.

## Author

The repository was created in 2020 by [Frans van Berckel](https://www.fransvanberckel.nl)
