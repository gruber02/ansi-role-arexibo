# ansi-role-arexibo

This Ansible role deploys Arexibo on a machine that is supposed to become a "Xibo display" (therefore, there must be a Xibo server available in the network). It is supposed to make rebooting unattended, i.e., it will auto-unlock the LUKS encryption, auto-login into the OS, and auto-start Arexibo with proper connection to the Xibo server.
It will also lock down the GRUB bootloader (if a password is specified in a variables file like `inventory/host_vars/[target-machine-name]`), but allow unattended normal boot (only ask for password if somebody tries to edit the boot parameters).

# how to use this role

Prerequisites for "Xibo display" target machine:
* TPM 2.0 must be enabled in the BIOS/UEFI settings
* Ubuntu Server LTS with LUKS encryption must be installed

Edit `defaults/main.yml` to your liking (at minimum, set the Xibo CMS IP address and the Xibo CMS key for proper Display linking).
Copy the arexibo binary into the `files/` folder of this ansible role.
Set a GRUB password in an ansible-vault encrypted file.

Run the ansible playbook that calls this role, then reboot after successful completion of the playbook to check if it does properly boot up without any manual intervention.

# limitations

* arexibo binary must be copied by hand (we don't have something like an artifactory or other registry)
* the `/etc/crypttab` parser simply always selects the first matching encrypted device for TPM2 enrollment, so this ansible role might show unexpected behavior on systems that have more than one LUKS volume
* apt cache must be updated by hand (simply run `sudo apt update` on the target machine before you run the role)