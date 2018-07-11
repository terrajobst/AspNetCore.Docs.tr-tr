---
uid: web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
title: Rapor sayfalama ve sıralama (C#) veri | Microsoft Docs
author: rick-anderson
description: Sayfalama ve sıralama iki yaygın veri çevrimiçi uygulamada görüntülenirken özellikleridir. Bu öğreticide size sıralama ekleme sırasında ilk göz atacağız ve...
ms.author: aspnetcontent
ms.date: 08/15/2006
ms.assetid: 811a6ef2-ec66-4c8e-a089-6f795056e288
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/paging-and-sorting-report-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 741404bda11fd1d5776a7493b95ffe5d0c61fce2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819351"
---
<a name="paging-and-sorting-report-data-c"></a>Rapor verilerini (C#) sayfalama ve sıralama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_24_CS.exe) veya [PDF olarak indirin](paging-and-sorting-report-data-cs/_static/datatutorial24cs1.pdf)

> Sayfalama ve sıralama iki yaygın veri çevrimiçi uygulamada görüntülenirken özellikleridir. Bu öğreticide size sıralama ve ardından sonraki öğreticiler oluşturulacak raporlarımızla için disk belleği ekleme sırasında ilk göz atacağız.


## <a name="introduction"></a>Giriş

Sayfalama ve sıralama iki yaygın veri çevrimiçi uygulamada görüntülenirken özellikleridir. Örneğin, bir çevrimiçi kitaplığı, ASP.NET books için arama yaparken böyle books yüzlerce olabilir, ancak arama sonuçlarını listeleme rapor sayfa başına yalnızca on eşleşmeleri listeler. Ayrıca, sonuçları, başlık, fiyat, sayfa sayısı, yazarın adı ve benzeri göre sıralanabilir. While 23 öğreticiler raporları, ekleme, düzenleme ve verileri silme izin arabirimleri de dahil olmak üzere çeşitli oluşturma incelenirken son biz ne veri ve yalnızca sıralamak Aranan değil ve örnekler sayfalama biz görülen ve DetailsView ve FormView ile yapıldı denetimler.

Bu öğreticide yalnızca birkaç onay kutularını işaretleyerek gerçekleştirilebilir raporlarımızla için sayfalama ve sıralama ekleme göreceğiz. Ne yazık ki, sıralama arabirimi istenen için biraz bırakır bunun dezavantajlarını bu basitleştirilmiş bir uygulamaya sahiptir ve disk belleği yordamlar büyük sonuç kümelerini etkili bir şekilde sayfalama için tasarlanmamıştır. Sonraki öğreticiler,-sayfalama ve sıralama çözümleri hazır sınırlamaları üstesinden nasıl inceleyeceksiniz.

## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>1. adım: sayfalama ekleme ve öğretici Web sayfaları sıralama

Biz bu öğreticiye başlamadan önce öncelikle Bu öğretici ve sonraki üç gerekir ASP.NET sayfaları eklemek için bir dakikanızı ayırın s olanak tanır. Adlı projede yeni bir klasör oluşturarak başlayın `PagingAndSorting`. Ardından, tüm bunları ana sayfaya kullanacak şekilde yapılandırılmış olması ve bu klasörü için aşağıdaki beş ASP.NET sayfaları ekleyin `Site.master`:

- `Default.aspx`
- `SimplePagingSorting.aspx`
- `EfficientPaging.aspx`
- `SortParameter.aspx`
- `CustomSortingUI.aspx`


![PagingAndSorting bir klasör oluşturun ve öğretici ASP.NET sayfaları ekleme](paging-and-sorting-report-data-cs/_static/image1.png)

**Şekil 1**: PagingAndSorting bir klasör oluşturun ve öğretici ASP.NET sayfaları ekleme


Ardından, açık `Default.aspx` sürükleyin ve sayfa `SectionLevelTutorialListing.ascx` kullanıcı denetimi `UserControls` tasarım yüzeyine klasör. Bu kullanıcı, oluşturduğumuz denetimini [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-cs.md) öğretici, site haritası numaralandırır ve öğreticilerle geçerli bir madde işaretli liste bölümünde görüntüler.


![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](paging-and-sorting-report-data-cs/_static/image2.png)

**Şekil 2**: Default.aspx için SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle


Madde işaretli liste sayfalama ve sıralama biz oluşturursunuz öğreticileri görüntülemek için site eşlemesinin ekleneceği gerekiyor. Açık `Web.sitemap` dosya ve düzenleme, ekleme ve silme site haritası düğüm biçimlendirme sonra aşağıdaki işaretlemeyi ekleyin:


[!code-xml[Main](paging-and-sorting-report-data-cs/samples/sample1.xml)]


![Yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir](paging-and-sorting-report-data-cs/_static/image3.png)

**Şekil 3**: yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir


## <a name="step-2-displaying-product-information-in-a-gridview"></a>2. adım: GridView ürün bilgilerini görüntüleme

Biz aslında sayfalama ve sıralama yetenekleri uygulamadan önce ilk olarak bir standart olmayan-srotable, ürün bilgilerini listeler alınamayan GridView oluşturmanız s olanak tanır. Bu görev, size birçok kez önce kadar bu adımları Bu öğretici serisinin bitti ve tanıdık. Başlangıç açarak `SimplePagingSorting.aspx` sayfasında ve ayar Tasarımcısı araç kutusundan bir GridView denetimi sürükleyin, `ID` özelliğini `Products`. Ardından, s ProductsBLL sınıfı kullanan yeni bir ObjectDataSource oluşturun `GetProducts()` tüm ürün bilgileri döndürmek için yöntemi.


![Tüm ürünleri GetProducts() yöntemi kullanma hakkında bilgi alın](paging-and-sorting-report-data-cs/_static/image4.png)

**Şekil 4**: tüm ürünleri GetProducts() yöntemi kullanma hakkında bilgi alma


Bu rapor bir salt okunur rapor, orada s Hayır olduğundan ObjectDataSource s harita gerek `Insert()`, `Update()`, veya `Delete()` karşılık gelen yöntemleri `ProductsBLL` yöntemleri; (hiçbiri) güncelleştirme, ekleme, aşağı açılan listeden bu nedenle, seçin ve DELETE sekmeler.


![Seçin (hiçbiri) seçeneğini INSERT, UPDATE, aşağı açılan listesinde ve sekmeleri Sil](paging-and-sorting-report-data-cs/_static/image5.png)

**Şekil 5**: seçin (hiçbiri) seçeneğini INSERT, UPDATE, aşağı açılan listesinde ve sekmeleri Sil


Ardından, böylece yalnızca ürün adları, üreticiler, kategoriler, fiyatları ve artık sağlanmayan durumları görüntülenen GridView s alanları özelleştirebilir s olanak tanır. Ayrıca, herhangi bir alan düzeyindeki biçimlendirme yapma ücretsiz kullanım değişikliği, ayarlama gibi `HeaderText` özellikleri veya fiyat bir para birimi olarak biçimlendirme. GridView s bildirim temelli biçimlendirme bu değişikliklerden sonra aşağıdakine benzer görünmelidir:


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample2.aspx)]

