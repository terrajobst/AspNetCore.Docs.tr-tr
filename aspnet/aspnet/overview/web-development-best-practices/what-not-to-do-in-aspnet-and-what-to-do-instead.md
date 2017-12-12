---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: "ASP.NET yapmanız gerekenler ve bunun yerine yapmanız gerekenler | Microsoft Docs"
author: tfitzmac
description: "Bu konuda kişiler ASP.NET web projeleri içinde birçok ortak hataları açıklanmaktadır. Bu commo önlemek için ne yapması gerektiğini yönelik öneriler sağlar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 24c6a35a6b663ebb0f8d0e3e7988322fa5d9018c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="3869b-104">ASP.NET yapmanız gerekenler ve bunun yerine yapmanız gerekenler</span><span class="sxs-lookup"><span data-stu-id="3869b-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="3869b-105">tarafından [zel FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3869b-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3869b-106">Bu konuda kişiler ASP.NET web projeleri içinde birçok ortak hataları açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="3869b-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="3869b-107">Bu ortak hataları önlemek için ne yapması gerektiğini yönelik öneriler sağlar.</span><span class="sxs-lookup"><span data-stu-id="3869b-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="3869b-108">Bağlı olduğu bir [sunu](http://vimeo.com/68390507) tarafından **Damian Edirneli** Norveççe geliştiriciler konferansında.</span><span class="sxs-lookup"><span data-stu-id="3869b-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="3869b-109">Sorumluluk reddi</span><span class="sxs-lookup"><span data-stu-id="3869b-109">Disclaimer</span></span>

<span data-ttu-id="3869b-110">Bu konu, uygulamanızı güvenli ve etkin olduğundan emin olmak için tam bir kılavuz tasarlanmamıştır.</span><span class="sxs-lookup"><span data-stu-id="3869b-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="3869b-111">Hala güvenlik ve performans için bu konudaki belirtilmeyen en iyi uygulamaları izlemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="3869b-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="3869b-112">Yalnızca .NET sınıfları ve işlemleri ile ilgili sık karşılaşılan hatalardan kaçınmanızı nasıl olabileceğini gösteriyor.</span><span class="sxs-lookup"><span data-stu-id="3869b-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="3869b-113">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="3869b-113">Overview</span></span>

<span data-ttu-id="3869b-114">Bu konu aşağıdaki bölümleri içermektedir:</span><span class="sxs-lookup"><span data-stu-id="3869b-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="3869b-115">Standartları uyumluluğu</span><span class="sxs-lookup"><span data-stu-id="3869b-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="3869b-116">Denetim bağdaştırıcıları</span><span class="sxs-lookup"><span data-stu-id="3869b-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="3869b-117">Stil özellikleri denetimlerinde</span><span class="sxs-lookup"><span data-stu-id="3869b-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="3869b-118">Sayfa ve denetim geri aramalar</span><span class="sxs-lookup"><span data-stu-id="3869b-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="3869b-119">Tarayıcı yetenek algılama</span><span class="sxs-lookup"><span data-stu-id="3869b-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="3869b-120">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="3869b-120">Security</span></span>](#security)

    - [<span data-ttu-id="3869b-121">İstek doğrulama</span><span class="sxs-lookup"><span data-stu-id="3869b-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="3869b-122">Cookieless form kimlik doğrulaması ve oturum</span><span class="sxs-lookup"><span data-stu-id="3869b-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="3869b-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="3869b-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="3869b-124">Orta güven</span><span class="sxs-lookup"><span data-stu-id="3869b-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="3869b-125">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="3869b-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="3869b-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="3869b-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="3869b-127">Güvenilirlik ve performans</span><span class="sxs-lookup"><span data-stu-id="3869b-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="3869b-128">PreSendRequestHeaders ve PreSendRequestContext</span><span class="sxs-lookup"><span data-stu-id="3869b-128">PreSendRequestHeaders and PreSendRequestContext</span></span>](#presend)
    - [<span data-ttu-id="3869b-129">Web Forms ile zaman uyumsuz sayfası olayları</span><span class="sxs-lookup"><span data-stu-id="3869b-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="3869b-130">Yangın ve unut çalışma</span><span class="sxs-lookup"><span data-stu-id="3869b-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="3869b-131">İstek Varlık gövdesi</span><span class="sxs-lookup"><span data-stu-id="3869b-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="3869b-132">Response.Redirect ve Response.End</span><span class="sxs-lookup"><span data-stu-id="3869b-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="3869b-133">EnableViewState ve ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="3869b-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="3869b-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="3869b-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="3869b-135">Uzun çalışan istekler (> 110 saniye)</span><span class="sxs-lookup"><span data-stu-id="3869b-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="3869b-136">Standartları uyumluluğu</span><span class="sxs-lookup"><span data-stu-id="3869b-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="3869b-137">Denetim bağdaştırıcıları</span><span class="sxs-lookup"><span data-stu-id="3869b-137">Control Adapters</span></span>

<span data-ttu-id="3869b-138">Öneri: denetim bağdaştırıcıları Uyarlamalı işleme için kullanmayı ve CSS medya sorgular ve standartlarıyla uyumlu HTML yerine kullanın.</span><span class="sxs-lookup"><span data-stu-id="3869b-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="3869b-139">Denetimleri bağdaştırıcıları farklı cihaz ve ortamlar için özelleştirilmiş sunu kodu oluşturmak için .NET 2.0 tanıtıldı.</span><span class="sxs-lookup"><span data-stu-id="3869b-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="3869b-140">Şimdi, Uyarlamalı bu işleme HTML ve CSS ile gerçekleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="3869b-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="3869b-141">Denetim bağdaştırıcıları kullanmayı ve varolan tüm bağdaştırıcıları CSS ve HTML dönüştürün.</span><span class="sxs-lookup"><span data-stu-id="3869b-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="3869b-142">Daha fazla bilgi için bkz: [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) ve [nasıl yapılır: Mobil sayfalara eklemek bilgisayarınızı ASP.NET Web Forms / MVC uygulaması](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="3869b-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="3869b-143">Stil özellikleri denetimlerinde</span><span class="sxs-lookup"><span data-stu-id="3869b-143">Style Properties on Controls</span></span>

<span data-ttu-id="3869b-144">Öneri: denetim biçimlendirmede stili değerleri ayarlama durdurun ve CSS stil biçimlendirme değerlerini ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3869b-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="3869b-145">Web sunucusu denetimleri satır içi stil özelliklerini ayarlamak için kullanılan özellikler düzinelerce içerir.</span><span class="sxs-lookup"><span data-stu-id="3869b-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="3869b-146">Örneğin, bir denetim için metin rengini ForeColor özelliği ayarlar.</span><span class="sxs-lookup"><span data-stu-id="3869b-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="3869b-147">CSS stil sayfaları aracılığıyla daha verimli bir şekilde bu aynı etkiyi gerçekleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3869b-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="3869b-148">Stil sayfaları, stil değerleri merkezileştirmek ve uygulamanızı boyunca bu değerler ayarlama önlemek etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="3869b-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="3869b-149">Aşağıdaki örnek, bir CSS sınıfı kırmızı kümeleri metne gösterir.</span><span class="sxs-lookup"><span data-stu-id="3869b-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="3869b-150">Sonraki örnek nasıl dinamik olarak CSS sınıfı uygulanacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="3869b-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="3869b-151">Sayfa ve denetim geri aramalar</span><span class="sxs-lookup"><span data-stu-id="3869b-151">Page and Control Callbacks</span></span>

<span data-ttu-id="3869b-152">Öneri: sayfa ve denetim geri aramalar kullanarak durdurun ve bunun yerine aşağıdakilerden herhangi birini kullanın: AJAX, UpdatePanel, MVC eylem yöntemleri, Web API veya SignalR.</span><span class="sxs-lookup"><span data-stu-id="3869b-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="3869b-153">ASP.NET önceki sürümlerde sayfa ve denetim geri çağırma yöntemlerini, tam bir sayfayı yenilemeyi olmadan web sayfasının parçası güncelleştirmek etkin.</span><span class="sxs-lookup"><span data-stu-id="3869b-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="3869b-154">Kısmi Sayfa güncelleştirmelerini aracılığıyla şimdi gerçekleştirebileceğiniz [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/en-US/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) veya [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="3869b-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/en-US/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="3869b-155">Kolay URL'ler ile sorunlara neden olabilir çünkü geri çağırma yöntemlerini kullanarak ve yönlendirme durdurmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3869b-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="3869b-156">Varsayılan olarak, denetimleri geri arama yöntemleri etkinleştirmeyin, ancak bir denetim'deki bu özellik etkinleştirildiğinde, onu devre dışı bırakmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3869b-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="3869b-157">Tarayıcı yetenek algılama</span><span class="sxs-lookup"><span data-stu-id="3869b-157">Browser Capability Detection</span></span>

<span data-ttu-id="3869b-158">Öneri: statik tarayıcı yetenek algılama kullanarak durdurun ve bunun yerine dinamik özellik algılama kullanın.</span><span class="sxs-lookup"><span data-stu-id="3869b-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="3869b-159">ASP.NET önceki sürümlerinde desteklenen özellikler her tarayıcı için bir XML dosyasında depolanırdı.</span><span class="sxs-lookup"><span data-stu-id="3869b-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="3869b-160">Statik bir arama yoluyla algılama özelliğini desteği en iyi yaklaşım değildir.</span><span class="sxs-lookup"><span data-stu-id="3869b-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="3869b-161">Şimdi, dinamik olarak bir desteklenen bir tarayıcıdan özellikler gibi bir özellik algılama framework kullanarak algılayabilir [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="3869b-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="3869b-162">Özellik algılama yöntemi veya özelliği kullanmaya çalışıyor ve tarayıcı istenen sonucu üretilen olmadığını görmek için denetimi destek belirler.</span><span class="sxs-lookup"><span data-stu-id="3869b-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="3869b-163">Varsayılan olarak, Web uygulaması şablonlarında Modernizr dahil edilir.</span><span class="sxs-lookup"><span data-stu-id="3869b-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="3869b-164">Güvenlik</span><span class="sxs-lookup"><span data-stu-id="3869b-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="3869b-165">İstek doğrulama</span><span class="sxs-lookup"><span data-stu-id="3869b-165">Request Validation</span></span>

<span data-ttu-id="3869b-166">Öneri: kullanıcı girişi doğrulayın ve çıktı kullanıcılardan kodlayın.</span><span class="sxs-lookup"><span data-stu-id="3869b-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="3869b-167">İstek doğrulama, her isteği inceler ve algılanan tehdit bulunursa istek durdurur ASP.NET özelliğidir.</span><span class="sxs-lookup"><span data-stu-id="3869b-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="3869b-168">Uygulamanızı siteler arası komut dosyası saldırılarına karşı güvenliğini sağlamak için istek doğrulamayı bağlı değil.</span><span class="sxs-lookup"><span data-stu-id="3869b-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="3869b-169">Bunun yerine, kullanıcıların gelen tüm girdiyi doğrulayın ve çıktı kodlayın.</span><span class="sxs-lookup"><span data-stu-id="3869b-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="3869b-170">Sınırlı bazı durumlarda giriş doğrulamak için normal ifadeleri kullanabilirsiniz, ancak doğrulamanız gerekir daha karmaşık durumlarda değerle eşleşiyorsa belirleyen .NET sınıfları kullanarak kullanıcı girişini izin verilen değerler.</span><span class="sxs-lookup"><span data-stu-id="3869b-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="3869b-171">Aşağıdaki örnek, bir statik yöntem URI sınıfında bir kullanıcı tarafından sağlanan URI geçerli olup olmadığını belirlemek için nasıl kullanılacağını gösterir.</span><span class="sxs-lookup"><span data-stu-id="3869b-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="3869b-172">Ancak, yeterince URI doğrulamak için ayrıca belirtir emin olmak için kontrol `http` veya `https`.</span><span class="sxs-lookup"><span data-stu-id="3869b-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="3869b-173">Aşağıdaki örnek, URI geçerli olduğunu doğrulamak için örnek yöntemleri kullanır.</span><span class="sxs-lookup"><span data-stu-id="3869b-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="3869b-174">HTML olarak kullanıcı girişini işleme veya kullanıcı girişi bir SQL sorgusuna dahil olmak üzere önce kötü amaçlı kod dahil değildir emin olmak için değerleri kodlayın.</span><span class="sxs-lookup"><span data-stu-id="3869b-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="3869b-175">HTML için değer ile biçimlendirmede kodlamak &lt;%: %&gt; aşağıda gösterildiği gibi sözdizimi.</span><span class="sxs-lookup"><span data-stu-id="3869b-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="3869b-176">Veya, Razor sözdizimini HTML ile kodlamak @, aşağıda gösterildiği gibi.</span><span class="sxs-lookup"><span data-stu-id="3869b-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="3869b-177">Sonraki örnekte gösterildiği nasıl arka plan kodu değerinde HTML olarak kodlayın.</span><span class="sxs-lookup"><span data-stu-id="3869b-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="3869b-178">Güvenli bir şekilde SQL komutları için bir değer kodlamak için komut parametreleri gibi kullandığınız [SqlParameter](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="3869b-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="3869b-179">Cookieless form kimlik doğrulaması ve oturum</span><span class="sxs-lookup"><span data-stu-id="3869b-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="3869b-180">Öneri: tanımlama bilgileri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3869b-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="3869b-181">Kimlik doğrulama bilgilerini sorgu dizesinde geçirme güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="3869b-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="3869b-182">Bu nedenle, uygulamanızın kimlik doğrulamasını içerdiğinde tanımlama bilgileri gerektirir.</span><span class="sxs-lookup"><span data-stu-id="3869b-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="3869b-183">Tanımlama bilgisi gizli bilgileri depoluyorsa, tanımlama bilgisi için SSL gerektirme göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="3869b-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="3869b-184">Aşağıdaki örnekte, form kimlik doğrulamasını SSL üzerinden aktarılan bir tanımlama bilgisi gerektirdiğini Web.config dosyasında belirtebilir gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="3869b-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="3869b-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="3869b-185">EnableViewStateMac</span></span>

<span data-ttu-id="3869b-186">Öneri: Hiçbir zaman false olarak ayarlanmış.</span><span class="sxs-lookup"><span data-stu-id="3869b-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="3869b-187">Varsayılan olarak EnbableViewStateMac ayarlanır true.</span><span class="sxs-lookup"><span data-stu-id="3869b-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="3869b-188">Görünüm durumu, uygulamanızın kullanmıyor olsa bile EnableViewStateMac false olarak ayarlamayın.</span><span class="sxs-lookup"><span data-stu-id="3869b-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="3869b-189">Bu değer false olarak ayarlanırsa, uygulamanızı siteler arası komut dosyası karşı savunmasız hale getirir.</span><span class="sxs-lookup"><span data-stu-id="3869b-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="3869b-190">ASP.NET 4.5.2 ile başlayarak, çalışma zamanı zorlar **EnableViewStateMac = true**.</span><span class="sxs-lookup"><span data-stu-id="3869b-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="3869b-191">False olarak ayarlanmış olsa bile, çalışma zamanı bu değer yoksayar ve değer kümesiyle true olarak devam eder.</span><span class="sxs-lookup"><span data-stu-id="3869b-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="3869b-192">Daha fazla bilgi için bkz: [ASP.NET 4.5.2 ve EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="3869b-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="3869b-193">Aşağıdaki örnek, EnableViewStateMac true olarak ayarlanması gösterilmektedir.</span><span class="sxs-lookup"><span data-stu-id="3869b-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="3869b-194">Gerçekte bu değer varsayılan olarak true olduğundan true olarak ayarlamak gerekmez.</span><span class="sxs-lookup"><span data-stu-id="3869b-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="3869b-195">Ancak, false olarak herhangi bir sayfasında uygulamanızda ayarlarsanız, bu değer hemen düzeltmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="3869b-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="3869b-196">Orta güven</span><span class="sxs-lookup"><span data-stu-id="3869b-196">Medium Trust</span></span>

<span data-ttu-id="3869b-197">Öneri: Orta güven (veya başka bir güven düzeyinde) bir güvenlik sınırı bağlı değil.</span><span class="sxs-lookup"><span data-stu-id="3869b-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="3869b-198">Kısmi güven uygulamanız yeterli koruma sağlamaz ve kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="3869b-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="3869b-199">Bunun yerine, tam güven kullanın ve güvenilmeyen uygulamalarını ayrı uygulama havuzlarında yalıtma.</span><span class="sxs-lookup"><span data-stu-id="3869b-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="3869b-200">Ayrıca, her bir uygulama havuzunun benzersiz bir kimlik altında çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="3869b-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="3869b-201">Daha fazla bilgi için bkz: [ASP.NET kısmi güven garanti etmez uygulama yalıtımı](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="3869b-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="3869b-202">&lt;appSettings&gt;</span><span class="sxs-lookup"><span data-stu-id="3869b-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="3869b-203">Öneri: güvenlik ayarlarını devre dışı bırakmayın &lt;appSettings&gt; öğesi.</span><span class="sxs-lookup"><span data-stu-id="3869b-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="3869b-204">AppSettings öğe güvenlik güncelleştirmeleri için gerekli olan birden fazla değer içeriyor.</span><span class="sxs-lookup"><span data-stu-id="3869b-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="3869b-205">Değil, değiştirmek veya bu değerleri devre dışı bırakmak gerekir.</span><span class="sxs-lookup"><span data-stu-id="3869b-205">You should not change or disable these values.</span></span> <span data-ttu-id="3869b-206">Bir güncelleştirme dağıtırken bu değerleri devre dışı bırakmanız gerekir, hemen dağıtımını tamamladıktan sonra yeniden etkinleştirin.</span><span class="sxs-lookup"><span data-stu-id="3869b-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="3869b-207">Ayrıntılar için bkz [ASP.NET appSettings öğesi](https://msdn.microsoft.com/en-us/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="3869b-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/en-us/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="3869b-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="3869b-208">UrlPathEncode</span></span>

<span data-ttu-id="3869b-209">Öneri: Kullanmak [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx) yerine.</span><span class="sxs-lookup"><span data-stu-id="3869b-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="3869b-210">UrlPathEncode yöntemi çok özel tarayıcı uyumluluk sorunu gidermek için .NET Framework eklendi.</span><span class="sxs-lookup"><span data-stu-id="3869b-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="3869b-211">Yeterli bir URL kodlama değil ve uygulamanızı siteler arası komut dosyası korumaz.</span><span class="sxs-lookup"><span data-stu-id="3869b-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="3869b-212">Hiçbir zaman uygulamanızda kullanmalısınız.</span><span class="sxs-lookup"><span data-stu-id="3869b-212">You should never use it in your application.</span></span> <span data-ttu-id="3869b-213">Bunun yerine, kullanın [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="3869b-213">Instead, use [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="3869b-214">Aşağıdaki örnek, bir köprü denetim için bir sorgu dizesi parametresi olarak kodlanmış bir URL geçirmek gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="3869b-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="3869b-215">Güvenilirlik ve performans</span><span class="sxs-lookup"><span data-stu-id="3869b-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontext"></a><span data-ttu-id="3869b-216">PreSendRequestHeaders ve PreSendRequestContext</span><span class="sxs-lookup"><span data-stu-id="3869b-216">PreSendRequestHeaders and PreSendRequestContext</span></span>

<span data-ttu-id="3869b-217">Öneri: Bu olayları ile yönetilen modüller kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="3869b-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="3869b-218">Bunun yerine, gerekli görev gerçekleştirmek için yerel bir IIS modül yazma.</span><span class="sxs-lookup"><span data-stu-id="3869b-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="3869b-219">Bkz: [yerel kodlu HTTP modülleri oluşturma](https://msdn.microsoft.com/en-us/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="3869b-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/en-us/library/ms693629.aspx).</span></span>

<span data-ttu-id="3869b-220">Kullanabileceğiniz [PreSendRequestHeaders](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestheaders.aspx) ve [PreSendRequestContext](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestcontent.aspx) yerel IIS modülleri olaylarla ancak bunları IHttpModule uygulayan yönetilen modülleri kullanmayın.</span><span class="sxs-lookup"><span data-stu-id="3869b-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContext](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules, but do not use them with managed modules that implement IHttpModule.</span></span> <span data-ttu-id="3869b-221">Bu özellikleri ayarlama ile zaman uyumsuz istekleri sorunlara neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="3869b-221">Setting these properties can cause issues with asynchronous requests.</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="3869b-222">Web Forms ile zaman uyumsuz sayfası olayları</span><span class="sxs-lookup"><span data-stu-id="3869b-222">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="3869b-223">Sayfa yaşam döngüsü olayları için void yöntemleri zaman uyumsuz yazma Web formlarında öneri: Kaçının ve bunun yerine kullanın [Page.RegisterAsyncTask](https://msdn.microsoft.com/en-us/library/system.web.ui.page.registerasynctask.aspx) zaman uyumsuz kodu.</span><span class="sxs-lookup"><span data-stu-id="3869b-223">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/en-us/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="3869b-224">Bir sayfa olayla işaretlediğinizde **zaman uyumsuz** ve **void**, zaman uyumsuz kod bittiği olmadığını belirleyemez.</span><span class="sxs-lookup"><span data-stu-id="3869b-224">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="3869b-225">Bunun yerine, Page.RegisterAsyncTask zaman uyumsuz kod kendi tamamlama izlemenize olanak sağlayan bir şekilde çalıştırmak için kullanın.</span><span class="sxs-lookup"><span data-stu-id="3869b-225">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="3869b-226">Aşağıdaki örnekte gösterildiği düğmesine bir zaman uyumsuz kod içeren işleyicisi'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="3869b-226">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="3869b-227">Bu örnek bir dize değeri zaman uyumsuz olarak okumak önerilen bir uygulama olarak değil de yalnızca zaman uyumsuz bir görevi basitleştirilmiş bir örneği olarak sağlanan içerir.</span><span class="sxs-lookup"><span data-stu-id="3869b-227">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="3869b-228">Zaman uyumsuz görevleri kullanıyorsanız, Http Çalışma zamanı hedef framework 4.5 Web.config dosyasında ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3869b-228">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="3869b-229">Hedef framework 4.5 kapatır yeni eşitleme bağlamda ayarı .NET 4. 5 ' eklendi.</span><span class="sxs-lookup"><span data-stu-id="3869b-229">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="3869b-230">Bu değer varsayılan Visual Studio 2012'de yeni projeler olarak ayarlanmış, ancak var olan bir proje ile çalışıyorsanız ayarlanmış olmaması.</span><span class="sxs-lookup"><span data-stu-id="3869b-230">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="3869b-231">Yangın ve unut çalışma</span><span class="sxs-lookup"><span data-stu-id="3869b-231">Fire-and-Forget Work</span></span>

<span data-ttu-id="3869b-232">Öneri: (Bu tür ThreadPool.QueueUserWorkItem yöntemi çağırma veya sürekli olarak bir temsilci çağıran bir zamanlayıcı oluşturma) iş yangın ve unut başlatma ASP.NET içindeki bir isteği işlerken kaçının.</span><span class="sxs-lookup"><span data-stu-id="3869b-232">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="3869b-233">Uygulamanız ASP.NET içinde çalışan yangın ve unut iş varsa, uygulamanızın eşitlenmemiş alabilirsiniz. Herhangi bir zamanda uygulama etki alanı, devam eden işlem artık uygulama geçerli durumunu eşleşmeyebilir anlamı yok edilmesi.</span><span class="sxs-lookup"><span data-stu-id="3869b-233">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="3869b-234">Bu tür iş ASP.NET dışında taşımanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="3869b-234">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="3869b-235">Web işleri, Windows hizmeti veya çalışan rolü, Azure'da devam eden iş gerçekleştirmek için kullanın ve başka bir işlemden bu kodu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="3869b-235">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="3869b-236">ASP.NET içine bu iş gerçekleştirmeniz gerekirse, adlı Nuget paketi ekleyebilirsiniz [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) kodu çalıştırmak için.</span><span class="sxs-lookup"><span data-stu-id="3869b-236">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="3869b-237">İstek Varlık gövdesi</span><span class="sxs-lookup"><span data-stu-id="3869b-237">Request Entity Body</span></span>

<span data-ttu-id="3869b-238">Öneri: olay işleyicinin yürütmeden önce Request.Form veya Request.InputStream okuma kaçının.</span><span class="sxs-lookup"><span data-stu-id="3869b-238">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="3869b-239">En kısa Request.Form veya Request.InputStream okumalısınız olduğu işleyicinin sırasında yürütme olay.</span><span class="sxs-lookup"><span data-stu-id="3869b-239">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="3869b-240">MVC, işleyici denetleyicisidir ve eylem yöntemi çalıştığında execute etkinliğidir.</span><span class="sxs-lookup"><span data-stu-id="3869b-240">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="3869b-241">Web Forms işleyici sayfasıdır ve Page.Init olay başlatıldığında execute etkinliğidir.</span><span class="sxs-lookup"><span data-stu-id="3869b-241">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="3869b-242">Execute olay'den önceki istek Varlık gövdesi okuma, isteğin işlenmesi müdahale.</span><span class="sxs-lookup"><span data-stu-id="3869b-242">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="3869b-243">Execute olayından önce istek Varlık gövdesi okuma gerekiyorsa kullanın ya da [Request.GetBufferlessInputStream](https://msdn.microsoft.com/en-us/library/ff406798.aspx) veya [Request.GetBufferedInputStream](https://msdn.microsoft.com/en-us/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="3869b-243">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/en-us/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/en-us/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="3869b-244">GetBufferlessInputStream kullandığınızda ham akış istekten alma ve tüm istek işleme sorumluluğunu üstlenmesini.</span><span class="sxs-lookup"><span data-stu-id="3869b-244">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="3869b-245">Bunlar ASP.NET tarafından doldurulduğunu değil çünkü GetBufferlessInputStream çağrıldıktan sonra Request.Form ve Request.InputStream kullanılabilir değil.</span><span class="sxs-lookup"><span data-stu-id="3869b-245">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="3869b-246">GetBufferedInputStream kullandığınızda, gelen isteği akış bir kopyasını alın.</span><span class="sxs-lookup"><span data-stu-id="3869b-246">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="3869b-247">ASP.NET diğer kopya doldurur çünkü Request.Form ve Request.InputStream isteği daha sonra hala kullanılabilir durumdadır.</span><span class="sxs-lookup"><span data-stu-id="3869b-247">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="3869b-248">Response.Redirect ve Response.End</span><span class="sxs-lookup"><span data-stu-id="3869b-248">Response.Redirect and Response.End</span></span>

<span data-ttu-id="3869b-249">Öneri: iş parçacığı çağrıldıktan sonra nasıl işleneceğini farklar haberdar [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="3869b-249">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="3869b-250">[Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx) yöntemi Response.End yöntemini çağırır.</span><span class="sxs-lookup"><span data-stu-id="3869b-250">The [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="3869b-251">Zaman uyumlu bir işlemde hemen durdurmak geçerli iş parçacığının Request.Redirect çağırma neden olur.</span><span class="sxs-lookup"><span data-stu-id="3869b-251">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="3869b-252">İstek için kod yürütme devam eder ancak, zaman uyumsuz bir işlem Response.Redirect çağırma geçerli iş parçacığının abort değil.</span><span class="sxs-lookup"><span data-stu-id="3869b-252">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="3869b-253">Zaman uyumsuz bir işlem görevi kod yürütmeyi durdurmaya yönteminden döndürmelidir.</span><span class="sxs-lookup"><span data-stu-id="3869b-253">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="3869b-254">MVC projesinde Response.Redirect çağırmalısınız değil.</span><span class="sxs-lookup"><span data-stu-id="3869b-254">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="3869b-255">Bunun yerine, bir RedirectResult döndürür.</span><span class="sxs-lookup"><span data-stu-id="3869b-255">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="3869b-256">EnableViewState ve ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="3869b-256">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="3869b-257">Öneri: Görünüm durumu üzerinde denetimleri kullanın ayrıntılı bir denetim sağlamak için EnableViewState yerine kullanım ViewStateMode.</span><span class="sxs-lookup"><span data-stu-id="3869b-257">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="3869b-258">Sayfa yönergesi false EnableViewState ayarladığınızda, Görünüm durumu sayfa dahilindeki tüm denetimler için devre dışıdır ve etkinleştirilemez.</span><span class="sxs-lookup"><span data-stu-id="3869b-258">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="3869b-259">Görünüm durumu yalnızca, sayfadaki bazı denetimler için etkinleştirmek istiyorsanız, ViewStateMode sayfa için devre dışı olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3869b-259">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="3869b-260">Ardından, ViewStateMode gerçekten görünüm durumu gereken denetimlere etkin olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="3869b-260">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="3869b-261">Yalnızca ihtiyaç denetimleri için görünüm durumuna etkinleştirerek, web sayfaları için görünüm durumuna boyutunu küçültebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="3869b-261">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="3869b-262">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="3869b-262">SqlMembershipProvider</span></span>

<span data-ttu-id="3869b-263">Öneri: Evrensel sağlayıcıları kullanır.</span><span class="sxs-lookup"><span data-stu-id="3869b-263">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="3869b-264">SqlMembershipProvider geçerli proje şablonlarını almıştır [ASP.NET Evrensel Sağlayıcılar](http://www.nuget.org/packages/Microsoft.AspNet.Providers), olduğu bir NuGet paketi olarak kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="3869b-264">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="3869b-265">Şablonlar önceki bir sürümüyle oluşturulmuş bir projede SqlMembershipProvider kullanıyorsanız, Evrensel sağlayıcıları geçmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="3869b-265">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="3869b-266">Evrensel sağlayıcıları Entity Framework tarafından desteklenen tüm veritabanları ile çalışır.</span><span class="sxs-lookup"><span data-stu-id="3869b-266">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="3869b-267">Daha fazla bilgi için bkz: [ASP.NET Evrensel Sağlayıcılar Tanıtımı](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="3869b-267">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="3869b-268">Uzun süre çalışan istekler (> 110 saniye)</span><span class="sxs-lookup"><span data-stu-id="3869b-268">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="3869b-269">Öneri: Kullanmak [WebSockets](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket.aspx) veya [SignalR](../../../signalr/index.md) bağlanan istemciler ve zaman uyumsuz g/ç işlemleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="3869b-269">Recommendation: Use [WebSockets](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="3869b-270">Uzun süre çalışan istekler öngörülemeyen sonuçlara ve performansın web uygulamanızda neden olabilir.</span><span class="sxs-lookup"><span data-stu-id="3869b-270">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="3869b-271">Bir istek için varsayılan zaman aşımı ayarını 110 saniyedir.</span><span class="sxs-lookup"><span data-stu-id="3869b-271">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="3869b-272">ASP.NET oturum durumu uzun süre çalışan istekle kullanıyorsanız, oturum nesnesi üzerinde kilit 110 saniye sonra serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="3869b-272">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="3869b-273">Ancak, uygulamanızın oturum nesnesi üzerinde bir işlemi gerçekleştiriyor kilidi serbest ve işlemi başarıyla tamamlanmayabilir olabilir.</span><span class="sxs-lookup"><span data-stu-id="3869b-273">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="3869b-274">İlk istek çalışırken kullanıcıdan ikinci bir isteği engellenirse, ikinci bir istek tutarsız bir duruma Session nesnesinde erişebilir.</span><span class="sxs-lookup"><span data-stu-id="3869b-274">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="3869b-275">Uygulamanızı durdurma (veya zaman uyumlu) g/ç işlemleri içeriyorsa, uygulama yanıt vermeyen olacaktır.</span><span class="sxs-lookup"><span data-stu-id="3869b-275">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="3869b-276">Performansı artırmak için .NET Framework zaman uyumsuz g/ç işlemleri kullanın.</span><span class="sxs-lookup"><span data-stu-id="3869b-276">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="3869b-277">Ayrıca, WebSockets veya SignalR sunucuya bağlanan istemciler için kullanın.</span><span class="sxs-lookup"><span data-stu-id="3869b-277">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="3869b-278">Bu özellikler, uzun süredir çalışan istekleri verimli bir şekilde işlemek üzere tasarlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="3869b-278">These features are designed to efficiently handle long-running requests.</span></span>
