---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: "Laboratuvar üzerinde aktarır: sürdürülebilir Azure Web siteleri: değişiklik ve ölçek yönetme | Microsoft Docs"
author: rick-anderson
description: "Microsoft Azure ve üretim Web siteleri dağıtacak daha kolay hale getirir. Ancak, uygulamanızın Canlı olduğunda bitirdiniz değil, yeni başlıyor olun! ..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 3d24c633368abc14efcd9fcf200a4d05c5b182c9
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a><span data-ttu-id="708eb-105">Laboratuvar üzerinde aktarır: sürdürülebilir Azure Web siteleri: değişiklik ve ölçek yönetme</span><span class="sxs-lookup"><span data-stu-id="708eb-105">Hands on Lab: Maintainable Azure Websites: Managing Change and Scale</span></span>
====================
<span data-ttu-id="708eb-106">tarafından [Web Camps ekibi](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="708eb-106">by [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="708eb-107">Kit eğitim Web Camps indirin</span><span class="sxs-lookup"><span data-stu-id="708eb-107">Download Web Camps Training Kit</span></span>](http://aka.ms/webcamps-training-kit)

> <span data-ttu-id="708eb-108">Microsoft Azure ve üretim Web siteleri dağıtacak daha kolay hale getirir.</span><span class="sxs-lookup"><span data-stu-id="708eb-108">Microsoft Azure makes it easy to build and deploy websites to production.</span></span> <span data-ttu-id="708eb-109">Ancak, uygulamanızın Canlı olduğunda bitirdiniz değil, yeni başlıyor olun!</span><span class="sxs-lookup"><span data-stu-id="708eb-109">But you're not done when your application is live, you're just getting started!</span></span> <span data-ttu-id="708eb-110">Değişen gereksinimleri, veritabanı güncelleştirmeleri, ölçeklendirme ve daha fazla işleme gerekir.</span><span class="sxs-lookup"><span data-stu-id="708eb-110">You'll need to handle changing requirements, database updates, scale, and more.</span></span> <span data-ttu-id="708eb-111">Neyse ki, Azure App Service, yeterince sorunsuz çalışmasını sitelerinizi sağlamanıza yardımcı olmak için özellikler ele sahiptir.</span><span class="sxs-lookup"><span data-stu-id="708eb-111">Fortunately, Azure App Service has you covered, with plenty of features to help you keep your sites running smoothly.</span></span>
> 
> <span data-ttu-id="708eb-112">Azure, güvenli ve esnek geliştirme, dağıtımı ve ölçeklendirme herhangi bir boyutu web uygulaması için seçenekleri sunar.</span><span class="sxs-lookup"><span data-stu-id="708eb-112">Azure offers secure and flexible development, deployment and scaling options for any size web application.</span></span> <span data-ttu-id="708eb-113">Altyapı yönetme uğraşmadan uygulamalar oluşturmak ve dağıtmak için varolan araçlarınızı yararlanın.</span><span class="sxs-lookup"><span data-stu-id="708eb-113">Leverage your existing tools to create and deploy applications without the hassle of managing infrastructure.</span></span>
> 
> <span data-ttu-id="708eb-114">Bir üretim web uygulaması, sık kullanılan geliştirme aracını kullanarak oluşturduğunuz içeriği kolayca dağıtarak kendiniz dakika cinsinden sağlayın.</span><span class="sxs-lookup"><span data-stu-id="708eb-114">Provision a production web application yourself in minutes by easily deploying content created using your favorite development tool.</span></span> <span data-ttu-id="708eb-115">Mevcut bir site için destek ile kaynak denetiminden doğrudan dağıtabilirsiniz **Git**, **GitHub**, **Bitbucket**, **TFS**ve hatta  **DropBox**.</span><span class="sxs-lookup"><span data-stu-id="708eb-115">You can deploy an existing site directly from source control with support for **Git**, **GitHub**, **Bitbucket**, **TFS**, and even **DropBox**.</span></span> <span data-ttu-id="708eb-116">Doğrudan sık kullanılan IDE'yi veya komut dosyalarını kullanarak dağıtmak **PowerShell** Windows veya **CLI** üzerinde herhangi bir işletim sistemi çalıştıran araçları.</span><span class="sxs-lookup"><span data-stu-id="708eb-116">Deploy directly from your favorite IDE or from scripts using **PowerShell** in Windows or **CLI** tools running on any OS.</span></span> <span data-ttu-id="708eb-117">Uygulama dağıtıldıktan sonra siteleriniz sürekli dağıtım desteği ile sürekli güncel tutun.</span><span class="sxs-lookup"><span data-stu-id="708eb-117">Once deployed, keep your sites constantly up-to-date with support for continuous deployment.</span></span>
> 
> <span data-ttu-id="708eb-118">Azure ölçeklenebilir, dayanıklı bulut depolama, yedekleme ve kurtarma çözümleri büyük veya küçük herhangi bir veri sağlar.</span><span class="sxs-lookup"><span data-stu-id="708eb-118">Azure provides scalable, durable cloud storage, backup, and recovery solutions for any data, big or small.</span></span> <span data-ttu-id="708eb-119">Bir üretim ortamında, tablolar, BLOB'lar ve SQL veritabanları gibi depolama hizmetleri uygulamaları dağıtırken, uygulamayı bulutta ölçek yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="708eb-119">When deploying applications to a production environment, storage services such as Tables, Blobs and SQL Databases, help you scale your application in the cloud.</span></span>
> 
> <span data-ttu-id="708eb-120">SQL veritabanları ile dağıtırken, uygulamanın yeni sürümlerini üretken veritabanınızı güncel tutmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="708eb-120">With SQL Databases, it is important to keep your productive database up-to-date when deploying new versions of your application.</span></span> <span data-ttu-id="708eb-121">Teşekkürler **Entity Framework Code First Migrations**, ortamınızı dakika cinsinden güncelleştirmek için veri modelinizi dağıtımını ve geliştirme basitleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="708eb-121">Thanks to **Entity Framework Code First Migrations**, the development and deployment of your data model has been simplified to update your environments in minutes.</span></span> <span data-ttu-id="708eb-122">Bu uygulamalı Laboratuvar web uygulamanızı Microsoft Azure üretim ortamlarında dağıtırken karşılaşabileceği farklı konuları gösterilir.</span><span class="sxs-lookup"><span data-stu-id="708eb-122">This hands-on lab will show you the different topics you could encounter when deploying your web app to production environments in Microsoft Azure.</span></span>
> 
> <span data-ttu-id="708eb-123">Tüm örnek kod ve parçacıkları Web Camps eğitim Seti, adresinde yer alan [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span><span class="sxs-lookup"><span data-stu-id="708eb-123">All sample code and snippets are included in the Web Camps Training Kit, available at [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).</span></span>
> 
> <span data-ttu-id="708eb-124">Daha fazla ayrıntılı kapsamını Bu konu için bkz: [Azure e-kitap ile yapı gerçek bulut uygulamaları](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span><span class="sxs-lookup"><span data-stu-id="708eb-124">For more in-depth coverage of this topic, see the [Building Real-World Cloud Apps with Azure e-book](building-real-world-cloud-apps-with-windows-azure/introduction.md).</span></span>


<a id="Overview"></a>
## <a name="overview"></a><span data-ttu-id="708eb-125">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="708eb-125">Overview</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="708eb-126">Amaçlar</span><span class="sxs-lookup"><span data-stu-id="708eb-126">Objectives</span></span>

<span data-ttu-id="708eb-127">Uygulamalı bu laboratuvarda, öğreneceksiniz nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="708eb-127">In this hands-on lab, you will learn how to:</span></span>

- <span data-ttu-id="708eb-128">Varolan bir modelle Entity Framework geçişleri etkinleştir</span><span class="sxs-lookup"><span data-stu-id="708eb-128">Enable Entity Framework Migrations with an existing model</span></span>
- <span data-ttu-id="708eb-129">Nesne modeli ve buna göre Entity Framework geçişler kullanarak veritabanını güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="708eb-129">Update the object model and database accordingly using Entity Framework Migrations</span></span>
- <span data-ttu-id="708eb-130">Git kullanarak Azure App Service'e dağıtma</span><span class="sxs-lookup"><span data-stu-id="708eb-130">Deploy to Azure App Service using Git</span></span>
- <span data-ttu-id="708eb-131">Azure Yönetim Portalı'nı kullanarak önceki bir dağıtıma geri alma</span><span class="sxs-lookup"><span data-stu-id="708eb-131">Rollback to a previous deployment using the Azure Management portal</span></span>
- <span data-ttu-id="708eb-132">Bir web uygulaması ölçeklendirmek için Azure Storage'ı kullanma</span><span class="sxs-lookup"><span data-stu-id="708eb-132">Use Azure Storage to scale a web app</span></span>
- <span data-ttu-id="708eb-133">Azure Yönetim Portalı'nı kullanarak bir web uygulaması için otomatik olarak ölçeklendirme yapılandırın</span><span class="sxs-lookup"><span data-stu-id="708eb-133">Configure auto-scaling for a web app using the Azure Management Portal</span></span>
- <span data-ttu-id="708eb-134">Oluşturma ve Visual Studio Yük testi projesi yapılandırma</span><span class="sxs-lookup"><span data-stu-id="708eb-134">Create and configure a load test project in Visual Studio</span></span>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="708eb-135">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="708eb-135">Prerequisites</span></span>

<span data-ttu-id="708eb-136">Aşağıdaki uygulamalı bu laboratuvarı tamamlamak için gereklidir:</span><span class="sxs-lookup"><span data-stu-id="708eb-136">The following is required to complete this hands-on lab:</span></span>

- <span data-ttu-id="708eb-137">[Visual Studio Express 2013 Web](https://www.microsoft.com/visualstudio/) veya daha büyük</span><span class="sxs-lookup"><span data-stu-id="708eb-137">[Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/) or greater</span></span>
- [<span data-ttu-id="708eb-138">2.2 .NET için Azure SDK</span><span class="sxs-lookup"><span data-stu-id="708eb-138">Azure SDK for .NET 2.2</span></span>](https://www.microsoft.com/windowsazure/sdk/)
- [<span data-ttu-id="708eb-139">GIT sürüm denetimi sistemi</span><span class="sxs-lookup"><span data-stu-id="708eb-139">GIT Version Control System</span></span>](http://git-scm.com/download)
- <span data-ttu-id="708eb-140">Microsoft Azure aboneliği</span><span class="sxs-lookup"><span data-stu-id="708eb-140">A Microsoft Azure subscription</span></span> 

    - <span data-ttu-id="708eb-141">Kaydolun bir [ücretsiz deneme sürümü](http://aka.ms/watk-freetrial)</span><span class="sxs-lookup"><span data-stu-id="708eb-141">Sign up for a [Free Trial](http://aka.ms/watk-freetrial)</span></span>
    - <span data-ttu-id="708eb-142">Visual Studio Professional, Test Professional, Premium veya Ultimate MSDN veya MSDN platformları aboneye varsa, etkinleştirme, [MSDN avantajı](http://aka.ms/watk-msdn) geliştirip Azure üzerinde test şimdi başlatmak için</span><span class="sxs-lookup"><span data-stu-id="708eb-142">If you are a Visual Studio Professional, Test Professional, Premium or Ultimate with MSDN or MSDN Platforms subscriber, activate your [MSDN benefit](http://aka.ms/watk-msdn) now to start developing and testing on Azure</span></span>
    - <span data-ttu-id="708eb-143">[BizSpark](http://aka.ms/watk-bizspark) üyeleri otomatik olarak Azure almak, Visual Studio Ultimate MSDN abonelikleri ile aracılığıyla avantajı</span><span class="sxs-lookup"><span data-stu-id="708eb-143">[BizSpark](http://aka.ms/watk-bizspark) members automatically receive the Azure benefit through their Visual Studio Ultimate with MSDN subscriptions</span></span>
    - <span data-ttu-id="708eb-144">Üyeleri [Microsoft iş ortağı ağı](http://aka.ms/watk-mpn) Bulut Temel program ücret ödemeden aylık Azure krediler alırsınız</span><span class="sxs-lookup"><span data-stu-id="708eb-144">Members of the [Microsoft Partner Network](http://aka.ms/watk-mpn) Cloud Essentials program receive monthly Azure credits at no charge</span></span>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="708eb-145">Kurulum</span><span class="sxs-lookup"><span data-stu-id="708eb-145">Setup</span></span>

<span data-ttu-id="708eb-146">Bu uygulamalı Laboratuvar alıştırmaları çalıştırmak için ortamınızı ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="708eb-146">In order to run the exercises in this hands-on lab, you will need to set up your environment first.</span></span>

1. <span data-ttu-id="708eb-147">Açık Windows Gezgini ve Laboratuvar 's gözatın **kaynak** klasör.</span><span class="sxs-lookup"><span data-stu-id="708eb-147">Open Windows Explorer and browse to the lab's **Source** folder.</span></span>
2. <span data-ttu-id="708eb-148">Sağ **Setup.cmd** seçip **yönetici olarak çalıştır** ortamınızı yapılandırmanıza ve bu Laboratuvar için Visual Studio kod parçacıkları yükleme kurulum işlemi başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="708eb-148">Right-click on **Setup.cmd** and select **Run as administrator** to launch the setup process that will configure your environment and install the Visual Studio code snippets for this lab.</span></span>
3. <span data-ttu-id="708eb-149">Kullanıcı Hesabı Denetimi iletişim gösteriliyorsa, devam etmek için eylemi onaylayın.</span><span class="sxs-lookup"><span data-stu-id="708eb-149">If the User Account Control dialog is shown, confirm the action to proceed.</span></span>

> [!NOTE]
> <span data-ttu-id="708eb-150">Kurulumu çalıştırmadan önce bu Laboratuvar için tüm bağımlılıkların işaretli olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="708eb-150">Make sure you have checked all the dependencies for this lab before running the setup.</span></span>


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a><span data-ttu-id="708eb-151">Kod parçacıkları</span><span class="sxs-lookup"><span data-stu-id="708eb-151">Using the Code Snippets</span></span>

<span data-ttu-id="708eb-152">Laboratuvar belge boyunca kod blokları eklemeye yönlendirilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="708eb-152">Throughout the lab document, you will be instructed to insert code blocks.</span></span> <span data-ttu-id="708eb-153">Size kolaylık olması için bu kodu çoğunu, Visual Studio el ile eklemek zorunda kalmamak için 2013 içinde erişebileceğiniz Visual Studio kod parçacıkları, olarak sağlanır.</span><span class="sxs-lookup"><span data-stu-id="708eb-153">For your convenience, most of this code is provided as Visual Studio Code Snippets, which you can access from within Visual Studio 2013 to avoid having to add it manually.</span></span>

> [!NOTE]
> <span data-ttu-id="708eb-154">Her alıştırma bulunan Başlangıç bir çözüm tarafından eşlik **başlamak** diğer bağımsız olarak her alıştırma izlemenizi sağlar alıştırmanın klasör.</span><span class="sxs-lookup"><span data-stu-id="708eb-154">Each exercise is accompanied by a starting solution located in the **Begin** folder of the exercise that allows you to follow each exercise independently of the others.</span></span> <span data-ttu-id="708eb-155">Lütfen bir alıştırma sırasında eklenen kod parçacıkları çözümleri başlangıç bunlardan eksik ve alıştırma tamamlayıncaya kadar çalışmayabilir unutmayın.</span><span class="sxs-lookup"><span data-stu-id="708eb-155">Please be aware that the code snippets that are added during an exercise are missing from these starting solutions and may not work until you have completed the exercise.</span></span> <span data-ttu-id="708eb-156">Bir alıştırma için kaynak kodunu içinde da bulacaksınız bir **son** karşılık gelen alıştırmada adımları tamamlamanızı sonuçları kodunu içeren bir Visual Studio çözümü içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="708eb-156">Inside the source code for an exercise, you will also find an **End** folder containing a Visual Studio solution with the code that results from completing the steps in the corresponding exercise.</span></span> <span data-ttu-id="708eb-157">Uygulamalı bu Laboratuvar çalışırken Ek Yardım gerekirse, bu çözümlerin kılavuz olarak kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="708eb-157">You can use these solutions as guidance if you need additional help as you work through this hands-on lab.</span></span>


* * *

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="708eb-158">Alıştırmaları</span><span class="sxs-lookup"><span data-stu-id="708eb-158">Exercises</span></span>

<span data-ttu-id="708eb-159">Bu uygulamalı Laboratuvar aşağıdaki alıştırmaları içerir:</span><span class="sxs-lookup"><span data-stu-id="708eb-159">This hands-on lab includes the following exercises:</span></span>

1. [<span data-ttu-id="708eb-160">Entity Framework geçişler kullanma</span><span class="sxs-lookup"><span data-stu-id="708eb-160">Using Entity Framework Migrations</span></span>](#Exercise1)
2. [<span data-ttu-id="708eb-161">Hazırlama için bir Web uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="708eb-161">Deploying a Web App to Staging</span></span>](#Exercise2)
3. [<span data-ttu-id="708eb-162">Üretim dağıtımı geri alınmasının</span><span class="sxs-lookup"><span data-stu-id="708eb-162">Performing Deployment Rollback in Production</span></span>](#Exercise3)
4. [<span data-ttu-id="708eb-163">Azure Storage kullanma ölçeklendirme</span><span class="sxs-lookup"><span data-stu-id="708eb-163">Scaling Using Azure Storage</span></span>](#Exercise4)
5. <span data-ttu-id="708eb-164">[Web uygulamaları için otomatik ölçeklendirme kullanarak](#Exercise5) (Visual Studio 2013 Ultimate edition için isteğe bağlı)</span><span class="sxs-lookup"><span data-stu-id="708eb-164">[Using Autoscale for Web Apps](#Exercise5) (Optional for Visual Studio 2013 Ultimate edition)</span></span>

<span data-ttu-id="708eb-165">Bu laboratuvarı tamamlamak için süre tahmini: **75 dakika**</span><span class="sxs-lookup"><span data-stu-id="708eb-165">Estimated time to complete this lab: **75 minutes**</span></span>

> [!NOTE]
> <span data-ttu-id="708eb-166">Visual Studio ilk kez başlattığınızda, önceden tanımlı ayarlar koleksiyonları birini seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="708eb-166">When you first start Visual Studio, you must select one of the predefined settings collections.</span></span> <span data-ttu-id="708eb-167">Önceden tanımlanmış her koleksiyon belirli geliştirme stili eşleşecek şekilde tasarlanmıştır ve pencere düzenlerini, düzenleyicisinin davranışı, IntelliSense kod parçacıkları ve iletişim kutusu seçenekleri belirler.</span><span class="sxs-lookup"><span data-stu-id="708eb-167">Each predefined collection is designed to match a particular development style and determines window layouts, editor behavior, IntelliSense code snippets, and dialog box options.</span></span> <span data-ttu-id="708eb-168">Bu Laboratuvar yordamlarda kullanırken Visual Studio'da belirli bir görevi gerçekleştirmek için gerekli eylemleri açıklamak **genel geliştirme ayarları** koleksiyonu.</span><span class="sxs-lookup"><span data-stu-id="708eb-168">The procedures in this lab describe the actions necessary to accomplish a given task in Visual Studio when using the **General Development Settings** collection.</span></span> <span data-ttu-id="708eb-169">Geliştirme ortamınız için farklı ayarlar koleksiyonu seçerseniz, dikkate almanız gereken adımları farklılıklar olabilir.</span><span class="sxs-lookup"><span data-stu-id="708eb-169">If you choose a different settings collection for your development environment, there may be differences in the steps that you should take into account.</span></span>


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a><span data-ttu-id="708eb-170">Alıştırma 1: Entity Framework geçişler kullanma</span><span class="sxs-lookup"><span data-stu-id="708eb-170">Exercise 1: Using Entity Framework Migrations</span></span>

<span data-ttu-id="708eb-171">Bir uygulama geliştirirken, veri modelinizi zaman içinde değişebilir.</span><span class="sxs-lookup"><span data-stu-id="708eb-171">When you are developing an application, your data model might change over time.</span></span> <span data-ttu-id="708eb-172">(Yeni bir sürüm oluşturuyorsanız) bu değişiklikler, veritabanınızdaki varolan modeli etkileyebilir ve veritabanınızı hataları önlemek için güncel tutmak önemlidir.</span><span class="sxs-lookup"><span data-stu-id="708eb-172">These changes could affect the existing model in your database (if you are creating a new version) and it is important to keep your database up-to-date to prevent errors.</span></span>

<span data-ttu-id="708eb-173">Bu değişiklikler, modelinizdeki izleme basitleştirmek için **Entity Framework Code First Migrations** modelinizi veritabanı şeması karşılaştırma değişiklikleri otomatik olarak algılayıp, veritabanını güncelleştirmek için özel kod oluşturur Yeni oluşturma *sürümleri* veritabanınızın.</span><span class="sxs-lookup"><span data-stu-id="708eb-173">To simplify the tracking of these changes in your model, **Entity Framework Code First Migrations** automatically detect changes comparing your model with the database schema and generates specific code to update your database, creating new *versions* of your database.</span></span>

<span data-ttu-id="708eb-174">Bu alıştırmada nasıl etkinleştirileceği gösterilmiştir **geçişler** uygulamanız ve nasıl kolayca algılayabilir ve veritabanlarınızı güncelleştirmek için değişiklikleri oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="708eb-174">This exercise shows you how to enable **Migrations** for your application and how you can easily detect and generate changes to update your databases.</span></span>

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a><span data-ttu-id="708eb-175">Görev 1 – etkinleştirme geçişleri</span><span class="sxs-lookup"><span data-stu-id="708eb-175">Task 1 – Enabling Migrations</span></span>

<span data-ttu-id="708eb-176">Bu görevde, etkinleştirme adımları boyunca geçer **Entity Framework Code First Migrations** için **günlük test** modelini değiştirme ve bu değişiklikleri nasıl yansıtılır anlama veritabanı Veritabanı.</span><span class="sxs-lookup"><span data-stu-id="708eb-176">In this task, you will go through the steps of enabling **Entity Framework Code First Migrations** to the **Geek Quiz** database, changing the model and understanding how those changes are reflected in the database.</span></span>

1. <span data-ttu-id="708eb-177">Visual Studio'yu açın ve açık **GeekQuiz.sln** çözüm dosyasından **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.</span><span class="sxs-lookup"><span data-stu-id="708eb-177">Open Visual Studio and open the **GeekQuiz.sln** solution file from **Source\Ex1-UsingEntityFrameworkMigrations\Begin**.</span></span>
2. <span data-ttu-id="708eb-178">İndirmek ve yüklemek için çözüm yapı **NuGet** paket bağımlılıkları.</span><span class="sxs-lookup"><span data-stu-id="708eb-178">Build the solution in order to download and install the **NuGet** package dependencies.</span></span> <span data-ttu-id="708eb-179">Bunu yapmak için çözüme sağ tıklayın ve **yapı çözümü** veya basın **Ctrl + Shift + B**.</span><span class="sxs-lookup"><span data-stu-id="708eb-179">To do this, right-click the solution and click **Build Solution** or press **Ctrl + Shift + B**.</span></span>
3. <span data-ttu-id="708eb-180">Gelen **Araçları** menü Visual Studio'da seçin **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="708eb-180">From the **Tools** menu in Visual Studio, select **Library Package Manager**, and then click **Package Manager Console**.</span></span>
4. <span data-ttu-id="708eb-181">İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="708eb-181">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="708eb-182">Varolan modelini temel alan bir ilk geçiş oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="708eb-182">An initial migration based on the existing model will be created.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    <span data-ttu-id="708eb-183">![Geçişler etkinleştirme](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "geçişler etkinleştirme")</span><span class="sxs-lookup"><span data-stu-id="708eb-183">![Enabling Migrations](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "Enabling Migrations")</span></span>

    <span data-ttu-id="708eb-184">*Geçişler etkinleştirme*</span><span class="sxs-lookup"><span data-stu-id="708eb-184">*Enabling Migrations*</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-185">Bu komut ekler bir **geçişler** adlı bir dosyayı içeren günlük test projesi için klasörde **Configuration.cs**.</span><span class="sxs-lookup"><span data-stu-id="708eb-185">This command adds a **Migrations** folder to Geek Quiz project that contains a file called **Configuration.cs**.</span></span> <span data-ttu-id="708eb-186">**Yapılandırma** sınıfı geçişler içeriğiniz için nasıl davranacağını yapılandırmanıza olanak verir.</span><span class="sxs-lookup"><span data-stu-id="708eb-186">The **Configuration** class allows you to configure how Migrations behaves for your context.</span></span>
5. <span data-ttu-id="708eb-187">Etkin geçiş ile güncelleştirmeniz gerekir. **yapılandırma** veritabanı ilk verileri ile doldurmak için sınıf, **günlük test** gerektirir.</span><span class="sxs-lookup"><span data-stu-id="708eb-187">With Migrations enabled, you need to update the **Configuration** class to populate the database with the initial data that **Geek Quiz** requires.</span></span> <span data-ttu-id="708eb-188">Altında **geçişler**, yerine **Configuration.cs** bir dosyayla bulunan **Source\Assets** bu laboratuvarı klasör.</span><span class="sxs-lookup"><span data-stu-id="708eb-188">Under **Migrations**, replace the **Configuration.cs** file with the one located in the **Source\Assets** folder of this lab.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-189">Bu yana **geçişler** çağıracak **çekirdek** yöntemi her veritabanı güncelleştirme ihtiyacınız kayıtları veritabanında çoğaltılmadığından emin olmak.</span><span class="sxs-lookup"><span data-stu-id="708eb-189">Since **Migrations** will call the **Seed** method with every database update, you need to be sure that records are not duplicated in the database.</span></span> <span data-ttu-id="708eb-190">**Örnek** yöntemi yinelenen verileri önlemek için yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="708eb-190">The **AddOrUpdate** method will help to prevent duplicate data.</span></span>
6. <span data-ttu-id="708eb-191">İlk geçiş eklemek için aşağıdaki komutu girin ve sonra basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="708eb-191">To add an initial migration, enter the following command and then press **Enter**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-192">Adlı bir veritabanı olduğundan emin olun &quot;GeekQuizProd&quot; LocalDB Örneğinizde.</span><span class="sxs-lookup"><span data-stu-id="708eb-192">Make sure that there is no database named &quot;GeekQuizProd&quot; in your LocalDB instance.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    <span data-ttu-id="708eb-193">![Temel şema geçiş ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "ekleme temel şema geçiş")</span><span class="sxs-lookup"><span data-stu-id="708eb-193">![Adding base schema migration](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "Adding base schema migration")</span></span>

    <span data-ttu-id="708eb-194">*Temel şema geçiş ekleme*</span><span class="sxs-lookup"><span data-stu-id="708eb-194">*Adding base schema migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-195">**Add-Migration** son geçiş oluşturulmasından bu yana modelinize yapmış değişikliklere dayalı sonraki geçişin iskelesi.</span><span class="sxs-lookup"><span data-stu-id="708eb-195">**Add-Migration** will scaffold the next migration based on changes you have made to your model since the last migration was created.</span></span> <span data-ttu-id="708eb-196">Bu durumda, ilk geçiş projesinin olduğu gibi tanımlanan tüm tabloları oluşturmak için komut dosyalarını ekleyecek **TriviaContext** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="708eb-196">In this case, as it is the first migration of the project, it will add the scripts to create all the tables defined in the **TriviaContext** class.</span></span>
7. <span data-ttu-id="708eb-197">Aşağıdaki komutu çalıştırarak veritabanını güncelleştirmek için geçiş yürütün.</span><span class="sxs-lookup"><span data-stu-id="708eb-197">Execute the migration to update the database by running the following command.</span></span> <span data-ttu-id="708eb-198">Bu komut için belirttiğiniz **ayrıntılı** hedef veritabanına uygulanmakta olan SQL deyimlerini görüntülemek için bayrak.</span><span class="sxs-lookup"><span data-stu-id="708eb-198">For this command you will specify the **Verbose** flag to view the SQL statements being applied to the target database.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    <span data-ttu-id="708eb-199">![Oluşturma ilk veritabanı](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "ilk veritabanı oluşturma")</span><span class="sxs-lookup"><span data-stu-id="708eb-199">![Creating initial database](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "Creating initial database")</span></span>

    <span data-ttu-id="708eb-200">*İlk veritabanı oluşturma*</span><span class="sxs-lookup"><span data-stu-id="708eb-200">*Creating initial database*</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-201">**Update-Database** bekleyen tüm geçişleri veritabanına uygulanır.</span><span class="sxs-lookup"><span data-stu-id="708eb-201">**Update-Database** will apply any pending migrations to the database.</span></span> <span data-ttu-id="708eb-202">Bu durumda, web.config dosyasında tanımlanan bağlantı dizesi kullanılarak veritabanı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="708eb-202">In this case, it will create the database using the connection string defined in your web.config file.</span></span>
8. <span data-ttu-id="708eb-203">Git **Görünüm** menü ve açık **SQL Server Nesne Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="708eb-203">Go to **View** menu and open **SQL Server Object Explorer**.</span></span>

    <span data-ttu-id="708eb-204">![SQL Server nesne Gezgini'nde Aç](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "SQL Server nesne Gezgini'nde Aç")</span><span class="sxs-lookup"><span data-stu-id="708eb-204">![Open in SQL Server Object Explorer](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "Open in SQL Server Object Explorer")</span></span>

    <span data-ttu-id="708eb-205">*SQL Server nesne Gezgini'nde Aç*</span><span class="sxs-lookup"><span data-stu-id="708eb-205">*Open in SQL Server Object Explorer*</span></span>
9. <span data-ttu-id="708eb-206">İçinde **SQL Server Nesne Gezgini** penceresinde sağ tıklayarak, yerel veritabanı örneğine bağlanın **SQL Server** düğümü ve seçerek **SQL Server Ekle...**  seçeneği.</span><span class="sxs-lookup"><span data-stu-id="708eb-206">In the **SQL Server Object Explorer** window, connect to your LocalDB instance by right-clicking the **SQL Server** node and selecting **Add SQL Server...** option.</span></span>

    <span data-ttu-id="708eb-207">![Bir SQL Server örneği ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "bir SQL Server örneği ekleme")</span><span class="sxs-lookup"><span data-stu-id="708eb-207">![Adding a SQL Server Instance](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "Adding a SQL Server Instance")</span></span>

    <span data-ttu-id="708eb-208">*Bir SQL Server örneği için SQL Server Nesne Gezgini ekleme*</span><span class="sxs-lookup"><span data-stu-id="708eb-208">*Adding a SQL Server instance to SQL Server Object Explorer*</span></span>
10. <span data-ttu-id="708eb-209">Ayarlama **sunucu adı** için *(localdb) \v11.0* bırakıp **Windows kimlik doğrulaması** , kimlik doğrulama modu.</span><span class="sxs-lookup"><span data-stu-id="708eb-209">Set the **server name** to *(localdb)\v11.0* and leave **Windows Authentication** as your authentication mode.</span></span> <span data-ttu-id="708eb-210">Tıklatın **Bağlan** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="708eb-210">Click **Connect** to continue.</span></span>

    <span data-ttu-id="708eb-211">![Yerel veritabanına bağlanma](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "yerel veritabanına bağlanma")</span><span class="sxs-lookup"><span data-stu-id="708eb-211">![Connecting to LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "Connecting to LocalDB")</span></span>

    <span data-ttu-id="708eb-212">*Yerel veritabanına bağlanma*</span><span class="sxs-lookup"><span data-stu-id="708eb-212">*Connecting to LocalDB*</span></span>
11. <span data-ttu-id="708eb-213">Açık **GeekQuizProd** veritabanı ve genişletin **tabloları** düğümü.</span><span class="sxs-lookup"><span data-stu-id="708eb-213">Open the **GeekQuizProd** database and expand the **Tables** node.</span></span> <span data-ttu-id="708eb-214">Gördüğünüz gibi **Update-Database** komutu oluşturulan tanımlanan tüm tabloları **TriviaContext** sınıfı.</span><span class="sxs-lookup"><span data-stu-id="708eb-214">As you can see, the **Update-Database** command generated all the tables defined in the **TriviaContext** class.</span></span> <span data-ttu-id="708eb-215">Bulun **dbo. TriviaQuestions** tablo ve sütun düğümünü açın.</span><span class="sxs-lookup"><span data-stu-id="708eb-215">Locate the **dbo.TriviaQuestions** table and open the columns node.</span></span> <span data-ttu-id="708eb-216">Sonraki görev, komut bu tabloya yeni bir sütun ekleyin ve kullanarak veritabanını güncelleştirir **geçişler**.</span><span class="sxs-lookup"><span data-stu-id="708eb-216">In the next task, you will add a new column to this table and update the database using **Migrations**.</span></span>

    <span data-ttu-id="708eb-217">![Trivia sorular sütunları](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia sorular sütunları")</span><span class="sxs-lookup"><span data-stu-id="708eb-217">![Trivia Questions Columns](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "Trivia Questions Columns")</span></span>

    <span data-ttu-id="708eb-218">*Sütunları Trivia sorular*</span><span class="sxs-lookup"><span data-stu-id="708eb-218">*Trivia Questions Columns*</span></span>

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a><span data-ttu-id="708eb-219">Görev 2 – geçişleri kullanmaya güncelleştirme veritabanı şeması</span><span class="sxs-lookup"><span data-stu-id="708eb-219">Task 2 – Updating Database Schema Using Migrations</span></span>

<span data-ttu-id="708eb-220">Bu görevde, kullanacağınız **Entity Framework Code First Migrations** modelinizi bir değişikliği algılar ve veritabanını güncelleştirmek için gerekli kodu oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="708eb-220">In this task, you will use **Entity Framework Code First Migrations** to detect a change in your model and generate the necessary code to update the database.</span></span> <span data-ttu-id="708eb-221">Güncelleştirmek **TriviaQuestions** yeni bir özellik ekleyerek varlık.</span><span class="sxs-lookup"><span data-stu-id="708eb-221">You will update the **TriviaQuestions** entity by adding a new property to it.</span></span> <span data-ttu-id="708eb-222">Ardından tabloda yeni bir sütun eklemek için yeni bir geçiş oluşturmak için komutları çalışır.</span><span class="sxs-lookup"><span data-stu-id="708eb-222">Then you will run commands to create a new Migration to include the new column in the table.</span></span>

1. <span data-ttu-id="708eb-223">İçinde **Çözüm Gezgini**, çift **TriviaQuestion.cs** içinde bulunan dosyasını **modelleri** klasör.</span><span class="sxs-lookup"><span data-stu-id="708eb-223">In **Solution Explorer**, double-click the **TriviaQuestion.cs** file located inside the **Models** folder.</span></span>
2. <span data-ttu-id="708eb-224">Adlı yeni bir özellik eklemek **İpucu**, aşağıdaki kod parçacığında gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="708eb-224">Add a new property named **Hint**, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. <span data-ttu-id="708eb-225">İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="708eb-225">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span> <span data-ttu-id="708eb-226">Yeni bir geçiş modelimizi değişikliği yansıtacak şekilde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="708eb-226">A new migration will be created reflecting the change in our model.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    <span data-ttu-id="708eb-227">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "eklemek geçiş")</span><span class="sxs-lookup"><span data-stu-id="708eb-227">![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")</span></span>

    <span data-ttu-id="708eb-228">*Add-Migration*</span><span class="sxs-lookup"><span data-stu-id="708eb-228">*Add-Migration*</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-229">Bir geçiş dosyası iki yöntemden oluşan **yukarı** ve **aşağı**.</span><span class="sxs-lookup"><span data-stu-id="708eb-229">A Migration file is composed of two methods, **Up** and **Down**.</span></span>
    > 
    > - <span data-ttu-id="708eb-230">**Yukarı** yöntemi, ne bizim uygulama veritabanı için geçerli gerek geçerli sürümü değişiklikler belirtmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="708eb-230">The **Up** method will be used to specify what changes the current version of our application need to apply to the database.</span></span>
    > - <span data-ttu-id="708eb-231">**Aşağı** biz eklediğiniz değişiklikleri geri almak için kullanılan **yukarı** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="708eb-231">The **Down** is used to reverse the changes we have added to the **Up** method.</span></span>
    > 
    > <span data-ttu-id="708eb-232">Veritabanı geçiş veritabanı güncelleştirildiğinde, bu zaman damgası sırasını ve yalnızca son güncelleştirmeden bu yana kullanılan değil tüm geçişler çalışır ( \_MigrationHistory tablosu izler hangi geçiş uygulanmadı).</span><span class="sxs-lookup"><span data-stu-id="708eb-232">When the Database Migration updates the database, it will run all migrations in the timestamp order, and only those that have not been used since the last update (The \_MigrationHistory table keeps track of which migrations have been applied).</span></span> <span data-ttu-id="708eb-233">**Yukarı** tüm geçişler yöntemi çağrılır ve biz veritabanına belirttiğiniz değişiklikleri yapar.</span><span class="sxs-lookup"><span data-stu-id="708eb-233">The **Up** method of all migrations will be called and will make the changes we have specified to the database.</span></span> <span data-ttu-id="708eb-234">Önceki bir geçiş için geri dönün karar verirseniz **aşağı** yöntemi, ters sırada değişiklikleri yinelemek için çağrılır.</span><span class="sxs-lookup"><span data-stu-id="708eb-234">If we decide to go back to a previous migration, the **Down** method will be called to redo the changes in a reverse order.</span></span>
4. <span data-ttu-id="708eb-235">İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="708eb-235">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. <span data-ttu-id="708eb-236">Çıktısını **Update-Database** oluşturulan komut bir **Alter Table** yeni bir sütun eklemek için SQL deyimi **TriviaQuestions** , aşağıdaki resimde gösterildiği gibi tablo.</span><span class="sxs-lookup"><span data-stu-id="708eb-236">The output of the **Update-Database** command generated an **Alter Table** SQL statement to add a new column to the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="708eb-237">![Oluşturulan sütun SQL deyimine](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "oluşturulan sütun SQL deyimini ekleyin")</span><span class="sxs-lookup"><span data-stu-id="708eb-237">![Add column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "Add column SQL statement generated")</span></span>

    <span data-ttu-id="708eb-238">*Oluşturulan sütun SQL deyimini ekleyin*</span><span class="sxs-lookup"><span data-stu-id="708eb-238">*Add column SQL statement generated*</span></span>
6. <span data-ttu-id="708eb-239">İçinde **SQL Server Nesne Gezgini**, yenileme **dbo. TriviaQuestions** tablo ve denetleyin yeni **İpucu** sütununda görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="708eb-239">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the new **Hint** column is displayed.</span></span>

    <span data-ttu-id="708eb-240">![Yeni bir ipucu sütun gösteren](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "yeni ipucu sütunu gösterme")</span><span class="sxs-lookup"><span data-stu-id="708eb-240">![Showing the new Hint Column](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "Showing the new Hint Column")</span></span>

    <span data-ttu-id="708eb-241">*Yeni bir ipucu sütun gösterme*</span><span class="sxs-lookup"><span data-stu-id="708eb-241">*Showing the new Hint Column*</span></span>
7. <span data-ttu-id="708eb-242">Geri **TriviaQuestion.cs** Düzenleyicisi eklemek bir **StringLength** kısıtlama *İpucu* aşağıdaki kod parçacığında gösterildiği gibi özelliği.</span><span class="sxs-lookup"><span data-stu-id="708eb-242">Back in the **TriviaQuestion.cs** editor, add a **StringLength** constraint to the *Hint* property, as shown in the following code snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. <span data-ttu-id="708eb-243">İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="708eb-243">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. <span data-ttu-id="708eb-244">İçinde **Paket Yöneticisi Konsolu**, aşağıdaki komutu girin ve ardından basın **Enter**.</span><span class="sxs-lookup"><span data-stu-id="708eb-244">In the **Package Manager Console**, enter the following command and then press **Enter**.</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. <span data-ttu-id="708eb-245">Çıktısını **Update-Database** oluşturulan komut bir **Alter Table** güncelleştirmek için SQL deyimi *İpucu* sütunun türü **TriviaQuestions** , aşağıdaki resimde gösterildiği gibi tablo.</span><span class="sxs-lookup"><span data-stu-id="708eb-245">The output of the **Update-Database** command generated an **Alter Table** SQL statement to update the *hint* column type of the **TriviaQuestions** table, as shown in the image below.</span></span>

    <span data-ttu-id="708eb-246">![Oluşturulan sütun SQL deyimini alter](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter oluşturulan sütun SQL deyimi")</span><span class="sxs-lookup"><span data-stu-id="708eb-246">![Alter column SQL statement generated](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "Alter column SQL statement generated")</span></span>

    <span data-ttu-id="708eb-247">*Oluşturulan sütun SQL deyimini değiştirme*</span><span class="sxs-lookup"><span data-stu-id="708eb-247">*Alter column SQL statement generated*</span></span>
11. <span data-ttu-id="708eb-248">İçinde **SQL Server Nesne Gezgini**, yenileme **dbo. TriviaQuestions** tablo ve denetleyin **İpucu** sütunun türü **nvarchar(150)**.</span><span class="sxs-lookup"><span data-stu-id="708eb-248">In **SQL Server Object Explorer**, refresh the **dbo.TriviaQuestions** table and check that the **Hint** column type is **nvarchar(150)**.</span></span>

    <span data-ttu-id="708eb-249">![New kısıtlaması gösteren](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "new kısıtlaması gösterme")</span><span class="sxs-lookup"><span data-stu-id="708eb-249">![Showing the new constraint](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "Showing the new constraint")</span></span>

    <span data-ttu-id="708eb-250">*New kısıtlaması gösterme*</span><span class="sxs-lookup"><span data-stu-id="708eb-250">*Showing the new constraint*</span></span>

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a><span data-ttu-id="708eb-251">Alıştırma 2: hazırlama için bir Web uygulaması dağıtma</span><span class="sxs-lookup"><span data-stu-id="708eb-251">Exercise 2: Deploying a Web App to Staging</span></span>

<span data-ttu-id="708eb-252">**Web uygulamaları Azure App Service'te** hazırlanmış yayımlamayı gerçekleştirmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="708eb-252">**Web Apps in Azure App Service** enables you to perform staged publishing.</span></span> <span data-ttu-id="708eb-253">Hazırlanmış yayımlamayı her varsayılan üretim sitesi için hazırlama bir site yuva oluşturur ve hiçbir zaman aşağı bu yuvaları takas olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="708eb-253">Staged publishing creates a staging site slot for each default production site and enables you to swap these slots with no down time.</span></span> <span data-ttu-id="708eb-254">Bu ortak serbest bırakmadan önce değişiklikleri doğrulamak için artımlı olarak site içeriğini tümleştirmek ve geri değişiklikleri beklendiği gibi çalışmıyorsanız alma gerçekten yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="708eb-254">This is really useful to validate changes before releasing to the public, incrementally integrate site content, and roll back if changes are not working as expected.</span></span>

<span data-ttu-id="708eb-255">Bu alıştırmada, dağıtacağınız **günlük test** hazırlama ortamına Git kaynak denetimi kullanarak, web uygulamanızın uygulama.</span><span class="sxs-lookup"><span data-stu-id="708eb-255">In this exercise, you will deploy the **Geek Quiz** application to the staging environment of your web app using Git source control.</span></span> <span data-ttu-id="708eb-256">Bunu yapmak için web uygulaması oluşturma ve Yönetim Portalı'nı gerekli bileşenler sağlama ve yapılandırma bir **Git** depo ve anında iletme uygulamanın kaynak kodu yerel bilgisayarınızdan hazırlama yuvası için.</span><span class="sxs-lookup"><span data-stu-id="708eb-256">To do this, you will create the web app and provision the required components at the management portal, configure a **Git** repository and push the application source code from your local computer to the staging slot.</span></span> <span data-ttu-id="708eb-257">Üretim veritabanınız güncelleştirmek **Code First Migrations** önceki alıştırmada oluşturulan.</span><span class="sxs-lookup"><span data-stu-id="708eb-257">You will also update your production database with the **Code First Migrations** you created in the previous exercise.</span></span> <span data-ttu-id="708eb-258">Ardından, işlemi doğrulamak için bu test ortamında uygulama yürütülür.</span><span class="sxs-lookup"><span data-stu-id="708eb-258">You will then execute the application in this test environment to verify its operation.</span></span> <span data-ttu-id="708eb-259">Memnun sonra onun beklentilerinizi göre çalıştığından, üretim uygulamaya yükseltmez.</span><span class="sxs-lookup"><span data-stu-id="708eb-259">Once you are satisfied that it is working according to your expectations, you will promote the application to production.</span></span>

> [!NOTE]
> <span data-ttu-id="708eb-260">Hazırlanmış yayımlamayı etkinleştirmek için web uygulaması olmalıdır **Standart mod**.</span><span class="sxs-lookup"><span data-stu-id="708eb-260">To enable staged publishing, the web app must be in **Standard mode**.</span></span> <span data-ttu-id="708eb-261">Web uygulamanızı standart moda değiştirirseniz, ek ücret tahakkuk olduğunu unutmayın.</span><span class="sxs-lookup"><span data-stu-id="708eb-261">Note that additional charges will be incurred if you change your web app to Standard mode.</span></span> <span data-ttu-id="708eb-262">Fiyatlandırma hakkında daha fazla bilgi için bkz: [App Service fiyatlandırması](https://azure.microsoft.com/pricing/details/app-service/).</span><span class="sxs-lookup"><span data-stu-id="708eb-262">For more information about pricing, see [App Service Pricing](https://azure.microsoft.com/pricing/details/app-service/).</span></span>


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a><span data-ttu-id="708eb-263">Görev 1 – Azure App Service'te bir Web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="708eb-263">Task 1 – Creating a Web App in Azure App Service</span></span>

<span data-ttu-id="708eb-264">Bu görevde, bir web uygulaması oluşturacak **Azure App Service** Yönetim Portalı.</span><span class="sxs-lookup"><span data-stu-id="708eb-264">In this task, you will create a web app in **Azure App Service** from the management portal.</span></span> <span data-ttu-id="708eb-265">Ayrıca yapılandıracak bir **SQL veritabanı** uygulama verilerini kalıcı hale getirmek ve kaynak denetimi için yerel bir Git deposu yapılandırmak için.</span><span class="sxs-lookup"><span data-stu-id="708eb-265">You will also configure a **SQL Database** to persist the application data, and configure a local Git repository for source control.</span></span>

1. <span data-ttu-id="708eb-266">Git [Azure Yönetim Portalı](https://manage.windowsazure.com) ve aboneliğinizle ilişkili Microsoft hesabı kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="708eb-266">Go to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>

    ![Azure yönetim portalında oturum açın](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    <span data-ttu-id="708eb-268">*Azure yönetim portalında oturum açın*</span><span class="sxs-lookup"><span data-stu-id="708eb-268">*Sign in to the Azure management portal*</span></span>
2. <span data-ttu-id="708eb-269">Tıklatın **yeni** sayfanın altındaki komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="708eb-269">Click **New** in the command bar at the bottom of the page.</span></span>

    <span data-ttu-id="708eb-270">![Yeni bir web uygulaması oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "yeni bir web uygulaması oluşturma")</span><span class="sxs-lookup"><span data-stu-id="708eb-270">![Creating a new web app](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "Creating a new web app")</span></span>

    <span data-ttu-id="708eb-271">*Yeni bir web uygulaması oluşturma*</span><span class="sxs-lookup"><span data-stu-id="708eb-271">*Creating a new web app*</span></span>
3. <span data-ttu-id="708eb-272">Tıklatın **işlem**, **Web sitesi** ve ardından **özel Oluştur**.</span><span class="sxs-lookup"><span data-stu-id="708eb-272">Click **Compute**, **Website** and then **Custom Create**.</span></span>

    <span data-ttu-id="708eb-273">![Özel Oluştur kullanarak yeni bir web uygulaması oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "özel Oluştur kullanarak yeni bir web uygulaması oluşturma")</span><span class="sxs-lookup"><span data-stu-id="708eb-273">![Creating a new web app using Custom Create](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "Creating a new web app using Custom Create")</span></span>

    <span data-ttu-id="708eb-274">*Özel Oluştur kullanarak yeni bir web uygulaması oluşturma*</span><span class="sxs-lookup"><span data-stu-id="708eb-274">*Creating a new web app using Custom Create*</span></span>
4. <span data-ttu-id="708eb-275">İçinde **yeni Web sitesi - özel Oluştur** iletişim kutusunda, kullanılabilir bir sağlayın **URL** (örneğin *günlük test*), bir konum seçin **bölge** aşağı açılan liste ve select **yeni bir SQL veritabanı oluşturma** içinde **veritabanı** aşağı açılan liste.</span><span class="sxs-lookup"><span data-stu-id="708eb-275">In the **New Website - Custom Create** dialog box, provide an available **URL** (e.g. *geek-quiz*), select a location in the **Region** drop-down list, and select **Create a new SQL database** in the **Database** drop-down list.</span></span> <span data-ttu-id="708eb-276">Son olarak, seçin **kaynak denetiminden Yayımla** onay kutusunu ve tıklatın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="708eb-276">Finally, select the **Publish from source control** check box and click **Next**.</span></span>

    ![Yeni web uygulamasını özelleştirme](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    <span data-ttu-id="708eb-278">*Yeni web uygulamasını özelleştirme*</span><span class="sxs-lookup"><span data-stu-id="708eb-278">*Customizing the new web app*</span></span>
5. <span data-ttu-id="708eb-279">Veritabanı ayarları için aşağıdaki bilgileri belirtin:</span><span class="sxs-lookup"><span data-stu-id="708eb-279">Specify the following information for the database settings:</span></span>

    - <span data-ttu-id="708eb-280">İçinde **adı** metin kutusunda, bir veritabanı adı girin (örneğin *geekquiz\_db*)</span><span class="sxs-lookup"><span data-stu-id="708eb-280">In the **Name** text box, enter a database name (e.g. *geekquiz\_db*)</span></span>
    - <span data-ttu-id="708eb-281">Sunucudaki **açılan** listesinde **yeni SQL veritabanı sunucusuna**.</span><span class="sxs-lookup"><span data-stu-id="708eb-281">In the Server **drop-down** list, select **New SQL database server**.</span></span> <span data-ttu-id="708eb-282">Alternatif olarak, var olan bir sunucu seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="708eb-282">Alternatively, you can select an existing server.</span></span>
    - <span data-ttu-id="708eb-283">İçinde **veritabanı kullanıcı adı** ve **veritabanı parolasını** kutularına için SQL veritabanı sunucusu yönetici kullanıcı adı ve parola girin.</span><span class="sxs-lookup"><span data-stu-id="708eb-283">In the **Database username** and **Database password** boxes, enter the administrator username and password for the SQL database server.</span></span> <span data-ttu-id="708eb-284">Bir sunucu seçerseniz oluşturduysanız, parola girmesi istenir.</span><span class="sxs-lookup"><span data-stu-id="708eb-284">If you select a server you have already created, you will be prompted for the password.</span></span>

    ![Veritabanı ayarlarını belirtme](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

    <span data-ttu-id="708eb-286">*Veritabanı ayarlarını belirtme*</span><span class="sxs-lookup"><span data-stu-id="708eb-286">*Specifying the database settings*</span></span>
6. <span data-ttu-id="708eb-287">Devam etmek için **İleri** 'ye tıklayın.</span><span class="sxs-lookup"><span data-stu-id="708eb-287">Click **Next** to continue.</span></span>
7. <span data-ttu-id="708eb-288">Seçin **yerel Git deposu** kullanın ve'ı tıklatın Kaynak denetimi için **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="708eb-288">Select **Local Git repository** for the source control to use and click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-289">Dağıtım kimlik bilgileri (kullanıcı adı ve parola) istenebilir.</span><span class="sxs-lookup"><span data-stu-id="708eb-289">You may be prompted for the deployment credentials (a username and password).</span></span>

    ![Git deposu oluşturuluyor](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    <span data-ttu-id="708eb-291">*Git deposu oluşturuluyor*</span><span class="sxs-lookup"><span data-stu-id="708eb-291">*Creating the Git repository*</span></span>
8. <span data-ttu-id="708eb-292">Yeni web uygulaması oluşturulana kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="708eb-292">Wait until the new web app is created.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-293">Varsayılan olarak, etki alanları adresindeki Azure sağlar *azurewebsites.net* ancak aynı zamanda, Azure Yönetim Portalı'nı kullanarak özel etki alanlarını ayarlamak için olanağını sağlar.</span><span class="sxs-lookup"><span data-stu-id="708eb-293">By default, Azure provides domains at *azurewebsites.net* but also gives you the possibility to set custom domains using the Azure management portal.</span></span> <span data-ttu-id="708eb-294">Ancak, belirli Azure App Service modları kullanıyorsanız, yalnızca özel etki alanlarını yönetebilir.</span><span class="sxs-lookup"><span data-stu-id="708eb-294">However, you can only manage custom domains if you are using certain Azure App Service modes.</span></span>
    > 
    > <span data-ttu-id="708eb-295">Azure uygulama hizmeti ücretsiz, paylaşılan, temel, standart ve Premium sürümlerinde kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="708eb-295">Azure App Service is available in Free, Shared, Basic, Standard, and Premium editions.</span></span> <span data-ttu-id="708eb-296">Ücretsiz ve paylaşılan modda, tüm web uygulamaları çok kiracılı bir ortamda çalıştırılabilir ve CPU, bellek ve ağ kullanımı için kotaları sahip.</span><span class="sxs-lookup"><span data-stu-id="708eb-296">In Free and Shared mode, all web apps run in a multi-tenant environment and have quotas for CPU, Memory, and Network usage.</span></span> <span data-ttu-id="708eb-297">Ücretsiz uygulamalar en fazla planınızla farklılık gösterebilir.</span><span class="sxs-lookup"><span data-stu-id="708eb-297">The maximum number of free apps may vary with your plan.</span></span> <span data-ttu-id="708eb-298">Standart modda, standart Azure karşılık ayrılmış sanal makinelerde çalıştırma hangi uygulamaların işlem kaynaklarını seçin.</span><span class="sxs-lookup"><span data-stu-id="708eb-298">In Standard mode, you choose which apps run on dedicated virtual machines that correspond to the standard Azure compute resources.</span></span> <span data-ttu-id="708eb-299">Web uygulama modu yapılandırmasında bulabilirsiniz **ölçek** , web uygulamanızın menüsü.</span><span class="sxs-lookup"><span data-stu-id="708eb-299">You can find the web app mode configuration in the **Scale** menu of your web app.</span></span>
    > 
    > <span data-ttu-id="708eb-300">![Azure uygulama hizmeti modları](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure uygulama hizmeti modları")</span><span class="sxs-lookup"><span data-stu-id="708eb-300">![Azure App Service Modes](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "Azure App Service Modes")</span></span>
    > 
    > <span data-ttu-id="708eb-301">Kullanıyorsanız **paylaşılan** veya **standart** modu, görebilirsiniz, uygulamanızın giderek web uygulamanız için özel etki alanlarını yönetmek **yapılandırma** menü ve tıklayarak**Etki alanlarını yönetme** altında *etki alanı adları*.</span><span class="sxs-lookup"><span data-stu-id="708eb-301">If you are using **Shared** or **Standard** mode, you will be able to manage custom domains for your web app by going to your app's **Configure** menu and clicking **Manage Domains** under *domain names*.</span></span>
    > 
    > <span data-ttu-id="708eb-302">![Etki alanlarını yönetme](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "etki alanlarını yönetme")</span><span class="sxs-lookup"><span data-stu-id="708eb-302">![Manage Domains](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "Manage Domains")</span></span>
    > 
    > <span data-ttu-id="708eb-303">![Özel etki alanlarını yönetme](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "özel etki alanlarını yönetme")</span><span class="sxs-lookup"><span data-stu-id="708eb-303">![Manage Custom Domains](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "Manage Custom Domains")</span></span>
9. <span data-ttu-id="708eb-304">Web uygulaması oluşturulduktan sonra altında bağlantıyı tıklatın **URL** sütun yeni web uygulaması çalışıp çalışmadığını denetleyin.</span><span class="sxs-lookup"><span data-stu-id="708eb-304">Once the web app is created, click the link under the **URL** column to check that the new web app is running.</span></span>

    ![Yeni web uygulamasına göz atma](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    <span data-ttu-id="708eb-306">*Yeni web uygulamasına göz atma*</span><span class="sxs-lookup"><span data-stu-id="708eb-306">*Browsing to the new web app*</span></span>

    ![çalışan web uygulaması](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    <span data-ttu-id="708eb-308">*çalışan web uygulaması*</span><span class="sxs-lookup"><span data-stu-id="708eb-308">*web app running*</span></span>

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a><span data-ttu-id="708eb-309">Görev 2 – üretim SQL veritabanı oluşturuluyor</span><span class="sxs-lookup"><span data-stu-id="708eb-309">Task 2 – Creating the Production SQL Database</span></span>

<span data-ttu-id="708eb-310">Bu görevde, kullanacağınız **Entity Framework Code First Migrations** hedefleme veritabanı oluşturmak için **Azure SQL veritabanı** önceki görevde oluşturduğunuz örneği.</span><span class="sxs-lookup"><span data-stu-id="708eb-310">In this task, you will use the **Entity Framework Code First Migrations** to create the database targeting the **Azure SQL Database** instance you created in the previous task.</span></span>

1. <span data-ttu-id="708eb-311">Yönetim Portalı'nda önceki görevde oluşturduğunuz web uygulamasına gidin ve Git kendi **Pano**.</span><span class="sxs-lookup"><span data-stu-id="708eb-311">In the Management Portal, navigate to the web app you created in the previous task and go to its **Dashboard**.</span></span>
2. <span data-ttu-id="708eb-312">İçinde **Pano** sayfasında, **bağlantı dizelerini görüntüle** altında bağlantı **Hızlı Bakış** bölümü.</span><span class="sxs-lookup"><span data-stu-id="708eb-312">In the **Dashboard** page, click **View connection strings** link under the **quick glance** section.</span></span>

    <span data-ttu-id="708eb-313">![Bağlantı dizelerini görüntüle](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "bağlantı dizelerini görüntüle")</span><span class="sxs-lookup"><span data-stu-id="708eb-313">![View connection strings](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "View connection strings")</span></span>

    <span data-ttu-id="708eb-314">*Bağlantı dizelerini görüntüle*</span><span class="sxs-lookup"><span data-stu-id="708eb-314">*View connection strings*</span></span>
3. <span data-ttu-id="708eb-315">Kopya **bağlantı dizesi** değer ve iletişim kutusunu kapatın.</span><span class="sxs-lookup"><span data-stu-id="708eb-315">Copy the **connection string** value and close the dialog box.</span></span>

    <span data-ttu-id="708eb-316">![Azure Yönetim Portalı'nda bağlantı dizesi](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Azure Yönetim Portalı'nda bağlantı dizesi")</span><span class="sxs-lookup"><span data-stu-id="708eb-316">![Connection String in Azure Management Portal](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "Connection String in Azure Management Portal")</span></span>

    <span data-ttu-id="708eb-317">*Azure Yönetim Portalı'nda bağlantı dizesi*</span><span class="sxs-lookup"><span data-stu-id="708eb-317">*Connection String in Azure Management Portal*</span></span>
4. <span data-ttu-id="708eb-318">Tıklatın **SQL veritabanları** Azure SQL veritabanlarının listesini görmek için</span><span class="sxs-lookup"><span data-stu-id="708eb-318">Click **SQL Databases** to see the list of SQL databases in Azure</span></span>

    <span data-ttu-id="708eb-319">![SQL veritabanı menü](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL veritabanı menüsü")</span><span class="sxs-lookup"><span data-stu-id="708eb-319">![SQL Database menu](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "SQL Database menu")</span></span>

    <span data-ttu-id="708eb-320">*SQL veritabanı menüsü*</span><span class="sxs-lookup"><span data-stu-id="708eb-320">*SQL Database menu*</span></span>
5. <span data-ttu-id="708eb-321">Önceki görevde oluşturduğunuz veritabanını bulun ve sunucuda tıklatın.</span><span class="sxs-lookup"><span data-stu-id="708eb-321">Locate the database you created in the previous task and click on the Server.</span></span>

    <span data-ttu-id="708eb-322">![SQL veritabanı sunucusu](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL veritabanı sunucusu")</span><span class="sxs-lookup"><span data-stu-id="708eb-322">![SQL Database server](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "SQL Database server")</span></span>

    <span data-ttu-id="708eb-323">*SQL veritabanı sunucusu*</span><span class="sxs-lookup"><span data-stu-id="708eb-323">*SQL Database server*</span></span>
6. <span data-ttu-id="708eb-324">İçinde **Hızlı Başlangıç** sayfa sunucusunun tıklayın **yapılandırma**.</span><span class="sxs-lookup"><span data-stu-id="708eb-324">In the **Quick Start** page of the server, click on **Configure**.</span></span>

    <span data-ttu-id="708eb-325">![Yapılandırma menü](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "yapılandırma menüsü")</span><span class="sxs-lookup"><span data-stu-id="708eb-325">![Configure menu](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "Configure menu")</span></span>

    <span data-ttu-id="708eb-326">*Menü yapılandırın*</span><span class="sxs-lookup"><span data-stu-id="708eb-326">*Configure menu*</span></span>
7. <span data-ttu-id="708eb-327">İçinde **izin verilen IP adresleri** bölümünde, tıklayın **eklemek için izin verilen IP adreslerini** SQL veritabanı sunucusuna bağlanmak IP etkinleştirmek için bağlantı.</span><span class="sxs-lookup"><span data-stu-id="708eb-327">In the **Allowed IP addresses** section, click on **Add to the allowed IP addresses** link to enable your IP to connect to the SQL Database server.</span></span>

    <span data-ttu-id="708eb-328">![İzin verilen IP adreslerini](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "izin verilen IP adresleri")</span><span class="sxs-lookup"><span data-stu-id="708eb-328">![Allowed IP addresses](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "Allowed IP addresses")</span></span>

    <span data-ttu-id="708eb-329">*İzin verilen IP adresi*</span><span class="sxs-lookup"><span data-stu-id="708eb-329">*Allowed IP addresses*</span></span>
8. <span data-ttu-id="708eb-330">Tıklatın **kaydetmek** adımı tamamlamak için sayfanın altındaki.</span><span class="sxs-lookup"><span data-stu-id="708eb-330">Click **Save** at the bottom of the page to complete the step.</span></span>
9. <span data-ttu-id="708eb-331">Visual Studio'ya geri çevirin.</span><span class="sxs-lookup"><span data-stu-id="708eb-331">Switch back to Visual Studio.</span></span>
10. <span data-ttu-id="708eb-332">İçinde **Paket Yöneticisi Konsolu**, komutu aşağıdaki değiştirilirken yürütme *[YOUR bağlantı DİZESİ]* Azure'dan kopyaladığınız bağlantı dizesiyle yer tutucusu</span><span class="sxs-lookup"><span data-stu-id="708eb-332">In the **Package Manager Console**, execute the following command replacing *[YOUR-CONNECTION-STRING]* placeholder with the connection string you copied from Azure</span></span>

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    <span data-ttu-id="708eb-333">![Windows Azure SQL veritabanı hedefleme güncelleştirme veritabanı](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "güncelleştirme veritabanı Windows Azure SQL veritabanı hedefleme")</span><span class="sxs-lookup"><span data-stu-id="708eb-333">![Update database targeting Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Update database targeting Windows Azure SQL Database")</span></span>

    <span data-ttu-id="708eb-334">*Azure SQL veritabanı hedefleme veritabanını güncelleştirme*</span><span class="sxs-lookup"><span data-stu-id="708eb-334">*Update database targeting Azure SQL Database*</span></span>

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a><span data-ttu-id="708eb-335">Görev 3 – dağıtma günlük test için Git kullanarak hazırlama</span><span class="sxs-lookup"><span data-stu-id="708eb-335">Task 3 – Deploying Geek Quiz to Staging Using Git</span></span>

<span data-ttu-id="708eb-336">Bu görevde, web uygulamanızı hazırlanmış yayımlamayı etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="708eb-336">In this task, you will enable staged publishing in your web app.</span></span> <span data-ttu-id="708eb-337">Ardından, web uygulamanızı hazırlama ortamını, doğrudan yerel bilgisayarınızdan günlük test uygulamayı yayımlamak için Git kullanacaktır.</span><span class="sxs-lookup"><span data-stu-id="708eb-337">Then, you will use Git to publish the Geek Quiz application directly from your local computer to the staging environment of your web app.</span></span>

1. <span data-ttu-id="708eb-338">Portalına geri dönün ve altında web uygulamasının adını tıklatın **adı** yönetim sayfaları görüntülemek için sütun.</span><span class="sxs-lookup"><span data-stu-id="708eb-338">Go back to the portal and click the name of the web app under the **Name** column to display the management pages.</span></span>

    ![Web uygulaması yönetim sayfalarını açma](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    <span data-ttu-id="708eb-340">*Web uygulaması yönetim sayfalarını açma*</span><span class="sxs-lookup"><span data-stu-id="708eb-340">*Opening the web app management pages*</span></span>
2. <span data-ttu-id="708eb-341">Gidin **ölçek** sayfası.</span><span class="sxs-lookup"><span data-stu-id="708eb-341">Navigate to the **Scale** page.</span></span> <span data-ttu-id="708eb-342">Altında **genel** bölümünde, select **standart** tıklatın ve yapılandırma için **kaydetmek** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="708eb-342">Under the **general** section, select **Standard** for the configuration and click **Save** in the command bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-343">Abonelikte ve geçerli bölge içindeki tüm web uygulamalarının çalıştırılması için **standart** modu, bırakın **Tümünü Seç** onay kutusu seçili **seçin siteleri** yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="708eb-343">To run all web apps in the current region and subscription in **Standard** mode, leave the **Select All** check box selected in the **Choose Sites** configuration.</span></span> <span data-ttu-id="708eb-344">Aksi takdirde temizleyin **Tümünü Seç** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="708eb-344">Otherwise, clear the **Select All** check box.</span></span>

    <span data-ttu-id="708eb-345">![Web uygulaması Standart modu yükseltme](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "standart moda web uygulamasını yükseltme")</span><span class="sxs-lookup"><span data-stu-id="708eb-345">![Upgrading the web app to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "Upgrading the web app to Standard mode")</span></span>

    <span data-ttu-id="708eb-346">*Web uygulaması Standart modu yükseltme*</span><span class="sxs-lookup"><span data-stu-id="708eb-346">*Upgrading the Web App to Standard mode*</span></span>
3. <span data-ttu-id="708eb-347">Tıklatın **Evet** değişiklikleri onaylamak için.</span><span class="sxs-lookup"><span data-stu-id="708eb-347">Click **Yes** to confirm the changes.</span></span>

    <span data-ttu-id="708eb-348">![Standart mod değişikliği onayladığınız](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "web uygulaması modunu değiştirme ile devam etmeden")</span><span class="sxs-lookup"><span data-stu-id="708eb-348">![Confirming the change to Standard mode](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "Continuing with the changing of the web app mode")</span></span>

    <span data-ttu-id="708eb-349">*Standart mod değişikliği onaylama*</span><span class="sxs-lookup"><span data-stu-id="708eb-349">*Confirming the change to Standard mode*</span></span>
4. <span data-ttu-id="708eb-350">Git **Pano** sayfasında ve tıklayın **etkinleştirme yayımlamayı hazırlanan** altında **Hızlı Bakış** bölümü.</span><span class="sxs-lookup"><span data-stu-id="708eb-350">Go to the **Dashboard** page and click **Enable staged publishing** under the **quick glance** section.</span></span>

    <span data-ttu-id="708eb-351">![Aşamalı yayımlamayı etkinleştirmeye](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "etkinleştirme yayımlamayı hazırlanan")</span><span class="sxs-lookup"><span data-stu-id="708eb-351">![Enabling staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "Enabling staged publishing")</span></span>

    <span data-ttu-id="708eb-352">*Hazırlanmış yayımlamayı etkinleştirme*</span><span class="sxs-lookup"><span data-stu-id="708eb-352">*Enabling staged publishing*</span></span>
5. <span data-ttu-id="708eb-353">Tıklatın **Evet** hazırlanmış yayımlamayı etkinleştirmek için.</span><span class="sxs-lookup"><span data-stu-id="708eb-353">Click **Yes** to enable staged publishing.</span></span>

    <span data-ttu-id="708eb-354">![Onaylama hazırlanmış yayımlamayı](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "hazırlanmış yayımlamayı etkinleştirmek için Evet'i tıklatmak")</span><span class="sxs-lookup"><span data-stu-id="708eb-354">![Confirming staged publishing](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "Clicking Yes to enable staged publishing")</span></span>

    <span data-ttu-id="708eb-355">*Hazırlanmış yayımlamayı onaylama*</span><span class="sxs-lookup"><span data-stu-id="708eb-355">*Confirming staged publishing*</span></span>
6. <span data-ttu-id="708eb-356">Web uygulamaları listesinde işareti hazırlama yuvasıyla görüntülemek için web uygulaması adı soluna genişletin.</span><span class="sxs-lookup"><span data-stu-id="708eb-356">In the list of web apps, expand the mark to the left of your web app name to display the staging site slot.</span></span> <span data-ttu-id="708eb-357">Ardından, web uygulamanızın adına sahip ***(hazırlama)***.</span><span class="sxs-lookup"><span data-stu-id="708eb-357">It has the name of your web app followed by ***(staging)***.</span></span> <span data-ttu-id="708eb-358">Yönetim sayfasına gitmek için hazırlama sitesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="708eb-358">Click the staging site to go to the management page.</span></span>

    <span data-ttu-id="708eb-359">![Hazırlama web uygulaması'na giderek](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "hazırlama web uygulamasına gidin")</span><span class="sxs-lookup"><span data-stu-id="708eb-359">![Navigating to the staging web app](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "Navigating to the staging web app")</span></span>

    <span data-ttu-id="708eb-360">*Hazırlama uygulamasına gidin*</span><span class="sxs-lookup"><span data-stu-id="708eb-360">*Navigating to the staging app*</span></span>
7. <span data-ttu-id="708eb-361">Herhangi diğer web uygulamanızın Yönetim sayfası gibi he Yönetim sayfasında görünür dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="708eb-361">Notice that he management page looks like any other web app's management page.</span></span> <span data-ttu-id="708eb-362">Gidin **dağıtımları** sayfası ve kopyalama **Git URL'si** değeri.</span><span class="sxs-lookup"><span data-stu-id="708eb-362">Navigate to the **Deployments** page and copy the **Git URL** value.</span></span> <span data-ttu-id="708eb-363">Bu alıştırmada, onu kullanır.</span><span class="sxs-lookup"><span data-stu-id="708eb-363">You will use it later in this exercise.</span></span>

    ![Git URL değeri kopyalama](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    <span data-ttu-id="708eb-365">*Git URL değeri kopyalama*</span><span class="sxs-lookup"><span data-stu-id="708eb-365">*Copying the Git URL value*</span></span>
8. <span data-ttu-id="708eb-366">Yeni bir **Git Bash** konsol ve aşağıdaki komutları yürütün.</span><span class="sxs-lookup"><span data-stu-id="708eb-366">Open a new **Git Bash** console and execute the following commands.</span></span> <span data-ttu-id="708eb-367">Güncelleştirme *[YOUR uygulama yolu]* yolu ile yer tutucu **GeekQuiz** bulunan Çözüm **Source\Ex1 DeployingWebSiteToStaging\Begin** klasörü Bu laboratuvar.</span><span class="sxs-lookup"><span data-stu-id="708eb-367">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution, located in the **Source\Ex1-DeployingWebSiteToStaging\Begin** folder of this lab.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Git başlatma ve ilk tamamlama](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    <span data-ttu-id="708eb-369">*Git başlatma ve ilk tamamlama*</span><span class="sxs-lookup"><span data-stu-id="708eb-369">*Git initialization and first commit*</span></span>
9. <span data-ttu-id="708eb-370">Web uygulamanız için uzaktan göndermek için aşağıdaki komutu çalıştırın **Git** deposu.</span><span class="sxs-lookup"><span data-stu-id="708eb-370">Run the following command to push your web app to the remote **Git** repository.</span></span> <span data-ttu-id="708eb-371">Yer tutucu Yönetim Portalı'ndan alınan URL ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="708eb-371">Replace the placeholder with the URL you obtained from the management portal.</span></span> <span data-ttu-id="708eb-372">Dağıtım parolanızı girmeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="708eb-372">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Windows Azure iletme](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    <span data-ttu-id="708eb-374">*Azure'a dağıtmaya*</span><span class="sxs-lookup"><span data-stu-id="708eb-374">*Pushing to Azure*</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-375">FTP ana bilgisayarına veya bir web uygulaması GIT deposuna içerik dağıttığınızda, kullanarak kimliğini doğrulaması gerekir **dağıtım kimlik bilgileri** web uygulamasından 's oluşturduğunuz **Hızlı Başlangıç** veya **Panosu**  yönetim sayfaları.</span><span class="sxs-lookup"><span data-stu-id="708eb-375">When you deploy content to the FTP host or GIT repository of a web app, you must authenticate using the **deployment credentials** that you created from the web app's **Quick Start** or **Dashboard** management pages.</span></span> <span data-ttu-id="708eb-376">Dağıtım kimlik bilgilerinizi bilmiyorsanız, bunları Yönetim Portalı'nı kullanarak kolayca sıfırlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="708eb-376">If you do not know your deployment credentials you can easily reset them using the management portal.</span></span> <span data-ttu-id="708eb-377">Web uygulamasını açın **Pano** sayfasında ve tıklayın **dağıtım kimlik bilgilerinizi sıfırlayın** bağlantı.</span><span class="sxs-lookup"><span data-stu-id="708eb-377">Open the web app **Dashboard** page and click the **Reset your deployment credentials** link.</span></span> <span data-ttu-id="708eb-378">Yeni bir parola sağlayın ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="708eb-378">Provide a new password and click **OK**.</span></span> <span data-ttu-id="708eb-379">Dağıtım kimlik bilgileri, aboneliğinizle ilişkilendirilmiş tüm web uygulamaları ile kullanım için geçerli değil.</span><span class="sxs-lookup"><span data-stu-id="708eb-379">Deployment credentials are valid for use with all web apps associated with your subscription.</span></span>
10. <span data-ttu-id="708eb-380">Web uygulaması için Azure başarıyla gönderilen doğrulamak için Yönetim Portalı'na geri dönün ve **Web siteleri**.</span><span class="sxs-lookup"><span data-stu-id="708eb-380">In order to verify the web app was successfully pushed to Azure, go back to the management portal and click **Websites**.</span></span>
11. <span data-ttu-id="708eb-381">Web uygulamanızı bulun ve hazırlama yuvasıyla görüntülemek için giriş genişletin.</span><span class="sxs-lookup"><span data-stu-id="708eb-381">Locate your web app and expand the entry to display the staging site slot.</span></span> <span data-ttu-id="708eb-382">' I tıklatın, **adı** yönetimi sayfasına gidin.</span><span class="sxs-lookup"><span data-stu-id="708eb-382">Click its **Name** to go to the management page.</span></span>
12. <span data-ttu-id="708eb-383">Tıklatın **dağıtımları** görmek için **dağıtım geçmişi**.</span><span class="sxs-lookup"><span data-stu-id="708eb-383">Click **Deployments** to see the **deployment history**.</span></span> <span data-ttu-id="708eb-384">Olduğunu doğrulayın bir **etkin dağıtım** ile  *&quot;ilk yürütme&quot;*.</span><span class="sxs-lookup"><span data-stu-id="708eb-384">Verify that there is an **Active Deployment** with your *&quot;Initial Commit&quot;*.</span></span>

    ![Etkin dağıtım](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    <span data-ttu-id="708eb-386">*Etkin dağıtım*</span><span class="sxs-lookup"><span data-stu-id="708eb-386">*Active deployment*</span></span>
13. <span data-ttu-id="708eb-387">Son olarak, tıklatın **Gözat** komut çubuğunda web uygulamasına gidin.</span><span class="sxs-lookup"><span data-stu-id="708eb-387">Finally, click **Browse** in the command bar to go to the web app.</span></span>

    ![Web uygulaması Gözat](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    <span data-ttu-id="708eb-389">*Web uygulaması Gözat*</span><span class="sxs-lookup"><span data-stu-id="708eb-389">*Browse web app*</span></span>
14. <span data-ttu-id="708eb-390">Uygulama başarıyla dağıtılır günlük test oturum açma sayfasını görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="708eb-390">If the application is successfully deployed, you will see the Geek Quiz login page.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-391">Dağıtılmış uygulamanın adresi URL'sini ve ardından web uygulamanızı adını içeren *-hazırlama*.</span><span class="sxs-lookup"><span data-stu-id="708eb-391">The address URL of the deployed application contains the name of your web app followed by *-staging*.</span></span>

    ![Hazırlama ortamında çalışan uygulama](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    <span data-ttu-id="708eb-393">*Hazırlama ortamında çalışan uygulama*</span><span class="sxs-lookup"><span data-stu-id="708eb-393">*Application running in the staging environment*</span></span>
15. <span data-ttu-id="708eb-394">Uygulama keşfetmek isterseniz tıklayın **kaydetmek** yeni bir kullanıcı kaydetmek için.</span><span class="sxs-lookup"><span data-stu-id="708eb-394">If you wish to explore the application, click **Register** to register a new user.</span></span> <span data-ttu-id="708eb-395">Hesap ayrıntıları, bir kullanıcı adı, e-posta adresi ve parola girerek tamamlayın.</span><span class="sxs-lookup"><span data-stu-id="708eb-395">Complete the account details by entering a user name, email address and password.</span></span> <span data-ttu-id="708eb-396">Ardından, uygulamayı test ilk soru gösterir.</span><span class="sxs-lookup"><span data-stu-id="708eb-396">Next, the application shows the first question of the quiz.</span></span> <span data-ttu-id="708eb-397">Beklendiği gibi çalıştığından emin olmak için birkaç soruları yanıtlayın.</span><span class="sxs-lookup"><span data-stu-id="708eb-397">Answer a few questions to make sure it is working as expected.</span></span>

    ![Uygulama kullanılmaya hazır](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    <span data-ttu-id="708eb-399">*Uygulama kullanılmaya hazır*</span><span class="sxs-lookup"><span data-stu-id="708eb-399">*Application ready to be used*</span></span>

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a><span data-ttu-id="708eb-400">Görev 4 – üretim Web uygulamasına yükseltme</span><span class="sxs-lookup"><span data-stu-id="708eb-400">Task 4 – Promoting the Web App to Production</span></span>

<span data-ttu-id="708eb-401">Web uygulaması hazırlama ortamında düzgün çalıştığını doğruladıktan, üretime yükseltmek hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="708eb-401">Now that you have verified that the web app is working correctly in the staging environment, you are ready to promote it to production.</span></span> <span data-ttu-id="708eb-402">Bu görevde, üretim yuvasıyla ile hazırlama site yuvası takas.</span><span class="sxs-lookup"><span data-stu-id="708eb-402">In this task, you will swap the staging site slot with the production site slot.</span></span>

1. <span data-ttu-id="708eb-403">Yönetim Portalı'na geri dönün ve hazırlama site yuvası seçin.</span><span class="sxs-lookup"><span data-stu-id="708eb-403">Go back to the management portal and select the staging site slot.</span></span> <span data-ttu-id="708eb-404">Tıklatın **takas** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="708eb-404">Click **Swap** in the command bar.</span></span>

    ![Üretim için değiştirme](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    <span data-ttu-id="708eb-406">*Üretim için değiştirme*</span><span class="sxs-lookup"><span data-stu-id="708eb-406">*Swap to production*</span></span>
2. <span data-ttu-id="708eb-407">Tıklatın **Evet** takas işlemine devam etmek için onay iletişim kutusunda.</span><span class="sxs-lookup"><span data-stu-id="708eb-407">Click **Yes** in the confirmation dialog box to proceed with the swap operation.</span></span> <span data-ttu-id="708eb-408">Azure hemen hazırlama site içeriğini üretim sitesini içerikle değiştireceksiniz.</span><span class="sxs-lookup"><span data-stu-id="708eb-408">Azure will immediately swap the content of the production site with the content of the staging site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-409">Bazı ayarlar hazırlanmış sürümünden (örn. bağlantı dizesi geçersiz kılmalar, işleyici eşlemeleri, vb.) üretim sürümü için otomatik olarak kopyalanır, ancak diğer ayarları değiştirmez (örn. DNS uç noktaları, SSL bağlamaları, vb.).</span><span class="sxs-lookup"><span data-stu-id="708eb-409">Some settings from the staged version will automatically be copied to the production version (e.g. connection string overrides, handler mappings, etc.) but other settings will not change (e.g. DNS endpoints, SSL bindings, etc.).</span></span>

    ![Değiştirme işlemi onaylama](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    <span data-ttu-id="708eb-411">*Değiştirme işlemi onaylama*</span><span class="sxs-lookup"><span data-stu-id="708eb-411">*Confirming swap operation*</span></span>
3. <span data-ttu-id="708eb-412">Değiştirme işlemi tamamlandıktan sonra üretim yuvasına seçin ve tıklatın **Gözat** üretim sitesini açmak için komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="708eb-412">Once the swap is complete, select the production slot and click **Browse** in the command bar to open the production site.</span></span> <span data-ttu-id="708eb-413">Adres çubuğundaki URL'yi dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="708eb-413">Notice the URL in the address bar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-414">Önbelleği temizlemek için tarayıcınızı yenilemeniz gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="708eb-414">You might need to refresh your browser to clear cache.</span></span> <span data-ttu-id="708eb-415">Internet Explorer'da tuşlarına basarak bunu yapabilirsiniz **CTRL + R**.</span><span class="sxs-lookup"><span data-stu-id="708eb-415">In Internet Explorer, you can do this by pressing **CTRL+R**.</span></span>

    ![Üretim ortamında çalışan web uygulaması](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. <span data-ttu-id="708eb-417">İçinde **Gitbash'i** konsol, üretim yuvasına hedeflemek için yerel Git deposu uzak URL güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="708eb-417">In the **GitBash** console, update the remote URL for the local Git repository to target the production slot.</span></span> <span data-ttu-id="708eb-418">Bunu yapmak için yer tutucuları dağıtım kullanıcı adınızı ve web uygulamanızın adıyla değiştirerek aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="708eb-418">To do this, run the following command replacing the placeholders with your deployment username and the name of your web app.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-419">Aşağıdaki alıştırmalarda yalnızca Laboratuvar Basitlik için hazırlama yerine üretim siteye değişiklikleri gönderir.</span><span class="sxs-lookup"><span data-stu-id="708eb-419">In the following exercises, you will push changes to the production site instead of staging just for the simplicity of the lab.</span></span> <span data-ttu-id="708eb-420">Gerçek dünya senaryoda üretime yükseltmeden önce hazırlama ortamında değişiklikleri doğrulamak için önerilir.</span><span class="sxs-lookup"><span data-stu-id="708eb-420">In a real-world scenario, it is recommended to verify the changes in the staging environment before promoting to production.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a><span data-ttu-id="708eb-421">Alıştırma 3: Dağıtımı geri alma üretimde gerçekleştirme</span><span class="sxs-lookup"><span data-stu-id="708eb-421">Exercise 3: Performing Deployment Rollback in Production</span></span>

<span data-ttu-id="708eb-422">Burada sahip olmadığınız arasında hazırlama ve üretim, örneğin, sık kullanılan takas gerçekleştirmek için bir hazırlama yuvası ile çalışıyorsanız senaryolarda **serbest** veya **paylaşılan** modu.</span><span class="sxs-lookup"><span data-stu-id="708eb-422">There are scenarios where you do not have a staging slot to perform hot swap between staging and production, for example, if you are working with **Free** or **Shared** mode.</span></span> <span data-ttu-id="708eb-423">Bu senaryolarda, uygulamanızı bir sınama ortamında – yerel veya uzak bir sitedeki – üretime dağıtılmadan önce test etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="708eb-423">In those scenarios, you should test your application in a testing environment –either locally or in a remote site– before deploying to production.</span></span> <span data-ttu-id="708eb-424">Ancak, test aşaması sırasında algılanmadı sorunu üretim sitede kaynaklanabilecek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="708eb-424">However, it is possible that an issue not detected during the testing phase may arise in the production site.</span></span> <span data-ttu-id="708eb-425">Bu durumda, mümkün olan en kısa sürede uygulamanın önceki ve daha kararlı sürümünü arasında kolayca geçiş için bir mekanizma olması önemlidir.</span><span class="sxs-lookup"><span data-stu-id="708eb-425">In this case, it is important to have a mechanism to easily switch to a previous and more stable version of the application as quickly as possible.</span></span>

<span data-ttu-id="708eb-426">İçinde **Azure App Service**, kaynak denetiminden sürekli dağıtım yapar bu olası teşekkür **dağıtmanız** eylemi Yönetim Portalı'nda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="708eb-426">In **Azure App Service**, continuous deployment from source control makes this possible thanks to the **redeploy** action available in the management portal.</span></span> <span data-ttu-id="708eb-427">Azure depoya gönderilen işlemeleri ile ilişkilendirilen dağıtımları izler ve herhangi bir önceki dağıtımlarınızı herhangi bir zamanda kullanarak uygulamanızı yeniden dağıtmak için bir seçenek sağlar.</span><span class="sxs-lookup"><span data-stu-id="708eb-427">Azure keeps track of the deployments associated with the commits pushed to the repository and provides an option to redeploy your application using any of your previous deployments, at any time.</span></span>

<span data-ttu-id="708eb-428">Bu alıştırmada kodda değişiklik gerçekleştirecek **günlük test** bilerek yerleştirir uygulama bir *hata*.</span><span class="sxs-lookup"><span data-stu-id="708eb-428">In this exercise you will perform a change to the code in the **Geek Quiz** application that intentionally injects a *bug*.</span></span> <span data-ttu-id="708eb-429">Hatayı görmek için üretim uygulamasına dağıtır ve ardından önceki bir duruma geri dönmek için yeniden dağıtın özelliğinin avantajlarından yararlanmak.</span><span class="sxs-lookup"><span data-stu-id="708eb-429">You will deploy the application to production to see the error, and then you will take advantage of the redeploy feature to go back to the previous state.</span></span>

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a><span data-ttu-id="708eb-430">Görev 1 – günlük test uygulaması güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="708eb-430">Task 1 – Updating the Geek Quiz Application</span></span>

<span data-ttu-id="708eb-431">Bu görevde kodunu küçük bir parçasını yeniden düzenlemeniz **TriviaController** seçili test seçeneği veritabanından yeni bir yöntemi alır mantığının parçası ayıklamak için sınıf.</span><span class="sxs-lookup"><span data-stu-id="708eb-431">In this task, you will refactor a small piece of code of the **TriviaController** class to extract part of the logic that retrieves the selected quiz option from the database into a new method.</span></span>

1. <span data-ttu-id="708eb-432">Visual Studio örneğiyle geçin **GeekQuiz** önceki alıştırmada çözümden.</span><span class="sxs-lookup"><span data-stu-id="708eb-432">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="708eb-433">İçinde **Çözüm Gezgini**, açık **TriviaController.cs** içinde dosya **denetleyicileri** klasör.</span><span class="sxs-lookup"><span data-stu-id="708eb-433">In **Solution Explorer**, open the **TriviaController.cs** file inside the **Controllers** folder.</span></span>
3. <span data-ttu-id="708eb-434">Bulun **StoreAsync** yöntemi ve kod aşağıdaki çizimde vurgulanan seçin.</span><span class="sxs-lookup"><span data-stu-id="708eb-434">Locate the **StoreAsync** method and select the code highlighted in the following figure.</span></span>

    ![Kod seçme](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    <span data-ttu-id="708eb-436">*Kod seçme*</span><span class="sxs-lookup"><span data-stu-id="708eb-436">*Selecting the code*</span></span>
4. <span data-ttu-id="708eb-437">Seçili kodu sağ tıklayın, genişletin **yeniden düzenlemeniz** menü ve select **ayıklama yöntemi...** .</span><span class="sxs-lookup"><span data-stu-id="708eb-437">Right-click the selected code, expand the **Refactor** menu and select **Extract Method...**.</span></span>

    ![Yeni bir yöntem olarak kodu ayıklanıyor](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    <span data-ttu-id="708eb-439">*Ayıklama yöntemi seçme*</span><span class="sxs-lookup"><span data-stu-id="708eb-439">*Selecting Extract Method*</span></span>
5. <span data-ttu-id="708eb-440">İçinde **ayıklama yöntemi** iletişim kutusunda, yeni yöntemin adı *MatchesOption* tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="708eb-440">In the **Extract Method** dialog box, name the new method *MatchesOption* and click **OK**.</span></span>

    ![Yöntem adı belirtme](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    <span data-ttu-id="708eb-442">*Ayıklanan yöntemin adını belirtme*</span><span class="sxs-lookup"><span data-stu-id="708eb-442">*Specifying the name for the extracted method*</span></span>
6. <span data-ttu-id="708eb-443">Seçili kod içine ayıklanır **MatchesOption** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="708eb-443">The selected code is then extracted into the **MatchesOption** method.</span></span> <span data-ttu-id="708eb-444">Sonuçta elde edilen kod aşağıdaki kod parçacığında gösterilir.</span><span class="sxs-lookup"><span data-stu-id="708eb-444">The resulting code is shown in the following snippet.</span></span>

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. <span data-ttu-id="708eb-445">Tuşuna **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="708eb-445">Press **CTRL + S** to save the changes.</span></span>

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a><span data-ttu-id="708eb-446">Görev 2 – günlük test uygulamanızı yeniden dağıtmanıza gerek</span><span class="sxs-lookup"><span data-stu-id="708eb-446">Task 2 – Redeploying the Geek Quiz Application</span></span>

<span data-ttu-id="708eb-447">Üretim ortamına yeni bir dağıtım tetikleyecek depo önceki görevde yapılan değişiklikleri şimdi gönderir.</span><span class="sxs-lookup"><span data-stu-id="708eb-447">You will now push the changes you made in the previous task to the repository, which will trigger a new deployment to the production environment.</span></span> <span data-ttu-id="708eb-448">Ardından, troubleshot kullanarak bir sorun **F12 geliştirme araçları** Internet Explorer tarafından sağlanan ve Azure Yönetim Portalı'ndan bir geri alma önceki dağıtım gerçekleştirin.</span><span class="sxs-lookup"><span data-stu-id="708eb-448">Then, you will troubleshot an issue using the **F12 development tools** provided by Internet Explorer, and then perform a rollback to the previous deployment from the Azure management portal.</span></span>

1. <span data-ttu-id="708eb-449">Yeni bir **Git Bash** güncelleştirilmiş uygulamayı Azure App Service'e dağıtmak için konsol.</span><span class="sxs-lookup"><span data-stu-id="708eb-449">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
2. <span data-ttu-id="708eb-450">Azure'a değişiklikleri göndermek için aşağıdaki komutları yürütün.</span><span class="sxs-lookup"><span data-stu-id="708eb-450">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="708eb-451">Güncelleştirme *[YOUR uygulama yolu]* yolu ile yer tutucu **GeekQuiz** çözümü.</span><span class="sxs-lookup"><span data-stu-id="708eb-451">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="708eb-452">Dağıtım parolanızı girmeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="708eb-452">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Azure koymadan işlenmiş kodu](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    <span data-ttu-id="708eb-454">*Azure koymadan işlenmiş kodu*</span><span class="sxs-lookup"><span data-stu-id="708eb-454">*Pushing refactored code to Azure*</span></span>
3. <span data-ttu-id="708eb-455">Internet Explorer'ı açın ve web uygulamanıza gidin (örneğin `http://<your-web-site>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="708eb-455">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="708eb-456">Önceden oluşturulmuş kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="708eb-456">Log in using the previously created credentials.</span></span>
4. <span data-ttu-id="708eb-457">Tuşuna **F12** geliştirme araçları başlatmak için seçin **ağ** sekmesinde **Yürüt** kaydı başlatmak için düğmesi.</span><span class="sxs-lookup"><span data-stu-id="708eb-457">Press **F12** to launch the development tools, select the **Network** tab and click the **Play** button to start recording.</span></span>

    <span data-ttu-id="708eb-458">![Ağ kayıt başlangıç](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "ağ kayıt başlatılıyor")</span><span class="sxs-lookup"><span data-stu-id="708eb-458">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Starting network recording")</span></span>

    <span data-ttu-id="708eb-459">*Başlangıç ağ kaydı*</span><span class="sxs-lookup"><span data-stu-id="708eb-459">*Starting network recording*</span></span>
5. <span data-ttu-id="708eb-460">Test herhangi bir seçenek seçin.</span><span class="sxs-lookup"><span data-stu-id="708eb-460">Select any option of the quiz.</span></span> <span data-ttu-id="708eb-461">Hiçbir şey olmaz görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="708eb-461">You will see that nothing happens.</span></span>
6. <span data-ttu-id="708eb-462">İçinde **F12** penceresi, POST HTTP isteğine karşılık gelen girişi gösterir bir HTTP **500** sonucu.</span><span class="sxs-lookup"><span data-stu-id="708eb-462">In the **F12** window, the entry corresponding to the POST HTTP request shows an HTTP **500** result.</span></span>

    ![HTTP 500 hata](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    <span data-ttu-id="708eb-464">*HTTP 500 hata*</span><span class="sxs-lookup"><span data-stu-id="708eb-464">*HTTP 500 error*</span></span>
7. <span data-ttu-id="708eb-465">Seçin **konsol** sekmesi. Bir hata neden ayrıntıları ile günlüğe kaydedilir.</span><span class="sxs-lookup"><span data-stu-id="708eb-465">Select the **Console** tab. An error is logged with the details of the cause.</span></span>

    ![Günlüğe kaydedilen hata](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    <span data-ttu-id="708eb-467">*Günlüğe kaydedilen hata*</span><span class="sxs-lookup"><span data-stu-id="708eb-467">*Logged error*</span></span>
8. <span data-ttu-id="708eb-468">Hata ayrıntılarını bölümünü bulun.</span><span class="sxs-lookup"><span data-stu-id="708eb-468">Locate the details part of the error.</span></span> <span data-ttu-id="708eb-469">Açıkçası, bu hata, önceki adımda kaydedilen yeniden düzenleme kod kaynaklanır.</span><span class="sxs-lookup"><span data-stu-id="708eb-469">Clearly, this error is caused by the code refactoring you committed in the previous steps.</span></span>

    <span data-ttu-id="708eb-470">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span><span class="sxs-lookup"><span data-stu-id="708eb-470">`Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.</span></span>
9. <span data-ttu-id="708eb-471">Tarayıcı kapatmayın.</span><span class="sxs-lookup"><span data-stu-id="708eb-471">Do not close the browser.</span></span>
10. <span data-ttu-id="708eb-472">Yeni bir tarayıcı örneğiyle gidin [Azure Yönetim Portalı](https://manage.windowsazure.com) ve aboneliğinizle ilişkili Microsoft hesabı kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="708eb-472">In a new browser instance, navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
11. <span data-ttu-id="708eb-473">Seçin **Web siteleri** ve alıştırma 2'de oluşturulan web uygulaması'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="708eb-473">Select **Websites** and click the web app you created in Exercise 2.</span></span>
12. <span data-ttu-id="708eb-474">Gidin **dağıtımları** sayfası.</span><span class="sxs-lookup"><span data-stu-id="708eb-474">Navigate to the **Deployments** page.</span></span> <span data-ttu-id="708eb-475">Gerçekleştirilen tüm işlemeleri dağıtım geçmişini listelenen dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="708eb-475">Notice that all the commits performed are listed in the deployment history.</span></span>

    ![Var olan dağıtımlar listesi](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    <span data-ttu-id="708eb-477">*Var olan dağıtımlar listesi*</span><span class="sxs-lookup"><span data-stu-id="708eb-477">*List of existing deployments*</span></span>
13. <span data-ttu-id="708eb-478">Önceki yürütme tıklatıp **dağıtmanız** komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="708eb-478">Select the previous commit and click **Redeploy** on the command bar.</span></span>

    ![Önceki yürütme dağıtarak](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    <span data-ttu-id="708eb-480">*Önceki yürütme dağıtarak*</span><span class="sxs-lookup"><span data-stu-id="708eb-480">*Redeploying the previous commit*</span></span>
14. <span data-ttu-id="708eb-481">Onaylamanız istendiğinde tıklatın **Evet**.</span><span class="sxs-lookup"><span data-stu-id="708eb-481">When prompted to confirm, click **Yes**.</span></span>

    ![Yeniden dağıtım onaylama](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. <span data-ttu-id="708eb-483">Dağıtım tamamlandığında, web app ve tuşuna tarayıcı örneğiyle dönmek **CTRL + F5**.</span><span class="sxs-lookup"><span data-stu-id="708eb-483">When the deployment completes, switch back to the browser instance with your web app and press **CTRL + F5**.</span></span>
16. <span data-ttu-id="708eb-484">Seçeneklerden birini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="708eb-484">Click any of the options.</span></span> <span data-ttu-id="708eb-485">Çevir animasyon artık yerinde ve sonucu olur (*düzeltmek/yanlış*) görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="708eb-485">The flip animation will now take place and the result (*correct/incorrect*) will be displayed.</span></span>
17. <span data-ttu-id="708eb-486">(İsteğe bağlı) Geçiş **Git Bash** konsol ve önceki yürütme döndürmek için aşağıdaki komutları yürütün.</span><span class="sxs-lookup"><span data-stu-id="708eb-486">(Optional) Switch to the **Git Bash** console and execute the following commands to revert to the previous commit.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-487">Bu komutlar Git deposu bozuk yürütmede yapılan tüm değişiklikleri geri alır yeni bir kayıt oluşturun.</span><span class="sxs-lookup"><span data-stu-id="708eb-487">These commands create a new commit that undoes all changes in the Git repository that were made in the bad commit.</span></span> <span data-ttu-id="708eb-488">Azure sonra yeni tamamlama kullanarak uygulamayı yeniden Dağıt.</span><span class="sxs-lookup"><span data-stu-id="708eb-488">Azure will then redeploy the application using the new commit.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a><span data-ttu-id="708eb-489">Alıştırma 4: Azure Storage kullanma ölçeklendirme</span><span class="sxs-lookup"><span data-stu-id="708eb-489">Exercise 4: Scaling Using Azure Storage</span></span>

<span data-ttu-id="708eb-490">**BLOB'ları** büyük miktarlarda yapılandırılmamış metin veya ikili veri video, ses ve görüntü gibi depolamak için kolay bir yoludur.</span><span class="sxs-lookup"><span data-stu-id="708eb-490">**Blobs** are the simplest way to store large amounts of unstructured text or binary data such as video, audio and images.</span></span> <span data-ttu-id="708eb-491">Statik içerik depolama için uygulamanızın taşıma, görüntülerin veya belgelerin doğrudan tarayıcıya sunarak uygulamanız ölçeği yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="708eb-491">Moving the static content of your application to Storage, helps to scale your application by serving images or documents directly to the browser.</span></span>

<span data-ttu-id="708eb-492">Bu alıştırmada, bir Blob kapsayıcısını, uygulamanızın statik içerik taşınır.</span><span class="sxs-lookup"><span data-stu-id="708eb-492">In this exercise, you will move the static content of your application to a Blob container.</span></span> <span data-ttu-id="708eb-493">Sonra da eklemek için uygulamanızın yapılandıracak bir **ASP.NET URL yeniden yazma kuralı** içinde **Web.config** içeriğinizi Blob kapsayıcısına yeniden yönlendirmek için.</span><span class="sxs-lookup"><span data-stu-id="708eb-493">Then you will configure your application to add an **ASP.NET URL rewrite rule** in the **Web.config** to redirect your content to the Blob container.</span></span>

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a><span data-ttu-id="708eb-494">Görev 1 – bir Azure depolama hesabı oluşturma</span><span class="sxs-lookup"><span data-stu-id="708eb-494">Task 1 – Creating an Azure Storage Account</span></span>

<span data-ttu-id="708eb-495">Bu görevde, Yönetim Portalı'nı kullanarak yeni bir depolama hesabının nasıl oluşturulacağını öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="708eb-495">In this task you will learn how to create a new storage account using the management portal.</span></span>

1. <span data-ttu-id="708eb-496">Gidin [Azure Yönetim Portalı](https://manage.windowsazure.com) ve aboneliğinizle ilişkili Microsoft hesabı kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="708eb-496">Navigate to the [Azure management portal](https://manage.windowsazure.com) and sign in using the Microsoft account associated with your subscription.</span></span>
2. <span data-ttu-id="708eb-497">Seçin **yeni | Veri Hizmetleri | Depolama | Hızlı Oluştur** yeni bir depolama hesabı oluşturmaya başlamak için.</span><span class="sxs-lookup"><span data-stu-id="708eb-497">Select **New | Data Services | Storage | Quick Create** to start creating a new storage account.</span></span> <span data-ttu-id="708eb-498">Seçin ve hesabı için benzersiz bir ad girin bir **bölge** listeden.</span><span class="sxs-lookup"><span data-stu-id="708eb-498">Enter a unique name for the account and select a **Region** from the list.</span></span> <span data-ttu-id="708eb-499">Tıklatın **depolama hesabı oluştur** devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="708eb-499">Click **Create Storage Account** to continue.</span></span>

    <span data-ttu-id="708eb-500">![Yeni bir depolama hesabı oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "yeni bir depolama hesabı oluşturma")</span><span class="sxs-lookup"><span data-stu-id="708eb-500">![Creating a new Storage Account](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "Creating a new Storage Account")</span></span>

    <span data-ttu-id="708eb-501">*Yeni bir depolama hesabı oluşturma*</span><span class="sxs-lookup"><span data-stu-id="708eb-501">*Creating a new storage account*</span></span>
3. <span data-ttu-id="708eb-502">İçinde **depolama** bölümünde, yeni depolama hesabı durumunu olana kadar bekleyin *çevrimiçi* aşağıdaki adımıyla devam etmek için.</span><span class="sxs-lookup"><span data-stu-id="708eb-502">In the **Storage** section, wait until the status of the new storage account changes to *Online* in order to continue with the following step.</span></span>

    <span data-ttu-id="708eb-503">![Oluşturulan depolama hesabı](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "depolama hesabı oluşturuldu")</span><span class="sxs-lookup"><span data-stu-id="708eb-503">![Storage Account created](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "Storage Account created")</span></span>

    <span data-ttu-id="708eb-504">*Depolama hesabı oluşturuldu*</span><span class="sxs-lookup"><span data-stu-id="708eb-504">*Storage Account created*</span></span>
4. <span data-ttu-id="708eb-505">Depolama hesabının adına tıklayın ve ardından **Pano** sayfanın üst kısmındaki bağlantı.</span><span class="sxs-lookup"><span data-stu-id="708eb-505">Click on the storage account name and then click the **Dashboard** link at the top of the page.</span></span> <span data-ttu-id="708eb-506">**Pano** sayfası hesap ve uygulamalarınız içinde kullanılan hizmet uç noktalarına durumu hakkında bilgi ile sağlar.</span><span class="sxs-lookup"><span data-stu-id="708eb-506">The **Dashboard** page provides you with information about the status of the account and the service endpoints that can be used within your applications.</span></span>

    <span data-ttu-id="708eb-507">![Depolama hesabı Panosu görüntüleme](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "depolama hesabı Panosu görüntüleme")</span><span class="sxs-lookup"><span data-stu-id="708eb-507">![Displaying the Storage Account Dashboard](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "Displaying the Storage Account Dashboard")</span></span>

    <span data-ttu-id="708eb-508">*Depolama hesabı Panosu görüntüleme*</span><span class="sxs-lookup"><span data-stu-id="708eb-508">*Displaying the Storage Account Dashboard*</span></span>
5. <span data-ttu-id="708eb-509">Tıklatın **erişim anahtarlarını Yönet** gezinti çubuğu düğmesini.</span><span class="sxs-lookup"><span data-stu-id="708eb-509">Click the **Manage Access Keys** button in the navigation bar.</span></span>

    <span data-ttu-id="708eb-510">![Yönet erişim tuşları düğmesi](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "erişim anahtarlarını Yönet düğmesi")</span><span class="sxs-lookup"><span data-stu-id="708eb-510">![Manage Access Keys button](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "Manage Access Keys button")</span></span>

    <span data-ttu-id="708eb-511">*Erişim tuşları düğmesi yönetme*</span><span class="sxs-lookup"><span data-stu-id="708eb-511">*Manage Access Keys button*</span></span>
6. <span data-ttu-id="708eb-512">İçinde **erişim anahtarlarını Yönet** iletişim kutusu, kopya **depolama hesabı adı** ve **birincil erişim anahtarını** aşağıdaki alıştırmada ihtiyaç duyacaksınız.</span><span class="sxs-lookup"><span data-stu-id="708eb-512">In the **Manage Access Keys** dialog box, copy the **Storage Account Name** and **Primary Access Key** as you will need them in the following exercise.</span></span> <span data-ttu-id="708eb-513">Ardından, iletişim kutusunu kapatın.</span><span class="sxs-lookup"><span data-stu-id="708eb-513">Then, close the dialog box.</span></span>

    <span data-ttu-id="708eb-514">![Yönet erişim tuşu iletişim kutusunda](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "erişim anahtarını yönet iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="708eb-514">![Manage Access Key dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "Manage Access Key dialog box")</span></span>

    <span data-ttu-id="708eb-515">*Erişim anahtarı iletişim kutusu yönetme*</span><span class="sxs-lookup"><span data-stu-id="708eb-515">*Manage Access Key dialog box*</span></span>

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a><span data-ttu-id="708eb-516">Görev 2 – bir varlık Azure Blob depolama alanına yüklemek</span><span class="sxs-lookup"><span data-stu-id="708eb-516">Task 2 – Uploading an Asset to Azure Blob Storage</span></span>

<span data-ttu-id="708eb-517">Bu görevde, depolama hesabınıza bağlanmak için Sunucu Gezgini penceresini Visual Studio'dan kullanır.</span><span class="sxs-lookup"><span data-stu-id="708eb-517">In this task, you will use the Server Explorer window from Visual Studio to connect to your storage account.</span></span> <span data-ttu-id="708eb-518">Ardından, bir blob kapsayıcı oluşturun ve kapsayıcıya günlük test logosu bir dosyayı karşıya.</span><span class="sxs-lookup"><span data-stu-id="708eb-518">You will then create a blob container and upload a file with the Geek Quiz logo to the container.</span></span>

1. <span data-ttu-id="708eb-519">Visual Studio örneğiyle geçin **GeekQuiz** önceki alıştırmada çözümden.</span><span class="sxs-lookup"><span data-stu-id="708eb-519">Switch to the Visual Studio instance with the **GeekQuiz** solution from the previous exercise.</span></span>
2. <span data-ttu-id="708eb-520">Menü çubuğundan seçin **Görünüm** ve ardından **Sunucu Gezgini**.</span><span class="sxs-lookup"><span data-stu-id="708eb-520">From the menu bar, select **View** and then click **Server Explorer**.</span></span>
3. <span data-ttu-id="708eb-521">İçinde **Sunucu Gezgini**, sağ **Azure** düğümü ve select **Azure Bağlan...** . Aboneliğinizle ilişkili Microsoft hesabı kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="708eb-521">In **Server Explorer**, right-click the **Azure** node and select **Connect to Azure...**. Sign in using the Microsoft account associated with your subscription.</span></span>

    ![Microsoft Azure'a Bağlan](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    <span data-ttu-id="708eb-523">*Azure'a bağlanma*</span><span class="sxs-lookup"><span data-stu-id="708eb-523">*Connect to Azure*</span></span>
4. <span data-ttu-id="708eb-524">Genişletin **Azure** düğümünü sağ tıklatın **depolama** seçip **harici depolamayı Ekle...** .</span><span class="sxs-lookup"><span data-stu-id="708eb-524">Expand the **Azure** node, right-click **Storage** and select **Attach External Storage...**.</span></span>
5. <span data-ttu-id="708eb-525">İçinde **yeni depolama hesabı Ekle** iletişim kutusunda, girin **hesap adı** ve **hesap anahtarı** önceki görev ve tıklatın Elde **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="708eb-525">In the **Add New Storage Account** dialog box, enter the **Account name** and **Account key** you obtained in the previous task and click **OK**.</span></span>

    ![Yeni depolama hesabı iletişim kutusu ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    <span data-ttu-id="708eb-527">*Yeni depolama hesabı iletişim kutusu ekleme*</span><span class="sxs-lookup"><span data-stu-id="708eb-527">*Add New Storage Account dialog box*</span></span>
6. <span data-ttu-id="708eb-528">Depolama hesabınızın altında görünmelidir **depolama** düğümü.</span><span class="sxs-lookup"><span data-stu-id="708eb-528">Your storage account should appear under the **Storage** node.</span></span> <span data-ttu-id="708eb-529">Depolama hesabınızı ve sağ **BLOB'lar** seçip **Blob kapsayıcı oluşturun...** .</span><span class="sxs-lookup"><span data-stu-id="708eb-529">Expand your storage account, right-click **Blobs** and select **Create Blob Container...**.</span></span>

    <span data-ttu-id="708eb-530">![BLOB kapsayıcı oluşturun](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Blob kapsayıcı oluşturun")</span><span class="sxs-lookup"><span data-stu-id="708eb-530">![Create Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "Create Blob Container")</span></span>

    <span data-ttu-id="708eb-531">*BLOB kapsayıcı oluşturun*</span><span class="sxs-lookup"><span data-stu-id="708eb-531">*Create Blob Container*</span></span>
7. <span data-ttu-id="708eb-532">İçinde **Blob kapsayıcısı oluşturmak** iletişim kutusuna, blob kapsayıcısı için bir ad girin ve tıklatın **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="708eb-532">In the **Create Blob Container** dialog box, enter a name for the blob container and click **OK**.</span></span>

    <span data-ttu-id="708eb-533">![Create Blob kapsayıcısı iletişim kutusunda](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Blob kapsayıcı Oluştur iletişim kutusu")</span><span class="sxs-lookup"><span data-stu-id="708eb-533">![Create Blob Container dialog box](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "Create Blob Container dialog box")</span></span>

    <span data-ttu-id="708eb-534">*BLOB kapsayıcısı iletişim kutusu oluşturma*</span><span class="sxs-lookup"><span data-stu-id="708eb-534">*Create Blob Container dialog box*</span></span>
8. <span data-ttu-id="708eb-535">Yeni blob kapsayıcı eklenmelidir **BLOB'lar** düğümü.</span><span class="sxs-lookup"><span data-stu-id="708eb-535">The new blob container should be added to the **Blobs** node.</span></span> <span data-ttu-id="708eb-536">Kapsayıcı genel hale getirmek üzere kapsayıcıda erişim izinlerini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="708eb-536">Change the access permissions in the container to make the container public.</span></span> <span data-ttu-id="708eb-537">Bunu yapmak için sağ **görüntüleri** kapsayıcı ve select **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="708eb-537">To do this, right-click the **images** container and select **Properties**.</span></span>

    <span data-ttu-id="708eb-538">![kapsayıcı özellikleri görüntüleri](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "görüntüleri kapsayıcı özellikleri")</span><span class="sxs-lookup"><span data-stu-id="708eb-538">![images container properties](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "images container properties")</span></span>

    <span data-ttu-id="708eb-539">*Görüntü kapsayıcısı özellikleri*</span><span class="sxs-lookup"><span data-stu-id="708eb-539">*Images container properties*</span></span>
9. <span data-ttu-id="708eb-540">İçinde **özellikleri** penceresindeki ayarlayın **herkese okuma erişimi** için **kapsayıcı**.</span><span class="sxs-lookup"><span data-stu-id="708eb-540">In the **Properties** window, set the **Public Read Access** to **Container**.</span></span>

    <span data-ttu-id="708eb-541">![Ortak okuma erişimi özelliğini değiştirmek](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "herkese okuma erişimi özelliğini değiştirme")</span><span class="sxs-lookup"><span data-stu-id="708eb-541">![Changing public read access property](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "Changing public read access property")</span></span>

    <span data-ttu-id="708eb-542">*Ortak okuma erişimi özelliğini değiştirme*</span><span class="sxs-lookup"><span data-stu-id="708eb-542">*Changing public read access property*</span></span>
10. <span data-ttu-id="708eb-543">Eminseniz istendiğinde, istediğiniz genel erişim özelliği değiştirmek **Evet**.</span><span class="sxs-lookup"><span data-stu-id="708eb-543">When prompted if you are sure you want to change the public access property, click **Yes**.</span></span>

    <span data-ttu-id="708eb-544">![Microsoft Visual Studio uyarı](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio uyarı")</span><span class="sxs-lookup"><span data-stu-id="708eb-544">![Microsoft Visual Studio warning](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "Microsoft Visual Studio warning")</span></span>

    <span data-ttu-id="708eb-545">*Microsoft Visual Studio uyarı*</span><span class="sxs-lookup"><span data-stu-id="708eb-545">*Microsoft Visual Studio warning*</span></span>
11. <span data-ttu-id="708eb-546">İçinde **Sunucu Gezgini**, sağ tıklatın, **görüntüleri** blob kapsayıcısı ve select **görünüm Blob kapsayıcısını**.</span><span class="sxs-lookup"><span data-stu-id="708eb-546">In **Server Explorer**, right-click in the **images** blob container and select **View Blob Container**.</span></span>

    <span data-ttu-id="708eb-547">![Blob kapsayıcısı görüntülemek](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "görüntülemek Blob kapsayıcısı")</span><span class="sxs-lookup"><span data-stu-id="708eb-547">![View Blob Container](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "View Blob Container")</span></span>

    <span data-ttu-id="708eb-548">*Görünüm Blob kapsayıcısı*</span><span class="sxs-lookup"><span data-stu-id="708eb-548">*View Blob Container*</span></span>
12. <span data-ttu-id="708eb-549">Görüntü kapsayıcısı yeni bir pencerede açmak ve hiçbir girdi bir gösterge gösterilecek.</span><span class="sxs-lookup"><span data-stu-id="708eb-549">The images container should open in a new window and a legend with no entries should be shown.</span></span> <span data-ttu-id="708eb-550">Tıklatın **karşıya** blob kapsayıcısına bir dosyayı karşıya yüklemeyi simgesi.</span><span class="sxs-lookup"><span data-stu-id="708eb-550">Click the **upload** icon to upload a file to the blob container.</span></span>

    <span data-ttu-id="708eb-551">![Görüntü kapsayıcısı yok girişlerle](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "hiçbir girdi ile görüntü kapsayıcısı")</span><span class="sxs-lookup"><span data-stu-id="708eb-551">![Images container with no entries](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "Images container with no entries")</span></span>

    <span data-ttu-id="708eb-552">*Görüntü kapsayıcısı yok girişleri*</span><span class="sxs-lookup"><span data-stu-id="708eb-552">*Images container with no entries*</span></span>
13. <span data-ttu-id="708eb-553">İçinde **karşıya Blob** iletişim kutusunda, gitmek **varlıklar** laboratuvarı klasör.</span><span class="sxs-lookup"><span data-stu-id="708eb-553">In the **Upload Blob** dialog box, navigate to the **Assets** folder of the lab.</span></span> <span data-ttu-id="708eb-554">Seçin **logosu big.png** dosya ve tıklayın **açık**.</span><span class="sxs-lookup"><span data-stu-id="708eb-554">Select the **logo-big.png** file and click **Open**.</span></span>
14. <span data-ttu-id="708eb-555">Dosya karşıya kadar bekleyin.</span><span class="sxs-lookup"><span data-stu-id="708eb-555">Wait until the file is uploaded.</span></span> <span data-ttu-id="708eb-556">Karşıya yükleme tamamlandığında, dosya görüntüleri kapsayıcısında listelenmelidir.</span><span class="sxs-lookup"><span data-stu-id="708eb-556">When the upload completes, the file should be listed in the images container.</span></span> <span data-ttu-id="708eb-557">Dosya girişi sağ tıklatın ve seçin **kopya URL**.</span><span class="sxs-lookup"><span data-stu-id="708eb-557">Right-click the file entry and select **Copy URL**.</span></span>

    <span data-ttu-id="708eb-558">![BLOB URL'sini Kopyala](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "kopyalama blob dosya URL'si")</span><span class="sxs-lookup"><span data-stu-id="708eb-558">![Copy blob URL](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copy blob file URL")</span></span>

    <span data-ttu-id="708eb-559">*BLOB URL'sini Kopyala*</span><span class="sxs-lookup"><span data-stu-id="708eb-559">*Copy blob URL*</span></span>
15. <span data-ttu-id="708eb-560">Internet Explorer'ı açın ve URL'yi yapıştırabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="708eb-560">Open Internet Explorer and paste the URL.</span></span> <span data-ttu-id="708eb-561">Aşağıdaki resimde tarayıcıda gösterilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="708eb-561">The following image should be shown in the browser.</span></span>

    <span data-ttu-id="708eb-562">![Blob Storage'ı Windows logo big.png görüntüden](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "depolama biriminden big.png logo görüntüsü")</span><span class="sxs-lookup"><span data-stu-id="708eb-562">![logo-big.png image from Windows Blob Storage](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "logo-big.png image from storage")</span></span>

    <span data-ttu-id="708eb-563">*Azure Blob depolama biriminden logosu big.png görüntüsü*</span><span class="sxs-lookup"><span data-stu-id="708eb-563">*logo-big.png image from Azure Blob Storage*</span></span>

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a><span data-ttu-id="708eb-564">Görev 3: Azure Blob depolama biriminden statik içeriği kullanmak için çözüm güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="708eb-564">Task 3 – Updating the Solution to Consume Static Content from Azure Blob Storage</span></span>

<span data-ttu-id="708eb-565">Bu görevde, yapılandıracağınız **GeekQuiz** görüntüyü kullanmak için çözüm karşıya Azure Blob depolama alanına (yerine web uygulamasında bulunan görüntü) bir ASP.NET URL yeniden yazma kuralı ekleyerek **web.config**dosya.</span><span class="sxs-lookup"><span data-stu-id="708eb-565">In this task, you will configure the **GeekQuiz** solution to consume the image uploaded to Azure Blob Storage (instead of the image located in the web app) by adding an ASP.NET URL rewrite rule in the **web.config** file.</span></span>

1. <span data-ttu-id="708eb-566">Visual Studio'da açın **Web.config** içinde dosya **GeekQuiz** proje ve bulun  **&lt;system.webServer&gt;**  öğesi.</span><span class="sxs-lookup"><span data-stu-id="708eb-566">In Visual Studio, open the **Web.config** file inside the **GeekQuiz** project and locate the **&lt;system.webServer&gt;** element.</span></span>
2. <span data-ttu-id="708eb-567">Bir URL ile depolama hesabınızın adını yer tutucu güncelleştirme kuralı, yeniden eklemek için aşağıdaki kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="708eb-567">Add the following code to add an URL rewrite rule, updating the placeholder with your storage account name.</span></span>

    <span data-ttu-id="708eb-568">(Kod parçacığını - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span><span class="sxs-lookup"><span data-stu-id="708eb-568">(Code Snippet - *WebSitesInProduction - Ex4 - UrlRewriteRule*)</span></span>

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > <span data-ttu-id="708eb-569">URL yeniden yazma işlemi, bir gelen Web isteği kesintiye uğratan ve istek farklı bir kaynak için yönlendirme işlemidir.</span><span class="sxs-lookup"><span data-stu-id="708eb-569">URL rewriting is the process of intercepting an incoming Web request and redirecting the request to a different resource.</span></span> <span data-ttu-id="708eb-570">URL kuralları yeniden yazma isteği yönlendirilecek gerektiği zaman ve nerede yönlendirilir yazmaksızın altyapısı söyler.</span><span class="sxs-lookup"><span data-stu-id="708eb-570">The URL rewriting rules tells the rewriting engine when a request needs to be redirected, and where should they be redirected.</span></span> <span data-ttu-id="708eb-571">Yazmaksızın kural iki dizesini oluşur: İstenen URL'de aranacak deseni (genellikle, normal ifadeler kullanarak) ve desenle değiştirirseniz için dizesi bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="708eb-571">A rewriting rule is composed of two strings: the pattern to look for in the requested URL (usually, using regular expressions), and the string to replace the pattern with, if found.</span></span> <span data-ttu-id="708eb-572">Daha fazla bilgi için bkz: [URL yeniden yazma işlemi ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span><span class="sxs-lookup"><span data-stu-id="708eb-572">For more information, see [URL Rewriting in ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).</span></span>
3. <span data-ttu-id="708eb-573">Tuşuna **CTRL + S** değişiklikleri kaydedin.</span><span class="sxs-lookup"><span data-stu-id="708eb-573">Press **CTRL + S** to save the changes.</span></span>
4. <span data-ttu-id="708eb-574">Yeni bir **Git Bash** güncelleştirilmiş uygulamayı Azure App Service'e dağıtmak için konsol.</span><span class="sxs-lookup"><span data-stu-id="708eb-574">Open a new **Git Bash** console to deploy the updated application to Azure App Service.</span></span>
5. <span data-ttu-id="708eb-575">Azure'a değişiklikleri göndermek için aşağıdaki komutları yürütün.</span><span class="sxs-lookup"><span data-stu-id="708eb-575">Execute the following commands to push the changes to Azure.</span></span> <span data-ttu-id="708eb-576">Güncelleştirme *[YOUR uygulama yolu]* yolu ile yer tutucu **GeekQuiz** çözümü.</span><span class="sxs-lookup"><span data-stu-id="708eb-576">Update the *[YOUR-APPLICATION-PATH]* placeholder with the path to the **GeekQuiz** solution.</span></span> <span data-ttu-id="708eb-577">Dağıtım parolanızı girmeniz istenir.</span><span class="sxs-lookup"><span data-stu-id="708eb-577">You will be prompted for your deployment password.</span></span>

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Güncelleştirme Azure'a dağıtma](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    <span data-ttu-id="708eb-579">*Güncelleştirme Azure'a dağıtma*</span><span class="sxs-lookup"><span data-stu-id="708eb-579">*Deploying update to Azure*</span></span>

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a><span data-ttu-id="708eb-580">Görev 4 – doğrulama</span><span class="sxs-lookup"><span data-stu-id="708eb-580">Task 4 – Verification</span></span>

<span data-ttu-id="708eb-581">Bu görevde kullanacağınız **Internet Explorer** göz atmak için **günlük test** uygulama ve URL görüntüleri çalışır ve kuralı yeniden onay barındırılan görüntüsüne yönlendirilir **Azure Blob Depolama**.</span><span class="sxs-lookup"><span data-stu-id="708eb-581">In this task you will use **Internet Explorer** to browse the **Geek Quiz** application and check that the URL rewrite rule for images works and you are redirected to the image hosted on **Azure Blob Storage**.</span></span>

1. <span data-ttu-id="708eb-582">Internet Explorer'ı açın ve web uygulamanıza gidin (örneğin `http://<your-web-site>.azurewebsites.net`).</span><span class="sxs-lookup"><span data-stu-id="708eb-582">Open Internet Explorer and navigate to your web app (e.g. `http://<your-web-site>.azurewebsites.net`).</span></span> <span data-ttu-id="708eb-583">Önceden oluşturulmuş kimlik bilgilerini kullanarak oturum açın.</span><span class="sxs-lookup"><span data-stu-id="708eb-583">Log in using the previously created credentials.</span></span>

    <span data-ttu-id="708eb-584">![Görüntü ile günlük test web uygulamasını gösteren](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "görüntüsü ile günlük test web uygulamasını gösteren")</span><span class="sxs-lookup"><span data-stu-id="708eb-584">![Showing the Geek Quiz web app with the image](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "Showing the Geek Quiz web app with the image")</span></span>

    <span data-ttu-id="708eb-585">*Görüntü ile günlük test web uygulamasını gösteren*</span><span class="sxs-lookup"><span data-stu-id="708eb-585">*Showing the Geek Quiz web app with the image*</span></span>
2. <span data-ttu-id="708eb-586">Tuşuna **F12** geliştirme araçları başlatmak için seçin **ağ** sekmesinde ve kaydı başlatın.</span><span class="sxs-lookup"><span data-stu-id="708eb-586">Press **F12** to launch the development tools, select the **Network** tab and start recording.</span></span>

    <span data-ttu-id="708eb-587">![Ağ kayıt başlangıç](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "ağ kayıt başlatılıyor")</span><span class="sxs-lookup"><span data-stu-id="708eb-587">![Starting network recording](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Starting network recording")</span></span>

    <span data-ttu-id="708eb-588">*Başlangıç ağ kaydı*</span><span class="sxs-lookup"><span data-stu-id="708eb-588">*Starting network recording*</span></span>
3. <span data-ttu-id="708eb-589">Tuşuna **CTRL + F5** web sayfasını yenilemek için.</span><span class="sxs-lookup"><span data-stu-id="708eb-589">Press **CTRL + F5** to refresh the web page.</span></span>
4. <span data-ttu-id="708eb-590">Sayfa yükleme tamamlandıktan sonra bir HTTP isteği için görmelisiniz **/img/logo-big.png** URL bir HTTP ile **301** sonucu (Yönlendirme) ve başka bir istek için `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL bir HTTP ile**200** sonucu.</span><span class="sxs-lookup"><span data-stu-id="708eb-590">Once the page has finished loading, you should see an HTTP request for the **/img/logo-big.png** URL with an HTTP **301** result (redirect) and another request for `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` URL with a HTTP **200** result.</span></span>

    <span data-ttu-id="708eb-591">![URL yeniden yönlendirme doğrulama](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Geliştirme Araçları'nda yeniden yönlendirme gösterme")</span><span class="sxs-lookup"><span data-stu-id="708eb-591">![Verifying the URL redirect](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "Showing the redirect in Dev Tools")</span></span>

    <span data-ttu-id="708eb-592">*URL yeniden yönlendirme doğrulanıyor*</span><span class="sxs-lookup"><span data-stu-id="708eb-592">*Verifying the URL redirect*</span></span>

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a><span data-ttu-id="708eb-593">Alıştırma 5: Web uygulamaları için otomatik ölçeklendirme kullanma</span><span class="sxs-lookup"><span data-stu-id="708eb-593">Exercise 5: Using Autoscale for Web Apps</span></span>

> [!NOTE]
> <span data-ttu-id="708eb-594">Destek Web yükü gerektirdiğinden bu alıştırmada isteğe bağlı, &amp; yalnızca için kullanılabilir olan performans testi **Visual Studio 2013 Ultimate Edition**.</span><span class="sxs-lookup"><span data-stu-id="708eb-594">This exercise is optional, since it requires support for Web Load &amp; Performance Testing which is only available for **Visual Studio 2013 Ultimate Edition**.</span></span> <span data-ttu-id="708eb-595">Belirli Visual Studio 2013 özellikleri hakkında daha fazla bilgi için sürümlerini karşılaştırma [burada](https://www.microsoft.com/visualstudio/eng/products/compare).</span><span class="sxs-lookup"><span data-stu-id="708eb-595">For more information on specific Visual Studio 2013 features, compare versions [here](https://www.microsoft.com/visualstudio/eng/products/compare).</span></span>


<span data-ttu-id="708eb-596">**Azure App Service Web Apps** çalışan web uygulamaları için otomatik ölçeklendirme özelliği sağlar **Standart mod**.</span><span class="sxs-lookup"><span data-stu-id="708eb-596">**Azure App Service Web Apps** provides the Autoscale feature for web apps running in **Standard Mode**.</span></span> <span data-ttu-id="708eb-597">Otomatik ölçeklendirme, örnek sayısı, web uygulamanızın yük bağlı olarak otomatik şekilde ölçeklendirebilir Azure olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="708eb-597">Autoscale lets Azure automatically scale the instance count of your web app depending on the load.</span></span> <span data-ttu-id="708eb-598">Otomatik ölçeklendirme etkinleştirildiğinde, Azure web uygulamanızın CPU her beş dakikada denetler ve örnekleri zamandaki o noktada gerektiği gibi ekler.</span><span class="sxs-lookup"><span data-stu-id="708eb-598">When Autoscale is enabled, Azure checks the CPU of your web app once every five minutes and adds instances as needed at that point in time.</span></span> <span data-ttu-id="708eb-599">CPU kullanımı düşükse, Azure web uygulamanızın performansını değil düşürülmüş emin olmak için her iki saatte örneklerini kaldırır.</span><span class="sxs-lookup"><span data-stu-id="708eb-599">If the CPU usage is low, Azure will remove instances once every two hours to ensure that the performance of your web app is not degraded.</span></span>

<span data-ttu-id="708eb-600">Bu alıştırmada yapılandırmak için gereken adımlarda size gidecek **otomatik ölçeklendirme** için özellik **günlük test** web uygulaması.</span><span class="sxs-lookup"><span data-stu-id="708eb-600">In this exercise you will go through the steps required to configure the **Autoscale** feature for the **Geek Quiz** web app.</span></span> <span data-ttu-id="708eb-601">Bu özellik, bir örnek yükseltilmesini sağlamak için uygulama yeterli CPU yükünü oluşturmak için Visual Studio yükleme testi çalıştırarak doğrular.</span><span class="sxs-lookup"><span data-stu-id="708eb-601">You will verify this feature by running a Visual Studio load test to generate enough CPU load on the application to trigger an instance upgrade.</span></span>

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a><span data-ttu-id="708eb-602">Görev 1 – CPU ölçüm tabanlı yapılandırma otomatik ölçeklendirme</span><span class="sxs-lookup"><span data-stu-id="708eb-602">Task 1 – Configuring Autoscale Based on the CPU Metric</span></span>

<span data-ttu-id="708eb-603">Bu görevde alıştırma 2'de oluşturulan web uygulaması için otomatik ölçeklendirme özelliğini etkinleştirmek için Azure Yönetim Portalı'nı kullanır.</span><span class="sxs-lookup"><span data-stu-id="708eb-603">In this task you will use the Azure management portal to enable the Autoscale feature for the web app you created in Exercise 2.</span></span>

1. <span data-ttu-id="708eb-604">İçinde [Azure Yönetim Portalı](https://manage.windowsazure.com/)seçin **Web siteleri** ve alıştırma 2'de oluşturulan web uygulaması'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="708eb-604">In the [Azure management portal](https://manage.windowsazure.com/), select **Websites** and click the web app you created in Exercise 2.</span></span>
2. <span data-ttu-id="708eb-605">Gidin **ölçek** sayfası.</span><span class="sxs-lookup"><span data-stu-id="708eb-605">Navigate to the **Scale** page.</span></span> <span data-ttu-id="708eb-606">Altında **kapasite** bölümünde, select **CPU** için **ölçek ölçüm tarafından** yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="708eb-606">Under the **capacity** section, select **CPU** for the **Scale by Metric** configuration.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-607">CPU ile ölçekleme sırasında Azure CPU kullanımı değişirse uygulamanın kullandığı örneklerinin sayısını dinamik olarak ayarlar.</span><span class="sxs-lookup"><span data-stu-id="708eb-607">When scaling by CPU, Azure dynamically adjusts the number of instances that the app uses if the CPU usage changes.</span></span>

    <span data-ttu-id="708eb-608">![Ölçek CPU tarafından seçme](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "otomatik ölçekleme için CPU ölçüm seçme")</span><span class="sxs-lookup"><span data-stu-id="708eb-608">![Selecting to scale by CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "Selecting the CPU metric for auto scaling")</span></span>

    <span data-ttu-id="708eb-609">*Ölçek CPU tarafından seçme*</span><span class="sxs-lookup"><span data-stu-id="708eb-609">*Selecting to scale by CPU*</span></span>
3. <span data-ttu-id="708eb-610">Değişiklik **hedef CPU** yapılandırmasına **20**-**40** yüzde.</span><span class="sxs-lookup"><span data-stu-id="708eb-610">Change the **Target CPU** configuration to **20**-**40** percent.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-611">Bu aralık, web uygulamanız için ortalama CPU kullanımını temsil eder.</span><span class="sxs-lookup"><span data-stu-id="708eb-611">This range represents the average CPU usage for your web app.</span></span> <span data-ttu-id="708eb-612">Azure ekleyin veya web uygulamanızın bu aralıkta tutmak için örnekler kaldırın.</span><span class="sxs-lookup"><span data-stu-id="708eb-612">Azure will add or remove instances to keep your web app in this range.</span></span> <span data-ttu-id="708eb-613">Ölçeklendirme için kullanılan örneği minimum ve maksimum sayısı belirtilen **örnek sayısı** yapılandırma.</span><span class="sxs-lookup"><span data-stu-id="708eb-613">The minimum and maximum number of instances used for scaling is specified in the **Instance Count** configuration.</span></span> <span data-ttu-id="708eb-614">Azure üzerinde veya bu sınırı aşan hiçbir zaman geçer.</span><span class="sxs-lookup"><span data-stu-id="708eb-614">Azure will never go above or beyond that limit.</span></span>
    > 
    > <span data-ttu-id="708eb-615">Varsayılan **hedef CPU** değerleri yalnızca bu laboratuvarı amaçları doğrultusunda değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="708eb-615">The default **Target CPU** values are modified just for the purposes of this lab.</span></span> <span data-ttu-id="708eb-616">Küçük değerlerle CPU aralığı yapılandırarak, tetikleyici otomatik ölçeklendirme için büyük olasılıkla Orta yük uygulamanın yerleştirildiğinde artmaktadır.</span><span class="sxs-lookup"><span data-stu-id="708eb-616">By configuring the CPU range with small values, you are increasing the chances to trigger Autoscale when a moderate load is placed on the application.</span></span>

    <span data-ttu-id="708eb-617">![Hedef CPU 20 ve yüzde 40 arasında olacak şekilde değiştirmeyi](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "20 ve yüzde 40 arasında olacak şekilde hedef CPU değiştirme")</span><span class="sxs-lookup"><span data-stu-id="708eb-617">![Changing the target CPU to be between 20 and 40 percent](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "Changing the target CPU to be between 20 and 40 percent")</span></span>

    <span data-ttu-id="708eb-618">*Hedef CPU 20 ve yüzde 40 arasında olacak şekilde değiştirme*</span><span class="sxs-lookup"><span data-stu-id="708eb-618">*Changing the Target CPU to be between 20 and 40 percent*</span></span>
4. <span data-ttu-id="708eb-619">Tıklatın **kaydetmek** değişiklikleri kaydetmek için komut çubuğunda.</span><span class="sxs-lookup"><span data-stu-id="708eb-619">Click **Save** in the command bar to save the changes.</span></span>

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a><span data-ttu-id="708eb-620">Görev 2 – yük ile Visual Studio test etme</span><span class="sxs-lookup"><span data-stu-id="708eb-620">Task 2 – Load Testing with Visual Studio</span></span>

<span data-ttu-id="708eb-621">Şimdi **otomatik ölçeklendirme** bırakıldı oluşturacağınız yapılandırılmış, bir **Web performansı ve yük testi projesi** bazı CPU yükünü web uygulamanızı oluşturmak için Visual Studio'da.</span><span class="sxs-lookup"><span data-stu-id="708eb-621">Now that **Autoscale** has been configured, you will create a **Web Performance and Load Test Project** in Visual Studio to generate some CPU load on your web app.</span></span>

1. <span data-ttu-id="708eb-622">Açık **Visual Studio Ultimate 2013** seçip **dosya | Yeni | Proje...**  yeni bir çözüm başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="708eb-622">Open **Visual Studio Ultimate 2013** and select **File | New | Project...** to start a new solution.</span></span>

    <span data-ttu-id="708eb-623">![Yeni proje oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "yeni proje oluşturma")</span><span class="sxs-lookup"><span data-stu-id="708eb-623">![Creating a new project](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "Creating a new project")</span></span>

    <span data-ttu-id="708eb-624">*Yeni proje oluşturma*</span><span class="sxs-lookup"><span data-stu-id="708eb-624">*Creating a new project*</span></span>
2. <span data-ttu-id="708eb-625">İçinde **yeni proje** iletişim kutusunda **Web performansı ve yük testi projesi** altında **Visual C# | Test** sekmesi. Emin olun **.NET Framework 4.5** olduğunu belirlenirse, proje adı *WebAndLoadTestProject*, seçin bir **konumu** tıklatıp **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="708eb-625">In the **New Project** dialog box, select **Web Performance and Load Test Project** under the **Visual C# | Test** tab. Make sure **.NET Framework 4.5** is selected, name the project *WebAndLoadTestProject*, choose a **Location** and click **OK**.</span></span>

    <span data-ttu-id="708eb-626">![Yeni Web ve yük testi projesi oluşturma](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "yeni Web ve yük testi projesi oluşturma")</span><span class="sxs-lookup"><span data-stu-id="708eb-626">![Creating a new Web and Load Test project](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "Creating a new Web and Load Test project")</span></span>

    <span data-ttu-id="708eb-627">*Yeni Web ve yük testi projesi oluşturma*</span><span class="sxs-lookup"><span data-stu-id="708eb-627">*Creating a new Web and Load Test project*</span></span>
3. <span data-ttu-id="708eb-628">İçinde **WebTest1.webtest** sağ **WebTest1'i** düğümü ve tıklatın **ekleme isteği**.</span><span class="sxs-lookup"><span data-stu-id="708eb-628">In the **WebTest1.webtest** Right-click the **WebTest1** node and click **Add Request**.</span></span>

    <span data-ttu-id="708eb-629">![Bir istek için WebTest1'i ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "WebTest1'i için bir istek ekleme")</span><span class="sxs-lookup"><span data-stu-id="708eb-629">![Adding a request to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "Adding a request to WebTest1")</span></span>

    <span data-ttu-id="708eb-630">*Bir istek için WebTest1'i ekleme*</span><span class="sxs-lookup"><span data-stu-id="708eb-630">*Adding a request to WebTest1*</span></span>
4. <span data-ttu-id="708eb-631">İçinde **özellikleri** penceresi yeni istek düğümünün güncelleştirme **Url** , web uygulamanızın URL'sine işaret edecek şekilde özelliği (örneğin  *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)* ).</span><span class="sxs-lookup"><span data-stu-id="708eb-631">In the **Properties** window of the new request node, update the **Url** property to point to the URL of your web app (e.g. *[http://geek-quiz.azurewebsites.net/](http://geek-quiz.azurewebsites.net/)*).</span></span>

    <span data-ttu-id="708eb-632">![Url özelliğini değiştirmek](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Url özelliğini değiştirme")</span><span class="sxs-lookup"><span data-stu-id="708eb-632">![Changing the Url property](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "Changing the Url property")</span></span>

    <span data-ttu-id="708eb-633">*Url özelliğini değiştirme*</span><span class="sxs-lookup"><span data-stu-id="708eb-633">*Changing the Url property*</span></span>
5. <span data-ttu-id="708eb-634">İçinde **WebTest1.webtest** penceresinde sağ tıklayın **WebTest1'i** tıklatıp **döngü Ekle...** .</span><span class="sxs-lookup"><span data-stu-id="708eb-634">In the **WebTest1.webtest** window, right-click **WebTest1** and click **Add Loop...**.</span></span>

    <span data-ttu-id="708eb-635">![Döngü WebTest1'i için ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "WebTest1'i için bir döngü ekleme")</span><span class="sxs-lookup"><span data-stu-id="708eb-635">![Adding a loop to WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "Adding a loop to WebTest1")</span></span>

    <span data-ttu-id="708eb-636">*Döngü WebTest1'i için ekleme*</span><span class="sxs-lookup"><span data-stu-id="708eb-636">*Adding a loop to WebTest1*</span></span>
6. <span data-ttu-id="708eb-637">İçinde **koşullu Kuralı Ekle ve döngü öğeler** iletişim kutusunda **For döngüsü** kural ve aşağıdaki özellikleri değiştirin.</span><span class="sxs-lookup"><span data-stu-id="708eb-637">In the **Add Conditional Rule and Items to Loop** dialog box, select the **For Loop** rule and modify the following properties.</span></span>

    1. <span data-ttu-id="708eb-638">**Değeri sonlandırma:** 1000</span><span class="sxs-lookup"><span data-stu-id="708eb-638">**Terminating value:** 1000</span></span>
    2. <span data-ttu-id="708eb-639">**Bağlam parametresi adı:** yineleyici</span><span class="sxs-lookup"><span data-stu-id="708eb-639">**Context Parameter Name:** Iterator</span></span>
    3. <span data-ttu-id="708eb-640">**Artış değeri:** 1</span><span class="sxs-lookup"><span data-stu-id="708eb-640">**Increment Value:** 1</span></span>

    <span data-ttu-id="708eb-641">![For döngüsü kuralı seçip özelliklerini güncelleştirme](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "For döngüsü kuralı seçip özelliklerini güncelleştirme")</span><span class="sxs-lookup"><span data-stu-id="708eb-641">![Selecting the For Loop rule and updating the properties](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "Selecting the For Loop rule and updating the properties")</span></span>

    <span data-ttu-id="708eb-642">*For döngüsü kuralı seçip özelliklerini güncelleştirme*</span><span class="sxs-lookup"><span data-stu-id="708eb-642">*Selecting the For Loop rule and updating the properties*</span></span>
7. <span data-ttu-id="708eb-643">Altında **Döngüdeki öğeler** bölümünde, daha önce oluşturduğunuz döngü için ilk ve son öğeyi olmasını isteği seçin.</span><span class="sxs-lookup"><span data-stu-id="708eb-643">Under the **Items in loop** section, select the request you created previously to be the first and last item for the loop.</span></span> <span data-ttu-id="708eb-644">Devam etmek için **Tamam** 'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="708eb-644">Click **OK** to continue.</span></span>

    <span data-ttu-id="708eb-645">![Döngünün ilk ve son öğeler seçme](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "döngünün ilk ve son öğeler seçme")</span><span class="sxs-lookup"><span data-stu-id="708eb-645">![Selecting the first and last items for the loop](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "Selecting the first and last items for the loop")</span></span>

    <span data-ttu-id="708eb-646">*Döngünün ilk ve son öğeler seçme*</span><span class="sxs-lookup"><span data-stu-id="708eb-646">*Selecting the first and last items for the loop*</span></span>
8. <span data-ttu-id="708eb-647">İçinde **Çözüm Gezgini**, sağ tıklatın **WebAndLoadTestProject** projesi, genişletin **Ekle** menü ve select **yük testi...** .</span><span class="sxs-lookup"><span data-stu-id="708eb-647">In **Solution Explorer**, right-click the **WebAndLoadTestProject** project, expand the **Add** menu and select **Load Test...**.</span></span>

    <span data-ttu-id="708eb-648">![Bir yük testi WebAndLoadTestProject projesine ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "bir yük testi WebAndLoadTestProject projesine ekleme")</span><span class="sxs-lookup"><span data-stu-id="708eb-648">![Adding a Load Test to the WebAndLoadTestProject project](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "Adding a Load Test to the WebAndLoadTestProject project")</span></span>

    <span data-ttu-id="708eb-649">*Bir yük testi WebAndLoadTestProject projesine ekleme*</span><span class="sxs-lookup"><span data-stu-id="708eb-649">*Adding a Load Test to the WebAndLoadTestProject project*</span></span>
9. <span data-ttu-id="708eb-650">İçinde **Yeni Yük Testi Sihirbazı** iletişim kutusu, tıklatın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="708eb-650">In the **New Load Test Wizard** dialog box, click **Next**.</span></span>

    <span data-ttu-id="708eb-651">![Yeni Yük Testi Sihirbazı](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Yeni Yük Testi Sihirbazı")</span><span class="sxs-lookup"><span data-stu-id="708eb-651">![New Load Test Wizard](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "New Load Test Wizard")</span></span>

    <span data-ttu-id="708eb-652">*Yeni Yük Testi Sihirbazı*</span><span class="sxs-lookup"><span data-stu-id="708eb-652">*New Load Test Wizard*</span></span>
10. <span data-ttu-id="708eb-653">İçinde **senaryo** sayfasında, **düşünme sürelerini kullanmayın** tıklatıp **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="708eb-653">In the **Scenario** page, select **Do not use think times** and click **Next**.</span></span>

    <span data-ttu-id="708eb-654">![Düşünme sürelerini kullanmamayı seçme](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "düşünme sürelerini kullanmamayı seçme")</span><span class="sxs-lookup"><span data-stu-id="708eb-654">![Selecting not to use think times](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "Selecting not to use think times")</span></span>

    <span data-ttu-id="708eb-655">*Düşünme sürelerini kullanmamayı seçme*</span><span class="sxs-lookup"><span data-stu-id="708eb-655">*Selecting not to use think times*</span></span>
11. <span data-ttu-id="708eb-656">İçinde **yük düzeni** sayfasında, olduğundan emin olun **sabit yük** seçeneği işaretlidir.</span><span class="sxs-lookup"><span data-stu-id="708eb-656">In the **Load Pattern** page, make sure that the **Constant Load** option is selected.</span></span> <span data-ttu-id="708eb-657">Değişiklik **kullanıcı sayısı** ayarını **250** kullanıcılar ve tıklatın **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="708eb-657">Change the **User Count** setting to **250** users and click **Next**.</span></span>

    <span data-ttu-id="708eb-658">![Kullanıcı sayısı için 250 değiştirme](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "kullanıcı sayısı için 250 değiştirme")</span><span class="sxs-lookup"><span data-stu-id="708eb-658">![Changing the user count to 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "Changing the user count to 250")</span></span>

    <span data-ttu-id="708eb-659">*Kullanıcı sayısı için 250 değiştirme*</span><span class="sxs-lookup"><span data-stu-id="708eb-659">*Changing the user count to 250*</span></span>
12. <span data-ttu-id="708eb-660">İçinde **Test karışımı modeli** sayfasında, **sıralı test sırasına göre** tıklatıp **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="708eb-660">In the **Test Mix Model** page, select **Based on sequential test order** and click **Next**.</span></span>

    <span data-ttu-id="708eb-661">![Test karışımı modeli seçme](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "test karışımı modeli seçme")</span><span class="sxs-lookup"><span data-stu-id="708eb-661">![Selecting the test mix model](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "Selecting the test mix model")</span></span>

    <span data-ttu-id="708eb-662">*Test karışımı modeli seçme*</span><span class="sxs-lookup"><span data-stu-id="708eb-662">*Selecting the test mix model*</span></span>
13. <span data-ttu-id="708eb-663">İçinde **Test karışımı modeli** sayfasında, **Ekle...**  test Karışıma eklemek için.</span><span class="sxs-lookup"><span data-stu-id="708eb-663">In the **Test Mix Model** page, click **Add...** to add a test to the mix.</span></span>

    <span data-ttu-id="708eb-664">![Bir testi için test karışımını ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "bir testi için test karışımını ekleme")</span><span class="sxs-lookup"><span data-stu-id="708eb-664">![Adding a test to the test mix](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "Adding a test to the test mix")</span></span>

    <span data-ttu-id="708eb-665">*Bir testi için test karışımını ekleme*</span><span class="sxs-lookup"><span data-stu-id="708eb-665">*Adding a test to the test mix*</span></span>
14. <span data-ttu-id="708eb-666">İçinde **testler Ekle** iletişim kutusu, çift **WebTest1'i** testine ekleme **seçili testleri** listesi.</span><span class="sxs-lookup"><span data-stu-id="708eb-666">In the **Add Tests** dialog box, double-click **WebTest1** to add the test to the **Selected tests** list.</span></span> <span data-ttu-id="708eb-667">Devam etmek için **Tamam** 'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="708eb-667">Click **OK** to continue.</span></span>

    <span data-ttu-id="708eb-668">![WebTest1'i test ekleme](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "WebTest1'i test ekleme")</span><span class="sxs-lookup"><span data-stu-id="708eb-668">![Adding the WebTest1 test](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "Adding the WebTest1 test")</span></span>

    <span data-ttu-id="708eb-669">*WebTest1'i test ekleme*</span><span class="sxs-lookup"><span data-stu-id="708eb-669">*Adding the WebTest1 test*</span></span>
15. <span data-ttu-id="708eb-670">Geri **Test karışımı** sayfasında, **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="708eb-670">Back in the **Test Mix** page, click **Next**.</span></span>

    <span data-ttu-id="708eb-671">![Test Karışımı sayfası Tamamlanıyor](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Test Karışımı sayfası Tamamlanıyor")</span><span class="sxs-lookup"><span data-stu-id="708eb-671">![Completing the Test Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "Completing the Test Mix page")</span></span>

    <span data-ttu-id="708eb-672">*Test Karışımı sayfası Tamamlanıyor*</span><span class="sxs-lookup"><span data-stu-id="708eb-672">*Completing the Test Mix page*</span></span>
16. <span data-ttu-id="708eb-673">İçinde **ağ karışımı** sayfasında, **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="708eb-673">In the **Network Mix** page, click **Next**.</span></span>

    <span data-ttu-id="708eb-674">![Ağ karışımı sayfasındaki sonraki tıkladığınızda](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "tıkladığınızda sonraki ağ karışımı sayfasındaki")</span><span class="sxs-lookup"><span data-stu-id="708eb-674">![Clicking next in the Network Mix page](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "Clicking next in the Network Mix page")</span></span>

    <span data-ttu-id="708eb-675">*Ağ karışımı sayfasında İleri'yi tıklatmadan*</span><span class="sxs-lookup"><span data-stu-id="708eb-675">*Clicking next in the Network Mix page*</span></span>
17. <span data-ttu-id="708eb-676">İçinde **tarayıcı karışımı** sayfasında, **Internet Explorer 10.0** tıklatın ve tarayıcı türü olarak **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="708eb-676">In the **Browser Mix** page, select **Internet Explorer 10.0** as the browser type and click **Next**.</span></span>

    <span data-ttu-id="708eb-677">![Tarayıcı türü seçme](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "tarayıcı türü seçme")</span><span class="sxs-lookup"><span data-stu-id="708eb-677">![Selecting the browser type](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "Selecting the browser type")</span></span>

    <span data-ttu-id="708eb-678">*Tarayıcı türü seçme*</span><span class="sxs-lookup"><span data-stu-id="708eb-678">*Selecting the browser type*</span></span>
18. <span data-ttu-id="708eb-679">İçinde **sayaç kümelerini** sayfasında, **sonraki**.</span><span class="sxs-lookup"><span data-stu-id="708eb-679">In the **Counter Sets** page, click **Next**.</span></span>

    <span data-ttu-id="708eb-680">![Sayaç kümeleri sayfasında İleri'yi tıklatmadan](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "sayaç kümelerini sayfasında İleri tıklatarak")</span><span class="sxs-lookup"><span data-stu-id="708eb-680">![Clicking Next in the Counter Sets page](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "Clicking Next in the Counter Sets page")</span></span>

    <span data-ttu-id="708eb-681">*Sayaç kümeleri sayfasında İleri'yi tıklatmadan*</span><span class="sxs-lookup"><span data-stu-id="708eb-681">*Clicking Next in the Counter Sets page*</span></span>
19. <span data-ttu-id="708eb-682">İçinde **çalıştırma ayarları** sayfasında **yük test süresi** için **5 dakika** tıklatıp **son**.</span><span class="sxs-lookup"><span data-stu-id="708eb-682">In the **Run Settings** page, set the **Load test duration** to **5 minutes** and click **Finish**.</span></span>

    <span data-ttu-id="708eb-683">![Yük testi süre 5 dakika olarak ayarlama](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "yük testi süre 5 dakika olarak ayarlama")</span><span class="sxs-lookup"><span data-stu-id="708eb-683">![Setting the load test duration to 5 minutes](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "Setting the load test duration to 5 minutes")</span></span>

    <span data-ttu-id="708eb-684">*Yük testi süre 5 dakika olarak ayarlama*</span><span class="sxs-lookup"><span data-stu-id="708eb-684">*Setting the load test duration to 5 minutes*</span></span>
20. <span data-ttu-id="708eb-685">İçinde **Çözüm Gezgini**, çift **Local.settings** test ayarları keşfetmek için dosya.</span><span class="sxs-lookup"><span data-stu-id="708eb-685">In **Solution Explorer**, double-click the **Local.settings** file to explore the test settings.</span></span> <span data-ttu-id="708eb-686">Varsayılan olarak, Visual Studio testleri çalıştırmak için yerel bilgisayarınıza kullanır.</span><span class="sxs-lookup"><span data-stu-id="708eb-686">By default, Visual Studio uses your local computer to run the tests.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-687">Alternatif olarak, bulut kullanılarak yük testleri çalıştırmak için test projenizi yapılandırabilirsiniz **Visual Studio Online (VSO)**.</span><span class="sxs-lookup"><span data-stu-id="708eb-687">Alternatively, you can configure your test project to run the load tests in the cloud using **Visual Studio Online (VSO)**.</span></span> <span data-ttu-id="708eb-688">VSO daha gerçekçi yük benzetim hizmeti sınama, CPU kapasitesini, kullanılabilir bellek ve ağ bant genişliği gibi yerel ortamınıza kısıtlamaları önleme bir bulut tabanlı yük sağlar.</span><span class="sxs-lookup"><span data-stu-id="708eb-688">VSO provides a cloud-based load testing service that simulates a more realistic load, avoiding local environment constraints like CPU capacity, available memory and network bandwidth.</span></span> <span data-ttu-id="708eb-689">Yük testleri çalıştırmak için VSO kullanma hakkında daha fazla bilgi için bkz: [bu makalede](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span><span class="sxs-lookup"><span data-stu-id="708eb-689">For more information about using VSO to run load tests, see [this article](https://www.visualstudio.com/get-started/load-test-your-app-vs).</span></span>

    ![Test ayarları](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a><span data-ttu-id="708eb-691">Görev 3 – otomatik ölçeklendirme doğrulama</span><span class="sxs-lookup"><span data-stu-id="708eb-691">Task 3 – Autoscale Verification</span></span>

<span data-ttu-id="708eb-692">Şimdi, önceki görevde oluşturduğunuz yük testi yürütün ve yük altında web uygulamanızı nasıl davranacağını bakın.</span><span class="sxs-lookup"><span data-stu-id="708eb-692">You will now execute the load test you created in the previous task and see how your web app behaves under load.</span></span>

1. <span data-ttu-id="708eb-693">İçinde **Çözüm Gezgini**, çift **LoadTest1.loadtest** yük testi açın.</span><span class="sxs-lookup"><span data-stu-id="708eb-693">In **Solution Explorer**, double-click **LoadTest1.loadtest** to open the load test.</span></span>

    <span data-ttu-id="708eb-694">![LoadTest1.loadtest açma](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "LoadTest1.loadtest açma")</span><span class="sxs-lookup"><span data-stu-id="708eb-694">![Opening LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "Opening LoadTest1.loadtest")</span></span>

    <span data-ttu-id="708eb-695">*Opening LoadTest1.loadtest*</span><span class="sxs-lookup"><span data-stu-id="708eb-695">*Opening LoadTest1.loadtest*</span></span>
2. <span data-ttu-id="708eb-696">İçinde **LoadTest1.loadtest** penceresinde, yükleme testini çalıştırmak için araç kutusu ilk düğmesini tıklatın.</span><span class="sxs-lookup"><span data-stu-id="708eb-696">In the **LoadTest1.loadtest** window, click the first button in the toolbox to run the load test.</span></span>

    <span data-ttu-id="708eb-697">![Yük testi çalıştırma](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "yük testi çalıştırma")</span><span class="sxs-lookup"><span data-stu-id="708eb-697">![Running the load test](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "Running the load test")</span></span>

    <span data-ttu-id="708eb-698">*Yük testi çalıştırma*</span><span class="sxs-lookup"><span data-stu-id="708eb-698">*Running the load test*</span></span>
3. <span data-ttu-id="708eb-699">Yük testi tamamlanana dek bekleyin.</span><span class="sxs-lookup"><span data-stu-id="708eb-699">Wait until the load test completes.</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-700">Yük testi istekleri göndermek için web uygulaması aynı anda birden çok kullanıcı benzetimini yapar.</span><span class="sxs-lookup"><span data-stu-id="708eb-700">The load test simulates multiple users that send requests to the web app simultaneously.</span></span> <span data-ttu-id="708eb-701">Test çalıştırılırken, hatalar, uyarılar veya yükleme testi için ilgili diğer bilgileri algılamak için kullanılabilir sayaçlar izleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="708eb-701">When the test is running, you can monitor the available counters to detect any errors, warnings or other information related to your load test run.</span></span>

    <span data-ttu-id="708eb-702">![Yük testi çalıştırma](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "yük testi tamamlanıncaya kadar bekleniyor")</span><span class="sxs-lookup"><span data-stu-id="708eb-702">![Load test running](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "Waiting until the load test completes")</span></span>

    <span data-ttu-id="708eb-703">*Yük testi çalıştırma*</span><span class="sxs-lookup"><span data-stu-id="708eb-703">*Load test running*</span></span>
4. <span data-ttu-id="708eb-704">Test tamamlandıktan sonra Yönetim Portalı'na geri dönün ve gidin **ölçek** sayfası, web uygulamanızın.</span><span class="sxs-lookup"><span data-stu-id="708eb-704">Once the test completes, go back to the management portal and navigate to the **Scale** page of your web app.</span></span> <span data-ttu-id="708eb-705">Altında **kapasite** bölümünde, yeni bir örneğini otomatik olarak dağıtılan grafikte görmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="708eb-705">Under the **capacity** section, you should see in the graph that a new instance was automatically deployed.</span></span>

    ![Otomatik olarak dağıtılan yeni örneği](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    <span data-ttu-id="708eb-707">*Otomatik olarak dağıtılan yeni örneği*</span><span class="sxs-lookup"><span data-stu-id="708eb-707">*New instance automatically deployed*</span></span>

    > [!NOTE]
    > <span data-ttu-id="708eb-708">Değişikliklerin grafikte görünmesi birkaç dakika sürebilir (basın **CTRL + F5** düzenli aralıklarla sayfayı yenilemek için).</span><span class="sxs-lookup"><span data-stu-id="708eb-708">It may take several minutes for the changes to appear in the graph (press **CTRL + F5** periodically to refresh the page).</span></span> <span data-ttu-id="708eb-709">Herhangi bir değişiklik yoksa aşağıdakileri deneyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="708eb-709">If you do not see any changes, you can try the following:</span></span>
    > 
    > - <span data-ttu-id="708eb-710">Yük testi süresini artırın (örneğin için **10 dakika**)</span><span class="sxs-lookup"><span data-stu-id="708eb-710">Increase the duration of the load test (e.g. to **10 minutes**)</span></span>
    > - <span data-ttu-id="708eb-711">Maksimum ve minimum değerler, azaltmak **hedef CPU** web uygulamanız otomatik ölçeklendirme yapılandırmasını aralığı</span><span class="sxs-lookup"><span data-stu-id="708eb-711">Reduce the maximum and minimum values of the **Target CPU** range in the Autoscale configuration of your web app</span></span>
    > - <span data-ttu-id="708eb-712">Yük testi ile bulutta çalıştırmak **Visual Studio Online**.</span><span class="sxs-lookup"><span data-stu-id="708eb-712">Run the load test in the cloud with **Visual Studio Online**.</span></span> <span data-ttu-id="708eb-713">Daha fazla bilgi [burada](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span><span class="sxs-lookup"><span data-stu-id="708eb-713">More information [here](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)</span></span>

* * *

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="708eb-714">Özet</span><span class="sxs-lookup"><span data-stu-id="708eb-714">Summary</span></span>

<span data-ttu-id="708eb-715">Bu uygulamalı laboratuar ortamında ayarlama ve üretim web uygulamaları Azure uygulamanızı dağıtmak öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="708eb-715">In this hands-on lab, you learned how to set up and deploy your application to production web apps in Azure.</span></span> <span data-ttu-id="708eb-716">Algılama ve kullanarak veritabanlarınız güncelleştirme başlattığınız **Entity Framework Code First Migrations**, site kullanarak, yeni sürümlerini dağıtarak devam **Git** ve düzeyine gerçekleştirme siteniz en son kararlı sürümü.</span><span class="sxs-lookup"><span data-stu-id="708eb-716">You started by detecting and updating your databases using **Entity Framework Code First Migrations**, then continued by deploying new versions of your site using **Git** and performing rollbacks to the latest stable version of your site.</span></span> <span data-ttu-id="708eb-717">Ayrıca, bir Blob kapsayıcısına, statik içerik taşımak için depolama birimi kullanarak uygulamanızı ölçeklendirmek nasıl öğrendiniz.</span><span class="sxs-lookup"><span data-stu-id="708eb-717">Additionally, you learned how to scale your app using Storage to move your static content to a Blob container.</span></span>