Şekil 6 ilerlememizin şimdiye kadarki bir tarayıcıdan görüntülendiğinde gösterir. Sayfa her s ürün adı, kategori, tedarikçi, fiyat, gösteren bir ekran ürünleri listeler ve durum kullanımdan unutmayın.


[![Her ürün listelenir](paging-and-sorting-report-data-cs/_static/image7.png)](paging-and-sorting-report-data-cs/_static/image6.png)

**Şekil 6**: her ürün listelenir ([tam boyutlu görüntüyü görmek için tıklatın](paging-and-sorting-report-data-cs/_static/image8.png))


## <a name="step-3-adding-paging-support"></a>3. adım: Disk belleği desteği ekleme

Listeleme *tüm* ürünlerinin bir ekrandaki verileri harcadığı kullanıcının bilgilerin aşırı yol açabilir. Sonuçları daha kolay yönetilebilir hale getirmek için size daha küçük veri sayfasını verileri bölün ve bir kerede veri bir sayfadan adım izin verin. Gerçekleştirmek için bu işaretleyerek GridView s akıllı etiket etkinleştirme sayfalama onay (Bu ayarlar GridView s [ `AllowPaging` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowpaging.aspx) için `true`).


[![Disk belleği desteği eklemek için etkin disk belleği onay kutusunu işaretleyin](paging-and-sorting-report-data-cs/_static/image10.png)](paging-and-sorting-report-data-cs/_static/image9.png)

