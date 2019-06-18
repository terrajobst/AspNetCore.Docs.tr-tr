---
title: ASP.NET core'da genel veri koruma yönetmeliği (GDPR) desteği
author: rick-anderson
description: Bir ASP.NET Core web uygulaması GDPR uzantı noktaları erişmeyi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/05/2019
uid: security/gdpr
ms.openlocfilehash: 1580187afef56e8e2f5be7a4bae32912e6305c5a
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2019
ms.locfileid: "67152862"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="92fd6-103">ASP.NET Core AB genel veri koruma yönetmeliği (GDPR) desteği</span><span class="sxs-lookup"><span data-stu-id="92fd6-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="92fd6-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="92fd6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="92fd6-105">ASP.NET Core API'leri ve şablonları bazı karşılamanıza yardımcı olmak üzere sağlar [AB genel veri koruma yönetmeliği (GDPR)](https://www.eugdpr.org/) gereksinimleri:</span><span class="sxs-lookup"><span data-stu-id="92fd6-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements:</span></span>

* <span data-ttu-id="92fd6-106">Proje şablonları, uzantı noktaları ve gizlilik ve tanımlama bilgisi kullan ilke ile değiştirebilirsiniz saptama biçimlendirme içerir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-106">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="92fd6-107">Bir tanımlama bilgisi onay özelliği, kişisel bilgileri depolamak için onay isteyin (ve izlemek), kullanıcılarınızdan gelen sağlar.</span><span class="sxs-lookup"><span data-stu-id="92fd6-107">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="92fd6-108">Veri toplama için bir kullanıcı tarafından onaylanan taşınmadığından ve uygulamasına sahipse [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) kümesine `true`, gerekli olmayan tanımlama bilgileri, tarayıcıya gönderilen değildir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-108">If a user hasn't consented to data collection and the app has [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) set to `true`, non-essential cookies aren't sent to the browser.</span></span>
* <span data-ttu-id="92fd6-109">Tanımlama bilgileri, önemli olarak işaretlenebilir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-109">Cookies can be marked as essential.</span></span> <span data-ttu-id="92fd6-110">Temel tanımlama, kullanıcı onaylı olmayan ve izleme devre dışı bile tarayıcıya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-110">Essential cookies are sent to the browser even when the user hasn't consented and tracking is disabled.</span></span>
* <span data-ttu-id="92fd6-111">[TempData ve oturum tanımlama bilgilerini](#tempdata) izleme devre dışı bırakıldığında işlevsel değildir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-111">[TempData and Session cookies](#tempdata) aren't functional when tracking is disabled.</span></span>
* <span data-ttu-id="92fd6-112">[Kimlik Yönetimi](#pd) sayfası, indirme ve kullanıcı verilerini silmek için bir bağlantı sağlar.</span><span class="sxs-lookup"><span data-stu-id="92fd6-112">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="92fd6-113">[Örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) GDPR uzantı noktaları çoğu test ve API'leri eklenen ASP.NET Core 2.1 şablonlara izin verir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-113">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) allows you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="92fd6-114">Bkz: [Benioku](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) yönergeleri test dosyası.</span><span class="sxs-lookup"><span data-stu-id="92fd6-114">See the [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="92fd6-115">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="92fd6-115">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="92fd6-116">ASP.NET Core GDPR şablon tarafından oluşturulan kodda desteği</span><span class="sxs-lookup"><span data-stu-id="92fd6-116">ASP.NET Core GDPR support in template-generated code</span></span>

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="92fd6-117">Razor sayfaları ve MVC proje şablonları ile oluşturulan projeleri sahip GDPR veya tanımlama bilgisi onayı için destek yok.</span><span class="sxs-lookup"><span data-stu-id="92fd6-117">Razor Pages and MVC projects created with the project templates have no support for GDPR or cookie consent.</span></span> <span data-ttu-id="92fd6-118">GDPR eklemek, ASP.NET Core 2.2 şablonlarında oluşturulan kodu kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="92fd6-118">To add GDPR, copy the code generated in the ASP.NET Core 2.2 templates.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="92fd6-119">Razor sayfaları ve MVC proje şablonları ile oluşturulan projeleri aşağıdaki GDPR desteği içerir:</span><span class="sxs-lookup"><span data-stu-id="92fd6-119">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

::: moniker-end

* <span data-ttu-id="92fd6-120">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) ve [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) ayarlanan `Startup` sınıfı.</span><span class="sxs-lookup"><span data-stu-id="92fd6-120">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in the `Startup` class.</span></span>
* <span data-ttu-id="92fd6-121">*\_CookieConsentPartial.cshtml* [kısmi Görünüm](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="92fd6-121">The *\_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="92fd6-122">Bir **kabul** düğmesi bu dosyasına eklenir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-122">An **Accept** button is included in this file.</span></span> <span data-ttu-id="92fd6-123">Kullanıcı tıkladığında **kabul** düğmesi, tanımlama bilgilerini depolamak için onay sağlanır.</span><span class="sxs-lookup"><span data-stu-id="92fd6-123">When the user clicks the **Accept** button, consent to store cookies is provided.</span></span>
* <span data-ttu-id="92fd6-124">*Pages/Privacy.cshtml* sayfası veya *Views/Home/Privacy.cshtml* ayrıntı sitenizin gizlilik ilkesi için bir sayfa görünümü sağlar.</span><span class="sxs-lookup"><span data-stu-id="92fd6-124">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="92fd6-125">*\_CookieConsentPartial.cshtml* dosyası gizlilik sayfasına bir bağlantı oluşturur.</span><span class="sxs-lookup"><span data-stu-id="92fd6-125">The *\_CookieConsentPartial.cshtml* file generates a link to the Privacy page.</span></span>
* <span data-ttu-id="92fd6-126">Bireysel kullanıcı hesapları ile oluşturulan uygulamalar için indirme ve silme için bağlantıları Yönet sayfasında sağlar [kişisel kullanıcı verileri](#pd).</span><span class="sxs-lookup"><span data-stu-id="92fd6-126">For apps created with individual user accounts, the Manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a><span data-ttu-id="92fd6-127">CookiePolicyOptions ve UseCookiePolicy</span><span class="sxs-lookup"><span data-stu-id="92fd6-127">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="92fd6-128">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) içinde başlatılan `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="92fd6-128">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="92fd6-129">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) çağrılma yeri `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="92fd6-129">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in `Startup.Configure`:</span></span>

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="92fd6-130">\_CookieConsentPartial.cshtml kısmi Görünüm</span><span class="sxs-lookup"><span data-stu-id="92fd6-130">\_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="92fd6-131">*\_CookieConsentPartial.cshtml* kısmi görünümü:</span><span class="sxs-lookup"><span data-stu-id="92fd6-131">The *\_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-html[](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="92fd6-132">Bu kısmi:</span><span class="sxs-lookup"><span data-stu-id="92fd6-132">This partial:</span></span>

* <span data-ttu-id="92fd6-133">Kullanıcı için izleme durumunu alır.</span><span class="sxs-lookup"><span data-stu-id="92fd6-133">Obtains the state of tracking for the user.</span></span> <span data-ttu-id="92fd6-134">Uygulama izni istemek için yapılandırılmışsa, tanımlama bilgileri izlenebilir önce kullanıcının onaylaması gerekir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-134">If the app is configured to require consent, the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="92fd6-135">Onayı gerekiyorsa, tanımlama bilgisi onayı paneli tarafından oluşturulan gezinti çubuğunun üst sabitlenmiştir  *\_Layout.cshtml* dosya.</span><span class="sxs-lookup"><span data-stu-id="92fd6-135">If consent is required, the cookie consent panel is fixed at top of the navigation bar created by the *\_Layout.cshtml* file.</span></span>
* <span data-ttu-id="92fd6-136">Bir HTML sağlar `<p>` gizlilik ve tanımlama bilgisi özetlemek için öğesi İlkesi kullanın.</span><span class="sxs-lookup"><span data-stu-id="92fd6-136">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="92fd6-137">Gizlilik sayfasına veya, sitenizin gizlilik ilkesi olduğu ayrıntılı görünüm için bir bağlantı sağlar.</span><span class="sxs-lookup"><span data-stu-id="92fd6-137">Provides a link to Privacy page or view where you can detail your site's privacy policy.</span></span>

## <a name="essential-cookies"></a><span data-ttu-id="92fd6-138">Temel tanımlama bilgileri</span><span class="sxs-lookup"><span data-stu-id="92fd6-138">Essential cookies</span></span>

<span data-ttu-id="92fd6-139">Varsa onay tanımlama bilgilerini depolamak için sağlanmış taşınmadığından, yalnızca önemli olarak işaretlenmiş tanımlama bilgilerini tarayıcıya gönderilir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-139">If consent to store cookies hasn't been provided, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="92fd6-140">Aşağıdaki kod, bir tanımlama bilgisi gerekli hale getirir:</span><span class="sxs-lookup"><span data-stu-id="92fd6-140">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a><span data-ttu-id="92fd6-141">TempData sağlayıcısı ve oturum durumu tanımlama bilgileri gerekli değil</span><span class="sxs-lookup"><span data-stu-id="92fd6-141">TempData provider and session state cookies aren't essential</span></span>

<span data-ttu-id="92fd6-142">[TempData sağlayıcısı](xref:fundamentals/app-state#tempdata) tanımlama bilgisi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-142">The [TempData provider](xref:fundamentals/app-state#tempdata) cookie isn't essential.</span></span> <span data-ttu-id="92fd6-143">İzleme devre dışı bırakılırsa TempData sağlayıcısı işlevsel değildir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-143">If tracking is disabled, the TempData provider isn't functional.</span></span> <span data-ttu-id="92fd6-144">İzleme devre dışı bırakıldığında TempData sağlayıcıyı etkinleştirmek için TempData tanımlama bilgisi, gerekli olarak işaretlemek `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="92fd6-144">To enable the TempData provider when tracking is disabled, mark the TempData cookie as essential in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

<span data-ttu-id="92fd6-145">[Oturum durumu](xref:fundamentals/app-state) tanımlama bilgisi gerekli değildir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-145">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="92fd6-146">İzleme devre dışı bırakıldığında, oturum durumu işlevsel değildir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-146">Session state isn't functional when tracking is disabled.</span></span> <span data-ttu-id="92fd6-147">Aşağıdaki kod, oturum tanımlama bilgileri önemli hale getirir:</span><span class="sxs-lookup"><span data-stu-id="92fd6-147">The following code makes session cookies essential:</span></span>

[!code-csharp[](gdpr/sample/RP/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="92fd6-148">Kişisel veriler</span><span class="sxs-lookup"><span data-stu-id="92fd6-148">Personal data</span></span>

<span data-ttu-id="92fd6-149">Oluşturulan kullanıcı hesaplarıyla ASP.NET Core uygulamaları indirmek ve kişisel verileri silmek için kod ekleyin.</span><span class="sxs-lookup"><span data-stu-id="92fd6-149">ASP.NET Core apps created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="92fd6-150">Kullanıcı adını seçin ve ardından **kişisel verileri**:</span><span class="sxs-lookup"><span data-stu-id="92fd6-150">Select the user name and then select **Personal data**:</span></span>

![Sayfa kişisel verileri yönetme](gdpr/_static/pd.png)

<span data-ttu-id="92fd6-152">Notlar:</span><span class="sxs-lookup"><span data-stu-id="92fd6-152">Notes:</span></span>

* <span data-ttu-id="92fd6-153">Oluşturulacak `Account/Manage` kod bkz [İskele kimlik](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="92fd6-153">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="92fd6-154">**Sil** ve **indirme** bağlantıları, yalnızca varsayılan kimlik verileri davranacak.</span><span class="sxs-lookup"><span data-stu-id="92fd6-154">The **Delete** and **Download** links only act on the default identity data.</span></span> <span data-ttu-id="92fd6-155">Özel kullanıcı verilerini oluşturduğunuz uygulamalar, özel kullanıcı verilerini silme/indirilecek genişletilmelidir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-155">Apps that create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="92fd6-156">Daha fazla bilgi için [Ekle, indirme ve silme özel kullanıcı verilerini kimliğe](xref:security/authentication/add-user-data).</span><span class="sxs-lookup"><span data-stu-id="92fd6-156">For more information, see [Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data).</span></span>
* <span data-ttu-id="92fd6-157">Kimlik veritabanı tablosunda depolanan kullanıcı için belirteç kaydedildi `AspNetUserTokens` basamaklı silme davranışı nedeniyle aracılığıyla kullanıcı silindiğinde silinir [yabancı anahtar](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span><span class="sxs-lookup"><span data-stu-id="92fd6-157">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>
* <span data-ttu-id="92fd6-158">[Dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index), Facebook ve Google değil gibi kullanılabilir tanımlama bilgisi ilkesi kabul edilmeden önce.</span><span class="sxs-lookup"><span data-stu-id="92fd6-158">[External provider authentication](xref:security/authentication/social/index), such as Facebook and Google, isn't available before the cookie policy is accepted.</span></span>

## <a name="encryption-at-rest"></a><span data-ttu-id="92fd6-159">Bekleme sırasında şifreleme</span><span class="sxs-lookup"><span data-stu-id="92fd6-159">Encryption at rest</span></span>

<span data-ttu-id="92fd6-160">Bekleme sırasında şifreleme için bazı veritabanları ve depolama mekanizmaları sağlar.</span><span class="sxs-lookup"><span data-stu-id="92fd6-160">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="92fd6-161">Bekleme sırasında şifreleme:</span><span class="sxs-lookup"><span data-stu-id="92fd6-161">Encryption at rest:</span></span>

* <span data-ttu-id="92fd6-162">Depolanan veriler otomatik olarak şifreler.</span><span class="sxs-lookup"><span data-stu-id="92fd6-162">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="92fd6-163">Yapılandırma, programlama veya diğer iş verilere erişen bir yazılım içermeyen şifreler.</span><span class="sxs-lookup"><span data-stu-id="92fd6-163">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="92fd6-164">Kolay ve güvenli bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-164">Is the easiest and safest option.</span></span>
* <span data-ttu-id="92fd6-165">Veritabanı, anahtarları ve şifreleme yönetmenizi sağlar.</span><span class="sxs-lookup"><span data-stu-id="92fd6-165">Allows the database to manage keys and encryption.</span></span>

<span data-ttu-id="92fd6-166">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92fd6-166">For example:</span></span>

* <span data-ttu-id="92fd6-167">Microsoft SQL ve Azure SQL [saydam veri şifrelemesi](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span><span class="sxs-lookup"><span data-stu-id="92fd6-167">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="92fd6-168">SQL Azure veritabanının varsayılan olarak şifreler.</span><span class="sxs-lookup"><span data-stu-id="92fd6-168">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="92fd6-169">[Azure Blobları, dosyalar, tablo ve kuyruk depolama varsayılan olarak şifrelenmiş](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span><span class="sxs-lookup"><span data-stu-id="92fd6-169">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="92fd6-170">Bekleyen yerleşik şifreleme sağlamayan veritabanları için aynı koruma sağlamak için disk şifreleme kullanmanız mümkün olabilir.</span><span class="sxs-lookup"><span data-stu-id="92fd6-170">For databases that don't provide built-in encryption at rest, you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="92fd6-171">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="92fd6-171">For example:</span></span>

* [<span data-ttu-id="92fd6-172">Windows Server için BitLocker'ı</span><span class="sxs-lookup"><span data-stu-id="92fd6-172">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="92fd6-173">Linux:</span><span class="sxs-lookup"><span data-stu-id="92fd6-173">Linux:</span></span>
  * [<span data-ttu-id="92fd6-174">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="92fd6-174">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="92fd6-175">[EncFS](https://github.com/vgough/encfs).</span><span class="sxs-lookup"><span data-stu-id="92fd6-175">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="92fd6-176">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="92fd6-176">Additional resources</span></span>

* [<span data-ttu-id="92fd6-177">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="92fd6-177">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/trustcenter/Privacy/GDPR)
