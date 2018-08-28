---
title: Visual Studio Code ile ASP.NET Core Razor sayfalar uygulama için model ekleme
author: rick-anderson
description: Visual Studio Code ile ASP.NET Core Razor sayfaları uygulamada bir model ekleme hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: b891b921baf1fe6d167c7bfb8b4c5278ce9fe9f5
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055870"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="b2b7e-103">Visual Studio Code ile ASP.NET Core Razor sayfalar uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="b2b7e-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="b2b7e-104">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="b2b7e-104">Add a data model</span></span>

* <span data-ttu-id="b2b7e-105">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="b2b7e-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="b2b7e-106">Bir sınıfa eklemek *modelleri* adlı klasöre *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="b2b7e-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="b2b7e-107">Aşağıdaki kodu ekleyin *Models/Movie.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b2b7e-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a><span data-ttu-id="b2b7e-108">SQLite için Entity Framework Core NuGet paketi</span><span class="sxs-lookup"><span data-stu-id="b2b7e-108">Entity Framework Core NuGet package for SQLite</span></span>

<span data-ttu-id="b2b7e-109">Komut satırından aşağıdaki .NET Core CLI komutunu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2b7e-109">From the command line, run the following .NET Core CLI command:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-4)]

<span data-ttu-id="b2b7e-110">Aşağıdaki `using` deyimleri en üstündeki *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="b2b7e-110">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="b2b7e-111">Herhangi bir hata yoksa doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="b2b7e-111">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="b2b7e-112">Geçişler için Entity Framework Core NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="b2b7e-112">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="b2b7e-113">EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="b2b7e-113">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="b2b7e-114">Bu paketi yüklemek için eklemeniz `DotNetCliToolReference` koleksiyonda *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="b2b7e-114">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="b2b7e-115">**Not:** düzenleyerek bu paketi yüklemek sahip olduğunuz *.csproj* dosya; kullanamazsınız `install-package` komut veya Paket Yöneticisi GUI.</span><span class="sxs-lookup"><span data-stu-id="b2b7e-115">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="b2b7e-116">Düzen *RazorPagesMovie.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="b2b7e-116">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="b2b7e-117">Seçin **dosya** > **açık dosya**ve ardından *RazorPagesMovie.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="b2b7e-117">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="b2b7e-118">Aracı başvurusunu eklemek `Microsoft.EntityFrameworkCore.Tools.DotNet` ikinci  **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="b2b7e-118">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="b2b7e-119">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="b2b7e-119">Scaffold the Movie model</span></span>

* <span data-ttu-id="b2b7e-120">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="b2b7e-120">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="b2b7e-121">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2b7e-121">Run the following command:</span></span>

<span data-ttu-id="b2b7e-122">**Not: Windows üzerinde aşağıdaki komutu çalıştırın. Sonraki komut, MacOS ve Linux için bkz.**</span><span class="sxs-lookup"><span data-stu-id="b2b7e-122">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="b2b7e-123">MacOS ve Linux üzerinde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2b7e-123">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="b2b7e-124">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="b2b7e-124">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="b2b7e-125">Visual Studio çıkın ve komutu yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b2b7e-125">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="b2b7e-126">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="b2b7e-126">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b2b7e-127">[Önceki: Başlama](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [sonraki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="b2b7e-127">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
