---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: ASP.NET'te yapılmaması gerekenler ve bunların yerine yapılması gerekenler | Microsoft Docs
author: tfitzmac
description: Bu konuda, ASP.NET web projeleri içinde kişi olun birkaç yaygın hataları açıklanır. Bu, bu or önlemek için ne yapmanız gerektiğini yönelik öneriler sağlar...
ms.author: riande
ms.date: 05/08/2014
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 7f2366ef8cb258a08c3cb96407f605864e5ec9a9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752971"
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="2da6c-104">ASP.NET'te yapılmaması gerekenler ve bunların yerine yapılması gerekenler</span><span class="sxs-lookup"><span data-stu-id="2da6c-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="2da6c-105">tarafından [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="2da6c-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="2da6c-106">Bu konuda, ASP.NET web projeleri içinde kişi olun birkaç yaygın hataları açıklanır.</span><span class="sxs-lookup"><span data-stu-id="2da6c-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="2da6c-107">Bu, bu yaygın hataları önlemek için ne yapmanız gerektiğini yönelik öneriler sağlar.</span><span class="sxs-lookup"><span data-stu-id="2da6c-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="2da6c-108">Bağlı olduğu bir [sunu](http://vimeo.com/68390507) tarafından **Damian Edwards** Norveç Geliştiriciler Konferansı'konumunda.</span><span class="sxs-lookup"><span data-stu-id="2da6c-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="2da6c-109">Sorumluluk reddi</span><span class="sxs-lookup"><span data-stu-id="2da6c-109">Disclaimer</span></span>

<span data-ttu-id="2da6c-110">Bu konu, güvenli ve verimli uygulama olduğundan emin olmak için tam bir kılavuz olarak tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="2da6c-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="2da6c-111">Yine de güvenlik ve performans için bu konudaki belirtilmeyen en iyi uygulamaları izlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="2da6c-112">Yalnızca, .NET sınıfları ve işlemleri ile ilgili yaygın hataları önlemek nasıl önerir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="2da6c-113">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="2da6c-113">Overview</span></span>

