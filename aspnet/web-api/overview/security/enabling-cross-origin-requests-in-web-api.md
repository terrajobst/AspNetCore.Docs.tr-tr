---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: ASP.NET Web API 2'de kaynaklar arası istekleri etkinleştirme | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API'de çıkış noktaları arası kaynak paylaşımı (CORS) destekleyecek şekilde gösterilmektedir.
ms.author: aspnetcontent
ms.date: 07/15/2014
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7ac6158c2365aa324cefe97db044f568a1a43795
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805418"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="e5470-103">ASP.NET Web API 2'de kaynaklar arası istekleri etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="e5470-103">Enabling Cross-Origin Requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="e5470-104">tarafından [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e5470-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="e5470-105">Tarayıcı Güvenliği, bir web sayfası, başka bir etki alanına AJAX istekleri yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="e5470-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="e5470-106">Bu kısıtlama adlı *aynı çıkış noktası İlkesi*ve kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önler.</span><span class="sxs-lookup"><span data-stu-id="e5470-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="e5470-107">Ancak, bazen, web API'si çağırma diğer sitelere izin vermek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5470-107">However, sometimes you might want to let other sites call your web API.</span></span>
> 
> <span data-ttu-id="e5470-108">[Kaynağın kaynak paylaşımını çapraz](http://www.w3.org/TR/cors/) (CORS) olan gevşek bir aynı çıkış noktası ilkesi izin veren bir W3C standart.</span><span class="sxs-lookup"><span data-stu-id="e5470-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="e5470-109">CORS kullanarak, bir sunucu açıkça bazı çıkış noktaları arası istekleri izin verirken diğerlerini.</span><span class="sxs-lookup"><span data-stu-id="e5470-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="e5470-110">CORS güvenli ve önceki teknikler daha esnek gibi [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="e5470-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="e5470-111">Bu öğreticide, Web API uygulamanıza CORS'yi etkinleştirme işlemi gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e5470-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e5470-112">Bu öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="e5470-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="e5470-113">Visual Studio 2013 Güncelleştirme 2</span><span class="sxs-lookup"><span data-stu-id="e5470-113">Visual Studio 2013 Update 2</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="e5470-114">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="e5470-114">Web API 2.2</span></span>


<a id="intro"></a>
## <a name="introduction"></a><span data-ttu-id="e5470-115">Giriş</span><span class="sxs-lookup"><span data-stu-id="e5470-115">Introduction</span></span>

<span data-ttu-id="e5470-116">Bu öğreticide, ASP.NET Web API CORS desteği gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="e5470-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="e5470-117">– "Web API denetleyicisi barındıran, bir çağrılan Web hizmetini" ve "Web hizmetini çağıran diğer adlı WebClient", iki ASP.NET projesi oluşturarak başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="e5470-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="e5470-118">İki uygulama farklı etki alanlarında barındırıldığından, WebClient bir AJAX isteği WebService çıkış noktaları arası isteğidir.</span><span class="sxs-lookup"><span data-stu-id="e5470-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="e5470-119">"Aynı kaynak" nedir?</span><span class="sxs-lookup"><span data-stu-id="e5470-119">What is "Same Origin"?</span></span>

<span data-ttu-id="e5470-120">Aynı düzeni, konaklar ve bağlantı noktaları varsa iki URL aynı kaynağa sahip.</span><span class="sxs-lookup"><span data-stu-id="e5470-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="e5470-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="e5470-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="e5470-122">Bu iki URL aynı kaynağa sahip:</span><span class="sxs-lookup"><span data-stu-id="e5470-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="e5470-123">Bu URL'ler önceki değerinden farklı kaynakları iki vardır:</span><span class="sxs-lookup"><span data-stu-id="e5470-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="e5470-124">`http://example.net` -Farklı bir etki alanı</span><span class="sxs-lookup"><span data-stu-id="e5470-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="e5470-125">`http://example.com:9000/foo.html` -Farklı bir bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="e5470-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="e5470-126">`https://example.com/foo.html` -Farklı düzeni</span><span class="sxs-lookup"><span data-stu-id="e5470-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="e5470-127">`http://www.example.com/foo.html` -Farklı bir alt etki alanı</span><span class="sxs-lookup"><span data-stu-id="e5470-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="e5470-128">Internet Explorer bağlantı noktası kaynakları karşılaştırılırken göz önünde bulundurmaz.</span><span class="sxs-lookup"><span data-stu-id="e5470-128">Internet Explorer does not consider the port when comparing origins.</span></span>


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a><span data-ttu-id="e5470-129">Web hizmeti projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e5470-129">Create the WebService Project</span></span>

> [!NOTE]
> <span data-ttu-id="e5470-130">Bu bölümde, Web API projeleri oluşturma bildiğiniz varsayılır.</span><span class="sxs-lookup"><span data-stu-id="e5470-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="e5470-131">Aksi takdirde bkz [ASP.NET Web API'si ile çalışmaya başlama](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e5470-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>


<span data-ttu-id="e5470-132">Visual Studio'yu başlatın ve yeni bir **ASP.NET Web uygulaması** proje.</span><span class="sxs-lookup"><span data-stu-id="e5470-132">Start Visual Studio and create a new **ASP.NET Web Application** project.</span></span> <span data-ttu-id="e5470-133">Seçin **boş** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="e5470-133">Select the **Empty** project template.</span></span> <span data-ttu-id="e5470-134">"Klasör eklemek ve çekirdek başvuruları için" altında seçin **Web API** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="e5470-134">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="e5470-135">İsteğe bağlı olarak, uygulamayı Mircosoft Azure'a dağıtmak için "Bulutta Barındır" seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="e5470-135">Optionally, select the "Host in Cloud" option to deploy the app to Mircosoft Azure.</span></span> <span data-ttu-id="e5470-136">Microsoft'un sunduğu en fazla 10 Web sitesi için ücretsiz bir web barındırma bir [ücretsiz Azure deneme hesabı](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="e5470-136">Microsoft offers free web hosting for up to 10 websites in a [free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

<span data-ttu-id="e5470-137">Adlı bir Web API denetleyicisi ekleme `TestController` aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="e5470-137">Add a Web API controller named `TestController` with the following code:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

<span data-ttu-id="e5470-138">Uygulamayı yerel olarak çalıştırmak veya Azure'a dağıtın.</span><span class="sxs-lookup"><span data-stu-id="e5470-138">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="e5470-139">(Bu öğreticideki ekran görüntüleri için ben Azure App Service Web Apps'e dağıtılır.) Web API'si çalıştığını doğrulamak için gidin `http://hostname/api/test/`burada *hostname* uygulamanın dağıtıldığı etki alanıdır.</span><span class="sxs-lookup"><span data-stu-id="e5470-139">(For the screenshots in this tutorial, I deployed to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="e5470-140">Yanıt metni görmelisiniz &quot;Al: sınama iletisi&quot;.</span><span class="sxs-lookup"><span data-stu-id="e5470-140">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a><span data-ttu-id="e5470-141">WebClient projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="e5470-141">Create the WebClient Project</span></span>

