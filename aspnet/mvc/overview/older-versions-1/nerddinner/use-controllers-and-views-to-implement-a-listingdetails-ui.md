---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: Bir listeleme/Ayrıntılar kullanıcı Arabirimi uygulama için denetleyicileri ve görünümleri kullanma | Microsoft Docs
author: microsoft
description: 4. adım, denetleyici modelimiz, kullanıcıların bir veri listeleme/Ayrıntılar Gezinti deneyimi sunmak için yararlanır uygulamaya ekleme işlemi gösterilmektedir...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 203a12473f79f38f7162d360d2179ca7c4a30303
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753588"
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Denetleyicileri ve görünümleri kullanarak listeleme/Ayrıntılar kullanıcı Arabirimi gerçekleştirmeyi
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Adım 4 / ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.
> 
> 4. adım, kullanıcıların bir veri listeleme/Ayrıntılar Gezinti deneyimi azalma NerdDinner sitemizde sağlamak için modelimizi faydalanan uygulama denetleyici ekleme işlemi gösterilmektedir.
> 
> ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner adım 4: Denetleyicileri ve görünümleri

Geleneksel web çerçeveleri ile (klasik ASP, PHP, ASP.NET Web Forms, vb.), genellikle dosyaları diskte gelen URL'ler eşlenir. Örneğin: bir URL isteği ister "/ Products.aspx" veya "/ Products.php" bir "Products.aspx" veya "Products.php" dosyası tarafından işlenir.

Web tabanlı MVC çerçeveleri URL'leri için sunucu kodu biraz farklı bir şekilde eşleştirin. Gelen URL'ler için dosya eşleme yerine bunların yerine URL'leri sınıfları yöntemlerde eşleyin. Bu sınıfların "Denetleyicileri" olarak adlandırılır ve kullanıcı girişini işleme gelen HTTP isteklerini işlemekten sorumlu oldukları, alma ve verilerini kaydetme ve gönderilecek yanıt belirleyen yedekleme istemciye (HTML görüntülemek, dosya indirme, farklı bir'yeniden yönlendirme URL, vb.).

NerdDinner uygulamamız için temel bir modeli derledik, denetleyici faydalanan kullanıcılar bir veri listeleme/Ayrıntılar Gezinti deneyimi azalma sitemizde sağlamak için uygulama eklemek için sonraki adımımız olacaktır.

### <a name="adding-a-dinnerscontroller-controller"></a>DinnersController denetleyici ekleme

Biz bizim web projesi içindeki "Denetleyicileri" klasörüne sağ tıklayarak başlayın ve ardından **Add -&gt;denetleyicisi** menü komutu (size de yürütebilir bu komut Ctrl-M, Ctrl-C yazarak):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Bu, "Denetleyici Ekle" iletişim kutusu getirir:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Biz "DinnersController" yeni denetleyici adı ve "Ekle" düğmesine tıklayın. Visual Studio, ardından bizim \Controllers dizini altında DinnersController.cs dosya ekleyecektir:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Ayrıca yukarı yeni DinnersController sınıf Kod Düzenleyicisi içinde açılır.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>İNDİS() ve Details() eylem yöntemleri DinnersController sınıfı ekleme

Yaklaşan azalma listesini inceleyin ve belirli ayrıntılarını görmek için tüm Akşam Yemeği listesinde tıklayarak izin vermek için uygulamamız kullanan ziyaretçilere sağlamak istiyoruz. Uygulamamızı aşağıdaki URL'lerden yayımlayarak bunu:

| **URL** | **Amaç** |
| --- | --- |
| */Dinners/* | Yaklaşan azalma bir HTML listesini görüntüleyin |
| */Dinners/Ayrıntılar / [ID]* | Akşam Yemeği veritabanında, DinnerID eşleşecektir URL içinde– katıştırılmış bir "id" parametresi tarafından belirtilen belirli bir Akşam Yemeği hakkında ayrıntıları görüntüler. Örneğin: /Dinners/Details/2 DinnerID değeri 2 olan Akşam Yemeği ayrıntılarını içeren bir HTML sayfası görüntüleme. |

