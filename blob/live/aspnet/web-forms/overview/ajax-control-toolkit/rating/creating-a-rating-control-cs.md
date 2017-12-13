---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
title: "Derecelendirme denetim (C#) oluşturma | Microsoft Docs"
author: wenz
description: "E-ticaret topluluk siteleri için birçok Web sitesi, kullanıcılar oranı makaleler veya öğeleri sunar. Bu genellikle bazı kodlama çaba gerektirir, ancak sahibiz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 969fb28f-2bff-4fc4-b24a-27f5e2534a37
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c004522ac72b848e42320862d77bced0c11ca15
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-rating-control-c"></a><span data-ttu-id="d0433-104">Derecelendirme denetim (C#) oluşturma</span><span class="sxs-lookup"><span data-stu-id="d0433-104">Creating a Rating Control (C#)</span></span>
====================
<span data-ttu-id="d0433-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d0433-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d0433-106">[Kodu indirme](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) veya [PDF indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d0433-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0CS.pdf)</span></span>

> <span data-ttu-id="d0433-107">E-ticaret topluluk siteleri için birçok Web sitesi, kullanıcılar oranı makaleler veya öğeleri sunar.</span><span class="sxs-lookup"><span data-stu-id="d0433-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="d0433-108">Bu genellikle bazı kodlama çaba gerektirir, ancak Denetim Araç Seti bizim elden sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="d0433-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="d0433-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="d0433-109">Overview</span></span>

<span data-ttu-id="d0433-110">E-ticaret topluluk siteleri için birçok Web sitesi, kullanıcılar oranı makaleler veya öğeleri sunar.</span><span class="sxs-lookup"><span data-stu-id="d0433-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="d0433-111">Bu genellikle bazı kodlama çaba gerektirir, ancak Denetim Araç Seti bizim elden sunuyoruz.</span><span class="sxs-lookup"><span data-stu-id="d0433-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="d0433-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="d0433-112">Steps</span></span>

<span data-ttu-id="d0433-113">Tüm, öncelikle görüntüler (en az) iki tür gerekir: bir doldurulmuş bir derecelendirme öğesi ve boş derecelendirme öğe için bir.</span><span class="sxs-lookup"><span data-stu-id="d0433-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="d0433-114">Bir derecelendirme genellikle bir yıldız veya bir gülen öğesidir.</span><span class="sxs-lookup"><span data-stu-id="d0433-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="d0433-115">Bu senaryo için üç dosyaları, smiley.png ve empty.png ve gülen done.png kaynak kodu yüklemeleri Bu öğreticinin bir parçası olarak bulur.</span><span class="sxs-lookup"><span data-stu-id="d0433-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="d0433-116">Ardından, yeni bir ASP.NET dosyası oluşturun ve ekleyerek başlayın bir `ScriptManager` kendisine denetimi:</span><span class="sxs-lookup"><span data-stu-id="d0433-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample1.aspx)]

<span data-ttu-id="d0433-117">Ardından, ekleyin `Rating` ASP.NET AJAX Denetim araç setinden denetim.</span><span class="sxs-lookup"><span data-stu-id="d0433-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="d0433-118">Aşağıdaki öznitelikler Bu örnek için ayarlanmış olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="d0433-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="d0433-119">`CurrentRating`kullanılacak ilk derecelendirme</span><span class="sxs-lookup"><span data-stu-id="d0433-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="d0433-120">`MaxRating`en yüksek derecelendirme</span><span class="sxs-lookup"><span data-stu-id="d0433-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="d0433-121">`EmptyStarCssClass`Derecelendirme öğesi (yıldız) boş olduğunda kullanılacak CSS sınıfı</span><span class="sxs-lookup"><span data-stu-id="d0433-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="d0433-122">`FilledStarCssClass`Derecelendirme öğesi (yıldız) doldurulurken kullanılacak CSS sınıfı</span><span class="sxs-lookup"><span data-stu-id="d0433-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="d0433-123">`StarCssClass`görünür stat için kullanılacak CSS sınıfı</span><span class="sxs-lookup"><span data-stu-id="d0433-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="d0433-124">`WaitingStarCssClass`bir yıldız derecesiyle sunucuya geri gönderilirken kullanılacak CSS sınıfı</span><span class="sxs-lookup"><span data-stu-id="d0433-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="d0433-125">Burada da bir derecelendirme denetimi ile beşinci oluşturan biçimlendirme hangisinin hiçbiri doldurulur çıkışı başlangıçta öğeleri (smileys):</span><span class="sxs-lookup"><span data-stu-id="d0433-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample2.aspx)]

<span data-ttu-id="d0433-126">Üç başvurulan CSS sınıfları şimdi uygun görüntü dosyaları göstermek hangi CSS kullanarak yapmak kolaydır gerekir:</span><span class="sxs-lookup"><span data-stu-id="d0433-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-cs/samples/sample3.css)]

<span data-ttu-id="d0433-127">Genişlik ve yükseklik üç görüntülerinin sağlar, aksi takdirde görüntü yukarı biraz messed görünebilir emin olun.</span><span class="sxs-lookup"><span data-stu-id="d0433-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="d0433-128">Son olarak, derecelendirme sonucunu kullanıcıya görüntüleneceğini (veya, en az bir veritabanına kaydedilir).</span><span class="sxs-lookup"><span data-stu-id="d0433-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="d0433-129">Bu nedenle bir kısa mesaj ve bir gönderme düğmesi derecelendirme formun sunucuya geri göndermek için çıktısı için bir etiket ekleyin:</span><span class="sxs-lookup"><span data-stu-id="d0433-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample4.aspx)]

<span data-ttu-id="d0433-130">Sunucu tarafı kodu erişim derecelendirme denetimi aracılığıyla kendi `ID` ve ardından erişim kendi `CurrentRating` örneğimizde 0 ile 5 arasında bir değer seçili derecelendirme öğe sayısı olan özelliği.</span><span class="sxs-lookup"><span data-stu-id="d0433-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-cs/samples/sample5.aspx)]

<span data-ttu-id="d0433-131">Sayfa kaydedin ve tarayıcınıza yükleyin.</span><span class="sxs-lookup"><span data-stu-id="d0433-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="d0433-132">(Başlangıçta boş) derecelendirme öğelerin üzerine geldiğinizde, bir JavaScript efekti oluşur: derecelendirme değişiklikleri.</span><span class="sxs-lookup"><span data-stu-id="d0433-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="d0433-133">Yıldız kümesinde tıkladığınızda, geçerli derecelendirme korunur.</span><span class="sxs-lookup"><span data-stu-id="d0433-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="d0433-134">Son olarak, formu gönderdiğinde, sunucu tarafı kodu seçili derecelendirme çıkarır.</span><span class="sxs-lookup"><span data-stu-id="d0433-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="d0433-135">[![Bir derecelendirme sistemi ile en az kod oluşturma](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d0433-135">[![Creating a rating system with minimal code](creating-a-rating-control-cs/_static/image2.png)](creating-a-rating-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="d0433-136">Bir derecelendirme sistemi ile en az kod oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-rating-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d0433-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d0433-137">Sonraki</span><span class="sxs-lookup"><span data-stu-id="d0433-137">Next</span></span>](creating-a-rating-control-vb.md)
