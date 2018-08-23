---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
title: ASP.NET Durum İzleme (VB) ile hata ayrıntılarını günlüğe kaydetme | Microsoft Docs
author: rick-anderson
description: Microsoft'un sistem durumu izleme sistemi, işlenmemiş özel durumlar dahil olmak üzere çeşitli web olaylarını günlüğe kaydedecek şekilde kolay ve özelleştirilebilir bir yol sağlar. Bu öğreticide k açıklanmaktadır...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 09a6c74e-936a-4c04-8547-5bb313a4e4a3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-vb
msc.type: authoredcontent
ms.openlocfilehash: f9aeeb0b3d21707324d239efad4deffcf0f7fead
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41755850"
---
<a name="logging-error-details-with-aspnet-health-monitoring-vb"></a>ASP.NET Durum İzleme (VB) ile hata ayrıntılarını günlüğe kaydetme
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indir](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_VB.zip) veya [PDF olarak indirin](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_vb.pdf)

> Microsoft'un sistem durumu izleme sistemi, işlenmemiş özel durumlar dahil olmak üzere çeşitli web olaylarını günlüğe kaydedecek şekilde kolay ve özelleştirilebilir bir yol sağlar. Bu öğreticide, İşlenmeyen özel durumlar bir veritabanında oturum ve e-posta aracılığıyla geliştiricilerin bildirmek için izleme sistem durumu ayarlanıyor aracılığıyla açıklanmaktadır.


## <a name="introduction"></a>Giriş

Dağıtılan bir uygulama durumunu izlemek için ve karşılaşabileceğiniz sorunları tanılamak için yararlı bir araçtır günlüğe kaydetme işlemi. Dağıtılan bir uygulamada gerçekleşir ve böylece bunlar giderilebilir hataları günlüğe kaydetmek özellikle önemlidir. `Error` Olayı, bir ASP.NET uygulamasında; işlenmeyen bir özel durum oluştuğunda oluşturulur [önceki öğretici](processing-unhandled-exceptions-vb.md) bir geliştirici bir hata bildirir ve ayrıntılarını içinbirolayişleyicisioluşturarakoturumgösterildi`Error` olay. Bununla birlikte, oluşturmak bir `Error` hatanın ayrıntılarını ve bir geliştirici bildirmek için olay işleyicisi seçimdir gereksiz, bu görevi, ASP tarafından gerçekleştirilebilir. NET *sistem durumu izleme sistemi*.

Sistem durumu izleme sistemi ASP.NET 2.0 sürümünde kullanıma sunulmuştur ve dağıtılan bir ASP.NET uygulama durumunu izlemek için uygulama veya isteğinin yaşam süresi gerçekleşen olayları tasarlanmıştır. Sistem durumu izleme sistemi tarafından kaydedilen olayları olarak ifade edilir *sistem durumu izleme olayları* veya *Web olayları*ve içerir:

- Ne zaman bir uygulama başlatıldığında veya durdurulduğunda gibi uygulama yaşam süresi olayları
- Güvenlik olaylarını da dahil olmak üzere, oturum açma denemesi başarısız oldu ve URL yetkilendirme isteği başarısız oldu
- Uygulama hatalarına, şunlar gibi özel durumlar, özel durumlar, istek doğrulama özel durumlar ve diğer hata türleri arasında belirli bir derleme ayrıştırma görünüm durumu işlenmemiş.

Bir sistem durumu izleme olayı zaman, dilediğiniz sayıda günlüğe kaydedilebilecek belirtilen *oturum kaynakları*. Sistem durumu izleme sistemi, Microsoft SQL Server veritabanı için Windows olay günlüğü veya aracılığıyla bir e-posta iletisinde, diğerlerinin yanı sıra Web olayları oturum günlüğü kaynakları ile birlikte gelir. Kendi günlük kaynaklarını da oluşturabilirsiniz.

Durum sistemini izleme günlükleri, günlük kaynakları birlikte kullanıldığında, olayların tanımlanan `Web.config`. Yapılandırma biçimlendirme birkaç satır kod ile tüm işlenmemiş özel durumların bir veritabanında oturum ve özel e-posta yoluyla bildiren izleme sistem durumu kullanabilirsiniz.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Sistem durumu izleme sistem yapılandırmasının keşfetme