Bu URL'ler ilk uygulamaları aşağıdaki gibi bizim DinnersController sınıfı iki genel "eylem yöntemleri" ekleyerek yayımlarız:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Ardından NerdDinner uygulamayı çalıştırın ve bizim tarayıcı bunları çağırmak için kullanırız. Yazarak *"/ azalma /"* URL neden bizim *İNDİS()* çalıştırma ve yönteme şu yanıtı geri gönderir:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Yazarak *"/ azalma/Ayrıntılar/2"* URL neden bizim *Details()* yöntemi çalıştırmak ve şu yanıtı geri göndermek için:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Merak ediyor - ASP.NET MVC bizim DinnersController sınıfı oluşturun ve bu yöntemleri çağırmak için nasıl biliyor muydunuz? Şimdi anlamak için nasıl yönlendirmenin çalıştığını hızlı göz atın.

### <a name="understanding-aspnet-mvc-routing"></a>Yönlendirmeyi anlama ASP.NET MVC

ASP.NET MVC denetleyici sınıflarına URL'lerin nasıl eşleştirildiğini denetleme esneklik birçok sağlayan güçlü bir yönlendirme URL'si altyapısını içerir. ASP.NET MVC hangi denetleyici sınıfı oluşturmak için hangi yöntemin yanı sıra üzerinde çağırmak için yapılandırma değişkenleri otomatik olarak URL/sorgu dizesi ayrıştırıldığında ve yönteme parametre olarak geçirilen, farklı yolu nasıl seçer tamamen özelleştirmek bize sağlar bağımsız değişkenler. Bu tamamen bir siteyi (arama motoru iyileştirme) SEO için en iyi duruma getirme yanı sıra bir uygulamadan istiyoruz herhangi bir URL yapısı yayımlama esnekliği sunar.

Varsayılan olarak, yeni ASP.NET MVC projeleri URL yönlendirme kuralları zaten kayıtlı önceden yapılandırılmış bir dizi birlikte gelir. Bu uygulama üzerinde herhangi bir şey yapılandırmak zorunda kalmadan kolayca kullanmaya başlamak sağlıyor. Varsayılan yönlendirme kuralı kayıtları, biz Projemizin kökünde "Global.asax" dosyasına çift tıklayarak açabileceğiniz – projelerimizden biri "Uygulama" sınıfı içinde bulunabilir:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Varsayılan ASP.NET MVC yönlendirme kuralları, bu sınıfın "RegisterRoutes" yöntemi kaydedilir:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

"Yolları. MapRoute() "yöntem çağrısının yukarıdaki gelen URL'ler için URL biçimi kullanılarak denetleyici sınıflarına eşleyen bir varsayılan yönlendirme kuralı kaydeder:" / {denetleyici} / {action} / {id} "– örneklemek için denetleyici sınıfının adı"denetleyici"burada"action"adıdır bir genel yöntem ve "id" çağrılacak yönteme bağımsız değişken olarak geçirilebilir URL içinde gömülü isteğe bağlı bir parametredir. "MapRoute()" yöntem çağrısına geçirilen üçüncü parametresi, URL'de olmadıkları durumunda, denetleyici/eylem/id değerler için kullanılacak varsayılan değerler kümesidir (denetleyici = "Home", Eylem = "Dizin", kimliği = "").

Aşağıda URL'leri çeşitli nasıl erişileceğini gösteren bir tablo varsayılan kullanılarak eşlendi. "<em>/ {denetleyicileri} / {eylem} / {id}"</em>yol kuralı:

| **URL** | **Denetleyici sınıfı** | **Eylem yöntemi** | **Geçirilen parametre** |
| --- | --- | --- | --- |
| */ Azalma/Ayrıntılar/2* | DinnersController | Details(id) | ID = 2 |
| */ Azalma/düzenleme/5* | DinnersController | Edit(id) | ID = 5 |
| */ Azalma/oluşturma* | DinnersController | Create() | Yok |
| */ Azalma* | DinnersController | İNDİS() | Yok |
| */ Giriş* | HomeController | İNDİS() | Yok |
| */* | HomeController | İNDİS() | Yok |

Varsayılan değerleri son üç satırları göster (denetleyicisi giriş, eylem = = dizini, kimliği = "") kullanılıyor. Bir belirtilmezse, "Index" yöntemi varsayılan eylem adı olarak kayıtlı olmadığından "/ azalma" ve bunların denetleyicisi sınıflarında çağrılacak "/ Home" URL'leri neden İNDİS() eylem yöntemi. Belirtilmezse, bir "Home" denetleyicisi varsayılan denetleyicisi olarak kayıtlı olduğundan, "/" URL oluşturulacak HomeController ve İNDİS() eylem yönteminin çağrılması için neden olur.

Bu varsayılan URL yönlendirme kuralları beğenmediniz, kolayca Değiştir - yukarıdaki RegisterRoutes yöntemi içinde düzenlemek için güzel bir haberimiz var demektir. NerdDinner uygulamamız için yine de, varsayılan URL yönlendirme kurallarını değiştirmek için kullanacağız olmayan: bunun yerine yalnızca olarak kullanacağız-olduğu.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Bizim DinnersController gelen DinnerRepository kullanma

Şimdi geçerli kararlılığımızın DinnersController'ın şimdi değiştirmek modelimizi kullanan uygulamaları ile İNDİS() ve Details() eylem yöntemleri.

Davranışı uygulamak daha önce oluşturulmuş DinnerRepository sınıfı kullanacağız. Biz "NerdDinner.Models" ad alanı başvuran bir "kullanarak" deyimi ekleyerek başlayın ve bizim DinnerRepository örneğini bizim DinnerController sınıfta bir alan olarak bildirmelisiniz.

Bu bölümde daha sonra biz "Bağımlılık ekleme" kavramını tanıtır ve yalnızca bizim DinnerRepository örneğini oluşturacağız artık bizim denetleyicileri daha iyi birim sağlayan bir DinnerRepository bir başvuru almak için başka bir şekilde test – ancak sağa Göster Aşağıdaki satır içi gibi.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Şimdi geri bizim alınan veri modeli nesneleri kullanarak bir HTML yanıtı oluşturmak hazırız.

### <a name="using-views-with-our-controller"></a>Denetleyici ile görünümleri kullanma

Bizim eylem yöntemleri HTML bir araya getirin ve ardından içinde kod yazmayı mümkün olmakla birlikte *Response.Write()* yaklaşım hızla oldukça zahmetli hale istemciye geri göndermek için yardımcı yöntemi. Yalnızca uygulama ve veri mantığı bizim DinnersController eylem yöntemleri içinde gerçekleştirin ve sonra HTML yanıtı HTML gösterimi çıktısı için sorumlu ayrı "Görünüm" şablonu işlemek için gerekli olan verileri geçirmek için bir daha iyi bir yaklaşımdır içindir Bunu. Bir dakika içinde anlatıldığı gibi "görüntüleme" şablon genellikle HTML İşaretleme ve kod katıştırılmış işleme bir birleşimini içeren bir metin dosyasıdır.

Bizim görünümü işleme bizim denetleyicisi mantıksal ayırma büyük avantaj getirir. Özellikle, bir "görev ayrımı nettir" uygulama kodu ve UI biçimlendirme işleme kodu arasında uygulanmasına yardımcı olur. Bu yalıtım halinde birim testi uygulama mantığı UI işleme mantığından kolaylaşır. Bu, daha sonra uygulama kodunu değişiklik yapmak zorunda kalmadan UI işleme şablonları değiştirme kolaylaştırır. Ve bu, geliştiricilerin ve tasarımcıların birlikte projeler üzerinde işbirliği yapmak için kolaylaştırabilir.

Biz size bizim iki eylem yöntemleri metodun metot imzalarını değiştirerek "void" yerine "ActionResult" dönüş türüne sahip dönüş türüne sahip bir HTML kullanıcı Arabirimi yanıt geri gönderilecek bir görünümü şablon kullanmak istediğinizi belirtmek için sunduğumuz DinnersController sınıfı güncelleştirebilirsiniz. Biz sonra çağırabilirsiniz *View()* yardımcı yöntem geri dönmek için denetleyici taban sınıfta bir "ViewResult" nesne gibi aşağıda:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

İmzası *View()* aşağıdaki gibi yardımcı yöntem yukarıda kullanıyoruz görünür:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

İlk parametre olarak *View()* yardımcı yöntemdir HTML yanıtını işlemek için kullanılacak istediğimiz görünümü şablon dosyasının adı. Şablonu Görüntüle HTML yanıtını işlemek için gereken verileri içeren bir model nesnesi ikinci parametredir.

Bizim İNDİS() eylem yöntemi içinde ki aradığınız *View()* yardımcı yöntem ve belirten bir HTML listeleyen bir "Index" Görünüm şablonu kullanarak bir azalma işlemek istiyoruz. Akşam Yemeği nesneleri listeden oluşturmak için bir dizi görünüm şablonu geçiriyoruz:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

Bizim Details() eylem yöntemi içinde biz URL içinde sağlanan kimliği kullanan bir Akşam Yemeği nesnesini alma denemesi. Geçerli bir Akşam Yemeği diyoruz bulunursa *View()* istiyoruz "Details" Görünüm şablonu alınan Dinner nesneyi işlemek için kullanılacağını belirten yardımcı yöntemi. Geçersiz bir Akşam Yemeği istenirse, biz Akşam Yemeği "Bulunamadı" görünümü şablon kullanarak yok gösteren bir yararlı hata mesajını oluştur (ve aşırı yüklenmiş bir sürümünü *View()* yalnızca şablon adı sürer yardımcı yöntemi ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Şimdi "Bulunamadı", "Details" ve "Index" Görünüm şablonları hemen uygulayın.

### <a name="implementing-the-notfound-view-template"></a>Uygulama "Bulunamadı" şablonu görüntüleme

Biz, istenen Akşam Yemeği bulunamadığını belirten bir kolay hata iletisi görüntüler "Bulunamadı" Görünüm şablonu – uygulayarak başlarsınız.

Size yeni bir görünüm şablonu bizim metin İmleç bir denetleyici eylem yöntemi içinde getirerek oluşturacak ve ardından sağ tıklayın ve (biz de bu komutu Ctrl-M, Ctrl-V yazarak yürütebilir) "Görünüm Ekle" menüsü komutu seçin:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Bu "Görünüm Ekle" iletişim gibi çıkarır. İletişim kutusu (Bu durumda "Details") başlatıldığında iletişim kutusunu oluşturma görünümünün adı önceden doldurulur varsayılan eylem yöntemi adını eşleştirmek için imleç içindeydi. İlk "Bulunamadı" şablonu uygulamak istediğimizden, biz bu görünüm adı geçersiz ve "Bulunamadı" yerine olacak şekilde ayarlamanız:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Biz "Ekle" düğmesine tıkladığınızda, Visual Studio yeni bir "NotFound.aspx" Görünüm şablonu bizim için (dizin zaten mevcut değilse de oluşturur) "\Views\Dinners" dizini içinde oluşturacak:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Ayrıca, sunduğumuz yeni "NotFound.aspx" Görünüm şablonu Kod Düzenleyicisi içinde açılır:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Görünüm şablonları varsayılan olarak "Burada içerik ve kodunun ekleyebiliriz iki içerik bölgeler" vardır. İlk "geri gönderilen başlığı" HTML sayfası özelleştirmenizi sağlar. İkinci "geri gönderilen ana içeriğiyle" HTML sayfası özelleştirmenizi sağlar.

Bizim "Bulunamadı" Görünüm şablonu uygulamak için bazı temel içerik ekleyeceğiz:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Biz daha sonra bu tarayıcı içinde deneyebilir. Bunu şimdi yapmak için istek *"/ azalma/Ayrıntılar/9999"* URL'si. Bu, şu anda veritabanında mevcut değil ve bizim "Bulunamadı" Görünüm şablonu işlemek sunduğumuz DinnersController.Details() eylem yönteminin neden olacak bir Yemeği başvurur:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Bizim temel görünüm şablonu bir sürü ana ekran içeriğini çevreleyen HTML devralmıştır ekran görüntüsündeki fark edeceksiniz çalışabilmenin tek yolu budur. Sitedeki tüm görünümleri arasında tutarlı bir düzen uygulama sağlıyor bir "ana sayfa" şablonu bizim görünüm şablonu kullanıyor olmasıdır. Nasıl iş bu öğreticinin sonraki bölümünde daha fazla ana sayfa açıklayacağız.

### <a name="implementing-the-details-view-template"></a>Uygulama "Details" şablonu görüntüleme

Şimdi HTML için tek bir Akşam Yemeği modeli oluşturacak "Details" Görünüm şablonu – hemen uygulayın.

Biz bizim metin imleç ayrıntıları eylem yöntemi içinde getirerek bunu ve ardından sağ tıklayın ve "Görünüm Ekle" menüsü komutu seçin (veya Ctrl-M, Ctrl-V'tuşuna basın):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Bu "Görünüm Ekle" iletişim kutusu getirir. Varsayılan Görünüm adı ("Details") saklayacağız. Biz da iletişim kutusunda "Kesin türü belirtilmiş Görünüm Oluştur" onay kutusunu seçin ve (combobox dropdown kullanarak) denetleyiciden görünüm geçiriyoruz model türünün adını seçin. Bu görünüm için bir Akşam Yemeği nesnesi geçiriyoruz (Bu tür için tam nitelikli ad: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

Burada "Boş Görünüm" oluşturmak için seçtik, önceki şablonu, bu süre için otomatik olarak seçeceğiz "bir"Ayrıntılı"şablonu kullanarak görünüm iskelesini". Biz, "Görünüm içeriği" açılan iletişim kutusunda yukarıdaki değiştirerek belirtebilirsiniz.

"İskele kurma özelliği" kendisine geçiriyoruz Dinner nesnesine dayalı bizim Ayrıntıları görünümü şablonunu ilk uygulaması oluşturur. Bu görünüm şablonu kararlılığımızın hızlıca başlamak kolay bir yol sağlar.

Biz "Ekle" düğmesine tıkladığınızda, Visual Studio yeni "Details.aspx" görünümü şablon dosyası bizim için sunduğumuz "\Views\Dinners" dizininde oluşturacak:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Ayrıca, sunduğumuz yeni "Details.aspx" Görünüm şablonu Kod Düzenleyicisi içinde açılır. Bunu Şimdi Akşam modelini temel alan bir Ayrıntılar görünümü ilk iskele uygulaması içerir. Yapı iskelesi altyapısı kendisine geçirilen sınıf üzerinde kullanıma sunulan ortak özellikleri bakmak için .NET, yansıtma kullanır ve bulduğu her türüne göre uygun içerik ekler:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Rica ederiz *"/ azalma/Ayrıntılar/1"* bu "ayrıntılı" iskele uygulama tarayıcıda nasıl göründüğünü görmek için URL. Bu URL'yi kullanarak size ilk oluşturduğunuzda bizim veritabanına el ile eklenmiştir azalma birini görüntüler:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Bu bize hızla çalışmaya alır ve bize bizim Details.aspx görünümüne ilk uygulaması ile sağlar. Biz sonra gidin ve bizim memnuniyet için kullanıcı arabirimini özelleştirmek için ince ayar.

Details.aspx şablon daha yakından baktığımızda, biz bunu içeren statik HTML yanı sıra, işleme kod gömülü bulabilirsiniz. &lt;%%&gt; kod nuggets görünüm şablonu işler, kod yürütün ve &lt;% = %&gt; kod nuggets içerdiği kod yürütün ve sonra şablon çıkış akışına sonucu işleme.

Biz, kesin türü belirtilmiş bir "Modeli" özelliğini kullanarak denetleyicimizin geçirildi "Akşam Yemeği" model nesnesi erişen bizim görünümü içinde kod yazabilirsiniz. Visual Studio bize tam kod IntelliSense ile "Model" Bu özellik Düzenleyici içindeki erişirken sağlar:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Aşağıdaki gibi bizim son Ayrıntılar görünümü şablon kaynağı aşağıdaki şekilde gözükmesi bazı tweaks olalım:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Biz eriştiğinizde *"/ azalma/Ayrıntılar/1"* URL yeniden olacak işleme ister aşağıda şimdi:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>Uygulama "Index" şablonu görüntüleme

Şimdi yaklaşan azalma listesini oluşturur "Index" Görünüm şablonu – hemen uygulayın. Yapılacaklar biz dizin eylem yönteminin içinde bizim metin imleci yerleştirin ve ardından sağ tıklayın ve "Görünüm Ekle" menüsü komutu seçin (veya Ctrl-M, Ctrl-V'tuşuna basın).

"Görünüm Ekle" iletişim kutusunun içinden biz "Index" adlı şablonu görüntüle tutun ve "kesin türü belirtilmiş Görünüm Oluştur" onay kutusunu işaretleyin. Biz seçim otomatik olarak bir "List" Görünüm şablonu oluşturmak ve model türü olarak "NerdDinner.Models.Dinner görünüme iletilen"'ı seçin. Bu süre (hangi biz size "iskele duyuyoruz varsaymak Görünüm Ekle iletişim kutusu neden olacak bir liste" oluşturmakta olduğunuz belirttiyseniz nedeni Akşam Yemeği nesnelerinin bir dizisi bizim denetleyiciden görünüm geçirme):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Biz "Ekle" düğmesine tıkladığınızda, Visual Studio yeni "Index.aspx" görünümü şablon dosyası bizim için sunduğumuz "\Views\Dinners" dizin içinde oluşturur. Bu "biz görünüme iletmek azalma HTML Tablo listesini sağlayan ilk uygulaması içindeki iskelesini".

Biz çalıştırıldığında uygulama ve erişim *"/ azalma /"* azalma listemizi oluşturmak URL şu şekilde:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

Yukarıdaki tabloda çözüm oldukça Dinner listeleme bizim tüketiciye için istediğimiz olmayan Dinner verilerimizi – kılavuz benzeri yerleşimini sağlıyor. Biz şablonu Index.aspx görüntüleme güncelleştirebilir ve veri daha az sütun listesinde ve kullanmak için değiştirmeniz bir &lt;ul&gt; aşağıdaki kodu kullanarak tablo yerine işlenecek öğe:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Modelimiz her Akşam Yemeği döngüye gireceğiz olarak yukarıdaki foreach deyimi içindeki "var" anahtar sözcüğü kullanılmıştır. C# 3.0 ile alışkın olanlar kullanarak "var" Akşam Yemeği nesne geç bağlanan anlamına düşünebilirsiniz. Bunun yerine derleyici tür çıkarımı, türü kesin belirlenmiş "Model" özelliği karşı kullandığını gösterir (türünde "IEnumerable&lt;Dinner&gt;") ve yerel "Akşam Yemeği" değişkeni tam aldığımız anlamına gelir Şimdi Akşam türünde – derleme IntelliSense ve kod blokları içinde teslim için derleme zamanı:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Biz isabet edildiğinde yenileme */Dinners* bizim güncelleştirilmiş görünüm şimdi göründüğüne aşağıda bizim tarayıcı URL:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Bu, daha iyi – görünüyor ancak tamamen henüz mevcut değildir. Son adımımız listesinde ayrı azalma tıklayın ve bunlarla ilgili ayrıntıları görmek son kullanıcılara etkinleştirmektir. Biz, bizim DinnersController ayrıntıları eylem yöntemine bağlama HTML Köprü öğeleri işleyerek uygulayacaksınız.

