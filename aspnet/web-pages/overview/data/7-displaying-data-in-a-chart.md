---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: ASP.NET Web sayfaları (Razor) içeren bir grafik veri görüntüleme | Microsoft Docs
author: microsoft
description: Bu bölümde, verileri bir grafikte görüntülemek açıklanmaktadır. Önceki bölümde, el ile ve kılavuz veri görüntüleme hakkında bilgi edindiniz. Bu bölümde anlatılmıştır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2012
ms.topic: article
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: 41af8ac4dba0ba5ad478df7075628186a96f48c4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393762"
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>ASP.NET Web sayfaları (Razor) içeren bir grafik veri görüntüleme
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu makalede, verileri kullanarak bir ASP.NET Web sayfaları (Razor) Web sitesinde görüntülemek için bir grafik kullanmayı açıklar `Chart` Yardımcısı.
> 
> **Öğrenecekleriniz**:
> 
> - Verileri bir grafikte görüntülemek nasıl.
> - Yerleşik Temalar kullanarak grafik stilini belirlemek nasıl.
> - Grafik kaydetme ve daha iyi performans için bunları önbelleğe alınacağını.
> 
> ASP.NET programlama makalesinde sunulan özellikler şunlardır:
> 
> - `Chart` Yardımcısı.
> 
> > [!NOTE]
> > Bu makaledeki bilgiler, ASP.NET Web sayfaları 1.0 ve Web Pages 2 için geçerlidir.


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>Grafik Yardımcısı

Verilerinizi grafik formunda görüntülemek istediğiniz zaman kullanabileceğiniz `Chart` Yardımcısı. `Chart` Yardımcısı, çeşitli grafik türleri veri görüntüleyen bir görüntü işleyebilirsiniz. Biçimlendirme ve etiketleme için birçok seçenek destekler. `Chart` Yardımcısı, Microsoft Excel veya başka araçlar alışkın olabileceğiniz grafiklerin tüm türleri dahil olmak üzere grafikleri, 30'dan fazla tür işleyebilirsiniz &#8212; alan grafikleri, çubuk grafikler, sütun grafikler, çizgi grafikler ve pasta grafikleri, birlikte daha fazla bilgi özelleştirilmiş grafikler stok grafikler gibi çalışır.

