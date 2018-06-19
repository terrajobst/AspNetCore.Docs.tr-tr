---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Visual Studio 2013'te ASP.NET İskele | Microsoft Docs
author: tfitzmac
description: ASP.NET İskele Visual Studio 2013'te bulunan yeni bir özelliktir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
ms.locfileid: "26566325"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="bb177-103">Visual Studio 2013'te ASP.NET İskele</span><span class="sxs-lookup"><span data-stu-id="bb177-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="bb177-104">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bb177-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="bb177-105">ASP.NET İskele Visual Studio 2013'te bulunan yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="bb177-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="bb177-106">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="bb177-106">Overview</span></span>

<span data-ttu-id="bb177-107">ASP.NET İskele bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="bb177-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="bb177-108">Visual Studio 2013 MVC ve Web API projeleri için önceden yüklenmiş kod oluşturucuları içerir.</span><span class="sxs-lookup"><span data-stu-id="bb177-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="bb177-109">Hızlı veri modelleri ile etkileşime giren kodu eklemek istediğinizde yapı iskelesi projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bb177-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="bb177-110">Yapı iskelesi kullanarak standart veri işlemleri projenizdeki geliştirmek için süreyi azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="bb177-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="bb177-111">Varsayılan olarak, Visual Studio 2013 için bir Web Forms projesi oluşturma kodunu desteklemez ancak yapı iskelesi projeye MVC bağımlılıkları ekleme veya uzantı yükleme Web Forms ile kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb177-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="bb177-112">Her iki yaklaşımın aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="bb177-112">Both approaches are shown below.</span></span>

