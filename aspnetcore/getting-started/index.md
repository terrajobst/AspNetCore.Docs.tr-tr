---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: ASP.NET Core kullanarak temel bir Merhaba Dünya uygulaması oluşturan ve çalıştıran kısa bir öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 01/07/2020
uid: getting-started
ms.openlocfilehash: c806bd1e79dea9119f1c9e99d0a2b9742a10987a
ms.sourcegitcommit: ef1720cb733908f36a54825d84c3461c5280bdbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75737483"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Öğretici: ASP.NET Core kullanmaya başlayın

Bu öğreticide, bir ASP.NET Core Web uygulaması oluşturmak ve çalıştırmak için .NET Core komut satırı arabirimi 'nin nasıl kullanılacağı gösterilmektedir.

Şunları öğreneceksiniz:

> [!div class="checklist"]
> * Bir Web uygulaması projesi oluşturun.
> * Geliştirme sertifikasına güvenin.
> * Uygulamayı çalıştırın.
> * Razor sayfasını düzenleyin.

Sonunda, yerel makinenizde çalışan bir çalışan Web uygulamanız olacaktır.

![Web uygulaması giriş sayfası](_static/home-page.png)

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE[](~/includes/3.1-SDK.md)]

## <a name="create-a-web-app-project"></a>Web uygulaması projesi oluşturma

Bir komut kabuğu açın ve şu komutu girin:

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

Yukarıdaki komut:

* Yeni bir Web uygulaması oluşturur.  
* `-o aspnetcoreapp` parametresi, uygulama için kaynak dosyalarla *aspnetcoreapp* adlı bir dizin oluşturur.

### <a name="trust-the-development-certificate"></a>Geliştirme sertifikasına güven

HTTPS geliştirme sertifikasına güvenin:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```dotnetcli
dotnet dev-certs https --trust
```

Yukarıdaki komutta aşağıdaki iletişim kutusu görüntülenir:

![Güvenlik Uyarısı iletişim kutusu](~/getting-started/_static/cert.png)

Geliştirme sertifikasına güvenmeyi kabul ediyorsanız **Evet** ' i seçin.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```dotnetcli
dotnet dev-certs https --trust
```

Yukarıdaki komut aşağıdaki iletiyi görüntüler:

*Https geliştirme sertifikasına güvenmek istendi. Sertifikaya zaten güvenilmiyorsa, şu komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`

Bu komut, sertifikayı sistem anahtarlığınıza yüklemek için parolanızı isteyebilir. Geliştirme sertifikasına güvenmeyi kabul ediyorsanız parolanızı girin.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

HTTPS geliştirme sertifikasına güvenmek için Linux dağılııza yönelik belgelere bakın.

---

Daha fazla bilgi için bkz [. asp.NET Core https geliştirme sertifikasına güven](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Aşağıdaki komutları çalıştırın:

```dotnetcli
cd aspnetcoreapp
dotnet watch run
```

Komut kabuğu, uygulamanın başlatıldığını gösteriyorsa, [https://localhost:5001](https://localhost:5001)gidin.

## <a name="edit-a-razor-page"></a>Razor sayfasını düzenleme

*Pages/Index. cshtml* dosyasını açın ve sayfayı aşağıdaki vurgulanmış işaretlerle değiştirin ve kaydedin:

[!code-cshtml[](sample/index.cshtml?highlight=9)]

[https://localhost:5001](https://localhost:5001)gidin, sayfayı yenileyin ve değişikliklerin görüntülendiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir Web uygulaması projesi oluşturun.
> * Geliştirme sertifikasına güvenin.
> * Projeyi çalıştırın.
> * Değişiklik yapın.

ASP.NET Core hakkında daha fazla bilgi edinmek için bkz. giriş bölümünde önerilen öğrenme yolu:

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
