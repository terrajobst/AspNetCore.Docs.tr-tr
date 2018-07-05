---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Kimlik doğrulama ve yetkilendirme kullanarak uygulamaların güvenliğini sağlama | Microsoft Docs
author: microsoft
description: 9. adım, böylece kullanıcılar kaydetmeniz gerekir. kimlik doğrulaması ve yetkilendirme NerdDinner uygulamamız güvenliğini sağlamak için ekleme gösterir ve site oluşturmak için oturum açın...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: d28102c8b80433b58a42cadc70b26c9fb5bc4404
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369890"
---
<a name="secure-applications-using-authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme kullanarak uygulamaların güvenliğini sağlama
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 9. adım bir ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.
> 
> 9. adım, böylece kullanıcılar kaydetmeniz gerekir. kimlik doğrulaması ve yetkilendirme NerdDinner uygulamamız güvenliğini sağlamak için ekleme gösterir ve yeni azalma ve yalnızca bir Akşam Yemeği barındırma kullanıcıyı oluşturmak için bir site için oturum açma düzenleyebilir, daha sonra.
> 
> ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner 9. adım: Kimlik doğrulaması ve yetkilendirme

Şu anda uygulamanın herkes vereceği bizim NerdDinner site oluşturma ve tüm Akşam Yemeği ayrıntılarını düzenleme olanağı ziyaret edin. Böylece kullanıcılar kaydetmeniz gerekir. Şimdi değiştirmek ve oturum açma sitesine yeni bir azalma oluşturup bir kısıtlama ekleyebilirsiniz, böylece yalnızca bir Akşam Yemeği barındırma kullanıcı daha sonra düzenleyebilirsiniz.

Bunu etkinleştirmek için kimlik doğrulama ve yetkilendirme uygulamamız güvenliğini sağlamak için kullanacağız.

### <a name="understanding-authentication-and-authorization"></a>Anlama kimlik doğrulaması ve yetkilendirme

*Kimlik doğrulaması* tanımlamak ve bir uygulamaya erişen bir istemci kimliğini doğrulama işlemi. Daha basit bir şekilde açıklamak, bu, "bir Web sitesini ziyaret ettiğinizde olan son kullanıcı" tanımlama hakkında olur. ASP.NET, tarayıcı kullanıcıların kimliğini doğrulamak için birden çok yöntemini destekler. Internet web uygulamaları için kullanılan en yaygın kimlik doğrulama yaklaşımı "Form kimlik doğrulaması" olarak adlandırılır. Form kimlik doğrulaması, bir HTML oturum açma formu, uygulamadaki yazar ve ardından bir veritabanı veya diğer parola kimlik bilgisi deposuna karşı bir son kullanıcı gönderen kullanıcı adı/parola doğrulamak bir geliştirici sağlar. Kullanıcı adı/parola birleşimi doğru ise, geliştirici ardından gelecek istekler genelinde kullanıcıyı tanımlamak için şifrelenmiş bir HTTP tanımlama vermek için ASP.NET sorabilirsiniz. NerdDinner uygulamamız ile forms kimlik doğrulaması kullanarak gerçekleştireceğiz.

*Yetkilendirme* kimliği doğrulanmış bir kullanıcı belirli bir URL/kaynağa veya bazı eylemleri gerçekleştirmek için izni olup olmadığını belirleme işlemidir. Örneğin, NerdDinner uygulamamız içinde biz yalnızca oturum açmış olan kullanıcılar erişebilir yetkilendirmek isteyeceksiniz */azalma/Create* URL'si ve yeni azalma oluşturun. Biz yalnızca bir Akşam Yemeği barındırma kullanıcı – düzenlemek ve diğer tüm kullanıcılar için düzenleme erişimi reddetme yetkilendirme mantığı eklemek isteyeceksiniz.

### <a name="forms-authentication-and-the-accountcontroller"></a>Form kimlik doğrulaması ve AccountController

ASP.NET MVC için varsayılan Visual Studio Proje şablonu, otomatik olarak yeni ASP.NET MVC uygulamaları oluştururken form kimlik doğrulamasını etkinleştirir. Bir site içinde güvenlik tümleştirme gerçekten kolay hale getirir projeye – önceden oluşturulmuş hesap oturum açma sayfasında uygulama da otomatik olarak ekler.

