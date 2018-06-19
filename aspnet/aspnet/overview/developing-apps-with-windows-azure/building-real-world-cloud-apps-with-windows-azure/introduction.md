---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Azure ile gerçek bulut uygulamaları derleme | Microsoft Docs
author: MikeWasson
description: Bu e-kitap desenleri dayalı bir yaklaşım ile gerçek bulut çözümleri oluşturmak için anlatılmaktadır. Geliştirme sürecinin olarak da desenleri uygulamak bir...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 5a62818a2dc21128bb0a42a8b296ade460e7b060
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870537"
---
<a name="building-real-world-cloud-apps-with-azure"></a>Azure ile gerçek bulut uygulamaları derleme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Bu e-kitap desenleri dayalı bir yaklaşım ile gerçek bulut çözümleri oluşturmak için anlatılmaktadır. Desenler geliştirme işlemi için Mimari ve kodlama uygulamalarını yanı sıra uygulanır.
> 
> İçerik, Scott Guthrie tarafından geliştirilen ve ona göre Norveççe geliştiriciler konferans (NDC) konumunda, Haziran 2013'te teslim sunu dayalı ([Kısım 1](http://vimeo.com/68215538), [Kısım 2](http://vimeo.com/68215602)) ve Microsoft Teknik Ed Avustralya'da Eylül 2013 ([Kısım 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [Kısım 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Diğer birçok](more-patterns-and-guidance.md#acknowledgments) güncelleştirilmiş ve bu video yazılı forma geçiş sırasında içeriğin genişletilebilir.


## <a name="intended-audience"></a>Hedef kitle

Bulut için geliştirme hakkında merak buluta taşıma dikkate ya da geliştirme bulut yeni geliştiriciler en önemli kavramlar ve bilmeniz gereken uygulamalar kısa bir genel bakış burada bulabilirsiniz. Kavramları somut örnekler ve diğer kaynakların daha ayrıntılı bilgiler için her bölüm bağlantılarını gösterilmiştir. Örnekler ve ek kaynaklara bağlantılar Microsoft çerçeveler ve hizmetler için ancak gösterilen ilkeleri diğer web geliştirme çerçeveleri uygulamak ve ortamlar da bulut.

Zaten bulut için geliştirme geliştiriciler yardımcı olacak fikirlerini burada onları daha başarılı olmak bulabilirsiniz. Her bölüm serideki bağımsız olarak, çekme ve ilgilendiğiniz konuları seçin okuyabilir.

Scott Guthrie'nın izlenen herkes *yapı gerçek dünya bulut uygulamalarını Azure ile* sunu ve daha fazla ayrıntı ve güncelleştirilmiş bilgileri bulacaksınız, burada istediği.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Bulut geliştirme desenleri

Bu e-kitap On üç desenleri bulut geliştirme için önerilen açıklanmaktadır. "Düzeni" Buraya geniş anlamda şeyler için önerilen yol anlamına için kullanılır: en iyi nasıl geliştirirken, tasarlama ve bulut uygulamalarını kodlama hakkında gidin. Bunlar, bunları izlerseniz, "başarısı pit içine sonbaharda" yardımcı olacak anahtar desenleri alır.

- [Her şeyi otomatikleştirmek](automate-everything.md).

    - Verimliliği en üst düzeye çıkarmak ve tekrarlayan işlemleri hatalar en aza indirmek için komut dosyalarını kullanın.
    - Gösteri: Azure yönetim komut dosyaları.
- [Kaynak denetimi](source-control.md). 

    - DevOps iş akışını kolaylaştırmak için kaynak denetimi dallanma yapısında ayarlayın.
    - Gösteri: kaynak denetimi için komut dosyalarını ekleyin.
    - Gösteri: hassas verileri kaynak denetimi dışında tutun.
    - Gösteri: Visual Studio'da Git kullanın.
- [Sürekli tümleştirme ve teslimat](continuous-integration-and-continuous-delivery.md). 

    - Derleme ve dağıtım ile her kaynak denetimine iade otomatikleştirin.
- [Web geliştirme en iyi yöntemler](web-development-best-practices.md). 

    - Web Katmanı durum bilgisiz tutun.
    - Gösteri: ölçekleme ve Azure App Service'te Web uygulamalarını, otomatik ölçeklendirme.
    - Oturum durumu kaçının.
    - CDN kullanılamıyorsa bir CDN geri dönüş ile kullanın.
    - Zaman uyumsuz programlama modeli kullanır.
    - Demo: ASP.NET MVC ve Entity Framework zaman uyumsuz.
- [Çoklu oturum açma](single-sign-on.md). 

    - Azure Active Directory'ye giriş.
    - Gösteri: Azure Active Directory kullanan bir ASP.NET uygulaması oluşturursunuz.
- [Veri Depolama Seçenekleri](data-storage-options.md). 

    - Veri depoları türleri.
    - Sağdaki veri deposunu seçmenizi nasıl.
    - Demo: Azure SQL veritabanı.
- [Stratejileri bölümleme veri](data-partitioning-strategies.md). 

    - Veri bölümü dikey, yatay olarak veya bir ilişkisel veritabanı ölçekleme kolaylaştırmak için her ikisini de.
- [Yapılandırılmamış blob depolama](unstructured-blob-storage.md). 

    - Dosyaları blob hizmeti kullanarak bulutta depolayın.
    - Gösteri: Düzelt uygulamada blob storage kullanarak.
- [Hatalarına karşı korur tasarım](design-to-survive-failures.md). 

    - Hataları türleri.
    - Hata kapsamı.
    - SLA'ları anlama.
- [İzleme ve telemetri](monitoring-and-telemetry.md). 

    - Neden hem telemetri uygulama satın alın ve gerekir, uygulamayı izlemek için kendi kodunuzu yazın.
    - Demo: Azure için New Relic
    - Gösteri: kod Düzelt uygulamada oturum.
    - Demo: Düzelt uygulamasında bağımlılık ekleme.
    - Tanıtım: Azure yerleşik günlük desteği.
- [Geçici hata işleme](transient-fault-handling.md). 

    - Geçici hataları etkisini azaltmak için Yeniden Dene'yi/geri alma akıllı mantığı kullanın.
    - Gösteri: yeniden deneme/geri alma Entity Framework 6.
- [Önbelleğe alma dağıtılmış](distributed-caching.md). 

    - Ölçeklenebilirliği artırmak ve dağıtılmış önbelleğe alma kullanarak veritabanı işlem maliyetleri azaltır.
- [Kuyruk merkezli çalışma deseni](queue-centric-work-pattern.md). 

    - Yüksek kullanılabilirliği etkinleştirme ve geniş web ve çalışan katmanları eşleyerek ölçeklenebilirlik geliştirebilirsiniz.
    - Demo: Düzelt uygulamasında Azure depolama sıralar.
- [Daha fazla bulut uygulaması desenleri ve kılavuz](more-patterns-and-guidance.md).
- [Ek: Düzelt Örnek Uygulaması](the-fix-it-sample-application.md)

    - Bilinen Sorunlar
    - En İyi Yöntemler
    - Karşıdan yükle, oluşturmak, çalıştırmak ve dağıtmak nasıl.

Bu düzenleri tüm bulut ortamları için geçerli ancak biz bunları Microsoft teknolojilerini ve Visual Studio, Team Foundation Service, ASP.NET ve Azure gibi hizmetler göre örnekleri kullanarak göstermeye.

Bu bölümde bu kalanı Düzelt örnek uygulaması ve Web uygulamaları Düzelt uygulama çalışan Azure App Service bulut ortamınızdaki tanıtır.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>Bu örnek uygulama düzeltme

Ekran görüntüleri ve kod örnekleri bu e-kitap gösterilen çoğunu başlangıçta tarafından geliştirilen Düzelt uygulama dayanır [Scott Guthrie](https://weblogs.asp.net/scottgu/) önerilen bulut uygulama geliştirme modelleri ve yöntemleri göstermek için.

![Uygulama giriş sayfası Düzelt](introduction/_static/image1.png)

Örnek uygulaması sistemimiz basit iş bir öğedir. Sabit bir şey gerektiğinde, anahtar ve birisi ve diğerleri için oturum açın ve atanan biletleri bkz Ata onlara oluşturun ve biletleri iş tamamlandığında tamamlanan olarak işaretle.

Standart bir Visual Studio web projesi değil. ASP.NET MVC tabanlıdır ve SQL Server veritabanını kullanır. IIS Express'te yerel olarak çalıştırabilirsiniz ve bir Azure Web bulutta çalıştırmak için siteye dağıtılabilir. Form kimlik doğrulaması ve yerel bir veritabanı kullanarak veya Google gibi sosyal sağlayıcıyı kullanarak oturum açabilir. (Daha sonra da bir Active Directory kuruluş hesabı ile oturum açmanız nasıl göstereceğiz.)

![Sayfasında oturum açın](introduction/_static/image2.png)

Oturum açtınız sonra bir anahtar oluşturun, birine atayın ve sabit istediğiniz bir resim karşıya yükleyin.

![Düzelt görev oluşturma](introduction/_static/image3.png)

![Oluşturulan görev Düzelt](introduction/_static/image4.png)

İş öğeleri oluşturduğunuz ilerlemesini izlemek, size, bilet ayrıntıları görüntüle ve işareti öğeleri için tamamlandı olarak atanmış bir bilet bakın.

Bu özellik açısından çok basit bir uygulaması, ancak milyonlarca kullanıcıya ölçeklenebilir ve esnek veritabanı hataları ve bağlantı sonlandırmalar gibi şeyler böylece oluşturma görürsünüz. Ayrıca, basit başlatma ve hızlı ve verimli bir şekilde geliştirme döngüsü yineleme uygulama daha iyi ve daha iyi yapmanıza olanak sağlayan bir otomatik ve Çevik Geliştirme iş akışının nasıl oluşturulacağını görürsünüz.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Azure App Service'te Web uygulamalarını

Bunu düzeltmek için kullanılan uygulama bulut ortamı, Web siteleri diyoruz Azure hizmetidir. Bu hizmet, VM'ler oluşturmak ve bunları güncel tutmak zorunda kalmadan kendi web uygulamanızı azure'da barındırmak, yükleyebilir ve yapılandırabilirsiniz, vb. IIS, bir yoludur. Biz, sitenizi bizim Vm'lerinde barındırabilir ve yedekleme ve kurtarma ve diğer hizmetleri sizin için otomatik olarak sağlayın. Web siteleri hizmeti, ASP.NET, Node.js, PHP ve Python ile çalışır. Çok hızlı bir şekilde Visual Studio, Web dağıtımı, FTP, Git veya TFS dağıtmanızı sağlar. Genellikle, yalnızca birkaç saniye arasında bir dağıtım başlangıç zamanı ve güncelleştirmenizi Internet üzerinden kullanılabilir olduğu zaman olur. Başlamak tüm ücretsizdir ve trafiğiniz büyüdükçe, ölçeği artırabilirsiniz.

Arka planda, Azure App Service'te Web uygulamalarını çok mimari bileşenleri ve kendi Vm'lerinde IIS kullanarak bir web sitesini barındırmak için olmaz kendiniz yapılandırmak zorunda özellikleri sağlar. Otomatik olarak IIS yapılandırır ve sitenizi çalıştırmak istediğiniz sayıda sanal makineleri üzerinde uygulamanızı yükleyen bir dağıtım uç noktası bir bileşendir.

![Dağıtım Hizmeti](introduction/_static/image5.png)

Kullanıcı web sitesine geldiğinde bunların IIS VM'ler doğrudan isabet yok, bunlar geçtikleri [uygulama isteği yönlendirme (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) yük dengeleyici. Bunları kendi sunucuları kullanabilirsiniz, ancak bunlar sizin için otomatik olarak ayarlamış olduğunu avantajı burada olmasıdır. Oturum benzeşimi, IIS Yöneticisi'nde sıra derinliği gibi hesap faktörlere alır akıllı bir buluşsal yöntem kullanabilir ve CPU kullanımı her makine VM'ler için doğrudan trafiğe barındıran web sitenizi.

![ARR yük dengeleyici](introduction/_static/image6.png)

Bir makine kullanılamaz hale gelirse Azure otomatik olarak döndürme çeker, yeni bir VM örneğini döner ve yeni örnek--tüm uygulamanız için aşağı hiçbir süresiyle trafiği yönlendirerek başlatır.

![Makine hatadan otomatik kurtarma](introduction/_static/image7.png)

Tüm bunlar kurulur otomatik olarak. Yapmanız gereken tek şey Windows PowerShell, Visual Studio veya Azure Yönetim Portalı'nı kullanarak, uygulama, bir web sitesi oluşturun ve dağıtın.

Visual Studio'da bir web uygulaması oluşturmak ve bir Azure Web sitesine dağıtmak nasıl oluşturulduğunu gösteren bir hızlı ve kolay adım adım öğretici için bkz [Azure ve ASP.NET kullanmaya başlama](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Özet

Bu giriş, kitap ele alınacaktır konuları, örnek uygulama ekran görüntüleri ve Azure App Service bulut ortamınızdaki Web Apps kısa bir genel bakış listesini sağlamıştır. Uygulama içinde ve bulut için geliştirme harika avantajları bir test ortamı oluşturma ve kodunuzu dağıtmayı gibi yinelenen geliştirme görevleri otomatikleştirmek kolaydır biridir. Diğer bir deyişle konusunun nasıl [sonraki bölümde](automate-everything.md).

## <a name="resources"></a>Kaynaklar

Bu bölümde ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

Belgeler:

- [Web uygulamaları Azure uygulama hizmetinde](https://azure.microsoft.com/services/app-service/web/). Web uygulamaları Azure belgelerine için portal sayfası.
- [Web uygulamaları, bulut Hizmetleri ve sanal makineleri: ne zaman hangi kullanılır?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) Bu bölümde gösterildiği gibi WAWS biridir yalnızca üç şekilde Azure web uygulamaları çalıştırabilirsiniz. Bu makalede, üç şekilde arasındaki farklar açıklanmaktadır ve hangisinin senaryonuz için uygun olduğunu seçin konusunda rehberlik sağlar. Web siteleri gibi bulut Hizmetleri, Azure, bir PaaS özelliğidir. Sanal makineleri bir Iaas özelliğidir. Iaas ve PaaS açıklaması için bkz: [veri seçenekleri](data-storage-options.md#paasiaas) bölüm.

Videolar:

- [Azure bulut OS nedir adım 0 - Scott Guthrie başlatılır?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Web siteleri mimarisiyle - Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Azure Web siteleri iç Nir Mashkowski ile](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Next](automate-everything.md)
