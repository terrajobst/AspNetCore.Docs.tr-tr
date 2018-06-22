---
title: Visual Studio Code ile ASP.NET Core Razor sayfalarının uygulama için model ekleme
author: rick-anderson
description: Visual Studio Code kullanarak ASP.NET Core bir Razor sayfalarının uygulama için bir model eklemeyi öğrenin.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: bf7566d1be0d7b520ab329ed5490e11f11240886
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276081"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="c30ba-103">Visual Studio Code ile ASP.NET Core Razor sayfalarının uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="c30ba-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="c30ba-104">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="c30ba-104">Add a data model</span></span>

* <span data-ttu-id="c30ba-105">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="c30ba-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="c30ba-106">Bir sınıfa eklemek *modelleri* adlı klasörü *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="c30ba-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="c30ba-107">Aşağıdaki kodu ekleyin *Models/Movie.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="c30ba-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="c30ba-108">Herhangi bir hata yoksa doğrulamak için projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="c30ba-108">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="c30ba-109">Geçişler için Entity Framework Core NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="c30ba-109">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="c30ba-110">EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="c30ba-110">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="c30ba-111">Bu paketi yüklemek için ekleyin `DotNetCliToolReference` koleksiyonunda *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="c30ba-111">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="c30ba-112">**Not:** düzenleyerek bu paketi yüklemek zorunda *.csproj* dosya; kullanamazsınız `install-package` komut veya GUI Paket Yöneticisi.</span><span class="sxs-lookup"><span data-stu-id="c30ba-112">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="c30ba-113">Düzen *RazorPagesMovie.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="c30ba-113">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="c30ba-114">Seçin **dosya** > **açık dosya**ve ardından *RazorPagesMovie.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="c30ba-114">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="c30ba-115">Aracı başvurusunu ekleyin `Microsoft.EntityFrameworkCore.Tools.DotNet` ikinci  **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="c30ba-115">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="c30ba-116">İskele film modeli</span><span class="sxs-lookup"><span data-stu-id="c30ba-116">Scaffold the Movie model</span></span>

* <span data-ttu-id="c30ba-117">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="c30ba-117">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="c30ba-118">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c30ba-118">Run the following command:</span></span>

<span data-ttu-id="c30ba-119">**Not: Windows aşağıdaki komutu çalıştırın. MacOS ve Linux için bkz: sonraki komutu**</span><span class="sxs-lookup"><span data-stu-id="c30ba-119">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="c30ba-120">MacOS ve Linux üzerinde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c30ba-120">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="c30ba-121">Hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="c30ba-121">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="c30ba-122">Visual Studio çıkın ve komutu yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c30ba-122">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="c30ba-123">Sonraki öğretici yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="c30ba-123">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c30ba-124">[Önceki: Başlama](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [sonraki: iskele kurulmuş Razor sayfaları](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="c30ba-124">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
