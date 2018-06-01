---
title: ASP.NET Core Razor sayfalarında, Visual Studio Code ile çalışmaya başlama
author: rick-anderson
description: Visual Studio Code ile bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temellerini öğrenin.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/razor-pages-start
ms.openlocfilehash: 3dda0f20dfbb7066dfeb3360361435ef65caaca4
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34688393"
---
# <a name="get-started-with-aspnet-core-razor-pages-in-visual-studio-code"></a>ASP.NET Core Razor sayfalarında, Visual Studio Code ile çalışmaya başlama

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu öğretici, bir ASP.NET Core Razor sayfalarının web uygulaması oluşturmanın temel öğretir. Tamamlamanız önerilir [Razor sayfalarının giriş](xref:mvc/razor-pages/index) bu öğreticiye başlamadan önce. Razor sayfalarının ASP.NET Core web uygulamaları için kullanıcı Arabirimi oluşturmak için önerilen yöntem olduğu.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [](~/includes/net-core-prereqs-vscode.md)]

## <a name="create-a-razor-web-app"></a>Bir Razor web uygulaması oluşturma

Terminal durumundan, aşağıdaki komutları çalıştırın:

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

Yukarıdaki komutları kullanım [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) oluşturun ve Razor sayfalarının proje çalıştırın. Bir tarayıcıda http://localhost:5000 uygulamayı görüntülemek için.

![Giriş veya dizin sayfası](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a>Projeyi açın

Uygulamayı kapatın CTRL + C tuşlarına basın.

Visual Studio kodundan (VS Code) seçin **Dosya > Klasör Aç**ve ardından *RazorPagesMovie* klasör.

- Seçin **Evet** için **uyar** "derleme ve hata ayıklamak için gerekli varlıklar 'RazorPagesMovie' eksik. ileti Bunları eklensin mi?"
- Seçin **geri** için **bilgisi** "Çözümlenmemiş bağımlılıkları bulunur" iletisi.

### <a name="launch-the-app"></a>Uygulamayı başlatın

Hata ayıklama olmadan uygulamayı başlatmak için CTRL + F5 tuşuna basın. Alternatif olarak, gelen **hata ayıklama** menüsünde, select **hata ayıklama olmadan Başlat**.

Sonraki öğreticide biz projenize bir model ekleyin. 

> [!div class="step-by-step"]
> [Sonraki: bir modeli ekleme](xref:tutorials/razor-pages-vsc/model)  
