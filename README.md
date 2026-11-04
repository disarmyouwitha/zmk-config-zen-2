# Corne-ish Zen V2 Custom Configuration
![Corne-ish Zen Logo](img/Zen_R3_sticker.png)

## Upstream migration and Plover steno

This configuration uses upstream ZMK main commit
[`4dd95d7840eb3d5a3f59bb29fbf1ab054715ef85`](https://github.com/zmkfirmware/zmk/commit/4dd95d7840eb3d5a3f59bb29fbf1ab054715ef85),
the latest upstream commit fetched during this migration. Both the west manifest
and reusable build workflow are pinned to it so the builds are reproducible.
The matrix explicitly selects **V2 / revision 2.0.0**, matching the old targets.

The keymap now follows the supplied Voyager layout: **MAIN (0)**,
**SYM+NUM (1)**, and **STENO (2)**. The unused Voyager number row is omitted
because the Zen has three main rows. Detailed diagrams and tap/hold actions
are in [KEYMAP.md](KEYMAP.md).

Hold-tap keeps the 200 ms tapping term, 125 ms quick-tap and prior-idle settings.
Sleep remains 15 minutes. The display uses upstream Zen defaults.

### Layer controls and Mac translation

- Hold the **middle left thumb key** for SYM+NUM; release it to return.
- Tap the **bottom-right main key** to enter STENO.
- Tap the **bottom-left key in STENO** to return to MAIN.
- A/S/Z/X/C/V/O hold actions send **Command+A/S/Z/X/C/V/F**.
- Hold **P** for **Command+Shift+4** (screenshot selection).
- Hold **semicolon** for ordinary **Enter**, as requested.
- Hold B for grave/backtick; the top-right main key is forward Delete.
- Physical Control and Shift, punctuation holds, arrows, and Shift+Left stay as shown.
- The old separate SYMBOL and GAMING layers are removed.
- Zen has one extra thumb key per half: left outer stays Command; right outer
  stays sticky Shift. The two Voyager right thumb positions both send Space.
- Voyager LED controls are omitted. On SYM+NUM, Q selects Bluetooth profile 1,
  W selects profile 2, and R clears the selected profile. Other LED positions
  are disabled. The top-left SYM+NUM key opens the Mac Force Quit dialog
  (Command+Option+Escape), replacing the Windows Control+Alt+Delete action.

### Plover HID

Use **Plover → Configure → Machine → Plover HID**, then connect to the Zen.
Leave Plover output enabled: MAIN and SYM+NUM send ordinary keyboard reports
and bypass Plover's steno input. Connect the left half by USB for initial testing;
the right half continues to communicate with it wirelessly.

The pinned module is
[`petercpark/zmk-hid-io-plover-hid@d895457`](https://github.com/petercpark/zmk-hid-io-plover-hid/commit/d895457418590dddf628c5ae907b39deec7095ff).
It sends an eight-byte state bitmap with usage page `0xFF50`, usage `0x4C56`,
and report ID `0x50`. NKRO is not enabled. Each physical S, star and number-bar
key uses its own report index. Translation and dictionaries live in Plover.

The previous build has been physically tested over USB by the user; macOS also
enumerated its Plover HID interface over Bluetooth. The Voyager keymap revision
still needs its own physical key-by-key and chord tests after flashing.

### Build and flash

Open the **Build Zen firmware** Actions run for the update branch and download
its `firmware` artifact after both halves pass. Flash
`corneish_zen_v2_left.uf2` to the left half and
`corneish_zen_v2_right.uf2` to the right half: connect that half by USB,
double-tap its reset button to mount the bootloader drive, then copy its matching
UF2 file onto the drive. Repeat for the other half. Keep the previous working
firmware as a rollback copy.

Both halves must be updated together because this crosses several ZMK versions.
Adding the steno interface alters the HID descriptors: forget the old keyboard pairing on
the host, clear that host profile on the Zen using SYM+NUM + R
(BT_CLR), and pair again. Try USB first to verify ordinary typing and Plover
chords. Flashing and physical/Bluetooth tests must be done on the actual keyboard.

References: [Plover HID protocol](https://github.com/dnaq/plover-machine-hid),
[Plover setup](https://plover.wiki/index.php/Beginner%27s_Guide), and
[Zen support and bootloader instructions](https://lowprokb.ca/pages/corne-ish-zen-support).


This repo is the official configuration of the Corne-ish Zen V2 low profile wireless mechanical keyboard. Use it to develop your own keymap and easily build your own ZMK firmware to run on your Corne-ish Zen. These steps will get you using your keymap on your keyboard in the fastest time possible. It uses the GitHub Actions feature to build your firmware online, rather than setting up a complex tool chain on your local computer.

If you are looking to dig deeper into ZMK and develop new functionality, it is recommended to follow the steps of installing ZMK as found on the official ZMK documentation site (linked below).

V2 PCBs were only used in the 3rd GB round (R3). To confirm which version you need, remove the bottom from the keyboard and look beside the Corne-ish Zen logo for a version number. Group Buy rounds 1 and 2 have V1 PCBs and R3 has V2 PCBs. (Also V2 PCBs have white power switches... V1 PCBs have black ones.)

## Resources

- The [official ZMK Firmware GitHub](https://github.com/zmkfirmware/zmk) repository. View the keymaps for other boards and shields as a starting point for your keymap.
- The [official ZMK Documentation](https://zmk.dev/docs) web site. Find the answers to many of your questions about ZMK Firmware.
- The [official ZMK Discord Server](https://discord.gg/8cfMkQksSB). Instant conversations with other ZMK developers and users. Great technical resource!

## Instructions

1. Log into, or sign up for, your personal GitHub account.
2. Fork this repository to your local computer, and then push it to your GitHub personal account. ([instructions](https://docs.github.com/en/get-started/quickstart/fork-a-repo))
3. Edit the keymap file(s) to suit your needs
4. Commit and push your changes to your personal repo. Upon pushing it, GitHub Actions will start building a new version of your firmware with the updated keymap.

## Firmware Files

To locate your firmware files...

1. log into GitHub and navigate to your personal config repository you just uploaded your keymap changes to.
2. Click "Actions" in the main navigation, and in the left navigation click the "Build" link.
3. Select the desired workflow run in the centre area of the page (based on date and time of the build you wish to use). You can also start a new build from this page by clicking the "Run workflow" button.
4. After clicking the desired workflow run, you should be presented with a section at the bottom of the page called "Artifacts". This section contains the results of your build, in a file called "firmware.zip"
5. Download the firmware zip archive and extract the two .uf2 files. They are named according to which side they need to be flashed to.
6. Flash the firmware to your keyboard by double-clicking the reset button to put the it in bootloader mode. A window should pop up showing the contents of the storage on the keyboard. Drag and drop the correct .uf2 file into the window. When the upload is complete the window will close and the keyboard will exit bootloader mode.

Your keyboard is now ready to use.
