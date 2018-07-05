---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
title: (VB) GridView denetiminde TemplateField kullanma | Microsoft Docs
author: rick-anderson
description: Esneklik sağlamak için bir şablon kullanarak işler TemplateField GridView sunar. Bir şablon statik HTML Web denetimleri bir karışımını içerebilir ve...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: a92cd6ed-609a-4e40-ad23-004b54afd436
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 845b5b07849420f03e60a9eb243f07641253540f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396514"
---
<a name="using-templatefields-in-the-gridview-control-vb"></a>(VB) GridView denetiminde TemplateField kullanma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_12_VB.exe) veya [PDF olarak indirin](using-templatefields-in-the-gridview-control-vb/_static/datatutorial12vb1.pdf)

> Esneklik sağlamak için bir şablon kullanarak işler TemplateField GridView sunar. Bir şablon bir karışımını içerebilir, statik HTML Web denetimleri ve veri bağlama söz dizimi. Bu öğreticide özelleştirme GridView denetimi ile yüksek düzeyde elde etmek için TemplateField kullanma inceleyeceğiz.


## <a name="introduction"></a>Giriş

GridView hangi özellikleri belirten alanlar kümesi oluşan `DataSource` verilerin nasıl görüntüleneceğini birlikte işlenmiş çıktı dahil edilecek. Basit alan metin olarak veri değeri görüntüler BoundField türüdür. Diğer HTML öğeleri kullanılarak verileri diğer alan türlerini görüntüleyin. CheckBoxField, örneğin, bir onay kutusu işaretli durumu belirtilen veri alanının değerine bağlı olarak işlenir; ImageField, görüntü kaynağı belirtilen veri alanı üzerinde temel alan bir görüntü oluşturur. Bağlar ve durumu bir temel alınan verileri alan değerine bağlıdır düğmeleri HyperLinkField ve ButtonField alan türleri kullanılarak oluşturulabilir.

Yine de biçimlendirme açısından oldukça sınırlı CheckBoxField, ImageField HyperLinkField ve ButtonField alan türleri için alternatif bir veri görünümünü olanak tanırken. Bir ImageField sadece tek bir görüntü görüntüleyebilir ancak bir CheckBoxField yalnızca tek bir onay kutusunu görüntüleyebilirsiniz. Ne bazı metni, bir onay kutusu görüntülemek belirli bir alan gerekiyor *ve* görüntü, farklı veri alan değerlerini tüm bağlı? Ya da durum onay kutusu, görüntü, köprü veya düğme dışındaki Web denetimi kullanarak verileri görüntülemek istedik? Ayrıca, bir tek veri alanına görünümünü BoundField sınırlar. Peki tek bir GridView sütunu iki veya daha fazla veri alanı değerlerini göstermek istedik?

Bu esneklik düzeyi uyum sağlayacak şekilde GridView kullanarak işler TemplateField sunan bir *şablon*. Bir şablon bir karışımını içerebilir, statik HTML Web denetimleri ve veri bağlama söz dizimi. Ayrıca, çeşitli farklı durumlar için işleme özelleştirmek için kullanılan şablonları TemplateField sahiptir. Örneğin, `ItemTemplate` varsayılan olarak, her satır için hücre işlemek için kullanılır ancak `EditItemTemplate` şablonu, veri düzenlerken arabirimini özelleştirmek için kullanılabilir.

Bu öğreticide özelleştirme GridView denetimi ile yüksek düzeyde elde etmek için TemplateField kullanma inceleyeceğiz. İçinde [önceki öğretici](custom-formatting-based-upon-data-vb.md) temel alınan verileri kullanarak temel biçimini özelleştirmek nasıl gördüğümüz `DataBound` ve `RowDataBound` olay işleyicileri. Temel alınan verileri temel alan biçimlendirme özelleştirmek için başka bir yöntem çağırarak yöntemlerinden şablonu içindeki biçimlendirme. Bu öğreticide bu tekniği inceleyeceğiz.

Bu öğretici için çalışanların bir listesini görünümünü özelleştirmek için TemplateField kullanacağız. Özellikle, size tüm çalışanların listesi, ancak çalışanın görüntüler ilk ve son adlarında bir sütun, bir Takvim denetimi ve kaç gün bunlar şirkette işe gösteren bir durum sütunu, işe alım tarihi.


