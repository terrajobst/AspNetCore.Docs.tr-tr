---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Sunucu denetimleri | Microsoft Docs
author: microsoft
description: "ASP.NET 2.0 sunucu denetimleri birçok yolla geliştirir. Bu modülde şu şekilde ASP.NET 2.0 ve Visual Studio 200 mimari değişikliklerden bazıları konulara değineceğiz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 09f1a2e4de024e5778e69fdd691d9cb0040459f3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="server-controls"></a>Sunucu denetimleri
====================
tarafından [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 sunucu denetimleri birçok yolla geliştirir. Bu modüldeki ASP.NET 2.0 şekilde mimari değişikliklerden bazıları şu konulara değineceğiz ve Visual Studio 2005 sunucu denetimleri ile ilgilidir.


ASP.NET 2.0 sunucu denetimleri birçok yolla geliştirir. Bu modüldeki ASP.NET 2.0 şekilde mimari değişikliklerden bazıları şu konulara değineceğiz ve Visual Studio 2005 sunucu denetimleri ile ilgilidir.

## <a name="view-state"></a>Görünüm durumu

Birincil ASP.NET 2.0 ile görünümünde durum boyutu önemli ölçüde azalma değişikliktir. Yalnızca bir Takvim denetimi üzerindeki içeren bir sayfa göz önünde bulundurun. ' De ASP.NET 1.1 görünüm durumu aşağıdadır.

[!code-css[Main](server-controls/samples/sample1.css)]

Şimdi aynı sayfada ASP.NET 2.0 ile görünüm durumu İşte.

[!code-css[Main](server-controls/samples/sample2.css)]

Oldukça önemli bir değişiklik olan ve görünüm durumunu ve geriye kablo üzerinden taşınan düşünüldüğünde, bu değişikliği önemli ölçüde performans artışı geliştiriciler verebilirsiniz. Görünüm durumunun boyutu azalma biz dahili olarak işleyen büyük ölçüde nedeniyle yoludur. Görünümü durumu bir Base64 ile kodlanmış dizesi unutmayın. ASP.NET 2.0 ile görünümünde durum değişikliği daha iyi anlamak için yukarıdaki örnek kodu çözülmüş değerleri göz şimdi sahip.

Kodunu çözdü 1.1 görünüm durumu şöyledir:

[!code-css[Main](server-controls/samples/sample3.css)]

Bu biraz arkadaşınızdan gibi görünebilir, ancak burada bir deseni yok. ASP.NET 1.x, biz tek karakter veri türleri tanımlamakta kullanılan ve değerlerini kullanarak ayrılmış &lt; &gt; karakter. Görünüm durumu örnek yukarıdaki "t" bir Üçlü temsil eder. Üçlü ("m" ArrayList temsil eder.) ArrayLists çifti içerir Bu ArrayLists birini içeren bir Int32 ("i") ve diğer 1 değerine sahip başka bir Üçlü içerir. Üçlü ArrayLists, vb. çifti içerir. Anımsaması önemli şey çiftleri içerir Triplets kullanıyoruz, biz bir harf ile veri türlerini tanımlamak ve kullanırız &lt; ve &gt; karakter ayırıcısı olarak.

ASP.NET 2. 0'da, kodu çözülmüş görünüm durumu biraz farklı görünür.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Kodu çözülmüş görünüm durumu görünümünü büyük bir değişiklik fark. Bu değişiklik birkaç mimari bağlamasından sahiptir. Görüntüleme durumu verilerini seri hale getirmek için ASP.NET 1.x LosFormatter kullanılır. 2. 0 ', yeni ObjectStateFormatter'ın sınıfı kullanın. Bu sınıf, özellikle seri hale getirme ve seri durumdan çıkarma görünüm durumu ve denetim durumunun yardımcı olmak için tasarlanmıştır. (Denetim durumu bir sonraki bölümde ele alınacaktır.) Tarafından seri hale getirme ve seri durumdan çıkarma gerçekleşmesi yöntemi değiştirerek elde edilen birçok avantaj vardır. En önemli ölçüde bir TextWriter kullanan LosFormatter, bir BinaryWriter ObjectStateFormatter'ın kullandığı olgu biridir. Bu görünüm durumu bir dizi bayt dizeler yerine depolamak ASP.NET 2.0 sağlar. , Örneğin, bir tamsayı alın. ' De ASP.NET 1.1, Görünüm durumu 4 bayt tamsayı gereklidir. ASP.NET 2. 0'da, aynı tamsayıyı yalnızca 1 bayt gerektirir. Depolanan Görünüm durumu miktarını azaltmak için diğer geliştirmeler yapıldı. DateTime değerleri, örneğin, artık bir TickCount yerine bir dize kullanılarak depolanır.

Tümünü yeterli doğru şekilde, özel dikkat 1.x görünüm durumuna en büyük tüketicilerinin birini benzer denetimleri ve DataGrid olduğunu olgu ödenmiş. Denetimlerin görünüm durumunu burada kaygı DataGrid gibi önemli bir dezavantajı, genellikle büyük miktarda yinelenen bilgi içermesidir. ASP.NET bilgileri yeniden bloated görünüm durumda kaynaklanan üzerinden ve üzerinden yalnızca depolandı yinelenen 1.x,. ASP.NET 2. 0'da, bu tür veri depolamak için yeni IndexedString sınıfı kullanın. Bir dize devam ederse, yalnızca belirteç IndexedString ve çalışan bir tablo IndexedString nesnelerin dizin depolarız.

## <a name="control-state"></a>Denetim durumu

Görünüm durumu ile geliştiriciler vardı ana gripes birini HTTP yükü eklenen boyutu oluştu. Daha önce belirtildiği gibi DataGrid denetimi görünüm durumu en büyük tüketicilerinin biridir. DataGrid tarafından oluşturulan görünüm durumu büyük miktarlarda önlemek için çoğu geliştiricinin yalnızca bu denetim için görünüm durumuna devre dışı. Ne yazık ki, bu çözüm, her zaman iyi bir değildi. Görüntüleme durumu ASP.NET 1.x yalnızca denetim doğru işlevselliği için gerekli verileri içerir. Denetimin UI durumu ile ilgili bilgiler de içerir. Başka bir deyişle, izin vermek bir DataGrid'de sayfalandırma için tüm görüntülemek UI bilgileri gerekmeyen olsa bile, Görünüm durumu etkinleştirmelisiniz istiyorsanız durumu içerir. Bu ya hep ya hiç bir senaryodur.

ASP.NET 2. 0'da, denetim durumu Denetim durumu giriş aracılığıyla sorunsuz şekilde bu sorunu çözer. Denetim durumu Denetim düzgün çalışması için kesinlikle gerekli verileri içerir. Görünüm durumu farklı olarak, denetim durumu devre dışı bırakılamaz. Bu nedenle, Denetim durumunda depolanmakta veri dikkatle denetlenir önemlidir.

> [!NOTE]
> Görünüm durumunun yanı sıra denetim durumu kalıcı \_ \_VIEWSTATE gizli bir form alanı.


Bu videoyu görünüm durumunu ve denetim durumu bir kılavuz ' dir.


![](server-controls/_static/image1.png)


[Açık Tam Ekran Video](server-controls/_static/state1.wmv)


Okuma ve yazma durumunu denetlemek bir sunucu denetimi için sırayla üç adımlar atmanız gerekir.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>1. adım: RegisterRequiresControlState yöntemini çağırın

RegisterRequiresControlState yöntemi, ASP.NET bir denetim denetim durumu kalıcı olması gerektiğini bildirir. Bir bağımsız değişken türü, kaydedilmekte olan denetim denetimi alır.

Kayıt isteği başka bir istek kalmaz dikkate almak önemlidir. Bu nedenle, bir denetim denetim durumu devam ettirmek için ise bu yöntem her istek çağrılmalıdır. Yöntemi içinde Onınit çağrılması önerilir.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>2. adım: SaveControlState geçersiz kıl

SaveControlState yöntemi denetim geri son post bu yana bir denetim için durum değişiklikleri kaydeder. Denetimin durumunu temsil eden bir nesne döndürür.

## <a name="step-3-override-loadcontrolstate"></a>3. adım: LoadControlState geçersiz kıl

LoadControlState yöntemi kaydedilmiş durum denetime yükler. Yöntem denetimi için kaydedilmiş durumu tutan nesne türünde bir bağımsız değişken alır.

## <a name="full-xhtml-compliance"></a>Tam XHTML uyumluluk

Herhangi bir Web geliştirici Web uygulamalarında standartları önemini bilir. Standartlara dayalı geliştirme ortamını sürdürmek için ASP.NET 2.0 tam olarak XHTML ile uyumlu değil. Bu nedenle, tüm etiketleri HTML 4.0 destekleyen tarayıcılarda XHTML standartlarına göre işlenmiş veya daha büyük.

ASP.NET 1.1 DOCTYPE tanımında gibi oluştu:

[!code-html[Main](server-controls/samples/sample6.html)]

ASP.NET 2. 0'da, varsayılan DOCTYPE tanımı aşağıdaki gibidir:

[!code-html[Main](server-controls/samples/sample7.html)]

Seçtiğiniz yapılandırma dosyasında xhtmlConformance düğümü aracılığıyla varsayılan XHML uyumluluk değiştirebilir. Örneğin, web.config dosyasında aşağıdaki düğümü XHTML uyumluluk XHTML 1.0 Strict değişir:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Seçerseniz, ASP.NET kullanılan eski yapılandırmasını kullanmak için ASP.NET de yapılandırabilirsiniz 1.x şekilde:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Uyarlamalı bağdaştırıcıları kullanarak oluşturma

ASP.NET 1.x, içerdiği yapılandırma dosyası bir &lt;browserCaps&gt; HttpBrowserCapabilities nesnesi doldurulmuş bölümü. Bu nesneye hangi cihaz belirli bir istek yapıyor belirlemek ve kod uygun şekilde işlemek bir geliştirici izin verilmiyor. ASP.NET 2. 0'da, model geliştirilmiştir ve artık yeni ControlAdapter sınıfını kullanır. ControlAdapter sınıfı denetimin yaşam döngüsü olayları geçersiz kılar ve kullanıcı aracısının özellikleri dayalı denetimler denetler. Belirli bir kullanıcı aracısı özelliklerini c:\windows\microsoft.net\framework\v2.0 depolanan tarayıcı tanım dosyası (.browser dosya uzantılı bir dosya) tarafından tanımlanır. \* \* \* \*\CONFIG\Browsers klasör.

> [!NOTE]
> Bir Özet sınıf ControlAdapter sınıftır.


Çok benzer şekilde &lt;browserCaps&gt; 1.x, tarayıcı tanım dosyası bölümünde, istekte bulunan tarayıcıyı belirlemek amacıyla kullanıcı aracısı dizesi ayrıştırmak için normal bir ifade kullanır. Bu, kullanıcı aracısı için bunları tanımlayan belirli özellikleri. ControlAdapter denetim işleme yöntemi üzerinden işler. İşleme yöntemi geçersiz kılarsanız, bu nedenle, işleme temel sınıfını çağırmalısınız değil. Bunun yapılması, iki kez bağdaştırıcısı için bir kez ve denetim için bir kez gerçekleşecek şekilde işleme neden olabilir.

## <a name="developing-a-custom-adapter"></a>Özel bağdaştırıcı geliştirme

ControlAdapter devralarak özel bağdaştırıcınızı geliştirebilirsiniz. Ayrıca, bir bağdaştırıcı için bir sayfa burada gereken durumlarda soyut sınıftan PageAdapter devralabilirsiniz. Özel bağdaştırıcınızı denetimlerine eşleme aracılığıyla gerçekleştirilir &lt;controlAdapters&gt; tarayıcı tanım dosyasındaki öğesi. Örneğin, bir tarayıcı tanım dosyasında aşağıdaki XML'den menü denetimi MenuAdapter sınıfına eşler:

[!code-html[Main](server-controls/samples/sample10.html)]

Bu modeli kullanarak, belirli bir aygıt veya tarayıcı hedeflemek denetim geliştirici için oldukça kolay olur. Ayrıca, oldukça basit bir geliştirici nasıl her cihazda sayfaları işlemek üzerinde tam denetime sahiptir.

## <a name="per-device-rendering"></a>Aygıt başına işleme

Sunucu denetimi ASP.NET 2.0 özelliklerinde belirtilen bir tarayıcıya özgü önek kullanarak cihaz başına olabilir. Örneğin, aşağıdaki kodu hangi cihaz sayfasına göz atmak için kullanıldığına bağlı olarak bir etiket metnini değiştirir.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Bu etiketi içeren sayfa Internet Explorer'dan gözatarken "Internet Explorer'dan gözatarken." belirten metin etiketi görüntülenir Sayfa Firefox gözatarken etiket metni "Firefox gözatarken." görüntülenir Sayfanın diğer herhangi bir aygıttan tarandığında, "Bilinmeyen bir aygıttan gözatarken." görüntülenir Herhangi bir özellik, bu özel bir sözdizimi kullanılarak belirtilebilir.

## <a name="setting-focus"></a>Odak ayarlama

ASP.NET 1.x geliştiriciler, belirli bir denetimde ilk odağı ayarlama hakkında sık sorulan. Örneğin, bir oturum açma sayfasında, sayfa ilk kez yüklediğinde odağı alması kullanıcı kimliği textbox sağlamak faydalı olur. ASP.NET, bazı istemci tarafı komut dosyası yazma 1.x, bunun yapılması gereklidir. Böyle bir komut dosyası karmaşık bir görev olsa da, artık sayesinde SetFocus yöntemini ASP.NET 2.0 ile gerekli değildir. SetFocus yöntemini odaklanması gereken denetimini gösteren bir değişken alır. Bu bağımsız değişken bir dize olarak denetiminin istemci Kimliğini veya sunucu denetimi denetim nesnesi olarak adını ya da olabilir. Örneğin, sayfa ilk kez yüklediğinde txtUserID adlı TextBox denetimine ilk odağı ayarlamak için sayfasına aşağıdaki kodu ekleyin\_yük:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--veya

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 (daha önce ele alınan) Webresource.axd işleyicisinin odağı ayarlar istemci-tarafı işlevi işlemek için kullanır. İstemci tarafı işlevi WebForm adıdır\_aşağıda gösterildiği gibi AutoFocus:

[!code-html[Main](server-controls/samples/sample14.html)]

Alternatif olarak, bu denetim için ilk odağı ayarlamak için bir denetim için odak yöntemini kullanabilirsiniz. Odağı yöntemi denetim sınıfından türetilen ve tüm ASP.NET 2.0 denetimleri için kullanılabilir. Bir doğrulama hatası oluştuğunda özel bir denetime odağı ayarlamak da mümkündür. Bir sonraki modüle ele alınacaktır.

## <a name="new-server-controls-in-aspnet-20"></a>ASP.NET 2.0 yeni sunucu denetimleri

Yeni sunucu denetimleri, ASP.NET 2.0 verilmiştir. Daha fazla ayrıntıya bazı bunları daha sonra modüllerinde ele alacağız.

## <a name="imagemap-control"></a>Görüntü eşleme denetimi

Görüntü eşleme denetimi post yeniden başlatmak veya bir URL'ye bir görüntü etkin eklemenize olanak sağlar. Etkin noktalarına üç tür kullanılabilir; CircleHotSpot, RectangleHotSpot ve PolygonHotSpot. Etkin noktalarına Visual Studio veya programlama kodu Koleksiyonu Düzenleyicisi aracılığıyla eklenir. Etkin noktalarına görüntüde çizim için kullanılabilir herhangi bir kullanıcı arabirimi yoktur. Koordinatları ve boyutunu veya etkin nokta RADIUS bildirimli olarak belirtilmelidir. Hiçbir görsel bir etkin nokta Tasarımcısı'nda yoktur. Bir etkin nokta URL'ye gitmek üzere yapılandırılmışsa, URL başvurulduğunda NavigateUrl özelliği belirtilir. Söz konusu olduğunda bir post etkin nokta, özelliği, sunucu tarafı kodu alınabilir post geri dizesinde geçirmenizi sağlar PostBackValue yedekleyin.


![Visual Studio'da etkin nokta Koleksiyonu Düzenleyicisi](server-controls/_static/image1.jpg)

**Şekil 1**: Visual Studio'da etkin nokta Koleksiyonu Düzenleyicisi


## <a name="bulletedlist-control"></a>Bulletedlıst denetimi

Veri bağlama kolayca olabilir madde işaretli bir liste Bulletedlıst denetimdir. Listenin (numaralı) sıralanabilir veya BulletStyle özelliği aracılığıyla sırasız. Listedeki her öğeye LISTITEM nesnesiyle temsil edilir.


![Visual Studio'da Bulletedlıst denetimi](server-controls/_static/image1.gif)

**Şekil 2**: Visual Studio'da Bulletedlıst denetimi


## <a name="hiddenfield-control"></a>HiddenField denetimi

HiddenField denetimi gizli bir form alanı değerini sunucu tarafı kodu kullanılabilir sayfanıza ekler. Gizli bir form alanı değerini genellikle post geri arasında değişmeden beklenir. Ancak, kötü niyetli bir kullanıcı geri göndermek için değeri önceki değiştirmek mümkündür. Bu durumda, HiddenField denetimi ValueChanged olay ortaya koyar. HiddenField denetiminiz hassas bilgileri ve değişmeden kalmasını sağlamak istiyorsanız, kodunuzda ValueChanged olay işlemesi gerekir.

## <a name="fileupload-control"></a>Dosya yükleme denetimi

ASP.NET 2.0 dosya yükleme denetiminde bir Web sunucusunda bir ASP.NET sayfası aracılığıyla dosyaları yüklemek mümkün kılar. Bu denetim, birkaç özel durum ile ASP.NET 1.x HtmlInputFile sınıfı oldukça benzer. ASP.NET 1.x, önerilir, iyi bir dosya sahip belirlemek için PostedFile özelliği için null denetlenmesi. ASP.NET 2.0 dosya yükleme denetiminde aynı amaçla kullanabilirsiniz ve biraz daha etkilidir yeni bir HasFile özelliği ekler.

PostedFile özellik HttpPostedFile nesneye erişim hala kullanılabilir, ancak bazı HttpPostedFile işlevlerini şimdi doğası gereği dosya yükleme denetimi ile kullanılabilir. Örneğin, ASP.NET karşıya yüklenen bir dosyayı kaydetmek için HttpPostedFile nesnesinde Farklı Kaydet yöntemi çağırmanız 1.x,. ASP.NET 2.0 ile dosya yükleme Denetimi'ni kullanarak dosya yükleme denetimi kendisini Farklı Kaydet yöntemi çağırırsınız.

Başka bir önemli değişiklik 2.0 davranış (ve olasılıkla en önemli değişiklik), artık kaydetmeden önce karşıya yüklenen dosyanın tamamı belleğe yüklemek gerekli olmasıdır. Belleğe yazılmakta önce tamamen kaydedilen 1.x içinde yüklenen herhangi bir dosyayı diske. Bu mimari büyük dosyaları karşıya yükleme engeller.

ASP.NET 2. 0'da, httpRuntime öğesinin MaxRequestLength özniteliği kaç kilobayt yapılandırmanıza izin verir arabelleği yazılmakta önce bellekte tutulan diske.

**Önemli**: Bu değer bayt (kilobayt değil) olduğunu ve varsayılan 256 MSDN belgeleri (ve başka bir yerde belgeleri) belirtir. Değer gerçekte kilobayt cinsinden belirtilir ve varsayılan değer 80'dir. Varsayılan değer 80 k sağlayarak, biz arabellek üzerinde büyük nesne yığın bitmez emin olun.

## <a name="wizard-control"></a>Sihirbaz Denetimi

ASP.NET geliştiricilerinin "paneller kullanarak sayfa" ya da sayfadan sayfaya aktararak bilgi serisinde toplanmaya çalışılıyor ile zorlanıyor karşılaştığınız oldukça yaygındır. Daha sık çok değil, çaba can sıkıcı bir ve zaman alıcıdır. Yeni sihirbaz denetimi kullanıcılar bilginiz bir sihirbaz arabirimi doğrusal ve doğrusal olmayan adımlarda izin vererek sorunları çözer. Sihirbaz Denetimi girdi biçimleri bir dizi adımı gösterir. Denetim StepType özelliği tarafından belirtilen belirli bir türdeki her adımdır. Kullanılabilir adım türleri aşağıdaki gibidir:

| **Adım türü** | **Açıklama** |
| --- | --- |
| Otomatik | Sihirbaz otomatik olarak adım adım hiyerarşi içindeki konumuna göre türünü belirler. |
| Başlat | Bir giriş ifadesi sunmak için sık kullanılan ilk adımı. |
| Adım | Normal bir adım. |
| Son | Sihirbazı tamamlamak için bir düğmeye sunmak için genellikle kullanılan son adımı. |
| Tamamlayın | Başarı veya başarısızlık iletişim kurulurken bir ileti gösterir. |

> [!NOTE]
> Sihirbaz denetimi, ASP.NET denetim durumu kullanarak durumunu izler. Bu nedenle, EnableViewState özelliği false hiçbir zarar olmadan ayarlanabilir.


Bu videoyu gözden geçirme Sihirbazı'nı denetiminin ' dir.


![](server-controls/_static/image2.png)


[Açık Tam Ekran Video](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Denetim yerelleştirme

Localıze denetimi, bir harf denetimi için benzer. Ancak, Localıze denetimi sahip bir **modu** eklenmeden biçimlendirme nasıl işlendiğine denetimleri özelliği. Aşağıdaki değerleri Mode özelliğini destekler:

| **Modu** | **Açıklama** |
| --- | --- |
| Dönüştürme | İsteği yapan tarayıcının Protokolü göre biçimlendirme dönüştürüldüğünde. |
| Geçiş | Biçimlendirme olarak işlenir-değil. |
| Kodlama | Denetime eklenen biçimlendirme HtmlEncode kullanılarak kodlanır. |

## <a name="multiview-and-view-controls"></a>MultiView ve görünümü denetimleri

MultiView denetiminin görünümü denetimleri için kapsayıcı görevi görür ve görünüm denetimi (çok benzer bir Panel denetimi) diğer denetimler için kapsayıcı olarak davranır. Her bir görünümde MultiView denetiminin tek bir görünüm denetim tarafından temsil edilir. MultiView ilk görünüm denetim görünüm 0, ikinci Görünüm 1, vs. MultiView denetiminin ActiveViewIndex belirterek görünümler arasında geçiş yapabilirsiniz.

## <a name="substitution-control"></a>Değişim Denetimi

Değişim Denetimi ASP.NET önbelleğe alma ile birlikte kullanılır. Burada önbelleğe alma avantajından yararlanmak istiyor, ancak her istekte (önbelleğe alınan muaf tutulan diğer bir deyişle, bölümleri sayfasının) güncelleştirilmesi gereken bir sayfa bölümlerini olması durumda değiştirme bileşeni harika bir çözüm sağlar. Denetimi aslında kendi başına herhangi bir çıktı oluşturmak değil. Bunun yerine, sunucu tarafı kodu yönteminde bağlıdır. Sayfa istendiğinde yöntemi çağrılır ve döndürülen biçimlendirme yerine Değişim Denetimi işlenir.

Değişim Denetimi bağlı yöntemi aracılığıyla belirtilen **MethodName** özelliği. Bu yöntem, aşağıdaki ölçütleri karşılamalıdır:

- Statik (paylaşılan içinde VB) yöntemi olmalıdır.
- Bu, HttpContext türünde bir parametre kabul eder.
- Denetimi sayfasına değiştirmelisiniz biçimlendirme temsil eden bir dize döndürür.

Değişim Denetimi herhangi bir sayfayı denetiminde değiştirme olanağına sahip değildir, ancak onun parametresi üzerinden geçerli HttpContext erişimi yok.

## <a name="gridview-control"></a>GridView denetimi

GridView denetiminin DataGrid denetimini yerini alır. Bu denetim, bir sonraki modüldeki daha ayrıntılı ele alınacaktır.

## <a name="detailsview-control"></a>DetailsView denetimi

DetailsView denetimi, bir veri kaynağından tek bir kaydını görüntülemek ve düzenlemek veya silmek sağlar. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="formview-control"></a>FormView denetimi

FormView denetimi, bir veri kaynağı tek bir kayıttan yapılandırılabilir arabiriminde görüntülemek için kullanılır. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="accessdatasource-control"></a>AccessDataSource denetimi

AccessDataSource denetimi için kullanılan veri Access veritabanını bağlayın. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="objectdatasource-control"></a>ObjectDataSource Denetimi

ObjectDataSource Denetimi denetimleri verilere iki katmanlı bir model aksine bir orta katman iş nesnesine denetimleri doğrudan veri kaynağına bağlı olduğu bağlı üç katmanlı mimari destekleyecek şekilde kullanılır. Daha ayrıntılı bir sonraki modüldeki incelenecektir.

## <a name="xmldatasource-control"></a>XmlDataSource denetimi

XmlDataSource denetim veri bağlamak için bir XML veri kaynağı için kullanılır. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="sitemapdatasource-control"></a>SiteMapDataSource denetimi

SiteMapDataSource denetimi veri bağlama için bir site haritası temel site gezinti denetimlerinin sunar. Daha ayrıntılı bir sonraki modüldeki incelenecektir.

## <a name="sitemappath-control"></a>ASP

ASP Gezinti bağlantıları içerik haritası adlandırılan bir dizi görüntüler. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="menu-control"></a>Menü denetimi

Menü denetimi DHTML kullanarak dinamik menüler görüntüler. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="treeview-control"></a>TreeView Denetimi

TreeView denetimi, veri hiyerarşik ağaç görünümünü görüntülemek için kullanılır. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="login-control"></a>Oturum açma denetimi

Oturum açma denetimi için bir Web sitesine oturum için bir mekanizma sağlar. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="loginview-control"></a>LoginView denetimi

Bir kullanıcının oturum açma durumu temel alınarak farklı şablon görüntüsünü LoginView denetimi sağlar. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="passwordrecovery-control"></a>PasswordRecovery denetimi

PasswordRecovery denetimi, bir ASP.NET uygulaması kullanıcılar tarafından Unutulan parolaları almak için kullanılır. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="loginstatus"></a>loginStatus

Bu denetim, kullanıcının oturum açma durumunu görüntüler. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="loginname"></a>LoginName

LoginName denetimi, bir ASP.NET uygulamasına oturum sonra bir kullanıcının kullanıcı adını görüntüler. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard kullanıcıların bir ASP.NET üyelik hesabı kullanmak için bir ASP.NET uygulaması oluşturma olanağı sunan yapılandırılabilir bir sihirbazıdır. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="changepassword"></a>Parola değiştirme

Bu denetim, kullanıcıların bir ASP.NET uygulaması için parolalarını değiştirip olanak tanır. Bir sonraki modüldeki daha ayrıntılı ele alınmıştır.

## <a name="various-webparts"></a>Çeşitli Web Bölümleri

ASP.NET 2.0 çeşitli Web Bölümleri ile birlikte gelir. Bu ayrıntılı daha yeni bir modül olarak ele alınacaktır.