Sistem durumu izleme sisteminin davranışı bulunan yapılandırma bilgileri, tanımlanan [ `<healthMonitoring>` öğesi](https://msdn.microsoft.com/library/2fwh2ss9.aspx) içinde `Web.config`. Bu yapılandırma bölümü yanı sıra, şu üç önemli bilgilere tanımlar:

1. Sistem durumu izleme olayları, gerçekleşti, oturum açmış olmanız,
2. Günlük kaynaklarını ve
3. Her durum (1) içinde tanımlanan olay izleme günlüğü kaynaklarına nasıl eşlendiğini (2) tanımlanır.

Bu bilgiler, üç alt yapılandırma öğeleri aracılığıyla belirtilir: [ `<eventMappings>` ](https://msdn.microsoft.com/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/library/zaa41kz1.aspx), ve [ `<rules>` ](https://msdn.microsoft.com/library/fe5wyxa0.aspx)sırasıyla.

Varsayılan durum sistemini yapılandırma bilgilerini izleme bulunabilir `Web.config` dosyası `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` klasör. Bu varsayılan yapılandırma bilgileri, konuyu uzatmamak amacıyla, kaldırılan bazı biçimlendirme ile aşağıda gösterilmiştir:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample1.xml)]

Sistem durumu izleme olaylarının tanımlanmış `<eventMappings>` öğesi, sistem durumu izleme olayları bir sınıf için bir insan kolay ad verir. Yukarıdaki biçimlendirmede `<eventMappings>` öğesi türünde olayları izleme sistem durumu için İnsan kolay adı "Tüm hataları" atar `WebBaseErrorEvent` ve adı "hata denetimleri" sistem durumu olay türü izleme için `WebFailureAuditEvent`.

`<providers>` Öğe bunları İnsan kolay ad vermek ve tüm günlük kaynak özgü yapılandırma bilgilerini belirterek günlük kaynaklarını tanımlar. İlk `<add>` öğe belirtilen sistem durumu olaylarını kullanarak izleme günlükleri "EventLogProvider" Sağlayıcı tanımlar `EventLogWebEventProvider` sınıfı. `EventLogWebEventProvider` Sınıfı olayı Windows olay günlüğüne kaydeder. İkinci `<add>` öğesi, bir Microsoft SQL Server veritabanına olayları günlüğe kaydeden "SqlWebEventProvider" Sağlayıcı tanımlar `SqlWebEventProvider` sınıfı. "SqlWebEventProvider" yapılandırma veritabanının bağlantı dizesini belirtir. (`connectionStringName`) diğer yapılandırma seçenekleri arasında.

`<rules>` Öğeyi belirtilen olayları eşleyen `<eventMappings>` kaynakları oturum açmak için öğe `<providers>` öğesi. Varsayılan olarak, ASP.NET web uygulamaları, tüm işlenmemiş özel durumları günlüğe kaydetmek ve hataları için Windows olay günlüğünü denetleyin.

## <a name="logging-events-to-a-database"></a>Bir veritabanı için günlük olayları

Sistem durumu izleme sistemin varsayılan yapılandırmasını ekleyerek web uygulama tarafından web uygulaması olarak özelleştirilebilir bir `<healthMonitoring>` uygulamanın bölümüne `Web.config` dosya. Ek öğeler içerebilir `<eventMappings>`, `<providers>`, ve `<rules>` kullanarak olarak bölümlerde `<add>` öğesi. Bir ayarı varsayılan yapılandırma kullanımdan kaldırılacağı `<remove>` öğesi ya da kullanım `<clear />` tüm varsayılan değerleri bu bölümlerin birinden kaldırmak için. Kitap incelemeleri web uygulamasını kullanarak bir Microsoft SQL Server veritabanı tüm işlenmemiş özel durumların günlüğe yapılandıralım `SqlWebEventProvider` sınıfı.

