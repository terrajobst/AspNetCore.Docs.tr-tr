---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
title: DataList veya Repeater denetiminde (C#) rapor verilerini sayfalama | Microsoft Docs
author: rick-anderson
description: DataList veya Repeater ne teklif otomatik sayfalama veya sıralama desteği, bu öğreticide sayfalama desteğini DataList veya Repeater nasıl ekleneceğini gösterir...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: e8e0809b-25c4-4c3b-8d12-9a17048148ae
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 4212b7bff41d76eaef18d638cf28441b50061159
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753169"
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-c"></a>Rapor verilerini sayfalama DataList veya Repeater denetiminde (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_CS.exe) veya [PDF olarak indirin](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial44cs1.pdf)

> DataList veya Repeater ne teklif sayfalama veya sıralama desteği, Bu öğretici otomatik DataList veya çok daha esnek sayfalama ve verileri görüntüleme arabirimleri sağlayan bir yineleyici için sayfalama desteği ekleme gösterirken.


## <a name="introduction"></a>Giriş

Sayfalama ve sıralama iki yaygın veri çevrimiçi uygulamada görüntülenirken özellikleridir. Örneğin, bir çevrimiçi kitaplığı, ASP.NET books için arama yaparken böyle books yüzlerce olabilir, ancak arama sonuçlarını listeleme rapor sayfa başına yalnızca on eşleşmeleri listeler. Ayrıca, sonuçları, başlık, fiyat, sayfa sayısı, yazarın adı ve benzeri göre sıralanabilir. Açıkladığımız gibi [sayfalama ve sıralama rapor verileri](../paging-and-sorting/paging-and-sorting-report-data-cs.md) Öğreticisi, tüm bir onay kutusu değer çizgisi etkin yerleşik sayfalama desteğini sağlamak FormView GridView ve DetailsView denetimlerini. GridView sıralama desteği de içerir.

Ne yazık ki, ne DataList veya Repeater teklif otomatik disk belleği veya sıralama desteği. Bu öğreticide DataList veya Repeater sayfalama desteği ekleme inceleyeceğiz. Biz gerekir el ile sayfalama arabirimi oluşturmak, uygun kayıt sayfasını görüntüler ve Geri göndermeler arasında ziyaret sayfa unutmayın. Bu daha fazla zaman ve daha bir kod GridView, DetailsView veya FormView ile alırken, DataList ve Repeater daha esnek sayfalama ve verileri görüntüleme arabirimleri sağlar.

> [!NOTE]
> Bu öğreticide, özel olarak sayfalamayı odaklanır. Sonraki öğreticide biz sıralama yetenekleri ekleme için uygulamamızla açacağım.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>1. adım: sayfalama ekleme ve öğretici Web sayfaları sıralama

Biz bu öğreticiye başlamadan önce ilk yapmamız gereken Bu öğretici ve sonraki komutu için ASP.NET sayfaları eklemek için bir dakikanızı ayırın s olanak tanır. Adlı projede yeni bir klasör oluşturarak başlayın `PagingSortingDataListRepeater`. Ardından, tüm bunları ana sayfaya kullanacak şekilde yapılandırılmış olması ve bu klasörü için aşağıdaki beş ASP.NET sayfaları ekleyin `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![PagingSortingDataListRepeater bir klasör oluşturun ve öğretici ASP.NET sayfaları ekleme](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Şekil 1**: oluşturma bir `PagingSortingDataListRepeater` klasörü ve öğretici ASP.NET sayfaları ekleyin


Ardından, açık `Default.aspx` sürükleyin ve sayfa `SectionLevelTutorialListing.ascx` kullanıcı denetimi `UserControls` tasarım yüzeyine klasör. Bu kullanıcı, oluşturduğumuz denetimini [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-cs.md) öğretici, site haritası numaralandırır ve öğreticilerle geçerli bir madde işaretli liste bölümünde görüntüler.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)

**Şekil 2**: ekleme `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image4.png))


Madde işaretli liste sayfalama ve sıralama biz oluşturursunuz öğreticileri görüntülemek için site eşlemesinin ekleneceği gerekiyor. Açık `Web.sitemap` dosya ve DataList site haritası düğüm biçimlendirme düzenleme ve silme sonra aşağıdaki işaretlemeyi ekleyin:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample1.xml)]


![Yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)

**Şekil 3**: yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir


## <a name="a-review-of-paging"></a>Disk belleği gözden geçirin

Önceki öğreticilerde FormView GridView ve DetailsView denetimlerini veri sayfasında nasıl gördük. Disk belleği adlı basit bir tür bu üç denetimleri sunan *varsayılan sayfalama* , uygulanabilir denetim s akıllı etiketinde etkinleştirme disk belleği seçeneğini işaretleyerek. Varsayılan disk belleği ile her zaman bir sayfa veri istenen ilk sayfasında ya da ziyaret edin veya ne zaman kullanıcı veri GridView DetailsView, farklı bir sayfaya gider veya FormView denetimi yeniden istekleri *tüm* verileri ObjectDataSource. Bunu daha sonra kayıtları görüntülemek için belirli kümesini istenen sayfa dizini ve sayfa başına görüntülenecek kayıt sayısı, verilen parçalar. Varsayılan disk belleği ayrıntılı olarak ele almıştık [sayfalama ve sıralama rapor verileri](../paging-and-sorting/paging-and-sorting-report-data-cs.md) öğretici.

Varsayılan disk belleği her sayfa için tüm kayıtları yeniden istekleri olduğundan, bu yeterince büyük miktarlarda veri sayfalama pratik değildir. Örneğin, bir sayfa boyutu 10 ile 50.000 kayıtları arasında disk belleği düşünün. Kullanıcı yeni bir sayfaya gittiğinde her defa rağmen bunların yalnızca on görüntülenen 50.000 tüm kayıtları veritabanından alınmalıdır.

