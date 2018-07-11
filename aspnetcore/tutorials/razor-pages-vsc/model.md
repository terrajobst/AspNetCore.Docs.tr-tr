---
title: Visual Studio Code ile ASP.NET Core Razor sayfalar uygulama için model ekleme
author: rick-anderson
description: Visual Studio Code ile ASP.NET Core Razor sayfaları uygulamada bir model ekleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 3552b541c43375aef43838800855ec63e7fed372
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38152977"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a>Visual Studio Code ile ASP.NET Core Razor sayfalar uygulama için model ekleme

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Bir veri modeli ekleme

* Adlı bir klasör ekleme *modelleri*.
* Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.
* Aşağıdaki kodu ekleyin *Models/Movie.cs* dosyası:

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-4)]

Aşağıdaki `using` deyimleri en üstündeki *Startup.cs*:

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

Herhangi bir hata yoksa doğrulamak için projeyi derleyin.

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Geçişler için Entity Framework Core NuGet paketleri

EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Bu paketi yüklemek için eklemeniz `DotNetCliToolReference` koleksiyonda *.csproj* dosya. **Not:** düzenleyerek bu paketi yüklemek sahip olduğunuz *.csproj* dosya; kullanamazsınız `install-package` komut veya Paket Yöneticisi GUI.

Düzen *RazorPagesMovie.csproj* dosyası:

* Seçin **dosya** > **açık dosya**ve ardından *RazorPagesMovie.csproj* dosya.
* Aracı başvurusunu eklemek `Microsoft.EntityFrameworkCore.Tools.DotNet` ikinci  **\<ItemGroup >**:

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>Film modeli iskelesini

* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).
* Şu komutu çalıştırın:

**Not: Windows üzerinde aşağıdaki komutu çalıştırın. Sonraki komut, MacOS ve Linux için bkz.**

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* MacOS ve Linux üzerinde aşağıdaki komutu çalıştırın:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Hatası alırsanız:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Visual Studio çıkın ve komutu yeniden çalıştırın.

[!INCLUDE [model 4](../../includes/RP/model4.md)]

Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.

> [!div class="step-by-step"]
> [Önceki: Başlama](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [sonraki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages-vsc/page)
