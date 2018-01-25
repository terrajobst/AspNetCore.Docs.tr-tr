---
uid: web-pages/overview/data/7-displaying-data-in-a-chart
title: "ASP.NET Web sayfaları (Razor) içeren bir grafik verileri görüntüleme | Microsoft Docs"
author: microsoft
description: "Bu bölümde, grafik verileri görüntülemek açıklanmaktadır. Önceki bölümlerde, el ile ve kılavuz verileri görüntülemeyi öğrendiniz. Bu bölümde anlatılmıştır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2012
ms.topic: article
ms.assetid: f889fd46-4dac-4ecb-83d8-60e64c22036e
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/data/7-displaying-data-in-a-chart
msc.type: authoredcontent
ms.openlocfilehash: f252b74bc42d0ea65b8b1150973c4f3c50cc9cf4
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="displaying-data-in-a-chart-with-aspnet-web-pages-razor"></a>ASP.NET Web sayfaları (Razor) içeren bir grafik verileri görüntüleme
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu makalede bir grafik kullanarak bir ASP.NET Web sayfaları (Razor) Web sitesi verileri görüntülemek için nasıl kullanılacağı açıklanmaktadır `Chart` Yardımcısı.
> 
> **Öğrenecekleriniz**:
> 
> - Grafik verileri görüntülemek nasıl.
> - Yerleşik Temalar kullanarak stili grafikler nasıl.
> - Grafikler kaydedin ve daha iyi performans için önbelleğe almak.
> 
> Bu makalede sunulan özellikler programlama ASP.NET şunlardır:
> 
> - `Chart` Yardımcısı.
> 
> > [!NOTE]
> > Bu makaledeki bilgileri, ASP.NET Web sayfaları 1.0 ve Web Pages 2 için geçerlidir.


<a id="The_Chart_Helper"></a>
## <a name="the-chart-helper"></a>Grafik Yardımcısı

Verilerinizi grafik formda görüntülemek istediğinizde, kullanabileceğiniz `Chart` Yardımcısı. `Chart` Yardımcısı, grafik türleri çeşitli verileri görüntüleyen bir görüntü işleyebilirsiniz. Biçimlendirme ve etiketleme için birçok seçenek destekler. `Chart` Yardımcı aşina Microsoft Excel veya diğer araçlar olabilecek grafikler tüm türleri &#8212;dahil olmak üzere grafikleri, 30'dan fazla tür işleyebilirsiniz; alan grafiklerinde, çubuk grafikler, sütun grafikler, çizgi grafikler ve pasta grafikler, bunların ile daha fazla bilgi Özel grafikler hisse senedi grafiklerinde ister.

| **Alan grafiği** ![Açıklama: alan grafik türünün resmi](7-displaying-data-in-a-chart/_static/image1.jpg) | **Çubuk grafik** ![Açıklama: çubuk grafik türünü resmi](7-displaying-data-in-a-chart/_static/image2.jpg) |
| --- | --- |
| **Sütun grafiği** ![Açıklama: sütun grafik türünün resmi](7-displaying-data-in-a-chart/_static/image3.jpg) | **Çizgi grafiği** ![Açıklama: çizgi grafik türü resmi](7-displaying-data-in-a-chart/_static/image4.jpg) |
| **Pasta grafik** ![Açıklama: pasta grafik türünün resmi](7-displaying-data-in-a-chart/_static/image5.jpg) | **Hisse senedi grafiği** ![Açıklama: hisse senedi grafik türünün resmi](7-displaying-data-in-a-chart/_static/image6.jpg) |

### <a name="chart-elements"></a>Grafik Öğeleri

Grafikler, veri ve göstergeleri, eksenleri, seri vb. gibi ek öğeler gösterir. Aşağıdaki resimde kullandığınızda, özelleştirebileceğiniz grafik öğeleri birçoğu gösteren `Chart` Yardımcısı. Bu makalede bazı ayarlanacağı gösterilmiştir (Tümü) bu öğelerin.

![Açıklama: grafik öğeleri gösteren resim](7-displaying-data-in-a-chart/_static/image7.jpg)

<a id="Creating_a_Chart"></a>
## <a name="creating-a-chart-from-data"></a>Verileri bir grafik oluşturma

