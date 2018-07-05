---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: ASP.NET Web API Yardım sayfaları oluşturma | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8803c406660398ad3314306a1bcdf99af418082d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393147"
---
<a name="creating-help-pages-for-aspnet-web-api"></a><span data-ttu-id="9db37-102">ASP.NET Web API Yardım sayfaları oluşturma</span><span class="sxs-lookup"><span data-stu-id="9db37-102">Creating Help Pages for ASP.NET Web API</span></span>
====================
<span data-ttu-id="9db37-103">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9db37-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9db37-104">Web API'si oluşturma, böylece diğer geliştiriciler, API'nin nasıl çağrılacağını bilir, genellikle bir Yardım sayfasını oluşturmak kullanışlıdır.</span><span class="sxs-lookup"><span data-stu-id="9db37-104">When you create a web API, it is often useful to create a help page, so that other developers will know how to call your API.</span></span> <span data-ttu-id="9db37-105">Tüm belgelerini el ile oluşturabilirsiniz, ancak mümkün olduğunca otomatik iyidir.</span><span class="sxs-lookup"><span data-stu-id="9db37-105">You could create all of the documentation manually, but it is better to autogenerate as much as possible.</span></span>

<span data-ttu-id="9db37-106">Bu görevi kolaylaştırmak için ASP.NET Web API kitaplık çalışma zamanında otomatik olarak oluşturmak için Yardım sayfaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="9db37-106">To make this task easier, ASP.NET Web API provides a library for auto-generating help pages at run time.</span></span>

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a><span data-ttu-id="9db37-107">API Yardım sayfaları oluşturma</span><span class="sxs-lookup"><span data-stu-id="9db37-107">Creating API Help Pages</span></span>

