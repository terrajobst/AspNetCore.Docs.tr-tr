---
title: Mac için Visual Studio ile ASP.NET Core Razor sayfalarının uygulama için model ekleme
author: rick-anderson
description: ASP.NET Core Mac için Visual Studio kullanarak bir Razor sayfalarının uygulama için bir model eklemeyi öğrenin
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 3ca6c9b9988b8335116b7248c6c4a89997d02b14
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273280"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a><span data-ttu-id="8850a-103">Mac için Visual Studio ile ASP.NET Core Razor sayfalarının uygulama için model ekleme</span><span class="sxs-lookup"><span data-stu-id="8850a-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio for Mac</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="8850a-104">Bir veri modeli ekleme</span><span class="sxs-lookup"><span data-stu-id="8850a-104">Add a data model</span></span>

* <span data-ttu-id="8850a-105">Çözüm Gezgini'nde sağ **RazorPagesMovie** proje ve ardından **Ekle** > **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="8850a-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="8850a-106">Klasör adı *modelleri*.</span><span class="sxs-lookup"><span data-stu-id="8850a-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="8850a-107">Sağ *modelleri* klasörünü ve ardından **Ekle** > **yeni dosya**.</span><span class="sxs-lookup"><span data-stu-id="8850a-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="8850a-108">İçinde **yeni dosya** iletişim:</span><span class="sxs-lookup"><span data-stu-id="8850a-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="8850a-109">Seçin **genel** sol bölmede.</span><span class="sxs-lookup"><span data-stu-id="8850a-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="8850a-110">Seçin **boş sınıfı** center sorun içinde.</span><span class="sxs-lookup"><span data-stu-id="8850a-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="8850a-111">Sınıf adını **film** seçip **yeni**.</span><span class="sxs-lookup"><span data-stu-id="8850a-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="8850a-112">Sağ tıklayın örneğin kırmızı kırık çizgi `MovieContext` satırına `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="8850a-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="8850a-113">Seçin **hızlı düzeltme > RazorPagesMovie.Models; kullanarak**.</span><span class="sxs-lookup"><span data-stu-id="8850a-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="8850a-114">Visual studio ekler using deyimi.</span><span class="sxs-lookup"><span data-stu-id="8850a-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="8850a-115">Herhangi bir hata yoksa doğrulamak için projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="8850a-115">Build the project to verify you don't have any errors.</span></span>

![Sayfa oluşturma](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="8850a-117">Geçişler için Entity Framework Core NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="8850a-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="8850a-118">EF Araçları komut satırı arabirimi (CLI) için sağlanan [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="8850a-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="8850a-119">Tıklayın [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) kullanmak için sürüm numarasını almak için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="8850a-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="8850a-120">Bu paketi yüklemek için ekleyin `DotNetCliToolReference` koleksiyonunda *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="8850a-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="8850a-121">**Not:** düzenleyerek bu paketi yüklemek zorunda *.csproj* dosya; kullanamazsınız `install-package` komut veya GUI Paket Yöneticisi.</span><span class="sxs-lookup"><span data-stu-id="8850a-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="8850a-122">Düzenlemek için bir *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="8850a-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="8850a-123">Seçin **dosya** > **açık**ve ardından *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="8850a-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="8850a-124">Seçin **seçenekleri**.</span><span class="sxs-lookup"><span data-stu-id="8850a-124">Select **Options**.</span></span>
* <span data-ttu-id="8850a-125">Değişiklik **birlikte Aç** için **kaynak kod düzenleyicisinde**.</span><span class="sxs-lookup"><span data-stu-id="8850a-125">Change **Open with** to **Source Code Editor**.</span></span>

![Csproj dosyasını düzenleyin](model/csproj.png)

<span data-ttu-id="8850a-127">Ekleme `Microsoft.EntityFrameworkCore.Tools.DotNet` aracı ikinci referansı  **\<ItemGroup >**.:</span><span class="sxs-lookup"><span data-stu-id="8850a-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="8850a-128">Aşağıdaki kodda gösterildiği sürüm numaralarını yazıldığı sırada doğru.</span><span class="sxs-lookup"><span data-stu-id="8850a-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="8850a-129">Sayfa/filmler dosyaları projeye ekleyin</span><span class="sxs-lookup"><span data-stu-id="8850a-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="8850a-130">Visual Studio'da sağ *sayfaları* klasörü ve seçin **Ekle > varolan klasörü Ekle**.</span><span class="sxs-lookup"><span data-stu-id="8850a-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="8850a-131">Seçin *filmler* klasör.</span><span class="sxs-lookup"><span data-stu-id="8850a-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="8850a-132">İçinde *projede eklenecek dosyaları seçin* iletişim kutusunda **dahil tüm**.</span><span class="sxs-lookup"><span data-stu-id="8850a-132">In the *Choose files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="8850a-133">Sonraki öğretici yapı iskelesi tarafından oluşturulan dosyalar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="8850a-133">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8850a-134">[Önceki: Başlama](xref:tutorials/razor-pages-mac/razor-pages-start)
> [sonraki: iskele kurulmuş Razor sayfaları](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="8850a-134">[Previous: Get Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
