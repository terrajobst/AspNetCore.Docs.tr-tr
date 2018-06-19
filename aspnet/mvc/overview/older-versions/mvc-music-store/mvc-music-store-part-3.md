---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: '3. Kısım: Görünümleri ve ViewModels | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. 3. kısım, görünümler ve ViewModels kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 497c2898db2e03b0650982c3ad1e6b5ac5ca639d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874856"
---
<a name="part-3-views-and-viewmodels"></a>3. Kısım: Görünümleri ve ViewModels
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.  
>   
> MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. 3. kısım, görünümler ve ViewModels kapsar.


Şu ana kadar biz yalnızca dizeleri denetleyicisi eylemlerden döndürme. Denetleyicileri nasıl çalıştığı hakkında bir fikir edinmek için iyi bir yolu, ancak nasıl gerçek web uygulaması oluşturma istemezsiniz gelir. Biz geri sitemizi ziyaret tarayıcılara HTML oluşturmak için daha iyi bir yolu istediğiniz olacak – bir biz daha kolay HTML içeriğini özelleştirmek için şablon dosyalarını nereye kullanabilirsiniz gönderir geri. Tam olarak görünümleri ne olur.

## <a name="adding-a-view-template"></a>Şablonu görüntüleme ekleme

Bir görünüm şablonu kullanmak için biz ActionResult döndürmek için HomeController dizin yöntemini değiştirin ve sahip aşağıda gibi View(), dönüş:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

Bunun yerine bir sonuç yeniden oluşturmak için "Görünüm" kullanmak istiyoruz, yerine bir dize döndürdü yukarıdaki değişiklik gösterir.

Şimdi uygun bir görünüm şablon bizim projeye ekleyeceğiz. Bunu yapmak için şu metin imleci dizin eylem yönteminin içinde getirin sonra sağ tıklatın ve "Görünümü Ekle" seçin. Bu Görünüm Ekle iletişim kutusunu getirir:

![](mvc-music-store-part-3/_static/image1.jpg)![](mvc-music-store-part-3/_static/image1.png)

"Görünüm Ekle" iletişim kutusu bize hızla ve kolayca görünüm şablon dosyalarını oluşturmak sağlar. Varsayılan olarak "Görünümü Ekle" iletişim kullanacağı eylem yönteminin eşleşen oluşturmak üzere Görünüm şablonu adı önceden doldurur. Yukarıdaki "Görünüm Ekle" iletişim kutusu, varsayılan olarak önceden doldurulmuş Görünüm adı olarak "Dizin" sahip, "Görünüm Ekle" bağlam menüsü bizim HomeController İNDİS() eylem yöntemi içinde kullandık olduğundan. Gerekli değil Bu iletişim kutusundaki seçeneklerden herhangi birini değiştirmek için bu nedenle Ekle düğmesini tıklatın.

Biz Ekle düğmesine tıkladığınızda Visual Web Developer \Views\Home dizininde durumlarda klasörü oluşturmak için bize görünüm şablonu önceden var olmayan bir yeni Index.cshtml oluşturun.

![](mvc-music-store-part-3/_static/image2.png)

"Index.cshtml" dosya adı ve klasör konumunu önemlidir ve varsayılan ASP.NET MVC adlandırma kuralları izler. Dizin adı, \Views\Home, HomeController adlı denetleyicisi - eşleşir. Görünüm şablonu adı, dizin, görünümün görüntüleme denetleyici eylem yöntemine eşleşir.

ASP.NET MVC bize bir görünüme dönmek için bu adlandırma kuralını kullanıyoruz, ad veya konum bir görünüm şablonu açıkça belirtmek zorunda kalmamak sağlar. Biz bizim HomeController içinde aşağıdaki gibi kodu yazarken \Views\Home\Index.cshtml görünüm şablon varsayılan olarak oluşturulur:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer oluşturulan ve şablonu görüntüle "Index.cshtml" biz "Görünümü Ekle" iletişim içinde "Ekle" düğmesini tıklattıktan sonra açılan. Index.cshtml içeriğini aşağıda verilmiştir.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Bu görünüm, ASP.NET Web Forms ve ASP.NET MVC önceki sürümlerinde kullanılan Web Forms görünüm altyapısı daha kısa Razor sözdizimi kullanıyor. Web Forms görünüm altyapısı ASP.NET MVC 3'te hala kullanılabilir, ancak Razor görüntüleme altyapısı ASP.NET MVC geliştirme gerçekten iyi uyduğunu geliştiricilerin çoğu bulma.

