---
uid: mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
title: "Kullanın denetleyicileri ve görünümleri listeleme/Ayrıntılar UI uygulamak için | Microsoft Docs"
author: microsoft
description: "Adım 4 modelimizi kullanıcıların bir veri dökümü/ayrıntıları gezinme deneyimi sunmak için yararlanır uygulama için bir denetleyici eklemeyi gösterir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 64116e56-1c9a-4f07-8097-bb36cbb6e57f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-controllers-and-views-to-implement-a-listingdetails-ui
msc.type: authoredcontent
ms.openlocfilehash: 2f9148a2d419863229e2c5a2a0c98984001fcee5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="use-controllers-and-views-to-implement-a-listingdetails-ui"></a>Denetleyicileri ve görünümleri listeleme/Ayrıntılar UI uygulamak için kullanın
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 4. adımını ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.
> 
> 4. adım, kullanıcılara bir veri dökümü/ayrıntıları gezinme deneyimi azalma NerdDinner sitemizde sağlamak için modelimizi yararlanan uygulama denetleyici ekleme gösterilmektedir.
> 
> ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.


## <a name="nerddinner-step-4-controllers-and-views"></a>NerdDinner adım 4: Denetleyicileri ve görünümler

Geleneksel web çerçeveleri (klasik ASP, PHP, ASP.NET Web Forms, vb.), gelen URL'ler genellikle disk üzerindeki dosyalara eşlenir. Örneğin: bir URL için bir istek ister "/ Products.aspx" veya "/ Products.php" "Products.aspx" veya "Products.php" dosyası tarafından işlenen.

Web tabanlı MVC çerçeveleri URL'ler için sunucu kodu biraz farklı bir şekilde eşleyin. Gelen URL'ler için dosya eşleme yerine bunların yerine URL'leri sınıfları yöntemlere eşlenir. Bu sınıfların "Denetleyicileri" adı verilir ve kullanıcı girişini işleme gelen HTTP isteklerini işlemekten sorumlu, alma ve verilerini kaydetme ve gönderilecek yanıt belirleme geri istemciye (HTML görüntülemek, dosya indirme, farklı bir'yeniden yönlendirme URL, vb.).

Temel bir modeli NerdDinner uygulamamız için oluşturduğumuz, bizim sonraki adım bir denetleyici faydalanan bir veri dökümü/ayrıntıları gezinme deneyimi azalma sitemizde kullanıcılara sağlamak için uygulama eklemek için olacaktır.

### <a name="adding-a-dinnerscontroller-controller"></a>Bir DinnersController denetleyicisi ekleme

Biz bizim web projesi içinde "Denetleyicileri" klasörüne sağ tıklayarak başlayın ve ardından **Ekle -&gt;denetleyicisi** menü komutu (sizin de yürütebilir bu komut Ctrl-M, Ctrl-C yazarak):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image1.png)

Bu "Denetleyici Ekle" iletişim kutusunu getirir:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image2.png)

Biz "DinnersController" yeni denetleyici adı ve "Ekle" düğmesini tıklatın. Visual Studio sonra bizim \Controllers dizini altında DinnersController.cs dosya ekleyecek:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image3.png)

Kod Düzenleyicisi içindeki yeni DinnersController sınıf yukarı de açılır.

### <a name="adding-index-and-details-action-methods-to-the-dinnerscontroller-class"></a>İNDİS() ve Details() eylem yöntemleri DinnersController sınıfına ekleme

Yaklaşan azalma listesini bulun ve bunları belirli ayrıntılarını görmek için tüm Yemeği listesinde tıklayın izin vermek için uygulamamız kullanan ziyaretçilere etkinleştirmek istiyoruz. Biz, uygulamamız aşağıdaki URL'lerden yayımlayarak gerçekleştirirsiniz:

| **URL** | **Amaç** |
| --- | --- |
| */Dinners/* | Yaklaşan azalma HTML listesini görüntüle |
| */Dinners/Ayrıntılar / [kimlik]* | URL'de – veritabanındaki Yemeği DinnerID eşleşir katıştırılmış bir "id" parametresi tarafından belirtilen belirli bir Yemeği hakkındaki ayrıntıları görüntüler. Örneğin: /Dinners/Details/2 DinnerID değeri olan 2 Yemeği ayrıntılarını içeren bir HTML sayfası görüntülemek. |

