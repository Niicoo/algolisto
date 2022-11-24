Playing with Grub
=================

Some context, on my laptop, I have 2 SSD (nvme) and 3 OS installed (Windows, Linux Mint and Ubuntu). I wanted the following behavior:

- Linux Mint and Ubuntu have their own grub
- Linux Mint and Ubuntu don't share the same SWAP partition(s).
- Boot first in the Linux Mint Grub Menu
- Linux Mint Grub Menu:

    - Being able to open the Ubuntu Grub Menu
    - Not being able to run Ubuntu directly
    - Being able to run Windows
- Ubuntu Grub Menu:

    - Being able to open the Linux Mint Grub Menu
    - Not being able to run Linux Mint directly
    - Being able to run Windows


Get Partitions UUID
###################

When manipulating grub and boot parameters, we often need to get the UUID of the partitions, you can get it with the command:

.. code-block:: bash

    sudo blkid

For example in my case I have the following partitions:

.. code-block:: text

    /dev/nvme0n1p3: UUID="291bd5e4-dbc4-4b54-aa7b-6329d43f35ce" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="0c3719af-0ad2-4b1a-8f44-12a3ba07734b"
    /dev/nvme0n1p1: UUID="5B0D-4E6B" BLOCK_SIZE="512" TYPE="vfat" PARTLABEL="EFI System Partition" PARTUUID="e4067cfd-bfb1-42a0-bfe4-629526f151f8"
    /dev/nvme0n1p4: UUID="a1fdc046-256f-486b-a26e-0972460c812b" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="7ef875d9-95fc-478d-b772-971033799107"
    /dev/nvme0n1p2: UUID="234744c8-ed3b-4f2b-a9c1-55dbc71c87ac" TYPE="swap" PARTUUID="132b0b66-7217-480f-a5af-f59713e43306"
    /dev/nvme1n1p4: BLOCK_SIZE="512" UUID="1602C35902C33C8D" TYPE="ntfs" PARTUUID="b91115c2-c01e-458b-a713-2498b4e15eae"
    /dev/nvme1n1p9: UUID="4585ba2e-a70a-4455-ab59-2eff7742d169" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="ae391fcd-8545-49ab-88b5-c720f6f4d5b6"
    /dev/nvme1n1p12: UUID="9f0cf58b-0b2a-49d9-9995-ae7a301317db" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="c60653be-38ed-4af4-885d-bd85e6c90a82"
    /dev/nvme1n1p7: LABEL="DELLSUPPORT" BLOCK_SIZE="512" UUID="104E7D674E7D468E" TYPE="ntfs" PARTUUID="66bd551f-2482-4121-8c66-0830fe0f179a"
    /dev/nvme1n1p10: UUID="1ebbde4d-c3f2-47b2-a4ac-76f85c60ee38" TYPE="swap" PARTUUID="4c575959-bc0c-4219-ab3c-65194dbca3cc"
    /dev/nvme1n1p5: LABEL="WINRETOOLS" BLOCK_SIZE="512" UUID="D8A2972DA2970F5E" TYPE="ntfs" PARTUUID="aa834c21-8c15-44d2-8a8c-5d9dfbbeb508"
    /dev/nvme1n1p3: TYPE="BitLocker" PARTLABEL="Basic data partition" PARTUUID="d0f71a8b-6ef3-4524-82af-e939b8e16d47"
    /dev/nvme1n1p1: UUID="9C57-4B3F" BLOCK_SIZE="512" TYPE="vfat" PARTLABEL="EFI system partition" PARTUUID="53cfb120-4349-487b-bbe0-e38871ac10d0"
    /dev/nvme1n1p8: UUID="6EF8-B15E" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="f28ad5d2-5cbd-4908-9492-5d6e0f56f59b"
    /dev/nvme1n1p11: UUID="0351dc8e-4a77-4f40-bf22-cbf1c0d9c043" BLOCK_SIZE="4096" TYPE="ext4" PARTUUID="7d1d4d89-45c3-42bd-9050-990a71180263"
    /dev/nvme1n1p6: LABEL="Image" BLOCK_SIZE="512" UUID="98FC9779FC97507C" TYPE="ntfs" PARTUUID="f486223d-4e9d-4e52-a313-90aff77b99e8"
    /dev/loop1: TYPE="squashfs"
    /dev/loop2: TYPE="squashfs"
    /dev/loop0: TYPE="squashfs"
    /dev/nvme1n1p2: PARTLABEL="Microsoft reserved partition" PARTUUID="dd2a760e-1b6b-4d8b-a756-bad6f240af7f"
    /dev/loop3: TYPE="squashfs"


