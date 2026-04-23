# ForgeBox 101

ForgeBox is an open hardware platform for developers, researchers, and educators who want to run their own code on a secure device.

The point of ForgeBox is control: you can experiment with custom firmware, build your own signing flows, prototype security features, and test cryptographic ideas on real hardware.

Built on the Keystone 3 hardware wallet architecture, ForgeBox is designed to give you a secure but flexible base for:

- cryptography and blockchain research
- custom signing and key-management tools
- hardware-backed agent or application experiments
- teaching secure systems with programmable hardware

## Order ForgeBox

You can order ForgeBox here: [Order ForgeBox](https://keyst.one/shop/products/keystone-3-forge-box?variant=KV032-FB-SD).

## What Makes ForgeBox Different

- **Open source**: the hardware platform is meant to be customized, not treated as a sealed black box.
- **Secure by design**: ForgeBox is based on hardened wallet architecture and uses secure hardware components.
- **Built for iteration**: you can inspect the device, generate keys, register your own public key, build firmware, and sign OTA packages with the CLI.

## Who This Page Is For

This guide is for people who ordered ForgeBox and want a practical starting point. It will help you set up ForgeBox and get your first Hello World firmware running.


## What You Need

Before you begin, make sure you have:

- your ForgeBox device
- a USB-C cable that supports data transfer
- a computer with Node.js and npm installed
- `python3` installed if you plan to build firmware
- an ARM embedded toolchain such as `arm-none-eabi-gcc` if you plan to compile firmware from source

## Setup In 10 Minutes

### 1. Install the CLI

ForgeBox is managed through the ForgeBox CLI.

```bash
npm install -g forgebox-cli
forgebox --version
forgebox --help
```

### 2. Connect the Device

Plug ForgeBox into your computer over USB-C.

Then list connected devices:

```bash
forgebox list-devices
```

If the device is detected, you should see ForgeBox in the device list.

### 3. Check Device Status

Confirm the device is reachable and inspect its basic information:

```bash
forgebox status
```

This should show information such as:

- model
- firmware version
- hardware version
- serial number

If this step works, your local setup is ready for key registration and firmware workflows.

### 4. Generate a Key Pair

ForgeBox uses your key material for signing and registration workflows.

Create a fresh key pair:

```bash
forgebox keygen --out ./my-keys
```

This writes:

- `./my-keys/private.pem`
- `./my-keys/pubkey.pem`

Treat `private.pem` as sensitive material. Anyone with that file can sign firmware as you.

### 5. Register Your Public Key On Device

Register the generated key pair with ForgeBox:

```bash
forgebox register ./my-keys
```

During registration:

1. The CLI checks that the public and private key match.
2. The device shows a fingerprint.
3. You compare the fingerprint shown in the terminal with the one shown on the device.
4. You confirm on the device.

Once registered, ForgeBox can verify firmware packages signed with the matching private key.

## Your First Real Workflow

After setup, most users do one of these three things first.

### Option A: Explore The Device

Use this if you want to confirm the hardware path before touching firmware.

```bash
forgebox list-devices
forgebox status
```

This is the safest first step and the right place to start if you are evaluating hardware quality, connectivity, or tooling.

### Option B: Build Custom Firmware

Clone the firmware repository:

```bash
git clone https://github.com/keystonehq/forgebox-firmware.git
```

Then build it:

```bash
forgebox build:firmware ./forgebox-firmware -o ./my-firmware
```

The CLI runs the firmware build process and writes the resulting binary to your output directory.

Use this path if you are:

- testing protocol ideas
- modifying cryptographic logic
- prototyping a custom hardware-backed application

### Option C: Sign An OTA Package

Once you have a firmware binary, turn it into a signed OTA package:

```bash
forgebox sign --s ./my-firmware/mh1903_full.bin --d ./my-firmware/forgebox.bin --key ./my-keys/private.pem
```

This produces an OTA image that can be verified by the device using your registered key.

## Suggested Learning Path

If this is your first day with ForgeBox, use this order:

1. Install the CLI.
2. Connect the device and run `forgebox status`.
3. Generate a new key pair.
4. Register the public key on the device.
5. Build stock firmware from source.
6. Sign the resulting firmware.
7. Move on to your own modifications.

That sequence keeps hardware verification, key setup, and firmware customization clearly separated.

## Safety Notes

- Do not reuse experimental signing keys for anything production-critical.
- Do not load firmware you do not understand onto a device holding real secrets.
- Back up your key material before making signing part of your workflow.
- Keep your private key outside version control.

## What You Can Build With ForgeBox

ForgeBox is intended to support open-ended development. A few obvious directions are:

- your own hardware-backed signer or HSM-like workflow
- blockchain protocol prototypes and custom key-management systems
- agent systems that need isolated signing or secret storage
- classroom or workshop demos for applied cryptography

## If You Get Stuck

Start with the smallest working loop:

1. Check that the device is visible with `forgebox list-devices`.
2. Check that `forgebox status` returns device information.
3. Regenerate a clean key pair and retry registration.

If those steps work, the hardware connection and CLI are in good shape, and the next problem is usually in your firmware or signing flow.

## First Draft Notes

This is a first-pass onboarding page. The next version can add:

- screenshots of the device registration flow
- a dedicated firmware flashing section
- a QR-based workflow section
- links to firmware examples and starter projects


