---
uid: single-page-application/overview/templates/backbonejs-template
title: "Omurga şablon | Microsoft Docs"
author: madskristensen
description: "Backbone.js SPA şablonu"
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 3b8eabd3cefcb96dc40bbf6cc6e3ee81accb0d7c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="backbone-template"></a>Omurga şablonu
====================
tarafından [Kristensen Mads](https://github.com/madskristensen)

> Omurga SPA şablonu Kazi Manzur Rashid tarafından yazıldı
> 
> [Backbone.js SPA şablonunu yükle](https://go.microsoft.com/fwlink/?LinkId=293631)


Backbone.js SPA şablon hızlı bir şekilde kullanarak etkileşimli istemci tarafı web uygulamaları oluşturmaya başlamanıza yardımcı olmak için tasarlanmıştır [Backbone.js.](http://backbonejs.org/)

Şablonu, bir ASP.NET MVC Backbone.js uygulama geliştirmek için ilk bir çatı sağlar. Kutudan çıktığında kullanıcı kaydolma, oturum açma, parola sıfırlama ve temel e-posta şablonları ile kullanıcı onayı dahil olmak üzere temel kullanıcı oturum açma işlevselliği sağlar.

Gereksinimleri:

- [ASP.NET ve Web Araçları 2012.2 güncelleştirme](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Bir omurga şablonu projesi oluşturma

İndirin ve şablonu yukarıdaki karşıdan yükleme düğmesini tıklatarak yükleyin. Şablonu bir Visual Studio Uzantısı (VSIX) dosyası olarak paketlenir. Visual Studio yeniden başlatmanız gerekebilir.

İçinde **şablonları** bölmesinde, **yüklü şablonlar** ve genişletin **Visual C#** düğümü. Altında **Visual C#**seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. Proje adı ve'ı tıklatın **Tamam**.

![](backbonejs-template/_static/image1.png)

İçinde **yeni proje** Backbone.js SPA Proje Sihirbazı, seçin.

![](backbonejs-template/_static/image2.png)

Derleme ve hata ayıklama olmadan uygulamayı çalıştırmak için CTRL-F5 tuşuna basın veya hata ayıklama ile çalıştırmak için F5 tuşuna basın.

![](backbonejs-template/_static/image3.png)

"Hesabım" tıklatarak ayarlama oturum açma sayfası açar:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>İzlenecek yol: İstemci kodu

Şimdi istemci tarafı ile başlar. İstemci uygulama betikler ~/Scripts/application klasöründe bulunur. Uygulama yazılmış [TypeScript](http://www.typescriptlang.org/) (.ts dosyaları) (.js dosyaları) JavaScript ile derlenen.

**Uygulama**

`Application`Application.TS içinde tanımlanmıştır. Bu nesne, uygulama başlatır ve kök ad alanı olarak davranır. Kullanıcının oturum açtığı gibi uygulama arasında paylaşılacak yapılandırma ve durum bilgilerini tutar.

`application.start` Yöntemi kalıcı görünümler oluşturur ve kullanıcı oturum açma gibi uygulama düzeyindeki olayları için olay işleyicileri iliştirir. Ardından, varsayılan yönlendirici oluşturur ve herhangi bir istemci-tarafı URL belirtilen olup olmadığını denetler. Varsayılan URL'ye yeniden yönlendirilen değil, varsa (#! /).

**Olayları**

Olaylar, her zaman geniş geliştirme bileşenleri bağlı olduğunda önemlidir. Uygulamalar genellikle birden çok yanıt olarak bir kullanıcı eylemi işlemleri. Omurga modeli, koleksiyon ve görünüm gibi bileşenlerle yerleşik olayları sağlar. Bu bileşenler arasında arası bağımlılıklar oluşturmak yerine, şablonu "pub/alt" modeli kullanır: `events` events.ts içinde tanımlanan nesne, olay hub'ı yayımlama ve uygulama olaylara abone olma görür. `events` Tek bir nesnedir. Aşağıdaki kod, bir olaya abone olmak ve olayı tetiklemek gösterilmektedir:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Yönlendirici**

Backbone.js içinde bir yönlendirici istemci-tarafı sayfaları Yönlendirme ve eylemleri ve olayları bağlanmak için kullanılan yöntemler sağlar. Şablonu router.ts içinde tek bir yönlendirici tanımlar. Yönlendirici activable görünümler oluşturur ve görünüm geçiş yaparken durumunu korur. (Activable görünümleri sonraki bölümde açıklanmaktadır.) Başlangıçta, iki kukla görünümler, projenin içerdiğinden ev ve hakkında. Ayrıca, yol olmayan biliniyorsa görüntülenen bir NotFound görünüme sahiptir.

**Görünümler**

Görünümleri ~/Scripts/application/görünümlerde tanımlanır. Görünümler, activable görünümleri ve kalıcı iletişim görünümleri iki tür vardır. Activable görünümleri yönlendirici tarafından çağrılır. Activable bir görünümü gösterilir, diğer tüm activable görünümleri etkin olur. Activable bir görünüm oluşturmak için görünümü ile genişletmek `Activable` nesnesi:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

İle genişletme `Activable` iki yeni yöntemleri görünümüne ekler `activate` ve `deactivate`. Yönlendirici etkinleştirmek ve devre dışı görünümü için bu yöntemleri çağırır.

Kalıcı görünümleri olarak gerçekleştirilen [Twitter Bootstrap](http://twitter.github.com/bootstrap/) kalıcı iletişim kutuları. `Membership` Ve `Profile` kalıcı görünümleri görünümlerdir. Model görünümlerini herhangi bir uygulama olayları tarafından çağrılabilir. Örneğin, `Navigation` "Hesabım" bağlantısını tıklatarak Görünüm, gösterir ya da `Membership` görünüm veya `Profile` kullanıcının oturum açtığı bağlı olarak görüntüle. `Navigation` Ekler tıklatın sahip herhangi bir alt öğe olay işleyicilerine `data-command` özniteliği. HTML biçimlendirmesi şöyledir:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Olayları kanca navigation.ts kod aşağıdadır:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modelleri**

Modelleri ~/Scripts/application/modellerinde tanımlanır. Tüm modelleri üç temel şey vardır: varsayılan öznitelikler, doğrulama kuralları ve bir sunucu tarafı bitiş noktası. Aşağıda, genel bir örnek verilmiştir:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Eklentiler**

~/Scripts/application/lib klasörü birkaç kullanışlı jQuery eklentileri içerir. Form.ts dosyası biçiminde verilerle çalışmak için bir eklenti tanımlar. Genelde serileştirmek veya form verileri seri durumdan ve herhangi bir model doğrulama hatasını göstermek gerekir. Eklenti form.ts gibi yöntemlerine sahiptir `serializeFields`, `deserializeFields`, ve `showFieldErrors`. Aşağıdaki örnek, bir model için bir form serileştirmek gösterilmiştir.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Eklenti flashbar.ts kullanıcıya geri bildirim iletileri çeşitli sağlar. Yöntemler `$.showSuccessbar`, `$.showErrorbar` ve `$.showInfobar`. Arka planda sorunsuz şekilde animasyonlu iletileri göstermek için Twitter Bootstrap uyarıları kullanır.

Eklenti confirm.ts tarayıcının değiştirir API biraz farklı olmasına rağmen iletişim kutusunda, onaylayın:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>İzlenecek yol: Sunucu kodu

Artık sunucu tarafında bakalım.

**Denetleyicileri**

Bir tek sayfalı uygulama sunucusu kullanıcı arabiriminde yalnızca küçük bir rol oynar. Genellikle, sunucunun başlangıç sayfası oluşturur ve ardından gönderir ve JSON verilerini alır.

İki MVC denetleyicileri şablonunu sahiptir: `HomeController` başlangıç sayfasını işler ve `SupportsController` yeni kullanıcı hesapları onaylayın ve parola sıfırlama için kullanılır. Şablondaki tüm diğer denetleyicileri JSON veri gönderip ASP.NET Web API denetleyicilerinin etkilenir. Varsayılan olarak, denetleyicileri yeni kullanmak `WebSecurity` kullanıcıyla ilgili görevleri gerçekleştirmek için sınıf. Ancak, aynı zamanda bu görevler için temsilcileri geçirmenize olanak sağlayan isteğe bağlı oluşturucular sahiptirler. Bu işlem, testi kolaylaştırır ve değiştirmenizi sağlar `WebSecurity` IOC kapsayıcısını kullanarak başka bir şey ile. Aşağıda bir örnek verilmiştir:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Görünümler

Görünümleri modüler olacak şekilde tasarlanmıştır: bir sayfanın her bölümünü kendi özel görünüme sahiptir. Bir tek sayfa uygulaması karşılık gelen bir denetleyici yok görünümleri de içerecek şekilde yaygındır. Bir görünüm çağırarak içerebilir `@Html.Partial('myView')`, ancak bu yorucu alır. Bu işlemi kolaylaştırmak için şablon bir yardımcı yöntem tanımlar `IncludeClientViews`, tüm belirtilen klasördeki görünümlerinin işleyen:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Klasör adı belirtilmezse, varsayılan klasör adı "ClientViews" olur. İstemci görünümünüzü kısmi görünümleri de kullanıyorsa, kısa çizgi karakteri ile kısmi görünüm adını (örneğin, `_SignUp`). `IncludeClientViews` Yöntemi adı alt çizgi ile başlayan tüm görünümleri dışlar. Kısmi görünüm istemci görünüme dahil etmek için arama `Html.ClientView('SignUp')` yerine `Html.Partial('_SignUp')`.

**E-posta gönderme**

E-posta göndermek için şablonun kullanan [posta](http://aboutcode.net/postal). Ancak, posta kodu ile geri kalanından soyutlanır `IMailer` , başka bir uygulama ile kolayca yerini alabilecek şekilde arabirim. E-posta şablonlarını görünümler/e-postalar klasöründe bulunur. Gönderenin e-posta adresi web.config dosyasında belirtilen `sender.email` , anahtar **appSettings** bölümü. Ayrıca, ne zaman `debug="true"` web.config dosyasında, uygulama geliştirme hızlandırmak için kullanıcı e-posta onayı gerektirmez.

## <a name="github"></a>GitHub

Üzerinde Backbone.js SPA şablon da bulabilirsiniz [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