**Şekil 7**: etkinleştirme sayfalama sayfalama desteği eklemek için onay ([tam boyutlu görüntüyü görmek için tıklatın](paging-and-sorting-report-data-cs/_static/image11.png))


Disk belleği etkinleştirme ve sayfa başına gösterilecek kayıt sayısını sınırlayan ekler bir *sayfalama arabirimi* GridView için. Şekil 7'de gösterilen varsayılan sayfalama arabirimi, sayfa numarası, kullanıcının hızlı bir şekilde verileri bir sayfadan diğerine giderler izin dizisidir. Disk belleği bu arabirimi olarak, tanıdık ve disk belleği desteği son öğreticilerde DetailsView ve FormView denetimlere eklerken görülen.

FormView hem DetailsView denetimlerini yalnızca sayfa başına tek bir kaydı gösterir. GridView ancak danışır kendi [ `PageSize` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.pagesize.aspx) sayfa başına gösterilecek kaç kayıtları belirlemek için (Bu özellik varsayılan olarak 10 değeri).

Bu GridView ve DetailsView FormView s sayfalama arabirimi aşağıdaki özellikleri kullanarak özelleştirilebilir:

- `PagerStyle` disk belleği arabirimi için stil bilgilerini gösterir. gibi ayarları belirtebilirsiniz `BackColor`, `ForeColor`, `CssClass`, `HorizontalAlign`ve benzeri.
- `PagerSettings` disk belleği arabiriminin işlevselliğini özelleştirebilirsiniz özelliklerinin bir bevy içerir. `PageButtonCount` sayısı (varsayılan: 10) disk belleği arabiriminde görüntülenen sayısal sayfa numaralarını gösterir; [ `Mode` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pagersettings.mode.aspx) sayfalama arabirimi nasıl çalışır ve ayarlanabilir gösterir: 

    - `NextPrevious` kullanıcının ileteceğini veya geriye doğru bir şekilde bir kerede bir sayfa adım izin sonraki ve önceki bir düğme gösterilmektedir
    - `NextPreviousFirstLast` Sonraki ve önceki düğmeleri ek olarak, ilk ve son düğmeleri de, hızlı bir şekilde veri ilk veya son sayfasına gitmek kullanıcının dahildir
    - `Numeric` bir dizi kullanıcının herhangi bir sayfa hemen atlamasına izin vererek, sayfa numarası gösterir
    - `NumericFirstLast` Sayfa numaralarını yanı sıra hızlı bir şekilde veri ilk veya son sayfasına gitmek kullanıcının ilk ve son düğmeleri içerir; İlk/Son düğme, yalnızca sayısal sayfa numaralarını sığamıyorsa varsa gösterilir

Ayrıca, GridView DetailsView ve tüm teklif FormView `PageIndex` ve `PageCount` görüntülenmekte olan geçerli sayfayı ve toplam sayısı, veri sayfaları sırasıyla gösteren Özellikler. `PageIndex` Özelliği veri'nın ilk sayfasında görüntülerken, yani 0'da başlangıç tarihine `PageIndex` 0 eşit olacaktır. `PageCount`, diğer taraftan, başlar sayım 1, güncelleştirmeyeceği `PageIndex` 0 arasındaki değerleri sınırlıdır ve `PageCount - 1`.

GridView s sayfalama arabirimimizi varsayılan görünümünü iyileştirmek için bir dakikanızı ayırın s olanak tanır. Özellikle, disk belleği arabirimi sağa hizalı açık gri arka plana olan s olanak tanır. GridView s aracılığıyla doğrudan bu özellikleri ayarlamak yerine `PagerStyle` özelliği, let s oluşturmak, bir CSS sınıfı `Styles.css` adlı `PagerRowStyle` atayın `PagerStyle` s `CssClass` özelliği aracılığıyla bizim tema. Başlangıç açarak `Styles.css` ve sınıf tanımını aşağıdaki CSS ekleme:


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample3.css)]

