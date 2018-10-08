# Commands 

Easy to read, non one-liner version of all the `audit` and `fix` commands used by mOSL.   


## Enable Automatic Updates

### Audit
```
sudo softwareupdate --schedule | grep -q 'Automatic check is on'
```

### Fix 
```
sudo softwareupdate --schedule on
```

## Enable Gatekeeper

### Audit
```
spctl --status | grep -q "assessments enabled"
```

### Fix 
```
sudo spctl --master-enable'
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

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```

## 

### Audit
```

```

### Fix 
```

```