<span data-ttu-id="9db37-108">Yükleme [ASP.NET ve Web Araçları 2012.2 güncelleştirme](https://go.microsoft.com/fwlink/?LinkId=282650).</span><span class="sxs-lookup"><span data-stu-id="9db37-108">Install [ASP.NET and Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650).</span></span> <span data-ttu-id="9db37-109">Bu güncelleştirme ile Web API proje şablonunu yardım sayfalarına tümleştirir.</span><span class="sxs-lookup"><span data-stu-id="9db37-109">This update integrates help pages into the Web API project template.</span></span>

<span data-ttu-id="9db37-110">Ardından, yeni bir ASP.NET MVC 4 projesi oluşturun ve Web API proje şablonunu seçin.</span><span class="sxs-lookup"><span data-stu-id="9db37-110">Next, create a new ASP.NET MVC 4 project and select the Web API project template.</span></span> <span data-ttu-id="9db37-111">Proje şablonu adlı bir örnek API denetleyicisi oluşturur `ValuesController`.</span><span class="sxs-lookup"><span data-stu-id="9db37-111">The project template creates an example API controller named `ValuesController`.</span></span> <span data-ttu-id="9db37-112">Şablon API Yardım sayfaları da oluşturur.</span><span class="sxs-lookup"><span data-stu-id="9db37-112">The template also creates the API help pages.</span></span> <span data-ttu-id="9db37-113">Tüm kod dosyaları Yardım sayfası proje alanlarını klasöründe yerleştirilir.</span><span class="sxs-lookup"><span data-stu-id="9db37-113">All of the code files for the help page are placed in the Areas folder of the project.</span></span>

![](creating-api-help-pages/_static/image2.png)

<span data-ttu-id="9db37-114">Uygulamayı çalıştırdığınızda giriş sayfasının API Yardım sayfası için bir bağlantı içerir.</span><span class="sxs-lookup"><span data-stu-id="9db37-114">When you run the application, the home page contains a link to the API help page.</span></span> <span data-ttu-id="9db37-115">Giriş sayfasından, göreli yol/Help ' dir.</span><span class="sxs-lookup"><span data-stu-id="9db37-115">From the home page, the relative path is /Help.</span></span>

![](creating-api-help-pages/_static/image3.png)

<span data-ttu-id="9db37-116">Bu bağlantı için bir API Özet sayfasında getirir.</span><span class="sxs-lookup"><span data-stu-id="9db37-116">This link brings you to an API summary page.</span></span>

![](creating-api-help-pages/_static/image4.png)

<span data-ttu-id="9db37-117">Bu sayfa için MVC görünümü Areas/HelpPage/Views/Help/Index.cshtml içinde tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="9db37-117">The MVC view for this page is defined in Areas/HelpPage/Views/Help/Index.cshtml.</span></span> <span data-ttu-id="9db37-118">Düzen, giriş, başlık, stillerini ve benzeri değiştirmek için bu sayfayı düzenleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9db37-118">You can edit this page to modify the layout, introduction, title, styles, and so forth.</span></span>

<span data-ttu-id="9db37-119">Ana sayfanın parçası, API, denetleyici tarafından gruplandırılmış bir tablodur.</span><span class="sxs-lookup"><span data-stu-id="9db37-119">The main part of the page is a table of APIs, grouped by controller.</span></span> <span data-ttu-id="9db37-120">Tablo girişleri kullanarak dinamik olarak oluşturulan **IApiExplorer** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="9db37-120">The table entries are generated dynamically, using the **IApiExplorer** interface.</span></span> <span data-ttu-id="9db37-121">(Ben daha sonra bu arabirimi hakkında daha fazla konuşacağız.) Yeni bir API denetleyicisi eklerseniz, tablo, çalışma zamanında otomatik olarak güncelleştirilir.</span><span class="sxs-lookup"><span data-stu-id="9db37-121">(I'll talk more about this interface later.) If you add a new API controller, the table is automatically updated at run time.</span></span>

<span data-ttu-id="9db37-122">Göreli URI ve HTTP yöntemi "API" sütunu listeler.</span><span class="sxs-lookup"><span data-stu-id="9db37-122">The "API" column lists the HTTP method and relative URI.</span></span> <span data-ttu-id="9db37-123">"Description" sütunu, her API belgelerini içerir.</span><span class="sxs-lookup"><span data-stu-id="9db37-123">The "Description" column contains documentation for each API.</span></span> <span data-ttu-id="9db37-124">Başlangıçta, yer tutucu metnini belgelerdir.</span><span class="sxs-lookup"><span data-stu-id="9db37-124">Initially, the documentation is just placeholder text.</span></span> <span data-ttu-id="9db37-125">Sonraki bölümde, ı, belgeleri XML açıklamalarını ekleme göstereceğiz.</span><span class="sxs-lookup"><span data-stu-id="9db37-125">In the next section, I'll show you how to add documentation from XML comments.</span></span>

<span data-ttu-id="9db37-126">Her API, örnek istek ve yanıt gövdeleri dahil olmak üzere daha ayrıntılı bilgi içeren bir sayfa için bir bağlantı içerir.</span><span class="sxs-lookup"><span data-stu-id="9db37-126">Each API has a link to a page with more detailed information, including example request and response bodies.</span></span>

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a><span data-ttu-id="9db37-127">Mevcut bir projeyi yardım sayfalarına ekleme</span><span class="sxs-lookup"><span data-stu-id="9db37-127">Adding Help Pages to an Existing Project</span></span>

<span data-ttu-id="9db37-128">NuGet Paket Yöneticisi'ni kullanarak yardım sayfalarına varolan bir Web API projesine ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9db37-128">You can add help pages to an existing Web API project by using NuGet Package Manager.</span></span> <span data-ttu-id="9db37-129">Bu seçenek, bir "Web API" şablonu değerinden farklı proje şablonundan başlattığınız yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="9db37-129">This option is useful you start from a different project template than the "Web API" template.</span></span>

<span data-ttu-id="9db37-130">Gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**ve ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="9db37-130">From the **Tools** menu, select **Library Package Manager**, and then select **Package Manager Console**.</span></span> <span data-ttu-id="9db37-131">İçinde [Paket Yöneticisi Konsolu](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) penceresinde, aşağıdaki komutlardan birini yazın:</span><span class="sxs-lookup"><span data-stu-id="9db37-131">In the [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) window, type one of the following commands:</span></span>

<span data-ttu-id="9db37-132">İçin bir **C#** uygulama: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span><span class="sxs-lookup"><span data-stu-id="9db37-132">For a **C#** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage`</span></span>

<span data-ttu-id="9db37-133">İçin bir **Visual Basic** uygulama: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span><span class="sxs-lookup"><span data-stu-id="9db37-133">For a **Visual Basic** application: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`</span></span>

<span data-ttu-id="9db37-134">İki paket, bir C# ve Visual Basic için bir tane vardır.</span><span class="sxs-lookup"><span data-stu-id="9db37-134">There are two packages, one for C# and one for Visual Basic.</span></span> <span data-ttu-id="9db37-135">Projenizi eşleşen bir kullandığınızdan emin olun.</span><span class="sxs-lookup"><span data-stu-id="9db37-135">Make sure to use the one that matches your project.</span></span>

<span data-ttu-id="9db37-136">Bu komut, gerekli bütünleştirilmiş kodları yükler ve MVC görünümleri (alanlar/HelpPage klasöründe bulunur) Yardım sayfaları için ekler.</span><span class="sxs-lookup"><span data-stu-id="9db37-136">This command installs the necessary assemblies and adds the MVC views for the help pages (located in the Areas/HelpPage folder).</span></span> <span data-ttu-id="9db37-137">Yardım sayfasına bir bağlantıyı el ile eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="9db37-137">You'll need to manually add a link to the Help page.</span></span> <span data-ttu-id="9db37-138">/ Help URI'dir.</span><span class="sxs-lookup"><span data-stu-id="9db37-138">The URI is /Help.</span></span> <span data-ttu-id="9db37-139">Razor Görünümü'nde bir bağlantı oluşturmak için aşağıdakileri ekleyin:</span><span class="sxs-lookup"><span data-stu-id="9db37-139">To create a link in a razor view, add the following:</span></span>

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

<span data-ttu-id="9db37-140">Ayrıca, alanları kaydedilecek emin olun.</span><span class="sxs-lookup"><span data-stu-id="9db37-140">Also, make sure to register areas.</span></span> <span data-ttu-id="9db37-141">Global.asax dosyasında aşağıdaki kodu ekleyin **uygulama\_Başlat** orada değilse yöntemi:</span><span class="sxs-lookup"><span data-stu-id="9db37-141">In the Global.asax file, add the following code to the **Application\_Start** method, if it is not there already:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a><span data-ttu-id="9db37-142">API belgelerine ekleme</span><span class="sxs-lookup"><span data-stu-id="9db37-142">Adding API Documentation</span></span>

<span data-ttu-id="9db37-143">Varsayılan olarak, Yardım sayfaları, belgeler için yer tutucu dizeleri vardır.</span><span class="sxs-lookup"><span data-stu-id="9db37-143">By default, the help pages have placeholder strings for documentation.</span></span> <span data-ttu-id="9db37-144">Kullanabileceğiniz [XML belgeleri yorumları](https://msdn.microsoft.com/library/b2s063f7.aspx) belgeleri oluşturmak için.</span><span class="sxs-lookup"><span data-stu-id="9db37-144">You can use [XML documentation comments](https://msdn.microsoft.com/library/b2s063f7.aspx) to create the documentation.</span></span> <span data-ttu-id="9db37-145">Bu özelliği etkinleştirmek için ' % s'dosyasını HelpPage/alanlar/uygulama açın\_Start/HelpPageConfig.cs ve aşağıdaki satırı açıklamadan çıkarın:</span><span class="sxs-lookup"><span data-stu-id="9db37-145">To enable this feature, open the file Areas/HelpPage/App\_Start/HelpPageConfig.cs and uncomment the following line:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

<span data-ttu-id="9db37-146">XML belgeleri artık etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="9db37-146">Now enable XML documentation.</span></span> <span data-ttu-id="9db37-147">Çözüm Gezgini'nde projeye sağ tıklayıp seçin **özellikleri**.</span><span class="sxs-lookup"><span data-stu-id="9db37-147">In Solution Explorer, right-click the project and select **Properties**.</span></span> <span data-ttu-id="9db37-148">Seçin **derleme** sayfası.</span><span class="sxs-lookup"><span data-stu-id="9db37-148">Select the **Build** page.</span></span>

![](creating-api-help-pages/_static/image6.png)

<span data-ttu-id="9db37-149">Altında **çıkış**, kontrol **XML belge dosyası**.</span><span class="sxs-lookup"><span data-stu-id="9db37-149">Under **Output**, check **XML documentation file**.</span></span> <span data-ttu-id="9db37-150">Düzenleme kutusuna "uygulama\_Data/XmlDocument.xml".</span><span class="sxs-lookup"><span data-stu-id="9db37-150">In the edit box, type "App\_Data/XmlDocument.xml".</span></span>

![](creating-api-help-pages/_static/image7.png)

<span data-ttu-id="9db37-151">Ardından, kodunu açmak `ValuesController` /Controllers/ValuesControler.cs içinde tanımlanan API denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="9db37-151">Next, open the code for the `ValuesController` API controller, which is defined in /Controllers/ValuesControler.cs.</span></span> <span data-ttu-id="9db37-152">Bazı belge açıklamaları için denetleyici yöntemleri ekleyin.</span><span class="sxs-lookup"><span data-stu-id="9db37-152">Add some documentation comments to the controller methods.</span></span> <span data-ttu-id="9db37-153">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="9db37-153">For example:</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> <span data-ttu-id="9db37-154">İpucu: XML öğeleri, Visual Studio otomatik olarak giriş işaretini satırın yöntem yukarıda getirin ve üç eğik çizgi yazın, ekler.</span><span class="sxs-lookup"><span data-stu-id="9db37-154">Tip: If you position the caret on the line above the method and type three forward slashes, Visual Studio automatically inserts the XML elements.</span></span> <span data-ttu-id="9db37-155">Ardından, boşlukları doldurabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9db37-155">Then you can fill in the blanks.</span></span>


<span data-ttu-id="9db37-156">Artık derleme ve uygulamayı yeniden çalıştırın ve Yardım sayfalarına gidin.</span><span class="sxs-lookup"><span data-stu-id="9db37-156">Now build and run the application again, and navigate to the help pages.</span></span> <span data-ttu-id="9db37-157">Belge dizeleri API tabloda görüntülenmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="9db37-157">The documentation strings should appear in the API table.</span></span>

![](creating-api-help-pages/_static/image8.png)

<span data-ttu-id="9db37-158">Yardım sayfasına dizeleri çalışma zamanında XML dosyasından okur.</span><span class="sxs-lookup"><span data-stu-id="9db37-158">The help page reads the strings from the XML file at run time.</span></span> <span data-ttu-id="9db37-159">(Uygulama dağıttığınızda, XML dosyasını dağıtmak emin olun.)</span><span class="sxs-lookup"><span data-stu-id="9db37-159">(When you deploy the application, make sure to deploy the XML file.)</span></span>

## <a name="under-the-hood"></a><span data-ttu-id="9db37-160">Başlık altında</span><span class="sxs-lookup"><span data-stu-id="9db37-160">Under the Hood</span></span>

<span data-ttu-id="9db37-161">Üst kısmındaki yardım sayfalarına yerleşik **ApiExplorer** Web API çerçevesi parçası olan sınıf.</span><span class="sxs-lookup"><span data-stu-id="9db37-161">The help pages are built on top of the **ApiExplorer** class, which is part of the Web API framework.</span></span> <span data-ttu-id="9db37-162">**ApiExplorer** sınıfı, bir Yardım sayfasını oluşturmak için ham madde sağlar.</span><span class="sxs-lookup"><span data-stu-id="9db37-162">The **ApiExplorer** class provides the raw material for creating a help page.</span></span> <span data-ttu-id="9db37-163">Her API için **ApiExplorer** içeren bir **ApiDescription** API'sini açıklayan.</span><span class="sxs-lookup"><span data-stu-id="9db37-163">For each API, **ApiExplorer** contains an **ApiDescription** that describes the API.</span></span> <span data-ttu-id="9db37-164">Bu amaç için bir "API" HTTP yöntemi ile göreli URL birleşimi tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="9db37-164">For this purpose, an "API" is defined as the combination of HTTP method and relative URI.</span></span> <span data-ttu-id="9db37-165">Örneğin, bazı ayrı API şunlardır:</span><span class="sxs-lookup"><span data-stu-id="9db37-165">For example, here are some distinct APIs:</span></span>

- <span data-ttu-id="9db37-166">/Api/Products Al</span><span class="sxs-lookup"><span data-stu-id="9db37-166">GET /api/Products</span></span>
- <span data-ttu-id="9db37-167">Alma/API'si/ürünler / {id}</span><span class="sxs-lookup"><span data-stu-id="9db37-167">GET /api/Products/{id}</span></span>
- <span data-ttu-id="9db37-168">/ Api/ürünleri gönderin</span><span class="sxs-lookup"><span data-stu-id="9db37-168">POST /api/Products</span></span>

<span data-ttu-id="9db37-169">Bir denetleyici eylemi birden çok HTTP yöntemleri destekliyorsa **ApiExplorer** her yöntem farklı bir API olarak değerlendirir.</span><span class="sxs-lookup"><span data-stu-id="9db37-169">If a controller action supports multiple HTTP methods, the **ApiExplorer** treats each method as a distinct API.</span></span>

<span data-ttu-id="9db37-170">API'den gizlemek için **ApiExplorer**, ekleme **ApiExplorerSettings** öznitelik kümesi ve eylem *IgnoreApi* true.</span><span class="sxs-lookup"><span data-stu-id="9db37-170">To hide an API from the **ApiExplorer**, add the **ApiExplorerSettings** attribute to the action and set *IgnoreApi* to true.</span></span>

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

<span data-ttu-id="9db37-171">Ayrıca, bu özniteliği denetleyiciye tüm denetleyicinin hariç tutmak için ekleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="9db37-171">You can also add this attribute to the controller, to exclude the entire controller.</span></span>

<span data-ttu-id="9db37-172">Belgeleri dizelerden ApiExplorer sınıfı alır **IDocumentationProvider** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="9db37-172">The ApiExplorer class gets documentation strings from the **IDocumentationProvider** interface.</span></span> <span data-ttu-id="9db37-173">Daha önce bahsettiğim gibi Yardım sayfaları kitaplığı sağlayan bir **IDocumentationProvider** XML belgeleri dizelerden belgeleri alır.</span><span class="sxs-lookup"><span data-stu-id="9db37-173">As you saw earlier, the Help Pages library provides an **IDocumentationProvider** that gets documentation from XML documentation strings.</span></span> <span data-ttu-id="9db37-174">Kod içinde /Areas/HelpPage/XmlDocumentationProvider.cs bulunur.</span><span class="sxs-lookup"><span data-stu-id="9db37-174">The code is located in /Areas/HelpPage/XmlDocumentationProvider.cs.</span></span> <span data-ttu-id="9db37-175">Belge başka bir kaynaktan yazarak kendi alabileceğiniz **IDocumentationProvider**.</span><span class="sxs-lookup"><span data-stu-id="9db37-175">You can get documentation from another source by writing your own **IDocumentationProvider**.</span></span> <span data-ttu-id="9db37-176">Yukarı wire çağrısı **SetDocumentationProvider** genişletme yöntemi, tanımlanan **HelpPageConfigurationExtensions**</span><span class="sxs-lookup"><span data-stu-id="9db37-176">To wire it up, call the **SetDocumentationProvider** extension method, defined in **HelpPageConfigurationExtensions**</span></span>

<span data-ttu-id="9db37-177">**ApiExplorer** otomatik olarak içine yapılan çağrılar **IDocumentationProvider** her bir API için belgeler dizelerini almak için arabirim.</span><span class="sxs-lookup"><span data-stu-id="9db37-177">**ApiExplorer** automatically calls into the **IDocumentationProvider** interface to get documentation strings for each API.</span></span> <span data-ttu-id="9db37-178">Bu depolar **belgeleri** özelliği **ApiDescription** ve **ApiParameterDescription** nesneleri.</span><span class="sxs-lookup"><span data-stu-id="9db37-178">It stores them in the **Documentation** property of the **ApiDescription** and **ApiParameterDescription** objects.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9db37-179">Sonraki Adımlar</span><span class="sxs-lookup"><span data-stu-id="9db37-179">Next Steps</span></span>

<span data-ttu-id="9db37-180">Burada gösterilen yardım sayfalarına sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="9db37-180">You aren't limited to the help pages shown here.</span></span> <span data-ttu-id="9db37-181">Aslında, **ApiExplorer** Yardım sayfaları oluşturma için sınırlı değildir.</span><span class="sxs-lookup"><span data-stu-id="9db37-181">In fact, **ApiExplorer** is not limited to creating help pages.</span></span> <span data-ttu-id="9db37-182">Kullanıma hazır düşünmek başlamanızı sağlayacak bazı harika blog yazılarını yazılmış yao Huang Bağla:</span><span class="sxs-lookup"><span data-stu-id="9db37-182">Yao Huang Lin has written some great blog posts to get you thinking out of the box:</span></span>

- [<span data-ttu-id="9db37-183">ASP.NET Web API Yardım sayfası için basit bir Test istemcisi ekleme</span><span class="sxs-lookup"><span data-stu-id="9db37-183">Adding a simple Test Client to ASP.NET Web API Help Page</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [<span data-ttu-id="9db37-184">ASP.NET Web API Yardım şirket içinde barındırılan hizmetleri üzerinde çalışma sayfası yapma</span><span class="sxs-lookup"><span data-stu-id="9db37-184">Making ASP.NET Web API Help Page work on self-hosted services</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [<span data-ttu-id="9db37-185">Yardım sayfası (veya istemci) ASP.NET Web API'si için tasarım zamanı oluşturma</span><span class="sxs-lookup"><span data-stu-id="9db37-185">Design-time generation of help page (or client) for ASP.NET Web API</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [<span data-ttu-id="9db37-186">Gelişmiş Yardım sayfası özelleştirmeleri</span><span class="sxs-lookup"><span data-stu-id="9db37-186">Advanced Help Page customizations</span></span>](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