Ardından, açık `GridView.skin` dosyası `DataWebControls` klasördeki `App_Themes` klasör. Açıkladığımız gibi *ana sayfalar ve Site gezintisi* eğitmen, dış görünüm dosyaları, Web denetimi için varsayılan özellik değerlerini belirtmek için kullanılabilir. Bu nedenle, ayarı eklemek için var olan ayarları büyütmek `PagerStyle` s `CssClass` özelliğini `PagerRowStyle`. Ayrıca, let s yapılandırma en fazla beş sayısal sayfa düğmelerini kullanarak göstermek için disk belleği arabirimi `NumericFirstLast` sayfalama arabirimi.


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample4.aspx)]

## <a name="the-paging-user-experience"></a>Disk belleği kullanıcı deneyimi

Şekil 8 GridView s sayfalama etkinleştir onay kutusunu kaydedildikten sonra bir tarayıcıdan ziyaret edildiğinde bir web sayfası gösterilir ve `PagerStyle` ve `PagerSettings` yapılandırmaları aracılığıyla yapıldı `GridView.skin` dosya. Not yalnızca on kayıt gösterilir ve veri'nın ilk sayfasında görüntülüyorsunuz sayfalama arabirimi gösterir.


[![Disk belleği etkin yalnızca bir alt kayıtları görüntülenir aynı anda](paging-and-sorting-report-data-cs/_static/image13.png)](paging-and-sorting-report-data-cs/_static/image12.png)

**Şekil 8**: disk belleği etkin yalnızca bir alt kayıtları birer görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](paging-and-sorting-report-data-cs/_static/image14.png))


Kullanıcı, bir disk belleği arabiriminde sayfa numaralarını tıkladığında bir geri gönderme ensues ve istenen sayfa s kayıtları gösteren sayfayı yeniden yükler. Şekil 9, verilerinizin nihai sayfasını görüntülemek için katılmamayı seçtikten sonra sonuçları gösterilmektedir. Son sayfa yalnızca bir kayıtla olduğuna dikkat edin; Toplam sekiz sayfalarında silmenizin kaydını 10 kayıt sayfasına ek bir sayfa başına sonuç, 81 kayıtları olduğundan budur.


[![Bir sayfa numarası tıklayarak geri göndermeye neden olur ve uygun bir alt kayıtları gösterir](paging-and-sorting-report-data-cs/_static/image16.png)](paging-and-sorting-report-data-cs/_static/image15.png)

**Şekil 9**: sayfa sayısına tıklayarak geri göndermeye neden olur ve, uygun alt kayıtları gösterir ([tam boyutlu görüntüyü görmek için tıklatın](paging-and-sorting-report-data-cs/_static/image17.png))


## <a name="paging-s-server-side-workflow"></a>Disk belleği s sunucu tarafı iş akışı

Son kullanıcı disk belleği arabiriminde bir düğmeye tıkladığında bir geri gönderme ensues ve aşağıdaki sunucu tarafı iş akışı başlar:

1. GridView s (veya DetailsView veya FormView) `PageIndexChanging` olay harekete geçirilir
2. ObjectDataSource yeniden istekleri *tüm* BLL; verilerin GridView s `PageIndex` ve `PageSize` BLL döndürülen kayıtları GridView içinde görüntülenecek gerektiğini belirlemek için kullanılan özellik değerleri
3. GridView s `PageIndexChanged` olay harekete geçirilir

Adım 2'de ObjectDataSource tüm verileri veri kaynağından yeniden ister. Disk belleği bu stilini, yaygın olarak adlandırılır *varsayılan sayfalama*, s sayfalama davranışını kullanıldıkları varsayılan olarak ayarlanırken `AllowPaging` özelliğini `true`. Yalnızca bir alt kayıtları gerçekten işlenen olsa bile HTML'e tarayıcıya gönderilen, s ile varsayılan Web denetimi veri naively disk belleği, verilerin her sayfa için tüm kayıtları alır. Veritabanı verilerini BLL veya ObjectDataSource tarafından önbelleğe alınmadığı sürece varsayılan disk belleği çok sayıda eşzamanlı kullanıcıyı ile web uygulamaları veya yeterince büyük sonuç kümeleri için çalışmaz.

Sonraki öğreticide nasıl uygulanacağı inceleyeceğiz *özel disk belleği*. Özel disk belleği ile ObjectDataSource yalnızca hassas verilerin istenen sayfa için gerekli kayıt kümesini almak için özel olarak bildirebilirsiniz. Tahmin edebileceğiniz gibi özel disk belleği büyük ölçüde büyük sonuç kümesi üzerinden disk belleği verimliliğini artırır.

