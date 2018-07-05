---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
title: Denetim Kimliği adlandırma (VB) İçerik sayfalarında | Microsoft Docs
author: rick-anderson
description: ContentPlaceHolder denetimlerinin nasıl bir adlandırma kapsayıcısı olarak görev yapar ve bu nedenle (FindConrol) zor bir denetimi ile program aracılığıyla çalışma olun gösterir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: dbb024a6-f043-4fc5-ad66-56556711875b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 1b922fb230169824659222da0b9504ec38c36d8a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387718"
---
<a name="control-id-naming-in-content-pages-vb"></a>Denetim Kimliği adlandırma içerik sayfalar (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_VB.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_VB.pdf)

> ContentPlaceHolder denetimlerinin nasıl bir adlandırma kapsayıcısı olarak görev yapar ve bu nedenle (FindConrol) zor bir denetimi ile program aracılığıyla çalışma olun gösterilmektedir. Bu sorun ve geçici çözümler bakar. Ayrıca sonuç ClientID değerini programlı olarak erişmek nasıl anlatılmaktadır.


## <a name="introduction"></a>Giriş

Tüm ASP.NET sunucu denetimleri içeren bir `ID` özellik benzersiz olarak denetimi tanımlar ve olarak denetimin program aracılığıyla erişilen arka plan kod sınıfı anlamına gelir. Benzer şekilde, bir HTML belgedeki öğeler içerebilir bir `id` öğeyi benzersiz olarak tanımlayan özniteliği; bu `id` değerleri genellikle istemci tarafı betikte program aracılığıyla belirli bir HTML öğesi başvurmak için kullanılır. Bir ASP.NET sunucu denetimi HTML işlendiğinde bu göz önünde bulundurulduğunda, kabul, `ID` değeri olarak kullanılır `id` İşlenmiş HTML öğesinin değeri. Bazı durumlarda tek bir denetim tek bir tıklatmayla bu mutlaka çalışması olmadığından `ID` değeri biçimlendirmenin içinde birden çok kez görünebilir. Bir etiket Web denetimiyle birlikte bir TemplateField içeren bir GridView denetimi göz önünde bir `ID` değerini `ProductName`. GridView zamanında kendi veri kaynağına bağlandığında bu etiket her GridView satır için bir kez yinelenir. Her etiket gereksinimlerini benzersiz bir işlenen `id` değeri.

Bu senaryolara işlemek için belirli denetimler kapsayıcılarını adlandırmayla olarak bilinen ASP.NET sağlar. Adlandırma kapsayıcısı yeni bir hizmet `ID` ad alanı. Adlandırma kapsayıcısı içinde görüntülenen tüm sunucu denetimlerinin, işlenmiş olan `id` değeri önekiyle `ID` adlandırma kapsayıcı denetimi. Örneğin, `GridView` ve `GridViewRow` sınıfları adlandırma hem kapsayıcılardır. Sonuç olarak, bir GridView TemplateField ile tanımlanan bir etiket denetimi `ID` `ProductName` işlenen verilen `id` değerini `GridViewID_GridViewRowID_ProductName`. Çünkü *GridViewRowID* ortaya çıkan her GridView satır için benzersiz olan `id` değerler benzersiz.

> [!NOTE]
> [ `INamingContainer` Arabirimi](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) belirli bir ASP.NET sunucu denetimi bir adlandırma kapsayıcısı olarak çalışması belirtmek için kullanılır. `INamingContainer` Arabirimi, sunucu denetimi uygulamalıdır herhangi bir yöntemin yazım değil; bunun yerine, bir işaretçi olarak kullanılır. Biçimlendirmenin oluşturmak, bir denetimi bu arabirimi uyguluyorsa ardından ASP.NET altyapısı otomatik olarak ekler, `ID` kendi alt öğelerini değerine işlenen `id` öznitelik değerleri. Bu işlem 2. adım daha ayrıntılı ele alınmıştır.


Adlandırma kapsayıcılar yalnızca değiştirme işlenen `id` öznitelik değeri, ancak nasıl denetim programlı olarak ASP.NET sayfa arka plan kod sınıfı başvurulabilir da etkiler. `FindControl("controlID")` Yöntemi program aracılığıyla bir Web denetime başvurmak için yaygın olarak kullanılır. Ancak, `FindControl` kapsayıcılarını adlandırmayla aracılığıyla nüfuz değil. Sonuç olarak, doğrudan kullanamazsınız `Page.FindControl` GridView veya diğer adlandırma kapsayıcısı içindeki denetimleri başvurmak için yöntemi.

