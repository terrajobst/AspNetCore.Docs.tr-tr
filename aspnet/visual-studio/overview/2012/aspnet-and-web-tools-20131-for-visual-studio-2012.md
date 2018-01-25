---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: "ASP.NET ve Web Araçları 2013.1 için Visual Studio 2012 için sürüm notları | Microsoft Docs"
author: microsoft
description: "Bu belgede, ASP.NET ve Web Araçları 2013.1 sürümü için Visual Studio 2012 açıklanmaktadır."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/13/2013
ms.topic: article
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: c11e2ef9c33b0cae1f196690533094ce1c342da5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>ASP.NET ve Web Araçları 2013.1 için Visual Studio 2012 için sürüm notları
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu belgede, ASP.NET ve Web Araçları 2013.1 sürümü için Visual Studio 2012 açıklanmaktadır.


## <a name="contents"></a>İçindekiler

- [Yükleme notları](#install)
- [Yazılım gereksinimleri](#requirements)
- ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için yeni özellikler

    - [Önyükleme](#bootstrap)
    - [Şablonlar](#templates)

        - [ASP.NET MVC 5 şablonu](#mvc5template)
        - [ASP.NET Web API 2 şablonu](#apitemplate)
        - [Öğe şablonları](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.NET Scaffolding](#scaffold)
    - [Razor Düzenleyicisi](#razor)
    - [NuGet 2.7](#nuget)
- Bilinen sorunlar ve yeni değişiklikler

    - [ASP.NET Scaffolding](#issuescaffolding)

        - [MVC ve Web API yapı İskelesi - HTTP 404 bulunamadı hatası](#404issue)
        - [Visual Studio Express 2012 için Web iskele kurulmuş öğe sonra çalışmayı durdurur.](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [Gözat ile ya da F5 cshtml dosyasını görüntüleyerek bir sunucu hatasına neden oluyor](#browseissue)
        - [URL yeniden yazma ve Tilde(~)](#rewriteissue)
    - [Şablonlar](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Yükleme notları

[Yükleme](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET ve Web Araçları Visual Studio 2012 için 2013.1.

<a id="requirements"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

Visual Studio 2012 veya Visual Studio Express 2012 için Web olması gerekir.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için yeni özellikler

<a id="bootstrap"></a>
### <a name="bootstrap"></a>önyükleme

MVC 5 denetleyicileri ve görünümler iskele kurduğunuzda görünümleri için biçimlendirme kullanan [önyükleme](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Şablonlar

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>ASP.NET MVC 5 şablonu

Yeni bir MVC 5 şablonu eklediğimiz. Son MVC 5 NuGet paketlerini başvurur ve yapı iskelesi denetleyicileri ve görünümleri eklemek için kullanabilirsiniz.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>ASP.NET Web API 2 şablonu

Yeni bir Web API 2 şablonu eklediğimiz. En son Web API 2 NuGet paketleri başvurur ve yapı iskelesi denetleyicileri ve görünümleri eklemek için kullanabilirsiniz.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Öğe şablonları

MVC 5 görünümleri, Web sayfaları (Razor 3) ve Web API 2 denetleyicisi için yeni öğe şablonları eklediğimiz. Bunlar ilgili NuGet paketlerini projeye yeni öğeleri eklenirken yükler.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework kullanarak bir MVC veya Web API denetleyicisi iskele kurduğunuzda Framework 6 kullanırız. Entity Framework hakkında daha fazla bilgi için bkz: [Entity Framework sürüm geçmişi](https://msdn.com/data/jj574253).

Ayrıca, indirin ve Visual Studio 2012 için Entity Framework 6 araçlarını yükleyin. Bkz: [alma Entity Framework](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

ASP.NET İskele bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir. Bir veri modeli ile etkileşime giren projeniz Demirbaş kod eklemek kolaylaştırır.

Visual Studio'nun önceki sürümleri yapı iskelesi ASP.NET MVC projelerine sınırlı. Bu güncelleştirmeyle, artık Web Forms dahil olmak üzere tüm ASP.NET projesi için yapı iskelesi kullanabilirsiniz. Bu güncelleştirme için bir Web Forms projesi oluşturma sayfaları desteklemez, ancak yapı iskelesi ile Web Forms projeye MVC bağımlılıkları ekleyerek kullanmaya devam edebilirsiniz. İçin Web formları sayfaları oluşturmaya yönelik destek gelecek bir güncelleştirmede eklenir.

Yapı iskelesi kullanırken, tüm gerekli bağımlılıkların projede yüklü olduğundan emin olun. Bir ASP.NET Web Forms projeyle başlatın ve sonra bir Web API denetleyicisi eklemek için yapı iskelesi kullanın, örneğin, gerekli NuGet paket ve başvuruları projenize otomatik olarak eklenir.

Bir Web Forms projeye MVC yapı iskelesi eklemek için Ekle bir **yeni iskele kurulmuş öğe** seçip **MVC 5 bağımlılıkları** iletişim penceresinde. MVC iskele için iki seçenek vardır; Minimal ve tam. En az seçerseniz, yalnızca NuGet paketlerini ve ASP.NET MVC için başvuruları projenize eklenir. Tam seçeneğini belirlerseniz MVC projesinde gerekli içerik dosyaların yanı sıra en az bağımlılıkları eklenir.

Zaman uyumsuz denetleyicileri iskele desteği yeni Entity Framework 6 zaman uyumsuz özelliklerini kullanır.

Daha fazla bilgi ve öğreticiler için bkz: [ASP.NET yapı İskelesi genel bakış](../2013/aspnet-scaffolding-overview.md). Bu öğreticiler yapı iskelesi ile Visual Studio 2013 gösterir, ancak ayrıca ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için geçerli olan.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor Düzenleyicisi

Bu güncelleştirmeyle, Visual Studio 2012 şimdi Razor 3 araçları/düzenlemeyi destekler.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 yakından açıklanan yeni özellikleri zengin bir dizi içeren [NuGet 2.7 Sürüm Notları](http://docs.nuget.org/docs/release-notes/nuget-2.7).

NuGet bu sürümü eksik paketleri geri yüklemek için NuGet açıkça izin verilen kullanıcı gereksinimini ortadan kaldırır. NuGet 2.7 yüklerken, kullanıcıların örtük olarak eksik paketleri otomatik olarak geri yüklemek için onay. Kullanıcılar, Visual Studio'da NuGet ayarları aracılığıyla paket geri yükleme dışında açıkça seçebilirsiniz. Bu değişiklik, paket geri yükleme şeklini basitleştirir.

## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Scaffolding

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC and Web API Scaffolding - HTTP 404, Not Found error

Bir projeye iskele kurulmuş öğe olduğunda hatayla karşılaşırsanız, projenizin tutarsız bir durumda bırakılır mümkündür. Bazı yapı iskelesi yapılan değişiklikler geri alınacak ancak yüklü NuGet paketleri gibi diğer değişiklikler geri alınacak değil. Yönlendirme yapılandırması değişiklikler geri alınır, gezinme öğeleri iskele kurulmuş kullanıcılar bir HTTP 404 hata alır.

MVC için bu hatayı düzeltmek için yeni iskele kurulmuş Öğe Ekle ve MVC 5 bağımlılıkları seçin (en az veya tam). Bu işlem tüm gerekli değişiklikleri projenize ekleyin.

Web API için bu hatayı düzeltmek için:

1. Aşağıdaki WebApiConfig sınıfı projenize ekleyin.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Uygulamada WebApiConfig.Register yapılandırma\_yöntemi Global.asax dosyasında şu şekilde başlatın:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 için Web iskele kurulmuş öğe sonra çalışmayı durdurur.

Visual Studio Express 2012 için Web iskele kurulmuş öğe Entity Framework (örneğin, Web API 2 denetleyici Entity Framework kullanarak Eylemler ile) sonra çalışmayı durdurursa, Visual Studio Express yerel görüntü Derleme yüklenemedi, mümkündür System.Web.Extensions bağlıdır.

Bu sorunu düzeltmek için System.Web.Extensions MSIL görüntüsü ile çalışmak için Visual Studio Express yapılandırın:

1. Komut istemini yönetici modunda açın.
2. %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE veya % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE (için 64-bit Windows) gidin.
3. VWDExpress.exe.config bir metin düzenleyicisinde açın.
4. Altında aşağıdaki satırı ekleyin &lt;yapılandırma&gt;/&lt;çalışma zamanı&gt; öğe:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Web için yeniden başlatma Visual Studio Express 2012.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Bir sunucu hatası cshtml dosyası withBrowse WithorF5causes görüntüleme

Visual Studio 2012 (veya Visual Studio 2013'te oluşturulan Visual Studio 2012 bir MVC 5 Proje Aç) bir MVC 5 projesi oluşturduğunuzda ve Gözat ile ya da F5 kullanarak cshtml dosyasını görüntüleme girişiminde durumları - hatayla karşılaşırsınız **sunucu hatası '/' Uygulama**. Sunucu gitmek çalışır`http://localhost:XXXX/Views/../XXXX.cshtml`

Bu sorunu çözmek için değiştirme **eylemi Başlat** projenize ayarını **belirli sayfa**. Sayfa için bir değer sağlamanız gerekmez.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Bu değişikliği yaptıktan sonra F5 seçerek gider, uygulamanızın kök dizinine (`http://localhost:XXXX`). Bu davranış Visual Studio 2013'te MVC 5 projeleri için davranış aynı değil nerede **geçerli sayfa** ayarı sayfasını aç başlatır.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>URL yeniden yazma ve Tilde(~)

URL yeniden yazdırmaya kullanıyorsanız, ASP.NET Razor 3 veya ASP.NET MVC 5'e yükselttikten sonra tilde(~) gösterimi artık doğru çalışmayabilir. URL yeniden yazma gibi HTML öğeleri tilde(~) gösterimde etkiler &lt;A /&gt;, &lt;komut dosyası /&gt;, &lt;bağlantı /&gt;, ve bunun sonucunda tilde kök dizinine artık eşler.

Örneğin, istek için yeniden **asp.net/content** için **asp.net**, href özniteliği &lt;A href = "~/content/" /&gt; çözümler **/content/ İçerik /** yerine  **/** . Bu değişiklik gizlemek için ayarlayabileceğiniz **IIS\_WasUrlRewritten** false her Web sayfası veya bağlam **uygulama\_BeginRequest** Global.asax içinde.

<a id="templateissue"></a>
### <a name="templates"></a>Şablonlar

ASP.NET MVC projeleri Visual Studio 2012 Windows 8.1 veya Windows Server 2012 R2, Visual Studio ile oluşturduğunuzda "Yapılandırma Web [url] için ASP.NET 4.5 başarısız oldu." bildiren bir hata iletisi görüntüler

![Yapılandırma hatası](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Bu Windows sürümlerini yüklediğinizde Visual Studio 2012 ASP.NET 4.5 özelliğini etkinleştirmez olduğundan, bu hatayı görürsünüz. ASP.NET 4.5 etkinleştirmek için açıklanan adımları gerçekleştirmek [kapatma Windows özelliklerini aç veya Kapat](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![Windows özelliklerini açma veya kapatma](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Alternatif olarak, komut satırı kullanarak ASP.NET 4.5 etkinleştirebilirsiniz.

1. Komut istemini yönetici modunda açın.
2. ASP.NET 4.5 etkinleştirmek için aşağıdaki komutu çalıştırın.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
