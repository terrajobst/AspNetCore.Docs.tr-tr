---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
title: "3. Bölüm: Görünümler ve Viewmodel'lar | Microsoft Docs"
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. Bölüm 3, görünümler ve Viewmodel'lar kapsar.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: 94297aa0-1f2d-4d72-bbcb-63f64653e0c0
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-3
msc.type: authoredcontent
ms.openlocfilehash: 8fd89c2a448877bf13a7828f545ffcd400f63bb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837416"
---
<a name="part-3-views-and-viewmodels"></a>3. Bölüm: Görünümler ve Viewmodel'lar
====================
tarafından [Jon Galloway](https://github.com/jongalloway)

> MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.  
>   
> MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.  
>   
> Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. Bölüm 3, görünümler ve Viewmodel'lar kapsar.


Şu ana kadar biz yalnızca dizeleri denetleyici eylemlerine döndürmeyi. Denetleyicileri nasıl çalıştığı hakkında fikir almak için iyi bir yolu olan ancak olduğu nasıl gerçek bir web uygulaması derleme istemezsiniz. Daha iyi bir yolu geri sitemizi ziyaret tarayıcılara HTML oluşturmak istediğiniz kullanacağız: geri bir kolayca HTML içeriğini özelleştirmek için şablon dosyaları burada kullanabiliriz gönderin. Tam olarak neler görünümleri olmasıdır.

## <a name="adding-a-view-template"></a>Bir görünüm şablonu ekleme

Görünüm şablonu kullanmak için biz ActionResult döndürülecek HomeController dizin yöntemini değiştirin ve sahip aşağıdaki gibi View(), dönüş:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample1.cs)]

Yukarıdaki değişikliği yerine bir dize döndürdüğünü gösterir, bunun yerine bir sonucu geri üretmek için "Görünüm" kullanılacak istiyoruz.

Artık uygun bir şablonu görüntüleme için Projemizin ekleyeceğiz. Bunu yapmak için biz dizin eylem yöntemi içinde metin imleci konumlandırma sonra sağ tıklayın ve "Görünüm Ekle"'i seçin. Bu Görünüm Ekle iletişim kutusu getirir:

![](mvc-music-store-part-3/_static/image1.jpg)![](mvc-music-store-part-3/_static/image1.png)

"Görünüm Ekle" iletişim kutusu hızlı ve kolay bir görünümü şablon dosyaları oluşturmasını sağlıyor. Varsayılan olarak "Görünüm Ekle" iletişim kullanacak olan eylem yönteminin eşleşmesi oluşturmak için Görünüm şablonunun adı önceden doldurur. Bizim HomeController İNDİS() eylem yöntemi içindeki "Görünüm Ekle" bağlam menüsü kullandığımız için yukarıdaki "Görünüm Ekle" iletişim kutusu "Index" Görünüm adını varsayılan olarak önceden doldurulmuş olarak sahiptir. Gerekmez bu iletişim kutusundaki seçeneklerden herhangi birini değiştirmek için bu nedenle Ekle düğmesine tıklayın.

Biz Ekle düğmesine tıkladığınızda, Visual Web Developer \Views\Home dizininde değilse klasör oluşturma görünümü şablon bizim için zaten yoksa yeni bir Index.cshtml oluşturacaksınız.

![](mvc-music-store-part-3/_static/image2.png)

"Index.cshtml" dosya adı ve klasör konumunu önemlidir ve varsayılan ASP.NET MVC adlandırma kurallarını izler. Dizin adı, \Views\Home, HomeController adlı denetleyicisi - eşleşir. Görünüm şablonu adı, dizin, görünümün görüntüleme denetleyici eylem yöntemine eşleşir.

ASP.NET MVC görünüm döndürmek için bu adlandırma kuralı kullanıyoruz, adına veya konumuna bir görünüm şablonunun açıkça belirtmek zorunda kalmamak olanak sağlıyor. Size sunduğumuz HomeController içinde aşağıdaki gibi kod yazdığınızda, varsayılan olarak \Views\Home\Index.cshtml görünüm şablonu işlenir:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample2.cs)]

