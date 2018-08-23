---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
title: ASP.NET barındırma seçenekleri (VB) | Microsoft Docs
author: rick-anderson
description: ASP.NET web uygulamaları genellikle tasarlanmıştır, oluşturulan bir yerel geliştirme ortamında test ve üretim ortamına o dağıtılması gerekiyor...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 492f5ae2-bad7-4107-89a9-f04a9525dee7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
msc.type: authoredcontent
ms.openlocfilehash: 95df9c7595dc10f2bfce44845236d11457b8b3cb
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41752597"
---
<a name="aspnet-hosting-options-vb"></a>ASP.NET barındırma seçenekleri (VB)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF'yi indirin](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_vb.pdf)

> Genellikle ASP.NET web uygulamaları tasarlanmış, oluşturduğunuz ve test yerel geliştirme ortamı ve yayın için hazır olduğunda üretim ortamına dağıtılması gerekir. Bu öğreticide, dağıtım işlemi üst düzey bir genel bakış sağlar ve Bu öğretici serisinin bir giriş işlevi görür.


## <a name="introduction"></a>Giriş

Genellikle Web uygulamaları tasarlanmış, oluşturulan ve sitesinde çalışan programcılar için erişilebilir bir geliştirme ortamı test. Uygulama kullanıma hazır olduğunda burada site Internet üzerinde herkes tarafından erişilebilir bir üretim ortamına taşınır. Bu dağıtım işlemi bir dizi güçlük sunar:

- Bir üretim ortamında, mevcut olmalı ve bir ASP.NET uygulaması dağıtılmadan önce düzgün Kurulum olabilir; Ayrıca, üretim ortamına en son güvenlik düzeltme ekleriyle güncel tutulması gerekir.
- Biçimlendirme dosyaları, kod dosyaları ve Destek dosyalarının doğru kümesini geliştirme ortamından üretim ortamına kopyalanmalıdır. Veri odaklı uygulamalar için veritabanı şeması ve/veya verileri de kopyalanmasını gerektirebilir.
- Yapılandırma farkları iki ortam arasında olabilir. Geliştirme ortamında kullanılan veritabanı bağlantı dizesi veya e-posta sunucusu, büyük olasılıkla üretim ortamında farklı olacaktır. Bunun da ötesinde, uygulama davranışını ortama bağlı olabilir. Örneğin, geliştirme aşamasında bir hata oluştuğunda, hatanın ayrıntıları ekranda görüntülenebilir, ancak üretimde hata oluştuğunda, bunun yerine kolay bir hata sayfası görüntülenmelidir ve geliştiriciler için hata ayrıntılarını e-posta ile.

-Ayarlamaya ve bir üretim ortamının bakımını yapmak - ilk testten ortadan için üretim ortamlarını birçok kişi ve işletmelerin dış *web barındırma hizmeti sağlayıcıları*. Bir web barındırma sağlayıcısı üretim ortamına sizin adınıza yöneten bir şirkettir. Konak sağlayıcıları, her biri farklı fiyatlar ve hizmet düzeyleri sayısız web vardır; Böyle bir hizmet sağlayıcı bulma hakkında ipuçları için "Bulma bir Web ana bilgisayar sağlayıcısı" bölümüne bakın.

Bir ASP.NET web uygulamasını bir web ana bilgisayar sağlayıcısı tarafından yönetilen üretim ortamına dağıtımı içinde yer alan adımların bakmak öğretici serisinin ilk budur. Bu eğitim kursunda inceleyeceğiz:

- Hangi dosyaların web ana bilgisayar sağlayıcıya dağıtılması gerekir.
- Dağıtım işlemini kolaylaştırarak için Araçlar'ı tıklatın.
- Bir veritabanı dağıtma
- Kullanan bir veritabanı dağıtmak için ipuçları [SQL tabanlı üyelik ve rol sağlayıcısı](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), bir üretim ortamında Web sitesi yönetim aracı taklit edecek şekilde birlikte.
- Sorunsuz üretim veritabanında, geliştirme sırasında yapılan değişikliklerle güncelleştirme stratejileri.
- Üretim ve geliştiricilerin bir hata oluştuğunda bildirmek için yol günlük kaydı hataları teknikleri.

Bu öğreticileri, kısa ve görsel olarak işleminde size kılavuzluk için yeterince ekran görüntüleri ile adım adım yönergeler sağlamak için geliştirilmiş. Bülteninin açılış sayısına Bu öğretici, bir web barındırma sağlayıcısına bulma hakkında öneriler ve ASP.NET dağıtım sürecine genel bakış sağlar. Haydi başlayalım!

## <a name="an-overview-of-the-aspnet-deployment-process"></a>ASP.NET dağıtım sürecine genel bakış

Buna koysalar bir ASP.NET uygulaması dağıtma, aşağıdaki üç adımdan oluşur:

1. Üretim ortamında web uygulaması, web sunucusu ve veritabanı yapılandırın.
2. ASP.NET sayfaları, kod dosyaları, derlemelerde eşitleme `Bin` klasörünü ve HTML ile ilgili destek dosyaları gibi CSS ve JavaScript dosyaları.
3. Veritabanı şeması ve/veya verileri eşitleyin.

Bir web uygulaması için yapılandırma bilgilerini genellikle bulunan `Web.config` dosya ve veritabanı bağlantı dizeleri, hata işleme ölçütlerini URL yeniden yazma kullandık kuralları ve e-posta sunucusu bilgilerini içerir. Aktardığınızda genellikle bu bilgiler, uygulamayı üretimde aynı uygulama ve geliştirme farklıdır. Örneğin, bir uygulama geliştirirken üretim veritabanına karşı test ettiğiniz değil, böylece geliştirme veritabanı kullanmak en iyisidir. Sonuç olarak, veritabanı bağlantı dizeleri, geliştirme ve üretim uygulamalar arasında genellikle farklı. Bu farklar nedeniyle, web uygulamasının yapılandırma bilgileri için değişiklik yapmadan dağıtımının bir parçası içerir.

Web uygulaması yapılandırma değişiklikleri yanı sıra 1. adım Ayrıca web sunucusu ve veritabanı için yapılandırma gerektirdiği. ASP.NET sayfası oluşturur veya web sunucusunda bir dizinden dosya siler, örneğin, ardından web sunucusu bu dosya sistemi değişikliklerine izin verecek şekilde yapılandırılması gerekir. Benzer şekilde, veritabanı için yapılması gereken izni veya kimlik doğrulaması ayarlarını da olabilir.


2. adım, bir dizi temel ASP.NET sayfaları ve Destek dosyalarını geliştirme ile üretim ortamları arasında eşitleme içerir. ASP belirli kümesi. Visual Studio'da oluşturulan ve bir sonraki öğreticide tartışmadır proje türü iki ortam arasında eşitlenmesi gereken NET ilgili dosyalar bağlıdır  <em>[belirleme dosyaları gerekenlerdağıtılabiliriçin](determining-what-files-need-to-be-deployed-vb.md)</em>. Üçüncü ve dördüncü öğreticiler -  <em>[bilgisayarınızı Site kullanarak FTP dağıtımı](deploying-your-site-using-an-ftp-client-vb.md)</em>ve <em>[dağıtma bilgisayarınızı Site kullanarak Visual Studio](deploying-your-site-using-visual-studio-vb.md)</em> -inceleyin farklı araçları ve teknikleri bu dosyalar eşitleniyor.

Veri odaklı uygulamalar vardır kullanılan genellikle iki veritabanı oluştururken: biri geliştirme, diğeri üretim. Geliştirme sırasında geliştirme veritabanının şema yeni tablolar, sütunlar, saklı yordamlar ve tetikleyicilerle içerecek şekilde değiştirilebilir veya kaldırın veya var olan veritabanı nesnelerini yeniden adlandırmak için değiştirilebilir. Bu değişiklikler yapılır ve uygulama üretime dağıtıldığında çıkışınızda arasında geliştirme ve üretim veritabanları eşitlenmiş halde değil. Bu zaman uyumsuzluğu, dağıtım işlemi sırasında düzeltilmesi gerekiyor. Sonraki öğreticilerde bu zorluklar denenecektir.

## <a name="finding-a-web-host-provider"></a>Bir Web ana bilgisayar sağlayıcısı bulma

ASP.NET uygulamaları .NET Framework ve Internet Information Services (IIS) yüklü olan herhangi bir web sunucusuna dağıtılabilir. Bir site, Internet ve bilinen geniş bant bağlantısı yönlendiriciniz gelen web isteklerini izin verecek şekilde yapılandırmak nasıl olduğu varsayılarak kişisel bilgisayarınızdan barındırabilir. Birçok şirket gibi bir bilgisayara bir intranet siteye de barındırabilir. Odak noktası, şu öğreticilerden ancak web ana bilgisayar sağlayıcısı ile Web sitenizi barındırma sağlayıcılarıdır.

