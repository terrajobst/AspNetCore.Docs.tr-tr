---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: GridView denetiminde (C#) TemplateFields kullanma | Microsoft Docs
author: rick-anderson
description: "Esneklik sağlamak için bir şablon kullanarak işler TemplateField GridView sunar. Bir şablon statik HTML, Web denetimleri bir karışımını içerebilir ve..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 004f1450937cc6543cb728e01586e3c3529a57d0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="using-templatefields-in-the-gridview-control-c"></a>GridView denetiminde (C#) TemplateFields kullanma
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe) veya [PDF indirin](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> Esneklik sağlamak için bir şablon kullanarak işler TemplateField GridView sunar. Bir şablon bir karışımını içerebilir statik HTML, Web denetimleri ve Veri bağlamada sözdizimi. Bu öğreticide biz TemplateField daha yüksek düzeyde özelleştirme GridView denetimi ile elde etmek için nasıl kullanılacağı anlatılmaktadır.


## <a name="introduction"></a>Giriş

GridView, hangi özelliklerinden gösteren kümesinden oluşan `DataSource` verileri nasıl görüntüleneceğini birlikte işlenmiş çıktıda dahil edilecek. En basit alan bir veri değeri metin olarak görüntüler BoundField türüdür. Diğer alan türleri alternatif HTML öğeleri kullanılarak verileri görüntüleyin. CheckBoxField, örneğin, bir onay kutusu işaretli durumu belirtilen veri alanının değerine bağlı olarak oluşturur; ImageField, görüntü kaynağı belirtilen veri alana dayanan bir görüntü oluşturur. Bağlar ve durumu bir temel alınan verileri alan değerine bağlıdır düğmeleri HyperLinkField ve ButtonField alan türleri kullanılarak oluşturulabilir.

Veriler için bir alternatif görünümü CheckBoxField, ImageField, HyperLinkField ve ButtonField alan türleri izin verirken, bunlar hala biçimlendirme göre oldukça sınırlıdır. Bir ImageField yalnızca tek bir görüntü görüntüleyebilir ancak bir CheckBoxField yalnızca tek bir onay kutusu görüntüleyebilir. Ne bazı metni, bir onay kutusu görüntülemek belirli bir alan gereken *ve* görüntü, farklı veri alan değerlerini tüm göre? Veya, biz Web denetimi onay kutusu, görüntü, köprü veya düğmesi dışında kullanarak verileri görüntülemek isterseniz? Ayrıca, bir tek veri alanına görünümünü BoundField sınırlar. Biz tek bir GridView sütunda iki veya daha fazla veri alan değerlerini göster isterseniz?

Bu esneklik düzeyi uyum sağlayacak şekilde kullanarak işler TemplateField GridView sunan bir *şablon*. Bir şablon bir karışımını içerebilir statik HTML, Web denetimleri ve Veri bağlamada sözdizimi. Ayrıca, farklı durumlar için işleme özelleştirmek için kullanılan şablonları çeşitli TemplateField vardır. Örneğin, `ItemTemplate` varsayılan olarak, her satır için hücre işlemek için kullanılır, ancak `EditItemTemplate` şablonu, veri düzenlerken arabirimini özelleştirmek için kullanılabilir.

Bu öğreticide biz TemplateField daha yüksek düzeyde özelleştirme GridView denetimi ile elde etmek için nasıl kullanılacağı anlatılmaktadır. İçinde [önceki öğretici](custom-formatting-based-upon-data-cs.md) temel alınan verileri kullanarak üzerinde temel biçimini özelleştirmek nasıl gördüğümüz `DataBound` ve `RowDataBound` olay işleyicileri. Temel alınan verileri temel alan biçimini özelleştirmek için bir başka yolu çağırarak bir şablon içindeki yöntemleri biçimlendirme. Biz bu öğreticide Bu teknik göreceğiz.

Bu öğretici için çalışanların listesini görünümünü özelleştirmek için TemplateFields kullanacağız. Özellikle, size tüm çalışanlar listesi, ancak çalışanın görüntüler bir sütun, işe alma tarihlerine Takvim denetimi ve kaç gün bunlar şirkette işe gösteren bir durum sütunu adları ve soyadları.


[![Üç TemplateFields görüntüsünü özelleştirmek için kullanılır](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**Şekil 1**: üç TemplateFields görüntüsünü özelleştirmek için kullanılır ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image3.png))


## <a name="step-1-binding-the-data-to-the-gridview"></a>1. adım: GridView veri bağlama

Raporlama görünümünü özelleştirmek için TemplateFields kullanmanız gereken senaryolarında, yalnızca BoundFields içeren bir GridView denetimi oluşturarak başlatmak kolay bulabilirim ve ardından yeni TemplateFields eklemek veya mevcut BoundFields dönüştürmek için Gerektiği şekilde TemplateFields. Bu nedenle, Bu öğretici Tasarımcısı aracılığıyla sayfasına GridView ekleyerek ve çalışanların listesi döndüren bir ObjectDataSource bağlama başlayalım. Bu adımları GridView BoundFields ile çalışan alanların her biri için oluşturur.

Açık `GridViewTemplateField.aspx` sayfasında ve GridView tasarımcıya araç çubuğuna sürükleyin. GridView'ın akıllı etiketten çağırır yeni bir ObjectDataSource denetimi eklemek seçin `EmployeesBLL` sınıfının `GetEmployees()` yöntemi.


[![Yeni ObjectDataSource GetEmployees() yöntemini çağıran bir denetim ekleme](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**Şekil 2**: yeni bir ObjectDataSource Denetimi o başlatır eklemek `GetEmployees()` yöntemi ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image6.png))


Bu şekilde GridView bağlama otomatik olarak ekleyecek bir BoundField her çalışan özellikleri için: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`, ve `Country`. Bu rapor için şimdi görüntüleme ile rahatsız değil `EmployeeID`, `ReportsTo`, veya `Country` özellikleri. Bu BoundFields kaldırmak için aşağıdakileri yapabilirsiniz:

- Bu iletişim kutusunu Getir için GridView'ın akıllı etiket Düzenle sütunları bağlantısından alanları iletişim kutusunda kullanın. Ardından, alt soldan BoundFields listesinde ve kırmızı X simgesini tıklatın seçin BoundField kaldırmak için düğmesine tıklayın.
- GridView'ın bildirim temelli söz dizimini kaynağı görünümünden el ile düzenleme, silme `<asp:BoundField>` öğesi kaldırmak istediğiniz BoundField için.

Kaldırdığınız sonra `EmployeeID`, `ReportsTo`, ve `Country` BoundFields, GridView'ın biçimlendirme gibi görünmelidir:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

Bir tarayıcıda bizim ilerleme durumunu görüntülemek için bir dakikanızı ayırın. Bu noktada her çalışan ve dört sütun için bir kayıt içeren bir tablo görmelisiniz: çalışanın soyadı, bir ilk adına, kendi başlığı için diğeri işe alınma tarihleri için bir tane.


[![LastName, FirstName, başlık ve HireDate alanları için her çalışan görüntülenir](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**Şekil 3**: `LastName`, `FirstName`, `Title`, ve `HireDate` alanları için her çalışan görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image9.png))


## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>2. adım: tek bir sütun adları ve soyadları görüntüleme

Şu anda, her çalışan ilk ve son adları, ayrı bir sütunda görüntülenir. Bunun yerine tek bir sütun halinde birleştirmek iyi olabilir. Bunu gerçekleştirmek için bir TemplateField kullanmamız gerekiyor. Biz ya da yeni TemplateField ekleyebileceğiniz, gerekli biçimlendirme ve Veri bağlamada sözdizimi ekleyin ve sonra silin `FirstName` ve `LastName` BoundFields veya biz dönüştürebilirsiniz `FirstName` BoundField bir TemplateField içine dahil etmek için TemplateField Düzenle `LastName` değer ve Kaldır'ı `LastName` BoundField.

Her iki yaklaşımın aynı sonucu net, ancak kişisel ı dönüştürme otomatik olarak eklediğinden BoundFields mümkün olduğunda TemplateFields dönüştürme ister bir `ItemTemplate` ve `EditItemTemplate` Web denetimleri ve görünüm taklit etmek üzere veri bağlamasını sözdizimi ve BoundField işlevselliğini. Biz dönüştürme işlemi bazı iş bize gerçekleştirmiş şekilde daha az iş TemplateField ile yapmanız gerekir avantajdır.

Varolan bir BoundField TemplateField'a dönüştürmek için alanları iletişim kutusu getiren GridView'ın akıllı etiket sütunları düzenleme bağlantısını tıklayın. Sol alt köşesinde listesinden dönüştürmek ve ardından sayfanın sağ alt köşesindeki "Dönüştürme bir TemplateField bu alana" bağlantısını tıklatın BoundField seçin.


[![TemplateField alanları iletişim kutusundan bir BoundField dönüştürün](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**Şekil 4**: alanları iletişim kutusundan bir BoundField içine bir TemplateField dönüştürme ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image12.png))


Bir tane Dönüştür `FirstName` bir TemplateField içine BoundField. Bu değişiklikten sonra Tasarımcısı'nda perceptive fark yoktur. TemplateField'a BoundField dönüştürme BoundField Görünüm ve yapısını tutan bir TemplateField oluşturmasıdır. Görsel fark bu noktada Tasarımcısı'nda olan orada rağmen bu dönüştürme işlemi BoundField'ın bildirim temelli söz dizimini - değiştirilmiştir `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />` - aşağıdaki TemplateField sözdizimi ile:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

Gördüğünüz gibi iki şablonları TemplateField oluşur bir `ItemTemplate` bir etikete sahip olan `Text` özelliği değerine ayarlanmış `FirstName` veri alanı ve bir `EditItemTemplate` bir kutusuyla özelliği kontrol `Text` özelliğini de ayarlayın için `FirstName` veri alanı. Veri bağlama sözdizimi - `<%# Bind("fieldName") %>` -belirten veri alanı  *`fieldName`*  belirtilen Web denetimi özelliğe bağlıdır.

Eklemek için `LastName` veri alanı ihtiyacımız başka bir etiket Web denetimi eklemek için bu TemplateField değerine `ItemTemplate` ve bağlama kendi `Text` özelliğine `LastName`. Bu, el ile veya Tasarımcısı aracılığıyla gerçekleştirilebilir. Yalnızca uygun tanımlayıcı sözdizimi yapmak için el ile eklemek `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

Designer üzerinden eklemek için GridView'ın akıllı etiket Şablonları Düzenle bağlantıyı tıklayın. Bu GridView'ın şablon düzenleme arabirimini görüntüler. Bu arabirimin akıllı etiket GridView şablonlarında listesi verilmiştir. Biz yalnızca bir TemplateField bu noktada sahip olduğundan, aşağı açılan listede yalnızca bu şablonları şablonlarıdır `FirstName` ile birlikte TemplateField `EmptyDataTemplate` ve `PagerTemplate`. `EmptyDataTemplate` Şablon belirtilmişse, veri için GridView; bağlı hiç sonuç yoksa GridView'ın çıkış işlemek için kullanılır `PagerTemplate`, belirtilmişse, disk belleği destekleyen GridView için disk belleği arabirimi işlemek için kullanılır.


[![GridView'ın şablonları Tasarımcısı düzenlenebilir](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**Şekil 5**: GridView's şablonları olabilir olması düzenlenebilir aracılığıyla Tasarımcı ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image15.png))


Ayrıca görüntülenecek `LastName` içinde `FirstName` TemplateField etiket denetimi araç sürükleyin `FirstName` TemplateField'ın `ItemTemplate` GridView kullanıcının şablon düzenleme arabirimi.


[![Bir etiket Web denetimi FirstName TemplateField'ın ItemTemplate ekleyin](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**Şekil 6**: bir etiket Web denetimine ekleme `FirstName` TemplateField'ın ItemTemplate ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image18.png))


Bu noktada TemplateField eklenen etiket Web denetimi sahip kendi `Text` özelliği "Etiketine" olarak ayarlanmış. Bu özellik değerine bağlı şekilde değiştirmek için ihtiyacımız `LastName` veri alanı yerine. İçin akıllı etiket denetimin etiket bu tıklatın gerçekleştirmek ve veri bağlamaları Düzenle seçeneğini seçin.


[![Etiketin akıllı etiketten düzenleme veri bağlamaları seçeneği](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**Şekil 7**: Düzenle veri bağlamaları etiketin akıllı etiketten seçeneği ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image21.png))


Bu veri bağlamaları iletişim kutusunu getirir. Buradan, sol taraftaki listeden databinding katılıyor ve aşağı açılan listeden verileri sağ tarafta bağlamak için alan seçmek için özellik seçebilirsiniz. Seçin `Text` sol özelliğinden ve `LastName` alan sağdan ve Tamam'ı tıklatın.


[![Text özelliğini LastName veri alanına bağlayın](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**Şekil 8**: bağlama `Text` özelliğine `LastName` veri alanı ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image24.png))


> [!NOTE]
> Veri bağlamaları iletişim kutusu iki yönlü veri bağlaması yapılıp yapılmayacağını belirtmenize olanak sağlar. Bu işaretlenmemişse bırakırsanız databinding sözdizimi `<%# Eval("LastName")%>` yerine kullanılacak `<%# Bind("LastName")%>`. Her iki yaklaşım Bu öğretici için uygundur. İki yönlü veri bağlaması ekleme ve verileri düzenleme önemli hale gelir. Yalnızca veri görüntülemek için ancak her iki yaklaşım eşit derecede iyi çalışır. Biz, gelecekteki eğitimlerine iki yönlü veri bağlaması ayrıntılı ele alacağız.


Bir tarayıcı aracılığıyla bu sayfayı görüntülemek için bir dakikanızı ayırın. Gördüğünüz gibi GridView hala dört sütun içerir; Ancak, `FirstName` sütunu şimdi listeler *her ikisi de* `FirstName` ve `LastName` veri alan değerleri.


[![Tek bir sütunda FirstName ve LastName değerleri gösterilir](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**Şekil 9**: hem `FirstName` ve `LastName` değerler, tek bir sütunda gösterilir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image27.png))


Bu ilk adımı tamamlamak için kaldırmak `LastName` BoundField ve yeniden adlandırma `FirstName` TemplateField'ın `HeaderText` özelliğini "Name". Bu değişikliklerden sonra GridView'ın bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]


