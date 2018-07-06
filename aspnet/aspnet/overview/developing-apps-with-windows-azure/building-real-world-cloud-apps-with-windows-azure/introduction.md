---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Azure ile gerçek hayatta kullanılan bulut uygulamaları oluşturma | Microsoft Docs
author: MikeWasson
description: Bu e-kitap, gerçek hayatta kullanılan bulut çözümleri oluşturmaya yönelik desen tabanlı yaklaşımda size gösterilmektedir. Olarak da geliştirme sürecine desenleri uygulamak bir...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 11388b9c58d2d21c7a337a343c10d33c7257ccf1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840504"
---
<a name="building-real-world-cloud-apps-with-azure"></a>Azure ile gerçek hayatta kullanılan bulut uygulamaları oluşturma
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Bu e-kitap, gerçek hayatta kullanılan bulut çözümleri oluşturmaya yönelik desen tabanlı yaklaşımda size gösterilmektedir. Desenler, geliştirme sürecine hem hem de mimari ve kodlama çalışmalarına uygulanır.
> 
> İçerik Scott Guthrie tarafından geliştirilen ve ona göre Haziran 2013'te Norveç Geliştiriciler Konferansı'konumunda (NDC) sunulan bir sunuma dayalıdır ([bölüm 1](http://vimeo.com/68215538), [2. bölüm](http://vimeo.com/68215602)) ve Microsoft Teknik Ed Avustralya Eylül 2013 ([bölüm 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [2. bölüm](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Diğer birçok](more-patterns-and-guidance.md#acknowledgments) güncelleştirildi ve içeriğin videodan yazılı biçime geçişi sırasında içerikte güncelleştirme yapmıştır.


## <a name="intended-audience"></a>Hedef kitle

Bulut geliştirme hakkında merak buluta öz veya Bulut geliştiriciler, en önemli kavramları ve bilmeniz gereken uygulamaları kısa bir genel bakış burada bulabilirsiniz. Kavramlar somut örnekler ve diğer kaynakların daha ayrıntılı bilgi için her bölüm bağlantılarını gösterilmiştir. Örnekler ve ek kaynakların bağlantıları Microsoft çerçeveleri ve Hizmetleri içindir, ancak anlatılan ilkeler diğer web geliştirme çerçeveleri için geçerlidir ve bulut ortamları da neden olabilir.

Zaten bulut için geliştirdiğiniz geliştiriciler daha başarılı olacak yardımcı olacak fikirler burada bulabilirsiniz. Serideki her bölüm bağımsız olarak, böylece çekme ve ilginizi çeken konuları seçin okuyabilirsiniz.

Scott Guthrie'nin izlenen herkes *yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile* sunu ve daha fazla ayrıntı ve güncelleştirilmiş bilgileri bulabilirsiniz, burada istediği.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Bulut geliştirme desenleri

Bu e-kitap, bulut geliştirme desenleri On üç önerilen açıklar. "Deseni" burada genel anlamda şeyler için önerilen yol anlama için kullanılır: nasıl en iyi geliştirme, tasarlama ve bulut uygulamaları kodlama gidin. Bunlar, izlerseniz, "başarı pıt ayrılır" yardımcı olan anahtar desenleri alır.

- [Her şeyi otomatikleştirin](automate-everything.md).

    - Verimliliği en üst düzeye çıkarmak ve tekrarlanan işlemler hataları en aza komut dosyalarını kullanın.
    - Demo: Azure yönetim komut dosyaları.
- [Kaynak denetimi](source-control.md). 

    - DevOps iş akışı kolaylaştırmak için kaynak denetimindeki dallandırma yapısını ayarlayın.
    - Demo: komut dosyaları kaynak denetimine ekleyin.
    - Demo: hassas verileri kaynak denetimi dışında tutun.
    - Demo: Visual Studio'da Git kullanın.
- [Sürekli tümleştirme ve teslim](continuous-integration-and-continuous-delivery.md). 

    - Derleme ve dağıtım her kaynak denetimine iade ile otomatik hale getirin.
- [Web geliştirme en iyi](web-development-best-practices.md). 

    - Durum bilgisi olmayan Web Katmanı tutun.
    - Demo: ölçeklendirme ve Azure App Service Web apps'te otomatik ölçeklendirme.
    - Oturum durumu kaçının.
    - CDN kullanılamıyorsa bir CDN geri dönüş konusunda kullanın.
    - Zaman uyumsuz programlama modeli kullanır.
    - Demo: ASP.NET MVC ve Entity Framework zaman uyumsuz.
- [Çoklu oturum açma](single-sign-on.md). 

    - Azure Active Directory giriş.
    - Demo: Azure Active Directory kullanan ASP.NET uygulaması oluşturun.
- [Veri Depolama Seçenekleri](data-storage-options.md). 

    - Veri deposu türleri.
    - Doğru veri deposunu seçme.
    - Demo: Azure SQL veritabanı.
- [Veri bölümleme stratejileri](data-partitioning-strategies.md). 

    - Veri bölümleme dikey, yatay olarak veya bir ilişkisel veritabanının ölçeklenmesi kolaylaştırmak için her ikisini de.
- [Yapılandırılmamış blob depolama](unstructured-blob-storage.md). 

    - Dosyaları blob hizmetini kullanarak bulutta Store.
    - Demo: Düzelt uygulamada blob depolama kullanma.
- [Hatalara karşı tasarım](design-to-survive-failures.md). 

    - Hata türleri.
    - Hatanın kapsamı.
    - SLA'ları anlama.
- [İzleme ve telemetri](monitoring-and-telemetry.md). 

    - Neden hem telemetri uygulamayı satın alın ve gerekir, uygulamayı izlemek için kendi kodunuzu yazın.
    - Demo: Azure için yeni Relic
    - Demo: kod Düzelt uygulamada günlüğe kaydetme.
    - Demo: düzeltme uygulama bağımlılık ekleme.
    - Demo: azure'da yerleşik günlük desteği.
- [Geçici hata işleme](transient-fault-handling.md). 

    - Geçici hatalar etkisini azaltmak için akıllı yeniden deneme/geri alma mantığını kullanın.
    - Demo: yeniden deneme/geri alma Entity Framework 6.
- [Dağıtılmış önbelleğe alma](distributed-caching.md). 

    - Ölçeklenebilirliği geliştirmek ve dağıtılmış önbelleğe alma'yı kullanarak, veritabanı işlem maliyetleri azaltabilirsiniz.
- [Kuyruk merkezli çalışma deseni](queue-centric-work-pattern.md). 

    - Yüksek kullanılabilirliği etkinleştirme ve gevşek web ve çalışan katmanlarını eşleyerek ölçeklenebilirliği geliştirme.
    - Demo: Azure depolama kuyrukları Düzelt uygulama.
- [Daha fazla bulut uygulaması düzenler ve yönergeler](more-patterns-and-guidance.md).
- [Ek: Düzelt Örnek Uygulaması](the-fix-it-sample-application.md)

    - Bilinen Sorunlar
    - En İyi Yöntemler
    - İndirin, oluşturmak, çalıştırmak ve dağıtmak nasıl.

Bu desenler tüm bulut ortamları için geçerlidir, ancak biz bunları Microsoft teknolojileri ve Hizmetleri, Visual Studio, Team Foundation Service, ASP.NET ve Azure gibi temel örnekler kullanarak göstermek.

Bu bölümün geri kalanında bu Düzelt örnek uygulaması ve Web uygulamalarını düzeltme uygulama olarak çalışır ve Azure App Service bulut ortamında tanıtır.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Düzelt örnek uygulaması

Bu e-kitap gösterilen kod örnekleri ve ekran görüntüleri çoğunu başlangıçta tarafından geliştirilen Düzelt uygulama temel [Scott Guthrie](https://weblogs.asp.net/scottgu/) önerilen bulut uygulaması geliştirme desenleri ve uygulamaları göstermek için.

![Uygulama giriş sayfası Düzelt](introduction/_static/image1.png)

Çağrı oluşturma sistemi basit bir iş öğesi örnek uygulamadır. Sabit bir şey gerektiğinde, anahtar ve birisi ve diğerleri için oturum açın ve atanan bilet bkz Ata onlara oluşturur ve biletleri iş tamamlandığında tamamlandı olarak işaretleyebilirsiniz.

Standart bir Visual Studio web projesini var. ASP.NET MVC yerleşik olarak bulunur ve bir SQL Server veritabanı kullanır. Bu, IIS Express'te URL'i yerel olarak çalıştırabilirsiniz ve bulutta çalıştırmak için Azure Web sitesi için dağıtılabilir. Form kimlik doğrulaması ve yerel bir veritabanı kullanarak veya Google gibi sosyal sağlayıcılar kullanarak oturum açabilir. (Daha sonra da bir Active Directory kuruluş hesabınızla oturum açmanız nasıl göstereceğiz.)

![Sayfasında oturum açın](introduction/_static/image2.png)

Oturum açmadıysanız sonra bir bilet oluşturun, birine atayabilir ve sabit istediğiniz bir resim karşıya yükleyin.

![Düzelt görev oluşturma](introduction/_static/image3.png)

![Oluşturulan görev Düzelt](introduction/_static/image4.png)

İş öğeleri oluşturduğunuz ilerlemesini izlemek, biletleri, bilet ayrıntılarını görüntüleyin ve işareti öğeleri tamamlandı olarak atanan bakın.

Bu özellik açısından çok basit bir uygulaması, ancak milyonlarca kullanıcıya ölçeklenebilir ve esnek veritabanı hataları ve bağlantı sonlandırmalar gibi şeyler oluşturma konusunu ele alacağız. Ayrıca, başlayın ve uygulama hızlı ve verimli bir şekilde geliştirme döngüsü Yinelem yaparak daha iyi ve daha iyi yapmanıza olanak sağlayan bir otomatik ve Çevik Geliştirme iş akışının nasıl oluşturulacağını görürsünüz.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Azure App Service'te Web uygulamaları

Bunu düzeltmek için kullanılan uygulama bulut ortamı, Azure Web siteleri diyoruz bir hizmettir. Bu hizmet, VM'ler oluşturabilir ve bunları güncel tutmayı gerek kalmadan kendi web uygulamanızı azure'da barındırın, yükleyebilir ve yapılandırabilirsiniz IIS, vb., bir yoludur. Biz üzerinde sanal makinelerimiz sitenizi barındırmak ve yedekleme ve kurtarma ve diğer hizmetleri sizin için otomatik olarak sağlar. Web siteleri hizmeti, ASP.NET, Node.js, PHP ve Python ile çalışır. Visual Studio, Web dağıtımı, FTP, Git veya TFS kullanarak çok hızlı bir şekilde dağıtmanıza olanak sağlar. Genellikle, yalnızca birkaç saniye arasında bir dağıtımı vakit, güncelleştirme Internet üzerinden kullanıma hazır olur. Başlamak tüm ücretsizdir ve trafiğiniz büyüdükçe, ölçeği artırabilirsiniz.

Arka planda, Azure App Service'te Web uygulamaları birçok mimari bileşenler ve kendi Vm'lerinde IIS kullanarak bir web sitesi barındırmak için oluşturacağınız, kendiniz yapılandırmak zorunda özellikleri sağlar. Dağıtım uç noktası, otomatik olarak IIS'yi yapılandırır ve uygulamanızın sitenizi çalıştırmak istediğiniz sayıda sanal makineleri yükler bir bileşendir.

![Dağıtım Hizmeti](introduction/_static/image5.png)

Bir kullanıcının web sitesi ulaştığında, bunlar IIS sanal makinelerin doğrudan isabet yoksa, Git [uygulama isteği yönlendirme (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) yük Dengeleyiciler. Bunları kendi sunucularla kullanabilirsiniz, ancak bunlar sizin için otomatik olarak ayarladığınız avantajı burada olmasıdır. Oturum benzeşimi, IIS Yöneticisi'nde sıra derinliğini hesabı etkene içine alan akıllı bir buluşsal yöntem kullandıkları ve CPU kullanımı her makine trafiği vm'lere barındıran web sitenizi.

![ARR yük dengeleyici](introduction/_static/image6.png)

Bir makine kalırsa, Azure otomatik olarak döndürme çeker, yeni bir VM örneği oluşturan döner ve yeni örnek--uygulamanız için aşağı hiçbir zaman tüm trafiği yönlendiren başlatır.

![Makine hatalarına karşı Otomatik Kurtarma](introduction/_static/image7.png)

Tüm bu gerçekleşir otomatik olarak. Tek yapmak için ihtiyacınız olan bir web sitesi oluşturabileceğinizi ve Windows PowerShell, Visual Studio veya Azure Yönetim Portalı'nı kullanarak uygulamanızı ona dağıtın.

Visual Studio'da bir web uygulaması oluşturma ve bir Azure Web sitesine dağıtma gösteren hızlı ve kolay adım adım öğretici için bkz. [Azure ve ASP.NET kullanmaya başlama](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Özet

Bu giriş, kitap kapsar konuları, ekran görüntüleri örnek uygulamanın ve kısa bir genel bulut ortamında Azure App Service Web Apps listesini sağlamıştır. İçinde ve bulut için uygulamalar geliştirmeye harika avantajlarından biri, bir test ortamı oluşturmak ve kodunuzun dağıttıktan gibi yinelenen geliştirme görevlerini otomatik hale getirmek kolay olmasıdır. Diğer bir deyişle konusunun yapmak nasıl [sonraki bölümde](automate-everything.md).

## <a name="resources"></a>Kaynaklar

Bu bölümde ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

Belgeler:

- [Web uygulamaları Azure App Service'te](https://azure.microsoft.com/services/app-service/web/). Web Apps hakkında Azure belgeleri için portal sayfası.
- [Web Apps, Cloud Services ve Vm'leri: hangisi ne zaman kullanılmalıdır?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) Bu bölümde gösterildiği gibi WAWS yalnızca biridir üç yoldan web uygulamalarını Azure'da çalıştırabilirsiniz. Bu makalede, kullandığı üç yöntem arasındaki farklar açıklanır ve nasıl hangisinin senaryonuz için uygun olduğunu seçin yönelik rehberlik sağlar. Web siteleri gibi bulut Hizmetleri, Azure'nın bir PaaS özelliğidir. Vm'leri bir Iaas özelliğidir. PaaS ve Iaas hangi açıklaması için bkz: [veri seçenekleri](data-storage-options.md#paasiaas) bölüm.

Videolar:

- [Scott Guthrie, Azure Cloud OS nedir adım 0 - başlar?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Web siteleri mimarisi - Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Ben Nir Mashkowski ile Azure Web siteleri iç](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Next](automate-everything.md)