Visual Web Developer, oluşturulan ve size "Görünüm Ekle" iletişim kutusu içinde "Ekle" düğmesine tıkladı sonra "Index.cshtml" Görünüm şablonu açılır. Index.cshtml içeriğini aşağıda gösterilmektedir.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample3.cshtml)]

Bu görünüm, Web Forms, ASP.NET Web Forms ve ASP.NET MVC önceki sürümlerinde kullanılan görünüm altyapısını daha kısa olan Razor sözdizimi kullanıyor. Web Forms görünüm altyapısı ASP.NET MVC 3'te hala kullanılabilir, ancak Razor görüntüleme motorunu ASP.NET MVC geliştirme gerçekten iyi uyduğunu birçok geliştiricinin bulun.

İlk üç satırını ViewBag.Title kullanarak sayfa başlığını ayarlayın. Biz bunun daha ayrıntılı olarak yakında, ancak ilk şimdi güncelleştirme işleyişi sırasında Başlık metin ara ve sayfayı görüntülemek. Güncelleştirme &lt;h2&gt; etiketi aşağıda gösterildiği gibi "Bu olduğundan ana sayfa" deyin.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample4.cshtml)]

Sunduğumuz yeni metin giriş sayfasında görünür olduğunu uygulama programları çalışıyor.

![](mvc-music-store-part-3/_static/image3.png)

## <a name="using-a-layout-for-common-site-elements"></a>Sık kullanılan site öğeleri bir düzen kullanarak

Çoğu Web sitesi birçok sayfalar arasında paylaşılan içeriğe sahip: gezinti, altbilgiler, logosu görüntüler, stil sayfası başvuruları, vs. Razor görünüm altyapısı bu adlı bir sayfasını kullanarak yönetmenizi kolaylaştırır \_Layout.cshtml, / görünümler/paylaşılan klasörü içinde bizim için otomatik olarak oluşturuldu.

![](mvc-music-store-part-3/_static/image4.png)

Bu klasör ve aşağıda da gösterilen içeriği görüntülemek için çift tıklayın.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample5.cshtml)]

İçeriği bizim tek bir görünüm tarafından görüntülenen @RenderBody() komut ve dışında görünen istediğiniz herhangi bir ortak içerik eklenebilir \_Layout.cshtml biçimlendirme. Biz, şablona doğrudan yukarıdaki ekleyeceğiz şekilde bizim giriş sayfası ve Store alanına sitesindeki tüm sayfalara bağlantılarla birlikte ortak bir üst bilgi sağlamak için sunduğumuz MVC müzik Store isteyeceksiniz @RenderBody() deyimi.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample6.cshtml)]

## <a name="updating-the-stylesheet"></a>Stil sayfası güncelleştiriliyor

Boş proje şablonu, yalnızca doğrulama iletileri görüntülemek için kullanılan stilleri içeren çok kolaylaştırılmış bir CSS dosyası içerir. Bizim Tasarımcısı, bazı ek CSS ve görüntüleri görünümü için sitemizi, şimdi de ekleyeceğiz şekilde tanımlamak için sağlamıştır.

