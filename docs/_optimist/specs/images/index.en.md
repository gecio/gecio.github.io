---
title: Images
lang: "en"
permalink: /optimist/specs/images/
parent: Specifications
nav_order: 9500
---

# Images

There are 4 types of images in OpenStack:

- **Public Images:** These images are maintained by us, available to all users, regularly updated and recommended for use.
- **Community Images:** Previously public images, which have been superseded by newer versions. We're keeping these images until they're no longer in use, so as not to compromise your deployments.
- **Private Images:** Images uploaded by you that are only available to your project.
- **Shared Images:** Private images, which are either shared by you, or with you, across multiple different projects.

Only the first two types are maintained by us.

## Public and community images

For your convenience, we're providing you with a number of selected images.

The current list of images is as follows:

- Ubuntu 22.04 LTS (Jammy Jellyfish)
- Ubuntu 20.04 LTS (Focal Fossa)
- Ubuntu 18.04 LTS (Bionic Beaver)
- Debian 11 (Bullseye)
- Debian 10 (Buster)
- CentOS 8
- CentOS 7
- CoreOS (stable)
- Flatcar Linux
- Windows Server 2019 (GUI/Core)

These images are checked for new releases daily. The latest available version is always a public image, and contains the `Latest`-suffix. All previous versions of an imags are automatically converted to "community images", renamed (`Latest` is replaced by the date of the first upload), and eventially deleted if they are no longer in use at all.

OpenStack and many deployment tools support using these images either by name or by their UUID. By using a name, for example `Ubuntu 22.04 Jammy Jellyfish - Latest`, you can easily stay up to date by redeploying or rebuilding your instances, even if we replace the image in the interim. You can avoid this behaviour by using the UUID instead. This may be useful for cluster deployments, where you want to ensure that all nodes are running the same version of the image.

## Linux Images

All of our provided linux images are unmodified and come directly from their official maintainers. We test them during the upload process to ensure they are deployable.

## Windows Images

### What's inside?

Sadly, there are no prebuilt images for windows deployments, so we built our own. Our changes are minimal, just enough to allow easy use within our instances.

Our images are based on a regular installation of Windows Server 2019 standard edition, version 1809 (LTSC). We have added the latest drivers for our virtualization infrastructure, for the network card and storage.

Next, we installed the most recent OpenSSH build for windows, and the most recent version of PowerShell. Both are required for the following provisioning steps, and to allow you to initially connect to your instance.

We also enabled the RDP service, which is required for remote desktop connections. Don't forget to add the required security groups for this, and be sure to limit access as much as possible. We have also disabled AutoLogon for security reasons.

Our images also come with Spectre and Meltdown mitigations enabled. Additionally we had to disable the random MAC address generator, since our virtual networks enforce fixed MAC addresses.

For your convenience and security, we provide these windows images with the latest cumulative updates for Windows and the .NET framework. After booting up an instance, you'll most likely only have to update the Windows Defender definitions.

Finally, we optimized the available DotNetAssemblies, added firewall rules to allow ICMP echo replies and installed cloud-init. The latter is responsible for adding your ssh keys to the new instances.

### How to use them?

Almost as easy as deploying a linux instance. Add your ssh key to OpenStack (CLI or Dashboard) and deploy the instance. You can then connect to the instance with the following command:

```bash
ssh -i ~/.ssh/id_rsa $instanceIP -l Administrator
```

From here, you can set a password for your Administrator account for use with Remote Desktop:

```bash
net user Administrator $password
```

We strongly discourage using the previous method, adding `admin_pass` to the instances metadata. This is not encrypted or protected in any way, and is not guaranteed to work due to password security requirements.

**Be aware:** Our Images come **without** product keys or licenses. You will have to provide your own.

## Uploading your own images

Instead of using the images we provide, you can upload your own images. Easiest way of doing so is to use the OpenStack CLI.

```bash
openstack image create \
  --property hw_disk_bus=scsi \
  --property hw_qemu_guest_agent=True \
  --property hw_scsi_model=virtio-scsi \
  --property os_require_quiesce=True \
  --private \
  --disk-format qcow2 \
  --container-format bare \
  --file ~/my-image.qcow2 \
  my-image
```

The command to upload images requires these fields at a minimum:

- `--disk-format`: qcow2, in this case. This depends on the image format.
- `--file`: The source file on your machine
- Name of the Image: `my-image` for example.

Additionally, to enable the creation of Snapshots on running Instances, we recommend that you set `--property hw_qemu_guest_agent=True` on the images you create, and to install the `qemu-guest-agent` upon creation of the new image. See our [FAQ](https://docs.gec.io/de/optimist/faq/#why-am-i-unable-to-create-a-snapshot-of-a-running-instance) for more details.

You can also use the dashboard to upload images. Make sure to use the same properties there.
