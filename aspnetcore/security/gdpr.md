---
title: ASP.NET core'da genel veri koruma yönetmeliği (GDPR) desteği
author: rick-anderson
description: Bir ASP.NET Core web uygulaması GDPR uzantı noktaları erişmeyi öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: security/gdpr
ms.openlocfilehash: 01d2f8943c0995c1400122b89c4ca7c459a85279
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724572"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="10742-103">ASP.NET Core AB genel veri koruma yönetmeliği (GDPR) desteği</span><span class="sxs-lookup"><span data-stu-id="10742-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="10742-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="10742-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="10742-105">ASP.NET Core API'leri ve şablonları bazı karşılamanıza yardımcı olmak üzere sağlar [AB genel veri koruma yönetmeliği (GDPR)](https://www.eugdpr.org/) gereksinimleri:</span><span class="sxs-lookup"><span data-stu-id="10742-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="10742-106">Proje şablonları, uzantı noktaları ve gizlilik ve tanımlama bilgisi kullan ilke ile değiştirebilirsiniz saptama biçimlendirme içerir.</span><span class="sxs-lookup"><span data-stu-id="10742-106">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="10742-107">*Pages/Privacy.cshtml* sayfası veya *Views/Home/Privacy.cshtml* ayrıntı sitenizin gizlilik ilkesi için bir sayfa görünümü sağlar.</span><span class="sxs-lookup"><span data-stu-id="10742-107">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span>

<span data-ttu-id="10742-108">Gibi varsayılan tanımlama bilgisi onayı özelliği etkinleştirmek için bir ASP.NET Core 3.0 Şablonu oluşturulan uygulamada ASP.NET çekirdek 2.2 şablonları bulunamadı:</span><span class="sxs-lookup"><span data-stu-id="10742-108">To enable the default cookie consent feature like that found in the ASP.NET Core 2.2 templates in an ASP.NET Core 3.0 template generated app:</span></span>

