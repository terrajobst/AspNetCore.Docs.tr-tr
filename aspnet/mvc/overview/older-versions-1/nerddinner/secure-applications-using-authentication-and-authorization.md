---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: "Güvenli kimlik doğrulama ve yetkilendirme kullanarak uygulamaları | Microsoft Docs"
author: microsoft
description: "Adım 9 kullanıcılar kaydetmeniz gerekir böylece NerdDinner uygulamamız güvenli hale getirmek için yetkilendirme ve kimlik doğrulaması ekleme gösterir ve oturum açma oluşturmak için siteye..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: a23b2cf4d1728624698c0db49c25ea7efd3af67d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="secure-applications-using-authentication-and-authorization"></a>Güvenli kimlik doğrulama ve yetkilendirme kullanarak uygulamaları
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 9. adım bir ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.
> 
> Adım 9 kullanıcılar kaydetmeniz gerekir böylece NerdDinner uygulamamız güvenli hale getirmek için yetkilendirme ve kimlik doğrulaması ekleme gösterir ve yeni azalma ve yalnızca bir Yemeği barındırma kullanıcı oluşturmak için siteye oturum açma düzenleyebilir, daha sonra.
> 
> ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner 9. adım: Kimlik doğrulama ve yetkilendirme

Şu anda uygulama herkes verir bizim NerdDinner oluşturma ve tüm Yemeği ayrıntılarını düzenleme yeteneğini sitesini ziyaret edin. Kullanıcıların kaydetmeniz gerekir böylece bu değiştirelim ve yeni azalma oluşturmak ve böylece kimin bir Yemeği barındırma kullanıcı onu daha sonra düzenleyebilirsiniz bir kısıtlama eklemek için site oturum açın.

Bu ayarı etkinleştirmek için kimlik doğrulama ve yetkilendirme uygulamamız güvenli hale getirmek için kullanacağız.

### <a name="understanding-authentication-and-authorization"></a>Anlama kimlik doğrulama ve yetkilendirme

*Kimlik doğrulama* tanımlamak ve bir uygulamaya erişmeyi istemci kimliğini doğrulama işlemidir. Daha fazla basitçe, "bir Web sitesini ziyaret ettiğinizde olan son kullanıcı" tanımlama hakkında öyledir. ASP.NET tarayıcı kullanıcıların kimliklerini doğrulamak için birden çok yöntemini destekler. Internet web uygulamaları için kullanılan en yaygın kimlik doğrulama yaklaşımını "Forms kimlik doğrulaması" adı verilir. Form kimlik doğrulaması, bir HTML oturum açma formu, uygulama içinde yazar ve ardından bir son kullanıcı bir veritabanı veya başka bir parola kimlik bilgisi deposunda karşı gönderen kullanıcı adı/parola doğrulamak bir geliştirici sağlar. Kullanıcı adı/parola bileşimi doğru ise, geliştirici sonra gelecekteki istekler genelinde kullanıcıyı tanımlamak için şifrelenmiş bir HTTP tanımlama vermek için ASP.NET sorabilirsiniz. Form kimlik doğrulaması ile NerdDinner uygulamamızı kullanarak gerekir.

*Yetkilendirme* kimliği doğrulanmış bir kullanıcı belirli bir URL/kaynağa erişmek için veya bazı eylemleri gerçekleştirmek için izni olup olmadığını belirleme işlemidir. Örneğin, NerdDinner uygulamamız içinde biz yalnızca oturum açan kullanıcılar erişebilir yetkilendirmek istersiniz */azalma/oluşturma* URL ve yeni azalma oluşturun. Biz de böylece yalnızca bir Yemeği barındırma kullanıcının – düzenlemek ve diğer tüm kullanıcılara düzenleme erişimi reddetme yetkilendirme mantığı eklemek istersiniz.

### <a name="forms-authentication-and-the-accountcontroller"></a>Form kimlik doğrulaması ve AccountController

