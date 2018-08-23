---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
title: AJAX kullanarak eşleme senaryoları gerçekleştirmeyi | Microsoft Docs
author: microsoft
description: 11. adım, oluşturma, düzenleme veya görüntüleme azalma l görmek için kullanıcıların etkinleştirme NerdDinner uygulamamıza, AJAX eşleme desteği tümleştirme işlemi açıklanır...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: f731990a-0a81-4d62-81df-87d676cdedd6
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-implement-mapping-scenarios
msc.type: authoredcontent
ms.openlocfilehash: f7de23ca46e6dc00fe8075e28068a8b3f95d02cd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755833"
---
<a name="use-ajax-to-implement-mapping-scenarios"></a>AJAX kullanarak eşleme senaryoları gerçekleştirmeyi
====================
tarafından [Microsoft](https://github.com/microsoft)

[PDF'yi indirin](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 11. adım bir ücretsiz budur ["NerdDinner" uygulaması Öğreticisi](introducing-the-nerddinner-tutorial.md) , Yürüyüşü nasıl küçük bir derleme, ancak tamamlandı, ASP.NET MVC 1 kullanarak web uygulaması aracılığıyla.
> 
> 11. adım, oluşturma, düzenleme veya görüntüleme azalma Akşam Yemeği konumunu grafik görmek için kullanıcılar etkinleştirme NerdDinner uygulamamıza, AJAX eşleme desteği tümleştirme işlemi açıklanır.
> 
> ASP.NET MVC 3 kullanıyorsanız, takip ettiğiniz öneririz [MVC 3 ile çalışmaya başlama](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) veya [MVC müzik Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) öğreticiler.


## <a name="nerddinner-step-11-integrating-an-ajax-map"></a>NerdDinner 11. adım: bir AJAX harita tümleştirme

Şimdi uygulamamız biraz daha görsel olarak AJAX eşleme desteği tümleştirerek heyecan verici oluşturacağız. Bu, kullanıcılar oluşturma, düzenleme veya görüntüleme Akşam Yemeği konumunu grafik görmek için azalma olanak sağlar.

### <a name="creating-a-map-partial-view"></a>Bir harita kısmi görünümü oluşturma

Uygulamamızı içinde çeşitli yerlerde eşleme işlevselliği kullanmak için kullanacağız. Kodumuzu KURU tutmak biz birden çok denetleyici eylemleri ve görünümleri arasında yeniden kullanabilir miyiz tek bir kısmi şablonu içindeki ortak eşleme işlevselliği kapsülleyen. Biz, bu kısmi Görünüm "map.ascx" olarak adlandırın ve \Views\Dinners dizini içinde oluşturun.

Map.ascx kısmi \Views\Dinners dizinde sağ tıklayıp Ekle - i seçerek oluşturabiliriz&gt;görüntüle menü komutu. Biz "Map.ascx" Görünüm adı, kısmi görünüm denetleyin ve kesin tür belirtilmiş "Dinner" model sınıfı geçirilecek kullanacağız olduğunu gösterir:

![](use-ajax-to-implement-mapping-scenarios/_static/image1.png)

Biz "Ekle" düğmesine tıkladığınızda, bizim kısmi şablonu oluşturulur. Ardından Map.ascx aşağıdaki içeriği dosyaya güncelleştireceğiz:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample1.aspx)]

İlk &lt;betik&gt; noktaları Microsoft sanal Earth 6.2 eşleme kitaplığına başvuru. İkinci &lt;betik&gt; noktaları ortak müşterilerimize Javascript eşleme mantığı kapsülleyen, kısa süre içinde oluşturacağız bir map.js dosyasına başvuru. &lt;Div kimliği = "theMap"&gt; Virtual Earth harita barındırmak için kullanacağınız HTML kapsayıcı bir öğedir.

Ardından katıştırılmış sahibiz &lt;betik&gt; bu görünüme belirli iki JavaScript işlevleri içeren blok. İlk işlev kablo yukarı sayfa istemci tarafı betiği çalıştırmaya hazır olduğunda yürüten bir işlev için jQuery kullanır. LoadMap() yardımcı işlevi, çağrı sanal earth harita denetiminin yüklemek için sunduğumuz Map.js betik dosyası içinde tanımlarız. İkinci işlevi tanımlayan bir konum eşleme için bir PIN ekleyen bir geri çağırma olay işleyicisidir.