> [!NOTE]
> Varsayılan disk belleği yeterince büyük sonuç kümeleri aracılığıyla veya siteler için çok sayıda eşzamanlı kullanıcı sayfalama uygun olsa da, özel disk belleği daha fazla değişiklikleri ve uygulamak için çaba gerektirir ve bir onay kutusu denetimi (varsayılan değer olarak kadar basit değil fark disk belleği). Bu nedenle, varsayılan sayfalama küçük, trafik düzeyi düşük Web siteleri veya ne zaman görece küçük sonuçlarından disk belleği, olarak ayarlar için ideal seçim olabilir s çok daha kolay ve hızlı uygulamak.


Örneğin, biz size hiçbir zaman 100'den fazla ürünleri veritabanımızda yer sahip biliyorsanız, özel disk belleği tarafından kullanılabilen en az bir performans kazancı büyük olasılıkla uygulamak için gereken çabayı tarafından uzaklığı. Ancak, bir gün binlerce veya on binlerce ürünleri sahibiz, *değil* özel sayfalama uygulama büyük ölçüde engel uygulamamız ölçeklenebilirliğini.

## <a name="step-4-customizing-the-paging-experience"></a>4. adım: disk belleği deneyimini özelleştirme

Veri Web denetimleri, bir dizi kullanıcı s sayfalama deneyimini geliştirmek için kullanılan özellikleri sağlar. `PageCount` Özelliği, örneğin, belirtir kaç toplam sayfa vardır, ancak `PageIndex` özelliği ziyaret geçerli sayfayı belirtir ve bir kullanıcı belirli bir sayfaya hızlı bir şekilde taşımak için ayarlanabilir. Bu özellikleri kullanıcı s sayfalama deneyimini iyileştirmek, bir etiket ekleyin s izin nasıl kullanılacağını göstermek için Web denetim kullanıcıya hangi sayfanın bildirir sayfamızı bunlar şu anda, belirli bir sayfaya hızlı bir şekilde atlamak izin veren bir DropDownList denetimi ziyaret re .

İlk olarak, sayfanıza bir etiket Web denetimi ekleyin, kendi `ID` özelliğini `PagingInformation`ve temizleyin, `Text` özelliği. Ardından, GridView s için bir olay işleyicisi oluşturun `DataBound` olay ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample5.cs)]

Bu olay işleyicisi atar `PagingInformation` etiket s `Text` şu anda ziyaret sayfası kullanıcı bildiren bir ileti özelliğine `Products.PageIndex + 1` kaç toplam sayfa dışında `Products.PageCount` (1 eklediğimiz `Products.PageIndex` özelliği olduğundan `PageIndex` 0'dan başlayan dizine alınır). Ata bu etiketi s seçtiğim `Text` özelliğinde `DataBound` olay işleyicisi olarak `PageIndexChanged` olay işleyicisi nedeniyle `DataBound` olayı tetikler veri GridView'a bağlıdır, ancak her zaman `PageIndexChanged` yalnızca olay işleyicisi sayfa dizini değiştirildiğinde ateşlenir. GridView başlangıçta ilk sayfasında veriye olduğunda ziyaret `PageIndexChanging` olay eklenmemişse t Ateş (ise `DataBound` olay yok).

Bu eklenmesiyle, kullanıcı artık hangi sayfa, ziyaret ettiğiniz ve var. verilerin toplam kaç sayfalar belirten bir ileti gösterilir.


[![Geçerli sayfa numarası ve toplam sayfa sayısı görüntülenir.](paging-and-sorting-report-data-cs/_static/image19.png)](paging-and-sorting-report-data-cs/_static/image18.png)

**Şekil 10**: geçerli sayfa numarası ve toplam sayfa sayısı görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](paging-and-sorting-report-data-cs/_static/image20.png))


Etiket denetimine ek olarak, ayrıca seçili şu anda görüntülenen sayfa ile GridView sayfa numaraları listeleyen bir DropDownList denetimi ekleyin, s olanak tanır. Buraya kullanıcı hızla Geçerli sayfadan diğerine yalnızca DropDownList'e yeni sayfa dizini'i seçerek atlayabilirsiniz emin olur. Bir DropDownList Tasarımcı ayarı ekleyerek başlayın, `ID` özelliğini `PageList` ve akıllı etiketinde AutoPostBack Etkinleştir seçeneği denetleniyor.