Yeni ASP.NET MVC uygulamaları oluştururken ASP.NET MVC için varsayılan Visual Studio Proje şablonu otomatik olarak form kimlik doğrulamasını etkinleştirir. Bir site içinde güvenlik tümleştirmek gerçekten kolay hale getirir projeye – önceden oluşturulmuş hesap oturum açma sayfası uygulama da otomatik olarak ekler.

Ona erişen kullanıcı kimliği doğrulanmamış olduğunda varsayılan Site.master ana sayfa "Oturum Aç" bağlantı sağ üst tarafında sitenin görüntüler:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

"Oturum Aç" bağlantısını tıklatarak geçen bir kullanıcıya */Account/oturum açma* URL'si:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Kayıtlı olmayabilirsiniz ziyaretçileri yapabilirsiniz onlara sürer "Register" bağlantıyı – tıklatarak */Account/Register* URL ve izin vermeniz hesabı ayrıntılarını girin:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

"Register" düğmesini tıklatarak ASP.NET üyelik sistemi içinde yeni bir kullanıcı oluşturun ve forms kimlik doğrulaması kullanarak site üzerine kullanıcı kimlik doğrulaması.

Bir kullanıcı oturum açmış olduğunda, bir "Hoş Geldiniz [username]!" çıktısını almak için sayfanın sağ üst Site.master değiştirir ileti ve işler bir "günlük birinde yerine" bir "Oturumu Kapat" bağlayın. "Oturumu Kapat" bağlantısını tıklatarak kullanıcı kaydeder:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

Yukarıdaki oturum açma, oturum kapatma ve kayıt işlevi proje oluşturduğunuzda, Projemizin için Visual Studio tarafından eklenen AccountController sınıfı içinde uygulanır. AccountController ilişkin kullanıcı Arabirimi \Views\Account dizininde görünüm şablonları kullanılarak uygulanır:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

AccountController sınıf ASP.NET formları kimlik doğrulamasını sistem şifreli kimlik doğrulaması tanımlama bilgileri ve depolamak ve kullanıcı adları/parolaları doğrulamak için ASP.NET üyelik API'si vermek için kullanır. ASP.NET üyelik API genişletilebilir ve kullanılacak bir parola kimlik bilgisi deposunu sağlar. ASP.NET, kullanıcı adı/parola Active Directory veya bir SQL veritabanı içinde depolama yerleşik üyelik sağlayıcısı uygulamaları ile birlikte gelir.

Proje kökündeki "web.config" dosyasını açarak ve aramakta NerdDinner uygulamamızı kullanması gereken hangi üyelik sağlayıcısı yapılandırabilmeniz için &lt;üyelik&gt; içindeki bölümü. Proje oluşturduğunuzda eklenen varsayılan web.config SQL üyelik sağlayıcısı kaydeder ve "ApplicationServices" adlı bir bağlantı dizesi kullanmak için yapılandırır veritabanı konumu belirtmek için.

Varsayılan "ApplicationServices" bağlantı dizesini (içinde belirtilen &lt;connectionStrings&gt; web.config dosyasının) SQL Express kullanmak üzere yapılandırılmış. "ASPNETDB. adlı SQL Express bir veritabanına işaret eder MDF"uygulamanın altında" uygulama\_Data "dizini. Bu veritabanı üyelik API'si uygulama içerisinden ilk defa yoksa, ASP.NET otomatik olarak veritabanını oluşturmak ve uygun üyelik veritabanı şeması içindeki sağlayın:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Biz isteyen bir tam SQL Server örneğini kullanın (veya uzak bir veritabanına bağlanmak için) SQL Express kullanmak yerine, tüm yapılacaklar ihtiyacımız ise "ApplicationServices" bağlantı dizesi web.config dosyasında güncelleştirmek ve emin olmak için uygun üyelik şeması konumundaki işaret veritabanına eklendi. Çalıştırabilirsiniz "aspnet\_regsql.exe" bir üyelik ve bir ASP.NET uygulama hizmetleri için uygun şema eklemek için \Windows\Microsoft.NET\Framework\v2.0.50727\ dizininde yardımcı programı.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Azalma/oluşturma [Authorize] filtresini kullanarak URL yetkilendirme

