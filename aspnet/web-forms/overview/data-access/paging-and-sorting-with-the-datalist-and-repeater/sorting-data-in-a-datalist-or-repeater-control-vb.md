---
uid: web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
title: "Bir DataList veya yineleyici denetimi (VB) verileri sıralama | Microsoft Docs"
author: rick-anderson
description: "Bu öğreticide biz bir DataList veya yineleyici verileri oluşturmak nasıl yanı sıra nasıl DataList ve yineleyici desteği sıralama dahil inceleyeceğiz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 97c13898-0741-45f9-b3fa-7540ab1679e6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting-with-the-datalist-and-repeater/sorting-data-in-a-datalist-or-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: e3f505e525fd5e701bb40dc3e6467b880bf75447
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="sorting-data-in-a-datalist-or-repeater-control-vb"></a>Bir DataList veya yineleyici denetimi (VB) verileri sıralama
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_45_VB.exe) veya [PDF indirin](sorting-data-in-a-datalist-or-repeater-control-vb/_static/datatutorial45vb1.pdf)

> Bu öğreticide biz DataList veya verileri disk belleği ve sıralanmış yineleyici oluşturmak nasıl yanı sıra nasıl DataList ve yineleyici desteği sıralama dahil inceleyeceğiz.


## <a name="introduction"></a>Giriş

İçinde [önceki öğretici](paging-report-data-in-a-datalist-or-repeater-control-vb.md) biz DataList için sayfalama desteği ekleme inceledi. Yeni bir yöntemi oluşturduğumuz `ProductsBLL` sınıfı (`GetProductsAsPagedDataSource`) döndürülen bir `PagedDataSource` nesnesi. Ne zaman bir DataList veya yineleyici bağlı, DataList veya yineleyici yalnızca istenen sayfasında veri görüntüler. Bu teknik ne dahili olarak GridView, DetailsView ve FormView denetimleri tarafından yerleşik varsayılan disk belleği işlevleri sağlamak için kullanılan benzer.

Sayfalama desteği sunan yanı sıra GridView destek sıralama kutu dışı de içerir. DataList ne yineleyici yerleşik bir sıralama işlevi sağlar; Ancak, sıralama özellikleri kodunun bir bit eklenebilir. Bu öğreticide biz DataList veya verileri disk belleği ve sıralanmış yineleyici oluşturmak nasıl yanı sıra nasıl DataList ve yineleyici desteği sıralama dahil inceleyeceğiz.

## <a name="a-review-of-sorting"></a>Sıralama bir gözden geçirme

İçinde gördüğümüz gibi [disk belleği ve rapor verilerini sıralama](../paging-and-sorting/paging-and-sorting-report-data-vb.md) öğretici, GridView denetiminin destek sıralama kutu dışı sağlar. Her bir GridView alan bir ilişkili olabilir `SortExpression`, verileri sıralamak veri alan gösterir. Zaman GridView s `AllowSorting` özelliği ayarlanmış `true`, sahip her bir GridView alan bir `SortExpression` özellik değerine sahip üstbilgisinde LinkButton çizilir. Bir kullanıcı belirli bir GridView alan s üst bilgisi tıkladığında, geri gönderimin oluştuğu ve veriler s tıklatılan alanı göre sıralanır `SortExpression`.

GridView denetiminin bir `SortExpression` depolayan özelliği de `SortExpression` GridView alanın veri değerlerine göre sıralanır. Ayrıca, bir `SortDirection` özelliği, veri artan veya azalan (iki kez art arda, sıralama düzeni belirli GridView s alanı üstbilgi bağlantı yükseğe bir kullanıcı tıklama varsa) sıralanacak olup olmadığını belirtir.

GridView, veri kaynağı denetimine bağlı olduğunda kapalı aktarır kendi `SortExpression` ve `SortDirection` özellikleri veri kaynağı denetimi. Veri kaynağı denetimi verileri alır ve ardından sağlanan göre sıralar `SortExpression` ve `SortDirection` özellikleri. Verileri sıralama sonra veri kaynağı denetimi, GridView döndürür.

Bu işlev DataList veya yineleyici denetimleri ile çoğaltmak için biz gerekir:

- Sıralama arabirimi oluşturma
- Göre sıralamak için veri alan ve artan veya azalan sıralama görüntülenmeyeceğini unutmayın
- Belirli veri alanına göre verileri sıralamak için ObjectDataSource isteyin

Biz bu üç adım 3 ve 4 görevlerinde üstesinden gelmek. Nasıl disk belleği hem de bir DataList veya yineleyici desteği sıralama dahil inceleyeceğiz.

## <a name="step-2-displaying-the-products-in-a-repeater"></a>2. adım: bir yineleyici ürünleri görüntüleme

Sıralama ile ilgili işlevlerden herhangi birini uygulama hakkında endişelenmeniz önce ürünleri yineleyici denetiminde listeleyerek Başlat s olanak tanır. Başlangıç açarak `Sorting.aspx` sayfasındaki `PagingSortingDataListRepeater` klasör. Yineleyici denetim web ayarı sayfasına ekleme kendi `ID` özelliğine `SortableProducts`. Yineleyici s akıllı etiketten adlı yeni bir ObjectDataSource oluşturma `ProductsDataSource` ve verilerin alınacağı yapılandırın `ProductsBLL` s sınıfı `GetProducts()` yöntemi. (Hiçbiri) INSERT, UPDATE ve DELETE sekmelerdeki aşağı açılan listelerden seçeneğini seçin.


