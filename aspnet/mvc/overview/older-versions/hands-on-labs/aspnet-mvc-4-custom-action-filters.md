---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: "ASP.NET MVC 4 özel eylem filtreleri | Microsoft Docs"
author: rick-anderson
description: "ASP.NET MVC önce ya da bir eylem yöntemi çağrıldıktan sonra filtreleme mantığını yürütme için eylem filtrelerini sağlar. Eylem filtreleri özel öznitelikler tha olan..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 103cd68c576463d87d0077cc149f9b89c6e028e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="37f3d-104">ASP.NET MVC 4 özel eylem filtreleri</span><span class="sxs-lookup"><span data-stu-id="37f3d-104">ASP.NET MVC 4 Custom Action Filters</span></span>
====================
<span data-ttu-id="37f3d-105">tarafından [Web Camps ekibi](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="37f3d-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="37f3d-106">ASP.NET MVC önce ya da bir eylem yöntemi çağrıldıktan sonra filtreleme mantığını yürütme için eylem filtrelerini sağlar.</span><span class="sxs-lookup"><span data-stu-id="37f3d-106">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="37f3d-107">Eylem filtreleri eylem öncesi ve sonrası eylem davranışı denetleyicinin eylem yöntemlerini eklemek için bildirime sağlayan özel öznitelikleridir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-107">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>
> 
> <span data-ttu-id="37f3d-108">Bu uygulamalı laboratuar ortamında MvcMusicStore çözüm denetleyicinin istekleri yakalamak ve bir site etkinlik bir veritabanı tablosunun içine oturum için bir özel eylem filtresi öznitelik oluşturur.</span><span class="sxs-lookup"><span data-stu-id="37f3d-108">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="37f3d-109">Günlüğe yazma filtresini yerleştirme tarafından herhangi bir denetleyici veya eylem eklemeniz mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="37f3d-109">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="37f3d-110">Son olarak, ziyaretçi listesini gösterir günlük görünümü görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-110">Finally, you will see the log view that shows the list of visitors.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="37f3d-111">Bu uygulamalı Laboratuvar temel bilgiye sahip varsayar **ASP.NET MVC**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-111">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="37f3d-112">Değil kullandıysanız **ASP.NET MVC** gitmenizi öneririz önce **ASP.NET MVC 4 Temelleri** uygulamalı Laboratuvar.</span><span class="sxs-lookup"><span data-stu-id="37f3d-112">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="37f3d-113">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="37f3d-113">Objectives</span></span>

<span data-ttu-id="37f3d-114">Bu uygulamalı laboratuar ortamında, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="37f3d-114">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="37f3d-115">Filtreleme yeteneklerini artırmak için bir özel eylem filtresi öznitelik oluşturun</span><span class="sxs-lookup"><span data-stu-id="37f3d-115">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="37f3d-116">Belirli bir düzeye yerleştirme tarafından özel bir filtre özniteliğini uygulayın</span><span class="sxs-lookup"><span data-stu-id="37f3d-116">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="37f3d-117">Genel bir özel eylem Filtreleri Kaydet</span><span class="sxs-lookup"><span data-stu-id="37f3d-117">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="37f3d-118">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="37f3d-118">Prerequisites</span></span>

<span data-ttu-id="37f3d-119">Bu laboratuvarı tamamlamak için aşağıdaki öğeleri sahip olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="37f3d-119">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="37f3d-120">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="37f3d-120">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="37f3d-121">Kurulum</span><span class="sxs-lookup"><span data-stu-id="37f3d-121">Setup</span></span>

<span data-ttu-id="37f3d-122">**Kod parçacıkları yükleme**</span><span class="sxs-lookup"><span data-stu-id="37f3d-122">**Installing Code Snippets**</span></span>

<span data-ttu-id="37f3d-123">Kolaylık olması için bu Laboratuvar yönetme kod çoğunu Visual Studio kod parçacıkları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-123">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="37f3d-124">Çalıştırma kod parçacıkları yüklemek için **.\Source\Setup\CodeSnippets.vsi** dosya.</span><span class="sxs-lookup"><span data-stu-id="37f3d-124">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="37f3d-125">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek C: kullanarak kod parçacıkları](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="37f3d-125">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="37f3d-126">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="37f3d-126">Exercises</span></span>

<span data-ttu-id="37f3d-127">Bu uygulamalı Laboratuvar tarafından aşağıdaki alıştırmaları oluşmaktadır:</span><span class="sxs-lookup"><span data-stu-id="37f3d-127">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="37f3d-128">Alıştırma 1: Eylemler günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="37f3d-128">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="37f3d-129">Alıştırma 2: Birden çok eylem filtrelerini yönetme</span><span class="sxs-lookup"><span data-stu-id="37f3d-129">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="37f3d-130">Bu laboratuvarı tamamlamak için süre tahmini: **30 dakika**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-130">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="37f3d-131">Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="37f3d-131">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="37f3d-132">Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-132">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="37f3d-133">Alıştırma 1: Eylemler günlüğe kaydetme</span><span class="sxs-lookup"><span data-stu-id="37f3d-133">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="37f3d-134">Bu alıştırmada, ASP.NET MVC 4 filtre sağlayıcıları kullanarak bir özel eylem günlüğü filtresi oluşturma öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-134">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="37f3d-135">Bu amaç için günlüğe yazma filtresini tüm etkinlikleri seçili denetleyicileri kaydedilecek MusicStore site uygulanır.</span><span class="sxs-lookup"><span data-stu-id="37f3d-135">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="37f3d-136">Filtre uzatır **ActionFilterAttributeClass** ve geçersiz kılma **OnActionExecuting** her istek catch ve günlüğe kaydetme eylemleri gerçekleştirmek için yöntem.</span><span class="sxs-lookup"><span data-stu-id="37f3d-136">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="37f3d-137">HTTP istekleri hakkında bağlam bilgisi, yöntemleri, sonuçları ve parametreleri yürütme ASP.NET MVC tarafından sağlanacak **ActionExecutingContext** sınıfı **.**</span><span class="sxs-lookup"><span data-stu-id="37f3d-137">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="37f3d-138">ASP.NET MVC 4, özel bir filtre oluşturmadan kullanabileceğiniz varsayılan filtreleri sağlayıcıları da sahiptir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-138">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="37f3d-139">ASP.NET MVC 4 Aşağıdaki filtre türleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="37f3d-139">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="37f3d-140">**Yetkilendirme** filtre, hangi kimlik doğrulaması gerçekleştirmeye veya istek özelliklerini doğrulama gibi bir eylem yönteminin yürütülecek olup olmadığına dair güvenlik kararlarını verir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-140">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="37f3d-141">**Eylem** eylem yöntemi yürütme sarmalar filtre.</span><span class="sxs-lookup"><span data-stu-id="37f3d-141">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="37f3d-142">Bu filtre eylem yöntemine ek verileri sağlayan, dönüş değerini inceleyerek veya eylem yönteminin yürütülmesi iptal etme gibi ek işleme gerçekleştirebilirsiniz</span><span class="sxs-lookup"><span data-stu-id="37f3d-142">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="37f3d-143">**Sonuç** ActionResult nesnesinin yürütme sarmalar filtre.</span><span class="sxs-lookup"><span data-stu-id="37f3d-143">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="37f3d-144">Bu filtre sonucunun HTTP yanıtı değiştirme gibi ek işleme gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-144">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="37f3d-145">**Özel durum** yere eylem yöntemindeki yetkilendirme filtreleri ile başlayıp sonucu yürütülmesiyle işlenmemiş özel durum ise, yürütür filtre.</span><span class="sxs-lookup"><span data-stu-id="37f3d-145">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="37f3d-146">Özel durum filtreleri, günlük veya bir hata sayfası görüntüleme gibi görevleri için kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-146">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="37f3d-147">Filtre sağlayıcıları hakkında daha fazla bilgi için bu MSDN bağlantıyı ziyaret edin: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)).</span><span class="sxs-lookup"><span data-stu-id="37f3d-147">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="37f3d-148">MVC müzik deposu uygulama günlüğü özelliği hakkında</span><span class="sxs-lookup"><span data-stu-id="37f3d-148">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="37f3d-149">Bu müzik deposu çözüm site günlüğü için yeni bir veri modeli tablosu sahip **ActionLog**, aşağıdaki alanlarla: bir istek, aranan eylemini, istemci IP'si ve zaman damgası alınan denetleyicinin adı.</span><span class="sxs-lookup"><span data-stu-id="37f3d-149">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="37f3d-150">![Veri modeli. ActionLog tablo. ] (aspnet-mvc-4-custom-action-filters/_static/image1.png "Veri modeli. ActionLog tablo.")</span><span class="sxs-lookup"><span data-stu-id="37f3d-150">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="37f3d-151">*Veri modeli - ActionLog tablosu*</span><span class="sxs-lookup"><span data-stu-id="37f3d-151">*Data model - ActionLog table*</span></span>

<span data-ttu-id="37f3d-152">Konumunda bulunabilir eylem günlüğü için bir ASP.NET MVC görünümü bir çözüm sunar **görünümler/MvcMusicStores/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="37f3d-152">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="37f3d-153">![Eylem günlüğü görünümü](aspnet-mvc-4-custom-action-filters/_static/image2.png "eylem günlüğü görüntüle")</span><span class="sxs-lookup"><span data-stu-id="37f3d-153">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="37f3d-154">*Eylem Günlüğü Görüntüle*</span><span class="sxs-lookup"><span data-stu-id="37f3d-154">*Action Log view*</span></span>

<span data-ttu-id="37f3d-155">Denetleyicinin isteği kesintiye ve özel filtreleme kullanarak günlüğe kaydetme gerçekleştirme bu yapısı verilen, tüm iş odaklanmış.</span><span class="sxs-lookup"><span data-stu-id="37f3d-155">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="37f3d-156">Görev 1 - bir denetleyicinin isteği yakalamak için özel bir filtresi oluşturma</span><span class="sxs-lookup"><span data-stu-id="37f3d-156">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="37f3d-157">Bu görevde günlük kaydı mantığı içeren bir özel filtre öznitelik sınıfı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="37f3d-157">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="37f3d-158">Bu amaç için ASP.NET MVC uzatır **ActionFilterAttribute** sınıfı ve uygulama arabirimi **IActionFilter**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-158">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="37f3d-159">**ActionFilterAttribute** tüm öznitelik filtreleri için temel sınıftır.</span><span class="sxs-lookup"><span data-stu-id="37f3d-159">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="37f3d-160">Belirli bir mantığı sonra ve denetleyici eylemin yürütme önce yürütmek için aşağıdaki yöntemleri sağlar:</span><span class="sxs-lookup"><span data-stu-id="37f3d-160">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="37f3d-161">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span><span class="sxs-lookup"><span data-stu-id="37f3d-161">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="37f3d-162">**OnActionExecuted**(ActionExecutedContext filterContext): eylem yöntemi çağrıldıktan sonra ve sonucu (Görünüm oluşturmadan önce) yürütülmeden önce.</span><span class="sxs-lookup"><span data-stu-id="37f3d-162">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="37f3d-163">**OnResultExecuting**(ResultExecutingContext filterContext): (Görünüm oluşturmadan önce) sonucu yürütülmeden önce yalnızca.</span><span class="sxs-lookup"><span data-stu-id="37f3d-163">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="37f3d-164">**OnResultExecuted**(ResultExecutedContext filterContext): (Görünüm işlenen sonra) sonucu yürütüldükten sonra.</span><span class="sxs-lookup"><span data-stu-id="37f3d-164">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="37f3d-165">Bu yöntemlerin herhangi biriyle türetilmiş bir sınıf geçersiz kılarak, filtreleme kodunuzu yürütebilir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-165">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="37f3d-166">Açık **başlamak** çözüm bulunan **\Source\Ex01-LoggingActions\Begin** klasör.</span><span class="sxs-lookup"><span data-stu-id="37f3d-166">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

    1. <span data-ttu-id="37f3d-167">Devam etmeden önce bazı eksik NuGet paketlerini karşıdan yüklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-167">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="37f3d-168">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-168">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="37f3d-169">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="37f3d-169">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="37f3d-170">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-170">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="37f3d-171">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-171">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="37f3d-172">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="37f3d-172">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="37f3d-173">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="37f3d-173">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
    > 
    > <span data-ttu-id="37f3d-174">Bu makalede daha fazla bilgi için bkz: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="37f3d-174">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="37f3d-175">Yeni bir C# sınıfına ekleme **filtreleri** klasörü ve adlandırın *CustomActionFilter.cs*.</span><span class="sxs-lookup"><span data-stu-id="37f3d-175">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="37f3d-176">Bu klasör, tüm özel filtreler depolar.</span><span class="sxs-lookup"><span data-stu-id="37f3d-176">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="37f3d-177">Açık **CustomActionFilter.cs** ve bir başvuru ekleyin **System.Web.Mvc** ve **MvcMusicStore.Models** ad alanları:</span><span class="sxs-lookup"><span data-stu-id="37f3d-177">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="37f3d-178">(Kod parçacığını - *ASP.NET MVC 4 özel eylem filtreleri - Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="37f3d-178">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="37f3d-179">Devralma **CustomActionFilter** sınıfıyla **ActionFilterAttribute** yapın **CustomActionFilter** sınıfı uygulama **IActionFilter** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="37f3d-179">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="37f3d-180">Olun **CustomActionFilter** sınıfı yöntemi geçersiz kılma **OnActionExecuting** ve filtrenin yürütme oturum için gerekli mantığı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="37f3d-180">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="37f3d-181">Bunu yapmak için aşağıdaki vurgulanmış kodu içinde ekleyin **CustomActionFilter** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="37f3d-181">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="37f3d-182">(Kod parçacığını - *ASP.NET MVC 4 özel eylem filtreleri - Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="37f3d-182">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="37f3d-183">**OnActionExecuting** yöntemi kullanarak **Entity Framework** yeni bir ActionLog kayıt eklemek için.</span><span class="sxs-lookup"><span data-stu-id="37f3d-183">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="37f3d-184">Oluşturur ve yeni bir varlık örneğinin bağlamı bilgilerle doldurur **filterContext**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-184">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="37f3d-185">Daha fazla bilgi edinebilirsiniz **ControllerContext** adresindeki sınıf [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="37f3d-185">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="37f3d-186">Görev 2 - bir kod dinleyiciyi depolama denetleyicisi sınıfına Injecting</span><span class="sxs-lookup"><span data-stu-id="37f3d-186">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="37f3d-187">Bu görevde tüm denetleyicisi sınıfları ve kaydedilir denetleyici eylemleri ekleyerek özel filtre ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-187">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="37f3d-188">Bu alıştırmada amacıyla depolama denetleyicisi sınıfı bir günlük sahip olur.</span><span class="sxs-lookup"><span data-stu-id="37f3d-188">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="37f3d-189">Yöntem **OnActionExecuting** gelen **ActionLogFilterAttribute** özel filtre eklenen öğenin çağrıldığında çalışır.</span><span class="sxs-lookup"><span data-stu-id="37f3d-189">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="37f3d-190">Belirli denetleyici yöntemi müdahale mümkündür.</span><span class="sxs-lookup"><span data-stu-id="37f3d-190">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="37f3d-191">Açık **StoreController** adresindeki **MvcMusicStore\Controllers** ve bir başvuru ekleyin **filtreleri** ad alanı:</span><span class="sxs-lookup"><span data-stu-id="37f3d-191">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="37f3d-192">Özel filtre ekleme **CustomActionFilter** içine **StoreController** ekleyerek sınıfı **[CustomActionFilter]** sınıf bildiriminin önce öznitelik.</span><span class="sxs-lookup"><span data-stu-id="37f3d-192">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

    > [!NOTE]
    > <span data-ttu-id="37f3d-193">Bir filtre denetleyici sınıfına eklenen, tüm eylemleri de eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="37f3d-193">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="37f3d-194">Yalnızca bir dizi eylemi için filtre uygulamak istiyorsanız, eklemesine olurdu **[CustomActionFilter]** her biri için:</span><span class="sxs-lookup"><span data-stu-id="37f3d-194">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="37f3d-195">Görev 3 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="37f3d-195">Task 3 - Running the Application</span></span>

<span data-ttu-id="37f3d-196">Bu görevde, günlüğe yazma filtresini çalışıp çalışmadığını test edeceğiz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-196">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="37f3d-197">Uygulamayı başlatın ve mağazası ziyaret edebilmesi ve günlüğe kaydedilmiş etkinliklerin kontrol eder.</span><span class="sxs-lookup"><span data-stu-id="37f3d-197">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="37f3d-198">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-198">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="37f3d-199">Gözat **/ActionLog** günlük Görünüm ilk durumunu görmek için:</span><span class="sxs-lookup"><span data-stu-id="37f3d-199">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="37f3d-200">![İzleyici durum sayfa etkinliği önce oturum](aspnet-mvc-4-custom-action-filters/_static/image3.png "oturum sayfa etkinliği önce İzleyici durumu")</span><span class="sxs-lookup"><span data-stu-id="37f3d-200">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="37f3d-201">*Sayfa etkinliği önce günlük İzleyici durumu*</span><span class="sxs-lookup"><span data-stu-id="37f3d-201">*Log tracker status before page activity*</span></span>

    > [!NOTE]
    > <span data-ttu-id="37f3d-202">Varsayılan olarak, var olan türler menüsü alınırken oluşturulan bir öğe her zaman gösterir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-202">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
    > 
    > <span data-ttu-id="37f3d-203">Kolaylık olması amacıyla şu Temizleme **ActionLog** tablo uygulamanın yalnızca belirli her görevin doğrulama günlükleri gösterir şekilde her çalıştığında.</span><span class="sxs-lookup"><span data-stu-id="37f3d-203">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
    > 
    > <span data-ttu-id="37f3d-204">Aşağıdaki kod kaldırmanız gerekebilir **oturum\_Başlat** yöntemi (içinde **Global.asax** sınıfı) deposu içinde yürütülen tüm eylemler için geçmişe dönük bir günlüğünü kaydetmek için Denetleyici.</span><span class="sxs-lookup"><span data-stu-id="37f3d-204">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
    > 
    > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="37f3d-205">Birini tıklatın **türler** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-205">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="37f3d-206">Gözat **/ActionLog** ve günlük boş tuşuna ise **F5** sayfayı yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="37f3d-206">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="37f3d-207">Ziyaretleriniz izlenmekte olan denetleyin:</span><span class="sxs-lookup"><span data-stu-id="37f3d-207">Check that your visits were tracked:</span></span>

    <span data-ttu-id="37f3d-208">![Eylem günlüğü ile kaydedilen etkinliğin](aspnet-mvc-4-custom-action-filters/_static/image4.png "oturum aktivitesiyle eylem günlüğü")</span><span class="sxs-lookup"><span data-stu-id="37f3d-208">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="37f3d-209">*Eylem günlüğü ile kaydedilen etkinliğin*</span><span class="sxs-lookup"><span data-stu-id="37f3d-209">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="37f3d-210">Alıştırma 2: Birden çok eylem filtrelerini yönetme</span><span class="sxs-lookup"><span data-stu-id="37f3d-210">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="37f3d-211">Bu alıştırmada, ikinci bir özel eylem filtre StoreController sınıfına ekleyin ve hem filtreleri yürütülecek belirli bir sıraya tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-211">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="37f3d-212">Sonra genel filtre kaydetmek için kodu güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-212">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="37f3d-213">Filtreler yürütme sırası tanımlarken dikkate farklı seçenekler vardır.</span><span class="sxs-lookup"><span data-stu-id="37f3d-213">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="37f3d-214">Örneğin, sipariş özelliği ve filtreleri kapsamı:</span><span class="sxs-lookup"><span data-stu-id="37f3d-214">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="37f3d-215">Tanımlayabileceğiniz bir **kapsam** filtrelerin her biri için örneğin, içinde çalıştırmak için tüm eylem filtrelerini kapsamını **denetleyicisi kapsamı**ve çalıştırmak için tüm yetkilendirme filtrelerini **genel kapsamlı** .</span><span class="sxs-lookup"><span data-stu-id="37f3d-215">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="37f3d-216">Tanımlı yürütme sırası kapsamlar.</span><span class="sxs-lookup"><span data-stu-id="37f3d-216">The scopes have a defined execution order.</span></span>

<span data-ttu-id="37f3d-217">Ayrıca, her eylem filtresi filtre kapsamı yürütme sırasında belirlemek için kullanılan bir sipariş özelliğine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-217">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="37f3d-218">Özel eylem filtrelerini yürütme sırası hakkında daha fazla bilgi için lütfen bu MSDN makalesine bakın: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span><span class="sxs-lookup"><span data-stu-id="37f3d-218">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="37f3d-219">Görev 1: yeni bir özel eylem filtresi oluşturma</span><span class="sxs-lookup"><span data-stu-id="37f3d-219">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="37f3d-220">Bu görevde, filtrelerin uygulanma sırası yönetmek öğrenme StoreController sınıfına eklemesine yeni bir özel eylem filtresi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="37f3d-220">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="37f3d-221">Açık **başlamak** çözüm bulunan **\Source\Ex02-ManagingMultipleActionFilters\Begin** klasör.</span><span class="sxs-lookup"><span data-stu-id="37f3d-221">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="37f3d-222">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="37f3d-222">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="37f3d-223">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="37f3d-223">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="37f3d-224">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-224">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="37f3d-225">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="37f3d-225">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="37f3d-226">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-226">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="37f3d-227">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-227">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="37f3d-228">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="37f3d-228">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="37f3d-229">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="37f3d-229">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="37f3d-230">Bu makalede daha fazla bilgi için bkz: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span><span class="sxs-lookup"><span data-stu-id="37f3d-230">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="37f3d-231">Yeni bir C# sınıfına ekleme **filtreleri** klasörü ve adlandırın *MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="37f3d-231">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="37f3d-232">Açık **MyNewCustomActionFilter.cs** ve bir başvuru ekleyin **System.Web.Mvc** ve **MvcMusicStore.Models** ad alanı:</span><span class="sxs-lookup"><span data-stu-id="37f3d-232">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="37f3d-233">(Kod parçacığını - *ASP.NET MVC 4 özel eylem filtreleri - Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="37f3d-233">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="37f3d-234">Varsayılan sınıf bildirimi aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="37f3d-234">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="37f3d-235">(Kod parçacığını - *ASP.NET MVC 4 özel eylem filtreleri - Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="37f3d-235">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="37f3d-236">Bu özel eylem filtresi neredeyse daha önceki alıştırmada oluşturulan aynıdır.</span><span class="sxs-lookup"><span data-stu-id="37f3d-236">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="37f3d-237">Ana farktır olduğunu  *&quot;oturum tarafından&quot;*  öznitelik wich filtresi tanımlamak için bu yeni sınıf adıyla güncelleştirildiğinde günlük kayıtlı.</span><span class="sxs-lookup"><span data-stu-id="37f3d-237">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="37f3d-238">Görev 2: yeni bir kod dinleyiciyi StoreController sınıfına Injecting.</span><span class="sxs-lookup"><span data-stu-id="37f3d-238">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="37f3d-239">Bu görevde, yeni bir özel filtre StoreController sınıfına ekleyin ve nasıl hem filtreleri birlikte çalışmasını doğrulamak için çözümü çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-239">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="37f3d-240">Açık **StoreController** sınıfı bulunan **MvcMusicStore\Controllers** ve yeni özel filtre ekleme **MyNewCustomActionFilter** içine  **StoreController** gibi sınıfına aşağıdaki kodda gösterilir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-240">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="37f3d-241">Şimdi, bu iki özel eylem filtrelerini nasıl çalıştığını görmek için uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-241">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="37f3d-242">Bunu yapmak için basın **F5** ve uygulama başlatana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="37f3d-242">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="37f3d-243">Gözat **/ActionLog** günlük Görünüm ilk durumunu görmek için.</span><span class="sxs-lookup"><span data-stu-id="37f3d-243">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="37f3d-244">![İzleyici durum sayfa etkinliği önce oturum](aspnet-mvc-4-custom-action-filters/_static/image5.png "oturum sayfa etkinliği önce İzleyici durumu")</span><span class="sxs-lookup"><span data-stu-id="37f3d-244">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="37f3d-245">*Sayfa etkinliği önce günlük İzleyici durumu*</span><span class="sxs-lookup"><span data-stu-id="37f3d-245">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="37f3d-246">Birini tıklatın **türler** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-246">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="37f3d-247">Bu süre denetleyin; ziyaretleriniz iki kez izlenmekte olan: her özel eylem filtreleri için de eklendikten sonra **StorageController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="37f3d-247">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="37f3d-248">![Eylem günlüğü ile kaydedilen etkinliğin](aspnet-mvc-4-custom-action-filters/_static/image6.png "oturum aktivitesiyle eylem günlüğü")</span><span class="sxs-lookup"><span data-stu-id="37f3d-248">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="37f3d-249">*Eylem günlüğü ile kaydedilen etkinliğin*</span><span class="sxs-lookup"><span data-stu-id="37f3d-249">*Action log with activity logged*</span></span>
6. <span data-ttu-id="37f3d-250">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-250">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="37f3d-251">Görev 3: Filtresi sıralama yönetme</span><span class="sxs-lookup"><span data-stu-id="37f3d-251">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="37f3d-252">Bu görevde, sipariş verilir kullanarak filtreleri yürütme sırası yönetmeyi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-252">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="37f3d-253">Açık **StoreController** sınıfı bulunan **MvcMusicStore\Controllers** ve belirtin **sipariş** hem filtreleri özelliğinde ister aşağıda gösterilen.</span><span class="sxs-lookup"><span data-stu-id="37f3d-253">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="37f3d-254">Şimdi, filtreleri, sipariş özellik değerine bağlı olarak nasıl yürütüldüğünden emin olun.</span><span class="sxs-lookup"><span data-stu-id="37f3d-254">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="37f3d-255">En küçük sıra değeri filtresiyle bulabilirsiniz (**CustomActionFilter**) yürütülen ilk sağlayıcıdır.</span><span class="sxs-lookup"><span data-stu-id="37f3d-255">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="37f3d-256">Tuşuna **F5** ve uygulama başlatana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="37f3d-256">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="37f3d-257">Gözat **/ActionLog** günlük Görünüm ilk durumunu görmek için.</span><span class="sxs-lookup"><span data-stu-id="37f3d-257">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="37f3d-258">![İzleyici durum sayfa etkinliği önce oturum](aspnet-mvc-4-custom-action-filters/_static/image7.png "oturum sayfa etkinliği önce İzleyici durumu")</span><span class="sxs-lookup"><span data-stu-id="37f3d-258">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="37f3d-259">*Sayfa etkinliği önce günlük İzleyici durumu*</span><span class="sxs-lookup"><span data-stu-id="37f3d-259">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="37f3d-260">Birini tıklatın **türler** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-260">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="37f3d-261">Bu süre, ziyaretleriniz izlenmekte olan onay filtreleri sipariş değere göre sıralı: **CustomActionFilter** günlükleri ilk.</span><span class="sxs-lookup"><span data-stu-id="37f3d-261">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="37f3d-262">![Eylem günlüğü ile kaydedilen etkinliğin](aspnet-mvc-4-custom-action-filters/_static/image8.png "oturum aktivitesiyle eylem günlüğü")</span><span class="sxs-lookup"><span data-stu-id="37f3d-262">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="37f3d-263">*Eylem günlüğü ile kaydedilen etkinliğin*</span><span class="sxs-lookup"><span data-stu-id="37f3d-263">*Action log with activity logged*</span></span>
6. <span data-ttu-id="37f3d-264">Şimdi, filtreleri sipariş değeri güncelleştirin ve günlük sırası nasıl değiştiğini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-264">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="37f3d-265">İçinde **StoreController** sınıfı, aşağıda gösterildiği gibi filtreler sıra değeri güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="37f3d-265">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="37f3d-266">Tuşlarına basarak uygulamayı yeniden çalıştırın **F5**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-266">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="37f3d-267">Birini tıklatın **türler** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-267">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="37f3d-268">Bu süre, günlükleri tarafından oluşturulan denetleyin **MyNewCustomActionFilter** filtre ilk görünür.</span><span class="sxs-lookup"><span data-stu-id="37f3d-268">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="37f3d-269">![Eylem günlüğü ile kaydedilen etkinliğin](aspnet-mvc-4-custom-action-filters/_static/image9.png "oturum aktivitesiyle eylem günlüğü")</span><span class="sxs-lookup"><span data-stu-id="37f3d-269">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="37f3d-270">*Eylem günlüğü ile kaydedilen etkinliğin*</span><span class="sxs-lookup"><span data-stu-id="37f3d-270">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="37f3d-271">Görev 4: Kaydetme genel filtreler</span><span class="sxs-lookup"><span data-stu-id="37f3d-271">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="37f3d-272">Bu görevde, yeni filtre kaydetmek için çözüm güncelleştirmek (**MyNewCustomActionFilter**) genel filtre olarak.</span><span class="sxs-lookup"><span data-stu-id="37f3d-272">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="37f3d-273">Bunu yaparak, bu uygulamada ve yalnızca önceki görev olduğu gibi olanları StoreController ilgili tüm eylemleri perfomed tarafından tetiklenir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-273">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="37f3d-274">İçinde **StoreController** sınıfı, Kaldır **[MyNewCustomActionFilter]** özniteliği ve sipariş özelliğinden **[CustomActionFilter]**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-274">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="37f3d-275">Aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="37f3d-275">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="37f3d-276">Açık **Global.asax** dosya ve bulun **uygulama\_Başlat** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="37f3d-276">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="37f3d-277">Her thime uygulama başlar, bildirim kaydeden genel filtreleri çağırarak **RegisterGlobalFilters** yöntemi içinde **FilterConfig** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="37f3d-277">Notice that each thime the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="37f3d-278">![Genel filtrelerin Global.asax dosyasında kaydetme](aspnet-mvc-4-custom-action-filters/_static/image10.png "Global.asax dosyasında genel filtreleri kaydetme")</span><span class="sxs-lookup"><span data-stu-id="37f3d-278">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="37f3d-279">*Genel filtrelerin Global.asax dosyasında kaydetme*</span><span class="sxs-lookup"><span data-stu-id="37f3d-279">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="37f3d-280">Açık **FilterConfig.cs** içinde dosya **uygulama\_Başlat** klasör.</span><span class="sxs-lookup"><span data-stu-id="37f3d-280">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="37f3d-281">System.Web.Mvc kullanarak bir başvuru ekleyin; MvcMusicStore.Filters kullanarak; ad alanı.</span><span class="sxs-lookup"><span data-stu-id="37f3d-281">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="37f3d-282">Güncelleştirme **RegisterGlobalFilters** özel filtre ekleme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="37f3d-282">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="37f3d-283">Bunu yapmak için vurgulanmış kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="37f3d-283">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="37f3d-284">Tuşlarına basarak uygulamayı çalıştırın **F5**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-284">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="37f3d-285">Birini tıklatın **türler** menüsünde ve kullanılabilir albüm gözatma gibi bazı eylemleri gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-285">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="37f3d-286">Onay artık **[MyNewCustomActionFilter]** HomeController ve ActionLogController çok eklenmiş.</span><span class="sxs-lookup"><span data-stu-id="37f3d-286">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="37f3d-287">![Eylem günlüğü ile kaydedilen etkinliğin](aspnet-mvc-4-custom-action-filters/_static/image11.png "oturum aktivitesiyle eylem günlüğü")</span><span class="sxs-lookup"><span data-stu-id="37f3d-287">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="37f3d-288">*Genel etkinlik oturum eylem günlüğü*</span><span class="sxs-lookup"><span data-stu-id="37f3d-288">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="37f3d-289">Ayrıca, Windows Azure Web siteleri aşağıdaki bu uygulamayı dağıtabilmek için [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="37f3d-289">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="37f3d-290">Özet</span><span class="sxs-lookup"><span data-stu-id="37f3d-290">Summary</span></span>

<span data-ttu-id="37f3d-291">Bu uygulamalı Laboratuvar tamamlayarak özel eylemleri yürütmek için bir eylem filtresi genişletmek nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-291">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="37f3d-292">Ayrıca, sayfa denetleyicilerine herhangi bir filtre eklemesine öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-292">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="37f3d-293">Aşağıdaki kavramlar kullanılıyordu:</span><span class="sxs-lookup"><span data-stu-id="37f3d-293">The following concepts were used:</span></span>

- <span data-ttu-id="37f3d-294">Özel eylem filtreleri ile ASP.NET MVC ActionFilterAttribute sınıfındaki oluşturma</span><span class="sxs-lookup"><span data-stu-id="37f3d-294">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="37f3d-295">ASP.NET MVC denetleyicileri filtreleri eklemesine nasıl</span><span class="sxs-lookup"><span data-stu-id="37f3d-295">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="37f3d-296">Filtre sırası özelliğini kullanarak sıralama yönetme</span><span class="sxs-lookup"><span data-stu-id="37f3d-296">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="37f3d-297">Filtreleri genel olarak kaydetme</span><span class="sxs-lookup"><span data-stu-id="37f3d-297">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="37f3d-298">Ek A: Yükleme Web Visual Studio Express 2012 için</span><span class="sxs-lookup"><span data-stu-id="37f3d-298">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="37f3d-299">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak  **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="37f3d-299">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="37f3d-300">Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="37f3d-300">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="37f3d-301">Git [ [https://go.microsoft.com/? LinkID 9810169 =](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="37f3d-301">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="37f3d-302">Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *Visual Studio Express 2012 için Windows Azure SDK'sı Web*&quot;.</span><span class="sxs-lookup"><span data-stu-id="37f3d-302">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="37f3d-303">Tıklayın **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-303">Click on **Install Now**.</span></span> <span data-ttu-id="37f3d-304">Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-304">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="37f3d-305">Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="37f3d-305">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="37f3d-306">![Visual Studio Express yükleme](aspnet-mvc-4-custom-action-filters/_static/image12.png "yükleme Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="37f3d-306">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="37f3d-307">*Visual Studio Express yükleme*</span><span class="sxs-lookup"><span data-stu-id="37f3d-307">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="37f3d-308">Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="37f3d-308">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşulları kabul ediliyor](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="37f3d-310">*Lisans koşulları kabul ediliyor*</span><span class="sxs-lookup"><span data-stu-id="37f3d-310">*Accepting the license terms*</span></span>
5. <span data-ttu-id="37f3d-311">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="37f3d-311">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="37f3d-313">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="37f3d-313">*Installation progress*</span></span>
6. <span data-ttu-id="37f3d-314">Yükleme tamamlandığında tıklatın **son**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-314">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="37f3d-316">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="37f3d-316">*Installation completed*</span></span>
7. <span data-ttu-id="37f3d-317">Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-317">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="37f3d-318">Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.</span><span class="sxs-lookup"><span data-stu-id="37f3d-318">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web döşemeye](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="37f3d-320">*VS Express Web döşemeye*</span><span class="sxs-lookup"><span data-stu-id="37f3d-320">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="37f3d-321">Ek B: Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="37f3d-321">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="37f3d-322">Bu ekte Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturmak ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlamak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-322">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="37f3d-323">Görev 1 - yeni bir Web sitesi oluşturma Windows Azure portalı</span><span class="sxs-lookup"><span data-stu-id="37f3d-323">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="37f3d-324">Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) ve aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-324">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="37f3d-325">Windows Azure ile 10 ASP.NET Web siteleri ücretsiz barındırma ve ardından trafiğiniz büyüdükçe ölçeğinizi.</span><span class="sxs-lookup"><span data-stu-id="37f3d-325">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="37f3d-326">Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="37f3d-326">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="37f3d-327">![Windows Azure portalında oturum açtığı](aspnet-mvc-4-custom-action-filters/_static/image17.png "Windows Azure Portal'da oturum açın")</span><span class="sxs-lookup"><span data-stu-id="37f3d-327">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="37f3d-328">*Windows Azure yönetim portalında oturum açtığı*</span><span class="sxs-lookup"><span data-stu-id="37f3d-328">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="37f3d-329">Tıklatın **yeni** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="37f3d-329">Click **New** on the command bar.</span></span>

    <span data-ttu-id="37f3d-330">![Yeni bir Web sitesi oluşturma](aspnet-mvc-4-custom-action-filters/_static/image18.png "yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="37f3d-330">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="37f3d-331">*Yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="37f3d-331">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="37f3d-332">Tıklatın **işlem** | **Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-332">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="37f3d-333">Ardından **hızlı Oluştur** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="37f3d-333">Then select **Quick Create** option.</span></span> <span data-ttu-id="37f3d-334">Yeni web sitesi için kullanılabilir bir URL girin ve tıklayın **Web sitesi oluştur**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-334">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="37f3d-335">Windows Azure Web sitesi, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="37f3d-335">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="37f3d-336">Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portal dışındaki dağıtmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="37f3d-336">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="37f3d-337">Bir veritabanını ayarlamak için adımları içermez.</span><span class="sxs-lookup"><span data-stu-id="37f3d-337">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="37f3d-338">![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](aspnet-mvc-4-custom-action-filters/_static/image19.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="37f3d-338">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="37f3d-339">*Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="37f3d-339">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="37f3d-340">Yeni kadar bekleyin **Web sitesi** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="37f3d-340">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="37f3d-341">Web sitesi oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun.</span><span class="sxs-lookup"><span data-stu-id="37f3d-341">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="37f3d-342">Yeni Web sitesi çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="37f3d-342">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="37f3d-343">![Yeni web sitesi için gözatma](aspnet-mvc-4-custom-action-filters/_static/image20.png "yeni web sitesi için gözatma")</span><span class="sxs-lookup"><span data-stu-id="37f3d-343">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="37f3d-344">*Yeni web sitesi için gözatma*</span><span class="sxs-lookup"><span data-stu-id="37f3d-344">*Browsing to the new web site*</span></span>

    <span data-ttu-id="37f3d-345">![Çalışan Web sitesi](aspnet-mvc-4-custom-action-filters/_static/image21.png "çalışan Web sitesi")</span><span class="sxs-lookup"><span data-stu-id="37f3d-345">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="37f3d-346">*Çalışan Web sitesi*</span><span class="sxs-lookup"><span data-stu-id="37f3d-346">*Web site running*</span></span>
6. <span data-ttu-id="37f3d-347">Portalına geri dönün ve web sitesi altında adına tıklayın **adı** yönetim sayfaları görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="37f3d-347">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="37f3d-348">![Web sitesi Yönetim sayfalarının açma](aspnet-mvc-4-custom-action-filters/_static/image22.png "web sitesi Yönetim sayfalarının açma")</span><span class="sxs-lookup"><span data-stu-id="37f3d-348">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="37f3d-349">*Web Sitesi Yönetim sayfalarının açma*</span><span class="sxs-lookup"><span data-stu-id="37f3d-349">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="37f3d-350">İçinde **Pano** sayfasında **Hızlı Bakış** 'yi tıklatın **yayım profili indirin** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="37f3d-350">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="37f3d-351">*Yayımlama profili* her etkin yayımlama yöntemi için bir Windows Azure Web sitesi bir web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-351">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="37f3d-352">Yayımlama profili URL'leri, kullanıcı kimlik bilgilerini ve bağlanmak ve her bir yayımlama yönteminin etkinleştirildiği uç noktaları karşı kimlik doğrulaması için gerekli veritabanı dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-352">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="37f3d-353">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma destek yayımlamak için bu programlar yapılandırılmasını otomatikleştirmek için profilleri Windows Azure Web siteleri için Web uygulamaları yayımlama.</span><span class="sxs-lookup"><span data-stu-id="37f3d-353">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="37f3d-354">![Yayımlama profili web sitesi Yükleniyor](aspnet-mvc-4-custom-action-filters/_static/image23.png "yayımlama profili web sitesi yükleniyor")</span><span class="sxs-lookup"><span data-stu-id="37f3d-354">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="37f3d-355">*Yayımlama profili Web sitesi yükleniyor*</span><span class="sxs-lookup"><span data-stu-id="37f3d-355">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="37f3d-356">Yayımlama profili dosyasını bilinen bir konuma indirin.</span><span class="sxs-lookup"><span data-stu-id="37f3d-356">Download the publish profile file to a known location.</span></span> <span data-ttu-id="37f3d-357">Daha fazla Bu alıştırmada Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlamak için bu dosyayı kullanmak nasıl göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="37f3d-357">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="37f3d-358">![Yayımlama profili dosyasını kaydetme](aspnet-mvc-4-custom-action-filters/_static/image24.png "yayımlama profilini kaydetme")</span><span class="sxs-lookup"><span data-stu-id="37f3d-358">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="37f3d-359">*Yayımlama profili dosyasını kaydetme*</span><span class="sxs-lookup"><span data-stu-id="37f3d-359">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="37f3d-360">Görev 2 - veritabanı sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="37f3d-360">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="37f3d-361">Uygulamanızı SQL Server'ın kullanmak yaparsa veritabanlarının bir SQL veritabanı sunucusu oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-361">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="37f3d-362">SQL Server kullanmayan basit bir uygulamayı dağıtmak istiyorsanız, bu görevi atlamak.</span><span class="sxs-lookup"><span data-stu-id="37f3d-362">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="37f3d-363">Uygulama veritabanını depolamak için bir SQL veritabanı sunucusu gerekir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-363">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="37f3d-364">Aboneliğiniz Windows Azure Yönetim Portalı'nda SQL veritabanı sunucularının görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-364">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="37f3d-365">Oluşturulan bir sunucu yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğundan düğme.</span><span class="sxs-lookup"><span data-stu-id="37f3d-365">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="37f3d-366">Not edin **sunucu adı ve URL, yönetici oturum açma adı ve parola**sonraki görevleri kullanacağı gibi.</span><span class="sxs-lookup"><span data-stu-id="37f3d-366">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="37f3d-367">Daha sonraki bir aşamada oluşturulacak şekilde veritabanı henüz oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-367">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="37f3d-368">![SQL veritabanı sunucusu Pano](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL veritabanı sunucu Panosu")</span><span class="sxs-lookup"><span data-stu-id="37f3d-368">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="37f3d-369">*SQL veritabanı sunucu Panosu*</span><span class="sxs-lookup"><span data-stu-id="37f3d-369">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="37f3d-370">İşlemin sonraki görev, sunucunun listesinde, yerel IP adresi içermesi gereken bu nedenle veritabanı bağlantısı Visual Studio'dan test edecek **izin verilen IP adreslerini**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-370">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="37f3d-371">Bunu yapmak için tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** üzerinde yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) düğmesi.</span><span class="sxs-lookup"><span data-stu-id="37f3d-371">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![İstemci IP adresi ekleme](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="37f3d-373">*İstemci IP adresi ekleme*</span><span class="sxs-lookup"><span data-stu-id="37f3d-373">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="37f3d-374">Bir kez **istemci IP adresi** izin verilen IP adreslerine eklenen listesinde, tıklayın **kaydetmek** değişiklikleri onaylamak için.</span><span class="sxs-lookup"><span data-stu-id="37f3d-374">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Değişiklikleri onaylamak](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="37f3d-376">*Değişiklikleri onaylamak*</span><span class="sxs-lookup"><span data-stu-id="37f3d-376">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="37f3d-377">Görev 3 - Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="37f3d-377">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="37f3d-378">ASP.NET MVC 4 çözüme geri dönün.</span><span class="sxs-lookup"><span data-stu-id="37f3d-378">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="37f3d-379">İçinde **Çözüm Gezgini**, web sitesi projesine sağ tıklatın ve **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-379">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="37f3d-380">![Uygulama yayımlama](aspnet-mvc-4-custom-action-filters/_static/image29.png "uygulama yayımlama")</span><span class="sxs-lookup"><span data-stu-id="37f3d-380">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="37f3d-381">*Web sitesi yayımlama*</span><span class="sxs-lookup"><span data-stu-id="37f3d-381">*Publishing the web site*</span></span>
2. <span data-ttu-id="37f3d-382">İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-382">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="37f3d-383">![Yayımlama profilini içeri aktarma](aspnet-mvc-4-custom-action-filters/_static/image30.png "yayımlama profilini içeri aktarma")</span><span class="sxs-lookup"><span data-stu-id="37f3d-383">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="37f3d-384">*Yayımlama profilini içeri aktarma*</span><span class="sxs-lookup"><span data-stu-id="37f3d-384">*Importing publish profile*</span></span>
3. <span data-ttu-id="37f3d-385">Tıklatın **bağlantısı doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-385">Click **Validate Connection**.</span></span> <span data-ttu-id="37f3d-386">Doğrulama tamamlandıktan sonra tıklayın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-386">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="37f3d-387">Yanındaki bağlantıyı doğrula düğmesi görünür yeşil bir onay işareti gördüğünüzde doğrulama tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="37f3d-387">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="37f3d-388">![Bağlantı doğrulama](aspnet-mvc-4-custom-action-filters/_static/image31.png "bağlantısı doğrulanıyor")</span><span class="sxs-lookup"><span data-stu-id="37f3d-388">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="37f3d-389">*Doğrulama bağlantısı*</span><span class="sxs-lookup"><span data-stu-id="37f3d-389">*Validating connection*</span></span>
4. <span data-ttu-id="37f3d-390">İçinde **ayarları** sayfasında **veritabanları** bölümünde, veritabanı bağlantının textbox yanındaki düğmesini tıklatın (yani **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="37f3d-390">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="37f3d-391">![Web dağıtımı yapılandırma](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web dağıtımı yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="37f3d-391">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="37f3d-392">*Web dağıtımı yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="37f3d-392">*Web deploy configuration*</span></span>
5. <span data-ttu-id="37f3d-393">Veritabanı bağlantısı aşağıdaki gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="37f3d-393">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="37f3d-394">İçinde **sunucu adı** , SQL veritabanı sunucusu URL'yi kullanarak yazın *tcp:* öneki.</span><span class="sxs-lookup"><span data-stu-id="37f3d-394">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="37f3d-395">İçinde **kullanıcı adı** sunucunuzun yönetici oturum açma adını yazın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-395">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="37f3d-396">İçinde **parola** sunucu yönetici oturum açma parolasını yazın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-396">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="37f3d-397">Yeni bir veritabanı adı yazın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-397">Type a new database name.</span></span>

    <span data-ttu-id="37f3d-398">![Hedef bağlantı dizesi yapılandırma](aspnet-mvc-4-custom-action-filters/_static/image33.png "hedef bağlantı dizesi yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="37f3d-398">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="37f3d-399">*Hedef bağlantı dizesi yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="37f3d-399">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="37f3d-400">Sonra **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-400">Then click **OK**.</span></span> <span data-ttu-id="37f3d-401">Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-401">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="37f3d-402">![Veritabanı oluşturma](aspnet-mvc-4-custom-action-filters/_static/image34.png "veritabanı dizesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="37f3d-402">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="37f3d-403">*Veritabanı oluşturuluyor*</span><span class="sxs-lookup"><span data-stu-id="37f3d-403">*Creating the database*</span></span>
7. <span data-ttu-id="37f3d-404">Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini varsayılan bağlantı textbox içinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-404">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="37f3d-405">Sonra **İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-405">Then click **Next**.</span></span>

    <span data-ttu-id="37f3d-406">![SQL veritabanına işaret eden bağlantı dizesi](aspnet-mvc-4-custom-action-filters/_static/image35.png "SQL veritabanına işaret eden bağlantı dizesi")</span><span class="sxs-lookup"><span data-stu-id="37f3d-406">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="37f3d-407">*SQL veritabanına işaret eden bağlantı dizesi*</span><span class="sxs-lookup"><span data-stu-id="37f3d-407">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="37f3d-408">İçinde **Önizleme** sayfasında, **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-408">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="37f3d-409">![Web uygulaması yayımlama](aspnet-mvc-4-custom-action-filters/_static/image36.png "web uygulaması yayımlama")</span><span class="sxs-lookup"><span data-stu-id="37f3d-409">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="37f3d-410">*Web uygulaması yayımlama*</span><span class="sxs-lookup"><span data-stu-id="37f3d-410">*Publishing the web application*</span></span>
9. <span data-ttu-id="37f3d-411">Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesini açın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-411">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="37f3d-412">Ek C: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="37f3d-412">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="37f3d-413">Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip.</span><span class="sxs-lookup"><span data-stu-id="37f3d-413">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="37f3d-414">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="37f3d-414">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="37f3d-415">![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-custom-action-filters/_static/image37.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="37f3d-415">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="37f3d-416">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="37f3d-416">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="37f3d-417">***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="37f3d-417">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="37f3d-418">İmleci, burada kod eklemek istediğiniz yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="37f3d-418">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="37f3d-419">(Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-419">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="37f3d-420">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="37f3d-420">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="37f3d-421">Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="37f3d-421">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="37f3d-422">İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-422">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="37f3d-423">![Kod parçacığında adını yazmaya başlayın](aspnet-mvc-4-custom-action-filters/_static/image38.png "parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="37f3d-423">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="37f3d-424">*Kod parçacığında adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="37f3d-424">*Start typing the snippet name*</span></span>

<span data-ttu-id="37f3d-425">![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](aspnet-mvc-4-custom-action-filters/_static/image39.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="37f3d-425">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="37f3d-426">*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="37f3d-426">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="37f3d-427">![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](aspnet-mvc-4-custom-action-filters/_static/image40.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")</span><span class="sxs-lookup"><span data-stu-id="37f3d-427">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="37f3d-428">*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*</span><span class="sxs-lookup"><span data-stu-id="37f3d-428">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="37f3d-429">***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1.</span><span class="sxs-lookup"><span data-stu-id="37f3d-429">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="37f3d-430">Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="37f3d-430">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="37f3d-431">Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.</span><span class="sxs-lookup"><span data-stu-id="37f3d-431">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="37f3d-432">Tıklayarak ilgili kod parçacığında listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="37f3d-432">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="37f3d-433">![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](aspnet-mvc-4-custom-action-filters/_static/image41.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")</span><span class="sxs-lookup"><span data-stu-id="37f3d-433">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="37f3d-434">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*</span><span class="sxs-lookup"><span data-stu-id="37f3d-434">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="37f3d-435">![Tıklayarak ilgili kod parçacığında listeden çekme](aspnet-mvc-4-custom-action-filters/_static/image42.png "tıklayarak ilgili kod parçacığında listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="37f3d-435">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="37f3d-436">*Tıklayarak ilgili kod parçacığında listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="37f3d-436">*Pick the relevant snippet from the list, by clicking on it*</span></span>
