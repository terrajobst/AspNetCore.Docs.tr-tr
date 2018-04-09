---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
title: (Azure ile gerçek bulut uygulamaları derleme) hatalarına karşı korur tasarım | Microsoft Docs
author: MikeWasson
description: Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 364ce84e-5af8-4e08-afc9-75a512b01f84
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/design-to-survive-failures
msc.type: authoredcontent
ms.openlocfilehash: 01883cb0be3e7c7b5dc8d32b784ccb3a28652f1e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="design-to-survive-failures-building-real-world-cloud-apps-with-azure"></a>(Azure ile gerçek bulut uygulamaları derleme) hatalarına karşı korur tasarlama
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. E-kitap hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


Uygulama, ancak özellikle bir yere pek çok kişiyle kullanacağınız, bulutta çalışacak herhangi bir türde oluşturduğunuzda hakkında düşünmek şeyleri biri, düzgün biçimde hataları işlemek ve değer sunmaya devam böylece uygulama tasarlamak nasıl kadar olası. Yeterli zamanı olduğunda şeyleri herhangi bir ortamın veya herhangi bir yazılım sisteminin yanlış Git adımıdır. Bu durumlarda, uygulamanızın nasıl işlediğini nasıl üzmeye müşterilerinizin alırsınız ve ne kadar süre belirler sorunlarını çözümleme ve kullanması gerekir.

## <a name="types-of-failures"></a>Hata türleri

Farklı bir şekilde işlemek istersiniz hatalar iki temel kategoriye ayrılır:

- Geçici, aralıklı ağ bağlantısı sorunları gibi hataları kendi kendini iyileştirme.
- Müdahale gerektiren verip bunu hataları.

Geçici hatalar için bu çoğu uygulamayı hızla ve otomatik olarak kurtarır zaman emin olmak için bir yeniden deneme ilkesi uygulayabilirsiniz. Müşterilerinize biraz daha uzun yanıt süresi fark edebilirsiniz, ancak bunlar aksi etkilenmeyecek. Bu hataları işlemek için bazı yollar göstereceğiz [geçici hata işleme bölüm](transient-fault-handling.md).

Hataları verip bunu için izleme ve sorunlar çıkması ve kök neden analizi kolaylaştıran hemen uyarır işlevselliği günlüğü uygulayabilirsiniz. Bu tür hatalara üstünde olmanıza yardımcı olması için bazı yollar göstereceğiz [izleme ve Telemetri bölüm](monitoring-and-telemetry.md).

## <a name="failure-scope"></a>Hata kapsamı

Tek bir makineye etkileyip etkilemediğini, hata kapsam hakkında – düşünmek de SQL veritabanı veya depolama ya da tüm bir bölgeyi gibi tüm hizmet.

![Hata kapsamı](design-to-survive-failures/_static/image1.png)

### <a name="machine-failures"></a>Makine hataları

Azure, başarısız bir sunucu tarafından yeni bir otomatik olarak değiştirilir ve bir iyi tasarlanmış bulut uygulaması hızla ve otomatik olarak bu tür bir hata kurtarır. Daha önce bu biz durum bilgisiz web katmanı ölçeklenebilirlik avantajlarını kullanımı yoğun ve başarısız bir sunucudan kurtarma kolay statelessness başka bir yararı biridir. Kurtarma kolay SQL Database ve Azure App Service Web Apps gibi hizmet olarak platform (PaaS) özellikleri avantajlarından biri biridir. Donanım hataları nadir, ancak bunlar bu hizmetleri bunları otomatik olarak işlemek ortaya çıktığında; bile bu hizmetlerden biri kullanırken makine hataları işlemek için kod yazmak zorunda değilsiniz.

### <a name="service-failures"></a>Hizmet hataları

