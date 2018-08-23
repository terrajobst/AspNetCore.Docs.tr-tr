---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Visual Studio 2013'te ASP.NET iskeleti oluşturma | Microsoft Docs
author: tfitzmac
description: ASP.NET iskeleti oluşturma Visual Studio 2013'te bulunan yeni bir özelliktir.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: c554f592e57f2e6017f7fcfcc9b4c98051e21b37
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755180"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a><span data-ttu-id="74b6c-103">Visual Studio 2013'te ASP.NET iskeleti oluşturma</span><span class="sxs-lookup"><span data-stu-id="74b6c-103">ASP.NET Scaffolding in Visual Studio 2013</span></span>
====================
<span data-ttu-id="74b6c-104">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="74b6c-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="74b6c-105">ASP.NET iskeleti oluşturma Visual Studio 2013'te bulunan yeni bir özelliktir.</span><span class="sxs-lookup"><span data-stu-id="74b6c-105">ASP.NET Scaffolding is a new feature that is included in Visual Studio 2013.</span></span>


## <a name="overview"></a><span data-ttu-id="74b6c-106">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="74b6c-106">Overview</span></span>

<span data-ttu-id="74b6c-107">ASP.NET iskeleti oluşturma bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir.</span><span class="sxs-lookup"><span data-stu-id="74b6c-107">ASP.NET Scaffolding is a code generation framework for ASP.NET Web applications.</span></span> <span data-ttu-id="74b6c-108">Visual Studio 2013, MVC ve Web API projeleri için önceden yüklenmiş kod oluşturucuları içerir.</span><span class="sxs-lookup"><span data-stu-id="74b6c-108">Visual Studio 2013 includes pre-installed code generators for MVC and Web API projects.</span></span> <span data-ttu-id="74b6c-109">Hızlı bir şekilde veri modelleri ile etkileşime giren kodu eklemek istediğinizde yapı iskelesi projenize ekleyin.</span><span class="sxs-lookup"><span data-stu-id="74b6c-109">You add scaffolding to your project when you want to quickly add code that interacts with data models.</span></span> <span data-ttu-id="74b6c-110">Yapı iskelesini kullanmaya projenizdeki standart veri işlemlerini geliştirmek için gereken süreyi azaltabilir.</span><span class="sxs-lookup"><span data-stu-id="74b6c-110">Using scaffolding can reduce the amount of time to develop standard data operations in your project.</span></span>

<span data-ttu-id="74b6c-111">Varsayılan olarak, Visual Studio 2013 Web formları projesi için kod desteklemez, ancak yapı iskelesi MVC bağımlılıkları projeye eklenirken veya bir uzantının yüklenmesi Web Forms ile kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="74b6c-111">By default, Visual Studio 2013 does not support generating code for a Web Forms project, but you can use scaffolding with Web Forms by either adding MVC dependencies to the project or installing an extension.</span></span> <span data-ttu-id="74b6c-112">Her iki yaklaşım aşağıda gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="74b6c-112">Both approaches are shown below.</span></span>

