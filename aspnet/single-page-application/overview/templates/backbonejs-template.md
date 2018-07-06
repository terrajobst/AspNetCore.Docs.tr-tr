---
uid: single-page-application/overview/templates/backbonejs-template
title: Omurga şablonu | Microsoft Docs
author: madskristensen
description: Backbone.js SPA şablonu
ms.author: aspnetcontent
ms.date: 04/04/2013
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: f483a7ef840b7bce71187fd929e9667ee1eaf053
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811010"
---
<a name="backbone-template"></a>Omurga şablonu
====================
tarafından [Mads Kristensen](https://github.com/madskristensen)

> Omurga SPA şablon Kazi Manzur Rashid tarafından yazılmıştır.
> 
> [Backbone.js SPA şablonunu indirme](https://go.microsoft.com/fwlink/?LinkId=293631)


Backbone.js SPA şablonu kullanarak etkileşimli istemci tarafı web uygulamalarını hızlı bir şekilde oluşturmaya başlamanıza yardımcı olmak üzere tasarlanmıştır [Backbone.js.](http://backbonejs.org/)

Şablon, bir ASP.NET MVC Backbone.js uygulama geliştirmek için ilk bir çatı sağlar. Kullanıma hazır, kullanıcı oturum açma, kaydolma, parola sıfırlama ve kullanıcı onayı ile temel bir e-posta şablonları da dahil olmak üzere temel kullanıcı oturum açma işlevselliği sağlar.

Gereksinimler:

- [ASP.NET ve Web Araçları 2012.2 güncelleştirme](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Omurga şablonu projesi oluşturma

İndirin ve yukarıdaki indir düğmesine tıklayarak şablonu yükleyin. Şablon, Visual Studio Uzantısı (VSIX) dosyası olarak paketlenir. Visual Studio'yu yeniden başlatmanız gerekebilir.

İçinde **şablonları** bölmesinde **yüklü şablonlar** genişletin **Visual C#** düğümü. Altında **Visual C#** seçin **Web**. Proje şablonları listesinde seçin **ASP.NET MVC 4 Web uygulaması**. Projeyi adlandırın ve tıklayın **Tamam**.

![](backbonejs-template/_static/image1.png)

İçinde **yeni proje** Backbone.js SPA proje seçin.

![](backbonejs-template/_static/image2.png)

Derleme ve hata ayıklama olmadan uygulamayı çalıştırmak için CTRL-F5 tuşuna basın veya hata ayıklamayla çalıştırmak için F5 tuşuna basın.

![](backbonejs-template/_static/image3.png)

"Hesabım" tıklayarak oturum açma sayfası sunar:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>İzlenecek yol: İstemci kodu

Şimdi, istemci tarafı ile başlar. İstemci uygulama betiklerini ~/Scripts/application klasöründe bulunur. Uygulamanın yazıldığı [TypeScript](http://www.typescriptlang.org/) (.ts dosyalarını) JavaScript (.js dosyaları) ile derlenir.

**Uygulama**

`Application` Application.TS içinde tanımlanır. Bu nesne, uygulamayı başlatır ve kök ad alanı görev yapar. Kullanıcının oturum açtığı gibi uygulama arasında paylaşılan yapılandırma ve durum bilgilerini tutar.

`application.start` Yöntemi kalıcı görünümler oluşturur ve kullanıcı oturum açma gibi uygulama düzeyinde olaylar için olay işleyicileri ekler. Ardından, varsayılan yönlendirici oluşturur ve herhangi bir istemci-tarafı URL belirtilen olup olmadığını denetler. Varsayılan URL'ye yeniden yönlendirilen değil, varsa (#! /).

**Olaylar**

Geliştirme gevşek bir şekilde eşlenen bileşenler olduğunda olayları her zaman önemlidir. Uygulamalar genellikle yanıt olarak bir kullanıcı eylemi birden çok işlem gerçekleştirin. Omurga bileşenleri gibi Model, koleksiyon ve görünüm ile yerleşik olayları sağlar. Bu bileşenler arasındaki arası bağımlılıklar oluşturmak yerine, şablon "pub/sub" modelini kullanır: `events` events.ts içinde tanımlanan nesne yayımlamak ve uygulama olaylara abone olma bir olay hub'ı olarak görev yapar. `events` Tek bir nesnedir. Aşağıdaki kod, bir olaya abone olun ve ardından olayı tetiklemek gösterilmektedir:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Yönlendirici**

Backbone.js yönlendirici istemci-tarafı sayfaları Yönlendirme ve bunları Eylemler ve olaylar bağlamak için yöntemler sağlar. Şablon router.ts içinde tek bir yönlendirici tanımlar. Yönlendirici activable görünümler oluşturur ve görünümler arasında geçiş yaparken durumu korur. (Activable görünümleri, sonraki bölümde açıklanmıştır.) Başlangıçta, proje işlevsiz iki görünüm sahip giriş ve ilgili. Ayrıca, yol olmayan biliniyorsa görüntülenen bir NotFound görünüme sahiptir.

**Görünümler**

Görünümleri ~/Scripts/application/görünümler içinde tanımlanır. Görünümler, activable görünümleri ve kalıcı bir iletişim görünümleri iki tür vardır. Activable görünümleri yönlendirici tarafından çağrılır. Bir activable görünümü gösterilirken, diğer activable görünümleri devre dışı olur. Görünüme activable bir görünüm oluşturmak için genişletme `Activable` nesnesi:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

İle genişletme `Activable` iki yeni yöntem görünümüne ekler `activate` ve `deactivate`. Yönlendirici etkinleştirmek ve görünüm kalmış devre dışı için bu yöntemleri çağırır.

Görünümleri kalıcı olarak gerçekleştirilen [Twitter Bootstrap](http://twitter.github.com/bootstrap/) kalıcı iletişim kutuları. `Membership` Ve `Profile` kalıcı görünümleri görünümleridir. Model görünümlerini herhangi bir uygulama olayları tarafından çağrılabilir. Örneğin, `Navigation` görünüm, "Hesabım" bağlantısını tıklatarak gösterir ya da `Membership` görünümü veya `Profile` görünümü, kullanıcının oturum açtığı bağlı olarak. `Navigation` Ekler tıklama olay işleyicileri olan alt öğeler için `data-command` özniteliği. HTML biçimlendirmesi şöyledir:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Olayları yeteneklerinizi navigation.ts kod şöyledir:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modelleri**

Modelleri ~/Scripts/application/modellerinde tanımlanır. Tüm modeller üç temel noktalar vardır: varsayılan öznitelikler, doğrulama kurallarını ve sunucu tarafı uç noktası. Tipik bir örnek aşağıda verilmiştir:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Eklentiler**

Bazı kullanışlı jQuery eklentileri ~/Scripts/application/lib klasör içeriyor. Form verileri ile çalışma için bir eklentiyi form.ts dosya tanımlar. Genelde serileştirmek veya form verilerinin serisini ve tüm model doğrulama hatalarını göster gerekir. Eklenti form.ts yöntemleri gibi sahip `serializeFields`, `deserializeFields`, ve `showFieldErrors`. Aşağıdaki örnek, bir form bir Modeli'ne serileştirmek gösterilmektedir.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

Eklenti flashbar.ts kullanıcıya geri bildirim iletileri çeşitli sağlar. Yöntemler `$.showSuccessbar`, `$.showErrorbar` ve `$.showInfobar`. Arka planda güzelce animasyonlu iletileri göstermek için Twitter Bootstrap uyarılar kullanır.

Tarayıcı eklentisi confirm.ts değiştirir API biraz farklı olmasına karşın, iletişim kutusunda onaylayın:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>İzlenecek yol: Sunucu kodu

Artık sunucu tarafında bakalım.

**Denetleyiciler**

Bir tek sayfalı uygulamada, sunucu kullanıcı arabiriminde yalnızca küçük bir rol oynar. Genellikle, sunucu başlangıç sayfasını işler ve gönderir ve JSON verilerini alır.

İki MVC denetleyicileri şablonda: `HomeController` ilk sayfasında, işler ve `SupportsController` yeni kullanıcı hesaplarını onaylamak ve parola sıfırlama için kullanılır. Tüm diğer şablonda, JSON veri göndermek ve almak, ASP.NET Web APİ'si denetleyicilerinin denetleyicileridir. Varsayılan olarak, yeni denetleyicileri kullanmak `WebSecurity` kullanıcı ile ilgili görevleri gerçekleştirmek için sınıf. Ancak, aynı zamanda bu görevler için varyans geçirmenize olanak sağlayan isteğe bağlı oluşturucular sahiptirler. Bu, testi kolaylaştırır ve değiştirmenizi sağlar `WebSecurity` IOC kapsayıcısını kullanarak başka bir şey ile. Aşağıda bir örnek verilmiştir:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Görünümler

Görünümler, modüler olmak tasarlanmıştır: kendi özel görünümü sayfasının her bölümüne sahip. Bir tek sayfalı uygulamada, karşılık gelen hiçbir denetleyici yoktur görünümleri de içerecek şekilde yaygındır. Çağırarak bir görünüm içerebilir `@Html.Partial('myView')`, ancak bu yorucu bir süreç alır. Bunu kolaylaştırmak için şablon bir yardımcı yöntem tanımlar `IncludeClientViews`, tüm belirtilen klasördeki görünümlerinin işleyen:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Klasör adı belirtilmezse, varsayılan klasör adı "ClientViews" şeklindedir. İstemci görünümünüzü kısmi görünümler de kullanıyorsa, alt çizgi karakteri ile kısmi görünüm adını (örneğin, `_SignUp`). `IncludeClientViews` Yöntemi adı alt çizgi ile başlayan herhangi bir görünüm dahil değildir. Kısmi görünüm istemci Görünümü'nde eklemek için çağrı `Html.ClientView('SignUp')` yerine `Html.Partial('_SignUp')`.

**E-posta gönderme**

E-posta göndermek için şablonu kullanan [posta](http://aboutcode.net/postal). Ancak, posta kodu ile geri kalanından soyutlanır `IMailer` arabirim için bunu kolaylıkla başka bir uygulama ile değiştirebilir. E-posta şablonları, görünümler/e-postaları klasöründe bulunur. Gönderenin e-posta adresi web.config dosyasında belirtilen `sender.email` anahtarı **appSettings** bölümü. Ayrıca, `debug="true"` web.config dosyasında, uygulama geliştirme sürecinizi hızlandırmak için kullanıcının e-posta onayı gerekli değildir.

## <a name="github"></a>GitHub

Üzerinde Backbone.js SPA şablonunu da bulabilirsiniz [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
