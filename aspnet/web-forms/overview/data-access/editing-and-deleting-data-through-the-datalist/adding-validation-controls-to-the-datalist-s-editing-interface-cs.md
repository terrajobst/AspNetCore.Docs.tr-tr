---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
title: DataList için doğrulama denetimleri ekleme (C#) arabirimi düzenleme kullanıcının | Microsoft Docs
author: rick-anderson
description: Bu öğreticide daha iyi bir düzenleme kullanıcı int sağlayabilmek için DataList'in EditItemTemplate doğrulama denetimleri eklemek için ne kadar kolay olduğunu görüyoruz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 3ecc21c5-da0e-40ab-abb4-fac1e47398ad
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 23b7a0a4fd5786d5bbc2905022a76dfc048d44bf
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389975"
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-c"></a>DataList'in düzenleme arabirimine (C#) doğrulama denetimleri ekleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_CS.exe) veya [PDF olarak indirin](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/datatutorial39cs1.pdf)

> Bu öğreticide daha iyi bir düzenleme kullanıcı arabirimi sağlayabilmek için DataList'in EditItemTemplate doğrulama denetimleri eklemek için ne kadar kolay olduğunu göreceğiz.


## <a name="introduction"></a>Giriş

Ürün adı eksik ya da bir özel durum negatif fiyat sonuçları gibi geçersiz kullanıcı girişi olsa bile öğreticiler şimdiye kadarki düzenleme DataList herhangi bir etkin kullanıcı girdisi doğrulama arabirimleri düzenleme belirleyebilen eklemediniz. İçinde [önceki öğretici](handling-bll-and-dal-level-exceptions-cs.md) biz özel durum işleme DataList s için kod ekleme incelenirken `UpdateCommand` catch ve düzgün bir şekilde ortaya çıktı özel durumları hakkında bilgi görüntülemek için olay işleyicisi. İdeal olarak, ancak düzenleme arabirimi kullanıcı geçersiz tür verilerin ilk başta girmesini önlemek için doğrulama denetimleri dahildir.

Bu öğreticide DataList s doğrulama denetimleri eklemek için ne kadar kolay olduğunu göreceğiz `EditItemTemplate` daha iyi bir düzenleme kullanıcı arabirimi sağlamak için. Özellikle, bu öğreticide, önceki öğreticide oluşturulan örnek alır ve uygun doğrulama eklemek için düzenleme arabirimi artırmaktadır.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-csmd"></a>1. adım: örnek çoğaltma[BLL ve DAL düzeyi özel durumları işleme](handling-bll-and-dal-level-exceptions-cs.md)

İçinde [işleme BLL ve DAL düzeyi özel durumları](handling-bll-and-dal-level-exceptions-cs.md) öğretici adları ve iki sütunlu, düzenlenebilir bir DataList ürünleri fiyatları listelenen bir sayfa oluşturduk. Hedefimiz Bu öğretici için doğrulama denetimleri içerecek şekilde DataList s düzenleme arabirimi genişletmek sağlamaktır. Özellikle, bizim Doğrulama mantığı olur:

- Ürün s adı sağlanmasını gerektirir
- Fiyat için girilen değer geçerli bir para birimi biçiminde olduğundan emin olun
- Fiyat değerinden büyük veya sıfır, negatif bir yana için girilen değer emin `UnitPrice` değer geçersizdir

Biz sırasında doğrulama eklemek için önceki örnekte deneyimlerinizi göz atmadan önce ilk örnekte çoğaltmak ihtiyacımız `ErrorHandling.aspx` sayfasını `EditDeleteDataList` sayfanın Bu öğretici için bir klasöre `UIValidation.aspx`. İhtiyacımız hem de kopyalamak için bunu başarmanın `ErrorHandling.aspx` sayfasında s bildirim temelli biçimlendirme ve kaynak kodu. İlk bildirim temelli biçimlendirme, aşağıdaki adımları uygulayarak kopyalayın:

1. Açık `ErrorHandling.aspx` Visual Studio'daki sayfası
2. Sayfa s bildirim temelli biçimlendirme (sayfanın alt kısmındaki kaynağı düğmesini tıklatın) gidin
3. Metni kopyalayın `<asp:Content>` ve `</asp:Content>` gösterildiği Şekil 1 olarak etiketleri (satırlar 3 ile 32 arasında).


[![Metin içindeki kopyalama &lt;asp: Content&gt; denetimi](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image1.png)

**Şekil 1**: metin içinde kopyalama `<asp:Content>` denetimi ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image3.png))


