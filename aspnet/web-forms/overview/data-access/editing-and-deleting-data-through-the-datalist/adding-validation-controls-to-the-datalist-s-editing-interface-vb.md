---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
title: Arabirim (VB) DataList doğrulama denetimleri ekleme düzenleme kullanıcının | Microsoft Docs
author: rick-anderson
description: Bu öğreticide daha iyi bir düzenleme kullanıcı int sağlamak amacıyla DataList'ın EditItemTemplate doğrulama denetimleri ekleme ne kadar kolay olduğunu göreceksiniz...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/30/2006
ms.topic: article
ms.assetid: 6b073fc6-524d-453d-be7c-0c30986de391
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 46ff0b18c1ea24dd73c9e3034c1b5e53f2e6d0c2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888337"
---
<a name="adding-validation-controls-to-the-datalists-editing-interface-vb"></a>DataList'ın düzenleme arabirimine (VB) doğrulama denetimleri ekleme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Örnek uygulamayı indirin](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_VB.exe) veya [PDF indirin](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/datatutorial39vb1.pdf)

> Bu öğreticide daha iyi bir düzenleme kullanıcı arabirimi için DataList'ın EditItemTemplate doğrulama denetimleri ekleme ne kadar kolay olduğunu göreceksiniz.


## <a name="introduction"></a>Giriş

Ürün adı eksik veya bir özel durum negatif fiyat sonuçlarında gibi geçersiz kullanıcı girişi olsa bile öğreticileri bugüne kadarki düzenleme DataList herhangi öngörülü kullanıcı girdisi doğrulama arabirimleri düzenleme belirleyebilen eklemediniz. İçinde [önceki öğretici](handling-bll-and-dal-level-exceptions-vb.md) şu özel durum işleme DataList s için kod ekleme incelenmesi `UpdateCommand` yakalamak ve düzgün biçimde ortaya çıktı tüm özel durumlar hakkında bilgi görüntülemek için olay işleyicisi. İdeal olarak, ancak düzenleme arabirimi bir kullanıcı ilk başta böyle geçersiz veri girişini engellemek için doğrulama denetimleri içerir.

Bu öğreticide s DataList doğrulama denetimleri ekleme ne kadar kolay olduğunu göreceğiz `EditItemTemplate` daha iyi bir düzenleme kullanıcı arabirimi için. Özellikle, bu öğreticinin önceki öğreticide oluşturulan örnek alır ve uygun doğrulama içerecek şekilde düzenleme arabirimi artırmaktadır.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-vbmd"></a>1. adım: örnekten çoğaltma[BLL ve DAL düzeyi özel durumları işleme](handling-bll-and-dal-level-exceptions-vb.md)

İçinde [işleme BLL - ve DAL düzeyinde istisnalar](handling-bll-and-dal-level-exceptions-vb.md) adları ve iki sütun, düzenlenebilir DataList ürünlerinde fiyatları listelenen bir sayfa oluşturduğumuz öğretici. Amacımız Bu öğretici için doğrulama denetimlerinin içerecek şekilde DataList s düzenleme arabirimi büyütmek olmaktır. Özellikle, doğrulama mantığımızı olur:

- Ürün s adının sağlanması gerekir
- Fiyat için girilen değer geçerli para birimi biçiminde olduğundan emin olun
- Fiyat negatif itibaren sıfıra eşit veya daha büyük için girilen değer emin `UnitPrice` değeri geçersiz

Biz doğrulama dahil etmek için önceki örnekte program.cs'ye adresindeki bakabilirsiniz önce ilk örnekten çoğaltmak ihtiyacımız `ErrorHandling.aspx` sayfasındaki `EditDeleteDataList` Bu öğretici için sayfa klasörüne `UIValidation.aspx`. Bu hem kopyalamak için ihtiyacımız elde etmek için `ErrorHandling.aspx` sayfa s bildirim temelli biçimlendirme ve kaynak kodu. İlk bildirim temelli biçimlendirme, aşağıdaki adımları gerçekleştirerek kopyalayın:

1. Açık `ErrorHandling.aspx` Visual Studio'da sayfası
2. Sayfa s bildirim temelli biçimlendirme (sayfanın sonundaki kaynağı düğmesini tıklatın) gidin
3. Metni kopyalayın `<asp:Content>` ve `</asp:Content>` gösterildiği Şekil 1 olarak etiketler (çizgiler 3 ile 32 arasında).