Bu köprüler şu iki yoldan biriyle bizim dizini görünümü içinde oluşturur. İlk HTML el ile oluşturmak için olan &lt;bir&gt; gibi öğeler burada biz ekleme aşağıda &lt;%%&gt; içinde engeller &lt;bir&gt; HTML öğesi:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Yerleşik "Html.ActionLink()" yardımcı yöntemi içinde program aracılığıyla bir HTML oluşturma destekleyen bir ASP.NET MVC yararlanmak için kullanabileceğiniz alternatif bir yaklaşım olan &lt;bir&gt; üzerinde başka bir eylem yöntemiyle bağlantılı öğe bir Denetleyici:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

Görüntülenecek bağlantı metni Html.ActionLink() yardımcı yöntemin ilk parametresi olan (Bu durumda, Şimdi Akşam başlığı), ikinci parametresi istiyoruz (Bu örnekte Ayrıntılar yöntemi için) bağlantısı oluşturmak için denetleyici eylem adı ve üçüncü parametre (özellik adı/değerlerini içeren bir anonim tür olarak uygulanmadı) eylem göndermek için parametre kümesi. Bu durumda biz size bağlamak istediğiniz ve ASP.NET MVC'de URL yönlendirmeyi varsayılan kural çünkü Akşam Yemeği "id" parametresinin belirtiyorsanız "{denetleyici} / {eylem} / {id}" Html.ActionLink() yardımcı yöntemi, aşağıdaki çıktıyı oluşturur:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Bizim Index.aspx görünümü için Html.ActionLink() yardımcı yöntemi yaklaşımı kullanır ve her Akşam Yemeği uygun detayları URL listesi bağlantısı olması:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Ve şimdi biz isabet edildiğinde */Dinners* aşağıdaki gibi dinner listemize görünen URL'si:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Biz herhangi bir azalma listesinde tıkladığınızda ayrıntılarını görmek giderek:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Kural tabanlı adlandırma ve \Views dizin yapısı

