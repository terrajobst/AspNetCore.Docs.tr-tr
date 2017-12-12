---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Model ekleme | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 13aab58e86829a8d4accd1d304420dcb34ffa472
ms.sourcegitcommit: ec9371e2fbfcb8d62e7e7cae69e7752f3f205385
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
<a name="adding-a-model"></a><span data-ttu-id="3d250-102">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="3d250-102">Adding a Model</span></span>
====================
<span data-ttu-id="3d250-103">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="3d250-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

<span data-ttu-id="3d250-104">Bu bölümde bir veritabanında filmler yönetmek için bazı sınıfları ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="3d250-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="3d250-105">Bu sınıfların olacaktır &quot;modeli&quot; ASP.NET MVC uygulaması parçası.</span><span class="sxs-lookup"><span data-stu-id="3d250-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="3d250-106">Olarak bilinen bir .NET Framework veri erişimi teknoloji kullanacağınız [Entity Framework](https://docs.microsoft.com/ef/) tanımlamak ve bu modeli sınıfları ile çalışmak için.</span><span class="sxs-lookup"><span data-stu-id="3d250-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="3d250-107">Bir geliştirme standardı adlı Entity Framework (genellikle EF adlandırılır) destekler *Code First*.</span><span class="sxs-lookup"><span data-stu-id="3d250-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="3d250-108">Kod ilk model nesneleri basit sınıfları yazarak oluşturmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="3d250-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="3d250-109">(Bu olarak da bilinen POCO sınıfları arasındadır &quot;düz eski CLR nesnelerini.&quot;) Ardından, çok temiz ve hızlı geliştirme iş akışı sağlayan anında, sınıflardan oluşturulan veritabanı olabilir.</span><span class="sxs-lookup"><span data-stu-id="3d250-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="3d250-110">Veritabanını oluşturmak için önce gerekli olduğunda hala MVC ve EF uygulama geliştirme hakkında bilgi edinmek için bu öğreticiyi izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3d250-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="3d250-111">Ardından zel Fizmakens izleyin [ASP.NET yapı İskelesi](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) veritabanı ilk yaklaşımı kapsayan öğretici.</span><span class="sxs-lookup"><span data-stu-id="3d250-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="3d250-112">Model sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="3d250-112">Adding Model Classes</span></span>

<span data-ttu-id="3d250-113">İçinde **Çözüm Gezgini**, sağ tıklatın *modelleri* klasöründe seçin **Ekle**ve ardından **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="3d250-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="3d250-114">Girin *sınıfı* adı &quot;film&quot;.</span><span class="sxs-lookup"><span data-stu-id="3d250-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="3d250-115">Aşağıdaki beş özellikleri ekleyin `Movie` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="3d250-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="3d250-116">Kullanacağız `Movie` bir veritabanında filmler temsil eden sınıf.</span><span class="sxs-lookup"><span data-stu-id="3d250-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="3d250-117">Her bir örneğini bir `Movie` nesne bir veritabanı tablosu ve her bir özelliğinin içinde bir satır karşılık `Movie` sınıfı tablodaki bir sütun eşleme.</span><span class="sxs-lookup"><span data-stu-id="3d250-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="3d250-118">Not: System.Data.Entity ve ilgili sınıfını kullanmak için yüklemeniz gerekir [Entity Framework NuGet paketi](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="3d250-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="3d250-119">Daha fazla yönerge için bağlantıyı izleyin.</span><span class="sxs-lookup"><span data-stu-id="3d250-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="3d250-120">Aynı dosyada aşağıdakileri ekleyin `MovieDBContext` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="3d250-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="3d250-121">`MovieDBContext` Sınıfı temsil eder getirme, depolama ve güncelleştirme işleme Entity Framework film veritabanı bağlamı `Movie` sınıfı bir veritabanı örneği.</span><span class="sxs-lookup"><span data-stu-id="3d250-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="3d250-122">`MovieDBContext` Türetilen `DbContext` temel Entity Framework tarafından sağlanan sınıfı.</span><span class="sxs-lookup"><span data-stu-id="3d250-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="3d250-123">Başvuru için `DbContext` ve `DbSet`, aşağıdaki eklemeniz `using` deyimini dosyanın üst:</span><span class="sxs-lookup"><span data-stu-id="3d250-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="3d250-124">Bunu kullanarak el ile ekleyerek yapabilirsiniz deyimi veya kırmızı dalgalı çizgiler getirin, tıklatın `Show potential fixes` ve'ı tıklatın`using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="3d250-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="3d250-125">Not: Birkaç kullanılmayan `using` deyimleri kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="3d250-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="3d250-126">Visual Studio kullanılmayan bağımlılıklarını gri renkte gösterilir.</span><span class="sxs-lookup"><span data-stu-id="3d250-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="3d250-127">Gri bağımlılıkları gelerek unnused bağımlılıkları kaldırın, tıklatın `Show potential fixes` tıklatıp **kullanılmayan kullanımları kaldırma.**</span><span class="sxs-lookup"><span data-stu-id="3d250-127">You can remove unnused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="3d250-128">Son olarak bir modeli (MVC M) ekledik.</span><span class="sxs-lookup"><span data-stu-id="3d250-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="3d250-129">Sonraki bölümde veritabanı bağlantı dizesi ile çalışması.</span><span class="sxs-lookup"><span data-stu-id="3d250-129">In the next section you'll work with the database connection string.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="3d250-130">[Önceki](adding-a-view.md)
[sonraki](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="3d250-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
