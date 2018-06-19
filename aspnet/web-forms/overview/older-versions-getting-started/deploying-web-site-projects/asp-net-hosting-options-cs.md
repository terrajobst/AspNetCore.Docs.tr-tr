---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: ASP.NET seçenekleri (C#) barındırma | Microsoft Docs
author: rick-anderson
description: ASP.NET web uygulamaları genellikle tasarlanmıştır, oluşturulan bir yerel geliştirme ortamında test ve bir üretim ortamında o dağıtılması gerekiyor...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 6f8bb0e5a34d84d448af56285e8761c447229f7d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888516"
---
<a name="aspnet-hosting-options-c"></a>ASP.NET barındırma seçenekleri (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF indirin](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> Genellikle ASP.NET web uygulamaları tasarlanmış, oluşturulan ve bir yerel geliştirme ortamı ve sürüm için hazır olduktan sonra üretim ortamına dağıtılması gerek test. Bu öğretici, dağıtım işlemi üst düzey bir genel bakış sağlar ve Bu öğretici seri giriş görevi görür.


## <a name="introduction"></a>Giriş

Genellikle Web uygulamaları tasarlanmış, oluşturulan ve yalnızca sitesinde çalışan programcıları için erişilebilir olan bir geliştirme ortamında test. Uygulama kullanıma hazır olduktan sonra burada site Internet üzerinden herkes tarafından erişilebilecek bir üretim ortamına taşınır. Bu dağıtım işlemi belirli zorluklar sunar:

- Bir üretim ortamında var ve bir ASP.NET uygulaması dağıtılmadan önce düzgün bir şekilde Kurulum olması gerekir; Ayrıca, üretim ortamında en son güvenlik düzeltme eklerini güncel tutulması gerekir.
- Biçimlendirme dosyaları, kod dosyaları ve Destek dosyalarını doğru kümesini geliştirme ortamından üretim ortamına kopyalanmalıdır. Veri tabanlı uygulamalar için bu veritabanı şema ve/veya yanısıra veri kopyalama gerektirebilir.
- Yapılandırma farkları iki ortam arasında olabilir. Geliştirme ortamında kullanılan veritabanı bağlantı dizesi veya e-posta sunucusu olasılıkla oranla üretim ortamına farklı olacaktır. Daha uygulamanın davranışını ortamda bağlı olabilir. Örneğin, geliştirme bir hata oluştuğunda hatanın ayrıntıları ekranda görüntülenebilir, ancak üretim aşamasında hata oluştuğunda, kullanımı kolay hata sayfası yerine görüntülenmesi gerekir ve geliştiricileri için hata ayrıntılarını e-posta ile.

-Ayarlama ve bir üretim ortamı bulundurmak - ilk testten obviate için üretim ortamlarını birçok kişiler ve işletmeler dış *barındırma hizmeti sağlayıcıları web*. Bir web barındırma sağlayıcısına üretim ortamına sizin adınıza yöneten bir şirkettir. Her değişken fiyatları ve hizmet düzeyleri ile ana bilgisayar sağlayıcıları sayısız web vardır; Bu tür bir hizmet sağlayıcısı bulma hakkında ipuçları için "Bulma bir Web ana makine sağlayıcısı" bölümüne bakın.

ASP.NET web uygulaması web ana bilgisayar sağlayıcısı tarafından yönetilen bir üretim ortamına dağıtma adımları bakmak eğitim serileri ilk budur. Bu öğreticiler süresince inceleyeceğiz:

- Hangi dosyaların web ana bilgisayar sağlayıcıya dağıtılması gerekiyor.
- Dağıtım işlemi hızlandırma için Araçlar'ı tıklatın.
- Bir veritabanını dağıtmak nasıl.
- Kullanan bir veritabanı dağıtmak için ipuçları [SQL tabanlı üyelik ve roller sağlayıcısı](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), bir üretim ortamında Web sitesi yönetim aracı taklit edecek şekilde yanı sıra.
- Sorunsuz geliştirme sırasında yapılan değişikliklerle üretim veritabanında güncelleştirme stratejileri.
- Üretim ve bir hata oluştuğunda geliştiriciler bildirmek için yollar günlük hataları teknikler.

Bu öğreticileri, kısa ve görsel olarak işleminde size kılavuzluk için yeterince ekran görüntüleri ile adım adım yönergeler sağlamak için sağlamıştır. Bülteninin açılış sayısına Bu öğretici, bir web barındırma sağlayıcısına bulma ASP.NET dağıtım işlemi ve öneriler genel bakış sağlar. Haydi başlayalım!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>ASP.NET dağıtım işlemine genel bakış

Buna koysalar bir ASP.NET uygulaması dağıtma aşağıdaki üç adımdan oluşur:

1. Web uygulaması, web sunucusu ve veritabanı üretim ortamında yapılandırın.
2. ASP.NET sayfaları, kod dosyaları, derlemelerde eşitleme `Bin` klasörü ve HTML ile ilgili destek dosyalarını CSS ve JavaScript dosyaları gibi.
3. Veritabanı şema ve/veya veri eşitleyin.

Bir web uygulaması için yapılandırma bilgilerini genellikle bulunan `Web.config` dosya ve veritabanı bağlantı dizeleri, hata işleme ölçütlerini URL yeniden yazma kuralları ve e-posta sunucusu bilgilerini içerir. Görmemeleri bu bilgileri, bir uygulama geliştirme üretim aynı uygulamada karşı farklıdır. Örneğin, bir uygulama geliştirirken üretim veritabanına karşı test ettiğiniz değil böylece bir geliştirme veritabanını kullanmak en iyisidir. Sonuç olarak, veritabanı bağlantı dizelerini genellikle geliştirme ve üretim uygulamalar arasında farklılık gösterir. Bu farklılıklar nedeniyle, web uygulamasının yapılandırma bilgileri için değişiklik yapmadan dağıtımının bir parçası içerir.

Web uygulaması yapılandırma değişiklikleri ek olarak, 1. adım Ayrıca web sunucusu ve veritabanı için yapılandırma gerektirdiği. Bir ASP.NET sayfasının oluşturur veya web sunucusunda bir dizinin dosyaları siler, örneğin, ardından web sunucusu bu dosya sistemi değişikliklerine izin verecek şekilde yapılandırılması gerekir. Benzer şekilde, veritabanına yapılması gereken izni veya kimlik doğrulama ayarları olabilir.


2. adım, temel ASP.NET sayfaları ve Destek dosyalarını geliştirme ve üretim ortamlarını arasında kümesini eşitleme içerir. ASP belirli kümesi. Visual Studio'da oluşturulur ve sonraki öğretici tartışmada olan proje türü bağımlı iki ortamlar arasında eşitlenmesi gereken NET ilgili dosyaları [ *belirleme dosyaları gerekenlerdağıtılacağınıiçin*](determining-what-files-need-to-be-deployed-cs.md). Üçüncü ve dördüncü öğreticileri - [ *bilgisayarınızı Site kullanarak FTP dağıtımı* ](deploying-your-site-using-an-ftp-client-cs.md) ve [ *dağıtma bilgisayarınızı Site kullanarak Visual Studio* ](deploying-your-site-using-visual-studio-cs.md) -inceleyin farklı araçlar ve bu dosyaları eşitlenmesi teknikler.

Var olan veri tabanlı uygulamalar genellikle iki veritabanı kullanılan oluştururken: geliştirme, diğeri üretim için. Geliştirme sırasında geliştirme veritabanının şema yeni tablolar, sütunlar, saklı yordamları ve Tetikleyicileri eklemek için değiştirilebilir veya kaldırmak veya var olan veritabanı nesnelerini yeniden adlandırmak için değiştirilebilir. Bu değişiklikler yapıldıktan zaman ve uygulama üretime dağıtılır saat arasında geliştirme ve üretim veritabanları eşitlenmemiş. Bu asynchrony dağıtım işlemi sırasında düzeltilmesi gerekiyor. Bu zorluklar gelecekteki eğitimlerine denenecektir.

## <a name="finding-a-web-host-provider"></a>Bir Web ana makine sağlayıcısı bulma

ASP.NET uygulamalarının Internet Information Services (IIS) ve .NET Framework sahip herhangi bir web sunucusuna dağıtılabilir. Internet ve bilinen geniş bant bağlantısı yönlendiricinizi gelen web isteklerini izin verecek şekilde yapılandırmak nasıl sahip olduğu varsayılarak kişisel bilgisayarınızdan bir site barındırabilir. Birçok şirket gibi bir bilgisayar, intranet sitesinden de barındırabilir. Bu öğreticileri odak ancak, bir web ana makine sağlayıcısı ile Web sitenizi barındıran.

> [!NOTE]
> [IIS](https://www.iis.net/) Microsoft'un Kurumsal düzeyde bir web sunucusudur. Windows, Windows Server 2008 ve Windows Vista'nın belirli sürümleri gibi giriş olmayan sürümleri ile birlikte gelir. Visual Studio ASP.NET Geliştirme Web sunucusu içerir gibi bir geliştirme ortamında, ASP.NET uygulamalarını sunmak için IIS yüklemeniz gerekmez. Ancak, ASP.NET Geliştirme Web sunucusu yalnızca yerel bağlantıları kabul eder ve bir üretim ortamında bu nedenle kullanılamaz.


Bir web ana bilgisayar sağlayıcısına sitenizi dağıtmadan önce hangi şirket birlikte iş yaptığınız için karar vermeniz gerekir. Sayısız web barındırma şirketleri Market'te vardır; "web şirket barındırma" için arama beş milyondan fazla sonuçlar döndürür. Nasıl sizin için uygun olan bir bulurum? Web siteleri gibi gibi sık kullanılan arama motorunuz iyi bir başlangıç noktası olan [TopHosts](http://www.tophosts.com/) ve [HostCritique](http://www.hostcritique.net/), karşılaştırır ve çeşitli barındırma hizmetleri karşılaştırın. Ayrıca iş arkadaşlarınızı ve iş arkadaşlarınızla için herhangi bir önerimiz isteyen bildirmek; önerisi için sorabilirsiniz [barındırma açık bir Forum](https://forums.asp.net/158.aspx) Burada, [ASP.NET forumları](https://forums.asp.net/).

Web barındırma şirketinin genellikle paylaşılan barındırma planları sunar ve barındırma planları ayrılmış. İle bir tek bir web sunucusunun barındırdığı yüzlerce değilse düzinelerce farklı Web sitelerinin paylaşılan barındırma. Ayrılmış barındırma ile sitenizi ve sitenizi tek başına hizmet şirket bilgisayardan kira. Paylaşılan bir barındırma planı ASP.NET sayfaları için destek $ ay 9,95 için Microsoft Access veritabanları, 5 GB disk alanı ve aylık bant genişliği trafik 100 GB ile çalışma olanağını içerebilir. Başka bir paylaşılan barındırma planı ASP.NET sayfaları için destek 19.95 her ay için Microsoft SQL Server 2008 veritabanı sunucusu, 10 GB disk alanı ve aylık bant genişliği trafik 250 GB erişmeye içerebilir. Barındırma planları ayrılmış genellikle çok daha pahalı, maliyetlendirme birkaç yüz ABD Doları her ay, ancak daha iyi performans sunar ve paylaşılan daha fazla denetim seçeneklerini barındırır. Seçtiğiniz hangi planı bütçenizi üzerinde bağlıdır, ne kadar trafik Web sitenizi alır ve, düşündüğünüz özellikleri gerekir.

Bir web ana makine sağlayıcısı seçerken iki önemli müşteri hizmetleri ve hizmet kalitesi faktörlerdir. Bir soru veya yapılandırma sorunu varsa, ne kadar süreyle yanıt elde edene kadar web ana bilgisayarın yardım masasının sorunu gönderme sürer? Şirketin hizmetler nasıl güvenilir var mı? Sık veritabanı kesintileri var mı? Kullanıcıların e-posta sunucusu ne sıklıkta çevrimdışı duruma? Her zaman kendi çalışma süresi hakkında ayrıntılı bilgi sunar ve bunların müşteri hizmet ilkesi hakkında bilgi almak için bir şirket isteyebilir, ancak bir daha surefire çevrimiçi forumları, haber grupları ve e-posta listservs yapmak için geçerli ve geçmiş müşteri geri bildirim istemek için yoludur .

> [!NOTE]
> Bazı web barındırma şirketinin .NET gibi bir belirli teknoloji yığını işletmelerini odaklanmak veya [AMPUL](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **A** pache, **M** ySQL, ve **P** HP), bu nedenle seçtiğiniz şirket ASP.NET uygulamaları barındıran emin olun. Ayrıca, uygulamanızı oluşturmak için kullandığınız ASP.NET sürümünü destekleyen emin olun. Ve veri tabanlı bir uygulama geliştiriyorsanız, web ana bilgisayarı aynı veritabanı sunucusu ve kullandığınız sürüm sunar emin olun.


## <a name="summary"></a>Özet

Genellikle ASP.NET web uygulamaları tasarlanmış, oluşturulan ve bir yerel geliştirme ortamında test. Bir sürüm sürüm için hazır olduktan sonra bir üretim ortamına taşınır. Ana bilgisayar ASP.NET Web siteleri, kişisel bilgisayar veya şirketiniz içindeki sunucularda mümkün olsa da, kullanıcıların bir web ana bilgisayar sağlayıcısına barındırma dış kaynak sağlamak birçok işletme ve kişiler seçin.

Bu öğretici seri sık karşılaşılan zorluklar keşfetme web ana makine sağlayıcısı, bir ASP.NET uygulamasını dağıtma adımları inceler. Bu öğretici ASP.NET dağıtım işlemi üst düzey bir genel bakış sunulan ve uygun web ana makine sağlayıcısı bulmak için ipuçları vermiş oldunuz. Sonraki öğretici ASP.NET ile ilgili dosyalar Web sitenizi dağıtırken üretim ortamına kopyalanacak gerekenler adresindeki arar.

Mutluluk programlama!

### <a name="special-thanks-to"></a>Özel teşekkürler...

Bu öğretici seri pek çok yararlı gözden geçirenler tarafından gözden geçirildi. Bu öğretici için sağlama İnceleme Teresa Murphy oluştu. My yaklaşan MSDN makaleleri gözden geçirme ilginizi çekiyor mu? Öyleyse, bana bir satırında bırakma [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Next](determining-what-files-need-to-be-deployed-cs.md)