Bulut uygulamalarını genellikle birden çok hizmet kullanın. Örneğin, depolama birimi hizmeti SQL veritabanı hizmetinin Düzelt uygulama kullanır ve web uygulamasını Azure App Service'e dağıtılır. Üzerinde bağımlı hizmetlerden biri başarısız olursa, uygulamanızın ne? Bazı hataları kolay bir hizmet için "ne yazık ki daha sonra yeniden deneyin" iletisi yapabileceğiniz en iyi olabilir. Ancak, birçok senaryoda, daha iyi yapabilirsiniz. Örneğin, arka uç veri deponuza aşağı olduğunda, kullanıcı girişi kabul, "isteğiniz alındı" görüntülemek ve ortalarda başka giriş geçici olarak depolar; Daha sonra ihtiyacınız hizmeti yeniden işletimsel olduğunda giriş almak ve işleme.

[Kuyruk merkezli çalışma deseni](queue-centric-work-pattern.md) bölüm bu senaryo işlemek için bir yol gösterir. Düzelt uygulama görevleri SQL veritabanına depolar, ancak SQL veritabanı aşağı olduğunda çalışma çıkmak sahip değil. Bu bölümde bir kuyruktaki bir görev için kullanıcı girişi depolamak ve sıra okuma ve görev güncelleştirmek için bir çalışan işlem kullanma göreceğiz. SQL kapalı ise, Düzelt görevler oluşturma olanağı etkilenmez; çalışan işlemi bekleyin ve SQL veritabanı kullanılabilir olduğunda yeni görevleri işleme.

### <a name="region-failures"></a>Bölge hataları

Tüm bölgeler başarısız olabilir. Doğal afet bir veri merkezi zarar verebilir, meteor tarafından düzleştirilmiş, veri merkezi santral satıra inek backhoe, vb. ile burying farmer Kes. Uygulamanızı stricken veri merkezine barındırılıyorsa ne yapacaksınız? Varsa bir olağanüstü durum birinde, başka bir bölgede çalışmaya devam birden çok bölgede aynı anda çalışmasına Azure uygulamanızda ayarlamak mümkündür. Ender oluşum böyle hatalarıdır ve çoğu uygulamalar bu sıralama hatalarının aracılığıyla kesintisiz hizmet sağlamak için gereken hoops aracılığıyla bağlantı yok. Uygulamanız bile bir bölge hatası kullanılabilir devam hakkında bilgi için bu bölümde, sonunda kaynaklar bölümüne bakın.

Bir Azure tüm hataları çok daha kolay bu tür işleme yapmak için hedefidir ve nasıl biz, aşağıdaki bölümlerde yaptığınız bazı örnekler görürsünüz.

## <a name="slas"></a>SLA'ları

Hizmet düzeyi sözleşmelerine (SLA) bulut ortamınızdaki konusunda kişiler genellikle öğrenin. Temel olarak kendi hizmet nasıl güvenilir olduğu konusunda şirketler yapan öneriler şunlardır. Bir onay % 99,9 SLA anlamına gelir, hizmet zaman % 99,9 düzgün çalışmıyor olabilir beklemelisiniz. Bir SLA için oldukça tipik bir değer olan ve çok yüksek bir sayı gibi ses ancak aşağı ne kadar süre farkına varmazsınız. %1 için gerçekten tutar. Aşağıda, bir yıl, ay ve bir hafta için çeşitli SLA yüzdeleri tutar ne kadar kapalı kalma süresi gösteren bir tablo verilmiştir.

![SLA tablosu](design-to-survive-failures/_static/image2.png)

Bu nedenle bir onay % 99,9 SLA hizmetinizi anlamına gelir 8.76 saatleri bir yıl veya ayda bir 43,2 dakika olabilir. Çoğu kişi fark ettiğinizden çok fazla zaman olmasıdır. Böylece bir uygulama geliştiricisi olarak belirli bir miktar kesinti mümkün olduğunu unutmayın ve zarif bir şekilde işlemek istiyor. Belirli bir noktada birisi, uygulamanızın kullanıyor olacağını ve bir hizmet aşağı alacağını ve, negatif müşteri üzerindeki etkisini en aza istiyorsunuz.