[![Metin içinde kopyalama &lt;asp: içerik&gt; denetimi](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image1.png)

**Şekil 2**: metin içinde kopyalama `<asp:Content>` denetimi ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image3.png))


1. Açık `UIValidation.aspx` sayfası
2. Sayfa s bildirim temelli işaretlemede gidin
3. İçindeki metni yapıştırın `<asp:Content>` denetim.

Kaynak kodu kopyalamak için açık `ErrorHandling.aspx.vb` sayfasında ve yalnızca metni kopyalayın *içinde* `EditDeleteDataList_ErrorHandling` sınıfı. Üç olay işleyicileri kopyalayın (`Products_EditCommand`, `Products_CancelCommand`, ve `Products_UpdateCommand`) ile birlikte `DisplayExceptionDetails` yöntemi, ancak **değil** sınıf bildiriminin kopyalayın veya `using` deyimleri. Kopyalanan metni yapıştırmayı *içinde* `EditDeleteDataList_UIValidation` sınıfını `UIValidation.aspx.vb`.

İçerik ve kod üzerinden taşıdıktan `ErrorHandling.aspx` için `UIValidation.aspx`, bir tarayıcı sayfalarında çıkışı test etmek için bir dakikanızı ayırın. Aynı çıktı bakın ve her (bkz: Şekil 2) bu iki sayfaları aynı işlevselliği deneyimi gerekir.


[![UIValidation.aspx sayfa ErrorHandling.aspx işlevindeki taklit eder](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image4.png)

**Şekil 2**: `UIValidation.aspx` sayfa taklit eder işlevindeki `ErrorHandling.aspx` ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image6.png))


## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>2. adım: DataList s EditItemTemplate için doğrulama denetimleri ekleme

Veri girişi formları oluşturulurken, kullanıcılar gerekli alanları girin ve kendi sağlanan tüm girişleri yasal, düzgün biçimlendirilmiş değerler olduğunu önemlidir. Bir kullanıcı s girişleri geçerli olduğundan emin olun yardımcı olmak için ASP.NET, tek bir giriş Web denetim değerini doğrulamak için tasarlanmış beş yerleşik doğrulama denetimleri sağlar:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) değeri sağlanmış sağlar
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) bir değeri başka bir Web denetimi değer veya sabit bir değer karşı doğrular veya değer s biçimi, belirtilen veri türü için geçerli olmasını sağlar
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) değerleri aralığı içinde bir değer olmasını sağlar
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) bir değer karşı doğrular bir [normal ifade](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) bir değer özel, kullanıcı tanımlı bir yöntem karşı doğrular

Bu beş denetimleri hakkında daha fazla bilgi için geri başvurmak [doğrulama denetimleri ekleme düzenleme ve ekleme arabirimleri için](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) öğretici veya kullanıma [doğrulama denetimleri bölüm](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) ,[ASP.NET Quickstart öğreticileri](https://quickstarts.asp.net).

Bizim öğretici için size ürün adı için bir değer sağlanan emin olmak için bir RequiredFieldValidator ve girilen fiyat 0 değerine eşit veya daha büyük bir değere sahip ve geçerli para birimi biçiminde sunulan emin olmak için bir CompareValidator kullanmanız gerekir.

> [!NOTE]
> While ASP.NET 1.x sahip aynı bu beş doğrulama denetimleri, ASP.NET 2.0 bazı geliştirmeler eklendi, ana iki istemci tarafı komut dosyası olan Internet Explorer yanı sıra tarayıcılar ve bir sayfaya bölüm doğrulama denetimleri yeteneği desteği doğrulama grupları. 2. 0 doğrulama denetimi yenilikleri hakkında daha fazla bilgi için başvurmak [ASP.NET 2. 0 doğrulama denetimleri Dissecting](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).


S DataList gerekli doğrulama denetimlerini ekleyerek başlayın s izin `EditItemTemplate`. Bu görev, DataList s akıllı etiket Şablonları Düzenle bağlantısından tıklayarak Tasarımcısı aracılığıyla ya da bildirim temelli söz dizimi aracılığıyla gerçekleştirilebilir. S adım Tasarım görünümünden Şablonları Düzenle seçeneğini kullanarak işlemiyle olanak tanır. S DataList düzenlemek seçme sonra `EditItemTemplate`, şablon düzenleme arabirimine Araç Kutusu'ndan sürükleyerek bir RequiredFieldValidator ekleyin, sonra yerleştirme `ProductName` metin kutusu.


[![Bir RequiredFieldValidator EditItemTemplate sonra ProductName metin kutusu ekleyin.](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image7.png)

**Şekil 3**: bir RequiredFieldValidator eklemek `EditItemTemplate After` `ProductName` TextBox ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image9.png))