[![Bir ObjectDataSource oluşturun ve GetProductsAsPagedDataSource() yöntemi kullanacak şekilde yapılandırın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image2.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image1.png)

**Şekil 1**: bir ObjectDataSource oluşturma ve kullanımı için yapılandırma `GetProductsAsPagedDataSource()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image3.png))


[![Güncelleştirme, ekleme, aşağı açılan listeler ayarlayın ve sekmeleri (hiçbiri) silme](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image5.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image4.png)

**Şekil 2**: güncelleştirme, ekleme, aşağı açılan listeler ayarlayın ve silme (hiçbiri) sekmeler ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image6.png))


Aksine DataList ile Visual Studio otomatik olarak oluşturmaz bir `ItemTemplate` bir veri kaynağına bağlama sonra yineleyici denetimi için. Ayrıca, bu eklediğimiz gerekir `ItemTemplate` yineleyici denetim s akıllı etiket DataList s bulunan Şablonları Düzenle seçeneği eksik gibi bildirimli olarak. Let s kullanmak aynı `ItemTemplate` önceki öğretici görüntülenen s ürün adı, üretici ve kategori.

Ekledikten sonra `ItemTemplate`, yineleyici ve ObjectDataSource s bildirim temelli biçimlendirmesi aşağıdakine benzer görünmelidir:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample1.aspx)]

Şekil 3'te bir tarayıcıdan görüntülendiğinde bu sayfada görüntülenir.


[![Her ürünün s adı, üretici ve kategori görüntülenir](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image8.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image7.png)

**Şekil 3**: her ürün s adı, üretici ve kategori görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image9.png))


## <a name="step-3-instructing-the-objectdatasource-to-sort-the-data"></a>3. adım: verileri sıralamak için ObjectDataSource bilgilendirerek

Yineleyicideki görüntülenen verileri sıralamak için biz verilerin sıralanması gerektiğini sıralama ifadesi ObjectDataSource bildirmeniz gerekir. ObjectDataSource verisini alır. önce ilk ateşlenir kendi [ `Selecting` olay](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.selecting.aspx), bize bir sıralama ifadesi belirtmek bir fırsat sağlar. `Selecting` Olay işleyicisi türünde bir nesne geçirilen [ `ObjectDataSourceSelectingEventArgs` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.aspx), adında bir özellik olan [ `Arguments` ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasourceselectingeventargs.arguments.aspx) türü [ `DataSourceSelectArguments` ](https://msdn.microsoft.com/en-us/library/system.web.ui.datasourceselectarguments.aspx). `DataSourceSelectArguments` Sınıf verilerle ilgili istekleri için veri kaynağı denetimi veri tüketiciden geçirmek için tasarlanmıştır ve içeren bir [ `SortExpression` özelliği](https://msdn.microsoft.com/en-us/library/system.web.ui.datasourceselectarguments.sortexpression.aspx).

ObjectDataSource ASP.NET sayfasından sıralama bilgileri geçirmek için bir olay işleyicisi oluşturun `Selecting` olay ve aşağıdaki kodu kullanın:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample2.vb)]

*SortExpression* değeri verileri (örneğin, ProductName) göre sıralamak için veri alanının adı atanmış. Sıralama yönü ile ilgili bir özellik yok, azalan düzende verileri sıralamak istiyorsanız, bu nedenle DESC dize eklemek için *sortExpression* değeri (örneğin, ProductName DESC).

Bir tane için bazı farklı sabit kodlanmış değerler deneyin *sortExpression* ve sonuçları bir tarayıcıda sınayın. ProductName DESC olarak kullanırken, Şekil 4'te gösterildiği gibi *sortExpression*, ürünleri ters alfabetik sırada kendi ada göre sıralanır.


[![Ürünleri adı ters alfabetik sırada sıralanır.](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image11.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image10.png)

**Şekil 4**: Ürün adı ters alfabetik sırada sıralanır ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image12.png))


## <a name="step-4-creating-the-sorting-interface-and-remembering-the-sort-expression-and-direction"></a>Adım 4: sıralama arabirimi oluşturma ve sıralama ifadesi ve yönü anımsama

GridView desteği sıralama kapatma her s sıralanabilir alanı üstbilgi metni LinkButton dönüştürür, tıklatıldığında, verileri buna göre sıralar. Sıralama bir arabirim, burada, veriler düzgün sütunlarında düzenlendiğini GridView için anlamlıdır. DataList ve yineleyici denetimleri, ancak, farklı bir sıralama arabirimi gereklidir. Bir ortak sıralama (aksine, bir kılavuz veri), bir veri listesini arabirimidir açılan listesini veri sıralanabilir alanları sağlar. Bu öğretici için böyle bir arabirim uygulamak s olanak tanır.

Yukarıdaki DropDownList Web denetim ekleme `SortableProducts` Yineleyici ve kümesi kendi `ID` özelliğine `SortBy`. Özellikler penceresinden içinde üç noktaya tıklayın `Items` yukarı ListItem Koleksiyonu Düzenleyicisi getirmek için özellik. Ekleme `ListItem` verileri göre sıralamak için s `ProductName`, `CategoryName`, ve `SupplierName` alanları. Ayrıca bir `ListItem` ürün adı ters alfabetik sırada sıralamak için.

`ListItem` `Text` Özellikleri (örneğin, adı), herhangi bir değere ayarlanabilir ancak `Value` özellikleri ayarlanmış olması gerekir (örneğin, ProductName) veri alanının adı. Azalan düzende sonuçları sıralamak için DESC dize ProductName DESC gibi veri alan adı ekleyin.


![Sıralanabilir veri alanların her biri için bir ListItem ekleme](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image13.png)

**Şekil 5**: ekleme bir `ListItem` sıralanabilir veri alanların her biri için


Son olarak, bir düğme Web denetimi DropDownList sağındaki ekleyin. Ayarlama, `ID` için `RefreshRepeater` ve kendi `Text` yenileme özelliği.

Oluşturduktan sonra `ListItem` s ve Yenile düğmesini ekleyerek, DropDownList ve düğmesi s Tanımlayıcı Sözdizimi görünmelidir aşağıdakine benzer:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample3.aspx)]

Tam sıralama DropDownList ile sonraki ObjectDataSource s güncelleştirmek ihtiyacımız `Selecting` olan seçili kullanan olay işleyicisi `SortBy``ListItem` s `Value` özelliği bir sabit kodlanmış sıralama ifadesi aksine.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample4.vb)]

İlk sitesini ziyaret ettiğinde bu noktada ürünleri tarafından başlangıçta sıralanacağını `ProductName` şekliyle veri alanı s `SortBy` `ListItem` varsayılan olarak seçili (bkz. Şekil 6). Bir farklı seçeneği kategori gibi sıralama ve yenileme tıklatarak seçerek geri gönderimin neden ve Şekil 7'de gösterildiği gibi veri kategorisi adıyla yeniden Sırala.


[![Ürün başlangıçta sıralanmış adı değil](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image15.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image14.png)

**Şekil 6**: ürünleri olan başlangıçta sıralanmış adı ile ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image16.png))


[![Şimdi sıralanmış kategoriye göre ürün değil](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image18.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image17.png)

**Şekil 7**: ürünleri olan şimdi sıralanmış kategoriye göre ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image19.png))


> [!NOTE]
> Yenile düğmesini tıklatarak yineleyici s görünüm durumu devre dışı olduğundan, otomatik olarak böylelikle kendi veri kaynağına her geri gönderme üzerinde rebind yineleyici yol yeniden sıralanmış verilerin neden olur. Önceden açılan sıralama değiştirme etkinse, yineleyici s görünüm durumu bırakılırsa t won listesi sıralama düzenini hiçbir etkisi vardır. Bu sorunu gidermek için Yenile düğmesini s için bir olay işleyicisi oluşturun `Click` olay ve yeniden bağlamasını kendi veri kaynağına yineleyici (s yineleyici çağırarak `DataBind()` yöntemi).


## <a name="remembering-the-sort-expression-and-direction"></a>Yön ve sıralama ifadesi anımsama

Sıralama dışı burada ilgili Geri göndermeler oluşabilir, bir sayfa üzerinde bir sıralanabilir DataList veya yineleyici oluştururken, sıralama ifadesi ve yönü Geri göndermeler arasında anımsanacağını s kesinliği. Örneğin, biz Bu öğreticide her ürünle Sil düğmesini içerecek biçimde yineleyici güncelleştirilmiş düşünün. Kullanıcı Sil düğmesini tıklattığında d biz seçili ürün silip yineleyici verileri yeniden bağlayın biraz kod çalıştırın. Sıralama ayrıntıları geri gönderme arasında kalıcı değildir, ekranda görüntülenen verileri özgün sıralama düzeni döner.

Bu öğretici için DropDownList örtük olarak sıralama ifadesi ve yön kendi Görünüm durumunda bize kaydeder. Farklı bir sıralama arabirim bir sağlanan çeşitli sıralama seçeneklerini söyleyin, LinkButtons birlikte kullanıyorsanız, d halletmeniz sıralama düzenini Geri göndermeler arasında hatırlamak ihtiyacımız. Bu sorgu dizesi veya diğer durumu kalıcılığını teknik aracılığıyla sıralama parametresi dahil ederek sıralama parametreleri sayfa s görünüm durumu depolayarak bunu sağlayabilirsiniz.

Bu öğreticide gelecekteki örnekler sayfa s görünüm durumu sıralama Ayrıntılar kalıcı hale getirmek nasıl keşfedin.

## <a name="step-5-adding-sorting-support-to-a-datalist-that-uses-default-paging"></a>5. adım: Varsayılan disk belleği kullanan bir DataList destek sıralama ekleme

İçinde [önceki öğretici](paging-report-data-in-a-datalist-or-repeater-control-vb.md) biz varsayılan sayfalama DataList ile uygulama inceledi. Disk belleğine alınan verileri sıralama özelliği eklemek için bu önceki örnek genişletilerek s olanak tanır. Başlangıç açarak `SortingWithDefaultPaging.aspx` ve `Paging.aspx` içinde sayfaları `PagingSortingDataListRepeater` klasör. Gelen `Paging.aspx` sayfasında, sayfa s bildirim temelli biçimlendirme görüntülemek için kaynak düğmesine tıklayın. Seçili metni kopyalayın (bkz. Şekil 8) ve bildirim temelli biçimlerini yapıştırma `SortingWithDefaultPaging.aspx` arasında `<asp:Content>` etiketler.


[![Bildirim temelli biçimlendirmede çoğaltmak &lt;asp: içerik&gt; SortingWithDefaultPaging.aspx Paging.aspx gelen etiketleri](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image21.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image20.png)

**Şekil 8**: bildirim temelli biçimlendirmede çoğaltmak `<asp:Content>` gelen etiketleri `Paging.aspx` için `SortingWithDefaultPaging.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image22.png))


