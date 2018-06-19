---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: Web.config dosyası dönüşümleri | Microsoft Docs'
author: tdykstra
description: Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcısı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 77ed0d8b2fe85adb009a3f4759030b7fba8fb9d7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880511"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: Web.config dosyası dönüşümleri
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici seri nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seri hakkında daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğretici değiştirme işlemini otomatikleştirmek gösterilmiştir *Web.config* dosya farklı bir hedef ortamlara dağıttığınızda. Çoğu uygulama ayarlarına sahip *Web.config* uygulama dağıtıldığında, farklı dosya. Bu değişiklik yapmadan sürecini otomatik hale getirme dağıttığınız her zaman, bu yorucu olur ve hataya bunları el ile yapmak zorunda kalmaktan tutar.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Web dağıtımı parametreleri karşı Web.config dönüşümleri

Değiştirme işlemini otomatikleştirmek için iki yolla *Web.config* dosya ayarlarını: [Web.config dönüşümleri](https://msdn.microsoft.com/library/dd465326.aspx) ve [Web dağıtımı parametreleri](https://msdn.microsoft.com/library/ff398068.aspx). A *Web.config* dönüşüm dosyasını içeren nasıl değiştirileceğini belirten XML biçimlendirme *Web.config* dosya dağıtıldığında. Belirli farklı değişiklikleri derleme yapılandırmaları ve özel yayımlama profillerini belirtebilirsiniz. Hata ayıklama ve yayın varsayılan derleme yapılandırmaları olan ve özel derleme yapılandırmaları oluşturabilirsiniz. Bir yayımlama profili, hedef ortam için genellikle karşılık gelir. (Profillerinde yayımlama hakkında daha fazla bilgi edineceksiniz [IIS'ye bir Test ortamı olarak dağıtma](deploying-to-iis.md) öğretici.)

Web dağıtım parametrelerini, birçok farklı türde bulunan ayarlar dahil olmak üzere, dağıtımı sırasında yapılandırılmalıdır ayarlarını belirtmek için kullanılabilir *Web.config* dosyaları. Belirtmek için kullanıldığında *Web.config* dosya değişiklikleri Web dağıtım parametrelerini ayarlamak için daha karmaşık, ancak dağıttığınız kadar ayarlanacak değer bilmiyorsanız, yararlı olur. Örneğin, bir kuruluş ortamında oluşturabilirsiniz bir *dağıtım paketi* ve BT departmanı üretimde yüklemek için bir kişiye verin ve bu kişiye bağlantı dizeleri veya olmayan bir parola girmeniz mümkün olması gerekir bilirsiniz.

Bu öğretici seri kapsayan senaryo için önceden için yapılması gereken her şeyi bildiğiniz *Web.config* dosya Web dağıtımı parametrelerini kullanmak gerek yoktur. Kullanılan yapı yapılandırmasına bağlı olarak farklı ve bazı kullanılan yayımlama profili bağlı olarak farklı bazı dönüştürmeler yapılandıracaksınız.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Azure'da Web.config ayarları belirtme

Varsa *Web.config* değiştirmek istediğiniz dosya ayarları olan `<connectionStrings>` veya `<appSettings>` öğesi ve Azure App Service'te Web uygulamalarını dağıtıyorsanız, değişiklikleri sırasında otomatikleştirmek için başka bir seçeneğiniz vardır. dağıtımı. Azure üzerinde etkili olmasını istediğiniz ayarları girebilirsiniz **yapılandırma** management portal sayfasında web uygulamanız için (aşağı kaydırın **uygulama ayarları** ve **bağlantı dizeleri**  bölümler). Projeyi dağıttığınızda, Azure değişiklikleri otomatik olarak uygular. Daha fazla bilgi için bkz: [Windows Azure Web siteleri: nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Varsayılan dönüştürme dosyaları

İçinde **Çözüm Gezgini**, genişletin *Web.config* görmek için *Web.Debug.config* ve *Web.Release.config* dönüştürme dosyaları iki varsayılan derleme yapılandırmaları için varsayılan olarak oluşturulur.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Özel derleme yapılandırmaları için dönüştürme dosyaları Web.config dosyasını sağ tıklayıp seçerek oluşturabilirsiniz **eklemek Config dönüştüren** bağlam menüsünden. Bu öğretici için yapmanız gerekmez ve hiçbir özel derleme yapılandırmaları oluşturmadınız çünkü menü seçeneği devre dışıdır.

Daha sonra üç daha fazla dönüştürme dosyaları, her biri için test hazırlama oluşturacağınız ve üretim yayımlama profilleri. Hedef ortamda bağımlı olduğundan, bir yayımlama profili dönüşüm dosyasında işleyen bir ayar tipik örneğidir üretim karşı test için farklı bir WCF uç noktadır. Bunlar birlikte Git yayımlama profilleri oluşturduktan sonra sonraki öğreticilerde profili dönüştürme dosyaları Yayımla oluşturacaksınız.

## <a name="disable-debug-mode"></a>Hata ayıklama modunu devre dışı bırak

Hedef ortamı yerine yapı yapılandırmasına bağlıdır bir ayar örneği `debug` özniteliği. Yayın derlemesi için genellikle devre dışı hata ayıklama hangi ortam bağımsız olarak dağıttığınız istiyor. Bu nedenle, varsayılan olarak Visual Studio Proje şablonları oluşturma *Web.Release.config* dönüştürme kaldırır kod dosyalarıyla `debug` özniteliğini `compilation` öğesi. Varsayılan işte *Web.Release.config*: kodda içerdiği kılınmıştır bazı örnek dönüşüm kodu yanı sıra `compilation` kaldırır öğesi `debug` özniteliği:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

`xdt:Transform="RemoveAttributes(debug)"` Özniteliği belirtir, istediğiniz `debug` kaldırılmasını özniteliği `system.web/compilation` dağıtılan öğesinde *Web.config* dosya. Yayın derlemesi dağıtmak her zaman bu yapılır.

## <a name="limit-error-log-access-to-administrators"></a>Yöneticiler hata günlüğü erişimini sınırlama

Uygulama çalışırken bir hata olduğundan, bir genel hata sayfası sistem tarafından oluşturulan hata sayfası yerine uygulama görüntüler ve onu kullanıyorsa [Elmah NuGet paketi](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) hata günlüğü ve raporlama için. `customErrors` Uygulama öğesinde *Web.config* dosyası hata sayfası belirtir:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Hata sayfası görmek için geçici olarak değiştirmek `mode` özniteliği `customErrors` öğesinden "Açık" için "RemoteOnly" ve Visual Studio uygulamayı çalıştırın. Geçersiz bir URL isteyerek hataya neden *Studentsxxx.aspx*. IIS tarafından oluşturulan "kaynak bulunamadı" hatası yerine sayfasında, gördüğünüz *GenericErrorPage.aspx* sayfası.

![Hata sayfası](web-config-transformations/_static/image2.png)

Hata günlüğünü görmek için URL'sindeki her şeyi sonra bağlantı noktası numarası ile Değiştir *elmah.axd* (örneğin, `http://localhost:51130/elmah.axd`) ve Enter tuşuna basın:

![ELMAH sayfası](web-config-transformations/_static/image3.png)

Ayarlamayı unutmayın `customErrors` tamamladığınızda "RemoteOnly" moduna geri öğesi.

Geliştirme bilgisayarınızda hata günlüğü sayfasında, ancak bir güvenlik riski doğurur üretimde ücretsiz erişime izin vermek uygundur. Yöneticiler için hata günlüğüne erişimi kısıtlayan bir yetkilendirme kuralı eklemek ve kısıtlama, çalıştığından emin olmak için üretim site için istediğiniz test ve ayrıca hazırlama istiyor. Bu nedenle bu yayın derlemesi dağıtmak her zaman ve ait olduğunu şekilde uygulamak istediğiniz başka bir değişikliktir *Web.Release.config* dosya.

Açık *Web.Release.config* ve yeni bir ekleme `location` öğesi kapatmadan önce hemen `configuration` , aşağıda gösterildiği gibi etiketi.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

`Transform` "Ekle" özniteliğinin değeri, bu neden olan `location` varolan herhangi bir eşdüzeyi eklenecek öğe `location` öğelerinde *Web.config* dosya. (Zaten bir olduğundan `location` yetkilendirme belirtir öğesi kuralları için **güncelleştirme krediler** sayfası.)

Artık bunu doğru şekilde kodlanmış emin olmak için dönüştürme önizleyebilirsiniz.

İçinde **Çözüm Gezgini**, sağ *Web.Release.config* tıklatıp **Önizleme dönüştürme**.

![Önizleme dönüştürme menüsü](web-config-transformations/_static/image4.png)

Geliştirme gösteren bir sayfa açar *Web.config* solda ve hangi dosya dağıtılan *Web.config* dosya görünür gibi sağ tarafta vurgulanan değişikliklerle.

![Hata ayıklama dönüştürme önizlemesi](web-config-transformations/_static/image5.png)

![Konum dönüştürme önizlemesi](web-config-transformations/_static/image6.png)

(Önizlemede yazma kaydetmedi bazı ek değişiklikler dönüştüren görebilirsiniz: Bunlar genellikle işlevselliğini etkilemez boşluk kaldırılmasını içerir.)

Dağıtımdan sonra site test ettiğinizde, Yetkilendirme kuralının etkin olduğunu doğrulamak için de test edeceksiniz.

> [!NOTE] 
> 
> **Güvenlik Notu** hiçbir zaman bir üretim uygulamasında genel hata ayrıntıları görüntülemek veya bu bilgileri ortak bir yerde saklayın. Saldırganlar, bir sitedeki güvenlik açıkları bulmak için hata bilgilerini kullanabilirsiniz. Kendi uygulamanızı ELMAH kullanırsanız, güvenlik riskleri en aza indirmek için ELMAH yapılandırın. Bu öğretici ELMAH örnekte önerilen bir yapılandırma kabul edilmemelidir. Uygulama dosyalarında oluşturabilmek için bir klasör nasıl ele alınacağını göstermek için seçilmiş bir örnektir. Daha fazla bilgi için bkz: [ELMAH uç noktası güvenli hale getirme](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>İçinde ele alacağız bir ayar yayımlama profili dönüştürme dosyaları

Yaygın bir senaryo yapmaktır *Web.config* dosya dağıttığınız her ortamda farklı ayarlar. Örneğin, bir WCF Hizmeti çağıran bir uygulama test ve üretim ortamlarında farklı bir uç gerekebilir. Contoso University uygulama bu tür bir ayar da içerir. Bu ayar, geliştirme, test ve üretim gibi olduğunuz hangi ortamı bildiren görünür bir gösterge bir sitenin sayfalarında denetler. Ayar değeri uygulama "(Geliştirme)" ekleyeceği olup olmadığını belirler veya "(Test)" ana başlıkta *Site.Master* ana sayfa:

![Ortam göstergesi](web-config-transformations/_static/image7.png)

Uygulama hazırlık veya üretim çalışırken ortamı göstergesi atlanır.

Contoso University web sayfaları kümesinde bir değer okuma `appSettings` içinde *Web.config* uygulamanın çalıştığı hangi ortamını belirlemek için dosya:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Değer "Test" test ortamında olması ve "hazırlama ve üretim için üretim" gerekir.

Dönüşüm dosyası aşağıdaki kod bu dönüşüm gerçekleştireceksiniz:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

`xdt:Transform` Öznitelik değeri "SetAttributes" var olan bir öğe, öznitelik değerleri değiştirmek için bu dönüşüm amacı olduğunu gösterir *Web.config* dosya. `xdt:Locator` Öznitelik değeri "Match(key)" değiştirilecek öğenin bir olduğunu gösterir, `key` öznitelik eşleşmeleri `key` burada belirtilen öznitelik. Yalnızca diğer öznitelik `add` öğesi `value`, ve ne değiştirilmiş olan dağıtılmış içinde *Web.config* dosya. Burada gösterilen neden kodu `value` özniteliği `Environment` `appSettings` "Test" ayarlanması öğe *Web.config* dağıtılmış dosya.

Bu dönüştürme, henüz oluşturmadınız yayımlama profili dönüştürme dosyalarında aittir. Oluşturun ve test, hazırlama ve üretim ortamları için yayımlama profili oluşturduğunuzda, bu değişikliği uygulamak dönüştürme dosyaları güncelleştirmeniz. Bu gerçekleştirirsiniz [IIS dağıtmak](deploying-to-iis.md) ve [üretime dağıtabilirsiniz](deploying-to-production.md) öğreticileri.

> [!NOTE]
> Bu ayar çünkü `<appSettings>` öğesi, Azure uygulama hizmeti görmek Web uygulamalarında dağıtıyorsanız dönüştürme belirtmek için başka bir alternatif olan [Azure belirten Web.config ayarlarında](#watransforms) önceki Bu konuda.


## <a name="setting-connection-strings"></a>Bağlantı dizelerini ayarlama

Varsayılan dönüştürme dosyası bir bağlantı dizesi güncelleştirmek nasıl oluşturulduğunu gösteren bir örnek içerse de, yayımlama profilinde bağlantı dizeleri belirtebildiğinizden çoğu durumda, bağlantı dizesi dönüşümleri ayarlamak gerekmez. Bu gerçekleştirirsiniz [IIS dağıtmak](deploying-to-iis.md) ve [üretime dağıtabilirsiniz](deploying-to-production.md) öğreticileri.

## <a name="summary"></a>Özet

İle kadar şimdi yaptığınız *Web.config* yayımlama profilleri oluşturmak ve dağıtılmış Web.config dosyasında ne olacağını önizlemesini gördünüz önce dönüşümler.

![Konum dönüştürme önizlemesi](web-config-transformations/_static/image8.png)

Aşağıdaki öğreticide, proje özelliklerini ayarlama gerektiren dağıtım kurulum görevlerini ilgilenebilmek.

## <a name="more-information"></a>Daha fazla bilgi

Bu öğretici kapsamında konular hakkında daha fazla bilgi için bkz: [dağıtımı sırasında hedef Web.config veya app.config dosyasını ayarları değiştirmek için Web.config kullanarak dönüşümleri](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) için Web dağıtımı içerik haritası içindeki Visual Studio ve ASP.NET.

> [!div class="step-by-step"]
> [Önceki](preparing-databases.md)
> [sonraki](project-properties.md)