`SqlWebEventProvider` Sınıfı sistem durumu izleme sistemi bir parçasıdır ve sistem durumu izleme olayı belirtilen SQL Server veritabanına kaydeder. `SqlWebEventProvider` Sınıfı bekliyor belirtilen veritabanı adında bir saklı yordamı içerdiğini `aspnet_WebEvent_LogEvent`. Bu saklı yordamı bir olayın ayrıntıları geçirilir ve olay ayrıntılarını depolama ile görevli. Bu depolanan oluşturmak gerekmez güzel bir haberimiz var olduğu ya da olay ayrıntılarını depolamak üzere tablo yordamı. Bu nesneleri kullanarak veritabanı ekleyebileceğiniz `aspnet_regsql.exe` aracı.

> [!NOTE]
> `aspnet_regsql.exe` Aracı ele alınan geri [ *bir Web sitesi, kullandığı uygulama hizmetleri yapılandırma* öğretici](configuring-a-website-that-uses-application-services-vb.md) ASP desteği zaman ekledik. NET uygulama hizmetleri. Sonuç olarak, Kitap incelemeleri Web sitesinin veritabanı zaten var. `aspnet_WebEvent_LogEvent` saklı yordamsa, adlı bir tabloya olay bilgilerini depolayan `aspnet_WebEvent_Events`.


Veritabanına eklenen tablo ve gerekli saklı yordam aldıktan sonra kalan tek şey durum tüm işlenmemiş özel durumların veritabanında oturum izleme bildirin. Aşağıdaki biçimlendirme, Web sitenizin ekleyerek bunu `Web.config` dosyası:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample2.xml)]

Sistem durumu izleme yapılandırma biçimlendirme kullanımlar yukarıda `<clear />` önceden tanımlanmış sistem durumu izleme yapılandırma bilgilerini silmek için öğeler `<eventMappings>`, `<providers>`, ve `<rules>` bölümler. Ardından tek bir giriş bu bölümlerin her öğesine ekler.

- `<eventMappings>` Öğe tek sistem durumu izleme "Tüm işlenmeyen bir özel durum oluştuğunda gerçekleşmek üzereyken hataları", adlı ilgilendiğiniz olayı tanımlar.
- `<providers>` Öğe tanımlar "kullanan SqlWebEventProvider" adlı tek bir günlük kaynağına `SqlWebEventProvider` sınıfı. `connectionStringName` "Bizim bağlantısının adı ReviewsConnectionString için", öznitelik ayarlandı içinde tanımlanan bir dize `<connectionStrings>` bölümü.
- Son olarak, &lt;kuralları&gt; öğesi, "Tüm hataları" olay transpires olduğunda, "SqlWebEventProvider" sağlayıcısını kullanarak günlüğe kaydedilmesi gerektiğini gösterir.

Bu yapılandırma bilgileri sistem durumu izleme sistemi, tüm işlenmemiş özel durumların Kitap incelemeleri veritabanında oturum bildirir.

> [!NOTE]
> `WebBaseErrorEvent` Olayı için sunucu hataları yalnızca oluşturulur; bulunamadığını bildiren bir ASP.NET kaynak istekleri gibi HTTP hata oluşmaz. Bu davranışından farklıdır `HttpApplication` sınıfın `Error` hem sunucu hem de HTTP hataları için oluşan olayı.


Sistem durumu izleme sistemi uygulamada görmek için Web sitesini ziyaret edin ve bir çalışma zamanı hatası ederek oluşturmak `Genre.aspx?ID=foo`. Uygun hata sayfası - özel durum ayrıntıları sarı ekran'ın (yerel olarak açtıklarında) ölüm ya da (üretim sitesini ziyaret ederken) özel hata sayfası görmeniz gerekir. Arka planda sistem durumu izleme sistemi veritabanına hata bilgilerini günlüğe kaydedilir. Bir kayıt olmalıdır `aspnet_WebEvent_Events` tablo (bkz **Şekil 1**); bu kaydı yalnızca oluşan çalışma zamanı hata hakkındaki bilgileri içerir.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image1.png)