Güncelleştirilmiş CSS dosyası ve görüntü kullanılabilir olan MvcMusicStore Assets.zip içerik dizinin içinde yer [MVC müzik Store](https://github.com/evilDave/MVC-Music-Store). Biz ikisini birden Windows Gezgini'nde seçin ve aşağıda gösterildiği gibi bizim çözümün Visual Web Developer, içerik klasörüne bırakın:

![](mvc-music-store-part-3/_static/image5.png)

Mevcut Site.css dosyanın üzerine yazmak isteyip istemediğinizi onaylamanız istenir. Evet'e tıklayın.

![](mvc-music-store-part-3/_static/image6.png)

Uygulamanızın içerik klasörünü artık şu şekilde görünür:

![](mvc-music-store-part-3/_static/image7.png)

Şimdi uygulamayı çalıştırabilir ve yaptığımız değişiklikleri giriş sayfasında nasıl göründüğünü görmek şimdi.

![](mvc-music-store-part-3/_static/image8.png)

- Nelerin değiştiğini gözden geçirelim: HomeController'ın dizin eylem yöntemi bulunamadı ve bizim görünüm şablonu standart bir adlandırma kuralını izleyen çünkü kodumuz "dönüş View()" adlı olsa bile \Views\Home\Index.cshtmlView şablonu görüntülenir.
- Giriş sayfası \Views\Home\Index.cshtml görünüm şablonu içinde tanımlanan basit bir Hoş Geldiniz iletisi görüntülüyor.
- Giriş sayfasını kullanarak bizim \_Layout.cshtml şablonu ve HTML standart site düzenine Hoş Geldiniz iletisi bulunur.

## <a name="using-a-model-to-pass-information-to-our-view"></a>Bilgi bizim görünüme iletmek için bir modeli kullanma

Sabit kodlanmış HTML yalnızca görüntüleyen bir görünüm şablonu çok ilginç bir web sitesi yapacağını değil. Dinamik bir web sitesi oluşturun, biz bunun yerine görünümü şablonlarımızı denetleyicisi eylemlerimiz bilgi geçirmek isteyebilirsiniz.

Model-View-Controller desende, uygulamadaki verileri temsil modeli başvurduğu terimi nesneleri. Genellikle, model nesneleri, veritabanınızdaki tablolar karşılık gelir, ancak gerek yoktur.

ActionResult döndürmesi denetleyici eylem yöntemlerinde görünümü için bir model nesnesi geçirebilirsiniz. Bu, uygun HTML yanıtı oluşturmak için kullanılacak bir yanıt oluşturur ve ardından bir görünüm şablonu için bu bilgileri geçirin için gerekli tüm bilgileri indrebilirsiniz paketi bir denetleyiciye sağlar. Bu eylem görerek anlamak, başlayabiliriz en kolay yöntemdir.

Türleri ve albümleri mağazamız içinde temsil etmek için bazı Model sınıfları ilk oluşturacağız. Bir türe sınıfı oluşturarak başlayalım. Projeniz içindeki "Modelleri" klasörü sağ tıklatın, "Sınıf Ekle" seçeneğini belirleyin ve "Genre.cs" dosya adı.

![](mvc-music-store-part-3/_static/image2.jpg)

![](mvc-music-store-part-3/_static/image9.png)

Daha sonra oluşturulan sınıfa genel bir dize adı özelliği ekleyin:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample7.cs)]

*Not: durumda merak ediyor, {alın; ayarlayın;} gösterimi yapma, C# ' nin otomatik olarak uygulanan özellikler özelliğinin kullanımını. Bu işlem ABD bize destek alanı bildirmek gerek kalmadan özellik avantajlarını sağlar.*

Ardından, bir başlık ve bir türe özelliğine sahiptir (Album.cs adlı) bir albüm sınıfı oluşturmak için aynı adımları izleyin:

[!code-csharp[Main](mvc-music-store-part-3/samples/sample8.cs)]

Şimdi biz Modelimizi dinamik bilgileri gösteren görünümleri kullanmayı StoreController değiştirebilirsiniz. İstek Kimliği temel alarak bizim albümleri, biz şu anda - tanıtım amacıyla adlı - varsa, bu bilgileri aşağıdaki görünüm olduğu gibi görüntüleriz.

![](mvc-music-store-part-3/_static/image10.png)

