---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Sunucu denetimleri | Microsoft Docs
author: microsoft
description: ASP.NET 2.0 sunucu denetimleri pek çok yolla geliştirir. Bu modülde bazı mimari değişiklikler şekilde ASP.NET 2.0 ve Visual Studio 200 ele...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: ecf99fa894c1f662542aa8a613195b828bf2c67b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757323"
---
<a name="server-controls"></a>Sunucu denetimleri
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 sunucu denetimleri pek çok yolla geliştirir. Bu modüldeki bazı mimari değişiklikler olduğu gibi ASP.NET 2.0 şu konulara değineceğiz ve Visual Studio 2005 sunucu denetimleri ile ilgilidir.


ASP.NET 2.0 sunucu denetimleri pek çok yolla geliştirir. Bu modüldeki bazı mimari değişiklikler olduğu gibi ASP.NET 2.0 şu konulara değineceğiz ve Visual Studio 2005 sunucu denetimleri ile ilgilidir.

## <a name="view-state"></a>Görünüm durumu

Birincil ASP.NET 2.0 durumu görünümünde boyutunu önemli ölçüde bir düşüş değişikliktir. Yalnızca bir Takvim denetimi üzerindeki bir sayfayla göz önünde bulundurun. ' De ASP.NET 1.1 görünüm durumu aşağıdadır.

[!code-css[Main](server-controls/samples/sample1.css)]

Şimdi de benzer bir sayfa ASP.NET 2. 0'görünüm durumu aşağıdadır.

[!code-css[Main](server-controls/samples/sample2.css)]

Oldukça önemli bir değişiklik olan ve görünüm durumunu ve geriye kablo üzerinden gerçekleştirilen göz önüne alındığında, bu değişiklik önemli ölçüde performans artışı geliştiriciler verebilirsiniz. Görünüm durumu boyutu azaltmaya biz dahili olarak işleyen büyük ölçüde en nedeniyle yoludur. Görünümü durumu bir Base64 kodlamalı dize unutmayın. ASP.NET 2. 0'görünümünde durum değişikliği daha iyi anlamak için yukarıdaki örnek kodu çözülmüş değerleri göz şimdi sahip.

Kodu çözülen 1.1 görünüm durumu aşağıda verilmiştir:

[!code-css[Main](server-controls/samples/sample3.css)]

Bu biraz saçma gibi görünebilir, ancak burada bir düzeni vardır. ASP.NET'te 1.x, biz tek karakter veri türleri tanımlamak için kullanılmaz ve değerlerini kullanarak ayrılmış &lt; &gt; karakter. Görünüm durumu örnek yukarıdaki "t" bir Üçlü temsil eder. Üçlü içeren bir çift ArrayLists ("m", bir ArrayList öğesini temsil eder.) Bir Int32 bu ArrayLists içerir ("i") 1 ve başka bir değerle başka bir Üçlü içerir. Üçlü bir çift ArrayLists vb. içerir. Anımsanması gereken önemli şey çiftlerinin bulunduğu Triplets kullanıyoruz, şu veri türlerini bir harf aracılığıyla tanımlamak ve kullanıyoruz olan &lt; ve &gt; sınırlayıcı olarak karakter.

ASP.NET 2. 0'da, kodu çözülmüş görünüm durumu biraz farklı görünür.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Kodu çözülen görünüm durumu görünümünü büyük bir değişiklik göreceksiniz. Bu değişiklik birkaç mimari bağlamasından sahiptir. Görüntüleme durumu verileri seri hale getirme için ASP.NET 1.x LosFormatter kullanılır. 2. 0'yeni ObjectStateFormatter'ın sınıfı kullanırız. Bu sınıf, özellikle serileştirme ve seri durumundan çıkarma görünüm durumunu ve denetim durumu yardımcı olmak için tasarlanmıştır. (Denetim durumu sonraki bölümde ele alınacaktır.) Tarafından hangi serileştirme ve seri durumundan çıkarma gerçekleşmesi yöntemi değiştirerek kazanılan birçok faydası vardır. En önemli ölçüde birini kullanan bir TextWriter LosFormatter bir BinaryWriter ObjectStateFormatter'ın kullandığı gerçeğidir. Bu görünüm durumu bir seri dizeleri yerine bayt depolamak ASP.NET 2.0 sağlar. Örneğin, bir tamsayı'ı alın. ASP.NET 1.1 görünüm durumu 4 baytlık tamsayı gereklidir. ASP.NET 2. 0'da, aynı tamsayıyı yalnızca 1 bayt gerektirir. Depolanan Görünüm durumu miktarını azaltmak için diğer geliştirmeler yapıldı. DateTime değerleri, örneğin, şimdi bir TickCount yerine bir dize kullanılarak depolanır.