.. warning:: 
    Careful not to mix between UUID and partUUID (most of the time you just need the UUID)


Get Mount points
################

Using the **lsblk** give a human readable view of the mount points of the currently running distribution:

Output from Linux Mint:

.. code-block:: text

    NAME         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
    loop0          7:0    0  63,2M  1 loop /snap/core20/1634
    loop1          7:1    0  63,2M  1 loop /snap/core20/1695
    loop2          7:2    0    48M  1 loop /snap/snapd/17336
    loop3          7:3    0  49,6M  1 loop /snap/snapd/17576
    sda            8:0    1     0B  0 disk 
    nvme0n1      259:0    0 931,5G  0 disk 
    ├─nvme0n1p1  259:2    0   512M  0 part /boot/efi
    ├─nvme0n1p2  259:3    0  60,5G  0 part [SWAP]
    ├─nvme0n1p3  259:4    0 810,4G  0 part /home
    └─nvme0n1p4  259:5    0    60G  0 part /
    nvme1n1      259:1    0 953,9G  0 disk 
    ├─nvme1n1p1  259:6    0   100M  0 part 
    ├─nvme1n1p2  259:7    0    16M  0 part 
    ├─nvme1n1p3  259:8    0 543,3G  0 part 
    ├─nvme1n1p4  259:9    0   947M  0 part 
    ├─nvme1n1p5  259:10   0   990M  0 part 
    ├─nvme1n1p6  259:11   0  16,6G  0 part 
    ├─nvme1n1p7  259:12   0   1,3G  0 part 
    ├─nvme1n1p8  259:13   0   488M  0 part 
    ├─nvme1n1p9  259:14   0   977M  0 part 
    ├─nvme1n1p10 259:15   0    61G  0 part 
    ├─nvme1n1p11 259:16   0  65,2G  0 part 
    └─nvme1n1p12 259:17   0   263G  0 part

Output from Ubuntu:

.. code-block:: text

    NAME         MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
    loop0          7:0    0     4K  1 loop /snap/bare/5
    loop1          7:1    0  55,6M  1 loop /snap/core18/2620
    loop2          7:2    0  63,2M  1 loop /snap/core20/1623
    loop3          7:3    0  63,2M  1 loop /snap/core20/1634
    loop4          7:4    0 164,8M  1 loop /snap/gnome-3-28-1804/161
    loop5          7:5    0 346,3M  1 loop /snap/gnome-3-38-2004/119
    loop6          7:6    0  91,7M  1 loop /snap/gtk-common-themes/1535
    loop7          7:7    0  45,9M  1 loop /snap/snap-store/599
    loop8          7:8    0    48M  1 loop /snap/snapd/17336
    loop9          7:9    0   284K  1 loop /snap/snapd-desktop-integration/14
    loop10         7:10   0 169,4M  1 loop /snap/spotify/60
    sda            8:0    1     0B  0 disk 
    nvme0n1      259:0    0 931,5G  0 disk 
    ├─nvme0n1p1  259:1    0   512M  0 part 
    ├─nvme0n1p2  259:2    0  60,5G  0 part 
    ├─nvme0n1p3  259:3    0 810,4G  0 part 
    └─nvme0n1p4  259:4    0    60G  0 part 
    nvme1n1      259:5    0 953,9G  0 disk 
    ├─nvme1n1p1  259:6    0   100M  0 part 
    ├─nvme1n1p2  259:7    0    16M  0 part 
    ├─nvme1n1p3  259:8    0 543,3G  0 part 
    ├─nvme1n1p4  259:9    0   947M  0 part 
    ├─nvme1n1p5  259:10   0   990M  0 part 
    ├─nvme1n1p6  259:11   0  16,6G  0 part 
    ├─nvme1n1p7  259:12   0   1,3G  0 part 
    ├─nvme1n1p8  259:13   0   488M  0 part /boot/efi
    ├─nvme1n1p9  259:14   0   977M  0 part /boot
    ├─nvme1n1p10 259:15   0    61G  0 part [SWAP]
    ├─nvme1n1p11 259:16   0  65,2G  0 part /
    └─nvme1n1p12 259:17   0   263G  0 part /home


