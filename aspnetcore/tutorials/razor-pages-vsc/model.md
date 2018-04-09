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
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a><span data-ttu-id="369c7-103">Mac için Visual Studio ile ASP.NET Core Razor sayfalarının uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="369c7-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio for Mac</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="369c7-104">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="369c7-104">Add a data model</span></span>

* <span data-ttu-id="369c7-105">Adlı bir klasör ekleme *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="369c7-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="369c7-106">Bir sınıfa eklemek *modelleri* adlı klasörü *Movie.cs*.</span><span class="sxs-lookup"><span data-stu-id="369c7-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="369c7-107">Aşağıdaki kodu ekleyin *Models/Movie.cs* dosyası:</span><span class="sxs-lookup"><span data-stu-id="369c7-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="369c7-108">Herhangi bir hata yoksa doğrulamak için projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="369c7-108">Build the project to verify you don't have any errors.</span></span>

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="369c7-109">Geçişler için Entity Framework Core NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="369c7-109">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="369c7-110">EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="369c7-110">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="369c7-111">Bu paketi yüklemek için ekleyin `DotNetCliToolReference` koleksiyonunda *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="369c7-111">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="369c7-112">**Not:** düzenleyerek bu paketi yüklemek zorunda *.csproj* dosya; kullanamazsınız `install-package` komut veya GUI Paket Yöneticisi.</span><span class="sxs-lookup"><span data-stu-id="369c7-112">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="369c7-113">Düzen *RazorPagesMovie.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="369c7-113">Edit the *RazorPagesMovie.csproj* file:</span></span>

* <span data-ttu-id="369c7-114">Seçin **dosya** > **açık dosya**ve ardından *RazorPagesMovie.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="369c7-114">Select **File** > **Open File**, and then select the *RazorPagesMovie.csproj* file.</span></span>
* <span data-ttu-id="369c7-115">Aracı başvurusunu ekleyin `Microsoft.EntityFrameworkCore.Tools.DotNet` ikinci  **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="369c7-115">Add tool reference for `Microsoft.EntityFrameworkCore.Tools.DotNet` to the second **\<ItemGroup>**:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj)]

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="369c7-116">İskele film modeli</span><span class="sxs-lookup"><span data-stu-id="369c7-116">Scaffold the Movie model</span></span>

* <span data-ttu-id="369c7-117">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="369c7-117">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="369c7-118">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="369c7-118">Run the following command:</span></span>

<span data-ttu-id="369c7-119">**Not: Windows aşağıdaki komutu çalıştırın. MacOS ve Linux için bkz: sonraki komutu**</span><span class="sxs-lookup"><span data-stu-id="369c7-119">**Note: Run the following command on Windows. For MacOS and Linux, see the next command**</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="369c7-120">MacOS ve Linux üzerinde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="369c7-120">On MacOS and Linux, run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="369c7-121">Hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="369c7-121">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="369c7-122">Visual Studio çıkın ve komutu yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="369c7-122">Exit Visual Studio and run the command again.</span></span>

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="369c7-123">Sonraki öğretici yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="369c7-123">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="369c7-124">[Önceki: Başlama](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [sonraki: iskele kurulmuş Razor sayfaları](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="369c7-124">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
