---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
title: "Rapor verileri bir DataList ya da yineleyici denetimi (C#) disk belleği | Microsoft Docs"
author: rick-anderson
description: "DataList ne yineleyici teklif otomatik sayfalama veya sıralama desteği, Bu öğretici sayfalama desteğini DataList ya da yineleyici nasıl ekleneceğini gösterir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: e8e0809b-25c4-4c3b-8d12-9a17048148ae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/paging-report-data-in-a-datalist-or-repeater-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 783557b69486c284a6ed927e32e71cb602695080
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="paging-report-data-in-a-datalist-or-repeater-control-c"></a>Disk belleği rapor verilerini bir DataList ya da yineleyici denetimi (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_44_CS.exe) veya [PDF indirin](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/datatutorial44cs1.pdf)

> Disk belleği veya destek, Bu öğretici sıralama otomatik DataList ne yineleyici teklif DataList ya da çok daha esnek disk belleği ve veriler için görüntü arabirimleri sağlar yineleyici sayfalama desteği ekleme gösterirken.


## <a name="introduction"></a>Giriş

Disk belleği ve sıralama iki yaygın veri çevrimiçi uygulamada görüntülenirken özellikleridir. Örneğin, bir çevrimiçi kitap mağazasına ASP.NET books aranırken böyle books yüzlerce olabilir, ancak sayfa başına yalnızca on eşleşmeler arama sonuçlarını listeleme rapor listeler. Ayrıca, sonuçları başlık, fiyat, sayfa sayısı, yazar adı ve benzeri göre sıralanabilir. Biz anlatıldığı gibi [disk belleği ve rapor verilerini sıralama](../paging-and-sorting/paging-and-sorting-report-data-cs.md) Öğreticisi, GridView, DetailsView ve FormView denetimlerini tüm bir onay kutusu değer çizgilerinin etkinleştirilebilir yerleşik disk belleği destek sağlar. GridView, destek sıralama de içerir.

Ne yazık ki, ne DataList ne de yineleyici sunar otomatik disk belleği veya destek sıralama. Bu öğreticide biz DataList veya yineleyici sayfalama desteği eklemek nasıl inceleyeceğiz. Biz gerekir el ile disk belleği arabirimi oluşturmak, kayıt uygun sayfasını görüntüler ve Geri göndermeler arasında ziyaret sayfa unutmayın. Bu daha fazla zaman ve daha bir kod GridView, DetailsView veya FormView alırken, DataList ve yineleyici çok daha esnek disk belleği ve veriler için görüntü arabirimleri sağlar.

> [!NOTE]
> Bu öğretici yalnızca disk belleği odaklanır. Sonraki öğreticide biz sıralama yetenekleri ekleme için uygulamamızla kapatmanız.


## <a name="step-1-adding-the-paging-and-sorting-tutorial-web-pages"></a>1. adım: sayfalama ekleme ve öğretici Web sayfaları sıralama

Biz Bu öğretici başlamadan önce öncelikle Bu öğretici ve bir sonraki yapmamız gereken ASP.NET sayfaları eklemek için bir dakikanızı ayırın s olanak tanır. Başlangıç adlı projeye yeni bir klasör oluşturarak `PagingSortingDataListRepeater`. Ardından, aşağıdaki beş ASP.NET sayfaları tümünün ana sayfa kullanacak şekilde yapılandırılmış olan bu klasöre eklemek `Site.master`:

- `Default.aspx`
- `Paging.aspx`
- `Sorting.aspx`
- `SortingWithDefaultPaging.aspx`
- `SortingWithCustomPaging.aspx`


![PagingSortingDataListRepeater bir klasör oluşturun ve öğretici ASP.NET sayfaları ekleme](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image1.png)

**Şekil 1**: oluşturmak bir `PagingSortingDataListRepeater` klasörü ve öğretici ASP.NET sayfaları ekleme