Grub: ignoring a partition
##########################

If you want grub to ignore another OS, you can ask grub to ignore a specific partition (the one containing the kernels = root partition `/` ) so that it won't scan for it when grub is being updated.
You need to edit the file `/etc/default/grub` and set the parameter **GRUB_OS_PROBER_SKIP_LIST**.

Ignoring Ubuntu entries in Linux Mint Grub (`/etc/default/grub`):

.. code-block:: text

    # Ignoring Ubuntu root Partition ( `/`)
    GRUB_OS_PROBER_SKIP_LIST="0351dc8e-4a77-4f40-bf22-cbf1c0d9c043@/dev/nvme1n1p11"

Ignoring Linux Mint entries in Ubuntu Grub (`/etc/default/grub`):

.. code-block:: text

    # Ignoring Linux mint root Partition ( `/`)
    GRUB_OS_PROBER_SKIP_LIST="a1fdc046-256f-486b-a26e-0972460c812b@/dev/nvme0n1p4"


Grub: Link to another grub
##########################

Adding the Ubuntu Grub in the Linux Mint grub (to be added in `/etc/grub.d/40_custom`):

.. code-block:: text

    menuentry 'Ubuntu Grub' {
        insmod chain
        search --no-floppy --fs-uuid --set=root 4585ba2e-a70a-4455-ab59-2eff7742d169
        configfile /grub/grub.cfg
    }

Adding the Linux Mint Grub in the Ubuntu grub (to be added in `/etc/grub.d/40_custom`):

.. code-block:: text

    menuentry 'Mint Grub' {
        insmod chain
        search --no-floppy --fs-uuid --set=root a1fdc046-256f-486b-a26e-0972460c812b
        configfile /boot/grub/grub.cfg
    }
.. warning::
    You need to set the UUID of the partition containing the /boot/grub/grub.cfg file of the **OTHER** grub.

    If the other grub has a `/boot` partition then you need to write the UUID of the /boot partition and write the path of the config file **WITHOUT** the `/boot` prefix.

    If the other grub does not have a `/boot` partition then you need to write the UUID of the root partition `/` and write the path of the config file **WITH** the `/boot` prefix.


You can get the partition of the /boot/grub/grub.cfg file using the command:

.. code-block:: bash

    # From Linux Mint
    df -T /boot/grub/grub.cfg
    # Filesystem     Type 1K-blocks     Used Available Use% Mounted on
    # /dev/nvme0n1p4 ext4  61646628 38707212  19775464  67% /

    # From Ubuntu
    df -T /boot/grub/grub.cfg
    # Filesystem     Type 1K-blocks   Used Available Use% Mounted on
    # /dev/nvme1n1p9 ext4    965872 179908    719560  21% /boot


Managing boot entries from Linux
################################

You can manipulate (view/add/remove/change) boot entries with the command line tool: `efibootmgb` (Add `-v` for verbose info)

To view the current state of your boot, just run the command without any argument:

.. code-block:: bash

    efibootmgb

Output:

.. code-block:: text

    BootCurrent: 0000
    Timeout: 0 seconds
    BootOrder: 0000,0002,0004,0003,0001,0005
    Boot0000* Mint
    Boot0001* UEFI PM981a NVMe Samsung 1024GB S4GXNX0NB09373 1
    Boot0002* Ubuntu
    Boot0003* UEFI Samsung SSD 970 EVO Plus 1TB S4EWNZ0R200729F 1
    Boot0004* Windows Boot Manager
    Boot0005* UEFI PM981a NVMe Samsung 1024GB S4GXNX0NB09373 1 2


Example to add Ubuntu and Mint boot entries. You need to identify the EFI partition (/boot/efi) that the OS uses, in my case:

- nvme0n1p1 for Linux Mint
- nvme1n1p8 for Ubuntu

.. code-block:: bash

    sudo efibootmgr -c -d /dev/nvme1n1 -p 8 -L Ubuntu -l "\EFI\ubuntu\shimx64.efi"
    sudo efibootmgr -c -d /dev/nvme0n1 -p 1 -L Mint -l "\EFI\ubuntu\shimx64.efi"

Examle to update the boot order:

.. code-block:: bash

    sudo efibootmgr -o 0000,0002,0004,0003,0001,0005



------------------------------------------------------------

**Sources**:


