---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: (C#) silerken istemci tarafı doğrulama ekleme | Microsoft Docs
author: rick-anderson
description: Şu ana kadar oluşturduk arabirimlerde, bir kullanıcı yanlışlıkla veri Düzenle düğmesini tıklatın belirttiğinizi olduğunda Sil düğmesine tıklayarak silebilirsiniz. Bu t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: 7ad66e24198a83d32edeb9fddf7a6b648b993fcc
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386568"
---
<a name="adding-client-side-confirmation-when-deleting-c"></a>(C#) silerken istemci tarafı doğrulama ekleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) veya [PDF olarak indirin](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> Şu ana kadar oluşturduk arabirimlerde, bir kullanıcı yanlışlıkla veri Düzenle düğmesini tıklatın belirttiğinizi olduğunda Sil düğmesine tıklayarak silebilirsiniz. Bu öğreticide Sil düğmesine tıklandığında görüntülenen bir istemci tarafı doğrulama iletişim kutusu ekleyeceğiz.


## <a name="introduction"></a>Giriş

Son birkaç öğreticiler üzerinden ediyoruz ve görülen uygulama mimarimiz ObjectDataSource ve veri Web denetimleri ekleme, düzenleme ve silme özelliklerini sağlamak için birlikte kullanma. Silme arabirimleri biz incelenir ve şimdiye kadarki bir oluşturulmuş olan, düğmesine tıklandığında, geri göndermeye neden olur ve ObjectDataSource s çağırır `Delete()` yöntemi. `Delete()` Yöntemi ardından gerçek verme veri erişim katmanı aşağı çağrı yayar iş mantığı katmanı yapılandırılmış olan yöntemi çağırır `DELETE` veritabanına deyimi.

Bu kullanıcı arabirimi GridView, DetailsView veya FormView kontrollerin kayıtları silmek ziyaretçiler etkinleştirse bile, kullanıcı Sil düğmesine tıkladığında onay her türlü eksik. Bir kullanıcı yanlışlıkla tıklarsa Sil düğmesine tıklayın belirttiğinizi, düzenleme, bunun yerine güncelleştirme belirttiğinizi kaydı silinecek. Bu, bu öğreticide önlemeye yardımcı olmak için Sil düğmesine tıklandığında görüntülenen bir istemci tarafı doğrulama iletişim kutusu ekleyeceğiz.

JavaScript `confirm(string)` işlevi olarak Tamam iki düğme ile - birlikte sunulur ve (bkz. Şekil 1) iptal kalıcı bir iletişim kutusu içindeki metni kendi dize giriş parametresini görüntüler. `confirm(string)` İşlevi hangi düğmeye tıklandığında bağlı olarak bir Boole değeri döndürür (`true`, kullanıcı Tamam tıklarsa ve `false` İptal'i tıklatırsanız).


![JavaScript confirm(string) yöntemi kalıcı bir istemci-tarafı Messagebox görüntüler.](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Şekil 1**: JavaScript `confirm(string)` yöntemi, kalıcı, istemci tarafı bir Messagebox görüntüler


Bir değeri varsa, bir form gönderme sırasında `false` form gönderme iptal sonra bir istemci-tarafı olay işleyicisinden döndürülür. Bu özelliği kullanarak, biz Sil düğmesini s istemci-tarafı olabilir `onclick` olay işleyicisi dönüş değeri yapılan bir çağrının `confirm("Are you sure you want to delete this product?")`. Kullanıcı iptali tıklarsa `confirm(string)` false, böylece iptal etmek form gönderme neden döndürür. Hiçbir geri gönderme ile t kazandı, Sil düğmesine tıklandığını ürün silinecek. Ancak, kullanıcı Onayı iletişim kutusunda Tamam tıklarsa, geri gönderme unabated devam edecek ve ürün silinecek. Başvurun [kullanarak JavaScript s `confirm()` denetimi Form Gönderme yönteme](http://www.webreference.com/programming/javascript/confirm/) bu tekniği hakkında daha fazla bilgi için.

Gerekli istemci tarafı komut dosyası ekleme bir CommandField kullanırken daha şablonları kullanıyorsanız biraz farklıdır. Bu nedenle, bu öğreticide size FormView ve GridView örneğe görünecektir.

> [!NOTE]
> İstemci tarafı doğrulama teknikleri kullanarak, bu öğreticide ele alınan olanlar gibi JavaScript destekleyen tarayıcılar ile kullanıcılarınızın ziyaret ettiğiniz ve JavaScript etkin olduğunu varsayar. Bu varsayımlardan ya da belirli bir kullanıcı için doğru değilse, Sil düğmesine tıklanarak (Onayla messagebox görüntüleme değil) bir geri gönderme hemen neden olur.


## <a name="step-1-creating-a-formview-that-supports-deletion"></a>1. adım: silme işlemini destekleyen bir FormView'da oluşturma

Başlamak için bir FormView'da ekleyerek `ConfirmationOnDelete.aspx` sayfasını `EditInsertDelete` klasörü, ürün bilgilerini aracılığıyla geri çeker yeni bir ObjectDataSource bağlama `ProductsBLL` s sınıfı `GetProducts()` yöntemi. ObjectDataSource de yapılandırmanız için `ProductsBLL` s sınıfı `DeleteProduct(productID)` yöntemi ObjectDataSource s eşlenen `Delete()` yöntemi; açılır listede (hiçbiri) ayarlanmış INSERT ve UPDATE sekmeleri emin olun. Son olarak, FormView s akıllı etiketinde sayfalama etkinleştir onay kutusunu işaretleyin.

Bu adımlardan sonra yeni ObjectDataSource s bildirim temelli biçimlendirme, aşağıdaki gibi görünür:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

İyimser eşzamanlılık kullanmayan son örneklerimizde olduğu gibi out ObjectDataSource s temizlemek için bir dakikanızı `OldValuesParameterFormatString` özelliği.

FormView s silme yalnızca destekleyen bir ObjectDataSource denetimine bağlı olduğundan `ItemTemplate` yalnızca Sil düğmesini, New ve Update düğmeleri eksik sunar. FormView s bildirim temelli biçimlendirme ancak gereksiz bir içerir `EditItemTemplate` ve `InsertItemTemplate`, hangi kaldırılabilir. Özelleştirme için bir dakikanızı ayırın `ItemTemplate` kadar olan veri alanları yalnızca bir alt ürünün gösterir. Ben ve araştırmasına s ürün adında göstermek için yapılandırılmış bir `<h3>` , tedarikçi ve kategori adları (birlikte Sil düğmesini) yukarıda başlığı.


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

Bu değişikliklerle ürünler bir ürünü Sil düğmesine tıklayarak silme olanağı ile bir kerede geçiş sağlar tam olarak işlevsel bir web sayfası sahibiz. Şekil 2 ilerlememizin ekran görüntüsü şimdiye kadarki bir tarayıcıdan görüntülendiğinde gösterir.


[![FormView tek bir ürün hakkındaki bilgileri gösterir](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Şekil 2**: FormView gösteren bilgiler hakkında bir tek ürün ([tam boyutlu görüntüyü görmek için tıklatın](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))


## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>2. adım: Sil düğmeleri istemci-tarafı onclick olay confirm(string) işlevi çağırma

FormView ile oluşturulan son adım Sil düğmesini böyle yapılandırmaktır kullanırken, s tıklatıldığında JavaScript ziyaretçisi tarafından `confirm(string)` işlevi çağrılır. Bir düğme, LinkButton veya ImageButton s istemci tarafı için istemci tarafı komut dosyası ekleme `onclick` olay kullanılarak gerçekleştirilebilir `OnClientClick property`, ASP.NET 2.0 için yeni olan. Değerine sahip olacak şekilde istediğinden `confirm(string)` işlevi döndürülen, yalnızca bu özelliği ayarlayın: `return confirm('Are you certain that you want to delete this product?');`

Bu değişiklikten sonra Sil LinkButton s bildirim temelli söz dizimi gibi görünmelidir:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

Tüm var. Bu s için İşte bu kadar! Şekil 3 eylemi bu onayının ekran görüntüsü gösterilmektedir. Sil düğmesine tıklanarak Onayla iletişim kutusunu açar. Kullanıcı iptal tıklarsa, geri gönderme iptal edilir ve ürün silinmez. Ancak, kullanıcı Tamam'ı tıklattığında, geri gönderme devam eder ve ObjectDataSource s `Delete()` yöntemi çağrılır, sonuçlanan veritabanı kaydı siliniyor.

> [!NOTE]
> Dize yöntemlere geçirilen `confirm(string)` JavaScript işlevi kesme (tırnak işaretleri yerine) ayrılmış. JavaScript'te, dizeleri iki karakteri ile sınırlandırılabilir. Böylece sınırlayıcıları dize için yöntemlere geçirilen kesme burada kullandığımız `confirm(string)` bir belirsizlik için kullanılan sınırlayıcılarla İstemediğimiz `OnClientClick` özellik değeri.


[![Şimdi görüntülenen zaman tıklayarak Sil düğmesine bir onay olduğu](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Şekil 3**: bir onay olan şimdi görüntülenen zaman tıklayarak Sil düğmesine ([tam boyutlu görüntüyü görmek için tıklatın](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))


## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>3. adım: bir CommandField Sil düğmesini OnClientClick özelliği yapılandırma

Bir düğme, LinkButton veya ImageButton doğrudan bir şablonda çalışırken, onay iletişim kutusunda yalnızca yapılandırarak kendisiyle ilişkilendirilmiş olabilir, `OnClientClick` JavaScript sonuçlarını döndürmek için özellik `confirm(string)` işlevi. Ancak, - ekleyen bir alan silme düğmelerinin GridView ya da DetailsView - CommandField bulunmayan bir `OnClientClick` bildirimli olarak ayarlama özelliği. Bunun yerine, biz programlama yoluyla uygun GridView veya DetailsView s Sil düğmesini başvurmalıdır `DataBound` olay işleyicisi ve ardından kendi `OnClientClick` özelliği vardır.

> [!NOTE]
> Sil düğmesini s ayarlarken `OnClientClick` uygun özelliğinde `DataBound` olay işleyicisi, biz erişiminiz verileri geçerli kayda bağlı değildi. Bu size "Chai ürünü silmek istediğinizden emin misiniz?" gibi belirli bir kayıtla ilgili ayrıntılar dahil etmek için onay iletisi genişletebilirsiniz anlamına gelir. Bu tür özelleştirme, veri bağlama söz dizimini kullanarak şablonları da mümkündür.


Uygulama ayarı `OnClientClick` özelliği için bir CommandField let s olarak silme button(s) GridView sayfaya ekleyin. FormView kullandığı aynı ObjectDataSource denetimi kullanmak için bu GridView yapılandırın. Ayrıca GridView s BoundFields yalnızca ürün adı, kategori ve tedarikçi içerecek şekilde sınırlayın. Son olarak, GridView s akıllı etiketinde silmeyi etkinleştir onay kutusunu işaretleyin. Bu bir CommandField GridView s ekler `Columns` koleksiyonuyla kendi `ShowDeleteButton` özelliğini `true`.

Bu değişiklikleri yaptıktan sonra GridView s bildirim temelli biçimlendirme aşağıdaki gibi görünmelidir:


[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

GridView s program aracılığıyla erişilebilen tek bir silme LinkButton örnek CommandField içeren `RowDataBound` olay işleyicisi. Başvurulan sonra biz ayarlayabilirsiniz kendi `OnClientClick` özelliği uygun şekilde. İçin bir olay işleyicisi oluşturun `RowDataBound` aşağıdaki kodu kullanarak olay:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Bu olay işleyicisi, veri satırları (Bu Sil düğmesini olacaktır) çalışır ve Sil düğmesine programlı olarak başvuruda bulunarak başlar. Genel şu biçimi kullanın:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType* CommandField tarafından - düğme, LinkButton veya ImageButton kullanılan düğme türü. Varsayılan olarak, CommandField LinkButtons kullanır, ancak bu CommandField s özelleştirilebilir `ButtonType property`. *CommandFieldIndex* CommandField GridView s içinde sıralı dizinidir `Columns` koleksiyonu, oysa *controlIndex* CommandField s içinde Sil düğmesini dizinidir `Controls` koleksiyonu. *ControlIndex* CommandField s düğmesi konumu diğer düğmeleri göreli değer bağlıdır. Örneğin, Sil düğmesini CommandField içinde görüntülenen tek düğmeye ise 0 dizinini kullanın. Ancak, orada s Sil düğmesini önündeki bir Düzenle düğmesi dizinini kullanırsanız, 2. İki denetimi CommandField Sil düğmesini önce eklendiğinden 2 dizinini kullanılan nedeni: Düzenle düğmesine ve LiteralControl Düzenle ve Sil düğmeleri arasında bazı alanı eklemek için kullanılan, s.

Belirli bizim örneğimizin CommandField LinkButtons kullanır ve, en soldaki alan olan bir *commandFieldIndex* 0. Başka bir düğme ancak CommandField Sil düğmesini olduğundan, kullandığımız bir *controlIndex* 0.

Sil düğmesini CommandField başvuran sonra size sonraki geçerli GridView satır için ilişkili ürün hakkında bilgi alın. Son olarak, s Sil düğmesini ayarladık `OnClientClick` s ürün adını içeren uygun JavaScript özelliğine. JavaScript dize yöntemlere geçirilen beri `confirm(string)` işlevi ayrılmış kullanarak kesme biz s ürün adı içinde görüntülenen tüm kesme kaçış gerekir. Tüm kesme s ürün adı ile özellikle atlanır "`\'`".

Özelleştirilmiş onay iletişim kutusunda (bkz. Şekil 4) GridView görüntüler bir Sil düğmesine tıklayarak bu değişikliklerle tamamlayın. Kullanıcı iptal tıklarsa olarak FormView gelen onay messagebox ile geri gönderme, böylece silinemediğini oluşmasını iptal edildi.

> [!NOTE]
> Bu teknik, içinde bir DetailsView CommandField Sil düğmesini programlı olarak erişmek için de kullanılabilir. Bir olay işleyicisi, d, ancak DetailsView için oluşturduğunuz `DataBound` olay DetailsView sahip olduğundan, bir `RowDataBound` olay.


[![GridView s Sil düğmesine tıklanarak özelleştirilmiş onay bir iletişim kutusu görüntüler](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Şekil 4**: GridView s Sil düğmesini tıklatarak bir özelleştirilmiş onay iletişim kutusu görüntüler ([tam boyutlu görüntüyü görmek için tıklatın](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))


## <a name="using-templatefields"></a>TemplateField kullanma

Düğmeleri dizin aracılığıyla erişilmelidir ve elde edilen nesnenin uygun düğme türü (düğme, LinkButton veya ImageButton) dönüştürülmelidir CommandField dezavantajları biridir. "Sihirli sayı" ve sabit kodlu türlerini kullanarak çalışma zamanına kadar bulunan sorunları davet eder. Örneğin, siz veya başka bir geliştirici, belirli bir noktada gelecekte (örneğin, bir düzenleme düğmesi) ya da değişiklikleri CommandField yeni düğmeler ekler, `ButtonType` özelliği, var olan kod hala hatasız derlenir, ancak sayfasını ziyaret ederek bir özel durum neden olabilir ya da beklenmeyen davranışlarla, bağlı olarak, kodunuzun nasıl yazılmıştır ve hangi değişiklikler yapıldı.

GridView ve DetailsView s CommandFields TemplateField dönüştürmek için alternatif bir yoludur. Bu işlem bir TemplateField ile oluşturur bir `ItemTemplate` CommandField içinde her düğme için bir LinkButton (veya düğme veya ImageButton) sahip. Bu düğmeler `OnClientClick` özellikleri atanabilir bildirimli olarak, biz gördüğünüz FormView ile veya programlama yoluyla uygun erişilebilir `DataBound` şu biçimi kullanarak olay işleyicisi:


[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Burada *ControlId* s düğmesi değeri `ID` özelliği. Bu düzen yine de bir sabit kodlanmış tür dönüştürme için gerekirken, düzenin oluşan bir çalışma zamanı hata olmadan değiştirmek izin dizin oluşturma gereksinimini kaldırır.

## <a name="summary"></a>Özet

JavaScript `confirm(string)` işlevidir denetleme form gönderme iş akışı için yaygın olarak kullanılan bir tekniktir. Yürütüldüğünde, işlev Tamam ve iptal iki düğme içeren bir kalıcı, istemci tarafı iletişim kutusu görüntüler. Tamam, kullanıcı tıklarsa `confirm(string)` işlevinin döndürdükleriyle `true`; İptal'i tıklatarak döndürür `false`. Bu işlevsellik, gönderme işlemi sırasında bir olay işleyicisi döndürüyorsa form gönderme iptal etmek için bir tarayıcı s davranışı eşleşmiş `false`, kayıt silme onayı messagebox görüntülemek için kullanılabilir.

`confirm(string)` İşlevi bir düğme Web denetimi s istemci-tarafı ile ilişkilendirilebilir `onclick` s denetim yoluyla olay işleyicisini `OnClientClick` özelliği. Bu öğreticide gördüğümüz gibi bir şablon - ya FormView s şablonlardan birini veya bir TemplateField DetailsView veya GridView - Sil düğmesini ile çalışırken bu özellik bildirimli olarak veya programlama yoluyla ayarlanabilir.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Önceki](implementing-optimistic-concurrency-cs.md)
> [İleri](limiting-data-modification-functionality-based-on-the-user-cs.md)