Bildirim temelli biçimlendirme kopyaladıktan sonra yöntemleri ve özellikleri kopyalamak `Paging.aspx` sayfasında s arka plandaki kod sınıfı için arka plan kodu sınıfına `SortingWithDefaultPaging.aspx`. Ardından, görüntülemek için bir dakikanızı ayırın `SortingWithDefaultPaging.aspx` sayfasını bir tarayıcıda. Aynı işlevselliğe ve görünüm olarak göstermelidir `Paging.aspx`.

## <a name="enhancing-productsbll-to-include-a-default-paging-and-sorting-method"></a>Disk belleği ve sıralama yöntemi varsayılan içerecek şekilde ProductsBLL geliştirme

Önceki öğreticide oluşturduğumuz bir `GetProductsAsPagedDataSource(pageIndex, pageSize)` yönteminde `ProductsBLL` döndürülen sınıfı bir `PagedDataSource` nesnesi. Bu `PagedDataSource` nesnesi doldurulmuş ile *tüm* ürünlerin (BLL s aracılığıyla `GetProducts()` yöntemi), ancak ne zaman DataList yalnızca belirtilen karşılık gelen kayıtları bağlı *PageIndex* ve *pageSize* giriş parametreleri gösterilir.

Bu öğreticide daha önce sıralama destek ObjectDataSource s sıralama ifadesi belirterek eklediğimiz `Selecting` olay işleyicisi. Bu çalışır iyi ObjectDataSource, gibi sıralanabilir bir nesne döndürüldüğünde `ProductsDataTable` tarafından döndürülen `GetProducts()` yöntemi. Ancak, `PagedDataSource` tarafından döndürülen nesne `GetProductsAsPagedDataSource` yöntemi iç veri kaynağı sıralamayı desteklemiyor. Bunun yerine, döndürülen sonuçları sıralamak ihtiyacımız `GetProducts()` yöntemi *önce* biz bunu koymak `PagedDataSource`.

