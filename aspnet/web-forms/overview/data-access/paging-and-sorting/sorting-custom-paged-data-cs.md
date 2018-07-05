---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: Özel Sayfalanmış verileri (C#) | Microsoft Docs
author: rick-anderson
description: Önceki öğreticide size özel sayfalama verileri bir web sayfasında presentating olduğunda uygulama öğrendiniz. Bu öğreticide önceki genişletme görüyoruz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 36080799c94b359a7e5b7ba6ccb47e359fc2161d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362803"
---
<a name="sorting-custom-paged-data-c"></a>Özel Sayfalanmış verileri sıralama (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe) veya [PDF olarak indirin](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> Önceki öğreticide size özel sayfalama verileri bir web sayfasında presentating olduğunda uygulama öğrendiniz. Bu öğreticide özel disk belleği sıralamak için destek eklemek için önceki örneği genişletmek nasıl görüyoruz.


## <a name="introduction"></a>Giriş

Varsayılan sayfalama için karşılaştırıldığında özel disk belleği, birkaç büyük miktarlarda veri sayfalama pratikte sayfalama uygulama seçimi sayfalama özel yapma kat tarafından veri sayfalama performansını geliştirebilir. Özel disk belleği uygulama, uygulama varsayılan, ancak özellikle Karışıma sıralama eklerken, disk belleği değerinden daha karmaşıktır. Bu öğreticide size sıralamak için destek eklemek için önceki bir örneği genişletmek *ve* özel disk belleği.

> [!NOTE]
> Başına geçmeden önce bir süre içinde bildirim temelli söz dizimi kopyalamak için bu öğreticinin önceki bir oluşturur. bu yana `<asp:Content>` öğesi önceki öğretici s web sayfasından (`EfficientPaging.aspx`) arasında kopyalayıp yapıştırın `<asp:Content>` öğesinde`SortParameter.aspx` sayfası. Geri adım 1'ine bakın [arabirimleri ekleme ve düzenleme için doğrulama denetimleri ekleme](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) işlevlerini bir ASP.NET sayfasının diğerine çoğaltma üzerinde daha ayrıntılı bir açıklaması için öğretici.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>1. adım: özel bir disk belleği teknik yeniden inceleme

Düzgün çalışması özel disk belleği için belirtilen satır dizini başlatmak ve en fazla satır parametrelerine kayıtları belirli bir alt verimli bir şekilde tutabileceğinizi bazı teknik kodumuza. Bu bir yandan elde etmek için kullanılan teknikleri birkaç vardır. Bunu gerçekleştirmenin en incelemiştik önceki öğreticide Microsoft SQL Server 2005 s kullanarak yeni `ROW_NUMBER()` işlevi sıralaması. Kısacası, `ROW_NUMBER()` işlevi sıralaması atar bir satır numarası tarafından belirtilen bir sıralama sıralanmış bir sorgu tarafından döndürülen her satır için. Kayıtların uygun bir alt numaralı sonuçları belirli bir bölümünü döndürerek elde edilir. Aşağıdaki sorgu sonuçları sıralamasına göre alfabetik olarak sıralanmış olduğunda 11 ile 20 numaralı bu ürünleri döndürmek için bu tekniği kullanması gösterilmektedir `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

Bu teknik de belirli bir sıralama kullanarak sayfalama çalışır (`ProductName` alfabetik olarak sıralanmış bu durumda), ancak sorguyu bir farklı bir sıralama ifadesi tarafından sıralanmış sonuçları gösterecek şekilde değiştirilmesi gerekir. İdeal olarak, yukarıdaki sorguda bir parametre içinde kullanmak için yazılması `OVER` yan tümcesi şu şekilde:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

Ne yazık ki, parametreli `ORDER BY` yan tümceleri izin verilmez. Bunun yerine, biz kabul eden bir saklı yordam oluşturmalısınız bir `@sortExpression` giriş parametresi, ancak aşağıdaki geçici çözümlerden birini kullanır:

- Sabit kodlanmış sorguları her kullanılabilir sıralama ifadeleri için yazabilirsiniz. Ardından, `IF/ELSE` T-SQL deyimleri yürütmek için hangi sorgu belirlemek için.
- Kullanım bir `CASE` dinamik sağlamak için deyimi `ORDER BY` ifadeleri temelinde `@sortExpressio` n giriş parametresi; kullanılan dinamik olarak sorgu sonuçlarını sıralama bölümüne bakın [SQL Power `CASE` deyimleri](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Daha fazla bilgi için.
- Saklı yordamı bir dize olarak uygun sorgu oluşturabilir ve ardından [ `sp_executesql` sistem saklı yordamını](https://msdn.microsoft.com/library/ms188001.aspx) dinamik sorguyu yürütmek için.

Bu çözümler bazı dezavantajları vardır. İlk seçenek, her olası bir sıralama ifadesi için sorgu oluşturmanızı gerektirir ve diğer iki olarak sürdürülebilir değil. GridView'a yeni, sıralanabilir alanları eklemek daha sonra karar verirseniz bu nedenle, ayrıca geri dönün ve saklı yordam güncelleştirme gerekir. İkinci yaklaşım, dize olmayan veritabanı sütuna göre sıralama sırasında performans endişelerini tanıtır ve aynı bakım sorunlardan ilk olarak da alternatife bazı ıot'nin sahiptir. Ve dinamik SQL kullanır, üçüncü seçenek, saldırgan PKI'nizin giriş parametre değerlerini geçirme saklı yordamı yürütmek erişebiliyorsa, SQL ekleme saldırısına için riskini beraberinde getirir.

Bu yaklaşımların hiçbiri mükemmel olmakla birlikte, üç en iyi şekilde üçüncü bir seçenek olduğunu düşünüyorum. İle kullanımı dinamik SQL, diğer iki sağlamadığı esneklik düzeyi sunar. Ayrıca, SQL ekleme saldırısına yalnızca bir saldırgan kendi tercih ettiğiniz giriş parametrelerini geçirme saklı yordamı yürütmek erişebiliyorsa yararlanılabilir. DAL parametreli sorgular kullandığından, ADO.NET mimarisi aracılığıyla veritabanına gönderilen bu parametreleri saldırgan doğrudan saklı yordamı yürütebilir, SQL ekleme saldırısına açığı yalnızca bulunduğu anlamına gelir korur.

Bu işlevselliği uygulamak için Northwind veritabanında adlı yeni bir saklı yordam oluşturma `GetProductsPagedAndSorted`. Bu saklı yordamı üç giriş parametreleri kabul etmeniz gerekir: `@sortExpression`, türünde bir giriş parametresi `nvarchar(100`) nasıl sonuçları sıralanmalıdır ve hemen sonra eklenen belirten `ORDER BY` metinde `OVER` yan tümcesi; ve `@startRowIndex` ve `@maximumRows`, aynı iki tamsayı giriş parametrelerini `GetProductsPaged` saklı yordam önceki öğreticide incelenir. Oluşturma `GetProductsPagedAndSorted` saklı yordamını kullanarak aşağıdaki komut dosyası:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

Saklı yordamı için bir değer, sağlayarak başlar `@sortExpression` parametresi belirtilmedi. Eksikse, sonuçları tarafından sıralanır `ProductID`. Ardından, dinamik SQL sorgusu oluşturulur. Burada dinamik SQL sorgu biraz ürünlerin tablosundan tüm satırları almak için kullanılan bizim önceki sorguları farklı olduğuna dikkat edin. Önceki örneklerde, biz bir alt sorgu kullanarak s ve tedarikçi s adları her ürün s ilişkili kategorisi alınır. Bu kararı geri alınmıştır [veri erişim katmanını oluşturma](../introduction/creating-a-data-access-layer-cs.md) öğretici ve kullanarak yerine yapıldığı `JOIN` s TableAdapter bağdaştırıcısının ilişkili ekleme, otomatik olarak oluşturulamıyor çünkü güncelleştirme ve yöntemleri, örneğin silme sorgular. `GetProductsPagedAndSorted` Saklı yordam, ancak kullanmalıdır `JOIN` s sonuçlarının kategori veya tedarikçi ada göre sıralanmalıdır.

Bu dinamik sorgu statik sorgu kısımlarını birleştirerek şekilde oluşturulduğunu ve `@sortExpression`, `@startRowIndex`, ve `@maximumRows` parametreleri. Bu yana `@startRowIndex` ve `@maximumRows` tamsayı Parametreler, bunlar nvarchars doğru şekilde birleştirilmesine için dönüştürülmesi gerekir. Bu dinamik SQL sorgu oluşturulmuş sonra yoluyla yürütülür `sp_executesql`.

Bu saklı yordam için farklı değerlerle test etmek için birkaç dakikanızı `@sortExpression`, `@startRowIndex`, ve `@maximumRows` parametreleri. Sunucu Gezgini'nden saklı yordam adına sağ tıklayın ve Çalıştır'ı seçin. Bu saklı yordam Çalıştır iletişim kutusu (bkz. Şekil 1) giriş parametreleri girebileceğiniz çıkarır. Kategori adına göre sonuçları sıralamak için CategoryName için kullanın. `@sortExpression` parametre değeri; tedarikçi s şirket adına göre sıralamak için şirket adı kullanın. Parametre değerleri girdikten sonra Tamam'a tıklayın. Sonuçlar, çıkış penceresinde görüntülenir. Şekil 2 ürünleri döndüren 11 ile 20 tarafından sıralanırken sıralanmış, sonuçları gösterir `UnitPrice` azalan sırayla düzenleyin.


![Saklı yordam s üç giriş parametreleri için farklı değerler deneyin](sorting-custom-paged-data-cs/_static/image1.png)

**Şekil 1**: s üç saklı yordam giriş parametreleri için farklı değerler deneyin


[![Saklı yordam s çıkış penceresinde sonuç gösterilmiyor](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**Şekil 2**: saklı yordam s sonuçları çıkış penceresinde gösterilir ([tam boyutlu görüntüyü görmek için tıklatın](sorting-custom-paged-data-cs/_static/image4.png))


> [!NOTE]
> Sonuçları tarafından belirtilen sıralama sırasında `ORDER BY` sütununda `OVER` yan tümcesi, SQL Server sonuçlarını sıralama gerekir. Bu sonuçları sipariş edilen tarafından sütunların üzerinden kümelenmiş bir dizin ise hızlı bir işlemdir veya bir kapsayıcı olup olmadığını dizin, ancak Aksi halde daha yüksek maliyetli olabilir. Yeterince büyük sorgular için performansı artırmak için kümelenmemiş bir dizin olarak sonuçları göre sıralanmış sütun ekleme göz önünde bulundurun. Başvurmak [sıralama işlevlerini ve SQL Server 2005'te performans](http://www.sql-server-performance.com/ak_ranking_functions.asp) daha fazla ayrıntı için.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>2. adım: iş mantığı katmanı ve veri erişimi deneyimlerinizi

İle `GetProductsPagedAndSorted` oluşturulan saklı yordam sonraki adımımız etmektir uygulama mimarimiz aracılığıyla bu saklı yordamı yürütmek için bir yol sağlar. Bu DAL ve BLL için uygun bir yöntem eklemeyi gerektirir. DAL için bir yöntem ekleyerek başlayın s olanak tanır. Açık `Northwind.xsd` türü belirtilmiş veri kümesi, sağ `ProductsTableAdapter`ve bağlam menüsünden Sorgu Ekle seçeneğini seçin. Önceki öğreticide yaptığımız gibi biz varolan bir saklı yordam - kullanmak için bu yeni DAL yöntemi yapılandırmak istediğiniz `GetProductsPagedAndSorted`, bu durumda. Varolan bir saklı yordam kullanmak için yeni TableAdapter metodu istediğinizi belirterek başlayın.


![Varolan bir saklı yordam kullanmak için seçin](sorting-custom-paged-data-cs/_static/image5.png)

**Şekil 3**: varolan bir saklı yordam kullanmak için seçin


Saklı yordamın kullanılacağını belirtmek için seçin `GetProductsPagedAndSorted` saklı yordamı aşağı açılan listeden sonraki ekranda.


![GetProductsPagedAndSorted kullanın depolanan yordamı](sorting-custom-paged-data-cs/_static/image6.png)

**Şekil 4**: GetProductsPagedAndSorted kullanın depolanan yordamı


Sonraki ekranda, bu nedenle, belirtmek tablo verisi döndüren sonuçlarını olarak bu saklı yordamı bir kayıt kümesini döndürür.


![Saklı yordam tablo verisi döndüren belirtin](sorting-custom-paged-data-cs/_static/image7.png)

**Şekil 5**: saklı yordam tablo verisi döndüren belirtin


Son olarak, her iki dolgu kullanan DAL yöntemleri DataTable oluşturma ve adlandırma yöntemleri bir DataTable desenleri döndürür `FillPagedAndSorted` ve `GetProductsPagedAndSorted`sırasıyla.


![Yöntem adları seçin](sorting-custom-paged-data-cs/_static/image8.png)

**Şekil 6**: yöntem adları seçin


Artık görüyoruz ve genişletilmiş DAL biz yeniden etkinleştirmek için BLL için hazır. Açık `ProductsBLL` sınıf dosyası ve bir yeni yöntem ekleyin `GetProductsPagedAndSorted`. Bu yöntem, üç giriş parametrelerini kabul eden gerekiyor `sortExpression`, `startRowIndex`, ve `maximumRows` ve DAL s ile yalnızca çağırmalıdır `GetProductsPagedAndSorted` yöntemi şu şekilde:


[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>3. adım: geçişine ObjectDataSource SortExpression parametreyi yapılandırma

BLL ve DAL kullanan yöntemleri içerecek şekilde Genişletilmiş `GetProductsPagedAndSorted` ObjectDataSource olarak yapılandırmak için tüm kalan saklı yordamı, olan `SortParameter.aspx` sayfası yeni BLL yöntemi kullanın ve geçirin `SortExpression` parametresini temel alan sonuçları sıralamak için kullanıcı tarafından istenen sütun.

ObjectDataSource s değiştirerek başlayın `SelectMethod` gelen `GetProductsPaged` için `GetProductsPagedAndSorted`. Bu veri kaynağı Yapılandırma Sihirbazı Özellikler penceresinden veya doğrudan bildirim temelli söz dizimi aracılığıyla yapılabilir. Ardından, ObjectDataSource s için bir değer sağlamak için ihtiyacımız [ `SortParameterName` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). ObjectDataSource GridView s'te geçirmek bu özelliği ayarlarsanız çalışır `SortExpression` özelliğini `SelectMethod`. Özellikle, bir giriş parametresi adı değerine eşit olan ObjectDataSource arar `SortParameterName` özelliği. BLL s beri `GetProductsPagedAndSorted` yöntemi adlı sıralama ifadesi giriş parametresi vardır `sortExpression`, ObjectDataSource s ayarlamak `SortExpression` sortExpression özelliği.

Bu iki değişikliği yaptıktan sonra ObjectDataSource s bildirim temelli söz dizimi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> Önceki öğreticide ObjectDataSource yaptığından emin olmak gibi *değil* sortExpression, startRowIndex veya maximumRows giriş parametrelerini kendi SelectParameters koleksiyona dahil.


İçinde GridView sıralamayı etkinleştirmek için basitçe GridView s ayarlar GridView s akıllı etiketinde sıralamayı etkinleştir onay kutusunu işaretleyin `AllowSorting` özelliğini `true` ve bir LinkButton işlenmek üzere her bir sütunun üst bilgi metni neden. Son kullanıcı bir üstbilgi LinkButtons tıkladığında, bir geri gönderme ensues ve aşağıdaki adımları niteler:

1. GridView güncelleştirmeleri onun [ `SortExpression` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) değerine `SortExpression` üstbilgi bağlantısını tıklattığınız alan
2. ObjectDataSource BLL s çağırır `GetProductsPagedAndSorted` yöntemini GridView s'te `SortExpression` özellik değeri ' % s'metodu s olarak `sortExpression` giriş parametresi (uygun birlikte `startRowIndex` ve `maximumRows` giriş parametresi değerleri)
3. DAL s BLL çağırır `GetProductsPagedAndSorted` yöntemi
4. DAL yürütür `GetProductsPagedAndSorted` içinde geçirme saklı yordamı, `@sortExpression` parametre (ile birlikte `@startRowIndex` ve `@maximumRows` giriş parametresi değerleri)
5. Saklı yordam için ObjectDataSource döndürür BLL uygun veri alt kümesini döndürür; Bu veriler daha sonra GridView'a bağlı HTML'e işlenen ve son kullanıcıya gönderilen

Şekil 7 göre sıralanmış sonuçları ilk sayfasını gösterir `UnitPrice` artan düzende.


[![Sonuçlar UnitPrice göre sıralanır.](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**Şekil 7**: sonuçları UnitPrice göre sıralanır ([tam boyutlu görüntüyü görmek için tıklatın](sorting-custom-paged-data-cs/_static/image11.png))


Geçerli uygulama doğru ürün adı, kategori adı, birim ve birim fiyatı başına miktarını sonuçlar sıralayabilirsiniz, ancak sonuçları sağlayıcısına göre adı sonuçları bir çalışma zamanı özel durum sipariş çalışılıyor (bkz. Şekil 8).


![Sonuçları tedarikçi sonuçları aşağıdaki çalışma zamanı özel durumuna göre sıralamak çalışılıyor](sorting-custom-paged-data-cs/_static/image12.png)

**Şekil 8**: sonuçlarına göre çalışma zamanı özel durumu tedarikçi sonuçları sıralama çalışılıyor


Bu özel durum oluşur `SortExpression` GridView s `SupplierName` BoundField ayarlandığında `SupplierName`. Ancak, s sağlayıcı adı `Suppliers` Tablo adında gerçekten `CompanyName` diğer adlı olarak bu sütun adı olan `SupplierName`. Ancak, `OVER` yan tümcesi tarafından kullanılan `ROW_NUMBER()` işlev diğer adı kullanılamıyor ve gerçek sütun adı kullanmanız gerekir. Bu nedenle, değiştirme `SupplierName` BoundField s `SortExpression` üretici CompanyName için gelen (bkz. Şekil 9). Şekil 10 gösterildiği gibi bu değişiklikten sonra sonuçları sağlayıcı tarafından sıralanabilir.


![Üretici BoundField s SortExpression'birlikte CompanyName olarak değiştirin.](sorting-custom-paged-data-cs/_static/image13.png)

**Şekil 9**: üretici BoundField s SortExpression'birlikte CompanyName olarak değiştirin.


[![Sonuçları artık sağlayıcı tarafından sıralanabilir](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**Şekil 10**: sonuçları artık sıralanıp sıralanamayacağını sağlayıcı tarafından ([tam boyutlu görüntüyü görmek için tıklatın](sorting-custom-paged-data-cs/_static/image16.png))


## <a name="summary"></a>Özet

Biz önceki öğreticide incelenirken özel disk belleği uygulaması, sonuçları tarafından sıralanacak olan sırasını tasarım zamanında belirtilmesi gerekir. Kısacası, uyguladık özel sayfalama uygulama aynı zamanda, sıralama yetenekleri sağladığı değil, bu geliyordu. Bu öğreticide size bu sınırlamayı içerecek şekilde birinciden saklı yordamı genişleterek overcame bir `@sortExpression` giriş parametresi olarak sonuçlar sıralanıp sıralanamayacağını.

Bu durum saklı yordam ve yeni yöntemler BLL ve DAL oluşturma sonra hem bir sıralama sunulan GridView uygulamak için ve özel GridView s'te geçerli geçirilecek ObjectDataSource yapılandırarak sayfalama biz `SortExpression` BLL özelliği `SelectMethod`.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Carlos Santos oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](efficiently-paging-through-large-amounts-of-data-cs.md)
> [İleri](creating-a-customized-sorting-user-interface-cs.md)
