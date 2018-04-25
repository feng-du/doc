 


--------- Certificate Authority -----------------------------------------------------------

mkdir myCertificateAuthority

mkdir certificates

echo '01' > serial


openssl req -x509 -out certificateAuthorityCertificate.pem -newkey rsa:2048 -keyout certificateAuthorityPrivateKey.pem -days 365 -config /home/fengdu/myCertificateAuthority/caconf.cnf


-------- Certificate Signing Request (CSR) -----------------------------------------------

mkdir myCertificates

openssl req -out cefcfcoCsr.pem -newkey rsa:2048 -keyout cefcfcoPrivateKey.pem -config /home/fengdu/myCertificates/cefcfco/cefcfco.cnf

openssl ca -in /home/fengdu/myCertificates/cefcfco/cefcfcoCsr.pem -out /home/fengdu/myCertificates/cefcfco/cefcfcoCertificate.pem -config /home/fengdu/myCertificateAuthority/caconf.cnf


-------- PKCS12 --------------------------------------------------------------------------

openssl pkcs12 -export -in cefcfcoCertificate.pem -inkey cefcfcoPrivateKey.pem -out cefcfco.pfx