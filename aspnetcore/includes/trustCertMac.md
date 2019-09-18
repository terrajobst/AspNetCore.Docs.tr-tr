* <span data-ttu-id="5fe45-101">Aşağıdaki komutu çalıştırarak HTTPS geliştirme sertifikasına güvenin:</span><span class="sxs-lookup"><span data-stu-id="5fe45-101">Trust the HTTPS development certificate by running the following command:</span></span>

    ```dotnetcli
    dotnet dev-certs https --trust
    ```

* <span data-ttu-id="5fe45-102">Yukarıdaki komut aşağıdaki çıktıyı görüntüler:</span><span class="sxs-lookup"><span data-stu-id="5fe45-102">The preceding command displays the following output:</span></span>

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* <span data-ttu-id="5fe45-103">İstenirse yönetici kullanıcı adını ve parolasını girin.</span><span class="sxs-lookup"><span data-stu-id="5fe45-103">Enter the admin username and password if prompted.</span></span>  <span data-ttu-id="5fe45-104">Sertifika şimdi yüklenir ve güvenilir olur.</span><span class="sxs-lookup"><span data-stu-id="5fe45-104">The certificate will now be installed and trusted.</span></span>

    <span data-ttu-id="5fe45-105">Daha fazla bilgi için bkz. [ASP.NET Core https geliştirme sertifikasına güvenin](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) .</span><span class="sxs-lookup"><span data-stu-id="5fe45-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>