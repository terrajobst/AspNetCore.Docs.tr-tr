---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: "ASP.NET Web API Yardım sayfası oluşturma | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 18d04492529e96b6c0e14f1d7a30378b4832f4c8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="a22c9-102">ASP.NET Web API Yardım sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="a22c9-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="a22c9-103">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a22c9-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="a22c9-104">Web API'si oluşturduğunuzda, diğer geliştiriciler API'nizi nasıl bilmesini genellikle bir Yardım sayfası oluşturmak yararlı olacaktır.</span><span class="sxs-lookup"><span data-stu-id="a22c9-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="a22c9-105">Tüm belgelerin el ile oluşturabilirsiniz, ancak otomatik olarak mümkün olduğu kadar iyidir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="a22c9-106">Bu görev kolaylaştırmak için ASP.NET Web API kitaplığını çalışma zamanında yardım sayfalarına otomatik oluşturmak için sağlar.</span><span class="sxs-lookup"><span data-stu-id="a22c9-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="a22c9-107">API Yardım sayfası oluşturma</span><span class="sxs-lookup"><span data-stu-id="a22c9-107">Creating API Help Pages</span></span>

<span data-ttu-id="a22c9-108">Yükleme [ASP.NET ve Web Araçları 2012.2 güncelleştirme](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="a22c9-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="a22c9-109">Bu güncelleştirme, Web API projesi şablonuna yardım sayfalarına tümleştirir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="a22c9-110">Ardından, yeni bir ASP.NET MVC 4 projesi oluşturun ve Web API projesi şablonu seçin.</span><span class="sxs-lookup"><span data-stu-id="a22c9-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="a22c9-111">Proje şablonu adlı bir örnek API denetleyicisi oluşturur `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="a22c9-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="a22c9-112">Şablon API yardım sayfalarına da oluşturur.</span><span class="sxs-lookup"><span data-stu-id="a22c9-112">The template also creates the API help pages.</span></span> <span data-ttu-id="a22c9-113">Tüm kod dosyaları Yardım sayfası, proje alanları klasöründe yer alır.</span><span class="sxs-lookup"><span data-stu-id="a22c9-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="a22c9-114">Uygulamayı çalıştırdığınızda, giriş sayfası API yardım sayfasına bir bağlantı içerir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="a22c9-115">Giriş sayfasından göreli yolu/Help ' dir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="a22c9-116">Bu bağlantıyı bir API Özet sayfasında getirir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="a22c9-117">Bu sayfa için MVC görünümü Areas/HelpPage/Views/Help/Index.cshtml tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="a22c9-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="a22c9-118">Düzen, giriş, başlık, stil ve benzeri değiştirmek için bu sayfayı düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a22c9-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="a22c9-119">Ana sayfa API'leri, denetleyici tarafından gruplandırılmış bir tablo parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="a22c9-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="a22c9-120">Tablo girişleri kullanarak dinamik olarak üretilen **IApiExplorer** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="a22c9-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="a22c9-121">(Ben daha sonra bu arabirimi hakkında daha fazla konuşun.) Yeni bir API denetleyicisi eklerseniz, tablonun çalışma zamanında otomatik olarak güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="a22c9-122">Göreli URI ve HTTP yöntemi "API" sütununda listelenir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="a22c9-123">Her API belgelerine "Açıklama" sütun içeriyor.</span><span class="sxs-lookup"><span data-stu-id="a22c9-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="a22c9-124">Başlangıçta, yalnızca yer tutucu metin belgesidir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="a22c9-125">Sonraki bölümde, t, XML açıklamalardan belgelerine ekleme göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="a22c9-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="a22c9-126">Her API örnek istek ve yanıt gövdesi dahil olmak üzere daha ayrıntılı bilgi içeren bir sayfa için bir bağlantı vardır.</span><span class="sxs-lookup"><span data-stu-id="a22c9-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="a22c9-127">Varolan bir projeye yardım sayfalarına ekleme</span><span class="sxs-lookup"><span data-stu-id="a22c9-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="a22c9-128">NuGet Paket Yöneticisi'ni kullanarak, varolan bir Web API projesine yardım sayfalarına ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a22c9-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="a22c9-129">Bu seçenek, "Web API'sini" şablonundan farklı proje şablonu başlangıç yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="a22c9-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="a22c9-130">Gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="a22c9-130">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="a22c9-131">İçinde [Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) penceresinde, aşağıdaki komutlardan birini yazın:</span><span class="sxs-lookup"><span data-stu-id="a22c9-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="a22c9-132">İçin bir **C#** uygulama:`Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="a22c9-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="a22c9-133">İçin bir **Visual Basic** uygulama:`Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="a22c9-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="a22c9-134">İki paketler, bir C# ve Visual Basic için bir vardır.</span><span class="sxs-lookup"><span data-stu-id="a22c9-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="a22c9-135">Projenizi eşleşen bir kullandığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="a22c9-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="a22c9-136">Bu komut gerekli derlemeleri yükler ve (alanları/HelpPage klasöründe bulunur) Yardım sayfalarına için MVC görünümleri ekler.</span><span class="sxs-lookup"><span data-stu-id="a22c9-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="a22c9-137">Yardım sayfasına bir bağlantıyı el ile eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="a22c9-138">/ Help URI'dir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-138">The URI is /Help.</span></span> <span data-ttu-id="a22c9-139">Bir razor görünümünde bir bağlantı oluşturmak için aşağıdakileri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="a22c9-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="a22c9-140">Ayrıca, alanları kaydetmek emin olun.</span><span class="sxs-lookup"><span data-stu-id="a22c9-140">Also, make sure to register areas.</span></span> <span data-ttu-id="a22c9-141">Global.asax dosyasına aşağıdaki kodu ekleyin **uygulama\_Başlat** orada değilse yöntemi:</span><span class="sxs-lookup"><span data-stu-id="a22c9-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="a22c9-142">API belgelerine ekleme</span><span class="sxs-lookup"><span data-stu-id="a22c9-142">Adding API Documentation</span></span>

<span data-ttu-id="a22c9-143">Varsayılan olarak, Yardım sayfaları sahip belgeleri için yer tutucu dize.</span><span class="sxs-lookup"><span data-stu-id="a22c9-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="a22c9-144">Kullanabileceğiniz [XML belgeleri yorumları](https://msdn.microsoft.com/en-us/library/b2s063f7.aspx) belgeleri oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="a22c9-144">You can use [XML documentation comments](https://msdn.microsoft.com/en-us/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="a22c9-145">Bu özelliği etkinleştirmek için HelpPage/alanları/uygulama dosyasını açın\_Start/HelpPageConfig.cs ve aşağıdaki satırı açıklamadan çıkarın:</span><span class="sxs-lookup"><span data-stu-id="a22c9-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="a22c9-146">XML belgeleri şimdi etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="a22c9-146">Now enable XML documentation.</span></span> <span data-ttu-id="a22c9-147">Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="a22c9-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="a22c9-148">Seçin **yapı** sayfası.</span><span class="sxs-lookup"><span data-stu-id="a22c9-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="a22c9-149">Altında **çıkış**, denetleme **XML belge dosyası**.</span><span class="sxs-lookup"><span data-stu-id="a22c9-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="a22c9-150">Düzenleme kutusuna "uygulama\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="a22c9-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="a22c9-151">Ardından, kodunu açmak `ValuesController` /Controllers/ValuesControler.cs içinde tanımlanan API denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="a22c9-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="a22c9-152">Bazı belge açıklamaları için denetleyici yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="a22c9-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="a22c9-153">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="a22c9-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="a22c9-154">İpucu: XML öğelerini, Visual Studio otomatik olarak düzeltme işareti yöntemi üstündeki satırın getirin ve üç eğik yazarsanız, ekler.</span><span class="sxs-lookup"><span data-stu-id="a22c9-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="a22c9-155">Ardından boşlukları doldurabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a22c9-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="a22c9-156">Şimdi oluşturmak ve uygulamayı yeniden çalıştırın ve Yardım sayfalarına gidin.</span><span class="sxs-lookup"><span data-stu-id="a22c9-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="a22c9-157">Belge dizeleri API tablosunda görünmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="a22c9-158">Yardım sayfası dizeleri çalışma zamanında XML dosyasından okur.</span><span class="sxs-lookup"><span data-stu-id="a22c9-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="a22c9-159">(Uygulama dağıttığınızda, XML dosyasını dağıtmak emin olun.)</span><span class="sxs-lookup"><span data-stu-id="a22c9-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="a22c9-160">Başlık altında</span><span class="sxs-lookup"><span data-stu-id="a22c9-160">Under the Hood</span></span>

<span data-ttu-id="a22c9-161">Yardım sayfalarına üstünde oluşturulan **ApiExplorer** Web API çerçevesi parçası olan sınıf.</span><span class="sxs-lookup"><span data-stu-id="a22c9-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="a22c9-162">**ApiExplorer** sınıfı bir Yardım sayfasını oluşturmak için ham madde sağlar.</span><span class="sxs-lookup"><span data-stu-id="a22c9-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="a22c9-163">Her API için **ApiExplorer** içeren bir **ApiDescription** API açıklar.</span><span class="sxs-lookup"><span data-stu-id="a22c9-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="a22c9-164">Bu amaç için bir "API" HTTP yöntemi ile göreli URL birleşimi tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="a22c9-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="a22c9-165">Örneğin, bazı farklı API'leri şunlardır:</span><span class="sxs-lookup"><span data-stu-id="a22c9-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="a22c9-166">/Api/Products Al</span><span class="sxs-lookup"><span data-stu-id="a22c9-166">GET /api/Products</span></span>
- <span data-ttu-id="a22c9-167">/Api/ürünler / {id} Al</span><span class="sxs-lookup"><span data-stu-id="a22c9-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="a22c9-168">/ Api/ürünleri sonrası</span><span class="sxs-lookup"><span data-stu-id="a22c9-168">POST /api/Products</span></span>

<span data-ttu-id="a22c9-169">Bir denetleyici eylemi birden çok HTTP yöntemleri destekliyorsa **ApiExplorer** her yöntem farklı bir API değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="a22c9-170">API'den gizlemek için **ApiExplorer**, ekleme **ApiExplorerSettings** öznitelik kümesi ve eylem *IgnoreApi* true.</span><span class="sxs-lookup"><span data-stu-id="a22c9-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="a22c9-171">Bu öznitelik tüm denetleyicinin dışlamak için denetleyiciye de ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="a22c9-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="a22c9-172">Belge dizelerden ApiExplorer sınıfı alır **IDocumentationProvider** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="a22c9-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="a22c9-173">Daha önce anlatıldığı gibi Yardımı sayfalarında kitaplığı sağlar bir **IDocumentationProvider** XML belgeleri dizelerden belgeleri alır.</span><span class="sxs-lookup"><span data-stu-id="a22c9-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="a22c9-174">Kod içinde /Areas/HelpPage/XmlDocumentationProvider.cs bulunur.</span><span class="sxs-lookup"><span data-stu-id="a22c9-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="a22c9-175">Belge başka bir kaynaktan yazarak kendi alabileceğiniz **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="a22c9-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="a22c9-176">Yukarı wire çağrısı **SetDocumentationProvider** genişletme yöntemi, tanımlanan **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="a22c9-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="a22c9-177">**ApiExplorer** içine otomatik olarak çağırır **IDocumentationProvider** için her API belgelerine dizeleri almak için arabirim.</span><span class="sxs-lookup"><span data-stu-id="a22c9-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="a22c9-178">Bunları depolar **belgelerine** özelliği **ApiDescription** ve **ApiParameterDescription** nesneleri.</span><span class="sxs-lookup"><span data-stu-id="a22c9-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a22c9-179">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="a22c9-179">Next Steps</span></span>

<span data-ttu-id="a22c9-180">Burada gösterilen yardım sayfalarına sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="a22c9-181">Aslında, **ApiExplorer** Yardım sayfaları oluşturma için sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="a22c9-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="a22c9-182">Kutudan çıktığında düşünüyorum sağlamak için bazı harika blog yazılarını yao Huang Bağla yazmıştır:</span><span class="sxs-lookup"><span data-stu-id="a22c9-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="a22c9-183">ASP.NET Web API Yardım sayfası için basit bir Test istemcisi ekleme</span><span class="sxs-lookup"><span data-stu-id="a22c9-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="a22c9-184">ASP.NET Web API Yardım kendini barındıran Services'de çalışma sayfası yapma</span><span class="sxs-lookup"><span data-stu-id="a22c9-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="a22c9-185">Tasarım zamanı nesil Yardım sayfası (veya istemcisi) için ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="a22c9-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="a22c9-186">Gelişmiş Yardım sayfası özelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="a22c9-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
