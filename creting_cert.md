openssl req -new -sha256 \
    -out cert.csr \
    -newkey rsa:2048 \
    -nodes \
    -keyout key.pem \
    -config req.conf
	
openssl req -text -noout -in private.csr

openssl x509 -req \
    -days 3650 \
    -in cert.csr \
    -signkey key.pem \
    -out cert.pem \
    -extensions req_ext \
    -extfile req.conf
    
    req.conf

    [ req ]
default_bits       = 2048
distinguished_name = req_distinguished_name
req_extensions     = req_ext

[ req_distinguished_name ]
countryName                 = Country Name (2 letter code)
countryName_default         = RU
stateOrProvinceName         = State or Province Name (full name)
stateOrProvinceName_default = Moscow
localityName                = Locality Name (eg, city)
localityName_default        = Moscow
organizationName            = Organization Name (eg, company)
organizationName_default    = HomeLabs Ltd.
commonName                  = Common Name (e.g. server FQDN or YOUR name)
commonName_max              = 64
commonName_default          = homneassistant.loc

[ req_ext ]
subjectAltName = @alt_names

[alt_names]
IP.1   = <YOUR IP>
DNS.1  = homeassistant.loc


openssl x509 -req -days 3650 -in traefik_cert.csr -out traefik_cert.pem -extensions req_ext -extfile req.conf -CA ca.pem -CAkey ca-key.pem
