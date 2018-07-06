---
uid: mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
title: ASP.NET MVC 4 sürümündeki yenilikler | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 4 tanınmış tasarım desenleri ve ASP.NET gücünü kullanarak ölçeklenebilir, standartlara dayanan web uygulamaları oluşturmaya yönelik bir çerçeve olan ve...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 48f7feb3-872f-485d-b96f-e30011ff8c4a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/whats-new-in-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 718a31de3d2d60788ba4affb0463a4ae871ef89a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805356"
---
# <a name="whats-new-in-aspnet-mvc-4"></a><span data-ttu-id="6518e-103">ASP.NET MVC 4 sürümündeki yenilikler</span><span class="sxs-lookup"><span data-stu-id="6518e-103">What's New in ASP.NET MVC 4</span></span>

<span data-ttu-id="6518e-104">Tarafından [Team Web Kampları](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="6518e-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="6518e-105">Eğitim Seti Web Kampları indirin</span><span class="sxs-lookup"><span data-stu-id="6518e-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="6518e-106">ASP.NET MVC 4, tanınmış tasarım desenleri ve ASP.NET ve .NET framework gücünü kullanarak ölçeklenebilir, standartlara dayanan web uygulamaları oluşturmaya yönelik bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="6518e-106">ASP.NET MVC 4 is a framework for building scalable, standards-based web applications using well-established design patterns and the power of the ASP.NET and the .NET framework.</span></span> <span data-ttu-id="6518e-107">Bu yeni, dördüncü framework sürümü, mobil web uygulaması geliştirmeyi daha kolay hale odaklanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6518e-107">This new, fourth version of the framework focuses on making mobile web application development easier.</span></span>

<span data-ttu-id="6518e-108">Başlangıç olarak, yeni bir ASP.NET MVC 4 projesi oluşturduğunuzda var. Şimdi mobil cihazlar için özel olarak tek başına uygulama derlemek için kullanabileceğiniz bir mobil uygulaması proje şablonu</span><span class="sxs-lookup"><span data-stu-id="6518e-108">To begin with, when you create a new ASP.NET MVC 4 project there is now a mobile application project template you can use to build a standalone app specifically for mobile devices.</span></span> <span data-ttu-id="6518e-109">Buna ek olarak, ASP.NET MVC 4 jQuery.Mobile.MVC NuGet paketi aracılığıyla jQuery Mobile ile entegre olur.</span><span class="sxs-lookup"><span data-stu-id="6518e-109">Additionally, ASP.NET MVC 4 integrates with jQuery Mobile through a jQuery.Mobile.MVC NuGet package.</span></span> <span data-ttu-id="6518e-110">jQuery Mobile, Windows Phone, iPhone, Android vb. dahil olmak üzere tüm popüler mobil cihaz platformları, uyumlu olan web uygulamaları geliştirmek için bir HTML5 tabanlı çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="6518e-110">jQuery Mobile is an HTML5-based framework for developing web apps that are compatible with all popular mobile device platforms, including Windows Phone, iPhone, Android and so on.</span></span> <span data-ttu-id="6518e-111">Özelleştirmesi gerekiyorsa, Bununla birlikte, ASP.NET MVC 4 ayrıca farklı cihazlar için farklı görünümleri hizmet ve cihaza özgü iyileştirmeleri sağlamanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="6518e-111">However, if you need specialization, ASP.NET MVC 4 also enables you to serve different views for different devices and provide device-specific optimizations.</span></span>

<span data-ttu-id="6518e-112">Bu uygulamalı bir laboratuvarda, ASP.NET MVC 4 ile başlar &quot;Internet uygulaması&quot; bir Fotoğraf Galerisi uygulaması oluşturmak için proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="6518e-112">In this hands-on lab, you will start with the ASP.NET MVC 4 &quot;Internet Application&quot; project template to create a Photo Gallery application.</span></span> <span data-ttu-id="6518e-113">Farklı mobil cihaz ve masaüstü web tarayıcıları ile uyumlu hale getirmek için jQuery Mobile ve ASP.NET MVC 4'ün yeni özelliklerini kullanarak uygulamayı kademeli olarak artırır.</span><span class="sxs-lookup"><span data-stu-id="6518e-113">You will progressively enhance the app using jQuery Mobile and ASP.NET MVC 4's new features to make it compatible with different mobile devices and desktop web browsers.</span></span> <span data-ttu-id="6518e-114">Kod oluşturma ve nasıl ASP.NET MVC 4 görev destekleyerek zaman uyumsuz eylem yöntemi yazmanızı kolaylaştırır için yeni kod tarifleri hakkında bilgi edineceksiniz&lt;ActionResult&gt; dönüş türü.</span><span class="sxs-lookup"><span data-stu-id="6518e-114">You will also learn about new code recipes for code generation and how ASP.NET MVC 4 makes it easier for you to write asynchronous action methods by supporting Task&lt;ActionResult&gt; return types.</span></span>

> [!NOTE]
> <span data-ttu-id="6518e-115">Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [Microsoft-Web/WebCampTrainingKit yayınlar](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="6518e-115">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="6518e-116">Bu Laboratuvar için belirli proje kullanılabilir [ASP.NET 4.5 Web Forms yenilikleri](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).</span><span class="sxs-lookup"><span data-stu-id="6518e-116">The project specific to this lab is available at [What's New in Web Forms in ASP.NET 4.5](https://github.com/Microsoft-Web/HOL-ASPNETWebForms).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="6518e-117">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="6518e-117">Objectives</span></span>

<span data-ttu-id="6518e-118">Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="6518e-118">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="6518e-119">ASP.NET MVC proje şablonları-dahil olmak üzere yeni mobil uygulama projesi şablon geliştirmeleri avantajlarından yararlanın</span><span class="sxs-lookup"><span data-stu-id="6518e-119">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="6518e-120">Mobil cihazlarda görünen geliştirmek için CSS medya sorgular ve HTML5 Görünüm penceresi özniteliği kullanın</span><span class="sxs-lookup"><span data-stu-id="6518e-120">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="6518e-121">JQuery Mobile için aşamalı geliştirmeleri ve dokunma özelliği iyileştirilmiş web kullanıcı Arabirimi oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="6518e-121">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="6518e-122">Mobile özel görünümlerini oluşturma</span><span class="sxs-lookup"><span data-stu-id="6518e-122">Create mobile-specific views</span></span>
- <span data-ttu-id="6518e-123">Uygulamasında mobil ve Masaüstü görünüm arasında geçiş yapmak için Görünüm değiştirici bileşenini kullan</span><span class="sxs-lookup"><span data-stu-id="6518e-123">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="6518e-124">Görev desteğini kullanarak zaman uyumsuz denetleyicileri oluşturma</span><span class="sxs-lookup"><span data-stu-id="6518e-124">Create asynchronous controllers using task support</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="6518e-125">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="6518e-125">Prerequisites</span></span>

<span data-ttu-id="6518e-126">Bu laboratuvarı tamamlamak için aşağıdakiler olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="6518e-126">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="6518e-127">[Web için Visual Studio Express 2012 Microsoft](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) veya üst (okuma [ek B](#AppendixB) nasıl yükleneceği hakkında yönergeler için).</span><span class="sxs-lookup"><span data-stu-id="6518e-127">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix B](#AppendixB) for instructions on how to install it).</span></span>
- <span data-ttu-id="6518e-128">[ASP.NET MVC 4](../../../mvc4.md) (Microsoft Visual Studio 2012 yüklemesine dahil)</span><span class="sxs-lookup"><span data-stu-id="6518e-128">[ASP.NET MVC 4](../../../mvc4.md) (included in the Microsoft Visual Studio 2012 installation)</span></span>
- <span data-ttu-id="6518e-129">Windows Phone öykünücüsü'nü (dahil [7.1.1 Windows Phone SDK'sı](https://www.microsoft.com/download/details.aspx?id=29233))</span><span class="sxs-lookup"><span data-stu-id="6518e-129">Windows Phone Emulator (included in the [Windows Phone 7.1.1 SDK](https://www.microsoft.com/download/details.aspx?id=29233))</span></span>
- <span data-ttu-id="6518e-130">İsteğe bağlı - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) ile **Electric Plum iPhone simülatörü** uzatma (yalnızca web uygulaması ile bir iPhone benzeticisi göz atmak için kullanılan alıştırma 3)</span><span class="sxs-lookup"><span data-stu-id="6518e-130">Optional - [WebMatrix 2](https://www.microsoft.com/web/webmatrix/) with **Electric Plum iPhone Simulator** extension (only for Exercise 3 used to browse the web application with an iPhone simulator)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="6518e-131">Kurulum</span><span class="sxs-lookup"><span data-stu-id="6518e-131">Setup</span></span>

<span data-ttu-id="6518e-132">Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-132">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="6518e-133">Kolaylık olması için bu kodu çoğu Visual Studio kodu Visual Studio içinden el ile eklemek zorunda kalmamak için kullanabileceğiniz kod parçacıkları sağlanır.</span><span class="sxs-lookup"><span data-stu-id="6518e-133">For your convenience, most of that code is provided as Visual Studio Code Snippets, which you can use from within Visual Studio to avoid having to add it manually.</span></span>

<span data-ttu-id="6518e-134">Kod parçacıkları yüklemek için:</span><span class="sxs-lookup"><span data-stu-id="6518e-134">To install the code snippets:</span></span>

1. <span data-ttu-id="6518e-135">Bir Windows Explorer penceresi açın ve Laboratuvar için Gözat **Source\Setup** klasör.</span><span class="sxs-lookup"><span data-stu-id="6518e-135">Open a Windows Explorer window and browse to the lab's **Source\Setup** folder.</span></span>
2. <span data-ttu-id="6518e-136">Çift **Setup.cmd** Visual Studio kod parçacıkları yüklemek için bu klasördeki bir dosya.</span><span class="sxs-lookup"><span data-stu-id="6518e-136">Double-click the **Setup.cmd** file in this folder to install the Visual Studio code snippets.</span></span>

<span data-ttu-id="6518e-137">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istediğiniz konusunda bilgi sahibi değilseniz, bu belge, ek başvurabilir &quot; [ek A: kullanarak kod parçacıklarına](#AppendixA)&quot;.</span><span class="sxs-lookup"><span data-stu-id="6518e-137">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix A: Using Code Snippets](#AppendixA)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="6518e-138">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="6518e-138">Exercises</span></span>

<span data-ttu-id="6518e-139">Bu uygulamalı laboratuvarı aşağıdaki alıştırmaları içerir:</span><span class="sxs-lookup"><span data-stu-id="6518e-139">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="6518e-140">Yeni ASP.NET MVC 4 proje şablonları</span><span class="sxs-lookup"><span data-stu-id="6518e-140">New ASP.NET MVC 4 Project Templates</span></span>](#Exercise1)
2. [<span data-ttu-id="6518e-141">Fotoğraf Galerisi Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="6518e-141">Creating the Photo Gallery Web Application</span></span>](#Exercise2)
3. [<span data-ttu-id="6518e-142">Mobil cihazlar için destek ekleme</span><span class="sxs-lookup"><span data-stu-id="6518e-142">Adding Support for Mobile Devices</span></span>](#Exercise3)
4. [<span data-ttu-id="6518e-143">Zaman uyumsuz denetleyicilerini kullanma</span><span class="sxs-lookup"><span data-stu-id="6518e-143">Using Asynchronous Controllers</span></span>](#Exercise4)

> [!NOTE]
> <span data-ttu-id="6518e-144">Her bir alıştırma olarak sunulduğu bir **son** elde alıştırmalar tamamladıktan sonra ortaya çıkan çözüm içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="6518e-144">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="6518e-145">Çalışma alıştırmaları ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-145">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="6518e-146">Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**.</span><span class="sxs-lookup"><span data-stu-id="6518e-146">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_New_ASPNET_MVC_4_Project_Templates"></a>
### <a name="exercise-1-new-aspnet-mvc-4-project-templates"></a><span data-ttu-id="6518e-147">Alıştırma 1: Yeni ASP.NET MVC 4 proje şablonları</span><span class="sxs-lookup"><span data-stu-id="6518e-147">Exercise 1: New ASP.NET MVC 4 Project Templates</span></span>

<span data-ttu-id="6518e-148">Bu alıştırmada, ASP.NET MVC 4 proje şablonları geliştirmeleri inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-148">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 Project templates.</span></span> <span data-ttu-id="6518e-149">Internet uygulaması şablonu ek olarak, MVC 3, zaten var. bu sürümü artık mobil uygulamalar için ayrı bir şablon içerir.</span><span class="sxs-lookup"><span data-stu-id="6518e-149">In addition to the Internet Application template, already present in MVC 3, this version now includes a separate template for Mobile applications.</span></span> <span data-ttu-id="6518e-150">İlk olarak, ilgili bazı özellikleri şablonlarının her biri, görünecektir.</span><span class="sxs-lookup"><span data-stu-id="6518e-150">First, you will look at some relevant features of each of the templates.</span></span> <span data-ttu-id="6518e-151">Ardından, sayfanız farklı platformlarda düzgün bir şekilde doğru yaklaşımı kullanarak işleme üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="6518e-151">Then, you will work on rendering your page properly on the different platforms by using the right approach.</span></span>

<a id="Task_1_-_Exploring_the_Internet_Application_Template"></a>
#### <a name="task-1---exploring-the-internet-application-template"></a><span data-ttu-id="6518e-152">Görev 1 - Internet uygulama şablonu keşfetme</span><span class="sxs-lookup"><span data-stu-id="6518e-152">Task 1 - Exploring the Internet Application Template</span></span>

1. <span data-ttu-id="6518e-153">Açık **Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="6518e-153">Open **Visual Studio**.</span></span>
2. <span data-ttu-id="6518e-154">Seçin **dosya | Yeni | Proje** menü komutu.</span><span class="sxs-lookup"><span data-stu-id="6518e-154">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="6518e-155">İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** Şablonu'nu seçin ve ağaç **ASP.NET MVC 4 Web uygulaması.**</span><span class="sxs-lookup"><span data-stu-id="6518e-155">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="6518e-156">Projeyi adlandırın **Fotografgalerisi**, bir konum seçin (veya varsayılan değeri bırakın) ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="6518e-156">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6518e-157">Daha sonra artık oluşturmakta olduğunuz Fotografgalerisi ASP.NET MVC 4 çözüm özelleştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-157">You will later customize the PhotoGallery ASP.NET MVC 4 solution you are now creating.</span></span>

    <span data-ttu-id="6518e-158">![Yeni proje oluşturma](whats-new-in-aspnet-mvc-4/_static/image1.png "yeni proje oluşturma")</span><span class="sxs-lookup"><span data-stu-id="6518e-158">![Creating a new project](whats-new-in-aspnet-mvc-4/_static/image1.png "Creating a new project")</span></span>

    <span data-ttu-id="6518e-159">*Yeni proje oluşturma*</span><span class="sxs-lookup"><span data-stu-id="6518e-159">*Creating a new project*</span></span>
3. <span data-ttu-id="6518e-160">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulaması** proje şablonu ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="6518e-160">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="6518e-161">Razor görünüm altyapısı seçtiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="6518e-161">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="6518e-162">![Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma](whats-new-in-aspnet-mvc-4/_static/image2.png "yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma")</span><span class="sxs-lookup"><span data-stu-id="6518e-162">![Creating a new ASP.NET MVC 4 Internet Application](whats-new-in-aspnet-mvc-4/_static/image2.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="6518e-163">*Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma*</span><span class="sxs-lookup"><span data-stu-id="6518e-163">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="6518e-164">Razor sözdizimi, ASP.NET MVC 3'te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="6518e-164">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="6518e-165">Karakterler ve tuş vuruşları gerekli bir dosya içinde hızlı ve iş akışı kodlama sıvı etkinleştirme sayısını en aza indirmek için hedefi sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="6518e-165">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="6518e-166">Razor yararlanır mevcut C# / VB (veya diğer) dil becerilerine ve harika bir HTML oluşturma iş akışı sağlayan bir şablon işaretleme söz dizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="6518e-166">Razor leverages existing C# / VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="6518e-167">Tuşuna **F5** yenilenen şablonları görmek ve çözümü çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6518e-167">Press **F5** to run the solution and see the renewed templates.</span></span> <span data-ttu-id="6518e-168">Aşağıdaki özellikleri kontrol edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="6518e-168">You can check out the following features:</span></span>

    <span data-ttu-id="6518e-169">**Modern stili şablonları**</span><span class="sxs-lookup"><span data-stu-id="6518e-169">**Modern-style templates**</span></span>

    <span data-ttu-id="6518e-170">Şablonlar, daha fazla modern görünümlü stilleri sağlama yenilenmiş.</span><span class="sxs-lookup"><span data-stu-id="6518e-170">The templates have been renewed, providing more modern-looking styles.</span></span>

    <span data-ttu-id="6518e-171">![ASP.NET MVC 4 restyled şablonları](whats-new-in-aspnet-mvc-4/_static/image3.png "şablonları restyled MVC 4")</span><span class="sxs-lookup"><span data-stu-id="6518e-171">![ASP.NET MVC 4 restyled templates](whats-new-in-aspnet-mvc-4/_static/image3.png "MVC 4 restyled templates")</span></span>

    <span data-ttu-id="6518e-172">*ASP.NET MVC 4 restyled şablonları*</span><span class="sxs-lookup"><span data-stu-id="6518e-172">*ASP.NET MVC 4 restyled templates*</span></span>

    <span data-ttu-id="6518e-173">![Yeni kişi sayfası](whats-new-in-aspnet-mvc-4/_static/image4.png "yeni kişi sayfası")</span><span class="sxs-lookup"><span data-stu-id="6518e-173">![New Contact page](whats-new-in-aspnet-mvc-4/_static/image4.png "New Contact page")</span></span>

    <span data-ttu-id="6518e-174">*Yeni kişi sayfası*</span><span class="sxs-lookup"><span data-stu-id="6518e-174">*New Contact page*</span></span>

    <span data-ttu-id="6518e-175">**Uyarlamalı işleme**</span><span class="sxs-lookup"><span data-stu-id="6518e-175">**Adaptive Rendering**</span></span>

    <span data-ttu-id="6518e-176">Tarayıcı penceresini yeniden boyutlandırmadan kullanıma denetleyin ve sayfa düzeni için yeni pencere boyutunu dinamik olarak Uyarlanır nasıl dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-176">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="6518e-177">Bu şablonlar, hem Masaüstü hem de mobil platformlarda özelleştirme olmadan düzgün bir şekilde işlemek için Uyarlamalı işleme tekniği kullanın.</span><span class="sxs-lookup"><span data-stu-id="6518e-177">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

    <span data-ttu-id="6518e-178">![ASP.NET MVC 4 proje şablonu farklı tarayıcı boyutlarda](whats-new-in-aspnet-mvc-4/_static/image5.png "farklı tarayıcı boyutlarda ASP.NET MVC 4 proje şablonu")</span><span class="sxs-lookup"><span data-stu-id="6518e-178">![ASP.NET MVC 4 project template in different browser sizes](whats-new-in-aspnet-mvc-4/_static/image5.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

    <span data-ttu-id="6518e-179">*ASP.NET MVC 4 Proje şablonunda farklı tarayıcı boyutları*</span><span class="sxs-lookup"><span data-stu-id="6518e-179">*ASP.NET MVC 4 project template in different browser sizes*</span></span>

    <span data-ttu-id="6518e-180">**JavaScript ile daha zengin kullanıcı Arabirimi**</span><span class="sxs-lookup"><span data-stu-id="6518e-180">**Richer UI with JavaScript**</span></span>

    <span data-ttu-id="6518e-181">Varsayılan proje şablonları için başka bir geliştirme, JavaScript daha etkileşimli bir JavaScript sağlamak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6518e-181">Another enhancement to default project templates is the use of JavaScript to provide a more interactive JavaScript.</span></span> <span data-ttu-id="6518e-182">Şablonda kullanılan oturum açma ve kaydetme bağlantıları nasıl doğrulamaları jQuery istemci-tarafı giriş alanları doğrulamada exemplify.</span><span class="sxs-lookup"><span data-stu-id="6518e-182">The Login and Register links used in the template exemplify how to use the jQuery Validations to validate the input fields from client-side.</span></span>

    ![jQuery doğrulaması](whats-new-in-aspnet-mvc-4/_static/image6.png)

    <span data-ttu-id="6518e-184">*jQuery doğrulaması*</span><span class="sxs-lookup"><span data-stu-id="6518e-184">*jQuery Validation*</span></span>

    > [!NOTE]
    > <span data-ttu-id="6518e-185">Bildirim, ilk bölümü olarak bölümlerde iki günlük sitesinden kayıtlı hesabıyla ve google (varsayılan olarak devre dışı) gibi başka bir kimlik doğrulama hizmeti kullanarak altenativelly oturum ikinci bölümü oturum açabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-185">Notice the two log in sections, in the first section you can log in using a registerd account from the site and in the second section you can altenativelly log in using another authentication service like google (disabled by default).</span></span>
5. <span data-ttu-id="6518e-186">Hata ayıklayıcıyı durdurduktan ve Visual Studio'ya dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="6518e-186">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="6518e-187">Dosyayı açmak **AuthConfig.cs** altında bulunan **uygulama\_Başlat** klasör.</span><span class="sxs-lookup"><span data-stu-id="6518e-187">Open the file **AuthConfig.cs** located under the **App\_Start** folder.</span></span>
7. <span data-ttu-id="6518e-188">İçin Google istemcisini kaydetmek için son satırından açıklamayı kaldırın *OAuth* kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="6518e-188">Remove the comment from the last line to register Google client for *OAuth* authentication.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample1.cs)]

    > [!NOTE]
    > <span data-ttu-id="6518e-189">Herhangi bir Openıd veya OAuth hizmeti Facebook, Twitter, Microsoft, vb. kullanarak kimlik doğrulamasını kolayca etkinleştirebilirsiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-189">Notice you can easily enable authentication using any OpenID or OAuth service like Facebook, Twitter, Microsoft, etc.</span></span>
8. <span data-ttu-id="6518e-190">Tuşuna **F5** çözümü çalıştırın ve oturum açma sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="6518e-190">Press **F5** to run the solution and navigate to the login page.</span></span>
9. <span data-ttu-id="6518e-191">Seçin **Google** oturum açmak için hizmet.</span><span class="sxs-lookup"><span data-stu-id="6518e-191">Select **Google** service to log in.</span></span>

    ![Günlük hizmetini seçme](whats-new-in-aspnet-mvc-4/_static/image7.png)

    <span data-ttu-id="6518e-193">*Günlük hizmetini seçme*</span><span class="sxs-lookup"><span data-stu-id="6518e-193">*Selecting the log in service*</span></span>
10. <span data-ttu-id="6518e-194">Google hesabınızı kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="6518e-194">Log in using your Google account.</span></span>
11. <span data-ttu-id="6518e-195">Google hesabı bilgilerini almak site (localhost) sağlar.</span><span class="sxs-lookup"><span data-stu-id="6518e-195">Allow the site (localhost) to retrieve information from Google account.</span></span>
12. <span data-ttu-id="6518e-196">Son olarak, Google hesabıyla ilişkilendirmek için sitesine kaydetmek gerekir.</span><span class="sxs-lookup"><span data-stu-id="6518e-196">Finally, you will have to register in the site to associate the Google account.</span></span>

   ![Google hesabınızı ilişkilendirin](whats-new-in-aspnet-mvc-4/_static/image8.png)

   <span data-ttu-id="6518e-198">*Google hesabınızı ilişkilendirme*</span><span class="sxs-lookup"><span data-stu-id="6518e-198">*Associating your Google account*</span></span>
13. <span data-ttu-id="6518e-199">Hata ayıklayıcıyı durdurduktan ve Visual Studio'ya dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="6518e-199">Close the browser to stop the debugger and return to Visual Studio.</span></span>
14. <span data-ttu-id="6518e-200">ASP.NET MVC 4 Proje şablonunda sunulan diğer bazı yeni özellikleri kullanıma çözümü şimdi keşfedin.</span><span class="sxs-lookup"><span data-stu-id="6518e-200">Now explore the solution to check out some other new features introduced by ASP.NET MVC 4 in the project template.</span></span>

   <span data-ttu-id="6518e-201">![ASP.NET MVC 4 Internet uygulaması proje şablonu](whats-new-in-aspnet-mvc-4/_static/image9.png "ASP.NET MVC 4 Internet uygulaması proje şablonu")</span><span class="sxs-lookup"><span data-stu-id="6518e-201">![The ASP.NET MVC 4 Internet Application Project Template](whats-new-in-aspnet-mvc-4/_static/image9.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

   <span data-ttu-id="6518e-202">*ASP.NET MVC 4 Internet uygulaması proje şablonu*</span><span class="sxs-lookup"><span data-stu-id="6518e-202">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   - <span data-ttu-id="6518e-203">**HTML 5 biçimlendirme**</span><span class="sxs-lookup"><span data-stu-id="6518e-203">**HTML 5 Markup**</span></span>

       <span data-ttu-id="6518e-204">Yeni tema biçimlendirme bulabilmek için şablon görünümleri göz atın.</span><span class="sxs-lookup"><span data-stu-id="6518e-204">Browse template views to find out the new theme markup.</span></span>

       <span data-ttu-id="6518e-205">![Razor ve HTML5 biçimlendirme About.cshtml kullanarak yeni şablonu. ](whats-new-in-aspnet-mvc-4/_static/image10.png "Razor ve HTML5 biçimlendirme About.cshtml kullanarak yeni şablonu.")</span><span class="sxs-lookup"><span data-stu-id="6518e-205">![New template, using Razor and HTML5 markup About.cshtml.](whats-new-in-aspnet-mvc-4/_static/image10.png "New template, using Razor and HTML5 markup About.cshtml.")</span></span>

       <span data-ttu-id="6518e-206">*Razor ve HTML5 biçimlendirme (About.cshtml) kullanarak yeni şablon.*</span><span class="sxs-lookup"><span data-stu-id="6518e-206">*New template, using Razor and HTML5 markup (About.cshtml).*</span></span>
   - <span data-ttu-id="6518e-207">**Güncelleştirilmiş JavaScript kitaplıkları**</span><span class="sxs-lookup"><span data-stu-id="6518e-207">**Updated JavaScript libraries**</span></span>

       <span data-ttu-id="6518e-208">ASP.NET MVC 4 varsayılan şablonu artık KnockoutJS, zengin oluşturmanızı sağlayan bir JavaScript MVVM çerçeve ve JavaScript ve HTML kullanarak yüksek derecede yanıt veren web uygulamaları içerir.</span><span class="sxs-lookup"><span data-stu-id="6518e-208">The ASP.NET MVC 4 default template now includes KnockoutJS, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="6518e-209">Gibi MVC3 jQuery ve jQuery UI kitaplıkları da ASP.NET MVC 4'te dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="6518e-209">Like in MVC3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

     > [!NOTE]
     > <span data-ttu-id="6518e-210">Bu bağlantıda KnockOutJS Kitaplığı hakkında daha fazla bilgi edinebilirsiniz: [ [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/) ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="6518e-210">You can get more information about KnockOutJS library in this link: [[http://learn.knockoutjs.com/](http://learn.knockoutjs.com/)](http://learn.knockoutjs.com/).</span></span> <span data-ttu-id="6518e-211">Ayrıca, jQuery ve jQuery kullanıcı Arabirimi hakkında bilgi edinebilirsiniz, [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="6518e-211">Additionally, you can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>

<a id="Task_2_-_Exploring_the_Mobile_Application_Template"></a>
#### <a name="task-2---exploring-the-mobile-application-template"></a><span data-ttu-id="6518e-212">Görev 2 - mobil uygulama şablonu keşfetme</span><span class="sxs-lookup"><span data-stu-id="6518e-212">Task 2 - Exploring the Mobile Application Template</span></span>

<span data-ttu-id="6518e-213">ASP.NET MVC 4 Web siteleri için mobil ve tablet tarayıcılar geliştirilmesini kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="6518e-213">ASP.NET MVC 4 facilitates the development of websites for mobile and tablet browsers.</span></span> <span data-ttu-id="6518e-214">Bu şablon Internet uygulaması şablonu (denetleyici kodlarının neredeyse aynı olduğuna dikkat edin) aynı uygulama yapısını var, ancak kendi stili dokunmatik tabanlı mobil cihazlarda düzgün işlenecek değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="6518e-214">This template has the same application structure as the Internet Application template (notice that the controller code is practically identical), but its style was modified to render properly in touch-based mobile devices.</span></span>

1. <span data-ttu-id="6518e-215">Seçin **dosya | Yeni | Proje** menü komutu.</span><span class="sxs-lookup"><span data-stu-id="6518e-215">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="6518e-216">İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** Şablonu'nu seçin ve ağaç **ASP.NET MVC 4 Web uygulaması.**</span><span class="sxs-lookup"><span data-stu-id="6518e-216">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="6518e-217">Projeyi adlandırın **PhotoGallery.Mobile**, bir konum seçin (veya varsayılan değeri bırakın), seçin &quot;eklemek için çözüm&quot; tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="6518e-217">Name the project **PhotoGallery.Mobile**, select a location (or leave the default), select &quot;Add to solution&quot; and click **OK**.</span></span>
2. <span data-ttu-id="6518e-218">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **mobil uygulama** proje şablonu ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="6518e-218">In the **New ASP.NET MVC 4 Project** dialog, select the **Mobile Application** project template and click **OK**.</span></span> <span data-ttu-id="6518e-219">Razor görünüm altyapısı seçtiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="6518e-219">Make sure you have selected Razor as the view engine.</span></span>

    <span data-ttu-id="6518e-220">![Yeni bir ASP.NET MVC 4 mobil uygulama oluşturma](whats-new-in-aspnet-mvc-4/_static/image11.png "yeni bir ASP.NET MVC 4 mobil uygulama oluşturma")</span><span class="sxs-lookup"><span data-stu-id="6518e-220">![Creating a new ASP.NET MVC 4 Mobile Application](whats-new-in-aspnet-mvc-4/_static/image11.png "Creating a new ASP.NET MVC 4 Mobile Application")</span></span>

    <span data-ttu-id="6518e-221">*Yeni bir ASP.NET MVC 4 mobil uygulama oluşturma*</span><span class="sxs-lookup"><span data-stu-id="6518e-221">*Creating a new ASP.NET MVC 4 Mobile Application*</span></span>
3. <span data-ttu-id="6518e-222">Artık çözümü keşfedin ve bazı mobil cihazlar için ASP.NET MVC 4 çözüm şablonu tarafından sunulan yeni özellikleri kullanıma şunlardır:</span><span class="sxs-lookup"><span data-stu-id="6518e-222">Now you are able to explore the solution and check out some of the new features introduced by the ASP.NET MVC 4 solution template for mobile:</span></span>

    - <span data-ttu-id="6518e-223">**jQuery Mobile kitaplığı**</span><span class="sxs-lookup"><span data-stu-id="6518e-223">**jQuery Mobile Library**</span></span>

        <span data-ttu-id="6518e-224">Mobil uygulaması proje şablonu, mobil tarayıcı uyumluluğu için bir açık kaynak kitaplığı jQuery Mobile kitaplığı içerir.</span><span class="sxs-lookup"><span data-stu-id="6518e-224">The Mobile Application project template includes the jQuery Mobile library, which is an open source library for mobile browser compatibility.</span></span> <span data-ttu-id="6518e-225">jQuery Mobile kademeli geliştirmeyi, CSS ve JavaScript destekleyen mobil tarayıcılar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="6518e-225">jQuery Mobile applies progressive enhancement to mobile browsers that support CSS and JavaScript.</span></span> <span data-ttu-id="6518e-226">Kademeli geliştirmeyi yalnızca Zengin içeriği görüntülemek en güçlü tarayıcılar olanak sağlarken temel bir web sayfası içeriği görüntülemek kullanılan tüm tarayıcılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="6518e-226">Progressive enhancement enables all browsers to display the basic content of a web page, while it only enables the most powerful browsers to display the rich content.</span></span> <span data-ttu-id="6518e-227">JQuery Mobile stili içinde bulunan JavaScript ve CSS dosyalarına içeriği, sayfa biçimlendirme içinde herhangi bir değişiklik yapmadan ekrana sığacak şekilde mobil tarayıcılar yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="6518e-227">The JavaScript and CSS files, included in the jQuery Mobile style, help mobile browsers to fit the content in the screen without making any change in the page markup.</span></span>

        ![jQuery-mobile-library-included-in-the-template](whats-new-in-aspnet-mvc-4/_static/image12.png)

        <span data-ttu-id="6518e-229">*jQuery mobile kitaplığı şablona dahil*</span><span class="sxs-lookup"><span data-stu-id="6518e-229">*jQuery mobile library included in the template*</span></span>
    - <span data-ttu-id="6518e-230">**HTML5 tabanlı biçimlendirme**</span><span class="sxs-lookup"><span data-stu-id="6518e-230">**HTML5 based markup**</span></span>

        ![Mobile-application-template-using-HTML5-markup](whats-new-in-aspnet-mvc-4/_static/image13.png)

        <span data-ttu-id="6518e-232">*HTML5 biçimlendirme, (Login.cshtml ve Index.cshtml) mobil uygulama şablonu*</span><span class="sxs-lookup"><span data-stu-id="6518e-232">*Mobile application template using HTML5 markup, (Login.cshtml and index.cshtml)*</span></span>
4. <span data-ttu-id="6518e-233">Tuşuna **F5** çözümü çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6518e-233">Press **F5** to run the solution.</span></span>
5. <span data-ttu-id="6518e-234">Açık **Windows Phone 7 öykünücüsü**.</span><span class="sxs-lookup"><span data-stu-id="6518e-234">Open the **Windows Phone 7 Emulator**.</span></span>
6. <span data-ttu-id="6518e-235">Telefonunuzun Başlangıç ekranına Internet Explorer'ı açın.</span><span class="sxs-lookup"><span data-stu-id="6518e-235">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="6518e-236">Masaüstü uygulaması başlatıldığı URL denetleyin ve telefonunuzdan bu URL'ye gidin (örn `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="6518e-236">Check out the URL where the desktop application started and browse to that URL from the phone (e.g. `http://localhost:[PortNumber]/`).</span></span>
7. <span data-ttu-id="6518e-237">Oturum açma sayfasına girin ya da kullanıma göre sayfa hakkında.</span><span class="sxs-lookup"><span data-stu-id="6518e-237">Now you are able to enter the login page or check out the about page.</span></span> <span data-ttu-id="6518e-238">Mobil cihazlar için yeni Metro uygulaması Web sitesinin stili dayalı dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-238">Notice that the style of the website is based on the new Metro app for mobile.</span></span> <span data-ttu-id="6518e-239">ASP.NET MVC 4 proje şablonu, tüm öğelerin görünür ve etkin olduğundan emin yapma, mobil cihazlarda düzgün şekilde görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="6518e-239">The ASP.NET MVC 4 project template is correctly displayed on mobile devices, making sure all the elements of the page are visible and enabled.</span></span> <span data-ttu-id="6518e-240">Üst bağlantılar tıklandığında veya dokunulduğunda büyük olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-240">Notice that the links on the header are big enough to be clicked or tapped.</span></span>

    <span data-ttu-id="6518e-241">![Proje şablonunu bir mobil cihaz sayfalarında](whats-new-in-aspnet-mvc-4/_static/image14.png "proje şablonu sayfalarında bir mobil cihaz")</span><span class="sxs-lookup"><span data-stu-id="6518e-241">![Project template pages in a mobile device](whats-new-in-aspnet-mvc-4/_static/image14.png "Project template pages in a mobile device")</span></span>

    <span data-ttu-id="6518e-242">*Proje şablonu sayfalarından bir mobil cihaz*</span><span class="sxs-lookup"><span data-stu-id="6518e-242">*Project template pages in a mobile device*</span></span>
8. <span data-ttu-id="6518e-243">Yeni şablonu da kullanır **Görünüm penceresi meta etiketi**.</span><span class="sxs-lookup"><span data-stu-id="6518e-243">The new template also uses the **Viewport meta tag**.</span></span> <span data-ttu-id="6518e-244">Çoğu mobil Tarayıcı tanımlamak sanal tarayıcı penceresi için bir genişlik veya &quot;Görünüm penceresi&quot;, mobil cihazın gerçek genişliğinden daha büyük olduğu.</span><span class="sxs-lookup"><span data-stu-id="6518e-244">Most mobile browsers define a width for a virtual browser window or &quot;viewport&quot;, which is larger than the actual width of the mobile device.</span></span> <span data-ttu-id="6518e-245">Bu sanal görüntü içinde tüm web sayfasını görüntülemek mobil tarayıcılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="6518e-245">This enables mobile browsers to display the entire web page inside the virtual display.</span></span> <span data-ttu-id="6518e-246">**Görünüm penceresi meta etiketi** genişlik, yükseklik ve tarayıcı alanının ölçeği, mobil cihazlarda ayarlamak, web geliştiricilerinin sağlayan **.**</span><span class="sxs-lookup"><span data-stu-id="6518e-246">The **Viewport meta tag** allows web developers to set the width, height and scale of the browser area on mobile devices **.**</span></span> <span data-ttu-id="6518e-247">Mobil uygulamalar için ASP.NET MVC 4 şablon cihaz genişliğine görünüm penceresinin ayarlar (&quot;genişliği cihaz width =&quot;) düzeni şablondaki (*görünümler/paylaşılan\_Layout.cshtml*), böylece tüm cihaz ekranı genişliğine ayarlayın, Görünüm penceresi sayfaları olacaktır.</span><span class="sxs-lookup"><span data-stu-id="6518e-247">The ASP.NET MVC 4 template for Mobile Applications sets the viewport to the device width (&quot;width=device-width&quot;) in the layout template (*Views\Shared\_Layout.cshtml*), so that all the pages will have their viewport set to the device screen width.</span></span> <span data-ttu-id="6518e-248">Görünüm penceresi meta etiketi varsayılan tarayıcı görünümü değiştirmez dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-248">Notice that the Viewport meta tag will not change the default browser view.</span></span>
9. <span data-ttu-id="6518e-249">Açık  **\_Layout.cshtml**, bulunan **görünümleri | Paylaşılan** klasörü ve açıklama Görünüm penceresi meta etiketi.</span><span class="sxs-lookup"><span data-stu-id="6518e-249">Open **\_Layout.cshtml**, located in the **Views | Shared** folder, and comment the Viewport meta tag.</span></span> <span data-ttu-id="6518e-250">Uygulamayı çalıştırmak yoksa zaten açılmış ve farklılıkları denetleyin.</span><span class="sxs-lookup"><span data-stu-id="6518e-250">Run the application, if not already opened, and check out the differences.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample2.cshtml)]

<span data-ttu-id="6518e-251">![Görünüm penceresi meta etiketi için yorum oluşturma sonrasında site](whats-new-in-aspnet-mvc-4/_static/image15.png "Görünüm penceresi meta etiketi için yorum oluşturma sonrasında site")</span><span class="sxs-lookup"><span data-stu-id="6518e-251">![The site after commenting the viewport meta tag](whats-new-in-aspnet-mvc-4/_static/image15.png "The site after commenting the viewport meta tag")</span></span>

<span data-ttu-id="6518e-252">*Görünüm penceresi meta etiketi için yorum oluşturma sonrasında site*</span><span class="sxs-lookup"><span data-stu-id="6518e-252">*The site after commenting the viewport meta tag*</span></span>
10. <span data-ttu-id="6518e-253">Visual Studio'da **SHIFT** + **F5** uygulama hata ayıklamayı durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="6518e-253">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
11. <span data-ttu-id="6518e-254">Görünüm penceresi meta etiketi açıklamasını kaldırın.</span><span class="sxs-lookup"><span data-stu-id="6518e-254">Uncomment the viewport meta tag.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample3.cshtml)]

<a id="Task_3_-_Using_Adaptive_Rendering"></a>
#### <a name="task-3---using-adaptive-rendering"></a><span data-ttu-id="6518e-255">Görev 3 - Uyarlamalı işleme kullanma</span><span class="sxs-lookup"><span data-stu-id="6518e-255">Task 3 - Using Adaptive Rendering</span></span>

<span data-ttu-id="6518e-256">Bu görevde, herhangi bir özelleştirme olmadan aynı anda bir Web sayfası mobil cihazlar ve tarayıcılar üzerinde doğru şekilde işlemek için başka bir yöntem öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-256">In this task, you will learn another method to render a Web page correctly on mobile devices and Web browsers at the same time without any customization.</span></span> <span data-ttu-id="6518e-257">Görünüm penceresi meta etiketi ile benzer bir amaç zaten kullandınız.</span><span class="sxs-lookup"><span data-stu-id="6518e-257">You have already used Viewport meta tag with a similar purpose.</span></span> <span data-ttu-id="6518e-258">Artık başka bir güçlü yöntemi karşılar: *Uyarlamalı işleme*.</span><span class="sxs-lookup"><span data-stu-id="6518e-258">Now you will meet another powerful method: *adaptive rendering*.</span></span>

<span data-ttu-id="6518e-259">Uyarlamalı işleme kullanan bir teknik olduğu **CSS3 media sorguları** bir sayfaya uygulanan stilini özelleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="6518e-259">Adaptive rendering is a technique that uses **CSS3 media queries** to customize the style applied to a page.</span></span> <span data-ttu-id="6518e-260">Medya sorgular, belirli bir koşul altında CSS stilleri gruplandırma bir stil sayfası içinde koşulları tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="6518e-260">Media queries define conditions inside a style sheet, grouping CSS styles under a certain condition.</span></span> <span data-ttu-id="6518e-261">Yalnızca koşul true olduğunda, stil bildirilen nesnelere uygulanır.</span><span class="sxs-lookup"><span data-stu-id="6518e-261">Only when the condition is true, the style is applied to the declared objects.</span></span>

<span data-ttu-id="6518e-262">Site farklı cihazlarda görüntülemek için herhangi bir özelleştirme Uyarlamalı işleme teknikleri tarafından sağlanan esneklik sağlar.</span><span class="sxs-lookup"><span data-stu-id="6518e-262">The flexibility provided by the adaptive rendering technique enables any customization for displaying the site on different devices.</span></span> <span data-ttu-id="6518e-263">Tek bir stil sayfasında mantığı kod yazmaya gerek kalmadan stili seçmek için istediğiniz sayıda stilleri tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-263">You can define as many styles as you want on a single style sheet without writing logic code to choose the style.</span></span> <span data-ttu-id="6518e-264">Yinelenen kod ve amacıyla işlemek için mantığı miktarını azaltır olarak bu nedenle, sayfa stilleri uyarlama bir çok masaüstünüzdeki yoludur.</span><span class="sxs-lookup"><span data-stu-id="6518e-264">Therefore, it is a very neat way of adapting page styles, as it reduces the amount of duplicated code and logic for rendering purposes.</span></span> <span data-ttu-id="6518e-265">CSS dosyaları boyutunu fazladır büyüyebilir gibi diğer taraftan, bant genişliği tüketimi, artırır.</span><span class="sxs-lookup"><span data-stu-id="6518e-265">On the other hand, bandwidth consumption would increase, as the size of your CSS files could grow marginally.</span></span>

<span data-ttu-id="6518e-266">Uyarlamalı oluşturma tekniği kullanarak, siteniz olacaktır **düzgün bir şekilde bağımsız olarak tarayıcı görüntülenir.**</span><span class="sxs-lookup"><span data-stu-id="6518e-266">By using the adaptive rendering technique, your site will be **displayed properly, regardless of the browser.**</span></span> <span data-ttu-id="6518e-267">Ancak, ek bant genişliği yüklerseniz önemli olduğu düşünmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-267">However, you should consider if the bandwidth extra load is a concern.</span></span>

> [!NOTE]
> <span data-ttu-id="6518e-268">Medya sorgusu temel biçimi: @media \[kapsamı: tüm | el | yazdırma | projeksiyon | ekran\] ([özellik: değer] ve... [özellik: değer])</span><span class="sxs-lookup"><span data-stu-id="6518e-268">The basic format of a media query is: @media \[Scope: all | handheld | print | projection | screen\] ([property:value] and ... [property:value])</span></span>


<span data-ttu-id="6518e-269">Medya sorgularının örnekleri: &gt;  <strong>@media tüm ve (max-width: 1000px) ve (en az Genişlik: 700px) {}:</strong> 700px 1000px arasındaki tüm çözümler için.</span><span class="sxs-lookup"><span data-stu-id="6518e-269">Examples of media queries: &gt;<strong>@media all and (max-width: 1000px) and (min-width: 700px) {}:</strong> For all the resolutions between 700px and 1000px.</span></span>

> <span data-ttu-id="6518e-270"><strong>@media ekran ve (min-width: 400px) ve (max-width: 700px) {…}:</strong> yalnızca ekranlar için.</span><span class="sxs-lookup"><span data-stu-id="6518e-270"><strong>@media screen and (min-width: 400px) and (max-width: 700px) { ... }:</strong> Only for screens.</span></span> <span data-ttu-id="6518e-271">Çözüm 700px ile 400 arasında olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6518e-271">The resolution must be between 400 and 700px.</span></span>
> 
> <span data-ttu-id="6518e-272"><strong>@media taşınabilir ve (en az Genişlik: 20em), ekranı ve (en az Genişlik: 20em) {…}:</strong> el bilgisayarlarında çalışmak (Mobil ve cihazlar) ve ekranlar için.</span><span class="sxs-lookup"><span data-stu-id="6518e-272"><strong>@media handheld and (min-width: 20em), screen and (min-width: 20em) { ... }:</strong> For handhelds(mobile and devices) and screens.</span></span> <span data-ttu-id="6518e-273">Minimum genişliğini 20em büyük olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="6518e-273">The minimum width must be greater than 20em.</span></span>
> 
> <span data-ttu-id="6518e-274">Bu konu hakkında daha fazla bilgi bulabilirsiniz [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span><span class="sxs-lookup"><span data-stu-id="6518e-274">You can find more information about this on the [W3C site](http://www.w3.org/TR/css3-mediaqueries/).</span></span>


<span data-ttu-id="6518e-275">Okunurluğunu ASP.NET MVC 4 Web sitesi şablonu varsayılan, Uyarlamalı işleme nasıl çalıştığını şimdi keşfedin.</span><span class="sxs-lookup"><span data-stu-id="6518e-275">You will now explore how the adaptive rendering works, improving the readability of the ASP.NET MVC 4 default website template.</span></span>

1. <span data-ttu-id="6518e-276">Açık **PhotoGallery.sln** görev 1'den oluşturdunuz ve seçin çözüm **Fotografgalerisi** proje.</span><span class="sxs-lookup"><span data-stu-id="6518e-276">Open the **PhotoGallery.sln** solution you have created at Task 1 and select the **PhotoGallery** project.</span></span> <span data-ttu-id="6518e-277">Tuşuna **F5** çözümü çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6518e-277">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="6518e-278">Tarayıcının genişliği windows yarısı veya küçüktür özgün boyutuna çeyreği ayarlama, yeniden boyutlandırın.</span><span class="sxs-lookup"><span data-stu-id="6518e-278">Resize the browser's width, setting the windows to half or to less than a quarter of its original size.</span></span> <span data-ttu-id="6518e-279">Üstbilgi öğeleri ile neler olduğuna dikkat edin: bazı öğeleri üstbilgi görünür alanında görünmez.</span><span class="sxs-lookup"><span data-stu-id="6518e-279">Notice what happens with the items in the header: Some elements will not appear in the visible area of the header.</span></span>
3. <span data-ttu-id="6518e-280">Açık <strong>Site.css</strong> bulunan Visual Studio Çözüm Gezgini'nde, bir dosyadan <strong>içerik</strong> proje klasörü.</span><span class="sxs-lookup"><span data-stu-id="6518e-280">Open <strong>Site.css</strong> file from the Visual Studio Solution explorer, located in <strong>Content</strong> project folder.</span></span> <span data-ttu-id="6518e-281">Tuşuna <strong>CTRL + F</strong> Visual Studio tümleşik arama açın ve yazmak için <strong>@media</strong> bulunacak <strong>CSS medya sorgusu</strong>.</span><span class="sxs-lookup"><span data-stu-id="6518e-281">Press <strong>CTRL + F</strong> to open Visual Studio integrated search, and write <strong>@media</strong> to locate the <strong>CSS media query</strong>.</span></span>

    <span data-ttu-id="6518e-282">Bu şablonda tanımlanan medya sorgu koşulu bu şekilde çalışır: tarayıcının pencere boyutunu olduğunda aşağıda **850 px**, uygulanan CSS kurallarını bu medya blok içinde tanımlanan olanlardır.</span><span class="sxs-lookup"><span data-stu-id="6518e-282">The media query condition defined in this template works in this way: When the browser's window size is below **850 px**, the CSS rules applied are the ones defined inside this media block.</span></span>

    <span data-ttu-id="6518e-283">![Medya sorgusu bulma](whats-new-in-aspnet-mvc-4/_static/image16.png "medya sorgusu bulma")</span><span class="sxs-lookup"><span data-stu-id="6518e-283">![Locating the media query](whats-new-in-aspnet-mvc-4/_static/image16.png "Locating the media query")</span></span>

    <span data-ttu-id="6518e-284">*Medya sorgusu bulma*</span><span class="sxs-lookup"><span data-stu-id="6518e-284">*Locating the media query*</span></span>
4. <span data-ttu-id="6518e-285">Max-width öznitelik değeri içinde 850 ayarlamak yerine piksel ile **10px**, Uyarlamalı işleme devre dışı bırakmak için basın **CTRL + S** değişiklikleri kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="6518e-285">Replace the max-width attribute value set in 850 px with **10px**, in order to disable the adaptive rendering, and press **CTRL + S** to save the changes.</span></span> <span data-ttu-id="6518e-286">Dönüş tuşuna basın ve tarayıcı **CTRL + F5 tuşlarına basarak** yaptığınız değişikliklerle sayfayı yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="6518e-286">Return to the browser and press **CTRL + F5** to refresh the page with the changes you have made.</span></span> <span data-ttu-id="6518e-287">Her iki sayfa farklılıkları pencerenin genişliğini ayarlarken dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-287">Notice the differences in both pages when adjusting the width of the window.</span></span>

    <span data-ttu-id="6518e-288">![Sayfanın sol tarafta uyguluyor @media stilde stil sağındaki atlanırsa](whats-new-in-aspnet-mvc-4/_static/image17.png "sol, sayfanın uyguluyor @media stilde stil sağındaki atlanırsa")</span><span class="sxs-lookup"><span data-stu-id="6518e-288">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image17.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="6518e-289"><em>Sayfanın sol tarafta uyguluyor @media stilde stil sağındaki atlanırsa</em></span><span class="sxs-lookup"><span data-stu-id="6518e-289"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="6518e-290">Şimdi, şimdi mobil cihazlarda neler olduğunu kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="6518e-290">Now, let's check out what happens on mobile devices:</span></span>

    <span data-ttu-id="6518e-291">![Sayfanın sol tarafta uyguluyor @media stilde stil sağındaki atlanırsa](whats-new-in-aspnet-mvc-4/_static/image18.png "sol, sayfanın uyguluyor @media stilde stil sağındaki atlanırsa")</span><span class="sxs-lookup"><span data-stu-id="6518e-291">![In the left, the page is applying the @media style, in the right, the style is omitted](whats-new-in-aspnet-mvc-4/_static/image18.png "In the left, the page is applying the @media style, in the right, the style is omitted")</span></span>

    <span data-ttu-id="6518e-292"><em>Sayfanın sol tarafta uyguluyor @media stilde stil sağındaki atlanırsa</em></span><span class="sxs-lookup"><span data-stu-id="6518e-292"><em>In the left, the page is applying the @media style, in the right, the style is omitted</em></span></span>

    <span data-ttu-id="6518e-293">Bir Web tarayıcısında sayfa işlendiğinde değişiklikleri bir mobil cihazı kullanırken çok önemli olmadığını fark edeceksiniz rağmen farkları daha belirgin hale gelir.</span><span class="sxs-lookup"><span data-stu-id="6518e-293">Although you will notice that the changes when the page is rendered in a Web browser are not very significant, when using a mobile device the differences become more obvious.</span></span> <span data-ttu-id="6518e-294">Özel bir stil okunabilirliği geliştirildi görüntü sol tarafında görebiliriz.</span><span class="sxs-lookup"><span data-stu-id="6518e-294">On the left side of the image, we can see that the custom style improved the readability.</span></span>

    <span data-ttu-id="6518e-295">Uyarlamalı işleme koşullu bir Web sitesi için stil oluşturma ve net bir yaklaşım ile ortak stil sorunlarını çözme uygulamak kolaylaştıran birçok daha fazla senaryoda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="6518e-295">Adaptive rendering can be used in many more scenarios, making it easier to apply conditional styling to a Web site and solving common style issues with a neat approach.</span></span>

    <span data-ttu-id="6518e-296">Bu özelliklerden herhangi bir web uygulamasına olabilmesi için CSS medya sorgu ve Görünüm penceresi meta etiketi ASP.NET MVC 4'e özgü değildir.</span><span class="sxs-lookup"><span data-stu-id="6518e-296">The Viewport meta tag and CSS media queries are not specific to ASP.NET MVC 4, so you can take advantage of these features in any web application.</span></span>
5. <span data-ttu-id="6518e-297">Visual Studio'da **SHIFT** + **F5** uygulama hata ayıklamayı durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="6518e-297">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_the_Photo_Gallery_Web_Application"></a>
### <a name="exercise-2-creating-the-photo-gallery-web-application"></a><span data-ttu-id="6518e-298">Alıştırma 2: Fotoğraf Galerisi Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="6518e-298">Exercise 2: Creating the Photo Gallery Web Application</span></span>

<span data-ttu-id="6518e-299">Bu alıştırmada, fotoğrafları görüntülemek için bir Fotoğraf Galerisi uygulamasında çalışmaz.</span><span class="sxs-lookup"><span data-stu-id="6518e-299">In this exercise, you will work on a Photo Gallery application to display photos.</span></span> <span data-ttu-id="6518e-300">ASP.NET MVC 4 proje şablonu ile başlar ve ardından bir hizmetten fotoğraf almak ve bunları giriş sayfasında görüntülemek için bir özellik ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-300">You will start with the ASP.NET MVC 4 project template, and then you will add a feature to retrieve photos from a service and display them in the home page.</span></span>

<span data-ttu-id="6518e-301">Aşağıdaki alıştırmada, mobil cihazlarda görüntülenme biçimini geliştirmek için bu çözüm güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="6518e-301">In the following exercise, you will update this solution to enhance the way it is displayed on mobile devices.</span></span>

<a id="Task_1_-_Creating_a_Mock_Photo_Service"></a>
#### <a name="task-1---creating-a-mock-photo-service"></a><span data-ttu-id="6518e-302">Görev 1 - sahte fotoğraf hizmet oluşturma</span><span class="sxs-lookup"><span data-stu-id="6518e-302">Task 1 - Creating a Mock Photo Service</span></span>

<span data-ttu-id="6518e-303">Bu görevde, sahte galeride görüntülenecek içeriği almak için fotoğraf hizmeti oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="6518e-303">In this task, you will create a mock of the photo service to retrieve the content that will be displayed in the gallery.</span></span> <span data-ttu-id="6518e-304">Bunu yapmak için her fotoğraf verileri içeren bir JSON dosyası yalnızca döndüreceği yeni bir denetleyici ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-304">To do this, you will add a new controller that will simply return a JSON file with the data of each photo.</span></span>

1. <span data-ttu-id="6518e-305">Açık **Visual Studio** zaten açtıysanız.</span><span class="sxs-lookup"><span data-stu-id="6518e-305">Open **Visual Studio** if not already opened.</span></span>
2. <span data-ttu-id="6518e-306">Seçin **dosya | Yeni | Proje** menü komutu.</span><span class="sxs-lookup"><span data-stu-id="6518e-306">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="6518e-307">İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** Şablonu'nu seçin ve ağaç **ASP.NET MVC 4 Web uygulaması.**</span><span class="sxs-lookup"><span data-stu-id="6518e-307">In the **New Project** dialog, select the **Visual C# | Web** template on the left pane tree, and choose **ASP.NET MVC 4 Web Application.**</span></span> <span data-ttu-id="6518e-308">Projeyi adlandırın **Fotografgalerisi**, bir konum seçin (veya varsayılan değeri bırakın) ve tıklayın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="6518e-308">Name the project **PhotoGallery**, select a location (or leave the default) and click **OK**.</span></span> <span data-ttu-id="6518e-309">Alternatif olarak, mevcut ASP.NET MVC 4 ile çalışmaya devam edebilirsiniz **Internet uygulaması** çözümünden **alıştırma 1** ve bir sonraki adımı atlayın.</span><span class="sxs-lookup"><span data-stu-id="6518e-309">Alternatively, you can continue working from your existing ASP.NET MVC 4 **Internet Application** solution from **Exercise 1** and skip the next step.</span></span>
3. <span data-ttu-id="6518e-310">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulaması** proje şablonu ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="6518e-310">In the **New ASP.NET MVC 4 Project** dialog box, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="6518e-311">Razor görünüm altyapısı seçili olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="6518e-311">Make sure you have Razor selected as the View Engine.</span></span>
4. <span data-ttu-id="6518e-312">İçinde **Çözüm Gezgini**, sağ **uygulama\_veri** klasörü seçin ve proje **Ekle | Var olan öğe**.</span><span class="sxs-lookup"><span data-stu-id="6518e-312">In the **Solution Explorer**, right-click the **App\_Data** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="6518e-313">Göz atın **Source\Assets\App\_veri** bu Laboratuvar, klasör ve eklemek **Photos.json** dosya.</span><span class="sxs-lookup"><span data-stu-id="6518e-313">Browse to the **Source\Assets\App\_Data** folder of this lab and add the **Photos.json** file.</span></span>
5. <span data-ttu-id="6518e-314">Adlı bir yeni denetleyici oluşturun **PhotoController**.</span><span class="sxs-lookup"><span data-stu-id="6518e-314">Create a new controller with the name **PhotoController**.</span></span> <span data-ttu-id="6518e-315">Bunu yapmak için sağ **denetleyicileri** klasörüne gidin **Ekle** seçip **denetleyicisi.**</span><span class="sxs-lookup"><span data-stu-id="6518e-315">To do this, right-click on the **Controllers** folder, go to **Add** and select **Controller.**</span></span> <span data-ttu-id="6518e-316">Denetleyici adı tamamlamak, bırakın **boş MVC denetleyicisi** şablonu ve tıklatın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="6518e-316">Complete the controller name, leave the **Empty MVC controller** template and click **Add**.</span></span>

    <span data-ttu-id="6518e-317">![PhotoController ekleme](whats-new-in-aspnet-mvc-4/_static/image19.png "PhotoController ekleme")</span><span class="sxs-lookup"><span data-stu-id="6518e-317">![Adding the PhotoController](whats-new-in-aspnet-mvc-4/_static/image19.png "Adding the PhotoController")</span></span>

    <span data-ttu-id="6518e-318">*PhotoController ekleme*</span><span class="sxs-lookup"><span data-stu-id="6518e-318">*Adding the PhotoController*</span></span>
6. <span data-ttu-id="6518e-319">Değiştirin **dizin** yöntemi aşağıdaki **galeri** eylem ve son eklediğiniz projeye JSON dosyasından içerik döndürülecek.</span><span class="sxs-lookup"><span data-stu-id="6518e-319">Replace the **Index** method with the following **Gallery** action, and return the content from the JSON file you have recently added to the project.</span></span>

    <span data-ttu-id="6518e-320">(Kod parçacığını - *ASP.NET MVC 4 - Ex02 - Laboratuvar galeri eylem*)</span><span class="sxs-lookup"><span data-stu-id="6518e-320">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Gallery Action*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample4.cs)]
7. <span data-ttu-id="6518e-321">Tuşuna **F5** çözümü çalıştırın ve ardından sahte fotoğraf hizmeti test etmek için aşağıdaki URL'ye gidin: `http://localhost:[port]/photo/gallery` (uygulamanın nerede başlatıldı geçerli bir bağlantı noktasında [bağlantı noktası] değeri bağlıdır).</span><span class="sxs-lookup"><span data-stu-id="6518e-321">Press **F5** to run the solution, and then browse to the following URL in order to test the mocked photo service: `http://localhost:[port]/photo/gallery` (the [port] value depends on the current port where the application was launched).</span></span> <span data-ttu-id="6518e-322">Bu istek içeriğini almak **Photos.json** dosya.</span><span class="sxs-lookup"><span data-stu-id="6518e-322">The request to this URL should retrieve the content of the **Photos.json** file.</span></span>

    <span data-ttu-id="6518e-323">![Sahte fotoğraf hizmeti test](whats-new-in-aspnet-mvc-4/_static/image20.png "sahte fotoğraf hizmeti test etme")</span><span class="sxs-lookup"><span data-stu-id="6518e-323">![Testing the mocked photo service](whats-new-in-aspnet-mvc-4/_static/image20.png "Testing the mocked photo service")</span></span>

    <span data-ttu-id="6518e-324">*Sahte fotoğraf hizmeti test etme*</span><span class="sxs-lookup"><span data-stu-id="6518e-324">*Testing the mocked photo service*</span></span>

<span data-ttu-id="6518e-325">Gerçek bir uygulamada kullanabileceğinizi [ASP.NET Web API](../../../../web-api/index.md) Fotoğraf Galerisi hizmeti uygulamak için.</span><span class="sxs-lookup"><span data-stu-id="6518e-325">In a real implementation you could use [ASP.NET Web API](../../../../web-api/index.md) to implement the Photo Gallery service.</span></span> <span data-ttu-id="6518e-326">ASP.NET Web API istemciler, tarayıcılar ve mobil cihazlar dahil olmak üzere geniş bir yelpazede ulaşan HTTP hizmetlerini oluşturmayı kolaylaştıran bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="6518e-326">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="6518e-327">ASP.NET Web API'si, .NET Framework üzerinde RESTful uygulamaları geliştirmek için ideal bir platformdur.</span><span class="sxs-lookup"><span data-stu-id="6518e-327">ASP.NET Web API is an ideal platform for building RESTful applications on the .NET Framework.</span></span>

<a id="Task_2_-_Displaying_the_Photo_Gallery"></a>
#### <a name="task-2---displaying-the-photo-gallery"></a><span data-ttu-id="6518e-328">Görev 2 - Fotoğraf Galerisi görüntüleme</span><span class="sxs-lookup"><span data-stu-id="6518e-328">Task 2 - Displaying the Photo Gallery</span></span>

<span data-ttu-id="6518e-329">Bu görevde, giriş sayfası, bu alıştırmada ilk görevde oluşturduğunuz sahte hizmetini kullanarak Fotoğraf Galerisi göstermek için güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="6518e-329">In this task, you will update the Home page to show the photo gallery by using the mocked service you created in the first task of this exercise.</span></span> <span data-ttu-id="6518e-330">Model dosyaları ekleme ve galeri görünümleri güncelleştirme.</span><span class="sxs-lookup"><span data-stu-id="6518e-330">You will add model files and update the gallery views.</span></span>

1. <span data-ttu-id="6518e-331">Visual Studio'da **SHIFT** + **F5** uygulama hata ayıklamayı durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="6518e-331">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="6518e-332">Oluşturma **fotoğraf** sınıfını **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="6518e-332">Create the **Photo** class in the **Models** folder.</span></span> <span data-ttu-id="6518e-333">Bunu yapmak için sağ **modelleri** klasörüne **Ekle** tıklatıp **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="6518e-333">To do this, right-click on the **Models** folder, select **Add** and click **Class**.</span></span> <span data-ttu-id="6518e-334">Ardından, kümesinin adı **Photo.cs** tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="6518e-334">Then, set the name to **Photo.cs** and click **Add**.</span></span>
3. <span data-ttu-id="6518e-335">Aşağıdaki üye ekleme **fotoğraf** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6518e-335">Add the following members to the **Photo** class.</span></span>

    <span data-ttu-id="6518e-336">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - fotoğraf modeli*)</span><span class="sxs-lookup"><span data-stu-id="6518e-336">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo model*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample5.cs)]
4. <span data-ttu-id="6518e-337">Açık **HomeController.cs** dosya **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="6518e-337">Open the **HomeController.cs** file from the **Controllers** folder.</span></span>
5. <span data-ttu-id="6518e-338">Aşağıdaki using deyimlerini.</span><span class="sxs-lookup"><span data-stu-id="6518e-338">Add the following using statements.</span></span>

    <span data-ttu-id="6518e-339">(Kod parçacığını - *ASP.NET MVC 4 - Ex02 - Laboratuvar HomeController kullanımları*)</span><span class="sxs-lookup"><span data-stu-id="6518e-339">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - HomeController Usings*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample6.cs)]
6. <span data-ttu-id="6518e-340">Güncelleştirme **dizin** eylemin kullanmasını **HttpClient** galeri verileri almak ve ardından **JavaScriptSerializer** görünüm modeli için seri durumdan çıkarılamıyor.</span><span class="sxs-lookup"><span data-stu-id="6518e-340">Update the **Index** action to use **HttpClient** to retrieve the gallery data, and then use the **JavaScriptSerializer** to deserialize it to the view model.</span></span>

    <span data-ttu-id="6518e-341">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex02 - dizin eylem*)</span><span class="sxs-lookup"><span data-stu-id="6518e-341">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Index Action*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample7.cs)]
7. <span data-ttu-id="6518e-342">Açık **Index.cshtml** altında bulunan dosya **görünümler/giriş** klasörünü ve tüm içeriğini aşağıdaki kodla değiştirin.</span><span class="sxs-lookup"><span data-stu-id="6518e-342">Open the **Index.cshtml** file located under the **Views\Home** folder and replace all the content with the following code.</span></span>

    <span data-ttu-id="6518e-343">Bu kodu hizmetten alınan tüm fotoğraflar aracılığıyla döngüye girer ve bunları sırasız bir listesini görüntüler.</span><span class="sxs-lookup"><span data-stu-id="6518e-343">This code loops through all the photos retrieved from the service and displays them into an unordered list.</span></span>

    <span data-ttu-id="6518e-344">(Kod parçacığını - *ASP.NET MVC 4 - Ex02 - Laboratuvar fotoğraf listesi*)</span><span class="sxs-lookup"><span data-stu-id="6518e-344">(Code Snippet - *ASP.NET MVC 4 Lab - Ex02 - Photo List*)</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample8.cshtml)]
