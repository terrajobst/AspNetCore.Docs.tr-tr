---
title: ASP.NET core'da genel veri koruma yönetmeliği (GDPR) desteği
author: rick-anderson
description: Bir ASP.NET Core web uygulaması GDPR uzantı noktaları erişmeyi öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/29/2018
uid: security/gdpr
ms.openlocfilehash: 7688ac6264b089ebd3544494c967684cb291a547
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207521"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>ASP.NET Core AB genel veri koruma yönetmeliği (GDPR) desteği

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core API'leri ve şablonları bazı karşılamanıza yardımcı olmak üzere sağlar [AB genel veri koruma yönetmeliği (GDPR)](https://www.eugdpr.org/) gereksinimleri:

* Proje şablonları, uzantı noktaları ve gizlilik ve tanımlama bilgisi kullan ilke ile değiştirebilirsiniz saptama biçimlendirme içerir.
* Bir tanımlama bilgisi onay özelliği, kişisel bilgileri depolamak için onay isteyin (ve izlemek), kullanıcılarınızdan gelen sağlar. Veri toplama için bir kullanıcı tarafından onaylanan taşınmadığından ve uygulamasına sahipse [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) kümesine `true`, gerekli olmayan tanımlama bilgileri, tarayıcıya gönderilen değildir.
* Tanımlama bilgileri, önemli olarak işaretlenebilir. Temel tanımlama, kullanıcı onaylı olmayan ve izleme devre dışı bile tarayıcıya gönderilir.
* [TempData ve oturum tanımlama bilgilerini](#tempdata) izleme devre dışı bırakıldığında işlevsel değildir.
* [Kimlik Yönetimi](#pd) sayfası, indirme ve kullanıcı verilerini silmek için bir bağlantı sağlar.

[Örnek uygulaması](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) GDPR uzantı noktaları çoğu test ve API'leri eklenen ASP.NET Core 2.1 şablonlara izin verir. Bkz: [Benioku](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) yönergeleri test dosyası.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>Oluşturulan şablon kodunun ASP.NET Core GDPR desteği

Razor sayfaları ve MVC proje şablonları ile oluşturulan projeleri aşağıdaki GDPR desteği içerir:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) ve [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) ayarlanan `Startup`.
* *_CookieConsentPartial.cshtml* [kısmi Görünüm](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).
* *Pages/Privacy.cshtml* sayfası veya *Views/Home/Privacy.cshtml* ayrıntı sitenizin gizlilik ilkesi için bir sayfa görünümü sağlar. *_CookieConsentPartial.cshtml* dosyası gizlilik sayfasına bir bağlantı oluşturur.
* Bireysel kullanıcı hesapları ile oluşturulan uygulamalar için indirme ve silme için bağlantıları Yönet sayfasında sağlar [kişisel kullanıcı verileri](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions ve UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) içinde başlatılan `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) çağrılma yeri `Startup.Configure`:

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>_CookieConsentPartial.cshtml kısmi Görünüm

*_CookieConsentPartial.cshtml* kısmi görünümü:

[!code-html[](gdpr/sample/RP/Pages/Shared/_CookieConsentPartial.cshtml)]

Bu kısmi:

* Kullanıcı için izleme durumunu alır. Uygulama izni istemek için yapılandırılmışsa, tanımlama bilgileri izlenebilir önce kullanıcının onaylaması gerekir. Onayı gerekiyorsa, tanımlama bilgisi onayı paneli tarafından oluşturulan gezinti çubuğunun üst sabitlenmiştir *_Layout.cshtml* dosya.
* Bir HTML sağlar `<p>` gizlilik ve tanımlama bilgisi özetlemek için öğesi İlkesi kullanın.
* Gizlilik sayfasına veya, sitenizin gizlilik ilkesi olduğu ayrıntılı görünüm için bir bağlantı sağlar.

## <a name="essential-cookies"></a>Temel tanımlama bilgileri

İzin verilen değil, yalnızca önemli olarak işaretlenmiş tanımlama bilgilerini tarayıcıya gönderilir. Aşağıdaki kod, bir tanımlama bilgisi gerekli hale getirir:

[!code-csharp[Main](gdpr/sample/RP/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

## <a name="tempdata-provider-and-session-state-cookies-are-not-essential"></a>Tempdata sağlayıcısı ve oturum durum tanımlama bilgisi gerekli değildir

[Tempdata sağlayıcısı](xref:fundamentals/app-state#tempdata) tanımlama bilgisi gerekli değildir. İzleme devre dışı bırakılırsa Tempdata sağlayıcısı işlevsel değildir. İzleme devre dışı bırakıldığında Tempdata sağlayıcıyı etkinleştirmek için TempData tanımlama bilgisi, gerekli olarak işaretlemek `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP/Startup.cs?name=snippet1)]

[Oturum durumu](xref:fundamentals/app-state) tanımlama bilgisi gerekli değildir. İzleme devre dışı bırakıldığında, oturum durumu işlevsel değildir.

<a name="pd"></a>

## <a name="personal-data"></a>Kişisel veriler

Oluşturulan kullanıcı hesaplarıyla ASP.NET Core uygulamaları indirmek ve kişisel verileri silmek için kod ekleyin.

Kullanıcı adını seçin ve ardından **kişisel verileri**:

![Sayfa kişisel verileri yönetme](gdpr/_static/pd.png)

Notlar:

* Oluşturulacak `Account/Manage` kod bkz [İskele kimlik](xref:security/authentication/scaffold-identity).
* **Sil** ve **indirme** bağlantıları, yalnızca varsayılan kimlik verileri davranacak. Özel kullanıcı verilerini oluşturduğunuz uygulamalar, özel kullanıcı verilerini silme/indirilecek genişletilmelidir. Daha fazla bilgi için [Ekle, indirme ve silme özel kullanıcı verilerini kimliğe](xref:security/authentication/add-user-data).
* Kimlik veritabanı tablosunda depolanan kullanıcı için belirteç kaydedildi `AspNetUserTokens` basamaklı silme davranışı nedeniyle aracılığıyla kullanıcı silindiğinde silinir [yabancı anahtar](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).

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