<span data-ttu-id="e5470-142">Başka bir ASP.NET Web uygulaması projesi oluşturun ve seçin **MVC** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="e5470-142">Create another ASP.NET Web Application project and select the **MVC** project template.</span></span> <span data-ttu-id="e5470-143">İsteğe bağlı olarak **kimlik doğrulamayı Değiştir** > **kimlik doğrulaması yok**.</span><span class="sxs-lookup"><span data-stu-id="e5470-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="e5470-144">Bu öğretici için kimlik doğrulaması gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e5470-144">You don't need authentication for this tutorial.</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

<span data-ttu-id="e5470-145">Çözüm Gezgini'nde Views/Home/Index.cshtml dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="e5470-145">In Solution Explorer, open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="e5470-146">Bu dosyadaki kodu aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="e5470-146">Replace the code in this file with the following:</span></span>

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

<span data-ttu-id="e5470-147">İçin *serviceUrl* değişkeni, Web hizmeti uygulama URI'sini kullanın.</span><span class="sxs-lookup"><span data-stu-id="e5470-147">For the *serviceUrl* variable, use the URI of the WebService app.</span></span> <span data-ttu-id="e5470-148">Artık WebClient uygulamayı yerel olarak çalıştırmak veya başka bir Web sitesine yayımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5470-148">Now run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="e5470-149">"Try It" düğmesine tıklayarak listelenen HTTP yöntemini kullanarak Web hizmeti uygulaması için bir AJAX isteği gönderen açılan kutuda (GET, POST veya PUT).</span><span class="sxs-lookup"><span data-stu-id="e5470-149">Clicking the "Try It" button submits an AJAX request to the WebService app, using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="e5470-150">Bu, bize farklı çıkış noktaları arası istekleri incelemek olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="e5470-150">This lets us examine different cross-origin requests.</span></span> <span data-ttu-id="e5470-151">Şu anda, Web hizmeti uygulama CORS desteklemiyor böylece düğmesine tıklarsanız, bir hata alırsınız.</span><span class="sxs-lookup"><span data-stu-id="e5470-151">Right now, the WebService app does not support CORS, so if you click the button, you will get an error.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="e5470-152">Bir aracının HTTP trafiğini izleme hoşlanıyorsanız [Fiddler](http://www.telerik.com/fiddler), tarayıcı GET isteği gönderir ve isteğin başarılı ancak AJAX çağrısı bir hata döndürür görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="e5470-152">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you will see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="e5470-153">Aynı çıkış noktası İlkesi tarayıcıdan engellemez anlaşılması önemlidir *gönderme* istek.</span><span class="sxs-lookup"><span data-stu-id="e5470-153">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="e5470-154">Bunun yerine, uygulama görmesini engeller *yanıt*.</span><span class="sxs-lookup"><span data-stu-id="e5470-154">Instead, it prevents the application from seeing the *response*.</span></span>


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a><span data-ttu-id="e5470-155">CORS'yi etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="e5470-155">Enable CORS</span></span>