Store Ayrıntılar eylemi için tek bir albümü bilgileri gösterecek şekilde değiştirerek başlayacağız. En üst kısmına "kullanarak" deyimine **StoreControllers** biz albüm sınıfını kullanmak istediğiniz her seferinde MvcMusicStore.Models.Album yazmak zorunda kalmazsınız MvcMusicStore.Models ad alanı içerecek şekilde sınıfı. Bu sınıfın "kullanımları" bölümü artık görünmesi gereken aşağıdaki gibi.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample9.cs)]

Böylece bir dize yerine ActionResult döndürür HomeController'ın dizin yöntemiyle yaptığımız gibi ardından, Ayrıntılar denetleyici eylemi güncelleştireceğiz.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample10.cs)]

Şimdi biz albüm nesnenin görünümüne dönmek için mantığı değiştirebilirsiniz. Bu öğreticinin ilerleyen bölümlerinde biz veriler bir veritabanından – alınıyor, ancak şu an için "veri kukla" kullanacağız kullanmaya başlamak için.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample11.cs)]

*Not: C# ile ilk kez kullanıyorsanız var kullanarak albüm değişkeni geç bağlanan anlamına varsayabilir. Doğru değil-C# derleyici tür çıkarımı ne biz albüm türüdür, albüm belirlemek için bir değişkene atayarak ve derleme zamanı denetimi ve Visual Studio Kod Düzenleyicisi aldığımız için yerel albüm değişkeni albüm türü olarak, derleme göre kullanıyor destekler.*

Artık bir HTML yanıtı oluşturmak için sunduğumuz albüm kullanan bir görünüm şablonu oluşturalım. Bunu yapmadan önce projeyi derleyin, Görünüm Ekle iletişim kutusunda yeni oluşturulan bizim albüm sınıfı hakkında bilebilmesi gerekir. Proje Debug⇨Build MvcMusicStore seçerek menü öğesi oluşturabileceğinizi (götürmek için Ctrl-Shift-B kısayolu Projeyi derlemek için kullanabileceğiniz).

![](mvc-music-store-part-3/_static/image11.png)

Size sunduğumuz destekleyen sınıfları ayarladınız, bizim görünüm şablonu oluşturmak hazırız. Ayrıntılar yöntemi içinde sağ tıklayın ve bağlam menüsünden "Görünümü Ekle..."'i seçin.

![](mvc-music-store-part-3/_static/image12.png)

Önce HomeController ile yaptığımız gibi yeni bir görünüm şablonu oluşturmak için kullanacağız. StoreController oluşturuyoruz çünkü \Views\Store\Index.cshtml dosyasında varsayılan olarak oluşturulur.

Aksine, önce "Oluşturma türü kesin belirlenmiş bir" görünümü onay kullanacağız. Ardından, "Albümü" sınıfı "Görünüm veri class" açılır liste içinde kullanacağız. Bu, bekliyor bir görünüm şablonu kullanmak için nesne geçirilir albüm oluşturmak "Görünüm Ekle" iletişim neden olur.

![](mvc-music-store-part-3/_static/image13.png)

Biz "Ekle" düğmesine tıkladığınızda, aşağıdaki kodu içeren bizim \Views\Store\Details.cshtml görünüm şablonu oluşturulur.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample12.cshtml)]

Bu görünüm, bizim albüm sınıfı için kesin olduğunu gösteren ilk satırda dikkat edin. Razor görüntüleme motorunu, biz model özelliklerini kolayca erişin ve hatta Visual Web Developer düzenleyicisindeki IntelliSense avantajı vardır, albüm nesneyi geçirildi olduğunu anlar.

Güncelleştirme &lt;h2&gt; bu satırı aşağıdaki gibi görünen değiştirerek albüm başlık özelliğini görüntüleyecek şekilde etiketleyin.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample13.cshtml)]

Sonra nokta girdiğinizde, IntelliSense tetiklenen fark @Model anahtar sözcüğü, özellikleri ve yöntemleri albüm sınıfı desteklediği gösteriliyor.