[![Üç TemplateField görüntüsünü özelleştirmek için kullanılır](using-templatefields-in-the-gridview-control-vb/_static/image2.png)](using-templatefields-in-the-gridview-control-vb/_static/image1.png)

**Şekil 1**: üç TemplateField görüntüsünü özelleştirmek için kullanılır ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>1. adım: verileri GridView'a bağlama

Raporlama TemplateField görünümünü özelleştirmek için kullanmanız gereken senaryolarında bunu yalnızca BoundFields içeren bir GridView denetimi oluşturarak başlatmak kolay bulabilirim ve ardından yeni TemplateField eklemek veya mevcut BoundFields için dönüştürmek için Gerektiğinde TemplateField. Bu nedenle, Bu öğretici için sayfa tasarımcıyı aracılığıyla GridView ekleyerek ve çalışanların listesi döndüren bir ObjectDataSource bağlama başlayalım. Bu adımları GridView BoundFields ile çalışan alanların her biri için oluşturur.

Açık `GridViewTemplateField.aspx` sayfasında ve GridView tasarımcı araç kutusundan sürükleyin. GridView'ın akıllı etiketten çağıran yeni bir ObjectDataSource denetimi eklemek seçin `EmployeesBLL` sınıfın `GetEmployees()` yöntemi.


[![GetEmployees() yöntemini çağıran yeni ObjectDataSource denetim ekleme](using-templatefields-in-the-gridview-control-vb/_static/image5.png)](using-templatefields-in-the-gridview-control-vb/_static/image4.png)

**Şekil 2**: yeni bir ObjectDataSource Denetimi, Invoke'lar Ekle `GetEmployees()` yöntemi ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image6.png))


Bu şekilde GridView bağlama otomatik olarak ekleyecek bir BoundField her çalışan özellikleri: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, ve `Country`. Bu rapor için şimdi görüntüleme ile rahatsız değil `EmployeeID`, `ReportsTo`, veya `Country` özellikleri. Bu BoundFields kaldırmak için şunları yapabilirsiniz:

- Bu iletişim kutusunu GridView'ın akıllı etiket sütunları Düzenle bağlantıdan alanları iletişim kutusu öğesini kullanabilirsiniz. Ardından, BoundField kaldırılacağı seçin sol alt BoundFields listelemek ve kırmızı X düğmesini.
- GridView'ın bildirim temelli söz dizimi el ile kaynağı görünümünden Düzenle, Sil `<asp:BoundField>` kaldırmak istediğiniz BoundField için öğesi.

Kaldırılan sonra `EmployeeID`, `ReportsTo`, ve `Country` BoundFields, GridView'ın biçimlendirme gibi görünmelidir:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample1.aspx)]

İlerlememizin bir tarayıcıda görüntülemek için bir dakikanızı ayırın. Bu noktada her çalışan ve dört sütun için bir kayıt içeren bir tablo görürsünüz: bir çalışanın soyadı, kendi ad için kendi başlık için bir tane ve bir işe alınma tarihleri.


[![Soyadı, FirstName, başlık ve HireDate alanları her çalışanın görüntülenir](using-templatefields-in-the-gridview-control-vb/_static/image8.png)](using-templatefields-in-the-gridview-control-vb/_static/image7.png)

**Şekil 3**: `LastName`, `FirstName`, `Title`, ve `HireDate` alanları her çalışanın görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>2. adım: tek bir sütunu ilk ve son adlarını görüntüleme

Şu anda, her çalışan ilk ve son adları, ayrı bir sütunda görüntülenir. Bunun yerine tek bir sütun halinde birleştirmek kullanışlı olabilir. Bunu gerçekleştirmek için size bir TemplateField kullanmanız gerekir. Biz ya da yeni TemplateField ekleyebilir, gerekli biçimlendirme ve veri bağlama söz dizimi ekleyin ve ardından silin `FirstName` ve `LastName` BoundFields ya da biz dönüştürebilirsiniz `FirstName` BoundField bir TemplateField içine eklenecek TemplateField Düzenle `LastName` değeri ve Kaldır'ı `LastName` BoundField.

