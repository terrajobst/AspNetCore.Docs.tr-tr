---
uid: visual-studio/overview/2013/release-notes
title: ASP.NET ve Web Araçları Visual Studio 2013 sürüm notları | Microsoft Docs
author: microsoft
description: Bu belgede, ASP.NET ve Web Araçları Visual Studio 2013 için sürüm açıklanmaktadır.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08815768-2702-42ae-ae85-0a59934a11d1
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/release-notes
msc.type: authoredcontent
ms.openlocfilehash: e9ddd96f186564834ff6bb2c30cf0ed5444cbf1b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877807"
---
<a name="aspnet-and-web-tools-for-visual-studio-2013-release-notes"></a><span data-ttu-id="75cc9-103">ASP.NET ve Web Araçları Visual Studio 2013 sürüm notları</span><span class="sxs-lookup"><span data-stu-id="75cc9-103">ASP.NET and Web Tools for Visual Studio 2013 Release Notes</span></span>
====================
<span data-ttu-id="75cc9-104">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="75cc9-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="75cc9-105">Bu belgede, ASP.NET ve Web Araçları Visual Studio 2013 için sürüm açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-105">This document describes the release of ASP.NET and Web Tools for Visual Studio 2013.</span></span>


## <a name="contents"></a><span data-ttu-id="75cc9-106">İçindekiler</span><span class="sxs-lookup"><span data-stu-id="75cc9-106">Contents</span></span>