1. Açık `UIValidation.aspx` sayfası
2. Sayfa s bildirim temelli işaretlemede gidin
3. İçindeki metni yapıştırın `<asp:Content>` denetimi.

Kaynak kodu kopyalamak açın `ErrorHandling.aspx.vb` sayfasında ve metni kopyalayın *içinde* `EditDeleteDataList_ErrorHandling` sınıfı. Üç olay işleyicileri kopyalayın (`Products_EditCommand`, `Products_CancelCommand`, ve `Products_UpdateCommand`) ile birlikte `DisplayExceptionDetails` yöntemi, ancak **değil** sınıf bildiriminin kopyalayın veya `using` deyimleri. Kopyalanan metni yapıştırmayı *içinde* `EditDeleteDataList_UIValidation` sınıfını `UIValidation.aspx.vb`.

İçerik ve kod üzerinde taşıdıktan `ErrorHandling.aspx` için `UIValidation.aspx`, bir tarayıcıda sayfaların kullanıma test etmek için bir dakikanızı ayırın. Aynı çıktıyı görmek ve her iki sayfaların (bkz: Şekil 2) aynı işlevselliği deneyimi gerekir.


[![UIValidation.aspx sayfanın ErrorHandling.aspx işlevlerini taklit eder.](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image4.png)

**Şekil 2**: `UIValidation.aspx` sayfa işlevlerini taklit eden `ErrorHandling.aspx` ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>2. adım: DataList s EditItemTemplate için doğrulama denetimleri ekleme

Veri girişi formları oluştururken kullanıcılar gerekli alanları girin ve bunların sağlanan girişler geçerli, doğru biçimlendirilmiş değerlerinin olduğunu önemlidir. Bir kullanıcı s girişleri geçerli olduğundan emin olun yardımcı olmak için tek bir giriş Web denetim değerini doğrulamak için tasarlanmış beş yerleşik doğrulama denetimleri ASP.NET sağlar:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) bir değer sağlandı sağlar
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) bir değeri başka bir Web denetim değerini veya bir sabit değer karşı doğrular ve s değeri biçimi belirtilen veri türü için geçerli olmasını sağlar
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) değerleri aralığı içinde bir değer olmasını sağlar
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) karşı bir değer doğrulayan bir [normal ifade](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) bir değer, kullanıcı tarafından tanımlanan özel bir yöntem karşı doğrular

Bu beş denetimleri hakkında daha fazla bilgi için kiracıurl [arabirimleri ekleme ve düzenleme için doğrulama denetimleri ekleme](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) öğretici veya kullanıma [doğrulama denetimleri bölümüne](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) ,[ASP.NET hızlı başlangıç öğreticileri](https://quickstarts.asp.net).

Müşterilerimize öğreticide biz ürün adı için bir değer sağlandığından emin olmak için bir RequiredFieldValidator ve girilen fiyat 0'a eşit veya daha büyük bir değere sahip ve geçerli para birimi biçiminde sunulan emin olmak için bir CompareValidator kullanmanız gerekir.

> [!NOTE]
> While ASP.NET 1.x sahip aynı bu beş doğrulama denetimleri, ASP.NET 2.0 birkaç geliştirme eklendi, Internet Explorer'a ek olarak tarayıcılar ve bölüm doğrulama denetimleri içeren bir sayfa ile özelliği için destek ana iki istemci tarafı komut dosyası oluşturuluyor doğrulama gruplar. Yeni doğrulama denetimi 2.0 hakkında daha fazla bilgi için başvurmak [doğrulama denetimleri ASP.NET 2.0 ayrıntıları](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


DataList s gerekli doğrulama denetimleri ekleyerek başlayın s izin `EditItemTemplate`. Bu görevi, Tasarımcı DataList s akıllı etiketinde Şablonları Düzenle bağlantısına tıklayarak veya bildirim temelli söz dizimi aracılığıyla gerçekleştirilebilir. Tasarım görünümünde Şablonları Düzenle seçeneğini kullanarak işlem s adımlayın olanak tanır. DataList s düzenlenecek seçtikten sonra `EditItemTemplate`, yerleştirme bundan sonra şablon düzenleme arabirimine Toolbox'tan sürükleyerek bir RequiredFieldValidator ekleme `ProductName` metin.


[![Bir RequiredFieldValidator için EditItemTemplate sonra ProductName metin kutusu ekleyin.](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image7.png)

**Şekil 3**: eklemek için bir RequiredFieldValidator `EditItemTemplate After` `ProductName` TextBox ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image9.png))