Her iki yaklaşım aynı sonucu net, ancak kişisel dönüştürme otomatik olarak eklediğinden, mümkün olduğunda TemplateField BoundFields dönüştürme istiyorum bir `ItemTemplate` ve `EditItemTemplate` Web denetimleri ve görünümünü taklit etmek için veri bağlama söz dizimi ve BoundField işlevselliği. Biz dönüştürme işlemi işinin bir kısmını bizim için gerçekleştirilen şekilde daha az çalışma ile TemplateField yapmanız gerektiğini avantajdır.

Mevcut bir BoundField bir TemplateField dönüştürmek için alanları iletişim kutusu getirme GridView'ın akıllı etiketinde sütunları Düzenle bağlantısına tıklayın. Sol alt köşesine listeden dönüştürmek ve ardından sağ alt köşesinde "Dönüştürme bu alana bir TemplateField" bağlantıyı BoundField seçin.


[![Bir TemplateField alanları iletişim kutusundan bir BoundField dönüştürün](using-templatefields-in-the-gridview-control-vb/_static/image11.png)](using-templatefields-in-the-gridview-control-vb/_static/image10.png)

**Şekil 4**: alanları iletişim kutusundan bir BoundField içine bir TemplateField dönüştürme ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image12.png))


Devam edip dönüştürme `FirstName` içine bir TemplateField BoundField. Bu değişiklikten sonra tasarımcıda perceptive fark yoktur. Bir TemplateField BoundField dönüştürme BoundField Görünüm ve yapısını tutan bir TemplateField oluşturduğundan budur. Burada visual fark bu noktada Tasarımcısı'nda olan rağmen bu dönüştürme işlemini BoundField'ın bildirim temelli söz dizimi - değiştirilmiştir `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` - aşağıdaki TemplateField söz dizimi ile:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample2.aspx)]

Gördüğünüz gibi iki şablonları TemplateField oluşur bir `ItemTemplate` bir etikete sahip olan `Text` özelliği değeri olarak ayarlanır `FirstName` veri alan ve bir `EditItemTemplate` ayarlanmış olan TextBox denetimi `Text` özelliğini de ayarlayın için `FirstName` veri alanı. Veri bağlama söz dizimi - `<%# Bind("fieldName") %>` -bildiren veri alanı *`fieldName`* belirtilen Web denetimi özelliğine bağlıdır.

