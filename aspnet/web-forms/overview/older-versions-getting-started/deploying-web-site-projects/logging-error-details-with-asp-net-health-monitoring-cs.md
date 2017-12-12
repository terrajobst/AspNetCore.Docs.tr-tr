---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
title: "ASP.NET Durum İzleme (C#) hata ayrıntıları günlüğü | Microsoft Docs"
author: rick-anderson
description: "Microsoft'un sistem durumu izleme sistemi işlenmeyen özel durumlar dahil olmak üzere çeşitli web olayların günlüğe kaydedilmesi için kolay ve özelleştirilebilir bir yol sağlar. Bu öğreticide k açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: b1abb452-642a-4ff3-8504-37b85590ff79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs
msc.type: authoredcontent
ms.openlocfilehash: 06f8b57c8973fff5c07e82100cd43f6757d454f9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="logging-error-details-with-aspnet-health-monitoring-c"></a>Hata ayrıntılarını günlüğü ASP.NET Durum İzleme (C#)
====================
tarafından [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Kodu indirme](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_13_CS.zip) veya [PDF indirin](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial13_HealthMonitoring_cs.pdf)

> Microsoft'un sistem durumu izleme sistemi işlenmeyen özel durumlar dahil olmak üzere çeşitli web olayların günlüğe kaydedilmesi için kolay ve özelleştirilebilir bir yol sağlar. Bu öğreticide, bir veritabanına işlenmeyen özel durumları günlüğe kaydetmek için ve e-posta iletisine aracılığıyla geliştiricilerin bildirmek için sistem izleme durumunun dökümünü ayarı aracılığıyla açıklanmaktadır.


## <a name="introduction"></a>Giriş

Günlük bir dağıtılmış uygulama sistem durumu izleme ve ortaya çıkabilecek sorunları tanılamak için yararlı araçtır. Dağıtılan bir uygulama ortaya ve böylece bunlar düzeltilebilir hataları günlüğe kaydetmek özellikle önemlidir. `Error` Olayı bir ASP.NET uygulamasındaki; işlenmeyen bir özel durum oluştuğunda [önceki öğretici](processing-unhandled-exceptions-cs.md) hata geliştiricisine bildirin ve içinbirolayişleyicisioluşturarakayrıntılarınıoturumnasıloluşturulacağınıgösterir`Error` olay. Ancak, oluşturma bir `Error` hatanın ayrıntıları oturum ve geliştirici işleyicidir gereksiz, bu görev ASP tarafından gerçekleştirilebileceği gibi. NET'in *durum sistemini izleme*.

Durumunu sistem izleme ASP.NET 2.0 ile sunulan ve dağıtılan bir ASP.NET uygulama sağlığını izlemek için uygulama veya isteğin ömrü sırasında meydana gelen olayları günlüğe kaydetmeyi tarafından tasarlanmıştır. Durumunu sistem izleme tarafından kaydedilen olayları denir *durum olaylarını izleme* veya *Web olayları*ve içerir:

- Ne zaman bir uygulama başlatıldığında veya durdurulduğunda gibi uygulama ömür olayları
- Güvenlik olayları dahil olmak üzere, oturum açma denemesi başarısız oldu ve URL yetkilendirme isteği başarısız oldu
- Uygulama hataları dahil olmak üzere, özel durumlar, özel durumlar, istek doğrulama özel durumlar ve hata diğer türleri arasında belirli bir derleme ayrıştırma görünüm durumu işlenmemiş.

Bir sistem durumu izleme olayı olduğunda, herhangi bir sayıda için kaydedilebilir belirtilen *oturum kaynakları*. Durumunu sistem izleme için Windows olay günlüğü veya diğerlerinin yanı sıra bir e-posta iletisi yoluyla bir Microsoft SQL Server veritabanı Web olayları oturum günlük kaynakları ile birlikte gelir. Kendi günlük kaynakları da oluşturabilirsiniz.

Durumunu sistem izleme günlükleri, kullanılan, günlük kaynakları birlikte olayları tanımlanan `Web.config`. Yapılandırma biçimlendirme birkaç satırıyla durumunu bir veritabanına tüm işlenmeyen özel durumları günlüğe kaydetmek için ve özel durumun e-posta ile bilgilendirmek için izleme kullanabilirsiniz.

## <a name="exploring-the-health-monitoring-systems-configuration"></a>Durumunu sistem yapılandırmasının izleme keşfetme

Sistemin davranış izlemeyi sistem bulunan yapılandırma bilgileri tarafından tanımlanan [ `<healthMonitoring>` öğesi](https://msdn.microsoft.com/en-us/library/2fwh2ss9.aspx) içinde `Web.config`. Bu yapılandırma bölümü yanı sıra, aşağıdaki üç önemli bilgi parçalarını tanımlar:

1. Olayları izleme sistem durumu, gerçekleşti, oturum açmış olmanız,
2. Günlük kaynakları ve
3. Her sistem durumu (1)'de tanımlanan olay izleme günlüğü kaynakları nasıl eşlenir (2) tanımlanır.

Bu bilgiler üç alt yapılandırma öğeleri ile belirtilmiştir: [ `<eventMappings>` ](https://msdn.microsoft.com/en-us/library/yc5yk01w.aspx), [ `<providers>` ](https://msdn.microsoft.com/en-us/library/zaa41kz1.aspx), ve [ `<rules>` ](https://msdn.microsoft.com/en-us/library/fe5wyxa0.aspx)sırasıyla.

Varsayılan durum sistem yapılandırma bilgileri izleme bulunabilir `Web.config` dosyasını `%WINDIR%\Microsoft.NET\Framework\version\CONFIG` klasör. Bu varsayılan yapılandırma bilgilerini okumanızdır kaldırılan bazı biçimlendirme ile aşağıda verilmiştir:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample1.xml)]

Sistem durumu izleme olaylarının tanımlanmış `<eventMappings>` durum olaylarını izleme sınıfına bir insan kolay ad verir öğesi. Yukarıdaki biçimlendirmede `<eventMappings>` öğesi atar İnsan kolay adı "Tüm hataları" türünde olayları izleme durumu `WebBaseErrorEvent` ve adı "hata denetimleri" türünde olayları izleme sistem durumu için `WebFailureAuditEvent`.

`<providers>` Öğesi İnsan kolay ad vermek ve tüm günlük kaynak özgü yapılandırma bilgilerini belirtme günlük kaynaklarını tanımlar. İlk `<add>` öğe, belirtilen durum kullanarak olayları izleme günlükleri "EventLogProvider" Sağlayıcı tanımlar `EventLogWebEventProvider` sınıfı. `EventLogWebEventProvider` Sınıfı için Windows olay günlüğü olayı günlüğe kaydeder. İkinci `<add>` öğesi tanımlayan bir Microsoft SQL Server veritabanına olayları günlüğe kaydeden "SqlWebEventProvider" sağlayıcı `SqlWebEventProvider` sınıfı. Veritabanı bağlantı dizesi "SqlWebEventProvider" yapılandırma belirtir (`connectionStringName`) diğer yapılandırma seçenekleri arasında.

`<rules>` Öğesi eşler belirtilen olayları `<eventMappings>` kaynakları oturum açmak için öğesi `<providers>` öğesi. Varsayılan olarak, ASP.NET web uygulamalarının tüm işlenmeyen özel durumları günlüğe kaydetmek ve hataları için Windows olay günlüğünü denetleyin.

## <a name="logging-events-to-a-database"></a>Bir veritabanına günlük olayları

Durumunu sistem varsayılan yapılandırmasının izleme ekleyerek bir web uygulaması tarafından web uygulaması temelinde özelleştirilebilir bir `<healthMonitoring>` uygulamanın bölümüne `Web.config` dosya. Ek öğeler içerebilir `<eventMappings>`, `<providers>`, ve `<rules>` kullanarak bölümleri `<add>` öğesi. Bir ayar varsayılan yapılandırma kullanımdan kaldırmak için `<remove>` öğesi ya da kullanım `<clear />` tüm varsayılan değerleri bu bölümlerden biri kaldırmak için. Kullanarak bir Microsoft SQL Server veritabanı tüm işlenmeyen özel durumları günlüğe kaydetmek için Kitap incelemeleri web uygulamasına yapılandıralım `SqlWebEventProvider` sınıfı.

`SqlWebEventProvider` Sınıfı durumunu belirtilen bir SQL Server veritabanı için olay izleme günlüklerini ve durum sistemini izleme parçasıdır. `SqlWebEventProvider` Sınıfı bekliyor belirtilen veritabanı adlı bir saklı yordam içerdiğini `aspnet_WebEvent_LogEvent`. Bu saklı yordam olay ayrıntılarını geçirilir ve olay ayrıntılarını depolamanın görevli. İyi haber depolanan oluşturmak gerekmez olan yordamı veya olay ayrıntılarını saklamak için tablo. Bu nesneler kullanarak veritabanı ekleyebileceğiniz `aspnet_regsql.exe` aracı.

> [!NOTE]
> `aspnet_regsql.exe` Aracı ele alınan geri [ *bir Web sitesi, kullanan uygulama hizmetlerini yapılandırma* öğretici](configuring-a-website-that-uses-application-services-cs.md) ASP desteği eklediğimiz zaman. NET uygulama hizmetleri. Sonuç olarak, Kitap incelemeleri Web sitesinin veritabanı zaten içeriyor `aspnet_WebEvent_LogEvent` saklı adlı bir tabloya olay bilgilerini depolar yordamı `aspnet_WebEvent_Events`.


Gerekli saklı yordamı ve tabloyu veritabanınıza eklenmesi olduktan sonra kalan tek şey durumunu tüm işlenmeyen özel durumlar veritabanında oturum izleme istemek üzere. Bu, Web sitenizin için aşağıdaki biçimlendirme ekleyerek gerçekleştirmek `Web.config` dosyası:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample2.xml)]