<span data-ttu-id="e5470-156">Şimdi github'dan WebService uygulamada CORS'yi etkinleştirme.</span><span class="sxs-lookup"><span data-stu-id="e5470-156">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="e5470-157">İlk olarak, CORS NuGet paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e5470-157">First, add the CORS NuGet package.</span></span> <span data-ttu-id="e5470-158">Visual Studio'da gelen **Araçları** menüsünde **kitaplık Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="e5470-158">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="e5470-159">Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu yazın:</span><span class="sxs-lookup"><span data-stu-id="e5470-159">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="e5470-160">Bu komut, en son paketini yükler ve çekirdek Web API kitaplıkları dahil olmak üzere tüm bağımlılıkları güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="e5470-160">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="e5470-161">Kullanıcı belirli bir sürümünü hedefleyecek şekilde - sürüm bayrağı.</span><span class="sxs-lookup"><span data-stu-id="e5470-161">User the -Version flag to target a specific version.</span></span> <span data-ttu-id="e5470-162">CORS paketi, Web API 2.0 veya sonraki sürümünü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e5470-162">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="e5470-163">Uygulama dosyasını açın\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="e5470-163">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="e5470-164">Aşağıdaki kodu ekleyin **WebApiConfig.Register** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e5470-164">Add the following code to the **WebApiConfig.Register** method.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="e5470-165">Ardından, ekleme **[EnableCors]** özniteliğini `TestController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="e5470-165">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="e5470-166">İçin *kaynakları* parametresi WebClient uygulamanın dağıtıldığı bir URI kullanın.</span><span class="sxs-lookup"><span data-stu-id="e5470-166">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="e5470-167">Bu çıkış noktaları arası istekleri WebClient tüm diğer etki alanları arası istekleri engelleyerek hala çalışırken sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5470-167">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="e5470-168">Daha sonra parametrelerini açıklayacağız **[EnableCors]** daha ayrıntılı bir şekilde.</span><span class="sxs-lookup"><span data-stu-id="e5470-168">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="e5470-169">Sonunda eğik çizgi içermez *kaynakları* URL'si.</span><span class="sxs-lookup"><span data-stu-id="e5470-169">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="e5470-170">Güncelleştirilmiş WebService uygulamayı yeniden dağıtın.</span><span class="sxs-lookup"><span data-stu-id="e5470-170">Redeploy the updated WebService application.</span></span> <span data-ttu-id="e5470-171">WebClient güncelleştirmek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e5470-171">You don't need to update WebClient.</span></span> <span data-ttu-id="e5470-172">WebClient AJAX isteği başarılı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e5470-172">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="e5470-173">GET, PUT ve POST yöntemleri tüm izin verilir.</span><span class="sxs-lookup"><span data-stu-id="e5470-173">The GET, PUT, and POST methods are all allowed.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a><span data-ttu-id="e5470-174">CORS nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="e5470-174">How CORS Works</span></span>

