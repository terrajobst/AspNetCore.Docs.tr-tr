---
title: ASP.NET Core Genel Veri Koruma Yönetmeliği (GDPR) desteği
author: rick-anderson
description: ASP.NET Core Web uygulamasındaki GDPR uzantı noktalarına nasıl erişebileceğinizi öğrenin.
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: security/gdpr
ms.openlocfilehash: 2ccba780ba81bd805d08c9b898617387a879bed3
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660547"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>ASP.NET Core içinde AB Genel Veri Koruma Yönetmeliği (GDPR) desteği

Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core, bazı [ab genel veri koruma yönetmeliği (GDPR)](https://www.eugdpr.org/) gereksinimlerini karşılamaya yardımcı olmak Için API 'ler ve şablonlar sağlar:

::: moniker range=">= aspnetcore-3.0"

* Proje şablonları, gizlilik ve tanımlama bilgisi kullanım ilkenize göre değiştireceğiniz uzantı noktalarını ve saplaması işaretlemesini içerir.
* *Sayfalar/gizlilik. cshtml* sayfası veya *Görünümler/Home/privacy. cshtml* görünümü sitenizin gizlilik ilkesini ayrıntılandırmakta olan bir sayfa sağlar.

ASP.NET Core 3,0 şablonu tarafından oluşturulan bir uygulamada ASP.NET Core 2,2 şablonlarında bulunan gibi varsayılan tanımlama bilgisi onayı özelliğini etkinleştirmek için:

* Using yönergeleri listesine `using Microsoft.AspNetCore.Http` ekleyin.
* `Startup.Configure`için `Startup.ConfigureServices` ve [Usetarif ıepolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) 'e Ilişkin [ıepolicyoptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) ekleyin:

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* Tanımlama bilgisi onayını kısmen *_Layout. cshtml* dosyasına ekleyin:

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* Projeye *\_Sentıeconsentpartial. cshtml* dosyasını ekleyin:

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* Tanımlama bilgisi onayı özelliği hakkında bilgi edinmek için bu makalenin ASP.NET Core 2,2 sürümünü seçin.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* Proje şablonları, gizlilik ve tanımlama bilgisi kullanım ilkenize göre değiştireceğiniz uzantı noktalarını ve saplaması işaretlemesini içerir.
* Bir tanımlama bilgisi onayı özelliği, kişisel bilgileri depolamak için kullanıcılarınıza izin vermenizi (ve izlemenizi) sağlar. Bir kullanıcı veri toplamaya ayrılmamışsa ve uygulamanın [Checkconsentwith](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) `true`olarak ayarlandıysa, temel olmayan tanımlama bilgileri tarayıcıya gönderilmez.
* Tanımlama bilgileri, temel olarak işaretlenebilir. Kullanıcı onaylı değil ve izleme devre dışı bırakılmış olsa bile, temel tanımlama bilgileri tarayıcıya gönderilir.
* İzleme devre dışı bırakıldığında [TempData ve oturum tanımlama bilgileri](#tempdata) işlevsel değildir.
* [Kimlik yönetimi](#pd) sayfası, Kullanıcı verilerini indirmek ve silmek için bir bağlantı sağlar.

[Örnek uygulama](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) , ASP.NET Core 2,1 ŞABLONLARıNA eklenen GDPR uzantı noktalarının ve API 'lerin çoğunu test etmenize olanak tanır. Test yönergeleri için [Benioku](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) dosyasına bakın.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>Şablon tarafından üretilen kodda ASP.NET Core GDPR desteği

Proje Şablonlarıyla oluşturulan Razor Pages ve MVC projeleri aşağıdaki GDPR desteğini içerir:

* , `Startup` sınıfında, [tanımlama, ıepolicyoptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) ve [Useınte ıepolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) ayarlanır.
* *\_, ıeconsentpartial. cshtml* [kısmi görünümü](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper). Bu dosyaya bir **kabul** düğmesi dahildir. Kullanıcı **kabul et** düğmesine tıkladığında, tanımlama bilgilerini depolamak için onay sağlanır.
* *Sayfalar/gizlilik. cshtml* sayfası veya *Görünümler/Home/privacy. cshtml* görünümü sitenizin gizlilik ilkesini ayrıntılandırmakta olan bir sayfa sağlar. *\_, ıeconsentpartial. cshtml* dosyası, gizlilik sayfasına bir bağlantı oluşturur.
* Bireysel kullanıcı hesaplarıyla oluşturulan uygulamalarda, Yönet sayfası [kişisel kullanıcı verilerini](#pd)indirmek ve silmek için bağlantılar sağlar.

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>Tanımlama, ıepolicyoptions ve Usepişiriepolicy

[Pişirme ıepolicyoptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) `Startup.ConfigureServices`olarak başlatılır:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[Usetarif ıepolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) `Startup.Configure`içinde çağrılır:

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="_cookieconsentpartialcshtml-partial-view"></a>\_tanımlama, ıeconsentpartial. cshtml kısmi görünümü

*\_, ıeconsentpartial. cshtml* kısmi görünümü:

[!code-html[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

Bu kısmi:

* Kullanıcı için izleme durumunu alır. Uygulama onay gerektirecek şekilde yapılandırıldıysa, tanımlama bilgilerinin izlenmemesi için kullanıcının onayı gerekir. İzin gerekliyse, tanımlama bilgisi onay paneli *\_Layout. cshtml* dosyası tarafından oluşturulan gezinti çubuğunun üstünde düzeltilir.
* Gizlilik ve tanımlama bilgisi kullanım ilkenizi özetlemek için bir HTML `<p>` öğesi sağlar.
* Gizlilik sayfasına bir bağlantı sağlar veya sitenizin gizlilik ilkesini ayrıntılandırınızın ne olduğunu görebilirsiniz.

## <a name="essential-cookies"></a>Temel tanımlama bilgileri

Tanımlama bilgilerini depolamak için izin sağlanmazsa, tarayıcıya yalnızca temel olarak işaretlenen tanımlama bilgileri gönderilir. Aşağıdaki kod, bir tanımlama bilgisini temel yapar:

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a>TempData sağlayıcısı ve oturum durumu tanımlama bilgileri gerekli değildir

[TempData sağlayıcısı](xref:fundamentals/app-state#tempdata) tanımlama bilgisi gerekli değildir. İzleme devre dışıysa, TempData sağlayıcısı işlevsel değildir. İzleme devre dışı bırakıldığında TempData sağlayıcısını etkinleştirmek için, `Startup.ConfigureServices`bölümünde gerekli olan TempData tanımlama bilgisini işaretleyin:

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

[Oturum durumu](xref:fundamentals/app-state) tanımlama bilgileri gerekli değildir. İzleme devre dışı olduğunda oturum durumu işlevsel değil. Aşağıdaki kod, oturum tanımlama bilgilerini temel yapar:

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a>Kişisel veriler

Bireysel kullanıcı hesaplarıyla oluşturulan ASP.NET Core uygulamalar, kişisel verileri indirmek ve silmek için kod içerir.

Kullanıcı adını seçin ve **kişisel veriler**' i seçin:

![Kişisel verileri yönetme sayfası](gdpr/_static/pd.png)

Notlar:

* `Account/Manage` kodu oluşturmak için bkz. [Yapı Iskelesi kimliği](xref:security/authentication/scaffold-identity).
* **Silme** ve **indirme** bağlantıları yalnızca varsayılan kimlik verilerinde işlem görür. Özel Kullanıcı verileri oluşturan uygulamalar, özel kullanıcı verilerini silmek/indirmek için genişletilmelidir. Daha fazla bilgi için bkz. [Özel Kullanıcı verilerini kimlik Ile ekleme, indirme ve silme](xref:security/authentication/add-user-data).
* Kimlik veritabanı tablosu `AspNetUserTokens` depolanan kullanıcı için kaydedilen belirteçler, Kullanıcı [yabancı anahtar](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152)nedeniyle basamaklı silme davranışı aracılığıyla silindiğinde silinir.
* Facebook ve Google gibi [dış sağlayıcı kimlik doğrulaması](xref:security/authentication/social/index), tanımlama bilgisi İlkesi kabul edilmeden önce kullanılamaz.

::: moniker-end

## <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Bazı veritabanları ve depolama mekanizmaları, bekleyen şifreleme için izin verir. Bekleyen şifreleme:

* Depolanan verileri otomatik olarak şifreler.
* Veriye erişen yazılımlar için yapılandırma, programlama veya diğer çalışmasız şifreler.
* En kolay ve en güvenli seçenektir.
* Veritabanının anahtarları ve şifrelemeyi yönetmesine izin verir.

Örnek:

* Microsoft SQL ve Azure SQL, [Saydam veri şifrelemesi](/sql/relational-databases/security/encryption/transparent-data-encryption) (tde) sağlar.
* [SQL Azure, varsayılan olarak veritabanını şifreler](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Azure Blobları, dosyalar, tablo ve kuyruk depolaması varsayılan olarak şifrelenir](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Bekleyen şifreleme sağlamayan veritabanları için aynı korumayı sağlamak üzere disk şifrelemeyi kullanabilirsiniz. Örnek:

* [Windows Server için BitLocker](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [Eccryptotfs](https://launchpad.net/ecryptfs)
  * [Şifreleme](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Ek kaynaklar

* [Microsoft.com/GDPR](https://www.microsoft.com/trustcenter/Privacy/GDPR)
* [GDPR-ASP.NET Core Izin Iptali düğmesi ekleme](https://www.joeaudette.com/blog/2018/08/28/gdpr---adding-a-revoke-consent-button-in-aspnet-core)
