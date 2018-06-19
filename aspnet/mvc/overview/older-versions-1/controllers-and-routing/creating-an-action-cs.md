---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Bir eylem (C#) oluşturma | Microsoft Docs
author: microsoft
description: ASP.NET MVC denetleyicisi için yeni bir eylem eklemeyi öğrenin. Bir yöntemin bir eylem gereksinimleri hakkında bilgi edinin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c6145902db59b07e96a5563b138c1a6323946b2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867911"
---
<a name="creating-an-action-c"></a><span data-ttu-id="f442b-104">Bir eylem (C#) oluşturma</span><span class="sxs-lookup"><span data-stu-id="f442b-104">Creating an Action (C#)</span></span>
====================
<span data-ttu-id="f442b-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="f442b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="f442b-106">ASP.NET MVC denetleyicisi için yeni bir eylem eklemeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="f442b-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="f442b-107">Bir yöntemin bir eylem gereksinimleri hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="f442b-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="f442b-108">Bu öğretici, yeni bir denetleyici eylemi nasıl oluşturabileceğinizi açıklamak için hedefidir.</span><span class="sxs-lookup"><span data-stu-id="f442b-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="f442b-109">Bir eylem yönteminin gereksinimleri hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="f442b-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="f442b-110">Ayrıca bir yöntem bir eylem olarak açıklanmasını önlemenize öğrenin.</span><span class="sxs-lookup"><span data-stu-id="f442b-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="f442b-111">Bir denetleyici için eylem ekleme</span><span class="sxs-lookup"><span data-stu-id="f442b-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="f442b-112">Yeni bir eylem denetleyicisi için yeni bir yöntem ekleyerek bir denetleyiciye ekleyin.</span><span class="sxs-lookup"><span data-stu-id="f442b-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="f442b-113">Örneğin, denetleyici listeleme 1 İNDİS() adlı bir eylem ve SayHello() adlı bir eylem içerir.</span><span class="sxs-lookup"><span data-stu-id="f442b-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="f442b-114">Her iki yöntem eylemler olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="f442b-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="f442b-115">**1 - Controllers\HomeController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="f442b-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="f442b-116">Bir eylem olarak universe maruz kalabilir için bir yöntem belirli gereksinimleri karşılaması gerekir:</span><span class="sxs-lookup"><span data-stu-id="f442b-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="f442b-117">Metodu genel olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="f442b-117">The method must be public.</span></span>
- <span data-ttu-id="f442b-118">Yöntem statik yöntemi olamaz.</span><span class="sxs-lookup"><span data-stu-id="f442b-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="f442b-119">Yöntemi, bir genişletme yöntemi olamaz.</span><span class="sxs-lookup"><span data-stu-id="f442b-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="f442b-120">Yöntem oluşturucusu, alıcı veya ayarlayıcı olamaz.</span><span class="sxs-lookup"><span data-stu-id="f442b-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="f442b-121">Yöntemi, açık genel türleri sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="f442b-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="f442b-122">Yöntemi denetleyici temel sınıfın bir yöntem değildir.</span><span class="sxs-lookup"><span data-stu-id="f442b-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="f442b-123">Yöntem içeremez **ref** veya **çıkışı** parametreleri.</span><span class="sxs-lookup"><span data-stu-id="f442b-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="f442b-124">Bir denetleyici eylemi dönüş türü üzerinde herhangi bir kısıtlama olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="f442b-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="f442b-125">Bir denetleyici eylemi bir dize bir DateTime rastgele sınıfının veya void bir örnek döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="f442b-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="f442b-126">ASP.NET MVC çerçevesi bir eylem sonucu bir dizeye olmayan herhangi bir dönüş türü dönüştürün ve tarayıcı dizeye işleme.</span><span class="sxs-lookup"><span data-stu-id="f442b-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="f442b-127">Bu gereksinimleri denetleyicisi ihlal olmayan herhangi bir yöntemini eklediğinizde, yöntemi bir denetleyici eylemi sunulur.</span><span class="sxs-lookup"><span data-stu-id="f442b-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="f442b-128">Burada dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="f442b-128">Be careful here.</span></span> <span data-ttu-id="f442b-129">Bir denetleyici eylemi, Internet'e bağlı herhangi bir kişi tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="f442b-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="f442b-130">Örneğin, bir DeleteMyWebsite() denetleyici eylemi oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="f442b-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="f442b-131">Genel yöntem çağrılmasını önleme</span><span class="sxs-lookup"><span data-stu-id="f442b-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="f442b-132">Genel yöntem bir controller sınıfında oluşturmanız gerekir ve metodu olarak bir denetleyici eylemi açmak istemiyorsanız [NonAction] özniteliğini kullanarak çağrılmasını yöntemi engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="f442b-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="f442b-133">Örneğin, denetleyici listeleme 2'deki [NonAction] özniteliği ile donatılmış CompanySecrets() adlı genel bir yöntem içerir.</span><span class="sxs-lookup"><span data-stu-id="f442b-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="f442b-134">**2 - Controllers\WorkController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="f442b-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="f442b-135">Ardından, tarayıcınızın adres çubuğuna /Work/CompanySecrets yazarak CompanySecrets() denetleyici eylemini çağırma çalışırsanız, Şekil 1'de hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="f442b-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="f442b-136">[![NonAction yöntemi çağırma](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f442b-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="f442b-137">**Şekil 01**: NonAction yöntemi çağırma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="f442b-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f442b-138">[Önceki](creating-a-controller-cs.md)
> [sonraki](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f442b-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
