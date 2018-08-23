---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: DataList ve Repeater (C#) özel düğmeler | Microsoft Docs
author: rick-anderson
description: Bu öğreticide kategorileri sistemde bir düğmenin kendi associ sağlayarak her kategorisiyle listelemek için bir yineleyici kullanan bir arabirim oluşturacağız...
ms.author: riande
ms.date: 11/13/2006
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: a10acd00dd8243f92c1b255acb8328e2b76e87cc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41753634"
---
<a name="custom-buttons-in-the-datalist-and-repeater-c"></a>Özel düğmeler DataList ve Repeater'daki (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe) veya [PDF olarak indirin](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> Bu öğreticide kategorileri sistemde Bulletedlıst denetimini kullanarak kendi ilişkili ürünleri bir düğmenin sağlayarak her kategorisiyle listelemek için bir yineleyici kullanan bir arabirim oluşturacağız.


## <a name="introduction"></a>Giriş

Son yedi DataList ve Repeater öğretici boyunca ediyoruz ve oluşturulan hem salt okunur örnekleri düzenleme ve örnekleri siliniyor. DataList s düğmelerini ekledik düzenleme ve silme özelliklerini DataList içinde kolaylaştırmak için `ItemTemplate` , tıklandığında geri göndermeye neden ve düğmeyi s karşılık gelen bir DataList olayı `CommandName` özelliği. Örneğin, bir düğme ekleme `ItemTemplate` ile bir `CommandName` düzenleme özelliği değeri neden olur, s DataList `EditCommand` geri göndermede; ateşlenmesine biriyle `CommandName` başlatır Delete `DeleteCommand`.

Buna ek olarak Düzenle ve Sil düğmeleri için DataList ve Repeater denetimleri de düğmeler, LinkButtons veya ImageButtons içerebilir, tıklandığında, özel sunucu tarafı mantık gerçekleştirin. Bu öğreticide kategorileri sistemde listelemek için bir yineleyici kullanan bir arabirim oluşturacağız. Her kategori için Repeater Bulletedlıst denetimi kullanarak s ilişkili ürünleri bir düğmenin kategorisi içerir (bkz. Şekil 1).


[![Bir madde işaretli liste kategori s ürünleri göster ürünleri bağlantı görüntüler tıklayarak](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**Şekil 1**: bir madde işaretli liste kategorisinde s ürünleri göster ürünleri bağlantı görüntüler tıklayarak ([tam boyutlu görüntüyü görmek için tıklatın](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>1. adım: özel düğme Eğitmen Web sayfaları ekleme

Özel bir düğme eklemek üzere nasıl tümleştirildiği incelenmektedir önce öncelikle Bu öğretici için yapmamız gereken Web sitesi Projemizin ASP.NET sayfaları oluşturmak için bir dakikanızı ayırın s olanak tanır. Başlangıç adlı yeni bir klasör ekleyerek `CustomButtonsDataListRepeater`. Ardından, o klasördeki her bir sayfayla ilişkilendirilecek emin olmak için aşağıdaki iki ASP.NET sayfaları ekleyin `Site.master` ana sayfa:

- `Default.aspx`
- `CustomButtons.aspx`


![Özel düğmeler ilgili öğreticiler için ASP.NET sayfaları ekleme](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**Şekil 2**: özel düğmeler ilgili öğreticiler için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `CustomButtonsDataListRepeater` klasörü kendi bölümünde öğreticileri listeler. Bu geri çağırma `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu kullanıcı denetimine ekleme `Default.aspx` sayfaya s Tasarım görünümü Çözüm Gezgini'nde sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**Şekil 3**: ekleme `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))


Son olarak, girişleri olarak istediğiniz sayfaları eklemek `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme sayfalama ve sıralama sonra DataList ve Repeater ile eklemeniz `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticiler Web sitesini görüntülemek için bir dakikanızı ayırın. Sol taraftaki menüden, artık düzenleme, ekleme ve silme öğreticiler için öğeleri içerir.


![Site Haritası, giriş artık özel düğmeler öğretici içerir.](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**Şekil 4**: Site Haritası, giriş artık özel düğmeler öğretici içerir.


## <a name="step-2-adding-the-list-of-categories"></a>2. adım: kategori listesi ekleme

Bu öğretici için size bir Göster ürünleri LinkButton yanı sıra tüm kategorileri listeleyen bir yineleyici oluşturmanız gerekir, tıklandığında bir madde işaretli listede ilişkili kategori s ürünleri görüntüler. İlk sistemde kategorileri listeler basit bir yineleyici oluşturun s olanak tanır. Başlangıç açarak `CustomButtons.aspx` sayfasını `CustomButtonsDataListRepeater` klasör. Repeater'da kümesi ve Tasarımcısı araç kutusundan sürükleyin, `ID` özelliğini `Categories`. Ardından, yeni bir veri kaynağı denetimi Repeater s akıllı etiketten oluşturun. Özellikle, adlı yeni bir ObjectDataSource denetimi oluşturma `CategoriesDataSource` , verileri seçer `CategoriesBLL` s sınıfı `GetCategories()` yöntemi.


[![ObjectDataSource CategoriesBLL sınıfı s GetCategories() yöntemi kullanmak üzere yapılandırma](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**Şekil 5**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` s sınıfı `GetCategories()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))


Visual Studio varsayılan oluşturur DataList denetimi aksine `ItemTemplate` veri kaynağını temel alan, yineleyici s şablonları el ile tanımlanması gerekir. Ayrıca, yineleyici s şablonları oluşturulmalı ve bildirimli olarak düzenlenen (diğer bir deyişle, orada s hiçbir Düzen şablonları yineleyici s akıllı etiketinde seçeneği).

Sol alt köşedeki kaynak sekmesine tıklayın ve Ekle bir `ItemTemplate` s kategori adını görüntüler bir `<h3>` öğesi ve bir paragraf, açıklama, etiket; dahil bir `SeparatorTemplate` yatay bir kural görüntüleyen (`<hr />`) arasındaki Kategori. Ayrıca bir Linkbutton'a ekleyin, `Text` özelliği için ürünleri göster. Bu adımları tamamladıktan sonra sayfa s bildirim temelli biçimlendirmeyi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

Şekil 6, sayfada bir tarayıcıdan görüntülendiğinde gösterilir. Her kategori adı ve açıklaması listelenir. Ürünleri Göster düğmesine tıklandığında, geri göndermeye neden olur, ancak henüz herhangi bir işlem gerçekleştirmez.


[![Her kategori s ad ve açıklama göster ürünleri LinkButton birlikte görüntülenir](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**Şekil 6**: her kategori s ad ve açıklama görüntülenir, birlikte Göster ürünleri LinkButton ([tam boyutlu görüntüyü görmek için tıklatın](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>3. adım: Yürütülen sunucu tarafı mantığı olduğunda göster ürünleri Linkbutton'a Tıklandı

Bir düğme, LinkButton veya DataList veya Repeater şablonu içindeki ImageButton tıklandı her zaman, bir geri gönderme gerçekleşir ve DataList veya Repeater s `ItemCommand` olay harekete geçirilir. Ek olarak `ItemCommand` DataList denetimi da yükseltmek başka bir, daha belirli bir olay, olay s düğmesi `CommandName` özelliği (silme, düzenleme, iptal, güncelleştirme veya Seç) ayrılmış dizeleri birine ayarlanır ancak `ItemCommand` olaydır*her zaman* tetiklendi.

DataList veya Repeater içinde bir düğmeye tıklandığında, önerilmesine (birden çok düğmeler gibi her iki bir düzenleme denetiminde ve Sil düğmesine olabileceğini durumda) hangi düğmesine tıklandığını ve belki de bazı ek bilgiler boyunca (geçirileceğini ihtiyacımız birincil anahtar değeri olan düğmesine tıklandığını öğesi). Düğme, LinkButton ve ImageButton iki özellik değerleri geçirilir sağlayan `ItemCommand` olay işleyicisi:

- `CommandName` genellikle şablondaki her düğme tanımlamak için kullanılan bir dize
- `CommandArgument` birincil anahtar değeri gibi bazı veri alanının değerini tutmak için yaygın olarak kullanılan

Bu örnekte, LinkButton s ayarlamak `CommandName` ShowProducts ve geçerli kayıt s birincil anahtar değeri bağlama özelliğini `CategoryID` için `CommandArgument` özelliği veri bağlama söz dizimini kullanarak `CategoryArgument='<%# Eval("CategoryID") %>'`. Bu iki özellik belirttikten sonra LinkButton s bildirim temelli söz dizimi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

Düğme, bir geri gönderme gerçekleşir tıklandığında ve DataList veya Repeater s `ItemCommand` olay harekete geçirilir. S düğmesi olay işleyicisi geçirilir `CommandName` ve `CommandArgument` değerleri.

S yineleyici için bir olay işleyicisi oluşturun `ItemCommand` olay ve ikinci parametre Not olay işleyicisine geçirilen (adlı `e`). Bu ikinci parametre türüdür [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) ve aşağıdaki dört özelliklere sahiptir:

- `CommandArgument` tıklandı düğmeyi s değerinin `CommandArgument` özelliği
- `CommandName` s düğmesi değerini `CommandName` özelliği
- `CommandSource` bir başvuru tıklandığını düğme denetimi
- `Item` bir başvuru [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) tıklandığını düğmesi içerir; her kayıt için bir yineleyici bağlı olarak bildirilen bir `RepeaterItem`

Seçili kategoriyi s beri `CategoryID` aracılığıyla geçirilen `CommandArgument` özelliği, biz seçilen kategori ile ilişkili ürünleri kümesini alabilirsiniz `ItemCommand` olay işleyicisi. Bu ürünler ardından Bulletedlıst denetiminde bağlanabilir `ItemTemplate` (hangi ediyoruz ve henüz eklemek için). İçinde kalır, ardından, Bulletedlıst eklemektir tüm başvuru `ItemCommand` olay işleyicisi ve ürünleri için size adım 4'te üstesinden seçilen kategori kümesini bağlar.

> [!NOTE]
> DataList s `ItemCommand` olay işleyicisi, türünde bir nesne geçirilir [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), aynı dört özellikleri sunan `RepeaterCommandEventArgs` sınıfı.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>4. adım: bir madde işaretli listede seçilen kategori s ürünleri görüntüleme

Seçilen kategori s ürünleri s yineleyici içinde görüntülenebilen `ItemTemplate` herhangi bir sayıda denetimleri kullanarak. Başka bir yineleyici, bir DataList, bir DropDownList bir GridView ve benzeri iç içe geçmiş eklemeniz mümkündür. Ancak, bu yana bir madde işaretli liste ürünleri görüntülemek istiyoruz Bulletedlıst denetim kullanacağız. Dönme `CustomButtons.aspx` s bildirim temelli biçimlendirme sayfasında, bir Bulletedlıst denetimine ekleme `ItemTemplate` Göster ürünleri LinkButton sonra. BulletedLists s ayarlamak `ID` için `ProductsInCategory`. Belirtilen veri alanının değeri Bulletedlıst görüntüler `DataTextField` özelliği; bu denetim kümesi, kendisiyle ilişkili ürün bilgileri olduğundan `DataTextField` özelliğini `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

İçinde `ItemCommand` olay işleyicisi, bu denetimi kullanarak başvuru `e.Item.FindControl("ProductsInCategory")` ve seçilen kategori ile ilişkili ürünleri kümesine bağlayın.


[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

Herhangi bir işlem gerçekleştirmeden önce `ItemCommand` olay işleyicisi, s gelen değeri ilk denetlenecek akıllıca `CommandName`. Bu yana `ItemCommand` olay işleyicisi harekete *herhangi* düğmesine tıklandığında, şablonu kullanımda birden çok düğmeleri varsa `CommandName` hangi eylemin yapılacağını elde edilmesi güç değeri. Denetimi `CommandName` İşte moot, biz yalnızca tek bir düğmeye sahip olduğundan, ancak bu iyi bir forma alışkanlıktır. Ardından, `CategoryID` seçilen kategori alınır `CommandArgument` özelliği. Bulletedlıst denetimin şablonunda ardından başvurulan ve sonuçlarına bağlı `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi.

İçinde bir DataList düğmeleri gibi kullanılan önceki öğreticilerde [, bir genel bakış düzenleme ve silme DataList'te verileri](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md), biz belirli bir öğe birincil anahtar değerinin belirlendiği `DataKeys` koleksiyonu. Bu yaklaşım da DataList'i ile çalışırken, yineleyici bulunmayan bir `DataKeys` özelliği. Bunun yerine, biz birincil anahtar değeri gibi s düğmesi sağlama için alternatif bir yaklaşım kullanmalısınız `CommandArgument` özelliği veya şablon içinde bir gizli etiket Web denetimi için birincil anahtar değerini atayarak ve değeriyle geri Okuma`ItemCommand`kullanarak olay işleyicisi `e.Item.FindControl("LabelID")`.

Tamamladıktan sonra `ItemCommand` olay işleyicisi, bir tarayıcıda bu sayfası test etmek için bir dakikanızı ayırın. Şekil 7 gösterildiği gibi göster ürünleri tıklayarak bağlantı geri göndermeye neden olur ve bir Bulletedlıst seçilen kategori için ürünleri gösterir. Ayrıca, diğer kategorilerde ürünleri göster bağlantıları tıklanan olsa bile bu ürün bilgileri kalmasını unutmayın.

> [!NOTE]
> Bir kerede yalnızca bir kategori s ürünleri listelenir, da, bu raporun davranışını değiştirmek istiyorsanız, s Bulletedlıst denetimi ayarlamanız yeterlidir `EnableViewState` özelliğini `False`.


[![Bir Bulletedlıst seçili kategorinin ürünleri görüntülemek için kullanılır.](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**Şekil 7**: A Bulletedlıst, seçilen kategori ürünleri görüntülemek için kullanılır ([tam boyutlu görüntüyü görmek için tıklatın](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))


## <a name="summary"></a>Özet

DataList ve Repeater denetimleri kendi şablonlarında düğmeler, LinkButtons veya ImageButtons herhangi bir sayı içerebilir. Böyle düğme tıklandığında, bir geri göndermeye neden olur ve yükseltmek `ItemCommand` olay. Özel sunucu tarafı eylem bir düğmeye tıkladı ile ilişkilendirilecek olay işleyicisi oluşturma `ItemCommand` olay. Bu olay işleyicisinde gelen ilk denetleyin `CommandName` hangi düğmesine tıklandığını belirleme için değer. Ek bilgi s düğmesi isteğe bağlı olarak sağlanabilir `CommandArgument` özelliği.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Dennis Patterson ve Konuğu oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](custom-buttons-in-the-datalist-and-repeater-vb.md)