Sistem durumu izleme yapılandırması biçimlendirme kullanımlar yukarıda `<clear />` önceden tanımlanmış sistem durumu izleme yapılandırma bilgilerini silmek için öğeler `<eventMappings>`, `<providers>`, ve `<rules>` bölümler. Ardından tek bir giriş her bu bölümleri ekler.

- `<eventMappings>` Öğesi "Tüm işlenmeyen bir özel durum oluştuğunda tetiklenir hataları", adlı ilgi olay izleme tek bir sistem durumu tanımlar.
- `<providers>` Öğesi tanımlar "kullanan SqlWebEventProvider" adlı bir tek günlük kaynağı `SqlWebEventProvider` sınıfı. `connectionStringName` Özniteliği "bizim bağlantısının adı olan ReviewsConnectionString için", ayarlandıktan içinde tanımlanan bir dize `<connectionStrings>` bölümü.
- Son olarak, &lt;kuralları&gt; öğesi, "Tüm hataları" olay transpires olduğunda, "SqlWebEventProvider" sağlayıcısını kullanarak günlüğe kaydedileceğini gösterir.

Bu yapılandırma bilgisi tüm işlenmeyen özel durumlar Kitap incelemeleri veritabanında oturum sistem izleme sistem durumu bildirir.

> [!NOTE]
> `WebBaseErrorEvent` Sunucusu hataları için olay gerçekleşti yalnızca; bulunamadığını bildiren bir ASP.NET kaynak için bir istek gibi HTTP hataları için oluşmaz. Bu davranışından farklıdır `HttpApplication` sınıfının `Error` hem sunucu hem de HTTP hataları için tetiklenir olay.