Eklenecek `LastName` veri alanı ihtiyacımız başka bir etiket Web denetimi eklemek için bu TemplateField değerine `ItemTemplate` ve bağlama kendi `Text` özelliğini `LastName`. Bu, el ile veya Tasarımcısı aracılığıyla gerçekleştirilebilir. Bunu yapmanın el ile uygun olan bildirim temelli söz dizimi eklemeniz yeterlidir `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample3.aspx)]

Tasarımcı eklemek için GridView'ın akıllı etiketinde Şablonları Düzenle bağlantısına tıklayın. GridView'ın şablon düzenleme arabirimi bu görüntüler. Bu arabirimin akıllı etiket GridView şablonlarında listesi verilmiştir. Biz yalnızca bir TemplateField bu noktada olduğundan, bu şablonlar için aşağı açılan listede yalnızca şablonlar olan `FirstName` ile birlikte TemplateField `EmptyDataTemplate` ve `PagerTemplate`. `EmptyDataTemplate` Şablonu, belirtilmişse, veri GridView'a; hiç sonuç yoksa GridView'ın çıktı işlemek için kullanılır `PagerTemplate`, belirtilmişse, disk belleği destekleyen bir GridView için disk belleği arabirimi işlemek için kullanılır.


[![GridView'ın şablonları Tasarımcısı yoluyla düzenlenebilir](using-templatefields-in-the-gridview-control-vb/_static/image14.png)](using-templatefields-in-the-gridview-control-vb/_static/image13.png)

**Şekil 5**: GridView'ın şablonları olabilir olması düzenlenen aracılığıyla Tasarımcısı ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image15.png))


Ayrıca görüntülenecek `LastName` içinde `FirstName` TemplateField etiket denetimi araç kutusundan sürükleyin `FirstName` TemplateField'ın `ItemTemplate` GridView kullanıcının şablon düzenleme arabirimi.


[![FirstName TemplateField'ın ItemTemplate için bir etiket Web denetimi ekleme](using-templatefields-in-the-gridview-control-vb/_static/image17.png)](using-templatefields-in-the-gridview-control-vb/_static/image16.png)

**Şekil 6**: bir etiketin Web denetimine ekleme `FirstName` TemplateField'ın ItemTemplate ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image18.png))


Bu noktada TemplateField için eklediğiniz etiket Web denetimi olan kendi `Text` özelliği "Etiketine" olarak ayarlanmış. Bu özellik değerine bağlı şekilde değiştirmek için ihtiyacımız `LastName` veri alanı yerine. İçin akıllı etiket denetiminin etiket üzerinde tıklanabilir gerçekleştirmek ve veri bağlamaları Düzenle seçeneğini belirleyin.


[![Etiketin akıllı etiketi Düzenle DataBindings seçeneği](using-templatefields-in-the-gridview-control-vb/_static/image20.png)](using-templatefields-in-the-gridview-control-vb/_static/image19.png)

**Şekil 7**: etiketin akıllı etiketi Düzenle DataBindings seçeneğini belirleyin ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image21.png))


Bu veri bağlamaları iletişim kutusunu getirir. Buradan, sol taraftaki listede veri bağlama katılmak ve sağdaki aşağı açılan listeden veri bağlamak için bir alan seçmek için özellik seçebilirsiniz. Seçin `Text` sol özelliğinden ve `LastName` sağ taraftan alanına girin ve Tamam'a tıklayın.


[![Metin özelliği LastName veri alanına bağlama](using-templatefields-in-the-gridview-control-vb/_static/image23.png)](using-templatefields-in-the-gridview-control-vb/_static/image22.png)

**Şekil 8**: bağlama `Text` özelliğini `LastName` veri alanı ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image24.png))


> [!NOTE]
> Vlastnosti DataBindings iletişim kutusu yapılıp yapılmayacağını çift yönlü veri bağlama belirtmenize olanak sağlar. Bu işaretli bırakırsanız veri bağlama söz dizimi `<%# Eval("LastName")%>` yerine kullanılacak `<%# Bind("LastName")%>`. Her iki yöntemle Bu öğretici için uygundur. İki yönlü veri bağlama ekleme ve veri düzenleme önemli hale gelir. Yalnızca verileri görüntülemek için ancak her iki yöntemle eşit derecede iyi çalışır. Sonraki öğreticilerde çift yönlü veri bağlama ayrıntılı bir şekilde açıklayacağız.


Bir tarayıcı aracılığıyla bu sayfayı görüntülemek için bir dakikanızı ayırın. Gördüğünüz gibi GridView hala dört sütun içerir; Ancak, `FirstName` sütun şimdi listeler *hem* `FirstName` ve `LastName` veri alan değerleri.


[![FirstName ve LastName değerleri tek bir sütunda gösterilir](using-templatefields-in-the-gridview-control-vb/_static/image26.png)](using-templatefields-in-the-gridview-control-vb/_static/image25.png)

**Şekil 9**: hem `FirstName` ve `LastName` değerleri tek bir sütunda gösterilir ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image27.png))


Bu ilk adımı tamamlamak için kaldırmak `LastName` BoundField ve yeniden adlandırma `FirstName` TemplateField'ın `HeaderText` "Name" özelliği. GridView'ın bildirim temelli biçimlendirme bu değişikliklerden sonra aşağıdaki gibi görünmelidir:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample4.aspx)]


[![Her çalışanın ilk ve son adları bir sütunda görüntülenir](using-templatefields-in-the-gridview-control-vb/_static/image29.png)](using-templatefields-in-the-gridview-control-vb/_static/image28.png)

**Şekil 10**: her çalışanın ilk ve son adları bir sütunda görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>3. adım: ekran için takvim denetimini kullanarak`HiredDate`alan

