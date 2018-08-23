---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET ve Web Tools 2013.2 için Visual Studio 2013 sürüm notları | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 2a22c5b686cb8e02054f421f78a8fc910af7ce28
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754673"
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>İçin ASP.NET and Web Tools 2013.2 Visual Studio 2013 sürüm notları
====================
tarafından [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Yükleme notları

ASP.NET ve Web Araçları Visual Studio 2013.2 ana yükleyicide paketlenir ve bir parçası olarak indirilebilir [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Belgeler

Öğreticiler ve diğer bilgileri ASP.NET ve Web Araçları için Visual Studio 2013.2 web'da [ASP.NET web sitesi](https://www.asp.net/).

## <a name="software-requirements"></a>Yazılım Gereksinimleri

Visual Studio 2013 için ASP.NET and Web Tools Visual Studio 2013.2 gerektirir.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>ASP.NET ve Web Araçları Visual Studio 2013.2 için yeni özellikler

Aşağıdaki bölümlerde sürümünde sunulan özellikler açıklanmaktadır.

- [Bir ASP.NET projesi şablonları](#oneaspnet)
- [IIS Express Web uygulamalarını başlatma sırasında SSL desteği](#ssl)
- [Visual Studio Web Düzenleyicisi geliştirmeleri](#vswebeditor)
- [Tarayıcı Bağlantısı](#browserlink)
- [Visual Studio'da Azure App Service Web uygulamaları için destek](#waws)
- [Yeni bir Web projesi oluştururken, uzak Azure kaynakları oluşturma](#AzureResources)
- [Web yayımlama geliştirmeleri](#webpublish)
- [ASP.NET iskeleti oluşturma](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [ASP.NET Web formları](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2'yi](#webapi)
- [3.1.2 ASP.NET Web sayfaları](#webpages)
- [Entity Framework 6.1](#ef)
- [ASP.NET Identity 2.0.0](#identity)
- [Microsoft OWIN bileşenleri](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Bir ASP.NET projesi şablonları

- Hesap onaylama ve parola sıfırlama desteklemek için ASP.NET proje şablonları güncelleştirmeleri.
- ASP.NET Web API şablonu, şirket içi Kurumsal hesaplarını kullanan kimlik doğrulamayı destekleyecek şekilde güncelleştirin.
- ASP.NET SPA şablon, MVC ve sunucu tarafı görünümleri tabanlı kimlik doğrulaması artık içerir. Şablon, yalnızca kimliği doğrulanmış kullanıcılar tarafından erişilebilen bir Webapı denetleyicisi vardır.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>IIS Express Web uygulamalarını başlatma sırasında SSL desteği

Göz atma ve HTTPS komut localhost üzerinde hata ayıklama güvenlik uyarısını ortadan kaldırmak için Internet Explorer ve Chrome otomatik olarak imzalanan IIS güvenmek için SSL sertifikası express izin vermek için bir iletişim kutusu ekledik.

Örneğin, bir web projesi özelliği, SSL kullanacak şekilde ayarlanabilir. F4 Özellikleri iletişim kutusunu açmak için tıklayın. Değişiklik **SSL etkin** true. SSL URL'sini kopyalayın.

![Özellik SSL etkin](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Küme HTTPS kullanmak üzere web proje özelliği sayfasında web sekmesi, temel URL (SSL URL'si olacaktır `https://localhost:44300/` SSL Web sitelerini daha önce oluşturduğunuz sürece.)

![Proje URL'si (HTTPS) ayarlama](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Uygulamayı çalıştırmak için CTRL + F5 tuşlarına basın. IIS Express'in oluşturduğu otomatik olarak imzalanan bir sertifika güven için yönergeleri izleyin.

![SSL Uyarısı](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Okuma **Güvenlik Uyarısı** iletişim ve ardından **Evet** localhost temsil eden bir sertifika yüklemek istiyorsanız.

![Güvenlik Uyarısı](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

Site gösterilir, IE veya Chrome tarayıcıda Sertifika uyarısı olmadan.

![Uyarılar olmadan HTTPS sayfa](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox, kendi sertifika deposu kullanır, bu nedenle, bir uyarı görüntülenir.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Visual Studio Web Düzenleyicisi geliştirmeleri

- **Yeni JSON proje öğesi ve düzenleyici**: JSON proje öğesi ve Düzenleyicisi Visual Studio'ya ekledik. Geçerli JSON Düzenleyicisi özelliklerini renklendirme, söz dizimi doğrulama, küme ayracı tamamlama, anahat oluşturma, araçları seçeneği ayarı ve daha fazlasını içerir.

    ![JSON Düzenleyicisi](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense destekler [JSON şeması](http://json-schema.org/) v3 ve v4. Mevcut şemaları seçin, yerel şema yolu düzenlemek için bir şema birleşik giriş kutusu yok veya basit sürükle bırak proje JSON dosyasına göreli yol almak için.

    ![JSON IntelliSense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![JSON şema Düzenleyicisi](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Yeni (SCSS) Sass Düzenleyicisi**: VS2013 RTM daha düşük ekledik ve Sass proje öğesi ve düzenleyici artık vardır. Ayar vb. özellikler LESS Düzenleyicisi için karşılaştırılabilir ve renklendirme, değişken ve Mixin'ler IntelliSense, yorum açıklamasını kaldırın, hızlı bilgi, biçimlendirme, söz dizimi doğrulama, anahat oluşturma, Git tanımı, Renk Seçici sass Düzenleyicisi araçları.

    ![Yeni Öğe Ekle: SCSS stil sayfası](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Stil Sayfası Düzenleyicisi](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Yeni URL Seçici HTML, Razor, CSS, LESS ve Sass belgeleri:** VS 2013 Web formları sayfaları dışında hiçbir URL'yi Seçici ile birlikte gönderilir. Yeni URL Seçici için HTML, Razor, CSS, LESS ve Sass düzenleyicilerde olduğu anlayan bir iletişim ücretsiz, fluent yazma Seçici '..' ve süzgeçler dosyası img etiketleri ve bağlantılar için uygun şekilde listeler.

    ![URL Seçici için resim etiketi](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![URL Seçici görünümleri](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![URL Seçici için CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Daha fazla özellik ekleyerek LESS Düzenleyicisi için güncelleştirmeleri**
- **Knockout IntelliSense yükseltme**: VS IntelliSense için standart bir KnockOut söz dizimi ekledik "ko vs Düzenleyici viewModel:" söz dizimi. Açıklamaları formunda kullanılarak bir sayfada birden çok görünüm modelleri bağlamak için kullanılabilir:

    ![Knockout IntelliSense](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    ViewModel derin şekilde iç içe geçmiş nesnelerde ayrıntılarına şekilde iç içe geçmiş ViewModel IntelliSense desteği de ekledik.

    `<div data-bind="text: foo.bar.baz.etc" />`

    Görüntülenen IntelilSense IntelliSense JavaScript nesne olur.

    ![IntelliSense gösteren tam JavaScript nesnesi](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Yeni URL Seçici HTML, Razor, CSS, LESS ve Sass belgeleri**: VS 2013 Web formları sayfaları dışında hiçbir URL'yi Seçici ile birlikte gönderilir. Yeni URL Seçici için HTML, Razor, CSS, LESS ve Sass düzenleyicilerde olduğu anlayan bir iletişim ücretsiz, fluent yazma Seçici '..' ve süzgeçler dosyası img etiketleri ve bağlantılar için uygun şekilde listeler.

    ![URL Seçici için resim etiketi](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![URL Seçici görünümleri](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![URL Seçici için CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Tarayıcı bağlantısı

- Tarayıcı bağlantısı artık HTTPS bağlantılarını destekler ve tarayıcı tarafından güvenilen sertifika sürece, diğer bağlantılarla Panoda listeler.
- Statik HTML kaynak eşlemesi
- SPA eşleme verilerini destekler
- Eşleme verilerini otomatik olarak güncelleştir

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Visual Studio'da Azure App Service Web uygulamaları için destek

- **Azure desteği oturum açın.**
- **Uzaktan hata ayıklama ve web uygulamaları için Uzak görünümü**: artık desteklediğimiz [Azure App service'taki web apps için uzaktan hata ayıklama](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) ve sunucu Gezgini'ndeki web app içerik dosyalarının uzak görünümü.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Yeni bir Web projesi oluştururken, uzak Azure kaynakları oluşturma

Bir Azure ekledik ["Uzak kaynaklar oluştur"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) onay kutusuna yeni web uygulaması iletişim kutusu. Seçim yaparak, test ve birkaç basit adımda yayımlama profilini oluşturmak için Azure yayımlama site kurma yeni bir web uygulaması oluşturma deneyimi tümleştirebilir olacaktır.

![Azure kaynaklarıyla yeni proje](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Azure'da yayımlamak için](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Web yayımlama geliştirmeleri

- Yayımlama kullanıcı deneyimini geliştirin.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET iskeleti oluşturma

- **Numaralandırma desteği:** model, sabit listeleri kullanarak sonra MVC iskele kurucu Enum için açılan listeyi oluşturur. Bu, MVC'de Enum Yardımcıları kullanır.
- **Bootstrap Destek**: MVC yapı İskelesi EditorFor şablonlarında önyükleme sınıfları kullandıkları şekilde güncelleştirildi.
- **Destek paketini**: MVC ve Web API platformları eklenir 5.1 paketleri MVC ve Web API'si için

Aşağıdaki ekran görüntüleri, yapı iskelesi modellerini göstermektedir.

- Kod modeli:

     ![Kod modeli](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Kod modeli, sağ tıklatın ve seçin derleme **Ekle**, **yeni iskele kurulmuş öğe**.

     ![Yeni iskele kurulmuş Öğe Ekle](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Seçin **MVC5 denetleyici Entity Framework kullanarak görünümler ile**:

     ![Görünümlere sahip yeni MVC5 denetleyici Ekle](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Denetleyici ekleme** modelini kullanarak:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Oluşturulan kod, örneğin Views/WeekdayModels/Edit.cshtml içeren denetleyin `@Html.EnumDropDownListFor`: ![EnumDropDownListFor içeren görüntüleyin](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Oluşturulan sabit listesi combobox görmek için bir değer null olabilir, boş bir dize için birleşik giriş kutusu seçilebilir dikkat edin sayfasında çalıştırın. Örneğin, **Oluştur** sayfası aşağıdaki gösterir:

    ![Birleşik giriş kutusu boş dizeye izin verme](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 RTM Nisan 2014'te kullanıma sunulacaktır. Sürüm Notları dikkat çekici noktalarından İşte, ancak lütfen denetleyin [tam yayın notlarını](http://docs.nuget.org/docs/release-notes/nuget-2.8) bu değişiklikler hakkında daha fazla bilgi için.

- **Windows Phone 8.1 uygulamaları hedefleyen**: NuGet 2.8.1 artık destekliyor hedefleyen Windows Phone 8.1 uygulamaları hedef framework takma ad 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' ve 'WPA81' kullanımı.
- **Bağımlılıklar için düzeltme eki çözümleme**: NuGet Paket bağımlılıklarını çözülürken geçmişte paket bağımlılıkları karşılayan en düşük büyük ve küçük bir paket sürümü seçmek için bir strateji uygulamıştır. Birincil ve ikincil sürüm, ancak, düzeltme eki sürümü her zaman için en yüksek sürüm çözümlendi. Davranışı iyi niyetli olsa, bağımlılıkları olan paket yüklemek için gerekircilik eksikliği oluşturuldu.
- **DependencyVersion anahtar**: değiştirir ancak NuGet 2.8 *varsayılan* bağımlılıkları çözümlemek için davranış de ekler - DependencyVersion anahtarı aracılığıyla bağımlılık çözümleme işlemi üzerinde daha kesin denetim Paket Yöneticisi konsolu. Çözümleme bağımlılıkları olası en düşük sürüm (varsayılan davranış), en yüksek olası sürümü veya en yüksek ikincil veya düzeltme eki sürümü için anahtar sağlar. Bu anahtar yalnızca powershell komutu Install-package çalışır.
- **DependencyVersion özniteliği**: yukarıda ayrıntılı - DependencyVersion anahtarına ek olarak, NuGet ayrıca için yeni bir öznitelik - DependencyVersion geçerseniz, varsayılan değer nedir, tanımlama nuget.config dosyasında ayarlamanıza olanak tanıdı Install-package bir çağırma hedefi belirtilmedi. Bu değer, NuGet Paket Yöneticisi iletişim kutusu için herhangi bir yükleme paketi işlemi tarafından da kanalla. Bu değeri ayarlamak için nuget.config dosyanızda aşağıdaki özniteliği ekleyin:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **NuGet işlemleri ile - whatif Önizleme**: bazı NuGet paketlerini ayrıntılı bağımlılık grafikleri sahip olabilir ve bu nedenle, bir yükleme sırasında yararlı, kaldırma veya yükleyebilir güncelleştirme işlemi ilk ne olacağını görmek için. NuGet 2.8 ekler standart PowerShell-tüm paketler için komut uygulanacak kapatılmasını görselleştirme etkinleştirmek için Install-package, kaldırma-package ve update-package komutları için ne geçin.
- **Paketinin eski sürümünü**: Bu yeni özellikler araştırmak için bir paket yayım öncesi bir sürümü yükleyin ve ardından en son kararlı sürüme geri karar vermektir. NuGet 2.8 önce bu ön sürüm paketi ve bağımlılıklarını kaldırmak ve ardından önceki bir sürümünü yükleme çok adımlı bir işlemin değildi. NuGet 2.8 ile ancak, güncelleştirme paketini şimdi tüm paket kapatma (örneğin paket Bağımlılık ağacı) önceki sürümüne geri döner.
- **Geliştirme bağımlılıkları**: farklı türlerde özellikleri geliştirme süreci iyileştirmek için kullanılan araçları dahil olmak üzere NuGet paketleri - olarak teslim edilebilir. Yeni bir paket geliştirirken ınstrumental olabilir, ancak bu bileşenler, sonraki olduğunda yeni paketi bir bağımlılık yayımlanan kabul edilmemelidir. NuGet 2.8 kendisini .nuspec dosyasında bir developmentDependency olarak tanımlamak bir paket sağlar. Yüklendiğinde, bu meta veriler Ayrıca paket içine yüklenmiş proje packages.config dosyasına eklenir. Bu packages.config dosyası daha sonra NuGet bağımlılıklarını sırasında nuget.exe paketi analiz edilirken geliştirme bağımlılıkları olarak işaretlenmiş bu bağımlılıklara dışında bırakır.
- **Farklı platformlar için tek tek packages.config dosyaları**: birden çok hedef platformlar için uygulama geliştirirken, farklı proje dosyalarının her biri kendi yapı ortamları için çok yaygındır. Paketleri farklı platformları için destek farklı düzeylerde olduğundan da farklı proje dosyalarındaki farklı NuGet paketlerini kullanmak için yaygındır. NuGet 2.8 farklı platforma özgü proje dosyaları için farklı packages.config dosyaları oluşturarak, bu senaryo için gelişmiş destek sağlar.
- **Geri dönüş için yerel önbellek**: rağmen NuGet paketleri genellikle kullandığı uzak bir Galeriden gibi [NuGet galerisinde](http://www.nuget.org) ağ bağlantısını kullanarak, burada istemci bağlı değil birçok senaryo vardır. Bir ağ bağlantısı olmadan NuGet istemci bile bu paketleri zaten istemcinin makinede yerel NuGet önbelleğini olduğu durumlarda paketler - başarılı bir şekilde yükleyebilmesi için o değildi. 2.8 NuGet Paket Yöneticisi konsolu için geri dönüş otomatik önbellek ekler.

    Önbellek temel özellik, herhangi belirli komut satırı bağımsız değişkenlerini gerektirmez. Ayrıca, geri dönüş önbelleği şu anda yalnızca Paket Yöneticisi Konsolu'nda çalışır - davranışı Paket Yöneticisi iletişim kutusunda şu anda çalışmıyor.
- **Hata düzeltmeleri**: yapılan önemli hata düzeltmeleri biriydi performans geliştirmesinden-package-komut yeniden yükleyin.

    Bu özellikler ve yukarıda sözü edilen performans düzeltme ek olarak, bu sürümü NuGet, ayrıca diğer birçok hata düzeltmeleri içerir. Sürümünde giderilen 181 toplam sorunlar oluştu. Tam bir listesi için iş öğeleri NuGet 2.8 Lütfen görünümü sabit [NuGet sorun İzleyicisi](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) bu sürüm için.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>ASP.NET Web Forms

- Web Forms şablonlarında artık hesap onaylama ve parola sıfırlama için ASP.NET Identity yapmak nasıl gösterir.
- Varlık veri kaynağı denetimini ve Entity Framework 6 için dinamik veri sağlayıcısı. Daha fazla ayrıntı için lütfen şu MSDN Web günlüğü postasına bakın: [dinamik veri sağlayıcısı ve Entity Framework 6 EntityDataSource denetimi](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Öznitelik yönlendirme geliştirmeleri](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Düzenleyici şablonları önyükleme desteği](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Görünümlerde enum desteği](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [MinLength Unobstrusive desteği / MaxLength öznitelikleri](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- ['This' bağlamı içinde örtük Ajax destekleme](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Çeşitli [hata düzeltmeleri](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2'yi

- [Genel hata işleme](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Öznitelik yönlendirme geliştirmeleri](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Yardım sayfası geliştirmeleri](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [IgnoreRoute desteği](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [BSON medya türü biçimlendiricisi](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Zaman uyumsuz filtreleri için daha iyi destek](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [İstemci Kitaplığı biçimlendirme için ayrıştırma sorgulama](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Çeşitli [hata düzeltmeleri](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>3.1.2 ASP.NET Web sayfaları

- Çeşitli [hata düzeltmeleri](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework, hem çalışma zamanı ve araç için 6.1 sürümüne güncelleştirildi. Entity Framework (EF) 6.1 Entity Framework 6 küçük bir güncelleştirmesidir ve hata düzeltmeleri ve yeni özellikler içerir. Yeni özellikler için belgelere bağlantılar dahil olmak üzere, EF6.1 hakkında ayrıntılı bilgi için bkz. [Entity Framework sürüm geçmişi](https://msdn.microsoft.com/data/jj574253). Bu sürümdeki yeni özellikler şunları içerir:

- **Birleştirme Araçları** yeni EF modeli oluşturmak için tutarlı bir yol sağlar. Bu özellik, ADO.NET varlık veri modeli Sihirbazı'nı oluşturma Code First modelleri, var olan bir veritabanından tersine mühendislik dahil olmak üzere destekleyecek şekilde genişletir. Bu özellikler daha önce Beta kalite EF Power Tools içinde mevcuttu.
- **İşlem işleme hatalarının işleme** yeni sağlar [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) hale getirir işlem operations kesiştirmek için yeni sunulan özelliğini kullanın. **CommitFailureHandler** bir işlem Sistemi'ne artırabileceksiniz bağlantı hatalardan otomatik kurtarma sağlar.
- **IndexAttribute** dizinleri Code First modelinizde bir özelliği (veya Özellikler) üzerindeki bir özniteliği yerleştirerek belirtilmesini sağlar. Kod önce sonra karşılık gelen bir dizin veritabanında oluşturursunuz.
- **Genel eşleme API** EF sahip nasıl özellikler ve türler sütun ve veritabanındaki tablolar eşlendiğine şirket bilgilerine erişim sağlar. Sürümleri bu API iç içindeydi.
- **Kesiciler App/Web.config dosyasıyla yapılandırma yeteneği**(uygulama yeniden derlemeye gerek kalmadan eklenecek dinleyicileri olanak tanır).
- **DatabaseLogger** günlük dosyasında tüm veritabanı işlemleri kolaylaştıran yeni bir dinleyiciyi olduğu. Bir önceki özelliği ile birlikte, bu yeniden derlemenize gerek kalmadan, dağıtılmış bir uygulama için veritabanı işlem günlüğünü bir kolayca geçiş yapmanızı sağlar.
- **Geçişleri modeli değişiklik algılama** iskele kurulmuş geçişleri değişiklik algılama işleminin performansını de büyük ölçüde geliştirilmiştir daha doğru; olacak şekilde iyileştirilmiştir.
- **Performans iyileştirmeleri** başlatma sırasında azaltılmış veritabanı işlemleri dahil olmak üzere en iyi duruma getirme LINQ sorgularında null eşitlik karşılaştırması için daha hızlı oluşturma (modeli oluşturma) daha fazla senaryolarda ve görüntüleme daha verimli materialization birden fazla ilişki ile izlenen varlık.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>ASP.NET Identity 2.0.0

- **İki öğeli kimlik doğrulama**: ASP.NET Identity artık iki öğeli kimlik doğrulamayı destekler. İki öğeli kimlik doğrulama güvenlik burada parolanızı güvenliğinin tehlikeye girmesi durumunda, kullanıcı hesapları için ek bir koruma katmanı sağlar. İki öğeli kodları deneme yanılma saldırıları için koruma de mevcuttur.
- **Hesap kilitleme:** kullanıcının oturumunu kullanıcı parolalarını veya iki öğeli kodları hatalı şekilde girerse kilitlemek için bir yol sağlar. Kullanıcıların kilitli geçersiz denemeleri ve zaman aralığı sayısı yapılandırılabilir. Bir geliştirici isteğe bağlı olarak belirli bir kullanıcı hesapları için hesap kilitleme devre dışı ihtiyacınız olursa dışı bırakabilirsiniz.
- **Hesap onaylama:** ASP.NET kimlik sistemi artık hesap onaylama destekler. Burada, Web sitesinde yeni bir hesap için kaydolduğunuzda, Web sitesinde herhangi bir şey yapmadan önce e-postanızı doğrulayın gerekmez oldukça sık karşılaşılan bir senaryodur bugün çoğu Web Siteleri'nde budur. E-posta onayı yararlıdır, çünkü bu sahte hesapları oluşturulmasını engeller. E-posta, Web sitenizin Forumu siteleri, bankacılık, e-ticaret ve sosyal web siteleri gibi kullanıcılarla iletişim yöntemi olarak kullanıyorsanız bu son derece kullanışlıdır.
- **Parola sıfırlama:** parola sıfırlama, burada kullanıcı sıfırlayabilmeniz parolalarını parolalarını unuttuysa bir özelliktir.
- **Güvenlik damgasını (oturumunuzu her yerde):** destekleyen bir kullanıcı parolasını ya da herhangi bir güvenlik değiştiğinde durumlarda kullanıcı için güvenlik belirteci yeniden şekilde ilgili (örneğin, Facebook, Google, ilişkili bir oturum kaldırma gibi bilgileri Microsoft hesabı ve benzeri). Bu, eski parola ile oluşturulan herhangi bir belirteçler geçersiz emin olmak için gereklidir. Örnek Proje, kullanıcının parolasını değiştirirseniz sonra kullanıcı için yeni bir belirteç oluşturulur ve önceki tarafından istenen belirteçleri geçersiz kılınır. Bu özellik, güvenlik, parola değiştirdiğinizde bu yana, uygulamanıza ek bir koruma katmanı sağlar, size yerden (tüm diğer tarayıcılar için), bu uygulamaya nereden oturum oturumunuz kapatılacak.
- **Kullanıcılar ve roller için Genişletilebilir olmak birincil anahtar türü olun**: ASP.NET Identity 1.0, tablo kullanıcılar ve roller için birincil anahtar türündeydi dizeleri. ASP.NET kimlik sistemi için Entity Framework kullanarak SQL Server trvalý olduğunda bunun anlamı, biz nvarchar kullanıyordu. Bu varsayılan uygulama Stack overflow'da etrafında birçok tartışmaları vardı ve gelen geri bildirimine göre. Hangi kullanıcıların ve rollerin tablonun birincil anahtarı olmalıdır belirleyebileceğiniz bir genişletilebilirlik kanca sağladık. Bu genişletilebilirlik kanca uygulamanızı geçişi ve uygulama depolama sağlayıcılarıyla GUID'leri ya da tamsayıları kayanlara olan özellikle yararlıdır.
- **Kullanıcıların ve rollerin Iqueryable Destek**: Iqueryable UsersStore ve RolesStore desteği eklendi, kullanıcıların ve rollerin listesini kolayca elde edebilirsiniz.
- **UserManager aracılığıyla destek silme işlemi**
- **Kullanıcı adı özniteliklerinde dizin**: ASP.NET Identity Entity Framework uygulamasında, eklediğimiz benzersiz bir dizin kullanıcı adı özniteliklerinde EF 6.1.0 yeni IndexAttribute kullanarak. Bu, kullanıcı adları her zaman benzersizdir ve hangi, yinelenen kullanıcı adları ile bulunabileceğini herhangi bir yarış durumu oluştu emin olur.
- **Gelişmiş Parola doğrulayıcı:** ASP.NET Identity 1.0 setleriyle Parola doğrulayıcı yalnızca en az uzunluğu doğrulama bir oldukça temel bir parola Doğrulayıcı oluştu. Parola karmaşıklığını üzerinde daha fazla denetim sağlayan yeni bir parola Doğrulayıcı yok. Bu parola, tüm ayarlarını açmanız bile, kullanıcı hesapları için iki öğeli kimlik doğrulamasını etkinleştirmek için öneriyoruz olduğunu lütfen unutmayın.
- **IdentityFactory ara yazılım / CreatePerOwinContext**:

    - **Kullanıcı yöneticisini**: Üreteç uygulaması OWIN bağlamında UserManager örneğini almak için kullanabilirsiniz. Bu düzen, bulunan OWIN bağlamından oturum açma ve oturum kapatma için almak için kullandığımız için benzerdir. Bu uygulama için istek başına UserManager örneğini alma önerilen bir yoldur.
    - **DbContextFactory**: ASP.NET Identity SQL Server'da kimlik sistemi kalıcı hale getirmeniz için Entity Framework kullanır. Kimlik sistemi bunu ApplicationDbContext başvuru gerekir. DbContextFactory ara yazılım, uygulamanızda kullanabileceğiniz istek başına ApplicationDbContext örneğini döndürür.
- **ASP.NET Identity örnekleri NuGet paketini**: örnekleri NuGet paketi kolaylaştıran yüklemek için ASP.NET Identity örneklerini çalıştırma ve en iyi uygulamaları izleyin. Örnek bir ASP.NET MVC uygulaması budur. Lütfen kodu, bu üretim ortamında dağıtmadan önce uygulamanızın uyacak şekilde değiştirin. Boş bir ASP.NET uygulamasında örneğe'nın yüklü olması gerekir. Paketi hakkında daha fazla bilgi için aşağıdaki Web günlüğü gönderisinde için Git: [Duyurusu RTM, ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Microsoft OWIN bileşenleri

Bu sürümde düzeltilen hataların çok sayıda vardı. Lütfen [2.1.0 için sürüm notları yayın](https://katanaproject.codeplex.com/releases/view/113281) daha ayrıntılı bilgi için.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Bu sürümde düzeltilen hataların çok sayıda vardı. Lütfen [sürüm notları 2.0.2 yayın](https://github.com/SignalR/SignalR/releases/tag/2.0.2) daha ayrıntılı bilgi için.
