---
uid: web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
title: "Özel düğmeler DataList ve yineleyici (C#) | Microsoft Docs"
author: rick-anderson
description: "Bu öğreticide biz kategorileri sistemde kendi associ göstermek için bir düğmeye sağlayan her kategorisiyle listelemek için bir yineleyici kullanan bir arabirim yapı..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2006
ms.topic: article
ms.assetid: 1f42e332-78dc-438b-9e35-0c97aa0ad929
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-button-actions-with-the-datalist-and-repeater/custom-buttons-in-the-datalist-and-repeater-cs
msc.type: authoredcontent
ms.openlocfilehash: 9a072ae18bbb19d086eb825c6e72b68d40b2e429
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="custom-buttons-in-the-datalist-and-repeater-c"></a>Özel düğmeler DataList ve yineleyici (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_46_CS.exe) veya [PDF indirin](custom-buttons-in-the-datalist-and-repeater-cs/_static/datatutorial46cs1.pdf)

> Bu öğreticide biz kategorileri sistemde Bulletedlıst denetimini kullanarak ilişkili ürünlerinden göstermek için bir düğmeye sağlayan her kategorisiyle listelemek için bir yineleyici kullanan bir arabirim yapı.


## <a name="introduction"></a>Giriş

Son seventeen DataList ve yineleyici öğreticileri boyunca biz ve oluşturulan hem salt okunur örnekler ve düzenleme ve örnekler silme. S DataList düğmeleri eklediğimiz düzenleme ve silme DataList içinde özellikleri kolaylaştırmak için `ItemTemplate` , tıklatıldığında geri gönderimin neden ve düğme s karşılık gelen bir DataList olayı `CommandName` özelliği. Örneğin, bir düğme ekleme `ItemTemplate` ile bir `CommandName` düzenleme özelliği değeri neden s DataList `EditCommand` geri göndermede; tetiklenecek biriyle `CommandName` silmek başlatır `DeleteCommand`.

Ayrıca düzenleme ve silme düğmeleri için DataList ve yineleyici denetimleri de düğmeler, LinkButtons veya ImageButtons içerir, tıklatıldığında, bazı özel sunucu tarafı mantığını gerçekleştirirler. Bu öğreticide biz sistemde kategorilerini liste için bir yineleyici kullanan bir arabirim yapı. Her kategori için yineleyici Bulletedlıst denetimini kullanarak ilişkili s ürün kategorisi göstermek için bir düğme içerir (bkz: Şekil 1).