Gibi bizim DinnersController sınıfına iki ortak "eylem yöntemleri" ekleyerek biz ilk uygulamalarında, bu URL'ler yayımlayacak:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample1.cs)]

Biz sonra NerdDinner uygulamayı çalıştırın ve bunları çağırmak için bizim tarayıcı kullanın. Yazarak *"/ azalma /"* URL neden olacak bizim *İNDİS()* çalıştırma ve yöntemine aşağıdaki yanıtı geri gönderir:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image4.png)

Yazarak *"/ Ayrıntılar/azalma/2"* URL neden olacak bizim *Details()* çalıştırın ve aşağıdaki yanıtı geri göndermek için yöntem:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image5.png)

Merak ediyor - ASP.NET MVC bizim DinnersController sınıf oluşturun ve bu yöntemi çağırmak için nasıl biliyor muydunuz? Şimdi anlamak için yönlendirmenin nasıl çalıştığını hızlı göz atın.

### <a name="understanding-aspnet-mvc-routing"></a>Anlama ASP.NET MVC yönlendirmesinin

ASP.NET MVC büyük bir URL'leri denetleyicisi sınıflarına nasıl eşlendiğini denetlenmesinde esneklik sağlayan güçlü bir yönlendirme URL'si altyapısını içerir. Bize nasıl oluşturulacağını, yanı sıra üzerinde çağırma değişkenleri otomatik olarak URL/Querystring Ayrıştırılan ve yönteme parametre olarak geçirilen, farklı şekilde yapılandırmak için hangi yöntemi hangi denetleyici sınıfı ASP.NET MVC seçer tamamen özelleştirmenizi sağlar bağımsız değişkenler. Tamamen bir site (arama motoru iyileştirme) SEO için en iyi duruma getirme yanı sıra bir uygulamadan istiyoruz herhangi bir URL yapısını yayımlama esnekliği sunar.

Varsayılan olarak, yeni ASP.NET MVC projeleri zaten kayıtlı URL yönlendirme kuralları önceden yapılandırılmış bir dizi birlikte gelir. Bu, bize açıkça herhangi bir şey yapılandırmak zorunda kalmadan bir uygulama kolayca başlamak sağlar. Varsayılan yönlendirme kuralı kayıtlar bizim projelerinin – sizi Projemizin kökünde "Global.asax" dosyasına çift tıklayarak açabilirsiniz "Uygulama" sınıfı içinde bulunabilir:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image6.png)

Varsayılan ASP.NET MVC yönlendirme kuralları bu sınıf "RegisterRoutes" yöntemi içinde kaydedilir:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample2.cs)]

"Yolları. MapRoute() "yöntem çağrısı yukarıdaki gelen URL'ler için URL biçimi kullanılarak denetleyicisi sınıfları eşleyen varsayılan yönlendirme kuralı kaydeder:" / {controller} / {action} / {id} "– örneği oluşturmak için denetleyici sınıfı adı"denetleyici"burada"eylem"adı olan bir Bunu ve "id" çağırmak için ortak yöntemi yöntemi için bağımsız değişken olarak geçirilebilir URL'de katıştırılmış isteğe bağlı bir parametredir. "MapRoute()" yöntem çağrısına geçirilen üçüncü parametre URL'de olmadıkları gerektiğinde, eylem/denetleyici/kimliği değerleri kullanmak için varsayılan değerler kümesidir (denetleyicisi "Home", Eylem = "Dizin", kimliği = = "").

URL'leri çeşitli nasıl gösteren bir tablo, varsayılan kullanılarak eşlendi aşağıdadır "*/ {denetleyicileri} / {action} / {id}"*yol kuralı:

| **URL** | **Denetleyici sınıfı** | **Eylem yöntemi** | **Geçirilen parametre** |
| --- | --- | --- | --- |
| */ Azalma/Ayrıntılar/2* | DinnersController | Details(id) | ID = 2 |
| */ Azalma/düzenleme/5* | DinnersController | Edit(id) | ID = 5 |
| */ Azalma/oluşturma* | DinnersController | Create() | Yok |
| */ Azalma* | DinnersController | İNDİS() | Yok |
| */ Giriş* | HomeController | İNDİS() | Yok |
| */* | HomeController | İNDİS() | Yok |

Varsayılan değerleri son üç satırları göster (denetleyicisi giriş, eylem = = dizini, kimliği = "") kullanılan. Bir belirtilmezse, "Dizin" yöntemi varsayılan eylem adı olarak kayıtlı olduğu için "/ azalma" ve bunların denetleyicisi sınıflarında çağrılacak "/ Home" URL'leri neden İNDİS() eylem yöntemi. Belirtilmezse, bir "Home" denetleyici varsayılan denetleyicisi kaydedildiği için "/" URL oluşturulacak HomeController ve İNDİS() eylem yönteminin çağrılması için neden olur.

Bu varsayılan URL yönlendirme kuralları istemeyiz, iyi haber Değiştir - yukarıdaki RegisterRoutes yöntemi içinde düzenleyebilir kolay olur. NerdDinner uygulamamız için yine de biz varsayılan URL yönlendirme kurallarını değiştirmek için giderek olmayan – yerine yalnızca olarak kullanacağız-değil.

### <a name="using-the-dinnerrepository-from-our-dinnerscontroller"></a>Bizim DinnersController gelen DinnerRepository kullanma

Şimdi DinnersController'ın geçerli bizim uyarlamasını şimdi değiştirmek modelimizi de kullanabilirsiniz uygulamalarıyla birlikte İNDİS() ve Details() eylem yöntemleri.

Daha önce davranışı uygulamak için oluşturduğumuz DinnerRepository sınıfı kullanacağız. Biz "NerdDinner.Models" ad alanı referanslarını bir "kullanarak" deyimi ekleyerek başlayın ve bizim DinnerController sınıfında'da bir alan bizim DinnerRepository örneği bildirin.

Bu bölümde daha sonra biz "Bağımlılık ekleme" kavramı getirir ve bizim DinnerRepository örneği yalnızca oluşturacağız artık test – ancak hakkını daha iyi birim sağlayan bir DinnerRepository bir başvuru almak bizim denetleyicileri için başka bir yol Göster Aşağıda satır içi gibi.

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample3.cs)]

Şimdi biz bizim alınan veriler model nesneleri geri kullanarak bir HTML yanıtı oluşturmak hazır olursunuz.

### <a name="using-views-with-our-controller"></a>Bizim denetleyicisiyle görünümleri kullanma

Bizim eylem yöntemleri HTML toplayın ve ardından içindeki kod yazmaya mümkün olmakla birlikte *Response.Write()* yaklaşım hızlı bir şekilde kullanılması oldukça zor hale istemciye geri göndermek için yardımcı yöntemi. Bize yalnızca uygulama ve veri mantığı bizim DinnersController eylem yöntemleri içinde gerçekleştirmek için ve HTML temsilini çıktısı için sorumlu ayrı "Görünüm" Şablon HTML yanıta işlemek için gereken verileri geçirmek için bir daha iyi bir yaklaşımdır içindir Bunu. Bir dakika içinde anlatıldığı gibi "görüntüle" şablonu genellikle HTML İşaretleme ve katıştırılmış işleme kodunun birleşimini içeren bir metin dosyasıdır.

Bizim Görünüm işleme denetleyicisi mantığımızı ayırarak, birden fazla büyük avantajları beraberinde getirir. Özellikle, bir açıkça "ayrılması sorunları" UI biçimlendirme işleme kod ve uygulama kodu arasında zorlanmasını sağlar. Bu birim testi uygulama mantığını yalıtımı için kullanıcı Arabirimi oluşturma mantığından kolaylaşır. Bu, daha sonra uygulama kodu değişiklik yapmak zorunda kalmadan UI işleme şablonları değiştirmek kolaylaştırır. Ve onu, geliştiricilerin ve tasarımcıların birlikte projelerde işbirliği yapmak için kolaylaştırabilir.

Biz biz dönüş türü "hükümsüz kılmak" yerine "ActionResult" bir dönüş türüne sahip olmaktan bizim iki eylem yöntemleri yöntemi imzalarını değiştirerek bir HTML UI yanıt geri gönderilecek bir görünüm şablonu kullanmak istediğinizi belirtmek için bizim DinnersController sınıfı güncelleştirebilirsiniz. Biz sonra çağırabilirsiniz *View()* yardımcı yöntem denetleyicisi temel sınıfındaki geri dönmek için "ViewResult" nesne gibi altında:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample4.cs)]

İmzası *View()* aşağıdaki gibi yukarıda kullandığınız biz yardımcı yöntemi arar:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image7.png)

İlk parametre olarak *View()* yardımcı yöntemdir istiyoruz HTML yanıtını işlemek için kullanılacak görünüm şablonu dosyasının adı. İkinci parametre şablonu görüntüleme HTML yanıtını işlemek için gereken verileri içeren bir model nesnesidir.

Bizim İNDİS() eylem yönteminin içinde biz aradığınız *View()* yardımcı yöntem ve gösteren bir "Dizin" Görünüm şablonu kullanarak azalma HTML listesini oluşturmak istiyoruz. Listeden oluşturmak için yemeği nesnelerinin bir dizisi şablonu görüntüleme geçiriyoruz:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample5.cs)]

Bizim Details() eylem yönteminin içinde şu URL içinde sağlanan kimliğini kullanarak Yemeği nesnesi alma denemesi. Geçerli Yemeği diyoruz bulunursa *View()* alınan Yemeği nesneyi işlemek için "Ayrıntılar" görünümü şablon kullanma istediğimizi belirten yardımcı yöntemi. Geçersiz bir Yemeği istediyseniz, biz "Bulunamadı" Görünüm şablonu kullanarak Yemeği yok belirten bir yararlı hata iletisi işleme (ve aşırı yüklenmiş bir sürümünü *View()* yalnızca şablon adı geçen yardımcı yöntemi ):

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample6.cs)]

Şimdi "Bulunamadı", "Ayrıntılar" ve "Dizin" Görünüm şablonları artık uygulayın.

### <a name="implementing-the-notfound-view-template"></a>"Bulunamadı" görünümü şablon uygulama

Biz, istenen Yemeği bulunamadığını belirten bir kolay hata iletisi görüntüler "Bulunamadı" Görünüm şablonu – uygulayarak başlarsınız.

Biz bizim metin imleci denetleyici eylem yöntemi içinde getirerek yeni bir görünüm şablonu oluşturun ve ardından sağ tıklayın ve (biz de bu komutun Ctrl-M, Ctrl-V yazarak yürütülüp) "Görünümü Ekle" menü komutu seçin:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image8.png)

Bu bir "Ekle" iletişim kutusunu görüntüle gibi getirir. İletişim kutusu (Bu durumda "Ayrıntılar") başlatıldığında iletişim oluşturmak için görünümün adını önceden doldurur varsayılan eylem yönteminin adı ile eşleşmesi için imleci içindeydi. Biz öncelikle "Bulunamadı" şablonu uygulamak istediğiniz için Biz bu görünüm adı geçersiz kılmak ve bunun yerine "Bulunamadı" olması için ayarlama:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image9.png)

Biz "Ekle" düğmesini tıklatın, Visual Studio yeni bir "NotFound.aspx" Görünüm şablonu bize (dizin zaten mevcut değilse, ayrıca oluşturacak olan) "\Views\Dinners" dizininde oluşturacak:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image10.png)

Ayrıca, Kod Düzenleyicisi içindeki yeni bizim "NotFound.aspx" Görünüm şablonu açar:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image11.png)

Görünüm şablonları varsayılan olarak "nerede içerik ve kod ekleyebiliriz iki içerik bölgeleri" sahiptir. Birinci bize "geri gönderilen başlığı" HTML sayfası özelleştirmenizi sağlar. İkinci bize "geri gönderilen ana"sayfasının içeriği HTML özelleştirmenizi sağlar.

Bizim "Bulunamadı" Görünüm şablonu uygulamak için bazı temel içerik ekleyeceğiz:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample7.aspx)]

Biz sonra bu tarayıcı içinden deneyebilirsiniz. Şimdi bunun için istek *"/ Ayrıntılar/azalma/9999"* URL. Bu veritabanında şu anda mevcut değil ve bizim "Bulunamadı" Görünüm şablonu oluşturmak bizim DinnersController.Details() eylem yöntemi neden olacak bir Yemeği başvurur:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image12.png)

Ekran görüntüsündeki fark edeceksiniz bir bizim temel görünüm şablonu bir grup ana ekran içeriğini çevreleyen HTML devralınan şeydir. Bizim görünüm şablonu bize tutarlı bir yerleşim sitesindeki tüm görünümleri uygulamak sağlayan bir "ana sayfa" şablonu kullanılarak olmasıdır. Bu öğreticinin sonraki bölümünde daha fazla iş nasıl ana sayfa ele alacağız.

### <a name="implementing-the-details-view-template"></a>"Ayrıntılar" görünümü şablon uygulama

Şimdi HTML için tek bir Yemeği modeli oluşturur "Ayrıntılar" Görünüm şablonu – şimdi uygulayın.

Biz bizim metin imleci ayrıntıları eylem yönteminin içinde getirerek bunu ve ardından sağ tıklatın ve "Görünümü Ekle" menü komutu seçin (veya Ctrl-M, Ctrl-V basın):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image13.png)

Bu, "Görünüm Ekle" iletişim kutusunu getirir. Biz, varsayılan görünüm adı ("Ayrıntılar") tutarsınız. Biz da iletişim kutusunda "Kesin türü belirtilmiş Görünüm Oluştur" onay kutusunu seçin ve (combobox açılan listeyi kullanarak) görünümüne denetleyicisinden geçiriyoruz model türünün adını seçin. Bu görünüm için bir Yemeği nesnesi geçiriyoruz (Bu tür için tam ad: "NerdDinner.Models.Dinner"):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image14.png)

Burada biz "Boş Görünüm" oluşturmayı seçerseniz, önceki şablon, bu süre için otomatik olarak seçeceğiz "" Ayrıntılar"şablonu kullanarak görünüm iskelesi". Biz "İçeriğini görüntüleme" açılan iletişim kutusunda yukarıdaki değiştirerek belirtebilirsiniz.

"İskele" kendisine geçiriyoruz Yemeği nesnesine dayalı bizim Ayrıntılar görünümü şablon başlangıç uygulaması oluşturur. Bu, bize bizim görünüm şablon uygulaması hızlı bir şekilde başlamak kolay bir yöntem sağlar.

Biz "Ekle" düğmesini tıklatın, Visual Studio yeni bir "Details.aspx" görünümü şablon dosyası bize bizim "\Views\Dinners" dizininde oluşturacak:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image15.png)

Kod Düzenleyicisi içindeki yeni bizim "Details.aspx" Görünüm şablonu da açılır. Yemeği modelini temel alan bir Ayrıntılar görünümü ilk iskele uygulaması içerecektir. Yapı iskelesi altyapısı kendisine geçirilen sınıfı üzerinde gösterilen ortak özelliklerine bakmak için .NET yansıma kullanır ve bulduğu her türüne göre uygun içerik ekler:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample8.aspx)]

Rica ederiz *"/ Ayrıntılar/azalma/1"* "Ayrıntılar" iskele uygulaması nasıl tarayıcıda göründüğünü görmek için URL. Bu URL'yi kullanarak biz ilk oluşturduğunuzda el ile Veritabanımıza eklediğimiz azalma birini görüntüler:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image16.png)

Bu bize hazır ve çalışır hızla alır ve bize bizim Details.aspx görünümüne ilk uygulaması ile sağlar. Biz sonra gidin ve onu bizim memnuniyet için kullanıcı arabirimini özelleştirmek için ince ayar.

Biz Details.aspx şablon daha yakından baktığınızda, biz bunu içeren statik HTML yanı sıra, işleme kod katıştırılmış bulabilirsiniz. &lt;%%&gt; kod nuggets şablonu görüntüleme işler, kod yürütmek ve &lt;% = %&gt; kod nuggets içerdiği kod yürütmek ve çıkış akışı şablonunun sonucu işlenemiyor.

Kesin türü belirtilmiş bir "Modeli" özelliğini kullanarak bizim denetleyicisinden geçildi "Yemeği" model nesnesi erişen bizim görünümündeki biz kod yazabilirsiniz. Visual Studio bize Düzenleyicisi içindeki "Model" Bu özelliğine erişirken tam kodu IntelliSense ile sağlar:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image17.png)

Aşağıdaki gibi bizim son Ayrıntılar görünümü şablonunun kaynağı tanıyarak bazı tweaks olalım:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample9.aspx)]

Biz eriştiğinizde *"/ Ayrıntılar/azalma/1"* URL yeniden içinde işleme ister aşağıda artık:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image18.png)

### <a name="implementing-the-index-view-template"></a>"Dizin" görünümü şablon uygulama

Şimdi yaklaşan azalma listesini oluşturur "Dizin" Görünüm şablonu – şimdi uygulayın. Bu sorundan bizim metin imleci dizin eylem yönteminin içinde yerleştirin ve ardından sağ tıklatın ve "Görünümü Ekle" menü komutu seçin (veya Ctrl-M, Ctrl-V tuşuna basın) Yapılacaklar.

"Görünümü Ekle" iletişim biz "Dizin" adlı görünüm şablonu tutmak ve "kesin türü belirtilmiş Görünüm Oluştur" onay kutusunu işaretleyin. Seçeneğini belirledik otomatik olarak bir "List" Görünüm şablonu oluşturmak ve model türü olarak "NerdDinner.Models.Dinner görünüme iletilen" seçmek için bu süre (hangi biz "iskele ki varsaymak Görünüm Ekle iletişim kutusu neden olacak bir listesi" oluşturma biz belirttiyseniz çünkü Yemeği nesnelerinin bir dizisi bizim denetleyicisinden görünümüne geçirme):

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image19.png)

Biz "Ekle" düğmesini tıklatın, Visual Studio yeni bir "Index.aspx" görünümü şablon dosyası bize bizim "\Views\Dinners" dizini içinde oluşturur. Bu "biz görünüme iletmek azalma HTML Tablo listesini sağlayan ilk uygulaması içindeki iskelesi".

Biz çalıştırdığınızda uygulama ve erişim *"/ azalma /"* azalma listemiz sokacak URL şu şekilde:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image20.png)

Yukarıdaki tabloda çözümü bize verilerimizi oldukça hangi Yemeği listeleme karşılıklı bizim tüketici için istiyoruz olmayan Yemeği – kılavuz benzeri bir düzende sunar. Biz Index.aspx görünüm şablonu güncelleştirin ve daha az veri sütunlarının listelemek ve kullanmak için değiştirmek bir &lt;ul&gt; aşağıdaki kodu kullanarak bir tablo yerine işlemek için:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample10.aspx)]

Biz bizim modelinde her Yemeği döngü olarak yukarıdaki foreach deyimi içindeki "var" anahtar sözcüğü kullanıyoruz. C# 3.0 ile tanınmayan olanlar "var" kullanarak Yemeği nesne geç bağlama olduğu anlamına gelir düşünebilirsiniz. Bunun yerine derleyici tür çıkarımı kesin türü belirtilmiş "Model" özelliği karşı kullandığını gösterir (türündeki "IEnumerable&lt;Yemeği&gt;") ve yerel "Yemeği" değişkeni biz tam alma anlamına gelir Yemeği türünde – derleme IntelliSense ve bunun için kod blokları içinde denetleme zamanı:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image21.png)

Ne zaman biz isabet yenileme */Dinners* bizim güncelleştirilmiş bir görünüm şimdi görülüyor aşağıda bizim tarayıcıda URL'si:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image22.png)