Surmised gibi ana sayfalar ve ContentPlaceHolder hem de kapsayıcı adlandırma olarak uygulanır. Bu öğreticide nasıl ana sayfalarını etkiler HTML öğesi inceleyeceğiz `id` değerleri ve program aracılığıyla bir içerik sayfasını kullanarak içinde Web denetimleri başvuru yolları `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>1. adım: yeni bir ASP.NET sayfası ekleme

Bu öğreticide ele alınan kavramları göstermek için yeni bir ASP.NET sayfası için Web sitemizi ekleyelim. Adlı yeni bir içerik sayfası oluşturma `IDIssues.aspx` kök klasöründe için bağlama `Site.master` ana sayfa.


![İçerik sayfası IDIssues.aspx kök klasöre ekleyin.](control-id-naming-in-content-pages-vb/_static/image1.png)

**Şekil 01**: içerik sayfası Ekle `IDIssues.aspx` kök klasörüne


Visual Studio içerik denetimi her dört ana sayfanın ContentPlaceHolder için otomatik olarak oluşturur. Belirtilen [ *birden çok ContentPlaceHolder ve varsayılan içerik* ](multiple-contentplaceholders-and-default-content-vb.md) öğretici, bir içerik denetimi mevcut değilse ana sayfanın varsayılan ContentPlaceHolder içeriğinin yayılan yerine. Çünkü `QuickLoginUI` ve `LeftColumnContent` ContentPlaceHolder bu sayfa için uygun bir varsayılan biçimlendirme içerir, devam edin ve bunlara karşılık gelen kaldırmak içerik denetimlerini gelen `IDIssues.aspx`. Bu noktada, içerik sayfasının bildirim temelli biçimlendirme, aşağıdaki gibi görünmelidir:


[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample1.aspx)]

İçinde [ *ana sayfada başlık, Meta etiketler ve diğer HTML üst bilgilerini belirtme* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) özel taban sayfası sınıfı oluşturduk öğretici (`BasePage`), otomatik olarak yapılandırır başlığı ise açıkça ayarlayın. İçin `IDIssues.aspx` bu işlevselliği kullanmak istemiyorsunuz sayfasında, sayfanın arka plan kod sınıfı öğesinden türetilmelidir `BasePage` sınıfı (yerine `System.Web.UI.Page`). Aşağıdaki gibi görünür, böylece arka plan kod sınıfın tanımını değiştirin:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample2.vb)]

Son olarak, güncelleştirme `Web.sitemap` bu yeni ders için bir giriş eklemek için dosya. Ekleme bir `<siteMapNode>` öğesi ve kümesi kendi `title` ve `url` "İçin denetim kimliği adlandırma sorunları" öznitelikleri ve `~/IDIssues.aspx`sırasıyla. Bu ayrıca yaptıktan sonra `Web.sitemap` dosyanın biçimlendirme aşağıdakine benzer görünmelidir:


[!code-xml[Main](control-id-naming-in-content-pages-vb/samples/sample3.xml)]

Şekil 2 gösterildiği gibi yeni site haritası giriş `Web.sitemap` dersleri bölümünde sol sütunda anında yansıtılır.


![Ders bölümü artık bir bağlantı içerir &quot;denetim kimliği adlandırma sorunları&quot;](control-id-naming-in-content-pages-vb/_static/image2.png)

**Şekil 02**: dersleri bölüm "Sorunlarını denetim kimliği adlandırma" için bir bağlantı artık içerir


## <a name="step-2-examining-the-renderedidchanges"></a>2. adım: işlenen İnceleme`ID`değişiklikleri

ASP.NET değişiklikler daha iyi anlamak için altyapısı için işlenen yapar `id` değerlerini server denetimleri, birkaç Web denetimleri ekleyelim `IDIssues.aspx` sayfasında ve tarayıcıya gönderilen biçimlendirmenin görüntüleyin. Özellikle, metin türü "Lütfen yaşınızı girin:" metin Web denetim tarafından izlenen. Daha fazla aşağı sayfada bir düğme Web ve bir etiket Web denetimi ekleyin. Metin kutusunun ayarlamak `ID` ve `Columns` özelliklerine `Age` ve 3, sırasıyla. Düğmenin ayarlamak `Text` ve `ID` "Gönder" özelliklerine ve `SubmitButton`. Etiketin Temizle `Text` özelliği ve kümesi kendi `ID` için `Results`.

Bu noktada içerik denetiminizin bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample4.aspx)]

Şekil 3, Visual Studio'nun Tasarımcı görüntülendiğinde sayfada gösterilir.


[![Sayfanın üç Web denetimleri içerir: bir metin, düğme ve etiket](control-id-naming-in-content-pages-vb/_static/image4.png)](control-id-naming-in-content-pages-vb/_static/image3.png)

**Şekil 03**: sayfa ile içeren üç Web denetimleri: bir metin, düğme ve etiket ([tam boyutlu görüntüyü görmek için tıklatın](control-id-naming-in-content-pages-vb/_static/image5.png))


Bir tarayıcı aracılığıyla sayfasını ziyaret edin ve ardından HTML kaynağını görüntüleyin. Gösterir, aşağıda biçimlendirmesi olarak `id` değerler metin kutusu ve düğme etiket Web denetimleri için HTML öğelerinin birleşimi `ID` değerler Web denetimleri ve `ID` sayfasında adlandırma kapsayıcıların değerleri.


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample5.html)]

Bu öğreticide daha önce belirtildiği gibi ana sayfa hem kendi ContentPlaceHolder kapsayıcılarını adlandırmayla olarak hizmet eder. Sonuç olarak, her ikisi de işlenen katkıda `ID` değerleri, iç içe geçmiş denetim. Metin kutusunun ele `id` özniteliği, örneğin: `ctl00_MainContent_Age`. Bu geri çağırma TextBox denetiminin `ID` değerindeydi `Age`. Bu, ContentPlaceHolder denetimin ile önek `ID` değeri `MainContent`. Ayrıca, bu değer ana sayfa ile öneki `ID` değeri `ctl00`. Net etkisi bir `id` oluşan öznitelik değeri `ID` ana sayfaya, ContentPlaceHolder denetimi ve metin değerleridir.

Şekil 4, bu davranış gösterir. İşlenen belirlemek için `id` , `Age` metin ile başlangıç `ID` TextBox denetiminin değerini `Age`. Ardından, denetim hiyerarşisi'kurmak istediğiniz şekilde çalışın. İşlenen geçerli (turuncu renk ile düğümleri) her adlandırma kapsayıcıda, önek `id` adlandırma kapsayıcının ile `id`.


![Rendered ID özniteliklerine dayalı üzerinde kimliği adlandırma kapsayıcıların değerler](control-id-naming-in-content-pages-vb/_static/image6.png)

**Şekil 04**: işlenen `id` öznitelikleridir dayalı `ID` değerleri adlandırma kapsayıcısı


> [!NOTE]
> Ele aldığımız gibi `ctl00` işlenen kısmı `id` özniteliği oluşturan `ID` ana sayfaya, ancak değeri olsun nasıl bu `ID` değeri ortaya çıktığını. Biz bunu her yerden ana veya içerik sayfamızı belirtmedi. Bir ASP.NET sayfasında çoğu sunucu denetimleri bildirim temelli işaretleme sayfanın açıkça eklenir. `MainContent` ContentPlaceHolder denetimi biçimlerini açıkça belirtilmiş `Site.master`; `Age` TextBox tanımlandı `IDIssues.aspx`'s biçimlendirme. Biz belirtebilirsiniz `ID` bu tür denetimler, Özellikler penceresinden veya bildirim temelli söz dizimi için değerler. Ana sayfanın kendisi gibi başka denetimler de bildirim temelli biçimlendirme içinde tanımlı değil. Sonuç olarak, kendi `ID` değerleri otomatik olarak oluşturulmalıdır çözümdü. ASP.NET altyapısı kümeleri `ID` değerleri çalışma zamanında bu denetimlerin kimlikleri değil açık olarak ayarlandı. Adlandırma deseni kullanan `ctlXX`burada *XX* sıralı olarak artan bir tamsayı değerdir.


Ana sayfa için kendi hizmet etmesi bir adlandırma kapsayıcısı olarak, ana sayfada tanımlı Web denetimleri de işlenmiş değiştirmiş `id` öznitelik değerleri. Örneğin, `DisplayDate` ekledik ana sayfasına etiket [ *ana sayfalarıyla Site geneli bir düzen oluşturma* ](creating-a-site-wide-layout-using-master-pages-vb.md) öğretici aşağıdaki biçimlendirme işlenen vardır:


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample6.html)]

Unutmayın `id` özniteliği içeren iki ana sayfanın `ID` değeri (`ctl00`) ve `ID` etiket Web denetiminin değerini (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>3. adım: Program aracılığıyla Web denetimleri aracılığıyla başvurma`FindControl`

Her bir ASP.NET sunucu denetimi içeren bir `FindControl("controlID")` adlı bir denetim için denetimin yapıştırılamaz arayan yöntemi *ControlId*. Bu tür bir denetim bulunursa döndürülür; eşleşen hiçbir denetim bulunursa `FindControl` döndürür `Nothing`.

`FindControl` Burada bir denetim erişmeniz gerekir ancak kendisine doğrudan başvuru olmayan senaryolarda yararlı olur. Veri Web denetimleri bu gibi bir durumda GridView gibi ile çalışırken denetimleri GridView'ın alanları içinde bir kez bildirim temelli söz diziminde tanımlandı, ancak çalışma zamanında denetim örneği her GridView satır için oluşturulur. Sonuç olarak, çalışma zamanında oluşturulan denetimleri bulunur, ancak arka plan kod sınıfı kullanılabilir bir doğrudan başvuru yoktur. Sonuç olarak kullanılacak ihtiyacımız `FindControl` GridView'ın alanlar içinde belirli bir denetimi programlı bir şekilde çalışmak için. (Kullanma hakkında daha fazla bilgi için `FindControl` denetimleri veri Web denetim şablonları içinde erişmek için bkz: [özel biçimlendirme sırasında verileri](../../data-access/custom-formatting/custom-formatting-based-upon-data-vb.md).) Dinamik olarak Web denetimleri için Web formu eklerken aynı senaryo gerçekleşir, bir konuda ele [dinamik veri girişi kullanıcı arabirimleri oluşturmaya](https://msdn.microsoft.com/library/aa479330.aspx).

Kullanarak göstermek için `FindControl` bir içerik sayfasındaki denetimleri aramak için yöntemi oluşturmak için bir olay işleyicisi `SubmitButton`'s `Click` olay. Olay işleyicisi, programlı olarak başvuran aşağıdaki kodu ekleyin `Age` metin kutusu ve `Results` kullanarak etiket `FindControl` yöntemi ve bir ileti görüntüler `Results` kullanıcı girişini temel alarak.

> [!NOTE]
> Elbette, kullanılacak gerekmez `FindControl` Bu örnek için etiket ve metin denetimleri başvurmak için. Biz aracılığıyla doğrudan başvurabilirsiniz kendi `ID` özellik değerleri. Kullanmam `FindControl` kullanılırken neler göstermek için burada `FindControl` içerik sayfasından.


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample7.vb)]

While çağırmak için kullanılan sözdizimi `FindControl` yöntemi biraz farklıdır ilk iki satırını `SubmitButton_Click`, anlam olarak eşdeğerdir. Tüm ASP.NET sunucu denetimlerini içeren geri çağırma bir `FindControl` yöntemi. Bu içerir `Page` sınıfı, hangi tüm ASP.NET tarafından arka plan kod sınıfları öğesinden türetilmelidir. Bu nedenle, çağırma `FindControl("controlID")` çağırmakla eşdeğerdir `Page.FindControl("controlID")`, henüz geçersiz kılınan varsayılarak `FindControl` yöntemi, arka plan kod sınıfı veya özel bir temel sınıf.

Bu kodu girdikten sonra ziyaret `IDIssues.aspx` sayfasında bir tarayıcıdan yaşınızı girin ve "Gönder" düğmesine tıklayın. "Gönder" düğmesine tıkladığınızda bağlı bir `NullReferenceException` oluşturulur (bkz: Şekil 5).


[![NullReferenceException tetiklenir](control-id-naming-in-content-pages-vb/_static/image8.png)](control-id-naming-in-content-pages-vb/_static/image7.png)

**Şekil 05**: A `NullReferenceException` tetiklenir ([tam boyutlu görüntüyü görmek için tıklatın](control-id-naming-in-content-pages-vb/_static/image9.png))


Bir kesme noktası ayarlarsanız `SubmitButton_Click` hem çağrılar olduğunu göreceksiniz olay işleyicisi `FindControl` dönüş `Nothing`. `NullReferenceException` Erişmeye çalıştığında tetiklenir `Age` metin kutusunun `Text` özelliği.

Sorun `Control.FindControl` yalnızca arar *denetimi*kişinin aynı adlandırma kapsayıcı içinde bulunan yapıştırılamaz. Ana sayfayı yeni adlandırma bir kapsayıcı olan bir çağrı oluşturduğundan `Page.FindControl("controlID")` hiçbir zaman bir ana sayfa nesnesi permeates `ctl00`. (Geri gösteren denetim hiyerarşisi görüntülemek için Şekil 4'e başvuran `Page` ana sayfa nesnenin üst nesnesi `ctl00`.) Bu nedenle, `Results` etiket ve `Age` TextBox bulunamadı ve `ResultsLabel` ve `AgeTextBox` değerlerini atanan `Nothing`.

Bu sınama için iki geçici çözüm vardır: bir adlandırma kapsayıcısı, biz, uygun denetimi; bir zaman detaya gidebilirsiniz veya kendi oluşturabiliriz `FindControl` adlandırma kapsayıcıları permeates yöntemi. Bu seçeneklerin her birinin inceleyelim.

### <a name="drilling-into-the-appropriate-naming-container"></a>Uygun adlandırma kapsayıcıya araştırıp bulma

Kullanılacak `FindControl` başvurusuna `Results` etiket veya `Age` metin ihtiyacımız çağırmak `FindControl` aynı adlandırma kapsayıcıda üst denetimindeki. Şekil 4'te gösterilen şekilde `MainContent` ContentPlaceHolder denetimi, yalnızca üst `Results` veya `Age` aynı adlandırma kapsayıcısı içinde olmasıdır. Diğer bir deyişle, çağırma `FindControl` yönteminden `MainContent` denetimi, aşağıdaki kod parçacığında gösterildiği gibi doğru başvurusu döndürür `Results` veya `Age` kontrol eder.


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample8.vb)]

Ancak biz ile çalışamazsınız `MainContent` ContentPlaceHolder bizim içerik sayfasının arka plan kod sınıfından ContentPlaceHolder ana sayfada tanımlı olduğundan, yukarıdaki söz dizimini kullanarak. Bunun yerine, size kullanmak zorunda `FindControl` bir başvuru almak için `MainContent`. Değiştirin `SubmitButton_Click` olay işleyicisi aşağıdaki değişiklikler ile:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample9.vb)]

Bir tarayıcı aracılığıyla sayfasını ziyaret edin, yaşınızı girin ve "Gönder" düğmesine bir `NullReferenceException` tetiklenir. Bir kesme noktası ayarlarsanız `SubmitButton_Click` olay işleyicisi bu özel durum çağrı çalışılırken oluşuyor görürsünüz `MainContent` nesnenin `FindControl` yöntemi. `MainContent` Nesne eşittir `Nothing` çünkü `FindControl` yöntemi "MainContent" adlı bir nesne bulunamıyor. Temel nedeni ile aynıdır `Results` etiket ve `Age` TextBox denetimleri: `FindControl` , arama denetim hiyerarşisi başından başlar ve adlandırma kapsayıcıları nüfuz değil ancak `MainContent` ContentPlaceHolder olduğu ana sayfa içinde hangi adlandırma bir kapsayıcıdır.

Kullanabilmeniz için önce `FindControl` bir başvuru almak için `MainContent`, biz ilk ana sayfası denetimi başvuru gerekir. Biz ana sayfaya bir başvurusu oluşturduktan sonra bir başvuru almak `MainContent` aracılığıyla ContentPlaceHolder `FindControl` ve buradan başvurular `Results` etiket ve `Age` metin (aracılığıyla yeniden `FindControl`). Ancak biz bir başvuru ana sayfaya nasıl elde ederim? İnceleyerek `id` biçimlendirmenin öznitelikleri, yetkisiz değiştirmeye karşı korumalı, ana sayfanın `ID` değer `ctl00`. Bu nedenle, kullanabiliriz `Page.FindControl("ctl00")` ana sayfaya bir başvuru almak için sonra bu nesneye bir başvuru almak için kullanın `MainContent`ve benzeri. Aşağıdaki kod parçacığını bu mantığı gösterilmektedir:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample10.vb)]

Bu kod kesinlikle çalışır durumdayken olduğunu varsayar ve ana sayfadaki otomatik olarak oluşturulan `ID` her zaman `ctl00`. Otomatik olarak oluşturulan değerler hakkında varsayımlar için hiçbir zaman iyi bir fikirdir.

Neyse ki, bir başvuru ana sayfaya aracılığıyla erişilebilir `Page` sınıfın `Master` özelliği. Bu nedenle, kullanmak zorunda yerine `FindControl("ctl00")` erişmek için ana sayfanın bir başvuru almak için `MainContent` ContentPlaceHolder, bunun yerine kullanabileceğiniz `Page.Master.FindControl("MainContent")`. Güncelleştirme `SubmitButton_Click` olay işleyicisi aşağıdaki kod ile:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample11.vb)]

Bu kez, bir tarayıcı aracılığıyla sayfasını ziyaret ederek yaşınızı girme ve "Gönder" düğmesine tıkladığınızda iletisi görüntüler `Results` beklendiği gibi etiketleyin.


[![Kullanıcının yaşını etikette görüntülenir](control-id-naming-in-content-pages-vb/_static/image11.png)](control-id-naming-in-content-pages-vb/_static/image10.png)

**Şekil 06**: Kullanıcının yaşını etikette görüntülenir ([tam boyutlu görüntüyü görmek için tıklatın](control-id-naming-in-content-pages-vb/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>Yinelemeli olarak kapsayıcılarını adlandırmayla aracılığıyla aranıyor

Başvurulan bir önceki kod örneğinde nedeni `MainContent` ContentPlaceHolder denetiminden ana sayfa ve ardından `Results` etiket ve `Age` TextBox denetimleri gelen `MainContent`, çünkü `Control.FindControl` yöntemi yalnızca arar içinde *denetimi*kapsayıcı adlandırma. Sahip `FindControl` adlandırma kapsayıcısı içinde kalın anlamlı Çoğu senaryoda iki denetimlerinde iki farklı adlandırma kapsayıcı aynı olabileceğinden `ID` değerleri. Adlı bir etiket Web denetimi tanımlayan GridView'ın bir durum düşünün `ProductName` kendi TemplateField biri. Verileri zamanında GridView bağlandığında bir `ProductName` etiket, her GridView satır için oluşturulur. Varsa `FindControl` tüm adlandırma aracılığıyla Aranan kapsayıcılar ve adlı `Page.FindControl("ProductName")`, hangi etiket örneği gerekir `FindControl` döndürür? `ProductName` Etiket ilk GridView satırında? Bir son sıradaki?

Bu nedenle sahip `Control.FindControl` yalnızca arama *denetimi*adlandırma kapsayıcısı alanı anlamlı çoğu durumda. Vardır, ancak diğer durumlarda, bize benzersiz bir sahip olduğumuz bakan bir gibi `ID` tamamında adlandırma kapsayıcılar ve gelen Denetim erişim denetimi hiyerarşideki her adlandırma kapsayıcısı başvurmak zorunda kalmamak istiyor. Sahip bir `FindControl` tüm adlandırma kapsayıcıları yapar yinelemeli olarak arar mantıklı, çok değişken. Ne yazık ki, .NET Framework gibi bir yöntem içermez.

Kendi oluşturabiliriz, güzel bir haberimiz var olan `FindControl` yöntemi tüm adlandırma kapsayıcıları, yinelemeli olarak arar. Aslında, kullanarak *genişletme yöntemleri* biz pus bir `FindControlRecursive` yönteme `Control` sınıfı, varolan eşlik eden `FindControl` yöntemi.

> [!NOTE]
> Genişletme yöntemleri bir yeni bir .NET Framework sürüm 3.5 ve Visual Studio 2008 ile birlikte gönderilen diller C# 3.0 ve Visual Basic 9 özelliğidir. Kısacası, özel bir söz dizimi aracılığıyla var olan bir sınıf türü için yeni bir yöntem oluşturmak için geliştirici için genişletme yöntemleri sağlar. Bu yararlı özelliğe ilişkin daha fazla bilgi için Bilgisayarım makaleye bakın [genişletme yöntemleri ile temel türü işlevselliği genişletme](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Genişletme yöntemi oluşturmak için yeni bir dosyaya ekleme `App_Code` adlı klasöre `PageExtensionMethods.vb`. Adlı bir uzantı ekleme metodu `FindControlRecursive` girdi olarak alır bir `String` adlı parametre `controlID`. Genişletme yöntemleri düzgün çalışması sınıf olarak işaretlenmesi önemli bir `Module` ve uzantı yöntemlerini önekini almalıdır `<Extension()>` özniteliği. Ayrıca, tüm uzantı yöntemleri kendi ilk parametre olarak bir nesne türü kabul etmelisiniz genişletme yöntemini uygulandığı için.

Aşağıdaki kodu ekleyin `PageExtensionMethods.vb` bu tanımlamak için dosya `Module` ve `FindControlRecursive` genişletme yöntemi:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample12.vb)]

Yerinde şu kodla dönmek `IDIssues.aspx` sayfa arka plan kod sınıfı ve geçerli bir yorum `FindControl` yöntemi çağırır. Bunları çağrılarıyla değiştirin `Page.FindControlRecursive("controlID")`. Uzantı yöntemleri hakkında NET nedir, doğrudan IntelliSense açılan listeler içinde görünürler olduğu. Şekil 7 gösterildiği gibi yazarken `Page` ve ardından süresi, isabet `FindControlRecursive` yöntemi yanı sıra diğer aşağı açılan IntelliSense dahil `Control` sınıfı yöntemleri.


[![Genişletme yöntemleri, IntelliSense açılan menülerde dahildir](control-id-naming-in-content-pages-vb/_static/image14.png)](control-id-naming-in-content-pages-vb/_static/image13.png)

**Şekil 07**: genişletme yöntemleri, IntelliSense açılan menülerde eklenir ([tam boyutlu görüntüyü görmek için tıklatın](control-id-naming-in-content-pages-vb/_static/image15.png))


Aşağıdaki kodu girin `SubmitButton_Click` olay işleyicisi ve sayfasını ziyaret ederek yaşınızı girme ve "Gönder" düğmesine tıklayarak test edin. Şekil 6'da gösterildiği gibi elde edilen çıkış iletisi, "Yaş yaşında yaşa!"


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample13.vb)]

> [!NOTE]
> Visual Studio 2005 kullanıyorsanız C# 3.0 ve Visual Basic 9 için yeni uzantı yöntemleri için genişletme yöntemleri kullanamazsınız. Bunun yerine, uygulama gerekecektir `FindControlRecursive` yöntemi bir yardımcı sınıfı. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) kendi blog gönderisinde, böyle bir örneği olan [ASP.NET Mazer sayfaları ve `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>4. adım: doğru kullanma`id`öznitelik değeri istemci tarafı komut dosyası

