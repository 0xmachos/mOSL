# macOS Lockdown (mOSL)
[![Build Status](https://travis-ci.org/0xmachos/mOSL.svg?branch=master)](https://travis-ci.org/0xmachos/mOSL) [![GitHub Release](https://github-basic-badges.herokuapp.com/release/0xmachos/mOSL.svg)](https://github.com/0xmachos/mOSL/releases/latest)

Bash script to audit and fix macOS High Sierra (`10.13.x`) security settings

## Warnings
- This script will **only ever** support the _latest_ macOS release  
- This script requires your **password** to invoke some commands with `sudo`  

## Verification

The executable `Lockdown` file can be verified with [Minisign](https://jedisct1.github.io/minisign/):
```
minisign -Vm Lockdown -P RWTiYbJbLl7q6uQ70l1XCvGExizUgEBNDPH0m/1yMimcsfgh542+RDPU
```
Install via `brew`: `brew install minisign`

## Verification

The executable `Lockdown` file can be verified with [Minisign](https://jedisct1.github.io/minisign/):
```
minisign -Vm Lockdown -P RWTiYbJbLl7q6uQ70l1XCvGExizUgEBNDPH0m/1yMimcsfgh542+RDPU
```
Install via `brew`: `brew install minisign`

## Usage

```
$ ./Lockdown

Audit or Fix macOS security settings🔒🍎

Usage:
  list         - List all settings that can be audited/ fixed
  audit        - Audit the status of all settings (Does NOT change settings)
  fix  {force} - Attempt to fix all settings (Does change settings)
```

## Settings

Settings that can be audited/ fixed:
```
enable automatic updates
enable gatekeeper
enable firewall
enable admin password preferences
enable terminal secure entry
disable firewall builin software
disable firewall downloaded signed
disable ipv6
disable mail remote content
disable remote apple events
disable remote login
set airdrop contacts only
set appstore update check daily
check SIP
check kext loading consent
check EFI integrity
check filevault
check firmware password set
```