> [!NOTE]
> [IIS](https://www.iis.net/) Microsoft'un Kurumsal düzeyde web sunucusudur. Windows, Windows Server 2008 ve Windows Vista belirli sürümleri gibi giriş olmayan sürümleri ile birlikte gelir. Visual Studio ASP.NET Geliştirme Web sunucusu olarak IIS'yi bir geliştirme ortamında ASP.NET uygulamaları yüklemeniz gerekmez. Ancak, ASP.NET Geliştirme Web sunucusunu yalnızca yerel bağlantılar kabul eder ve bir üretim ortamında kullanılamaz.


Bir web ana bilgisayar sağlayıcısına sitenizi dağıtmadan önce iş yapmak için hangi şirket karar vermeniz gerekir. Sayısız web barındırma şirketleri Market'te vardır; "web barındırma şirketi" için arama beş milyondan fazla sonuçlarını döndürür. Size uygun olanı nasıl bulduğunuz? Web siteleri gibi gibi sık kullanılan arama motorunuz iyi bir başlangıç noktası olan [TopHosts](http://www.tophosts.com/) ve [HostCritique](http://www.hostcritique.net/), hangi karşılaştırın ve çeşitli barındırma hizmetleri. Ayrıca iş arkadaşlarınızla ve iş arkadaşlarınız için herhangi bir öneri isteyen öneri; önerileri için sorabilir [barındırma açık bir Forum](https://forums.asp.net/158.aspx) Burada, [ASP.NET forumları](https://forums.asp.net/).

Web barındırma şirketleri genellikle paylaşılan barındırma planlarını sunar ve ayrılmış barındırma planları. İle bir tek bir web sunucusu ana bilgisayarları yüzlerce değilse düzinelerce farklı Web sitelerinin paylaşılan barındırma. Ayrılmış barındırma, sitenizi ve sitenizi tek başına hizmet şirket bir bilgisayardan kira. Paylaşılan bir barındırma planı, ASP.NET sayfaları desteği için aylık 9.95 Microsoft Access veritabanlarına, 5 GB disk alanı ve 100 GB'lık aylık bant genişliği trafiğini çalışma olanağı içerebilir. ASP.NET sayfaları için destek Microsoft SQL Server 2008 veritabanı sunucusu, 10 GB disk alanı ve 250 GB'lık aylık bant genişliği trafiğini erişim 19.95 aylık için başka bir paylaşılan barındırma planı içerebilir. Barındırma planları ayrılmış genellikle çok daha pahalı ve maliyet birkaç yüz dolar aylık ve teklif daha iyi performans ve paylaşılan daha fazla denetim seçenekleri barındırır. Web sitenizi, bütçe seçtiğiniz hangi planı bağlıdır, ne kadar trafik alır ve, tahmin özelliklerini gerekir.

Bir web ana bilgisayar sağlayıcısı seçerken iki önemli müşteri hizmetleri ve hizmet kalitesi faktörlerdir. Bir soru veya bir yapılandırma sorunu varsa, ne kadar süreyle yanıt gelene kadar web ana bilgisayarın yardım masasının sorunu gönderme sürer? Şirketin hizmetleri nasıl güvenilir misiniz? Bunlar sık veritabanı kesintiler var mı? Kendi e-posta sunucusu ne sıklıkta çevrimdışı duruma? Bir şirketin kendi çalışma süresi hakkında bilgi sağlayan ve müşteri hizmet ilkelerinin hakkında sorgulamak için her zaman sor ancak daha surefire gidermenizi çevrimiçi forumlar, haber ve e-posta listservs yapabilirsiniz, güncel ve geçmiş müşteri geri bildirim isteği .

> [!NOTE]
> Bazı web barındırma şirketleri, .NET gibi belirli bir teknoloji yığınında işletmelerini odak veya [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **A** pache, **M** ySQL, ve **P** HP), bu nedenle seçtiğiniz şirket ASP.NET uygulamaları barındırdığından emin olun. Ayrıca, uygulamanızı oluşturmak için kullandığınız ASP.NET sürümünü destekledikleri emin olun. Ve veri odaklı bir uygulama oluşturuyorsanız web ana bilgisayarı aynı veritabanı sunucusu ve kullandığınız sürüm sunduğundan emin olun.


## <a name="summary"></a>Özet

ASP.NET web uygulamaları genellikle tasarlanmış, oluşturulur ve yerel geliştirme ortamında test edildi. Bir sürümü yayımlanmaya hazır hale geldikten sonra bir üretim ortamına taşınır. Konak ASP.NET Web siteleri, kişisel bilgisayar veya şirket içindeki sunucularda mümkün olsa da, kendi web ana bilgisayar sağlayıcıya barındırma dış kaynak sağlamak birçok işletme ve kişiler seçin.

Bu öğretici serisinde, sık karşılaşılan zorluklar keşfetmeye bir web ana bilgisayar sağlayıcısı bir ASP.NET uygulaması dağıtma adımları inceler. Bu öğretici, ASP.NET dağıtım işlemi üst düzey bir genel bakış sunulur ve uygun web ana bilgisayar sağlayıcısı bulmak için ipuçları verdi. ASP.NET ile ilgili dosyaları sitenizi dağıtırken üretim ortamına kopyalanacak gerekenler sonraki öğreticiye arar.

Mutlu programlama!

### <a name="special-thanks-to"></a>Özel performanstan...

Bu öğretici serisinde, birçok yararlı Gözden Geçiren tarafından gözden geçirildi. Bu öğretici için müşteri adayı İnceleme Teresa Murphy oluştu. Yaklaşan My MSDN makaleleri gözden geçirme ilgileniyor musunuz? Bu durumda, bir satır bana bırak [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Önceki](users-and-roles-on-the-production-website-cs.md)
> [İleri](determining-what-files-need-to-be-deployed-vb.md)
