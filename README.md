# LINUX-PRIVILEGE-ESCALATION-PrivEsc-LOG
> Focus: Escalating from Low-Privileged User to Root
> Platform: TryHackMe / Local Lab
> Date: March 1, 2026

---

## [ 01 ] INITIAL ENUMERATION
Sebelum melakukan eksploitasi, langkah pertama adalah mengumpulkan informasi sistem (SITREP):
* **OS Kernel Version**: `uname -a` (Mencari celah kernel lama seperti Dirty Cow atau Dirty Pipe).
* **Current User Context**: `whoami` & `id` (Melihat grup user, apakah masuk ke grup `sudo`, `docker`, atau `lxd`).
* **Sudo Rights**: `sudo -l` (Mengecek perintah apa yang bisa dijalankan sebagai root tanpa password).

---

## [ 02 ] EXPLOITATION VECTORS
Teknik PrivEsc yang dipelajari dan dipraktikkan hari ini:

### A. SUID / SGID Binaries
Mencari file yang memiliki bit SUID set, yang memungkinkan file dijalankan dengan hak akses pemiliknya (biasanya root).
* **Discovery**: `find / -perm -4000 -type f 2>/dev/null`
* **GTFOBins**: Menggunakan [GTFOBins](https://gtfobins.github.io/) untuk mengeksploitasi binary seperti `find`, `vim`, atau `bash` yang memiliki SUID.

### B. Writable /etc/passwd
Mengecek apakah file sensitif seperti `/etc/passwd` bisa ditulis oleh user biasa.
* **Exploit**: Menambahkan user root baru dengan password yang sudah di-hash secara manual.

### C. Capabilities
Mengecek "Capabilities" pada binary yang memberikan izin spesifik tanpa perlu SUID penuh.
* **Command**: `getcap -r / 2>/dev/null`



---

## [ 03 ] AUTOMATION TOOLS
Mempelajari cara menggunakan script otomatis untuk mempercepat proses audit:
1. **LinPEAS**: Tool paling powerfull untuk mencari hampir semua celah PrivEsc secara otomatis.
2. **LinEnum**: Script bash ringan untuk check-list konfigurasi sistem.
3. **Linux Exploit Suggester**: Menganalisis versi kernel dan menyarankan exploit yang cocok.

---

## [ 04 ] DEFENSIVE INSIGHT (HARDENING)
* **Principle of Least Privilege**: Jangan pernah memberikan akses `sudo` jika tidak benar-benar diperlukan.
* **Restrict SUID**: Hapus bit SUID pada binary yang tidak membutuhkan izin root.
* **Kernel Patching**: Selalu update kernel Linux ke versi terbaru untuk menutup celah *Zero-Day*.

---
*Status: Root Access Obtained*
*Researcher: sepkascurty-cpu*