* <span data-ttu-id="10742-109">Ekleme [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) çok `Startup.ConfigureServices` ve [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) için `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="10742-109">Add [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) too `Startup.ConfigureServices` and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) to `Startup.Configure`:</span></span>

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* <span data-ttu-id="10742-110">Ekleme için tanımlama bilgisi onayı kısmi *_Layout.cshtml* dosyası:</span><span class="sxs-lookup"><span data-stu-id="10742-110">Add the cookie consent partial to the *_Layout.cshtml* file:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* <span data-ttu-id="10742-111">Ekleme  *\_CookieConsentPartial.cshtml* projeye dosya:</span><span class="sxs-lookup"><span data-stu-id="10742-111">Add the *\_CookieConsentPartial.cshtml* file to the project:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* <span data-ttu-id="10742-112">Bu makalede, tanımlama bilgisi onayı özelliği hakkında okumak için ASP.NET Core 2.2 sürümünü seçin.</span><span class="sxs-lookup"><span data-stu-id="10742-112">Select the ASP.NET Core 2.2 version of this article to read about the cookie consent feature.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* <span data-ttu-id="10742-113">Proje şablonları, uzantı noktaları ve gizlilik ve tanımlama bilgisi kullan ilke ile değiştirebilirsiniz saptama biçimlendirme içerir.</span><span class="sxs-lookup"><span data-stu-id="10742-113">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="10742-114">Bir tanımlama bilgisi onay özelliği, kişisel bilgileri depolamak için onay isteyin (ve izlemek), kullanıcılarınızdan gelen sağlar.</span><span class="sxs-lookup"><span data-stu-id="10742-114">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="10742-115">Veri toplama için bir kullanıcı tarafından onaylanan taşınmadığından ve uygulamasına sahipse [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) kümesine `true`, gerekli olmayan tanımlama bilgileri, tarayıcıya gönderilen değildir.</span><span class="sxs-lookup"><span data-stu-id="10742-115">If a user hasn't consented to data collection and the app has [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) set to `true`, non-essential cookies aren't sent to the browser.</span></span>
* <span data-ttu-id="10742-116">Tanımlama bilgileri, önemli olarak işaretlenebilir.</span><span class="sxs-lookup"><span data-stu-id="10742-116">Cookies can be marked as essential.</span></span> <span data-ttu-id="10742-117">Temel tanımlama, kullanıcı onaylı olmayan ve izleme devre dışı bile tarayıcıya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="10742-117">Essential cookies are sent to the browser even when the user hasn't consented and tracking is disabled.</span></span>
* <span data-ttu-id="10742-118">[TempData ve oturum tanımlama bilgilerini](#tempdata) izleme devre dışı bırakıldığında işlevsel değildir.</span><span class="sxs-lookup"><span data-stu-id="10742-118">[TempData and Session cookies](#tempdata) aren't functional when tracking is disabled.</span></span>
* <span data-ttu-id="10742-119">[Kimlik Yönetimi](#pd) sayfası, indirme ve kullanıcı verilerini silmek için bir bağlantı sağlar.</span><span class="sxs-lookup"><span data-stu-id="10742-119">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="10742-120">[Örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) GDPR uzantı noktaları çoğu test ve API'leri eklenen ASP.NET Core 2.1 şablonlara izin verir.</span><span class="sxs-lookup"><span data-stu-id="10742-120">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) allows you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="10742-121">Bkz: [Benioku](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) yönergeleri test dosyası.</span><span class="sxs-lookup"><span data-stu-id="10742-121">See the [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="10742-122">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="10742-122">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="10742-123">ASP.NET Core GDPR şablon tarafından oluşturulan kodda desteği</span><span class="sxs-lookup"><span data-stu-id="10742-123">ASP.NET Core GDPR support in template-generated code</span></span>

<span data-ttu-id="10742-124">Razor sayfaları ve MVC proje şablonları ile oluşturulan projeleri aşağıdaki GDPR desteği içerir:</span><span class="sxs-lookup"><span data-stu-id="10742-124">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="10742-125">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) ve [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) ayarlanan `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="10742-125">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in the `Startup` class.</span></span>
* <span data-ttu-id="10742-126">*\_CookieConsentPartial.cshtml* [kısmi Görünüm](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="10742-126">The *\_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="10742-127">Bir **kabul** düğmesi bu dosyasına eklenir.</span><span class="sxs-lookup"><span data-stu-id="10742-127">An **Accept** button is included in this file.</span></span> <span data-ttu-id="10742-128">Kullanıcı tıkladığında **kabul** düğmesi, tanımlama bilgilerini depolamak için onay sağlanır.</span><span class="sxs-lookup"><span data-stu-id="10742-128">When the user clicks the **Accept** button, consent to store cookies is provided.</span></span>
* <span data-ttu-id="10742-129">*Pages/Privacy.cshtml* sayfası veya *Views/Home/Privacy.cshtml* ayrıntı sitenizin gizlilik ilkesi için bir sayfa görünümü sağlar.</span><span class="sxs-lookup"><span data-stu-id="10742-129">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="10742-130">*\_CookieConsentPartial.cshtml* dosyası gizlilik sayfasına bir bağlantı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="10742-130">The *\_CookieConsentPartial.cshtml* file generates a link to the Privacy page.</span></span>
* <span data-ttu-id="10742-131">Bireysel kullanıcı hesapları ile oluşturulan uygulamalar için indirme ve silme için bağlantıları Yönet sayfasında sağlar [kişisel kullanıcı verileri](#pd).</span><span class="sxs-lookup"><span data-stu-id="10742-131">For apps created with individual user accounts, the Manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="10742-132">CookiePolicyOptions ve UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="10742-132">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="10742-133">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) içinde başlatılan `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="10742-133">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="10742-134">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) çağrılma yeri `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="10742-134">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in `Startup.Configure`:</span></span>

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="10742-135">\_CookieConsentPartial.cshtml kısmi Görünüm</span><span class="sxs-lookup"><span data-stu-id="10742-135">\_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="10742-136">*\_CookieConsentPartial.cshtml* kısmi görünümü:</span><span class="sxs-lookup"><span data-stu-id="10742-136">The *\_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="10742-137">Bu kısmi:</span><span class="sxs-lookup"><span data-stu-id="10742-137">This partial:</span></span>

* <span data-ttu-id="10742-138">Kullanıcı için izleme durumunu alır.</span><span class="sxs-lookup"><span data-stu-id="10742-138">Obtains the state of tracking for the user.</span></span> <span data-ttu-id="10742-139">Uygulama izni istemek için yapılandırılmışsa, tanımlama bilgileri izlenebilir önce kullanıcının onaylaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="10742-139">If the app is configured to require consent, the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="10742-140">Onayı gerekiyorsa, tanımlama bilgisi onayı paneli tarafından oluşturulan gezinti çubuğunun üst sabitlenmiştir  *\_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="10742-140">If consent is required, the cookie consent panel is fixed at top of the navigation bar created by the *\_Layout.cshtml* file.</span></span>
* <span data-ttu-id="10742-141">Bir HTML sağlar `<p>` gizlilik ve tanımlama bilgisi özetlemek için öğesi İlkesi kullanın.</span><span class="sxs-lookup"><span data-stu-id="10742-141">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="10742-142">Gizlilik sayfasına veya, sitenizin gizlilik ilkesi olduğu ayrıntılı görünüm için bir bağlantı sağlar.</span><span class="sxs-lookup"><span data-stu-id="10742-142">Provides a link to Privacy page or view where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="10742-143">Temel tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="10742-143">Essential cookies</span></span>

<span data-ttu-id="10742-144">Varsa onay tanımlama bilgilerini depolamak için sağlanmış taşınmadığından, yalnızca önemli olarak işaretlenmiş tanımlama bilgilerini tarayıcıya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="10742-144">If consent to store cookies hasn't been provided, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="10742-145">Aşağıdaki kod, bir tanımlama bilgisi gerekli hale getirir:</span><span class="sxs-lookup"><span data-stu-id="10742-145">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a><span data-ttu-id="10742-146">TempData sağlayıcısı ve oturum durumu tanımlama bilgileri gerekli değil</span><span class="sxs-lookup"><span data-stu-id="10742-146">TempData provider and session state cookies aren't essential</span></span>

<span data-ttu-id="10742-147">[TempData sağlayıcısı](xref:fundamentals/app-state#tempdata) tanımlama bilgisi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="10742-147">The [TempData provider](xref:fundamentals/app-state#tempdata) cookie isn't essential.</span></span> <span data-ttu-id="10742-148">İzleme devre dışı bırakılırsa TempData sağlayıcısı işlevsel değildir.</span><span class="sxs-lookup"><span data-stu-id="10742-148">If tracking is disabled, the TempData provider isn't functional.</span></span> <span data-ttu-id="10742-149">İzleme devre dışı bırakıldığında TempData sağlayıcıyı etkinleştirmek için TempData tanımlama bilgisi, gerekli olarak işaretlemek `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="10742-149">To enable the TempData provider when tracking is disabled, mark the TempData cookie as essential in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

<span data-ttu-id="10742-150">[Oturum durumu](xref:fundamentals/app-state) tanımlama bilgisi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="10742-150">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="10742-151">İzleme devre dışı bırakıldığında, oturum durumu işlevsel değildir.</span><span class="sxs-lookup"><span data-stu-id="10742-151">Session state isn't functional when tracking is disabled.</span></span> <span data-ttu-id="10742-152">Aşağıdaki kod, oturum tanımlama bilgileri önemli hale getirir:</span><span class="sxs-lookup"><span data-stu-id="10742-152">The following code makes session cookies essential:</span></span>

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="10742-153">Kişisel veriler</span><span class="sxs-lookup"><span data-stu-id="10742-153">Personal data</span></span>

<span data-ttu-id="10742-154">Oluşturulan kullanıcı hesaplarıyla ASP.NET Core uygulamaları indirmek ve kişisel verileri silmek için kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="10742-154">ASP.NET Core apps created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="10742-155">Kullanıcı adını seçin ve ardından **kişisel verileri**:</span><span class="sxs-lookup"><span data-stu-id="10742-155">Select the user name and then select **Personal data**:</span></span>

![Sayfa kişisel verileri yönetme](gdpr/_static/pd.png)

<span data-ttu-id="10742-157">Notlar:</span><span class="sxs-lookup"><span data-stu-id="10742-157">Notes:</span></span>

* <span data-ttu-id="10742-158">Oluşturulacak `Account/Manage` kod bkz [İskele kimlik](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="10742-158">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="10742-159">**Sil** ve **indirme** bağlantıları, yalnızca varsayılan kimlik verileri davranacak.</span><span class="sxs-lookup"><span data-stu-id="10742-159">The **Delete** and **Download** links only act on the default identity data.</span></span> <span data-ttu-id="10742-160">Özel kullanıcı verilerini oluşturduğunuz uygulamalar, özel kullanıcı verilerini silme/indirilecek genişletilmelidir.</span><span class="sxs-lookup"><span data-stu-id="10742-160">Apps that create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="10742-161">Daha fazla bilgi için [Ekle, indirme ve silme özel kullanıcı verilerini kimliğe](xref:security/authentication/add-user-data).</span><span class="sxs-lookup"><span data-stu-id="10742-161">For more information, see [Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data).</span></span>
* <span data-ttu-id="10742-162">Kimlik veritabanı tablosunda depolanan kullanıcı için belirteç kaydedildi `AspNetUserTokens` basamaklı silme davranışı nedeniyle aracılığıyla kullanıcı silindiğinde silinir [yabancı anahtar](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="10742-162">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>
* <span data-ttu-id="10742-163">[Dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index), Facebook ve Google değil gibi kullanılabilir tanımlama bilgisi ilkesi kabul edilmeden önce.</span><span class="sxs-lookup"><span data-stu-id="10742-163">[External provider authentication](xref:security/authentication/social/index), such as Facebook and Google, isn't available before the cookie policy is accepted.</span></span>

::: moniker-end

## <a name="encryption-at-rest"></a><span data-ttu-id="10742-164">Bekleme sırasında şifreleme</span><span class="sxs-lookup"><span data-stu-id="10742-164">Encryption at rest</span></span>

<span data-ttu-id="10742-165">Bekleme sırasında şifreleme için bazı veritabanları ve depolama mekanizmaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="10742-165">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="10742-166">Bekleme sırasında şifreleme:</span><span class="sxs-lookup"><span data-stu-id="10742-166">Encryption at rest:</span></span>

* <span data-ttu-id="10742-167">Depolanan veriler otomatik olarak şifreler.</span><span class="sxs-lookup"><span data-stu-id="10742-167">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="10742-168">Yapılandırma, programlama veya diğer iş verilere erişen bir yazılım içermeyen şifreler.</span><span class="sxs-lookup"><span data-stu-id="10742-168">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="10742-169">Kolay ve güvenli bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="10742-169">Is the easiest and safest option.</span></span>
* <span data-ttu-id="10742-170">Veritabanı, anahtarları ve şifreleme yönetmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="10742-170">Allows the database to manage keys and encryption.</span></span>

<span data-ttu-id="10742-171">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="10742-171">For example:</span></span>

* <span data-ttu-id="10742-172">Microsoft SQL ve Azure SQL [saydam veri şifrelemesi](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span><span class="sxs-lookup"><span data-stu-id="10742-172">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="10742-173">SQL Azure veritabanının varsayılan olarak şifreler.</span><span class="sxs-lookup"><span data-stu-id="10742-173">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="10742-174">[Azure Blobları, dosyalar, tablo ve kuyruk depolama varsayılan olarak şifrelenmiş](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="10742-174">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="10742-175">Bekleyen yerleşik şifreleme sağlamayan veritabanları için aynı koruma sağlamak için disk şifreleme kullanmanız mümkün olabilir.</span><span class="sxs-lookup"><span data-stu-id="10742-175">For databases that don't provide built-in encryption at rest, you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="10742-176">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="10742-176">For example:</span></span>

* [<span data-ttu-id="10742-177">Windows Server için BitLocker'ı</span><span class="sxs-lookup"><span data-stu-id="10742-177">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="10742-178">Linux:</span><span class="sxs-lookup"><span data-stu-id="10742-178">Linux:</span></span>
  * [<span data-ttu-id="10742-179">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="10742-179">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="10742-180">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="10742-180">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="10742-181">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="10742-181">Additional resources</span></span>

* [<span data-ttu-id="10742-182">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="10742-182">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/trustcenter/Privacy/GDPR)