| **Alan grafiği** ![açıklaması: alan grafik türü resmi](7-displaying-data-in-a-chart/_static/image1.jpg) | **Çubuk grafik** ![açıklaması: çubuk grafik türünü resmi](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Sütun grafiği** ![açıklaması: sütun grafik türü resmi](7-displaying-data-in-a-chart/_static/image3.jpg) | **Çizgi grafik** ![açıklaması: çizgi grafik türü resmi](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Pasta grafiği** ![açıklaması: resim pasta grafik türü](7-displaying-data-in-a-chart/_static/image5.jpg) | **Hisse senedi grafiği** ![açıklaması: resim stoğu grafik türü](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Grafik Öğeleri

Grafikler, veri ve göstergeler, eksen, serisi vb. gibi ek öğeleri gösterir. Aşağıdaki resimde kullandığınızda özelleştirebileceğiniz grafik öğelerinin çoğunu gösterir `Chart` Yardımcısı. Bu makalede bazı ayarlanacağı gösterilmektedir (tüm) bu öğeleri.

![Açıklama: grafik öğelerini gösteren resim](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Verileri bir grafik oluşturma

Bir grafikte görüntüleme veriler bir veritabanından döndürülen sonuçlar, diziden veya bir XML dosyasındaki veriler olabilir.

### <a name="using-an-array"></a>Bir dizi kullanma

Açıklandığı şekilde [ASP.NET Web sayfaları programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkId=202890), bir dizi benzer öğelerin bir koleksiyonunu tek bir değişkende depolamak olanak tanır. Grafiğinizde dahil etmek istediğiniz verileri içeren dizileri kullanabilirsiniz.

Bu yordam, nasıl bir grafik verileri kullanarak dizileri için varsayılan grafik türü kullanılarak oluşturacağınızı gösterir. Ayrıca grafik içinde sayfa görüntüleme işlemini de gösterir.

1. Adlı yeni bir dosya oluşturun *ChartArrayBasic.cshtml*.
2. Mevcut içeriğini aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Kod, önce yeni bir grafik oluşturur ve genişlik ve yükseklik ayarlar. Grafik başlığı kullanarak belirttiğiniz `AddTitle` yöntemi. Veri eklemek için kullandığınız `AddSeries` yöntemi. Bu örnekte, kullandığınız `name`, `xValue`, ve `yValues` parametrelerinin `AddSeries` yöntemi. `name` Parametresi grafik açıklamasında görüntülenir. `xValue` Parametresi, grafiğin yatay ekseni boyunca görüntülenen bir veri dizisi içerir. `yValues` Parametresi, grafiğin dikey noktaları çizmek için kullanılan veri dizisi içerir.

    `Write` Yöntemi aslında grafik oluşturur. Bu durumda, bir grafik türü belirtmediğiniz `Chart` yardımcı bir sütun grafiği, varsayılan grafiği oluşturur.
3. Sayfanın tarayıcıda çalıştırın. Tarayıcı grafiği görüntüler. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Veritabanı sorgusu için grafik verilerini kullanma

Grafik istediğiniz bilgileri veritabanında ise, veritabanı sorgusu çalıştırın ve grafik oluşturmak için veri sonuçlarından'i kullanın. Bu yordam, okuma ve bu makalede oluşturulan veritabanından veri görüntüleme işlemini göstermektedir [ASP.NET Web sayfaları sitelerinde bir veritabanıyla çalışmaya giriş](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Ekleme bir *uygulama\_veri* klasör zaten mevcut değilse Web sitesinin kök klasörü.
2. İçinde *uygulama\_veri* klasöründe adlı veritabanı dosyası ekleme *SmallBakery.sdf* içinde açıklanan [ASP.NET Web sayfaları sitelerindebirveritabanıylaçalışmayagiriş](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Adlı yeni bir dosya oluşturun *ChartDataQuery.cshtml*.
4. Mevcut içeriğini aşağıdakiyle değiştirin:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Kod ilk SmallBakery veritabanı açılır ve adlı bir değişkene atar `db`. Bu değişken temsil eden bir `Database` veritabanına yazma ve okuma için kullanılan nesne. Ardından, kod her ürünün fiyatını ve adını almak için bir SQL sorgusunu çalıştırır. Kod yeni bir grafik oluşturur ve grafiğin çağırarak veritabanı sorgusu geçirir `DataBindTable` yöntemi. Bu yöntem iki parametre alır: `dataSource` parametredir sorgudan gelen veriler için ve `xField` parametresi hangi veri sütununun grafiğin x ekseni için kullanılan ayarlamanızı sağlar.

    Kullanmaya alternatif olarak `DataBindTable` yöntemi kullanabileceğiniz `AddSeries` yöntemi `Chart` Yardımcısı. `AddSeries` Yöntemi ayarlamanızı sağlar `xValue` ve `yValues` parametreleri. Örneğin, kullanmak yerine `DataBindTable` şöyle yöntemi:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Kullanabileceğiniz `AddSeries` şöyle yöntemi:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Her ikisi de aynı sonuçları işleyin. `AddSeries` Yöntemi olduğundan daha esnek grafik türünü ve verileri daha açıkça belirtebilirsiniz ancak `DataBindTable` yöntemdir ek esneklik gerekmiyorsa, kullanımı daha kolay.
5. Sayfanın tarayıcıda çalıştırın. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>XML verilerini kullanma

Grafik üçüncü bir seçenek, grafik verileri olarak bir XML dosyası kullanmaktır. Bu XML dosyasını aynı zamanda bir şema dosyası olmasını gerektirir (*.xsd* dosyası), XML yapısını açıklar. Bu yordam, verilerini bir XML dosyasından okuma işlemini göstermektedir.

1. İçinde *uygulama\_veri* klasöründe adlı yeni bir XML dosyası oluşturun *data.xml*.
2. Var olan XML kurgusal bir şirkette çalışanlar hakkında bazı XML verileri aşağıdaki değiştirin. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. İçinde *uygulama\_veri* klasöründe adlı yeni bir XML dosyası oluşturun *data.xsd*. (Bu kez uzantısı olduğuna dikkat edin *.xsd*.)
4. Var olan XML aşağıdakiyle değiştirin: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. Web sitesinin kök dizininde adlı yeni bir dosya oluşturmak *ChartDataXML.cshtml*.
6. Mevcut içeriğini aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    İlk kod oluşturur bir `DataSet` nesne. Bu nesne, şema dosyasındaki bilgilere göre düzenlemek ve XML dosyasından okunan verileri yönetmek için kullanılır. (Kodun üstüne deyim içeren bildirim `using SystemData`. Bu çalışmak için gerekli `DataSet` nesne. Daha fazla bilgi için [ &quot;kullanma&quot; deyimleri ve tam olarak nitelenmiş adlar](#SB_UsingStatements) bu makalenin ilerleyen bölümlerinde.)

    Ardından, kod oluşturur bir `DataView` dayalı veri kümesinde nesne. Veri Görünümü grafik bağlayabilirsiniz nesne sağlar &#8212; diğer bir deyişle, okuma ve çizim. Grafik verileri kullanarak bağlar `AddSeries` yöntemi gördüğünüz daha önce bu zaman dışında dizi verileri grafiğini tıklattığınızda `xValue` ve `yValues` parametrelerini ayarlamak `DataView` nesne.

    Bu örnek ayrıca belirli grafik türü belirtmek nasıl gösterir. İçinde veri eklendiğinde `AddSeries` yöntemi `chartType` parametresi bir pasta grafiği görüntülemek için de ayarlanır.
7. Sayfanın tarayıcıda çalıştırın. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP]
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>"Kullanma" ifadeleri ve tam olarak nitelenmiş adlar
> 
> Razor sözdizimi olan ASP.NET Web sayfaları temel alan .NET Framework bileşenlerini (sınıflar) binlerce oluşur. Bu sınıfların ile çalışması için yönetilebilir hale getirmek için bunlar halinde düzenlenmiştir *ad alanları*, bakıma kitaplıkları olduğu. Örneğin, `System.Web` ad alanı, tarayıcı/sunucu iletişimini destekleyen sınıflar içerir `System.Xml` ad alanı oluşturmak ve XML dosyaları okumak için kullanılan sınıfları içerir ve `System.Data` ad alanı çalışmanıza olanak sağlayan sınıflar içerir verilerle.
> 
> .NET Framework içinde verilen herhangi bir sınıf erişmek için kod yalnızca sınıf adı, aynı zamanda sınıf olan ad alanı bilmesi gerekir. Örneğin kullanmak için `Chart` Yardımcısı, kod bulması gerektiğinde `System.Web.Helpers.Chart` ad alanı birleştiren sınıfı (`System.Web.Helpers`) sınıf adıyla (`Chart`). Bu sınıfın bilinir *tam* adı &#8212; tam, Belirsiz olmayan konumuna vastness içinde .NET Framework'ün. Kodda, bunun aşağıdaki gibi görünür:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Ancak, hantal (ve hata yapmaya açık) bir sınıf ya da yardımcı başvurmak istediğiniz her zaman bu uzun, tam olarak nitelenmiş adlar kullanmak zorunda. Bu nedenle, sınıf adlarının kullanımını kolaylaştırmak için *alma* yalnızca birkaç birçok ad alanları .NET Framework'teki arasından genellikle, ilgilendiğiniz, ad olduğu. Bir ad alanı aldıysanız, yalnızca bir sınıf adı kullanabilirsiniz (`Chart`) tam adı yerine (`System.Web.Helpers.Chart`). Kodunuzu çalıştırdığında ve bir sınıf adı karşılaştığında, sadece o sınıf bulmak için içeri aktardığımıza ad alanlarında bakabilirsiniz.
> 
> Web sayfaları için Razor sözdizimi olan ASP.NET Web Pages kullandığınızda, genellikle her zaman aynı sınıfları kümesi kullanmak da dahil olmak üzere `WebPage` sınıfı, çeşitli yardımcıları ve benzeri. Her bir Web sitesi oluşturduğunuzda ilgili ad alanlarını içeri aktarma işi kaydetmek için bir dizi temel ad alanı her Web sitesi için otomatik olarak alır, böylece ASP.NET yapılandırılır. İşte bu ad alanları veya şimdiye kadar; içeri aktarma ile uğraşmak zorunda henüz üzerinde çalıştığınız tüm önceden sizin için içe aktarılan ad alanlarında sınıflardır.
> 
> Ancak, bazen sizin için otomatik olarak içeri ad alanında olmayan bir sınıf ile çalışmak gerekir. Bu durumda, bu sınıfın tam adını kullanabilirsiniz veya sınıfı içeren ad uzayı el ile içeri aktarabilirsiniz. Bir ad alanı içeri aktarmak için kullandığınız `using` deyimi (`import` Visual Basic'te), bir örnek daha önce bahsettiğim gibi makale.
> 
> Örneğin, `DataSet` sınıfı `System.Data` ad alanı. `System.Data` Ad alanı, ASP.NET Razor sayfaları için otomatik olarak kullanılabilir değil. Bu nedenle, birlikte çalışmak için `DataSet` tam adı kullanarak sınıfının, bu kodu kullanabilirsiniz:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Kullanmanız gerekiyorsa `DataSet` art arda sınıfı şunun gibi bir ad alanı içeri aktarabilir ve ardından yalnızca sınıf adını kodda kullanın:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Ekleyebileceğiniz `using` deyimleri başvurmak istediğiniz herhangi bir .NET Framework isim için. ASP.NET tarafından kullanılmak üzere otomatik olarak içeri aktarılır ad alanlarında çoğu sınıfların ile çalışması çünkü ancak belirtildiği gibi genellikle bunu gerekmez *.cshtml* ve *.vbhtml* sayfaları.


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Bir Web sayfasındaki grafikleri görüntüleme

Örneklerde grafiğin grafik olarak doğrudan tarayıcıya sonra işlenir ve bir grafik oluşturma şu ana kadar gördünüz. Çoğu durumda, ancak bir grafik bir sayfanın bir parçası yalnızca tek başına tarayıcıda görüntülemek istediğiniz. Bunu yapmak için iki adımlı bir işlem gerektirir. İlk adım zaten gördüğünüz gibi grafik oluşturan bir sayfa oluşturmaktır.

İkinci adım, başka bir sayfaya elde edilen görüntü görüntülemektir. Görüntüyü görüntülemek için bir HTML kullanan `<img>` öğesi, aynı herhangi bir görüntü görüntülemek için yaptığınız şekilde. Ancak başvuran yerine bir *.jpg* veya *.png* dosyası `<img>` öğesi başvuruları *.cshtml* içeren dosya `Chart` Yardımcısı, grafik oluşturur. Görüntü sayfa çalıştığında, `<img>` öğesi çıktısını alır `Chart` Yardımcısı ve grafik oluşturur.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Adlı bir dosya oluşturun *ShowChart.cshtml*.
2. Mevcut içeriğini aşağıdakiyle değiştirin: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Kod `<img>` , daha önce oluşturulmuş grafiği görüntülemek için öğeyi *ChartArrayBasic.cshtml* dosya.
3. Web sayfasının bir tarayıcıda çalıştırın. *ShowChart.cshtml* dosyayı görüntüler yer alan kod temel grafik görüntüsünün *ChartArrayBasic.cshtml* dosya.

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Bir grafik stil oluşturma

`Chart` Yardımcısı, çok sayıda grafiğin görünümünü özelleştirmenize olanak sağlayan seçenekleri destekler. Renkleri, yazı tipleri, kenarlık ve benzeri ayarlayabilirsiniz. Kullanılacak bir grafiğin görünümünü özelleştirmek için kolay bir yolu olan bir *tema*. Tema Yazı tiplerini, renkleri, etiketler, paletler, kenarlık ve etkileri kullanarak bir grafik nasıl oluşturulacağını belirten bilgi topluluklarıdır. (Not grafiğinin stilini grafik türünü göstermez.)

Aşağıdaki tabloda yerleşik Temalar listeler.

| Tema | Açıklama |
| --- | --- |
| `Vanilla` | Beyaz arka plan üzerinde kırmızı sütunlarını görüntüler. |
| `Blue` | Mavi gradyan arka planı sütunlarda görüntüler mavi. |
| `Green` | Görüntüler, yeşil bir gradyan arka plan sütunlarda mavi. |
| `Yellow` | Sarı gradyan arka plan üzerinde turuncu sütunlarını görüntüler. |
| `Vanilla3D` | 3B kırmızı sütunlarını beyaz arka plan üzerinde görüntüler. |

Yeni bir grafik oluşturduğunuzda kullanılacak temayı belirtebilirsiniz.

1. Adlı yeni bir dosya oluşturun *ChartStyleGreen.cshtml*.
2. Sayfanın mevcut içeriği aşağıdakiyle değiştirin:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Bu kod veritabanı için veri kullanır, ancak ekler önceki örnekteki gibi aynıdır `theme` oluştururken parametresi `Chart` nesne. Değiştirilmiş kodun aşağıda gösterilmiştir:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Sayfanın tarayıcıda çalıştırın. Önce aynı verileri görürsünüz, ancak daha kaliteli grafik görünür: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Grafik kaydetme

Kullanırken `Chart` yazarken yardımcı görülen kadar bu makalede, yardımcı yeniden çalıştırıldığında her zaman sıfırdan grafiği oluşturur. Gerekirse, grafik için kod ayrıca veritabanını yeniden sorgular veya veri almak için XML dosyasını yeniden okur. Bazı durumlarda, bunun yapılması karmaşık bir işlem gibi olabilir sorguladığınız veritabanı büyükse veya XML dosyası çok fazla veri içeriyorsa. Grafik çok fazla veri içermeyen olsa bile, dinamik görüntü oluşturma işlemi sunucu kaynaklarını kullanır ve çoğu kişi sayfası veya grafiği görüntüleyen sayfalar istemesi durumunda olabilir bir etkisi sitenizin performansını.

Bir grafik oluşturmanın olası performans etkisini azaltmanıza yardımcı olmak üzere bir grafik ilk zaman oluşturabilirsiniz gereksinim ve kaydedin. Grafiği yeniden, yeniden üretmek yerine gerektiğinde yalnızca kaydedilen sürümü getirebilir ve onu dönüştürebilirsiniz.

Bir grafiği aşağıdaki şekillerde kaydedebilirsiniz:

- (Sunucu) bilgisayar belleğini grafikte önbelleğe alın.
- Grafik bir resim dosyası olarak kaydedin.
- Grafiği bir XML dosyası olarak kaydedin. Bu seçenek, kaydetmeden önce grafik değiştirmenizi sağlar.

### <a name="caching-a-chart"></a>Bir grafiği önbelleğe alma

Bir hesap oluşturduktan sonra önbelleğe alabilir. Bir grafiği önbelleğe kaydetmeye yeniden görüntülenmesi gerekiyorsa yeniden oluşturulması sahip olmadığı anlamına gelir. Önbellekte bir grafiği kaydettiğinizde, bu grafik için benzersiz olması gereken bir anahtar sağlar.

Sunucu belleği düşük çalıştırıyorsa önbelleğe kaydedilen grafikleri kaldırılabilir. Ayrıca, uygulamanızı herhangi bir nedenle yeniden başlatılırsa önbellek temizlenir. Bu nedenle, önbelleğe alınan bir grafikle çalışmak için standart her zaman önce önbellekte kullanılabilir olup olmadığını ve değilse denetleyin ardından oluşturmak veya yeniden oluşturmak için yoludur.

1. Web sitenizi kökünde adlı bir dosya oluşturun *ShowCachedChart.cshtml*.
2. Mevcut içeriğini aşağıdakiyle değiştirin: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    `<img>` Etiket içeren bir `src` işaret eden bir öznitelik *ChartSaveToCache.cshtml* dosyası ve bir anahtar sayfa bir sorgu dizesi olarak geçirir. Anahtar değeri içeren &quot;myChartKey&quot;. *ChartSaveToCache.cshtml* dosyasını içeren `Chart` grafiği oluşturan yardımcı. Birazdan bu sayfayı oluşturacaksınız.

    Sayfa sonunda yok adlı bir sayfaya bağlantı *ClearCache.cshtml*. Ayrıca, kısa bir süre sonra oluşturursunuz bir sayfa olmasıdır. Gereksinim duyduğunuz *ClearCache.cshtml* yalnızca bu örneğin önbelleğe almayı test etmek için — bu bağlantıyı veya önbelleğe alınmış grafiklerle çalışırken normalde verilebilir sayfa değil.
3. Web sitenizi kök adlı yeni bir dosya oluşturun *ChartSaveToCache.cshtml*.
4. Mevcut içeriğini aşağıdakiyle değiştirin:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Kod ilk şey sorgu dizesinin anahtar değer olarak geçilen olup olmadığını denetler. Bu nedenle, kod çağırarak bir grafiğin önbellekten okumaya çalıştığında `GetFromCache` yöntemi ve anahtar geçirme. Hiçbir şey önbelleğinde (Bu grafiğin istenen ilk kez olacağını) bu anahtarı altında olduğunu ettik, kod zamanki grafiği oluşturur. Grafik sona erdiğinde, kod önbelleğine çağırarak kaydeder `SaveToCache`. Bu yöntem (grafiği daha sonra istenebilir biçimde) bir anahtar ve grafik önbelleğine kaydedilmesi gereken süreyi gerektirir. (Grafik önbelleğe kesin zaman ne sıklıkta, temsil ettiği veri değişebilir düşündüğünüz üzerinde bağlıdır.) `SaveToCache` Yöntem de gerektirir bir `slidingExpiration` parametre &#8212; Bu ayarlanırsa true olarak zaman aşımı grafik erişildiğinde her zaman sayaç sıfırlanır. Bu durumda, bu aslında grafiğin önbellek girişi birisi grafik erişilen son saatten sonra 2 dakika süresi anlamına gelir. (Kayan zaman aşımı için önbellek girdisi olarak ne sıklıkta, erişilen ' olursa olsun önbelleğe alınma sonra tam olarak 2 dakika sonra sona erer yani mutlak zaman aşımı alternatiftir.)

    Son olarak, kod kullanır `WriteFromCache` getirmek ve grafiğin önbellekten işlemek için yöntemi. Bu yöntem dışında Not `if` önbellek, grafik ile başlaması oluştu veya oluşturulan ve önbellekte kaydedilmiş olsa da grafiği önbellekten alır çünkü denetleyen blok.

    Örnekte, dikkat `AddTitle` yöntemi, bir zaman damgası içerir. (Geçerli tarihi ve saati ekler &#8212; `DateTime.Now` &#8212; başlığa.)
5. Adlı yeni bir sayfa oluşturun *ClearCache.cshtml* ve içeriğini aşağıdakilerle değiştirin:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Bu sayfa `WebCache` önbelleğe alınmış grafik kaldırmaya yardımcı *ChartSaveToCache.cshtml*. Daha önce belirtildiği gibi normalde bu gibi bir sayfası olması gerekmez. Yalnızca önbelleğe almayı test etme kolaylaştırmak için buraya oluşturuyorsunuz.
6. Çalıştırma *ShowCachedChart.cshtml* bir tarayıcıda web sayfası. Sayfa içindeki kod temel grafik görüntüsünün görüntüler *ChartSaveToCache.cshtml* dosya. Grafik başlığında ne zaman damgasını belirten, not alın. 

    ![Açıklaması: Grafik başlığı zaman damgası ile temel bir grafik resmini](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Tarayıcıyı kapatın.
8. Çalıştırma *ShowCachedChart.cshtml* yeniden. Zaman damgası aynı önce olduğu gibi grafik oluşturulmaması, ancak bunun yerine önbelleğinden okunduğu gösteren olduğuna dikkat edin.
9. İçinde *ShowCachedChart.cshtml*, tıklayın **Önbelleği Temizle** bağlantı. Sayfasına yönlendirileceksiniz *ClearCache.cshtml*, hangi raporların önbelleği temizlendi.
10. Tıklayın **döndürmek için ShowCachedChart.cshtml** yeniden çalıştırın veya bağlantı *ShowCachedChart.cshtml* webmatrix'ten. Önbellek temizlenene çünkü bu zaman zaman damgası, değiştirilir. Bu nedenle, kod grafiği yeniden oluşturun ve tekrar önbelleğe yerleştirin gerekiyordu.

### <a name="saving-a-chart-as-an-image-file"></a>Grafik bir resim dosyası olarak kaydetme

Bir grafiği bir görüntü dosyası olarak kaydedebilirsiniz (örneğin, bir *.jpg* dosya) sunucusunda. Bu gibi durumlarda, görüntü dosyası daha sonra herhangi bir görüntü olduğu gibi kullanabilirsiniz. Dosya depolanan yerine geçici bir önbelleğe kaydedilen avantajlarındandır. Yeni bir grafik görüntüsü (örneğin, her saat) farklı zamanlarda kaydedebilir ve sonra zaman içinde gerçekleşen değişiklikleri kalıcı kaydını tutun. Unutmayın, web uygulamanızın bir dosya, görüntü dosyası koymak istediğiniz sunucuda klasöre kaydetme izniniz olduğundan emin olmanız gerekir.

1. Web sitenizi kökünde adlı bir klasör oluşturun  *\_ChartFiles* zaten yoksa.
2. Web sitenizi kök adlı yeni bir dosya oluşturun *ChartSave.cshtml*.
3. Mevcut içeriğini aşağıdakiyle değiştirin:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Kod ilk bakar olmadığını *.jpg* dosya var. çağırarak `File.Exists` yöntemi. Kod dosyası mevcut değilse yeni bir oluşturur `Chart` bir diziden. Bu süre, kod çağrıları `Save` yöntemi ve geçişleri `path` dosyası yolu ve dosya adını grafik kaydedileceği yeri belirtmek için parametre. Sayfanın gövdesindeki bir `<img>` öğe yolu işaret edecek şekilde kullanır *.jpg* dosya görüntülenecek.
4. Çalıştırma *ChartSave.cshtml* dosya.
5. WebMatrix için döndürür. Bir resim dosyası adlı bildirim *chart01.jpg* kaydedilmiş  *\_ChartFiles* klasör.

### <a name="saving-a-chart-as-an-xml-file"></a>Grafik bir XML dosyası olarak kaydetme

Son olarak, bir grafiği sunucuya bir XML dosyası olarak kaydedebilirsiniz. Grafiği önbelleğe kaydetmeye veya grafiği bir dosyaya kaydetme üzerinde bu yöntemi kullanmanın bir avantajı, istediyseniz, grafiği görüntülemeden önce XML'yi değiştirebilir ' dir. Uygulamanızın görüntü dosyasını yerleştirmek istediğiniz klasörü sunucuda için okuma/yazma izinlerine sahip gerekir.

1. Web sitenizi kök adlı yeni bir dosya oluşturun *ChartSaveXml.cshtml*.
2. Mevcut içeriğini aşağıdakiyle değiştirin:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Bu kod, bir XML dosyası kullanması hariç, bir grafik önbellekte depolamak için daha önce gördüğünüzle koda benzer. Kod ilk XML dosyasını çağırarak bulunup bulunmadığını denetler `File.Exists` yöntemi. Kod dosyası mevcut değilse yeni bir oluşturur `Chart` nesne ve dosya adı olarak geçirir `themePath` parametresi. Bu XML dosyasında ne olursa olsun tabanlı grafik oluşturur. XML dosyası zaten mevcut değilse, kod gibi normal bir grafik oluşturur ve `SaveXml` kaydetmek için. Grafik, kullanılarak işlenir `Write` yöntemi görmüş önce.

    Bu kod, önbelleğe alma gösterdi sayfayla gibi grafik başlığında bir zaman damgası içerir.
3. Adlı yeni bir sayfa oluşturun *ChartDisplayXMLChart.cshtml* ve aşağıdaki işaretlemeyi ekleyin: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Çalıştırma *ChartDisplayXMLChart.cshtml* sayfası. Grafik görüntülenir. Grafiğin başlığı Not zaman damgası.
5. Tarayıcıyı kapatın.
6. Webmatrix'te, sağ  *\_ChartFiles* klasöründe tıklayın **Yenile**ve ardından klasörünü açın. *XMLChart.xml* dosyayı bu klasöre tarafından oluşturuldu `Chart` Yardımcısı. 

    ![Açıklama: grafik Yardımcısı tarafından oluşturulan XMLChart.xml dosyasını gösteren _ChartFiles klasör.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Çalıştırma *ChartDisplayXMLChart.cshtml* yeniden sayfa. Grafik sayfası çalıştırdığınız ilk kez aynı zaman damgası gösterilir. Daha önce kaydettiğiniz XML'den oluşturulan grafik olmasıdır.
8. Webmatrix'te, açın  *\_ChartFiles* klasörü ve delete *XMLChart.xml* dosya.
9. Çalıştırma *ChartDisplayXMLChart.cshtml* daha sonra sayfa. Bu kez, çünkü zaman damgası güncelleştirilir `Chart` Yardımcısı XML dosyasını yeniden gerekiyordu. İsterseniz, kontrol  *\_ChartFiles* klasörü ve XML dosyasını geri olduğuna dikkat edin.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET Web veritabanıyla çalışmaya giriş sayfaları](https://go.microsoft.com/fwlink/?LinkId=202893)
- [ASP.NET Web önbelleğe alma özelliğini kullanma performansını artırmak için sayfaları](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Grafik sınıfı](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (MSDN üzerinde ASP.NET Web sayfaları API Başvurusu)