**Şekil 1**: hata ayrıntılarını günlüğe kaydedilen `aspnet_WebEvent_Events` tablo  
([Tam boyutlu görüntüyü görmek için tıklatın](logging-error-details-with-asp-net-health-monitoring-vb/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Bir Web sayfasında hata günlüğünü görüntüleme

Web sitesinin geçerli yapılandırma ile sistem durumu izleme sistemi, veritabanına tüm işlenmemiş özel durumların günlüğe kaydeder. Ancak, sistem durumu izlemesi aracılığıyla bir web sayfası için hata günlüğünü görüntülemek için herhangi bir mekanizma sağlamaz. Ancak, bu bilgileri veritabanından görüntüleyen bir ASP.NET sayfası da oluşturabilirsiniz. (Kısa bir süre içinde anlatıldığı gibi bir e-posta iletisinde size gönderilen hata ayrıntılarını sağlamak için seçebilirsiniz.)

Böyle bir sayfa oluşturursanız, yalnızca yetkili kullanıcıların hata ayrıntılarını görüntülemek adımlar emin olun. Sitenizi kullanıcı hesaplarını içeriyorsa belirli kullanıcılar ya da roller sayfasına erişimi kısıtlamak için URL yetkilendirme kuralları kullanabilirsiniz. Vermek veya oturum açmış kullanıcıya bağlı web sayfalarına erişimi kısıtlamak nasıl daha fazla bilgi için Bilgisayarım [Web sitesi güvenlik öğreticileri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> Sonraki öğretici ELMAH adlı bir alternatif hata günlüğü ve bildirim sistemi inceler. ELMAH hem bir web sayfasından ve bir RSS akışı olarak hata günlüğünü görüntülemek için yerleşik bir mekanizma içerir.


## <a name="logging-events-to-email"></a>E-posta olayları günlüğe kaydetme

Sistem durumu izleme sistemi, "e-posta iletisine bir olayı günlüğe kaydeder" günlük bir kaynak sağlayıcısı içerir. Günlük kaynak e-posta ileti gövdesindeki veritabanına kaydedilir aynı olan bilgileri içerir. Belirli bir sistem durumu izleme olayı ortaya çıktığında, bir geliştirici bildirmek için bu günlüğü kaynağı kullanabilirsiniz.

Bir özel durum olduğunda e-posta aldığımız böylece Web sitesinin yapılandırma gerçekleşir Kitap incelemeleri güncelleştirelim. Bunu gerçekleştirmek için şu üç görev gerçekleştirmeniz gerekir:

1. ASP.NET web uygulaması, e-posta göndermek için yapılandırın. Bu e-posta iletilerini aracılığıyla nasıl gönderileceğini belirterek gerçekleştirilir `<system.net>` yapılandırma öğesi. E-posta gönderme hakkında daha fazla bilgi için bir ASP.NET uygulamasında iletileri başvurmak [ASP.NET e-posta gönderme](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) ve [System.Net.Mail SSS](http://systemnetmail.com/).
2. İçindeki e-posta günlük kaynak sağlayıcısını kaydetme `<providers>` öğesi, ve
3. Bir girdiyi `<rules>` (2) basamaktaki günlüğü kaynak sağlayıcısı için "Tüm hataları" olay eşlemeleri öğesi.

Sistem durumu izleme sistemi iki e-posta günlük kaynak sağlayıcısı sınıfları içerir: `SimpleMailWebEventProvider` ve `TemplatedMailWebEventProvider`. [ `SimpleMailWebEventProvider` Sınıfı](https://msdn.microsoft.com/library/system.web.management.simplemailwebeventprovider.aspx) olay içeren bir düz metin e-posta iletisi ayrıntılarını ve e-posta gövdesi çok az özelleştirme sağlar gönderir. İle [ `TemplatedMailWebEventProvider` sınıfı](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) olan biçimlendirmenin için e-posta iletisi gövdesi olarak kullanılan bir ASP.NET sayfasında belirtin. [ `TemplatedMailWebEventProvider` Sınıfı](https://msdn.microsoft.com/library/system.web.management.templatedmailwebeventprovider.aspx) içeriğini ve e-posta iletisinin biçimi üzerinde daha büyük denetim sağlar, ancak e-posta iletisinin gövdesi oluşturan ASP.NET sayfası oluşturmak sahip olduğunuz biraz daha fazla ön çalışma gerektirir. Bu öğreticiyi kullanmaya odaklanmıştır `SimpleMailWebEventProvider` sınıfı.

Sistem durumu izleme sistemi güncelleştirme `<providers>` öğesinde `Web.config` günlük kaynağı için dahil edilecek dosyası `SimpleMailWebEventProvider` sınıfı:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample3.xml)]

Yukarıdaki biçimlendirme kullanan `SimpleMailWebEventProvider` günlüğü kaynak sağlayıcısı olarak sınıfı ve kolay adı "EmailWebEventProvider" atar. Ayrıca, `<add>` öznitelik gibi yapılır ve e-posta iletisinin adreslerinden ek yapılandırma seçenekleri içerir.

Tanımlanan e-posta günlüğü kaynağı ile kalan tek şey sistem durumu izleme sistemi "İşlenmeyen özel durumları günlüğe kaydetmek için" Bu kaynağı kullanmaya bildirin. Bu yeni bir kuralda ekleyerek gerçekleştirilir `<rules>` bölümü:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-vb/samples/sample4.xml)]

`<rules>` Bölüm artık iki kural içerir. "Tüm hatalar için e-posta" adlı birinci tüm işlenmemiş özel durumların "EmailWebEventProvider" günlük kaynağına gönderir. Bu kural Web sitesinde hatalarıyla ilgili ayrıntıları belirtilen gönderme etkisi adresine. "Tüm hataları için veritabanı" kuralı sitenin veritabanına hata ayrıntılarını günlüğe kaydeder. Sonuç olarak, sitesinde ayrıntılarını işlenmeyen bir özel durum oluştuğunda hem de veritabanında günlüğe ve belirtilen e-posta adresine gönderildi.

**Şekil 2** tarafından oluşturulan e-posta gösterir `SimpleMailWebEventProvider` sınıfı ziyaret `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-vb/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-vb/_static/image4.png)

**Şekil 2**: hata ayrıntıları, bir e-posta iletisi gönderilir  
([Tam boyutlu görüntüyü görmek için tıklatın](logging-error-details-with-asp-net-health-monitoring-vb/_static/image6.png))

## <a name="summary"></a>Özet

ASP.NET sistem durumu izleme sistemi, yöneticilerin bir dağıtılan web uygulamasının sistem izleme sağlamak için tasarlanmıştır. Sistem durumu izleme olayları, uygulama durdurduğunda başarıyla sitesinde oturum açtığında gibi belirli eylemleri düzleştirme veya işlenmeyen bir özel durum oluştuğunda oluşturulur. Bu olaylar, herhangi bir sayıda günlüğü kaynakları kaydedilebilir. Bu öğretici, bir veritabanı ile bir e-posta iletisi işlenmeyen özel durumların ayrıntılarını oturum nasıl oluşturulacağını gösterir.

Bu öğreticide durum işlenmeyen özel durumlar, ancak sistem durumu izleme dağıtılan bir ASP.NET uygulaması genel durumunu ölçmek için tasarlanmıştır ve tesis sistem durumu izleme olayları içeren olduğunu aklınızda bulundurun ve oturum olmayan kaynakları için izleme kullanarak odaklanır. Burada incelediniz. Dahası, kendi sistem durumu izleme olayları ve günlük kaynaklarını oluşturabilirsiniz gereken ortaya çıkar. Sistem durumu izleme hakkında daha fazla bilgi edinmek istiyorsanız, okumak için iyi bir ilk adım olan [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)'s [sistem durumu izleme SSS](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). Başvurun [nasıl yapılır: ASP.NET 2.0 kullanım sistem durumu izleme](https://msdn.microsoft.com/library/ms998306.aspx).

Mutlu programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide ele alınan konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET Durum İzleme Genel Görünümü](https://msdn.microsoft.com/library/bb398933.aspx)
- [Yapılandırma ve durum sistemini ASP.NET izleme özelleştirme](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [SSS - durum ASP.NET 2. 0'ı izleme](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Nasıl yapılır: Sistem durumu izleme bildirimlerini e-posta Gönder](https://msdn.microsoft.com/library/ms227553.aspx)
- [Nasıl yapılır: ASP.NET Durum İzleme kullanma](https://msdn.microsoft.com/library/ms998306.aspx)
- [ASP.NET izleme sistem durumu](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

> [!div class="step-by-step"]
> [Önceki](processing-unhandled-exceptions-vb.md)
> [İleri](logging-error-details-with-elmah-vb.md)