[![İçinde bir Sütu her çalışanın ilk ve son adları görüntülenir](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**Şekil 10**: her çalışanın ilk ve son adları içinde bir Sütu görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image30.png))


## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>3. adım: görüntülenecek Takvim denetimi kullanarak`HiredDate`alan

Bir veri alanı değeri metin GridView olarak görüntüleme bir BoundField kullanmanız yeterlidir. Bazı senaryolarda, ancak veri en iyi yalnızca metin yerine belirli bir Web denetimi kullanarak ifade edilir. Bu tür verilerin görünümünü özelleştirme TemplateFields ile mümkündür. Örneğin, bunun yerine metin olarak çalışan işe alma tarihi görüntüler küçük, bir takvim gösteriyoruz (kullanarak [Takvim denetimi](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) işe alma tarihlerine vurgulanır.

Bunu gerçekleştirmek için dönüştürerek Başlat `HiredDate` bir TemplateField içine BoundField. Sadece GridView'ın akıllı etiket gidin ve alanları iletişim kutusu getiren sütunları Düzenle bağlantısına tıklayın. Seçin `HiredDate` BoundField ve tıklatın "dönüştürme Bu alan TemplateField'a."


[![TemplateField'a HiredDate BoundField Dönüştür](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**Şekil 11**: Dönüştür `HiredDate` BoundField içine bir TemplateField ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image33.png))


2. adımda gördüğümüz gibi bu BoundField içeren bir TemplateField ile değiştirecek bir `ItemTemplate` ve `EditItemTemplate` bir etiket ve metin kutusuna, `Text` özellikleri bağlı `HiredDate` databindingsözdiziminikullanarakdeğer`<%# Bind("HiredDate")%>`.

Metni Takvim denetimi ile değiştirmek için şablon etiketi kaldırarak ve bir Takvim denetimi ekleyerek düzenleyin. Tasarımcısından Şablonları Düzenle GridView'ın akıllı etiketten seçip `HireDate` TemplateField'ın `ItemTemplate` aşağı açılan listeden. Ardından, etiket denetimi silin ve bir Takvim Denetimi Araç Kutusu'ndan şablon düzenleme arabirimine sürükleyin.


[![Bir takvim denetimine ekleme TemplateField'ın ItemTemplate HireDate](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**Şekil 12**: bir takvim denetimine ekleme `HireDate` TemplateField'ın `ItemTemplate` ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image36.png))


Bu noktada GridView her satırda bir Takvim denetimi içerecek kendi `HiredDate` TemplateField. Ancak, çalışan gerçek `HiredDate` değeri ayarlanmazsa herhangi bir yerden her Takvim denetimi tarih ve geçerli ay gösteren için varsayılan neden Takvim denetimi. Bu sorunu gidermek için her çalışanın atamak ihtiyacımız `HiredDate` Takvim denetiminin için [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) ve [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) özellikleri.

Takvim denetim akıllı etiketten veri bağlamaları Düzenle seçin. Ardından, her ikisini bağlamak `SelectedDate` ve `VisibleDate` özelliklerine `HiredDate` veri alanı.


[![SelectedDate ve VisibleDate özellikleri HiredDate veri alanına bağlayın](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**Şekil 13**: bağlama `SelectedDate` ve `VisibleDate` özelliklerine `HiredDate` veri alanı ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image39.png))


> [!NOTE]
> Takvim denetimin seçili tarih mutlaka görünür olması gerekmez. Örneğin, bir takvim 1 Ağustos olabilir<sup>st</sup>, 1999 seçili tarih olarak ancak geçerli ay ve yıl gösterme. Seçilen tarih ve görünen tarih Takvim denetiminin tarafından belirtilen `SelectedDate` ve `VisibleDate` özellikleri. Her iki select çalışanın istiyoruz beri `HiredDate` ve ihtiyacımız için bu özelliklerin her ikisini bağlamak gösterilen emin olun `HireDate` veri alanı.


Sayfasını bir tarayıcıda görüntülerken, takvim, şimdi çalışanın işe alınan tarih ayın gösterir ve belirli bir tarihte seçer.


[![Çalışanın HiredDate, Takvim denetimi gösterilir](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**Şekil 14**: çalışanın `HiredDate` Takvim denetiminde gösterilen ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image42.png))


> [!NOTE]
> Anlatıldığı bugüne kadarki tüm örnekler aykırı, Bu öğretici için yaptığımız *değil* ayarlamak `EnableViewState` özelliğine `false` bu GridView için. Takvim denetimi tarihleri tıklatarak yalnızca tıklattınız tarih Takvim seçili tarihi ayarlama geri gönderme neden olduğundan bu kararı nedenidir. GridView'ın Görünüm durumu devre dışıysa, ancak her geri göndermede GridView'ın veri ayarlanacak Takvim seçilen tarihten neden olan temel kendi veri kaynağına DataSet'e bağlanır *geri* çalışanın için `HireDate`, üzerine yazma kullanıcı tarafından seçilen tarih.


Kullanıcı çalışanın güncelleştirmek mümkün olmadığı için bu öğreticiyi moot tartışma budur `HireDate`. Büyük olasılıkla Takvim denetimi tarihleri seçilemeyen şekilde yapılandırmak en iyi olacaktır. Bazı durumlarda görünüm durumu belirli işlevleri sağlamak için etkinleştirilmelidir ne olursa olsun, bu öğreticide gösterilmiştir.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Adım 4: çalışan gün sayısını gösteren şirket için çalışmıştır

Şu ana kadar TemplateFields iki uygulamalarının anlatıldığı:

- Tek bir sütunda iki veya daha fazla veri alan değerlerini birleştirme ve
- Metin yerine bir Web denetimi kullanarak bir veri alanı değerini belirtme

TemplateFields üçüncü kullanımını GridView'ın hakkında meta veri görüntüleme, veri arka plandaki. Çalışanların işe alma tarihleri gösteren ek olarak, örneğin, biz de iş üzerinde bırakıldı kaç toplam gün görüntüleyen bir sütun isteyebilirsiniz.

Henüz temel alınan veri biçimi, veritabanında depolanan daha farklı web sayfası raporda görüntülenecek gerektiğinde TemplateFields başka bir kullanımını senaryolarda ortaya çıkar. Düşünün `Employees` tablo olan bir `Gender` karakter depolanan alan `M` veya `F` çalışan seks belirtmek için. Bir web sayfasında bu bilgileri görüntülerken, biz cinsiyeti Göster "Erkek" veya "Dişi" olarak, yalnızca "M" veya "F" aksine isteyebilirsiniz.

Bu iki senaryo oluşturarak işlenebilir bir *yöntemi biçimlendirme* ASP.NET sayfa arka plan kodu sınıfında (veya olarak uygulanan bir ayrı Sınıf Kitaplığı'nda bir `static` yöntemi) şablondan çağrılır. Biçimlendirme bir yöntemi, daha önce görülen aynı veri bağlama sözdizimini kullanarak şablonu çağrılır. Biçimlendirme yöntemi herhangi bir sayıda parametre alabilir, ancak bir dize döndürmesi gerekir. Bu döndürülen dize şablonuna eklenen HTML'dir.

Bu kavramı göstermeye şimdi bir çalışan iş üzerinde bırakıldı gün sayısı toplam listeleyen sütun göstermek için öğreticimizi kullanmasıdır. Bu biçimlendirme yöntemi sürer bir `Northwind.EmployeesRow` nesne ve çalışan, bir dize olarak işe gün sayısını döndürür. Bu yöntem ASP.NET sayfa arka plan kodu sınıfına eklenebilir, ancak *gerekir* olarak işaretlenmelidir `protected` veya `public` şablondan erişilebilir olması için.


[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

Bu yana `HiredDate` alan içerebilir `NULL` veritabanı size gereken ilk emin değeri değerleri `NULL` hesaplama işlemine devam etmeden önce. Varsa `HiredDate` değer `NULL`, değilse yalnızca "Bilinmiyor"; dize döndürürüz `NULL`, biz geçerli saati arasındaki farkı işlem ve `HiredDate` değer ve gün sayısını döndürür.

Veri bağlama söz dizimini kullanarak GridView içinde TemplateField gelen çağırmak için ihtiyacımız bu yöntemi kullanmak için. GridView'ın akıllı etiket Düzenle sütunları bağlantıya tıklayarak ve yeni TemplateField ekleme GridView yeni TemplateField ekleyerek başlayın.


[![GridView yeni TemplateField ekleme](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**Şekil 15**: yeni TemplateField GridView ekleyin ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image45.png))


Bu yeni TemplateField's ayarlamak `HeaderText` "İş üzerinde gün" özelliğine ve kendi `ItemStyle`'s `HorizontalAlign` özelliğine `Center`. Çağrılacak `DisplayDaysOnJob` şablondan yöntemi ekleme bir `ItemTemplate` ve aşağıdaki veri bağlama söz dizimini kullanın:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem`döndüren bir `DataRowView` nesne karşılık gelen `DataSource` kayıt bağlı `GridViewRow`. Kendi `Row` özelliği döndürür kesin türü belirtilmiş `Northwind.EmployeesRow`, için geçirilen `DisplayDaysOnJob` yöntemi. Bu veri bağlama sözdizimini doğrudan görünebilir `ItemTemplate` (bildirim temelli aşağıdaki sözdiziminde gösterildiği gibi) veya atanabilir `Text` bir etiket Web denetimi özelliği.

> [!NOTE]
> Alternatif olarak, bilgilerinde yerine bir `EmployeesRow` örneği, biz geçirmeniz yeterlidir `HireDate` kullanarak değer `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Ancak, `Eval` yöntemi döndürür bir `object`, biz değiştirmeniz gerekir böylece bizim `DisplayDaysOnJob` türünde bir giriş parametresi kabul etmek için yöntem imzası `object`, bunun yerine. Biz doğrudan atanamaz `Eval("HireDate")` çağrısı bir `DateTime` çünkü `HireDate` sütununda `Employees` tablo içerebilir `NULL` değerleri. Bu nedenle, biz kabul etmeniz gerekir bir `object` giriş parametresi olarak `DisplayDaysOnJob` yöntemi, bir veritabanına sahip olmadığını görmek için onay `NULL` değeri (gerçekleştirilebilir kullanarak `Convert.IsDBNull(objectToCheck)`) ve buna göre devam edin.


Bu subtleties nedeniyle ı tüm geçirmek için tercih `EmployeesRow` örneği. Sonraki öğreticide kullanmak için daha fazla sığdırma örnek göreceğiz `Eval("columnName")` giriş parametresi bir biçimlendirme yöntemine aktararak söz dizimi.

TemplateField eklendikten sonra aşağıdaki bizim GridView için bildirim temelli söz dizimini gösterir ve `DisplayDaysOnJob` yöntemi çağrılır `ItemTemplate`:


[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

Şekil 16 tamamlanmış öğretici, bir tarayıcı üzerinden görüntülendiğinde gösterir.


[![Sayı çalışan iş üzerinde bırakıldı gün görüntülenir](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**Şekil 16**: The Number of çalışan süredir işinde görüntülenir Days ([tam boyutlu görüntüyü görüntülemek için tıklatın](using-templatefields-in-the-gridview-control-cs/_static/image48.png))


## <a name="summary"></a>Özet

GridView denetiminde TemplateField bir yüksek derecede diğer alan denetimleriyle bulunandan verileri görüntüleme esneklik sağlar. TemplateFields durumlar için ideal burada:

- Birden çok veri alanları bir GridView sütununda görüntülenen gerekir
- Verileri bir Web denetimi düz metin yerine en iyi ifade
- Temel alınan verileri, meta veri görüntüleme gibi veya verileri yeniden biçimlendirme çıkış bağlıdır

Veri görüntüsünü özelleştirme yanı sıra TemplateFields de öğreticileri gelecekte anlatıldığı gibi düzenleme ve veri, ekleme için kullanılan kullanıcı arabirimleri özelleştirmek için kullanılır.

Sonraki iki öğreticileri şablonları, bir DetailsView'da TemplateFields kullanarak bir görünüm başlayarak keşfetme devam edin. Biz için düzenini ve verilerin yapısı, daha fazla esneklik sağlamak için alanları yerine şablonları kullanır FormView etkinleştirmeniz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Dan Jagers oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Önceki](custom-formatting-based-upon-data-cs.md)
[sonraki](using-templatefields-in-the-detailsview-control-cs.md)