8. <span data-ttu-id="6518e-345">İçinde **Çözüm Gezgini**, sağ **içerik** klasörü seçin ve proje **Ekle | Var olan öğe**.</span><span class="sxs-lookup"><span data-stu-id="6518e-345">In the **Solution Explorer**, right-click the **Content** folder of your project, and select **Add | Existing Item**.</span></span> <span data-ttu-id="6518e-346">Göz atın **Source\Assets\Content** bu Laboratuvar, klasör ve eklemek **Site.css** dosya.</span><span class="sxs-lookup"><span data-stu-id="6518e-346">Browse to the **Source\Assets\Content** folder of this lab and add the **Site.css** file.</span></span> <span data-ttu-id="6518e-347">Değişimi onaylamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6518e-347">You will have to confirm its replacement.</span></span> <span data-ttu-id="6518e-348">Varsa **Site.css** dosya açık, ayrıca dosyayı yeniden doğrulamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="6518e-348">If you have the **Site.css** file open, you will have to confirm to reload the file also.</span></span>
9. <span data-ttu-id="6518e-349">Dosya Gezgini'ni açın ve tamamını **fotoğraf** klasörünün altında **Source\Assets** klasör, bu Laboratuvarın Çözüm Gezgini'nde projenizin kök klasörüne.</span><span class="sxs-lookup"><span data-stu-id="6518e-349">Open File Explorer and copy the entire **Photos** folder located under the **Source\Assets** folder of this lab to the root folder of your project in Solution Explorer.</span></span>
10. <span data-ttu-id="6518e-350">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6518e-350">Run the application.</span></span> <span data-ttu-id="6518e-351">Şimdi, galeride fotoğrafları görüntülemek giriş sayfası görmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="6518e-351">You should now see the home page displaying the photos in the gallery.</span></span>

    <span data-ttu-id="6518e-352">![Fotoğraf Galerisi](whats-new-in-aspnet-mvc-4/_static/image21.png "Fotoğraf Galerisi")</span><span class="sxs-lookup"><span data-stu-id="6518e-352">![Photo Gallery](whats-new-in-aspnet-mvc-4/_static/image21.png "Photo Gallery")</span></span>

    <span data-ttu-id="6518e-353">*Fotoğraf Galerisi*</span><span class="sxs-lookup"><span data-stu-id="6518e-353">*Photo Gallery*</span></span>