[![Göster ürünleri bağlantı görüntüler madde işaretli listede kategori s ürünleri tıklatarak](custom-buttons-in-the-datalist-and-repeater-cs/_static/image2.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image1.png)

**Şekil 1**: Madde işaretli listede s ürün kategorisi Göster ürünleri bağlantı görüntüler'i tıklatarak ([tam boyutlu görüntüyü görüntülemek için tıklatın](custom-buttons-in-the-datalist-and-repeater-cs/_static/image3.png))


## <a name="step-1-adding-the-custom-button-tutorial-web-pages"></a>1. adım: özel düğme öğretici Web sayfaları ekleme

Özel bir düğme eklemek üzere nasıl tümleştirildiği incelenmektedir önce öncelikle Bu öğretici için yapmamız gereken Web sitesi Projemizin ASP.NET sayfaları oluşturmak için bir dakikanızı ayırın s olanak tanır. Adlı yeni bir klasör eklemeye başlayın `CustomButtonsDataListRepeater`. Ardından, o klasördeki her sayfayla ilişkilendirilecek emin olmak için aşağıdaki iki ASP.NET sayfaları ekleme `Site.master` ana sayfa:

- `Default.aspx`
- `CustomButtons.aspx`


![Özel düğmeler ilgili öğreticileri için ASP.NET sayfaları ekleme](custom-buttons-in-the-datalist-and-repeater-cs/_static/image4.png)

**Şekil 2**: özel düğmeler ilgili öğreticileri için ASP.NET sayfaları ekleme


Diğer klasörler gibi `Default.aspx` içinde `CustomButtonsDataListRepeater` klasörü öğreticileri kendi bölümünde listeler. Sözcüğünün `SectionLevelTutorialListing.ascx` kullanıcı denetimi bu işlevselliği sağlar. Bu kullanıcı denetimi Ekle `Default.aspx` s Tasarım görünümü sayfaya Çözüm Gezgini'nden sürükleyerek.


[![İçin Default.aspx SectionLevelTutorialListing.ascx kullanıcı denetimi Ekle](custom-buttons-in-the-datalist-and-repeater-cs/_static/image6.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image5.png)

**Şekil 3**: eklemek `SectionLevelTutorialListing.ascx` kullanıcı denetimine `Default.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](custom-buttons-in-the-datalist-and-repeater-cs/_static/image7.png))


Son olarak, girişleri olarak sayfaları eklemek `Web.sitemap` dosya. Özellikle, aşağıdaki biçimlendirme sıralama ve disk belleği sonra DataList ve yineleyici eklemeniz `<siteMapNode>`:


[!code-xml[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample1.xml)]

Güncelleştirdikten sonra `Web.sitemap`, bir tarayıcı aracılığıyla öğreticileri Web sitesini görüntülemek için bir dakikanızı ayırın. Soldaki menüde artık düzenleme, ekleme ve öğreticiler silmek için öğeler içerir.


![Site Haritasını giriş özel düğmeler öğretici için artık içerir.](custom-buttons-in-the-datalist-and-repeater-cs/_static/image8.png)

**Şekil 4**: Site Haritası giriş özel düğmeler öğretici için artık içerir.


## <a name="step-2-adding-the-list-of-categories"></a>2. adım: kategori listesi ekleme

Bu öğretici için oluşturmamız Göster ürünleri LinkButton yanı sıra tüm kategorileri listeler yineleyici gerekir, tıklatıldığında madde işaretli listede ilişkili kategori s ürünleri görüntüler. İlk sistemde kategorilerini listeler basit bir yineleyici oluşturmak s olanak tanır. Başlangıç açarak `CustomButtons.aspx` sayfasındaki `CustomButtonsDataListRepeater` klasör. Araç kutusu Tasarımcısı ve kümesi üzerine bir yineleyici sürükleyin kendi `ID` özelliğine `Categories`. Ardından, yeni bir veri kaynağı denetim yineleyici s akıllı etiketten oluşturun. Özellikle, adlı yeni bir ObjectDataSource denetimi oluşturma `CategoriesDataSource` kendi verisinden seçer `CategoriesBLL` s sınıfı `GetCategories()` yöntemi.


[![ObjectDataSource CategoriesBLL sınıfı s GetCategories() yöntemi kullanmak üzere yapılandırma](custom-buttons-in-the-datalist-and-repeater-cs/_static/image10.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image9.png)

**Şekil 5**: ObjectDataSource kullanılacak yapılandırma `CategoriesBLL` s sınıfı `GetCategories()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](custom-buttons-in-the-datalist-and-repeater-cs/_static/image11.png))


Visual Studio oluşturur varsayılan DataList denetimi tersine `ItemTemplate` veri kaynağına göre yineleyici s şablonları el ile tanımlanmış olmalıdır. Ayrıca, yineleyici s şablonları oluşturulmalı ve bildirimli olarak düzenlenmiş (diğer bir deyişle, orada s Düzenle şablon yineleyici s akıllı etiketinde seçeneği).

Sol alt köşedeki kaynağı sekmesinde tıklatıp ekleyin bir `ItemTemplate` kategori s adını görüntüler bir `<h3>` öğesi ve bir paragraf, açıklama etiketi; dahil bir `SeparatorTemplate` yatay çizgi görüntüleyen (`<hr />`) her arasında Kategori. Ayrıca bir LinkButton ile ekleyin, `Text` özelliği Göster ürünler için ayarlanmış. Bu adımları tamamladıktan sonra sayfa s bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample2.aspx)]

Şekil 6 bir tarayıcıdan görüntülendiğinde sayfası gösterilir. Her kategori adı ve açıklaması listelenir. Tıklatıldığında, ürünleri göster düğmesini geri gönderimin neden olur, ancak henüz herhangi bir işlem gerçekleştirmez.


