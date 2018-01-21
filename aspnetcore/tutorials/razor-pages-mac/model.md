---
title: "Mac için Visual Studio Razor sayfalarının uygulamayla bir modeli ekleme"
author: rick-anderson
description: "Mac için Visual Studio kullanarak ASP.NET Core Razor sayfalarının uygulamada bir modeli ekleme"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 7b1b2d54e9c68b0a6f2b1355726d0d1cb484f69e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="f65fc-103">Mac için Visual Studio ile ASP.NET Core Razor sayfalarının uygulamada bir modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="f65fc-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="f65fc-104">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="f65fc-104">Add a data model</span></span>

* <span data-ttu-id="f65fc-105">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="f65fc-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="f65fc-106">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="f65fc-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="f65fc-107">Sağ *modelleri* klasörünü ve ardından **Ekle** > **yeni dosya**.</span><span class="sxs-lookup"><span data-stu-id="f65fc-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="f65fc-108">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="f65fc-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="f65fc-109">Seçin **genel** sol bölmede.</span><span class="sxs-lookup"><span data-stu-id="f65fc-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="f65fc-110">Seçin **boş sınıfı** center sorun içinde.</span><span class="sxs-lookup"><span data-stu-id="f65fc-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="f65fc-111">Sınıf adını **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="f65fc-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="f65fc-112">Sağ tıklayın örneğin kırmızı kırık çizgi `MovieContext` satırına `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="f65fc-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="f65fc-113">Seçin **hızlı düzeltme > RazorPagesMovie.Models; kullanarak**.</span><span class="sxs-lookup"><span data-stu-id="f65fc-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="f65fc-114">Visual studio ekler using deyimi.</span><span class="sxs-lookup"><span data-stu-id="f65fc-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="f65fc-115">Herhangi bir hata yoksa doğrulamak için projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="f65fc-115">Build the project to verify you don't have any errors.</span></span>

![Sayfa oluşturma](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="f65fc-117">Geçişler için Entity Framework Core NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="f65fc-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="f65fc-118">EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="f65fc-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="f65fc-119">Bu paketi yüklemek için ekleyin `DotNetCliToolReference` koleksiyonunda *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="f65fc-119">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="f65fc-120">**Not:** düzenleyerek bu paketi yüklemek zorunda *.csproj* dosya; kullanamazsınız `install-package` komut veya GUI Paket Yöneticisi.</span><span class="sxs-lookup"><span data-stu-id="f65fc-120">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="f65fc-121">Düzenlemek için bir *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="f65fc-121">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="f65fc-122">Seçin **dosya** > **açık**ve ardından *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="f65fc-122">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="f65fc-123">Seçin **seçenekleri**.</span><span class="sxs-lookup"><span data-stu-id="f65fc-123">Select **Options**.</span></span>
* <span data-ttu-id="f65fc-124">Değişiklik **birlikte Aç** için **kaynak kod düzenleyicisinde**.</span><span class="sxs-lookup"><span data-stu-id="f65fc-124">Change **Open with** to **Source Code Editor**.</span></span>

![Csproj dosyasını düzenleyin](model/csproj.png)

<span data-ttu-id="f65fc-126">Ekleme `Microsoft.EntityFrameworkCore.Tools.DotNet` aracı ikinci referansı  **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="f65fc-126">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="f65fc-127">Sayfa/filmler dosyaları projeye ekleyin</span><span class="sxs-lookup"><span data-stu-id="f65fc-127">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="f65fc-128">Visual Studio'da sağ *sayfaları* klasörü ve seçin **Ekle > varolan klasörü Ekle**.</span><span class="sxs-lookup"><span data-stu-id="f65fc-128">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="f65fc-129">Seçin *filmler* klasör.</span><span class="sxs-lookup"><span data-stu-id="f65fc-129">Select the *Movies* folder.</span></span>
* <span data-ttu-id="f65fc-130">İçinde *projeye dahil edilecek Chosse dosyalar* iletişim kutusunda **dahil tüm**.</span><span class="sxs-lookup"><span data-stu-id="f65fc-130">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="f65fc-131">Sonraki öğretici yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="f65fc-131">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f65fc-132">[Önceki: Başlarken](xref:tutorials/razor-pages-mac/razor-pages-start)
[sonraki: iskele kurulmuş Razor sayfaları](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="f65fc-132">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
