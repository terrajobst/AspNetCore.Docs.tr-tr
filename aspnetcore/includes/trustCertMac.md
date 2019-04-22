---
ms.openlocfilehash: 2ec079606cb48670dbc3852482fd8d401e7db44b
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59737332"
---
* Aşağıdaki komutu çalıştırarak HTTPS geliştirme sertifikası güven:

    ```console
    dotnet dev-certs https --trust
    ```

* Yukarıdaki komut, aşağıdaki çıkışı görüntüler:

    ```console
    Trusting the HTTPS development certificate was requested. If the certificate 
    is not already trusted we will run the following command:
    'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain 
    <<certificate>>'
    This command might prompt you for your password to install the certificate on the 
    system keychain.
    The HTTPS developer certificate was generated successfully.
    ```

* Yönetici kullanıcı adı ve parola istenirse girin.  Sertifika artık güvenilir ve yüklenir.

    Bkz: [ASP.NET Core HTTPS geliştirme sertifikasına güvenmek](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) daha fazla bilgi için.