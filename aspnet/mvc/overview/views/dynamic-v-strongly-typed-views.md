---
uid: mvc/overview/views/dynamic-v-strongly-typed-views
title: Dinamik ve Kesin tür belirtilmiş görünümleri | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2011
ms.topic: article
ms.assetid: 0cbd88da-0da6-4605-b222-2835c6478304
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/views/dynamic-v-strongly-typed-views
msc.type: authoredcontent
ms.openlocfilehash: 7622ca8248374da27f4190075df5a6bfc32bb2e6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389093"
---
<a name="dynamic-v-strongly-typed-views"></a><span data-ttu-id="c02d5-103">Dinamik ve</span><span class="sxs-lookup"><span data-stu-id="c02d5-103">Dynamic v.</span></span> <span data-ttu-id="c02d5-104">Kesin türü belirtilmiş görünümler</span><span class="sxs-lookup"><span data-stu-id="c02d5-104">Strongly Typed Views</span></span>
====================
<span data-ttu-id="c02d5-105">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="c02d5-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

<span data-ttu-id="c02d5-106">ASP.NET MVC 3'te bir görünüm için bir denetleyiciden bilgi geçirmek için üç yolu vardır:</span><span class="sxs-lookup"><span data-stu-id="c02d5-106">There are three ways to pass information from a controller to a view in ASP.NET MVC 3:</span></span>

1. <span data-ttu-id="c02d5-107">Kesin olarak belirlenmiş model nesnesi.</span><span class="sxs-lookup"><span data-stu-id="c02d5-107">As a strongly typed model object.</span></span>
2. <span data-ttu-id="c02d5-108">Dinamik tür olarak (kullanarak @model dinamik)</span><span class="sxs-lookup"><span data-stu-id="c02d5-108">As a dynamic type (using @model dynamic)</span></span>
3. <span data-ttu-id="c02d5-109">Görünüm paketini kullanma</span><span class="sxs-lookup"><span data-stu-id="c02d5-109">Using the ViewBag</span></span>

<span data-ttu-id="c02d5-110">Ben karşılaştırın ve dinamik ve kesin türü belirtilmiş görünümleri için basit bir MVC 3 üst Blog uygulaması yazmamış.</span><span class="sxs-lookup"><span data-stu-id="c02d5-110">I've written a simple MVC 3 Top Blog application to compare and contrast dynamic and strongly typed views.</span></span> <span data-ttu-id="c02d5-111">Denetleyici basit bir listesini blogları ile başlar:</span><span class="sxs-lookup"><span data-stu-id="c02d5-111">The controller starts out with a simple list of blogs:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample1.cs)]

<span data-ttu-id="c02d5-112">IndexNotStonglyTyped() yönteminde sağ tıklayın ve bir Razor görünüm ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c02d5-112">Right click in the IndexNotStonglyTyped() method and add a Razor view.</span></span>

<span data-ttu-id="c02d5-113">[![8475.NotStronglyTypedView [1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c02d5-113">[![8475.NotStronglyTypedView[1]](dynamic-v-strongly-typed-views/_static/image2.png)](dynamic-v-strongly-typed-views/_static/image1.png)</span></span>

<span data-ttu-id="c02d5-114">Emin **kesin türü belirtilmiş görünüm oluşturmak** kutunun işaretli değil.</span><span class="sxs-lookup"><span data-stu-id="c02d5-114">Make sure the **Create a strongly-typed view** box is not checked.</span></span> <span data-ttu-id="c02d5-115">Ekranda çok içermiyor:</span><span class="sxs-lookup"><span data-stu-id="c02d5-115">The resulting view doesn't contain much:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample2.cshtml)]

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample3.cshtml)]

<span data-ttu-id="c02d5-116">Dinamik ve bir kesin türü belirtilmiş görünüm kullandığımızdan, IntelliSense bize yardımcı olmaz.</span><span class="sxs-lookup"><span data-stu-id="c02d5-116">Because we're using a dynamic and not a strongly typed view, intellisense doesn't help us.</span></span> <span data-ttu-id="c02d5-117">Tamamlanan kodu aşağıda gösterilmiştir:</span><span class="sxs-lookup"><span data-stu-id="c02d5-117">The completed code is shown below:</span></span>

[!code-cshtml[Main](dynamic-v-strongly-typed-views/samples/sample4.cshtml)]

<span data-ttu-id="c02d5-118">[![6646.NotStronglyTypedView_5F00_IE [1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="c02d5-118">[![6646.NotStronglyTypedView_5F00_IE[1]](dynamic-v-strongly-typed-views/_static/image4.png)](dynamic-v-strongly-typed-views/_static/image3.png)</span></span>

<span data-ttu-id="c02d5-119">Kesin türü belirtilmiş görünüm şimdi ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="c02d5-119">Now we'll add a strongly typed view.</span></span> <span data-ttu-id="c02d5-120">Denetleyici için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="c02d5-120">Add the following code to the controller:</span></span>

[!code-csharp[Main](dynamic-v-strongly-typed-views/samples/sample5.cs)]


<span data-ttu-id="c02d5-121">Tam olarak aynı dönüş View(topBlogs) olduğuna dikkat edin; kesin olmayan türü belirtilmiş görünüm çağırın.</span><span class="sxs-lookup"><span data-stu-id="c02d5-121">Notice it's exactly the same return View(topBlogs); call as the non-strongly typed view.</span></span> <span data-ttu-id="c02d5-122">İçine sağ tıklayın *StonglyTypedIndex()* seçip **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="c02d5-122">Right click inside of *StonglyTypedIndex()* and select **Add View**.</span></span> <span data-ttu-id="c02d5-123">Bu süre seçin **Blog** seçin ve Model sınıfı **listesi** İskele şablon olarak.</span><span class="sxs-lookup"><span data-stu-id="c02d5-123">This time select the **Blog** Model class and select **List** as the Scaffold template.</span></span>

<span data-ttu-id="c02d5-124">[![5658.StrongView [1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="c02d5-124">[![5658.StrongView[1]](dynamic-v-strongly-typed-views/_static/image6.png)](dynamic-v-strongly-typed-views/_static/image5.png)</span></span>

<span data-ttu-id="c02d5-125">Yeni Görünüm şablon içinde size IntelliSense desteği alın.</span><span class="sxs-lookup"><span data-stu-id="c02d5-125">Inside the new view template we get intellisense support.</span></span>

<span data-ttu-id="c02d5-126">[![7002.intellesince [1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="c02d5-126">[![7002.intellesince[1]](dynamic-v-strongly-typed-views/_static/image8.png)](dynamic-v-strongly-typed-views/_static/image7.png)</span></span>

<span data-ttu-id="c02d5-127">C# projesi indirilebilir [burada](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span><span class="sxs-lookup"><span data-stu-id="c02d5-127">The c# project can be downloaded [here](https://blogs.msdn.com/cfs-file.ashx/__key/CommunityServer-Blogs-Components-WeblogFiles/00-00-01-11-73-SSMS/1817.Mvc3ViewDemo.zip).</span></span>