Ardından, açık `Default.aspx` sayfasında ve sürükleyin `SectionLevelTutorialListing.ascx` kullanıcı denetiminden `UserControls` tasarım yüzeyine klasör. Bu kullanıcı, içinde oluşturduğumuz denetimi [ana sayfalar ve Site gezintisi](../introduction/master-pages-and-site-navigation-cs.md) öğretici, site haritası numaralandırır ve madde işaretli listede geçerli bölümdeki bu öğreticiler görüntüler.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image3.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image2.png)

**Şekil 2**: eklemek `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image4.png))


Disk belleği ve biz oluşturma öğreticileri sıralama görüntülemek listeyi olması için biz site haritası eklemeniz gerekir. Açık `Web.sitemap` dosya ve aşağıdaki biçimlendirme düzenleme ve silme sonra olan DataList site haritası düğümü biçimlendirme ekleyin:


[!code-xml[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample1.xml)]


![Yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image5.png)

**Şekil 3**: yeni ASP.NET sayfaları dahil etmek için Site Haritası güncelleştir


## <a name="a-review-of-paging"></a>Disk belleği listesini gözden geçirme

Önceki eğitimlerine GridView, DetailsView ve FormView denetimlerinde verileri aracılığıyla sayfasında nasıl gördük. Bu üç denetimleri adlı disk belleği, basit bir form teklif *varsayılan disk belleği* , uygulanabilir denetim s akıllı etiket etkinleştirmek sayfalama seçeneğinde denetleyerek. Varsayılan disk belleği ile her zaman bir sayfa veri istenen ilk sayfasında ya da ziyaret edin veya ne zaman veri GridView DetailsView, farklı bir sayfaya kullanıcı gider veya FormView denetimi yeniden istekleri *tüm* verileri ObjectDataSource. Bunu ardından görüntülenecek kayıt kümesinin istenen sayfa dizini ve sayfa başına görüntülenecek kayıt sayısı, verilen parçalar. Biz varsayılan disk belleği ayrıntılı olarak ele alınan [disk belleği ve rapor verilerini sıralama](../paging-and-sorting/paging-and-sorting-report-data-cs.md) Öğreticisi.

Varsayılan disk belleği her sayfanın tüm kayıtları yeniden istekleri beri yeterince büyük miktarlarda verinin sayfalama pratik değil. Örneğin, bir sayfa boyutu 10 50.000 kayıtlarıyla sayfalama düşünün. Kullanıcı yeni bir sayfaya hareket her zaman yalnızca on bunların görüntülenme şeklini olsa bile tüm 50.000 kayıtları veritabanından alınmalıdır.

*Özel sayfalama* istenen sayfasında görüntülenecek kayıt yalnızca kesin kısmı ele geçirme varsayılan disk belleği, performans sorunları çözer. Özel sayfalama uygularken, biz kayıtları yalnızca doğru kümesini verimli bir şekilde döndürülecek SQL sorgusu yazmanız gerekir. SQL Server 2005 s kullanarak yeni sorgu oluşturma gördüğümüz [ `ROW_NUMBER()` anahtar sözcüğü](http://www.4guysfromrolla.com/webtech/010406-1.shtml) geri [verimli bir şekilde disk belleği üzerinden büyük miktarlarda veri](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) Öğreticisi.

Varsayılan disk belleği DataList veya yineleyici denetimlerinde uygulamak için biz kullanabilirsiniz [ `PagedDataSource` sınıfı](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.aspx) çevresinde bir sarmalayıcı olarak `ProductsDataTable` içeriği disk belleği. `PagedDataSource` Sınıfına sahip bir `DataSource` herhangi bir numaralandırılabilir nesnesine atanabilir özelliği ve [ `PageSize` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.pagesize.aspx) ve [ `CurrentPageIndex` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.currentpageindex.aspx) kaç kayıtlara belirtmek özellikleri Sayfa başına gösterilecek ve geçerli sayfa dizini. Bu özellikleri ayarladıktan sonra `PagedDataSource` herhangi bir veri Web denetimi veri kaynağı olarak kullanılabilir. `PagedDataSource`Numaralandırılan olduğunda, kendi iç kayıtların uygun bir alt yalnızca iade edecek `DataSource` göre `PageSize` ve `CurrentPageIndex` özellikleri. Şekil 4 gösterilmektedir işlevselliğini `PagedDataSource` sınıfı.


![Disk belleğine alınabilir arabirimi ile bir numaralandırma nesnesi PagedDataSource sarmalar](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image6.png)

**Şekil 4**: `PagedDataSource` disk belleğine alınabilir arabirimi ile bir numaralandırma nesnesi sarmalar


`PagedDataSource` Nesnesi olabilir oluşturulan ve doğrudan iş mantığı katmanından yapılandırılmış ve bir DataList ya da bir ObjectDataSource aracılığıyla yineleyici bağlı veya oluşturulabilir ve doğrudan ASP.NET sayfası s arka plan kodu sınıfında yapılandırılmış. İkinci yaklaşımı kullanılırsa, biz ObjectDataSource kullanarak atlayabilirsiniz ve bunun yerine disk belleğine alınan veri DataList veya yineleyici programlı olarak bağlamak gerekir.

`PagedDataSource` Nesne özel sayfalama desteklemek için özellikler de vardır. Ancak, biz kullanarak devre dışı bırakabilir bir `PagedDataSource` biz bir zaten BLL yöntemleri yüklü olduğu için özel disk belleği `ProductsBLL` görüntülemek için kesin kayıtları döndüren özel disk belleği için tasarlanmış sınıfı.

Bu öğreticide biz varsayılan disk belleği bir DataList uygulanmasına yeni bir yönteme ekleyerek göreceğiz `ProductsBLL` uygun şekilde yapılandırılmış döndürür sınıfı `PagedDataSource` nesnesi. Sonraki öğreticide özel disk belleği kullanmayı göreceğiz.

## <a name="step-2-adding-a-default-paging-method-in-the-business-logic-layer"></a>Adım 2: varsayılan disk belleği yöntem iş mantığı katmanı ekleme

`ProductsBLL` Sınıfı, şu anda tüm ürün bilgileri döndürmek için bir yöntem yok `GetProducts()` ve bir başlangıç dizininde ürünlerin belirli bir alt döndürmek için `GetProductsPaged(startRowIndex, maximumRows)`. Varsayılan disk belleği ile GridView, DetailsView ve FormView tüm kullanım denetleyen `GetProducts()` tüm ürünleri almak, ancak daha sonra kullanmak için yöntemi bir `PagedDataSource` dahili olarak yalnızca doğru alt kayıtları görüntülemek için. Bu işlev DataList ve yineleyici denetimleri ile çoğaltmak için yeni bir yöntem bu davranışını taklit eden BLL içinde oluşturabiliriz.

Bir yöntem ekleme `ProductsBLL` adlı sınıf `GetProductsAsPagedDataSource` iki tamsayı giriş parametreleri alır:

- `pageIndex`görüntülemek için sayfanın dizini sıfır olarak dizin oluşturulmuş ve
- `pageSize`Sayfa başına görüntülenecek kayıt sayısı.

`GetProductsAsPagedDataSource`alarak başlatır *tüm* kayıtlarını `GetProducts()`. Daha sonra oluşturur bir `PagedDataSource` ayarı nesne, kendi `CurrentPageIndex` ve `PageSize` geçilen değerlerini özelliklerine `pageIndex` ve `pageSize` parametreleri. Yöntemi bu yapılandırılmış döndürerek sonucuna `PagedDataSource`:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample2.cs)]

## <a name="step-3-displaying-product-information-in-a-datalist-using-default-paging"></a>3. adım: Varsayılan disk belleği kullanarak bir DataList ürün bilgilerini görüntüleme

İle `GetProductsAsPagedDataSource` eklenen yöntemi `ProductsBLL` sınıfı, biz şimdi oluşturabilirsiniz DataList veya varsayılan disk belleği sunar yineleyici. Başlangıç açarak `Paging.aspx` sayfasındaki `PagingSortingDataListRepeater` klasörü ve sürükleme DataList s DataList ayarı tasarımcıya araç `ID` özelliğine `ProductsDefaultPaging`. Adlı yeni bir ObjectDataSource DataList s akıllı etiketten oluşturma `ProductsDefaultPagingDataSource` ve verileri kullanarak alır şekilde yapılandırın `GetProductsAsPagedDataSource` yöntemi.


[![Bir ObjectDataSource oluşturun ve GetProductsAsPagedDataSource () yönteminin kullanacak şekilde yapılandırın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image8.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image7.png)

**Şekil 5**: bir ObjectDataSource oluşturma ve kullanımı için yapılandırma `GetProductsAsPagedDataSource` `()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image9.png))