Şimdi artık Projemizin yeniden çalıştırın ve 5/Ayrıntılar/Store URL'sini ziyaret edin. Gibi bir albümü ayrıntılarını göreceğiz.

![](mvc-music-store-part-3/_static/image14.png)

Artık Store Gözat eylem yöntemine benzer bir güncelleştirme oluşturacağız. ActionResult döndürecek şekilde güncelleştirme yöntemi ve yeni bir türe nesnesi oluşturur ve görünümü döndürür yöntemi mantığını değiştirin.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample14.cs)]

Göz atma yönteminde sağ tıklayın ve bağlam menüsünden "Görünümü Ekle..." türü kesin belirlenmiş bir görünüm eklersiniz türü kesin belirlenmiş Tarz sınıfına ekleyin.

![](mvc-music-store-part-3/_static/image15.png)

Güncelleştirme &lt;h2&gt; öğesi Görünümü'nde (/Views/Store/Browse.cshtml içinde) Tarz bilgileri görüntülemek için kod.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample15.cshtml)]

Şimdi şimdi Projemizin yeniden çalıştırın ve Gözat / Store/dizinine göz atma? Tarz = DISCO URL. Aşağıda gösterilen gibi göz atma sayfasından göreceğiz.

![](mvc-music-store-part-3/_static/image16.png)

Son olarak, biraz daha karmaşık bir güncelleştirme olalım **Store dizini** eylem yöntemi ve bizim deposunda tüm türleri listesini görüntülemek için görünümü. Biz bu, bir liste türleri yalnızca tek bir türe yerine bizim model nesnesi kullanarak gerçekleştirirsiniz.

[!code-csharp[Main](mvc-music-store-part-3/samples/sample16.cs)]

Store dizini eylem yönteminde sağ tıklatın ve eklemek daha önce tarzı Model sınıfı seçin ve Ekle düğmesine basın görünümü seçin.

![](mvc-music-store-part-3/_static/image17.png)

İlk değiştireceğiz @model görünümü birkaç Tarz bekleniyor belirtmek için bildirimi yerine yalnızca bir tane nesneleri. Şu şekilde okunacak /Store/Index.cshtml ilk satırı değiştirin:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample17.cshtml)]

Bu, birkaç Tarz nesnesi tutabilen bir model nesnesi ile çalışacaksınız Razor görünüm altyapısı bildirir. Bir IEnumerable kullanıyoruz&lt;Tarz&gt; bir listesi yerine&lt;Tarz&gt; daha genel olduğundan, daha sonra model türü IEnumerable arabirimini destekleyen herhangi bir nesne türü değiştirmek bize verme.

Ardından, tamamlanmış görünüm kodda gösterildiği gibi modelinde Tarz nesneler aracılığıyla döngüye gireceğiz.

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample18.cshtml)]

Biz bu kodun girerken biz yazdığınızda emin ediyoruz tam IntelliSense desteği olduğunu fark edeceksiniz "@Model." tüm yöntemleri ve özellikleri Tarz türünde bir IEnumerable tarafından desteklenen görüyoruz.

![](mvc-music-store-part-3/_static/image18.png)

Bizim "foreach" döngü içinde IntelliSence her biri için görüyoruz her öğenin tür Tarz, tarz türü olduğundan, Visual Web Developer bilir.

![](mvc-music-store-part-3/_static/image19.png)

Ardından, iskele kurma özelliği Tarz nesne incelenir ve aracılığıyla döngüye girer ve bunları yazıyor her bir Name özelliğine sahip olacağını belirler. Ayrıca, her bir öğe Düzenle, Ayrıntılar ve Sil bağlantılarını oluşturur. Biz, daha sonra Depolama Yöneticisi'nde yararlanmak, ancak şu an için basit bir listesi yerine olmasını istiyoruz.

Uygulamayı çalıştırmak ve/Store için Gözat sayısı ve türleri listesi görüntülenir bakın.