Bu öğreticinin giriş belirtildiği gibi bir Web denetimi işlenen `id` özniteliği önerilmesine istemci tarafı betikte program aracılığıyla belirli bir HTML öğesi başvurmak için kullanılır. Örneğin, aşağıdaki JavaScript HTML öğesi tarafından başvuruda kendi `id` ve değerini bir kalıcı ileti kutusunda görüntüler:


[!code-csharp[Main](control-id-naming-in-content-pages-vb/samples/sample14.cs)]

Bu sayfa, ASP.NET geri çağırma bir adlandırma kapsayıcısı, İşlenmiş HTML öğesi içermiyor `id` özniteliktir Web denetimi için aynı `ID` özellik değeri. Bu nedenle, sabit kod daha cazip `id` öznitelik değerlerini JavaScript kodu. Biliyorsanız, diğer bir deyişle, erişmek istediğiniz `Age` TextBox Web denetimi istemci tarafı betiği aracılığıyla bunu çağrısıyla `document.getElementById("Age")`.

Bu yaklaşım, ana sayfalar (veya diğer adlandırma kapsayıcısı denetimleri) kullanırken sorundur İşlenmiş HTML `id` Web denetimi ile eşanlamlı değil `ID` özelliği. Bir tarayıcı aracılığıyla sayfasını ziyaret edin ve gerçek belirlemek için kaynak görüntülemek için ilk Eğim olabilir `id` özniteliği. İşlenen öğrendikten sonra `id` değeri yapıştırabilirsiniz bu çağrıya dönüştürür `getElementById` istemci tarafı komut dosyası çalışmanız gereken HTML öğesine erişmek için. Bu yaklaşım sayfanın bazı değişiklikler hiyerarşi denetlediğinden küçüktür idealdir veya değişikliklerini `ID` adlandırma denetimlerin özelliklerini, ortaya çıkan olmadığını alter `id` böylece JavaScript kodunuza kesme özniteliği.

