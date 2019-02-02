---
title: Razor bileşenleri ile çalışmaya başlama
author: guardrex
description: Razor bileşenleri framework ile çalışmaya başlama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2019
uid: razor-components/get-started
ms.openlocfilehash: c83af10fd84bc8238f5fe20c66b91ba17de80ae3
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2019
ms.locfileid: "55668158"
---
# <a name="get-started-with-razor-components"></a>Razor bileşenleri ile çalışmaya başlama

[!INCLUDE[](~/includes/razor-components-preview-notice.md)]

## <a name="setup"></a>Kurulum

Aşağıdakileri yükleyin:

1. [.NET core 2.1 SDK](https://go.microsoft.com/fwlink/?linkid=873092) (2.1.500 veya üzeri).
1. [Visual Studio 2017](https://go.microsoft.com/fwlink/?linkid=873093) (15,9 veya üzeri) ile *ASP.NET ve web geliştirme* Seçili iş yükü.
1. En son [Blazor dil Hizmetleri Uzantısı](https://go.microsoft.com/fwlink/?linkid=870389) Visual Studio Market'ten.
1. Komut satırında Blazor şablonları:

   ```console
   dotnet new -i Microsoft.AspNetCore.Blazor.Templates
   ```

Visual Studio'dan ilk projenizi oluşturmak için:

1. Seçin **dosya** > **yeni proje** > **Web** > **ASP.NET Core Web uygulaması**.
1. Emin **.NET Core** ve **ASP.NET Core 2.1** üstünde seçilir.
1. Blazor şablonu seçip **Tamam**.

   ![Yeni uygulama iletişim kutusu](https://msdnshared.blob.core.windows.net/media/2018/07/new-blazor-app-dialog-0.5.0.png)

1. Tuşuna **Ctrl-F5** uygulamayı çalıştırmak için *hata ayıklayıcı olmadan*. Hata ayıklayıcısı ile çalıştırma (**F5**) şu anda desteklenmiyor.

Yeni bir Blazor uygulama komut satırından oluşturmak için:

```console
dotnet new blazor -o BlazorApp1
cd BlazorApp1
dotnet run
```

Tebrikler! Yalnızca ilk Blazor uygulamanızı çalıştırdığınız!

![Blazor uygulama ana sayfası](https://msdnshared.blob.core.windows.net/media/2018/04/blazor-bootstrap-4.png)

## <a name="help--feedback"></a>Yardım ve geri bildirim

Geri bildiriminiz Blazor için Deneysel bu aşamasında bizim için özellikle önemlidir. Bir sorunla karşılaşırsanız veya Blazor çalışırken sorularınız varsa lütfen bize bildirin!

* [Github'da dosya sorunları](https://github.com/aspnet/AspNetCore/issues) çalıştırmanız halinde herhangi bir sorun veya geliştirme önerilerde bulunmak için.
* Sohbet bize ve Blazor toplulukla üzerinde [Gitter](https://gitter.im/aspnet/blazor) takılı kalmasına ya da paylaşmak için nasıl Blazor çalıştığından sizin için.

Blazor denemenize sonra lütfen bize bizim ürün içi ankete katılarak düşüncelerinizi iletin. Yalnızca Blazor proje şablonlarından birini çalıştırıldığında, uygulama giriş sayfasında görüntülenen anket bağlantıya tıklayın:

![Blazor anketi](https://msdnshared.blob.core.windows.net/media/2018/05/blazor-survey-new.png)

## <a name="next-steps"></a>Sonraki adımlar

<xref:tutorials/first-blazor-app>