[![Her kategori s ad ve açıklama göster ürünleri LinkButton birlikte görüntülenir](custom-buttons-in-the-datalist-and-repeater-cs/_static/image13.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image12.png)

**Şekil 6**: her kategori s ad ve açıklama görüntülenir, birlikte Göster ürünleri LinkButton ([tam boyutlu görüntüyü görüntülemek için tıklatın](custom-buttons-in-the-datalist-and-repeater-cs/_static/image14.png))


## <a name="step-3-executing-server-side-logic-when-the-show-products-linkbutton-is-clicked"></a>3. adım: Sunucu tarafı mantığı olduğunda göster ürünleri LinkButton yürütme tıklandığında

Düğme, LinkButton veya bir DataList ya da yineleyici şablonu içindeki ImageButton tıklandığında zaman geri gönderimin oluştuğu ve DataList veya yineleyici s `ItemCommand` olay etkinleşir. Ek olarak `ItemCommand` olay, denetim de Yükselt başka bir ve daha belirli olay varsa DataList s düğmesi `CommandName` özelliği (silme, düzenleme, iptal, güncelleştirme ya da seçin) ayrılmış dizeleri birine ayarlanır ancak `ItemCommand` olaydır *her zaman* tetiklenir.

Bir düğme DataList ya da yineleyici içinde tıklatıldığında görmemeleri hangi düğmeyi (birden çok denetim hem bir düzen gibi içindeki düğmeler ve Sil düğmesini olabileceğini durumda) tıklandığını ve belki de bazı ek bilgiler (gibi iletmek ihtiyacımız birincil anahtar değeri, düğme tıklandığını öğesi). Düğme, LinkButton ve ImageButton iki özellik değerleri için geçirilir sağlamak `ItemCommand` olay işleyicisi:

- `CommandName`genellikle her düğme şablonda tanımlamak için kullanılan bir dize
- `CommandArgument`birincil anahtar değeri gibi bazı veri alanının değeri tutmak için yaygın olarak kullanılan

Bu örnekte, LinkButton s ayarlamak `CommandName` özelliğini ShowProducts ve bağ geçerli kayıt s birincil anahtar değerine `CategoryID` için `CommandArgument` özelliği veri bağlama sözdizimini kullanarak `CategoryArgument='<%# Eval("CategoryID") %>'`. Bu iki özellik belirttikten sonra LinkButton s tanımlayıcı sözdizimi aşağıdaki gibi görünmelidir:


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample3.aspx)]

Düğme, geri gönderimin oluştuğu tıklatıldığında ve DataList veya yineleyici s `ItemCommand` olay etkinleşir. S düğmesi olay işleyicisi geçirilen `CommandName` ve `CommandArgument` değerleri.

