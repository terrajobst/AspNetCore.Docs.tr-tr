---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
title: Disk belleği ve sıralama raporu verilerini (VB) | Microsoft Docs
author: rick-anderson
description: Disk belleği ve sıralama iki yaygın veri çevrimiçi uygulamada görüntülenirken özellikleridir. Bu öğreticide biz sıralama ekleme bir ilk göz atalım ve...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: b895e37e-0e69-45cc-a7e4-17ddd2e1b38d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-vb
msc.type: authoredcontent
ms.openlocfilehash: c5e7e110d436caa7b7526eae105fde601367007a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887245"
---
<a name="paging-and-sorting-report-data-vb"></a>Disk belleği ve rapor verilerini (VB) sıralama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_VB.exe) veya [PDF indirin](paging-and-sorting-report-data-vb/_static/datatutorial24vb1.pdf)

> Disk belleği ve sıralama iki yaygın veri çevrimiçi uygulamada görüntülenirken özellikleridir. Bu öğreticide biz sıralama ve biz sonra gelecekteki öğreticileri oluşturacaksınız bizim raporları için disk belleği ekleme bir ilk göz atalım.


## <a name="introduction"></a>Giriş

Disk belleği ve sıralama iki yaygın veri çevrimiçi uygulamada görüntülenirken özellikleridir. Örneğin, bir çevrimiçi kitap mağazasına ASP.NET books aranırken böyle books yüzlerce olabilir, ancak sayfa başına yalnızca on eşleşmeler arama sonuçlarını listeleme rapor listeler. Ayrıca, sonuçları başlık, fiyat, sayfa sayısı, yazar adı ve benzeri göre sıralanabilir. While 23 öğreticileri nasıl oluşturulacağını raporları, ekleme, düzenleme ve verileri silme izin arabirimleri de dahil olmak üzere çeşitli incelenmesi geçmiş biz ne veri ve yalnızca sıralamak arama değil ve örnekler disk belleği biz görülen ve DetailsView ve FormView edilmiştir denetler.

Bu öğreticide sıralama ve sadece birkaç onay kutularını işaretleyerek gerçekleştirilebilir bizim raporları için disk belleği nasıl ekleneceği göreceğiz. Ne yazık ki, bu simplistic uygulaması sıralama arabirimi bir istenen için bit bırakır kendi dezavantajları sahiptir ve disk belleği yordamları büyük sonuç kümeleri disk belleği verimli bir şekilde için tasarlanmamıştır. Gelecekteki öğreticileri out-of--disk belleği ve çözümleri sıralama box sınırlamaların üstesinden gelmek nasıl inceleyeceksiniz.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>1. adım: sayfalama ekleme ve öğretici Web sayfaları sıralama

