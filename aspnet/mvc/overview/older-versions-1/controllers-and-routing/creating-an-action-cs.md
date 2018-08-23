---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Eylem (C#) oluşturma | Microsoft Docs
author: microsoft
description: ASP.NET MVC denetleyicisi için yeni bir eylem eklemeyi öğrenin. Bir eylem için bir yöntem gereksinimleri hakkında bilgi edinin.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 243248ee30c6a2db7f102f7743d0393d4a6a9d24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752320"
---
<a name="creating-an-action-c"></a><span data-ttu-id="59976-104">Eylem (C#) oluşturma</span><span class="sxs-lookup"><span data-stu-id="59976-104">Creating an Action (C#)</span></span>
====================
<span data-ttu-id="59976-105">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="59976-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="59976-106">ASP.NET MVC denetleyicisi için yeni bir eylem eklemeyi öğrenin.</span><span class="sxs-lookup"><span data-stu-id="59976-106">Learn how to add a new action to an ASP.NET MVC controller.</span></span> <span data-ttu-id="59976-107">Bir eylem için bir yöntem gereksinimleri hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="59976-107">Learn about the requirements for a method to be an action.</span></span>


<span data-ttu-id="59976-108">Bu öğreticinin amacı, yeni bir denetleyici eylemi nasıl oluşturacağınızı açıklar sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="59976-108">The goal of this tutorial is to explain how you can create a new controller action.</span></span> <span data-ttu-id="59976-109">Bir eylem yönteminin gereksinimleri hakkında bilgi edinin.</span><span class="sxs-lookup"><span data-stu-id="59976-109">You learn about the requirements of an action method.</span></span> <span data-ttu-id="59976-110">Ayrıca bir yöntem bir eylem olarak açıklanmasını önlemenize öğrenin.</span><span class="sxs-lookup"><span data-stu-id="59976-110">You also learn how to prevent a method from being exposed as an action.</span></span>

## <a name="adding-an-action-to-a-controller"></a><span data-ttu-id="59976-111">Denetleyici için eylem ekleme</span><span class="sxs-lookup"><span data-stu-id="59976-111">Adding an Action to a Controller</span></span>

<span data-ttu-id="59976-112">Bir denetleyici için yeni bir yöntem denetleyiciye ekleyerek yeni bir eylem ekleyin.</span><span class="sxs-lookup"><span data-stu-id="59976-112">You add a new action to a controller by adding a new method to the controller.</span></span> <span data-ttu-id="59976-113">Örneğin, denetleyici 1 listeleme İNDİS() adlı bir eylem ve SayHello() adlı bir eylem içerir.</span><span class="sxs-lookup"><span data-stu-id="59976-113">For example, the controller in Listing 1 contains an action named Index() and an action named SayHello().</span></span> <span data-ttu-id="59976-114">Her iki yöntem de eylem olarak kullanıma sunulur.</span><span class="sxs-lookup"><span data-stu-id="59976-114">Both methods are exposed as actions.</span></span>

<span data-ttu-id="59976-115">**1 - Controllers\HomeController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="59976-115">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

<span data-ttu-id="59976-116">Bir eylem olarak evreni maruz kalabilir için bir yöntem belirli gereksinimleri karşılamalıdır:</span><span class="sxs-lookup"><span data-stu-id="59976-116">In order to be exposed to the universe as an action, a method must meet certain requirements:</span></span>

- <span data-ttu-id="59976-117">Yöntemi, ortak olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="59976-117">The method must be public.</span></span>
- <span data-ttu-id="59976-118">Yöntem statik bir yöntemi olamaz.</span><span class="sxs-lookup"><span data-stu-id="59976-118">The method cannot be a static method.</span></span>
- <span data-ttu-id="59976-119">Yöntemin bir genişletme yöntemi olamaz.</span><span class="sxs-lookup"><span data-stu-id="59976-119">The method cannot be an extension method.</span></span>
- <span data-ttu-id="59976-120">Bir oluşturucu, alıcı veya ayarlayıcı yöntemi olamaz.</span><span class="sxs-lookup"><span data-stu-id="59976-120">The method cannot be a constructor, getter, or setter.</span></span>
- <span data-ttu-id="59976-121">Yöntemi, açık genel türler sahip olamaz.</span><span class="sxs-lookup"><span data-stu-id="59976-121">The method cannot have open generic types.</span></span>
- <span data-ttu-id="59976-122">Yöntemi denetleyici temel sınıfın bir yöntem değil.</span><span class="sxs-lookup"><span data-stu-id="59976-122">The method is not a method of the controller base class.</span></span>
- <span data-ttu-id="59976-123">Bir yöntemi içeremez **ref** veya **kullanıma** parametreleri.</span><span class="sxs-lookup"><span data-stu-id="59976-123">The method cannot contain **ref** or **out** parameters.</span></span>

<span data-ttu-id="59976-124">Bir denetleyici eylemi dönüş türüne hiçbir kısıtlama olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="59976-124">Notice that there are no restrictions on the return type of a controller action.</span></span> <span data-ttu-id="59976-125">Bir denetleyici eylemi bir dize bir DateTime Random sınıfını veya void örneği döndürebilir.</span><span class="sxs-lookup"><span data-stu-id="59976-125">A controller action can return a string, a DateTime, an instance of the Random class, or void.</span></span> <span data-ttu-id="59976-126">ASP.NET MVC çerçevesi bir eylem sonucu bir dizeye değil herhangi bir dönüş türü dönüştürmek ve tarayıcıya dize işleme.</span><span class="sxs-lookup"><span data-stu-id="59976-126">The ASP.NET MVC framework will convert any return type that is not an action result into a string and render the string to the browser.</span></span>

<span data-ttu-id="59976-127">Bu gereksinimleri denetleyicisi ihlal etmemesini herhangi bir yöntemi eklediğinizde, yöntem bir denetleyici eylemi kullanıma sunulur.</span><span class="sxs-lookup"><span data-stu-id="59976-127">When you add any method that does not violate these requirements to a controller, the method is exposed as a controller action.</span></span> <span data-ttu-id="59976-128">Burada dikkatli olun.</span><span class="sxs-lookup"><span data-stu-id="59976-128">Be careful here.</span></span> <span data-ttu-id="59976-129">Bir denetleyici eylemi, Internet'e bağlı herhangi bir kişi tarafından çağrılabilir.</span><span class="sxs-lookup"><span data-stu-id="59976-129">A controller action can be invoked by anyone connected to the Internet.</span></span> <span data-ttu-id="59976-130">Örneğin, bir DeleteMyWebsite() denetleyici eylemi oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="59976-130">Do not, for example, create a DeleteMyWebsite() controller action.</span></span>

## <a name="preventing-a-public-method-from-being-invoked"></a><span data-ttu-id="59976-131">Genel bir yöntem çağrılmakta olan önleme</span><span class="sxs-lookup"><span data-stu-id="59976-131">Preventing a Public Method from Being Invoked</span></span>

<span data-ttu-id="59976-132">Ardından bir controller sınıfında genel bir yöntem oluşturmanız gerekir ve yöntemi bir denetleyici eylemi olarak kullanıma sunmak istemiyorsanız, yöntemi [NonAction] özniteliğini kullanarak çağrılmasını engelleyebilir.</span><span class="sxs-lookup"><span data-stu-id="59976-132">If you need to create a public method in a controller class and you don't want to expose the method as a controller action then you can prevent the method from being invoked by using the [NonAction] attribute.</span></span> <span data-ttu-id="59976-133">Örneğin, denetleyici listeleme 2'deki [NonAction] özniteliği ile donatılmış CompanySecrets() adlı bir genel yöntem içerir.</span><span class="sxs-lookup"><span data-stu-id="59976-133">For example, the controller in Listing 2 contains a public method named CompanySecrets() that is decorated with the [NonAction] attribute.</span></span>

<span data-ttu-id="59976-134">**2 - Controllers\WorkController.cs listeleme**</span><span class="sxs-lookup"><span data-stu-id="59976-134">**Listing 2 - Controllers\WorkController.cs**</span></span>

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

<span data-ttu-id="59976-135">Tarayıcınızın adres çubuğuna /Work/CompanySecrets yazarak CompanySecrets() denetleyici eylemini çağırma denerseniz, Şekil 1'de hata iletisi alırsınız.</span><span class="sxs-lookup"><span data-stu-id="59976-135">If you attempt to invoke the CompanySecrets() controller action by typing /Work/CompanySecrets into the address bar of your browser then you'll get the error message in Figure 1.</span></span>


<span data-ttu-id="59976-136">[![NonAction yöntemi çağırma](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="59976-136">[![Invoking a NonAction method](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)</span></span>

<span data-ttu-id="59976-137">**Şekil 01**: NonAction yöntemi çağırma ([tam boyutlu görüntüyü görmek için tıklatın](creating-an-action-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="59976-137">**Figure 01**: Invoking a NonAction method([Click to view full-size image](creating-an-action-cs/_static/image2.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="59976-138">[Önceki](creating-a-controller-cs.md)
> [İleri](asp-net-mvc-routing-overview-vb.md)</span><span class="sxs-lookup"><span data-stu-id="59976-138">[Previous](creating-a-controller-cs.md)
[Next](asp-net-mvc-routing-overview-vb.md)</span></span>