Tüm bu yeterli vermediği gibi özel olarak dikkat 1.x görünüm durumuna büyük tüketicilerinin birini ve DataGrid denetimleri benzer olduğunu olgusu ödenmiş. Denetimlerin görünüm durumunu burada ilgilenmektedir DataGrid gibi önemli bir dezavantajı, genellikle büyük miktarda yinelenen bilgi içermesidir. ASP.NET'te bilgileri yalnızca üzerinde ve bir bloated görünüm durumuna yeniden kaynaklanan üzerinden depolandığı yinelenen 1.x. ASP.NET 2. 0'da, bu tür verileri depolamak için yeni IndexedString sınıfı kullanın. Bir dize yinelenirse yalnızca belirteç IndexedString ve çalışan bir tablo IndexedString nesne içinde dizin için depolarız.

## <a name="control-state"></a>Denetim durumu

Görünüm durumu ile geliştiriciler olan başlıca gripes için HTTP yük eklenen boyutu biriydi. Daha önceden belirtildiği gibi görünüm durumu en yüksek tüketicilerinin biridir DataGrid denetimi. Görünüm durumu DataGrid tarafından oluşturulan büyük miktarda önlemek için geliştiricilerin çoğu yalnızca bu denetim için Görünüm durumu devre dışı. Ne yazık ki, bu çözüm, her zaman iyi bir değildi. Görüntüleme durumu ASP.NET'te 1.x yalnızca doğru denetimi işlevlerini için gereken verileri içerir. Ayrıca, denetimin UI durumu ile ilgili bilgiler içerir. Bu izin için sayfalandırma bir DataGrid üzerinde bile tüm görüntüleyebileceği kullanıcı Arabirimi bilgileri gerekmeyen görünüm durumunu etkinleştirebilirsiniz isterseniz durumu içerdiğini anlamına gelir. Bunu ya bir senaryodur.

ASP.NET 2. 0'da, denetim durumu Denetim durumu sunulmasıyla güzelce aracılığıyla bu sorunu çözer. Denetim durumu, bir denetimin düzgün çalışması için gerçekten gerekli verileri içerir. Görünüm durumu, denetim durumu devre dışı bırakılamaz. Bu nedenle, Denetim durumda depolanmakta olan verilerin dikkatli bir şekilde denetlenir önemlidir.

> [!NOTE]
> Denetim durumu, görünüm durumuna birlikte kalıcıdır \_ \_VIEWSTATE gizli form alanı.


Bu videoda bir kılavuz görünüm durumunu ve denetim durumu ' dir.


![](server-controls/_static/image1.png)


[Açık tam ekran görüntü](server-controls/_static/state1.wmv)


Okuma ve yazma durumu denetlemek bir sunucu denetimi için sırada üç adımları atmanız gerekir.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>1. adım: RegisterRequiresControlState yöntemi çağırın.

RegisterRequiresControlState yöntem, bir denetim denetim durumu kalıcı hale getirmek gereken ASP.NET bildirir. Bu denetimin kaydedilmekte olan denetim türünde bir bağımsız değişken alır.

Kayıt istemek için kalmaz dikkat edin önemlidir. Bu nedenle, bir denetim denetim durumu kalıcı hale getirmek için ise bu yöntem her istek çağrılmalıdır. Yöntem içinde Onınit çağrılması önerilir.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>2. adım: Geçersiz kılma SaveControlState

