---
uid: ajax/cdn/overview
title: "Microsoft Ajax içerik teslim ağı | Microsoft Docs"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 2955888bc745fa3d49e40d364196283f16ad95ef
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2017
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="abe14-102">Microsoft Ajax içerik teslim ağı</span><span class="sxs-lookup"><span data-stu-id="abe14-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="abe14-103">Not: Bir Azure CDN kullanarak özellik hiçbir SLA Microsoft Ajax CDN sahiptir.</span><span class="sxs-lookup"><span data-stu-id="abe14-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="abe14-104">İçindekiler tablosu</span><span class="sxs-lookup"><span data-stu-id="abe14-104">Table of Contents</span></span>

<span data-ttu-id="abe14-105">**[ajax.microsoft.com AJAX.aspnetcdn.com için yeniden adlandırıldı](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="abe14-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="abe14-106">**[Visual Studio .vsdoc desteği](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="abe14-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="abe14-107">**[ASP.NET Ajax CDN kullanma](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="abe14-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="abe14-108">**[CDN jQuery kullanma](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="abe14-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="abe14-109">**[JQuery UI CDN kullanma](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="abe14-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="abe14-110">**[CDN üçüncü taraf dosyaları](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="abe14-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="abe14-111">CDN üzerinde jQuery sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="abe14-112">CDN üzerinde jQuery geçirmek sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="abe14-113">jQuery UI sürümleri CDN üzerinde</span><span class="sxs-lookup"><span data-stu-id="abe14-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="abe14-114">jQuery CDN üzerinde doğrulama sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="abe14-115">jQuery Mobile sürümleri CDN üzerinde</span><span class="sxs-lookup"><span data-stu-id="abe14-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="abe14-116">jQuery CDN üzerinde şablonları sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="abe14-117">jQuery CDN üzerinde döngüsü sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="abe14-118">jQuery CDN üzerinde DataTables sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="abe14-119">CDN üzerinde Modernizr sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="abe14-120">CDN üzerinde JSHint sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="abe14-121">CDN üzerinde Boşaltılan sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="abe14-122">CDN üzerinde sürümleri globalize</span><span class="sxs-lookup"><span data-stu-id="abe14-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="abe14-123">CDN üzerinde sürümleri yanıt</span><span class="sxs-lookup"><span data-stu-id="abe14-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="abe14-124">CDN üzerinde önyükleme sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="abe14-125">CDN üzerinde önyükleme TouchCarousel sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="abe14-126">CDN üzerinde Hammer.js sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="abe14-127">ASP.NET Web formları ve CDN üzerinde Ajax sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="abe14-128">ASP.NET MVC üzerinde CDN serbest bırakır</span><span class="sxs-lookup"><span data-stu-id="abe14-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="abe14-129">ASP.NET SignalR üzerinde CDN serbest bırakır</span><span class="sxs-lookup"><span data-stu-id="abe14-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="abe14-130">Microsoft Ajax içerik teslim ağı (CDN) jQuery gibi popüler üçüncü taraf JavaScript kitaplıklarını barındırır ve bunları Web uygulamalarınızın kolayca eklemenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="abe14-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="abe14-131">Örneğin, basitçe ekleyerek bu CDN üzerinde barındırılan jQuery kullanmaya başlayabilmeniz için bir &lt;betik&gt; etiketi sayfanıza ajax.aspnetcdn.com için işaret eder.</span><span class="sxs-lookup"><span data-stu-id="abe14-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="abe14-132">CDN yararlanarak, Ajax uygulamalarınızın performansını önemli ölçüde artırabilir.</span><span class="sxs-lookup"><span data-stu-id="abe14-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="abe14-133">CDN içeriğini tüm dünyada bulunan sunucuları önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="abe14-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="abe14-134">Ayrıca, CDN farklı etki alanlarında bulunan web siteleri için önbelleğe alınan üçüncü taraf JavaScript dosyaları yeniden tarayıcılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="abe14-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="abe14-135">Güvenli Yuva Katmanı'ni kullanarak bir web sayfası hizmet gerekebileceği CDN SSL (HTTPS) destekler.</span><span class="sxs-lookup"><span data-stu-id="abe14-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="abe14-136">CDN yüklenmiş ve size bu kitaplıkları sahibi tarafından lisanslanır aşağıdaki üçüncü taraf betik kitaplıkları barındırır:</span><span class="sxs-lookup"><span data-stu-id="abe14-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="abe14-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="abe14-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="abe14-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="abe14-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="abe14-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="abe14-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="abe14-140">jQuery doğrulama (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="abe14-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="abe14-141">jQuery döngüsü (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="abe14-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="abe14-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="abe14-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="abe14-143">Microsoft Ajax CDN Ayrıca, Microsoft tarafından yüklenen aşağıdaki kitaplıkları içerir:</span><span class="sxs-lookup"><span data-stu-id="abe14-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="abe14-144">ASP.NET Ajax'ı</span><span class="sxs-lookup"><span data-stu-id="abe14-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="abe14-145">ASP.NET MVC JavaScript dosyaları</span><span class="sxs-lookup"><span data-stu-id="abe14-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="abe14-146">ASP.NET SignalR JavaScript dosyaları</span><span class="sxs-lookup"><span data-stu-id="abe14-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="abe14-147">Microsoft, bu CDN üzerinde barındırılan herhangi bir üçüncü taraf kitaplıklar sahipliğini talep değil.</span><span class="sxs-lookup"><span data-stu-id="abe14-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="abe14-148">Telif hakkı sahipleri kitaplıkları, size bu kitaplıklar lisans.</span><span class="sxs-lookup"><span data-stu-id="abe14-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="abe14-149">Karşıdan yükleme ve bu tür kitaplıklarını kullanma gerekebilir herhangi bir hakka yalnızca ilgili telif hakkı sahipleri tarafından verilir.</span><span class="sxs-lookup"><span data-stu-id="abe14-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="abe14-150">Bunlar Microsoft kitaplıkları olduğundan, Microsoft bu CDN üzerinde barındırılan üçüncü taraf kitaplıklar için hiçbir garanti vermez veya fikri mülkiyet hakları lisansları (zımni hiçbir patent hakları dahil) sağlar.</span><span class="sxs-lookup"><span data-stu-id="abe14-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="abe14-151">JavaScript kitaplığı göndermek istiyor ve kitaplığınızın üst (http://trends.builtwith.com üzerinde listelendiği gibi) JavaScript kitaplıklarını veya bu kitaplıkları uzantıları/eklentilerinde biri (a) popüler; veya (b) için faydalı üzerindeki ASP.NET kullanın sonra temasa AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="abe14-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="abe14-152">ajax.microsoft.com AJAX.aspnetcdn.com için yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="abe14-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="abe14-153">CDN microsoft.com etki alanı adını kullanmak için kullanılan ve aspnetcdn.com etki alanı adını kullanmak üzere değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="abe14-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="abe14-154">Bu değişiklik, bir tarayıcı microsoft.com etki alanı başvurulduğunda tüm tanımlama bilgilerini bu etki alanından her istek ile kablo üzerinden göndermeden çünkü performansı artırmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="abe14-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="abe14-155">Microsoft.com dışında bir etki alanı adı için adlandırarak performans çok % 25 olarak artırılabilir.</span><span class="sxs-lookup"><span data-stu-id="abe14-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="abe14-156">Not ajax.microsoft.com çalışmaya devam eder ancak ajax.aspnetcdn.com önerilir.</span><span class="sxs-lookup"><span data-stu-id="abe14-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="abe14-157">Eski biçimi: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="abe14-158">Yeni biçim: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="abe14-159">Visual Studio .vsdoc desteği</span><span class="sxs-lookup"><span data-stu-id="abe14-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="abe14-160">.Vsdoc dosyaları düzgün VS 2008 SP1'e sahip olduğunuzdan emin olmanız gerekir Visual Studio 2008 ile kullanmak için yüklenir ve vsdoc dosyaları düzeltmesinin yüklü.</span><span class="sxs-lookup"><span data-stu-id="abe14-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="abe14-161">Bunlar buradan edinebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="abe14-161">You can get these from here:</span></span>

- [<span data-ttu-id="abe14-162">Visual Studio 2008 SP1'i karşıdan</span><span class="sxs-lookup"><span data-stu-id="abe14-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Visual Studio 2008 SP1'i Yükle")
- [<span data-ttu-id="abe14-163">Visual Studio 2008 SP1 için .vsdoc Düzeltme karşıdan</span><span class="sxs-lookup"><span data-stu-id="abe14-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 ".vsdoc düzeltme için Visual Studio 2008 SP1 yükle")

<span data-ttu-id="abe14-164">Visual Studio 2010 ek düzeltme eklerinin olmadan .vsdoc dosyalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="abe14-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="abe14-165">ASP.NET Ajax CDN kullanma</span><span class="sxs-lookup"><span data-stu-id="abe14-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="abe14-166">ASP.NET 4 kullanırken, ASP.NET framework komut dosyaları için tüm istekleri için CDN yönlendirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abe14-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="abe14-167">Yerel web sunucunuzun yerine CDN komut dosyaları alma önemli ölçüde ortak ASP.NET Web siteleri performansını artırabilir.</span><span class="sxs-lookup"><span data-stu-id="abe14-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="abe14-168">Microsoft Ajax CDN ile tüm ASP.NET framework betik isteklerini yeniden yönlendirmek için ScriptManager EnableCDN özelliğini kullanın:</span><span class="sxs-lookup"><span data-stu-id="abe14-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="abe14-169">CDN jQuery kullanma</span><span class="sxs-lookup"><span data-stu-id="abe14-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="abe14-170">Aşağıdaki betik öğesi bir sayfaya ekleyerek Web uygulamanızda CDN üzerinde barındırılan jQuery betikleri kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="abe14-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="abe14-171">CDN alabileceğiniz jQuery komut dosyasının küçültülmüş sürümünü de içerir. aşağıdaki öğeyi kullanarak:</span><span class="sxs-lookup"><span data-stu-id="abe14-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="abe14-172">CDN kullanılamaz durumda, kendi Web sitesinde bir yerel yol veritabanından jQuery yükleme için geri dönüş sayfanıza izin vermek için hemen CDN başvuran öğesinden sonra aşağıdaki öğeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="abe14-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="abe14-173">Aşağıdaki örnek sayfasına bir düğme tıklatıldığında div öğesinin içeriğini görüntülemek için jQuery kitaplığı (ile yerel bir kopya için geri dönüş) CDN sürümünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="abe14-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="abe14-174">JQuery hakkında daha fazla bilgi ve ziyaret ederek jQuery yerel bir kopyasını indirin [jQuery](http://jquery.com/) Web sitesi.</span><span class="sxs-lookup"><span data-stu-id="abe14-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="abe14-175">JQuery UI CDN kullanma</span><span class="sxs-lookup"><span data-stu-id="abe14-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="abe14-176">CDN jQuery kullanıcı Arabirimi kitaplığı da barındırır.</span><span class="sxs-lookup"><span data-stu-id="abe14-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="abe14-177">JQuery kullanıcı Arabirimi kitaplığı zengin bir pencere öğeleri ve ASP.NET uygulamalarınızın kullanabilirsiniz efektler içerir.</span><span class="sxs-lookup"><span data-stu-id="abe14-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="abe14-178">Örneğin, aşağıdaki sayfayı açılır takvimi görüntülemek için bir ASP.NET Web Forms uygulama bağlamında jQuery UI Datepicker nasıl kullanabileceğiniz gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="abe14-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="abe14-179">Klavyenizi kullanarak metin kutusuna odağı taşıdığınızda, bir takvim görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="abe14-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Oluşturulan DatePicker popup Takvim](overview/_static/image1.png)

<span data-ttu-id="abe14-181">Yukarıdaki kod CDN üç dosyaları içermelidir dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="abe14-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="abe14-182">JQuery Kitaplığı &mdash; jQuery kullanıcı Arabirimi kitaplığı jQuery kitaplıkta bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="abe14-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="abe14-183">JQuery kullanıcı Arabirimi kitaplığı eklemeden önce jQuery kitaplığı sayfanıza eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="abe14-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="abe14-184">JQuery kullanıcı Arabirimi Kitaplığı &mdash; jQuery kullanıcı Arabirimi kitaplığı tüm jQuery UI etkiler ve yukarıdaki sayfa kullanılan Datepicker pencere gibi pencere öğeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="abe14-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="abe14-185">JQuery UI tema &mdash; farklı temaları jQuery UI destekler.</span><span class="sxs-lookup"><span data-stu-id="abe14-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="abe14-186">Yukarıdaki sayfa Redmond temayı içeri aktarmak için bir CSS dosyası için bir bağlantı içerir.</span><span class="sxs-lookup"><span data-stu-id="abe14-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="abe14-187">Tüm standart jQuery UI temaları CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="abe14-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="abe14-188">[Bu sayfayı ziyaret](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 Microsoft Ajax CDN üzerinde") her tema için küçük resimler görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="abe14-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="abe14-189">JQuery kullanıcı Arabirimi Kitaplığı hakkında daha fazla bilgi edinmek için resmi ziyaret [jQuery UI Web sitesi](http://jQueryUI.com "jQuery UI Web sitesi").</span><span class="sxs-lookup"><span data-stu-id="abe14-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="abe14-190">CDN üçüncü taraf dosyaları</span><span class="sxs-lookup"><span data-stu-id="abe14-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="abe14-191">CDN en popüler üçüncü taraf JavaScript kitaplıklarını bazıları barındırır.</span><span class="sxs-lookup"><span data-stu-id="abe14-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="abe14-192">Microsoft, bu CDN üzerinde barındırılan herhangi bir üçüncü taraf kitaplıklar sahipliğini talep değil.</span><span class="sxs-lookup"><span data-stu-id="abe14-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="abe14-193">Telif hakkı sahipleri kitaplıkları, size bu kitaplıklar lisans.</span><span class="sxs-lookup"><span data-stu-id="abe14-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="abe14-194">Karşıdan yükleme ve bu tür kitaplıklarını kullanma gerekebilir herhangi bir hakka yalnızca ilgili telif hakkı sahipleri tarafından verilir.</span><span class="sxs-lookup"><span data-stu-id="abe14-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="abe14-195">Bunlar Microsoft kitaplıkları olduğundan, Microsoft bu CDN üzerinde barındırılan üçüncü taraf kitaplıklar için hiçbir garanti vermez veya fikri mülkiyet hakları lisansları (zımni hiçbir patent hakları dahil) sağlar.</span><span class="sxs-lookup"><span data-stu-id="abe14-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="abe14-196">CDN üzerinde jQuery sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="abe14-197">JQuery aşağıdaki sürümleri CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="abe14-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="abe14-198">jQuery sürüm 3.2.1</span><span class="sxs-lookup"><span data-stu-id="abe14-198">jQuery version 3.2.1</span></span>
- <span data-ttu-id="abe14-199">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="abe14-200">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="abe14-201">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="abe14-202">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="abe14-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="abe14-203">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.1.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="abe14-204">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.1.Slim.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="abe14-205">jQuery sürüm 3.2.0</span><span class="sxs-lookup"><span data-stu-id="abe14-205">jQuery version 3.2.0</span></span>

- <span data-ttu-id="abe14-206">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="abe14-207">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="abe14-208">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="abe14-209">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="abe14-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="abe14-210">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.0.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="abe14-211">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.0.Slim.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="abe14-212">jQuery sürüm 3.1.1</span><span class="sxs-lookup"><span data-stu-id="abe14-212">jQuery version 3.1.1</span></span>

- <span data-ttu-id="abe14-213">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="abe14-214">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="abe14-215">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="abe14-216">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="abe14-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="abe14-217">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.1.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="abe14-218">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.1.Slim.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="abe14-219">jQuery sürüm 3.1.0</span><span class="sxs-lookup"><span data-stu-id="abe14-219">jQuery version 3.1.0</span></span>

- <span data-ttu-id="abe14-220">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="abe14-221">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="abe14-222">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="abe14-223">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="abe14-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="abe14-224">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.0.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="abe14-225">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.0.Slim.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="abe14-226">jQuery sürüm 3.0.0</span><span class="sxs-lookup"><span data-stu-id="abe14-226">jQuery version 3.0.0</span></span>

- <span data-ttu-id="abe14-227">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="abe14-228">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="abe14-229">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.0.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="abe14-230">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.0.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="abe14-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="abe14-231">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.0.0.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="abe14-232">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.0.0.Slim.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="abe14-233">jQuery sürüm 2.2.4</span><span class="sxs-lookup"><span data-stu-id="abe14-233">jQuery version 2.2.4</span></span>

- <span data-ttu-id="abe14-234">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="abe14-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="abe14-235">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="abe14-236">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.4.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="abe14-237">jQuery sürüm 2.2.3</span><span class="sxs-lookup"><span data-stu-id="abe14-237">jQuery version 2.2.3</span></span>

- <span data-ttu-id="abe14-238">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="abe14-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="abe14-239">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="abe14-240">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.3.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-240">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="abe14-241">jQuery sürüm 2.2.2</span><span class="sxs-lookup"><span data-stu-id="abe14-241">jQuery version 2.2.2</span></span>

- <span data-ttu-id="abe14-242">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="abe14-243">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="abe14-244">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.2.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-244">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="abe14-245">jQuery sürüm 2.2.1</span><span class="sxs-lookup"><span data-stu-id="abe14-245">jQuery version 2.2.1</span></span>

- <span data-ttu-id="abe14-246">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="abe14-247">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="abe14-248">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-248">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="abe14-249">jQuery sürüm 2.2.0</span><span class="sxs-lookup"><span data-stu-id="abe14-249">jQuery version 2.2.0</span></span>

- <span data-ttu-id="abe14-250">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="abe14-251">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="abe14-252">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-252">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="abe14-253">jQuery sürüm 2.1.4</span><span class="sxs-lookup"><span data-stu-id="abe14-253">jQuery version 2.1.4</span></span>

- <span data-ttu-id="abe14-254">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="abe14-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="abe14-255">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="abe14-256">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.4.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-256">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="abe14-257">jQuery sürüm 2.1.3</span><span class="sxs-lookup"><span data-stu-id="abe14-257">jQuery version 2.1.3</span></span>

- <span data-ttu-id="abe14-258">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="abe14-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="abe14-259">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="abe14-260">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.3.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-260">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="abe14-261">jQuery sürüm 2.1.2</span><span class="sxs-lookup"><span data-stu-id="abe14-261">jQuery version 2.1.2</span></span>

- <span data-ttu-id="abe14-262">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="abe14-263">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="abe14-264">jQuery sürüm 2.1.1</span><span class="sxs-lookup"><span data-stu-id="abe14-264">jQuery version 2.1.1</span></span>

- <span data-ttu-id="abe14-265">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="abe14-266">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="abe14-267">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="abe14-268">jQuery sürüm 2.1.0</span><span class="sxs-lookup"><span data-stu-id="abe14-268">jQuery version 2.1.0</span></span>

- <span data-ttu-id="abe14-269">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="abe14-270">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="abe14-271">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-271">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="abe14-272">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="abe14-273">jQuery sürüm 2.0.3</span><span class="sxs-lookup"><span data-stu-id="abe14-273">jQuery version 2.0.3</span></span>

- <span data-ttu-id="abe14-274">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="abe14-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="abe14-275">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-275">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="abe14-276">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="abe14-277">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.3.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="abe14-278">jQuery sürüm 2.0.2</span><span class="sxs-lookup"><span data-stu-id="abe14-278">jQuery version 2.0.2</span></span>

- <span data-ttu-id="abe14-279">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="abe14-280">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-280">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="abe14-281">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="abe14-282">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.2.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="abe14-283">jQuery sürüm 2.0.1</span><span class="sxs-lookup"><span data-stu-id="abe14-283">jQuery version 2.0.1</span></span>

- <span data-ttu-id="abe14-284">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="abe14-285">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-285">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="abe14-286">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="abe14-287">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="abe14-288">jQuery sürüm 2.0.0</span><span class="sxs-lookup"><span data-stu-id="abe14-288">jQuery version 2.0.0</span></span>

- <span data-ttu-id="abe14-289">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="abe14-290">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-290">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="abe14-291">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="abe14-292">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="abe14-293">jQuery sürüm 1.12.4</span><span class="sxs-lookup"><span data-stu-id="abe14-293">jQuery version 1.12.4</span></span>

- <span data-ttu-id="abe14-294">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="abe14-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="abe14-295">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-295">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="abe14-296">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.4.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="abe14-297">jQuery sürüm 1.12.3</span><span class="sxs-lookup"><span data-stu-id="abe14-297">jQuery version 1.12.3</span></span>

- <span data-ttu-id="abe14-298">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="abe14-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="abe14-299">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="abe14-300">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.3.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-300">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="abe14-301">jQuery sürüm 1.12.2</span><span class="sxs-lookup"><span data-stu-id="abe14-301">jQuery version 1.12.2</span></span>

- <span data-ttu-id="abe14-302">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="abe14-303">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="abe14-304">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.2.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-304">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="abe14-305">jQuery sürüm 1.12.1</span><span class="sxs-lookup"><span data-stu-id="abe14-305">jQuery version 1.12.1</span></span>

- <span data-ttu-id="abe14-306">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="abe14-307">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="abe14-308">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-308">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="abe14-309">jQuery sürüm 1.12.0</span><span class="sxs-lookup"><span data-stu-id="abe14-309">jQuery version 1.12.0</span></span>

- <span data-ttu-id="abe14-310">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="abe14-311">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="abe14-312">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-312">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="abe14-313">jQuery sürüm 1.11.3</span><span class="sxs-lookup"><span data-stu-id="abe14-313">jQuery version 1.11.3</span></span>

- <span data-ttu-id="abe14-314">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="abe14-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="abe14-315">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="abe14-316">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.3.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-316">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="abe14-317">jQuery sürüm 1.11.2</span><span class="sxs-lookup"><span data-stu-id="abe14-317">jQuery version 1.11.2</span></span>

- <span data-ttu-id="abe14-318">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="abe14-319">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="abe14-320">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.2.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-320">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="abe14-321">jQuery sürüm 1.11.1</span><span class="sxs-lookup"><span data-stu-id="abe14-321">jQuery version 1.11.1</span></span>

- <span data-ttu-id="abe14-322">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="abe14-323">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="abe14-324">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-324">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="abe14-325">jQuery sürüm 1.11.0</span><span class="sxs-lookup"><span data-stu-id="abe14-325">jQuery version 1.11.0</span></span>

- <span data-ttu-id="abe14-326">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="abe14-327">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="abe14-328">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-328">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="abe14-329">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="abe14-330">jQuery sürüm 1.10.2</span><span class="sxs-lookup"><span data-stu-id="abe14-330">jQuery version 1.10.2</span></span>

- <span data-ttu-id="abe14-331">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="abe14-332">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-332">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="abe14-333">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="abe14-334">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.2.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="abe14-335">jQuery sürüm 1.10.1</span><span class="sxs-lookup"><span data-stu-id="abe14-335">jQuery version 1.10.1</span></span>

- <span data-ttu-id="abe14-336">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="abe14-337">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-337">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="abe14-338">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="abe14-339">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="abe14-340">jQuery sürüm 1.10.0</span><span class="sxs-lookup"><span data-stu-id="abe14-340">jQuery version 1.10.0</span></span>

- <span data-ttu-id="abe14-341">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="abe14-342">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-342">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="abe14-343">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="abe14-344">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="abe14-345">jQuery sürüm 1.9.1</span><span class="sxs-lookup"><span data-stu-id="abe14-345">jQuery version 1.9.1</span></span>

- <span data-ttu-id="abe14-346">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="abe14-347">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-347">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="abe14-348">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="abe14-349">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="abe14-350">jQuery sürüm 1.9.0</span><span class="sxs-lookup"><span data-stu-id="abe14-350">jQuery version 1.9.0</span></span>

- <span data-ttu-id="abe14-351">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="abe14-352">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-352">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="abe14-353">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="abe14-354">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="abe14-355">jQuery sürüm 1.8.3</span><span class="sxs-lookup"><span data-stu-id="abe14-355">jQuery version 1.8.3</span></span>

- <span data-ttu-id="abe14-356">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="abe14-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="abe14-357">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-357">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="abe14-358">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="abe14-359">jQuery sürüm 1.8.2</span><span class="sxs-lookup"><span data-stu-id="abe14-359">jQuery version 1.8.2</span></span>

- <span data-ttu-id="abe14-360">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="abe14-361">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="abe14-362">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-362">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="abe14-363">jQuery sürüm 1.8.1</span><span class="sxs-lookup"><span data-stu-id="abe14-363">jQuery version 1.8.1</span></span>

- <span data-ttu-id="abe14-364">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="abe14-365">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="abe14-366">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-366">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="abe14-367">jQuery sürüm 1.8.0</span><span class="sxs-lookup"><span data-stu-id="abe14-367">jQuery version 1.8.0</span></span>

- <span data-ttu-id="abe14-368">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="abe14-369">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="abe14-370">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-370">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="abe14-371">jQuery sürüm 1.7.2</span><span class="sxs-lookup"><span data-stu-id="abe14-371">jQuery version 1.7.2</span></span>

- <span data-ttu-id="abe14-372">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="abe14-373">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="abe14-374">jQuery sürüm 1.7.1</span><span class="sxs-lookup"><span data-stu-id="abe14-374">jQuery version 1.7.1</span></span>

- <span data-ttu-id="abe14-375">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="abe14-376">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="abe14-377">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="abe14-378">jQuery sürüm 1.7</span><span class="sxs-lookup"><span data-stu-id="abe14-378">jQuery version 1.7</span></span>

- <span data-ttu-id="abe14-379">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="abe14-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="abe14-380">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="abe14-381">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-381">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="abe14-382">jQuery sürüm 1.6.4</span><span class="sxs-lookup"><span data-stu-id="abe14-382">jQuery version 1.6.4</span></span>

- <span data-ttu-id="abe14-383">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="abe14-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="abe14-384">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="abe14-385">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-385">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="abe14-386">jQuery sürüm 1.6.3</span><span class="sxs-lookup"><span data-stu-id="abe14-386">jQuery version 1.6.3</span></span>

- <span data-ttu-id="abe14-387">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="abe14-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="abe14-388">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="abe14-389">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-389">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="abe14-390">jQuery sürüm 1.6.2</span><span class="sxs-lookup"><span data-stu-id="abe14-390">jQuery version 1.6.2</span></span>

- <span data-ttu-id="abe14-391">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="abe14-392">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="abe14-393">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-393">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="abe14-394">jQuery sürüm 1.6.1</span><span class="sxs-lookup"><span data-stu-id="abe14-394">jQuery version 1.6.1</span></span>

- <span data-ttu-id="abe14-395">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="abe14-396">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="abe14-397">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-397">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="abe14-398">jQuery sürüm 1.6</span><span class="sxs-lookup"><span data-stu-id="abe14-398">jQuery version 1.6</span></span>

- <span data-ttu-id="abe14-399">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="abe14-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="abe14-400">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="abe14-401">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-401">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="abe14-402">jQuery sürüm 1.5.2</span><span class="sxs-lookup"><span data-stu-id="abe14-402">jQuery version 1.5.2</span></span>

- <span data-ttu-id="abe14-403">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="abe14-404">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="abe14-405">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-405">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="abe14-406">jQuery sürüm 1.5.1</span><span class="sxs-lookup"><span data-stu-id="abe14-406">jQuery version 1.5.1</span></span>

- <span data-ttu-id="abe14-407">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="abe14-408">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="abe14-409">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-409">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="abe14-410">jQuery sürüm 1.5</span><span class="sxs-lookup"><span data-stu-id="abe14-410">jQuery version 1.5</span></span>

- <span data-ttu-id="abe14-411">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="abe14-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="abe14-412">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="abe14-413">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-413">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="abe14-414">jQuery sürüm 1.4.4</span><span class="sxs-lookup"><span data-stu-id="abe14-414">jQuery version 1.4.4</span></span>

- <span data-ttu-id="abe14-415">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="abe14-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="abe14-416">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="abe14-417">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-417">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="abe14-418">jQuery sürüm 1.4.3</span><span class="sxs-lookup"><span data-stu-id="abe14-418">jQuery version 1.4.3</span></span>

- <span data-ttu-id="abe14-419">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="abe14-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="abe14-420">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="abe14-421">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-421">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="abe14-422">jQuery sürüm 1.4.2</span><span class="sxs-lookup"><span data-stu-id="abe14-422">jQuery version 1.4.2</span></span>

- <span data-ttu-id="abe14-423">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="abe14-424">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="abe14-425">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-425">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="abe14-426">jQuery sürüm 1.4.1</span><span class="sxs-lookup"><span data-stu-id="abe14-426">jQuery version 1.4.1</span></span>

- <span data-ttu-id="abe14-427">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="abe14-428">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="abe14-429">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-429">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="abe14-430">jQuery sürüm 1.4</span><span class="sxs-lookup"><span data-stu-id="abe14-430">jQuery version 1.4</span></span>

- <span data-ttu-id="abe14-431">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="abe14-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="abe14-432">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="abe14-433">jQuery sürüm 1.3.2</span><span class="sxs-lookup"><span data-stu-id="abe14-433">jQuery version 1.3.2</span></span>

- <span data-ttu-id="abe14-434">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="abe14-435">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.3.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="abe14-436">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="abe14-437">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.3.2.Min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="abe14-437">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="abe14-438">CDN üzerinde jQuery geçirmek sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-438">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="abe14-439">JQuery geçirme aşağıdaki sürümleri CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="abe14-439">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="abe14-440">jQuery sürüm 3.0.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="abe14-440">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="abe14-441">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-441">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="abe14-442">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-3.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-442">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="abe14-443">jQuery geçirme 1.2.1 sürümü</span><span class="sxs-lookup"><span data-stu-id="abe14-443">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="abe14-444">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-444">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="abe14-445">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-445">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="abe14-446">jQuery sürüm 1.2.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="abe14-446">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="abe14-447">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-447">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="abe14-448">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="abe14-449">jQuery sürüm 1.1.1 geçirme</span><span class="sxs-lookup"><span data-stu-id="abe14-449">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="abe14-450">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-450">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="abe14-451">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="abe14-452">jQuery sürüm 1.1.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="abe14-452">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="abe14-453">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-453">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="abe14-454">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="abe14-455">jQuery sürümü 1.0.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="abe14-455">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="abe14-456">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-456">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="abe14-457">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="abe14-458">jQuery UI sürümleri CDN üzerinde</span><span class="sxs-lookup"><span data-stu-id="abe14-458">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="abe14-459">JQuery kullanıcı Arabirimi kitaplığı aşağıdaki sürümleri, bu CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="abe14-459">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="abe14-460">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="abe14-460">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="abe14-461">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="abe14-461">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-462">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="abe14-462">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-463">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="abe14-463">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-464">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="abe14-464">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-465">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="abe14-465">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-466">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="abe14-466">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-467">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="abe14-467">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-468">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="abe14-468">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-469">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="abe14-469">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-470">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="abe14-470">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2 UI Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-471">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="abe14-471">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-472">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="abe14-472">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-473">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="abe14-473">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-474">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="abe14-474">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-475">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="abe14-475">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-476">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="abe14-476">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-477">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="abe14-477">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-478">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="abe14-478">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-479">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="abe14-479">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-480">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="abe14-480">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-481">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="abe14-481">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-482">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="abe14-482">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-483">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="abe14-483">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-484">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="abe14-484">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-485">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="abe14-485">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-486">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="abe14-486">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-487">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="abe14-487">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-488">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="abe14-488">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-489">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="abe14-489">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-490">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="abe14-490">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-491">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="abe14-491">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-492">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="abe14-492">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-493">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="abe14-493">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-494">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="abe14-494">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-495">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="abe14-495">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="abe14-496">jQuery CDN üzerinde doğrulama sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-496">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="abe14-497">JQuery doğrulama kitaplığı aşağıdaki sürümleri, bu CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="abe14-497">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="abe14-498">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="abe14-498">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="abe14-499">jQuery doğrulama 1.17.0</span><span class="sxs-lookup"><span data-stu-id="abe14-499">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery doğrulama 1.17.0")
- [<span data-ttu-id="abe14-500">jQuery doğrulama 1.16.0</span><span class="sxs-lookup"><span data-stu-id="abe14-500">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery doğrulama 1.16.0")
- [<span data-ttu-id="abe14-501">jQuery doğrulama 1.15.1</span><span class="sxs-lookup"><span data-stu-id="abe14-501">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery doğrulama 1.15.1")
- [<span data-ttu-id="abe14-502">jQuery doğrulama 1.15.0</span><span class="sxs-lookup"><span data-stu-id="abe14-502">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery doğrulama 1.15.0")
- [<span data-ttu-id="abe14-503">jQuery doğrulama 1.14.0</span><span class="sxs-lookup"><span data-stu-id="abe14-503">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery doğrulama 1.14.0")
- [<span data-ttu-id="abe14-504">jQuery doğrulama 1.13.1</span><span class="sxs-lookup"><span data-stu-id="abe14-504">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery doğrulama 1.13.1")
- [<span data-ttu-id="abe14-505">jQuery doğrulama 1.13.0</span><span class="sxs-lookup"><span data-stu-id="abe14-505">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery doğrulama 1.13.0")
- [<span data-ttu-id="abe14-506">jQuery doğrulama 1.12.0</span><span class="sxs-lookup"><span data-stu-id="abe14-506">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery doğrulama 1.12.0")
- [<span data-ttu-id="abe14-507">jQuery doğrulama 1.11.1</span><span class="sxs-lookup"><span data-stu-id="abe14-507">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery doğrulama 1.11.1")
- [<span data-ttu-id="abe14-508">jQuery doğrulama 1.11.0</span><span class="sxs-lookup"><span data-stu-id="abe14-508">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery doğrulama 1.11.0")
- [<span data-ttu-id="abe14-509">jQuery doğrulama 1.10.0</span><span class="sxs-lookup"><span data-stu-id="abe14-509">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery doğrulama 1.10.0")
- [<span data-ttu-id="abe14-510">jQuery doğrulama 1.9</span><span class="sxs-lookup"><span data-stu-id="abe14-510">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate sürüm 1.9")
- [<span data-ttu-id="abe14-511">jQuery doğrulama 1.8.1</span><span class="sxs-lookup"><span data-stu-id="abe14-511">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate sürüm 1.8.1")
- [<span data-ttu-id="abe14-512">jQuery doğrulama 1.8</span><span class="sxs-lookup"><span data-stu-id="abe14-512">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate sürüm 1,8")
- [<span data-ttu-id="abe14-513">jQuery doğrulama 1.7</span><span class="sxs-lookup"><span data-stu-id="abe14-513">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate sürüm 1.7")
- [<span data-ttu-id="abe14-514">jQuery doğrulama 1.6</span><span class="sxs-lookup"><span data-stu-id="abe14-514">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery doğrulama 1.6")
- [<span data-ttu-id="abe14-515">jQuery doğrulama 1.5.5</span><span class="sxs-lookup"><span data-stu-id="abe14-515">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery doğrulama 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="abe14-516">jQuery Mobile sürümleri CDN üzerinde</span><span class="sxs-lookup"><span data-stu-id="abe14-516">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="abe14-517">JQuery Mobile kitaplığı aşağıdaki sürümleri, bu CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="abe14-517">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="abe14-518">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="abe14-518">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="abe14-519">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="abe14-519">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-520">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="abe14-520">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-521">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="abe14-521">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-522">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="abe14-522">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-523">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="abe14-523">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-524">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="abe14-524">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-525">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="abe14-525">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-526">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="abe14-526">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-527">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="abe14-527">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-528">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="abe14-528">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-529">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="abe14-529">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-530">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="abe14-530">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 için Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-531">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="abe14-531">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-532">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="abe14-532">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 Microsoft Ajax CDN")
- [<span data-ttu-id="abe14-533">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="abe14-533">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 için Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-534">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="abe14-534">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 için Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="abe14-535">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="abe14-535">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.0 Beta 3")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="abe14-536">jQuery CDN üzerinde şablonları sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-536">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="abe14-537">JQuery şablonları eklentisi aşağıdaki sürümleri, bu CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="abe14-537">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="abe14-538">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="abe14-538">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="abe14-539">jQuery şablonları Beta 1</span><span class="sxs-lookup"><span data-stu-id="abe14-539">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery şablonları Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="abe14-540">jQuery CDN üzerinde döngüsü sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-540">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="abe14-541">JQuery döngüsü eklentisi aşağıdaki sürümleri, bu CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="abe14-541">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="abe14-542">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="abe14-542">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="abe14-543">jQuery döngüsü 2.99</span><span class="sxs-lookup"><span data-stu-id="abe14-543">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery döngüsü 2.99")
- [<span data-ttu-id="abe14-544">jQuery döngüsü 2.94</span><span class="sxs-lookup"><span data-stu-id="abe14-544">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery döngüsü 2.94")
- [<span data-ttu-id="abe14-545">jQuery döngüsü 2.88</span><span class="sxs-lookup"><span data-stu-id="abe14-545">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery döngüsü 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="abe14-546">jQuery CDN üzerinde DataTables sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-546">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="abe14-547">JQuery DataTables eklentisi aşağıdaki sürümleri, bu CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="abe14-547">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="abe14-548">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="abe14-548">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="abe14-549">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="abe14-549">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="abe14-550">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="abe14-550">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="abe14-551">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="abe14-551">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="abe14-552">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="abe14-552">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="abe14-553">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="abe14-553">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="abe14-554">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="abe14-554">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="abe14-555">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="abe14-555">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="abe14-556">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="abe14-556">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="abe14-557">CDN üzerinde Modernizr sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-557">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="abe14-558">Aşağıdaki sürümleri [Modernizr](http://www.modernizr.com "Modernizr") CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="abe14-558">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="abe14-559">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="abe14-559">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="abe14-560">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-560">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="abe14-561">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-561">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="abe14-562">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-562">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="abe14-563">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="abe14-563">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="abe14-564">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="abe14-564">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="abe14-565">CDN üzerinde JSHint sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-565">JSHint Releases on the CDN</span></span>

<span data-ttu-id="abe14-566">Aşağıdaki sürümleri [JSHint](http://www.jshint.com "JSHint") CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="abe14-566">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="abe14-567">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="abe14-567">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="abe14-568">CDN üzerinde Boşaltılan sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-568">Knockout Releases on the CDN</span></span>

<span data-ttu-id="abe14-569">Aşağıdaki sürümleri [Boşaltılan](http://www.knockoutjs.com "Boşaltılan") CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="abe14-569">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="abe14-570">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-570">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="abe14-571">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="abe14-571">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="abe14-572">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-572">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="abe14-573">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="abe14-573">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="abe14-574">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-574">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="abe14-575">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="abe14-575">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="abe14-576">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-576">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="abe14-577">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="abe14-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="abe14-578">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="abe14-579">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="abe14-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="abe14-580">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="abe14-581">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="abe14-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="abe14-582">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="abe14-583">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="abe14-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="abe14-584">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="abe14-585">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="abe14-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="abe14-586">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="abe14-587">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="abe14-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="abe14-588">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="abe14-589">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="abe14-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="abe14-590">CDN üzerinde sürümleri globalize</span><span class="sxs-lookup"><span data-stu-id="abe14-590">Globalize Releases on the CDN</span></span>

<span data-ttu-id="abe14-591">Aşağıdaki sürümleri [Globalize](https://github.com/jquery/globalize "Globalize") CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="abe14-591">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="abe14-592">Sürümü 1.0.0 globalize</span><span class="sxs-lookup"><span data-stu-id="abe14-592">Globalize version 1.0.0</span></span>

- <span data-ttu-id="abe14-593">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="abe14-593">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="abe14-594">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/node-Main.js</span><span class="sxs-lookup"><span data-stu-id="abe14-594">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="abe14-595">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="abe14-595">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="abe14-596">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Date.js</span><span class="sxs-lookup"><span data-stu-id="abe14-596">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="abe14-597">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="abe14-597">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="abe14-598">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="abe14-598">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="abe14-599">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="abe14-599">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="abe14-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Relative-Time.js</span><span class="sxs-lookup"><span data-stu-id="abe14-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="abe14-601">Sürüm 0.1.1 globalize</span><span class="sxs-lookup"><span data-stu-id="abe14-601">Globalize version 0.1.1</span></span>

- <span data-ttu-id="abe14-602">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-602">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="abe14-603">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="abe14-603">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="abe14-604">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.cultures.js</span><span class="sxs-lookup"><span data-stu-id="abe14-604">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="abe14-605">tüm kültürler</span><span class="sxs-lookup"><span data-stu-id="abe14-605">all cultures</span></span>
- <span data-ttu-id="abe14-606">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.culture. {kültür kodu} .js</span><span class="sxs-lookup"><span data-stu-id="abe14-606">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="abe14-607">"{Kodu kültür}" istenen kültürü kod ile değiştirin, örneğin globalize.culture.en GB.js== Microsoft CDN üzerinde dosyaları bu == kitaplıkları, Microsoft tarafından karşıya.</span><span class="sxs-lookup"><span data-stu-id="abe14-607">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="abe14-608">CDN üzerinde sürümleri yanıt</span><span class="sxs-lookup"><span data-stu-id="abe14-608">Respond Releases on the CDN</span></span>

<span data-ttu-id="abe14-609">Aşağıdaki sürümleri [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") yanıt CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="abe14-609">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="abe14-610">Sürüm 1.4.2 yanıt</span><span class="sxs-lookup"><span data-stu-id="abe14-610">Respond version 1.4.2</span></span>

- <span data-ttu-id="abe14-611">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.js</span><span class="sxs-lookup"><span data-stu-id="abe14-611">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="abe14-612">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-612">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="abe14-613">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="abe14-613">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="abe14-614">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-614">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="abe14-615">Sürüm 1.4.1 yanıt</span><span class="sxs-lookup"><span data-stu-id="abe14-615">Respond version 1.4.1</span></span>

- <span data-ttu-id="abe14-616">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.js</span><span class="sxs-lookup"><span data-stu-id="abe14-616">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="abe14-617">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-617">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="abe14-618">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="abe14-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="abe14-619">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="abe14-620">Sürüm 1.4.0 yanıt</span><span class="sxs-lookup"><span data-stu-id="abe14-620">Respond version 1.4.0</span></span>

- <span data-ttu-id="abe14-621">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="abe14-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="abe14-622">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-622">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="abe14-623">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="abe14-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="abe14-624">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="abe14-625">Sürüm 1.3.0 yanıt</span><span class="sxs-lookup"><span data-stu-id="abe14-625">Respond version 1.3.0</span></span>

- <span data-ttu-id="abe14-626">http://AJAX.aspnetcdn.com/AJAX/Respond/1.3.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="abe14-626">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="abe14-627">Sürüm 1.2.0 yanıt</span><span class="sxs-lookup"><span data-stu-id="abe14-627">Respond version 1.2.0</span></span>

- <span data-ttu-id="abe14-628">http://AJAX.aspnetcdn.com/AJAX/Respond/1.2.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="abe14-628">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="abe14-629">CDN üzerinde önyükleme sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-629">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="abe14-630">Aşağıdaki sürümleri [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") önyükleme CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="abe14-630">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="abe14-631">Önyükleme sürüm 3.3.7</span><span class="sxs-lookup"><span data-stu-id="abe14-631">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="abe14-632">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-632">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="abe14-633">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-633">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-634">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-634">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-635">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-635">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="abe14-636">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-636">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-637">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-637">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-638">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-638">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="abe14-639">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-640">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-641">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-642">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-643">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="abe14-644">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="abe14-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="abe14-645">Önyükleme sürüm 3.3.6</span><span class="sxs-lookup"><span data-stu-id="abe14-645">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="abe14-646">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="abe14-647">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-648">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-649">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="abe14-650">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-651">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-652">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-652">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="abe14-653">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-654">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-655">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-656">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-657">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="abe14-658">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="abe14-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="abe14-659">Önyükleme sürüm 3.3.5</span><span class="sxs-lookup"><span data-stu-id="abe14-659">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="abe14-660">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="abe14-661">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-662">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-663">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="abe14-664">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-665">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-666">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-666">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="abe14-667">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-668">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-669">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-670">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-671">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="abe14-672">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="abe14-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="abe14-673">Önyükleme sürüm 3.3.4</span><span class="sxs-lookup"><span data-stu-id="abe14-673">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="abe14-674">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="abe14-675">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-676">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-677">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="abe14-678">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-679">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-680">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-680">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="abe14-681">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-682">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-683">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-684">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-685">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="abe14-686">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="abe14-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="abe14-687">Önyükleme sürüm 3.3.2</span><span class="sxs-lookup"><span data-stu-id="abe14-687">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="abe14-688">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="abe14-689">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-690">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-691">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="abe14-692">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-693">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-694">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-694">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="abe14-695">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-696">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-697">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-698">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-699">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="abe14-700">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="abe14-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="abe14-701">Önyükleme sürüm 3.3.1</span><span class="sxs-lookup"><span data-stu-id="abe14-701">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="abe14-702">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="abe14-703">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-704">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-705">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="abe14-706">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-707">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-708">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-708">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="abe14-709">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-710">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-711">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-712">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-713">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="abe14-714">Önyükleme sürüm 3.3.0</span><span class="sxs-lookup"><span data-stu-id="abe14-714">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="abe14-715">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="abe14-716">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-717">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-718">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="abe14-719">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-720">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-721">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-721">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="abe14-722">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-723">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-724">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-725">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-726">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="abe14-727">Önyükleme sürüm 3.2.0</span><span class="sxs-lookup"><span data-stu-id="abe14-727">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="abe14-728">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="abe14-729">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-730">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-731">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="abe14-732">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-733">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-734">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-734">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="abe14-735">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-736">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-737">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-738">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-739">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="abe14-740">Önyükleme sürüm 3.1.1</span><span class="sxs-lookup"><span data-stu-id="abe14-740">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="abe14-741">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="abe14-742">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-743">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-744">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="abe14-745">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-746">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-747">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-747">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="abe14-748">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-749">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-750">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-751">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-752">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="abe14-753">Önyükleme sürüm 3.1.0</span><span class="sxs-lookup"><span data-stu-id="abe14-753">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="abe14-754">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="abe14-755">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-756">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-757">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="abe14-758">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-759">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-760">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="abe14-760">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="abe14-761">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-762">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-763">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-764">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-765">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="abe14-766">Önyükleme sürüm 3.0.3</span><span class="sxs-lookup"><span data-stu-id="abe14-766">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="abe14-767">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="abe14-768">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-769">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-770">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-771">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-772">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-773">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-773">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-774">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-775">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-776">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="abe14-777">Önyükleme sürüm 3.0.2</span><span class="sxs-lookup"><span data-stu-id="abe14-777">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="abe14-778">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="abe14-779">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-780">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-781">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-782">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-783">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-784">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-784">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-785">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-786">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-787">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="abe14-788">Önyükleme sürüm 3.0.1</span><span class="sxs-lookup"><span data-stu-id="abe14-788">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="abe14-789">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="abe14-790">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-791">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-792">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-793">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-794">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-795">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-795">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-796">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-797">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-798">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="abe14-799">Önyükleme sürüm 3.0.0</span><span class="sxs-lookup"><span data-stu-id="abe14-799">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="abe14-800">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="abe14-801">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-802">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-803">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-804">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="abe14-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="abe14-805">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="abe14-806">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="abe14-806">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="abe14-807">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="abe14-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="abe14-808">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="abe14-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="abe14-809">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="abe14-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="abe14-810">Önyükleme sürüm 2.3.2</span><span class="sxs-lookup"><span data-stu-id="abe14-810">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="abe14-811">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-811">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="abe14-812">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-812">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-813">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-813">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-814">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-814">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-815">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.css</span><span class="sxs-lookup"><span data-stu-id="abe14-815">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="abe14-816">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-816">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="abe14-817">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="abe14-817">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="abe14-818">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="abe14-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="abe14-819">Önyükleme sürüm 2.3.1</span><span class="sxs-lookup"><span data-stu-id="abe14-819">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="abe14-820">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="abe14-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="abe14-821">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="abe14-822">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="abe14-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="abe14-823">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="abe14-824">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.css</span><span class="sxs-lookup"><span data-stu-id="abe14-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="abe14-825">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.Min.css</span><span class="sxs-lookup"><span data-stu-id="abe14-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="abe14-826">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="abe14-826">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="abe14-827">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="abe14-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="abe14-828">CDN üzerinde önyükleme TouchCarousel sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-828">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="abe14-829">Aşağıdaki sürümleri [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") önyükleme TouchCarousel sürümleri CDN üzerinde barındırılan :</span><span class="sxs-lookup"><span data-stu-id="abe14-829">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="abe14-830">Önyükleme TouchCarousel sürüm 0.8.0</span><span class="sxs-lookup"><span data-stu-id="abe14-830">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="abe14-831">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/CSS/Bootstrap-Touch-carousel.css</span><span class="sxs-lookup"><span data-stu-id="abe14-831">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="abe14-832">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/js/Bootstrap-Touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="abe14-832">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="abe14-833">CDN üzerinde Hammer.js sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-833">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="abe14-834">Aşağıdaki sürümleri [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js sürümleri CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="abe14-834">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="abe14-835">Hammer.js sürüm 2.0.4</span><span class="sxs-lookup"><span data-stu-id="abe14-835">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="abe14-836">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="abe14-836">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="abe14-837">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-837">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="abe14-838">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.Min.map</span><span class="sxs-lookup"><span data-stu-id="abe14-838">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="abe14-839">ASP.NET Web formları ve CDN üzerinde Ajax sürümleri</span><span class="sxs-lookup"><span data-stu-id="abe14-839">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="abe14-840">ASP.NET Ajax kitaplığı aşağıdaki sürümleri üzerinde CDN barındırılır.</span><span class="sxs-lookup"><span data-stu-id="abe14-840">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="abe14-841">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="abe14-841">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="abe14-842">ASP.NET Web Forms ve Ajax sürüm 4.5.2</span><span class="sxs-lookup"><span data-stu-id="abe14-842">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "ASP.NET Web Forms ve Ajax 4.5.2")
- [<span data-ttu-id="abe14-843">ASP.NET Web Forms ve Ajax sürüm 4</span><span class="sxs-lookup"><span data-stu-id="abe14-843">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Forms ve Ajax 4")
- [<span data-ttu-id="abe14-844">ASP.NET Ajax sürüm 3.5</span><span class="sxs-lookup"><span data-stu-id="abe14-844">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="abe14-845">ASP.NET MVC üzerinde CDN serbest bırakır</span><span class="sxs-lookup"><span data-stu-id="abe14-845">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="abe14-846">Aşağıdaki ASP.NET MVC JavaScript dosyaları bu CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="abe14-846">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="abe14-847">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="abe14-847">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="abe14-848">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/JQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="abe14-848">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="abe14-849">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/JQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-849">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="abe14-850">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="abe14-850">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="abe14-851">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/JQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="abe14-851">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="abe14-852">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/JQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-852">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="abe14-853">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="abe14-853">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="abe14-854">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/JQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="abe14-854">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="abe14-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/JQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-855">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="abe14-856">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="abe14-856">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="abe14-857">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/JQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="abe14-857">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="abe14-858">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/JQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-858">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="abe14-859">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="abe14-859">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="abe14-860">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/JQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="abe14-860">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="abe14-861">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/JQuery.unobtrusive-AJAX.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-861">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="abe14-862">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/JQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="abe14-862">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="abe14-863">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/JQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-863">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="abe14-864">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="abe14-864">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="abe14-865">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="abe14-865">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="abe14-866">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="abe14-866">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="abe14-867">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="abe14-867">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="abe14-868">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="abe14-868">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="abe14-869">ASP.NET MVC 1,0</span><span class="sxs-lookup"><span data-stu-id="abe14-869">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="abe14-870">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="abe14-870">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="abe14-871">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="abe14-871">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="abe14-872">ASP.NET SignalR üzerinde CDN serbest bırakır</span><span class="sxs-lookup"><span data-stu-id="abe14-872">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="abe14-873">Aşağıdaki ASP.NET SignalR JavaScript dosyaları bu CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="abe14-873">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="abe14-874">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="abe14-874">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="abe14-875">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.2.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-875">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="abe14-876">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-876">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="abe14-877">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="abe14-877">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="abe14-878">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-878">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="abe14-879">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-879">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="abe14-880">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="abe14-880">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="abe14-881">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-881">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="abe14-882">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="abe14-883">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="abe14-883">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="abe14-884">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-884">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="abe14-885">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="abe14-886">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="abe14-886">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="abe14-887">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-887">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="abe14-888">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="abe14-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="abe14-889">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="abe14-889">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="abe14-890">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-890">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="abe14-891">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="abe14-892">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="abe14-892">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="abe14-893">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-893">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="abe14-894">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="abe14-895">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="abe14-895">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="abe14-896">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-896">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="abe14-897">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="abe14-898">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="abe14-898">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="abe14-899">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-899">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="abe14-900">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="abe14-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="abe14-901">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="abe14-901">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="abe14-902">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-902">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="abe14-903">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="abe14-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="abe14-904">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="abe14-904">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="abe14-905">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-905">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="abe14-906">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="abe14-907">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="abe14-907">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="abe14-908">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-908">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="abe14-909">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="abe14-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="abe14-910">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="abe14-910">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="abe14-911">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.0.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="abe14-911">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="abe14-912">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="abe14-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="abe14-913">CDN kullanım koşulları hakkında daha fazla bilgi için bkz: [Microsoft Ajax CDN Kullanım Koşulları'nı](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Kullanım Koşulları'nı").</span><span class="sxs-lookup"><span data-stu-id="abe14-913">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
