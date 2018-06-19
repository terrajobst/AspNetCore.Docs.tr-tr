---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: AJAX senaryolarında eşleme uygulamak için kullandığınız | Microsoft Docs
author: microsoft
description: 11. adım, AJAX eşleme destek oluşturma, düzenleme veya l görmek için azalma görüntüleme kullanıcıları etkinleştirme bizim NerdDinner uygulamasına tümleştirmek gösterilmektedir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: 4b3f1e46886c4c1f054e43768b0a44695d71bf09
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872721"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>AJAX senaryolarında eşleme uygulamak için kullanın
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 11. adım bir ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , yetenekte küçük bir yapı ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulamasına nasıl aracılığıyla.
> 
> 11. adım, AJAX eşleme destek oluşturma, düzenleme veya görüntüleme Yemeği konumunu grafik görmek için azalma kullanıcıları etkinleştirme bizim NerdDinner uygulamasına tümleştirmek gösterilmektedir.
> 
> ASP.NET MVC 3 kullanıyorsanız, izlemeniz önerilir [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik deposu](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticileri.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner 11. adım: bir AJAX eşlemesi tümleştirme

Şimdi uygulamamız biraz daha görsel olarak AJAX eşleme destek tümleştirerek heyecan verici vermiyoruz. Bu, oluşturma, düzenleme veya görüntüleme Yemeği konumunu grafik görmek için azalma kullanıcıları olanak tanır.

### <a name="creating-a-map-partial-view"></a>Bir harita kısmi görünümü oluşturma

Uygulamamız içinde çeşitli yerlerde eşleme işlevselliği kullanmak olacak. Bizim kod KURU tutmak için birden fazla denetleyici eylemleri ve görünümleri yeniden kullandığımız tek bir kısmi şablonu içindeki ortak eşleme işlevselliği kapsülleyeceğiz. Biz "map.ascx" Bu kısmi görünüm adını ve \Views\Dinners dizininde oluşturun.

Map.ascx kısmi \Views\Dinners dizinde sağ tıklayıp seçerek Ekle - oluşturabiliriz&gt;görüntülemek menü komutu. Biz "Map.ascx" Görünüm adı, bir kısmi görünüm olarak denetleyin ve biz kesin türü belirtilmiş "Yemeği" model sınıfı geçirmek zorunda kalacaklarını belirtin:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Biz "Ekle" düğmesini tıklattığınızda bizim kısmi şablonu oluşturulur. Ardından aşağıdaki içeriğe sahip Map.ascx dosya güncelleştiriyoruz:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

İlk &lt;betik&gt; noktaları Microsoft Sanal dünya 6.2 eşleme kitaplığına başvuru. İkinci &lt;betik&gt; referans ortak Javascript eşleme mantığımızı kapsülleyen, kısa süre içinde oluşturacağız bir map.js dosyasına noktaları. &lt;Div ID "theMap" =&gt; Virtual Earth harita barındırmak için kullanacağınız HTML kapsayıcı bir öğedir.

Ardından katıştırılmış sahibiz &lt;betik&gt; bu görünüme özgü iki JavaScript işlevleri içeren bloğu. İlk işlev jQuery kablo li sayfa istemci tarafı komut dosyası çalışmaya hazır olduğunda yürüten bir işlev için kullanır. LoadMap() yardımcı işlevi, çağıran Sanal dünya harita denetiminin yüklemek için bizim Map.js komut dosyası içinde tanımlama olasılığınız yüksektir. İkinci bir konuma tanımlayan eşlemesine bir PIN ekler bir geri çağırma olay işleyicisi işlevdir.

Sunucu tarafı nasıl kullanıyoruz fark &lt;% = %&gt; enlem ve boylamını istiyoruz JavaScript ile eşlemek için yemeği katıştırmak için istemci tarafı komut dosyası bloğunda blokta. (Daha hızlı olmasını sağlayan – değerleri almak için ayrı bir AJAX çağrısı sunucuya geri gerekmeksizin) istemci tarafı komut dosyası tarafından kullanılabilir dinamik değerler çıktısını almak için kullanışlı bir tekniktir. &lt;% = %&gt; Blokları görünümü sunucuda – işlenirken yürütülecek ve bu nedenle HTML çıkışını yalnızca bitiş katıştırılmış JavaScript değerlerle (örneğin: var enlem 47.64312; =).

### <a name="creating-a-mapjs-utility-library"></a>Map.js yardımcı programı kitaplığı oluşturma

Şimdi biz bizim eşlemesi için JavaScript işlevleri kapsülleyen (ve yukarıdaki LoadMap ve LoadPin yöntemleri uygulamak için) kullanabilirsiniz Map.js dosya oluşturalım. Sizi Projemizin \Scripts dizininde üzerinde sağ tıklayarak bunu ve ardından "Ekle -&gt;yeni öğesi" menü komutu, JScript öğesini seçin ve "Map.js" olarak adlandırın.

Hesabına ekleyeceğiz JavaScript kodu aşağıdadır bizim harita görüntülemek ve konumları PIN'ler için bizim azalma eklemek için Virtual Earth etkileşim kuracağı Map.js dosyasına:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Eşleme oluşturma ve düzenleme Forms ile tümleştirme

Biz şimdi harita destek bizim varolan oluşturma ve düzenleme senaryoları ile tümleştirmeniz. İyi haber bu oldukça kolay yapılacak iş ve denetleyici kodumuza değiştirmek için bize gerektirmeyen ' dir. Bizim oluşturma ve düzenleme görünümleri Yemeği formun UI uygulamak için ortak "DinnerForm" kısmi görünüm paylaştığından, tek bir yerde harita ekleyebilir ve kullanmak hem bizim oluşturma ve düzenleme senaryolara sahip.

Tüm yapılacaklar ihtiyacımız var. \Views\Dinners\DinnerForm.ascx kısmi görünümü açın ve kısmi bizim yeni eşleme içerecek şekilde güncelleştirmek için Harita eklendikten sonra güncelleştirilmiş DinnerForm nasıl görüneceği aşağıda verilmiştir (Not: kod parçacığını kısaltma gelen HTML form öğelerini göz ardı edilir):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

(, Hem bir Yemeği nesnesi yanı sıra ülkelerde dropdownlist doldurmak için bir SelectList gerektiğinden) yukarıdaki kısmi DinnerForm model türü olarak "DinnerFormViewModel" türünde bir nesne alır. Kısmi bizim harita "Yemeği" türünde bir nesne modeli türü olarak yalnızca gerekiyor. ve biz harita kısmi işlemek, böylece yalnızca Yemeği alt özelliği DinnerFormViewModel kendisine geçiriyoruz:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

"Adres" HTML metin kutusuna "Bulanıklaştırma" olay eklemek için kısmi kullanır jQuery için JavaScript işlevinin ekledik. Bir metin kutusuna kullanıcı tıkladığında yangın "odak" olayları veya sekmeler büyük olasılıkla duymadığınız. Bir kullanıcı bir metin kutusu çıktığında tetiklenen bir "Bulanıklaştırma" olay tersidir. Bu olur ve yeni adres konumun bizim haritaya çizer yukarıdaki olay işleyicisi enlem ve boylam metin değerlerini temizler. Biz map.js dosyasında tanımlanan bir geri çağırma olay işleyicisi sonra biz vermiş adresini temel alan sanal earth tarafından döndürülen değerleri kullanarak bizim formdaki boylam ve enlem kutularındaki güncelleştirir.

Şimdi ne zaman biz bizim uygulamayı yeniden çalıştırın ve ve bizim standart Yemeği form öğelerini birlikte görüntülenen eşleme varsayılan göreceğiz "Ana bilgisayar Yemeği" sekmesini tıklatın:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Biz bir adres yazın ve hemen sekmesinde eşleme konumu göstermek için dinamik olarak güncelleştirir ve bizim olay işleyicisi enlem/boylam kutularındaki konumu değerleriyle dolduracaktır:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Yeni Yemeği kaydedin ve ardından yeniden düzenlemek için açın, biz sayfa yüklendiğinde eşleme konumu görüntülenir bulabilirsiniz:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Adres alanı her değiştiğinde harita ve enlem/boylam koordinatları güncelleştirir.

Harita Yemeği konumu görüntüler, (harita otomatik olarak bunları her zaman bir adres girdiniz güncelleştiriyor bu yana) yerine gizli öğeleri olacak şekilde görünür kutularındaki olmaktan biz de enlem ve boylam form alanlarını değiştirebilirsiniz. Yapılacaklar bu biz Html.Hidden() yardımcı yöntemi kullanarak Html.TextBox() HTML Yardımcısı kullanarak geçiş:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Ve şimdi bizim forms biraz daha kullanıcı dostu (hala bunları veritabanında her Yemeği ile depolarken) ham enlem/boylam görüntüleme kaçının:

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Harita Ayrıntılar görünümü ile tümleştirme

Biz bizim oluşturma ve düzenleme senaryoları ile tümleşik harita sahip olduğunuza göre şimdi de bu ayrıntıları Senaryomuzda ile tümleştirin. Tüm yapılacaklar ihtiyacımız çağırmaktır &lt;% Html.RenderPartial("map"); %&gt; Ayrıntılar görünümü içinde.

(Harita tümleştirmesiyle) tam ayrıntıları görüntülemek için kaynak kodunu nasıl göründüğünü aşağıdadır:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

Şimdi bir kullanıcı bir /Dinners/Ayrıntılar / [kimlik] URL'sine gittiğinde bunlar harita üzerinde Yemeği konumunu Yemeği ayrıntılarını görürsünüz (tam bir itme ile olduğunda üzerine getirildiği görüntüler Yemeği başlığı ve adresidir), ve RSVP fo AJAX Bağlan r onu:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Bizim veritabanı ve havuz konumu arama uygulama

Bizim AJAX uygulama devre dışı tamamlamak için bir harita grafik bunları yakın azalma aramak kullanıcılara uygulama giriş sayfasına ekleyelim.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Biz azalma konum temelli RADIUS Ara verimli bir şekilde gerçekleştirmek için veritabanı ve veri deposu katman içinde destek uygulayarak başlarsınız. Biz yeni kullanabilirsiniz [Jeo-uzamsal özellikleri SQL 2008'in](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) Bunu uygulamak için veya alternatif olarak biz Gary Dryden burada makalesinde açıklanan bir SQL işlevi yaklaşım kullanabilirsiniz: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) ve Ramiz Conery LINQ-SQL ile burada kullanma hakkında blogged: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Bu teknik uygulamak için biz "Server" Visual Studio içinde Gezgini, NerdDinner veritabanını seçin ve ardından altında "işlevleri" alt düğümünü sağ tıklatın ve yeni bir "skaler değerli işlev" oluşturmak için seçin:

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Biz aşağıdaki DistanceBetween işlevinde yapıştırın:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Ardından yeni bir tablo değerli işlev "NearestDinners" arayacağız SQL Server'da oluşturacağız:

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Enlem, 100 mil içindeki tüm azalma döndürmek için bu "NearestDinners" tablo işlev DistanceBetween yardımcı işlevini kullanır ve boylam biz bunu sağlayın:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Bu işlevi çağırmak için size ilk LINQ-SQL Tasarımcısı bizim \Models dizini içindeki NerdDinner.dbml dosyasını çift tıklatarak açmak:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Biz sonra NearestDinners ve DistanceBetween işlevleri LINQ bunları SQL NerdDinnerDataContext sınıfına bizim LINQ yöntemi olarak eklenecek neden olacak SQL Tasarımcısı sürükleyin:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Biz ardından gelecek döndürmek için NearestDinner işlevini kullanan bizim DinnerRepository sınıfı "FindByLocation" sorgu yöntemi getirebilir 100 mili belirtilen konum olan azalma:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>JSON tabanlı AJAX arama eylem yöntemi uygulama

Biz şimdi faydalanan bir harita doldurmak için kullanılan bir Yemeği veri listesini geri dönmek için yeni FindByLocation() depo yöntemi denetleyici eylem yöntemine uygulamanız. Biz, böylece istemci üzerinde JavaScript kullanarak kolayca yönetilebilir geri Yemeği JSON (JavaScript nesne gösterimi) biçiminde dönüş verileri bu eylem yöntemine sahip olacaksınız.

Bu uygulama için yeni bir "SearchController" sınıf \Controllers dizinde sağ tıklayıp seçerek Ekle - oluşturacağız&gt;denetleyicisi menü komutu. Biz, ardından "SearchByLocation" eylem yöntemi gibi yeni SearchController sınıfına içinde uygulayabilirsiniz:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

SearchController'ın SearchByLocation eylem yöntemi, dahili olarak yakındaki azalma listesini almak DinnerRespository FindByLocation yöntemini çağırır. Ancak, doğrudan istemciye Yemeği nesneleri döndürmek yerine, bunun yerine JsonDinner nesneleri döndürür. JsonDinner sınıfı bir alt kümesini Yemeği özellikler sunar (örneğin: güvenlik nedenleriyle, yemeği için RSVP'd kişilerin adlarını ifşa değil). Ayrıca, Yemeği – üzerinde mevcut değil ve dinamik olarak belirli bir Yemeği ile ilişkili RSVP nesne sayısı sayım tarafından hesaplandığı RSVPCount özelliği içerir.

Ardından Json() yardımcı yöntemi denetleyici temel sınıfını JSON tabanlı kablo biçimini kullanarak azalma dizisini döndürmek için kullanıyoruz. JSON, basit veri yapıları temsil eden bir standart metin biçimidir. JSON biçimli iki JsonDinner nesnelerin listesini bizim eylem yönteminden döndürülen zaman nasıl göründüğünü örneği aşağıdadır:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>JQuery kullanılarak JSON tabanlı AJAX yöntemi çağırma

Biz şimdi SearchController'ın SearchByLocation eylem yöntemini kullanmak için NerdDinner uygulama giriş sayfası güncelleştirmek hazır olursunuz. Yapılacaklar Bu, biz /Views/Home/Index.aspx görünüm şablonu açın ve bir metin kutusuna, arama düğmesini, bizim harita olmasını güncelleştirme ve bir &lt;div&gt; dinnerList adlı öğe:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

İki JavaScript işlevleri sonra sayfasına ekleyebilirsiniz:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

Sayfa ilk kez yüklediğinde ilk JavaScript işlevi harita yükler. İkinci JavaScript işlevi kablolarına bir JavaScript yukarı olay işleyicisi arama düğmesine tıklayın. Düğmeye basıldığında bizim Map.js dosyasına ekleyeceğiz FindDinnersGivenLocation() JavaScript işlevi çağırır:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Bu FindDinnersGivenLocation() işlev eşlemesi çağırır. Find() sanal dünya denetimindeki girilen konumunda Merkezi. Sanal dünya harita hizmet döndüğünde eşleme. Find() yöntemi biz son bağımsız değişken olarak geçirilen callbackUpdateMapDinners geri çağırma yöntemini çağırır.

Gerçek iş yeri yapılır callbackUpdateMapDinners() yöntemidir. AJAX çağrısı bizim SearchController'ın SearchByLocation() eylem yöntemine – enlem ve boylam yeni ortalanmış harita geçirme gerçekleştirmek için jQuery'nın $.post() yardımcı yöntemini kullanır. $.Post() yardımcı yöntem tamamlandığında çağrılacak bir satır içi işlev tanımlar ve "azalma" adlı bir değişken kullanarak eylem yöntemine geçirilir SearchByLocation() JSON biçimli Yemeği sonuç döndürmedi. Ardından bir foreach döndürülen her Yemeği yapar ve Yemeği'nın enlem ve boylam ve diğer özellikleri harita üzerinde yeni bir PIN eklemek için kullanır. Ayrıca harita sağındaki azalma HTML listesine bir Yemeği girdisi ekler. Bunu ardından kablolarını vurgulu olay iğneleri ve HTML listesi için böylece kullanıcı üzerinde dolaştığında Yemeği ayrıntılarını görüntülenir Yukarı:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

Ve şimdi zaman biz uygulamayı çalıştırın ve biz bir haritası sunulur giriş sayfasını ziyaret edin. Biz bir şehir adını girerken harita, yakın gelecek azalma görüntülenir:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Yemeği gelerek veya onları hakkındaki ayrıntıları görüntüler.

Yemeği başlığını tıklatarak Kabarcık veya HTML listesinde sağ taraftaki bize biz ardından isteğe bağlı olarak için RSVP Yemeği – gideceği:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Sonraki adım

Şimdi NerdDinner uygulamamız tüm uygulama işlevselliğini uyguladık. Şimdi nasıl etkinleştirme göz birim artık otomatik sınama.

> [!div class="step-by-step"]
> [Önceki](use-ajax-to-deliver-dynamic-updates.md)
> [sonraki](enable-automated-unit-testing.md)
