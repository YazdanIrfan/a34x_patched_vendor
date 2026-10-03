Samsung Galaxy A34 5G — Patched Vendor

A patched Vendor image for the Samsung Galaxy A34 5G ("a34x") designed to support multiple A34 variants using the bootloader versions listed in ""supported_bootloaders"" (./supported_bootloaders).

The goal of this project is to provide a unified patched vendor that can be booted across supported Galaxy A34 5G variants without requiring a separate vendor for every individual variant.

«⚠️ Important: Compatibility is determined by the bootloader version, not simply by the fact that the device is a Galaxy A34 5G.
Always check the ""supported_bootloaders"" (./supported_bootloaders) file before flashing.»

---

📱 Supported Device

Device| Codename| SoC
Samsung Galaxy A34 5G| "a34x"| MediaTek Dimensity 1080 / MT6877

Variant Support

This patched vendor is intended to work across supported Galaxy A34 5G variants running a bootloader listed in:

supported_bootloaders

If your device's bootloader is not listed there, do not assume compatibility.

---

🔐 Check Your Bootloader

Before using the patched vendor, check your current bootloader version.

Using ADB

adb shell getprop ro.boot.bootloader

Using Termux

getprop ro.boot.bootloader

Compare the returned value with the versions listed in:

supported_bootloaders

Only continue if your bootloader is supported.

---

🛠️ What Is a Patched Vendor?

Samsung's vendor partition contains hardware-specific components required for Android to communicate with the device's hardware.

On the A34 5G, some components—particularly TEE (Trusted Execution Environment) components—are tied to specific bootloader/firmware versions.

This project patches the vendor so that a single vendor package can support multiple supported bootloader versions.

The approach is inspired by the A34x Multi-TEE work from UN1CA, which allows different TEE components to be selected dynamically depending on the device's bootloader version.

Conceptually:

                 Patched Vendor
                       │
                       ▼
              Detect Bootloader
                       │
          ┌────────────┴────────────┐
          │                         │
          ▼                         ▼
   Supported BL A             Supported BL B
          │                         │
          ▼                         ▼
      TEE Blob A                 TEE Blob B
          │                         │
          └────────────┬────────────┘
                       ▼
                  Android Boot

This makes it possible to maintain a common vendor while supporting multiple compatible bootloader revisions.

---

📦 Repository Structure

The repository contains the files required to build and/or distribute the patched vendor.

.
├── supported_bootloaders
├── vendor/
├── customize.sh
├── file_context-vendor
├── fs_config-vendor
├── module.prop
└── README.md

"supported_bootloaders"

Contains the bootloader versions currently supported by the patched vendor.

Always check this file before flashing.

"vendor/"

Contains the vendor-side files required by the patch.

"customize.sh"

Installation/customization logic used when packaging the patched vendor.

"file_context-vendor"

SELinux file-context definitions required by the vendor.

"fs_config-vendor"

Filesystem ownership and permission configuration.

"module.prop"

Module metadata when the vendor is distributed through a root/module-based package.

---

🔧 Building the Patched Vendor

The patched vendor is based on the concept and patching methodology used by the A34x UN1CA Multi-TEE implementation.

The original UN1CA A34x patches are available here:

"UN1CA — target_a34x_patches_tee" (https://reference-url-citation.invalid/2)

The UN1CA project documents that its A34x patches are intended to support multiple bootloader versions by providing the appropriate TEE components and selecting them at boot.

General Build Flow

Samsung A34 Firmware
        │
        ▼
Extract Vendor
        │
        ▼
Prepare Vendor Files
        │
        ▼
Apply A34x Patches
        │
        ├── TEE modifications
        ├── Bootloader compatibility
        ├── SELinux contexts
        ├── Filesystem configuration
        └── Required vendor modifications
        │
        ▼
Add Supported TEE Components
        │
        ▼
Build Patched Vendor
        │
        ▼
Test on Supported A34 Variant

The exact build process depends on the vendor source/firmware being used and the version of the patches included in this repository.

---

⚠️ Compatibility

Supported

A device is considered supported when:

- It is a Samsung Galaxy A34 5G ("a34x").
- Its bootloader version is listed in ""supported_bootloaders"" (./supported_bootloaders).
- The required firmware/vendor generation matches the vendor used to create the package.
- The device has an unlocked bootloader and supports the required flashing method.

Not Guaranteed

This project does not guarantee compatibility with:

- Unsupported bootloader versions.
- Future bootloaders that are not listed in "supported_bootloaders".
- Unrelated Samsung devices.
- Vendors from incompatible Android/One UI generations.
- Modified or corrupted vendor images.

---

📋 Before Flashing

Always make a complete backup before modifying the vendor partition.

At minimum, keep backups of:

boot
init_boot
vendor_boot
dtbo
vbmeta
vendor
super

If possible, keep a complete stock firmware package for your exact device variant.

Also verify:

adb shell getprop ro.boot.bootloader

Then compare the result with:

supported_bootloaders

---

🚨 Important Warning

Flashing an incompatible vendor can result in:

- Bootloops
- Loss of hardware functionality
- Camera problems
- Audio problems
- Connectivity issues
- SELinux failures
- TEE-related errors
- Encryption/keymaster problems
- A device that requires restoring the stock vendor/firmware

You are responsible for what you flash to your device.

Do not flash this vendor simply because you own an A34 5G.

Check the bootloader first.

---

🧪 Testing

Testing should be performed on each supported bootloader/variant combination whenever possible.

Recommended checks after boot:

✓ Android boots normally
✓ Touch/display
✓ Wi-Fi
✓ Bluetooth
✓ Mobile network
✓ Calls
✓ SMS
✓ Camera
✓ Audio
✓ Microphone
✓ GPS
✓ Fingerprint
✓ NFC
✓ USB
✓ Charging
✓ Sensors
✓ DRM
✓ Keymaster / security services
✓ SELinux status

For debugging:

adb shell getprop ro.boot.bootloader
adb shell getprop ro.boot.hardware
adb shell getprop ro.board.platform
adb shell getenforce
adb shell dmesg

Additional Android logs can be collected with:

adb logcat -b all

---

🤝 Credits

This project would not exist without the work of the developers and projects that made A34x custom firmware development possible.

UN1CA

Special thanks to UN1CA for the original A34x Multi-TEE patching work and the foundation that inspired this project.

"UN1CA — target_a34x_patches_tee" (https://reference-url-citation.invalid/4)

The original project specifically documents its purpose as making the same package boot across different supported bootloaders.

Fede2782

Huge thanks to Fede2782 for his extensive work on the Galaxy A34 5G, including his A34x kernel and UN1CA development work, which provided significant inspiration for this project.

"Fede2782 on GitHub" (https://reference-url-citation.invalid/7)

Salvo Giangreco

Special thanks to Salvo Giangreco ("salvogiangri") for his work on UN1CA and the broader Samsung custom firmware ecosystem.

"Salvo Giangreco on GitHub" (https://reference-url-citation.invalid/9)

---

📜 License & Attribution

This project incorporates ideas and/or work derived from the A34x Multi-TEE implementation developed by UN1CA.

Please respect the original project's licensing and attribution requirements when redistributing modified or derived work.

The original UN1CA A34x patch repository states that its project files are GPLv3-licensed, while certain prebuilt files are excluded from that license.

If you redistribute this project or a derivative, keep the appropriate credits and licensing information intact.

---

⭐ Disclaimer

This is an independent community project for the Samsung Galaxy A34 5G.

Samsung has no affiliation with this project.

The developers and contributors are not responsible for:

- Bricked devices
- Data loss
- Warranty issues
- Security problems
- Hardware damage
- Failed firmware installations

Flash at your own risk.

---

❤️ A34x Community

If this project helped you with A34x development, consider giving the repository a ⭐ and contributing compatibility reports, fixes, and improvements.

Galaxy A34 5G development is still alive.
