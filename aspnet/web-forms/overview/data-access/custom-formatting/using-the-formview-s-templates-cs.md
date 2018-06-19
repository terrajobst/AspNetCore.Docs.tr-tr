---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
title: FormView'ın şablonları (C#) kullanarak | Microsoft Docs
author: rick-anderson
description: DetailsView FormView alanlarının oluşur değil. Bunun yerine, FormView şablonları kullanılarak işlenir. Bu öğreticide f kullanarak inceleyeceğiz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: d3f062af-88cf-426d-af44-e41f32c41672
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-cs
msc.type: authoredcontent
ms.openlocfilehash: 0e1b36f0bfc244e39bb620c1c066b3e2403722cb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875831"
---
<a name="using-the-formviews-templates-c"></a>FormView'ın şablonları (C#) kullanarak
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_14_CS.exe) veya [PDF indirin](using-the-formview-s-templates-cs/_static/datatutorial14cs1.pdf)

> DetailsView FormView alanlarının oluşur değil. Bunun yerine, FormView şablonları kullanılarak işlenir. Biz inceleyeceğiz Bu öğreticide FormView Denetim verilerin daha az katı bir görünümünü sunmak için kullanma.


## <a name="introduction"></a>Giriş

Son iki eğitimlerine TemplateFields kullanarak GridView ve DetailsView denetimleri çıkışları özelleştirmeyi gördük. Yüksek oranda özelleştirilmiş belirli bir alan için içerik TemplateFields izin ancak sonunda GridView ve DetailsView yerine boxy, kılavuz benzeri bir görünümü vardır. Bu tür bir kılavuz benzeri bir düzende birçok senaryoları için idealdir, ancak daha esnektir, daha az katı bir görüntü bazen gereklidir. Tek bir kaydını görüntülerken, akıcı bir düzen FormView denetimini kullanarak mümkündür.

DetailsView FormView alanlarının oluşur değil. FormView için BoundField veya TemplateField ekleyemezsiniz. Bunun yerine, FormView şablonları kullanılarak işlenir. FormView, tek bir TemplateField içeren bir DetailsView denetimi düşünün. FormView aşağıdaki şablonları destekler:

- `ItemTemplate` FormView görüntülenen belirli kayıt işlemek için kullanılan
- `HeaderTemplate` bir isteğe bağlı üstbilgi satırını belirtmek için kullanılır
- `FooterTemplate` bir isteğe bağlı altbilgi satırı belirtmek için kullanılır
- `EmptyDataTemplate` zaman FormView's `DataSource` herhangi bir kayıt eksik `EmptyDataTemplate` yerine kullanılan `ItemTemplate` denetimin biçimlendirme işlemek için
- `PagerTemplate` disk belleği etkin olan FormViews için disk belleği arabirimini özelleştirmek için kullanılabilir
- `EditItemTemplate` / `InsertItemTemplate` Bu tür işlevleri desteklemek için FormViews düzenleme arabirimi veya ekleme arabirimi özelleştirmek için kullanılır

Biz inceleyeceğiz Bu öğreticide FormView denetim ürünleri daha az katı bir görünümünü sunmak için kullanma. Ad, kategori, tedarikçi vb. FormView's için alanları sahip yerine `ItemTemplate` bir üstbilgi öğesi bir birleşimini kullanarak bu değerleri gösterir ve `<table>` (bkz: Şekil 1).


[![DetailsView'da görülen kılavuz benzeri düzeninin FormView kesilmeler](using-the-formview-s-templates-cs/_static/image2.png)](using-the-formview-s-templates-cs/_static/image1.png)

**Şekil 1**: FormView keser Grid-Like düzeni görülen dışında DetailsView'da ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-formview-s-templates-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-formview"></a>1. adım: FormView veri bağlama

Açık `FormView.aspx` sayfasında ve bir FormView tasarımcıya araç çubuğuna sürükleyin. İlk FormView eklerken bize söyleyen bir gri kutu olarak görünür, bir `ItemTemplate` gereklidir.


[![FormView tasarımcıda oluşturulamayan bir ItemTemplate sağlanmadıkça](using-the-formview-s-templates-cs/_static/image5.png)](using-the-formview-s-templates-cs/_static/image4.png)

**Şekil 2**: FormView olamaz çizilir Tasarımcısı kadar içinde bir `ItemTemplate` sağlanır ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-formview-s-templates-cs/_static/image6.png))


`ItemTemplate` (Bildirim temelli söz dizimi) el ile oluşturulabilir veya veri kaynağı denetimi Tasarımcısı aracılığıyla FormView bağlama tarafından otomatik oluşturulan olabilir. Bu otomatik oluşturulan `ItemTemplate` içeren listeleri her bir alan ve bir etiket adı, Denetim HTML `Text` özelliği alanın değerini bağlı. Bu yaklaşım da otomatik oluşturur-bir `InsertItemTemplate` ve `EditItemTemplate`, her veri kaynağı denetimi tarafından döndürülen veri alanlarının her ikisi de giriş denetimleri ile doldurulur.

Şablonu otomatik olarak oluşturmak istiyorsanız, FormView'ın akıllı etiketten çağıran bir yeni ObjectDataSource denetimine ekleme `ProductsBLL` sınıfının `GetProducts()` yöntemi. Bu FormView ile oluşturacak bir `ItemTemplate`, `InsertItemTemplate`, ve `EditItemTemplate`. Kaynak görünümden Kaldır `InsertItemTemplate` ve `EditItemTemplate` ilginizi değiliz bu yana bir FormView düzenleme veya henüz ekleme destekleyen oluşturma. Sonraki içinde Biçimlendirmeyi Temizle `ItemTemplate` böylece çalışabilmesi için temiz bir Kurşun sunuyoruz.

