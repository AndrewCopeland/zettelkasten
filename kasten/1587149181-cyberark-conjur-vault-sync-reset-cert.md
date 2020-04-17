# 1587149181 cyberark-conjur-vault-sync-reset-cert
#cyber #conjur #syncronizer #certificate

To reset the conjur certificate on the Vault Conjur Syncronizer service open a powershell console with Administrator privileges. Then copy and paste this function into the powershell console:

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

And then execute this function by running:
```powershell
Save-Certificate -serverUri "https://<conjur master hostname>/info" -certPath "C:\Program Files\Cyberark\Syncronizer\conjur.pem"
```

The new certificate will be retrieved from the conjur master and will be loaded in the `LocalMachine\Root` certificate store.
Now restart the Vault Conjur Syncronizer service.
## Links