Grafikte görüntüleme veriler, veritabanı tarafından döndürülen sonuçlarından bir dizi veya bir XML dosyasındaki veri olabilir.

### <a name="using-an-array"></a>Bir dizi kullanma

İçinde anlatıldığı gibi [ASP.NET Web sayfalarını programlama kullanarak Razor sözdizimi giriş](https://go.microsoft.com/fwlink/?LinkId=202890), bir dizi benzer öğe koleksiyonunu tek bir değişkende saklamanızı sağlar. Grafiğinizde dahil etmek istediğiniz verileri içerecek şekilde diziler kullanın.

Bu yordam, nasıl bir grafik, dizilerde verilerden varsayılan grafik türü kullanılarak oluşturabileceğinizi gösterir. Ayrıca, grafik sayfa içinde görüntülemek nasıl gösterir.

1. Adlı yeni bir dosya oluşturun *ChartArrayBasic.cshtml*.
2. Varolan içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample1.cshtml)]

    Kod önce yeni bir grafik oluşturur ve genişlik ve yükseklik ayarlar. Grafik başlığını kullanarak belirttiğiniz `AddTitle` yöntemi. Veri eklemek için kullandığınız `AddSeries` yöntemi. Bu örnekte, kullandığınız `name`, `xValue`, ve `yValues` parametrelerinin `AddSeries` yöntemi. `name` Parametresi grafik açıklamasında görüntülenir. `xValue` Parametresi grafiğin yatay ekseni boyunca görüntülenen bir veri dizisi içerir. `yValues` Parametresi grafiğin dikey noktaları çizmek için kullanılan veri dizisi içerir.

    `Write` Yöntemi gerçekte grafik işler. Bu durumda, bir grafik türü belirtmediğiniz `Chart` yardımcı bir sütun grafiği, varsayılan grafik oluşturur.
3. Sayfayı tarayıcıda çalıştırın. Tarayıcı grafiği görüntüler. 

    ![](7-displaying-data-in-a-chart/_static/image8.jpg)

### <a name="using-a-database-query-for-chart-data"></a>Grafik verileri için bir veritabanı sorgusu kullanma

Grafik istediğiniz bilgileri bir veritabanında ise, bir veritabanı sorgusu çalıştırmak ve grafik oluşturmak için sonuçları verileri kullanın. Bu yordam okuyup makalesinde oluşturulan veritabanındaki verileri görüntülemek nasıl gösterir [ASP.NET Web sayfaları sitelerdeki veritabanı ile çalışmaya giriş](https://go.microsoft.com/fwlink/?LinkId=202893).

1. Ekleme bir *uygulama\_veri* klasörü zaten yoksa, Web sitesinin kök klasörü.
2. İçinde *uygulama\_veri* klasörünü adlı veritabanı dosyası ekleme *SmallBakery.sdf* içinde açıklanan [ASP.NET Web Pages sitelerindebirveritabanıileçalışmayagiriş](https://go.microsoft.com/fwlink/?LinkId=202893).
3. Adlı yeni bir dosya oluşturun *ChartDataQuery.cshtml*.
4. Varolan içeriği aşağıdakiyle değiştirin:   

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample2.cshtml)]

    Kod ilk SmallBakery veritabanını açar ve adlı bir değişkene atar `db`. Bu değişken temsil eden bir `Database` okuma ve veritabanına yazmak için kullanılan nesne. Ardından, kod adını ve her ürünün fiyatı almak için bir SQL sorgusunu çalıştırır. Kod yeni bir grafik oluşturur ve grafiğin çağırarak veritabanı sorgusu geçirilir `DataBindTable` yöntemi. Bu yöntemi iki parametre alır: `dataSource` parametredir sorgudaki verileri için ve `xField` parametresi hangi veri sütununun grafiğin x ekseni için kullanılan ayarlamanıza olanak tanır.

    Kullanmaya alternatif olarak `DataBindTable` kullanabileceğiniz yöntemi, `AddSeries` yöntemi `Chart` Yardımcısı. `AddSeries` Yöntemi ayarlamanıza olanak tanır `xValue` ve `yValues` parametreleri. Örneğin, kullanmak yerine `DataBindTable` yöntemi şuna benzer:

    [!code-css[Main](7-displaying-data-in-a-chart/samples/sample3.css)]

    Kullanabileceğiniz `AddSeries` yöntemi şuna benzer:

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample4.html)]

    Her ikisi de aynı sonuçları işleyebilir. `AddSeries` Yöntemi olduğundan daha esnek grafik türü ve veri daha açık olarak belirtebilirsiniz, ancak `DataBindTable` yöntemdir ek esneklik gerekmiyorsa kullanmak daha kolay.
