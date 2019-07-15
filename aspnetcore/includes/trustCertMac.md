* <span data-ttu-id="6437f-101">Aşağıdaki komutu çalıştırarak HTTPS geliştirme sertifikası güven:</span><span class="sxs-lookup"><span data-stu-id="6437f-101">Trust the HTTPS development certificate by running the following command:</span></span>

    ```console
    dotnet dev-certs https --trust
    ```

* <span data-ttu-id="6437f-102">Yukarıdaki komut, aşağıdaki çıkışı görüntüler:</span><span class="sxs-lookup"><span data-stu-id="6437f-102">The preceding command displays the following output:</span></span>

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* <span data-ttu-id="6437f-103">Yönetici kullanıcı adı ve parola istenirse girin.</span><span class="sxs-lookup"><span data-stu-id="6437f-103">Enter the admin username and password if prompted.</span></span>  <span data-ttu-id="6437f-104">Sertifika artık güvenilir ve yüklenir.</span><span class="sxs-lookup"><span data-stu-id="6437f-104">The certificate will now be installed and trusted.</span></span>

    <span data-ttu-id="6437f-105">Bkz: [ASP.NET Core HTTPS geliştirme sertifikasına güvenmek](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="6437f-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>