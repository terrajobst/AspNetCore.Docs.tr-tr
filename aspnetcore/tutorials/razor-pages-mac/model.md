---
title: Mac için Visual Studio ile ASP.NET Core Razor sayfalar uygulama için model ekleme
author: rick-anderson
description: Bir model bir Mac için Visual Studio kullanarak ASP.NET Core Razor sayfaları uygulamasına eklemeyi öğrenin
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 3ca6c9b9988b8335116b7248c6c4a89997d02b14
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38186764"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a><span data-ttu-id="6c385-103">Mac için Visual Studio ile ASP.NET Core Razor sayfalar uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="6c385-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio for Mac</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="6c385-104">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="6c385-104">Add a data model</span></span>

* <span data-ttu-id="6c385-105">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="6c385-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="6c385-106">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="6c385-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="6c385-107">Sağ *modelleri* klasöre tıklayın ve ardından **Ekle** > **yeni dosya**.</span><span class="sxs-lookup"><span data-stu-id="6c385-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="6c385-108">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="6c385-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="6c385-109">Seçin **genel** sol bölmesinde.</span><span class="sxs-lookup"><span data-stu-id="6c385-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="6c385-110">Seçin **boş sınıf** içinde merkezi kolaylaştırın.</span><span class="sxs-lookup"><span data-stu-id="6c385-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="6c385-111">Sınıf adı **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="6c385-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="6c385-112">Sağ tıklayın örneğin kırmızı dalgalı çizgi `MovieContext` satırında `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="6c385-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="6c385-113">Seçin **hızlı düzeltme > RazorPagesMovie.Models; kullanarak**.</span><span class="sxs-lookup"><span data-stu-id="6c385-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="6c385-114">Visual studio ekler using deyimi.</span><span class="sxs-lookup"><span data-stu-id="6c385-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="6c385-115">Herhangi bir hata yoksa doğrulamak için projeyi derleyin.</span><span class="sxs-lookup"><span data-stu-id="6c385-115">Build the project to verify you don't have any errors.</span></span>

![sayfası oluşturma](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="6c385-117">Geçişler için Entity Framework Core NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="6c385-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="6c385-118">EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="6c385-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="6c385-119">Tıklayarak [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) kullanmak için sürüm numarasını almak için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="6c385-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="6c385-120">Bu paketi yüklemek için eklemeniz `DotNetCliToolReference` koleksiyonda *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="6c385-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="6c385-121">**Not:** düzenleyerek bu paketi yüklemek sahip olduğunuz *.csproj* dosya; kullanamazsınız `install-package` komut veya Paket Yöneticisi GUI.</span><span class="sxs-lookup"><span data-stu-id="6c385-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="6c385-122">Düzenlenecek bir *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="6c385-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="6c385-123">Seçin **dosya** > **açık**ve ardından *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="6c385-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="6c385-124">Seçin **seçenekleri**.</span><span class="sxs-lookup"><span data-stu-id="6c385-124">Select **Options**.</span></span>
* <span data-ttu-id="6c385-125">Değişiklik **birlikte Aç** için **Kaynak Kod Düzenleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="6c385-125">Change **Open with** to **Source Code Editor**.</span></span>

![Csproj dosyasını düzenleyin](model/csproj.png)

<span data-ttu-id="6c385-127">Ekleme `Microsoft.EntityFrameworkCore.Tools.DotNet` aracı ikinci başvuru  **\<ItemGroup >**.:</span><span class="sxs-lookup"><span data-stu-id="6c385-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="6c385-128">Aşağıdaki kodda gösterilen sürüm numaraları makalenin yazıldığı sırada, doğru.</span><span class="sxs-lookup"><span data-stu-id="6c385-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="6c385-129">Sayfa/film dosyaları projeye Ekle</span><span class="sxs-lookup"><span data-stu-id="6c385-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="6c385-130">Visual Studio'da sağ *sayfaları* klasörü ve select **Ekle > mevcut klasörü Ekle**.</span><span class="sxs-lookup"><span data-stu-id="6c385-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="6c385-131">Seçin *filmler* klasör.</span><span class="sxs-lookup"><span data-stu-id="6c385-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="6c385-132">İçinde *projeye eklenecek dosyaları seçin* iletişim kutusunda **dahil tüm**.</span><span class="sxs-lookup"><span data-stu-id="6c385-132">In the *Choose files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="6c385-133">Sonraki öğreticiye yapı iskelesi tarafından oluşturulan dosyaları açıklar.</span><span class="sxs-lookup"><span data-stu-id="6c385-133">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6c385-134">[Önceki: Başlama](xref:tutorials/razor-pages-mac/razor-pages-start)
> [sonraki: Razor sayfaları için iskele kurulmuş](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="6c385-134">[Previous: Get Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
