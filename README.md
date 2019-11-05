# macOS Lockdown (mOSL)
[![Build Status](https://travis-ci.org/0xmachos/mOSL.svg?branch=master)](https://travis-ci.org/0xmachos/mOSL) [![GitHub Release](https://github-basic-badges.herokuapp.com/release/0xmachos/mOSL.svg)](https://github.com/0xmachos/mOSL/releases/latest)

Bash script to audit and fix macOS Catalina (`10.15.x`) security settings

Inspired by and based on [Lockdown](https://objective-see.com/products/lockdown.html) by [Patrick Wardle](https://twitter.com/patrickwardle) and [osxlockdown](https://github.com/SummitRoute/osxlockdown) by [Scott Piper](https://twitter.com/0xdabbad00https://twitter.com/0xdabbad00). 

## Warnings

**mOSL is being rewritten in Swift and the Bash version will be deprecated.**. See: "[The Future of mOSL](https://0xmachos.github.io/2019-09-21-The-Future-of-mOSL/)".

- **Always** run the [latest release](https://github.com/0xmachos/mOSL/releases/latest) **not** the code in `master`!
- This script will **only ever** support the _latest_ macOS release  
- This script requires your **password** to invoke some commands with `sudo`  


## `brew`

tap: [`0xmachos/homebrew-mosl`](https://github.com/0xmachos/homebrew-mosl)

To install mOSL via `brew` execute:

```
brew tap 0xmachos/homebrew-mosl
brew install mosl
```

mOSL will then be available as: 

```
Lockdown
```

## Threat Model(ish) 

The main goal is to enforce already secure defaults and apply more strict non-default options. 

It aims to reduce attack surface but it is pragmatic in this pursuit. The author utilises Bluetooth for services such as Handoff so it is left enabled.

There is **no specific focus** on enhancing privacy. 

Finally, mOSL will not protect you from the [FSB](https://en.wikipedia.org/wiki/Federal_Security_Service), [MSS](https://en.wikipedia.org/wiki/Ministry_of_State_Security_(China)), [DGSE](https://en.wikipedia.org/wiki/Directorate-General_for_External_Security), or [FSM](https://en.wikipedia.org/wiki/Flying_Spaghetti_Monster).

## `Full Disk Access` Permission

In macOS Mojave and later certain application data is protected by the OS. For example, if `Example.app` wishes to access `Contacts.app` data `Example.app` must be given explicit permission via `System Preferences > Security & Privacy > Privacy`. However some application data cannot be accessed via a specific permission. Access to this data requires the `Full Disk Access` permission. 

mOSL requires that `Terminal.app` be given the `Full Disk Access` permission. It needs this permission to audit/fix the following settings: 

- `disable mail remote content`
- `disable_auto_open_safe_downloads`

These are *currently* the **only** settings which require `Full Disk Access`.

It is not possible to programatically get or prompt for this permission, it must be manually given by the user.

To give `Terminal.app` `Full Disk Access`:

```
System Preferences > Security & Privacy > Privacy > Full Disk Access > Add Terminal.app
```

Once you are done with mOSL you can revoke `Full Disk Access` for `Terminal.app`. There's a small checkbox next to `Terminal` which you can uncheck to revoke the premssion without entirely removing `Terminal.app` from the list.  

More info on macOS's new permission model:

- [`Working with Mojave’s Privacy Protection`](https://eclecticlight.co/2018/09/06/working-with-mojaves-privacy-protection/) by [Howard Oakley](https://twitter.com/howardnoakley)
- [`TCC Round Up`](https://carlashley.com/2018/09/28/tcc-round-up/) by [Carl Ashley](https://twitter.com/carlashleyphoto)
- WWDC 2018 Session 702 [`Your Apps and the Future of macOS Security`](https://developer.apple.com/videos/play/wwdc2018/702/)

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
  [0] enable automatic system updates
  [1] enable automatic app store updates
  [2] enable gatekeeper
  [3] enable firewall
  [4] enable admin password preferences
  [5] enable terminal secure entry
  [6] enable sip
  [7] enable filevault
  [8] disable firewall builin software
  [9] disable firewall downloaded signed
  [10] disable ipv6
  [11] disable mail remote content
  [12] disable remote apple events
  [13] disable remote login
  [14] disable auto open safe downloads
  [15] set airdrop contacts only
  [16] set appstore update check daily
  [17] set firmware password
  [18] check kext loading consent
  [19] check efi integrity
  [20] check if standard user
```
