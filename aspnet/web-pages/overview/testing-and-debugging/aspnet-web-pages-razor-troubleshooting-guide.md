---
uid: web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
title: "ASP.NET Web sayfaları (Razor) sorun giderme kılavuzu | Microsoft Docs"
author: tfitzmac
description: "Bu makalede, ASP.NET Web sayfaları (Razor) ve bazı önerilen çözümlerle çalışırken sahip olabileceği sorunlar anlatılmaktadır. Yazılım sürümleri ASP.NET Web sayfa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 2a2c1833-0bfe-4e2e-9cc0-341b52c7b121
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/aspnet-web-pages-razor-troubleshooting-guide
msc.type: authoredcontent
ms.openlocfilehash: e507d97903583c7233456e9139e1a869f8bf75bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-troubleshooting-guide"></a>ASP.NET Web sayfaları (Razor) sorun giderme kılavuzu
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> Bu makalede, ASP.NET Web sayfaları (Razor) ve bazı önerilen çözümlerle çalışırken sahip olabileceği sorunlar anlatılmaktadır.
> 
> ## <a name="software-versions"></a>Yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2 ve ASP.NET Web sayfaları 1.0 ile de çalışır.


Bu konu aşağıdaki bölümleri içermektedir:

- [Sayfaları çalıştıran sorunlar](#Issues_Running_.cshtml_Pages)
- [Razor kod sorunları](#IssuesWithRazorCode)
- [Güvenlik ve üyeliği ile ilgili sorunlar](#membership)
- [E-posta gönderme sorunları](#email)
- [Ek kaynaklar](#AdditionalResources)

Genel sorular için bkz: [ASP.NET Web sayfaları (Razor) SSS](https://go.microsoft.com/fwlink/?LinkId=253000).

<a id="Issues_Running_.cshtml_Pages"></a>
## <a name="issues-with-running-pages"></a>Sayfaları çalıştıran sorunlar

Çok çeşitli sorunlar engelleyebilirsiniz *.cshtml* ve *.vbhtml* düzgün çalışmasını sayfaları. Bu bölüm yaygın hata iletileri listelenmiştir ve büyük olasılıkla neden olur.

### <a name="http-error-403---forbidden-access-is-denied"></a>HTTP Hatası 403 - Yasak: Erişim engellendi

*Bu dizini veya sayfayı sağladığınız kimlik bilgileriyle görüntüleme izniniz yok.*

Sunucu .NET Framework'ün doğru sürümünü çalışmıyorsa bu hata oluşabilir. (Yerel ve uzaktan) server çalıştıran bilgisayarın en az .NET Framework 4 yüklü olduğundan emin olun. Ayrıca uygulamanın kendisinin doğru sürüme çalıştırmak için yapılandırıldığından emin olun.

Bu sorunu yerel olarak Webmatrix'te çalışırken görürseniz, tıklatın **Site** çalışma alanında ve ardından treeview tıklatın **ayarları**. İçinde **.NET Framework sürüm seçin** listesinde, seçin **.NET 4 (tümleşik)**. Bu sürüm zaten ayarladıysanız, WebMatrix yönetici olarak çalıştırmayı deneyin.

Web sitenizin kök en az bir tane olduğundan emin olun *.cshtml* içindeki dosya.

Web sunucusu uzak bir sunucuda olduğunda bu hatayı görürseniz, sunucu yöneticisine başvurun. Veya üstü yüklü sunucu .NET Framework 4 sahip olduğundan emin olun. Ayrıca uygulamayı,.NET Framework sürümünü kullanmak üzere yapılandırılmış bir uygulama havuzunda çalıştığından emin olun.

Sunucu üzerinde denetim varsa, .NET Framework doğru sürümü çalıştırdığından emin olun. Çalıştırarak yüklemesini onarmayı deneyebilirsiniz `aspnet_regiis -iru` komutu. (.NET Framework'u yükledikten sonra IIS yüklerseniz, örneğin, IIS doğru ASP.NET sayfaları çalıştırmak için yapılandırılacak değil.) Daha fazla bilgi için bkz: [ASP.NET IIS Kayıt Aracı (Aspnet\_regiis.exe)](https://msdn.microsoft.com/en-US/library/k6h9cz8h(v=vs.100).aspx).

### <a name="http-error-40314---forbidden"></a>HTTP Hatası 403.14 - Yasak

*Web sunucusu bu dizinin içindekileri listelemeyecek şekilde yapılandırılmış.*

Bu hata, korunan bir kaynağa isteği oluşabilir (gibi *Web.config* dosya) veya korumalı bir klasörde olan (gibi *uygulama\_veri* veya *uygulama\_Kod*).

### <a name="http-error-40417---not-found"></a>HTTP Hatası 404,17 - bulunamadı

*İstenen içerik komut dizisi olarak gözüküyor ve statik dosya işleyici tarafından sunulmayacak.*

Sunucu .NET Framework 4 kullanmak için düzgün yapılandırılmamış veya daha sonra ve bu nedenle kodda tanımıyor bu hata oluşabilir `@{ }` engeller. Açıklama için önceki bkz *HTTP Hata 403 - Yasak: erişim reddedildi*.

### <a name="http-error-4047---not-found"></a>HTTP Hatası 404.7 - bulunamadı

*İstek Filtreleme modülü dosya uzantısını reddedecek şekilde yapılandırıldı*

Bu hata oluşabilir *.cshtml* veya *.vbhtml* uzantıları sunucuda açıkça da engellenmiş. Bu sorunun belirtisi uzantısı, ancak dahil URL'leri dahil etmeyin URL'leri çalışan olduğunda *.cshtml* veya *.vbhtml* çalışmaz. Sitenin uzantılarında yeniden etkinleştirmek için olası bir çözüm olan *Web.config* dosya. Aşağıdaki örnekte nasıl etkinleştirileceği gösterilmiştir *.cshtml* uzantısı.

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample1.xml?highlight=5-6)]

### <a name="http-error-4048---not-found"></a>HTTP Hatası 404.8 - bulunamadı

*İstek Filtreleme modülü URL'deki bir hiddenSegment bölümü içeren yolu reddedecek şekilde yapılandırıldı.*

Bu hata, korunan bir kaynağa isteği oluşabilir (gibi *Web.config* dosya) veya korumalı bir klasörde olan (gibi *uygulama\_veri* veya *uygulama\_Kod*).

### <a name="this-type-of-page-is-not-served-server-error-in--application"></a>Bu sayfa türü ('/' uygulamasında sunucu hatası) sunulmuyor

HTTP Hatası 404,17 önceki açıklamasına bakın.

<a id="IssuesWithRazorCode"></a>
## <a name="issues-with-razor-code"></a>Razor kod sorunları

### <a name="the-name-class-does-not-exist-in-the-current-context"></a>Adı '*sınıfı*' geçerli bağlamda mevcut değil

Genellikle, bu hata gördüğünüz bir neden olan `class` başvuruları yardımcıyı ancak Yardımcısı yüklü değil. Örneğin, bir yardımcı kullanmayı denerseniz, ancak paket Nuget'ten yüklemediyseniz, bu hatayı görürsünüz. WebMatrix Galerisi'nde bulmak ve yardımcı yüklemek için kullanın.

Yardımcı yüklenir, ancak sayfa hala bu tanımıyor varsa, deneyin ekleme bir `using` kodu ifadesine. İçinde `using` deyimi, başvurusu yardımcı içeren ad alanı. Örneğin, ASP.NET Web Yardımcıları paketteki temel içinde yardımcılardır `System.Web.Helpers` ad alanı. Yardımcıyı kullanmak istediğiniz sayfanın üst kısmında, aşağıdaki satırı ekleyin:

`@using Microsoft.Web.Helpers;`

<a id="membership"></a>
## <a name="issues-with-security-and-membership"></a>Güvenlik ve üyeliği ile ilgili sorunlar

ASP.NET Web Pages'da (Razor) yerleşik güvenlik (Üyelik) sistemi kullanıyorsanız aşağıdaki sorunlarla karşılaşabilirsiniz.

### <a name="to-call-this-method-the-membershipprovider-property-must-be-an-instance-of-extendedmembershipprovider"></a>Bu yöntemi çağırabilmeniz için "Membership.Provider" özelliği "ExtendedMembershipProvider" örneği olmalıdır

Bu hatayı bildiren hiçbir `AspNetSqlMembershipProvider` sınıfı yapılandırılır. (Bir belirti sitesi düzgün yerel olarak çalışır ancak bir barındırma sağlayıcısının sunucuya yayımladığınızda, bu hata oluşturur olur.) Bu sorun için bir düzeltme olduğu sitenin aşağıdakileri ekleyerek basit üyelik açıkça etkinleştirmek için *Web.config* dosyası:

[!code-xml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample2.xml?highlight=6)]

<a id="email"></a>
## <a name="issues-with-sending-email"></a>E-posta gönderme sorunları

E-posta gönderme sorunlarını hata ayıklamak için zor olabilir. SMTP sunucusuna bağlanılamıyor ilk bir sorun olabilir. Bağlantı başarılı olursa, ASP.NET ileti kapalı SMTP sunucusuna aktarır. Ancak, SMTP sunucusu, göndermesini engeller ileti kendisini sorunları olabilir.

E-posta uygulamanızı başarıyla göndermez, aşağıdakileri deneyin:

- SMTP sunucusu adı şöyle görülür `smtp.provider.com` veya `smtp.provider.net`. Ancak, bir barındırma sağlayıcısına sitenizi yayımlarsanız, SMTP sunucu adı bu noktada olabilir `localhost`. Bu durum, yayımladıktan sonra sitenizi sağlayıcının sunucusunda çalıştığından, SMTP sunucusu uygulamanız açısından bakıldığında yerel olabileceğinden oluşur. Bu değişikliği sunucu adları, yayımlama işleminin bir parçası SMTP sunucu adını değiştirmek zorunda anlamına gelebilir.
- Bağlantı noktası numarası genellikle 25'tir. Ancak, bazı sağlayıcılar bağlantı noktasını kullanacak biçimde 587 veya bazı başka bir bağlantı gerektirir. SMTP sunucusuna sahip hangi bağlantı noktası numarasını kullanmanızı bekledikleri denetleyin.
- Doğru kimlik bilgilerini kullandığınızdan emin olun. Bir barındırma sağlayıcısına sitenizi yayımladıysanız, sağlayıcı özellikle e-posta için belirtilir kimlik bilgileri kullanın. Bu kimlik bilgileri yayımlamak için kullandığınız kimlik bilgilerinden farklı olabilir.
- Bazen kimlik bilgilerini hiç gerekmez. Kişisel ISS kullanarak e-posta gönderiyorsanız, e-posta sağlayıcınız kimlik bilgilerinizi zaten biliyor olabilirsiniz. Yayımladığınızda, yerel bilgisayarınızda test ne zaman farklı kimlik bilgileri kullanmanız gerekebilir.
- E-posta sağlayıcınız şifreleme kullanıyorsa, ayarlamak `WebMail.EnableSsl` için `true`.

E-posta gönderilirken bir hata varsa, şuna benzeyen bir standart ASP.NET hata iletisi görebilirsiniz:

![E-posta ile ilgili bir sorun olduğunda ASP.NET hata iletisi](aspnet-web-pages-razor-troubleshooting-guide/_static/image1.png)

De kullanarak e-posta gönderme sorunlarını ayıklayabilirsiniz bir `try-catch` aşağıdaki örnekteki gibi bloğu. Kullandığınızda, bir `try-catch` bloğu, ASP.NET, standart hata iletileri görüntülenmez. Bunun yerine, hatayı yakalamak için `catch` blok kısmı.

[!code-cshtml[Main](aspnet-web-pages-razor-troubleshooting-guide/samples/sample3.cshtml)]

İçin uygun değerleri yerine `your-SMTP-server-name`ve benzeri. Bu şekilde görebileceğiniz hata iletileri bazıları şunlardır:

- *Posta gönderme başarısız oldu.*

    veya

    *Bağlı taraf düzgün zaman ya da kurulan bağlantı bağlanılan ana makine yanıt başarısız olduğundan başarısız oldu, bir süre sonra yanıt vermediği için bir bağlantı girişimi başarısız oldu*

    Bu hata genellikle uygulama SMTP sunucusuna bağlanamadı anlamına gelir. Sunucu adını denetleyin ve bağlantı noktası numarası.
- *Posta kutusu kullanılamıyor. Sunucu yanıt: 5.1.0 &lt; someuser@invaliddomain &gt; gönderen reddedilen: Geçersiz gönderen etki alanı*

    Bu iletiyi bildiren `From` adresi doğru değil veya eksik.
- *Belirtilen dize için bir e-posta adresi gereken biçimde değil.*

    Bu hata, gösterebilir değerini `To` veya `From` özellikleri e-posta adresleri olarak tanınmadı. (ASP.NET, e-posta adresi doğru biçimde gibi kullanıcının yalnızca geçerli olduğunu denetleyemez  *name@domain.com* .)

> [!NOTE]
> Hatayı görüntüler biçimlendirme Kaldır (`@errorMessage`) canlı bir siteye sayfa yayımlamadan önce. Kullanıcıların bir sunucudan alma hata iletilerine izin vermek için iyi bir fikir değil.


<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

[ASP.NET Web sayfaları (Razor) ile ilgili SSS](https://go.microsoft.com/fwlink/?LinkId=253000)

[WebMatrix ve ASP.NET Web sayfaları](https://forums.asp.net/1224.aspx/1?WebMatrix) ASP.NET Web sitesi Forumu