Eylem sistem izleme sistem durumu görmek için Web sitesini ziyaret edin ve bir çalışma zamanı hatası ziyaret ederek oluşturmak `Genre.aspx?ID=foo`. İlgili hata sayfası - özel durum ayrıntıları sarı ekran, (yerel olarak ziyaret ederken) ölüm veya (üretim sitesini ziyaret ederken) özel hata sayfasını görmeniz gerekir. Arka planda durum sistemini izleme veritabanına hata bilgilerini günlüğe. Bir kayıt olmalıdır `aspnet_WebEvent_Events` tablosu (bkz **Şekil 1**); bu kayıt yalnızca oluştu çalışma zamanı hatasıyla ilgili bilgileri içerir.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image2.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image1.png)

**Şekil 1**: hata ayrıntıları için kaydedilmiş `aspnet_WebEvent_Events` tablosu  
([Tam boyutlu görüntüyü görüntülemek için tıklatın](logging-error-details-with-asp-net-health-monitoring-cs/_static/image3.png))

### <a name="displaying-the-error-log-in-a-web-page"></a>Bir Web sayfasında hata günlüğünü görüntüleme

Web sitesinin geçerli yapılandırması ile durum sistemini izleme tüm işlenmeyen özel durumlar veritabanına kaydeder. Bununla birlikte, sistem durumu izleme web sayfası aracılığıyla hata günlüğünü görüntülemek için herhangi bir mekanizma sağlamaz. Ancak, bu bilgileri veritabanından görüntüleyen bir ASP.NET sayfası oluşturabilirsiniz. (Kısa bir süre içinde anlatıldığı gibi e-posta iletisinde size gönderilen hata ayrıntıları için tercih edebilirsiniz.)

