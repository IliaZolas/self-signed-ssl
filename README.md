To generate SSL/TLS certificates without passwords using OpenSSL, you can follow these steps:

1. **Generate a Private Key without a Passphrase:**
    
    Use the `openssl genpkey` command to generate a private key without a passphrase. Replace `path/to/localhost-no-passphrase.key` with the desired path and filename for your private key:
    
    ```bash
    openssl genpkey -algorithm RSA -out path/to/localhost-no-passphrase.key
    
    ```
    
    This command will generate an unencrypted private key in PEM format.
    
2. **Generate a Certificate Signing Request (CSR):**
    
    Next, you'll generate a Certificate Signing Request (CSR) that you can use to create a certificate. Use the following command:
    
    ```bash
    openssl req -new -key path/to/localhost-no-passphrase.key -out path/to/localhost.csr
    
    ```
    
    You'll be prompted to enter information about your organization and the certificate. You can leave some fields blank, but make sure to specify the Common Name (CN) as the domain name for which you're creating the certificate (e.g., localhost).
    
3. **Create a Self-Signed Certificate:**
    
    To create a self-signed certificate from the CSR, use the following command:
    
    ```bash
    openssl x509 -req -days 365 -in path/to/localhost.csr -signkey path/to/localhost-no-passphrase.key -out path/to/localhost.crt
    
    ```
    
    This command will create a self-signed certificate (`localhost.crt`) that is valid for 365 days.
    

Now, you have a private key (`localhost-no-passphrase.key`) and a self-signed certificate (`localhost.crt`) that you can use for your local development server without a passphrase.

Make sure to update your Node.js server code to use these new files:

```jsx
const httpsOptions = {
  key: fs.readFileSync('path/to/localhost-no-passphrase.key'),  // Use the new key without a passphrase
  cert: fs.readFileSync('path/to/localhost.crt'),              // Use the new certificate
};

```

Remember that using a certificate without a passphrase is less secure, so only use it for development purposes. In a production environment, it's strongly recommended to protect your private key with a passphrase and follow proper security practices for certificate management.