Yineleyici s için bir olay işleyicisi oluşturun `ItemCommand` olay ve Not ikinci parametre olay işleyiciye geçirilen (adlı `e`). Bu ikinci parametre türünde [ `RepeaterCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeatercommandeventargs.aspx) ve aşağıdaki dört özelliklere sahiptir:

- `CommandArgument`Tıklatılan düğme s değerini `CommandArgument` özelliği
- `CommandName`s düğmesinin değeri `CommandName` özelliği
- `CommandSource`bir başvuru tıklandığını düğmesi denetimi
- `Item`bir başvuru [ `RepeaterItem` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.repeateritem.aspx) tıklandığını düğmesi içerir; yineleyici bağlı her bir kayıt olarak bildirilmiş bir`RepeaterItem`

Seçilen kategori s itibaren `CategoryID` aracılığıyla geçirilen `CommandArgument` özelliği, biz seçili kategorideki ile ilişkili ürün kümesi alabilirsiniz `ItemCommand` olay işleyicisi. Bu ürünler sonra Bulletedlıst denetiminde bağlanabilir `ItemTemplate` (hangi biz ve henüz eklemek için). Tüm kalır, böylece Bulletedlıst eklemektir içinde başvuru `ItemCommand` olay işleyicisi ve ürünleri için adım 4'te üstesinden seçilen kategori kümesi bağlayın.

> [!NOTE]
> S DataList `ItemCommand` olay işleyicisi türünde bir nesne geçirilen [ `DataListCommandEventArgs` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalistcommandeventargs.aspx), aynı dört özellikleri sunan `RepeaterCommandEventArgs` sınıfı.


## <a name="step-4-displaying-the-selected-category-s-products-in-a-bulleted-list"></a>4. adım: Madde işaretli listede seçilen kategori s ürünleri görüntüleme

Seçilen kategori s ürünleri s yineleyici içinde görüntülenebilir `ItemTemplate` herhangi bir sayıda denetimleri kullanarak. Biz, başka bir yineleyici, DataList, bir DropDownList bir GridView ve benzeri iç içe geçmiş ekleyebilirsiniz. Ancak, ürünleri madde işaretli bir liste olarak görüntülenecek istiyoruz olduğundan, Bulletedlıst denetim kullanacağız. Dönme `CustomButtons.aspx` sayfasında s bildirim temelli biçimlendirme, Bulletedlıst denetimine ekleme `ItemTemplate` Göster ürünleri LinkButton sonra. BulletedLists s ayarlamak `ID` için `ProductsInCategory`. Bulletedlıst üzerinden belirtilen verileri alanın değerini görüntüler `DataTextField` bu denetim bağlı, Ayarla ürün bilgisi gerekeceğinden özellik; `DataTextField` özelliğine `ProductName`.


[!code-aspx[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample4.aspx)]

İçinde `ItemCommand` olay işleyicisi kullanarak bu denetim başvuru `e.Item.FindControl("ProductsInCategory")` ve seçili kategoriyle ilişkili ürünleri kümesine bağlayın.


[!code-csharp[Main](custom-buttons-in-the-datalist-and-repeater-cs/samples/sample5.cs)]

İçinde herhangi bir işlem gerçekleştirmeden önce `ItemCommand` olay işleyicisi, onu önce gelen değerini denetlemek akıllıca s `CommandName`. Bu yana `ItemCommand` olay işleyicisi harekete *herhangi* düğmesine tıklandığında, şablonu kullanımda birden çok düğmeleri varsa `CommandName` hangi eylemin yapılacağını keşfedilir değeri. Denetimi `CommandName` İşte moot, biz yalnızca tek bir düğme sahip olduğundan, ancak bu iyi bir forma alışkanlıktır. Ardından, `CategoryID` seçili kategorisini alınır `CommandArgument` özelliği. Şablon Bulletedlıst denetiminde ardından başvurulan ve sonuçlarına bağlı `ProductsBLL` s sınıfı `GetProductsByCategoryID(categoryID)` yöntemi.

Bir DataList içinde düğmeleri gibi kullanılan önceki öğreticileri içinde [, bir genel bakış düzenleme ve silme verileri DataList](../editing-and-deleting-data-through-the-datalist/an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md), biz belirli bir öğe birincil anahtar değerinin belirlenen `DataKeys` koleksiyonu. Bu yaklaşım iyi DataList ile çalışırken, yineleyici sahip olmayan bir `DataKeys` özelliği. Bunun yerine, biz birincil anahtar değeri gibi s düğmesi sağlama için alternatif bir yaklaşım kullanmalısınız `CommandArgument` özelliği veya gizli bir etiket Web denetim şablonu içindeki birincil anahtar değerine atama ve değeriyle geri Okuma`ItemCommand`olay işleyicisi kullanarak `e.Item.FindControl("LabelID")`.

Tamamladıktan sonra `ItemCommand` olay işleyicisi bu sayfasını bir tarayıcıda çıkışı test etmek için bir dakikanızı ayırın. Şekil 7'de görüldüğü gibi göster ürünleri tıklayarak bağlantı geri gönderimin neden olur ve bir Bulletedlıst seçilen kategori için ürünleri görüntüler. Ayrıca, diğer kategorileri göster ürünleri bağlantılar tıklandığında olsa bile bu ürün bilgilerini kalmasını unutmayın.

> [!NOTE]
> Aynı anda yalnızca bir kategori s ürünleri listelenen şekilde da, bu raporun davranışını değiştirmek istiyorsanız, yalnızca Bulletedlıst denetim s ayarlamak `EnableViewState` özelliğine `False`.


[![Bir Bulletedlıst seçili kategorinin ürünleri görüntülemek için kullanılır.](custom-buttons-in-the-datalist-and-repeater-cs/_static/image16.png)](custom-buttons-in-the-datalist-and-repeater-cs/_static/image15.png)

**Şekil 7**: A Bulletedlıst seçili kategorinin ürünleri görüntülemek için kullanılır ([tam boyutlu görüntüyü görüntülemek için tıklatın](custom-buttons-in-the-datalist-and-repeater-cs/_static/image17.png))


## <a name="summary"></a>Özet

DataList ve yineleyici denetimleri herhangi bir sayıda düğmeleri, LinkButtons veya ImageButtons kendi şablonlar içindeki içerebilir. Bu tür düğme tıklatıldığında, geri gönderimin neden ve yükseltmek `ItemCommand` olay. Özel sunucu tarafı eylem tıklattınız bir düğme ile ilişkilendirmek için bir olay işleyicisi oluşturun `ItemCommand` olay. Bu olay işleyicisi önce gelen kontrol `CommandName` hangi düğmenin tıklandığını belirleme için değer. Ek bilgi isteğe bağlı olarak sağlanan s düğmesi `CommandArgument` özelliği.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Dennis Patterson oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Next](custom-buttons-in-the-datalist-and-repeater-vb.md)