Güzel bir haberimiz var olan `id` işlenen öznitelik değeri, sunucu tarafı kod Web denetiminin üzerinden erişilebilir [ `ClientID` özelliği](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). Belirlemek için bu özelliği kullanması gereken `id` öznitelik değeri istemci tarafı betikte kullanılır. Örneğin, sayfaya JavaScript işlevi eklemek için çağrıldığında, değerini görüntüler `Age` kalıcı ileti kutusunda, metin kutusuna aşağıdaki kodu ekleyin `Page_Load` olay işleyicisi:


[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample15.vb)]

Yukarıdaki kodu değerini ekler `Age` metin kutusunun `ClientID` JavaScript çağrısı özelliğine `getElementById`. Bir tarayıcı aracılığıyla bu sayfasını ziyaret edin ve HTML kaynağını görüntülemek, aşağıdaki JavaScript kodunu bulabilirsiniz:


[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample16.html)]

Bildirim nasıl doğru `id` öznitelik değeri, `ctl00_MainContent_Age`, çağrısındaki görünür `getElementById`. Bu değer, çalışma zamanında hesaplandığından, sonraki değişiklikler sayfası denetimi hiyerarşi için bağımsız olarak çalışır.

> [!NOTE]
> Bu JavaScript örnek yalnızca doğru sunucu denetimi tarafından oluşturulan HTML öğesine başvuran bir JavaScript işlevi ekleme gösterir. Bu işlevi kullanmak için belge yüklendiğinde veya bazı belirli bir kullanıcı eylemi transpires işlevi çağırmak için ek JavaScript Yazar gerekecektir. Bunlar hakkında daha fazla bilgi ve ilgili konuları okuyun, [ile istemci tarafı komut dosyası çalışma](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Özet

Belirli ASP.NET sunucu denetimleri işlenen etkiler adlandırma kapsayıcıları olarak davranan `id` öznitelik değerleri kendi alt denetimlerin yanı sıra kapsamı tarafından canvassed denetimlerin `FindControl` yöntemi. Ana sayfa hem kendi ContentPlaceHolder denetimleri ana sayfalar bakımından kapsayıcılarını adlandırmayla. Sonuç olarak, içerik sayfasını kullanarak içinde denetimlerini programlı olarak başvurmak için biraz daha ileri çalışma moduna ihtiyacımız `FindControl`. Biz bu öğreticide iki teknik incelenir: ContentPlaceHolder denetimine ayrıntıya gitme ve arama kendi `FindControl` yöntemi; ve kendi çalışırken `FindControl` uygulama bu yinelemeli olarak tüm adlandırma kapsayıcılar arar.

Sunucu tarafı sorunları adlandırma kapsayıcılar Web denetimlere başvurma bakımından tanıtmak yanı sıra ayrıca istemci-tarafı sorunları vardır. Kapsayıcılar, Web denetimi 's adlandırma olmadığında `ID` özellik değeri ve işlenen `id` öznitelik değeri, bir aynı. Ancak ek adlandırma kapsayıcısı, işlenen olarak `id` özniteliği hem de içeren `ID` Web denetimi ve kendi denetimi hiyerarşisinin ailesi içinde adlandırma kapsayıcılarda değerlerini. Web denetimin kullandığınız sürece bu adlandırma sorunu olmayan edilir `ClientID` işlenen belirlemek için özellik `id` öznitelik değeri, istemci tarafı komut dosyası.

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET ana sayfaları ve `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Dinamik veri girişi kullanıcı arabirimi oluşturma](https://msdn.microsoft.com/library/aa479330.aspx)
- [Temel tür işlevselliği genişletme yöntemleri ile genişletme](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Nasıl yapılır: ASP.NET ana sayfası içeriği başvurusu](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Traps mater sayfaları: İpuçları ve püf noktaları](http://www.odetocode.com/articles/450.aspx)
- [İstemci tarafı komut dosyası ile çalışma](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar 1998'de bu yana birden çok ASP/ASP.NET books ve poshbeauty.com sitesinin 4GuysFromRolla.com, Microsoft Web teknolojileri ile çalışmaktadır. Scott, bağımsız Danışman, Eğitimci ve yazıcı çalışır. En son nitelemiştir olan [ *Unleashed'i öğretin kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott, konumunda ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog'da aracılığıyla [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı gözden geçirenler Zack Jones ve Suchi Barnerjee yoktu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Önceki](urls-in-master-pages-vb.md)
> [İleri](interacting-with-the-master-page-from-the-content-page-vb.md)