Bu, daha iyi – arıyor ancak tamamen henüz yok. Bizim son adım, tek tek azalma listesinde tıklatın ve bunlarla ilgili ayrıntıları görmek son kullanıcıların etkinleştirmektir. Biz bu bizim DinnersController ayrıntıları eylem yöntemine bağlama HTML Köprü öğeleri işleyerek uygulamanız.

Bizim dizini görünümde iki yoldan biriyle Biz bu köprüler oluşturabilirsiniz. İlk el ile HTML oluşturmaktır &lt;bir&gt; gibi öğeler burada biz katıştırmak aşağıda &lt;%%&gt; içinde engeller &lt;bir&gt; HTML öğesi:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image23.png)

Program aracılığıyla bir HTML oluşturma destekleyen ASP.NET MVC içindeki yerleşik "Html.ActionLink()" yardımcı yöntem yararlanmak için kullanabileceğiniz alternatif bir yaklaşım olan &lt;bir&gt; başka bir eylem yöntemine üzerinde öğesi bir Denetleyici:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample11.aspx)]

İlk parametre Html.ActionLink() yardımcı yöntemi olarak görüntülenecek bağlantı metni, (Bu durumda, Yemeği başlık), ikinci parametre istiyoruz (Bu durumda ayrıntıları yöntemi için) bağlantısı oluşturmak için denetleyici eylem adı ve üçüncü parametre (özellik adı/değerlerine sahip anonim bir tür olarak uygulanan) eylem göndermek için parametre kümesi. Bu durumda biz biz bağlamak istediğiniz ve varsayılan URL yönlendirme kuralı çünkü ASP.NET MVC uygulamasında Yemeği "id" parametresinin belirtiyorsanız "{Controller} / {Action} / {id}" Html.ActionLink() yardımcı yöntem şu çıkışı üretir:

[!code-html[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample12.html)]

Bizim Index.aspx görünümünü biz Html.ActionLink() yardımcı yöntem yaklaşımı kullanın ve her Yemeği uygun bilgileri URL listesi bağlantısı olması:

[!code-aspx[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample13.aspx)]

Ve ne zaman isabet biz şimdi */Dinners* URL aşağıdaki gibi bizim Yemeği listesi arar:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image24.png)

Biz listesinde azalma hiçbirini tıklattığınızda biz hakkındaki ayrıntıları görmek için gidin:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image25.png)

### <a name="convention-based-naming-and-the-views-directory-structure"></a>Kurala dayalı adlandırma ve \Views dizin yapısı

ASP.NET MVC uygulamaları varsayılan görünüm şablonları çözülürken adlandırma yapısı kurala dayalı bir dizin kullanın. Bu, tam olarak-konum yolu görünümlerinden denetleyici sınıfı içinde başvururken nitelemek zorunda kalmamak geliştiricilere sağlar. Varsayılan olarak ASP.NET MVC görünümü şablon dosyası içinde arar * \Views\[controllername öğesi]\* uygulamanın altında dizin.

Örneğin, biz açıkça üç görünüm şablonları başvuran DinnersController sınıfı üzerinde – çalışmakta: "Index", "Ayrıntılar" ve "Bulunamadı". ASP.NET MVC varsayılan arar bu görünümler içinde *\Views\Dinners* bizim uygulama kök dizini altında dizin:

![](use-controllers-and-views-to-implement-a-listingdetails-ui/_static/image26.png)

Yukarıdaki nasıl var. şu anda proje içinde üç denetleyicisi sınıfların olduğuna dikkat edin (DinnersController, HomeController ve AccountController – biz proje oluşturduğunuzda varsayılan olarak son iki eklenen), ve üç alt dizinleri (her bir vardır Denetleyici) \Views dizininde.

Giriş ve hesapları denetleyicilerinden başvurulan görünümler otomatik olarak kendi görünüm şablonlardan ilgili gidermek *\Views\Home* ve *\Views\Account* dizinleri. *\Views\Shared* alt dizini uygulama içinde birden çok genelinde yeniden kullanılan görünümü şablonlarını depolamak için bir yol sağlar. ASP.NET MVC şablonu görüntüleme çözümlemeye çalışırken, ilk içinde kontrol eder *\Views\[denetleyicisi]* belirli dizin ve görünüm şablonu bulamazsa var. içinde görüneceğini *\Views\ Paylaşılan* dizin.

Tek görünüm şablonları adlandırma için geldiğinde, önerilen yönerge işlemek neden eylem yöntemi olarak aynı adı paylaşan şablonu görüntüleme sağlamaktır. Örneğin, "Dizinimizi", Görünüm sonuç işlemek için "Dizin" görünümü eylem yöntemini kullanıyor ve "Ayrıntılar" eylem yönteminin sonuçlarını işlemek için "Ayrıntılar" görünümünü kullanıyor. Bu, hızlı bir şekilde hangi şablonu her eylemiyle ilişkili olduğunu görmek kolaylaştırır.