Ona erişen kullanıcı kimliği doğrulanmamış olduğunda varsayılan Site.master ana sayfanın sağ üst tarafında sitenin "Oturum Aç" bağlantısı görüntüler:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

"Oturum Aç" bağlantısına tıklayarak, bir kullanıcı alır */hesabı/oturum açma* URL'si:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Kayıtlı olmayabilirsiniz ziyaretçiler yapabilirsiniz kendisine sürer "Kaydet" bağlantısına – tıklayarak */hesabı/kaydı* URL'si ve bunları hesap ayrıntılarını girmek izin ver:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

"Kaydet" düğmesine tıklayarak ASP.NET üyelik sistemi içinde yeni bir kullanıcı oluşturun ve forms kimlik doğrulaması'nı kullanarak site üzerine bir kullanıcı kimlik doğrulaması.

Oturum açmış bir kullanıcı olduğunda, bir "Hoş Geldiniz [username]!" çıkış için sayfanın üst sağ Site.master değiştirir. ileti ve işleme bir "günlük birinde yerine" bir "Oturumu Kapat" bağlayın. "Oturumu Kapat" bağlantısını tıklatarak, geçerli kullanıcının oturumunu kapatır:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

Yukarıdaki oturum açma, oturum kapatma ve kayıt işlevi, projeyi oluşturduğunuzda, Visual Studio tarafından bizim projeye eklenen AccountController sınıfında uygulanır. AccountController için kullanıcı Arabirimi \Views\Account dizininden görünüm şablonları kullanarak uygulanır:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

AccountController sınıf ASP.NET formları kimlik doğrulamasını sistem şifrelenmiş kimlik doğrulaması tanımlama bilgileri ve depolamak ve kullanıcı adları/parolaları doğrulamak için ASP.NET üyelik API'si vermek için kullanır. ASP.NET üyelik API'si genişletilebilir ve kullanılacak bir parola kimlik bilgisi deposu sağlar. ASP.NET, kullanıcı adı/parola Active Directory veya bir SQL veritabanı içinde depolama yerleşik üyelik sağlayıcısını uygulamaları ile birlikte gelir.

NerdDinner uygulamamız projesinin kökünde "web.config" dosyasını açarak ve aramak kullanması gereken hangi üyelik sağlayıcısı yapılandırabiliriz &lt;üyelik&gt; içerdiği bölümü. Projeyi oluşturduğunuzda eklenen varsayılan web.config SQL üyelik sağlayıcısını kaydeder ve "ApplicationServices" adlı bir bağlantı dizesi kullanacak şekilde yapılandırır. veritabanı konumunu belirtmek için.

Varsayılan "ApplicationServices" bağlantı dizesini (içinde belirtilen &lt;connectionStrings&gt; web.config dosyasının) SQL Express'i kullanacak şekilde yapılandırılır. "ASPNETDB. adlandırılmış bir SQL Express veritabanına işaret MDF"uygulamanın altındaki" uygulama\_veri "dizin. Üyelik API'si uygulama içinde kullanılan ilk kez bu veritabanı yoksa, ASP.NET otomatik olarak veritabanı oluşturun ve uygun bir üyelik veritabanı şeması içindeki sağlayın:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Tam SQL Server örneği kullanmayı (veya uzak bir veritabanına bağlanmak için) istedik SQL Express kullanmak yerine, tüm yapılacaklar ihtiyacımız ise "ApplicationServices" bağlantı dizesini web.config dosyasında güncelleştirip emin uygun üyelik şeması adresindeki işaret veritabanına eklendi. Çalıştırabileceğiniz "aspnet\_regsql.exe" \Windows\Microsoft.NET\Framework\v2.0.50727\ dizininden içindeki uygun şemayı üyeliği ve diğer ASP.NET uygulama hizmetleri için bir veritabanına eklemek için yardımcı program.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Azalma/oluşturma [Authorize] filtresi kullanarak URL yetkilendirme

