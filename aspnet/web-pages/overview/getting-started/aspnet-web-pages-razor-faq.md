---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Web sayfaları (Razor) ile ilgili SSS | Microsoft Docs
author: tfitzmac
description: Bu makalede, ASP.NET Web sayfaları (Razor) ve WebMatrix hakkında sık sorulan bazı sorular listelenmektedir. Öğretici ASP.NET Web Pages'da (r.. kullanılan yazılım sürümleri
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 60cc4ca364923cb131d5e91cd7b6307b1e68644b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042465"
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web sayfaları (Razor) ile ilgili SSS
====================
tarafından [zel FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix, artık bir tümleşik geliştirme ortamı olarak ASP.NET Web sayfaları için önerilir. Kullanım [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) veya [Visual Studio Code](https://code.visualstudio.com/).
>
> Bu makalede, ASP.NET Web sayfaları (Razor) ve WebMatrix hakkında sık sorulan bazı sorular listelenmektedir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2, WebMatrix 2 ve Visual Studio 2012 ile de çalışır.


- [ASP.NET Web sayfaları, ASP.NET Web Forms ve ASP.NET MVC arasındaki fark nedir?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [WebMatrix Web sayfaları ile çalışmak için ihtiyacım var mı?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [ASP.NET Web Forms denetimleri Web Pages sayfasında kullanabilir miyim?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Bir ASP.NET Web Pages sitesinde WebMatrix kullanmadan dağıtabilir miyim?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [Oturum açma bilgileri desteklemek için WebSecurity Yardımcısı'nı kullanabilirsiniz var mı?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [ASP.NET Web sayfaları HTML5 destekliyor mu?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [JavaScript ve Jquery'de Web sayfalarıyla kullanabilir miyim?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Ek kaynaklar](#AdditionalResources)

Hataları ve diğer sorunlar hakkında daha fazla soruları için bkz [ASP.NET Web sayfaları (Razor) sorun giderme kılavuzu](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>ASP.NET Web sayfaları, ASP.NET Web Forms ve ASP.NET MVC arasındaki fark nedir?

Üç dinamik web uygulamaları oluşturmak için ASP.NET teknolojileri şunlardır:

- ASP.NET Web sayfaları odaklanır dinamik (sunucu tarafı) kodu ve veritabanı erişimi HTML sayfaları ve özellikleri basit ve basit sözdizimi ekleme.
- ASP.NET Web Forms sayfası nesne modeli ve geleneksel pencere türü denetimleri (düğmeleri, listeler, vb.) dayanır. Web Forms ile istemci tabanlı (Windows forms) geliştirme çalıştığınız seçen alışkın olduğu olay tabanlı bir modeli kullanır.
- ASP.NET MVC için ASP.NET model view controller deseni uygular. Vurgu "sorunları üzerinde" ayrılmasıdır (işleme, veri ve UI katmanları).

Tüm üç çerçevelerine tam olarak desteklenir ve ASP.NET ekibiniz tarafından geliştirilen devam eder. Genel olarak, arka plan üzerinde kullanmak için hangi framework seçimi bağlıdır ve ASP.NET ile karşılaşırsınız.

ASP.NET Web sayfaları özellikle sunucu işleme kendi sayfalara eklemek için HTML zaten bilen kişiler için kolaylaştırmak için tasarlanmıştır. Öğrenciler, deneyimli olmayanlar, genel programlama için yeni olan kişiler için iyi bir seçimdir. ASP.NET olmayan web teknolojileri ile deneyim sahibi geliştiriciler için iyi bir seçimdir da olabilir.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>WebMatrix Web sayfaları ile çalışmak için ihtiyacım var mı?

Hayır. WebMatrix, artık bir tümleşik geliştirme ortamı olarak ASP.NET Web sayfaları için önerilir. Kullanım [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) veya [Visual Studio Code](https://code.visualstudio.com/).

Visual Studio veya Visual Studio Code kullanmak istemiyorsanız, kullanarak tek tek bileşen ürünleri yükleyebilirsiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx). Şu ürünler gerekir:

- Microsoft .NET Framework 4.5
- ASP.NET (hangi ASP.NET Web sayfaları framework de yükler) MVC 5
- IIS Express (the web server)
- Microsoft SQL Server Compact 4.0 (veritabanı)

Bir metin Düzenleyicisi'ni düzenlemek için kullanabileceğiniz *.cshtml* (veya *.vbhtml*) sayfaları.

SQL Server Compact veritabanları yönetme (*.sdf* dosyaları) olmadan bir aracı biraz daha zordur. Yönetmek için visual Studio containds Araçları *.sdf* veritabanları. Ayrıca, birçok SQL Server yönetim görevlerini gerçekleştirmek için kodunda SQL komutlarını çalıştırabilirsiniz.

Test etmek için *.cshtml* tümleşik geliştirme ortamı (IDE) kullanmadan sayfaları, dağıtabileceğiniz bunları canlı bir sunucu. (Bkz [WebMatrix kullanmadan bir ASP.NET Web Pages sitesinde ı dağıtabilir miyim?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Bir IDE kullanmadan çalışan IIS Express

IIS Express web sunucusu olarak bilgisayarınızda yüklerseniz, bu sayfaları test etmek için kullanabilirsiniz. IIS Express komut satırından çalıştırabilir ve bu belirli bağlantı noktası numarası ile ilişkilendirin. İstediğinde, bu bağlantı noktasını ardından belirtin *.cshtml* tarayıcınızda dosyaları.

Windows'da, yönetici ayrıcalıklarıyla bir komut istemi açın ve değiştirmek *C:\Program Files\IIS Express.* (64 bit sistemler için klasörü kullanın *C:\Program Files (x86) \IIS Express.)* Ardından, sitenize gerçek yolu kullanarak aşağıdaki komutu girin:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Başka bir işlem tarafından zaten ayrılmış olmayan herhangi bir bağlantı noktası numarası kullanabilirsiniz. (Bağlantı noktası numaraları 1024'ten genellikle ücretsizdir.) İçin `path` değeri, Web sitesi klasörünün yolunu kullanın nerede *.cshtml* dosyalarıdır.

Sayfalarınızın sunmak için IIS Express'i ayarlamak için bu komutu çalıştırdıktan sonra bir tarayıcı açın ve gidin bir *.cshtml* dosya. Aşağıdaki gibi bir URL kullanın:

`http://localhost:35896/default.cshtml`

IIS Express komut satırı seçenekleri ile ilgili Yardım için girin `iisexpress.exe /?` komut satırında.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>ASP.NET Web Forms denetimleri Web Pages sayfasında kullanabilir miyim?

Hayır. Web Forms denetimleri gibi [onay kutusunu](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) denetimi [doğrulama denetimleri](https://msdn.microsoft.com/library/bwd43d0x)ve [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) yalnızca Web formları sayfaları iş denetim (*.aspx* dosyaları). Bu denetimler, Web Forms sayfası altyapısı gerektirir.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Bir ASP.NET Web Pages sitesinde WebMatrix kullanmadan dağıtabilir miyim?

Evet. Web sitesi dosyalarını bir sunucuya (genellikle FTP kullanarak) el ile kopyalayabilirsiniz. El ile kopyalama gerçekleştirirseniz, ayrıca SQL Server Compact (veritabanı) destek dosyalarını kopyalayın gerekir. Ayrıntılar için blog girişine bakın [bir aracı olmadan Web Pages dağıtımı uygulamalar](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>Oturum açma bilgileri desteklemek için WebSecurity Yardımcısı'nı kullanabilirsiniz var mı?

Hayır. `SimpleMembership` Bölümü, ASP.NET Web sayfaları sağlayıcı bir seçenek değil. (Web Forms ile çalışmaya kullanılabilir) ASP.NET parçası olan güvenlik sağlayıcıları de kullanılabilir. Örneğin, Web Forms gibi ASP.NET Web Pages'da form kimlik doğrulaması kullanabilirsiniz. Form kimlik doğrulamasını nasıl kullanılacağına ilişkin bir örnek için Microsoft Support makalesini bkz [ASP.NET uygulamanızı kullanarak C# .NET tarafından How To Implement Forms-Based kimlik doğrulaması](https://support.microsoft.com/kb/301240). Basit bir örnek indirmek için bkz: [ASP.NET sürümü "oturum açma &amp; parola](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Windows kimlik doğrulaması kullanma hakkında daha fazla bilgi için blog gönderisine bakın [kullanarak Windows kimlik doğrulaması, ASP.NET Web sayfaları](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>ASP.NET Web sayfaları HTML5 destekliyor mu?

Evet. ASP.NET Web sayfaları ile oluşturduğunuz sayfaları (*.cshtml* veya *.vbhtml* sayfaları) temelde sayfa işlenmeden önce sunucuda çalışan bir kod da içeren HTML sayfaları. HTML5 kullanıcının tarayıcısının desteklediği sürece HTML5 öğelerinde kullanabileceğiniz bir *.cshtml* veya *.vbhtml* sayfası.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>JavaScript ve Jquery'de Web sayfalarıyla kullanabilir miyim?

Kesinlikle. ASP.NET Web sayfaları ile oluşturduğunuz sayfaları (*.cshtml* veya *.vbhtml* sayfaları) yalnızca HTML sayfaları bunlara sunucu koduna sahip. Bu nedenle, herhangi bir şey yapabilirsiniz tarafından normal bir HTML sayfasında JavaScript veya jQuery kullanarak da yapabilirsiniz bir *.cshtml* veya *.vbhtml* sayfası.

**Başlangıç sitesi** WebMatrix şablonunda jQuery kitaplıkları içerir. Bu şablonu kullanarak bir site oluşturursanız *betikleri* klasörde jQuery çekirdek kitaplığı (*jquery 1.6.2.js)* ve jQuery doğrulama için kitaplıkları (*jquery.validate.js*, vb..).

JQuery ASP.NET Web sayfaları ile kullanmak için yol göstermeye bazı blog gönderileri şunlardır:

- [JQuery güzelliklerine Webmatrix'i kullanarak ASP.NET Web sayfaları için ekleme](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) Rachel Appel tarafından
- [5 dk.: WebMatrix jQuery UI + json + jQuery şablonları](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) Jonas Eriksson tarafından
- [WebMatrix ve jQuery form](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) CAN Brind tarafından

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[ASP.NET Web Sayfaları (Razor) Sorun Giderme Kılavuzu](https://go.microsoft.com/fwlink/?LinkId=253001)

[WebMatrix ve ASP.NET Web sayfaları Forumu](https://forums.asp.net/1224.aspx/1?WebMatrix) ASP.NET Web sitesi