Geliştiriciler, şablonu görüntüleme denetleyicisinde çağrılan eylem yöntemi aynı ada sahip olduğunda görünüm şablonu adı açıkça belirtmek gerekmez. Biz bunun yerine yalnızca model nesnesi "View()" yardımcı yöntemine (Görünüm adı belirtmeden) geçirebilir ve ASP.NET MVC otomatik olarak Infer kullanılacak istiyoruz *\Views\[controllername öğesi]\[EylemAdı]* şablonu oluşturmak için disk üzerinde görüntüleme.

Bu denetleyici kodumuza biraz temizleyin ve kodumuza adlarında iki kez çoğaltılmasını önlemek için bize sağlar:

[!code-csharp[Main](use-controllers-and-views-to-implement-a-listingdetails-ui/samples/sample14.cs)]

İyi bir Yemeği dökümü/ayrıntıları uygulamak için gereken tüm site için deneyimi yukarıdaki kodudur.

#### <a name="next-step"></a>Sonraki adım

Şimdi yerleşik gezinirken iyi bir Yemeği sunuyoruz.

Şimdi CRUD (oluşturma, okuma, güncelleştirme, silme) veri form destek düzenleme şimdi etkinleştirin.

>[!div class="step-by-step"]
[Önceki](build-a-model-with-business-rule-validations.md)
[sonraki](provide-crud-create-read-update-delete-data-form-entry-support.md)
