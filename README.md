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
Install via [brew](https://brew.sh/): `brew install minisign`

## Usage

```
$ ./Lockdown 

  Audit or Fix macOS security settings🔒🍎

  Usage: ./Lockdown [list | audit {setting_index} | fix {setting_index}]

    list         - List settings that can be audited/ fixed
    audit        - Audit the status of all or chosen setting(s) (Does NOT change settings)
    fix          - Attempt to fix all or chosen setting(s) (Does change settings)

    fix-force    - Same as 'fix' however bypasses user confirmation prompt
                   (Can be used to invoke Lockdown from other scripts)
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