Güncelleştirme, ekleme, açılan listeleri ayarlayın ve sekmeleri (hiçbiri) SİLİN.


[![Güncelleştirme, ekleme, aşağı açılan listeler ayarlayın ve sekmeleri (hiçbiri) silme](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image11.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image10.png)

**Şekil 6**: güncelleştirme, ekleme, aşağı açılan listeler ayarlayın ve silme (hiçbiri) sekmeler ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image12.png))


Bu yana `GetProductsAsPagedDataSource` yöntemi iki giriş parametreleri bekler, sihirbaz bize Bu parametre değerleri kaynağı ister.

Sayfa dizini ve sayfa boyutu değerleri arasında Geri göndermeler hatırlanan gerekir. Bunlar görünüm durumuna depolanabilir, sorgu dizesi için kalıcı, oturum değişkenleri depolanan veya diğer bir yöntem kullanarak anımsanacak. Bu öğretici için belirli bir sayfa işareti verilerin izin vererek avantajı vardır, sorgu dizesi kullanacağız.

Özellikle, sorgu dizesi alanlarını PageIndex ve pageSize için kullanmak `pageIndex` ve `pageSize` parametreleri, sırasıyla (bkz. Şekil 7). Bir kullanıcı ilk olarak bu sayfayı ziyaret ettiğinde t won sorgu dizesi değerleri olması gibi bu parametre için varsayılan değerleri ayarlamak için bir dakikanızı ayırın. İçin `pageIndex`, varsayılan değer (hangi veri'nın ilk sayfasında gösterilir) 0 olarak ayarlayın ve `pageSize` s varsayılan değer 4.


[![Sorgu dizesi kaynak olarak PageIndex ve pageSize parametrelerini kullanın.](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image14.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image13.png)

**Şekil 7**: sorgu dizesi için kaynak olarak kullanmak `pageIndex` ve `pageSize` parametreleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image15.png))