Veri alanı değeri GridView metin olarak görüntüleyen bir BoundField kullanmanız yeterlidir. Belirli senaryolar için ancak veri en iyi yalnızca metin yerine bir özel Web denetimi kullanarak ifade edilir. Bu tür verilerin görünümünü özelleştirme TemplateField ile mümkündür. Örneğin, bunun yerine metin olarak çalışan işe alma tarihi görüntüleme daha bir takvim göstereceğiz (kullanarak [Takvim denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) ile işe alım tarihlerine vurgulanır.

Bunu yapmak için başlangıç dönüştürerek `HiredDate` içine bir TemplateField BoundField. GridView'ın akıllı etiket için Git yeterlidir alanlar iletişim kutusunu getirme sütunları Düzenle bağlantısına tıklayın. Seçin `HiredDate` BoundField tıklayın ve "dönüştürmek Bu alan bir TemplateField."


[![Bir TemplateField HiredDate BoundField Dönüştür](using-templatefields-in-the-gridview-control-vb/_static/image32.png)](using-templatefields-in-the-gridview-control-vb/_static/image31.png)

**Şekil 11**: dönüştürme `HiredDate` BoundField içine bir TemplateField ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image33.png))


2. adımda gördüğümüz gibi bu BoundField içeren bir TemplateField ile değiştirecek bir `ItemTemplate` ve `EditItemTemplate` bir etiket ve metin kutusu olan `Text` için ilişkili özellikleri `HiredDate` veribağlamasözdiziminikullanarakdeğer`<%# Bind("HiredDate")%>`.

Metni bir takvimin denetimle değiştirmek için şablonu etiketi kaldırarak ve bir Takvim denetimi ekleyerek düzenleyin. Tasarımcıdan Şablonları Düzenle GridView'ın akıllı etiketi seçip `HireDate` TemplateField'ın `ItemTemplate` aşağı açılan listeden. Ardından, etiket denetimini silin ve şablon düzenleme arabirimine araç kutusundan bir Takvim denetimi sürükleyin.


[![Bir takvim denetimine ekleme TemplateField'ın ItemTemplate HireDate](using-templatefields-in-the-gridview-control-vb/_static/image35.png)](using-templatefields-in-the-gridview-control-vb/_static/image34.png)

**Şekil 12**: bir takvim denetimine ekleme `HireDate` TemplateField'ın `ItemTemplate` ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image36.png))


Bu noktada bir Takvim denetiminde her GridView satır içerir, `HiredDate` TemplateField. Ancak, çalışan gerçek `HiredDate` ayarlanmamışsa her yerden her Takvim denetimi varsayılan tarih ve geçerli ay gösteren için neden Takvim denetimi. Bu sorunu gidermek için her çalışanın atamak ihtiyacımız `HiredDate` Takvim denetiminin [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) ve [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) özellikleri.

Takvim denetim akıllı etiketten veri bağlamaları Düzenle'ı seçin. Ardından, her ikisi de bağlama `SelectedDate` ve `VisibleDate` özelliklerine `HiredDate` veri alanı.


[![SelectedDate ve VisibleDate özellikleri HiredDate veri alanına bağlama](using-templatefields-in-the-gridview-control-vb/_static/image38.png)](using-templatefields-in-the-gridview-control-vb/_static/image37.png)

**Şekil 13**: bağlama `SelectedDate` ve `VisibleDate` özelliklerine `HiredDate` veri alanı ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image39.png))


> [!NOTE]
> Seçilen tarih Takvim denetiminin mutlaka görünür olması gerekmez. Örneğin, bir takvim 1 Ağustos olabilir<sup>st</sup>, 1999 seçilen tarih, ancak geçerli ay ve yıl gösteriliyor. Takvim denetim tarafından görünür tarih ve seçilen tarihten belirtilen `SelectedDate` ve `VisibleDate` özellikleri. Çalışanın hem seçmek için istediğimiz beri `HiredDate` ve ihtiyacımız bu özelliklerin her ikisi de bağlamak gösterilen emin olun `HireDate` veri alanı.


Sayfasını bir tarayıcıda görüntülerken, takvim, artık çalışan işe alındığı tarih ayı gösterir ve belirli bir tarihte seçer.


[![Çalışanın HiredDate Takvim denetimi gösterilir](using-templatefields-in-the-gridview-control-vb/_static/image41.png)](using-templatefields-in-the-gridview-control-vb/_static/image40.png)

**Şekil 14**: çalışanın `HiredDate` Takvim denetimi gösterilir ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image42.png))