Ardından, dönmek `DataBound` olay işleyicisi ve aşağıdaki kodu ekleyin:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample6.cs)]

Bu kod öğeleri temizleyerek başlar `PageList` DropDownList. Bu bir wouldn t değiştirmek için sayfa sayısını bekler, ancak diğer kullanıcılar sistem aynı anda kullanarak, ekleme veya olabilir kayıtları kaldırarak bu yana gereksiz, görünebilir `Products` tablo. Veri sayfa sayısı gibi eklemeler ve silmeleri değiştirecek.

Ardından, geçerli GridView'a eşleyen bir sayfa numaralarını yeniden oluşturun ve ihtiyacımız `PageIndex` varsayılan olarak seçilidir. Biz bunu bir döngüden kurtulmak için 0 ile gerçekleştirmek `PageCount - 1`, yeni bir ekleme `ListItem` her yineleme ve ayarı kendi `Selected` özelliğini geçerli yineleme dizini GridView s eşitse true olarak `PageIndex` özelliği.

Son olarak, DropDownList s için bir olay işleyicisi oluşturmak ihtiyacımız `SelectedIndexChanged` olayı kullanıcıyı seçin listeden farklı bir öğe her değişiminde tetikler. Bu olay işleyicisi oluşturmak için yalnızca Tasarımcısı'nda DropDownList çift tıklayın, sonra aşağıdaki kodu ekleyin:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample7.cs)]

Şekil 11 gösterildiği gibi yalnızca GridView s değiştirme `PageIndex` özellik verileri GridView'a DataSet'e neden olur. GridView s `DataBound` olay işleyicisi, uygun DropDownList `ListItem` seçilir.


[![Kullanıcı otomatik olarak yapılan altıncı sayfası seçme sayfası 6 Aşağı açılan liste öğesi için](paging-and-sorting-report-data-cs/_static/image22.png)](paging-and-sorting-report-data-cs/_static/image21.png)

**Şekil 11**: kullanıcı, otomatik olarak yapılan altıncı sayfası seçme sayfası 6 Aşağı açılan liste öğesi için ([tam boyutlu görüntüyü görmek için tıklatın](paging-and-sorting-report-data-cs/_static/image23.png))


## <a name="step-5-adding-bi-directional-sorting-support"></a>5. adım: İki yönlü sıralama desteği ekleme

Ekleme yönlü sıralama destek sayfalama desteğini kadar basittir GridView s akıllı etiket sıralama etkinleştir seçeneğini işaretleyerek (GridView s ayarlar [ `AllowSorting` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.allowsorting.aspx) için `true`). Bu her GridView s alanlarının üst bilgi LinkButtons, tıklandığında işleyen bir geri göndermeye neden olur ve artan düzende tıklandı sütuna göre sıralanmış verileri döndürür. LinkButton aynı üst bilgiyi yeniden tıklayarak verileri azalan düzende yeniden sıralar.

> [!NOTE]
> Türü belirtilmiş veri kümesi yerine özel bir veri erişim katmanı kullanıyorsanız, bir etkinleştirme sıralama seçeneği GridView s akıllı etiket olmayabilir. Yalnızca yerel olarak sıralama destekleyen veri kaynaklarına bağlı GridViews kullanılabilir bu onay kutusu var. Türü belirtilmiş veri kümesi sağlayan ADO.NET DataTable kullanıma hazır sıralama desteği sağlanamadığından bir `Sort` yöntem, çağrıldığında DataTable s belirtilen ölçütü kullanarak sıralar.


DAL DAL tarafından yerel olarak iş mantığı olan verileri sıralamak veya veri katmanı için sıralama bilgilerini geçirmek için ObjectDataSource yapılandırmanız gerekecektir destek sıralama sıralanmış nesnelerin döndürmezse. Biz, bir sonraki öğreticide, verileri iş mantığı ve veri erişim katmanları sıralama hakkında bilgi edineceksiniz.