ObjectDataSource yapılandırdıktan sonra Visual Studio otomatik olarak oluşturur bir `ItemTemplate` DataList için. Özelleştirme `ItemTemplate` böylece yalnızca s ürün adı, kategori ve tedarikçi gösterilir. Ayrıca s DataList ayarlamak `RepeatColumns` özelliğini 2, kendi `Width` % 100 ve kendi `ItemStyle` s `Width` % 50'si. Bu genişliği ayarları iki sütun için eşit aralığı sağlar.

Bu değişiklikleri yaptıktan sonra DataList ve ObjectDataSource s biçimlendirmesi aşağıdakine benzer görünmelidir:


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample3.aspx)]

> [!NOTE]
> Herhangi bir güncelleştirme gerçekleştirmek değil veya Bu öğreticide silme işlevlerinin beri işlenen sayfa boyutunu azaltmak için DataList s görünüm durumu devre dışı bırakabilir.


Başlangıçta bir tarayıcı aracılığıyla bu sayfa, ziyaret ederken `pageIndex` ya da `pageSize` sorgu dizesi parametreleri sağlanır. Bu nedenle, 0 ve 4 varsayılan değerler kullanılır. Şekil 8'de görüldüğü gibi bu ilk dört ürünleri görüntüler DataList sonuçlanır.