Tüm doğrulama denetimleri, tek bir ASP.NET Web denetim girişi doğrulayarak çalışır. Bu nedenle, eklediğimiz yöntemlerin RequiredFieldValidator karşı doğrulamalıdır belirtmek ihtiyacımız `ProductName` TextBox; bu doğrulama denetimi s ayarlayarak yapılır [ `ControlToValidate` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) için `ID` , uygun Web denetimi (`ProductName`, bu örnekte). Ardından, ayarlama [ `ErrorMessage` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) için ürün s adını belirtmeniz gerekir ve [ `Text` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) için \*. `Text` Özellik değeri, sağlanan varsa, doğrulama başarısız olursa doğrulama denetimi tarafından görüntülenen metin. `ErrorMessage` Gerekli olan özellik değeri varsa ValidationSummary denetimi tarafından kullanılan `Text` özellik değeri atlanırsa, `ErrorMessage` özellik değeri geçersiz giriş doğrulama denetimi tarafından görüntülenir.

Bu üç özelliklerini RequiredFieldValidator ayarladıktan sonra ekranınızın Şekil 4'e benzer görünmelidir.


[![RequiredFieldValidator s ControlToValidate, ErrorMessage ve metin özellikleri ayarlama](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image10.png)

**Şekil 4**: RequiredFieldValidator s ayarlamak `ControlToValidate`, `ErrorMessage`, ve `Text` özellikleri ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image12.png))


İle eklenen RequiredFieldValidator `EditItemTemplate`, tüm kalan olduğunu ürün s fiyatı metin kutusu için gerekli doğrulama eklemek için. Bu yana `UnitPrice` isteğe bağlı olduğu bir kaydın düzenlenmesi, biz bir RequiredFieldValidator eklemek için gereksinim t ki. Ancak, emin olmak için bir CompareValidator eklemek ihtiyacımız `UnitPrice`, belirtilirse, bir para birimi olarak düzgün biçimlendirildiğinden ve büyük veya 0'a eşit.

CompareValidator içine ekleme `EditItemTemplate` ve kendi `ControlToValidate` özelliğini `UnitPrice`, kendi `ErrorMessage` fiyat özelliğini değerinden büyük veya sıfıra eşit olmalı ve para birimi sembolünü içeremez ve kendi `Text` özelliğini\*. Göstermek için `UnitPrice` değeri 0'a eşit veya sıfırdan büyük, CompareValidator s ayarlamak [ `Operator` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) için `GreaterThanEqual`, kendi [ `ValueToCompare` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) 0 ve kendi [ `Type` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) için `Currency`.

Bu iki doğrulama denetimleri, s DataList ekledikten sonra `EditItemTemplate` s bildirim temelli söz dizimi aşağıdakine benzer görünmelidir:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Bu değişiklikleri yaptıktan sonra sayfasını bir tarayıcıda açın. Bir ürün düzenlerken geçersiz Fiyat değeri girin veya adını Atla çalışırsanız, metin kutusunun yanındaki bir yıldız işareti görünür. Şekil 5 gösterildiği gibi $19.95 para birimi sembolü içeren bir fiyat değerini geçersiz olarak kabul edilir. CompareValidator s `Currency` `Type` rakam ayırıcıları (örneğin, kültür ayarlarına bağlı olarak, nokta veya virgül) ve önüne bir artı veya eksi işareti için izin verir, ancak mu *değil* bir para birimi simgesi izin verir. Şu anda düzenleme arabirimi işler gibi bu davranış kullanıcılar perplex `UnitPrice` para birimi biçimi kullanarak.


