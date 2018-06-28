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
