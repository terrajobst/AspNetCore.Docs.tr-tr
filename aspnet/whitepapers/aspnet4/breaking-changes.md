---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 önemli değişiklikler | Microsoft Docs
author: rick-anderson
description: Bu belge için .NET Framework sürümü kullanılarak oluşturulan uygulamaların olası etkileyebilir 4 yayın yapılan değişiklikleri açıklar...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 7eea51add6b05684357314e3d6aa5087383c6408
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-4-breaking-changes"></a>ASP.NET 4 önemli değişiklikler
====================
> Bu belge için .NET Framework sürümü büyük olasılıkla önceki sürümler, ASP.NET 4 Beta 1 ve Beta 2 sürümleri de dahil olmak üzere kullanılarak oluşturulan uygulamaları etkileyebilir 4 yayın yapılan değişiklikleri açıklar.
> 
> [Bu teknik indirin](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)


<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>İçindekiler

[Web.config dosyasındaki ControlRenderingCompatibilityVersion ayarı](#0.1__Toc256770141 "_Toc256770141")  
[ClientIDMode değişiklikleri](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode ve UrlEncode şimdi kodlamak tek tırnak işareti](#0.1__Toc256770143 "_Toc256770143")  
[ASP.NET sayfası (.aspx) ayrıştırıcı olan Stricter](#0.1__Toc256770144 "_Toc256770144")  
[Güncelleştirilmiş tarayıcı tanım dosyalarını](#0.1__Toc256770145 "_Toc256770145")  
[System.Web.Mobile.dll kaldırılan kök Web yapılandırma dosyasından](#0.1__Toc256770146 "_Toc256770146")  
[ASP.NET istek doğrulama](#0.1__Toc256770147 "_Toc256770147")  
[Varsayılan karma algoritması olan şimdi HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Yapılandırma hataları yeni ASP.NET 4 kök yapılandırmayla ilgili](#0.1__Toc256770149 "_Toc256770149")  
[ASP.NET 4 alt uygulamaları başına 3.5 uygulamaları ASP.NET 2.0 veya ASP.NET altında başarısız](#0.1__Toc256770150 "_Toc256770150")  
[Web sitelerini ASP.NET 4 başarısız SharePoint yüklü olduğu bilgisayarlarda başlatmak](#0.1__Toc256770151 "_Toc256770151")  
[HttpRequest.FilePath özelliği artık PATHINFO değerleri içeren](#0.1__Toc256770152 "_Toc256770152")  
[ASP.NET 2.0 uygulamaları eurl.axd başvuru HttpException hataları oluşturmak](#0.1__Toc256770153 "_Toc256770153")  
[Olay işleyicileri değil oluşmayabilir IIS 7 veya IIS 7.5 varsayılan belgedeki tümleşik modu](#0.1__Toc256770154 "_Toc256770154")  
[Değişiklikler için ASP.NET kodu erişim güvenliği (CAS) uygulama](#0.1__Toc256770155 "_Toc256770155")  
[MembershipUser ve diğer türleri System.Web.Security Namespace taşınmış](#0.1__Toc256770156 "_Toc256770156")  
[Çıkış önbelleğe alma farklılık değişiklik \* HTTP üstbilgisi](#0.1__Toc256770157 "_Toc256770157")  
[System.Web.Security Passport türleridir Kullanımdan kalktı](#0.1__Toc256770158 "_Toc256770158")  
[ASP.NET 4 görüntüdeki işlemek MenuItem.PopOutImageUrl özelliği başarısız](#0.1__Toc256770159 "_Toc256770159")  
[Menu.StaticPopOutImageUrl ve Menu.DynamicPopOutImageUrl yolları ters eğik çizgi içerdiğinde resimleri işlemek için başarısız](#0.1__Toc256770160 "_Toc256770160")  
[Disclaimer](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Web.config dosyasındaki ControlRenderingCompatibilityVersion ayarı

.NET Framework sürüm 4 ASP.NET denetimleri daha kesin olarak bunlar biçimlendirmesi nasıl oluşturmak belirtmenize izin vermek için değiştirilmiş. .NET Framework önceki sürümleri, bazı denetimleri devre dışı bırakmak için hiçbir yolu yoktu biçimlendirme yayılan. Varsayılan olarak, ASP.NET 4 biçimlendirme bu tür artık oluşturulur.

ASP.NET 2.0 veya ASP.NET 3.5 uygulamanızı yükseltmek için Visual Studio 2010 kullanıyorsanız, aracı için bir ayar otomatik olarak ekler. `Web.config` eski işleme korur dosya. Ancak, bir uygulama .NET Framework 4 hedeflemek için IIS uygulama havuzunda değiştirerek yükseltirseniz, ASP.NET varsayılan olarak yeni işleme modunu kullanır. Yeni işleme modunu devre dışı bırakmak için aşağıdaki ayarı ekleyin `Web.config` dosyası:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Yeni davranış getirir ana işleme değişiklikleri aşağıdaki gibidir:

- **Görüntü** ve **ImageButton** artık denetimlerini işlemeye bir `border="0"` özniteliği.
- **BaseValidator** bu sınıftan türetilen sınıf ve doğrulama denetimleri artık varsayılan olarak kırmızı metin oluşturma.
- **HtmlForm** denetimi oluşturmak olmayan bir **adı** özniteliği.
- **Tablo** denetim artık işler bir `border="0"` özniteliği.
- Kullanıcı girişi için tasarlanmamıştır denetimleri (örneğin, **etiket** denetimi) artık işleme `disabled="disabled"` varsa özniteliği kendi **etkin** özelliği ayarlanmış **yanlış**(veya bir kapsayıcı denetimden bunlar bu ayarı devralan varsa).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode değişiklikleri

**ClientIDMode** ayarı ASP.NET 4'te nasıl ASP.NET oluşturur belirtmenize olanak sağlar **kimliği** HTML öğeleri için öznitelik. ASP.NET önceki sürümlerde varsayılan davranışı eşdeğer **AutoID** ayarıyla **ClientIDMode**. Ancak, varsayılan ayar sunulmuştur **tahmin edilebilir**.

ASP.NET 2.0 veya ASP.NET 3.5 uygulamanızı yükseltmek için Visual Studio 2010 kullanıyorsanız, aracı için bir ayar otomatik olarak ekler. `Web.config` .NET Framework'ün önceki sürümlerinde davranışını korur dosya. Ancak, bir uygulama .NET Framework 4 hedeflemek için IIS uygulama havuzunda değiştirerek yükseltirseniz, ASP.NET yeni mod varsayılan olarak kullanır. Yeni istemci kimliği modu devre dışı bırakmak için aşağıdaki ayarı ekleyin `Web.config` dosyası:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode ve UrlEncode şimdi tek tırnak işareti kodlama

ASP.NET 4'te **HtmlEncode** ve **UrlEncode** yöntemlerinin **HttpUtility** ve **HttpServerUtility** sınıfları için güncelleştirildi tek tırnak işareti karakteri (') gibi kodla:

- **HtmlEncode** yöntem kodlar tek tırnak işareti örnekleri olarak.
- **UrlEncode** yöntem tek tırnak işareti örneklerini % 27 kodlar.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>ASP.NET sayfası (.aspx) ayrıştırıcı Stricter olduğu

ASP.NET sayfaları için sayfa ayrıştırıcısı (`.aspx` dosyaları) ve kullanıcı denetimleri (`.ascx` dosyaları) ASP.NET 4'te daha sıkı ve daha fazla geçersiz biçimlendirme örneklerini reddeder. Örneğin, aşağıdaki iki parçacıkları önceki sürümlerinde, ASP.NET başarıyla ayrıştırma, ancak şimdi ayrıştırıcı hataları ASP.NET 4'te yükseltirsiniz.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Geçersiz noktalı sonunda fark **HiddenField** etiketi.

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Kapatılmamış fark **stili** içine çalıştıran özniteliği **CssClass** özniteliği.

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Güncelleştirilmiş tarayıcı tanımı dosyaları

Tarayıcı tanım dosyalarını yeni ve güncelleştirilmiş tarayıcılar ve cihazlar hakkındaki bilgileri içerecek şekilde güncelleştirildi. Eski tarayıcılar ve cihazlar Netscape Gezgini gibi kaldırılır ve yeni tarayıcılar atanmış olması ve Google Chrome ve Apple iPhone aygıtların eklenir.

Uygulamanızı kaldırılmış olan tarayıcı tanımları birinden devral özel tarayıcı tanımları içeriyorsa, bir hata görürsünüz. Örneğin, varsa `App_Browsers` IE2 tarayıcı tanımından devralan bir tarayıcı tanımı içeren klasör, aşağıdaki yapılandırma hata iletisini alırsınız:

- 'IE2' kimlikli tarayıcı veya ağ geçidi öğe bulunamıyor.

> [!NOTE]
> **HttpBrowserCapabilities** nesne (sayfanın tarafından sunulan **Request.Browser** özelliği) tarayıcı tanımları dosyaları tarafından yönetilir. Bu nedenle, ASP.NET 4'te bu nesnenin bir özelliğini erişerek dönen bilgiyi ASP.NET daha önceki bir sürümünde döndürülen bilgiler farklı olabilir.


Eski tarayıcı tanım dosyalarını aşağıdaki klasörden tarayıcı tanım dosyalarını kopyalayarak döndürebilirsiniz:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Karşılık gelen dosyaları kopyalayın `\CONFIG\Browsers` ASP.NET 4 için klasör. Dosyaları kopyaladıktan sonra Aspnet çalıştırmak\_regbrowsers.exe komut satırı aracı.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>Kök Web yapılandırma dosyasından kaldırılmış System.Web.Mobile.dll

ASP.NET önceki sürümlerde kök dizininde System.Web.Mobile.dll derlemesine başvuru eklenmiştir `Web.config` dosyasını **derlemeleri** altında bölüm. Performansı artırmak için bu derlemesine başvuru kaldırıldı.

System.Web.Mobile.dll derleme ASP.NET 4'te bulunur, ancak kullanım dışıdır. System.Web.Mobile.dll derleme türlerinden kullanmak istiyorsanız, bu derlemesine başvuru ya da kök eklemek `Web.config` dosya veya bir uygulamaya `Web.config` dosya. (Kullanım dışı) ASP.NET Mobil denetimleri kullanmak istiyorsanız, örneğin, System.Web.Mobile.dll derlemesine başvuru eklemelisiniz `Web.config` dosya.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>ASP.NET istek doğrulama

ASP.NET istek doğrulama özelliği belirli bir düzeyde varsayılan siteler arası komut dosyası (XSS) saldırılara karşı koruma sağlar. ASP.NET önceki sürümlerde varsayılan olarak istek doğrulama etkinleştirildi. Ancak, yalnızca ASP.NET sayfaları için uygulanan (`.aspx` dosyaları ve bunların sınıfı dosyaları) ve yalnızca zaman bu sayfaları yürütülmekte.

Önce etkinleştirildiğinden ASP.NET 4'te, varsayılan olarak, istek doğrulamanın tüm istekler için etkin **BeginRequest** bir HTTP isteği aşaması. Sonuç olarak, istek doğrulama isteklerini yalnızca .aspx sayfa istekleri, tüm ASP.NET kaynaklar için geçerlidir. Bu Web hizmeti çağrıları ve özel HTTP işleyicileri gibi istekleri içerir. Özel HTTP modülleri bir HTTP isteğinin içeriğini okurken istek doğrulama de etkindir.

Sonuç olarak, istek doğrulama hataları için daha önce hataları tetiklemek değil istekleri artık ortaya çıkabilir. ASP.NET 2.0 istek doğrulama özelliği davranışa geri dönmek için aşağıdaki ayar Ekle `Web.config` dosyası:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Ancak, mevcut işleyicileri, modüller veya diğer özel kodu potansiyel olarak erişen olup olmadığını XSS olabilir güvenli olmayan HTTP girişleri vektörlerinin saldırı belirlemek için istek doğrulama hataları çözümleyin öneririz.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>Varsayılan karma algoritması HMACSHA256 sunulmuştur

ASP.NET forms kimlik doğrulaması tanımlama bilgileri gibi verilerin güvenliğini sağlamaya yardımcı ve görünüm durumunu için şifreleme ve karma algoritmaları kullanır. Varsayılan olarak, ASP.NET 4 şimdi HMACSHA256 algoritması tanımlama bilgileri ve görünüm durumu üzerinde karma işlemler için kullanır. ASP.NET önceki sürümlerini eski HMACSHA1 algoritma kullanılır.

Karma ASP.NET 2.0/ASP.NET 4 çalıştırırsanız, uygulamalarınızın etkilenebilir burada veri forms kimlik doğrulaması tanımlama bilgileri gibi gerekir çalışma across.NET Framework sürümlerini ortamları. Eski HMACSHA1 algoritmasını kullanmak için bir ASP.NET 4 Web uygulaması yapılandırmak için aşağıdaki ayarı ekleyin `Web.config` dosyası:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Yeni ASP.NET 4 kök yapılandırması ile ilgili yapılandırma hataları

Kök yapılandırma dosyaları ( `machine.config` dosya ve kök `Web.config` dosyası) için .NET Framework 4 (ve bu nedenle ASP.NET 4) ASP.NET 3.5 bulundu Demirbaş yapılandırma bilgilerini çoğunu içerecek şekilde güncelleştirildi Uygulama `Web.config` dosyaları. Yönetilen IIS 7.5 ve IIS 7 yapılandırma sistemler karmaşıklığı nedeniyle, ASP.NET 3.5 uygulamalarını IIS 7 ve IIS 7.5 altında ve ASP.NET 4 çalıştırmak ASP.NET veya IIS yapılandırma hataları neden olabilir.

ASP.NET 4 ASP.NET 3.5 uygulamalarını Visual Studio 2010'da, proje yükseltme araçları kullanarak mümkünse yükseltmenizi öneririz. Visual Studio 2010 ASP.NET 3.5 uygulamanın otomatik olarak değiştirir `Web.config` ASP.NET 4 için uygun ayarları içeren dosyaya.

Ancak, .NET Framework 4 gerekmeksizin kullanarak ASP.NET 3.5 uygulamalarını çalıştırmak için desteklenen bir senaryo değildir. Bu durumda, uygulamanın el ile değiştirmeniz gerekebilir `Web.config` .NET Framework 4 altında ve IIS 7 veya IIS 7.5 altında uygulamayı çalıştırmadan önce dosya.

Sonraki iki bölümde farklı yazılım birleşimler için yapmanız gerekebilecek değişiklikleri açıklar.

**Ne düzeltme KB958854 ne de SP2 yüklendiği Windows Vista SP1 veya Windows Server 2008 SP1.** Bu yapılandırmada, IIS 7 yapılandırma sistemi yanlış bir yönetilen uygulamayı uygulama düzeyi karşılaştırarak birleştirir `Web.config` ASP.NET 2.0 dosyasına `machine.config` dosyaları. Bu, uygulama düzeyi nedeniyle `Web.config` dosyaları .NET Framework 3.5 veya üzeri olmalıdır bir **system.web.extensions** bir IIS 7 doğrulama hatasına neden değil sırayla yapılandırma bölümü tanımı (öğesi).

Ancak, uygulama düzeyinde el ile değişiklik `Web.config` Visual Studio 2008 ile sunulan özgün Demirbaş yapılandırma bölümü tanımları tam olarak eşleşmiyor dosyası girdileri ASP.NET yapılandırma hataları neden olur. (Visual Studio 2008 tarafından oluşturulan varsayılan yapılandırma girdileri düzgün çalışmaz.) Sık karşılaşılan bir el ile değiştirilen sorundur `Web.config` dosyaları dışlamayı **allowDefinition** ve **requirePermission** çeşitli yapılandırma bölümünde bulunan configuration öznitelikleri tanımları. Bu uygulama düzeyi kısaltılmış yapılandırma bölümünde arasında uyuşmazlığa neden `Web.config` dosyaları ve ASP.NET 4 tam tanımında `machine.config` dosya. Sonuç olarak, çalışma zamanında, ASP.NET 4 yapılandırma sistemi bir yapılandırma hatası oluşturur.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2, and also Windows Vista SP1 and Windows Server 2008 SP1 where hotfix KB958854 is installed.**

Üzerinde bir metin karşılaştırma gerçekleştirdiğinden Bu senaryoda, IIS 7.5 ve IIS 7 yerel yapılandırma sistemi bir yapılandırma hatası döndürür. **türü** yönetilen yapılandırma bölümü işleyicisi için tanımlı öznitelik. Çünkü tüm `Web.config` Visual Studio 2008 ve Visual Studio 2008 SP1 tarafından oluşturulan dosyalarınız türü dizesinde "3.5" **system.web.extensions** (ve ilişkili) yapılandırma bölümü işleyicileri ve ASP.NET 4 `machine.config` dosyası "4.0" sahip **türü** yapılandırma doğrulama IIS 7'de Visual Studio 2008 veya Visual Studio 2008 SP1'i her zaman oluşturulan uygulamalar aynı yapılandırma bölümü işleyicileri başarısız özniteliğinin ve IIS 7.5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Bu sorunları çözme

Uygulama düzeyi ilk senaryo için geçici çözüm güncelleştirmektir `Web.config` Demirbaş yapılandırma metinden ekleyerek dosya bir `Web.config` Visual Studio 2008 tarafından otomatik olarak oluşturulan dosya.

İlk senaryo için alternatif bir geçici çözüm Vista veya Windows Server 2008 için Service Pack 2 bilgisayarınıza yüklemek için düzeltme KB958854 yüklemek için mi ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) yanlış yapılandırma birleştirme davranışını düzeltmek için IIS yapılandırma sistemi. Ancak, bu eylemlerden gerçekleştirdikten sonra uygulamanızı büyük olasılıkla bir yapılandırma hatası ikinci senaryo için açıklanan sorunu nedeniyle karşılaşır.

İkinci senaryo için geçici çözüm silin veya tüm açıklama kullanmaktır **system.web.extensions** grup yapılandırma bölümü tanımları ve yapılandırma bölümü, uygulama düzeyinde tanımları `Web.config` dosya. Bu tanımları genellikle uygulama düzeyi en üstünde olacak `Web.config` dosya ve tanımlanabilir **configSections** öğeyi ve alt öğelerini.

Her iki senaryo için de el ile silmeniz önerilir **system.codedom** bu gerekli olmamakla birlikte, bölüm.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>ASP.NET 4 alt uygulamaları başına ASP.NET 2.0 veya ASP.NET altında 3.5 uygulamalar başarısız

ASP.NET 4 ASP.NET önceki sürümlerini çalıştıran uygulamalar, alt yapılandırma veya derleme hataları nedeniyle başlatmak başarısız olabilir olarak yapılandırılmış olan uygulamaların. Aşağıdaki örnek, etkilenen bir uygulama için bir dizin yapısına gösterir.

`/parentwebapp` (ASP.NET 2.0 veya ASP.NET 3.5 kullanmak üzere yapılandırılmış)  
`/childwebapp` (ASP.NET 4 kullanmak üzere yapılandırılmış)

Uygulamada `childwebapp` klasörü IIS 7 veya IIS 7.5 başlatın ve rapor bir yapılandırma hatasıyla başarısız olur. Hata metni aşağıdakine benzer bir ileti içerir:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`
  

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

IIS 6, uygulamada `childwebapp` klasörü de başarısız olur başlatmak, ancak farklı bir hata bildirir. Örneğin, hata metni aşağıdaki durum:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Bu senaryolar için meydana üst uygulamada yapılandırma bilgilerini `parentwebapp` klasör alt web tarafından kullanılan son birleştirilmiş yapılandırma ayarlarını belirler yapılandırma bilgilerini hiyerarşisini bir parçasıdır Uygulama `childwebapp` klasör. ASP.NET 4 Web uygulamasını IIS 7 (veya IIS 7.5) veya IIS 6 kullanılıp kullanılmadığını bağlı olarak, IIS yapılandırma sistemini veya ASP.NET 4 derleme sistem, bir hata döndürecektir.

Bu sorunu çözmek için ve alt çalışmak için ASP.NET 4 uygulama almak için izlemeniz gereken adımlar olup ASP.NET 4 uygulama IIS 6 veya IIS 7 (veya IIS 7.5) çalıştığına göre değişir.

### <a name="step-1-iis-7-or-iis-75-only"></a>Adım 1 (IIS 7 veya IIS 7.5 yalnızca)

Bu adım, yalnızca IIS 7 çalıştıran işletim sistemleri veya Windows Vista, Windows Server 2008, Windows 7 ve Windows Server 2008 R2 içeren IIS 7.5 gereklidir.

Taşıma **configSections** tanımında `Web.config` üst uygulama (ASP.NET 2.0 veya ASP.NET 3.5 çalıştıran) dosyasının kök içine `Web.config` .NET Framework 2.0 için dosya. IIS 7 ve IIS 7.5 yerel yapılandırma sistemi tarar **configSections** yapılandırma dosyalarını hiyerarşisini birleştirir zaman öğesi. Taşıma **configSections** üst Web uygulaması'nın tanımından `Web.config` kök dosyaya `Web.config` dosya etkili bir şekilde alt ASP.NET 4 oluşur yapılandırma birleştirme işlemi öğeyi gizler uygulama.

Bir 32 bit işletim sisteminde veya 32 bit uygulama havuzları, kök `Web.config` for ASP.NET 2.0 ve ASP.NET 3.5 dosya normal olarak aşağıdaki klasörde bulunur:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

Bir 64-bit işletim sisteminde veya 64-bit uygulama havuzları, kök `Web.config` for ASP.NET 2.0 ve ASP.NET 3.5 dosya normal olarak aşağıdaki klasörde bulunur:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Bir 64-bit bilgisayarda 32 bit ve 64-bit Web uygulamaları çalıştırırsanız, taşımanız **configSections** kök yukarı öğesine `Web.config` 32 bit ve 64 bit sistemler için dosyaları.

Yerleştirdiğinizde **configSections** kök öğesinde `Web.config` dosya, bölüm yapıştırın hemen sonra **yapılandırma** öğesi. Aşağıdaki örnek, hangi üst kısmını kök gösterir `Web.config` dosya zaman öğeleri taşıma işlemini tamamladıktan gibi görünmelidir.

> [!NOTE]
> Aşağıdaki örnekte, satırları okunabilirlik için sarmalanmış.


[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Adım 2 (IIS tüm sürümleri)

Bu adım, ASP.NET 4 alt Web uygulamasını IIS 7 (veya IIS 7.5) veya IIS 6 üzerinde çalışıp çalışmadığını gereklidir.

İçinde `Web.config` ASP.NET 2 veya ASP.NET 3.5, çalışan Web uygulamasını üst dosyası ekleme bir **konumu** (IIS ve ASP.NET yapılandırma sistemi için) açıkça belirten etiketi, yalnızca yapılandırma girdileri üst Web uygulaması için geçerlidir. Aşağıdaki örnek söz dizimi görülmektedir **konumu** öğesi eklemek için:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

Aşağıdaki örnekte gösterildiği nasıl **konumu** etiketi ile başlayan tüm yapılandırma bölümlerinin sarmalamak için kullanılan **appSettings** bölümünde ve ile biten **system.webServer**bölümü.

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

1. ve 2. adımları tamamlandığında, alt ASP.NET 4 Web uygulamalarını hatasız başlar.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 Web siteleri, SharePoint yüklü olduğu bilgisayarlarda başlatılamadı

SharePoint çalıştıran web sunucuları bir `Web.config` bir SharePoint Web sitesinin kök dizininde dağıtılmış dosya (örneğin, `c:\inetpub\wwwroot\web.config` varsayılan Web sitesi için). Bu `Web.config` dosyasını, SharePoint ayarlar özel bir kısmi güven düzeyi WSS adlı\_en az.

SharePoint Web sitesinin bu tür alt olarak dağıtılan ASP.NET 4 Web sitesi çalıştırmayı denerseniz, aşağıdaki hatayı görürsünüz:

`Could not find permission set named 'ASP.NET'.`

ASP.NET 4 kod erişim güvenliği (CAS) altyapısını ASP.NET adlı bir izin kümesi için görüneceğinden bu hata oluşur. Ancak, kısmi güven WSS tarafından başvurulan yapılandırma dosyası\_en az bu ada sahip bir izin kümesi içermiyor.

Şu anda yok ASP.NET ile uyumlu olan SharePoint sürümü. Sonuç olarak, ASP.NET 4 Web sitesi alt sitesi altında SharePoint Web sitelerini çalıştırmak çalışmamalısınız.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>HttpRequest.FilePath özelliği artık PATHINFO değerlerini içerir

ASP.NET dahil önceki sürümlerini bir **PATHINFO** dahil olmak üzere çeşitli dosya yolu ile ilgili özelliklerinden, döndürülen değer değerinde **HttpRequest.FilePath**,  **HttpRequest.AppRelativeCurrentExecutionFilePath**, ve **HttpRequest.CurrentExecutionFilePath**. ASP.NET 4 artık içerir **PATHINFO** bu özellikleri dönüş değerleri değeri. Bunun yerine, **PATHINFO** bilgi sağlanmıştır **HttpRequest.PathInfo**. Örneğin, aşağıdaki URL parçası düşünün:

`/testapp/Action.mvc/SomeAction`

ASP.NET, önceki sürümlerinde **HttpRequest** özellikler aşağıdaki değerlere sahiptir:

**HttpRequest.FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest.PathInfo**: (boş)

ASP.NET 4'te **HttpRequest** özellikleri, bunun yerine aşağıdaki değerlere sahiptir:

**HttpRequest.FilePath**: `/testapp/Action.mvc`

**HttpRequest.PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>ASP.NET 2.0 uygulamaları eurl.axd başvuru HttpException hataları oluşturmak

ASP.NET 4 IIS 6'etkinleştirildikten sonra IIS 6 (', Windows Server 2003 veya Windows Server 2003 R2) üzerinde çalışan ASP.NET 2.0 uygulamaları aşağıdaki gibi hatalara neden:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

ASP.NET Web sitesi ASP.NET 4 kullanmak için yapılandırıldığını algıladığında, yerel bir bileşeni olan ASP.NET 4 uzantısız URL ASP.NET yönetilen bölümüne daha ayrıntılı işleme için geçirir bu hata oluşur. Ancak, ASP.NET 4 Web sitesi altında olan sanal dizinleri ASP.NET 2.0 kullanmak üzere yapılandırıldıysa, bu "eurl.axd" dizesini içeren değiştirilmiş bir URL yolu sonuçlarında uzantısız URL işleniyor. Değiştirilen bu URL ASP.NET 2.0 uygulamaya gönderilir. ASP.NET 2.0 "eurl.axd" biçimini algılayamıyor. Bu nedenle, ASP.NET 2.0 adlı bir dosya bulmaya çalışır `eurl.axd` ve yürütebilirsiniz. Bu tür bir dosyanın var olduğundan istek başarısız olur bir **HttpException** özel durum.

Aşağıdaki seçeneklerden birini kullanarak bu sorunun geçici çözümü.

### <a name="option-1"></a>seçenek 1

ASP.NET 4 Web sitesini çalıştırmak için gerekli değilse, ASP.NET 2.0 kullanmayı site yeniden eşleyin.

### <a name="option-2"></a>Seçenek 2

ASP.NET 4 Web sitesini çalıştırmak için gerekli ise, tüm alt ASP.NET 2.0 sanal dizinleri ASP.NET 2.0 eşlenen başka bir Web sitesine taşıyın.

### <a name="option-3"></a>Seçenek 3

ASP.NET 2.0 Web sitesine yeniden eşlemek için veya bir sanal dizin konumunu değiştirmek için pratik değilse, ASP.NET 4'te işleme uzantısız URL açıkça devre dışı bırakın. Aşağıdaki yordamı kullanın:

1. Windows Kayıt Defteri'nde aşağıdaki düğüm açın:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Yeni bir **DWORD** adlı değeri **EnableExtensionlessUrls**.
2. Ayarlama **EnableExtensionlessUrls** 0. Bu uzantısız URL davranışı devre dışı bırakır.
3. Kayıt defteri değerini kaydedin ve Kayıt Defteri Düzenleyicisi'ni kapatın.
4. Çalıştırma **iisreset** yeni kayıt defteri değerini okumaya IIS neden olan komut satırı aracı.

> [!NOTE]
> Ayarı **EnableExtensionlessUrls** 1 uzantısız URL davranışı etkinleştirir. Bu değer belirtilmezse, varsayılan ayardır.


<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Olay işleyicileri değil oluşmayabilir IIS 7 veya IIS 7.5 varsayılan belgedeki tümleşik modu

ASP.NET 4 içerir değiştirmek değişiklikleri nasıl **eylem** HTML öznitelik **form** öğesi uzantısız URL için varsayılan bir belge çözdüğünde işlenir. Bir varsayılan belge çözme uzantısız URL bir örneği olacaktır [ http://contoso.com/ ](http://contoso.com/), sonuçta ortaya çıkan bir istekte [ http://contoso.com/Default.aspx ](http://contoso.com/Default.aspx).

ASP.NET 4 şimdi HTML işler **form** öğenin **eylem** eşlenmiş bir varsayılan belge sahip uzantısız bir URL'ye bir istekte bulunulduğunda olarak boş bir dize öznitelik değeri. Örneğin, ASP.NET, bir istek önceki sürümlerinde de [ http://contoso.com ](http://contoso.com) bir isteği ile sonuçlandı `Default.aspx`. Bu belgede, açılış **form** etiketi aşağıdaki örnekte olduğu gibi çizilir:

`<form action="Default.aspx" />`

ASP.NET 4, bir istek [ http://contoso.com ](http://contoso.com) ayrıca sonuçları bir istekte `Default.aspx`. Ancak, ASP.NET HTML açılış şimdi işler **form** aşağıdaki örnekteki gibi etiketi:

`<form action="" />`

Bu fark nasıl **eylem** özniteliği işlenir ince değişiklikler nasıl form post IIS ve ASP.NET tarafından işlenen de neden olabilir. Zaman **eylem** özniteliktir IIS boş bir dize **DefaultDocumentModule** nesnesi, bir alt istek oluşturacak `Default.aspx`. Çoğu koşullar altında bu alt istek uygulama kodu için saydamdır ve `Default.aspx` sayfa normal şekilde çalışır.

Ancak, yönetilen kod ve IIS 7 veya IIS 7.5 Tümleşik modu arasındaki olası bir etkileşim yönetilen .aspx sayfalarının düzgün alt istek sırasında durmasına neden olabilir. Aşağıdaki durumlar ortaya varsa, alt isteğine bir `Default.aspx` belge bir hata veya beklenmeyen davranışlara neden olur:

1. Tarayıcı ile .aspx sayfası gönderilmesini **form** öğenin **eylem** özniteliği ayarlamak "".
2. Form, ASP.NET ile nakledilir.
3. Yönetilen bir HTTP modülü varlık gövdesini kısmı okur. Örneğin, bir modül okur **Request.Form** veya **Request.Params**. Bu yönetilen belleğe okumak için POST isteğinin varlık gövdesini neden olur. Sonuç olarak, varlık gövdesini artık IIS 7 veya IIS 7.5 Tümleşik modunda çalışan tüm yerel kod modülleri kullanılabilir.
4. IIS **DefaultDocumentModule** nesne sonunda çalıştırır ve bir alt istek oluşturur `Default.aspx` belge. Bununla birlikte, varlık gövdesini yönetilen kodun bir parçası okumak için kullanılabilir varlık gövdesini alt isteği göndermek kullanılabilir yoktur.
5. HTTP ardışık düzen çalıştığında alt istek işleyicisi için `.aspx` dosyaları handler-execute aşamasında çalıştırır.
6. Hiçbir varlık gövdesini olduğundan, hiçbir form değişkeni ve görünüm durumu vardır ve bu nedenle hiçbir bilgi .aspx sayfası işleyicisi (varsa) hangi olay oluşturulması gerektiği belirlemek kullanılabilir. Bunun sonucunda, etkilenen .aspx sayfası için geri gönderme olay işleyicileri hiçbiri çalıştırın.

Aşağıdaki yollarla bu davranışı geçici çözüm bulabilirsiniz:

- İsteğin varlık gövdesine sırasında varsayılan belge istekler erişiyor HTTP modülü tanımlamak ve yalnızca yönetilen istekleri için çalıştırmak için yapılandırılabilir olup olmadığını belirler. IIS 7 ve IIS 7.5 için tümleşik modda, yalnızca yönetilen istekleri için modülün aşağıdaki öznitelik ekleyerek çalıştırmak için HTTP modülleri işaretlenebilir **system.webServer/modules** girişi:

- `precondition="managedHandler"`

- IIS 7 ve IIS 7.5 değil olacak şekilde belirlemek için modülü istek bu ayarı devre dışı bırakır istekleri yönetilen. Varsayılan Belge istekler için ilk isteği için uzantısız bir URL'dir. Bu nedenle, IIS işaretlenmiş tüm yönetilen modülleri yönetilen işleyici önkoşulu ile ilk isteği işleme sırasında çalışmaz. Sonuç olarak, yönetilen modülleri yanlışlıkla varlık gövdesini okuyacaksa değil ve bu nedenle varlık gövdesini hala kullanılabilir olduğundan ve alt istek ve varsayılan belge boyunca sonuna geçirilir.

- Sorunlu HTTP modülleri için tüm istekleri çalıştırmasını varsa (çözümlemek uzantısız URL'ler için statik dosyaların **DefaultDocumentModule** nesne yönetilen istekleri, vb. için), etkilenen .aspx sayfaları tarafından açıkça değiştirme ayarı **eylem** sayfanın özelliğinin **System.Web.UI.HtmlControls.HtmlForm** denetlemek için boş olmayan bir dize. Örneğin, varsayılan belge ise `Default.aspx`, açıkça ayarlamak için sayfanın kodunu değiştirme **HtmlForm** denetimin **eylem** "Default.aspx" özelliğine.

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>ASP.NET kodu erişim güvenliği (CAS) uygulama değişiklikleri

ASP.NET 2.0 ve uzantısı tarafından 3. 5 ', eklenen ASP.NET özellikleri kullanmak .NET Framework 1.1 ve 2.0 kod erişim güvenliği (CAS) modeli. Ancak, CA'ların ASP.NET 4'te uygulama önemli ölçüde overhauled olmuştur. Sonuç olarak, genel derleme önbelleğinde (GAC) çalıştıran güvenilen kod Bel kısmi güven ASP.NET uygulamaları çeşitli güvenlik durumlarla başarısız olabilir. CA ilke makine kapsamlı değişikliklerin kullanan kısmi güven uygulamaları, ayrıca güvenlik durumlarla başarısız olabilir.

ASP.NET 1.1 ve 2.0 kullanarak yeni davranışı kısmi güven ASP.NET 4 uygulamaları döndürebilirsiniz **legacyCasModel** özniteliğini **güven** aşağıdaki örnekte gösterildiği gibi yapılandırma öğesi :

`<trust level= "Medium" legacyCasModel="true" />`

Eski CA modele geri döndürdüğünüzde, aşağıdaki eski CA davranışları etkinleştirilir:

- Makine CA'ları ilkesi uygulanır.
- Tek bir uygulama etki alanında birden çok farklı izin kümeleri izin verilir.
- Açık izne onaylar yalnızca ASP.NET veya başka bir .NET Framework kod yığında olduğunda çağrılan derlemeleri GAC'de için gerekli değildir.

.NET Framework 4'te bir senaryo döndürülemiyor: Web olmayan kısmi güven uygulamaları artık System.Web.dll ve System.Web.Extensions.dll belirli API'larını çağırma. .NET Framework önceki sürümlerde açıkça verilebilmesi, Web olmayan kısmi güven uygulamaları için olası <strong>AspNetHostingPermission</strong> izinleri. Bu uygulamalar sonra kullanabilir <strong>System.Web.HttpUtility</strong>, türlerini <strong>System.Web.ClientServices.\< / strong > * ad alanları ve türler ilgili üyelik, roller ve profilleri. Bu tür Web olmayan kısmi güven uygulamalardan çağrılırken, .NET Framework 4'te artık desteklenmiyor.

> [!NOTE]
> **HtmlEncode** ve **HtmlDecode** işlevselliğini **System.Web.HttpUtility** sınıfı, yeni .NET Framework 4'e taşındı  **System.Net.WebUtility** sınıfı. Kullanılan yalnızca ASP.NET işlevselliği olduysa, uygulamanın kodunu yeni değiştirme **WebUtility** yerine sınıfı.


ASP.NET 4'te varsayılan CA uygulama değişiklikleri üst düzey bir özeti verilmiştir:

- ASP.NET uygulama etki alanları artık homojen uygulama etki alanları bulunur. Yalnızca kısmi güven ve tam güven verme kümeleri bir uygulama etki alanında kullanılabilir.
- ASP.NET kısmi güven verme ayarlar, tüm kuruluş düzeyi, makine düzeyinde veya kullanıcı düzeyi CA'lar ilkesinden bağımsızdır.
- .NET Framework 4 saydamlık modelini kullanmak için 3.5 ve 3.5 SP1 ile birlikte gelen ASP.NET derlemeleri dönüştürüldü.
- ASP.NET kullanımını **AspNetHostingPermission** özniteliği önemli ölçüde küçültülen. Bu öznitelik çoğu örneklerini ortak ASP.NET API'lerden kaldırılmıştır.
- ASP.NET derleme sağlayıcıları tarafından oluşturulan dinamik olarak derlenmiş derlemeleri açıkça derlemeleri saydam olarak işaretlemek için güncelleştirilmiştir.
- Tüm ASP.NET derlemeler şimdi APTCA özniteliği yalnızca barındırma ortamlarında Web uygulanır şekilde işaretlenir. Kısmen güvenilir olmayan Web barındırma ortamları ClickOnce gibi ASP.NET derlemelerine arayabilmesi için olmaz.

Yeni ASP.NET 4 kod erişim güvenlik modeli hakkında daha fazla bilgi için bkz: [kullanarak kod erişim güvenliği ASP.NET uygulamalarında](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) MSDN Web sitesinde.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>MembershipUser ve diğer türleri System.Web.Security Namespace taşındı

ASP.NET üyelik kullanılan bazı türleri gelen taşınmış `System.Web.dll` yeni System.Web.ApplicationServices.dll derleme. Türleri türleri istemci ve genişletilmiş .NET Framework SKU'ları arasındaki mimari katmanlama bağımlılıkları çözümlemek amacıyla taşındı.

System.Web.ApplicationServices.dll ASP.NET derleme sistem tarafından varsayılan olarak kullanılan başvurulan derlemelerin listesine eklenmiş web sitesi projeleri bu tür taşıma sonucunda sorunlara sahip değilsiniz. Visual Studio 2010'da açarak ASP.NET için ASP.NET 4 önceki bir sürümü kullanılarak oluşturulmuş bir Web sitesi projesini yükseltirseniz, projeyi hatasız derleyin.

Benzer şekilde, Visual Studio 2010'da açarak ASP.NET için ASP.NET 4'ün önceki bir sürümde oluşturulmuş bir Web uygulaması projesi yükseltirseniz, yükseltme işlemi System.Web.ApplicationServices.dll başvuru projeye ekler. Bu nedenle, Web Uygulama projeleri de hatasız derlenir yükseltilmiştir.

Üyelik türleri için farklı bir derleme taşındı olsa da ASP.NET önceki sürümleri kullanılarak oluşturulan derlenmiş (ikili) dosyalar hatasız ASP.NET 4'üzerinde de çalıştırabilirsiniz. Tür bilgileri iletme ASP.NET 4 sürümüne eklenmiştir `System.Web.dll` , otomatik olarak bu tür için çalışma zamanı başvuruları türleri için yeni bir konuma yönlendirir.

Ancak, belirli üyelik türleri kullanan ve ASP.NET önceki sürümlerinden yükseltme sınıf kitaplıkları, ASP.NET 4 projesinde kullanıldığında derlemek başarısız olur. Örneğin, bir sınıf kitaplığı projesi derlemek ve aşağıdaki gibi bir hata raporu başarısız olabilir:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
  

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Sınıf kitaplığı projenizin System.Web.ApplicationServices.dll için bir başvuru ekleyerek bu soruna geçici çözüm bulabilirsiniz.

Aşağıdaki liste gösterir *System.Web.Security* gelen taşındı türleri `System.Web.dll` System.Web.ApplicationServices.dll için:

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

## <a name="output-caching-changes-to-vary--http-header"></a>Çıkış önbelleğe alma farklılık değişiklik \* HTTP üstbilgisi

ASP.NET 1. 0'da, bir hata nedeniyle belirtilen sayfaları önbelleğe `Location="ServerAndClient"` yaymak üzere bir çıktı – önbellek ayarı olarak bir `Vary:*` HTTP üstbilgisi yanıt. Hiçbir zaman sayfanın yerel olarak önbelleğe almak için istemci tarayıcıları söyleyen etkisini vardı.

ASP.NET 1.1 içinde **System.Web.HttpCachePolicy.SetOmitVaryStar** yöntemi eklendi, hangi gizlemek için çağırabilirsiniz `Vary:*` üstbilgi. Bu yöntem verilmiş HTTP üstbilgisi değiştirme olası yeni olarak kabul edildiği için seçildi zaman değişikliği. Ancak, geliştiricilerin kafası ASP.NET davranış tarafından ve hata raporları önerir geliştiriciler var olan farkında **SetOmitVaryStar** davranışı.

ASP.NET 4'te, kök sorunu gidermek için kararı yapıldı. `Vary:*` HTTP üstbilgisi aşağıdaki yönergesini belirtmek yanıtlarının artık yayılan:

`<%@OutputCache Location="ServerAndClient" %>`

Sonuç olarak, **SetOmitVaryStar** gösterilmemesi için artık gerekli `Vary:*` üstbilgi.

Belirtin uygulamalarda `Location="ServerAndClient"` içinde **@ OutputCache** yönerge sayfasında, şimdi adına göre kapsanan davranışı göreceksiniz **konumu** olan özniteliğin değeri –, sayfalar olacak alınabilir, arama gerektirmeden tarayıcıda **SetOmitVaryStar** yöntemi.

Uygulamanızdaki sayfalar yayma gerekir, `Vary:*`, çağrı **AppendHeader** aşağıdaki örnekteki gibi yöntemi:

`HttpResponse.AppendHeader("Vary","*");`

Alternatif olarak, çıktı önbelleği değerini değiştirebilirsiniz **konumu** özniteliği "Server".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>Kullanımdan kalktı Passport System.Web.Security türleri:

ASP.NET 2.0 yerleşik Passport desteği Passport (şimdi LiveId) değişiklikleri nedeniyle birkaç yıl için artık kullanılmayan ve desteklenmeyen olmuştur. Sonuç olarak, beş türden ile ilgili Passport **System.Web.Security** şimdi işaretlenir **ObsoleteAttribute** özniteliği.

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>ASP.NET 4 görüntüdeki işlemek MenuItem.PopOutImageUrl özelliği başarısız

ASP.NET 3.5 *MenuItem.PopOutImageUrl* özelliği menü öğesi dinamik alt olduğunu belirtmek için bir menü öğesi görüntülenen görüntünün URL'sini belirtmenize olanak sağlar. Aşağıdaki örnek, ASP.NET 3.5 biçimlendirmede bu özelliği belirtmeniz gösterilmektedir.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

ASP.NET 4'te bir tasarım değişiklik sonucunda hiçbir çıktı için işlenen *PopOutImageUrl* özelliği için ayarlanmışsa *MenuItem* sınıfı. Bunun yerine, doğrudan bir resim URL'si belirtmeniz gerekir *menü* kullanarak denetim *StaticPopOutImageUrl* özelliği veya *DynamicPopOutImageUrl* özelliği. Statik bir menü ile çalışırken *Menu.StaticPopOutImageUrl* özelliği, aşağıdaki örnekte gösterildiği gibi statik menü öğesi bir alt olduğunu belirtmek için görüntülenen görüntünün URL'sini belirtir:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Dinamik menü ile çalışıyorsanız, kullandığınız *Menu.DynamicPopOutImageUrl* özelliği dinamik menü öğesi bir alt olduğunu gösteren görüntü URL'sini belirtin. Aşağıdaki örnek, önceki benzer ancak nasıl ayarlanacağını gösterir *DynamicPopOutImageUrl* Dinamik menü özelliği.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Varsa *Menu.DynamicPopOutImageUrl* özelliği ayarlanmamış ve *Menu.DynamicEnableDefaultPopOutImage* özelliği ayarlanmış *yanlış*, hiçbir resim görüntülenir. Benzer şekilde, varsa *StaticPopOutImageUrl* özelliği ayarlanmamış ve *StaticEnableDefaultPopOutImage* özelliği ayarlanmış *yanlış*, hiçbir resim görüntülenir.

Bu özellikler için yollar ayarladığınızda, eğik çizgi (/) yerine bir ters eğik çizgi kullanın (\). Daha fazla bilgi için bkz: [Menu.StaticPopOutImageUrl ve işleme görüntüleri zaman yollarını içeren ters eğik çizgi Menu.DynamicPopOutImageUrl başarısız](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu.StaticPopOutImageUrl_and_Menu.") başka bir yerde bu belgede.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Yolları ters eğik çizgi içerdiğinde resimleri işlemek Menu.StaticPopOutImageUrl ve Menu.DynamicPopOutImageUrl başarısız

ASP.NET 4, kullanarak belirttiğiniz görüntüleri *Menu.StaticPopOutImageUrl* ve *Menu.DynamicPopOutImageUrl* özellikleri değil sokacak yol backlashes içeriyorsa (\). Bu, ASP.NET önceki sürümlerini farklıdır.

Aşağıdaki örnekte *menü* denetim biçimlendirme gösterir *StaticPopOutImageUrl* özelliği bir ters eğik çizgi içeren bir yolu kullanarak ayarlayın. ASP.NET 4'te özelliğinde belirtilen görüntü işlenmez.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Bu sorunu gidermek için belirtilen yol değerlerini değiştirmek *StaticPopOutImageUrl* ve *DynamicPopOutImageUrl* özellikleri eğik çizgi (/) kullanın. Aşağıdaki örnek, bu değişiklik gösterir:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

ASP.NET için ASP.NET 4 önceki sürümlerinden geçirilen uygulamaları da içerebileceğini unutmayın etkilenmez, çünkü *MenuItem.PopOutImageUrl* özelliği değiştirildi. Daha fazla bilgi için bkz: [MenuItem.PopOutImageUrl ASP.NET 4'te bir görüntü oluşturmak için özellik başarısız](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem.PopOutImageUrl_Propert") bu belgedeki başka bir yerde.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Sorumluluk reddi

Bu ön belge ve burada açıklanan yazılım son ticari sürümünden önce önemli ölçüde değişebilir.

Bu belgede yer alan bilgileri yayımlandığı tarih itibariyle açıklanan sorunları, Microsoft Corporation'ın geçerli görünümde temsil eder. Microsoft değişen piyasa koşullarına yanıt vermesi gerektiğinden Microsoft tarafında taahhüdü olarak yorumlanmamalıdır ve Microsoft belgenin yayımlanma tarihinden sonra sunulan bilgilerin doğruluğu garanti edemez.

Bu teknik incelemede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR AÇIK, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ.

İlgili tüm telif hakkı yasalarına uymak kullanıcının sorumluluğundadır. Telif Hakkı altındaki sınırlamaksızın, bu belgenin hiçbir bölümü çoğaltılamaz, depolanan ya da bir geri alma sisteminde sunulan ya da herhangi bir yöntem (elektronik, mekanik, fotokopi, kayıt veya diğer) veya herhangi bir amaçla aktarılan, Microsoft Corporation'ın izni hızlı yazılır.

Microsoft, patentler, patent uygulamaları, ticari markalar, telif hakkı veya bu belgedeki konuyla ilgili diğer fikri mülkiyet hakları sahip olabilir. Microsoft'tan herhangi yazılı Lisans sözleşmesindeki, bu belgeyi bulundurmak, herhangi bir lisans bu patentler, ticari markaları, telif hakkı veya diğer fikri mülkiyet hakkı sağlamaz açıkça belirtilmediği sürece.

Aksi belirtilmediği sürece, örnek şirketler, kuruluşlar, ürünler, etki alanı adları, e-posta adresleri, logolar, kişiler, yerler ve olaylar burada olan kurgusal ve tüm gerçek şirket, kuruluş, ürün, etki alanı adı, e-posta ile ilişki gösterilen adresi, logo, kişi, yer veya olay amaçlanmamıştır veya çıkarılmamalıdır.

© 2010 Microsoft Corporation. Tüm hakları saklıdır.

Microsoft ve Windows kayıtlı ticari markaları ya da Microsoft Corporation'ın ABD'de ve/veya diğer ülkelerdeki ticari markalarıdır.

Burada sözü edilen gerçek şirketler ve ürünler adları, ticari markalar ilgili sahiplerinin olabilir.
