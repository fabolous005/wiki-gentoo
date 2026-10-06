<!-- source: https://wiki.gentoo.org/wiki/Curl | group: Gentoo Wiki (Main) | wiki-title: Curl -->
---
title: curl
url: https://wiki.gentoo.org/wiki/Curl
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-19"
fingerprint: be005b1c498638ce
license: CC BY-SA 4.0
---

# curl

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**curl** is a utility for transferring data to or from a server using URLs. While often used with HTTP, a range of protocols are supported.

In its most basic usage, curl sends a request to a server, and prints the response to that request (or an error) on the terminal. An extensive array of options make it a highly flexible tool for fetching content from the network, and diagnosing network services. curl allows customization of the sent request and the handling of the response, and can be thought of as a 'Swiss-army knife' for request-response based network protocols.

A similar tool is [wget](https://wiki.gentoo.org/wiki/Wget), which is included in the @system set on Gentoo systems.

## Installation

### USE flags


### USE flags for
            [net-misc/curl](https://packages.gentoo.org/packages/net-misc/curl)
            
            A Client that groks URLs

| [+adns](https://packages.gentoo.org/useflags/+adns) | Add support for asynchronous DNS resolution | 
| [+alt-svc](https://packages.gentoo.org/useflags/+alt-svc) | Enable alt-svc support | 
| [+ftp](https://packages.gentoo.org/useflags/+ftp) | Enable FTP support | 
| [+hsts](https://packages.gentoo.org/useflags/+hsts) | Enable HTTP Strict Transport Security | 
| [+http2](https://packages.gentoo.org/useflags/+http2) | Enable support for the HTTP/2 protocol | 
| [+http3](https://packages.gentoo.org/useflags/+http3) | Enable HTTP/3 support | 
| [+httpsrr](https://packages.gentoo.org/useflags/+httpsrr) | Enable HTTPS Resource Record support | 
| [+imap](https://packages.gentoo.org/useflags/+imap) | Enable Internet Message Access Protocol support | 
| [+openssl](https://packages.gentoo.org/useflags/+openssl) | Enable openssl ssl backend | 
| [+pop3](https://packages.gentoo.org/useflags/+pop3) | Enable Post Office Protocol 3 support | 
| [+psl](https://packages.gentoo.org/useflags/+psl) | Enable Public Suffix List (PSL) support. See https://daniel.haxx.se/blog/2024/01/10/psl-in-curl/. | 
| [+quic](https://packages.gentoo.org/useflags/+quic) | Enable support for QUIC (RFC 9000); a UDP-based protocol intended to replace TCP | 
| [+smtp](https://packages.gentoo.org/useflags/+smtp) | Enable Simple Mail Transfer Protocol support | 
| [+tftp](https://packages.gentoo.org/useflags/+tftp) | Enable TFTP support | 
| [+websockets](https://packages.gentoo.org/useflags/+websockets) | Enable websockets support | 
| [brotli](https://packages.gentoo.org/useflags/brotli) | Enable Brotli compression support | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [ech](https://packages.gentoo.org/useflags/ech) | Enable Encrypted Client Hello support | 
| [gnutls](https://packages.gentoo.org/useflags/gnutls) | Enable gnutls ssl backend | 
| [gopher](https://packages.gentoo.org/useflags/gopher) | Enable Gopher protocol support | 
| [idn](https://packages.gentoo.org/useflags/idn) | Enable support for Internationalized Domain Names | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [ldap](https://packages.gentoo.org/useflags/ldap) | Add LDAP support (Lightweight Directory Access Protocol) | 
| [mbedtls](https://packages.gentoo.org/useflags/mbedtls) | Enable mbedtls ssl backend | 
| [rustls](https://packages.gentoo.org/useflags/rustls) | Enable Rustls ssl backend | 
| [sasl-scram](https://packages.gentoo.org/useflags/sasl-scram) | Enable snupport for additional SASL SCRAM-SHA authentication methods via net-misc/gsasl | 
| [ssh](https://packages.gentoo.org/useflags/ssh) | Enable SSH urls in curl using libssh2 | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Enable crypto engine support (via openssl if USE='-gnutls -nss') | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [telnet](https://packages.gentoo.org/useflags/telnet) | Enable Telnet protocol support | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [zstd](https://packages.gentoo.org/useflags/zstd) | Enable support for ZSTD compression | 

### Emerge

Install [net-misc/curl](https://packages.gentoo.org/packages/net-misc/curl):

`root #``emerge --ask net-misc/curl`
## Usage

### curl'ing a Web page

To "curl" a web page, call the curl command with the appropriate URL:

`user $``curl` [https://example.com](https://example.com)```
<!doctype html>
<html>
<head>
    <title>Example Domain</title>
    <meta charset="utf-8" />
    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style type="text/css">
    body {
        background-color: #f0f0f2;
        margin: 0;
        padding: 0;
        font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;
    }
    div {
        width: 600px;
        margin: 5em auto;
        padding: 2em;
        background-color: #fdfdff;
        border-radius: 0.5em;
        box-shadow: 2px 3px 7px 2px rgba(0,0,0,0.02);
    }
    a:link, a:visited {
        color: #38488f;
        text-decoration: none;
    }
    @media (max-width: 700px) {
        div {
            margin: 0 auto;
            width: auto;
        }
    }
    </style>
</head>
<body>
<div>
    <h1>Example Domain</h1>
    <p>This domain is for use in illustrative examples in documents. You may use this
    domain in literature without prior coordination or asking for permission.</p>
    <p><a href="https://www.iana.org/domains/example">More information...</a></p>
</div>
</body>
</html>
```
### Following redirect responses

By default, curl returns the first response from the server verbatim. This might not always be desirable, for example if the server returns an HTTP 3xx redirect response — which a browser would usually follow transparently — curl will only retrieve that response and do nothing else.

Use the `--location` / `-L` option to instruct curl to follow redirections with a new request when receiving a redirect response.

### Saving files to disk

By default, curl writes its output to the terminal. This may surprise users used to the behavior of [wget](https://wiki.gentoo.org/wiki/Wget), a similar utility. This is often useful when piping the output of curl to another utility.

Files can be saved to disk by specifying the destination file name with the `--output` / `-o` flag:

`user $``curl` [https://example.com](https://example.com) -L -o index.html
The `--remote-name` / `-O` option saves the file with the same name as on the server. This is most useful when the URL contains a file name. If the file name can not be determined, such as 'default' content where the URL contains no file name, the output is saved as curl\_response.

`user $``curl` [https://example.com/index.html](https://example.com/index.html) -OL
### Proxies

curl supports proxy configuration as either command line options or environmental variables.

It expects a "proxy string" formatted like `<[protocol://]host[:port]>`.

Supported protocols are

- HTTP
- HTTPS (from version 7.87.0)
- SOCKS, in different versions:
  - `socks4://`
  - `socks4a://`
  - `socks5://`
  - `socks5h://`



#### Environment variables

The `HTTP_PROXY` (for plaintext HTTP) and/or `HTTPS_PROXY` environment variables need to be set. The `NO_PROXY` variable can be set to a comma-separated list of hosts to exclude.

For example, when using a remote [Squid](https://wiki.gentoo.org/wiki/Squid) on port 3128 for both HTTP and HTTPS:

`user $``export http_proxy=`[http://squid.proxy:3128](http://squid.proxy:3128)
`user $````
export https_proxy="${http_proxy}"
```
`user $``curl -v` [https://example.com](https://example.com) # Uses proxy env variable https_proxy == '[http://squid.proxy:3128'](http://squid.proxy:3128')
#### Command line options

The two main options to control proxy usage are:

- `-x <proxy-string>` / `--proxy`, to set the proxy string to be used.
- `--noproxy <host-list>` to supply the list of hosts to exclude from proxying.



Authentication credentials for proxy servers can be also passed as a command line option, via `-U <user:password>` / `--proxy-user`). For example:

`user $``curl --proxy-user jdoe:secret --proxy` [http://proxy](http://proxy) [https://example.com](https://example.com)
### Use curl as Portage's FETCHCOMMAND

It's possible to replace the standard [FETCHCOMMAND](https://wiki.gentoo.org/wiki/FETCHCOMMAND) variable in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf) (e.g. to use a SOCKS5 Proxy for fetching, which is not supported by standard [wget](https://wiki.gentoo.org/wiki/Wget)):

**`/etc/portage/make.conf`**

```
FETCHCOMMAND="curl --progress-bar --location --connect-timeout 3 --ftp-pasv --user-agent \"Portage (Gentoo, https://www.gentoo.org) distfile-fetch\" --output  \"\${DISTDIR}/\${FILE}\" \"\${URI}\""
RESUMECOMMAND="curl --continue-at - --progress-bar --location --connect-timeout 3 --ftp-pasv --user-agent \"Portage (Gentoo, https://www.gentoo.org) distfile-fetch\" --output  \"\${DISTDIR}/\${FILE}\" \"\${URI}\""
```
The `--location` option is important, since curl won't follow redirects like wget.

`--progress-bar` is optional but suggested: the standard "meter table" output could mess up the emerge-fetch.log file.
