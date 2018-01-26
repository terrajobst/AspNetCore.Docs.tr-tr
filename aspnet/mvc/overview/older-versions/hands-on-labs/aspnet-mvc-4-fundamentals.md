---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 temelleri | Microsoft Docs
author: rick-anderson
description: "Bu uygulamalı Laboratuvar MVC (Model View Controller) müzik deposu tanıtır ve ASP.NET MV kullanmak adım adım açıklanmaktadır öğretici bir uygulama tabanlı..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 468f6d5dabb645b1c005680dc5a1ffc4debd63b6
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="db6ad-103">ASP.NET MVC 4 temelleri</span><span class="sxs-lookup"><span data-stu-id="db6ad-103">ASP.NET MVC 4 Fundamentals</span></span>
====================
<span data-ttu-id="db6ad-104">tarafından [Web Camps ekibi](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="db6ad-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

> <span data-ttu-id="db6ad-105">Bu uygulamalı Laboratuvar MVC (Model View Controller) müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio nasıl kullanılacağını hakkında adım adım anlatan öğretici bir uygulama temel alır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-105">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="db6ad-106">Laboratuvar Basitlik öğreneceksiniz henüz güç bu teknolojiler birlikte kullanma.</span><span class="sxs-lookup"><span data-stu-id="db6ad-106">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="db6ad-107">Basit bir uygulama ile başlar ve tam olarak işlevsel bir ASP.NET MVC 4 Web uygulaması elde edene kadar oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="db6ad-107">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>
> 
> <span data-ttu-id="db6ad-108">Bu Laboratuvar, ASP.NET MVC 4 ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-108">This Lab works with ASP.NET MVC 4.</span></span>
> 
> <span data-ttu-id="db6ad-109">Eğitmen uygulama ASP.NET MVC 3 sürümünü keşfetmek isterseniz içinde bulabilirsiniz [MVC müzik deposu](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="db6ad-109">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="db6ad-110">Bu uygulamalı Laboratuvar Geliştirici HTML ve JavaScript gibi Web geliştirme teknolojilerinde deneyim sahibi olduğunu varsayar.</span><span class="sxs-lookup"><span data-stu-id="db6ad-110">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>
> 
> 
> <span data-ttu-id="db6ad-111">Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span><span class="sxs-lookup"><span data-stu-id="db6ad-111">All sample code and snippets are included in the Web Camps Training Kit, available at [https://www.microsoft.com/download/29843](https://www.microsoft.com/download/29843).</span></span>


<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="db6ad-112">Müzik deposu uygulama</span><span class="sxs-lookup"><span data-stu-id="db6ad-112">The Music Store application</span></span>

<span data-ttu-id="db6ad-113">Bu Laboratuvar oluşturulacak müzik deposu web uygulaması üç ana bölümden oluşur: alışveriş, kullanıma alma ve yönetim.</span><span class="sxs-lookup"><span data-stu-id="db6ad-113">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="db6ad-114">Ziyaretçilerin albümleri türe göre Gözat, Albümler kendi Sepete Ekle, bunların seçimini inceleyin ve son olarak oturum açma ve sırasını tamamlamak için kullanıma alma devam mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-114">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="db6ad-115">Ayrıca, depolama yöneticileri ana özelliklerinin yanı sıra kullanılabilir albümleri yönetebilmek için olacaktır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-115">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="db6ad-116">![Müzik deposu ekranlar](aspnet-mvc-4-fundamentals/_static/image1.png "müzik deposu ekranları")</span><span class="sxs-lookup"><span data-stu-id="db6ad-116">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="db6ad-117">*Müzik deposu ekranlar*</span><span class="sxs-lookup"><span data-stu-id="db6ad-117">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="db6ad-118">ASP.NET MVC 4 temelleri</span><span class="sxs-lookup"><span data-stu-id="db6ad-118">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="db6ad-119">Müzik deposu uygulaması kullanılarak oluşturulur **Model View Controller (MVC)**, uygulamanın üç ana bileşene ayırır mimari bir desen:</span><span class="sxs-lookup"><span data-stu-id="db6ad-119">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="db6ad-120">**Modelleri**: Model nesneleri olan etki alanı mantığı uygulamak uygulama bölümlerinin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-120">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="db6ad-121">Genellikle, model nesneleri de alabilir ve model durumu bir veritabanında depolar.</span><span class="sxs-lookup"><span data-stu-id="db6ad-121">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="db6ad-122">**Görünümleri:** görünümler, uygulamanın kullanıcı arabirimini (UI) görüntüleme bileşenleridir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-122">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="db6ad-123">Genellikle, bu UI model verilerinden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-123">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="db6ad-124">Bir örnek metin kutuları ve açılan listesini albüm nesnenin geçerli durumu görüntüler, Albümler düzenleme görünümünü olabilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-124">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="db6ad-125">**Denetleyicileri:** denetleyicileri, kullanıcı etkileşimini işleyen, modelle ve sonuçta kullanıcı arabirimini oluşturmak için bir görünüm seçin bileşenleridir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-125">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="db6ad-126">Bir MVC uygulamasında görünüm yalnızca bilgileri görüntüler; Denetleyici işler ve kullanıcı girişini ve etkileşimini yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-126">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="db6ad-127">MVC örüntüsü, öğeler arasında gevşek bağlantı sağlarken uygulama (giriş mantığı, iş mantığı ve UI mantığı) farklı yönlerini ayrı uygulamaları oluşturmak için yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-127">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="db6ad-128">Bu ayrım bir uygulama oluşturduğunuzda bir seferde bir yönüne odaklanmanızı sağlar gibi karmaşıklığını yönetmenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-128">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="db6ad-129">Buna ek olarak, MVC örüntüsü, uygulamalar, aynı zamanda teste dayalı geliştirme (TDD) kullanımını uygulamaları oluşturmak ve sınamak kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-129">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="db6ad-130">**ASP.NET MVC** framework ASP.NET MVC tabanlı Web uygulamaları oluşturmak için ASP.NET Web Forms örüntüsüne bir alternatif sunar.</span><span class="sxs-lookup"><span data-stu-id="db6ad-130">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="db6ad-131">**ASP.NET MVC** çerçevedir basit, yüksek düzeyde sınanabilir bir sunu çerçevesidir, (Web forms tabanlı uygulamaları olduğu gibi) mevcut ASP.NET özellikleriyle gibi ana sayfalar ve üyelik tabanlı Tümleşik Çekirdek .NET framework'ün tüm güç almak için kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="db6ad-131">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="db6ad-132">Bu zaten kullandığınız tüm kitaplıkları da ASP.NET MVC 4'te kullanılabilir olmadığından, ASP.NET Web Forms bilginiz varsa yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-132">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="db6ad-133">Ayrıca, bir MVC uygulamasının ilgili üç ana bileşeni arasındaki bu sıkı bağ paralel gelişimi de kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-133">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="db6ad-134">Örneğin, bir geliştirici görünümde çalışabilir, ikinci bir geliştirici denetleyici mantığında çalışabilir ve üçüncü Geliştirici üzerinde modeldeki iş mantığına odaklanabilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-134">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="db6ad-135">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="db6ad-135">Objectives</span></span>

<span data-ttu-id="db6ad-136">Bu uygulamalı laboratuar ortamında, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="db6ad-136">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="db6ad-137">Müzik deposu uygulama öğretici temel sıfırdan bir ASP.NET MVC uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="db6ad-137">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="db6ad-138">Giriş sayfasına sitenin ve ana işlevselliğini gözatmak için URL'leri işlemek için denetleyicileri ekleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-138">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="db6ad-139">Kendi stil birlikte görüntülenen içeriği özelleştirmek için bir görünüm ekleyin</span><span class="sxs-lookup"><span data-stu-id="db6ad-139">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="db6ad-140">İçerir ve veri ve etki alanı mantığı yönetmek için modeli sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-140">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="db6ad-141">Görünüm şablonları denetleyicisi eylemlerden bilgi geçirmek görünüm modeli deseni kullan</span><span class="sxs-lookup"><span data-stu-id="db6ad-141">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="db6ad-142">Internet uygulamaları için ASP.NET MVC 4 yeni şablon keşfedin</span><span class="sxs-lookup"><span data-stu-id="db6ad-142">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="db6ad-143">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="db6ad-143">Prerequisites</span></span>

<span data-ttu-id="db6ad-144">Bu laboratuvarı tamamlamak için aşağıdaki öğeleri sahip olmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="db6ad-144">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="db6ad-145">[Web için Visual Studio 2012 Express](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için)</span><span class="sxs-lookup"><span data-stu-id="db6ad-145">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="db6ad-146">Kurulum</span><span class="sxs-lookup"><span data-stu-id="db6ad-146">Setup</span></span>

<span data-ttu-id="db6ad-147">**Kod parçacıkları yükleme**</span><span class="sxs-lookup"><span data-stu-id="db6ad-147">**Installing Code Snippets**</span></span>

<span data-ttu-id="db6ad-148">Kolaylık olması için bu Laboratuvar yönetme kod çoğunu Visual Studio kod parçacıkları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-148">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="db6ad-149">Çalıştırma kod parçacıkları yüklemek için **.\Source\Setup\CodeSnippets.vsi** dosya.</span><span class="sxs-lookup"><span data-stu-id="db6ad-149">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="db6ad-150">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istiyorsanız bilmiyorsanız, bu belgedeki eke başvurabilir &quot; [ek C: kullanarak kod parçacıkları](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="db6ad-150">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="db6ad-151">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="db6ad-151">Exercises</span></span>

<span data-ttu-id="db6ad-152">Bu uygulamalı Laboratuvar tarafından aşağıdaki alıştırmaları oluşmaktadır:</span><span class="sxs-lookup"><span data-stu-id="db6ad-152">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="db6ad-153">Alıştırma 1: MusicStore ASP.NET MVC Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="db6ad-153">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="db6ad-154">Alıştırma 2: bir denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="db6ad-154">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="db6ad-155">Alıştırma 3: bir denetleyiciye parametreleri geçirme</span><span class="sxs-lookup"><span data-stu-id="db6ad-155">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="db6ad-156">Alıştırma 4: bir görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="db6ad-156">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="db6ad-157">Alıştırma 5: görünüm Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="db6ad-157">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="db6ad-158">Alıştırma 6: görünümünde parametrelerini kullanma</span><span class="sxs-lookup"><span data-stu-id="db6ad-158">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="db6ad-159">Alıştırma 7: ASP.NET MVC 4 yeni şablonu çevresinde bir diz</span><span class="sxs-lookup"><span data-stu-id="db6ad-159">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="db6ad-160">Her alıştırma tarafından eşlik bir **son** elde alıştırmaları tamamladıktan sonra sonuçta elde edilen çözümü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="db6ad-160">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="db6ad-161">Alıştırmaları ile çalışma hakkında ek Yardım gerekirse, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-161">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="db6ad-162">Bu laboratuvarı tamamlamak için süre tahmini: **60 dakika**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-162">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="db6ad-163">Alıştırma 1: MusicStore ASP.NET MVC Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="db6ad-163">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="db6ad-164">Bu alıştırmada, bir ASP.NET MVC uygulaması Visual Studio Express 2012 ana klasör kuruluş yanı sıra Web için nasıl oluşturulacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-164">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="db6ad-165">Ayrıca, yeni denetleyici ekleyin ve basit bir dize uygulamanın giriş sayfası görüntüleme sağlamak nasıl öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-165">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="db6ad-166">Görev 1 - ASP.NET MVC Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="db6ad-166">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="db6ad-167">Bu görevde, MVC Visual Studio şablonu kullanarak boş bir ASP.NET MVC uygulaması projesi oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="db6ad-167">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="db6ad-168">Başlat **VS için Web Express**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-168">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="db6ad-169">Üzerinde **dosya** menüsünde tıklatın **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-169">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="db6ad-170">İçinde **yeni proje** iletişim kutusu seç **ASP.NET MVC 4 Web uygulaması** proje türü, altında bulunan **Visual C#** **Web** şablonu Liste.</span><span class="sxs-lookup"><span data-stu-id="db6ad-170">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="db6ad-171">Değişiklik **adı** için *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="db6ad-171">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="db6ad-172">İçinde yeni bir çözüm konumunu ayarlama **başlamak** klasöründe Bu alıştırmada'nın kaynak, örneğin **[YOUR HOL yol] \Source\Ex01-CreatingMusicStoreProject\Begin**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-172">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="db6ad-173">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-173">Click **OK**.</span></span>

    <span data-ttu-id="db6ad-174">![Yeni Proje iletişim kutusu oluşturma](aspnet-mvc-4-fundamentals/_static/image2.png "yeni proje iletişim kutusu oluşturma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-174">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="db6ad-175">*Yeni Proje iletişim kutusu oluşturma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-175">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="db6ad-176">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusu seç **temel** şablonu emin olun **görünüm altyapısı** seçili **Razor**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-176">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="db6ad-177">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-177">Click **OK**.</span></span>

    <span data-ttu-id="db6ad-178">![Yeni ASP.NET MVC 4 Proje iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image3.png "yeni ASP.NET MVC 4 Proje iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="db6ad-178">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="db6ad-179">*Yeni ASP.NET MVC 4 Proje iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="db6ad-179">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="db6ad-180">Görev 2 - çözüm yapısını keşfetme</span><span class="sxs-lookup"><span data-stu-id="db6ad-180">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="db6ad-181">ASP.NET MVC çerçevesi, MVC örüntüsünü destekleyen Web uygulamaları oluşturmanıza yardımcı olacak bir Visual Studio Proje şablonu içerir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-181">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="db6ad-182">Bu şablon, gerekli klasörleri, öğe şablonları ve yapılandırma dosyası girdileri yeni bir ASP.NET MVC Web uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-182">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="db6ad-183">Bu görevde, ilgili olan öğeler anlamak için çözüm yapısını ve ilişkilerini inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-183">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="db6ad-184">ASP.NET MVC çerçevesi varsayılan olarak kullandığı için aşağıdaki klasörlerin tüm ASP.NET MVC uygulamasında bulunan bir &quot;kuralı yapılandırması üzerinden&quot; yaklaşım ve bazı varsayılan varsayımları temel klasörü adlandırma yapar kuralları.</span><span class="sxs-lookup"><span data-stu-id="db6ad-184">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="db6ad-185">Proje oluşturulduktan sonra sağ taraftaki Solution Explorer'da oluşturulan klasör yapısı gözden geçirin:</span><span class="sxs-lookup"><span data-stu-id="db6ad-185">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="db6ad-186">![Çözüm Gezgini'nde ASP.NET MVC klasör yapısını](aspnet-mvc-4-fundamentals/_static/image4.png "Çözüm Gezgini'nde ASP.NET MVC klasör yapısı")</span><span class="sxs-lookup"><span data-stu-id="db6ad-186">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="db6ad-187">*Çözüm Gezgini'nde ASP.NET MVC klasör yapısı*</span><span class="sxs-lookup"><span data-stu-id="db6ad-187">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

    1. <span data-ttu-id="db6ad-188">**Denetleyicileri**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-188">**Controllers**.</span></span> <span data-ttu-id="db6ad-189">Bu klasör denetleyicisi sınıfları içerir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-189">This folder will contain the controller classes.</span></span> <span data-ttu-id="db6ad-190">Bir temel MVC uygulamasındaki denetleyicileri son kullanıcı etkileşimi işleme, model düzenleme ve sonuçta UI görünümü seçerek sorumludur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-190">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

        > [!NOTE]
        > <span data-ttu-id="db6ad-191">MVC çerçevesi ile sona erdirmek için tüm denetleyicilerinin adlarını gerektirir &quot;denetleyicisi&quot;-Örneğin, HomeController, LoginController veya ProductController.</span><span class="sxs-lookup"><span data-stu-id="db6ad-191">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
    2. <span data-ttu-id="db6ad-192">**Modelleri**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-192">**Models**.</span></span> <span data-ttu-id="db6ad-193">Bu klasör, MVC Web uygulaması için uygulama modeli temsil eden sınıflar için sağlanır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-193">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="db6ad-194">Bu genellikle, nesneleri ve veri deposu ile etkileşim için mantığı tanımlayan kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-194">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="db6ad-195">Tipik olarak gerçek model nesneleri ayrı sınıf kitaplıkları olacaktır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-195">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="db6ad-196">Ancak, yeni bir uygulama oluşturduğunuzda, sınıfları içerir ve ardından bunları ayrı sınıf kitaplıkları sonraki bir zamanda geliştirme döngüsü taşınıp.</span><span class="sxs-lookup"><span data-stu-id="db6ad-196">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
    3. <span data-ttu-id="db6ad-197">**Görünümleri**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-197">**Views**.</span></span> <span data-ttu-id="db6ad-198">Görünümler, uygulamanın kullanıcı arabirimini görüntülemeden sorumlu bileşenler için önerilen konum klasörüdür.</span><span class="sxs-lookup"><span data-stu-id="db6ad-198">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="db6ad-199">Görünümleri .aspx, .ascx, .cshtml ve .master dosyaları işleme görünümlerine ilgili diğer dosyaların yanı sıra kullanın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-199">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="db6ad-200">Görünümler klasöründe her denetleyici için bir klasör içerir; Bu klasör Denetleyici adı önekiyle adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-200">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="db6ad-201">Örneğin, adlandırılmış bir denetleyiciniz varsa **HomeController**, görünümler klasör giriş adlı bir klasör içerir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-201">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="db6ad-202">ASP.NET MVC çerçevesi bir görünüm yüklendiğinde varsayılan olarak, .aspx dosyası Views\controllerName klasöründe istenen görünüm adıyla arar (**görünümler [controllername öğesi] [eylem] .aspx**) veya (**görünümler [controllername öğesi] [Action] .cshtml**) Razor görünümleri için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-202">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-203">Daha önce listelenen klasörler ek olarak, bir MVC Web uygulamasını kullanan **Global.asax** Genel Yönlendirme URL'sini ayarlamak için dosya Varsayılanları ve kullandığı **Web.config** uygulamayı yapılandırmak için bir dosya.</span><span class="sxs-lookup"><span data-stu-id="db6ad-203">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="db6ad-204">Görev 3 - bir HomeController ekleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-204">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="db6ad-205">MVC çerçevesi kullanmayın ASP.NET uygulamalarında kullanıcı etkileşimi sayfaların çevresine ve oluşturma ve bu sayfaların etkinlikleri işleme etrafında düzenlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-205">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="db6ad-206">Buna karşılık, ASP.NET MVC uygulamaları ile kullanıcı etkileşimi denetleyicileri ve kendi eylem yöntemlerini düzenlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-206">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="db6ad-207">Diğer taraftan, ASP.NET MVC çerçevesi denetleyicileri olarak adlandırılır sınıfları URL'leri eşler.</span><span class="sxs-lookup"><span data-stu-id="db6ad-207">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="db6ad-208">Denetleyicileri gelen istekleri işleyen, kullanıcı girişi ve etkileşimleri işlemek, uygun uygulama mantığını yürütme ve istemciye geri gönderilecek yanıt belirlemek (HTML görüntülemek, dosya indirme, farklı bir URL vb. için yeniden yönlendirme).</span><span class="sxs-lookup"><span data-stu-id="db6ad-208">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="db6ad-209">HTML görüntüleme söz konusu olduğunda, denetleyici sınıfını genellikle bir istek için HTML biçimlendirmesi oluşturmak için ayrı görünümü bileşen çağırır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-209">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="db6ad-210">Bir MVC uygulamasında görünüm yalnızca bilgileri görüntüler; Denetleyici işler ve kullanıcı girişini ve etkileşimini yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-210">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="db6ad-211">Bu görevde URL'leri müzik deposu site giriş sayfasına işleyecek denetleyici sınıfını ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-211">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="db6ad-212">Sağ **denetleyicileri** Klasör Seç Çözüm Gezgini içinde **Ekle** ve ardından **denetleyicisi** komutu:</span><span class="sxs-lookup"><span data-stu-id="db6ad-212">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="db6ad-213">![Bir denetleyici komut ekleme](aspnet-mvc-4-fundamentals/_static/image5.png "denetleyicisi komut ekleme")</span><span class="sxs-lookup"><span data-stu-id="db6ad-213">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="db6ad-214">*Denetleyici komut ekleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-214">*Add Controller Command*</span></span>
2. <span data-ttu-id="db6ad-215">**Denetleyici Ekle** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-215">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="db6ad-216">Denetleyici adı *HomeController* ve basın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-216">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="db6ad-217">![Denetleyici Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image6.png "denetleyici Ekle iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="db6ad-217">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="db6ad-218">*Denetleyici Ekle iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="db6ad-218">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="db6ad-219">Dosya **HomeController.cs** oluşturulan **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="db6ad-219">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="db6ad-220">Olması için **HomeController** dizin eylemi üzerinde bir dize döndürecek, yerine **dizin** aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="db6ad-220">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="db6ad-221">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex1 HomeController dizin*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-221">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="db6ad-222">Görev 4 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="db6ad-222">Task 4 - Running the Application</span></span>

<span data-ttu-id="db6ad-223">Bu görevde, uygulama bir web tarayıcısında çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-223">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="db6ad-224">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-224">Press **F5** to run the Application.</span></span> <span data-ttu-id="db6ad-225">Proje derlenir ve yerel IIS Web sunucusu başlatır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-225">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="db6ad-226">Yerel IIS Web sunucusu otomatik olarak Web sunucusu URL'sine yönlendiren bir web tarayıcısı açılır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-226">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="db6ad-227">![Bir web tarayıcısında çalışan uygulama](aspnet-mvc-4-fundamentals/_static/image7.png "bir web tarayıcısında çalışan uygulama")</span><span class="sxs-lookup"><span data-stu-id="db6ad-227">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="db6ad-228">*Bir web tarayıcısında çalışan uygulama*</span><span class="sxs-lookup"><span data-stu-id="db6ad-228">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-229">Yerel IIS Web sunucusu Web sitesi bir rastgele serbest bağlantı noktası numarası üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-229">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="db6ad-230">Yukarıdaki şekilde, site çalışıyor `http://localhost:50103/`, bağlantı noktası 50103 kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="db6ad-230">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="db6ad-231">Bağlantı noktası numarası farklılık gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-231">Your port number may vary.</span></span>
2. <span data-ttu-id="db6ad-232">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-232">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="db6ad-233">Alıştırma 2: bir denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="db6ad-233">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="db6ad-234">Bu alıştırmada, basit müzik deposu uygulamanın işlevselliğini uygulamak için denetleyici güncelleştirmek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-234">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="db6ad-235">Bu denetleyici her aşağıdaki belirli isteklerini işlemek için eylem yöntemleri tanımlar:</span><span class="sxs-lookup"><span data-stu-id="db6ad-235">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="db6ad-236">Müzik deposundaki müzik türler listeleme sayfası</span><span class="sxs-lookup"><span data-stu-id="db6ad-236">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="db6ad-237">Belirli bir tarzını müzik albümlerini tümünün listeleyen bir Gözat sayfası</span><span class="sxs-lookup"><span data-stu-id="db6ad-237">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="db6ad-238">Belirli Müzik albüm hakkındaki bilgileri gösterir Ayrıntılar sayfası</span><span class="sxs-lookup"><span data-stu-id="db6ad-238">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="db6ad-239">Bu alıştırmada kapsam için bu eylemleri yalnızca artık bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="db6ad-239">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="db6ad-240">Görev 1 - yeni StoreController sınıf ekleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-240">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="db6ad-241">Bu görevde, yeni bir denetleyicisi ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-241">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="db6ad-242">Zaten açık değilse, başlangıç **VS Express Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-242">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="db6ad-243">İçinde **dosya** menüsünde seçin **Proje Aç**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-243">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="db6ad-244">Proje Aç iletişim kutusunda, Gözat **Source\Ex02 CreatingAController\Begin**seçin **Begin.sln** tıklatıp **açık**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-244">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="db6ad-245">Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-245">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="db6ad-246">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="db6ad-246">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="db6ad-247">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-247">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="db6ad-248">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-248">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="db6ad-249">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-249">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-250">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-250">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="db6ad-251">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-251">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="db6ad-252">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-252">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="db6ad-253">Yeni denetleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-253">Add the new controller.</span></span> <span data-ttu-id="db6ad-254">Bunu yapmak için sağ **denetleyicileri** Klasör Seç Çözüm Gezgini içinde **Ekle** ve ardından **denetleyicisi** komutu.</span><span class="sxs-lookup"><span data-stu-id="db6ad-254">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="db6ad-255">Değişiklik **Denetleyici adı** için *StoreController*, tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-255">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="db6ad-256">![Denetleyici Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image8.png "denetleyici Ekle iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="db6ad-256">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="db6ad-257">*Denetleyici Ekle iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="db6ad-257">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="db6ad-258">Görev 2 - StoreController'ın Eylemler değiştirme</span><span class="sxs-lookup"><span data-stu-id="db6ad-258">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="db6ad-259">Bu görevde denir denetleyici yöntemlerine değiştirecek **Eylemler**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-259">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="db6ad-260">Eylemler URL istekleri işleme ve tarayıcı veya URL çağrılan kullanıcıya geri gönderilmesi gereken içerik belirleme sorumludur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-260">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="db6ad-261">**StoreController** sınıf zaten bir **dizin** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-261">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="db6ad-262">Müzik deposu tüm türler listeler sayfası uygulamak için bu laboratuvarda daha sonra onu kullanır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-262">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="db6ad-263">Şu an için yalnızca Değiştir **dizin** bir dize döndürür aşağıdaki kod ile yöntemi &quot;Store.Index() gelen Hello&quot;:</span><span class="sxs-lookup"><span data-stu-id="db6ad-263">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="db6ad-264">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex2 StoreController dizin*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-264">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="db6ad-265">Ekleme **Gözat** ve **ayrıntıları** yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="db6ad-265">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="db6ad-266">Bunu yapmak için aşağıdaki kodu ekleyin **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="db6ad-266">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="db6ad-267">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-267">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="db6ad-268">Görev 3 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="db6ad-268">Task 3 - Running the Application</span></span>

<span data-ttu-id="db6ad-269">Bu görevde, uygulama bir web tarayıcısında çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-269">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="db6ad-270">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-270">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db6ad-271">Proje başlayacağını **giriş** sayfası.</span><span class="sxs-lookup"><span data-stu-id="db6ad-271">The project starts in the **Home** page.</span></span> <span data-ttu-id="db6ad-272">Her eylemin uygulama doğrulamak için URL'yi değiştirin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-272">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="db6ad-273">**/ Depolama**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-273">**/Store**.</span></span> <span data-ttu-id="db6ad-274">Göreceğiniz  **&quot;Store.Index() gelen Hello&quot;**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-274">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="db6ad-275">**/ Deposu/Gözat**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-275">**/Store/Browse**.</span></span> <span data-ttu-id="db6ad-276">Göreceğiniz  **&quot;Store.Browse() gelen Hello&quot;**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-276">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="db6ad-277">**/ Deposu/ayrıntıları**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-277">**/Store/Details**.</span></span> <span data-ttu-id="db6ad-278">Göreceğiniz  **&quot;Store.Details() gelen Hello&quot;**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-278">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="db6ad-279">![StoreBrowse gözatma](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse gözatma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-279">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="db6ad-280">*/Store/Browse gözatma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-280">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="db6ad-281">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-281">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="db6ad-282">Alıştırma 3: bir denetleyiciye parametreleri geçirme</span><span class="sxs-lookup"><span data-stu-id="db6ad-282">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="db6ad-283">Şimdiye kadar sabit dizeler denetleyicilerinden iade.</span><span class="sxs-lookup"><span data-stu-id="db6ad-283">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="db6ad-284">Bu alıştırmada URL ve sorgu dizesi kullanılarak ve tarayıcıya metinle yanıt yöntemi eylemler yapma denetleyicisi parametreleri geçirmek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-284">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="db6ad-285">Görev 1 - StoreController ekleme Tarz parametresi</span><span class="sxs-lookup"><span data-stu-id="db6ad-285">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="db6ad-286">Bu görevde, kullanacağınız **querystring** parametreleri göndermek için **Gözat** eylem yönteminde **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-286">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="db6ad-287">Zaten açık değilse, başlangıç **VS Express Web**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-287">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="db6ad-288">İçinde **dosya** menüsünde seçin **Proje Aç**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-288">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="db6ad-289">Proje Aç iletişim kutusunda, Gözat **Source\Ex03 PassingParametersToAController\Begin**seçin **Begin.sln** tıklatıp **açık**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-289">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="db6ad-290">Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-290">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="db6ad-291">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="db6ad-291">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="db6ad-292">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-292">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="db6ad-293">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-293">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="db6ad-294">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-294">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-295">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-295">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="db6ad-296">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-296">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="db6ad-297">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-297">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="db6ad-298">Açık **StoreController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-298">Open **StoreController** class.</span></span> <span data-ttu-id="db6ad-299">Bunu yapmak için **Çözüm Gezgini**, genişletin **denetleyicileri** klasörü ve çift **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-299">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="db6ad-300">Değişiklik **Gözat** için belirli bir tarzını istemek için bir dize parametresi ekleme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-300">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="db6ad-301">ASP.NET MVC otomatik olarak herhangi bir sorgu dizesi geçti veya adlandırılmış parametreleri form gönderme **Tarz** çağrıldığında Bu eylem yöntemine.</span><span class="sxs-lookup"><span data-stu-id="db6ad-301">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="db6ad-302">Bunu yapmak için yerini **Gözat** aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="db6ad-302">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="db6ad-303">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-303">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

    > [!NOTE]
    > <span data-ttu-id="db6ad-304">Kullanmakta olduğunuz **HttpUtility.HtmlEncode** yardımcı yöntemi, Javascript gibi bir bağlantıyla görünüme injecting gelen kullanıcılar önler   **/deposu/Gözat? Tarz =&lt;betik&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-304">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
    > 
    > <span data-ttu-id="db6ad-305">Daha fazla açıklama için lütfen şu adresi ziyaret [bu msdn makalesine](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="db6ad-305">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="db6ad-306">Görev 2 - uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="db6ad-306">Task 2 - Running the Application</span></span>

<span data-ttu-id="db6ad-307">Bu görevde, uygulama bir web tarayıcısında denemek ve kullanmanızı **Tarz** parametresi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-307">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="db6ad-308">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-308">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db6ad-309">Proje başlayacağını **giriş** sayfası.</span><span class="sxs-lookup"><span data-stu-id="db6ad-309">The project starts in the **Home** page.</span></span> <span data-ttu-id="db6ad-310">URL'ye değiştirin   */depolamak/Gözat? Tarz DISCO =* eylemi Tarz parametre aldığını doğrulamak için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-310">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="db6ad-311">![StoreBrowseGenre gözatma DISCO =](aspnet-mvc-4-fundamentals/_static/image10.png "StoreBrowseGenre gözatma DISCO =")</span><span class="sxs-lookup"><span data-stu-id="db6ad-311">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="db6ad-312">*Gözatma /Store/Browse? Tarz DISCO =*</span><span class="sxs-lookup"><span data-stu-id="db6ad-312">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="db6ad-313">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-313">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="db6ad-314">Görev 3 - URL'de katıştırılmış bir ID parametresi ekleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-314">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="db6ad-315">Bu görevde, kullanacağınız **URL** geçirmek için bir **kimliği** parametresi **ayrıntıları** eylem yöntemi **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-315">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="db6ad-316">Yönlendirme kuralı bir URL kesimi eylem yöntemi adından sonra adlı bir parametre işlemek için ASP.NET MVC'ın varsayılan **kimliği**. Eylem yöntemi kimliği adlı parametre varsa, daha sonra ASP.NET MVC otomatik olarak URL kesimi için parametre olarak geçer.</span><span class="sxs-lookup"><span data-stu-id="db6ad-316">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="db6ad-317">URL'deki **deposu/Ayrıntılar/5**, **kimliği** olarak yorumlanacak **5**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-317">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="db6ad-318">Değişiklik **ayrıntıları** yöntemi **StoreController**, ekleyerek bir **int** adlı bir parametreye **kimliği**. Bunu yapmak için yerini **ayrıntıları** aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="db6ad-318">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="db6ad-319">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-319">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="db6ad-320">Görev 4 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="db6ad-320">Task 4 - Running the Application</span></span>

<span data-ttu-id="db6ad-321">Bu görevde, uygulama bir web tarayıcısında denemek ve kullanmanızı **kimliği** parametresi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-321">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="db6ad-322">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-322">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db6ad-323">Proje başlayacağını **giriş** sayfası.</span><span class="sxs-lookup"><span data-stu-id="db6ad-323">The project starts in the **Home** page.</span></span> <span data-ttu-id="db6ad-324">URL'ye değiştirin */Store/Details/5* eylemi ID parametresi aldığını doğrulamak için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-324">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="db6ad-325">![StoreDetails5 gözatma](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 gözatma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-325">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="db6ad-326">*/Store/details/5 gözatma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-326">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="db6ad-327">Alıştırma 4: bir görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="db6ad-327">Exercise 4: Creating a View</span></span>

<span data-ttu-id="db6ad-328">Şu ana kadar dizeleri denetleyicisi eylemlerden iade.</span><span class="sxs-lookup"><span data-stu-id="db6ad-328">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="db6ad-329">Denetleyicileri nasıl çalıştığını anlamak yararlı bir yolu olsa da, değil gerçek Web uygulamalarınızın nasıl oluşturulur düşürür.</span><span class="sxs-lookup"><span data-stu-id="db6ad-329">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="db6ad-330">Şablon dosyalarını kullanarak tarayıcıya HTML oluşturmak için daha iyi bir yaklaşım sağlayan bileşenleri görünümlerdir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-330">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="db6ad-331">Bu alıştırmada, ortak HTML içerik, site ve son olarak HTML döndürülecek HomeController etkinleştirmek için bir şablonu görüntüleme Görünüm ve yapısını geliştirmek için bir stil sayfası için bir şablon kurulumu için bir düzen ana sayfa eklemek öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-331">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="db6ad-332">Görev 1 - dosyasını değiştirerek \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="db6ad-332">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="db6ad-333">Dosya **~/Views/Shared/\_layout.cshtml** tüm Web sitesi kullanmak üzere ortak HTML için bir şablon kurulum olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-333">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="db6ad-334">Bu görevde giriş sayfası ve depolama alanına bir düzen ana sayfa bağlantılar ile birlikte ortak bir üstbilgiyle ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-334">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="db6ad-335">Zaten açık değilse, başlangıç **VS Express Web**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-335">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="db6ad-336">İçinde **dosya** menüsünde seçin **Proje Aç**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-336">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="db6ad-337">Proje Aç iletişim kutusunda, Gözat **Source\Ex04 CreatingAView\Begin**seçin **Begin.sln** tıklatıp **açık**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-337">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="db6ad-338">Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-338">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="db6ad-339">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="db6ad-339">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="db6ad-340">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-340">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="db6ad-341">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-341">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="db6ad-342">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-342">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-343">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-343">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="db6ad-344">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-344">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="db6ad-345">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-345">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="db6ad-346">Dosya  **\_layout.cshtml** sitesindeki tüm sayfalara HTML kapsayıcı düzeni içeriyor.</span><span class="sxs-lookup"><span data-stu-id="db6ad-346">The file **\_layout.cshtml** contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="db6ad-347">İçerdiği  **&lt;html&gt;**  öğe için HTML yanıtını yanı sıra  **&lt;head&gt;**  ve  **&lt;gövde&gt;**  öğeleri.</span><span class="sxs-lookup"><span data-stu-id="db6ad-347">It includes the **&lt;html&gt;** element for the HTML response, as well as the **&lt;head&gt;** and **&lt;body&gt;** elements.</span></span> <span data-ttu-id="db6ad-348">**@RenderBody()** HTML içindeki gövde tanımlamak bölgeler şablonları kurulamayacak oturum dinamik içerik doldurmak bu görünümü.</span><span class="sxs-lookup"><span data-stu-id="db6ad-348">**@RenderBody()** within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
<span data-ttu-id="db6ad-349">(C#)</span><span class="sxs-lookup"><span data-stu-id="db6ad-349">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="db6ad-350">Giriş sayfası ve depolama alanı sitesindeki tüm sayfalara bağlantılar sahip ortak bir üstbilgi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-350">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="db6ad-351">Bunu yapmak için aşağıdaki aşağıdaki kodu ekleyin &lt;gövde&gt; deyimi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-351">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
<span data-ttu-id="db6ad-352">(C#)</span><span class="sxs-lookup"><span data-stu-id="db6ad-352">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="db6ad-353">Her sayfanın gövde bölümü oluşturmak için bir sayı içerir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-353">Include a div to render the body section of each page.</span></span> <span data-ttu-id="db6ad-354">Değiştir  **@RenderBody()** aşağıdaki higlighted kodla: (C#)</span><span class="sxs-lookup"><span data-stu-id="db6ad-354">Replace **@RenderBody()** with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="db6ad-355">Biliyor muydunuz?</span><span class="sxs-lookup"><span data-stu-id="db6ad-355">Did you know?</span></span> <span data-ttu-id="db6ad-356">Visual Studio 2012 yaygın olarak kullanılan kod HTML, kod dosyaları ve daha fazlasını eklemek kolaylaştıran parçacıkları var!</span><span class="sxs-lookup"><span data-stu-id="db6ad-356">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="db6ad-357">Out yazarak deneyin  **&lt;div&gt;**  tuşuna basarak **sekmesini** iki kez bir tam eklemek için **div** etiketi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-357">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="db6ad-358">Görev 2 - ekleme CSS stil sayfası</span><span class="sxs-lookup"><span data-stu-id="db6ad-358">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="db6ad-359">Boş proje şablonu yalnızca temel formlar ve doğrulama iletileri görüntülemek için kullanılan stiller içeren çok kolaylaştırılmış bir CSS dosyası içerir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-359">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="db6ad-360">Site Görünüm ve yapısını geliştirmek için ek CSS ve görüntüleri (büyük olasılıkla bir tasarımcı tarafından sağlanan) kullanır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-360">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="db6ad-361">Bu görevde, sitenin stillerini tanımlamak için bir CSS stil ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-361">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="db6ad-362">CSS dosyası ve görüntüler dahil edilen **Source\Assets\Content** bu laboratuvarı klasör.</span><span class="sxs-lookup"><span data-stu-id="db6ad-362">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="db6ad-363">Uygulama eklemek için bunların içerikten sürükleyin bir **Windows Explorer** penceresine **Çözüm Gezgini** aşağıda gösterildiği gibi Web için Visual Studio Express içinde:</span><span class="sxs-lookup"><span data-stu-id="db6ad-363">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="db6ad-364">![Stil içeriği sürükleyerek](aspnet-mvc-4-fundamentals/_static/image12.png "stili içeriği sürükleme")</span><span class="sxs-lookup"><span data-stu-id="db6ad-364">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="db6ad-365">*Stil içeriği sürükleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-365">*Dragging style contents*</span></span>
2. <span data-ttu-id="db6ad-366">Bir uyarı iletişim kutusu, değiştirdiğinizden onayı soran görünür **Site.css** dosyası ve var olan bazı görüntüler.</span><span class="sxs-lookup"><span data-stu-id="db6ad-366">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="db6ad-367">Denetleme **tüm öğeleri Uygula** tıklatıp **Evet**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-367">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="db6ad-368">Görev 3 - bir görünüm şablonu ekleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-368">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="db6ad-369">Bu görevde, Düzen ana sayfa kullanacağınız HTML yanıtı oluşturmak için bir şablonu görüntüleme ekleyecek ve bu alıştırmada CSS eklenir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-369">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="db6ad-370">Sitenin giriş sayfasına göz atarken görünüm şablonu kullanmak için önce bir dize döndüren yerine belirtmek gerekir **HomeController dizin** yöntemi döndürür bir **Görünüm**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-370">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="db6ad-371">Açık **HomeController** sınıfı ve değiştirmek kendi **dizin** döndürülecek yöntemi bir **ActionResult**, ve dönüş **View()**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-371">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="db6ad-372">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex4 HomeController dizin*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-372">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="db6ad-373">Şimdi, uygun bir görünüm şablon eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-373">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="db6ad-374">Bunu yapmak için **sağ** içinde **dizin** eylem yöntemi ve select **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-374">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="db6ad-375">Bu getirir **Görünüm Ekle** iletişim.</span><span class="sxs-lookup"><span data-stu-id="db6ad-375">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="db6ad-376">![Dizin yöntemi içinden bir görünümle ekleme](aspnet-mvc-4-fundamentals/_static/image13.png "dizin yöntemi içinden bir görünümle ekleme")</span><span class="sxs-lookup"><span data-stu-id="db6ad-376">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="db6ad-377">*Dizin yöntemi içinden bir görünümle ekleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-377">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="db6ad-378">**Görünüm Ekle** bir görünüm şablon dosyası oluşturmak için iletişim kutusu görünecektir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-378">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="db6ad-379">Varsayılan olarak, bu iletişim kutusu görünümü şablonunun adını böylece kullanacağı eylem yönteminin eşleşen önceden doldurur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-379">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="db6ad-380">Kullandığınız olduğundan **Görünüm Ekle** bağlam menüsü içinde **dizin** HomeController içinde eylem yönteminin **Görünüm Ekle** iletişim dizin varsayılan görünüm adı olarak sahip.</span><span class="sxs-lookup"><span data-stu-id="db6ad-380">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="db6ad-381">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-381">Click **Add**.</span></span>

    <span data-ttu-id="db6ad-382">![Görünüm Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image14.png "Görünüm Ekle iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="db6ad-382">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="db6ad-383">*Görünüm Ekle iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="db6ad-383">*Add View Dialog*</span></span>
4. <span data-ttu-id="db6ad-384">Visual Studio'nun oluşturduğu bir **Index.cshtml** görünüm şablonu içinde **görünümler** klasörünü ve ardından açar.</span><span class="sxs-lookup"><span data-stu-id="db6ad-384">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="db6ad-385">![Oluşturulan dizin görünüm ev](aspnet-mvc-4-fundamentals/_static/image15.png "oluşturulan giriş dizini görünüm")</span><span class="sxs-lookup"><span data-stu-id="db6ad-385">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="db6ad-386">*Oluşturulan giriş dizini görünüm*</span><span class="sxs-lookup"><span data-stu-id="db6ad-386">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-387">adını ve konumunu **Index.cshtml** dosya geçerlidir ve varsayılan ASP.NET MVC adlandırma kuralları izler.</span><span class="sxs-lookup"><span data-stu-id="db6ad-387">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="db6ad-388">Klasör \Views\**giriş** denetleyici adıyla eşleşen (**giriş** denetleyicisi).</span><span class="sxs-lookup"><span data-stu-id="db6ad-388">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="db6ad-389">Görünüm şablonu adı (**dizin**), görünüm görüntüleme denetleyici eylem yöntemine eşleşir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-389">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="db6ad-390">Bu şekilde, ASP.NET MVC açıkça bir görünüme dönmek için bu adlandırma kuralını kullanırken, bir görünüm şablonu konumu veya adı belirtin gereğini ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-390">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="db6ad-391">Oluşturulan görünüm şablonu dayanır  **\_layout.cshtml** daha önce tanımlanan şablon.</span><span class="sxs-lookup"><span data-stu-id="db6ad-391">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="db6ad-392">ViewBag.Title özelliğine güncelleştirme **giriş**, ana içeriği değiştirip **bu giriş sayfasıdır**, aşağıdaki kodda gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="db6ad-392">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="db6ad-393">Seçin **MvcMusicStore** basın ve Çözüm Gezgini proje **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-393">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="db6ad-394">Görev 4: doğrulama</span><span class="sxs-lookup"><span data-stu-id="db6ad-394">Task 4: Verification</span></span>

<span data-ttu-id="db6ad-395">Önceki alıştırmada tüm adımları doğru gerçekleştirmiş doğrulamak için aşağıdaki gibi ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="db6ad-395">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="db6ad-396">Bir tarayıcıda açıldığından uygulamayla unutmamalısınız:</span><span class="sxs-lookup"><span data-stu-id="db6ad-396">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="db6ad-397">Bulundu ve görüntülenen HomeController'ın dizin eylem yöntemi **\Views\Home\Index.cshtml** kodu denilen olsa bile şablonu görüntülemek **View() dönmek**, ardından şablonu görüntüleme standart adlandırma kuralı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-397">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="db6ad-398">Giriş sayfası içinde tanımlanan Hoş Geldiniz iletisi görüntüler **\Views\Home\Index.cshtml** şablonu görüntüle.</span><span class="sxs-lookup"><span data-stu-id="db6ad-398">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="db6ad-399">Giriş sayfasını kullanarak  **\_layout.cshtml** şablonu ve Hoş Geldiniz iletisi standart site HTML düzenini içinde yer alır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-399">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="db6ad-400">![Giriş dizini tanımlı LayoutPage ve stil kullanarak görünümü](aspnet-mvc-4-fundamentals/_static/image16.png "tanımlı LayoutPage ve stil kullanarak giriş dizini görünümü")</span><span class="sxs-lookup"><span data-stu-id="db6ad-400">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="db6ad-401">*Giriş dizini tanımlı LayoutPage ve stil kullanarak görünümü*</span><span class="sxs-lookup"><span data-stu-id="db6ad-401">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="db6ad-402">Alıştırma 5: görünüm Model oluşturma</span><span class="sxs-lookup"><span data-stu-id="db6ad-402">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="db6ad-403">Şu ana kadar sabit kodlanmış HTML görüntülemek görünümlerinizi yapılan ancak dinamik web uygulamaları oluşturmak için şablonu görüntüleme denetleyicisinden bilgi almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-403">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="db6ad-404">Bu amaç için kullanılacak bir ortak tekniktir **ViewModel** uygun HTML yanıtı oluşturmak için gereken tüm bilgileri paketini denetleyicinin sağlar düzeni.</span><span class="sxs-lookup"><span data-stu-id="db6ad-404">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="db6ad-405">Bu alıştırmada, ViewModel sınıfı oluşturmak ve gerekli özellikleri eklemek öğreneceksiniz: türler deposu ve bu tür listesi sayısı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-405">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="db6ad-406">Oluşturulan ViewModel kullanmak için StoreController güncelleştirmek ve son olarak, belirtilen özellikleri sayfasında görüntüler yeni bir görünüm şablonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-406">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="db6ad-407">Görev 1 - ViewModel sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="db6ad-407">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="db6ad-408">Bu görevde deposu Tarz listeleme senaryo gerçekleştireceksiniz ViewModel sınıfı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-408">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="db6ad-409">Zaten açık değilse, başlangıç **VS Express Web**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-409">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="db6ad-410">İçinde **dosya** menüsünde seçin **Proje Aç**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-410">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="db6ad-411">Proje Aç iletişim kutusunda, Gözat **Source\Ex05 CreatingAViewModel\Begin**seçin **Begin.sln** tıklatıp **açık**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-411">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="db6ad-412">Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-412">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="db6ad-413">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="db6ad-413">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="db6ad-414">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-414">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="db6ad-415">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-415">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="db6ad-416">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-416">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-417">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-417">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="db6ad-418">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-418">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="db6ad-419">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-419">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="db6ad-420">Oluşturma bir **ViewModels** ViewModel tutmak için klasör.</span><span class="sxs-lookup"><span data-stu-id="db6ad-420">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="db6ad-421">Bunu yapmak için üst düzey sağ **MvcMusicStore** proje, select **Ekle** ve ardından **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-421">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="db6ad-422">![Yeni bir klasör ekleme](aspnet-mvc-4-fundamentals/_static/image17.png "yeni bir klasör ekleme")</span><span class="sxs-lookup"><span data-stu-id="db6ad-422">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="db6ad-423">*Yeni bir klasör ekleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-423">*Adding a new folder*</span></span>
4. <span data-ttu-id="db6ad-424">Klasör adı *ViewModels*.</span><span class="sxs-lookup"><span data-stu-id="db6ad-424">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="db6ad-425">![Çözüm Gezgini'nde ViewModels klasörü](aspnet-mvc-4-fundamentals/_static/image18.png "Çözüm Gezgini'nde ViewModels klasörü")</span><span class="sxs-lookup"><span data-stu-id="db6ad-425">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="db6ad-426">*Çözüm Gezgini'nde ViewModels klasörü*</span><span class="sxs-lookup"><span data-stu-id="db6ad-426">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="db6ad-427">Oluşturma bir **ViewModel** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-427">Create a **ViewModel** class.</span></span> <span data-ttu-id="db6ad-428">Bunu yapmak için sağ **ViewModels** kısa süre önce oluşturuldu, klasörü seçin **Ekle** ve ardından **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-428">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="db6ad-429">Altında **kod**, seçin **sınıfı** öğe ve dosya adı *StoreIndexViewModel.cs*, ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-429">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="db6ad-430">![Yeni bir sınıf ekleme](aspnet-mvc-4-fundamentals/_static/image19.png "yeni sınıf ekleme")</span><span class="sxs-lookup"><span data-stu-id="db6ad-430">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="db6ad-431">*Yeni bir sınıf ekleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-431">*Adding a new Class*</span></span>

    <span data-ttu-id="db6ad-432">![StoreIndexViewModel sınıfı oluşturma](aspnet-mvc-4-fundamentals/_static/image20.png "oluşturma StoreIndexViewModel sınıfı")</span><span class="sxs-lookup"><span data-stu-id="db6ad-432">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="db6ad-433">*StoreIndexViewModel sınıfı oluşturma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-433">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="db6ad-434">Görev 2 - ViewModel sınıfı özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-434">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="db6ad-435">StoreController beklenen HTML yanıtı oluşturmak için şablonu görüntüleme iletilmek üzere iki parametre vardır: türler deposu ve bu tür listesi sayısı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-435">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="db6ad-436">Bu görevde, 2 Bu özellikler ekleyeceksiniz **StoreIndexViewModel** sınıfı: **NumberOfGenres** (tamsayı) ve **türler** (dizeler listesi).</span><span class="sxs-lookup"><span data-stu-id="db6ad-436">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="db6ad-437">Ekleme **NumberOfGenres** ve **türler** özelliklerine **StoreIndexViewModel** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-437">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="db6ad-438">Bunu yapmak için sınıf tanımına 2 aşağıdaki satırları ekleyin:</span><span class="sxs-lookup"><span data-stu-id="db6ad-438">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="db6ad-439">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex5 StoreIndexViewModel özellikleri*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-439">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

    > [!NOTE]
    > <span data-ttu-id="db6ad-440">**{Alın; ayarlayın;}**  gösterimi yapar C#, kullanıcının kullanan otomatik uygulanan özellikler özelliği.</span><span class="sxs-lookup"><span data-stu-id="db6ad-440">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="db6ad-441">Bir özelliğin avantajlarından bize yedekleme alanı bildirmek gerek kalmadan sağlar.</span><span class="sxs-lookup"><span data-stu-id="db6ad-441">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="db6ad-442">Görev 3 - güncelleştirme StoreController StoreIndexViewModel kullanmak için</span><span class="sxs-lookup"><span data-stu-id="db6ad-442">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="db6ad-443">**StoreIndexViewModel** sınıfı geçirmek için gereken bilgileri yalıtır **StoreController**'s **dizin** yanıt oluşturmak için bir şablonu görüntüleme yöntemi .</span><span class="sxs-lookup"><span data-stu-id="db6ad-443">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="db6ad-444">Bu görevde, güncelleştirecektir **StoreController** kullanmak için **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-444">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="db6ad-445">Açık **StoreController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-445">Open **StoreController** class.</span></span>

    <span data-ttu-id="db6ad-446">![StoreController sınıfı açma](aspnet-mvc-4-fundamentals/_static/image21.png "açılış StoreController sınıfı")</span><span class="sxs-lookup"><span data-stu-id="db6ad-446">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="db6ad-447">*Açılış StoreController sınıfı*</span><span class="sxs-lookup"><span data-stu-id="db6ad-447">*Opening StoreController class*</span></span>
2. <span data-ttu-id="db6ad-448">Kullanmak için **StoreIndexViewModel** sınıfıyla **StoreController**, şu ad alanının üstüne eklemeniz **StoreController** kod:</span><span class="sxs-lookup"><span data-stu-id="db6ad-448">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="db6ad-449">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex5 ViewModels kullanarak StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-449">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="db6ad-450">Değişiklik **StoreController**'s **dizin** şekilde oluşturur ve doldurur eylem yöntemi bir **StoreIndexViewModel** nesnesi ve ardından bunu bir görünüm şablonu geçirir devre dışı bir HTML yanıtını onunla oluşturur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-450">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-451">Laboratuarda &quot;ASP.NET MVC modelleri ve veri erişimi&quot; deposu türler listesini veritabanından alır kod yazacaksınız.</span><span class="sxs-lookup"><span data-stu-id="db6ad-451">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="db6ad-452">Aşağıdaki kodda oluşturacağınız bir **listesi** dolduracaktır kukla veri müzik **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-452">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="db6ad-453">Oluşturma ve ayarlama sonra **StoreIndexViewModel** nesnesi, onu geçirilecektir bağımsız değişken olarak **Görünüm** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-453">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="db6ad-454">Bu görünüm şablon söz konusu nesne sahip bir HTML yanıtı oluşturmak için kullanacağı gösterir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-454">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="db6ad-455">Değiştir **dizin** aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="db6ad-455">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="db6ad-456">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex5 StoreController dizin yöntemi*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-456">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

    > [!NOTE]
    > <span data-ttu-id="db6ad-457">C# ile tanınmayan, kullanmanın varsayabilir **var** anlamına **viewModel** değişkenidir geç bağlama.</span><span class="sxs-lookup"><span data-stu-id="db6ad-457">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="db6ad-458">C# Derleyici kullanarak tür çıkarımı değişkene atayın göre belirlemek için doğru değil - **viewModel** türü **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-458">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="db6ad-459">Ayrıca, yerel derleme tarafından **viewModel** değişken olarak bir **StoreIndexViewModel** derleme zamanı get denetimi ve Visual Studio kod düzenleyicisini desteği yazın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-459">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="db6ad-460">Görev 4 - StoreIndexViewModel kullanan bir görünüm şablonu oluşturma</span><span class="sxs-lookup"><span data-stu-id="db6ad-460">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="db6ad-461">Bu görevde denetleyicisinden geçirilen StoreIndexViewModel nesnesi türler listesini görüntülemek için kullanacağı bir görünüm şablonu oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="db6ad-461">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="db6ad-462">Yeni Görünüm şablonu oluşturmadan önce şimdi Projeyi derlemek için **Ekle iletişim kutusunu görüntüle** bildiği **StoreIndexViewModel** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-462">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="db6ad-463">Seçerek Projeyi derlemek **yapı** menü öğesini ve ardından **yapı MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-463">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="db6ad-464">![Proje derleme](aspnet-mvc-4-fundamentals/_static/image22.png "projesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-464">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="db6ad-465">*Proje derleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-465">*Building the project*</span></span>
2. <span data-ttu-id="db6ad-466">Yeni bir görünüm şablonu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="db6ad-466">Create a new View template.</span></span> <span data-ttu-id="db6ad-467">Bunu yapmak için sağ tıklatın içinde **dizin** yöntemi ve seçin **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-467">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="db6ad-468">![Bir görünümü ekleme](aspnet-mvc-4-fundamentals/_static/image23.png "bir görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="db6ad-468">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="db6ad-469">*Görünüm Ekleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-469">*Adding a View*</span></span>
3. <span data-ttu-id="db6ad-470">Çünkü **Ekle iletişim kutusunu görüntüle** gelen çağrıldı **StoreController**, varsayılan olarak şablonu görüntüleme ekleyecek bir **\Views\Store\Index.cshtml** dosya.</span><span class="sxs-lookup"><span data-stu-id="db6ad-470">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="db6ad-471">Denetleme **bir kesin türü belirtilmiş-görünüm oluşturma** onay kutusunu ve ardından **StoreIndexViewModel** olarak **Model sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-471">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="db6ad-472">Ayrıca, seçili görünüm altyapısı olduğundan emin olun **Razor**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-472">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="db6ad-473">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-473">Click **Add**.</span></span>

    <span data-ttu-id="db6ad-474">![Görünüm Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image24.png "Görünüm Ekle iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="db6ad-474">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="db6ad-475">*Görünüm Ekle iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="db6ad-475">*Add View Dialog*</span></span>

    <span data-ttu-id="db6ad-476">**\Views\Store\Index.cshtml** görünüm şablon dosyası oluşturulur ve açılır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-476">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="db6ad-477">Sağlanan bilgilere dayanarak **Görünüm Ekle** iletişim şablon beklediğiniz görünümü son adımda bir **StoreIndexViewModel** örneği bir HTML yanıtı oluşturmak için kullanılacak veri olarak.</span><span class="sxs-lookup"><span data-stu-id="db6ad-477">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="db6ad-478">Şablon devralması fark edeceksiniz bir `ViewPage<musicstore.viewmodels.storeindexviewmodel>` C#.</span><span class="sxs-lookup"><span data-stu-id="db6ad-478">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="db6ad-479">Görev 5 - görünüm şablonunu güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="db6ad-479">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="db6ad-480">Bu görevde, türler ve sayfa içinde adları sayısını almak üzere son görevde oluşturduğunuz görünüm şablonu güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-480">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="db6ad-481">@ Sözdizimini kullanır (genellikle olarak adlandırılan &quot;kod nuggets&quot;) görünüm şablonu içindeki kod yürütmek için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-481">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>


1. <span data-ttu-id="db6ad-482">İçinde **Index.cshtml** içinde dosya **deposu** klasör, kendisine kodu şununla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="db6ad-482">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>


    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="db6ad-483">Word süre yazarak biter bitmez **modeli**, Visual Studio'nun IntelliSense, olası özellikleri ve yöntemleri arasından seçim listesi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-483">As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.</span></span>
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > <span data-ttu-id="db6ad-484">*Model özellikleri ve yöntemleri Visual Studio'nun IntelliSense ile Başlarken*</span><span class="sxs-lookup"><span data-stu-id="db6ad-484">*Getting Model properties and methods with Visual Studio's IntelliSense*</span></span>
    > 
    > <span data-ttu-id="db6ad-485">**Modeli** özelliği başvuruları **StoreIndexViewModel** denetleyici görünüm şablona geçirilen nesne.</span><span class="sxs-lookup"><span data-stu-id="db6ad-485">The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template.</span></span> <span data-ttu-id="db6ad-486">Bu görünüm şablon denetleyicisinden geçirilen verilerin tümünü erişemeyeceği anlamına gelir. **modeli** özelliği ve görünüm şablonu içindeki uygun bir HTML yanıt içine biçimlendirin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-486">This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.</span></span>
    > 
    > <span data-ttu-id="db6ad-487">Yalnızca seçebilirsiniz **NumberOfGenres** IntelliSense özelliğinden liste içinde ve ardından yazarak otomatik tamamlama olacak yerine onu basarak **SEKME tuşuna**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-487">You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.</span></span>
2. <span data-ttu-id="db6ad-488">Döngü Tarz listesinde üzerinden **StoreIndexViewModel** ve bir HTML oluşturmak  **&lt;ul&gt;**  kullanarak listesinde bir **foreach** döngü.</span><span class="sxs-lookup"><span data-stu-id="db6ad-488">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
<span data-ttu-id="db6ad-489">(C#)</span><span class="sxs-lookup"><span data-stu-id="db6ad-489">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="db6ad-490">Tuşuna **F5** uygulamayı çalıştırın ve gözatmak için **/deposu**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-490">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="db6ad-491">Geçirilen türler listesini görürsünüz **StoreIndexViewModel** nesnesinin **StoreController** görünüm şablon.</span><span class="sxs-lookup"><span data-stu-id="db6ad-491">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="db6ad-492">![Türler listesini görüntüleyen görünümü](aspnet-mvc-4-fundamentals/_static/image26.png "türler listesini görüntüleyen görünümü")</span><span class="sxs-lookup"><span data-stu-id="db6ad-492">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="db6ad-493">*Türler listesini görüntüleyen görünümü*</span><span class="sxs-lookup"><span data-stu-id="db6ad-493">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="db6ad-494">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-494">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="db6ad-495">Alıştırma 6: Görünümünde parametrelerini kullanma</span><span class="sxs-lookup"><span data-stu-id="db6ad-495">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="db6ad-496">Alıştırma 3'te denetleyiciye parametreler öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-496">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="db6ad-497">Bu alıştırmada, görünüm şablonunda bu parametreleri kullanmayı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-497">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="db6ad-498">Bu amaç için önce veri ve etki alanı mantığı yönetmenize yardımcı olacak Model sınıflarına görülecektir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-498">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="db6ad-499">Ayrıca, kodlama URL yolu gibi şeyler endişelenmeden ASP.NET MVC uygulaması içindeki sayfalarına bağlantılar oluşturma öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-499">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="db6ad-500">Görev 1 - modeli sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-500">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="db6ad-501">Yalnızca bilgi denetleyicisinden görünüme iletmek için oluşturulan ViewModels modeli sınıfları içerir ve veri ve etki alanı mantığı yönetmek için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-501">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="db6ad-502">Bu görevde bu kavramları göstermek için iki modeli sınıfları ekleyeceksiniz: **Tarz** ve **albüm**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-502">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="db6ad-503">Zaten açık değilse, başlangıç **VS Express Web**</span><span class="sxs-lookup"><span data-stu-id="db6ad-503">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="db6ad-504">İçinde **dosya** menüsünde seçin **Proje Aç**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-504">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="db6ad-505">Proje Aç iletişim kutusunda, Gözat **Source\Ex06 UsingParametersInView\Begin**seçin **Begin.sln** tıklatıp **açık**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-505">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="db6ad-506">Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-506">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

    1. <span data-ttu-id="db6ad-507">Sağlanan açtıysanız **başlamak** çözümü gerekir bazı eksik NuGet paketlerini indirmek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="db6ad-507">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="db6ad-508">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-508">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="db6ad-509">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklatın **geri** eksik paketleri indirmesine için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-509">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="db6ad-510">Son olarak, tıklayarak çözümü derleme **yapı** | **yapı çözümü**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-510">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-511">NuGet kullanarak avantajlarından biri, projenizdeki tüm kitaplıkları dağıtmayı proje boyutunun azaltılması gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-511">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="db6ad-512">NuGet güç araçları ile Packages.config dosyasında paket sürümlerini belirterek, tüm gerekli kitaplıkları ilk kez proje çalıştırdığınızda indirebilirsiniz olacaktır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-512">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="db6ad-513">Varolan bir çözümü bu Laboratuvar açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-513">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="db6ad-514">Ekleme bir **Tarz** Model sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-514">Add a **Genre** Model class.</span></span> <span data-ttu-id="db6ad-515">Bunu yapmak için sağ **modelleri** klasöründe **Çözüm Gezgini**seçin **Ekle** ve ardından **yeni öğe** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="db6ad-515">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="db6ad-516">Altında **kod**, seçin **sınıfı** öğe ve dosya adı *Genre.cs*, ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-516">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="db6ad-517">![Sınıf ekleme](aspnet-mvc-4-fundamentals/_static/image27.png "sınıf ekleme")</span><span class="sxs-lookup"><span data-stu-id="db6ad-517">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="db6ad-518">*Yeni bir öğe ekleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-518">*Adding a new item*</span></span>

    <span data-ttu-id="db6ad-519">![Tarz Model sınıfı ekleme](aspnet-mvc-4-fundamentals/_static/image28.png "Tarz Model sınıfı ekleme")</span><span class="sxs-lookup"><span data-stu-id="db6ad-519">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="db6ad-520">*Tarz Model sınıfı ekleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-520">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="db6ad-521">Ekleme bir **adı** Tarz sınıf özelliğine.</span><span class="sxs-lookup"><span data-stu-id="db6ad-521">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="db6ad-522">Bunu yapmak için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="db6ad-522">To do this, add the following code:</span></span>

    <span data-ttu-id="db6ad-523">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 Tarz*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-523">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="db6ad-524">Yordamın aynısını önce aşağıdaki eklemek bir **albüm** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-524">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="db6ad-525">Bunu yapmak için sağ **modelleri** klasöründe **Çözüm Gezgini**seçin **Ekle** ve ardından **yeni öğe** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="db6ad-525">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="db6ad-526">Altında **kod**, seçin **sınıfı** öğe ve dosya adı *Album.cs*, ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-526">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="db6ad-527">İki özellik albüm sınıfına ekleyin: **Tarz** ve **başlık**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-527">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="db6ad-528">Bunu yapmak için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="db6ad-528">To do this, add the following code:</span></span>

    <span data-ttu-id="db6ad-529">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 albüm*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-529">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="db6ad-530">Görev 2 - bir StoreBrowseViewModel ekleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-530">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="db6ad-531">A **StoreBrowseViewModel** bu görevin seçilen bir tarzını eşleşen albümleri göstermek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-531">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="db6ad-532">Bu görevde, bu sınıf oluşturacak ve işlemek için iki özellik eklemek **Tarz** ve kendi **albüm**kullanıcının listeleyin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-532">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="db6ad-533">Ekleme bir **StoreBrowseViewModel** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-533">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="db6ad-534">Bunu yapmak için sağ **ViewModels** klasöründe **Çözüm Gezgini**seçin **Ekle** ve ardından **yeni öğe** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="db6ad-534">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="db6ad-535">Altında **kod**, seçin **sınıfı** öğe ve dosya adı *StoreBrowseViewModel.cs*, ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-535">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="db6ad-536">Modeller için bir başvuru ekleyin **StoreBrowseViewModel** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-536">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="db6ad-537">Bunu yapmak için aşağıdakileri ekleyin. ad alanını kullanarak:</span><span class="sxs-lookup"><span data-stu-id="db6ad-537">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="db6ad-538">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="db6ad-539">İki özellikleri **StoreBrowseViewModel** sınıfı: **Tarz** ve **albümleri**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-539">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="db6ad-540">Bunu yapmak için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="db6ad-540">To do this, add the following code:</span></span>

    <span data-ttu-id="db6ad-541">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-541">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

    > [!NOTE]
    > <span data-ttu-id="db6ad-542">Nedir **listesi&lt;albüm&gt;**  ?: Bu tanımı kullanarak **listesi&lt;T&gt;**  yazın, burada **T** kısıtlar hangi öğelerin bu türe **listesi** , bu durumda ait **albüm** (veya alt öğelerinden birini).</span><span class="sxs-lookup"><span data-stu-id="db6ad-542">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
    > 
    > <span data-ttu-id="db6ad-543">Sınıflar ve sınıf veya yöntemin bildirilir ve istemci kodu tarafından örneği C# dili özelliğidir kadar bir veya daha fazla türü belirtimini erteleneceği yöntemler tasarlamak için bu özelliği adlı **genel türler**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-543">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
    > 
    > <span data-ttu-id="db6ad-544">**Liste&lt;T&gt;**  genel eşdeğerdir **ArrayList** yazın ve kullanılabilir **System.Collections.Generic** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-544">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="db6ad-545">Kullanmanın avantajlarından biri **genel türler** türü belirtilmiş olduğundan, ilgilenebilmek denetimi elemanlara atama gibi işlemleri türü gerek olmayan **albüm** bir ileyaptığınızgibi**ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-545">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="db6ad-546">Görev 3 - yeni ViewModel StoreController kullanma</span><span class="sxs-lookup"><span data-stu-id="db6ad-546">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="db6ad-547">Bu görevde değiştirecek **StoreController**'s **Gözat** ve **ayrıntıları** yeni kullanmak için eylem yöntemleri **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="db6ad-547">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="db6ad-548">Modeller klasörü için bir başvuru ekleyin **StoreController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-548">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="db6ad-549">Bunu yapmak için sırasıyla **denetleyicileri** klasöründe **Çözüm Gezgini** açarak **StoreController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-549">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="db6ad-550">Daha sonra aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="db6ad-550">Then add the following code:</span></span>

    <span data-ttu-id="db6ad-551">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-551">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="db6ad-552">Değiştir **Gözat** eylem yönteminin kullanılacağını **StoreViewBrowseController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-552">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="db6ad-553">Sahte verilerle bir tarzını ve iki yeni Albümler nesneler oluşturur (sonraki uygulamalı laboratuar ortamında, bir veritabanından gerçek veri kullanır).</span><span class="sxs-lookup"><span data-stu-id="db6ad-553">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="db6ad-554">Bunu yapmak için yerini **Gözat** aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="db6ad-554">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="db6ad-555">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-555">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="db6ad-556">Değiştir **ayrıntıları** eylem yönteminin kullanılacağını **StoreViewBrowseController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-556">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="db6ad-557">Yeni oluşturduğunuz **albüm** için döndürülecek nesne **Görünüm**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-557">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="db6ad-558">Bunu yapmak için yerini **ayrıntıları** aşağıdaki kod ile yöntemi:</span><span class="sxs-lookup"><span data-stu-id="db6ad-558">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="db6ad-559">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="db6ad-559">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="db6ad-560">Görev 4 - Gözat görünüm şablonu ekleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-560">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="db6ad-561">Bu görevde, ekleyeceksiniz bir **Gözat** için belirli bir tarzını bulunan albümleri göstermek için Görünüm.</span><span class="sxs-lookup"><span data-stu-id="db6ad-561">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="db6ad-562">Yeni Görünüm şablonu oluşturmadan önce projeyi derlemek böylece **Görünüm Ekle** iletişim bilir hakkında **ViewModel** kullanılacak sınıfı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-562">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="db6ad-563">Seçerek Projeyi derlemek **yapı** menü öğesini ve ardından **yapı MvcMusicStore**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-563">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="db6ad-564">Ekleme bir **Gözat** görünümü.</span><span class="sxs-lookup"><span data-stu-id="db6ad-564">Add a **Browse** View.</span></span> <span data-ttu-id="db6ad-565">Bunu yapmak için sağ **Gözat** eylem yöntemi **StoreController** tıklatıp **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-565">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="db6ad-566">İçinde **Görünüm Ekle** iletişim kutusunda, Görünüm adı olduğundan emin olun **Gözat**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-566">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="db6ad-567">Denetleme **kesin türü belirtilmiş görünüm oluşturmak** onay kutusunu seçip **StoreBrowseViewModel** gelen **Model sınıfı** açılır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-567">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="db6ad-568">Diğer alanları varsayılan değerlerine bırakın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-568">Leave the other fields with their default value.</span></span> <span data-ttu-id="db6ad-569">Ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-569">Then click **Add**.</span></span>

    <span data-ttu-id="db6ad-570">![Gözat görünümü ekleme](aspnet-mvc-4-fundamentals/_static/image29.png "Gözat görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="db6ad-570">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="db6ad-571">*Gözat görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-571">*Adding a Browse View*</span></span>
4. <span data-ttu-id="db6ad-572">Değiştirme **Browse.cshtml** Tarz'ın bilgilerini görüntülemek için erişim **StoreBrowseViewModel** şablonu görüntüleme geçirilen nesne.</span><span class="sxs-lookup"><span data-stu-id="db6ad-572">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="db6ad-573">Bunu yapmak için içerik şununla değiştirin: (C#)</span><span class="sxs-lookup"><span data-stu-id="db6ad-573">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="db6ad-574">Görev 5 - Uygulama çalışıyor</span><span class="sxs-lookup"><span data-stu-id="db6ad-574">Task 5 - Running the Application</span></span>

<span data-ttu-id="db6ad-575">Bu görevde, test **Gözat** yöntemi alır albümleri gelen **Gözat** yöntemi eylem.</span><span class="sxs-lookup"><span data-stu-id="db6ad-575">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="db6ad-576">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-576">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db6ad-577">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-577">The project starts in the Home page.</span></span> <span data-ttu-id="db6ad-578">URL'ye değiştirin   **/depolamak/Gözat? Tarz DISCO =** eylemi iki albümleri döndürür doğrulanamadı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-578">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="db6ad-579">![Mağaza DISCO albümleri gözatma](aspnet-mvc-4-fundamentals/_static/image30.png "deposu DISCO albümleri gözatma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-579">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="db6ad-580">*Mağaza DISCO albümleri gözatma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-580">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="db6ad-581">Görev 6 - bilgi hakkında belirli bir albümü görüntüleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-581">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="db6ad-582">Bu görevde, gerçekleştireceksiniz **deposu/ayrıntıları** belirli bir albümü hakkında bilgi görüntülemek için görünümü.</span><span class="sxs-lookup"><span data-stu-id="db6ad-582">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="db6ad-583">Bu uygulamalı laboratuar ortamında her şeyi hakkında albüm görüntülenir zaten bulunan **Görünüm** şablonu.</span><span class="sxs-lookup"><span data-stu-id="db6ad-583">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="db6ad-584">Bunu oluşturmak yerine bir **StoreDetailsViewModel** sınıfı, geçerli kullanacağınız **StoreBrowseViewModel** albümü kendisine geçirme şablonu.</span><span class="sxs-lookup"><span data-stu-id="db6ad-584">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="db6ad-585">Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-585">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="db6ad-586">Yeni bir ekleme **ayrıntıları** görüntüleme **StoreController**'s **ayrıntıları** eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-586">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="db6ad-587">Bunu yapmak için sağ tıklatın **ayrıntıları** yönteminde **StoreController** sınıfı ve tıklayın **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-587">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="db6ad-588">İçinde **Görünüm Ekle** iletişim kutusunda, doğrulayın **Görünüm adı** olan **ayrıntıları**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-588">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="db6ad-589">Denetleyin **kesin türü belirtilmiş görünüm oluşturma** onay kutusunu seçip **albüm** gelen **Model sınıfı** açılır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-589">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="db6ad-590">Diğer alanları varsayılan değerlerine bırakın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-590">Leave the other fields with their default value.</span></span> <span data-ttu-id="db6ad-591">Ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-591">Then click **Add**.</span></span> <span data-ttu-id="db6ad-592">Bu oluşturun ve açık bir **\Views\Store\Details.cshtml** dosya.</span><span class="sxs-lookup"><span data-stu-id="db6ad-592">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="db6ad-593">![Ayrıntılar görünümü ekleme](aspnet-mvc-4-fundamentals/_static/image31.png "Ayrıntılar görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="db6ad-593">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="db6ad-594">*Ayrıntılar görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-594">*Adding a Details View*</span></span>
3. <span data-ttu-id="db6ad-595">Değiştirme **Details.cshtml** albüm bilgileri görüntülemek için dosya erişme **albüm** şablonu görüntüleme geçirilen nesne.</span><span class="sxs-lookup"><span data-stu-id="db6ad-595">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="db6ad-596">Bunu yapmak için içerik şununla değiştirin: (C#)</span><span class="sxs-lookup"><span data-stu-id="db6ad-596">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="db6ad-597">7 - uygulama çalışan görev</span><span class="sxs-lookup"><span data-stu-id="db6ad-597">Task 7 - Running the Application</span></span>

<span data-ttu-id="db6ad-598">Bu görevde, test **ayrıntıları** görünümü alır albüm bilgilerinden **ayrıntıları eylem** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-598">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="db6ad-599">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-599">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db6ad-600">Proje başlayacağını **giriş** sayfası.</span><span class="sxs-lookup"><span data-stu-id="db6ad-600">The project starts in the **Home** page.</span></span> <span data-ttu-id="db6ad-601">URL'ye değiştirin **/Store/Details/5** albüm bilgilerini doğrulamak için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-601">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="db6ad-602">![Albümleri ayrıntı gözatma](aspnet-mvc-4-fundamentals/_static/image32.png "albümleri ayrıntı gözatma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-602">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="db6ad-603">*Albüm ayrıntı gözatma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-603">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="db6ad-604">Görev 8 - sayfaları arasında bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-604">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="db6ad-605">Bu görevde her Tarz adı uygun bir bağlantı sağlamak için depolama görünümünde bir bağlantı ekleyeceksiniz **/deposu/Gözat** URL.</span><span class="sxs-lookup"><span data-stu-id="db6ad-605">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="db6ad-606">Bu şekilde, bir tarzını üzerinde örneği için tıklattığınızda **DISCO**, gider **/deposu/Gözat? Tarz DISCO =** URL.</span><span class="sxs-lookup"><span data-stu-id="db6ad-606">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="db6ad-607">Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-607">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="db6ad-608">Güncelleştirme **dizin** bağlantısı eklemek için sayfa **Gözat** sayfası.</span><span class="sxs-lookup"><span data-stu-id="db6ad-608">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="db6ad-609">Bunu yapmak için **Çözüm Gezgini** genişletin **görünümleri** klasörü, sonra **deposu** klasörü ve çift **Index.cshtml** sayfası.</span><span class="sxs-lookup"><span data-stu-id="db6ad-609">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="db6ad-610">Bir bağlantı seçili Tarz belirten Gözat görünümüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-610">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="db6ad-611">Bunu yapmak için aşağıdaki vurgulanmış kodu içinde yerini  **&lt;li&gt;**  etiketler: (C#)</span><span class="sxs-lookup"><span data-stu-id="db6ad-611">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="db6ad-612">başka bir yaklaşım, aşağıdaki gibi bir kodla, doğrudan sayfasına bağlantılandırma:</span><span class="sxs-lookup"><span data-stu-id="db6ad-612">another approach would be linking directly to the page, with a code like the following:</span></span>
    > 
    > <span data-ttu-id="db6ad-613">&lt;bir href =&quot;/deposu/Gözat? Tarz =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="db6ad-613">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
    > 
    > <span data-ttu-id="db6ad-614">Bu yaklaşım kullanılabilse de, bir sabit kodlanmış dize bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-614">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="db6ad-615">Daha sonra denetleyicisi yeniden adlandırırsanız, bu yönerge el ile değiştirmeniz gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-615">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="db6ad-616">Daha iyi bir alternatif kullanmaktır bir **HTML Yardımcısı** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-616">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="db6ad-617">ASP.NET MVC gibi görevler için kullanılabilir olan bir HTML yardımcı yöntemi içerir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-617">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="db6ad-618">**Html.ActionLink()** yardımcı yöntem HTML oluşturmanızı kolaylaştırır  **&lt;bir&gt;**  bağlantılar, URL yollarını URL kodlanmış düzgünce emin olun.</span><span class="sxs-lookup"><span data-stu-id="db6ad-618">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
    > 
    > <span data-ttu-id="db6ad-619">Htlm.ActionLink birçok aşırı yüklemeye sahip.</span><span class="sxs-lookup"><span data-stu-id="db6ad-619">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="db6ad-620">Bu alıştırmada üç parametre alır birini kullanır:</span><span class="sxs-lookup"><span data-stu-id="db6ad-620">In this exercise you will use one that takes three parameters:</span></span>
    > 
    > 1. <span data-ttu-id="db6ad-621">Bağlantı metnini, tarz adını görüntüler</span><span class="sxs-lookup"><span data-stu-id="db6ad-621">Link text, which will display the Genre name</span></span>
    > 2. <span data-ttu-id="db6ad-622">Denetleyici eylem adı (**Gözat**)</span><span class="sxs-lookup"><span data-stu-id="db6ad-622">Controller action name (**Browse**)</span></span>
    > 3. <span data-ttu-id="db6ad-623">Rota parametre değerleri, hem adı belirtme (**Tarz**) ve değeri (**Tarz adı**)</span><span class="sxs-lookup"><span data-stu-id="db6ad-623">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="db6ad-624">9 - uygulama çalışan görev</span><span class="sxs-lookup"><span data-stu-id="db6ad-624">Task 9 - Running the Application</span></span>

<span data-ttu-id="db6ad-625">Bu görevde her bir tarzını uygun bir bağlantı görüntülenir sınayacak **/deposu/Gözat** URL.</span><span class="sxs-lookup"><span data-stu-id="db6ad-625">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="db6ad-626">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-626">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db6ad-627">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-627">The project starts in the Home page.</span></span> <span data-ttu-id="db6ad-628">URL'ye değiştirin **/deposu** her bir tarzını uygun bağlantıları doğrulamak için **/deposu/Gözat** URL.</span><span class="sxs-lookup"><span data-stu-id="db6ad-628">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="db6ad-629">![Türler Gözat sayfasına bağlantı gözatma](aspnet-mvc-4-fundamentals/_static/image33.png "Gözat sayfasına bağlantılarla gözatma türler")</span><span class="sxs-lookup"><span data-stu-id="db6ad-629">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="db6ad-630">*Türler Gözat sayfasına bağlantı gözatma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-630">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="db6ad-631">10 - değerleri geçirmek için dinamik ViewModel koleksiyonu kullanarak görev</span><span class="sxs-lookup"><span data-stu-id="db6ad-631">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="db6ad-632">Bu görevde, modelde değişiklik yapmadan değerleri denetleyici ve görünüm arasında geçirmek için basit ve güçlü bir yöntemdir öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-632">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="db6ad-633">ASP.NET MVC 4 sağlar koleksiyon &quot;ViewModel&quot;, hangi herhangi bir dinamik değer atanabilir ve denetleyicileri ve görünümleri de içinde erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-633">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="db6ad-634">Şimdi ViewBag dinamik koleksiyon listesi geçirmek için kullanacağınız &quot; **Starred türler** &quot; görünümüne denetleyicisinden.</span><span class="sxs-lookup"><span data-stu-id="db6ad-634">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="db6ad-635">Depolama dizini görünümü için erişecek **ViewModel** ve bilgileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="db6ad-635">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="db6ad-636">Gerekirse Visual Studio penceresine geri dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-636">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="db6ad-637">Açık **StoreController.cs** ve değiştirme **dizin** ViewModel koleksiyona listesini oluşturmak için yöntem starred türler:</span><span class="sxs-lookup"><span data-stu-id="db6ad-637">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>


    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="db6ad-638">Sözdizimi da kullanabilirsiniz **ViewBag [&quot;Starred&quot;]** özelliklerine erişmek için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-638">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="db6ad-639">Yıldız simgesine  **&quot;starred.png&quot;**  dahil **Source\Assets\Images** bu laboratuvarı klasör.</span><span class="sxs-lookup"><span data-stu-id="db6ad-639">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="db6ad-640">Uygulama eklemek için bunların içerikten sürükleyin bir **Windows Explorer** penceresine **Çözüm Gezgini** Express'te Visual Web Developer aşağıda gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="db6ad-640">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="db6ad-641">![Çözüme yıldız görüntü ekleme](aspnet-mvc-4-fundamentals/_static/image34.png "çözüme yıldız görüntü ekleme")</span><span class="sxs-lookup"><span data-stu-id="db6ad-641">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="db6ad-642">*Çözüme yıldız görüntü ekleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-642">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="db6ad-643">Görünümü açma **Store/Index.cshtml** ve içeriği değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-643">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="db6ad-644">Okuma yapacak &quot;starred&quot; özelliğinde **ViewBag** koleksiyonu ve geçerli bir tarzını adı listede olup olmadığını isteyin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-644">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="db6ad-645">Bu durumda Tarz bağlantısına sağ yıldız simgesiyle gösterilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-645">In that case you will show a star icon right to the genre link.</span></span>
<span data-ttu-id="db6ad-646">(C#)</span><span class="sxs-lookup"><span data-stu-id="db6ad-646">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="db6ad-647">11 - uygulama çalışan görev</span><span class="sxs-lookup"><span data-stu-id="db6ad-647">Task 11 - Running the Application</span></span>

<span data-ttu-id="db6ad-648">Bu görevde yıldızlanmıştır türler yıldız simgesiyle görüntülemek test edeceğiz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-648">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="db6ad-649">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-649">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="db6ad-650">Proje başlayacağını **giriş** sayfası.</span><span class="sxs-lookup"><span data-stu-id="db6ad-650">The project starts in the **Home** page.</span></span> <span data-ttu-id="db6ad-651">URL'ye değiştirin **/deposu** her öne çıkan bir tarzını saygı etiket olduğunu doğrulayın:</span><span class="sxs-lookup"><span data-stu-id="db6ad-651">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="db6ad-652">![Türler yıldızlanmıştır öğeleriyle gözatma](aspnet-mvc-4-fundamentals/_static/image35.png "yıldızlanmıştır öğelerle gözatma türler")</span><span class="sxs-lookup"><span data-stu-id="db6ad-652">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="db6ad-653">*Yıldızlanmıştır öğelerle gözatma türler*</span><span class="sxs-lookup"><span data-stu-id="db6ad-653">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="db6ad-654">Alıştırma 7: ASP.NET MVC 4 yeni şablonu çevresinde bir diz</span><span class="sxs-lookup"><span data-stu-id="db6ad-654">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="db6ad-655">Bu alıştırmada, ASP.NET MVC 4 proje şablonları geliştirmeleri göz en ilgili özellikleri yeni şablon alma inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-655">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="db6ad-656">Görev 1: ASP.NET MVC 4 Internet uygulama şablonu keşfetme</span><span class="sxs-lookup"><span data-stu-id="db6ad-656">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="db6ad-657">Zaten açık değilse, başlangıç **VS Express Web**</span><span class="sxs-lookup"><span data-stu-id="db6ad-657">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="db6ad-658">Seçin **dosya | Yeni | Proje** menü komutu.</span><span class="sxs-lookup"><span data-stu-id="db6ad-658">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="db6ad-659">İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** şablonu sol bölmesindeki ağaç ve seçin **ASP.NET MVC 4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-659">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="db6ad-660">**Ad** proje *MusicStore* ve güncelleştirme **çözüm adı** için *başlamak*, ardından bir konum seçin (veya varsayılan adı bırakın) tıklatıp**Tamam**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-660">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="db6ad-661">![Yeni bir ASP.NET MVC 4 proje oluşturma](aspnet-mvc-4-fundamentals/_static/image36.png "yeni bir ASP.NET MVC 4 proje oluşturma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-661">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="db6ad-662">*Yeni bir ASP.NET MVC 4 proje oluşturma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-662">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="db6ad-663">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulama** proje şablonu ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-663">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="db6ad-664">Görünüm altyapısı olarak Razor veya ASPX seçebilirsiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-664">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="db6ad-665">![Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma](aspnet-mvc-4-fundamentals/_static/image37.png "yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-665">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="db6ad-666">*Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-666">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-667">Razor sözdizimi ASP.NET MVC 3'te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-667">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="db6ad-668">Karakterler ve gerekli bir dosya bir hızlı ve iş akışı kodlama sıvı etkinleştirme tuş vuruşları sayısını en aza indirmek için kendi hedeftir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-668">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="db6ad-669">Razor yararlanır varolan C# /VB (veya diğer) dil becerileri ve harika bir HTML oluşturma iş akışı sağlayan bir şablon biçimlendirme sözdizimi sunar.</span><span class="sxs-lookup"><span data-stu-id="db6ad-669">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="db6ad-670">Tuşuna **F5** çözümü çalıştırın ve yenilenen şablonu görmek için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-670">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="db6ad-671">Aşağıdaki özellikleri denetleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="db6ad-671">You can check out the following features:</span></span>

    1. <span data-ttu-id="db6ad-672">**Modern stili şablonları**</span><span class="sxs-lookup"><span data-stu-id="db6ad-672">**Modern-style templates**</span></span>

        <span data-ttu-id="db6ad-673">Şablonlar, daha fazla modern görünümlü stilleri sağlama yenilenmiş.</span><span class="sxs-lookup"><span data-stu-id="db6ad-673">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="db6ad-674">![ASP.NET MVC 4 stili şablonları](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC şablonları Stili 4")</span><span class="sxs-lookup"><span data-stu-id="db6ad-674">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="db6ad-675">*ASP.NET MVC 4 stili şablonları*</span><span class="sxs-lookup"><span data-stu-id="db6ad-675">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="db6ad-676">**Uyarlamalı işleme**</span><span class="sxs-lookup"><span data-stu-id="db6ad-676">**Adaptive Rendering**</span></span>

        <span data-ttu-id="db6ad-677">Tarayıcı penceresini yeniden boyutlandırma çıkışı denetleyin ve nasıl sayfa düzeni yeni pencere boyutunu dinamik olarak uyum dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-677">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="db6ad-678">Bu şablonlar, hem Masaüstü hem de mobil platformları hiçbir özelleştirme gerektirmeden düzgün işlenecek Uyarlamalı işleme tekniği kullanın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-678">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="db6ad-679">![ASP.NET MVC 4 proje şablonu farklı tarayıcı boyutlarda](aspnet-mvc-4-fundamentals/_static/image39.png "farklı tarayıcı boyutlarda ASP.NET MVC 4 proje şablonu")</span><span class="sxs-lookup"><span data-stu-id="db6ad-679">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="db6ad-680">*ASP.NET MVC 4 proje şablonu farklı tarayıcı boyutlarda*</span><span class="sxs-lookup"><span data-stu-id="db6ad-680">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="db6ad-681">Hata ayıklayıcıyı durdurduktan ve Visual Studio'ya dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-681">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="db6ad-682">Şimdi çözümü keşfedin ve ASP.NET MVC 4 proje şablonu içinde sunulan yeni özelliklerden bazıları göz atın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-682">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="db6ad-683">![ASP.NET MVC4-Internet-uygulama-proje-şablonu](aspnet-mvc-4-fundamentals/_static/image40.png "ASP.NET MVC 4 Internet uygulaması proje şablonu")</span><span class="sxs-lookup"><span data-stu-id="db6ad-683">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="db6ad-684">*ASP.NET MVC 4 Internet uygulaması proje şablonu*</span><span class="sxs-lookup"><span data-stu-id="db6ad-684">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

    1. <span data-ttu-id="db6ad-685">**HTML5 biçimlendirme**</span><span class="sxs-lookup"><span data-stu-id="db6ad-685">**HTML5 markup**</span></span>

        <span data-ttu-id="db6ad-686">Şablon görünümleri yeni temayı işaretleme, örneğin açık bulmak için Gözat **About.cshtml** görünümü **giriş** klasör.</span><span class="sxs-lookup"><span data-stu-id="db6ad-686">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

        <span data-ttu-id="db6ad-687">![Razor ve HTML5 biçimlendirme kullanarak yeni şablonu](aspnet-mvc-4-fundamentals/_static/image41.png "Razor ve HTML5 biçimlendirme kullanarak yeni şablonu")</span><span class="sxs-lookup"><span data-stu-id="db6ad-687">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

        <span data-ttu-id="db6ad-688">*Razor ve HTML5 biçimlendirme kullanarak yeni şablonu*</span><span class="sxs-lookup"><span data-stu-id="db6ad-688">*New template, using Razor and HTML5 markup*</span></span>
    2. <span data-ttu-id="db6ad-689">**JavaScript kitaplıklarını dahil**</span><span class="sxs-lookup"><span data-stu-id="db6ad-689">**JavaScript libraries included**</span></span>

        1. <span data-ttu-id="db6ad-690">**jQuery**: jQuery HTML belge çapraz geçiş yapma, olay işleme, animasyon ve Ajax etkileşimleri basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-690">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
        2. <span data-ttu-id="db6ad-691">**jQuery UI**: Bu kitaplık için alt düzey etkileşim ve etkileri Gelişmiş animasyon ve bölümlerinin tema eklenebilir widgets, jQuery JavaScript kitaplığı üzerine inşa soyutlamalar sağlar.</span><span class="sxs-lookup"><span data-stu-id="db6ad-691">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

            > [!NOTE]
            > <span data-ttu-id="db6ad-692">JQuery ve jQuery UI hakkında bilgi edinin içinde [ [http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="db6ad-692">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
        3. <span data-ttu-id="db6ad-693">**Çakıştırmaları**: ASP.NET MVC 4 varsayılan şablonu artık içerir **Çakıştırmaları**, JavaScript ve HTML kullanarak zengin ve hızlı yanıt veren web uygulamaları oluşturmanıza olanak sağlayan bir JavaScript MVVM çerçevesi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-693">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="db6ad-694">Gibi ASP.NET MVC 3'te, jQuery ve jQuery UI kitaplıkları da ASP.NET MVC 4'te bulunur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-694">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

            > [!NOTE]
            > <span data-ttu-id="db6ad-695">Bu bağlantıyı Çakıştırmaları kitaplıkta hakkında daha fazla bilgi edinebilirsiniz: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="db6ad-695">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
        4. <span data-ttu-id="db6ad-696">**Modernizr**: sitenizi HTML5 ve CSS3 teknolojiler kullanılırken eski tarayıcılarla uyumlu hale getirme bu kitaplığı otomatik olarak çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-696">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

            > [!NOTE]
            > <span data-ttu-id="db6ad-697">Bu bağlantıyı Modernizr kitaplıkta hakkında daha fazla bilgi edinebilirsiniz: [http://www.modernizr.com/](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="db6ad-697">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
    3. <span data-ttu-id="db6ad-698">**Çözümdeki SimpleMembership**</span><span class="sxs-lookup"><span data-stu-id="db6ad-698">**SimpleMembership included in the solution**</span></span>

        <span data-ttu-id="db6ad-699">SimpleMembership, önceki ASP.NET rol ve üyelik sağlayıcısı sistem için bir yedek olarak tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-699">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="db6ad-700">Güvenli web sayfalarına geliştirici için daha esnek bir şekilde kolaylaştıran birçok yeni özellik vardır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-700">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

        <span data-ttu-id="db6ad-701">Internet şablonu SimpleMembership tümleştirmek için birkaç şey zaten ayarlanmış, örneğin, AccountController OAuthWebSecurity (için OAuth hesap kaydı, oturum açma, yönetim, vb.) ve Web güvenlik kullanmak için hazırlanır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-701">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

        <span data-ttu-id="db6ad-702">![Çözüm SimpleMembership dahil](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership dahil çözümü")</span><span class="sxs-lookup"><span data-stu-id="db6ad-702">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

        <span data-ttu-id="db6ad-703">*Çözüm SimpleMembership dahil*</span><span class="sxs-lookup"><span data-stu-id="db6ad-703">*SimpleMembership Included in the solution*</span></span>

        > [!NOTE]
        > <span data-ttu-id="db6ad-704">Hakkında daha fazla bilgi bulmak [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) MSDN'de.</span><span class="sxs-lookup"><span data-stu-id="db6ad-704">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="db6ad-705">Ayrıca, Windows Azure Web siteleri aşağıdaki bu uygulamayı dağıtabilmek için [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="db6ad-705">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="db6ad-706">Özet</span><span class="sxs-lookup"><span data-stu-id="db6ad-706">Summary</span></span>

<span data-ttu-id="db6ad-707">Bu uygulamalı Laboratuvar tamamlayarak ASP.NET MVC ile ilgili temel bilgileri öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="db6ad-707">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="db6ad-708">Bir MVC uygulaması ve nasıl etkileşim kurduklarını çekirdek öğeleri</span><span class="sxs-lookup"><span data-stu-id="db6ad-708">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="db6ad-709">Bir ASP.NET MVC uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="db6ad-709">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="db6ad-710">URL ve sorgu dizesi eklemek ve parametreleri işlemek için denetleyicileri yapılandırmak nasıl geçirildi</span><span class="sxs-lookup"><span data-stu-id="db6ad-710">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="db6ad-711">Ortak HTML içerik, Görünüm ve HTML içeriğini görüntülemek için bir görünüm şablonu geliştirmek için bir stil sayfası için bir şablon kurulumu için bir düzen ana sayfası ekleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-711">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="db6ad-712">Dinamik bilgileri görüntülemek için Görünüm şablonu özellikleri geçirmesi ViewModel desen kullanma</span><span class="sxs-lookup"><span data-stu-id="db6ad-712">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="db6ad-713">Görünüm şablonunda denetleyicilerine geçirilen parametrelerini kullanma</span><span class="sxs-lookup"><span data-stu-id="db6ad-713">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="db6ad-714">ASP.NET MVC uygulaması içindeki sayfalar için bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="db6ad-714">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="db6ad-715">Ekleme ve bir görünümde dinamik özelliklerini kullanın</span><span class="sxs-lookup"><span data-stu-id="db6ad-715">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="db6ad-716">ASP.NET MVC 4 proje şablonları'deki geliştirmeler</span><span class="sxs-lookup"><span data-stu-id="db6ad-716">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="db6ad-717">Ek A: Yükleme Web Visual Studio Express 2012 için</span><span class="sxs-lookup"><span data-stu-id="db6ad-717">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="db6ad-718">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümü kullanılarak  **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)** .</span><span class="sxs-lookup"><span data-stu-id="db6ad-718">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="db6ad-719">Aşağıdaki yönergeler yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="db6ad-719">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="db6ad-720">Git [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="db6ad-720">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="db6ad-721">Web Platformu yükleyicisi zaten yüklü değilse, alternatif olarak, bunu ve ürün için arama açabilirsiniz &quot; *Visual Studio Express 2012 için Windows Azure SDK'sı Web*&quot;.</span><span class="sxs-lookup"><span data-stu-id="db6ad-721">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;*Visual Studio Express 2012 for Web with Windows Azure SDK*&quot;.</span></span>
2. <span data-ttu-id="db6ad-722">Tıklayın **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-722">Click on **Install Now**.</span></span> <span data-ttu-id="db6ad-723">Sahip değilse **Web Platformu yükleyicisi** indirip önce yüklemek için yönlendirilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-723">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="db6ad-724">Bir kez **Web Platformu yükleyicisi** açık tıklatın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-724">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="db6ad-725">![Visual Studio Express yükleme](aspnet-mvc-4-fundamentals/_static/image43.png "yükleme Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="db6ad-725">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="db6ad-726">*Visual Studio Express yükleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-726">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="db6ad-727">Tüm ürünlerin lisans koşullarını okuyup ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-727">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşulları kabul ediliyor](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="db6ad-729">*Lisans koşulları kabul ediliyor*</span><span class="sxs-lookup"><span data-stu-id="db6ad-729">*Accepting the license terms*</span></span>
5. <span data-ttu-id="db6ad-730">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-730">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="db6ad-732">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="db6ad-732">*Installation progress*</span></span>
6. <span data-ttu-id="db6ad-733">Yükleme tamamlandığında tıklatın **son**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-733">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="db6ad-735">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="db6ad-735">*Installation completed*</span></span>
7. <span data-ttu-id="db6ad-736">Tıklatın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-736">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="db6ad-737">Web için Visual Studio Express açmak için Git **Başlat** ekranında ve yazmaya başlayın &quot; **VS Express**&quot;, tıklayın **VS Express Web** Döşeme.</span><span class="sxs-lookup"><span data-stu-id="db6ad-737">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express Web döşemeye](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="db6ad-739">*VS Express Web döşemeye*</span><span class="sxs-lookup"><span data-stu-id="db6ad-739">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="db6ad-740">Ek B: Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="db6ad-740">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="db6ad-741">Bu ekte Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturmak ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlamak nasıl yapacağınızı gösterir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-741">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="db6ad-742">Görev 1 - yeni bir Web sitesi oluşturma Windows Azure portalı</span><span class="sxs-lookup"><span data-stu-id="db6ad-742">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="db6ad-743">Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) ve aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-743">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-744">Windows Azure ile 10 ASP.NET Web siteleri ücretsiz barındırma ve ardından trafiğiniz büyüdükçe ölçeğinizi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-744">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="db6ad-745">Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="db6ad-745">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="db6ad-746">![Windows Azure portalında oturum açtığı](aspnet-mvc-4-fundamentals/_static/image48.png "Windows Azure Portal'da oturum açın")</span><span class="sxs-lookup"><span data-stu-id="db6ad-746">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="db6ad-747">*Windows Azure yönetim portalında oturum açtığı*</span><span class="sxs-lookup"><span data-stu-id="db6ad-747">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="db6ad-748">Tıklatın **yeni** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="db6ad-748">Click **New** on the command bar.</span></span>

    <span data-ttu-id="db6ad-749">![Yeni bir Web sitesi oluşturma](aspnet-mvc-4-fundamentals/_static/image49.png "yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-749">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="db6ad-750">*Yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-750">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="db6ad-751">Tıklatın **işlem** | **Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-751">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="db6ad-752">Ardından **hızlı Oluştur** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="db6ad-752">Then select **Quick Create** option.</span></span> <span data-ttu-id="db6ad-753">Yeni web sitesi için kullanılabilir bir URL girin ve tıklayın **Web sitesi oluştur**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-753">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-754">Windows Azure Web sitesi, denetleyebileceğiniz ve yönetebileceğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-754">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="db6ad-755">Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portal dışındaki dağıtmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="db6ad-755">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="db6ad-756">Bir veritabanını ayarlamak için adımları içermez.</span><span class="sxs-lookup"><span data-stu-id="db6ad-756">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="db6ad-757">![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](aspnet-mvc-4-fundamentals/_static/image50.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-757">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="db6ad-758">*Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-758">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="db6ad-759">Yeni kadar bekleyin **Web sitesi** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="db6ad-759">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="db6ad-760">Web sitesi oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun.</span><span class="sxs-lookup"><span data-stu-id="db6ad-760">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="db6ad-761">Yeni Web sitesi çalıştığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="db6ad-761">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="db6ad-762">![Yeni web sitesi için gözatma](aspnet-mvc-4-fundamentals/_static/image51.png "yeni web sitesi için gözatma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-762">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="db6ad-763">*Yeni web sitesi için gözatma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-763">*Browsing to the new web site*</span></span>

    <span data-ttu-id="db6ad-764">![Çalışan Web sitesi](aspnet-mvc-4-fundamentals/_static/image52.png "çalışan Web sitesi")</span><span class="sxs-lookup"><span data-stu-id="db6ad-764">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="db6ad-765">*Çalışan Web sitesi*</span><span class="sxs-lookup"><span data-stu-id="db6ad-765">*Web site running*</span></span>
6. <span data-ttu-id="db6ad-766">Portalına geri dönün ve web sitesi altında adına tıklayın **adı** yönetim sayfaları görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="db6ad-766">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="db6ad-767">![Web sitesi Yönetim sayfalarının açma](aspnet-mvc-4-fundamentals/_static/image53.png "web sitesi Yönetim sayfalarının açma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-767">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="db6ad-768">*Web Sitesi Yönetim sayfalarının açma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-768">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="db6ad-769">İçinde **Pano** sayfasında **Hızlı Bakış** 'yi tıklatın **yayım profili indirin** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="db6ad-769">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-770">*Yayımlama profili* her etkin yayımlama yöntemi için bir Windows Azure Web sitesi bir web uygulamasına yayımlamak için gereken bilgilerin tümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-770">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="db6ad-771">Yayımlama profili URL'leri, kullanıcı kimlik bilgilerini ve bağlanmak ve her bir yayımlama yönteminin etkinleştirildiği uç noktaları karşı kimlik doğrulaması için gerekli veritabanı dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-771">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="db6ad-772">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma destek yayımlamak için bu programlar yapılandırılmasını otomatikleştirmek için profilleri Windows Azure Web siteleri için Web uygulamaları yayımlama.</span><span class="sxs-lookup"><span data-stu-id="db6ad-772">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="db6ad-773">![Yayımlama profili web sitesi Yükleniyor](aspnet-mvc-4-fundamentals/_static/image54.png "yayımlama profili web sitesi yükleniyor")</span><span class="sxs-lookup"><span data-stu-id="db6ad-773">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="db6ad-774">*Yayımlama profili Web sitesi yükleniyor*</span><span class="sxs-lookup"><span data-stu-id="db6ad-774">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="db6ad-775">Yayımlama profili dosyasını bilinen bir konuma indirin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-775">Download the publish profile file to a known location.</span></span> <span data-ttu-id="db6ad-776">Daha fazla Bu alıştırmada Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlamak için bu dosyayı kullanmak nasıl göreceksiniz.</span><span class="sxs-lookup"><span data-stu-id="db6ad-776">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="db6ad-777">![Yayımlama profili dosyasını kaydetme](aspnet-mvc-4-fundamentals/_static/image55.png "yayımlama profilini kaydetme")</span><span class="sxs-lookup"><span data-stu-id="db6ad-777">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="db6ad-778">*Yayımlama profili dosyasını kaydetme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-778">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="db6ad-779">Görev 2 - veritabanı sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="db6ad-779">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="db6ad-780">Uygulamanızı SQL Server'ın kullanmak yaparsa veritabanlarının bir SQL veritabanı sunucusu oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-780">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="db6ad-781">SQL Server kullanmayan basit bir uygulamayı dağıtmak istiyorsanız, bu görevi atlamak.</span><span class="sxs-lookup"><span data-stu-id="db6ad-781">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="db6ad-782">Uygulama veritabanını depolamak için bir SQL veritabanı sunucusu gerekir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-782">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="db6ad-783">Aboneliğiniz Windows Azure Yönetim Portalı'nda SQL veritabanı sunucularının görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-783">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="db6ad-784">Oluşturulan bir sunucu yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğundan düğme.</span><span class="sxs-lookup"><span data-stu-id="db6ad-784">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="db6ad-785">Not edin **sunucu adı ve URL, yönetici oturum açma adı ve parola**sonraki görevleri kullanacağı gibi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-785">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="db6ad-786">Daha sonraki bir aşamada oluşturulacak şekilde veritabanı henüz oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-786">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="db6ad-787">![SQL veritabanı sunucusu Pano](aspnet-mvc-4-fundamentals/_static/image56.png "SQL veritabanı sunucu Panosu")</span><span class="sxs-lookup"><span data-stu-id="db6ad-787">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="db6ad-788">*SQL veritabanı sunucu Panosu*</span><span class="sxs-lookup"><span data-stu-id="db6ad-788">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="db6ad-789">İşlemin sonraki görev, sunucunun listesinde, yerel IP adresi içermesi gereken bu nedenle veritabanı bağlantısı Visual Studio'dan test edecek **izin verilen IP adreslerini**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-789">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="db6ad-790">Bunu yapmak için tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** üzerinde yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) düğmesi.</span><span class="sxs-lookup"><span data-stu-id="db6ad-790">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![İstemci IP adresi ekleme](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="db6ad-792">*İstemci IP adresi ekleme*</span><span class="sxs-lookup"><span data-stu-id="db6ad-792">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="db6ad-793">Bir kez **istemci IP adresi** izin verilen IP adreslerine eklenen listesinde, tıklayın **kaydetmek** değişiklikleri onaylamak için.</span><span class="sxs-lookup"><span data-stu-id="db6ad-793">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Değişiklikleri onaylamak](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="db6ad-795">*Değişiklikleri onaylamak*</span><span class="sxs-lookup"><span data-stu-id="db6ad-795">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="db6ad-796">Görev 3 - Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="db6ad-796">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="db6ad-797">ASP.NET MVC 4 çözüme geri dönün.</span><span class="sxs-lookup"><span data-stu-id="db6ad-797">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="db6ad-798">İçinde **Çözüm Gezgini**, web sitesi projesine sağ tıklatın ve **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-798">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="db6ad-799">![Uygulama yayımlama](aspnet-mvc-4-fundamentals/_static/image60.png "uygulama yayımlama")</span><span class="sxs-lookup"><span data-stu-id="db6ad-799">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="db6ad-800">*Web sitesi yayımlama*</span><span class="sxs-lookup"><span data-stu-id="db6ad-800">*Publishing the web site*</span></span>
2. <span data-ttu-id="db6ad-801">İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-801">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="db6ad-802">![Yayımlama profilini içeri aktarma](aspnet-mvc-4-fundamentals/_static/image61.png "yayımlama profilini içeri aktarma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-802">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="db6ad-803">*Yayımlama profilini içeri aktarma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-803">*Importing publish profile*</span></span>
3. <span data-ttu-id="db6ad-804">Tıklatın **bağlantısı doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-804">Click **Validate Connection**.</span></span> <span data-ttu-id="db6ad-805">Doğrulama tamamlandıktan sonra tıklayın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-805">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="db6ad-806">Yanındaki bağlantıyı doğrula düğmesi görünür yeşil bir onay işareti gördüğünüzde doğrulama tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="db6ad-806">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="db6ad-807">![Bağlantı doğrulama](aspnet-mvc-4-fundamentals/_static/image62.png "bağlantısı doğrulanıyor")</span><span class="sxs-lookup"><span data-stu-id="db6ad-807">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="db6ad-808">*Doğrulama bağlantısı*</span><span class="sxs-lookup"><span data-stu-id="db6ad-808">*Validating connection*</span></span>
4. <span data-ttu-id="db6ad-809">İçinde **ayarları** sayfasında **veritabanları** bölümünde, veritabanı bağlantının textbox yanındaki düğmesini tıklatın (yani **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="db6ad-809">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="db6ad-810">![Web dağıtımı yapılandırma](aspnet-mvc-4-fundamentals/_static/image63.png "Web dağıtımı yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-810">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="db6ad-811">*Web dağıtımı yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-811">*Web deploy configuration*</span></span>
5. <span data-ttu-id="db6ad-812">Veritabanı bağlantısı aşağıdaki gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="db6ad-812">Configure the database connection as follows:</span></span>

    - <span data-ttu-id="db6ad-813">İçinde **sunucu adı** , SQL veritabanı sunucusu URL'yi kullanarak yazın *tcp:* öneki.</span><span class="sxs-lookup"><span data-stu-id="db6ad-813">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
    - <span data-ttu-id="db6ad-814">İçinde **kullanıcı adı** sunucunuzun yönetici oturum açma adını yazın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-814">In **User name** type your server administrator login name.</span></span>
    - <span data-ttu-id="db6ad-815">İçinde **parola** sunucu yönetici oturum açma parolasını yazın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-815">In **Password** type your server administrator login password.</span></span>
    - <span data-ttu-id="db6ad-816">Yeni bir veritabanı adı girin: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="db6ad-816">Type a new database name, for example: *MVC4SampleDB*.</span></span>

    <span data-ttu-id="db6ad-817">![Hedef bağlantı dizesi yapılandırma](aspnet-mvc-4-fundamentals/_static/image64.png "hedef bağlantı dizesi yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-817">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

    <span data-ttu-id="db6ad-818">*Hedef bağlantı dizesi yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="db6ad-818">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="db6ad-819">Sonra **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-819">Then click **OK**.</span></span> <span data-ttu-id="db6ad-820">Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-820">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="db6ad-821">![Veritabanı oluşturma](aspnet-mvc-4-fundamentals/_static/image65.png "veritabanı dizesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="db6ad-821">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="db6ad-822">*Veritabanı oluşturuluyor*</span><span class="sxs-lookup"><span data-stu-id="db6ad-822">*Creating the database*</span></span>
7. <span data-ttu-id="db6ad-823">Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini varsayılan bağlantı textbox içinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-823">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="db6ad-824">Sonra **İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-824">Then click **Next**.</span></span>

    <span data-ttu-id="db6ad-825">![SQL veritabanına işaret eden bağlantı dizesi](aspnet-mvc-4-fundamentals/_static/image66.png "SQL veritabanına işaret eden bağlantı dizesi")</span><span class="sxs-lookup"><span data-stu-id="db6ad-825">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="db6ad-826">*SQL veritabanına işaret eden bağlantı dizesi*</span><span class="sxs-lookup"><span data-stu-id="db6ad-826">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="db6ad-827">İçinde **Önizleme** sayfasında, **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-827">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="db6ad-828">![Web uygulaması yayımlama](aspnet-mvc-4-fundamentals/_static/image67.png "web uygulaması yayımlama")</span><span class="sxs-lookup"><span data-stu-id="db6ad-828">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="db6ad-829">*Web uygulaması yayımlama*</span><span class="sxs-lookup"><span data-stu-id="db6ad-829">*Publishing the web application*</span></span>
9. <span data-ttu-id="db6ad-830">Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesini açın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-830">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="db6ad-831">![Uygulama için Windows Azure yayımlanan](aspnet-mvc-4-fundamentals/_static/image68.png "uygulama için Windows Azure yayımlanır")</span><span class="sxs-lookup"><span data-stu-id="db6ad-831">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="db6ad-832">*Windows Azure için yayımlanan uygulama*</span><span class="sxs-lookup"><span data-stu-id="db6ad-832">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="db6ad-833">Ek C: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="db6ad-833">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="db6ad-834">Kod parçacıkları ile parmaklarınızın ucunda gerek duyduğunuz tüm koduna sahip.</span><span class="sxs-lookup"><span data-stu-id="db6ad-834">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="db6ad-835">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki resimde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="db6ad-835">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="db6ad-836">![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-fundamentals/_static/image69.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="db6ad-836">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="db6ad-837">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="db6ad-837">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="db6ad-838">***Klavye (C# yalnızca) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="db6ad-838">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="db6ad-839">İmleci, burada kod eklemek istediğiniz yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-839">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="db6ad-840">(Olmadan, boşluk veya tire) parçacığı adını yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-840">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="db6ad-841">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-841">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="db6ad-842">Doğru parçacığı seçin (veya tüm kod parçacığını kişinin adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="db6ad-842">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="db6ad-843">İki kez parçacığını İmleç konumuna eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-843">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="db6ad-844">![Kod parçacığında adını yazmaya başlayın](aspnet-mvc-4-fundamentals/_static/image70.png "parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="db6ad-844">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="db6ad-845">*Kod parçacığında adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="db6ad-845">*Start typing the snippet name*</span></span>

<span data-ttu-id="db6ad-846">![Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın](aspnet-mvc-4-fundamentals/_static/image71.png "vurgulanan kod parçacığını seçmek için SEKME tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="db6ad-846">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="db6ad-847">*Vurgulanan kod parçacığını seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="db6ad-847">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="db6ad-848">![Yeniden SEKME tuşuna basın ve kod parçacığını genişletin](aspnet-mvc-4-fundamentals/_static/image72.png "yeniden SEKME tuşuna basın ve kod parçacığını genişletin")</span><span class="sxs-lookup"><span data-stu-id="db6ad-848">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="db6ad-849">*Yeniden SEKME tuşuna basın ve kod parçacığını genişletin*</span><span class="sxs-lookup"><span data-stu-id="db6ad-849">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="db6ad-850">***Fareyi (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1.</span><span class="sxs-lookup"><span data-stu-id="db6ad-850">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="db6ad-851">Kod parçacığını eklemek istediğiniz yeri sağ tıklatın.</span><span class="sxs-lookup"><span data-stu-id="db6ad-851">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="db6ad-852">Seçin **Ekle parçacığı** arkasından **My kod parçacıkları**.</span><span class="sxs-lookup"><span data-stu-id="db6ad-852">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="db6ad-853">Tıklayarak ilgili kod parçacığında listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="db6ad-853">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="db6ad-854">![Sağ tıklatın, istediğiniz kod parçacığını eklemek ve Ekle parçacık](aspnet-mvc-4-fundamentals/_static/image73.png "sağ tıklatın, istediğiniz kod parçacığını eklemek ve parçacık Ekle")</span><span class="sxs-lookup"><span data-stu-id="db6ad-854">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="db6ad-855">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz yeri sağ tıklatın*</span><span class="sxs-lookup"><span data-stu-id="db6ad-855">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="db6ad-856">![Tıklayarak ilgili kod parçacığında listeden çekme](aspnet-mvc-4-fundamentals/_static/image74.png "tıklayarak ilgili kod parçacığında listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="db6ad-856">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="db6ad-857">*Tıklayarak ilgili kod parçacığında listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="db6ad-857">*Pick the relevant snippet from the list, by clicking on it*</span></span>