[![Geçersiz giriş içeren metin kutuları yanında bir yıldız işareti görünür](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image13.png)

**Şekil 5**: bir yıldız işareti görünür bir sonraki geçersiz giriş içeren metin kutuları ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image15.png))


While olarak doğrulama çalışır-olduğundan, kullanıcının sahip olduğu kabul edilebilir değil bir kaydı düzenleme yaparken para birimi simgesi el ile kaldırmak. Ayrıca, varsa geçersiz girişler düzenleme tıklandığında, bir geri gönderme çağıracağı hiçbiri güncelleştirme ya da İptal düğmeleri, arabirim. İdeal olarak, iptal düğmesine DataList kullanıcı s girişleri geçerliliğini bağımsız olarak önceden düzenleme durumuna getirir. Ayrıca, ürün bilgisi s DataList'te güncelleştirmeden önce sayfa s verileri geçerli olmasını sağlamak ihtiyacımız `UpdateCommand` istemci tarafı mantığını cors'un atlanması tarayıcıları t destek JavaScript ki ya da kullanıcılar tarafından doğrulama denetimleri olarak bir olay işleyicisi desteğini devre dışı.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Para birimi simgesi EditItemTemplate s UnitPrice TextBox'dan kaldırılıyor

CompareValidator s kullanırken `Currency``Type`, Doğrulanmakta olan giriş para birimi sembollerini içermemesi gerekir. Tür simgeleri varlığını giriş geçersiz olarak işaretlemek CompareValidator neden olur. Şu anda bir para birimi sembolü, ancak düzenleme arabirimimizi içerir `UnitPrice` metin kullanıcı açıkça kaldırmalısınız para birimi simgesi yaptıkları değişiklikleri kaydetmeden önce anlamına gelir. Bu sorunu gidermek için üç seçenek sunuyoruz:

1. Yapılandırma `EditItemTemplate` böylece `UnitPrice` TextBox değeri, para birimi olarak biçimlendirilmemiş.
2. Kullanıcının bir para birimi simgesi CompareValidator kaldırarak ve düzgün şekilde biçimlendirilmiş bir para birimi değerini denetleyen bir RegularExpressionValidator yerine girin izin verin. Buradaki zorluk, bir para birimi değeri doğrulamak için normal ifade CompareValidator basit değildir ve kültür ayarları eklemek isterseniz, kod yazma gerekir.
3. Doğrulama denetimi tamamen kaldırın ve özel sunucu tarafı doğrulama mantığını GridView s dayanan `RowUpdating` olay işleyicisi.

Bu öğretici için 1 seçeneği ile Git s olanak tanır. Şu anda `UnitPrice` metin kutusunda veri bağlama ifadesi nedeniyle para birimi değeri olarak biçimlendirilmiş `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Değişiklik `Eval` ifadesine `Eval("UnitPrice", "{0:n2}")`, duyarlık iki basamaklı sayı olarak sonucu biçimlendirir. Bu bildirim temelli söz dizimi aracılığıyla doğrudan veya veri bağlamaları Düzenle bağlantısına tıklayarak yapılabilir `UnitPrice` TextBox s DataList'te `EditItemTemplate`.

Bu değişikliğe düzenleme arabirimi biçimlendirilmiş fiyatına Grup ayırıcı olarak virgül ve ondalık ayırıcısı olarak nokta içerir, ancak para birimi simgesi devre dışı bırakır.

> [!NOTE]
> Para birimi biçimi düzenlenebilir arabiriminden kaldırırken, ı TextBox dışında metin olarak para birimi simgesini yerleştirmek yararlı. Bu, para birimi simgesi sağlamak zorunda değildir kullanıcıya bir ipucu görür.


## <a name="fixing-the-cancel-button"></a>İptal düğmesi düzeltme

Varsayılan olarak, istemci tarafında doğrulama gerçekleştirmek için JavaScript doğrulama Web denetimleri gösterin. Bir düğme, LinkButton veya ImageButton tıklandı, geri gönderme gerçekleşmeden önce sayfadaki doğrulama denetimleri istemci tarafında denetlenir. Herhangi bir geçersiz veri varsa, geri gönderme işlemi iptal edildi. Belirli düğmelerin için yine de verilerin geçerliliğini kurucularýn olabilir; Böyle bir durumda, geçersiz veriler nedeniyle iptal geri gönderme bir sıkıntı yaşanmaktadır.

İptal düğmesi böyle bir örnektir. Bir kullanıcı s ürün adını atlama gibi geçersiz verilerden girer ve ardından SEH eklenmemişse t istediğiniz tüm ürün kaydetmeye karar verir ve iptal düğmesi İsabetleri olduğunu hayal edin. Şu anda iptal düğmesi, ürün adı eksik ve geri göndermeyi önlemek rapor sayfasında doğrulama denetimleri tetikler. Metin türü kullanıcı sahip `ProductName` yalnızca dışında düzenleme işlemi iptal etmek için metin kutusu.

Neyse ki, düğme, LinkButton ve ImageButton sahip bir [ `CausesValidation` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) , belirtebilirsiniz tıklayarak olup olmadığını doğrulama mantığı düğme başlatmak (varsayılan olarak `True`). İptal düğmesi s ayarlamak `CausesValidation` özelliğini `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Geçerli UpdateCommand olay işleyicisinde olan giriş sağlama

