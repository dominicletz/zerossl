
# Zerossl client library

Zerossl is a Elixir library to automatically manage and refresh your Zerossl certificates natively, without the need for extra applications like [acme.sh](https://github.com/acmesh-official/acme.sh)  bash script or [certbot](https://certbot.eff.org/) clients.
The client implements the [ACME(v2) rfc8555](https://datatracker.ietf.org/doc/html/rfc8555) `http-01` challenge auth mechanism to issue and refresh a genuine certificate against [Zerossl](https://zerossl.com/)

## Installation

If [available in Hex](https://hex.pm/docs/publish), the package can be installed by adding `zerossl` 
to your list of dependencies in `mix.exs`:

  
```elixir
def  deps  do
[
  {:zerossl, "~> 0.1.0"}
]
end
```

## Configuration

In your `config.exs` or `prod.exs` add the following config:

```elixir
config  :zerossl,
  user_email:  "myfancy-email@gmail.com",
  cert_domain:  "myfancy-domain.com",
  certfile:  "./cert.pem",
  keyfile:  "./key.pem",
```
where
* `user_email` is the email used to register your account on Zerossl;
* `cert_domain` is the domain that resolves your software application project, and for which you want to issue the certificate
* `certfile` and `keyfile` are the places where you want to store your certificate and key respectively.

Note that key and certificate are always stored on FS to reduce the number of interrogations of Zerossl servers when the service is rebooted.

### Additional optional config
* `selfsigned` [default: false]: forces "dryrun" selfsigned certificate generation without zerossl exchanges.
* `update_handler` [default: nil]: permits to specify a module that implements the `Zerossl.UpdateHandler` behavior to get notifications when the certificate is renewed. This can be used as trigger to reload a listening HTTPs server with the new certificate/key. The handler is always invoked upon start of the process: subordinating the start of the HTTPs server to the call by this handler is legitimate.
* `access_key`: **This is at the moment not used** unless for special operations like [getting EAB credentials](https://zerossl.com/documentation/acme/generate-eab-credentials/) without email. Since the same operation is atm carried out through the account email, currently I have no use of this attribute nor I have found what is the benefit of using it in place of the `user_email`.
