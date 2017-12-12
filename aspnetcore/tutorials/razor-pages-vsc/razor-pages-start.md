---
title: "Visual Studio Code ile ASP.NET Core Razor sayfalarında ile çalışmaya başlama"
author: rick-anderson
description: "Razor sayfalarında ASP.NET Core kullanarak Visual Studio Code ile çalışmaya başlama"
keywords: "ASP.NET, iskele kurma Entity Framework Çekirdek, EF, EF çekirdek, veritabanı, mac, macOS çekirdek, Razor sayfalarının, Visual Studio Code, kod"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 1b9dff14fa98314601fa44aa229aef6b73bb79d0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="getting-started-with-razor-pages-in-aspnet-core-with-visual-studio-code"></a>Visual Studio Code ile ASP.NET Core Razor sayfalarında ile çalışmaya başlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğretici, bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel öğretir. Tamamlamanız önerilir [Razor sayfalarının giriş](xref:mvc/razor-pages/index) bu öğreticiye başlamadan önce. Razor sayfalarının ASP.NET Core web uygulamaları için kullanıcı Arabirimi oluşturmak için önerilen yöntem olduğu.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yükleyin:

* [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya daha yenisi
* [Visual Studio Code](https://code.visualstudio.com)
* VS Code [C# uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp) 

## <a name="create-a-razor-web-app"></a>Bir Razor web uygulaması oluşturma

Terminal durumundan, aşağıdaki komutları çalıştırın:

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

Yukarıdaki komutları kullanım [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) oluşturun ve Razor sayfalarının proje çalıştırın. Http://localhost: 5000 uygulamayı görüntülemek için bir tarayıcı açın.

![Giriş veya dizin sayfası](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE[razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Projeyi açın

Uygulamayı kapatın CTRL + C tuşlarına basın.

Visual Studio kodundan (VS Code) seçin **Dosya > Klasör Aç**ve ardından *RazorPagesMovie* klasör.

- Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'RazorPagesMovie' eksik. ileti Bunları eklensin mi?"
- Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.

### <a name="launch-the-app"></a>Uygulamayı başlatın

Hata ayıklama olmadan uygulamayı başlatmak için CTRL + F5 tuşuna basın. Alternatif olarak, gelen **hata ayıklama** menüsünde, select **hata ayıklama olmadan Başlat**.

Sonraki öğreticide biz projenize bir model ekleyin. 

>[!div class="step-by-step"]
[Sonraki: bir modeli ekleme](xref:tutorials/razor-pages-vsc/model)  