- [<span data-ttu-id="75cc9-107">Yükleme notları</span><span class="sxs-lookup"><span data-stu-id="75cc9-107">Installation Notes</span></span>](#TOC1)
- [<span data-ttu-id="75cc9-108">Belgeler</span><span class="sxs-lookup"><span data-stu-id="75cc9-108">Documentation</span></span>](#TOC2)
- [<span data-ttu-id="75cc9-109">Yazılım gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="75cc9-109">Software Requirements</span></span>](#TOC4)

### <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a><span data-ttu-id="75cc9-110">ASP.NET ve Web Araçları Visual Studio 2013 için yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="75cc9-110">New Features in ASP.NET and Web Tools for Visual Studio 2013</span></span>

- [<span data-ttu-id="75cc9-111">One ASP.NET</span><span class="sxs-lookup"><span data-stu-id="75cc9-111">One ASP.NET</span></span>](#TOC6)
- [<span data-ttu-id="75cc9-112">Yeni Web projesi deneyimi</span><span class="sxs-lookup"><span data-stu-id="75cc9-112">New Web Project Experience</span></span>](#newproj)
- [<span data-ttu-id="75cc9-113">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="75cc9-113">ASP.NET Scaffolding</span></span>](#scaffold)
- [<span data-ttu-id="75cc9-114">Tarayıcı Bağlantısı</span><span class="sxs-lookup"><span data-stu-id="75cc9-114">Browser Link</span></span>](#browser-link)
- [<span data-ttu-id="75cc9-115">Visual Studio Web Düzenleyicisi geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="75cc9-115">Visual Studio Web Editor Enhancements</span></span>](#web-editor)
- [<span data-ttu-id="75cc9-116">Visual Studio'da Azure App Service Web Apps desteği</span><span class="sxs-lookup"><span data-stu-id="75cc9-116">Azure App Service Web Apps Support in Visual Studio</span></span>](#waws)
- [<span data-ttu-id="75cc9-117">Web yayımlama geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="75cc9-117">Web Publish Enhancements</span></span>](#publish)
- [<span data-ttu-id="75cc9-118">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="75cc9-118">NuGet 2.7</span></span>](#nuget)
- [<span data-ttu-id="75cc9-119">ASP.NET Web formları</span><span class="sxs-lookup"><span data-stu-id="75cc9-119">ASP.NET Web Forms</span></span>](#TOC9)
- [<span data-ttu-id="75cc9-120">ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="75cc9-120">ASP.NET MVC 5</span></span>](#TOC10)
- [<span data-ttu-id="75cc9-121">ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="75cc9-121">ASP.NET Web API 2</span></span>](#TOC11)
- [<span data-ttu-id="75cc9-122">ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="75cc9-122">ASP.NET SignalR</span></span>](#TOC13)
- [<span data-ttu-id="75cc9-123">ASP.NET kimliği</span><span class="sxs-lookup"><span data-stu-id="75cc9-123">ASP.NET Identity</span></span>](#TOC8)
- [<span data-ttu-id="75cc9-124">Microsoft OWIN bileşenleri</span><span class="sxs-lookup"><span data-stu-id="75cc9-124">Microsoft OWIN Components</span></span>](#TOC7)
- [<span data-ttu-id="75cc9-125">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="75cc9-125">Entity Framework 6</span></span>](#ef6)
- [<span data-ttu-id="75cc9-126">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="75cc9-126">ASP.NET Razor 3</span></span>](#TOC14)
- [<span data-ttu-id="75cc9-127">ASP.NET uygulama askıya alma</span><span class="sxs-lookup"><span data-stu-id="75cc9-127">ASP.NET App Suspend</span></span>](#TOC15)
- [<span data-ttu-id="75cc9-128">Bilinen sorunlar ve yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="75cc9-128">Known Issues and Breaking Changes</span></span>](#knownissues)

<a id="TOC1"></a>
## <a name="installation-notes"></a><span data-ttu-id="75cc9-129">Yükleme notları</span><span class="sxs-lookup"><span data-stu-id="75cc9-129">Installation Notes</span></span>

<span data-ttu-id="75cc9-130">ASP.NET ve Web Araçları Visual Studio 2013 için ana yükleyicisinde paketlenmiştir ve indirilebilir [burada](https://www.asp.net/downloads).</span><span class="sxs-lookup"><span data-stu-id="75cc9-130">ASP.NET and Web Tools for Visual Studio 2013 are bundled in the main installer and can be downloaded [here](https://www.asp.net/downloads).</span></span>

<a id="TOC2"></a>
## <a name="documentation"></a><span data-ttu-id="75cc9-131">Belgeler</span><span class="sxs-lookup"><span data-stu-id="75cc9-131">Documentation</span></span>

<span data-ttu-id="75cc9-132">Öğreticiler ve Visual Studio 2013 için ASP.NET ve Web Araçları hakkında diğer bilgi web'da [ASP.NET web sitesi](https://www.asp.net/).</span><span class="sxs-lookup"><span data-stu-id="75cc9-132">Tutorials and other information about ASP.NET and Web Tools for Visual Studio 2013 are available from the [ASP.NET web site](https://www.asp.net/).</span></span>

<a id="TOC4"></a>
## <a name="software-requirements"></a><span data-ttu-id="75cc9-133">Yazılım Gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="75cc9-133">Software Requirements</span></span>

<span data-ttu-id="75cc9-134">ASP.NET ve Web Araçları, Visual Studio 2013 gerektirir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-134">ASP.NET and Web Tools requires Visual Studio 2013.</span></span>

<a id="TOC5"></a>
## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-2013"></a><span data-ttu-id="75cc9-135">ASP.NET ve Web Araçları Visual Studio 2013 için yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="75cc9-135">New Features in ASP.NET and Web Tools for Visual Studio 2013</span></span>

<span data-ttu-id="75cc9-136">Aşağıdaki bölümlerde sürümünde tanıtılan özellikleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-136">The following sections describe the features that have been introduced in the release.</span></span>

<a id="TOC6"></a>
## <a name="one-aspnet"></a><span data-ttu-id="75cc9-137">One ASP.NET</span><span class="sxs-lookup"><span data-stu-id="75cc9-137">One ASP.NET</span></span>

<span data-ttu-id="75cc9-138">Visual Studio 2013 sürümüyle, biz kolayca karıştırmak ve istediğiniz olanlarla eşleşmesi ASP.NET teknolojilerini kullanarak deneyimi birleştirin doğru bir adım gerçekleştirmişsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-138">With the release of Visual Studio 2013, we have taken a step towards unifying the experience of using ASP.NET technologies, so that you can easily mix and match the ones you want.</span></span> <span data-ttu-id="75cc9-139">Örneğin, MVC kullanarak bir proje başlatın ve kolayca Web formları sayfaları daha sonra projeye ekleyin veya bir Web Forms proje ile Web API iskelesi.</span><span class="sxs-lookup"><span data-stu-id="75cc9-139">For example, you can start a project using MVC and easily add Web Forms pages to the project later, or scaffold Web APIs in a Web Forms project.</span></span> <span data-ttu-id="75cc9-140">Bir ASP.NET tümünü sizin için ASP.NET sevdiğiniz şeyleri gerçekleştirmek için bir geliştirici olarak kolaylaştırarak hakkında ' dir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-140">One ASP.NET is all about making it easier for you as a developer to do the things you love in ASP.NET.</span></span> <span data-ttu-id="75cc9-141">Seçtiğiniz hangi teknoloji olsun, bir ASP.NET arka plandaki güvenilen çerçevesinin oluşturmakta olduğunuz güvenirlik olabilir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-141">No matter what technology you choose, you can have confidence that you are building on the trusted underlying framework of One ASP.NET.</span></span>

<a id="newproj"></a>
## <a name="new-web-project-experience"></a><span data-ttu-id="75cc9-142">Yeni Web projesi deneyimi</span><span class="sxs-lookup"><span data-stu-id="75cc9-142">New Web Project Experience</span></span>

<span data-ttu-id="75cc9-143">Biz, Visual Studio 2013'te yeni web projeleri oluşturma deneyimi Gelişmiş.</span><span class="sxs-lookup"><span data-stu-id="75cc9-143">We have enhanced the experience of creating new web projects in Visual Studio 2013.</span></span> <span data-ttu-id="75cc9-144">İçinde **yeni ASP.NET Web projesi** iletişim, istediğiniz, herhangi bir bileşimini teknolojileri (Web Forms, MVC, Web API) yapılandırma, kimlik doğrulama seçeneklerini yapılandırmak ve birim testi projesi eklemek proje türü seçebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-144">In the **New ASP.NET Web Project** dialog you can select the project type you want, configure any combination of technologies (Web Forms, MVC, Web API), configure authentication options, and add a unit test project.</span></span>

![New ASP.NET Project](release-notes/_static/image1.png)

<span data-ttu-id="75cc9-146">Yeni iletişim kutusu şablonları birçoğu için varsayılan kimlik doğrulama seçenekleri değiştirmenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-146">The new dialog enables you to change the default authentication options for many of the templates.</span></span> <span data-ttu-id="75cc9-147">Örneğin, bir ASP.NET Web Forms projesi oluşturduğunuzda, aşağıdaki seçeneklerden birini seçebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="75cc9-147">For example, when you create an ASP.NET Web Forms project you can select any of the following options:</span></span>

- <span data-ttu-id="75cc9-148">Kimlik doğrulaması yok</span><span class="sxs-lookup"><span data-stu-id="75cc9-148">No Authentication</span></span>
- <span data-ttu-id="75cc9-149">Bireysel kullanıcı hesapları (ASP.NET üyelik veya sosyal sağlayıcısı günlüğüne)</span><span class="sxs-lookup"><span data-stu-id="75cc9-149">Individual User Accounts (ASP.NET membership or social provider log in)</span></span>
- <span data-ttu-id="75cc9-150">Kurumsal hesaplar (Internet uygulamasını Active Directory'de)</span><span class="sxs-lookup"><span data-stu-id="75cc9-150">Organizational Accounts (Active Directory in an internet application)</span></span>
- <span data-ttu-id="75cc9-151">Windows kimlik doğrulaması (Active Directory içinde bir intranet uygulaması)</span><span class="sxs-lookup"><span data-stu-id="75cc9-151">Windows Authentication (Active Directory in an intranet application)</span></span>

![Kimlik doğrulama seçenekleri](release-notes/_static/image2.png)

<span data-ttu-id="75cc9-153">Web projeleri oluşturmak için yeni işlem hakkında daha fazla bilgi için bkz: [Visual Studio 2013'da ASP.NET Web projeleri oluşturma](creating-web-projects-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="75cc9-153">For more information about the new process for creating web projects, see [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span></span> <span data-ttu-id="75cc9-154">Yeni kimlik doğrulama seçenekleri hakkında daha fazla bilgi için bkz: [ASP.NET Identity](#TOC8) belgesinde.</span><span class="sxs-lookup"><span data-stu-id="75cc9-154">For more information about the new authentication options, see [ASP.NET Identity](#TOC8) later in this document.</span></span>

<a id="scaffold"></a>
## <a name="aspnet-scaffolding"></a><span data-ttu-id="75cc9-155">ASP.NET Scaffolding</span><span class="sxs-lookup"><span data-stu-id="75cc9-155">ASP.NET Scaffolding</span></span>

<span data-ttu-id="75cc9-156">ASP.NET İskele bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-156">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="75cc9-157">Bir veri modeli ile etkileşime giren projeniz Demirbaş kod eklemek kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-157">It makes it easy to add boilerplate code to your project that interacts with a data model.</span></span>

<span data-ttu-id="75cc9-158">Visual Studio'nun önceki sürümleri yapı iskelesi ASP.NET MVC projelerine sınırlı.</span><span class="sxs-lookup"><span data-stu-id="75cc9-158">In previous versions of Visual Studio, scaffolding was limited to ASP.NET MVC projects.</span></span> <span data-ttu-id="75cc9-159">Visual Studio 2013 ile Web Forms dahil olmak üzere tüm ASP.NET projesi için yapı iskelesi artık kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-159">With Visual Studio 2013, you can now use scaffolding for any ASP.NET project, including Web Forms.</span></span> <span data-ttu-id="75cc9-160">Visual Studio 2013 oluşturma sayfaları şu anda Web Forms projesi için desteklemiyor, ancak yapı iskelesi ile Web Forms projeye MVC bağımlılıkları ekleyerek kullanmaya devam edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-160">Visual Studio 2013 does not currently support generating pages for a Web Forms project, but you can still use scaffolding with Web Forms by adding MVC dependencies to the project.</span></span> <span data-ttu-id="75cc9-161">İçin Web formları sayfaları oluşturmaya yönelik destek gelecek bir güncelleştirmede eklenir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-161">Support for generating pages for Web Forms will be added in a future update.</span></span>

<span data-ttu-id="75cc9-162">Yapı iskelesi kullanırken, tüm gerekli bağımlılıkların projede yüklü olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="75cc9-162">When using scaffolding, we ensure that all required dependencies are installed in the project.</span></span> <span data-ttu-id="75cc9-163">Bir ASP.NET Web Forms projeyle başlatın ve sonra bir Web API denetleyicisi eklemek için yapı iskelesi kullanın, örneğin, gerekli NuGet paket ve başvuruları projenize otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-163">For example, if you start with an ASP.NET Web Forms project and then use scaffolding to add a Web API Controller, the required NuGet packages and references are added to your project automatically.</span></span>

<span data-ttu-id="75cc9-164">Bir Web Forms projeye MVC yapı iskelesi eklemek için Ekle bir **yeni iskele kurulmuş öğe** seçip **MVC 5 bağımlılıkları** iletişim penceresinde.</span><span class="sxs-lookup"><span data-stu-id="75cc9-164">To add MVC scaffolding to a Web Forms project, add a **New Scaffolded Item** and select **MVC 5 Dependencies** in the dialog window.</span></span> <span data-ttu-id="75cc9-165">MVC iskele için iki seçenek vardır; Minimal ve tam.</span><span class="sxs-lookup"><span data-stu-id="75cc9-165">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="75cc9-166">En az seçerseniz, yalnızca NuGet paketlerini ve ASP.NET MVC için başvuruları projenize eklenir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-166">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="75cc9-167">Tam seçeneğini belirlerseniz MVC projesinde gerekli içerik dosyaların yanı sıra en az bağımlılıkları eklenir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-167">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span>

<span data-ttu-id="75cc9-168">Zaman uyumsuz denetleyicileri iskele desteği yeni Entity Framework 6 zaman uyumsuz özelliklerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-168">Support for scaffolding async controllers uses the new async features from Entity Framework 6.</span></span>

<span data-ttu-id="75cc9-169">Daha fazla bilgi ve öğreticiler için bkz: [ASP.NET yapı İskelesi genel bakış](aspnet-scaffolding-overview.md).</span><span class="sxs-lookup"><span data-stu-id="75cc9-169">For more information and tutorials, see [ASP.NET Scaffolding Overview](aspnet-scaffolding-overview.md).</span></span>

<a id="browser-link"></a>
## <a name="browser-link--signalr-channel-between-browser-and-visual-studio"></a><span data-ttu-id="75cc9-170">Tarayıcı bağlantısı – tarayıcı ve Visual Studio arasında SignalR kanal</span><span class="sxs-lookup"><span data-stu-id="75cc9-170">Browser Link – SignalR channel between browser and Visual Studio</span></span>

<span data-ttu-id="75cc9-171">Yeni [tarayıcı bağlantısı](using-browser-link.md) özelliği, birden çok tarayıcı Visual Studio'ya bağlanın ve tüm bir araç çubuğu düğmesini tıklatarak yenileme olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-171">The new [Browser Link](using-browser-link.md) feature lets you connect multiple browsers to Visual Studio and refresh them all by clicking a button in the toolbar.</span></span> <span data-ttu-id="75cc9-172">Birden çok tarayıcı mobil öykünücülerinden dahil olmak üzere, geliştirme siteye bağlayabilir ve tüm aynı anda tüm tarayıcılar yenileme için Yenile'yi tıklatın.</span><span class="sxs-lookup"><span data-stu-id="75cc9-172">You can connect multiple browsers to your development site, including mobile emulators, and click refresh to refresh all the browsers all at the same time.</span></span> <span data-ttu-id="75cc9-173">Tarayıcı bağlantısı da geliştiricilerin tarayıcı bağlantısı uzantıları yazmak için bir API sunar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-173">Browser Link also exposes an API to enable developers to write Browser Link extensions.</span></span>

![](release-notes/_static/image3.png)

<span data-ttu-id="75cc9-174">Tarayıcı bağlantısı API'sini yararlanmak geliştiricilere etkinleştirerek, Visual Studio ile bağlı herhangi bir tarayıcıda arasında sınırları kestiği çok Gelişmiş senaryolar oluşturmak mümkün olur.</span><span class="sxs-lookup"><span data-stu-id="75cc9-174">By enabling developers to take advantage of the Browser Link API, it becomes possible to create very advanced scenarios that crosses boundaries between Visual Studio and any browser that's connected.</span></span> <span data-ttu-id="75cc9-175">Web Essentials Visual Studio ve tarayıcının geliştirici araçları, mobil öykünücülerinden ve çok daha fazlasını denetleme uzak arasında tümleşik bir deneyim oluşturmak için API avantajlarından yararlanır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-175">Web Essentials takes advantage of the API to create an integrated experience between Visual Studio and the browser's developer tools, remote controlling mobile emulators and a lot more.</span></span>

<a id="web-editor"></a>
## <a name="visual-studio-web-editor-enhancements"></a><span data-ttu-id="75cc9-176">Visual Studio Web Düzenleyicisi geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="75cc9-176">Visual Studio Web Editor Enhancements</span></span>

<span data-ttu-id="75cc9-177">Visual Studio 2013 Razor dosyaları ve HTML dosyaları için yeni bir HTML düzenleyicisi web uygulamalarını içerir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-177">Visual Studio 2013 includes a new HTML editor for Razor files and HTML files in web applications.</span></span> <span data-ttu-id="75cc9-178">Yeni HTML düzenleyicisi üzerinde HTML5 dayalı tek bir birleşik şema sağlar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-178">The new HTML editor provides a single unified schema based on HTML5.</span></span> <span data-ttu-id="75cc9-179">Otomatik küme parantezi tamamlama, jQuery UI ve IntelliSense özniteliği, IntelliSense gruplandırma, kimlik ve sınıf adı IntelliSense ve daha iyi performans, biçimlendirme dahil olmak üzere diğer geliştirmeler özniteliği AngularJS sahiptir ve akıllı etiketler.</span><span class="sxs-lookup"><span data-stu-id="75cc9-179">It has automatic brace completion, jQuery UI and AngularJS attribute IntelliSense, attribute IntelliSense Grouping, ID and class name Intellisense, and other improvements including better performance, formatting and SmartTags.</span></span>

<span data-ttu-id="75cc9-180">Aşağıdaki ekran görüntüsü, önyükleme özniteliği IntelliSense HTML Düzenleyicisi'ni kullanarak gösterir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-180">The following screenshot demonstrates using Bootstrap attribute IntelliSense in the HTML editor.</span></span>

![HTML Düzenleyicisi'nde IntelliSense](release-notes/_static/image4.png)

<span data-ttu-id="75cc9-182">Visual Studio 2013 de yerleşik düzenleyicileri daha az hem CoffeeScript ile birlikte gelir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-182">Visual Studio 2013 also comes with both CoffeeScript and LESS editors built in.</span></span> <span data-ttu-id="75cc9-183">LESS düzenleyici ile tüm harika özellikler CSS Düzenleyicisi'nden gelen ve değişkenleri ve mixins için belirli IntelliSense belgelerde daha az sahip @import zinciri.</span><span class="sxs-lookup"><span data-stu-id="75cc9-183">The LESS editor comes with all the cool features from the CSS editor and has specific Intellisense for variables and mixins across all the LESS documents in the @import chain.</span></span>

<a id="waws"></a>
## <a name="azure-app-service-web-apps-support-in-visual-studio"></a><span data-ttu-id="75cc9-184">Visual Studio'da Azure App Service Web Apps desteği</span><span class="sxs-lookup"><span data-stu-id="75cc9-184">Azure App Service Web Apps Support in Visual Studio</span></span>

<span data-ttu-id="75cc9-185">Visual Studio 2013'te .NET 2.2 için Azure SDK'sı ile kullandığınız **Sunucu Gezgini** doğrudan uzak web uygulamalarınızı ile etkileşim kurmak için.</span><span class="sxs-lookup"><span data-stu-id="75cc9-185">In Visual Studio 2013 with the Azure SDK for .NET 2.2, you can use **Server Explorer** to interact directly with your remote web apps.</span></span> <span data-ttu-id="75cc9-186">Azure hesabınızda oturum açın, yeni web uygulamaları oluşturmak, uygulama yapılandırma, gerçek zamanlı günlükleri ve daha fazla bilgi görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="75cc9-186">You can sign in to your Azure account, create new web apps, configure apps, view real-time logs, and more.</span></span> <span data-ttu-id="75cc9-187">SDK 2.2 hemen sonra gelen yayımlanan, Azure'nın uzaktan hata ayıklama modunda çalıştırmak kullanabileceksiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-187">Coming soon after SDK 2.2 is released, you'll be able to run in debug mode remotely in Azure.</span></span> <span data-ttu-id="75cc9-188">.NET için Azure SDK'sının geçerli sürüm yüklediğinizde, Azure App Service Web Apps için yeni özelliklerin çoğunu Visual Studio 2012'de de çalışır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-188">Most of the new features for Azure App Service Web Apps also work in Visual Studio 2012 when you install the current release of the Azure SDK for .NET.</span></span>

<span data-ttu-id="75cc9-189">Daha fazla bilgi için aşağıdaki kaynaklara bakın:</span><span class="sxs-lookup"><span data-stu-id="75cc9-189">For more information, see the following resources:</span></span>

- [<span data-ttu-id="75cc9-190">Azure App Service'te bir ASP.NET web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="75cc9-190">Create an ASP.NET web app in Azure App Service</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)
- [<span data-ttu-id="75cc9-191">Visual Studio kullanarak Azure App Service web uygulamasında sorun giderme</span><span class="sxs-lookup"><span data-stu-id="75cc9-191">Troubleshoot a web app in Azure App Service using Visual Studio</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)

<a id="publish"></a>
## <a name="web-publish-enhancements"></a><span data-ttu-id="75cc9-192">Web yayımlama geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="75cc9-192">Web Publish Enhancements</span></span>

<span data-ttu-id="75cc9-193">Visual Studio 2013 yeni ve geliştirilmiş Web yayımlama özellikleri içerir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-193">Visual Studio 2013 includes new and enhanced Web Publish features.</span></span> <span data-ttu-id="75cc9-194">Birkaç tanesi aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="75cc9-194">Here are a few of them:</span></span>

- <span data-ttu-id="75cc9-195">Kolayca [Web.config dosyasını şifreleme otomatikleştirmek](https://go.microsoft.com/fwlink/?LinkId=325529).</span><span class="sxs-lookup"><span data-stu-id="75cc9-195">Easily [automate Web.config file encryption](https://go.microsoft.com/fwlink/?LinkId=325529).</span></span> <span data-ttu-id="75cc9-196">(Bu bağlantı ve aşağıdaki iki kadar günde 10/17 aşamalarda kullanılamayabilir MSDN'de belgelerin üzerine gelin.)</span><span class="sxs-lookup"><span data-stu-id="75cc9-196">(This link and the following two point to documentation on MSDN that might not be available until late in the day on 10/17.)</span></span>
- <span data-ttu-id="75cc9-197">Kolayca [uygulama dağıtımı sırasında çevrimdışı duruma getirmeden otomatikleştirmek](https://go.microsoft.com/fwlink/?LinkId=325530).</span><span class="sxs-lookup"><span data-stu-id="75cc9-197">Easily [automate taking an application offline during deployment](https://go.microsoft.com/fwlink/?LinkId=325530).</span></span>
- <span data-ttu-id="75cc9-198">Web dağıtımı için yapılandırma [son değiştirilen tarihi yerine dosya sağlama toplamı kullanmak](https://go.microsoft.com/fwlink/?LinkId=325531) hangi dosyaların sunucusuna kopyalanması belirlemek için.</span><span class="sxs-lookup"><span data-stu-id="75cc9-198">Configure Web Deploy to [use file checksum instead of last-changed date](https://go.microsoft.com/fwlink/?LinkId=325531) for determining which files should be copied to the server.</span></span>
- <span data-ttu-id="75cc9-199">Hızlı bir şekilde (Web.config dahil) tek tek seçilen dosyaları FTP kullanıyorsanız veya dosya sistemi yayımladığınızda yöntemleri yanı sıra Web dağıtımı ile yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="75cc9-199">Quickly publish individual selected files (including Web.config) when you're using the FTP or file system publish methods as well as with Web Deploy.</span></span>

<span data-ttu-id="75cc9-200">ASP.NET web dağıtımı hakkında daha fazla bilgi için bkz: [ASP.NET sitesi](https://go.microsoft.com/fwlink/?LinkId=322027).</span><span class="sxs-lookup"><span data-stu-id="75cc9-200">For more information about ASP.NET web deployment, see [the ASP.NET site](https://go.microsoft.com/fwlink/?LinkId=322027).</span></span>

<a id="nuget"></a>
## <a name="nuget-27"></a><span data-ttu-id="75cc9-201">NuGet 2.7</span><span class="sxs-lookup"><span data-stu-id="75cc9-201">NuGet 2.7</span></span>

<span data-ttu-id="75cc9-202">NuGet 2.7 yakından açıklanan yeni özellikleri zengin bir dizi içeren [NuGet 2.7 Sürüm Notları](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span><span class="sxs-lookup"><span data-stu-id="75cc9-202">NuGet 2.7 includes a rich set of new features which are described in detail at [NuGet 2.7 Release Notes](http://docs.nuget.org/docs/release-notes/nuget-2.7).</span></span>

<span data-ttu-id="75cc9-203">Bu sürümü NuGet paketlerini indirmek NuGet paket geri yükleme özelliği için açık izin sağlamak için gereken de kaldırır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-203">This version of NuGet also removes the need to provide explicit consent for NuGet's package restore feature to download packages.</span></span> <span data-ttu-id="75cc9-204">İzin (ve ilişkili onay NuGet Tercihleri iletişim kutusuna) NuGet yükleyerek şimdi verilir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-204">Consent (and the associated checkbox in NuGet's preferences dialog) is now granted by installing NuGet.</span></span> <span data-ttu-id="75cc9-205">Şimdi paket geri yükleme, yalnızca varsayılan olarak çalışır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-205">Now package restore simply works by default.</span></span>

<a id="TOC9"></a>
## <a name="aspnet-web-forms"></a><span data-ttu-id="75cc9-206">ASP.NET Web Forms</span><span class="sxs-lookup"><span data-stu-id="75cc9-206">ASP.NET Web Forms</span></span>

### <a name="one-aspnet"></a><span data-ttu-id="75cc9-207">One ASP.NET</span><span class="sxs-lookup"><span data-stu-id="75cc9-207">One ASP.NET</span></span>

<span data-ttu-id="75cc9-208">Web Forms proje şablonları ile yeni bir ASP.NET deneyimi sorunsuz şekilde tümleşir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-208">The Web Forms project templates integrate seamlessly with the new One ASP.NET experience.</span></span> <span data-ttu-id="75cc9-209">MVC ve Web API Web Forms projenize destekler ve bir ASP.NET proje oluşturma Sihirbazı'nı kullanarak kimlik doğrulaması yapılandırabilirsiniz ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-209">You can add MVC and Web API support to your Web Forms project, and you can configure authentication using the One ASP.NET project creation wizard.</span></span> <span data-ttu-id="75cc9-210">Daha fazla bilgi için bkz: [Visual Studio 2013'da ASP.NET Web projeleri oluşturma](creating-web-projects-in-visual-studio.md).</span><span class="sxs-lookup"><span data-stu-id="75cc9-210">For more information, see [Creating ASP.NET Web Projects in Visual Studio 2013](creating-web-projects-in-visual-studio.md).</span></span>

### <a name="aspnet-identity"></a><span data-ttu-id="75cc9-211">ASP.NET Kimlik</span><span class="sxs-lookup"><span data-stu-id="75cc9-211">ASP.NET Identity</span></span>

<span data-ttu-id="75cc9-212">Web Forms proje şablonları yeni ASP.NET Identity framework desteği.</span><span class="sxs-lookup"><span data-stu-id="75cc9-212">The Web Forms project templates support the new ASP.NET Identity framework.</span></span> <span data-ttu-id="75cc9-213">Ayrıca, şablonları artık Web Forms Intranet projesi oluşturulmasını destekler.</span><span class="sxs-lookup"><span data-stu-id="75cc9-213">In addition, the templates now support creation of a Web Forms intranet project.</span></span> <span data-ttu-id="75cc9-214">Daha fazla bilgi için bkz: [kimlik doğrulama yöntemleri](creating-web-projects-in-visual-studio.md#auth) içinde **Visual Studio 2013'da ASP.NET Web projeleri oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="75cc9-214">For more information, see [Authentication Methods](creating-web-projects-in-visual-studio.md#auth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="bootstrap"></a><span data-ttu-id="75cc9-215">önyükleme</span><span class="sxs-lookup"><span data-stu-id="75cc9-215">Bootstrap</span></span>

<span data-ttu-id="75cc9-216">Web Forms şablonlarını kullanma [önyükleme](http://twitter.github.io/bootstrap/) bir şık ve esnek kolayca özelleştirebileceğiniz görünüm sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="75cc9-216">The Web Forms templates use [Bootstrap](http://twitter.github.io/bootstrap/) to provide a sleek and responsive look and feel that you can easily customize.</span></span> <span data-ttu-id="75cc9-217">Daha fazla bilgi için bkz: [Visual Studio 2013 web projesi şablonları önyükleme](creating-web-projects-in-visual-studio.md#bootstrap).</span><span class="sxs-lookup"><span data-stu-id="75cc9-217">For more information, see [Bootstrap in the Visual Studio 2013 web project templates](creating-web-projects-in-visual-studio.md#bootstrap).</span></span>

<a id="TOC10"></a>
## <a name="aspnet-mvc-5"></a><span data-ttu-id="75cc9-218">ASP.NET MVC 5</span><span class="sxs-lookup"><span data-stu-id="75cc9-218">ASP.NET MVC 5</span></span>

### <a name="one-aspnet"></a><span data-ttu-id="75cc9-219">One ASP.NET</span><span class="sxs-lookup"><span data-stu-id="75cc9-219">One ASP.NET</span></span>

<span data-ttu-id="75cc9-220">Web MVC proje şablonları ile yeni bir ASP.NET deneyimi sorunsuz şekilde tümleşir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-220">The Web MVC project templates integrate seamlessly with the new One ASP.NET experience.</span></span> <span data-ttu-id="75cc9-221">MVC projenizi özelleştirebilir ve bir ASP.NET proje oluşturma Sihirbazı'nı kullanarak kimlik doğrulaması yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="75cc9-221">You can customize your MVC project and configure authentication using the One ASP.NET project creation wizard.</span></span> <span data-ttu-id="75cc9-222">Bir tanıtım öğretici ASP.NET MVC 5 bulunabilir [ASP.NET MVC 5 ile çalışmaya başlama](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="75cc9-222">An introductory tutorial to ASP.NET MVC 5 can be found at [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="75cc9-223">MVC 4 projeleri MVC 5'e yükseltme hakkında daha fazla bilgi için bkz: [bir ASP.NET MVC 4 ve Web API projesi ASP.NET MVC 5 ve Web API 2'ye yükseltme](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="75cc9-223">For information on upgrading MVC 4 projects to MVC 5, see [How to Upgrade an ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span></span>

### <a name="aspnet-identity"></a><span data-ttu-id="75cc9-224">ASP.NET Kimlik</span><span class="sxs-lookup"><span data-stu-id="75cc9-224">ASP.NET Identity</span></span>

<span data-ttu-id="75cc9-225">MVC proje şablonları, ASP.NET Identity kimlik doğrulama ve kimlik yönetimi için kullanmak üzere güncelleştirilmiştir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-225">The MVC project templates have been updated to use ASP.NET Identity for authentication and identity management.</span></span> <span data-ttu-id="75cc9-226">Facebook ve Google kimlik doğrulama ve yeni üyelik API'si gösteren bir öğretici bulunabilir [Facebook ve Google OAuth2 ve Openıd oturum açma ile bir ASP.NET MVC 5 uygulaması oluşturmak](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) ve [auth ile bir ASP.NET MVC uygulaması oluşturma ve SQL DB ve Azure App Service'e dağıtma](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="75cc9-226">A tutorial featuring Facebook and Google authentication and the new membership API can be found at [Create an ASP.NET MVC 5 App with Facebook and Google OAuth2 and OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) and [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/).</span></span>

### <a name="bootstrap"></a><span data-ttu-id="75cc9-227">önyükleme</span><span class="sxs-lookup"><span data-stu-id="75cc9-227">Bootstrap</span></span>

<span data-ttu-id="75cc9-228">MVC proje şablonu kullanmak için güncelleştirilmiş [önyükleme](http://getbootstrap.com/) bir şık ve esnek kolayca özelleştirebileceğiniz görünüm sağlamak için.</span><span class="sxs-lookup"><span data-stu-id="75cc9-228">The MVC project template has been updated to use [Bootstrap](http://getbootstrap.com/) to provide a sleek and responsive look and feel that you can easily customize.</span></span> <span data-ttu-id="75cc9-229">Daha fazla bilgi için bkz: [Visual Studio 2013 web projesi şablonları önyükleme](creating-web-projects-in-visual-studio.md#bootstrap).</span><span class="sxs-lookup"><span data-stu-id="75cc9-229">For more information, see [Bootstrap in the Visual Studio 2013 web project templates](creating-web-projects-in-visual-studio.md#bootstrap).</span></span>

### <a name="authentication-filters"></a><span data-ttu-id="75cc9-230">Kimlik doğrulaması filtreleri</span><span class="sxs-lookup"><span data-stu-id="75cc9-230">Authentication filters</span></span>

<span data-ttu-id="75cc9-231">Kimlik doğrulaması filtreleri olan yeni bir ASP.NET MVC ardışık düzenine yetkilendirme filtreleri önce çalıştıran ve kimlik doğrulama mantığı başına-eylemi belirtmenize olanak veren ASP.NET MVC filtre tür başına denetleyicisi, veya genel olarak tüm denetleyicileri için.</span><span class="sxs-lookup"><span data-stu-id="75cc9-231">Authentication filters are a new kind of filter in ASP.NET MVC that run prior to authorization filters in the ASP.NET MVC pipeline and allow you to specify authentication logic per-action, per-controller, or globally for all controllers.</span></span> <span data-ttu-id="75cc9-232">Kimlik doğrulaması filtreleri kimlik bilgileri isteği işlemek ve karşılık gelen asıl sağlayın.</span><span class="sxs-lookup"><span data-stu-id="75cc9-232">Authentication filters process credentials in the request and provide a corresponding principal.</span></span> <span data-ttu-id="75cc9-233">Kimlik doğrulaması filtreleri, kimlik doğrulama sınaması yetkisiz isteklerine yanıt olarak da ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-233">Authentication filters can also add authentication challenges in response to unauthorized requests.</span></span>

### <a name="filter-overrides"></a><span data-ttu-id="75cc9-234">Filtre geçersiz kılar</span><span class="sxs-lookup"><span data-stu-id="75cc9-234">Filter overrides</span></span>

<span data-ttu-id="75cc9-235">Şimdi bir geçersiz kılma filtresi belirterek belirli bir eylem yöntemini veya denetleyicileri için hangi filtre uygulamak geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-235">You can now override which filters apply to a given action method or controller by specifying an override filter.</span></span> <span data-ttu-id="75cc9-236">Geçersiz kılma filtreleri verilen kapsam (eylem veya denetleyici) için çalıştırılmamalıdır filtre türleri kümesi belirtin.</span><span class="sxs-lookup"><span data-stu-id="75cc9-236">Override filters specify a set of filter types that should not be run for a given scope (action or controller).</span></span> <span data-ttu-id="75cc9-237">Bu genel olarak geçerli ancak ardından belirli eylemler veya denetleyicileri uygulama bazı genel filtreleri hariç filtreleri yapılandırmanıza olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-237">This allows you to configure filters that apply globally but then exclude certain global filters from applying to specific actions or controllers.</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="75cc9-238">Öznitelik yönlendirme</span><span class="sxs-lookup"><span data-stu-id="75cc9-238">Attribute routing</span></span>

<span data-ttu-id="75cc9-239">ASP.NET MVC şimdi destekleyen bir katkı Tim McCall, yazarı tarafından sayesinde özniteliği yönlendirme [ http://attributerouting.net ](http://attributerouting.net).</span><span class="sxs-lookup"><span data-stu-id="75cc9-239">ASP.NET MVC now supports attribute routing, thanks to a contribution by Tim McCall, the author of [http://attributerouting.net](http://attributerouting.net).</span></span> <span data-ttu-id="75cc9-240">Öznitelik yönlendirme ile eylemlerin ve denetleyicilerin yorumlama yollarınızı belirtebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-240">With attribute routing you can specify your routes by annotating your actions and controllers.</span></span>

<a id="TOC11"></a>
## <a name="aspnet-web-api-2"></a><span data-ttu-id="75cc9-241">ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="75cc9-241">ASP.NET Web API 2</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="75cc9-242">Öznitelik yönlendirme</span><span class="sxs-lookup"><span data-stu-id="75cc9-242">Attribute routing</span></span>

<span data-ttu-id="75cc9-243">ASP.NET Web API artık destekliyor katkı Tim McCall, yazarı tarafından sayesinde özniteliği yönlendirme [ http://attributerouting.net ](http://attributerouting.net).</span><span class="sxs-lookup"><span data-stu-id="75cc9-243">ASP.NET Web API now supports attribute routing, thanks to a contribution by Tim McCall, the author of [http://attributerouting.net](http://attributerouting.net).</span></span> <span data-ttu-id="75cc9-244">Öznitelik yönlendirme ile Web API yollarınızı eylemleri ve bu gibi denetleyicileri yorumlama belirtebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="75cc9-244">With attribute routing you can specify your Web API routes by annotating your actions and controllers like this:</span></span>

[!code-csharp[Main](release-notes/samples/sample1.cs)]

<span data-ttu-id="75cc9-245">Öznitelik yönlendirme, web API URI'ler üzerinde daha fazla denetim sağlar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-245">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="75cc9-246">Örneğin, tek bir API denetleyicisi kullanarak bir kaynak hiyerarşi kolayca tanımlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="75cc9-246">For example, you can easily define a resource hierarchy using a single API controller:</span></span>

[!code-csharp[Main](release-notes/samples/sample2.cs)]

<span data-ttu-id="75cc9-247">Özniteliği de yönlendirme, isteğe bağlı parametreler, varsayılan değerleri ve rota kısıtlamaları belirtmek için kullanışlı bir sözdizimi sağlar:</span><span class="sxs-lookup"><span data-stu-id="75cc9-247">Attribute routing also provides a convenient syntax for specifying optional parameters, default values, and route constraints:</span></span>

[!code-csharp[Main](release-notes/samples/sample3.cs)]

<span data-ttu-id="75cc9-248">Öznitelik yönlendirme hakkında daha fazla bilgi için bkz: [özniteliği yönlendirme Web API 2'deki](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="75cc9-248">For more information about attribute routing, see [Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2.md).</span></span>

### <a name="oauth-20"></a><span data-ttu-id="75cc9-249">OAuth 2.0</span><span class="sxs-lookup"><span data-stu-id="75cc9-249">OAuth 2.0</span></span>

<span data-ttu-id="75cc9-250">Web API ve tek sayfa uygulaması proje şablonları artık OAuth 2.0 kullanarak yetkilendirmeyi destekler.</span><span class="sxs-lookup"><span data-stu-id="75cc9-250">The Web API and Single Page Application project templates now support authorization using OAuth 2.0.</span></span> <span data-ttu-id="75cc9-251">OAuth 2.0 korunan kaynakları'na istemci erişimini yetkilendirmek için bir çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-251">OAuth 2.0 is a framework for authorizing client access to protected resources.</span></span> <span data-ttu-id="75cc9-252">Çeşitli tarayıcılar ve mobil cihazlar dahil olmak üzere istemciler için çalışır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-252">It works for a variety of clients including browsers and mobile devices.</span></span>

<span data-ttu-id="75cc9-253">Destek için OAuth 2.0 taşıyıcı kimlik doğrulaması için Microsoft OWIN bileşenleri tarafından sağlanan ve yetkilendirme sunucusu rolünü uygulama yeni güvenlik ara yazılım üzerinde temel alır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-253">Support for OAuth 2.0 is based on new security middleware provided by the Microsoft OWIN Components for bearer authentication and implementing the authorization server role.</span></span> <span data-ttu-id="75cc9-254">Alternatif olarak, istemciler Azure Active Directory veya ADFS Windows Server 2012 R2 gibi bir kuruluş yetkilendirme sunucusu kullanarak yetki verilebilir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-254">Alternatively, clients can be authorized using an organizational authorization server, such as Azure Active Directory or ADFS in Windows Server 2012 R2.</span></span>

### <a name="odata-improvements"></a><span data-ttu-id="75cc9-255">OData geliştirmeleri</span><span class="sxs-lookup"><span data-stu-id="75cc9-255">OData Improvements</span></span>

<span data-ttu-id="75cc9-256">**$Select, $ desteği'ni genişletin, $batch ve $value**</span><span class="sxs-lookup"><span data-stu-id="75cc9-256">**Support for $select, $expand, $batch, and $value**</span></span>

<span data-ttu-id="75cc9-257">ASP.NET Web API OData $select için artık tam destek sahiptir, $'ni genişletin ve $value.</span><span class="sxs-lookup"><span data-stu-id="75cc9-257">ASP.NET Web API OData now has full support for $select, $expand, and $value.</span></span> <span data-ttu-id="75cc9-258">Toplu işleme ve değişiklik kümesi işleme isteği için $batch de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-258">You can also use $batch for request batching and processing of change sets.</span></span>

<span data-ttu-id="75cc9-259">$Select ve $expand seçeneklerini genişletin olanak sağlayan bir OData uç noktadan döndürülen verileri şeklini değiştirin.</span><span class="sxs-lookup"><span data-stu-id="75cc9-259">The $select and $expand options let you change the shape of the data that is returned from an OData endpoint.</span></span> <span data-ttu-id="75cc9-260">Daha fazla bilgi için bkz: [Introducing $select ve $expand genişletin Web API OData desteği](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="75cc9-260">For more information, see [Introducing $select and $expand support in Web API OData](../../../web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value.md).</span></span>

<span data-ttu-id="75cc9-261">**Geliştirilmiş genişletilebilirliği**</span><span class="sxs-lookup"><span data-stu-id="75cc9-261">**Improved extensibility**</span></span>

<span data-ttu-id="75cc9-262">OData biçimlendiricilerini genişletilebilir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-262">The OData formatters are now extensible.</span></span> <span data-ttu-id="75cc9-263">Atom girişi meta verileri ekleme, adlandırılmış akış ve medya bağlantısı girişler destekleyen, örneği ek açıklama ekleyin ve bağlantıları nasıl oluşturulduğu özelleştirme.</span><span class="sxs-lookup"><span data-stu-id="75cc9-263">You can add Atom entry metadata, support named stream and media link entries, add instance annotations, and customize how links are generated.</span></span>

<span data-ttu-id="75cc9-264">**Türü daha az desteği**</span><span class="sxs-lookup"><span data-stu-id="75cc9-264">**Type-less support**</span></span>

<span data-ttu-id="75cc9-265">CLR türleri, varlık türleri için tanımlamak gerek kalmadan OData Hizmetleri artık oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-265">You can now build OData services without needing to define CLR types for your entity types.</span></span> <span data-ttu-id="75cc9-266">Bunun yerine, OData denetleyicilerinizi alın veya dönüş örneklerini **IEdmObject**, hangi serileştirme/seri durumdan OData biçimlendiricilerini şunlardır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-266">Instead, your OData controllers can take or return instances of **IEdmObject**, which are the OData formatters serialize/deserialize.</span></span>

<span data-ttu-id="75cc9-267">**Varolan modeli yeniden kullanma**</span><span class="sxs-lookup"><span data-stu-id="75cc9-267">**Reuse an existing model**</span></span>

<span data-ttu-id="75cc9-268">Var olan bir varlık veri modeli (EDM) zaten varsa, şimdi onu doğrudan yeni bir tane oluşturmak zorunda kalmadan yerine tekrar kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-268">If you already have an existing entity data model (EDM), you can now reuse it directly, instead of having to build a new one.</span></span> <span data-ttu-id="75cc9-269">Örneğin, Entity Framework kullanıyorsanız, EF sizin için oluşturur EDM kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-269">For example, if you are using Entity Framework, you can use the EDM that EF builds for you.</span></span>

### <a name="request-batching"></a><span data-ttu-id="75cc9-270">Toplu işleme istek</span><span class="sxs-lookup"><span data-stu-id="75cc9-270">Request Batching</span></span>

<span data-ttu-id="75cc9-271">Toplu işleme istek ağ trafiğini azaltmak ve daha az chatty kullanıcı arabirimi bir yumuşak sağlamak için tek bir HTTP POST isteği, birden çok işleme birleştirir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-271">Request batching combines multiple operations into a single HTTP POST request, to reduce network traffic and provide a smoother, less chatty user interface.</span></span> <span data-ttu-id="75cc9-272">ASP.NET Web API isteği toplu işleme için birkaç stratejileri artık destekler:</span><span class="sxs-lookup"><span data-stu-id="75cc9-272">ASP.NET Web API now supports several strategies for request batching:</span></span>

- <span data-ttu-id="75cc9-273">OData hizmetine $batch uç noktasını kullanın.</span><span class="sxs-lookup"><span data-stu-id="75cc9-273">Use the $batch endpoint of an OData service.</span></span>
- <span data-ttu-id="75cc9-274">Birden çok istek tek bir MIME çok bölümlü istek paketi.</span><span class="sxs-lookup"><span data-stu-id="75cc9-274">Package multiple requests into a single MIME multipart request.</span></span>
- <span data-ttu-id="75cc9-275">Özel bir toplu işleme biçimini kullanın.</span><span class="sxs-lookup"><span data-stu-id="75cc9-275">Use a custom batching format.</span></span>

<span data-ttu-id="75cc9-276">Toplu işleme istek etkinleştirmek için bir yol toplu işleyici ile Web API yapılandırmanızı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="75cc9-276">To enable request batching, simply add a route with a batching handler to your Web API configuration:</span></span>

[!code-csharp[Main](release-notes/samples/sample4.cs)]

<span data-ttu-id="75cc9-277">Ayrıca denetleyebilirsiniz olup olmadığını istekleri veya sıralı olarak ya da herhangi bir sırada yürütülür.</span><span class="sxs-lookup"><span data-stu-id="75cc9-277">You can also control whether requests or executed sequentially or in any order.</span></span>

### <a name="portable-aspnet-web-api-client"></a><span data-ttu-id="75cc9-278">Taşınabilir ASP.NET Web API istemcisi</span><span class="sxs-lookup"><span data-stu-id="75cc9-278">Portable ASP.NET Web API Client</span></span>

<span data-ttu-id="75cc9-279">ASP.NET Web API İstemci artık Windows mağazası ve Windows Phone 8 uygulamalar arasında iş taşınabilir sınıf kitaplıkları oluşturmak için de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-279">You can now use the ASP.NET Web API Client to create portable class libraries that work across your Windows Store and Windows Phone 8 applications.</span></span> <span data-ttu-id="75cc9-280">İstemci ile sunucu arasında paylaşılabilir taşınabilir biçimlendiricileri de oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-280">You can also create portable formatters that can be shared across client and server.</span></span>

### <a name="improved-testability"></a><span data-ttu-id="75cc9-281">Geliştirilmiş Test Edilebilirlik</span><span class="sxs-lookup"><span data-stu-id="75cc9-281">Improved Testability</span></span>

<span data-ttu-id="75cc9-282">Web API 2 yapar birimine çok daha kolay test API denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="75cc9-282">Web API 2 makes it much easier to unit test your API controllers.</span></span> <span data-ttu-id="75cc9-283">Yalnızca istek iletisi ve yapılandırma ile API denetleyicisi oluşturma ve test etmek istediğiniz eylem yöntemini çağırın.</span><span class="sxs-lookup"><span data-stu-id="75cc9-283">Just instantiate your API controller with your request message and configuration, and then call the action method you wish to test.</span></span> <span data-ttu-id="75cc9-284">Mock kolaydır **UrlHelper** bağlantı oluşturma gerçekleştiren eylem yöntemleri için sınıf.</span><span class="sxs-lookup"><span data-stu-id="75cc9-284">It is also easy to mock the **UrlHelper** class, for action methods that perform link generation.</span></span>

### <a name="ihttpactionresult"></a><span data-ttu-id="75cc9-285">IHttpActionResult</span><span class="sxs-lookup"><span data-stu-id="75cc9-285">IHttpActionResult</span></span>

<span data-ttu-id="75cc9-286">Şimdi Web API eylem yöntemleri sonucunu kapsülleyen Ihttpactionresult uygulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-286">You can now implement IHttpActionResult to encapsulate the result of your Web API action methods.</span></span> <span data-ttu-id="75cc9-287">Bir Web API eylem yönteminden döndürülen Ihttpactionresult sonuç yanıt iletisi oluşturmak için ASP.NET Web API çalışma zamanı tarafından yürütülür.</span><span class="sxs-lookup"><span data-stu-id="75cc9-287">An IHttpActionResult returned from a Web API action method is executed by the ASP.NET Web API runtime to produce the resultant response message.</span></span> <span data-ttu-id="75cc9-288">Bir Ihttpactionresult birim basitleştirmek için tüm Web API eylemden döndürülebilecek Web API uygulamanızı test etme.</span><span class="sxs-lookup"><span data-stu-id="75cc9-288">An IHttpActionResult can be returned from any Web API action to simplify unit testing of your Web API implementation.</span></span> <span data-ttu-id="75cc9-289">Özel durum kodları döndürme için sonuçları dahil olmak üzere kutusu dışında Ihttpactionresult uygulamaları sayısı sağlanan kolaylık olması için içerik veya içerik anlaşması yanıtlarını biçimlendirilmiş.</span><span class="sxs-lookup"><span data-stu-id="75cc9-289">For convenience a number of IHttpActionResult implementations are provided out of the box including results for returning specific status codes, formatted content or content-negotiated responses.</span></span>

### <a name="httprequestcontext"></a><span data-ttu-id="75cc9-290">HttpRequestContext</span><span class="sxs-lookup"><span data-stu-id="75cc9-290">HttpRequestContext</span></span>

<span data-ttu-id="75cc9-291">Yeni **HttpRequestContext** isteğe bağlıdır ancak istekten hemen kullanılabilir olmayan herhangi bir durum izler.</span><span class="sxs-lookup"><span data-stu-id="75cc9-291">The new **HttpRequestContext** tracks any state that is tied to the request but is not immediately available from the request.</span></span> <span data-ttu-id="75cc9-292">Örneğin, kullanabileceğiniz **HttpRequestContext** rota verileri, istemci sertifikası, istekle ilişkili asıl almak için **UrlHelper** ve sanal yol kökü.</span><span class="sxs-lookup"><span data-stu-id="75cc9-292">For example, you can use the **HttpRequestContext** to get route data, the principal associated with the request, the client certificate, the **UrlHelper** and the virtual path root.</span></span> <span data-ttu-id="75cc9-293">Kolayca oluşturabileceğiniz bir **HttpRequestContext** için birim testi amaçlar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-293">You can easily create an **HttpRequestContext** for unit testing purposes.</span></span>

<span data-ttu-id="75cc9-294">İstek için asıl güvenmek yerine istekle akıtılan çünkü **Thread.CurrentPrincipal**, Web API ardışık düzeninde olsa asıl artık istek ömrü kullanılabilir durumdadır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-294">Because the principal for the request is flowed with the request instead of relying on **Thread.CurrentPrincipal**, the principal is now available throughout the lifetime of the request while it is in the Web API pipeline.</span></span>

### <a name="cors"></a><span data-ttu-id="75cc9-295">CORS</span><span class="sxs-lookup"><span data-stu-id="75cc9-295">CORS</span></span>

<span data-ttu-id="75cc9-296">Brock Allen alanından başka bir harika katkı sayesinde ASP.NET artık tam olarak arası kaynak isteği Paylaşımı (CORS) destekler.</span><span class="sxs-lookup"><span data-stu-id="75cc9-296">Thanks to another great contribution from Brock Allen, ASP.NET now fully supports Cross Origin Request Sharing (CORS).</span></span>

<span data-ttu-id="75cc9-297">Tarayıcı güvenlik bir web sayfası AJAX istekleri başka bir etki alanına yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="75cc9-297">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="75cc9-298">[CORS](http://www.w3.org/TR/cors/) sunucusunun aynı kaynak İlkesi hafifletin sağlar W3C standardıdır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-298">[CORS](http://www.w3.org/TR/cors/) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="75cc9-299">CORS kullanarak, bir sunucu açıkça bazı cross-origin istekleri başkalarının reddetme çalışırken izin verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-299">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span>

<span data-ttu-id="75cc9-300">Web API 2 CORS denetim öncesi isteklerde otomatik işlenmesini de dahil olmak üzere, artık destekler.</span><span class="sxs-lookup"><span data-stu-id="75cc9-300">Web API 2 now supports CORS, including automatic handling of preflight requests.</span></span> <span data-ttu-id="75cc9-301">Daha fazla bilgi için bkz: [ASP.NET Web API çıkış noktaları arası istekleri etkinleştirme](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="75cc9-301">For more information, see [Enabling Cross-Origin Requests in ASP.NET Web API](../../../web-api/overview/security/enabling-cross-origin-requests-in-web-api.md).</span></span>

### <a name="authentication-filters"></a><span data-ttu-id="75cc9-302">Kimlik doğrulaması filtreleri</span><span class="sxs-lookup"><span data-stu-id="75cc9-302">Authentication Filters</span></span>

<span data-ttu-id="75cc9-303">Kimlik doğrulaması filtreleri olan yeni bir ASP.NET Web API ardışık düzeninde yetkilendirme filtreleri önce çalıştıran ve kimlik doğrulama mantığı başına-eylemi belirtmenize olanak veren ASP.NET Web API filtrede tür başına denetleyicisi, veya genel olarak tüm denetleyicileri için.</span><span class="sxs-lookup"><span data-stu-id="75cc9-303">Authentication filters are a new kind of filter in ASP.NET Web API that run prior to authorization filters in the ASP.NET Web API pipeline and allow you to specify authentication logic per-action, per-controller, or globally for all controllers.</span></span> <span data-ttu-id="75cc9-304">Kimlik doğrulaması filtreleri kimlik bilgileri isteği işlemek ve karşılık gelen asıl sağlayın.</span><span class="sxs-lookup"><span data-stu-id="75cc9-304">Authentication filters process credentials in the request and provide a corresponding principal.</span></span> <span data-ttu-id="75cc9-305">Kimlik doğrulaması filtreleri, kimlik doğrulama sınaması yetkisiz isteklerine yanıt olarak da ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-305">Authentication filters can also add authentication challenges in response to unauthorized requests.</span></span>

### <a name="filter-overrides"></a><span data-ttu-id="75cc9-306">Filtre geçersiz kılma</span><span class="sxs-lookup"><span data-stu-id="75cc9-306">Filter Overrides</span></span>

<span data-ttu-id="75cc9-307">Şimdi bir geçersiz kılma filtresi belirterek bir belirtilen eylem yöntemini veya denetleyicileri, hangi filtre uygulamak geçersiz kılabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-307">You can now override which filters apply to a given action method or controller, by specifying an override filter.</span></span> <span data-ttu-id="75cc9-308">Geçersiz kılma filtreleri verilen kapsam (eylem veya denetleyici) için çalıştırmamalısınız filtre türleri kümesi belirtin.</span><span class="sxs-lookup"><span data-stu-id="75cc9-308">Override filters specify a set of filter types that should not run for a given scope (action or controller).</span></span> <span data-ttu-id="75cc9-309">Bu, genel filtreler ekleyin, ama bazı belirli eylemleri veya denetleyicileri hariç olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-309">This allows you to add global filters, but then exclude some from specific actions or controllers.</span></span>

### <a name="owin-integration"></a><span data-ttu-id="75cc9-310">OWIN tümleştirme</span><span class="sxs-lookup"><span data-stu-id="75cc9-310">OWIN Integration</span></span>

<span data-ttu-id="75cc9-311">ASP.NET Web API artık tam olarak OWIN destekler ve herhangi bir OWIN özellikli ana üzerinde çalıştırılabilir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-311">ASP.NET Web API now fully supports OWIN and can be run on any OWIN capable host.</span></span> <span data-ttu-id="75cc9-312">De dahil olduğu bir **HostAuthenticationFilter** OWIN kimlik doğrulama sistemi ile tümleştirme sağlar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-312">Also included is a **HostAuthenticationFilter** that provides integration with the OWIN authentication system.</span></span>

<span data-ttu-id="75cc9-313">OWIN Tümleştirmesi ile Web API SignalR gibi diğer OWIN ara yazılımı yanı sıra kendi işleminde kendi kendine barındırabilir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-313">With OWIN integration, you can self-host Web API in your own process alongside other OWIN middleware, such as SignalR.</span></span> <span data-ttu-id="75cc9-314">Daha fazla bilgi için bkz: [Self-Host ASP.NET Web API kullanımı OWIN](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="75cc9-314">For more information, see [Use OWIN to Self-Host ASP.NET Web API](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<a id="TOC13"></a>
## <a name="aspnet-signalr-20"></a><span data-ttu-id="75cc9-315">ASP.NET SignalR 2.0</span><span class="sxs-lookup"><span data-stu-id="75cc9-315">ASP.NET SignalR 2.0</span></span>

<span data-ttu-id="75cc9-316">Aşağıdaki bölümlerde SignalR 2.0 özellikleri açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-316">The following sections describe features of SignalR 2.0.</span></span>

- [<span data-ttu-id="75cc9-317">OWIN üzerinde oluşturulmuş</span><span class="sxs-lookup"><span data-stu-id="75cc9-317">Built on OWIN</span></span>](#builtonowin)
- [<span data-ttu-id="75cc9-318">MapHubs ve MapConnection MapSignalR sunulmuştur</span><span class="sxs-lookup"><span data-stu-id="75cc9-318">MapHubs and MapConnection are now MapSignalR</span></span>](#MapSignalR)
- [<span data-ttu-id="75cc9-319">Etki alanları arası desteği</span><span class="sxs-lookup"><span data-stu-id="75cc9-319">Cross-Domain Support</span></span>](#crossdomain)
- [<span data-ttu-id="75cc9-320">iOS ve Android MonoTouch ve MonoDroid aracılığıyla desteği</span><span class="sxs-lookup"><span data-stu-id="75cc9-320">iOS and Android support via MonoTouch and MonoDroid</span></span>](#mobile)
- [<span data-ttu-id="75cc9-321">Taşınabilir .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="75cc9-321">Portable .NET Client</span></span>](#portable)
- [<span data-ttu-id="75cc9-322">Yeni paket kendini barındırma</span><span class="sxs-lookup"><span data-stu-id="75cc9-322">New Self-Host Package</span></span>](#selfhost)
- [<span data-ttu-id="75cc9-323">Geriye dönük olarak uyumlu sunucu desteği</span><span class="sxs-lookup"><span data-stu-id="75cc9-323">Backward-compatible server support</span></span>](#backwardcompat)
- [<span data-ttu-id="75cc9-324">.NET 4.0 için sunucu desteği kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="75cc9-324">Removed server support for .NET 4.0</span></span>](#remove40)
- [<span data-ttu-id="75cc9-325">İstemciler ve gruplar listesine ileti gönderme</span><span class="sxs-lookup"><span data-stu-id="75cc9-325">Sending a message to a list of clients and groups</span></span>](#messagelist)
- [<span data-ttu-id="75cc9-326">Belirli bir kullanıcı için bir ileti gönderme</span><span class="sxs-lookup"><span data-stu-id="75cc9-326">Sending a message to a specific user</span></span>](#sendtouser)
- [<span data-ttu-id="75cc9-327">Daha iyi hata işleme desteği</span><span class="sxs-lookup"><span data-stu-id="75cc9-327">Better error handling support</span></span>](#errorhandling)
- [<span data-ttu-id="75cc9-328">Daha kolay birim hub'ları test etme</span><span class="sxs-lookup"><span data-stu-id="75cc9-328">Easier unit testing of hubs</span></span>](#unittesting)
- [<span data-ttu-id="75cc9-329">JavaScript hata işleme</span><span class="sxs-lookup"><span data-stu-id="75cc9-329">JavaScript error handling</span></span>](#javascripterror)

<span data-ttu-id="75cc9-330">Mevcut bir 1.x projesini SignalR 2.0 sürümüne yükseltmek nasıl bir örnek için bkz: [bir SignalR yükseltme 1.x proje](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).</span><span class="sxs-lookup"><span data-stu-id="75cc9-330">For an example of how to upgrade an existing 1.x project to SignalR 2.0, see [Upgrading a SignalR 1.x Project](../../../signalr/overview/releases/upgrading-signalr-1x-projects-to-20.md).</span></span>

<a id="builtonowin"></a>
### <a name="built-on-owin"></a><span data-ttu-id="75cc9-331">OWIN üzerinde oluşturulmuş</span><span class="sxs-lookup"><span data-stu-id="75cc9-331">Built on OWIN</span></span>

<span data-ttu-id="75cc9-332">SignalR 2.0 tamamen üzerinde kurulu [OWIN (.NET için açık Web arabirimi)](http://owin.org/).</span><span class="sxs-lookup"><span data-stu-id="75cc9-332">SignalR 2.0 is built completely on [OWIN (the Open Web Interface for .NET)](http://owin.org/).</span></span> <span data-ttu-id="75cc9-333">Bu değişiklik Kurulum işlemi için SignalR web barındırılan ve kendi kendini barındıran SignalR uygulamalar arasında çok daha tutarlı hale getirir, ancak ayrıca bir dizi API değişiklikleri istedi.</span><span class="sxs-lookup"><span data-stu-id="75cc9-333">This change makes the setup process for SignalR much more consistent between web-hosted and self-hosted SignalR applications, but has also required a number of API changes.</span></span>

<a id="MapSignalR"></a>

### <a name="maphubs-and-mapconnection-are-now-mapsignalr"></a><span data-ttu-id="75cc9-334">MapHubs ve MapConnection MapSignalR sunulmuştur</span><span class="sxs-lookup"><span data-stu-id="75cc9-334">MapHubs and MapConnection are now MapSignalR</span></span>

<span data-ttu-id="75cc9-335">OWIN standartları ile uyumluluk için bu yöntemleri için yeniden adlandırıldığı `MapSignalR`.</span><span class="sxs-lookup"><span data-stu-id="75cc9-335">For compatibility with OWIN standards, these methods have been renamed to `MapSignalR`.</span></span> <span data-ttu-id="75cc9-336">`MapSignalR` parametreleri tüm hub'ları eşler olmadan adlı (olarak `MapHubs` sürümünde mu 1.x); tek tek eşlemek için **PersistentConnection** nesneleri, tür parametresi ve bağlantı olarak URL uzantısı olarak bağlantı türünü belirtin ilk bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="75cc9-336">`MapSignalR` called without parameters will map all hubs (as `MapHubs` does in version 1.x); to map individual **PersistentConnection** objects, specify the connection type as the type parameter, and the URL extension for the connection as the first argument.</span></span>

<span data-ttu-id="75cc9-337">`MapSignalR` Yöntemi, bir Owın başlangıç sınıfı çağrılır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-337">The `MapSignalR` method is called in an Owin startup class.</span></span> <span data-ttu-id="75cc9-338">Visual Studio 2013 Owın başlangıç sınıfı için yeni bir şablon içerir; Bu şablonu kullanmak için aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="75cc9-338">Visual Studio 2013 contains a new template for an Owin startup class; to use this template, do the following:</span></span>

1. <span data-ttu-id="75cc9-339">Projeye sağ tıklayın</span><span class="sxs-lookup"><span data-stu-id="75cc9-339">Right-click on the project</span></span>
2. <span data-ttu-id="75cc9-340">Seçin **ekleme**, **yeni öğe...**</span><span class="sxs-lookup"><span data-stu-id="75cc9-340">Select **Add**, **New Item...**</span></span>
3. <span data-ttu-id="75cc9-341">Seçin **Owın başlangıç sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="75cc9-341">Select **Owin Startup class**.</span></span> <span data-ttu-id="75cc9-342">Yeni sınıf **haline**.</span><span class="sxs-lookup"><span data-stu-id="75cc9-342">Name the new class **Startup.cs**.</span></span>

<span data-ttu-id="75cc9-343">İçinde bir **Web uygulaması,** Owın başlangıç sınıfı içeren `MapSignalR` aşağıda gösterildiği gibi Web.Config dosyası uygulama ayarları düğümünde bir giriş kullanılarak Owın'ın başlatma işlemi yöntemi daha sonra eklenen.</span><span class="sxs-lookup"><span data-stu-id="75cc9-343">In a **Web application,** the Owin startup class containing the `MapSignalR` method is then added to Owin's startup process using an entry in the application settings node of the Web.Config file, as shown below.</span></span>

<span data-ttu-id="75cc9-344">İçinde bir **uygulama'kendi kendini barındıran**, başlangıç sınıfı türü parametresi olarak geçirilen `WebApp.Start` yöntemi.</span><span class="sxs-lookup"><span data-stu-id="75cc9-344">In a **Self-hosted application**, the Startup class is passed as the type parameter of the `WebApp.Start` method.</span></span>

<span data-ttu-id="75cc9-345">**Hub'ları ve SignalR bağlantıları eşleme 1.x (bir web uygulaması genel uygulama dosyasından):**</span><span class="sxs-lookup"><span data-stu-id="75cc9-345">**Mapping hubs and connections in SignalR 1.x (from the global application file in a web application):**</span></span> 

[!code-csharp[Main](release-notes/samples/sample5.cs)]

<span data-ttu-id="75cc9-346">**Eşleme hub'ları ve bağlantıları SignalR 2.0 (dosyadan bir Owın başlangıç sınıfı):**</span><span class="sxs-lookup"><span data-stu-id="75cc9-346">**Mapping hubs and connections in SignalR 2.0 (from an Owin Startup class file):**</span></span> 

[!code-csharp[Main](release-notes/samples/sample6.cs)]

<span data-ttu-id="75cc9-347">İçinde bir **uygulama'kendi kendini barındıran**, başlangıç sınıfı için tür parametre olarak geçirilen `WebApp.Start` aşağıda gösterildiği gibi yöntemi.</span><span class="sxs-lookup"><span data-stu-id="75cc9-347">In a **Self-hosted application**, the Startup class is passed as the type parameter for the `WebApp.Start` method, as shown below.</span></span>

[!code-csharp[Main](release-notes/samples/sample7.cs)]

<a id="crossdomain"></a>

### <a name="cross-domain-support"></a><span data-ttu-id="75cc9-348">Etki alanları arası desteği</span><span class="sxs-lookup"><span data-stu-id="75cc9-348">Cross-Domain Support</span></span>

<span data-ttu-id="75cc9-349">Etki alanı istekleri arası 1.x tek EnableCrossDomain tarafından kontrol SignalR bayrak.</span><span class="sxs-lookup"><span data-stu-id="75cc9-349">In SignalR 1.x, cross domain requests were controlled by a single EnableCrossDomain flag.</span></span> <span data-ttu-id="75cc9-350">Bu bayrak JSONP ve CORS isteklerini denetlenir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-350">This flag controlled both JSONP and CORS requests.</span></span> <span data-ttu-id="75cc9-351">Daha fazla esneklik için tüm CORS desteği SignalR sunucu bileşeninden kaldırıldı (JavaScript lients kullanmaya devam CORS normalde tarayıcı onu destekleyen algılanırsa), ve yeni OWIN ara yazılımı sağlandıktan bu senaryoları desteklemek kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-351">For greater flexibility, all CORS support has been removed from the server component of SignalR (JavaScript lients still use CORS normally if it is detected that the browser supports it), and new OWIN middleware has been made available to support these scenarios.</span></span>

<span data-ttu-id="75cc9-352">SignalR 2. 0'da, (etki alanları arası istekleri eski tarayıcılarda desteklemek için), istemcide varsa JSONP gereklidir, açıkça ayarlayarak etkinleştirilmesi gerekir `EnableJSONP` üzerinde `HubConfiguration` nesnesine `true`, aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="75cc9-352">In SignalR 2.0, If JSONP is required on the client (to support cross-domain requests in older browsers), it will need to be enabled explicitly by setting `EnableJSONP` on the `HubConfiguration` object to `true`, as shown below.</span></span> <span data-ttu-id="75cc9-353">CORS az güvenlidir JSONP varsayılan olarak devre dışıdır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-353">JSONP is disabled by default, as it is less secure than CORS.</span></span>

<span data-ttu-id="75cc9-354">SignalR 2. 0 ' yeni CORS Ara eklemek için Ekle `Microsoft.Owin.Cors` projenizi ve çağrı kitaplığa `UseCors` aşağıdaki bölümde gösterildiği gibi SignalR Ara önce.</span><span class="sxs-lookup"><span data-stu-id="75cc9-354">To add the new CORS middleware in SignalR 2.0, add the `Microsoft.Owin.Cors` library to your project, and call `UseCors` before your SignalR middleware, as shown in the section below.</span></span>

<span data-ttu-id="75cc9-355">**Microsoft.Owin.Cors projenize ekleme**: Bu Kitaplığı'nı yüklemek için Paket Yöneticisi konsolunda aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="75cc9-355">**Adding Microsoft.Owin.Cors to your project**: To install this library, run the following command in the Package Manager Console:</span></span>

[!code-powershell[Main](release-notes/samples/sample8.ps1)]

<span data-ttu-id="75cc9-356">Bu komut 2.0.0 ekleyeceksiniz projenize paketin sürümü.</span><span class="sxs-lookup"><span data-stu-id="75cc9-356">This command will add the 2.0.0 version of the package to your project.</span></span>

<span data-ttu-id="75cc9-357">**UseCors çağırma**</span><span class="sxs-lookup"><span data-stu-id="75cc9-357">**Calling UseCors**</span></span>

<span data-ttu-id="75cc9-358">Aşağıdaki kod parçacıkları nasıl SignalR öğesinde etki alanları arası bağlantılar uygulandığını gösteren 1.x ve 2. 0.</span><span class="sxs-lookup"><span data-stu-id="75cc9-358">The following code snippets demonstrate how to implement cross-domain connections in SignalR 1.x and 2.0.</span></span>

<span data-ttu-id="75cc9-359">**SignalR öğesinde etki alanları arası istekleri uygulama 1.x (genel uygulama dosyadan)**</span><span class="sxs-lookup"><span data-stu-id="75cc9-359">**Implementing cross-domain requests in SignalR 1.x (from the global application file)**</span></span>

[!code-csharp[Main](release-notes/samples/sample9.cs)]

<span data-ttu-id="75cc9-360">**Uygulama etki alanları arası isteklerde SignalR 2. 0 (C# kod dosyası)**</span><span class="sxs-lookup"><span data-stu-id="75cc9-360">**Implementing cross-domain requests in SignalR 2.0 (from a C# code file)**</span></span>

<span data-ttu-id="75cc9-361">Aşağıdaki kod, CORS veya JSONP SignalR 2.0 projede etkinleştirme gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-361">The following code demonstrates how to enable CORS or JSONP in a SignalR 2.0 project.</span></span> <span data-ttu-id="75cc9-362">Bu kod örneği kullanan `Map` ve `RunSignalR` yerine `MapSignalR`, CORS desteği gerektiren SignalR istekleri için CORS Ara çalışır (yerine belirtilen yoldaki tüm trafik için `MapSignalR`.) `Map` belirli bir URL öneki için yerine tüm uygulama için çalıştırmak için gereken diğer tüm ara yazılımı için de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-362">This code sample uses `Map` and `RunSignalR` instead of `MapSignalR`, so that the CORS middleware runs only for the SignalR requests that require CORS support (rather than for all traffic at the path specified in `MapSignalR`.) `Map` can also be used for any other middleware that needs to run for a specific URL prefix, rather than for the entire application.</span></span>

[!code-csharp[Main](release-notes/samples/sample10.cs)]

<a id="mobile"></a>

### <a name="ios-and-android-support-via-monotouch-and-monodroid"></a><span data-ttu-id="75cc9-363">iOS ve Android MonoTouch ve MonoDroid aracılığıyla desteği</span><span class="sxs-lookup"><span data-stu-id="75cc9-363">iOS and Android support via MonoTouch and MonoDroid</span></span>

<span data-ttu-id="75cc9-364">İOS ve MonoTouch ve MonoDroid bileşenlerini kullanan Android istemciler için destek eklenmiştir [Xamarin Kitaplığı](https://xamarin.com/).</span><span class="sxs-lookup"><span data-stu-id="75cc9-364">Support has been added for iOS and Android clients using MonoTouch and MonoDroid components from the [Xamarin library](https://xamarin.com/).</span></span> <span data-ttu-id="75cc9-365">Bunların nasıl kullanılacağını hakkında daha fazla bilgi için bkz: [kullanarak Xamarin bileşen](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln).</span><span class="sxs-lookup"><span data-stu-id="75cc9-365">For more information on how to use them, see [Using Xamarin Components](https://github.com/SignalR/SignalR/wiki/Building-Mono.Mobile.sln).</span></span> <span data-ttu-id="75cc9-366">Bu bileşenlerin kullanılabilir olması [Xamarin mağazasında](https://store.xamarin.com/) SignalR RTW yayın olduğunda kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-366">These components will be available in the [Xamarin Store](https://store.xamarin.com/) when the SignalR RTW release is available.</span></span>

<a id="portable"></a> <span data-ttu-id="75cc9-367">### Taşınabilir .NET istemcisi</span><span class="sxs-lookup"><span data-stu-id="75cc9-367">### Portable .NET client</span></span>

<span data-ttu-id="75cc9-368">Platformlar arası geliştirme, Silverlight, WinRT daha iyi kolaylaştırmak ve Windows Phone istemcileri, aşağıdaki platformları destekleyen bir tek taşınabilir .NET istemcisi ile değiştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="75cc9-368">To better facilitate cross-platform development, the Silverlight, WinRT and Windows Phone clients have been replaced with a single portable .NET client that supports the following platforms:</span></span>

- <span data-ttu-id="75cc9-369">NET 4.5</span><span class="sxs-lookup"><span data-stu-id="75cc9-369">NET 4.5</span></span>
- <span data-ttu-id="75cc9-370">Silverlight 5</span><span class="sxs-lookup"><span data-stu-id="75cc9-370">Silverlight 5</span></span>
- <span data-ttu-id="75cc9-371">WinRT (Windows mağazası uygulamaları için .NET)</span><span class="sxs-lookup"><span data-stu-id="75cc9-371">WinRT (.NET for Windows Store Apps)</span></span>
- <span data-ttu-id="75cc9-372">Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="75cc9-372">Windows Phone 8</span></span>

<a id="selfhost"></a>

### <a name="new-self-host-package"></a><span data-ttu-id="75cc9-373">Yeni paket kendini barındırma</span><span class="sxs-lookup"><span data-stu-id="75cc9-373">New Self-Host Package</span></span>

<span data-ttu-id="75cc9-374">SignalR kendi kendini barındıran (bir işlemde barındırılan SignalR uygulamalarını veya başka bir uygulamanın yerine bir web sunucusunda barındırılan) kullanmaya başlama kolaylaştırmak için bir NuGet paketi artık yoktur.</span><span class="sxs-lookup"><span data-stu-id="75cc9-374">There is now a NuGet package to make it easier to get started with SignalR Self-Host (SignalR applications that are hosted in a process or other application, rather than being hosted in a web server).</span></span> <span data-ttu-id="75cc9-375">SignalR ile yerleşik bir kendi kendini barındıran proje yükseltmek için 1.x, Microsoft.AspNet.SignalR.Owin paketini kaldırın ve Microsoft.AspNet.SignalR.SelfHost paketi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="75cc9-375">To upgrade a self-host project built with SignalR 1.x, remove the Microsoft.AspNet.SignalR.Owin package, and add the Microsoft.AspNet.SignalR.SelfHost package.</span></span> <span data-ttu-id="75cc9-376">Kendini barındırma paketi ile çalışmaya başlama hakkında daha fazla bilgi için bkz: [Öğreticisi: SignalR kendini barındırma](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="75cc9-376">For more information on getting started with the self-host package, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<a id="backwardcompat"></a>

### <a name="backward-compatible-server-support"></a><span data-ttu-id="75cc9-377">Geriye dönük olarak uyumlu sunucu desteği</span><span class="sxs-lookup"><span data-stu-id="75cc9-377">Backward-compatible server support</span></span>

<span data-ttu-id="75cc9-378">' In önceki sürümlerinde SignalR, istemcide kullanılan SignalR paket ve aynı olması için gereken sunucu sürümleri.</span><span class="sxs-lookup"><span data-stu-id="75cc9-378">In previous versions of SignalR, the versions of the SignalR package used in the client and the server needed to be identical.</span></span> <span data-ttu-id="75cc9-379">Güncelleştirme zor olurdu kalın istemci uygulamalarını desteklemek için eski bir istemciyi daha yeni bir sunucu sürümünü kullanarak SignalR 2.0 artık destekler.</span><span class="sxs-lookup"><span data-stu-id="75cc9-379">In order to support thick-client applications that would be difficult to update, SignalR 2.0 now supports using a newer server version with an older client.</span></span> <span data-ttu-id="75cc9-380">**Not: SignalR 2.0 yeni istemcilerle eski sürümleriyle oluşturulan sunucularını desteklemez.**</span><span class="sxs-lookup"><span data-stu-id="75cc9-380">**Note: SignalR 2.0 does not support servers built with older versions with newer clients.**</span></span>

<a id="remove40"></a>

### <a name="removed-server-support-for-net-40"></a><span data-ttu-id="75cc9-381">.NET 4.0 için sunucu desteği kaldırıldı</span><span class="sxs-lookup"><span data-stu-id="75cc9-381">Removed server support for .NET 4.0</span></span>

<span data-ttu-id="75cc9-382">SignalR 2.0, .NET 4.0 ile sunucu birlikte çalışabilirlik için destek bıraktı.</span><span class="sxs-lookup"><span data-stu-id="75cc9-382">SignalR 2.0 has dropped support for server interoperability with .NET 4.0.</span></span> <span data-ttu-id="75cc9-383">.NET 4.5 SignalR 2.0 sunucularıyla kullanılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-383">.NET 4.5 must be used with SignalR 2.0 servers.</span></span> <span data-ttu-id="75cc9-384">SignalR 2.0 için bir .NET 4.0 istemci hala var.</span><span class="sxs-lookup"><span data-stu-id="75cc9-384">There is still a .NET 4.0 client for SignalR 2.0.</span></span>

<a id="messagelist"></a>

### <a name="sending-a-message-to-a-list-of-clients-and-groups"></a><span data-ttu-id="75cc9-385">İstemciler ve gruplar listesine ileti gönderme</span><span class="sxs-lookup"><span data-stu-id="75cc9-385">Sending a message to a list of clients and groups</span></span>

<span data-ttu-id="75cc9-386">SignalR 2. 0'da, istemci ve grup kimlikleri listesini kullanarak ileti göndermek mümkündür.</span><span class="sxs-lookup"><span data-stu-id="75cc9-386">In SignalR 2.0, it's possible to send a message using a list of client and group IDs.</span></span> <span data-ttu-id="75cc9-387">Aşağıdaki kod parçacıkları bunun nasıl yapılacağı gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-387">The following code snippets demonstrate how to do this.</span></span>

<span data-ttu-id="75cc9-388">**İstemcileri ve PersistentConnection kullanarak grupları listesine ileti gönderme**</span><span class="sxs-lookup"><span data-stu-id="75cc9-388">**Sending a message to a list of clients and groups using PersistentConnection**</span></span>

[!code-csharp[Main](release-notes/samples/sample11.cs)]

<span data-ttu-id="75cc9-389">**İstemcileri ve hub'ları kullanarak grupları listesine ileti gönderme**</span><span class="sxs-lookup"><span data-stu-id="75cc9-389">**Sending a message to a list of clients and groups using Hubs**</span></span>

[!code-csharp[Main](release-notes/samples/sample12.cs)]

<a id="sendtouser"></a>

### <a name="sending-a-message-to-a-specific-user"></a><span data-ttu-id="75cc9-390">Belirli bir kullanıcı için bir ileti gönderme</span><span class="sxs-lookup"><span data-stu-id="75cc9-390">Sending a message to a specific user</span></span>

<span data-ttu-id="75cc9-391">Bu özellik, USERID belirtmek kullanıcılara bir Irequest IUserIdProvider yeni arabirimi aracılığıyla göre:</span><span class="sxs-lookup"><span data-stu-id="75cc9-391">This feature allows users to specify what the userId is based on an IRequest via a new interface IUserIdProvider:</span></span>

<span data-ttu-id="75cc9-392">**IUserIdProvider arabirimi**</span><span class="sxs-lookup"><span data-stu-id="75cc9-392">**The IUserIdProvider interface**</span></span>

[!code-csharp[Main](release-notes/samples/sample13.cs)]

<span data-ttu-id="75cc9-393">Varsayılan olarak kullanıcının IPrincipal.Identity.Name kullanıcı adı olarak kullandığı uygulaması olacaktır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-393">By default there will be an implementation that uses the user's IPrincipal.Identity.Name as the user name.</span></span>

<span data-ttu-id="75cc9-394">Hub'ları, bu kullanıcılara yeni bir API aracılığıyla iletileri göndermek kullanabileceksiniz:</span><span class="sxs-lookup"><span data-stu-id="75cc9-394">In hubs, you'll be able to send messages to these users via a new API:</span></span>

<span data-ttu-id="75cc9-395">**API Clients.User kullanma**</span><span class="sxs-lookup"><span data-stu-id="75cc9-395">**Using the Clients.User API**</span></span>

[!code-csharp[Main](release-notes/samples/sample14.cs)]

<a id="errorhandling"></a>

### <a name="better-error-handling-support"></a><span data-ttu-id="75cc9-396">Daha iyi hata işleme desteği</span><span class="sxs-lookup"><span data-stu-id="75cc9-396">Better Error Handling Support</span></span>

<span data-ttu-id="75cc9-397">Kullanıcılar şimdi throw **HubException** herhangi bir hub çağrısının gelen.</span><span class="sxs-lookup"><span data-stu-id="75cc9-397">Users can now throw **HubException** from any hub invocation.</span></span> <span data-ttu-id="75cc9-398">Oluşturucusunun **HubException** dize ileti ve nesneyi alabilir ek hata verileri.</span><span class="sxs-lookup"><span data-stu-id="75cc9-398">The constructor of the **HubException** can take a string message and an object extra error data.</span></span> <span data-ttu-id="75cc9-399">SignalR otomatik-özel durum seri hale getirmek ve burada bu reddetme/hub yöntemi çağrısının başarısız kullanılacak istemciye göndermek.</span><span class="sxs-lookup"><span data-stu-id="75cc9-399">SignalR will auto-serialize the exception and send it to the client where it will be used to reject/fail the hub method invocation.</span></span>

<span data-ttu-id="75cc9-400">**Ayrıntılı hub'ı özel durumlar Göster** ayarı var. şifrelemeyle **HubException** veya değil; istemciye gönderilen her zaman gönderildi.</span><span class="sxs-lookup"><span data-stu-id="75cc9-400">The **show detailed hub exceptions** setting has no bearing on **HubException** being sent back to the client or not; it is always sent.</span></span>

<span data-ttu-id="75cc9-401">**Sunucu tarafı kodu bir HubException istemciye gönderme gösterme**</span><span class="sxs-lookup"><span data-stu-id="75cc9-401">**Server-side code demonstrating sending a HubException to the client**</span></span>

[!code-csharp[Main](release-notes/samples/sample15.cs)]

<span data-ttu-id="75cc9-402">**Sunucudan gönderilen bir HubException yanıtlama gösteren JavaScript istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="75cc9-402">**JavaScript client code demonstrating responding to a HubException sent from the server**</span></span>

[!code-html[Main](release-notes/samples/sample16.html)]

<span data-ttu-id="75cc9-403">**Sunucudan gönderilen bir HubException yanıtlama gösteren .NET istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="75cc9-403">**.NET client code demonstrating responding to a HubException sent from the server**</span></span>

[!code-csharp[Main](release-notes/samples/sample17.cs)]

<a id="unittesting"></a>

### <a name="easier-unit-testing-of-hubs"></a><span data-ttu-id="75cc9-404">Daha kolay birim hub'ları test etme</span><span class="sxs-lookup"><span data-stu-id="75cc9-404">Easier unit testing of hubs</span></span>

<span data-ttu-id="75cc9-405">SignalR 2.0 içerir adlı bir arabirim `IHubCallerConnectionContext` sahte istemci tarafı çağrılarını oluşturmak kolaylaştırır hub'ları üzerinde.</span><span class="sxs-lookup"><span data-stu-id="75cc9-405">SignalR 2.0 includes an interface called `IHubCallerConnectionContext` on Hubs that makes it easier to create mock client side invocations.</span></span> <span data-ttu-id="75cc9-406">Aşağıdaki kod parçacıkları popüler test harnesses ile bu arabirimi kullanarak göstermek [xUnit.net](https://github.com/xunit/xunit) ve [moq](https://code.google.com/p/moq/).</span><span class="sxs-lookup"><span data-stu-id="75cc9-406">The following code snippets demonstrate using this interface with popular test harnesses [xUnit.net](https://github.com/xunit/xunit) and [moq](https://code.google.com/p/moq/).</span></span>

<span data-ttu-id="75cc9-407">**Birim SignalR ile xUnit.net testi**</span><span class="sxs-lookup"><span data-stu-id="75cc9-407">**Unit testing SignalR with xUnit.net**</span></span>

[!code-csharp[Main](release-notes/samples/sample18.cs)]

<span data-ttu-id="75cc9-408">**Birim SignalR ile moq testi**</span><span class="sxs-lookup"><span data-stu-id="75cc9-408">**Unit testing SignalR with moq**</span></span>

[!code-csharp[Main](release-notes/samples/sample19.cs)]

<a id="javascripterror"></a>

### <a name="javascript-error-handling"></a><span data-ttu-id="75cc9-409">JavaScript hata işleme</span><span class="sxs-lookup"><span data-stu-id="75cc9-409">JavaScript error handling</span></span>

<span data-ttu-id="75cc9-410">SignalR 2. 0'da, tüm JavaScript hata işleme geri aramalar ham dizeler yerine JavaScript hata nesneleri döndürür.</span><span class="sxs-lookup"><span data-stu-id="75cc9-410">In SignalR 2.0, all JavaScript error handling callbacks return JavaScript error objects instead of raw strings.</span></span> <span data-ttu-id="75cc9-411">Bu hata işleyicileri için daha zengin bilgi akışı SignalR sağlar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-411">This allows SignalR to flow richer information to your error handlers.</span></span> <span data-ttu-id="75cc9-412">İç özel duruma gelen alabilirsiniz `source` hata özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-412">You can get the inner exception from the `source` property of the error.</span></span>

<span data-ttu-id="75cc9-413">**Start.Fail özel durumu işler JavaScript istemci kodu**</span><span class="sxs-lookup"><span data-stu-id="75cc9-413">**JavaScript client code that handles the Start.Fail exception**</span></span>

[!code-javascript[Main](release-notes/samples/sample20.js)]

<a id="TOC8"></a>
## <a name="aspnet-identity"></a><span data-ttu-id="75cc9-414">ASP.NET Kimlik</span><span class="sxs-lookup"><span data-stu-id="75cc9-414">ASP.NET Identity</span></span>

### <a name="new-aspnet-membership-system"></a><span data-ttu-id="75cc9-415">Yeni ASP.NET üyelik sistemini</span><span class="sxs-lookup"><span data-stu-id="75cc9-415">New ASP.NET Membership System</span></span>

<span data-ttu-id="75cc9-416">ASP.NET, ASP.NET uygulamaları için yeni üyelik sistemi kimliğidir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-416">ASP.NET Identity is the new membership system for ASP.NET applications.</span></span> <span data-ttu-id="75cc9-417">ASP.NET Identity, kullanıcı özel profil verileri uygulama verileriyle tümleştirmeyi kolaylaştırır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-417">ASP.NET Identity makes it easy to integrate user-specific profile data with application data.</span></span> <span data-ttu-id="75cc9-418">ASP.NET Identity Kalıcılık modeli kullanıcı profilleri için uygulamanızda seçmenize olanak sağlar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-418">ASP.NET Identity also allows you to choose the persistence model for user profiles in your application.</span></span> <span data-ttu-id="75cc9-419">Verileri bir SQL Server veritabanına veya Azure depolama tabloları gibi NoSQL veri depolarına dahil olmak üzere başka bir veri deposu depolayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-419">You can store the data in a SQL Server database or another data store, including NoSQL data stores such as Azure Storage Tables.</span></span> <span data-ttu-id="75cc9-420">Daha fazla bilgi için bkz: [tek tek kullanıcı hesaplarını](creating-web-projects-in-visual-studio.md#indauth) içinde **Visual Studio 2013'da ASP.NET Web projeleri oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="75cc9-420">For more information, see [Individual User Accounts](creating-web-projects-in-visual-studio.md#indauth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="claims-based-authentication"></a><span data-ttu-id="75cc9-421">Beyana dayalı kimlik doğrulama</span><span class="sxs-lookup"><span data-stu-id="75cc9-421">Claims-based authentication</span></span>

<span data-ttu-id="75cc9-422">ASP.NET, şimdi burada kullanıcının kimliğini güvenilir verene gelen talepler kümesi olarak temsil edilir talep tabanlı kimlik doğrulaması, destekler.</span><span class="sxs-lookup"><span data-stu-id="75cc9-422">ASP.NET now supports claims-based authentication, where the user's identity is represented as a set of claims from a trusted issuer.</span></span> <span data-ttu-id="75cc9-423">Kullanıcılar, kimliği doğrulanmış bir kullanıcı adı ve parola bir uygulama veritabanında tutulan veya sosyal kimlik sağlayıcısı kullanarak olabilir (örneğin: Microsoft Accounts, Facebook, Google, Twitter), veya Azure Active Directory kuruluş hesaplarıyla kullanarak veya Active Directory Federasyon Hizmetleri (ADFS).</span><span class="sxs-lookup"><span data-stu-id="75cc9-423">Users can be authenticated using a username and password maintained in an application database, or using social identity providers (for example: Microsoft Accounts, Facebook, Google, Twitter), or using organizational accounts through Azure Active Directory or Active Directory Federation Services (ADFS).</span></span>

### <a name="integration-with-azure-active-directory-and-windows-server-active-directory"></a><span data-ttu-id="75cc9-424">Azure Active Directory ve Windows Server Active Directory ile tümleştirme</span><span class="sxs-lookup"><span data-stu-id="75cc9-424">Integration with Azure Active Directory and Windows Server Active Directory</span></span>

<span data-ttu-id="75cc9-425">Artık Azure Active Directory veya Windows Server Active Directory (AD) kimlik doğrulaması için kullanmak ASP.NET projeleri oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-425">You can now create ASP.NET projects that use Azure Active Directory or Windows Server Active Directory (AD) for authentication.</span></span> <span data-ttu-id="75cc9-426">Daha fazla bilgi için bkz: [Kurumsal hesaplar](creating-web-projects-in-visual-studio.md#orgauth) içinde **Visual Studio 2013'da ASP.NET Web projeleri oluşturma**.</span><span class="sxs-lookup"><span data-stu-id="75cc9-426">For more information, see [Organizational Accounts](creating-web-projects-in-visual-studio.md#orgauth) in **Creating ASP.NET Web Projects in Visual Studio 2013**.</span></span>

### <a name="owin-integration"></a><span data-ttu-id="75cc9-427">OWIN tümleştirme</span><span class="sxs-lookup"><span data-stu-id="75cc9-427">OWIN Integration</span></span>

<span data-ttu-id="75cc9-428">ASP.NET kimlik doğrulaması şimdi tüm OWIN tabanlı ana bilgisayarda kullanılan OWIN ara yazılımı temel alır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-428">ASP.NET authentication is now based on OWIN middleware that can be used on any OWIN-based host.</span></span> <span data-ttu-id="75cc9-429">OWIN hakkında daha fazla bilgi için aşağıdakilere bakın [Microsoft OWIN bileşenleri](#TOC7) bölümü.</span><span class="sxs-lookup"><span data-stu-id="75cc9-429">For more information about OWIN, see the following [Microsoft OWIN Components](#TOC7) section.</span></span>

<a id="TOC7"></a>
## <a name="microsoft-owin-components"></a><span data-ttu-id="75cc9-430">Microsoft OWIN bileşenleri</span><span class="sxs-lookup"><span data-stu-id="75cc9-430">Microsoft OWIN Components</span></span>

<span data-ttu-id="75cc9-431">[.NET için Web Arabirimi'ni açmak](http://owin.org/) (OWIN) .NET web sunucuları ve web uygulamaları arasındaki bir Özet tanımlar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-431">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="75cc9-432">OWIN web uygulamaları konak belirsiz yapmadan sunucu web uygulamasından ayrıştırır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-432">OWIN decouples the web application from the server, making web applications host-agnostic.</span></span> <span data-ttu-id="75cc9-433">Örneğin, IIS'de bir OWIN tabanlı web uygulaması konağı ya da özel bir işlem olarak kendini barındırma.</span><span class="sxs-lookup"><span data-stu-id="75cc9-433">For example, you can host an OWIN-based web application in IIS or self-host it in a custom process.</span></span>

<span data-ttu-id="75cc9-434">Microsoft OWIN bileşenleri (Katana proje olarak da bilinir) sunulan değişiklikler, yeni sunucu ve ana bileşenlerini, yeni yardımcı kitaplıkları ve ara yazılımı ve yeni kimlik doğrulaması ara yazılımı içerir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-434">Changes introduced in the Microsoft OWIN components (also known as the Katana project) include new server and host components, new helper libraries and middleware, and new authentication middleware.</span></span>

<span data-ttu-id="75cc9-435">OWIN ve Katana hakkında daha fazla bilgi için bkz: [OWIN ve Katana yenilikler](../../../aspnet/overview/owin-and-katana/index.md).</span><span class="sxs-lookup"><span data-stu-id="75cc9-435">For more information about OWIN and Katana, see [What's new in OWIN and Katana](../../../aspnet/overview/owin-and-katana/index.md).</span></span>

<span data-ttu-id="75cc9-436">**Not: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) uygulamalar IIS Klasik modda çalışamaz; tümleşik modunda çalıştırılması gerekir.**</span><span class="sxs-lookup"><span data-stu-id="75cc9-436">**Note: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) applications cannot run in IIS classic mode; they must be run in integrated mode.**</span></span>

<span data-ttu-id="75cc9-437">**Not: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) uygulamalarını tam güvende çalıştırmanız gerekir.**</span><span class="sxs-lookup"><span data-stu-id="75cc9-437">**Note: [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) applications must be run in full trust.**</span></span>

### <a name="new-servers-and-hosts"></a><span data-ttu-id="75cc9-438">Yeni sunucuları ve ana bilgisayarlar</span><span class="sxs-lookup"><span data-stu-id="75cc9-438">New Servers and Hosts</span></span>

<span data-ttu-id="75cc9-439">Bu sürümle birlikte, kendi kendini barındıran senaryoları etkinleştirmek için yeni bileşenler eklendi.</span><span class="sxs-lookup"><span data-stu-id="75cc9-439">With this release, new components were added to enable self-host scenarios.</span></span> <span data-ttu-id="75cc9-440">Bu bileşenler aşağıdaki NuGet paketleri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="75cc9-440">These components include the following NuGet packages:</span></span>

- <span data-ttu-id="75cc9-441">**Microsoft.Owin.Host.HttpListener**.</span><span class="sxs-lookup"><span data-stu-id="75cc9-441">**Microsoft.Owin.Host.HttpListener**.</span></span> <span data-ttu-id="75cc9-442">Kullanan bir OWIN sunucusu sağlar **HttpListener** HTTP isteklerini dinlemeye ve OWIN ardışık düzenine yönlendirir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-442">Provides an OWIN server that uses **HttpListener** to listen for HTTP requests and direct them into the OWIN pipeline.</span></span>
- <span data-ttu-id="75cc9-443">**Microsoft.Owin.Hosting** bir konsol uygulaması veya Windows hizmeti gibi özel bir işlemde bir OWIN ardışık kendini barındırma isteyen geliştiriciler için bir kitaplık sağlar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-443">**Microsoft.Owin.Hosting** Provides a library for developers who wish to self-host an OWIN pipeline in a custom process, such as a console application or Windows service.</span></span>
- <span data-ttu-id="75cc9-444">**OwinHost**.</span><span class="sxs-lookup"><span data-stu-id="75cc9-444">**OwinHost**.</span></span> <span data-ttu-id="75cc9-445">Tek başına yürütülebilir dosya sarmalar sağlar `Microsoft.Owin.Hosting` ve bir özel ana bilgisayar uygulaması yazmak zorunda kalmadan bir OWIN ardışık kendini barındırma olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-445">Provides a stand-alone executable that wraps `Microsoft.Owin.Hosting` and lets you self-host an OWIN pipeline without having to write a custom host application.</span></span>

<span data-ttu-id="75cc9-446">Ayrıca, `Microsoft.Owin.Host.SystemWeb` paket şimdi için ipuçları sağlamak üzere ara yazılım sağlar **SystemWeb** sunucu, belirli bir ASP.NET ardışık düzen aşaması sırasında ara yazılımın çağrılması gerektiğini belirten.</span><span class="sxs-lookup"><span data-stu-id="75cc9-446">In addition, the `Microsoft.Owin.Host.SystemWeb` package now enables middleware to provide hints to the **SystemWeb** server, indicating that the middleware should be called during a specific ASP.NET pipeline stage.</span></span> <span data-ttu-id="75cc9-447">Bu özellik, erken ASP.NET ardışık düzeninde çalışması gerektiğini kimlik doğrulaması ara yazılımı için özellikle yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-447">This feature is particularly useful for authentication middleware, which should run early in the ASP.NET pipeline.</span></span>

### <a name="helper-libraries-and-middleware"></a><span data-ttu-id="75cc9-448">Yardımcı kitaplıkları ve Ara</span><span class="sxs-lookup"><span data-stu-id="75cc9-448">Helper Libraries and Middleware</span></span>

<span data-ttu-id="75cc9-449">Yalnızca işlev ve tür tanımları OWIN belirtiminden kullanarak OWIN bileşenlerini yazabilirsiniz olsa da yeni `Microsoft.Owin` paket soyutlamalar daha kullanıcı dostu kümesi sağlar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-449">Although you can write OWIN components using only the function and type definitions from the OWIN specification, the new `Microsoft.Owin` package provides a more user-friendly set of abstractions.</span></span> <span data-ttu-id="75cc9-450">Çeşitli önceki paketler bu pakete birleştirir (örn., `Owin.Extensions`, `Owin.Types`) bir tek, iyi yapılandırılmış nesne modeline daha sonra kolayca diğer OWIN bileşenleri tarafından kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-450">This package combines several earlier packages (e.g., `Owin.Extensions`, `Owin.Types`) into a single, well-structured object model that can then be easily used by other OWIN components.</span></span> <span data-ttu-id="75cc9-451">Aslında, Microsoft OWIN bileşenlerin çoğunun şimdi bu paketi kullanan.</span><span class="sxs-lookup"><span data-stu-id="75cc9-451">In fact, the majority of Microsoft OWIN components now use this package.</span></span>

> [!NOTE]
> <span data-ttu-id="75cc9-452">[OWIN](http://www.owin.org) uygulamalar IIS Klasik modda çalışamaz; tümleşik modunda çalıştırılması gerekir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-452">[OWIN](http://www.owin.org) applications cannot run in IIS classic mode; they must be run in integrated mode.</span></span>

> [!NOTE]
> <span data-ttu-id="75cc9-453">[OWIN](http://www.owin.org) uygulamalarını tam güvende çalıştırmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-453">[OWIN](http://www.owin.org) applications must be run in full trust.</span></span>

<span data-ttu-id="75cc9-454">Bu sürüm, çalışan bir OWIN uygulama doğrulamak için ara yazılım artı hataları araştırmanıza yardımcı olması için hata sayfası ara yazılımını içeren Microsoft.Owin.Diagnostics paketi de içerir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-454">This release also includes the Microsoft.Owin.Diagnostics package, which includes middleware to validate a running OWIN application, plus error-page middleware to help investigate failures.</span></span>

### <a name="authentication-components"></a><span data-ttu-id="75cc9-455">Kimlik doğrulaması bileşenleri</span><span class="sxs-lookup"><span data-stu-id="75cc9-455">Authentication Components</span></span>

<span data-ttu-id="75cc9-456">Aşağıdaki kimlik doğrulama bileşenleri kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-456">The following authentication components are available.</span></span>

- <span data-ttu-id="75cc9-457">**Microsoft.Owin.Security.ActiveDirectory**.</span><span class="sxs-lookup"><span data-stu-id="75cc9-457">**Microsoft.Owin.Security.ActiveDirectory**.</span></span> <span data-ttu-id="75cc9-458">Şirket içi veya bulut tabanlı dizin hizmetlerini kullanarak kimlik doğrulamasını etkinleştirir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-458">Enables authentication using on-premise or cloud-based directory services.</span></span>
- <span data-ttu-id="75cc9-459">**Microsoft.Owin.Security.Cookies** etkinleştirir kimlik doğrulaması tanımlama bilgileri kullanma.</span><span class="sxs-lookup"><span data-stu-id="75cc9-459">**Microsoft.Owin.Security.Cookies** Enables authentication using cookies.</span></span> <span data-ttu-id="75cc9-460">Bu paketi daha önce adlı `Microsoft.Owin.Security.Forms`.</span><span class="sxs-lookup"><span data-stu-id="75cc9-460">This package was previously named `Microsoft.Owin.Security.Forms`.</span></span>
- <span data-ttu-id="75cc9-461">**Microsoft.Owin.Security.Facebook** Facebook'ın OAuth tabanlı hizmet kullanarak etkinleştirir kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="75cc9-461">**Microsoft.Owin.Security.Facebook** Enables authentication using Facebook's OAuth-based service.</span></span>
- <span data-ttu-id="75cc9-462">**Microsoft.Owin.Security.Google** etkinleştirir kimlik doğrulaması kullanarak Google Openıd tabanlı hizmet.</span><span class="sxs-lookup"><span data-stu-id="75cc9-462">**Microsoft.Owin.Security.Google** Enables authentication using Google's OpenID-based service.</span></span>
- <span data-ttu-id="75cc9-463">**Microsoft.Owin.Security.Jwt** JWT belirteçleri kullanarak etkinleştirir kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="75cc9-463">**Microsoft.Owin.Security.Jwt** Enables authentication using JWT tokens.</span></span>
- <span data-ttu-id="75cc9-464">**Microsoft.Owin.Security.MicrosoftAccount** Enables authentication using Microsoft accounts.</span><span class="sxs-lookup"><span data-stu-id="75cc9-464">**Microsoft.Owin.Security.MicrosoftAccount** Enables authentication using Microsoft accounts.</span></span>
- <span data-ttu-id="75cc9-465">**Microsoft.Owin.Security.OAuth**.</span><span class="sxs-lookup"><span data-stu-id="75cc9-465">**Microsoft.Owin.Security.OAuth**.</span></span> <span data-ttu-id="75cc9-466">Ara yazılım yanı sıra bir OAuth yetkilendirme sunucusu taşıyıcı belirteçlerin kimlik doğrulamasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-466">Provides an OAuth authorization server as well as middleware for authenticating bearer tokens.</span></span>
- <span data-ttu-id="75cc9-467">**Microsoft.Owin.Security.Twitter** Twitter'ın OAuth tabanlı hizmet kullanarak etkinleştirir kimlik doğrulaması.</span><span class="sxs-lookup"><span data-stu-id="75cc9-467">**Microsoft.Owin.Security.Twitter** Enables authentication using Twitter's OAuth-based service.</span></span>

<span data-ttu-id="75cc9-468">Bu sürüm ayrıca içerir `Microsoft.Owin.Cors` çıkış noktaları arası HTTP isteklerini işlemek için ara yazılımı içeren paket.</span><span class="sxs-lookup"><span data-stu-id="75cc9-468">This release also includes the `Microsoft.Owin.Cors` package, which contains middleware for processing cross-origin HTTP requests.</span></span>

> [!NOTE]
> <span data-ttu-id="75cc9-469">Visual Studio 2013'ın son sürümünde JWT imzalama desteği kaldırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-469">Support for JWT signing has been removed in the final version of Visual Studio 2013.</span></span>

<a id="ef6"></a>
## <a name="entity-framework-6"></a><span data-ttu-id="75cc9-470">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="75cc9-470">Entity Framework 6</span></span>

<span data-ttu-id="75cc9-471">Yeni özellikler ve diğer değişiklikler Entity Framework 6 listesi için bkz: [Entity Framework sürüm geçmişi](https://msdn.com/data/jj574253).</span><span class="sxs-lookup"><span data-stu-id="75cc9-471">For a list of new features and other changes in Entity Framework 6, see [Entity Framework Version History](https://msdn.com/data/jj574253).</span></span>

<a id="TOC14"></a>
## <a name="aspnet-razor-3"></a><span data-ttu-id="75cc9-472">ASP.NET Razor 3</span><span class="sxs-lookup"><span data-stu-id="75cc9-472">ASP.NET Razor 3</span></span>

<span data-ttu-id="75cc9-473">ASP.NET Razor 3 aşağıdaki yeni özellikleri içerir:</span><span class="sxs-lookup"><span data-stu-id="75cc9-473">ASP.NET Razor 3 includes the following new features:</span></span>

- <span data-ttu-id="75cc9-474">Sekme düzenleme desteği.</span><span class="sxs-lookup"><span data-stu-id="75cc9-474">Support for Tab editing.</span></span> <span data-ttu-id="75cc9-475">Preivously, **belgeyi Biçimlendir** komutu, otomatik girintileme ve Visual Studio'da biçimlendirme otomatik işe doğru kullanırken **sekmeleri tut** seçeneği.</span><span class="sxs-lookup"><span data-stu-id="75cc9-475">Preivously, the **Format Document** command, auto indenting, and auto formatting in Visual Studio did not work correctly when using the **Keep Tabs** option.</span></span> <span data-ttu-id="75cc9-476">Bu değişiklik, Visual Studio için biçimlendirme sekmesini Razor kodu biçimlendirme düzeltir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-476">This change corrects Visual Studio formatting for Razor code for tab formatting.</span></span>
- <span data-ttu-id="75cc9-477">Bağlantıları oluşturulurken URL yeniden yazma kuralı desteği.</span><span class="sxs-lookup"><span data-stu-id="75cc9-477">Support for URL Rewrite rules when generating links.</span></span>
- <span data-ttu-id="75cc9-478">Güvenliği saydam öznitelik kaldırma.</span><span class="sxs-lookup"><span data-stu-id="75cc9-478">Removal of security transparent attribute.</span></span>
  > [!NOTE]
  > <span data-ttu-id="75cc9-479">Bu önemli bir değişiklik ve Razor 2 MVC5 veya MVC5 karşı derlenmiş derlemeler ile uyumsuz durumdayken Razor 3 MVC4 ve önceki sürümlerinde, uyumsuz hale getirir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-479">This is a breaking change, and makes Razor 3 incompatible with MVC4 and earlier, while Razor 2 is incompatible with MVC5 or assemblies compiled against MVC5.</span></span>

<span data-ttu-id="75cc9-480">Visual Studio 2013'te yayın öncesi sürümlerinden sabit razor 3 sorunları bulunabilir [burada](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).</span><span class="sxs-lookup"><span data-stu-id="75cc9-480">Razor 3 issues fixed in Visual Studio 2013 from pre-release versions can be found [here](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Resolved%7cClosed&type=All&priority=All&release=All%7cv5.0%2bPreview%7cv5.0%2bRC%7cv5.0%2bRTM&assignedTo=All&component=Web%2bPages%252fRazor&reasonClosed=Fixed&sortField=LastUpdatedDate&sortDirection=Descending&page=0).</span></span>

<a id="TOC15"></a>
## <a name="aspnet-app-suspend"></a><span data-ttu-id="75cc9-481">ASP.NET uygulama askıya alma</span><span class="sxs-lookup"><span data-stu-id="75cc9-481">ASP.NET App Suspend</span></span>

<span data-ttu-id="75cc9-482">ASP.NET uygulama askıya alma tüketicisinin kullanıcı deneyimi ve çok sayıda tek bir makinede ASP.NET siteleri barındırmak için ekonomik model değişiklikleri .NET Framework 4.5.1'deki oyun değiştirme bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-482">ASP.NET App Suspend is a game-changing feature in the .NET Framework 4.5.1 that radically changes the user experience and economic model for hosting large numbers of ASP.NET sites on a single machine.</span></span> <span data-ttu-id="75cc9-483">Daha fazla bilgi için bkz: [ASP.NET uygulama Askıya Al – esnek paylaşılan .NET web barındırma](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).</span><span class="sxs-lookup"><span data-stu-id="75cc9-483">For more information, see [ASP.NET App Suspend – responsive shared .NET web hosting](https://blogs.msdn.com/b/dotnet/archive/2013/10/09/asp-net-app-suspend-responsive-shared-net-web-hosting.aspx).</span></span>

<a id="knownissues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="75cc9-484">Bilinen sorunlar ve yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="75cc9-484">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="75cc9-485">Bu bölümde, Visual Studio 2013 için bilinen sorunlar ve ASP.NET ve Web Araçları önemli değişiklikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-485">This section describes known issues and breaking changes in the ASP.NET and Web Tools for Visual Studio 2013.</span></span>

### <a name="nuget"></a><span data-ttu-id="75cc9-486">NuGet</span><span class="sxs-lookup"><span data-stu-id="75cc9-486">NuGet</span></span>

- <span data-ttu-id="75cc9-487">[SLN dosyasını kullanırken, yeni paket geri yüklemesi üzerinde Mono işe yaramazsa](https://nuget.codeplex.com/workitem/3596) – bir yaklaşan nuget.exe indirme düzeltilecektir ve [NuGet.CommandLine paket](http://www.nuget.org/packages/NuGet.CommandLine/) güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="75cc9-487">[New package restore doesn't work on Mono when using SLN file](https://nuget.codeplex.com/workitem/3596) – will be fixed in an upcoming nuget.exe download and [NuGet.CommandLine package](http://www.nuget.org/packages/NuGet.CommandLine/) update.</span></span>
- <span data-ttu-id="75cc9-488">[Yeni paket geri yüklemesi Wix projelerle işe yaramazsa](https://nuget.codeplex.com/workitem/3598) – bir yaklaşan nuget.exe indirme düzeltilecektir ve [NuGet.CommandLine paket](http://www.nuget.org/packages/NuGet.CommandLine/) güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="75cc9-488">[New package restore doesn't work with Wix projects](https://nuget.codeplex.com/workitem/3598) – will be fixed in an upcoming nuget.exe download and [NuGet.CommandLine package](http://www.nuget.org/packages/NuGet.CommandLine/) update.</span></span>
- <span data-ttu-id="75cc9-489">[Otomatik paket geri yüklemesi çözüm klasörünün altında projeleri için işe yaramazsa](https://nuget.codeplex.com/workitem/3625) – NuGet 2.8 düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-489">[Automatic Package restore doesn't work for projects under a solution folder](https://nuget.codeplex.com/workitem/3625) – will be fixed in NuGet 2.8.</span></span>

### <a name="aspnet-web-api"></a><span data-ttu-id="75cc9-490">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="75cc9-490">ASP.NET Web API</span></span>

1. <span data-ttu-id="75cc9-491">`ODataQueryOptions<T>.ApplyTo(IQueryable)` döndürmez `IQueryable<T>` desteği eklediğimiz olarak her zaman `$select` ve `$expand`.</span><span class="sxs-lookup"><span data-stu-id="75cc9-491">`ODataQueryOptions<T>.ApplyTo(IQueryable)` doesn't return `IQueryable<T>` always, as we added support for `$select` and `$expand`.</span></span>

    <span data-ttu-id="75cc9-492">Önceki örneklerimizi `ODataQueryOptions<T>` her zaman Integer dönüş değeri `ApplyTo` için `IQueryable<T>`.</span><span class="sxs-lookup"><span data-stu-id="75cc9-492">Our earlier samples for `ODataQueryOptions<T>` always casted the return value from `ApplyTo` to `IQueryable<T>`.</span></span> <span data-ttu-id="75cc9-493">Bu sorgu seçenekleri olduğundan daha önce çalışan biz daha önce desteklenen (`$filter`, `$orderby`, `$skip`, `$top`) şekli sorgusunun değiştirmeyin.</span><span class="sxs-lookup"><span data-stu-id="75cc9-493">This worked earlier because the query options that we supported earlier (`$filter`, `$orderby`, `$skip`, `$top`) do not change the shape of the query.</span></span> <span data-ttu-id="75cc9-494">Destekliyoruz göre `$select` ve `$expand` yönteminden döndürülen değer `ApplyTo` değişmeyecek `IQueryable<T>` her zaman.</span><span class="sxs-lookup"><span data-stu-id="75cc9-494">Now that we support `$select` and `$expand` the return value from `ApplyTo` will not be `IQueryable<T>` always.</span></span>

    [!code-csharp[Main](release-notes/samples/sample21.cs)]

    <span data-ttu-id="75cc9-495">Örnek kodu daha önce kullanıyorsanız, istemci değil gönderirseniz, çalışmaya devam eder `$select` ve `$expand`.</span><span class="sxs-lookup"><span data-stu-id="75cc9-495">If you are using the sample code from earlier, it will continue working if the client does not send `$select` and `$expand`.</span></span> <span data-ttu-id="75cc9-496">Ancak, desteklemek istiyorsanız `$select` ve `$expand` için bu kodu değiştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-496">However, if you wish to support `$select` and `$expand` you have to change that code to this.</span></span>

    [!code-csharp[Main](release-notes/samples/sample22.cs)]
2. <span data-ttu-id="75cc9-497">**Request.Url veya RequestContext.Url toplu isteği sırasında null**</span><span class="sxs-lookup"><span data-stu-id="75cc9-497">**Request.Url or RequestContext.Url is null during a batch request**</span></span>

    <span data-ttu-id="75cc9-498">Bir toplu senaryosunda **UrlHelper** erişilen zaman null **Request.Url** veya **RequestContext.Url**.</span><span class="sxs-lookup"><span data-stu-id="75cc9-498">In a batching scenario, **UrlHelper** is null when accessed from **Request.Url** or **RequestContext.Url**.</span></span>

    <span data-ttu-id="75cc9-499">Bu sorun şu anda burada izlenir: [BatchRequestContext.Url isteği toplu işleme için null](http://aspnetwebstack.codeplex.com/workitem/1301).</span><span class="sxs-lookup"><span data-stu-id="75cc9-499">This issue is currently tracked here: [BatchRequestContext.Url is null for batching request](http://aspnetwebstack.codeplex.com/workitem/1301).</span></span>

    <span data-ttu-id="75cc9-500">Yeni bir örneğini oluşturmak için bu sorun için geçici çözüm olan **UrlHelper**, aşağıdaki örnekte olduğu gibi:</span><span class="sxs-lookup"><span data-stu-id="75cc9-500">The workaround for this issue is to create a new instance of **UrlHelper**, as in the following example:</span></span>

    <span data-ttu-id="75cc9-501">**UrlHelper yeni bir örneğini oluşturma**</span><span class="sxs-lookup"><span data-stu-id="75cc9-501">**Creating a new instance of UrlHelper**</span></span>

    [!code-csharp[Main](release-notes/samples/sample23.cs)]

### <a name="aspnet-mvc"></a><span data-ttu-id="75cc9-502">ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="75cc9-502">ASP.NET MVC</span></span>

1. <span data-ttu-id="75cc9-503">Veri görünümüne duyurduğumuzda AntiForgerToken doğrulama yapmak görünümler varsa MVC5 ve OrgAuth, kullanırken, aşağıdaki hata arasında gelebilir:</span><span class="sxs-lookup"><span data-stu-id="75cc9-503">When using MVC5 and OrgAuth, if you have views which do AntiForgerToken validation, you might come across the following error when you post data to the view:</span></span>

    <span data-ttu-id="75cc9-504">**Hata**:</span><span class="sxs-lookup"><span data-stu-id="75cc9-504">**Error**:</span></span>

    <span data-ttu-id="75cc9-505">*'/' Uygulamasında sunucu hatası.*</span><span class="sxs-lookup"><span data-stu-id="75cc9-505">*Server Error in '/' Application.*</span></span>

    <span data-ttu-id="75cc9-506"><em>Bir talep türü '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>'veya'<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' üzerinde sağlanan Claimsıdentity yoktu. Talep tabanlı kimlik doğrulaması ile sahteciliğe karşı koruma belirteci desteğini etkinleştirmek için lütfen yapılandırılmış Talep sağlayıcı ürettiği Claimsıdentity örneklerinde bu talepler her ikisi de sağladığını doğrulayın. Yapılandırılmış Talep sağlayıcı yerine farklı bir talep türüyle benzersiz bir tanımlayıcı olarak kullanıyorsa, AntiForgeryConfig.UniqueClaimTypeIdentifier statik özelliği ayarlanarak yapılandırılabilir.</em></span><span class="sxs-lookup"><span data-stu-id="75cc9-506"><em>A claim of type '<http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier>' or '<http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider>' was not present on the provided ClaimsIdentity. To enable anti-forgery token support with claims-based authentication, please verify that the configured claims provider is providing both of these claims on the ClaimsIdentity instances it generates. If the configured claims provider instead uses a different claim type as a unique identifier, it can be configured by setting the static property AntiForgeryConfig.UniqueClaimTypeIdentifier.</em></span></span>

    <span data-ttu-id="75cc9-507">**Geçici çözüm**:</span><span class="sxs-lookup"><span data-stu-id="75cc9-507">**Workaround**:</span></span>

    <span data-ttu-id="75cc9-508">Sorunu gidermek için Global.asax dosyasında aşağıdaki satırı ekleyin:</span><span class="sxs-lookup"><span data-stu-id="75cc9-508">Add the following line in Global.asax to fix it:</span></span>

    `AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.Name;`

    <span data-ttu-id="75cc9-509">Bu, bir sonraki sürümde düzeltilecektir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-509">This will be fixed for the next release.</span></span>
2. <span data-ttu-id="75cc9-510">MVC4 uygulaması için MVC5 yükseltme yaptıktan sonra çözümü oluşturun ve başlatın.</span><span class="sxs-lookup"><span data-stu-id="75cc9-510">After upgrading an MVC4 app to MVC5, build the solution and launch it.</span></span> <span data-ttu-id="75cc9-511">Aşağıdaki hata görmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="75cc9-511">You should see the following error:</span></span>

    <span data-ttu-id="75cc9-512">[A]System.Web.WebPages.Razor.Configuration.HostSection cannot be cast to [B]System.Web.WebPages.Razor.Configuration.HostSection.</span><span class="sxs-lookup"><span data-stu-id="75cc9-512">[A]System.Web.WebPages.Razor.Configuration.HostSection cannot be cast to [B]System.Web.WebPages.Razor.Configuration.HostSection.</span></span> <span data-ttu-id="75cc9-513">Tür A kaynaklanan ' System.Web.WebPages.Razor, sürüm 2.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' konumunda ' Default' bağlamda ' C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\ v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'.</span><span class="sxs-lookup"><span data-stu-id="75cc9-513">Type A originates from 'System.Web.WebPages.Razor, Version=2.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\windows\Microsoft.Net\assembly\GAC\_MSIL\System.Web.WebPages.Razor\v4.0\_2.0.0.0\_\_31bf3856ad364e35\System.Web.WebPages.Razor.dll'.</span></span> <span data-ttu-id="75cc9-514">Tür B kaynaklanan ' System.Web.WebPages.Razor, sürüm 3.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' konumunda ' Default' bağlamda ' C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\ e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.</span><span class="sxs-lookup"><span data-stu-id="75cc9-514">Type B originates from 'System.Web.WebPages.Razor, Version=3.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' in the context 'Default' at location 'C:\Windows\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files\root\6d05bbd0\e8b5908e\assembly\dl3\c9cbca63\f8910382\_6273ce01\System.Web.WebPages.Razor.dll'.</span></span>

    <span data-ttu-id="75cc9-515">Yukarıdaki hatayı düzeltmek için açık *tüm* (görünümler klasöründe olanlar dahil) Web.config dosyasında projenizi ve aşağıdakileri yapın:</span><span class="sxs-lookup"><span data-stu-id="75cc9-515">To fix the above error, open *all* the Web.config files (including the ones in the Views folder) in your project and do the following:</span></span>

   1. <span data-ttu-id="75cc9-516">Sürümüne "4.0.0.0" "System.Web.Mvc" "5.0.0.0" tüm oluşumlarını güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="75cc9-516">Update all occurrences of version "4.0.0.0" of "System.Web.Mvc" to "5.0.0.0".</span></span>
   2. <span data-ttu-id="75cc9-517">"System.Web.Helpers", "2.0.0.0" sürümünü tüm oluşumlarını güncelleştirme &quot;System.Web.WebPages&quot; ve &quot;System.Web.WebPages.Razor&quot; "3.0.0.0" için</span><span class="sxs-lookup"><span data-stu-id="75cc9-517">Update all occurrences of version "2.0.0.0" of "System.Web.Helpers", &quot;System.Web.WebPages&quot; and &quot;System.Web.WebPages.Razor&quot; to "3.0.0.0"</span></span>

      <span data-ttu-id="75cc9-518">Örneğin, yukarıdaki değişiklikleri yaptıktan sonra derleme bağlamaları aşağıdaki gibi görünmelidir:</span><span class="sxs-lookup"><span data-stu-id="75cc9-518">For example, after you make the above changes, the assembly bindings should look like this:</span></span>

      [!code-xml[Main](release-notes/samples/sample24.xml)]

      <span data-ttu-id="75cc9-519">MVC 4 projeleri MVC 5'e yükseltme hakkında daha fazla bilgi için bkz: [bir ASP.NET MVC 4 ve Web API projesi ASP.NET MVC 5 ve Web API 2'ye yükseltme](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="75cc9-519">For information on upgrading MVC 4 projects to MVC 5, see [How to Upgrade an ASP.NET MVC 4 and Web API Project to ASP.NET MVC 5 and Web API 2](../../../mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).</span></span>
3. <span data-ttu-id="75cc9-520">İstemci tarafı doğrulama örtük doğrulama jQuery ile kullanırken, doğrulama iletisi bazen türüne sahip bir HTML input öğesi için geçersiz = 'number'.</span><span class="sxs-lookup"><span data-stu-id="75cc9-520">When using client-side validation with jQuery Unobtrusive Validation, the validation message is sometimes incorrect for an HTML input element with type='number'.</span></span> <span data-ttu-id="75cc9-521">Doğrulama hatasını gerekli değeri ("Yaş alanı gereklidir") geçersiz bir sayı geçerli bir sayı gerekli olduğunu doğru ileti yerine girildiğinde gösterilir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-521">The validation error for a required value ("The Age field is required") is shown when an invalid number is entered instead of the correct message that a valid number is required.</span></span>

    <span data-ttu-id="75cc9-522">Bu sorun sık kurulmuş kodu ile oluşturma ve düzenleme görünümleri bir tamsayı özelliği ile bir model için bulunamadı.</span><span class="sxs-lookup"><span data-stu-id="75cc9-522">This issue is commonly found with scaffolded code for a model with an integer property on the Create and Edit views.</span></span>

    <span data-ttu-id="75cc9-523">Bu sorunu çözmek için gelen Düzenleyicisi yardımcı değiştirin:</span><span class="sxs-lookup"><span data-stu-id="75cc9-523">To work around this issue, change the editor helper from:</span></span>

    `@Html.EditorFor(person => person.Age)`

    <span data-ttu-id="75cc9-524">Hedef:</span><span class="sxs-lookup"><span data-stu-id="75cc9-524">To:</span></span>

    `@Html.TextBoxFor(person => person.Age)`
4. <span data-ttu-id="75cc9-525">ASP.NET MVC 5 artık kısmi güven destekler.</span><span class="sxs-lookup"><span data-stu-id="75cc9-525">ASP.NET MVC 5 no longer supports partial trust.</span></span> <span data-ttu-id="75cc9-526">MVC veya Webapı ikili dosyaları bağlama projeleri kaldırmalısınız [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) özniteliği ve [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) özniteliği.</span><span class="sxs-lookup"><span data-stu-id="75cc9-526">Projects linking to the MVC or WebAPI binaries should remove the [SecurityTransparent](https://msdn.microsoft.com/library/system.security.securitytransparentattribute.aspx) attribute and the [AllowPartiallyTrustedCallers](https://msdn.microsoft.com/library/system.security.allowpartiallytrustedcallersattribute.aspx) attribute.</span></span> <span data-ttu-id="75cc9-527">Bu öznitelikler kaldırma derleyici hataları aşağıdaki gibi ortadan kaldırır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-527">Removing these attributes will eliminate compiler errors such as the following.</span></span>

    `Attempt by security transparent method ‘MyComponent' to access security critical type 'System.Web.Mvc.MvcHtmlString' failed. Assembly 'PagedList.Mvc, Version=4.3.0.0, Culture=neutral, PublicKeyToken=abbb863e9397c5e1' is marked with the AllowPartiallyTrustedCallersAttribute, and uses the level 2 security transparency model. Level 2 transparency causes all methods in AllowPartiallyTrustedCallers assemblies to become security transparent by default, which may be the cause of this exception.`

    > <span data-ttu-id="75cc9-528">Not, bir yan etkisi Bu, aynı uygulamada 4.0 ve 5.0 derlemeleri kullanamazsınız.</span><span class="sxs-lookup"><span data-stu-id="75cc9-528">Note, as a side effect of this you cannot use 4.0 and 5.0 assemblies in the same application.</span></span> <span data-ttu-id="75cc9-529">Bunların tümünün 5.0 güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-529">You need to update all of them to 5.0.</span></span>

### <a name="spa-template-with-facebook-authorization-may-cause-instability-in-ie-while-the-web-site-is-hosted-in-intranet-zone"></a><span data-ttu-id="75cc9-530">Web sitesi intranet bölgesinde barındırılmasına karşın Facebook yetkilendirme SPA şablonla IE kararsızlığına neden olabilir</span><span class="sxs-lookup"><span data-stu-id="75cc9-530">SPA Template with Facebook authorization may cause instability in IE while the web site is hosted in intranet zone</span></span>

<span data-ttu-id="75cc9-531">SPA şablonu Facebook dış oturum sağlar.</span><span class="sxs-lookup"><span data-stu-id="75cc9-531">The SPA template provides external log in with Facebook.</span></span> <span data-ttu-id="75cc9-532">Şablon kullanılarak oluşturulan projeyi yerel olarak çalıştırırken, oturum açma IE çökmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-532">When the project created with the template is running locally, signing in may cause IE to crash.</span></span>

<span data-ttu-id="75cc9-533">Çözüm:</span><span class="sxs-lookup"><span data-stu-id="75cc9-533">Solution:</span></span>

1. <span data-ttu-id="75cc9-534">Web sitesi Internet bölgesinde barındırması; veya</span><span class="sxs-lookup"><span data-stu-id="75cc9-534">Host the web site in internet zone; or</span></span>

2. <span data-ttu-id="75cc9-535">IE dışında bir tarayıcıda senaryosunu sınayın.</span><span class="sxs-lookup"><span data-stu-id="75cc9-535">Test the scenario in a browser other than IE.</span></span>

### <a name="web-forms-scaffolding"></a><span data-ttu-id="75cc9-536">Web Forms Scaffolding</span><span class="sxs-lookup"><span data-stu-id="75cc9-536">Web Forms Scaffolding</span></span>

<span data-ttu-id="75cc9-537">Web Forms İskele VS2013 kaldırıldı ve Visual Studio için gelecekteki bir güncelleştirme kullanıma sunulacaktır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-537">Web Forms Scaffolding has been removed from VS2013 and will be available in a future update to Visual Studio.</span></span> <span data-ttu-id="75cc9-538">Ancak, MVC bağımlılıkları ekleyerek ve yapı iskelesi MVC için oluşturma Web Forms projedeki yapı iskelesi kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="75cc9-538">However, you can still use scaffolding within a Web Forms project by adding MVC dependencies and generating scaffolding for MVC.</span></span> <span data-ttu-id="75cc9-539">Projenizi Web Forms ve MVC bir birleşimini içerir.</span><span class="sxs-lookup"><span data-stu-id="75cc9-539">Your project will contain a combination of Web Forms and MVC.</span></span>

<span data-ttu-id="75cc9-540">MVC Web Forms projenize eklemek için yeni iskele kurulmuş Öğe Ekle ve seçin **MVC 5 bağımlılıkları**.</span><span class="sxs-lookup"><span data-stu-id="75cc9-540">To add MVC to your Web Forms project, add a new scaffolded item and select **MVC 5 Dependencies**.</span></span> <span data-ttu-id="75cc9-541">En az veya betikleri gibi içerik tüm dosyaların gerekmediğini bağlı olarak tam seçin.</span><span class="sxs-lookup"><span data-stu-id="75cc9-541">Select either Minimal or Full depending on whether you need all of the content files, such as scripts.</span></span> <span data-ttu-id="75cc9-542">Ardından, görünümler ve denetleyicisi projenizde oluşturacak MVC için kurulmuş öğe ekleyin.</span><span class="sxs-lookup"><span data-stu-id="75cc9-542">Then, add a scaffolded item for MVC, which will create views and a controller in your project.</span></span>

### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a><span data-ttu-id="75cc9-543">MVC and Web API Scaffolding - HTTP 404, Not Found error</span><span class="sxs-lookup"><span data-stu-id="75cc9-543">MVC and Web API Scaffolding - HTTP 404, Not Found error</span></span>

<span data-ttu-id="75cc9-544">Bir projeye iskele kurulmuş öğe olduğunda hatayla karşılaşırsanız, projenizin tutarsız bir durumda bırakılır mümkündür.</span><span class="sxs-lookup"><span data-stu-id="75cc9-544">If an error is encountered when adding a scaffolded item to a project, it is possible your project will be left in an inconsistent state.</span></span> <span data-ttu-id="75cc9-545">Bazı yapı iskelesi yapılan değişiklikler geri alınacak ancak yüklü NuGet paketleri gibi diğer değişiklikler geri alınacak değil.</span><span class="sxs-lookup"><span data-stu-id="75cc9-545">Some of the changes made be scaffolding will be rolled back but other changes, such as the installed NuGet packages, will not be rolled back.</span></span> <span data-ttu-id="75cc9-546">Yönlendirme yapılandırması değişiklikler geri alınır, gezinme öğeleri iskele kurulmuş kullanıcılar bir HTTP 404 hata alır.</span><span class="sxs-lookup"><span data-stu-id="75cc9-546">If the routing configuration changes are rolled back, users will receive an HTTP 404 error when navigating to scaffolded items.</span></span>

<span data-ttu-id="75cc9-547">Geçici çözüm:</span><span class="sxs-lookup"><span data-stu-id="75cc9-547">Workaround:</span></span>

- <span data-ttu-id="75cc9-548">MVC için bu hatayı düzeltmek için yeni iskele kurulmuş Öğe Ekle ve MVC 5 bağımlılıkları seçin (en az veya tam).</span><span class="sxs-lookup"><span data-stu-id="75cc9-548">To fix this error for MVC, add a new scaffolded item and select MVC 5 Dependencies (either Minimal or Full).</span></span> <span data-ttu-id="75cc9-549">Bu işlem tüm gerekli değişiklikleri projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="75cc9-549">This process will add all of the required changes to your project.</span></span>
- <span data-ttu-id="75cc9-550">Web API için bu hatayı düzeltmek için:</span><span class="sxs-lookup"><span data-stu-id="75cc9-550">To fix this error for Web API:</span></span>

  1. <span data-ttu-id="75cc9-551">WebApiConfig sınıfı projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="75cc9-551">Add the WebApiConfig class to your project.</span></span>

      [!code-csharp[Main](release-notes/samples/sample25.cs)]

      [!code-vb[Main](release-notes/samples/sample26.vb)]
  2. <span data-ttu-id="75cc9-552">Uygulamada WebApiConfig.Register yapılandırma\_yöntemi Global.asax dosyasında şu şekilde başlatın:</span><span class="sxs-lookup"><span data-stu-id="75cc9-552">Configure WebApiConfig.Register in the Application\_Start method in Global.asax as follows:</span></span>

      [!code-csharp[Main](release-notes/samples/sample27.cs)]

      [!code-vb[Main](release-notes/samples/sample28.vb)]