<span data-ttu-id="e5470-175">Bu bölümde, HTTP iletileri düzeyinde bir CORS isteğinde ne açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="e5470-175">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="e5470-176">Böylece yapılandırabileceğiniz CORS nasıl çalıştığını anlamak önemlidir **[EnableCors]** doğru öznitelik ve beklediğiniz gibi şeyler çalışmazsa sorun giderme.</span><span class="sxs-lookup"><span data-stu-id="e5470-176">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly, and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="e5470-177">CORS belirtimi çıkış noktaları arası istekleri etkinleştirme birkaç yeni HTTP üst bilgilerini ortaya çıkarır.</span><span class="sxs-lookup"><span data-stu-id="e5470-177">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="e5470-178">Bir tarayıcı CORS destekliyorsa, bu üstbilgileri çıkış noktaları arası istekleri için otomatik olarak ayarlar; JavaScript kodunuza özel herhangi bir şey yapmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e5470-178">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="e5470-179">Çıkış noktaları arası isteğinin bir örneği aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="e5470-179">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="e5470-180">"Kaynak" üst bilgisi istekte site etki alanı sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5470-180">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="e5470-181">Sunucu isteği izin veriyorsa, Access-Control-Allow-Origin üst bilgi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e5470-181">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="e5470-182">Bu üst bilgi değeri ile eşleşen kaynak üst bilgisi ya da joker karakter değeri "\*", yani her türlü kaynağa izin verilir.</span><span class="sxs-lookup"><span data-stu-id="e5470-182">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="e5470-183">Access-Control-Allow-Origin üst bilgi yanıtı içermez AJAX isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="e5470-183">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="e5470-184">Özellikle, tarayıcının isteği izin vermiyor.</span><span class="sxs-lookup"><span data-stu-id="e5470-184">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="e5470-185">Tarayıcı yanıt, sunucunun başarılı bir yanıt döndürürse bile, istemci uygulama tarafından kullanılabilir yapmaz.</span><span class="sxs-lookup"><span data-stu-id="e5470-185">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="e5470-186">**Denetim öncesi isteği**</span><span class="sxs-lookup"><span data-stu-id="e5470-186">**Preflight Requests**</span></span>

<span data-ttu-id="e5470-187">Bazı CORS istekleri için tarayıcı kaynak gerçek isteği göndermeden önce bir "Denetim öncesi isteği" adlı ek bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="e5470-187">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="e5470-188">Aşağıdaki koşullar doğruysa, tarayıcının denetim öncesi isteği atlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e5470-188">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="e5470-189">İstek yöntemini GET, HEAD veya sonrası, olan *ve*</span><span class="sxs-lookup"><span data-stu-id="e5470-189">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="e5470-190">Uygulama dışındaki içeriği kabul et, Accept-Language, dil, herhangi bir istek üst ayarlamaz Content-Type veya son-Event-ID *ve*</span><span class="sxs-lookup"><span data-stu-id="e5470-190">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="e5470-191">Content-Type üst bilgisi (varsa ayarlayın) aşağıdakilerden biridir:</span><span class="sxs-lookup"><span data-stu-id="e5470-191">The Content-Type header (if set) is one of the following:</span></span> 

    - <span data-ttu-id="e5470-192">Application/x-www-form-urlencoded işlemek</span><span class="sxs-lookup"><span data-stu-id="e5470-192">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="e5470-193">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="e5470-193">multipart/form-data</span></span>
    - <span data-ttu-id="e5470-194">metin/düz</span><span class="sxs-lookup"><span data-stu-id="e5470-194">text/plain</span></span>

<span data-ttu-id="e5470-195">Uygulama çağırarak ayarlar üst istek üst bilgileri hakkında kuralın uygulanacağı **setRequestHeader** üzerinde **XMLHttpRequest** nesne.</span><span class="sxs-lookup"><span data-stu-id="e5470-195">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="e5470-196">(Bu "Yazar istek üstbilgilerini" CORS belirtimi çağırır.) Kural üstbilgileri uygulanmaz *tarayıcı* kullanıcı aracısı, konak veya Content-Length gibi ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5470-196">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="e5470-197">Denetim öncesi isteğinin bir örneği aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="e5470-197">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="e5470-198">Uçuş öncesi isteğinin HTTP OPTIONS yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="e5470-198">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="e5470-199">Bu, iki özel üst bilgileri içerir:</span><span class="sxs-lookup"><span data-stu-id="e5470-199">It includes two special headers:</span></span>

- <span data-ttu-id="e5470-200">Access-Control-Request-Method: fiili istek için kullanılacak HTTP yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e5470-200">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="e5470-201">Access-Control-Request-Headers: İstek üst bilgilerinin bir listesi, *uygulama* gerçek istek üzerinde ayarlanan.</span><span class="sxs-lookup"><span data-stu-id="e5470-201">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="e5470-202">(Yeniden, bu tarayıcı ayarlar üst bilgileri içermez.)</span><span class="sxs-lookup"><span data-stu-id="e5470-202">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="e5470-203">Sunucunun isteği izin varsayılarak bir yanıt örneği, şu şekildedir:</span><span class="sxs-lookup"><span data-stu-id="e5470-203">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="e5470-204">Yanıt, izin verilen yöntemleri listeleyen bir erişim-denetim-Allow-Methods üst bilgisi ve isteğe bağlı olarak izin verilen üstbilgileri listeleyen bir Access-Control-izin ver-Headers üstbilgisi içeriyor.</span><span class="sxs-lookup"><span data-stu-id="e5470-204">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="e5470-205">Denetim öncesi isteği başarıyla sonuçlanırsa, tarayıcı daha önce açıklandığı gibi gerçek bir istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="e5470-205">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="e5470-206">Kapsam kuralları [EnableCors] için</span><span class="sxs-lookup"><span data-stu-id="e5470-206">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="e5470-207">Uygulamanızdaki her eylem, denetleyici başına veya genel olarak tüm Web APİ'si denetleyicilerinin için CORS etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="e5470-207">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="e5470-208">**Eylem başına**</span><span class="sxs-lookup"><span data-stu-id="e5470-208">**Per Action**</span></span>

<span data-ttu-id="e5470-209">Tek bir eylem için CORS etkinleştirmek için ayarlayın **[EnableCors]** özniteliği eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e5470-209">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="e5470-210">Aşağıdaki örnek için CORS `GetItem` yalnızca yöntemi.</span><span class="sxs-lookup"><span data-stu-id="e5470-210">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="e5470-211">**Denetleyici**</span><span class="sxs-lookup"><span data-stu-id="e5470-211">**Per Controller**</span></span>

<span data-ttu-id="e5470-212">Ayarlarsanız **[EnableCors]** denetleyici sınıfı üzerinde denetleyicinin tüm eylemleri için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e5470-212">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="e5470-213">Bir eylem için CORS devre dışı bırakmak için ekleme **[DisableCors]** eyleme özniteliği.</span><span class="sxs-lookup"><span data-stu-id="e5470-213">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="e5470-214">Aşağıdaki örnek her yöntemi dışında için CORS sağlar `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="e5470-214">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="e5470-215">**Genel olarak**</span><span class="sxs-lookup"><span data-stu-id="e5470-215">**Globally**</span></span>