> [!NOTE]
> Gördük şimdiye kadarki tüm örnekler aykırı de Bu öğretici için yaptığımız *değil* ayarlamak `EnableViewState` özelliğini `False` bu GridView için. Takvim denetimi tarihleri tıklatarak takvimin seçilen tarihten yalnızca tıkladı tarihe ayarlama geri göndermenin neden olduğu için bu kararı nedenidir. GridView'ın görünüm durumu devre dışı bırakılırsa, ancak her geri göndermede GridView'ın veri ayarlamak, seçilen tarih takvimin neden olur, temel alınan veri kaynağına DataSet'e *geri* çalışanın için `HireDate`, üzerine yazma kullanıcı tarafından seçmiş tarih.


Kullanıcı çalışanın güncelleştiremezsiniz olmadığından Bu öğretici için moot tartışma budur `HireDate`. Büyük olasılıkla Takvim denetimi tarihleri seçilemeyen şekilde yapılandırmak en iyi olacaktır. Ne olursa olsun, Bu öğretici, bazı durumlarda görünüm durumu bazı işlevleri sağlamak için etkinleştirilmesi gerektiğini gösterir.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>4. adım: çalışan gün sayısını gösteren şirket için çalıştı

Şu ana kadar TemplateField iki uygulamaları gördük:

- İki veya daha fazla veri alanı değerlerini tek bir sütunda birleştirerek ve
- Metin yerine Web denetimi kullanarak bir veri alanı değerini belirtme

TemplateField kullanımını üçüncü GridView'ın hakkında meta veri görüntüleme temel alınan veriler. Çalışanların işe alım tarihleri gösteren ek olarak, örneğin, biz de işin gittiklerini kaç toplam gün görüntüleyen bir sütun sahip olmak isteyebilirsiniz.

Henüz temel alınan verileri biçiminde, veritabanında depolanan daha farklı web sayfası raporda görüntülenen gerektiğinde TemplateField başka bir kullanımını senaryolar ortaya çıkar. Imagine `Employees` tablonuz bir `Gender` karakter depolanan alan `M` veya `F` çalışanın seks belirtmek için. Bir web sayfasında bu bilgileri görüntülerken, biz "Erkek" veya "Kadın", "M" veya "F" ın aksine cinsiyet göstermek isteyebilirsiniz.