Sunucu tarafı nasıl kullanıyoruz fark &lt;% = %&gt; enlem ve boylamını istiyoruz JavaScript ile eşlemek için Dinner eklemek için istemci tarafı komut dosyası bloğu içinde blok. (Daha hızlı hale getiren – değerleri almak için ayrı bir AJAX çağrısı sunucuya geri gerek kalmadan) istemci tarafı komut dosyası tarafından kullanılabilecek dinamik değer çıktısı için yararlı bir tekniktir. &lt;% = %&gt; Blokları görünümü sunucuda – işlenirken yürütülecek ve bu nedenle HTML çıktısını yalnızca sona erecek gömülü JavaScript değerlerle (örneğin: var enlem 47.64312; =).

### <a name="creating-a-mapjs-utility-library"></a>Map.js yardımcı program kitaplığı oluşturma

Artık bizim eşlemesi için JavaScript işlevi kapsülleyen (ve yukarıdaki LoadMap ve LoadPin yöntemleri uygulamak için) kullanabiliriz Map.js dosya oluşturalım. Biz bunu Projemizin \Scripts dizininde tıklatılarak ve ardından "Ekle -&gt;yeni öğe" menü komutu JScript öğesini seçip "Map.js" olarak adlandırın.

Aşağıda ekleyeceğiz JavaScript kodu verilmiştir, bizim harita görüntülemek ve konumları PIN'ler için sunduğumuz azalma eklemek için Virtual Earth etkileşim kuracağı Map.js dosya:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample2.js)]

### <a name="integrating-the-map-with-create-and-edit-forms"></a>Eşleme oluşturma ve düzenleme formları ile tümleştirme

Biz artık eşleme desteğinin bizim mevcut oluşturma ve düzenleme senaryoları ile tümleştirerek. Güzel bir haberimiz var, bu oldukça kolay bir yapılacak iş ve bize denetleyicisi kodumuz değiştirmek gerektirmez ' dir. Bizim oluşturma ve düzenleme görünümleri dinner formun kullanıcı arabirimini uygulamak için ortak "DinnerForm" kısmi görünüm paylaştığından, tek bir yerde harita ekleyebilir ve bunu kullanmak hem bizim oluşturma ve düzenleme senaryolara sahip.

Yapılacaklar ihtiyacımız olan \Views\Dinners\DinnerForm.ascx kısmi görünümü açın ve kısmi sunduğumuz yeni harita içerecek şekilde güncelleştirin. Harita eklendikten sonra güncelleştirilmiş DinnerForm nasıl görüneceğini aşağıda verilmiştir (Not: kod parçacığında kısaltma HTML form öğelerini göz ardı edilir):

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample3.aspx)]

(Bu hem bir Akşam Yemeği nesnesi, hem de ülkelerde dropdownlist doldurmak için bir SelectList gerektiğinden) üzerinde kısmi DinnerForm model türü olarak "DinnerFormViewModel" türünde bir nesne alır. Kısmi bizim Haritası "Akşam Yemeği" türünde bir nesne modeli türü olarak yalnızca gerekiyor. ve biz harita kısmi işleme, bu nedenle yalnızca Şimdi Akşam alt özelliğinin DinnerFormViewModel için geçiriyoruz:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample4.aspx)]

"Bulanıklaştırma" olay "Adres" HTML TextBox'a eklemek için kısmi kullandığı jQuery için JavaScript işlevi ekledik. Büyük olasılıkla bir metin kutusuna bir kullanıcı tıkladığında yangın "odak" olayları veya sekmeleri aldık. Bir kullanıcı bir metin kutusu çıktığında tetiklenen "Bulanıklaştırma" olay tersidir. Bu olur ve ardından yeni adres konumu bizim harita üzerinde çizim, yukarıdaki olay işleyicisi enlem ve boylam metin değerlerini temizler. Biz map.js dosyasında tanımlanan bir geri çağırma olay işleyicisi sonra size verdiği adresini temel alarak sanal earth tarafından döndürülen değerleri kullanarak formumuzu üzerinde boylam ve enlem metin kutuları güncelleştirilir.