<span data-ttu-id="e5470-216">Uygulamanızdaki tüm Web APİ'si denetleyicilerinin CORS'yi etkinleştirmek için geçirmek bir **EnableCorsAttribute** için örnek **EnableCors** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="e5470-216">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="e5470-217">Birden fazla kapsamda öznitelik ayarlanırsa, öncelik sırası şöyledir:</span><span class="sxs-lookup"><span data-stu-id="e5470-217">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="e5470-218">Eylem</span><span class="sxs-lookup"><span data-stu-id="e5470-218">Action</span></span>
2. <span data-ttu-id="e5470-219">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="e5470-219">Controller</span></span>
3. <span data-ttu-id="e5470-220">Global</span><span class="sxs-lookup"><span data-stu-id="e5470-220">Global</span></span>

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a><span data-ttu-id="e5470-221">İzin verilen çıkış noktaları ayarlama</span><span class="sxs-lookup"><span data-stu-id="e5470-221">Set the Allowed Origins</span></span>

<span data-ttu-id="e5470-222">*Kaynakları* parametresinin **[EnableCors]** özniteliği belirtir kaynağa erişmek için hangi çıkış noktaları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="e5470-222">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="e5470-223">İzin verilen çıkış noktaları, virgülle ayrılmış listesini değerdir.</span><span class="sxs-lookup"><span data-stu-id="e5470-223">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="e5470-224">Joker karakter değeri de kullanabilirsiniz "\*" tüm kaynaklar gelen isteklere izin vermek için.</span><span class="sxs-lookup"><span data-stu-id="e5470-224">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="e5470-225">Her türlü kaynağa gelen isteklere izin vermeden önce dikkatlice düşünün.</span><span class="sxs-lookup"><span data-stu-id="e5470-225">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="e5470-226">Bu, tam anlamıyla herhangi bir Web sitesinde Web API AJAX çağrıları yapabileceğini anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="e5470-226">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="e5470-227">İzin verilen HTTP yöntemleri Ayarla</span><span class="sxs-lookup"><span data-stu-id="e5470-227">Set the Allowed HTTP Methods</span></span>

<span data-ttu-id="e5470-228">*Yöntemleri* parametresinin **[EnableCors]** özniteliği belirtir kaynağa erişmek için hangi HTTP yöntemlerine izin verilir.</span><span class="sxs-lookup"><span data-stu-id="e5470-228">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="e5470-229">Tüm yöntemlere izin için joker karakter değeri kullanın. "\*".</span><span class="sxs-lookup"><span data-stu-id="e5470-229">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="e5470-230">Aşağıdaki örnek yalnızca GET ve POST istekleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="e5470-230">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="e5470-231">İzin verilen istek üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="e5470-231">Set the Allowed Request Headers</span></span>

<span data-ttu-id="e5470-232">Daha önce ben bir denetim öncesi isteği bir Access-Control-Request-Headers üstbilgisi nasıl içerebilir uygulama tarafından ayarlanıp HTTP üst bilgilerini listeleme açıklanan (Malum "yazar, istek üst bilgileri").</span><span class="sxs-lookup"><span data-stu-id="e5470-232">Earlier I described how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="e5470-233">*Üstbilgileri* parametresinin **[EnableCors]** özniteliği belirtir hangi yazar istek üst bilgiye izin verilir.</span><span class="sxs-lookup"><span data-stu-id="e5470-233">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="e5470-234">Üst bilgileri izin verecek şekilde ayarlanmış *üstbilgileri* için "\*".</span><span class="sxs-lookup"><span data-stu-id="e5470-234">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="e5470-235">Beyaz liste belirli üstbilgileri için ayarlanmış *üstbilgileri* izin verilen üstbilgileri virgülle ayrılmış bir listesi için:</span><span class="sxs-lookup"><span data-stu-id="e5470-235">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="e5470-236">Ancak, tarayıcılar nasıl bunlar Access-Control-Request-Headers kümesinde tamamen tutarlı değil.</span><span class="sxs-lookup"><span data-stu-id="e5470-236">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="e5470-237">Şu anda "Başlangıç"; Örneğin, Chrome içerir olsa da bile uygulama bunları betikte ayarladığında FireFox "Kabul et" gibi standart başlıklarını içermez.</span><span class="sxs-lookup"><span data-stu-id="e5470-237">For example, Chrome currently includes "origin"; while FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="e5470-238">Ayarlarsanız *üstbilgileri* dışında herhangi bir şey için "\*", "kabul", "content-type" ve "Başlangıç" yanı sıra, desteklemek istediğiniz tüm özel üst en az içermelidir.</span><span class="sxs-lookup"><span data-stu-id="e5470-238">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="e5470-239">İzin verilen yanıt üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="e5470-239">Set the Allowed Response Headers</span></span>