Tüm doğrulama denetimleri, tek bir ASP.NET Web denetim girişi doğrulayarak çalışır. Bu nedenle, az önce eklediğimiz RequiredFieldValidator karşı doğrulamalıdır belirtmek ihtiyacımız `ProductName` TextBox; bu doğrulama denetimi s ayarlayarak yapılır [ `ControlToValidate` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) için `ID` , uygun Web denetimi (`ProductName`, bu örnekte). Ardından, ayarlayın [ `ErrorMessage` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) için ürün s adını sağlayın ve [ `Text` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) için \*. `Text` Özellik değeri, sağlanan varsa, doğrulama başarısız olursa doğrulama denetimi tarafından görüntülenen metin. `ErrorMessage` , Gerekli özellik değeri ValidationSummary denetimi tarafından; kullanılır `Text` özellik değeri atlanırsa, `ErrorMessage` özellik değeri geçersiz giriş doğrulama denetimi tarafından görüntülenir.

Bu üç RequiredFieldValidator özelliklerini ayarladıktan sonra ekranınızın Şekil 4'e benzer görünmelidir.


[![RequiredFieldValidator s ControlToValidate, ErrorMessage ve metin özelliklerini ayarlama](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image10.png)

**Şekil 4**: RequiredFieldValidator s ayarlamak `ControlToValidate`, `ErrorMessage`, ve `Text` özellikleri ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image12.png))


Eklenen RequiredFieldValidator ile `EditItemTemplate`, tüm kalan olduğunu ürün s fiyat TextBox gerekli doğrulama eklemek için. Bu yana `UnitPrice` isteğe bağlı olan bir kayıt düzenlerken, biz t bir RequiredFieldValidator eklemek gerek güncelleştireceğinizi. Ancak, emin olmak için bir CompareValidator eklemek ihtiyacımız `UnitPrice`, sağlandıysa, para birimi olarak düzgün biçimlendirildiğinden ve büyük veya 0 değerine eşit.

CompareValidator içine ekleme `EditItemTemplate` ve kendi `ControlToValidate` özelliğine `UnitPrice`, kendi `ErrorMessage` fiyat özelliğine değerinden büyük veya sıfıra eşit olmalı ve para birimi simgesini içeremez ve kendi `Text` özelliğine\*. Belirtmek için `UnitPrice` değeri sıfırdan büyük veya 0 değerine eşit, CompareValidator s ayarlamak [ `Operator` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) için `GreaterThanEqual`, kendi [ `ValueToCompare` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) 0 ve kendi [ `Type` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) için `Currency`.

Bu iki doğrulama denetimleri, s DataList ekledikten sonra `EditItemTemplate` s Tanımlayıcı Sözdizimi aşağıdakine benzer görünmelidir:


[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample1.aspx)]

Bu değişiklikleri yaptıktan sonra sayfasını bir tarayıcıda açın. Adını Atla veya bir ürün düzenlerken geçersiz Fiyat değerini girin denerseniz, textbox yanındaki bir yıldız işareti görünür. Şekil 5 gösterildiği gibi $19.95 gibi para birimi simgesini içeren bir fiyat değer geçersiz olarak kabul edilir. CompareValidator s `Currency` `Type` ve bir başında artı veya eksi işareti (örneğin, virgül veya nokta kültür ayarlarına bağlı olarak) basamak ayırıcıları için verir, ancak mu *değil* para birimi simgesini izin verir. Şu anda düzenleme arabirimi işler gibi bu davranış kullanıcılar perplex `UnitPrice` para birimi biçimi kullanarak.


