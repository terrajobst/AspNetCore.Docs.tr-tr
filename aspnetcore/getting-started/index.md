---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulaması çalıştıran kısa bir öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 5/15/2019
uid: getting-started
ms.openlocfilehash: 9227dcfbc84376d9d73bc6fc0dd76085779acae1
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610317"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Öğretici: ASP.NET Core kullanmaya başlayın

Bu öğreticide, .NET Core komut satırı arabirimi oluşturmak ve bir ASP.NET Core web uygulaması çalıştırmak için kullanılacak gösterilmektedir.

Öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> * Bir web uygulaması projesi oluşturun.
> * Geliştirme sertifikasına güvenmek.
> * Uygulamayı çalıştırın.
> * Bir Razor sayfası düzenleyin.

Sonunda, yerel makinenizde çalışan bir çalışan web uygulaması oluşturmuş olacaksınız.

![Web uygulama ana sayfası](_static/home-page.png)

## <a name="prerequisites"></a>Önkoşullar

* [.NET core SDK'sını 2.2](https://www.microsoft.com/net/download/all)

## <a name="create-a-web-app-project"></a>Bir web uygulaması projesi oluşturma

Bir komut kabuğunu açın ve aşağıdaki komutu girin:

```console
dotnet new webapp -o aspnetcoreapp
```

### <a name="trust-the-development-certificate"></a>Geliştirme sertifikasına güvenmek

HTTPS geliştirme sertifikasına güvenmek:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
dotnet dev-certs https --trust
```

Yukarıdaki komut, aşağıdaki iletişim kutusunu görüntüler:

![Güvenlik Uyarısı iletişim kutusu](~/getting-started/_static/cert.png)

Seçin **Evet** geliştirme sertifikasına güvenmek kabul etmesi durumunda.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```console
dotnet dev-certs https --trust
```

Yukarıdaki komut, şu iletiyi görüntüler:

*HTTPS geliştirme sertifikaya güvenme istendi. Sertifika zaten güvenilir değilse aşağıdaki komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`

Bu komut, üzerinde sistem Anahtarlık sertifikayı yüklemek, parola isteyebilir. Geliştirme sertifikasına güvenmek kabul ediyorsanız, parolanızı girin.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Linux için Windows alt sistemi için bkz: [Linux için Windows alt sistemi güven HTTPS sertifikadan](xref:security/enforcing-ssl#wsl).

HTTPS geliştirme sertifikasına güvenmek nasıl Linux dağıtımınız için belgelere bakın.

---

Daha fazla bilgi için [ASP.NET Core HTTPS geliştirme sertifikasına güvenmek](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Aşağıdaki komutları çalıştırın:

```console
cd aspnetcoreapp
dotnet run
```

Komut kabuğu uygulama başladığını belirtir sonra Gözat [ https://localhost:5001 ](https://localhost:5001). Tıklayın **kabul** gizlilik ve tanımlama bilgisi ilkesini kabul etmek için. Bu uygulama, kişisel bilgileri tutmak değil.

## <a name="edit-a-razor-page"></a>Bir Razor sayfası Düzenle

Açık *Pages/Index.cshtml* ve sayfanın vurgulanan aşağıdaki işaretlemeyle değiştirin:

[!code-cshtml[](sample/index.cshtml?highlight=9)]

Gözat [ https://localhost:5001 ](https://localhost:5001), değişiklikleri görüntülenir doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir web uygulaması projesi oluşturun.
> * Geliştirme sertifikasına güvenmek.
> * Projeyi çalıştırın.
> * Bir değişiklik yapın.

ASP.NET Core hakkında daha fazla bilgi için önerilen öğrenme yolunu giriş bakın:

> [!div class="nextstepaction"]
> <xref:index#recommended-learning-path>