[![İlk dört ürünler listelenir](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image17.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image16.png)

**Şekil 8**: ilk dört ürünleri listelenen ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image18.png))


Veri ikinci sayfasına gitmek bir kullanıcı için burada s şu anda Hayır basit bir disk belleği arabirimi anlamına gelir olmadan. 4. adımda bir disk belleği arabirimi oluşturacağız. Şimdilik, ancak, disk belleği yalnızca doğrudan sorgu dizesine disk belleği ölçütleri belirterek gerçekleştirilebilir. Örneğin, ikinci sayfasını görüntülemek için s tarayıcınızın adres çubuğundan URL'yi değiştirmek `Paging.aspx` için `Paging.aspx?pageIndex=2` ve Enter tuşuna basın. Bu ikinci sayfasında görüntülenecek verileri neden olur (bkz. Şekil 9).


[![İkinci sayfa veri görüntülenir](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image20.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image19.png)

**Şekil 9**: ikinci sayfasında veri görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image21.png))


## <a name="step-4-creating-the-paging-interface"></a>4. adım: sayfalama arabirimi oluşturma

Uygulanabilecek farklı disk belleği arabirimleri çeşitli vardır. GridView, DetailsView ve FormView denetimleri arasından seçim yapma dört farklı arabirimleri sağlar:

- **Daha sonra önceki** kullanıcılar sonraki veya önceki bir için her seferinde bir sayfa taşıyabilirsiniz.
- **Ardından, önceki, ilk, son** İleri ve geri düğmelerini ek olarak, bu arabirim, ilk veya son sayfasına taşıma için ilk ve son düğmelerini içerir.
- **Sayısal** hızlı bir şekilde belirli bir sayfaya atlamak bir kullanıcı izin vererek disk belleği arabirimi sayfa numaralarını listeler.
- **Sayısal, ilk, son** sayısal sayfa numaraları ek olarak, ilk veya son sayfasına gitmeye düğmelerini içerir.

DataList ve yineleyici için bir disk belleği arabirimi karar ve çözümü uygulamadan sorumlu duyuyoruz. Bu sayfada gerekli Web denetimleri oluşturma ve belirli bir disk belleği arabirimi düğmesine tıklandığında istenen sayfanın görüntüleme içerir. Ayrıca, belirli disk belleği arabirimi denetimlerini devre dışı bırakılması gerekebilir. Örneğin, sonraki kullanarak verileri'nın ilk sayfasında görüntülerken, önceki, ilk olarak, en son arabirim, ilk ve önceki düğmelerini devre dışı bırakılması.

Bu öğreticide, let s kullanın bir sonraki önceki, ilk olarak, en son arabirim. Dört düğmesi Web denetimleri sayfasına ekleyin ve ayarlayın kendi `ID` s `FirstPage`, `PrevPage`, `NextPage`, ve `LastPage`. Ayarlama `Text` özelliklerine &lt; &lt; ilk &lt; önceki, sonraki &gt;ve son &gt; &gt; .


[!code-aspx[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample4.aspx)]

Ardından, oluşturun bir `Click` bu düğmelerin her biri için olay işleyicisi. İstenen sayfaya göstermek için gerekli kod birazdan ekleyeceğiz.

## <a name="remembering-the-total-number-of-records-being-paged-through"></a>Hatırlamak aracılığıyla havuzda kayıtlarının toplam sayısı

