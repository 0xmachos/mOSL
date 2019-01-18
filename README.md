# macOS Lockdown (mOSL)
[![Build Status](https://travis-ci.org/0xmachos/mOSL.svg?branch=master)](https://travis-ci.org/0xmachos/mOSL) [![GitHub Release](https://github-basic-badges.herokuapp.com/release/0xmachos/mOSL.svg)](https://github.com/0xmachos/mOSL/releases/latest)

Bash script to audit and fix macOS Mojave (`10.14.x`) security settings

## Warnings
- **Always** run the [latest release](https://github.com/0xmachos/mOSL/releases/latest) **not** the code in `master`!
- This script will **only ever** support the _latest_ macOS release  
- This script requires your **password** to invoke some commands with `sudo`  

## Threat Model(ish) 

The main goal is to enforce already secure defaults and apply more strict non-default options. 

It aims to reduce attack surface but it is pragmatic in this pursuit. The author utilises Bluetooth for services such as Handoff so it is left enabled.

There is **no specific focus** on enhancing privacy. 

Finally, mOSL will not protect you from the [FSB](https://en.wikipedia.org/wiki/Federal_Security_Service), [MSS](https://en.wikipedia.org/wiki/Ministry_of_State_Security_(China)), [DGSE](https://en.wikipedia.org/wiki/Directorate-General_for_External_Security), or [FSM](https://en.wikipedia.org/wiki/Flying_Spaghetti_Monster).

## Mojave Permissions

In order to audit/ fix `disable mail remote content` `Terminal.app` needs to read from and write to `com.apple.mail-shared.plist`. Mojave requires that `Terminal.app` be given the `Full Disk Access` permission to access this file as files related to `Mail.app` are now a protected. 

It is not possible to programatically get or prompt for this permission, it must be manually given by the user.

To give `Terminal.app` `Full Disk Access`:

`System Preferences` > `Security & Privacy` > `Privacy` > `Full Disk Access` > Add `Terminal.app`

This is *currently* the **only** setting which requires the `Full Disk Access` permission.   

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

  Usage: ./Lockdown [list | audit {setting_index} | fix {setting_index} | debug]

    list         - List settings that can be audited/ fixed
    audit        - Audit the status of all or chosen setting(s) (Does NOT change settings)
    fix          - Attempt to fix all or chosen setting(s) (Does change settings)

    fix-force    - Same as 'fix' however bypasses user confirmation prompt
                   (Can be used to invoke Lockdown from other scripts)

    debug        - Print debug info for troubleshooting

```

## Settings

See [`Commands.md`](https://github.com/0xmachos/mOSL/blob/master/Commands.md) for a easy to read list of commands used to `audit`/ `fix` the below settings.

Settings that can be audited/ fixed:
```
  [0] enable automatic updates
  [1] enable gatekeeper
  [2] enable firewall
  [3] enable admin password preferences
  [4] enable terminal secure entry
  [5] enable sip
  [6] enable filevault
  [7] disable firewall builin software
  [8] disable firewall downloaded signed
  [9] disable ipv6
  [10] disable mail remote content
  [11] disable remote apple events
  [12] disable remote login
  [13] set airdrop contacts only
  [14] set appstore update check daily
  [15] check kext loading consent
  [16] check efi integrity
  [17] check firmware password set
  [18] check if standard user
```
