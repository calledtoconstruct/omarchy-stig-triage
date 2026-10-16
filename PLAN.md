# Omarchy STIG triage plan

Working copy of `Omarchy_STIG_Triage_UBTU24_V1R6.xlsx` lives in this folder. The file in `~/Downloads` is untouched. This folder is outside the Omarchy repository. It does not continue the GPOS overlay branches, the regulated-profile branch, or the advisory-sidecar pull requests.

## Premise

Omarchy is not Arch Linux. The project is likely to fork and maintain its own kernel, its own base package repository, and its own user package repository. Rows in this workbook are Ubuntu 24.04 STIG V1R6 rules. The long-term check is against that future Omarchy, not against today's Arch defaults.

What ships today still matters. The current image is built on Arch, uses pacman, and inherits Arch's pam, kernel, and the AUR. A note that says "already true" has to say whether that is true of the current image only, or still true once Omarchy owns the kernel and both repositories.

The SRG-OS ID is the join key to the parent requirement. Ubuntu rule IDs are not Omarchy rule IDs. This work does not mint STIG IDs and does not call the result a STIG.

## What "filled in" means

Yellow columns on the Triage tab:

- **Arch/Omarchy check.** A command or file path aimed at an Omarchy image. Package checks name the Omarchy base or user repository, not "whatever Arch has."
- **Arch/Omarchy fix.** Either "current image already does this, and the fork keeps it by owning that package" or a specific change. A change that would hit every public install is not the fix. The fix is an opt-in preset, a repository policy, or an accepted gap.
- **Status.** `In progress` while the note is only a join. `Done` only when the check and the fix are both written. `Open finding (POA&M)` when the default image does not meet the rule and must not be changed to meet it.
- **Owner / notes.** The parent SRG judgment, and whether owning the kernel or the repositories changes the answer.
- **Proposed bucket.** Leave the heuristic in place until a pass actually rejects it. The note says when the heuristic is not accepted.

`Done` is not a passed SCAP scan.

## Passes

### Phase 1. Join the parent requirements

Sixteen Ubuntu rules sit under twelve SRG-OS IDs that the earlier overlay also covered. Record that join in Owner / notes and set Status to `In progress`. Do not write checks yet. Do not treat the overlay's old "implemented" as a forever claim.

Nine parent findings have no row in this workbook (privileged functions, both nonlocal-maintenance crypto rows, NSA-approved crypto, both at-rest crypto rows, transmission protected by a PDS, the 30-day patch rule, and the GPOS vendor-supported-version rule). They stay outside this spreadsheet.

### Phase 2. The sixteen CAT I rules

Research each one. Rewrite the check for an Omarchy base repository. Separate "package is absent" from "cipher policy." FIPS and classified-crypto rows become open findings. The public image does not grow a FIPS kernel to close them.

### Phase 3. The remaining non-As-is rules

About thirty-six rules after the CAT I set: AIDE, AppArmor, FIPS SSH algorithms, the graphical lock, the DoD banner, and the audit record for `apparmor_parser`. Same status rule as phase 2. Ubuntu's 10-minute lock and the parent 15-minute lock are different numbers. The packaged Omarchy lock is 300 seconds.

### Phase 4. The 149 As-is rows, by check type

One hundred grep a file, thirty-five test a path, fifteen query a package, six read a systemd unit. Write one Omarchy equivalent per type, then sample. Any sample that fails on a default Omarchy install sends that whole type back to individual review. Package-query rows are about the Omarchy repositories after the fork, not about `pacman -Q` on an Arch box.

## Stop lines

Do not change the default Omarchy desktop from this folder. Do not open a pull request on `omarchy` or `omarchy-pkgs` from this research. Do not mark a row Done because the Ubuntu text looks familiar or because Arch happens to match today.

## Phase 1 result

Phase 1 is recorded in the working workbook. Sixteen rows moved to `In progress`. The heuristic bucket was not edited in that pass. Notes reject the As-is guess on the SSH-transport rule, the strong-authenticator rule, and the audit-tool rule. The signed-package rule is aimed at a future Omarchy user repository, not at the AUR as a permanent fact. FIPS rows stay open findings even though an Omarchy-owned kernel is what would make a later preset possible.

## Phase 2 result

All sixteen CAT I rules now have an Omarchy check, a fix, and a status. Nine are `Done`. Seven are `Open finding (POA&M)`. None of the fixes change the public image.

Done, because the default image already meets the rule or the rule does not apply:

- V-270648 rsh-server is not published in the base package list.
- V-279938 the NFS kernel server is absent. `gvfs-nfs` is a file-manager client and stays.
- V-270708 no sshd, so no X11 forwarding. The setup drop-in still needs an explicit `X11Forwarding no` once a preset enables sshd.
- V-270711 Hyprland does not bind Ctrl-Alt-Delete to reboot. The Ubuntu check is a GNOME setting.
- V-270713 the check is an empty shadow password field. Nothing in this repository writes one. A fresh ISO was not scanned.
- V-270717 sshd is off, so unattended SSH login cannot happen. The setup drop-in should also set `PermitEmptyPasswords no` and `PermitUserEnvironment no`.
- V-270736 sssd is not installed. Bucket changed from As-is to Not applicable.
- V-270748 the owner is in `wheel` on purpose. There is no `sudo` group.
- V-278917 only the stable channel is the supported train. Edge and rc fail the rule on a gov/mil image.

Open findings, left open so the public image stays general purpose:

- V-270647 `inetutils` in the base list ships `/usr/bin/telnetd`. The Ubuntu package name `telnetd` is absent, which is why Translate was too weak. Bucket set to Build.
- V-270665 and V-270666 OpenSSH is not in the base list and sshd is not enabled. Ubuntu calls both a finding. Omarchy calls both the default. Buckets set to Policy answer.
- V-270675 Limine has no supervisor password. The grub.cfg check does not apply. Bucket set to Build.
- V-270712 `ctrl-alt-del.target` is an alias of reboot, not masked. Bucket set to Build.
- V-270714 `pam_unix.so nullok` is in `/etc/pam.d/system-auth` and `omarchy-lock-password`, inherited from the current pam stack. The base repository has to ship pam without `nullok`. Bucket set to Build.
- V-270744 no FIPS mode. An Omarchy kernel is the prerequisite for a later preset.