Bunu gerçekleştirmek için yeni bir yöntemi oluşturun `ProductsBLL` sınıfı, `GetProductsSortedAsPagedDataSource(sortExpression, pageIndex, pageSize)`. Sıralanacak `ProductsDataTable` tarafından döndürülen `GetProducts()` yöntemini belirtin `Sort` varsayılan özelliği `DataTableView`:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample5.vb)]

`GetProductsSortedAsPagedDataSource` Yöntemi yalnızca biraz daha farklı gelen `GetProductsAsPagedDataSource` önceki öğreticide oluşturduğunuz yöntemi. Özellikle, `GetProductsSortedAsPagedDataSource` ek bir giriş parametresi kabul `sortExpression` ve bu değeri atar `Sort` özelliği `ProductDataTable` s `DefaultView`. Kodu daha sonra birkaç satırlık `PagedDataSource` s veri kaynağı nesnesi atandığı `ProductDataTable` s `DefaultView`.

## <a name="calling-the-getproductssortedaspageddatasource-method-and-specifying-the-value-for-the-sortexpression-input-parameter"></a>GetProductsSortedAsPagedDataSource yöntemini çağırıp SortExpression giriş parametresinin değeri belirtme

İle `GetProductsSortedAsPagedDataSource` tam, sonraki adıma yöntemdir Bu parametre için değer sağlamak için. ObjectDataSource `SortingWithDefaultPaging.aspx` çağırmak için şu anda yapılandırılmış `GetProductsAsPagedDataSource` yöntemi ve iki giriş parametreleri, iki aracılığıyla geçişinde `QueryStringParameters`, içinde belirtilen `SelectParameters` koleksiyonu. Bu iki `QueryStringParameters` belirtmek için kaynak `GetProductsAsPagedDataSource` s yöntemi *PageIndex* ve *pageSize* parametreleri gelen sorgu dizesi alanlardan `pageIndex` ve `pageSize`.

ObjectDataSource s güncelleştirme `SelectMethod` olan yeni çağırır özelliğini `GetProductsSortedAsPagedDataSource` yöntemi. Ardından, yeni bir ekleyin `QueryStringParameter` böylece *sortExpression* giriş parametresi querystring alanından erişilen `sortExpression`. Ayarlama `QueryStringParameter` s `DefaultValue` ProductName.

Bu değişikliklerden sonra ObjectDataSource s bildirim temelli biçimlendirme gibi görünmelidir:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample6.aspx)]

Bu noktada, `SortingWithDefaultPaging.aspx` sayfa sıralama sonuçları alfabetik olarak ürün adına göre (bkz. Şekil 9). Varsayılan olarak, bir ProductName değeri olarak geçirilen, bunun nedeni `GetProductsSortedAsPagedDataSource` s yöntemi *sortExpression* parametresi.


