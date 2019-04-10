---
ms.openlocfilehash: a9bdff481b1a72a9ee19f4e51fada177530c0cbb
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472329"
---
*  Aşağıdaki komutu çalıştırarak HTTPS geliştirme sertifikası güven:

    ```console
    dotnet dev-certs https --trust
    ```

    Yukarıdaki komut, aşağıdaki iletişim kutusunu görüntüler:

    ![Güvenlik Uyarısı iletişim kutusu](~/getting-started/_static/cert.png)

*    Seçin **Evet** geliştirme sertifikasına güvenmek kabul etmesi durumunda.

     Bkz: [ASP.NET Core HTTPS geliştirme sertifikasına güvenmek](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) daha fazla bilgi için.