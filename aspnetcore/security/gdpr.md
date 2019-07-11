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
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>ASP.NET Core AB genel veri koruma yönetmeliği (GDPR) desteği

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core API'leri ve şablonları bazı karşılamanıza yardımcı olmak üzere sağlar [AB genel veri koruma yönetmeliği (GDPR)](https://www.eugdpr.org/) gereksinimleri:

::: moniker range=">= aspnetcore-3.0"

* Proje şablonları, uzantı noktaları ve gizlilik ve tanımlama bilgisi kullan ilke ile değiştirebilirsiniz saptama biçimlendirme içerir.
* *Pages/Privacy.cshtml* sayfası veya *Views/Home/Privacy.cshtml* ayrıntı sitenizin gizlilik ilkesi için bir sayfa görünümü sağlar.

Gibi varsayılan tanımlama bilgisi onayı özelliği etkinleştirmek için bir ASP.NET Core 3.0 Şablonu oluşturulan uygulamada ASP.NET çekirdek 2.2 şablonları bulunamadı:

* Ekleme [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) çok `Startup.ConfigureServices` ve [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) için `Startup.Configure`:

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* Ekleme için tanımlama bilgisi onayı kısmi *_Layout.cshtml* dosyası:

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* Ekleme  *\_CookieConsentPartial.cshtml* projeye dosya:

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* Bu makalede, tanımlama bilgisi onayı özelliği hakkında okumak için ASP.NET Core 2.2 sürümünü seçin.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* Proje şablonları, uzantı noktaları ve gizlilik ve tanımlama bilgisi kullan ilke ile değiştirebilirsiniz saptama biçimlendirme içerir.
* Bir tanımlama bilgisi onay özelliği, kişisel bilgileri depolamak için onay isteyin (ve izlemek), kullanıcılarınızdan gelen sağlar. Veri toplama için bir kullanıcı tarafından onaylanan taşınmadığından ve uygulamasına sahipse [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) kümesine `true`, gerekli olmayan tanımlama bilgileri, tarayıcıya gönderilen değildir.
* Tanımlama bilgileri, önemli olarak işaretlenebilir. Temel tanımlama, kullanıcı onaylı olmayan ve izleme devre dışı bile tarayıcıya gönderilir.
* [TempData ve oturum tanımlama bilgilerini](#tempdata) izleme devre dışı bırakıldığında işlevsel değildir.
* [Kimlik Yönetimi](#pd) sayfası, indirme ve kullanıcı verilerini silmek için bir bağlantı sağlar.

[Örnek uygulaması](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) GDPR uzantı noktaları çoğu test ve API'leri eklenen ASP.NET Core 2.1 şablonlara izin verir. Bkz: [Benioku](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) yönergeleri test dosyası.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>ASP.NET Core GDPR şablon tarafından oluşturulan kodda desteği

Razor sayfaları ve MVC proje şablonları ile oluşturulan projeleri aşağıdaki GDPR desteği içerir:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) ve [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) ayarlanan `Startup` sınıfı.
* *\_CookieConsentPartial.cshtml* [kısmi Görünüm](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper). Bir **kabul** düğmesi bu dosyasına eklenir. Kullanıcı tıkladığında **kabul** düğmesi, tanımlama bilgilerini depolamak için onay sağlanır.
* *Pages/Privacy.cshtml* sayfası veya *Views/Home/Privacy.cshtml* ayrıntı sitenizin gizlilik ilkesi için bir sayfa görünümü sağlar. *\_CookieConsentPartial.cshtml* dosyası gizlilik sayfasına bir bağlantı oluşturur.
* Bireysel kullanıcı hesapları ile oluşturulan uygulamalar için indirme ve silme için bağlantıları Yönet sayfasında sağlar [kişisel kullanıcı verileri](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions ve UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) içinde başlatılan `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) çağrılma yeri `Startup.Configure`:

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>\_CookieConsentPartial.cshtml kısmi Görünüm

*\_CookieConsentPartial.cshtml* kısmi görünümü:

[!code-html[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

Bu kısmi:

* Kullanıcı için izleme durumunu alır. Uygulama izni istemek için yapılandırılmışsa, tanımlama bilgileri izlenebilir önce kullanıcının onaylaması gerekir. Onayı gerekiyorsa, tanımlama bilgisi onayı paneli tarafından oluşturulan gezinti çubuğunun üst sabitlenmiştir  *\_Layout.cshtml* dosya.
* Bir HTML sağlar `<p>` gizlilik ve tanımlama bilgisi özetlemek için öğesi İlkesi kullanın.
* Gizlilik sayfasına veya, sitenizin gizlilik ilkesi olduğu ayrıntılı görünüm için bir bağlantı sağlar.

## <a name="essential-cookies"></a>Temel tanımlama bilgileri

Varsa onay tanımlama bilgilerini depolamak için sağlanmış taşınmadığından, yalnızca önemli olarak işaretlenmiş tanımlama bilgilerini tarayıcıya gönderilir. Aşağıdaki kod, bir tanımlama bilgisi gerekli hale getirir:

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a>TempData sağlayıcısı ve oturum durumu tanımlama bilgileri gerekli değil

[TempData sağlayıcısı](xref:fundamentals/app-state#tempdata) tanımlama bilgisi gerekli değildir. İzleme devre dışı bırakılırsa TempData sağlayıcısı işlevsel değildir. İzleme devre dışı bırakıldığında TempData sağlayıcıyı etkinleştirmek için TempData tanımlama bilgisi, gerekli olarak işaretlemek `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

[Oturum durumu](xref:fundamentals/app-state) tanımlama bilgisi gerekli değildir. İzleme devre dışı bırakıldığında, oturum durumu işlevsel değildir. Aşağıdaki kod, oturum tanımlama bilgileri önemli hale getirir:

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a>Kişisel veriler

Oluşturulan kullanıcı hesaplarıyla ASP.NET Core uygulamaları indirmek ve kişisel verileri silmek için kod ekleyin.

Kullanıcı adını seçin ve ardından **kişisel verileri**:

![Sayfa kişisel verileri yönetme](gdpr/_static/pd.png)

Notlar:

* Oluşturulacak `Account/Manage` kod bkz [İskele kimlik](xref:security/authentication/scaffold-identity).
* **Sil** ve **indirme** bağlantıları, yalnızca varsayılan kimlik verileri davranacak. Özel kullanıcı verilerini oluşturduğunuz uygulamalar, özel kullanıcı verilerini silme/indirilecek genişletilmelidir. Daha fazla bilgi için [Ekle, indirme ve silme özel kullanıcı verilerini kimliğe](xref:security/authentication/add-user-data).
* Kimlik veritabanı tablosunda depolanan kullanıcı için belirteç kaydedildi `AspNetUserTokens` basamaklı silme davranışı nedeniyle aracılığıyla kullanıcı silindiğinde silinir [yabancı anahtar](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).
* [Dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index), Facebook ve Google değil gibi kullanılabilir tanımlama bilgisi ilkesi kabul edilmeden önce.

::: moniker-end

## <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Bekleme sırasında şifreleme için bazı veritabanları ve depolama mekanizmaları sağlar. Bekleme sırasında şifreleme:

* Depolanan veriler otomatik olarak şifreler.
* Yapılandırma, programlama veya diğer iş verilere erişen bir yazılım içermeyen şifreler.
* Kolay ve güvenli bir seçenektir.
* Veritabanı, anahtarları ve şifreleme yönetmenizi sağlar.

Örneğin:

* Microsoft SQL ve Azure SQL [saydam veri şifrelemesi](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).
* [SQL Azure veritabanının varsayılan olarak şifreler.](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Azure Blobları, dosyalar, tablo ve kuyruk depolama varsayılan olarak şifrelenmiş](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Bekleyen yerleşik şifreleme sağlamayan veritabanları için aynı koruma sağlamak için disk şifreleme kullanmanız mümkün olabilir. Örneğin:

* [Windows Server için BitLocker'ı](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Ek kaynaklar

* [Microsoft.com/GDPR](https://www.microsoft.com/trustcenter/Privacy/GDPR)
