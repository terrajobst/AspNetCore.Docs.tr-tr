---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: ASP.NET Web sayfaları (Razor) sorun giderme kılavuzu | Microsoft Docs
author: Rick-Anderson
description: Bu makalede, ASP.NET Web sayfaları (Razor) ve bazı önerilen çözümleri ile çalışırken karşılaşabileceğiniz sorunlar açıklanmaktadır. Yazılım sürümleri ASP.NET Web fası...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: ec8cdda5c5b298736a650f82cd6b52d73b6dfe3d
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021202"
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a>ASP.NET Web Sayfaları (Razor) Sorun Giderme Kılavuzu
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> Bu makalede, ASP.NET Web sayfaları (Razor) ve bazı önerilen çözümleri ile çalışırken karşılaşabileceğiniz sorunlar açıklanmaktadır.
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web sayfaları 1.0 ve ASP.NET Web Pages 2 ile de çalışır.


Bu konu aşağıdaki bölümleri içermektedir:

- [Sayfaları çalıştıran sorunları](#Issues_Running_.cshtml_Pages)
- [Razor kod ile ilgili sorunlar](#IssuesWithRazorCode)
- [Güvenlik ve üyelik ile ilgili sorunlar](#membership)
- [E-posta gönderme sorunları](#email)
- [Ek Kaynaklar](#AdditionalResources)

Genel sorular için bkz. [ASP.NET Web sayfaları (Razor) SSS](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Sayfaları çalıştıran sorunları

Çeşitli sorunlar engelleyebilir *.cshtml* ve *.vbhtml* düzgün çalışmasını sayfaları. Bu bölümde, genel hata iletileri listeler ve büyük olasılıkla neden olur.

### <a name="http-error-403---forbidden-access-is-denied"></a>HTTP Hata 403 - Yasak: Erişim engellendi

*Bu dizini veya sayfayı sağladığınız kimlik bilgileriyle görüntüleme izniniz yok.*

Sunucu .NET Framework sürümünü doğru çalışmıyorsa, bu hata oluşabilir. Sunucu (yerel olarak veya uzaktan) çalıştıran bilgisayarın en az .NET Framework 4 yüklü olduğundan emin olun. Ayrıca uygulamanın kendisinin doğru sürümünü çalıştırmak üzere yapılandırılmış olduğundan emin olun.

Bu sorunu yerel olarak Webmatrix'te çalışırken görürseniz, tıklayın **Site** çalışma alanında, ağaç görünümünde tıklayın, sonra da **ayarları**. İçinde **.NET Framework sürümünü seçin** listesinde, seçin **.NET 4 (tümleşik)**. Bu sürümü zaten ayarlanmışsa, WebMatrix yönetici olarak çalıştırın.

Web sitenizin kök en az bir sahip olduğundan emin olun *.cshtml* içindeki dosya.

Web sunucusu uzak bir sunucuda olduğunda bu hatayı görürseniz, sunucu yöneticisine başvurun. Veya üzerinin yüklü olduğundan emin sunucuda .NET Framework 4 olduğundan emin olun. Ayrıca uygulamayı,.NET Framework sürümünü kullanacak şekilde yapılandırılmış bir uygulama havuzunda çalıştığından emin olun.

Sunucu üzerinde denetime sahip olursunuz, .NET Framework sürümünü doğru çalıştığından emin olun. Çalıştırarak onarmayı deneyebilirsiniz `aspnet_regiis -iru` komutu. (IIS, .NET Framework'ü yükledikten sonra yüklerseniz, örneğin, IIS doğru ASP.NET sayfaları çalıştırmak için yapılandırılmaz.) Daha fazla bilgi için [ASP.NET IIS Kayıt Aracı (Aspnet\_regiis.exe)](https://msdn.microsoft.com/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>HTTP Hatası 403.14 - Yasak

*Web sunucusu bu dizinin içeriklerini değil listelemek için yapılandırılır.*

Korumalı bir kaynağın istemesi durumunda bu hata oluşabilir (gibi *Web.config* dosyası) veya korunan bir klasöre olan (gibi *uygulama\_veri* veya *uygulama\_Kod*).

### <a name="http-error-40417---not-found"></a>HTTP Hatası 404,17 - bulunamadı

*Talep edilen içeriği, betik gibi görünüyor ve statik dosya işleyici tarafından sunulan değil.*

Sunucu .NET Framework 4 kullanmak için düzgün yapılandırılmamış veya daha sonra ve bu nedenle kodda tanımıyor bu hata oluşabilir `@{ }` engeller. Bir açıklaması için önceki bkz *HTTP Hata 403 - Yasak: erişim engellendi*.

### <a name="http-error-4047---not-found"></a>HTTP Hatası 404.7 - bulunamadı

*İstek Filtreleme modülü dosya uzantısını Reddet yapılandırılır*

Bu hata oluşabilir *.cshtml* veya *.vbhtml* uzantıları sunucuda açıkça da engellenmiş. Bu sorunun belirtisi uzantısı, ancak dahil URL'leri dahil etmeyin URL'leri çalışan olduğunda *.cshtml* veya *.vbhtml* çalışmaz. Site uzantıları yeniden etkinleştirmek için olası bir çözüm olan *Web.config* dosya. Aşağıdaki örnek nasıl etkinleştirileceğini gösterir *.cshtml* uzantısı.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>HTTP Hatası 404.8 - bulunamadı

*İstek Filtreleme modülü hiddenSegment bölüm içeren URL yolunda reddetmek üzere yapılandırılır.*

Korumalı bir kaynağın istemesi durumunda bu hata oluşabilir (gibi *Web.config* dosyası) veya korunan bir klasöre olan (gibi *uygulama\_veri* veya *uygulama\_Kod*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Bu sayfa türü ('/' uygulamasında sunucu hatası) sunulmuyor

HTTP Hatası 404,17 için önceki açıklamasına bakın.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Razor kod ile ilgili sorunlar

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Adı '*sınıfı*' geçerli bağlamda yok

Genellikle, bu hatayı görmek için bir neden olan `class` başvuruları yardımcı, ancak Yardımcısı yüklü değil. Örneğin, bir yardımcı kullanmayı denerseniz, ancak paket Nuget'ten yüklemediyseniz, bu hatayı görürsünüz. WebMatrix galeride bulmak ve Yardımcısı'nı yüklemek için kullanın.

Yardımcısı yüklü, ancak sayfa hala tanıyamadığı varsa deneyin ekleme bir `using` deyimi için kod. İçinde `using` deyimi, yardımcı içeren ad alanı başvurusu. Örneğin, ASP.NET Web Yardımcıları pakette olan temel olarak yardımcılardır `System.Web.Helpers` ad alanı. Yardımcı kullanmak istediğiniz sayfanın üst kısmında, aşağıdaki satırı ekleyin:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Güvenlik ve üyelik ile ilgili sorunlar

ASP.NET Web Pages'da (Razor) yerleşik güvenlik (Üyelik) sistemi kullanıyorsanız, aşağıdaki sorunlarla karşılaşabilirsiniz.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Bu yöntem çağırmak için "Membership.Provider" özelliği "ExtendedMembershipProvider" örneği olmalıdır

Bu hata bildiren hiçbir `AspNetSqlMembershipProvider` sınıfı yapılandırılır. (Bir sitenin düzgün yerel olarak çalışır ancak bir barındırma sağlayıcısının sunucuya yayımladığınızda, bu hata oluşturuyor belirtisidir.) Bu sorun için bir düzeltme olan sitenin aşağıdakileri ekleyerek basit üyelik açıkça etkinleştirmek için *Web.config* dosyası:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>E-posta gönderme sorunları

Hata ayıklamak için e-posta gönderme sorunlarını zor olabilir. SMTP sunucusuna bağlanamıyorsunuz ilk bir sorun olabilir. Bağlantı başarılı olursa, ASP.NET ileti kapalı SMTP sunucusuna uygulamalı. Ancak, SMTP sunucusu, göndermesini engeller ileti kendisi ile ilgili sorunlar olabilir.

Uygulamanız başarıyla e-posta göndermez, aşağıdakileri deneyin:

- SMTP sunucusu adını genellikle gibi bir şeydir `smtp.provider.com` veya `smtp.provider.net`. Ancak, bir barındırma sağlayıcısına sitenizi yayımlayın, SMTP sunucusu adını bu noktada olabilir `localhost`. Yayımladığınız sonra sitenizi sağlayıcıya ait sunucu üzerinde çalıştığından, SMTP sunucusu yerel uygulamanızı perspektifinden olabileceğinden, bu durum oluşur. Sunucu adları bu değişikliğe, yayımlama işleminin bir parçası SMTP sunucusu adını değiştirmek zorunda anlamına gelebilir.
- Bağlantı noktası numarası genellikle 25'tir. Ancak, bazı sağlayıcılar 587 veya bazı başka bir bağlantı, bağlantı noktası kullanmayı gerektirir. SMTP sunucusuna sahip, hangi bağlantı noktası numarasını kullanmak için beklediği denetleyin.
- Doğru kimlik bilgilerini kullandığınızdan emin olun. Bir barındırma sağlayıcısına sitenizi yayımladığınız sağlayıcısı, e-posta için özellikle belirtti kimlik bilgilerini kullanın. Bu kimlik bilgileri yayımlamak için kullandığınız kimlik bilgilerinizden farklı olabilir.
- Bazı durumlarda tüm kimlik bilgisi gerekmez. Kişisel ISS kullanarak e-posta gönderiyorsanız, e-posta sağlayıcınız kimlik bilgilerinizi zaten biliyor olabilirsiniz. Yayımladıktan sonra yerel bilgisayarınızda test ettiğinizde, farklı kimlik bilgileri kullanmanız gerekebilir.
- E-posta sağlayıcınız şifreleme kullanıyorsa, ayarlama `WebMail.EnableSsl` için `true`.

E-posta gönderilirken bir hata varsa, şuna benzer bir standart ASP.NET hata iletisini görebilirsiniz:

![E-posta ile ilgili bir sorun olduğunda ASP.NET hata iletisi](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

Kullanarak e-posta gönderme sorunlarını da ayıklayabilirsiniz bir `try-catch` aşağıdaki örnekteki gibi bir blok. Kullandığınızda, bir `try-catch` blok, ASP.NET, standart hata iletileri görüntülenmez. Bunun yerine, hatayı yakalayabilirsiniz `catch` blok kısmı.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

İçin uygun değerleri yerine `your-SMTP-server-name`ve benzeri. Bu şekilde görebileceğiniz hata iletileri bazıları şunlardır:

- *Posta gönderme başarısız oldu.*

    veya

    *Bağlı olan taraf doğru zaman ya da kurulan bağlantı bağlı konak yanıt başarısız olduğundan başarısız oldu. bir süre sonra yanıt vermediğinden bağlantı denemesi başarısız oldu.*

    Bu hata genellikle uygulama SMTP sunucusuna bağlanamadı anlamına gelir. Sunucu adını denetleyin ve bağlantı noktası numarası.
- <em>Posta kutusu kullanılamaz. Sunucu yanıtı: 5.1.0 &lt; someuser@invaliddomain &gt; gönderen reddetti: Geçersiz gönderen etki alanı</em>

    Bu iletiyi bildiren `From` adresi doğru değil veya eksik.
- *Belirtilen dizenin bir e-posta adresi için gereken biçimde değil.*

    Bu hata olabileceğini gösteren değeri `To` veya `From` özellikleri e-posta adresleri olarak tanınmıyor. (ASP.NET, e-posta adresi doğru biçimde gibi kullanıcının yalnızca geçerli olduğunu denetleyemez *name@domain.com*.)

> [!NOTE]
> Hatayı görüntüler Biçimlendirmeyi Kaldır (`@errorMessage`) Canlı site için sayfayı yayımlamadan önce. Kullanıcıların bir sunucudan alın hata iletilerini görmek için iyi bir fikir değil.


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET Web Sayfaları (Razor) SSS](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix ve ASP.NET Web sayfaları](https://forums.asp.net/1224.aspx/1?WebMatrix) ASP.NET Web sitesinde Forumu
