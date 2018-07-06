---
uid: web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
title: Derecelendirme denetimi (VB) oluşturma | Microsoft Docs
author: wenz
description: E-ticaret topluluk sitelerine, birçok Web sitesi kullanıcılarının oranı makaleler veya öğeleri sunar. Bu genellikle bazı bir kodlama çabası gerektirir, ancak sahibiz...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 6d0d70f4-725e-4258-8ae8-24a6ba1ddbf7
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/rating/creating-a-rating-control-vb
msc.type: authoredcontent
ms.openlocfilehash: cb8bd8ab400bf5633b8218e724ad86cf7271699d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811979"
---
<a name="creating-a-rating-control-vb"></a><span data-ttu-id="1568d-104">Derecelendirme denetimi (VB) oluşturma</span><span class="sxs-lookup"><span data-stu-id="1568d-104">Creating a Rating Control (VB)</span></span>
====================
<span data-ttu-id="1568d-105">tarafından [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1568d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1568d-106">[Kodu indir](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="1568d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/rating0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/rating0VB.pdf)</span></span>

> <span data-ttu-id="1568d-107">E-ticaret topluluk sitelerine, birçok Web sitesi kullanıcılarının oranı makaleler veya öğeleri sunar.</span><span class="sxs-lookup"><span data-stu-id="1568d-107">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="1568d-108">Bu genellikle bazı bir kodlama çabası gerektirir, ancak Denetim Araç Seti için sunduğumuz elden sahibiz.</span><span class="sxs-lookup"><span data-stu-id="1568d-108">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>


## <a name="overview"></a><span data-ttu-id="1568d-109">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="1568d-109">Overview</span></span>

<span data-ttu-id="1568d-110">E-ticaret topluluk sitelerine, birçok Web sitesi kullanıcılarının oranı makaleler veya öğeleri sunar.</span><span class="sxs-lookup"><span data-stu-id="1568d-110">Many websites, from e-commerce to community sites, offer their users to rate articles or items.</span></span> <span data-ttu-id="1568d-111">Bu genellikle bazı bir kodlama çabası gerektirir, ancak Denetim Araç Seti için sunduğumuz elden sahibiz.</span><span class="sxs-lookup"><span data-stu-id="1568d-111">This usually requires some coding effort, but we do have the Control Toolkit to our disposal.</span></span>

## <a name="steps"></a><span data-ttu-id="1568d-112">Adımlar</span><span class="sxs-lookup"><span data-stu-id="1568d-112">Steps</span></span>

<span data-ttu-id="1568d-113">Öncelikle, görüntüleri (en az) iki tür ihtiyacınız: bir doldurulmuş bir derecelendirme öğesi ve boş derecelendirme öğenin bir.</span><span class="sxs-lookup"><span data-stu-id="1568d-113">First of all, you need (at least) two kinds of images: one for a filled out rating item, and one for an empty rating item.</span></span> <span data-ttu-id="1568d-114">Bir derecelendirme genellikle bir yıldız veya bir gülen öğesidir.</span><span class="sxs-lookup"><span data-stu-id="1568d-114">A rating item is usually a star or a smiley.</span></span> <span data-ttu-id="1568d-115">Bu senaryo için üç dosyaları, smiley.png ve empty.png ve gülen done.png Bu öğretici için kaynak kodu indirmeleri bir parçası olarak bulabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="1568d-115">For this scenario, you find three files, smiley.png and empty.png and smiley-done.png as part of the source code downloads for this tutorial.</span></span>

<span data-ttu-id="1568d-116">Ardından, yeni bir ASP.NET dosyası oluşturun ve ekleyerek başlayın bir `ScriptManager` denetimi:</span><span class="sxs-lookup"><span data-stu-id="1568d-116">Then, create a new ASP.NET file and start with adding a `ScriptManager` control to it:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample1.aspx)]

<span data-ttu-id="1568d-117">Ardından, ekleme `Rating` ASP.NET AJAX Denetim Araç Seti denetim.</span><span class="sxs-lookup"><span data-stu-id="1568d-117">Then, add the `Rating` control from the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="1568d-118">Aşağıdaki öznitelikler için bu örneği ayarlamanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="1568d-118">The following attributes need to be set for this example:</span></span>

- <span data-ttu-id="1568d-119">`CurrentRating` kullanılacak ilk derecelendirme</span><span class="sxs-lookup"><span data-stu-id="1568d-119">`CurrentRating` the initial rating to be used</span></span>
- <span data-ttu-id="1568d-120">`MaxRating` en yüksek derecelendirme</span><span class="sxs-lookup"><span data-stu-id="1568d-120">`MaxRating` the maximum rating</span></span>
- <span data-ttu-id="1568d-121">`EmptyStarCssClass` CSS sınıfının bir derecelendirme öğesi (star) boş olduğunda kullanın</span><span class="sxs-lookup"><span data-stu-id="1568d-121">`EmptyStarCssClass` the CSS class to use when a rating item ( star ) is empty</span></span>
- <span data-ttu-id="1568d-122">`FilledStarCssClass` bir derecelendirme öğesi (star) doldurulurken kullanmak için CSS sınıfı</span><span class="sxs-lookup"><span data-stu-id="1568d-122">`FilledStarCssClass` the CSS class to use when a rating item ( star ) is filled out</span></span>
- <span data-ttu-id="1568d-123">`StarCssClass` görünür bir durum için kullanılacak bir CSS sınıfı</span><span class="sxs-lookup"><span data-stu-id="1568d-123">`StarCssClass` the CSS class to use for a visible stat</span></span>
- <span data-ttu-id="1568d-124">`WaitingStarCssClass` Yıldız derecelendirmesi sunucuya geri gönderilirken kullanılacak CSS sınıfı</span><span class="sxs-lookup"><span data-stu-id="1568d-124">`WaitingStarCssClass` the CSS class to use while a star rating is sent back to the server</span></span>

