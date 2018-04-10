---
uid: mvc/overview/releases/mvc51-release-notes
title: ASP.NET MVC 5.1 yenilikler | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: be10486c9fd39738f44cdda4fedb409058017601
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2018
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="ed63f-102">ASP.NET MVC 5.1 yenilikler nelerdir?</span><span class="sxs-lookup"><span data-stu-id="ed63f-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="ed63f-103">tarafından [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="ed63f-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="ed63f-104">Bu konuda, ASP.NET Web MVC 5.1 yenilikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="ed63f-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="ed63f-105">Yazılım gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="ed63f-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="ed63f-106">İndir</span><span class="sxs-lookup"><span data-stu-id="ed63f-106">Download</span></span>](#download)
- [<span data-ttu-id="ed63f-107">Belgeler</span><span class="sxs-lookup"><span data-stu-id="ed63f-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="ed63f-108">ASP.NET MVC 5.1 yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="ed63f-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="ed63f-109">Yönlendirme geliştirmeleri özniteliği</span><span class="sxs-lookup"><span data-stu-id="ed63f-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="ed63f-110">Düzenleyici şablonları önyükleme desteği</span><span class="sxs-lookup"><span data-stu-id="ed63f-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="ed63f-111">Görünümlerde enum desteği</span><span class="sxs-lookup"><span data-stu-id="ed63f-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="ed63f-112">MinLength/MaxLength öznitelikler için örtük doğrulama</span><span class="sxs-lookup"><span data-stu-id="ed63f-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="ed63f-113">'This' bağlam içinde örtük Ajax destekleme</span><span class="sxs-lookup"><span data-stu-id="ed63f-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="ed63f-114">[Bilinen sorunlar ve önemli değişiklikler](#KnownBreakingChanges)- [hata düzeltmeleri](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="ed63f-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="ed63f-115">Yazılım Gereksinimleri</span><span class="sxs-lookup"><span data-stu-id="ed63f-115">Software Requirements</span></span>

- <span data-ttu-id="ed63f-116">Visual Studio 2012: İndirme [ASP.NET ve Web Araçları Visual Studio 2012 için 2013.1](https://go.microsoft.com/fwlink/?LinkId=390062).</span><span class="sxs-lookup"><span data-stu-id="ed63f-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="ed63f-117">Visual Studio 2013: İndirebilirsiniz [Visual Studio 2013 güncelleştirme 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span><span class="sxs-lookup"><span data-stu-id="ed63f-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="ed63f-118">Bu güncelleştirme, ASP.NET MVC 5.1 Razor görünümleri düzenleme için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="ed63f-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="ed63f-119">İndir</span><span class="sxs-lookup"><span data-stu-id="ed63f-119">Download</span></span>

<span data-ttu-id="ed63f-120">Çalışma zamanı özellikleri NuGet galerisinde NuGet paketlerini olarak yayımlanmıştır.</span><span class="sxs-lookup"><span data-stu-id="ed63f-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="ed63f-121">Tüm çalışma zamanı paketleri izleyin [anlamsal sürüm oluşturma](http://semver.org/) belirtimi.</span><span class="sxs-lookup"><span data-stu-id="ed63f-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="ed63f-122">En son ASP.NET MVC 5.1 RTM paketini aşağıdaki sürümü vardır: "5.1.2".</span><span class="sxs-lookup"><span data-stu-id="ed63f-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="ed63f-123">Yükleme veya güncelleştirme bu paketleri yoluyla [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span><span class="sxs-lookup"><span data-stu-id="ed63f-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="ed63f-124">Sürüm, karşılık gelen yerelleştirilmiş NuGet paketlerini de içerir.</span><span class="sxs-lookup"><span data-stu-id="ed63f-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="ed63f-125">Yükleme veya NuGet Paket Yöneticisi konsolu kullanılarak yayımlanan NuGet paketlerini güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="ed63f-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="ed63f-126">Belgeler</span><span class="sxs-lookup"><span data-stu-id="ed63f-126">Documentation</span></span>

<span data-ttu-id="ed63f-127">Öğreticiler ve ASP.NET MVC 5.1 RTM ilgili diğer bilgileri ASP.NET web sitesinden kullanılabilir ( https://www.asp.net).</span><span class="sxs-lookup"><span data-stu-id="ed63f-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="ed63f-128">ASP.NET MVC 5.1 yeni özellikler</span><span class="sxs-lookup"><span data-stu-id="ed63f-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="ed63f-129">Yönlendirme geliştirmeleri özniteliği</span><span class="sxs-lookup"><span data-stu-id="ed63f-129">Attribute routing improvements</span></span>

 <span data-ttu-id="ed63f-130">Öznitelik şimdi destekler kısıtlamaları, sürüm ve üstbilgi etkinleştirme yönlendirme rota seçimi temel.</span><span class="sxs-lookup"><span data-stu-id="ed63f-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="ed63f-131">Öznitelik rotaları pek çok görünüşünün şimdi aracılığıyla özelleştirilebilir `IDirectRouteFactory` arabirimi ve `RouteFactoryAttribute` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ed63f-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="ed63f-132">Rota öneki artık yoluyla Genişletilebilir `IRoutePrefix` arabirimi ve `RoutePrefixAttribute` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="ed63f-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="ed63f-133">Görünümlerde enum desteği</span><span class="sxs-lookup"><span data-stu-id="ed63f-133">Enum support in views</span></span>

1. <span data-ttu-id="ed63f-134">Yeni `@Html.EnumDropDownListFor()` yardımcı yöntemler.</span><span class="sxs-lookup"><span data-stu-id="ed63f-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="ed63f-135">Bu ifade değerlendirilmelidir uyarısıyla ile HTML Yardımcıları çoğunu gibi kullanılması gereken bir [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) türü veya [null atanabilir&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) burada `T` bir olduğu[enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) türü.</span><span class="sxs-lookup"><span data-stu-id="ed63f-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="ed63f-136">Kullanım `EnumHelper.IsValidForEnumHelper()` bu gereksinimleri denetlemek için.</span><span class="sxs-lookup"><span data-stu-id="ed63f-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="ed63f-137">Yeni `EnumHelper.GetSelectList()` döndürmesi yöntemleri bir `IList<SelectListItem>`.</span><span class="sxs-lookup"><span data-stu-id="ed63f-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="ed63f-138">Çağrılırken, örneğin, önce bir seçim listesi yönlendirmek gerektiğinde bu faydalıdır `@Html.DropDownListFor()`, veya adlarını görüntülemek istediğiniz zaman hangi `@Html.EnumDropDownListFor()` gösterir.</span><span class="sxs-lookup"><span data-stu-id="ed63f-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="ed63f-139">Aşağıdaki kod bu API'leri gösterir.</span><span class="sxs-lookup"><span data-stu-id="ed63f-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="ed63f-140">Tam bir örnek görebilirsiniz [burada](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span><span class="sxs-lookup"><span data-stu-id="ed63f-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="ed63f-141">Düzenleyici şablonları önyükleme desteği</span><span class="sxs-lookup"><span data-stu-id="ed63f-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="ed63f-142">Şimdi geçirme olarak HTML özniteliklerindeki izin veriyoruz [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) olarak bir [anonim nesneyi](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span><span class="sxs-lookup"><span data-stu-id="ed63f-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="ed63f-143">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ed63f-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="ed63f-144">MinLengthAttribute ve MaxLengthAttribute için örtük doğrulama</span><span class="sxs-lookup"><span data-stu-id="ed63f-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="ed63f-145">İstemci tarafı doğrulama dize ve dizi türleri için artık özellikleri ile donatılmış için desteklenecektir [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) ve [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) öznitelikleri.</span><span class="sxs-lookup"><span data-stu-id="ed63f-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="ed63f-146">'This' bağlam içinde örtük Ajax destekleme</span><span class="sxs-lookup"><span data-stu-id="ed63f-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="ed63f-147">Geri arama işlevleri (`OnBegin, OnComplete, OnFailure, OnSuccess`) şimdi aracılığıyla bildirilecekse öğesini bulun kuramaz `this` bağlamı.</span><span class="sxs-lookup"><span data-stu-id="ed63f-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="ed63f-148">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="ed63f-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="ed63f-149">Bilinen sorunlar ve yeni değişiklikler</span><span class="sxs-lookup"><span data-stu-id="ed63f-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="ed63f-150">Öznitelik yönlendirme</span><span class="sxs-lookup"><span data-stu-id="ed63f-150">Attribute Routing</span></span>

<span data-ttu-id="ed63f-151">Öznitelik yönlendirme eşleşmeleri belirsizlikleri şimdi ilk eşleşmeye seçmek yerine bir hata bildirir.</span><span class="sxs-lookup"><span data-stu-id="ed63f-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="ed63f-152">Öznitelik rotaları yasaklanmış kullanarak `{controller}` parametresini kullanarak gelen ve giden `{action}` rota parametresi yerleştirilen eylemleri.</span><span class="sxs-lookup"><span data-stu-id="ed63f-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="ed63f-153">Bu parametreler kullanımları, büyük olasılıkla belirsizlikleri için yol açar.</span><span class="sxs-lookup"><span data-stu-id="ed63f-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="ed63f-154">Yapı iskelesi MVC/Web API projesine projede zaten mevcut olanları 5.0 paketleri 5.1 paketleri sonuçlarında ile</span><span class="sxs-lookup"><span data-stu-id="ed63f-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="ed63f-155">ASP.NET MVC 5.1 RTM için NuGet paketlerini güncelleştirme ASP.NET yapı iskelesi gibi Visual Studio araçlarını veya ASP.NET Web uygulaması proje şablonu güncelleştirmez.</span><span class="sxs-lookup"><span data-stu-id="ed63f-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="ed63f-156">ASP.NET çalışma zamanı paketleri (5.0.0.0)'ın önceki sürümünü kullanırlar.</span><span class="sxs-lookup"><span data-stu-id="ed63f-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="ed63f-157">Sonuç olarak, bunlar zaten projelerinizde yoksa ASP.NET yapı iskelesi gerekli paketleri, önceki sürümü (5.0.0.0) yükleyin.</span><span class="sxs-lookup"><span data-stu-id="ed63f-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="ed63f-158">Ancak, Visual Studio 2013 RTM veya güncelleştirme 1 ASP.NET iskele projelerinizi son paketlerinde üzerine yazmaz.</span><span class="sxs-lookup"><span data-stu-id="ed63f-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="ed63f-159">Web API 2.1 veya ASP.NET MVC 5.1 projelerinizi paketleri güncelleştirdikten sonra ASP.NET yapı iskelesi kullanırsanız, Web API ve ASP.NET MVC sürümleri tutarlı olduğundan emin olun.</span><span class="sxs-lookup"><span data-stu-id="ed63f-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="ed63f-160">Visual Studio 2013'te Razor görünümleri için söz dizimi vurgulama</span><span class="sxs-lookup"><span data-stu-id="ed63f-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="ed63f-161">Visual Studio 2013 güncelleştirmeden ASP.NET MVC 5.1 RTM için'ı güncelleştirirseniz, Visual Studio Düzenleyicisi desteği Razor görünümleri düzenlerken, sözdizimi vurgulama için almazsınız.</span><span class="sxs-lookup"><span data-stu-id="ed63f-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="ed63f-162">Bu destek almak için Visual Studio 2013 güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="ed63f-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="ed63f-163">Türü yeniden adlandırma</span><span class="sxs-lookup"><span data-stu-id="ed63f-163">Type Renames</span></span>

<span data-ttu-id="ed63f-164">Bazı öznitelik yönlendirme genişletilebilirliği için kullanılan türleri 5.1 RTM adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="ed63f-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="ed63f-165">**Eski tür adı (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="ed63f-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="ed63f-166">**Yeni tür adı (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="ed63f-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="ed63f-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="ed63f-167">IDirectRouteProvider</span></span> | <span data-ttu-id="ed63f-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="ed63f-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="ed63f-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="ed63f-169">RouteProviderAttribute</span></span> | <span data-ttu-id="ed63f-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="ed63f-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="ed63f-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="ed63f-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="ed63f-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="ed63f-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="ed63f-173">Hata Düzeltmeleri</span><span class="sxs-lookup"><span data-stu-id="ed63f-173">Bug Fixes</span></span>

<span data-ttu-id="ed63f-174">Bu sürüm çeşitli hata düzeltmeleri de içerir.</span><span class="sxs-lookup"><span data-stu-id="ed63f-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="ed63f-175">Tam listesini burada bulabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ed63f-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="ed63f-176">5.1.0 package</span><span class="sxs-lookup"><span data-stu-id="ed63f-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="ed63f-177">5.1.1 paketi</span><span class="sxs-lookup"><span data-stu-id="ed63f-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="ed63f-178">5.1.2 paket IntelliSense güncelleştirmeler ancak hiçbir hata düzeltmeleri içerir.</span><span class="sxs-lookup"><span data-stu-id="ed63f-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
