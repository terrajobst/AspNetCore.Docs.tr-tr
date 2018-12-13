---
title: ASP.NET Core kullanmaya başlayın
author: rick-anderson
description: Oluşturur ve ASP.NET Core kullanarak basit bir Hello World uygulaması çalıştıran bir hızlı öğretici.
ms.author: riande
ms.custom: mvc
ms.date: 12/11/2018
uid: getting-started
ms.openlocfilehash: dc85fe9189c93476859a6c00d60ec24d6eeec3fa
ms.sourcegitcommit: a16352c1c88a71770ab3922200a8cd148fb278a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53335318"
---
# <a name="tutorial-get-started-with-aspnet-core"></a>Öğretici: ASP.NET Core kullanmaya başlayın

Bu öğreticide, ASP.NET Core web uygulaması oluşturmak için .NET Core komut satırı arabirimi kullanmayı gösterir.

Öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> * Bir web uygulaması projesi oluşturun.
> * Yerel HTTPS etkinleştirin.
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

## <a name="enable-local-https"></a>Yerel HTTPS'yi etkinleştirme

HTTPS geliştirme sertifikasına güvenmek:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
dotnet dev-certs https --trust
```

Yukarıdaki komut, aşağıdaki iletişim kutusunu görüntüler:

![Güvenlik Uyarısı iletişim kutusu](_static/cert.png)

Seçin **Evet** geliştirme sertifikasına güvenmek kabul etmesi durumunda.

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```console
dotnet dev-certs https --trust
```

Yukarıdaki komut, şu iletiyi görüntüler:

*HTTPS geliştirme sertifikaya güvenme istendi. Sertifika zaten güvenilir değilse aşağıdaki komutu çalıştıracağız:* `'sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain <<certificate>>'`.  
* Bu komut, üzerinde sistem Anahtarlık sertifikayı yüklemek, parola isteyebilir.

Parola: *

Geliştirme sertifikasına güvenmek kabul ediyorsanız, parolanızı girin.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

HTTPS geliştirme sertifikasına güvenmek nasıl Linux dağıtımınız için belgelere bakın.

---

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Aşağıdaki komutları çalıştırın:

```console
cd aspnetcoreapp
dotnet run
```

Gözat [ https://localhost:5001 ](https://localhost:5001). Tıklayın **kabul** gizlilik ve tanımlama bilgisi ilkesini kabul etmek için. Bu uygulama, kişisel bilgileri tutmak değil.

## <a name="edit-a-razor-page"></a>Bir Razor sayfası Düzenle

Açık *Pages/Index.cshtml* ve sayfanın vurgulanan aşağıdaki işaretlemeyle değiştirin:

[!code-cshtml[](sample/index.cshtml?highlight=9)]

Gözat [ https://localhost:5001 ](https://localhost:5001), değişiklikleri görüntülenir doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Bir web uygulaması projesi oluşturun.
> * Yerel HTTPS etkinleştirin.
> * Projeyi çalıştırın.
> * Bir değişiklik yapın.

ASP.NET Core hakkında daha fazla bilgi için girişine bakın:

> [!div class="nextstepaction"]
> <xref:index>