Güvenli kimlik doğrulaması ve hesap yönetim uygulaması NerdDinner uygulama için etkinleştirmek için herhangi bir kod yazmak zorunda alamadık. Kullanıcılar yeni hesapları bizim uygulama ve sitenin oturum açma/oturum kapatma ile kaydedebilirsiniz.

Şimdi biz yetkilendirme mantığı uygulamaya ekleyin ve bunlar görebileceği ve neleri site içinde yapamayacağı denetlemek için ziyaretçileri kullanıcı adı ve kimlik doğrulama durumu kullanın. Bizim DinnersController sınıfı "Oluştur" eylem yöntemlerinin yetkilendirme mantığı ekleyerek başlayalım. Biz, özellikle gerektirecektir erişen kullanıcılar */azalma/oluşturma* URL oturum,. Bunlar oturum açmadıysanız, oturum açma böylece biz bunları oturum açma sayfasına yeniden yönlendir.

Bu mantık uygulanması oldukça kolaydır. Tüm yapılacaklar ihtiyacımız olan bizim oluşturma eylem yöntemleri [Authorize] filtresi öznitelik eklemek için şu şekilde:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC "için eylem yöntemleri bildirimli olarak uygulanabilir yeniden kullanılabilir mantığını uygulamak için kullanılan eylem filtrelerini" oluşturabilme destekler. [Authorize] filtre ASP.NET MVC tarafından sağlanan yerleşik eylem filtreleri biridir ve yetkilendirme kuralları eylem yöntemleri ve denetleyici sınıfları bildirimli olarak uygulamak bir geliştirici sağlar.

Herhangi bir parametre (like yukarıda) olmadan uygulandığında [Authorize] filtre eylem yöntemi isteği yapan kullanıcı içinde – oturum açmış olmanız gerekir ve yoksa onu otomatik olarak tarayıcı oturum açma URL'sine yönlendirir zorlar. İlk olarak istenen URL bir sorgu dizesi bağımsız değişken olarak geçirilen bu yeniden yönlendirme yaparken (örneğin: / Account/oturum açma? ReturnUrl = % 2fDinners % 2fCreate). Bunlar oturum sonra AccountController kullanıcıyı başta istenen URL'ye sonra yönlendirir.

[Authorize] filtre isteğe bağlı olarak kullanıcı hem de ve izin verilen kullanıcıların veya izin verilen güvenlik rolünün bir üyesi listesini içinde kaydedildiğini istemek için kullanılan bir "Kullanıcılar" veya "Rol" özelliği belirtmek için özelliğini destekler. Örneğin, aşağıdaki kodu yalnızca iki belirli kullanıcılar, "scottgu" ve "billg" azalma/Oluştur URL'ye erişmek sağlar:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Kod içinde belirli bir kullanıcı adları katıştırma ancak oldukça beklemediğiniz sürdürülebilir olma eğilimindedir. Daha iyi bir yaklaşım üst düzey "rol" tanımlamaktır, kod karşı denetler ve ardından bir veritabanı veya active directory sistemi (koddan dışarıdan depolanması gerçek kullanıcı eşleme listesi etkinleştirme) kullanarak rolü halinde kullanıcıları eşlemek için. ASP.NET, yerleşik rol yönetimi API bu kullanıcı/rol eşleme gerçekleştirmek yardımcı olabilir (olanları SQL ve Active Directory dahil) rol sağlayıcıları yerleşik bir kümesini içerir. Ardından yalnızca kullanıcıların belirli "Yönetici" rolü içindeki azalma/Oluştur URL erişmesine izin vermek üzere kod güncelleştiriyoruz:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Oluştururken User.Identity.Name özelliğini kullanarak azalma

Biz denetleyici temel sınıfını kullanıma sunulan User.Identity.Name özelliğini kullanarak bir isteğin geçerli oturum açma kullanıcı adını alabilir.

