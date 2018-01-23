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
ms.openlocfilehash: f69f707ba64d13fc372b7bc44718c9dcf8cec6e2
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="fa965-102">Microsoft Ajax içerik teslim ağı</span><span class="sxs-lookup"><span data-stu-id="fa965-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="fa965-103">Not: Bir Azure CDN kullanarak özellik hiçbir SLA Microsoft Ajax CDN sahiptir.</span><span class="sxs-lookup"><span data-stu-id="fa965-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="fa965-104">İçindekiler tablosu</span><span class="sxs-lookup"><span data-stu-id="fa965-104">Table of Contents</span></span>

<span data-ttu-id="fa965-105">**[ajax.microsoft.com AJAX.aspnetcdn.com için yeniden adlandırıldı](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="fa965-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="fa965-106">**[Visual Studio .vsdoc desteği](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="fa965-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="fa965-107">**[ASP.NET Ajax CDN kullanma](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="fa965-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="fa965-108">**[CDN jQuery kullanma](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="fa965-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="fa965-109">**[JQuery UI CDN kullanma](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="fa965-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="fa965-110">**[CDN üçüncü taraf dosyaları](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="fa965-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="fa965-111">CDN üzerinde jQuery sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="fa965-112">CDN üzerinde jQuery geçirmek sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="fa965-113">jQuery UI sürümleri CDN üzerinde</span><span class="sxs-lookup"><span data-stu-id="fa965-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="fa965-114">jQuery CDN üzerinde doğrulama sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="fa965-115">jQuery Mobile sürümleri CDN üzerinde</span><span class="sxs-lookup"><span data-stu-id="fa965-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="fa965-116">jQuery CDN üzerinde şablonları sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="fa965-117">jQuery CDN üzerinde döngüsü sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="fa965-118">jQuery CDN üzerinde DataTables sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="fa965-119">CDN üzerinde Modernizr sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="fa965-120">CDN üzerinde JSHint sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="fa965-121">CDN üzerinde Boşaltılan sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="fa965-122">CDN üzerinde sürümleri globalize</span><span class="sxs-lookup"><span data-stu-id="fa965-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="fa965-123">CDN üzerinde sürümleri yanıt</span><span class="sxs-lookup"><span data-stu-id="fa965-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="fa965-124">CDN üzerinde önyükleme sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="fa965-125">CDN üzerinde önyükleme TouchCarousel sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="fa965-126">CDN üzerinde Hammer.js sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="fa965-127">ASP.NET Web formları ve CDN üzerinde Ajax sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="fa965-128">ASP.NET MVC üzerinde CDN serbest bırakır</span><span class="sxs-lookup"><span data-stu-id="fa965-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="fa965-129">ASP.NET SignalR üzerinde CDN serbest bırakır</span><span class="sxs-lookup"><span data-stu-id="fa965-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="fa965-130">Microsoft Ajax içerik teslim ağı (CDN) jQuery gibi popüler üçüncü taraf JavaScript kitaplıklarını barındırır ve bunları Web uygulamalarınızın kolayca eklemenize olanak tanır.</span><span class="sxs-lookup"><span data-stu-id="fa965-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="fa965-131">Örneğin, basitçe ekleyerek bu CDN üzerinde barındırılan jQuery kullanmaya başlayabilmeniz için bir &lt;betik&gt; etiketi sayfanıza ajax.aspnetcdn.com için işaret eder.</span><span class="sxs-lookup"><span data-stu-id="fa965-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="fa965-132">CDN yararlanarak, Ajax uygulamalarınızın performansını önemli ölçüde artırabilir.</span><span class="sxs-lookup"><span data-stu-id="fa965-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="fa965-133">CDN içeriğini tüm dünyada bulunan sunucuları önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="fa965-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="fa965-134">Ayrıca, CDN farklı etki alanlarında bulunan web siteleri için önbelleğe alınan üçüncü taraf JavaScript dosyaları yeniden tarayıcılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="fa965-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="fa965-135">Güvenli Yuva Katmanı'ni kullanarak bir web sayfası hizmet gerekebileceği CDN SSL (HTTPS) destekler.</span><span class="sxs-lookup"><span data-stu-id="fa965-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="fa965-136">CDN yüklenmiş ve size bu kitaplıkları sahibi tarafından lisanslanır aşağıdaki üçüncü taraf betik kitaplıkları barındırır:</span><span class="sxs-lookup"><span data-stu-id="fa965-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="fa965-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="fa965-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="fa965-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="fa965-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="fa965-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="fa965-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="fa965-140">jQuery doğrulama (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="fa965-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="fa965-141">jQuery döngüsü (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="fa965-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="fa965-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="fa965-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="fa965-143">Microsoft Ajax CDN Ayrıca, Microsoft tarafından yüklenen aşağıdaki kitaplıkları içerir:</span><span class="sxs-lookup"><span data-stu-id="fa965-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="fa965-144">ASP.NET Ajax'ı</span><span class="sxs-lookup"><span data-stu-id="fa965-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="fa965-145">ASP.NET MVC JavaScript dosyaları</span><span class="sxs-lookup"><span data-stu-id="fa965-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="fa965-146">ASP.NET SignalR JavaScript dosyaları</span><span class="sxs-lookup"><span data-stu-id="fa965-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="fa965-147">Microsoft, bu CDN üzerinde barındırılan herhangi bir üçüncü taraf kitaplıklar sahipliğini talep değil.</span><span class="sxs-lookup"><span data-stu-id="fa965-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="fa965-148">Telif hakkı sahipleri kitaplıkları, size bu kitaplıklar lisans.</span><span class="sxs-lookup"><span data-stu-id="fa965-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="fa965-149">Karşıdan yükleme ve bu tür kitaplıklarını kullanma gerekebilir herhangi bir hakka yalnızca ilgili telif hakkı sahipleri tarafından verilir.</span><span class="sxs-lookup"><span data-stu-id="fa965-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="fa965-150">Bunlar Microsoft kitaplıkları olduğundan, Microsoft bu CDN üzerinde barındırılan üçüncü taraf kitaplıklar için hiçbir garanti vermez veya fikri mülkiyet hakları lisansları (zımni hiçbir patent hakları dahil) sağlar.</span><span class="sxs-lookup"><span data-stu-id="fa965-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="fa965-151">JavaScript kitaplığı göndermek istiyor ve kitaplığınızın üst (http://trends.builtwith.com üzerinde listelendiği gibi) JavaScript kitaplıklarını veya bu kitaplıkları uzantıları/eklentilerinde biri (a) popüler; veya (b) için faydalı üzerindeki ASP.NET kullanın sonra temasa AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="fa965-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="fa965-152">ajax.microsoft.com AJAX.aspnetcdn.com için yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="fa965-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="fa965-153">CDN microsoft.com etki alanı adını kullanmak için kullanılan ve aspnetcdn.com etki alanı adını kullanmak üzere değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="fa965-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="fa965-154">Bu değişiklik, bir tarayıcı microsoft.com etki alanı başvurulduğunda tüm tanımlama bilgilerini bu etki alanından her istek ile kablo üzerinden göndermeden çünkü performansı artırmak için yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="fa965-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="fa965-155">Microsoft.com dışında bir etki alanı adı için adlandırarak performans çok % 25 olarak artırılabilir.</span><span class="sxs-lookup"><span data-stu-id="fa965-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="fa965-156">Not ajax.microsoft.com çalışmaya devam eder ancak ajax.aspnetcdn.com önerilir.</span><span class="sxs-lookup"><span data-stu-id="fa965-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="fa965-157">Eski biçimi: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="fa965-158">Yeni biçim: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="fa965-159">Visual Studio .vsdoc desteği</span><span class="sxs-lookup"><span data-stu-id="fa965-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="fa965-160">.Vsdoc dosyaları düzgün VS 2008 SP1'e sahip olduğunuzdan emin olmanız gerekir Visual Studio 2008 ile kullanmak için yüklenir ve vsdoc dosyaları düzeltmesinin yüklü.</span><span class="sxs-lookup"><span data-stu-id="fa965-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="fa965-161">Bunlar buradan edinebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fa965-161">You can get these from here:</span></span>

- [<span data-ttu-id="fa965-162">Visual Studio 2008 SP1'i karşıdan</span><span class="sxs-lookup"><span data-stu-id="fa965-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Visual Studio 2008 SP1'i Yükle")
- [<span data-ttu-id="fa965-163">Visual Studio 2008 SP1 için .vsdoc Düzeltme karşıdan</span><span class="sxs-lookup"><span data-stu-id="fa965-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 ".vsdoc düzeltme için Visual Studio 2008 SP1 yükle")

<span data-ttu-id="fa965-164">Visual Studio 2010 ek düzeltme eklerinin olmadan .vsdoc dosyalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="fa965-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="fa965-165">ASP.NET Ajax CDN kullanma</span><span class="sxs-lookup"><span data-stu-id="fa965-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="fa965-166">ASP.NET 4 kullanırken, ASP.NET framework komut dosyaları için tüm istekleri için CDN yönlendirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="fa965-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="fa965-167">Yerel web sunucunuzun yerine CDN komut dosyaları alma önemli ölçüde ortak ASP.NET Web siteleri performansını artırabilir.</span><span class="sxs-lookup"><span data-stu-id="fa965-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="fa965-168">Microsoft Ajax CDN ile tüm ASP.NET framework betik isteklerini yeniden yönlendirmek için ScriptManager EnableCDN özelliğini kullanın:</span><span class="sxs-lookup"><span data-stu-id="fa965-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="fa965-169">CDN jQuery kullanma</span><span class="sxs-lookup"><span data-stu-id="fa965-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="fa965-170">Aşağıdaki betik öğesi bir sayfaya ekleyerek Web uygulamanızda CDN üzerinde barındırılan jQuery betikleri kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="fa965-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="fa965-171">CDN alabileceğiniz jQuery komut dosyasının küçültülmüş sürümünü de içerir. aşağıdaki öğeyi kullanarak:</span><span class="sxs-lookup"><span data-stu-id="fa965-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="fa965-172">CDN kullanılamaz durumda, kendi Web sitesinde bir yerel yol veritabanından jQuery yükleme için geri dönüş sayfanıza izin vermek için hemen CDN başvuran öğesinden sonra aşağıdaki öğeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="fa965-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="fa965-173">Aşağıdaki örnek sayfasına bir düğme tıklatıldığında div öğesinin içeriğini görüntülemek için jQuery kitaplığı (ile yerel bir kopya için geri dönüş) CDN sürümünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="fa965-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="fa965-174">JQuery hakkında daha fazla bilgi ve ziyaret ederek jQuery yerel bir kopyasını indirin [jQuery](http://jquery.com/) Web sitesi.</span><span class="sxs-lookup"><span data-stu-id="fa965-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="fa965-175">JQuery UI CDN kullanma</span><span class="sxs-lookup"><span data-stu-id="fa965-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="fa965-176">CDN jQuery kullanıcı Arabirimi kitaplığı da barındırır.</span><span class="sxs-lookup"><span data-stu-id="fa965-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="fa965-177">JQuery kullanıcı Arabirimi kitaplığı zengin bir pencere öğeleri ve ASP.NET uygulamalarınızın kullanabilirsiniz efektler içerir.</span><span class="sxs-lookup"><span data-stu-id="fa965-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="fa965-178">Örneğin, aşağıdaki sayfayı açılır takvimi görüntülemek için bir ASP.NET Web Forms uygulama bağlamında jQuery UI Datepicker nasıl kullanabileceğiniz gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="fa965-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="fa965-179">Klavyenizi kullanarak metin kutusuna odağı taşıdığınızda, bir takvim görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="fa965-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Oluşturulan DatePicker popup Takvim](overview/_static/image1.png)

<span data-ttu-id="fa965-181">Yukarıdaki kod CDN üç dosyaları içermelidir dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="fa965-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="fa965-182">JQuery Kitaplığı &mdash; jQuery kullanıcı Arabirimi kitaplığı jQuery kitaplıkta bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="fa965-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="fa965-183">JQuery kullanıcı Arabirimi kitaplığı eklemeden önce jQuery kitaplığı sayfanıza eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="fa965-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="fa965-184">JQuery kullanıcı Arabirimi Kitaplığı &mdash; jQuery kullanıcı Arabirimi kitaplığı tüm jQuery UI etkiler ve yukarıdaki sayfa kullanılan Datepicker pencere gibi pencere öğeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="fa965-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="fa965-185">JQuery UI tema &mdash; farklı temaları jQuery UI destekler.</span><span class="sxs-lookup"><span data-stu-id="fa965-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="fa965-186">Yukarıdaki sayfa Redmond temayı içeri aktarmak için bir CSS dosyası için bir bağlantı içerir.</span><span class="sxs-lookup"><span data-stu-id="fa965-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="fa965-187">Tüm standart jQuery UI temaları CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="fa965-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="fa965-188">[Bu sayfayı ziyaret](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 Microsoft Ajax CDN üzerinde") her tema için küçük resimler görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="fa965-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="fa965-189">JQuery kullanıcı Arabirimi Kitaplığı hakkında daha fazla bilgi edinmek için resmi ziyaret [jQuery UI Web sitesi](http://jQueryUI.com "jQuery UI Web sitesi").</span><span class="sxs-lookup"><span data-stu-id="fa965-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="fa965-190">CDN üçüncü taraf dosyaları</span><span class="sxs-lookup"><span data-stu-id="fa965-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="fa965-191">CDN en popüler üçüncü taraf JavaScript kitaplıklarını bazıları barındırır.</span><span class="sxs-lookup"><span data-stu-id="fa965-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="fa965-192">Microsoft, bu CDN üzerinde barındırılan herhangi bir üçüncü taraf kitaplıklar sahipliğini talep değil.</span><span class="sxs-lookup"><span data-stu-id="fa965-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="fa965-193">Telif hakkı sahipleri kitaplıkları, size bu kitaplıklar lisans.</span><span class="sxs-lookup"><span data-stu-id="fa965-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="fa965-194">Karşıdan yükleme ve bu tür kitaplıklarını kullanma gerekebilir herhangi bir hakka yalnızca ilgili telif hakkı sahipleri tarafından verilir.</span><span class="sxs-lookup"><span data-stu-id="fa965-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="fa965-195">Bunlar Microsoft kitaplıkları olduğundan, Microsoft bu CDN üzerinde barındırılan üçüncü taraf kitaplıklar için hiçbir garanti vermez veya fikri mülkiyet hakları lisansları (zımni hiçbir patent hakları dahil) sağlar.</span><span class="sxs-lookup"><span data-stu-id="fa965-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="fa965-196">CDN üzerinde jQuery sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="fa965-197">JQuery aşağıdaki sürümleri CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="fa965-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="fa965-198">jQuery sürüm 3.3.1</span><span class="sxs-lookup"><span data-stu-id="fa965-198">jQuery version 3.3.1</span></span>
- <span data-ttu-id="fa965-199">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.3.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js</span></span>
- <span data-ttu-id="fa965-200">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.3.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js</span></span>
- <span data-ttu-id="fa965-201">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.3.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map</span></span>
- <span data-ttu-id="fa965-202">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.3.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="fa965-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js</span></span>
- <span data-ttu-id="fa965-203">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.3.1.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js</span></span>
- <span data-ttu-id="fa965-204">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.3.1.Slim.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="fa965-205">jQuery sürüm 3.2.1</span><span class="sxs-lookup"><span data-stu-id="fa965-205">jQuery version 3.2.1</span></span>
- <span data-ttu-id="fa965-206">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="fa965-207">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="fa965-208">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="fa965-209">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="fa965-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="fa965-210">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.1.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="fa965-211">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.1.Slim.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="fa965-212">jQuery sürüm 3.2.0</span><span class="sxs-lookup"><span data-stu-id="fa965-212">jQuery version 3.2.0</span></span>

- <span data-ttu-id="fa965-213">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="fa965-214">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="fa965-215">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="fa965-216">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="fa965-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="fa965-217">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.0.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="fa965-218">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.2.0.Slim.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="fa965-219">jQuery sürüm 3.1.1</span><span class="sxs-lookup"><span data-stu-id="fa965-219">jQuery version 3.1.1</span></span>

- <span data-ttu-id="fa965-220">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="fa965-221">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="fa965-222">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="fa965-223">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="fa965-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="fa965-224">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.1.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="fa965-225">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.1.Slim.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="fa965-226">jQuery sürüm 3.1.0</span><span class="sxs-lookup"><span data-stu-id="fa965-226">jQuery version 3.1.0</span></span>

- <span data-ttu-id="fa965-227">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="fa965-228">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="fa965-229">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="fa965-230">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="fa965-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="fa965-231">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.0.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="fa965-232">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.1.0.Slim.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="fa965-233">jQuery sürüm 3.0.0</span><span class="sxs-lookup"><span data-stu-id="fa965-233">jQuery version 3.0.0</span></span>

- <span data-ttu-id="fa965-234">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="fa965-235">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="fa965-236">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.0.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="fa965-237">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.0.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="fa965-237">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="fa965-238">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.0.0.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="fa965-239">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-3.0.0.Slim.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="fa965-240">jQuery sürüm 2.2.4</span><span class="sxs-lookup"><span data-stu-id="fa965-240">jQuery version 2.2.4</span></span>

- <span data-ttu-id="fa965-241">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="fa965-241">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="fa965-242">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="fa965-243">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.4.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="fa965-244">jQuery sürüm 2.2.3</span><span class="sxs-lookup"><span data-stu-id="fa965-244">jQuery version 2.2.3</span></span>

- <span data-ttu-id="fa965-245">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="fa965-245">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="fa965-246">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="fa965-247">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.3.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="fa965-248">jQuery sürüm 2.2.2</span><span class="sxs-lookup"><span data-stu-id="fa965-248">jQuery version 2.2.2</span></span>

- <span data-ttu-id="fa965-249">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-249">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="fa965-250">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="fa965-251">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.2.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="fa965-252">jQuery sürüm 2.2.1</span><span class="sxs-lookup"><span data-stu-id="fa965-252">jQuery version 2.2.1</span></span>

- <span data-ttu-id="fa965-253">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-253">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="fa965-254">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="fa965-255">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="fa965-256">jQuery sürüm 2.2.0</span><span class="sxs-lookup"><span data-stu-id="fa965-256">jQuery version 2.2.0</span></span>

- <span data-ttu-id="fa965-257">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-257">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="fa965-258">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="fa965-259">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.2.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="fa965-260">jQuery sürüm 2.1.4</span><span class="sxs-lookup"><span data-stu-id="fa965-260">jQuery version 2.1.4</span></span>

- <span data-ttu-id="fa965-261">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="fa965-261">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="fa965-262">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="fa965-263">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.4.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="fa965-264">jQuery sürüm 2.1.3</span><span class="sxs-lookup"><span data-stu-id="fa965-264">jQuery version 2.1.3</span></span>

- <span data-ttu-id="fa965-265">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="fa965-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="fa965-266">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="fa965-267">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.3.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="fa965-268">jQuery sürüm 2.1.2</span><span class="sxs-lookup"><span data-stu-id="fa965-268">jQuery version 2.1.2</span></span>

- <span data-ttu-id="fa965-269">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="fa965-270">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="fa965-271">jQuery sürüm 2.1.1</span><span class="sxs-lookup"><span data-stu-id="fa965-271">jQuery version 2.1.1</span></span>

- <span data-ttu-id="fa965-272">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="fa965-273">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-273">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="fa965-274">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="fa965-275">jQuery sürüm 2.1.0</span><span class="sxs-lookup"><span data-stu-id="fa965-275">jQuery version 2.1.0</span></span>

- <span data-ttu-id="fa965-276">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="fa965-277">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="fa965-278">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-278">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="fa965-279">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.1.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="fa965-280">jQuery sürüm 2.0.3</span><span class="sxs-lookup"><span data-stu-id="fa965-280">jQuery version 2.0.3</span></span>

- <span data-ttu-id="fa965-281">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="fa965-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="fa965-282">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="fa965-283">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-283">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="fa965-284">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.3.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="fa965-285">jQuery sürüm 2.0.2</span><span class="sxs-lookup"><span data-stu-id="fa965-285">jQuery version 2.0.2</span></span>

- <span data-ttu-id="fa965-286">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="fa965-287">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="fa965-288">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-288">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="fa965-289">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.2.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="fa965-290">jQuery sürüm 2.0.1</span><span class="sxs-lookup"><span data-stu-id="fa965-290">jQuery version 2.0.1</span></span>

- <span data-ttu-id="fa965-291">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="fa965-292">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="fa965-293">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-293">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="fa965-294">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="fa965-295">jQuery sürüm 2.0.0</span><span class="sxs-lookup"><span data-stu-id="fa965-295">jQuery version 2.0.0</span></span>

- <span data-ttu-id="fa965-296">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="fa965-297">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-297">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="fa965-298">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="fa965-299">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-2.0.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="fa965-300">jQuery sürüm 1.12.4</span><span class="sxs-lookup"><span data-stu-id="fa965-300">jQuery version 1.12.4</span></span>

- <span data-ttu-id="fa965-301">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="fa965-301">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="fa965-302">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="fa965-303">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.4.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="fa965-304">jQuery sürüm 1.12.3</span><span class="sxs-lookup"><span data-stu-id="fa965-304">jQuery version 1.12.3</span></span>

- <span data-ttu-id="fa965-305">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="fa965-305">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="fa965-306">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="fa965-307">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.3.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="fa965-308">jQuery sürüm 1.12.2</span><span class="sxs-lookup"><span data-stu-id="fa965-308">jQuery version 1.12.2</span></span>

- <span data-ttu-id="fa965-309">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-309">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="fa965-310">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="fa965-311">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.2.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="fa965-312">jQuery sürüm 1.12.1</span><span class="sxs-lookup"><span data-stu-id="fa965-312">jQuery version 1.12.1</span></span>

- <span data-ttu-id="fa965-313">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-313">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="fa965-314">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="fa965-315">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="fa965-316">jQuery sürüm 1.12.0</span><span class="sxs-lookup"><span data-stu-id="fa965-316">jQuery version 1.12.0</span></span>

- <span data-ttu-id="fa965-317">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-317">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="fa965-318">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="fa965-319">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.12.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="fa965-320">jQuery sürüm 1.11.3</span><span class="sxs-lookup"><span data-stu-id="fa965-320">jQuery version 1.11.3</span></span>

- <span data-ttu-id="fa965-321">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="fa965-321">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="fa965-322">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="fa965-323">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.3.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="fa965-324">jQuery sürüm 1.11.2</span><span class="sxs-lookup"><span data-stu-id="fa965-324">jQuery version 1.11.2</span></span>

- <span data-ttu-id="fa965-325">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-325">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="fa965-326">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="fa965-327">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.2.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="fa965-328">jQuery sürüm 1.11.1</span><span class="sxs-lookup"><span data-stu-id="fa965-328">jQuery version 1.11.1</span></span>

- <span data-ttu-id="fa965-329">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="fa965-330">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-330">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="fa965-331">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="fa965-332">jQuery sürüm 1.11.0</span><span class="sxs-lookup"><span data-stu-id="fa965-332">jQuery version 1.11.0</span></span>

- <span data-ttu-id="fa965-333">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="fa965-334">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="fa965-335">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-335">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="fa965-336">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.11.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="fa965-337">jQuery sürüm 1.10.2</span><span class="sxs-lookup"><span data-stu-id="fa965-337">jQuery version 1.10.2</span></span>

- <span data-ttu-id="fa965-338">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="fa965-339">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="fa965-340">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-340">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="fa965-341">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.2.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="fa965-342">jQuery sürüm 1.10.1</span><span class="sxs-lookup"><span data-stu-id="fa965-342">jQuery version 1.10.1</span></span>

- <span data-ttu-id="fa965-343">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="fa965-344">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="fa965-345">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-345">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="fa965-346">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="fa965-347">jQuery sürüm 1.10.0</span><span class="sxs-lookup"><span data-stu-id="fa965-347">jQuery version 1.10.0</span></span>

- <span data-ttu-id="fa965-348">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="fa965-349">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="fa965-350">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-350">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="fa965-351">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.10.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="fa965-352">jQuery sürüm 1.9.1</span><span class="sxs-lookup"><span data-stu-id="fa965-352">jQuery version 1.9.1</span></span>

- <span data-ttu-id="fa965-353">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="fa965-354">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="fa965-355">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-355">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="fa965-356">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.1.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="fa965-357">jQuery sürüm 1.9.0</span><span class="sxs-lookup"><span data-stu-id="fa965-357">jQuery version 1.9.0</span></span>

- <span data-ttu-id="fa965-358">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="fa965-359">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-359">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="fa965-360">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="fa965-361">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.9.0.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="fa965-362">jQuery sürüm 1.8.3</span><span class="sxs-lookup"><span data-stu-id="fa965-362">jQuery version 1.8.3</span></span>

- <span data-ttu-id="fa965-363">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="fa965-363">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="fa965-364">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="fa965-365">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="fa965-366">jQuery sürüm 1.8.2</span><span class="sxs-lookup"><span data-stu-id="fa965-366">jQuery version 1.8.2</span></span>

- <span data-ttu-id="fa965-367">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-367">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="fa965-368">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="fa965-369">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="fa965-370">jQuery sürüm 1.8.1</span><span class="sxs-lookup"><span data-stu-id="fa965-370">jQuery version 1.8.1</span></span>

- <span data-ttu-id="fa965-371">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-371">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="fa965-372">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="fa965-373">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="fa965-374">jQuery sürüm 1.8.0</span><span class="sxs-lookup"><span data-stu-id="fa965-374">jQuery version 1.8.0</span></span>

- <span data-ttu-id="fa965-375">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="fa965-376">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="fa965-377">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="fa965-378">jQuery sürüm 1.7.2</span><span class="sxs-lookup"><span data-stu-id="fa965-378">jQuery version 1.7.2</span></span>

- <span data-ttu-id="fa965-379">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="fa965-380">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="fa965-381">jQuery sürüm 1.7.1</span><span class="sxs-lookup"><span data-stu-id="fa965-381">jQuery version 1.7.1</span></span>

- <span data-ttu-id="fa965-382">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-382">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="fa965-383">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="fa965-384">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="fa965-385">jQuery sürüm 1.7</span><span class="sxs-lookup"><span data-stu-id="fa965-385">jQuery version 1.7</span></span>

- <span data-ttu-id="fa965-386">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="fa965-386">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="fa965-387">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="fa965-388">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="fa965-389">jQuery sürüm 1.6.4</span><span class="sxs-lookup"><span data-stu-id="fa965-389">jQuery version 1.6.4</span></span>

- <span data-ttu-id="fa965-390">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="fa965-390">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="fa965-391">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="fa965-392">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="fa965-393">jQuery sürüm 1.6.3</span><span class="sxs-lookup"><span data-stu-id="fa965-393">jQuery version 1.6.3</span></span>

- <span data-ttu-id="fa965-394">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="fa965-394">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="fa965-395">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="fa965-396">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="fa965-397">jQuery sürüm 1.6.2</span><span class="sxs-lookup"><span data-stu-id="fa965-397">jQuery version 1.6.2</span></span>

- <span data-ttu-id="fa965-398">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-398">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="fa965-399">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="fa965-400">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="fa965-401">jQuery sürüm 1.6.1</span><span class="sxs-lookup"><span data-stu-id="fa965-401">jQuery version 1.6.1</span></span>

- <span data-ttu-id="fa965-402">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-402">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="fa965-403">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="fa965-404">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="fa965-405">jQuery sürüm 1.6</span><span class="sxs-lookup"><span data-stu-id="fa965-405">jQuery version 1.6</span></span>

- <span data-ttu-id="fa965-406">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="fa965-406">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="fa965-407">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="fa965-408">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="fa965-409">jQuery sürüm 1.5.2</span><span class="sxs-lookup"><span data-stu-id="fa965-409">jQuery version 1.5.2</span></span>

- <span data-ttu-id="fa965-410">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-410">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="fa965-411">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="fa965-412">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="fa965-413">jQuery sürüm 1.5.1</span><span class="sxs-lookup"><span data-stu-id="fa965-413">jQuery version 1.5.1</span></span>

- <span data-ttu-id="fa965-414">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-414">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="fa965-415">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="fa965-416">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="fa965-417">jQuery sürüm 1.5</span><span class="sxs-lookup"><span data-stu-id="fa965-417">jQuery version 1.5</span></span>

- <span data-ttu-id="fa965-418">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="fa965-418">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="fa965-419">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="fa965-420">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="fa965-421">jQuery sürüm 1.4.4</span><span class="sxs-lookup"><span data-stu-id="fa965-421">jQuery version 1.4.4</span></span>

- <span data-ttu-id="fa965-422">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="fa965-422">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="fa965-423">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="fa965-424">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="fa965-425">jQuery sürüm 1.4.3</span><span class="sxs-lookup"><span data-stu-id="fa965-425">jQuery version 1.4.3</span></span>

- <span data-ttu-id="fa965-426">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="fa965-426">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="fa965-427">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="fa965-428">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="fa965-429">jQuery sürüm 1.4.2</span><span class="sxs-lookup"><span data-stu-id="fa965-429">jQuery version 1.4.2</span></span>

- <span data-ttu-id="fa965-430">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-430">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="fa965-431">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="fa965-432">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="fa965-433">jQuery sürüm 1.4.1</span><span class="sxs-lookup"><span data-stu-id="fa965-433">jQuery version 1.4.1</span></span>

- <span data-ttu-id="fa965-434">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="fa965-435">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="fa965-436">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="fa965-437">jQuery sürüm 1.4</span><span class="sxs-lookup"><span data-stu-id="fa965-437">jQuery version 1.4</span></span>

- <span data-ttu-id="fa965-438">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="fa965-438">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="fa965-439">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-439">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="fa965-440">jQuery sürüm 1.3.2</span><span class="sxs-lookup"><span data-stu-id="fa965-440">jQuery version 1.3.2</span></span>

- <span data-ttu-id="fa965-441">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-441">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="fa965-442">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.3.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-442">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="fa965-443">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-443">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="fa965-444">http://AJAX.aspnetcdn.com/AJAX/JQuery/JQuery-1.3.2.Min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="fa965-444">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="fa965-445">CDN üzerinde jQuery geçirmek sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-445">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="fa965-446">JQuery geçirme aşağıdaki sürümleri CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="fa965-446">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="fa965-447">jQuery sürüm 3.0.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="fa965-447">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="fa965-448">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="fa965-449">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-3.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-449">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="fa965-450">jQuery geçirme 1.2.1 sürümü</span><span class="sxs-lookup"><span data-stu-id="fa965-450">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="fa965-451">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="fa965-452">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-452">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="fa965-453">jQuery sürüm 1.2.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="fa965-453">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="fa965-454">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="fa965-455">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-455">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="fa965-456">jQuery sürüm 1.1.1 geçirme</span><span class="sxs-lookup"><span data-stu-id="fa965-456">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="fa965-457">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="fa965-458">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-458">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="fa965-459">jQuery sürüm 1.1.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="fa965-459">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="fa965-460">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-460">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="fa965-461">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-461">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="fa965-462">jQuery sürümü 1.0.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="fa965-462">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="fa965-463">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-463">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="fa965-464">http://AJAX.aspnetcdn.com/AJAX/JQuery.Migrate/JQuery-Migrate-1.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-464">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="fa965-465">jQuery UI sürümleri CDN üzerinde</span><span class="sxs-lookup"><span data-stu-id="fa965-465">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="fa965-466">JQuery kullanıcı Arabirimi kitaplığı aşağıdaki sürümleri, bu CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="fa965-466">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="fa965-467">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fa965-467">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="fa965-468">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="fa965-468">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-469">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="fa965-469">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-470">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="fa965-470">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-471">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="fa965-471">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-472">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="fa965-472">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-473">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="fa965-473">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-474">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="fa965-474">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-475">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="fa965-475">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-476">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="fa965-476">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-477">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="fa965-477">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2 UI Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-478">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="fa965-478">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-479">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="fa965-479">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-480">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="fa965-480">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-481">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="fa965-481">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-482">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="fa965-482">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-483">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="fa965-483">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-484">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="fa965-484">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-485">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="fa965-485">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-486">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="fa965-486">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-487">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="fa965-487">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-488">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="fa965-488">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-489">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="fa965-489">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-490">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="fa965-490">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-491">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="fa965-491">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-492">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="fa965-492">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-493">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="fa965-493">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-494">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="fa965-494">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-495">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="fa965-495">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-496">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="fa965-496">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-497">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="fa965-497">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-498">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="fa965-498">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-499">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="fa965-499">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-500">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="fa965-500">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-501">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="fa965-501">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-502">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="fa965-502">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="fa965-503">jQuery CDN üzerinde doğrulama sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-503">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="fa965-504">JQuery doğrulama kitaplığı aşağıdaki sürümleri, bu CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="fa965-504">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="fa965-505">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fa965-505">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="fa965-506">jQuery doğrulama 1.17.0</span><span class="sxs-lookup"><span data-stu-id="fa965-506">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery doğrulama 1.17.0")
- [<span data-ttu-id="fa965-507">jQuery doğrulama 1.16.0</span><span class="sxs-lookup"><span data-stu-id="fa965-507">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery doğrulama 1.16.0")
- [<span data-ttu-id="fa965-508">jQuery doğrulama 1.15.1</span><span class="sxs-lookup"><span data-stu-id="fa965-508">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery doğrulama 1.15.1")
- [<span data-ttu-id="fa965-509">jQuery doğrulama 1.15.0</span><span class="sxs-lookup"><span data-stu-id="fa965-509">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery doğrulama 1.15.0")
- [<span data-ttu-id="fa965-510">jQuery doğrulama 1.14.0</span><span class="sxs-lookup"><span data-stu-id="fa965-510">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery doğrulama 1.14.0")
- [<span data-ttu-id="fa965-511">jQuery doğrulama 1.13.1</span><span class="sxs-lookup"><span data-stu-id="fa965-511">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery doğrulama 1.13.1")
- [<span data-ttu-id="fa965-512">jQuery doğrulama 1.13.0</span><span class="sxs-lookup"><span data-stu-id="fa965-512">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery doğrulama 1.13.0")
- [<span data-ttu-id="fa965-513">jQuery doğrulama 1.12.0</span><span class="sxs-lookup"><span data-stu-id="fa965-513">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery doğrulama 1.12.0")
- [<span data-ttu-id="fa965-514">jQuery doğrulama 1.11.1</span><span class="sxs-lookup"><span data-stu-id="fa965-514">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery doğrulama 1.11.1")
- [<span data-ttu-id="fa965-515">jQuery doğrulama 1.11.0</span><span class="sxs-lookup"><span data-stu-id="fa965-515">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery doğrulama 1.11.0")
- [<span data-ttu-id="fa965-516">jQuery doğrulama 1.10.0</span><span class="sxs-lookup"><span data-stu-id="fa965-516">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery doğrulama 1.10.0")
- [<span data-ttu-id="fa965-517">jQuery doğrulama 1.9</span><span class="sxs-lookup"><span data-stu-id="fa965-517">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate sürüm 1.9")
- [<span data-ttu-id="fa965-518">jQuery doğrulama 1.8.1</span><span class="sxs-lookup"><span data-stu-id="fa965-518">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate sürüm 1.8.1")
- [<span data-ttu-id="fa965-519">jQuery doğrulama 1.8</span><span class="sxs-lookup"><span data-stu-id="fa965-519">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate sürüm 1,8")
- [<span data-ttu-id="fa965-520">jQuery doğrulama 1.7</span><span class="sxs-lookup"><span data-stu-id="fa965-520">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate sürüm 1.7")
- [<span data-ttu-id="fa965-521">jQuery doğrulama 1.6</span><span class="sxs-lookup"><span data-stu-id="fa965-521">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery doğrulama 1.6")
- [<span data-ttu-id="fa965-522">jQuery doğrulama 1.5.5</span><span class="sxs-lookup"><span data-stu-id="fa965-522">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery doğrulama 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="fa965-523">jQuery Mobile sürümleri CDN üzerinde</span><span class="sxs-lookup"><span data-stu-id="fa965-523">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="fa965-524">JQuery Mobile kitaplığı aşağıdaki sürümleri, bu CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="fa965-524">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="fa965-525">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fa965-525">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="fa965-526">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="fa965-526">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-527">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="fa965-527">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-528">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="fa965-528">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-529">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="fa965-529">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-530">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="fa965-530">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-531">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="fa965-531">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-532">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="fa965-532">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-533">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="fa965-533">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-534">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="fa965-534">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-535">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="fa965-535">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-536">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="fa965-536">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-537">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="fa965-537">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 için Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-538">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="fa965-538">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-539">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="fa965-539">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 Microsoft Ajax CDN")
- [<span data-ttu-id="fa965-540">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="fa965-540">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 için Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-541">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="fa965-541">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 için Microsoft Ajax CDN üzerinde")
- [<span data-ttu-id="fa965-542">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="fa965-542">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.0 Beta 3")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="fa965-543">jQuery CDN üzerinde şablonları sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-543">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="fa965-544">JQuery şablonları eklentisi aşağıdaki sürümleri, bu CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="fa965-544">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="fa965-545">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fa965-545">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="fa965-546">jQuery şablonları Beta 1</span><span class="sxs-lookup"><span data-stu-id="fa965-546">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery şablonları Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="fa965-547">jQuery CDN üzerinde döngüsü sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-547">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="fa965-548">JQuery döngüsü eklentisi aşağıdaki sürümleri, bu CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="fa965-548">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="fa965-549">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fa965-549">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="fa965-550">jQuery döngüsü 2.99</span><span class="sxs-lookup"><span data-stu-id="fa965-550">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery döngüsü 2.99")
- [<span data-ttu-id="fa965-551">jQuery döngüsü 2.94</span><span class="sxs-lookup"><span data-stu-id="fa965-551">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery döngüsü 2.94")
- [<span data-ttu-id="fa965-552">jQuery döngüsü 2.88</span><span class="sxs-lookup"><span data-stu-id="fa965-552">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery döngüsü 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="fa965-553">jQuery CDN üzerinde DataTables sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-553">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="fa965-554">JQuery DataTables eklentisi aşağıdaki sürümleri, bu CDN üzerinde barındırılır.</span><span class="sxs-lookup"><span data-stu-id="fa965-554">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="fa965-555">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fa965-555">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="fa965-556">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="fa965-556">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="fa965-557">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="fa965-557">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="fa965-558">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="fa965-558">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="fa965-559">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="fa965-559">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="fa965-560">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="fa965-560">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="fa965-561">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="fa965-561">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="fa965-562">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="fa965-562">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="fa965-563">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="fa965-563">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="fa965-564">CDN üzerinde Modernizr sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-564">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="fa965-565">Aşağıdaki sürümleri [Modernizr](http://www.modernizr.com "Modernizr") CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="fa965-565">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="fa965-566">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="fa965-566">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="fa965-567">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-567">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="fa965-568">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-568">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="fa965-569">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-569">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="fa965-570">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="fa965-570">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="fa965-571">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="fa965-571">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="fa965-572">CDN üzerinde JSHint sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-572">JSHint Releases on the CDN</span></span>

<span data-ttu-id="fa965-573">Aşağıdaki sürümleri [JSHint](http://www.jshint.com "JSHint") CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="fa965-573">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="fa965-574">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="fa965-574">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="fa965-575">CDN üzerinde Boşaltılan sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-575">Knockout Releases on the CDN</span></span>

<span data-ttu-id="fa965-576">Aşağıdaki sürümleri [Boşaltılan](http://www.knockoutjs.com "Boşaltılan") CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="fa965-576">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="fa965-577">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="fa965-578">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="fa965-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="fa965-579">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="fa965-580">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="fa965-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="fa965-581">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="fa965-582">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="fa965-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="fa965-583">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="fa965-584">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="fa965-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="fa965-585">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="fa965-586">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="fa965-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="fa965-587">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="fa965-588">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="fa965-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="fa965-589">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="fa965-590">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="fa965-590">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="fa965-591">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-591">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="fa965-592">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="fa965-592">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="fa965-593">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-593">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="fa965-594">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="fa965-594">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="fa965-595">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-595">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="fa965-596">http://AJAX.aspnetcdn.com/AJAX/knockout/knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="fa965-596">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="fa965-597">CDN üzerinde sürümleri globalize</span><span class="sxs-lookup"><span data-stu-id="fa965-597">Globalize Releases on the CDN</span></span>

<span data-ttu-id="fa965-598">Aşağıdaki sürümleri [Globalize](https://github.com/jquery/globalize "Globalize") CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="fa965-598">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="fa965-599">Sürümü 1.0.0 globalize</span><span class="sxs-lookup"><span data-stu-id="fa965-599">Globalize version 1.0.0</span></span>

- <span data-ttu-id="fa965-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="fa965-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="fa965-601">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/node-Main.js</span><span class="sxs-lookup"><span data-stu-id="fa965-601">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="fa965-602">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="fa965-602">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="fa965-603">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Date.js</span><span class="sxs-lookup"><span data-stu-id="fa965-603">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="fa965-604">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="fa965-604">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="fa965-605">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="fa965-605">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="fa965-606">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/plural.js</span><span class="sxs-lookup"><span data-stu-id="fa965-606">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="fa965-607">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Relative-Time.js</span><span class="sxs-lookup"><span data-stu-id="fa965-607">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="fa965-608">Sürüm 0.1.1 globalize</span><span class="sxs-lookup"><span data-stu-id="fa965-608">Globalize version 0.1.1</span></span>

- <span data-ttu-id="fa965-609">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-609">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="fa965-610">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="fa965-610">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="fa965-611">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.cultures.js</span><span class="sxs-lookup"><span data-stu-id="fa965-611">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="fa965-612">tüm kültürler</span><span class="sxs-lookup"><span data-stu-id="fa965-612">all cultures</span></span>
- <span data-ttu-id="fa965-613">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/cultures/globalize.culture. {kültür kodu} .js</span><span class="sxs-lookup"><span data-stu-id="fa965-613">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="fa965-614">"{Kodu kültür}" istenen kültürü kod ile değiştirin, örneğin globalize.culture.en GB.js== Microsoft CDN üzerinde dosyaları bu == kitaplıkları, Microsoft tarafından karşıya.</span><span class="sxs-lookup"><span data-stu-id="fa965-614">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="fa965-615">CDN üzerinde sürümleri yanıt</span><span class="sxs-lookup"><span data-stu-id="fa965-615">Respond Releases on the CDN</span></span>

<span data-ttu-id="fa965-616">Aşağıdaki sürümleri [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") yanıt CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="fa965-616">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="fa965-617">Sürüm 1.4.2 yanıt</span><span class="sxs-lookup"><span data-stu-id="fa965-617">Respond version 1.4.2</span></span>

- <span data-ttu-id="fa965-618">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.js</span><span class="sxs-lookup"><span data-stu-id="fa965-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="fa965-619">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="fa965-620">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="fa965-620">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="fa965-621">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.addListener.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="fa965-622">Sürüm 1.4.1 yanıt</span><span class="sxs-lookup"><span data-stu-id="fa965-622">Respond version 1.4.1</span></span>

- <span data-ttu-id="fa965-623">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.js</span><span class="sxs-lookup"><span data-stu-id="fa965-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="fa965-624">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="fa965-625">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="fa965-625">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="fa965-626">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.addListener.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-626">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="fa965-627">Sürüm 1.4.0 yanıt</span><span class="sxs-lookup"><span data-stu-id="fa965-627">Respond version 1.4.0</span></span>

- <span data-ttu-id="fa965-628">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="fa965-628">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="fa965-629">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-629">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="fa965-630">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.js</span><span class="sxs-lookup"><span data-stu-id="fa965-630">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="fa965-631">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.addListener.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-631">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="fa965-632">Sürüm 1.3.0 yanıt</span><span class="sxs-lookup"><span data-stu-id="fa965-632">Respond version 1.3.0</span></span>

- <span data-ttu-id="fa965-633">http://AJAX.aspnetcdn.com/AJAX/Respond/1.3.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="fa965-633">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="fa965-634">Sürüm 1.2.0 yanıt</span><span class="sxs-lookup"><span data-stu-id="fa965-634">Respond version 1.2.0</span></span>

- <span data-ttu-id="fa965-635">http://AJAX.aspnetcdn.com/AJAX/Respond/1.2.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="fa965-635">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="fa965-636">CDN üzerinde önyükleme sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-636">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="fa965-637">Aşağıdaki sürümleri [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") önyükleme CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="fa965-637">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="fa965-638">Önyükleme sürüm 3.3.7</span><span class="sxs-lookup"><span data-stu-id="fa965-638">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="fa965-639">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="fa965-640">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-641">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-642">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="fa965-643">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-644">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-645">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-645">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="fa965-646">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-647">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-648">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-649">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-650">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="fa965-651">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="fa965-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="fa965-652">Önyükleme sürüm 3.3.6</span><span class="sxs-lookup"><span data-stu-id="fa965-652">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="fa965-653">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="fa965-654">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-655">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-656">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="fa965-657">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-658">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-659">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-659">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="fa965-660">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-661">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-662">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-663">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-664">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="fa965-665">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="fa965-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="fa965-666">Önyükleme sürüm 3.3.5</span><span class="sxs-lookup"><span data-stu-id="fa965-666">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="fa965-667">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="fa965-668">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-669">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-670">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="fa965-671">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-672">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-673">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-673">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="fa965-674">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-675">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-676">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-677">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-678">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="fa965-679">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="fa965-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="fa965-680">Önyükleme sürüm 3.3.4</span><span class="sxs-lookup"><span data-stu-id="fa965-680">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="fa965-681">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="fa965-682">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-683">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-684">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="fa965-685">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-686">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-687">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-687">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="fa965-688">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-689">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-690">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-691">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-692">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="fa965-693">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="fa965-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="fa965-694">Önyükleme sürüm 3.3.2</span><span class="sxs-lookup"><span data-stu-id="fa965-694">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="fa965-695">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="fa965-696">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-697">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-698">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="fa965-699">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-700">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-701">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-701">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="fa965-702">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-703">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-704">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-705">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-706">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="fa965-707">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="fa965-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="fa965-708">Önyükleme sürüm 3.3.1</span><span class="sxs-lookup"><span data-stu-id="fa965-708">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="fa965-709">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="fa965-710">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-711">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-712">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="fa965-713">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-714">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-714">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-715">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="fa965-716">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-717">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-718">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-719">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-720">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="fa965-721">Önyükleme sürüm 3.3.0</span><span class="sxs-lookup"><span data-stu-id="fa965-721">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="fa965-722">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="fa965-723">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-724">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-725">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="fa965-726">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-727">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-727">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-728">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="fa965-729">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-730">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-731">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-732">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-733">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="fa965-734">Önyükleme sürüm 3.2.0</span><span class="sxs-lookup"><span data-stu-id="fa965-734">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="fa965-735">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="fa965-736">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-737">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-738">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="fa965-739">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-740">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-740">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-741">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="fa965-742">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-743">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-744">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-745">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-746">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="fa965-747">Önyükleme sürüm 3.1.1</span><span class="sxs-lookup"><span data-stu-id="fa965-747">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="fa965-748">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="fa965-749">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-750">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-751">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="fa965-752">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-753">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-753">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-754">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="fa965-755">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-756">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-757">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-758">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-759">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="fa965-760">Önyükleme sürüm 3.1.0</span><span class="sxs-lookup"><span data-stu-id="fa965-760">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="fa965-761">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="fa965-762">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-763">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-764">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="fa965-765">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-766">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-766">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-767">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-theme.css.map</span><span class="sxs-lookup"><span data-stu-id="fa965-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="fa965-768">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-769">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-770">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-771">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-772">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="fa965-773">Önyükleme sürüm 3.0.3</span><span class="sxs-lookup"><span data-stu-id="fa965-773">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="fa965-774">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="fa965-775">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-776">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-777">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-777">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-778">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-779">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-780">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-781">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-782">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-783">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="fa965-784">Önyükleme sürüm 3.0.2</span><span class="sxs-lookup"><span data-stu-id="fa965-784">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="fa965-785">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="fa965-786">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-787">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-788">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-788">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-789">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-790">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-791">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-792">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-793">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-794">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="fa965-795">Önyükleme sürüm 3.0.1</span><span class="sxs-lookup"><span data-stu-id="fa965-795">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="fa965-796">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="fa965-797">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-798">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-799">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-799">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-800">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-801">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-802">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-803">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-804">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-805">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="fa965-806">Önyükleme sürüm 3.0.0</span><span class="sxs-lookup"><span data-stu-id="fa965-806">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="fa965-807">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="fa965-808">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-809">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-810">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-810">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-811">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-theme.css</span><span class="sxs-lookup"><span data-stu-id="fa965-811">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="fa965-812">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-theme.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-812">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="fa965-813">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.EOT</span><span class="sxs-lookup"><span data-stu-id="fa965-813">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="fa965-814">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="fa965-814">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="fa965-815">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="fa965-815">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="fa965-816">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="fa965-816">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="fa965-817">Önyükleme sürüm 2.3.2</span><span class="sxs-lookup"><span data-stu-id="fa965-817">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="fa965-818">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="fa965-819">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-819">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-820">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-821">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-822">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.css</span><span class="sxs-lookup"><span data-stu-id="fa965-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="fa965-823">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-responsive.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="fa965-824">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="fa965-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="fa965-825">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="fa965-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="fa965-826">Önyükleme sürüm 2.3.1</span><span class="sxs-lookup"><span data-stu-id="fa965-826">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="fa965-827">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="fa965-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="fa965-828">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-828">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="fa965-829">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.css</span><span class="sxs-lookup"><span data-stu-id="fa965-829">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="fa965-830">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-830">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="fa965-831">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.css</span><span class="sxs-lookup"><span data-stu-id="fa965-831">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="fa965-832">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-responsive.Min.css</span><span class="sxs-lookup"><span data-stu-id="fa965-832">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="fa965-833">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="fa965-833">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="fa965-834">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="fa965-834">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="fa965-835">CDN üzerinde önyükleme TouchCarousel sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-835">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="fa965-836">Aşağıdaki sürümleri [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") önyükleme TouchCarousel sürümleri CDN üzerinde barındırılan :</span><span class="sxs-lookup"><span data-stu-id="fa965-836">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="fa965-837">Önyükleme TouchCarousel sürüm 0.8.0</span><span class="sxs-lookup"><span data-stu-id="fa965-837">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="fa965-838">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/CSS/Bootstrap-Touch-carousel.css</span><span class="sxs-lookup"><span data-stu-id="fa965-838">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="fa965-839">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-carousel/0.8.0/js/Bootstrap-Touch-carousel.js</span><span class="sxs-lookup"><span data-stu-id="fa965-839">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="fa965-840">CDN üzerinde Hammer.js sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-840">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="fa965-841">Aşağıdaki sürümleri [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js sürümleri CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="fa965-841">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="fa965-842">Hammer.js sürüm 2.0.4</span><span class="sxs-lookup"><span data-stu-id="fa965-842">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="fa965-843">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.js</span><span class="sxs-lookup"><span data-stu-id="fa965-843">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="fa965-844">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-844">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="fa965-845">http://AJAX.aspnetcdn.com/AJAX/hammer.js/2.0.4/hammer.Min.map</span><span class="sxs-lookup"><span data-stu-id="fa965-845">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="fa965-846">ASP.NET Web formları ve CDN üzerinde Ajax sürümleri</span><span class="sxs-lookup"><span data-stu-id="fa965-846">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="fa965-847">ASP.NET Ajax kitaplığı aşağıdaki sürümleri üzerinde CDN barındırılır.</span><span class="sxs-lookup"><span data-stu-id="fa965-847">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="fa965-848">Dosyaların gerçek listesini görmek için her bağlantıyı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="fa965-848">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="fa965-849">ASP.NET Web Forms ve Ajax sürüm 4.5.2</span><span class="sxs-lookup"><span data-stu-id="fa965-849">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "ASP.NET Web Forms ve Ajax 4.5.2")
- [<span data-ttu-id="fa965-850">ASP.NET Web Forms ve Ajax sürüm 4</span><span class="sxs-lookup"><span data-stu-id="fa965-850">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Forms ve Ajax 4")
- [<span data-ttu-id="fa965-851">ASP.NET Ajax sürüm 3.5</span><span class="sxs-lookup"><span data-stu-id="fa965-851">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="fa965-852">ASP.NET MVC üzerinde CDN serbest bırakır</span><span class="sxs-lookup"><span data-stu-id="fa965-852">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="fa965-853">Aşağıdaki ASP.NET MVC JavaScript dosyaları bu CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="fa965-853">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="fa965-854">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="fa965-854">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="fa965-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/JQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="fa965-855">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="fa965-856">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/JQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-856">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="fa965-857">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="fa965-857">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="fa965-858">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/JQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="fa965-858">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="fa965-859">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/JQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-859">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="fa965-860">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="fa965-860">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="fa965-861">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/JQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="fa965-861">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="fa965-862">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/JQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-862">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="fa965-863">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="fa965-863">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="fa965-864">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/JQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="fa965-864">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="fa965-865">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/JQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-865">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="fa965-866">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="fa965-866">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="fa965-867">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/JQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="fa965-867">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="fa965-868">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/JQuery.unobtrusive-AJAX.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-868">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="fa965-869">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/JQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="fa965-869">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="fa965-870">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/JQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-870">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="fa965-871">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="fa965-871">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="fa965-872">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="fa965-872">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="fa965-873">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="fa965-873">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="fa965-874">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="fa965-874">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="fa965-875">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="fa965-875">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="fa965-876">ASP.NET MVC 1,0</span><span class="sxs-lookup"><span data-stu-id="fa965-876">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="fa965-877">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="fa965-877">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="fa965-878">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="fa965-878">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="fa965-879">ASP.NET SignalR üzerinde CDN serbest bırakır</span><span class="sxs-lookup"><span data-stu-id="fa965-879">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="fa965-880">Aşağıdaki ASP.NET SignalR JavaScript dosyaları bu CDN üzerinde barındırılan:</span><span class="sxs-lookup"><span data-stu-id="fa965-880">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="fa965-881">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="fa965-881">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="fa965-882">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.2.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="fa965-883">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-883">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="fa965-884">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="fa965-884">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="fa965-885">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="fa965-886">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-886">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="fa965-887">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="fa965-887">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="fa965-888">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="fa965-889">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-889">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="fa965-890">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="fa965-890">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="fa965-891">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="fa965-892">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-892">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="fa965-893">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="fa965-893">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="fa965-894">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="fa965-895">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="fa965-895">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="fa965-896">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="fa965-896">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="fa965-897">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="fa965-898">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-898">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="fa965-899">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="fa965-899">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="fa965-900">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="fa965-901">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-901">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="fa965-902">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="fa965-902">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="fa965-903">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="fa965-904">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-904">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="fa965-905">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="fa965-905">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="fa965-906">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="fa965-907">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="fa965-907">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="fa965-908">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="fa965-908">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="fa965-909">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="fa965-910">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="fa965-910">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="fa965-911">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="fa965-911">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="fa965-912">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="fa965-913">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-913">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="fa965-914">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="fa965-914">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="fa965-915">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-915">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="fa965-916">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="fa965-916">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="fa965-917">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="fa965-917">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="fa965-918">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.0.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="fa965-918">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="fa965-919">http://AJAX.aspnetcdn.com/AJAX/signalr/JQuery.signalr-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="fa965-919">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="fa965-920">CDN kullanım koşulları hakkında daha fazla bilgi için bkz: [Microsoft Ajax CDN Kullanım Koşulları'nı](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Kullanım Koşulları'nı").</span><span class="sxs-lookup"><span data-stu-id="fa965-920">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