Ve artık ne zaman yeniden uygulamamızı çalıştırmak bizim standart Dinner form öğelerini birlikte görüntülenen bir varsayılan eşleme görüyoruz "Konak Dinner" sekmesine tıklayın:

![](use-ajax-to-implement-mapping-scenarios/_static/image2.png)

Biz bir adresi yazın ve hemen sekmesinde, harita konumu göstermek için dinamik olarak güncelleştirir ve bizim olay işleyicisi enlem/boylam metin kutuları konumu değerlerle doldurulur:

![](use-ajax-to-implement-mapping-scenarios/_static/image3.png)

Yeni Akşam Yemeği kaydedin ve yeniden düzenleme için açın, biz sayfa yüklendiğinde, harita konumu gösterilen bulabilirsiniz:

![](use-ajax-to-implement-mapping-scenarios/_static/image4.png)

Adres alanı her değiştiğinde haritayı ve enlem/boylam koordinatları güncelleştirir.

Akşam Yemeği konumu haritada görüntüler, ayrıca enlem ve boylam form alanlarını (harita otomatik olarak bunları her zaman bir adres girdiniz güncelleştiriyor beri) bunun yerine gizli öğeleri olarak görünür metin kutuları olan Değiştirebiliriz. Yapılacaklar bu biz Html.Hidden() yardımcı yöntemi kullanarak Html.TextBox() HTML Yardımcısı kullanarak geçiş:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample5.aspx)]

Ve artık bizim formlar biraz daha kullanıcı dostu ve ham enlem/boylam (yine de bunları veritabanındaki her Akşam Yemeği ile depolarken) gösterilmesini:

![](use-ajax-to-implement-mapping-scenarios/_static/image5.png)

### <a name="integrating-the-map-with-the-details-view"></a>Eşleme Ayrıntıları görünümü ile tümleştirme

Size sunduğumuz oluşturma ve düzenleme senaryoları ile tümleşik harita sahip olduğunuza göre şimdi de bunu ayrıntıları senaryomuz tümleştirin. Yapılacaklar ihtiyacımız olan çağrılacak &lt;% Html.RenderPartial("map"); %&gt; Ayrıntılar görünümü içinde.

Tam Ayrıntılar görünümünü (harita tümleştirmesiyle) kaynak koda benzer aşağıdadır:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample6.aspx)]

Artık bir kullanıcı bir /Dinners/Ayrıntılar / [ID] URL gittiğinde, Şimdi Akşam Yemeği harita üzerinde konumunu ayrıntılarını göreceksiniz (tam bir anında iletme PIN ile kullanırken vurgulanan görüntüler Akşam Yemeği başlığı ve bu adresi), ve RSVP fo için bir AJAX bağlantısı r onu:

![](use-ajax-to-implement-mapping-scenarios/_static/image6.png)

### <a name="implementing-location-search-in-our-database-and-repository"></a>Veritabanı ve havuz uygulama konum arama

AJAX kararlılığımızın işlemini tamamlamak için bir harita giriş sayfasına grafik bunları neredeyse azalma aramak kullanıcılara uygulamanın ekleyelim.

![](use-ajax-to-implement-mapping-scenarios/_static/image7.png)

Biz, RADIUS konum tabanlı arama azalma için verimli bir şekilde gerçekleştirmek için veritabanı ve veri deposu katman içinde destek uygulayarak başlarsınız. Kullanabiliriz yeni [Jeo-uzamsal özellikler SQL 2008](https://www.microsoft.com/sqlserver/2008/en/us/spatial-data.aspx) Bunu uygulamak için veya alternatif olarak, Gary Dryden makalede ele alınan bir SQL işlev yaklaşımı kullanabiliriz: [ http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx ](http://www.codeproject.com/KB/cs/distancebetweenlocations.aspx) ve Rob Conery LINQ to SQL ile burada kullanma hakkında işaretlerinize: [http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/](http://blog.wekeroad.com/2007/08/30/linq-and-geocoding/)

Bu tekniği uygulamak için biz "Sunucu Gezgini" Visual Studio içinde açın, NerdDinner veritabanını seçin ve altındaki "işlevleri" alt düğüme sağ tıklayın ve yeni bir "skaler değerli işlev" oluşturmak seçin:

![](use-ajax-to-implement-mapping-scenarios/_static/image8.png)

Biz, ardından aşağıdaki DistanceBetween işlevinde yapıştıracaksınız:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample7.sql)]