*Özel disk belleği* yalnızca kesin kayıt alt kümesi üzerinde istenen sayfasını görüntülemek için kapmasını tarafından varsayılan disk belleği, performans sorunlarını çözer. Özel disk belleği uygularken, biz yalnızca doğru kayıt kümesini döndürür verimli bir şekilde SQL sorgusu yazmanız gerekir. SQL Server 2005 s kullanarak yeni sorgu oluşturma gördüğümüz [ `ROW_NUMBER()` anahtar sözcüğü](http://www.4guysfromrolla.com/webtech/010406-1.shtml) geri [verimli bir şekilde sayfalama aracılığıyla büyük miktarda veri](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) öğretici.

DataList veya Repeater denetimleri varsayılan sayfalama uygulamak için kullanabilir miyiz [ `PagedDataSource` sınıfı](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.aspx) çevresinde sarmalayıcı olarak `ProductsDataTable` içerikleri havuzda. `PagedDataSource` Sınıfında bir `DataSource` numaralandırılabilir herhangi bir nesneye atanabilir özelliği ve [ `PageSize` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) ve [ `CurrentPageIndex` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) kaç kayıtlara belirten özellikleri Sayfa başına Göster ve geçerli sayfa dizini. Bu özellikleri ayarladıktan sonra `PagedDataSource` herhangi bir veri Web denetimi veri kaynağı olarak kullanılabilir. `PagedDataSource`, Numaralandırılan, uygun bir alt kendi iç kayıtlarının yalnızca dönüş olacak `DataSource` göre `PageSize` ve `CurrentPageIndex` özellikleri. Şekil 4 işlevlerini göstermektedir `PagedDataSource` sınıfı.


![Bir numaralandırma nesnesi alınabilir bir arabirim PagedDataSource sarmalar](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image6.png)

**Şekil 4**: `PagedDataSource` alınabilir bir arabirim bir numaralandırma nesnesi sarmalar


`PagedDataSource` Nesnesi olabilir oluşturulan ve yapılandırılan doğrudan iş mantığı katmanından ve DataList veya Repeater bir ObjectDataSource üzerinden bağlı veya oluşturulabilir ve doğrudan ASP.NET sayfalarının arka plan kod sınıfında yapılandırılmış. İkinci yaklaşımda kullandıysanız, biz ObjectDataSource kullanmayı bırakmayı ve bunun yerine disk belleğine alınan verilere DataList veya Repeater programlı bir şekilde bağlayın.

`PagedDataSource` Nesne özel disk belleği desteklemek için özellikleri de vardır. Ancak biz kullanarak atlayabilir bir `PagedDataSource` biz bir zaten BLL yöntemleri sahip olduğundan özel disk belleği için `ProductsBLL` görüntülemek için kesin kayıt döndüren özel disk belleği için tasarlanmış bir sınıf.

Bu öğreticide varsayılan sayfalama DataList içinde uygulanmasına yeni bir yönteme ekleyerek göz atacağız `ProductsBLL` uygun şekilde yapılandırılmış döndüren sınıfı `PagedDataSource` nesne. Sonraki öğreticide, özel disk belleği kullanmayı göreceğiz.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>2. adım: varsayılan bir disk belleği yöntemi içinde iş mantığı katmanı ekleme

`ProductsBLL` Sınıfı, şu anda tüm ürün bilgileri döndürmek için bir yöntem olan `GetProducts()` ve bir başlangıç dizini ürünlerin belirli bir alt döndürmek için `GetProductsPaged(startRowIndex, maximumRows)`. Varsayılan disk belleği ile GridView DetailsView ve FormView tüm kullanım denetimleri `GetProducts()` tüm ürünleri almak, ancak daha sonra kullanmak için yöntemi bir `PagedDataSource` dahili olarak yalnızca doğru alt kayıtları görüntülemek için. Bu işlev DataList ve Repeater denetimleri ile çoğaltmak için yeni bir yöntem bu davranışını taklit eden BLL içinde oluşturabiliriz.

Bir yöntem ekleyin `ProductsBLL` adlı sınıfı `GetProductsAsPagedDataSource` iki tamsayı giriş parametreleri alır:

- `pageIndex` Dizin sıfır olarak görüntülemek için sayfanın dizini ve
- `pageSize` Sayfa başına görüntülenecek kayıt sayısı.

`GetProductsAsPagedDataSource` alarak başlatır *tüm* kayıtlarını `GetProducts()`. Ardından oluşturur bir `PagedDataSource` ayarlama nesne, kendi `CurrentPageIndex` ve `PageSize` geçilen değerlerini özelliklerine `pageIndex` ve `pageSize` parametreleri. Bu yapılandırmayı döndürerek yöntemin sonucuna `PagedDataSource`:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>3. adım: Varsayılan disk belleği'ni kullanarak bir DataList ürün bilgilerini görüntüleme

İle `GetProductsAsPagedDataSource` eklenen yöntemi `ProductsBLL` sınıfı artık oluştururuz DataList veya varsayılan sayfalama sağlayan yineleyici. Başlangıç açarak `Paging.aspx` sayfasını `PagingSortingDataListRepeater` klasörü ve DataList s ayar Tasarımcısı araç kutusundan sürükleyip DataList `ID` özelliğini `ProductsDefaultPaging`. DataList s akıllı etiketten adlı yeni bir ObjectDataSource oluşturma `ProductsDefaultPagingDataSource` ve verileri kullanarak alır şekilde yapılandırın `GetProductsAsPagedDataSource` yöntemi.


[![Bir ObjectDataSource oluşturun ve bunu GetProductsAsPagedDataSource () yöntemini kullanmak için yapılandırın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Şekil 5**: bir ObjectDataSource oluşturmak ve kullanmak için yapılandırma `GetProductsAsPagedDataSource` `()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


Güncelleştirme, ekleme, açılan listeler ayarlayın ve sekme (hiçbiri) SİLİN.


[![Aşağı açılan listeler, ekleme, güncelleştirme ayarlayın ve sekme (hiçbiri) silme](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Şekil 6**: aşağı açılan listeler ayarlayın, ekleme, güncelleştirme ve silme (hiçbiri) için sekmeler ([tam boyutlu görüntüyü görmek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


Bu yana `GetProductsAsPagedDataSource` yöntemi giriş iki parametre bekliyor, sihirbaz bize kaynağı bu parametre değerlerini ister.

Sayfa dizini ve sayfa boyutu değerleri Geri göndermeler arasında anımsanacak gerekir. Bunlar görünüm durumuna depolanabilir, sorgu dizesi için kalıcı, oturumu değişkenlerinde depolanan veya diğer bir yöntem kullanarak anımsanacak. Bu öğretici için veri bozulmasına için belirli bir sayfada izin vererek avantajına sahiptir querystring kullanacağız.

Özellikle, sorgu dizesi alanlar PageIndex ve pageSize için kullanmak `pageIndex` ve `pageSize` parametreleri, sırasıyla (bkz. Şekil 7). Bir kullanıcı bu sayfa ilk ziyaret ettiğinde t kazanılan sorgu dizesi değerleri bulunması bu parametreler için varsayılan değerleri ayarlamak için biraz alın. İçin `pageIndex`, varsayılan değeri (hangi veri'nın ilk sayfasında gösterilir) 0 olarak ayarlayın ve `pageSize` s varsayılan değer 4.


[![Sorgu dizesi kaynak olarak PageIndex ve pageSize parametrelerini kullanın.](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Şekil 7**: sorgu dizesi için kaynak olarak kullanmak `pageIndex` ve `pageSize` parametreleri ([tam boyutlu görüntüyü görmek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image15.png))


ObjectDataSource yapılandırdıktan sonra Visual Studio otomatik olarak oluşturur bir `ItemTemplate` DataList için. Özelleştirme `ItemTemplate` böylece yalnızca s ürün adı, kategori ve tedarikçi gösterilir. DataList s de ayarlanmış `RepeatColumns` özelliği 2, kendi `Width` % 100 ve kendi `ItemStyle` s `Width` % 50'si. Bu genişliği ayarları için iki sütun eşit aralık sağlar.

Bu değişiklikleri yaptıktan sonra DataList ve ObjectDataSource s biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

> [!NOTE]
> Herhangi bir güncelleştirme gerçekleştirme değil veya Bu öğreticide silme işlevi beri işlenen sayfa boyutunu azaltmak için DataList s görünüm durumu devre dışı bırakabilir.


Başlangıçta bu sayfayı bir tarayıcı aracılığıyla diğerinden ziyaret `pageIndex` ya da `pageSize` querystring parametreleri sağlanır. Bu nedenle, varsayılan değerleri 0 ile 4 kullanılır. Şekil 8 gösterildiği gibi bu ilk dört ürünleri görüntüler DataList sonuçlanır.


[![İlk dört ürünler listelenir](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image16.png)

**Şekil 8**: ilk dört ürünleri listelenir ([tam boyutlu görüntüyü görmek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image18.png))


Veri ikinci sayfasına gitmek bir kullanıcı için orada s şu anda en basit bir disk belleği arabirimi anlamına gelir olmadan. 4. adımda bir disk belleği arabirimi oluşturacağız. Şimdilik, yine de disk belleği yalnızca doğrudan disk belleği ölçütleri girilerek gerçekleştirilebilir. Örneğin, ikinci sayfasında görüntülemek için tarayıcı s Adres çubuğundan URL'yi değiştirmeniz `Paging.aspx` için `Paging.aspx?pageIndex=2` ve Enter tuşuna basın. Bu ikinci sayfasında görüntülenecek verileri neden olur (bkz. Şekil 9).


[![İkinci sayfasında, veriler görüntülenir](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image19.png)

**Şekil 9**: İkinci sayfa olan verileri görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>4. adım: disk belleği arabirimi oluşturma

Uygulanabilir farklı disk belleği arabirimleri çeşitli vardır. FormView GridView ve DetailsView denetimlerini arasından seçim için dört farklı arabirimler sağlar:

- **Sonra önceki** kullanıcılar sonraki veya önceki bir için bir seferde bir sayfa taşıyabilirsiniz.
- **Yanında, Previous, ilk, son** sonraki ve önceki düğmeleri ek olarak, bu arabirim ilk veya son sayfasına taşımak için ilk ve son düğmeleri içerir.
- **Sayısal** bir kullanıcının belirli bir sayfaya hızlı bir şekilde atlamak disk belleği arabirimi sayfa numaralarını listeler.
- **Sayısal, ilk olarak, son** sayısal sayfa numaralarını ek olarak, ilk veya son sayfasına taşımak için düğmeleri içerir.

DataList ve Repeater için bir disk belleği arabirimi karar verme ve uygulamadan sorumlu duyuyoruz. Bu sayfada gerekli Web denetimleri oluşturma ve belirli bir disk belleği arabirimi düğmesine tıklandığında, istenen sayfa görüntüleme içerir. Ayrıca, belirli bir disk belleği arabirimi denetimleri devre dışı bırakılması gerekebilir. Örneğin, ilk sayfa sonraki kullanarak veri görüntüleme, önceki, ilk olarak, en son arabirim, hem ilk hem de önceki düğmeleri devre dışı.

Bu öğreticide, let s kullanmak bir sonraki önceki, ilk olarak, en son arabirim. Dört düğme Web denetimi için bir sayfa ekleyin ve ayarlamak kendi `ID` s `FirstPage`, `PrevPage`, `NextPage`, ve `LastPage`. Ayarlama `Text` özelliklerine &lt; &lt; ilk &lt; önceki, sonraki &gt;ve son &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample4.aspx)]

Ardından, oluşturun bir `Click` bu düğmelerin her biri için olay işleyicisi. Bir dakika içinde istenen sayfasını görüntülemek gerekli kod ekleyeceğiz.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Toplam sayısı üzerinden havuzda kayıtları hatırlama

Seçili disk belleği arabirimi bağımsız olarak işlem ve kayıtlar aracılığıyla havuzda toplam sayısı ihtiyacımız var. Toplam satır sayısı (gelen sayfa boyutu ile birlikte), veri kaç toplam sayfa, ne sayfalama arabirimi denetimleri eklenir veya etkin belirleyen disk belleğine alınan belirler. İleri, Previous, ilk, biz oluşturuyorsanız, son arabirimi sayfa sayısı iki şekilde kullanılır:

- Biz son sayfasında, bu durumda görüntülüyorsunuz olup olmadığını belirlemek için İleri ve son düğmelerini devre dışı bırakılır.
- Kullanıcının son whisk ihtiyacımız Son düğmesine tıklarsa sayfa değerinden dizini biridir, sayfa sayısı.

Sayfa sayısı, toplam satır sayısı tavanını sayfa boyutu tarafından ayrılmış olarak hesaplanır. Biz sayfa başına dört kayıtlarla 79 kayıtları arasında sayfalama, örneğin, ardından sayfa sayısı 20'dir (79 tavanını / 4). Sayısal sayfalama arabirimi de kullanıyorsanız, bu bilgileri bize görüntülemek için kaç sayısal sayfa düğmelerini seçeceğine bildirir; Sayfa sayısı, disk belleği arabirimimizi İleri'yi veya son düğmeleri içeriyorsa, İleri'yi veya son düğmeleri devre dışı bırakmak ne zaman belirlemek için kullanılır.

Disk belleği arabirimi son düğme içeriyorsa, böylece son düğme tıklandığında son sayfa dizini belirleyebiliriz kayıtları aracılığıyla havuzda toplam sayısı Geri göndermeler anımsanacağını zorunludur. Bunu kolaylaştırmak için oluşturun bir `TotalRowCount` durumunu görüntülemek için değeri devam ederse ASP.NET sayfalarının arka plan kod sınıfı özelliği:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Ek olarak `TotalRowCount`kolayca sayfa boyutu, sayfa dizini erişmek için salt okunur sayfa düzeyi özellikleri oluşturmak için bir dakikanızı ayırın ve sayfa sayısı:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample6.cs)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Kayıtları aracılığıyla havuzda toplam sayısını belirleme

`PagedDataSource` Nesnesi döndürülen ObjectDataSource s `Select()` yöntemi içinde olan *tüm* ürün kayıtları, olsa bile yalnızca bir alt kümesini görüntülenir DataList. `PagedDataSource` s [ `Count` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.count.aspx) yalnızca içinde DataList; gösterilecek öğe sayısını döndürür [ `DataSourceCount` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) içindeki öğeleri toplam sayısını döndürür `PagedDataSource`. Bu nedenle, ASP.NET sayfası s atamak ihtiyacımız `TotalRowCount` özellik değeri, `PagedDataSource` s `DataSourceCount` özelliği.

Bunu gerçekleştirmek için bir olay işleyicisi ObjectDataSource s için oluşturma `Selected` olay. İçinde `Selected` ObjectDataSource s dönüş değerini erişimi sahibiz olay işleyicisi `Select()` yöntemi bu durumda, `PagedDataSource`.


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

## <a name="displaying-the-requested-page-of-data"></a>İstenen sayfa veri görüntüleme

Kullanıcı düğmeleri disk belleği arabiriminde tıkladığında, istenen veri sayfasını görüntülemek ihtiyacımız var. İstenen sayfa veri kullanımını göstermek için belirtilen sayfalama parametreleri sorgu dizesi bu yana `Response.Redirect(url)` kullanıcı s tarayıcı yeniden isteği için `Paging.aspx` uygun disk belleği parametrelere sahip sayfa. Örneğin, ikinci veri sayfasını görüntülemek için şu kullanıcı için yeniden yönlendirme `Paging.aspx?pageIndex=1`.

Bunu kolaylaştırmak için oluşturun bir `RedirectUser(sendUserToPageIndex)` kullanıcıya yönlendiren yöntemi `Paging.aspx?pageIndex=sendUserToPageIndex`. Ardından bu dört düğmeyi bu yöntemi çağıran `Click` olay işleyicileri. İçinde `FirstPage` `Click` olay işleyicisi, çağrı `RedirectUser(0)`, ilk sayfa; şirketlerde `PrevPage` `Click` olay işleyicisi, kullanım `PageIndex - 1` ; sayfa dizini olarak ve benzeri.


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample8.cs)]

İle `Click` DataList s kayıtları aracılığıyla düğmelere tıklayarak belleğine alınabilen, olay işleyicileri tamamlayın. Denemek için birkaç dakikanızı!

## <a name="disabling-paging-interface-controls"></a>Disk belleği arabirimi denetimleri devre dışı bırakma

Şu anda tüm dört düğme, görüntülenmekte olan sayfa bağımsız olarak etkinleştirilir. Ancak, veri ve sonraki ve son düğmeleri'nın ilk sayfasında son sayfa gösterilirken gösterilirken ilk ve önceki düğmelerini devre dışı bırakmak istiyoruz. `PagedDataSource` ObjectDataSource s tarafından döndürülen nesne `Select()` yöntemi olan özellikleri [ `IsFirstPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) ve [ `IsLastPage` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) , biz görüntüleme, belirlemek inceleyeceğiz ilk veya son sayfasına veri.

ObjectDataSource s için aşağıdakileri ekleyin `Selected` olay işleyicisi:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Bu ekleme ile ilk ve önceki düğmeleri sonraki ve son düğmeleri son sayfayı görüntülerken devre dışı bırakılacak ancak ilk sayfasında, görüntülerken devre dışı bırakılır.

Let s tamamlamak sayfalama arabirimi kullanıcı bilgilendirerek ne sayfa bunlar şu anda görüntülemekte ve kaç toplam sayfa mevcut. Bir etiket Web denetimi için bir sayfa ekleyin ve ayarlamak kendi `ID` özelliğini `CurrentPageNumber`. Ayarlama, `Text` ObjectDataSource s seçili olay işleyicisi böyle bir özellik görüntülenmekte olan geçerli sayfa içerir (`PageIndex + 1`) ve toplam sayfa sayısı (`PageCount`).


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample10.cs)]