5. Bir tarayıcıda. Sayfayı çalıştırın. 

    ![](7-displaying-data-in-a-chart/_static/image9.jpg)

### <a name="using-xml-data"></a>XML verileri kullanma

Grafik üçüncü seçenek, grafik için veri olarak bir XML dosyası kullanmaktır. Bu XML dosyasını bir şema dosyası sahip olmasını gerektirir (*.xsd* dosyası), XML yapısını açıklar. Bu yordamda verilerini bir XML dosyasından okuma gösterilmiştir.

1. İçinde *uygulama\_veri* klasörünü adlı yeni bir XML dosyası oluşturma *data.xml*.
2. Varolan XML kurgusal bir şirkette çalışanları hakkında bazı XML verileri aşağıdaki değiştirin. 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample5.xml)]
3. İçinde *uygulama\_veri* klasörünü adlı yeni bir XML dosyası oluşturma *data.xsd*. (Bu kez uzantısıdır Not *.xsd*.)
4. Varolan XML aşağıdakiyle değiştirin: 

    [!code-xml[Main](7-displaying-data-in-a-chart/samples/sample6.xml)]
5. Web sitesinin kök dizininde adlı yeni bir dosya oluşturun *ChartDataXML.cshtml*.
6. Varolan içeriği aşağıdakiyle değiştirin: 

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample7.cshtml)]

    Kod ilk oluşturur bir `DataSet` nesnesi. Bu nesnenin XML dosyasından okuma ve şema dosyasındaki bilgiler göre düzenlemek verileri yönetmek için kullanılır. (Kod üstündeki deyimi içeren bildirim `using SystemData`. İle çalışabilmek için bu gereklidir `DataSet` nesnesi. Daha fazla bilgi için bkz: [ &quot;kullanma&quot; deyimleri ve tam olarak nitelenmiş adlar](#SB_UsingStatements) bu makalenin ilerisinde yer.)

    Ardından, kod oluşturur bir `DataView` nesne veri kümesine bağlı. Veri Görünümü grafik bağlayabilirsiniz bir nesne &#8212;sağlar; diğer bir deyişle, okuma ve çizim. Grafik verileri kullanarak bağlar `AddSeries` yöntemi gördüğünüz daha önce bu süre dışında dizi veri grafiğini tıklattığınızda `xValue` ve `yValues` parametreleri ayarlandığında `DataView` nesne.

    Bu örnek ayrıca belirli grafik türü belirtmek nasıl gösterir. İçinde veri eklendiğinde `AddSeries` yöntemi, `chartType` pasta grafiği görüntülemek için de parametresi ayarlanmış.
7. Bir tarayıcıda. Sayfayı çalıştırın. 

    ![](7-displaying-data-in-a-chart/_static/image10.jpg)

> [!TIP] 
> 
> <a id="SB_UsingStatements"></a>
> ### <a name="using-statements-and-fully-qualified-names"></a>"Deyimleri ve tam olarak nitelenmiş adlar kullanarak"
> 
> Razor sözdizimi ile ASP.NET Web sayfaları dayanır .NET Framework bileşenlerini (sınıflar) binlerce oluşur. Bu sınıfların ile çalışmak için yönetilebilir hale getirmek amacıyla halinde düzenlenmiş *ad alanları*, bakıma kitaplıkları olduğu. Örneğin, `System.Web` ad alanı, tarayıcı/sunucu iletişimi destekleyen sınıfları içerir `System.Xml` ad alanı oluşturmak ve XML dosyaları okumak için kullanılan sınıfları içerir ve `System.Data` ad alanı çalışmanıza olanak sağlayan sınıflar içerir verilerle.
> 
> .NET Framework'teki verilen herhangi bir sınıf erişmek için kod yalnızca sınıf adını değil, aynı zamanda sınıfı bir deyişle ad alanı bilmek ister. Örneğin, kullanmak için `Chart` Yardımcısı, kod gereken bulmak `System.Web.Helpers.Chart` ad birleştirir sınıfı (`System.Web.Helpers`) sınıf adıyla (`Chart`). Bu sınıfın bilinir *tam* ad &#8212; tam, anlaşılır konumuna vastness içinde .NET Framework'ün. Kod içinde bu aşağıdaki gibi görünür:
> 
> `var myChart = new System.Web.Helpers.Chart(width: 600, height: 400) // etc.`
> 
> Ancak, kullanışsız (ve hataya) için bir sınıf veya Yardımcısı başvurmak istediğinizde her zaman bu uzun, tam olarak nitelenmiş adlar kullanmak zorunda. Bu nedenle, sınıf adları kullanın kolaylaştırmak için *alma* genellikle, ilgilendiğiniz, ad alanlarının yalnızca .NET Framework birçok ad alanları kümelerini sayıda olduğu. Bir ad alanı aldıysanız, yalnızca sınıf adını kullanabilirsiniz (`Chart`) tam adı yerine (`System.Web.Helpers.Chart`). Kodunuzu çalıştırır ve bir sınıf adı karşılaştığında, yalnızca o sınıfın bulmak için aldığınız ad alanlarında bakabilirsiniz.
> 
> Web sayfaları oluşturmak için Razor sözdizimi ile ASP.NET Web sayfaları kullandığınızda, genellikle aynı kümesi sınıfların her zaman kullandığınız da dahil olmak üzere `WebPage` sınıfı, çeşitli yardımcıları ve benzeri. Bir Web sitesi oluşturmak her zaman ilgili ad alanlarını alma işi kaydetmek için ASP.NET otomatik olarak her Web sitesi için çekirdek ad alanları kümesi alır şekilde yapılandırılır. İşte bu nedenle ad alanları veya şimdi kadar; alma uğraşmanız gerekiyordu henüz üzerinde çalıştığınız tüm sınıflar, önceden içe aktarılan ad alanları arasındadır.
> 
> Ancak, bazen, sizin için otomatik olarak alınır bir ad alanında olmayan bir sınıfla çalışması gerekir. Bu durumda, o sınıfın tam adı ya da kullanabilirsiniz veya sınıfı içeren ad alanı el ile içeri aktarabilirsiniz. Bir ad alanı almak için kullandığınız `using` deyimi (`import` Visual Basic'te), bir örnek daha önce gördüğünüz gibi makaleyi.
> 
> Örneğin, `DataSet` sınıfı olan `System.Data` ad alanı. `System.Data` Ad alanı, ASP.NET Razor sayfalara otomatik olarak kullanılabilir değil. Bu nedenle, çalışmak için `DataSet` tam adını kullanarak sınıfı, aşağıdakine benzer bir kod kullanabilirsiniz:
> 
> `var dataSet = new System.Data.DataSet();`
> 
> Kullanmanız gerekiyorsa `DataSet` art arda sınıfı bu gibi bir ad alanı içe aktarabilir ve ardından kodda yalnızca sınıf adını kullanın:
> 
> [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample8.cshtml)]
> 
> Ekleyebileceğiniz `using` deyimleri için başvuru yapmak istediğinizi diğer .NET Framework ad. ASP.NET tarafından kullanılmak üzere otomatik olarak alınır ad alanları ile karşılaşmayacağınızı sınıfların çoğu olduğundan ancak belirtildiği gibi genellikle, bunu gerekmez *.cshtml* ve *.vbhtml* sayfaları.


<a id="Displaying_Charts"></a>
## <a name="displaying-charts-inside-a-web-page"></a>Bir Web sayfası içinde grafik görüntüleme

Örneklerde bir grafik oluşturabilir ve grafiğin grafik olarak tarayıcıya doğrudan sonra işlenen o ana kadarki gördünüz. Çoğu durumda, ancak bir grafik bir sayfanın parçası yalnızca tek başına tarayıcıda görüntülemek istediğiniz. Bunu yapmak için iki adımlı bir işlem gerektirir. İlk adım zaten gördüğünüz gibi grafik oluşturan bir sayfa oluşturmaktır.

İkinci adım, başka bir sayfaya elde edilen görüntü görüntülemektir. Görüntüyü görüntülemek için bir HTML kullanmak `<img>` aynı öğesi yaptığınız herhangi bir görüntü görüntülenecek şekilde. Ancak, başvuran yerine bir *.jpg* veya *.png* dosya, `<img>` öğesi başvuruları *.cshtml* içeren dosya `Chart` Yardımcısı, grafik oluşturur. Görünen sayfa çalıştığında, `<img>` öğesi çıktısını alır `Chart` Yardımcısı ve grafik işler.

![](7-displaying-data-in-a-chart/_static/image11.jpg)

1. Adlı bir dosya oluşturun *ShowChart.cshtml*.
2. Varolan içeriği aşağıdakiyle değiştirin: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample9.html)]

    Kod kullanan `<img>` , daha önce oluşturulmuş grafik görüntülemek için öğeyi *ChartArrayBasic.cshtml* dosya.
