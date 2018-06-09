---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: ASP.NET Web API 2 çıkış noktaları arası istekleri etkinleştirme | Microsoft Docs
author: MikeWasson
description: ASP.NET Web API'de çıkış noktaları arası kaynak paylaşımı (CORS) desteklemek nasıl gösterir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26566820"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a><span data-ttu-id="16041-103">ASP.NET Web API 2 çıkış noktaları arası istekleri etkinleştirme</span><span class="sxs-lookup"><span data-stu-id="16041-103">Enabling Cross-Origin Requests in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="16041-104">tarafından [CAN Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="16041-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="16041-105">Tarayıcı güvenlik bir web sayfası AJAX istekleri başka bir etki alanına yapmasını engeller.</span><span class="sxs-lookup"><span data-stu-id="16041-105">Browser security prevents a web page from making AJAX requests to another domain.</span></span> <span data-ttu-id="16041-106">Bu kısıtlama adlı *kaynak aynı ilke*ve kötü amaçlı bir siteyi başka bir siteden hassas verileri okumasını önler.</span><span class="sxs-lookup"><span data-stu-id="16041-106">This restriction is called the *same-origin policy*, and prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="16041-107">Ancak, bazen web API çağrısı diğer sitelere izin vermek isteyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16041-107">However, sometimes you might want to let other sites call your web API.</span></span>
> 
> <span data-ttu-id="16041-108">[Kaynak kaynak paylaşma arası](http://www.w3.org/TR/cors/) (CORS) olan W3C standart bir kaynak aynı ilke hafifletin sunucusunun sağlar.</span><span class="sxs-lookup"><span data-stu-id="16041-108">[Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) is a W3C standard that allows a server to relax the same-origin policy.</span></span> <span data-ttu-id="16041-109">CORS kullanarak, bir sunucu açıkça bazı cross-origin istekleri başkalarının reddetme çalışırken izin verebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16041-109">Using CORS, a server can explicitly allow some cross-origin requests while rejecting others.</span></span> <span data-ttu-id="16041-110">CORS daha güvenli ve önceki teknikleri daha esnek gibi [JSONP](http://en.wikipedia.org/wiki/JSONP).</span><span class="sxs-lookup"><span data-stu-id="16041-110">CORS is safer and more flexible than earlier techniques such as [JSONP](http://en.wikipedia.org/wiki/JSONP).</span></span> <span data-ttu-id="16041-111">Bu öğretici, Web API uygulamanızda CORS'yi etkinleştirmeniz gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="16041-111">This tutorial shows how to enable CORS in your Web API application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="16041-112">Öğreticide kullanılan yazılım sürümleri</span><span class="sxs-lookup"><span data-stu-id="16041-112">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="16041-113">Visual Studio 2013 Güncelleştirme 2</span><span class="sxs-lookup"><span data-stu-id="16041-113">Visual Studio 2013 Update 2</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="16041-114">Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="16041-114">Web API 2.2</span></span>


<a id="intro"></a>
## <a name="introduction"></a><span data-ttu-id="16041-115">Giriş</span><span class="sxs-lookup"><span data-stu-id="16041-115">Introduction</span></span>

<span data-ttu-id="16041-116">Bu öğretici, ASP.NET Web API CORS desteği gösterir.</span><span class="sxs-lookup"><span data-stu-id="16041-116">This tutorial demonstrates CORS support in ASP.NET Web API.</span></span> <span data-ttu-id="16041-117">İki ASP.NET projeleri – "bir Web API denetleyicisi barındırır, bir çağrılan WebService" ve "WebService çağıran diğer çağrılan WebClient", oluşturarak başlayacağız.</span><span class="sxs-lookup"><span data-stu-id="16041-117">We'll start by creating two ASP.NET projects – one called "WebService", which hosts a Web API controller, and the other called "WebClient", which calls WebService.</span></span> <span data-ttu-id="16041-118">Farklı etki alanlarında iki uygulama barındırıldığından, WebClient bir AJAX isteği WebService çıkış noktaları arası bir istektir.</span><span class="sxs-lookup"><span data-stu-id="16041-118">Because the two applications are hosted at different domains, an AJAX request from WebClient to WebService is a cross-origin request.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a><span data-ttu-id="16041-119">"Aynı kaynak" nedir?</span><span class="sxs-lookup"><span data-stu-id="16041-119">What is "Same Origin"?</span></span>

<span data-ttu-id="16041-120">Aynı düzenleri, konakları ve bağlantı noktaları varsa iki URL'leri aynı kaynağa sahip.</span><span class="sxs-lookup"><span data-stu-id="16041-120">Two URLs have the same origin if they have identical schemes, hosts, and ports.</span></span> <span data-ttu-id="16041-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span><span class="sxs-lookup"><span data-stu-id="16041-121">([RFC 6454](http://tools.ietf.org/html/rfc6454))</span></span>

<span data-ttu-id="16041-122">Bu iki URL'leri aynı kaynağa sahip:</span><span class="sxs-lookup"><span data-stu-id="16041-122">These two URLs have the same origin:</span></span>

- `http://example.com/foo.html`
- `http://example.com/bar.html`

<span data-ttu-id="16041-123">Bu URL'leri iki farklı çıkış önceki daha vardır:</span><span class="sxs-lookup"><span data-stu-id="16041-123">These URLs have different origins than the previous two:</span></span>

- <span data-ttu-id="16041-124">`http://example.net` -Farklı bir etki alanı</span><span class="sxs-lookup"><span data-stu-id="16041-124">`http://example.net` - Different domain</span></span>
- <span data-ttu-id="16041-125">`http://example.com:9000/foo.html` -Farklı bir bağlantı noktası</span><span class="sxs-lookup"><span data-stu-id="16041-125">`http://example.com:9000/foo.html` - Different port</span></span>
- <span data-ttu-id="16041-126">`https://example.com/foo.html` -Farklı düzeni</span><span class="sxs-lookup"><span data-stu-id="16041-126">`https://example.com/foo.html` - Different scheme</span></span>
- <span data-ttu-id="16041-127">`http://www.example.com/foo.html` -Farklı bir alt etki alanı</span><span class="sxs-lookup"><span data-stu-id="16041-127">`http://www.example.com/foo.html` - Different subdomain</span></span>

> [!NOTE]
> <span data-ttu-id="16041-128">Internet Explorer bağlantı noktası kaynakları karşılaştırılırken dikkate almaz.</span><span class="sxs-lookup"><span data-stu-id="16041-128">Internet Explorer does not consider the port when comparing origins.</span></span>


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a><span data-ttu-id="16041-129">Web hizmeti projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="16041-129">Create the WebService Project</span></span>

> [!NOTE]
> <span data-ttu-id="16041-130">Bu bölümde, Web API projeleri oluşturmak nasıl zaten biliyor varsayar.</span><span class="sxs-lookup"><span data-stu-id="16041-130">This section assumes you already know how to create Web API projects.</span></span> <span data-ttu-id="16041-131">Aksi takdirde bkz [ASP.NET Web API ile çalışmaya başlama](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="16041-131">If not, see [Getting Started with ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>


<span data-ttu-id="16041-132">Visual Studio'yu açın ve yeni bir **ASP.NET Web uygulaması** projesi.</span><span class="sxs-lookup"><span data-stu-id="16041-132">Start Visual Studio and create a new **ASP.NET Web Application** project.</span></span> <span data-ttu-id="16041-133">Seçin **boş** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="16041-133">Select the **Empty** project template.</span></span> <span data-ttu-id="16041-134">"Klasörleri ekleyin ve çekirdek için başvurular" altında seçin **Web API** onay kutusu.</span><span class="sxs-lookup"><span data-stu-id="16041-134">Under "Add folders and core references for", select the **Web API** checkbox.</span></span> <span data-ttu-id="16041-135">İsteğe bağlı olarak, uygulama Mircosoft Azure'a dağıtmak için "Bulutta Barındır" seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="16041-135">Optionally, select the "Host in Cloud" option to deploy the app to Mircosoft Azure.</span></span> <span data-ttu-id="16041-136">Microsoft, 10 Web siteleri için ücretsiz bir web barındırma sunar bir [ücretsiz Azure deneme hesabı](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span><span class="sxs-lookup"><span data-stu-id="16041-136">Microsoft offers free web hosting for up to 10 websites in a [free Azure trial account](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

<span data-ttu-id="16041-137">Adlı bir Web API denetleyicisi ekleme `TestController` aşağıdaki kod ile:</span><span class="sxs-lookup"><span data-stu-id="16041-137">Add a Web API controller named `TestController` with the following code:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

<span data-ttu-id="16041-138">Uygulamayı yerel olarak çalıştırın veya Azure'a dağıtın.</span><span class="sxs-lookup"><span data-stu-id="16041-138">You can run the application locally or deploy to Azure.</span></span> <span data-ttu-id="16041-139">(Bu öğreticide ekran ı Azure App Service Web Apps için dağıtılır.) Web API çalıştığını doğrulamak için gidin `http://hostname/api/test/`, burada *ana bilgisayar adı* uygulamanın dağıtıldığı etki alanıdır.</span><span class="sxs-lookup"><span data-stu-id="16041-139">(For the screenshots in this tutorial, I deployed to Azure App Service Web Apps.) To verify that the web API is working, navigate to `http://hostname/api/test/`, where *hostname* is the domain where you deployed the application.</span></span> <span data-ttu-id="16041-140">Yanıt metni görmelisiniz &quot;alın: sınama iletisi&quot;.</span><span class="sxs-lookup"><span data-stu-id="16041-140">You should see the response text, &quot;GET: Test Message&quot;.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a><span data-ttu-id="16041-141">WebClient projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="16041-141">Create the WebClient Project</span></span>

<span data-ttu-id="16041-142">Başka bir ASP.NET Web uygulaması projesi oluşturmak ve **MVC** proje şablonu.</span><span class="sxs-lookup"><span data-stu-id="16041-142">Create another ASP.NET Web Application project and select the **MVC** project template.</span></span> <span data-ttu-id="16041-143">İsteğe bağlı olarak, seçin **kimlik doğrulamayı Değiştir** > **doğrulaması yok**.</span><span class="sxs-lookup"><span data-stu-id="16041-143">Optionally, select **Change Authentication** > **No Authentication**.</span></span> <span data-ttu-id="16041-144">Bu öğretici için kimlik doğrulama gerekmez.</span><span class="sxs-lookup"><span data-stu-id="16041-144">You don't need authentication for this tutorial.</span></span>

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

<span data-ttu-id="16041-145">Çözüm Gezgini'nde Views/Home/Index.cshtml dosyasını açın.</span><span class="sxs-lookup"><span data-stu-id="16041-145">In Solution Explorer, open the file Views/Home/Index.cshtml.</span></span> <span data-ttu-id="16041-146">Bu dosyadaki kod aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="16041-146">Replace the code in this file with the following:</span></span>

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

<span data-ttu-id="16041-147">İçin *serviceUrl* değişkeni, Web hizmeti uygulama URI'sini kullanın.</span><span class="sxs-lookup"><span data-stu-id="16041-147">For the *serviceUrl* variable, use the URI of the WebService app.</span></span> <span data-ttu-id="16041-148">Şimdi WebClient uygulamayı yerel olarak çalıştırma veya başka bir Web sitesine yayımlayın.</span><span class="sxs-lookup"><span data-stu-id="16041-148">Now run the WebClient app locally or publish it to another website.</span></span>

<span data-ttu-id="16041-149">Listelenen HTTP yöntemini kullanarak Web hizmeti uygulama AJAX isteği gönderen "deneyin" düğmesini tıklatarak açılan kutusundan (GET, POST veya PUT).</span><span class="sxs-lookup"><span data-stu-id="16041-149">Clicking the "Try It" button submits an AJAX request to the WebService app, using the HTTP method listed in the dropdown box (GET, POST, or PUT).</span></span> <span data-ttu-id="16041-150">Bu, bize farklı cross-origin istekleri inceleyin sağlar.</span><span class="sxs-lookup"><span data-stu-id="16041-150">This lets us examine different cross-origin requests.</span></span> <span data-ttu-id="16041-151">Şu anda, Web hizmeti uygulama CORS, desteklemiyor nedenle düğmesine tıklarsanız, hatayla karşılaşırsınız.</span><span class="sxs-lookup"><span data-stu-id="16041-151">Right now, the WebService app does not support CORS, so if you click the button, you will get an error.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> <span data-ttu-id="16041-152">Bir aracının HTTP trafiğini izleme hoşlanıyorsanız [Fiddler](http://www.telerik.com/fiddler), tarayıcıyı GET isteğini göndermek ve istek başarılı ancak AJAX çağrısı bir hata döndürüyor görürsünüz.</span><span class="sxs-lookup"><span data-stu-id="16041-152">If you watch the HTTP traffic in a tool like [Fiddler](http://www.telerik.com/fiddler), you will see that the browser does send the GET request, and the request succeeds, but the AJAX call returns an error.</span></span> <span data-ttu-id="16041-153">Kaynak aynı ilke tarayıcıdan engellemez anlamak önemlidir *gönderme* istek.</span><span class="sxs-lookup"><span data-stu-id="16041-153">It's important to understand that same-origin policy does not prevent the browser from *sending* the request.</span></span> <span data-ttu-id="16041-154">Bunun yerine, uygulama görmesini engeller *yanıt*.</span><span class="sxs-lookup"><span data-stu-id="16041-154">Instead, it prevents the application from seeing the *response*.</span></span>


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a><span data-ttu-id="16041-155">CORS etkinleştir</span><span class="sxs-lookup"><span data-stu-id="16041-155">Enable CORS</span></span>

<span data-ttu-id="16041-156">Şimdi şimdi CORS WebService uygulamada etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="16041-156">Now let's enable CORS in the WebService app.</span></span> <span data-ttu-id="16041-157">İlk olarak, CORS NuGet paketini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="16041-157">First, add the CORS NuGet package.</span></span> <span data-ttu-id="16041-158">Visual Studio'da gelen **Araçları** menüsünde, select **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**.</span><span class="sxs-lookup"><span data-stu-id="16041-158">In Visual Studio, from the **Tools** menu, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="16041-159">Paket Yöneticisi konsolu penceresinde aşağıdaki komutu yazın:</span><span class="sxs-lookup"><span data-stu-id="16041-159">In the Package Manager Console window, type the following command:</span></span>

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

<span data-ttu-id="16041-160">Bu komut, en son paketini yükler ve çekirdek Web API kitaplıkları da dahil olmak üzere tüm bağımlılıkları güncelleştirir.</span><span class="sxs-lookup"><span data-stu-id="16041-160">This command installs the latest package and updates all dependencies, including the core Web API libraries.</span></span> <span data-ttu-id="16041-161">Kullanıcı belirli bir sürümünü hedefleyecek şekilde sürüm bayrağı.</span><span class="sxs-lookup"><span data-stu-id="16041-161">User the -Version flag to target a specific version.</span></span> <span data-ttu-id="16041-162">CORS paketi Web API 2.0 veya sonraki sürümünü gerektirir.</span><span class="sxs-lookup"><span data-stu-id="16041-162">The CORS package requires Web API 2.0 or later.</span></span>

<span data-ttu-id="16041-163">Uygulama dosyasını açın\_Start/WebApiConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="16041-163">Open the file App\_Start/WebApiConfig.cs.</span></span> <span data-ttu-id="16041-164">Aşağıdaki kodu ekleyin **WebApiConfig.Register** yöntemi.</span><span class="sxs-lookup"><span data-stu-id="16041-164">Add the following code to the **WebApiConfig.Register** method.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

<span data-ttu-id="16041-165">Ardından, eklemek **[EnableCors]** özniteliğini `TestController` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="16041-165">Next, add the **[EnableCors]** attribute to the `TestController` class:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

<span data-ttu-id="16041-166">İçin *çıkış* parametresi, WebClient uygulamanın dağıtıldığı bir URI kullanın.</span><span class="sxs-lookup"><span data-stu-id="16041-166">For the *origins* parameter, use the URI where you deployed the WebClient application.</span></span> <span data-ttu-id="16041-167">Bu cross-origin istekleri WebClient tüm diğer etki alanları arası istekleri hala vermemek sırasında sağlar.</span><span class="sxs-lookup"><span data-stu-id="16041-167">This allows cross-origin requests from WebClient, while still disallowing all other cross-domain requests.</span></span> <span data-ttu-id="16041-168">Daha sonra parametrelerini açýklayacaðýz **[EnableCors]** daha ayrıntılı.</span><span class="sxs-lookup"><span data-stu-id="16041-168">Later, I'll describe the parameters for **[EnableCors]** in more detail.</span></span>

<span data-ttu-id="16041-169">Eğik sonunda içermez *çıkış* URL.</span><span class="sxs-lookup"><span data-stu-id="16041-169">Do not include a forward slash at the end of the *origins* URL.</span></span>

<span data-ttu-id="16041-170">Güncelleştirilmiş WebService uygulamayı yeniden dağıtın.</span><span class="sxs-lookup"><span data-stu-id="16041-170">Redeploy the updated WebService application.</span></span> <span data-ttu-id="16041-171">WebClient güncelleştirme gerek yoktur.</span><span class="sxs-lookup"><span data-stu-id="16041-171">You don't need to update WebClient.</span></span> <span data-ttu-id="16041-172">WebClient AJAX isteği başarılı olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="16041-172">Now the AJAX request from WebClient should succeed.</span></span> <span data-ttu-id="16041-173">GET, PUT ve POST yöntemleri tüm izin verilir.</span><span class="sxs-lookup"><span data-stu-id="16041-173">The GET, PUT, and POST methods are all allowed.</span></span>

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a><span data-ttu-id="16041-174">CORS nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="16041-174">How CORS Works</span></span>

<span data-ttu-id="16041-175">Bu bölümde, HTTP iletileri düzeyinde bir CORS istek olanlar açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="16041-175">This section describes what happens in a CORS request, at the level of the HTTP messages.</span></span> <span data-ttu-id="16041-176">CORS nasıl çalışır, böylece yapılandırabileceğiniz anlamak önemlidir **[EnableCors]** doğru özniteliği ve beklediğiniz gibi şeyler çalışmazsa sorun giderme.</span><span class="sxs-lookup"><span data-stu-id="16041-176">It's important to understand how CORS works, so that you can configure the **[EnableCors]** attribute correctly, and troubleshoot if things don't work as you expect.</span></span>

<span data-ttu-id="16041-177">CORS belirtimi çıkış noktaları arası istekleri etkinleştirme birkaç yeni HTTP üstbilgileri tanıtır.</span><span class="sxs-lookup"><span data-stu-id="16041-177">The CORS specification introduces several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="16041-178">Bir tarayıcı CORS destekliyorsa, bu üstbilgileri cross-origin istekleri için otomatik olarak ayarlar; JavaScript kodunuzda özel bir şey yapmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="16041-178">If a browser supports CORS, it sets these headers automatically for cross-origin requests; you don't need to do anything special in your JavaScript code.</span></span>

<span data-ttu-id="16041-179">Çıkış noktaları arası istek bir örneği burada verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="16041-179">Here is an example of a cross-origin request.</span></span> <span data-ttu-id="16041-180">"Kaynak" başlığı istekte sitenin etki alanını sağlar.</span><span class="sxs-lookup"><span data-stu-id="16041-180">The "Origin" header gives the domain of the site that is making the request.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

<span data-ttu-id="16041-181">Sunucu isteği izin veriyorsa, Access-Control-Allow-Origin üstbilgisini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="16041-181">If the server allows the request, it sets the Access-Control-Allow-Origin header.</span></span> <span data-ttu-id="16041-182">Bu üstbilgi değerini kaynak üst bilgisini eşleşen veya joker karakter değeri "\*", yani her türlü kaynağa izin verilir.</span><span class="sxs-lookup"><span data-stu-id="16041-182">The value of this header either matches the Origin header, or is the wildcard value "\*", meaning that any origin is allowed.</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

<span data-ttu-id="16041-183">Yanıt Access-Control-Allow-Origin üst bilgi içermeyen AJAX isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="16041-183">If the response does not include the Access-Control-Allow-Origin header, the AJAX request fails.</span></span> <span data-ttu-id="16041-184">Özellikle, tarayıcı istek izin vermez.</span><span class="sxs-lookup"><span data-stu-id="16041-184">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="16041-185">Tarayıcı yanıt, sunucunun başarılı bir yanıt döndürürse bile, istemci uygulamasının kullanımına yapmaz.</span><span class="sxs-lookup"><span data-stu-id="16041-185">Even if the server returns a successful response, the browser does not make the response available to the client application.</span></span>

<span data-ttu-id="16041-186">**Denetim öncesi isteği**</span><span class="sxs-lookup"><span data-stu-id="16041-186">**Preflight Requests**</span></span>

<span data-ttu-id="16041-187">Bazı CORS istekler için tarayıcı kaynak gerçek isteği göndermeden önce bir "Denetim öncesi isteği" adlı bir ek istek gönderir.</span><span class="sxs-lookup"><span data-stu-id="16041-187">For some CORS requests, the browser sends an additional request, called a "preflight request", before it sends the actual request for the resource.</span></span>

<span data-ttu-id="16041-188">Aşağıdaki koşullar doğruysa, tarayıcı denetim öncesi isteği atlayabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="16041-188">The browser can skip the preflight request if the following conditions are true:</span></span>

- <span data-ttu-id="16041-189">İstek yöntemini GET, HEAD veya POST, olan *ve*</span><span class="sxs-lookup"><span data-stu-id="16041-189">The request method is GET, HEAD, or POST, *and*</span></span>
- <span data-ttu-id="16041-190">Uygulama kabul etme, Accept-Language, Content-Language, dışındaki tüm istek üstbilgileri ayarlı değil Content-Type veya son-olay-ID *ve*</span><span class="sxs-lookup"><span data-stu-id="16041-190">The application does not set any request headers other than Accept, Accept-Language, Content-Language, Content-Type, or Last-Event-ID, *and*</span></span>
- <span data-ttu-id="16041-191">Content-Type üstbilgisi (varsa ayarlayın) aşağıdakilerden biri:</span><span class="sxs-lookup"><span data-stu-id="16041-191">The Content-Type header (if set) is one of the following:</span></span> 

    - <span data-ttu-id="16041-192">Uygulama/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="16041-192">application/x-www-form-urlencoded</span></span>
    - <span data-ttu-id="16041-193">multipart/form-data</span><span class="sxs-lookup"><span data-stu-id="16041-193">multipart/form-data</span></span>
    - <span data-ttu-id="16041-194">Metin/düz</span><span class="sxs-lookup"><span data-stu-id="16041-194">text/plain</span></span>

<span data-ttu-id="16041-195">Uygulama çağırarak ayarlar üstbilgileri istek üstbilgileri hakkında kuralın uygulanacağı **setRequestHeader** üzerinde **XMLHttpRequest** nesnesi.</span><span class="sxs-lookup"><span data-stu-id="16041-195">The rule about request headers applies to headers that the application sets by calling **setRequestHeader** on the **XMLHttpRequest** object.</span></span> <span data-ttu-id="16041-196">(CORS belirtimi bu "Yazar istek üstbilgileri" çağırır.) Kural üstbilgileri uygulanmaz *tarayıcı* User-Agent, ana bilgisayar veya Content-Length gibi ayarlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16041-196">(The CORS specification calls these "author request headers".) The rule does not apply to headers the *browser* can set, such as User-Agent, Host, or Content-Length.</span></span>

<span data-ttu-id="16041-197">Bir denetim öncesi isteği bir örneği burada verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="16041-197">Here is an example of a preflight request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

<span data-ttu-id="16041-198">Ön uçuş isteği HTTP OPTIONS yöntemini kullanır.</span><span class="sxs-lookup"><span data-stu-id="16041-198">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="16041-199">İki özel üstbilgi içerir:</span><span class="sxs-lookup"><span data-stu-id="16041-199">It includes two special headers:</span></span>

- <span data-ttu-id="16041-200">Access-Control-Request-Method: fiili istek için kullanılacak HTTP yöntemi.</span><span class="sxs-lookup"><span data-stu-id="16041-200">Access-Control-Request-Method: The HTTP method that will be used for the actual request.</span></span>
- <span data-ttu-id="16041-201">Access-Control-Request-Headers: İstek üstbilgilerini listesi, *uygulama* gerçek isteği ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="16041-201">Access-Control-Request-Headers: A list of request headers that the *application* set on the actual request.</span></span> <span data-ttu-id="16041-202">(Yeniden, bu tarayıcı ayarlar üstbilgileri dahil değildir.)</span><span class="sxs-lookup"><span data-stu-id="16041-202">(Again, this does not include headers that the browser sets.)</span></span>

<span data-ttu-id="16041-203">Sunucu isteği izin verdiğini varsayarak bir örnek yanıt şöyledir:</span><span class="sxs-lookup"><span data-stu-id="16041-203">Here is an example response, assuming that the server allows the request:</span></span>

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

<span data-ttu-id="16041-204">Yanıt, izin verilen yöntemleri listeleyen bir erişim-denetim-Allow-Methods üstbilgisi ve isteğe bağlı olarak izin verilen üstbilgileri listeler bir Access-Control-izin ver-Headers üstbilgisi içeriyor.</span><span class="sxs-lookup"><span data-stu-id="16041-204">The response includes an Access-Control-Allow-Methods header that lists the allowed methods, and optionally an Access-Control-Allow-Headers header, which lists the allowed headers.</span></span> <span data-ttu-id="16041-205">Denetim öncesi isteği başarılı olursa, tarayıcı daha önce açıklandığı gibi gerçek isteğini gönderir.</span><span class="sxs-lookup"><span data-stu-id="16041-205">If the preflight request succeeds, the browser sends the actual request, as described earlier.</span></span>

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a><span data-ttu-id="16041-206">[EnableCors] için kapsam kuralları</span><span class="sxs-lookup"><span data-stu-id="16041-206">Scope Rules for [EnableCors]</span></span>

<span data-ttu-id="16041-207">Uygulamanızda eylem, denetleyici başına veya genel olarak tüm Web API denetleyicilerinin başına CORS etkinleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="16041-207">You can enable CORS per action, per controller, or globally for all Web API controllers in your application.</span></span>

<span data-ttu-id="16041-208">**Her eylem**</span><span class="sxs-lookup"><span data-stu-id="16041-208">**Per Action**</span></span>

<span data-ttu-id="16041-209">Tek bir eylem için CORS etkinleştirmek için ayarlanmış **[EnableCors]** özniteliği eylem yöntemi.</span><span class="sxs-lookup"><span data-stu-id="16041-209">To enable CORS for a single action, set the **[EnableCors]** attribute on the action method.</span></span> <span data-ttu-id="16041-210">Aşağıdaki örnek için CORS etkinleştirir `GetItem` yalnızca yöntemi.</span><span class="sxs-lookup"><span data-stu-id="16041-210">The following example enables CORS for the `GetItem` method only.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

<span data-ttu-id="16041-211">**Denetleyici**</span><span class="sxs-lookup"><span data-stu-id="16041-211">**Per Controller**</span></span>

<span data-ttu-id="16041-212">Ayarlarsanız **[EnableCors]** denetleyici sınıfını denetleyicisinde tüm eylemler için geçerli olur.</span><span class="sxs-lookup"><span data-stu-id="16041-212">If you set **[EnableCors]** on the controller class, it applies to all the actions on the controller.</span></span> <span data-ttu-id="16041-213">Bir eylem için CORS devre dışı bırakmak için ekleme **[DisableCors]** özniteliği eylem.</span><span class="sxs-lookup"><span data-stu-id="16041-213">To disable CORS for an action, add the **[DisableCors]** attribute to the action.</span></span> <span data-ttu-id="16041-214">Aşağıdaki örnek dışında her yöntemi için CORS etkinleştirir `PutItem`.</span><span class="sxs-lookup"><span data-stu-id="16041-214">The following example enables CORS for every method except `PutItem`.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

<span data-ttu-id="16041-215">**Genel**</span><span class="sxs-lookup"><span data-stu-id="16041-215">**Globally**</span></span>

<span data-ttu-id="16041-216">Geçişi, uygulamanızın Web API denetleyicilerinin tümü için CORS etkinleştirmek için bir **EnableCorsAttribute** için örnek **EnableCors** yöntemi:</span><span class="sxs-lookup"><span data-stu-id="16041-216">To enable CORS for all Web API controllers in your application, pass an **EnableCorsAttribute** instance to the **EnableCors** method:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

<span data-ttu-id="16041-217">Özniteliği birden fazla kapsamda olarak ayarlanmış, öncelik sırası şöyledir:</span><span class="sxs-lookup"><span data-stu-id="16041-217">If you set the attribute at more than one scope, the order of precedence is:</span></span>

1. <span data-ttu-id="16041-218">Eylem</span><span class="sxs-lookup"><span data-stu-id="16041-218">Action</span></span>
2. <span data-ttu-id="16041-219">Denetleyici</span><span class="sxs-lookup"><span data-stu-id="16041-219">Controller</span></span>
3. <span data-ttu-id="16041-220">Global</span><span class="sxs-lookup"><span data-stu-id="16041-220">Global</span></span>

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a><span data-ttu-id="16041-221">İzin verilen çıkış noktası ayarlama</span><span class="sxs-lookup"><span data-stu-id="16041-221">Set the Allowed Origins</span></span>

<span data-ttu-id="16041-222">*Çıkış* parametresinin **[EnableCors]** özniteliği belirtir kaynağa erişmek için hangi çıkış noktaları kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="16041-222">The *origins* parameter of the **[EnableCors]** attribute specifies which origins are allowed to access the resource.</span></span> <span data-ttu-id="16041-223">Değer izin verilen çıkış noktası, virgülle ayrılmış bir listesidir.</span><span class="sxs-lookup"><span data-stu-id="16041-223">The value is a comma-separated list of the allowed origins.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

<span data-ttu-id="16041-224">Joker karakter değer de kullanabilirsiniz "\*" tüm kaynakları gelen isteklere izin vermek için.</span><span class="sxs-lookup"><span data-stu-id="16041-224">You can also use the wildcard value "\*" to allow requests from any origins.</span></span>

<span data-ttu-id="16041-225">Her türlü kaynağa gelen isteklere izin vermeden önce dikkatlice düşünün.</span><span class="sxs-lookup"><span data-stu-id="16041-225">Consider carefully before allowing requests from any origin.</span></span> <span data-ttu-id="16041-226">Bu, gerçek anlamda bir Web sitesi web API AJAX çağrıları yapabilirsiniz anlamına gelir.</span><span class="sxs-lookup"><span data-stu-id="16041-226">It means that literally any website can make AJAX calls to your web API.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a><span data-ttu-id="16041-227">İzin verilen HTTP yöntemleri Ayarla</span><span class="sxs-lookup"><span data-stu-id="16041-227">Set the Allowed HTTP Methods</span></span>

<span data-ttu-id="16041-228">*Yöntemleri* parametresinin **[EnableCors]** özniteliği, kaynağa erişmek için hangi HTTP yöntemlerine izin belirtir.</span><span class="sxs-lookup"><span data-stu-id="16041-228">The *methods* parameter of the **[EnableCors]** attribute specifies which HTTP methods are allowed to access the resource.</span></span> <span data-ttu-id="16041-229">Tüm yöntemlere izin vermek için joker karakter değeri kullanmak "\*".</span><span class="sxs-lookup"><span data-stu-id="16041-229">To allow all methods, use the wildcard value "\*".</span></span> <span data-ttu-id="16041-230">Aşağıdaki örnekte, yalnızca GET ve POST isteklere izin verir.</span><span class="sxs-lookup"><span data-stu-id="16041-230">The following example allows only GET and POST requests.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a><span data-ttu-id="16041-231">İzin verilen istek üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="16041-231">Set the Allowed Request Headers</span></span>

<span data-ttu-id="16041-232">Daha önce t bir denetim öncesi isteği bir Access-Control-Request-Headers üstbilgisi nasıl içerebilir uygulama tarafından ayarlanıp HTTP üstbilgileri listeleme açıklanan (sözde "yazar, istek üstbilgileri").</span><span class="sxs-lookup"><span data-stu-id="16041-232">Earlier I described how a preflight request might include an Access-Control-Request-Headers header, listing the HTTP headers set by the application (the so-called "author request headers").</span></span> <span data-ttu-id="16041-233">*Üstbilgileri* parametresinin **[EnableCors]** özniteliği belirtir hangi yazar İstek üstbilgilerinin izin verilir.</span><span class="sxs-lookup"><span data-stu-id="16041-233">The *headers* parameter of the **[EnableCors]** attribute specifies which author request headers are allowed.</span></span> <span data-ttu-id="16041-234">Tüm üstbilgileri izin verecek şekilde ayarlamanız *üstbilgileri* için "\*".</span><span class="sxs-lookup"><span data-stu-id="16041-234">To allow any headers, set *headers* to "\*".</span></span> <span data-ttu-id="16041-235">Beyaz liste belirli üst bilgileri için Ayarla *üstbilgileri* izin verilen üstbilgileri virgülle ayrılmış bir listesi için:</span><span class="sxs-lookup"><span data-stu-id="16041-235">To whitelist specific headers, set *headers* to a comma-separated list of the allowed headers:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

<span data-ttu-id="16041-236">Ancak, tarayıcılar nasıl bunlar Access-Control-Request-Headers kümesinde tamamen tutarlı değil.</span><span class="sxs-lookup"><span data-stu-id="16041-236">However, browsers are not entirely consistent in how they set Access-Control-Request-Headers.</span></span> <span data-ttu-id="16041-237">Örneğin, Chrome "kaynak"; şu anda içerir bile uygulama bunları betikte ayarladığında FireFox "Kabul et" gibi standart başlıklarını içermez yapılırken.</span><span class="sxs-lookup"><span data-stu-id="16041-237">For example, Chrome currently includes "origin"; while FireFox does not include standard headers such as "Accept", even when the application sets them in script.</span></span>

<span data-ttu-id="16041-238">Ayarlarsanız *üstbilgileri* dışında herhangi bir şeye "\*", "kabul", "content-type" ve "kaynak" artı desteklemek istediğiniz tüm özel üstbilgileri en az içermelidir.</span><span class="sxs-lookup"><span data-stu-id="16041-238">If you set *headers* to anything other than "\*", you should include at least "accept", "content-type", and "origin", plus any custom headers that you want to support.</span></span>

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a><span data-ttu-id="16041-239">İzin verilen yanıt üstbilgilerini Ayarla</span><span class="sxs-lookup"><span data-stu-id="16041-239">Set the Allowed Response Headers</span></span>

<span data-ttu-id="16041-240">Varsayılan olarak, tarayıcı tüm uygulama ve yanıt üst bilgilerini kullanıma sunmuyor.</span><span class="sxs-lookup"><span data-stu-id="16041-240">By default, the browser does not expose all of the response headers to the application.</span></span> <span data-ttu-id="16041-241">Varsayılan olarak kullanılabilir yanıt üstbilgilerini şunlardır:</span><span class="sxs-lookup"><span data-stu-id="16041-241">The response headers that are available by default are:</span></span>

- <span data-ttu-id="16041-242">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="16041-242">Cache-Control</span></span>
- <span data-ttu-id="16041-243">İçerik dili</span><span class="sxs-lookup"><span data-stu-id="16041-243">Content-Language</span></span>
- <span data-ttu-id="16041-244">İçerik Türü</span><span class="sxs-lookup"><span data-stu-id="16041-244">Content-Type</span></span>
- <span data-ttu-id="16041-245">Süre sonu</span><span class="sxs-lookup"><span data-stu-id="16041-245">Expires</span></span>
- <span data-ttu-id="16041-246">Son değiştiren</span><span class="sxs-lookup"><span data-stu-id="16041-246">Last-Modified</span></span>
- <span data-ttu-id="16041-247">Pragma</span><span class="sxs-lookup"><span data-stu-id="16041-247">Pragma</span></span>

<span data-ttu-id="16041-248">CORS spec bunlar çağırır [basit yanıt üstbilgilerini](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span><span class="sxs-lookup"><span data-stu-id="16041-248">The CORS spec calls these [simple response headers](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header).</span></span> <span data-ttu-id="16041-249">Diğer üstbilgileri uygulama kullanılabilir hale getirmek için ayarlanmış *exposedHeaders* parametresinin **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="16041-249">To make other headers available to the application, set the *exposedHeaders* parameter of **[EnableCors]**.</span></span>

<span data-ttu-id="16041-250">Aşağıdaki örnekte, denetleyici 's `Get` yöntemi 'X-özel-üstbilgi' adlı özel bir üstbilgi ayarlar.</span><span class="sxs-lookup"><span data-stu-id="16041-250">In the following example, the controller's `Get` method sets a custom header named ‘X-Custom-Header'.</span></span> <span data-ttu-id="16041-251">Varsayılan olarak, tarayıcı bu çıkış noktaları arası istek üstbilgisinde kullanıma değil.</span><span class="sxs-lookup"><span data-stu-id="16041-251">By default, the browser will not expose this header in a cross-origin request.</span></span> <span data-ttu-id="16041-252">Üstbilgi kullanılabilir hale getirmek 'X-özel-üstbilgi' dahil *exposedHeaders*.</span><span class="sxs-lookup"><span data-stu-id="16041-252">To make the header available, include ‘X-Custom-Header' in *exposedHeaders*.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a><span data-ttu-id="16041-253">Cross-Origin istekleri geçirme kimlik bilgileri</span><span class="sxs-lookup"><span data-stu-id="16041-253">Passing Credentials in Cross-Origin Requests</span></span>

<span data-ttu-id="16041-254">Kimlik bilgileri bir CORS istek özel işleme gerektirir.</span><span class="sxs-lookup"><span data-stu-id="16041-254">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="16041-255">Varsayılan olarak, tarayıcı çıkış noktaları arası istek ile herhangi bir kimlik bilgisi göndermez.</span><span class="sxs-lookup"><span data-stu-id="16041-255">By default, the browser does not send any credentials with a cross-origin request.</span></span> <span data-ttu-id="16041-256">Kimlik bilgileri, tanımlama bilgilerinin yanı sıra HTTP kimlik doğrulama şemasını içerir.</span><span class="sxs-lookup"><span data-stu-id="16041-256">Credentials include cookies as well as HTTP authentication schemes.</span></span> <span data-ttu-id="16041-257">Kimlik bilgileri ile bir çıkış noktaları arası istek göndermek için istemci ayarlamalısınız **XMLHttpRequest.withCredentials** true.</span><span class="sxs-lookup"><span data-stu-id="16041-257">To send credentials with a cross-origin request, the client must set **XMLHttpRequest.withCredentials** to true.</span></span>

<span data-ttu-id="16041-258">Kullanarak **XMLHttpRequest** doğrudan:</span><span class="sxs-lookup"><span data-stu-id="16041-258">Using **XMLHttpRequest** directly:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

<span data-ttu-id="16041-259">JQuery içinde:</span><span class="sxs-lookup"><span data-stu-id="16041-259">In jQuery:</span></span>

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

<span data-ttu-id="16041-260">Ayrıca, sunucu kimlik bilgilerine izin vermeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="16041-260">In addition, the server must allow the credentials.</span></span> <span data-ttu-id="16041-261">Çıkış noktaları arası kimlik bilgileri Web API'si izin verecek şekilde ayarlamanız **SupportsCredentials** özelliğini true olarak **[EnableCors]** özniteliği:</span><span class="sxs-lookup"><span data-stu-id="16041-261">To allow cross-origin credentials in Web API, set the **SupportsCredentials** property to true on the **[EnableCors]** attribute:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

<span data-ttu-id="16041-262">Bu özellik true ise, HTTP yanıtı bir erişim-denetim-Allow-Credentials üstbilgisi içerir.</span><span class="sxs-lookup"><span data-stu-id="16041-262">If this property is true, the HTTP response will include an Access-Control-Allow-Credentials header.</span></span> <span data-ttu-id="16041-263">Bu üst sunucu çıkış noktaları arası istek için kimlik bilgilerini sağlayan tarayıcı söyler.</span><span class="sxs-lookup"><span data-stu-id="16041-263">This header tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="16041-264">Kimlik bilgilerini tarayıcı gönderir, ancak yanıt geçerli erişim-denetim-Allow-Credentials üst bilgi içermeyen, tarayıcı uygulaması yanıtı kullanıma değil ve AJAX isteği başarısız olur.</span><span class="sxs-lookup"><span data-stu-id="16041-264">If the browser sends credentials, but the response does not include a valid Access-Control-Allow-Credentials header, the browser will not expose the response to the application, and the AJAX request fails.</span></span>

<span data-ttu-id="16041-265">Ayar hakkında çok dikkatli olun **SupportsCredentials** başka bir etki alanındaki bir Web sitesi gönderebilir oturum açmış kullanıcının kimlik bilgileri Web apı'nize kullanıcı adınıza farkına kullanıcı gösterdiğinden, true olarak.</span><span class="sxs-lookup"><span data-stu-id="16041-265">Be very careful about setting **SupportsCredentials** to true, because it means a website at another domain can send a logged-in user's credentials to your Web API on the user's behalf, without the user being aware.</span></span> <span data-ttu-id="16041-266">CORS spec de bu ayar durumları *çıkış* için &quot; \* &quot; geçersiz durumunda **SupportsCredentials** doğrudur.</span><span class="sxs-lookup"><span data-stu-id="16041-266">The CORS spec also states that setting *origins* to &quot;\*&quot; is invalid if **SupportsCredentials** is true.</span></span>

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a><span data-ttu-id="16041-267">Özel CORS İlkesi sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="16041-267">Custom CORS Policy Providers</span></span>

<span data-ttu-id="16041-268">**[EnableCors]** özniteliği uygulayan **ICorsPolicyProvider** arabirimi.</span><span class="sxs-lookup"><span data-stu-id="16041-268">The **[EnableCors]** attribute implements the **ICorsPolicyProvider** interface.</span></span> <span data-ttu-id="16041-269">Türeyen bir sınıf oluşturarak, kendi uygulama sağlayabilir **özniteliği** ve uygulayan **ICorsProlicyProvider**.</span><span class="sxs-lookup"><span data-stu-id="16041-269">You can provide your own implementation by creating a class that derives from **Attribute** and implements **ICorsProlicyProvider**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

<span data-ttu-id="16041-270">Herhangi bir yerleştirme, koyabilirsiniz özniteliği uygulayabilirsiniz artık **[EnableCors]**.</span><span class="sxs-lookup"><span data-stu-id="16041-270">Now you can apply the attribute any place that you would put **[EnableCors]**.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

<span data-ttu-id="16041-271">Örneğin, bir özel CORS ilke sağlayıcısı ayarları yapılandırma dosyasından okunur.</span><span class="sxs-lookup"><span data-stu-id="16041-271">For example, a custom CORS policy provider could read the settings from a configuration file.</span></span>

<span data-ttu-id="16041-272">Öznitelikleri kullanarak alternatif olarak, kaydettiğiniz bir **ICorsPolicyProviderFactory** oluşturur nesne **ICorsPolicyProvider** nesneleri.</span><span class="sxs-lookup"><span data-stu-id="16041-272">As an alternative to using attributes, you can register an **ICorsPolicyProviderFactory** object that creates **ICorsPolicyProvider** objects.</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

<span data-ttu-id="16041-273">Ayarlamak için **ICorsPolicyProviderFactory**, çağrı **SetCorsPolicyProviderFactory** şekilde başlangıçta genişletme yöntemi:</span><span class="sxs-lookup"><span data-stu-id="16041-273">To set the **ICorsPolicyProviderFactory**, call the **SetCorsPolicyProviderFactory** extension method at startup, as follows:</span></span>

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a><span data-ttu-id="16041-274">Tarayıcı desteği</span><span class="sxs-lookup"><span data-stu-id="16041-274">Browser Support</span></span>

<span data-ttu-id="16041-275">Web API CORS, bir sunucu tarafı teknoloji paketidir.</span><span class="sxs-lookup"><span data-stu-id="16041-275">The Web API CORS package is a server-side technology.</span></span> <span data-ttu-id="16041-276">Ayrıca kullanıcının tarayıcısına CORS desteklemesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="16041-276">The user's browser also needs to support CORS.</span></span> <span data-ttu-id="16041-277">Neyse ki, önde gelen tüm tarayıcılar geçerli sürümleri dahildir [desteklemek için CORS](http://caniuse.com/cors).</span><span class="sxs-lookup"><span data-stu-id="16041-277">Fortunately, the current versions of all major browsers include [support for CORS](http://caniuse.com/cors).</span></span>

<span data-ttu-id="16041-278">Internet Explorer 8 ve Internet Explorer 9 XMLHttpRequest yerine eski XDomainRequest nesnesini kullanarak CORS, kısmi desteği vardır.</span><span class="sxs-lookup"><span data-stu-id="16041-278">Internet Explorer 8 and Internet Explorer 9 have partial support for CORS, using the legacy XDomainRequest object instead of XMLHttpRequest.</span></span> <span data-ttu-id="16041-279">Daha fazla bilgi için bkz: [XDomainRequest - kısıtlamaları, sınırlamalar ve geçici çözümler](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span><span class="sxs-lookup"><span data-stu-id="16041-279">For more information, see [XDomainRequest - Restrictions, Limitations and Workarounds](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).</span></span>
