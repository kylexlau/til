# Understanding SSL for Puppet

## What is Public Key Cryptography

- `Key pair`(public key and private key) are large, nearly impossible
  to guess numbers, which are mathematically related to each other in
  a special way.
- A private key can't be reverse engineered from its corresponding
  public key.
- Using one part of the key pair, you can perform a calculation that
  can only be reversed by using the other part.
- Key pairs can encypt information: if you have a public key, you can
  encrypt a message so that it can only be deciphered by the
  corresponding private key.
- Key pairs can sign information: if you have a private key, you can
  combining the message and the private key to create a string of
  unique nonsense(signature), so the public key owner can prove the
  message was actually signed by the private key owner and determine
  whether the message was altered after the signature was made.

## What are Certificates and PKI

- `PKI`(Public Key Infrastructure): a way to associate public keys with
  trusted information about their owners, so each participant doesn't
  have to keep a personal list of all known public keys.
- `X.509 standard`, specifies formats for public key certificates,
  certificate revocation lists, attribute certificates, and a
  certification path validation algorithm.
- `OpenSSL`, is a software library to be used in applications that need
  to secure communications against eavesdropping or need to ascertain
  the identity of the party at the other end.
- `Certificate`, a cryptographic identification document that contains:
  a public key, metadata about the certificate and its owner, a signature
  from the CA(certificate authority).
- Puppet store certificates in `PEM`(Privacy-enhanced Electronic Mail) format.
- `CA`(Certificate Authority), a trusted person or institution that controls a
  key pair.  Root CA and intermediate CA.
- `CSR`, Certificate Signing Requests, a specially formatted cryptographic
  document that contains the applicant's public key and any metadata that the
  applicant wants to have in their certificate.  It's just a certificate minus
  the CA signature.
- `CRL`, Certificate Revocation List, contains the serial numbers of any
  certificates that should no longer be trusted.

## What is TLS/SSL

- `TLS`, Transport Layer Security, is a protocol that uses an X.509
  PKI to create secure and authenticated channels of network
  communication.
- `SSL`, Secure Socket Layer, is an older version of that same
  protocol, which is still in widespread use.
- `TLS` and `SSL` both refer to essentially the same thing.
- Web SSL certificates contain a list of domain names that are allowed
  to present that certificate.

## What is HTTPS

- `HTTPS`, the standard HTTP protocol wrapped with SSL.  The entire
  HTTP protocol is wrapped, including headers, this means that even
  URLs, parameters, and POST data will be encrypted.
- Puppet uses HTTPS for all of its traffic.
- Any port can be used to serve HTTPS, the usual convention is port
  443, Puppet uses port 8140.
- `SSL terminating proxy`, a single component that handles SSL. It
  handles the incoming connection, then send a second unencrypted HTTP
  request to the real application server.  When the proxy receive a
  reply, it will forward it to the client along the original
  connection.

## References

- http://docs.puppetlabs.com/background/ssl/
- http://www.masterzen.fr/2010/11/14/puppet-ssl-explained/
- [X.509 standard](http://tools.ietf.org/html/rfc5280)
- [How the RSA Cipher Works](http://www.muppetlabs.com/~breadbox/txt/rsa.html)
- https://en.wikipedia.org/wiki/Privacy-enhanced_Electronic_Mail