Bunun yerine oluşturmayı tercih ederseniz `ItemTemplate` el ile ekleyin ve araç tasarımcıya sürükleyerek ObjectDataSource yapılandırın. Ancak, FormView'ın veri kaynağı Tasarımcısı'ndan ayarlamanız gerekmez. Bunun yerine, kaynak görünümüne gidin ve el ile FormView's ayarlamak `DataSourceID` özelliğine `ID` ObjectDataSource değeri. Ardından, el ile eklemeniz `ItemTemplate`.

Hangi yaklaşımın bağımsız olarak yararlanmak, bu noktada FormView'ın bildirim temelli biçimlendirme görünüm ister karar:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample1.aspx)]

FormView'ın akıllı etiketinde etkinleştirmek disk belleği onay için bir dakikanızı ayırın; Bu ekler `AllowPaging="True"` özniteliği FormView'ın Tanımlayıcı Sözdizimi. Ayrıca, `EnableViewState` özelliğini false olarak ayarlayın.

## <a name="step-2-defining-theitemtemplates-markup"></a>2. adım: Tanımlama`ItemTemplate`'s biçimlendirme

Disk belleği desteklemek için ObjectDataSource denetimine bağlı ve yapılandırılmış FormView ile için içeriği belirtmek hazır değiliz `ItemTemplate`. Bu öğretici için şimdi görüntülenen ürün adına sahip bir `<h3>` başlığı. HTML kullanalım `<table>` burada birinci ve üçüncü sütun özellik adlarını listelemek ve ikinci ve dördüncü değerlerine listesinde dört sütunlu tabloda kalan ürün özelliklerini görüntüleyin.

Bu biçimlendirme Tasarımcısı'nda FormView'ın şablon düzenleme arabirimi aracılığıyla girilen veya bildirim temelli söz dizimi el ile girilebilir. Şablonları ile çalışırken, genellikle, doğrudan bildirim temelli söz dizimi ile çalışır, ancak en çok rahat hangi yöntemi kullanmak çekinmeyin daha hızlı bulabilirim.

Aşağıdaki biçimlendirmede sonra FormView bildirim temelli biçimlendirmeyi gösterir `ItemTemplate`'s yapısı tamamlandı:


[!code-aspx[Main](using-the-formview-s-templates-cs/samples/sample2.aspx)]

Dikkat databinding sözdizimi - `<%# Eval("ProductName") %>`için örnek doğrudan şablonun çıkış yerleştirilebilir. Diğer bir deyişle, onu bir etiket denetimin atanmaması `Text` özelliği. Örneğin, sahibiz `ProductName` görüntülenen değer bir `<h3>` öğesi kullanılarak `<h3><%# Eval("ProductName") %></h3>`, hangi Chai olarak işleyecek ürün için `<h3>Chai</h3>`.

`ProductPropertyLabel` Ve `ProductPropertyValue` CSS sınıfları ürün özellik adları ve değerleri stilini belirlemek için kullanılan `<table>`. Bu CSS sınıfları tanımlanan `Styles.css` ve özellik adları kalın ve sağa hizalı ve özellik değerlerini doldurma sağ eklemek neden olabilir.

Olduğundan hiçbir CheckBoxFields FormView ile kullanılabilir gösterebilmeniz `Discontinued` değeri bir onay kutusu biz kendi onay kutusu denetimi eklemeniz gerekir. `Enabled` Özelliği False, salt okunur yapma ve CheckBox'ın ayarlanmış `Checked` özellik değerine bağlı `Discontinued` veri alanı.

İle `ItemTemplate` tam, ürün bilgilerini bir çok daha esnektir şekilde da görüntülenir. Bu öğreticide (Şekil 4) FormView tarafından oluşturulan çıkış son öğretici (Şekil 3) DetailsView çıktısını karşılaştırır.


[![Katı DetailsView çıktı](using-the-formview-s-templates-cs/_static/image8.png)](using-the-formview-s-templates-cs/_static/image7.png)

**Şekil 3**: katı DetailsView çıkış ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-formview-s-templates-cs/_static/image9.png))


[![Akıcı FormView çıktı](using-the-formview-s-templates-cs/_static/image11.png)](using-the-formview-s-templates-cs/_static/image10.png)

**Şekil 4**: sıvı FormView çıkış ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-the-formview-s-templates-cs/_static/image12.png))


## <a name="summary"></a>Özet

GridView ve DetailsView denetimleri TemplateFields kullanarak özelleştirilmiş çıktılarını varken her ikisi de yine verilerine bir kılavuz benzeri, boxy biçimde sunar. Tek bir kaydını gerektiği zaman gösterilecek bu kez için daha az katı bir düzen kullanarak, FormView ideal bir seçimdir. Tek bir kayıttan FormView DetailsView gibi işler kendi `DataSource`, ancak DetailsView aksine yalnızca şablonları oluşur ve alanları desteklemez.

Biz bu öğreticide gördüğünüz gibi tek bir kaydını görüntülenirken FormView için daha esnek bir düzen sağlar. Gelecekte öğreticiler size esneklik FormsView olarak aynı düzeyde sağlar, ancak (gibi GridView) birden çok kayıt görüntüleyemeyecek DataList ve yineleyici denetimleri inceleyeceğiz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme E.R. edildi Gilmore. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](using-templatefields-in-the-detailsview-control-cs.md)
> [sonraki](displaying-summary-information-in-the-gridview-s-footer-cs.md)
