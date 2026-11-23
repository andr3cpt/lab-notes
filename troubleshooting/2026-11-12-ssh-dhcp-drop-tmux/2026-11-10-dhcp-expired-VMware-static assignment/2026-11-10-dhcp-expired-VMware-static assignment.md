### Details

| Field | Detail |
| :--- | :--- |
| Date | 10 nov 2026 |
| Environment | Ubuntu Server VM on VMware |
| Host OS | macOS |
| Components / Services | `vmnet8`, `openssh-server` |

---

### Breakdown

| Section | Description |
| :--- | :--- |
| **Issues** | • Couldn't connect to the Ubuntu VM via host alias `ssh ubuntu`; connection timed-out |
| **Attempts** | • Checked network interface: `ip a`; showed a different IPv4  address than the one saved in the SSH config file<br>• Tried to restart service: `sudo systemctl restart systemd-networkd`<br>• Tried to renew DHCP lease: `sudo networkctl renew ens33`<br>• Tried to reboot both VMware and Ubuntu VM |
| **Fix** | • Manually edited the `vmnet8` config file (VMware's own DHCP server) to set a static IP address<br>• Updated the `.ssh/config` file with the new IP address bound to the `ubuntu` alias |
| **Causes** | VMware's built-in DHCP server (`vmnet8`) automatically assigns a new lease to any VM every few days, hence simply typing `ssh <alias>` won't work because the new IP is no longer bound to that alias |
| **Takeaways** | Learned about DHCP leases, dynamic vs static IP assignment, NAT vs Bridged connecion |
| **Related Repo** | https://github.com/andr3cpt/vmware-static-ip |

