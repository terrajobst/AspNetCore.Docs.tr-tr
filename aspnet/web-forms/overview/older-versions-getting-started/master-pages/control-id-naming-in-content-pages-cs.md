---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
title: "Denetim Kimliği içerik sayfalarında (C#) adlandırma | Microsoft Docs"
author: rick-anderson
description: "Nasıl ContentPlaceHolder denetimleri adlandırma kapsayıcı olarak hizmet ve bu nedenle program aracılığıyla (FindConrol) zor bir denetimiyle çalıştığını gösterir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 1c7d0916-0988-4b4f-9a03-935e4b5af6af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 0c0db7fd76a7a486ff45085329ef7c77b0af5ebe
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="control-id-naming-in-content-pages-c"></a>İçerik sayfaları (C#) adlandırma denetim kimliği
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_CS.pdf)

> Nasıl ContentPlaceHolder denetimleri adlandırma kapsayıcı olarak hizmet ve bu nedenle program aracılığıyla (FindConrol) zor bir denetimiyle çalıştığını gösterir. Bu sorun ve geçici çözümler arar. Ayrıca sonuç ClientID değeri programlı olarak erişmek nasıl anlatılmaktadır.


## <a name="introduction"></a>Giriş

Tüm ASP.NET sunucu denetimleri içeren bir `ID` olarak denetimi program aracılığıyla erişilir arka plan kodu sınıfında anlamına gelir benzersiz olarak denetimi tanımlayan ve özellik. Benzer şekilde, bir HTML belgesi öğeleri içerebilir bir `id` öğeyi benzersiz olarak tanıtan özniteliği; bu `id` değerleri genellikle istemci tarafı komut dosyası programlı olarak belirli bir HTML öğesi başvurmak için kullanılır. Bir ASP.NET sunucu denetimi HTML işlendiğinde bu verildiğinde, size, varsayabilir kendi `ID` değeri olarak kullanıldığından `id` işlenen HTML öğesinin değeri. Bazı durumlarda tek bir denetim tek bir tıklatmayla bu mutlaka durumda olmadığından `ID` değeri oluşturulan biçimlendirmenin birden çok kez görünebilir. Bir etiket Web denetimiyle birlikte bir TemplateField içeren GridView denetimini göz önünde bulundurun bir `ID` ProductName değeri. GridView, çalışma zamanında kendi veri kaynağına bağlandığında, bu etiket her GridView satır için bir kez yinelenir. Her etiket gereksinimlerini benzersiz bir işlenen `id` değeri.

Bu tür senaryoların işlemek için ASP.NET kapsayıcı adlandırma olarak gösterilen bazı denetimleri sağlar. Adlandırma kapsayıcısı yeni bir hizmet `ID` ad alanı. Adlandırma kapsayıcılarında görünme tüm sunucu denetimlerinin kendi işlenmiş olan `id` değeri önekiyle `ID` adlandırma kapsayıcı denetiminin. Örneğin, `GridView` ve `GridViewRow` sınıflardır hem adlandırma kapsayıcıları. Sonuç olarak, GridView TemplateField ile tanımlanan bir etiket denetimi `ID` ProductName işlenmiş verilen `id` değerini `GridViewID_GridViewRowID_ProductName`. Çünkü *GridViewRowID* elde edilen her GridView satır için benzersizdir `id` değerleri benzersizdir.

> [!NOTE]
> [ `INamingContainer` Arabirimi](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) belirli ASP.NET sunucu denetimi adlandırma kapsayıcı olarak çalışması belirtmek için kullanılır. `INamingContainer` Arabirimi sunucu denetimi uygulamalıdır herhangi bir yöntem yazım değil; bunun yerine, bir işaretçi olarak kullanılır. Oluşturulan biçimlendirmenin oluşturmak, bir denetim bu arabirim uyguluyorsa sonra ASP.NET altyapısı otomatik olarak önekleri kendi `ID` alt öğelerinden değerine çizilir `id` öznitelik değerlerini. Bu işlem, adım 2'deki daha ayrıntılı ele alınmıştır.


Adlandırma kapsayıcılar yalnızca değiştirme işlenen `id` öznitelik değeri, ancak nasıl denetimi programlı olarak ASP.NET sayfa arka plan kodu sınıfından başvurulabilir de etkiler. `FindControl("controlID")` Yöntemi program aracılığıyla bir Web denetimi başvurmak için yaygın olarak kullanılır. Ancak, `FindControl` kapsayıcı adlandırma ile sızmasını değil. Sonuç olarak, doğrudan kullanamazsınız `Page.FindControl` GridView veya diğer adlandırma kapsayıcısı içinde denetimleri başvurmak için yöntem.

