---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: ASP.NET MVC 4 temelleri | Microsoft Docs
author: rick-anderson
description: Bu uygulamalı laboratuvarı MVC (Model View Controller) müzik Store tanıtır ve nasıl ASP.NET MV kullanılacağını adım adım anlatan bir öğretici uygulama alan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 3a282d02ba929eb86571e92f190550614962524d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386581"
---
# <a name="aspnet-mvc-4-fundamentals"></a><span data-ttu-id="9e1b1-103">ASP.NET MVC 4 temelleri</span><span class="sxs-lookup"><span data-stu-id="9e1b1-103">ASP.NET MVC 4 Fundamentals</span></span>

<span data-ttu-id="9e1b1-104">Tarafından [Team Web Kampları](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-104">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="9e1b1-105">Eğitim Seti Web Kampları indirin</span><span class="sxs-lookup"><span data-stu-id="9e1b1-105">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="9e1b1-106">Bu uygulamalı laboratuvarı MVC (Model View Controller) müzik Store tanıtır ve ASP.NET MVC ve Visual Studio'nun nasıl kullanılacağını adım adım anlatan bir öğretici uygulama temel alır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-106">This Hands-On Lab is based on MVC (Model View Controller) Music Store, a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="9e1b1-107">Laboratuvar Basitlik öğreneceksiniz. Bu teknolojiler birlikte kullanarak, henüz güç.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-107">Throughout the lab you will learn the simplicity, yet power of using these technologies together.</span></span> <span data-ttu-id="9e1b1-108">Basit bir uygulamayla başlar ve tam işlevsel bir ASP.NET MVC 4 Web uygulaması elde edene kadar oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-108">You will start with a simple application and will build it until you have a fully functional ASP.NET MVC 4 Web Application.</span></span>

<span data-ttu-id="9e1b1-109">Bu Laboratuvar, ASP.NET MVC 4 ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-109">This Lab works with ASP.NET MVC 4.</span></span>

<span data-ttu-id="9e1b1-110">Öğretici uygulama ASP.NET MVC 3 sürümünü keşfedin istiyorsanız, bunu bulabilirsiniz [MVC müzik Store](https://github.com/evilDave/MVC-Music-Store).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-110">If you wish to explore the ASP.NET MVC 3 version of the tutorial application, you can find it in [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).</span></span>

<span data-ttu-id="9e1b1-111">Bu uygulamalı Laboratuvar, geliştirici deneyimi HTML ve JavaScript gibi Web geliştirme teknolojilerine olduğu varsayılır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-111">This Hands-On Lab assumes that the developer has experience in Web development technologies, such as HTML and JavaScript.</span></span>

> [!NOTE]
> <span data-ttu-id="9e1b1-112">Web Kampları eğitim Seti, kullanılabilir tüm örnek kodu ve kod parçacıkları dahil [Microsoft-Web/WebCampTrainingKit yayınlar](https://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-112">All sample code and snippets are included in the Web Camps Training Kit, available at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="9e1b1-113">Bu Laboratuvar için belirli proje kullanılabilir [ASP.NET MVC 4 Temelleri](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-113">The project specific to this lab is available at [ASP.NET MVC 4 Fundamentals](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).</span></span>

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a><span data-ttu-id="9e1b1-114">Müzik Store uygulaması</span><span class="sxs-lookup"><span data-stu-id="9e1b1-114">The Music Store application</span></span>

<span data-ttu-id="9e1b1-115">Bu Laboratuvar içinde oluşturulacak müzik Store web uygulaması üç ana bölümden oluşur: alışveriş, kullanıma alma ve yönetim.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-115">The Music Store web application that will be built throughout this Lab comprises three main parts: shopping, checkout, and administration.</span></span> <span data-ttu-id="9e1b1-116">Ziyaretçi albümleri türe göre göz atın, albümleri, Sepete Ekle, seçimi gözden geçirin ve son oturum açma ve sırasını tamamlamak için kullanıma devam etmek mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-116">Visitors will be able to browse albums by genre, add albums to their cart, review their selection and finally proceed to checkout to login and complete the order.</span></span> <span data-ttu-id="9e1b1-117">Ayrıca, depolama yöneticileri kullanılabilir albümleri, hem de ana özelliklerini yönetmek mümkün olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-117">Additionally, store administrators will be able to manage the available albums as well as their main properties.</span></span>

<span data-ttu-id="9e1b1-118">![Müzik Store ekranlar](aspnet-mvc-4-fundamentals/_static/image1.png "müzik Store ekranları")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-118">![Music Store screens](aspnet-mvc-4-fundamentals/_static/image1.png "Music Store screens")</span></span>

<span data-ttu-id="9e1b1-119">*Müzik Store ekranları*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-119">*Music Store screens*</span></span>

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a><span data-ttu-id="9e1b1-120">ASP.NET MVC 4 temelleri</span><span class="sxs-lookup"><span data-stu-id="9e1b1-120">ASP.NET MVC 4 Essentials</span></span>

<span data-ttu-id="9e1b1-121">Müzik Store uygulaması kullanarak oluşturulur **Model View Controller (MVC)**, uygulamanın üç ana bileşene ayıran bir mimari deseni:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-121">Music Store application will be built using **Model View Controller (MVC)**, an architectural pattern that separates an application into three main components:</span></span>

- <span data-ttu-id="9e1b1-122">**Modelleri**: Model nesneleri, etki alanının mantığını uygulayan uygulamanın parçaları.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-122">**Models**: Model objects are the parts of the application that implement the domain logic.</span></span> <span data-ttu-id="9e1b1-123">Genellikle model nesneleri ayrıca almak ve model durumu bir veritabanında depolar.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-123">Often, model objects also retrieve and store model state in a database.</span></span>
- <span data-ttu-id="9e1b1-124">**Görünümler:** görünümler, uygulamanın kullanıcı arabirimini (UI) görüntüleyen bileşenleridir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-124">**Views:** Views are the components that display the application's user interface (UI).</span></span> <span data-ttu-id="9e1b1-125">Genellikle, bu UI model verilerinden oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-125">Typically, this UI is created from the model data.</span></span> <span data-ttu-id="9e1b1-126">Örnek metin kutuları ve albüm nesnenin geçerli durumuna bağlı açılan listesini görüntüleyen albümleri düzenleme görünümü olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-126">An example would be the edit view of Albums that displays text boxes and a drop-down list based on the current state of an Album object.</span></span>
- <span data-ttu-id="9e1b1-127">**Denetleyicileri:** denetleyicileri, kullanıcı etkileşimi işlemek, modelle ve sonuçta UI işlemek için bir görünüm seçin bileşenleridir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-127">**Controllers:** Controllers are the components that handle user interaction, manipulate the model, and ultimately select a view to render the UI.</span></span> <span data-ttu-id="9e1b1-128">Bir MVC uygulamasında, görünüm yalnızca bilgileri görüntüler; Denetleyici, işler ve kullanıcı girişini ve etkileşimini yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-128">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="9e1b1-129">MVC örüntüsü, öğeler arasında gevşek bir bağlantı sağlayarak uygulama (giriş mantığı, iş mantığı ve UI mantığı) farklı yönlerini birbirlerinden ayıran uygulamalar oluşturmanıza yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-129">The MVC pattern helps you to create applications that separate the different aspects of the application (input logic, business logic, and UI logic), while providing a loose coupling between these elements.</span></span> <span data-ttu-id="9e1b1-130">Bu ayrım bir uygulama oluşturduğunuzda uygulamasının aynı anda tek bir yönüne odaklanma sağladığından karmaşıklığını yönetmenize yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-130">This separation helps you manage complexity when you build an application, as it allows you to focus on one aspect of the implementation at a time.</span></span> <span data-ttu-id="9e1b1-131">Ayrıca, MVC örüntüsü, aynı zamanda teste dayalı geliştirme (TDD) uygulamaları oluşturmak için kullanımı ve uygulamaları test etmek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-131">In addition, the MVC pattern makes it easy to test applications, also encouraging the use of test-driven development (TDD) for creating applications.</span></span>

<span data-ttu-id="9e1b1-132">**ASP.NET MVC** framework, ASP.NET MVC tabanlı Web uygulamaları oluşturmak için ASP.NET Web Forms örüntüsüne bir alternatif sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-132">The **ASP.NET MVC** framework provides an alternative to the ASP.NET Web Forms pattern for creating ASP.NET MVC-based Web applications.</span></span> <span data-ttu-id="9e1b1-133">**ASP.NET MVC** çerçevedir basit, yüksek düzeyde sınanabilir bir sunu çerçevesidir, (Web forms tabanlı uygulamaları olduğu gibi) mevcut ASP.NET özellikleriyle gibi ana sayfalar ve üyelik tabanlı Tümleşik temel .NET framework'ün tüm gücüne sahip olursunuz, böylece kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-133">The **ASP.NET MVC** framework is a lightweight, highly testable presentation framework that (as with Web-forms-based applications) is integrated with existing ASP.NET features, such as master pages and membership-based authentication so you get all the power of the core .NET framework.</span></span> <span data-ttu-id="9e1b1-134">Kullanmakta olduğunuz tüm kitaplıkları da ASP.NET MVC 4'te kullanılabilir olmadığından, ASP.NET Web Forms ile bilginiz varsa, bu yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-134">This is useful if you are already familiar with ASP.NET Web Forms because all the libraries that you already use are available in ASP.NET MVC 4 as well.</span></span>

<span data-ttu-id="9e1b1-135">Ayrıca, bir MVC uygulamasının ilgili üç ana bileşeni arasındaki bu sıkı bağ paralel gelişimi de kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-135">In addition, the loose coupling between the three main components of an MVC application also promotes parallel development.</span></span> <span data-ttu-id="9e1b1-136">Örneğin, bir geliştirici görünümde çalışabilir, ikinci bir geliştirici denetleyici mantığında çalışabilir ve üçüncü Geliştirici modelinde iş mantığı üzerinde odaklanabilir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-136">For instance, one developer can work on the view, a second developer can work on the controller logic, and a third developer can focus on the business logic in the model.</span></span>

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="9e1b1-137">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="9e1b1-137">Objectives</span></span>

<span data-ttu-id="9e1b1-138">Bu uygulamalı laboratuvarda, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-138">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="9e1b1-139">Sıfırdan müzik Store uygulaması Öğreticisi üzerinde bir ASP.NET MVC uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-139">Create an ASP.NET MVC application from scratch based on the Music Store Application tutorial</span></span>
- <span data-ttu-id="9e1b1-140">Giriş sayfasında sitenin ve ana işlevselliğini gözatma için URL'leri işlemek için denetleyicileri ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-140">Add Controllers to handle URLs to the Home page of the site and for browsing its main functionality</span></span>
- <span data-ttu-id="9e1b1-141">Stilinin birlikte görüntülenen içerik özelleştirmek için bir Görünüm Ekle</span><span class="sxs-lookup"><span data-stu-id="9e1b1-141">Add a View to customize the content displayed along with its style</span></span>
- <span data-ttu-id="9e1b1-142">Model sınıfları içeren ve veri ve etki alanı mantığı yönetmek için ekleyin</span><span class="sxs-lookup"><span data-stu-id="9e1b1-142">Add Model classes to contain and manage data and domain logic</span></span>
- <span data-ttu-id="9e1b1-143">Denetleyici eylemlerine görünüm şablonları için bilgi geçirmek görünüm modeli düzeni kullanın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-143">Use View Model pattern to pass information from controller actions to the view templates</span></span>
- <span data-ttu-id="9e1b1-144">ASP.NET MVC 4 yeni şablon Internet uygulamaları için keşfetmek</span><span class="sxs-lookup"><span data-stu-id="9e1b1-144">Explore the ASP.NET MVC 4 new template for internet applications</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="9e1b1-145">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="9e1b1-145">Prerequisites</span></span>

<span data-ttu-id="9e1b1-146">Bu laboratuvarı tamamlamak için aşağıdakiler olmalıdır:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-146">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="9e1b1-147">[Web için Visual Studio 2012 Express](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (okuma [ek A](#AppendixA) nasıl yükleneceği hakkında yönergeler için)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-147">[Visual Studio 2012 Express for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (read [Appendix A](#AppendixA) for instructions on how to install it)</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="9e1b1-148">Kurulum</span><span class="sxs-lookup"><span data-stu-id="9e1b1-148">Setup</span></span>

<span data-ttu-id="9e1b1-149">**Kod parçacıkları yükleniyor**</span><span class="sxs-lookup"><span data-stu-id="9e1b1-149">**Installing Code Snippets**</span></span>

<span data-ttu-id="9e1b1-150">Kolaylık olması için bu Laboratuvar yöneteceğiniz kodun çoğu Visual Studio kod parçacıkları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-150">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="9e1b1-151">Kod parçacıklarını çalıştırmak yüklenecek **.\Source\Setup\CodeSnippets.vsi** dosya.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-151">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="9e1b1-152">Visual Studio kod parçacıkları ve bunları nasıl kullanacağınızı öğrenmek istediğiniz konusunda bilgi sahibi değilseniz, bu belge, ek başvurabilir &quot; [ek C: kullanarak kod parçacıklarına](#AppendixC)&quot;.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-152">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="9e1b1-153">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="9e1b1-153">Exercises</span></span>

<span data-ttu-id="9e1b1-154">Bu uygulamalı Laboratuvar tarafından aşağıdaki çalışmaları oluşur:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-154">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="9e1b1-155">Alıştırma 1: MusicStore ASP.NET MVC Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-155">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>](#Exercise1)
2. [<span data-ttu-id="9e1b1-156">Alıştırma 2: Denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-156">Exercise 2: Creating a Controller</span></span>](#Exercise2)
3. [<span data-ttu-id="9e1b1-157">Alıştırma 3: bir denetleyiciye parametreleri geçirme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-157">Exercise 3: Passing parameters to a Controller</span></span>](#Exercise3)
4. [<span data-ttu-id="9e1b1-158">Alıştırma 4: bir görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-158">Exercise 4: Creating a View</span></span>](#Exercise4)
5. [<span data-ttu-id="9e1b1-159">Alıştırma 5: bir görünüm modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-159">Exercise 5: Creating a View Model</span></span>](#Exercise5)
6. [<span data-ttu-id="9e1b1-160">Alıştırma 6: görünüm parametrelerini kullanma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-160">Exercise 6: Using parameters in View</span></span>](#Exercise6)
7. [<span data-ttu-id="9e1b1-161">Alıştırma 7: ASP.NET MVC 4 yeni şablonu turu</span><span class="sxs-lookup"><span data-stu-id="9e1b1-161">Exercise 7: A lap around ASP.NET MVC 4 New Template</span></span>](#Exercise7)

> [!NOTE]
> <span data-ttu-id="9e1b1-162">Her bir alıştırma olarak sunulduğu bir **son** elde alıştırmalar tamamladıktan sonra ortaya çıkan çözüm içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-162">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="9e1b1-163">Çalışma alıştırmaları ek yardıma ihtiyacınız varsa, bu çözüm bir kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-163">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<span data-ttu-id="9e1b1-164">Bu laboratuvarı tamamlamak için tahmini süre: **60 dakika**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-164">Estimated time to complete this lab: **60 minutes**.</span></span>

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a><span data-ttu-id="9e1b1-165">Alıştırma 1: MusicStore ASP.NET MVC Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-165">Exercise 1: Creating MusicStore ASP.NET MVC Web Application Project</span></span>

<span data-ttu-id="9e1b1-166">Bu alıştırmada, bir ASP.NET MVC uygulamasını Visual Studio 2012 Express kendi ana klasörü kuruluş yanı sıra Web için nasıl oluşturulacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-166">In this exercise, you will learn how to create an ASP.NET MVC application in Visual Studio 2012 Express for Web as well as its main folder organization.</span></span> <span data-ttu-id="9e1b1-167">Ayrıca, yeni bir denetleyici ekleyeceksiniz ve basit bir dize uygulamanın giriş sayfasında görüntülenen kolaylaştırmak nasıl öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-167">Additionally, you will learn how to add a new Controller and make it display a simple string in the application's home page.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a><span data-ttu-id="9e1b1-168">Görev 1 - ASP.NET MVC Web uygulaması projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-168">Task 1 - Creating the ASP.NET MVC Web Application Project</span></span>

1. <span data-ttu-id="9e1b1-169">Bu görevde, Visual Studio MVC şablonu kullanarak boş bir ASP.NET MVC uygulaması projesi oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-169">In this task, you will create an empty ASP.NET MVC application project using the MVC Visual Studio template.</span></span> <span data-ttu-id="9e1b1-170">Başlangıç **Web için VS Express**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-170">Start **VS Express for Web**.</span></span>
2. <span data-ttu-id="9e1b1-171">Üzerinde **dosya** menüsünü tıklatın **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-171">On the **File** menu, click **New Project**.</span></span>
3. <span data-ttu-id="9e1b1-172">İçinde **yeni proje** iletişim kutusu seç **ASP.NET MVC 4 Web uygulaması** proje türü, altında bulunan **Visual C#** **Web** şablonu Liste.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-172">In the **New Project** dialog box select the **ASP.NET MVC 4 Web Application** project type, located under **Visual C#,** **Web** template list.</span></span>
4. <span data-ttu-id="9e1b1-173">Değişiklik **adı** için *MvcMusicStore*.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-173">Change the **Name** to *MvcMusicStore*.</span></span>
5. <span data-ttu-id="9e1b1-174">İçinde yeni bir çözüm konumunu ayarlayın **başlamak** klasöründe Bu alıştırmada'nın kaynak, örneğin **[YOUR-HOL-PATH] \Source\Ex01-CreatingMusicStoreProject\Begin**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-174">Set the location of the solution inside a new **Begin** folder in this Exercise's Source folder, for example **[YOUR-HOL-PATH]\Source\Ex01-CreatingMusicStoreProject\Begin**.</span></span> <span data-ttu-id="9e1b1-175">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-175">Click **OK**.</span></span>

    <span data-ttu-id="9e1b1-176">![Yeni Proje iletişim kutusu oluşturma](aspnet-mvc-4-fundamentals/_static/image2.png "yeni proje iletişim kutusu oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-176">![Create New Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image2.png "Create New Project Dialog Box")</span></span>

    <span data-ttu-id="9e1b1-177">*Yeni Proje iletişim kutusu oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-177">*Create New Project Dialog Box*</span></span>
6. <span data-ttu-id="9e1b1-178">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusu seç **temel** şablon emin olun **görünüm altyapısı** seçilmişse **Razor**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-178">In the **New ASP.NET MVC 4 Project** dialog box select the **Basic** template and make sure that the **View engine** selected is **Razor**.</span></span> <span data-ttu-id="9e1b1-179">**Tamam**'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-179">Click **OK**.</span></span>

    <span data-ttu-id="9e1b1-180">![Yeni ASP.NET MVC 4 Proje iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image3.png "yeni ASP.NET MVC 4 Proje iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-180">![New ASP.NET MVC 4 Project Dialog Box](aspnet-mvc-4-fundamentals/_static/image3.png "New ASP.NET MVC 4 Project Dialog Box")</span></span>

    <span data-ttu-id="9e1b1-181">*Yeni ASP.NET MVC 4 Proje iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-181">*New ASP.NET MVC 4 Project Dialog Box*</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a><span data-ttu-id="9e1b1-182">Görev 2 - çözüm yapısını keşfetme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-182">Task 2 - Exploring the Solution Structure</span></span>

<span data-ttu-id="9e1b1-183">ASP.NET MVC çerçevesi, MVC örüntüsünü destekleyen Web uygulamaları oluşturmanıza yardımcı olan bir Visual Studio Proje şablonu içerir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-183">The ASP.NET MVC framework includes a Visual Studio project template that helps you create Web applications supporting the MVC pattern.</span></span> <span data-ttu-id="9e1b1-184">Bu şablon, gerekli klasörleri, öğe şablonları ve yapılandırma dosyası girdileri ile yeni bir ASP.NET MVC Web uygulaması oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-184">This template creates a new ASP.NET MVC Web application with the required folders, item templates, and configuration-file entries.</span></span>

<span data-ttu-id="9e1b1-185">Bu görevde, söz konusu olan öğeleri anlamak için çözüm yapısını ve ilişkilerini inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-185">In this task, you will examine the solution structure to understand the elements that are involved and their relationships.</span></span> <span data-ttu-id="9e1b1-186">ASP.NET MVC çerçevesi varsayılan olarak kullandığı için aşağıdaki klasörleri tüm ASP.NET MVC uygulamasında bulunan bir &quot;kuralı yapılandırmanız üzerinde&quot; yaklaşım ve bazı varsayılan varsayımlara temel klasör adlandırma üzerinde yapar kuralları.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-186">The following folders are included in all the ASP.NET MVC application because the ASP.NET MVC framework by default uses a &quot;convention over configuration&quot; approach, and makes some default assumptions based on folder naming conventions.</span></span>

1. <span data-ttu-id="9e1b1-187">Proje oluşturulduktan sonra Çözüm Gezgini'nde sağ tarafındaki oluşturulan klasör yapısını inceleyin:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-187">Once the project is created, review the folder structure that has been created in the Solution Explorer on the right side:</span></span>

    <span data-ttu-id="9e1b1-188">![ASP.NET MVC klasör yapısını Çözüm Gezgini'nde](aspnet-mvc-4-fundamentals/_static/image4.png "Çözüm Gezgini'nde ASP.NET MVC klasör yapısı")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-188">![ASP.NET MVC Folder structure in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image4.png "ASP.NET MVC Folder structure in Solution Explorer")</span></span>

    <span data-ttu-id="9e1b1-189">*Çözüm Gezgini'nde ASP.NET MVC klasör yapısı*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-189">*ASP.NET MVC Folder structure in Solution Explorer*</span></span>

   1. <span data-ttu-id="9e1b1-190">**Denetleyicileri**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-190">**Controllers**.</span></span> <span data-ttu-id="9e1b1-191">Bu klasör denetleyicisi sınıfları içerir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-191">This folder will contain the controller classes.</span></span> <span data-ttu-id="9e1b1-192">Son kullanıcı etkileşimini işlemedeki, modeli düzenleme ve sonuçta görünümü kullanıcı arabirimini seçmek için bir temel MVC uygulamasındaki denetleyicileri sorumludur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-192">In an MVC based application, controllers are responsible for handling end user interaction, manipulating the model, and ultimately choosing a view to render the UI.</span></span>

       > [!NOTE]
       > <span data-ttu-id="9e1b1-193">MVC çerçevesi ile bitmesi tüm denetleyicileri adlarını gerektiren &quot;denetleyicisi&quot;-Örneğin, HomeController, LoginController veya ProductController.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-193">The MVC framework requires the names of all controllers to end with &quot;Controller&quot;-for example, HomeController, LoginController, or ProductController.</span></span>
   2. <span data-ttu-id="9e1b1-194">**Modelleri**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-194">**Models**.</span></span> <span data-ttu-id="9e1b1-195">Bu klasör, MVC Web uygulaması için uygulama modelini temsil eden sınıflar sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-195">This folder is provided for classes that represent the application model for the MVC Web application.</span></span> <span data-ttu-id="9e1b1-196">Bu genellikle, nesneleri ve veri deposu ile etkileşim kurmak için mantığı tanımlayan kodu içerir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-196">This usually includes code that defines objects and the logic for interacting with the data store.</span></span> <span data-ttu-id="9e1b1-197">Genellikle, gerçek model nesneleri ayrı sınıf kitaplıkları olacaktır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-197">Typically, the actual model objects will be in separate class libraries.</span></span> <span data-ttu-id="9e1b1-198">Ancak, yeni bir uygulama oluşturduğunuzda, sınıfları ve bunları daha sonraki bir noktada ayrı sınıf kitaplıkları olarak geliştirme döngüsünde taşırsanız.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-198">However, when you create a new application, you might include classes and then move them into separate class libraries at a later point in the development cycle.</span></span>
   3. <span data-ttu-id="9e1b1-199">**Görünümleri**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-199">**Views**.</span></span> <span data-ttu-id="9e1b1-200">Bu klasör, görünümler, uygulamanın kullanıcı arabirimini görüntülemek için sorumlu bileşenleri için önerilen konumdur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-200">This folder is the recommended location for views, the components responsible for displaying the application's user interface.</span></span> <span data-ttu-id="9e1b1-201">İşleme Görünümlerle ilgili diğer tüm dosyaları ek olarak, .aspx, .ascx, .cshtml ve .master dosyaları görünümlerini kullanın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-201">Views use .aspx, .ascx, .cshtml and .master files, in addition to any other files that are related to rendering views.</span></span> <span data-ttu-id="9e1b1-202">Görünüm klasörü her denetleyici için bir klasör içerir; Bu klasör, denetleyici adı ön ekine sahip olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-202">Views folder contains a folder for each controller; the folder is named with the controller-name prefix.</span></span> <span data-ttu-id="9e1b1-203">Örneğin, adında bir denetleyici varsa **HomeController**, görünümleri klasör giriş adlı bir klasör içerir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-203">For example, if you have a controller named **HomeController**, the Views folder will contain a folder named Home.</span></span> <span data-ttu-id="9e1b1-204">ASP.NET MVC çerçevesi bir görünüm yüklendiğinde varsayılan olarak, Views\controllerName klasöründe istenen görünüm ada sahip .aspx dosyası arar (**görünümler [ControllerName] [Action] .aspx**) veya (**görünümler [ControllerName] [Action] .cshtml**) Razor görünümleri için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-204">By default, when the ASP.NET MVC framework loads a view, it looks for an .aspx file with the requested view name in the Views\controllerName folder (**Views[ControllerName][Action].aspx**) or (**Views[ControllerName][Action].cshtml**) for Razor Views.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9e1b1-205">Daha önce listelenen klasörler ek olarak, bir MVC Web uygulaması kullanan **Global.asax** genel URL yönlendirmeyi ayarlamak için dosyası varsayılan olarak ve kullanır **Web.config** uygulama yapılandırma dosyası.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-205">In addition to the folders listed previously, an MVC Web application uses the **Global.asax** file to set global URL routing defaults, and it uses the **Web.config** file to configure the application.</span></span>

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a><span data-ttu-id="9e1b1-206">Görev 3 - bir HomeController ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-206">Task 3 - Adding a HomeController</span></span>

<span data-ttu-id="9e1b1-207">MVC çerçevesi kullanmayan ASP.NET uygulamalarında kullanıcı etkileşimi sayfaların çevresine ve oluşturma ve bu sayfalarındaki olayları işleme düzenlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-207">In ASP.NET applications that do not use the MVC framework, user interaction is organized around pages, and around raising and handling events from those pages.</span></span> <span data-ttu-id="9e1b1-208">Buna karşılık, ASP.NET MVC uygulamaları ile kullanıcı etkileşimi denetleyicileri ve kendi eylem yöntemlerini düzenlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-208">In contrast, user interaction with ASP.NET MVC applications is organized around controllers and their action methods.</span></span>

<span data-ttu-id="9e1b1-209">Öte yandan, ASP.NET MVC çerçevesi denetleyicileri olarak başvurulan sınıfların URL'leri eşler.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-209">On the other hand, ASP.NET MVC framework maps URLs to classes that are referred to as controllers.</span></span> <span data-ttu-id="9e1b1-210">Denetleyicileri gelen istekleri işleyemedi, kullanıcı girişini ve etkileşimler, uygun uygulama mantığı yürütün ve istemciye geri gönderilecek yanıt belirlemek (HTML görüntülemek, dosya indirme, farklı bir URL vb. için yeniden yönlendirme).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-210">Controllers process incoming requests, handle user input and interactions, execute appropriate application logic and determine the response to send back to the client (display HTML, download a file, redirect to a different URL, etc.).</span></span> <span data-ttu-id="9e1b1-211">Denetleyici sınıfı HTML görüntüleme söz konusu olduğunda, isteği için HTML biçimlendirmesi oluşturmak için ayrı görünümü bileşen genellikle çağırır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-211">In the case of displaying HTML, a controller class typically calls a separate view component to generate the HTML markup for the request.</span></span> <span data-ttu-id="9e1b1-212">Bir MVC uygulamasında, görünüm yalnızca bilgileri görüntüler; Denetleyici, işler ve kullanıcı girişini ve etkileşimini yanıt verir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-212">In an MVC application, the view only displays information; the controller handles and responds to user input and interaction.</span></span>

<span data-ttu-id="9e1b1-213">Bu görevde, giriş sayfasına müzik Store sitesinin URL'leri işleyecek bir denetleyici sınıfı ekler.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-213">In this task, you will add a Controller class that will handle URLs to the Home page of the Music Store site.</span></span>

1. <span data-ttu-id="9e1b1-214">Sağ **denetleyicileri** klasör Çözüm Gezgini'nde, seçme içinde **Ekle** ardından **denetleyicisi** komutu:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-214">Right-click **Controllers** folder within the Solution Explorer, select **Add** and then **Controller** command:</span></span>

    <span data-ttu-id="9e1b1-215">![Denetleyici komut ekleme](aspnet-mvc-4-fundamentals/_static/image5.png "denetleyicisi komut ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-215">![Add a Controller Command](aspnet-mvc-4-fundamentals/_static/image5.png "Add a Controller Command")</span></span>

    <span data-ttu-id="9e1b1-216">*Denetleyici komut ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-216">*Add Controller Command*</span></span>
2. <span data-ttu-id="9e1b1-217">**Denetleyici Ekle** iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-217">The **Add Controller** dialog appears.</span></span> <span data-ttu-id="9e1b1-218">Denetleyici adı *HomeController* basın **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-218">Name the controller *HomeController* and press **Add**.</span></span>

    <span data-ttu-id="9e1b1-219">![Denetleyici Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image6.png "denetleyici Ekle iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-219">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image6.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="9e1b1-220">*Denetleyici Ekle iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-220">*Add Controller Dialog*</span></span>
3. <span data-ttu-id="9e1b1-221">Dosya **HomeController.cs** oluşturulur **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-221">The file **HomeController.cs** is created in the **Controllers** folder.</span></span> <span data-ttu-id="9e1b1-222">Sahip olmak için **HomeController** değiştirin, dizin eylem üzerinde bir dize döndürecek **dizin** yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-222">In order to have the **HomeController** return a string on its Index action, replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="9e1b1-223">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex1 HomeController dizin*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-223">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex1 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="9e1b1-224">Görev 4 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-224">Task 4 - Running the Application</span></span>

<span data-ttu-id="9e1b1-225">Bu görevde, uygulama bir web tarayıcısında çalışır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-225">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="9e1b1-226">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-226">Press **F5** to run the Application.</span></span> <span data-ttu-id="9e1b1-227">Proje derlendiğinde ve yerel IIS Web sunucusu başlatır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-227">The project is compiled and the local IIS Web Server starts.</span></span> <span data-ttu-id="9e1b1-228">Yerel IIS Web sunucusu otomatik Web sunucusunun URL'sine bir web tarayıcısı açılır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-228">The local IIS Web Server will automatically open a web browser pointing to the URL of the Web server.</span></span>

    <span data-ttu-id="9e1b1-229">![Bir web tarayıcısında çalışan uygulama](aspnet-mvc-4-fundamentals/_static/image7.png "bir web tarayıcısında çalışan uygulama")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-229">![Application running in a web browser](aspnet-mvc-4-fundamentals/_static/image7.png "Application running in a web browser")</span></span>

    <span data-ttu-id="9e1b1-230">*Bir web tarayıcısında çalışan uygulama*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-230">*Application running in a web browser*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9e1b1-231">Yerel IIS Web sunucusunda bir rastgele ücretsiz bağlantı noktası numarası Web sitesinin çalışır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-231">The local IIS Web Server will run the website on a random free port number.</span></span> <span data-ttu-id="9e1b1-232">Yukarıdaki şekilde, sitesinin çalışır durumda `http://localhost:50103/`, bağlantı noktası 50103 kullanıyor.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-232">In the figure above, the site is running at `http://localhost:50103/`, so it's using port 50103.</span></span> <span data-ttu-id="9e1b1-233">Bağlantı noktası numaranızı farklılık gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-233">Your port number may vary.</span></span>
2. <span data-ttu-id="9e1b1-234">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-234">Close the browser.</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a><span data-ttu-id="9e1b1-235">Alıştırma 2: Denetleyici oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-235">Exercise 2: Creating a Controller</span></span>

<span data-ttu-id="9e1b1-236">Bu alıştırmada, müzik Store uygulamasının basit işlevselliği uygulamak için denetleyici güncelleştirme öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-236">In this exercise, you will learn how to update the controller to implement simple functionality of the Music Store application.</span></span> <span data-ttu-id="9e1b1-237">Söz konusu denetleyici eylem yöntemleri, her biri aşağıdaki belirli isteklerini işlemek için tanımlar:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-237">That controller will define action methods to handle each of the following specific requests:</span></span>

- <span data-ttu-id="9e1b1-238">Müzik Store içinde müzik türleri listesi sayfası</span><span class="sxs-lookup"><span data-stu-id="9e1b1-238">A listing page of the music genres in the Music Store</span></span>
- <span data-ttu-id="9e1b1-239">Tüm için belirli bir türe müzik albümleri listeleyen bir göz atma sayfasından</span><span class="sxs-lookup"><span data-stu-id="9e1b1-239">A browse page that lists all of the music albums for a particular genre</span></span>
- <span data-ttu-id="9e1b1-240">Belirli bir müzik Albüm hakkında bilgi gösteren Ayrıntılar sayfası</span><span class="sxs-lookup"><span data-stu-id="9e1b1-240">A details page that shows information about a specific music album</span></span>

<span data-ttu-id="9e1b1-241">Bu alıştırmada kapsam için bu eylemleri yalnızca artık bir dize döndürür.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-241">For the scope of this exercise, those actions will simply return a string by now.</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a><span data-ttu-id="9e1b1-242">Görev 1 - yeni StoreController sınıf ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-242">Task 1 - Adding a New StoreController Class</span></span>

<span data-ttu-id="9e1b1-243">Bu görevde, yeni bir denetleyici ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-243">In this task, you will add a new Controller.</span></span>

1. <span data-ttu-id="9e1b1-244">Açık değilse, başlangıç **VS Express Web 2012**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-244">If not already open, start **VS Express for Web 2012**.</span></span>
2. <span data-ttu-id="9e1b1-245">İçinde **dosya** menüsünde seçin **Proje Aç**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-245">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="9e1b1-246">Proje Aç iletişim kutusunda, Gözat **Source\Ex02 CreatingAController\Begin**seçin **Begin.sln** tıklatıp **açık**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-246">In the Open Project dialog, browse to **Source\Ex02-CreatingAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="9e1b1-247">Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-247">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="9e1b1-248">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-248">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9e1b1-249">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-249">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9e1b1-250">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-250">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9e1b1-251">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-251">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9e1b1-252">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-252">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9e1b1-253">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-253">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9e1b1-254">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-254">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="9e1b1-255">Yeni denetleyici ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-255">Add the new controller.</span></span> <span data-ttu-id="9e1b1-256">Bunu yapmak için sağ **denetleyicileri** klasör Çözüm Gezgini'nde, seçme içinde **Ekle** ardından **denetleyicisi** komutu.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-256">To do this, right-click the **Controllers** folder within the Solution Explorer, select **Add** and then the **Controller** command.</span></span> <span data-ttu-id="9e1b1-257">Değişiklik **Denetleyici adı** için *StoreController*, tıklatıp **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-257">Change the **Controller Name** to *StoreController*, and click **Add**.</span></span>

    <span data-ttu-id="9e1b1-258">![Denetleyici Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image8.png "denetleyici Ekle iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-258">![Add Controller Dialog](aspnet-mvc-4-fundamentals/_static/image8.png "Add Controller Dialog")</span></span>

    <span data-ttu-id="9e1b1-259">*Denetleyici Ekle iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-259">*Add Controller Dialog*</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a><span data-ttu-id="9e1b1-260">Görev 2 - StoreController'ın eylemleri değiştirme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-260">Task 2 - Modifying the StoreController's Actions</span></span>

<span data-ttu-id="9e1b1-261">Bu görevde, çağrılan denetleyici yöntemlerinde değiştirecek **eylemleri**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-261">In this task, you will modify the Controller methods that are called **actions**.</span></span> <span data-ttu-id="9e1b1-262">Eylemler, URL isteklerini işleme ve tarayıcı veya URL çağrılan kullanıcı geri gönderilmesi gereken içeriği belirleyen sorumludur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-262">Actions are responsible for handling URL requests and determining the content that should be sent back to the browser or user that invoked the URL.</span></span>

1. <span data-ttu-id="9e1b1-263">**StoreController** sınıfı zaten bir **dizin** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-263">The **StoreController** class already has an **Index** method.</span></span> <span data-ttu-id="9e1b1-264">Müzik deposu tüm türleri listeler sayfası uygulamak için daha sonra bu laboratuvarda bu kullanır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-264">You will use it later in this Lab to implement the page that lists all genres of the music store.</span></span> <span data-ttu-id="9e1b1-265">Şimdilik yalnızca değiştirin **dizin** yöntemini aşağıdaki kodla bir dize döndürür &quot;Hello Store.Index() gelen&quot;:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-265">For now, just replace the **Index** method with the following code that returns a string &quot;Hello from Store.Index()&quot;:</span></span>

    <span data-ttu-id="9e1b1-266">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex2 StoreController dizin*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-266">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. <span data-ttu-id="9e1b1-267">Ekleme **Gözat** ve **ayrıntıları** yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-267">Add **Browse** and **Details** methods.</span></span> <span data-ttu-id="9e1b1-268">Bunu yapmak için aşağıdaki kodu ekleyin. **StoreController**:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-268">To do this, add the following code to the **StoreController**:</span></span>

    <span data-ttu-id="9e1b1-269">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex2 StoreController BrowseAndDetails*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-269">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex2 StoreController BrowseAndDetails*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="9e1b1-270">Görev 3 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-270">Task 3 - Running the Application</span></span>

<span data-ttu-id="9e1b1-271">Bu görevde, uygulama bir web tarayıcısında çalışır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-271">In this task, you will try out the Application in a web browser.</span></span>

1. <span data-ttu-id="9e1b1-272">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-272">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9e1b1-273">Proje başlar **giriş** sayfası.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-273">The project starts in the **Home** page.</span></span> <span data-ttu-id="9e1b1-274">Her eylemin uygulama doğrulamak için URL'yi değiştirin.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-274">Change the URL to verify each action's implementation.</span></span>

    1. <span data-ttu-id="9e1b1-275">**/ Store**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-275">**/Store**.</span></span> <span data-ttu-id="9e1b1-276">Göreceğiniz  **&quot;Hello Store.Index() gelen&quot;**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-276">You will see **&quot;Hello from Store.Index()&quot;**.</span></span>
    2. <span data-ttu-id="9e1b1-277">**/ Store/Gözat**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-277">**/Store/Browse**.</span></span> <span data-ttu-id="9e1b1-278">Göreceğiniz  **&quot;Hello Store.Browse() gelen&quot;**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-278">You will see **&quot;Hello from Store.Browse()&quot;**.</span></span>
    3. <span data-ttu-id="9e1b1-279">**/ Store/Details**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-279">**/Store/Details**.</span></span> <span data-ttu-id="9e1b1-280">Göreceğiniz  **&quot;Hello Store.Details() gelen&quot;**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-280">You will see **&quot;Hello from Store.Details()&quot;**.</span></span>

        <span data-ttu-id="9e1b1-281">![StoreBrowse gözatma](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse gözatma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-281">![Browsing StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "Browsing StoreBrowse")</span></span>

        <span data-ttu-id="9e1b1-282">*Gözatma /Store/Browse*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-282">*Browsing /Store/Browse*</span></span>
3. <span data-ttu-id="9e1b1-283">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-283">Close the browser.</span></span>

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a><span data-ttu-id="9e1b1-284">Alıştırma 3: bir denetleyiciye parametreleri geçirme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-284">Exercise 3: Passing parameters to a Controller</span></span>

<span data-ttu-id="9e1b1-285">Şimdiye kadar sabit dizelerini denetleyicilerinden iade.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-285">Until now, you have been returning constant strings from the Controllers.</span></span> <span data-ttu-id="9e1b1-286">Bu alıştırmada, parametreleri URL ve sorgu dizesi kullanılarak ve sonra metinle tarayıcıya yanıt yöntemi eylemleri yapma denetleyicisi nasıl geçireceğinizi öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-286">In this exercise you will learn how to pass parameters to a Controller using the URL and querystring, and then making the method actions respond with text to the browser.</span></span>

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a><span data-ttu-id="9e1b1-287">Görev 1 - StoreController Tarz parametresi ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-287">Task 1 - Adding Genre Parameter to StoreController</span></span>

<span data-ttu-id="9e1b1-288">Bu görevde kullanacağınız **querystring** parametreleri göndermek için **Gözat** eylem yönteminde **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-288">In this task, you will use the **querystring** to send parameters to the **Browse** action method in the **StoreController**.</span></span>

1. <span data-ttu-id="9e1b1-289">Açık değilse, başlangıç **Web için VS Express**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-289">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="9e1b1-290">İçinde **dosya** menüsünde seçin **Proje Aç**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-290">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="9e1b1-291">Proje Aç iletişim kutusunda, Gözat **Source\Ex03 PassingParametersToAController\Begin**seçin **Begin.sln** tıklatıp **açık**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-291">In the Open Project dialog, browse to **Source\Ex03-PassingParametersToAController\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="9e1b1-292">Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-292">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="9e1b1-293">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-293">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9e1b1-294">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-294">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9e1b1-295">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-295">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9e1b1-296">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-296">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9e1b1-297">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-297">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9e1b1-298">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-298">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9e1b1-299">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-299">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="9e1b1-300">Açık **StoreController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-300">Open **StoreController** class.</span></span> <span data-ttu-id="9e1b1-301">Bunu yapmak için **Çözüm Gezgini**, genişletme **denetleyicileri** klasörü ve çift **StoreController.cs**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-301">To do this, in the **Solution Explorer**, expand the **Controllers** folder and double-click **StoreController.cs**.</span></span>
4. <span data-ttu-id="9e1b1-302">Değişiklik **Gözat** için belirli bir türe istemek için bir dize parametresi ekleme yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-302">Change the **Browse** method, adding a string parameter to request for a specific genre.</span></span> <span data-ttu-id="9e1b1-303">ASP.NET MVC otomatik olarak herhangi bir sorgu dizesi geçti veya form gönderme parametreleri adlı **Tarz** çağrıldığında Bu eylem yöntemine.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-303">ASP.NET MVC will automatically pass any querystring or form post parameters named **genre** to this action method when invoked.</span></span> <span data-ttu-id="9e1b1-304">Bunu yapmak için değiştirin **Gözat** yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-304">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="9e1b1-305">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex3 StoreController BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-305">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="9e1b1-306">Kullanmakta olduğunuz **HttpUtility.HtmlEncode** yardımcı yöntemine görünüme Javascript ekleme gibi bir bağlantı içeren gelen kullanıcıları engeller   **/Store/göz atma? Tarz =&lt;betik&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/SCRIPT&gt;**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-306">You are using the **HttpUtility.HtmlEncode** utility method to prevents users from injecting Javascript into the View with a link like **/Store/Browse?Genre=&lt;script&gt;window.location='[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.</span></span>
> 
> <span data-ttu-id="9e1b1-307">Daha fazla açıklama için lütfen [bu msdn makalesinde](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-307">For further explanation, please visit [this msdn article](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).</span></span>

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a><span data-ttu-id="9e1b1-308">Görev 2 - uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-308">Task 2 - Running the Application</span></span>

<span data-ttu-id="9e1b1-309">Bu görevde, bir web tarayıcısında uygulama denemek ve kullanmak **Tarz** parametresi.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-309">In this task, you will try out the Application in a web browser and use the **genre** parameter.</span></span>

1. <span data-ttu-id="9e1b1-310">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-310">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9e1b1-311">Proje başlar **giriş** sayfası.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-311">The project starts in the **Home** page.</span></span> <span data-ttu-id="9e1b1-312">URL'yi   */Store/Gözat? Tarz DISCO =* eylemi Tarz parametresi aldığını doğrulamak için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-312">Change the URL to */Store/Browse?Genre=Disco* to verify that the action receives the genre parameter.</span></span>

    <span data-ttu-id="9e1b1-313">![StoreBrowseGenre gözatma DISCO =](aspnet-mvc-4-fundamentals/_static/image10.png "StoreBrowseGenre gözatma DISCO =")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-313">![Browsing StoreBrowseGenre=Disco](aspnet-mvc-4-fundamentals/_static/image10.png "Browsing StoreBrowseGenre=Disco")</span></span>

    <span data-ttu-id="9e1b1-314">*Gözatma /Store/Browse? Tarz DISCO =*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-314">*Browsing /Store/Browse?Genre=Disco*</span></span>
3. <span data-ttu-id="9e1b1-315">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-315">Close the browser.</span></span>

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a><span data-ttu-id="9e1b1-316">Görev 3 - URL'de katıştırılmış bir kimliği parametre ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-316">Task 3 - Adding an Id Parameter Embedded in the URL</span></span>

<span data-ttu-id="9e1b1-317">Bu görevde kullanacağınız **URL** geçirilecek bir **kimliği** parametresi **ayrıntıları** eylem yöntemi **StoreController**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-317">In this task, you will use the **URL** to pass an **Id** parameter to the **Details** action method of the **StoreController**.</span></span> <span data-ttu-id="9e1b1-318">ASP.NET MVC'nin varsayılan yönlendirme kuralı olan bir URL kesimi sonra eylem yöntemi adının adlı bir parametre olarak değerlendirilecek **kimliği**. Eylem yönteminizin kimliği adlı parametre varsa, sonra ASP.NET MVC otomatik olarak URL kesimi, parametre olarak geçirir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-318">ASP.NET MVC's default routing convention is to treat the segment of a URL after the action method name as a parameter named **Id**. If your action method has parameter named Id, then ASP.NET MVC will automatically pass the URL segment to you as a parameter.</span></span> <span data-ttu-id="9e1b1-319">URL'deki **Store/Ayrıntılar/5**, **kimliği** olarak yorumlanacaktır **5**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-319">In the URL **Store/Details/5**, **Id** will be interpreted as **5**.</span></span>

1. <span data-ttu-id="9e1b1-320">Değişiklik **ayrıntıları** yöntemi **StoreController**, ekleyerek bir **int** adlı parametreyi **kimliği**. Bunu yapmak için değiştirin **ayrıntıları** yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-320">Change the **Details** method of the **StoreController**, adding an **int** parameter called **id**. To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="9e1b1-321">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex3 StoreController DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-321">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex3 StoreController DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a><span data-ttu-id="9e1b1-322">Görev 4 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-322">Task 4 - Running the Application</span></span>

<span data-ttu-id="9e1b1-323">Bu görevde, bir web tarayıcısında uygulama denemek ve kullanmak **kimliği** parametresi.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-323">In this task, you will try out the Application in a web browser and use the **Id** parameter.</span></span>

1. <span data-ttu-id="9e1b1-324">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-324">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9e1b1-325">Proje başlar **giriş** sayfası.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-325">The project starts in the **Home** page.</span></span> <span data-ttu-id="9e1b1-326">URL'yi */Store/Details/5* eylem kimliği parametresi aldığını doğrulamak için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-326">Change the URL to */Store/Details/5* to verify that the action receives the id parameter.</span></span>

    <span data-ttu-id="9e1b1-327">![StoreDetails5 gözatma](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 gözatma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-327">![Browsing StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "Browsing StoreDetails5")</span></span>

    <span data-ttu-id="9e1b1-328">*Gözatma /Store/Details/5*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-328">*Browsing /Store/Details/5*</span></span>

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a><span data-ttu-id="9e1b1-329">Alıştırma 4: bir görünüm oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-329">Exercise 4: Creating a View</span></span>

<span data-ttu-id="9e1b1-330">Şu ana kadar dizeleri denetleyici eylemlerine iade.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-330">So far you have been returning strings from controller actions.</span></span> <span data-ttu-id="9e1b1-331">Denetleyicileri nasıl çalıştığını anlamak için kullanışlı bir yol olan rağmen değil gerçek Web uygulamalarınızı nasıl oluşturulur açıktır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-331">Although that is a useful way of understanding how controllers work, it is not how your real Web applications are built.</span></span> <span data-ttu-id="9e1b1-332">Görünümleri kullanarak şablon dosyaları tarayıcıya HTML oluşturmak için daha iyi bir yaklaşım sunan bileşenlerdir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-332">Views are components that provide a better approach for generating HTML back to the browser with the use of template files.</span></span>

<span data-ttu-id="9e1b1-333">Bu alıştırmada, ortak HTML içeriğini, görünümünü site ve son olarak HTML döndürülecek HomeController etkinleştirmek için bir görünüm şablonu geliştirmek için bir stil sayfası için bir şablon kurmak için bir düzen ana sayfasını nasıl ekleneceğini öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-333">In this exercise you will learn how to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel of the site and finally a View template to enable HomeController to return HTML.</span></span>

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a><span data-ttu-id="9e1b1-334">Görev 1 - dosyasını değiştirerek \_layout.cshtml</span><span class="sxs-lookup"><span data-stu-id="9e1b1-334">Task 1 - Modifying the file \_layout.cshtml</span></span>

<span data-ttu-id="9e1b1-335">Dosya **~/Views/Shared/\_layout.cshtml** tüm Web sitesi kullanmak üzere ortak HTML için bir şablon kurulum olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-335">The file **~/Views/Shared/\_layout.cshtml** allows you to setup a template for common HTML to use across the entire website.</span></span> <span data-ttu-id="9e1b1-336">Bu görevde giriş sayfası ve Store alanına bağlantılarla birlikte ortak bir üst bilgisi düzeni ana sayfayla ekleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-336">In this task you will add a layout master page with a common header with links to the Home page and Store area.</span></span>

1. <span data-ttu-id="9e1b1-337">Açık değilse, başlangıç **Web için VS Express**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-337">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="9e1b1-338">İçinde **dosya** menüsünde seçin **Proje Aç**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-338">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="9e1b1-339">Proje Aç iletişim kutusunda, Gözat **Source\Ex04 CreatingAView\Begin**seçin **Begin.sln** tıklatıp **açık**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-339">In the Open Project dialog, browse to **Source\Ex04-CreatingAView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="9e1b1-340">Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-340">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="9e1b1-341">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-341">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9e1b1-342">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-342">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9e1b1-343">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-343">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9e1b1-344">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-344">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9e1b1-345">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-345">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9e1b1-346">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-346">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9e1b1-347">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-347">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="9e1b1-348">Dosya  <strong>\_layout.cshtml</strong> sitedeki tüm sayfalar için HTML kapsayıcı düzeni içerir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-348">The file <strong>\_layout.cshtml</strong> contains the HTML container layout for all pages on the site.</span></span> <span data-ttu-id="9e1b1-349">İçerdiği <strong>&lt;html&gt;</strong> öğe için HTML yanıtını yanı sıra <strong>&lt;baş&gt;</strong> ve <strong>&lt;gövdesi&gt;</strong> öğeleri.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-349">It includes the <strong>&lt;html&gt;</strong> element for the HTML response, as well as the <strong>&lt;head&gt;</strong> and <strong>&lt;body&gt;</strong> elements.</span></span> <span data-ttu-id="9e1b1-350"><strong>@RenderBody()</strong> HTML'yi gövdesi tanımlamak bölgeleri şablonları oluşturabileceksiniz oturum dinamik içerik doldurmak bu görünümü.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-350"><strong>@RenderBody()</strong> within the HTML body identify regions that view templates will be able to fill in with dynamic content.</span></span>
   <span data-ttu-id="9e1b1-351">(C#)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-351">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. <span data-ttu-id="9e1b1-352">Giriş sayfası ve Store alanına sitesindeki tüm sayfalara bağlantılar içeren ortak bir üst bilgisi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-352">Add a common header with links to the Home page and Store area on all pages in the site.</span></span> <span data-ttu-id="9e1b1-353">Bunu yapmak için aşağıdaki kodu ekleyin. &lt;gövdesi&gt; deyimi.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-353">In order to do that, add the following code below &lt;body&gt; statement.</span></span>
   <span data-ttu-id="9e1b1-354">(C#)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-354">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. <span data-ttu-id="9e1b1-355">Her sayfanın gövde bölümü oluşturmak için bir sayı içerir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-355">Include a div to render the body section of each page.</span></span> <span data-ttu-id="9e1b1-356">Değiştirin  <strong>@RenderBody()</strong> aşağıdaki higlighted kodla: (C#)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-356">Replace <strong>@RenderBody()</strong> with the following higlighted code: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > <span data-ttu-id="9e1b1-357">Biliyor muydunuz?</span><span class="sxs-lookup"><span data-stu-id="9e1b1-357">Did you know?</span></span> <span data-ttu-id="9e1b1-358">Visual Studio 2012 HTML, kod dosyaları ve daha sık kullanılan kodu eklemek kolaylaştıran parçacıkları var!</span><span class="sxs-lookup"><span data-stu-id="9e1b1-358">Visual Studio 2012 has snippets that make it easy to add commonly used code in HTML, code files and more!</span></span> <span data-ttu-id="9e1b1-359">Out yazarak deneme **&lt;div&gt;** tuşuna basarak **sekmesini** iki kez tam eklemek için **div** etiketi.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-359">Try it out by typing **&lt;div&gt;** and pressing **TAB** twice to insert a complete **div** tag.</span></span>

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a><span data-ttu-id="9e1b1-360">Görev 2 - CSS stil ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-360">Task 2 - Adding CSS Stylesheet</span></span>

<span data-ttu-id="9e1b1-361">Boş proje şablonu, yalnızca temel formlar ve doğrulama iletileri görüntülemek için kullanılan stilleri içeren çok kolaylaştırılmış bir CSS dosyası içerir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-361">The empty project template includes a very streamlined CSS file which just includes styles used to display basic forms and validation messages.</span></span> <span data-ttu-id="9e1b1-362">Site Azure görünümü ve deneyimini geliştirmek için ek CSS ve görüntüleri (büyük olasılıkla bir tasarımcı tarafından sağlanan) kullanır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-362">You will use additional CSS and images (potentially provided by a designer) in order to enhance the look and feel of the site.</span></span>

<span data-ttu-id="9e1b1-363">Bu görevde, site stillerini tanımlamak için bir CSS stil sayfası ekler.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-363">In this task, you will add a CSS stylesheet to define the styles of the site.</span></span>

1. <span data-ttu-id="9e1b1-364">CSS dosyası ve görüntüler dahil edilen **Source\Assets\Content** , bu Laboratuvarın klasör.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-364">The CSS file and images are included in the **Source\Assets\Content** folder of this Lab.</span></span> <span data-ttu-id="9e1b1-365">Uygulamanıza bunları eklemek için bunların içeriğini sürükleyin bir **Windows Explorer** penceresine **Çözüm Gezgini** aşağıda gösterildiği gibi Web için Visual Studio Express'te:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-365">In order to add them to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Studio Express for Web, as shown below:</span></span>

    <span data-ttu-id="9e1b1-366">![Stil içeriğini sürükleyerek](aspnet-mvc-4-fundamentals/_static/image12.png "stil içeriğini sürükleme")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-366">![Dragging style contents](aspnet-mvc-4-fundamentals/_static/image12.png "Dragging style contents")</span></span>

    <span data-ttu-id="9e1b1-367">*Stil içeriğini sürükleme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-367">*Dragging style contents*</span></span>
2. <span data-ttu-id="9e1b1-368">Bir uyarı iletişim kutusu, onay değiştirilecek istenecektir **Site.css** dosyası ve bazı mevcut görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-368">A warning dialog will appear, asking for confirmation to replace **Site.css** file and some existing images.</span></span> <span data-ttu-id="9e1b1-369">Denetleme **tüm öğelere uygula** tıklatıp **Evet**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-369">Check **Apply to all items** and click **Yes**.</span></span>

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a><span data-ttu-id="9e1b1-370">Görev 3 - bir görünüm şablonu ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-370">Task 3 - Adding a View Template</span></span>

<span data-ttu-id="9e1b1-371">Bu görevde, şablonu Düzen ana sayfası kullanacağınız HTML yanıtı oluşturmak için görüntüleme ekleyecek ve bu alıştırmada, CSS eklendi.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-371">In this task, you will add a View template to generate the HTML response that will use the layout master page and CSS added in this exercise.</span></span>

1. <span data-ttu-id="9e1b1-372">Sitenin ana sayfasını göz atarken görünüm şablonu kullanmak için önce bir dizeyi döndürmek yerine belirtmek gerekir **HomeController dizin** yöntemi döndürür bir **görünümü**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-372">To use a View template when browsing the site's home page, you will first need to indicate that instead of returning a string, the **HomeController Index** method will return a **View**.</span></span> <span data-ttu-id="9e1b1-373">Açık **HomeController** sınıfı ve değiştirmek, **dizin** döndürülecek yöntemi bir **actionresult öğesini**, ve dönüş **View()**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-373">Open **HomeController** class and change its **Index** method to return an **ActionResult**, and have it return **View()**.</span></span>

    <span data-ttu-id="9e1b1-374">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex4 HomeController dizin*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-374">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex4 HomeController Index*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. <span data-ttu-id="9e1b1-375">Şimdi, uygun bir şablonu görüntüleme eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-375">Now, you need to add an appropriate View template.</span></span> <span data-ttu-id="9e1b1-376">Bunu yapmak için **sağ** içinde **dizin** eylem yöntemini seçip alt **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-376">To do this, **right-click** inside the **Index** action method and select **Add View**.</span></span> <span data-ttu-id="9e1b1-377">Bu getirir **Görünüm Ekle** iletişim.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-377">This will bring up the **Add View** dialog.</span></span>

    <span data-ttu-id="9e1b1-378">![Index yöntemi içinden bir görünümle ekleme](aspnet-mvc-4-fundamentals/_static/image13.png "dizin metodundaki bir görünümden ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-378">![Adding a View from within the Index method](aspnet-mvc-4-fundamentals/_static/image13.png "Adding a View from within the Index method")</span></span>

    <span data-ttu-id="9e1b1-379">*Index yöntemi içinden bir görünümle ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-379">*Adding a View from within the Index method*</span></span>
3. <span data-ttu-id="9e1b1-380">**Görünüm Ekle** görünümü şablon dosyası oluşturmak için iletişim kutusu görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-380">The **Add View** Dialog will appear to generate a View template file.</span></span> <span data-ttu-id="9e1b1-381">Varsayılan olarak, bu iletişim kutusunu görüntüle şablonunun adını kullanacak olan eylem yönteminin eşleşmesi önceden doldurur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-381">By default, this dialog pre-populates the name of the View template so that it matches the action method that will use it.</span></span> <span data-ttu-id="9e1b1-382">Nedeniyle **Görünüm Ekle** bağlam menüsü içinde **dizin** HomeController içinde eylem yönteminin **Görünüm Ekle** iletişim varsayılan görünüm adını dizinine sahip.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-382">Because you used the **Add View** context menu within the **Index** action method within the HomeController, the **Add View** dialog has Index as the default view name.</span></span> <span data-ttu-id="9e1b1-383">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-383">Click **Add**.</span></span>

    <span data-ttu-id="9e1b1-384">![Görünüm Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image14.png "Görünüm Ekle iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-384">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image14.png "Add View Dialog")</span></span>

    <span data-ttu-id="9e1b1-385">*Görünüm Ekle iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-385">*Add View Dialog*</span></span>
4. <span data-ttu-id="9e1b1-386">Visual Studio'nun oluşturduğu bir **Index.cshtml** görünümü şablon içinde **görünümler/giriş** klasörü ve onu açar.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-386">Visual Studio generates an **Index.cshtml** view template inside the **Views\Home** folder and then opens it.</span></span>

    <span data-ttu-id="9e1b1-387">![Oluşturulan dizini görünüm ev](aspnet-mvc-4-fundamentals/_static/image15.png "oluşturulan giriş dizini görünümü")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-387">![Home Index view created](aspnet-mvc-4-fundamentals/_static/image15.png "Home Index view created")</span></span>

    <span data-ttu-id="9e1b1-388">*Oluşturulan giriş dizini görünümü*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-388">*Home Index view created*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9e1b1-389">ad ve konum **Index.cshtml** dosya geçerlidir ve varsayılan ASP.NET MVC adlandırma kurallarını izler.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-389">name and location of the **Index.cshtml** file is relevant and follows the default ASP.NET MVC naming conventions.</span></span>
    > 
    > <span data-ttu-id="9e1b1-390">Klasör \Views\**giriş** denetleyici adıyla eşleşen (**giriş** denetleyicisi).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-390">The folder \Views\**Home** matches the controller name (**Home** Controller).</span></span> <span data-ttu-id="9e1b1-391">Görünüm şablonu adı (**dizin**), görünüm görüntüleme denetleyici eylem yöntemine eşleşir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-391">The View template name (**Index**), matches the controller action method which will be displaying the View.</span></span>
    > 
    > <span data-ttu-id="9e1b1-392">Bu şekilde, bir görünüme dönmek için bu adlandırma kuralını kullanırken adına veya konumuna bir görünüm şablonunun açıkça belirtmek zorunda ASP.NET MVC önler.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-392">This way, ASP.NET MVC avoids having to explicitly specify the name or location of a View template when using this naming convention to return a View.</span></span>
5. <span data-ttu-id="9e1b1-393">Oluşturulan görünüm şablon temel  **\_layout.cshtml** önceden tanımlı bir şablon.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-393">The generated View template is based on the **\_layout.cshtml** template earlier defined.</span></span> <span data-ttu-id="9e1b1-394">ViewBag.Title özelliğini güncelleştirme **giriş**, ana içeriğe değiştirip **bu giriş sayfasıdır**, aşağıdaki kodda gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-394">Update the ViewBag.Title property to **Home**, and change the main content to **This is the Home Page**, as shown in the code below:</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. <span data-ttu-id="9e1b1-395">Seçin **MvcMusicStore** tuşuna basın ve Çözüm Gezgini projedeki **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-395">Select **MvcMusicStore** project in the Solution Explorer and Press **F5** to run the Application.</span></span>

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a><span data-ttu-id="9e1b1-396">Görev 4: doğrulama</span><span class="sxs-lookup"><span data-stu-id="9e1b1-396">Task 4: Verification</span></span>

<span data-ttu-id="9e1b1-397">Önceki alıştırmada tüm adımları doğru olarak yaptığınızdan doğrulamak için aşağıda gösterildiği gibi ilerleyin:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-397">In order to verify that you have correctly performed all the steps in the previous exercise, proceed as follows:</span></span>

<span data-ttu-id="9e1b1-398">Bir tarayıcıda açıldığından uygulama ile birlikte unutmamalısınız:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-398">With the application opened in a browser, you should note that:</span></span>

1. <span data-ttu-id="9e1b1-399">HomeController'ın dizin eylem yöntemi bulunamadı ve görüntülenen **\Views\Home\Index.cshtml** kodu adlı olsa bile görüntülemek şablon **View() dönüş**, ardından şablonu görüntüleme standart adlandırma kuralı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-399">The HomeController's Index action method found and displayed the **\Views\Home\Index.cshtml** View template, even though the code called **return View()**, because the View template followed the standard naming convention.</span></span>
2. <span data-ttu-id="9e1b1-400">Giriş sayfası içinde tanımlanan Hoş Geldiniz iletisi görüntüler **\Views\Home\Index.cshtml** şablonu görüntüle.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-400">The Home Page displays the welcome message defined within the **\Views\Home\Index.cshtml** view template.</span></span>
3. <span data-ttu-id="9e1b1-401">Giriş sayfasını kullanarak  **\_layout.cshtml** şablonu ve HTML standart site düzenine Hoş Geldiniz iletisi bulunur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-401">The Home Page is using the **\_layout.cshtml** template, and so the welcome message is contained within the standard site HTML layout.</span></span>

    <span data-ttu-id="9e1b1-402">![Giriş dizini tanımlı LayoutPage ve stili kullanarak görünümünü](aspnet-mvc-4-fundamentals/_static/image16.png "giriş dizini tanımlı LayoutPage ve stili kullanarak görünümünü")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-402">![Home Index View using the defined LayoutPage and style](aspnet-mvc-4-fundamentals/_static/image16.png "Home Index View using the defined LayoutPage and style")</span></span>

    <span data-ttu-id="9e1b1-403">*Giriş dizini tanımlı LayoutPage ve stili kullanarak görünümünü*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-403">*Home Index View using the defined LayoutPage and style*</span></span>

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a><span data-ttu-id="9e1b1-404">Alıştırma 5: bir görünüm modeli oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-404">Exercise 5: Creating a View Model</span></span>

<span data-ttu-id="9e1b1-405">Şu ana kadar sabit kodlanmış HTML görüntüleme Görünümlerinizde yapılan ancak dinamik web uygulamaları oluşturmak için şablonu görüntüle denetleyicisinden bilgi almanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-405">So far, you made your Views display hardcoded HTML, but, in order to create dynamic web applications, the View template should receive information from the Controller.</span></span> <span data-ttu-id="9e1b1-406">Bu amaçla kullanılmak üzere bir yaygın bir tekniktir **ViewModel** deseni, denetleyicinin uygun HTML yanıtı oluşturmak için gereken tüm bilgiler paketini sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-406">One common technique to be used for that purpose is the **ViewModel** pattern, which allows a Controller to package up all the information needed to generate the appropriate HTML response.</span></span>

<span data-ttu-id="9e1b1-407">Bu alıştırmada ViewModel sınıfı oluşturun ve gerekli özellikleri ekleme hakkında bilgi edineceksiniz: depolama ve bu tür listesi türleri sayısı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-407">In this exercise, you will learn how to create a ViewModel class and add the required properties: the number of genres in the store and a list of those genres.</span></span> <span data-ttu-id="9e1b1-408">Ayrıca oluşturulan ViewModel kullanılacak StoreController güncelleştirir ve son olarak, belirtilen özellikleri sayfasında görüntülenecek yeni bir görünüm şablonu oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-408">You will also update the StoreController to use the created ViewModel, and finally, you will create a new View template that will display the mentioned properties in the page.</span></span>

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a><span data-ttu-id="9e1b1-409">Görev 1 - ViewModel sınıfı oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-409">Task 1 - Creating a ViewModel Class</span></span>

<span data-ttu-id="9e1b1-410">Bu görevde, Store Tarz listeleme senaryosu uygulayan bir ViewModel sınıfı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-410">In this task, you will create a ViewModel class that will implement the Store genre listing scenario.</span></span>

1. <span data-ttu-id="9e1b1-411">Açık değilse, başlangıç **Web için VS Express**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-411">If not already open, start **VS Express for Web**.</span></span>
2. <span data-ttu-id="9e1b1-412">İçinde **dosya** menüsünde seçin **Proje Aç**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-412">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="9e1b1-413">Proje Aç iletişim kutusunda, Gözat **Source\Ex05 CreatingAViewModel\Begin**seçin **Begin.sln** tıklatıp **açık**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-413">In the Open Project dialog, browse to **Source\Ex05-CreatingAViewModel\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="9e1b1-414">Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-414">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="9e1b1-415">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-415">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9e1b1-416">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-416">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9e1b1-417">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-417">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9e1b1-418">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-418">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9e1b1-419">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-419">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9e1b1-420">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-420">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9e1b1-421">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-421">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="9e1b1-422">Oluşturma bir **Viewmodel'lar** ViewModel tutmak için klasör.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-422">Create a **ViewModels** folder to hold the ViewModel.</span></span> <span data-ttu-id="9e1b1-423">Bunu yapmak için sağ üst düzey **MvcMusicStore** proje, select **Ekle** ve ardından **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-423">To do this, right-click the top-level **MvcMusicStore** project, select **Add** and then **New Folder**.</span></span>

    <span data-ttu-id="9e1b1-424">![Yeni bir klasör ekleme](aspnet-mvc-4-fundamentals/_static/image17.png "yeni bir klasör ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-424">![Adding a new folder](aspnet-mvc-4-fundamentals/_static/image17.png "Adding a new folder")</span></span>

    <span data-ttu-id="9e1b1-425">*Yeni bir klasör ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-425">*Adding a new folder*</span></span>
4. <span data-ttu-id="9e1b1-426">Klasör adı *Viewmodel'lar*.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-426">Name the folder *ViewModels*.</span></span>

    <span data-ttu-id="9e1b1-427">![Çözüm Gezgini'nde Viewmodel'lar klasör](aspnet-mvc-4-fundamentals/_static/image18.png "Viewmodel'lar Çözüm Gezgininde klasör")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-427">![ViewModels folder in Solution Explorer](aspnet-mvc-4-fundamentals/_static/image18.png "ViewModels folder in Solution Explorer")</span></span>

    <span data-ttu-id="9e1b1-428">*Çözüm Gezgini'nde Viewmodel'lar klasörü*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-428">*ViewModels folder in Solution Explorer*</span></span>
5. <span data-ttu-id="9e1b1-429">Oluşturma bir **ViewModel** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-429">Create a **ViewModel** class.</span></span> <span data-ttu-id="9e1b1-430">Bunu yapmak için sağ **Viewmodel'lar** kısa süre önce oluşturduğunuz klasör seç **Ekle** ardından **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-430">To do this, right-click on the **ViewModels** folder recently created, select **Add** and then **New Item**.</span></span> <span data-ttu-id="9e1b1-431">Altında **kod**, seçin **sınıfı** öğe ve dosya adı *StoreIndexViewModel.cs*, ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-431">Under **Code**, choose the **Class** item and name the file *StoreIndexViewModel.cs*, then click **Add**.</span></span>

    <span data-ttu-id="9e1b1-432">![Yeni sınıf ekleme](aspnet-mvc-4-fundamentals/_static/image19.png "yeni sınıf ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-432">![Adding a new Class](aspnet-mvc-4-fundamentals/_static/image19.png "Adding a new Class")</span></span>

    <span data-ttu-id="9e1b1-433">*Yeni sınıf ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-433">*Adding a new Class*</span></span>

    <span data-ttu-id="9e1b1-434">![StoreIndexViewModel sınıfı oluşturma](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel oluşturma sınıfı")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-434">![Creating StoreIndexViewModel class](aspnet-mvc-4-fundamentals/_static/image20.png "Creating StoreIndexViewModel class")</span></span>

    <span data-ttu-id="9e1b1-435">*StoreIndexViewModel sınıfı oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-435">*Creating StoreIndexViewModel class*</span></span>

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a><span data-ttu-id="9e1b1-436">Görev 2 - ViewModel sınıfı özellik ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-436">Task 2 - Adding Properties to the ViewModel class</span></span>

<span data-ttu-id="9e1b1-437">StoreController görünümü şablona beklenen HTML yanıtını oluşturmak için geçirilecek iki parametre vardır: depolama ve bu tür listesi türleri sayısı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-437">There are two parameters to be passed from the StoreController to the View template in order to generate the expected HTML response: the number of genres in the store and a list of those genres.</span></span>

<span data-ttu-id="9e1b1-438">Bu görevde, 2 özelliklere ekleyeceksiniz **StoreIndexViewModel** sınıfı: **NumberOfGenres** (tamsayı) ve **türleri** (dizelerinin listesini).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-438">In this task, you will add those 2 properties to the **StoreIndexViewModel** class: **NumberOfGenres** (an integer) and **Genres** (a list of strings).</span></span>

1. <span data-ttu-id="9e1b1-439">Ekleme **NumberOfGenres** ve **türleri** özelliklerine **StoreIndexViewModel** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-439">Add **NumberOfGenres** and **Genres** properties to the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="9e1b1-440">Bunu yapmak için aşağıdaki 2 satırları sınıf tanımına ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-440">To do this, add the following 2 lines to the class definition:</span></span>

    <span data-ttu-id="9e1b1-441">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex5 StoreIndexViewModel özellikleri*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-441">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel properties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> <span data-ttu-id="9e1b1-442">**{Alın; ayarlayın;}**  gösterimi yapar kullanmak C# otomatik uygulanan özellikler özelliği.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-442">The **{ get; set; }** notation makes use of C#'s auto-implemented properties feature.</span></span> <span data-ttu-id="9e1b1-443">Bir özelliğin avantajlarından, bize destek alanı bildirmek gerek kalmadan sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-443">It provides the benefits of a property without requiring us to declare a backing field.</span></span>

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a><span data-ttu-id="9e1b1-444">Görev 3 - güncelleştirme StoreController StoreIndexViewModel kullanmak için</span><span class="sxs-lookup"><span data-stu-id="9e1b1-444">Task 3 - Updating StoreController to use the StoreIndexViewModel</span></span>

<span data-ttu-id="9e1b1-445">**StoreIndexViewModel** sınıfı geçirmek için gereken bilgileri yalıtan **StoreController**'s **dizin** yöntemi için bir yanıt oluşturmak için bir şablonu görüntüleme .</span><span class="sxs-lookup"><span data-stu-id="9e1b1-445">The **StoreIndexViewModel** class encapsulates the information needed to pass from **StoreController**'s **Index** method to a View template in order to generate a response.</span></span>

<span data-ttu-id="9e1b1-446">Bu görevde, güncelleştirecektir **StoreController** kullanılacak **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-446">In this task, you will update the **StoreController** to use the **StoreIndexViewModel**.</span></span>

1. <span data-ttu-id="9e1b1-447">Açık **StoreController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-447">Open **StoreController** class.</span></span>

    <span data-ttu-id="9e1b1-448">![StoreController sınıfı açma](aspnet-mvc-4-fundamentals/_static/image21.png "açılış StoreController sınıfı")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-448">![Opening StoreController class](aspnet-mvc-4-fundamentals/_static/image21.png "Opening StoreController class")</span></span>

    <span data-ttu-id="9e1b1-449">*Açılış StoreController sınıfı*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-449">*Opening StoreController class*</span></span>
2. <span data-ttu-id="9e1b1-450">Kullanmak için **StoreIndexViewModel** gelen sınıfı **StoreController**, aşağıdaki ad üstüne ekleyin **StoreController** kod:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-450">In order to use the **StoreIndexViewModel** class from the **StoreController**, add the following namespace at the top of the **StoreController** code:</span></span>

    <span data-ttu-id="9e1b1-451">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex5 Viewmodel'lar kullanarak StoreIndexViewModel*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-451">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel using ViewModels*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. <span data-ttu-id="9e1b1-452">Değişiklik **StoreController**'s **dizin** BT'nin oluşturur ve doldurur eylem yöntemi bir **StoreIndexViewModel** nesnesi ve ardından bunu bir şablonu görüntüle geçirir devre dışı ile bir HTML yanıtı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-452">Change the **StoreController**'s **Index** action method so that it creates and populates a **StoreIndexViewModel** object and then passes it off to a View template to generate an HTML response with it.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9e1b1-453">Laboratuvardaki &quot;ASP.NET MVC modeller ve veri erişimi&quot; depolama türleri listesini veritabanından alır, kod yazacaksınız.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-453">In Lab &quot;ASP.NET MVC Models and Data Access&quot; you will write code that retrieves the list of store genres from a database.</span></span> <span data-ttu-id="9e1b1-454">Aşağıdaki kodda, oluşturacağınız bir **listesi** doldurur amaçlı olarak sahte veri türleri, **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-454">In the following code, you will create a **List** of dummy data genres that will populate the **StoreIndexViewModel**.</span></span>
    > 
    > <span data-ttu-id="9e1b1-455">Oluşturma ve ayarlama sonra **StoreIndexViewModel** nesnesi, bu geçirilir bağımsız değişkeni olarak **görünümü** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-455">After creating and setting up the **StoreIndexViewModel** object, it will be passed as an argument to the **View** method.</span></span> <span data-ttu-id="9e1b1-456">Bu görünüm şablonu ile bir HTML yanıtı oluşturmak için söz konusu nesne kullanacağını belirtir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-456">This indicates that the View template will use that object to generate an HTML response with it.</span></span>
4. <span data-ttu-id="9e1b1-457">Değiştirin **dizin** yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-457">Replace the **Index** method with the following code:</span></span>

    <span data-ttu-id="9e1b1-458">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex5 StoreController Index yöntemi*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-458">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex5 StoreController Index method*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> <span data-ttu-id="9e1b1-459">C# ile ilk kez kullanıyorsanız kullanmanın varsayabilir **var** anlamına **viewModel** geç bağlanan bir değişkendir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-459">If you're unfamiliar with C#, you may assume that using **var** means that the **viewModel** variable is late-bound.</span></span> <span data-ttu-id="9e1b1-460">Doğru - C# derleyicisi kullanarak değişkene atayın temel tür çıkarımı, belirlemek için değil **viewModel** türünde **StoreIndexViewModel**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-460">That's not correct - the C# compiler is using type-inference based on what you assign to the variable to determine that **viewModel** is of type **StoreIndexViewModel**.</span></span> <span data-ttu-id="9e1b1-461">Ayrıca, yerel derleme tarafından **viewModel** değişken olarak bir **StoreIndexViewModel** get derleme zamanı denetimi ve Visual Studio Kod Düzenleyicisi desteği yazın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-461">Also, by compiling the local **viewModel** variable as a **StoreIndexViewModel** type you get compile-time checking and Visual Studio code-editor support.</span></span>

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a><span data-ttu-id="9e1b1-462">Görev 4 - StoreIndexViewModel kullanan bir görünüm şablonu oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-462">Task 4 - Creating a View Template that Uses StoreIndexViewModel</span></span>

<span data-ttu-id="9e1b1-463">Bu görevde denetleyicisinden geçirilen bir StoreIndexViewModel nesne türleri listesini görüntülemek için kullanacağı bir görünüm şablonu oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-463">In this task, you will create a View template that will use a StoreIndexViewModel object passed from the Controller to display a list of genres.</span></span>

1. <span data-ttu-id="9e1b1-464">Yeni Görünüm şablonu oluşturmadan önce şimdi Projeyi derlemek için **Ekle iletişim kutusunu görüntüle** bildiği **StoreIndexViewModel** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-464">Before creating the new View template, let's build the project so that the **Add View Dialog** knows about the **StoreIndexViewModel** class.</span></span> <span data-ttu-id="9e1b1-465">Seçerek projenizi **derleme** menü öğesini ve ardından **MvcMusicStore yapı**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-465">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>

    <span data-ttu-id="9e1b1-466">![Proje derleme](aspnet-mvc-4-fundamentals/_static/image22.png "projesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-466">![Building the project](aspnet-mvc-4-fundamentals/_static/image22.png "Building the project")</span></span>

    <span data-ttu-id="9e1b1-467">*Proje oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-467">*Building the project*</span></span>
2. <span data-ttu-id="9e1b1-468">Yeni bir görünüm şablonu oluşturun.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-468">Create a new View template.</span></span> <span data-ttu-id="9e1b1-469">İçinde Bunu yapmak için sağ **dizin** yöntemini seçip alt **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-469">To do that, right-click inside the **Index** method and select **Add View**.</span></span>

    <span data-ttu-id="9e1b1-470">![Görünüm ekleme](aspnet-mvc-4-fundamentals/_static/image23.png "görünüm ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-470">![Adding a View](aspnet-mvc-4-fundamentals/_static/image23.png "Adding a View")</span></span>

    <span data-ttu-id="9e1b1-471">*Görünüm Ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-471">*Adding a View*</span></span>
3. <span data-ttu-id="9e1b1-472">Çünkü **Ekle iletişim kutusunu görüntüle** gelen çağrıldı **StoreController**, varsayılan olarak görünüm şablonu ekleyeceksiniz bir **\Views\Store\Index.cshtml** dosya.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-472">Because the **Add View Dialog** was invoked from the **StoreController**, it will add the View template by default in a **\Views\Store\Index.cshtml** file.</span></span> <span data-ttu-id="9e1b1-473">Denetleme **bir türü kesin belirlenmiş-görünüm oluşturma** onay kutusunu seçip **StoreIndexViewModel** olarak **Model sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-473">Check the **Create a strongly-typed-view** checkbox and then select **StoreIndexViewModel** as the **Model class**.</span></span> <span data-ttu-id="9e1b1-474">Ayrıca, seçili görünüm altyapısı olduğundan emin olun **Razor**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-474">Also, make sure that the View engine selected is **Razor**.</span></span> <span data-ttu-id="9e1b1-475">**Ekle**'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-475">Click **Add**.</span></span>

    <span data-ttu-id="9e1b1-476">![Görünüm Ekle iletişim kutusu](aspnet-mvc-4-fundamentals/_static/image24.png "Görünüm Ekle iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-476">![Add View Dialog](aspnet-mvc-4-fundamentals/_static/image24.png "Add View Dialog")</span></span>

    <span data-ttu-id="9e1b1-477">*Görünüm Ekle iletişim kutusu*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-477">*Add View Dialog*</span></span>

    <span data-ttu-id="9e1b1-478">**\Views\Store\Index.cshtml** görünümü şablon dosyası oluşturulup açılır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-478">The **\Views\Store\Index.cshtml** View template file is created and opened.</span></span> <span data-ttu-id="9e1b1-479">Sağlanan bilgilere dayanarak **Görünüm Ekle** iletişim son adımda, şablonu beklediğiniz görünümü bir **StoreIndexViewModel** örnek olarak bir HTML yanıtı oluşturmak için kullanılacak veri.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-479">Based on the information provided to the **Add View** dialog in the last step, the View template will expect a **StoreIndexViewModel** instance as the data to use to generate an HTML response.</span></span> <span data-ttu-id="9e1b1-480">Şablon devralması fark edeceksiniz bir `ViewPage<musicstore.viewmodels.storeindexviewmodel>` C#.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-480">You will notice that the template inherits a `ViewPage<musicstore.viewmodels.storeindexviewmodel>` in C#.</span></span>

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a><span data-ttu-id="9e1b1-481">Görev 5 - görünüm şablonu güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="9e1b1-481">Task 5 - Updating the View Template</span></span>

<span data-ttu-id="9e1b1-482">Bu görevde, türleri ve sayfa içindeki adları sayısını almak için son görevde oluşturduğunuz görünüm şablonu güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-482">In this task, you will update the View template created in the last task to retrieve the number of genres and their names within the page.</span></span>

> [!NOTE]
> <span data-ttu-id="9e1b1-483">Sözdizimi @ kullanır (genellikle olarak adlandırılan &quot;kod nuggets&quot;) görünüm şablonu içindeki kodu çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-483">You will use @ syntax (often referred to as &quot;code nuggets&quot;) to execute code within the View template.</span></span>

1. <span data-ttu-id="9e1b1-484">İçinde **Index.cshtml** içinde dosya **Store** klasöründe kodunu şununla değiştirin:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-484">In the **Index.cshtml** file, within the **Store** folder, replace its code with the following:</span></span>

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. <span data-ttu-id="9e1b1-485">Tarz listesinde üzerinde döngü **StoreIndexViewModel** ve bir HTML oluşturmak **&lt;ul&gt;** kullanarak listesinde bir **foreach** döngü.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-485">Loop over the genre list in **StoreIndexViewModel** and create an HTML **&lt;ul&gt;** list using a **foreach** loop.</span></span>
   <span data-ttu-id="9e1b1-486">(C#)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-486">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. <span data-ttu-id="9e1b1-487">Tuşuna **F5** uygulamayı çalıştırın ve göz atmayı **/Store**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-487">Press **F5** to run the Application and browse **/Store**.</span></span> <span data-ttu-id="9e1b1-488">Geçirilen türleri listesini görürsünüz **StoreIndexViewModel** nesnesinden **StoreController** görünümü şablon.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-488">You will see the list of genres passed in the **StoreIndexViewModel** object from the **StoreController** to the View template.</span></span>

    <span data-ttu-id="9e1b1-489">![Görünüm türleri listesini görüntüleyen](aspnet-mvc-4-fundamentals/_static/image26.png "görünüm türleri listesini görüntüleme")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-489">![View displaying a list of genres](aspnet-mvc-4-fundamentals/_static/image26.png "View displaying a list of genres")</span></span>

    <span data-ttu-id="9e1b1-490">*Görünüm türleri listesini görüntüleme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-490">*View displaying a list of genres*</span></span>
4. <span data-ttu-id="9e1b1-491">Tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-491">Close the browser.</span></span>

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a><span data-ttu-id="9e1b1-492">Alıştırma 6: Görünüm parametrelerini kullanma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-492">Exercise 6: Using Parameters in View</span></span>

<span data-ttu-id="9e1b1-493">Alıştırma 3'te denetleyiciye parametreler öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-493">In Exercise 3 you learned how to pass parameters to the Controller.</span></span> <span data-ttu-id="9e1b1-494">Bu alıştırmada, görünümü şablonunda bu parametreleri kullanmayı öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-494">In this exercise, you will learn how to use those parameters in the View template.</span></span> <span data-ttu-id="9e1b1-495">Bu amaç için önce verileri ve etki alanı mantığınızı yönetmenize yardımcı olacak Model sınıflarına sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-495">For that purpose, you will be introduced first to Model classes that will help you to manage your data and domain logic.</span></span> <span data-ttu-id="9e1b1-496">Buna ek olarak, nesnelerin interneti kodlama URL yolları gibi endişelenmenize gerek kalmadan bir ASP.NET MVC uygulaması içindeki sayfalara bağlantılar oluşturmak nasıl öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-496">Additionally, you will learn how to create links to pages inside the ASP.NET MVC application without worrying of things like URL paths encoding.</span></span>

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a><span data-ttu-id="9e1b1-497">Görev 1 - Model sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-497">Task 1 - Adding Model Classes</span></span>

<span data-ttu-id="9e1b1-498">Yalnızca bilgi denetleyicisinden görünüme iletmek için oluşturulan Viewmodel'lar, Model sınıfları içeren ve veri ve etki alanı mantığı yönetmek için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-498">Unlike ViewModels, which are created just to pass information from the Controller to the View, Model classes are built to contain and manage data and domain logic.</span></span> <span data-ttu-id="9e1b1-499">Bu görevde, bu kavramları göstermek için iki model sınıfları ekleyeceksiniz: **Tarz** ve **albüm**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-499">In this task you will add two model classes to represent these concepts: **Genre** and **Album**.</span></span>

1. <span data-ttu-id="9e1b1-500">Açık değilse, başlangıç **Web için VS Express**</span><span class="sxs-lookup"><span data-stu-id="9e1b1-500">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="9e1b1-501">İçinde **dosya** menüsünde seçin **Proje Aç**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-501">In the **File** menu, choose **Open Project**.</span></span> <span data-ttu-id="9e1b1-502">Proje Aç iletişim kutusunda, Gözat **Source\Ex06 UsingParametersInView\Begin**seçin **Begin.sln** tıklatıp **açık**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-502">In the Open Project dialog, browse to **Source\Ex06-UsingParametersInView\Begin**, select **Begin.sln** and click **Open**.</span></span> <span data-ttu-id="9e1b1-503">Alternatif olarak, önceki alıştırmada tamamladıktan sonra aldığınız çözümüyle devam edebilir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-503">Alternatively, you may continue with the solution that you obtained after completing the previous exercise.</span></span>

   1. <span data-ttu-id="9e1b1-504">Sağlanan açtıysanız **başlamak** çözümü ihtiyaç duyacağınız bazı eksik NuGet paketlerini yüklemek devam etmeden önce.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-504">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="9e1b1-505">Bunu yapmak için tıklatın **proje** menü ve select **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-505">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="9e1b1-506">İçinde **NuGet paketlerini Yönet** iletişim kutusunda, tıklayın **geri** eksik paketleri indirmek için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-506">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="9e1b1-507">Son olarak, tıklayarak çözüm oluşturun **derleme** | **Çözümü Derle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-507">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="9e1b1-508">NuGet kullanmanın yararlarından biri, projenizdeki tüm kitaplıkları göndermeye proje boyutunu küçültmeyi gerekmemesidir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-508">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="9e1b1-509">NuGet güç araçları ile paket sürümlerini Packages.config dosyasında belirterek, gerekli tüm kitaplıkların projeyi Çalıştır ilk kez yüklemeye mümkün olmayacak.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-509">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="9e1b1-510">Bu laboratuvarda varolan bir çözümü açtıktan sonra aşağıdaki adımları çalıştırmanız gerekecek nedeni budur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-510">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
3. <span data-ttu-id="9e1b1-511">Ekleme bir **Tarz** Model sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-511">Add a **Genre** Model class.</span></span> <span data-ttu-id="9e1b1-512">Bunu yapmak için sağ **modelleri** klasöründe **Çözüm Gezgini**seçin **Ekle** ardından **yeni öğe** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-512">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="9e1b1-513">Altında **kod**, seçin **sınıfı** öğe ve dosya adı *Genre.cs*, ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-513">Under **Code**, choose the **Class** item and name the file *Genre.cs*, then click **Add**.</span></span>

    <span data-ttu-id="9e1b1-514">![Sınıf ekleme](aspnet-mvc-4-fundamentals/_static/image27.png "sınıf ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-514">![Adding a class](aspnet-mvc-4-fundamentals/_static/image27.png "Adding a class")</span></span>

    <span data-ttu-id="9e1b1-515">*Yeni bir öğe ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-515">*Adding a new item*</span></span>

    <span data-ttu-id="9e1b1-516">![Tarz Model sınıfı ekleme](aspnet-mvc-4-fundamentals/_static/image28.png "Tarz Model sınıfı ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-516">![Add Genre Model Class](aspnet-mvc-4-fundamentals/_static/image28.png "Add Genre Model Class")</span></span>

    <span data-ttu-id="9e1b1-517">*Tarz Model sınıfı ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-517">*Add Genre Model Class*</span></span>
4. <span data-ttu-id="9e1b1-518">Ekleme bir **adı** Tarz sınıf özelliği.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-518">Add a **Name** property to the Genre class.</span></span> <span data-ttu-id="9e1b1-519">Bunu yapmak için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-519">To do this, add the following code:</span></span>

    <span data-ttu-id="9e1b1-520">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 Tarz*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-520">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Genre*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. <span data-ttu-id="9e1b1-521">Aşağıdaki yordamın aynısını önce bir **albüm** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-521">Following the same procedure as before, add an **Album** class.</span></span> <span data-ttu-id="9e1b1-522">Bunu yapmak için sağ **modelleri** klasöründe **Çözüm Gezgini**seçin **Ekle** ardından **yeni öğe** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-522">To do this, right-click the **Models** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="9e1b1-523">Altında **kod**, seçin **sınıfı** öğe ve dosya adı *Album.cs*, ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-523">Under **Code**, choose the **Class** item and name the file *Album.cs*, then click **Add**.</span></span>
6. <span data-ttu-id="9e1b1-524">İki özellik albüm sınıfa ekleyin: **Tarz** ve **başlık**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-524">Add two properties to the Album class: **Genre** and **Title**.</span></span> <span data-ttu-id="9e1b1-525">Bunu yapmak için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-525">To do this, add the following code:</span></span>

    <span data-ttu-id="9e1b1-526">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 albüm*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-526">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 Album*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a><span data-ttu-id="9e1b1-527">Görev 2 - bir StoreBrowseViewModel ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-527">Task 2 - Adding a StoreBrowseViewModel</span></span>

<span data-ttu-id="9e1b1-528">A **StoreBrowseViewModel** bu görevde, seçilen türe eşleşen albümleri göstermek için kullanılacak.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-528">A **StoreBrowseViewModel** will be used in this task to show the Albums that match a selected Genre.</span></span> <span data-ttu-id="9e1b1-529">Bu görevde, bu sınıf oluşturacak ve ardından işlemek için iki özellik ekleyin **Tarz** ve kendi **albüm**kullanıcının listesi.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-529">In this task, you will create this class and then add two properties to handle the **Genre** and its **Album**'s List.</span></span>

1. <span data-ttu-id="9e1b1-530">Ekleme bir **StoreBrowseViewModel** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-530">Add a **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="9e1b1-531">Bunu yapmak için sağ **Viewmodel'lar** klasöründe **Çözüm Gezgini**seçin **Ekle** ardından **yeni öğe** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-531">To do this, right-click the **ViewModels** folder in the **Solution Explorer**, select **Add** and then the **New Item** option.</span></span> <span data-ttu-id="9e1b1-532">Altında **kod**, seçin **sınıfı** öğe ve dosya adı *StoreBrowseViewModel.cs*, ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-532">Under **Code**, choose the **Class** item and name the file *StoreBrowseViewModel.cs*, then click **Add**.</span></span>
2. <span data-ttu-id="9e1b1-533">Modeller başvuruyu eklemek **StoreBrowseViewModel** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-533">Add a reference to the Models in **StoreBrowseViewModel** class.</span></span> <span data-ttu-id="9e1b1-534">Bunu yapmak için aşağıdakileri ekleyin.'using namespace:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-534">To do this, add the following using namespace:</span></span>

    <span data-ttu-id="9e1b1-535">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 UsingModel*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-535">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModel*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. <span data-ttu-id="9e1b1-536">İki özellikleri **StoreBrowseViewModel** sınıfı: **Tarz** ve **albümleri**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-536">Add two properties to **StoreBrowseViewModel** class: **Genre** and **Albums**.</span></span> <span data-ttu-id="9e1b1-537">Bunu yapmak için aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-537">To do this, add the following code:</span></span>

    <span data-ttu-id="9e1b1-538">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 ModelProperties*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-538">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 ModelProperties*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> <span data-ttu-id="9e1b1-539">Nedir **listesi&lt;albüm&gt;**  ?: Bu tanımı kullanan **listesi&lt;T&gt;**  yazın, burada **T** kısıtlar hangi öğelere bu tür **listesi** için bu durumda ait **albüm** (veya alt öğelerinden birini).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-539">What is **List&lt;Album&gt;** ?: This definition is using the **List&lt;T&gt;** type, where **T** constrains the type to which elements of this **List** belong to, in this case **Album** (or any of its descendants).</span></span>
> 
> <span data-ttu-id="9e1b1-540">Sınıflar ve sınıf ya da yöntem bildirilir ve istemci kodu tarafından oluşturulan C# dil özelliğidir kadar bir veya daha fazla tür belirtimi erteleme yöntemler tasarlamak için bu özelliği adlı **genel türler**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-540">This ability to design classes and methods that defer the specification of one or more types until the class or method is declared and instantiated by client code is a feature of the C# language called **Generics**.</span></span>
> 
> <span data-ttu-id="9e1b1-541">**Liste&lt;T&gt;**  genel eşdeğerdir **ArrayList** yazın ve kullanılabilir **System.Collections.Generic** ad alanı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-541">**List&lt;T&gt;** is the generic equivalent of the **ArrayList** type and is available in the **System.Collections.Generic** namespace.</span></span> <span data-ttu-id="9e1b1-542">Kullanmanın faydaları birini **genel türler** türü belirtilmiş olduğundan, tür denetimini öğeyi atama gibi işlemleri ölçeklendirilmesini gerek olmayan **albüm** ile bir yaptığınızşekilde**ArrayList**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-542">One of the benefits of using **generics** is that since the type is specified, you do not need to take care of type checking operations such as casting the elements into **Album** as you would do with an **ArrayList**.</span></span>

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a><span data-ttu-id="9e1b1-543">Görev 3 - yeni ViewModel StoreController içinde kullanma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-543">Task 3 - Using the New ViewModel in the StoreController</span></span>

<span data-ttu-id="9e1b1-544">Bu görevde, değiştireceğiniz **StoreController**'s **Gözat** ve **ayrıntıları** yeni eylem yöntemleri **StoreBrowseViewModel** .</span><span class="sxs-lookup"><span data-stu-id="9e1b1-544">In this task, you will modify the **StoreController**'s **Browse** and **Details** action methods to use the new **StoreBrowseViewModel**.</span></span>

1. <span data-ttu-id="9e1b1-545">Modeller klasörü içinde bir başvuru ekleyin **StoreController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-545">Add a reference to the Models folder in **StoreController** class.</span></span> <span data-ttu-id="9e1b1-546">Bunu yapmak için genişletme **denetleyicileri** klasöründe **Çözüm Gezgini** açın **StoreController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-546">To do this, expand the **Controllers** folder in the **Solution Explorer** and open the **StoreController** class.</span></span> <span data-ttu-id="9e1b1-547">Ardından aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-547">Then add the following code:</span></span>

    <span data-ttu-id="9e1b1-548">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 UsingModelInController*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-548">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 UsingModelInController*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. <span data-ttu-id="9e1b1-549">Değiştirin **Gözat** eylem yönteminin kullanılacağını **StoreViewBrowseController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-549">Replace the **Browse** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="9e1b1-550">Sahte veri bir türe ve iki yeni albümleri nesneleri oluşturur (sonraki uygulamalı laboratuvarda bir veritabanındaki gerçek veriler tükettiğiniz).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-550">You will create a Genre and two new Albums objects with dummy data (in the next Hands-on Lab you will consume real data from a database).</span></span> <span data-ttu-id="9e1b1-551">Bunu yapmak için değiştirin **Gözat** yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-551">To do this, replace the **Browse** method with the following code:</span></span>

    <span data-ttu-id="9e1b1-552">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 BrowseMethod*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-552">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 BrowseMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. <span data-ttu-id="9e1b1-553">Değiştirin **ayrıntıları** eylem yönteminin kullanılacağını **StoreViewBrowseController** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-553">Replace the **Details** action method to use the **StoreViewBrowseController** class.</span></span> <span data-ttu-id="9e1b1-554">Yeni oluşturduğunuz **albüm** için döndürülecek nesne **görünümü**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-554">You will create a new **Album** object to be returned to the **View**.</span></span> <span data-ttu-id="9e1b1-555">Bunu yapmak için değiştirin **ayrıntıları** yöntemini aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-555">To do this, replace the **Details** method with the following code:</span></span>

    <span data-ttu-id="9e1b1-556">(Kod parçacığını - *ASP.NET MVC 4 temelleri - Ex6 DetailsMethod*)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-556">(Code Snippet - *ASP.NET MVC 4 Fundamentals - Ex6 DetailsMethod*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a><span data-ttu-id="9e1b1-557">Görev 4 - Gözat görünüm şablonu ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-557">Task 4 - Adding a Browse View Template</span></span>

<span data-ttu-id="9e1b1-558">Bu görevde, ekleyeceksiniz bir **Gözat** görünümü için belirli bir türe bulunan albümleri gösterecek şekilde.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-558">In this task, you will add a **Browse** View to show the Albums found for a specific Genre.</span></span>

1. <span data-ttu-id="9e1b1-559">Yeni Görünüm şablonu oluşturmadan önce projeyi oluşturmalısınız böylece **Görünüm Ekle** iletişim bilir hakkında **ViewModel** kullanılacak sınıfı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-559">Before creating the new View template, you should build the project so that the **Add View** Dialog knows about the **ViewModel** class to use.</span></span> <span data-ttu-id="9e1b1-560">Seçerek projenizi **derleme** menü öğesini ve ardından **MvcMusicStore yapı**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-560">Build the project by selecting the **Build** menu item and then **Build MvcMusicStore**.</span></span>
2. <span data-ttu-id="9e1b1-561">Ekleme bir **Gözat** görünümü.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-561">Add a **Browse** View.</span></span> <span data-ttu-id="9e1b1-562">Bunu yapmak için sağ **Gözat** eylem yöntemi **StoreController** tıklatıp **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-562">To do this, right-click in the **Browse** action method of the **StoreController** and click **Add View**.</span></span>
3. <span data-ttu-id="9e1b1-563">İçinde **Görünüm Ekle** iletişim kutusunda, Görünüm adı olduğunu doğrulayın **Gözat**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-563">In the **Add View** dialog box, verify that the View Name is **Browse**.</span></span> <span data-ttu-id="9e1b1-564">Denetleme **kesin türü belirtilmiş görünüm oluşturmak** onay kutusunu seçip **StoreBrowseViewModel** gelen **Model sınıfı** açılır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-564">Check the **Create a strongly-typed view** checkbox and select **StoreBrowseViewModel** from the **Model class** dropdown.</span></span> <span data-ttu-id="9e1b1-565">Diğer alanları varsayılan değerlerine bırakın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-565">Leave the other fields with their default value.</span></span> <span data-ttu-id="9e1b1-566">Ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-566">Then click **Add**.</span></span>

    <span data-ttu-id="9e1b1-567">![Göz atma görünüm ekleme](aspnet-mvc-4-fundamentals/_static/image29.png "Gözat görünüm ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-567">![Adding a Browse View](aspnet-mvc-4-fundamentals/_static/image29.png "Adding a Browse View")</span></span>

    <span data-ttu-id="9e1b1-568">*Göz atma görünüm ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-568">*Adding a Browse View*</span></span>
4. <span data-ttu-id="9e1b1-569">Değiştirme **Browse.cshtml** tarzı'nın bilgilerini görüntülemek için erişim **StoreBrowseViewModel** görünümü şablona geçirilen nesne.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-569">Modify the **Browse.cshtml** to display the Genre's information, accessing the **StoreBrowseViewModel** object that is passed to the view template.</span></span> <span data-ttu-id="9e1b1-570">Bunu yapmak için içeriği aşağıdakiyle değiştirin: (C#)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-570">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a><span data-ttu-id="9e1b1-571">Görev 5 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-571">Task 5 - Running the Application</span></span>

<span data-ttu-id="9e1b1-572">Bu görevde, test **Gözat** yöntemi albümleri öğesinden alır **Gözat** yöntemi eylem.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-572">In this task, you will test that the **Browse** method retrieves Albums from the **Browse** method action.</span></span>

1. <span data-ttu-id="9e1b1-573">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-573">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9e1b1-574">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-574">The project starts in the Home page.</span></span> <span data-ttu-id="9e1b1-575">URL'yi   **/Store/Gözat? Tarz DISCO =** eylemi iki albümleri döndürdüğünü doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-575">Change the URL to **/Store/Browse?Genre=Disco** to verify that the action returns two Albums.</span></span>

    <span data-ttu-id="9e1b1-576">![Store DISCO albümleri gözatma](aspnet-mvc-4-fundamentals/_static/image30.png "Store DISCO albümleri gözatma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-576">![Browsing Store Disco Albums](aspnet-mvc-4-fundamentals/_static/image30.png "Browsing Store Disco Albums")</span></span>

    <span data-ttu-id="9e1b1-577">*Store DISCO albümleri gözatma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-577">*Browsing Store Disco Albums*</span></span>

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a><span data-ttu-id="9e1b1-578">Görev 6 - bilgi hakkında belirli bir albüm görüntüleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-578">Task 6 - Displaying information About a Specific Album</span></span>

<span data-ttu-id="9e1b1-579">Bu görevde, gerçekleştireceksiniz **Store/Details** belirli bir albümü hakkındaki bilgileri görüntülemek için görünümü.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-579">In this task, you will implement the **Store/Details** view to display information about a specific album.</span></span> <span data-ttu-id="9e1b1-580">Bu uygulamalı laboratuvarda, görüntüler albümü hakkında her şeyi zaten bulunan **görünümü** şablonu.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-580">In this Hands-on Lab, everything you will display about the album is already contained in the **View** template.</span></span> <span data-ttu-id="9e1b1-581">Bunu oluşturmak yerine bir **StoreDetailsViewModel** sınıfı, geçerli kullanacağınız **StoreBrowseViewModel** albümü iletmeden şablonu.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-581">So, instead of creating a **StoreDetailsViewModel** class, you will use the current **StoreBrowseViewModel** template passing the Album to it.</span></span>

1. <span data-ttu-id="9e1b1-582">Gerekirse Visual Studio penceresine dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-582">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="9e1b1-583">Yeni bir **ayrıntıları** görüntüleme **StoreController**'s **ayrıntıları** eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-583">Add a new **Details** view for the **StoreController**'s **Details** action method.</span></span> <span data-ttu-id="9e1b1-584">Bunu yapmak için sağ **ayrıntıları** yönteminde **StoreController** sınıfı ve tıklayın **Görünüm Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-584">To do this, right-click the **Details** method in the **StoreController** class and click **Add View**.</span></span>
2. <span data-ttu-id="9e1b1-585">İçinde **Görünüm Ekle** iletişim kutusunda doğrulayın **Görünüm adı** olduğu **ayrıntıları**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-585">In the **Add View** dialog, verify that the **View Name** is **Details**.</span></span> <span data-ttu-id="9e1b1-586">Denetleyin **kesin türü belirtilmiş görünüm oluşturmak** onay kutusunu seçip **albüm** gelen **Model sınıfı** açılır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-586">Check the **Create a strongly-typed view** checkbox and select **Album** from the **Model class** drop-down.</span></span> <span data-ttu-id="9e1b1-587">Diğer alanları varsayılan değerlerine bırakın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-587">Leave the other fields with their default value.</span></span> <span data-ttu-id="9e1b1-588">Ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-588">Then click **Add**.</span></span> <span data-ttu-id="9e1b1-589">Bu oluşturma ve açık bir **\Views\Store\Details.cshtml** dosya.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-589">This will create and open a **\Views\Store\Details.cshtml** file.</span></span>

    <span data-ttu-id="9e1b1-590">![Ayrıntılar görünümü ekleme](aspnet-mvc-4-fundamentals/_static/image31.png "Ayrıntılar görünümü ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-590">![Adding a Details View](aspnet-mvc-4-fundamentals/_static/image31.png "Adding a Details View")</span></span>

    <span data-ttu-id="9e1b1-591">*Ayrıntılar görünümü ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-591">*Adding a Details View*</span></span>
3. <span data-ttu-id="9e1b1-592">Değiştirme **Details.cshtml** albüm bilgilerini görüntülemek için dosya erişim **albüm** görünümü şablona geçirilen nesne.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-592">Modify the **Details.cshtml** file to display the Album's information, accessing the **Album** object that is passed to the view template.</span></span> <span data-ttu-id="9e1b1-593">Bunu yapmak için içeriği aşağıdakiyle değiştirin: (C#)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-593">To do this, replace the content with the following: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a><span data-ttu-id="9e1b1-594">Görev 7 - Uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-594">Task 7 - Running the Application</span></span>

<span data-ttu-id="9e1b1-595">Bu görevde, test **ayrıntıları** görünümü alır albüm bilgilerinden **eylem ayrıntıları** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-595">In this task, you will test that the **Details** View retrieves Album's information from the **Details action** method.</span></span>

1. <span data-ttu-id="9e1b1-596">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-596">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9e1b1-597">Proje başlar **giriş** sayfası.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-597">The project starts in the **Home** page.</span></span> <span data-ttu-id="9e1b1-598">URL'yi **/Store/Details/5** albüm bilgileri doğrulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-598">Change the URL to **/Store/Details/5** to verify the album's information.</span></span>

    <span data-ttu-id="9e1b1-599">![Albümleri ayrıntılı tarama](aspnet-mvc-4-fundamentals/_static/image32.png "albümleri ayrıntılı tarama")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-599">![Browsing Albums Detail](aspnet-mvc-4-fundamentals/_static/image32.png "Browsing Albums Detail")</span></span>

    <span data-ttu-id="9e1b1-600">*Albüm ayrıntılı tarama*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-600">*Browsing Album's Detail*</span></span>

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a><span data-ttu-id="9e1b1-601">Görev 8 - sayfaları arasında bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-601">Task 8 - Adding Links Between Pages</span></span>

<span data-ttu-id="9e1b1-602">Bu görevde, Store görünümünde her Tarz ad uygun bir bağlantı sağlamak için bir bağlantı ekleyeceksiniz **/Store/göz atma** URL'si.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-602">In this task, you will add a link in the Store View to have a link in every Genre name to the appropriate **/Store/Browse** URL.</span></span> <span data-ttu-id="9e1b1-603">Bu şekilde bir türe üzerinde örneği için tıkladığınızda **DISCO**, gider **/Store/göz atma? Tarz DISCO =** URL'si.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-603">This way, when you click on a Genre, for instance **Disco**, it will navigate to **/Store/Browse?genre=Disco** URL.</span></span>

1. <span data-ttu-id="9e1b1-604">Gerekirse Visual Studio penceresine dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-604">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="9e1b1-605">Güncelleştirme **dizin** bağlantısı eklemek için sayfa **Gözat** sayfası.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-605">Update the **Index** page to add a link to the **Browse** page.</span></span> <span data-ttu-id="9e1b1-606">Bunu yapmak için **Çözüm Gezgini** genişletin **görünümleri** klasörü, ardından **Store** klasörü ve çift **Index.cshtml** sayfası.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-606">To do this, in the **Solution Explorer** expand the **Views** folder, then the **Store** folder and double-click the **Index.cshtml** page.</span></span>
2. <span data-ttu-id="9e1b1-607">Bağlantı, seçili Tarz gösteren göz atma görünümüne ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-607">Add a link to the Browse view indicating the genre selected.</span></span> <span data-ttu-id="9e1b1-608">Bunu yapmak için aşağıdaki vurgulanmış kodu içinde değiştirin **&lt;li&gt;** etiketler: (C#)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-608">To do this, replace the following highlighted code within the **&lt;li&gt;** tags: (C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > <span data-ttu-id="9e1b1-609">başka bir yaklaşım ile aşağıdaki gibi bir kod sayfası için doğrudan bağlama:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-609">another approach would be linking directly to the page, with a code like the following:</span></span>
   > 
   > <span data-ttu-id="9e1b1-610">&lt;bir href =&quot;/Store/göz atma? Tarz =@genreName&quot;&gt;@genreName&lt;/a&gt;</span><span class="sxs-lookup"><span data-stu-id="9e1b1-610">&lt;a href=&quot;/Store/Browse?genre=@genreName&quot;&gt;@genreName&lt;/a&gt;</span></span>
   > 
   > <span data-ttu-id="9e1b1-611">Bu yaklaşım çalışır, ancak bir sabit kodlanmış dizesine bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-611">Although this approach works, it depends on a hardcoded string.</span></span> <span data-ttu-id="9e1b1-612">Daha sonra kumanda yeniden adlandırma, bu yönerge el ile değiştirmeniz gerekecektir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-612">If you later rename the Controller, you will have to change this instruction manually.</span></span> <span data-ttu-id="9e1b1-613">Daha iyi bir alternatif kullanmaktır bir **HTML Yardımcısı** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-613">A better alternative is to use an **HTML Helper** method.</span></span> <span data-ttu-id="9e1b1-614">ASP.NET MVC gibi görevler için kullanılabilir olan bir HTML yardımcı yöntemini içerir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-614">ASP.NET MVC includes an HTML Helper method which is available for such tasks.</span></span> <span data-ttu-id="9e1b1-615">**Html.ActionLink()** yardımcı yöntem HTML oluşturmak kolaylaştırır **&lt;bir&gt;** bağlantılar, URL yolları URL kodlanmış düzgün olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-615">The **Html.ActionLink()** helper method makes it easy to build HTML **&lt;a&gt;** links, making sure URL paths are properly URL encoded.</span></span>
   > 
   > <span data-ttu-id="9e1b1-616">Htlm.ActionLink birçok aşırı yüklemeye sahip.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-616">Htlm.ActionLink has several overloads.</span></span> <span data-ttu-id="9e1b1-617">Bu alıştırmada, üç parametre almayan kullanır:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-617">In this exercise you will use one that takes three parameters:</span></span>
   > 
   > 1. <span data-ttu-id="9e1b1-618">Bağlantı metnini, tarzı adı görüntülenir</span><span class="sxs-lookup"><span data-stu-id="9e1b1-618">Link text, which will display the Genre name</span></span>
   > 2. <span data-ttu-id="9e1b1-619">Denetleyici eylem adı (**Gözat**)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-619">Controller action name (**Browse**)</span></span>
   > 3. <span data-ttu-id="9e1b1-620">Rota parametresi değerleri, her iki adı belirtme (**Tarz**) değeri (**Tarz adı**)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-620">Route parameter values, specifying both the name (**Genre**) and the value (**Genre name**)</span></span>

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a><span data-ttu-id="9e1b1-621">Görev 9 - uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-621">Task 9 - Running the Application</span></span>

<span data-ttu-id="9e1b1-622">Bu görevde her bir tür için uygun bir bağlantı olarak görüntülendiğini sınayacak **/Store/göz atma** URL'si.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-622">In this task, you will test that each Genre is displayed with a link to the appropriate **/Store/Browse** URL.</span></span>

1. <span data-ttu-id="9e1b1-623">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-623">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9e1b1-624">Proje giriş sayfası başlatır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-624">The project starts in the Home page.</span></span> <span data-ttu-id="9e1b1-625">URL'yi **/Store** her türe uygun bağlantılar doğrulamak için **/Store/göz atma** URL'si.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-625">Change the URL to **/Store** to verify that each Genre links to the appropriate **/Store/Browse** URL.</span></span>

    <span data-ttu-id="9e1b1-626">![Göz atma sayfasından bağlantı türleri gözatma](aspnet-mvc-4-fundamentals/_static/image33.png "göz atma sayfasından bağlantılarla gözatma türleri")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-626">![Browsing Genres with links to Browse page](aspnet-mvc-4-fundamentals/_static/image33.png "Browsing Genres with links to Browse page")</span></span>

    <span data-ttu-id="9e1b1-627">*Göz atma sayfasından bağlantı türleri gözatma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-627">*Browsing Genres with links to Browse page*</span></span>

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a><span data-ttu-id="9e1b1-628">Görev 10 - dinamik ViewModel koleksiyonu kullanarak değerleri geçirmek için</span><span class="sxs-lookup"><span data-stu-id="9e1b1-628">Task 10 - Using Dynamic ViewModel Collection to Pass Values</span></span>

<span data-ttu-id="9e1b1-629">Bu görevde, modeldeki herhangi bir değişiklik yapmadan değerleri denetleyici ve görünüm arasında geçirmek için basit ve güçlü bir yöntemdir öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-629">In this task, you will learn a simple and powerful method to pass values between the Controller and the View without making any changes in the Model.</span></span> <span data-ttu-id="9e1b1-630">ASP.NET MVC 4 sağlar koleksiyonu &quot;ViewModel&quot;, hangi herhangi bir dinamik değer atanabilir ve denetleyicileri ve görünümleri de içinde erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-630">ASP.NET MVC 4 provides the collection &quot;ViewModel&quot;, which can be assigned to any dynamic value and accessed inside controllers and views as well.</span></span>

<span data-ttu-id="9e1b1-631">Artık ViewBag dinamik koleksiyon listesini geçirmek için kullanacak &quot; **yıldızlı türleri** &quot; görünümüne denetleyicisinden.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-631">You will now use the ViewBag dynamic collection to pass a list of &quot;**Starred genres**&quot; from the controller to the view.</span></span> <span data-ttu-id="9e1b1-632">Store dizini görünümü için erişecek **ViewModel** ve bilgileri görüntüler.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-632">The Store Index view will access to **ViewModel** and display the information.</span></span>

1. <span data-ttu-id="9e1b1-633">Gerekirse Visual Studio penceresine dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-633">Close the browser if needed, to return to the Visual Studio window.</span></span> <span data-ttu-id="9e1b1-634">Açık **StoreController.cs** ve değiştirme **dizin** listesini oluşturmak için yöntem türleri ViewModel koleksiyona yıldızlı:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-634">Open **StoreController.cs** and modify **Index** method to create a list of starred genres into ViewModel collection :</span></span>

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > <span data-ttu-id="9e1b1-635">Söz dizimi de kullanabilirsiniz **ViewBag [&quot;Starred&quot;]** özelliklerine erişmek için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-635">You could also use the syntax **ViewBag[&quot;Starred&quot;]** to access the properties.</span></span>
2. <span data-ttu-id="9e1b1-636">Yıldız simgesini **&quot;starred.png&quot;** dahil **Source\Assets\Images** , bu Laboratuvarın klasör.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-636">The star icon **&quot;starred.png&quot;** is included in the **Source\Assets\Images** folder of this lab.</span></span> <span data-ttu-id="9e1b1-637">Uygulama eklemek için bunların içeriğini sürükleyin bir **Windows Explorer** penceresine **Çözüm Gezgini** Express'te Visual Web Developer aşağıda gösterildiği gibi:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-637">In order to add it to the application, drag their content from a **Windows Explorer** window into the **Solution Explorer** in Visual Web Developer Express, as shown below:</span></span>

    <span data-ttu-id="9e1b1-638">![Çözüm ekleme yıldız görüntüsüne](aspnet-mvc-4-fundamentals/_static/image34.png "çözüme yıldız görüntü ekleme")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-638">![Adding star image to the solution](aspnet-mvc-4-fundamentals/_static/image34.png "Adding star image to the solution")</span></span>

    <span data-ttu-id="9e1b1-639">*Çözüme yıldız görüntü ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-639">*Adding star image to the solution*</span></span>
3. <span data-ttu-id="9e1b1-640">Görünümü açma **Store/Index.cshtml** ve içeriği değiştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-640">Open the view **Store/Index.cshtml** and modify the content.</span></span> <span data-ttu-id="9e1b1-641">Okuyacaksa &quot;yıldızlı&quot; özelliğinde **ViewBag** koleksiyonu geçerli Tarz ad listesinde olup olmadığını isteyin.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-641">You will read the &quot;starred&quot; property in the **ViewBag** collection, and ask if the current genre name is in the list.</span></span> <span data-ttu-id="9e1b1-642">Bu durumda, bir yıldız simgesini sağ Tarz bağlantıyı gösterir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-642">In that case you will show a star icon right to the genre link.</span></span>
   <span data-ttu-id="9e1b1-643">(C#)</span><span class="sxs-lookup"><span data-stu-id="9e1b1-643">(C#)</span></span>

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a><span data-ttu-id="9e1b1-644">Görev 11 - uygulamayı çalıştırma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-644">Task 11 - Running the Application</span></span>

<span data-ttu-id="9e1b1-645">Bu görevde yıldızlı türleri bir yıldız simgesini görüntüleme test eder.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-645">In this task, you will test that the starred genres display a star icon.</span></span>

1. <span data-ttu-id="9e1b1-646">Tuşuna **F5** uygulamayı çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-646">Press **F5** to run the Application.</span></span>
2. <span data-ttu-id="9e1b1-647">Proje başlar **giriş** sayfası.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-647">The project starts in the **Home** page.</span></span> <span data-ttu-id="9e1b1-648">URL'yi **/Store** öne çıkan her Tarz saygı etiket olduğunu doğrulamak için:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-648">Change the URL to **/Store** to verify that each featured genre has the respecting label:</span></span>

    <span data-ttu-id="9e1b1-649">![Yıldızlı öğelerle türleri gözatma](aspnet-mvc-4-fundamentals/_static/image35.png "yıldızlı öğelerle gözatma türleri")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-649">![Browsing Genres with starred elements](aspnet-mvc-4-fundamentals/_static/image35.png "Browsing Genres with starred elements")</span></span>

    <span data-ttu-id="9e1b1-650">*Yıldızlı öğelerle gözatma türleri*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-650">*Browsing Genres with starred elements*</span></span>

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a><span data-ttu-id="9e1b1-651">Alıştırma 7: ASP.NET MVC 4 yeni şablonu turu</span><span class="sxs-lookup"><span data-stu-id="9e1b1-651">Exercise 7: A lap around ASP.NET MVC 4 new template</span></span>

<span data-ttu-id="9e1b1-652">Bu alıştırmada, bir görünüm en ilgili özellikleri yeni şablon alma ASP.NET MVC 4 proje şablonları geliştirmeleri inceleyeceksiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-652">In this exercise, you will explore the enhancements in the ASP.NET MVC 4 project templates, taking a look at the most relevant features of the new template.</span></span>

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a><span data-ttu-id="9e1b1-653">1. Görev: ASP.NET MVC 4 Internet uygulaması şablonu keşfetme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-653">Task 1: Exploring the ASP.NET MVC 4 Internet Application Template</span></span>

1. <span data-ttu-id="9e1b1-654">Açık değilse, başlangıç **Web için VS Express**</span><span class="sxs-lookup"><span data-stu-id="9e1b1-654">If not already open, start **VS Express for Web**</span></span>
2. <span data-ttu-id="9e1b1-655">Seçin **dosya | Yeni | Proje** menü komutu.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-655">Select the **File | New | Project** menu command.</span></span> <span data-ttu-id="9e1b1-656">İçinde **yeni proje** iletişim kutusunda **Visual C# | Web** Şablonu'nu seçin ve ağaç **ASP.NET MVC 4 Web uygulaması**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-656">In the **New Project** dialog, select the **Visual C#|Web** template on the left pane tree, and choose the **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="9e1b1-657">**Adı** proje *MusicStore* ve güncelleştirme **çözüm adı** için *başlamak*, ardından bir konum seçin (veya varsayılan değeri bırakın) ve tıklayın **Tamam** .</span><span class="sxs-lookup"><span data-stu-id="9e1b1-657">**Name** the project *MusicStore* and update the **solution name** to *Begin*, then select a location (or leave the default) and click **OK**.</span></span>

    <span data-ttu-id="9e1b1-658">![Yeni bir ASP.NET MVC 4 projesi oluşturma](aspnet-mvc-4-fundamentals/_static/image36.png "yeni bir ASP.NET MVC 4 projesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-658">![Creating a new ASP.NET MVC 4 Project](aspnet-mvc-4-fundamentals/_static/image36.png "Creating a new ASP.NET MVC 4 Project")</span></span>

    <span data-ttu-id="9e1b1-659">*Yeni bir ASP.NET MVC 4 projesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-659">*Creating a new ASP.NET MVC 4 Project*</span></span>
3. <span data-ttu-id="9e1b1-660">İçinde **yeni ASP.NET MVC 4 proje** iletişim kutusunda **Internet uygulaması** proje şablonu ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-660">In the **New ASP.NET MVC 4 Project** dialog, select the **Internet Application** project template and click **OK**.</span></span> <span data-ttu-id="9e1b1-661">Görünüm altyapısı Razor ya da ASPX seçebilirsiniz dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-661">Notice you can select either Razor or ASPX as the view engine.</span></span>

    <span data-ttu-id="9e1b1-662">![Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma](aspnet-mvc-4-fundamentals/_static/image37.png "yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-662">![Creating a new ASP.NET MVC 4 Internet Application](aspnet-mvc-4-fundamentals/_static/image37.png "Creating a new ASP.NET MVC 4 Internet Application")</span></span>

    <span data-ttu-id="9e1b1-663">*Yeni bir ASP.NET MVC 4 Internet uygulaması oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-663">*Creating a new ASP.NET MVC 4 Internet Application*</span></span>

    > [!NOTE]
    > <span data-ttu-id="9e1b1-664">Razor sözdizimi, ASP.NET MVC 3'te sunulmuştur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-664">Razor syntax has been introduced in ASP.NET MVC 3.</span></span> <span data-ttu-id="9e1b1-665">Karakterler ve tuş vuruşları gerekli bir dosya içinde hızlı ve iş akışı kodlama sıvı etkinleştirme sayısını en aza indirmek için hedefi sağlamaktır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-665">Its goal is to minimize the number of characters and keystrokes required in a file, enabling a fast and fluid coding workflow.</span></span> <span data-ttu-id="9e1b1-666">Mevcut C# /VB (veya diğer) Razor yararlanır dil becerilerine ve harika bir HTML oluşturma iş akışı sağlayan bir şablon işaretleme söz dizimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-666">Razor leverages existing C#/VB (or other) language skills and delivers a template markup syntax that enables an awesome HTML construction workflow.</span></span>
4. <span data-ttu-id="9e1b1-667">Tuşuna **F5** çözümü çalıştırın ve yenilenen şablonu görmek için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-667">Press **F5** to run the solution and see the renewed template.</span></span> <span data-ttu-id="9e1b1-668">Aşağıdaki özellikleri kontrol edebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-668">You can check out the following features:</span></span>

    1. <span data-ttu-id="9e1b1-669">**Modern stili şablonları**</span><span class="sxs-lookup"><span data-stu-id="9e1b1-669">**Modern-style templates**</span></span>

        <span data-ttu-id="9e1b1-670">Şablonlar, daha fazla modern görünümlü stilleri sağlama yenilenmiş.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-670">The templates have been renewed, providing more modern-looking styles.</span></span>

        <span data-ttu-id="9e1b1-671">![ASP.NET MVC 4 restyled şablonları](aspnet-mvc-4-fundamentals/_static/image38.png "şablonları restyled ASP.NET MVC 4")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-671">![ASP.NET MVC 4 restyled templates](aspnet-mvc-4-fundamentals/_static/image38.png "ASP.NET MVC 4 restyled templates")</span></span>

        <span data-ttu-id="9e1b1-672">*ASP.NET MVC 4 restyled şablonları*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-672">*ASP.NET MVC 4 restyled templates*</span></span>
    2. <span data-ttu-id="9e1b1-673">**Uyarlamalı işleme**</span><span class="sxs-lookup"><span data-stu-id="9e1b1-673">**Adaptive Rendering**</span></span>

        <span data-ttu-id="9e1b1-674">Tarayıcı penceresini yeniden boyutlandırmadan kullanıma denetleyin ve sayfa düzeni için yeni pencere boyutunu dinamik olarak Uyarlanır nasıl dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-674">Check out resizing the browser window and notice how the page layout dynamically adapts to the new window size.</span></span> <span data-ttu-id="9e1b1-675">Bu şablonlar, hem Masaüstü hem de mobil platformlarda özelleştirme olmadan düzgün bir şekilde işlemek için Uyarlamalı işleme tekniği kullanın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-675">These templates use the adaptive rendering technique to render properly in both desktop and mobile platforms without any customization.</span></span>

        <span data-ttu-id="9e1b1-676">![ASP.NET MVC 4 proje şablonu farklı tarayıcı boyutlarda](aspnet-mvc-4-fundamentals/_static/image39.png "farklı tarayıcı boyutlarda ASP.NET MVC 4 proje şablonu")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-676">![ASP.NET MVC 4 project template in different browser sizes](aspnet-mvc-4-fundamentals/_static/image39.png "ASP.NET MVC 4 project template in different browser sizes")</span></span>

        <span data-ttu-id="9e1b1-677">*ASP.NET MVC 4 Proje şablonunda farklı tarayıcı boyutları*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-677">*ASP.NET MVC 4 project template in different browser sizes*</span></span>
5. <span data-ttu-id="9e1b1-678">Hata ayıklayıcıyı durdurduktan ve Visual Studio'ya dönmek için tarayıcıyı kapatın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-678">Close the browser to stop the debugger and return to Visual Studio.</span></span>
6. <span data-ttu-id="9e1b1-679">Artık çözümü keşfedin ve bazı proje şablonunda ASP.NET MVC 4'tarafından sunulan yeni özellikleri kullanıma sunuldu.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-679">Now you are able to explore the solution and check out some of the new features introduced by ASP.NET MVC 4 in the project template.</span></span>

    <span data-ttu-id="9e1b1-680">![ASP.NET MVC4-internet-uygulaması-proje-şablonu](aspnet-mvc-4-fundamentals/_static/image40.png "ASP.NET MVC 4 Internet uygulaması proje şablonu")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-680">![ASP.NET MVC4-internet-application-project-template](aspnet-mvc-4-fundamentals/_static/image40.png "The ASP.NET MVC 4 Internet Application Project Template")</span></span>

    <span data-ttu-id="9e1b1-681">*ASP.NET MVC 4 Internet uygulaması proje şablonu*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-681">*The ASP.NET MVC 4 Internet Application Project Template*</span></span>

   1. <span data-ttu-id="9e1b1-682">**HTML5 biçimlendirme**</span><span class="sxs-lookup"><span data-stu-id="9e1b1-682">**HTML5 markup**</span></span>

       <span data-ttu-id="9e1b1-683">Şablon görünümleri yeni tema biçimlendirme açık örnek bulmak için Gözat **About.cshtml** görünümü **giriş** klasör.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-683">Browse template views to find out the new theme markup, for example open **About.cshtml** view within **Home** folder.</span></span>

       <span data-ttu-id="9e1b1-684">![Razor ve HTML5 biçimlendirme kullanarak yeni şablon](aspnet-mvc-4-fundamentals/_static/image41.png "Razor ve HTML5 biçimlendirme kullanarak yeni şablon")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-684">![New template, using Razor and HTML5 markup](aspnet-mvc-4-fundamentals/_static/image41.png "New template, using Razor and HTML5 markup")</span></span>

       <span data-ttu-id="9e1b1-685">*Razor ve HTML5 biçimlendirme kullanarak yeni şablon*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-685">*New template, using Razor and HTML5 markup*</span></span>
   2. <span data-ttu-id="9e1b1-686">**JavaScript kitaplıkları dahil**</span><span class="sxs-lookup"><span data-stu-id="9e1b1-686">**JavaScript libraries included**</span></span>

      1. <span data-ttu-id="9e1b1-687">**jQuery**: HTML belge geçiş yapma, olay işleme, animasyon ve Ajax etkileşimleri jQuery basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-687">**jQuery**: jQuery simplifies HTML document traversing, event handling, animating, and Ajax interactions.</span></span>
      2. <span data-ttu-id="9e1b1-688">**jQuery kullanıcı Arabirimi**: Bu kitaplık, alt düzey etkileşim, animasyon efektleri Gelişmiş ve jQuery JavaScript kitaplığı üzerine kurulu bölümlerinin tema eklenebilir pencere öğeleri için soyutlamalar sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-688">**jQuery UI**: This library provides abstractions for low-level interaction and animation, advanced effects and themeable widgets, built on top of the jQuery JavaScript Library.</span></span>

         > [!NOTE]
         > <span data-ttu-id="9e1b1-689">JQuery ve jQuery kullanıcı Arabirimi hakkında bilgi edinin, [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-689">You can learn about jQuery and jQuery UI in [[http://docs.jquery.com/](http://docs.jquery.com/)](http://docs.jquery.com/).</span></span>
      3. <span data-ttu-id="9e1b1-690">**KnockoutJS**: ASP.NET MVC 4 varsayılan şablonu artık içerir **KnockoutJS**, JavaScript ve HTML kullanarak zengin ve yüksek derecede yanıt veren web uygulamaları oluşturmanızı sağlayan bir JavaScript MVVM çerçeve.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-690">**KnockoutJS**: The ASP.NET MVC 4 default template now includes **KnockoutJS**, a JavaScript MVVM framework that lets you create rich and highly responsive web applications using JavaScript and HTML.</span></span> <span data-ttu-id="9e1b1-691">Gibi ASP.NET MVC 3'te, jQuery ve jQuery UI kitaplıkları da ASP.NET MVC 4'te dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-691">Like in ASP.NET MVC 3, jQuery and jQuery UI libraries are also included in ASP.NET MVC 4.</span></span>

          > [!NOTE]
          > <span data-ttu-id="9e1b1-692">Bu bağlantıda KnockOutJS Kitaplığı hakkında daha fazla bilgi edinebilirsiniz: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-692">You can get more information about KnockOutJS library in this link: [http://learn.knockoutjs.com/](http://learn.knockoutjs.com/).</span></span>
      4. <span data-ttu-id="9e1b1-693">**Modernizr**: Bu kitaplık HTML5 ve CSS3 teknolojileri kullanırken, sitenizi eski tarayıcıları ile uyumlu hale otomatik olarak çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-693">**Modernizr**: This library runs automatically, making your site compatible with older browsers when using HTML5 and CSS3 technologies.</span></span>

          > [!NOTE]
          > <span data-ttu-id="9e1b1-694">Bu bağlantıda Modernizr Kitaplığı hakkında daha fazla bilgi edinebilirsiniz: [ http://www.modernizr.com/ ](http://www.modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-694">You can get more information about Modernizr library in this link: [http://www.modernizr.com/](http://www.modernizr.com/).</span></span>
   3. <span data-ttu-id="9e1b1-695">**Çözümde yer alan SimpleMembership**</span><span class="sxs-lookup"><span data-stu-id="9e1b1-695">**SimpleMembership included in the solution**</span></span>

       <span data-ttu-id="9e1b1-696">SimpleMembership önceki ASP.NET rol ve üyelik sağlayıcısını sistemin bir ardılı olarak tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-696">SimpleMembership has been designed as a replacement for the previous ASP.NET Role and Membership provider system.</span></span> <span data-ttu-id="9e1b1-697">Güvenli web sayfaları geliştiricinin daha esnek bir biçimde kolaylaştıran birçok yeni özellik var.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-697">It has many new features that make it easier for the developer to secure web pages in a more flexible way.</span></span>

       <span data-ttu-id="9e1b1-698">Internet şablonu zaten SimpleMembership tümleştirmek için birkaç şeyi ayarlamanız ayarladı, AccountController OAuthWebSecurity (için OAuth hesap kaydı, oturum açma, yönetim, vb.) ve Web güvenliği gibi hazırlanır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-698">The Internet template already has set up a few things to integrate SimpleMembership, for example, the AccountController is prepared to use OAuthWebSecurity (for OAuth account registration, login, management, etc.) and Web Security.</span></span>

       <span data-ttu-id="9e1b1-699">![Çözüm SimpleMembership dahil](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership dahil çözümü")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-699">![SimpleMembership Included in the solution](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership Included in the solution")</span></span>

       <span data-ttu-id="9e1b1-700">*Çözüm SimpleMembership dahil*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-700">*SimpleMembership Included in the solution*</span></span>

       > [!NOTE]
       > <span data-ttu-id="9e1b1-701">Hakkında daha fazla bilgi bulmak [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) MSDN'de.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-701">Find more information about [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) in MSDN.</span></span>

> [!NOTE]
> <span data-ttu-id="9e1b1-702">Ayrıca, bu uygulama için Windows Azure Web siteleri aşağıdaki dağıtabilirsiniz [ek B: yayımlama Web dağıtımı kullanarak bir ASP.NET MVC 4 uygulaması](#AppendixB).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-702">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="9e1b1-703">Özet</span><span class="sxs-lookup"><span data-stu-id="9e1b1-703">Summary</span></span>

<span data-ttu-id="9e1b1-704">Bu uygulamalı laboratuvarı tamamlayarak ASP.NET MVC temelleri öğrendiniz:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-704">By completing this Hands-On Lab you have learned the fundamentals of ASP.NET MVC:</span></span>

- <span data-ttu-id="9e1b1-705">Bir MVC uygulaması ve nasıl etkileşim kurduklarını temel öğeleri</span><span class="sxs-lookup"><span data-stu-id="9e1b1-705">The core elements of an MVC application and how they interact</span></span>
- <span data-ttu-id="9e1b1-706">Bir ASP.NET MVC uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-706">How to create an ASP.NET MVC Application</span></span>
- <span data-ttu-id="9e1b1-707">URL ve sorgu dizesi ekleyin ve parametreleri işlemek için denetleyicileri yapılandırma başarılı</span><span class="sxs-lookup"><span data-stu-id="9e1b1-707">How to add and configure Controllers to handle parameters passed through the URL and querystring</span></span>
- <span data-ttu-id="9e1b1-708">Ortak HTML içeriğini, görünümünü ve HTML içeriğini görüntülemek için bir görünüm şablonu geliştirmek için bir stil sayfası için bir şablon kurmak için bir düzen ana sayfası ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-708">How to add a layout master page to setup a template for common HTML content, a StyleSheet to enhance the look and feel and a View template to display HTML content</span></span>
- <span data-ttu-id="9e1b1-709">Dinamik bilgileri görüntülemek için Özellikler Görünümü şablona geçirme ViewModel desenini kullanma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-709">How to use the ViewModel pattern for passing properties to the View template to display dynamic information</span></span>
- <span data-ttu-id="9e1b1-710">Görünüm şablonda denetleyicilerine geçirilen parametreleri kullanma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-710">How to use parameters passed to Controllers in the View template</span></span>
- <span data-ttu-id="9e1b1-711">ASP.NET MVC uygulaması içindeki sayfalara bağlantılar ekleme</span><span class="sxs-lookup"><span data-stu-id="9e1b1-711">How to add links to pages inside the ASP.NET MVC application</span></span>
- <span data-ttu-id="9e1b1-712">Ekleme ve bir görünümle dinamik özelliklerini kullanma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-712">How to add and use dynamic properties in a View</span></span>
- <span data-ttu-id="9e1b1-713">ASP.NET MVC 4 proje şablonları iyileştirmeler</span><span class="sxs-lookup"><span data-stu-id="9e1b1-713">The enhancements in the ASP.NET MVC 4 project templates</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="9e1b1-714">Ek A: Visual Studio'yu Web için Express 2012</span><span class="sxs-lookup"><span data-stu-id="9e1b1-714">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="9e1b1-715">Yükleyebileceğiniz **Web için Visual Studio Express 2012 Microsoft** veya başka bir &quot;Express&quot; sürümüyle **[Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-715">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="9e1b1-716">Aşağıdaki yönergeler, yüklemek için gereken adımlarda size kılavuzluk *Web için Visual studio Express 2012* kullanarak *Microsoft Web Platformu yükleyicisi*.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-716">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="9e1b1-717">Git [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-717">Go to [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="9e1b1-718">Web Platformu Yükleyicisi'ı zaten yüklediyseniz, bunun yerine ve ürün için arama açabileceğiniz &quot; <em>Visual Studio Express 2012 için Windows Azure SDK ile Web</em>&quot;.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-718">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="9e1b1-719">Tıklayarak **Şimdi Yükle**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-719">Click on **Install Now**.</span></span> <span data-ttu-id="9e1b1-720">Yoksa **Web Platformu yükleyicisi** indirmek ve ilk yüklemek için yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-720">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="9e1b1-721">Bir kez **Web Platformu yükleyicisi** açık tıklayın **yükleme** Kurulum'u başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-721">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="9e1b1-722">![Visual Studio Express yükleyin](aspnet-mvc-4-fundamentals/_static/image43.png "Visual Studio Express'i yükle")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-722">![Install Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="9e1b1-723">*Visual Studio Express yükleyin*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-723">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="9e1b1-724">Tüm ürünlerin lisans ve koşulları okuyun ve tıklayın **kabul ediyorum** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-724">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![Lisans koşullarını kabul etme](aspnet-mvc-4-fundamentals/_static/image44.png)

    <span data-ttu-id="9e1b1-726">*Lisans koşullarını kabul etme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-726">*Accepting the license terms*</span></span>
5. <span data-ttu-id="9e1b1-727">İndirme ve yükleme işlemi tamamlanana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-727">Wait until the downloading and installation process completes.</span></span>

    ![Yükleme ilerleme durumu](aspnet-mvc-4-fundamentals/_static/image45.png)

    <span data-ttu-id="9e1b1-729">*Yükleme ilerleme durumu*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-729">*Installation progress*</span></span>
6. <span data-ttu-id="9e1b1-730">Yükleme tamamlandığında, tıklayın **son**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-730">When the installation completes, click **Finish**.</span></span>

    ![Yükleme tamamlandı](aspnet-mvc-4-fundamentals/_static/image46.png)

    <span data-ttu-id="9e1b1-732">*Yükleme tamamlandı*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-732">*Installation completed*</span></span>
7. <span data-ttu-id="9e1b1-733">Tıklayın **çıkış** Web Platformu Yükleyicisi'ni kapatın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-733">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="9e1b1-734">Web için Visual Studio Express'te açmak için Git **Başlat** ekranında ve yazmaya başlayabilirsiniz &quot; **VS Express**&quot;, ardından **Web için VS Express** bir kutucuk.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-734">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![Web kutucuğu için VS Express](aspnet-mvc-4-fundamentals/_static/image47.png)

    <span data-ttu-id="9e1b1-736">*Web kutucuğu için VS Express*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-736">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="9e1b1-737">Ek B: bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="9e1b1-737">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="9e1b1-738">Bu ekte, Windows Azure Yönetim Portalı'ndan yeni bir web sitesi oluşturun ve Laboratuvar izleyerek Windows Azure tarafından sağlanan Web dağıtımı Yayımlama özelliğini avantajlarını elde ettiğiniz uygulama yayımlama gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-738">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="9e1b1-739">Görev 1 Windows yeni bir Web sitesi oluşturma - Azure portalı</span><span class="sxs-lookup"><span data-stu-id="9e1b1-739">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="9e1b1-740">Git [Windows Azure Yönetim Portalı](https://manage.windowsazure.com/) aboneliğinizle ilişkili Microsoft kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-740">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9e1b1-741">Windows Azure'la 10 ASP.NET Web sitesini ücretsiz olarak barındırın ve ardından trafiğiniz büyüdükçe ölçeğinizi artırın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-741">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="9e1b1-742">Kaydolabilirsiniz [burada](http://aka.ms/aspnet-hol-azure).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-742">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="9e1b1-743">![Windows Azure Portal'da oturum açın](aspnet-mvc-4-fundamentals/_static/image48.png "Windows Azure Portal'da oturum açın")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-743">![Log on to Windows Azure portal](aspnet-mvc-4-fundamentals/_static/image48.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="9e1b1-744">*Windows Azure yönetim portalında oturum açın*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-744">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="9e1b1-745">Tıklayın **yeni** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-745">Click **New** on the command bar.</span></span>

    <span data-ttu-id="9e1b1-746">![Yeni bir Web sitesi oluşturma](aspnet-mvc-4-fundamentals/_static/image49.png "yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-746">![Creating a new Web Site](aspnet-mvc-4-fundamentals/_static/image49.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="9e1b1-747">*Yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-747">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="9e1b1-748">Tıklayın **işlem** | **Web sitesi**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-748">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="9e1b1-749">Ardından **hızlı Oluştur** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-749">Then select **Quick Create** option.</span></span> <span data-ttu-id="9e1b1-750">Yeni web sitesi için kullanılabilen bir URL girin ve tıklatın **Web sitesi oluştur**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-750">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9e1b1-751">Bir Windows Azure Web sitesi kontrol edebildiğiniz ve yönetebildiğiniz bulutta çalışan bir web uygulaması için ana bilgisayardır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-751">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="9e1b1-752">Hızlı oluştur seçeneği, tamamlanan web uygulaması için Windows Azure Web sitesinden portalın dışında dağıtmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-752">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="9e1b1-753">Bir veritabanını ayarlamak için adımları içermez.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-753">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="9e1b1-754">![Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma](aspnet-mvc-4-fundamentals/_static/image50.png "hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-754">![Creating a new Web Site using Quick Create](aspnet-mvc-4-fundamentals/_static/image50.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="9e1b1-755">*Hızlı oluşturma yöntemini kullanarak yeni bir Web sitesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-755">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="9e1b1-756">Yeni kadar bekleyin **Web sitesi** oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-756">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="9e1b1-757">Web sitesi oluşturulduktan sonra altındaki bağlantıya tıklayın **URL** sütun.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-757">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="9e1b1-758">Yeni Web sitesi çalışıp çalışmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-758">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="9e1b1-759">![Yeni web sitesi için gözatma](aspnet-mvc-4-fundamentals/_static/image51.png "yeni web sitesine göz atma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-759">![Browsing to the new web site](aspnet-mvc-4-fundamentals/_static/image51.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="9e1b1-760">*Yeni web sitesine göz atma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-760">*Browsing to the new web site*</span></span>

    <span data-ttu-id="9e1b1-761">![Web sitesi çalışan](aspnet-mvc-4-fundamentals/_static/image52.png "çalışan Web sitesi")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-761">![Web site running](aspnet-mvc-4-fundamentals/_static/image52.png "Web site running")</span></span>

    <span data-ttu-id="9e1b1-762">*Çalışan Web sitesi*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-762">*Web site running*</span></span>
6. <span data-ttu-id="9e1b1-763">Portala geri dönün ve web sitesi altında adına **adı** yönetim sayfaları görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-763">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="9e1b1-764">![Web sitesi Yönetim sayfalarının açma](aspnet-mvc-4-fundamentals/_static/image53.png "web sitesi Yönetim sayfalarının açma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-764">![Opening the web site management pages](aspnet-mvc-4-fundamentals/_static/image53.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="9e1b1-765">*Web Sitesi Yönetim sayfalarının açma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-765">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="9e1b1-766">İçinde **Pano** sayfasındaki **Hızlı Bakış** bölümünde **yayımlama profili indir** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-766">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9e1b1-767">*Yayımlama profilini* tüm her etkin yayımlama yöntemi için bir Windows Azure Web sitesi için bir web uygulaması yayımlamak için gereken bilgileri içerir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-767">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="9e1b1-768">Yayımlama profili URL'leri, kullanıcı kimlik bilgileri ve her bir yayımlama yönteminin etkinleştirildiği uç noktalarına karşı kimlik doğrulaması yapmak ve bağlanmak için gereken veritabanı dizelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-768">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="9e1b1-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express Web** ve **Microsoft Visual Studio 2012** okuma desteği yayımlama profillerini yapılandırma için bu programların otomatik hale getirmek için Windows Azure Web siteleri'ne yayımlama web uygulamaları.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-769">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="9e1b1-770">![Yayımlama profili web sitesi indiriliyor](aspnet-mvc-4-fundamentals/_static/image54.png "yayımlama profili web sitesi indiriliyor")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-770">![Downloading the web site publish profile](aspnet-mvc-4-fundamentals/_static/image54.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="9e1b1-771">*Yayımlama profili Web sitesi indiriliyor*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-771">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="9e1b1-772">Yayımlama profili dosyasını bilinen bir konuma indirin.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-772">Download the publish profile file to a known location.</span></span> <span data-ttu-id="9e1b1-773">Daha fazla Bu alıştırmada, bu dosyayı Visual Studio'dan bir web uygulaması için bir Windows Azure Web siteleri yayımlama için nasıl kullanılacağını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-773">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="9e1b1-774">![Yayımlama profili dosyasını kaydetme](aspnet-mvc-4-fundamentals/_static/image55.png "yayımlama profili kaydediliyor")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-774">![Saving the publish profile file](aspnet-mvc-4-fundamentals/_static/image55.png "Saving the publish profile")</span></span>

    <span data-ttu-id="9e1b1-775">*Yayımlama profili dosyasını kaydetme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-775">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="9e1b1-776">Görev 2 - veritabanı sunucusunu yapılandırma</span><span class="sxs-lookup"><span data-stu-id="9e1b1-776">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="9e1b1-777">Uygulamanızı kullanan SQL Server'ın yaparsa veritabanlarını bir SQL veritabanı sunucusu oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-777">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="9e1b1-778">SQL Server kullanmayan basit bir uygulama dağıtmak istiyorsanız bu görevi atla.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-778">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="9e1b1-779">SQL veritabanı sunucusu, uygulama veritabanını depolamak için gerekir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-779">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="9e1b1-780">SQL veritabanı sunucularını, aboneliğinizde Windows Azure Yönetim Portalı'nda görüntüleyebilirsiniz **Sql veritabanları** | **sunucuları** | **sunucunun Pano**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-780">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="9e1b1-781">Oluşturulan server yoksa kullanarak bir tane oluşturabilirsiniz **Ekle** komut çubuğunda düğme.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-781">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="9e1b1-782">Not **sunucu adı ve URL, yönetici oturum açma adı ve parola**, sonraki görevleri kullanacağınız.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-782">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="9e1b1-783">Daha sonraki bir aşamasında oluşturulacak şekilde veritabanı henüz oluşturmayın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-783">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="9e1b1-784">![SQL veritabanı sunucu Panosu](aspnet-mvc-4-fundamentals/_static/image56.png "SQL veritabanı sunucu Panosu")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-784">![SQL Database Server Dashboard](aspnet-mvc-4-fundamentals/_static/image56.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="9e1b1-785">*SQL veritabanı sunucu Panosu*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-785">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="9e1b1-786">İşlemin sonraki görev ihtiyacınız sunucunun listesinde yerel IP adresinizi eklemek, bu nedenle Visual Studio'dan veritabanı bağlantısını test eder **izin verilen IP adresleri**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-786">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="9e1b1-787">Bunu yapmanın tıklatın **yapılandırma**, IP adresi seçin **geçerli istemci IP adresi** ve yapıştırın **başlangıç IP adresi** ve **bitiş IP adresi** metin kutuları ve tıklatın ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) düğmesi.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-787">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) button.</span></span>

    ![İstemci IP adresi ekleme](aspnet-mvc-4-fundamentals/_static/image58.png)

    <span data-ttu-id="9e1b1-789">*İstemci IP adresi ekleme*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-789">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="9e1b1-790">Bir kez **istemci IP adresi** için izin verilen IP adreslerini eklenir listesinde, tıklayın **Kaydet** değişiklikleri onaylamak için.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-790">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![Değişiklikleri onaylayın](aspnet-mvc-4-fundamentals/_static/image59.png)

    <span data-ttu-id="9e1b1-792">*Değişiklikleri onaylayın*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-792">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="9e1b1-793">Görev 3 - bir ASP.NET MVC 4 Web dağıtımı kullanarak uygulama yayımlama</span><span class="sxs-lookup"><span data-stu-id="9e1b1-793">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="9e1b1-794">ASP.NET MVC 4 çözüme geri dönün.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-794">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="9e1b1-795">İçinde **Çözüm Gezgini**, web sitesi projesini sağ tıklatın ve seçin **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-795">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="9e1b1-796">![Uygulama yayımlama](aspnet-mvc-4-fundamentals/_static/image60.png "uygulama yayımlama")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-796">![Publishing the Application](aspnet-mvc-4-fundamentals/_static/image60.png "Publishing the Application")</span></span>

    <span data-ttu-id="9e1b1-797">*Web sitesi yayımlama*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-797">*Publishing the web site*</span></span>
2. <span data-ttu-id="9e1b1-798">İlk görevde kaydettiğiniz yayımlama profilini içeri aktarın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-798">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="9e1b1-799">![Yayımlama profilini içeri aktarma](aspnet-mvc-4-fundamentals/_static/image61.png "yayımlama profilini içeri aktarma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-799">![Importing the publish profile](aspnet-mvc-4-fundamentals/_static/image61.png "Importing the publish profile")</span></span>

    <span data-ttu-id="9e1b1-800">*Yayımlama profilini içeri aktarma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-800">*Importing publish profile*</span></span>
3. <span data-ttu-id="9e1b1-801">Tıklayın **bağlantısını doğrulama**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-801">Click **Validate Connection**.</span></span> <span data-ttu-id="9e1b1-802">Doğrulama tamamlandıktan sonra tıklayın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-802">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="9e1b1-803">Bağlantıyı doğrula düğmesi yanında görünür yeşil bir onay işareti gördükten sonra doğrulama tamamlanır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-803">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="9e1b1-804">![Bağlantı doğrulama](aspnet-mvc-4-fundamentals/_static/image62.png "bağlantısı doğrulanıyor")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-804">![Validating connection](aspnet-mvc-4-fundamentals/_static/image62.png "Validating connection")</span></span>

    <span data-ttu-id="9e1b1-805">*Bağlantı doğrulama*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-805">*Validating connection*</span></span>
4. <span data-ttu-id="9e1b1-806">İçinde **ayarları** sayfasındaki **veritabanları** bölümünde, veritabanı bağlantının metin kutusunun yanındaki düğmeye tıklayın (yani **DefaultConnection**).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-806">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="9e1b1-807">![Web dağıtımı yapılandırma](aspnet-mvc-4-fundamentals/_static/image63.png "Web dağıtımı yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-807">![Web deploy configuration](aspnet-mvc-4-fundamentals/_static/image63.png "Web deploy configuration")</span></span>

    <span data-ttu-id="9e1b1-808">*Web dağıtımı yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-808">*Web deploy configuration*</span></span>
5. <span data-ttu-id="9e1b1-809">Veritabanı bağlantısı aşağıdaki gibi yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="9e1b1-809">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="9e1b1-810">İçinde **sunucu adı** , SQL veritabanı sunucu URL'sini kullanarak yazın *tcp:* önek.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-810">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="9e1b1-811">İçinde **kullanıcı adı** , Sunucu Yöneticisi oturum açma adı yazın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-811">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="9e1b1-812">İçinde **parola** Sunucu Yöneticisi oturum açma parolanızı yazın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-812">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="9e1b1-813">Örneğin yeni bir veritabanı adı yazın: *MVC4SampleDB*.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-813">Type a new database name, for example: *MVC4SampleDB*.</span></span>

     <span data-ttu-id="9e1b1-814">![Hedef bağlantı dizesi yapılandırma](aspnet-mvc-4-fundamentals/_static/image64.png "hedef bağlantı dizesini yapılandırma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-814">![Configuring destination connection string](aspnet-mvc-4-fundamentals/_static/image64.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="9e1b1-815">*Hedef bağlantı dizesini yapılandırma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-815">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="9e1b1-816">Sonra **Tamam**'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-816">Then click **OK**.</span></span> <span data-ttu-id="9e1b1-817">Veritabanı oluşturmak isteyip istemediğiniz sorulduğunda **Evet**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-817">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="9e1b1-818">![Veritabanı oluşturma](aspnet-mvc-4-fundamentals/_static/image65.png "veritabanı dizesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-818">![Creating the database](aspnet-mvc-4-fundamentals/_static/image65.png "Creating the database string")</span></span>

    <span data-ttu-id="9e1b1-819">*Veritabanı oluşturma*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-819">*Creating the database*</span></span>
7. <span data-ttu-id="9e1b1-820">Windows Azure SQL veritabanına bağlanmak için kullanacağı bağlantı dizesini, varsayılan bağlantı metin kutusu içinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-820">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="9e1b1-821">Sonra **İleri**'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-821">Then click **Next**.</span></span>

    <span data-ttu-id="9e1b1-822">![SQL veritabanı'na işaret eden bağlantı dizesi](aspnet-mvc-4-fundamentals/_static/image66.png "SQL veritabanına işaret eden bağlantı dizesi")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-822">![Connection string pointing to SQL Database](aspnet-mvc-4-fundamentals/_static/image66.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="9e1b1-823">*SQL veritabanı'na işaret eden bağlantı dizesi*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-823">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="9e1b1-824">İçinde **Önizleme** sayfasında **Yayımla**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-824">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="9e1b1-825">![Web uygulaması yayımlama](aspnet-mvc-4-fundamentals/_static/image67.png "web uygulaması yayımlama")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-825">![Publishing the web application](aspnet-mvc-4-fundamentals/_static/image67.png "Publishing the web application")</span></span>

    <span data-ttu-id="9e1b1-826">*Web uygulaması yayımlama*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-826">*Publishing the web application*</span></span>
9. <span data-ttu-id="9e1b1-827">Yayımlama işlemi tamamlandıktan sonra varsayılan tarayıcınız yayımlanan web sitesi açılır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-827">Once the publishing process finishes, your default browser will open the published web site.</span></span>

    <span data-ttu-id="9e1b1-828">![Uygulama Windows Azure'da yayımlanan](aspnet-mvc-4-fundamentals/_static/image68.png "uygulama yayımlanan Windows Azure'a")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-828">![Application published to Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "Application published to Windows Azure")</span></span>

    <span data-ttu-id="9e1b1-829">*Windows Azure'da yayımlanan uygulama*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-829">*Application published to Windows Azure*</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="9e1b1-830">Ek C: kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="9e1b1-830">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="9e1b1-831">Kod parçacıkları ile parmaklarınızın ucunda ihtiyacınız olan tüm kod vardır.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-831">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="9e1b1-832">Laboratuvar belgenin tam olarak ne zaman, kullanabilmek için aşağıdaki şekilde gösterildiği gibi size bildirir.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-832">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="9e1b1-833">![Kod projenize eklemek için Visual Studio kod parçacıkları](aspnet-mvc-4-fundamentals/_static/image69.png "kod projenize eklemek için Visual Studio kullanarak kod parçacıkları")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-833">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-fundamentals/_static/image69.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="9e1b1-834">*Kod projenize eklemek için Visual Studio kod parçacıkları*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-834">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="9e1b1-835">***Klavye (yalnızca C#) kullanarak bir kod parçacığı eklemek için***</span><span class="sxs-lookup"><span data-stu-id="9e1b1-835">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="9e1b1-836">Kod eklemesini istediğiniz imleci yerleştirin.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-836">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="9e1b1-837">(Olmadan, boşluk veya tire) kod parçacığı adı yazmaya başlayın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-837">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="9e1b1-838">Kod parçacıkları adlarla eşleşen IntelliSense görüntüler izleyin.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-838">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="9e1b1-839">Doğru kod parçacığını seçin (veya tüm parçacığının adı seçilene kadar yazmaya devam edin).</span><span class="sxs-lookup"><span data-stu-id="9e1b1-839">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="9e1b1-840">İki kez İmleç konumuna kod parçacığını eklemek için SEKME tuşuna basın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-840">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="9e1b1-841">![Kod parçacığı adını yazmaya başlayın](aspnet-mvc-4-fundamentals/_static/image70.png "kod parçacığı adını yazmaya başlayın")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-841">![Start typing the snippet name](aspnet-mvc-4-fundamentals/_static/image70.png "Start typing the snippet name")</span></span>

<span data-ttu-id="9e1b1-842">*Kod parçacığı adını yazmaya başlayın*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-842">*Start typing the snippet name*</span></span>

<span data-ttu-id="9e1b1-843">![Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın](aspnet-mvc-4-fundamentals/_static/image71.png "vurgulanan kod parçacığı seçmek için Tab tuşuna basın")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-843">![Press Tab to select the highlighted snippet](aspnet-mvc-4-fundamentals/_static/image71.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="9e1b1-844">*Vurgulanan kod parçacığı seçmek için SEKME tuşuna basın*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-844">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="9e1b1-845">![Yeniden Tab tuşuna basın ve kod parçacığı genişletir](aspnet-mvc-4-fundamentals/_static/image72.png "yeniden Tab tuşuna basın ve kod parçacığı genişletir")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-845">![Press Tab again and the snippet will expand](aspnet-mvc-4-fundamentals/_static/image72.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="9e1b1-846">*Yeniden Tab tuşuna basın ve kod parçacığı genişletir*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-846">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="9e1b1-847">***Fare (C#, Visual Basic ve XML) kullanarak bir kod parçacığı eklemek için*** 1.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-847">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="9e1b1-848">Kod parçacığını eklemek istediğiniz yeri sağ tıklayın.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-848">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="9e1b1-849">Seçin **parçacık Ekle** ardından **kod Parçacıklarım**.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-849">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="9e1b1-850">Tıklayarak ilgili kod parçacığı listeden seçin.</span><span class="sxs-lookup"><span data-stu-id="9e1b1-850">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="9e1b1-851">![İstediğiniz kod parçacığını eklemek ve parçacık eklemek için sağ tıklama](aspnet-mvc-4-fundamentals/_static/image73.png "sağ tıklayın, istediğiniz kod parçacığını eklemek ve kod parçacığı Ekle")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-851">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-fundamentals/_static/image73.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="9e1b1-852">*Kod parçacığını eklemek ve parçacık eklemek istediğiniz sağ tıklayın*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-852">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="9e1b1-853">![Tıklayarak ilgili kod parçacığını listesinden çekme](aspnet-mvc-4-fundamentals/_static/image74.png "tıklayarak ilgili kod parçacığı listeden seçin")</span><span class="sxs-lookup"><span data-stu-id="9e1b1-853">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-fundamentals/_static/image74.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="9e1b1-854">*Tıklayarak ilgili kod parçacığı listeden seçin*</span><span class="sxs-lookup"><span data-stu-id="9e1b1-854">*Pick the relevant snippet from the list, by clicking on it*</span></span>