<span data-ttu-id="2da6c-114">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="2da6c-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="2da6c-115">Uyumluluk standartları</span><span class="sxs-lookup"><span data-stu-id="2da6c-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="2da6c-116">Denetim bağdaştırıcılarını</span><span class="sxs-lookup"><span data-stu-id="2da6c-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="2da6c-117">Stil özellikleri denetimlerinde</span><span class="sxs-lookup"><span data-stu-id="2da6c-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="2da6c-118">Sayfa ve denetim geri çağırmaları</span><span class="sxs-lookup"><span data-stu-id="2da6c-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="2da6c-119">Tarayıcı özelliği algılama</span><span class="sxs-lookup"><span data-stu-id="2da6c-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="2da6c-120">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="2da6c-120">Security</span></span>](#security)

    - [<span data-ttu-id="2da6c-121">İsteği doğrulama</span><span class="sxs-lookup"><span data-stu-id="2da6c-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="2da6c-122">Cookieless form kimlik doğrulaması ve oturum</span><span class="sxs-lookup"><span data-stu-id="2da6c-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="2da6c-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="2da6c-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="2da6c-124">Orta güven</span><span class="sxs-lookup"><span data-stu-id="2da6c-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="2da6c-125">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="2da6c-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="2da6c-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="2da6c-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="2da6c-127">Güvenilirlik ve performans</span><span class="sxs-lookup"><span data-stu-id="2da6c-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="2da6c-128">PreSendRequestHeaders ve PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="2da6c-128">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="2da6c-129">Web Forms ile zaman uyumsuz sayfası olayları</span><span class="sxs-lookup"><span data-stu-id="2da6c-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="2da6c-130">İş Başlat ve unut</span><span class="sxs-lookup"><span data-stu-id="2da6c-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="2da6c-131">İstek Varlık gövdesi</span><span class="sxs-lookup"><span data-stu-id="2da6c-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="2da6c-132">Response.Redirect ve Response.End</span><span class="sxs-lookup"><span data-stu-id="2da6c-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="2da6c-133">EnableViewState ve ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="2da6c-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="2da6c-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="2da6c-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="2da6c-135">Uzun çalışan istek (> 110 saniye)</span><span class="sxs-lookup"><span data-stu-id="2da6c-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="2da6c-136">Uyumluluk standartları</span><span class="sxs-lookup"><span data-stu-id="2da6c-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="2da6c-137">Denetim bağdaştırıcılarını</span><span class="sxs-lookup"><span data-stu-id="2da6c-137">Control Adapters</span></span>

<span data-ttu-id="2da6c-138">Öneri: Uyarlamalı işleme için Denetim bağdaştırıcılarını kullanma durdurun ve CSS medya sorgular ve standartlara uygun HTML kullanın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="2da6c-139">.NET 2. 0'ı farklı cihaz ve ortamları için özelleştirildiğinden sunu kod işlemek için denetimleri bağdaştırıcıları sunulur.</span><span class="sxs-lookup"><span data-stu-id="2da6c-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="2da6c-140">Şimdi, Uyarlamalı bu işleme, CSS ve HTML ile gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="2da6c-141">Denetim bağdaştırıcılarını kullanmayı bırakmak ve herhangi bir mevcut bağdaştırıcıları için CSS ve HTML dönüştürme gerekir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="2da6c-142">Daha fazla bilgi için [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) ve [nasıl yapılır: Mobil sayfalara ekleme bilgisayarınızı ASP.NET Web Forms / MVC uygulaması](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="2da6c-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="2da6c-143">Stil özellikleri denetimlerinde</span><span class="sxs-lookup"><span data-stu-id="2da6c-143">Style Properties on Controls</span></span>

<span data-ttu-id="2da6c-144">Öneri: denetim işaretlemede stil değerlerini ayarlama durdurun ve CSS stil sayfalarını biçimlendirme değerlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="2da6c-145">Satır içi stil özellikleri ayarlamak için kullanılan özellikler onlarca Web sunucusu denetimleri içerir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="2da6c-146">Örneğin, bir denetim için metin rengi ForeColor özelliği ayarlar.</span><span class="sxs-lookup"><span data-stu-id="2da6c-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="2da6c-147">CSS stil sayfaları ile daha verimli bir şekilde bu aynı etkiyi gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2da6c-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="2da6c-148">Stil sayfaları, stil değer merkezileştirebilir ve uygulamanızda bu değerleri ayarlamaktan kaçının sağlar.</span><span class="sxs-lookup"><span data-stu-id="2da6c-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="2da6c-149">Aşağıdaki örnek, bir CSS sınıfı kırmızı kümeleri metni gösterir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="2da6c-150">Sonraki örnekte, CSS sınıfı dinamik olarak uygulanacak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="2da6c-151">Sayfa ve denetim geri çağırmaları</span><span class="sxs-lookup"><span data-stu-id="2da6c-151">Page and Control Callbacks</span></span>

<span data-ttu-id="2da6c-152">Öneri: sayfa ve denetim geri çağırmaları kullanmayı ve aşağıdakilerden birini kullanın: AJAX, UpdatePanel, MVC eylem yöntemleri, Web API'sini veya SignalR.</span><span class="sxs-lookup"><span data-stu-id="2da6c-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="2da6c-153">ASP.NET önceki sürümlerinde, geri çağırma yöntemleri sayfası ve denetim web sitesine ait bir sayfanın tamamını yenilemeden güncelleştirmek etkin.</span><span class="sxs-lookup"><span data-stu-id="2da6c-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="2da6c-154">Kısmi Sayfa güncelleştirmelerini aracılığıyla artık gerçekleştirebilirsiniz [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) veya [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="2da6c-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="2da6c-155">Kolay URL'lerle sorunlara neden olabilir çünkü geri çağırma yöntemleri kullanılarak ve yönlendirme durdurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="2da6c-156">Varsayılan olarak, denetimleri geri çağırma yöntemleri etkinleştirmeyin, ancak denetimi içinde bu özellik etkinleştirilirse, bunu devre dışı.</span><span class="sxs-lookup"><span data-stu-id="2da6c-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="2da6c-157">Tarayıcı özelliği algılama</span><span class="sxs-lookup"><span data-stu-id="2da6c-157">Browser Capability Detection</span></span>

<span data-ttu-id="2da6c-158">Öneri: statik tarayıcı özelliği algılama kullanarak durdurun ve bunun yerine dinamik özellik algılama kullanın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="2da6c-159">ASP.NET önceki sürümlerinde desteklenen özellikler her tarayıcıda bir XML dosyasında saklanır.</span><span class="sxs-lookup"><span data-stu-id="2da6c-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="2da6c-160">Statik bir arama yoluyla algılama özelliği desteği en iyi yaklaşım değildir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="2da6c-161">Şimdi, dinamik olarak bir desteklenen bir tarayıcıdan özellikleri gibi bir özellik algılama altyapısı kullanarak algılayabilir [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="2da6c-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="2da6c-162">Özellik algılama yöntemi veya özelliği kullanmak çalışıyor ve tarayıcı istenen sonucu üretilen varsa bkz denetleniyor destek belirler.</span><span class="sxs-lookup"><span data-stu-id="2da6c-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="2da6c-163">Varsayılan olarak, Web uygulaması şablonları Modernizr dahildir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="2da6c-164">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="2da6c-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="2da6c-165">İsteği doğrulama</span><span class="sxs-lookup"><span data-stu-id="2da6c-165">Request Validation</span></span>

<span data-ttu-id="2da6c-166">Öneri: kullanıcı girişini doğrulamak ve kullanıcıların çıkış kodlayın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="2da6c-167">İstek doğrulamanın, her isteği inceler ve algılanan bir tehdit bulunursa istek durduran ASP.NET özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="2da6c-168">Uygulamanızı siteler arası betik saldırılara karşı güvenli hale getirmek için istek doğrulamayı bağlı değil.</span><span class="sxs-lookup"><span data-stu-id="2da6c-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="2da6c-169">Bunun yerine, kullanıcılardan gelen tüm girişini doğrulamak ve çıkış kodlayın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="2da6c-170">Sınırlı bazı durumlarda, giriş doğrulamak için normal ifadeleri kullanabilirsiniz, ancak daha karmaşık durumlarda, doğrulamalıdır değerle eşleşiyorsa belirleyen .NET sınıflarını kullanarak kullanıcı girişi izin verilen değerler.</span><span class="sxs-lookup"><span data-stu-id="2da6c-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="2da6c-171">Aşağıdaki örnek, statik bir yöntem URI sınıfında bir kullanıcı tarafından sağlanan URI geçerli olup olmadığını belirlemek için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="2da6c-172">Ancak, URI yeterince doğrulamak için Ayrıca, belirttiğinden emin olun için denetleme yapmalıdır `http` veya `https`.</span><span class="sxs-lookup"><span data-stu-id="2da6c-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="2da6c-173">Aşağıdaki örnek, URI geçerli olduğunu doğrulamak için örnek yöntemler kullanır.</span><span class="sxs-lookup"><span data-stu-id="2da6c-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="2da6c-174">Kötü amaçlı kod dahil edilmez emin olmak için değerleri HTML olarak kullanıcı girişini işleme veya bir SQL sorgusunun kullanıcı girişi dahil olmak üzere önce kodlayın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="2da6c-175">HTML için biçimlendirme değer kodlama &lt;%: %&gt; söz dizimi, aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="2da6c-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="2da6c-176">Veya, HTML, Razor sözdizimini ile kodlama @, aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="2da6c-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="2da6c-177">Sonraki örnekte gösterildiği nasıl, arka plan kod içinde bir değer için HTML kodlayın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="2da6c-178">Güvenli bir şekilde SQL komutları için bir değer kodlamak için komut parametreleri gibi kullanın [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="2da6c-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="2da6c-179">Cookieless form kimlik doğrulaması ve oturum</span><span class="sxs-lookup"><span data-stu-id="2da6c-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="2da6c-180">Öneri: tanımlama bilgileri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="2da6c-181">Sorgu dizesinde kimlik doğrulama bilgilerini bu işleve geçirerek güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="2da6c-182">Uygulamanız kimlik doğrulaması içerdiğinde, bu nedenle, tanımlama bilgileri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="2da6c-183">Tanımlama bilgisi duyarlı bilgi içermiyorsa, tanımlama bilgisi için SSL kullanılmasını gerekli tutmayı dikkate alın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="2da6c-184">Aşağıdaki örnek, form kimlik doğrulaması, SSL üzerinden iletilen bir tanımlama bilgisi gerektiren Web.config dosyasında belirlemek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="2da6c-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="2da6c-185">EnableViewStateMac</span></span>

<span data-ttu-id="2da6c-186">Öneri: Hiçbir zaman false olarak ayarlanmış.</span><span class="sxs-lookup"><span data-stu-id="2da6c-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="2da6c-187">Varsayılan olarak, EnbableViewStateMac ayarlanır true.</span><span class="sxs-lookup"><span data-stu-id="2da6c-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="2da6c-188">Uygulama görünüm durumunu kullanmıyor olsa bile, EnableViewStateMac false olarak ayarlı değil.</span><span class="sxs-lookup"><span data-stu-id="2da6c-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="2da6c-189">Bu değerin false olarak ayarlanması, uygulamanızı siteler arası betik karşı savunmasız hale getirir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="2da6c-190">ASP.NET 4.5.2 ile başlayarak, çalışma zamanı zorlar **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="2da6c-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="2da6c-191">False olarak ayarlanmış olsa bile, çalışma zamanı bu değeri yoksayar ve true olarak değer kümesi ile devam eder.</span><span class="sxs-lookup"><span data-stu-id="2da6c-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="2da6c-192">Daha fazla bilgi için [ASP.NET 4.5.2 ve EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="2da6c-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="2da6c-193">Aşağıdaki örnek, EnableViewStateMac true olarak ayarlamak gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="2da6c-194">Aslında bu değer varsayılan olarak true olduğundan true olarak ayarlamak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="2da6c-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="2da6c-195">Ancak, bunu false olarak herhangi bir sayfa üzerinde uygulamanızda ayarladıysanız, bu değer hemen düzeltmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="2da6c-196">Orta güven</span><span class="sxs-lookup"><span data-stu-id="2da6c-196">Medium Trust</span></span>

<span data-ttu-id="2da6c-197">Öneri: Orta güven (veya herhangi bir güven düzeyi) bir güvenlik sınırı olarak güvenmeyin.</span><span class="sxs-lookup"><span data-stu-id="2da6c-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="2da6c-198">Kısmi güven yeterince uygulamanızı korumaz ve kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2da6c-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="2da6c-199">Bunun yerine tam güven ve ayrı uygulama havuzlarında güvenilmeyen uygulamaları ayırmak.</span><span class="sxs-lookup"><span data-stu-id="2da6c-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="2da6c-200">Ayrıca, her bir uygulama havuzunun benzersiz bir kimlik altında çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="2da6c-201">Daha fazla bilgi için [ASP.NET kısmi güven uygulama yalıtımı garantilemez](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="2da6c-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="2da6c-202">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="2da6c-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="2da6c-203">Öneri: güvenlik ayarlarını devre dışı bırakmayın &lt;appSettings&gt; öğesi.</span><span class="sxs-lookup"><span data-stu-id="2da6c-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="2da6c-204">AppSettings öğesi, güvenlik güncelleştirmeleri için gerekli olan birçok değerlerini içerir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="2da6c-205">Değil, değiştirmek veya bu değerleri devre dışı bırakmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-205">You should not change or disable these values.</span></span> <span data-ttu-id="2da6c-206">Bir güncelleştirme dağıtımı sırasında bu değerleri devre dışı bırakmanız gerekir, hemen dağıtımını tamamladıktan sonra yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="2da6c-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="2da6c-207">Ayrıntılar için bkz [ASP.NET appSettings öğesi](https://msdn.microsoft.com/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="2da6c-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="2da6c-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="2da6c-208">UrlPathEncode</span></span>

<span data-ttu-id="2da6c-209">Öneri: Kullanın [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) yerine.</span><span class="sxs-lookup"><span data-stu-id="2da6c-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="2da6c-210">UrlPathEncode yöntemi, bir çok özel tarayıcı uyumluluk sorunu çözmek için .NET Framework eklendi.</span><span class="sxs-lookup"><span data-stu-id="2da6c-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="2da6c-211">Bir URL yeterince kodlamaz ve uygulamanızı siteler arası komut dosyası oluşturmaya karşı koruma sağlamaz.</span><span class="sxs-lookup"><span data-stu-id="2da6c-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="2da6c-212">Bunu uygulamanızda hiçbir zaman kullanmalısınız.</span><span class="sxs-lookup"><span data-stu-id="2da6c-212">You should never use it in your application.</span></span> <span data-ttu-id="2da6c-213">Bunun yerine, [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="2da6c-213">Instead, use [UrlEncode](https://msdn.microsoft.com/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="2da6c-214">Aşağıdaki örnek, bir hyperlink denetimi için bir sorgu dizesi parametresi olarak kodlanmış bir URL geçirilecek gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="2da6c-215">Güvenilirlik ve performans</span><span class="sxs-lookup"><span data-stu-id="2da6c-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="2da6c-216">PreSendRequestHeaders ve PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="2da6c-216">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="2da6c-217">Öneri: Bu olayları ile yönetilen modülleri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="2da6c-218">Bunun yerine, gerekli görev gerçekleştirmek için yerel bir IIS modül yazın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="2da6c-219">Bkz: [yerel kodlu HTTP modülleri oluşturma](https://msdn.microsoft.com/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="2da6c-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/library/ms693629.aspx).</span></span>

<span data-ttu-id="2da6c-220">Kullanabileceğiniz [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) ve [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) yerel IIS modülleri ile olayları.</span><span class="sxs-lookup"><span data-stu-id="2da6c-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="2da6c-221">Kullanmayın `PreSendRequestHeaders` ve `PreSendRequestContent` uygulayan yönetilen modülleri ile `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="2da6c-221">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="2da6c-222">Bu özellikleri ayarlama ile zaman uyumsuz istekler sorunlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-222">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="2da6c-223">Uygulama istenen yönlendirme (ARR) ve websockets birleşimi w3wp kilitlenmesine neden olabilecek erişim ihlali özel durumlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-223">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="2da6c-224">Örneğin, iiscore! Bir erişim ihlali özel durumu (0xC0000005) W3_CONTEXT_BASE::GetIsLastNotification + iiscore.dll, 68 oldu.</span><span class="sxs-lookup"><span data-stu-id="2da6c-224">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="2da6c-225">Web Forms ile zaman uyumsuz sayfası olayları</span><span class="sxs-lookup"><span data-stu-id="2da6c-225">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="2da6c-226">Öneri: Sayfa yaşam döngüsü olayları için void metotları zaman uyumsuz yazma Web formlarında önlemek ve bunun yerine kullanın [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) zaman uyumsuz kod için.</span><span class="sxs-lookup"><span data-stu-id="2da6c-226">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="2da6c-227">Bir sayfa olay ile işaretlediğinizde **zaman uyumsuz** ve **void**, zaman uyumsuz kodun bittiği olmadığını belirleyemez.</span><span class="sxs-lookup"><span data-stu-id="2da6c-227">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="2da6c-228">Bunun yerine, Page.RegisterAsyncTask tamamlanmasını izlemek sağlayan bir şekilde zaman uyumsuz kodu çalıştırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-228">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="2da6c-229">Aşağıdaki örnekte gösterildiği bir düğme içeren zaman uyumsuz kod işleyicisi'a tıklayın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-229">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="2da6c-230">Bu örnek bir dize değeri zaman uyumsuz olarak okuma önerilen uygulama değil de, yalnızca bir Basitleştirilmiş zaman uyumsuz bir görev örneği olarak sağlanan içerir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-230">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="2da6c-231">Zaman uyumsuz görevler kullanıyorsanız, Http Çalışma zamanı hedef Framework'ü Web.config dosyasında 4.5 olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-231">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="2da6c-232">Hedef framework 4.5 kapatır için yeni eşitleme kapsamının üzerinde ayarı .NET 4.5 eklendi.</span><span class="sxs-lookup"><span data-stu-id="2da6c-232">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="2da6c-233">Bu değer yeni projeler Visual Studio 2012'de varsayılan olarak ayarlanmış, ancak mevcut bir projeyi ile çalışıyorsanız ayarlanmış olması değil.</span><span class="sxs-lookup"><span data-stu-id="2da6c-233">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="2da6c-234">İş Başlat ve unut</span><span class="sxs-lookup"><span data-stu-id="2da6c-234">Fire-and-Forget Work</span></span>

<span data-ttu-id="2da6c-235">Öneri:, (böyle ThreadPool.QueueUserWorkItem yöntemi çağrılırken veya sürekli bir temsilci çağıran bir zamanlayıcı oluşturma) iş Başlat ve unut başlatma ASP.NET içinde bir isteği işlerken kaçının.</span><span class="sxs-lookup"><span data-stu-id="2da6c-235">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="2da6c-236">Uygulamanızın içinde ASP.NET çalışan Başlat ve unut iş varsa, uygulamanızı eşitlenmemiş alabilirsiniz. Herhangi bir zamanda uygulamanın geçerli durumu, devam eden işlemini artık eşleşmiyor olabilir yani uygulama etki alanı edilebilir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-236">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="2da6c-237">Bu tür iş ASP.NET dışında taşımanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-237">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="2da6c-238">Web işleri, Windows hizmeti veya çalışan rolü, Azure'da devam eden çalışmayı gerçekleştirmek için kullanın ve kodun başka bir işlemden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-238">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="2da6c-239">ASP.NET içine bu iş, gerçekleştirmeniz gerekirse adlı Nuget paketi ekleyebilirsiniz [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) kodu çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="2da6c-239">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="2da6c-240">İstek Varlık gövdesi</span><span class="sxs-lookup"><span data-stu-id="2da6c-240">Request Entity Body</span></span>

<span data-ttu-id="2da6c-241">Öneri: olay işleyicinin yürütmeden önce Request.Form veya Request.InputStream okuma kaçının.</span><span class="sxs-lookup"><span data-stu-id="2da6c-241">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="2da6c-242">Erken Request.Form veya Request.InputStream okumalıdır olan işleyicinin sırasında yürütme olay.</span><span class="sxs-lookup"><span data-stu-id="2da6c-242">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="2da6c-243">Mvc'de denetleyicisi işleyici ve eylem yöntemi çalıştığında yürütme etkinliğidir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-243">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="2da6c-244">Web Forms, işleyici sayfasıdır ve Page.Init olay oluşturulduğunda yürütme etkinliğidir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-244">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="2da6c-245">Yürütme Olay'den önceki istek Varlık gövdesi okuma isteğin işlenmesi müdahale.</span><span class="sxs-lookup"><span data-stu-id="2da6c-245">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="2da6c-246">Yürütme olayından önce istek Varlık gövdesi okuma gerekiyorsa kullanın [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) veya [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="2da6c-246">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="2da6c-247">GetBufferlessInputStream kullandığınızda, ham akışı istekten alma ve tüm istek işlemeye sorumluluğunu.</span><span class="sxs-lookup"><span data-stu-id="2da6c-247">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="2da6c-248">Bunlar ASP.NET tarafından doldurulduğunu değil çünkü GetBufferlessInputStream çağrıldıktan sonra Request.Form ve Request.InputStream kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="2da6c-248">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="2da6c-249">GetBufferedInputStream kullandığınızda, istekten akışın bir kopyasını alın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-249">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="2da6c-250">ASP.NET dolduran başka bir kopyası olduğundan Request.Form ve Request.InputStream isteği daha sonra yine kullanılabilir durumdadır.</span><span class="sxs-lookup"><span data-stu-id="2da6c-250">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="2da6c-251">Response.Redirect ve Response.End</span><span class="sxs-lookup"><span data-stu-id="2da6c-251">Response.Redirect and Response.End</span></span>

<span data-ttu-id="2da6c-252">Öneri: iş parçacığı çağırdıktan sonra nasıl işlendiğini fark dikkat [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="2da6c-252">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="2da6c-253">[Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) yöntemi Response.End yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="2da6c-253">The [Response.Redirect(String)](https://msdn.microsoft.com/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="2da6c-254">Bir zaman uyumlu işlemde Request.Redirect çağırma hemen iptal etmek geçerli iş parçacığının neden olur.</span><span class="sxs-lookup"><span data-stu-id="2da6c-254">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="2da6c-255">İstek için kod yürütme devam eder ancak zaman uyumsuz bir işlemde Response.Redirect çağırma geçerli iş parçacığını iptal değil.</span><span class="sxs-lookup"><span data-stu-id="2da6c-255">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="2da6c-256">Zaman uyumsuz bir işlemde görevi kod yürütmeyi durdurmak için yöntemden döndürmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-256">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="2da6c-257">Bir MVC projesinde, Response.Redirect çağırmamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-257">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="2da6c-258">Bunun yerine, bir RedirectResult döndürür.</span><span class="sxs-lookup"><span data-stu-id="2da6c-258">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="2da6c-259">EnableViewState ve ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="2da6c-259">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="2da6c-260">Öneri: Görünüm durumu üzerinde denetimleri kullanma ayrıntılı bir denetim sağlamak için EnableViewState yerine kullanım ViewStateMode.</span><span class="sxs-lookup"><span data-stu-id="2da6c-260">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="2da6c-261">Yanlış sayfa yönergesinde EnableViewState ayarladığınızda, Görünüm durumu sayfa içindeki tüm denetimler için devre dışıdır ve etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="2da6c-261">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="2da6c-262">Görünüm durumu sayfanızın belirli denetimler için etkinleştirmek istiyorsanız, ViewStateMode sayfa için devre dışı olarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-262">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="2da6c-263">Ardından, ViewStateMode görünüm durumu gerektiren denetimlere etkin olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-263">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="2da6c-264">Yalnızca gerekli denetimleri için görünüm durumuna etkinleştirerek, web sayfaları için Görünüm durumu boyutunu küçültebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2da6c-264">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="2da6c-265">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="2da6c-265">SqlMembershipProvider</span></span>

<span data-ttu-id="2da6c-266">Öneri: Evrensel sağlayıcıları kullanın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-266">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="2da6c-267">Geçerli proje şablonlarında SqlMembershipProvider ile değiştirilmiştir [ASP.NET Evrensel sağlayıcıları](http://www.nuget.org/packages/Microsoft.AspNet.Providers), olduğu bir NuGet paketi olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-267">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="2da6c-268">SqlMembershipProvider şablonları önceki bir sürümüyle oluşturulmuş bir projede kullanıyorsanız, Evrensel sağlayıcıları geçer.</span><span class="sxs-lookup"><span data-stu-id="2da6c-268">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="2da6c-269">Evrensel sağlayıcıları, Entity Framework tarafından desteklenen tüm veritabanları ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="2da6c-269">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="2da6c-270">Daha fazla bilgi için [Karşınızda ASP.NET Evrensel sağlayıcıları](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="2da6c-270">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="2da6c-271">Uzun süren istekleri (> 110 saniye)</span><span class="sxs-lookup"><span data-stu-id="2da6c-271">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="2da6c-272">Öneri: Kullanın [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) veya [SignalR](../../../signalr/index.md) bağlı istemcileri ve zaman uyumsuz g/ç işlemleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-272">Recommendation: Use [WebSockets](https://msdn.microsoft.com/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="2da6c-273">Uzun süreli istekler web uygulamanızda öngörülemeyen sonuçlara ve performansın düşmesine neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-273">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="2da6c-274">Bir istek için varsayılan zaman aşımı ayarını 110 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-274">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="2da6c-275">ASP.NET oturum durumu uzun süre çalışan istekle kullanıyorsanız, 110 saniye sonra oturum nesnesi üzerindeki kilidi serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="2da6c-275">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="2da6c-276">Ancak, uygulamanız oturum nesnesinde bir işlem ortasında kilidi serbest bırakılır ve işlem başarıyla tamamlanmayabilir olabilir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-276">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="2da6c-277">İlk istek çalışırken kullanıcı ikinci bir isteği engellenirse, ikinci isteği tutarsız bir duruma oturum nesnesinde erişebilir.</span><span class="sxs-lookup"><span data-stu-id="2da6c-277">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="2da6c-278">Uygulamanızın g/ç işlemleri engelleme (veya zaman uyumlu) içeriyorsa, uygulamanın yanıt veremez olacaktır.</span><span class="sxs-lookup"><span data-stu-id="2da6c-278">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="2da6c-279">Performansı artırmak için .NET Framework zaman uyumsuz g/ç işlemleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-279">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="2da6c-280">Ayrıca, WebSockets veya SignalR sunucuya bağlanan istemciler için kullanın.</span><span class="sxs-lookup"><span data-stu-id="2da6c-280">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="2da6c-281">Bu özellikler, uzun süre çalışan istekleri verimli bir şekilde işlemek üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="2da6c-281">These features are designed to efficiently handle long-running requests.</span></span>