Bir SLA hakkında bildiğiniz bir şey. hangi zaman çerçevesinde başvurduğu: saatini her hafta, her ay ya da her yıl sıfırlamak alma mu? Azure'da biz iyi ayda bir dizi kaydırma işlemi tarafından hatalı ay yıllık SLA Gizle bu yana, yıllık bir SLA'ya sizin için daha iyi olan her ay saati sıfırlayın.

Elbette ki her zaman SLA daha iyi yapmak için aspire; Genellikle, çok küçüktür aşağı olması. Promise şimdiye kadar aşağı kesinti maksimum daha uzun süre ki, para geri sorabilirsiniz ' dir. Büyük olasılıkla ulaşırsınız para miktarını tam olarak, aşırı kesinti iş etkisini dengelemek olmayacaktır, ancak bu boyut SLA zorlama ilke olarak davranır ve, biz bunu çok ciddiye olduğunu bilmenizi sağlar.

### <a name="composite-slas"></a>Bileşik SLA'ları

Ayrı bir SLA sahip her hizmeti ile bir uygulamada birden fazla hizmet kullanarak etkisini SLA sırasında ne zaman arıyorsanız düşünmek için önemli bir şeydir. Örneğin, Azure App Service Web Apps, Azure Storage ve SQL veritabanı Düzelt uygulama kullanır. Bu e-kitap aralık, 2013'te yazılmış tarih itibariyle SLA numaralarına aşağıda verilmiştir:

![SLA Web sitesi, depolama, SQL veritabanı](design-to-survive-failures/_static/image3.png)

Bu hizmet SLA tabanlı uygulama için beklediğiniz kesinti maksimum nedir? Aşağı zaman bu durumda en kötü SLA yüzdesi, % 99,9'e eşit veya olacağını düşünebilirsiniz. Tüm üç hizmeti her zaman aynı anda başarısız oldu, ancak bu mutlaka gerçekten neler değil ise true olabilir. Ayrı ayrı SLA numaraları çarparak bileşik SLA hesaplamak alacak şekilde her bir hizmet farklı zamanlarda bağımsız olarak başarısız olabilir.

