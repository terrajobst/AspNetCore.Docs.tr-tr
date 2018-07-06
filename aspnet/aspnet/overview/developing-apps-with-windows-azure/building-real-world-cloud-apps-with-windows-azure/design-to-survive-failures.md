---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: Tasarım (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) hatalara karşı | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 29a430a223fb62d9530ea00a60d458a6dbc5199a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820661"
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>(Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) hatalara karşı tasarlama
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. E-kitabı hakkında daha fazla bilgi için bkz. [ilk bölüm](introduction.md).


Uygulama, ancak özellikle de burada birçok kişi kullanacağınız, bulutta çalışacak herhangi bir türde oluşturduğunuzda hakkında düşünmek zorunda şeylerden biridir, düzgün bir şekilde işlemek hataları ve değer sunmak devam şekilde uygulama tasarlayın nasıl kadar olası. Yeterli zaman göz önünde bulundurulduğunda, öğeleri herhangi bir ortam veya herhangi bir yazılım sisteminin yanlış oluşturacaksınız. Müşterilerinize nasıl rahatsız etmeyi hedefleyen alırsınız ve ne kadar süre belirler, uygulamanız bu durumları nasıl işlediğini sorunlarını çözümlemek ve harcama gerekir.

## <a name="types-of-failures"></a>Hata türleri

Farklı şekilde işlemek isteyebilirsiniz hataları temel iki kategorisi vardır:

- Geçici, aralıklı ağ bağlantısı sorunları gibi hataları kendi kendine iyileştirme.
- Müdahale gerektiren enduring hataları.

Geçici hatalar için çoğu zaman hızla ve otomatik olarak uygulama kurtarır emin olmak için bir yeniden deneme ilkesi uygulayabilir. Müşterilerinizin biraz daha uzun yanıt süresi fark edebilirsiniz, ancak Aksi takdirde bunlar etkilenmez. Bu hataları işlemek için bazı yollar göstereceğiz [geçici hata işleme bölüm](transient-fault-handling.md).