Sıralama LinkButtons geçerli renklerini (ziyaret edilmemiş bir bağlantı ve ziyaret edilmiş bağlantı için koyu kırmızı mavi) için başlık satırındaki arka plan rengi ile çakışır HTML Köprü olarak işlenir. Bunun yerine, let s Beyaz, bağımsız olarak görüntülenen tüm üst bilgi satırı bağlantılara sahip oldukları ve silinmiş veya ziyaret. Bu aşağıdaki ekleyerek gerçekleştirilebilir `Styles.css` sınıfı:


[!code-css[Main](paging-and-sorting-report-data-cs/samples/sample8.css)]

Beyaz metinli köprülere HeaderStyle sınıfını kullanan bir öğe içinde görüntülenirken kullanmak için şu söz dizimini gösterir.

Bu CSS ekleme sonra sayfanın tarayıcısından ziyaret edildiğinde ekranınız Şekil 12'ye benzer görünmelidir. Özellikle, fiyat alanı s üstbilgi bağlantısı tıklatıldıktan sonra Şekil 12 sonuçları gösterilmektedir.


[![Sonuçları artan düzende UnitPrice göre sıralanmış](paging-and-sorting-report-data-cs/_static/image25.png)](paging-and-sorting-report-data-cs/_static/image24.png)

**Şekil 12**: sonuçları sahip olan göre sıralanacağını UnitPrice artan sırada ([tam boyutlu görüntüyü görmek için tıklatın](paging-and-sorting-report-data-cs/_static/image26.png))


## <a name="examining-the-sorting-workflow"></a>Sıralama iş akışı İnceleme

GridView'ın tüm alanları BoundField, CheckBoxField TemplateField ve benzeri sahip bir `SortExpression` özellik başlığı bağlantı sıralama Bu alan s tıklandığında verileri sıralamak için kullanılacak ifadeyi belirtir. GridView de sahip bir `SortExpression` özelliği. LinkButton tıklandığında bir sıralama üst bilgisi, bu alan s GridView atar `SortExpression` değerini kendi `SortExpression` özelliği. Ardından, verileri yeniden ObjectDataSource alınan ve GridView s göre sıralanmış `SortExpression` özelliği. Aşağıdaki listede, son kullanıcı GridView verileri sıralar yükleyen transpires adımlar dizisini Ayrıntılar:

1. GridView s [sıralama olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorting(VS.80).aspx) etkinleşir
2. GridView s [ `SortExpression` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) ayarlanır `SortExpression` alan Linkbutton'a tıklandı sıralama üstbilgisi
3. ObjectDataSource tüm BLL verileri yeniden alır ve ardından verileri GridView s kullanarak sıralar `SortExpression`
4. GridView s `PageIndex` özelliği 0 olarak sıfırlama, kullanıcı sıralarken anlamına gelir (disk belleği desteği uygulanan varsayılarak) verilerin ilk sayfaya döndürülür
5. GridView s [ `Sorted` olay](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sorted(VS.80).aspx) etkinleşir

Varsayılan Sayfalamayı ile yeniden sıralama seçeneği varsayılan alır gibi *tüm* BLL kayıtlarını. Disk belleği olmayan sıralama kullanırken veya sıralama ile kullanırken, disk belleği, burada s (veritabanı verileri önbelleğe alma eksikliği) isabet bu performans aşmak için hiçbir yol varsayılan. Ancak, bir sonraki öğreticide anlatıldığı gibi verimli bir şekilde özel disk belleği kullanırken verileri sıralamak olası s.

Her GridView alan bir ObjectDataSource GridView GridView s akıllı etiket aşağı açılan listeden üzerinden bağlanırken, otomatik olarak sahip kendi `SortExpression` özelliğini veri alanına adı atanmış `ProductsRow` sınıfı. Örneğin, `ProductName` BoundField s `SortExpression` ayarlanır `ProductName`aşağıdaki bildirim temelli işaretlemede gösterildiği gibi:


[!code-aspx[Main](paging-and-sorting-report-data-cs/samples/sample9.aspx)]

Bir alan yapılandırılabilir böylece onu s öğenizin sıralanamaz kendi `SortExpression` (boş dize olarak atama) özelliği. Bunu görmek için Imagine biz etmedi t müşterilerimizin Ürünlerimiz fiyatına göre sıralama izin istiyor. `UnitPrice` BoundField s `SortExpression` özelliği, bildirim temelli işaretleme ya da (GridView s akıllı etiketinde sütunları Düzenle bağlantısına tıklayarak erişilebilir olan) alanları iletişim kutusu aracılığıyla kaldırılabilir.


