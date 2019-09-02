# Terraform provider for Microsft Windows DNS

This enables Terraform to control Microsoft DNS servers, it utilises a Go library that implements WinRM and 
dynamically creates PowerShell scripts to make changes required.

At present it only supports A and CNAME records over HTTP and HTTPS. 

## Incompatibility with Parent Fork

Changes to this fork will break any Terraform usage on the parent fork - specifically the ID field for Terraform has changed to help detect drift and be more flexible in a large environment.ds

## Usage

### Provider configuration

```
provider "windows-dns" {
        server      = "dc.test.local"
        username    = "<username>"
        password    = "<password>"
}
```

#### Required

`server` - Server name or IP address of Microsoft DNS server

`username` - Username to authenticate
 
`pasword` - Password to authenticate

#### Optional

`https` - Use HTTPS protocol instead of HTTP

`insecure` - Set true to bypass HTTPS certificate verification

`port` - Specify a port, otherwise uses defaults (HTTP: 5985, HTTPS: 5986)

------

### Resource configuration

```
resource "windowsdns_record" "test99" {
        domain = "test.local"
        name   = "test99"
        type   = "A"
        value  = "10.0.0.99"
        ttl    = "10m0s"
}
```

#### Required

`domain` - Domain to make changes to

`name` - Name of record

`type` - Type of record

`value` - Value of record

#### Optional

`ttl` - TTL of record as a duration

------

## Importing Existing Resources

Resources can be imported using the terraform import command
```terraform import windows-dns 

The library this uses can be found [here][1]

[1]: https://github.com/Bourne-ID/winrmdns-client

To update to support a new version of Terraform update govendor with the following command
```
for i in $(govendor list | grep terraform | grep -v Bourne-ID | awk '{print $2}'); do govendor fetch "${i}@v0.9.1"; done
```