[![Geçersiz giriş ile kutularındaki yanında bir yıldız işareti görünür](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image13.png)

**Şekil 5**: bir yıldız işareti görünür sonraki geçersiz giriş içeren metin kutuları için ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image15.png))


While olarak doğrulama çalışır-olduğundan, kullanıcının sahip olduğu kabul edilebilir değil bir kayıt düzenlerken para birimi simgesini el ile kaldırmak. Ayrıca, varsa geçersiz düzenleme girişleri tıklandığında, bir geri gönderme çağıracağı hiçbiri güncelleştirme ya da İptal düğmeleri, arabirim. İdeal olarak, İptal düğmesi DataList kullanıcı s girişleri geçerliliğini bağımsız olarak önceden düzenleme durumuna döndürür. Ayrıca, ürün bilgileri için s DataList güncelleştirmeden önce sayfa s verileri geçerli olduğundan emin olmak ihtiyacımız `UpdateCommand` istemci-tarafı mantığı tarayıcıları t desteği JavaScript güncelleştireceğinizi ya da sahip kullanıcılar tarafından atlanır doğrulama denetimleri olarak olay işleyicisi desteğini devre dışı.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Para birimi simgesini EditItemTemplate s UnitPrice metin kaldırma

CompareValidator s kullanırken `Currency``Type`, Doğrulanmakta olan giriş hiçbir para birimi simgesini içermemesi gerekir. Bu tür simgeleri varlığını giriş geçersiz olarak işaretlemek CompareValidator neden olur. Ancak, düzenleme arabirimimizi bir para birimi simgesini şu anda içerir `UnitPrice` metin kutusuna, kullanıcının açıkça kaldırmalısınız para birimi simgesini yaptıkları değişiklikleri kaydetmeden önce anlamına gelir. Bu sorunu gidermek için şu üç seçeneğiniz vardır:

1. Yapılandırma `EditItemTemplate` böylece `UnitPrice` TextBox değeri para birimi olarak biçimlendirilmemiş.
2. Kullanıcının CompareValidator kaldırarak ve düzgün şekilde biçimlendirilmiş para birimi değeri için denetler RegularExpressionValidator ile değiştirerek bir para birimi simgesini girin izin verin. Sınama burada para birimi değeri doğrulamak için normal ifade CompareValidator kadar basit değildir ve kültür ayarları içerecek şekilde istediyseniz kod yazma gerektirir.
3. Doğrulama denetimi tamamen kaldırın ve özel sunucu tarafı doğrulama mantığını GridView s Bel `RowUpdating` olay işleyicisi.

Bu öğretici için 1 seçeneğiyle Git s olanak tanır. Şu anda `UnitPrice` metin kutusunda veri bağlama deyimi nedeniyle para birimi değeri olarak biçimlendirilmiş `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Değişiklik `Eval` ifadesine `Eval("UnitPrice", "{0:n2}")`, bir dizi duyarlık iki basamaklı sonucu biçimlendirir. Bu bildirim temelli söz dizimi aracılığıyla doğrudan veya veri bağlamaları Düzenle bağlantıyı tıklatarak yapılabilir `UnitPrice` metin kutusuna s DataList `EditItemTemplate`.

Bu değişikliği düzenleme arabiriminde biçimlendirilmiş Fiyat Grup ayırıcı olarak virgül ve ondalık ayırıcı olarak nokta içerir, ancak para birimi simgesini devre dışı bırakır.

> [!NOTE]
> Para birimi biçiminde düzenlenebilir arabiriminden kaldırırken, ı TextBox dışında metin olarak para birimi simgesini koymak yararlı. Para birimi simgesini sağlamak zorunda değildir kullanıcı bir ipucu olarak görev yapar.


## <a name="fixing-the-cancel-button"></a>İptal düğmesi çözme

Varsayılan olarak, istemci tarafında doğrulama gerçekleştirmek için JavaScript doğrulama Web denetimleri yayma. Düğme, LinkButton veya ImageButton tıklatıldığında geri gönderme oluşmadan önce sayfasında doğrulama denetimleri istemci tarafında denetlenir. Herhangi bir geçersiz veri varsa, geri gönderme işlemi iptal edildi. Belirli düğmeleri için yine de verilerin geçerliliğini kurucularýn olabilir; Böyle bir durumda, geçersiz veri nedeniyle iptal geri gönderme sahip bir zarar verici istenir.

İptal düğmesi böyle bir örnektir. Bir kullanıcı s ürün adı, atlanması gibi geçersiz veri girer ve ardından SEH içermiyor t istediğiniz ürün tüm kaydetmeye karar verir ve iptal düğmesi isabetler olduğunu düşünün. Şu anda iptal düğmesi, ürün adı eksik ve geri gönderme önlemek rapor doğrulama denetimleri sayfasında tetikler. Bizim kullanıcının bazı metin türüne sahip `ProductName` yalnızca dışında düzenleme işlemi iptal etmek için metin kutusu.

Neyse ki, düğme, LinkButton ve ImageButton sahip bir [ `CausesValidation` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) , belirtebilirsiniz tıklatarak olup olmadığına bakılmaksızın düğmesi doğrulama mantığını başlatmak (varsayılan olarak `True`). İptal düğmesi s ayarlamak `CausesValidation` özelliğine `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Olan geçerli UpdateCommand olay işleyicisi alanındaki girişleri sağlama

