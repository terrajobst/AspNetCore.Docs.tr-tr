---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Bir denetleyici (VB) oluşturma | Microsoft Docs
author: StephenWalther
description: Bu öğreticide, Stephen Walther bir denetleyici için bir ASP.NET MVC uygulamasını nasıl ekleyebileceğiniz gösterilmektedir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: e9a2bbcb09672f5247429064908cd4d2ef67f518
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879016"
---
<a name="creating-a-controller-vb"></a><span data-ttu-id="a3463-103">Bir denetleyici (VB) oluşturma</span><span class="sxs-lookup"><span data-stu-id="a3463-103">Creating a Controller (VB)</span></span>
====================
<span data-ttu-id="a3463-104">tarafından [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="a3463-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="a3463-105">Bu öğreticide, Stephen Walther bir denetleyici için bir ASP.NET MVC uygulamasını nasıl ekleyebileceğiniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="a3463-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="a3463-106">Bu öğretici, yeni ASP.NET MVC denetleyicileri nasıl oluşturabileceğinizi açıklamak için hedefidir.</span><span class="sxs-lookup"><span data-stu-id="a3463-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="a3463-107">Visual Studio denetleyici Ekle menü seçeneğini kullanarak hem bir sınıf dosyası el ile oluşturarak denetleyicileri oluşturmayı öğrenin.</span><span class="sxs-lookup"><span data-stu-id="a3463-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="a3463-108">Kullanarak denetleyici menü seçeneği ekleyin</span><span class="sxs-lookup"><span data-stu-id="a3463-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="a3463-109">Visual Studio Çözüm Gezgini penceresinde denetleyicileri klasörü sağ tıklatın ve seçmek için yeni bir denetleyicisi oluşturmak için en kolay yolu olan **Ekle, denetleyici** menü seçeneği (bkz: Şekil 1).</span><span class="sxs-lookup"><span data-stu-id="a3463-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="a3463-110">Bu menü seçeneğini seçerek açılır **denetleyici Ekle** iletişim (bkz: Şekil 2).</span><span class="sxs-lookup"><span data-stu-id="a3463-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="a3463-111">[![Yeni Proje iletişim kutusu](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="a3463-111">[![The New Project dialog box](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span></span>

<span data-ttu-id="a3463-112">**Şekil 01**: yeni bir denetleyicisi ekleme ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-controller-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="a3463-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-vb/_static/image2.png))</span></span>


<span data-ttu-id="a3463-113">[![Yeni Proje iletişim kutusu](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="a3463-113">[![The New Project dialog box](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span></span>

<span data-ttu-id="a3463-114">**Şekil 02**: Denetleyici Ekle iletişim kutusu ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-controller-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="a3463-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-vb/_static/image4.png))</span></span>


<span data-ttu-id="a3463-115">Denetleyici adı ilk bölümü vurgulanan bildirimi **denetleyici Ekle** iletişim.</span><span class="sxs-lookup"><span data-stu-id="a3463-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="a3463-116">Her Denetleyici adı son eki ile bitmelidir *denetleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="a3463-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="a3463-117">Örneğin, adlı bir denetleyicisi oluşturabilirsiniz *ProductController* ancak adlı bir denetleyicisi *ürün*.</span><span class="sxs-lookup"><span data-stu-id="a3463-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="a3463-118">Eksik bir denetleyici oluşturursanız *denetleyicisi* denetleyicisi çağırma açamayacaksınız sonra soneki.</span><span class="sxs-lookup"><span data-stu-id="a3463-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="a3463-119">Bunu yapmayın--ı my ömrü sayısız saatleri bu hata yaptıktan sonra küçülttüğü iyi bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="a3463-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="a3463-120">**1 - Controllers\ProductController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="a3463-120">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

<span data-ttu-id="a3463-121">Her zaman denetleyicileri klasöründe denetleyicileri oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="a3463-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="a3463-122">Aksi takdirde, ASP.NET MVC kurallarını ihlal ve diğer geliştiriciler, uygulamanızın anlamak daha zor bir zaman gerekir.</span><span class="sxs-lookup"><span data-stu-id="a3463-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="a3463-123">Yapı iskelesi eylem yöntemleri</span><span class="sxs-lookup"><span data-stu-id="a3463-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="a3463-124">Bir denetleyici oluşturduğunuzda, oluşturma, güncelleştirme ve ayrıntılarını eylem yöntemlerine otomatik olarak oluşturmak için seçeneğiniz vardır (Şekil 3 bakın).</span><span class="sxs-lookup"><span data-stu-id="a3463-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="a3463-125">Bu seçeneği belirlerseniz listeleme 2 denetleyicisi sınıfında oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="a3463-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="a3463-126">[![Eylem yöntemleri otomatik olarak oluşturma](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="a3463-126">[![Creating action methods automatically](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span></span>

<span data-ttu-id="a3463-127">**Şekil 03**: eylem yöntemlerine otomatik olarak oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-controller-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="a3463-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-vb/_static/image6.png))</span></span>


<span data-ttu-id="a3463-128">**2 - Controllers\CustomerController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="a3463-128">**Listing 2 - Controllers\CustomerController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

<span data-ttu-id="a3463-129">Bu oluşturulan yöntemler saplama yöntemleridir.</span><span class="sxs-lookup"><span data-stu-id="a3463-129">These generated methods are stub methods.</span></span> <span data-ttu-id="a3463-130">Oluşturma, güncelleştirme ve bir müşteri için kendiniz ayrıntıları gösteren fiili mantığı eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a3463-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="a3463-131">Ancak, saplama yöntemleri ile iyi bir başlangıç noktası sağlar.</span><span class="sxs-lookup"><span data-stu-id="a3463-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="a3463-132">Denetleyici sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="a3463-132">Creating a Controller Class</span></span>

<span data-ttu-id="a3463-133">ASP.NET MVC denetleyicisi yalnızca bir sınıftır.</span><span class="sxs-lookup"><span data-stu-id="a3463-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="a3463-134">İsterseniz, uygun Visual Studio denetleyicisi yapı iskelesi yoksay ve denetleyici sınıfını el ile oluşturun.</span><span class="sxs-lookup"><span data-stu-id="a3463-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="a3463-135">Aşağıdaki adımları uygulayın:</span><span class="sxs-lookup"><span data-stu-id="a3463-135">Follow these steps:</span></span>

1. <span data-ttu-id="a3463-136">Denetleyicileri klasörünü sağ tıklatın ve menü seçeneğini belirleyin **Ekle, yeni öğe** seçip **sınıfı** şablonu (Şekil 4'e bakın).</span><span class="sxs-lookup"><span data-stu-id="a3463-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="a3463-137">Yeni sınıf PersonController.vb ve tıklayın **Ekle** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="a3463-137">Name the new class PersonController.vb and click the **Add** button.</span></span>
3. <span data-ttu-id="a3463-138">Sonuçta elde edilen sınıf dosyasını sınıf taban System.Web.Mvc.Controller (3 listeleme bakın) sınıftan şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="a3463-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="a3463-139">[![Yeni bir sınıf oluşturma](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="a3463-139">[![Creating a new class](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span></span>

<span data-ttu-id="a3463-140">**Şekil 04**: yeni bir sınıf oluşturma ([tam boyutlu görüntüyü görüntülemek için tıklatın](creating-a-controller-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="a3463-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-vb/_static/image8.png))</span></span>


<span data-ttu-id="a3463-141">**3 - Controllers\PersonController.vb listeleme**</span><span class="sxs-lookup"><span data-stu-id="a3463-141">**Listing 3 - Controllers\PersonController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

<span data-ttu-id="a3463-142">"Hello World!" dizesini döndürür İNDİS() adlı bir eylem listeleme 3'te denetleyicisi sunar.</span><span class="sxs-lookup"><span data-stu-id="a3463-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="a3463-143">Bu denetleyici eylemi, uygulamanızı çalıştıran ve aşağıdaki gibi bir URL isteyen çağırabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="a3463-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="a3463-144">ASP.NET Geliştirme Sunucusu, bir rastgele bağlantı noktası numarası (örneğin, 40071) kullanır.</span><span class="sxs-lookup"><span data-stu-id="a3463-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="a3463-145">Bir denetleyici çağırmak için bir URL girerken, doğru bağlantı noktası numarası girmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a3463-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="a3463-146">ASP.NET Geliştirme Sunucusu (sağ alt ekranınızın) Windows bildirim alanındaki simgenin üzerine farenizi gelerek, bağlantı noktası numarasını belirleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a3463-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="a3463-147">[Önceki](adding-dynamic-content-to-a-cached-page-vb.md)
> [sonraki](creating-an-action-vb.md)</span><span class="sxs-lookup"><span data-stu-id="a3463-147">[Previous](adding-dynamic-content-to-a-cached-page-vb.md)
[Next](creating-an-action-vb.md)</span></span>
