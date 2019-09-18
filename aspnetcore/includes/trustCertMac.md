* Aşağıdaki komutu çalıştırarak HTTPS geliştirme sertifikasına güvenin:

    ```dotnetcli
    dotnet dev-certs https --trust
    ```

* Yukarıdaki komut aşağıdaki çıktıyı görüntüler:

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* İstenirse yönetici kullanıcı adını ve parolasını girin.  Sertifika şimdi yüklenir ve güvenilir olur.

    Daha fazla bilgi için bkz. [ASP.NET Core https geliştirme sertifikasına güvenin](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) .