Doğrulama denetimleri tarafından gösterilen istemci tarafı komut dosyası nedeniyle kullanıcı geçersiz giriş girerse doğrulama denetimleri LinkButton, düğme tarafından başlatılan tüm Geri göndermeler iptal veya ImageButton denetimleri `CausesValidation` özellikleri `True` ( varsayılan). Ancak, bir kullanıcı antiquated bir tarayıcı veya biri, JavaScript desteği devre dışı ziyaret, istemci tarafı doğrulama denetimlerini çalıştırmaz.

Tüm ASP.NET doğrulama denetimleri geri gönderme hemen sonra kullanıcıların doğrulama mantığını yineleyin ve sayfa s girişleri genel geçerliliğini rapor [ `Page.IsValid` özelliği](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Ancak, sayfa akışı kesildi veya olmayan herhangi bir şekilde değere göre durduruldu `Page.IsValid`. Geliştiricileri de bu emin olmak için bizim sorumluluğundadır `Page.IsValid` özellik değerine sahip `True` giriş geçerli varsayar kod işlemine devam etmeden önce.

Bir kullanıcının devre dışı JavaScript varsa sayfamızı ziyaret eder, bir ürün düzenler, çok fiyat değerini girer pahalıdır ve güncelleştirme düğmesine tıklar istemci tarafı doğrulama atlanır ve geri gönderimin olun. Geri gönderme, ASP.NET sayfası s üzerinde `UpdateCommand` olay işleyicisi yürütür ve çok ayrıştırma çalışılırken özel durum oluşturuldu için pahalı bir `Decimal`. Özel durum işleme sahip olduğumuz, böyle bir özel durum düzgün bir şekilde ele alınacağını ancak üzerinden ilk başta ile yalnızca etmeden tarafından kayan gelen biz geçersiz veriler engelleyebilir beri `UpdateCommand` olay işleyicisi varsa `Page.IsValid` değerine sahip `True`.

Aşağıdaki kodu başlangıcına ekleyin `UpdateCommand` olay işleyicisi hemen önce `Try` engelle:


[!code-vb[Main](adding-validation-controls-to-the-datalist-s-editing-interface-vb/samples/sample2.vb)]

Bu eklenmesiyle ürün gönderilen verilerin geçerli ise güncelleştirilmesi dener. T won çoğu kullanıcılar doğrulama denetimleri istemci tarafı betikler nedeniyle geçersiz veri geri gönderme, ancak t desteği JavaScript tarayıcıları güncelleştireceğinizi veya destek JavaScript sahip kullanıcılar devre dışı, istemci-tarafı denetimlerini atlamak ve geçersiz veriler gönderme.

> [!NOTE]
> Akıllı duruma okuyucu veri GridView ile güncelleştirirken sözcüğünün biz etmedi t gereksinim açıkça denetlemek `Page.IsValid` bizim sayfa s arka plandaki kod sınıfı bir özellik. GridView danışır Bunun nedeni `Page.IsValid` özelliği yalnızca yalnızca değerini döndürürse güncelleştirmesi ile devam eder ve bize için `True`.


## <a name="step-3-summarizing-data-entry-problems"></a>3. adım: Veri girişi sorunlarını özetleme

Beş doğrulama denetimleri ek olarak, ASP.NET içerir [ValidationSummary denetimi](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), görüntüleyen `ErrorMessage` geçersiz veriler algılandı bu doğrulama denetimleri s. Bu özet verilerini kalıcı, istemci-tarafı messagebox aracılığıyla ya da web sayfasında metin olarak görüntülenebilir. Doğrulama sorunları özetlemeye bir istemci-tarafı messagebox dahil etmek için bu öğreticiyi geliştirmek s olanak tanır.

Bunu gerçekleştirmek için araç kutusu tasarımcıya ValidationSummary denetimi sürükleyin. ValidationSummary denetimi içermiyor t konumunu gerçekten önemli, bu yana biz yalnızca Özet messagebox görüntülemek için yapılandırmak için ekleyeceğiz. Denetim ekledikten sonra ayarlamak kendi [ `ShowSummary` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) için `False` ve kendi [ `ShowMessageBox` özelliği](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) için `True`. Bu ayrıca ile bir istemci-tarafı messagebox tüm doğrulama hatalarını özetlenir (bkz. Şekil 6).


[![Doğrulama hatalarını bir istemci-tarafı Messagebox özetlenir](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image16.png)

**Şekil 6**: doğrulama hataları, bir istemci-tarafı Messagebox özetlenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](adding-validation-controls-to-the-datalist-s-editing-interface-vb/_static/image18.png))


