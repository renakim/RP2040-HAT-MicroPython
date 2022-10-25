# Certificate

Certificate files are required to connect a device to the AWS IoT endpoint.

* Device Certificate
* Device Private key


For more detail about AWS IoT resource, refer to official guide.

* [Create AWS IoT Resouces](https://docs.aws.amazon.com/en_us/iot/latest/developerguide/create-iot-resources.html)


## Convert certificate

Convert the downloaded certificate files to `der` form.

```
openssl x509 -outform der -in device_cert.pem.cert -out device_cert.crt.der
openssl rsa -inform pem -in privateKey.pem.key -outform DER -out privateKey.key.der
```

## Upload and Config path

Upload the certificates and config path.

`main.py`

```python
# Certificate path
cert_file = 'cert/device_cert.crt.der'
key_file = 'cert/privateKey.key.der'
```