[![Varsayılan olarak, sonuçlar ProductName göre sıralanır.](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image24.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image23.png)

**Şekil 9**: varsayılan olarak, sonuçları tarafından sıralanır `ProductName` ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image25.png))


El ile eklerseniz, bir `sortExpression` sorgu dizesi alanı gibi `SortingWithDefaultPaging.aspx?sortExpression=CategoryName` sonuçları sıralanmış belirtilen tarafından `sortExpression`. Ancak, bu `sortExpression` parametresi dahil edilmemiştir sorgu dizesine verilerin başka bir sayfaya taşırken. Aslında, İleri'yi veya son sayfasında tıklatarak izin ver düğmeleri bize geri alır `Paging.aspx`! Ayrıca, orada s şu anda hiçbir sıralama arabirim. Bir kullanıcı disk belleğine alınan verileri sıralama düzenini değiştirebilirsiniz yalnızca doğrudan sorgu dizesi işleyerek yoludur.

## <a name="creating-the-sorting-interface"></a>Sıralama arabirimi oluşturma

İlk güncelleştirmek ihtiyacımız `RedirectUser` kullanıcıya gönderilecek yöntemi `SortingWithDefaultPaging.aspx` (yerine `Paging.aspx`) ve dahil etmek için `sortExpression` sorgu dizesi değeri. Biz de bir salt okunur sayfa adlı düzeyinde eklemelisiniz `SortExpression` özelliği. Bu özellik, benzer `PageIndex` ve `PageSize` önceki öğreticide oluşturduğunuz özellikleri değerini döndürür `sortExpression` varsa querystring alan ve varsayılan değer (ProductName) Aksi takdirde.

Şu anda `RedirectUser` yöntemi yalnızca tek bir giriş parametresi görüntülemek için sayfanın dizini kabul eder. Ancak, ne zaman bir sıralama ifadesi sorgu dizesinde belirtilen hangi s dışında kullanarak verilerin belirli bir sayfa için kullanıcıyı yeniden yönlendirmek için istiyoruz zamanlar olabilir. Birazdan düğmesi Web denetimleri verileri belirtilen sütuna göre sıralamak için bir dizi içerecektir bu sayfa için sıralama arabirimi oluşturacağız. Bu düğmeleri birini tıklandığında uygun sıralama ifadesi değeri geçirme kullanıcıyı yeniden yönlendirmek istiyoruz. Bu işlevselliği sağlayacak şekilde iki sürümü oluşturma `RedirectUser` yöntemi. Birinci, ikinci bir sayfa dizini ve sıralama ifadesi kabul ederken görüntülenecek yalnızca sayfa dizini kabul etmelidir.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample7.vb)]

Bu öğretici ilk örnekte bir DropDownList kullanarak bir sıralama arabirimi oluşturduk. Bu örnekte, let s kullanmak göre sıralamak için DataList biri konumlandırılmış üç düğme Web denetimleri `ProductName`, bir için `CategoryName`, diğeri `SupplierName`. Ayarı üç düğme Web denetimleri ekleme kendi `ID` ve `Text` özellikleri uygun şekilde:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample8.aspx)]

Ardından, oluşturun bir `Click` her olay işleyicisi. Olay işleyicileri çağırmalıdır `RedirectUser` yöntemi, kullanıcının uygun sıralama ifadesi kullanarak ilk sayfasının döndürüyor.


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample9.vb)]

İlk sitesini ziyaret ettiğinde verileri ürün adına göre alfabetik (geri Şekil 9'a bakın). Veri ikinci sayfasına ilerleyin ve sıralama tarafından kategori düğmesini tıklatıp için İleri düğmesine tıklayın. Kategori adına göre sıralanmış verilerin ilk sayfasına bu bize döndürür (bkz. Şekil 10). Benzer şekilde, tedarikçi düğmesi göre sıralamayı verileri veri ilk sayfadan başlayarak sağlayıcısına göre sıralar. Verileri üzerinden disk belleği gibi sıralama seçimi hatırlanır. Şekil 11 kategoriye göre sıralama ve veri on üçüncü sayfasına ilerledikten sonra sayfada görüntülenir.


[![Ürünleri kategoriye göre sıralanır.](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image27.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image26.png)

**Şekil 10**: ürün kategorisine göre sıralanır ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image28.png))


[![Anımsanacak, disk belleği ile veri sıralama ifadesidir](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image30.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image29.png)

**Şekil 11**: sıralama ifadesi verilerdir hatırlanan, disk belleği aracılığıyla ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image31.png))


## <a name="step-6-custom-paging-through-records-in-a-repeater"></a>6. adım: Bir yineleyici kayıtlarında aracılığıyla özel disk belleği

DataList örnek adımda verimsiz varsayılan disk belleği tekniği kullanarak verileri 5 sayfalarıyla incelendi. Yeterince büyük miktarlarda verinin disk belleği, özel sayfalama kullanılması zorunludur. Geri [verimli bir şekilde disk belleği üzerinden büyük miktarlarda veri](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md) ve [özel disk belleğine alınan verileri sıralama](../paging-and-sorting/sorting-custom-paged-data-vb.md) öğreticileri, biz incelenmesi BLL için varsayılan ve özel disk belleği ile oluşturulan yöntemleri arasındaki farklılıkları özel disk belleği ve özel disk belleğine alınan verileri sıralama kullanan. Özellikle, bu iki önceki öğreticileri için aşağıdaki üç yöntemi eklediğimiz `ProductsBLL` sınıfı:

- `GetProductsPaged(startRowIndex, maximumRows)`belirli bir alt kümesini başlayarak kayıtları döndürür *startRowIndex* ve aşmadan *maximumRows*.
- `GetProductsPagedAndSorted(sortExpression, startRowIndex, maximumRows)`belirli bir alt kümesini tarafından belirtilen sıralanmış kayıtları döndürür *sortExpression* giriş parametresi.
- `TotalNumberOfProducts()`kayıtlarının toplam sayısını sağlar `Products` veritabanı tablosu.

Bu yöntemler, verimli bir şekilde sayfasında ve DataList veya yineleyici denetimini kullanarak verilerine sıralamak için kullanılabilir. Bunu göstermek için özel sayfalama desteği ile yineleyici denetim oluşturarak başlayın s sağlar; Sıralama yeteneklerini sonra ekleyeceğiz.

Açık `SortingWithCustomPaging.aspx` sayfasındaki `PagingSortingDataListRepeater` klasör ve bir yineleyici sayfasına ekleme ayarını kendi `ID` özelliğine `Products`. Yineleyici s akıllı etiketten adlı yeni bir ObjectDataSource oluşturma `ProductsDataSource`. Kendi verileri seçmek için yapılandırma `ProductsBLL` s sınıfı `GetProductsPaged` yöntemi.


[![ObjectDataSource s ProductsBLL sınıfı GetProductsPaged yöntemi kullanmak üzere yapılandırma](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image33.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image32.png)

**Şekil 12**: ObjectDataSource kullanılacak yapılandırma `ProductsBLL` s sınıfı `GetProductsPaged` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image34.png))


Güncelleştirme, ekleme, açılan listeleri ayarlamak ve sekmeleri (hiçbiri) SİLİN ve sonra İleri düğmesine tıklayın. Veri Kaynağı Yapılandırma Sihirbazı'nı şimdi kaynakları için ister `GetProductsPaged` s yöntemi *startRowIndex* ve *maximumRows* giriş parametreleri. Çünkü, bu giriş parametreleri yok sayılır. Bunun yerine, *startRowIndex* ve *maximumRows* değerleri geçirilir içinde aracılığıyla `Arguments` ObjectDataSource s özelliğinde `Selecting` belirttiğimize gibi olay işleyicisi *sortExpression* Bu öğretici s ilk gösteride. Bu nedenle, parametre kaynağı None Ayarlama Sihirbazı'nda açılan listeleri bırakın.


[![Parametre kaynakları kümesi None olarak bırakın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image36.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image35.png)

**Şekil 13**: parametre kaynakları Yok'a bırakın ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image37.png))


> [!NOTE]
> Yapmak *değil* ObjectDataSource s ayarlamak `EnablePaging` özelliğine `true`. Bu otomatik olarak kendi içerecek şekilde ObjectDataSource neden olacak *startRowIndex* ve *maximumRows* parametreleri `SelectMethod` s varolan parametre listesi. `EnablePaging` Özelliği, bu denetimler, s ObjectDataSource belirli davranışından beklediğiniz olduğundan GridView, DetailsView ya da FormView denetimine veri bağlama Özel havuzda olduğunda yararlıdır yalnızca kullanılabilir `EnablePaging` özelliği `true`. Biz DataList ve yineleyici sayfalama desteğini el ile eklemek zorunda olduğundan, bu özelliği bırakın `false` (varsayılan), biz gerekli işlevselliği doğrudan ASP.NET sayfamızı içinde Fırından gibi.


Son olarak, yineleyici s tanımlamak `ItemTemplate` böylece s ürün adı, kategori ve tedarikçi gösterilir. Yineleyici ObjectDataSource s bildirim temelli söz dizimi ve bu değişikliklerden sonra aşağıdakine benzer görünmelidir:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample10.aspx)]

Bir tarayıcı aracılığıyla sayfasını ziyaret edin ve kayıt verdiğini unutmayın için bir dakikanızı ayırın. Bunun nedeni, biz henüz belirtmek için ve *startRowIndex* ve *maximumRows* parametre değerlerini; bu nedenle, değeri 0 olarak her ikisi için geçirilen. Bu değerleri belirtmek için olay işleyicisi ObjectDataSource s oluşturmak `Selecting` olay ve bu parametrelerin değerlerini programlı olarak sabit kodlanmış değerleri 0 ile 5, sırasıyla ayarla:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample11.vb)]

Bu değişiklikle, bir tarayıcı görüntülendiğinde sayfasında, ilk beş ürünlerini gösterir.


[![İlk beş kayıtlar görüntülenir](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image39.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image38.png)

**Şekil 14**: ilk beş kayıt görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image40.png))


> [!NOTE]
> Şekil 14'te listelenen ürünler için ürün adına göre sıralanacak durum `GetProductsPaged` verimli özel disk belleği sorgu gerçekleştirir saklı yordam siparişleri sonuçlarına göre `ProductName`.


