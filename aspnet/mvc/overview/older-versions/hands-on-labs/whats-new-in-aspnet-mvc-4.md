---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: ASP.NET MVC 4'te Yenilikler | Microsoft Docs
author: rick-anderson
description: "ASP.NET MVC 4 tanınmış tasarım desenleri ve ASP.NET gücünü kullanarak ölçeklenebilir, standartlara dayalı web uygulamaları oluşturmaya yönelik bir çerçevedir ve..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 8b1bdae048afc78399ccc7b0eac7125d9b983c13
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="9553b-103">ASP.NET MVC 4'te yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="9553b-103">What's New in ASP.NET MVC 4</span></span>
====================
<span data-ttu-id="9553b-104">tarafından [Web Camps ekibi](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="9553b-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="9553b-105">Kit eğitim Web Camps indirin</span><span class="sxs-lookup"><span data-stu-id="9553b-105">Download Web Camps Training Kit</span></span>](http://www.microsoft.com/download/29843)

> <span data-ttu-id="9553b-106">ASP.NET MVC 4 tanınmış tasarım desenleri ve ASP.NET ve .NET framework gücünü kullanarak ölçeklenebilir, standartlara dayalı web uygulamaları oluşturmaya yönelik bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="9553b-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="9553b-107">Bu yeni, mobil web uygulaması geliştirme daha kolay hale dördüncü framework sürümünü odaklanır.</span><span class="sxs-lookup"><span data-stu-id="9553b-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>
> 
> <span data-ttu-id="9553b-108">Başından itibaren yeni bir ASP.NET MVC 4 proje oluşturduğunuzda var. Şimdi mobil cihazlar için özellikle bir tek başına uygulamasını oluşturmak için kullanabileceğiniz bir mobil uygulama proje şablonu</span><span class="sxs-lookup"><span data-stu-id="9553b-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="9553b-109">Ayrıca, ASP.NET MVC 4 jQuery Mobile jQuery.Mobile.MVC NuGet paketi ile tümleşir.</span><span class="sxs-lookup"><span data-stu-id="9553b-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="9553b-110">jQuery Mobile, Windows Phone, iPhone, Android vb. de dahil olmak üzere tüm popüler mobil cihaz platformları ile uyumlu olan web uygulamaları geliştirmek için bir HTML5 tabanlı çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="9553b-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="9553b-111">Uzmanlık ihtiyacınız varsa, ancak, ASP.NET MVC 4 ayrıca farklı görünümleri farklı aygıtlar için hizmet ve cihaza özgü iyileştirmeler sağlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="9553b-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>
> 
> <span data-ttu-id="9553b-112">Uygulamalı bu laboratuvarda, ASP.NET MVC 4 ile başlar &quot;Internet uygulama&quot; bir Fotoğraf Galerisi uygulaması oluşturmak için proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="9553b-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="9553b-113">Farklı mobil cihaz ve masaüstü web tarayıcıları ile uyumlu hale getirmek için jQuery Mobile ve ASP.NET MVC 4'ın yeni özellikleri kullanarak uygulamayı aşamalı olarak artırır.</span><span class="sxs-lookup"><span data-stu-id="9553b-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="9553b-114">Kod oluşturma ve nasıl ASP.NET MVC 4 görev destekleyerek zaman uyumsuz eylem yöntemleri yazmanızı kolaylaştırır için yeni kod tarif hakkında bilgi edineceksiniz&lt;ActionResult&gt; dönüş türü.</span><span class="sxs-lookup"><span data-stu-id="9553b-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>
> 
> <span data-ttu-id="9553b-115">Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="9553b-115">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="9553b-116">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="9553b-116">Objectives</span></span>

<span data-ttu-id="9553b-117">Uygulamalı bu laboratuvarda, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="9553b-117">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="9553b-118">ASP.NET MVC proje şablonları-dahil olmak üzere yeni mobil uygulama proje şablonu geliştirmeler yararlanın</span><span class="sxs-lookup"><span data-stu-id="9553b-118">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="9553b-119">HTML5 görünüm penceresinin özniteliği ve CSS medya sorguları mobil cihazlarda görünen geliştirmek için kullanın</span><span class="sxs-lookup"><span data-stu-id="9553b-119">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="9553b-120">JQuery Mobile aşamalı geliştirmeleri ve dokunma özelliği iyileştirilmiş web kullanıcı Arabirimi oluşturmak için kullanın</span><span class="sxs-lookup"><span data-stu-id="9553b-120">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="9553b-121">Mobile özgü görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="9553b-121">Create mobile-specific views</span></span>
- <span data-ttu-id="9553b-122">Görünüm değiştirici bileşeni uygulamasında mobil ve Masaüstü görünümleri arasında geçiş yapmak için kullanın</span><span class="sxs-lookup"><span data-stu-id="9553b-122">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="9553b-123">Görev desteğini kullanarak zaman uyumsuz denetleyicileri oluşturun</span><span class="sxs-lookup"><span data-stu-id="9553b-123">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="9553b-124">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="9553b-124">Prerequisites</span></span>

<span data-ttu-id="9553b-125">Bu laboratuvarı tamamlamak için aşağıdaki öğeleri sahip olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="9553b-125">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="9553b-126">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya daha üstün (okuma [ek B](#AppendixB) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="9553b-126">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="9553b-127">[ASP.NET MVC 4](../../../mvc4.md) (Microsoft Visual Studio 2012 yüklemesine dahil)</span><span class="sxs-lookup"><span data-stu-id="9553b-127">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="9553b-128">Windows Phone öykünücüsü (dahil [7.1.1 Windows Phone SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="9553b-128">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="9553b-129">İsteğe bağlı - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) ile **Electric Plum iPhone benzeticisi** uzantısına (yalnızca web uygulaması ile bir iPhone benzeticisi göz atmak için kullanılan alıştırma 3)</span><span class="sxs-lookup"><span data-stu-id="9553b-129">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="9553b-130">Kurulum</span><span class="sxs-lookup"><span data-stu-id="9553b-130">Setup</span></span>

<span data-ttu-id="9553b-131">Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="9553b-132">Size kolaylık sağlamak için Visual Studio kod Visual Studio içinde el ile eklemek zorunda kalmamak için kullanabileceğiniz parçacıkları, bu kodu çoğu sağlanır.</span><span class="sxs-lookup"><span data-stu-id="9553b-132">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="9553b-133">Kod parçacıkları yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="9553b-133">To install the code snippets:</span></span>

1. <span data-ttu-id="9553b-134">Bir Windows Explorer penceresi açın ve Gözat Laboratuvar için 's **Source\Setup** klasör.</span><span class="sxs-lookup"><span data-stu-id="9553b-134">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="9553b-135">Çift **Setup.cmd** Visual Studio kod parçacıkları yüklemek için bu klasörde dosya.</span><span class="sxs-lookup"><span data-stu-id="9553b-135">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="9553b-136">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek A: kullanarak kod parçacıkları](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="9553b-136">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="9553b-137">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="9553b-137">Exercises</span></span>

<span data-ttu-id="9553b-138">Bu uygulamalı Laboratuvar aşağıdaki alıştırmaları içerir:</span><span class="sxs-lookup"><span data-stu-id="9553b-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="9553b-139">Yeni ASP.NET MVC 4 proje şablonları</span><span class="sxs-lookup"><span data-stu-id="9553b-139">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="9553b-140">Fotoğraf Galerisi Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="9553b-140">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="9553b-141">Mobil cihaz desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="9553b-141">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="9553b-142">Zaman uyumsuz denetleyicileri kullanma</span><span class="sxs-lookup"><span data-stu-id="9553b-142">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="9553b-143">Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="9553b-143">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="9553b-144">Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-144">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="9553b-145">Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**.</span><span class="sxs-lookup"><span data-stu-id="9553b-145">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="9553b-146">Alıştırma 1: Yeni ASP.NET MVC 4 proje şablonları</span><span class="sxs-lookup"><span data-stu-id="9553b-146">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="9553b-147">Bu alıştırmada, ASP.NET MVC 4 proje şablonları geliştirmeleri inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-147">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="9553b-148">Internet uygulaması şablonu ek olarak, MVC 3'te zaten mevcut bu sürümü artık mobil uygulamalar için ayrı bir şablon içerir.</span><span class="sxs-lookup"><span data-stu-id="9553b-148">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="9553b-149">İlk olarak, her bir şablonlar ilgili bazı özellikleri arar.</span><span class="sxs-lookup"><span data-stu-id="9553b-149">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="9553b-150">Ardından, doğru yaklaşımı kullanarak sayfanızı farklı platformlarda düzgün işleme çalışır.</span><span class="sxs-lookup"><span data-stu-id="9553b-150">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="9553b-151">Görev 1 - Internet uygulama şablonu keşfetme</span><span class="sxs-lookup"><span data-stu-id="9553b-151">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="9553b-152">Açık **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="9553b-152">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="9553b-153">Seçin **dosya | Yeni | Proje** menü komutu.</span><span class="sxs-lookup"><span data-stu-id="9553b-153">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="9553b-154">İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** şablonu sol bölmesindeki ağaç ve seçin **ASP.NET MVC 4 Web uygulaması.**</span><span class="sxs-lookup"><span data-stu-id="9553b-154">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="9553b-155">Proje adı **Fotografgalerisi**, bir konum seçin (veya varsayılan adı bırakın) tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="9553b-155">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9553b-156">Daha sonra artık oluşturmakta olduğunuz Fotografgalerisi ASP.NET MVC 4 çözümü özelleştirme.</span><span class="sxs-lookup"><span data-stu-id="9553b-156">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="9553b-157">![Yeni proje oluşturma](whats-new-in-aspnet-mvc-4/_static/image1.png "yeni proje oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9553b-157">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="9553b-158">*Yeni proje oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9553b-158">*Creating a new project*</span></span>
3. <span data-ttu-id="9553b-159">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulama** proje şablonu ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="9553b-159">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="9553b-160">Razor görüntüleme altyapısı seçtiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="9553b-160">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="9553b-161">![Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma](whats-new-in-aspnet-mvc-4/_static/image2.png "yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9553b-161">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="9553b-162">*Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9553b-162">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9553b-163">Razor sözdizimi ASP.NET MVC 3'te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="9553b-163">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="9553b-164">Karakterler ve gerekli bir dosya bir hızlı ve iş akışı kodlama sıvı etkinleştirme tuş vuruşları sayısını en aza indirmek için kendi hedeftir.</span><span class="sxs-lookup"><span data-stu-id="9553b-164">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="9553b-165">Razor yararlanır varolan C# / VB (veya diğer) dil becerileri ve harika bir HTML oluşturma iş akışı sağlayan bir şablon biçimlendirme sözdizimi sunar.</span><span class="sxs-lookup"><span data-stu-id="9553b-165">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="9553b-166">Tuşuna **F5** çözümü çalıştırın ve yenilenen şablonları görmek için.</span><span class="sxs-lookup"><span data-stu-id="9553b-166">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="9553b-167">Aşağıdaki özellikleri denetleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9553b-167">You can check out the following features:</span></span>

    <span data-ttu-id="9553b-168">**Modern stili şablonları**</span><span class="sxs-lookup"><span data-stu-id="9553b-168">**Modern-style templates**</span></span>

    <span data-ttu-id="9553b-169">Şablonlar, daha fazla modern görünümlü stilleri sağlama yenilenmiş.</span><span class="sxs-lookup"><span data-stu-id="9553b-169">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="9553b-170">![ASP.NET MVC 4 stili şablonları](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC şablonları Stili 4")</span><span class="sxs-lookup"><span data-stu-id="9553b-170">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="9553b-171">*ASP.NET MVC 4 stili şablonları*</span><span class="sxs-lookup"><span data-stu-id="9553b-171">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="9553b-172">![Yeni bir kişi sayfa](whats-new-in-aspnet-mvc-4/_static/image4.png "yeni kişi sayfası")</span><span class="sxs-lookup"><span data-stu-id="9553b-172">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="9553b-173">*Yeni ilgili kişi sayfası*</span><span class="sxs-lookup"><span data-stu-id="9553b-173">*New Contact page*</span></span>

    <span data-ttu-id="9553b-174">**Uyarlamalı işleme**</span><span class="sxs-lookup"><span data-stu-id="9553b-174">**Adaptive Rendering**</span></span>

    <span data-ttu-id="9553b-175">Tarayıcı penceresini yeniden boyutlandırma çıkışı denetleyin ve nasıl sayfa düzeni yeni pencere boyutunu dinamik olarak uyum dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-175">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="9553b-176">Bu şablonlar, hem Masaüstü hem de mobil platformları hiçbir özelleştirme gerektirmeden düzgün işlenecek Uyarlamalı işleme tekniği kullanın.</span><span class="sxs-lookup"><span data-stu-id="9553b-176">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="9553b-177">![ASP.NET MVC 4 proje şablonu farklı tarayıcı boyutlarda](whats-new-in-aspnet-mvc-4/_static/image5.png "farklı tarayıcı boyutlarda ASP.NET MVC 4 proje şablonu")</span><span class="sxs-lookup"><span data-stu-id="9553b-177">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="9553b-178">*ASP.NET MVC 4 proje şablonu farklı tarayıcı boyutlarda*</span><span class="sxs-lookup"><span data-stu-id="9553b-178">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="9553b-179">**JavaScript ile zengin kullanıcı Arabirimi**</span><span class="sxs-lookup"><span data-stu-id="9553b-179">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="9553b-180">Varsayılan proje şablonları için başka bir geliştirme JavaScript daha etkileşimli JavaScript sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9553b-180">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="9553b-181">Şablonda kullanılan oturum açma ve kaydetme bağlantıları doğrulamaları jQuery istemci-tarafı giriş alanları doğrulamak için nasıl kullanılacağını exemplify.</span><span class="sxs-lookup"><span data-stu-id="9553b-181">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![jQuery doğrulama](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="9553b-183">*jQuery Validation*</span><span class="sxs-lookup"><span data-stu-id="9553b-183">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9553b-184">Bildirim, bölümlerinde, ilk bölüm iki oturum sitesinden kayıtlı hesabı kullanarak ve google (varsayılan olarak devre dışı) gibi başka bir kimlik doğrulama hizmeti kullanarak altenativelly oturum yapabilecekleriniz ikinci kısmında oturum açabilir.</span><span class="sxs-lookup"><span data-stu-id="9553b-184">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="9553b-185">Hata ayıklayıcıyı durdurduktan ve Visual Studio'ya dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="9553b-185">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="9553b-186">Dosyayı açmak **AuthConfig.cs** altında bulunan **uygulama\_Başlat** klasör.</span><span class="sxs-lookup"><span data-stu-id="9553b-186">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="9553b-187">Google istemci kaydetmek için son satırından açıklamayı Kaldır *OAuth* kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="9553b-187">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > <span data-ttu-id="9553b-188">Facebook, Twitter, Microsoft, vb. gibi tüm Openıd veya OAuth hizmeti kullanarak kimlik doğrulamasını kolayca etkinleştirebilirsiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-188">Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.</span></span>
8. <span data-ttu-id="9553b-189">Tuşuna **F5** çözümü çalıştırın ve oturum açma sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="9553b-189">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="9553b-190">Seçin **Google** oturum açmak için hizmet.</span><span class="sxs-lookup"><span data-stu-id="9553b-190">Select **Google** service to log in.</span></span>

    ![Günlük hizmetinde seçme](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="9553b-192">*Günlük hizmetinde seçme*</span><span class="sxs-lookup"><span data-stu-id="9553b-192">*Selecting the log in service*</span></span>
10. <span data-ttu-id="9553b-193">Google hesabınızı kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="9553b-193">Log in using your Google account.</span></span>
11. <span data-ttu-id="9553b-194">Google hesabı bilgilerini almak site (localhost) izin verin.</span><span class="sxs-lookup"><span data-stu-id="9553b-194">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="9553b-195">Son olarak, Google hesabıyla ilişkilendirmek için siteyi kaydettirmeniz gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="9553b-195">Finally, you will have to register in the site to associate the Google account.</span></span>

    ![Google hesabınız ilişkilendirme](whats-new-in-aspnet-mvc-4/_static/image8.png)

    <span data-ttu-id="9553b-197">*Google hesabınız ilişkilendirme*</span><span class="sxs-lookup"><span data-stu-id="9553b-197">*Associating your Google account*</span></span>
13. <span data-ttu-id="9553b-198">Hata ayıklayıcıyı durdurduktan ve Visual Studio'ya dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="9553b-198">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="9553b-199">Şimdi proje şablonu ASP.NET MVC 4 tarafından sunulan diğer bazı yeni özellikler kullanıma için çözüm keşfedin.</span><span class="sxs-lookup"><span data-stu-id="9553b-199">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="9553b-200">![ASP.NET MVC 4 Internet uygulaması proje şablonu](whats-new-in-aspnet-mvc-4/_static/image9.png "ASP.NET MVC 4 Internet uygulaması proje şablonu")</span><span class="sxs-lookup"><span data-stu-id="9553b-200">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="9553b-201">*ASP.NET MVC 4 Internet uygulaması proje şablonu*</span><span class="sxs-lookup"><span data-stu-id="9553b-201">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

    - <span data-ttu-id="9553b-202">**HTML 5 biçimlendirme**</span><span class="sxs-lookup"><span data-stu-id="9553b-202">**HTML 5 Markup**</span></span>

        <span data-ttu-id="9553b-203">Yeni temayı biçimlendirme bulmak için şablon görünümleri göz atın.</span><span class="sxs-lookup"><span data-stu-id="9553b-203">Browse template views to find out the new theme markup.</span></span>

        <span data-ttu-id="9553b-204">![Razor ve HTML5 biçimlendirme About.cshtml kullanarak yeni şablonu. ] (whats-new-in-aspnet-mvc-4/_static/image10.png "Razor ve HTML5 biçimlendirme About.cshtml kullanarak yeni şablonu.")</span><span class="sxs-lookup"><span data-stu-id="9553b-204">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

        <span data-ttu-id="9553b-205">*Razor ve HTML5 biçimlendirme (About.cshtml) kullanarak yeni şablonu.*</span><span class="sxs-lookup"><span data-stu-id="9553b-205">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
    - <span data-ttu-id="9553b-206">**Güncelleştirilmiş JavaScript kitaplıkları**</span><span class="sxs-lookup"><span data-stu-id="9553b-206">**Updated JavaScript libraries**</span></span>

        <span data-ttu-id="9553b-207">ASP.NET MVC 4 varsayılan şablonu artık Çakıştırmaları, zengin oluşturmanıza olanak sağlayan bir JavaScript MVVM framework ve JavaScript ve HTML kullanarak hızlı yanıt veren web uygulamaları içerir.</span><span class="sxs-lookup"><span data-stu-id="9553b-207">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="9553b-208">Gibi MVC3 jQuery ve jQuery UI kitaplıkları da ASP.NET MVC 4'te dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="9553b-208">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

        > [!NOTE]
        > <span data-ttu-id="9553b-209">Bu bağlantıyı Çakıştırmaları kitaplıkta hakkında daha fazla bilgi edinebilirsiniz: [ [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="9553b-209">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="9553b-210">Ayrıca, jQuery ve jQuery UI hakkında öğrenebilirsiniz içinde [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="9553b-210">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="9553b-211">Görev 2 - mobil uygulama şablonu keşfetme</span><span class="sxs-lookup"><span data-stu-id="9553b-211">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="9553b-212">ASP.NET MVC 4 mobil için Web siteleri ve tablet tarayıcılar geliştirilmesini kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="9553b-212">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="9553b-213">Bu şablon Internet uygulama şablonu (bildirim denetleyicisi kodu hemen hemen aynıdır) aynı uygulama yapısını var, ancak kendi stil dokunma tabanlı mobil cihazlarda düzgün işlenecek değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="9553b-213">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="9553b-214">Seçin **dosya | Yeni | Proje** menü komutu.</span><span class="sxs-lookup"><span data-stu-id="9553b-214">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="9553b-215">İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** şablonu sol bölmesindeki ağaç ve seçin **ASP.NET MVC 4 Web uygulaması.**</span><span class="sxs-lookup"><span data-stu-id="9553b-215">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="9553b-216">Proje adı **PhotoGallery.Mobile**, bir konum seçin (veya varsayılan adı bırakın), seçin &quot;eklemek için çözüm&quot; tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="9553b-216">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="9553b-217">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **mobil uygulama** proje şablonu ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="9553b-217">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="9553b-218">Razor görüntüleme altyapısı seçtiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="9553b-218">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="9553b-219">![Yeni bir ASP.NET MVC 4 mobil uygulaması oluşturma](whats-new-in-aspnet-mvc-4/_static/image11.png "yeni bir ASP.NET MVC 4 mobil uygulaması oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9553b-219">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="9553b-220">*Yeni bir ASP.NET MVC 4 mobil uygulaması oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9553b-220">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="9553b-221">Şimdi çözümü keşfedin ve mobile için ASP.NET MVC 4 çözüm şablonu tarafından sunulan yeni özelliklerden bazıları göz atın:</span><span class="sxs-lookup"><span data-stu-id="9553b-221">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="9553b-222">**jQuery Mobile Library**</span><span class="sxs-lookup"><span data-stu-id="9553b-222">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="9553b-223">Mobil uygulama proje şablonu mobil tarayıcı uyumluluğu için bir açık kaynak kitaplığı jQuery mobil kitaplık içerir.</span><span class="sxs-lookup"><span data-stu-id="9553b-223">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="9553b-224">jQuery Mobile kademeli geliştirmeyi CSS ve JavaScript destekleyen mobil tarayıcılar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="9553b-224">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="9553b-225">Kademeli geliştirmeyi yalnızca Zengin içeriği görüntülemek en güçlü tarayıcılar olanak sağlarken temel bir web sayfası içeriğini görüntülemek tüm tarayıcılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="9553b-225">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="9553b-226">JQuery Mobile stili dahil JavaScript ve CSS dosyaları sayfası biçimlendirme içinde herhangi bir değişiklik yapmadan ekranında içeriğin sığması için mobil tarayıcılar yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="9553b-226">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="9553b-228">*şablona dahil jQuery mobil kitaplığı*</span><span class="sxs-lookup"><span data-stu-id="9553b-228">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="9553b-229">**Temel HTML5 biçimlendirme**</span><span class="sxs-lookup"><span data-stu-id="9553b-229">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="9553b-231">*HTML5 biçimlendirme, (Login.cshtml ve Index.cshtml) kullanılarak mobil uygulama şablonu*</span><span class="sxs-lookup"><span data-stu-id="9553b-231">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="9553b-232">Tuşuna **F5** çözümü çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9553b-232">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="9553b-233">Açık **Windows Phone 7 öykünücüsü**.</span><span class="sxs-lookup"><span data-stu-id="9553b-233">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="9553b-234">Telefon başlangıç ekranında da Internet Explorer'ı açın.</span><span class="sxs-lookup"><span data-stu-id="9553b-234">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="9553b-235">Masaüstü uygulaması başlatıldığı URL denetleyin ve bu URL'sine gidin telefonunuzdan (örneğin `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="9553b-235">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="9553b-236">Oturum açma sayfasına girin ya da kullanıma artık sayfa hakkında.</span><span class="sxs-lookup"><span data-stu-id="9553b-236">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="9553b-237">Mobil için yeni Metro uygulamasını Web sitesinin stil dayalı dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-237">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="9553b-238">ASP.NET MVC 4 proje şablonu, sayfanın tüm öğeleri görünür ve etkin emin mobil cihazlarda düzgün görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="9553b-238">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="9553b-239">Üstbilgi bağlantıları tıklattınız veya dokunduğunuz büyük olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-239">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="9553b-240">![Proje şablonunu bir mobil cihaz sayfalarında](whats-new-in-aspnet-mvc-4/_static/image14.png "proje şablonu sayfalarında bir mobil cihaz")</span><span class="sxs-lookup"><span data-stu-id="9553b-240">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="9553b-241">*Mobil aygıtta proje şablonu sayfaları*</span><span class="sxs-lookup"><span data-stu-id="9553b-241">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="9553b-242">Yeni şablon de kullanır **görünüm penceresinin meta etiketi**.</span><span class="sxs-lookup"><span data-stu-id="9553b-242">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="9553b-243">Çoğu mobil tarayıcılar sanal tarayıcı penceresi için bir genişliği tanımlayın veya &quot;görünüm penceresinin&quot;, mobil cihaz gerçek genişliğinden daha büyük olduğu.</span><span class="sxs-lookup"><span data-stu-id="9553b-243">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="9553b-244">Bu sanal görüntü içindeki tüm web sayfasını görüntülemek mobil tarayıcılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="9553b-244">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="9553b-245">**Görünüm penceresinin meta etiketi** genişlik, yükseklik ve tarayıcı alanı ölçeğini mobil cihazlarda ayarlamak web geliştiricileri sağlayan **.**</span><span class="sxs-lookup"><span data-stu-id="9553b-245">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="9553b-246">ASP.NET MVC 4 şablon mobil uygulamalar için cihaz genişliğine görünüm penceresinin ayarlar (&quot;genişliği aygıt-width =&quot;) düzeni şablondaki (*görünümler/paylaşılan\_Layout.cshtml*), böylece tüm sayfalar, cihaz ekran genişliği kendi görünüm penceresinin sahip olur.</span><span class="sxs-lookup"><span data-stu-id="9553b-246">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="9553b-247">Görünüm penceresinin meta etiketi varsayılan tarayıcı görünümü değiştirmez dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-247">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="9553b-248">Açık  **\_Layout.cshtml**, bulunan **görünümleri | Paylaşılan** klasörü ve yorum görünüm penceresinin meta etiketi.</span><span class="sxs-lookup"><span data-stu-id="9553b-248">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="9553b-249">Uygulamayı çalıştırın yoksa zaten açılmış ve farklılıkları denetleyin.</span><span class="sxs-lookup"><span data-stu-id="9553b-249">Run the application, if not already opened, and check out the differences.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

    <span data-ttu-id="9553b-250">![Görünüm penceresinin meta etiketi yorum sonra site](whats-new-in-aspnet-mvc-4/_static/image15.png "görünüm penceresinin meta etiketi yorum sonra site")</span><span class="sxs-lookup"><span data-stu-id="9553b-250">![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")</span></span>

    <span data-ttu-id="9553b-251">*Görünüm penceresinin meta etiketi yorum sonra site*</span><span class="sxs-lookup"><span data-stu-id="9553b-251">*The site after commenting the viewport meta tag*</span></span>
10. <span data-ttu-id="9553b-252">Visual Studio'da basın **SHIFT** + **F5** uygulamanın hata ayıklamasını durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="9553b-252">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="9553b-253">Görünüm penceresinin meta etiketi açıklamadan çıkarın.</span><span class="sxs-lookup"><span data-stu-id="9553b-253">Uncomment the viewport meta tag.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="9553b-254">Görev 3 - Uyarlamalı işleme kullanma</span><span class="sxs-lookup"><span data-stu-id="9553b-254">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="9553b-255">Bu görevde, hiçbir özelleştirme gerektirmeden aynı anda bir Web sayfasında doğru mobil cihazlar ve Web tarayıcıları işlemek için başka bir yöntem öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-255">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="9553b-256">Görünüm penceresinin meta etiketi benzer amacı ile zaten kullandınız.</span><span class="sxs-lookup"><span data-stu-id="9553b-256">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="9553b-257">Başka bir güçlü yöntemi karşılayacağını artık: *Uyarlamalı işleme*.</span><span class="sxs-lookup"><span data-stu-id="9553b-257">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="9553b-258">Uyarlamalı işleme kullanan bir teknik olduğu **CSS3 media sorguları** bir sayfaya uygulanan stil özelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="9553b-258">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="9553b-259">Medya sorguları, belirli bir koşul altında CSS stilleri gruplandırma bir stil sayfası içinden koşulları tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="9553b-259">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="9553b-260">Yalnızca koşulun true olduğunda, stil bildirilen nesnelere uygulanır.</span><span class="sxs-lookup"><span data-stu-id="9553b-260">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="9553b-261">Farklı cihazlarda site görüntüleme için hiçbir özelleştirme Uyarlamalı işleme tekniği tarafından sağlanan esneklik sağlar.</span><span class="sxs-lookup"><span data-stu-id="9553b-261">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="9553b-262">Tek bir stil sayfasında mantığı kod yazmadan stili seçmek için istediğiniz sayıda stilleri tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-262">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="9553b-263">Yinelenen kodu ve amacıyla işleme mantığı miktarını azaltır bu nedenle, sayfa stilleri uyarlama çok düzgün bir şekilde olur.</span><span class="sxs-lookup"><span data-stu-id="9553b-263">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="9553b-264">CSS dosyalarınızın boyutunu fazladır büyüdükçe diğer yandan, bant genişliği tüketimi, artırır.</span><span class="sxs-lookup"><span data-stu-id="9553b-264">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="9553b-265">Uyarlamalı işleme yöntemi kullanarak, sitenizi olacaktır **düzgün bir şekilde, tarayıcı bağımsız olarak görüntülenir.**</span><span class="sxs-lookup"><span data-stu-id="9553b-265">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="9553b-266">Ancak, bant genişliği ek yük ise ilgili bir sorun düşünmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-266">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="9553b-267">Bir ortam sorgusu temel biçimi: @media \[kapsamı: tüm | el | yazdırma | projeksiyon | ekran\] ([özellik: değer] ve... [özellik: değer])</span><span class="sxs-lookup"><span data-stu-id="9553b-267">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="9553b-268">Medya sorguları örnekleri: &gt;  **@media tüm ve (en fazla genişlik: 1000px) ve (min-width: 700px) {}:** 700px 1000px arasındaki tüm çözümler için.</span><span class="sxs-lookup"><span data-stu-id="9553b-268">Examples of media queries: &gt;**@media all and (max-width: 1000px) and (min-width: 700px) {}:** For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="9553b-269">**@mediaekran ve (min-width: 400px) ve (en fazla genişlik: 700px) {...}:** yalnızca ekranlar için.</span><span class="sxs-lookup"><span data-stu-id="9553b-269">**@media screen and (min-width: 400px) and (max-width: 700px) { ... }:** Only for screens.</span></span> <span data-ttu-id="9553b-270">Çözünürlüğü 400 ile 700px arasında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9553b-270">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="9553b-271">**@mediataşınabilir ve (min-width: 20em), ekran ve (min-width: 20em) {...}:** el bilgisayarlarında çalışmak (Mobil ve aygıtlar) ve ekranlar için.</span><span class="sxs-lookup"><span data-stu-id="9553b-271">**@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:** For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="9553b-272">En küçük genişliği 20em büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="9553b-272">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="9553b-273">Bu konu hakkında daha fazla bilgi bulabilirsiniz [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span><span class="sxs-lookup"><span data-stu-id="9553b-273">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="9553b-274">ASP.NET MVC okunabilirliğini artırmak 4 varsayılan Web sitesi şablonu, Uyarlamalı işleme nasıl çalıştığını şimdi keşfedin.</span><span class="sxs-lookup"><span data-stu-id="9553b-274">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="9553b-275">Açık **PhotoGallery.sln** görev 1'den oluşturdunuz ve seçin çözümü **Fotografgalerisi** projesi.</span><span class="sxs-lookup"><span data-stu-id="9553b-275">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="9553b-276">Tuşuna **F5** çözümü çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9553b-276">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="9553b-277">Tarayıcının genişlik, yarısı veya değerinden özgün boyutuna çeyreği windows ayarı yeniden boyutlandırın.</span><span class="sxs-lookup"><span data-stu-id="9553b-277">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="9553b-278">Üstbilgi öğeleri ile neler dikkat edin: bazı öğeler üstbilgi görünür alanında görünmez.</span><span class="sxs-lookup"><span data-stu-id="9553b-278">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="9553b-279">Açık **Site.css** dosya bulunan Visual Studio Çözüm Gezgini'nden **içerik** proje klasörü.</span><span class="sxs-lookup"><span data-stu-id="9553b-279">Open **Site.css** file from the Visual Studio Solution explorer, located in **Content** project folder.</span></span> <span data-ttu-id="9553b-280">Tuşuna **CTRL + F** Visual Studio tümleşik arama açın ve yazmak için  **@media**  bulmak için **CSS ortam sorgusu**.</span><span class="sxs-lookup"><span data-stu-id="9553b-280">Press **CTRL + F** to open Visual Studio integrated search, and write **@media** to locate the **CSS media query**.</span></span>

    <span data-ttu-id="9553b-281">Bu şablonda tanımlanan medya sorgu koşulu bu şekilde çalışır: tarayıcının pencere boyutu olduğunda aşağıda **850 piksel**, uygulanan CSS kuralları bu ortam bloğu içinde tanımlanan olanlardır.</span><span class="sxs-lookup"><span data-stu-id="9553b-281">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="9553b-282">![Ortam sorgusu bulma](whats-new-in-aspnet-mvc-4/_static/image16.png "ortam sorgusu bulma")</span><span class="sxs-lookup"><span data-stu-id="9553b-282">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="9553b-283">*Ortam sorgusu bulma*</span><span class="sxs-lookup"><span data-stu-id="9553b-283">*Locating the media query*</span></span>
4. <span data-ttu-id="9553b-284">İçinde 850 ayarlamak en büyük genişliği öznitelik değeri değiştirin piksel ile **10px**, Uyarlamalı işleme devre dışı bırakmak için ve basın **CTRL + S** değişiklikleri kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="9553b-284">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="9553b-285">Dönüş basın ve tarayıcı **CTRL + F5** yapmış olduğunuz değişiklikleri sayfasıyla yenilenecek.</span><span class="sxs-lookup"><span data-stu-id="9553b-285">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="9553b-286">Her iki sayfa farklılıkları penceresinin genişliğini ayarlarken dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-286">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="9553b-287">![Sayfanın sol, uygulama @media sağ, stil stili atlanırsa](whats-new-in-aspnet-mvc-4/_static/image17.png "sayfanın sol, uygulama @media sağ, stil stili atlanırsa")</span><span class="sxs-lookup"><span data-stu-id="9553b-287">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="9553b-288">*Sayfanın sol, uygulama @media sağ, stil stili atlanırsa*</span><span class="sxs-lookup"><span data-stu-id="9553b-288">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="9553b-289">Şimdi, şirketinizdeki mobil cihazlarda olanlar çıkışı denetleyin:</span><span class="sxs-lookup"><span data-stu-id="9553b-289">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="9553b-290">![Sayfanın sol, uygulama @media sağ, stil stili atlanırsa](whats-new-in-aspnet-mvc-4/_static/image18.png "sayfanın sol, uygulama @media sağ, stil stili atlanırsa")</span><span class="sxs-lookup"><span data-stu-id="9553b-290">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="9553b-291">*Sayfanın sol, uygulama @media sağ, stil stili atlanırsa*</span><span class="sxs-lookup"><span data-stu-id="9553b-291">*In the left, the page is applying the @media style, in the right, the style is omitted*</span></span>

    <span data-ttu-id="9553b-292">Sayfa bir Web tarayıcısında işlendiğinde değişiklikleri bir mobil aygıtı kullanırken çok önemli olmadığını göreceksiniz rağmen farklar daha belirgin hale gelir.</span><span class="sxs-lookup"><span data-stu-id="9553b-292">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="9553b-293">Görüntünün sol tarafındaki özel stili okunabilirlik geliştirilmiş görebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-293">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="9553b-294">Uyarlamalı işleme koşullu bir Web sitesi için stil oluşturma ve masaüstünüzdeki bir yaklaşım ile ilgili ortak stil sorunları çözme uygulamak daha kolay pek çok daha fazla senaryolarda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9553b-294">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="9553b-295">Bu özelliklerden herhangi bir web uygulamasında olabilmesi için görünüm penceresinin meta etiketi ve CSS medya sorgular için ASP.NET MVC 4, özel değildir.</span><span class="sxs-lookup"><span data-stu-id="9553b-295">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="9553b-296">Visual Studio'da basın **SHIFT** + **F5** uygulamanın hata ayıklamasını durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="9553b-296">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="9553b-297">Alıştırma 2: Fotoğraf Galerisi Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="9553b-297">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="9553b-298">Bu alıştırmada, fotoğraf görüntülemek için bir Fotoğraf Galerisi uygulaması üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="9553b-298">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="9553b-299">ASP.NET MVC 4 proje şablonu ile başlar ve ardından, bir hizmetinden fotoğraf almak ve bunları giriş sayfası görüntülemek için bir özellik ekler.</span><span class="sxs-lookup"><span data-stu-id="9553b-299">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="9553b-300">Aşağıdaki alıştırmada mobil cihazlarda görüntülenme şeklini artırmak için bu çözümün güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="9553b-300">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="9553b-301">Görev 1 - sahte fotoğraf hizmet oluşturma</span><span class="sxs-lookup"><span data-stu-id="9553b-301">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="9553b-302">Bu görevde mock galeride gösterilecek içeriği almak için fotoğraf hizmetinin oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9553b-302">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="9553b-303">Bunu yapmak için sadece bir JSON dosyası her fotoğraf verilerle döndürülecek yeni bir denetleyici ekleyecek.</span><span class="sxs-lookup"><span data-stu-id="9553b-303">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="9553b-304">Açık **Visual Studio** zaten açtıysanız.</span><span class="sxs-lookup"><span data-stu-id="9553b-304">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="9553b-305">Seçin **dosya | Yeni | Proje** menü komutu.</span><span class="sxs-lookup"><span data-stu-id="9553b-305">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="9553b-306">İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** şablonu sol bölmesindeki ağaç ve seçin **ASP.NET MVC 4 Web uygulaması.**</span><span class="sxs-lookup"><span data-stu-id="9553b-306">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="9553b-307">Proje adı **Fotografgalerisi**, bir konum seçin (veya varsayılan adı bırakın) tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="9553b-307">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="9553b-308">Alternatif olarak, var olan ASP.NET MVC 4'ten çalışmaya devam edebilirsiniz **Internet uygulama** çözümden **alıştırma 1** ve bir sonraki adımı atlayın.</span><span class="sxs-lookup"><span data-stu-id="9553b-308">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="9553b-309">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulama** proje şablonu ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="9553b-309">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="9553b-310">Razor görüntüleme altyapısı seçili olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="9553b-310">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="9553b-311">İçinde **Çözüm Gezgini**, sağ **uygulama\_veri** klasörü seçin ve proje **Ekle | Varolan öğeyi**.</span><span class="sxs-lookup"><span data-stu-id="9553b-311">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="9553b-312">Gözat **Source\Assets\App\_veri** bu laboratuvarı klasör ekleyin **Photos.json** dosya.</span><span class="sxs-lookup"><span data-stu-id="9553b-312">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="9553b-313">Adlı yeni bir denetleyicisi oluşturun **PhotoController**.</span><span class="sxs-lookup"><span data-stu-id="9553b-313">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="9553b-314">Bunu yapmak için sağ **denetleyicileri** klasörüne gidin **Ekle** seçip **denetleyicisi.**</span><span class="sxs-lookup"><span data-stu-id="9553b-314">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="9553b-315">Denetleyici adı tamamlamak, bırakın **boş MVC denetleyicisi** şablonu ve tıklatın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9553b-315">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="9553b-316">![PhotoController ekleme](whats-new-in-aspnet-mvc-4/_static/image19.png "PhotoController ekleme")</span><span class="sxs-lookup"><span data-stu-id="9553b-316">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="9553b-317">*PhotoController ekleme*</span><span class="sxs-lookup"><span data-stu-id="9553b-317">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="9553b-318">Değiştir **dizin** aşağıdaki yöntemiyle **galeri** eylem ve son eklediğiniz projeye JSON dosyasından içerik return.</span><span class="sxs-lookup"><span data-stu-id="9553b-318">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="9553b-319">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - galeri eylem*)</span><span class="sxs-lookup"><span data-stu-id="9553b-319">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. <span data-ttu-id="9553b-320">Tuşuna **F5** çözümü çalıştırın ve mocked fotoğraf hizmet test etmek için aşağıdaki URL'ye gidin: `http://localhost:[port]/photo/gallery` ([bağlantı noktası] değeri uygulama burada başlatılmış geçerli bağlantı noktasına bağlıdır).</span><span class="sxs-lookup"><span data-stu-id="9553b-320">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="9553b-321">Bu URL'sine yönelik istek içeriğini almak **Photos.json** dosya.</span><span class="sxs-lookup"><span data-stu-id="9553b-321">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="9553b-322">![Mocked fotoğraf hizmetini sınama](whats-new-in-aspnet-mvc-4/_static/image20.png "mocked fotoğraf hizmeti test etme")</span><span class="sxs-lookup"><span data-stu-id="9553b-322">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="9553b-323">*Mocked fotoğraf hizmeti test etme*</span><span class="sxs-lookup"><span data-stu-id="9553b-323">*Testing the mocked photo service*</span></span>

<span data-ttu-id="9553b-324">Gerçek bir uygulamasında kullanabilirsiniz [ASP.NET Web API](../../../../web-api/index.md) Fotoğraf Galerisi hizmeti uygulama.</span><span class="sxs-lookup"><span data-stu-id="9553b-324">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="9553b-325">ASP.NET Web API istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere çok çeşitli ulaşmak HTTP hizmetlerini oluşturmayı kolaylaştıran bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="9553b-325">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="9553b-326">ASP.NET Web API, .NET Framework üzerinde RESTful uygulamaları geliştirmek için ideal bir platformdur.</span><span class="sxs-lookup"><span data-stu-id="9553b-326">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="9553b-327">Görev 2 - Fotoğraf Galerisi görüntüleme</span><span class="sxs-lookup"><span data-stu-id="9553b-327">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="9553b-328">Bu görevde, bu alıştırmada ilk görevde oluşturduğunuz mocked hizmetini kullanarak Fotoğraf Galerisi göstermek için giriş sayfası güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="9553b-328">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="9553b-329">Model dosyaları ekleyin ve galeri görünümlerini güncelleştirmek.</span><span class="sxs-lookup"><span data-stu-id="9553b-329">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="9553b-330">Visual Studio'da basın **SHIFT** + **F5** uygulamanın hata ayıklamasını durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="9553b-330">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="9553b-331">Oluşturma **fotoğraf** sınıfını **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="9553b-331">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="9553b-332">Bunu yapmak için sağ **modelleri** klasöründe seçin **Ekle** tıklatıp **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="9553b-332">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="9553b-333">Ardından, kümesinin adı **Photo.cs** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9553b-333">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="9553b-334">Aşağıdaki üye ekleme **fotoğraf** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9553b-334">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="9553b-335">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - fotoğraf modeli*)</span><span class="sxs-lookup"><span data-stu-id="9553b-335">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. <span data-ttu-id="9553b-336">Açık **HomeController.cs** dosya **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="9553b-336">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="9553b-337">Aşağıdaki using deyimlerini.</span><span class="sxs-lookup"><span data-stu-id="9553b-337">Add the following using statements.</span></span>

    <span data-ttu-id="9553b-338">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - HomeController kullanımları*)</span><span class="sxs-lookup"><span data-stu-id="9553b-338">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. <span data-ttu-id="9553b-339">Güncelleştirme **dizin** kullanmak için eylem **HttpClient** galeri verileri almak ve daha sonra kullanmak için **JavaScriptSerializer** görünüm modeli seri durumdan çıkarılamadı.</span><span class="sxs-lookup"><span data-stu-id="9553b-339">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="9553b-340">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - dizin eylem*)</span><span class="sxs-lookup"><span data-stu-id="9553b-340">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. <span data-ttu-id="9553b-341">Açık **Index.cshtml** altında bulunan dosya **görünümler** klasörünü ve tüm içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9553b-341">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="9553b-342">Bu kod aracılığıyla hizmetinden alınan tüm fotoğrafları döngüler ve sırasız bir listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9553b-342">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="9553b-343">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - fotoğraf listesi*)</span><span class="sxs-lookup"><span data-stu-id="9553b-343">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. <span data-ttu-id="9553b-344">İçinde **Çözüm Gezgini**, sağ **içerik** klasörü seçin ve proje **Ekle | Varolan öğeyi**.</span><span class="sxs-lookup"><span data-stu-id="9553b-344">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="9553b-345">Gözat **Source\Assets\Content** bu laboratuvarı klasör ekleyin **Site.css** dosya.</span><span class="sxs-lookup"><span data-stu-id="9553b-345">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="9553b-346">Değişimi onaylamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9553b-346">You will have to confirm its replacement.</span></span> <span data-ttu-id="9553b-347">Varsa **Site.css** dosya açık, ayrıca dosyayı yeniden doğrulamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="9553b-347">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="9553b-348">Dosya Gezgini'ni açın ve tüm kopyalama **fotoğraf** klasörünün altında **Source\Assets** bu laboratuvarı Çözüm Gezgini'nde projenizin kök klasörünün klasör.</span><span class="sxs-lookup"><span data-stu-id="9553b-348">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="9553b-349">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9553b-349">Run the application.</span></span> <span data-ttu-id="9553b-350">Fotoğraf Galerisi görüntüleme giriş sayfası görmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-350">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="9553b-351">![Fotoğraf Galerisi](whats-new-in-aspnet-mvc-4/_static/image21.png "Fotoğraf Galerisi")</span><span class="sxs-lookup"><span data-stu-id="9553b-351">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="9553b-352">*Fotoğraf Galerisi*</span><span class="sxs-lookup"><span data-stu-id="9553b-352">*Photo Gallery*</span></span>
11. <span data-ttu-id="9553b-353">Visual Studio'da basın **SHIFT** + **F5** uygulamanın hata ayıklamasını durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="9553b-353">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="9553b-354">Alıştırma 3: mobil cihaz desteği ekleme</span><span class="sxs-lookup"><span data-stu-id="9553b-354">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="9553b-355">ASP.NET MVC 4 anahtar güncelleştirmeleri mobil geliştirme desteği biridir.</span><span class="sxs-lookup"><span data-stu-id="9553b-355">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="9553b-356">Bu alıştırmada, önceki alıştırmada oluşturduğunuz Fotografgalerisi çözüm genişleterek mobil uygulamalar için ASP.NET MVC 4 yeni özellikleri inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-356">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="9553b-357">Görev 1 - bir ASP.NET MVC 4 uygulamasında yükleme jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="9553b-357">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="9553b-358">Açık **başlamak** çözüm bulunan **kaynak/Ex3-MobileSupport/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="9553b-358">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="9553b-359">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="9553b-359">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="9553b-360">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="9553b-360">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9553b-361">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="9553b-361">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="9553b-362">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="9553b-362">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="9553b-363">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="9553b-363">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9553b-364">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="9553b-364">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9553b-365">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9553b-365">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9553b-366">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="9553b-366">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9553b-367">Açık **Paket Yöneticisi Konsolu** tıklayarak **Araçları** &gt; **kitaplık Paket Yöneticisi** &gt; **Paket Yöneticisi Konsol** menü seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9553b-367">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="9553b-368">![NuGet Paket Yöneticisi konsolu açma](whats-new-in-aspnet-mvc-4/_static/image22.png "NuGet Paket Yöneticisi konsolunu açma")</span><span class="sxs-lookup"><span data-stu-id="9553b-368">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="9553b-369">*NuGet Paket Yöneticisi konsolu açma*</span><span class="sxs-lookup"><span data-stu-id="9553b-369">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="9553b-370">Paket Yöneticisi konsolunda yüklemek için aşağıdaki komutu çalıştırın **jQuery.Mobile.MVC** paket.</span><span class="sxs-lookup"><span data-stu-id="9553b-370">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="9553b-371">jQuery Mobile, dokunma özelliği iyileştirilmiş web kullanıcı Arabirimi oluşturmak için bir açık kaynak kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="9553b-371">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="9553b-372">JQuery Mobile'ı ASP.NET MVC 4 uygulamayla kullanmak için Yardımcıları jQuery.Mobile.MVC NuGet paketini içerir.</span><span class="sxs-lookup"><span data-stu-id="9553b-372">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9553b-373">Aşağıdaki komutu çalıştırarak, jQuery.Mobile.MVC kitaplığını Nuget'ten indiriliyor.</span><span class="sxs-lookup"><span data-stu-id="9553b-373">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="9553b-374">PM</span><span class="sxs-lookup"><span data-stu-id="9553b-374">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="9553b-375">Bu komut, jQuery Mobile ve aşağıdakiler de dahil olmak üzere bazı yardımcı dosyaları yükler:</span><span class="sxs-lookup"><span data-stu-id="9553b-375">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="9553b-376">**Görünümler/paylaşılan/\_Layout.Mobile.cshtml**: küçük bir ekran için en iyi hale getirilmiş bir jQuery Mobile tabanlı düzeni.</span><span class="sxs-lookup"><span data-stu-id="9553b-376">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="9553b-377">Web sitesi mobil tarayıcıdan bir istek aldığında, özgün düzeni değiştirir (\_Layout.cshtml) Bu bir.</span><span class="sxs-lookup"><span data-stu-id="9553b-377">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="9553b-378">Bir görünüm değiştirici bileşeni: oluşan **görünümler/paylaşılan/\_ViewSwitcher.cshtml** kısmi Görünüm ve **ViewSwitcherController.cs** denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="9553b-378">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="9553b-379">Bu bileşen, kullanıcıların sayfanın Masaüstü sürümüne geçiş mobil tarayıcılar üzerinde bir bağlantı gösterir.</span><span class="sxs-lookup"><span data-stu-id="9553b-379">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="9553b-380">![Fotoğraf Galerisi proje mobil desteğiyle](whats-new-in-aspnet-mvc-4/_static/image23.png "mobil desteğiyle Fotoğraf Galerisi projesi")</span><span class="sxs-lookup"><span data-stu-id="9553b-380">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="9553b-381">*Fotoğraf Galerisi proje mobil desteği*</span><span class="sxs-lookup"><span data-stu-id="9553b-381">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="9553b-382">Mobil paketleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="9553b-382">Register the Mobile bundles.</span></span> <span data-ttu-id="9553b-383">Bunu yapmak için açın **Global.asax.cs** dosya ve aşağıdaki satırı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9553b-383">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="9553b-384">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex03 - kayıt mobil paketleri*)</span><span class="sxs-lookup"><span data-stu-id="9553b-384">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. <span data-ttu-id="9553b-385">Bir masaüstü web tarayıcısı kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9553b-385">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="9553b-386">Açık **Windows Phone 7 öykünücüsü** bulunan **Başlat menüsü | Tüm Programlar | Windows Phone SDK 7.1 | Windows Phone öykünücüsü.**</span><span class="sxs-lookup"><span data-stu-id="9553b-386">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="9553b-387">Telefon başlangıç ekranında da Internet Explorer'ı açın.</span><span class="sxs-lookup"><span data-stu-id="9553b-387">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="9553b-388">Uygulama başlatıldığı URL denetleyin ve telefon tarayıcı bu URL'yi gidin (örneğin `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="9553b-388">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="9553b-389">JQuery.Mobile.MVC projenizdeki mobil aygıtlar için en iyi duruma getirilmiş görünümleri göster yeni varlıklar oluşturan gibi uygulamanızı Windows Phone öykünücüsünde farklı göründüğünden emin fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-389">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="9553b-390">Masaüstü görünümüne geçiş bağlantısını gösteren telefon üstündeki iletisini görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="9553b-390">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="9553b-391">Ayrıca,  **\_Layout.Mobile.cshtml** yüklediğiniz paketi tarafından oluşturulmuş düzeni uygulamada farklı bir düzen dahil.</span><span class="sxs-lookup"><span data-stu-id="9553b-391">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9553b-392">Şu ana kadar mobil görünümüne dönmek için bağlantı yok.</span><span class="sxs-lookup"><span data-stu-id="9553b-392">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="9553b-393">Sonraki sürümlerde de dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="9553b-393">It will be included in later versions.</span></span>

    <span data-ttu-id="9553b-394">![Fotoğraf Galerisi giriş sayfasının mobil Görünüm](whats-new-in-aspnet-mvc-4/_static/image24.png "Fotoğraf Galerisi giriş sayfasının mobil Görünüm")</span><span class="sxs-lookup"><span data-stu-id="9553b-394">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="9553b-395">*Fotoğraf Galerisi giriş sayfasının mobil Görünüm*</span><span class="sxs-lookup"><span data-stu-id="9553b-395">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="9553b-396">Visual Studio'da basın **SHIFT** + **F5** uygulamanın hata ayıklamasını durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="9553b-396">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="9553b-397">Görev 2 - mobil görünümlerini oluşturma</span><span class="sxs-lookup"><span data-stu-id="9553b-397">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="9553b-398">Bu görevde, mobil cihazlarda iyi appareance için uyarlanmış içerikle dizin görünümünün mobil bir sürümünü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9553b-398">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="9553b-399">Kopya **Views\Home\Index.cshtml** görüntülemek ve bir kopya oluşturun, yeni dosyayı yeniden adlandırmak için Yapıştır **Index.Mobile.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="9553b-399">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="9553b-400">Açık yeni oluşturulan **Index.Mobile.cshtml** görüntülemek ve varolan &lt;ul&gt; bu kodu etiketi.</span><span class="sxs-lookup"><span data-stu-id="9553b-400">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="9553b-401">Bunu yaparak, güncelleştirmekte &lt;ul&gt; jQuery mobil temalardan kullanmak üzere jQuery mobil veri ek açıklamaları etiketi.</span><span class="sxs-lookup"><span data-stu-id="9553b-401">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > <span data-ttu-id="9553b-402">Şunlara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="9553b-402">Notice that:</span></span>
    > 
    > - <span data-ttu-id="9553b-403">**Data-role** özniteliğini **listview** listview stilleri kullanarak listesini kabul eder.</span><span class="sxs-lookup"><span data-stu-id="9553b-403">The **data-role** attribute set to **listview** will render the list using the listview styles.</span></span>
    > 
    > - <span data-ttu-id="9553b-404">**Verileri dışarı** özniteliği true olarak ayarlandığında, listenin yuvarlatılmış kenarlık ve kenar boşluğu ile gösterilir.</span><span class="sxs-lookup"><span data-stu-id="9553b-404">The **data-inset** attribute set to true will show the list with rounded border and margin.</span></span>
    > 
    > - <span data-ttu-id="9553b-405">**Veri filtresini** özniteliğini **doğru** bir arama kutusu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9553b-405">The **data-filter** attribute set to **true** will generate a search box.</span></span>
    > 
    > <span data-ttu-id="9553b-406">Proje belgelerinde jQuery mobil kuralları hakkında daha fazla bilgi edinebilirsiniz: [ [http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span><span class="sxs-lookup"><span data-stu-id="9553b-406">You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span></span>
3. <span data-ttu-id="9553b-407">Tuşuna **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="9553b-407">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="9553b-408">Geçiş **Windows Phone öykünücüsü** ve site yenileyin.</span><span class="sxs-lookup"><span data-stu-id="9553b-408">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="9553b-409">Yeni Görünüm ve yapısını galeri listesinin yanı sıra üstte bulunan yeni arama kutusuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-409">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="9553b-410">Bir sözcük arama kutusuna yazın (örneğin, **Tulips**) Fotoğraf Galerisi bir aramayı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="9553b-410">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="9553b-411">![Filtre ile ListView stili kullanılarak galeri](whats-new-in-aspnet-mvc-4/_static/image25.png "filtreleme ile listview stili kullanılarak Galerisi")</span><span class="sxs-lookup"><span data-stu-id="9553b-411">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="9553b-412">*Filtre ile ListView stili kullanılarak Galerisi*</span><span class="sxs-lookup"><span data-stu-id="9553b-412">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="9553b-413">Özetlemek için Görünüm Mobilizer tarif dizin görünümünün bir kopyasını oluşturmak için kullandığınız &quot;mobil&quot; soneki.</span><span class="sxs-lookup"><span data-stu-id="9553b-413">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="9553b-414">Bu soneki ASP.NET MVC 4 ile bir mobil cihaz aracılığıyla oluşturulan her istek dizini bu kopyası kullanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="9553b-414">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="9553b-415">Ayrıca, jQuery Mobile site görünüm mobil cihazlarda geliştirme için kullanılacak dizini görünümün mobil sürümü güncelleştirdiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-415">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="9553b-416">Visual Studio geri gidin ve açık **Site.Mobile.css** altında bulunan **içerik** klasör.</span><span class="sxs-lookup"><span data-stu-id="9553b-416">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="9553b-417">Görüntü sağ tarafında Göster yapmak için Fotoğraf Başlığı konumlandırma düzeltin.</span><span class="sxs-lookup"><span data-stu-id="9553b-417">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="9553b-418">Bunu yapmak için aşağıdaki kodu ekleyin **Site.Mobile.css** dosya.</span><span class="sxs-lookup"><span data-stu-id="9553b-418">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="9553b-419">CSS</span><span class="sxs-lookup"><span data-stu-id="9553b-419">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="9553b-420">Tuşuna **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="9553b-420">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="9553b-421">Dönmek **Windows Phone öykünücüsü** ve site yenileyin.</span><span class="sxs-lookup"><span data-stu-id="9553b-421">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="9553b-422">Fotoğraf Başlığı düzgün şimdi konumlandırılır dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-422">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="9553b-423">![Görüntünün sağına konumlandırılmış başlık](whats-new-in-aspnet-mvc-4/_static/image26.png "görüntünün sağına konumlandırılmış başlığı")</span><span class="sxs-lookup"><span data-stu-id="9553b-423">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="9553b-424">*Görüntünün sağına konumlandırılmış başlığı*</span><span class="sxs-lookup"><span data-stu-id="9553b-424">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="9553b-425">Görev 3 - jQuery Mobile temaları</span><span class="sxs-lookup"><span data-stu-id="9553b-425">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="9553b-426">Her düzeni ve pencere öğesinde jQuery Mobile tam birleşik görsel tasarım tema sitelere ve uygulamalara uygulamak olanaklı kılan bir yeni nesne yönelimli CSS framework çevresinde tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9553b-426">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="9553b-427">jQuery Mobile'nın varsayılan tema harf verilen 5 örnekleri içerir (e) hızlı başvuru için a b c d.</span><span class="sxs-lookup"><span data-stu-id="9553b-427">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="9553b-428">Bu görevde, varsayılandan farklı bir tema kullanılacak mobil düzenini güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="9553b-428">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="9553b-429">Visual Studio'ya geri çevirin.</span><span class="sxs-lookup"><span data-stu-id="9553b-429">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="9553b-430">Açık  **\_Layout.Mobile.cshtml** bulunan dosya **görünümler/paylaşılan**.</span><span class="sxs-lookup"><span data-stu-id="9553b-430">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="9553b-431">Div öğesinin ayarlamak veri rol bulur. &quot;sayfa&quot; ve güncelleştirme **veri tema** için &quot; **e**&quot;.</span><span class="sxs-lookup"><span data-stu-id="9553b-431">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>


    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. <span data-ttu-id="9553b-432">Tuşuna **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="9553b-432">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="9553b-433">Siteyi Yenile **Windows Phone öykünücüsü** ve yeni renk düzenini dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-433">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="9553b-434">![Farklı renk düzenini mobil düzeniyle](whats-new-in-aspnet-mvc-4/_static/image27.png "farklı renk düzenini mobil Düzen")</span><span class="sxs-lookup"><span data-stu-id="9553b-434">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="9553b-435">*Farklı renk düzenini mobil Düzen*</span><span class="sxs-lookup"><span data-stu-id="9553b-435">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="9553b-436">Görev 4 - görünüm değiştirici bileşeni ve özelliklerini geçersiz kılma tarayıcı kullanma</span><span class="sxs-lookup"><span data-stu-id="9553b-436">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="9553b-437">Bir mobil iyileştirilmiş web sayfaları için metni bir şey Masaüstü görünümünde veya kullanıcıların bir masaüstü sürümüne geçiş sağlayan sitenin tam modu gibi olan bağlantı eklemek için kuralıdır.</span><span class="sxs-lookup"><span data-stu-id="9553b-437">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="9553b-438">Bir örnek jQuery.Mobile.MVC paketi içeren **görünüm değiştirici** bu amaçla kullanılan bileşen  **\_Layout.Mobile.cshtml** görünümü.</span><span class="sxs-lookup"><span data-stu-id="9553b-438">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="9553b-439">![Masaüstü görünümüne geçiş yapmak için bağlantı](whats-new-in-aspnet-mvc-4/_static/image28.png "Masaüstü görünümüne geçiş yapmak için bağlantı")</span><span class="sxs-lookup"><span data-stu-id="9553b-439">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="9553b-440">*Masaüstü görünümüne geçiş yapmak için bağlantı*</span><span class="sxs-lookup"><span data-stu-id="9553b-440">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="9553b-441">Görünüm değiştirici adında yeni bir özellik kullanır **tarayıcı geçersiz kılma**.</span><span class="sxs-lookup"><span data-stu-id="9553b-441">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="9553b-442">Bu özellik gerçekten geldikleri olandan farklı bir tarayıcıdan (kullanıcı aracısı) çıkıyormuş gibi istekleri kabul uygulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="9553b-442">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="9553b-443">Bu görevde, jQuery.Mobile.MVC ve ASP.NET MVC 4'te özelliklerini geçersiz kılma yeni tarayıcı tarafından eklenen bir görünüm değiştirici örnek uygulaması inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-443">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="9553b-444">Visual Studio'ya geri çevirin.</span><span class="sxs-lookup"><span data-stu-id="9553b-444">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="9553b-445">Açık  **\_Layout.Mobile.cshtml** görünümü bulunan altında **görünümler/paylaşılan** klasör ve bir kısmi görünüm olarak başvurulan görünüm değiştirici bileşeni dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-445">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="9553b-446">![Görünüm değiştirici bileşenini kullanarak mobil düzeni](whats-new-in-aspnet-mvc-4/_static/image29.png "görünüm değiştirici bileşenini kullanarak mobil düzeni")</span><span class="sxs-lookup"><span data-stu-id="9553b-446">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="9553b-447">*Görünüm değiştirici bileşenini kullanarak mobil düzeni*</span><span class="sxs-lookup"><span data-stu-id="9553b-447">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="9553b-448">Açık  **\_ViewSwitcher.cshtml** kısmi görünüm.</span><span class="sxs-lookup"><span data-stu-id="9553b-448">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="9553b-449">Kısmi görünüm yeni yöntemi kullanan **ViewContext.HttpContext.GetOverriddenBrowser()** web isteğini kökenini belirlemek ve Masaüstü veya mobil görünümlere ya da geçiş yapmak için karşılık gelen bağlantıyı göstermek için.</span><span class="sxs-lookup"><span data-stu-id="9553b-449">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="9553b-450">**GetOverridenBrowser** yöntemi döndürür bir **HttpBrowserCapabilitiesBase** istek için ayarlanmış kullanıcı aracısı karşılık gelen örnek (gerçek veya geçersiz kılınan).</span><span class="sxs-lookup"><span data-stu-id="9553b-450">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="9553b-451">Bu değer gibi özellikleri almak için kullanabileceğiniz **IsMobileDevice**.</span><span class="sxs-lookup"><span data-stu-id="9553b-451">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="9553b-452">![ViewSwitcher kısmi Görünüm](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher kısmi görünümü")</span><span class="sxs-lookup"><span data-stu-id="9553b-452">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="9553b-453">*ViewSwitcher kısmi görünümü*</span><span class="sxs-lookup"><span data-stu-id="9553b-453">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="9553b-454">Açık **ViewSwitcherController.cs** sınıfı bulunan **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="9553b-454">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="9553b-455">Bu SwitchView eylemi kullanıma ViewSwitcher bileşen bağlantıyı tarafından çağrılır ve yeni HttpContext yöntemleri dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-455">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="9553b-456">**HttpContext.ClearOverridenBrowser()** yöntemi, tüm geçerli istek için geçersiz kılınan Kullanıcı aracısını kaldırır.</span><span class="sxs-lookup"><span data-stu-id="9553b-456">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="9553b-457">**HttpContext.SetOverridenBrowser()** yöntemi isteğin asıl kullanıcı aracısı değerini belirtilen kullanıcı aracısını kullanarak geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="9553b-457">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="9553b-458">![ViewSwitcher denetleyicisi](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher denetleyicisi")</span><span class="sxs-lookup"><span data-stu-id="9553b-458">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="9553b-459">*ViewSwitcher denetleyicisi*</span><span class="sxs-lookup"><span data-stu-id="9553b-459">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="9553b-460">Tarayıcı geçersiz kılınıyor çekirdek, bir ASP.NET MVC 4, jQuery.Mobile.MVC paket yüklemeyin olsa da kullanılabilir olduğu özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="9553b-460">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="9553b-461">Ancak, bu özellik yalnızca görünüm, Düzen ve kısmi görünüm etkiler ve Request.Browser nesneye bağlı özelliklerinden herhangi birini etkilemez.</span><span class="sxs-lookup"><span data-stu-id="9553b-461">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="9553b-462">Görev 5 - görünüm değiştirici Masaüstü görünümünde ekleme</span><span class="sxs-lookup"><span data-stu-id="9553b-462">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="9553b-463">Bu görevde, masaüstü düzeni görünüm değiştirici içerecek şekilde güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="9553b-463">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="9553b-464">Bu Masaüstü görünümünde gezinirken mobil görünüme geri dönmek mobil kullanıcılar izin verir.</span><span class="sxs-lookup"><span data-stu-id="9553b-464">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="9553b-465">Siteyi Yenile **Windows Phone öykünücüsü**.</span><span class="sxs-lookup"><span data-stu-id="9553b-465">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="9553b-466">Tıklayın **Masaüstü görünümünde** galeri üstündeki bağlantı.</span><span class="sxs-lookup"><span data-stu-id="9553b-466">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="9553b-467">Görünüm değiştirici mobil görünümüne dönmek izin vermek için Masaüstü görünümünde fark.</span><span class="sxs-lookup"><span data-stu-id="9553b-467">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="9553b-468">Visual Studio geri gidin ve açık  **\_Layout.cshtml** görünümü.</span><span class="sxs-lookup"><span data-stu-id="9553b-468">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="9553b-469">Oturum açma bölümü bulun ve işlemek için bir çağrı ekleyin  **\_ViewSwitcher** kısmi görünüm aşağıdaki  **\_LogOnPartial** kısmi görünüm.</span><span class="sxs-lookup"><span data-stu-id="9553b-469">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="9553b-470">Ardından, basın **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="9553b-470">Then, press **CTRL + S** to save the changes.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. <span data-ttu-id="9553b-471">Tuşuna **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="9553b-471">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="9553b-472">Windows Phone öykünücüsü içinde sayfayı yenileyin ve yakınlaştırmak için ekran'ı çift tıklatın.</span><span class="sxs-lookup"><span data-stu-id="9553b-472">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="9553b-473">Giriş sayfası artık gösterir bildirimi **mobil Görünüm** mobil masaüstü görünümüne geçer bağlantı.</span><span class="sxs-lookup"><span data-stu-id="9553b-473">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="9553b-474">![Masaüstü görünümünde çizilir değiştirici görüntülemek](whats-new-in-aspnet-mvc-4/_static/image32.png "Masaüstü görünümünde işlenmiş görünüm değiştirici")</span><span class="sxs-lookup"><span data-stu-id="9553b-474">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="9553b-475">*Masaüstü görünümünde işlenmiş görünüm değiştirici*</span><span class="sxs-lookup"><span data-stu-id="9553b-475">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="9553b-476">Yeniden mobil görünümüne geçin ve göz **hakkında** sayfa (http://localhost[bağlantınoktası]/Home/hakkında).</span><span class="sxs-lookup"><span data-stu-id="9553b-476">Switch to the Mobile view again and browse to **About** page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="9553b-477">About.Mobile.cshtml görünüm oluşturmadınız olsa bile hakkında sayfa mobil düzenini kullanarak görüntülendiğini fark (\_Layout.Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="9553b-477">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="9553b-478">![Sayfayla ilgili](whats-new-in-aspnet-mvc-4/_static/image33.png "sayfa hakkında")</span><span class="sxs-lookup"><span data-stu-id="9553b-478">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="9553b-479">*Sayfa hakkında*</span><span class="sxs-lookup"><span data-stu-id="9553b-479">*About page*</span></span>
8. <span data-ttu-id="9553b-480">Son olarak, site masaüstü bir Web tarayıcısında açın.</span><span class="sxs-lookup"><span data-stu-id="9553b-480">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="9553b-481">Önceki güncelleştirmelerinin hiçbiri Masaüstü görünümünde etkiledi dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-481">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="9553b-482">![Fotografgalerisi Masaüstü görünümünde](whats-new-in-aspnet-mvc-4/_static/image34.png "Fotografgalerisi Masaüstü görünümü")</span><span class="sxs-lookup"><span data-stu-id="9553b-482">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="9553b-483">*Fotografgalerisi Masaüstü görünümü*</span><span class="sxs-lookup"><span data-stu-id="9553b-483">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="9553b-484">Görev 6 - oluşturma yeni görüntü modları</span><span class="sxs-lookup"><span data-stu-id="9553b-484">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="9553b-485">Yeni görüntüleme modları özellik isteği oluşturma tarayıcı bağlı olarak görünümleri seçin uygulama olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="9553b-485">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="9553b-486">Örneğin, bir masaüstü tarayıcısı giriş sayfası isterse, uygulama döndürülecek **Views\Home\Index.cshtml** şablonu.</span><span class="sxs-lookup"><span data-stu-id="9553b-486">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="9553b-487">Bir mobil tarayıcı giriş sayfası isterse, uygulama daha sonra döndürülecek **Views\Home\Index.mobile.cshtml** şablonu.</span><span class="sxs-lookup"><span data-stu-id="9553b-487">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="9553b-488">Bu görev iPhone cihazları için özelleştirilmiş bir düzen oluşturacak ve iPhone cihazları gelen istekleri benzetimini yapmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="9553b-488">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="9553b-489">Bunu yapmak için ya da bir iPhone öykünücüde/benzeticide kullanabilirsiniz (gibi [elektrik mobil Simulator](http://www.electricplum.com/)) veya bir tarayıcı kullanıcı aracısı değiştirme eklentilere.</span><span class="sxs-lookup"><span data-stu-id="9553b-489">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="9553b-490">İPhone benzetmek yönergeler için bir Safari tarayıcı kullanıcı aracısı dizesi ayarlamak, bkz: [IE olmasından içeriğini Safari izin vermek nasıl](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David Alison'ın Web günlüğündeki.</span><span class="sxs-lookup"><span data-stu-id="9553b-490">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="9553b-491">**Bu görev isteğe bağlıdır ve yürütme olmadan Laboratuvar devam edebilirsiniz dikkat edin.**</span><span class="sxs-lookup"><span data-stu-id="9553b-491">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="9553b-492">Visual Studio'da basın **SHIFT** + **F5** uygulamanın hata ayıklamasını durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="9553b-492">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="9553b-493">Açık **Global.asax.cs** ve aşağıdaki ekleme deyimini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="9553b-493">Open **Global.asax.cs** and add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. <span data-ttu-id="9553b-494">Aşağıdaki vurgulanmış kodu uygulamasına ekleme\_Başlat yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9553b-494">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="9553b-495">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex03 - iPhone DisplayMode*)</span><span class="sxs-lookup"><span data-stu-id="9553b-495">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

    <span data-ttu-id="9553b-496">Yeni kaydettiğiniz **adlı DefaultDisplayMode &quot;iPhone&quot;**, statik içinde **DisplayModeProvider.Instance.Modes** karşı eşleşen statik listesi her gelen istek.</span><span class="sxs-lookup"><span data-stu-id="9553b-496">You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request.</span></span> <span data-ttu-id="9553b-497">Gelen istek dize içeriyorsa &quot;iPhone&quot;, ASP.NET MVC adı içeren görünümler bulacaksınız &quot;iPhone&quot; soneki.</span><span class="sxs-lookup"><span data-stu-id="9553b-497">If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix.</span></span> <span data-ttu-id="9553b-498">0 parametresi yeni modu nasıl belirli gösterir; Örneğin, bu görünüm genel daha fazla belirli &quot;.mobile&quot; istekleri mobil aygıtlardan eşleşen kural.</span><span class="sxs-lookup"><span data-stu-id="9553b-498">The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.</span></span>

    <span data-ttu-id="9553b-499">Bir iPhone tarayıcı istek oluşturduğunda bu kodu çalıştıktan sonra uygulamanız kullanacak **görünümler/paylaşılan\\_Layout.iPhone.cshtml** düzeni sonraki adımlarda oluşturacaktır.</span><span class="sxs-lookup"><span data-stu-id="9553b-499">After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9553b-500">İPhone tanıtım amacıyla Basitleştirilmiş ve (örneğin test büyük küçük harfe duyarlı olması nedeniyle) her iPhone kullanıcı aracısı dizesi beklendiği gibi çalışmayabilir istek sınama bu yolu.</span><span class="sxs-lookup"><span data-stu-id="9553b-500">This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).</span></span>
4. <span data-ttu-id="9553b-501">Bir kopyasını oluşturmak  **\_Layout.Mobile.cshtml** dosyasını **görünümler/paylaşılan** klasörü ve kopyalayın adlandırın &quot;  **\_Layout.iPhone.csthml** &quot;.</span><span class="sxs-lookup"><span data-stu-id="9553b-501">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="9553b-502">Açık  **\_Layout.iPhone.csthml** önceki adımda oluşturduğunuz.</span><span class="sxs-lookup"><span data-stu-id="9553b-502">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="9553b-503">Div öğesinin ayarlamak veri role özniteliğini bulmak **sayfa** değiştirip **veri tema** özniteliğini &quot; **bir**&quot;.</span><span class="sxs-lookup"><span data-stu-id="9553b-503">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

    <span data-ttu-id="9553b-504">Şimdi, ASP.NET MVC 4 uygulamanızda 3 düzenleri vardır:</span><span class="sxs-lookup"><span data-stu-id="9553b-504">Now you have 3 layouts in your ASP.NET MVC 4 application:</span></span>

    1. <span data-ttu-id="9553b-505">**\_Layout.cshtml**: masaüstü tarayıcıları için kullanılan varsayılan düzen.</span><span class="sxs-lookup"><span data-stu-id="9553b-505">**\_Layout.cshtml**: default layout used for desktop browsers.</span></span>
    2. <span data-ttu-id="9553b-506">**\_Layout.Mobile.cshtml**: Mobil aygıtlar için kullanılan varsayılan düzen.</span><span class="sxs-lookup"><span data-stu-id="9553b-506">**\_Layout.mobile.cshtml**: default layout used for mobile devices.</span></span>
    3. <span data-ttu-id="9553b-507">**\_Layout.iPhone.cshtml**: gelen ayırt etmek için farklı renk düzenini kullanarak, iPhone cihazları için belirli düzen \_Layout.mobile.cshtml.</span><span class="sxs-lookup"><span data-stu-id="9553b-507">**\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.</span></span>
7. <span data-ttu-id="9553b-508">Tuşuna **F5** uygulamayı çalıştırın ve sitede gözatmak için **Windows Phone öykünücüsü**.</span><span class="sxs-lookup"><span data-stu-id="9553b-508">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="9553b-509">Açık bir **iPhone benzeticisi** (bkz [ek C](#AppendixC) yükleme ve bir iPhone benzeticisi yapılandırma hakkında yönergeler için) ve siteye çok göz atın.</span><span class="sxs-lookup"><span data-stu-id="9553b-509">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="9553b-510">Her telefon belirli bir şablon kullandığını dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-510">Notice that each phone is using the specific template.</span></span>

    ![Using-Different-Views-for-each-Mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="9553b-512">*Her mobil cihaz için farklı görünümleri kullanma*</span><span class="sxs-lookup"><span data-stu-id="9553b-512">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="9553b-513">Alıştırma 4: Zaman uyumsuz denetleyicileri kullanma</span><span class="sxs-lookup"><span data-stu-id="9553b-513">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="9553b-514">Microsoft .NET Framework 4.5 C# ve Visual Basic .NET programlama asynchrony için yeni bir temel sağlamaya yönelik yeni dil özellikleri sunar.</span><span class="sxs-lookup"><span data-stu-id="9553b-514">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="9553b-515">Bu yeni temel zaman uyumsuz programlama-benzer ve yaklaşık olarak kolay olarak - zaman uyumlu programlama hale getirir.</span><span class="sxs-lookup"><span data-stu-id="9553b-515">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="9553b-516">Şimdi kullanarak ASP.NET MVC 4'te zaman uyumsuz eylem yöntemleri yazabilmesi **AsyncController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9553b-516">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="9553b-517">CPU olmayan istekleri bağlı, uzun süre çalışan için zaman uyumsuz eylem yöntemlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-517">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="9553b-518">Bu istek gerçekleştirilirken iş gerçekleştirmeyi Web sunucusu engelleme önler.</span><span class="sxs-lookup"><span data-stu-id="9553b-518">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="9553b-519">AsyncController sınıfı genellikle uzun süre çalışan Web hizmeti çağrıları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="9553b-519">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="9553b-520">Bu alıştırmada, ASP.NET MVC 4'te zaman uyumsuz işlem temelleri açıklanır.</span><span class="sxs-lookup"><span data-stu-id="9553b-520">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="9553b-521">Daha ayrıntılı bilgi edinmek istiyorsanız, aşağıdaki makaleyi denetleyebilirsiniz: [ [https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="9553b-521">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="9553b-522">Görev 1 - zaman uyumsuz bir denetleyici uygulama</span><span class="sxs-lookup"><span data-stu-id="9553b-522">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="9553b-523">Açık **başlamak** çözüm bulunan **kaynak/Ex4-zamanuyumsuz/başlangıç/** klasörü.</span><span class="sxs-lookup"><span data-stu-id="9553b-523">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="9553b-524">Aksi takdirde kullanarak devam edebilir **son** çözüm elde önceki alıştırmada tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="9553b-524">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="9553b-525">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="9553b-525">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9553b-526">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="9553b-526">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="9553b-527">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="9553b-527">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="9553b-528">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="9553b-528">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9553b-529">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="9553b-529">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9553b-530">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9553b-530">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9553b-531">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="9553b-531">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="9553b-532">Açık **HomeController.cs** sınıfıyla **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="9553b-532">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="9553b-533">Aşağıdaki ekleme deyimini kullanarak.</span><span class="sxs-lookup"><span data-stu-id="9553b-533">Add the following using statement.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. <span data-ttu-id="9553b-534">Güncelleştirme **HomeController** devralmak için sınıf **AsyncController**.</span><span class="sxs-lookup"><span data-stu-id="9553b-534">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="9553b-535">Zaman uyumsuz isteklerini işlemek ASP.NET AsyncController türetilen denetleyicileri etkinleştirmek ve hizmet zaman uyumlu eylem yöntemleri hala yapabilir.</span><span class="sxs-lookup"><span data-stu-id="9553b-535">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. <span data-ttu-id="9553b-536">Ekleme **zaman uyumsuz** anahtar **dizin** yöntemi ve dönüş türü hale **görev&lt;ActionResult&gt;**.</span><span class="sxs-lookup"><span data-stu-id="9553b-536">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > <span data-ttu-id="9553b-537">**Zaman uyumsuz** anahtar sözcüğü .NET Framework 4.5 sağlayan yeni anahtar sözcüklerin biridir; bu yöntem zaman uyumsuz kodu içerir derleyici söyler.</span><span class="sxs-lookup"><span data-stu-id="9553b-537">The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code.</span></span> <span data-ttu-id="9553b-538">A **görev** nesnesi, belirli bir noktada gelecekte tamamlanabilir zaman uyumsuz bir işlem temsil eder.</span><span class="sxs-lookup"><span data-stu-id="9553b-538">A **Task** object represents an asynchronous operation that may complete at some point in the future.</span></span>
6. <span data-ttu-id="9553b-539">Değiştir **istemci. GetAsync()** sürümünü kullanarak tam zaman uyumsuz çağrı await anahtar sözcüğü aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="9553b-539">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="9553b-540">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex04 - GetAsync*)</span><span class="sxs-lookup"><span data-stu-id="9553b-540">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="9553b-541">Önceki sürümde, kullanmakta olduğunuz **sonuç** özelliğinden **görev** (eşitleme sürümü) sonuç döndürülmeden kadar iş parçacığı engellemek için nesne.</span><span class="sxs-lookup"><span data-stu-id="9553b-541">In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).</span></span>
    > 
    > <span data-ttu-id="9553b-542">Ekleme **await** anahtar sözcüğü yöntemi çağrısından döndürülen görev için zaman uyumsuz olarak beklenecek derleyici söyler.</span><span class="sxs-lookup"><span data-stu-id="9553b-542">Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call.</span></span> <span data-ttu-id="9553b-543">Bu, yalnızca awaited yöntemi tamamlandıktan sonra rest kodunun bir geri çağırma yürütülecek anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="9553b-543">This means that the rest of the code will be executed as a callback only after the awaited method completes.</span></span> <span data-ttu-id="9553b-544">Try-catch bloğu Bunun çalışmasını sağlamak için değişiklik gerekmez başka bir şey fark olduğu: arka planda veya ön planda gerçekleşecek özel durumlar hala framework tarafından sağlanan bir işleyici kullanarak ek iş olmadan yakalanacak.</span><span class="sxs-lookup"><span data-stu-id="9553b-544">Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.</span></span>
7. <span data-ttu-id="9553b-545">Aşağıda gösterildiği gibi yeni kodu ile satırları değiştirerek zaman uyumsuz bir uygulama ile devam etmek için kodu değiştirin</span><span class="sxs-lookup"><span data-stu-id="9553b-545">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="9553b-546">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex04 - ReadAsStringAsync*)</span><span class="sxs-lookup"><span data-stu-id="9553b-546">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. <span data-ttu-id="9553b-547">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9553b-547">Run the application.</span></span> <span data-ttu-id="9553b-548">Hiçbir önemli değişiklikler fark edeceksiniz, ancak kodunuzu daha iyi bir sunucu kaynaklarının kullanımını yapma ve performansı iyileştirme iş parçacığı havuzunun bir iş parçacığından engellemez.</span><span class="sxs-lookup"><span data-stu-id="9553b-548">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9553b-549">Laboratuvara yeni zaman uyumsuz programlama özellikler hakkında daha fazla bilgiyi &quot; **C# ve Visual Basic ile .NET 4.5 zaman uyumsuz programlama** &quot; Visual Studio eğitim Seti'nde yer.</span><span class="sxs-lookup"><span data-stu-id="9553b-549">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="9553b-550">Görev 2 - iptal belirteçlerini zaman aşımlarını işleme</span><span class="sxs-lookup"><span data-stu-id="9553b-550">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="9553b-551">Görev örnekleri döndüren zaman uyumsuz eylem yöntemleri zaman aşımlarını de destekler.</span><span class="sxs-lookup"><span data-stu-id="9553b-551">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="9553b-552">Bu görevde bir iptal belirteci kullanarak bir zaman aşımı senaryoda işlemek için dizin yöntemi kod güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="9553b-552">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="9553b-553">Geri dönerek Visual Studio ve tuşuna **SHIFT + F5** hata ayıklamasını durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="9553b-553">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="9553b-554">Aşağıdaki using deyimi için **HomeController.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="9553b-554">Add the following using statement to the **HomeController.cs** file.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. <span data-ttu-id="9553b-555">Güncelleştirme almak için dizin eylemi bir **CancellationToken** bağımsız değişkeni.</span><span class="sxs-lookup"><span data-stu-id="9553b-555">Update the Index action to receive a **CancellationToken** argument.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. <span data-ttu-id="9553b-556">Güncelleştirme **GetAsync** Çağrı iptal belirtecini geçirin.</span><span class="sxs-lookup"><span data-stu-id="9553b-556">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="9553b-557">(Kod parçacığını - *CancellationToken ile ASP.NET MVC 4 Laboratuvar - Ex04 - SendAsync*)</span><span class="sxs-lookup"><span data-stu-id="9553b-557">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. <span data-ttu-id="9553b-558">İşaretleme *dizin* yöntemiyle bir **AsyncTimeout** özniteliği için 500 milisaniye olarak ayarlanmış ve **HandleError** işlemek üzere yapılandırılmış öznitelik  **TaskCanceledException** için yönlendirerek bir **süresi sona erdi** görünümü.</span><span class="sxs-lookup"><span data-stu-id="9553b-558">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="9553b-559">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex04 - öznitelikleri*)</span><span class="sxs-lookup"><span data-stu-id="9553b-559">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. <span data-ttu-id="9553b-560">Açık **PhotoController** sınıfı ve güncelleştirme **galeri** uzun çalışan bir görev benzetimini yapmak için yürütme 1000 miliseconds (1 saniye) gecikme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9553b-560">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>


    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. <span data-ttu-id="9553b-561">Açık **Web.config** dosya ve özel hatalar, aşağıdaki öğeyi ekleyerek etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="9553b-561">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>


    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. <span data-ttu-id="9553b-562">Yeni bir görünüm oluşturma **görünümler/paylaşılan** adlı **süresi sona erdi** ve varsayılan düzenini kullanın.</span><span class="sxs-lookup"><span data-stu-id="9553b-562">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="9553b-563">Çözüm Gezgini'nde sağ **görünümler/paylaşılan** klasörü ve select **Ekle | Görünüm**.</span><span class="sxs-lookup"><span data-stu-id="9553b-563">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="9553b-564">![Her mobil cihaz için farklı görünümleri](whats-new-in-aspnet-mvc-4/_static/image36.png "her mobil cihaz için farklı görünümleri kullanma")</span><span class="sxs-lookup"><span data-stu-id="9553b-564">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="9553b-565">*Her mobil cihaz için farklı görünümleri kullanma*</span><span class="sxs-lookup"><span data-stu-id="9553b-565">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="9553b-566">Güncelleştirme **süresi sona erdi** aşağıda gösterildiği gibi içerik görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="9553b-566">Update the **TimedOut** view content as shown below.</span></span>


    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. <span data-ttu-id="9553b-567">Uygulamayı çalıştırın ve kök URL'ye gidin.</span><span class="sxs-lookup"><span data-stu-id="9553b-567">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="9553b-568">Eklediğiniz gibi bir **Thread.Sleep** 1000 milisaniye olarak tarafından oluşturulan bir zaman aşımı hatası alırsınız **AsyncTimeout** özniteliği ve tarafından catch **HandleError** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="9553b-568">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="9553b-569">![Zaman aşımı özel durumun ele](whats-new-in-aspnet-mvc-4/_static/image37.png "işlenen zaman aşımı özel durumu")</span><span class="sxs-lookup"><span data-stu-id="9553b-569">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="9553b-570">*İşlenen zaman aşımı özel durumu*</span><span class="sxs-lookup"><span data-stu-id="9553b-570">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="9553b-571">Ayrıca, Windows Azure Web siteleri aşağıdaki bu uygulamayı dağıtabilmek için [ek D: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixD).</span><span class="sxs-lookup"><span data-stu-id="9553b-571">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="9553b-572">Özet</span><span class="sxs-lookup"><span data-stu-id="9553b-572">Summary</span></span>

<span data-ttu-id="9553b-573">Bu hands-on-laboratuvarında, bazı yeni özellikler ASP.NET MVC 4'te yerleşik gözlenen.</span><span class="sxs-lookup"><span data-stu-id="9553b-573">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="9553b-574">Aşağıdaki kavramlar ele:</span><span class="sxs-lookup"><span data-stu-id="9553b-574">The following concepts have been discussed:</span></span>

- <span data-ttu-id="9553b-575">ASP.NET MVC proje şablonları-dahil olmak üzere yeni mobil uygulama proje şablonu geliştirmeler yararlanın</span><span class="sxs-lookup"><span data-stu-id="9553b-575">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="9553b-576">HTML5 görünüm penceresinin özniteliği ve CSS medya sorguları mobil cihazlarda görünen geliştirmek için kullanın</span><span class="sxs-lookup"><span data-stu-id="9553b-576">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="9553b-577">JQuery Mobile aşamalı geliştirmeleri ve dokunma özelliği iyileştirilmiş web kullanıcı Arabirimi oluşturmak için kullanın</span><span class="sxs-lookup"><span data-stu-id="9553b-577">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="9553b-578">Mobile özgü görünümler oluşturma</span><span class="sxs-lookup"><span data-stu-id="9553b-578">Create mobile-specific views</span></span>
- <span data-ttu-id="9553b-579">Görünüm değiştirici bileşeni uygulamasında mobil ve Masaüstü görünümleri arasında geçiş yapmak için kullanın</span><span class="sxs-lookup"><span data-stu-id="9553b-579">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="9553b-580">Görev desteğini kullanarak zaman uyumsuz denetleyicileri oluşturun</span><span class="sxs-lookup"><span data-stu-id="9553b-580">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="9553b-581">Ek A: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="9553b-581">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="9553b-582">Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip.</span><span class="sxs-lookup"><span data-stu-id="9553b-582">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="9553b-583">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="9553b-583">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="9553b-584">![Kod projenize eklemek için Visual Studio kod parçacıkları](whats-new-in-aspnet-mvc-4/_static/image38.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="9553b-584">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="9553b-585">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="9553b-585">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="9553b-586">***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="9553b-586">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="9553b-587">İmleci, burada kod eklemek istediğiniz yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="9553b-587">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="9553b-588">(Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="9553b-588">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="9553b-589">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="9553b-589">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="9553b-590">Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="9553b-590">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="9553b-591">İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="9553b-591">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="9553b-592">![Kod parçacığında adını yazmaya başlayın](whats-new-in-aspnet-mvc-4/_static/image39.png "parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="9553b-592">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="9553b-593">*Kod parçacığında adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="9553b-593">*Start typing the snippet name*</span></span>

<span data-ttu-id="9553b-594">![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](whats-new-in-aspnet-mvc-4/_static/image40.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="9553b-594">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="9553b-595">*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="9553b-595">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="9553b-596">![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](whats-new-in-aspnet-mvc-4/_static/image41.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")</span><span class="sxs-lookup"><span data-stu-id="9553b-596">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="9553b-597">*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*</span><span class="sxs-lookup"><span data-stu-id="9553b-597">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="9553b-598">***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="9553b-598">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="9553b-599">Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="9553b-599">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="9553b-600">Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.</span><span class="sxs-lookup"><span data-stu-id="9553b-600">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="9553b-601">Tıklayarak ilgili kod parçacığında listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="9553b-601">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="9553b-602">![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](whats-new-in-aspnet-mvc-4/_static/image42.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")</span><span class="sxs-lookup"><span data-stu-id="9553b-602">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="9553b-603">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*</span><span class="sxs-lookup"><span data-stu-id="9553b-603">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="9553b-604">![Tıklayarak ilgili kod parçacığında listeden çekme](whats-new-in-aspnet-mvc-4/_static/image43.png "tıklayarak ilgili kod parçacığında listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="9553b-604">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="9553b-605">*Tıklayarak ilgili kod parçacığında listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="9553b-605">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="9553b-606">Ek B: Yükleme Web Visual Studio Express 2012 için</span><span class="sxs-lookup"><span data-stu-id="9553b-606">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="9553b-607">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak  **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="9553b-607">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="9553b-608">Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="9553b-608">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="9553b-609">Git [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="9553b-609">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="9553b-610">Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *Visual Studio Express 2012 için Windows Azure SDK'sı Web*&quot;.</span><span class="sxs-lookup"><span data-stu-id="9553b-610">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="9553b-611">Tıklayın **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="9553b-611">Click on **Install Now**.</span></span> <span data-ttu-id="9553b-612">Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="9553b-612">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="9553b-613">Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="9553b-613">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="9553b-614">![Visual Studio Express yükleme](whats-new-in-aspnet-mvc-4/_static/image44.png "yükleme Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="9553b-614">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="9553b-615">*Visual Studio Express yükleme*</span><span class="sxs-lookup"><span data-stu-id="9553b-615">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="9553b-616">Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="9553b-616">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşulları kabul ediliyor](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="9553b-618">*Lisans koşulları kabul ediliyor*</span><span class="sxs-lookup"><span data-stu-id="9553b-618">*Accepting the license terms*</span></span>
5. <span data-ttu-id="9553b-619">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="9553b-619">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="9553b-621">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="9553b-621">*Installation progress*</span></span>
6. <span data-ttu-id="9553b-622">Yükleme tamamlandığında tıklatın **son**.</span><span class="sxs-lookup"><span data-stu-id="9553b-622">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="9553b-624">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="9553b-624">*Installation completed*</span></span>
7. <span data-ttu-id="9553b-625">Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="9553b-625">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="9553b-626">Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.</span><span class="sxs-lookup"><span data-stu-id="9553b-626">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web döşemeye](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="9553b-628">*VS Express Web döşemeye*</span><span class="sxs-lookup"><span data-stu-id="9553b-628">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="9553b-629">Ek C: yükleme WebMatrix 2 ve iPhone benzeticisi</span><span class="sxs-lookup"><span data-stu-id="9553b-629">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="9553b-630">Bir sanal iPhone aygıt sitenizi çalıştırmak için WebMatrix genişletmesi kullanabilirsiniz &quot;iPhone elektrik mobil simülatörü&quot;.</span><span class="sxs-lookup"><span data-stu-id="9553b-630">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="9553b-631">Ayrıca, Visual Studio 2012'den simulator çalıştırmak için aynı uzantı yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-631">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="9553b-632">Görev 1 - yükleme WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="9553b-632">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="9553b-633">Git [ [https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="9553b-633">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="9553b-634">Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *WebMatrix 2*&quot;.</span><span class="sxs-lookup"><span data-stu-id="9553b-634">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*WebMatrix 2*&quot;.</span></span>
2. <span data-ttu-id="9553b-635">Tıklayın **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="9553b-635">Click on **Install Now**.</span></span> <span data-ttu-id="9553b-636">Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="9553b-636">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="9553b-637">Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="9553b-637">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="9553b-638">![WebMatrix 2 yükleme](whats-new-in-aspnet-mvc-4/_static/image49.png "yükleme WebMatrix 2")</span><span class="sxs-lookup"><span data-stu-id="9553b-638">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="9553b-639">*WebMatrix 2 yükleyin*</span><span class="sxs-lookup"><span data-stu-id="9553b-639">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="9553b-640">Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="9553b-640">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="9553b-641">![Lisans koşullarını kabul](whats-new-in-aspnet-mvc-4/_static/image50.png "lisans koşulları kabul ediliyor")</span><span class="sxs-lookup"><span data-stu-id="9553b-641">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="9553b-642">*Lisans koşulları kabul ediliyor*</span><span class="sxs-lookup"><span data-stu-id="9553b-642">*Accepting the license terms*</span></span>
5. <span data-ttu-id="9553b-643">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="9553b-643">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="9553b-644">![Yükleme ilerleme durumu](whats-new-in-aspnet-mvc-4/_static/image51.png "yükleme ilerleme durumu")</span><span class="sxs-lookup"><span data-stu-id="9553b-644">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="9553b-645">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="9553b-645">*Installation progress*</span></span>
6. <span data-ttu-id="9553b-646">Yükleme tamamlandığında tıklatın **son**.</span><span class="sxs-lookup"><span data-stu-id="9553b-646">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="9553b-647">![Yükleme tamamlandı](whats-new-in-aspnet-mvc-4/_static/image52.png "yüklemesi tamamlandı")</span><span class="sxs-lookup"><span data-stu-id="9553b-647">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="9553b-648">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="9553b-648">*Installation completed*</span></span>
7. <span data-ttu-id="9553b-649">Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="9553b-649">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="9553b-650">Görev 2 - iPhone benzeticisi uzantı yükleme</span><span class="sxs-lookup"><span data-stu-id="9553b-650">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="9553b-651">Çalıştırma **WebMatrix** ve varolan bir Web sitesini açın veya yeni bir tane oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9553b-651">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="9553b-652">Tıklatın **çalıştırmak** gelen düğmesini **giriş** Şerit ve seçin **yeni Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9553b-652">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="9553b-653">![Yeni WebMatrix genişletmesi ekleme](whats-new-in-aspnet-mvc-4/_static/image53.png "yeni WebMatrix genişletmesi ekleme")</span><span class="sxs-lookup"><span data-stu-id="9553b-653">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="9553b-654">*Yeni WebMatrix genişletmesi ekleme*</span><span class="sxs-lookup"><span data-stu-id="9553b-654">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="9553b-655">Seçin **iPhone benzeticisi** tıklatıp **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="9553b-655">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="9553b-656">![WebMatrix uzantıları gözatma](whats-new-in-aspnet-mvc-4/_static/image54.png "gözatma WebMatrix uzantıları")</span><span class="sxs-lookup"><span data-stu-id="9553b-656">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="9553b-657">*WebMatrix uzantıları gözatma*</span><span class="sxs-lookup"><span data-stu-id="9553b-657">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="9553b-658">Paket ayrıntılarını tıklatın **yükleme** uzantısını yükleme işlemine devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="9553b-658">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="9553b-659">![iPhone benzeticisi uzantısı](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone benzeticisi uzantısı")</span><span class="sxs-lookup"><span data-stu-id="9553b-659">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="9553b-660">*iPhone benzeticisi uzantısı*</span><span class="sxs-lookup"><span data-stu-id="9553b-660">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="9553b-661">Okuyun ve uzantı EULA kabul edin.</span><span class="sxs-lookup"><span data-stu-id="9553b-661">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="9553b-662">![WebMatrix genişletmesi EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix genişletmesi EULA")</span><span class="sxs-lookup"><span data-stu-id="9553b-662">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="9553b-663">*WebMatrix genişletmesi EULA*</span><span class="sxs-lookup"><span data-stu-id="9553b-663">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="9553b-664">Şimdi, iPhone benzeticisi seçeneğini kullanarak Web sitenizi Webmatrix'ten çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-664">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="9553b-665">![İPhone kullanarak çalışan](whats-new-in-aspnet-mvc-4/_static/image57.png "iPhone kullanarak çalıştırma")</span><span class="sxs-lookup"><span data-stu-id="9553b-665">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="9553b-666">*İPhone kullanarak çalıştırma*</span><span class="sxs-lookup"><span data-stu-id="9553b-666">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="9553b-667">Görev 3 - iPhone benzeticisi çalıştırmak için Visual Studio 2012 yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9553b-667">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="9553b-668">Açık **Visual Studio 2012** ve herhangi bir Web sitesini açın veya yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9553b-668">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="9553b-669">Çalışma düğmesinden aşağı oka tıklayın ve **ile Gözat**.</span><span class="sxs-lookup"><span data-stu-id="9553b-669">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="9553b-670">![İle Gözat](whats-new-in-aspnet-mvc-4/_static/image58.png "ile Gözat")</span><span class="sxs-lookup"><span data-stu-id="9553b-670">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="9553b-671">*İle Gözat*</span><span class="sxs-lookup"><span data-stu-id="9553b-671">*Browse with*</span></span>
3. <span data-ttu-id="9553b-672">İçinde &quot;Gözat ile&quot; iletişim kutusunda, tıklatın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9553b-672">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="9553b-673">İçinde &quot;Program Ekle&quot; iletişim kutusunda, aşağıdaki değerleri kullanın:</span><span class="sxs-lookup"><span data-stu-id="9553b-673">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

    - <span data-ttu-id="9553b-674">**Program**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(yolunu uygun şekilde güncelleştirin)*</span><span class="sxs-lookup"><span data-stu-id="9553b-674">**Program**: C:\Users\*{CurrentUser}*\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)*</span></span>
    - <span data-ttu-id="9553b-675">**Bağımsız değişkenler**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="9553b-675">**Arguments**: &quot;1&quot;</span></span>
    - <span data-ttu-id="9553b-676">**Kolay ad**: iPhone benzeticisi</span><span class="sxs-lookup"><span data-stu-id="9553b-676">**Friendly name**: iPhone Simulator</span></span>

    <span data-ttu-id="9553b-677">![Ekle program](whats-new-in-aspnet-mvc-4/_static/image59.png "Ekle programı")</span><span class="sxs-lookup"><span data-stu-id="9553b-677">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

    <span data-ttu-id="9553b-678">*İle göz atmak için program Ekle*</span><span class="sxs-lookup"><span data-stu-id="9553b-678">*Add program to browse with*</span></span>
5. <span data-ttu-id="9553b-679">Tıklatın **Tamam** ve iletişim kutularını kapatın.</span><span class="sxs-lookup"><span data-stu-id="9553b-679">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="9553b-680">Şimdi, Web uygulamalarınızı iPhone benzeticisi Visual Studio 2012'den çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="9553b-680">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="9553b-681">![İPhone benzeticisi ile Gözat](whats-new-in-aspnet-mvc-4/_static/image60.png "iPhone benzeticisi ile Gözat")</span><span class="sxs-lookup"><span data-stu-id="9553b-681">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="9553b-682">*İPhone benzeticisi ile Gözat*</span><span class="sxs-lookup"><span data-stu-id="9553b-682">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="9553b-683">Ek D: Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="9553b-683">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="9553b-684">Bu ekte Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturmak ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlamak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="9553b-684">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="9553b-685">Görev 1 - yeni bir Web sitesi oluşturma Windows Azure portalı</span><span class="sxs-lookup"><span data-stu-id="9553b-685">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="9553b-686">Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) ve aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="9553b-686">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9553b-687">Windows Azure ile 10 ASP.NET Web siteleri ücretsiz barındırma ve ardından trafiğiniz büyüdükçe ölçeğinizi.</span><span class="sxs-lookup"><span data-stu-id="9553b-687">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="9553b-688">Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="9553b-688">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="9553b-689">![Windows Azure portalında oturum açtığı](whats-new-in-aspnet-mvc-4/_static/image61.png "Windows Azure Portal'da oturum açın")</span><span class="sxs-lookup"><span data-stu-id="9553b-689">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="9553b-690">*Windows Azure yönetim portalında oturum açtığı*</span><span class="sxs-lookup"><span data-stu-id="9553b-690">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="9553b-691">Tıklatın **yeni** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="9553b-691">Click **New** on the command bar.</span></span>

    <span data-ttu-id="9553b-692">![Yeni bir Web sitesi oluşturma](whats-new-in-aspnet-mvc-4/_static/image62.png "yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9553b-692">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="9553b-693">*Yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9553b-693">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="9553b-694">Tıklatın **işlem** | **Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="9553b-694">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="9553b-695">Ardından **hızlı Oluştur** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9553b-695">Then select **Quick Create** option.</span></span> <span data-ttu-id="9553b-696">Yeni web sitesi için kullanılabilir bir URL girin ve tıklayın **Web sitesi oluştur**.</span><span class="sxs-lookup"><span data-stu-id="9553b-696">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9553b-697">Windows Azure Web sitesi, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="9553b-697">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="9553b-698">Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portal dışındaki dağıtmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="9553b-698">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="9553b-699">Bir veritabanını ayarlamak için adımları içermez.</span><span class="sxs-lookup"><span data-stu-id="9553b-699">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="9553b-700">![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](whats-new-in-aspnet-mvc-4/_static/image63.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9553b-700">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="9553b-701">*Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9553b-701">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="9553b-702">Yeni kadar bekleyin **Web sitesi** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9553b-702">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="9553b-703">Web sitesi oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun.</span><span class="sxs-lookup"><span data-stu-id="9553b-703">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="9553b-704">Yeni Web sitesi çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="9553b-704">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="9553b-705">![Yeni web sitesi için gözatma](whats-new-in-aspnet-mvc-4/_static/image64.png "yeni web sitesi için gözatma")</span><span class="sxs-lookup"><span data-stu-id="9553b-705">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="9553b-706">*Yeni web sitesi için gözatma*</span><span class="sxs-lookup"><span data-stu-id="9553b-706">*Browsing to the new web site*</span></span>

    <span data-ttu-id="9553b-707">![Çalışan Web sitesi](whats-new-in-aspnet-mvc-4/_static/image65.png "çalışan Web sitesi")</span><span class="sxs-lookup"><span data-stu-id="9553b-707">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="9553b-708">*Çalışan Web sitesi*</span><span class="sxs-lookup"><span data-stu-id="9553b-708">*Web site running*</span></span>
6. <span data-ttu-id="9553b-709">Portalına geri dönün ve web sitesi altında adına tıklayın **adı** yönetim sayfaları görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="9553b-709">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="9553b-710">![Web sitesi Yönetim sayfalarının açma](whats-new-in-aspnet-mvc-4/_static/image66.png "web sitesi Yönetim sayfalarının açma")</span><span class="sxs-lookup"><span data-stu-id="9553b-710">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="9553b-711">*Web Sitesi Yönetim sayfalarının açma*</span><span class="sxs-lookup"><span data-stu-id="9553b-711">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="9553b-712">İçinde **Pano** sayfasında **Hızlı Bakış** 'yi tıklatın **yayım profili indirin** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="9553b-712">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9553b-713">*Yayımlama profili* her etkin yayımlama yöntemi için bir Windows Azure Web sitesi bir web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="9553b-713">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="9553b-714">Yayımlama profili URL'leri, kullanıcı kimlik bilgilerini ve bağlanmak ve her bir yayımlama yönteminin etkinleştirildiği uç noktaları karşı kimlik doğrulaması için gerekli veritabanı dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="9553b-714">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="9553b-715">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma destek yayımlamak için bu programlar yapılandırılmasını otomatikleştirmek için profilleri Windows Azure Web siteleri için Web uygulamaları yayımlama.</span><span class="sxs-lookup"><span data-stu-id="9553b-715">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="9553b-716">![Yayımlama profili web sitesi Yükleniyor](whats-new-in-aspnet-mvc-4/_static/image67.png "yayımlama profili web sitesi yükleniyor")</span><span class="sxs-lookup"><span data-stu-id="9553b-716">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="9553b-717">*Yayımlama profili Web sitesi yükleniyor*</span><span class="sxs-lookup"><span data-stu-id="9553b-717">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="9553b-718">Yayımlama profili dosyasını bilinen bir konuma indirin.</span><span class="sxs-lookup"><span data-stu-id="9553b-718">Download the publish profile file to a known location.</span></span> <span data-ttu-id="9553b-719">Daha fazla Bu alıştırmada Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlamak için bu dosyayı kullanmak nasıl göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9553b-719">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="9553b-720">![Yayımlama profili dosyasını kaydetme](whats-new-in-aspnet-mvc-4/_static/image68.png "yayımlama profilini kaydetme")</span><span class="sxs-lookup"><span data-stu-id="9553b-720">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="9553b-721">*Yayımlama profili dosyasını kaydetme*</span><span class="sxs-lookup"><span data-stu-id="9553b-721">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="9553b-722">Görev 2 - veritabanı sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9553b-722">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="9553b-723">Uygulamanızı SQL Server'ın kullanmak yaparsa veritabanlarının bir SQL veritabanı sunucusu oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9553b-723">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="9553b-724">SQL Server kullanmayan basit bir uygulamayı dağıtmak istiyorsanız, bu görevi atlamak.</span><span class="sxs-lookup"><span data-stu-id="9553b-724">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="9553b-725">Uygulama veritabanını depolamak için bir SQL veritabanı sunucusu gerekir.</span><span class="sxs-lookup"><span data-stu-id="9553b-725">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="9553b-726">Aboneliğiniz Windows Azure Yönetim Portalı'nda SQL veritabanı sunucularının görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**.</span><span class="sxs-lookup"><span data-stu-id="9553b-726">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="9553b-727">Oluşturulan bir sunucu yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğundan düğme.</span><span class="sxs-lookup"><span data-stu-id="9553b-727">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="9553b-728">Not edin **sunucu adı ve URL, yönetici oturum açma adı ve parola**sonraki görevleri kullanacağı gibi.</span><span class="sxs-lookup"><span data-stu-id="9553b-728">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="9553b-729">Daha sonraki bir aşamada oluşturulacak şekilde veritabanı henüz oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="9553b-729">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="9553b-730">![SQL veritabanı sunucusu Pano](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL veritabanı sunucu Panosu")</span><span class="sxs-lookup"><span data-stu-id="9553b-730">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="9553b-731">*SQL veritabanı sunucu Panosu*</span><span class="sxs-lookup"><span data-stu-id="9553b-731">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="9553b-732">İşlemin sonraki görev, sunucunun listesinde, yerel IP adresi içermesi gereken bu nedenle veritabanı bağlantısı Visual Studio'dan test edecek **izin verilen IP adreslerini**.</span><span class="sxs-lookup"><span data-stu-id="9553b-732">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="9553b-733">Bunu yapmak için tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** üzerinde yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) düğmesi.</span><span class="sxs-lookup"><span data-stu-id="9553b-733">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![İstemci IP adresi ekleme](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="9553b-735">*İstemci IP adresi ekleme*</span><span class="sxs-lookup"><span data-stu-id="9553b-735">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="9553b-736">Bir kez **istemci IP adresi** izin verilen IP adreslerine eklenen listesinde, tıklayın **kaydetmek** değişiklikleri onaylamak için.</span><span class="sxs-lookup"><span data-stu-id="9553b-736">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Değişiklikleri onaylamak](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="9553b-738">*Değişiklikleri onaylamak*</span><span class="sxs-lookup"><span data-stu-id="9553b-738">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="9553b-739">Görev 3 - Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="9553b-739">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="9553b-740">ASP.NET MVC 4 çözüme geri dönün.</span><span class="sxs-lookup"><span data-stu-id="9553b-740">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="9553b-741">İçinde **Çözüm Gezgini**, web sitesi projesine sağ tıklatın ve **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="9553b-741">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="9553b-742">![Uygulama yayımlama](whats-new-in-aspnet-mvc-4/_static/image73.png "uygulama yayımlama")</span><span class="sxs-lookup"><span data-stu-id="9553b-742">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="9553b-743">*Web sitesi yayımlama*</span><span class="sxs-lookup"><span data-stu-id="9553b-743">*Publishing the web site*</span></span>
2. <span data-ttu-id="9553b-744">İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="9553b-744">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="9553b-745">![Yayımlama profilini içeri aktarma](whats-new-in-aspnet-mvc-4/_static/image74.png "yayımlama profilini içeri aktarma")</span><span class="sxs-lookup"><span data-stu-id="9553b-745">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="9553b-746">*Yayımlama profilini içeri aktarma*</span><span class="sxs-lookup"><span data-stu-id="9553b-746">*Importing publish profile*</span></span>
3. <span data-ttu-id="9553b-747">Tıklatın **bağlantısı doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="9553b-747">Click **Validate Connection**.</span></span> <span data-ttu-id="9553b-748">Doğrulama tamamlandıktan sonra tıklayın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="9553b-748">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9553b-749">Yanındaki bağlantıyı doğrula düğmesi görünür yeşil bir onay işareti gördüğünüzde doğrulama tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="9553b-749">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="9553b-750">![Bağlantı doğrulama](whats-new-in-aspnet-mvc-4/_static/image75.png "bağlantısı doğrulanıyor")</span><span class="sxs-lookup"><span data-stu-id="9553b-750">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="9553b-751">*Doğrulama bağlantısı*</span><span class="sxs-lookup"><span data-stu-id="9553b-751">*Validating connection*</span></span>
4. <span data-ttu-id="9553b-752">İçinde **ayarları** sayfasında **veritabanları** bölümünde, veritabanı bağlantının textbox yanındaki düğmesini tıklatın (yani **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="9553b-752">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="9553b-753">![Web dağıtımı yapılandırma](whats-new-in-aspnet-mvc-4/_static/image76.png "Web dağıtımı yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="9553b-753">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="9553b-754">*Web dağıtımı yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="9553b-754">*Web deploy configuration*</span></span>
5. <span data-ttu-id="9553b-755">Veritabanı bağlantısı aşağıdaki gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="9553b-755">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="9553b-756">İçinde **sunucu adı** , SQL veritabanı sunucusu URL'yi kullanarak yazın *tcp:* öneki.</span><span class="sxs-lookup"><span data-stu-id="9553b-756">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="9553b-757">İçinde **kullanıcı adı** sunucunuzun yönetici oturum açma adını yazın.</span><span class="sxs-lookup"><span data-stu-id="9553b-757">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="9553b-758">İçinde **parola** sunucu yönetici oturum açma parolasını yazın.</span><span class="sxs-lookup"><span data-stu-id="9553b-758">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="9553b-759">Yeni bir veritabanı adı girin: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="9553b-759">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="9553b-760">![Hedef bağlantı dizesi yapılandırma](whats-new-in-aspnet-mvc-4/_static/image77.png "hedef bağlantı dizesi yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="9553b-760">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="9553b-761">*Hedef bağlantı dizesi yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="9553b-761">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="9553b-762">Sonra **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="9553b-762">Then click **OK**.</span></span> <span data-ttu-id="9553b-763">Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.</span><span class="sxs-lookup"><span data-stu-id="9553b-763">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="9553b-764">![Veritabanı oluşturma](whats-new-in-aspnet-mvc-4/_static/image78.png "veritabanı dizesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9553b-764">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="9553b-765">*Veritabanı oluşturuluyor*</span><span class="sxs-lookup"><span data-stu-id="9553b-765">*Creating the database*</span></span>
7. <span data-ttu-id="9553b-766">Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini varsayılan bağlantı textbox içinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="9553b-766">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="9553b-767">Sonra **İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="9553b-767">Then click **Next**.</span></span>

    <span data-ttu-id="9553b-768">![SQL veritabanına işaret eden bağlantı dizesi](whats-new-in-aspnet-mvc-4/_static/image79.png "SQL veritabanına işaret eden bağlantı dizesi")</span><span class="sxs-lookup"><span data-stu-id="9553b-768">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="9553b-769">*SQL veritabanına işaret eden bağlantı dizesi*</span><span class="sxs-lookup"><span data-stu-id="9553b-769">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="9553b-770">İçinde **Önizleme** sayfasında, **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="9553b-770">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="9553b-771">![Web uygulaması yayımlama](whats-new-in-aspnet-mvc-4/_static/image80.png "web uygulaması yayımlama")</span><span class="sxs-lookup"><span data-stu-id="9553b-771">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="9553b-772">*Web uygulaması yayımlama*</span><span class="sxs-lookup"><span data-stu-id="9553b-772">*Publishing the web application*</span></span>
9. <span data-ttu-id="9553b-773">Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesini açın.</span><span class="sxs-lookup"><span data-stu-id="9553b-773">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="9553b-774">![Uygulama için Windows Azure yayımlanan](whats-new-in-aspnet-mvc-4/_static/image81.png "uygulama için Windows Azure yayımlanır")</span><span class="sxs-lookup"><span data-stu-id="9553b-774">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="9553b-775">*Windows Azure için yayımlanan uygulama*</span><span class="sxs-lookup"><span data-stu-id="9553b-775">*Application published to Windows Azure*</span></span>