Seçili disk belleği arabirimi bağımsız olarak, biz işlem ve aracılığıyla havuzda kayıtlarının toplam sayısı unutmayın gerekir. Kaç tane toplam sayfa veri aracılığıyla, hangi disk belleği arabirimi denetimleri eklenir veya etkinleştirilir belirleyen disk belleği toplam satır sayısı (sayfa boyutu ile birlikte) belirler. İleri, önceki, ilk, biz oluşturmakta olduğunuz son arabirimi sayfa sayısı iki yolla kullanılır:

- Biz son sayfasında, bu durumda görüntülemekte olduğunuz olup olmadığını belirlemek için sonraki ve son düğmeleri devre dışı bırakılır.
- Kullanıcı son whisk ihtiyacımız Son düğmesine tıklarsa sayfa değerinden dizinini biridir sayfa sayısı.

Sayfa sayısı, toplam satır sayısı üst sınıra bölü sayfa boyutundan olarak hesaplanır. Biz sayfa başına dört kayıt 79 kayıtlarıyla aracılığıyla sayfalama, örneğin, daha sonra sayfa sayısı 20'dir (79 tavan / 4). Sayısal disk belleği arabirimi de kullanıyorsanız, bu bilgileri bize görüntülemek için kaç tane sayısal sayfa düğmelerini konusunda bilgilendirir; disk belleği arabirimimizi İleri veya son düğmelerini içeriyorsa, sayfa sayısı İleri veya son düğmelerini devre dışı bırakmak ne zaman belirlemek için kullanılır.

Disk belleği arabirimi Son düğmesine içeriyorsa, Son düğmesine tıklandığında, biz son sayfa dizini belirleyebilmesi aracılığıyla havuzda kayıtlarının toplam sayısı Geri göndermeler anımsanacağını zorunludur. Bu kolaylaştırmak için oluşturma bir `TotalRowCount` durumunu görüntülemek için kendi değerini devam ederse ASP.NET sayfası s arka plandaki kod sınıfı bir özellik:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample5.cs)]

Ek olarak `TotalRowCount`kolayca erişme sayfa boyutu, sayfa dizini için salt okunur sayfa düzeyinde özellikler oluşturmak için bir dakikanızı ayırın ve sayfa sayısı:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample6.cs)]

## <a name="determining-the-total-number-of-records-being-paged-through"></a>Kayıtların aracılığıyla havuzda toplam sayısını belirleme

`PagedDataSource` Nesnesi döndürülen ObjectDataSource s'den `Select()` yöntemi içinde sahip *tüm* ürün kayıtları, rağmen yalnızca bir alt kümesini görüntülenir DataList. `PagedDataSource` s [ `Count` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.count.aspx) yalnızca DataList; gösterilecek öğe sayısını döndürür [ `DataSourceCount` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.datasourcecount.aspx) toplam öğe sayısını döndürür `PagedDataSource`. Bu nedenle, ASP.NET sayfası s atamak ihtiyacımız `TotalRowCount` özellik değeri, `PagedDataSource` s `DataSourceCount` özelliği.

Bunu gerçekleştirmek için ObjectDataSource s için bir olay işleyicisi oluşturun `Selected` olay. İçinde `Selected` erişim ObjectDataSource s dönüş değerine sahip olduğumuz olay işleyicisi `Select()` bu durumda, yöntem `PagedDataSource`.


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample7.cs)]

## <a name="displaying-the-requested-page-of-data"></a>İstenen sayfanın veri görüntüleme

Kullanıcı düğmeleri disk belleği arabiriminde tıklattığında veri istenen sayfanın görüntüleme gerekir. İstenen sayfanın veri kullanımı göstermek için belirtilen sayfalama parametreleri querystring bu yana `Response.Redirect(url)` kullanıcı s tarayıcı yeniden isteği için `Paging.aspx` uygun disk belleği parametrelere sahip sayfa. Örneğin, verilerin ikinci sayfasında görüntülenecek biz kullanıcıya yeniden yönlendirme `Paging.aspx?pageIndex=1`.

