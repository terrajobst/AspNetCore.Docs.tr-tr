---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: "SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: Web.Config dosyası dönüşümleri - 3 / 12 | Microsoft Docs"
author: tdykstra
description: "Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Visual Stu kullanarak bir SQL Server Compact veritabanı içeren web uygulama projesi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: ed78b55d2b0315cf428f137c56ad85b29a95e1c5
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanılarak Compact ile ASP.NET Web uygulaması dağıtma: Web.Config dosyası dönüşümleri - 3 / 12
====================
by [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesi indirme](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğreticiler dizi nasıl dağıtacağınız gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC kullanılarak bir SQL Server Compact veritabanı içeren web uygulama projesi. Web yayımlama güncelleştirmesi yüklerseniz, Visual Studio 2010 de kullanabilirsiniz. Seri giriş için bkz: [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC yayımlandıktan sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümleri SQL Server Compact dışında dağıtma gösterir ve Azure App Service Web Apps için dağıtılacağı gösterilmiştir bir öğretici için bkz: [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğretici değiştirme işlemini otomatikleştirmek gösterilmiştir *Web.config* dosya farklı bir hedef ortamlara dağıttığınızda. Çoğu uygulama ayarlarına sahip *Web.config* uygulama dağıtıldığında, farklı dosya. Bu değişiklik yapmadan sürecini otomatik hale getirme dağıttığınız her zaman, bu yorucu olur ve hataya bunları el ile yapmak zorunda kalmaktan tutar.

Anımsatıcı: bir hata iletisi alırsınız veya öğreticide ilerlerken bir şey işe yaramazsa, kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Parametreler Web karşı Web.config dönüşümleri dağıtma

Değiştirme işlemini otomatikleştirmek için iki yolla *Web.config* dosya ayarlarını: [Web.config dönüşümleri](https://msdn.microsoft.com/library/dd465326.aspx) ve [Web dağıtımı parametreleri](https://msdn.microsoft.com/library/ff398068.aspx). A *Web.config* dönüşüm dosyasını içeren nasıl değiştirileceğini belirten XML biçimlendirme *Web.config* dosya dağıtıldığında. Belirli farklı değişiklikleri derleme yapılandırmaları ve özel yayımlama profillerini belirtebilirsiniz. Hata ayıklama ve yayın varsayılan derleme yapılandırmaları olan ve özel derleme yapılandırmaları oluşturabilirsiniz. Bir yayımlama profili, hedef ortam için genellikle karşılık gelir. (Profillerinde yayımlama hakkında daha fazla bilgi edineceksiniz [IIS'ye bir Test ortamı olarak dağıtma](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) öğretici.)

Web dağıtım parametrelerini, birçok farklı türde bulunan ayarlar dahil olmak üzere, dağıtımı sırasında yapılandırılmalıdır ayarlarını belirtmek için kullanılabilir *Web.config* dosyaları. Belirtmek için kullanıldığında *Web.config* dosya değişiklikleri Web dağıtım parametrelerini ayarlamak için daha karmaşık, ancak dağıttığınız kadar ayarlanacak değer bilmiyorsanız, yararlı olur. Örneğin, bir kuruluş ortamında oluşturabilirsiniz bir *dağıtım paketi* ve BT departmanı üretimde yüklemek için bir kişiye verin ve bu kişiye bağlantı dizeleri veya olmayan bir parola girmeniz mümkün olması gerekir bilirsiniz.

Bu öğretici kapsayan senaryosu için için yapılması gereken her şeyi bildiğiniz *Web.config* dosya Web dağıtımı parametrelerini kullanmak gerek yoktur. Kullanılan yapı yapılandırmasına bağlı olarak farklı ve bazı kullanılan yayımlama profili bağlı olarak farklı bazı dönüştürmeler yapılandıracaksınız.

## <a name="creating-transformation-files-for-publish-profiles"></a>Dönüştürme dosyaları için yayımlama profilleri oluşturma

İçinde **Çözüm Gezgini**, genişletin *Web.config* görmek için *Web.Debug.config* ve *Web.Release.config* dönüştürme dosyaları iki varsayılan derleme yapılandırmaları için varsayılan olarak oluşturulur.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Özel derleme yapılandırmaları için dönüştürme dosyaları Web.config dosyasını sağ tıklayıp seçerek oluşturabilirsiniz **eklemek Config dönüştürür** ve bağlam menüsünden, ancak bu öğretici için yapmanız gerekmez.

Dağıtım hedefi yerine yapı yapılandırması ilgili değişiklikleri yapılandırmak için iki dönüştürme dosya'ı daha gerekir. Bu tür bir ayar tipik örneğidir üretim karşı test için farklı bir WCF uç noktadır. Sonraki öğreticilerde oluşturacaksınız yayımlama profillerini ihtiyacınız şekilde Test ve üretim, adlandırılmış bir *Web.Test.config* dosyası ve bir *Web.Production.config* dosya.

Yayımlama profilleri bağlı dönüştürme dosyaları el ile oluşturulması gerekir. İçinde **Çözüm Gezgini**, ContosoUniversity projesine sağ tıklatın ve **klasörü Windows Gezgini'nde Aç**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

İçinde **Windows Explorer**seçin *Web.Release.config* dosya, dosyayı kopyalayın ve ardından iki kopyasını yapıştırın. Bu kopyalar yeniden adlandırma *Web.Production.config* ve *Web.Test.config*, ardından Kapat **Windows Explorer**.

İçinde **Çözüm Gezgini**, tıklatın **yenileme** yeni dosyaları görmek için.

Yeni dosyalar, sağ tıklatın ve ardından seçin **Proje Ekle** bağlam menüsünde.

![Projeye dahil olmak üzere Test ve üretim yapılandırma dosyaları](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Bu dosyalar dağıtılmasını önlemek için bunları seçin **Çözüm Gezgini**ve ardından **özellikleri** penceresi değişiklik **yapı eylemi** özelliğinden**İçerik** için **hiçbiri**. (Derleme yapılandırmalarını temel alan dönüştürme dosyaları otomatik olarak dağıtması engellenir.)

Girmek artık hazırsınız *Web.config* dönüşümleri içine *Web.config* dönüştürme dosyaları.

## <a name="limiting-error-log-access-to-administrators"></a>Yöneticiler için hata günlüğüne erişimi sınırlandırma

Uygulama çalışırken bir hata olduğundan, bir genel hata sayfası sistem tarafından oluşturulan hata sayfası yerine uygulama görüntüler ve onu kullanıyorsa [Elmah NuGet paketi](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) hata günlüğü ve raporlama için. `customErrors` Öğesinde *Web.config* dosyası hata sayfası belirtir:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Hata sayfası görmek için geçici olarak değiştirmek `mode` özniteliği `customErrors` öğesinden "Açık" için "RemoteOnly" ve Visual Studio uygulamayı çalıştırın. Geçersiz bir URL isteyerek hataya neden *Studentsxxx.aspx*. Bir "IIS tarafından oluşturulan sayfa bulunamadı" hata sayfası yerine gördüğünüz *GenericErrorPage.aspx* sayfası.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Hata günlüğünü görmek için URL'sindeki her şeyi sonra bağlantı noktası numarası ile Değiştir *elmah.axd* (örneğin, ekran görüntüsündeki `http://localhost:51130/elmah.axd`) ve Enter tuşuna basın:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Ayarlamayı unutmayın `customErrors` tamamladığınızda "RemoteOnly" moduna geri öğesi.

Geliştirme bilgisayarınızda hata günlüğü sayfasında, ancak bir güvenlik riski doğurur üretimde ücretsiz erişime izin vermek uygundur. Üretim site için bir dönüşüm yapılandırarak yalnızca Yöneticiler için hata günlüğüne erişimi kısıtlayan bir yetkilendirme kuralı ekleyebilirsiniz *Web.Production.config* dosya.

Açık *Web.Production.config* ve yeni bir ekleme `location` öğesi açtıktan hemen sonra `configuration` , aşağıda gösterildiği gibi etiketi. (Yalnızca eklediğinizden emin olun `location` öğesi ve değil yalnızca bazı içerik sağlamak için burada gösterilen çevresindeki işaretleme.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

`Transform` "Ekle" özniteliğinin değeri, bu neden olan `location` varolan herhangi bir eşdüzeyi eklenecek öğe `location` öğelerinde *Web.config* dosya. (Zaten bir olduğundan `location` yetkilendirme belirtir öğesi kuralları için **güncelleştirme krediler** sayfası.) Dağıtımdan sonra üretim sitesini test ettiğinizde, bu yetkilendirme kuralı etkin olduğunu doğrulamak için test edeceksiniz.

Bu kod eklemek zorunda kalmamak için test ortamında hata günlüğü erişimi kısıtlamak yok *Web.Test.config* dosya.

> [!NOTE] 
> 
> **Güvenlik Notu** hiçbir zaman bir üretim uygulamasında genel hata ayrıntıları görüntülemek veya bu bilgileri ortak bir yerde saklayın. Saldırganlar, bir sitedeki güvenlik açıkları bulmak için hata bilgilerini kullanabilirsiniz. Kendi uygulamanızı ELMAH kullanırsanız, güvenlik riskleri en aza indirmek için ELMAH içinde yapılandırılabilir yolları araştırmaya emin olun. Bu öğretici ELMAH örnekte önerilen bir yapılandırma kabul edilmemelidir. Uygulama dosyalarında oluşturabilmek için bir klasör nasıl ele alınacağını göstermek için seçilmiş bir örnektir.


## <a name="setting-an-environment-indicator"></a>Bir ortam gösterge ayarlama

Yaygın bir senaryo yapmaktır *Web.config* dosya dağıttığınız her ortamda farklı ayarlar. Örneğin, bir WCF Hizmeti çağıran bir uygulama test ve üretim ortamlarında farklı bir uç gerekebilir. Contoso University uygulama bu tür bir ayar da içerir. Bu ayar, geliştirme, test ve üretim gibi olduğunuz hangi ortamı bildiren görünür bir gösterge bir sitenin sayfalarında denetler. Ayar değeri uygulama "(Geliştirme)" ekleyeceği olup olmadığını belirler veya "(Test)" ana başlıkta *Site.Master* ana sayfa:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

Uygulama üretimde çalışırken ortamı göstergesi atlanır.

Contoso University web sayfaları kümesinde bir değer okuma `appSettings` içinde *Web.config* uygulamanın çalıştığı hangi ortamını belirlemek için dosya:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

Değer, "üretim ortamında test ortamı ve"Ürün"Test" olmalıdır.

Açık *Web.Production.config* ve ekleme bir `appSettings` öğesi açılış etiketinde hemen önce `location` daha önce eklediğiniz öğe:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

`xdt:Transform` Öznitelik değeri "SetAttributes" var olan bir öğe, öznitelik değerleri değiştirmek için bu dönüşüm amacı olduğunu gösterir *Web.config* dosya. `xdt:Locator` Öznitelik değeri "Match(key)" değiştirilecek öğenin bir olduğunu gösterir, `key` öznitelik eşleşmeleri `key` burada belirtilen öznitelik. Yalnızca diğer öznitelik `add` öğesi `value`, ve ne değiştirilmiş olan dağıtılmış içinde *Web.config* dosya. Bu kod neden `value` özniteliği `Environment` `appSettings` "Ürün" ayarlanması öğe *Web.config* üretime dağıtılmadan dosya.

Ardından, aynı değişikliği uygulamak *Web.Test.config* dosya kümesi dışında `value` "Ürün" yerine "test". İşiniz bittiğinde, `appSettings` bölümüne *Web.Test.config* aşağıdaki gibi görünür:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Hata ayıklama modunu devre dışı bırakma

Yayın derlemesi için etkin hata ayıklama hangi ortam bağımsız olarak dağıttığınız istemezsiniz. Varsayılan olarak *Web.Release.config* dönüştürme dosyası kaldırır koduyla otomatik olarak oluşturulan `debug` özniteliğini `compilation` öğe:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

`Transform` Özniteliği nedenler `debug` özniteliği atlanacak dağıtılan gelen *Web.config* yayın derlemesi dağıtmak her dosya.

Yayın dönüşüm dosyasına kopyalayarak oluşturduğundan bu aynı dönüştürmeyi Test ve üretim dönüştürme dosya sayısıdır. Var. Yinelenen ihtiyacınız yoktur, böylece açık her bu dosyalar, Kaldır **derleme** öğesi ve her dosyayı kaydedip kapatın.

## <a name="setting-connection-strings"></a>Bağlantı dizelerini ayarlama

Bağlantı dizeleri yayımlama profilinde belirtebildiğinizden çoğu durumda, bağlantı dizesi dönüşümleri ayarlamak gerekmez. Ancak bir SQL Server Compact veritabanı dağıtırken bir özel durum olan ve hedef sunucuda veritabanını güncelleştirmek için Entity Framework Code First Migrations kullanıyorsanız. Bu senaryo için sunucuda veritabanı şeması güncelleştirmek için kullanılacak bir ek bağlantı dizesi belirtmeniz gerekir. Bu dönüşüm ayarlamak için ekleme bir  **&lt;connectionStrings&gt;**  öğesi açtıktan hemen sonra  **&lt;yapılandırma&gt;**  hem de etiketi *Web.Test.config* ve *Web.Production.config* dönüştürme dosyaları:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

`Transform` Özniteliği belirtir. Bu bağlantı dizesi eklenecek *connectionStrings* dağıtılan öğesinde *Web.config* dosya. (Yayımlama işlemi bu ek bağlantı dizesi otomatik olarak, yoksa oluşturur, ancak varsayılan olarak **providerName** özniteliği kümesine `System.Data.SqlClient`, hangi çalışmıyor olmayan SQL Server Compact için. Bağlantı dizesi el ile ekleyerek, dağıtım işlemi yanlış sağlayıcı adı ile bir bağlantı dizesi öğesini oluşturmaktan tutun.)

Şimdi tüm belirttiğiniz *Web.config* üretim ve test etmek için Contoso University uygulama dağıtmak için gereken dönüşümler. Aşağıdaki öğreticide, proje özelliklerini ayarlama gerektiren dağıtım kurulum görevlerini ilgilenebilmek.

## <a name="more-information"></a>Daha fazla bilgi

Bu öğretici kapsamında konular hakkında daha fazla bilgi için Web.config dönüşümü senaryoda bkz [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/library/bb386521.aspx).

>[!div class="step-by-step"]
[Önceki](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
[sonraki](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
