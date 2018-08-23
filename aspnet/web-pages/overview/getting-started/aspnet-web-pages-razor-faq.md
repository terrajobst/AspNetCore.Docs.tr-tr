---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Web sayfaları (Razor) SSS | Microsoft Docs
author: tfitzmac
description: Bu makalede, Webmatrix'i ve ASP.NET Web sayfaları (Razor) hakkında sık sorulan bazı sorular listelenmektedir. Öğretici ASP.NET Web Pages'da (R... kullanılan yazılım sürümleri
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 03e89adc2415808ea49107616f529fa895d6e52a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41757053"
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web sayfaları (Razor) SSS
====================
tarafından [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix, artık bir tümleşik geliştirme ortamı olarak ASP.NET Web sayfaları için önerilir. Kullanım [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) veya [Visual Studio Code'u](https://code.visualstudio.com/).
>
> Bu makalede, Webmatrix'i ve ASP.NET Web sayfaları (Razor) hakkında sık sorulan bazı sorular listelenmektedir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - ASP.NET Web sayfaları (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Bu öğreticide, ASP.NET Web Pages 2, WebMatrix 2 ve Visual Studio 2012 ile de çalışır.


- [ASP.NET Web sayfaları, ASP.NET Web Forms ve ASP.NET MVC arasındaki fark nedir?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [WebMatrix Web sayfaları ile çalışmak için ihtiyacım var?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Web Pages sayfası üzerinde ASP.NET Web Forms denetimleri kullanabilir miyim?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Bir ASP.NET Web sayfaları sitesinde Webmatrix'i kullanarak olmadan dağıtabilir miyim?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [Oturum açma bilgileri desteklemek için WebSecurity Yardımcısı'nı kullanın zorunda mıyım?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [ASP.NET Web sayfaları, HTML5 destekliyor mu?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [JavaScript ve jQuery Web sayfaları ile kullanabilir miyim?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Ek Kaynaklar](#AdditionalResources)

Hataları ve diğer sorunlar hakkında sorular için bkz [ASP.NET Web sayfaları (Razor) sorun giderme kılavuzu](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>ASP.NET Web sayfaları, ASP.NET Web Forms ve ASP.NET MVC arasındaki fark nedir?

Üç dinamik web uygulamaları oluşturmaya yönelik ASP.NET teknolojilerini şunlardır:

- ASP.NET Web sayfaları, dinamik (sunucu tarafı) kodu ve veritabanı erişimi HTML sayfalarını ve özelliklerini basit ve basit söz dizimi ekleme hakkında ele alınmaktadır.
- ASP.NET Web Forms sayfası nesne modeli ve geleneksel pencere türü denetimlerini (düğmeler, liste, vb.) dayanır. Web Forms ile istemci tabanlı (Windows forms) geliştirme çalıştım kişiler alışkın olduğu olay-tabanlı bir modeli kullanır.
- ASP.NET MVC için ASP.NET model görünüm denetleyicisi desenini uygular. Vurgu "görev ayrımı nettir üzerinde" olan (işleme, veri ve kullanıcı Arabirimi katmanları).

Üç tüm çerçeveleri, tam olarak desteklenir ve ASP.NET takımı tarafından geliştirilecek devam edin. Genel olarak, arka plan üzerinde kullanmak için hangi framework seçimi bağlıdır ve ASP.NET ile karşılaşabilirsiniz.

ASP.NET Web Pages özellikle sunucu işleme bunların sayfalarını eklemek için HTML zaten bilen kişilere yönelik kolaylaştırmak için tasarlanmıştır. Öğrenciler, amatörler, genel programlama için yeni olan kişiler için iyi bir seçimdir. ASP.NET dışında web teknolojileri de deneyimi olan geliştiriciler için iyi bir seçim yapılabilir.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>WebMatrix Web sayfaları ile çalışmak için ihtiyacım var?

Hayır. WebMatrix, artık bir tümleşik geliştirme ortamı olarak ASP.NET Web sayfaları için önerilir. Kullanım [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) veya [Visual Studio Code'u](https://code.visualstudio.com/).

Visual Studio veya Visual Studio Code'u kullanmak istemiyorsanız, tek tek kullanarak bileşeni ürünleri yükleyebileceğiniz [Microsoft Web Platformu yükleyicisi](https://www.microsoft.com/web/downloads/platform.aspx). Aşağıdaki ürünler ihtiyacınız vardır:

- Microsoft .NET Framework 4.5
- ASP.NET (Bu da ASP.NET Web Pages framework yükler) MVC 5
- IIS Express (web sunucusu)
- Microsoft SQL Server Compact 4.0 (veritabanı)

Bir metin düzenleyicisinde düzenlemek için kullanabileceğiniz *.cshtml* (veya *.vbhtml*) sayfalar.

SQL Server Compact veritabanını yönetme (*.sdf* dosyaları) olmadan bir biraz daha zor bir araçtır. Visual Studio containds araçları yönetmek için *.sdf* veritabanları. Bu gibi durumlarda, SQL komutlarını de birçok SQL Server yönetim görevlerini gerçekleştirmek için kodu çalıştırabilirsiniz.

Sınanacak *.cshtml* bir tümleşik geliştirme ortamı (IDE) kullanmadan sayfaları, dağıtabileceğiniz bunları için Canlı bir sunucu. (Bkz [WebMatrix kullanmadan bir ASP.NET Web sayfaları sitesinde dağıtabilir?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>IIS Express, bir IDE kullanarak olmadan çalıştırma

IIS Express web sunucusu olarak bilgisayarınızda yüklerseniz, bu sayfaları test etmek için kullanabilirsiniz. IIS Express komut satırından çalıştırabilir ve bunu bir özel bağlantı noktası numarası ile ilişkilendirebilirsiniz. Sonra istediğinizde bu bağlantı noktasını belirtin *.cshtml* tarayıcınızda dosyaları.

Windows, yönetici ayrıcalıklarıyla bir komut istemi açın ve değiştirmek *Files\IIS C:\Program Express.* (64-bit sistemler için klasör kullanmak *C:\Program Files (x86) \IIS Express.)* Ardından, sitenize gerçek yolu kullanarak aşağıdaki komutu girin:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Başka bir işlem tarafından zaten ayrılmış olmayan herhangi bir bağlantı noktası numarası kullanabilirsiniz. (Bağlantı noktası numaralarını 1024'ten genellikle ücretsizdir.) İçin `path` değeri, Web sitesi klasörünün yolunu kullanın. burada *.cshtml* dosyalarıdır.

Sayfalarınızı sunmak için IIS Express'i ayarlamak için bu komutu çalıştırdıktan sonra bir tarayıcı açın ve gidin bir *.cshtml* dosya. Aşağıdaki gibi bir URL kullanın:

`http://localhost:35896/default.cshtml`

IIS Express komut satırı seçenekleri konusunda yardım için girin `iisexpress.exe /?` komut satırına.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Web Pages sayfası üzerinde ASP.NET Web Forms denetimleri kullanabilir miyim?

Hayır. Web Forms denetimleri gibi [onay kutusu](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) denetimi [doğrulama denetimleri](https://msdn.microsoft.com/library/bwd43d0x)ve [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) yalnızca iş Web Forms sayfalarında denetleme (*.aspx* dosyaları). Bu denetimler, Web Forms sayfası altyapısı gerektirir.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Bir ASP.NET Web sayfaları sitesinde Webmatrix'i kullanarak olmadan dağıtabilir miyim?

Evet. Web sitesi dosyalarını bir sunucuya (genellikle FTP kullanarak) el ile kopyalayabilirsiniz. El ile kopyalama gerçekleştirirseniz, ayrıca, SQL Server Compact (veritabanı) destek dosyaları gerekir. Ayrıntılar için blog girişine bakın [bir araç olmadan Web Pages dağıtımı uygulamalar](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>Oturum açma bilgileri desteklemek için WebSecurity Yardımcısı'nı kullanın zorunda mıyım?

Hayır. `SimpleMembership` Bölümü, ASP.NET Web Pages sağlayıcısı, bir seçenektir. (Yani, Web Forms ile çalışmaya kullanılıyor olabilir) ASP.NET parçası olan güvenlik sağlayıcıları de mevcuttur. Örneğin, Web formları içinde olduğu gibi ASP.NET Web sayfaları'nda form kimlik doğrulaması kullanabilirsiniz. Form kimlik doğrulamasını nasıl kullanılacağına ilişkin bir örnek için bkz. Microsoft Support makalesini [ASP.NET uygulamanızı kullanarak C# .NET ile kimlik doğrulaması How To Implement Forms-Based](https://support.microsoft.com/kb/301240). Basit bir örnek yüklemek için bkz [ASP.NET sürümü "oturum açma &amp; parola](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Windows kimlik doğrulaması kullanma hakkında daha fazla bilgi için bkz. blog gönderisine [kullanarak Windows kimlik doğrulaması ASP.NET Web Pages'de](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>ASP.NET Web sayfaları, HTML5 destekliyor mu?

Evet. ASP.NET Web sayfaları ile oluşturduğunuz sayfaları (*.cshtml* veya *.vbhtml* sayfaları) aslında de sayfa işlenmeden önce sunucuda çalışan kod içeren HTML sayfaları. Kullanıcının tarayıcı HTML5 desteklediği sürece, HTML5 öğeleri kullanabilirsiniz. bir *.cshtml* veya *.vbhtml* sayfası.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>JavaScript ve jQuery Web sayfaları ile kullanabilir miyim?

Kesinlikle. ASP.NET Web sayfaları ile oluşturduğunuz sayfaları (*.cshtml* veya *.vbhtml* sayfaları) yalnızca bunları sunucu kodunda HTML sayfaları. Bu nedenle, herhangi bir şey yapabilirsiniz tarafından normal bir HTML sayfasında JavaScript veya jQuery kullanarak da yapabilirsiniz bir *.cshtml* veya *.vbhtml* sayfası.

**Başlangıç sitesi** WebMatrix şablonunda jQuery kitaplıkları içerir. Bu şablonu kullanarak bir site oluşturursanız *betikleri* klasörünü içeren jQuery çekirdek kitaplığı (*jquery 1.6.2.js)* ve jQuery doğrulama kitaplıkları (*jquery.validate.js*vb..).

ASP.NET Web sayfaları ile jQuery kullanmanın yolları göstermeye bazı blog gönderilerini şunlardır:

- [WebMatrix kullanarak ASP.NET Web sayfaları için jQuery özelliği ekleyerek](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) Rachel Appel ile
- [5 dakika: WebMatrix jQuery kullanıcı Arabirimi + json + jQuery şablonları](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) Jonas Eriksson tarafından
- [WebMatrix ve jQuery form](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) Mike Brind tarafından

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Ek Kaynaklar


[ASP.NET Web Sayfaları (Razor) Sorun Giderme Kılavuzu](https://go.microsoft.com/fwlink/?LinkId=253001)

[WebMatrix ve ASP.NET Web sayfaları Forumu](https://forums.asp.net/1224.aspx/1?WebMatrix) ASP.NET Web sitesi