11. <span data-ttu-id="6518e-354">Visual Studio'da **SHIFT** + **F5** uygulama hata ayıklamayı durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="6518e-354">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Adding_support_for_mobile_devices"></a>
### <a name="exercise-3-adding-support-for-mobile-devices"></a><span data-ttu-id="6518e-355">Alıştırma 3: mobil cihazlar için destek ekleme</span><span class="sxs-lookup"><span data-stu-id="6518e-355">Exercise 3: Adding support for mobile devices</span></span>

<span data-ttu-id="6518e-356">ASP.NET MVC 4'te anahtar güncelleştirmelerden biri mobil geliştirme desteğidir.</span><span class="sxs-lookup"><span data-stu-id="6518e-356">One of the key updates in ASP.NET MVC 4 is the support for mobile development.</span></span> <span data-ttu-id="6518e-357">Bu alıştırmada önceki alıştırmada oluşturduğunuz Fotografgalerisi çözüm genişleterek, ASP.NET MVC 4 mobil uygulamalar için yeni özellikleri inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-357">In this exercise, you will explore ASP.NET MVC 4 new features for mobile applications by extending the PhotoGallery solution you have created in the previous exercise.</span></span>

<a id="Task_1_-_Installing_jQuery_Mobile_in_an_ASPNET_MVC_4_Application"></a>
#### <a name="task-1---installing-jquery-mobile-in-an-aspnet-mvc-4-application"></a><span data-ttu-id="6518e-358">Görev 1 - bir ASP.NET MVC 4 uygulamasında yükleme jQuery Mobile</span><span class="sxs-lookup"><span data-stu-id="6518e-358">Task 1 - Installing jQuery Mobile in an ASP.NET MVC 4 Application</span></span>