3. Web sayfasını bir tarayıcıda çalıştırın. *ShowChart.cshtml* dosyasını görüntüler içinde yer alan kodu temel grafik görüntüsünün *ChartArrayBasic.cshtml* dosya.

<a id="Styling_a_Chart"></a>
## <a name="styling-a-chart"></a>Bir grafik stil oluşturma

`Chart` Yardımcı çok sayıda grafik görünümünü özelleştirmenize olanak sağlayan seçenekleri destekler. Renkleri, yazı tipi, kenarlıklar ve benzeri ayarlayabilirsiniz. Kullanılacak bir grafiğin görünümünü özelleştirmek için kolay bir yoludur bir *tema*. Temalar nasıl yazı tiplerini, renkler, etiketler, paletleri, kenarlıklar ve efektleri kullanarak bir grafik oluşturulacağını belirten bilgi topluluklarıdır. (Grafiğinin stilini grafik türünü göstermez unutmayın.)

Aşağıdaki tabloda yerleşik Temalar listeler.

| Tema | Açıklama |
| --- | --- |
| `Vanilla` | Üzerinde beyaz bir arka plan kırmızı sütunlarını görüntüler. |
| `Blue` | Mavi gradyan arka planı sütunlarda görüntüler mavi. |
| `Green` | Yeşil gradyan arka planı sütunlarda görüntüler mavi. |
| `Yellow` | Sarı gradyan arka planı sütunlarda turuncu görüntüler. |
| `Vanilla3D` | 3-b kırmızı sütunlar üzerinde beyaz bir arka plan görüntüler. |

