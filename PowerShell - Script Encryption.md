

## Encrypt [[PowerShell]] Scripts

Function to obfuscate code containing sensitive data:

```powershell
function Encrypt-Script($path, $destination) {
    $script = Get-Content $path | Out-String
    $secure = ConvertTo-SecureString $script -asPlainText -force
    $export = $secure | ConvertFrom-SecureString
    Set-Content $destination $export
    "Script '$path' has been encrypted as '$destination'"
}


function Execute-EncryptedScript($path) {
    trap { "Decryption failed"; break }
    $raw = Get-Content $path
    $secure = ConvertTo-SecureString $raw
    $helper = New-Object system.Management.Automation.PSCredential("test", $secure)
    $plain = $helper.GetNetworkCredential().Password
    Invoke-Expression $plain
}
```

From: [idera](https://community.idera.com/database-tools/powershell/powertips/b/tips/posts/encrypting-powershell-scripts#)