![Sonuçları artan düzende UnitPrice göre sıralanmış](paging-and-sorting-report-data-cs/_static/image27.png)

**Şekil 13**: sonuçları artan düzende UnitPrice göre sıralanmış


Bir kez `SortExpression` özelliği için kaldırılmıştır `UnitPrice` BoundField, üst bilgi metni yerine böylece kullanıcıların verileri göre fiyat sıralamasını engelleme, bir bağlantı olarak işlenir.


[![SortExpression özelliğine kaldırarak, kullanıcılar artık ürünleri fiyata göre sıralayabilirsiniz](paging-and-sorting-report-data-cs/_static/image29.png)](paging-and-sorting-report-data-cs/_static/image28.png)

**Şekil 14**: SortExpression özelliğine kaldırarak, kullanıcılar artık tarafından Ürünleri fiyat sıralayabilir ([tam boyutlu görüntüyü görmek için tıklatın](paging-and-sorting-report-data-cs/_static/image30.png))


## <a name="programmatically-sorting-the-gridview"></a>GridView programlamayla sıralama

GridView s kullanarak GridView içeriğini programlı olarak sıralayabilirsiniz [ `Sort` yöntemi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sort.aspx). Yalnızca geçirin `SortExpression` ile birlikte göre sıralamak için değer [ `SortDirection` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sortdirection.aspx) (`Ascending` veya `Descending`), ve GridView s veriler yeniden sıralanan olacaktır.

Neden şu sıralamada kapalı açık emin Imagine `UnitPrice` ki müşterilerimiz, yalnızca en düşük fiyatlı ürünlerini yalnızca satın kaygılı nedeni. Bununla birlikte, d biz bunları ürünler için en az fiyat, ancak yalnızca en pahalı fiyatından sıralama yapmak istediğiniz şekilde en pahalı ürünleri satın almak için onları teşvik etmek istiyoruz.

Bunu gerçekleştirmek için bir düğme Web denetimi sayfasına ekleyin ayarlayın, `ID` özelliğini `SortPriceDescending`ve onun `Text` fiyata göre sıralama özelliğini. Ardından, s düğmesi için olay işleyicisi oluşturun `Click` Olay Tasarımcısı'nda düğme denetimini çift tıklayın. Bu olay işleyicisine aşağıdaki kodu ekleyin:


[!code-csharp[Main](paging-and-sorting-report-data-cs/samples/sample10.cs)]

Kullanıcı bu düğmeye tıklandığında fiyatından, ucuz (bkz: Şekil 15) en pahalı ölçütü ürünleri ile ilk sayfasına döndürür.


[![Düğmeye tıklandığında en pahalı ürünleri siparişleri en az](paging-and-sorting-report-data-cs/_static/image32.png)](paging-and-sorting-report-data-cs/_static/image31.png)

**Şekil 15**: düğmeye tıklandığında siparişleri ürünleri gelen en pahalı en az ([tam boyutlu görüntüyü görmek için tıklatın](paging-and-sorting-report-data-cs/_static/image33.png))


## <a name="summary"></a>Özet

Sayfalama ve sıralama yetenekleri varsayılan uygulama gördüğümüz Bu öğreticide, ikisi için de bir onay kutusu denetimi kadar kolay! Bir kullanıcı sıralar veya verilerine sayfaları benzer bir iş akışı açılan:

1. Bir geri gönderme ensues
2. Veri Web denetimi s önceden düzeyi olay harekete geçirilir (`PageIndexChanging` veya `Sorting`)
3. Tüm verileri yeniden alınır ObjectDataSource tarafından
4. Veri Web denetimi s sonrası olay harekete geçirilir düzey (`PageIndexChanged` veya `Sorted`)

Uygulama temel sayfalama ve sıralama barındırmamıza olsa da, daha verimli özel disk belleği kullanan veya disk belleği veya sıralama arabirimi daha da geliştirmek için daha fazla çaba sarf gerekir. Sonraki öğreticiler Bu konular inceleyeceksiniz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Next](efficiently-paging-through-large-amounts-of-data-cs.md)