Bu tür bir sayfa oluşturursanız, yalnızca yetkili kullanıcılar hata ayrıntılarını görüntülemek izin vermek için adımlar emin olun. Sitenizi kullanıcı hesapları içeriyorsa belirli kullanıcılar ya da roller sayfasına erişimi kısıtlamak için URL yetkilendirme kuralları kullanabilirsiniz. Vermek veya oturum açan kullanıcıyı temel alarak web sayfalarına erişimi kısıtlamak nasıl daha fazla bilgi için bkz my [Web sitesi güvenlik öğreticileri](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

> [!NOTE]
> Sonraki öğretici ELMAH adlı bir alternatif hata günlüğü ve bildirim sistemi araştırır. ELMAH hem de bir web sayfasından ve bir RSS olarak hata günlüğünü görüntülemek için yerleşik bir mekanizma içerir.


## <a name="logging-events-to-e-mail"></a>E-posta olayları günlüğe kaydetme

Durumunu sistem izleme "e-posta iletisine bir olayı günlüğe kaydeder" günlük kaynak sağlayıcısı içerir. Günlüğü kaynağı e-posta ileti gövdesinde veritabanına kaydedilir aynı bilgileri içerir. Belirli bir sistem durumu izleme olayı oluştuğunda bir geliştirici bildirmek için bu günlüğü kaynağı kullanabilirsiniz.

Şimdi bir özel durum olduğunda e-posta aldığımız böylece Web sitesinin yapılandırma gerçekleşir Kitap incelemeleri güncelleştirin. Bunu gerçekleştirmek için üç görevleri gerçekleştirmek ihtiyacımız var:

1. E-posta göndermek için ASP.NET web uygulaması yapılandırın. Bu e-posta iletileri üzerinden nasıl gönderileceğini belirterek gerçekleştirilir `<system.net>` yapılandırma öğesi. E-posta gönderme hakkında daha fazla bilgi için bir ASP.NET uygulamasında iletileri başvurmak [ASP.NET e-posta gönderme](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx) ve [System.Net.Mail SSS](http://systemnetmail.com/).
2. E-posta günlüğü kaynak sağlayıcısı kaydetme `<providers>` öğesi, ve
3. Bir giriş eklemek `<rules>` (2) adımda eklenen günlük kaynak sağlayıcısı "Tüm hataları" olay eşlendiği öğe.

Durumunu sistem izleme iki e-posta günlük kaynak sağlayıcısı sınıfları içerir: `SimpleMailWebEventProvider` ve `TemplatedMailWebEventProvider`. [ `SimpleMailWebEventProvider` Sınıfı](https://msdn.microsoft.com/en-us/library/system.web.management.simplemailwebeventprovider.aspx) e-posta gövdesi çok az özelleştirmesini sağlar ve ayrıntıları olay içeren bir düz metin e-posta iletisi gönderir. İle [ `TemplatedMailWebEventProvider` sınıfı](https://msdn.microsoft.com/en-us/library/system.web.management.templatedmailwebeventprovider.aspx) , oluşturulan biçimlendirmenin için e-posta ileti gövdesi olarak kullanılan bir ASP.NET sayfası belirtin. [ `TemplatedMailWebEventProvider` Sınıfı](https://msdn.microsoft.com/en-us/library/system.web.management.templatedmailwebeventprovider.aspx) kadar içeriğini ve e-posta ileti biçimi üzerinde daha fazla denetim sağlar, ancak e-posta iletisinin gövdesi oluşturur ASP.NET sayfası oluşturmak zorunda biraz daha fazla ön iş gerektirir. Bu öğreticiyi kullanmaya odaklanır `SimpleMailWebEventProvider` sınıfı.

Sistem durumu sistemin izleme güncelleştirme `<providers>` öğesinde `Web.config` için bir günlük kaynak eklenecek dosyası `SimpleMailWebEventProvider` sınıfı:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample3.xml)]

Yukarıdaki biçimlendirme kullanan `SimpleMailWebEventProvider` günlüğü kaynak sağlayıcısı olarak sınıfı ve kolay adı "EmailWebEventProvider" atar. Ayrıca, `<add>` özniteliği Kime gibi ve e-posta iletisinin adreslerinden ek yapılandırma seçenekleri içerir.

E-posta günlüğü kaynağı tanımlanan sonra kalan tek şey "İşlenmeyen özel durumları günlüğe kaydetmek için" Bu kaynağı kullanmak için sistem izleme sistem durumu istemek üzere. Bu yeni bir kural eklenmesiyle gerçekleştirilir `<rules>` bölümü:

[!code-xml[Main](logging-error-details-with-asp-net-health-monitoring-cs/samples/sample4.xml)]

`<rules>` Bölüm şimdi iki kural içerir. "Tüm hatalar için e-posta" adlı birincisini tüm işlenmeyen özel durumlar "EmailWebEventProvider" günlük kaynağına gönderir. Bu kural Web sitesinde hatalarla ilgili ayrıntıları belirtilen gönderme etkisi adresine. "Tüm hatalar için veritabanı" kuralı hata ayrıntıları sitenin veritabanına kaydeder. Sonuç olarak, işlenmeyen bir özel durum ayrıntılarını sitesinde oluştuğunda hem de veritabanında günlüğe ve belirtilen e-posta adresine gönderilen.

**Şekil 2** tarafından oluşturulan e-posta gösterir `SimpleMailWebEventProvider` sınıf ziyaret eden `Genre.aspx?ID=foo`.

[![](logging-error-details-with-asp-net-health-monitoring-cs/_static/image5.png)](logging-error-details-with-asp-net-health-monitoring-cs/_static/image4.png)

**Şekil 2**: hata ayrıntıları, bir e-posta iletisi gönderilir  
([Tam boyutlu görüntüyü görüntülemek için tıklatın](logging-error-details-with-asp-net-health-monitoring-cs/_static/image6.png))

## <a name="summary"></a>Özet

ASP.NET sistem durumu izleme sistemi, dağıtılan web uygulamasının sağlığını izlemek yöneticilerin sağlamak için tasarlanmıştır. Sistem durumu izleme olayları, uygulama durdurduğunda başarıyla sitesinde oturum açtığında gibi bazı eylemler unfold veya işlenmeyen bir özel durum oluştuğunda oluşturulur. Bu olaylar günlük kaynakları herhangi bir sayıda kaydedilebilir. Bu öğretici, bir veritabanına ve e-posta iletisi işlenmeyen özel durumlar ayrıntılarını günlüğe kaydetmek nasıl oluşturulacağını gösterir.

Bu öğretici durumunu işlenmeyen özel durumlar, ancak sistem durumu izleme dağıtılan bir ASP.NET uygulama genel durumunu ölçmek için tasarlanmıştır ve sistem durumu izleme olayları bol içerir olduğunu aklınızda bulundurun ve kaynakları oturum değil için izleme kullanımına odaklanmış Burada incelediniz. Dahası, kendi sistem durumu izleme olayları ve günlük kaynakları oluşturabilirsiniz artırılması gereken ortaya çıkar. Sistem durumu izleme hakkında daha fazla bilgi ilgileniyorsanız okumak için iyi bir ilk adım olan [Erik Reitan](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)'s [durumunu SSS izleme](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx). Başvurun [nasıl yapılır: ASP.NET 2.0 kullanım izleme sistem durumu](https://msdn.microsoft.com/en-us/library/ms998306.aspx).

Mutluluk programlama!

### <a name="further-reading"></a>Daha Fazla Bilgi

Bu öğreticide konular hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [ASP.NET sistem durumu izlemeye genel bakış](https://msdn.microsoft.com/en-us/library/bb398933.aspx)
- [Yapılandırma ve durum sistemini ASP.NET izleme özelleştirme](http://dotnetslackers.com/articles/aspnet/ConfiguringAndCustomizingTheHealthMonitoringSystemOfASPNET.aspx)
- [SSS - durum ASP.NET 2.0 izleme](https://blogs.msdn.com/erikreitan/archive/2006/05/22/603586.aspx)
- [Nasıl yapılır: E-posta bildirimleri izleme sistem durumu için Gönder](https://msdn.microsoft.com/en-us/library/ms227553.aspx)
- [Nasıl yapılır: ASP.NET sistem durumu izleme kullanma](https://msdn.microsoft.com/en-us/library/ms998306.aspx)
- [ASP.NET izleme sistem durumu](http://aspnet.4guysfromrolla.com/articles/031407-1.aspx)

>[!div class="step-by-step"]
[Önceki](processing-unhandled-exceptions-cs.md)
[sonraki](logging-error-details-with-elmah-cs.md)