1. <span data-ttu-id="6518e-359">Açık **başlamak** çözüm bulunan **kaynak/Ex3-MobileSupport/başlangıç/** klasör.</span><span class="sxs-lookup"><span data-stu-id="6518e-359">Open the **Begin** solution located at **Source/Ex3-MobileSupport/Begin/** folder.</span></span> <span data-ttu-id="6518e-360">Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="6518e-360">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="6518e-361">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="6518e-361">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6518e-362">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="6518e-362">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="6518e-363">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="6518e-363">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="6518e-364">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="6518e-364">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="6518e-365">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="6518e-365">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6518e-366">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="6518e-366">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6518e-367">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="6518e-367">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="6518e-368">Açık **Paket Yöneticisi Konsolu** tıklayarak **Araçları** &gt; **kitaplık Paket Yöneticisi** &gt; **Paket Yöneticisi Konsol** menü seçeneği.</span><span class="sxs-lookup"><span data-stu-id="6518e-368">Open the **Package Manager Console** by clicking the **Tools** &gt; **Library Package Manager** &gt; **Package Manager Console** menu option.</span></span>

    <span data-ttu-id="6518e-369">![NuGet Paket Yöneticisi konsolu açma](whats-new-in-aspnet-mvc-4/_static/image22.png "NuGet Paket Yöneticisi konsolu açma")</span><span class="sxs-lookup"><span data-stu-id="6518e-369">![Opening the NuGet Package Manager Console](whats-new-in-aspnet-mvc-4/_static/image22.png "Opening the NuGet Package Manager Console")</span></span>

    <span data-ttu-id="6518e-370">*NuGet Paket Yöneticisi konsolu açma*</span><span class="sxs-lookup"><span data-stu-id="6518e-370">*Opening the NuGet Package Manager Console*</span></span>
3. <span data-ttu-id="6518e-371">Paket Yöneticisi Konsolu'nda yüklemek için aşağıdaki komutu çalıştırın **jQuery.Mobile.MVC** paket.</span><span class="sxs-lookup"><span data-stu-id="6518e-371">In the Package Manager Console run the following command to install the **jQuery.Mobile.MVC** package.</span></span>

    <span data-ttu-id="6518e-372">jQuery Mobile, dokunma özelliği iyileştirilmiş web kullanıcı arabirimini oluşturmaya yönelik bir açık kaynak kitaplığıdır.</span><span class="sxs-lookup"><span data-stu-id="6518e-372">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="6518e-373">JQuery Mobile bir ASP.NET MVC 4 uygulama ile kullanma Yardımcıları jQuery.Mobile.MVC NuGet paketini içerir.</span><span class="sxs-lookup"><span data-stu-id="6518e-373">The jQuery.Mobile.MVC NuGet package includes helpers to use jQuery Mobile with an ASP.NET MVC 4 application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6518e-374">Aşağıdaki komutu çalıştırarak jQuery.Mobile.MVC kitaplığı Nuget'ten indirdiğiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-374">By running the following command, you will be downloading the jQuery.Mobile.MVC library from Nuget.</span></span>

    <span data-ttu-id="6518e-375">PM</span><span class="sxs-lookup"><span data-stu-id="6518e-375">PM</span></span>

    [!code-powershell[Main](whats-new-in-aspnet-mvc-4/samples/sample9.ps1)]

    <span data-ttu-id="6518e-376">Bu komut, jQuery Mobile ve aşağıdakiler dahil olmak üzere bazı yardımcı dosyalar yüklenir:</span><span class="sxs-lookup"><span data-stu-id="6518e-376">This command installs jQuery Mobile and some helper files, including the following:</span></span>

    - <span data-ttu-id="6518e-377">**Görünümler/paylaşılan/\_Layout.Mobile.cshtml**: bir küçük ekranlar için optimize edilmiş bir jQuery Mobile tabanlı düzeni.</span><span class="sxs-lookup"><span data-stu-id="6518e-377">**Views/Shared/\_Layout.Mobile.cshtml**: is a jQuery Mobile-based layout optimized for a smaller screen.</span></span> <span data-ttu-id="6518e-378">Web sitesi, bir mobil tarayıcıda bir istek aldığında, orijinal düzenini değiştirir (\_Layout.cshtml) Bu bir.</span><span class="sxs-lookup"><span data-stu-id="6518e-378">When the website receives a request from a mobile browser, it will replace the original layout (\_Layout.cshtml) with this one.</span></span>
    - <span data-ttu-id="6518e-379">Görünüm değiştirici bileşeni: oluşan **görünümler/paylaşılan/\_ViewSwitcher.cshtml** kısmi Görünüm ve **ViewSwitcherController.cs** denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="6518e-379">A view-switcher component: consists of the **Views/Shared/\_ViewSwitcher.cshtml** partial view and the **ViewSwitcherController.cs** controller.</span></span> <span data-ttu-id="6518e-380">Bu bileşen, kullanıcıların masaüstü sürümüne geçiş sağlamak için mobil tarayıcılarda bir bağlantı gösterilir.</span><span class="sxs-lookup"><span data-stu-id="6518e-380">This component will show a link on mobile browsers to enable users to switch to the desktop version of the page.</span></span>  
        <span data-ttu-id="6518e-381">![Fotoğraf Galerisi proje mobil desteğiyle](whats-new-in-aspnet-mvc-4/_static/image23.png "Fotoğraf Galerisi proje mobil desteği")</span><span class="sxs-lookup"><span data-stu-id="6518e-381">![Photo Gallery project with mobile support](whats-new-in-aspnet-mvc-4/_static/image23.png "Photo Gallery project with mobile support")</span></span>

        <span data-ttu-id="6518e-382">*Fotoğraf Galerisi proje mobil desteği*</span><span class="sxs-lookup"><span data-stu-id="6518e-382">*Photo Gallery project with mobile support*</span></span>
4. <span data-ttu-id="6518e-383">Mobil paketleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="6518e-383">Register the Mobile bundles.</span></span> <span data-ttu-id="6518e-384">Bunu yapmak için açık **Global.asax.cs** dosyasını açıp aşağıdaki satırı ekleyin.</span><span class="sxs-lookup"><span data-stu-id="6518e-384">To do this, open the **Global.asax.cs** file and add the following line.</span></span>

    <span data-ttu-id="6518e-385">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex03 - kayıt mobil paketleri*)</span><span class="sxs-lookup"><span data-stu-id="6518e-385">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - Register Mobile Bundles*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample10.cs)]
5. <span data-ttu-id="6518e-386">Bir masaüstü web tarayıcısını kullanarak uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6518e-386">Run the application using a desktop web browser.</span></span>
6. <span data-ttu-id="6518e-387">Açık **Windows Phone 7 öykünücüsü'nü** bulunan **Başlat menüsündeki | Tüm Programlar | Windows Phone SDK 7.1 | Windows Phone öykünücüsü.**</span><span class="sxs-lookup"><span data-stu-id="6518e-387">Open the **Windows Phone 7 Emulator,** located in **Start Menu | All Programs | Windows Phone SDK 7.1 | Windows Phone Emulator.**</span></span>
7. <span data-ttu-id="6518e-388">Telefonunuzun Başlangıç ekranına Internet Explorer'ı açın.</span><span class="sxs-lookup"><span data-stu-id="6518e-388">In the phone start screen, open Internet Explorer.</span></span> <span data-ttu-id="6518e-389">Uygulama başlatıldığı URL denetleyin ve söz konusu URL ile telefon tarayıcı göz atın (örneğin `http://localhost:[PortNumber]/`).</span><span class="sxs-lookup"><span data-stu-id="6518e-389">Check out the URL where the application started and browse to that URL with the phone browser (e.g. `http://localhost:[PortNumber]/`).</span></span>

    <span data-ttu-id="6518e-390">JQuery.Mobile.MVC mobil cihazlar için iyileştirilmiş görünümlerini Göster, projenizdeki yeni varlıklar oluşturan gibi uygulamanızı Windows Phone öykünücüsü'nde, farklı görünür olduğunu fark edeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-390">You will notice that your application will look different in the Windows Phone emulator, as the jQuery.Mobile.MVC has created new assets in your project that show views optimized for mobile devices.</span></span>

    <span data-ttu-id="6518e-391">İletinin üst kısmında telefon Masaüstü görünümüne geçer bağlantıyı gösteren dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-391">Notice the message at the top of the phone, showing the link that switches to the Desktop view.</span></span> <span data-ttu-id="6518e-392">Ayrıca,  **\_Layout.Mobile.cshtml** düzenini yüklediğiniz paketi tarafından oluşturulan uygulamada farklı bir düzene dahil.</span><span class="sxs-lookup"><span data-stu-id="6518e-392">Additionally, the **\_Layout.Mobile.cshtml** layout that was created by the package you have installed is including a different layout in the application.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6518e-393">Şu ana kadar mobil görünüme geri dönmek için bağlantı yok.</span><span class="sxs-lookup"><span data-stu-id="6518e-393">So far, there is no link to get back to mobile view.</span></span> <span data-ttu-id="6518e-394">Sonraki sürümlerde eklenecektir.</span><span class="sxs-lookup"><span data-stu-id="6518e-394">It will be included in later versions.</span></span>

    <span data-ttu-id="6518e-395">![Fotoğraf Galerisi giriş sayfası mobil görünüme](whats-new-in-aspnet-mvc-4/_static/image24.png "mobil görünüme Fotoğraf Galerisi giriş sayfası")</span><span class="sxs-lookup"><span data-stu-id="6518e-395">![Mobile view of the Photo Gallery Home page](whats-new-in-aspnet-mvc-4/_static/image24.png "Mobile view of the Photo Gallery Home page")</span></span>

    <span data-ttu-id="6518e-396">*Fotoğraf Galerisi giriş sayfası mobil Görünüm*</span><span class="sxs-lookup"><span data-stu-id="6518e-396">*Mobile view of the Photo Gallery Home page*</span></span>
8. <span data-ttu-id="6518e-397">Visual Studio'da **SHIFT** + **F5** uygulama hata ayıklamayı durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="6518e-397">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>

<a id="Task_2_-_Creating_Mobile_Views"></a>
#### <a name="task-2---creating-mobile-views"></a><span data-ttu-id="6518e-398">Görev 2 - mobil görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="6518e-398">Task 2 - Creating Mobile Views</span></span>

<span data-ttu-id="6518e-399">Bu görevde, mobil cihazlarda iyi appareance için uyarlanmış içerikle dizin görünümünün mobil bir sürümünü oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6518e-399">In this task, you will create a mobile version of the index view with content adapted for better appareance in mobile devices.</span></span>

1. <span data-ttu-id="6518e-400">Kopyalama **Views\Home\Index.cshtml** görüntülemek ve bir kopya oluşturmak için yeni dosyayı yeniden adlandır yapıştırın **Index.Mobile.cshtml**.</span><span class="sxs-lookup"><span data-stu-id="6518e-400">Copy the **Views\Home\Index.cshtml** view and paste it to create a copy, rename the new file to **Index.Mobile.cshtml**.</span></span>
2. <span data-ttu-id="6518e-401">Açık yeni oluşturulan **Index.Mobile.cshtml** görüntüleyin ve değiştirin &lt;ul&gt; bu kodla etiketi.</span><span class="sxs-lookup"><span data-stu-id="6518e-401">Open the new created **Index.Mobile.cshtml** view and replace the existing &lt;ul&gt; tag with this code.</span></span> <span data-ttu-id="6518e-402">Bunu yaparak, güncelleştiriliyor &lt;ul&gt; jQuery mobile temalarından kullanmak için jQuery Mobile veri ek açıklamaları etiketi.</span><span class="sxs-lookup"><span data-stu-id="6518e-402">By doing this, you will be updating the &lt;ul&gt; tag with jQuery Mobile data annotations to use the mobile themes from jQuery.</span></span>

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample11.html)]

    > [!NOTE] 
    > 
    > <span data-ttu-id="6518e-403">Şunlara dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="6518e-403">Notice that:</span></span>
    > 
    > - <span data-ttu-id="6518e-404">**Veri rolü** özniteliğini **listview** listview stilleri kullanarak listesinin işleyecek.</span><span class="sxs-lookup"><span data-stu-id="6518e-404">The **data-role** attribute set to **listview** will render the list using the listview styles.</span></span>
    > 
    > - <span data-ttu-id="6518e-405">**Veri dışarı** yuvarlatılmış kenarlık ve kenar boşluğu listesiyle özniteliği true olarak ayarlanırsa gösterir.</span><span class="sxs-lookup"><span data-stu-id="6518e-405">The **data-inset** attribute set to true will show the list with rounded border and margin.</span></span>
    > 
    > - <span data-ttu-id="6518e-406">**Veri filtresi** özniteliğini **true** bir arama kutusu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="6518e-406">The **data-filter** attribute set to **true** will generate a search box.</span></span>
    > 
    > <span data-ttu-id="6518e-407">Proje belgelerinde jQuery Mobile kuralları hakkında daha fazla bilgi edinebilirsiniz: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span><span class="sxs-lookup"><span data-stu-id="6518e-407">You can learn more about jQuery Mobile conventions in the project documentation: [[http://jquerymobile.com/demos/1.1.1/](http://jquerymobile.com/demos/1.1.1/)](http://jquerymobile.com/demos/1.1.1/)</span></span>
3. <span data-ttu-id="6518e-408">Tuşuna **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="6518e-408">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="6518e-409">Geçiş **Windows Phone öykünücüsü** ve sitesini yenileyin.</span><span class="sxs-lookup"><span data-stu-id="6518e-409">Switch to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="6518e-410">Yeni Azure görünümü ve deneyimini galeri listesinin yanı sıra üstte bulunan yeni arama kutusuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-410">Notice the new look and feel of the gallery list, as well as the new search box located on the top.</span></span> <span data-ttu-id="6518e-411">Ardından, arama kutusuna bir sözcük yazın (örneğin, **Tulips**) bir arama Fotoğraf Galerisi başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="6518e-411">Then, type a word in the search box (for instance, **Tulips**) to start a search in the photo gallery.</span></span>

    <span data-ttu-id="6518e-412">![ListView stili filtreleme ile kullanarak galeri](whats-new-in-aspnet-mvc-4/_static/image25.png "filtreleme ile listview stil kullanarak Galerisi")</span><span class="sxs-lookup"><span data-stu-id="6518e-412">![Gallery using listview style with filtering](whats-new-in-aspnet-mvc-4/_static/image25.png "Gallery using listview style with filtering")</span></span>

    <span data-ttu-id="6518e-413">*Galeri ListView stili filtreleme ile kullanma*</span><span class="sxs-lookup"><span data-stu-id="6518e-413">*Gallery using listview style with filtering*</span></span>

    <span data-ttu-id="6518e-414">Özetlemek gerekirse, görünüm Mobilizer tarif şekilde dizin görünümünün bir kopyasını oluşturmak için kullandığınız &quot;mobil&quot; soneki.</span><span class="sxs-lookup"><span data-stu-id="6518e-414">To summarize, you have used the View Mobilizer recipe to create a copy of the Index view with the &quot;mobile&quot; suffix.</span></span> <span data-ttu-id="6518e-415">Bu sonekin ASP.NET MVC 4'e bir mobil CİHAZDAN oluşturulan her bir istek bu kopyası dizini kullanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="6518e-415">This suffix indicates to ASP.NET MVC 4 that every request generated from a mobile device will use this copy of the index.</span></span> <span data-ttu-id="6518e-416">Ayrıca, mobil cihazlarda site görünümü ve deneyimini geliştirmek için jQuery Mobile'ı kullanmak için dizin görünümünün mobil sürümü güncelleştirdik.</span><span class="sxs-lookup"><span data-stu-id="6518e-416">Additionally, you have updated the mobile version of the Index view to use jQuery Mobile for enhancing the site look and feel in mobile devices.</span></span>
5. <span data-ttu-id="6518e-417">Visual Studio geri gidin ve açık **Site.Mobile.css** altında bulunan **içerik** klasör.</span><span class="sxs-lookup"><span data-stu-id="6518e-417">Go back to Visual Studio and open **Site.Mobile.css** located under the **Content** folder.</span></span>
6. <span data-ttu-id="6518e-418">Resmin sağ tarafında Göster hale Fotoğraf Başlığı konumlandırma düzeltin.</span><span class="sxs-lookup"><span data-stu-id="6518e-418">Fix the positioning of the photo title to make it show at the right side of the image.</span></span> <span data-ttu-id="6518e-419">Bunu yapmak için aşağıdaki kodu ekleyin. **Site.Mobile.css** dosya.</span><span class="sxs-lookup"><span data-stu-id="6518e-419">To do this, add the following code to the **Site.Mobile.css** file.</span></span>

    <span data-ttu-id="6518e-420">CSS</span><span class="sxs-lookup"><span data-stu-id="6518e-420">CSS</span></span>

    [!code-css[Main](whats-new-in-aspnet-mvc-4/samples/sample12.css)]
7. <span data-ttu-id="6518e-421">Tuşuna **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="6518e-421">Press **CTRL + S** to save the changes.</span></span>
8. <span data-ttu-id="6518e-422">Dönmek **Windows Phone öykünücüsü** ve sitesini yenileyin.</span><span class="sxs-lookup"><span data-stu-id="6518e-422">Switch back to the **Windows Phone Emulator** and refresh the site.</span></span> <span data-ttu-id="6518e-423">Fotoğraf Başlığı düzgün artık konumlandırıldı dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-423">Notice the photo title is properly positioned now.</span></span>

    <span data-ttu-id="6518e-424">![Resmin sağ tarafında konumlandırılan başlık](whats-new-in-aspnet-mvc-4/_static/image26.png "resmin sağ tarafında konumlandırılan başlığı")</span><span class="sxs-lookup"><span data-stu-id="6518e-424">![Title positioned on the right side of the image](whats-new-in-aspnet-mvc-4/_static/image26.png "Title positioned on the right side of the image")</span></span>

    <span data-ttu-id="6518e-425">*Resmin sağ tarafında konumlandırılan başlığı*</span><span class="sxs-lookup"><span data-stu-id="6518e-425">*Title positioned on the right side of the image*</span></span>

<a id="Task_3_-_jQuery_Mobile_Themes"></a>
#### <a name="task-3---jquery-mobile-themes"></a><span data-ttu-id="6518e-426">Görev 3 - jQuery Mobile temalar</span><span class="sxs-lookup"><span data-stu-id="6518e-426">Task 3 - jQuery Mobile Themes</span></span>

<span data-ttu-id="6518e-427">Her düzen ve pencere öğesinde jQuery Mobile tam birleşik görsel tasarım tema sitelere ve uygulamalara uygulamak mümkün kılan bir yeni nesne yönelimli CSS framework geçici olarak tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="6518e-427">Every layout and widget in jQuery Mobile is designed around a new object-oriented CSS framework that makes it possible to apply a complete unified visual design theme to sites and applications.</span></span>

<span data-ttu-id="6518e-428">jQuery Mobile'nın varsayılan tema harf verildiğinde 5 örneklerini içerir (e) hızlı başvuru için b, c, d.</span><span class="sxs-lookup"><span data-stu-id="6518e-428">jQuery Mobile's default Theme includes 5 swatches that are given letters (a, b, c, d, e) for quick reference.</span></span>

<span data-ttu-id="6518e-429">Bu görevde, varsayılandan farklı bir tema mobil düzenin güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="6518e-429">In this task, you will update the mobile layout to use a different theme than the default.</span></span>

1. <span data-ttu-id="6518e-430">Visual Studio'ya geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="6518e-430">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="6518e-431">Açık  **\_Layout.Mobile.cshtml** bulunan dosya **görünümler/paylaşılan**.</span><span class="sxs-lookup"><span data-stu-id="6518e-431">Open the **\_Layout.Mobile.cshtml** file located in **Views\Shared**.</span></span>
3. <span data-ttu-id="6518e-432">Div öğesinin ayarlamak veri rolüne sahip bulma &quot;sayfa&quot; ve güncelleştirme **data-theme** için &quot; **e**&quot;.</span><span class="sxs-lookup"><span data-stu-id="6518e-432">Find the div element with the data-role set to &quot;page&quot; and update the **data-theme** to &quot;**e**&quot;.</span></span>

    [!code-html[Main](whats-new-in-aspnet-mvc-4/samples/sample13.html)]
4. <span data-ttu-id="6518e-433">Tuşuna **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="6518e-433">Press **CTRL + S** to save the changes.</span></span>
5. <span data-ttu-id="6518e-434">Siteyi Yenile **Windows Phone öykünücüsü** ve yeni renk düzeni dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-434">Refresh the site in the **Windows Phone Emulator** and notice the new colors scheme.</span></span>

    <span data-ttu-id="6518e-435">![Farklı renk şeması bir mobil düzenin](whats-new-in-aspnet-mvc-4/_static/image27.png "farklı renk şeması ile mobil Düzen")</span><span class="sxs-lookup"><span data-stu-id="6518e-435">![Mobile layout with a different color scheme](whats-new-in-aspnet-mvc-4/_static/image27.png "Mobile layout with a different color scheme")</span></span>

    <span data-ttu-id="6518e-436">*Farklı renk şeması ile mobil Düzen*</span><span class="sxs-lookup"><span data-stu-id="6518e-436">*Mobile layout with a different color scheme*</span></span>

<a id="Task_4_-_Using_the_View-Switcher_Component_and_the_Browser_Overriding_Features"></a>
#### <a name="task-4---using-the-view-switcher-component-and-the-browser-overriding-features"></a><span data-ttu-id="6518e-437">Görev 4 - görünüm değiştirici bileşen ve tarayıcı geçersiz kılma özellikleri kullanma</span><span class="sxs-lookup"><span data-stu-id="6518e-437">Task 4 - Using the View-Switcher Component and the Browser Overriding Features</span></span>

<span data-ttu-id="6518e-438">Mobil için iyileştirilmiş web sayfaları için bir kuralı metni bir şey Masaüstü görünüm veya kullanıcıların bir masaüstü sürümüne geçiş olanak sağlayan tam site modu gibi olan bir bağlantı eklemektir.</span><span class="sxs-lookup"><span data-stu-id="6518e-438">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="6518e-439">Bir örnek jQuery.Mobile.MVC paket içerir **görünüm değiştirici** bu amaçla kullanılan bileşen  **\_Layout.Mobile.cshtml** görünümü.</span><span class="sxs-lookup"><span data-stu-id="6518e-439">The jQuery.Mobile.MVC package includes a sample **view-switcher** component for this purpose used in the **\_Layout.Mobile.cshtml** view.</span></span>

<span data-ttu-id="6518e-440">![Masaüstü görünümüne geçmek için bağlantı](whats-new-in-aspnet-mvc-4/_static/image28.png "Masaüstü görünümüne geçmek için bağlantı")</span><span class="sxs-lookup"><span data-stu-id="6518e-440">![Link to switch to Desktop View](whats-new-in-aspnet-mvc-4/_static/image28.png "Link to switch to Desktop View")</span></span>

<span data-ttu-id="6518e-441">*Masaüstü görünümüne geç bağlama*</span><span class="sxs-lookup"><span data-stu-id="6518e-441">*Link to switch to Desktop View*</span></span>

<span data-ttu-id="6518e-442">Görünüm değiştirici adlı yeni bir özellik kullanır **tarayıcı geçersiz kılma**.</span><span class="sxs-lookup"><span data-stu-id="6518e-442">The view switcher uses a new feature called **Browser Overriding**.</span></span> <span data-ttu-id="6518e-443">Bu özellik, bunlar gerçekten geldikleri olandan farklı bir tarayıcıdan (kullanıcı aracısı) çıkıyormuş gibi istekleri kabul uygulamanızı sağlar.</span><span class="sxs-lookup"><span data-stu-id="6518e-443">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they are actually coming from.</span></span>

<span data-ttu-id="6518e-444">Bu görevde, örnek uygulaması jQuery.Mobile.MVC ve geçersiz kılma özellikleri ASP.NET MVC 4'te yeni tarayıcı tarafından eklenen bir görünüm değiştirici inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-444">In this task, you will explore the sample implementation of a view-switcher added by jQuery.Mobile.MVC and the new browser overriding features in ASP.NET MVC 4.</span></span>

1. <span data-ttu-id="6518e-445">Visual Studio'ya geçiş yapın.</span><span class="sxs-lookup"><span data-stu-id="6518e-445">Switch back to Visual Studio.</span></span>
2. <span data-ttu-id="6518e-446">Açık  **\_Layout.Mobile.cshtml** görünümü altında yer **görünümler/paylaşılan** klasör ve bir kısmi görünüm olarak başvurulan görünüm değiştirici bileşen dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-446">Open the **\_Layout.Mobile.cshtml** view located under the **Views\Shared** folder and notice the view-switcher component being referenced as a partial view.</span></span>

    <span data-ttu-id="6518e-447">![Görünüm değiştirici bileşenini kullanarak mobil Düzen](whats-new-in-aspnet-mvc-4/_static/image29.png "görünüm değiştirici bileşenini kullanarak mobil Düzen")</span><span class="sxs-lookup"><span data-stu-id="6518e-447">![Mobile layout using View-Switcher component](whats-new-in-aspnet-mvc-4/_static/image29.png "Mobile layout using View-Switcher component")</span></span>

    <span data-ttu-id="6518e-448">*Görünüm değiştirici bileşenini kullanarak mobil Düzen*</span><span class="sxs-lookup"><span data-stu-id="6518e-448">*Mobile layout using View-Switcher component*</span></span>
3. <span data-ttu-id="6518e-449">Açık  **\_ViewSwitcher.cshtml** kısmi görünüm.</span><span class="sxs-lookup"><span data-stu-id="6518e-449">Open the **\_ViewSwitcher.cshtml** partial view.</span></span>

    <span data-ttu-id="6518e-450">Kısmi görünüm yeni kullanmaktadır **ViewContext.HttpContext.GetOverriddenBrowser()** web isteğinin kaynağını belirlemek ve Masaüstü veya mobil görünümler için ya da geçiş yapmak için ilgili bağlantıyı göstermek için.</span><span class="sxs-lookup"><span data-stu-id="6518e-450">The partial view uses the new method **ViewContext.HttpContext.GetOverriddenBrowser()** to determine the origin of the web request and show the corresponding link to switch either to the Desktop or Mobile views.</span></span>

    <span data-ttu-id="6518e-451">**GetOverridenBrowser** yöntemi döndürür bir **HttpBrowserCapabilitiesBase** istek için ayarlanan kullanıcı aracısı karşılık gelen bir örneği (gerçek ya da geçersiz kılınan).</span><span class="sxs-lookup"><span data-stu-id="6518e-451">The **GetOverridenBrowser** method returns an **HttpBrowserCapabilitiesBase** instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="6518e-452">Bu değer gibi özellikleri almak için kullanabileceğiniz **IsMobileDevice**.</span><span class="sxs-lookup"><span data-stu-id="6518e-452">You can use this value to get properties such as **IsMobileDevice**.</span></span>

    <span data-ttu-id="6518e-453">![Kısmi görünüm ViewSwitcher](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher kısmi Görünüm")</span><span class="sxs-lookup"><span data-stu-id="6518e-453">![ViewSwitcher partial view](whats-new-in-aspnet-mvc-4/_static/image30.png "ViewSwitcher partial view")</span></span>

    <span data-ttu-id="6518e-454">*ViewSwitcher kısmi Görünüm*</span><span class="sxs-lookup"><span data-stu-id="6518e-454">*ViewSwitcher partial view*</span></span>
4. <span data-ttu-id="6518e-455">Açık **ViewSwitcherController.cs** sınıfı bulunan **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="6518e-455">Open the **ViewSwitcherController.cs** class located in the **Controllers** folder.</span></span> <span data-ttu-id="6518e-456">Bu SwitchView eylemi kullanıma bağlantıya ViewSwitcher bileşen tarafından çağrılır ve yeni HttpContext yöntemleri dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-456">Check out that SwitchView action is called by the link in the ViewSwitcher component, and notice the new HttpContext methods.</span></span>

    - <span data-ttu-id="6518e-457">**HttpContext.ClearOverridenBrowser()** yöntemi, geçerli istek için herhangi bir geçersiz kılınan Kullanıcı aracısını kaldırır.</span><span class="sxs-lookup"><span data-stu-id="6518e-457">The **HttpContext.ClearOverridenBrowser()** method removes any overridden user agent for the current request.</span></span>
    - <span data-ttu-id="6518e-458">**HttpContext.SetOverridenBrowser()** yöntemi isteğin asıl kullanıcı aracısı değerini belirtilen kullanıcı aracısını kullanarak geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="6518e-458">The **HttpContext.SetOverridenBrowser()** method overrides the request's actual user agent value using the specified user agent.</span></span>  
        <span data-ttu-id="6518e-459">![ViewSwitcher denetleyicisi](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher denetleyicisi")</span><span class="sxs-lookup"><span data-stu-id="6518e-459">![ViewSwitcher Controller](whats-new-in-aspnet-mvc-4/_static/image31.png "ViewSwitcher Controller")</span></span>  
<span data-ttu-id="6518e-460">*ViewSwitcher denetleyicisi*</span><span class="sxs-lookup"><span data-stu-id="6518e-460">*ViewSwitcher Controller*</span></span>

        <span data-ttu-id="6518e-461">Tarayıcı geçersiz kılmak çekirdek ASP.NET MVC 4 jQuery.Mobile.MVC paketi yüklemezseniz bile aynı zamanda kullanılabilir olan özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="6518e-461">Browser Overriding is a core feature of ASP.NET MVC 4, which is also available even if you do not install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="6518e-462">Ancak, bu özellik yalnızca görüntüleme, Düzen ve kısmi görünüm etkiler ve Request.Browser nesneye bağlı özelliklerinden herhangi birinin etkilemez.</span><span class="sxs-lookup"><span data-stu-id="6518e-462">However, this feature affects only view, layout, and partial-view, and it does not affect any of the features that depend on the Request.Browser object.</span></span>

<a id="Task_5_-_Adding_the_View-Switcher_in_the_Desktop_View"></a>
#### <a name="task-5---adding-the-view-switcher-in-the-desktop-view"></a><span data-ttu-id="6518e-463">Görev 5 - görünüm değiştirici Masaüstü görünüm ekleme</span><span class="sxs-lookup"><span data-stu-id="6518e-463">Task 5 - Adding the View-Switcher in the Desktop View</span></span>

<span data-ttu-id="6518e-464">Bu görevde, Masaüstü düzenini görünüm değiştirici içerecek şekilde güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="6518e-464">In this task, you will update the desktop layout to include the view-switcher.</span></span> <span data-ttu-id="6518e-465">Bu Masaüstü görünümünde gezinirken mobil görünüme geri dönmek mobil kullanıcılar izin verir.</span><span class="sxs-lookup"><span data-stu-id="6518e-465">This will allow mobile users to go back to the mobile view when browsing the desktop view.</span></span>

1. <span data-ttu-id="6518e-466">Siteyi Yenile **Windows Phone öykünücüsü**.</span><span class="sxs-lookup"><span data-stu-id="6518e-466">Refresh the site in the **Windows Phone Emulator**.</span></span>
2. <span data-ttu-id="6518e-467">Tıklayarak **Masaüstü Görünüm** galerinin üstündeki bağlantısı.</span><span class="sxs-lookup"><span data-stu-id="6518e-467">Click on the **Desktop view** link at the top of the gallery.</span></span> <span data-ttu-id="6518e-468">Görünüm değiştirici mobil görünüme dönmek izin vermek için Masaüstü görünümünde dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-468">Notice there is no view-switcher in the desktop view to allow you return to the mobile view.</span></span>
3. <span data-ttu-id="6518e-469">Visual Studio geri gidin ve açık  **\_Layout.cshtml** görünümü.</span><span class="sxs-lookup"><span data-stu-id="6518e-469">Go back to Visual Studio and open the **\_Layout.cshtml** view.</span></span>
4. <span data-ttu-id="6518e-470">Oturum açma bölümü bulun ve işlemek için bir çağrı ekleyin  **\_ViewSwitcher** kısmi görünüm aşağıdaki  **\_LogOnPartial** kısmi görünüm.</span><span class="sxs-lookup"><span data-stu-id="6518e-470">Find the login section and insert a call to render the **\_ViewSwitcher** partial view below the **\_LogOnPartial** partial view.</span></span> <span data-ttu-id="6518e-471">Ardından, basın **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="6518e-471">Then, press **CTRL + S** to save the changes.</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample14.cshtml)]
5. <span data-ttu-id="6518e-472">Tuşuna **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="6518e-472">Press **CTRL + S** to save the changes.</span></span>
6. <span data-ttu-id="6518e-473">Windows Phone öykünücüsü sayfayı yeniler ve ekran yakınlaştırmak için çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6518e-473">Refresh the page in the Windows Phone Emulator and double-click the screen to zoom in.</span></span> <span data-ttu-id="6518e-474">Giriş sayfası gösterdiğini bildirimi **mobil görünüme** mobil masaüstü görünümüne geçer bağlantı.</span><span class="sxs-lookup"><span data-stu-id="6518e-474">Notice that the home page now shows the **Mobile view** link that switches from mobile to desktop view.</span></span>

    <span data-ttu-id="6518e-475">![Masaüstü görünümünde işlenen değiştirici görüntülemek](whats-new-in-aspnet-mvc-4/_static/image32.png "Masaüstü görünümünde oluşturulması görünüm değiştirici")</span><span class="sxs-lookup"><span data-stu-id="6518e-475">![View Switcher rendered in desktop view](whats-new-in-aspnet-mvc-4/_static/image32.png "View Switcher rendered in desktop view")</span></span>

    <span data-ttu-id="6518e-476">*Masaüstü görünümünde oluşturulması görünüm değiştirici*</span><span class="sxs-lookup"><span data-stu-id="6518e-476">*View Switcher rendered in desktop view*</span></span>
7. <span data-ttu-id="6518e-477">Göz atın ve mobil görünüme geçiş yeniden <strong>hakkında</strong> sayfa (http://localhosthakkında [bağlantı noktası] / Home /).</span><span class="sxs-lookup"><span data-stu-id="6518e-477">Switch to the Mobile view again and browse to <strong>About</strong> page (http://localhost[port]/Home/About).</span></span> <span data-ttu-id="6518e-478">About.Mobile.cshtml görünümü oluşturmadıysanız bile hakkında sayfası mobil düzen kullanılarak görüntülenir, dikkat edin (\_Layout.Mobile.cshtml).</span><span class="sxs-lookup"><span data-stu-id="6518e-478">Notice that, even if you haven't created an About.Mobile.cshtml view, the About page is displayed using the mobile layout (\_Layout.Mobile.cshtml).</span></span>

    <span data-ttu-id="6518e-479">![Sayfa hakkında](whats-new-in-aspnet-mvc-4/_static/image33.png "sayfa hakkında")</span><span class="sxs-lookup"><span data-stu-id="6518e-479">![About page](whats-new-in-aspnet-mvc-4/_static/image33.png "About page")</span></span>

    <span data-ttu-id="6518e-480">*Sayfa hakkında*</span><span class="sxs-lookup"><span data-stu-id="6518e-480">*About page*</span></span>
8. <span data-ttu-id="6518e-481">Son olarak, site bir Masaüstü Web tarayıcısında açın.</span><span class="sxs-lookup"><span data-stu-id="6518e-481">Finally, open the site in a desktop Web browser.</span></span> <span data-ttu-id="6518e-482">Önceki güncelleştirmelerinin hiçbiri Masaüstü görünüm etkiledi dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-482">Notice that none of the previous updates has affected the desktop view.</span></span>

    <span data-ttu-id="6518e-483">![Masaüstü görünüm Fotografgalerisi](whats-new-in-aspnet-mvc-4/_static/image34.png "Fotografgalerisi Masaüstü Görünüm")</span><span class="sxs-lookup"><span data-stu-id="6518e-483">![PhotoGallery desktop view](whats-new-in-aspnet-mvc-4/_static/image34.png "PhotoGallery desktop view")</span></span>

    <span data-ttu-id="6518e-484">*Fotografgalerisi Masaüstü Görünüm*</span><span class="sxs-lookup"><span data-stu-id="6518e-484">*PhotoGallery desktop view*</span></span>

<a id="Task_6_-_Creating_New_Display_Modes"></a>
#### <a name="task-6---creating-new-display-modes"></a><span data-ttu-id="6518e-485">Görev 6 - oluşturma, yeni görüntü modları</span><span class="sxs-lookup"><span data-stu-id="6518e-485">Task 6 - Creating New Display Modes</span></span>

<span data-ttu-id="6518e-486">Yeni görüntü modu özelliğini isteği oluşturma tarayıcı bağlı olarak görünümleri seçin bir uygulama sağlar.</span><span class="sxs-lookup"><span data-stu-id="6518e-486">The new display modes feature lets an application select views depending on the browser that is generating the request.</span></span> <span data-ttu-id="6518e-487">Örneğin, bir masaüstü tarayıcısı giriş sayfasını isterse, uygulama döndüreceği **Views\Home\Index.cshtml** şablonu.</span><span class="sxs-lookup"><span data-stu-id="6518e-487">For example, if a desktop browser requests the Home page, the application will return the **Views\Home\Index.cshtml** template.</span></span> <span data-ttu-id="6518e-488">Daha sonra uygulama giriş sayfası mobil tarayıcı isteğinde bulunursa döndürür **Views\Home\Index.mobile.cshtml** şablonu.</span><span class="sxs-lookup"><span data-stu-id="6518e-488">Then, if a mobile browser requests the Home page, the application will return the **Views\Home\Index.mobile.cshtml** template.</span></span>

<span data-ttu-id="6518e-489">Bu görevde, iPhone cihazları için özel bir düzen oluşturacak ve iPhone cihazları gelen istekleri benzetimini yapmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="6518e-489">In this task, you will create a customized layout for iPhone devices, and you will have to simulate requests from iPhone devices.</span></span> <span data-ttu-id="6518e-490">Bunu yapmak için ya da bir iPhone öykünücüsü/simülatör kullanabilirsiniz (gibi [elektrik mobil simülatör](http://www.electricplum.com/)) veya bir tarayıcı kullanıcı aracısı değiştirme eklentiler ile.</span><span class="sxs-lookup"><span data-stu-id="6518e-490">To do this, you can use either an iPhone emulator/simulator (like [Electric Mobile Simulator](http://www.electricplum.com/)) or a browser with add-ons that modify the user agent.</span></span> <span data-ttu-id="6518e-491">İPhone benzetmek bir Safari tarayıcı kullanıcı aracısı dizesi ayarlama konusunda yönergeler için [IE olduğu anlatabilirsiniz Safari sağlama](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) David geçiş'ın blogunda.</span><span class="sxs-lookup"><span data-stu-id="6518e-491">For instructions on how to set the user agent string in an Safari browser to emulate an iPhone, see [How to let Safari pretend it's IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) in David Alison's blog.</span></span>

<span data-ttu-id="6518e-492">**Bu görev isteğe bağlı olduğuna dikkat edin ve Laboratuvar yürütme olmadan devam edebilirsiniz.**</span><span class="sxs-lookup"><span data-stu-id="6518e-492">**Notice that this task is optional and you can continue throughout the lab without executing it.**</span></span>

1. <span data-ttu-id="6518e-493">Visual Studio'da **SHIFT** + **F5** uygulama hata ayıklamayı durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="6518e-493">In Visual Studio, press **SHIFT** + **F5** to stop debugging the application.</span></span>
2. <span data-ttu-id="6518e-494">Açık **Global.asax.cs** ve aşağıdaki deyimi kullanarak.</span><span class="sxs-lookup"><span data-stu-id="6518e-494">Open **Global.asax.cs** and add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample15.cs)]
3. <span data-ttu-id="6518e-495">Uygulamaya aşağıdaki vurgulanmış kodu ekleyin\_yöntemi başlatın.</span><span class="sxs-lookup"><span data-stu-id="6518e-495">Add the following highlighted code into the Application\_Start method.</span></span>

    <span data-ttu-id="6518e-496">(Kod parçacığını - *ASP.NET MVC 4 Laboratuvar - Ex03 - iPhone DisplayMode*)</span><span class="sxs-lookup"><span data-stu-id="6518e-496">(Code Snippet - *ASP.NET MVC 4 Lab - Ex03 - iPhone DisplayMode*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample16.cs)]

<span data-ttu-id="6518e-497">Yeni kaydettiğiniz **adlı DefaultDisplayMode &quot;iPhone&quot;**, statik içinde **DisplayModeProvider.Instance.Modes** karşı eşleşen statik listesi her gelen istek.</span><span class="sxs-lookup"><span data-stu-id="6518e-497">You have registered a new **DefaultDisplayMode named &quot;iPhone&quot;**, within the static **DisplayModeProvider.Instance.Modes** static list, that will be matched against each incoming request.</span></span> <span data-ttu-id="6518e-498">Gelen istek dize içeriyorsa &quot;iPhone&quot;, ASP.NET MVC görünümleri adını içeren bulacaksınız &quot;iPhone&quot; soneki.</span><span class="sxs-lookup"><span data-stu-id="6518e-498">If the incoming request contains the string &quot;iPhone&quot;, ASP.NET MVC will find the views whose name contain the &quot;iPhone&quot; suffix.</span></span> <span data-ttu-id="6518e-499">0 parametresi, yeni modu nasıl özel olduğunu belirtir; Örneğin, bu görünümü genel ayrıntılı &quot;.mobile&quot; mobil cihazlardan gelen isteklere eşleşen kural.</span><span class="sxs-lookup"><span data-stu-id="6518e-499">The 0 parameter indicates how specific is the new mode; for instance, this view is more specific than the general &quot;.mobile&quot; rule that matches requests from mobile devices.</span></span>

<span data-ttu-id="6518e-500">Bir iPhone tarayıcısı, isteği oluşturduğunda bu kod çalıştırıldıktan sonra uygulamanızın kullanacağı **görünümler/paylaşılan\\_Layout.iPhone.cshtml** sonraki adımda oluşturacağınız düzeni.</span><span class="sxs-lookup"><span data-stu-id="6518e-500">After this code runs, when an iPhone browser generates a request, your application will use the **Views\Shared\\_Layout.iPhone.cshtml** layout you will create in the next steps.</span></span>

> [!NOTE]
> <span data-ttu-id="6518e-501">İPhone için Tanıtım amaçlı Basitleştirilmiş ve (örneğin test büyük küçük harfe duyarlı olması nedeniyle), her iPhone kullanıcı aracısı dizesi beklendiği gibi çalışmayabilir, istek testinin bu şekilde.</span><span class="sxs-lookup"><span data-stu-id="6518e-501">This way of testing the request for iPhone has been simplified for demo purposes and might not work as expected for every iPhone user agent string (for example test is case sensitive).</span></span>

4. <span data-ttu-id="6518e-502">Bir kopyasını oluşturmak  **\_Layout.Mobile.cshtml** dosyası **görünümler/paylaşılan** klasörü ve kopyalayın adlandırın &quot; **\_Layout.iPhone.csthml**&quot;.</span><span class="sxs-lookup"><span data-stu-id="6518e-502">Create a copy of the **\_Layout.Mobile.cshtml** file in the **Views\Shared** folder and rename the copy to &quot;**\_Layout.iPhone.csthml**&quot;.</span></span>
5. <span data-ttu-id="6518e-503">Açık  **\_Layout.iPhone.csthml** önceki adımda oluşturduğunuz.</span><span class="sxs-lookup"><span data-stu-id="6518e-503">Open **\_Layout.iPhone.csthml** you created in the previous step.</span></span>
6. <span data-ttu-id="6518e-504">Div öğesinin ayarlanmış veri role özniteliğini bulmak **sayfa** değiştirip **data-theme** özniteliğini &quot; **bir**&quot;.</span><span class="sxs-lookup"><span data-stu-id="6518e-504">Find the div element with the data-role attribute set to **page** and change the **data-theme** attribute to &quot;**a**&quot;.</span></span>


[!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample17.cshtml)]

<span data-ttu-id="6518e-505">Artık ASP.NET MVC 4 uygulamanızı 3 düzenlere sahip:</span><span class="sxs-lookup"><span data-stu-id="6518e-505">Now you have 3 layouts in your ASP.NET MVC 4 application:</span></span>

1. <span data-ttu-id="6518e-506">**\_Layout.cshtml**: masaüstü tarayıcıları için kullanılan varsayılan düzen.</span><span class="sxs-lookup"><span data-stu-id="6518e-506">**\_Layout.cshtml**: default layout used for desktop browsers.</span></span>
2. <span data-ttu-id="6518e-507">**\_Layout.Mobile.cshtml**: mobil cihazlar için kullanılan varsayılan düzen.</span><span class="sxs-lookup"><span data-stu-id="6518e-507">**\_Layout.mobile.cshtml**: default layout used for mobile devices.</span></span>
3. <span data-ttu-id="6518e-508">**\_Layout.iPhone.cshtml**: ayırmak farklı renk düzenini kullanarak, iPhone cihazları için özel düzen \_Layout.mobile.cshtml.</span><span class="sxs-lookup"><span data-stu-id="6518e-508">**\_Layout.iPhone.cshtml**: specific layout for iPhone devices, using a different color scheme to differentiate from \_Layout.mobile.cshtml.</span></span>
7. <span data-ttu-id="6518e-509">Tuşuna **F5** uygulamayı çalıştırın ve site için göz atmayı **Windows Phone öykünücüsü**.</span><span class="sxs-lookup"><span data-stu-id="6518e-509">Press **F5** to run the application and browse the site in the **Windows Phone Emulator**.</span></span>
8. <span data-ttu-id="6518e-510">Açık bir **iPhone benzeticisi** (bkz [ek C](#AppendixC) yükleme ve bir iPhone benzeticisi yapılandırma konusunda yönergeler için) ve çok siteye göz atın.</span><span class="sxs-lookup"><span data-stu-id="6518e-510">Open an **iPhone simulator** (see [Appendix C](#AppendixC) for instructions on how to install and configure an iPhone simulator), and browse to the site too.</span></span> <span data-ttu-id="6518e-511">Belirli bir şablon her telefon kullandığını dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-511">Notice that each phone is using the specific template.</span></span>

    ![Using-different-views-for-each-mobile-device2](whats-new-in-aspnet-mvc-4/_static/image35.png)

    <span data-ttu-id="6518e-513">*Tüm mobil cihazlar için farklı görünümleri kullanma*</span><span class="sxs-lookup"><span data-stu-id="6518e-513">*Using different views for each mobile device*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Using_Asynchronous_Controllers"></a>
### <a name="exercise-4-using-asynchronous-controllers"></a><span data-ttu-id="6518e-514">Alıştırma 4: Zaman uyumsuz denetleyicilerini kullanma</span><span class="sxs-lookup"><span data-stu-id="6518e-514">Exercise 4: Using Asynchronous Controllers</span></span>

<span data-ttu-id="6518e-515">Microsoft .NET Framework 4.5, C# ve Visual Basic .NET programlama zaman uyumsuzluk için yeni bir temel sağlayacak yeni dil özellikleri tanıtır.</span><span class="sxs-lookup"><span data-stu-id="6518e-515">Microsoft .NET Framework 4.5 introduces new language features in C# and Visual Basic to provide a new foundation for asynchrony in .NET programming.</span></span> <span data-ttu-id="6518e-516">Bu yeni foundation zaman uyumsuz programlama-benzer ve yaklaşık olarak basit olarak - eşzamanlı programlama yapar.</span><span class="sxs-lookup"><span data-stu-id="6518e-516">This new foundation makes asynchronous programming similar to - and about as straightforward as - synchronous programming.</span></span> <span data-ttu-id="6518e-517">Şimdi kullanarak ASP.NET MVC 4'te zaman uyumsuz eylem yöntemleri yazabilmesi **AsyncController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="6518e-517">You are now able to write asynchronous action methods in ASP.NET MVC 4 by using the **AsyncController** class.</span></span> <span data-ttu-id="6518e-518">Uzun süre çalışan zaman uyumsuz eylem yöntemlerini kullanabilirsiniz, CPU dışı istekleri bağlı.</span><span class="sxs-lookup"><span data-stu-id="6518e-518">You can use asynchronous action methods for long-running, non-CPU bound requests.</span></span> <span data-ttu-id="6518e-519">Bu, Web sunucusu isteği işlenirken iş gerçekleştirmeyi engelleme önler.</span><span class="sxs-lookup"><span data-stu-id="6518e-519">This avoids blocking the Web server from performing work while the request is being processed.</span></span> <span data-ttu-id="6518e-520">AsyncController sınıf genellikle uzun süre çalışan Web hizmeti çağrıları için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="6518e-520">The AsyncController class is typically used for long-running Web service calls.</span></span>

<span data-ttu-id="6518e-521">Bu alıştırmada, ASP.NET MVC 4'te zaman uyumsuz işlem temelleri açıklanır.</span><span class="sxs-lookup"><span data-stu-id="6518e-521">This exercise explains the basics of asynchronous operation in ASP.NET MVC 4.</span></span> <span data-ttu-id="6518e-522">Bir daha ayrıntılı bilgi edinmek istiyorsanız aşağıdaki makaleye denetleyebilirsiniz: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span><span class="sxs-lookup"><span data-stu-id="6518e-522">If you want a deeper dive, you can check out the following article: [[https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)](https://msdn.microsoft.com/library/ee728598%28v=vs.100%29.aspx)</span></span>

<a id="Task_1_-_Implementing_an_Asynchronous_Controller"></a>
#### <a name="task-1---implementing-an-asynchronous-controller"></a><span data-ttu-id="6518e-523">Görev 1 - zaman uyumsuz bir denetleyiciyi uygulama</span><span class="sxs-lookup"><span data-stu-id="6518e-523">Task 1 - Implementing an Asynchronous Controller</span></span>

1. <span data-ttu-id="6518e-524">Açık **başlamak** çözüm bulunan **kaynak/Ex4-Async/başlangıç/** klasör.</span><span class="sxs-lookup"><span data-stu-id="6518e-524">Open the **Begin** solution located at **Source/Ex4-Async/Begin/** folder.</span></span> <span data-ttu-id="6518e-525">Aksi takdirde kullanarak devam edebilir **son** çözüm elde edilen önceki egzersizini tamamlayarak.</span><span class="sxs-lookup"><span data-stu-id="6518e-525">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

   1. <span data-ttu-id="6518e-526">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="6518e-526">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="6518e-527">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="6518e-527">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="6518e-528">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="6518e-528">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="6518e-529">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="6518e-529">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="6518e-530">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="6518e-530">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="6518e-531">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="6518e-531">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="6518e-532">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="6518e-532">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
2. <span data-ttu-id="6518e-533">Açık **HomeController.cs** gelen sınıfı **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="6518e-533">Open the **HomeController.cs** class from the **Controllers** folder.</span></span>
3. <span data-ttu-id="6518e-534">Aşağıdaki using deyimi.</span><span class="sxs-lookup"><span data-stu-id="6518e-534">Add the following using statement.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample18.cs)]
4. <span data-ttu-id="6518e-535">Güncelleştirme **HomeController** devralınacak sınıfı **AsyncController**.</span><span class="sxs-lookup"><span data-stu-id="6518e-535">Update the **HomeController** class to inherit from **AsyncController**.</span></span> <span data-ttu-id="6518e-536">Zaman uyumsuz istek işlemeye ASP.NET AsyncController türetilen denetleyicileri etkinleştirin ve hala hizmeti zaman uyumlu eylem yöntemleri yapabilirler.</span><span class="sxs-lookup"><span data-stu-id="6518e-536">Controllers that derive from AsyncController enable ASP.NET to process asynchronous requests, and they can still service synchronous action methods.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample19.cs)]
5. <span data-ttu-id="6518e-537">Ekleme **zaman uyumsuz** anahtar **dizin** yöntemi ve dönüş türü hale **görev&lt;actionresult öğesini&gt;**.</span><span class="sxs-lookup"><span data-stu-id="6518e-537">Add the **async** keyword to the **Index** method and make it return the type **Task&lt;ActionResult&gt;**.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample20.cs)]

    > [!NOTE]
    > <span data-ttu-id="6518e-538">**Zaman uyumsuz** anahtar sözcüğü, .NET Framework 4.5 sağlayan yeni anahtar sözcüklerle biridir; bu yöntemini zaman uyumsuz kod içeren derleyiciye bildirir.</span><span class="sxs-lookup"><span data-stu-id="6518e-538">The **async** keyword is one of the new keywords the .NET Framework 4.5 provides; it tells the compiler that this method contains asynchronous code.</span></span> <span data-ttu-id="6518e-539">A **görev** nesne belirli bir noktada gelecekte tamamlanabilir bir zaman uyumsuz işlemi temsil eder.</span><span class="sxs-lookup"><span data-stu-id="6518e-539">A **Task** object represents an asynchronous operation that may complete at some point in the future.</span></span>
6. <span data-ttu-id="6518e-540">Değiştirin **istemci. GetAsync()** aşağıda gösterildiği gibi kullanarak tam zaman uyumsuz sürümü ile çağrı await anahtar sözcüğü.</span><span class="sxs-lookup"><span data-stu-id="6518e-540">Replace the **client.GetAsync()** call with the full async version using await keyword as shown below.</span></span>

    <span data-ttu-id="6518e-541">(Kod parçacığını - *ASP.NET MVC 4 - Ex04 - Laboratuvar GetAsync*)</span><span class="sxs-lookup"><span data-stu-id="6518e-541">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - GetAsync*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample21.cs)]

    > [!NOTE]
    > <span data-ttu-id="6518e-542">Önceki sürümde, kullanmakta olduğunuz **sonucu** özelliğinden **görev** sonucu (eşitleme sürüm) döndürülünceye kadar iş parçacığını engellemek için nesne.</span><span class="sxs-lookup"><span data-stu-id="6518e-542">In the previous version, you were using the **Result** property from the **Task** object to block the thread until the result is returned (sync version).</span></span>
    > 
    > <span data-ttu-id="6518e-543">Ekleme **await** anahtar sözcüğü, derleyicinin zaman uyumsuz yöntemin çağrısından döndürülen göreve bekleyin söyler.</span><span class="sxs-lookup"><span data-stu-id="6518e-543">Adding the **await** keyword tells the compiler to asynchronously wait for the task returned from the method call.</span></span> <span data-ttu-id="6518e-544">Bu, kodun geri kalanını yalnızca beklenen yöntemi tamamlandıktan sonra geri arama olarak yürütülecek anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="6518e-544">This means that the rest of the code will be executed as a callback only after the awaited method completes.</span></span> <span data-ttu-id="6518e-545">Başka bir şey fark, try-catch bloğu Bunun çalışmasını sağlamak için değiştirmeniz gerekmez olduğu: ön plan veya arka planda gerçekleşen özel durumları hala framework tarafından sağlanan bir işleyici kullanarak herhangi bir ek çalışma yapmadan yakalanır.</span><span class="sxs-lookup"><span data-stu-id="6518e-545">Another thing to notice is that you do not need to change your try-catch block in order to make this work: the exceptions that happen in background or in foreground will still be caught without any extra work using a handler provided by the framework.</span></span>
7. <span data-ttu-id="6518e-546">Aşağıda gösterildiği gibi yeni kod iler satırları değiştirerek zaman uyumsuz bir uygulama ile devam etmek için kodu değiştirin</span><span class="sxs-lookup"><span data-stu-id="6518e-546">Change the code to continue with the asynchronous implementation by replacing the lines with the new code as shown below</span></span>

    <span data-ttu-id="6518e-547">(Kod parçacığını - *ASP.NET MVC 4 - Ex04 - Laboratuvar ReadAsStringAsync*)</span><span class="sxs-lookup"><span data-stu-id="6518e-547">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - ReadAsStringAsync*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample22.cs)]
8. <span data-ttu-id="6518e-548">Uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6518e-548">Run the application.</span></span> <span data-ttu-id="6518e-549">Hiçbir önemli değişiklikler fark edeceksiniz, ancak bir iş parçacığından yapmak daha iyi bir sunucu kaynak kullanımı ve performansı geliştirme iş parçacığı havuzu kodunuzu engellemez.</span><span class="sxs-lookup"><span data-stu-id="6518e-549">You will notice no major changes, but your code will not block a thread from the thread pool making a better usage of the server resources and improving performance.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6518e-550">Laboratuvardaki yeni zaman uyumsuz programlama özellikleri hakkında daha fazla bilgi &quot; **C# ve Visual Basic ile .NET 4.5 içinde zaman uyumsuz programlama** &quot; Visual Studio eğitim Seti dahildir.</span><span class="sxs-lookup"><span data-stu-id="6518e-550">You can learn more about the new asynchronous programming features in the lab &quot;**Asynchronous Programming in .NET 4.5 with C# and Visual Basic**&quot; included in the Visual Studio Training Kit.</span></span>

<a id="Task_2_-_Handling_Time-Outs_with_Cancellation_Tokens"></a>
#### <a name="task-2---handling-time-outs-with-cancellation-tokens"></a><span data-ttu-id="6518e-551">Görev 2 - işleme zaman aşımı ile iptal belirteçleri</span><span class="sxs-lookup"><span data-stu-id="6518e-551">Task 2 - Handling Time-Outs with Cancellation Tokens</span></span>

<span data-ttu-id="6518e-552">Görev örnekleri döndüren zaman uyumsuz eylem yöntemleri de zaman aşımlarını destekler.</span><span class="sxs-lookup"><span data-stu-id="6518e-552">Asynchronous action methods that return Task instances can also support time-outs.</span></span> <span data-ttu-id="6518e-553">Bu görevde, bir iptal belirteci kullanarak bir zaman aşımı senaryonun işlenmesi için dizin yöntemi kodu güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="6518e-553">In this task, you will update the Index method code to handle a time-out scenario using a cancellation token.</span></span>

1. <span data-ttu-id="6518e-554">Geri Git Visual Studio ve ENTER tuşuna **SHIFT + F5 tuşlarına basarak** hata ayıklamayı durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="6518e-554">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>
2. <span data-ttu-id="6518e-555">Aşağıdaki deyimi kullanarak **HomeController.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="6518e-555">Add the following using statement to the **HomeController.cs** file.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample23.cs)]
3. <span data-ttu-id="6518e-556">Güncelleştirme almak için dizin eylemi bir **CancellationToken** bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="6518e-556">Update the Index action to receive a **CancellationToken** argument.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample24.cs)]
4. <span data-ttu-id="6518e-557">Güncelleştirme **GetAsync** Çağrı iptal belirteci geçirmek.</span><span class="sxs-lookup"><span data-stu-id="6518e-557">Update the **GetAsync** call to pass the cancellation token.</span></span>

    <span data-ttu-id="6518e-558">(Kod parçacığını - *CancellationToken ile ASP.NET MVC 4 - Ex04 - Laboratuvar SendAsync*)</span><span class="sxs-lookup"><span data-stu-id="6518e-558">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - SendAsync with CancellationToken*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample25.cs)]
5. <span data-ttu-id="6518e-559">Süslemek *dizin* yöntemi ile bir **AsyncTimeout** özniteliği ayarlanmış için aralığını 500 milisaniyenin ve **HandleError** işlemek için yapılandırılmış öznitelik  **TaskCanceledException** yönlendirmek için bir **zaman aşımına uğradı** görünümü.</span><span class="sxs-lookup"><span data-stu-id="6518e-559">Decorate the *Index* method with an **AsyncTimeout** attribute set to 500 milliseconds and a **HandleError** attribute configured to handle **TaskCanceledException** by redirecting to a **TimedOut** view.</span></span>

    <span data-ttu-id="6518e-560">(Kod parçacığını - *ASP.NET MVC 4 - Ex04 - Laboratuvar öznitelikleri*)</span><span class="sxs-lookup"><span data-stu-id="6518e-560">(Code Snippet - *ASP.NET MVC 4 Lab - Ex04 - Attributes*)</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample26.cs)]
6. <span data-ttu-id="6518e-561">Açık **PhotoController** sınıfı ve güncelleştirme **galeri** yürütme 1000 milisaniye (1 saniye) uzun süre çalışan bir görevin benzetimini yapmak için gecikme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="6518e-561">Open the **PhotoController** class and update the **Gallery** method to delay the execution 1000 miliseconds (1 second) to simulate a long running task.</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-mvc-4/samples/sample27.cs)]
7. <span data-ttu-id="6518e-562">Açık **Web.config** dosya ve özel hataları öğesi ekleyerek etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="6518e-562">Open the **Web.config** file and enable custom errors by adding the following element.</span></span>

    [!code-xml[Main](whats-new-in-aspnet-mvc-4/samples/sample28.xml)]
8. <span data-ttu-id="6518e-563">Yeni bir görünüm oluşturma **görünümler/paylaşılan** adlı **zaman aşımına uğradı** ve varsayılan düzenini kullanın.</span><span class="sxs-lookup"><span data-stu-id="6518e-563">Create a new view in **Views\Shared** named **TimedOut** and use the default layout.</span></span> <span data-ttu-id="6518e-564">Çözüm Gezgini'nde sağ **görünümler/paylaşılan** klasörü ve select **Ekle | Görünüm**.</span><span class="sxs-lookup"><span data-stu-id="6518e-564">In the Solution Explorer, right-click the **Views\Shared** folder and select **Add | View**.</span></span>

    <span data-ttu-id="6518e-565">![Tüm mobil cihazlar için farklı görünümleri kullanma](whats-new-in-aspnet-mvc-4/_static/image36.png "tüm mobil cihazlar için farklı görünümleri kullanma")</span><span class="sxs-lookup"><span data-stu-id="6518e-565">![Using different views for each mobile device](whats-new-in-aspnet-mvc-4/_static/image36.png "Using different views for each mobile device")</span></span>

    <span data-ttu-id="6518e-566">*Tüm mobil cihazlar için farklı görünümleri kullanma*</span><span class="sxs-lookup"><span data-stu-id="6518e-566">*Using different views for each mobile device*</span></span>
9. <span data-ttu-id="6518e-567">Güncelleştirme **zaman aşımına uğradı** aşağıda gösterildiği gibi içeriği görüntüle.</span><span class="sxs-lookup"><span data-stu-id="6518e-567">Update the **TimedOut** view content as shown below.</span></span>

    [!code-cshtml[Main](whats-new-in-aspnet-mvc-4/samples/sample29.cshtml)]
10. <span data-ttu-id="6518e-568">Uygulamayı çalıştırmak ve kök URL'ye gidin.</span><span class="sxs-lookup"><span data-stu-id="6518e-568">Run the application and navigate to the root URL.</span></span> <span data-ttu-id="6518e-569">Eklediğiniz gibi bir **net_thread_sleep** 1000 milisaniye olarak tarafından oluşturulan bir zaman aşımı hatası alırsınız **AsyncTimeout** özniteliği ve tarafından catch **HandleError** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="6518e-569">As you have added a **Thread.Sleep** of 1000 milliseconds, you will get a time-out error, generated by the **AsyncTimeout** attribute and catch by the **HandleError** attribute.</span></span>

    <span data-ttu-id="6518e-570">![Zaman aşımı özel durumu işlenmiş](whats-new-in-aspnet-mvc-4/_static/image37.png "işlenen zaman aşımı özel durumu")</span><span class="sxs-lookup"><span data-stu-id="6518e-570">![Time-out exception handled](whats-new-in-aspnet-mvc-4/_static/image37.png "Time-out exception handled")</span></span>

    <span data-ttu-id="6518e-571">*Zaman aşımı özel durumu işlenmiş*</span><span class="sxs-lookup"><span data-stu-id="6518e-571">*Time-out exception handled*</span></span>

> [!NOTE]
> <span data-ttu-id="6518e-572">Ayrıca, bu uygulama için Windows Azure Web siteleri aşağıdaki dağıtabilirsiniz [ek D: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixD).</span><span class="sxs-lookup"><span data-stu-id="6518e-572">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixD).</span></span>


<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="6518e-573">Özet</span><span class="sxs-lookup"><span data-stu-id="6518e-573">Summary</span></span>

<span data-ttu-id="6518e-574">Bu uygulamalı-lab içinde size bazı yeni özellikleri ASP.NET MVC 4'te yerleşik gözlemlenen.</span><span class="sxs-lookup"><span data-stu-id="6518e-574">In this hands-on-lab, you've observed some of the new features resident in ASP.NET MVC 4.</span></span> <span data-ttu-id="6518e-575">Aşağıdaki kavramları açıklanmıştır:</span><span class="sxs-lookup"><span data-stu-id="6518e-575">The following concepts have been discussed:</span></span>

- <span data-ttu-id="6518e-576">ASP.NET MVC proje şablonları-dahil olmak üzere yeni mobil uygulama projesi şablon geliştirmeleri avantajlarından yararlanın</span><span class="sxs-lookup"><span data-stu-id="6518e-576">Take advantage of the enhancements to the ASP.NET MVC project templates-including the new mobile application project template</span></span>
- <span data-ttu-id="6518e-577">Mobil cihazlarda görünen geliştirmek için CSS medya sorgular ve HTML5 Görünüm penceresi özniteliği kullanın</span><span class="sxs-lookup"><span data-stu-id="6518e-577">Use the HTML5 viewport attribute and CSS media queries to improve the display on mobile devices</span></span>
- <span data-ttu-id="6518e-578">JQuery Mobile için aşamalı geliştirmeleri ve dokunma özelliği iyileştirilmiş web kullanıcı Arabirimi oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="6518e-578">Use jQuery Mobile for progressive enhancements and for building touch-optimized web UI</span></span>
- <span data-ttu-id="6518e-579">Mobile özel görünümlerini oluşturma</span><span class="sxs-lookup"><span data-stu-id="6518e-579">Create mobile-specific views</span></span>
- <span data-ttu-id="6518e-580">Uygulamasında mobil ve Masaüstü görünüm arasında geçiş yapmak için Görünüm değiştirici bileşenini kullan</span><span class="sxs-lookup"><span data-stu-id="6518e-580">Use the view-switcher component to toggle between mobile and desktop views in the application</span></span>
- <span data-ttu-id="6518e-581">Görev desteğini kullanarak zaman uyumsuz denetleyicileri oluşturma</span><span class="sxs-lookup"><span data-stu-id="6518e-581">Create asynchronous controllers using task support</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a><span data-ttu-id="6518e-582">Ek A: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="6518e-582">Appendix A: Using Code Snippets</span></span>

<span data-ttu-id="6518e-583">Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır.</span><span class="sxs-lookup"><span data-stu-id="6518e-583">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="6518e-584">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="6518e-584">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="6518e-585">![Kod projenize eklemek için Visual Studio kod parçacıkları](whats-new-in-aspnet-mvc-4/_static/image38.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="6518e-585">![Using Visual Studio code snippets to insert code into your project](whats-new-in-aspnet-mvc-4/_static/image38.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="6518e-586">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="6518e-586">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="6518e-587">***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="6518e-587">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="6518e-588">Kod eklemesini istediğiniz imleci yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="6518e-588">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="6518e-589">(Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="6518e-589">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="6518e-590">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="6518e-590">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="6518e-591">Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="6518e-591">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="6518e-592">İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="6518e-592">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="6518e-593">![Kod parçacığı adını yazmaya başlayın](whats-new-in-aspnet-mvc-4/_static/image39.png "kod parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="6518e-593">![Start typing the snippet name](whats-new-in-aspnet-mvc-4/_static/image39.png "Start typing the snippet name")</span></span>

<span data-ttu-id="6518e-594">*Kod parçacığı adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="6518e-594">*Start typing the snippet name*</span></span>

<span data-ttu-id="6518e-595">![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](whats-new-in-aspnet-mvc-4/_static/image40.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="6518e-595">![Press Tab to select the highlighted snippet](whats-new-in-aspnet-mvc-4/_static/image40.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="6518e-596">*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="6518e-596">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="6518e-597">![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](whats-new-in-aspnet-mvc-4/_static/image41.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")</span><span class="sxs-lookup"><span data-stu-id="6518e-597">![Press Tab again and the snippet will expand](whats-new-in-aspnet-mvc-4/_static/image41.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="6518e-598">*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*</span><span class="sxs-lookup"><span data-stu-id="6518e-598">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="6518e-599">***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="6518e-599">***To add a code snippet using the mouse (C#, Visual Basic and XML)***</span></span>

1. <span data-ttu-id="6518e-600">Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6518e-600">Right-click where you want to insert the code snippet.</span></span>
2. <span data-ttu-id="6518e-601">Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.</span><span class="sxs-lookup"><span data-stu-id="6518e-601">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
3. <span data-ttu-id="6518e-602">Tıklayarak ilgili kod parçacığı listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="6518e-602">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="6518e-603">![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](whats-new-in-aspnet-mvc-4/_static/image42.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")</span><span class="sxs-lookup"><span data-stu-id="6518e-603">![Right-click where you want to insert the code snippet and select Insert Snippet](whats-new-in-aspnet-mvc-4/_static/image42.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="6518e-604">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*</span><span class="sxs-lookup"><span data-stu-id="6518e-604">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="6518e-605">![Tıklayarak ilgili kod parçacığını listesinden çekme](whats-new-in-aspnet-mvc-4/_static/image43.png "tıklayarak ilgili kod parçacığı listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="6518e-605">![Pick the relevant snippet from the list, by clicking on it](whats-new-in-aspnet-mvc-4/_static/image43.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="6518e-606">*Tıklayarak ilgili kod parçacığı listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="6518e-606">*Pick the relevant snippet from the list, by clicking on it*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="6518e-607">Ek B: yükleme Visual Studio 2012 Web için Express</span><span class="sxs-lookup"><span data-stu-id="6518e-607">Appendix B: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="6518e-608">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="6518e-608">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="6518e-609">Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="6518e-609">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="6518e-610">Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="6518e-610">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="6518e-611">Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="6518e-611">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="6518e-612">Tıklayarak **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="6518e-612">Click on **Install Now**.</span></span> <span data-ttu-id="6518e-613">Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-613">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="6518e-614">Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="6518e-614">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="6518e-615">![Visual Studio Express yükleyin](whats-new-in-aspnet-mvc-4/_static/image44.png "Visual Studio Express'i yükle")</span><span class="sxs-lookup"><span data-stu-id="6518e-615">![Install Visual Studio Express](whats-new-in-aspnet-mvc-4/_static/image44.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="6518e-616">*Visual Studio Express yükleyin*</span><span class="sxs-lookup"><span data-stu-id="6518e-616">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="6518e-617">Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="6518e-617">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşullarını kabul etme](whats-new-in-aspnet-mvc-4/_static/image45.png)

    <span data-ttu-id="6518e-619">*Lisans koşullarını kabul etme*</span><span class="sxs-lookup"><span data-stu-id="6518e-619">*Accepting the license terms*</span></span>
5. <span data-ttu-id="6518e-620">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="6518e-620">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](whats-new-in-aspnet-mvc-4/_static/image46.png)

    <span data-ttu-id="6518e-622">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="6518e-622">*Installation progress*</span></span>
6. <span data-ttu-id="6518e-623">Yükleme tamamlandığında, tıklayın **son**.</span><span class="sxs-lookup"><span data-stu-id="6518e-623">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](whats-new-in-aspnet-mvc-4/_static/image47.png)

    <span data-ttu-id="6518e-625">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="6518e-625">*Installation completed*</span></span>
7. <span data-ttu-id="6518e-626">Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="6518e-626">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="6518e-627">Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.</span><span class="sxs-lookup"><span data-stu-id="6518e-627">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Web kutucuğu için VS Express](whats-new-in-aspnet-mvc-4/_static/image48.png)

    <span data-ttu-id="6518e-629">*Web kutucuğu için VS Express*</span><span class="sxs-lookup"><span data-stu-id="6518e-629">*VS Express for Web tile*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Installing_WebMatrix_2_and_iPhone_Simulator"></a>
## <a name="appendix-c-installing-webmatrix-2-and-iphone-simulator"></a><span data-ttu-id="6518e-630">Ek C: yükleme WebMatrix 2 ve iPhone simülatörü</span><span class="sxs-lookup"><span data-stu-id="6518e-630">Appendix C: Installing WebMatrix 2 and iPhone Simulator</span></span>

<span data-ttu-id="6518e-631">Sitenizi iPhone sanal cihaz çalıştırmak için WebMatrix genişletmesi kullanabilirsiniz &quot;mobil iPhone simülatörü elektrik&quot;.</span><span class="sxs-lookup"><span data-stu-id="6518e-631">To run your site in a simulated iPhone device you can use the WebMatrix extension &quot;Electric Mobile Simulator for the iPhone&quot;.</span></span> <span data-ttu-id="6518e-632">Ayrıca, Visual Studio 2012'den simülatörünü çalıştırmak için aynı uzantı yapılandırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-632">Also, you can configure the same extension to run the simulator from Visual Studio 2012.</span></span>

<a id="ApxCTask1"></a>

<a id="Task_1_-_Installing_WebMatrix_2"></a>
#### <a name="task-1---installing-webmatrix-2"></a><span data-ttu-id="6518e-633">Görev 1 - yükleme WebMatrix 2</span><span class="sxs-lookup"><span data-stu-id="6518e-633">Task 1 - Installing WebMatrix 2</span></span>

1. <span data-ttu-id="6518e-634">Git [ [ https://go.microsoft.com/?linkid=9809776 ](https://go.microsoft.com/?linkid=9809776) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="6518e-634">Go to [[https://go.microsoft.com/?linkid=9809776](https://go.microsoft.com/?linkid=9809776)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="6518e-635">Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>WebMatrix 2</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="6518e-635">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>WebMatrix 2</em>&quot;.</span></span>
2. <span data-ttu-id="6518e-636">Tıklayarak **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="6518e-636">Click on **Install Now**.</span></span> <span data-ttu-id="6518e-637">Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-637">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="6518e-638">Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="6518e-638">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="6518e-639">![WebMatrix 2 Yükle](whats-new-in-aspnet-mvc-4/_static/image49.png "WebMatrix 2'yi yükleme")</span><span class="sxs-lookup"><span data-stu-id="6518e-639">![Install WebMatrix 2](whats-new-in-aspnet-mvc-4/_static/image49.png "Install WebMatrix 2")</span></span>

    <span data-ttu-id="6518e-640">*WebMatrix 2 yükle*</span><span class="sxs-lookup"><span data-stu-id="6518e-640">*Install WebMatrix 2*</span></span>
4. <span data-ttu-id="6518e-641">Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="6518e-641">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    <span data-ttu-id="6518e-642">![Lisans koşullarını kabul](whats-new-in-aspnet-mvc-4/_static/image50.png "lisans koşullarını kabul etme")</span><span class="sxs-lookup"><span data-stu-id="6518e-642">![Accepting the license terms](whats-new-in-aspnet-mvc-4/_static/image50.png "Accepting the license terms")</span></span>

    <span data-ttu-id="6518e-643">*Lisans koşullarını kabul etme*</span><span class="sxs-lookup"><span data-stu-id="6518e-643">*Accepting the license terms*</span></span>
5. <span data-ttu-id="6518e-644">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="6518e-644">Wait until the downloading and installation process completes.</span></span>

    <span data-ttu-id="6518e-645">![Yükleme ilerleme durumu](whats-new-in-aspnet-mvc-4/_static/image51.png "yükleme ilerleme durumu")</span><span class="sxs-lookup"><span data-stu-id="6518e-645">![Installation progress](whats-new-in-aspnet-mvc-4/_static/image51.png "Installation progress")</span></span>

    <span data-ttu-id="6518e-646">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="6518e-646">*Installation progress*</span></span>
6. <span data-ttu-id="6518e-647">Yükleme tamamlandığında, tıklayın **son**.</span><span class="sxs-lookup"><span data-stu-id="6518e-647">When the installation completes, click **Finish**.</span></span>

    <span data-ttu-id="6518e-648">![Yükleme tamamlandı](whats-new-in-aspnet-mvc-4/_static/image52.png "yükleme tamamlandı")</span><span class="sxs-lookup"><span data-stu-id="6518e-648">![Installation completed](whats-new-in-aspnet-mvc-4/_static/image52.png "Installation completed")</span></span>

    <span data-ttu-id="6518e-649">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="6518e-649">*Installation completed*</span></span>
7. <span data-ttu-id="6518e-650">Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="6518e-650">Click **Exit** to close Web Platform Installer.</span></span>

<a id="ApxCTask2"></a>

<a id="Task_2_-_Installing_the_iPhone_Simulator_Extension"></a>
#### <a name="task-2---installing-the-iphone-simulator-extension"></a><span data-ttu-id="6518e-651">Görev 2 - iPhone simülatörü uzantısını yükleme</span><span class="sxs-lookup"><span data-stu-id="6518e-651">Task 2 - Installing the iPhone Simulator Extension</span></span>

1. <span data-ttu-id="6518e-652">Çalıştırma **WebMatrix** ve varolan bir Web sitesini açın veya yeni bir tane oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6518e-652">Run **WebMatrix** and open any existing Web site or create a new one.</span></span>
2. <span data-ttu-id="6518e-653">Tıklayın **çalıştırma** düğmesini **giriş** seçin ve Şerit **yeni Ekle**.</span><span class="sxs-lookup"><span data-stu-id="6518e-653">Click the **Run** button from the **Home** ribbon and select **Add new**.</span></span>

    <span data-ttu-id="6518e-654">![Yeni bir WebMatrix genişletmesi ekleme](whats-new-in-aspnet-mvc-4/_static/image53.png "yeni bir WebMatrix genişletmesi ekleme")</span><span class="sxs-lookup"><span data-stu-id="6518e-654">![Adding new WebMatrix extension](whats-new-in-aspnet-mvc-4/_static/image53.png "Adding new WebMatrix extension")</span></span>

    <span data-ttu-id="6518e-655">*Yeni bir WebMatrix genişletmesi ekleme*</span><span class="sxs-lookup"><span data-stu-id="6518e-655">*Adding new WebMatrix extension*</span></span>
3. <span data-ttu-id="6518e-656">Seçin **iPhone simülatörü** tıklatıp **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="6518e-656">Select **iPhone Simulator** and click **Install**.</span></span>

    <span data-ttu-id="6518e-657">![WebMatrix uzantıları gözatma](whats-new-in-aspnet-mvc-4/_static/image54.png "gözatma WebMatrix uzantıları")</span><span class="sxs-lookup"><span data-stu-id="6518e-657">![Browsing WebMatrix extensions](whats-new-in-aspnet-mvc-4/_static/image54.png "Browsing WebMatrix extensions")</span></span>

    <span data-ttu-id="6518e-658">*WebMatrix uzantıları gözatma*</span><span class="sxs-lookup"><span data-stu-id="6518e-658">*Browsing WebMatrix extensions*</span></span>
4. <span data-ttu-id="6518e-659">Paket Ayrıntıları tıklatın **yükleme** uzantısını yükleme işlemine devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="6518e-659">In the package details, click **Install** to continue with the extension installation.</span></span>

    <span data-ttu-id="6518e-660">![iPhone simülatörü uzantısı](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone simülatörü uzantısı")</span><span class="sxs-lookup"><span data-stu-id="6518e-660">![iPhone Simulator extension](whats-new-in-aspnet-mvc-4/_static/image55.png "iPhone Simulator extension")</span></span>

    <span data-ttu-id="6518e-661">*iPhone simülatörü uzantısı*</span><span class="sxs-lookup"><span data-stu-id="6518e-661">*iPhone Simulator extension*</span></span>
5. <span data-ttu-id="6518e-662">Okuyun ve uzantı EULA'yı kabul edin.</span><span class="sxs-lookup"><span data-stu-id="6518e-662">Read and accept the extension EULA.</span></span>

    <span data-ttu-id="6518e-663">![WebMatrix genişletmesi EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix genişletmesi EULA")</span><span class="sxs-lookup"><span data-stu-id="6518e-663">![WebMatrix extension EULA](whats-new-in-aspnet-mvc-4/_static/image56.png "WebMatrix extension EULA")</span></span>

    <span data-ttu-id="6518e-664">*WebMatrix genişletmesi EULA*</span><span class="sxs-lookup"><span data-stu-id="6518e-664">*WebMatrix extension EULA*</span></span>
6. <span data-ttu-id="6518e-665">Artık, Web sitenizi iPhone simülatörü seçeneği kullanarak Webmatrix'ten çalıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="6518e-665">Now, you can run your Web site from WebMatrix using the iPhone Simulator option.</span></span>

    <span data-ttu-id="6518e-666">![İPhone kullanarak çalıştırma](whats-new-in-aspnet-mvc-4/_static/image57.png "iPhone kullanarak çalıştırma")</span><span class="sxs-lookup"><span data-stu-id="6518e-666">![Run using iPhone](whats-new-in-aspnet-mvc-4/_static/image57.png "Run using iPhone")</span></span>

    <span data-ttu-id="6518e-667">*İPhone kullanarak çalıştırma*</span><span class="sxs-lookup"><span data-stu-id="6518e-667">*Run using iPhone*</span></span>

<a id="ApxCTask3"></a>

<a id="Task_3_-_Configuring_Visual_Studio_2012_to_run_iPhone_Simulator"></a>
#### <a name="task-3---configuring-visual-studio-2012-to-run-iphone-simulator"></a><span data-ttu-id="6518e-668">Görev 3 - iPhone simülatörü çalıştırmak için Visual Studio 2012 yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6518e-668">Task 3 - Configuring Visual Studio 2012 to run iPhone Simulator</span></span>

1. <span data-ttu-id="6518e-669">Açık **Visual Studio 2012** ve herhangi bir Web sitesini açın veya yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="6518e-669">Open **Visual Studio 2012** and open any Web site or create a new project.</span></span>
2. <span data-ttu-id="6518e-670">Çalıştırma düğmesi aşağı oka tıklayın ve **birlikte Gözat**.</span><span class="sxs-lookup"><span data-stu-id="6518e-670">Click the down arrow from the Run button and select **Browse with**.</span></span>

    <span data-ttu-id="6518e-671">![Birlikte Gözat](whats-new-in-aspnet-mvc-4/_static/image58.png "birlikte Gözat")</span><span class="sxs-lookup"><span data-stu-id="6518e-671">![Browse with](whats-new-in-aspnet-mvc-4/_static/image58.png "Browse with")</span></span>

    <span data-ttu-id="6518e-672">*Birlikte Gözat*</span><span class="sxs-lookup"><span data-stu-id="6518e-672">*Browse with*</span></span>
3. <span data-ttu-id="6518e-673">İçinde &quot;şununla Gözat&quot; iletişim kutusunda, tıklayın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="6518e-673">In the &quot;Browse With&quot; dialog, click **Add**.</span></span>
4. <span data-ttu-id="6518e-674">İçinde &quot;Program Ekle&quot; iletişim kutusunda aşağıdaki değerleri kullanın:</span><span class="sxs-lookup"><span data-stu-id="6518e-674">In the &quot;Add Program&quot; dialog, use the following values:</span></span>

   - <span data-ttu-id="6518e-675"><strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe * (yolunu uygun şekilde güncelleştirin)</em></span><span class="sxs-lookup"><span data-stu-id="6518e-675"><strong>Program</strong>: C:\Users\*{CurrentUser}<em>\AppData\Local\Microsoft\WebMatrix\Extensions\20\iPhoneSimulator\ElectricMobileSim\ElectricMobileSim.exe *(update the path accordingly)</em></span></span>
   - <span data-ttu-id="6518e-676">**Bağımsız değişkenler**: &quot;1&quot;</span><span class="sxs-lookup"><span data-stu-id="6518e-676">**Arguments**: &quot;1&quot;</span></span>
   - <span data-ttu-id="6518e-677">**Kolay ad**: iPhone simülatörü</span><span class="sxs-lookup"><span data-stu-id="6518e-677">**Friendly name**: iPhone Simulator</span></span>

     <span data-ttu-id="6518e-678">![Ekleme programı](whats-new-in-aspnet-mvc-4/_static/image59.png "ekleme programı")</span><span class="sxs-lookup"><span data-stu-id="6518e-678">![Add program](whats-new-in-aspnet-mvc-4/_static/image59.png "Add program")</span></span>

     <span data-ttu-id="6518e-679">*İle göz atmak için program ekleyin*</span><span class="sxs-lookup"><span data-stu-id="6518e-679">*Add program to browse with*</span></span>
5. <span data-ttu-id="6518e-680">Tıklayın **Tamam** ve iletişim kutularını kapatın.</span><span class="sxs-lookup"><span data-stu-id="6518e-680">Click **OK** and close the dialogs.</span></span>
6. <span data-ttu-id="6518e-681">Artık Web uygulamalarınızı iPhone simülatör'de Visual Studio 2012'den çalıştırabilir.</span><span class="sxs-lookup"><span data-stu-id="6518e-681">Now you are able to run your Web applications in the iPhone simulator from Visual Studio 2012.</span></span>

    <span data-ttu-id="6518e-682">![İPhone simülatörü ile Gözat](whats-new-in-aspnet-mvc-4/_static/image60.png "iPhone simülatörü ile Gözat")</span><span class="sxs-lookup"><span data-stu-id="6518e-682">![Browse with iPhone Simulator](whats-new-in-aspnet-mvc-4/_static/image60.png "Browse with iPhone Simulator")</span></span>

    <span data-ttu-id="6518e-683">*İPhone simülatörü ile Gözat*</span><span class="sxs-lookup"><span data-stu-id="6518e-683">*Browse with iPhone Simulator*</span></span>

<a id="AppendixD"></a>

<a id="Appendix_D_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-d-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="6518e-684">Ek D: bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="6518e-684">Appendix D: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="6518e-685">Bu ekte, Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturun ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlama gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="6518e-685">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxDTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="6518e-686">Görev 1 Windows yeni bir Web sitesi oluşturma - Azure portalı</span><span class="sxs-lookup"><span data-stu-id="6518e-686">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="6518e-687">Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="6518e-687">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6518e-688">Windows Azure'la 10 ASP.NET Web sitesini ücretsiz olarak barındırın ve ardından trafiğiniz büyüdükçe ölçeğinizi artırın.</span><span class="sxs-lookup"><span data-stu-id="6518e-688">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="6518e-689">Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="6518e-689">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="6518e-690">![Windows Azure Portal'da oturum açın](whats-new-in-aspnet-mvc-4/_static/image61.png "Windows Azure Portal'da oturum açın")</span><span class="sxs-lookup"><span data-stu-id="6518e-690">![Log on to Windows Azure portal](whats-new-in-aspnet-mvc-4/_static/image61.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="6518e-691">*Windows Azure yönetim portalında oturum açın*</span><span class="sxs-lookup"><span data-stu-id="6518e-691">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="6518e-692">Tıklayın **yeni** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="6518e-692">Click **New** on the command bar.</span></span>

    <span data-ttu-id="6518e-693">![Yeni bir Web sitesi oluşturma](whats-new-in-aspnet-mvc-4/_static/image62.png "yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="6518e-693">![Creating a new Web Site](whats-new-in-aspnet-mvc-4/_static/image62.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="6518e-694">*Yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="6518e-694">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="6518e-695">Tıklayın **işlem** | **Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="6518e-695">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="6518e-696">Ardından **hızlı Oluştur** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="6518e-696">Then select **Quick Create** option.</span></span> <span data-ttu-id="6518e-697">Yeni web sitesi için kullanılabilen bir URL girin ve tıklatın **Web sitesi oluştur**.</span><span class="sxs-lookup"><span data-stu-id="6518e-697">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6518e-698">Bir Windows Azure Web sitesi kontrol edebildiğiniz ve yönetebildiğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="6518e-698">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="6518e-699">Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portalın dışında dağıtmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="6518e-699">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="6518e-700">Bir veritabanını ayarlamak için adımları içermez.</span><span class="sxs-lookup"><span data-stu-id="6518e-700">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="6518e-701">![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](whats-new-in-aspnet-mvc-4/_static/image63.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="6518e-701">![Creating a new Web Site using Quick Create](whats-new-in-aspnet-mvc-4/_static/image63.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="6518e-702">*Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="6518e-702">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="6518e-703">Yeni kadar bekleyin **Web sitesi** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="6518e-703">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="6518e-704">Web sitesi oluşturulduktan sonra altındaki bağlantıya tıklayın **URL** sütun.</span><span class="sxs-lookup"><span data-stu-id="6518e-704">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="6518e-705">Yeni Web sitesi çalışıp çalışmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="6518e-705">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="6518e-706">![Yeni web sitesi için gözatma](whats-new-in-aspnet-mvc-4/_static/image64.png "yeni web sitesine göz atma")</span><span class="sxs-lookup"><span data-stu-id="6518e-706">![Browsing to the new web site](whats-new-in-aspnet-mvc-4/_static/image64.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="6518e-707">*Yeni web sitesine göz atma*</span><span class="sxs-lookup"><span data-stu-id="6518e-707">*Browsing to the new web site*</span></span>

    <span data-ttu-id="6518e-708">![Web sitesi çalışan](whats-new-in-aspnet-mvc-4/_static/image65.png "çalışan Web sitesi")</span><span class="sxs-lookup"><span data-stu-id="6518e-708">![Web site running](whats-new-in-aspnet-mvc-4/_static/image65.png "Web site running")</span></span>

    <span data-ttu-id="6518e-709">*Çalışan Web sitesi*</span><span class="sxs-lookup"><span data-stu-id="6518e-709">*Web site running*</span></span>
6. <span data-ttu-id="6518e-710">Portala geri dönün ve web sitesi altında adına **adı** yönetim sayfaları görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="6518e-710">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="6518e-711">![Web sitesi Yönetim sayfalarının açma](whats-new-in-aspnet-mvc-4/_static/image66.png "web sitesi Yönetim sayfalarının açma")</span><span class="sxs-lookup"><span data-stu-id="6518e-711">![Opening the web site management pages](whats-new-in-aspnet-mvc-4/_static/image66.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="6518e-712">*Web Sitesi Yönetim sayfalarının açma*</span><span class="sxs-lookup"><span data-stu-id="6518e-712">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="6518e-713">İçinde **Pano** sayfasındaki **Hızlı Bakış** bölümünde **yayımlama profili indir** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="6518e-713">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6518e-714">*Yayımlama profilini* tüm her etkin yayımlama yöntemi için bir Windows Azure Web sitesi için bir web uygulaması yayımlamak için gereken bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="6518e-714">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="6518e-715">Yayımlama profili URL'leri, kullanıcı kimlik bilgileri ve her bir yayımlama yönteminin etkinleştirildiği uç noktalarına karşı kimlik doğrulaması yapmak ve bağlanmak için gereken veritabanı dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="6518e-715">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="6518e-716">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma desteği yayımlama profillerini yapılandırma için bu programların otomatik hale getirmek için Windows Azure Web siteleri'ne yayımlama web uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="6518e-716">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="6518e-717">![Yayımlama profili web sitesi indiriliyor](whats-new-in-aspnet-mvc-4/_static/image67.png "yayımlama profili web sitesi indiriliyor")</span><span class="sxs-lookup"><span data-stu-id="6518e-717">![Downloading the web site publish profile](whats-new-in-aspnet-mvc-4/_static/image67.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="6518e-718">*Yayımlama profili Web sitesi indiriliyor*</span><span class="sxs-lookup"><span data-stu-id="6518e-718">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="6518e-719">Yayımlama profili dosyasını bilinen bir konuma indirin.</span><span class="sxs-lookup"><span data-stu-id="6518e-719">Download the publish profile file to a known location.</span></span> <span data-ttu-id="6518e-720">Daha fazla Bu alıştırmada, bu dosyayı Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlama için nasıl kullanılacağını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="6518e-720">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="6518e-721">![Yayımlama profili dosyasını kaydetme](whats-new-in-aspnet-mvc-4/_static/image68.png "yayımlama profili kaydediliyor")</span><span class="sxs-lookup"><span data-stu-id="6518e-721">![Saving the publish profile file](whats-new-in-aspnet-mvc-4/_static/image68.png "Saving the publish profile")</span></span>

    <span data-ttu-id="6518e-722">*Yayımlama profili dosyasını kaydetme*</span><span class="sxs-lookup"><span data-stu-id="6518e-722">*Saving the publish profile file*</span></span>

<a id="ApxDTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="6518e-723">Görev 2 - veritabanı sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="6518e-723">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="6518e-724">Uygulamanızı kullanan SQL Server'ın yaparsa veritabanlarını bir SQL veritabanı sunucusu oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="6518e-724">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="6518e-725">SQL Server kullanmayan basit bir uygulama dağıtmak istiyorsanız bu görevi atla.</span><span class="sxs-lookup"><span data-stu-id="6518e-725">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="6518e-726">SQL veritabanı sunucusu, uygulama veritabanını depolamak için gerekir.</span><span class="sxs-lookup"><span data-stu-id="6518e-726">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="6518e-727">SQL veritabanı sunucularını, aboneliğinizde Windows Azure Yönetim Portalı'nda görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**.</span><span class="sxs-lookup"><span data-stu-id="6518e-727">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="6518e-728">Oluşturulan server yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğunda düğme.</span><span class="sxs-lookup"><span data-stu-id="6518e-728">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="6518e-729">Not **sunucu adı ve URL, yönetici oturum açma adı ve parola**, sonraki görevleri kullanacağınız.</span><span class="sxs-lookup"><span data-stu-id="6518e-729">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="6518e-730">Daha sonraki bir aşamasında oluşturulacak şekilde veritabanı henüz oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="6518e-730">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="6518e-731">![SQL veritabanı sunucu Panosu](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL veritabanı sunucu Panosu")</span><span class="sxs-lookup"><span data-stu-id="6518e-731">![SQL Database Server Dashboard](whats-new-in-aspnet-mvc-4/_static/image69.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="6518e-732">*SQL veritabanı sunucu Panosu*</span><span class="sxs-lookup"><span data-stu-id="6518e-732">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="6518e-733">İşlemin sonraki görev ihtiyacınız sunucunun listesinde yerel IP adresinizi eklemek, bu nedenle Visual Studio'dan veritabanı bağlantısını test eder **izin verilen IP adresleri**.</span><span class="sxs-lookup"><span data-stu-id="6518e-733">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="6518e-734">Bunu yapmanın tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** ve yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) düğmesi.</span><span class="sxs-lookup"><span data-stu-id="6518e-734">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](whats-new-in-aspnet-mvc-4/_static/image70.png) button.</span></span>

    ![İstemci IP adresi ekleme](whats-new-in-aspnet-mvc-4/_static/image71.png)

    <span data-ttu-id="6518e-736">*İstemci IP adresi ekleme*</span><span class="sxs-lookup"><span data-stu-id="6518e-736">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="6518e-737">Bir kez **istemci IP adresi** için izin verilen IP adreslerini eklenir listesinde, tıklayın **Kaydet** değişiklikleri onaylamak için.</span><span class="sxs-lookup"><span data-stu-id="6518e-737">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Değişiklikleri onaylayın](whats-new-in-aspnet-mvc-4/_static/image72.png)

    <span data-ttu-id="6518e-739">*Değişiklikleri onaylayın*</span><span class="sxs-lookup"><span data-stu-id="6518e-739">*Confirm Changes*</span></span>

<a id="ApxDTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="6518e-740">Görev 3 - bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="6518e-740">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="6518e-741">ASP.NET MVC 4 çözüme geri dönün.</span><span class="sxs-lookup"><span data-stu-id="6518e-741">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="6518e-742">İçinde **Çözüm Gezgini**, web sitesi projesini sağ tıklatın ve seçin **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="6518e-742">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="6518e-743">![Uygulama yayımlama](whats-new-in-aspnet-mvc-4/_static/image73.png "uygulama yayımlama")</span><span class="sxs-lookup"><span data-stu-id="6518e-743">![Publishing the Application](whats-new-in-aspnet-mvc-4/_static/image73.png "Publishing the Application")</span></span>

    <span data-ttu-id="6518e-744">*Web sitesi yayımlama*</span><span class="sxs-lookup"><span data-stu-id="6518e-744">*Publishing the web site*</span></span>
2. <span data-ttu-id="6518e-745">İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="6518e-745">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="6518e-746">![Yayımlama profilini içeri aktarma](whats-new-in-aspnet-mvc-4/_static/image74.png "yayımlama profilini içeri aktarma")</span><span class="sxs-lookup"><span data-stu-id="6518e-746">![Importing the publish profile](whats-new-in-aspnet-mvc-4/_static/image74.png "Importing the publish profile")</span></span>

    <span data-ttu-id="6518e-747">*Yayımlama profilini içeri aktarma*</span><span class="sxs-lookup"><span data-stu-id="6518e-747">*Importing publish profile*</span></span>
3. <span data-ttu-id="6518e-748">Tıklayın **bağlantısını doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="6518e-748">Click **Validate Connection**.</span></span> <span data-ttu-id="6518e-749">Doğrulama tamamlandıktan sonra tıklayın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="6518e-749">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="6518e-750">Bağlantıyı doğrula düğmesi yanında görünür yeşil bir onay işareti gördükten sonra doğrulama tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="6518e-750">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="6518e-751">![Bağlantı doğrulama](whats-new-in-aspnet-mvc-4/_static/image75.png "bağlantısı doğrulanıyor")</span><span class="sxs-lookup"><span data-stu-id="6518e-751">![Validating connection](whats-new-in-aspnet-mvc-4/_static/image75.png "Validating connection")</span></span>

    <span data-ttu-id="6518e-752">*Bağlantı doğrulama*</span><span class="sxs-lookup"><span data-stu-id="6518e-752">*Validating connection*</span></span>
4. <span data-ttu-id="6518e-753">İçinde **ayarları** sayfasındaki **veritabanları** bölümünde, veritabanı bağlantının metin kutusunun yanındaki düğmeye tıklayın (yani **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="6518e-753">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="6518e-754">![Web dağıtımı yapılandırma](whats-new-in-aspnet-mvc-4/_static/image76.png "Web dağıtımı yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="6518e-754">![Web deploy configuration](whats-new-in-aspnet-mvc-4/_static/image76.png "Web deploy configuration")</span></span>

    <span data-ttu-id="6518e-755">*Web dağıtımı yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="6518e-755">*Web deploy configuration*</span></span>
5. <span data-ttu-id="6518e-756">Veritabanı bağlantısı aşağıdaki gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="6518e-756">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="6518e-757">İçinde **sunucu adı** , SQL veritabanı sunucu URL'sini kullanarak yazın *tcp:* önek.</span><span class="sxs-lookup"><span data-stu-id="6518e-757">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="6518e-758">İçinde **kullanıcı adı** , Sunucu Yöneticisi oturum açma adı yazın.</span><span class="sxs-lookup"><span data-stu-id="6518e-758">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="6518e-759">İçinde **parola** Sunucu Yöneticisi oturum açma parolanızı yazın.</span><span class="sxs-lookup"><span data-stu-id="6518e-759">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="6518e-760">Örneğin yeni bir veritabanı adı yazın: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="6518e-760">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="6518e-761">![Hedef bağlantı dizesi yapılandırma](whats-new-in-aspnet-mvc-4/_static/image77.png "hedef bağlantı dizesini yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="6518e-761">![Configuring destination connection string](whats-new-in-aspnet-mvc-4/_static/image77.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="6518e-762">*Hedef bağlantı dizesini yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="6518e-762">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="6518e-763">Sonra **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6518e-763">Then click **OK**.</span></span> <span data-ttu-id="6518e-764">Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.</span><span class="sxs-lookup"><span data-stu-id="6518e-764">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="6518e-765">![Veritabanı oluşturma](whats-new-in-aspnet-mvc-4/_static/image78.png "veritabanı dizesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="6518e-765">![Creating the database](whats-new-in-aspnet-mvc-4/_static/image78.png "Creating the database string")</span></span>

    <span data-ttu-id="6518e-766">*Veritabanı oluşturma*</span><span class="sxs-lookup"><span data-stu-id="6518e-766">*Creating the database*</span></span>
7. <span data-ttu-id="6518e-767">Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini, varsayılan bağlantı metin kutusu içinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="6518e-767">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="6518e-768">Sonra **İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="6518e-768">Then click **Next**.</span></span>

    <span data-ttu-id="6518e-769">![SQL veritabanı'na işaret eden bağlantı dizesi](whats-new-in-aspnet-mvc-4/_static/image79.png "SQL veritabanına işaret eden bağlantı dizesi")</span><span class="sxs-lookup"><span data-stu-id="6518e-769">![Connection string pointing to SQL Database](whats-new-in-aspnet-mvc-4/_static/image79.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="6518e-770">*SQL veritabanı'na işaret eden bağlantı dizesi*</span><span class="sxs-lookup"><span data-stu-id="6518e-770">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="6518e-771">İçinde **Önizleme** sayfasında **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="6518e-771">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="6518e-772">![Web uygulaması yayımlama](whats-new-in-aspnet-mvc-4/_static/image80.png "web uygulaması yayımlama")</span><span class="sxs-lookup"><span data-stu-id="6518e-772">![Publishing the web application](whats-new-in-aspnet-mvc-4/_static/image80.png "Publishing the web application")</span></span>

    <span data-ttu-id="6518e-773">*Web uygulaması yayımlama*</span><span class="sxs-lookup"><span data-stu-id="6518e-773">*Publishing the web application*</span></span>
9. <span data-ttu-id="6518e-774">Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesi açılır.</span><span class="sxs-lookup"><span data-stu-id="6518e-774">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="6518e-775">![Uygulama Windows Azure'da yayımlanan](whats-new-in-aspnet-mvc-4/_static/image81.png "uygulama yayımlanan Windows Azure'a")</span><span class="sxs-lookup"><span data-stu-id="6518e-775">![Application published to Windows Azure](whats-new-in-aspnet-mvc-4/_static/image81.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="6518e-776">*Windows Azure'da yayımlanan uygulama*</span><span class="sxs-lookup"><span data-stu-id="6518e-776">*Application published to Windows Azure*</span></span>
