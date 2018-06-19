---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: Özel sıralama disk belleğine alınan verileri (C#) | Microsoft Docs
author: rick-anderson
description: Önceki öğreticide nasıl verileri bir web sayfasında presentating olduğunda özel sayfalama uygulanacağı öğrendiniz. Bu öğreticide önceki genişletme bakın …
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: c30f13703ef20cd764785b00cd812ef4486e0f16
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882422"
---
<a name="sorting-custom-paged-data-c"></a>Özel sıralama havuzda verileri (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe) veya [PDF indirin](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> Önceki öğreticide nasıl verileri bir web sayfasında presentating olduğunda özel sayfalama uygulanacağı öğrendiniz. Bu öğreticide özel sayfalama sıralamak için destek eklemek için önceki örnekte genişletme bakın.


## <a name="introduction"></a>Giriş

Varsayılan disk belleği için karşılaştırıldığında özel sayfalama birkaç gerçek disk belleği uygulama seçimi büyük miktarlarda verinin sayfalama disk belleği özel yapma büyüklük tarafından veri sayfalama performansını geliştirebilir. Özel sayfalama uygulama, varsayılan, ancak özellikle Karışıma sıralama eklerken, disk belleği uygulama değerinden daha karmaşıktır. Bu öğreticide biz sıralamak için destek eklemek için önceki bir örneği genişletmek *ve* özel sayfalama.

> [!NOTE]
> Başlangıç bildirim temelli söz dizimi içinde kopyalamak için bir dakikanızı olabilmesi Bu öğreticinin önceki bir derlemeler beri `<asp:Content>` önceki öğretici s web sayfası öğesinden (`EfficientPaging.aspx`) arasında yapıştırın `<asp:Content>` öğesinde`SortParameter.aspx` sayfası. Adım 1'ine geri başvuran [doğrulama denetimleri ekleme düzenleme ve ekleme arabirimleri için](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) öğretici bir ASP.NET sayfası işlevselliğini diğerine çoğaltma hakkında daha ayrıntılı bilgi için.


## <a name="step-1-reexamining-the-custom-paging-technique"></a>1. adım: özel bir disk belleği teknik yeniden inceleme

Özel disk belleği düzgün çalışması biz verimli bir şekilde belirli bir alt kümesini Başlat satır dizini ve en fazla satır parametreleri verilen kayıtları alın bazı teknik uygulamalıdır. Bu amacı elde etmek için kullanılan teknikleri sayıda vardır. Aranan Bunu gerçekleştirmenin en önceki öğreticideki Microsoft SQL Server 2005 s kullanarak yeni `ROW_NUMBER()` işlevi sıralaması. Kısacası, `ROW_NUMBER()` işlevi sıralaması atar bir satır numarası tarafından belirtilen sıralama düzeni derece bir sorgu tarafından döndürülen her satır için. Kayıtların uygun bir alt numaralı sonuçları belirli bir bölüme döndürerek elde edilir. Aşağıdaki sorgu sonuçlarını sıralama göre alfabetik olarak sıralanan zaman 20 11 numaralı bu ürünlerden döndürmek için bu tekniği kullanmak verilmektedir `ProductName`:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

Bu teknik de belirli bir sıralama kullanarak sayfalandırma için çalışır (`ProductName` , bu durumda alfabetik), ancak sorgu farklı bir sıralama ifadesi tarafından sıralanan sonuçları gösterecek şekilde değiştirilmesi gerekir. İdeal olarak, yukarıdaki sorguda bir parametre kullanmak için yazılması `OVER` yan tümcesi, şu şekilde:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

Ne yazık ki, parametreli `ORDER BY` yan tümcelerinde izin verilmez. Bunun yerine, biz kabul eden bir saklı yordam oluşturmalısınız bir `@sortExpression` giriş parametresi, ancak aşağıdaki geçici çözümlerden birini birini kullanır:

- Sabit kodlanmış sorguları her kullanılabilir sıralama ifadeleri yazmak; Ardından, `IF/ELSE` T-SQL deyimlerini yürütmek için hangi sorgu belirlemek için.
- Kullanım bir `CASE` dinamik sağlamak için deyimi `ORDER BY` ifadeler temelinde `@sortExpressio` n giriş parametresi; kullanılan dinamik olarak sorgu sonuçlarını sıralama bölümüne bakın [SQL güç `CASE` deyimleri](http://www.4guysfromrolla.com/webtech/102704-1.shtml) Daha fazla bilgi için.
- Saklı yordam içinde bir dize olarak uygun sorgusu oluşturabilir ve ardından [ `sp_executesql` sistem saklı yordamı](https://msdn.microsoft.com/library/ms188001.aspx) dinamik sorgusu yürütülemedi.

Her Bu çözümler, bazı sakıncaları vardır. İlk seçenek, her olası sıralama ifadesi için bir sorgu oluşturun gerektirdiğinden diğer iki olarak korunabilir değil. Daha sonra GridView yeni, sıralanabilir alanları eklemeye karar verirseniz bu nedenle, aynı zamanda geri dönün ve saklı yordam güncelleştirmek gerekir. İkinci bir yaklaşım, dize olmayan veritabanı sütuna göre sıralama performans sorunları getirir ve aynı bakım sorunlardan ilk olarak da yükselmesine bazı subtleties vardır. Ve bir saldırganın istediği giriş parametresi değerleri geçirme saklı yordamı yürütme mümkün ise dinamik SQL kullanır, üçüncü seçenek için bir SQL ekleme saldırısı riskini beraberinde getirir.

Bu yaklaşım hiçbiri kusursuz olmakla birlikte, en iyi üç üçüncü seçenek olan düşünüyorum. Dinamik SQL kullanımı ile diğer iki sağlamadığı esneklik düzeyi sunar. Ayrıca, bir saldırganın kendi seçtikleri giriş parametrelerini geçirme saklı yordamı yürütme mümkün ise bir SQL ekleme saldırısı yalnızca yararlanılabilir. DAL parametreli sorgular kullandığından, ADO.NET saldırgan doğrudan saklı yordamını yürütebilir SQL ekleme saldırısı güvenlik açığı yalnızca varsa, yani Mimarisi yoluyla veritabanına gönderilen bu parametreleri korur.

Bu işlevselliği uygulamak için Northwind veritabanına adlı yeni bir saklı yordam oluşturmak `GetProductsPagedAndSorted`. Bu saklı yordam üç giriş parametreleri kabul etmeniz gerekir: `@sortExpression`, türünde bir giriş parametresi `nvarchar(100`) nasıl sonuçları sıralanması gerektiğini ve hemen sonra eklenen belirleyen `ORDER BY` metinde `OVER` yan tümcesi; ve `@startRowIndex` ve `@maximumRows`, aynı iki tamsayı giriş parametrelerini `GetProductsPaged` saklı yordamı önceki öğreticide inceledi. Oluşturma `GetProductsPagedAndSorted` saklı yordamı aşağıdaki komut dosyası kullanarak:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

Saklı yordam için bir değer, sağlayarak başlatır `@sortExpression` parametresi belirtilmedi. Eksik ise, sonuçları tarafından sıralanır `ProductID`. Ardından, dinamik SQL sorgusu oluşturulur. Burada dinamik SQL sorgusu biraz ürünleri tablodan tüm satırları almak için kullanılan bizim önceki sorgular farklı olduğunu unutmayın. Önceki örneklerde, biz her ürün s ilişkili kategorisi s ve tedarikçi s adlarına bir alt sorgu kullanılarak elde edilen. Bu kararı geri alınmıştır [veri erişim katmanı oluşturma](../introduction/creating-a-data-access-layer-cs.md) öğretici ve kullanarak yerine yapıldığı `JOIN` s TableAdapter otomatik olarak ilişkili INSERT oluşturamazsınız olduğundan güncelleştirme ve yöntemleri gibi için silme sorgular. `GetProductsPagedAndSorted` Saklı yordamı, ancak kullanmalıdır `JOIN` s sonuçlar için kategori veya tedarikçi adları sıralanmalıdır.

Bu dinamik sorgu statik sorgu bölümleri birleştirerek şekilde oluşturulduğunu ve `@sortExpression`, `@startRowIndex`, ve `@maximumRows` parametreleri. Bu yana `@startRowIndex` ve `@maximumRows` tamsayı parametreleri, bunların nvarchars doğru birleştirilmiş için dönüştürülmesi gerekir. Bu dinamik bir SQL sorgusu oluşturulan sonra yoluyla yürütülür `sp_executesql`.

Bu saklı yordam için farklı değerler ile test etmek için bir dakikanızı ayırın `@sortExpression`, `@startRowIndex`, ve `@maximumRows` parametreleri. Sunucu Gezgini'nde, saklı yordamın adını sağ tıklatın ve Çalıştır'ı seçin. Bu saklı yordam Çalıştır iletişim kutusunu (bkz: Şekil 1) giriş parametreleri girebileceği çıkarır. Sonuçları kategori ada göre sıralamak için CategoryName için kullanmak `@sortExpression` parametre değeri; tedarikçi s şirket adına göre sıralamak için şirket adı kullanın. Parametre değerleri girdikten sonra Tamam'a tıklayın. Sonuçlar çıktı penceresinde görüntülenir. Şekil 2 ürünleri döndürme 20 11 tarafından sıralanırken derece zaman sonuçları gösterir `UnitPrice` azalan sırayla düzenleyin.


![Saklı yordam s üç giriş parametreleri için farklı değerler deneyin](sorting-custom-paged-data-cs/_static/image1.png)

**Şekil 1**: s üç saklı yordam giriş parametreleri için farklı değerler deneyin


[![Saklı yordam s çıkış penceresinde sonuçlar gösterilir](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**Şekil 2**: saklı yordam s sonuçları çıktı penceresinde gösterilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-custom-paged-data-cs/_static/image4.png))


> [!NOTE]
> Sonuçları tarafından belirtilen sıralama zaman `ORDER BY` sütununda `OVER` yan tümcesi, SQL Server sonuçlarını sıralama gerekir. Bu sonuçları sipariş edilen tarafından sütunları üzerinden kümelenmiş bir dizin ise hızlı bir işlemdir veya bir kapsayıcı olup olmadığını dizin ancak Aksi halde daha pahalı olabilir. Yeterli büyüklükte sorgular için performansı artırmak için kümelenmemiş bir dizin olarak sonuçları göre sıralanmış sütun ekleme göz önünde bulundurun. Başvurmak [sıralaması işlevleri ve SQL Server 2005'te performans](http://www.sql-server-performance.com/ak_ranking_functions.asp) daha fazla ayrıntı için.


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>2. adım: veri erişimi ve iş mantığı katmanları program.cs'ye

İle `GetProductsPagedAndSorted` oluşturulan saklı yordam bizim sonraki adımdır bizim Uygulama Mimarisi yoluyla bu saklı yordamı yürütmek için bir yol sağlamak için. Bu DAL ve BLL için uygun bir yöntem ekleme kapsar. DAL için bir yöntem ekleyerek başlayın s olanak tanır. Açık `Northwind.xsd` yazılan veri kümesi, sağ tıklatın `ProductsTableAdapter`ve bağlam menüsünden Sorgu Ekle seçeneğini belirleyin. Önceki öğreticide yaptığımız gibi varolan bir saklı yordam - kullanmak için bu yeni DAL yöntemi yapılandırmak istiyoruz `GetProductsPagedAndSorted`, bu durumda. Varolan bir saklı yordamı kullanmak için yeni TableAdapter yöntemi istediğiniz belirterek başlatın.


![Varolan bir saklı yordamı kullanmak için seçin](sorting-custom-paged-data-cs/_static/image5.png)

**Şekil 3**: varolan bir saklı yordamı kullanmak için seçin


Kullanmak için saklı yordam belirtmek için işaretleyin `GetProductsPagedAndSorted` saklı yordamı aşağı açılan listeden sonraki ekranında.


![GetProductsPagedAndSorted kullanmak saklı yordamı](sorting-custom-paged-data-cs/_static/image6.png)

**Şekil 4**: GetProductsPagedAndSorted kullanmak saklı yordamı


Sonraki ekranda, bu nedenle, belirtmek tablo verisi döndüren sonuçlarını gibi bu saklı yordamı bir kayıt kümesini döndürür.


![Saklı yordam tablo verisi döndüren belirtin](sorting-custom-paged-data-cs/_static/image7.png)

**Şekil 5**: saklı yordamı tablo verisi döndüren belirtin


Son olarak, her iki dolgusu kullanma DAL yöntemleri DataTable oluşturup yöntemleri adlandırma bir DataTable desenleri dönmek `FillPagedAndSorted` ve `GetProductsPagedAndSorted`sırasıyla.


![Yöntem adları seçin](sorting-custom-paged-data-cs/_static/image8.png)

**Şekil 6**: yöntem adları seçin


Şimdi görüyoruz ve genişletilmiş DAL biz re BLL etkinleştirmek için hazır. Açık `ProductsBLL` sınıf dosya ve yeni bir yöntem ekleyin `GetProductsPagedAndSorted`. Bu yöntem üç giriş parametreleri kabul etmesi gerekir `sortExpression`, `startRowIndex`, ve `maximumRows` ve DAL s yalnızca çağırmalıdır `GetProductsPagedAndSorted` yöntemi, şu şekilde:


[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>3. adım: geçişine ObjectDataSource SortExpression parametresinde yapılandırma

DAL ve BLL kullanan yöntemleri eklemek için Genişletilebilir `GetProductsPagedAndSorted` tüm kalır saklı yordamı, olan ObjectDataSource yapılandırmak için `SortParameter.aspx` sayfası yeni BLL yöntemini kullanın ve geçirin `SortExpression` parametresi temel alarak kullanıcının sonuçlarına göre sıralamak için istediği sütun.

Başlangıç ObjectDataSource s değiştirerek `SelectMethod` gelen `GetProductsPaged` için `GetProductsPagedAndSorted`. Bu veri kaynağı Yapılandırma Sihirbazı, Özellikler penceresini veya doğrudan bildirim temelli söz dizimi aracılığıyla yapılabilir. Ardından, ObjectDataSource s'için bir değer sağlamak ihtiyacımız [ `SortParameterName` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx). Bu özellik ayarlanırsa, ObjectDataSource GridView s geçirmeye çalışır `SortExpression` özelliğine `SelectMethod`. Özellikle, adı değerine eşit olan bir giriş parametresi ObjectDataSource arar `SortParameterName` özelliği. BLL s itibaren `GetProductsPagedAndSorted` yöntemi adlı sıralama ifadesi giriş parametresi var `sortExpression`, ObjectDataSource s ayarlamak `SortExpression` sortExpression özelliğine.

Bu iki değişiklikleri yaptıktan sonra ObjectDataSource s Tanımlayıcı Sözdizimi aşağıdakine benzer görünmelidir:


[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> Önceki eğitici ObjectDataSource yaptığından emin olmak gibi *değil* kendi SelectParameters koleksiyonunda sortExpression, startRowIndex veya maximumRows giriş parametrelerini içerir.


GridView sıralamayı etkinleştirmek için GridView s ayarlar GridView s akıllı etiket sıralama etkinleştir onay kutusunu işaretlemeniz yeterli `AllowSorting` özelliğine `true` ve LinkButton işlenmek üzere her sütun için üstbilgi metni neden olur. Son kullanıcı LinkButtons üstbilgi birinde tıkladığında, geri gönderimin ensues ve aşağıdaki adımları gerçekleşen:

1. GridView güncelleştirmeleri kendi [ `SortExpression` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) değerine `SortExpression` üstbilgi bağlantısını tıklattınız alanının
2. ObjectDataSource BLL s çağırır `GetProductsPagedAndSorted` GridView s geçirerek yöntemini `SortExpression` özellik değeri s yöntemi olarak `sortExpression` giriş parametresi (uygun birlikte `startRowIndex` ve `maximumRows` giriş parametre değerleri)
3. DAL s BLL çağırır `GetProductsPagedAndSorted` yöntemi
4. DAL yürütür `GetProductsPagedAndSorted` içinde geçirme saklı yordamı, `@sortExpression` parametre (ile birlikte `@startRowIndex` ve `@maximumRows` giriş parametre değerleri)
5. Saklı yordam ObjectDataSource döndürür BLL uygun veri alt kümesini döndürür; Bu veriler sonra GridView bağlı HTML'e işlenmiş ve son kullanıcıya gönderilen

Şekil 7 gösterir tarafından sıralandığında sonuçları'nın ilk sayfasında `UnitPrice` artan.


[![Sonuçlar UnitPrice göre sıralanır.](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**Şekil 7**: sonuçları UnitPrice göre sıralanır ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-custom-paged-data-cs/_static/image11.png))


Bir çalışma zamanı özel ad sonuçları sonuçları sağlayıcısına göre sıralamak geçerli doğru sonuçları ürün adı, kategori adı, birim ve birim fiyat başına miktar göre sıralayabilirsiniz, ancak çalışırken (bkz. Şekil 8).


![Sonuçları şu çalışma zamanı özel tedarikçi sonuçlarında göre sıralamak çalışılıyor](sorting-custom-paged-data-cs/_static/image12.png)

**Şekil 8**: sonuçlarına şu çalışma zamanı özel tedarikçi sonuçlarında göre sıralama çalışılıyor


Bu özel durum oluşur `SortExpression` GridView s `SupplierName` BoundField ayarlanmış `SupplierName`. Ancak, s sağlayıcı adı `Suppliers` tablo gerçekten adlı `CompanyName` diğer adı bu sütun adı olarak kaldırılmış `SupplierName`. Ancak, `OVER` yan tümcesi tarafından kullanılan `ROW_NUMBER()` işlevi diğer ad kullanılamıyor ve gerçek sütun adı kullanmanız gerekir. Bu nedenle, değiştirme `SupplierName` BoundField s `SortExpression` ŞirketAdı üretici gelen (bkz. Şekil 9). Şekil 10 gösterildiği gibi bu değişiklikten sonra sonuçları sağlayıcı tarafından sıralanabilir.


![Üretici BoundField s SortExpression ŞirketAdı için değiştirme](sorting-custom-paged-data-cs/_static/image13.png)

**Şekil 9**: üretici BoundField s SortExpression ŞirketAdı için değiştirme


[![Sonuçları şimdi tedarikçi tarafından sıralanabilir](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**Şekil 10**: sonuçları şimdi sıralanıp sıralanamayacağını tedarikçi tarafından ([tam boyutlu görüntüyü görüntülemek için tıklatın](sorting-custom-paged-data-cs/_static/image16.png))


## <a name="summary"></a>Özet

Biz önceki öğreticide incelenmesi özel disk belleği uygulaması tarafından sonuçları sıralanacak bulundukları sırada tasarım zamanında belirtilmesi gerekir. Kısacası, bu biz uygulanan özel disk belleği uygulamasını aynı anda sıralama yeteneklerini sağlayamadı, olduğunu anlamına gelir. Bu öğreticide Biz bu sınırlamaya dahil etmek için ilk saklı yordamı genişleterek overcame bir `@sortExpression` giriş parametresi olarak sonuçlar sıralanır.

Her iki sıralama sunulan GridView uygulamak için ve özel GridView s geçerli geçirmek için ObjectDataSource yapılandırarak disk belleği bu oluşturma yordamı ve yeni yöntemleri DAL ve BLL oluşturma depolanan sonra bulamadığımız `SortExpression` BLL özelliği `SelectMethod`.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Carlos Santos oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](efficiently-paging-through-large-amounts-of-data-cs.md)
> [sonraki](creating-a-customized-sorting-user-interface-cs.md)