SaveControlState yöntemi denetim geri son gönderi beri bir denetim için durum değişikliklerini kaydeder. Denetimin durumunu temsil eden bir nesne döndürür.

## <a name="step-3-override-loadcontrolstate"></a>3. adım: Geçersiz kılma LoadControlState

LoadControlState yöntemi denetime kaydedilmiş durumunu yükler. Yöntem, denetim için kaydedilmiş durumu tutan Object türünde bir bağımsız değişken alır.

## <a name="full-xhtml-compliance"></a>Tam XHTML uyumluluk

Her Web geliştiricinin Web uygulamalarında standartları önemini bilir. ASP.NET 2.0 standartlara dayalı geliştirme ortamını sürdürmek için tam olarak XHTML uyumlu olur. Bu nedenle, tüm etiketleri İşlenmiş HTML 4.0 destekleyen tarayıcılarda XHTML standartlarına göre veya büyük.

ASP.NET 1.1 belge türü tanımı aşağıdaki gibi şöyleydi:

[!code-html[Main](server-controls/samples/sample6.html)]

ASP.NET 2. 0'da, varsayılan belge türü tanımı aşağıdaki gibidir:

[!code-html[Main](server-controls/samples/sample7.html)]

Seçerseniz, varsayılan XHML uyumluluk xhtmlConformance düğüm yapılandırma dosyası aracılığıyla değiştirebilirsiniz. Örneğin, web.config dosyasında aşağıdaki düğüm XHTML uyumluluk XHTML 1.0 katı değişir:

[!code-xml[Main](server-controls/samples/sample8.xml)]

İsterseniz, ASP.NET, ASP.NET içinde kullanılan eski yapılandırmasını kullanmak üzere de yapılandırabilirsiniz 1.x aşağıdaki gibi:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Uyarlamalı bağdaştırıcıları kullanarak oluşturma

ASP.NET'te 1.x, içerdiği yapılandırma dosyası bir &lt;browserCaps&gt; HttpBrowserCapabilities nesnesi doldurulmuş bir bölüm. Bu nesne, hangi cihaz belirli bir istek yapıyor belirlemek ve kodu uygun şekilde işlemek bir geliştirici izin verilir. ASP.NET 2. 0'da, modeli geliştirdi ve artık yeni ControlAdapter sınıfı kullanır. ControlAdapter sınıfı, denetimin yaşam döngüsü olayları geçersiz kılar ve kullanıcı aracısının özelliklerine göre denetimleri işlenmesini denetler. Belirli bir kullanıcı aracısı yeteneklerini c:\windows\microsoft.net\framework\v2.0 içinde depolanan bir tarayıcı tanım dosyası (.browser dosya uzantılı bir dosya) tarafından tanımlanır. \* \* \* \*\CONFIG\Browsers klasör.

> [!NOTE]
> Bir soyut sınıfı ControlAdapter sınıftır.


Benzer şekilde &lt;browserCaps&gt; 1.x, tarayıcı tanım dosyası bölümünde isteyen tarayıcıya belirlemek amacıyla kullanıcı aracısı dizesini ayrıştırmak için normal bir ifade kullanır. Bu, kullanıcı aracısı için bunları tanımladığı belirli özellikleri. ControlAdapter denetim işleme yöntemi ile işler. İşleme yöntemi geçersiz kılarsanız, bu nedenle, işleme taban sınıfa çağırmamanız gerekir. Bunun yapılması, işleme bağdaştırıcısı için bir kez ve denetim için bir kez iki kez gerçekleşmesine neden olabilir.

## <a name="developing-a-custom-adapter"></a>Özel bağdaştırıcı geliştirme

Kendi özel bağdaştırıcı ControlAdapter devralarak geliştirebilirsiniz. Ayrıca, bir sayfa için bir bağdaştırıcı gerekmesi halinde durumlarda PageAdapter soyut sınıftan devralabilir. Eşleme için özel bağdaştırıcınıza denetimlerin aracılığıyla gerçekleştirilir &lt;controlAdapters&gt; tarayıcı tanım dosyasındaki öğesi. Örneğin, bir tarayıcı tanım dosyasından aşağıdaki XML MenuAdapter sınıfa menü denetimi eşler:

[!code-html[Main](server-controls/samples/sample10.html)]

Bu modeli kullanarak, belirli bir cihaz veya tarayıcı hedeflemek bir denetim geliştiricisi için oldukça kolay olur. Ayrıca, oldukça basit bir geliştiricinin nasıl her cihazda sayfaları işlemek üzerinde tam denetime sahip olur.

## <a name="per-device-rendering"></a>Cihaz başına işleme

Sunucu denetimi ASP.NET 2.0 özelliklerinde belirtilen tarayıcı özgü önekini kullanarak cihaz başına olabilir. Örneğin, aşağıdaki kod hangi cihaz sayfasına göz atmak için kullanıldığına bağlı olarak bir etiket metnini değiştirir.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Bu etiketi içeren sayfa Internet Explorer'dan gözatılırken "Internet Explorer'dan göz attığınız." belirten bir metin etiketi görüntülenir Sayfa Firefox gözatılırken etiket metni "Firefox göz attığınız." görüntülenir Sayfanın diğer herhangi bir CİHAZDAN gözatılırken "Bilinmeyen bir CİHAZDAN göz attığınız." görüntülenir Herhangi bir özelliği, bu özel söz dizimi kullanılarak belirtilebilir.

## <a name="setting-focus"></a>Odak ayarlama

ASP.NET 1.x geliştiriciler, belirli bir denetimde ilk odak ayarlama hakkında sık sorulan. Örneğin, bir oturum açma sayfasında kullanıcı kimliği textbox sayfa ilk yüklendiğinde odağı alması kullanışlıdır. ASP.NET'te, bazı istemci tarafı komut dosyası yazma 1.x, bunun yapılması gereklidir. Böyle bir komut dosyası basit bir görev olsa da, artık SetFocus yöntemi sayesinde ASP.NET 2.0 sürümünde gerekli değildir. SetFocus yöntem odaklanması gereken denetimini gösteren tek bir bağımsız değişken alır. Bu bağımsız değişken, bir dize olarak denetimin istemci kimliği veya sunucu denetimi olarak denetim nesnesi adı ya da olabilir. Örneğin, sayfa ilk yüklendiğinde txtUserID adlı bir TextBox denetimine başlatma odağı ayarlamak için sayfaya aşağıdaki kodu ekleyin\_yük:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--veya

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 (daha önce açıklanmıştır) Webresource.axd işleyicisinin odağı ayarlar bir istemci-tarafı işlevi işlemek için kullanır. İstemci tarafı işlevi WebForm adıdır\_burada gösterildiği gibi AutoFocus:

[!code-html[Main](server-controls/samples/sample14.html)]

Alternatif olarak, ilgili denetimin ilk odağı ayarlamak için bir denetim için odak yöntemi kullanabilirsiniz. Odak yöntemi denetim sınıfından türetilen ve tüm ASP.NET 2.0 denetimler için kullanılabilir. Bir doğrulama hatası oluştuğunda özel bir denetime Odaklan da mümkündür. Bir sonraki modüle ele alınmaktadır.

## <a name="new-server-controls-in-aspnet-20"></a>ASP.NET 2.0 sürümünde yeni sunucu denetimleri

ASP.NET 2.0 yeni sunucu denetimleri aşağıda verilmiştir. Daha fazla ayrıntıya bazı bunları sonraki modüllerdeki ele alacağız.

## <a name="imagemap-control"></a>Görüntü eşleme denetimi

Görüntü eşleme denetimi tekrar başlatmak veya bir URL'ye bir görüntü etkin noktaları eklemenize olanak sağlar. Etkin nokta üç tür vardır; CircleHotSpot RectangleHotSpot ve PolygonHotSpot. Etkin noktaları Koleksiyonu Düzenleyicisi Visual Studio veya programlama yoluyla kod aracılığıyla eklenir. Herhangi bir kullanıcı arabirimi üzerindeki resmin sıcak çizmek için kullanılabilir. Koordinatları ve boyutunu veya başvurulduğunda yarıçapını bildirimli olarak belirtilmelidir. Herhangi bir etkin nokta Tasarımcısı'nda görsel bir temsilini yoktur. Etkin nokta, bir URL'ye için yapılandırılmışsa, URL başvurulduğunda NavigateUrl özelliği aracılığıyla belirtilir. Söz konusu olduğunda bir gönderi etkin özelliği sunucu tarafı kodunda alınabilmesi için bir dize gönderisinde geri geçirmenize olanak PostBackValue yedekleyin.