Şekil 10 gösteren `Paging.aspx` ilk ziyaret edildiğinde. Sorgu dizesi boş olduğundan, ilk dört ürünleri gösterecek şekilde DataList varsayılan olarak; İlk ve önceki düğmelerini devre dışı bırakıldı. İleri'ye tıklama (bkz. Şekil 11) sonraki dört kayıtları görüntüler. İlk ve önceki düğmeleri şimdi etkinleştirilir.


[![İlk sayfasında, veriler görüntülenir](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image22.png)

**Şekil 10**: ilk sayfasında veri görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image24.png))


[![İkinci sayfasında, veriler görüntülenir](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image25.png)

**Şekil 11**: İkinci sayfa olan verileri görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image27.png))


> [!NOTE]
> Daha fazla disk belleği arabirimi kaç sayfa başına görüntülenecek sayfaları belirtmesini sağlayarak geliştirilebilir. Örneğin bir DropDownList listesi sayfa boyutu seçeneklerini 5, 10, 25, 50 ve tüm gibi eklenemedi. Sayfa boyutu seçtikten sonra kullanıcı yeniden yönlendirilmesi gereken `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Ben bu geliştirme için okuyucu bir alıştırma olarak uygulama bırakın.


## <a name="using-custom-paging"></a>Özel disk belleği kullanma

Disk belleği verimsiz varsayılan tekniğiyle verilerini DataList sayfalarıyla. Yeterince büyük miktarda veriyi disk belleği, özel disk belleği kullanılması zorunludur. Uygulama Ayrıntıları biraz farklılık gösterse de özel sayfalama uygulama içinde bir DataList arkasında kavramları varsayılan disk belleği ile aynıdır. Özel disk belleği ile kullanmak `ProductBLL` s sınıfı `GetProductsPaged` yöntemi (yerine `GetProductsAsPagedDataSource`). Bölümünde açıklandığı gibi [verimli bir şekilde sayfalama aracılığıyla büyük miktarda veri](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) öğreticide `GetProductsPaged` başlangıç satır dizini ve en yüksek döndürülecek satır sayısını geçirilmelidir. Bu parametreler sorgu dizesi olduğu gibi sürdürülebilir `pageIndex` ve `pageSize` varsayılan sayfalama kullanılan parametreler.

Orada s bu yana hiçbir `PagedDataSource` özel disk belleği ile alternatif teknikleri aracılığıyla ve olup disk belleği kayıt toplam sayısını belirlemek için kullanılması gereken biz verilerin ilk veya son sayfa görüntüleme re. `TotalNumberOfProducts()` Yönteminde `ProductsBLL` sınıfı ürünler aracılığıyla havuzda toplam sayısını döndürür. Sıfır ise ilk sayfasında görüntülenen sonra veri'nın ilk sayfasında görüntülenen olmadığını belirlemek için başlangıç satır dizini inceleyin. Büyüktür veya eşittir kayıtları aracılığıyla havuzda toplam sayısına başlangıç satır dizini artı döndürülecek en fazla satır, son sayfa görüntüleniyor.

Özel disk belleği daha ayrıntılı olarak sonraki öğreticide uygulama ele alacağız.

## <a name="summary"></a>Özet

DataList veya Repeater ne dışı sunarken GridView, DetailsView, disk belleği desteği bulunan ve FormView denetimleri, çok az çabayla tür işlevselliği eklenebilir. Ürünlerde kümesinin tamamını sarmalamak için varsayılan sayfalama uygulama yapmanın en kolay yolu olan bir `PagedDataSource` ve ardından `PagedDataSource` DataList veya Repeater. Bu öğreticide eklediğimiz `GetProductsAsPagedDataSource` yönteme `ProductsBLL` döndürülecek sınıfı `PagedDataSource`. `ProductsBLL` Sınıfı zaten var. özel disk belleği için gereken yöntemleri `GetProductsPaged` ve `TotalNumberOfProducts`.

Özel disk belleği için görüntülenecek kayıt kesin kümesini veya tüm kayıtlar alınırken yanı sıra bir `PagedDataSource` varsayılan sayfalama de disk belleği arabirimi el ile eklemek ihtiyacımız. Bu öğretici için oluşturduğumuz bir sonraki, Previous, ilk olarak, son dört düğme Web denetimleri ile arabirim. Ayrıca, toplam sayfa sayısı ve geçerli sayfa numarası görüntüleyen bir etiket denetimi eklendi.

DataList ve Repeater sıralama desteği ekleme sonraki öğreticide göreceğiz. Ayrıca, hem disk belleğine alınan ve (varsayılan ve özel disk belleği kullanarak örneklerle) sıralanmış bir DataList oluşturma göreceğiz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Liz Shulok, Ken Pespisa ve Bernadette Leigh yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](sorting-data-in-a-datalist-or-repeater-control-cs.md)
