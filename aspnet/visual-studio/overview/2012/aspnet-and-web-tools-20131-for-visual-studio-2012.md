---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: ASP.NET ve Visual Studio 2012 Web Araçları 2013.1 sürüm notları | Microsoft Docs
author: microsoft
description: Bu belgede ASP.NET ve Web Araçları 2013.1 sürüm Visual Studio 2012 için açıklanmaktadır.
ms.author: aspnetcontent
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 33577dec7278694f932ac7eded84359baacf65bc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829107"
---
<a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>ASP.NET ve Visual Studio 2012 Web Araçları 2013.1 sürüm notları
====================
tarafından [Microsoft](https://github.com/microsoft)

> Bu belgede ASP.NET ve Web Araçları 2013.1 sürüm Visual Studio 2012 için açıklanmaktadır.


## <a name="contents"></a>İçindekiler

- [Yükleme notları](#install)
- [Yazılım gereksinimleri](#requirements)
- ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için yeni özellikler

    - [Önyükleme](#bootstrap)
    - [Şablonlar](#templates)

        - [ASP.NET MVC 5 şablonu](#mvc5template)
        - [ASP.NET Web API 2 şablon](#apitemplate)
        - [Öğe şablonları](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [ASP.NET iskeleti oluşturma](#scaffold)
    - [Razor Düzenleyicisi](#razor)
    - [NuGet 2.7](#nuget)
- Bilinen sorunlar ve yeni değişiklikler

    - [ASP.NET iskeleti oluşturma](#issuescaffolding)

        - [MVC ve Web APİ'si yapı iskelesini - HTTP 404 bulunamadı hatası](#404issue)
        - [Visual Studio Express 2012 Web için iskele kurulmuş öğe ekledikten sonra çalışmayı durduruyor.](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [Şununla Gözat veya F5 ile cshtml dosyasını görüntüleyerek bir sunucu hatasına neden olur](#browseissue)
        - [URL yeniden yazma ve Tilde(~)](#rewriteissue)
    - [Şablonlar](#templateissue)

<a id="install"></a>
## <a name="installation-notes"></a>Yükleme notları

[Yükleme](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için.

<a id="requirements"></a>
## <a name="software-requirements"></a>Yazılım Gereksinimleri

Visual Studio 2012 veya Visual Studio Express 2012 için Web olmalıdır.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için yeni özellikler

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Önyükleme

MVC 5 denetleyicileri ve görünümleri iskelesini görünümleri için biçimlendirme kullanır [önyükleme](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Şablonlar

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>ASP.NET MVC 5 şablonu

Yeni bir MVC 5 şablon ekledik. MVC 5 NuGet paketlerini en son başvuruyor ve yapı iskelesinde denetleyicileri ve görünümleri eklemek için kullanabilirsiniz.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>ASP.NET Web API 2 şablon

Yeni bir Web API 2 şablonu ekledik. En son Web API 2 NuGet paket başvuruları ve yapı iskelesinde denetleyicileri ve görünümleri eklemek için kullanabilirsiniz.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Öğe şablonları

MVC 5 görünüm, Web sayfaları (Razor 3) ve Web API 2 denetleyicileri için yeni bir öğe şablonları ekledik. Bunlar ilgili NuGet paketleri projeye yeni öğeleri eklenirken yükler.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Entity Framework kullanarak bir MVC veya Web API denetleyicisi iskelesini, Framework 6 kullanırız. Entity Framework hakkında daha fazla bilgi için bkz: [Entity Framework sürüm geçmişi](https://msdn.com/data/jj574253).

Ayrıca, indirin ve Visual Studio 2012 için Entity Framework 6 Tools yükleyin. Bkz: [alın, varlık çerçevesi](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET iskeleti oluşturma

ASP.NET iskeleti oluşturma bir ASP.NET Web uygulamaları için kod oluşturma çerçevedir. Bu, ortak kod projenize bir veri modeli ile etkileşim eklemek kolaylaştırır.

Visual Studio'nun önceki sürümlerinde, ASP.NET MVC projeleri için yapı iskelesi sınırlıydı. Bu güncelleştirmeyle, artık Web Forms dahil olmak üzere tüm ASP.NET proje için yapı iskelesi kullanabilirsiniz. Bu güncelleştirme için bir Web formları projesi oluşturma sayfaları desteklemez, ancak yapı iskelesi Web Forms ile projeye MVC bağımlılıkları ekleyerek kullanmaya devam edebilirsiniz. İçin Web formları sayfaları oluşturmak için destek, gelecek güncelleştirmelerden birinde eklenecektir.

Yapı iskelesi kullanırken, gerekli tüm bağımlılıkların projede yüklü olduğundan emin olun. Bir ASP.NET Web formları projesi ile başlayın ve ardından bir Web API denetleyicisi eklemek için yapı iskelesi kullanın, örneğin, başvurular ve gerekli NuGet paketlerini projenize otomatik olarak eklenir.

MVC yapı iskelesi Web Forms projesine eklemek için Ekle bir **yeni iskele kurulmuş öğe** seçip **MVC 5 bağımlılıkları** iletişim penceresinde. MVC yapı iskelesini oluşturmak için iki seçenek vardır; Minimal ve tam. En az seçerseniz, yalnızca NuGet paketleri ve ASP.NET MVC için başvuruları projenize eklenir. Tam seçeneği seçerseniz bir MVC projesi için gerekli içerik dosyalarının yanı sıra en az bağımlılıklar eklenir.

Zaman uyumsuz denetleyicilerinin yapı iskelesini oluşturmak için destek, Entity Framework 6 yeni zaman uyumsuz özelliklerini kullanır.

Daha fazla bilgi ve eğitimler için bkz. [ASP.NET yapı İskelesi genel bakış](../2013/aspnet-scaffolding-overview.md). Bu öğreticiler Visual Studio 2013 ile yapı iskelesi gösterir, ancak aynı zamanda ASP.NET ve Web Araçları 2013.1 Visual Studio 2012 için geçerli olan.

<a id="razor"></a>
### <a name="razor-editor"></a>Razor Düzenleyicisi

Bu güncelleştirme ile Visual Studio 2012, Razor 3 araç veya düzenleme artık desteklemektedir.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2.7 adresinde ayrıntılı olarak açıklanan yeni özellikleri zengin bir dizi içeren [NuGet 2.7 Sürüm Notları](http://docs.nuget.org/docs/release-notes/nuget-2.7).

NuGet'ın bu sürümü eksik paketleri geri yüklemek için NuGet açıkça izin verilen kullanıcı gereksinimini kaldırır. Örtük olarak NuGet 2.7, kullanıcılar yükleme sırasında otomatik olarak eksik paketleri geri yüklemek için onay vermiş olursunuz. Kullanıcılar, Visual Studio'da NuGet ayarları aracılığıyla paket geri yükleme dışında açıkça tercih edebilirsiniz. Bu değişiklik, paket geri yükleme şeklini basitleştirir.

## <a name="known-issues-and-breaking-changes"></a>Bilinen sorunlar ve yeni değişiklikler

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET iskeleti oluşturma

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>MVC ve Web APİ'si yapı iskelesini - HTTP 404 bulunamadı hatası

İskele kurulmuş öğe projeye eklenirken bir hatayla karşılaşırsanız, projenizi tutarsız bir durumda bırakılır mümkündür. Bazı yapı iskelesi yapılacak değişiklikler geri alınır ancak yüklü NuGet paketleri gibi diğer değişiklikleri geri alınmaz. Yönlendirme yapılandırma değişikliklerini geri giderek iskele kurulmuş öğe olduğunda kullanıcıların bu HTTP 404 hatası alırsınız.

MVC için bu hatayı düzeltmek için yeni iskele kurulmuş öğe ekleme ve MVC 5 bağımlılıkları seçin (en az veya tam). Bu işlem tüm gerekli değişiklikleri projenize ekleyin.

Web API'si için bu hatayı düzeltmek için:

1. Aşağıdaki WebApiConfig sınıfı projenize ekleyin.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. WebApiConfig.Register yapılandırın\_yöntemi Global.asax dosyasında şu şekilde başlatın:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 Web için iskele kurulmuş öğe ekledikten sonra çalışmayı durduruyor.

Visual Studio Express 2012 için Web (örneğin, Web API 2 denetleyici Entity Framework kullanarak Eylemler ile) Entity Framework iskele kurulmuş öğe ekledikten sonra çalışmayı durdurursa, Visual Studio Express bir derlemenin yerel görüntüsünü yüklenemedi, mümkündür System.Web.Extensions bağlıdır.

Bu sorunu düzeltmek için System.Web.Extensions MSIL görüntüsü ile çalışmak için Visual Studio Express yapılandırın:

1. Yönetici modunda komut istemini açın.
2. %ProgramFiles%\Microsoft Visual Studio 11.0\Common7\IDE ya da (64 bit Windows için) % ProgramFiles(x86) %\Microsoft Visual Studio 11.0\Common7\IDE gidin.
3. VWDExpress.exe.config bir metin düzenleyicisinde açın.
4. Altında aşağıdaki satırı ekleyin &lt;yapılandırma&gt;/&lt;çalışma zamanı&gt; öğesi:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Web için Express 2012 Visual Studio'yu yeniden başlatın.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-withbrowse-withorf5causes-a-server-error"></a>Bir sunucu hatası cshtml dosyası withBrowse WithorF5causes görüntüleme

Visual Studio 2012 (veya Visual Studio 2013'te oluşturulan Visual Studio 2012 MVC 5 Proje Aç) bir MVC 5 projesi oluşturun ve cshtml dosyası ile Gözat veya F5'i kullanarak görüntülemeye çalışmak bildiren - hata alırsınız **sunucu hatası '/' Uygulama**. Gitmek sunucunun çalışır `http://localhost:XXXX/Views/../XXXX.cshtml`

Bu sorunu çözmek için değiştirme **başlatma eylemi** projenize ayarını **belirli sayfa**. Sayfa için bir değer sağlamanız gerekmez.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Bu değişikliği yaptıktan sonra F5'i seçerek gider, uygulamanızın kök dizinine (`http://localhost:XXXX`). Bu davranışı Visual Studio 2013'te MVC 5 projeleri için davranışı aynı değildir burada **geçerli sayfa** ayarı açık sayfanın başlatır.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>URL yeniden yazma ve Tilde(~)

URL yeniden yazma işlemleri kullanıyorsanız, ASP.NET Razor 3 veya ASP.NET MVC 5'e yükselttikten sonra tilde(~) gösterimi artık doğru şekilde çalışmayabilir. URL yeniden yazma gibi HTML öğeleri tilde(~) gösteriminde etkiler &lt;A /&gt;, &lt;BETİK /&gt;, &lt;bağlantı /&gt;, ve bunun sonucunda tilde kök dizinine artık eşler.

Örneğin, istek için yeniden **asp.net/content** için **asp.net**, href özniteliği &lt;bir href = "~/content/" /&gt; çözümler **/content/ İçerik /** yerine **/**. Bu değişiklik bastırmak için ayarlayabileceğiniz **IIS\_WasUrlRewritten** false her Web sayfasını veya bağlam **uygulama\_BeginRequest** içindeki.

<a id="templateissue"></a>
### <a name="templates"></a>Şablonlar

ASP.NET MVC projeleri Visual Studio 2012 Windows 8.1 veya Windows Server 2012 R2, Visual Studio ile oluşturduğunuzda "Yapılandırma Web [url] için ASP.NET 4.5 başarısız oldu." diyen bir hata iletisi görüntüler

![yapılandırma hatası](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Visual Studio 2012 ASP.NET 4.5 özelliğini etkinleştirmez, bu Windows sürümleri üzerinde yüklü olduğunda dolayı bu hatayı görebilirsiniz. ASP.NET 4.5 etkinleştirmek için açıklanan adımları uygulayın. [kapatma Windows özelliklerini aç veya Kapat](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![Windows özelliklerini açma veya kapatma](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Alternatif olarak, komut satırı aracılığıyla ASP.NET 4.5 etkinleştirebilirsiniz.

1. Yönetici modunda komut istemini açın.
2. ASP.NET 4.5 etkinleştirmek için aşağıdaki komutu çalıştırın.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