ASP.NET MVC uygulamaları varsayılan adlandırma yapısını görüntüleme şablonları çözülürken kural tabanlı bir dizin kullanın. Bu, geliştiricilerin tam olarak-konumu yolu bir denetleyici sınıfı içinde görünümleri başvururken nitelemek zorunda kalmamak olanak tanır. Varsayılan olarak ASP.NET MVC görünüm şablon dosyası içinde arar * \Views\[ControllerName]\* uygulama altında dizin.

Örneğin, biz açıkça üç görünüm şablonları başvuran DinnersController sınıf üzerinde – çalışmalar: "Index", "Details" ve "Bulunamadı". ASP.NET MVC varsayılan olarak arar bu görünümler içinde *\Views\Dinners* bizim uygulaması kök dizini altında dizin:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Yukarıda nasıl var. şu anda proje içinde üç denetleyici sınıflarına olduğuna dikkat edin (DinnersController ve HomeController AccountController – son iki biz projeyi oluşturduğunuzda varsayılan olarak eklenen), ve üç alt dizinleri (her biri vardır Denetleyici) \Views dizininden.

Giriş ve hesapları denetleyicilerinden başvurulan görünümlere otomatik olarak kendi görünüm şablonlardan ilgili Çözümle *\Views\Home* ve *\Views\Account* dizinleri. *\Views\Shared* alt dizini uygulama içinde birden çok genelinde yeniden kullanılacağını görünüm şablonları depolamak için bir yol sağlar. ASP.NET MVC görünüm şablonu çözümlemeye çalışırken, ilk içinde denetleyecek *\Views\[denetleyicisi]* belirli dizin ve görünüm şablonu bulamıyorsanız var. Bunun içinde görünür *\Views\ Paylaşılan* dizin.

