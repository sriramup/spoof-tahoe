# spoof-tahoe

A lightweight MAC address randomizer for **macOS Tahoe**.

`spoof-tahoe` generates a random, locally administered MAC address and applies it to your Wi-Fi interface with a single command.

```bash
spoof-tahoe
```

## Why?

Older MAC spoofing tools for macOS relied on Apple's private `airport` utility:

```text
/System/Library/PrivateFrameworks/Apple80211.framework/Resources/airport
```

Newer versions of macOS no longer include this utility, causing tools that depend on it to fail with errors such as:

```text
airport: No such file or directory
Error: Unable to disassociate from wifi networks
```

`spoof-tahoe` avoids the deprecated `airport` utility entirely and uses tools still available in macOS to change the interface's MAC address.

## Requirements

* macOS Tahoe
* Administrator access
* Wi-Fi interface (defaults to `en0`)

> **Important:** Wi-Fi must be **on but disconnected from the current network** before running `spoof-tahoe`. Turning Wi-Fi completely off may prevent macOS from accepting the new MAC address.

## Installation

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/spoof-tahoe.git
cd spoof-tahoe
```

Make the script executable:

```bash
chmod +x spoof-tahoe
```

Move it into `/usr/local/bin`:

```bash
sudo mv spoof-tahoe /usr/local/bin/spoof-tahoe
```

Verify the installation:

```bash
which spoof-tahoe
```

You should see:

```text
/usr/local/bin/spoof-tahoe
```

## Usage

### 1. Disconnect from Wi-Fi

Disconnect from your current Wi-Fi network using the macOS Wi-Fi menu.

**Do not turn Wi-Fi off.**

### 2. Run spoof-tahoe

```bash
spoof-tahoe
```

You'll be prompted for your administrator password because changing the MAC address requires elevated privileges.

On success:

```text
Success: MAC changed to 02:xx:xx:xx:xx:xx
```

### 3. Reconnect to Wi-Fi

Reconnect to your Wi-Fi network normally.

You can verify the current MAC address with:

```bash
ifconfig en0 | grep ether
```

## Using a Different Interface

`spoof-tahoe` defaults to:

```text
en0
```

You can specify another interface:

```bash
spoof-tahoe en1
```

## How It Works

`spoof-tahoe` generates an address in the following format:

```text
02:xx:xx:xx:xx:xx
```

The first byte is intentionally set to `02`, making the generated address a **locally administered unicast MAC address**.

The remaining five bytes are randomized each time the command runs.

The generated address is then applied using:

```bash
sudo ifconfig en0 ether <MAC>
```

The script reads the interface afterward to verify that macOS actually applied the requested address.

## Uninstall

Remove the executable:

```bash
sudo rm /usr/local/bin/spoof-tahoe
```

Verify that it is gone:

```bash
which spoof-tahoe
```

## Notes

* The spoofed MAC may not persist after restarting your Mac.
* Run `spoof-tahoe` again whenever you want a new random address.
* macOS's **Private Wi-Fi Address** feature is separate from this utility and may affect which MAC address a particular network sees.
* Future macOS updates may change the behavior of `ifconfig` or Wi-Fi interfaces.

## Disclaimer

This project is intended for privacy, testing, development, and educational purposes. Use it only on devices and networks you are authorized to use.