Önceki biz bizim Create() eylem yöntemini HTTP POST sürümü uygulandığında biz sabit kodlanmış bir statik dizeye Yemeği "HostedBy" özelliği vardı. Biz şimdi bunun yerine User.Identity.Name özelliği kullanmak için bu kodu güncelleştir yanı sıra bir RSVP Yemeği oluşturma ana bilgisayar için otomatik olarak Ekle:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Create() yöntemi [Authorize] özniteliği ekledik olduğundan, ASP.NET MVC azalma/Oluştur URL'yi ziyaret eden kullanıcı sitesinde oturum açtıysa, eylem yönteminin yalnızca yürütüldüğünü sağlar. Bu nedenle, User.Identity.Name özellik değeri her zaman geçerli bir kullanıcı adı içerir.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Düzenlerken User.Identity.Name özelliğini kullanarak azalma

Artık kullanıcıları sınırlar ve böylece yalnızca bunlar kendilerini barındıran azalma özelliklerini düzenleyebilirsiniz bazı yetkilendirme mantığı ekleyelim.

Bu konuda yardımcı olmak için önce bir "IsHostedBy(username)" yardımcı yöntemi (içinde daha önce oluşturduğumuz Dinner.cs parçalı sınıf) bizim Yemeği nesnesine ekleyeceğiz. True veya false sağlanan kullanıcı Yemeği HostedBy özelliği eşleşen ve bunları büyük küçük harf duyarlı dize karşılaştırması gerçekleştirmek için gerekli mantığı yalıtır bağlı olarak bu yardımcı yöntemini döndürür:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Ardından bir [Authorize] özniteliği Edit() eylem yöntemlerini bizim DinnersController sınıfı içinde ekleyeceğiz. Bu kullanıcılar isteğine oturum açmanız gerekir, sağlayacak bir */Dinners/düzenleme / [kimlik]* URL.

Kodu daha sonra oturum açan kullanıcının Yemeği konak eşleşip eşleşmediğini doğrulamak için Dinner.IsHostedBy(username) yardımcı yöntemini kullanan bizim düzenleme yöntemleri ekleyebilirsiniz. Kullanıcı konak değilse, biz "InvalidOwner" görüntülemek ve istek sonlandırmak. Bunu yapmak için kod aşağıdaki gibi görünür:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Biz sonra \Views\Dinners dizinde sağ tıklayın ve Add - seçin&gt;görüntülemek yeni bir "InvalidOwner" görünümü oluşturmak için menü komutu. Biz ile dolduracaksınız aşağıdaki hata iletisi:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

Ve şimdi kullanıcının sahip olmadığınız bir Yemeği Düzenle girişiminde bulunduğunda, bunlar bir hata iletisi alırsınız:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Azalma de silmek ve yalnızca bir Yemeği ana onu silebilirsiniz olun izni kilitlemek için bizim denetleyicisi içinde biz Delete() eylem yöntemleri için aynı adımları yineleyebilirsiniz.

### <a name="showinghiding-edit-and-delete-links"></a>Gösterme/gizleme düzenleme ve silme bağlantılar

Biz, düzenleme ve silme eylem yöntemine bizim DinnersController sınıfının bizim ayrıntıları URL'den bağlıyorsanız:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Şu anda biz ayrıntıları URL ziyaretçiye Yemeği ana olup bakılmaksızın düzenleme ve silme eylem bağlantıları gösteriliyor. Böylece bağlantıları ziyaret kullanıcı Yemeği sahibi ise yalnızca görüntülenir bu değiştirelim.

Bizim DinnersController içinde Details() eylem yöntemi Yemeği nesnesini alır ve bunu bizim görünüm şablonu model nesnesi olarak geçirir:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Biz koşullu Göster/Düzenle ve Sil bağlantıları yardımcı yöntemi gibi aşağıda Dinner.IsHostedBy() kullanarak Gizle için bizim görünüm şablonu güncelleştirebilirsiniz:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Sonraki Adımlar

Şimdi nasıl RSVP AJAX kullanarak azalma için kimliği doğrulanmış kullanıcılara etkinleştirme sırasında bakalım.

>[!div class="step-by-step"]
[Önceki](implement-efficient-data-paging.md)
[sonraki](use-ajax-to-deliver-dynamic-updates.md)
