---
title: ASP.NET Core genel veri koruma düzenleme (GDPR) desteği
author: rick-anderson
description: Bir ASP.NET Core GDPR uzantısı noktaların erişmek web uygulaması gösterilmiştir.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 5/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: 92a7000f4f8e4c2097065cb530fe106ef0e98545
ms.sourcegitcommit: 545ff5a632e2281035c1becec1f99137298e4f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2018
ms.locfileid: "34688633"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="3cf66-103">ASP.NET Core AB genel veri koruma düzenleme (GDPR) desteği</span><span class="sxs-lookup"><span data-stu-id="3cf66-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="3cf66-104">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3cf66-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="3cf66-105">ASP.NET Core API ve şablonları bazı karşılamak amacıyla sağlar [AB genel veri koruma düzenleme (GDPR)](https://www.eugdpr.org/) gereksinimleri:</span><span class="sxs-lookup"><span data-stu-id="3cf66-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="3cf66-106">Proje şablonları uzantı noktaları ve gizlilik ve tanımlama bilgisi kullanım ilkesi ile değiştirebilirsiniz tamamlanmamış biçimlendirme içerir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-106">The project templates include extension points and stubbed markup you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="3cf66-107">Bir tanımlama bilgisi onayı özelliği onay için sormak (ve izlemek), kullanıcıların kişisel bilgilerini depolamak için sağlar.</span><span class="sxs-lookup"><span data-stu-id="3cf66-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="3cf66-108">Bir kullanıcı için veri toplama seçtiği değil ve uygulama ile ayarlanırsa [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) için `true`, gerekli olmayan tanımlama bilgilerini tarayıcıya gönderilmeyecek.</span><span class="sxs-lookup"><span data-stu-id="3cf66-108">If a user has not consented to data collection and the app is set with [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded?view=aspnetcore-2.1#Microsoft_AspNetCore_Builder_CookiePolicyOptions_CheckConsentNeeded) to `true`, non-essential cookies will not be sent to the browser.</span></span>
* <span data-ttu-id="3cf66-109">Tanımlama bilgileri gibi önemli olarak işaretlenebilir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="3cf66-110">Temel tanımlama bilgileri kullanıcının değil seçtiği ve izleme devre dışı bile tarayıcıya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-110">Essential cookies are sent to the browser even when the user has not consented and tracking is disabled.</span></span>
* <span data-ttu-id="3cf66-111">[TempData ve oturum tanımlama bilgileri](#tempdata) izleme devre dışı bırakıldığında işlevsel değildir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-111">[TempData and Session cookies](#tempdata) are not functional when tracking is disabled.</span></span>
* <span data-ttu-id="3cf66-112">[Kimlik](#pd) sayfası, indirin ve kullanıcı verilerini silmek için bir bağlantı sağlar.</span><span class="sxs-lookup"><span data-stu-id="3cf66-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="3cf66-113">[Örnek uygulaması](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) GDPR uzantı noktaları ve ASP.NET Core 2.1 şablonları eklenen API'leri çoğunu test sağlar.</span><span class="sxs-lookup"><span data-stu-id="3cf66-113">The [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="3cf66-114">Bkz: [Benioku](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) yönergeleri sınama dosyası.</span><span class="sxs-lookup"><span data-stu-id="3cf66-114">See the [ReadMe](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="3cf66-115">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3cf66-115">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="3cf66-116">Oluşturulan şablon kodunun ASP.NET Core GDPR desteği</span><span class="sxs-lookup"><span data-stu-id="3cf66-116">ASP.NET Core GDPR support in template generated code</span></span>

<span data-ttu-id="3cf66-117">Razor sayfalarının ve MVC proje şablonları ile oluşturulan projeleri aşağıdaki GDPR desteği içerir:</span><span class="sxs-lookup"><span data-stu-id="3cf66-117">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="3cf66-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) ve [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) ayarlanır `Startup`.</span><span class="sxs-lookup"><span data-stu-id="3cf66-118">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) are set in `Startup`.</span></span>
* <span data-ttu-id="3cf66-119">*_CookieConsentPartial.cshtml* [kısmi Görünüm](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="3cf66-119">The *_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span>
* <span data-ttu-id="3cf66-120">*Pages/Privacy.cshtml* veya *Home/Privacy.cshtml* görünümü, bir sayfa, sitenin gizlilik ilkesi ayrıntı sağlar.</span><span class="sxs-lookup"><span data-stu-id="3cf66-120">The *Pages/Privacy.cshtml* or *Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="3cf66-121">*_CookieConsentPartial.cshtml* dosyası gizlilik sayfasına bir bağlantı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="3cf66-121">The *_CookieConsentPartial.cshtml* file generates a link to the privacy page.</span></span>
* <span data-ttu-id="3cf66-122">Bireysel kullanıcı hesapları ile oluşturulan uygulamalar için Yönet sayfasını indirin ve silmek için bağlantılar sağlar. [kişisel kullanıcı verilerini](#pd).</span><span class="sxs-lookup"><span data-stu-id="3cf66-122">For applications created with individual user accounts, the manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="3cf66-123">CookiePolicyOptions ve UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="3cf66-123">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="3cf66-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) içinde başlatılan `Startup` sınıfı `ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3cf66-124">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions?view=aspnetcore-2.0) are initialized in the `Startup` class `ConfigureServices` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="3cf66-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) olarak adlandırılır `Startup` sınıfı `Configure` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="3cf66-125">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_CookiePolicyAppBuilderExtensions_UseCookiePolicy_Microsoft_AspNetCore_Builder_IApplicationBuilder_) is called in the `Startup` class `Configure` method:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="3cf66-126">_CookieConsentPartial.cshtml kısmi görünümü</span><span class="sxs-lookup"><span data-stu-id="3cf66-126">_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="3cf66-127">*_CookieConsentPartial.cshtml* kısmi görünümü:</span><span class="sxs-lookup"><span data-stu-id="3cf66-127">The *_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="3cf66-128">Bu kısmi:</span><span class="sxs-lookup"><span data-stu-id="3cf66-128">This partial:</span></span>

* <span data-ttu-id="3cf66-129">Kullanıcı için izleme durumunu alır.</span><span class="sxs-lookup"><span data-stu-id="3cf66-129">Gets the state of tracking for the user.</span></span> <span data-ttu-id="3cf66-130">Uygulama onayı gerektirecek şekilde yapılandırılmışsa, tanımlama bilgilerini izlenebilir önce kullanıcının onaylaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-130">If the application is configured to require consent the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="3cf66-131">Onay gerekiyorsa, tanımlama bilgisi onayı chrome oluşturulan gezinti çubuğu üstünde sabittir *Pages/Shared/_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="3cf66-131">If consent is required, the cookie consent chrome is fixed on top of the navigation bar created in the *Pages/Shared/_Layout.cshtml* file.</span></span>
* <span data-ttu-id="3cf66-132">Bir HTML sağlar `<p>` gizlilik ve tanımlama bilgisi özetlemek için öğesi İlkesi kullanın.</span><span class="sxs-lookup"><span data-stu-id="3cf66-132">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="3cf66-133">Bir bağlantı sağlar *Pages/Privacy.cshtml* Burada, ayrıntı, sitenin gizlilik ilkesi.</span><span class="sxs-lookup"><span data-stu-id="3cf66-133">Provides a link to *Pages/Privacy.cshtml* where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="3cf66-134">Temel tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="3cf66-134">Essential cookies</span></span>

<span data-ttu-id="3cf66-135">İzin verilen değil, yalnızca önemli olarak işaretlenmiş tanımlama bilgilerini tarayıcıya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-135">If consent has not been given, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="3cf66-136">Aşağıdaki kod bir tanımlama bilgisi gerekli hale getirir:</span><span class="sxs-lookup"><span data-stu-id="3cf66-136">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a><span data-ttu-id="3cf66-137">Tempdata sağlayıcısı ve oturum durumu tanımlama bilgisi gerekli değildir</span><span class="sxs-lookup"><span data-stu-id="3cf66-137">Tempdata provider and session state cookies are not essential</span></span>

<span data-ttu-id="3cf66-138">[Tempdata sağlayıcı](xref:fundamentals/app-state#tempdata) tanımlama bilgisi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-138">The [Tempdata provider](xref:fundamentals/app-state#tempdata) cookie is not essential.</span></span> <span data-ttu-id="3cf66-139">İzleme devre dışıysa, Tempdata sağlayıcı işlevsel değildir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-139">If tracking is disabled, the Tempdata provider is not functional.</span></span> <span data-ttu-id="3cf66-140">İzleme devre dışı bırakıldığında Tempdata Sağlayıcısı'nı etkinleştirmek için TempData tanımlama bilgisi, temel olarak işaretlemek `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3cf66-140">To enable the Tempdata provider when tracking is disabled, mark the TempData cookie as essential in `ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="3cf66-141">[Oturum durumu](xref:fundamentals/app-state) tanımlama bilgisi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-141">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="3cf66-142">İzleme devre dışı bırakıldığında oturum durumu işlevsel değildir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-142">Session state is not functional when tracking is disabled.</span></span>

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="3cf66-143">Kişisel veriler</span><span class="sxs-lookup"><span data-stu-id="3cf66-143">Personal data</span></span>

<span data-ttu-id="3cf66-144">Bireysel kullanıcı hesapları ile oluşturulan ASP.NET Core uygulamaları indirmek ve kişisel verilerini silmek için kodu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="3cf66-144">ASP.NET Core applications created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="3cf66-145">Kullanıcı adını seçin ve ardından **kişisel veriler**:</span><span class="sxs-lookup"><span data-stu-id="3cf66-145">Select the user name and then select **Personal data**:</span></span>

![Kişisel veri sayfası yönetme](gdpr/_static/pd.png)

<span data-ttu-id="3cf66-147">Notlar:</span><span class="sxs-lookup"><span data-stu-id="3cf66-147">Notes:</span></span>

* <span data-ttu-id="3cf66-148">Oluşturulacak `Account/Manage` kod, bkz: [İskele kimlik](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="3cf66-148">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="3cf66-149">Sil ve yalnızca etkisi varsayılan kimlik verilerini indirin.</span><span class="sxs-lookup"><span data-stu-id="3cf66-149">Delete and download only impact the default identity data.</span></span> <span data-ttu-id="3cf66-150">Uygulamaları oluşturma özel kullanıcı verilerini silme/özel kullanıcı verilerini indirme genişletilmelidir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-150">Apps the create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="3cf66-151">GitHub sorunu [nasıl kimlik özel kullanıcı verilerini ekleme/silme](https://github.com/aspnet/Docs/issues/6226) özel/silme/indirme özel kullanıcı verilerini oluşturma önerilen bir makale izler.</span><span class="sxs-lookup"><span data-stu-id="3cf66-151">GitHub issue [How to add/delete custom user data to Identity](https://github.com/aspnet/Docs/issues/6226) tracks a proposed article on creating custom/deleting/downloading custom user data.</span></span> <span data-ttu-id="3cf66-152">Bu konuda öncelik görmek istediğiniz bir başparmak tepki yukarı sorunu bırakın.</span><span class="sxs-lookup"><span data-stu-id="3cf66-152">If you'd like to see that topic prioritized, leave a thumbs up reaction in the issue.</span></span>
* <span data-ttu-id="3cf66-153">Kullanıcı için kimlik veritabanı tablosunda depolanır belirteç kaydedilmiş `AspNetUserTokens` kullanıcı art arda silme davranışı nedeniyle aracılığıyla silindiğinde, silinen [yabancı anahtar](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="3cf66-153">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/b4fc72c944e0589a7e1f076794d7e5d8dcf163bf/src/EF/IdentityUserContext.cs#L152).</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="3cf66-154">Bekleyen şifreleme</span><span class="sxs-lookup"><span data-stu-id="3cf66-154">Encryption at rest</span></span>

<span data-ttu-id="3cf66-155">Bazı veritabanları ve depolama mekanizmaları bekleyen şifreleme izin verir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-155">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="3cf66-156">Bekleyen şifreleme:</span><span class="sxs-lookup"><span data-stu-id="3cf66-156">Encryption at rest:</span></span>

* <span data-ttu-id="3cf66-157">Depolanan veriler otomatik olarak şifreler.</span><span class="sxs-lookup"><span data-stu-id="3cf66-157">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="3cf66-158">Yapılandırma, programlama veya diğer iş verilere erişen yazılım olmadan şifreler.</span><span class="sxs-lookup"><span data-stu-id="3cf66-158">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="3cf66-159">Kolay ve güvenli bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-159">Is the easiest and safest option.</span></span>
* <span data-ttu-id="3cf66-160">Anahtarları ve şifrelemeyi yönetme veritabanı sağlar.</span><span class="sxs-lookup"><span data-stu-id="3cf66-160">Lets the database manage keys and encryption.</span></span>

<span data-ttu-id="3cf66-161">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3cf66-161">For example:</span></span>

* <span data-ttu-id="3cf66-162">Microsoft SQL ve Azure SQL sağlamak [saydam veri şifreleme](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span><span class="sxs-lookup"><span data-stu-id="3cf66-162">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) (TDE).</span></span>
* [<span data-ttu-id="3cf66-163">SQL Azure veritabanı varsayılan olarak şifreler.</span><span class="sxs-lookup"><span data-stu-id="3cf66-163">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/en-us/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="3cf66-164">[Azure BLOB'ları, dosyaları, tablo ve kuyruk depolama varsayılan olarak şifrelenir](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="3cf66-164">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/en-us/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="3cf66-165">Bekleyen yerleşik şifreleme sağlamıyorsa veritabanları için aynı koruma sağlamak üzere disk şifrelemesi kullanmak mümkün olabilir.</span><span class="sxs-lookup"><span data-stu-id="3cf66-165">For databases that don't provide built-in encryption at rest you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="3cf66-166">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="3cf66-166">For example:</span></span>

* [<span data-ttu-id="3cf66-167">Windows server için BitLocker'ı</span><span class="sxs-lookup"><span data-stu-id="3cf66-167">Bitlocker for windows server</span></span>](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="3cf66-168">Linux:</span><span class="sxs-lookup"><span data-stu-id="3cf66-168">Linux:</span></span>
  * [<span data-ttu-id="3cf66-169">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="3cf66-169">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="3cf66-170">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="3cf66-170">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3cf66-171">Ek Kaynaklar</span><span class="sxs-lookup"><span data-stu-id="3cf66-171">Additional Resources</span></span>

* [<span data-ttu-id="3cf66-172">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="3cf66-172">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/en-us/trustcenter/Privacy/GDPR)
