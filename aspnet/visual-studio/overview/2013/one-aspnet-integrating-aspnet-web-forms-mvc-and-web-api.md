---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: "Uygulamalı Laboratuvar: Tek ASP.NET: ASP.NET Web formları, MVC ve Web API'yi tümleştirme | Microsoft Docs"
author: rick-anderson
description: ASP.NET Web siteleri, uygulamaları ve Hizmetleri MVC, Web API ve diğerleri gibi özelleştirilmiş teknolojilerini kullanarak oluşturmaya yönelik bir çerçevedir. Genişletmeyle ASP.NET h...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: b6ac0dca92ab3d75eb871099882dcea549264354
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754659"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a><span data-ttu-id="e4a98-104">Uygulamalı Laboratuvar: Tek ASP.NET: ASP.NET Web formları, MVC ve Web API'yi tümleştirme</span><span class="sxs-lookup"><span data-stu-id="e4a98-104">Hands On Lab: One ASP.NET: Integrating ASP.NET Web Forms, MVC and Web API</span></span>
====================
<span data-ttu-id="e4a98-105">Tarafından [Team Web Kampları](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="e4a98-105">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="e4a98-106">Eğitim Seti Web Kampları indirin</span><span class="sxs-lookup"><span data-stu-id="e4a98-106">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="e4a98-107">ASP.NET Web siteleri, uygulamaları ve Hizmetleri MVC, Web API ve diğerleri gibi özelleştirilmiş teknolojilerini kullanarak oluşturmaya yönelik bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-107">ASP.NET is a framework for building Web sites, apps and services using specialized technologies such as MVC, Web API and others.</span></span> <span data-ttu-id="e4a98-108">Genişletmeyle oluşturulduktan sonra ASP.NET yakaladı ve ifade edilen teknolojiler tümleşik olması gerekir, doğru çalışma yeni çabalar olmuştur **tek ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-108">With the expansion ASP.NET has seen since its creation and the expressed need to have these technologies integrated, there have been recent efforts in working towards **One ASP.NET**.</span></span>
> 
> <span data-ttu-id="e4a98-109">Visual Studio 2013, bir uygulama oluşturmak ve tüm ASP.NET teknolojileri tek bir projede kullanmanıza olanak sağlayan yeni bir birleşik proje sistemi sunuyor.</span><span class="sxs-lookup"><span data-stu-id="e4a98-109">Visual Studio 2013 introduces a new unified project system which lets you build an application and use all the ASP.NET technologies in one project.</span></span> <span data-ttu-id="e4a98-110">Bu özellik, bir proje ve onunla Sopası başlangıcında bir teknolojisini seçin gereğini ortadan kaldırır ve bunun yerine bir projede birden çok ASP.NET çerçeve kullanımını teşvik eder.</span><span class="sxs-lookup"><span data-stu-id="e4a98-110">This feature eliminates the need to pick one technology at the start of a project and stick with it, and instead encourages the use of multiple ASP.NET frameworks within one project.</span></span>
> 
> <span data-ttu-id="e4a98-111">Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="e4a98-111">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="e4a98-112">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="e4a98-112">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="e4a98-113">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="e4a98-113">Objectives</span></span>

<span data-ttu-id="e4a98-114">Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="e4a98-114">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="e4a98-115">Temel bir Web sitesi oluşturma **tek ASP.NET** proje türü</span><span class="sxs-lookup"><span data-stu-id="e4a98-115">Create a Web site based on the **One ASP.NET** project type</span></span>
- <span data-ttu-id="e4a98-116">Farklı kullanım **ASP.NET** çerçeveleri ister **MVC** ve **Web API** aynı projede</span><span class="sxs-lookup"><span data-stu-id="e4a98-116">Use different **ASP.NET** frameworks like **MVC** and **Web API** in the same project</span></span>
- <span data-ttu-id="e4a98-117">Ana bileşenleri tanımlamak bir **ASP.NET** uygulama</span><span class="sxs-lookup"><span data-stu-id="e4a98-117">Identify the main components of an **ASP.NET** application</span></span>
- <span data-ttu-id="e4a98-118">Yararlanmak **ASP.NET iskeleti oluşturma** denetleyicileri ve görünümleri CRUD işlemleri gerçekleştirmek için otomatik olarak oluşturmak için framework tabanlı model sınıflarınızı</span><span class="sxs-lookup"><span data-stu-id="e4a98-118">Take advantage of the **ASP.NET Scaffolding** framework to automatically create Controllers and Views to perform CRUD operations based on your model classes</span></span>
- <span data-ttu-id="e4a98-119">Her iş için doğru aracı kullanarak makine ve İnsan okunabilir biçimde bilgi aynı kümesini kullanıma sunma</span><span class="sxs-lookup"><span data-stu-id="e4a98-119">Expose the same set of information in machine- and human-readable formats using the right tool for each job</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="e4a98-120">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="e4a98-120">Prerequisites</span></span>

<span data-ttu-id="e4a98-121">Aşağıda bu uygulamalı laboratuvarı tamamlamak için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="e4a98-121">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="e4a98-122">[Visual Studio Express 2013 Web](https://www.microsoft.com/visualstudio/) veya üzeri</span><span class="sxs-lookup"><span data-stu-id="e4a98-122">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="e4a98-123">Visual Studio 2013 Güncelleştirme 1</span><span class="sxs-lookup"><span data-stu-id="e4a98-123">Visual Studio 2013 Update 1</span></span>](https://go.microsoft.com/fwlink/?LinkId=301714)

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="e4a98-124">Kurulum</span><span class="sxs-lookup"><span data-stu-id="e4a98-124">Setup</span></span>

<span data-ttu-id="e4a98-125">Bu uygulamalı laboratuvarda alıştırmalar çalıştırmak için önce ortamı oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-125">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="e4a98-126">Açık Windows Gezgini ve Laboratuvar göz atın **kaynak** klasör.</span><span class="sxs-lookup"><span data-stu-id="e4a98-126">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="e4a98-127">Sağ **Setup.cmd** seçip **yönetici olarak çalıştır** ortamınızı yapılandırın ve bu Laboratuvar için Visual Studio kod parçacıkları yükleme kurulum işlemini başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="e4a98-127">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="e4a98-128">Kullanıcı Hesabı Denetimi iletişim kutusunu gösteriliyorsa, devam etmek için eylemi onaylayın.</span><span class="sxs-lookup"><span data-stu-id="e4a98-128">If the User Account Control dialog box is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="e4a98-129">Kurulumu çalıştırmadan önce bu Laboratuvar için tüm bağımlılıkların etkinleştirdiğinizden emin olun.</span><span class="sxs-lookup"><span data-stu-id="e4a98-129">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="e4a98-130">Kod parçacıklarını kullanma</span><span class="sxs-lookup"><span data-stu-id="e4a98-130">Using the Code Snippets</span></span>

<span data-ttu-id="e4a98-131">Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e4a98-131">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="e4a98-132">Kolaylık olması için bu kodu çoğunu, Visual Studio el ile eklemek zorunda kalmamak için 2013 içinde erişebileceğiniz Visual Studio kod parçacıkları, olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="e4a98-132">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="e4a98-133">Her alıştırma bulunan bir başlangıç çözüm eşlik **başlamak** her alıştırma diğerlerinden takip etmenize olanak tanıyan çalışma klasörü.</span><span class="sxs-lookup"><span data-stu-id="e4a98-133">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="e4a98-134">Lütfen bir alıştırma sırasında eklenen kod parçacıkları bu çözümleri başlangıç eksik ve alıştırma tamamlayıncaya kadar çalışmayabilir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e4a98-134">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="e4a98-135">Ayrıca bulabilirsiniz bir alıştırma için kaynak kod içinde bir **son** karşılık gelen bir alıştırma olarak adımları tamamlamanızı sonuçları kodunu içeren bir Visual Studio çözüm içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="e4a98-135">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="e4a98-136">Bu uygulamalı laboratuvarı çalışırken ek yardıma ihtiyacınız varsa, bu çözümleri kılavuz kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e4a98-136">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="e4a98-137">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="e4a98-137">Exercises</span></span>

<span data-ttu-id="e4a98-138">Bu uygulamalı laboratuvarı aşağıdaki alıştırmaları içerir:</span><span class="sxs-lookup"><span data-stu-id="e4a98-138">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="e4a98-139">Yeni bir Web formları projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e4a98-139">Creating a New Web Forms Project</span></span>](#Exercise1)
2. [<span data-ttu-id="e4a98-140">İskele kurma kullanarak MVC denetleyicisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e4a98-140">Creating an MVC Controller Using Scaffolding</span></span>](#Exercise2)
3. [<span data-ttu-id="e4a98-141">Yapı İskelesi kullanarak bir Web API denetleyicisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e4a98-141">Creating a Web API Controller Using Scaffolding</span></span>](#Exercise3)

<span data-ttu-id="e4a98-142">Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**</span><span class="sxs-lookup"><span data-stu-id="e4a98-142">Estimated time to complete this lab: **60 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="e4a98-143">Visual Studio'yu ilk başlattığınızda, önceden tanımlı ayar koleksiyonlarından birini seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-143">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="e4a98-144">Her önceden tanımlı bir koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyici davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler.</span><span class="sxs-lookup"><span data-stu-id="e4a98-144">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="e4a98-145">Bu Laboratuvar yordamları kullanarak Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklayan **genel geliştirme ayarları** koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="e4a98-145">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="e4a98-146">Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız adımlar farklılıklar olabilir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-146">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a><span data-ttu-id="e4a98-147">Alıştırma 1: yeni bir Web formları projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e4a98-147">Exercise 1: Creating a New Web Forms Project</span></span>

<span data-ttu-id="e4a98-148">Bu alıştırmada, Visual Studio 2013 kullanarak yeni Web Forms sitesi oluşturacaksınız **tek ASP.NET** birleşik Proje deneyimi, Web Forms, MVC ve Web API'si bileşenleri aynı uygulamada kolayca tümleştirmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="e4a98-148">In this exercise you will create a new Web Forms site in Visual Studio 2013 using the **One ASP.NET** unified project experience, which will allow you to easily integrate Web Forms, MVC and Web API components in the same application.</span></span> <span data-ttu-id="e4a98-149">Daha sonra oluşturulan çözümü keşfedin ve onun parçalarını tanımlamak ve son olarak eylem Web sitesinde görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e4a98-149">You will then explore the generated solution and identify its parts, and finally you will see the Web site in action.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a><span data-ttu-id="e4a98-150">Görev 1: tek ASP.NET deneyimi kullanarak yeni bir Site oluşturma</span><span class="sxs-lookup"><span data-stu-id="e4a98-150">Task 1 – Creating a New Site Using the One ASP.NET Experience</span></span>

<span data-ttu-id="e4a98-151">Bu görevde, başlayacak yeni bir Web sitesi Visual Studio'da oluşturma dayalı **tek ASP.NET** proje türü.</span><span class="sxs-lookup"><span data-stu-id="e4a98-151">In this task you will start creating a new Web site in Visual Studio based on the **One ASP.NET** project type.</span></span> <span data-ttu-id="e4a98-152">**Bir ASP.NET** tüm ASP.NET teknolojileri birleştirir ve karıştırın ve bunları istediğiniz gibi eşleşen seçeneği sunar.</span><span class="sxs-lookup"><span data-stu-id="e4a98-152">**One ASP.NET** unifies all ASP.NET technologies and gives you the option to mix and match them as desired.</span></span> <span data-ttu-id="e4a98-153">Canlı Web formları, MVC ve Web API farklı bileşenleri ardından yan yana ve uygulamanızdaki algılar.</span><span class="sxs-lookup"><span data-stu-id="e4a98-153">You will then recognize the different components of Web Forms, MVC and Web API that live side by side within your application.</span></span>

1. <span data-ttu-id="e4a98-154">Açık **Visual Studio Express 2013 Web** seçip **dosya | Yeni proje...**  yeni bir çözüm başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="e4a98-154">Open **Visual Studio Express 2013 for Web** and select **File | New Project...** to start a new solution.</span></span>

    ![Yeni proje oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    <span data-ttu-id="e4a98-156">*Yeni proje oluşturma*</span><span class="sxs-lookup"><span data-stu-id="e4a98-156">*Creating a New Project*</span></span>
2. <span data-ttu-id="e4a98-157">İçinde **yeni proje** iletişim kutusunda **ASP.NET Web uygulaması** altında **Visual C# | Web** sekmesini tıklatıp emin **.NET Framework 4.5** seçilir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-157">In the **New Project** dialog box, select **ASP.NET Web Application** under the **Visual C# | Web** tab, and make sure **.NET Framework 4.5** is selected.</span></span> <span data-ttu-id="e4a98-158">Projeyi adlandırın *MyHybridSite*, seçin bir **konumu** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-158">Name the project *MyHybridSite*, choose a **Location** and click **OK**.</span></span>

    ![Yeni ASP.NET Web uygulaması projesi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    <span data-ttu-id="e4a98-160">*Yeni bir ASP.NET Web uygulaması projesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="e4a98-160">*Creating a new ASP.NET Web Application project*</span></span>
3. <span data-ttu-id="e4a98-161">İçinde **yeni ASP.NET projesi** iletişim kutusunda **Web Forms** şablonu seçip alt **MVC** ve **Web API** seçenekleri.</span><span class="sxs-lookup"><span data-stu-id="e4a98-161">In the **New ASP.NET Project** dialog box, select the **Web Forms** template and select the **MVC** and **Web API** options.</span></span> <span data-ttu-id="e4a98-162">Ayrıca, emin **kimlik doğrulaması** seçeneği **bireysel kullanıcı hesapları**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-162">Also, make sure that the **Authentication** option is set to **Individual User Accounts**.</span></span> <span data-ttu-id="e4a98-163">Devam etmek için **Tamam** 'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e4a98-163">Click **OK** to continue.</span></span>

    ![Web API ve MVC bileşenler dahil olmak üzere Web Forms şablonuyla yeni proje oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    <span data-ttu-id="e4a98-165">*Web API ve MVC bileşenler dahil olmak üzere Web Forms şablonuyla yeni proje oluşturma*</span><span class="sxs-lookup"><span data-stu-id="e4a98-165">*Creating a new project with the Web Forms template, including Web API and MVC components*</span></span>
4. <span data-ttu-id="e4a98-166">Şimdi oluşturulan çözüm yapısını keşfedebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e4a98-166">You can now explore the structure of the generated solution.</span></span>

    ![Oluşturulan çözüm keşfetme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    <span data-ttu-id="e4a98-168">*Oluşturulan çözüm keşfetme*</span><span class="sxs-lookup"><span data-stu-id="e4a98-168">*Exploring the generated solution*</span></span>

    1. <span data-ttu-id="e4a98-169">**Hesap:** bu klasörü kaydetmek için oturum açın ve uygulamanın kullanıcı hesaplarını yönetmek için Web formu sayfalarını içerir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-169">**Account:** This folder contains the Web Form pages to register, log in to and manage the application's user accounts.</span></span> <span data-ttu-id="e4a98-170">Bu klasöre eklenen **bireysel kullanıcı hesapları** kimlik doğrulaması seçeneği Web Forms proje şablonu yapılandırması sırasında belirlenir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-170">This folder is added when the **Individual User Accounts** authentication option is selected during the configuration of the Web Forms project template.</span></span>
    2. <span data-ttu-id="e4a98-171">**Modelleri:** bu klasör, uygulama verilerinizi temsil eden sınıfları içerir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-171">**Models:** This folder will contain the classes that represent your application data.</span></span>
    3. <span data-ttu-id="e4a98-172">**Denetleyicileri** ve **görünümleri**: Bu klasör için gerekli olan **ASP.NET MVC** ve **ASP.NET Web API** bileşenleri.</span><span class="sxs-lookup"><span data-stu-id="e4a98-172">**Controllers** and **Views**: These folders are required for the **ASP.NET MVC** and **ASP.NET Web API** components.</span></span> <span data-ttu-id="e4a98-173">Sonraki alıştırmalarda MVC ve Web API teknolojileri inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e4a98-173">You will explore the MVC and Web API technologies in the next exercises.</span></span>
    4. <span data-ttu-id="e4a98-174">**Default.aspx**, **Contact.aspx** ve **About.aspx** dosyalar için belirli sayfaları oluşturmak için başlangıç noktası olarak kullanabileceğiniz önceden tanımlanmış Web formu sayfaları, uygulama.</span><span class="sxs-lookup"><span data-stu-id="e4a98-174">The **Default.aspx**, **Contact.aspx** and **About.aspx** files are pre-defined Web Form pages that you can use as starting points to build the pages specific to your application.</span></span> <span data-ttu-id="e4a98-175">Bu dosyaların programlama mantığı olarak adlandırılan ayrı bir dosyada bulunan &quot;arka plan kod&quot; olan dosya, bir &quot;. aspx.vb&quot; veya &quot;. aspx.cs&quot; uzantısı (bağlı olarak dili) kullanılır.</span><span class="sxs-lookup"><span data-stu-id="e4a98-175">The programming logic of those files resides in a separate file referred to as the &quot;code-behind&quot; file, which has an &quot;.aspx.vb&quot; or &quot;.aspx.cs&quot; extension (depending on the language used).</span></span> <span data-ttu-id="e4a98-176">Arka plan kod mantıksal sunucu üzerinde çalışır ve dinamik olarak sayfanız için bir HTML çıktı üretir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-176">The code-behind logic runs on the server and dynamically produces the HTML output for your page.</span></span>
    5. <span data-ttu-id="e4a98-177">**Site.Master** ve **Site.Mobile.Master** sayfaları, uygulamanın görünümünü ve tüm sayfaları standart davranışını tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="e4a98-177">The **Site.Master** and **Site.Mobile.Master** pages define the look and feel and the standard behavior of all the pages in the application.</span></span>
5. <span data-ttu-id="e4a98-178">Çift **Default.aspx** sayfasının içeriği keşfetmek için dosya.</span><span class="sxs-lookup"><span data-stu-id="e4a98-178">Double-click the **Default.aspx** file to explore the content of the page.</span></span>

    ![Default.aspx sayfasında keşfetme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    <span data-ttu-id="e4a98-180">*Default.aspx sayfasında keşfetme*</span><span class="sxs-lookup"><span data-stu-id="e4a98-180">*Exploring the Default.aspx page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4a98-181">**Sayfa** yönergesi dosyanın üst Web Forms sayfası özniteliklerini tanımlar.</span><span class="sxs-lookup"><span data-stu-id="e4a98-181">The **Page** directive at the top of the file defines the attributes of the Web Forms page.</span></span> <span data-ttu-id="e4a98-182">Örneğin, **MasterPageFile** öznitelik asıl yolunu belirtir - bu durumda, sayfa *Site.Master* sayfası - ve **Inherits** öznitelik tanımlar. arka plan kod sınıfı sayfanın devralır.</span><span class="sxs-lookup"><span data-stu-id="e4a98-182">For example, the **MasterPageFile** attribute specifies the path to the master page -in this case, the *Site.Master* page- and the **Inherits** attribute defines the code-behind class for the page to inherit.</span></span> <span data-ttu-id="e4a98-183">Bu sınıf tarafından belirlenen dosya bulunan **CodeBehind** özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e4a98-183">This class is located in the file determined by the **CodeBehind** attribute.</span></span>
    > 
    > <span data-ttu-id="e4a98-184">**Asp: Content** denetim (metin, biçimlendirme ve denetimleri) sayfanın gerçek içeriği bulunduran ve eşlenmiş bir **asp: ContentPlaceHolder** ana sayfadaki denetim.</span><span class="sxs-lookup"><span data-stu-id="e4a98-184">The **asp:Content** control holds the actual content of the page (text, markup and controls) and is mapped to a **asp:ContentPlaceHolder** control on the master page.</span></span> <span data-ttu-id="e4a98-185">Bu durumda, sayfa içeriği içinde işlenir *MainContent* tanımlı denetim *Site.Master* sayfası.</span><span class="sxs-lookup"><span data-stu-id="e4a98-185">In this case, the page content will be rendered inside the *MainContent* control defined in the *Site.Master* page.</span></span>
6. <span data-ttu-id="e4a98-186">Genişletin **uygulama\_Başlat** klasörü ve bildirim **WebApiConfig.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="e4a98-186">Expand the **App\_Start** folder and notice the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="e4a98-187">Web API'si ile tek ASP.NET şablon projenizi yapılandırırken içerdiğinden visual Studio bu dosyayı oluşturulan çözümde yer alan.</span><span class="sxs-lookup"><span data-stu-id="e4a98-187">Visual Studio included that file in the generated solution because you included Web API when configuring your project with the One ASP.NET template.</span></span>
7. <span data-ttu-id="e4a98-188">Açık **WebApiConfig.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="e4a98-188">Open the **WebApiConfig.cs** file.</span></span> <span data-ttu-id="e4a98-189">İçinde *WebApiConfig* HTTP eşleştiren, Web API, ile ilişkili yapılandırma bulacaksınız sınıf yolları için **Web APİ'si denetleyicilerinin**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-189">In the *WebApiConfig* class you will find the configuration associated with Web API, which maps HTTP routes to **Web API controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. <span data-ttu-id="e4a98-190">Açık **RouteConfig.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="e4a98-190">Open the **RouteConfig.cs** file.</span></span> <span data-ttu-id="e4a98-191">İçinde *RegisterRoutes* yöntemi için bir HTTP rotasıyla eşleştiren MVC ile ilişkili yapılandırmasını bulacaksınız **MVC denetleyicileri**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-191">Inside the *RegisterRoutes* method you will find the configuration associated with MVC, which maps HTTP routes to **MVC controllers**.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="e4a98-192">Görev 2 – çözümü çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e4a98-192">Task 2 – Running the Solution</span></span>

<span data-ttu-id="e4a98-193">Bu görevde oluşturulan çözümü çalıştırın, uygulama ve URL yeniden yazma ve yerleşik kimlik doğrulama gibi özelliklerinden bazılarını keşfedin.</span><span class="sxs-lookup"><span data-stu-id="e4a98-193">In this task you will run the generated solution, explore the app and some of its features, like URL rewriting and built-in authentication.</span></span>

1. <span data-ttu-id="e4a98-194">Çözümü çalıştırmak için basın **F5** veya **Başlat** düğme araç çubuğunda yer alan.</span><span class="sxs-lookup"><span data-stu-id="e4a98-194">To run the solution, press **F5** or click the **Start** button located on the toolbar.</span></span> <span data-ttu-id="e4a98-195">Tarayıcıda uygulama giriş sayfası açmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-195">The application home page should open in the browser.</span></span>

    ![Çözüm çalıştırma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. <span data-ttu-id="e4a98-197">Web formları sayfaları çağrılan doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="e4a98-197">Verify that the Web Forms pages are being invoked.</span></span> <span data-ttu-id="e4a98-198">Bunu yapmak için URL'ye **/contact.aspx** tuşuna basın ve adres çubuğuna URL'ye **Enter**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-198">To do this, append **/contact.aspx** to the URL in the address bar and press **Enter**.</span></span>

    ![Kolay URL'ler](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    <span data-ttu-id="e4a98-200">*Kolay URL'ler*</span><span class="sxs-lookup"><span data-stu-id="e4a98-200">*Friendly URLs*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4a98-201">Gördüğünüz gibi URL değişir **/başvurun**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-201">As you can see, the URL changes to **/contact**.</span></span> <span data-ttu-id="e4a98-202">Başlangıç **ASP.NET 4**, URL yönlendirme özellikleri, Web Forms eklendi, URL'ler, yazabilmesi ister *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* yerine  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span><span class="sxs-lookup"><span data-stu-id="e4a98-202">Starting from **ASP.NET 4**, URL routing capabilities were added to Web Forms, so you can write URLs like *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* instead of *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*.</span></span> <span data-ttu-id="e4a98-203">Daha fazla bilgi için [URL yönlendirme](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span><span class="sxs-lookup"><span data-stu-id="e4a98-203">For more information refer to [URL Routing](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).</span></span>
3. <span data-ttu-id="e4a98-204">Şimdi uygulamaya tümleşik kimlik doğrulaması akışı inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e4a98-204">You will now explore the authentication flow integrated into the application.</span></span> <span data-ttu-id="e4a98-205">Bunu yapmak için tıklatın **kaydetme** sayfanın sağ üst köşesindeki içinde.</span><span class="sxs-lookup"><span data-stu-id="e4a98-205">To do this, click **Register** in the upper-right corner of the page.</span></span>

    ![Yeni bir kullanıcı kaydetme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    <span data-ttu-id="e4a98-207">*Yeni bir kullanıcı kaydetme*</span><span class="sxs-lookup"><span data-stu-id="e4a98-207">*Registering a new user*</span></span>
4. <span data-ttu-id="e4a98-208">İçinde **kaydetme** want bir **kullanıcı adı** ve **parola**ve ardından **kaydetme**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-208">In the **Register** page, enter a **User name** and **Password**, and then click **Register**.</span></span>

    ![Kayıt sayfası](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    <span data-ttu-id="e4a98-210">*Kayıt sayfası*</span><span class="sxs-lookup"><span data-stu-id="e4a98-210">*Register page*</span></span>
5. <span data-ttu-id="e4a98-211">Uygulama, yeni hesabı kaydeder ve kullanıcının kimliği doğrulanır.</span><span class="sxs-lookup"><span data-stu-id="e4a98-211">The application registers the new account, and the user is authenticated.</span></span>

    ![Kimliği doğrulanmış kullanıcı](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    <span data-ttu-id="e4a98-213">*Kimliği doğrulanmış kullanıcı*</span><span class="sxs-lookup"><span data-stu-id="e4a98-213">*User authenticated*</span></span>
6. <span data-ttu-id="e4a98-214">Geri Git Visual Studio ve ENTER tuşuna **SHIFT + F5 tuşlarına basarak** hata ayıklamayı durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="e4a98-214">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a><span data-ttu-id="e4a98-215">Alıştırma 2: İskele kurma kullanarak MVC denetleyicisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e4a98-215">Exercise 2: Creating an MVC Controller Using Scaffolding</span></span>

<span data-ttu-id="e4a98-216">Bu alıştırmada, Eylemler ve Razor görünümleri, tek satırlık bir kod yazmadan CRUD işlemleri gerçekleştirmek için bir ASP.NET MVC 5 denetleyici oluşturmak için Visual Studio tarafından sağlanan ASP.NET iskeleti oluşturma çerçevesi yararlanır.</span><span class="sxs-lookup"><span data-stu-id="e4a98-216">In this exercise you will take advantage of the ASP.NET Scaffolding framework provided by Visual Studio to create an ASP.NET MVC 5 controller with actions and Razor views to perform CRUD operations, without writing a single line of code.</span></span> <span data-ttu-id="e4a98-217">Yapı iskelesi süreci Entity Framework Code First SQL veritabanı'nda veri bağlamı ve veritabanı şeması oluşturmak için kullanır.</span><span class="sxs-lookup"><span data-stu-id="e4a98-217">The scaffolding process will use Entity Framework Code First to generate the data context and the database schema in the SQL database.</span></span>

<span data-ttu-id="e4a98-218">**Entity Framework'ü ilk kod**</span><span class="sxs-lookup"><span data-stu-id="e4a98-218">**About Entity Framework Code First**</span></span>

<span data-ttu-id="e4a98-219">Entity Framework (EF) programlama ilişkisel depolama şeması kullanarak doğrudan programlama yerine kavramsal bir uygulama modeli tarafından veri erişimi uygulamaları oluşturmanızı sağlayan bir nesne ilişkisel eşleyicidir (ORM) olur.</span><span class="sxs-lookup"><span data-stu-id="e4a98-219">Entity Framework (EF) is an object-relational mapper (ORM) that enables you to create data access applications by programming with a conceptual application model instead of programming directly using a relational storage schema.</span></span>

<span data-ttu-id="e4a98-220">Entity Framework Code First modelleme iş akışı sorgulama, gerçekleştirirken EF dayalı modeli temsil etmek için kendi etki alanı sınıflarını kullanmak değişiklik izleme ve güncelleştirme işlevleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="e4a98-220">The Entity Framework Code First modeling workflow allows you to use your own domain classes to represent the model that EF relies on when performing querying, change-tracking and updating functions.</span></span> <span data-ttu-id="e4a98-221">Code First geliştirme iş akışı kullanarak, bir veritabanı oluşturmak ya da bir şema belirleme uygulamanızı başlatmak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e4a98-221">Using the Code First development workflow, you do not need to begin your application by creating a database or specifying a schema.</span></span> <span data-ttu-id="e4a98-222">Bunun yerine, uygulamanız için en uygun etki alanı model nesneleri tanımlayan standart .NET sınıfları yazabilirsiniz ve Entity Framework veritabanı sizin için oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e4a98-222">Instead, you can write standard .NET classes that define the most appropriate domain model objects for your application, and Entity Framework will create the database for you.</span></span>

> [!NOTE]
> <span data-ttu-id="e4a98-223">Entity Framework hakkında daha fazla bilgi [burada](../../../entity-framework.md).</span><span class="sxs-lookup"><span data-stu-id="e4a98-223">You can learn more about Entity Framework [here](../../../entity-framework.md).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a><span data-ttu-id="e4a98-224">Görev 1 – yeni bir Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="e4a98-224">Task 1 – Creating a New Model</span></span>

<span data-ttu-id="e4a98-225">Artık tanımlayacak bir **kişi** sınıfı MVC denetleyicisi ve görünümleri oluşturmak için iskele kurma işlemi tarafından kullanılan model olacaktır.</span><span class="sxs-lookup"><span data-stu-id="e4a98-225">You will now define a **Person** class, which will be the model used by the scaffolding process to create the MVC controller and the views.</span></span> <span data-ttu-id="e4a98-226">Oluşturarak başlayacaksınız bir **kişi** model sınıfı ve denetleyici de CRUD işlemlerini otomatik olarak oluşturulacak yapı iskelesi özelliklerini kullanma.</span><span class="sxs-lookup"><span data-stu-id="e4a98-226">You will start by creating a **Person** model class, and the CRUD operations in the controller will be automatically created using scaffolding features.</span></span>

1. <span data-ttu-id="e4a98-227">Açık **Visual Studio Express 2013 Web** ve **MyHybridSite.sln** çözüm bulunan **kaynak/Ex2-MvcScaffolding/başlangıcı** klasör.</span><span class="sxs-lookup"><span data-stu-id="e4a98-227">Open **Visual Studio Express 2013 for Web** and the **MyHybridSite.sln** solution located in the **Source/Ex2-MvcScaffolding/Begin** folder.</span></span> <span data-ttu-id="e4a98-228">Alternatif olarak, önceki alıştırmada aldığınız çözümüyle devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e4a98-228">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>
2. <span data-ttu-id="e4a98-229">İçinde **Çözüm Gezgini**, sağ **modelleri** klasörü **MyHybridSite** seçin ve proje **Ekle | Sınıfı...** .</span><span class="sxs-lookup"><span data-stu-id="e4a98-229">In **Solution Explorer**, right-click the **Models** folder of the **MyHybridSite** project and select **Add | Class...**.</span></span>

    ![Kişi model sınıfı ekleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    <span data-ttu-id="e4a98-231">*Kişi model sınıfı ekleme*</span><span class="sxs-lookup"><span data-stu-id="e4a98-231">*Adding the Person model class*</span></span>
3. <span data-ttu-id="e4a98-232">İçinde **Yeni Öğe Ekle** iletişim kutusunda, dosya adı *Person.cs* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-232">In the **Add New Item** dialog box, name the file *Person.cs* and click **Add**.</span></span>

    ![Kişi model sınıfı oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    <span data-ttu-id="e4a98-234">*Kişi model sınıfı oluşturma*</span><span class="sxs-lookup"><span data-stu-id="e4a98-234">*Creating the Person model class*</span></span>
4. <span data-ttu-id="e4a98-235">İçeriğinin yerine geçecek **Person.cs** dosyasındaki kodu aşağıdaki kodla.</span><span class="sxs-lookup"><span data-stu-id="e4a98-235">Replace the content of the **Person.cs** file with the following code.</span></span> <span data-ttu-id="e4a98-236">Tuşuna **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="e4a98-236">Press **CTRL + S** to save the changes.</span></span>

    <span data-ttu-id="e4a98-237">(Kod parçacığını - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span><span class="sxs-lookup"><span data-stu-id="e4a98-237">(Code Snippet - *BringingTogetherOneAspNet - Ex2 - PersonClass*)</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. <span data-ttu-id="e4a98-238">İçinde **Çözüm Gezgini**, sağ **MyHybridSite** seçin ve proje **derleme**, veya basın **CTRL + SHIFT + B** Projeyi derlemek için.</span><span class="sxs-lookup"><span data-stu-id="e4a98-238">In **Solution Explorer**, right-click the **MyHybridSite** project and select **Build**, or press **CTRL + SHIFT + B** to build the project.</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a><span data-ttu-id="e4a98-239">Görev 2-bir MVC denetleyicisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e4a98-239">Task 2 – Creating an MVC Controller</span></span>

<span data-ttu-id="e4a98-240">Şimdi **kişi** model oluşturulur, CRUD denetleyici eylemleri ve görünümleri oluşturmak için Entity Framework ile ASP.NET MVC yapı iskelesi kullanacağı **kişi**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-240">Now that the **Person** model is created, you will use ASP.NET MVC scaffolding with Entity Framework to create the CRUD controller actions and views for **Person**.</span></span>

1. <span data-ttu-id="e4a98-241">İçinde **Çözüm Gezgini**, sağ **denetleyicileri** klasörü **MyHybridSite** seçin ve proje **Ekle | Yeni İskeleli öğe...** .</span><span class="sxs-lookup"><span data-stu-id="e4a98-241">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Yeni iskele kurulmuş denetleyici oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    <span data-ttu-id="e4a98-243">*Yeni iskele kurulmuş denetleyici oluşturma*</span><span class="sxs-lookup"><span data-stu-id="e4a98-243">*Creating a new Scaffolded Controller*</span></span>
2. <span data-ttu-id="e4a98-244">İçinde **İskele Ekle** iletişim kutusunda **MVC 5 denetleyici Entity Framework kullanarak görünümler ile** ve ardından **Ekle.**</span><span class="sxs-lookup"><span data-stu-id="e4a98-244">In the **Add Scaffold** dialog box, select **MVC 5 Controller with views, using Entity Framework** and then click **Add.**</span></span>

    ![Entity Framework ve görünümler ile MVC 5 denetleyici seçme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    <span data-ttu-id="e4a98-246">*Entity Framework ve görünümler ile MVC 5 denetleyici seçme*</span><span class="sxs-lookup"><span data-stu-id="e4a98-246">*Selecting MVC 5 Controller with views and Entity Framework*</span></span>
3. <span data-ttu-id="e4a98-247">Ayarlama *MvcPersonController* olarak **Denetleyici adı**seçin **zaman uyumsuz denetleyici eylemlerini kullanmak** seçeneğini işaretleyip **kişi (MyHybridSite.Models)**  olarak **Model sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-247">Set *MvcPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** as the **Model class**.</span></span>

    ![Yapı iskelesi ile MVC denetleyicisi ekleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    <span data-ttu-id="e4a98-249">*Yapı iskelesi ile MVC denetleyicisi ekleme*</span><span class="sxs-lookup"><span data-stu-id="e4a98-249">*Adding an MVC controller with scaffolding*</span></span>
4. <span data-ttu-id="e4a98-250">Altında **veri bağlamı sınıfının**, tıklayın **yeni veri bağlamı...** .</span><span class="sxs-lookup"><span data-stu-id="e4a98-250">Under **Data context class**, click **New data context...**.</span></span>

    ![Yeni bir veri bağlamı oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    <span data-ttu-id="e4a98-252">*Yeni bir veri bağlamı oluşturma*</span><span class="sxs-lookup"><span data-stu-id="e4a98-252">*Creating a new data context*</span></span>
5. <span data-ttu-id="e4a98-253">İçinde **yeni veri bağlamı** iletişim kutusunda, yeni veri bağlamı adı *PersonContext* tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-253">In the **New Data Context** dialog box, name the new data context *PersonContext* and click **Add**.</span></span>

    ![Yeni PersonContext oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    <span data-ttu-id="e4a98-255">*Yeni PersonContext tür oluşturma*</span><span class="sxs-lookup"><span data-stu-id="e4a98-255">*Creating the new PersonContext type*</span></span>
6. <span data-ttu-id="e4a98-256">Tıklayın **Ekle** için yeni denetleyicisi oluşturmak için **kişi** yapı iskelesi ile.</span><span class="sxs-lookup"><span data-stu-id="e4a98-256">Click **Add** to create the new controller for **Person** with scaffolding.</span></span> <span data-ttu-id="e4a98-257">Visual Studio ardından denetleyici eylemleri, kişi veri bağlamı ve Razor görünümleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e4a98-257">Visual Studio will then generate the controller actions, the Person data context and the Razor views.</span></span>

    ![Yapı iskelesi ile MVC denetleyicisi oluşturduktan sonra](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    <span data-ttu-id="e4a98-259">*Yapı iskelesi ile MVC denetleyicisi oluşturduktan sonra*</span><span class="sxs-lookup"><span data-stu-id="e4a98-259">*After creating the MVC controller with scaffolding*</span></span>
7. <span data-ttu-id="e4a98-260">Açık **MvcPersonController.cs** dosyası **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="e4a98-260">Open the **MvcPersonController.cs** file in the **Controllers** folder.</span></span> <span data-ttu-id="e4a98-261">CRUD eylem yöntemlerine otomatik olarak oluşturulmuş dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="e4a98-261">Notice that the CRUD action methods have been generated automatically.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="e4a98-262">Seçerek **zaman uyumsuz denetleyici eylemlerini kullanmak** iskele onay kutusundan seçenekleri önceki adımlarda, Visual Studio kişi veri bağlamı erişim gerektiren tüm eylemler için zaman uyumsuz eylem yöntemi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="e4a98-262">By selecting the **Use async controller actions** check box from the scaffolding options in the previous steps, Visual Studio generates asynchronous action methods for all actions that involve access to the Person data context.</span></span> <span data-ttu-id="e4a98-263">Uzun süre çalışan zaman uyumsuz eylem yöntemleri kullanın, Web sunucusu isteği işlenirken iş gerçekleştirmeyi engellenmesini önlemek için istekleri CPU dışı bağlı önerilir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-263">It is recommended that you use asynchronous action methods for long-running, non-CPU bound requests to avoid blocking the Web server from performing work while the request is being processed.</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a><span data-ttu-id="e4a98-264">Görev 3: çözümü çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e4a98-264">Task 3 – Running the Solution</span></span>

<span data-ttu-id="e4a98-265">Bu görevde, görünümleri yeniden doğrulamak için çözümü çalıştıracak **kişi** beklendiği gibi çalışıyor.</span><span class="sxs-lookup"><span data-stu-id="e4a98-265">In this task, you will run the solution again to verify that the views for **Person** are working as expected.</span></span> <span data-ttu-id="e4a98-266">Veritabanı başarılı bir şekilde kaydedildiğini doğrulamak için yeni bir kişiye ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e4a98-266">You will add a new person to verify that it is successfully saved to the database.</span></span>

1. <span data-ttu-id="e4a98-267">Tuşuna **F5** çözümü çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e4a98-267">Press **F5** to run the solution.</span></span>
2. <span data-ttu-id="e4a98-268">Gidin **/MvcPerson**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-268">Navigate to **/MvcPerson**.</span></span> <span data-ttu-id="e4a98-269">Kişilerin listesini gösteren iskele kurulmuş görünümü görüntülenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-269">The scaffolded view that shows the list of people should appear.</span></span>
3. <span data-ttu-id="e4a98-270">Tıklayın **Yeni Oluştur** yeni bir kişiye eklemek için.</span><span class="sxs-lookup"><span data-stu-id="e4a98-270">Click **Create New** to add a new person.</span></span>

    ![İskele kurulmuş MVC görünümlerine gezinme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    <span data-ttu-id="e4a98-272">*İskele kurulmuş MVC görünümlerine gezinme*</span><span class="sxs-lookup"><span data-stu-id="e4a98-272">*Navigating to the scaffolded MVC views*</span></span>
4. <span data-ttu-id="e4a98-273">İçinde **Oluştur** görüntülemek için sağlayan bir **adı** ve bir **yaş** kişi ve tıklayın **Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-273">In the **Create** view, provide a **Name** and an **Age** for the person, and click **Create**.</span></span>

    ![Yeni bir kişi ekleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    <span data-ttu-id="e4a98-275">*Yeni bir kişi ekleme*</span><span class="sxs-lookup"><span data-stu-id="e4a98-275">*Adding a new person*</span></span>
5. <span data-ttu-id="e4a98-276">Yeni bir kişiye listesine eklenir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-276">The new person is added to the list.</span></span> <span data-ttu-id="e4a98-277">Öğe listesinde **ayrıntıları** kişinin ayrıntıları görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="e4a98-277">In the element list, click **Details** to display the person's details view.</span></span> <span data-ttu-id="e4a98-278">Ardından **ayrıntıları** yi **listesine geri** liste görünümüne geri dönmek için.</span><span class="sxs-lookup"><span data-stu-id="e4a98-278">Then, in the **Details** view, click **Back to List** to go back to the list view.</span></span>

    ![Kişi Ayrıntıları görünümü](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    <span data-ttu-id="e4a98-280">*Kişi Ayrıntıları görünümü*</span><span class="sxs-lookup"><span data-stu-id="e4a98-280">*Person's details view*</span></span>
6. <span data-ttu-id="e4a98-281">Tıklayın **Sil** kişi silmek için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="e4a98-281">Click the **Delete** link to delete the person.</span></span> <span data-ttu-id="e4a98-282">İçinde **Sil** yi **Sil** işlemini onaylamak için.</span><span class="sxs-lookup"><span data-stu-id="e4a98-282">In the **Delete** view, click **Delete** to confirm the operation.</span></span>

    ![Bir kişi siliniyor](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    <span data-ttu-id="e4a98-284">*Bir kişi siliniyor*</span><span class="sxs-lookup"><span data-stu-id="e4a98-284">*Deleting a person*</span></span>
7. <span data-ttu-id="e4a98-285">Geri Git Visual Studio ve ENTER tuşuna **SHIFT + F5 tuşlarına basarak** hata ayıklamayı durdurmak için.</span><span class="sxs-lookup"><span data-stu-id="e4a98-285">Go back to Visual Studio and press **SHIFT + F5** to stop debugging.</span></span>

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a><span data-ttu-id="e4a98-286">Alıştırma 3: iskele kurma kullanarak bir Web API denetleyicisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e4a98-286">Exercise 3: Creating a Web API Controller Using Scaffolding</span></span>

<span data-ttu-id="e4a98-287">Web API çerçevesi ASP.NET yığınının bir parçasıdır ve uygulama HTTP Hizmetleri genel veri gönderme ve JSON veya XML biçimli bir RESTful API'si aracılığıyla alma kolaylaştırmak için tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="e4a98-287">The Web API framework is part of the ASP.NET Stack and designed to make implementing HTTP services easier, generally sending and receiving JSON- or XML-formatted data through a RESTful API.</span></span>

<span data-ttu-id="e4a98-288">Bu alıştırmada, ASP.NET iskeleti oluşturma yeniden bir Web API denetleyicisi oluşturmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="e4a98-288">In this exercise, you will use ASP.NET Scaffolding again to generate a Web API controller.</span></span> <span data-ttu-id="e4a98-289">Aynı kullanacağınız **kişi** ve **PersonContext** aynı kişi verilerini JSON biçiminde sağlamak için önceki alıştırmada sınıflardan.</span><span class="sxs-lookup"><span data-stu-id="e4a98-289">You will use the same **Person** and **PersonContext** classes from the previous exercise to provide the same person data in JSON format.</span></span> <span data-ttu-id="e4a98-290">Aynı ASP.NET uygulamasından farklı yollarla aynı kaynakları nasıl getirebilir görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e4a98-290">You will see how you can expose the same resources in different ways within the same ASP.NET application.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a><span data-ttu-id="e4a98-291">Görev 1-bir Web API denetleyicisi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e4a98-291">Task 1 – Creating a Web API Controller</span></span>

<span data-ttu-id="e4a98-292">Bu görevde, yeni bir oluşturacaksınız **Web API denetleyicisi** kişi verilerini JSON gibi makine tüketilebilir biçiminde kullanıma.</span><span class="sxs-lookup"><span data-stu-id="e4a98-292">In this task you will create a new **Web API Controller** that will expose the person data in a machine-consumable format like JSON.</span></span>

1. <span data-ttu-id="e4a98-293">Henüz açık değilse açın **Visual Studio Express 2013 Web** açın **MyHybridSite.sln** çözüm bulunan **kaynak/Ex3-Webapı/başlangıcı** klasör.</span><span class="sxs-lookup"><span data-stu-id="e4a98-293">If not already opened, open **Visual Studio Express 2013 for Web** and open the **MyHybridSite.sln** solution located in the **Source/Ex3-WebAPI/Begin** folder.</span></span> <span data-ttu-id="e4a98-294">Alternatif olarak, önceki alıştırmada aldığınız çözümüyle devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e4a98-294">Alternatively, you can continue with the solution that you obtained in the previous exercise.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4a98-295">Alıştırma 3 başlangıç çözüm ile Başlat, basın **CTRL + SHIFT + B** çözümü derlemek için.</span><span class="sxs-lookup"><span data-stu-id="e4a98-295">If you start with the Begin solution from Exercise 3, press **CTRL + SHIFT + B** to build the solution.</span></span>
2. <span data-ttu-id="e4a98-296">İçinde **Çözüm Gezgini**, sağ **denetleyicileri** klasörü **MyHybridSite** seçin ve proje **Ekle | Yeni İskeleli öğe...** .</span><span class="sxs-lookup"><span data-stu-id="e4a98-296">In **Solution Explorer**, right-click the **Controllers** folder of the **MyHybridSite** project and select **Add | New Scaffolded Item...**.</span></span>

    ![Yeni iskele kurulmuş denetleyici oluşturma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    <span data-ttu-id="e4a98-298">*Yeni iskele kurulmuş denetleyici oluşturma*</span><span class="sxs-lookup"><span data-stu-id="e4a98-298">*Creating a new scaffolded Controller*</span></span>
3. <span data-ttu-id="e4a98-299">İçinde **İskele Ekle** iletişim kutusunda **Web API** sol bölmesinde, ardından **Web API 2 denetleyici Entity Framework kullanarak Eylemler ile** Orta bölmede ve 'yetıklayın **Ekleyin.**</span><span class="sxs-lookup"><span data-stu-id="e4a98-299">In the **Add Scaffold** dialog box, select **Web API** in the left pane, then **Web API 2 Controller with actions, using Entity Framework** in the middle pane and then click **Add.**</span></span>

    <span data-ttu-id="e4a98-300">![Eylemler ve Entity Framework ile Web API 2 denetleyicisi seçerek](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Eylemler ve Entity Framework ile Web API 2 denetleyicisi seçme")</span><span class="sxs-lookup"><span data-stu-id="e4a98-300">![Selecting Web API 2 Controller with actions and Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "Selecting Web API 2 Controller with actions and Entity Framework")</span></span>

    <span data-ttu-id="e4a98-301">*Eylemler ve Entity Framework ile Web API 2 denetleyicisi seçme*</span><span class="sxs-lookup"><span data-stu-id="e4a98-301">*Selecting Web API 2 Controller with actions and Entity Framework*</span></span>
4. <span data-ttu-id="e4a98-302">Ayarlama *ApiPersonController* olarak **Denetleyici adı**seçin **zaman uyumsuz denetleyici eylemlerini kullanmak** seçeneğini işaretleyip **kişi (MyHybridSite.Models)**  ve **PersonContext (MyHybridSite.Models)** olarak **modeli** ve **veri bağlamı** sırasıyla sınıfları.</span><span class="sxs-lookup"><span data-stu-id="e4a98-302">Set *ApiPersonController* as the **Controller name**, select the **Use async controller actions** option and select **Person (MyHybridSite.Models)** and **PersonContext (MyHybridSite.Models)** as the **Model** and **Data context** classes respectively.</span></span> <span data-ttu-id="e4a98-303">Ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-303">Then click **Add**.</span></span>

    <span data-ttu-id="e4a98-304">![Yapı iskelesi ile Web API denetleyici ekleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "yapı iskelesi ile Web API denetleyici ekleme")</span><span class="sxs-lookup"><span data-stu-id="e4a98-304">![Adding a Web API Controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "Adding a Web API controller with scaffolding")</span></span>

    <span data-ttu-id="e4a98-305">*Yapı iskelesi ile Web API denetleyici ekleme*</span><span class="sxs-lookup"><span data-stu-id="e4a98-305">*Adding a Web API controller with scaffolding*</span></span>
5. <span data-ttu-id="e4a98-306">Visual Studio ardından oluşturacağını **ApiPersonController** verilerinizle çalışmaya dört CRUD eylemleri ile sınıfı.</span><span class="sxs-lookup"><span data-stu-id="e4a98-306">Visual Studio will then generate the **ApiPersonController** class with the four CRUD actions to work with your data.</span></span>

    <span data-ttu-id="e4a98-307">![Yapı iskelesi ile Web API denetleyicisi oluşturduktan sonra](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "yapı iskelesi ile Web API denetleyicisi oluşturduktan sonra")</span><span class="sxs-lookup"><span data-stu-id="e4a98-307">![After creating the Web API controller with scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "After creating the Web API controller with scaffolding")</span></span>

    <span data-ttu-id="e4a98-308">*Yapı iskelesi ile Web API denetleyicisi oluşturduktan sonra*</span><span class="sxs-lookup"><span data-stu-id="e4a98-308">*After creating the Web API controller with scaffolding*</span></span>
6. <span data-ttu-id="e4a98-309">Açık **ApiPersonController.cs** dosya ve incelemek *GetPeople* eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e4a98-309">Open the **ApiPersonController.cs** file and inspect the *GetPeople* action method.</span></span> <span data-ttu-id="e4a98-310">Bu yöntem db alanı sorgular **PersonContext** kişiler veri alabilmek için türü.</span><span class="sxs-lookup"><span data-stu-id="e4a98-310">This method queries the db field of **PersonContext** type in order to get the people data.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. <span data-ttu-id="e4a98-311">Artık yorum yöntem tanımını yukarıda dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="e4a98-311">Now notice the comment above the method definition.</span></span> <span data-ttu-id="e4a98-312">Bu, sonraki görevde kullanacağınız bu eylemi kullanıma sunar. URI'yi sağlar.</span><span class="sxs-lookup"><span data-stu-id="e4a98-312">It provides the URI that exposes this action which you will use in the next task.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > <span data-ttu-id="e4a98-313">Varsayılan olarak, Web API'si sorguları yakalamak için yapılandırılmış */API'si* MVC denetleyicileri ile çarpışmalardan kaçınmak için yol.</span><span class="sxs-lookup"><span data-stu-id="e4a98-313">By default, Web API is configured to catch the queries to the */api* path to avoid collisions with MVC controllers.</span></span> <span data-ttu-id="e4a98-314">Bu ayarı değiştirmek istiyorsanız, başvurmak [ASP.NET Web API'de yönlendirme](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e4a98-314">If you need to change this setting, refer to [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a><span data-ttu-id="e4a98-315">Görev 2 – çözümü çalıştırma</span><span class="sxs-lookup"><span data-stu-id="e4a98-315">Task 2 – Running the Solution</span></span>

<span data-ttu-id="e4a98-316">Bu görevde, Internet Explorer kullandığınız **F12 Geliştirici araçlarıyla** Web API denetleyicisi gelen tam yanıtı denetlemek için.</span><span class="sxs-lookup"><span data-stu-id="e4a98-316">In this task you will use the Internet Explorer **F12 developer tools** to inspect the full response from the Web API controller.</span></span> <span data-ttu-id="e4a98-317">Uygulama verileriniz hakkında daha fazla bilgi almak için ağ trafiğini yakalamak için nasıl görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e4a98-317">You will see how you can capture network traffic to get more insight into your application data.</span></span>

> [!NOTE]
> <span data-ttu-id="e4a98-318">Emin olun **Internet Explorer** seçili **Başlat** düğmesi Visual Studio araç çubuğunda yer alan.</span><span class="sxs-lookup"><span data-stu-id="e4a98-318">Make sure that **Internet Explorer** is selected in the **Start** button located on the Visual Studio toolbar.</span></span>
> 
> ![Internet Explorer seçeneği](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> <span data-ttu-id="e4a98-320">**F12 Geliştirici araçlarıyla** geniş Bu uygulamalı-lab içinde kapsanmayan işlevleri kümesine sahiptir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-320">The **F12 developer tools** have a wide set of functionality that is not covered in this hands-on-lab.</span></span> <span data-ttu-id="e4a98-321">Hakkında daha fazla bilgi edinmek istiyorsanız, başvurmak [F12 geliştirici araçlarını kullanarak](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span><span class="sxs-lookup"><span data-stu-id="e4a98-321">If you want to learn more about it, refer to [Using the F12 developer tools](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).</span></span>


1. <span data-ttu-id="e4a98-322">Tuşuna **F5** çözümü çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e4a98-322">Press **F5** to run the solution.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4a98-323">Bu görevi doğru takip etmek için uygulamanız verileri olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e4a98-323">In order to follow this task correctly, your application needs to have data.</span></span> <span data-ttu-id="e4a98-324">Veritabanınızı boşsa, görev 3'te alıştırma 2 dönün ve MVC görünümleri kullanarak yeni bir kişiye oluşturma konusunda adımları izleyin.</span><span class="sxs-lookup"><span data-stu-id="e4a98-324">If your database is empty, you can go back to Task 3 in Exercise 2 and follow the steps on how to create a new person using the MVC views.</span></span>
2. <span data-ttu-id="e4a98-325">Tarayıcıda basın **F12** açmak için **Geliştirici Araçları** paneli.</span><span class="sxs-lookup"><span data-stu-id="e4a98-325">In the browser, press **F12** to open the **Developer Tools** panel.</span></span> <span data-ttu-id="e4a98-326">Tuşuna **CTRL** + **4** veya **ağ** simgesine ve ardından ağ trafiğini yakalamaktan başlamak için yeşil ok düğmesi.</span><span class="sxs-lookup"><span data-stu-id="e4a98-326">Press **CTRL** + **4** or click the **Network** icon, and then click the green arrow button to begin capturing network traffic.</span></span>

    <span data-ttu-id="e4a98-327">![Web API ağ yakalama başlatma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "başlatma Web API ağ yakalama")</span><span class="sxs-lookup"><span data-stu-id="e4a98-327">![Initiating Web API network capture](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "Initiating Web API network capture")</span></span>

    <span data-ttu-id="e4a98-328">*Web API ağ yakalama başlatılıyor*</span><span class="sxs-lookup"><span data-stu-id="e4a98-328">*Initiating Web API network capture*</span></span>
3. <span data-ttu-id="e4a98-329">Append **API/ApiPerson** tarayıcının adres çubuğundaki URL.</span><span class="sxs-lookup"><span data-stu-id="e4a98-329">Append **api/ApiPerson** to the URL in the browser's address bar.</span></span> <span data-ttu-id="e4a98-330">Şimdi gelen yanıt ayrıntılarını inceleyeceksiniz **ApiPersonController**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-330">You will now inspect the details of the response from the **ApiPersonController**.</span></span>

    <span data-ttu-id="e4a98-331">![Web API aracılığıyla kişi verilerini alma](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Web API aracılığıyla kişi verilerini alma")</span><span class="sxs-lookup"><span data-stu-id="e4a98-331">![Retrieving person data through Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Retrieving person data through Web API")</span></span>

    <span data-ttu-id="e4a98-332">*Web API aracılığıyla kişi verilerini alma*</span><span class="sxs-lookup"><span data-stu-id="e4a98-332">*Retrieving person data through Web API*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4a98-333">İndirme tamamlandıktan sonra indirilen dosya ile bir eylem yapmanız istenir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-333">Once the download finishes, you will be prompted to make an action with the downloaded file.</span></span> <span data-ttu-id="e4a98-334">İletişim kutusu, yanıt içeriği geliştiriciler araç penceresi aracılığıyla izlemek için açık bırakın.</span><span class="sxs-lookup"><span data-stu-id="e4a98-334">Leave the dialog box open in order to be able to watch the response content through the Developers Tool window.</span></span>
4. <span data-ttu-id="e4a98-335">Artık yanıt gövdesinin inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="e4a98-335">Now you will inspect the body of the response.</span></span> <span data-ttu-id="e4a98-336">Bunu yapmak için tıklatın **ayrıntıları** sekmesine ve ardından **yanıt gövdesi**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-336">To do this, click the **Details** tab and then click **Response body**.</span></span> <span data-ttu-id="e4a98-337">İndirilen veriler özellikleriyle nesnelerin bir listesini olduğunu denetleyebilirsiniz **kimliği**, **adı** ve **yaş** karşılık gelen için **kişi** sınıf.</span><span class="sxs-lookup"><span data-stu-id="e4a98-337">You can check that the downloaded data is a list of objects with the properties **Id**, **Name** and **Age** that correspond to the **Person** class.</span></span>

    <span data-ttu-id="e4a98-338">![Web API yanıt gövdesi görüntüleme](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "görüntüleme Web API yanıt gövdesi")</span><span class="sxs-lookup"><span data-stu-id="e4a98-338">![Viewing Web API Response Body](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "Viewing Web API Response Body")</span></span>

    <span data-ttu-id="e4a98-339">*Web API yanıt gövdesi görüntüleme*</span><span class="sxs-lookup"><span data-stu-id="e4a98-339">*Viewing Web API Response Body*</span></span>

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a><span data-ttu-id="e4a98-340">Görev 3: Web API Yardım sayfaları ekleme</span><span class="sxs-lookup"><span data-stu-id="e4a98-340">Task 3 – Adding Web API Help Pages</span></span>

<span data-ttu-id="e4a98-341">Bir Web API'si oluşturma, böylece diğer geliştiriciler, API'nin nasıl çağrılacağını öğrenmiş olacaksınız Yardım sayfasını oluşturmak kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="e4a98-341">When you create a Web API, it is useful to create a help page so that other developers will know how to call your API.</span></span> <span data-ttu-id="e4a98-342">Oluşturma ve belge sayfaları el ile güncelleştirmeniz, ancak bunları bakım işi yapmak zorunda kalmamak için otomatik olarak oluşturmak iyidir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-342">You could create and update the documentation pages manually, but it is better to auto-generate them to avoid having to do maintenance work.</span></span> <span data-ttu-id="e4a98-343">Bu görevde Web API Yardım sayfaları çözüme otomatik olarak oluşturmak için bir Nuget paketi kullanır.</span><span class="sxs-lookup"><span data-stu-id="e4a98-343">In this task you will use a Nuget package to automatically generate Web API help pages to the solution.</span></span>

1. <span data-ttu-id="e4a98-344">Gelen **Araçları** Visual Studio'da seçim menüsünde **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-344">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="e4a98-345">İçinde **Paket Yöneticisi Konsolu** penceresinde aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="e4a98-345">In the **Package Manager Console** window, execute the following command:</span></span>

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > <span data-ttu-id="e4a98-346">**Microsoft.AspNet.WebApi.HelpPage** paketi gerekli bütünleştirilmiş kodları yükler ve Yardım sayfaları'nın altında MVC görünümleri ekler **alanlar/HelpPage** klasör.</span><span class="sxs-lookup"><span data-stu-id="e4a98-346">The **Microsoft.AspNet.WebApi.HelpPage** package installs the necessary assemblies and adds MVC Views for the help pages under the **Areas/HelpPage** folder.</span></span>

    <span data-ttu-id="e4a98-347">![HelpPage alan](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage alan")</span><span class="sxs-lookup"><span data-stu-id="e4a98-347">![HelpPage Area](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage Area")</span></span>

    <span data-ttu-id="e4a98-348">*HelpPage alanı*</span><span class="sxs-lookup"><span data-stu-id="e4a98-348">*HelpPage Area*</span></span>
3. <span data-ttu-id="e4a98-349">Varsayılan olarak, Yardım sayfaları, belgeler için yer tutucu dizeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="e4a98-349">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="e4a98-350">XML belgeleri yorumları belgeleri oluşturmak için kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e4a98-350">You can use XML documentation comments to create the documentation.</span></span> <span data-ttu-id="e4a98-351">Bu özelliği etkinleştirmek için açık **HelpPageConfig.cs** bulunan dosya **HelpPage/alanlar/uygulama\_Başlat** klasörü ve aşağıdaki satırı açıklamadan çıkarın:</span><span class="sxs-lookup"><span data-stu-id="e4a98-351">To enable this feature, open the **HelpPageConfig.cs** file located in the **Areas/HelpPage/App\_Start** folder and uncomment the following line:</span></span>

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. <span data-ttu-id="e4a98-352">İçinde **Çözüm Gezgini**, projeye sağ tıklayın **MyHybridSite**seçin **özellikleri** tıklatıp **derleme** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="e4a98-352">In **Solution Explorer**, right-click the project **MyHybridSite**, select **Properties** and click the **Build** tab.</span></span>

    <span data-ttu-id="e4a98-353">![Derleme sekmesi](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "derleme bölümü")</span><span class="sxs-lookup"><span data-stu-id="e4a98-353">![Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "Build section")</span></span>

    <span data-ttu-id="e4a98-354">*Derleme sekmesi*</span><span class="sxs-lookup"><span data-stu-id="e4a98-354">*Build tab*</span></span>
5. <span data-ttu-id="e4a98-355">Altında **çıkış**seçin **XML belge dosyası**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-355">Under **Output**, select **XML documentation file**.</span></span> <span data-ttu-id="e4a98-356">Düzenleme kutusuna **uygulama\_Data/XmlDocument.xml**.</span><span class="sxs-lookup"><span data-stu-id="e4a98-356">In the edit box, type **App\_Data/XmlDocument.xml**.</span></span>

    <span data-ttu-id="e4a98-357">![Çıkış derleme sekmesi bölümünde](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "çıkış bölümünde derleme sekmesi")</span><span class="sxs-lookup"><span data-stu-id="e4a98-357">![Output section in Build tab](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "Output section in build tab")</span></span>

    <span data-ttu-id="e4a98-358">*Çıkış bölümünde derleme sekmesi*</span><span class="sxs-lookup"><span data-stu-id="e4a98-358">*Output section in Build tab*</span></span>
6. <span data-ttu-id="e4a98-359">Tuşuna **CTRL** + **S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="e4a98-359">Press **CTRL** + **S** to save the changes.</span></span>
7. <span data-ttu-id="e4a98-360">Açık **ApiPersonController.cs** dosya **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="e4a98-360">Open the **ApiPersonController.cs** file from the **Controllers** folder.</span></span>
8. <span data-ttu-id="e4a98-361">Arasında yeni bir satıra girin *GetPeople* yöntem imzası ve */ / GET API/ApiPerson* açıklama satırı yapın ve ardından üç eğik yazın.</span><span class="sxs-lookup"><span data-stu-id="e4a98-361">Enter a new line between the *GetPeople* method signature and the *// GET api/ApiPerson* comment, and then type three forward slashes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4a98-362">Visual Studio yöntemi belgelerine tanımlayan XML öğeleri otomatik olarak ekler.</span><span class="sxs-lookup"><span data-stu-id="e4a98-362">Visual Studio automatically inserts the XML elements which define the method documentation.</span></span>
9. <span data-ttu-id="e4a98-363">Bir Özet metni ve dönüş değeri Ekle *GetPeople* yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e4a98-363">Add a summary text and the return value for the *GetPeople* method.</span></span> <span data-ttu-id="e4a98-364">Aşağıdaki gibi görünmelidir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-364">It should look like the following.</span></span>

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. <span data-ttu-id="e4a98-365">Tuşuna **F5** çözümü çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e4a98-365">Press **F5** to run the solution.</span></span>
11. <span data-ttu-id="e4a98-366">Append **/help** adres çubuğundaki URL'ye yardım sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="e4a98-366">Append **/help** to the URL in the address bar to browse to the help page.</span></span>

    <span data-ttu-id="e4a98-367">![ASP.NET Web API Yardım sayfası](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Yardım sayfası")</span><span class="sxs-lookup"><span data-stu-id="e4a98-367">![ASP.NET Web API Help Page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API Help Page")</span></span>

    <span data-ttu-id="e4a98-368">*ASP.NET Web API Yardım sayfası*</span><span class="sxs-lookup"><span data-stu-id="e4a98-368">*ASP.NET Web API Help Page*</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4a98-369">Ana sayfanın içeriğini API'leri, denetleyici tarafından gruplandırılmış bir tablodur.</span><span class="sxs-lookup"><span data-stu-id="e4a98-369">The main content of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="e4a98-370">Tablo girişleri kullanarak dinamik olarak oluşturulan **IApiExplorer** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="e4a98-370">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="e4a98-371">Ekler veya güncelleştirirseniz bir API denetleyicisi, tablonun uygulamayı sonraki açışınızda otomatik olarak güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-371">If you add or update an API controller, the table will be automatically updated the next time you build the application.</span></span>
    > 
    > <span data-ttu-id="e4a98-372">**API** göreli URI ve HTTP yöntemi sütununda listelenir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-372">The **API** column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="e4a98-373">**Açıklama** sütun yöntemin belgelerinden ayıklanan bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="e4a98-373">The **Description** column contains information that has been extracted from the method's documentation.</span></span>
12. <span data-ttu-id="e4a98-374">Metot tanımına eklenen açıklama Açıklama sütununda görüntülendiğini unutmayın.</span><span class="sxs-lookup"><span data-stu-id="e4a98-374">Note that the description you added above the method definition is displayed in the description column.</span></span>

    <span data-ttu-id="e4a98-375">![API yöntemi açıklaması](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API yöntemi açıklaması")</span><span class="sxs-lookup"><span data-stu-id="e4a98-375">![API method description](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API method description")</span></span>

    <span data-ttu-id="e4a98-376">*API yöntemi açıklaması*</span><span class="sxs-lookup"><span data-stu-id="e4a98-376">*API method description*</span></span>
13. <span data-ttu-id="e4a98-377">Örnek yanıt gövdeleri dahil olmak üzere daha ayrıntılı bilgi içeren bir sayfaya gitmek için API yöntemleri birine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="e4a98-377">Click one of the API methods to navigate to a page with more detailed information, including sample response bodies.</span></span>

    <span data-ttu-id="e4a98-378">![Ayrıntı bilgi sayfası](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "ayrıntı bilgileri sayfası")</span><span class="sxs-lookup"><span data-stu-id="e4a98-378">![Detail Information page](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "Detail Information page")</span></span>

    <span data-ttu-id="e4a98-379">*Ayrıntılı bilgi sayfası*</span><span class="sxs-lookup"><span data-stu-id="e4a98-379">*Detailed information page*</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="e4a98-380">Özet</span><span class="sxs-lookup"><span data-stu-id="e4a98-380">Summary</span></span>

<span data-ttu-id="e4a98-381">Öğrendiğiniz Bu uygulamalı laboratuvarı tamamlayarak nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="e4a98-381">By completing this hands-on lab you have learned how to:</span></span>

- <span data-ttu-id="e4a98-382">Visual Studio 2013'te bir ASP.NET deneyimi kullanarak yeni bir Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="e4a98-382">Create a new Web application using the One ASP.NET Experience in Visual Studio 2013</span></span>
- <span data-ttu-id="e4a98-383">Tek bir projeye birden çok ASP.NET teknolojileri tümleştirme</span><span class="sxs-lookup"><span data-stu-id="e4a98-383">Integrate multiple ASP.NET technologies into one single project</span></span>
- <span data-ttu-id="e4a98-384">MVC denetleyicileri ve görünümleri kullanarak ASP.NET iskeleti oluşturma, modeli sınıfların üretileceği</span><span class="sxs-lookup"><span data-stu-id="e4a98-384">Generate MVC controllers and views from your model classes using ASP.NET Scaffolding</span></span>
- <span data-ttu-id="e4a98-385">Zaman uyumsuz programlama ve Entity Framework ile veri erişimi gibi özellikleri kullanan Web APİ'si denetleyicilerinin oluştur</span><span class="sxs-lookup"><span data-stu-id="e4a98-385">Generate Web API controllers, which use features such as Async Programming and data access through Entity Framework</span></span>
- <span data-ttu-id="e4a98-386">Web API Yardım sayfaları denetleyicileriniz için otomatik olarak oluştur</span><span class="sxs-lookup"><span data-stu-id="e4a98-386">Automatically generate Web API Help Pages for your controllers</span></span>
