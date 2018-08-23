---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
title: DataList özelleştirme arabirimi (C#) düzenleme kullanıcının | Microsoft Docs
author: rick-anderson
description: Bu öğreticide DataList için bir DropDownList ve bir onay kutusu içeren daha zengin bir düzenleme arabirimi oluşturacağız.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: a5d13067-ddfb-4c36-8209-0f69fd40e45c
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 7f7723895dd50b1923de49ca4a3a7055bbad5fe4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753621"
---
<a name="customizing-the-datalists-editing-interface-c"></a>DataList'in düzenleme arabirimini (C#) özelleştirme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_CS.exe) veya [PDF olarak indirin](customizing-the-datalist-s-editing-interface-cs/_static/datatutorial40cs1.pdf)

> Bu öğreticide DataList için bir DropDownList ve bir onay kutusu içeren daha zengin bir düzenleme arabirimi oluşturacağız.


## <a name="introduction"></a>Giriş

İşaretleme ve Web denetimleri s DataList'te `EditItemTemplate` düzenlenebilir arabirimi tanımlayın. Tüm düzenlenebilir DataList örneklerin ediyoruz ve düzenlenebilir incelenirken şimdiye kadar arabirimi, metin kutusu Web denetimleri oluşan. İçinde [önceki öğretici](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md) doğrulama denetimleri ekleyerek düzenleme zaman kullanıcı deneyimini geliştirdik.

`EditItemTemplate` Daha fazla Web denetimleri gibi DropDownList, RadioButtonLists, takvimler, metin kutusunun dışında içerecek şekilde genişletilebilir ve benzeri. İle diğer Web denetimleri dahil etmek için düzenleme arabirimini özelleştirme, metin kutuları gibi, aşağıdaki adımları kullanın:

1. Web denetimine ekleme `EditItemTemplate`.
2. Uygun özelliğine karşılık gelen veri alan değeri atamak için veri bağlama söz dizimi kullanın.
3. İçinde `UpdateCommand` olay işleyicisi, program aracılığıyla Web erişim değerini denetlemek ve uygun BLL metoduna geçirin.

Bu öğreticide DataList için bir DropDownList ve bir onay kutusu içeren daha zengin bir düzenleme arabirimi oluşturacağız. Özellikle, ürün bilgileri listeleyen ve s ürün adı, tedarikçi, kategori ve artık sağlanmayan durum güncelleştirilecek izin veren bir DataList oluşturacağız (bkz. Şekil 1).


[![Bir metin kutusu, iki DropDownList ve bir onay kutusu düzenleme arabirimi içerir.](customizing-the-datalist-s-editing-interface-cs/_static/image2.png)](customizing-the-datalist-s-editing-interface-cs/_static/image1.png)

**Şekil 1**: bir metin kutusu, iki DropDownList ve bir onay kutusu düzenleme arabirimi içerir ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-datalist-s-editing-interface-cs/_static/image3.png))


## <a name="step-1-displaying-product-information"></a>1. adım: Ürün bilgilerini görüntüleme

DataList s düzenlenebilir arabirimi oluşturabiliriz önce ilk salt okunur arabirimi oluşturmak ihtiyacımız var. Başlangıç açarak `CustomizedUI.aspx` gelen sayfasında `EditDeleteDataList` klasörü ve DataList sayfasına ekleme Designer'dan ayarlama, `ID` özelliğini `Products`. DataList s akıllı etiketten yeni ObjectDataSource oluşturun. Bu yeni ObjectDataSource ad `ProductsDataSource` ve bu verileri almak için yapılandırma `ProductsBLL` s sınıfı `GetProducts` yöntemi. Olarak önceki düzenlenebilir DataList öğreticiler ile düzenlenen ürün s bilgileri doğrudan iş mantığı katmanı giderek güncelleştireceğiz. Buna göre güncelleştirme, ekleme, açılan listeler ayarlayın ve sekme (hiçbiri) SİLİN.


[![UPDATE, INSERT ve DELETE sekmeleri açılan listeleri (yok)](customizing-the-datalist-s-editing-interface-cs/_static/image5.png)](customizing-the-datalist-s-editing-interface-cs/_static/image4.png)

**Şekil 2**: güncelleştirme, ekleme ve silme sekmelerini açılan listeler (hiçbiri) ayarlayın ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-datalist-s-editing-interface-cs/_static/image6.png))