Biz Bu öğretici başlamadan önce öncelikle Bu öğretici ve sonraki üç yapmamız gereken ASP.NET sayfaları eklemek için bir dakikanızı ayırın s olanak tanır. Başlangıç adlı projeye yeni bir klasör oluşturarak `PagingAndSorting`. Ardından, aşağıdaki beş ASP.NET sayfaları tümünün ana sayfa kullanacak şekilde yapılandırılmış olan bu klasöre eklemek `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![PagingAndSorting bir klasör oluşturun ve öğretici ASP.NET sayfaları ekleme](paging-and-sorting-report-data-vb/_static/image1.png)

**Şekil 1**: PagingAndSorting bir klasör oluşturun ve öğretici ASP.NET sayfaları ekleme


Ardından, açık `Default.aspx` sayfasında ve sürükleyin `SectionLevelTutorialListing.ascx` kullanıcı denetiminden `UserControls` tasarım yüzeyine klasör. Bu kullanıcı, içinde oluşturduğumuz denetimi [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-vb.md) öğretici, site haritası numaralandırır ve madde işaretli listede geçerli bölümdeki bu öğreticiler görüntüler.


![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](paging-and-sorting-report-data-vb/_static/image2.png)

**Şekil 2**: için Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle


Disk belleği ve biz oluşturma öğreticileri sıralama görüntülemek listeyi olması için biz site haritası eklemeniz gerekir. Açık `Web.sitemap` dosya ve düzenleme, ekleme ve silme site haritası düğümü işaretleme sonra aşağıdaki biçimlendirme ekleyin:


[!code-xml[Main](paging-and-sorting-report-data-vb/samples/sample1.xml)]


![Yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir](paging-and-sorting-report-data-vb/_static/image3.png)

**Şekil 3**: yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir


## <a name="step-2-displaying-product-information-in-a-gridview"></a>2. adım: Bir GridView ürün bilgilerini görüntüleme

Biz disk belleği ve sıralama yeteneklerini gerçekten uygulamadan önce öncelikle bir standart dışı-srotable, ürün bilgilerini listeler disk belleğine alınamayan GridView oluşturmanız s olanak tanır. Bu bir görevdir biz birçok kez önce kadar bu adımları Bu öğretici seri yapılır ve tanıdık. Başlangıç açarak `SimplePagingSorting.aspx` sayfasında ve GridView denetimini ayarlama tasarımcıya araç sürükleyin kendi `ID` özelliğine `Products`. Ardından, s ProductsBLL sınıfı kullanan yeni bir ObjectDataSource oluşturun `GetProducts()` tüm ürün bilgileri döndürmek için yöntem.


![GetProducts() yöntemi kullanarak ürünlerin tamamı hakkında bilgi alın](paging-and-sorting-report-data-vb/_static/image4.png)

**Şekil 4**: GetProducts() yöntemini kullanarak ürünlerin tamamı hakkında bilgi alma


Bu rapor bir salt okunur raporu, orada s Hayır olduğundan ObjectDataSource s eşlemeniz gerekir `Insert()`, `Update()`, veya `Delete()` karşılık gelen yöntemleri `ProductsBLL` yöntemleri; bu nedenle, (hiçbiri) aşağı açılan listeden güncelleştirme, ekleme, seçin ve DELETE sekmeleri.


![Seçin (hiçbiri) seçeneğini INSERT, UPDATE aşağı açılan listesinde ve silme sekmeleri](paging-and-sorting-report-data-vb/_static/image5.png)

**Şekil 5**: seçin (hiçbiri) seçeneğini INSERT, UPDATE aşağı açılan listesinde ve silme sekmeleri


Ardından, böylece yalnızca ürün adları, üreticiler, kategoriler, Fiyatlar ve devam etmeyen durumları görüntülenir GridView s alanları Özelleştir s olanak tanır. Ayrıca, herhangi bir alan düzeyindeki biçimlendirme yapmak ücretsiz kullanımında değiştirir, ayarlama gibi `HeaderText` özellikleri veya bir para birimi olarak fiyat biçimlendirme. Bu değişikliklerden sonra GridView s bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample2.aspx)]

Şekil 6 bizim ilerleme bugüne kadarki bir tarayıcıdan görüntülendiğinde gösterir. Sayfanın tüm gösteren her s ürün adı, kategori, tedarikçi, fiyat, bir ekran ürünleri listeler ve durum dönüştürülmesine unutmayın.


[![Ürünlerinin her biri listelenir](paging-and-sorting-report-data-vb/_static/image7.png)](paging-and-sorting-report-data-vb/_static/image6.png)

**Şekil 6**: ürünlerinin her biri listelenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-and-sorting-report-data-vb/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>3. adım: Sayfalama desteği ekleme

Listeleme *tüm* bir ekranında ürünlerin veri harcadığı kullanıcı için bilgileri aşırı yol açabilir. Sonuçlar daha kolay yönetilebilir hale getirmek için biz verilerin daha küçük sayfaları verisine bölmeniz ve aynı anda bir sayfa veri adım kullanıcıya izin verebilirsiniz. Gerçekleştirmek için bu işaretlemeniz yeterli GridView s akıllı etiketinden etkinleştirmek disk belleği checkbox (Bu GridView s ayarlar [ `AllowPaging` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) için `true`).


[![Disk belleği desteği eklemek için etkinleştir disk belleği onay kutusunu işaretleyin](paging-and-sorting-report-data-vb/_static/image10.png)](paging-and-sorting-report-data-vb/_static/image9.png)

**Şekil 7**: etkinleştirmek disk belleği sayfalama desteği eklemek için onay ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-and-sorting-report-data-vb/_static/image11.png))


Disk belleği etkinleştirme ve sayfa başına gösterilecek kayıt sayısını sınırlar ekler bir *disk belleği arabirimi* GridView için. Şekil 7'de gösterilen varsayılan disk belleği, bir dizi sayfa numarası, hızlı bir şekilde verileri bir sayfadan diğerine gezinmek arkasından arabirimidir. Bu disk belleği arabirimi size, tanıdık gelecektir ve disk belleği destek geçmiş eğitimlerine DetailsView ve FormView denetimlere eklerken görülür.

DetailsView ve FormView denetimlerini yalnızca sayfa başına tek bir kaydını gösterir. GridView, ancak danışır kendi [ `PageSize` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) sayfa başına gösterilecek kaç kayıtları belirlemek için (Bu özellik varsayılan olarak 10 değeri).

Bu GridView, DetailsView ve FormView s disk belleği arabirimi aşağıdaki özellikleri kullanarak özelleştirilebilir:

- `PagerStyle` disk belleği arabirimi için stil bilgilerini gösterir; gibi ayarları belirtebilirsiniz `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`ve benzeri.
- `PagerSettings` disk belleği arabirimi işlevselliğini özelleştirebilirsiniz özelliklerinin bevy içerir; `PageButtonCount` (varsayılan değer 10) disk belleği arabiriminde görüntülenen sayısal sayfa numarası en fazla sayısını gösterir; [ `Mode` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) nasıl disk belleği arabirimi çalışır ve ayarlanabilir gösterir: 

    - `NextPrevious` İleri ya da geri aynı anda bir sayfa adım arkasından bir sonraki ve önceki düğmelerini gösterir
    - `NextPreviousFirstLast` İleri ve geri düğmelerini ek olarak, ilk ve son düğmeleri, hızlı bir şekilde veri ilk veya son sayfasına gitmek kullanıcının kapsanmaktadır
    - `Numeric` bir dizi kullanıcının herhangi bir sayfayı hemen atlamasına izin vererek, sayfa numarası gösterir
    - `NumericFirstLast` Sayfa numaraları yanı sıra, hızlı bir şekilde veri ilk veya son sayfasına gitmek kullanıcının ilk ve son düğmeleri içerir; İlk/Son düğme, yalnızca sayısal sayfa numaralarını sığamıyorsa durumunda gösterilir

Ayrıca, GridView, DetailsView ve tüm teklif FormView `PageIndex` ve `PageCount` görüntülenmesini geçerli sayfayı ve verileri sayfaların toplam sayısı sırasıyla belirten özellikleri. `PageIndex` Özelliği dizine veri'nın ilk sayfasında görüntülerken, yani 0'da, başlangıç `PageIndex` 0'a eşit. `PageCount`, diğer yandan başlatır 1 konumunda sayım, yani `PageIndex` 0 arasındaki değerleri sınırlıdır ve `PageCount - 1`.

GridView s disk belleği arabirimimizi varsayılan görünümünü iyileştirmek için bir dakikanızı ayırın s olanak tanır. Özellikle, disk belleği arabirimi sağa hizalı olan açık gri arka plana sahip s olanak tanır. GridView s aracılığıyla doğrudan bu özellikleri ayarlamak yerine `PagerStyle` özelliği, let s oluşturma bir CSS sınıfı `Styles.css` adlı `PagerRowStyle` ve atayın `PagerStyle` s `CssClass` özelliği üzerinden bizim tema. Başlangıç açarak `Styles.css` ve sınıf tanımının aşağıdaki CSS ekleme:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample3.css)]

Ardından, açık `GridView.skin` dosyasını `DataWebControls` klasör içinde `App_Themes` klasör. Biz anlatıldığı gibi *ana sayfalar ve Site gezintisi* eğitmen, dış görünüm dosyaları, bir Web denetimi için varsayılan özellik değerleri belirtmek için kullanılabilir. Bu nedenle, ayarını eklemek için var olan ayarları büyütmek `PagerStyle` s `CssClass` özelliğine `PagerRowStyle`. Ayrıca, let s yapılandırma en fazla beş sayısal sayfa düğmelerini kullanarak göstermek için disk belleği arabirimi `NumericFirstLast` disk belleği arabirimi.


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>Disk belleği kullanıcı deneyimi

Şekil 8 GridView s onay kutusunu etkinleştirin yapılan sayfalandırma kaydedildikten sonra bir tarayıcı üzerinden sitesini ziyaret ettiğinizde web sayfası gösterilir ve `PagerStyle` ve `PagerSettings` yapılandırmaları üzerinden yapıldı `GridView.skin` dosya. Not yalnızca on kayıtları gösterilir ve veri'nın ilk sayfasında görüntülediğiniz disk belleği arabirimi gösterir.


[![Disk belleği etkin yalnızca bir alt kayıtlar görüntülenir aynı anda](paging-and-sorting-report-data-vb/_static/image13.png)](paging-and-sorting-report-data-vb/_static/image12.png)

**Şekil 8**: disk belleği etkin yalnızca bir alt kayıtlar aynı anda görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-and-sorting-report-data-vb/_static/image14.png))


Kullanıcı sayfa numaraları disk belleği arabiriminde birini tıkladığında geri gönderimin ensues ve istenen sayfa s kayıtları gösteren sayfa yeniden yükler. Şekil 9, verileri son sayfasında görüntülemek üzere seçim yaptıktan sonra sonuçlarını gösterir. Son sayfasında yalnızca bir kayıt olduğuna dikkat edin; 10 kayıtlarıyla silmenizin kayıt sayfası artı bir sayfa başına sekiz sayfalarında sonuçlanan toplam 81 kayıtları olduğundan budur.


[![Bir sayfa numarası tıklayarak geri gönderimin neden olur ve kayıtların uygun bir alt gösterir](paging-and-sorting-report-data-vb/_static/image16.png)](paging-and-sorting-report-data-vb/_static/image15.png)

**Şekil 9**: bir sayfa numarası tıklayarak geri gönderimin neden olur ve, uygun alt kayıtları gösterir ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-and-sorting-report-data-vb/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>Disk belleği s sunucu tarafı iş akışı

Son kullanıcı disk belleği arabiriminde düğmesine tıkladığında, geri gönderimin ensues ve aşağıdaki sunucu tarafı iş akışı başlar:

1. GridView s (veya DetailsView veya FormView) `PageIndexChanging` olay etkinleşir
2. ObjectDataSource yeniden istekleri *tüm* BLL; verileri GridView s `PageIndex` ve `PageSize` özellik değerlerini BLL döndürülen kayıt GridView görüntülenecek gerektiğini belirlemek için kullanılır
3. GridView s `PageIndexChanged` olay etkinleşir

2. adımda ObjectDataSource tüm verileri veri kaynağından yeniden ister. Disk belleği bu stili, yaygın olarak adlandırılır *varsayılan disk belleği*, olarak s disk belleği davranışı kullanılan varsayılan olarak ayarlanırken `AllowPaging` özelliğine `true`. Yalnızca bir alt kayıtların gerçekten işlenir olsa bile HTML'e tarayıcıya gönderilen bu s varsayılan Web denetimi veri naively disk belleği, verilerin her bir sayfa için tüm kayıtları alır. Veritabanı verileri BLL veya ObjectDataSource tarafından önbelleğe alınmadığı sürece varsayılan disk belleği yeterli büyüklükte sonuç kümesi veya çok sayıda eşzamanlı kullanıcı ile web uygulamaları için çalışmaz.

Sonraki öğreticide biz nasıl uygulanacağını inceleyeceğiz *özel sayfalama*. Özel disk belleği ile hassas verileri istenen sayfa için gerekli kayıt kümesi yalnızca almaya ObjectDataSource özellikle söyleyebilirsiniz. Tahmin edebileceğiniz gibi özel sayfalama büyük ölçüde disk belleği büyük sonuç kümeleri yoluyla verimliliğini artırır.

> [!NOTE]
> Varsayılan disk belleği ile çok sayıda eşzamanlı kullanıcı yeterli büyüklükte sonuç kümeleri yoluyla veya siteler için sayfalama uygun olmamasına karşın, özel disk belleği daha fazla değişiklikleri ve uygulamak için çaba gerektirir ve (varsayılan olarak bir onay kutusu denetimi olarak kadar basit farkında olun disk belleği). Bu nedenle, varsayılan disk belleği küçük, trafiği düşük Web siteleri veya ne zaman görece küçük sonucundan disk belleği, olarak ayarlar için ideal seçim olabilir s çok daha kolay ve hızlı uygulamak.


Örneğin, biz hiçbir zaman 100'den fazla ürünleri bizim veritabanında sahip olacaksınız biliyorsanız özel sayfalama tarafından kullanılabilen en düşük performans kazancı olasılıkla uygulamak için gereken çaba tarafından uzaklık. Ancak, biz bir gün binlerce veya on binlerce ürünler, gerekecekse, *değil* özel sayfalama uygulama büyük ölçüde engel uygulamamız ölçeklenebilirliğini.

## <a name="step-4-customizing-the-paging-experience"></a>4. adım: sayfalama deneyimini özelleştirme

Veri Web denetimleri çok sayıda kullanıcı s disk belleği deneyimini geliştirmek için kullanılan özellikleri sağlar. `PageCount` Özelliği, örneğin, gösterir kaç toplam sayfa var olan, ancak `PageIndex` özelliği ziyaret geçerli sayfayı belirtir ve hızlı bir kullanıcı belirli bir sayfaya gitmek için ayarlanabilir. Kullanıcı s disk belleği deneyimini geliştirmek amacıyla bir etiket eklemek s izin bu özellikleri kullanmak nasıl çalışılacağını Web denetimini kullanıcının hangi sayfa bilgisini sayfamızı bunlar şu anda, bunları hızlı bir şekilde belirli bir sayfaya atlamak sağlayan bir DropDownList denetimi birlikte ziyaret re .

İlk olarak, bir etiket Web denetimi sayfanıza ekleyin, Ayarla kendi `ID` özelliğine `PagingInformation`ve temizleyin, `Text` özelliği. Ardından, GridView s için bir olay işleyicisi oluşturun `DataBound` olay ve aşağıdaki kodu ekleyin:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample5.vb)]

Bu olay işleyicisi atar `PagingInformation` etiket s `Text` şu anda ziyaret sayfası kullanıcı bildiren bir ileti özelliğine `Products.PageIndex + 1` kaç toplam sayfa dışında `Products.PageCount` (1 eklediğimiz `Products.PageIndex` özelliği olduğundan `PageIndex` 0'den başlayan dizine). Bu etiket s Ata seçtiğim `Text` özelliğinde `DataBound` olay işleyicisi tersine `PageIndexChanged` olay işleyicisi nedeniyle `DataBound` olay harekete veri GridView bağlıdır, ancak her zaman `PageIndexChanged` yalnızca olay işleyicisi sayfa dizini değiştirildiğinde ateşlenir. GridView başlangıçta veri ilk sayfasında bağlı olduğunda ziyaret `PageIndexChanging` olay içermiyor t yangın (ancak `DataBound` olay değil).

Bu ayrıca ile kullanıcı şimdi ziyaret ettiğiniz hangi sayfası ve kaç tane toplam sayfalarıdır var. verilerin belirten bir ileti gösterilir.


[![Geçerli sayfa numarası ve toplam sayfa sayısı görüntülenir](paging-and-sorting-report-data-vb/_static/image19.png)](paging-and-sorting-report-data-vb/_static/image18.png)

**Şekil 10**: geçerli sayfa numarası ve toplam sayfa sayısı görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-and-sorting-report-data-vb/_static/image20.png))


Etiket denetimi ek olarak, ayrıca GridView sayfa numaraları seçili şu anda görüntülenen sayfa ile birlikte listeleyen bir DropDownList denetimi ekleyin s olanak tanır. Buraya kullanıcı hızla Geçerli sayfadan diğerine yalnızca DropDownList yeni sayfa dizini'i seçerek atlayabilirsiniz emin olur. Bir DropDownList ayarı Designer'a eklemeye başlayın kendi `ID` özelliğine `PageList` ve akıllı etiket etkinleştirmek AutoPostBack seçeneğinden denetleniyor.

Ardından, geri dönüp `DataBound` olay işleyicisi ve aşağıdaki kodu ekleyin:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample6.vb)]

Bu kod öğeleri temizleyerek başlar `PageList` DropDownList. Bu bir wouldn t beklediğiniz değiştirmek için sayfa sayısı, ancak diğer kullanıcılar aynı anda sistemiyle, ekleme veya olabilir kayıtlarından kaldırma beri gereksiz, görünebilir `Products` tablo. Bu tür eklemeleri ya da silme işlemleri, verilerin sayfaların sayısını değiştirecek.

Ardından, sayfa numaralarını yeniden oluşturun ve geçerli GridView eşleyen bir sağlamak ihtiyacımız `PageIndex` varsayılan olarak seçilidir. 0'dan bir döngü ile biz bunu başarmak `PageCount - 1`, yeni bir ekleme `ListItem` her yineleme ve ayar kendi `Selected` özelliğini geçerli yineleme dizin GridView s eşitse true olarak `PageIndex` özelliği.

Son olarak, bir olay işleyicisi DropDownList s için oluşturmak ihtiyacımız `SelectedIndexChanged` kullanıcıyı seçin listeden farklı bir öğe her zaman harekete olay. Bu olay işleyicisi oluşturmak için yalnızca Tasarımcısı'nda DropDownList çift tıklatın, sonra aşağıdaki kodu ekleyin:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample7.vb)]

Şekil 11 gösterildiği gibi yalnızca GridView s değiştirme `PageIndex` özellik, veri GridView DataSet'e eklenmesine neden olur. GridView s `DataBound` olay işleyicisi, uygun DropDownList `ListItem` seçilir.


[![Otomatik olarak gerçekleştirilecek altıncı sayfa seçme sayfası 6 Aşağı açılan liste öğesi için kullanıcıdır](paging-and-sorting-report-data-vb/_static/image22.png)](paging-and-sorting-report-data-vb/_static/image21.png)

**Şekil 11**: kullanıcı olduğunu otomatik olarak gerçekleştirilecek altıncı sayfa seçme sayfası 6 Aşağı açılan liste öğesi için ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-and-sorting-report-data-vb/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>5. adım: Çift yönlü sıralama desteği ekleme

Çift yönlü sıralama destek sayfalama desteği ekleme olarak basit ekleyerek işaretlemeniz yeterli GridView s akıllı etiket etkinleştirmek sıralama seçeneğinden (GridView s ayarlar [ `AllowSorting` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) için `true`). Bu her GridView s alanlarının üstbilgilerinin LinkButtons, tıklatıldığında işleyen, geri gönderimin neden ve artan sırada tıklatılan sütuna göre sıralanmış veri döndürür. Aynı üstbilgisi LinkButton yeniden tıklatarak azalan düzende verileri yeniden sıralar.

> [!NOTE]
> Yazılan veri kümesi yerine özel bir veri erişim katmanı kullanıyorsanız, bir etkinleştirme sıralama seçeneği GridView s akıllı etiket olmayabilir. Yalnızca yerel olarak sıralama destekleyen veri kaynaklarına bağlı GridViews kullanılabilir bu onay kutusu var. ADO.NET DataTable sağlar beri yazılan veri kümesi Giden kutusu sıralama desteği sağlar. bir `Sort` yöntemi, çağrıldığında, DataTable s DataRow belirtilen ölçütü kullanarak sıralar.


DAL DAL tarafından yerel olarak iş mantığı, verileri sıralamak veya veri katmanı için sıralama bilgi geçirmek ObjectDataSource yapılandırmanız gerekecektir destek sıralama sıralanmış nesneleri döndürmezse. Biz, iş mantığı veri ve veri erişim katmanları gelecekteki bir öğreticide sıralamak nasıl ele alacağız.

Sıralama LinkButtons (mavi edilmemiş bir bağlantı ve koyu kırmızı ziyaret edilen bağlantı için) geçerli renklerini üstbilgi satırındaki arka plan rengiyle artar HTML Köprü olarak işlenir. Bunun yerine, let s mi bağımsız olarak beyaz görüntülenen tüm üstbilgisi satır bağlantılara sahip oldukları ullanıcı bırakıldı veya ziyaret. Bu aşağıdakileri ekleyerek gerçekleştirilebilir `Styles.css` sınıfı:


[!code-css[Main](paging-and-sorting-report-data-vb/samples/sample8.css)]

Bu köprüler HeaderStyle sınıfını kullanan bir öğesi içinde görüntülerken, beyaz metinlerin kullanmak için bu söz dizimini gösterir.

Bu CSS ek olarak sonra bir tarayıcı aracılığıyla sitesini ziyaret ettiğinde ekranınızı Şekil 12'ye benzer görünmelidir. Özellikle, fiyat alanı s üstbilgi bağlantısı tıklatıldıktan sonra Şekil 12 sonuçları gösterir.


[![Sonuçları artan düzende UnitPrice göre sıralanmış](paging-and-sorting-report-data-vb/_static/image25.png)](paging-and-sorting-report-data-vb/_static/image24.png)

**Şekil 12**: sonuçları sahip olan sıralanmış göre artan sırada UnitPrice ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-and-sorting-report-data-vb/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Sıralama iş akışı inceleniyor

Tüm GridView alanları BoundField, CheckBoxField TemplateField ve benzeri sahip bir `SortExpression` özellik üstbilgi bağlantı sıralama o alanı s tıklatıldığında verileri sıralamak için kullanılacak ifadeyi belirtir. GridView de sahip bir `SortExpression` özelliği. LinkButton tıklandığında sıralama üstbilgi oluşturduğunuzda, bu alan s GridView atar `SortExpression` değerini kendi `SortExpression` özelliği. Ardından, verileri ObjectDataSource yeniden alınır ve GridView s göre sıralanmış `SortExpression` özelliği. Aşağıdaki listede, son kullanıcının GridView verileri sıralar yükleyen transpires adımların sırasını Ayrıntılar verilmiştir:

1. GridView s [olayını](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) etkinleşir
2. GridView s [ `SortExpression` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) ayarlanır `SortExpression` alanının LinkButton tıklattınız sıralama üstbilgisi
3. ObjectDataSource tüm BLL verileri yeniden alır ve GridView s kullanarak verileri sıralar `SortExpression`
4. GridView s `PageIndex` özelliği 0 olarak sıfırlanır, kullanıcı sıralarken anlamına gelir (sayfalama desteğini uygulanan varsayılarak) verileri ilk sayfasına döndürülür
5. GridView s [ `Sorted` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) etkinleşir

Varsayılan disk belleği ile varsayılan seçeneği sıralamayı yeniden alır gibi *tüm* BLL kayıtlarını. Disk belleği olmadan sıralama kullanırken ya da sıralama kullanırken disk belleği, burada s (veritabanı verileri önbelleğe alma eksikliği) ulaştı. Bu performans sağlamasına hiçbir şekilde varsayılan. Ancak, bir sonraki öğreticide anlatıldığı gibi verileri özel disk belleği kullanırken verimli bir şekilde sıralamak olası s.

GridView GridView s akıllı etiket aşağı açılan listesinde aracılığıyla bir ObjectDataSource bağlama, her GridView alan otomatik olarak sahip kendi `SortExpression` veri alanında adını atanan özellik `ProductsRow` sınıfı. Örneğin, `ProductName` BoundField s `SortExpression` ayarlanır `ProductName`aşağıdaki bildirim temelli biçimlendirmede gösterildiği gibi:


[!code-aspx[Main](paging-and-sorting-report-data-vb/samples/sample9.aspx)]

Bir alan yapılandırılabilir böylece bu s Temizleme tarafından sıralanamaz kendi `SortExpression` özelliği (boş bir dize atama). Bunu göstermek için düşünün biz etmedi t istediğiniz fiyat tarafından Ürünlerimiz sıralama müşterilerimizin izin vermek. `UnitPrice` BoundField s `SortExpression` özellik tanımlayıcı biçimlendirme veya (GridView s akıllı etiket Düzenle sütunları bağlantıya tıklayarak erişilebilir olan) alanları iletişim kutusu üzerinden kaldırılabilir.


![Sonuçları artan düzende UnitPrice göre sıralanmış](paging-and-sorting-report-data-vb/_static/image27.png)

**Şekil 13**: sonuçları artan düzende UnitPrice göre sıralanmış


Bir kez `SortExpression` özelliği için kaldırılmıştır `UnitPrice` BoundField, üstbilgi metin olarak değil, böylece kullanıcılar tarafından fiyat verileri sıralama engelleyen bir bağlantı olarak işlenir.


[![SortExpression özelliğine kaldırarak, kullanıcılar artık fiyat ürünler sıralayabilirsiniz](paging-and-sorting-report-data-vb/_static/image29.png)](paging-and-sorting-report-data-vb/_static/image28.png)

**Şekil 14**: SortExpression özelliğine kaldırarak, kullanıcılar artık tarafından Ürünleri fiyat sıralayabilirsiniz ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-and-sorting-report-data-vb/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>Program aracılığıyla GridView sıralama

GridView s kullanarak GridView içeriğini programlı olarak sıralayabilirsiniz [ `Sort` yöntemi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Yalnızca geçirin `SortExpression` ile birlikte göre sıralamak için değer [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` veya `Descending`), ve GridView s verileri yeniden sıralanmış olacaktır.

