---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: Web.Config dosyası dönüşümleri - 12 3 | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Visual Stu'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: a5afcf4d05d6fa51ad70f7e5bdfafd20002f188a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841764"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>SQL Server Visual Studio veya Visual Web Developer kullanarak Compact ile bir ASP.NET Web uygulaması dağıtma: Web.Config dosyası dönüşümleri - 3 12
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Bu öğretici serisinde, nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) Web için Visual Studio 2012 RC veya Visual Studio Express 2012 RC'Yİ'ı kullanarak bir SQL Server Compact veritabanı içeren web uygulaması projesi. Web yayımlama güncelleştirme yüklerseniz, Visual Studio 2010'u kullanabilirsiniz. Serinin bir giriş için bkz [serideki ilk öğreticide](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Visual Studio 2012 RC sürümünden sonra sunulan dağıtım özellikleri gösterir, SQL Server sürümlerinde SQL Server Compact dışında dağıtmayı gösterir ve Azure App Service Web Apps'e dağıtma işlemi gösterilmektedir bir öğretici için bkz. [ASP.NET Web dağıtımı Visual Studio kullanarak](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide değiştirme işlemini otomatik hale getirmek gösterilir *Web.config* dosya farklı bir hedef ortamlara dağıttığınızda. Çoğu uygulama ayarlarına sahip *Web.config* uygulama dağıtıldığında, farklı bir dosya. Bu değişiklikler sürecini otomatik hale getirme dağıttığınız her zaman, hangi can sıkıcı olabilir ve hata yapmaya açık bunları el ile yapmak zorunda kalmasını sağlar.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Web.config dönüşümlerini Web karşı parametreleri dağıtma

Değiştirme işlemini otomatik hale getirmek için iki yolu vardır *Web.config* dosyası ayarları: [Web.config dönüşümlerini](https://msdn.microsoft.com/library/dd465326.aspx) ve [Web dağıtımı parametreleri](https://msdn.microsoft.com/library/ff398068.aspx). A *Web.config* nasıl değiştirileceğini belirtir. XML işaretlemesini dönüşüm dosyası içeren *Web.config* dağıtıldığında dosya. Farklı değişiklikler belirli yapılandırmaları oluşturma ve yayımlama profilleri için özel belirtebilirsiniz. Hata ayıklama ve yayın varsayılan derleme yapılandırmaları olan ve özel yapı yapılandırması oluşturabilirsiniz. Bir yayımlama profili, bir hedef ortam için genellikle karşılık gelir. (Profillerinde yayımlama hakkında daha fazla bilgi edineceksiniz [bir Test ortamı olarak IIS'ye dağıtma](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) öğretici.)

Web dağıtım parametrelerini, birçok farklı türde bulunan ayarlar dahil olmak üzere, dağıtım sırasında yapılandırılması gereken ayarları belirtmek için kullanılabilir *Web.config* dosyaları. Belirtmek için kullanıldığında *Web.config* dosya değişiklikleri, Web dağıtım parametrelerini ayarlamak için daha karmaşık, ancak dağıttığınız kadar ayarlanacak değer bilmediğinizde yararlı olur. Örneğin, bir kuruluş ortamında oluşturabilirsiniz bir *dağıtım paketi* üretim ortamında yüklemek için bir kişi BT departmanı içindeki vermek ve söz konusu kişinin bağlantı dizesi ya da olmayan parolaları girmeniz mümkün olması gerekir bildirin.

Bu öğreticide kapsayan senaryo için için yapılması gereken her şeyi biliyor *Web.config* dosya Web dağıtımı parametrelerini kullanmak gerek yoktur. Kullanılan derleme yapılandırmasına bağlı olarak farklı ve bazı kullanılan yayımlama profili bağlı olarak farklılık gösteren bazı dönüştürmeler yapılandıracaksınız.

## <a name="creating-transformation-files-for-publish-profiles"></a>Yayımlama profilleri için dönüştürme dosyaları oluşturma

İçinde **Çözüm Gezgini**, genişletme *Web.config* görmek için *Web.Debug.config* ve *Web.Release.config* dönüştürme dosyaları iki varsayılan derleme yapılandırmaları için varsayılan olarak oluşturulur.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Özel derleme yapılandırmaları için dönüştürme dosyaları Web.config dosyasına sağ tıklayıp seçerek oluşturabilirsiniz **yapılandırma dönüşümleri Ekle** bağlam menüsünden, ancak bu öğretici için yapmanız gerekmez.

Dağıtım hedefi yerine derleme yapılandırmasını ilgili değişiklikleri yapılandırmak için daha fazla iki dönüşüm dosyası gerekir. Bu tür bir ayarı tipik bir örneği, üretim ve test için farklı bir WCF uç noktadır. Sonraki öğreticilerde oluşturacaksınız yayımlama profilleri böylece, Test ve üretim adlı bir *Web.Test.config* dosyası ve bir *Web.Production.config* dosya.

Yayımlama profilleri bağlıdır dönüştürme dosyaları el ile oluşturulması gerekir. İçinde **Çözüm Gezgini**ContosoUniversity projeye sağ tıklayın ve seçin **klasörü Windows Gezgini'nde Aç**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

İçinde **Windows Explorer**seçin *Web.Release.config* dosya, dosya kopyalayın ve ardından iki kopyasını yapıştırın. Bu kopyaları Yeniden Adlandır *Web.Production.config* ve *Web.Test.config*ardından kapatın **Windows Explorer**.

İçinde **Çözüm Gezgini**, tıklayın **Yenile** yeni dosyaları görmek için.

Yeni dosyaları, sağ tıklayın ve ardından seçin **Proje Ekle** bağlam menüsünde.

![Projede test ve üretim yapılandırma dosyaları dahil](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Bu dosyalar dağıtılmasını önlemek için bunları seçin **Çözüm Gezgini**ve ardından **özellikleri** penceresinde değişiklik **derleme eylemi** özelliğinden**İçerik** için **hiçbiri**. (Derleme yapılandırmalarını temel alan Dönüşüm dosyaları otomatik olarak dağıtmanızı engellenir.)

Girmek artık hazırsınız *Web.config* içine dönüşümleri *Web.config* dönüştürme dosyaları.

## <a name="limiting-error-log-access-to-administrators"></a>Yöneticiler için hata günlüğü erişimi sınırlandırma

Uygulama çalışırken bir hata olduğundan, bir genel hata sayfası sistem tarafından oluşturulan hata sayfası yerine uygulama görüntüler ve kullandığı [Elmah NuGet paketini](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) günlüğe kaydetme ve Raporlama hata. `customErrors` Öğesinde *Web.config* dosyası hata sayfası belirtir:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Hata sayfası görmek için geçici olarak değiştirme `mode` özniteliği `customErrors` "Açık" "RemoteOnly" ve uygulamayı Visual Studio'dan çalışma öğesi. Geçersiz bir URL gibi talep ederek hataya neden *Studentsxxx.aspx*. Bir "IIS tarafından oluşturulan sayfası bulunamadı" hata sayfası yerine gördüğünüz *GenericErrorPage.aspx* sayfası.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Hata günlüğünü görmek için URL'de bulunan her şeyi sonra bağlantı noktası numarasıyla değiştirin *elmah.axd* (örneğin, ekran görüntüsündeki `http://localhost:51130/elmah.axd`) ve Enter tuşuna basın:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Ayarlamayı unutmayın `customErrors` işiniz bittiğinde "RemoteOnly" moduna geri öğesi.

Geliştirme bilgisayarınızda hata günlüğü sayfasında, ancak bir güvenlik riski doğurur üretimde ücretsiz erişim izni vermek uygundur. Üretim sitesi için bir dönüşüm yapılandırarak yalnızca Yöneticiler için hata günlüğüne erişimi kısıtlayan bir yetkilendirme kuralı ekleyebilirsiniz *Web.Production.config* dosya.

Açık *Web.Production.config* ve yeni bir `location` öğesini hemen açılış `configuration` burada gösterildiği gibi etiketleyin. (Yalnızca eklediğinizden emin olun `location` öğesi ve değil yalnızca bazı bağlam sağlamak için aşağıda gösterilen çevreleyen işaretleme.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

`Transform` "Ekle" özniteliğinin değerini neden bu `location` varolan herhangi bir eşdüzeyi eklenecek öğe `location` öğelerinde *Web.config* dosya. (Zaten var. bir `location` yetkilendirme belirten öğesi kuralları için **güncelleştirme KREDİLERİ** sayfası.) Dağıtımdan sonra üretim sitesini test ettiğinizde, bu yetkilendirme kuralı'nın etkin olduğunu doğrulamak için test edeceğiz.

Bu kodu eklemek zorunda kalmamak için test ortamında, hata günlüğü erişimi sınırlamak zorunda değilsiniz *Web.Test.config* dosya.

> [!NOTE] 
> 
> **Güvenlik Notu** hiçbir zaman bir üretim uygulamasında genel hata ayrıntılarını görüntülemek veya bu bilgileri ortak bir yerde saklayın. Saldırganlar, güvenlik açıklarını bir site bulmak için hata bilgilerini kullanabilirsiniz. ELMAH kendi uygulamanıza kullanırsanız, ELMAH hangi güvenlik riskleri en aza indirmek için yapılandırılabilir bir şekilde araştırmanıza emin olun. Bu öğreticide ELMAH örnek önerilen bir yapılandırma kabul edilmemelidir. Uygulama dosyaları oluşturmak için bir klasör ne yapılacağını göstermek için seçilmiştir bir örnektir.


## <a name="setting-an-environment-indicator"></a>Bir ortam göstergesi ayarlama

Sık karşılaşılan bir senaryodur sağlamaktır *Web.config* dosya dağıttığınız her ortamda farklı ayarlar. Örneğin, bir WCF Hizmeti çağıran bir uygulama farklı bir uç nokta test ve üretim ortamlarında gerekebilir. Contoso University uygulama bu tür bir ayarı da içerir. Bu ayar, geliştirme, test veya üretim gibi bulunduğunuz hangi ortamı söyleyen görünür bir gösterge bir sitenin sayfalarında denetler. Uygulama "(Geliştirme)" ekleyeceği ayar değeri belirler veya "(Test)" ana başlıkta *Site.Master* ana sayfa:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

Uygulama, üretim ortamında çalışırken ortam göstergesi atlanır.

Contoso University web sayfaları kümesinde bir değer okuma `appSettings` içinde *Web.config* uygulamanın içinde çalıştığı hangi ortamı belirlemek için dosya:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

Değer, "test ortamı ve"Üretim"üretim ortamında Test" olmalıdır.

Açık *Web.Production.config* ve ekleme bir `appSettings` öğesini hemen açılış etiketinde önce `location` daha önce eklediğiniz öğesi:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

`xdt:Transform` Öznitelik değeri "SetAttributes" amacı bu dönüşüm, var olan bir öğe öznitelik değerlerini değiştirmek için olduğunu gösterir *Web.config* dosya. `xdt:Locator` Öznitelik değeri "Match(key)" değiştirilecek öğenin bir olduğunu gösterir, `key` özniteliği eşleşme `key` burada belirtilen öznitelik. Yalnızca diğer öznitelik `add` öğesi `value`, ve nelerin değiştiğini, dağıtılan *Web.config* dosya. Bu kod neden `value` özniteliği `Environment` `appSettings` "Prod" için ayarlanması öğe *Web.config* üretim ortamına dağıtıldığı dosya.

Ardından, aynı değişikliği Uygula *Web.Test.config* kümesi dışında bir dosya `value` "Prod" yerine "test". İşiniz bittiğinde `appSettings` konusundaki *Web.Test.config* aşağıdaki örnekteki gibi görünecektir:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Hata ayıklama modunu devre dışı bırakma

Bir yayın yapısı için hata ayıklama etkin bağımsız olarak hangi ortamı dağıtım yaptığınız istemezsiniz. Varsayılan olarak *Web.Release.config* dönüşüm dosyasını kaldırır ve kod ile otomatik olarak oluşturulan `debug` özniteliğini `compilation` öğesi:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

`Transform` Özniteliği nedenleri `debug` özniteliği atlanacak dağıtılan gelen *Web.config* yayın derlemesi dağıttığınız her dosya.

Yayın dönüşüm dosyasını kopyalayarak oluşturduğundan bu aynı Test ve üretim dönüştürme dosyalarını dönüşümdür. Var. Yinelenen gerekmez, bu nedenle açık bu dosya, Kaldır **derleme** öğesi ve her dosyayı kaydedip kapatın.

## <a name="setting-connection-strings"></a>Bağlantı dizelerini ayarlama

Bağlantı dizeleri yayımlama profilinde belirtebildiğinizden çoğu durumda, bağlantı dizesi dönüştürmeleri ayarlamak gerekmez. Ancak bir SQL Server Compact veritabanı dağıtırken bir özel durum olduğundan ve hedef sunucuda veritabanını güncellemek için Entity Framework Code First Migrations'ı kullanıyorsanız. Bu senaryo için sunucuda veritabanı şemasını güncelleştirmek için kullanılacak bir ek bağlantı dizesi belirtmeniz gerekir. Bu dönüştürme için ekleyin bir **&lt;connectionStrings&gt;** öğesini hemen açılış **&lt;yapılandırma&gt;** hem de etiketi *Web.Test.config* ve *Web.Production.config* dönüştürme dosyaları:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

`Transform` Özniteliği, bu bağlantı dizesini eklenecek belirtir *connectionStrings* dağıtılan öğesinde *Web.config* dosya. (Yayımlama işlemi bu ek bağlantı dizesini otomatik olarak mevcut değilse sizin için oluşturur, ancak varsayılan olarak **providerName** özniteliği kümesine `System.Data.SqlClient`, hangi seçenekleri çalışmaz olmayan SQL Server Compact için. Bağlantı dizesi el ile ekleyerek, dağıtım işlemi yanlış sağlayıcı adı ile bir bağlantı dizesi öğesini oluşturmaktan tutun.)

Artık tüm belirttiğiniz *Web.config* üretim ve test etmek için Contoso University uygulama dağıtmak için gereken dönüşümler. Aşağıdaki öğreticide, proje özelliklerini ayarlama gerektiren dağıtım kurulum görevlerini ilgileniriz.

## <a name="more-information"></a>Daha fazla bilgi

Bu öğretici kapsamında konular hakkında daha fazla bilgi için bkz: Web.config dönüşümü senaryoda [ASP.NET dağıtım içerik haritası](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Önceki](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [İleri](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