Yeni bir grafik oluşturduğunuzda kullanılacak Tema belirtebilirsiniz.

1. Adlı yeni bir dosya oluşturun *ChartStyleGreen.cshtml*.
2. Sayfa varolan içeriği aşağıdakiyle değiştirin:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample10.cshtml)]

    Bu kod veriler için veritabanını kullanır ancak ekler önceki örnek olarak aynıdır `theme` oluştururken parametre `Chart` nesnesi. Aşağıdaki kodu değişen gösterir:

    [!code-csharp[Main](7-displaying-data-in-a-chart/samples/sample11.cs)]
3. Bir tarayıcıda. Sayfayı çalıştırın. Önce aynı verilere bakın, ancak grafik daha çarpıcı arar: 

    ![](7-displaying-data-in-a-chart/_static/image12.jpg)

<a id="Saving_a_Chart"></a>
## <a name="saving-a-chart"></a>Bir grafik kaydetme

Kullandığınızda `Chart` yazarken yardımcı görülen şu ana kadar bu makalede, yardımcı çalıştırıldığında her zaman yeniden sıfırdan grafik oluşturur. Gerekirse, grafik için kod ayrıca veritabanını yeniden sorgular veya veri almak için XML dosyasını yeniden okur. Bazı durumlarda, bunun yapılması karmaşık bir işlem gibi olabilir sorgulamakta olduğunuz veritabanınız büyük olduğunda veya XML dosyası çok fazla veri içeriyorsa. Grafik çok miktarda veri içermeyen olsa bile, dinamik görüntü oluşturma işlemi sunucu kaynaklarını alır ve birçok kişi sayfa veya grafik görüntüleme sayfaları isterse olabilir bir etkisi Web sitenizin performansını.