Ardından yeni bir tablo değerli işlev "NearestDinners" ararız SQL Server'da oluşturacağız:

![](use-ajax-to-implement-mapping-scenarios/_static/image9.png)

Bu "NearestDinners" tablo işlevi enlem, 100 mil içindeki tüm azalma döndürülecek DistanceBetween yardımcı işlevini kullanır ve boylam biz bunu sağlayın:

[!code-sql[Main](use-ajax-to-implement-mapping-scenarios/samples/sample8.sql)]

Bu işlevi çağırmak için size ilk LINQ to SQL Tasarımcı bizim \Models dizininden NerdDinner.dbml dosyasını çift tıklayarak açmak:

![](use-ajax-to-implement-mapping-scenarios/_static/image10.png)

Ardından NearestDinners ve DistanceBetween işlevleri LINQ bunları SQL NerdDinnerDataContext sınıfına bizim LINQ yöntemleri olarak eklenecek neden SQL tasarımcısına sürükleyip:

![](use-ajax-to-implement-mapping-scenarios/_static/image11.png)

Ardından "FindByLocation" sorgu metodu gelecek döndürülecek NearestDinner işlevi kullanan müşterilerimizin DinnerRepository sınıf kullanıma sunuyoruz, belirtilen konumun içinde 100 mil azalma:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample9.cs)]

### <a name="implementing-a-json-based-ajax-search-action-method"></a>AJAX, JSON tabanlı arama eylem yöntemi uygulama

Biz, artık bir harita doldurmak için kullanılan bir Akşam Yemeği veri listesi geri dönmek için yeni FindByLocation() depo yöntemi faydalanan denetleyici eylem yöntemine uygulayacaksınız. Biz, böylece istemci üzerinde JavaScript kullanarak kolayca yönetilebilir Dinner verileri JSON (JavaScript nesne gösterimi) biçiminde geri döndürür, bu eylem yöntemine sahip olacaksınız.

Bunu gerçekleştirmek için yeni bir "SearchController" sınıf \Controllers dizinde sağ tıklayıp Ekle - i seçerek oluşturacağız&gt;denetleyicisi menü komutu. Biz, ardından "SearchByLocation" eylem yöntemi gibi yeni SearchController sınıftaki uygulayacaksınız:

[!code-csharp[Main](use-ajax-to-implement-mapping-scenarios/samples/sample10.cs)]

SearchController'ın SearchByLocation eylem yöntemi, yakındaki azalma bir listesini almak için DinnerRespository üzerinde FindByLocation yöntemi dahili olarak çağırır. Ancak, Şimdi Akşam nesneleri doğrudan istemciye döndürmek yerine, bunun yerine JsonDinner nesneleri döndürür. Bir Akşam Yemeği özellik alt kümesi JsonDinner sınıfı gösterir (örnek: güvenlik açısından, bir akşam RSVP'd kişilerin adları ifşa değil). Ayrıca, Şimdi Akşam – üzerinde yok ve dinamik olarak belirli bir Akşam Yemeği ile ilişkili RSVP nesne sayısını sayma tarafından hesaplandığı RSVPCount özelliği içerir.

Ardından Json() yardımcı yöntemi denetleyici temel sınıfta azalma JSON tabanlı kablo biçimini kullanarak bir dizisini döndürmek için kullanıyoruz. JSON, basit veri yapılarını temsil eden bir standart metin biçimidir. JSON biçimli bir iki JsonDinner nesnelerin listesini bizim eylem yönteminden döndürülen zaman göründüğünü bir örnek aşağıda verilmiştir:

[!code-json[Main](use-ajax-to-implement-mapping-scenarios/samples/sample11.json)]

### <a name="calling-the-json-based-ajax-method-using-jquery"></a>JQuery kullanarak AJAX, JSON tabanlı yöntemini çağırma

Artık SearchController'ın SearchByLocation eylem yönteminin kullanılacağını NerdDinner uygulama ana sayfası güncelleştirmek hazırız. Yapılacaklar, biz /Views/Home/Index.aspx görünüm şablonunu açın ve bir metin kutusu, arama düğmesini, bizim harita sağlamak için güncelleştirmeniz ve bir &lt;div&gt; dinnerList adlı bir öğe:

[!code-aspx[Main](use-ajax-to-implement-mapping-scenarios/samples/sample12.aspx)]

İki JavaScript işlevleri ardından sayfasına ekleyebilirsiniz:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample13.html)]

Sayfa ilk yüklendiğinde ilk JavaScript işlevi, eşleme yükler. İkinci JavaScript işlevi kablolarına bir JavaScript'kurmak, olay işleyicisi arama düğmesine tıklayın. Düğmeye basıldığında bizim Map.js dosyasına ekleyeceğiz FindDinnersGivenLocation() JavaScript işlevini çağırır:

[!code-javascript[Main](use-ajax-to-implement-mapping-scenarios/samples/sample14.js)]

Harita bu FindDinnersGivenLocation() işlevi çağırır. Find() girilen konuma göre ortalanmasını sanal Earth denetimi. Virtual earth harita hizmeti döndürdüğünde eşleme. Biz son bağımsız değişken olarak geçirilen callbackUpdateMapDinners geri çağırma yöntemi Find() yöntemini çağırır.

Gerçek iş yeri yapılır callbackUpdateMapDinners() yöntemidir. Bir AJAX çağrısı bizim SearchController'ın SearchByLocation() eylem yöntemine – enlem ve boylamını yeni Ortala eşleminin geçirme gerçekleştirmek için jQuery'nın $.post() yardımcı yöntemini kullanır. $.Post() yardımcı yöntem tamamlandığında çağırılacak olan bir satır içi işlevi tanımlar ve "azalma" adında bir değişkene kullanarak eylem yöntemine geçirilecek SearchByLocation() dinner JSON ile biçimlendirilmiş sonuç döndürmedi. Ardından bir foreach döndürülen her Akşam Yemeği yapar ve yeni bir PIN haritaya eklemek için Akşam Yemeği'nın enlem ve boylam ve diğer özellikleri kullanır. Harita sağındaki azalma HTML listesine de bir Akşam Yemeği girdisi ekler. Bunu ardından kablolarına Raptiyeler hem de HTML listenin üzerine gelindiğinde kullanılacak etkinliğimize Akşam Yemeği hakkındaki ayrıntıları, bir kullanıcı üzerine geldiğinde görüntülenir artırma:

[!code-html[Main](use-ajax-to-implement-mapping-scenarios/samples/sample15.html)]

Ve artık size uygulamayı çalıştırın ve biz sahip bir eşleme sunulur giriş sayfasını ziyaret edin. Biz bir şehir adı girdiğiniz zaman, yakın gelecek azalma harita görüntülenir:

![](use-ajax-to-implement-mapping-scenarios/_static/image12.png)

Bir Akşam Yemeği geldiğinizde hakkındaki ayrıntıları görüntüler.

Akşam Yemeği başlığa tıklayarak Kabarcık veya HTML listesinde sağ taraftaki bize biz ardından isteğe bağlı olarak için RSVP dinner – gideceği:

![](use-ajax-to-implement-mapping-scenarios/_static/image13.png)

### <a name="next-step"></a>Sonraki adım

NerdDinner uygulamamız tüm uygulama işlevselliğini artık uyguladık. Haydi şimdi biz nasıl olanak sağlayabileceğiniz göz otomatik birim sınama.

> [!div class="step-by-step"]
> [Önceki](use-ajax-to-deliver-dynamic-updates.md)
> [İleri](enable-automated-unit-testing.md)
