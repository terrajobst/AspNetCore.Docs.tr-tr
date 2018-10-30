---
title: Visual Studio Code ile ASP.NET Core Razor sayfalar uygulama için model ekleme
author: rick-anderson
description: Visual Studio Code ile ASP.NET Core Razor sayfaları uygulamada bir model ekleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244729"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a>Visual Studio Code ile ASP.NET Core Razor sayfalar uygulama için model ekleme

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Bir veri modeli ekleme

* Adlı bir klasör ekleme *modelleri*.
* Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.
* Aşağıdaki kodu ekleyin *Models/Movie.cs* dosyası:

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a>SQLite için Entity Framework Core NuGet paketi

Komut satırından aşağıdaki .NET Core CLI komutunu çalıştırın:

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a>Veritabanı bağlamı Kaydet

Veritabanı bağlamı ile kaydetme [bağımlılık ekleme](xref:fundamentals/dependency-injection) kapsayıcısında *Startup.cs* dosya.

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

Aşağıdaki `using` deyimleri en üstündeki *Startup.cs*:

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Herhangi bir hata yoksa doğrulamak için projeyi derleyin.

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a>Film modeli iskelesini

* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).
* **Windows için**: aşağıdaki komutu çalıştırın:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* **MacOS ve Linux için**: aşağıdaki komutu çalıştırın:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.

> [!div class="step-by-step"]
> [Önceki: Başlama](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [sonraki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages-vsc/page)
