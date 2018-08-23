---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 sürümündeki yeni değişiklikler | Microsoft Docs
author: rick-anderson
description: Bu belgede, .NET Framework sürümü için büyük olasılıkla kullanılarak oluşturulan uygulamaları etkileyebilir 4 yayın yapılan değişiklikleri açıklanmıştır...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 112483abdd920649fb530959a538b1d5ed6064d7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756502"
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4 sürümündeki yeni değişiklikler
====================
> Bu belge potansiyel olarak ASP.NET 4 Beta 1 ve 2 Beta sürümleri dahil önceki sürümleri kullanılarak oluşturulan uygulamaları etkileyebilir 4 yayın için .NET Framework sürümü yapılan değişiklikleri açıklar.
> 
> [Bu teknik incelemeyi indirin](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>İçindekiler

[ControlRenderingCompatibilityVersion ayarı Web.config dosyasında](#0.1__Toc256770141 "_Toc256770141")  
[ClientIDMode değişiklikleri](#0.1__Toc256770142 "_Toc256770142")  
[Tek tırnak işaretleri HtmlEncode ve UrlEncode şimdi kodlama](#0.1__Toc256770143 "_Toc256770143")  
[ASP.NET sayfası (.aspx) ayrıştırıcı olan Stricter](#0.1__Toc256770144 "_Toc256770144")  
[Tarayıcı tanımı dosyaları güncelleştirilmiş](#0.1__Toc256770145 "_Toc256770145")  
[System.Web.Mobile.dll kök Web yapılandırma dosyasından kaldırıldı](#0.1__Toc256770146 "_Toc256770146")  
[ASP.NET istek doğrulamayı](#0.1__Toc256770147 "_Toc256770147")  
[Varsayılan karma algoritması, artık HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Yapılandırma hatalarını ilgili yeni bir ASP.NET 4 kök yapılandırmaya](#0.1__Toc256770149 "_Toc256770149")  
[ASP.NET 4 alt uygulamalar için başlangıç 3.5 uygulamaları ASP.NET 2.0 veya ASP.NET altında başarısız](#0.1__Toc256770150 "_Toc256770150")  
[Web siteleri ASP.NET 4 başarısız SharePoint yüklü olduğu bilgisayarlarda başlatmak](#0.1__Toc256770151 "_Toc256770151")  
[HttpRequest.FilePath özelliği artık PATHINFO değerlerini içerir](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 uygulamaları eurl.axd başvuru HttpException hataları oluşturmak](#0.1__Toc256770153 "_Toc256770153")  
[Olay işleyicileri değil oluşmayabilir IIS 7 ya da IIS 7.5 varsayılan bir belge içinde tümleşik modu](#0.1__Toc256770154 "_Toc256770154")  
[Değişiklikleri için ASP.NET kodu erişim güvenliği (CAS) uygulama](#0.1__Toc256770155 "_Toc256770155")  
[MembershipUser ve diğer türleri System.Web.Security Namespace taşınmış](#0.1__Toc256770156 "_Toc256770156")  
[Çıktı önbelleğe alma farklılık değişiklik \* HTTP üstbilgisi](#0.1__Toc256770157 "_Toc256770157")  
[System.Web.Security türleri için Passport: geçersiz](#0.1__Toc256770158 "_Toc256770158")  
[ASP.NET 4 sürümünde bir görüntüsünü işlemek MenuItem.PopOutImageUrl özelliği başarısız](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl ve yolları ters eğik çizgi içerdiğinde görüntülerini işlemek için Menu.DynamicPopOutImageUrl başarısız](#0.1__Toc256770160 "_Toc256770160")  
[Sorumluluk reddi](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Web.config dosyasında ControlRenderingCompatibilityVersion ayarı

ASP.NET denetimleri .NET Framework sürüm 4 biçimlendirme işlenen nasıl daha kesin olarak belirlemenize olanak için değiştirildi. Önceki .NET Framework sürümlerinde, bazı denetimleri devre dışı bırakmak için imkanı yoktu biçimlendirme yayılan. Varsayılan olarak, ASP.NET 4 biçimlendirme bu tür artık oluşturulur.

ASP.NET 2.0 ya da ASP.NET 3.5 uygulamanızı yükseltmek için Visual Studio 2010 kullanıyorsanız, aracı için bir ayarı otomatik olarak ekler. `Web.config` eski işleme koruyan bir dosya. Ancak, bir uygulamayı .NET Framework 4 hedeflemek için IIS uygulama havuzunda değiştirerek yükseltirseniz, ASP.NET yeni işleme modu varsayılan olarak kullanır. Yeni işleme modu devre dışı bırakmak için aşağıdaki ayarı ekleyin. `Web.config` dosyası:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Yeni davranış getiren ana işleme değişiklikleri aşağıdaki gibidir:

- **Görüntü** ve **ImageButton** artık denetimlerini işlemeye bir `border="0"` özniteliği.
- **BaseValidator** ondan türetilen sınıf ve doğrulama denetimleri artık varsayılan olarak kırmızı bir metin işleme.
- **HtmlForm** denetim işleme değil bir **adı** özniteliği.
- **Tablo** denetim artık işler bir `border="0"` özniteliği.
- Kullanıcı girişi için tasarlanmamıştır denetimler (örneğin, **etiket** denetimi) artık işleme `disabled="disabled"` özniteliği kendi **etkin** özelliği **false**(ya da bir kapsayıcı denetimi bu ayar devralıyorsanız).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode değişiklikleri

**ClientIDMode** ayarı ASP.NET 4'te nasıl ASP.NET oluşturur belirtmenize olanak tanır **kimliği** HTML öğeleri için özniteliği. ASP.NET önceki sürümlerinde, varsayılan davranışı eşdeğer **AutoID** ayarıyla **ClientIDMode**. Ancak, artık varsayılan ayar olan **tahmin edilebilir**.

ASP.NET 2.0 ya da ASP.NET 3.5 uygulamanızı yükseltmek için Visual Studio 2010 kullanıyorsanız, aracı için bir ayarı otomatik olarak ekler. `Web.config` .NET Framework'ün önceki sürümlerinde davranışını koruyan bir dosya. Ancak, bir uygulamayı .NET Framework 4 hedeflemek için IIS uygulama havuzunda değiştirerek yükseltirseniz, ASP.NET yeni modu varsayılan olarak kullanır. Yeni istemci kimliği modu devre dışı bırakmak için aşağıdaki ayarı ekleyin. `Web.config` dosyası:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>Tek tırnak işaretleri HtmlEncode ve UrlEncode şimdi kodlayın

ASP.NET 4'te **HtmlEncode** ve **UrlEncode** yöntemlerinin **HttpUtility** ve **HttpServerUtility** sınıfları için güncelleştirildi tek tırnak işareti karakteri (') şu şekilde kodlama:

- **HtmlEncode** yöntem kodlar örneklerini tek tırnak işareti olarak.
- **UrlEncode** yöntem, tek tırnak işareti örneklerini % 27 ' kodlar.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>ASP.NET sayfası (.aspx) ayrıştırıcı Stricter olduğu

ASP.NET sayfaları için sayfa ayrıştırıcısı (`.aspx` dosyaları) ve kullanıcı denetimleri (`.ascx` dosyaları) ASP.NET 4'te daha sıkı ve daha fazla örnek geçersiz biçimlendirme reddeder. Örneğin, aşağıdaki iki kod parçacıkları ASP.NET'in daha önceki sürümlerde başarıyla ayrıştırmak, ancak artık ASP.NET 4'te ayrıştırıcısı hatalarını oluşturacak.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Sonunda geçersiz noktalı virgülü olduğuna dikkat edin **Hiddenfield'i** etiketi.

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Kapatılmamış fark **stili** halinde çalışan özniteliği **CssClass** özniteliği.

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Tarayıcı tanım dosyalarını güncelleştirildi

Tarayıcı tanım dosyalarını, yeni ve güncelleştirilmiş tarayıcılar ve cihazlar hakkındaki bilgileri içerecek şekilde güncelleştirildi. Eski tarayıcılar ve cihazlar Netscape Navigator gibi kaldırılır ve yeni tarayıcılar kaldırıldı ve cihazlar gibi Google Chrome ve Apple iPhone sürümüne eklenmiştir.

Uygulamanız kaldırılmış olan tarayıcı tanımlarını birinden devralan bir özel tarayıcı tanımlarını içeriyorsa bir hata görürsünüz. Örneğin, varsa `App_Browsers` klasörde IE2 tarayıcı tanımından devralan bir tarayıcı tanımı, şu yapılandırma hata iletisini alırsınız:

- Tarayıcı veya ağ geçidi 'IE2' kimlikli öğe bulunamıyor.

> [!NOTE]
> **HttpBrowserCapabilities** nesne (sayfanın tarafından sunulan **Request.Browser** özelliği) tarayıcı tanımlarını dosyaları tarafından yönlendirilir. Bu nedenle, ASP.NET 4'te bu nesnenin bir özelliğine erişerek döndürülen bilgileri ASP.NET'in önceki bir sürümde döndürülen bilgiler farklı olabilir.


Aşağıdaki klasörden tarayıcı tanım dosyalarını kopyalayarak eski tarayıcı tanım dosyalarında döndürebilirsiniz:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Karşılık gelen dosyaları kopyalayın `\CONFIG\Browsers` ASP.NET 4 için klasör. Dosyaları kopyaladıktan sonra ASP.NET çalıştıran\_regbrowsers.exe komut satırı aracı.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System.Web.Mobile.dll kök Web yapılandırma dosyasından kaldırıldı

ASP.NET önceki sürümlerinde, kök dizininde System.Web.Mobile.dll derlemesine bir başvuru eklenmiştir `Web.config` dosyası **derlemeleri** bölümüne. Performansını geliştirmek için bu derlemeye bir başvurusu kaldırıldı.

ASP.NET 4'te System.Web.Mobile.dll derleme dahildir, ancak kullanım dışıdır. System.Web.Mobile.dll derlemesinden türler kullanmak istiyorsanız, bu derlemeye bir başvuru ya da köküne eklemek `Web.config` dosya veya bir uygulamaya `Web.config` dosya. (Kullanım dışı) ASP.NET Mobil denetimleri kullanmak istiyorsanız, örneğin, System.Web.Mobile.dll derlemeye bir başvuru eklemelisiniz `Web.config` dosya.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>ASP.NET isteği doğrulama

ASP.NET isteği doğrulama özelliği belirli bir siteler arası betik (XSS) saldırılarına karşı varsayılan koruma düzeyini sağlar. ASP.NET önceki sürümlerinde, varsayılan olarak istek doğrulama etkinleştirildi. Ancak, yalnızca ASP.NET sayfaları uygulandığında (`.aspx` dosyaları ve bunların sınıf dosyaları) ve yalnızca zaman sayfalar yürütme.

Önce etkin olmadığından ASP.NET 4'te varsayılan olarak, istek doğrulamanın tüm istekler için etkin olup **BeginRequest** bir HTTP isteği aşaması. Sonuç olarak, yalnızca .aspx sayfası istekleri tüm ASP.NET kaynakları istekleri için istek doğrulamayı geçerlidir. Bu Web hizmeti çağrıları ve özel HTTP işleyicilerinin gibi istekleri içerir. Özel HTTP modülleri bir HTTP isteğinin içeriği okurken istek doğrulamayı da etkin değil.

Sonuç olarak, istek doğrulama hataları daha önce hataları tetiklendi değil istekleri için artık ortaya çıkabilir. ASP.NET 2.0 istek doğrulama özelliği davranışını geri almak için aşağıdaki ayarı ekleyin. `Web.config` dosyası:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Bununla birlikte, mevcut işleyiciler, modüller veya diğer özel kod potansiyel olarak erişen olmadığını XSS olabilir güvenli olmayan HTTP girişleri vektörleri saldırı belirlemek için tüm istek doğrulama hatalarını çözümleme öneririz.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Varsayılan karma algoritması HMACSHA256 sunulmuştur

ASP.NET formları kimlik doğrulama tanımlama bilgileri gibi verilerin korunmasına yardımcı olun ve durumunu görüntülemek için şifreleme ve karma algoritmaları kullanır. Varsayılan olarak, ASP.NET 4 artık HMACSHA256 algoritması tanımlama bilgilerini ve görünüm durumu üzerinde karma işlemler için kullanır. ASP.NET'in önceki sürümlerinde, eski HMACSHA1 algoritma kullanılır.

Karma ASP.NET 2.0/ASP.NET 4 çalıştırırsanız, uygulamalarınızın etkilenebilecek ortamlar nerede forms kimlik doğrulaması tanımlama bilgileri gibi gerekir çalışmanızı across.NET Framework sürümü. Eski HMACSHA1 algoritmasını kullanmak için bir ASP.NET 4 Web uygulamasını yapılandırmak için aşağıdaki ayarı ekleyin. `Web.config` dosyası:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Yeni ASP.NET 4 kök yapılandırması ile ilgili yapılandırma hataları

Kök yapılandırma dosyalarını ( `machine.config` dosya ve kök `Web.config` dosyası) için .NET Framework 4 (ve bu nedenle ASP.NET 4), ASP.NET 3.5 içinde bulundu ortak yapılandırma bilgilerini çoğunu içerecek şekilde güncelleştirildi Uygulama `Web.config` dosyaları. Yönetilen IIS 7 ve IIS 7.5 yapılandırma sistemi karmaşıklığı nedeniyle, ASP.NET 3.5 uygulamaları ASP.NET 4 altında ve IIS 7 ve IIS 7.5 altında çalışan ASP.NET veya IIS yapılandırma hataları neden olabilir.

ASP.NET 3.5 uygulamaları ASP.NET 4 için Visual Studio 2010'da, proje yükseltme araçları kullanarak mümkünse yükseltmenizi öneririz. Visual Studio 2010 otomatik olarak değiştirir ASP.NET 3.5 uygulamanın `Web.config` ASP.NET 4 için uygun ayarları içeren dosyaya.

Ancak, .NET Framework 4 gerekmeksizin kullanarak ASP.NET 3.5 uygulamaları çalıştırmak için desteklenen bir senaryo değildir. Bu durumda, uygulamanın el ile değiştirmeniz gerekebilir `Web.config` .NET Framework 4 altında IIS 7 veya IIS 7.5 ve uygulamayı çalıştırmadan önce dosya.

Sonraki iki bölümde, farklı yazılım bileşimleri için yapmanız gerekebilecek değişiklikleri açıklar.

**Ne düzeltme KB958854 ya da SP2 yüklendiği Windows Vista SP1 veya Windows Server 2008 SP1.** Bu yapılandırmada, IIS 7 yapılandırma sistemi yanlış yönetilen uygulama yapılandırma uygulama düzeyi karşılaştırarak birleştirir `Web.config` ASP.NET 2.0 dosyasına `machine.config` dosyaları. Bu, uygulama düzeyi nedeniyle `Web.config` dosyaları .NET Framework 3.5 veya üzeri olmalıdır bir **system.web.extensions** bir IIS 7 doğrulama hatasına neden olmayan sırayla yapılandırma bölümü tanımı (öğe).

Ancak, uygulama düzeyinde el ile değişiklik `Web.config` tam olarak Visual Studio 2008 ile sunulan özgün ortak yapılandırma bölümü tanımları eşleşmeyen dosya girişleri, ASP.NET yapılandırma hataları neden olur. (Visual Studio 2008 tarafından oluşturulan varsayılan yapılandırma girdileri düzgün çalışmaz.) Ortak bir sorunu el ile değiştirilmiş olduğundan `Web.config` dosyaları dışlamayı **allowDefinition** ve **requirePermission** çeşitli yapılandırma bölümünde bulunan configuration öznitelikleri tanımları. Bu uygulama düzeyinde kısaltılmış yapılandırma bölümünde arasında bir uyuşmazlık neden `Web.config` dosyaları ve ASP.NET 4'te tam tanımı `machine.config` dosya. Sonuç olarak, çalışma zamanında, bir yapılandırma hatası ASP.NET 4 yapılandırma sistemi oluşturur.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2'de ve ayrıca Windows Vista SP1 ve Windows Server 2008 SP1 düzeltmesi KB958854 yüklendiği.**

Metin karşılaştırması gerçekleştirir, çünkü bu senaryoda, IIS 7.5 ve IIS 7 yerel yapılandırma sistemi bir yapılandırma hatası döndürür. **türü** yönetilen yapılandırma bölümü işleyicisi için tanımlanan öznitelik. Çünkü tüm `Web.config` Visual Studio 2008 ve Visual Studio 2008 SP1 tarafından oluşturulan dosyaları sahip türü dizesi "3.5" **system.web.extensions** (ve ilgili) yapılandırma bölümü işleyicileri ve ASP.NET 4 `machine.config` dosyası "4.0" sahip **türü** yapılandırma doğrulama IIS 7'de Visual Studio 2008 veya Visual Studio 2008 SP1'i her zaman oluşturulan uygulamalar aynı yapılandırma bölümü işleyicileri başarısız özniteliği ve IIS 7.5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Bu sorunları çözme

Geçici çözüm ilk senaryosu için uygulama düzeyi güncelleştirmektir `Web.config` ortak yapılandırma metinden ekleyerek, dosya bir `Web.config` Visual Studio 2008 tarafından otomatik olarak oluşturulan dosya.

İlk senaryo için alternatif bir geçici çözüm Vista veya Windows Server 2008 Service Pack 2 bilgisayarınıza yüklemek için düzeltmeyi KB958854 yüklemek için mi ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) yanlış yapılandırma-birleştirme davranışı düzeltmek için IIS yapılandırma sistemi. Ancak, bu eylemlerden biri gerçekleştirildikten sonra uygulamanızın büyük olasılıkla bir yapılandırma hatası için ikinci senaryoda açıklanan sorunu nedeniyle karşılaşır.

İkinci senaryoda geçici çözümü silin veya tüm açıklama olmaktır **system.web.extensions** grup yapılandırma bölümü tanımları ve yapılandırma bölümü, uygulama düzeyinde tanımlarından `Web.config` dosya. Bu genellikle uygulama düzeyi üst kısmında tanımlardır `Web.config` tarafından tanımlanabilir ve dosya **configSections** öğe ve alt öğeleri.

Her iki senaryo için de el ile silmeniz önerilir **system.codedom** bölümünde ancak bu gerekli değildir.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>ASP.NET 4 alt uygulamalar başarısız 3.5 uygulamaları için başlangıç ASP.NET 2.0 veya ASP.NET altında

ASP.NET 4 alt ASP.NET'in önceki sürümlerinde çalışan uygulamalar, yapılandırma veya derleme hataları nedeniyle başlatmak başarısız olabilir olarak yapılandırılan uygulamalar. Aşağıdaki örnek, etkilenen bir uygulama için bir dizin yapısı gösterilmektedir.

`/parentwebapp` (ASP.NET 2.0 veya ASP.NET 3.5 kullanacak şekilde yapılandırılmış)  
`/childwebapp` (ASP.NET 4 kullanacak şekilde yapılandırılmış)

Uygulamada `childwebapp` klasör IIS 7.5 veya IIS 7'yi başlatın ve rapor bir yapılandırma hatasıyla başarısız olur. Hata metni aşağıdakine benzer bir ileti içerir:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

IIS 6, uygulamada `childwebapp` klasör de başarısız olur başlatmak, ancak farklı bir hata rapor eder. Örneğin, hata metni şu durum:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Bu senaryolar meydana üst uygulamadaki yapılandırma bilgileri `parentwebapp` klasör hiyerarşisi alt web tarafından kullanılan son birleştirilmiş yapılandırma ayarlarını belirleyen yapılandırma bilgilerinin bir parçasıdır Uygulama `childwebapp` klasör. ASP.NET 4 Web uygulamasını IIS 7 (veya IIS 7.5) veya IIS 6 kullanılıp kullanılmadığını bağlı olarak, IIS yapılandırma sistemini ya da ASP.NET 4 derleme sistemi, bir hata döndürür.

Bu sorunu çözmek ve uygulamanın çalışması için ASP.NET 4 alt almak için izlemeniz gereken adımları ASP.NET 4 uygulaması IIS 6 veya IIS 7 (veya IIS 7.5) çalıştığına göre değişir.

### <a name="step-1-iis-7-or-iis-75-only"></a>Adım 1 (IIS 7 ya da yalnızca IIS 7.5)

Bu adım, yalnızca IIS 7 çalıştıran işletim sistemleri veya Windows Vista, Windows Server 2008, Windows 7 ve Windows Server 2008 R2 içeren IIS 7.5 için gereklidir.

Taşıma **configSections** tanımında `Web.config` dosya üst uygulamanın (ASP.NET 2.0 ya da ASP.NET 3.5 çalıştıran uygulama) kök oturum `Web.config` .NET Framework 2.0 için dosya. IIS 7 ve IIS 7.5 yerel yapılandırma sistemi tarar **configSections** öğesini hiyerarşi yapılandırma dosyalarının birleştirir. Taşıma **configSections** ebeveynin Web uygulaması tanımından `Web.config` kök dosya `Web.config` dosya etkili bir şekilde alt ASP.NET 4 için ortaya çıkan yapılandırma birleştirme işlemini öğeyi gizler uygulama.

32 bit işletim sisteminde veya 32 bit uygulama havuzları, kök `Web.config` ASP.NET 2.0 ve ASP.NET 3.5 için dosya normal olarak aşağıdaki klasörde bulunur:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

64-bit işletim sisteminde veya 64-bit uygulama havuzları, kök `Web.config` ASP.NET 2.0 ve ASP.NET 3.5 için dosya normal olarak aşağıdaki klasörde bulunur:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Bir 64 bit bilgisayarda 32 bit ve 64-bit hem Web uygulamaları çalıştırırsanız, taşımanız **configSections** yukarı kök öğesine `Web.config` hem 32 bit hem de 64-bit sistemler için dosyaları.

Yerleştirdiğinizde **configSections** kök öğesinde `Web.config` dosya, bölümüne yapıştırın hemen sonra **yapılandırma** öğesi. Aşağıdaki örnek, hangi üst kısmını kök gösterir `Web.config` dosya öğeleri taşıma bittiğinde gibi görünmelidir.

> [!NOTE]
> Aşağıdaki örnekte, satırı okunabilirlik açısından sarmalanmış.


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Adım 2 (IIS tüm sürümleri)

Bu adım, Web uygulamasını ASP.NET 4 alt IIS 6 ya da IIS 7 (veya IIS 7.5) kullanılıp kullanılmadığını gereklidir.

İçinde `Web.config` dosya ve üst ASP.NET 2 veya ASP.NET 3.5, çalışan bir Web uygulaması Ekle bir **konumu** (IIS ve ASP.NET yapılandırma sistemi için) açıkça belirten etiketi, yalnızca yapılandırma girdileri üst Web uygulaması için geçerlidir. Aşağıdaki örnek söz dizimi görülmektedir **konumu** öğesi eklemek için:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

Aşağıdaki örnekte gösterildiği nasıl **konumu** etiketi ile başlayan tüm yapılandırma bölümlerini sarmalamak için kullanılan **appSettings** bölümünde ve ile biten **system.webServer**bölümü.

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

1. ve 2. adımları tamamladıktan sonra alt ASP.NET 4 Web uygulamaları hatasız başlar.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 Web siteleri, SharePoint yüklü olduğu bilgisayarlarda başlatılamadı

SharePoint çalıştıran web sunucusuna sahip bir `Web.config` bir SharePoint Web sitesinin kök dizininde dağıtılan dosya (örneğin, `c:\inetpub\wwwroot\web.config` varsayılan Web sitesi için). Bu `Web.config` dosya, SharePoint ayarlar özel bir kısmi güven düzeyi WSS adlı\_en az.

Bu tür SharePoint Web sitesi bir alt sitesi olarak dağıtılan bir ASP.NET 4 Web sitesi çalıştırmayı denerseniz, aşağıdaki hatayı görürsünüz:

`Could not find permission set named 'ASP.NET'.`

ASP.NET 4 kod erişim güvenliği (CAS) altyapısını ASP.NET adlı bir izin kümesi için göründüğünden, bu hata oluşur. Ancak, kısmi güven WSS tarafından başvurulan yapılandırma dosyası\_en az bu ada sahip tüm izin kümeleri içermiyor.

Şu anda yok ASP.NET ile uyumludur SharePoint sürümü. Sonuç olarak, ASP.NET 4 Web sitesi alt site altında SharePoint Web sitelerini çalıştırmak çalışmamalıdır.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>HttpRequest.FilePath özelliği artık PATHINFO değerlerini içerir

ASP.NET dahil önceki sürümlerini bir **PATHINFO** dahil olmak üzere çeşitli dosya yolu ile ilgili özellikleri, döndürülen değer değerinde **HttpRequest.FilePath**,  **HttpRequest.AppRelativeCurrentExecutionFilePath**, ve **HttpRequest.CurrentExecutionFilePath**. ASP.NET 4 artık içerir **PATHINFO** bu özellikleri dönüş değerleri değeri. Bunun yerine, **PATHINFO** bilgi sağlanmıştır **HttpRequest.PathInfo**. Örneğin, aşağıdaki URL parçası belirtmesini düşünün:

`/testapp/Action.mvc/SomeAction`

ASP.NET'in önceki sürümlerinde **HttpRequest** özellikleri, aşağıdaki değerlere sahip:

**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (boş)

ASP.NET 4'te **HttpRequest** özellikleri, bunun yerine aşağıdaki değerlere sahip:

**HttpRequest.FilePath**: `/testapp/Action.mvc`

**HttpRequest.PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 uygulamaları eurl.axd başvuru HttpException hataları oluşturmak

ASP.NET 4, IIS 6'da etkinleştirildikten sonra IIS 6 ' (', Windows Server 2003 veya Windows Server 2003 R2) üzerinde çalışan ASP.NET 2.0 uygulamaları aşağıdaki gibi hataları verebilir:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

ASP.NET Web sitesinde ASP.NET 4 kullanacak şekilde yapılandırıldığını algıladığında, ASP.NET 4'ün yerel bir bileşeni uzantısız bir URL'ye ASP.NET yönetilen bölümüne daha ayrıntılı işleme için geçirdiği için bu hata oluşur. ASP.NET 4 Web sitesi altında olan sanal dizinleri ASP.NET 2.0 kullanmak üzere yapılandırılmış, ancak "eurl.axd" dizesini içeren değiştirilmiş bir URL bu şekilde sonucu uzantısız URL işleniyor. Değiştirilen bu URL'yi daha sonra ASP.NET 2.0 uygulamaya gönderilir. ASP.NET 2.0 "eurl.axd" biçimi tanınmıyor. Bu nedenle, ASP.NET 2.0 adlı bir dosya bulmaya çalışır `eurl.axd` ve yürütün. Böyle bir dosya var olduğundan istek başarısız olur bir **HttpException** özel durum.

Aşağıdaki seçeneklerden birini kullanarak bu sorunu geçici olarak çalışabilir.

### <a name="option-1"></a>Seçenek 1

ASP.NET 4 Web sitesi çalıştırmak için gerekli değilse, siteyi ASP.NET 2.0 yerine kullanmak üzere yeniden eşleyin.

### <a name="option-2"></a>Seçenek 2

ASP.NET 4 Web sitesi çalıştırmak için gerekli ise, tüm alt ASP.NET 2.0 sanal dizinleri ASP.NET 2.0 için eşlenmiş başka bir Web sitesine taşıyın.

### <a name="option-3"></a>Seçenek 3

ASP.NET 2.0 Web sitesine yeniden eşlemek için veya bir sanal dizin konumunu değiştirmek için pratik değilse, ASP.NET 4'te işleme uzantısız URL açıkça devre dışı bırakın. Aşağıdaki yordamı kullanın:

1. Windows kayıt defterinde aşağıdaki düğüm açın:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Yeni bir **DWORD** değeri **EnableExtensionlessUrls**.
2. Ayarlama **EnableExtensionlessUrls** 0. Uzantısız URL davranışı, devre dışı bırakır.
3. Kayıt defteri değerini kaydedin ve Kayıt Defteri Düzenleyicisi'ni kapatın.
4. Çalıştırma **iisreset** yeni kayıt defteri değerini okumak IIS neden olan komut satırı aracı.

> [!NOTE]
> Ayarı **EnableExtensionlessUrls** 1 uzantısız URL davranışını etkinleştirir. Hiçbir değer belirtilmemişse varsayılan ayar budur.


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Olay işleyicileri değil oluşmayabilir IIS 7 ya da IIS 7.5 varsayılan bir belge içinde tümleşik modu

ASP.NET 4 değiştirme değişiklikleri içeren nasıl **eylem** HTML öznitelik **form** öğesi için varsayılan bir belge uzantısız bir URL'ye çözümler zaman işlenir. Bir örnek için varsayılan bir belge çözme uzantısız URL [ http://contoso.com/ ](http://contoso.com/), sonuçta elde edilen bir istekte [ http://contoso.com/Default.aspx ](http://contoso.com/Default.aspx).

ASP.NET 4, artık HTML işleyen **form** öğenin **eylem** eşlenmiş bir varsayılan belge uzantısız bir URL'ye bir istekte bulunulduğunda bir boş dize olarak öznitelik değeri. Örneğin, bir istek, ASP.NET'in önceki sürümlerinde de [ http://contoso.com ](http://contoso.com) istek sonuçlanır `Default.aspx`. Bu belgede, açılış **form** etiketi aşağıdaki örnekte olduğu gibi işlenen:

`<form action="Default.aspx" />`

ASP.NET 4, bir istek [ http://contoso.com ](http://contoso.com) da sonuçları bir istekte `Default.aspx`. Ancak, ASP.NET HTML açılış artık işler **form** etiketi aşağıdaki örnekte olduğu gibi:

`<form action="" />`

Bu fark nasıl **eylem** özniteliği işlenen nasıl bir form post IIS ve ASP.NET tarafından işlenir hafif değişiklikler neden olabilir. Zaman **eylem** özniteliktir boş bir dize IIS **DefaultDocumentModule** nesne alt istek oluşturur `Default.aspx`. Çoğu koşullar altında bu alt istek uygulama kodu için saydamdır ve `Default.aspx` sayfa normal şekilde çalışır.

Ancak, yönetilen kod ve IIS 7 veya IIS 7.5 Tümleşik modu arasındaki olası bir etkileşim yönetilen .aspx sayfası çalışmayı alt istek sırasında düzgün bir şekilde durmasına neden olabilir. Aşağıdaki durumlar ortaya varsa, alt isteğine bir `Default.aspx` belge bir hata ya da beklenmeyen davranışlara neden olur:

1. .Aspx sayfasına bir tarayıcı ile gönderilen **form** öğenin **eylem** özniteliğini "".
2. Form, ASP.NET ile paylaşılır.
3. Yönetilen bir HTTP modülü, varlık gövdesini kısmı okur. Örneğin, bir modül okur **Request.Form** veya **Request.Params**. Bu, yönetilen belleğe okumak için POST isteğinin varlık gövdesini neden olur. Sonuç olarak, varlık gövdesini artık yerel kod modüllerin IIS 7 veya IIS 7.5 Tümleşik modunda çalışan kullanılabilir.
4. IIS **DefaultDocumentModule** nesne sonunda çalışan ve bir alt istek oluşturan `Default.aspx` belge. Ancak, varlık gövdesini yönetilen kod parçasını tarafından zaten okunmuş olduğundan, hiçbir Varlık gövdesi için alt isteği göndermek kullanılabilir yoktur.
5. HTTP ardışık düzen çalıştığında alt istek işleyicisi için `.aspx` dosyaları handler-execute aşamasında çalıştırır.
6. Hiçbir varlık gövdesini olduğundan, hiçbir form değişkeni ve hiçbir görünüm durumu vardır ve bu nedenle hiçbir bilgi .aspx sayfası işleyici (varsa) hangi olay oluşturulması geç belirlemek kullanılabilir. Bunun sonucunda, etkilenen .aspx sayfasına geri gönderme olay işleyicileri hiçbiri çalıştırın.

Aşağıdaki yollarla bu davranışa geçici çözüm bulabilirsiniz:

- İsteğin varlık gövdesine varsayılan belge istekleri sırasında erişen HTTP modülü tanımlamak ve yalnızca yönetilen isteklerini çalıştırmak için yapılandırılabilir olup olmadığını belirler. HTTP modüllerinden hem IIS 7 ve IIS 7.5 için tümleşik modda, yalnızca yönetilen istekleri için aşağıdaki öznitelik modülün ekleyerek çalıştırmak için işaretlenebilir **system.webServer/modules** girişi:

- `precondition="managedHandler"`

- IIS 7 ve IIS 7.5 değil olacak şekilde belirlemek için modülün istekleri bu ayarı devre dışı bırakır, istekleri yönetilen. Varsayılan Belge istekler için ilk isteği için uzantısız bir URL'dir. Bu nedenle, IIS işaretlenmiş tüm yönetilen modülleri ile yönetilen işleyicinin önkoşulu ilk isteği işleme sırasında çalışmaz. Sonuç olarak, yönetilen modülleri yanlışlıkla varlık gövdesini okuyacaksa değil ve bu nedenle, varlık gövdesini hala kullanılabilir ve alt istek ve varsayılan belge boyunca sonuna geçirilir.

- Sorunlu HTTP modüller için tüm istekleri çalıştırmasını varsa (çözümlenmesi uzantısız URL'ler için statik dosyaların **DefaultDocumentModule** nesnesi yönetilen istekleri, vb.), etkilenen .aspx sayfaları tarafından açıkça değiştirme ayarı **eylem** özelliği sayfanın **System.Web.UI.HtmlControls.HtmlForm** denetlemek için boş olmayan bir dize. Örneğin, varsayılan belge ise `Default.aspx`, açıkça ayarlamak üzere kod sayfanın değiştirme **HtmlForm** denetimin **eylem** "Default.aspx" özelliğini.

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>ASP.NET kodu erişim güvenliği (CAS) uygulama değişiklikleri

ASP.NET 2.0 ve uzantısı tarafından 3. 5 ', eklenen ASP.NET özelliklerini kullanabilirsiniz .NET Framework 1.1 ve 2.0 kod erişim güvenliği (CAS) modeli. Ancak, CA'ların ASP.NET 4'te uygulama önemli ölçüde overhauled olmuştur. Sonuç olarak, genel derleme önbelleğinde (GAC) çalıştıran güvenilen kod dayanan kısmi güven ASP.NET uygulamaları, çeşitli güvenlik özel durumlarıyla başarısız olabilir. CAS ilkesini makine kapsamlı değişikliklerin kısmi güven uygulamaları, ayrıca güvenlik özel durumlarıyla başarısız olabilir.

ASP.NET 1.1 ve 2.0 kullanarak yeni kısmi güven ASP.NET 4 uygulamaları davranışını geri dönebilirsiniz **legacyCasModel** özniteliğini **güven** aşağıdaki örnekte gösterildiği gibi yapılandırma öğesi :

`<trust level= "Medium" legacyCasModel="true" />`

Eski CAS modele geri döndüğünüzde, aşağıdaki eski CAS davranışları etkinleştirilir:

- Makine CAS ilkesi uygulanır.
- Tek bir uygulama etki alanında birden çok farklı izin kümeleri izin verilir.
- Açık izin onaylar, yalnızca ASP.NET ya da diğer .NET Framework kodunda yığın üzerinde olduğunda, çağrılan GAC içindeki derlemeler için gerekli değildir.

Senaryolardan biri .NET Framework 4'te döndürülemiyor: Web olmayan kısmi güven uygulamaları artık belirli API'ler System.Web.dll ve System.Web.Extensions.dll çağırın. Önceki .NET Framework sürümlerinde, açıkça verilmesi Web olmayan kısmi güven uygulamaları için olası <strong>AspNetHostingPermission</strong> izinleri. Bu uygulamalar sonra kullanabilir <strong>System.Web.HttpUtility</strong>, türlerini <strong>System.Web.ClientServices.\< / strong > * ad alanları ve türler ilgili üyelik, roller ve Profiller. Bu tür kısmi güven olmayan Web uygulamalarından çağırma, .NET Framework 4'te artık desteklenmiyor.

> [!NOTE]
> **HtmlEncode** ve **HtmlDecode** işlevselliğini **System.Web.HttpUtility** sınıfı, yeni .NET Framework 4'e taşındı  **System.Net.WebUtility** sınıfı. Kullanılan tek ASP.NET işlevsellik oluşturduysanız, uygulamanın kodu yeni değiştirme **WebUtility** bunun yerine sınıf.


Varsayılan CA uygulamasını ASP.NET 4'te yapılan değişiklikler üst düzey bir özeti verilmiştir:

- ASP.NET uygulama etki alanları homojen bir uygulama etki alanları sunulmuştur. Kısmi güven ve tam güven izni kümeleri yalnızca bir uygulama etki alanında kullanılabilir.
- Kısmi güven verme ayarlar, tüm kurumsal düzeyde, makine düzeyinde veya kullanıcı düzeyi CAS ilkeden bağımsızdır.
- .NET Framework 4 saydamlık modeli kullanmak için 3.5 ve 3.5 SP1 ' sunulan ASP.NET derlemeleri dönüştürüldü.
- ASP.NET kullanımını **AspNetHostingPermission** özniteliği önemli ölçüde düşürüldü. Bu özniteliğin en çok örneği genel ASP.NET API'lerinden kaldırıldı.
- ASP.NET derleme sağlayıcıları tarafından oluşturulan dinamik olarak derlenmiş derlemeleri açıkça derlemeler saydam olarak işaretlemek için güncelleştirilmiştir.
- Tüm ASP.NET derlemeleri artık APTCA özniteliği yalnızca Web barındırma ortamları dikkate alınır şekilde işaretlenir. Kısmen güvenilir olmayan bir Web barındırma ortamları ClickOnce gibi ASP.NET derlemelerine çağırmak mümkün olmayacaktır.

Yeni ASP.NET 4 kod erişim güvenlik modeli hakkında daha fazla bilgi için bkz: [ASP.NET uygulamalarında kod erişim güvenliği kullanarak](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) MSDN Web sitesinde.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>MembershipUser ve diğer türleri System.Web.Security Namespace taşındı

ASP.NET üyelik kullanılan bazı türler den taşınmış olan `System.Web.dll` yeni System.Web.ApplicationServices.dll derleme. Türleri türler genişletilmiş .NET Framework SKU'ları ve istemci arasında Mimari katman bağımlılıkları çözümlemek için taşındı.

Web sitesi projeleri System.Web.ApplicationServices.dll varsayılan olarak kullanılan başvurulmuş derlemelerin listesini için ASP.NET derleme sistemi tarafından eklenmiş olduğundan bu tür taşıma sonucunda sorunları yok. Visual Studio 2010'da açarak ASP.NET'ten ASP.NET 4'ün önceki bir sürümü kullanılarak oluşturulmuş bir Web sitesi projesini yükseltirseniz, projenin hatasız derlenir.

Benzer şekilde, Visual Studio 2010'da açarak ASP.NET'ten ASP.NET 4'ün önceki bir sürümde oluşturulmuş bir Web uygulaması projesini yükseltirseniz, yükseltme işlemi System.Web.ApplicationServices.dll referansı projeye ekler. Bu nedenle, Web Uygulama projeleri de hatasız derlenir yükseltildi.

Üyelik türü için farklı bir derleme taşınan olsa bile ASP.NET'in önceki sürümleri kullanılarak oluşturulan derlenmiş (ikili) dosyaları hatasız ASP.NET 4 üzerine de çalıştırın. Tür bilgileri iletme için ASP.NET 4 sürümünde eklendiğini `System.Web.dll` , otomatik olarak bu tür için çalışma zamanı başvuruları türleri için yeni konuma yönlendirir.

Ancak, belirli bir üyelik türleri kullanan ve ASP.NET'in önceki sürümlerden yükseltilmiş sınıf kitaplıkları, ASP.NET 4 projesinde kullanıldığında derlemek başarısız olur. Örneğin, bir sınıf kitaplığı projesi, derlemek ve aşağıdaki gibi bir hata rapor başarısız olabilir:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Bu sorunu çözmek için System.Web.ApplicationServices.dll, sınıf kitaplığı projesinde başvuru ekleyerek çalışabilir.

Aşağıdaki liste gösterir *System.Web.Security* öğesinden taşındı türleri `System.Web.dll` System.Web.ApplicationServices.dll için:

- *System.Web.Security.MembershipCreateStatus*
- *System.Web.Security.Membership.CreateUserException*
- *System.Web.Security.MembershipPasswordException*
- *System.Web.Security.MembershipPasswordFormat*
- *System.Web.Security.MembershipProvider*
- *System.Web.Security.MembershipProviderCollection*
- *System.Web.Security.MembershipUser*
- *System.Web.Security.MembershipUserCollection*
- *System.Web.Security.MembershipValidatePasswordEventHandler*
- *System.Web.Security.ValidatePasswordEventArgs*
- *System.Web.Security.RoleProvider*
- <a id="0.1_a"></a>*System.Web.Configuration.MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>Çıktı önbelleğe alma farklılık değişiklik \* HTTP üstbilgisi

ASP.NET 1. 0'da, belirtilen sayfaları kaynaklanan bir hata önbelleğe `Location="ServerAndClient"` yaymak için bir çıkış – önbellek ayarı olarak bir `Vary:*` HTTP yanıt üst bilgisi. Bu, hiçbir zaman sayfasını yerel olarak önbelleğe alma, istemci tarayıcıları belirten etkisini vardı.

ASP.NET 1.1 içinde **System.Web.HttpCachePolicy.SetOmitVaryStar** yöntemi eklendi, hangi bastırmak için çağırabilir `Vary:*` başlığı. Bu yöntem, yayılan HTTP üst bilgisini değiştirerek bir bozucu olabilecek olarak kabul edildiği için seçildi zamanında. Ancak, geliştiricilerin yanıltıcı ASP.NET davranış tarafından ve hata raporlarını önerilen geliştiriciler mevcut farkında **SetOmitVaryStar** davranışı.

ASP.NET 4'te, kök sorunu düzeltmeye karar yapıldı. `Vary:*` HTTP üstbilgisi artık aşağıdaki yönerge belirttiğiniz alınan yanıtları yayılan:

`<%@OutputCache Location="ServerAndClient" %>`

Sonuç olarak, **SetOmitVaryStar** engellemek için artık gerekli `Vary:*` başlığı.

Belirttiğiniz uygulamalarda `Location="ServerAndClient"` içinde **@ OutputCache** yönerge sayfasında, şimdi adına göre örtük davranışı göreceksiniz **konumu** olan özniteliğin değeri –, sayfaları olacaktır önbelleğe alınabilir, arama gerekmeden tarayıcıda **SetOmitVaryStar** yöntemi.

Uygulamanızdaki sayfalar yaymalıdır varsa `Vary:*`, çağrı **AppendHeader** yöntemi, aşağıdaki örnekte olduğu gibi:

`HttpResponse.AppendHeader("Vary","*");`

Alternatif olarak, çıktı önbelleği değerini değiştirebilirsiniz **konumu** "Sunucuya" özniteliği.

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>System.Web.Security Passport eski türler

ASP.NET 2.0 ile oluşturulmuş Passport desteği Passport (artık LiveId) değişiklikleri nedeniyle birkaç yıl için eski ve desteklenmeyen olmuştur. Passport beş tür sonuç olarak, ilgili **System.Web.Security** artık ile işaretlenmiş **ObsoleteAttribute** özniteliği.

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>ASP.NET 4 sürümünde bir görüntüsünü işlemek MenuItem.PopOutImageUrl özelliği başarısız olur

ASP.NET 3.5 *MenuItem.PopOutImageUrl* özelliği bir menü öğesi dinamik bir alt menü öğesini belirtmek için görüntülenen resim URL'sini belirtmenize olanak sağlar. Aşağıdaki örnek, ASP.NET 3.5 biçimlendirme içinde bu özelliği belirtmeniz gösterilmektedir.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

ASP.NET 4'te bir tasarım değişikliği sonucu için hiç çıkış işlenen *PopOutImageUrl* özelliği için ayarlanmışsa *MenuItem* sınıfı. Bunun yerine, doğrudan bir resim URL'si belirtin *menü* kullanarak denetim *StaticPopOutImageUrl* özelliği veya *DynamicPopOutImageUrl* özelliği. Statik bir menü ile çalışırken *Menu.StaticPopOutImageUrl* özelliği, aşağıdaki örnekte gösterildiği gibi statik menü öğesi bir alt menüye sahip olduğunu belirtmek için görüntülenen görüntünün URL'sini belirtir:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Bir Dinamik menü ile çalışıyorsanız, kullandığınız *Menu.DynamicPopOutImageUrl* özelliği dinamik menü öğesi bir alt menüye sahip olduğunu gösteren bir görüntü URL'sini belirtin. Aşağıdaki örnek Öncekine benzer, ancak nasıl ayarlanacağını gösterir *DynamicPopOutImageUrl* özelliği için bir Dinamik menü.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Varsa *Menu.DynamicPopOutImageUrl* özelliği ayarlı değil ve *Menu.DynamicEnableDefaultPopOutImage* özelliği *false*, hiçbir resim görüntülenir. Benzer şekilde, varsa *StaticPopOutImageUrl* özelliği ayarlı değil ve *StaticEnableDefaultPopOutImage* özelliği *false*, hiçbir resim görüntülenir.

Bu özellikler yollarını ayarlarsanız, ters eğik çizgi yerine eğik çizgi (/) kullanın (\). Daha fazla bilgi için [Menu.StaticPopOutImageUrl ve işleme görüntüleri olduğunda yolları içeren ters eğik çizgi Menu.DynamicPopOutImageUrl başarısız](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu.") başka bir yerde bu belgede.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Yolları ters eğik çizgi içerdiğinde görüntülerini işlemek Menu.StaticPopOutImageUrl ve Menu.DynamicPopOutImageUrl başarısız

ASP.NET 4, belirttiğiniz kullanarak görüntüleri *Menu.StaticPopOutImageUrl* ve *Menu.DynamicPopOutImageUrl* özellikleri değil işleme yol backlashes içeriyorsa (\). Bu, ASP.NET'in önceki sürümlerinden bir değişikliktir.

Aşağıdaki örnek, *menü* denetim biçimlendirme gösterir *StaticPopOutImageUrl* ters eğik çizgi içeren bir yol kullanılarak ayarlanan özellik. ASP.NET 4'te özelliğinde belirtilen görüntünün işlenmez.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Bu sorunu düzeltmek için belirtilen yol değerlerini değiştirmek *StaticPopOutImageUrl* ve *DynamicPopOutImageUrl* eğik çizgi (/) kullanmak için özellikler. Aşağıdaki örnek, bu değişiklik gösterir:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

ASP.NET'ten ASP.NET 4'ün önceki sürümlerinden geçirilen uygulamaları da unutmayın etkilenmez, çünkü *MenuItem.PopOutImageUrl* özelliği değiştirildi. Daha fazla bilgi için [MenuItem.PopOutImageUrl ASP.NET 4'te bir görüntüsünü işlemek için özellik başarısız](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert") bu belgedeki başka bir yerde.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Sorumluluk reddi

Bu, Başlangıç belgesi ve burada açıklanan yazılım son ticari sürümünden önce önemli ölçüde değiştirilebilir.

Bu belgede yer alan bilgileri, Microsoft Corporation'ın geçerli görünümü tarih itibariyle doğrudur ele alınan sorunlar temsil eder. Microsoft'un değişen piyasa koşullarına yanıt vermesi gerekir çünkü Microsoft tarafında taahhüdü olarak yorumlanmamalıdır ve Microsoft yayın tarihinden sonra sunulan bilgilerin doğruluğunu garanti edemez.

Bu teknik incelemeyi yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ.

İlgili tüm telif hakkı yasalara uymak kullanıcının sorumluluğundadır. Telif Hakkı altındaki sınırlamaksızın, bu belgenin hiçbir bölümü çoğaltılamaz, depolanan veya bir sistemde saklanamaz ya da herhangi bir yolla (elektronik, mekanik, fotokopi, kaydı veya diğer) veya herhangi bir amaçla, Microsoft Corporation'ın izni hızlı yazılır.

Microsoft'un patentleri, patent başvuruları, ticari markaları, telif hakları veya diğer fikri mülkiyet hakları bu belgedeki konuyla ilgili olabilir. Microsoft'tan bir yazılı lisans anlaşmasında bu belgenin bulundurmak, herhangi bir lisans bu patentlere, ticari markaları, telif hakları veya diğer fikri mülkiyet sağlamazsa açıkça belirtilmediği sürece.

Aksi belirtilmediği sürece, örnek şirketler, kuruluşlar, ürünler, etki alanı adları, e-posta adresleri, logolar, kişiler, yerler ve olaylar burada olduğunu hayal ve tüm gerçek şirket, kuruluş, ürün, etki alanı adı, e-posta ile hiçbir ilişki adı geçen adresi, logo, kişi, yer veya olay amaçlanmamıştır veya çıkarılmamalıdır.

© 2010 Microsoft Corporation. Tüm hakları saklıdır.

Microsoft ve Windows tescilli veya Amerika Birleşik Devletleri ve/veya diğer ülkelerde Microsoft Corporation'ın ticari olan.

Burada sözü edilen gerçek şirketler ve ürünler ticari markalar kendi sahiplerinin olabilir.
