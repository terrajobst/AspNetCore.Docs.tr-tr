---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Laboratuvar üzerinde aktarır: sürdürülebilir Azure Web siteleri: değişiklik ve ölçek yönetme | Microsoft Docs'
author: rick-anderson
description: Bu laboratuar ortamında nasıl Microsoft Azure, derleme ve üretim Web siteleri dağıtma kolaylaştırır öğrenin.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: a79921681b4e742b5cd23f7119d19f4dd74c3f83
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a><span data-ttu-id="17767-103">Laboratuvar üzerinde aktarır: sürdürülebilir Azure Web siteleri: değişiklik ve ölçek yönetme</span><span class="sxs-lookup"><span data-stu-id="17767-103">Hands on Lab: Maintainable Azure Websites: Managing Change and Scale</span></span>
====================
<span data-ttu-id="17767-104">Tarafından [Web Camps ekibi](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="17767-104">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="17767-105">Kit eğitim Web Camps indirin</span><span class="sxs-lookup"><span data-stu-id="17767-105">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="17767-106">Microsoft Azure ve üretim Web siteleri dağıtacak daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="17767-106">Microsoft Azure makes it easy to build and deploy websites to production.</span></span> <span data-ttu-id="17767-107">Ancak, uygulamanızın Canlı olduğunda bitirdiniz değil, yeni başlıyor olun!</span><span class="sxs-lookup"><span data-stu-id="17767-107">But you're not done when your application is live, you're just getting started!</span></span> <span data-ttu-id="17767-108">Değişen gereksinimleri, veritabanı güncelleştirmeleri, ölçeklendirme ve daha fazla işleme gerekir.</span><span class="sxs-lookup"><span data-stu-id="17767-108">You'll need to handle changing requirements, database updates, scale, and more.</span></span> <span data-ttu-id="17767-109">Neyse ki, Azure App Service, yeterince sorunsuz çalışmasını sitelerinizi sağlamanıza yardımcı olmak için özellikler ele sahiptir.</span><span class="sxs-lookup"><span data-stu-id="17767-109">Fortunately, Azure App Service has you covered, with plenty of features to help you keep your sites running smoothly.</span></span>
> 
> <span data-ttu-id="17767-110">Azure, güvenli ve esnek geliştirme, dağıtımı ve ölçeklendirme herhangi bir boyutu web uygulaması için seçenekleri sunar.</span><span class="sxs-lookup"><span data-stu-id="17767-110">Azure offers secure and flexible development, deployment and scaling options for any size web application.</span></span> <span data-ttu-id="17767-111">Altyapı yönetme uğraşmadan uygulamalar oluşturmak ve dağıtmak için varolan araçlarınızı yararlanın.</span><span class="sxs-lookup"><span data-stu-id="17767-111">Leverage your existing tools to create and deploy applications without the hassle of managing infrastructure.</span></span>
> 
> <span data-ttu-id="17767-112">Bir üretim web uygulaması, sık kullanılan geliştirme aracını kullanarak oluşturduğunuz içeriği kolayca dağıtarak kendiniz dakika cinsinden sağlayın.</span><span class="sxs-lookup"><span data-stu-id="17767-112">Provision a production web application yourself in minutes by easily deploying content created using your favorite development tool.</span></span> <span data-ttu-id="17767-113">Mevcut bir site için destek ile kaynak denetiminden doğrudan dağıtabilirsiniz **Git**, **GitHub**, **Bitbucket**, **TFS**ve hatta  **DropBox**.</span><span class="sxs-lookup"><span data-stu-id="17767-113">You can deploy an existing site directly from source control with support for **Git**, **GitHub**, **Bitbucket**, **TFS**, and even **DropBox**.</span></span> <span data-ttu-id="17767-114">Doğrudan sık kullanılan IDE'yi veya komut dosyalarını kullanarak dağıtmak **PowerShell** Windows veya **CLI** üzerinde herhangi bir işletim sistemi çalıştıran araçları.</span><span class="sxs-lookup"><span data-stu-id="17767-114">Deploy directly from your favorite IDE or from scripts using **PowerShell** in Windows or **CLI** tools running on any OS.</span></span> <span data-ttu-id="17767-115">Uygulama dağıtıldıktan sonra siteleriniz sürekli dağıtım desteği ile sürekli güncel tutun.</span><span class="sxs-lookup"><span data-stu-id="17767-115">Once deployed, keep your sites constantly up-to-date with support for continuous deployment.</span></span>
> 
> <span data-ttu-id="17767-116">Azure ölçeklenebilir, dayanıklı bulut depolama, yedekleme ve kurtarma çözümleri büyük veya küçük herhangi bir veri sağlar.</span><span class="sxs-lookup"><span data-stu-id="17767-116">Azure provides scalable, durable cloud storage, backup, and recovery solutions for any data, big or small.</span></span> <span data-ttu-id="17767-117">Bir üretim ortamında, tablolar, BLOB'lar ve SQL veritabanları gibi depolama hizmetleri uygulamaları dağıtırken, uygulamayı bulutta ölçek yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="17767-117">When deploying applications to a production environment, storage services such as Tables, Blobs and SQL Databases, help you scale your application in the cloud.</span></span>
> 
> <span data-ttu-id="17767-118">SQL veritabanları ile dağıtırken, uygulamanın yeni sürümlerini üretken veritabanınızı güncel tutmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="17767-118">With SQL Databases, it is important to keep your productive database up-to-date when deploying new versions of your application.</span></span> <span data-ttu-id="17767-119">Teşekkürler **Entity Framework Code First Migrations**, ortamınızı dakika cinsinden güncelleştirmek için veri modelinizi dağıtımını ve geliştirme basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="17767-119">Thanks to **Entity Framework Code First Migrations**, the development and deployment of your data model has been simplified to update your environments in minutes.</span></span> <span data-ttu-id="17767-120">Bu uygulamalı Laboratuvar web uygulamanızı Microsoft Azure üretim ortamlarında dağıtırken karşılaşabileceği farklı konuları gösterilir.</span><span class="sxs-lookup"><span data-stu-id="17767-120">This hands-on lab will show you the different topics you could encounter when deploying your web app to production environments in Microsoft Azure.</span></span>
> 
> <span data-ttu-id="17767-121">Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="17767-121">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>
> 
> <span data-ttu-id="17767-122">Daha fazla ayrıntılı kapsamını Bu konu için bkz: [Azure e-kitap ile yapı gerçek bulut uygulamaları](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="17767-122">For more in-depth coverage of this topic, see the [Building Real-World Cloud Apps with Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="17767-123">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="17767-123">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="17767-124">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="17767-124">Objectives</span></span>

<span data-ttu-id="17767-125">Uygulamalı bu laboratuvarda, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="17767-125">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="17767-126">Varolan bir modelle Entity Framework geçişleri etkinleştir</span><span class="sxs-lookup"><span data-stu-id="17767-126">Enable Entity Framework Migrations with an existing model</span></span>
- <span data-ttu-id="17767-127">Nesne modeli ve buna göre Entity Framework geçişler kullanarak veritabanını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="17767-127">Update the object model and database accordingly using Entity Framework Migrations</span></span>
- <span data-ttu-id="17767-128">Git kullanarak Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="17767-128">Deploy to Azure App Service using Git</span></span>
- <span data-ttu-id="17767-129">Azure Yönetim Portalı'nı kullanarak önceki bir dağıtıma geri alma</span><span class="sxs-lookup"><span data-stu-id="17767-129">Rollback to a previous deployment using the Azure Management portal</span></span>
- <span data-ttu-id="17767-130">Bir web uygulaması ölçeklendirmek için Azure Storage'ı kullanma</span><span class="sxs-lookup"><span data-stu-id="17767-130">Use Azure Storage to scale a web app</span></span>
- <span data-ttu-id="17767-131">Azure Yönetim Portalı'nı kullanarak bir web uygulaması için otomatik olarak ölçeklendirme yapılandırın</span><span class="sxs-lookup"><span data-stu-id="17767-131">Configure auto-scaling for a web app using the Azure Management Portal</span></span>
- <span data-ttu-id="17767-132">Oluşturma ve Visual Studio Yük testi projesi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="17767-132">Create and configure a load test project in Visual Studio</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="17767-133">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="17767-133">Prerequisites</span></span>

<span data-ttu-id="17767-134">Aşağıdaki uygulamalı bu laboratuvarı tamamlamak için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="17767-134">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="17767-135">[Visual Studio Express 2013 Web](https://www.microsoft.com/visualstudio/) veya daha büyük</span><span class="sxs-lookup"><span data-stu-id="17767-135">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="17767-136">2.2 .NET için Azure SDK</span><span class="sxs-lookup"><span data-stu-id="17767-136">Azure SDK for .NET 2.2</span></span>](https://www.microsoft.com/windowsazure/sdk/)
- [<span data-ttu-id="17767-137">GIT sürüm denetimi sistemi</span><span class="sxs-lookup"><span data-stu-id="17767-137">GIT Version Control System</span></span>](http://git-scm.com/download)
- <span data-ttu-id="17767-138">Microsoft Azure aboneliği</span><span class="sxs-lookup"><span data-stu-id="17767-138">A Microsoft Azure subscription</span></span> 

    - <span data-ttu-id="17767-139">Kaydolun bir [ücretsiz deneme sürümü](http://aka.ms/watk-freetrial)</span><span class="sxs-lookup"><span data-stu-id="17767-139">Sign up for a [Free Trial](http://aka.ms/watk-freetrial)</span></span>
    - <span data-ttu-id="17767-140">Visual Studio Professional, Test Professional, Premium veya Ultimate MSDN veya MSDN platformları aboneye varsa, etkinleştirme, [MSDN avantajı](http://aka.ms/watk-msdn) geliştirip Azure üzerinde test şimdi başlatmak için</span><span class="sxs-lookup"><span data-stu-id="17767-140">If you are a Visual Studio Professional, Test Professional, Premium or Ultimate with MSDN or MSDN Platforms subscriber, activate your [MSDN benefit](http://aka.ms/watk-msdn) now to start developing and testing on Azure</span></span>
    - <span data-ttu-id="17767-141">[BizSpark](http://aka.ms/watk-bizspark) üyeleri otomatik olarak Azure almak, Visual Studio Ultimate MSDN abonelikleri ile aracılığıyla avantajı</span><span class="sxs-lookup"><span data-stu-id="17767-141">[BizSpark](http://aka.ms/watk-bizspark) members automatically receive the Azure benefit through their Visual Studio Ultimate with MSDN subscriptions</span></span>
    - <span data-ttu-id="17767-142">Üyeleri [Microsoft iş ortağı ağı](http://aka.ms/watk-mpn) Bulut Temel program ücret ödemeden aylık Azure krediler alırsınız</span><span class="sxs-lookup"><span data-stu-id="17767-142">Members of the [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials program receive monthly Azure credits at no charge</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="17767-143">Kurulum</span><span class="sxs-lookup"><span data-stu-id="17767-143">Setup</span></span>

<span data-ttu-id="17767-144">Bu uygulamalı Laboratuvar alıştırmaları çalıştırmak için ortamınızı ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="17767-144">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="17767-145">Açık Windows Gezgini ve Laboratuvar 's gözatın **kaynak** klasör.</span><span class="sxs-lookup"><span data-stu-id="17767-145">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="17767-146">Sağ **Setup.cmd** seçip **yönetici olarak çalıştır** ortamınızı yapılandırmanıza ve bu Laboratuvar için Visual Studio kod parçacıkları yükleme kurulum işlemi başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="17767-146">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="17767-147">Kullanıcı Hesabı Denetimi iletişim gösteriliyorsa, devam etmek için eylemi onaylayın.</span><span class="sxs-lookup"><span data-stu-id="17767-147">If the User Account Control dialog is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="17767-148">Kurulumu çalıştırmadan önce bu Laboratuvar için tüm bağımlılıkların işaretli olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="17767-148">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="17767-149">Kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="17767-149">Using the Code Snippets</span></span>

<span data-ttu-id="17767-150">Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="17767-150">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="17767-151">Size kolaylık olması için bu kodu çoğunu, Visual Studio el ile eklemek zorunda kalmamak için 2013 içinde erişebileceğiniz Visual Studio kod parçacıkları, olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="17767-151">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="17767-152">Her alıştırma bulunan Başlangıç bir çözüm tarafından eşlik **başlamak** diğer bağımsız olarak her alıştırma izlemenizi sağlar alıştırmanın klasör.</span><span class="sxs-lookup"><span data-stu-id="17767-152">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="17767-153">Lütfen bir alıştırma sırasında eklenen kod parçacıkları çözümleri başlangıç bunlardan eksik ve alıştırma tamamlayıncaya kadar çalışmayabilir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="17767-153">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="17767-154">Bir alıştırma için kaynak kodunu içinde da bulacaksınız bir **son** karşılık gelen alıştırmada adımları tamamlamanızı sonuçları kodunu içeren bir Visual Studio çözümü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="17767-154">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="17767-155">Uygulamalı bu Laboratuvar çalışırken Ek Yardım gerekirse, bu çözümlerin kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="17767-155">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="17767-156">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="17767-156">Exercises</span></span>

<span data-ttu-id="17767-157">Bu uygulamalı Laboratuvar aşağıdaki alıştırmaları içerir:</span><span class="sxs-lookup"><span data-stu-id="17767-157">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="17767-158">Entity Framework geçişler kullanma</span><span class="sxs-lookup"><span data-stu-id="17767-158">Using Entity Framework Migrations</span></span>](#Exercise1)
2. [<span data-ttu-id="17767-159">Hazırlama için bir Web uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="17767-159">Deploying a Web App to Staging</span></span>](#Exercise2)
3. [<span data-ttu-id="17767-160">Üretim dağıtımı geri alınmasının</span><span class="sxs-lookup"><span data-stu-id="17767-160">Performing Deployment Rollback in Production</span></span>](#Exercise3)
4. [<span data-ttu-id="17767-161">Azure Storage kullanma ölçeklendirme</span><span class="sxs-lookup"><span data-stu-id="17767-161">Scaling Using Azure Storage</span></span>](#Exercise4)
5. <span data-ttu-id="17767-162">[Web uygulamaları için otomatik ölçeklendirme kullanarak](#Exercise5) (Visual Studio 2013 Ultimate edition için isteğe bağlı)</span><span class="sxs-lookup"><span data-stu-id="17767-162">[Using Autoscale for Web Apps](#Exercise5) (Optional for Visual Studio 2013 Ultimate edition)</span></span>

<span data-ttu-id="17767-163">Bu laboratuvarı tamamlamak için süre tahmini: **75 dakika**</span><span class="sxs-lookup"><span data-stu-id="17767-163">Estimated time to complete this lab: **75 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="17767-164">Visual Studio ilk kez başlattığınızda, önceden tanımlı ayarlar koleksiyonları birini seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="17767-164">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="17767-165">Önceden tanımlanmış her koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyicisinin davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler.</span><span class="sxs-lookup"><span data-stu-id="17767-165">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="17767-166">Bu Laboratuvar yordamlarda kullanırken Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklamak **genel geliştirme ayarları** koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="17767-166">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="17767-167">Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız gereken adımları farklılıklar olabilir.</span><span class="sxs-lookup"><span data-stu-id="17767-167">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a><span data-ttu-id="17767-168">Alıştırma 1: Entity Framework geçişler kullanma</span><span class="sxs-lookup"><span data-stu-id="17767-168">Exercise 1: Using Entity Framework Migrations</span></span>

<span data-ttu-id="17767-169">Bir uygulama geliştirirken, veri modelinizi zaman içinde değişebilir.</span><span class="sxs-lookup"><span data-stu-id="17767-169">When you are developing an application, your data model might change over time.</span></span> <span data-ttu-id="17767-170">(Yeni bir sürüm oluşturuyorsanız) bu değişiklikler, veritabanınızdaki varolan modeli etkileyebilir ve veritabanınızı hataları önlemek için güncel tutmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="17767-170">These changes could affect the existing model in your database (if you are creating a new version) and it is important to keep your database up-to-date to prevent errors.</span></span>

<span data-ttu-id="17767-171">Bu değişiklikler, modelinizdeki izleme basitleştirmek için **Entity Framework Code First Migrations** modelinizi veritabanı şeması karşılaştırma değişiklikleri otomatik olarak algılayıp, veritabanını güncelleştirmek için özel kod oluşturur Yeni oluşturma *sürümleri* veritabanınızın.</span><span class="sxs-lookup"><span data-stu-id="17767-171">To simplify the tracking of these changes in your model, **Entity Framework Code First Migrations** automatically detect changes comparing your model with the database schema and generates specific code to update your database, creating new *versions* of your database.</span></span>

<span data-ttu-id="17767-172">Bu alıştırmada nasıl etkinleştirileceği gösterilmiştir **geçişler** uygulamanız ve nasıl kolayca algılayabilir ve veritabanlarınızı güncelleştirmek için değişiklikleri oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="17767-172">This exercise shows you how to enable **Migrations** for your application and how you can easily detect and generate changes to update your databases.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a><span data-ttu-id="17767-173">Görev 1 – etkinleştirme geçişleri</span><span class="sxs-lookup"><span data-stu-id="17767-173">Task 1 – Enabling Migrations</span></span>

<span data-ttu-id="17767-174">Bu görevde, etkinleştirme adımları boyunca geçer **Entity Framework Code First Migrations** için **günlük test** modelini değiştirme ve bu değişiklikleri nasıl yansıtılır anlama veritabanı Veritabanı.</span><span class="sxs-lookup"><span data-stu-id="17767-174">In this task, you will go through the steps of enabling **Entity Framework Code First Migrations** to the **Geek Quiz** database, changing the model and understanding how those changes are reflected in the database.</span></span>

1. <span data-ttu-id="17767-175">Visual Studio'yu açın ve açık **GeekQuiz.sln** çözüm dosyasından **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.</span><span class="sxs-lookup"><span data-stu-id="17767-175">Open Visual Studio and open the **GeekQuiz.sln** solution file from **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.</span></span>
2. <span data-ttu-id="17767-176">İndirmek ve yüklemek için çözüm yapı **NuGet** paket bağımlılıkları.</span><span class="sxs-lookup"><span data-stu-id="17767-176">Build the solution in order to download and install the **NuGet** package dependencies.</span></span> <span data-ttu-id="17767-177">Bunu yapmak için çözüme sağ tıklayın ve **yapı çözümü** veya basın **Ctrl + Shift + B**.</span><span class="sxs-lookup"><span data-stu-id="17767-177">To do this, right-click the solution and click **Build Solution** or press **Ctrl + Shift + B**.</span></span>
3. <span data-ttu-id="17767-178">Gelen **Araçları** menü Visual Studio'da seçin **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="17767-178">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
4. <span data-ttu-id="17767-179">İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="17767-179">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="17767-180">Varolan modelini temel alan bir ilk geçiş oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="17767-180">An initial migration based on the existing model will be created.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    <span data-ttu-id="17767-181">![Geçişler etkinleştirme](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "geçişler etkinleştirme")</span><span class="sxs-lookup"><span data-stu-id="17767-181">![Enabling Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Enabling Migrations")</span></span>

    <span data-ttu-id="17767-182">*Geçişler etkinleştirme*</span><span class="sxs-lookup"><span data-stu-id="17767-182">*Enabling Migrations*</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-183">Bu komut ekler bir **geçişler** adlı bir dosyayı içeren günlük test projesi için klasörde **Configuration.cs**.</span><span class="sxs-lookup"><span data-stu-id="17767-183">This command adds a **Migrations** folder to Geek Quiz project that contains a file called **Configuration.cs**.</span></span> <span data-ttu-id="17767-184">**Yapılandırma** sınıfı geçişler içeriğiniz için nasıl davranacağını yapılandırmanıza olanak verir.</span><span class="sxs-lookup"><span data-stu-id="17767-184">The **Configuration** class allows you to configure how Migrations behaves for your context.</span></span>
5. <span data-ttu-id="17767-185">Etkin geçiş ile güncelleştirmeniz gerekir. **yapılandırma** veritabanı ilk verileri ile doldurmak için sınıf, **günlük test** gerektirir.</span><span class="sxs-lookup"><span data-stu-id="17767-185">With Migrations enabled, you need to update the **Configuration** class to populate the database with the initial data that **Geek Quiz** requires.</span></span> <span data-ttu-id="17767-186">Altında **geçişler**, yerine **Configuration.cs** bir dosyayla bulunan **Source\Assets** bu laboratuvarı klasör.</span><span class="sxs-lookup"><span data-stu-id="17767-186">Under **Migrations**, replace the **Configuration.cs** file with the one located in the **Source\Assets** folder of this lab.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-187">Bu yana **geçişler** çağıracak **çekirdek** yöntemi her veritabanı güncelleştirme ihtiyacınız kayıtları veritabanında çoğaltılmadığından emin olmak.</span><span class="sxs-lookup"><span data-stu-id="17767-187">Since **Migrations** will call the **Seed** method with every database update, you need to be sure that records are not duplicated in the database.</span></span> <span data-ttu-id="17767-188">**Örnek** yöntemi yinelenen verileri önlemek için yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="17767-188">The **AddOrUpdate** method will help to prevent duplicate data.</span></span>
6. <span data-ttu-id="17767-189">İlk geçiş eklemek için aşağıdaki komutu girin ve sonra basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="17767-189">To add an initial migration, enter the following command and then press **Enter**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-190">Adlı bir veritabanı olduğundan emin olun &quot;GeekQuizProd&quot; LocalDB Örneğinizde.</span><span class="sxs-lookup"><span data-stu-id="17767-190">Make sure that there is no database named &quot;GeekQuizProd&quot; in your LocalDB instance.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    <span data-ttu-id="17767-191">![Temel şema geçiş ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "ekleme temel şema geçiş")</span><span class="sxs-lookup"><span data-stu-id="17767-191">![Adding base schema migration](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Adding base schema migration")</span></span>

    <span data-ttu-id="17767-192">*Temel şema geçiş ekleme*</span><span class="sxs-lookup"><span data-stu-id="17767-192">*Adding base schema migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-193">**Add-Migration** son geçiş oluşturulmasından bu yana modelinize yapmış değişikliklere dayalı sonraki geçişin iskelesi.</span><span class="sxs-lookup"><span data-stu-id="17767-193">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created.</span></span> <span data-ttu-id="17767-194">Bu durumda, ilk geçiş projesinin olduğu gibi tanımlanan tüm tabloları oluşturmak için komut dosyalarını ekleyecek **TriviaContext** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="17767-194">In this case, as it is the first migration of the project, it will add the scripts to create all the tables defined in the **TriviaContext** class.</span></span>
7. <span data-ttu-id="17767-195">Aşağıdaki komutu çalıştırarak veritabanını güncelleştirmek için geçiş yürütün.</span><span class="sxs-lookup"><span data-stu-id="17767-195">Execute the migration to update the database by running the following command.</span></span> <span data-ttu-id="17767-196">Bu komut için belirttiğiniz **ayrıntılı** hedef veritabanına uygulanmakta olan SQL deyimlerini görüntülemek için bayrak.</span><span class="sxs-lookup"><span data-stu-id="17767-196">For this command you will specify the **Verbose** flag to view the SQL statements being applied to the target database.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    <span data-ttu-id="17767-197">![Oluşturma ilk veritabanı](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "ilk veritabanı oluşturma")</span><span class="sxs-lookup"><span data-stu-id="17767-197">![Creating initial database](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Creating initial database")</span></span>

    <span data-ttu-id="17767-198">*İlk veritabanı oluşturma*</span><span class="sxs-lookup"><span data-stu-id="17767-198">*Creating initial database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-199">**Update-Database** bekleyen tüm geçişleri veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="17767-199">**Update-Database** will apply any pending migrations to the database.</span></span> <span data-ttu-id="17767-200">Bu durumda, web.config dosyasında tanımlanan bağlantı dizesi kullanılarak veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="17767-200">In this case, it will create the database using the connection string defined in your web.config file.</span></span>
8. <span data-ttu-id="17767-201">Git **Görünüm** menü ve açık **SQL Server Nesne Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="17767-201">Go to **View** menu and open **SQL Server Object Explorer**.</span></span>

    <span data-ttu-id="17767-202">![SQL Server nesne Gezgini'nde Aç](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "SQL Server nesne Gezgini'nde Aç")</span><span class="sxs-lookup"><span data-stu-id="17767-202">![Open in SQL Server Object Explorer](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Open in SQL Server Object Explorer")</span></span>

    <span data-ttu-id="17767-203">*SQL Server nesne Gezgini'nde Aç*</span><span class="sxs-lookup"><span data-stu-id="17767-203">*Open in SQL Server Object Explorer*</span></span>
9. <span data-ttu-id="17767-204">İçinde **SQL Server Nesne Gezgini** penceresinde sağ tıklayarak, yerel veritabanı örneğine bağlanın **SQL Server** düğümü ve seçerek **SQL Server Ekle...**  seçeneği.</span><span class="sxs-lookup"><span data-stu-id="17767-204">In the **SQL Server Object Explorer** window, connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="17767-205">![Bir SQL Server örneği ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "bir SQL Server örneği ekleme")</span><span class="sxs-lookup"><span data-stu-id="17767-205">![Adding a SQL Server Instance](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="17767-206">*Bir SQL Server örneği için SQL Server Nesne Gezgini ekleme*</span><span class="sxs-lookup"><span data-stu-id="17767-206">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
10. <span data-ttu-id="17767-207">Ayarlama **sunucu adı** için *(localdb) \v11.0* bırakıp **Windows kimlik doğrulaması** , kimlik doğrulama modu.</span><span class="sxs-lookup"><span data-stu-id="17767-207">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="17767-208">Tıklatın **Bağlan** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="17767-208">Click **Connect** to continue.</span></span>

    <span data-ttu-id="17767-209">![Yerel veritabanına bağlanma](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "yerel veritabanına bağlanma")</span><span class="sxs-lookup"><span data-stu-id="17767-209">![Connecting to LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="17767-210">*Yerel veritabanına bağlanma*</span><span class="sxs-lookup"><span data-stu-id="17767-210">*Connecting to LocalDB*</span></span>
11. <span data-ttu-id="17767-211">Açık **GeekQuizProd** veritabanı ve genişletin **tabloları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="17767-211">Open the **GeekQuizProd** database and expand the **Tables** node.</span></span> <span data-ttu-id="17767-212">Gördüğünüz gibi **Update-Database** komutu oluşturulan tanımlanan tüm tabloları **TriviaContext** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="17767-212">As you can see, the **Update-Database** command generated all the tables defined in the **TriviaContext** class.</span></span> <span data-ttu-id="17767-213">Bulun **dbo. TriviaQuestions** tablo ve sütun düğümünü açın.</span><span class="sxs-lookup"><span data-stu-id="17767-213">Locate the **dbo.TriviaQuestions** table and open the columns node.</span></span> <span data-ttu-id="17767-214">Sonraki görev, komut bu tabloya yeni bir sütun ekleyin ve kullanarak veritabanını güncelleştirir **geçişler**.</span><span class="sxs-lookup"><span data-stu-id="17767-214">In the next task, you will add a new column to this table and update the database using **Migrations**.</span></span>

    <span data-ttu-id="17767-215">![Trivia sorular sütunları](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia sorular sütunları")</span><span class="sxs-lookup"><span data-stu-id="17767-215">![Trivia Questions Columns](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia Questions Columns")</span></span>

    <span data-ttu-id="17767-216">*Sütunları Trivia sorular*</span><span class="sxs-lookup"><span data-stu-id="17767-216">*Trivia Questions Columns*</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a><span data-ttu-id="17767-217">Görev 2 – geçişleri kullanmaya güncelleştirme veritabanı şeması</span><span class="sxs-lookup"><span data-stu-id="17767-217">Task 2 – Updating Database Schema Using Migrations</span></span>

<span data-ttu-id="17767-218">Bu görevde, kullanacağınız **Entity Framework Code First Migrations** modelinizi bir değişikliği algılar ve veritabanını güncelleştirmek için gerekli kodu oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="17767-218">In this task, you will use **Entity Framework Code First Migrations** to detect a change in your model and generate the necessary code to update the database.</span></span> <span data-ttu-id="17767-219">Güncelleştirmek **TriviaQuestions** yeni bir özellik ekleyerek varlık.</span><span class="sxs-lookup"><span data-stu-id="17767-219">You will update the **TriviaQuestions** entity by adding a new property to it.</span></span> <span data-ttu-id="17767-220">Ardından tabloda yeni bir sütun eklemek için yeni bir geçiş oluşturmak için komutları çalışır.</span><span class="sxs-lookup"><span data-stu-id="17767-220">Then you will run commands to create a new Migration to include the new column in the table.</span></span>

1. <span data-ttu-id="17767-221">İçinde **Çözüm Gezgini**, çift **TriviaQuestion.cs** içinde bulunan dosyasını **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="17767-221">In **Solution Explorer**, double-click the **TriviaQuestion.cs** file located inside the **Models** folder.</span></span>
2. <span data-ttu-id="17767-222">Adlı yeni bir özellik eklemek **İpucu**, aşağıdaki kod parçacığında gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="17767-222">Add a new property named **Hint**, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. <span data-ttu-id="17767-223">İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="17767-223">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="17767-224">Yeni bir geçiş modelimizi değişikliği yansıtacak şekilde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="17767-224">A new migration will be created reflecting the change in our model.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    <span data-ttu-id="17767-225">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "eklemek geçiş")</span><span class="sxs-lookup"><span data-stu-id="17767-225">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span></span>

    <span data-ttu-id="17767-226">*Add-Migration*</span><span class="sxs-lookup"><span data-stu-id="17767-226">*Add-Migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-227">Bir geçiş dosyası iki yöntemden oluşan **yukarı** ve **aşağı**.</span><span class="sxs-lookup"><span data-stu-id="17767-227">A Migration file is composed of two methods, **Up** and **Down**.</span></span>
    > 
    > - <span data-ttu-id="17767-228">**Yukarı** yöntemi, ne bizim uygulama veritabanı için geçerli gerek geçerli sürümü değişiklikler belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="17767-228">The **Up** method will be used to specify what changes the current version of our application need to apply to the database.</span></span>
    > - <span data-ttu-id="17767-229">**Aşağı** biz eklediğiniz değişiklikleri geri almak için kullanılan **yukarı** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="17767-229">The **Down** is used to reverse the changes we have added to the **Up** method.</span></span>
    > 
    > <span data-ttu-id="17767-230">Veritabanı geçiş veritabanı güncelleştirildiğinde, bu zaman damgası sırasını ve yalnızca son güncelleştirmeden bu yana kullanılan değil tüm geçişler çalışır ( \_MigrationHistory tablosu izler hangi geçiş uygulanmadı).</span><span class="sxs-lookup"><span data-stu-id="17767-230">When the Database Migration updates the database, it will run all migrations in the timestamp order, and only those that have not been used since the last update (The \_MigrationHistory table keeps track of which migrations have been applied).</span></span> <span data-ttu-id="17767-231">**Yukarı** tüm geçişler yöntemi çağrılır ve biz veritabanına belirttiğiniz değişiklikleri yapar.</span><span class="sxs-lookup"><span data-stu-id="17767-231">The **Up** method of all migrations will be called and will make the changes we have specified to the database.</span></span> <span data-ttu-id="17767-232">Önceki bir geçiş için geri dönün karar verirseniz **aşağı** yöntemi, ters sırada değişiklikleri yinelemek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="17767-232">If we decide to go back to a previous migration, the **Down** method will be called to redo the changes in a reverse order.</span></span>
4. <span data-ttu-id="17767-233">İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="17767-233">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. <span data-ttu-id="17767-234">Çıktısını **Update-Database** oluşturulan komut bir **Alter Table** yeni bir sütun eklemek için SQL deyimi **TriviaQuestions** , aşağıdaki resimde gösterildiği gibi tablo.</span><span class="sxs-lookup"><span data-stu-id="17767-234">The output of the **Update-Database** command generated an **Alter Table** SQL statement to add a new column to the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="17767-235">![Oluşturulan sütun SQL deyimine](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "oluşturulan sütun SQL deyimini ekleyin")</span><span class="sxs-lookup"><span data-stu-id="17767-235">![Add column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Add column SQL statement generated")</span></span>

    <span data-ttu-id="17767-236">*Oluşturulan sütun SQL deyimini ekleyin*</span><span class="sxs-lookup"><span data-stu-id="17767-236">*Add column SQL statement generated*</span></span>
6. <span data-ttu-id="17767-237">İçinde **SQL Server Nesne Gezgini**, yenileme **dbo. TriviaQuestions** tablo ve denetleyin yeni **İpucu** sütununda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="17767-237">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the new **Hint** column is displayed.</span></span>

    <span data-ttu-id="17767-238">![Yeni bir ipucu sütun gösteren](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "yeni ipucu sütunu gösterme")</span><span class="sxs-lookup"><span data-stu-id="17767-238">![Showing the new Hint Column](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Showing the new Hint Column")</span></span>

    <span data-ttu-id="17767-239">*Yeni bir ipucu sütun gösterme*</span><span class="sxs-lookup"><span data-stu-id="17767-239">*Showing the new Hint Column*</span></span>
7. <span data-ttu-id="17767-240">Geri **TriviaQuestion.cs** Düzenleyicisi eklemek bir **StringLength** kısıtlama *İpucu* aşağıdaki kod parçacığında gösterildiği gibi özelliği.</span><span class="sxs-lookup"><span data-stu-id="17767-240">Back in the **TriviaQuestion.cs** editor, add a **StringLength** constraint to the *Hint* property, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. <span data-ttu-id="17767-241">İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="17767-241">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. <span data-ttu-id="17767-242">İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="17767-242">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. <span data-ttu-id="17767-243">Çıktısını **Update-Database** oluşturulan komut bir **Alter Table** güncelleştirmek için SQL deyimi *İpucu* sütunun türü **TriviaQuestions** , aşağıdaki resimde gösterildiği gibi tablo.</span><span class="sxs-lookup"><span data-stu-id="17767-243">The output of the **Update-Database** command generated an **Alter Table** SQL statement to update the *hint* column type of the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="17767-244">![Oluşturulan sütun SQL deyimini alter](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter oluşturulan sütun SQL deyimi")</span><span class="sxs-lookup"><span data-stu-id="17767-244">![Alter column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL statement generated")</span></span>

    <span data-ttu-id="17767-245">*Oluşturulan sütun SQL deyimini değiştirme*</span><span class="sxs-lookup"><span data-stu-id="17767-245">*Alter column SQL statement generated*</span></span>
11. <span data-ttu-id="17767-246">İçinde **SQL Server Nesne Gezgini**, yenileme **dbo. TriviaQuestions** tablo ve denetleyin **İpucu** sütunun türü **nvarchar(150)**.</span><span class="sxs-lookup"><span data-stu-id="17767-246">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the **Hint** column type is **nvarchar(150)**.</span></span>

    <span data-ttu-id="17767-247">![New kısıtlaması gösteren](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "new kısıtlaması gösterme")</span><span class="sxs-lookup"><span data-stu-id="17767-247">![Showing the new constraint](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Showing the new constraint")</span></span>

    <span data-ttu-id="17767-248">*New kısıtlaması gösterme*</span><span class="sxs-lookup"><span data-stu-id="17767-248">*Showing the new constraint*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a><span data-ttu-id="17767-249">Alıştırma 2: hazırlama için bir Web uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="17767-249">Exercise 2: Deploying a Web App to Staging</span></span>

<span data-ttu-id="17767-250">**Web uygulamaları Azure App Service'te** hazırlanmış yayımlamayı gerçekleştirmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="17767-250">**Web Apps in Azure App Service** enables you to perform staged publishing.</span></span> <span data-ttu-id="17767-251">Hazırlanmış yayımlamayı her varsayılan üretim sitesi için hazırlama bir site yuva oluşturur ve hiçbir zaman aşağı bu yuvaları takas olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="17767-251">Staged publishing creates a staging site slot for each default production site and enables you to swap these slots with no down time.</span></span> <span data-ttu-id="17767-252">Bu ortak serbest bırakmadan önce değişiklikleri doğrulamak için artımlı olarak site içeriğini tümleştirmek ve geri değişiklikleri beklendiği gibi çalışmıyorsanız alma gerçekten yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="17767-252">This is really useful to validate changes before releasing to the public, incrementally integrate site content, and roll back if changes are not working as expected.</span></span>

<span data-ttu-id="17767-253">Bu alıştırmada, dağıtacağınız **günlük test** hazırlama ortamına Git kaynak denetimi kullanarak, web uygulamanızın uygulama.</span><span class="sxs-lookup"><span data-stu-id="17767-253">In this exercise, you will deploy the **Geek Quiz** application to the staging environment of your web app using Git source control.</span></span> <span data-ttu-id="17767-254">Bunu yapmak için web uygulaması oluşturma ve Yönetim Portalı'nı gerekli bileşenler sağlama ve yapılandırma bir **Git** depo ve anında iletme uygulamanın kaynak kodu yerel bilgisayarınızdan hazırlama yuvası için.</span><span class="sxs-lookup"><span data-stu-id="17767-254">To do this, you will create the web app and provision the required components at the management portal, configure a **Git** repository and push the application source code from your local computer to the staging slot.</span></span> <span data-ttu-id="17767-255">Üretim veritabanınız güncelleştirmek **Code First Migrations** önceki alıştırmada oluşturulan.</span><span class="sxs-lookup"><span data-stu-id="17767-255">You will also update your production database with the **Code First Migrations** you created in the previous exercise.</span></span> <span data-ttu-id="17767-256">Ardından, işlemi doğrulamak için bu test ortamında uygulama yürütülür.</span><span class="sxs-lookup"><span data-stu-id="17767-256">You will then execute the application in this test environment to verify its operation.</span></span> <span data-ttu-id="17767-257">Memnun sonra onun beklentilerinizi göre çalıştığından, üretim uygulamaya yükseltmez.</span><span class="sxs-lookup"><span data-stu-id="17767-257">Once you are satisfied that it is working according to your expectations, you will promote the application to production.</span></span>

> [!NOTE]
> <span data-ttu-id="17767-258">Hazırlanmış yayımlamayı etkinleştirmek için web uygulaması olmalıdır **Standart mod**.</span><span class="sxs-lookup"><span data-stu-id="17767-258">To enable staged publishing, the web app must be in **Standard mode**.</span></span> <span data-ttu-id="17767-259">Web uygulamanızı standart moda değiştirirseniz, ek ücret tahakkuk olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="17767-259">Note that additional charges will be incurred if you change your web app to Standard mode.</span></span> <span data-ttu-id="17767-260">Fiyatlandırma hakkında daha fazla bilgi için bkz: [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="17767-260">For more information about pricing, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a><span data-ttu-id="17767-261">Görev 1 – Azure App Service'te bir Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="17767-261">Task 1 – Creating a Web App in Azure App Service</span></span>

<span data-ttu-id="17767-262">Bu görevde, bir web uygulaması oluşturacak **Azure App Service** Yönetim Portalı.</span><span class="sxs-lookup"><span data-stu-id="17767-262">In this task, you will create a web app in **Azure App Service** from the management portal.</span></span> <span data-ttu-id="17767-263">Ayrıca yapılandıracak bir **SQL veritabanı** uygulama verilerini kalıcı hale getirmek ve kaynak denetimi için yerel bir Git deposu yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="17767-263">You will also configure a **SQL Database** to persist the application data, and configure a local Git repository for source control.</span></span>

1. <span data-ttu-id="17767-264">Git [Azure Yönetim Portalı](https://manage.windowsazure.com) ve aboneliğinizle ilişkili Microsoft hesabı kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="17767-264">Go to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>

    ![Azure yönetim portalında oturum açın](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    <span data-ttu-id="17767-266">*Azure yönetim portalında oturum açın*</span><span class="sxs-lookup"><span data-stu-id="17767-266">*Sign in to the Azure management portal*</span></span>
2. <span data-ttu-id="17767-267">Tıklatın **yeni** sayfanın altındaki komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="17767-267">Click **New** in the command bar at the bottom of the page.</span></span>

    <span data-ttu-id="17767-268">![Yeni bir web uygulaması oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "yeni bir web uygulaması oluşturma")</span><span class="sxs-lookup"><span data-stu-id="17767-268">![Creating a new web app](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Creating a new web app")</span></span>

    <span data-ttu-id="17767-269">*Yeni bir web uygulaması oluşturma*</span><span class="sxs-lookup"><span data-stu-id="17767-269">*Creating a new web app*</span></span>
3. <span data-ttu-id="17767-270">Tıklatın **işlem**, **Web sitesi** ve ardından **özel Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="17767-270">Click **Compute**, **Website** and then **Custom Create**.</span></span>

    <span data-ttu-id="17767-271">![Özel Oluştur kullanarak yeni bir web uygulaması oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "özel Oluştur kullanarak yeni bir web uygulaması oluşturma")</span><span class="sxs-lookup"><span data-stu-id="17767-271">![Creating a new web app using Custom Create](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Creating a new web app using Custom Create")</span></span>

    <span data-ttu-id="17767-272">*Özel Oluştur kullanarak yeni bir web uygulaması oluşturma*</span><span class="sxs-lookup"><span data-stu-id="17767-272">*Creating a new web app using Custom Create*</span></span>
4. <span data-ttu-id="17767-273">İçinde **yeni Web sitesi - özel Oluştur** iletişim kutusunda, kullanılabilir bir sağlayın **URL** (örneğin *günlük test*), bir konum seçin **bölge** aşağı açılan liste ve select **yeni bir SQL veritabanı oluşturma** içinde **veritabanı** aşağı açılan liste.</span><span class="sxs-lookup"><span data-stu-id="17767-273">In the **New Website - Custom Create** dialog box, provide an available **URL** (e.g. *geek-quiz*), select a location in the **Region** drop-down list, and select **Create a new SQL database** in the **Database** drop-down list.</span></span> <span data-ttu-id="17767-274">Son olarak, seçin **kaynak denetiminden Yayımla** onay kutusunu ve tıklatın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="17767-274">Finally, select the **Publish from source control** check box and click **Next**.</span></span>

    ![Yeni web uygulamasını özelleştirme](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    <span data-ttu-id="17767-276">*Yeni web uygulamasını özelleştirme*</span><span class="sxs-lookup"><span data-stu-id="17767-276">*Customizing the new web app*</span></span>
5. <span data-ttu-id="17767-277">Veritabanı ayarları için aşağıdaki bilgileri belirtin:</span><span class="sxs-lookup"><span data-stu-id="17767-277">Specify the following information for the database settings:</span></span>

   - <span data-ttu-id="17767-278">İçinde **adı** metin kutusunda, bir veritabanı adı girin (örneğin *geekquiz\_db*)</span><span class="sxs-lookup"><span data-stu-id="17767-278">In the **Name** text box, enter a database name (e.g. *geekquiz\_db*)</span></span>
   - <span data-ttu-id="17767-279">Sunucudaki **açılan** listesinde **yeni SQL veritabanı sunucusuna**.</span><span class="sxs-lookup"><span data-stu-id="17767-279">In the Server **drop-down** list, select **New SQL database server**.</span></span> <span data-ttu-id="17767-280">Alternatif olarak, var olan bir sunucu seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="17767-280">Alternatively, you can select an existing server.</span></span>
   - <span data-ttu-id="17767-281">İçinde **veritabanı kullanıcı adı** ve **veritabanı parolasını** kutularına için SQL veritabanı sunucusu yönetici kullanıcı adı ve parola girin.</span><span class="sxs-lookup"><span data-stu-id="17767-281">In the **Database username** and **Database password** boxes, enter the administrator username and password for the SQL database server.</span></span> <span data-ttu-id="17767-282">Bir sunucu seçerseniz oluşturduysanız, parola girmesi istenir.</span><span class="sxs-lookup"><span data-stu-id="17767-282">If you select a server you have already created, you will be prompted for the password.</span></span>

     ![Veritabanı ayarlarını belirtme](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     <span data-ttu-id="17767-284">*Veritabanı ayarlarını belirtme*</span><span class="sxs-lookup"><span data-stu-id="17767-284">*Specifying the database settings*</span></span>
6. <span data-ttu-id="17767-285">Devam etmek için **İleri** 'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="17767-285">Click **Next** to continue.</span></span>
7. <span data-ttu-id="17767-286">Seçin **yerel Git deposu** kullanın ve'ı tıklatın Kaynak denetimi için **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="17767-286">Select **Local Git repository** for the source control to use and click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-287">Dağıtım kimlik bilgileri (kullanıcı adı ve parola) istenebilir.</span><span class="sxs-lookup"><span data-stu-id="17767-287">You may be prompted for the deployment credentials (a username and password).</span></span>

    ![Git deposu oluşturuluyor](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    <span data-ttu-id="17767-289">*Git deposu oluşturuluyor*</span><span class="sxs-lookup"><span data-stu-id="17767-289">*Creating the Git repository*</span></span>
8. <span data-ttu-id="17767-290">Yeni web uygulaması oluşturulana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="17767-290">Wait until the new web app is created.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-291">Varsayılan olarak, etki alanları adresindeki Azure sağlar *azurewebsites.net* ancak aynı zamanda, Azure Yönetim Portalı'nı kullanarak özel etki alanlarını ayarlamak için olanağını sağlar.</span><span class="sxs-lookup"><span data-stu-id="17767-291">By default, Azure provides domains at *azurewebsites.net* but also gives you the possibility to set custom domains using the Azure management portal.</span></span> <span data-ttu-id="17767-292">Ancak, belirli Azure App Service modları kullanıyorsanız, yalnızca özel etki alanlarını yönetebilir.</span><span class="sxs-lookup"><span data-stu-id="17767-292">However, you can only manage custom domains if you are using certain Azure App Service modes.</span></span>
    > 
    > <span data-ttu-id="17767-293">Azure uygulama hizmeti ücretsiz, paylaşılan, temel, standart ve Premium sürümlerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="17767-293">Azure App Service is available in Free, Shared, Basic, Standard, and Premium editions.</span></span> <span data-ttu-id="17767-294">Ücretsiz ve paylaşılan modda, tüm web uygulamaları çok kiracılı bir ortamda çalıştırılabilir ve CPU, bellek ve ağ kullanımı için kotaları sahip.</span><span class="sxs-lookup"><span data-stu-id="17767-294">In Free and Shared mode, all web apps run in a multi-tenant environment and have quotas for CPU, Memory, and Network usage.</span></span> <span data-ttu-id="17767-295">Ücretsiz uygulamalar en fazla planınızla farklılık gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="17767-295">The maximum number of free apps may vary with your plan.</span></span> <span data-ttu-id="17767-296">Standart modda, standart Azure karşılık ayrılmış sanal makinelerde çalıştırma hangi uygulamaların işlem kaynaklarını seçin.</span><span class="sxs-lookup"><span data-stu-id="17767-296">In Standard mode, you choose which apps run on dedicated virtual machines that correspond to the standard Azure compute resources.</span></span> <span data-ttu-id="17767-297">Web uygulama modu yapılandırmasında bulabilirsiniz **ölçek** , web uygulamanızın menüsü.</span><span class="sxs-lookup"><span data-stu-id="17767-297">You can find the web app mode configuration in the **Scale** menu of your web app.</span></span>
    > 
    > <span data-ttu-id="17767-298">![Azure uygulama hizmeti modları](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure uygulama hizmeti modları")</span><span class="sxs-lookup"><span data-stu-id="17767-298">![Azure App Service Modes](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service Modes")</span></span>
    > 
    > <span data-ttu-id="17767-299">Kullanıyorsanız **paylaşılan** veya **standart** modu, görebilirsiniz, uygulamanızın giderek web uygulamanız için özel etki alanlarını yönetmek **yapılandırma** menü ve tıklayarak**Etki alanlarını yönetme** altında *etki alanı adları*.</span><span class="sxs-lookup"><span data-stu-id="17767-299">If you are using **Shared** or **Standard** mode, you will be able to manage custom domains for your web app by going to your app's **Configure** menu and clicking **Manage Domains** under *domain names*.</span></span>
    > 
    > <span data-ttu-id="17767-300">![Etki alanlarını yönetme](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "etki alanlarını yönetme")</span><span class="sxs-lookup"><span data-stu-id="17767-300">![Manage Domains](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Manage Domains")</span></span>
    > 
    > <span data-ttu-id="17767-301">![Özel etki alanlarını yönetme](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "özel etki alanlarını yönetme")</span><span class="sxs-lookup"><span data-stu-id="17767-301">![Manage Custom Domains](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Manage Custom Domains")</span></span>
9. <span data-ttu-id="17767-302">Web uygulaması oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun yeni web uygulaması çalışıp çalışmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="17767-302">Once the web app is created, click the link under the **URL** column to check that the new web app is running.</span></span>

    ![Yeni web uygulamasına göz atma](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    <span data-ttu-id="17767-304">*Yeni web uygulamasına göz atma*</span><span class="sxs-lookup"><span data-stu-id="17767-304">*Browsing to the new web app*</span></span>

    ![çalışan web uygulaması](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    <span data-ttu-id="17767-306">*çalışan web uygulaması*</span><span class="sxs-lookup"><span data-stu-id="17767-306">*web app running*</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a><span data-ttu-id="17767-307">Görev 2 – üretim SQL veritabanı oluşturuluyor</span><span class="sxs-lookup"><span data-stu-id="17767-307">Task 2 – Creating the Production SQL Database</span></span>

<span data-ttu-id="17767-308">Bu görevde, kullanacağınız **Entity Framework Code First Migrations** hedefleme veritabanı oluşturmak için **Azure SQL veritabanı** önceki görevde oluşturduğunuz örneği.</span><span class="sxs-lookup"><span data-stu-id="17767-308">In this task, you will use the **Entity Framework Code First Migrations** to create the database targeting the **Azure SQL Database** instance you created in the previous task.</span></span>

1. <span data-ttu-id="17767-309">Yönetim Portalı'nda önceki görevde oluşturduğunuz web uygulamasına gidin ve Git kendi **Pano**.</span><span class="sxs-lookup"><span data-stu-id="17767-309">In the Management Portal, navigate to the web app you created in the previous task and go to its **Dashboard**.</span></span>
2. <span data-ttu-id="17767-310">İçinde **Pano** sayfasında, **bağlantı dizelerini görüntüle** altında bağlantı **Hızlı Bakış** bölümü.</span><span class="sxs-lookup"><span data-stu-id="17767-310">In the **Dashboard** page, click **View connection strings** link under the **quick glance** section.</span></span>

    <span data-ttu-id="17767-311">![Bağlantı dizelerini görüntüle](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "bağlantı dizelerini görüntüle")</span><span class="sxs-lookup"><span data-stu-id="17767-311">![View connection strings](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "View connection strings")</span></span>

    <span data-ttu-id="17767-312">*Bağlantı dizelerini görüntüle*</span><span class="sxs-lookup"><span data-stu-id="17767-312">*View connection strings*</span></span>
3. <span data-ttu-id="17767-313">Kopya **bağlantı dizesi** değer ve iletişim kutusunu kapatın.</span><span class="sxs-lookup"><span data-stu-id="17767-313">Copy the **connection string** value and close the dialog box.</span></span>

    <span data-ttu-id="17767-314">![Azure Yönetim Portalı'nda bağlantı dizesi](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Azure Yönetim Portalı'nda bağlantı dizesi")</span><span class="sxs-lookup"><span data-stu-id="17767-314">![Connection String in Azure Management Portal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Connection String in Azure Management Portal")</span></span>

    <span data-ttu-id="17767-315">*Azure Yönetim Portalı'nda bağlantı dizesi*</span><span class="sxs-lookup"><span data-stu-id="17767-315">*Connection String in Azure Management Portal*</span></span>
4. <span data-ttu-id="17767-316">Tıklatın **SQL veritabanları** Azure SQL veritabanlarının listesini görmek için</span><span class="sxs-lookup"><span data-stu-id="17767-316">Click **SQL Databases** to see the list of SQL databases in Azure</span></span>

    <span data-ttu-id="17767-317">![SQL veritabanı menü](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL veritabanı menüsü")</span><span class="sxs-lookup"><span data-stu-id="17767-317">![SQL Database menu](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database menu")</span></span>

    <span data-ttu-id="17767-318">*SQL veritabanı menüsü*</span><span class="sxs-lookup"><span data-stu-id="17767-318">*SQL Database menu*</span></span>
5. <span data-ttu-id="17767-319">Önceki görevde oluşturduğunuz veritabanını bulun ve sunucuda tıklatın.</span><span class="sxs-lookup"><span data-stu-id="17767-319">Locate the database you created in the previous task and click on the Server.</span></span>

    <span data-ttu-id="17767-320">![SQL veritabanı sunucusu](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL veritabanı sunucusu")</span><span class="sxs-lookup"><span data-stu-id="17767-320">![SQL Database server](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database server")</span></span>

    <span data-ttu-id="17767-321">*SQL veritabanı sunucusu*</span><span class="sxs-lookup"><span data-stu-id="17767-321">*SQL Database server*</span></span>
6. <span data-ttu-id="17767-322">İçinde **Hızlı Başlangıç** sayfa sunucusunun tıklayın **yapılandırma**.</span><span class="sxs-lookup"><span data-stu-id="17767-322">In the **Quick Start** page of the server, click on **Configure**.</span></span>

    <span data-ttu-id="17767-323">![Yapılandırma menü](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "yapılandırma menüsü")</span><span class="sxs-lookup"><span data-stu-id="17767-323">![Configure menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Configure menu")</span></span>

    <span data-ttu-id="17767-324">*Menü yapılandırın*</span><span class="sxs-lookup"><span data-stu-id="17767-324">*Configure menu*</span></span>
7. <span data-ttu-id="17767-325">İçinde **izin verilen IP adresleri** bölümünde, tıklayın **eklemek için izin verilen IP adreslerini** SQL veritabanı sunucusuna bağlanmak IP etkinleştirmek için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="17767-325">In the **Allowed IP addresses** section, click on **Add to the allowed IP addresses** link to enable your IP to connect to the SQL Database server.</span></span>

    <span data-ttu-id="17767-326">![İzin verilen IP adreslerini](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "izin verilen IP adresleri")</span><span class="sxs-lookup"><span data-stu-id="17767-326">![Allowed IP addresses](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Allowed IP addresses")</span></span>

    <span data-ttu-id="17767-327">*İzin verilen IP adresi*</span><span class="sxs-lookup"><span data-stu-id="17767-327">*Allowed IP addresses*</span></span>
8. <span data-ttu-id="17767-328">Tıklatın **kaydetmek** adımı tamamlamak için sayfanın altındaki.</span><span class="sxs-lookup"><span data-stu-id="17767-328">Click **Save** at the bottom of the page to complete the step.</span></span>
9. <span data-ttu-id="17767-329">Visual Studio'ya geri çevirin.</span><span class="sxs-lookup"><span data-stu-id="17767-329">Switch back to Visual Studio.</span></span>
10. <span data-ttu-id="17767-330">İçinde **Paket Yöneticisi Konsolu**, komutu aşağıdaki değiştirilirken yürütme *[YOUR bağlantı DİZESİ]* Azure'dan kopyaladığınız bağlantı dizesiyle yer tutucusu</span><span class="sxs-lookup"><span data-stu-id="17767-330">In the **Package Manager Console**, execute the following command replacing *[YOUR-CONNECTION-STRING]* placeholder with the connection string you copied from Azure</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    <span data-ttu-id="17767-331">![Windows Azure SQL veritabanı hedefleme güncelleştirme veritabanı](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "güncelleştirme veritabanı Windows Azure SQL veritabanı hedefleme")</span><span class="sxs-lookup"><span data-stu-id="17767-331">![Update database targeting Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Update database targeting Windows Azure SQL Database")</span></span>

    <span data-ttu-id="17767-332">*Azure SQL veritabanı hedefleme veritabanını güncelleştirme*</span><span class="sxs-lookup"><span data-stu-id="17767-332">*Update database targeting Azure SQL Database*</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a><span data-ttu-id="17767-333">Görev 3 – dağıtma günlük test için Git kullanarak hazırlama</span><span class="sxs-lookup"><span data-stu-id="17767-333">Task 3 – Deploying Geek Quiz to Staging Using Git</span></span>

<span data-ttu-id="17767-334">Bu görevde, web uygulamanızı hazırlanmış yayımlamayı etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="17767-334">In this task, you will enable staged publishing in your web app.</span></span> <span data-ttu-id="17767-335">Ardından, web uygulamanızı hazırlama ortamını, doğrudan yerel bilgisayarınızdan günlük test uygulamayı yayımlamak için Git kullanacaktır.</span><span class="sxs-lookup"><span data-stu-id="17767-335">Then, you will use Git to publish the Geek Quiz application directly from your local computer to the staging environment of your web app.</span></span>

1. <span data-ttu-id="17767-336">Portalına geri dönün ve altında web uygulamasının adını tıklatın **adı** yönetim sayfaları görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="17767-336">Go back to the portal and click the name of the web app under the **Name** column to display the management pages.</span></span>

    ![Web uygulaması yönetim sayfalarını açma](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    <span data-ttu-id="17767-338">*Web uygulaması yönetim sayfalarını açma*</span><span class="sxs-lookup"><span data-stu-id="17767-338">*Opening the web app management pages*</span></span>
2. <span data-ttu-id="17767-339">Gidin **ölçek** sayfası.</span><span class="sxs-lookup"><span data-stu-id="17767-339">Navigate to the **Scale** page.</span></span> <span data-ttu-id="17767-340">Altında **genel** bölümünde, select **standart** tıklatın ve yapılandırma için **kaydetmek** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="17767-340">Under the **general** section, select **Standard** for the configuration and click **Save** in the command bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-341">Abonelikte ve geçerli bölge içindeki tüm web uygulamalarının çalıştırılması için **standart** modu, bırakın **Tümünü Seç** onay kutusu seçili **seçin siteleri** yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="17767-341">To run all web apps in the current region and subscription in **Standard** mode, leave the **Select All** check box selected in the **Choose Sites** configuration.</span></span> <span data-ttu-id="17767-342">Aksi takdirde temizleyin **Tümünü Seç** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="17767-342">Otherwise, clear the **Select All** check box.</span></span>

    <span data-ttu-id="17767-343">![Web uygulaması Standart modu yükseltme](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "standart moda web uygulamasını yükseltme")</span><span class="sxs-lookup"><span data-stu-id="17767-343">![Upgrading the web app to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Upgrading the web app to Standard mode")</span></span>

    <span data-ttu-id="17767-344">*Web uygulaması Standart modu yükseltme*</span><span class="sxs-lookup"><span data-stu-id="17767-344">*Upgrading the Web App to Standard mode*</span></span>
3. <span data-ttu-id="17767-345">Tıklatın **Evet** değişiklikleri onaylamak için.</span><span class="sxs-lookup"><span data-stu-id="17767-345">Click **Yes** to confirm the changes.</span></span>

    <span data-ttu-id="17767-346">![Standart mod değişikliği onayladığınız](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "web uygulaması modunu değiştirme ile devam etmeden")</span><span class="sxs-lookup"><span data-stu-id="17767-346">![Confirming the change to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuing with the changing of the web app mode")</span></span>

    <span data-ttu-id="17767-347">*Standart mod değişikliği onaylama*</span><span class="sxs-lookup"><span data-stu-id="17767-347">*Confirming the change to Standard mode*</span></span>
4. <span data-ttu-id="17767-348">Git **Pano** sayfasında ve tıklayın **etkinleştirme yayımlamayı hazırlanan** altında **Hızlı Bakış** bölümü.</span><span class="sxs-lookup"><span data-stu-id="17767-348">Go to the **Dashboard** page and click **Enable staged publishing** under the **quick glance** section.</span></span>

    <span data-ttu-id="17767-349">![Aşamalı yayımlamayı etkinleştirmeye](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "etkinleştirme yayımlamayı hazırlanan")</span><span class="sxs-lookup"><span data-stu-id="17767-349">![Enabling staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Enabling staged publishing")</span></span>

    <span data-ttu-id="17767-350">*Hazırlanmış yayımlamayı etkinleştirme*</span><span class="sxs-lookup"><span data-stu-id="17767-350">*Enabling staged publishing*</span></span>
5. <span data-ttu-id="17767-351">Tıklatın **Evet** hazırlanmış yayımlamayı etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="17767-351">Click **Yes** to enable staged publishing.</span></span>

    <span data-ttu-id="17767-352">![Onaylama hazırlanmış yayımlamayı](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "hazırlanmış yayımlamayı etkinleştirmek için Evet'i tıklatmak")</span><span class="sxs-lookup"><span data-stu-id="17767-352">![Confirming staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Clicking Yes to enable staged publishing")</span></span>

    <span data-ttu-id="17767-353">*Hazırlanmış yayımlamayı onaylama*</span><span class="sxs-lookup"><span data-stu-id="17767-353">*Confirming staged publishing*</span></span>
6. <span data-ttu-id="17767-354">Web uygulamaları listesinde işareti hazırlama yuvasıyla görüntülemek için web uygulaması adı soluna genişletin.</span><span class="sxs-lookup"><span data-stu-id="17767-354">In the list of web apps, expand the mark to the left of your web app name to display the staging site slot.</span></span> <span data-ttu-id="17767-355">Ardından, web uygulamanızın adına sahip ***(hazırlama)***.</span><span class="sxs-lookup"><span data-stu-id="17767-355">It has the name of your web app followed by ***(staging)***.</span></span> <span data-ttu-id="17767-356">Yönetim sayfasına gitmek için hazırlama sitesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="17767-356">Click the staging site to go to the management page.</span></span>

    <span data-ttu-id="17767-357">![Hazırlama web uygulaması'na giderek](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "hazırlama web uygulamasına gidin")</span><span class="sxs-lookup"><span data-stu-id="17767-357">![Navigating to the staging web app](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigating to the staging web app")</span></span>

    <span data-ttu-id="17767-358">*Hazırlama uygulamasına gidin*</span><span class="sxs-lookup"><span data-stu-id="17767-358">*Navigating to the staging app*</span></span>
7. <span data-ttu-id="17767-359">Herhangi diğer web uygulamanızın Yönetim sayfası gibi he Yönetim sayfasında görünür dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="17767-359">Notice that he management page looks like any other web app's management page.</span></span> <span data-ttu-id="17767-360">Gidin **dağıtımları** sayfası ve kopyalama **Git URL'si** değeri.</span><span class="sxs-lookup"><span data-stu-id="17767-360">Navigate to the **Deployments** page and copy the **Git URL** value.</span></span> <span data-ttu-id="17767-361">Bu alıştırmada, onu kullanır.</span><span class="sxs-lookup"><span data-stu-id="17767-361">You will use it later in this exercise.</span></span>

    ![Git URL değeri kopyalama](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    <span data-ttu-id="17767-363">*Git URL değeri kopyalama*</span><span class="sxs-lookup"><span data-stu-id="17767-363">*Copying the Git URL value*</span></span>
8. <span data-ttu-id="17767-364">Yeni bir **Git Bash** konsol ve aşağıdaki komutları yürütün.</span><span class="sxs-lookup"><span data-stu-id="17767-364">Open a new **Git Bash** console and execute the following commands.</span></span> <span data-ttu-id="17767-365">Güncelleştirme *[YOUR uygulama yolu]* yolu ile yer tutucu **GeekQuiz** bulunan Çözüm **Source\Ex1 DeployingWebSiteToStaging\Begin** klasörü Bu laboratuvar.</span><span class="sxs-lookup"><span data-stu-id="17767-365">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution, located in the **Source\Ex1-DeployingWebSiteToStaging\Begin** folder of this lab.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git başlatma ve ilk tamamlama](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    <span data-ttu-id="17767-367">*Git başlatma ve ilk tamamlama*</span><span class="sxs-lookup"><span data-stu-id="17767-367">*Git initialization and first commit*</span></span>
9. <span data-ttu-id="17767-368">Web uygulamanız için uzaktan göndermek için aşağıdaki komutu çalıştırın **Git** deposu.</span><span class="sxs-lookup"><span data-stu-id="17767-368">Run the following command to push your web app to the remote **Git** repository.</span></span> <span data-ttu-id="17767-369">Yer tutucu Yönetim Portalı'ndan alınan URL ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="17767-369">Replace the placeholder with the URL you obtained from the management portal.</span></span> <span data-ttu-id="17767-370">Dağıtım parolanızı girmeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="17767-370">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Windows Azure iletme](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    <span data-ttu-id="17767-372">*Azure'a dağıtmaya*</span><span class="sxs-lookup"><span data-stu-id="17767-372">*Pushing to Azure*</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-373">FTP ana bilgisayarına veya bir web uygulaması GIT deposuna içerik dağıttığınızda, kullanarak kimliğini doğrulaması gerekir **dağıtım kimlik bilgileri** web uygulamasından 's oluşturduğunuz **Hızlı Başlangıç** veya **Panosu**  yönetim sayfaları.</span><span class="sxs-lookup"><span data-stu-id="17767-373">When you deploy content to the FTP host or GIT repository of a web app, you must authenticate using the **deployment credentials** that you created from the web app's **Quick Start** or **Dashboard** management pages.</span></span> <span data-ttu-id="17767-374">Dağıtım kimlik bilgilerinizi bilmiyorsanız, bunları Yönetim Portalı'nı kullanarak kolayca sıfırlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="17767-374">If you do not know your deployment credentials you can easily reset them using the management portal.</span></span> <span data-ttu-id="17767-375">Web uygulamasını açın **Pano** sayfasında ve tıklayın **dağıtım kimlik bilgilerinizi sıfırlayın** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="17767-375">Open the web app **Dashboard** page and click the **Reset your deployment credentials** link.</span></span> <span data-ttu-id="17767-376">Yeni bir parola sağlayın ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="17767-376">Provide a new password and click **OK**.</span></span> <span data-ttu-id="17767-377">Dağıtım kimlik bilgileri, aboneliğinizle ilişkilendirilmiş tüm web uygulamaları ile kullanım için geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="17767-377">Deployment credentials are valid for use with all web apps associated with your subscription.</span></span>
10. <span data-ttu-id="17767-378">Web uygulaması için Azure başarıyla gönderilen doğrulamak için Yönetim Portalı'na geri dönün ve **Web siteleri**.</span><span class="sxs-lookup"><span data-stu-id="17767-378">In order to verify the web app was successfully pushed to Azure, go back to the management portal and click **Websites**.</span></span>
11. <span data-ttu-id="17767-379">Web uygulamanızı bulun ve hazırlama yuvasıyla görüntülemek için giriş genişletin.</span><span class="sxs-lookup"><span data-stu-id="17767-379">Locate your web app and expand the entry to display the staging site slot.</span></span> <span data-ttu-id="17767-380">' I tıklatın, **adı** yönetimi sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="17767-380">Click its **Name** to go to the management page.</span></span>
12. <span data-ttu-id="17767-381">Tıklatın **dağıtımları** görmek için **dağıtım geçmişi**.</span><span class="sxs-lookup"><span data-stu-id="17767-381">Click **Deployments** to see the **deployment history**.</span></span> <span data-ttu-id="17767-382">Olduğunu doğrulayın bir **etkin dağıtım** ile  *&quot;ilk yürütme&quot;*.</span><span class="sxs-lookup"><span data-stu-id="17767-382">Verify that there is an **Active Deployment** with your *&quot;Initial Commit&quot;*.</span></span>

    ![Etkin dağıtım](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    <span data-ttu-id="17767-384">*Etkin dağıtım*</span><span class="sxs-lookup"><span data-stu-id="17767-384">*Active deployment*</span></span>
13. <span data-ttu-id="17767-385">Son olarak, tıklatın **Gözat** komut çubuğunda web uygulamasına gidin.</span><span class="sxs-lookup"><span data-stu-id="17767-385">Finally, click **Browse** in the command bar to go to the web app.</span></span>

    ![Web uygulaması Gözat](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    <span data-ttu-id="17767-387">*Web uygulaması Gözat*</span><span class="sxs-lookup"><span data-stu-id="17767-387">*Browse web app*</span></span>
14. <span data-ttu-id="17767-388">Uygulama başarıyla dağıtılır günlük test oturum açma sayfasını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="17767-388">If the application is successfully deployed, you will see the Geek Quiz login page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-389">Dağıtılmış uygulamanın adresi URL'sini ve ardından web uygulamanızı adını içeren *-hazırlama*.</span><span class="sxs-lookup"><span data-stu-id="17767-389">The address URL of the deployed application contains the name of your web app followed by *-staging*.</span></span>

    ![Hazırlama ortamında çalışan uygulama](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    <span data-ttu-id="17767-391">*Hazırlama ortamında çalışan uygulama*</span><span class="sxs-lookup"><span data-stu-id="17767-391">*Application running in the staging environment*</span></span>
15. <span data-ttu-id="17767-392">Uygulama keşfetmek isterseniz tıklayın **kaydetmek** yeni bir kullanıcı kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="17767-392">If you wish to explore the application, click **Register** to register a new user.</span></span> <span data-ttu-id="17767-393">Hesap ayrıntıları, bir kullanıcı adı, e-posta adresi ve parola girerek tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="17767-393">Complete the account details by entering a user name, email address and password.</span></span> <span data-ttu-id="17767-394">Ardından, uygulamayı test ilk soru gösterir.</span><span class="sxs-lookup"><span data-stu-id="17767-394">Next, the application shows the first question of the quiz.</span></span> <span data-ttu-id="17767-395">Beklendiği gibi çalıştığından emin olmak için birkaç soruları yanıtlayın.</span><span class="sxs-lookup"><span data-stu-id="17767-395">Answer a few questions to make sure it is working as expected.</span></span>

    ![Uygulama kullanılmaya hazır](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    <span data-ttu-id="17767-397">*Uygulama kullanılmaya hazır*</span><span class="sxs-lookup"><span data-stu-id="17767-397">*Application ready to be used*</span></span>

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a><span data-ttu-id="17767-398">Görev 4 – üretim Web uygulamasına yükseltme</span><span class="sxs-lookup"><span data-stu-id="17767-398">Task 4 – Promoting the Web App to Production</span></span>

<span data-ttu-id="17767-399">Web uygulaması hazırlama ortamında düzgün çalıştığını doğruladıktan, üretime yükseltmek hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="17767-399">Now that you have verified that the web app is working correctly in the staging environment, you are ready to promote it to production.</span></span> <span data-ttu-id="17767-400">Bu görevde, üretim yuvasıyla ile hazırlama site yuvası takas.</span><span class="sxs-lookup"><span data-stu-id="17767-400">In this task, you will swap the staging site slot with the production site slot.</span></span>

1. <span data-ttu-id="17767-401">Yönetim Portalı'na geri dönün ve hazırlama site yuvası seçin.</span><span class="sxs-lookup"><span data-stu-id="17767-401">Go back to the management portal and select the staging site slot.</span></span> <span data-ttu-id="17767-402">Tıklatın **takas** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="17767-402">Click **Swap** in the command bar.</span></span>

    ![Üretim için değiştirme](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    <span data-ttu-id="17767-404">*Üretim için değiştirme*</span><span class="sxs-lookup"><span data-stu-id="17767-404">*Swap to production*</span></span>
2. <span data-ttu-id="17767-405">Tıklatın **Evet** takas işlemine devam etmek için onay iletişim kutusunda.</span><span class="sxs-lookup"><span data-stu-id="17767-405">Click **Yes** in the confirmation dialog box to proceed with the swap operation.</span></span> <span data-ttu-id="17767-406">Azure hemen hazırlama site içeriğini üretim sitesini içerikle değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="17767-406">Azure will immediately swap the content of the production site with the content of the staging site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-407">Bazı ayarlar hazırlanmış sürümünden (örn. bağlantı dizesi geçersiz kılmalar, işleyici eşlemeleri, vb.) üretim sürümü için otomatik olarak kopyalanır, ancak diğer ayarları değiştirmez (örn. DNS uç noktaları, SSL bağlamaları, vb.).</span><span class="sxs-lookup"><span data-stu-id="17767-407">Some settings from the staged version will automatically be copied to the production version (e.g. connection string overrides, handler mappings, etc.) but other settings will not change (e.g. DNS endpoints, SSL bindings, etc.).</span></span>

    ![Değiştirme işlemi onaylama](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    <span data-ttu-id="17767-409">*Değiştirme işlemi onaylama*</span><span class="sxs-lookup"><span data-stu-id="17767-409">*Confirming swap operation*</span></span>
3. <span data-ttu-id="17767-410">Değiştirme işlemi tamamlandıktan sonra üretim yuvasına seçin ve tıklatın **Gözat** üretim sitesini açmak için komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="17767-410">Once the swap is complete, select the production slot and click **Browse** in the command bar to open the production site.</span></span> <span data-ttu-id="17767-411">Adres çubuğundaki URL'yi dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="17767-411">Notice the URL in the address bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-412">Önbelleği temizlemek için tarayıcınızı yenilemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="17767-412">You might need to refresh your browser to clear cache.</span></span> <span data-ttu-id="17767-413">Internet Explorer'da tuşlarına basarak bunu yapabilirsiniz **CTRL + R**.</span><span class="sxs-lookup"><span data-stu-id="17767-413">In Internet Explorer, you can do this by pressing **CTRL+R**.</span></span>

    ![Üretim ortamında çalışan web uygulaması](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. <span data-ttu-id="17767-415">İçinde **Gitbash'i** konsol, üretim yuvasına hedeflemek için yerel Git deposu uzak URL güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="17767-415">In the **GitBash** console, update the remote URL for the local Git repository to target the production slot.</span></span> <span data-ttu-id="17767-416">Bunu yapmak için yer tutucuları dağıtım kullanıcı adınızı ve web uygulamanızın adıyla değiştirerek aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="17767-416">To do this, run the following command replacing the placeholders with your deployment username and the name of your web app.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-417">Aşağıdaki alıştırmalarda yalnızca Laboratuvar Basitlik için hazırlama yerine üretim siteye değişiklikleri gönderir.</span><span class="sxs-lookup"><span data-stu-id="17767-417">In the following exercises, you will push changes to the production site instead of staging just for the simplicity of the lab.</span></span> <span data-ttu-id="17767-418">Gerçek dünya senaryoda üretime yükseltmeden önce hazırlama ortamında değişiklikleri doğrulamak için önerilir.</span><span class="sxs-lookup"><span data-stu-id="17767-418">In a real-world scenario, it is recommended to verify the changes in the staging environment before promoting to production.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a><span data-ttu-id="17767-419">Alıştırma 3: Dağıtımı geri alma üretimde gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="17767-419">Exercise 3: Performing Deployment Rollback in Production</span></span>

<span data-ttu-id="17767-420">Burada sahip olmadığınız arasında hazırlama ve üretim, örneğin, sık kullanılan takas gerçekleştirmek için bir hazırlama yuvası ile çalışıyorsanız senaryolarda **serbest** veya **paylaşılan** modu.</span><span class="sxs-lookup"><span data-stu-id="17767-420">There are scenarios where you do not have a staging slot to perform hot swap between staging and production, for example, if you are working with **Free** or **Shared** mode.</span></span> <span data-ttu-id="17767-421">Bu senaryolarda, uygulamanızı bir sınama ortamında – yerel veya uzak bir sitedeki – üretime dağıtılmadan önce test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="17767-421">In those scenarios, you should test your application in a testing environment –either locally or in a remote site– before deploying to production.</span></span> <span data-ttu-id="17767-422">Ancak, test aşaması sırasında algılanmadı sorunu üretim sitede kaynaklanabilecek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="17767-422">However, it is possible that an issue not detected during the testing phase may arise in the production site.</span></span> <span data-ttu-id="17767-423">Bu durumda, mümkün olan en kısa sürede uygulamanın önceki ve daha kararlı sürümünü arasında kolayca geçiş için bir mekanizma olması önemlidir.</span><span class="sxs-lookup"><span data-stu-id="17767-423">In this case, it is important to have a mechanism to easily switch to a previous and more stable version of the application as quickly as possible.</span></span>

<span data-ttu-id="17767-424">İçinde **Azure App Service**, kaynak denetiminden sürekli dağıtım yapar bu olası teşekkür **dağıtmanız** eylemi Yönetim Portalı'nda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="17767-424">In **Azure App Service**, continuous deployment from source control makes this possible thanks to the **redeploy** action available in the management portal.</span></span> <span data-ttu-id="17767-425">Azure depoya gönderilen işlemeleri ile ilişkilendirilen dağıtımları izler ve herhangi bir önceki dağıtımlarınızı herhangi bir zamanda kullanarak uygulamanızı yeniden dağıtmak için bir seçenek sağlar.</span><span class="sxs-lookup"><span data-stu-id="17767-425">Azure keeps track of the deployments associated with the commits pushed to the repository and provides an option to redeploy your application using any of your previous deployments, at any time.</span></span>

<span data-ttu-id="17767-426">Bu alıştırmada kodda değişiklik gerçekleştirecek **günlük test** bilerek yerleştirir uygulama bir *hata*.</span><span class="sxs-lookup"><span data-stu-id="17767-426">In this exercise you will perform a change to the code in the **Geek Quiz** application that intentionally injects a *bug*.</span></span> <span data-ttu-id="17767-427">Hatayı görmek için üretim uygulamasına dağıtır ve ardından önceki bir duruma geri dönmek için yeniden dağıtın özelliğinin avantajlarından yararlanmak.</span><span class="sxs-lookup"><span data-stu-id="17767-427">You will deploy the application to production to see the error, and then you will take advantage of the redeploy feature to go back to the previous state.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a><span data-ttu-id="17767-428">Görev 1 – günlük test uygulaması güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="17767-428">Task 1 – Updating the Geek Quiz Application</span></span>

<span data-ttu-id="17767-429">Bu görevde kodunu küçük bir parçasını yeniden düzenlemeniz **TriviaController** seçili test seçeneği veritabanından yeni bir yöntemi alır mantığının parçası ayıklamak için sınıf.</span><span class="sxs-lookup"><span data-stu-id="17767-429">In this task, you will refactor a small piece of code of the **TriviaController** class to extract part of the logic that retrieves the selected quiz option from the database into a new method.</span></span>

1. <span data-ttu-id="17767-430">Visual Studio örneğiyle geçin **GeekQuiz** önceki alıştırmada çözümden.</span><span class="sxs-lookup"><span data-stu-id="17767-430">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="17767-431">İçinde **Çözüm Gezgini**, açık **TriviaController.cs** içinde dosya **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="17767-431">In **Solution Explorer**, open the **TriviaController.cs** file inside the **Controllers** folder.</span></span>
3. <span data-ttu-id="17767-432">Bulun **StoreAsync** yöntemi ve kod aşağıdaki çizimde vurgulanan seçin.</span><span class="sxs-lookup"><span data-stu-id="17767-432">Locate the **StoreAsync** method and select the code highlighted in the following figure.</span></span>

    ![Kod seçme](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    <span data-ttu-id="17767-434">*Kod seçme*</span><span class="sxs-lookup"><span data-stu-id="17767-434">*Selecting the code*</span></span>
4. <span data-ttu-id="17767-435">Seçili kodu sağ tıklayın, genişletin **yeniden düzenlemeniz** menü ve select **ayıklama yöntemi...** .</span><span class="sxs-lookup"><span data-stu-id="17767-435">Right-click the selected code, expand the **Refactor** menu and select **Extract Method...**.</span></span>

    ![Yeni bir yöntem olarak kodu ayıklanıyor](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    <span data-ttu-id="17767-437">*Ayıklama yöntemi seçme*</span><span class="sxs-lookup"><span data-stu-id="17767-437">*Selecting Extract Method*</span></span>
5. <span data-ttu-id="17767-438">İçinde **ayıklama yöntemi** iletişim kutusunda, yeni yöntemin adı *MatchesOption* tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="17767-438">In the **Extract Method** dialog box, name the new method *MatchesOption* and click **OK**.</span></span>

    ![Yöntem adı belirtme](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    <span data-ttu-id="17767-440">*Ayıklanan yöntemin adını belirtme*</span><span class="sxs-lookup"><span data-stu-id="17767-440">*Specifying the name for the extracted method*</span></span>
6. <span data-ttu-id="17767-441">Seçili kod içine ayıklanır **MatchesOption** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="17767-441">The selected code is then extracted into the **MatchesOption** method.</span></span> <span data-ttu-id="17767-442">Sonuçta elde edilen kod aşağıdaki kod parçacığında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="17767-442">The resulting code is shown in the following snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. <span data-ttu-id="17767-443">Tuşuna **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="17767-443">Press **CTRL + S** to save the changes.</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a><span data-ttu-id="17767-444">Görev 2 – günlük test uygulamanızı yeniden dağıtmanıza gerek</span><span class="sxs-lookup"><span data-stu-id="17767-444">Task 2 – Redeploying the Geek Quiz Application</span></span>

<span data-ttu-id="17767-445">Üretim ortamına yeni bir dağıtım tetikleyecek depo önceki görevde yapılan değişiklikleri şimdi gönderir.</span><span class="sxs-lookup"><span data-stu-id="17767-445">You will now push the changes you made in the previous task to the repository, which will trigger a new deployment to the production environment.</span></span> <span data-ttu-id="17767-446">Ardından, troubleshot kullanarak bir sorun **F12 geliştirme araçları** Internet Explorer tarafından sağlanan ve Azure Yönetim Portalı'ndan bir geri alma önceki dağıtım gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="17767-446">Then, you will troubleshot an issue using the **F12 development tools** provided by Internet Explorer, and then perform a rollback to the previous deployment from the Azure management portal.</span></span>

1. <span data-ttu-id="17767-447">Yeni bir **Git Bash** güncelleştirilmiş uygulamayı Azure App Service'e dağıtmak için konsol.</span><span class="sxs-lookup"><span data-stu-id="17767-447">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
2. <span data-ttu-id="17767-448">Azure'a değişiklikleri göndermek için aşağıdaki komutları yürütün.</span><span class="sxs-lookup"><span data-stu-id="17767-448">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="17767-449">Güncelleştirme *[YOUR uygulama yolu]* yolu ile yer tutucu **GeekQuiz** çözümü.</span><span class="sxs-lookup"><span data-stu-id="17767-449">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="17767-450">Dağıtım parolanızı girmeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="17767-450">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Azure koymadan işlenmiş kodu](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    <span data-ttu-id="17767-452">*Azure koymadan işlenmiş kodu*</span><span class="sxs-lookup"><span data-stu-id="17767-452">*Pushing refactored code to Azure*</span></span>
3. <span data-ttu-id="17767-453">Internet Explorer'ı açın ve web uygulamanıza gidin (örneğin `http://<your-web-site>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="17767-453">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="17767-454">Önceden oluşturulmuş kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="17767-454">Log in using the previously created credentials.</span></span>
4. <span data-ttu-id="17767-455">Tuşuna **F12** geliştirme araçları başlatmak için seçin **ağ** sekmesinde **Yürüt** kaydı başlatmak için düğmesi.</span><span class="sxs-lookup"><span data-stu-id="17767-455">Press **F12** to launch the development tools, select the **Network** tab and click the **Play** button to start recording.</span></span>

    <span data-ttu-id="17767-456">![Ağ kayıt başlangıç](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "ağ kayıt başlatılıyor")</span><span class="sxs-lookup"><span data-stu-id="17767-456">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Starting network recording")</span></span>

    <span data-ttu-id="17767-457">*Başlangıç ağ kaydı*</span><span class="sxs-lookup"><span data-stu-id="17767-457">*Starting network recording*</span></span>
5. <span data-ttu-id="17767-458">Test herhangi bir seçenek seçin.</span><span class="sxs-lookup"><span data-stu-id="17767-458">Select any option of the quiz.</span></span> <span data-ttu-id="17767-459">Hiçbir şey olmaz görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="17767-459">You will see that nothing happens.</span></span>
6. <span data-ttu-id="17767-460">İçinde **F12** penceresi, POST HTTP isteğine karşılık gelen girişi gösterir bir HTTP **500** sonucu.</span><span class="sxs-lookup"><span data-stu-id="17767-460">In the **F12** window, the entry corresponding to the POST HTTP request shows an HTTP **500** result.</span></span>

    ![HTTP 500 hata](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    <span data-ttu-id="17767-462">*HTTP 500 hata*</span><span class="sxs-lookup"><span data-stu-id="17767-462">*HTTP 500 error*</span></span>
7. <span data-ttu-id="17767-463">Seçin **konsol** sekmesi. Bir hata neden ayrıntıları ile günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="17767-463">Select the **Console** tab. An error is logged with the details of the cause.</span></span>

    ![Günlüğe kaydedilen hata](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    <span data-ttu-id="17767-465">*Günlüğe kaydedilen hata*</span><span class="sxs-lookup"><span data-stu-id="17767-465">*Logged error*</span></span>
8. <span data-ttu-id="17767-466">Hata ayrıntılarını bölümünü bulun.</span><span class="sxs-lookup"><span data-stu-id="17767-466">Locate the details part of the error.</span></span> <span data-ttu-id="17767-467">Açıkçası, bu hata, önceki adımda kaydedilen yeniden düzenleme kod kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="17767-467">Clearly, this error is caused by the code refactoring you committed in the previous steps.</span></span>

    <span data-ttu-id="17767-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span><span class="sxs-lookup"><span data-stu-id="17767-468">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span></span>
9. <span data-ttu-id="17767-469">Tarayıcı kapatmayın.</span><span class="sxs-lookup"><span data-stu-id="17767-469">Do not close the browser.</span></span>
10. <span data-ttu-id="17767-470">Yeni bir tarayıcı örneğiyle gidin [Azure Yönetim Portalı](https://manage.windowsazure.com) ve aboneliğinizle ilişkili Microsoft hesabı kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="17767-470">In a new browser instance, navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
11. <span data-ttu-id="17767-471">Seçin **Web siteleri** ve alıştırma 2'de oluşturulan web uygulaması'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="17767-471">Select **Websites** and click the web app you created in Exercise 2.</span></span>
12. <span data-ttu-id="17767-472">Gidin **dağıtımları** sayfası.</span><span class="sxs-lookup"><span data-stu-id="17767-472">Navigate to the **Deployments** page.</span></span> <span data-ttu-id="17767-473">Gerçekleştirilen tüm işlemeleri dağıtım geçmişini listelenen dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="17767-473">Notice that all the commits performed are listed in the deployment history.</span></span>

    ![Var olan dağıtımlar listesi](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    <span data-ttu-id="17767-475">*Var olan dağıtımlar listesi*</span><span class="sxs-lookup"><span data-stu-id="17767-475">*List of existing deployments*</span></span>
13. <span data-ttu-id="17767-476">Önceki yürütme tıklatıp **dağıtmanız** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="17767-476">Select the previous commit and click **Redeploy** on the command bar.</span></span>

    ![Önceki yürütme dağıtarak](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    <span data-ttu-id="17767-478">*Önceki yürütme dağıtarak*</span><span class="sxs-lookup"><span data-stu-id="17767-478">*Redeploying the previous commit*</span></span>
14. <span data-ttu-id="17767-479">Onaylamanız istendiğinde tıklatın **Evet**.</span><span class="sxs-lookup"><span data-stu-id="17767-479">When prompted to confirm, click **Yes**.</span></span>

    ![Yeniden dağıtım onaylama](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. <span data-ttu-id="17767-481">Dağıtım tamamlandığında, web app ve tuşuna tarayıcı örneğiyle dönmek **CTRL + F5**.</span><span class="sxs-lookup"><span data-stu-id="17767-481">When the deployment completes, switch back to the browser instance with your web app and press **CTRL + F5**.</span></span>
16. <span data-ttu-id="17767-482">Seçeneklerden birini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="17767-482">Click any of the options.</span></span> <span data-ttu-id="17767-483">Çevir animasyon artık yerinde ve sonucu olur (*düzeltmek/yanlış*) görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="17767-483">The flip animation will now take place and the result (*correct/incorrect*) will be displayed.</span></span>
17. <span data-ttu-id="17767-484">(İsteğe bağlı) Geçiş **Git Bash** konsol ve önceki yürütme döndürmek için aşağıdaki komutları yürütün.</span><span class="sxs-lookup"><span data-stu-id="17767-484">(Optional) Switch to the **Git Bash** console and execute the following commands to revert to the previous commit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-485">Bu komutlar Git deposu bozuk yürütmede yapılan tüm değişiklikleri geri alır yeni bir kayıt oluşturun.</span><span class="sxs-lookup"><span data-stu-id="17767-485">These commands create a new commit that undoes all changes in the Git repository that were made in the bad commit.</span></span> <span data-ttu-id="17767-486">Azure sonra yeni tamamlama kullanarak uygulamayı yeniden Dağıt.</span><span class="sxs-lookup"><span data-stu-id="17767-486">Azure will then redeploy the application using the new commit.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a><span data-ttu-id="17767-487">Alıştırma 4: Azure Storage kullanma ölçeklendirme</span><span class="sxs-lookup"><span data-stu-id="17767-487">Exercise 4: Scaling Using Azure Storage</span></span>

<span data-ttu-id="17767-488">**BLOB'ları** büyük miktarlarda yapılandırılmamış metin veya ikili veri video, ses ve görüntü gibi depolamak için kolay bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="17767-488">**Blobs** are the simplest way to store large amounts of unstructured text or binary data such as video, audio and images.</span></span> <span data-ttu-id="17767-489">Statik içerik depolama için uygulamanızın taşıma, görüntülerin veya belgelerin doğrudan tarayıcıya sunarak uygulamanız ölçeği yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="17767-489">Moving the static content of your application to Storage, helps to scale your application by serving images or documents directly to the browser.</span></span>

<span data-ttu-id="17767-490">Bu alıştırmada, bir Blob kapsayıcısını, uygulamanızın statik içerik taşınır.</span><span class="sxs-lookup"><span data-stu-id="17767-490">In this exercise, you will move the static content of your application to a Blob container.</span></span> <span data-ttu-id="17767-491">Sonra da eklemek için uygulamanızın yapılandıracak bir **ASP.NET URL yeniden yazma kuralı** içinde **Web.config** içeriğinizi Blob kapsayıcısına yeniden yönlendirmek için.</span><span class="sxs-lookup"><span data-stu-id="17767-491">Then you will configure your application to add an **ASP.NET URL rewrite rule** in the **Web.config** to redirect your content to the Blob container.</span></span>

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a><span data-ttu-id="17767-492">Görev 1 – bir Azure depolama hesabı oluşturma</span><span class="sxs-lookup"><span data-stu-id="17767-492">Task 1 – Creating an Azure Storage Account</span></span>

<span data-ttu-id="17767-493">Bu görevde, Yönetim Portalı'nı kullanarak yeni bir depolama hesabının nasıl oluşturulacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="17767-493">In this task you will learn how to create a new storage account using the management portal.</span></span>

1. <span data-ttu-id="17767-494">Gidin [Azure Yönetim Portalı](https://manage.windowsazure.com) ve aboneliğinizle ilişkili Microsoft hesabı kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="17767-494">Navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
2. <span data-ttu-id="17767-495">Seçin **yeni | Veri Hizmetleri | Depolama | Hızlı Oluştur** yeni bir depolama hesabı oluşturmaya başlamak için.</span><span class="sxs-lookup"><span data-stu-id="17767-495">Select **New | Data Services | Storage | Quick Create** to start creating a new storage account.</span></span> <span data-ttu-id="17767-496">Seçin ve hesabı için benzersiz bir ad girin bir **bölge** listeden.</span><span class="sxs-lookup"><span data-stu-id="17767-496">Enter a unique name for the account and select a **Region** from the list.</span></span> <span data-ttu-id="17767-497">Tıklatın **depolama hesabı oluştur** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="17767-497">Click **Create Storage Account** to continue.</span></span>

    <span data-ttu-id="17767-498">![Yeni bir depolama hesabı oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "yeni bir depolama hesabı oluşturma")</span><span class="sxs-lookup"><span data-stu-id="17767-498">![Creating a new Storage Account](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Creating a new Storage Account")</span></span>

    <span data-ttu-id="17767-499">*Yeni bir depolama hesabı oluşturma*</span><span class="sxs-lookup"><span data-stu-id="17767-499">*Creating a new storage account*</span></span>
3. <span data-ttu-id="17767-500">İçinde **depolama** bölümünde, yeni depolama hesabı durumunu olana kadar bekleyin *çevrimiçi* aşağıdaki adımıyla devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="17767-500">In the **Storage** section, wait until the status of the new storage account changes to *Online* in order to continue with the following step.</span></span>

    <span data-ttu-id="17767-501">![Oluşturulan depolama hesabı](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "depolama hesabı oluşturuldu")</span><span class="sxs-lookup"><span data-stu-id="17767-501">![Storage Account created](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Storage Account created")</span></span>

    <span data-ttu-id="17767-502">*Depolama hesabı oluşturuldu*</span><span class="sxs-lookup"><span data-stu-id="17767-502">*Storage Account created*</span></span>
4. <span data-ttu-id="17767-503">Depolama hesabının adına tıklayın ve ardından **Pano** sayfanın üst kısmındaki bağlantı.</span><span class="sxs-lookup"><span data-stu-id="17767-503">Click on the storage account name and then click the **Dashboard** link at the top of the page.</span></span> <span data-ttu-id="17767-504">**Pano** sayfası hesap ve uygulamalarınız içinde kullanılan hizmet uç noktalarına durumu hakkında bilgi ile sağlar.</span><span class="sxs-lookup"><span data-stu-id="17767-504">The **Dashboard** page provides you with information about the status of the account and the service endpoints that can be used within your applications.</span></span>

    <span data-ttu-id="17767-505">![Depolama hesabı Panosu görüntüleme](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "depolama hesabı Panosu görüntüleme")</span><span class="sxs-lookup"><span data-stu-id="17767-505">![Displaying the Storage Account Dashboard](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Displaying the Storage Account Dashboard")</span></span>

    <span data-ttu-id="17767-506">*Depolama hesabı Panosu görüntüleme*</span><span class="sxs-lookup"><span data-stu-id="17767-506">*Displaying the Storage Account Dashboard*</span></span>
5. <span data-ttu-id="17767-507">Tıklatın **erişim anahtarlarını Yönet** gezinti çubuğu düğmesini.</span><span class="sxs-lookup"><span data-stu-id="17767-507">Click the **Manage Access Keys** button in the navigation bar.</span></span>

    <span data-ttu-id="17767-508">![Yönet erişim tuşları düğmesi](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "erişim anahtarlarını Yönet düğmesi")</span><span class="sxs-lookup"><span data-stu-id="17767-508">![Manage Access Keys button](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Manage Access Keys button")</span></span>

    <span data-ttu-id="17767-509">*Erişim tuşları düğmesi yönetme*</span><span class="sxs-lookup"><span data-stu-id="17767-509">*Manage Access Keys button*</span></span>
6. <span data-ttu-id="17767-510">İçinde **erişim anahtarlarını Yönet** iletişim kutusu, kopya **depolama hesabı adı** ve **birincil erişim anahtarını** aşağıdaki alıştırmada ihtiyaç duyacaksınız.</span><span class="sxs-lookup"><span data-stu-id="17767-510">In the **Manage Access Keys** dialog box, copy the **Storage Account Name** and **Primary Access Key** as you will need them in the following exercise.</span></span> <span data-ttu-id="17767-511">Ardından, iletişim kutusunu kapatın.</span><span class="sxs-lookup"><span data-stu-id="17767-511">Then, close the dialog box.</span></span>

    <span data-ttu-id="17767-512">![Yönet erişim tuşu iletişim kutusunda](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "erişim anahtarını yönet iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="17767-512">![Manage Access Key dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Manage Access Key dialog box")</span></span>

    <span data-ttu-id="17767-513">*Erişim anahtarı iletişim kutusu yönetme*</span><span class="sxs-lookup"><span data-stu-id="17767-513">*Manage Access Key dialog box*</span></span>

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a><span data-ttu-id="17767-514">Görev 2 – bir varlık Azure Blob depolama alanına yüklemek</span><span class="sxs-lookup"><span data-stu-id="17767-514">Task 2 – Uploading an Asset to Azure Blob Storage</span></span>

<span data-ttu-id="17767-515">Bu görevde, depolama hesabınıza bağlanmak için Sunucu Gezgini penceresini Visual Studio'dan kullanır.</span><span class="sxs-lookup"><span data-stu-id="17767-515">In this task, you will use the Server Explorer window from Visual Studio to connect to your storage account.</span></span> <span data-ttu-id="17767-516">Ardından, bir blob kapsayıcı oluşturun ve kapsayıcıya günlük test logosu bir dosyayı karşıya.</span><span class="sxs-lookup"><span data-stu-id="17767-516">You will then create a blob container and upload a file with the Geek Quiz logo to the container.</span></span>

1. <span data-ttu-id="17767-517">Visual Studio örneğiyle geçin **GeekQuiz** önceki alıştırmada çözümden.</span><span class="sxs-lookup"><span data-stu-id="17767-517">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="17767-518">Menü çubuğundan seçin **Görünüm** ve ardından **Sunucu Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="17767-518">From the menu bar, select **View** and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="17767-519">İçinde **Sunucu Gezgini**, sağ **Azure** düğümü ve select **Azure Bağlan...** . Aboneliğinizle ilişkili Microsoft hesabı kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="17767-519">In **Server Explorer**, right-click the **Azure** node and select **Connect to Azure...**. Sign in using the Microsoft account associated with your subscription.</span></span>

    ![Microsoft Azure'a Bağlan](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    <span data-ttu-id="17767-521">*Azure'a bağlanma*</span><span class="sxs-lookup"><span data-stu-id="17767-521">*Connect to Azure*</span></span>
4. <span data-ttu-id="17767-522">Genişletin **Azure** düğümünü sağ tıklatın **depolama** seçip **harici depolamayı Ekle...** .</span><span class="sxs-lookup"><span data-stu-id="17767-522">Expand the **Azure** node, right-click **Storage** and select **Attach External Storage...**.</span></span>
5. <span data-ttu-id="17767-523">İçinde **yeni depolama hesabı Ekle** iletişim kutusunda, girin **hesap adı** ve **hesap anahtarı** önceki görev ve tıklatın Elde **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="17767-523">In the **Add New Storage Account** dialog box, enter the **Account name** and **Account key** you obtained in the previous task and click **OK**.</span></span>

    ![Yeni depolama hesabı iletişim kutusu ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    <span data-ttu-id="17767-525">*Yeni depolama hesabı iletişim kutusu ekleme*</span><span class="sxs-lookup"><span data-stu-id="17767-525">*Add New Storage Account dialog box*</span></span>
6. <span data-ttu-id="17767-526">Depolama hesabınızın altında görünmelidir **depolama** düğümü.</span><span class="sxs-lookup"><span data-stu-id="17767-526">Your storage account should appear under the **Storage** node.</span></span> <span data-ttu-id="17767-527">Depolama hesabınızı ve sağ **BLOB'lar** seçip **Blob kapsayıcı oluşturun...** .</span><span class="sxs-lookup"><span data-stu-id="17767-527">Expand your storage account, right-click **Blobs** and select **Create Blob Container...**.</span></span>

    <span data-ttu-id="17767-528">![BLOB kapsayıcı oluşturun](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Blob kapsayıcı oluşturun")</span><span class="sxs-lookup"><span data-stu-id="17767-528">![Create Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Create Blob Container")</span></span>

    <span data-ttu-id="17767-529">*BLOB kapsayıcı oluşturun*</span><span class="sxs-lookup"><span data-stu-id="17767-529">*Create Blob Container*</span></span>
7. <span data-ttu-id="17767-530">İçinde **Blob kapsayıcısı oluşturmak** iletişim kutusuna, blob kapsayıcısı için bir ad girin ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="17767-530">In the **Create Blob Container** dialog box, enter a name for the blob container and click **OK**.</span></span>

    <span data-ttu-id="17767-531">![Create Blob kapsayıcısı iletişim kutusunda](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Blob kapsayıcı Oluştur iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="17767-531">![Create Blob Container dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Create Blob Container dialog box")</span></span>

    <span data-ttu-id="17767-532">*BLOB kapsayıcısı iletişim kutusu oluşturma*</span><span class="sxs-lookup"><span data-stu-id="17767-532">*Create Blob Container dialog box*</span></span>
8. <span data-ttu-id="17767-533">Yeni blob kapsayıcı eklenmelidir **BLOB'lar** düğümü.</span><span class="sxs-lookup"><span data-stu-id="17767-533">The new blob container should be added to the **Blobs** node.</span></span> <span data-ttu-id="17767-534">Kapsayıcı genel hale getirmek üzere kapsayıcıda erişim izinlerini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="17767-534">Change the access permissions in the container to make the container public.</span></span> <span data-ttu-id="17767-535">Bunu yapmak için sağ **görüntüleri** kapsayıcı ve select **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="17767-535">To do this, right-click the **images** container and select **Properties**.</span></span>

    <span data-ttu-id="17767-536">![kapsayıcı özellikleri görüntüleri](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "görüntüleri kapsayıcı özellikleri")</span><span class="sxs-lookup"><span data-stu-id="17767-536">![images container properties](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "images container properties")</span></span>

    <span data-ttu-id="17767-537">*Görüntü kapsayıcısı özellikleri*</span><span class="sxs-lookup"><span data-stu-id="17767-537">*Images container properties*</span></span>
9. <span data-ttu-id="17767-538">İçinde **özellikleri** penceresindeki ayarlayın **herkese okuma erişimi** için **kapsayıcı**.</span><span class="sxs-lookup"><span data-stu-id="17767-538">In the **Properties** window, set the **Public Read Access** to **Container**.</span></span>

    <span data-ttu-id="17767-539">![Ortak okuma erişimi özelliğini değiştirmek](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "herkese okuma erişimi özelliğini değiştirme")</span><span class="sxs-lookup"><span data-stu-id="17767-539">![Changing public read access property](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Changing public read access property")</span></span>

    <span data-ttu-id="17767-540">*Ortak okuma erişimi özelliğini değiştirme*</span><span class="sxs-lookup"><span data-stu-id="17767-540">*Changing public read access property*</span></span>
10. <span data-ttu-id="17767-541">Eminseniz istendiğinde, istediğiniz genel erişim özelliği değiştirmek **Evet**.</span><span class="sxs-lookup"><span data-stu-id="17767-541">When prompted if you are sure you want to change the public access property, click **Yes**.</span></span>

    <span data-ttu-id="17767-542">![Microsoft Visual Studio uyarı](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio uyarı")</span><span class="sxs-lookup"><span data-stu-id="17767-542">![Microsoft Visual Studio warning](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="17767-543">*Microsoft Visual Studio uyarı*</span><span class="sxs-lookup"><span data-stu-id="17767-543">*Microsoft Visual Studio warning*</span></span>
11. <span data-ttu-id="17767-544">İçinde **Sunucu Gezgini**, sağ tıklatın, **görüntüleri** blob kapsayıcısı ve select **görünüm Blob kapsayıcısını**.</span><span class="sxs-lookup"><span data-stu-id="17767-544">In **Server Explorer**, right-click in the **images** blob container and select **View Blob Container**.</span></span>

    <span data-ttu-id="17767-545">![Blob kapsayıcısı görüntülemek](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "görüntülemek Blob kapsayıcısı")</span><span class="sxs-lookup"><span data-stu-id="17767-545">![View Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "View Blob Container")</span></span>

    <span data-ttu-id="17767-546">*Görünüm Blob kapsayıcısı*</span><span class="sxs-lookup"><span data-stu-id="17767-546">*View Blob Container*</span></span>
12. <span data-ttu-id="17767-547">Görüntü kapsayıcısı yeni bir pencerede açmak ve hiçbir girdi bir gösterge gösterilecek.</span><span class="sxs-lookup"><span data-stu-id="17767-547">The images container should open in a new window and a legend with no entries should be shown.</span></span> <span data-ttu-id="17767-548">Tıklatın **karşıya** blob kapsayıcısına bir dosyayı karşıya yüklemeyi simgesi.</span><span class="sxs-lookup"><span data-stu-id="17767-548">Click the **upload** icon to upload a file to the blob container.</span></span>

    <span data-ttu-id="17767-549">![Görüntü kapsayıcısı yok girişlerle](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "hiçbir girdi ile görüntü kapsayıcısı")</span><span class="sxs-lookup"><span data-stu-id="17767-549">![Images container with no entries](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Images container with no entries")</span></span>

    <span data-ttu-id="17767-550">*Görüntü kapsayıcısı yok girişleri*</span><span class="sxs-lookup"><span data-stu-id="17767-550">*Images container with no entries*</span></span>
13. <span data-ttu-id="17767-551">İçinde **karşıya Blob** iletişim kutusunda, gitmek **varlıklar** laboratuvarı klasör.</span><span class="sxs-lookup"><span data-stu-id="17767-551">In the **Upload Blob** dialog box, navigate to the **Assets** folder of the lab.</span></span> <span data-ttu-id="17767-552">Seçin **logosu big.png** dosya ve tıklayın **açık**.</span><span class="sxs-lookup"><span data-stu-id="17767-552">Select the **logo-big.png** file and click **Open**.</span></span>
14. <span data-ttu-id="17767-553">Dosya karşıya kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="17767-553">Wait until the file is uploaded.</span></span> <span data-ttu-id="17767-554">Karşıya yükleme tamamlandığında, dosya görüntüleri kapsayıcısında listelenmelidir.</span><span class="sxs-lookup"><span data-stu-id="17767-554">When the upload completes, the file should be listed in the images container.</span></span> <span data-ttu-id="17767-555">Dosya girişi sağ tıklatın ve seçin **kopya URL**.</span><span class="sxs-lookup"><span data-stu-id="17767-555">Right-click the file entry and select **Copy URL**.</span></span>

    <span data-ttu-id="17767-556">![BLOB URL'sini Kopyala](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "kopyalama blob dosya URL'si")</span><span class="sxs-lookup"><span data-stu-id="17767-556">![Copy blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copy blob file URL")</span></span>

    <span data-ttu-id="17767-557">*BLOB URL'sini Kopyala*</span><span class="sxs-lookup"><span data-stu-id="17767-557">*Copy blob URL*</span></span>
15. <span data-ttu-id="17767-558">Internet Explorer'ı açın ve URL'yi yapıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="17767-558">Open Internet Explorer and paste the URL.</span></span> <span data-ttu-id="17767-559">Aşağıdaki resimde tarayıcıda gösterilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="17767-559">The following image should be shown in the browser.</span></span>

    <span data-ttu-id="17767-560">![Blob Storage'ı Windows logo big.png görüntüden](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "depolama biriminden big.png logo görüntüsü")</span><span class="sxs-lookup"><span data-stu-id="17767-560">![logo-big.png image from Windows Blob Storage](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo-big.png image from storage")</span></span>

    <span data-ttu-id="17767-561">*Azure Blob depolama biriminden logosu big.png görüntüsü*</span><span class="sxs-lookup"><span data-stu-id="17767-561">*logo-big.png image from Azure Blob Storage*</span></span>

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a><span data-ttu-id="17767-562">Görev 3: Azure Blob depolama biriminden statik içeriği kullanmak için çözüm güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="17767-562">Task 3 – Updating the Solution to Consume Static Content from Azure Blob Storage</span></span>

<span data-ttu-id="17767-563">Bu görevde, yapılandıracağınız **GeekQuiz** görüntüyü kullanmak için çözüm karşıya Azure Blob depolama alanına (yerine web uygulamasında bulunan görüntü) bir ASP.NET URL yeniden yazma kuralı ekleyerek **web.config**dosya.</span><span class="sxs-lookup"><span data-stu-id="17767-563">In this task, you will configure the **GeekQuiz** solution to consume the image uploaded to Azure Blob Storage (instead of the image located in the web app) by adding an ASP.NET URL rewrite rule in the **web.config** file.</span></span>

1. <span data-ttu-id="17767-564">Visual Studio'da açın **Web.config** içinde dosya **GeekQuiz** proje ve bulun **&lt;system.webServer&gt;** öğesi.</span><span class="sxs-lookup"><span data-stu-id="17767-564">In Visual Studio, open the **Web.config** file inside the **GeekQuiz** project and locate the **&lt;system.webServer&gt;** element.</span></span>
2. <span data-ttu-id="17767-565">Bir URL ile depolama hesabınızın adını yer tutucu güncelleştirme kuralı, yeniden eklemek için aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="17767-565">Add the following code to add an URL rewrite rule, updating the placeholder with your storage account name.</span></span>

    <span data-ttu-id="17767-566">(Kod parçacığını - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span><span class="sxs-lookup"><span data-stu-id="17767-566">(Code Snippet - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span></span>

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > <span data-ttu-id="17767-567">URL yeniden yazma işlemi, bir gelen Web isteği kesintiye uğratan ve istek farklı bir kaynak için yönlendirme işlemidir.</span><span class="sxs-lookup"><span data-stu-id="17767-567">URL rewriting is the process of intercepting an incoming Web request and redirecting the request to a different resource.</span></span> <span data-ttu-id="17767-568">URL kuralları yeniden yazma isteği yönlendirilecek gerektiği zaman ve nerede yönlendirilir yazmaksızın altyapısı söyler.</span><span class="sxs-lookup"><span data-stu-id="17767-568">The URL rewriting rules tells the rewriting engine when a request needs to be redirected, and where should they be redirected.</span></span> <span data-ttu-id="17767-569">Yazmaksızın kural iki dizesini oluşur: İstenen URL'de aranacak deseni (genellikle, normal ifadeler kullanarak) ve desenle değiştirirseniz için dizesi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="17767-569">A rewriting rule is composed of two strings: the pattern to look for in the requested URL (usually, using regular expressions), and the string to replace the pattern with, if found.</span></span> <span data-ttu-id="17767-570">Daha fazla bilgi için bkz: [URL yeniden yazma işlemi ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span><span class="sxs-lookup"><span data-stu-id="17767-570">For more information, see [URL Rewriting in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span></span>
3. <span data-ttu-id="17767-571">Tuşuna **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="17767-571">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="17767-572">Yeni bir **Git Bash** güncelleştirilmiş uygulamayı Azure App Service'e dağıtmak için konsol.</span><span class="sxs-lookup"><span data-stu-id="17767-572">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
5. <span data-ttu-id="17767-573">Azure'a değişiklikleri göndermek için aşağıdaki komutları yürütün.</span><span class="sxs-lookup"><span data-stu-id="17767-573">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="17767-574">Güncelleştirme *[YOUR uygulama yolu]* yolu ile yer tutucu **GeekQuiz** çözümü.</span><span class="sxs-lookup"><span data-stu-id="17767-574">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="17767-575">Dağıtım parolanızı girmeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="17767-575">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Güncelleştirme Azure'a dağıtma](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    <span data-ttu-id="17767-577">*Güncelleştirme Azure'a dağıtma*</span><span class="sxs-lookup"><span data-stu-id="17767-577">*Deploying update to Azure*</span></span>

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a><span data-ttu-id="17767-578">Görev 4 – doğrulama</span><span class="sxs-lookup"><span data-stu-id="17767-578">Task 4 – Verification</span></span>

<span data-ttu-id="17767-579">Bu görevde kullanacağınız **Internet Explorer** göz atmak için **günlük test** uygulama ve URL görüntüleri çalışır ve kuralı yeniden onay barındırılan görüntüsüne yönlendirilir **Azure Blob Depolama**.</span><span class="sxs-lookup"><span data-stu-id="17767-579">In this task you will use **Internet Explorer** to browse the **Geek Quiz** application and check that the URL rewrite rule for images works and you are redirected to the image hosted on **Azure Blob Storage**.</span></span>

1. <span data-ttu-id="17767-580">Internet Explorer'ı açın ve web uygulamanıza gidin (örneğin `http://<your-web-site>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="17767-580">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="17767-581">Önceden oluşturulmuş kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="17767-581">Log in using the previously created credentials.</span></span>

    <span data-ttu-id="17767-582">![Görüntü ile günlük test web uygulamasını gösteren](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "görüntüsü ile günlük test web uygulamasını gösteren")</span><span class="sxs-lookup"><span data-stu-id="17767-582">![Showing the Geek Quiz web app with the image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Showing the Geek Quiz web app with the image")</span></span>

    <span data-ttu-id="17767-583">*Görüntü ile günlük test web uygulamasını gösteren*</span><span class="sxs-lookup"><span data-stu-id="17767-583">*Showing the Geek Quiz web app with the image*</span></span>
2. <span data-ttu-id="17767-584">Tuşuna **F12** geliştirme araçları başlatmak için seçin **ağ** sekmesinde ve kaydı başlatın.</span><span class="sxs-lookup"><span data-stu-id="17767-584">Press **F12** to launch the development tools, select the **Network** tab and start recording.</span></span>

    <span data-ttu-id="17767-585">![Ağ kayıt başlangıç](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "ağ kayıt başlatılıyor")</span><span class="sxs-lookup"><span data-stu-id="17767-585">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Starting network recording")</span></span>

    <span data-ttu-id="17767-586">*Başlangıç ağ kaydı*</span><span class="sxs-lookup"><span data-stu-id="17767-586">*Starting network recording*</span></span>
3. <span data-ttu-id="17767-587">Tuşuna **CTRL + F5** web sayfasını yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="17767-587">Press **CTRL + F5** to refresh the web page.</span></span>
4. <span data-ttu-id="17767-588">Sayfa yükleme tamamlandıktan sonra bir HTTP isteği için görmelisiniz **/img/logo-big.png** URL bir HTTP ile **301** sonucu (Yönlendirme) ve başka bir istek için `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL bir HTTP ile**200** sonucu.</span><span class="sxs-lookup"><span data-stu-id="17767-588">Once the page has finished loading, you should see an HTTP request for the **/img/logo-big.png** URL with an HTTP **301** result (redirect) and another request for `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL with a HTTP **200** result.</span></span>

    <span data-ttu-id="17767-589">![URL yeniden yönlendirme doğrulama](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Geliştirme Araçları'nda yeniden yönlendirme gösterme")</span><span class="sxs-lookup"><span data-stu-id="17767-589">![Verifying the URL redirect](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Showing the redirect in Dev Tools")</span></span>

    <span data-ttu-id="17767-590">*URL yeniden yönlendirme doğrulanıyor*</span><span class="sxs-lookup"><span data-stu-id="17767-590">*Verifying the URL redirect*</span></span>

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a><span data-ttu-id="17767-591">Alıştırma 5: Web uygulamaları için otomatik ölçeklendirme kullanma</span><span class="sxs-lookup"><span data-stu-id="17767-591">Exercise 5: Using Autoscale for Web Apps</span></span>

> [!NOTE]
> <span data-ttu-id="17767-592">Destek Web yükü gerektirdiğinden bu alıştırmada isteğe bağlı, &amp; yalnızca için kullanılabilir olan performans testi **Visual Studio 2013 Ultimate Edition**.</span><span class="sxs-lookup"><span data-stu-id="17767-592">This exercise is optional, since it requires support for Web Load &amp; Performance Testing which is only available for **Visual Studio 2013 Ultimate Edition**.</span></span> <span data-ttu-id="17767-593">Belirli Visual Studio 2013 özellikleri hakkında daha fazla bilgi için sürümlerini karşılaştırma [burada](https://www.microsoft.com/visualstudio/eng/products/compare).</span><span class="sxs-lookup"><span data-stu-id="17767-593">For more information on specific Visual Studio 2013 features, compare versions [here](https://www.microsoft.com/visualstudio/eng/products/compare).</span></span>


<span data-ttu-id="17767-594">**Azure App Service Web Apps** çalışan web uygulamaları için otomatik ölçeklendirme özelliği sağlar **Standart mod**.</span><span class="sxs-lookup"><span data-stu-id="17767-594">**Azure App Service Web Apps** provides the Autoscale feature for web apps running in **Standard Mode**.</span></span> <span data-ttu-id="17767-595">Otomatik ölçeklendirme, örnek sayısı, web uygulamanızın yük bağlı olarak otomatik şekilde ölçeklendirebilir Azure olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="17767-595">Autoscale lets Azure automatically scale the instance count of your web app depending on the load.</span></span> <span data-ttu-id="17767-596">Otomatik ölçeklendirme etkinleştirildiğinde, Azure web uygulamanızın CPU her beş dakikada denetler ve örnekleri zamandaki o noktada gerektiği gibi ekler.</span><span class="sxs-lookup"><span data-stu-id="17767-596">When Autoscale is enabled, Azure checks the CPU of your web app once every five minutes and adds instances as needed at that point in time.</span></span> <span data-ttu-id="17767-597">CPU kullanımı düşükse, Azure web uygulamanızın performansını değil düşürülmüş emin olmak için her iki saatte örneklerini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="17767-597">If the CPU usage is low, Azure will remove instances once every two hours to ensure that the performance of your web app is not degraded.</span></span>

<span data-ttu-id="17767-598">Bu alıştırmada yapılandırmak için gereken adımlarda size gidecek **otomatik ölçeklendirme** için özellik **günlük test** web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="17767-598">In this exercise you will go through the steps required to configure the **Autoscale** feature for the **Geek Quiz** web app.</span></span> <span data-ttu-id="17767-599">Bu özellik, bir örnek yükseltilmesini sağlamak için uygulama yeterli CPU yükünü oluşturmak için Visual Studio yükleme testi çalıştırarak doğrular.</span><span class="sxs-lookup"><span data-stu-id="17767-599">You will verify this feature by running a Visual Studio load test to generate enough CPU load on the application to trigger an instance upgrade.</span></span>

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a><span data-ttu-id="17767-600">Görev 1 – CPU ölçüm tabanlı yapılandırma otomatik ölçeklendirme</span><span class="sxs-lookup"><span data-stu-id="17767-600">Task 1 – Configuring Autoscale Based on the CPU Metric</span></span>

<span data-ttu-id="17767-601">Bu görevde alıştırma 2'de oluşturulan web uygulaması için otomatik ölçeklendirme özelliğini etkinleştirmek için Azure Yönetim Portalı'nı kullanır.</span><span class="sxs-lookup"><span data-stu-id="17767-601">In this task you will use the Azure management portal to enable the Autoscale feature for the web app you created in Exercise 2.</span></span>

1. <span data-ttu-id="17767-602">İçinde [Azure Yönetim Portalı](https://manage.windowsazure.com/)seçin **Web siteleri** ve alıştırma 2'de oluşturulan web uygulaması'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="17767-602">In the [Azure management portal](https://manage.windowsazure.com/), select **Websites** and click the web app you created in Exercise 2.</span></span>
2. <span data-ttu-id="17767-603">Gidin **ölçek** sayfası.</span><span class="sxs-lookup"><span data-stu-id="17767-603">Navigate to the **Scale** page.</span></span> <span data-ttu-id="17767-604">Altında **kapasite** bölümünde, select **CPU** için **ölçek ölçüm tarafından** yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="17767-604">Under the **capacity** section, select **CPU** for the **Scale by Metric** configuration.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-605">CPU ile ölçekleme sırasında Azure CPU kullanımı değişirse uygulamanın kullandığı örneklerinin sayısını dinamik olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="17767-605">When scaling by CPU, Azure dynamically adjusts the number of instances that the app uses if the CPU usage changes.</span></span>

    <span data-ttu-id="17767-606">![Ölçek CPU tarafından seçme](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "otomatik ölçekleme için CPU ölçüm seçme")</span><span class="sxs-lookup"><span data-stu-id="17767-606">![Selecting to scale by CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selecting the CPU metric for auto scaling")</span></span>

    <span data-ttu-id="17767-607">*Ölçek CPU tarafından seçme*</span><span class="sxs-lookup"><span data-stu-id="17767-607">*Selecting to scale by CPU*</span></span>
3. <span data-ttu-id="17767-608">Değişiklik **hedef CPU** yapılandırmasına **20**-**40** yüzde.</span><span class="sxs-lookup"><span data-stu-id="17767-608">Change the **Target CPU** configuration to **20**-**40** percent.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-609">Bu aralık, web uygulamanız için ortalama CPU kullanımını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="17767-609">This range represents the average CPU usage for your web app.</span></span> <span data-ttu-id="17767-610">Azure ekleyin veya web uygulamanızın bu aralıkta tutmak için örnekler kaldırın.</span><span class="sxs-lookup"><span data-stu-id="17767-610">Azure will add or remove instances to keep your web app in this range.</span></span> <span data-ttu-id="17767-611">Ölçeklendirme için kullanılan örneği minimum ve maksimum sayısı belirtilen **örnek sayısı** yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="17767-611">The minimum and maximum number of instances used for scaling is specified in the **Instance Count** configuration.</span></span> <span data-ttu-id="17767-612">Azure üzerinde veya bu sınırı aşan hiçbir zaman geçer.</span><span class="sxs-lookup"><span data-stu-id="17767-612">Azure will never go above or beyond that limit.</span></span>
    > 
    > <span data-ttu-id="17767-613">Varsayılan **hedef CPU** değerleri yalnızca bu laboratuvarı amaçları doğrultusunda değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="17767-613">The default **Target CPU** values are modified just for the purposes of this lab.</span></span> <span data-ttu-id="17767-614">Küçük değerlerle CPU aralığı yapılandırarak, tetikleyici otomatik ölçeklendirme için büyük olasılıkla Orta yük uygulamanın yerleştirildiğinde artmaktadır.</span><span class="sxs-lookup"><span data-stu-id="17767-614">By configuring the CPU range with small values, you are increasing the chances to trigger Autoscale when a moderate load is placed on the application.</span></span>

    <span data-ttu-id="17767-615">![Hedef CPU 20 ve yüzde 40 arasında olacak şekilde değiştirmeyi](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "20 ve yüzde 40 arasında olacak şekilde hedef CPU değiştirme")</span><span class="sxs-lookup"><span data-stu-id="17767-615">![Changing the target CPU to be between 20 and 40 percent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Changing the target CPU to be between 20 and 40 percent")</span></span>

    <span data-ttu-id="17767-616">*Hedef CPU 20 ve yüzde 40 arasında olacak şekilde değiştirme*</span><span class="sxs-lookup"><span data-stu-id="17767-616">*Changing the Target CPU to be between 20 and 40 percent*</span></span>
4. <span data-ttu-id="17767-617">Tıklatın **kaydetmek** değişiklikleri kaydetmek için komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="17767-617">Click **Save** in the command bar to save the changes.</span></span>

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a><span data-ttu-id="17767-618">Görev 2 – yük ile Visual Studio test etme</span><span class="sxs-lookup"><span data-stu-id="17767-618">Task 2 – Load Testing with Visual Studio</span></span>

<span data-ttu-id="17767-619">Şimdi **otomatik ölçeklendirme** bırakıldı oluşturacağınız yapılandırılmış, bir **Web performansı ve yük testi projesi** bazı CPU yükünü web uygulamanızı oluşturmak için Visual Studio'da.</span><span class="sxs-lookup"><span data-stu-id="17767-619">Now that **Autoscale** has been configured, you will create a **Web Performance and Load Test Project** in Visual Studio to generate some CPU load on your web app.</span></span>

1. <span data-ttu-id="17767-620">Açık **Visual Studio Ultimate 2013** seçip **dosya | Yeni | Proje...**  yeni bir çözüm başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="17767-620">Open **Visual Studio Ultimate 2013** and select **File | New | Project...** to start a new solution.</span></span>

    <span data-ttu-id="17767-621">![Yeni proje oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "yeni proje oluşturma")</span><span class="sxs-lookup"><span data-stu-id="17767-621">![Creating a new project](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Creating a new project")</span></span>

    <span data-ttu-id="17767-622">*Yeni proje oluşturma*</span><span class="sxs-lookup"><span data-stu-id="17767-622">*Creating a new project*</span></span>
2. <span data-ttu-id="17767-623">İçinde **yeni proje** iletişim kutusunda **Web performansı ve yük testi projesi** altında **Visual C# | Test** sekmesi. Emin olun **.NET Framework 4.5** olduğunu belirlenirse, proje adı *WebAndLoadTestProject*, seçin bir **konumu** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="17767-623">In the **New Project** dialog box, select **Web Performance and Load Test Project** under the **Visual C# | Test** tab. Make sure **.NET Framework 4.5** is selected, name the project *WebAndLoadTestProject*, choose a **Location** and click **OK**.</span></span>

    <span data-ttu-id="17767-624">![Yeni Web ve yük testi projesi oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "yeni Web ve yük testi projesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="17767-624">![Creating a new Web and Load Test project](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Creating a new Web and Load Test project")</span></span>

    <span data-ttu-id="17767-625">*Yeni Web ve yük testi projesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="17767-625">*Creating a new Web and Load Test project*</span></span>
3. <span data-ttu-id="17767-626">İçinde **WebTest1.webtest** sağ **WebTest1'i** düğümü ve tıklatın **ekleme isteği**.</span><span class="sxs-lookup"><span data-stu-id="17767-626">In the **WebTest1.webtest** Right-click the **WebTest1** node and click **Add Request**.</span></span>

    <span data-ttu-id="17767-627">![Bir istek için WebTest1'i ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "WebTest1'i için bir istek ekleme")</span><span class="sxs-lookup"><span data-stu-id="17767-627">![Adding a request to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Adding a request to WebTest1")</span></span>

    <span data-ttu-id="17767-628">*Bir istek için WebTest1'i ekleme*</span><span class="sxs-lookup"><span data-stu-id="17767-628">*Adding a request to WebTest1*</span></span>
4. <span data-ttu-id="17767-629">İçinde **özellikleri** penceresi yeni istek düğümünün güncelleştirme **Url** , web uygulamanızın URL'sine işaret edecek şekilde özelliği (örneğin *[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).</span><span class="sxs-lookup"><span data-stu-id="17767-629">In the **Properties** window of the new request node, update the **Url** property to point to the URL of your web app (e.g. *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span></span>

    <span data-ttu-id="17767-630">![Url özelliğini değiştirmek](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Url özelliğini değiştirme")</span><span class="sxs-lookup"><span data-stu-id="17767-630">![Changing the Url property](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Changing the Url property")</span></span>

    <span data-ttu-id="17767-631">*Url özelliğini değiştirme*</span><span class="sxs-lookup"><span data-stu-id="17767-631">*Changing the Url property*</span></span>
5. <span data-ttu-id="17767-632">İçinde **WebTest1.webtest** penceresinde sağ tıklayın **WebTest1'i** tıklatıp **döngü Ekle...** .</span><span class="sxs-lookup"><span data-stu-id="17767-632">In the **WebTest1.webtest** window, right-click **WebTest1** and click **Add Loop...**.</span></span>

    <span data-ttu-id="17767-633">![Döngü WebTest1'i için ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "WebTest1'i için bir döngü ekleme")</span><span class="sxs-lookup"><span data-stu-id="17767-633">![Adding a loop to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Adding a loop to WebTest1")</span></span>

    <span data-ttu-id="17767-634">*Döngü WebTest1'i için ekleme*</span><span class="sxs-lookup"><span data-stu-id="17767-634">*Adding a loop to WebTest1*</span></span>
6. <span data-ttu-id="17767-635">İçinde **koşullu Kuralı Ekle ve döngü öğeler** iletişim kutusunda **For döngüsü** kural ve aşağıdaki özellikleri değiştirin.</span><span class="sxs-lookup"><span data-stu-id="17767-635">In the **Add Conditional Rule and Items to Loop** dialog box, select the **For Loop** rule and modify the following properties.</span></span>

   1. <span data-ttu-id="17767-636">**Değeri sonlandırma:** 1000</span><span class="sxs-lookup"><span data-stu-id="17767-636">**Terminating value:** 1000</span></span>
   2. <span data-ttu-id="17767-637">**Bağlam parametresi adı:** yineleyici</span><span class="sxs-lookup"><span data-stu-id="17767-637">**Context Parameter Name:** Iterator</span></span>
   3. <span data-ttu-id="17767-638">**Artış değeri:** 1</span><span class="sxs-lookup"><span data-stu-id="17767-638">**Increment Value:** 1</span></span>

      <span data-ttu-id="17767-639">![For döngüsü kuralı seçip özelliklerini güncelleştirme](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "For döngüsü kuralı seçip özelliklerini güncelleştirme")</span><span class="sxs-lookup"><span data-stu-id="17767-639">![Selecting the For Loop rule and updating the properties](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Selecting the For Loop rule and updating the properties")</span></span>

      <span data-ttu-id="17767-640">*For döngüsü kuralı seçip özelliklerini güncelleştirme*</span><span class="sxs-lookup"><span data-stu-id="17767-640">*Selecting the For Loop rule and updating the properties*</span></span>
7. <span data-ttu-id="17767-641">Altında **Döngüdeki öğeler** bölümünde, daha önce oluşturduğunuz döngü için ilk ve son öğeyi olmasını isteği seçin.</span><span class="sxs-lookup"><span data-stu-id="17767-641">Under the **Items in loop** section, select the request you created previously to be the first and last item for the loop.</span></span> <span data-ttu-id="17767-642">Devam etmek için **Tamam** 'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="17767-642">Click **OK** to continue.</span></span>

    <span data-ttu-id="17767-643">![Döngünün ilk ve son öğeler seçme](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "döngünün ilk ve son öğeler seçme")</span><span class="sxs-lookup"><span data-stu-id="17767-643">![Selecting the first and last items for the loop](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Selecting the first and last items for the loop")</span></span>

    <span data-ttu-id="17767-644">*Döngünün ilk ve son öğeler seçme*</span><span class="sxs-lookup"><span data-stu-id="17767-644">*Selecting the first and last items for the loop*</span></span>
8. <span data-ttu-id="17767-645">İçinde **Çözüm Gezgini**, sağ tıklatın **WebAndLoadTestProject** projesi, genişletin **Ekle** menü ve select **yük testi...** .</span><span class="sxs-lookup"><span data-stu-id="17767-645">In **Solution Explorer**, right-click the **WebAndLoadTestProject** project, expand the **Add** menu and select **Load Test...**.</span></span>

    <span data-ttu-id="17767-646">![Bir yük testi WebAndLoadTestProject projesine ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "bir yük testi WebAndLoadTestProject projesine ekleme")</span><span class="sxs-lookup"><span data-stu-id="17767-646">![Adding a Load Test to the WebAndLoadTestProject project](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Adding a Load Test to the WebAndLoadTestProject project")</span></span>

    <span data-ttu-id="17767-647">*Bir yük testi WebAndLoadTestProject projesine ekleme*</span><span class="sxs-lookup"><span data-stu-id="17767-647">*Adding a Load Test to the WebAndLoadTestProject project*</span></span>
9. <span data-ttu-id="17767-648">İçinde **Yeni Yük Testi Sihirbazı** iletişim kutusu, tıklatın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="17767-648">In the **New Load Test Wizard** dialog box, click **Next**.</span></span>

    <span data-ttu-id="17767-649">![Yeni Yük Testi Sihirbazı](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Yeni Yük Testi Sihirbazı")</span><span class="sxs-lookup"><span data-stu-id="17767-649">![New Load Test Wizard](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "New Load Test Wizard")</span></span>

    <span data-ttu-id="17767-650">*Yeni Yük Testi Sihirbazı*</span><span class="sxs-lookup"><span data-stu-id="17767-650">*New Load Test Wizard*</span></span>
10. <span data-ttu-id="17767-651">İçinde **senaryo** sayfasında, **düşünme sürelerini kullanmayın** tıklatıp **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="17767-651">In the **Scenario** page, select **Do not use think times** and click **Next**.</span></span>

    <span data-ttu-id="17767-652">![Düşünme sürelerini kullanmamayı seçme](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "düşünme sürelerini kullanmamayı seçme")</span><span class="sxs-lookup"><span data-stu-id="17767-652">![Selecting not to use think times](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selecting not to use think times")</span></span>

    <span data-ttu-id="17767-653">*Düşünme sürelerini kullanmamayı seçme*</span><span class="sxs-lookup"><span data-stu-id="17767-653">*Selecting not to use think times*</span></span>
11. <span data-ttu-id="17767-654">İçinde **yük düzeni** sayfasında, olduğundan emin olun **sabit yük** seçeneği işaretlidir.</span><span class="sxs-lookup"><span data-stu-id="17767-654">In the **Load Pattern** page, make sure that the **Constant Load** option is selected.</span></span> <span data-ttu-id="17767-655">Değişiklik **kullanıcı sayısı** ayarını **250** kullanıcılar ve tıklatın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="17767-655">Change the **User Count** setting to **250** users and click **Next**.</span></span>

    <span data-ttu-id="17767-656">![Kullanıcı sayısı için 250 değiştirme](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "kullanıcı sayısı için 250 değiştirme")</span><span class="sxs-lookup"><span data-stu-id="17767-656">![Changing the user count to 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Changing the user count to 250")</span></span>

    <span data-ttu-id="17767-657">*Kullanıcı sayısı için 250 değiştirme*</span><span class="sxs-lookup"><span data-stu-id="17767-657">*Changing the user count to 250*</span></span>
12. <span data-ttu-id="17767-658">İçinde **Test karışımı modeli** sayfasında, **sıralı test sırasına göre** tıklatıp **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="17767-658">In the **Test Mix Model** page, select **Based on sequential test order** and click **Next**.</span></span>

    <span data-ttu-id="17767-659">![Test karışımı modeli seçme](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "test karışımı modeli seçme")</span><span class="sxs-lookup"><span data-stu-id="17767-659">![Selecting the test mix model](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Selecting the test mix model")</span></span>

    <span data-ttu-id="17767-660">*Test karışımı modeli seçme*</span><span class="sxs-lookup"><span data-stu-id="17767-660">*Selecting the test mix model*</span></span>
13. <span data-ttu-id="17767-661">İçinde **Test karışımı modeli** sayfasında, **Ekle...**  test Karışıma eklemek için.</span><span class="sxs-lookup"><span data-stu-id="17767-661">In the **Test Mix Model** page, click **Add...** to add a test to the mix.</span></span>

    <span data-ttu-id="17767-662">![Bir testi için test karışımını ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "bir testi için test karışımını ekleme")</span><span class="sxs-lookup"><span data-stu-id="17767-662">![Adding a test to the test mix](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Adding a test to the test mix")</span></span>

    <span data-ttu-id="17767-663">*Bir testi için test karışımını ekleme*</span><span class="sxs-lookup"><span data-stu-id="17767-663">*Adding a test to the test mix*</span></span>
14. <span data-ttu-id="17767-664">İçinde **testler Ekle** iletişim kutusu, çift **WebTest1'i** testine ekleme **seçili testleri** listesi.</span><span class="sxs-lookup"><span data-stu-id="17767-664">In the **Add Tests** dialog box, double-click **WebTest1** to add the test to the **Selected tests** list.</span></span> <span data-ttu-id="17767-665">Devam etmek için **Tamam** 'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="17767-665">Click **OK** to continue.</span></span>

    <span data-ttu-id="17767-666">![WebTest1'i test ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "WebTest1'i test ekleme")</span><span class="sxs-lookup"><span data-stu-id="17767-666">![Adding the WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Adding the WebTest1 test")</span></span>

    <span data-ttu-id="17767-667">*WebTest1'i test ekleme*</span><span class="sxs-lookup"><span data-stu-id="17767-667">*Adding the WebTest1 test*</span></span>
15. <span data-ttu-id="17767-668">Geri **Test karışımı** sayfasında, **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="17767-668">Back in the **Test Mix** page, click **Next**.</span></span>

    <span data-ttu-id="17767-669">![Test Karışımı sayfası Tamamlanıyor](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Test Karışımı sayfası Tamamlanıyor")</span><span class="sxs-lookup"><span data-stu-id="17767-669">![Completing the Test Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Completing the Test Mix page")</span></span>

    <span data-ttu-id="17767-670">*Test Karışımı sayfası Tamamlanıyor*</span><span class="sxs-lookup"><span data-stu-id="17767-670">*Completing the Test Mix page*</span></span>
16. <span data-ttu-id="17767-671">İçinde **ağ karışımı** sayfasında, **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="17767-671">In the **Network Mix** page, click **Next**.</span></span>

    <span data-ttu-id="17767-672">![Ağ karışımı sayfasındaki sonraki tıkladığınızda](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "tıkladığınızda sonraki ağ karışımı sayfasındaki")</span><span class="sxs-lookup"><span data-stu-id="17767-672">![Clicking next in the Network Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Clicking next in the Network Mix page")</span></span>

    <span data-ttu-id="17767-673">*Ağ karışımı sayfasında İleri'yi tıklatmadan*</span><span class="sxs-lookup"><span data-stu-id="17767-673">*Clicking next in the Network Mix page*</span></span>
17. <span data-ttu-id="17767-674">İçinde **tarayıcı karışımı** sayfasında, **Internet Explorer 10.0** tıklatın ve tarayıcı türü olarak **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="17767-674">In the **Browser Mix** page, select **Internet Explorer 10.0** as the browser type and click **Next**.</span></span>

    <span data-ttu-id="17767-675">![Tarayıcı türü seçme](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "tarayıcı türü seçme")</span><span class="sxs-lookup"><span data-stu-id="17767-675">![Selecting the browser type](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Selecting the browser type")</span></span>

    <span data-ttu-id="17767-676">*Tarayıcı türü seçme*</span><span class="sxs-lookup"><span data-stu-id="17767-676">*Selecting the browser type*</span></span>
18. <span data-ttu-id="17767-677">İçinde **sayaç kümelerini** sayfasında, **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="17767-677">In the **Counter Sets** page, click **Next**.</span></span>

    <span data-ttu-id="17767-678">![Sayaç kümeleri sayfasında İleri'yi tıklatmadan](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "sayaç kümelerini sayfasında İleri tıklatarak")</span><span class="sxs-lookup"><span data-stu-id="17767-678">![Clicking Next in the Counter Sets page](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Clicking Next in the Counter Sets page")</span></span>

    <span data-ttu-id="17767-679">*Sayaç kümeleri sayfasında İleri'yi tıklatmadan*</span><span class="sxs-lookup"><span data-stu-id="17767-679">*Clicking Next in the Counter Sets page*</span></span>
19. <span data-ttu-id="17767-680">İçinde **çalıştırma ayarları** sayfasında **yük test süresi** için **5 dakika** tıklatıp **son**.</span><span class="sxs-lookup"><span data-stu-id="17767-680">In the **Run Settings** page, set the **Load test duration** to **5 minutes** and click **Finish**.</span></span>

    <span data-ttu-id="17767-681">![Yük testi süre 5 dakika olarak ayarlama](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "yük testi süre 5 dakika olarak ayarlama")</span><span class="sxs-lookup"><span data-stu-id="17767-681">![Setting the load test duration to 5 minutes](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Setting the load test duration to 5 minutes")</span></span>

    <span data-ttu-id="17767-682">*Yük testi süre 5 dakika olarak ayarlama*</span><span class="sxs-lookup"><span data-stu-id="17767-682">*Setting the load test duration to 5 minutes*</span></span>
20. <span data-ttu-id="17767-683">İçinde **Çözüm Gezgini**, çift **Local.settings** test ayarları keşfetmek için dosya.</span><span class="sxs-lookup"><span data-stu-id="17767-683">In **Solution Explorer**, double-click the **Local.settings** file to explore the test settings.</span></span> <span data-ttu-id="17767-684">Varsayılan olarak, Visual Studio testleri çalıştırmak için yerel bilgisayarınıza kullanır.</span><span class="sxs-lookup"><span data-stu-id="17767-684">By default, Visual Studio uses your local computer to run the tests.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-685">Alternatif olarak, bulut kullanılarak yük testleri çalıştırmak için test projenizi yapılandırabilirsiniz **Visual Studio Online (VSO)**.</span><span class="sxs-lookup"><span data-stu-id="17767-685">Alternatively, you can configure your test project to run the load tests in the cloud using **Visual Studio Online (VSO)**.</span></span> <span data-ttu-id="17767-686">VSO daha gerçekçi yük benzetim hizmeti sınama, CPU kapasitesini, kullanılabilir bellek ve ağ bant genişliği gibi yerel ortamınıza kısıtlamaları önleme bir bulut tabanlı yük sağlar.</span><span class="sxs-lookup"><span data-stu-id="17767-686">VSO provides a cloud-based load testing service that simulates a more realistic load, avoiding local environment constraints like CPU capacity, available memory and network bandwidth.</span></span> <span data-ttu-id="17767-687">Yük testleri çalıştırmak için VSO kullanma hakkında daha fazla bilgi için bkz: [bu makalede](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span><span class="sxs-lookup"><span data-stu-id="17767-687">For more information about using VSO to run load tests, see [this article](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span></span>

    ![Test ayarları](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a><span data-ttu-id="17767-689">Görev 3 – otomatik ölçeklendirme doğrulama</span><span class="sxs-lookup"><span data-stu-id="17767-689">Task 3 – Autoscale Verification</span></span>

<span data-ttu-id="17767-690">Şimdi, önceki görevde oluşturduğunuz yük testi yürütün ve yük altında web uygulamanızı nasıl davranacağını bakın.</span><span class="sxs-lookup"><span data-stu-id="17767-690">You will now execute the load test you created in the previous task and see how your web app behaves under load.</span></span>

1. <span data-ttu-id="17767-691">İçinde **Çözüm Gezgini**, çift **LoadTest1.loadtest** yük testi açın.</span><span class="sxs-lookup"><span data-stu-id="17767-691">In **Solution Explorer**, double-click **LoadTest1.loadtest** to open the load test.</span></span>

    <span data-ttu-id="17767-692">![LoadTest1.loadtest açma](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "LoadTest1.loadtest açma")</span><span class="sxs-lookup"><span data-stu-id="17767-692">![Opening LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Opening LoadTest1.loadtest")</span></span>

    <span data-ttu-id="17767-693">*Opening LoadTest1.loadtest*</span><span class="sxs-lookup"><span data-stu-id="17767-693">*Opening LoadTest1.loadtest*</span></span>
2. <span data-ttu-id="17767-694">İçinde **LoadTest1.loadtest** penceresinde, yükleme testini çalıştırmak için araç kutusu ilk düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="17767-694">In the **LoadTest1.loadtest** window, click the first button in the toolbox to run the load test.</span></span>

    <span data-ttu-id="17767-695">![Yük testi çalıştırma](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "yük testi çalıştırma")</span><span class="sxs-lookup"><span data-stu-id="17767-695">![Running the load test](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Running the load test")</span></span>

    <span data-ttu-id="17767-696">*Yük testi çalıştırma*</span><span class="sxs-lookup"><span data-stu-id="17767-696">*Running the load test*</span></span>
3. <span data-ttu-id="17767-697">Yük testi tamamlanana dek bekleyin.</span><span class="sxs-lookup"><span data-stu-id="17767-697">Wait until the load test completes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-698">Yük testi istekleri göndermek için web uygulaması aynı anda birden çok kullanıcı benzetimini yapar.</span><span class="sxs-lookup"><span data-stu-id="17767-698">The load test simulates multiple users that send requests to the web app simultaneously.</span></span> <span data-ttu-id="17767-699">Test çalıştırılırken, hatalar, uyarılar veya yükleme testi için ilgili diğer bilgileri algılamak için kullanılabilir sayaçlar izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="17767-699">When the test is running, you can monitor the available counters to detect any errors, warnings or other information related to your load test run.</span></span>

    <span data-ttu-id="17767-700">![Yük testi çalıştırma](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "yük testi tamamlanıncaya kadar bekleniyor")</span><span class="sxs-lookup"><span data-stu-id="17767-700">![Load test running](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Waiting until the load test completes")</span></span>

    <span data-ttu-id="17767-701">*Yük testi çalıştırma*</span><span class="sxs-lookup"><span data-stu-id="17767-701">*Load test running*</span></span>
4. <span data-ttu-id="17767-702">Test tamamlandıktan sonra Yönetim Portalı'na geri dönün ve gidin **ölçek** sayfası, web uygulamanızın.</span><span class="sxs-lookup"><span data-stu-id="17767-702">Once the test completes, go back to the management portal and navigate to the **Scale** page of your web app.</span></span> <span data-ttu-id="17767-703">Altında **kapasite** bölümünde, yeni bir örneğini otomatik olarak dağıtılan grafikte görmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="17767-703">Under the **capacity** section, you should see in the graph that a new instance was automatically deployed.</span></span>

    ![Otomatik olarak dağıtılan yeni örneği](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    <span data-ttu-id="17767-705">*Otomatik olarak dağıtılan yeni örneği*</span><span class="sxs-lookup"><span data-stu-id="17767-705">*New instance automatically deployed*</span></span>

    > [!NOTE]
    > <span data-ttu-id="17767-706">Değişikliklerin grafikte görünmesi birkaç dakika sürebilir (basın **CTRL + F5** düzenli aralıklarla sayfayı yenilemek için).</span><span class="sxs-lookup"><span data-stu-id="17767-706">It may take several minutes for the changes to appear in the graph (press **CTRL + F5** periodically to refresh the page).</span></span> <span data-ttu-id="17767-707">Herhangi bir değişiklik yoksa aşağıdakileri deneyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="17767-707">If you do not see any changes, you can try the following:</span></span>
    > 
    > - <span data-ttu-id="17767-708">Yük testi süresini artırın (örneğin için **10 dakika**)</span><span class="sxs-lookup"><span data-stu-id="17767-708">Increase the duration of the load test (e.g. to **10 minutes**)</span></span>
    > - <span data-ttu-id="17767-709">Maksimum ve minimum değerler, azaltmak **hedef CPU** web uygulamanız otomatik ölçeklendirme yapılandırmasını aralığı</span><span class="sxs-lookup"><span data-stu-id="17767-709">Reduce the maximum and minimum values of the **Target CPU** range in the Autoscale configuration of your web app</span></span>
    > - <span data-ttu-id="17767-710">Yük testi ile bulutta çalıştırmak **Visual Studio Online**.</span><span class="sxs-lookup"><span data-stu-id="17767-710">Run the load test in the cloud with **Visual Studio Online**.</span></span> <span data-ttu-id="17767-711">Daha fazla bilgi [burada](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span><span class="sxs-lookup"><span data-stu-id="17767-711">More information [here](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="17767-712">Özet</span><span class="sxs-lookup"><span data-stu-id="17767-712">Summary</span></span>

<span data-ttu-id="17767-713">Bu uygulamalı laboratuar ortamında ayarlama ve üretim web uygulamaları Azure uygulamanızı dağıtmak öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="17767-713">In this hands-on lab, you learned how to set up and deploy your application to production web apps in Azure.</span></span> <span data-ttu-id="17767-714">Algılama ve kullanarak veritabanlarınız güncelleştirme başlattığınız **Entity Framework Code First Migrations**, site kullanarak, yeni sürümlerini dağıtarak devam **Git** ve düzeyine gerçekleştirme siteniz en son kararlı sürümü.</span><span class="sxs-lookup"><span data-stu-id="17767-714">You started by detecting and updating your databases using **Entity Framework Code First Migrations**, then continued by deploying new versions of your site using **Git** and performing rollbacks to the latest stable version of your site.</span></span> <span data-ttu-id="17767-715">Ayrıca, bir Blob kapsayıcısına, statik içerik taşımak için depolama birimi kullanarak uygulamanızı ölçeklendirmek nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="17767-715">Additionally, you learned how to scale your app using Storage to move your static content to a Blob container.</span></span>