Nedeni biz sıralama dışı açık olduğunu düşünün `UnitPrice` müşterilerimizin, yalnızca en düşük fiyatlı ürünlerini yalnızca satın kaygılı bulamadığımız için oluştu. Ancak, bunları d biz ürünleri için en az fiyat, ancak yalnızca en pahalı fiyatından sıralamak için bunları istediğiniz şekilde en pahalı ürünleri satın teşvik istiyoruz.

Bunu gerçekleştirmek için bir düğme Web denetimi sayfasına ekleyin ayarlamak kendi `ID` özelliğine `SortPriceDescending`ve kendi `Text` fiyat sıralama özelliği. Ardından, olay işleyicisi için s düğme oluşturma `Click` Tasarımcısı'nda düğme denetimi çift tıklatarak olay. Bu olay işleyicisi için aşağıdaki kodu ekleyin:


[!code-vb[Main](paging-and-sorting-report-data-vb/samples/sample10.vb)]

Bu düğmeye tıkladığınızda kullanıcı ilk sayfasına fiyatından, ucuz (bkz. Şekil 15) en pahalı göre sıralanmış ürünlerle döndürür.


[![Düğmesini tıklatarak siparişleri en pahalı ürünleri için en az](paging-and-sorting-report-data-vb/_static/image32.png)](paging-and-sorting-report-data-vb/_static/image31.png)

**Şekil 15**: düğmesini tıklatarak siparişleri ürünleri gelen en pahalı için en az ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-and-sorting-report-data-vb/_static/image33.png))


## <a name="summary"></a>Özet

Disk belleği ve sıralama yeteneklerini varsayılan uygulama gördüğümüz Bu öğreticide, her ikisi de bir onay kutusu denetimi olarak kadar kolay! Bir kullanıcı sıralar veya data sayfaları, benzer bir iş akışı açılan:

1. Bir geri gönderme ensues
2. Web denetimi s veri önceden olay ateşlenir düzey (`PageIndexChanging` veya `Sorting`)
3. Tüm verilerin yeniden alınır ObjectDataSource tarafından
4. Web denetimi s veri sonrası olay ateşlenir düzey (`PageIndexChanged` veya `Sorted`)

Temel disk belleği ve sıralama uygulama çok kolay olsa da, daha fazla çaba daha verimli özel sayfalama kullanmaya ya da daha fazla disk belleği veya sıralama arabirimi geliştirmek için exerted gerekir. Sonraki öğreticileri, bu konularda inceleyeceksiniz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](creating-a-customized-sorting-user-interface-cs.md)
> [sonraki](efficiently-paging-through-large-amounts-of-data-vb.md)
