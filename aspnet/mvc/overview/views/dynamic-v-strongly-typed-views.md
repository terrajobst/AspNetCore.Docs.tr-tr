---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: "Dinamik v. Görünümleri'kesin türü belirtilmiş | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 8a96d43e04a0a50d5176c10c26aa49918a0e56ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="35d44-103">Dinamik v.</span><span class="sxs-lookup"><span data-stu-id="35d44-103">Dynamic v.</span></span> <span data-ttu-id="35d44-104">Kesin türü belirtilmiş görünümleri</span><span class="sxs-lookup"><span data-stu-id="35d44-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="35d44-105">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="35d44-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="35d44-106">ASP.NET MVC 3'te bir görünüm için bir denetleyicisinden bilgi aktarmak için üç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="35d44-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="35d44-107">Kesin türü belirtilmiş model nesnesi.</span><span class="sxs-lookup"><span data-stu-id="35d44-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="35d44-108">Dinamik tür olarak (kullanarak @model dinamik)</span><span class="sxs-lookup"><span data-stu-id="35d44-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="35d44-109">Görünüm paketini kullanma</span><span class="sxs-lookup"><span data-stu-id="35d44-109">Using the ViewBag</span></span>

<span data-ttu-id="35d44-110">Karşılaştırmak ve dinamik ve kesin türü belirtilmiş görünümleri karşıtlık için basit bir MVC 3 üst Blog uygulaması yazılmış.</span><span class="sxs-lookup"><span data-stu-id="35d44-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="35d44-111">Denetleyici bloglar basit bir listesi ile başlar:</span><span class="sxs-lookup"><span data-stu-id="35d44-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="35d44-112">IndexNotStonglyTyped() yönteminde sağ tıklayın ve Razor görünüm ekleyin.</span><span class="sxs-lookup"><span data-stu-id="35d44-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="35d44-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="35d44-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="35d44-114">Emin olun **kesin türü belirtilmiş görünüm oluşturmak** kutusu işaretli değildir.</span><span class="sxs-lookup"><span data-stu-id="35d44-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="35d44-115">Sonuçta elde edilen görünümü çok içermiyor:</span><span class="sxs-lookup"><span data-stu-id="35d44-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="35d44-116">Biz dinamik ve kesin türü belirtilmiş görünümleri kullandığımızdan, IntelliSense bize yardımcı değil.</span><span class="sxs-lookup"><span data-stu-id="35d44-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="35d44-117">Tamamlanan kodu aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="35d44-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="35d44-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="35d44-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="35d44-119">Kesin türü belirtilmiş görünüm şimdi ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="35d44-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="35d44-120">Denetleyici için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="35d44-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="35d44-121">Tam olarak aynı dönüş View(topBlogs) olduğuna dikkat edin; olmayan-kesin türü belirtilmiş görünüm çağırın.</span><span class="sxs-lookup"><span data-stu-id="35d44-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="35d44-122">İçinde sağ tıklayın *StonglyTypedIndex()* seçip **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="35d44-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="35d44-123">Bu süre seçin **Blog** Model sınıfı ve seçin **listesi** İskele şablon olarak.</span><span class="sxs-lookup"><span data-stu-id="35d44-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="35d44-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="35d44-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="35d44-125">İçinde yeni şablonu görüntüleme IntelliSense desteği alın.</span><span class="sxs-lookup"><span data-stu-id="35d44-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="35d44-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="35d44-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="35d44-127">C# projesinde indirilen [burada](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="35d44-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
