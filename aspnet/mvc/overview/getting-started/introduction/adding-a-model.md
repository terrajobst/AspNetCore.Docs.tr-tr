---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Model ekleme | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 3b9d84ae757eac6cc2a1ae8f7857244adff50d7c
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578282"
---
<a name="adding-a-model"></a><span data-ttu-id="b634e-102">Model ekleme</span><span class="sxs-lookup"><span data-stu-id="b634e-102">Adding a Model</span></span>
====================
<span data-ttu-id="b634e-103">Tarafından [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="b634e-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

<span data-ttu-id="b634e-104">Bu bölümde, bir veritabanında filmler yönetmek için bazı sınıflar ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="b634e-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="b634e-105">Bu sınıflar olacaktır &quot;modeli&quot; ASP.NET MVC uygulaması bir parçası.</span><span class="sxs-lookup"><span data-stu-id="b634e-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="b634e-106">Bir .NET Framework Veri erişim teknolojisi olarak bilinen kullanacağınız [Entity Framework](https://docs.microsoft.com/ef/) tanımlayın ve bu model sınıfları ile çalışmak için.</span><span class="sxs-lookup"><span data-stu-id="b634e-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="b634e-107">Geliştirme paradigma adlı Entity Framework (genellikle EF adlandırılır) destekler *Code First*.</span><span class="sxs-lookup"><span data-stu-id="b634e-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="b634e-108">Kod ilk basit sınıfları yazarak model nesneleri oluşturmanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="b634e-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="b634e-109">(Bu olarak da bilinen POCO sınıfları arasındadır &quot;düz eski CLR nesnesi.&quot;) Ardından, bir çok temiz ve hızlı geliştirme iş akışını sağlayan çalışma sırasında sınıflardan oluşturduğunuz veritabanına sahip olabilir.</span><span class="sxs-lookup"><span data-stu-id="b634e-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="b634e-110">İlk veritabanı oluşturmak için gerekli olduğunda, yine de MVC ve EF uygulama geliştirme hakkında bilgi edinmek için bu öğreticiyi takip edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="b634e-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="b634e-111">Ardından, Tom Fizmakens izleyebilirsiniz [ASP.NET iskeleti oluşturma](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) veritabanı ilk yaklaşım kapsayan bir öğretici.</span><span class="sxs-lookup"><span data-stu-id="b634e-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="b634e-112">Model sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="b634e-112">Adding Model Classes</span></span>

<span data-ttu-id="b634e-113">İçinde **Çözüm Gezgini**, sağ tıklayın *modelleri* klasörüne **Ekle**ve ardından **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="b634e-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="b634e-114">Girin *sınıfı* adı &quot;film&quot;.</span><span class="sxs-lookup"><span data-stu-id="b634e-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="b634e-115">Aşağıdaki beş özelliği Ekle `Movie` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="b634e-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="b634e-116">Kullanacağız `Movie` filmler veritabanındaki temsil eden sınıf.</span><span class="sxs-lookup"><span data-stu-id="b634e-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="b634e-117">Her bir örneği bir `Movie` nesne karşılık gelen bir veritabanı tablosu ve her bir özellik içinde bir satıra `Movie` sınıfı tablosunda bir sütun eşleme.</span><span class="sxs-lookup"><span data-stu-id="b634e-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="b634e-118">Not: System.Data.Entity ve ilgili sınıf kullanmak için yüklemeniz gerekir [Entity Framework NuGet paketi](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="b634e-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="b634e-119">Daha fazla yönerge için bağlantıyı izleyin.</span><span class="sxs-lookup"><span data-stu-id="b634e-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="b634e-120">Aynı dosyada, aşağıdaki ekleyin `MovieDBContext` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="b634e-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="b634e-121">`MovieDBContext` Sınıfı temsil eder, alma, depolama ve güncelleştirme işleme Entity Framework film veritabanı bağlamı `Movie` sınıfı bir veritabanında örnekleri.</span><span class="sxs-lookup"><span data-stu-id="b634e-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="b634e-122">`MovieDBContext` Türetildiği `DbContext` temel Entity Framework tarafından sağlanan sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b634e-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="b634e-123">Başvuru yapabilmek için `DbContext` ve `DbSet`, aşağıdaki eklemeniz `using` deyimini dosyanın üst:</span><span class="sxs-lookup"><span data-stu-id="b634e-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="b634e-124">Bunu kullanarak el ile ekleyerek yapabilirsiniz deyimi veya kırmızı dalgalı çizgiler gelin, tıklayın `Show potential fixes` tıklayın `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="b634e-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="b634e-125">Not: Birkaç kullanılmayan `using` deyimleri kaldırıldı.</span><span class="sxs-lookup"><span data-stu-id="b634e-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="b634e-126">Visual Studio kullanılmayan bağımlılıkları gri renkte gösterilir.</span><span class="sxs-lookup"><span data-stu-id="b634e-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="b634e-127">Gri bağımlılıkları gelerek kullanılmayan bağımlılıkları kaldırın, tıklayın `Show potential fixes` tıklatıp **kullanılmayan kullanımları kaldırma.**</span><span class="sxs-lookup"><span data-stu-id="b634e-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="b634e-128">Son olarak bir modeli (M mvc'de) ekledik.</span><span class="sxs-lookup"><span data-stu-id="b634e-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="b634e-129">Sonraki bölümde veritabanı bağlantı dizesi ile çalışırsınız.</span><span class="sxs-lookup"><span data-stu-id="b634e-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b634e-130">[Önceki](adding-a-view.md)
> [İleri](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="b634e-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