Surmised gibi ana sayfalar ve ContentPlaceHolders için hem de kapsayıcı adlandırma olarak uygulanır. Bu öğreticide inceleyeceğiz nasıl ana sayfalar etkileyen HTML öğesi `id` değerleri ve Web denetimleri kullanarak bir içerik sayfasının içinde program aracılığıyla başvuru yolları `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>1. adım: yeni bir ASP.NET sayfası ekleme

Bu öğreticide açıklanan kavramları göstermek için yeni bir ASP.NET sayfası bizim Web sitesine ekleyelim. Adlı yeni bir içerik sayfası oluşturun `IDIssues.aspx` kök klasöründe kendisine bağlama `Site.master` ana sayfa.


![İçerik sayfasını IDIssues.aspx kök klasörüne ekleyin](control-id-naming-in-content-pages-cs/_static/image1.png)

**Şekil 01**: içerik sayfası Ekle `IDIssues.aspx` kök klasörüne


Visual Studio içerik denetimi her dört ContentPlaceHolders için ana sayfa için otomatik olarak oluşturur. İçinde belirtildiği gibi [ *birden çok ContentPlaceHolders için ve varsayılan içerik* ](multiple-contentplaceholders-and-default-content-cs.md) Öğreticisi, içerik denetimi mevcut değilse ana sayfa varsayılan ContentPlaceHolder içeriğini yayılan yerine. Çünkü `QuickLoginUI` ve `LeftColumnContent` ContentPlaceHolders için bu sayfa için uygun bir varsayılan biçimlendirme içeren, bir tane karşılık gelen kaldırmak içerik denetimleri `IDIssues.aspx`. Bu noktada, içerik sayfasının bildirim temelli biçimlendirme, aşağıdaki gibi görünmelidir:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample1.aspx)]

İçinde [ *başlık, Meta etiketler ve diğer HTML üstbilgileri ana sayfasında belirtme* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) özel taban sayfası sınıfı oluşturduğumuz öğretici (`BasePage`), otomatik olarak yapılandırır sayfanın başlığı, açıkça ayarlayın. İçin `IDIssues.aspx` bu işlevselliği kullanmayı sayfasında, sayfanın arka plandaki kod sınıfı öğesinden türetilmelidir `BasePage` sınıfı (yerine `System.Web.UI.Page`). Böylece aşağıdaki gibi görünür arka plan kodu sınıfının tanımını değiştirin:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample2.cs)]

Son olarak, güncelleştirme `Web.sitemap` dosya bu yeni ders için bir giriş içerir. Ekleme bir `<siteMapNode>` öğesi ve kümesi kendi `title` ve `url` öznitelikleri "İçin denetim kimliği adlandırma sorunları" ve `~/IDIssues.aspx`sırasıyla. Bu ek yaptıktan sonra `Web.sitemap` dosyanın biçimlendirme aşağıdakine benzer görünmelidir:


[!code-xml[Main](control-id-naming-in-content-pages-cs/samples/sample3.xml)]

Şekil 2 gösterildiği gibi yeni site haritası girişte `Web.sitemap` sol sütunda dersleri bölümdeki hemen yansıtılır.


![Dersleri bölümü artık bir bağlantı içerir &quot;denetim sorunları adlandırma kimliği&quot;](control-id-naming-in-content-pages-cs/_static/image2.png)

**Şekil 02**: dersleri bölüm şimdi "Denetim kimliği sorunları adlandırma" bağlantısı içerir


## <a name="step-2-examining-the-renderedidchanges"></a>2. adım: işlenen inceleniyor`ID`değişiklikleri

ASP.NET değişiklikler daha iyi anlamak için altyapısı işlenmiş yapar `id` değerleri sunucusunun denetimleri, birkaç Web denetimleri ekleyelim `IDIssues.aspx` sayfasında ve daha sonra tarayıcıya gönderilen oluşturulan biçimlendirmenin görüntüleyin. Özellikle, metin türü "Lütfen yaşınızı girin:" metin kutusuna Web denetimi tarafından izlenen. Daha fazla aşağı sayfada bir düğme Web ve etiket Web denetimi ekleyin. TextBox's ayarlamak `ID` ve `Columns` özelliklerine `Age` ve 3 ' ü sırasıyla. Düğmenin ayarlamak `Text` ve `ID` "Gönderme" özelliklerine ve `SubmitButton`. Etiketin Temizle `Text` özelliği ve kümesi kendi `ID` için `Results`.

Bu noktada içerik denetiminizin bildirim temelli biçimlendirme aşağıdakine benzer görünmelidir:


[!code-aspx[Main](control-id-naming-in-content-pages-cs/samples/sample4.aspx)]

Şekil 3'te Visual Studio'nun designer üzerinden görüntülendiğinde sayfası gösterir.


[![Sayfanın üç Web denetimleri içerir: bir metin kutusuna, düğme ve etiket](control-id-naming-in-content-pages-cs/_static/image4.png)](control-id-naming-in-content-pages-cs/_static/image3.png)

**Şekil 03**: sayfa ile içeren üç Web denetimleri: bir metin kutusuna, düğmesi ve etiket ([tam boyutlu görüntüyü görüntülemek için tıklatın](control-id-naming-in-content-pages-cs/_static/image5.png))


Bir tarayıcı aracılığıyla sayfasını ziyaret edin ve ardından HTML kaynağını görüntüleyin. Gösterir, aşağıda biçimlendirmesi olarak `id` metin kutusuna, düğme ve etiket Web denetimleri için HTML öğeleri değerleri bileşimini `ID` Web denetimleri değerlerini ve `ID` sayfasındaki adlandırma kapsayıcıların değerleri.


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample5.html)]

Bu öğreticide daha önce belirtildiği gibi ana sayfa ve kendi ContentPlaceHolders için kapsayıcı adlandırma olarak hizmet eder. Sonuç olarak, her ikisi de işlenen katkıda `ID` değerleri kendi iç içe geçmiş denetim. Metin kutusu'nın ele `id` özniteliği, örneğin: `ctl00_MainContent_Age`. Sözcüğünün metin kutusu denetiminin `ID` değeri `Age`. Bu, ContentPlaceHolder denetimin ile konduğundan `ID` değeri `MainContent`. Ayrıca, bu değer ana sayfanın ile konduğundan `ID` değeri `ctl00`. Net etkisi bir `id` oluşan öznitelik değeri `ID` ana sayfa, ContentPlaceHolder denetimi ve metin değerleridir.

Şekil 4, bu davranış gösterilmektedir. İşlenen belirlemek için `id` , `Age` metin kutusuna, başlangıç ile `ID` TextBox denetimi değerini `Age`. Ardından, Denetim hiyerarşi, şekilde çalışır. Her adlandırma kapsayıcısı (turuncu renkle düğümleri), işlenen geçerli önek `id` adlandırma kapsayıcının ile `id`.


![Rendered ID özniteliklerine dayalı üzerinde kimliği adlandırma kapsayıcıları değerleri](control-id-naming-in-content-pages-cs/_static/image6.png)

**Şekil 04**: çizilir `id` öznitelikleridir temel alınan `ID` adlandırma kapsayıcıların değerleri


> [!NOTE]
> Biz anlatıldığı gibi `ctl00` işlenen kısmı `id` özniteliği oluşturduğunu `ID` ana sayfa, ancak değerini merak ediyor nasıl bu `ID` değeri ortaya çıktığını. Biz, herhangi bir yere master veya içerik sayfamızı belirtmedi. Bir ASP.NET sayfası, çoğu sunucu denetimleri açıkça sayfanın bildirim temelli biçimlendirme eklenir. `MainContent` ContentPlaceHolder denetimi biçimlendirme içinde açıkça belirtilmiş `Site.master`; `Age` TextBox tanımlanmıştı `IDIssues.aspx`'s biçimlendirme. Biz belirtebilirsiniz `ID` bu tür denetimler Özellikler penceresini aracılığıyla veya tanımlayıcı sözdizimi için değerler. Ana sayfa kendisini gibi diğer denetimleri bildirim temelli biçimlendirme içinde tanımlı değil. Sonuç olarak, kendi `ID` değerleri otomatik olarak oluşturulmalıdır bize. ASP.NET altyapısı kümeleri `ID` olan kimlikleri değil açıkça ayarlanmış bu denetimleri için çalışma zamanında değerleri. Adlandırma deseni kullanır `ctlXX`, burada *XX* sıralı olarak artan bir tamsayı değil.


Ana sayfa için kendisini görevi görür adlandırma kapsayıcı olarak, ana sayfa içinde tanımlanan Web denetimleri de işlenmiş değiştirmiş `id` öznitelik değerlerini. Örneğin, `DisplayDate` eklediğimiz ana sayfası etiket [ *ana sayfalar Site genelinde düzen oluşturma* ](creating-a-site-wide-layout-using-master-pages-cs.md) öğretici aşağıdaki biçimlendirme çizilir vardır:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample6.html)]

Unutmayın `id` özniteliğini içeren her iki ana sayfa `ID` değeri (`ctl00`) ve `ID` etiket Web denetimi değerini (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>3. adım: Program aracılığıyla Web denetimleri aracılığıyla başvurma`FindControl`

Her ASP.NET sunucu denetimi içeren bir `FindControl("controlID")` adlı bir denetim için denetimin alt arayan yöntemi *ControlId*. Böyle bir denetim bulunursa, döndürülür; eşleşen hiçbir denetim bulunursa, `FindControl` döndürür `null`.

`FindControl`Burada bir denetim erişmesi gerekiyor ancak doğrudan referansı bulunmuyor senaryolarda kullanışlıdır. GridView'ın alanları içindeki denetimler Web denetimleri bu gibi bir durumda GridView gibi verilerle çalışırken, bir kez bildirim temelli sözdiziminde tanımlanır, ancak çalışma zamanında her GridView satır için denetim örneği oluşturulur. Sonuç olarak, çalışma zamanında oluşturulan denetimler mevcut, ancak arka plan kodu sınıfından kullanılabilir doğrudan bir başvuru yok. Sonuç olarak kullanmak ihtiyacımız `FindControl` GridView'ın alanları içinde belirli bir denetim programlı olarak çalışmak için. (Kullanma hakkında daha fazla bilgi için `FindControl` veri Web denetiminin şablonları içindeki denetimler erişmek için bkz: [özel biçimlendirme göre bağlı verileri](../../data-access/custom-formatting/custom-formatting-based-upon-data-cs.md).) Dinamik olarak Web denetimleri için Web formu eklerken aynı bu senaryo ortaya, bir konu ele [dinamik veri girişi kullanıcı arabirimleri oluşturma](https://msdn.microsoft.com/library/aa479330.aspx).

Kullanarak göstermeye `FindControl` bir içerik sayfasını içinde denetimleri aranacak yöntemi oluşturmak için bir olay işleyicisi `SubmitButton`'s `Click` olay. Olay işleyicisi programlı olarak başvuran aşağıdaki kodu ekleyin `Age` TextBox ve `Results` kullanarak etiket `FindControl` yöntemi ve bir ileti görüntüler `Results` kullanıcının girişinize göre.

> [!NOTE]
> Elbette, biz kullanmanız gerekmez `FindControl` etiket ve metin kutusuna bu örnek için denetimleri başvurmak için. Biz doğrudan aracılığıyla başvurabilir kendi `ID` özellik değerleri. Kullandığım `FindControl` kullanırken olanlar göstermek için burada `FindControl` içerik sayfasından.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample7.cs)]

While çağırmak için kullanılan sözdizimi `FindControl` yöntemi biraz farklıdır ilk iki satırda `SubmitButton_Click`, anlam olarak eşdeğerdir. Tüm ASP.NET sunucu denetimleri dahil geri çağırma bir `FindControl` yöntemi. Bu içerir `Page` sınıfı, hangi tüm ASP.NET tarafından arka plan kodu sınıfları öğesinden türetilmelidir. Bu nedenle, çağırma `FindControl("controlID")` arama için eşdeğer bir gruba `Page.FindControl("controlID")`, henüz geçersiz kılınmış varsayılarak `FindControl` yöntemi, arka plandaki kod sınıfı veya özel bir temel sınıf.

Bu kodu girdikten sonra ziyaret `IDIssues.aspx` sayfasında bir tarayıcıdan, yaşınızı girin ve "Gönderme" düğmesini tıklatın. "Gönder" düğmesini tıklatarak bağlı bir `NullReferenceException` tetiklenir (bkz. Şekil 5).


[![NullReferenceException tetiklenir](control-id-naming-in-content-pages-cs/_static/image8.png)](control-id-naming-in-content-pages-cs/_static/image7.png)

**Şekil 05**: A `NullReferenceException` tetiklenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](control-id-naming-in-content-pages-cs/_static/image9.png))


Bir kesme noktası ayarlarsanız `SubmitButton_Click` görürsünüz: her ikisi de çağırdığı için olay işleyicisini `FindControl` dönüş bir `null` değeri. `NullReferenceException` Erişmeye çalıştığında tetiklenir `Age` TextBox'ın `Text` özelliği.

Sorun `Control.FindControl` yalnızca arar *denetim*ait olduğu alt *aynı adlandırma kapsayıcısında*. Ana sayfaya yeni bir adlandırma kapsayıcı, bir çağrı oluşturduğundan `Page.FindControl("controlID")` hiçbir zaman bir ana sayfa nesnesi permeates `ctl00`. (Geri gösterir denetim hiyerarşisi görüntülemek için Şekil 4'e bakın `Page` ana sayfa nesnenin üst nesnesi `ctl00`.) Bu nedenle, `Results` etiket ve `Age` TextBox bulunamadı ve `ResultsLabel` ve `AgeTextBox` değerlerini atanan `null`.

Bu sorunu için iki geçici çözüm vardır: biz uygun denetime; her seferinde bir adlandırma kapsayıcısı aşağı inebilir ya da kendi oluşturabilir `FindControl` adlandırma kapsayıcıları permeates yöntemi. Bu seçeneklerin her biri inceleyelim.

### <a name="drilling-into-the-appropriate-naming-container"></a>Uygun adlandırma kapsayıcıya ayrıntılara

Kullanılacak `FindControl` başvuru `Results` etiketi veya `Age` metin kutusuna, ihtiyacımız çağırmak `FindControl` aynı adlandırma kapsayıcısında üst denetimindeki. Şekil 4'te gösterilen şekilde `MainContent` ContentPlaceHolder denetimi, yalnızca üst öğesi olan `Results` veya `Age` aynı adlandırma kapsayıcısı içinde olmasıdır. Diğer bir deyişle, çağırma `FindControl` yönteminden `MainContent` denetimi, aşağıdaki kod parçacığında gösterildiği gibi doğru döndürür başvuru `Results` veya `Age` kontrol eder.


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample8.cs)]

Ancak, biz ile çalışamaz `MainContent` ContentPlaceHolder bizim içerik sayfasının arka plan kodu sınıfından ContentPlaceHolder ana sayfada tanımlı olduğu için yukarıdaki söz dizimini kullanarak. Bunun yerine, biz kullanmak zorunda `FindControl` bir başvuru almak için `MainContent`. Kodla `SubmitButton_Click` aşağıdaki değişiklikleri ile olay işleyicisi:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample9.cs)]

Bir tarayıcı aracılığıyla sayfasını ziyaret edin, yaşınızı girin ve "Gönderme" düğmesini tıklatın bir `NullReferenceException` tetiklenir. Bir kesme noktası ayarlarsanız `SubmitButton_Click` görürsünüz: Bu özel durum çağrı çalışılırken zaman oluştuğunu olay işleyicisi `MainContent` nesnesinin `FindControl` yöntemi. `MainContent` Nesne `null` çünkü `FindControl` yöntemi "MainContent" adlı bir nesne bulunamıyor. Temel nedeni ile aynıdır `Results` etiket ve `Age` TextBox denetimleri: `FindControl` denetim hiyerarşisi üstten, aramayı başlatır ve adlandırma kapsayıcıları sızmasını değil ancak `MainContent` ContentPlaceHolder olduğu ana sayfa içinde hangi adlandırma bir kapsayıcıdır.

Biz kullanmadan önce `FindControl` bir başvuru almak için `MainContent`, ilk ana sayfa denetlemek için bir başvuru ihtiyacımız. Ana sayfaya başvuru sahibiz sonra bir başvuru elde edebilirsiniz `MainContent` ContentPlaceHolder aracılığıyla `FindControl` ve buradan başvurular `Results` etiket ve `Age` TextBox (kullanarak aracılığıyla yeniden `FindControl`). Ancak biz ana sayfaya başvuru nereden? İnceleme tarafından `id` oluşturulan biçimlendirmenin öznitelikleri olduğu korumalı, ana sayfa `ID` değer `ctl00`. Bu nedenle, biz kullanabilirsiniz `Page.FindControl("ctl00")` ana sayfaya bir başvuru almak için ardından bu nesne bir başvuru almak için kullanın `MainContent`ve benzeri. Aşağıdaki kod parçacığında bu mantığı gösterilmektedir:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample10.cs)]

Bu kod kesinlikle çalışır durumdayken varsayılmaktadır ana sayfanın otomatik olarak oluşturulur `ID` her zaman açık `ctl00`. Otomatik olarak oluşturulur değerleri hakkında varsayımlarda için hiçbir zaman iyi bir fikirdir.

Neyse ki, bir ana sayfa üzerinden erişilebilir başvurusudur `Page` sınıfının `Master` özelliği. Bu nedenle, kullanmak zorunda yerine `FindControl("ctl00")` erişmek için ana sayfa bir başvuru almak için `MainContent` ContentPlaceHolder, bunun yerine kullanabileceğiniz `Page.Master.FindControl("MainContent")`. Güncelleştirme `SubmitButton_Click` aşağıdaki kod ile olay işleyicisi:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample11.cs)]

Bu süre, bir tarayıcı aracılığıyla sayfasını ziyaret yaşınızı girme ve "Gönderme" düğmesini tıklatarak iletisi görüntüler `Results` beklendiği şekilde etiketleyin.


[![Kullanıcının yaş etiketinde görüntülenir](control-id-naming-in-content-pages-cs/_static/image11.png)](control-id-naming-in-content-pages-cs/_static/image10.png)

**Şekil 06**: kullanıcının yaş etiketinde görüntülenir ([tam boyutlu görüntüyü görüntülemek için tıklatın](control-id-naming-in-content-pages-cs/_static/image12.png))


### <a name="recursively-searching-through-naming-containers"></a>Yinelemeli olarak kapsayıcı adlandırma ile arama

Başvurulan bir önceki kod örneğinde neden `MainContent` ana sayfadaki ContentPlaceHolder denetimi ve ardından `Results` etiket ve `Age` TextBox denetimleri `MainContent`, çünkü `Control.FindControl` yöntemi yalnızca arar içinde *denetim*kapsayıcı adlandırma. Sahip `FindControl` Kal adlandırma kapsayıcısı içinde anlamlı Çoğu senaryoda iki farklı adlandırma kapsayıcılarında iki denetimleri aynı olabileceğinden `ID` değerleri. Adlı bir etiket Web denetimi tanımlayan GridView durumunu göz önünde bulundurun `ProductName` kendi TemplateFields biri içinde. Çalışma zamanında GridView veri bağlandığında bir `ProductName` etiket her GridView satır için oluşturulur. Varsa `FindControl` tüm adlandırma aracılığıyla aranır kapsayıcıları ve biz adlı `Page.FindControl("ProductName")`, hangi etiket örneği gerekir `FindControl` dönüş? `ProductName` İlk GridView satırın etiketinde? Son satırını birinde?

Bu nedenle sahip `Control.FindControl` yalnızca arama *denetim*adlandırma kapsayıcısı alanı anlamlı çoğu durumda. Bize, benzersiz bir sahip olduğumuz bakan bir gibi diğer durumlarda, ancak `ID` tüm adlandırma kapsayıcıları ve gelen erişim bir denetimi için Denetim hiyerarşideki her adlandırma kapsayıcısı başvurmak zorunda kalmamak istiyor. Sahip bir `FindControl` tüm adlandırma kapsayıcıları yapar özyinelemeli olarak arar mantıklı, çok değişken. Ne yazık ki, .NET Framework bu tür bir yöntem içermez.

İyi haber biz kendi oluşturabilir olmanızdır `FindControl` yöntemi tüm adlandırma kapsayıcıları, özyinelemeli olarak arar. Aslında, kullanarak *genişletme yöntemleri* biz pus bir `FindControlRecursive` yönteme `Control` sınıfı, varolan eşlik `FindControl` yöntemi.

> [!NOTE]
> Genişletme yöntemleri bir yeni .NET Framework sürüm 3.5 ve Visual Studio 2008 ile birlikte gönderilen dil C# 3.0 ve Visual Basic 9 özelliğidir. Kısacası, özel bir sözdizimi ile varolan bir sınıf türü için yeni bir yöntem oluşturma uygulama geliştiricisine genişletme yöntemleri sağlar. My makalesi, bu yararlı özelliği hakkında daha fazla bilgi için başvuran [genişletme temel türü işlevselliği genişletme yöntemleri ile](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).


Create genişletme yöntemi, yeni bir dosyaya ekleyin `App_Code` adlı klasörü `PageExtensionMethods.cs`. Adlı bir uzantı ekleme yöntemi `FindControlRecursive` bir giriş olarak alan bir `string` adlı parametre `controlID`. Genişletme yöntemleri düzgün çalışması sınıf ve onun genişletme yöntemleri işaretlenmesi hayati önemdedir `static`. Ayrıca, kendi ilk parametre bir nesneye genişletme yöntemi uygulandığı türünde ve bu giriş parametresinin olan anahtar sözcük gelmelidir gibi tüm genişletme yöntemleri kabul etmelisiniz `this`.

Aşağıdaki kodu ekleyin `PageExtensionMethods.cs` Bu sınıf tanımlamak için sınıf dosyası ve `FindControlRecursive` genişletme yöntemi:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample12.cs)]

Yerinde şu kodla dönmek `IDIssues.aspx` sayfanın arka plandaki kod sınıfı ve açıklama geçerli çıkışı `FindControl` yöntem çağrıları. Çağrı yerine `Page.FindControlRecursive("controlID")`. Genişletme yöntemleri hakkında masaüstünüzdeki nedir doğrudan IntelliSense açılan listeleri içinde görünür değil. Şekil 7'de görüldüğü gibi zaman, sayfa yazın ve süre, isabet `FindControlRecursive` yöntemi yanı sıra diğer açılan IntelliSense dahil `Control` sınıfı yöntemlerinin.


[![Genişletme yöntemleri IntelliSense aşağı açılan listeler dahil edilen](control-id-naming-in-content-pages-cs/_static/image14.png)](control-id-naming-in-content-pages-cs/_static/image13.png)

**Şekil 07**: IntelliSense aşağı açılan listeler genişletme yöntemleri dahil edilen ([tam boyutlu görüntüyü görüntülemek için tıklatın](control-id-naming-in-content-pages-cs/_static/image15.png))


Aşağıdaki kodu girin `SubmitButton_Click` olay işleyicisi ve sayfasını ziyaret yaşınızı girme ve "Gönderme" düğmesini tıklatarak sınayın. Şekil 6 ' gösterildiği gibi sonuçta çıktı ileti olacaktır, "Yaş yaşında olduğunuz!"


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample13.cs)]

> [!NOTE]
> Genişletme yöntemleri Visual Studio 2005 kullanıyorsanız, C# 3.0 ve Visual Basic 9, yeni olduğu için genişletme yöntemleri kullanamazsınız. Bunun yerine, uygulama gerekecektir `FindControlRecursive` yöntemi bir yardımcı sınıfı. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) kendi blog postasına böyle bir örneği olan [ASP.NET Mazer sayfaları ve `FindControl` ](http://www.west-wind.com/WebLog/posts/5127.aspx).


## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>4. adım: doğru kullanarak`id`öznitelik değeri istemci tarafı komut dosyası

Bu öğreticinin girişte belirtildiği gibi bir Web denetimi çizilir `id` özniteliği görmemeleri istemci tarafı komut dosyası programlı olarak belirli bir HTML öğesi başvurmak için kullanılır. Örneğin, bir HTML öğesi tarafından aşağıdaki JavaScript başvuran kendi `id` ve ardından değeri bir kalıcı ileti kutusu görüntüler:


[!code-csharp[Main](control-id-naming-in-content-pages-cs/samples/sample14.cs)]

ASP.NET, sayfalar geri çağırma bir adlandırma kapsayıcının, işlenen HTML öğesi içermiyor `id` özniteliktir Web denetimin aynı `ID` özellik değeri. Bu nedenle, sabit kodunda tempting `id` öznitelik değerlerini JavaScript kodu. Biliyorsanız, diğer bir deyişle, erişmek istediğiniz `Age` TextBox Web denetimi istemci tarafı komut dosyası aracılığıyla bunu çağrısıyla `document.getElementById("Age")`.

Bu yaklaşım, ana sayfalar (veya diğer adlandırma kapsayıcı denetimleri) kullanırken sorundur işlenen HTML `id` Web denetimi ile eşanlamlı değil `ID` özelliği. Bir tarayıcı aracılığıyla sayfasını ziyaret edin ve gerçek belirlemek için kaynak görüntülemek için ilk inclination olabilir `id` özniteliği. İşlenen öğrendikten sonra `id` değeri yapıştırabilirsiniz, çağrı biçimine `getElementById` istemci tarafı komut dosyası çalışmak için ihtiyacınız HTML öğesi erişmek için. Bu yaklaşım sayfanın bazı değişiklikler hiyerarşi kontrol sağladığından değerinden idealdir veya değişikliklerini `ID` adlandırma denetimlerin özelliklerini, elde edilen olmadığını alter `id` özniteliği, böylece JavaScript kodunuzu kesiliyor.

İyi haber olan `id` işlenen öznitelik değeri sunucu tarafı kod Web denetimin üzerinden erişilebilir durumda [ `ClientID` özelliği](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx). Belirlemek için bu özelliği kullanması gereken `id` öznitelik istemci tarafı komut dosyasında kullanılan değeri. Örneğin, sayfaya JavaScript işlevini eklemek için çağrıldığında, değerini görüntüler `Age` metin kutusuna bir kalıcı ileti kutusu eklemek için aşağıdaki kodu `Page_Load` olay işleyicisi:


[!code-javascript[Main](control-id-naming-in-content-pages-cs/samples/sample15.js)]

Yukarıdaki kod değerini yerleştirir `Age` TextBox'ın ClientID özelliğinin JavaScript çağrısı içine `getElementById`. Bir tarayıcı aracılığıyla bu sayfasını ziyaret edin ve HTML kaynağını görüntülemek, şu JavaScript kodunu bulabilirsiniz:


[!code-html[Main](control-id-naming-in-content-pages-cs/samples/sample16.html)]

Bildirim nasıl doğru `id` öznitelik değeri `ctl00_MainContent_Age`, çağrısı içinde görünür `getElementById`. Bu değer çalışma zamanında hesaplandığından, daha sonra sayfa denetim hiyerarşisi yapılan değişiklikler bağımsız olarak çalışır.

> [!NOTE]
> Bu JavaScript örnek yalnızca doğru sunucu denetimi tarafından işlenen HTML öğesi başvuruda bulunan bir JavaScript işlevi ekleme gösterir. Bu işlevi kullanmak için belge yüklendiğinde veya bazı belirli bir kullanıcı eylemi transpires işlevi çağırmak için ek JavaScript yazmanız gerekir. Bunlar hakkında daha fazla bilgi ve ilgili konular, okuma [ile istemci tarafı komut dosyası çalışma](https://msdn.microsoft.com/library/aa479302.aspx).


## <a name="summary"></a>Özet

Belirli ASP.NET sunucu denetimleri işlenen etkiler adlandırma kapsayıcı hareket `id` öznitelik değerleri kendi alt denetimleri ve aynı zamanda tarafından canvassed denetimleri kapsamını `FindControl` yöntemi. Ana sayfalar göre ana sayfa ve ContentPlaceHolder denetimlerini kapsayıcı adlandırma. Sonuç olarak, içerik sayfasını kullanarak içinde denetimlerini programlı olarak başvurmak için İleri biraz daha fazla iş koymak ihtiyacımız `FindControl`. Bu öğreticide şu iki teknikleri incelenmesi: ayrıntılara ContentPlaceHolder denetimine ve arama kendi `FindControl` yöntemi; ve kendi çalışırken `FindControl` uygulaması bu özyinelemeli olarak arar tüm adlandırma kapsayıcıları.

Web denetimleri başvuran göre adlandırma kapsayıcıları tanıtmak sunucu tarafı sorunların yanı sıra, aynı zamanda istemci-tarafı sorunları vardır. Kapsayıcılar, Web denetimi 's adlandırma olmadığında `ID` özellik değeri ve işlenen `id` öznitelik değeri olan bir aynı. Ancak adlandırma kapsayıcısı, işlenen eklenmesi `id` özniteliği hem de içeren `ID` Web denetimi ve kendi denetimi hiyerarşisinin ilişkilerini içinde adlandırma kaldırıldığından değerleri. Web denetimin kullandığınız sürece bu adlandırma sorunları sorun olmayan olan `ClientID` işlenen belirlemek için özellik `id` öznitelik değeri, istemci tarafı komut dosyası.

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET ana sayfalar ve`FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Dinamik veri girişi kullanıcı arabirimleri oluşturma](https://msdn.microsoft.com/library/aa479330.aspx)
- [Temel tür işlevselliği genişletme yöntemleri ile genişletme](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Nasıl yapılır: ASP.NET ana sayfa içeriği başvurusu](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Yakalar mater sayfaları: İpuçları ve püf noktaları](http://www.odetocode.com/articles/450.aspx)
- [İstemci tarafı komut dosyası ile çalışma](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Yazar hakkında

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), yazar birden çok ASP/ASP.NET books ve 4GuysFromRolla.com kurucusu, 1998 itibaren Microsoft Web teknolojileri ile çalışmaktadır. Tan bağımsız Danışman, eğitmen ve yazıcı çalışır. En son kendi defteri [ *kendi öğretmek kendiniz ASP.NET 3.5 24 saat içindeki*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Tan adresindeki ulaşılabilir [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) veya kendi blog aracılığıyla [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Özel teşekkürler

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama gözden geçirenler Zack Can ve Suchi Barnerjee yoktu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Önceki](urls-in-master-pages-cs.md)
[sonraki](interacting-with-the-master-page-from-the-content-page-cs.md)