Güvenli kimlik doğrulaması ve hesap yönetim uygulaması NerdDinner uygulamanın etkinleştirmek için kod yazmadan yoktu. Kullanıcılar yeni hesaplar, uygulama ve sitenin oturum açma/oturum kapatma ile kaydedebilirsiniz.

Artık biz yetkilendirme mantıksal uygulamaya ekleyin ve bunlar neleri görebileceği ve neleri site içinde bunu yapamazsınız kontrol etmek için ziyaretçi kullanıcı adı ve kimlik doğrulama durumu kullanabilirsiniz. Bizim DinnersController sınıfı "Oluştur" eylem yöntemlerinin yetkilendirme mantığının ekleyerek başlayalım. Biz, özellikle gerektirecektir erişen kullanıcılar */azalma/Create* URL açmış olmanız gerekir. Bunlar oturum açmadıysanız, oturum açma böylece biz bunları oturum açma sayfasına yönlendir.

Bu mantıksal uygulama oldukça kolaydır. Yapılacaklar ihtiyacımız olan oluşturma eylem yöntemlerine [Authorize] filtresi öznitelik eklemek için şu şekilde:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC "için eylem yöntemleri bildirimli olarak uygulanabilir yeniden kullanılabilir mantığını uygulamak için kullanılabilir eylem filtreleri" oluşturma özelliğini destekler. ASP.NET MVC tarafından sağlanan yerleşik eylem filtreleri, [Authorize] filtresi biridir ve bildirimli olarak eylem yöntemleri ve denetleyici sınıflarına yetkilendirme kurallarını uygulamak bir geliştirici sağlar.

Herhangi bir parametre (yukarıda benzer) olmadan uygulandığında [Authorize] filtresi –, eylem yöntemi isteği yapan kullanıcının oturum açmanız gerekir ve yoksa onu otomatik olarak tarayıcı için oturum açma URL'sini yeniden zorlar. Bu yeniden yönlendirme başta istenen URL'ye sorgu dizesi bir bağımsız değişken olarak geçirilen yaparken (örneğin: / hesabı/oturum açma? ReturnUrl = % 2fDinners % 2fCreate). Kullanıcılar oturum açtıktan sonra AccountController kullanıcı daha sonra başta istenen URL'ye yeniden yönlendirir.

[Authorize] filtresi, isteğe bağlı olarak kullanıcı hem de ve izin verilen kullanıcıları veya izin verilen güvenlik rolüne üye listesi içinde kaydedildiğini zorunlu tutmak için kullanılan bir "Kullanıcılar" veya "Rolleri" özelliğini belirtme özelliğini destekler. Örneğin, aşağıdaki kod yalnızca iki belirli kullanıcı, "scottgu" ve "billg" azalma/Create URL'ye sağlar:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Ekleme kodu içinde belirli bir kullanıcı adları yine de düzgün hazırlanmamış sürdürülebilir olma eğilimindedir. Üst düzey "rolleri" tanımlamak için iyi bir yaklaşım olup, kod karşı denetler ve ardından bir veritabanı veya active directory sistemi (koddan harici olarak depolanması gerçek kullanıcı eşleme listesi etkinleştirme) kullanarak rol içine kullanıcıları eşleştirmek için. Yerleşik rol yönetim API'si ve bunun yanı sıra bu kullanıcı/rol eşlemeyi gerçekleştirmek yardımcı olabilecek rol sağlayıcıları (olanları SQL ve Active Directory dahil) yerleşik bir dizi ASP.NET içerir. Ardından, biz yalnızca azalma/Create URL'ye belirli bir "Yönetici" rolü içindeki kullanıcılar izin verecek kod güncelleştirebilir:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Oluştururken User.Identity.Name özelliğini kullanarak azalma

Biz denetleyicisi taban sınıfa kullanıma sunulan User.Identity.Name özelliğini kullanarak bir isteğin geçerli oturum açma kullanıcı adını alabilirsiniz.

