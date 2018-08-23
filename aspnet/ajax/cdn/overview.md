---
uid: ajax/cdn/overview
title: Microsoft Ajax içerik teslim ağı | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 10/14/2017
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 686cdfa5562f1f3569c30841420b112c522016dc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756130"
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="0be5f-102">Microsoft Ajax içerik teslim ağı</span><span class="sxs-lookup"><span data-stu-id="0be5f-102">Microsoft Ajax Content Delivery Network</span></span>
====================
> [!WARNING]
> <span data-ttu-id="0be5f-103">Üretim uygulamaları, sabit bir bağımlılık CDN varlıklar üzerinde almamalıdır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-103">Production applications should not take a hard dependency on CDN assets.</span></span> <span data-ttu-id="0be5f-104">Uygulamaları test başvurulan CDN varlık için ve CDN mevcut olmadığında, bir geri dönüş varlığını kullanan gerekir.</span><span class="sxs-lookup"><span data-stu-id="0be5f-104">Applications should test for the CDN asset referenced, and use a fallback asset when the CDN is not available.</span></span> 
>
> <span data-ttu-id="0be5f-105">Microsoft Ajax CDN, Azure CDN kullanarak sunmayan SLA yoktur.</span><span class="sxs-lookup"><span data-stu-id="0be5f-105">The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>
>
> <span data-ttu-id="0be5f-106">Kullanım [bu GitHub sorunu](https://github.com/aspnet/Docs/issues/5832) Microsoft Ajax CDN ile yaşayabileceğiniz sorunları.</span><span class="sxs-lookup"><span data-stu-id="0be5f-106">Use [this GitHub issue](https://github.com/aspnet/Docs/issues/5832) to report problems with the Microsoft Ajax CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="0be5f-107">İçindekiler tablosu</span><span class="sxs-lookup"><span data-stu-id="0be5f-107">Table of Contents</span></span>

<span data-ttu-id="0be5f-108">**[ajax.microsoft.com AJAX.aspnetcdn.com için yeniden adlandırıldı](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="0be5f-108">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="0be5f-109">**[Visual Studio .vsdoc desteği](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="0be5f-109">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="0be5f-110">**[ASP.NET Ajax CDN kullanma](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="0be5f-110">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="0be5f-111">**[JQuery CDN kullanma](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="0be5f-111">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="0be5f-112">**[JQuery kullanıcı Arabirimi CDN kullanma](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="0be5f-112">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="0be5f-113">**[Üçüncü taraf CDN dosyaları](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="0be5f-113">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="0be5f-114">CDN üzerinde jQuery yayınlar</span><span class="sxs-lookup"><span data-stu-id="0be5f-114">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="0be5f-115">CDN üzerinde jQuery geçirme yayınlar</span><span class="sxs-lookup"><span data-stu-id="0be5f-115">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="0be5f-116">jQuery kullanıcı Arabirimi yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-116">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="0be5f-117">jQuery doğrulama yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-117">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="0be5f-118">jQuery Mobile yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-118">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="0be5f-119">jQuery şablonları yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-119">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="0be5f-120">jQuery döngüsü yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-120">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="0be5f-121">DataTable yayınlar CDN üzerinde jQuery</span><span class="sxs-lookup"><span data-stu-id="0be5f-121">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="0be5f-122">CDN üzerinde Modernizr yayınlar</span><span class="sxs-lookup"><span data-stu-id="0be5f-122">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="0be5f-123">CDN üzerindeki JSHint yayınlar</span><span class="sxs-lookup"><span data-stu-id="0be5f-123">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="0be5f-124">Knockout yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-124">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="0be5f-125">CDN yayınlara globalize</span><span class="sxs-lookup"><span data-stu-id="0be5f-125">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="0be5f-126">CDN yayınlara yanıt</span><span class="sxs-lookup"><span data-stu-id="0be5f-126">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="0be5f-127">Önyükleme yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-127">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="0be5f-128">Önyükleme TouchCarousel yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-128">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="0be5f-129">CDN üzerinde Hammer.js yayınlar</span><span class="sxs-lookup"><span data-stu-id="0be5f-129">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="0be5f-130">ASP.NET Web Forms ve Ajax yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-130">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="0be5f-131">ASP.NET MVC CDN'de serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-131">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="0be5f-132">ASP.NET SignalR CDN'de serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-132">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="0be5f-133">Microsoft Ajax Content Delivery Network (CDN), jQuery gibi popüler üçüncü taraf JavaScript kitaplıklarını barındırır ve bunları, Web uygulamalarınıza kolayca eklemenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="0be5f-133">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="0be5f-134">Örneğin, yalnızca ekleyerek bu CDN'de barındırılan jQuery kullanmaya başlayabilirsiniz bir &lt;betik&gt; ajax.aspnetcdn.com için işaret eden sayfanıza etiketi.</span><span class="sxs-lookup"><span data-stu-id="0be5f-134">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="0be5f-135">CDN avantajlarından yararlanarak, Ajax uygulamalarınızın performansını önemli ölçüde artırabilir.</span><span class="sxs-lookup"><span data-stu-id="0be5f-135">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="0be5f-136">CDN içeriğini, tüm dünyada bulunan sunucularda önbelleğe alınır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-136">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="0be5f-137">Ayrıca, CDN önbelleğe alınan üçüncü taraf web siteleri, farklı etki alanlarında bulunan JavaScript dosyaları yeniden tarayıcılar sağlar.</span><span class="sxs-lookup"><span data-stu-id="0be5f-137">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="0be5f-138">CDN SSL (HTTPS), Güvenli Yuva Katmanı'nı kullanarak bir web sayfası hizmet gerektiği durumlarda destekler.</span><span class="sxs-lookup"><span data-stu-id="0be5f-138">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="0be5f-139">CDN, karşıya yüklediğiniz ve sizin için bu kitaplıkları sahipleri tarafından lisanslanır aşağıdaki üçüncü taraf betik kitaplıkları barındırır:</span><span class="sxs-lookup"><span data-stu-id="0be5f-139">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="0be5f-140">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="0be5f-140">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="0be5f-141">jQuery kullanıcı Arabirimi (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="0be5f-141">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="0be5f-142">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="0be5f-142">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="0be5f-143">jQuery doğrulaması (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="0be5f-143">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="0be5f-144">jQuery döngüsü (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="0be5f-144">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="0be5f-145">jQuery DataTable (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="0be5f-145">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="0be5f-146">Microsoft Ajax CDN ayrıca Microsoft tarafından karşıya yüklenen aşağıdaki kitaplıklarını içerir:</span><span class="sxs-lookup"><span data-stu-id="0be5f-146">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="0be5f-147">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="0be5f-147">ASP.NET Ajax</span></span>
- <span data-ttu-id="0be5f-148">ASP.NET MVC JavaScript dosyaları</span><span class="sxs-lookup"><span data-stu-id="0be5f-148">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="0be5f-149">ASP.NET SignalR JavaScript dosyaları</span><span class="sxs-lookup"><span data-stu-id="0be5f-149">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="0be5f-150">Microsoft, bu CDN'de barındırılan herhangi bir üçüncü taraf kitaplıkların mülkiyeti üzerine hak iddia değil.</span><span class="sxs-lookup"><span data-stu-id="0be5f-150">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="0be5f-151">Telif hakkı sahipleri kitaplıkları, bu kitaplıklara lisans.</span><span class="sxs-lookup"><span data-stu-id="0be5f-151">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="0be5f-152">Tür kitaplıkları indirip olabilecek herhangi bir hakkı, yalnızca ilgili telif hakkı sahipleri tarafından verilir.</span><span class="sxs-lookup"><span data-stu-id="0be5f-152">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="0be5f-153">Bunlar Microsoft kitaplıkları olmadığından, Microsoft bu CDN'de barındırılan üçüncü taraf kitaplıklar için hiçbir garanti veya fikri mülkiyet hakları lisansları (zımni hiçbir patent hakkı dahil) sağlar.</span><span class="sxs-lookup"><span data-stu-id="0be5f-153">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="0be5f-154">JavaScript Kitaplığı'nıza göndermek istediğiniz ve kitaplığınıza ilk JavaScript kitaplıklarını biridir (listelenmiş olarak http://trends.builtwith.com) veya uzantıları/eklentileri (a) popüler; veya (b) ASP.NET kullanmak faydalı sonra lütfen başvurun bu kitaplıklara AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="0be5f-154">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="0be5f-155">ajax.microsoft.com AJAX.aspnetcdn.com için yeniden adlandırıldı</span><span class="sxs-lookup"><span data-stu-id="0be5f-155">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="0be5f-156">CDN microsoft.com etki alanı adını kullanmak için kullanılan ve aspnetcdn.com etki alanı adını kullanacak şekilde değiştirildi.</span><span class="sxs-lookup"><span data-stu-id="0be5f-156">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="0be5f-157">Bir tarayıcı microsoft.com etki alanı başvurulduğunda, tanımlama bilgilerini bu etki alanından her istekle kablo üzerinden gönderir gibi olduğundan, performansı artırmak için bu değişiklik yapılmıştır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-157">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="0be5f-158">Microsoft.com dışındaki bir etki alanı adı için yeniden adlandırarak performans tarafından % 25 daha fazla artırılabilir.</span><span class="sxs-lookup"><span data-stu-id="0be5f-158">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="0be5f-159">Not ajax.microsoft.com çalışmaya devam eder ancak ajax.aspnetcdn.com tavsiye edilir.</span><span class="sxs-lookup"><span data-stu-id="0be5f-159">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="0be5f-160">Eski biçimi: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="0be5f-160">Old Format: https://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="0be5f-161">Yeni biçim: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="0be5f-161">New Format: https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="0be5f-162">Visual Studio .vsdoc desteği</span><span class="sxs-lookup"><span data-stu-id="0be5f-162">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="0be5f-163">.Vsdoc dosyaları VS 2008 SP1'e sahip olduğunuzdan emin olmak için gereken Visual Studio 2008 ile düzgün bir şekilde kullanmak için yüklü ve vsdoc dosyaları düzeltmesinin yüklü.</span><span class="sxs-lookup"><span data-stu-id="0be5f-163">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="0be5f-164">Bu uygulamaya buradan edinebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0be5f-164">You can get these from here:</span></span>

- [<span data-ttu-id="0be5f-165">Visual Studio 2008 SP1'i indirmek</span><span class="sxs-lookup"><span data-stu-id="0be5f-165">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Visual Studio 2008 SP1'i indirin")
- [<span data-ttu-id="0be5f-166">Visual Studio 2008 SP1 için .vsdoc düzeltmeyi indirmek</span><span class="sxs-lookup"><span data-stu-id="0be5f-166">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 ".vsdoc düzeltme için Visual Studio 2008 SP1 indirin")

<span data-ttu-id="0be5f-167">Visual Studio 2010, ek düzeltme eklerinin olmadan .vsdoc dosyalarını destekler.</span><span class="sxs-lookup"><span data-stu-id="0be5f-167">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="0be5f-168">ASP.NET Ajax CDN kullanma</span><span class="sxs-lookup"><span data-stu-id="0be5f-168">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="0be5f-169">ASP.NET 4 kullanırken, ASP.NET framework komut dosyaları için tüm istekler için CDN yönlendirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="0be5f-169">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="0be5f-170">Betikleri yerel web sunucunuzun yerine CDN alınırken genel ASP.NET Web sitesi performansını önemli ölçüde artırabilir.</span><span class="sxs-lookup"><span data-stu-id="0be5f-170">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="0be5f-171">Microsoft Ajax CDN için tüm ASP.NET framework betik istekleri yeniden yönlendirmek için ScriptManager EnableCDN özelliğini kullanın:</span><span class="sxs-lookup"><span data-stu-id="0be5f-171">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="0be5f-172">JQuery CDN kullanma</span><span class="sxs-lookup"><span data-stu-id="0be5f-172">Using jQuery from the CDN</span></span>

<span data-ttu-id="0be5f-173">Bir sayfaya aşağıdaki betiği öğesi ekleyerek Web uygulamanızda CDN'de barındırılan jQuery betikleri kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="0be5f-173">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="0be5f-174">CDN edinebilirsiniz jQuery betik küçültülmüş sürümünü de içerir. aşağıdaki öğesini kullanarak:</span><span class="sxs-lookup"><span data-stu-id="0be5f-174">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="0be5f-175">CDN kullanılamaz durumda, jQuery, kendi Web sitesinde bir yerel yolundan yüklenirken için geri dönüş sayfanıza izin vermek için öğenin CDN başvuran hemen sonra aşağıdaki öğeyi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="0be5f-175">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="0be5f-176">Aşağıdaki örnek sayfası, bir düğmeye tıklandığında bir div öğesine içeriğini görüntülemek için jQuery kitaplığı (ile yerel bir kopya için geri dönüş) CDN sürümünü kullanır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-176">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="0be5f-177">JQuery hakkında daha fazla bilgi edinin ve jQuery yerel bir kopyasını indirin ederek [jQuery](http://jquery.com/) Web sitesi.</span><span class="sxs-lookup"><span data-stu-id="0be5f-177">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="0be5f-178">JQuery kullanıcı Arabirimi CDN kullanma</span><span class="sxs-lookup"><span data-stu-id="0be5f-178">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="0be5f-179">CDN ayrıca jQuery kullanıcı Arabirimi kitaplığı barındırır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-179">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="0be5f-180">JQuery kullanıcı Arabirimi kitaplığı zengin bir pencere öğeleri ve ASP.NET uygulamalarınızda kullanabileceğiniz etkileri içerir.</span><span class="sxs-lookup"><span data-stu-id="0be5f-180">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="0be5f-181">Örneğin, aşağıdaki sayfa açılır takvimi görüntülemek için bir ASP.NET Web Forms uygulaması bağlamında jQuery UI Datepicker nasıl kullanabileceğinizi gösteren:</span><span class="sxs-lookup"><span data-stu-id="0be5f-181">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="0be5f-182">Klavyenizi kullanarak metin kutusu için odağı taşıyın, bir takvim görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="0be5f-182">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![DatePicker oluşturulan açılan takvimi](overview/_static/image1.png)

<span data-ttu-id="0be5f-184">Yukarıdaki kodda CDN üç dosyayı içermelidir dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="0be5f-184">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="0be5f-185">JQuery Kitaplığı &mdash; jQuery kullanıcı Arabirimi kitaplığı jQuery kitaplık bağlıdır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-185">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="0be5f-186">JQuery kullanıcı Arabirimi kitaplığı eklemeden önce sayfanıza jQuery kitaplığa eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="0be5f-186">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="0be5f-187">JQuery kullanıcı Arabirimi Kitaplığı &mdash; tüm pencere öğeleri gibi yukarıdaki kullanılan Datepicker pencere öğesi ve jQuery UI etkileri jQuery kullanıcı Arabirimi kitaplığı içerir.</span><span class="sxs-lookup"><span data-stu-id="0be5f-187">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="0be5f-188">JQuery kullanıcı Arabirimi teması &mdash; farklı Temalar jQuery kullanıcı Arabirimi destekler.</span><span class="sxs-lookup"><span data-stu-id="0be5f-188">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="0be5f-189">Yukarıdaki sayfayı Redmond temayı içeri aktarmak için bir CSS dosyası için bir bağlantı içerir.</span><span class="sxs-lookup"><span data-stu-id="0be5f-189">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="0be5f-190">Tüm standart jQuery kullanıcı Arabirimi teması, CDN'de barındırılan.</span><span class="sxs-lookup"><span data-stu-id="0be5f-190">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="0be5f-191">[Bu sayfayı ziyaret](jquery-ui/cdnjqueryui1910.md "kullanıcı Arabirimi 1.8.10 Microsoft Ajax CDN üzerinde jQuery") her teması için küçük resimleri görüntülemek için.</span><span class="sxs-lookup"><span data-stu-id="0be5f-191">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="0be5f-192">JQuery kullanıcı Arabirimi Kitaplığı hakkında daha fazla bilgi edinmek için resmi ziyaret [jQuery UI Web sitesi](http://jQueryUI.com "jQuery UI Web sitesi").</span><span class="sxs-lookup"><span data-stu-id="0be5f-192">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="0be5f-193">Üçüncü taraf CDN dosyaları</span><span class="sxs-lookup"><span data-stu-id="0be5f-193">Third-Party Files on the CDN</span></span>

<span data-ttu-id="0be5f-194">CDN, en popüler üçüncü taraf JavaScript kitaplıkları bazıları barındırır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-194">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="0be5f-195">Microsoft, bu CDN'de barındırılan herhangi bir üçüncü taraf kitaplıkların mülkiyeti üzerine hak iddia değil.</span><span class="sxs-lookup"><span data-stu-id="0be5f-195">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="0be5f-196">Telif hakkı sahipleri kitaplıkları, bu kitaplıklara lisans.</span><span class="sxs-lookup"><span data-stu-id="0be5f-196">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="0be5f-197">Tür kitaplıkları indirip olabilecek herhangi bir hakkı, yalnızca ilgili telif hakkı sahipleri tarafından verilir.</span><span class="sxs-lookup"><span data-stu-id="0be5f-197">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="0be5f-198">Bunlar Microsoft kitaplıkları olmadığından, Microsoft bu CDN'de barındırılan üçüncü taraf kitaplıklar için hiçbir garanti veya fikri mülkiyet hakları lisansları (zımni hiçbir patent hakkı dahil) sağlar.</span><span class="sxs-lookup"><span data-stu-id="0be5f-198">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="0be5f-199">CDN üzerinde jQuery yayınlar</span><span class="sxs-lookup"><span data-stu-id="0be5f-199">jQuery Releases on the CDN</span></span>

<span data-ttu-id="0be5f-200">JQuery aşağıdaki sürümleri, CDN'de barındırılan:</span><span class="sxs-lookup"><span data-stu-id="0be5f-200">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="0be5f-201">jQuery sürüm 3.3.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-201">jQuery version 3.3.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="0be5f-202">3.2.1 jQuery sürümü</span><span class="sxs-lookup"><span data-stu-id="0be5f-202">jQuery version 3.2.1</span></span>
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="0be5f-203">jQuery sürüm 3.2.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-203">jQuery version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="0be5f-204">3.1.1 jQuery sürümü</span><span class="sxs-lookup"><span data-stu-id="0be5f-204">jQuery version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="0be5f-205">jQuery sürüm 3.1.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-205">jQuery version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="0be5f-206">jQuery sürüm 3.0.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-206">jQuery version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="0be5f-207">jQuery sürüm 2.2.4</span><span class="sxs-lookup"><span data-stu-id="0be5f-207">jQuery version 2.2.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="0be5f-208">jQuery sürüm 2.2.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-208">jQuery version 2.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="0be5f-209">jQuery sürüm 2.2.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-209">jQuery version 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="0be5f-210">jQuery sürüm 2.2.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-210">jQuery version 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="0be5f-211">jQuery sürüm 2.2.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-211">jQuery version 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="0be5f-212">jQuery sürüm larını 2.1.4</span><span class="sxs-lookup"><span data-stu-id="0be5f-212">jQuery version 2.1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="0be5f-213">jQuery sürüm 2.1.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-213">jQuery version 2.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="0be5f-214">jQuery sürüm 2.1.2'ye</span><span class="sxs-lookup"><span data-stu-id="0be5f-214">jQuery version 2.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="0be5f-215">jQuery sürüm 2.1.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-215">jQuery version 2.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="0be5f-216">jQuery sürüm 2.1.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-216">jQuery version 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="0be5f-217">jQuery sürüm 2.0.3 sürümünü</span><span class="sxs-lookup"><span data-stu-id="0be5f-217">jQuery version 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="0be5f-218">jQuery sürüm 2.0.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-218">jQuery version 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="0be5f-219">jQuery sürüm 2.0.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-219">jQuery version 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="0be5f-220">jQuery sürüm 2.0.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-220">jQuery version 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="0be5f-221">jQuery sürüm 1.12.4</span><span class="sxs-lookup"><span data-stu-id="0be5f-221">jQuery version 1.12.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="0be5f-222">jQuery sürüm 1.12.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-222">jQuery version 1.12.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="0be5f-223">jQuery sürüm 1.12.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-223">jQuery version 1.12.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="0be5f-224">jQuery sürüm 1.12.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-224">jQuery version 1.12.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="0be5f-225">jQuery sürüm 1.12.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-225">jQuery version 1.12.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="0be5f-226">jQuery sürüm 1.11.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-226">jQuery version 1.11.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="0be5f-227">jQuery sürüm 1.11.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-227">jQuery version 1.11.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="0be5f-228">jQuery sürüm 1.11.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-228">jQuery version 1.11.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="0be5f-229">jQuery sürüm 1.11.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-229">jQuery version 1.11.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="0be5f-230">jQuery sürüm 1.10.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-230">jQuery version 1.10.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="0be5f-231">jQuery sürüm 1.10.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-231">jQuery version 1.10.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="0be5f-232">jQuery sürüm 1.10.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-232">jQuery version 1.10.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="0be5f-233">jQuery sürüm 1.9.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-233">jQuery version 1.9.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="0be5f-234">jQuery sürüm 1.9.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-234">jQuery version 1.9.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="0be5f-235">jQuery sürüm 1.8.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-235">jQuery version 1.8.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="0be5f-236">jQuery sürüm 1.8.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-236">jQuery version 1.8.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="0be5f-237">jQuery sürümü 1.8.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-237">jQuery version 1.8.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="0be5f-238">jQuery sürüm 1.8.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-238">jQuery version 1.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="0be5f-239">jQuery sürüm 1.7.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-239">jQuery version 1.7.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="0be5f-240">jQuery sürüm 1.7.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-240">jQuery version 1.7.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="0be5f-241">jQuery sürüm 1.7</span><span class="sxs-lookup"><span data-stu-id="0be5f-241">jQuery version 1.7</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="0be5f-242">jQuery sürüm 1.6.4</span><span class="sxs-lookup"><span data-stu-id="0be5f-242">jQuery version 1.6.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="0be5f-243">jQuery sürüm 1.6.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-243">jQuery version 1.6.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="0be5f-244">jQuery sürüm 1.6.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-244">jQuery version 1.6.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="0be5f-245">jQuery sürümü 1.6.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-245">jQuery version 1.6.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="0be5f-246">jQuery sürüm 1.6</span><span class="sxs-lookup"><span data-stu-id="0be5f-246">jQuery version 1.6</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="0be5f-247">1.5.2 jQuery sürümü</span><span class="sxs-lookup"><span data-stu-id="0be5f-247">jQuery version 1.5.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="0be5f-248">1.5.1 jQuery sürümü</span><span class="sxs-lookup"><span data-stu-id="0be5f-248">jQuery version 1.5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="0be5f-249">jQuery sürüm 1.5</span><span class="sxs-lookup"><span data-stu-id="0be5f-249">jQuery version 1.5</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="0be5f-250">jQuery sürüm 1.4.4</span><span class="sxs-lookup"><span data-stu-id="0be5f-250">jQuery version 1.4.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="0be5f-251">jQuery sürüm 1.4.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-251">jQuery version 1.4.3</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="0be5f-252">jQuery sürüm 1.4.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-252">jQuery version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="0be5f-253">jQuery sürüm 1.4.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-253">jQuery version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="0be5f-254">jQuery sürüm 1.4</span><span class="sxs-lookup"><span data-stu-id="0be5f-254">jQuery version 1.4</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="0be5f-255">jQuery sürüm 1.3.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-255">jQuery version 1.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="0be5f-256">CDN üzerinde jQuery geçirme yayınlar</span><span class="sxs-lookup"><span data-stu-id="0be5f-256">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="0be5f-257">JQuery geçirme aşağıdaki sürümleri, CDN'de barındırılan:</span><span class="sxs-lookup"><span data-stu-id="0be5f-257">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="0be5f-258">jQuery sürüm 3.0.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="0be5f-258">jQuery Migrate version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="0be5f-259">jQuery geçirme 1.2.1 sürümü</span><span class="sxs-lookup"><span data-stu-id="0be5f-259">jQuery Migrate version 1.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="0be5f-260">jQuery sürümü 1.2.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="0be5f-260">jQuery Migrate version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="0be5f-261">jQuery sürüm 1.1.1 geçirme</span><span class="sxs-lookup"><span data-stu-id="0be5f-261">jQuery Migrate version 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="0be5f-262">jQuery sürüm 1.1.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="0be5f-262">jQuery Migrate version 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="0be5f-263">jQuery sürüm 1.0.0 geçirme</span><span class="sxs-lookup"><span data-stu-id="0be5f-263">jQuery Migrate version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- https://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="0be5f-264">jQuery kullanıcı Arabirimi yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-264">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="0be5f-265">Bu CDN üzerinde jQuery kullanıcı Arabirimi kitaplığı aşağıdaki sürümleri barındırılır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-265">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="0be5f-266">Dosyaların gerçek listesini görmek için her bir bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0be5f-266">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0be5f-267">jQuery kullanıcı Arabirimi 1.12.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-267">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "kullanıcı Arabirimi 1.12.1 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-268">jQuery kullanıcı Arabirimi 1.12.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-268">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "kullanıcı Arabirimi 1.12.0 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-269">jQuery kullanıcı Arabirimi 1.11.4</span><span class="sxs-lookup"><span data-stu-id="0be5f-269">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "kullanıcı Arabirimi 1.11.4 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-270">jQuery kullanıcı Arabirimi 1.11.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-270">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "kullanıcı Arabirimi 1.11.3 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-271">jQuery kullanıcı Arabirimi 1.11.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-271">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "kullanıcı Arabirimi 1.11.2 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-272">jQuery kullanıcı Arabirimi 1.11.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-272">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "kullanıcı Arabirimi 1.11.1 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-273">jQuery kullanıcı Arabirimi 1.11.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-273">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "kullanıcı Arabirimi 1.11.0 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-274">jQuery kullanıcı Arabirimi 1.10.4</span><span class="sxs-lookup"><span data-stu-id="0be5f-274">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "kullanıcı Arabirimi 1.10.4 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-275">jQuery kullanıcı Arabirimi 1.10.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-275">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "kullanıcı Arabirimi 1.10.3 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-276">jQuery kullanıcı Arabirimi 1.10.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-276">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "kullanıcı Arabirimi 1.10.2 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-277">jQuery kullanıcı Arabirimi 1.10.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-277">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "kullanıcı Arabirimi 1.10.1 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-278">jQuery kullanıcı Arabirimi 1.10.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-278">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "kullanıcı Arabirimi 1.10.0 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-279">jQuery kullanıcı Arabirimi 1.9.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-279">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "kullanıcı Arabirimi 1.9.2 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-280">jQuery kullanıcı Arabirimi 1.9.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-280">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "kullanıcı Arabirimi 1.9.1 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-281">jQuery kullanıcı Arabirimi 1.9.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-281">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "kullanıcı Arabirimi 1.9.0 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-282">jQuery kullanıcı Arabirimi 1.8.24</span><span class="sxs-lookup"><span data-stu-id="0be5f-282">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "kullanıcı Arabirimi 1.8.24 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-283">jQuery kullanıcı Arabirimi 1.8.23</span><span class="sxs-lookup"><span data-stu-id="0be5f-283">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "kullanıcı Arabirimi 1.8.23 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-284">jQuery kullanıcı Arabirimi 1.8.22</span><span class="sxs-lookup"><span data-stu-id="0be5f-284">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "kullanıcı Arabirimi 1.8.22 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-285">jQuery kullanıcı Arabirimi 1.8.21</span><span class="sxs-lookup"><span data-stu-id="0be5f-285">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "kullanıcı Arabirimi 1.8.21 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-286">jQuery kullanıcı Arabirimi 1.8.20</span><span class="sxs-lookup"><span data-stu-id="0be5f-286">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "kullanıcı Arabirimi 1.8.20 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-287">jQuery kullanıcı Arabirimi 1.8.19</span><span class="sxs-lookup"><span data-stu-id="0be5f-287">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "kullanıcı Arabirimi 1.8.19 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-288">jQuery kullanıcı Arabirimi 1.8.18</span><span class="sxs-lookup"><span data-stu-id="0be5f-288">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "kullanıcı Arabirimi 1.8.18 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-289">jQuery kullanıcı Arabirimi 1.8.17</span><span class="sxs-lookup"><span data-stu-id="0be5f-289">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "kullanıcı Arabirimi 1.8.17 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-290">jQuery kullanıcı Arabirimi 1.8.16</span><span class="sxs-lookup"><span data-stu-id="0be5f-290">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "kullanıcı Arabirimi 1.8.16 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-291">jQuery kullanıcı Arabirimi 1.8.15</span><span class="sxs-lookup"><span data-stu-id="0be5f-291">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "kullanıcı Arabirimi 1.8.15 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-292">jQuery kullanıcı Arabirimi 1.8.14</span><span class="sxs-lookup"><span data-stu-id="0be5f-292">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "kullanıcı Arabirimi 1.8.14 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-293">jQuery kullanıcı Arabirimi 1.8.13</span><span class="sxs-lookup"><span data-stu-id="0be5f-293">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "kullanıcı Arabirimi 1.8.13 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-294">jQuery kullanıcı Arabirimi 1.8.12</span><span class="sxs-lookup"><span data-stu-id="0be5f-294">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "kullanıcı Arabirimi 1.8.12 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-295">jQuery kullanıcı Arabirimi 1.8.11</span><span class="sxs-lookup"><span data-stu-id="0be5f-295">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "kullanıcı Arabirimi 1.8.11 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-296">jQuery kullanıcı Arabirimi 1.8.10</span><span class="sxs-lookup"><span data-stu-id="0be5f-296">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "kullanıcı Arabirimi 1.8.10 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-297">jQuery kullanıcı Arabirimi 1.8.9</span><span class="sxs-lookup"><span data-stu-id="0be5f-297">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "kullanıcı Arabirimi 1.8.9 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-298">jQuery kullanıcı Arabirimi 1.8.8</span><span class="sxs-lookup"><span data-stu-id="0be5f-298">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "kullanıcı Arabirimi 1.8.8 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-299">jQuery kullanıcı Arabirimi 1.8.7</span><span class="sxs-lookup"><span data-stu-id="0be5f-299">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "kullanıcı Arabirimi 1.8.7 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-300">jQuery kullanıcı Arabirimi 1.8.6</span><span class="sxs-lookup"><span data-stu-id="0be5f-300">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "kullanıcı Arabirimi 1.8.6 Microsoft Ajax CDN üzerinde jQuery")
- [<span data-ttu-id="0be5f-301">jQuery kullanıcı arabirimi 1.8.5</span><span class="sxs-lookup"><span data-stu-id="0be5f-301">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery kullanıcı arabirimi 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="0be5f-302">jQuery doğrulama yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-302">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="0be5f-303">Bu CDN üzerinde jQuery doğrulama Kitaplığı'nın aşağıdaki sürümleri barındırılır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-303">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="0be5f-304">Dosyaların gerçek listesini görmek için her bir bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0be5f-304">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0be5f-305">jQuery doğrulama 1.17.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-305">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery doğrulaması 1.17.0")
- [<span data-ttu-id="0be5f-306">jQuery doğrulama 1.16.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-306">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery doğrulaması 1.16.0")
- [<span data-ttu-id="0be5f-307">jQuery doğrulama 1.15.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-307">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery doğrulaması 1.15.1")
- [<span data-ttu-id="0be5f-308">jQuery doğrulama 1.15.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-308">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery doğrulaması 1.15.0")
- [<span data-ttu-id="0be5f-309">jQuery doğrulama 1.14.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-309">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery doğrulaması 1.14.0")
- [<span data-ttu-id="0be5f-310">jQuery doğrulama 1.13.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-310">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery doğrulaması 1.13.1")
- [<span data-ttu-id="0be5f-311">jQuery doğrulama 1.13.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-311">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery doğrulaması 1.13.0")
- [<span data-ttu-id="0be5f-312">jQuery doğrulama 1.12.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-312">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery doğrulaması 1.12.0")
- [<span data-ttu-id="0be5f-313">jQuery doğrulama 1.11.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-313">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery doğrulaması 1.11.1")
- [<span data-ttu-id="0be5f-314">jQuery doğrulama 1.11.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-314">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery doğrulaması 1.11.0")
- [<span data-ttu-id="0be5f-315">jQuery doğrulama 1.10.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-315">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery doğrulaması 1.10.0")
- [<span data-ttu-id="0be5f-316">jQuery doğrulama 1.9</span><span class="sxs-lookup"><span data-stu-id="0be5f-316">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate sürümü 1.9")
- [<span data-ttu-id="0be5f-317">jQuery doğrulama 1.8.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-317">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate sürümü 1.8.1")
- [<span data-ttu-id="0be5f-318">jQuery doğrulama 1.8</span><span class="sxs-lookup"><span data-stu-id="0be5f-318">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate sürümü 1.8")
- [<span data-ttu-id="0be5f-319">jQuery doğrulama 1.7</span><span class="sxs-lookup"><span data-stu-id="0be5f-319">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate sürümü 1.7")
- [<span data-ttu-id="0be5f-320">jQuery doğrulama 1.6</span><span class="sxs-lookup"><span data-stu-id="0be5f-320">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery doğrulama 1.6")
- [<span data-ttu-id="0be5f-321">jQuery doğrulama 1.5.5</span><span class="sxs-lookup"><span data-stu-id="0be5f-321">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery doğrulama 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="0be5f-322">jQuery Mobile yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-322">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="0be5f-323">Bu CDN üzerinde jQuery Mobile Kitaplığı'nın aşağıdaki sürümleri barındırılır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-323">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="0be5f-324">Dosyaların gerçek listesini görmek için her bir bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0be5f-324">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0be5f-325">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="0be5f-325">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.4.5")
- [<span data-ttu-id="0be5f-326">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-326">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.4.2")
- [<span data-ttu-id="0be5f-327">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-327">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.4.1")
- [<span data-ttu-id="0be5f-328">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-328">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.4.0")
- [<span data-ttu-id="0be5f-329">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-329">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.3.2")
- [<span data-ttu-id="0be5f-330">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-330">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.3.1")
- [<span data-ttu-id="0be5f-331">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-331">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.3.0")
- [<span data-ttu-id="0be5f-332">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-332">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.2.0")
- [<span data-ttu-id="0be5f-333">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-333">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.1.2")
- [<span data-ttu-id="0be5f-334">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-334">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.1.1")
- [<span data-ttu-id="0be5f-335">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-335">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.1.0")
- [<span data-ttu-id="0be5f-336">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="0be5f-336">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.1.0 RC2")
- [<span data-ttu-id="0be5f-337">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-337">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.0.1")
- [<span data-ttu-id="0be5f-338">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-338">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.0")
- [<span data-ttu-id="0be5f-339">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="0be5f-339">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.0 RC2")
- [<span data-ttu-id="0be5f-340">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="0be5f-340">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.0 RC1")
- [<span data-ttu-id="0be5f-341">jQuery Mobile 1.0 beta 3</span><span class="sxs-lookup"><span data-stu-id="0be5f-341">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "Microsoft Ajax CDN üzerinde jQuery Mobile 1.0 Beta 3")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="0be5f-342">jQuery şablonları yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-342">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="0be5f-343">Bu CDN üzerinde jQuery şablonları eklentisi aşağıdaki sürümleri barındırılır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-343">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="0be5f-344">Dosyaların gerçek listesini görmek için her bir bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0be5f-344">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0be5f-345">jQuery şablonları Beta 1</span><span class="sxs-lookup"><span data-stu-id="0be5f-345">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery şablonları Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="0be5f-346">jQuery döngüsü yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-346">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="0be5f-347">Bu CDN üzerinde jQuery döngüsü eklentisi aşağıdaki sürümleri barındırılır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-347">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="0be5f-348">Dosyaların gerçek listesini görmek için her bir bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0be5f-348">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0be5f-349">jQuery döngüsü 2.99</span><span class="sxs-lookup"><span data-stu-id="0be5f-349">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery döngüsü 2.99")
- [<span data-ttu-id="0be5f-350">jQuery döngüsü 2.94</span><span class="sxs-lookup"><span data-stu-id="0be5f-350">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery döngüsü 2.94")
- [<span data-ttu-id="0be5f-351">jQuery döngüsü 2.88</span><span class="sxs-lookup"><span data-stu-id="0be5f-351">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery döngüsü 2.88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="0be5f-352">DataTable yayınlar CDN üzerinde jQuery</span><span class="sxs-lookup"><span data-stu-id="0be5f-352">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="0be5f-353">Bu CDN üzerinde jQuery DataTable eklentisi aşağıdaki sürümleri barındırılır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-353">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="0be5f-354">Dosyaların gerçek listesini görmek için her bir bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0be5f-354">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0be5f-355">jQuery DataTable 1.10.5</span><span class="sxs-lookup"><span data-stu-id="0be5f-355">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTable 1.10.5")
- [<span data-ttu-id="0be5f-356">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="0be5f-356">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="0be5f-357">jQuery DataTable 1.9.4</span><span class="sxs-lookup"><span data-stu-id="0be5f-357">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTable 1.9.4")
- [<span data-ttu-id="0be5f-358">jQuery DataTable 1.9.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-358">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTable 1.9.3")
- [<span data-ttu-id="0be5f-359">jQuery DataTable 1.9.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-359">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTable 1.9.2")
- [<span data-ttu-id="0be5f-360">jQuery DataTable 1.9.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-360">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTable 1.9.1")
- [<span data-ttu-id="0be5f-361">jQuery DataTable 1.9.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-361">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTable 1.9.0")
- [<span data-ttu-id="0be5f-362">jQuery DataTable 1.8.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-362">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTable 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="0be5f-363">CDN üzerinde Modernizr yayınlar</span><span class="sxs-lookup"><span data-stu-id="0be5f-363">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="0be5f-364">Aşağıdaki sürümleri [Modernizr](http://www.modernizr.com "Modernizr") CDN'de barındırılan:</span><span class="sxs-lookup"><span data-stu-id="0be5f-364">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- https://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="0be5f-365">CDN üzerindeki JSHint yayınlar</span><span class="sxs-lookup"><span data-stu-id="0be5f-365">JSHint Releases on the CDN</span></span>

<span data-ttu-id="0be5f-366">Aşağıdaki sürümleri [JSHint](http://www.jshint.com "JSHint") CDN'de barındırılan:</span><span class="sxs-lookup"><span data-stu-id="0be5f-366">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="0be5f-367">Knockout yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-367">Knockout Releases on the CDN</span></span>

<span data-ttu-id="0be5f-368">Aşağıdaki sürümleri [Knockout](http://www.knockoutjs.com "Knockout") CDN'de barındırılan:</span><span class="sxs-lookup"><span data-stu-id="0be5f-368">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- https://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="0be5f-369">CDN yayınlara globalize</span><span class="sxs-lookup"><span data-stu-id="0be5f-369">Globalize Releases on the CDN</span></span>

<span data-ttu-id="0be5f-370">Aşağıdaki sürümleri [Globalize](https://github.com/jquery/globalize "Globalize") CDN'de barındırılan:</span><span class="sxs-lookup"><span data-stu-id="0be5f-370">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="0be5f-371">Sürüm 1.0.0 globalize</span><span class="sxs-lookup"><span data-stu-id="0be5f-371">Globalize version 1.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- https://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="0be5f-372">Sürüm 0.1.1 globalize</span><span class="sxs-lookup"><span data-stu-id="0be5f-372">Globalize version 0.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="0be5f-373">tüm kültürler</span><span class="sxs-lookup"><span data-stu-id="0be5f-373">all cultures</span></span>
- https://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="0be5f-374">"{Kodu kültüre}" istenen kültürü kod ile değiştirin, örneğin globalize.culture.en GB.js== Microsoft CDN'de dosyaları bu == kitaplıkları Microsoft tarafından karşıya yüklendi.</span><span class="sxs-lookup"><span data-stu-id="0be5f-374">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="0be5f-375">CDN yayınlara yanıt</span><span class="sxs-lookup"><span data-stu-id="0be5f-375">Respond Releases on the CDN</span></span>

<span data-ttu-id="0be5f-376">Aşağıdaki sürümleri [ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") yanıt CDN'de barındırılan:</span><span class="sxs-lookup"><span data-stu-id="0be5f-376">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="0be5f-377">Sürüm 1.4.2 yanıt</span><span class="sxs-lookup"><span data-stu-id="0be5f-377">Respond version 1.4.2</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="0be5f-378">Sürüm 1.4.1 yanıt</span><span class="sxs-lookup"><span data-stu-id="0be5f-378">Respond version 1.4.1</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="0be5f-379">Sürüm 1.4.0 yanıt</span><span class="sxs-lookup"><span data-stu-id="0be5f-379">Respond version 1.4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- https://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="0be5f-380">Yanıt sürüm 1.3.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-380">Respond version 1.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="0be5f-381">Yanıt sürümü 1.2.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-381">Respond version 1.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="0be5f-382">Önyükleme yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-382">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="0be5f-383">Aşağıdaki sürümleri [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap, CDN'de barındırılır:</span><span class="sxs-lookup"><span data-stu-id="0be5f-383">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-411"></a><span data-ttu-id="0be5f-384">Önyükleme sürüm 4.1.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-384">Bootstrap version 4.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.1.1/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-400"></a><span data-ttu-id="0be5f-385">Önyükleme sürüm 4.0.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-385">Bootstrap version 4.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-grid.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap-reboot.css.map

#### <a name="bootstrap-version-337"></a><span data-ttu-id="0be5f-386">Önyükleme sürüm 3.3.7</span><span class="sxs-lookup"><span data-stu-id="0be5f-386">Bootstrap version 3.3.7</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="0be5f-387">Önyükleme sürüm 3.3.6</span><span class="sxs-lookup"><span data-stu-id="0be5f-387">Bootstrap version 3.3.6</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="0be5f-388">Önyükleme sürüm 3.3.5</span><span class="sxs-lookup"><span data-stu-id="0be5f-388">Bootstrap version 3.3.5</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="0be5f-389">Önyükleme sürüm 3.3.4</span><span class="sxs-lookup"><span data-stu-id="0be5f-389">Bootstrap version 3.3.4</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="0be5f-390">3.3.2 önyükleme sürümü</span><span class="sxs-lookup"><span data-stu-id="0be5f-390">Bootstrap version 3.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="0be5f-391">Önyükleme sürüm 3.3.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-391">Bootstrap version 3.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="0be5f-392">Önyükleme sürüm 3.3.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-392">Bootstrap version 3.3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="0be5f-393">Önyükleme sürüm 3.2.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-393">Bootstrap version 3.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="0be5f-394">3.1.1 önyükleme sürümü</span><span class="sxs-lookup"><span data-stu-id="0be5f-394">Bootstrap version 3.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="0be5f-395">Önyükleme sürüm 3.1.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-395">Bootstrap version 3.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="0be5f-396">Önyükleme sürüm 3.0.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-396">Bootstrap version 3.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="0be5f-397">Önyükleme sürüm 3.0.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-397">Bootstrap version 3.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="0be5f-398">Önyükleme 3.0.1 sürümü</span><span class="sxs-lookup"><span data-stu-id="0be5f-398">Bootstrap version 3.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="0be5f-399">Önyükleme sürüm 3.0.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-399">Bootstrap version 3.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- https://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="0be5f-400">Önyükleme sürüm 2.3.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-400">Bootstrap version 2.3.2</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="0be5f-401">Önyükleme sürüm 2.3.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-401">Bootstrap version 2.3.1</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- https://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="0be5f-402">Önyükleme TouchCarousel yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-402">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="0be5f-403">Aşağıdaki sürümleri [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") önyükleme TouchCarousel yayınlar CDN'de barındırılan:</span><span class="sxs-lookup"><span data-stu-id="0be5f-403">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="0be5f-404">Önyükleme TouchCarousel 0.8.0 sürümü</span><span class="sxs-lookup"><span data-stu-id="0be5f-404">Bootstrap TouchCarousel version 0.8.0</span></span>

- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- https://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="0be5f-405">CDN üzerinde Hammer.js yayınlar</span><span class="sxs-lookup"><span data-stu-id="0be5f-405">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="0be5f-406">Aşağıdaki sürümleri [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js yayınlar CDN'de barındırılan:</span><span class="sxs-lookup"><span data-stu-id="0be5f-406">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="0be5f-407">2.0.4 Hammer.js sürümü</span><span class="sxs-lookup"><span data-stu-id="0be5f-407">Hammer.js version 2.0.4</span></span>

- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- https://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="0be5f-408">ASP.NET Web Forms ve Ajax yayınlar CDN hakkında</span><span class="sxs-lookup"><span data-stu-id="0be5f-408">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="0be5f-409">ASP.NET Ajax Kitaplığı'nın aşağıdaki sürümleri, CDN'de barındırılan.</span><span class="sxs-lookup"><span data-stu-id="0be5f-409">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="0be5f-410">Dosyaların gerçek listesini görmek için her bir bağlantıya tıklayın.</span><span class="sxs-lookup"><span data-stu-id="0be5f-410">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="0be5f-411">ASP.NET Web Forms ve Ajax sürüm 4.5.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-411">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "ASP.NET Web Forms ve Ajax 4.5.2")
- [<span data-ttu-id="0be5f-412">ASP.NET Web Forms ve Ajax sürüm 4</span><span class="sxs-lookup"><span data-stu-id="0be5f-412">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Forms ve Ajax 4")
- [<span data-ttu-id="0be5f-413">ASP.NET Ajax sürüm 3.5</span><span class="sxs-lookup"><span data-stu-id="0be5f-413">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="0be5f-414">ASP.NET MVC CDN'de serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-414">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="0be5f-415">Aşağıdaki ASP.NET MVC JavaScript dosyaları üzerinde bu CDN barındırılır:</span><span class="sxs-lookup"><span data-stu-id="0be5f-415">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="0be5f-416">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-416">ASP.NET MVC 5.2.3</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="0be5f-417">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-417">ASP.NET MVC 5.1</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="0be5f-418">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-418">ASP.NET MVC 5.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="0be5f-419">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-419">ASP.NET MVC 4.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="0be5f-420">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-420">ASP.NET MVC 3.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.js
- https://ajax.aspnetcdn.com/ajax/jquery.unobtrusive-ajax/3.2.5/jquery.unobtrusive-ajax.min.js 
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.js 
- https://ajax.aspnetcdn.com/ajax/jquery.validation.unobtrusive/3.2.10/jquery.validate.unobtrusive.min.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="0be5f-421">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-421">ASP.NET MVC 2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="0be5f-422">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-422">ASP.NET MVC 1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- https://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="0be5f-423">ASP.NET SignalR CDN'de serbest bırakır.</span><span class="sxs-lookup"><span data-stu-id="0be5f-423">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="0be5f-424">Aşağıdaki ASP.NET SignalR JavaScript dosyaları üzerinde bu CDN barındırılır:</span><span class="sxs-lookup"><span data-stu-id="0be5f-424">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="0be5f-425">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-425">ASP.NET SignalR 2.2.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="0be5f-426">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-426">ASP.NET SignalR 2.2.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="0be5f-427">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-427">ASP.NET SignalR 2.2.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="0be5f-428">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-428">ASP.NET SignalR 2.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="0be5f-429">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-429">ASP.NET SignalR 2.0.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="0be5f-430">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-430">ASP.NET SignalR 2.0.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="0be5f-431">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-431">ASP.NET SignalR 2.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="0be5f-432">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-432">ASP.NET SignalR 2.0.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="0be5f-433">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="0be5f-433">ASP.NET SignalR 1.1.3</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="0be5f-434">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="0be5f-434">ASP.NET SignalR 1.1.2</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="0be5f-435">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-435">ASP.NET SignalR 1.1.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="0be5f-436">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="0be5f-436">ASP.NET SignalR 1.1.0</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="0be5f-437">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="0be5f-437">ASP.NET SignalR 1.0.1</span></span>

- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- https://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="0be5f-438">CDN kullanım koşulları hakkında daha fazla bilgi için bkz: [Microsoft Ajax CDN kullanım koşulları](https://www.asp.net/terms-of-use "Microsoft Ajax CDN kullanım koşulları").</span><span class="sxs-lookup"><span data-stu-id="0be5f-438">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