ObjectDataSource yapılandırdıktan sonra Visual Studio varsayılan oluşturacak `ItemTemplate` ad ve değer veri alanların her biri için listeler DataList döndürdü. Değiştirme `ItemTemplate` şablonu, ürün adı listeler. böylece bir `<h4>` kategori adı, sağlayıcı adı, fiyatı ve artık sağlanmayan durum birlikte öğesi. Ayrıca, dikkat ederek bir Düzenle düğmesi ekleyin, `CommandName` özelliği, düzenleme için ayarlanır. Bildirim temelli biçimlendirme için benim `ItemTemplate` izler:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Ürün bilgileri kullanarak yukarıdaki biçimlendirme yerleştirir bir &lt;h4&gt; s ürün adı ve bir dört sütunlu için başlık `<table>` kalan alanlar için. `ProductPropertyLabel` Ve `ProductPropertyValue` tanımlanan CSS sınıfları `Styles.css`, önceki öğreticilerdeki değerlendirildi. Şekil 3'te bir tarayıcıdan görüntülendiğinde ilerleme gösterir.


[![Ad, tedarikçi, kategori, kullanımdan durumu ve her ürünün fiyatını görüntülenir](customizing-the-datalist-s-editing-interface-cs/_static/image8.png)](customizing-the-datalist-s-editing-interface-cs/_static/image7.png)

**Şekil 3**: ad, tedarikçi, kategori, kullanımdan durumu ve her ürünün fiyatını görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-datalist-s-editing-interface-cs/_static/image9.png))


## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>2. adım: düzenleme arabirime Web denetimleri ekleme

Düzenleme arabirimidir gerekli Web denetimler eklemek için özelleştirilmiş DataList oluşturmanın ilk adımı `EditItemTemplate`. Özellikle, bir DropDownList başka bir tedarikçi ve bir onay kutusu artık sağlanmayan durum kategorisi için ihtiyacımız var. Ürün s fiyatı Bu örnekte düzenlenebilir olmadığından, biz görüntüleyen bir etiket Web denetimi kullanarak devam edebilirsiniz.

Düzenleme arabirimini özelleştirme için DataList s akıllı etiketinde Şablonları Düzenle bağlantısına tıklayın ve seçin `EditItemTemplate` aşağı açılan listeden seçeneği. Bir DropDownList için ekleme `EditItemTemplate` ve kendi `ID` için `Categories`.


[![Bir DropDownList kategorilerini ekleyin](customizing-the-datalist-s-editing-interface-cs/_static/image11.png)](customizing-the-datalist-s-editing-interface-cs/_static/image10.png)

**Şekil 4**: bir DropDownList kategorilerini ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-datalist-s-editing-interface-cs/_static/image12.png))


Ardından, DropDownList s akıllı etiket veri kaynağı Seç seçeneğini belirleyin ve adlı yeni bir ObjectDataSource oluşturma `CategoriesDataSource`. Kullanılacak bu ObjectDataSource yapılandırma `CategoriesBLL` s sınıfı `GetCategories()` metodu (bkz: Şekil 5). Ardından, her biri için kullanılacak veri alanlar için veri kaynağı Yapılandırma Sihirbazı DropDownList s ister `ListItem` s `Text` ve `Value` özellikleri. DropDownList görüntülemesi `CategoryName` veri alan ve kullanım `CategoryID` Şekil 6'da gösterildiği gibi değeri.


[![CategoriesDataSource adlı yeni bir ObjectDataSource oluşturma](customizing-the-datalist-s-editing-interface-cs/_static/image14.png)](customizing-the-datalist-s-editing-interface-cs/_static/image13.png)

**Şekil 5**: adlı yeni bir ObjectDataSource oluşturma `CategoriesDataSource` ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-datalist-s-editing-interface-cs/_static/image15.png))


[![DropDownList s görünümünü yapılandırma ve alan değeri](customizing-the-datalist-s-editing-interface-cs/_static/image17.png)](customizing-the-datalist-s-editing-interface-cs/_static/image16.png)

**Şekil 6**: DropDownList s görüntüleme ve değer alanları yapılandırın ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-datalist-s-editing-interface-cs/_static/image18.png))


Bu dizi bir DropDownList tedarikçileri oluşturmak için adımları yineleyin. Ayarlama `ID` bu DropDownList'e için `Suppliers` ve kendi ObjectDataSource ad `SuppliersDataSource`.

İki DropDownList ekledikten sonra artık sağlanmayan durum için bir onay kutusu ve ürün adı için bir metin kutusu ekleyin. Ayarlama `ID` TextBox'a ve onay kutusu için s `Discontinued` ve `ProductName`sırasıyla. Kullanıcı s ürün adı için bir değer sağladığından emin olmak için bir RequiredFieldValidator ekleyin.

Son olarak, Güncelleştir ve İptal düğmeleri ekleme. Bu iki düğme için bunu kesinlik temelli unutmayın, kendi `CommandName` özellikleri, Güncelleştir ve iptal etmek, sırasıyla için ayarlanır.

İstediğiniz gibi düzenleme arabirimi düzenleme çekinmeyin. Ben ve kabul dört sütunlu kullandığınızdan `<table>` salt okunur arabiriminden aşağıdaki bildirim temelli söz dizimi ve ekran düzeni göstermektedir:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample2.aspx)]


[![Düzenleme gibi salt okunur arabirimi kullanıma düzenlenir arabirimdir](customizing-the-datalist-s-editing-interface-cs/_static/image20.png)](customizing-the-datalist-s-editing-interface-cs/_static/image19.png)

**Şekil 7**: düzenleme arabirimini olduğunu düzenlenir salt okunur arabirimi gibi ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-datalist-s-editing-interface-cs/_static/image21.png))


## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>3. adım: EditCommand ve CancelCommand olay işleyicileri oluşturma

Şu anda hiçbir veri bağlama sözdizimi yoktur `EditItemTemplate` (dışında `UnitPriceLabel`, hangi kopyalanmıştır üzerinden verbatim `ItemTemplate`). Veri bağlama söz dizimi kısa bir süre içinde ekleyeceğiz ancak ilk let s s DataList için olay işleyicileri oluşturma `EditCommand` ve `CancelCommand` olayları. Bu geri çağırma sorumluluğu `EditCommand` olay işleyicisidir ise, Düzenle düğmesine tıklandığında, DataList öğesi düzenleme arabirimi işlemek için `CancelCommand` s iş DataList önceden düzenleme durumuna döndürmek için.

Bu iki olay işleyicileri oluşturun ve bunları aşağıdaki kodu kullanın:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample3.cs)]

Bu iki olay işleyicileri Düzenle düğmesine tıklayarak, yerinde düzenleme arabirimini görüntüler ve iptal düğmesine tıklandığında, salt okunur moda düzenlenen öğeyi döndürür. Şekil 8, Chef Acı s Baharat karışımı Düzenle düğmesine tıkladıktan sonra DataList gösterir. Biz bu yana herhangi bir veri bağlama söz dizimi düzenleme arabirimine henüz eklemek ve `ProductName` metin kutusu boşsa `Discontinued` onay kutusunu işaretlemeden ve ilk öğe seçilir `Categories` ve `Suppliers` DropDownList.


[![Düzenle düğmesi görüntüler düzenleme arabirimi tıklayarak](customizing-the-datalist-s-editing-interface-cs/_static/image23.png)](customizing-the-datalist-s-editing-interface-cs/_static/image22.png)

**Şekil 8**: Düzenle düğmesine tıklayarak düzenleme arabirimini görüntüler ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-datalist-s-editing-interface-cs/_static/image24.png))


## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>4. adım: veri bağlama söz dizimi düzenleme arabirimi ekleme

Geçerli ürün s değerlerini görüntüleme düzenleme arabirimi için biz veri alan değerlerini uygun Web denetim değerleri atamak için veri bağlama söz dizimi kullanmanız gerekir. Veri bağlama söz dizimi için şablonları düzenleme ekranına giderek Tasarımcısı uygulanabilir ve akıllı etiketler Web'den veri bağlamaları Düzenle bağlantısını seçerek denetler. Alternatif olarak, veri bağlama söz dizimi, bildirim temelli işaretlemede doğrudan eklenebilir.