<span data-ttu-id="74b6c-113">Visual Studio 2013 güncelleştirme 2 (şu anda RC), ASP.NET, senaryonuzun gereksinimlerini karşılamak için yapı İskelesi genişletme olanağı sağlar.</span><span class="sxs-lookup"><span data-stu-id="74b6c-113">Visual Studio 2013 Update 2 (currently RC) provides the ability to extend ASP.NET Scaffolding to meet the requirements of your scenario.</span></span> <span data-ttu-id="74b6c-114">Bu işlevsellik ile özelleştirilmiş yapı iskelesi şablonu oluşturmak ve yeni İskele Ekle iletişim kutusuna ekleyin.</span><span class="sxs-lookup"><span data-stu-id="74b6c-114">With this functionality, you can create a customized scaffolding template and add it to Add New Scaffold dialog.</span></span> <span data-ttu-id="74b6c-115">Özelleştirilmiş şablon içerisinde, iskele kurulmuş öğe ekleme sırasında oluşturulan kodu belirtin.</span><span class="sxs-lookup"><span data-stu-id="74b6c-115">Within the customized template, you specify the code that is generated when adding a scaffolded item.</span></span> <span data-ttu-id="74b6c-116">Daha fazla bilgi için [için Visual Studio bir özel destek oluşturma](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="74b6c-116">For more information, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74b6c-117">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="74b6c-117">Prerequisites</span></span>

<span data-ttu-id="74b6c-118">ASP.NET iskeleti oluşturma kullanmak için şunlara sahip olmalısınız:</span><span class="sxs-lookup"><span data-stu-id="74b6c-118">To use ASP.NET Scaffolding, you must have:</span></span>

- <span data-ttu-id="74b6c-119">Microsoft Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="74b6c-119">Microsoft Visual Studio 2013</span></span>
- <span data-ttu-id="74b6c-120">Web geliştirici araçları (varsayılan Visual Studio 2013 yüklemesinin bir parçası)</span><span class="sxs-lookup"><span data-stu-id="74b6c-120">Web Developer Tools (part of default Visual Studio 2013 installation)</span></span>
- <span data-ttu-id="74b6c-121">ASP.NET Web çerçeveleri ve araçları 2013'ü (varsayılan Visual Studio 2013 yüklemesinin bir parçası)</span><span class="sxs-lookup"><span data-stu-id="74b6c-121">ASP.NET Web Frameworks and Tools 2013 (part of default Visual Studio 2013 installation)</span></span>

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a><span data-ttu-id="74b6c-122">MVC veya Web API'si için iskele kurulmuş Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="74b6c-122">Add a scaffolded item to MVC or Web API</span></span>

<span data-ttu-id="74b6c-123">İskele eklemek, proje veya proje içinde bir klasöre sağ tıklayın ve seçin **Ekle** – **yeni iskele kurulmuş öğe**, aşağıdaki görüntüde gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="74b6c-123">To add a scaffold, right-click either the project or a folder within the project, and select **Add** – **New Scaffolded Item**, as shown in the following image.</span></span>

![İskele öğesi Ekle](aspnet-scaffolding-overview/_static/image1.png)

<span data-ttu-id="74b6c-125">Gelen **İskele Ekle** penceresinde eklemek için iskele türünü seçin.</span><span class="sxs-lookup"><span data-stu-id="74b6c-125">From the **Add Scaffold** window, select the type of scaffold to add.</span></span>

![İskele türünü seçin](aspnet-scaffolding-overview/_static/image2.png)

<span data-ttu-id="74b6c-127">**Denetleyici Ekle** penceresi denetleyici, Entity Framework 6'dan yeni zaman uyumsuz özelliklerinin isteyip istemediğinizi dahil oluşturma seçeneklerini belirlemek için bir fırsat sağlar.</span><span class="sxs-lookup"><span data-stu-id="74b6c-127">The **Add Controller** window gives you the opportunity to select options for generating the controller, including whether you want to use the new async features from Entity Framework 6.</span></span>

![Denetleyici ekleme](aspnet-scaffolding-overview/_static/image3.png)

<span data-ttu-id="74b6c-129">İlgili sınıflar ve sayfaları senaryonuz için oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="74b6c-129">The relevant classes and pages are created for your scenario.</span></span> <span data-ttu-id="74b6c-130">Örneğin, aşağıdaki görüntüde ve MVC denetleyici filmler adlı bir model sınıfı için yapı iskelesi ile oluşturulan görünümleri gösterir.</span><span class="sxs-lookup"><span data-stu-id="74b6c-130">For example, the following image shows the MVC controller and views that were created through scaffolding for a model class named Movies.</span></span>

![Oluşturulan dosyalar](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a><span data-ttu-id="74b6c-132">Web formları için iskele kurulmuş Öğe Ekle</span><span class="sxs-lookup"><span data-stu-id="74b6c-132">Add a scaffolded item to Web Forms</span></span>

<span data-ttu-id="74b6c-133">Web Forms kodunu üretir yapı iskelesi eklemek için Visual Studio için bir uzantı yüklemeniz veya MVC bağımlılıkları ekleyin.</span><span class="sxs-lookup"><span data-stu-id="74b6c-133">To add scaffolding that generates Web Forms code, you must either install an extension to Visual Studio or add MVC dependencies.</span></span> <span data-ttu-id="74b6c-134">Her iki yaklaşım aşağıda gösterilmektedir, ancak sadece Bu yaklaşımlardan birini yapmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="74b6c-134">Both approaches are shown below, but you only need to do one of these approaches.</span></span>

### <a name="web-forms-scaffolding-extension"></a><span data-ttu-id="74b6c-135">Web Forms uzantısı iskele kurma özelliği</span><span class="sxs-lookup"><span data-stu-id="74b6c-135">Web Forms Scaffolding Extension</span></span>

<span data-ttu-id="74b6c-136">Yapı iskelesi ile bir Web formları projesi kullanmanıza olanak sağlayan Visual Studio uzantısı yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="74b6c-136">You can install a Visual Studio extension that enable you to use scaffolding with a Web Forms project.</span></span> <span data-ttu-id="74b6c-137">Visual Studio'da **Araçları** ardından **Uzantılar ve güncelleştirmeler**.</span><span class="sxs-lookup"><span data-stu-id="74b6c-137">In Visual Studio, select **Tools** and then **Extensions and Updates**.</span></span> <span data-ttu-id="74b6c-138">Visual Studio Galerisi için bu iletişim kutusundan arama **Web Forms yapı İskelesi**.</span><span class="sxs-lookup"><span data-stu-id="74b6c-138">From this dialog search the Visual Studio Gallery for **Web Forms Scaffolding**.</span></span>

![web forms iskele kurma özelliği yükleme](aspnet-scaffolding-overview/_static/image5.png)

<span data-ttu-id="74b6c-140">Daha fazla bilgi için [Web Forms yapı İskelesi](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span><span class="sxs-lookup"><span data-stu-id="74b6c-140">For more information, see [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).</span></span>

### <a name="mvc-dependencies"></a><span data-ttu-id="74b6c-141">MVC bağımlılıkları</span><span class="sxs-lookup"><span data-stu-id="74b6c-141">MVC Dependencies</span></span>

<span data-ttu-id="74b6c-142">MVC bağımlılıkları eklemek için seçin **Ekle** - **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="74b6c-142">To add MVC dependencies, select **Add** - **New Scaffolded Item**.</span></span> <span data-ttu-id="74b6c-143">İskele Ekle penceresinde **MVC bağımlılıkları**, aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="74b6c-143">In the Add Scaffold window, select **MVC Dependencies**, as shown below.</span></span>

![MVC bağımlılıkları Ekle](aspnet-scaffolding-overview/_static/image6.png)

<span data-ttu-id="74b6c-145">MVC yapı iskelesini oluşturmak için iki seçenek vardır; Minimal ve tam.</span><span class="sxs-lookup"><span data-stu-id="74b6c-145">There are two options for scaffolding MVC; Minimal and Full.</span></span> <span data-ttu-id="74b6c-146">En az seçerseniz, yalnızca NuGet paketleri ve ASP.NET MVC için başvuruları projenize eklenir.</span><span class="sxs-lookup"><span data-stu-id="74b6c-146">If you select Minimal, only the NuGet packages and references for ASP.NET MVC are added to your project.</span></span> <span data-ttu-id="74b6c-147">Tam seçeneği seçerseniz bir MVC projesi için gerekli içerik dosyalarının yanı sıra en az bağımlılıklar eklenir.</span><span class="sxs-lookup"><span data-stu-id="74b6c-147">If you select the Full option, the Minimal dependencies are added, as well as the required content files for an MVC project.</span></span> <span data-ttu-id="74b6c-148">Yapı iskelesi kolayca kullanmak için tam bağımlılıkları seçin.</span><span class="sxs-lookup"><span data-stu-id="74b6c-148">To easily use scaffolding, select Full dependencies.</span></span>

![Tam bağımlılıkları seçin](aspnet-scaffolding-overview/_static/image7.png)

<span data-ttu-id="74b6c-150">Göreceğiniz bağımlılıkları ekledikten sonra bir **readme.txt** dosya.</span><span class="sxs-lookup"><span data-stu-id="74b6c-150">After adding the dependencies, you will see a **readme.txt** file.</span></span> <span data-ttu-id="74b6c-151">Dikkatli bir şekilde, projenizin doğru şekilde çalıştığından emin olmak için bu dosyasındaki yönergeleri izleyin.</span><span class="sxs-lookup"><span data-stu-id="74b6c-151">Carefully follow the instructions in this file to ensure that your project works correctly.</span></span>

<span data-ttu-id="74b6c-152">Readme.txt dosyasını adımları tamamladıktan sonra MVC ve Web API'si hakkında önceki bölümde gösterildiği gibi yeni iskele kurulmuş öğe ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="74b6c-152">When you have completed the steps in the readme.txt file, you can add a new scaffolded item as shown in the previous section about MVC and Web API.</span></span> <span data-ttu-id="74b6c-153">Denetleyici ve otomatik olarak oluşturulan görünümler içinde proje düzgün çalışmayacaktır.</span><span class="sxs-lookup"><span data-stu-id="74b6c-153">The automatically-generated views and controller will function correctly within your project.</span></span>

## <a name="tutorials"></a><span data-ttu-id="74b6c-154">Öğreticiler</span><span class="sxs-lookup"><span data-stu-id="74b6c-154">Tutorials</span></span>

<span data-ttu-id="74b6c-155">Özelleştirilmiş bir iskele kurucu oluşturmak için bkz [için Visual Studio bir özel destek oluşturma](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span><span class="sxs-lookup"><span data-stu-id="74b6c-155">To create a customized scaffolder, see [Creating a Custom Scaffolder for Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).</span></span>

<span data-ttu-id="74b6c-156">Oluşturulan dosyalar özelleştirmek için bkz: [yeni iskele kurulmuş öğe iletişim oluşturulan dosyalardan özelleştirme](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span><span class="sxs-lookup"><span data-stu-id="74b6c-156">To customize the generated files, see [How to customize the generated files from the New Scaffolded Item dialog](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).</span></span>

<span data-ttu-id="74b6c-157">Yapı iskelesi ile kullanma örneği için **veritabanı ilk geliştirme**, bkz: [ASP.NET MVC ile EF veritabanı ilk](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="74b6c-157">For an example of using scaffolding with **Database First development**, see [EF Database First with ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).</span></span>

<span data-ttu-id="74b6c-158">Yapı iskelesi içinde kullanma örneği için bir **MVC** projesine [ASP.NET MVC 5 ile çalışmaya başlama](../../../mvc/overview/getting-started/introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="74b6c-158">For an example of using scaffolding in an **MVC** project, see [Getting Started with ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).</span></span>

<span data-ttu-id="74b6c-159">Yapı iskelesi içinde kullanma örneği için bir **Web API** projesine [Web API 2'de öznitelik yönlendirme ile REST API oluşturma](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="74b6c-159">For an example of using scaffolding in a **Web API** project, see [Create a REST API with Attribute Routing in Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).</span></span>