İlk üç satırını ViewBag.Title kullanarak sayfa başlığını ayarla. Biz bu daha ayrıntılı olarak yakında, ancak ilk şimdi güncelleştirmenin nasıl çalıştığı metin başlık metni ara ve sayfasını görüntüleyin. Güncelleştirme &lt;h2&gt; etiketi "Bu olduğu giriş sayfası" aşağıda gösterildiği gibi söyleyin.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Bizim yeni metin giriş sayfasında görünür olduğunu uygulama gösterir çalışıyor.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Ortak site öğeler için bir düzen kullanarak

Çoğu Web siteleri, birçok sayfalar arasında paylaşılan içeriğe sahip: gezinme, altbilgi, logo görüntüler, stil sayfası başvuruları, vb. Razor görüntüleme altyapısı bu adlı sayfaya kullanarak yönetmenizi kolaylaştırır \_/ görünümler/paylaşılan klasöre bize için otomatik olarak oluşturuldu Layout.cshtml.

![](mvc-music-store-part-3/_static/image4.png)

Bu klasör, aşağıda gösterilen içeriğini görüntülemek için çift tıklayın.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

İçeriği bizim tek bir görünüm tarafından görüntülenen @RenderBody() komut ve dışında görünen istiyoruz herhangi bir ortak içerik eklenebilir \_Layout.cshtml biçimlendirme. Biz, doğrudan yukarıdaki şablon ekleyeceğiz bağlantılar sahip ortak bir üstbilgi bizim giriş sayfasının ve depolama alanına sitesindeki tüm sayfalara vardır, yani bizim MVC müzik deposu isteyeceksiniz @RenderBody() ifadesi.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Stil güncelleştiriliyor

Boş proje şablonu yalnızca doğrulama iletileri görüntülemek için kullanılan stiller içeren çok kolaylaştırılmış bir CSS dosyası içerir. Bizim Tasarımcısı bazı ek CSS ve görüntülerin görünüm için sitemizi, şimdi de ekleyeceğiz şekilde tanımlamak için sağlamıştır.