Ata `ProductName` veri alan için değer `ProductName` TextBox s `Text` özelliği `CategoryID` ve `SupplierID` veri alanı değerlerini `Categories` ve `Suppliers` DropDownList `SelectedValue` özellikleri ve `Discontinued` veri alan için değer `Discontinued` onay kutusu s `Checked` özelliği. Tasarımcı veya doğrudan yoluyla bildirim temelli işaretleme, bu değişiklikleri yaptıktan sonra sayfanın bir tarayıcı aracılığıyla yeniden ziyaret ve Chef Acı s Baharat karışımı Düzenle düğmesini tıklatın. Şekil 9 gösterildiği gibi veri bağlama söz dizimi geçerli değerlerin metin kutusu, DropDownList ve onay kutusu eklemiştir.


[![Düzenle düğmesi görüntüler düzenleme arabirimi tıklayarak](customizing-the-datalist-s-editing-interface-cs/_static/image26.png)](customizing-the-datalist-s-editing-interface-cs/_static/image25.png)

**Şekil 9**: Düzenle düğmesine tıklayarak düzenleme arabirimini görüntüler ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-datalist-s-editing-interface-cs/_static/image27.png))


## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>5. adım: kullanıcı s değişiklikleri UpdateCommand olay işleyicisi kaydediliyor

Kullanıcı bir ürün düzenler ve geri gönderme gerçekleşir güncelleştir düğmesine tıklar ve DataList s `UpdateCommand` olay harekete geçirilir. Olay işleyicisi, Web denetimlerinde değerleri okunacak ihtiyacımız `EditItemTemplate` ve BLL ürünü veritabanında güncelleştirmek için arabirim. Şöyle ve önceki öğreticilerde, görülen `ProductID` güncelleştirilmiş ürününü aracılığıyla erişilebilir `DataKeys` koleksiyonu. Kullanıcı tarafından girilen alanları kullanarak Web denetimleri başvurarak program aracılığıyla erişilen `FindControl("controlID")`aşağıdaki kodda gösterildiği gibi:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample4.cs)]

Danışmanlık tarafından kod başlar `Page.IsValid` özelliği sayfadaki tüm doğrulama denetimleri geçerli olduğundan emin olun. Varsa `Page.IsValid` olduğu `True`, düzenlenen ürün s `ProductID` değer okuma `DataKeys` toplama ve Web denetimleri içinde veri girişi `EditItemTemplate` programlı olarak başvurulur. Ardından, ardından uygun geçirilen değişkenleri halinde bu Web denetimleri değerleri okumak `UpdateProduct` aşırı yükleme. DataList veri güncelleştirdikten sonra önceden düzenleme durumuna döndürülür.

> [!NOTE]
> Ben ve özel durum işleme mantığı eklenen atlanmış [işleme BLL ve DAL düzeyi özel durumları](handling-bll-and-dal-level-exceptions-cs.md) kod ve bu örnek tutulabilmesi için öğretici odaklanır. Bir alıştırma olarak, bu öğreticiyi tamamladıktan sonra bu işlevler ekler.


## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>6. adım: NULL CategoryID ve SupplierID değerleri işleme

İçin Northwind veritabanı sağlayan `NULL` değerleri `Products` tablo s `CategoryID` ve `SupplierID` sütunları. Ancak, düzenleme, arabirim eklenmemişse t şu anda uyum `NULL` değerleri. Biz sahip bir ürün düzenlemeye çalışırsanız bir `NULL` değeri ya da kendi `CategoryID` veya `SupplierID` sütunları, biz elde edeceğiniz bir `ArgumentOutOfRangeException` benzer bir hata iletisiyle: *'Kategorisi' mevcut olmadığından, geçersiz olan bir SelectedValue vardır öğeler listesinde yok.* Ayrıca, şu anda orada s s ürün kategorisi veya tedarikçi değiştirme olanağı yoktur değeri olmayan bir`NULL` değerini bir `NULL` biri.

Desteklemek için `NULL` değerler kategori ve tedarikçi DropDownList ihtiyacımız bir ek eklemek `ListItem`. Ben (hiçbiri) olarak kullanmayı seçtiniz ve `Text` bu değeri `ListItem`, ancak d, isterseniz bunu (boş dize gibi) başka bir şekilde değiştirebilirsiniz. Son olarak, DropDownList ayarlamayı unutmayın `AppendDataBoundItems` için `True`; bunu kategorileri yapmak, parantezi unutsanız ve DropDownList'e bağlı tedarikçileri statik olarak eklenen üzerine yazılacak `ListItem`.

DataList s DropDownList işaretlemede bu değişiklikleri yaptıktan sonra `EditItemTemplate` aşağıdakine benzer görünmelidir:


[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> Statik `ListItem` s Tasarımcı veya bildirim temelli söz dizimi aracılığıyla doğrudan bir DropDownList eklenebilir. Bir DropDownList öğesini temsil eden bir veritabanı eklerken `NULL` eklediğinizden emin olun, değer `ListItem` bildirim temelli söz dizimi aracılığıyla. Kullanırsanız `ListItem` Koleksiyonu Düzenleyicisi Tasarımcısı'nda oluşturulan bildirim temelli söz dizimi sayar `Value` tamamen ayarlama gibi bildirim temelli biçimlendirme oluşturma, boş bir dize atanmış: `<asp:ListItem>(None)</asp:ListItem>`. Bu zararsız, görünebilir ancak eksik `Value` kullanılacak DropDownList neden `Text` onun yerine özellik değeri. Olması durumunda bu anlamına `NULL` `ListItem` olduğu belirlenirse, değeri (hiçbiri) ürün veri alanına atanacak kurulmaya çalışılır (`CategoryID` veya `SupplierID`, bu öğreticideki), bir özel durum oluşur. Açıkça ayarlayarak `Value=""`, `NULL` ürüne değeri atanır veri alanı `NULL` `ListItem` seçilir.


Bir tarayıcı aracılığıyla bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın. Bir ürün düzenlerken unutmayın `Categories` ve `Suppliers` (hiçbiri) sahip iki DropDownList DropDownList başlangıcında seçeneği.


[![Kategorileri ve tedarikçileri DropDownList bir (hiçbiri) seçeneği içerir](customizing-the-datalist-s-editing-interface-cs/_static/image29.png)](customizing-the-datalist-s-editing-interface-cs/_static/image28.png)

**Şekil 10**: `Categories` ve `Suppliers` DropDownList bir (hiçbiri) seçeneği içerir ([tam boyutlu görüntüyü görmek için tıklatın](customizing-the-datalist-s-editing-interface-cs/_static/image30.png))


Kaydetmek için bir veritabanı olarak (hiçbiri) seçeneğini `NULL` değeri ihtiyacımız dönmek `UpdateCommand` olay işleyicisi. Değişiklik `categoryIDValue` ve `supplierIDValue` boş değer atanabilir bir tamsayı olması ve dışında bir değer atamak için değişkenleri `Nothing` yalnızca DropDownList s `SelectedValue` boş bir dize değil:


[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample6.cs)]

Bu değişiklik, bir değeri ile `Nothing` yöntemlere geçirilen `UpdateProduct` kullanıcı (hiçbiri) seçtiyseniz BLL yöntemi seçeneğini ya da açılan listeleri, karşılık gelen bir `NULL` veritabanı değeri.

## <a name="summary"></a>Özet

Bu öğreticide, üç farklı giriş Web denetimleri TextBox, iki DropDownList ve doğrulama denetimleri ile birlikte bir onay kutusu dahil daha karmaşık bir düzenleme DataList arabirimi oluşturmak nasıl gördük. Düzenleme arabirimi oluştururken kullanılan Web denetimleri bağımsız olarak aynı adımlardır: DataList s Web denetimleri ekleyerek başlayın `EditItemTemplate`; uygun Web ile ilgili veri alan değerlerini atamak için veri bağlama söz dizimini kullanın Denetim Özellikleri. hem de `UpdateCommand` olay işleyicisi, Web denetimleri ve BLL değerlerine geçirme özelliklerini uygun programlamayla erişme.

Düzenleme bir arabirim olup olmadığını oluştururken, s oluşan yalnızca metin kutuları veya farklı Web denetimler koleksiyonuna, doğru veritabanını işlemek mutlaka `NULL` değerleri. Hesap, `NULL` s, bunu varolan yalnızca düzgün görüntülenmesini zorunlu `NULL` düzenleme arabirimi, aynı zamanda, değeri bir değer olarak işaretlemek için bir yol sunmak `NULL`. Belirleyebilen içinde DropDownList için genellikle anlamına statik ekleme `ListItem` olan `Value` özelliği boş dize olarak açıkça ayarlayın (`Value=""`), biraz kod ekleyerek `UpdateCommand` belirlemek için olay işleyicisi olup olmadığı `NULL``ListItem` seçilmedi.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Dennis Patterson ve Konuğu, David Suru ve Randy Etikan yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
> [İleri](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
