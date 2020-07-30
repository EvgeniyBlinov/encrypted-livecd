[https://askubuntu.com/questions/1041916/booting-encrypted-squashfs-from-live-cd](https://askubuntu.com/questions/1041916/booting-encrypted-squashfs-from-live-cd)

Below is a 'one step' bash script that creates an encrypted bootable livecd
from an existing Ubuntu installation. (Tested/Working on Ubuntu 18.10)

Basically, the script copies the existing Ubuntu installation into a set of
working directories at /tmp/livecd and:

    Uses chroot to add casper to the installation
    Modifies casper-helpers to add the encrypted squashfs booting functionality
    Creates the inital unencrypted squashfs housing the entire file system
    Uses a random string input to pre-encrypt a new encrypted squashfs file
    Uses an entered passphrase to then setup the encrypted squashfs file, create an ext4 file system, and then copy over the unencrypted squashfs file into it
    Finally, the entire encrypted bootable ISO (for squashfs files below 4GB) or IMG is created at /tmp/livecd/live-cd.iso or /tmp/livecd/live-cd.img

When the ISO/IMG is booted on the machine or in a VM, the encrypted squashfs is transfered completely into ram,
the user is asked to enter the proper passphrase, and the squashfs is then unencrypted and used to boot the system.

The rsync command line string can be modified to add/remove items that are copied
from the existing Ubuntu installation when the encrypted livecd is being created.

EDIT: The script now handles images above the iso6990 4GB limit.
If the created filesystem.squashfs file is above 4GB then a live-cd.img is created
which is a bootable ext4 image housing the same files as the iso would have had.
In addition, the script asks if you prefer to create and iso
or an img when the filesystem.squahfs file is below the 4GB limit.