Bir grafik oluşturmanın olası performans etkisini azaltmanıza yardımcı olması için bir grafik ilk zaman oluşturabilirsiniz ihtiyaç ve kaydedin. Grafik, yeniden üretmek yerine yeniden, gerekli olduğunda, yalnızca kaydedilen sürümü getirebilir ve onu dönüştürebilirsiniz.

Bir grafiği aşağıdaki şekillerde kaydedebilirsiniz:

- (Sunucu) bilgisayar belleğini grafikte önbelleğe alır.
- Grafiği bir görüntü dosyası olarak kaydedin.
- Grafiği bir XML dosyası olarak kaydedin. Bu seçenek, kaydetmeden önce grafik değiştirmenize olanak sağlar.

### <a name="caching-a-chart"></a>Bir grafik önbelleğe alma

Bir grafik oluşturduktan sonra önbelleğe alabilir. Bir grafik önbelleğe alma, onu yeniden görüntülenmesi gerekiyorsa yeniden oluşturma sahip değil anlamına gelir. Bir grafik önbellekte kaydettiğinizde, bu grafik için benzersiz olmalıdır bir anahtar verin.

Sunucu belleği azalıyor önbelleğe kaydedilen grafik kaldırılması. Ayrıca, uygulamanız için herhangi bir nedenle yeniden başlatılırsa önbellek temizlenir. Bu nedenle, önbelleğe alınan bir grafikle çalışmak için standart her zaman ilk önbellekte kullanılabilir olup olmadığını ve değilse, denetleyin ardından oluşturmak veya yeniden oluşturmak için yoludur.

1. Web sitenizin kök adlı bir dosya oluşturun *ShowCachedChart.cshtml*.
2. Varolan içeriği aşağıdakiyle değiştirin: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample12.html)]

    `<img>` Etiketi de içeren bir `src` işaret özniteliği *ChartSaveToCache.cshtml* dosyası ve bir anahtar sayfa bir sorgu dizesi olarak geçirir. Anahtar değeri içeren &quot;myChartKey&quot;. *ChartSaveToCache.cshtml* dosyasını içeren `Chart` grafik oluşturur Yardımcısı. Bu sayfayı birazdan oluşturacaksınız.

    Sayfa sonunda adlı bir sayfaya bir bağlantı yok *ClearCache.cshtml*. Ayrıca kısa süre içinde oluşturacaksınız bir sayfa olmasıdır. Gereksinim duyduğunuz *ClearCache.cshtml* yalnızca bu örnek için önbelleğe almayı test etmek için — bir bağlantı veya önbelleğe alınmış grafiklerle çalışırken normalde içerir sayfa değil.
3. Web sitenizin kök adlı yeni bir dosya oluşturun *ChartSaveToCache.cshtml*.
4. Varolan içeriği aşağıdakiyle değiştirin:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample13.cshtml)]

    Kod ilk şey sorgu dizesi anahtar değer olarak geçirilen olup olmadığını denetler. Bu nedenle, kod çağırarak önbellek dışında bir grafik okumaya çalışırsa `GetFromCache` yöntemi ve anahtar geçirme. Önbelleğinde (grafik istenen ilk kez olacağını) bu anahtarı altında bir şey var olan Öyle değilse kodu grafik her zamanki gibi oluşturur. Grafik bittiğinde kodu önbelleğe çağırarak kaydeder `SaveToCache`. Bu yöntem (grafik daha sonra istenebilir biçimde) bir anahtar ve grafik önbellekte kaydedilmesi gereken süreyi gerektirir. (Bir grafik önbelleğe tam zaman ne sıklıkta, temsil ettiği veri değişebilir zorlayıcı üzerinde bağlıdır.) `SaveToCache` Yöntemi de gerektiren bir `slidingExpiration` parametresi & # Bu ayarlanırsa 8212; true olarak zaman aşımı grafik erişildiğinde her zaman sayaç sıfırlanır. Bu durumda, bu uygulamada grafiğin önbellek girişinin birisi grafik erişilen son saatten sonra 2 dakika süresi anlamına gelir. (Kayan bitiş tarihinin mutlak zaman aşımı, önbellek girişi ne sıklıkta, erişilen olsun önbelleğine koyulmuş sonra tam olarak 2 dakika dolacağını anlamı alternatifidir.)

    Son olarak, kodu kullanan `WriteFromCache` getirin ve grafiğin önbellekten işlemek için yöntem. Bu yöntem dışında Not `if` önbellek grafik başından itibaren oluştu veya oluşturulan ve önbellekte kaydedilmesi olsa da grafiği önbellekten alır çünkü denetleyen bloğu.

    Örnekte, dikkat `AddTitle` yöntemi bir zaman damgası içerir. (Geçerli tarih ve saat &#8212;ekler; `DateTime.Now` &#8212; başlığına.)
5. Adlı yeni bir sayfa oluşturma *ClearCache.cshtml* ve içeriğini aşağıdakilerle değiştirin:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample14.cshtml)]

    Bu sayfa kullanıyor `WebCache` içinde önbelleğe grafik kaldırmaya yardımcı *ChartSaveToCache.cshtml*. Daha önce belirtildiği gibi normalde şuna benzer bir sayfa olması gerekmez. Buraya yalnızca önbelleğe almayı test etmek daha kolay oluşturmakta olduğunuz.
