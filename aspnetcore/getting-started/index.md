---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: ASP.NET Core kullanarak temel bir Merhaba Dünya uygulaması oluşturan ve çalıştıran kısa bir öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 05/15/2019
uid: getting-started
ms.openlocfilehash: d1edf91f1b37ba2b69732471dc6c1f306ac5ad24
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081116"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Öğretici: ASP.NET Core kullanmaya başlayın

Bu öğreticide, bir ASP.NET Core Web uygulaması oluşturmak ve çalıştırmak için .NET Core komut satırı arabirimi 'nin nasıl kullanılacağı gösterilmektedir.

Şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Bir Web uygulaması projesi oluşturun.
> * Geliştirme sertifikasına güvenin.
> * Uygulamayı çalıştırın.
> * Razor sayfasını düzenleyin.

Sonunda, yerel makinenizde çalışan bir çalışan Web uygulamanız olacaktır.

![Web uygulaması giriş sayfası](_static/home-page.png)

## <a name="prerequisites"></a>Önkoşullar

* [.NET Core 2,2 SDK](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a>Web uygulaması projesi oluşturma

Bir komut kabuğu açın ve şu komutu girin:

```dotnetcli
dotnet new webapp -o aspnetcoreapp
```

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

*HTTPS geliştirme sertifikasına güvenmek istendi. Sertifika zaten güvenilmiyorsa, şu komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`

Bu komut, sertifikayı sistem anahtarlığınıza yüklemek için parolanızı isteyebilir. Geliştirme sertifikasına güvenmeyi kabul ediyorsanız parolanızı girin.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Linux için Windows alt sistemi için bkz. [Linux Için Windows alt sistemi 'NDEN https sertifikasına güvenin](xref:security/enforcing-ssl#wsl).

HTTPS geliştirme sertifikasına güvenmek için Linux dağılııza yönelik belgelere bakın.

---

Daha fazla bilgi için bkz [. asp.NET Core https geliştirme sertifikasına güven](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Aşağıdaki komutları çalıştırın:

```dotnetcli
cd aspnetcoreapp
dotnet run
```

Komut kabuğu, uygulamanın başlatıldığını gösteriyorsa, öğesine [https://localhost:5001](https://localhost:5001)gidin. Gizlilik ve tanımlama bilgisi ilkesini kabul etmek için **kabul et** ' e tıklayın. Bu uygulama, kişisel bilgileri tutmak değil.

## <a name="edit-a-razor-page"></a>Razor sayfasını düzenleme

*Pages/Index. cshtml* dosyasını açın ve sayfayı aşağıdaki vurgulanmış işaretlerle değiştirin:

[!code-cshtml[](sample/index.cshtml?highlight=9)]

[https://localhost:5001](https://localhost:5001)' A gidin ve değişikliklerin görüntülendiğini doğrulayın.

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