Önceki size sunduğumuz Create() eylem yöntemini HTTP POST sürümünü uygulandığında sabit kodlanmış bir statik dize için Dinner "HostedBy" özelliği vardı. Biz bunun yerine User.Identity.Name özelliği kullanmak için bu kod şimdi güncelleştir, yapabilir bir RSVP Akşam Yemeği oluşturma konağı için otomatik olarak ekleyin:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Create() yöntemi [Authorize] özniteliği ekledik olduğundan, ASP.NET MVC azalma/Create URL'sini ziyaret kullanıcı sitede oturum açtıysa, eylem yönteminin yalnızca yürütüldüğünü sağlar. Bu nedenle, User.Identity.Name özellik değeri geçerli bir kullanıcı adı her zaman içerir.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Düzenleme yaparken User.Identity.Name özelliğini kullanarak azalma

Artık kullanıcılar yalnızca kendilerine barındırma azalma özelliklerini düzenleyebilmek sınırlayan yetkilendirme mantığa ekleyelim.

Bu konuda yardımcı olmak için önce bir "IsHostedBy(username)" yardımcı yöntem bizim Dinner nesnesine (içinde daha önce oluşturduğumuz Dinner.cs kısmi sınıf) ekleyeceğiz. True veya false olup belirtilen bir kullanıcı adı Dinner HostedBy özelliği eşleşen ve büyük küçük harf duyarlı dize karşılaştırması bunları gerçekleştirmek için gerekli mantığı kapsülleyen bağlı olarak bu yardımcı yöntemini döndürür:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

Ardından bir [Authorize] özniteliği Edit() eylem yöntemleri için sunduğumuz DinnersController sınıfında ekleyeceğiz. Kullanıcılar istek için oturum açmanız gerekir, bu sağlayacak bir */Dinners/düzenleme / [ID]* URL'si.

Kodu daha sonra oturum açmış kullanıcı Dinner konak eşleştiğinden emin olmak için Dinner.IsHostedBy(username) yardımcı yöntemini kullanan müşterilerimizin düzenleme metotlarını ekleyebilirsiniz. Kullanıcı konak değilse, biz "InvalidOwner" görüntülemek ve istek sonlandırın. Bunu yapmak için kod aşağıdaki gibi görünür:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Biz sonra \Views\Dinners dizinde sağ tıklayın ve Ekle - seçin&gt;görüntülemek yeni bir "InvalidOwner" görünümü oluşturmak için menü komutu. Biz ile dolduracaksınız aşağıdaki hata iletisi:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

Ve artık bir kullanıcı, sahip olmadığım bir Akşam Yemeği düzenleme girişiminde bulunduğunda, bunlar bir hata iletisi alırsınız:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Azalma da silin ve onu yalnızca ana bilgisayarının bir Akşam Yemeği silebilirsiniz olun izni kilitlemek için denetleyici içinde biz Delete() eylem yöntemleri için aynı adımları tekrarlayabilirsiniz.

### <a name="showinghiding-edit-and-delete-links"></a>Gösterme/gizleme Düzenle ve Sil bağlantılarını

Biz düzenleme ve silme eylem yöntemine bizim DinnersController sınıfın bizim ayrıntıları URL'den bağlantı:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Şu anda şu ayrıntıları URL ziyaretçi Akşam Yemeği konak olmasına bakılmaksızın düzenleme ve silme eylem bağlantıları gösteriliyor. Şimdi Akşam Yemeği sahibi ziyaret kullanıcı, bağlantıları yalnızca görüntülenen şekilde bu değiştirin.

Bizim DinnersController içinde Details() eylem yöntemi bir Akşam Yemeği nesnesi alır ve ardından model nesnesi bizim şablonu görüntüle geçer:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Biz, koşullu olarak göster/Düzenle ve Sil bağlantılarını yardımcı yöntem gibi aşağıda Dinner.IsHostedBy() kullanarak gizlemek için sunduğumuz görünüm şablonu güncelleştirebilirsiniz:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Sonraki Adımlar

Şimdi nasıl biz RSVP AJAX kullanarak azalma için kimliği doğrulanmış kullanıcılara etkinleştirebilirsiniz konumunda göz atalım.

> [!div class="step-by-step"]
> [Önceki](implement-efficient-data-paging.md)
> [İleri](use-ajax-to-deliver-dynamic-updates.md)
