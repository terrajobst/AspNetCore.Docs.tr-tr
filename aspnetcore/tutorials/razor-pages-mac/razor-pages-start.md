---
title: ASP.NET Core Razor sayfaları Visual Studio ile macOS üzerinde Mac için kullanmaya başlama
author: rick-anderson
description: Mac için Visual Studio kullanarak ASP.NET Core Razor sayfaları kullanmaya başlama öğrenin
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 8a4cc6c52684558ce9176f98205e439096e88cbf
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50089657"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a>ASP.NET Core Razor sayfaları Visual Studio ile macOS üzerinde Mac için kullanmaya başlama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğreticide bir ASP.NET Core Razor sayfaları web uygulaması oluşturmaya ilişkin temel bilgileri size öğretir. Gözden geçirmenizi öneririz [Razor sayfaları giriş](xref:razor-pages/index) bu öğreticiye başlamadan önce. Razor sayfaları, ASP.NET Core web uygulamaları için kullanıcı arabirimini derlemek için önerilen yoludur.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

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

Visual Studio'dan seçin **Dosya > Aç**ve ardından *RazorPagesMovie.csproj* dosya.

### <a name="launch-the-app"></a>Uygulamayı başlatın

Visual Studio'da **çalıştırın > hata ayıklama olmadan Başlat** uygulamayı başlatın. Visual Studio başlatır [Kestrel](xref:fundamentals/servers/kestrel), bir tarayıcı başlatır ve gider `http://localhost:5000`.

Sonraki öğreticide, projeye bir model ekleyeceğiz.

> [!div class="step-by-step"]
> [Sonraki: model ekleme](xref:tutorials/razor-pages-mac/model)