![Bileşik SLA'sı](design-to-survive-failures/_static/image4.png)

Bu nedenle uygulamanızın yalnızca 43,2 dakika bir ay ancak bu tutar 3 kez, ayda – 108 dakika olması ve Azure SLA sınırlarda devam.

Bu sorunu Azure için benzersiz değil. Her satıcının bulut hizmetlerini kullanıyorsanız uğraşmanız benzer sorunlar elde edersiniz ve en iyi bulut, kullanılabilir herhangi bir bulut hizmet SLA gerçekte sağlıyoruz. Bu vurgular bunlar sıklıkta müşteriler veya kullanıcıları etkileyecek şekilde gerçekleşebilir çünkü kaçınılmaz hizmet hataları işleyebilmesini uygulamanızı nasıl tasarlayacağınızı hakkında düşünmeye önemini olur.

### <a name="cloud-slas-compared-to-enterprise-down-time-experience"></a>Kurumsal kapalı kalma süresi deneyimine göre bulut SLA'ları

Kişiler bazen söyleyin, "my kuruluş uygulaması hiçbir zaman bu sorunları sahibim." Aşağı ne kadar süre ayda bir sorun varsa gerçekte, bunlar genellikle söyleyin, "Da zaman zaman ortaya çıkar." sahip oldukları Ve bunlar haklıymış ne sıklıkta isteyin, "ihtiyacımız yedeklemek veya yeni bir sunucu veya güncelleştirme yazılım yüklemek için bazen." Elbette, kesinti olarak sayılır. Özellikle kritik olmadıkça çoğu Kurumsal uygulama gerçekte bizim hizmet SLA tarafından izin verilen süre miktarından daha fazla bilgi için kullanılamıyor. Ancak, sunucunuz ve altyapınızı olduğunda ve sorumlu ve bunu denetiminde bulunmadığında, daha az angst kez eşitleyerek eğilimindedir. Başka birisi bağımlı bulut ortamında ve böylece hakkında daha fazla kaygılı almak için eğilimli olabilir, neler olduğunu bilmiyorsanız.

Kuruluş buluttan SLA almak daha büyük bir süresi yüzdesi erişir, bu çok daha fazla para donanımda harcama tarafından yaparlar. Bir bulut hizmeti, yapabilirsiniz ancak çok daha fazlasını hizmetlerinin için ücret etmesi gerekir. Bunun yerine, uygun maliyetli bir hizmet yararlanabilir ve böylece müşterileriniz için minimum kesintisi kaçınılmaz hataları neden yazılımınızı tasarlayın. Bir bulut Uygulama Tasarımcısı olarak işinizi çok catastrophe önlemek için hatadan kaçınmak için değil ve değil donanım, yazılım Odaklanıldığında bunu. Hataları arasındaki ortalama süre en üst düzeye çıkarmak için Kurumsal uygulamaları çaba gelirken, bulut uygulamaları kurtarmak için ortalama süreyi en aza indirmek çalışmalar yapılmaktadır.

### <a name="not-all-cloud-services-have-slas"></a>Tüm bulut Hizmetleri SLA sahip

Ayrıca her bulut hizmeti bile bir SLA olduğunu unutmayın. Uygulamanızın süresi garanti hizmetiyle bağımlı ise, ne kadar uzun aşağı Tahmin edebileceğiniz daha olabilir. Örneğin, sitenizi Facebook veya Twitter'da gibi sosyal bir sağlayıcı kullanarak oturum açma devre dışı bırakırsanız, bir SLA yoktur ve bir değil ölçeklendiriyor bulabileceğiniz öğrenmek için hizmet sağlayıcınıza başvurun. Ancak, kimlik doğrulama hizmeti bağlantısı kesilse veya adresinden throw istek hacmini destekleyemiyor, müşterilerinize uygulamanızın dışında kilitlenir. Gün veya daha uzun kullanılamıyor olabilir. Yeni bir uygulama oluşturucuları yüz milyonlarca yüklemeleri beklenen ve Facebook kimlik doğrulaması – bir bağımlılık sürdü ancak Facebook için canlı ve bulunan çok geç geçmeden önce konuşun alamadık, bu hizmet için hiçbir SLA vardı.

### <a name="not-all-downtime-counts-toward-slas"></a>SLA doğru tüm kapalı kalma süresi sayar

Uygulamanıza bunları aşırı kullanıyorsa, bazı bulut Hizmetleri kasıtlı olarak hizmet reddedebilir. Bu adlı *azaltma*. Bir hizmet bir SLA varsa, altında çalışacağı kısıtlanan ve Uygulama tasarımınız Bu koşullardan kaçının ve bu durumda azaltma için uygun şekilde tepki koşulları durumu. Örneğin, bir hizmet isteklerine saniye başına belirli sayıda aştıklarında başarısız başlatırsanız, otomatik yeniden denemeler şekilde hızlı neden bunlar devam etmek azaltma durum yok emin olmak istersiniz. Biz de azaltma hakkında söylemek için daha fazla gerekir [geçici hata işleme bölüm](transient-fault-handling.md).

## <a name="summary"></a>Özet

Bu bölüm, gerçek dünya bulut uygulama hataları düzgün biçimde varlığını sürdürmesi için tasarlanmalıdır neden olduğunu fark yardımcı denedi. İle başlayarak [sonraki bölümde](monitoring-and-telemetry.md), bunu yapmak için kullanabileceğiniz bazı stratejiler hakkında daha fazla ayrıntı içine bu serideki kalan desenleri gidin:

- Sahip iyi [izleme ve telemetri](monitoring-and-telemetry.md), böylece müdahale gerektiren hataları hakkında hızla bilgi almak ve bunları gidermek için yeterli bilgiye sahip.
- [Geçici hataları işlemek](transient-fault-handling.md) akıllı yeniden deneme mantığı, böylece uygulamanız otomatik olarak zaman kullanabilirsiniz ve geri döner kurtarır uygulayarak [devre kesici](transient-fault-handling.md#circuitbreakers) bunu yapamadığında mantığı.
- Kullanım [önbelleğe alma dağıtılmış](distributed-caching.md) veritabanı erişimi verimlilik, gecikme ve bağlantı sorunları en aza indirmek için.
- Uygulama aracılığıyla Kuplaj gevşek [kuyruk merkezli çalışma deseni](queue-centric-work-pattern.md), böylece, uygulama ön uç arka uç aşağı olduğunda çalışmaya devam edebilirsiniz.

## <a name="resources"></a>Kaynaklar

Daha fazla bilgi için sonraki bölümlerde bu e-kitap ve aşağıdaki kaynaklara bakın.

Belgeler:

- [Hatasız: Dayanıklı bulut mimarileri Kılavuzu](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Teknik incelemesi Marc Mercuri, Ulrich Homann ve Barış Townhill tarafından. Web sayfası hatasız video serisi sürümü.
- [En iyi uygulamalar için Azure bulut hizmetleri üzerinde büyük ölçekli hizmetler tasarımını](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Teknik incelemesi işareti SIMM'lerinin ve Michael Thomassy tarafından.
- [Azure iş sürekliliği teknik kılavuz](https://msdn.microsoft.com/library/windowsazure/hh873027.aspx). Teknik incelemesi CAN Wickline ve Jason Roth tarafından.
- [Azure uygulamaları için yüksek kullanılabilirlik ve olağanüstü durum kurtarma](https://msdn.microsoft.com/library/windowsazure/dn251004.aspx). Michael McKeown, Hanu Kommalapati ve Jason Roth teknik incelemesinde yanıtlanmıştır.
- [Microsoft Patterns and Practices - Azure Kılavuzu](https://msdn.microsoft.com/library/dn568099.aspx). Birden çok veri merkezi dağıtım kılavuzu, devre kesici düzeni bakın.
- [Azure desteği - hizmet düzeyi sözleşmelerine](https://azure.microsoft.com/support/legal/sla/).
- [Azure SQL veritabanındaki iş sürekliliği](https://msdn.microsoft.com/library/windowsazure/hh852669.aspx). SQL veritabanı yüksek kullanılabilirlik ve olağanüstü durum kurtarma özelliklerle ilgili belgelerine.
- [Yüksek kullanılabilirlik ve olağanüstü durum kurtarma için Azure sanal makinelerde SQL Server](https://msdn.microsoft.com/library/windowsazure/jj870962.aspx).

Videolar:

- [Hatasız: Ölçeklenebilir ve esnek bulut Hizmetleri derleme](https://channel9.msdn.com/Series/FailSafe). Dokuz parçalı seriyi Ulrich Homann, Marc Mercuri ve işareti SIMM'lerinin. Üst düzey kavramlarını ve mimari ilkeler çok erişilebilir ve ilginç yolla, Microsoft Müşteri danışma ekibi (CAT) deneyiminde gerçek müşterilerle çizilmiş hikayeleri sunar. Bölüm 1 ile 8 derinlemesine hatalarına karşı korur bulut uygulamaları tasarlama nedenleri gidin. Ayrıca 49:57, hata noktaları ve bölüm 2 56:05 başlatırken hata modları tartışması ve hattı bölüm 3 40:55 başlatırken ayırıcıları tartışma bölüm 2'de başlayarak hızlandırma izleme tartışma bakın.
- [Yapı büyük: Azure müşterilerden - bölümü II dersleri öğrenilen](https://channel9.msdn.com/Events/Build/2012/3-030). İşareti SIMM'lerinin için hatası tasarlama ve her şeyi düzenleme hakkında alınmaktadır. Hatasız serisi ancak yapılır Ayrıntılar gerçekleştirip gerçekleştirmeyeceğini benzer.

> [!div class="step-by-step"]
> [Önceki](unstructured-blob-storage.md)
> [sonraki](monitoring-and-telemetry.md)