Güncelleştirilmiş CSS dosya ve görüntüleri kullanılabilir olan MvcMusicStore Assets.zip içerik dizininin içinde yer alan [MVC müzik deposu](https://github.com/evilDave/MVC-Music-Store). Biz Windows Gezgini'nde ikisini birden seçin ve aşağıda gösterildiği gibi Visual Web Developer, içerik klasörüne bizim çözümün bırakın:

![](mvc-music-store-part-3/_static/image5.png)

Mevcut Site.css dosyanın üzerine yazmak isteyip istemediğinizi onaylamanız istenir. Evet'i tıklatın.

![](mvc-music-store-part-3/_static/image6.png)

İçerik klasörünü, uygulamanızın artık şu şekilde görünür:

![](mvc-music-store-part-3/_static/image7.png)

Şimdi şimdi uygulamayı çalıştırın ve değişikliklerimizi giriş sayfasında nasıl bakmak.

![](mvc-music-store-part-3/_static/image8.png)

- Şimdi Değiştirilenler gözden geçirin: HomeController'ın dizin eylem yöntemi bulunamadı ve standart bir adlandırma kuralına bizim görünüm şablonu ve ardından olduğundan kodumuza "dönüş View()" olarak adlandırılan olsa bile \Views\Home\Index.cshtmlView şablonu görüntülenir.
- Giriş sayfası \Views\Home\Index.cshtml görünüm şablonda tanımlı basit bir Hoş Geldiniz iletisi görüntülüyor.
- Giriş sayfasını kullanarak bizim \_Layout.cshtml şablonu ve Hoş Geldiniz iletisi standart site HTML düzenini içinde yer alır.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Bir Model bilgileri bizim görünüme iletmek için kullanma

Yalnızca sabit kodlanmış HTML görüntüleyen bir görünüm şablonu çok ilginç bir web sitesi kolaylaştıracağını değil. Dinamik bir web sitesi oluşturmak için bunun yerine görünümü şablonlarımız bizim denetleyicisi eylemlerden bilgi geçirmek istiyoruz.

Model-View-Controller desende uygulamasındaki verileri temsil eden modeli başvurduğu terim nesneleri. Genellikle, model nesneleri veritabanınızdaki tablolar karşılık gelir, ancak zorunda değilsiniz.

ActionResult döndürmesi denetleyici eylem yöntemlerine model nesnesi görünümüne geçiş yapabilir. Bu, uygun HTML yanıtı oluşturmak için kullanılacak bir yanıt oluşturmak ve bu bilgileri bir görünüm şablonu sonra geçirin için gerekli tüm bilgileri düzgün bir şekilde paketi bir denetleyiciye sağlar. Bu eylem görerek anlamak, başlayabiliriz en kolayıdır.

Türler ve albümleri bizim deposu içinde temsil etmek için bazı modeli sınıfları ilk oluşturacağız. Bir tarzını sınıfı oluşturarak başlayalım. Projenizin içinde "Modelleri" klasörü sağ tıklatın, "Sınıfı Ekle" seçeneğini seçin ve "Genre.cs" dosya adı.

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Daha sonra ortak dizesi Name özelliği, oluşturulan sınıfa ekleyin:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Not: durumda merak ediyor, {alın; ayarlayın;} gösterimi yapma C# ' ın otomatik uygulanan özellikler özelliğini kullanın. Bu işlem bize bize yedekleme alanı bildirmek gerek kalmadan bir özellik yararları sağlar.*

Ardından, bir başlığı ve bir tarzını özelliğine sahiptir (Album.cs adlı) bir albüm sınıfı oluşturmak için aynı adımları izleyin:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Şimdi biz bizim modelden dinamik bilgilerini gösteren görünümleri kullanmak için StoreController değiştirebilirsiniz. İstek Kimliği temel alarak bizim albümleri, biz şimdi - Tanıtım amaçlı adlı - varsa, biz görünüm olduğu gibi bilgileri görüntüleyebilirsiniz.

![](mvc-music-store-part-3/_static/image10.png)

Tek bir albümü bilgileri gösterecek şekilde deposu ayrıntıları eylem değiştirerek başlayacağız. En üst kısmına "kullanarak" deyimine **StoreControllers** biz albüm sınıfını kullanmak istiyoruz her zaman MvcMusicStore.Models.Album yazın gerek kalmaması MvcMusicStore.Models ad alanı içerecek şekilde sınıfı. Bu sınıf "kullanımları" bölümünü artık görünmelidir olarak aşağıda.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Böylece bir dize yerine ActionResult döndürür HomeController'ın dizin yöntemiyle yaptığımız gibi ardından, Ayrıntılar denetleyici eylemi güncelleştiriyoruz.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Şimdi biz albüm nesneyi görünümüne dönmek için mantığı değiştirebilirsiniz. Daha sonra Bu öğreticide biz verilerini veritabanından – alma ancak için şu anda "veri kukla" kullanacağız başlamak için.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Not: C# ile tanınmayan varsa var kullanarak bizim albüm değişkeni geç bağlama olduğu anlamına gelir varsayabilir. Doğru değil-C# derleyici tür çıkarımı ne Biz bu albüm albüm türünde belirlemek için değişkeni atama ve böylece biz derleme zamanı denetleme ve Visual Studio kod düzenleyicisini elde yerel albüm değişkeni bir albüm tür olarak derleme göre kullanma destekler.*

Şimdi bir HTML yanıtı oluşturmak için bizim albüm kullanan bir görünüm şablonu oluşturalım. Bunu yapmadan Görünüm Ekle iletişim kutusunda, yeni oluşturulan albüm sınıfı hakkında bilir böylece proje oluşturmamız gereken. Proje Debug⇨Build MvcMusicStore seçerek menü öğesi oluşturabileceğiniz (ek kredi için Ctrl-Shift-B kısayolu projeyi oluşturmak için kullanabileceğiniz).

![](mvc-music-store-part-3/_static/image11.png)

Biz bizim destekleyen sınıfları ayarladınız, biz bizim görünüm şablonu oluşturmaya hazırsınız. Ayrıntılar yöntemi içinde sağ tıklatın ve "Görünümü... Ekle" seçin bağlam menüsünden.

![](mvc-music-store-part-3/_static/image12.png)

Önce ile HomeController yaptığımız gibi yeni bir görünüm şablonu oluşturma olacak. StoreController oluşturuyoruz çünkü \Views\Store\Index.cshtml dosyasında varsayılan olarak oluşturulur.

Farklı olarak önce biz "Oluştur kesin türü belirtilmiş bir" görünümü onay alınacaktır. Biz, ardından "View veri sınıfı" açılır liste içinde bizim "Albümü" sınıf seçmek için çağıracaksınız. Bu nesne kullanmak için kendisine geçirilen bir albümü, bekliyor bir görünüm şablonu oluşturmak "Ekle" iletişim kutusunu görüntüle neden olur.

![](mvc-music-store-part-3/_static/image13.png)

Biz "Ekle" düğmesini tıklattığınızda, aşağıdaki kodu içeren bizim \Views\Store\Details.cshtml görünüm şablonu oluşturulur.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Bu görünüm kesin türü belirtilmiş-bizim albüm sınıfı gösterir ilk satırda dikkat edin. Razor görüntüleme altyapısı, böylece biz model özelliklerini kolayca erişmek ve IntelliSense avantajı Visual Web Developer düzenleyicisinde bile sahip, albüm nesneyi geçirildi olduğunu bilir.

Güncelleştirme &lt;h2&gt; gibi görünen üzere bu satırı değiştirerek albüm Title özelliğini gösterir şekilde etiketleyin.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Süre girdiğinizde, IntelliSense tetiklenir fark @Model anahtar sözcüğü, özellikleri ve albüm sınıfı destekler yöntemleri gösteren.

Şimdi şimdi Projemizin yeniden çalıştırın ve depolama/Ayrıntılar/5 URL'yi ziyaret edin. Aşağıdaki gibi bir albüm ayrıntılarını göreceğiz.

![](mvc-music-store-part-3/_static/image14.png)

Şimdi biz deposu Gözat eylem yöntemi için benzer bir güncelleştirme yapacağız. ActionResult döndürecek şekilde güncelleştirme yöntemi ve böylece yeni bir tarzını nesnesi oluşturur ve görünümüne döner yöntemi mantığını değiştirin.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Gözat yönteminde sağ tıklatın ve "Görünümü... Ekle" seçin bağlam menüsünden ardından kesin türü belirtilmiş bir görünüm ekleyin kesin türü belirtilmiş bir tarzını sınıfına ekleyin.

![](mvc-music-store-part-3/_static/image15.png)

Güncelleştirme &lt;h2&gt; görünümünde öğe Tarz bilgileri görüntülemek için (/Views/Store/Browse.cshtml içinde) kod.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Şimdi şimdi Projemizin yeniden çalıştırın ve Gözat / deposu/için Gözat? Tarz = DISCO URL. Gibi aşağıda gösterilen gözatma sayfası göreceğiz.

![](mvc-music-store-part-3/_static/image16.png)

Son olarak, biraz daha karmaşık bir güncelleştirmeye olalım **depolama dizini** eylem yöntemi ve bizim deposunda tüm türler listesini görüntülemek için görünümü. Yalnızca tek bir tarzını yerine bizim model nesnesi, bir liste türler kullanarak bunu.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Depolama dizini eylem yöntemine sağ tıklatın ve eklemek önce Tarz Model sınıfı seçin ve Ekle düğmesine basın görünümü seçin.

![](mvc-music-store-part-3/_static/image17.png)

Biz öncelikle değiştireceğiz @model görünümü birkaç Tarz bekleniyor göstermek için bildirim yerine yalnızca bir tanesini nesneleri. Aşağıdaki şekilde /Store/Index.cshtml ilk satırı değiştirin:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Bu, birkaç Tarz nesneleri tutabilen bir model nesnesi ile çalışacaksınız Razor görüntüleme altyapısı bildirir. Bir IEnumerable kullanmakta olduğunuz&lt;Tarz&gt; listesini yerine&lt;Tarz&gt; daha genel olduğuna göre bize IEnumerable arabirimini destekleyen herhangi bir nesne türüne bizim model türü daha sonra değiştirmeye izin verme.

Ardından, biz tamamlanmış görünüm kodda gösterildiği gibi modeldeki Tarz nesneleri döngü.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Biz bu kodu girerken biz yazdığınızda böylece biz tam IntelliSense desteği özen gösterin "@Model." Biz, tüm yöntemleri ve özellikleri bir tarzını türü IEnumerable tarafından desteklenen bakın.

![](mvc-music-store-part-3/_static/image18.png)

Bizim "foreach" döngü içinde Visual Web Developer biz IntelliSence her biri için görmeleri her bir öğe türü Tarz, tarz türü olduğunu olduğunu bilir.

![](mvc-music-store-part-3/_static/image19.png)

Ardından, yapı iskelesi özelliği Tarz nesne incelenmesi ve aracılığıyla döngüler ve bunları Yazar her bir Name özelliğine sahip olma olasılığı belirledi. Ayrıca, her öğeyle düzenleme, Ayrıntılar ve Delete bağlantılar oluşturur. Biz, daha sonra Depolama Yöneticisi'nde yararlanmak, ancak şu an için basit bir listesi yerine olmasını isteriz.

Uygulamayı çalıştırın ve/Store için Gözat sayısını ve türler listesi görüntülenir bakın.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Sayfaları arasında bağlantılar ekleme

Türler şu anda listeler bizim/Store URL yalnızca düz metin olarak Tarz adlarını listeler. Düz metin yerine bunun yerine uygun deposu/Gözat URL Tarz adları bağlantısını sahibiz böylece bu "Disco" / deposu/için Gözat gideceği gibi bir müzik Tarz tıklayarak böylece değiştirelim? Tarz = DISCO URL. Bizim \Views\Store\Index.cshtml görünüm şablonu aşağıda kullanarak bu bağlantıları kod gibi çıktıya güncelleştiriyoruz **(Bu tür yok - üzerinde geliştirmek için yapacağız)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Çalışan, ancak bir sabit kodlanmış dize kullandığından daha sonra sorun için neden olabilir. Biz denetleyicisini yeniden adlandırmak istiyorsanız, örneğin, biz güncelleştirilmesi gereken bağlantıları için arayan kodumuza arama yapmak gerekir.

Bir HTML yardımcı yöntem yararlanmak için kullandığımız alternatif bir yaklaşım değil. ASP.NET MVC bizim görünüm şablonu kodundan çeşitli olduğu gibi genel görevleri gerçekleştirmek için kullanılabilir olan HTML yardımcı yöntemler içerir. Html.ActionLink() yardımcı yöntem özellikle yararlı bir ve HTML oluşturmanızı kolaylaştırır &lt;bir&gt; bağlar ve URL yollarını URL kodlanmış düzgünce emin olmak gibi rahatsız edici ayrıntıları ilgilenir.

Html.ActionLink() bağlantılarınız için gereksinim duyduğunuz kadar çok bilgi belirtme izin vermek için birçok farklı aşırı yüklemeye sahip. En basit durumda, yalnızca bağlantı metni ve köprü istemcide tıklatıldığında gitmek için bir eylem yönteminin sağlamanız. Örneğin, biz "/ deposu /" ile bağlantı metni "Gitmek için deposu aşağıdaki çağrıyı kullanarak dizini" deposu Ayrıntıları sayfasında İNDİS() yöntemi bağlayabilirsiniz:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Not: Bu durumda, biz yalnızca geçerli görünüm işlemeyle aynı denetleyicinin içinde başka bir eylem için bağlıyorsanız çünkü denetleyicinin adını belirtmek ihtiyacımız alamadık.*

Başka bir aşırı yüklemesini üç parametre alır Html.ActionLink yöntemi kullanacağız şekilde Gözat sayfa bizim bağlanan bir parametre, yine de geçirmek gerekir:

- 1. Bağlantı metnini, tarz adını görüntüler
- 2. Denetleyici eylem adı (Gözat)
- 3. Rota parametre değerleri belirtme (Tarz) adı ve değeri (Tarz adı),

Hepsini bir araya burada olduğunu Biz bu bağlantıları deposu dizini görünümüne nasıl yazacaksınız koyma:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Şimdi sizi Projemizin yeniden çalıştırdığınızda ve /Store/ URL'ye erişim biz türler listesini görürsünüz. Her bir tarzını – köprü tıklatıldığında olan, bize bizim/deposu/için Gözat sürer? Tarz =*[Tarz]* URL.

![](mvc-music-store-part-3/_static/image3.jpg)

HTML Tarz listesi için şu şekildedir:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Önceki](mvc-music-store-part-2.md)
> [sonraki](mvc-music-store-part-4.md)
