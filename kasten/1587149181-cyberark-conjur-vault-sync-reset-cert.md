# 1587149181 cyberark-conjur-vault-sync-reset-cert
#cyber #conjur #syncronizer #certificate

To reset the Conjur certificate on the CyberArk Vault-Conjur Synchronizer service, open a PowerShell console with Administrator privileges. Then copy and paste this function into the PowerShell console:

```powershell
function Save-Certificate($serverUri, $certPath)
{
    $builder = New-Object System.Text.StringBuilder
    $request = [System.Net.WebRequest]::CreateHttp($serverUri)
    $request.ServerCertificateValidationCallback = 
    {
        param(
        [System.Object]$sender,
        [System.Security.Cryptography.X509Certificates.X509Certificate]$certificate,
        [System.Security.Cryptography.X509Certificates.X509Chain]$chain,
        [System.Net.Security.SslPolicyErrors]$sslPolicyErrors)

        $cert2 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList $certificate

        if ($chain.Build($cert2))
        {
            return $true;
        }

        $issuer = $chain.ChainElements[$chain.ChainElements.Count - 1].Certificate

        $builder.AppendLine("-----BEGIN CERTIFICATE-----")
        $builder.AppendLine([System.Convert]::ToBase64String($issuer.Export([System.Security.Cryptography.X509Certificates.X509ContentType]::Cert), [System.Base64FormattingOptions]::InsertLineBreaks))
        $builder.AppendLine("-----END CERTIFICATE-----")
    
        return $true
    }

    $response = $request.GetResponse()

    if ($builder.Length -gt 0)
    {
        [System.IO.File]::WriteAllText($certPath, $builder.ToString())
        $responseCertLoad = Import-Certificate -CertStoreLocation "Cert:\LocalMachine\Root" -FilePath $certPath
        Write-Host "Server certificate was loaded into LocalMachie\Root store, $responseCertLoad"
        [System.IO.File]::Delete($certPath)
    }

}
```

Then execute this function by running:
```powershell
Save-Certificate -serverUri "https://<conjur master hostname>/info" -certPath "C:\Program Files\Cyberark\Syncronizer\conjur.pem"
```

The new certificate is retrieved from the Master and is loaded in the `LocalMachine\Root` certificate store.

Restart the CyberArk Vault-Conjur Synchronizer service.

## Links
- [1588949676-cyberark-conjur-sync-unable-to-detect-conjur-server-version.md](1588949676-cyberark-conjur-sync-unable-to-detect-conjur-server-version.md)