Tek görünüm şablonları adlandırma için söz konusu olduğunda, önerilen yönerge işleme neden eylem yöntemi olarak aynı adı paylaşan görünüm şablonu sağlamaktır. Örneğin, "Dizinimizi", görünüm sonucu oluşturmak için "Index" görünümü eylem yöntemini kullanıyor ve "Details" eylem yönteminin sonuçlarını işlemek için "Ayrıntılar" görünümünü kullanıyor. Bu, hangi şablonun her eylemiyle ilişkili hızlıca görmek kolaylaştırır.

Geliştiriciler görünüm şablonu denetleyicisinde çağrılan eylem yönteminin aynı ada sahip olduğunda görünüm şablonu adı açıkça belirtmeniz gerekmez. Biz bunun yerine yalnızca model nesnesi "View()" yardımcı yöntemine (Görünüm adı belirtmeden) geçirebilirsiniz ve ASP.NET MVC otomatik olarak tanım Çıkarsama kullanılacak istiyoruz *\Views\[ControllerName]\[ActionName]* şablonu oluşturmak için disk üzerinde görüntüleme.

Bu denetleyici kodumuz biraz temizlemek ve kodumuz adlarında iki kez yeniden oluşturulması önlenebilir olanak tanır:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

Güzel bir Akşam Yemeği listeleme/Ayrıntılar uygulamak için gereken tüm site için deneyimi yukarıdaki kodudur.

#### <a name="next-step"></a>Sonraki adım

Göz atma deneyimini oluşturulan güzel bir Akşam Yemeği Şimdi sahibiz.

Haydi şimdi CRUD (oluşturma, okuma, güncelleştirme, silme) veri formu düzenleme desteği etkinleştirin.

> [!div class="step-by-step"]
> [Önceki](build-a-model-with-business-rule-validations.md)
> [İleri](provide-crud-create-read-update-delete-data-form-entry-support.md)
