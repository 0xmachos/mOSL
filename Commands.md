# Commands 

Easy to read, non one-liner, version of all the `audit` and `fix` commands used by mOSL.   


## Enable Automatic System Updates

### Audit
```
if ! defaults read "/Library/Preferences/com.apple.SoftwareUpdate.plist" "AutomaticallyInstallMacOSUpdates" >/dev/null 2>&1; then
	exit 1
	# This key isnt present if the user has not manually interacted with System Preferences > Software Update before
fi
defaults read "/Library/Preferences/com.apple.SoftwareUpdate.plist" "AutomaticallyInstallMacOSUpdates" | grep -q "1"

```

### Fix 
```
declare -a keys

keys=(AutomaticCheckEnabled AutomaticDownload AutomaticallyInstallMacOSUpdates ConfigDataInstall CriticalUpdateInstall)

for key in "${keys[@]}"; do
	defaults write "/Library/Preferences/com.apple.SoftwareUpdate.plist" "${key}" -bool true
done
```

## Enable Automatic App Store Updates

### Audit
```
if ! defaults read "/Library/Preferences/com.apple.commerce.plist" "AutoUpdate" >/dev/null 2>&1; then
	exit 1
fi
defaults read "/Library/Preferences/com.apple.commerce.plist" "AutoUpdate" >/dev/null 2>&1 | grep -q '0';
```

### Fix
```
defaults write "/Library/Preferences/com.apple.commerce.plist" "AutoUpdate" -bool true
```

## Enable Gatekeeper

### Audit
```
spctl --status | grep -q "assessments enabled"
```

### Fix 
```
sudo spctl --master-enable
```

## Enable Firewall

### Audit
```
sudo /usr/libexec/ApplicationFirewall/socketfilterfw  --getglobalstate | grep -q 'enabled'
```

### Fix 
```
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on
```

## Require an administrator password to access system-wide preferences

### Audit
```
security -q authorizationdb read system.preferences | grep -A1 'shared' | grep -q 'false'
```

### Fix 
```
security -q authorizationdb read system.preferences > /tmp/system.preferences.plist
/usr/libexec/PlistBuddy -c 'Set :shared false' /tmp/system.preferences.plist
sudo security -q authorizationdb write system.preferences < /tmp/system.preferences.plist
rm '/tmp/system.preferences.plist'
```

## Enable `Terminal.app` secure keyboard entry

### Audit
```
defaults read com.apple.Terminal SecureKeyboardEntry | grep -q '1'
```

### Fix 
```
defaults write com.apple.Terminal SecureKeyboardEntry -bool true
```

## Enable System Integrity Protection (SIP)

### Audit
```
csrutil status | grep -q 'enabled'
```

### Fix 
```
sudo csrutil clear
```

## Enable FileVault

### Audit
```
fdesetup status | grep -q 'On'
```

### Fix 
```
sudo fdesetup enable -user $USER > $HOME/FileVault_recovery_key.txt
```

## Disable built-in software from being auto-permitted to listen through firewall

### Audit
```
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getallowsigned | grep 'built-in' | grep -q 'DISABLED'
```

### Fix 
```
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setallowsigned off
```

## Disable downloaded signed software from being auto-permitted to listen through firewall

### Audit
```
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getallowsigned | grep 'downloaded' | grep -q 'DISABLED'
```

### Fix 
```
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setallowsignedapp off
```

## Disable IPv6

### Audit
```
while IFS= read -r i; do
  if ! networksetup -getinfo "${i}" | grep "IPv6: Off"; then
    exit 1
  fi
done <<< $(networksetup -listallnetworkservices | tail -n $(( $(networksetup -listallnetworkservices | wc -l) - 1 )))
```

### Fix 
```
while read -r i; do 
  networksetup -setv6off "${i}"
done <<< "$(networksetup -listallnetworkservices | tail -n $(( $(networksetup -listallnetworkservices | wc -l) - 1 )))"
```

## Disable automatic loading of remote content by `Mail.app`

### Audit
```
defaults read com.apple.mail-shared DisableURLLoading | grep -q '1'
```

### Fix 
```
defaults write com.apple.mail-shared DisableURLLoading -bool true
```

## Disable Remote Apple Events

### Audit
```
sudo systemsetup -getremoteappleevents | grep -q 'Remote Apple Events: Off'
```

### Fix 
```
sudo systemsetup -setremoteappleevents off
```

## Disable Remote Login

### Audit
``` 
sudo systemsetup -getremotelogin | grep -q 'Remote Login: Off'
```

### Fix 
```
sudo systemsetup -f -setremotelogin off
```

## Disable Safari Auto Open 'safe' Files

### Audit
``` 
defaults read com.apple.Safari AutoOpenSafeDownloads | grep -q '0'
```

### Fix 
```
defaults write com.apple.Safari AutoOpenSafeDownloads -bool false
```

## Set AirDrop Discoverability to 'Contacts Only'

### Audit
```
if defaults read com.apple.sharingd DiscoverableMode | grep -q 'Contacts Only'; then 
  exit 0
elif defaults read com.apple.sharingd DiscoverableMode | grep -q 'Off'; then 
  exit 0
else 
  exit 1
fi
```

### Fix 
```
defaults write com.apple.sharingd DiscoverableMode -string 'Contacts Only'
sudo killall -HUP sharingd
```

## Set AppStore update check to every one (`1`) day

### Audit
```
defaults read com.apple.SoftwareUpdate ScheduleFrequency | grep -q '1'
```

### Fix 
```
defaults write com.apple.SoftwareUpdate ScheduleFrequency -int 1
```

## Set a firmware password

### Audit
```
sudo firmwarepasswd -check | grep -q 'Yes'
```

### Fix 
```
sudo firmwarepasswd -setpasswd
```

## Check Kernel Extension User Consent required

### Audit
```
spctl kext-consent status | grep -q 'ENABLED'
```

### Fix 
```
n/a
```

## Check EFI Firmware Integrity

### Audit
```
/usr/libexec/firmwarecheckers/eficheck/eficheck --integrity-check
```

### Fix 
```
n/a
```

## `${USER}` is not an administrator

### Audit
```
groups | grep -qv 'admin'
```

### Fix 
```
n/a
```