6. Çalıştırma *ShowCachedChart.cshtml* web sayfasını bir tarayıcıda. Sayfa içinde yer alan kodu temel grafik görüntüsünü görüntüler *ChartSaveToCache.cshtml* dosya. Grafik başlığını ne zaman damgası diyor, not edin. 

    ![Açıklama: Grafik başlığını zaman damgasına sahip temel grafik resmini](7-displaying-data-in-a-chart/_static/image13.jpg)
7. Tarayıcıyı kapatın.
8. Çalıştırma *ShowCachedChart.cshtml* yeniden. Zaman damgası aynı önceki gibi grafik yeniden, ancak bunun yerine önbelleğinden okunduğu belirten olduğuna dikkat edin.
9. İçinde *ShowCachedChart.cshtml*, tıklatın **temizleyin önbellek** bağlantı. Bu, olanak sürer *ClearCache.cshtml*, hangi raporların önbelleği temizlendi.
10. Tıklatın **dönmek için ShowCachedChart.cshtml** bağlantı ya da yeniden Çalıştır *ShowCachedChart.cshtml* webmatrix'ten. Önbellek temizlenmiş olduğundan bu zaman damgası değiştiğini, dikkat edin. Bu nedenle, kod grafiği yeniden oluşturun ve tekrar önbelleğe koyulur gerekiyordu.

### <a name="saving-a-chart-as-an-image-file"></a>Bir grafiği bir görüntü dosyası olarak kaydetme

Bir grafiği bir görüntü dosyası olarak kaydedebilirsiniz (örneğin, bir *.jpg* dosyası) sunucusunda. Bu gibi durumlarda, görüntü dosyası sonra herhangi bir görüntü yaptığınız şekilde kullanabilirsiniz. Avantajı, dosyanın depolanan yerine geçici bir önbelleğe kaydedilen olmasıdır. Yeni bir grafik görüntüsü (örneğin, her saat) farklı zamanlarda kaydedin ve zaman içinde gerçekleşen değişiklikleri kalıcı kaydını tutun. Web uygulamanıza bir dosyayı klasöre görüntü dosyasını yerleştirmek istediğiniz sunucuda kaydetme izni olduğundan emin olmalısınız unutmayın.

1. Web sitenizin kök adlı bir klasör oluşturun  *\_ChartFiles* zaten yoksa.
2. Web sitenizin kök adlı yeni bir dosya oluşturun *ChartSave.cshtml*.
3. Varolan içeriği aşağıdakiyle değiştirin:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample15.cshtml)]

    Kod ilk bakar olup olmadığını *.jpg* dosya var çağırarak `File.Exists` yöntemi. Kod dosyası mevcut değilse yeni bir oluşturur `Chart` bir diziden. Bu süre, kod çağrıları `Save` yöntemi ve geçişleri `path` parametre dosyası yolu ve dosya adını grafik kaydedileceği yeri belirtin. Sayfasının gövdesinde bir `<img>` öğesi işaret edecek şekilde yolunu kullanır *.jpg* görüntülemek için dosya.
