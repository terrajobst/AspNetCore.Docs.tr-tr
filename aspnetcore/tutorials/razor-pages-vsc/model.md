---
title: Mac için Visual Studio ile ASP.NET Core Razor sayfalarının uygulama için model ekleme
author: rick-anderson
description: ASP.NET Core Mac için Visual Studio kullanarak bir Razor sayfalarının uygulama için bir model eklemeyi öğrenin
manager: wpickett
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: 4222b16e6a71913665bf03eee5973316b8218ad4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a>Mac için Visual Studio ile ASP.NET Core Razor sayfalarının uygulama için model ekleme

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a>Bir veri modeli ekleme

* Adlı bir klasör ekleme *modelleri*.
* Bir sınıfa eklemek *modelleri* adlı klasörü *Movie.cs*.
* Aşağıdaki kodu ekleyin *Models/Movie.cs* dosyası:

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

Herhangi bir hata yoksa doğrulamak için projeyi oluşturun.

### <a name="entity-framework-core-nuget-packages-for-migrations"></a>Geçişler için Entity Framework Core NuGet paketleri

EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet). Bu paketi yüklemek için ekleyin `DotNetCliToolReference` koleksiyonunda *.csproj* dosya. **Not:** düzenleyerek bu paketi yüklemek zorunda *.csproj* dosya; kullanamazsınız `install-package` komut veya GUI Paket Yöneticisi.

Düzen *RazorPagesMovie.csproj* dosyası:

* Seçin **dosya** > **açık dosya**ve ardından *RazorPagesMovie.csproj* dosya.
* Aracı başvurusunu ekleyin `Microsoft.EntityFrameworkCore.Tools.DotNet` ikinci  **\<ItemGroup >**:

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>İskele film modeli

* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).
* Şu komutu çalıştırın:

**Not: Windows aşağıdaki komutu çalıştırın. MacOS ve Linux için bkz: sonraki komutu**

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* MacOS ve Linux üzerinde aşağıdaki komutu çalıştırın:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

Hatayı alırsanız:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Visual Studio çıkın ve komutu yeniden çalıştırın.

[!INCLUDE [model 4](../../includes/RP/model4.md)]

Sonraki öğretici yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.

> [!div class="step-by-step"]
> [Önceki: Başlama](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [sonraki: iskele kurulmuş Razor sayfaları](xref:tutorials/razor-pages-vsc/page)
