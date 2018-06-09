---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET ve Web Araçları Visual Studio 2013 sürüm notları için 2013.2 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2014
ms.topic: article
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 0e7ad52662f7ceaa1f087d007d0b14b610f90bee
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "28036030"
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET ve Web Araçları 2013.2 Visual Studio 2013 sürüm notları
====================
tarafından [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Yükleme notları

ASP.NET ve Web Araçları Visual Studio 2013.2 için ana yükleyicisinde paketlenmiştir ve bir parçası olarak indirilebilir [Visual Studio 2013 güncelleştirme 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Belgeler

Öğreticiler ve Visual Studio 2013.2 ASP.NET ve Web Araçları ilgili diğer bilgiler web'da [ASP.NET web sitesi](https://www.asp.net/).

## <a name="software-requirements"></a>Yazılım Gereksinimleri

ASP.NET ve Web Araçları Visual Studio 2013.2 için Visual Studio 2013 gerektirir.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>ASP.NET ve Web Araçları Visual Studio 2013.2 için yeni özellikler

Aşağıdaki bölümlerde sürümünde tanıtılan özellikleri açıklanmaktadır.

- [Bir ASP.NET proje şablonları](#oneaspnet)
- [IIS Express Web uygulamalarını başlatılırken SSL desteği](#ssl)
- [Visual Studio Web Düzenleyicisi geliştirmeleri](#vswebeditor)
- [Tarayıcı Bağlantısı](#browserlink)
- [Visual Studio'da Azure App Service Web uygulamaları için destek](#waws)
- [Yeni bir Web projesi oluştururken, uzaktan Azure kaynakları oluşturun](#AzureResources)
- [Web yayımlama geliştirmeleri](#webpublish)
- [ASP.NET İskele](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [ASP.NET Web formları](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [ASP.NET Web sayfaları 3.1.2](#webpages)
- [Entity Framework 6.1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Microsoft OWIN bileşenleri](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Bir ASP.NET proje şablonları

- Güncelleştirmeler hesap ve parola sıfırlama desteklemek için ASP.NET proje şablonları sağlar.
- ASP.NET Web API şablonu şirket içi Kurumsal hesaplar kullanarak kimlik doğrulamayı destekleyecek şekilde güncelleştirin.
- ASP.NET SPA şablon şimdi MVC ve sunucu tarafı görünümleri temel kimlik doğrulaması içerir. Şablon, yalnızca kimliği doğrulanmış kullanıcılar tarafından erişilebilen bir Webapı denetleyicisi vardır.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>IIS Express Web uygulamalarını başlatılırken SSL desteği

Gözatma ve HTTPS localhost üzerinde hata ayıklama güvenlik uyarısı ortadan kaldırmak için Internet Explorer ve Chrome otomatik olarak imzalanan IIS güvenmek için SSL sertifikası express izin vermek için bir iletişim kutusu eklediğimiz.

Örneğin, bir web projesi özelliği, SSL kullanacak şekilde ayarlanabilir. Özellikler iletişim kutusunu açmak için F4'e tıklayın. Değişiklik **SSL etkin** true. SSL URL'sini kopyalayın.

![SSL özelliği etkin](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Küme HTTPS kullanmak üzere web projesi özellik sayfası web sekmesi, temel URL (SSL URL'si olacaktır `https://localhost:44300/` SSL Web siteleri daha önce oluşturmuş olduğunuz sürece.)

![Proje URL'si (HTTPS) ayarlayın](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Uygulamayı çalıştırmak için CTRL + F5 tuşuna basın. IIS Express üretti otomatik olarak imzalanan sertifika güven için yönergeleri izleyin.

![SSL uyarı](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Okuma **Güvenlik Uyarısı** iletişim ve ardından **Evet** localhost temsil eden sertifika yüklemek istiyorsanız.

![Güvenlik Uyarısı](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Sitesi gösterilir, IE veya Chrome tarayıcıda Sertifika uyarısı olmadan.

![HTTPS sayfa uyarılar olmadan](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox, kendi sertifika deposuna kullanır, bu nedenle bir uyarı görüntülenir.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web Düzenleyicisi geliştirmeleri

- **Yeni JSON proje öğesi ve düzenleyici**: JSON proje öğesi ve düzenleyici için Visual Studio ekledik. Geçerli JSON Düzenleyicisi özelliklerini renklendirme sözdizimi doğrulama, küme parantezi tamamlama, anahat oluşturma, araçları seçeneği ayarı ve daha fazla yer alır.

    ![JSON Düzenleyicisi](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense şimdi destekler [JSON şeması](http://json-schema.org/) v3 hem de v4. Mevcut şemaları seçin, yerel şema yolu düzenlemek için bir şema birleşik giriş kutusu yoktur veya yalnızca sürükle bırak proje JSON dosyasına, göreli yolu.

    ![JSON IntelliSense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![JSON şema Düzenleyicisi](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Yeni Sass (SCSS) Düzenleyicisi**: VS2013 RTM daha düşük eklediğimiz ve şimdi Sass proje öğesi ve düzenleyici sunuyoruz. Özellikleri için daha az Düzenleyicisi karşılaştırılabilir ve renklendirme, değişken ve Mixins IntelliSense, yorum/açıklamadan çıkarın, hızlı bilgi, biçimlendirme, sözdizimi doğrulama, anahat oluşturma, goto tanımı, Renk Seçici kapsar sass Düzenleyicisi seçeneği vb. ayarlamak araçları.

    ![Yeni Öğe Ekle: SCSS stil sayfası](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Stil Sayfası Düzenleyicisi](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **HTML, Razor, CSS, daha az yeni URL seçicide ve Sass belgeleri:** VS 2013 sevk Web formları sayfaları dışında hiçbir URL Seçici. HTML, Razor, CSS, daha az için yeni URL Seçici ve Sass düzenleyicileri olan özelliğini algılayan bir iletişim ücretsiz, fluent yazarak Seçici '..' ve filtreleri dosya img etiketleri ve bağlantıları için uygun şekilde listeler.

    ![Resim etiketi için URL Seçici](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![URL Seçici görünümleri](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![CSS için URL Seçici](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Daha fazla özellik ekleyerek LESS Düzenleyici güncelleştirmeleri**
- **Boşaltılan IntelliSense yükseltme**: VS IntelliSense için standart olmayan bir Boşaltılan sözdizimi eklediğimiz "ko vs Düzenleyicisi viewModel:" sözdizimi. Birden çok görünüm modeller biçiminde açıklamaları kullanılarak bir sayfa üzerinde bağlamak için kullanılabilir:

    ![Boşaltılan IntelliSense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    İç içe ViewModel nesnelerde ayrıntılarına şekilde biz de iç içe geçmiş ViewModel IntelliSense desteği eklendi.

    `<div data-bind="text: foo.bar.baz.etc" />`

    Görüntülenen IntelilSense JavaScript nesnesinin tam IntelliSense ' dir.

    ![IntelliSense gösteren tam JavaScript nesne](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **HTML, Razor, CSS, daha az yeni URL seçicide ve Sass belgeleri**: VS 2013 sevk Web formları sayfaları dışında hiçbir URL Seçici. HTML, Razor, CSS, daha az için yeni URL Seçici ve Sass düzenleyicileri olan özelliğini algılayan bir iletişim ücretsiz, fluent yazarak Seçici '..' ve filtreleri dosya img etiketleri ve bağlantıları için uygun şekilde listeler.

    ![Resim etiketi için URL Seçici](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![URL Seçici görünümleri](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![CSS için URL Seçici](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Tarayıcı bağlantısı

- Tarayıcı bağlantısı artık HTTPS bağlantılarını destekler ve sertifika tarayıcı tarafından güvenilir olduğu sürece, diğer bağlantılarla panosunda listeler.
- Statik HTML kaynağı eşleme
- SPA desteklemek için eşleme verileri
- Otomatik güncelleştirme eşleştirme verisi

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Visual Studio'da Azure App Service Web uygulamaları için destek

- **Destek Azure'da oturum açın.**
- **Uzaktan hata ayıklama ve web uygulamaları için Uzak görünümü**: artık desteklenmektedir [Azure App Service'deki web uygulamaları için uzaktan hata ayıklama](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) ve web uygulaması içerik dosyalarının server Explorer'da uzak görünümü.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Yeni bir Web projesi oluştururken, uzaktan Azure kaynakları oluşturun

Bir Azure eklediğimiz ["Uzak kaynaklar oluştur"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) yeni web uygulaması iletişim kutusundaki onay kutusu. Bunu seçerek, test ve birkaç basit adımda yayımlama profili oluşturmak için Azure yayımlama sitesi ayarlama yeni bir web uygulaması oluşturma deneyimi tümleştirebilir olacaktır.

![Azure kaynaklarıyla yeni proje](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Azure'da yayımlamak için](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Web yayımlama geliştirmeleri

- Yayımlama kullanıcı deneyimini geliştirir.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET İskele

- **Enum desteği:** model numaralandırmaları kullanarak sonra MVC iskele kurucu Enum için açılır liste oluşturur. Bu, MVC'de Enum Yardımcıları kullanır.
- **Bootstrap Destek**: önyükleme sınıfları kullanması için yapı İskelesi MVC EditorFor şablonlarında güncelleştirildi.
- **Destek paketini**: MVC ve Web API Scaffolders ekleyecek 5.1 paketleri MVC ve Web API için

Aşağıdaki ekran görüntüleri yapı iskelesi modellerini göstermektedir.

- Model kod:

     ![Model kod](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Model kod, sağ tıklatın ve seçin derleme **Ekle**, **yeni iskele kurulmuş öğe**.

     ![Yeni iskele kurulmuş Öğe Ekle](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Seçin **MVC5 Entity Framework kullanarak görüntüleme özellikleri ile denetleyicisi,**:

     ![Görünümlerle yeni MVC5 denetleyici ekleyin](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Denetleyici ekleme** modelini kullanarak:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Örneğin Views/WeekdayModels/Edit.cshtml içeren oluşturulan kodu kontrol `@Html.EnumDropDownListFor`: ![görüntülemek EnumDropDownListFor içeren](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Oluşturulan enum combobox görmek için bir değer null olabilir, boş bir dize açılan kutu için seçilebilir fark sayfayı çalıştırın. Örneğin, **oluşturma** sayfası şunları gösterir:

    ![Birleşik giriş kutusu boş dize izin verme](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1 ile

NuGet 2.8.1 ile RTM Nisan 2014'te kullanıma sunulacaktır. Sürüm Notları belirgin noktalarından İşte, ancak lütfen denetleyin [tam sürüm notları](http://docs.nuget.org/docs/release-notes/nuget-2.8) bu değişiklikler hakkında daha fazla bilgi için.

- **Hedef Windows Phone 8.1 uygulamaları**: NuGet 2.8.1 ile artık destekliyor 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' ve 'WPA81' hedef framework adlar kullanarak Windows Phone 8.1 uygulamaları hedefleme.
- **Bağımlılıklar için düzeltme eki çözümleme**: NuGet Paket bağımlılıklarını çözülürken tarihsel olarak, paket bağımlılıkları karşılayan düşük birincil ve ikincil Paket sürümü seçmek için bir strateji uyguladı. Birincil ve ikincil sürüm, ancak, düzeltme eki sürümü her zaman en yüksek sürüme çözümlendi. Davranış iyi niyetli olsa, bağımlılıkları olan paketleri yüklemek için determinism eksikliği oluşturulur.
- **DependencyVersion anahtar**: olsa NuGet 2.8 değiştirir *varsayılan* bağımlılıkları çözümleniyor davranışı, ayrıca ekler - DependencyVersion anahtarı aracılığıyla bağımlılık çözümleme işlemi üzerinde daha kesin denetim Paket Yöneticisi konsolu. Anahtarı, olası en düşük sürüm (varsayılan davranış), en yüksek olası sürümünü veya en yüksek ikincil ya da düzeltme eki sürüme çözümleme bağımlılıkları sağlar. Bu anahtar yalnızca powershell komutu Install-package için çalışır.
- **DependencyVersion özniteliği**: Yukarıdaki ayrıntılı - DependencyVersion anahtarı yanı sıra, NuGet de yeni bir öznitelik - DependencyVersion geçiş yaparsanız varsayılan değer nedir, tanımlama nuget.config dosyasında ayarlama özelliği için izin Install-package bir çalıştırılışı belirtilmedi. Bu değer ayrıca herhangi bir yükleme paketi işlem için NuGet Paket Yöneticisi iletişim kutusu tarafından uygulanır. Bu değeri ayarlamak için aşağıdaki öznitelik nuget.config dosyanıza ekleyin:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **NuGet işlemleri ile - whatIf Önizleme**: bazı NuGet paketleri derin bağımlılık grafikleri sahip olabilir ve bu nedenle, bir yükleme sırasında yararlı, kaldırma veya yükleyebilir önce ne olacağını görmek için güncelleştirme işlemi. NuGet 2.8 ekler standart PowerShell-ne olduğu komutu uygulanacak paketleri tüm kapatması görselleştirme etkinleştirmek için Install-package, kaldırma paketi ve güncelleştirme paketi komutları için geçiş yapın.
- **Paket düşürmek**: Bu yeni özellikler araştırmak için bir paket yayın öncesi bir sürümünü yükleyin ve ardından en son kararlı sürüme geri almak karar vermektir. Yayın öncesi paketi ve bağımlılıklarını kaldırmayı ve sonra önceki sürümü yükleme çok adımlı bir işlemi, bu NuGet 2.8 önce. NuGet 2.8 ile ancak, güncelleştirme paketini şimdi tüm paketi Kapatılmak üzere (örneğin paketin bağımlılığı ağacı) önceki sürümüne geri döner.
- **Geliştirme bağımlılıkları**: farklı türlerde yetenekleri geliştirme sürecinin iyileştirmek için kullanılan araçları dahil olmak üzere NuGet paketleri - olarak alınabilir. Yeni bir paket geliştirmede enstrümental olabileceği bu bileşenler, sonraki olduğunda yeni paketi bir bağımlılık yayımlanan düşünülmemelidir. NuGet 2.8 kendisini .nuspec dosyası bir developmentDependency olarak tanımlamak bir paket sağlar. Yüklendiğinde, bu meta veriler Ayrıca paket yüklendiği proje packages.config dosyasına eklenir. Daha sonra için NuGet bağımlılıkları, packages.config dosyasına sırasında nuget.exe paketi analiz edilir, geliştirme bağımlılıklar olarak işaretlenmiş bu bağımlılıklara dışında bırakır.
- **Farklı platformlar için tek tek packages.config dosyaları**: birden çok hedef platformlar için uygulama geliştirirken, farklı proje dosyaları her ilgili derleme ortamları için yaygın bir sorundur. Paketleri farklı platformlar için destek değişen düzeylerde olduğundan ayrıca farklı proje dosyalarında farklı NuGet paketlerini kullanmak için yaygındır. NuGet 2.8 farklı platforma özgü proje dosyaları için farklı packages.config dosyaları oluşturarak bu senaryo için gelişmiş destek sağlar.
- **Yerel önbelleğe almak için geri dönüş**: olsa NuGet paketlerini genellikle kullanılan uzak galerisinden gibi [NuGet galerisinde](http://www.nuget.org) ağ bağlantısını kullanarak, burada istemci bağlı değil birçok senaryo vardır. Bir ağ bağlantısı NuGet istemci bile bu paketleri zaten istemcinin makinede yerel NuGet önbellekteki zamanki paketleri - başarıyla yüklemek mümkün değildi. NuGet 2.8 otomatik önbellek geri dönüş için Paket Yöneticisi konsolu ekler.

    Önbellek geri dönüş özelliğinin tüm belirli komut bağımsız değişkenleri gerektirmez. Ayrıca, geri dönüş önbelleği şu anda yalnızca Paket Yöneticisi konsolunda çalışır - davranışı Paket Yöneticisi iletişim kutusunda şu anda çalışmıyor.
- **Hata düzeltmeleri**: yapılan önemli hata düzeltmeleri birinde performans geliştirmesi güncelleştirme paketindeki-komutu yeniden yükleyin.

    Bu özellikler ve daha önce bahsedilen performans düzeltme ek olarak, bu sürümü NuGet, diğer birçok hata düzeltmeleri de içerir. Sürümde ele 181 toplam sorunları vardı. İş tam listesi için öğeleri NuGet 2.8 Lütfen görünüm sabit [NuGet sorun İzleyicisi](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) bu sürüm için.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET Web Forms

- Web Forms şablonları artık hesap ve parola sıfırlama için ASP.NET Identity nasıl yapacağınızı.
- Varlık veri kaynağı denetimini ve Entity Framework 6 için dinamik veri sağlayıcısı. Daha fazla ayrıntı için lütfen aşağıdaki MSDN blog bkz: [dinamik veri sağlayıcı ve Entity Framework 6 EntityDataSource denetim](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Yönlendirme geliştirmeleri özniteliği](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Düzenleyici şablonları önyükleme desteği](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Görünümlerde enum desteği](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [MinLength Unobstrusive desteği / MaxLength öznitelikleri](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- ['This' bağlam içinde örtük Ajax destekleme](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Çeşitli [hata düzeltmeleri](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [Genel hata işleme](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Yönlendirme geliştirmeleri özniteliği](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Yardım sayfası geliştirmeleri](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [IgnoreRoute desteği](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [BSON medya türü biçimlendiricisi](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Zaman uyumsuz filtreleri için daha iyi destek](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Kitaplık biçimlendirme istemcisi ayrıştırma sorgulama](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Çeşitli [hata düzeltmeleri](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>ASP.NET Web sayfaları 3.1.2

- Çeşitli [hata düzeltmeleri](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework sürüm 6.1 çalışma zamanı ve araçları için güncelleştirilmiştir. Entity Framework (EF) 6.1 Entity Framework 6 küçük bir güncelleştirmesidir ve çeşitli hata düzeltmeleri ve yeni özellikler içerir. Yeni özellikleri için belgelere bağlantıları dahil olmak üzere EF6.1 hakkında ayrıntılı bilgi için bkz: [Entity Framework sürüm geçmişi](https://msdn.microsoft.com/data/jj574253). Bu sürümdeki yeni özellikleri şunlardır:

- **Birleştirme tooling** yeni bir EF modeli oluşturmak için tutarlı bir yol sağlar. Bu özelliği, var olan bir veritabanından ters mühendislik dahil olmak üzere oluşturma Code First modelleri desteklemek için ADO.NET varlık veri modeli Sihirbazı genişletir. Bu özellikler Beta kalitesinde EF Güç Araçları'nda önceden kullanılabilir.
- **İşlem yürütme hatalarının işleme** yeni sağlar [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) hangi yapar TRANSACTION işlemlerini müdahale yeni sunulan özelliğini kullanın. **CommitFailureHandler** bir işlem gerçekleştirmeden adımında bağlantı hataları otomatik kurtarma sağlar.
- **IndexAttribute** Code First modelinizin bir özellik (veya özellikleri) üzerinde bir öznitelik koyarak belirtilmesi için dizinler sağlar. Kod ilk ardından karşılık gelen bir dizin veritabanını oluşturur.
- **Ortak eşleme API** EF sahip nasıl özellikleri ve türleri sütunlara ve veritabanındaki tabloların eşlendiğini üzerinde bilgilerine erişim sağlar. Sürümleri bu API iç içindeydi.
- **Dinleyiciler App/Web.config dosyası aracılığıyla yapılandırma yeteneği**(uygulama derlemeden eklenecek dinleyiciler izin vererek).
- **DatabaseLogger** tüm veritabanı işlemleri bir günlük dosyasına daha kolay hale getirir yeni bir dinleyiciyi değil. Önceki özellik ile birlikte, bu, veritabanı işlemleri için yeniden derleyin gerek kalmadan dağıtılan bir uygulama günlüğünü kolayca geçiş yapmanızı sağlar.
- **Geçişler modeli değişikliği algılama** kurulmuş geçişler değişiklik Algılama işlemi performansını da büyük ölçüde geliştirilmiştir daha doğru; böylece geliştirilmiştir.
- **Performans iyileştirmeleri** başlatma sırasında azaltılmış veritabanı işlemleri dahil olmak üzere en iyi duruma getirme LINQ sorgularında null eşitlik karşılaştırması için daha hızlı görüntülemek oluşturma (modeli oluşturma) daha fazla senaryolarda ve daha verimli materialization birden fazla ilişki ile izlenen varlık.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **İki öğeli kimlik doğrulama**: ASP.NET Identity şimdi iki öğeli kimlik doğrulamayı destekler. İki öğeli kimlik doğrulama fazladan bir kullanıcı hesaplarınızı burada parolanızı gizliliğinin durumda güvenlik katmanı sağlar. İki öğeli kodları deneme yanılma saldırıları için koruma yoktur.
- **Hesap kilitleme:** kullanıcı parolalarını veya iki öğeli kodları yanlış girerse, kullanıcı kilitlemek için bir yol sağlar. Kullanıcıların kilitli için geçersiz denemeleri ve timespan sayısı yapılandırılabilir. Bir geliştirici isteğe bağlı olarak bazı kullanıcı hesapları için hesap kilitleme devre dışı ihtiyacınız olursa dışı bırakabilirsiniz.
- **Hesap doğrulama:** ASP.NET Identity sistem hesabı onayı artık destekler. Burada, Web sitesinde yeni bir hesap için kaydolduğunuzda, Web sitesindeki herhangi bir şey yapmak için önce e-posta onayı istenir oldukça yaygın bir senaryo Günümüzde çoğu Web sitelerindeki budur. E-posta onayı yararlıdır çünkü sahte hesaplar oluşturulmasını engeller. Forum siteler, bankacılık, e-ticaret veya sosyal web siteleri gibi Web sitenizin kullanıcılarla iletişim yöntemi olarak e-posta kullanıyorsanız, bu son derece yararlıdır.
- **Parola sıfırlama:** parola sıfırlama, burada kullanıcı sıfırlama parolalarını parolalarını unuttuysa bir özelliktir.
- **Güvenlik damgasını (oturum kapatma her yerde):** kullanıcı parolalarını veya başka bir güvenlik değiştirdiğinde durumlarda kullanıcı için güvenlik belirteci yeniden oluşturmak için yol desteklemektedir ilgili (örneğin, Facebook, Google, ilişkili bir oturum kaldırılması gibi bilgileri Microsoft hesabı vb.). Bu, eski parola ile oluşturulan herhangi bir belirtece geçersiz kılınır emin olmak için gereklidir. Örnek projesinde, kullanıcının parolasını değiştirirseniz, ardından yeni bir belirteç kullanıcı için oluşturulur ve herhangi bir önceki belirtece geçersiz kılınır. Her yerde (tüm diğer tarayıcılar için), bu uygulamaya burada oturum gelen kaydedilir, bu özellik fazladan bir parolanızı değiştirdiğinizde itibaren uygulamanız için güvenlik katmanı sağlar.
- **Kullanıcılar ve roller için Genişletilebilir olmak birincil anahtar türü olun**: ASP.NET Identity 1.0, tablo kullanıcılar ve roller için birincil anahtar türündeydi dizeleri. Entity Framework kullanarak SQL Server ASP.NET Identity sistem kalıcı olduğunda bunun anlamı, biz nvarchar kullanmakta olduğunuz. Bu varsayılan uygulama yığın taşması üzerinde birçok tartışmalarını vardı ve gelen geri bildirimi doğrultusunda. Ne, kullanıcılar ve roller tablonun birincil anahtarı olması gerektiğini belirtebileceğiniz bir genişletilebilirlik kanca sağladık. Bu genişletilebilirlik kanca uygulamanızı geçiriyorsanız ve uygulama depolanmasını kullanıcı kimliklerini GUID'leri veya ints demektir özellikle yararlıdır.
- **Kullanıcılar ve roller üzerinde Iqueryable Destek**: Iqueryable UsersStore ve RolesStore desteği eklendi, kullanıcıları ve rolleri listesini kolayca alabilirsiniz.
- **UserManager aracılığıyla destek silme işlemi**
- **Kullanıcı adı özniteliklerinde dizin**: ASP.NET Identity Entity Framework uygulamasını eklediğimiz benzersiz bir dizin kullanıcı EF 6.1.0 yeni IndexAttribute kullanarak. Bu kullanıcı adlarını her zaman benzersiz ve hangi, yinelenen kullanıcı adları ile şunun hiçbir yarış durumu oluştu emin olur.
- **Gelişmiş Parola doğrulayıcı:** ASP.NET Identity 1. 0 ' sevk edilen Parola doğrulayıcı yalnızca en az uzunluk doğrulama oldukça temel parola Doğrulayıcı oluştu. Parola karmaşıklığını üzerinde daha fazla denetim sağlar yeni bir parola Doğrulayıcı yoktur. Lütfen bu parolayı dosyasındaki tüm ayarların kapatmanız olsa bile, kullanıcı hesaplarını iki faktörlü kimlik doğrulamasını etkinleştirmek için öneririz olduğunu unutmayın.
- **IdentityFactory Ara / CreatePerOwinContext**:

    - **Kullanıcı Yöneticisi**: UserManager örneği OWIN bağlamı elde etmek için Üreteç uygulaması kullanabilirsiniz. Bu desen ne bulunan OWIN bağlamından Signın ve SignOut için almak için kullandığımız için benzer. Bu uygulama için istek başına UserManager örneği almanın önerilen bir yoludur.
    - **DbContextFactory**: ASP.NET Identity Entity Framework SQL Server'daki kimlik sistemi sürdürmek için kullanır. Kimlik sistemi bunun için ApplicationDbContext bir başvuru içeriyor. DbContextFactory ara yazılım, uygulamanızda kullanabilirsiniz istek başına ApplicationDbContext örneğini döndürür.
- **ASP.NET Identity örnekleri NuGet paketi**: örnekler NuGet paketi kolaylaştırır yükleme ve ASP.NET kimliği için örneklerini çalıştırma ve en iyi uygulamaları izleyin. Örnek bir ASP.NET MVC uygulaması budur. Lütfen bu üretimde dağıtmadan önce uygulamanızın uyacak şekilde kodu değiştirin. Örnek boş bir ASP.NET uygulamasında yüklenmesi gerekir. Paketi hakkında daha fazla bilgi için aşağıdaki blog yayını gidin: [tanışın RTM, ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Microsoft OWIN bileşenleri

Çok sayıda bu sürümünde düzeltilen hatalar vardı. Lütfen bakın [2.1.0 için sürüm notları yayın](https://katanaproject.codeplex.com/releases/view/113281) daha ayrıntılı bilgi için.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Çok sayıda bu sürümünde düzeltilen hatalar vardı. Lütfen bakın [2.0.2 için sürüm notları yayın](https://github.com/SignalR/SignalR/releases/tag/2.0.2) daha ayrıntılı bilgi için.