Sayfaları arasında adım yapmalarına izin vermek için kimliğinizi başlangıç satırı dizini ve en fazla satır izlemenize ve bu değerleri arasında Geri göndermeler unutmayın gerekiyor. Varsayılan disk belleği örnekte bu değerleri kalıcı hale getirmek için sorgu dizesi alanları kullanılır; Bu Tanıtım için bu sayfayı s görünüm durumu bilgileri kalıcı s olanak tanır. Aşağıdaki iki özelliği oluşturun:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample12.vb)]

Ardından, kullandığı seçme olay işleyicisi kodunda güncelleştirmek `StartRowIndex` ve `MaximumRows` 0 ile 5, sabit kodlanmış değerler yerine özellikleri:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample13.vb)]

Bu noktada sayfamızı yalnızca ilk beş kayıtları görüntülenmeye devam eder. Ancak, bu özellikleri, yerinde biz re disk belleği arabirimimizi oluşturmak için hazır.

## <a name="adding-the-paging-interface"></a>Disk belleği arabirim ekleme

Aynı ilk, önceki, sonraki en son disk belleği, arabirim let s kullanın, hangi veri sayfasını görüntüleyen denetim görüntülenen etiketi Web dahil olmak üzere varsayılan disk belleği örnek ve kaç tane toplam sayfa mevcut kullanılır. Dört düğmesi Web denetimleri ve yineleyici altına etiketi ekleyin.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample14.aspx)]

Ardından, oluşturun `Click` dört düğmeleri için olay işleyicileri. Bu düğmeleri birini tıklatıldığında güncelleştirmek ihtiyacımız `StartRowIndex` ve yineleyici verileri yeniden bağlayın. Kod ilk, geri ve İleri düğmelerini için yeterince basittir ancak için Son düğmesini nasıl veri son sayfasına başlangıç satır dizini belirleriz? Bu dizin yanı sıra sonraki ve son düğmeleri etkinleştirilmesi gerekip gerekmediğini toplam kaç kayıt üzerinden disk belleği bilmeniz ihtiyacımız belirlemek mümkün hesaplamak için. Biz çağırarak belirleyebilirsiniz `ProductsBLL` s sınıfı `TotalNumberOfProducts()` yöntemi. Let s oluşturmak adlı bir salt okunur ve sayfa düzeyinde özellik `TotalRowCount` sonuçlarını döndürür `TotalNumberOfProducts()` yöntemi:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample15.vb)]

Bu özellik ile biz şimdi son sayfa s başlangıç satır dizini belirleyebilirsiniz. Özellikle, onu s tamsayı sonuç, `TotalRowCount` eksi 1 bölü `MaximumRows`, tarafından çarpılan `MaximumRows`. Biz şimdi yazabilirsiniz `Click` olay işleyicileri dört disk belleği arabirimi düğmeleri için:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample16.vb)]

Son olarak, biz veri ve sonraki ve son düğmeleri'nın ilk sayfasında son sayfayı görüntülerken görüntülerken disk belleği arabiriminde ilk ve önceki düğmelerini devre dışı bırakmanız gerekir. Bunu gerçekleştirmek için aşağıdaki kodu ekleyin ObjectDataSource s `Selecting` olay işleyicisi:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample17.vb)]

Bunlar ekledikten sonra `Click` olay işleyicileri ve kodu etkinleştirmek veya devre dışı geçerli başlangıç satır dizini, temel disk belleği arabirim öğeleri test sayfasını bir tarayıcıda. Şekil 15, ilk ilk sayfasını ziyaret gösterir ve önceki düğmeleri olacak devre dışı bırakılır. İleri'yi tıklatmadan gösterir, verilerin ikinci sayfasında son tıklatarak son sayfasında görüntülerken (rakamları 16 ve 17 bakın). Verilerin son sayfayı görüntülerken sonraki ve son düğmeleri devre dışı bırakılır.


[![Önceki ve son düğmeleri, ilk sayfa ürünleri görüntülerken devre dışı bırakıldı](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image42.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image41.png)

**Şekil 15**: önceki ve son düğmeleri devre dışı bırakıldığında, ilk sayfa ürünleri görüntülerken ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image43.png))


[![İkinci sayfa ürün Dispalyed değil](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image45.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image44.png)

**Şekil 16**: İkinci sayfa olan ürünleri olan Dispalyed ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image46.png))


[![Son görüntüler veri son sayfasında tıklatarak](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image48.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image47.png)

**Şekil 17**: son tıklatmak, son sayfasında veri görüntüler ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image49.png))


## <a name="step-7-including-sorting-support-with-the-custom-paged-repeater"></a>7. adım: Özel desteğiyle sıralama dahil olmak üzere yineleyici disk belleği

Özel sayfalama uygulanmıştır, sıralama dahil etmek için hazır re destekliyoruz. `ProductsBLL` s sınıfı `GetProductsPagedAndSorted` yöntemi sahip aynı *startRowIndex* ve *maximumRows* giriş parametreleri olarak `GetProductsPaged`, ancak bir ek verir  *sortExpression* giriş parametresi. Kullanılacak `GetProductsPagedAndSorted` yönteminden `SortingWithCustomPaging.aspx`, aşağıdaki adımları gerçekleştirmeniz gerekir:

1. ObjectDataSource s değiştirme `SelectMethod` özelliğinden `GetProductsPaged` için `GetProductsPagedAndSorted`.
2. Ekleme bir *sortExpression* `Parameter` ObjectDataSource s nesnesine `SelectParameters` koleksiyonu.
3. Özel, sayfa düzeyinde oluşturmak `SortExpression` değerini Geri göndermeler sayfa s görünüm durumu aracılığıyla boyunca devam ederse özelliği.
4. ObjectDataSource s güncelleştirme `Selecting` ObjectDataSource s atamak için olay işleyicisini *sortExpression* parametresi sayfa düzeyi değeri `SortExpression` özelliği.
5. Sıralama arabirimi oluşturun.

Başlangıç ObjectDataSource s güncelleştirerek `SelectMethod` özelliği ve ekleyerek bir *sortExpression* `Parameter`. Olduğundan emin olun *sortExpression* `Parameter` s `Type` özelliği ayarlanmış `String`. Bu ilk iki görevleri tamamladıktan sonra ObjectDataSource s bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample18.aspx)]

Ardından, sayfa düzeyinde ihtiyacımız `SortExpression` özellik değeri serileştirilmiş durumu görüntülemek için. Herhangi bir sıralama ifadesi değer ayarlarsanız ProductName varsayılan olarak kullanın:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample19.vb)]

ObjectDataSource çağırır önce `GetProductsPagedAndSorted` ayarlamak için ihtiyacımız yöntemi *sortExpression* `Parameter` değerine `SortExpression` özelliği. İçinde `Selecting` olay işleyicisi, aşağıdaki kod satırını ekleyin:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample20.vb)]

Kalan tek şey sıralama arabirimi uygulamak için. Son örnekte yaptığımız gibi sonuçları sıralamak kullanıcı izin üç düğme Web denetimleri kullanılarak ürün adı, kategori veya sağlayıcı tarafından uygulanan sıralama arabirimine sahip s olanak tanır.


[!code-aspx[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample21.aspx)]

Oluşturma `Click` bu üç düğme denetimleri için olay işleyicileri. Olay işleyicisi sıfırlama `StartRowIndex` 0 olarak ayarlayın `SortExpression` uygun değere ve yineleyici verileri yeniden bağlayın:


[!code-vb[Main](sorting-data-in-a-datalist-or-repeater-control-vb/samples/sample22.vb)]

Bu s tüm var olan üzere! Çeşitli özel disk belleği ve uygulanan sıralama almak için adımları kopyalanırken adımları varsayılan disk belleği için gerekenler için çok benzer. Şekil 18 kategoriye göre sıralanmış verilerinin son sayfayı görüntülerken ürünlerini gösterir.


[![Son sayfa verilerini, kategorilere göre Sorted görüntülenir](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image51.png)](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image50.png)

**Şekil 18**: son sayfası, verilerini, kategorilere göre Sorted görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-data-in-a-datalist-or-repeater-control-vb/_static/image52.png))


> [!NOTE]
> Üretici sıralama ifadesi olarak kullanılan sağlayıcı tarafından sıralarken önceki örneklerde. Ancak, özel disk belleği uygulamasını, biz ŞirketAdı kullanmanız gerekir. Bunun nedeni, özel sayfalama uygulamak için sorumlu saklı yordam `GetProductsPagedAndSorted` sıralama ifadesine geçirir `ROW_NUMBER()` anahtar sözcüğü, `ROW_NUMBER()` anahtar sözcüğü bir diğer ad yerine gerçek sütun adı gerektirir. Bu nedenle, biz kullanmalısınız `CompanyName` (sütununun adı `Suppliers` tablo) içinde kullanılan diğer adların yerine `SELECT` sorgu (`SupplierName`) sıralama ifadesi.


## <a name="summary"></a>Özet

Ne DataList ya da yineleyici yerleşik sıralama destek sunar, ancak bu tür işlevselliği ile bir bit kod ve özel bir sıralama arabirim eklenebilir. Sıralama, ancak disk belleği olmayan uygulama, sıralama ifadesi aracılığıyla belirtilebilir `DataSourceSelectArguments` nesnesi geçirildi ObjectDataSource s `Select` yöntemi. Bu `DataSourceSelectArguments` s nesnesi `SortExpression` özelliği ObjectDataSource s atanabilir `Selecting` olay işleyicisi.

Bir DataList veya zaten sayfalama desteği sağlayan yineleyici sıralama özellikleri eklemek için kolay bir sıralama ifadesi kabul eden bir yöntem eklemek için iş mantığı katmanı özelleştirmek için bir yaklaşımdır. Bu bilgiler daha sonra ObjectDataSource s içindeki bir parametre üzerinden geçirilebilir `SelectParameters`.

Bu öğretici, disk belleği ve DataList ve yineleyici denetimleriyle sıralama bizim İnceleme tamamlar. Sonraki ve son öğreticimizi düğmesi Web denetimlerin DataList ve yineleyici s şablonları için bir öğe başına temelinde bazı özel, kullanıcı tarafından başlatılan işlevleri sağlamak için nasıl ekleneceğini inceleyeceksiniz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme David Suru oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](paging-report-data-in-a-datalist-or-repeater-control-vb.md)