<span data-ttu-id="bb177-113">Visual Studio 2013 güncelleştirme 2 (şu anda RC), ASP.NET, senaryonuzun gereksinimlerini karşılamak için yapı İskelesi genişletme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="bb177-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="bb177-114">Bu işlev ile bir özelleştirilmiş yapı iskelesi şablonu oluşturun ve yeni İskele Ekle iletişim kutusuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bb177-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="bb177-115">Özelleştirilmiş şablonda iskele kurulmuş öğe zaman oluşturulan kodu belirtin.</span><span class="sxs-lookup"><span data-stu-id="bb177-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="bb177-116">Daha fazla bilgi için bkz: [Visual Studio için özel iskele kurucu oluşturma](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="bb177-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bb177-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="bb177-117">Prerequisites</span></span>

<span data-ttu-id="bb177-118">ASP.NET İskele kullanmak için olması gerekir:</span><span class="sxs-lookup"><span data-stu-id="bb177-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="bb177-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bb177-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="bb177-120">Web geliştirici araçları (varsayılan Visual Studio 2013 yüklemesinin parçası)</span><span class="sxs-lookup"><span data-stu-id="bb177-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="bb177-121">ASP.NET Web çerçeveleri ve araçları 2013 (varsayılan Visual Studio 2013 yüklemesinin parçası)</span><span class="sxs-lookup"><span data-stu-id="bb177-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="bb177-122">MVC veya Web API kurulmuş öğe ekleme</span><span class="sxs-lookup"><span data-stu-id="bb177-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="bb177-123">İskele eklemek için proje veya proje içindeki bir klasörü sağ tıklatın ve seçin **Ekle** – **yeni iskele kurulmuş öğe**aşağıdaki görüntüde gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="bb177-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![İskele öğesi ekleme](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="bb177-125">Gelen **İskele Ekle** penceresinde eklemek için iskele türünü seçin.</span><span class="sxs-lookup"><span data-stu-id="bb177-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![İskele türünü seçin](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="bb177-127">**Denetleyici Ekle** penceresi Entity Framework 6 yeni zaman uyumsuz özelliklerinden kullanmak mı istediğinizi de dahil olmak üzere, denetleyici oluşturma seçeneklerini belirlemek için fırsatı sağlar.</span><span class="sxs-lookup"><span data-stu-id="bb177-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Denetleyici ekleme](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="bb177-129">Sayfalar ve ilgili sınıfların senaryonuz için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="bb177-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="bb177-130">Örneğin, aşağıdaki resimde MVC denetleyicisi ve filmler adlı bir model sınıfı için yapı iskelesi ile oluşturulan görünümleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="bb177-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Oluşturulan dosyalar](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="bb177-132">Web formları için kurulmuş Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="bb177-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="bb177-133">Web Forms kod oluşturur yapı iskelesi eklemek için bir uzantı için Visual Studio yüklemek veya MVC bağımlılıkları ekleyin gerekir.</span><span class="sxs-lookup"><span data-stu-id="bb177-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="bb177-134">Her iki yaklaşımın aşağıda gösterilmektedir, ancak yalnızca bu yaklaşımlardan birini yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="bb177-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="bb177-135">Web Forms uzantısı iskele kurma</span><span class="sxs-lookup"><span data-stu-id="bb177-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="bb177-136">Web Forms projeyle yapı iskelesi kullanmanıza olanak sağlayan bir Visual Studio uzantısı yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb177-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="bb177-137">Visual Studio'da seçin **Araçları** ve ardından **Uzantılar ve güncelleştirmeler**.</span><span class="sxs-lookup"><span data-stu-id="bb177-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="bb177-138">Visual Studio Galerisi için bu iletişim kutusundan arama **Web Forms İskele**.</span><span class="sxs-lookup"><span data-stu-id="bb177-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![web forms iskele yükleyin](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="bb177-140">Daha fazla bilgi için bkz: [Web Forms İskele](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="bb177-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="bb177-141">MVC bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="bb177-141">MVC Dependencies</span></span>

<span data-ttu-id="bb177-142">MVC bağımlılıkları eklemek için seçin **Ekle** - **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="bb177-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="bb177-143">İskele Ekle penceresinde seçin **MVC bağımlılıkları**, aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="bb177-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![MVC bağımlılıkları ekleyin.](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="bb177-145">MVC iskele için iki seçenek vardır; Minimal ve tam.</span><span class="sxs-lookup"><span data-stu-id="bb177-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="bb177-146">En az seçerseniz, yalnızca NuGet paketlerini ve ASP.NET MVC için başvuruları projenize eklenir.</span><span class="sxs-lookup"><span data-stu-id="bb177-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="bb177-147">Tam seçeneğini belirlerseniz MVC projesinde gerekli içerik dosyaların yanı sıra en az bağımlılıkları eklenir.</span><span class="sxs-lookup"><span data-stu-id="bb177-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="bb177-148">Yapı iskelesi kolayca kullanmak için tam bağımlılıkları seçin.</span><span class="sxs-lookup"><span data-stu-id="bb177-148">To easily use scaffolding, select Full dependencies.</span></span>

![Tam bağımlılıkları seçin](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="bb177-150">Göreceğiniz bağımlılıkları eklendikten sonra bir **readme.txt** dosya.</span><span class="sxs-lookup"><span data-stu-id="bb177-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="bb177-151">Dikkatle projenizi düzgün çalıştığından emin olmak için bu dosyayı'ndaki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="bb177-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="bb177-152">Readme.txt dosyasında adımlarını tamamladıktan sonra MVC ve Web API hakkında önceki bölümde gösterildiği gibi yeni iskele kurulmuş öğe ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bb177-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="bb177-153">Otomatik olarak oluşturulan görünümleri ve denetleyici projenizi içinde düzgün çalışacaktır.</span><span class="sxs-lookup"><span data-stu-id="bb177-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="bb177-154">Öğreticiler</span><span class="sxs-lookup"><span data-stu-id="bb177-154">Tutorials</span></span>

<span data-ttu-id="bb177-155">Özelleştirilmiş iskele kurucu oluşturmak için bkz: [Visual Studio için özel iskele kurucu oluşturma](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="bb177-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="bb177-156">Oluşturulan dosyalar özelleştirmek için bkz: [yeni iskele kurulmuş öğe iletişim oluşturulan dosyalardan özelleştirmek nasıl](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb177-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="bb177-157">Yapı iskelesi ile kullanma örneği için **Database First geliştirme**, bkz: [EF veritabanı ilk ASP.NET MVC ile](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="bb177-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="bb177-158">Yapı iskelesi içinde kullanma örneği için bir **MVC** proje için bkz: [ASP.NET MVC 5 ile çalışmaya başlama](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="bb177-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="bb177-159">Yapı iskelesi içinde kullanma örneği için bir **Web API** proje için bkz: [özniteliği yönlendirme Web API 2'deki bir REST API'si oluşturma](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="bb177-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
