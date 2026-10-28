### 📂 Main folders in `/` (the root directory)

#### 🔹 `bin`

* Contains **basic programs** (binaries = compiled commands) needed by all users.
* Examples: `ls`, `cp`, `mv`, `cat`.
* Think of it as your **toolbox of everyday commands**.

---

#### 🔹 `boot`

* Contains files needed to **start (boot) the system**, like the Linux kernel.
* Think of it like the **ignition key** of your computer.

---

#### 🔹 `dev`

* Contains **device files** (not real files, but ways to talk to hardware).
* Example: `/dev/sda` = your hard disk, `/dev/tty` = your terminal.
* Think of it like **remote controls for your hardware**.

---

#### 🔹 `etc`

* Contains **configuration files** for the system and applications.
* Example: `/etc/passwd` (user info), `/etc/ssh/sshd_config` (SSH config).
* Think of it as the **settings folder**.

---

#### 🔹 `home`

* Each user gets their own folder inside here.
* Example: `/home/alice`, `/home/bob`.
* It’s like your **Documents** folder in Windows — personal space.

---

#### 🔹 `lib` & `lib64`

* Contains **shared libraries** (like `.dll` files in Windows).
* Programs need these to run.
* Think of them as **ingredients that programs need to cook their recipes**.

---

#### 🔹 `lost+found`

* A special place where **recovered files** are stored after a crash.
* Usually empty.
* Think of it as the **lost & found box** at school.

---

#### 🔹 `media`

* Temporary mount point for external drives (USB, CD/DVD).
* If you plug in a USB stick, it might show up here.
* Think of it as the **USB ports drawer**.

---

#### 🔹 `mnt`

* Another place where filesystems can be temporarily mounted.
* Often used by system admins.
* Think of it as a **spare storage shelf**.

---

#### 🔹 `opt`

* Optional software installed here (not part of the base system).
* Example: some third-party apps may go into `/opt`.
* Think of it as a **miscellaneous apps folder**.

---

#### 🔹 `proc`

* A **virtual folder** that shows information about running processes and the system.
* Example: `/proc/cpuinfo` shows CPU details.
* Think of it as the **system’s self-reporting dashboard**.

---

#### 🔹 `root`

* The **home folder of the root user** (administrator).
* Different from `/` (the root directory).
* Think of it as the **admin’s private room**.

---

#### 🔹 `run`

* Stores **temporary runtime data** (like process IDs, sockets).
* Gets cleared after reboot.
* Think of it as the **scratchpad for running programs**.

---

#### 🔹 `sbin`

* Like `/bin`, but contains **system administration commands**.
* Example: `reboot`, `fdisk`, `ifconfig`.
* Think of it as the **toolbox for mechanics (admins)**.

---

#### 🔹 `srv`

* Contains **data served by the system** (like websites or FTP).
* Rarely used on desktops.
* Think of it as the **storefront** for services.

---

#### 🔹 `sys`

* A **virtual filesystem** showing devices and drivers.
* Works with `/proc`.
* Think of it as the **control panel for hardware**.

---

#### 🔹 `tmp`

* Temporary files go here.
* Cleared on reboot.
* Think of it as the **scratch paper** bin.

---

#### 🔹 `usr`

* Contains **user programs and libraries**.
* Inside, you’ll see `/usr/bin`, `/usr/lib`, etc.
* Think of it as the **app store shelf** — where most software lives.

---

#### 🔹 `var`

* Stores **variable data**: logs, mail, cache, databases.
* Example: `/var/log/syslog` (system logs).
* Think of it as the **diary/journal of the system**.

---

#### 🔹 Special merged folders:

* `bin.usr-is-merged`, `sbin.usr-is-merged`, `lib.usr-is-merged`

  * These are **Ubuntu’s way of merging `/bin` with `/usr/bin`, `/sbin` with `/usr/sbin`, etc.**
  * They’re basically **links** for backward compatibility. You don’t usually need to worry about them.

---

✅ **Summary for beginners**

* **/bin & /sbin** → commands (user vs admin)
* **/etc** → settings
* **/home** → personal files
* **/var** → logs & changing data
* **/tmp** → temporary junk
* **/boot** → startup stuff
* **/dev & /proc & /sys** → system internals
* **/root** → admin’s home
* **/usr** → most installed software

---