<span data-ttu-id="e5470-240">Varsayılan olarak, tarayıcı tüm yanıt üstbilgilerini uygulamaya kullanıma sunmuyor.</span><span class="sxs-lookup"><span data-stu-id="e5470-240">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="e5470-241">Varsayılan olarak kullanılabilir olan yanıt üstbilgilerini şunlardır:</span><span class="sxs-lookup"><span data-stu-id="e5470-241">The response headers that are available by default are:</span></span>

- <span data-ttu-id="e5470-242">Önbellek denetimi</span><span class="sxs-lookup"><span data-stu-id="e5470-242">Cache-Control</span></span>
- <span data-ttu-id="e5470-243">İçerik dili</span><span class="sxs-lookup"><span data-stu-id="e5470-243">Content-Language</span></span>
- <span data-ttu-id="e5470-244">İçerik Türü</span><span class="sxs-lookup"><span data-stu-id="e5470-244">Content-Type</span></span>
- <span data-ttu-id="e5470-245">Süre sonu</span><span class="sxs-lookup"><span data-stu-id="e5470-245">Expires</span></span>
- <span data-ttu-id="e5470-246">Son değiştirilme</span><span class="sxs-lookup"><span data-stu-id="e5470-246">Last-Modified</span></span>
- <span data-ttu-id="e5470-247">Pragması</span><span class="sxs-lookup"><span data-stu-id="e5470-247">Pragma</span></span>

