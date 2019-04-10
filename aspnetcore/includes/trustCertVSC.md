---
ms.openlocfilehash: a9bdff481b1a72a9ee19f4e51fada177530c0cbb
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472329"
---
*  <span data-ttu-id="d4b71-101">Aşağıdaki komutu çalıştırarak HTTPS geliştirme sertifikası güven:</span><span class="sxs-lookup"><span data-stu-id="d4b71-101">Trust the HTTPS development certificate by running the following command:</span></span>

    ```console
    dotnet dev-certs https --trust
    ```

    <span data-ttu-id="d4b71-102">Yukarıdaki komut, aşağıdaki iletişim kutusunu görüntüler:</span><span class="sxs-lookup"><span data-stu-id="d4b71-102">The preceding command displays the following dialog:</span></span>

    ![Güvenlik Uyarısı iletişim kutusu](~/getting-started/_static/cert.png)

*    <span data-ttu-id="d4b71-104">Seçin **Evet** geliştirme sertifikasına güvenmek kabul etmesi durumunda.</span><span class="sxs-lookup"><span data-stu-id="d4b71-104">Select **Yes** if you agree to trust the development certificate.</span></span>

     <span data-ttu-id="d4b71-105">Bkz: [ASP.NET Core HTTPS geliştirme sertifikasına güvenmek](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) daha fazla bilgi için.</span><span class="sxs-lookup"><span data-stu-id="d4b71-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>