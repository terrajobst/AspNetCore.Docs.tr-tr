---
ms.openlocfilehash: 2ec079606cb48670dbc3852482fd8d401e7db44b
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59737332"
---
* <span data-ttu-id="57db7-101">Aşağıdaki komutu çalıştırarak HTTPS geliştirme sertifikası güven:</span><span class="sxs-lookup"><span data-stu-id="57db7-101">Trust the HTTPS development certificate by running the following command:</span></span>

    ```console
    dotnet dev-certs https --trust
    ```

* <span data-ttu-id="57db7-102">Yukarıdaki komut, aşağıdaki çıkışı görüntüler:</span><span class="sxs-lookup"><span data-stu-id="57db7-102">The preceding command displays the following output:</span></span>

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* <span data-ttu-id="57db7-103">Yönetici kullanıcı adı ve parola istenirse girin.</span><span class="sxs-lookup"><span data-stu-id="57db7-103">Enter the admin username and password if prompted.</span></span>  <span data-ttu-id="57db7-104">Sertifika artık güvenilir ve yüklenir.</span><span class="sxs-lookup"><span data-stu-id="57db7-104">The certificate will now be installed and trusted.</span></span>

    <span data-ttu-id="57db7-105">Bkz: [ASP.NET Core HTTPS geliştirme sertifikasına güvenmek](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="57db7-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>