Bu kolaylaştırmak için oluşturma bir `RedirectUser(sendUserToPageIndex)` kullanıcıya yönlendiren yöntemi `Paging.aspx?pageIndex=sendUserToPageIndex`. Ardından, dört düğmesinden bu yöntemi çağırabilmeniz `Click` olay işleyicileri. İçinde `FirstPage` `Click` olay işleyicisi, çağrı `RedirectUser(0)`, ilk sayfasına; Göndereceğim `PrevPage` `Click` olay işleyicisi, kullanım `PageIndex - 1` sayfa dizini; olarak ve benzeri.


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample8.cs)]

İle `Click` olay işleyicileri tamamlamak, DataList s kayıtları aracılığıyla düğmelere tıklayarak belleğine alınabilen. Bunu denemek için bir dakikanızı ayırın!

## <a name="disabling-paging-interface-controls"></a>Disk belleği arabirimi denetimleri devre dışı bırakma

Tüm dört düğmeleri görüntülenmesini sayfa bağımsız olarak şu anda etkin. Ancak, veri ve sonraki ve son düğmeleri'nın ilk sayfasında son sayfaya gösterilirken gösterilirken ilk ve önceki düğmelerini devre dışı bırakmak istiyoruz. `PagedDataSource` ObjectDataSource s tarafından döndürülen nesne `Select()` yöntemi olan özellikler [ `IsFirstPage` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.isfirstpage.aspx) ve [ `IsLastPage` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.pageddatasource.islastpage.aspx) , biz görüntüleme varsa belirlemek inceleyeceğiz ilk veya son sayfasına veri.

ObjectDataSource s aşağıdakileri ekleyin `Selected` olay işleyicisi:


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample9.cs)]

Bu eklenmesiyle ilk ve önceki düğmeleri sonraki ve son düğmeleri son sayfayı görüntülerken devre dışı bırakılacak sırada ilk sayfasında, görüntülerken devre dışı bırakılır.

Let s tamamlamak disk belleği arabirimi kullanıcı bilgilendirerek ne sayfa bunlar şu anda görüntüleme ve kaç tane toplam sayfa mevcut. Bir etiket Web denetimi sayfasına ekleyin ve ayarlayın, `ID` özelliğine `CurrentPageNumber`. Ayarlama, `Text` ObjectDataSource s seçili olay işleyicisi böyle bir özellik görüntülenmesini geçerli sayfa içerir (`PageIndex + 1`) ve toplam sayfa sayısı (`PageCount`).


[!code-csharp[Main](paging-report-data-in-a-datalist-or-repeater-control-cs/samples/sample10.cs)]

Şekil 10 gösteren `Paging.aspx` ilk sitesini ziyaret ettiğinizde. Sorgu dizesi boş olduğundan, DataList ilk dört ürünleri göstermek için varsayılan olarak; İlk ve önceki düğmelerini devre dışı bırakılır. İleri'yi tıklatmadan (bkz. Şekil 11) sonraki dört kayıtları görüntüler; İlk ve önceki düğmelerini şimdi etkinleştirilir.


[![İlk sayfa verileri görüntülenir](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image23.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image22.png)

**Şekil 10**: ilk sayfa veri görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image24.png))


[![İkinci sayfa veri görüntülenir](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image26.png)](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image25.png)

**Şekil 11**: ikinci sayfasında veri görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](paging-report-data-in-a-datalist-or-repeater-control-cs/_static/image27.png))


> [!NOTE]
> Disk belleği arabirimi daha fazla sayfa başına görüntülemek için kaç tane sayfaları belirtmesini vererek geliştirilebilir. Örneğin, bir DropDownList listeleme sayfa boyutu seçenekleri 5, 10, 25, 50 ve tüm gibi eklenemedi. Bir sayfa boyutuna seçtikten sonra kullanıcının yeniden yönlendirilmesi gerekir `Paging.aspx?pageIndex=0&pageSize=selectedPageSize`. Bu geliştirme, okuyucu için bir alıştırma olarak uygulama I bırakın.


