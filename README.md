[![](https://img.shields.io/nuget/v/soenneker.cloudflare.origincerts.thumbprints.svg?style=for-the-badge)](https://www.nuget.org/packages/soenneker.cloudflare.origincerts.thumbprints/)
[![](https://img.shields.io/github/actions/workflow/status/soenneker/soenneker.cloudflare.origincerts.thumbprints/build-and-test.yml?style=for-the-badge)](https://github.com/soenneker/soenneker.cloudflare.origincerts.thumbprints/actions/workflows/build-and-test.yml)
[![](https://img.shields.io/nuget/dt/soenneker.cloudflare.origincerts.thumbprints.svg?style=for-the-badge)](https://www.nuget.org/packages/soenneker.cloudflare.origincerts.thumbprints/)
[![](https://img.shields.io/github/actions/workflow/status/soenneker/soenneker.cloudflare.origincerts.thumbprints/codeql.yml?label=CodeQL&style=for-the-badge)](https://github.com/soenneker/soenneker.cloudflare.origincerts.thumbprints/actions/workflows/codeql.yml)

# Soenneker.Cloudflare.OriginCerts.Thumbprints

A packaged snapshot of Cloudflare's shared Authenticated Origin Pull CA certificate and its SHA-256 fingerprint.

## Installation

```bash
dotnet add package Soenneker.Cloudflare.OriginCerts.Thumbprints
```

## Packaged resources

The package copies these files to the consuming application's output:

```text
Resources/cloudflareorigincerts.pem
Resources/cloudflareorigincerts.txt
```

- `cloudflareorigincerts.pem` contains the CA certificate in PEM form.
- `cloudflareorigincerts.txt` contains the uppercase SHA-256 fingerprint of the certificate's DER bytes, one fingerprint per line.

## Reading the resources

```csharp
string resources = Path.Combine(AppContext.BaseDirectory, "Resources");
string pem = await File.ReadAllTextAsync(
    Path.Combine(resources, "cloudflareorigincerts.pem"),
    cancellationToken);

string[] sha256Fingerprints = await File.ReadAllLinesAsync(
    Path.Combine(resources, "cloudflareorigincerts.txt"),
    cancellationToken);
```

The value in the text file is deliberately SHA-256. Do not compare it with `X509Certificate2.Thumbprint` unless the algorithm used by that property matches your validation scheme; hash `certificate.RawData` with SHA-256 when comparing to this file.

This package does not download updates at runtime and does not configure Authenticated Origin Pulls. Package upgrades can change the embedded trust material, so deploy and validate them with the same care as other certificate-store changes.