Bu senaryoların her ikisini de oluşturma tarafından işlenebilen bir *biçimlendirme yöntemi* ASP.NET sayfa arka plan kod sınıfı içinde (veya olarak uygulanan bir ayrı Sınıf Kitaplığı'nda bir `Shared` yöntemi) şablondan çağrılır. Böyle bir biçimlendirme yöntemi, daha önce görülen aynı veri bağlama söz dizimini kullanarak bir şablondan çağrılır. Biçimlendirme yöntemi, herhangi bir sayıda parametre alabilir, ancak bir dize döndürmelidir. Bu döndürülen dizeyi şablona eklenen HTML'dir.

Bu kavramı anlamak için şimdi bir çalışan iş üzerinde olan toplam sayısını listeler bir sütun göstermek için öğreticimize kullanmasıdır. Bu biçimlendirme yöntemi sürecek bir `Northwind.EmployeesRow` nesne ve çalışan, bir dize olarak işe gün sayısını döndürür. Bu yöntem ASP.NET sayfa arka plan kod sınıfı için eklenebilir, ancak *gerekir* olarak işaretlenmiş `Protected` veya `Public` şablondan erişilebilir olması için.


[!code-vb[Main](using-templatefields-in-the-gridview-control-vb/samples/sample5.vb)]

Bu yana `HiredDate` alanı içerebilir `NULL` veritabanı gereken ilk emin oluruz değerin olmadığını değerleri `NULL` hesaplama devam etmeden önce. Varsa `HiredDate` değer `NULL`, değilse biz yalnızca dize "Bilinmeyen"; döndürecek `NULL`, biz geçerli saati arasındaki farkı hesaplamak ve `HiredDate` değeri ve gün sayısını döndürür.

Bu yöntemi kullanmak için öğesinden bir TemplateField veri bağlama söz dizimini kullanarak GridView içinde çağırmak ihtiyacımız var. GridView'ın akıllı etiket sütunları Düzenle bağlantısına tıklayın ve yeni TemplateField ekleyerek GridView'a yeni TemplateField ekleyerek başlayın.


[![Yeni bir TemplateField GridView'a Ekle](using-templatefields-in-the-gridview-control-vb/_static/image44.png)](using-templatefields-in-the-gridview-control-vb/_static/image43.png)

**Şekil 15**: yeni TemplateField GridView'a ekleyin ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image45.png))


Bu yeni TemplateField's ayarlamak `HeaderText` "İşi üzerinde gün" özelliğini ve kendi `ItemStyle`'s `HorizontalAlign` özelliğini `Center`. Çağrılacak `DisplayDaysOnJob` şablondan bir yöntem ekleyin bir `ItemTemplate` ve aşağıdaki veri bağlama söz dizimini kullanın:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample6.aspx)]

`Container.DataItem` döndürür bir `DataRowView` nesne karşılık gelen `DataSource` kayıt bağlı `GridViewRow`. Kendi `Row` özelliği döndürür türü kesin belirlenmiş `Northwind.EmployeesRow`, için geçirilen `DisplayDaysOnJob` yöntemi. Bu veri bağlama söz dizimi doğrudan görünebilir `ItemTemplate` (bildirim temelli aşağıdaki sözdiziminde gösterildiği gibi) veya atanabilir `Text` etiket Web denetimi özelliği.

> [!NOTE]
> Alternatif olarak, geçirmek yerine bir `EmployeesRow` örneği, biz geçirmeniz yeterlidir `HireDate` kullanarak değer `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Ancak, `Eval` yöntemi döndürür bir `Object`, biz değiştirme bu nedenle bizim `DisplayDaysOnJob` yöntem imzası türü giriş parametresi kabul etmek için `Object`, bunun yerine. Biz körüne atanamaz `Eval("HireDate")` çağrısı bir `DateTime` çünkü `HireDate` sütununda `Employees` tablo içerebilir `NULL` değerleri. Bu nedenle, kabul etmek ihtiyacımız bir `Object` giriş parametresi olarak `DisplayDaysOnJob` yöntemi, bir veritabanına sahip olmadığını kontrol edin `NULL` değeri (gerçekleştirilebilir kullanarak `Convert.IsDBNull(objectToCheck)`) ve buna göre devam edin.


Bu ıot'nin nedeniyle ben tüm geçirilecek bıraktınız `EmployeesRow` örneği. Sonraki öğreticide kullanmak için daha fazla sığdırma örnek görüyoruz `Eval("columnName")` giriş parametresi biçimlendirme bir yönteme geçirmek için söz dizimi.

TemplateField eklendikten sonra aşağıdaki bildirim temelli söz dizimi için sunduğumuz GridView gösterir ve `DisplayDaysOnJob` yöntemi çağrılır `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-vb/samples/sample7.aspx)]

Şekil 16 öğretici tamamlanmış bir tarayıcıdan görüntülendiğinde gösterir.


[![Sayı çalışan işin olmuştur gün görüntülenir](using-templatefields-in-the-gridview-control-vb/_static/image47.png)](using-templatefields-in-the-gridview-control-vb/_static/image46.png)

**Şekil 16**: The Number of çalışan rolünüzün işinde görüntülenir Days ([tam boyutlu görüntüyü görmek için tıklatın](using-templatefields-in-the-gridview-control-vb/_static/image48.png))


## <a name="summary"></a>Özet

GridView denetiminde TemplateField daha ileri düzeyde bir diğer alan denetimleriyle kullanılabilir alandan verileri görüntüleme esneklik sağlar. TemplateField durumlar için ideal burada:

- Birden çok veri alanları bir GridView sütunu görüntülenmesi gerekir
- Veri en iyi şekilde düz metin yerine bir Web denetimi kullanılarak ifade edilir
- Temel alınan verileri, meta veri görüntüleme gibi veya verileri yeniden biçimlendirme çıkış bağlıdır

Veri görünümünü özelleştirme yanı sıra TemplateField de gelecekte öğreticiler anlatıldığı gibi veri ekleme ve düzenleme için kullanılan kullanıcı arabirimleri özelleştirmek için kullanılır.

Sonraki iki öğreticiler, şablonlar, içinde bir DetailsView TemplateField kullanma göz başlayarak'ı keşfetmeye devam edin. Biz için FormView şablonları, Düzen ve veri yapısını daha fazla esneklik sağlamak için yerine alanları kullanan etkinleştirmeniz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Dan Jagers oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](custom-formatting-based-upon-data-vb.md)
> [İleri](using-templatefields-in-the-detailsview-control-vb.md)