![](mvc-music-store-part-3/_static/image20.png)

## <a name="adding-links-between-pages"></a>Sayfaları arasında bağlantılar ekleme

Türleri şu anda listeleyen/Store URL'nin yalnızca düz metin olarak Tarz adlarını listeler. Düz metin yerine biz bunun yerine ilgili Store/göz atma URL Tarz adları bağlantısını edinecek olmanızdır bu "Disco" Store/için Gözat gider gibi bir müzik Tarz tıklayarak böylece değiştirelim? Tarz = DISCO URL. Biz aşağıdaki gibi kullanarak bu bağlantıları kod çıktısına bizim \Views\Store\Index.cshtml görünüm şablonu güncelleştirebilir **(Bu tür yok - üzerinde geliştirmek için yapacağız)**:

[!code-html[Main](mvc-music-store-part-3/samples/sample19.html)]

Çalışan, ancak bunu daha sonra bir sabit kodlanmış dizesine kullandığından sorun neden olabilir. Örneği için denetleyici yeniden adlandırmak istedik, biz güncelleştirilmesi gereken bağlantıları için arayan kodumuz arama gerekecektir.

Bir HTML yardımcı yöntem yararlanmak için kullanabiliriz alternatif bir yaklaşım olan. ASP.NET MVC görünüm şablonu kodumuz çeşitli olduğu gibi ortak görevleri gerçekleştirmek için kullanılabilir olan HTML yardımcı yöntemler içerir. Html.ActionLink() yardımcı yöntemi, özellikle yararlı bir ve HTML oluşturmayı kolaylaştırır &lt;bir&gt; bağlar ve URL yolları URL kodlanmış düzgün olduğundan emin olmak gibi rahatsız edici ayrıntılarının üstlenir.

Html.ActionLink() bağlantılarınız için gereksinim duyduğunuz kadar çok bilgi belirtilmesine izin verecek şekilde birçok farklı aşırı yüklemeye sahip. En basit durumda, yalnızca bağlantı metni ve istemcide köprü tıklandığında gitmek için bir eylem yönteminin verin. Örneğin, biz "/ Store /" ile bağlantı metni "Gitmek için Store çağrısını kullanarak dizin" Store ayrıntıları sayfasındaki İNDİS() yöntemi bağlayabilirsiniz:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample20.cshtml)]

*Not: Bu durumda, biz size yalnızca geçerli görünüm işlemeyle aynı denetleyici içinde başka bir eylem bağlantı kurduğunuz çünkü Denetleyici adı belirtmeniz gerekmediğine.*

Başka bir aşırı üç parametre almayan Html.ActionLink yöntemin kullanacağız. Bu nedenle bizim bağlantılar göz atma sayfasından bir parametre, yine de iletmeniz gerekir:

- 1. Bağlantı metnini, tarzı adı görüntülenir
- 2. Denetleyici eylem adı (göz atma)
- 3. Rota parametre değerleri, hem ' % s'adı (Tarz) hem de ' % s'değeri (Tarz adı) belirtme

Tümünü bir araya burada emin ediyoruz Store dizini görünümü için bu bağlantıları nasıl yazacaksınız sokarak:

[!code-cshtml[Main](mvc-music-store-part-3/samples/sample21.cshtml)]

Şimdi, projeyi yeniden çalıştırın ve /Store/ URL'sine erişin türleri listesini göreceğiz. Her bir tür – köprü tıklandığında olan bize bizim/Store/dizinine göz atma sürer? Tarz =*[Tarz]* URL'si.

![](mvc-music-store-part-3/_static/image3.jpg)

HTML Tarz listesi için şöyle görünür:

[!code-html[Main](mvc-music-store-part-3/samples/sample22.html)]


> [!div class="step-by-step"]
> [Önceki](mvc-music-store-part-2.md)
> [İleri](mvc-music-store-part-4.md)