<span data-ttu-id="e5470-248">CORS spec bu çağrıları [basit yanıt üstbilgilerini](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="e5470-248">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="e5470-249">Diğer üst bilgileri uygulama için kullanılabilir hale getirmek için ayarlanmış *exposedHeaders* parametresinin **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="e5470-249">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="e5470-250">Aşağıdaki örnekte, denetleyici 's `Get` yöntemi 'X-Custom-Header' adlı bir özel üst bilgi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="e5470-250">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="e5470-251">Varsayılan olarak, tarayıcı bu çıkış noktaları arası istek üstbilgisinde açığa çıkarmamak.</span><span class="sxs-lookup"><span data-stu-id="e5470-251">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="e5470-252">Üst bilgi kullanılabilir hale getirmek için 'X-Custom-Header' dahil *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="e5470-252">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a><span data-ttu-id="e5470-253">Çıkış noktaları arası istekleri kimlik bilgilerini geçirerek</span><span class="sxs-lookup"><span data-stu-id="e5470-253">Passing Credentials in Cross-Origin Requests</span></span>

<span data-ttu-id="e5470-254">Özel işleme bir CORS isteğinde kimlik bilgilerini gerektirir.</span><span class="sxs-lookup"><span data-stu-id="e5470-254">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="e5470-255">Varsayılan olarak, tarayıcının çıkış noktaları arası istek ile herhangi bir kimlik bilgisi göndermez.</span><span class="sxs-lookup"><span data-stu-id="e5470-255">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="e5470-256">Kimlik bilgileri, tanımlama bilgilerinin yanı sıra HTTP kimlik doğrulama düzenleri içerir.</span><span class="sxs-lookup"><span data-stu-id="e5470-256">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="e5470-257">İstemci kimlik bilgileriyle bir çıkış noktaları arası istek göndermek için ayarlamalısınız **XMLHttpRequest.withCredentials** true.</span><span class="sxs-lookup"><span data-stu-id="e5470-257">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="e5470-258">Kullanarak **XMLHttpRequest** doğrudan:</span><span class="sxs-lookup"><span data-stu-id="e5470-258">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="e5470-259">JQuery içinde:</span><span class="sxs-lookup"><span data-stu-id="e5470-259">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="e5470-260">Ayrıca, sunucunun kimlik bilgilerini izin vermeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e5470-260">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="e5470-261">Web API'de çıkış noktaları arası kimlik bilgileri'izin verecek şekilde ayarlanmış **SupportsCredentials** özelliğini true olarak **[EnableCors]** özniteliği:</span><span class="sxs-lookup"><span data-stu-id="e5470-261">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="e5470-262">Bu özellik true ise, HTTP yanıtı erişim-denetim-Allow-Credentials üst bilgisini içerir.</span><span class="sxs-lookup"><span data-stu-id="e5470-262">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="e5470-263">Bu üst bilgi, tarayıcı sunucunun bir çıkış noktaları arası istek için kimlik bilgilerini sağlar söyler.</span><span class="sxs-lookup"><span data-stu-id="e5470-263">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="e5470-264">Tarayıcı bilgilerini gönderir, ancak yanıt, geçerli bir erişim-denetim-Allow-Credentials üst bilgisi içermez, tarayıcı uygulamaya yanıt açığa çıkarmamak ve AJAX isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="e5470-264">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="e5470-265">İlgili ayarı çok dikkatli olun **SupportsCredentials** başka bir etki alanındaki bir Web sitesi gönderebilir bir oturum açmış kullanıcı kimlik bilgilerini kullanıcının adına, Web API'niz için farkına varmadan kullanıcı anlamına geldiğinden, true.</span><span class="sxs-lookup"><span data-stu-id="e5470-265">Be very careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="e5470-266">CORS spec Ayrıca bu ayar durumları *kaynakları* için &quot; \* &quot; geçersiz olursa **SupportsCredentials** geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="e5470-266">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a><span data-ttu-id="e5470-267">Özel CORS İlkesi sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="e5470-267">Custom CORS Policy Providers</span></span>

<span data-ttu-id="e5470-268">**[EnableCors]** özniteliğini uygular **ICorsPolicyProvider** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="e5470-268">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="e5470-269">Türetilen bir sınıf oluşturarak kendi uygulamanız sağlayabilir **özniteliği** ve uygulayan **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="e5470-269">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="e5470-270">Artık tüm yerleştirme, koyabilirsiniz öznitelik uygulayabilirsiniz **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="e5470-270">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="e5470-271">Örneğin, özel bir CORS ilke sağlayıcısı, ayarlar yapılandırma dosyasından okuyabilir.</span><span class="sxs-lookup"><span data-stu-id="e5470-271">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="e5470-272">Öznitelikleri kullanarak alternatif olarak, kaydedebileceğiniz bir **ICorsPolicyProviderFactory** oluşturan nesne **ICorsPolicyProvider** nesneleri.</span><span class="sxs-lookup"><span data-stu-id="e5470-272">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="e5470-273">Ayarlanacak **ICorsPolicyProviderFactory**, çağrı **SetCorsPolicyProviderFactory** genişletme yöntemi aşağıdaki gibi başlangıçta:</span><span class="sxs-lookup"><span data-stu-id="e5470-273">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a><span data-ttu-id="e5470-274">Tarayıcı desteği</span><span class="sxs-lookup"><span data-stu-id="e5470-274">Browser Support</span></span>

<span data-ttu-id="e5470-275">Web API CORS, bir sunucu tarafı teknoloji paketidir.</span><span class="sxs-lookup"><span data-stu-id="e5470-275">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="e5470-276">Ayrıca kullanıcının tarayıcı CORS desteği gerekir.</span><span class="sxs-lookup"><span data-stu-id="e5470-276">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="e5470-277">Neyse ki, tüm temel tarayıcılarda geçerli sürümleri dahil [desteklemek için CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="e5470-277">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>

<span data-ttu-id="e5470-278">Internet Explorer 8 ve Internet Explorer 9 eski XDomainRequest nesne yerine XMLHttpRequest kullanarak kısmi CORS desteği vardır.</span><span class="sxs-lookup"><span data-stu-id="e5470-278">Internet Explorer 8 and Internet Explorer 9 have partial support for CORS, using the legacy XDomainRequest object instead of XMLHttpRequest.</span></span> <span data-ttu-id="e5470-279">Daha fazla bilgi için [XDomainRequest - sınırlamaları, kısıtlamalar ve geçici çözümler](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5470-279">For more information, see [XDomainRequest - Restrictions, Limitations and Workarounds](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span></span>
