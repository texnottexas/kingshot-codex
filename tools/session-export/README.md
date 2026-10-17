# Download the Kingshot session exporter

**[Download the ZIP](https://github.com/texnottexas/kingshot-codex/raw/refs/heads/main/tools/session-export/kingshot-session-export.zip)**

Works with BlueStacks Air on Mac or BlueStacks 5 on Windows. Extract the ZIP
before running it. The same full instructions below are included inside the ZIP.

This exports login details for an account you own or are authorized to manage.
It does not enable auto-hunt by itself; the receiving operator still needs to
verify the account and configure its own marches and settings.

Never upload the resulting `kingshot-session.json` to this repository or its issues.

---

# Kingshot login export

Use this with a dummy account you own or have permission to manage.

## Before you start

- Mac: BlueStacks Air on an Apple Silicon Mac.
- Windows: BlueStacks 5 on Windows 10 or 11.
- Install Python 3.11 or newer from https://www.python.org/downloads/ if it is not installed.
  On Windows, include the Python launcher during setup.
- Keep all the files from this download together. Extract the ZIP first.

BlueStacks Air is the Mac version; friends using Windows can use BlueStacks 5.
They do not need to log in on both platforms.

## Export the login

1. Open the intended dummy account in Kingshot. Note its name and in-game Player ID.
2. Quit BlueStacks and stop its instances in Multi-instance Manager.
3. Double-click **Export-Mac.command** on Mac or **Export-Windows.cmd** on Windows.
   On the first run, the launcher downloads three pinned disk-reading libraries
   and their dependencies into a local Python environment. It does not install them globally.
4. Select the instance by its displayed name. The helper saves
   **kingshot-session.json** in a new **kingshot-login-...** folder inside Downloads.

The file contains only `fpid` and `session_key`, ready for a GitHub Actions secret.
**Keep the file private: it grants account access.** Share it only through your
agreed private credential channel, not an alliance chat or a public issue.
Send the in-game Player ID separately so the receiving reader can verify the account.
Delete the local export after it has been stored where intended.

The helper does not print the two values, upload them, log into the game, enable
root, alter BlueStacks settings, or write to the virtual disk. The extracted
FPID is a login identifier, not the in-game Player ID. The session key is the
reusable login value; it is not the changing request number in a packet capture.
Select the intended character last if multiple characters share one game login.
Extraction alone does not verify which character the server will select or
whether that session is still valid. A bounded authenticated read verifies that
before enabling an alliance reader.

## If it does not start

On Mac, open Terminal in the extracted folder and run:

```sh
bash Export-Mac.command
```

On Windows, run Export-Windows.cmd from the extracted folder. Its PowerShell
execution-policy setting applies only to that launched process; it does not
change your machine's saved policy. If organizational policy blocks it, use your
normal approved process instead of changing that policy.

## Custom installations

Once the launcher has installed dependencies, you can pass an instance name,
a data folder, or the exact disk path. Arguments also work with the launchers.

Mac:

```sh
bash Export-Mac.command --instance "Alliance Dummy"
bash Export-Mac.command --list
```

Windows:

```bat
Export-Windows.cmd --root "D:\BlueStacksData"
Export-Windows.cmd --disk "D:\BlueStacksData\Engine\Pie64\Data.vhdx"
```

`--root` points to the folder containing `bluestacks.conf` and `Engine`.
`--output-dir` chooses an existing parent folder instead of Downloads. A new,
private subfolder is always created; existing export files are never overwritten.

The helper detects QCOW2, VHDX and VHD by their contents, then looks for the exact
Kingshot settings file inside an ext4 volume. It supports raw ext4, primary MBR
partitions and GPT with 512/4096-byte sectors. Encrypted disks, unfamiliar layouts,
other games and older BlueStacks versions are not supported. It does not scan
for arbitrary tokens or export other login providers' settings.

Default locations:

- Mac: `/Users/Shared/Library/Application Support/BlueStacks/Engine/<instance>/data.qcow2`
- Windows: `%ProgramData%\BlueStacks_nxt\Engine\<instance>\Data.vhdx`

An instance that is still running must be stopped normally. The helper does not
kill it. On Mac it checks whether the disk is open and whether it changed during
the read. Windows holds an exclusive, read-only disk handle. Output is restricted
to the current OS user (Mac folder 700/file 600; Windows folder ACL before writing).

## Validation and alliance setup

The Mac path has been tested on a real BlueStacks Air instance. QCOW2, VHDX and
VHD decoding have also been tested against synthetic disks with fake credentials.
The Windows PowerShell launcher, private output creation, exclusive disk locking
and all three disk formats passed on a Windows GitHub runner with fake credentials.
An actual BlueStacks 5 installation is still a pilot check.

This export helper can be used for other alliances. The current game readers
are still deliberately restricted to Butler / WPK / Kingdom 1904. A new alliance
needs its own secret, verified account/alliance identity, observed trap object
IDs and separate stored results. Do not substitute another alliance's credentials
into WPK's configured reader and remove its checks.

Official BlueStacks references:

- Air for Mac: https://support.bluestacks.com/hc/en-us/articles/32272892398093-How-to-install-and-play-games-with-BlueStacks-Air-on-Mac
- Windows disk location and stopping instances: https://support.bluestacks.com/hc/en-us/articles/360059480692-How-to-backup-and-restore-data-on-BlueStacks-5
- Windows requirements: https://support.bluestacks.com/hc/en-us/articles/360056129211-System-requirements-for-BlueStacks-5
