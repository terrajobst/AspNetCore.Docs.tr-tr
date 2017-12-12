---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: "İstemci tarafı doğrulama (C#) silerken ekleme | Microsoft Docs"
author: rick-anderson
description: "Şu ana kadar oluşturduk arabirimlerde bir kullanıcı yanlışlıkla veri Düzenle düğmesini tıklatın yapısındaki, Sil düğmesini tıklatarak silebilirsiniz. Bu t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: c5e8ee76224a48d3132597016b81099bd70a1776
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>İstemci tarafı doğrulama (C#) silerken ekleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) veya [PDF indirin](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> Şu ana kadar oluşturduk arabirimlerde bir kullanıcı yanlışlıkla veri Düzenle düğmesini tıklatın yapısındaki, Sil düğmesini tıklatarak silebilirsiniz. Bu öğreticide Sil düğmesine tıklandığında görüntülenen bir istemci-tarafı onay iletişim kutusunda ekleyeceğiz.


## <a name="introduction"></a>Giriş

Son birkaç öğreticileri üzerinden biz ullanıcı bizim uygulama mimarisi, ObjectDataSource ve Web denetimleri veri ekleme, düzenleme ve silme yetenekleri sağlamak üzere birlikte kullanmak nasıl görülür. Silme arabirimleri biz incelenmesi ve bugüne kadarki silme işlemi oluşan olmuştur, düğmesine tıklandığında, geri gönderimin neden olur ve ObjectDataSource s çağırır `Delete()` yöntemi. `Delete()` Yöntemi çağrısı gerçek verme veri erişim katmanı aşağıya doğru yayar iş mantığı katmanı yapılandırılmış yönteminden sonra çağırır `DELETE` veritabanına deyimi.

Bu kullanıcı arabirimi GridView, DetailsView ya da FormView denetimleri kayıtlarda silmek ziyaretçileri olanak sağlarken Kullanıcı Sil düğmesini tıklattığında onay herhangi bir tür eksik. Bir kullanıcı yanlışlıkla tıklarsa de Sil düğmesini tıklatın anlamına gelir, düzenleme, güncelleştirme yapısındaki kaydı yerine silinir. Bu, bu öğreticide önlemeye yardımcı olmak için Sil düğmesine tıklandığında görüntülenen bir istemci-tarafı onay iletişim kutusunda ekleyeceğiz.

JavaScript `confirm(string)` işlevi, dize giriş parametresinin Tamam iki düğmeleriyle - donatılmıştır ve (bkz: Şekil 1) iptal bir modal iletişim kutusu içindeki metin olarak görüntüler. `confirm(string)` İşlevi hangi düğmeye tıklandığında bağlı olarak bir Boole değeri döndürür (`true`, kullanıcı Tamam tıklarsa ve `false` İptal'i tıklatırsanız).


![JavaScript confirm(string) yöntemi kalıcı bir istemci-tarafı Messagebox görüntüler.](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Şekil 1**: JavaScript `confirm(string)` yöntemi kalıcı, istemci-tarafı Messagebox görüntüler


Bir değeri varsa, bir form gönderme sırasında `false` form gönderme iptal sonra bir istemci-tarafı olay işleyiciden döndürülür. Bu özelliği kullanarak, biz Sil düğmesine s istemci-tarafı olabilir `onclick` olay işleyicisi dönüş çağrısı değeri `confirm("Are you sure you want to delete this product?")`. Kullanıcı, iptal tıklarsa `confirm(string)` false, böylece iptal etmek form gönderme neden döndürür. Hiçbir geri gönderme ile t won olan Sil düğmesini tıklandığını ürün silinir. Ancak, kullanıcı ve onay iletişim kutusunda Tamam tıklarsa geri gönderme unabated devam edecek ve ürün silinir. Başvurun [kullanarak JavaScript s `confirm()` denetim Form Gönderme yönteme](http://www.webreference.com/programming/javascript/confirm/) Bu teknik hakkında daha fazla bilgi için.

Gerekli istemci tarafı komut dosyası ekleme bir CommandField kullanırken daha şablonları kullanıyorsanız biraz farklıdır. Bu nedenle, bu öğreticide biz FormView ve GridView örneğe arar.

> [!NOTE]
> İstemci tarafı doğrulama teknikleri kullanarak, bu öğreticide, ele alınan olanlar gibi JavaScript destekleyen tarayıcılarda ile kullanıcıların ziyaret ettiği ekranları ve JavaScript'in etkin olduğunu varsayar. Bu varsayımları birini belirli bir kullanıcı için doğru değilse, Sil düğmesini tıklatarak (Onayla messagebox görüntüleme değil) geri gönderimin hemen neden olur.


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>1. adım: silme işlemi destekleyen bir FormView oluşturma

FormView için eklemeye başlayın `ConfirmationOnDelete.aspx` sayfasındaki `EditInsertDelete` klasörü, ürün bilgilerini aracılığıyla geri çeker yeni bir ObjectDataSource bağlama `ProductsBLL` s sınıfı `GetProducts()` yöntemi. ObjectDataSource de yapılandırmanız böylece `ProductsBLL` s sınıfı `DeleteProduct(productID)` yöntemi ObjectDataSource s eşlenen `Delete()` yöntemi; açılan listeleri (hiçbiri) ayarlanmış ekleme ve güncelleştirme sekmeleri emin olun. Son olarak, FormView s akıllı etiketinde disk belleği etkinleştir onay kutusunu işaretleyin.

Bu adımlar, yeni ObjectDataSource s bildirim temelli biçimlendirme aşağıdaki gibi görünür:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

İyimser eşzamanlılık kullanmayan son örneklerimizde olduğu gibi ObjectDataSource s temizlemek için bir dakikanızı ayırın `OldValuesParameterFormatString` özelliği.

Yalnızca silme, FormView s destekleyen bir ObjectDataSource denetimine bağlı olduğundan `ItemTemplate` yalnızca silme düğmesi, yeni ve güncelleştirme düğmeleri eksik sunar. FormView s bildirim temelli biçimlendirme ancak, gereksiz bir içeren `EditItemTemplate` ve `InsertItemTemplate`, hangi kaldırılabilir. Özelleştirmek için bir dakikanızı ayırın `ItemTemplate` kadar olan yalnızca bir alt ürünün veri alanlarını gösterir. I ullanıcı benim ürün s adında göstermek için yapılandırılmış bir `<h3>` (birlikte de Sil düğmesini) Tedarikçi ve kategori adları yukarıda başlığı.


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

Bu değişikliklerle Sil düğmesini tıklatarak ürünü silme olanağı bir kerede tek ürünleri üzerinden geçiş yapmak bir kullanıcının tam olarak işlevsel bir web sayfası vardır. Şekil 2 bizim ilerleme ekran görüntüsü bugüne kadarki bir tarayıcıdan görüntülendiğinde gösterir.


[![FormView tek bir ürün bilgilerini gösterir](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Şekil 2**: FormView gösterir bilgi hakkında bir tek ürün ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>2. adım: silme düğmeleri istemci-tarafı onclick olay confirm(string) işlevi çağırma

Oluşturulan FormView ile son adım böyle de Sil düğmesini yapılandırmaktır olduğunda bu s tıklatıldığında JavaScript ziyaretçisi tarafından `confirm(string)` işlevi çağrılır. Bir düğme, LinkButton veya ImageButton s istemci tarafı için istemci tarafı komut dosyası ekleme `onclick` olay kullanım yoluyla gerçekleştirilebilir `OnClientClick property`, ASP.NET 2.0 için yeni olan. Değerine sahip olacak şekilde istediğinden `confirm(string)` işlevi döndürdü, yalnızca bu özelliği ayarlamak:`return confirm('Are you certain that you want to delete this product?');`

Bu değişiklikten sonra Sil LinkButton s Tanımlayıcı Sözdizimi gibi görünmelidir:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

Bu s tüm var olan üzere! Şekil 3 eylemde bu onay ekran görüntüsü gösterilmektedir. Sil düğmesini tıklatarak Onayla iletişim kutusunu açar. Kullanıcı iptal tıklarsa, geri gönderme iptal edilir ve ürün silinmez. Ancak, Tamam kullanıcı varsa, geri gönderme devam eder ve ObjectDataSource s `Delete()` yöntemi çağrıldığında, silinen veritabanını kaydında sonuçlanan.

> [!NOTE]
> Dize içine geçirilen `confirm(string)` JavaScript işlevi kesme (tırnak işaretleri yerine) ayrılmış. JavaScript'te, her iki karakter kullanarak dizeleri sınırlandırılabilir. Böylece içine dize için sınırlayıcılar geçirilen kesme burada kullandığımız `confirm(string)` bir belirsizlik için kullanılan sınırlayıcıları ile tanıtmak değil `OnClientClick` özellik değeri.


[![Şimdi görüntülenen zaman tıklatarak Sil düğmesini bir onaydır](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Şekil 3**: bir onaydır şimdi görüntülenen zaman tıklatarak Sil düğmesini ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>3. adım: bir CommandField OnClientClick özelliği de Sil düğmesini için yapılandırma

Düğme, LinkButton veya ImageButton doğrudan bir şablonda çalışırken, bir onay iletişim kutusunda yalnızca yapılandırarak ile ilişkilendirilebilir kendi `OnClientClick` JavaScript sonuçlarını döndürmek için özellik `confirm(string)` işlevi. Ancak, -, silme düğmeleri alanı bir GridView veya DetailsView ekler - CommandField sahip olmayan bir `OnClientClick` bildirimli olarak ayarlama özelliği. Bunun yerine, sizi program aracılığıyla uygun GridView veya DetailsView s Sil düğmesini başvurmalıdır `DataBound` olay işleyicisi ayarlayın ve ardından kendi `OnClientClick` özelliği vardır.

> [!NOTE]
> Sil düğmesini s ayarlarken `OnClientClick` uygun özelliğinde `DataBound` olay işleyicisi, biz erişiminiz verileri geçerli kayda bağlı. Bu biz "Chai ürün silmek istediğinizden emin misiniz?" gibi belirli kayıt hakkındaki ayrıntıları eklemek için onay iletisi genişletebilirsiniz anlamına gelir. Bu tür özelleştirme databinding sözdizimini kullanarak şablonlarında de mümkündür.


Uygulama ayarı için `OnClientClick` özelliği için bir CommandField let s içinde silme button(s) GridView sayfasına ekleyin. FormView kullanan aynı ObjectDataSource denetimi kullanmak için bu GridView yapılandırın. Ayrıca ürün adı, kategori ve tedarikçi yalnızca dahil etmek BoundFields GridView s sınırlayın. Son olarak, GridView s akıllı etiketinden silme etkinleştir onay kutusunu işaretleyin. Bu bir CommandField GridView s ekler `Columns` koleksiyonuyla kendi `ShowDeleteButton` özelliğini `true`.

Bu değişiklikleri yaptıktan sonra GridView s bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

GridView s'den programlı olarak erişilebilir tek bir silme LinkButton örnek CommandField içeren `RowDataBound` olay işleyicisi. Başvurulan sonra biz ayarlayabilirsiniz kendi `OnClientClick` özelliği uygun şekilde. İçin bir olay işleyicisi oluşturun `RowDataBound` aşağıdaki kodu kullanarak olay:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Bu olay işleyicisi veri satırlarla (olanlar Sil düğmesini olacaktır) çalışır ve program aracılığıyla Sil düğmesini başvurarak başlar. Genel şu biçimi kullanın:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType* CommandField tarafından - düğme, LinkButton veya ImageButton kullanılan düğmesi türüdür. Varsayılan olarak, CommandField LinkButtons kullanır, ancak bu CommandField s özelleştirilebilir `ButtonType property`. *CommandFieldIndex* CommandField GridView s içinde sıra dizini olan `Columns` koleksiyonu, ancak *controlIndex* CommandField s içinde Sil düğmesini dizini `Controls` koleksiyonu. *ControlIndex* CommandField diğer düğmeleri göre düğmesi s konumda değeri bağlıdır. Örneğin, CommandField görüntülenen yalnızca düğmeye Sil düğmesini ise, bir dizin 0 kullanın. Ancak, orada s de Sil düğmesini önündeki bir Düzenle düğmesi dizini kullanırsanız, 2. İki denetimleri de Sil düğmesini önce CommandField tarafından eklendiğinden 2 dizini kullanılır nedeni: Düzenle düğmesi ve LiteralControl düzenleme ve silme düğmeleri arasında bazı alanı eklemek için kullanılan bu s.

Bizim belirli örneğin CommandField LinkButtons kullanır ve, en solundaki alanı olan bir *commandFieldIndex* 0. Başka bir düğme ancak CommandField de Sil düğmesini olduğundan, kullandığımız bir *controlIndex* 0.

CommandField de Sil düğmesini başvuran sonra biz sonraki geçerli GridView satır bağlı ürün hakkında bilgi alın. Son olarak, biz de Sil düğmesini s ayarlamak `OnClientClick` ürün s adını içeren uygun JavaScript özelliğine. JavaScript dize içine geçirilen beri `confirm(string)` işlevi ayrılmış biz ürün s adı içinde görünür kesme kaçış gerekir kesme kullanma. Özellikle, ürün s adındaki tüm kesme ile kaçışlı "`\'`".

Bir özelleştirilmiş onay iletişim kutusu (bkz. Şekil 4) GridView görüntülerindeki Sil düğmesine tıklayarak bu değişikliklerle tamamlayın. Kullanıcı iptal tıklarsa olarak FormView gelen onay messagebox ile geri gönderme, böylece oluşmasını Silmeyi önleme iptal edildi.

> [!NOTE]
> Bu teknik CommandField bir DetailsView de Sil düğmesini programlı olarak erişmek için de kullanılabilir. İçin bir olay işleyicisi, d, ancak DetailsView için oluşturduğunuz `DataBound` olay DetailsView sahip olduğundan, bir `RowDataBound` olay.


[![GridView s Sil düğmesini tıklatarak bir özelleştirilmiş onay iletişim kutusu görüntüler](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Şekil 4**: GridView s Sil düğmesini tıklatarak bir özelleştirilmiş onay iletişim kutusu görüntüler ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))


## <a name="using-templatefields"></a>TemplateFields kullanma

CommandField dezavantajları düğmeleri dizin üzerinden erişilmelidir ve elde edilen nesnenin uygun düğme türü (düğme, LinkButton veya ImageButton) dönüştürülmelidir biridir. "Sihirli sayı" ve sabit kodlanmış türlerini kullanarak çalışma zamanına kadar bulunan sorunları davet eder. Örneğin, siz veya başka bir geliştirici, belirli bir noktada gelecekte (örneğin, bir Düzenle düğmesi) veya değişiklikleri CommandField yeni düğmeler ekler, `ButtonType` özelliği, var olan kodu hatasız derleme, ancak sayfasını ziyaret bir özel durum neden olabilir ya da kodunuzu nasıl yazılmıştır ve hangi değişikliklerin yapıldığı bağlı olarak beklenmeyen davranışlar.

Alternatif bir yaklaşım, TemplateFields GridView ve DetailsView s CommandFields dönüştürmektir. Bu TemplateField ile oluşturacak bir `ItemTemplate` CommandField içinde her düğme için LinkButton (veya düğme veya ImageButton) sahip. Bu düğme `OnClientClick` özellikleri atanabilir bildirimli olarak, biz ile FormView gördüğünüz veya program aracılığıyla uygun erişilebilir olarak `DataBound` şu biçimi kullanarak olay işleyicisi:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Burada *ControlId* s düğmesi değeri `ID` özelliği. Bu desen dönüştürme için hala bir sabit kodlanmış tür gerekirken, oluşan bir çalışma zamanı hata olmadan değiştirmek Düzen izin vermeyi dizin oluşturma, gereksinimini kaldırır.

## <a name="summary"></a>Özet

JavaScript `confirm(string)` kontrol eden form gönderme iş akışı için yaygın olarak kullanılan bir teknik işlevdir. Çalıştırıldığında, işlevi Tamam ve iptal iki düğmeleri içeren bir kalıcı, istemci-tarafı iletişim kutusu görüntüler. Kullanıcı Tamam tıklarsa `confirm(string)` işlev döndürür `true`; İptal'i tıklatarak döndürür `false`. Bu işlevsellik gönderme işlemi sırasında bir olay işleyicisi döndürürse, bir form gönderme iptal etmek için bir tarayıcı s davranış eşleşmiş `false`, bir kaydı silinirken bir onay messagebox görüntülemek için kullanılır.

`confirm(string)` İşlevi bir düğme Web denetimi s istemci-tarafı ile ilişkilendirilebilir `onclick` s denetimi ile olay işleyicisi `OnClientClick` özelliği. Bu öğreticide gördüğümüz gibi bir şablona - da FormView s şablonlarından birini veya bir TemplateField DetailsView ya da GridView - Sil düğmesini ile çalışırken, bu özellik bildirimli olarak veya programlı olarak ayarlanabilir.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Önceki](implementing-optimistic-concurrency-cs.md)
[sonraki](limiting-data-modification-functionality-based-on-the-user-cs.md)