## <a name="using-custom-paging"></a>Özel sayfalama kullanma

Verimsiz varsayılan disk belleği tekniği kullanarak verileri DataList sayfalarıyla. Yeterince büyük miktarlarda verinin disk belleği, özel sayfalama kullanılması zorunludur. Uygulama Ayrıntıları biraz farklılık gösterir ancak bir DataList Özel sayfalama uygulama arkasında kavramları varsayılan disk belleği ile aynıdır. Özel disk belleği ile kullanmak `ProductBLL` s sınıfı `GetProductsPaged` yöntemi (yerine `GetProductsAsPagedDataSource`). ' Da anlatıldığı gibi [verimli bir şekilde disk belleği üzerinden büyük miktarlarda veri](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) öğretici, `GetProductsPaged` başlangıç satır dizini ve verilen döndürülecek satır sayısını geçirilmelidir. Bu parametreler sorgu dizesi olduğu gibi sürdürülebilir `pageIndex` ve `pageSize` varsayılan disk belleği kullanılan parametreler.

Orada s bu yana hiçbir `PagedDataSource` özel disk belleği ile alternatif teknikleri aracılığıyla ve olup disk belleği kayıtların toplam sayısını belirlemek için kullanılması gereken biz verilerin ilk veya son sayfasına görüntüleme re. `TotalNumberOfProducts()` Yönteminde `ProductsBLL` sınıfı aracılığıyla havuzda ürünleri toplam sayısını döndürür. Sıfır olursa ilk sayfasında görüntülenen sonra veri'nın ilk sayfasında görüntülenen varsa belirlemek için başlangıç satır dizini inceleyin. Başlangıç satırı dizini artı döndürülecek en fazla satır ise kayıtları aracılığıyla havuzda toplam sayısına eşit veya daha büyük son sayfaya görüntüleniyor.

Sonraki öğreticide daha ayrıntılı biçimde özel sayfalama uygulama ele alacağız.

## <a name="summary"></a>Özet

DataList ne yineleyici dışı sunarken GridView, DetailsView, disk belleği destek bulundu ve FormView denetimleri, bu tür işlevselliği en az çaba ile eklenebilir. İçinde ürünleri kümesinin tamamını kaydırmak için varsayılan disk belleği uygulamak için en kolay yolu olan bir `PagedDataSource` ve ardından bağlamak `PagedDataSource` DataList veya yineleyici. Bu öğreticide eklediğimiz `GetProductsAsPagedDataSource` yönteme `ProductsBLL` dönmek için sınıf `PagedDataSource`. `ProductsBLL` Sınıf zaten var. özel sayfalama için gereken yöntemleri `GetProductsPaged` ve `TotalNumberOfProducts`.

Özel sayfalama görüntülenecek kayıtları hassas kümesi ya da tüm kayıtları alınırken yanı sıra bir `PagedDataSource` için varsayılan disk belleği, biz de el ile disk belleği arabirimi eklemeniz gerekir. Bu öğretici için oluşturduğumuz sonraki, önceki, ilk olarak, son dört düğmesi Web denetimleriyle arabirim. Ayrıca, geçerli sayfa numarası ve toplam sayfa sayısı görüntüleyen bir etiket denetimi eklendi.

Sonraki öğreticide DataList ve yineleyici sıralama desteği eklemek nasıl göreceğiz. Hem disk belleği ve (varsayılan ve özel sayfalama kullanarak örnekler) sıralanmış bir DataList oluşturma de göreceğiz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Liz Shulok, Ken Pespisa ve Bernadette Leigh yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Sonraki](sorting-data-in-a-datalist-or-repeater-control-cs.md)