Doğrulama denetimleri tarafından yayılan istemci tarafı komut dosyası nedeniyle bir kullanıcı geçersiz giriş girerse doğrulama denetimleri LinkButton, düğme tarafından başlatılan tüm Geri göndermeler iptal etme veya ImageButton denetimleri `CausesValidation` özellikleri `True` ( varsayılan). Ancak, bir kullanıcı bir antiquated tarayıcı veya biri olan JavaScript desteği devre dışı bırakıldı ziyaret, istemci tarafı doğrulama denetimlerini çalıştırmaz.

Tüm ASP.NET doğrulama denetimleri kendi doğrulama mantığı hemen geri gönderme sırasında tekrarlayın ve rapor sayfası s girişleri genel geçerliliğini [ `Page.IsValid` özelliği](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Ancak, sayfa akışı değil kesildi veya herhangi bir şekilde değerine göre durduruldu `Page.IsValid`. Geliştiriciler olarak, bu emin olmak için sunduğumuz sorumluluğundadır `Page.IsValid` özellik değerine sahip `True` giriş verileri, geçerli varsayan kod devam etmeden önce.

JavaScript devre dışı bir kullanıcı varsa sayfamızı ziyaret eder, bir ürün düzenler, çok fiyat değeri girer pahalı ve güncelleştir düğmesine tıkladığında istemci tarafı doğrulama atlanır ve bir geri gönderme oluşması. Geri gönderme, ASP.NET sayfası s üzerinde `UpdateCommand` olay işleyicisi yürütülür ve çok ayrıştırma çalışılırken bir özel durum için pahalı bir `Decimal`. Özel durum işleme sahibiz, böyle bir özel durum düzgün bir şekilde ele alınır, ancak biz aracılığıyla başlangıçta yalnızca ile devam ederek kayan öğesinden geçersiz veri engelleyebilir beri `UpdateCommand` olay işleyicisi, `Page.IsValid` değeri `True`.

Başlangıcına aşağıdaki kodu ekleyin `UpdateCommand` olay işleyicisi, hemen önce `Try` engelle:


[!code-csharp[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample2.cs)]

Bu eklenmesiyle, ürün gönderilen verilerin geçerli ise güncelleştirilmesi dener. Çoğu kullanıcı t kazanılan geçersiz veri doğrulama denetimleri istemci tarafı betikleri nedeniyle geri gönderme mümkün olmayacaktır, ancak tarayıcıları t destek JavaScript ki veya JavaScript desteği olan kullanıcılar devre dışı, istemci tarafı denetimleri atlamak ve geçersiz veriler gönderme.

> [!NOTE]
> Kurnaz okuyucu hatları ile veri güncelleştirme yapılırken sözcüğünün ki etmedi t gereksinim açıkça denetlemek `Page.IsValid` bizim sayfası s arka plan kod sınıfı bir özellik. GridView danışır olmasıdır `Page.IsValid` özellik bize ve yalnızca güncelleştirme yalnızca değerini döndürürse sürdürür `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>3. adım: Veri girişi sorunlarını özetleme

Ek olarak beş doğrulama denetimleri, ASP.NET içerir [ValidationSummary denetimi](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), görüntüleyen `ErrorMessage` geçersiz veri algılandı. Bu doğrulama denetimleri, s. Bu Özet veriler, kalıcı, istemci tarafı messagebox aracılığıyla ya da web sayfasında metin olarak görüntülenebilir. Herhangi bir doğrulama sorunu özetleyen bir istemci-tarafı messagebox eklemek için bu öğreticiden geliştirmek s olanak tanır.

Bunu yapmak için ValidationSummary denetimi Tasarımcısı araç kutusundan sürükleyin. Konum, ValidationSummary denetimi eklenmemişse t gerçekten önemli, bu yana biz yalnızca bir messagebox özeti görüntülemek için yapılandırmak için ekleyeceğiz. Denetimi ekledikten sonra ayarlama, [ `ShowSummary` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) için `False` ve kendi [ `ShowMessageBox` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) için `True`. Bu ekleme ile bir istemci-tarafı messagebox tüm doğrulama hatalarını özetlenmiştir (bkz. Şekil 6).


[![Doğrulama hataları bir istemci-tarafı Messagebox özetlenmiştir](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image16.png)

**Şekil 6**: doğrulama hataları bir istemci-tarafı Messagebox özetlenir ([tam boyutlu görüntüyü görmek için tıklatın](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image18.png))


## <a name="summary"></a>Özet

Bu öğreticide proaktif olarak güncelleştirme iş akışında kullanmayı denemeden önce bizim kullanıcı girişleri geçerli olmasını sağlamak için doğrulama denetimlerini kullanarak özel durumları olasılığını azaltmak nasıl gördük. ASP.NET, belirli bir Web incelemek için tasarlanmış beş doğrulama Web denetimleri s giriş denetleme ve geri giriş s geçerlilik üzerinde rapor sağlar. Bu öğreticide bu beş denetimi iki RequiredFieldValidator ve CompareValidator s ürün adı sağlandı ve fiyat değerinden büyük veya sıfıra eşit bir değer ile para birimi biçimine sahip olmak için kullandık.

DataList s arabirimini düzenleme doğrulama denetimleri ekleme üzerine sürükleyerek olarak basit `EditItemTemplate` araç ve özelliklerini birkaç ayarı. Varsayılan olarak, doğrulama denetimleri otomatik olarak istemci tarafı doğrulama komut yayma; Şirket içinde toplu sonucu depolamadan geri gönderme, sunucu tarafı doğrulama de sağlanır `Page.IsValid` özelliği. Bir düğme, LinkButton veya ImageButton tıklandı olduğunda istemci tarafı doğrulamasını atlamak için düğmeye s ayarlamak `CausesValidation` özelliğini `False`. Ayrıca, geri göndermede gönderilen veriler ile tüm görevleri gerçekleştirmeden önce emin olun `Page.IsValid` özelliği döndürür `True`.

Tüm öğreticileri düzenleme DataList biz TextBox s ürün adı, diğeri için fiyat, şimdiye incelenir ve çok basit bir düzenleme arabirimleri vardı. Düzenleme, arabirim, ancak DropDownList, takvimler, RadioButton'ları, onay kutuları vb. gibi farklı Web denetimleri bir karışımını içerebilir. Web denetimleri çeşitli kullanan bir arabirim oluşturma sırasında sonraki müşterilerimize öğreticide inceleyeceğiz.

Mutlu programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar yedi ASP/ASP.NET kitaplardan ve poshbeauty.com sitesinin [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileriyle beri 1998'de çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). He adresinden ulaşılabilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Dennis Patterson ve Konuğu, Ken Pespisa ve Liz Shulok yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](handling-bll-and-dal-level-exceptions-cs.md)
> [İleri](customizing-the-datalist-s-editing-interface-cs.md)