4. Çalıştırma *ChartSave.cshtml* dosya.
5. WebMatrix için döndür. Bir görüntü dosyası adlı bildirim *chart01.jpg* kaydedilmiş  *\_ChartFiles* klasör.

### <a name="saving-a-chart-as-an-xml-file"></a>Bir grafiği bir XML dosyası olarak kaydetme

Son olarak, bir grafiği sunucuya bir XML dosyası olarak kaydedebilirsiniz. Grafiği önbelleğe kaydetmeye veya grafiği bir dosyaya kaydetmeyi üzerinden bu yöntemi kullanmanın bir avantajı, istemeniz durumunda, grafiği görüntülemeden önce XML'yi değiştirebilir ' dir. Uygulamanızı sunucuda görüntü dosyasını yerleştirmek istediğiniz klasör için okuma/yazma izinlerine sahip olması gerekir.

1. Web sitenizin kök adlı yeni bir dosya oluşturun *ChartSaveXml.cshtml*.
2. Varolan içeriği aşağıdakiyle değiştirin:

    [!code-cshtml[Main](7-displaying-data-in-a-chart/samples/sample16.cshtml)]

    Bu kod, bir XML dosyası kullanır ancak bu bir grafik önbellekte depolamak için daha önce gördüğünüzle kodu benzer. Kod ilk XML dosyasını çağırarak var olup olmadığını kontrol eder `File.Exists` yöntemi. Kod dosyası mevcut değilse yeni bir oluşturur `Chart` nesne ve dosya adı olarak geçirir `themePath` parametresi. Bu grafiğin ne olursa olsun XML dosyasında olduğuna bağlı olarak oluşturur. XML dosyası zaten mevcut değilse kod normal gibi bir grafik oluşturur ve ardından çağırır `SaveXml` kaydetmek için. Grafik kullanılarak oluşturulması `Write` yöntemi görmüş önce.

    Önbelleğe alma gösterdi sayfa gibi bu kod bir zaman damgası grafik başlığını içerir.
3. Adlı yeni bir sayfa oluşturma *ChartDisplayXMLChart.cshtml* ve aşağıdaki biçimlendirme ekleyin: 

    [!code-html[Main](7-displaying-data-in-a-chart/samples/sample17.html)]
4. Çalıştırma *ChartDisplayXMLChart.cshtml* sayfası. Grafik görüntülenir. Chart'ın başlığında zaman damgası not edin.
5. Tarayıcıyı kapatın.
6. Webmatrix'te, sağ tıklayın  *\_ChartFiles* klasörünü tıklatın **Yenile**ve klasörü açın. *XMLChart.xml* bu klasörde dosya tarafından oluşturulmuş `Chart` Yardımcısı. 

    ![Açıklama: grafik Yardımcısı tarafından oluşturulan XMLChart.xml dosyasını gösteren _ChartFiles klasör.](7-displaying-data-in-a-chart/_static/image14.jpg)
7. Çalıştırma *ChartDisplayXMLChart.cshtml* yeniden sayfa. Grafik sayfa çalıştırdığınız ilk kez olarak aynı zaman damgasını gösterir. Grafik daha önce kaydettiğiniz XML'den oluşturulan olmasıdır.
8. Webmatrix'te açın  *\_ChartFiles* klasörü ve delete *XMLChart.xml* dosya.
9. Çalıştırma *ChartDisplayXMLChart.cshtml* kez daha sayfa. Bu süre, çünkü zaman damgası güncelleştirilir `Chart` Yardımcısı XML dosyasını yeniden gerekiyordu. İsterseniz, denetleme  *\_ChartFiles* klasörü ve XML dosyasını geri olduğuna dikkat edin.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar

- [ASP.NET Web bir veritabanı ile çalışmaya giriş sayfaları](https://go.microsoft.com/fwlink/?LinkId=202893)
- [ASP.NET Web önbelleğe alma kullanarak performansı artırmak için sayfaları](https://go.microsoft.com/fwlink/?LinkId=202903)
- [Grafik sınıfı](https://msdn.microsoft.com/library/system.web.helpers.chart(v=vs.99)) (MSDN üzerinde ASP.NET Web sayfaları API Başvurusu)