## <a name="summary"></a>Özet

Bu öğreticide önceden güncelleştirme iş akışında kullanmayı denemeden önce bizim kullanıcılar girişleri geçerli olduğundan emin olmak için doğrulama denetimlerini kullanarak özel durumlar olasılığını azaltmak nasıl gördük. ASP.NET, belirli bir Web incelemek için tasarlanmış beş doğrulama Web denetimleri s girişi denetleyebilir ve geri giriş s geçerliliği rapor sağlar. Bu öğreticide iki bu beş denetimleri RequiredFieldValidator ve CompareValidator ürün s adı sağlandı ve fiyat değeri sıfırdan büyük veya sıfıra eşit bir para birimi biçimi olan emin olmak için kullandık.

Arabirim düzenleme DataList s doğrulama denetimleri ekleme üzerine sürükleyerek olarak basit `EditItemTemplate` araç ve Özellikler sayıda ayarlama. Varsayılan olarak, doğrulama denetimleri otomatik olarak istemci tarafı doğrulama betiği yayma; Ayrıca sunucu tarafında doğrulama geri gönderme, toplu sonucunda depolama üzerinde sağladıkları `Page.IsValid` özelliği. Düğme, LinkButton veya ImageButton tıklandığında istemci tarafı doğrulamasını atlamak için s düğmesi ayarlamak `CausesValidation` özelliğine `False`. Ayrıca, geri göndermede gönderilen veriler ile tüm görevleri gerçekleştirmeden önce emin `Page.IsValid` özelliği döndürür `True`.

Tüm öğreticileri düzenleme DataList biz kadarki incelenmesi ve ürün s adı için bir metin kutusu ve başka fiyat çok basit düzenleme arabirimleri sahip. Düzenleme arabirimi ancak DropDownLists, takvim, RadioButtons, onay kutularını ve benzerleri gibi farklı Web denetimleri bir karışımını içerebilir. Bizim sonraki öğreticide Web denetimleri çeşitli kullanan bir arabirim oluşturmayı öğreneceksiniz.

Mutluluk programlama!

## <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar ve yedi ASP/ASP.NET books kurucusu, [4GuysFromRolla.com](http://www.4guysfromrolla.com), Microsoft Web teknolojileri ile bu yana 1998 çalışma. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 2.0 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Kendisi üzerinde erişilebilir [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) veya kendi blog hangi adresinde bulunabilir [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Dennis Patterson, Ken Pespisa ve Liz Shulok yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Önceki](handling-bll-and-dal-level-exceptions-vb.md)
> [sonraki](customizing-the-datalist-s-editing-interface-vb.md)
