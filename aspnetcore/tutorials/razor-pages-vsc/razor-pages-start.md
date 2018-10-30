---
title: Visual Studio code'da ASP.NET Core Razor sayfaları kullanmaya başlama
author: rick-anderson
description: Bir Visual Studio Code ile ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: f18d0a8b3ce24c9844b02f8a0b6360f7e1b1bdb7
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244859"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a>Visual Studio code'da ASP.NET Core Razor sayfaları kullanmaya başlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide bir ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir. Tamamlamanız önerilir [Razor sayfaları giriş](xref:razor-pages/index) bu öğreticiye başlamadan önce. Razor sayfaları, ASP.NET Core web uygulamaları için kullanıcı arabirimini derlemek için önerilen yoludur.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a>Razor web uygulaması oluşturma

Bir terminalde aşağıdaki komutları çalıştırın:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

Kullanım komutları önceki [.NET Core CLI](/dotnet/core/tools/dotnet) oluşturup bir Razor sayfaları projeyi çalıştırın. Bir tarayıcıda http://localhost:5000 uygulamayı görüntülemek için.

![Giriş ya da dizin sayfası](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Projeyi açın

Uygulamayı kapatmak için CTRL + C tuşlarına basın.

Visual Studio Code'dan (VS Code), seçin **Dosya > Klasör Aç**ve ardından *RazorPagesMovie* klasör.

- Seçin **Evet** için **uyar** ileti "derleme ve hata ayıklamak için gerekli varlıkları 'RazorPagesMovie' eksik. Bunları eklensin mi?"
- Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıklar vardır" iletisi.

### <a name="launch-the-app"></a>Uygulamayı başlatın

Gelen **hata ayıklama** menüsünde **hata ayıklama olmadan Başlat**. Alternatif olarak, menü seçeneğinin yanında görüntülenen klavye kısayoluna basabilirsiniz. Bu kısayol, işletim sisteminize bağlı olarak değişir.

Sonraki öğreticide, projeye bir model ekleyeceğiz. 

> [!div class="step-by-step"]
> [Sonraki: model ekleme](xref:tutorials/razor-pages-vsc/model)  