<span data-ttu-id="1568d-125">Ve derecelendirme denetimi ile beşinci oluşturan biçimlendirmesi şöyledir hangi none doldurulan başlangıçta öğeleri (smileys):</span><span class="sxs-lookup"><span data-stu-id="1568d-125">And here is the markup which creates a rating control with five items (smileys) of which none is filled out initially:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample2.aspx)]

<span data-ttu-id="1568d-126">Üç başvurulan CSS sınıfları artık uygun görüntü dosyaları göstermek, CSS kullanarak yapmak kolaydır gerekir:</span><span class="sxs-lookup"><span data-stu-id="1568d-126">The three referenced CSS classes now need to show the appropriate image files, which is easy to do using CSS:</span></span>

[!code-css[Main](creating-a-rating-control-vb/samples/sample3.css)]

<span data-ttu-id="1568d-127">Üç görüntü yüksekliğini ve genişliğini sağlarsanız, görüntüyü oluşturan bir bit messed boyutlandırılabilecek emin olun.</span><span class="sxs-lookup"><span data-stu-id="1568d-127">Make sure that you provide the width and height of the three images, otherwise the display may look a bit messed up.</span></span>

<span data-ttu-id="1568d-128">Son olarak, derecelendirme sonucunu kullanıcıya görüntüleneceğini (veya, en az bir veritabanında kayıtlı).</span><span class="sxs-lookup"><span data-stu-id="1568d-128">Finally, the result of the rating should be displayed to the user (or, at least saved in a database).</span></span> <span data-ttu-id="1568d-129">Bu nedenle sunucu derecelendirme forma geri göndermek için bir kısa mesaj ve bir Gönder düğmesi çıktısı için bir etiket ekleyin:</span><span class="sxs-lookup"><span data-stu-id="1568d-129">So add a label for the output of a text message and a submit button to post back the rating form to the server:</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample4.aspx)]

<span data-ttu-id="1568d-130">Sunucu tarafı kodunda erişim derecelendirme denetimi aracılığıyla kendi `ID` ve ardından erişim kendi `CurrentRating` örneğimizde 0 ile 5 arasında bir değer seçili derecelendirme öğe sayısı olan özelliği.</span><span class="sxs-lookup"><span data-stu-id="1568d-130">In the server-side code, access the Rating control via its `ID` and then access its `CurrentRating` property which is the number of the selected rating items, in our example a value between 0 and 5.</span></span>

[!code-aspx[Main](creating-a-rating-control-vb/samples/sample5.aspx)]

<span data-ttu-id="1568d-131">Sayfayı kaydedin ve tarayıcınıza yükleyin.</span><span class="sxs-lookup"><span data-stu-id="1568d-131">Save the page and load it into your browser.</span></span> <span data-ttu-id="1568d-132">(Başlangıçta boş) derecelendirmesi öğelerin üzerine geldiğinizde, bir JavaScript etkisi oluşur: derecelendirme değişiklikleri.</span><span class="sxs-lookup"><span data-stu-id="1568d-132">When you hover over the (initially empty) rating items, a JavaScript effect occurs: The rating changes.</span></span> <span data-ttu-id="1568d-133">Yıldızlar kümesini temel tıkladığınızda, geçerli derecelendirme korunur.</span><span class="sxs-lookup"><span data-stu-id="1568d-133">When you click on the set of stars, the current rating is retained.</span></span> <span data-ttu-id="1568d-134">Son olarak, formu gönderdiğinde, sunucu tarafı kodu seçili derecelendirme çıkarır.</span><span class="sxs-lookup"><span data-stu-id="1568d-134">Finally, when you submit the form, the server-side code outputs the selected rating.</span></span>


<span data-ttu-id="1568d-135">[![En az kodla derecelendirme sistem oluşturma](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1568d-135">[![Creating a rating system with minimal code](creating-a-rating-control-vb/_static/image2.png)](creating-a-rating-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="1568d-136">En az kodla derecelendirme sistemi oluşturma ([tam boyutlu görüntüyü görmek için tıklatın](creating-a-rating-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1568d-136">Creating a rating system with minimal code ([Click to view full-size image](creating-a-rating-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1568d-137">Önceki</span><span class="sxs-lookup"><span data-stu-id="1568d-137">Previous</span></span>](creating-a-rating-control-cs.md)
