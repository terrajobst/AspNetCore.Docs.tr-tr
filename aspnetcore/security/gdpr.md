---
title: ASP.NET Core genel veri koruma düzenleme (GDPR) desteği
author: rick-anderson
description: Bir ASP.NET Core web uygulamasında GDPR uzantı noktaları erişim öğrenin.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/gdpr
ms.openlocfilehash: c3c8a3fcd4a303aea65c57ff6be2ff0434383f33
ms.sourcegitcommit: 7e87671fea9a5f36ca516616fe3b40b537f428d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/12/2018
ms.locfileid: "35341931"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>ASP.NET Core AB genel veri koruma düzenleme (GDPR) desteği

tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core API ve şablonları bazı karşılamak amacıyla sağlar [AB genel veri koruma düzenleme (GDPR)](https://www.eugdpr.org/) gereksinimleri:

* Proje şablonları uzantı noktaları ve gizlilik ve tanımlama bilgisi kullanım ilkesi ile değiştirebilirsiniz tamamlanmamış biçimlendirme içerir.
* Bir tanımlama bilgisi onayı özelliği onay için sormak (ve izlemek), kullanıcıların kişisel bilgilerini depolamak için sağlar. Bir kullanıcı için veri toplama seçtiği değil ve uygulama ile ayarlanırsa [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) için `true`, gerekli olmayan tanımlama bilgilerini tarayıcıya gönderilmeyecek.
* Tanımlama bilgileri gibi önemli olarak işaretlenebilir. Temel tanımlama bilgileri kullanıcının değil seçtiği ve izleme devre dışı bile tarayıcıya gönderilir.
* [TempData ve oturum tanımlama bilgileri](#tempdata) izleme devre dışı bırakıldığında işlevsel değildir.
* [Kimlik](#pd) sayfası, indirin ve kullanıcı verilerini silmek için bir bağlantı sağlar.

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) GDPR uzantı noktaları ve ASP.NET Core 2.1 şablonları eklenen API'leri çoğunu test sağlar. Bkz: [Benioku](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) yönergeleri sınama dosyası.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>Oluşturulan şablon kodunun ASP.NET Core GDPR desteği

Razor sayfalarının ve MVC proje şablonları ile oluşturulan projeleri aşağıdaki GDPR desteği içerir:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) ve [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) ayarlanır `Startup`.
* *_CookieConsentPartial.cshtml* [kısmi Görünüm](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).
* *Pages/Privacy.cshtml* veya *Home/Privacy.cshtml* görünümü, bir sayfa, sitenin gizlilik ilkesi ayrıntı sağlar. *_CookieConsentPartial.cshtml* dosyası gizlilik sayfasına bir bağlantı oluşturur.
* Bireysel kullanıcı hesapları ile oluşturulan uygulamalar için Yönet sayfasını indirin ve silmek için bağlantılar sağlar. [kişisel kullanıcı verilerini](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions ve UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) içinde başlatılan `Startup` sınıfı `ConfigureServices` yöntemi:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) olarak adlandırılır `Startup` sınıfı `Configure` yöntemi:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=49)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>_CookieConsentPartial.cshtml kısmi görünümü

*_CookieConsentPartial.cshtml* kısmi görünümü:

[!code-html[Main](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

Bu kısmi:

* Kullanıcı için izleme durumunu alır. Uygulama onayı gerektirecek şekilde yapılandırılmışsa, tanımlama bilgilerini izlenebilir önce kullanıcının onaylaması gerekir. Onay gerekiyorsa, tanımlama bilgisi onayı chrome oluşturulan gezinti çubuğu üstünde sabittir *Pages/Shared/_Layout.cshtml* dosya.
* Bir HTML sağlar `<p>` gizlilik ve tanımlama bilgisi özetlemek için öğesi İlkesi kullanın.
* Bir bağlantı sağlar *Pages/Privacy.cshtml* Burada, ayrıntı, sitenin gizlilik ilkesi.

## <a name="essential-cookies"></a>Temel tanımlama bilgileri

İzin verilen değil, yalnızca önemli olarak işaretlenmiş tanımlama bilgilerini tarayıcıya gönderilir. Aşağıdaki kod bir tanımlama bilgisi gerekli hale getirir:

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a>Tempdata sağlayıcısı ve oturum durumu tanımlama bilgisi gerekli değildir

[Tempdata sağlayıcı](xref:fundamentals/app-state#tempdata) tanımlama bilgisi gerekli değildir. İzleme devre dışıysa, Tempdata sağlayıcı işlevsel değildir. İzleme devre dışı bırakıldığında Tempdata Sağlayıcısı'nı etkinleştirmek için TempData tanımlama bilgisi, temel olarak işaretlemek `ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[Oturum durumu](xref:fundamentals/app-state) tanımlama bilgisi gerekli değildir. İzleme devre dışı bırakıldığında oturum durumu işlevsel değildir.

<a name="pd"></a>

## <a name="personal-data"></a>Kişisel veriler

Bireysel kullanıcı hesapları ile oluşturulan ASP.NET Core uygulamaları indirmek ve kişisel verilerini silmek için kodu ekleyin.

Kullanıcı adını seçin ve ardından **kişisel veriler**:

![Kişisel veri sayfası yönetme](gdpr/_static/pd.png)

Notlar:

* Oluşturulacak `Account/Manage` kod, bkz: [İskele kimlik](xref:security/authentication/scaffold-identity).
* Sil ve yalnızca etkisi varsayılan kimlik verilerini indirin. Uygulamaları oluşturma özel kullanıcı verilerini silme/özel kullanıcı verilerini indirme genişletilmelidir. GitHub sorunu [nasıl kimlik özel kullanıcı verilerini ekleme/silme](https://github.com/aspnet/Docs/issues/6226) özel/silme/indirme özel kullanıcı verilerini oluşturma önerilen bir makale izler. Bu konuda öncelik görmek istediğiniz bir başparmak tepki yukarı sorunu bırakın.
* Kullanıcı için kimlik veritabanı tablosunda depolanır belirteç kaydedilmiş `AspNetUserTokens` kullanıcı art arda silme davranışı nedeniyle aracılığıyla silindiğinde, silinen [yabancı anahtar](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).

## <a name="encryption-at-rest"></a>Bekleyen şifreleme

Bazı veritabanları ve depolama mekanizmaları bekleyen şifreleme izin verir. Bekleyen şifreleme:

* Depolanan veriler otomatik olarak şifreler.
* Yapılandırma, programlama veya diğer iş verilere erişen yazılım olmadan şifreler.
* Kolay ve güvenli bir seçenektir.
* Anahtarları ve şifrelemeyi yönetme veritabanı sağlar.

Örneğin:

* Microsoft SQL ve Azure SQL sağlamak [saydam veri şifreleme](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).
* [SQL Azure veritabanı varsayılan olarak şifreler.](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Azure BLOB'ları, dosyaları, tablo ve kuyruk depolama varsayılan olarak şifrelenir](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Bekleyen yerleşik şifreleme sağlamıyorsa veritabanları için aynı koruma sağlamak üzere disk şifrelemesi kullanmak mümkün olabilir. Örneğin:

* [Windows Server için BitLocker'ı](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Ek Kaynaklar

* [Microsoft.com/GDPR](https://www.microsoft.com/trustcenter/Privacy/GDPR)