![Visual Studio'da etkin nokta Koleksiyonu Düzenleyicisi](server-controls/_static/image1.jpg)

**Şekil 1**: Visual Studio'da etkin nokta Koleksiyonu Düzenleyicisi


## <a name="bulletedlist-control"></a>Bulletedlıst denetimi

Bulletedlıst kolayca veri bağımlı olabilir bir madde işaretli liste denetimidir. Liste (numaralandırılmış) sıralanabilir veya sıralanmamış BulletStyle özelliği aracılığıyla. Listedeki her öğe bir ListItem nesnesi tarafından temsil edilir.


![Visual Studio'da Bulletedlıst denetimi](server-controls/_static/image1.gif)

**Şekil 2**: Visual Studio'da Bulletedlıst denetimi


## <a name="hiddenfield-control"></a>Hiddenfield'i denetimi

Hiddenfield'i denetim gizlenmiş form alanı değeri, sunucu tarafı kodunda kullanılabilir sayfanıza ekler. Post telefonla geri arasında değişmeden kalmasını genellikle gizli form alanının değeri bekleniyor. Ancak, kötü niyetli bir kullanıcı geri gönderilecek değer önceki değiştirmek mümkündür. Böyle bir durumda Hiddenfield'i denetim ValueChanged olayı ortaya koyar. Hassas bilgileri Hiddenfield'i denetiminde olması ve değişmeden kalmasını sağlamak istiyorsanız, kodunuzda ValueChanged olay işlemesi gerekir.

## <a name="fileupload-control"></a>Dosya yükleme denetimi

ASP.NET 2.0 FileUpload denetiminde ASP.NET sayfası aracılığıyla bir Web sunucusunda dosyaları karşıya yükleme mümkün kılar. Bu denetim, birkaç özel durum ASP.NET 1.x HtmlInputFile sınıfı oldukça benzerdir. ASP.NET'te 1.x,, önerilen iyi bir dosya tablonuz varsa belirleyebilmesi PostedFile özelliği için null denetlenmesi. ASP.NET 2.0 FileUpload denetimi, aynı amaçla kullanabilirsiniz ve biraz daha verimlidir yeni HasFile özelliği ekler.

PostedFile özellik hala HttpPostedFile nesneye erişim için kullanılabilir, ancak bazı HttpPostedFile işlevselliğini doğası gereği FileUpload denetimiyle kullanıma sunuldu. Örneğin, ASP.NET'te karşıya yüklenen bir dosyayı kaydetmek için HttpPostedFile nesne üzerinde farklı kaydet yöntemi çağırırsınız 1.x,. ASP.NET 2.0 FileUpload Denetimi'ni kullanarak FileUpload denetimin kendisinde Farklı Kaydet yöntemi çağırırsınız.

Başka bir önemli değişiklik 2.0 davranışı (ve muhtemelen en önemli değişiklik), artık tüm karşıya yüklenen dosyayı kaydetmeden önce belleğe yüklemek gerekli olmasıdır. 1.x içinde yüklenen herhangi bir dosyayı yazılmadan önce tamamen bellek kaydedilir diske. Bu mimari, büyük dosyaları karşıya yüklenmesi engeller.

ASP.NET 2. 0'da, httpRuntime öğesinin MaxRequestLength özniteliği kaç kilobayt yapılandırmanıza olanak tanır arabelleği yazılmadan önce bellekte tutulan diske.

**Önemli**: Bu değer, bayt (kilobayt değil) olduğunu ve varsayılan 256 MSDN belgeleri (ve başka bir yerde belgeleri) belirtir. Değer gerçekte kilobayt cinsinden belirtilir ve varsayılan değeri 80'dir. Varsayılan değer 80 k sağlayarak, arabellek büyük nesne yığınını bitmiyor emin emin oluruz.

## <a name="wizard-control"></a>Sihirbaz Denetimi

ASP.NET geliştiricilerine "panelleri kullanarak sayfa" ya da sayfadan sayfaya aktararak bilgi serideki toplanmaya çalışılırken ile ulaşmakta karşılaştığınız oldukça yaygındır. Daha fazla değildir, çaba sıkıcı bir ve zaman alır. Yeni sihirbaz denetimi doğrusal ve doğrusal olmayan kullanıcıların aşina olduğu bir sihirbaz arabirimi adımlarda izin vererek bu sorunları çözer. Sihirbaz denetimi, bir dizi adım girdi biçimleri sunar. Denetimin StepType özelliği tarafından belirtilen belirli bir türün her adımdır. Kullanılabilir adım türleri aşağıdaki gibidir:

| **Adım türü** | **Açıklama** |
| --- | --- |
| Otomatik | Sihirbaz otomatik olarak adım adım hiyerarşi içindeki konumuna göre türünü belirler. |
| Başlat | Genellikle bir giriş ifadesi sunmak için kullanılan ilk adımı. |
| Adım | Normal bir adım. |
| Son | Sihirbazı tamamlamak için bir düğme sunmak için genellikle kullanılan son adımı. |
| Tamamlayın | Başarı veya başarısızlık iletişim kurulurken bir ileti gösterir. |

> [!NOTE]
> Sihirbaz denetimi, ASP.NET denetim durumunu kullanarak durumunu izler. Bu nedenle, herhangi bir zarar olmadan false EnableViewState özellik ayarlanabilir.


Bu videoda, bir gözden geçirme Sihirbazı'nı Denetim olur.


![](server-controls/_static/image2.png)


[Açık tam ekran görüntü](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Denetim yerelleştirme

Localıze denetimi değişmez bir denetime benzerdir. Localıze denetleyebilir ancak bir **modu** eklendiğinde biçimlendirme nasıl işleneceğini denetleyen özelliği. Mod özelliği aşağıdaki değerlerini destekler:

| **Modu** | **Açıklama** |
| --- | --- |
| Dönüştürme | Biçimlendirme isteği yapan tarayıcının Protokolü göre dönüştürülür. |
| Doğrudan geçiş | Biçimlendirme olarak işlenen-olduğu. |
| Kodlama | Eklediğiniz bir denetim için biçimlendirme HtmlEncode kullanılarak kodlanır. |

## <a name="multiview-and-view-controls"></a>MultiView ve görünüm denetimi

MultiView denetimi görünümü denetimler için kapsayıcı olarak davranır ve görünüm denetimi (çok benzer bir Panel denetimi) diğer denetimler için kapsayıcı olarak davranır. Her bir görünümde bir MultiView denetimi, tek bir görünüm denetimi tarafından temsil edilir. İlk MultiView görünümü denetiminde görünümü 0, ikinci görünümü 1, vs. Bir MultiView denetimi ActiveViewIndex belirterek görünümler geçiş yapabilirsiniz.

## <a name="substitution-control"></a>Değişim Denetimi

Değişim Denetimi ASP.NET önbelleğe alma ile birlikte kullanılır. Burada, önbelleğe alma avantajından yararlanmak istediğiniz, ancak her istek (önbelleğe alınan muaf tutulan başka bir deyişle, bölümleri sayfasının) güncelleştirilmesi gereken sayfasının bölümlerini sahip durumlarda değiştirme bileşeni harika bir çözüm sağlar. Denetim, aslında kendi kendine herhangi bir çıktı işlemesi yapmıyor. Bunun yerine, sunucu tarafı kodunuzda yöntemine bağlıdır. Sayfa istendiğinde yöntemi çağrılır ve döndürülen biçimlendirme, değişim denetimi yerine işlenir.

Değişim Denetimi bağlı yöntemi aracılığıyla belirtilen **MethodName** özelliği. Bu yöntem, aşağıdaki ölçütleri karşılaması gerekir:

- Statik (paylaşılan VB) yöntemi olmalıdır.
- Bunu HttpContext türünde bir parametre kabul eder.
- Denetimi sayfasına değiştirmeniz gerekir biçimlendirmeyi temsil eden bir dize döndürür.

Değişim Denetimi herhangi bir sayfa denetiminde değiştirme olanağına sahip değildir, ancak geçerli HttpContext parametresinin aracılığıyla erişimi.

## <a name="gridview-control"></a>GridView denetiminde

GridView denetiminde DataGrid denetimi yerini alır. Bu denetimi bir sonraki modülünde daha ayrıntılı ele alınmaktadır.

## <a name="detailsview-control"></a>DetailsView denetiminde

DetailsView denetiminde bir veri kaynağından tek bir kaydı görüntülemek ve düzenlemek veya silmek sağlar. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="formview-control"></a>FormView denetimi

FormView denetimi, bir veri kaynağı tek bir kayıttan yapılandırılabilir bir arabirimde görüntülemek için kullanılır. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="accessdatasource-control"></a>AccessDataSource denetimi

AccessDataSource denetimi için kullanılan verileri bir veritabanına bağlamak. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="objectdatasource-control"></a>ObjectDataSource Denetimi

ObjectDataSource Denetimi, denetimleri verilere iki katmanlı modeli yerine bir orta katman iş nesnesine denetimleri doğrudan veri kaynağına bağlı olduğu bağlı bir üç katmanlı mimariyi destekleyecek şekilde kullanılır. Bunu bir sonraki modülünde daha ayrıntılı açıklanmıştır.

## <a name="xmldatasource-control"></a>XmlDataSource'ta denetimi

XmlDataSource'ta denetimi, veri bağlamak için bir XML veri kaynağı kullanılır. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="sitemapdatasource-control"></a>SiteMapDataSource denetimi

Site gezinti denetimlerinin bir site haritasına dayalı veri bağlama SiteMapDataSource denetim sağlar. Bunu bir sonraki modülünde daha ayrıntılı açıklanmıştır.

## <a name="sitemappath-control"></a>SiteMapPath Control

Gezinti bağlantıları içerik haritası başvurulan bir dizi SiteMapPath denetimi görüntüler. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="menu-control"></a>Menü denetimi

Menü denetimi DHTML kullanan dinamik menüler görüntüler. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="treeview-control"></a>TreeView Denetimi

TreeView denetimi, verilerin bir hiyerarşik ağaç görünümünü görüntülemek için kullanılır. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="login-control"></a>Login denetimi

Oturum açma denetimi bir Web sitesini açmak bir mekanizma sağlar. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="loginview-control"></a>LoginView denetimi

Bir kullanıcının oturum açma durumuna farklı şablon görüntüsü için LoginView denetimi sağlar. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="passwordrecovery-control"></a>PasswordRecovery denetimi

PasswordRecovery denetim varsayılan olarak, bir ASP.NET uygulaması kullanıcılar tarafından Unutulan parolaları almak için kullanılır. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="loginstatus"></a>LoginStatus

Bir LoginStatus denetimi kullanıcının oturum açma durumunu görüntüler. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="loginname"></a>LoginName

Bir LoginName denetimi, bir ASP.NET uygulamasına oturum sonra bir kullanıcının kullanıcı adını görüntüler. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard kullanıcıların bir ASP.NET uygulamasında kullanmak için bir ASP.NET üyelik hesabı oluşturma olanağı sunan yapılandırılabilir bir sihirbazdır. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="changepassword"></a>ChangePassword

ChangePassword denetimi, kullanıcıların bir ASP.NET uygulaması için parola değiştirmesini sağlar. Bir sonraki modülünde daha ayrıntılı ele alınmıştır.

## <a name="various-webparts"></a>Çeşitli Web Bölümleri

ASP.NET 2.0 çeşitli Web Bölümleri ile birlikte gelir. Bu sonraki modülde ayrıntılarıyla ele alınmaktadır.