Hataları enduring için izleme ve günlüğe kaydetme, kök neden analizi kolaylaştırır ve sorunlar ortaya olduğunda en kısa sürede bildirir işlevi uygulayabilirsiniz. Bu tür hatalara haberdar olun yardımcı olmak için bazı yollar göstereceğiz [izleme ve Telemetri bölüm](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Hatanın kapsamı

Tek bir makine etkileyip etkilemediğini, hata kapsamı hakkında – düşünmek'iniz de bölgenin tamamını SQL veritabanı veya depolama gibi bir hizmet.

![Hatanın kapsamı](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Makine hataları

Azure'da, başarısız sunucu tarafından yeni bir otomatik olarak değiştirilir ve iyi tasarlanmış bir bulut uygulaması hızla ve otomatik olarak bu tür bir hata kurtarır. Daha önce biz bir durum bilgisi olmayan web katmanı ölçeklenebilirlik avantajlarını denebilecek ve başarısız bir sunucudan kurtarma kolaylığı statelessness, başka bir avantajdır. Kurtarma kolaylığı Ayrıca, SQL veritabanı ve Azure App Service Web Apps gibi hizmet olarak platform (PaaS) özellikleri avantajlarından biridir. Donanım hatalarının nadirdir, ancak bu hizmetler otomatik olarak bunları işler ne zaman ortaya; bile bu hizmetlerden biri kullanırken makine hatalarını işlemek için kod yazmanız gerekmez.

### <a name="service-failures"></a>Hizmet hataları

Bulut uygulamaları, genellikle birden çok hizmeti kullanın. Örneğin, SQL veritabanı hizmeti, depolama hizmeti, düzeltme uygulama kullanır ve web uygulamasını Azure App Service'e dağıtılır. Bağımlı hizmetlerden biri başarısız olursa, uygulamanız ne? Bazı hataları kolay bir hizmet için "Üzgünüz, daha sonra yeniden deneyin" iletisi yapabileceğiniz en iyi olabilir. Ancak çoğu senaryoda, daha iyi yapabilirsiniz. Örneğin, arka uç veri deponuz kapalı olduğunda, kullanıcı girişi kabul, "isteğiniz alındı" görüntülemek ve ortalarda başka giriş geçici olarak depolar; sonra hizmeti yeniden uygulandığında girdi almak ve işleyin.

[Kuyruk merkezli çalışma deseni](queue-centric-work-pattern.md) bölüm, bu senaryo işlemek için bir yol gösterir. Düzeltme uygulama görevleri SQL veritabanında depolanır ancak bu SQL veritabanı kapalı olduğunda çalışma çıkmak gerekli değildir. Bu bölümde kullanıcı girişi için bir görev bir bir kuyrukta depolamanıza ve bir çalışan işlemi kuyruğa okuma ve güncelleştirme görevi nasıl göreceğiz. SQL kapalı ise, Düzelt görevler oluşturma olanağı etkilenmez; çalışan işlemi, bekleyin ve SQL veritabanı kullanılabilir olduğunda yeni görevleri işleme.

### <a name="region-failures"></a>Bölge hataları

Tüm bölgeler başarısız olabilir. Doğal afetler bir veri merkezi zarar verebilir, bir meteor tarafından düzleştirilmiş, veri merkezi santral satıra bir inek backhoe vb. ile burying bir çiftçisi tarafından kesilebilir. Uygulamanızı stricken merkezinde barındırılıyorsa ne yaparsınız? Uygulamanızı Azure'a varsa olağanüstü bir durum, başka bir bölgede çalıştırmaya devam birden çok bölgede aynı anda çalıştırmayı ayarlamak mümkündür. Bu tür hataları ender oluşumlar: ve uygulamaların çoğu bu tür hataları aracılığıyla kesintisiz hizmet sağlanması için atlama aracılığıyla bağlantı yok. Uygulamanızı bile bölge hata kullanılabilir tutmak hakkında bilgi için bu bölümde, sonunda kaynaklar bölümüne bakın.

Azure'nın bir hedef işleme bu tür hatalar çok daha kolay hale getirmek için ve nasıl, sonraki bölümlerde bulduğunuzu ilişkin bazı örnekler göreceksiniz.

## <a name="slas"></a>SLA'ları

Kişiler, bulut ortamınızdaki hizmet düzeyi sözleşmelerine (SLA) hakkında sık dinleyin. Temel hizmet nasıl güvenilir olduğu konusunda şirketler yaptığınız gösterir şunlardır. % 99,9 SLA anlamına gelir %, % 99,9 oranında düzgün çalışıyor gibi hizmet beklemelisiniz. Bir SLA'sı için oldukça tipik bir değer olan ve çok yüksek bir sayı gibi görünüyor, ancak aşağı ne kadar süre fark değil. %1 için gerçekten tutar. Bir yıl, ay ve bir hafta içinde için çeşitli SLA yüzde tutar ne kadar kapalı kalma süresi gösteren bir tablo aşağıda verilmiştir.

![SLA tablo](design-to-survive-failures/_static/image2.png)

Bu nedenle % 99,9 SLA hizmetinizi anlamına gelir % yıllık veya aylık 43,2 dakika 8.76 saat olabilir. Çoğu kişi fark ettiğinizden çok fazla zaman olmasıdır. Bu nedenle bir geliştirici olarak belirli bir miktarda süresini mümkün olduğunu unutmayın ve zarif bir şekilde işlemek istersiniz. Belirli bir noktada birisi, uygulamanızın kullandığı geçiyor hizmet çalışmıyor durumda gittiği ve müşteri, söz konusu negatif etkisini en aza indirmek istediğiniz.

SLA hakkında bildiğiniz bir şey olduğunu başvurduğu hangi zaman çerçevesinde: saat her hafta, her ay ya da her yıl sıfırlamaz alma? Azure'da biz iyi ay bir dizi mahsubu tarafından yıllık bir SLA'sı hatalı ay Gizle ComRegisterFunction yıllık bir SLA'dan sizin için iyidir, her ay saati sıfırlayın.

Tabii ki her zaman SLA'sı daha iyi yapmanız uymaya; Genellikle, çok küçüktür aşağı olacaktır. Hiç olmadığı kadar aşağı kesinti en fazla çalışıyoruz, para geri sorabilir vaattir. Büyük olasılıkla geri alma para miktarını tam olarak, kesinti aşırı iş etkisini dengelemek mıydı ancak SLA'ın bu yönü bir yaptırım İlkesi işlevi görür ve biz bunu çok ciddiye olduğunu bildirir.

### <a name="composite-slas"></a>Bileşik SLA'lar

Birden çok hizmet bir uygulamada her hizmet sahip ayrı bir SLA'sı kullanarak etkisini zaman SLA'ları aradığınız düşünmek önemli bir şeydir. Örneğin, düzeltme uygulama, Azure App Service Web Apps, Azure depolama ve SQL veritabanı kullanır. Bu e-kitap aralık, 2013'te yazılan tarih itibariyle SLA numaralarına aşağıda verilmiştir:

![Web sitesi, depolama, SQL veritabanı SLA'sı](design-to-survive-failures/_static/image3.png)

Bu hizmet SLA'nıza bağlı uygulaması için beklediğiniz kesinti maksimum nedir? Aşağı sürenizi bu durumda kötü SLA yüzdesi, veya % 99,9 eşit İmparatoru düşünebilirsiniz. Tüm üç hizmeti her zaman aynı anda başarısız oldu, ancak, gerçekte ne olacağını olmak zorunda değildir true olabilir. Bileşik SLA ayrı ayrı SLA numaraları çarpılarak hesaplamak zorunda her hizmet farklı zamanlarda bağımsız olarak başarısız olabilir.

![Bileşik SLA'sı](design-to-survive-failures/_static/image4.png)

Bu nedenle uygulamanız yalnızca 43,2 dakika, ancak bu miktarın 3 kez, bir ay aylık – 108 dakika olması ve Azure SLA'sı sınırlarda devam.

Bu sorun, Azure'da benzersiz değil. En iyi bulutta kullanılabilir herhangi bir bulut hizmeti SLA'ları gerçekten sunuyoruz ve her satıcının bulut hizmetlerini kullanıyorsanız uğraşmanız benzer sorunlar sahip olacaksınız. Bu vurgular, müşterileriniz veya kullanıcılarınız etkilemek için yeterli sıklıkla meydana çünkü kaçınılmaz hizmet hatalarını işleyeceğinizi uygulamanızı nasıl tasarlayacağınızı hakkında düşünmeye önemini olur.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Kurumsal kapalı kalma süresini deneyimine karşılaştırıldığında bulut SLA'lar

Kişiler bazen örneğin "enterprise Uygulamam hiçbir zaman bu sorunlarla." Ayda aşağı ne kadar süre sorarsanız aslında, genellikle diyor, "Bu da, zaman zaman ortaya çıkar." sahip oldukları Ve bunlar suçlama ne sıklıkta sorarsanız "ihtiyacımız yedeklemek veya yeni bir sunucu veya güncelleştirme yazılım yüklemek için bazen." Elbette, kesinti olarak sayılır. Özellikle görev açısından kritik olmadıkları sürece çoğu Kurumsal uygulama gerçekten hizmet SLA'larımız tarafından izin verilen süre miktarından daha fazla bilgi için kullanılamıyor. Ancak, sunucunuz ve altyapınızı olduğunda ve sorumlu ve onu denetleyen kez daha az angst düşündüğünüz eğilimindedir. Başka birisi bağımlı bulut ortamında ve böylece bu konuda daha kaygılı almak için eğilimli olabilir, neler olduğunu bilmiyorum.

Kuruluş buluttan SLA elde daha büyük bir süresi yüzdesini sağlar, bunlar donanım üzerinde çok daha fazla para harcayarak bunu. Bir bulut hizmeti yapabilirsiniz, ancak çok daha fazlası için hizmetlerinin şarj etmek gerekir. Bunun yerine, uygun maliyetli bir hizmet avantajlarından yararlanın ve yazılımınızı kaçınılmaz hataları müşterileriniz için en az kesintiye neden olacağı şekilde tasarlayın. İşinizi bulut Uygulama Tasarımcısı olarak çok felaketler önlemek için hatadan kaçınmak için değil ve donanım göre değil yazılım odaklanarak bunu. Kurumsal uygulamaları hatalar arasındaki ortalama süreyi en üst düzeye çıkarmak için çaba gelirken, bulut uygulamaları ortalama kurtarma süresi en aza indirmek çaba harcar.

### <a name="not-all-cloud-services-have-slas"></a>Tüm bulut Hizmetleri SLA'ları sahip

Ayrıca her bir bulut hizmeti bile SLA'ya sahip olduğunu unutmayın. Uygulamanızın süresi Garantisi ile bir hizmete bağlı ise, çok uzun aşağı Tahmin edebileceğiniz daha olabilir. Örneğin, sitenizi Facebook veya Twitter gibi sosyal sağlayıcılar kullanmak için oturum açma etkinleştirirseniz, bir SLA ve burada bulabileceğiniz öğrenmek için hizmet sağlayıcısı ile onay biri değildir. Ancak, kimlik doğrulama hizmeti aşağıya inerse veya istek üzerine throw destekleyemiyor, müşterilerinizin uygulamanızı dışında kilitlenir. Gün veya daha uzun kullanılamıyor olabilir. Wordament'ın yeni bir uygulama yüz milyonlarca indirmeler beklenen ve Facebook kimlik doğrulaması – hale geldiği ancak Facebook için çok geç canlı ve bulunan geçmeden önce konuşmak olmadı oluştu. Bu hizmet için SLA sağlanmaz.

### <a name="not-all-downtime-counts-toward-slas"></a>SLA'ları doğru tüm kapalı kalma süresi sayar

Bazı bulut Hizmetleri, kasıtlı olarak uygulamanızı bunları aşırı kullanıyorsa, hizmet reddedebilir. Bu adlandırılır *azaltma*. Hizmet SLA'sı varsa, altında çalışacağı kısıtlanmış ve uygulama tasarımınızı bu koşullara önlemek ve bu durumda azaltma için uygun şekilde tepki koşulları belirtmelidir. Örneğin, saniye başına belirli sayıda aştığında başarısız hizmet istekleri başlatırsanız, otomatik yeniden denemeler meydana şekilde hızlı bunlar devam azaltma neden olmayan emin olmanız gerekir. Biz daha azaltmayı hakkında söylemeniz gerekir [geçici hata işleme bölüm](transient-fault-handling.md).

## <a name="summary"></a>Özet

Bu bölümde neden düzgün bir şekilde hatalara karşı koruma sağlayacak şekilde tasarlanmış olması gerçek bulut uygulaması olduğunu daha iyi anlamanıza yardımcı olması girişimde bulundu. İle başlayarak [sonraki bölümde](monitoring-and-telemetry.md), bu serideki diğer desenler, bunu yapmak için kullanabileceğiniz bazı stratejiler hakkında daha fazla ayrıntıya gidin:

- Sahip iyi [izleme ve telemetri](monitoring-and-telemetry.md), böylece müdahale gerektiren hataları hakkında hızlıca bilgi edinin ve bunları gidermek için yeterli bilgiye sahip.
- [Geçici hataları işlemek](transient-fault-handling.md) akıllı yeniden deneme mantığı, böylece uygulamanız otomatik olarak zaman kullanabilirsiniz ve geri döner kurtarır uygulayarak [devre kesici](transient-fault-handling.md#circuitbreakers) bunu bağlanamazken mantığı.
- Kullanım [dağıtılmış önbelleğe alma](distributed-caching.md) veritabanı erişimi olan aktarım hızı, gecikme süresi ve bağlantı sorunları en aza indirmek için.
- Uygulama aracılığıyla eşlenmesiyle gevşek [kuyruk merkezli çalışma deseni](queue-centric-work-pattern.md), böylece, uygulama ön ucu arka uç kapalı olduğunda çalışmaya devam edebilirsiniz.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için sonraki bölümlerde bu e-kitap ve aşağıdaki kaynaklara bakın.

Belgeler:

- [Hatasız: Dayanıklı bulut mimarileri Kılavuzu](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Andrew Townhill Marc Mercuri ve Ulrich Homann tarafından teknik incelemesinde yanıtlanmıştır. Web sayfası hatasız video serisi sürümü.
- [Azure bulut Hizmetleri'nde büyük ölçekli hizmetler tasarlamak için en iyi uygulamalar](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Mark Simms'in ve Michael Thomassy teknik incelemesinde yanıtlanmıştır.
- [Azure iş sürekliliği teknik rehberlik](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Patrick Wickline ve Jason Roth teknik incelemesinde yanıtlanmıştır.
- [Olağanüstü durum kurtarma ve Azure uygulamaları için yüksek kullanılabilirlik](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Michael McKeown, Hanu Kommalapati ve Jason Roth teknik incelemesinde yanıtlanmıştır.
- [Microsoft desenler ve uygulamalar - Azure Kılavuzu](https://msdn.microsoft.com/library/dn568099.aspx). Çoklu veri merkezi dağıtım kılavuzu, devre kesici düzeni bakın.
- [Azure desteği - hizmet düzeyi sözleşmeleri](https://azure.microsoft.com/support/legal/sla/).
- [Azure SQL veritabanı'nda iş sürekliliği](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). SQL veritabanı yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri hakkındaki belgeler.
- [Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için Azure sanal makineler'de SQL Server](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Videolar:

- [Hatasız: Ölçeklenebilir, dayanıklı bulut hizmetleri oluşturmaya](https://channel9.msdn.com/Series/FailSafe). Dokuz bölümden oluşan Ulrich Homann, Marc Mercuri ve Mark Simms'in. Üst düzey kavramlarını ve mimari ilkeleri gerçek müşterilerle Microsoft Müşteri danışma ekibi (CAT) deneyiminden çizilmiş hikayeleri çok erişilebilir ve ilgi çekici bir biçimde sunar. Bölüm 1 ile 8 derinlik hatalara karşı bulut uygulamaları tasarlama nedenlerini gidin. Ayrıca 49:57 ve açıklamalara hata noktalarını ve bölüm 2 sırasında 56:05 başlatırken hata modlarını devre Kesiciler, Bölüm 3 40:55 başlatırken Tartışılması bölüm 2'de başlayarak azaltma izleme tartışmalara bakın.
- [Yapı büyük: Azure müşterilerinin - Bölüm II dersler](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms'in hata için tasarlama ve her şeyi izleme hakkında konuşuyor. Benzer şekilde hatasız serisi ancak daha fazla nasıl yapılır ayrıntıya gider.

> [!div class="step-by-step"]
> [Önceki](unstructured-blob-storage.md)
> [İleri](monitoring-and-telemetry.md)
