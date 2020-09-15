# Show full TLS certificate details with OpenSSL

* [Example 1](certificate.md#example-1)
* [Example 2](certificate.md#example-2)

## Example 1

```
echo | openssl s_client -showcerts --connect example.com:443 2>/dev/null | openssl x509 -inform pem -noout -text
```

Output:
```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number:
            0f:d0:78:dd:48:f1:a2:bd:4d:0f:2b:a9:6b:60:38:fe
        Signature Algorithm: sha256WithRSAEncryption
        Issuer: C = US, O = DigiCert Inc, CN = DigiCert SHA2 Secure Server CA
        Validity
            Not Before: Nov 28 00:00:00 2018 GMT
            Not After : Dec  2 12:00:00 2020 GMT
        Subject: C = US, ST = California, L = Los Angeles, O = Internet Corporation for Assigned Names and Numbers, OU = Technology, CN = www.example.org
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                RSA Public-Key: (2048 bit)
                Modulus:
                    00:d0:f0:12:74:a0:96:20:72:08:65:19:12:5a:5d:
                    4a:d0:3a:8c:66:8f:a0:29:2b:a7:db:d5:ac:0c:cf:
                    a5:71:92:15:42:15:b0:07:92:76:31:75:d7:27:8e:
                    4d:50:6a:75:d1:7b:53:5e:27:aa:ed:eb:a4:60:3a:
                    f2:8e:45:18:6b:45:33:5c:85:11:aa:20:12:fe:60:
                    ac:9d:4c:45:8f:dd:d3:0e:3e:77:0f:09:c2:85:65:
                    34:c7:22:fb:74:13:b9:42:9f:f7:21:f6:f0:9c:44:
                    74:6d:c9:df:b3:1f:8f:60:b7:71:11:06:90:63:41:
                    9d:8f:34:7b:24:49:46:ac:f2:f0:8d:0b:48:f4:d3:
                    92:1a:f7:a2:45:ee:cc:e5:d7:83:7f:2e:82:bd:71:
                    dd:28:19:58:33:6e:11:a1:3a:a0:6a:72:60:92:01:
                    59:9f:63:17:7a:49:42:7b:9c:3f:db:d3:05:e8:cc:
                    87:7e:f8:aa:fc:9d:d1:05:50:ab:75:b1:1e:ba:20:
                    cb:89:d4:6d:6c:37:82:28:4c:c5:3f:7c:c1:10:f5:
                    a0:a5:66:6b:53:53:c9:db:ed:85:c3:6d:05:f8:64:
                    a7:c9:0e:eb:8f:e1:c4:b1:eb:2d:68:0e:15:3f:e5:
                    e2:dc:fc:21:64:2d:ee:69:2b:04:78:db:77:65:cb:
                    54:f9
                Exponent: 65537 (0x10001)
        X509v3 extensions:
            X509v3 Authority Key Identifier:
                keyid:0F:80:61:1C:82:31:61:D5:2F:28:E7:8D:46:38:B4:2C:E1:C6:D9:E2

            X509v3 Subject Key Identifier:
                66:98:62:02:E0:09:91:A7:D9:E3:36:FB:76:C6:B0:BF:A1:6D:A7:BE
            X509v3 Subject Alternative Name:
                DNS:www.example.org, DNS:example.com, DNS:example.edu, DNS:example.net, DNS:example.org, DNS:www.example.com, DNS:www.example.edu, DNS:www.example.net
            X509v3 Key Usage: critical
                Digital Signature, Key Encipherment
            X509v3 Extended Key Usage:
                TLS Web Server Authentication, TLS Web Client Authentication
            X509v3 CRL Distribution Points:

                Full Name:
                  URI:http://crl3.digicert.com/ssca-sha2-g6.crl

                Full Name:
                  URI:http://crl4.digicert.com/ssca-sha2-g6.crl

            X509v3 Certificate Policies:
                Policy: 2.16.840.1.114412.1.1
                  CPS: https://www.digicert.com/CPS
                Policy: 2.23.140.1.2.2

            Authority Information Access:
                OCSP - URI:http://ocsp.digicert.com
                CA Issuers - URI:http://cacerts.digicert.com/DigiCertSHA2SecureServerCA.crt

            X509v3 Basic Constraints: critical
                CA:FALSE
            CT Precertificate SCTs:
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : A4:B9:09:90:B4:18:58:14:87:BB:13:A2:CC:67:70:0A:
                                3C:35:98:04:F9:1B:DF:B8:E3:77:CD:0E:C8:0D:DC:10
                    Timestamp : Nov 28 21:20:12.614 2018 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:46:02:21:00:84:64:81:B7:21:1D:FA:1A:48:F5:76:
                                AE:4B:E8:46:86:57:27:17:B0:7B:E9:3B:B7:4A:57:42:
                                6C:A2:84:C4:6C:02:21:00:BB:93:B5:FE:30:C4:64:E4:
                                16:4C:7C:6E:58:53:57:EE:EC:7F:AA:45:4F:BF:0E:46:
                                8E:FE:70:FD:FD:8E:42:42
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : 87:75:BF:E7:59:7C:F8:8C:43:99:5F:BD:F3:6E:FF:56:
                                8D:47:56:36:FF:4A:B5:60:C1:B4:EA:FF:5E:A0:83:0F
                    Timestamp : Nov 28 21:20:12.821 2018 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:45:02:20:6F:AA:77:D2:1C:A7:94:C0:63:2D:2E:B3:
                                86:DD:41:8B:40:8A:1A:2F:7F:AE:66:C1:93:5F:73:1F:
                                48:93:50:11:02:21:00:D2:F9:9D:48:86:05:1E:A0:97:
                                44:25:0B:3C:EA:CE:FA:2B:19:7C:81:FF:27:7B:9E:DB:
                                58:B6:DC:E8:F0:4A:4E
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : 6F:53:76:AC:31:F0:31:19:D8:99:00:A4:51:15:FF:77:
                                15:1C:11:D9:02:C1:00:29:06:8D:B2:08:9A:37:D9:13
                    Timestamp : Nov 28 21:20:12.956 2018 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:45:02:21:00:E4:79:FB:43:84:8E:CA:A1:E4:4F:E9:
                                03:B0:7A:BB:92:EE:F3:44:3B:8C:EC:FE:14:0D:7D:9F:
                                B7:63:29:9F:2D:02:20:4D:77:5A:DC:49:01:4A:F4:68:
                                04:85:61:9F:D7:8D:20:0C:31:FA:C1:D3:F4:71:0A:5B:
                                D6:56:CB:3D:2C:72:8C
    Signature Algorithm: sha256WithRSAEncryption
         73:70:85:ef:40:41:a7:6a:43:d5:78:9c:7b:55:48:e6:bc:6b:
         99:86:ba:fb:0d:03:8b:78:fe:11:f0:29:a0:0c:cd:69:14:0b:
         c6:04:78:b2:ce:f0:87:d5:01:9d:c4:59:7a:71:fe:f0:6e:9e:
         c1:a0:b0:91:2d:1f:ea:3d:55:c5:33:05:0c:cd:c1:35:18:b0:
         6a:68:66:4c:bf:56:21:da:5b:d9:48:b9:8c:35:21:91:5d:dc:
         75:d7:7a:46:2c:22:27:a6:6f:d3:3a:17:eb:be:bd:13:c5:12:
         26:73:c0:5d:a3:35:89:6a:fb:27:d4:dd:aa:74:74:2e:37:e5:
         01:3b:a6:d0:30:b0:83:d0:a1:c4:75:21:85:b2:e5:fa:67:00:
         30:a2:bc:53:83:4d:bf:d6:a8:83:bb:bc:d6:ed:1c:b3:1e:f1:
         58:03:82:00:8e:9c:ef:90:f2:1a:5f:a2:a3:06:da:5d:be:9f:
         da:5d:a6:e6:2f:de:58:80:18:d3:f1:62:7b:a6:a3:9f:ae:a8:
         69:72:63:81:65:ae:82:83:a3:b5:97:8a:9b:20:51:ff:1a:3f:
         61:40:1e:48:d0:6b:38:f9:e1:fa:17:d8:77:4a:88:e6:3d:36:
         24:4f:ef:0a:b9:9f:70:f3:83:27:f8:cf:2a:05:75:10:a1:8a:
         0a:80:88:cd
```

## Example 2

```
openssl s_client -showcerts -connect example.com:443 </dev/null
```

Output:
```
CONNECTED(00000005)
depth=2 C = US, O = DigiCert Inc, OU = www.digicert.com, CN = DigiCert Global Root CA
verify return:1
depth=1 C = US, O = DigiCert Inc, CN = DigiCert SHA2 Secure Server CA
verify return:1
depth=0 C = US, ST = California, L = Los Angeles, O = Internet Corporation for Assigned Names and Numbers, OU = Technology, CN = www.example.org
verify return:1
---
Certificate chain
 0 s:C = US, ST = California, L = Los Angeles, O = Internet Corporation for Assigned Names and Numbers, OU = Technology, CN = www.example.org
   i:C = US, O = DigiCert Inc, CN = DigiCert SHA2 Secure Server CA
-----BEGIN CERTIFICATE-----
MIIHQDCCBiigAwIBAgIQD9B43Ujxor1NDyupa2A4/jANBgkqhkiG9w0BAQsFADBN
MQswCQYDVQQGEwJVUzEVMBMGA1UEChMMRGlnaUNlcnQgSW5jMScwJQYDVQQDEx5E
aWdpQ2VydCBTSEEyIFNlY3VyZSBTZXJ2ZXIgQ0EwHhcNMTgxMTI4MDAwMDAwWhcN
MjAxMjAyMTIwMDAwWjCBpTELMAkGA1UEBhMCVVMxEzARBgNVBAgTCkNhbGlmb3Ju
aWExFDASBgNVBAcTC0xvcyBBbmdlbGVzMTwwOgYDVQQKEzNJbnRlcm5ldCBDb3Jw
b3JhdGlvbiBmb3IgQXNzaWduZWQgTmFtZXMgYW5kIE51bWJlcnMxEzARBgNVBAsT
ClRlY2hub2xvZ3kxGDAWBgNVBAMTD3d3dy5leGFtcGxlLm9yZzCCASIwDQYJKoZI
hvcNAQEBBQADggEPADCCAQoCggEBANDwEnSgliByCGUZElpdStA6jGaPoCkrp9vV
rAzPpXGSFUIVsAeSdjF11yeOTVBqddF7U14nqu3rpGA68o5FGGtFM1yFEaogEv5g
rJ1MRY/d0w4+dw8JwoVlNMci+3QTuUKf9yH28JxEdG3J37Mfj2C3cREGkGNBnY80
eyRJRqzy8I0LSPTTkhr3okXuzOXXg38ugr1x3SgZWDNuEaE6oGpyYJIBWZ9jF3pJ
QnucP9vTBejMh374qvyd0QVQq3WxHrogy4nUbWw3gihMxT98wRD1oKVma1NTydvt
hcNtBfhkp8kO64/hxLHrLWgOFT/l4tz8IWQt7mkrBHjbd2XLVPkCAwEAAaOCA8Ew
ggO9MB8GA1UdIwQYMBaAFA+AYRyCMWHVLyjnjUY4tCzhxtniMB0GA1UdDgQWBBRm
mGIC4AmRp9njNvt2xrC/oW2nvjCBgQYDVR0RBHoweIIPd3d3LmV4YW1wbGUub3Jn
ggtleGFtcGxlLmNvbYILZXhhbXBsZS5lZHWCC2V4YW1wbGUubmV0ggtleGFtcGxl
Lm9yZ4IPd3d3LmV4YW1wbGUuY29tgg93d3cuZXhhbXBsZS5lZHWCD3d3dy5leGFt
cGxlLm5ldDAOBgNVHQ8BAf8EBAMCBaAwHQYDVR0lBBYwFAYIKwYBBQUHAwEGCCsG
AQUFBwMCMGsGA1UdHwRkMGIwL6AtoCuGKWh0dHA6Ly9jcmwzLmRpZ2ljZXJ0LmNv
bS9zc2NhLXNoYTItZzYuY3JsMC+gLaArhilodHRwOi8vY3JsNC5kaWdpY2VydC5j
b20vc3NjYS1zaGEyLWc2LmNybDBMBgNVHSAERTBDMDcGCWCGSAGG/WwBATAqMCgG
CCsGAQUFBwIBFhxodHRwczovL3d3dy5kaWdpY2VydC5jb20vQ1BTMAgGBmeBDAEC
AjB8BggrBgEFBQcBAQRwMG4wJAYIKwYBBQUHMAGGGGh0dHA6Ly9vY3NwLmRpZ2lj
ZXJ0LmNvbTBGBggrBgEFBQcwAoY6aHR0cDovL2NhY2VydHMuZGlnaWNlcnQuY29t
L0RpZ2lDZXJ0U0hBMlNlY3VyZVNlcnZlckNBLmNydDAMBgNVHRMBAf8EAjAAMIIB
fwYKKwYBBAHWeQIEAgSCAW8EggFrAWkAdwCkuQmQtBhYFIe7E6LMZ3AKPDWYBPkb
37jjd80OyA3cEAAAAWdcMZVGAAAEAwBIMEYCIQCEZIG3IR36Gkj1dq5L6EaGVycX
sHvpO7dKV0JsooTEbAIhALuTtf4wxGTkFkx8blhTV+7sf6pFT78ORo7+cP39jkJC
AHYAh3W/51l8+IxDmV+9827/Vo1HVjb/SrVgwbTq/16ggw8AAAFnXDGWFQAABAMA
RzBFAiBvqnfSHKeUwGMtLrOG3UGLQIoaL3+uZsGTX3MfSJNQEQIhANL5nUiGBR6g
l0QlCzzqzvorGXyB/yd7nttYttzo8EpOAHYAb1N2rDHwMRnYmQCkURX/dxUcEdkC
wQApBo2yCJo32RMAAAFnXDGWnAAABAMARzBFAiEA5Hn7Q4SOyqHkT+kDsHq7ku7z
RDuM7P4UDX2ft2Mpny0CIE13WtxJAUr0aASFYZ/XjSAMMfrB0/RxClvWVss9LHKM
MA0GCSqGSIb3DQEBCwUAA4IBAQBzcIXvQEGnakPVeJx7VUjmvGuZhrr7DQOLeP4R
8CmgDM1pFAvGBHiyzvCH1QGdxFl6cf7wbp7BoLCRLR/qPVXFMwUMzcE1GLBqaGZM
v1Yh2lvZSLmMNSGRXdx113pGLCInpm/TOhfrvr0TxRImc8BdozWJavsn1N2qdHQu
N+UBO6bQMLCD0KHEdSGFsuX6ZwAworxTg02/1qiDu7zW7RyzHvFYA4IAjpzvkPIa
X6KjBtpdvp/aXabmL95YgBjT8WJ7pqOfrqhpcmOBZa6Cg6O1l4qbIFH/Gj9hQB5I
0Gs4+eH6F9h3SojmPTYkT+8KuZ9w84Mn+M8qBXUQoYoKgIjN
-----END CERTIFICATE-----
 1 s:C = US, O = DigiCert Inc, CN = DigiCert SHA2 Secure Server CA
   i:C = US, O = DigiCert Inc, OU = www.digicert.com, CN = DigiCert Global Root CA
-----BEGIN CERTIFICATE-----
MIIElDCCA3ygAwIBAgIQAf2j627KdciIQ4tyS8+8kTANBgkqhkiG9w0BAQsFADBh
MQswCQYDVQQGEwJVUzEVMBMGA1UEChMMRGlnaUNlcnQgSW5jMRkwFwYDVQQLExB3
d3cuZGlnaWNlcnQuY29tMSAwHgYDVQQDExdEaWdpQ2VydCBHbG9iYWwgUm9vdCBD
QTAeFw0xMzAzMDgxMjAwMDBaFw0yMzAzMDgxMjAwMDBaME0xCzAJBgNVBAYTAlVT
MRUwEwYDVQQKEwxEaWdpQ2VydCBJbmMxJzAlBgNVBAMTHkRpZ2lDZXJ0IFNIQTIg
U2VjdXJlIFNlcnZlciBDQTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEB
ANyuWJBNwcQwFZA1W248ghX1LFy949v/cUP6ZCWA1O4Yok3wZtAKc24RmDYXZK83
nf36QYSvx6+M/hpzTc8zl5CilodTgyu5pnVILR1WN3vaMTIa16yrBvSqXUu3R0bd
KpPDkC55gIDvEwRqFDu1m5K+wgdlTvza/P96rtxcflUxDOg5B6TXvi/TC2rSsd9f
/ld0Uzs1gN2ujkSYs58O09rg1/RrKatEp0tYhG2SS4HD2nOLEpdIkARFdRrdNzGX
kujNVA075ME/OV4uuPNcfhCOhkEAjUVmR7ChZc6gqikJTvOX6+guqw9ypzAO+sf0
/RR3w6RbKFfCs/mC/bdFWJsCAwEAAaOCAVowggFWMBIGA1UdEwEB/wQIMAYBAf8C
AQAwDgYDVR0PAQH/BAQDAgGGMDQGCCsGAQUFBwEBBCgwJjAkBggrBgEFBQcwAYYY
aHR0cDovL29jc3AuZGlnaWNlcnQuY29tMHsGA1UdHwR0MHIwN6A1oDOGMWh0dHA6
Ly9jcmwzLmRpZ2ljZXJ0LmNvbS9EaWdpQ2VydEdsb2JhbFJvb3RDQS5jcmwwN6A1
oDOGMWh0dHA6Ly9jcmw0LmRpZ2ljZXJ0LmNvbS9EaWdpQ2VydEdsb2JhbFJvb3RD
QS5jcmwwPQYDVR0gBDYwNDAyBgRVHSAAMCowKAYIKwYBBQUHAgEWHGh0dHBzOi8v
d3d3LmRpZ2ljZXJ0LmNvbS9DUFMwHQYDVR0OBBYEFA+AYRyCMWHVLyjnjUY4tCzh
xtniMB8GA1UdIwQYMBaAFAPeUDVW0Uy7ZvCj4hsbw5eyPdFVMA0GCSqGSIb3DQEB
CwUAA4IBAQAjPt9L0jFCpbZ+QlwaRMxp0Wi0XUvgBCFsS+JtzLHgl4+mUwnNqipl
5TlPHoOlblyYoiQm5vuh7ZPHLgLGTUq/sELfeNqzqPlt/yGFUzZgTHbO7Djc1lGA
8MXW5dRNJ2Srm8c+cftIl7gzbckTB+6WohsYFfZcTEDts8Ls/3HB40f/1LkAtDdC
2iDJ6m6K7hQGrn2iWZiIqBtvLfTyyRRfJs8sjX7tN8Cp1Tm5gr8ZDOo0rwAhaPit
c+LJMto4JQtV05od8GiG7S5BNO98pVAdvzr508EIDObtHopYJeS4d60tbvVS3bR0
j6tJLp07kzQoH3jOlOrHvdPJbRzeXDLz
-----END CERTIFICATE-----
 2 s:C = US, O = DigiCert Inc, OU = www.digicert.com, CN = DigiCert Global Root CA
   i:C = US, O = DigiCert Inc, OU = www.digicert.com, CN = DigiCert Global Root CA
-----BEGIN CERTIFICATE-----
MIIDrzCCApegAwIBAgIQCDvgVpBCRrGhdWrJWZHHSjANBgkqhkiG9w0BAQUFADBh
MQswCQYDVQQGEwJVUzEVMBMGA1UEChMMRGlnaUNlcnQgSW5jMRkwFwYDVQQLExB3
d3cuZGlnaWNlcnQuY29tMSAwHgYDVQQDExdEaWdpQ2VydCBHbG9iYWwgUm9vdCBD
QTAeFw0wNjExMTAwMDAwMDBaFw0zMTExMTAwMDAwMDBaMGExCzAJBgNVBAYTAlVT
MRUwEwYDVQQKEwxEaWdpQ2VydCBJbmMxGTAXBgNVBAsTEHd3dy5kaWdpY2VydC5j
b20xIDAeBgNVBAMTF0RpZ2lDZXJ0IEdsb2JhbCBSb290IENBMIIBIjANBgkqhkiG
9w0BAQEFAAOCAQ8AMIIBCgKCAQEA4jvhEXLeqKTTo1eqUKKPC3eQyaKl7hLOllsB
CSDMAZOnTjC3U/dDxGkAV53ijSLdhwZAAIEJzs4bg7/fzTtxRuLWZscFs3YnFo97
nh6Vfe63SKMI2tavegw5BmV/Sl0fvBf4q77uKNd0f3p4mVmFaG5cIzJLv07A6Fpt
43C/dxC//AH2hdmoRBBYMql1GNXRor5H4idq9Joz+EkIYIvUX7Q6hL+hqkpMfT7P
T19sdl6gSzeRntwi5m3OFBqOasv+zbMUZBfHWymeMr/y7vrTC0LUq7dBMtoM1O/4
gdW7jVg/tRvoSSiicNoxBN33shbyTApOB6jtSj1etX+jkMOvJwIDAQABo2MwYTAO
BgNVHQ8BAf8EBAMCAYYwDwYDVR0TAQH/BAUwAwEB/zAdBgNVHQ4EFgQUA95QNVbR
TLtm8KPiGxvDl7I90VUwHwYDVR0jBBgwFoAUA95QNVbRTLtm8KPiGxvDl7I90VUw
DQYJKoZIhvcNAQEFBQADggEBAMucN6pIExIK+t1EnE9SsPTfrgT1eXkIoyQY/Esr
hMAtudXH/vTBH1jLuG2cenTnmCmrEbXjcKChzUyImZOMkXDiqw8cvpOp/2PV5Adg
06O/nVsJ8dWO41P0jmP6P6fbtGbfYmbW0W5BjfIttep3Sp+dWOIrWcBAI+0tKIJF
PnlUkiaY4IBIqDfv8NZ5YBberOgOzW6sRBc4L0na4UU+Krk2U886UAb3LujEV0ls
YSEY1QSteDwsOoBrp+uvFRTp2InBuThs4pFsiv9kuXclVzDAGySj4dzp30d8tbQk
CAUw7C29C79Fv1C5qfPrmAESrciIxpg0X40KPMbp1ZWVbd4=
-----END CERTIFICATE-----
---
Server certificate
subject=C = US, ST = California, L = Los Angeles, O = Internet Corporation for Assigned Names and Numbers, OU = Technology, CN = www.example.org

issuer=C = US, O = DigiCert Inc, CN = DigiCert SHA2 Secure Server CA

---
No client certificate CA names sent
Peer signing digest: SHA256
Peer signature type: RSA-PSS
Server Temp Key: ECDH, P-256, 256 bits
---
SSL handshake has read 4675 bytes and written 739 bytes
Verification: OK
---
New, TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
Server public key is 2048 bit
Secure Renegotiation IS NOT supported
Compression: NONE
Expansion: NONE
No ALPN negotiated
Early data was not sent
Verify return code: 0 (ok)
---
DONE
```
