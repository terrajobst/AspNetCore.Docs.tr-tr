---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Visual Studio kullanarak ASP.NET Web Dağıtımı: Web.config dosyası dönüşümleri | Microsoft Docs'
author: tdykstra
description: Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya bir üçüncü taraf barındırma sağlayıcı tarafından usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 4d8a7d2a6faa0b03fff4416778101b47df2dd26e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371771"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Visual Studio kullanarak ASP.NET Web Dağıtımı: Web.config dosyası dönüşümleri
====================
tarafından [Tom Dykstra](https://github.com/tdykstra)

[Başlangıç projesini indirin](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Bu öğretici serisinin nasıl dağıtılacağı gösterilir (bir ASP.NET Yayımlama) web uygulamasını Azure App Service Web Apps veya üçüncü taraf bir barındırma sağlayıcısı, Visual Studio 2012 veya Visual Studio 2010 kullanarak. Seriyle ilgili daha fazla bilgi için bkz: [serideki ilk öğreticide](introduction.md).


## <a name="overview"></a>Genel Bakış

Bu öğreticide değiştirme işlemini otomatik hale getirmek gösterilir *Web.config* dosya farklı bir hedef ortamlara dağıttığınızda. Çoğu uygulama ayarlarına sahip *Web.config* uygulama dağıtıldığında, farklı bir dosya. Bu değişiklikler sürecini otomatik hale getirme dağıttığınız her zaman, hangi can sıkıcı olabilir ve hata yapmaya açık bunları el ile yapmak zorunda kalmasını sağlar.

Anımsatıcı: hata iletisi alıyorum veya Bu öğreticide ilerlerken bir sorun oluşması durumunda kontrol ettiğinizden emin olun [sorun giderme sayfası](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Web dağıtımı parametreleri karşı Web.config dönüşümleri

Değiştirme işlemini otomatik hale getirmek için iki yolu vardır *Web.config* dosyası ayarları: [Web.config dönüşümlerini](https://msdn.microsoft.com/library/dd465326.aspx) ve [Web dağıtımı parametreleri](https://msdn.microsoft.com/library/ff398068.aspx). A *Web.config* nasıl değiştirileceğini belirtir. XML işaretlemesini dönüşüm dosyası içeren *Web.config* dağıtıldığında dosya. Farklı değişiklikler belirli yapılandırmaları oluşturma ve yayımlama profilleri için özel belirtebilirsiniz. Hata ayıklama ve yayın varsayılan derleme yapılandırmaları olan ve özel yapı yapılandırması oluşturabilirsiniz. Bir yayımlama profili, bir hedef ortam için genellikle karşılık gelir. (Profillerinde yayımlama hakkında daha fazla bilgi edineceksiniz [bir Test ortamı olarak IIS'ye dağıtma](deploying-to-iis.md) öğretici.)

Web dağıtım parametrelerini, birçok farklı türde bulunan ayarlar dahil olmak üzere, dağıtım sırasında yapılandırılması gereken ayarları belirtmek için kullanılabilir *Web.config* dosyaları. Belirtmek için kullanıldığında *Web.config* dosya değişiklikleri, Web dağıtım parametrelerini ayarlamak için daha karmaşık, ancak dağıttığınız kadar ayarlanacak değer bilmediğinizde yararlı olur. Örneğin, bir kuruluş ortamında oluşturabilirsiniz bir *dağıtım paketi* üretim ortamında yüklemek için bir kişi BT departmanı içindeki vermek ve söz konusu kişinin bağlantı dizesi ya da olmayan parolaları girmeniz mümkün olması gerekir bildirin.

Bu öğretici serisinin kapsayan senaryo için önceden için yapılması gereken her şeyi biliyor *Web.config* dosya Web dağıtımı parametrelerini kullanmak gerek yoktur. Kullanılan derleme yapılandırmasına bağlı olarak farklı ve bazı kullanılan yayımlama profili bağlı olarak farklılık gösteren bazı dönüştürmeler yapılandıracaksınız.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Azure'da Web.config ayarlarını belirtme

Varsa *Web.config* değiştirmek istediğiniz dosya ayarları bulunduğunuz `<connectionStrings>` veya `<appSettings>` öğesi ve Azure App Service'te Web uygulamalarını dağıtıyorsanız, değişiklik sırasında otomatik hale getirmek için başka bir seçeneğiniz vardır Dağıtım. Azure'da etkili olmasını istediğiniz ayarları girebilirsiniz **yapılandırma** Yönetim Portalı sayfasında web uygulamanız için sekmesinde (doğru aşağı kaydırın **uygulama ayarları** ve **bağlantı dizeleri**  bölümler). Projeyi dağıttığınızda, Azure değişiklikleri otomatik olarak uygular. Daha fazla bilgi için [Windows Azure Web siteleri: nasıl uygulama dizeleri ve bağlantı dizeleri çalışma](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Varsayılan dönüştürme dosyaları

İçinde **Çözüm Gezgini**, genişletme *Web.config* görmek için *Web.Debug.config* ve *Web.Release.config* dönüştürme dosyaları iki varsayılan derleme yapılandırmaları için varsayılan olarak oluşturulur.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Özel derleme yapılandırmaları için dönüştürme dosyaları Web.config dosyasına sağ tıklayıp seçerek oluşturabilirsiniz **yapılandırma dönüşümleri Ekle** bağlam menüsünden. Bu öğretici için yapmanız gerekmez ve herhangi bir özel yapı yapılandırmalarını oluşturmadınız çünkü menü seçeneği devre dışıdır.

Daha sonra hazırlama üç daha fazla dönüştürme dosyaları, her biri için test oluşturursunuz ve üretim yayımlama profilleri. Hedef ortama bağlı olduğundan, bir yayımlama profili dönüştürme dosyasında işlemek bir ayar tipik bir örneği, üretim ve test için farklı bir WCF uç noktadır. İle gitmeleri yayımlama profilleri oluşturduktan sonra yayımlama profili dönüştürme dosyaları sonraki öğreticilerde oluşturacaksınız.

## <a name="disable-debug-mode"></a>Hata ayıklama modunu devre dışı bırak

Hedef ortamı yerine derleme yapılandırmasına bağlıdır bir ayar örneği `debug` özniteliği. Bir yayın yapısı için genellikle hata ayıklaması devre dışı bağımsız olarak hangi ortamı dağıtım yaptığınız istersiniz. Bu nedenle, varsayılan olarak Visual Studio Proje şablonları oluşturma *Web.Release.config* dönüştürme kaldıran bir kod dosyalarıyla `debug` özniteliğini `compilation` öğesi. Burada, varsayılan *Web.Release.config*: isteğe bağlı olarak devre dışı bırakılmışsa bazı örnek dönüştürme kodu yanı sıra kodu içerir `compilation` kaldırır öğesi `debug` özniteliği:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

`xdt:Transform="RemoveAttributes(debug)"` Özniteliği belirtir, istediğiniz `debug` kaldırılmasını özniteliği `system.web/compilation` dağıtılan öğesinde *Web.config* dosya. Yayın derlemesi dağıttığınız her zaman bu gerçekleştirilir.

## <a name="limit-error-log-access-to-administrators"></a>Yöneticiler hata günlüğü erişimi sınırlayın

Uygulama çalışırken bir hata olduğundan, bir genel hata sayfası sistem tarafından oluşturulan hata sayfası yerine uygulama görüntüler ve kullandığı [Elmah NuGet paketini](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) günlüğe kaydetme ve Raporlama hata. `customErrors` Uygulama öğesinde *Web.config* dosyası hata sayfası belirtir:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Hata sayfası görmek için geçici olarak değiştirme `mode` özniteliği `customErrors` "Açık" "RemoteOnly" ve uygulamayı Visual Studio'dan çalışma öğesi. Geçersiz bir URL gibi talep ederek hataya neden *Studentsxxx.aspx*. Bir IIS tarafından oluşturulan "kaynağı bulunamadı" hatası yerine sayfasında, gördüğünüz *GenericErrorPage.aspx* sayfası.

![Hata sayfası](web-config-transformations/_static/image2.png)

Hata günlüğünü görmek için URL'de bulunan her şeyi sonra bağlantı noktası numarasıyla değiştirin *elmah.axd* (örneğin, `http://localhost:51130/elmah.axd`) ve Enter tuşuna basın:

![ELMAH sayfası](web-config-transformations/_static/image3.png)

Ayarlamayı unutmayın `customErrors` işiniz bittiğinde "RemoteOnly" moduna geri öğesi.

Geliştirme bilgisayarınızda hata günlüğü sayfasında, ancak bir güvenlik riski doğurur üretimde ücretsiz erişim izni vermek uygundur. Yöneticiler için hata günlüğüne erişimi kısıtlayan bir yetkilendirme kuralı ekleyin ve kısıtlama, çalıştığından emin olmak için üretim sitesi için istediğiniz test ve hazırlık ayrıca istiyor. Bu nedenle bu her zaman bir yayın derlemesi dağıtmak ve ait olduğunu için uygulamak istediğiniz başka bir değişikliktir *Web.Release.config* dosya.

Açık *Web.Release.config* ve yeni bir `location` kapatmadan hemen önce öğesi `configuration` burada gösterildiği gibi etiketleyin.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

`Transform` "Ekle" özniteliğinin değerini neden bu `location` varolan herhangi bir eşdüzeyi eklenecek öğe `location` öğelerinde *Web.config* dosya. (Zaten var. bir `location` yetkilendirme belirten öğesi kuralları için **güncelleştirme KREDİLERİ** sayfası.)

Şimdi, düzgün şekilde kodlanmış emin olmak için dönüştürme önizleyebilirsiniz.

İçinde **Çözüm Gezgini**, sağ *Web.Release.config* tıklatıp **Önizleme dönüştürme**.

![Önizleme dönüştürme menüsü](web-config-transformations/_static/image4.png)

Geliştirme gösteren bir sayfa açılır *Web.config* solda ve hangi dosya dağıtılan *Web.config* dosya görünür gibi sağ tarafta vurgulanan değişikliklerle.

![Hata ayıklama dönüştürme önizlemesi](web-config-transformations/_static/image5.png)

![Konum dönüştürme önizlemesi](web-config-transformations/_static/image6.png)

(Önizleme sürümünde yazmadığınız bazı ek değişiklikler dönüştüren için fark edebilirsiniz: Bunlar genellikle işlevselliğini etkilemez boşluk kaldırılmasını içerir.)

Dağıtımdan sonra site test ettiğinizde, Yetkilendirme kuralının etkin olduğunu doğrulamak için sınayacaksınız.

> [!NOTE] 
> 
> **Güvenlik Notu** hiçbir zaman bir üretim uygulamasında genel hata ayrıntılarını görüntülemek veya bu bilgileri ortak bir yerde saklayın. Saldırganlar, güvenlik açıklarını bir site bulmak için hata bilgilerini kullanabilirsiniz. ELMAH kendi uygulamanıza kullanırsanız, güvenlik riskleri en aza indirmek için ELMAH yapılandırın. Bu öğreticide ELMAH örnek önerilen bir yapılandırma kabul edilmemelidir. Uygulama dosyaları oluşturmak için bir klasör ne yapılacağını göstermek için seçilmiştir bir örnektir. Daha fazla bilgi için [ELMAH uç nokta güvenliği](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Yayımlama profili dönüştürme dosyaları ele alacağız bir ayarı

Sık karşılaşılan bir senaryodur sağlamaktır *Web.config* dosya dağıttığınız her ortamda farklı ayarlar. Örneğin, bir WCF Hizmeti çağıran bir uygulama farklı bir uç nokta test ve üretim ortamlarında gerekebilir. Contoso University uygulama bu tür bir ayarı da içerir. Bu ayar, geliştirme, test veya üretim gibi bulunduğunuz hangi ortamı söyleyen görünür bir gösterge bir sitenin sayfalarında denetler. Uygulama "(Geliştirme)" ekleyeceği ayar değeri belirler veya "(Test)" ana başlıkta *Site.Master* ana sayfa:

![Ortam göstergesi](web-config-transformations/_static/image7.png)

Ortam göstergesi, hazırlama veya üretim uygulama çalışırken atlanır.

Contoso University web sayfaları kümesinde bir değer okuma `appSettings` içinde *Web.config* uygulamanın içinde çalıştığı hangi ortamı belirlemek için dosya:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Değeri test ortamında olması "Test" ve "hazırlama ve üretim için Prod" gerekir.

Dönüşüm dosyası aşağıdaki kodda, bu dönüşüm uygular:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

`xdt:Transform` Öznitelik değeri "SetAttributes" amacı bu dönüşüm, var olan bir öğe öznitelik değerlerini değiştirmek için olduğunu gösterir *Web.config* dosya. `xdt:Locator` Öznitelik değeri "Match(key)" değiştirilecek öğenin bir olduğunu gösterir, `key` özniteliği eşleşme `key` burada belirtilen öznitelik. Yalnızca diğer öznitelik `add` öğesi `value`, ve nelerin değiştiğini, dağıtılan *Web.config* dosya. Neden burada gösterilen kod `value` özniteliği `Environment` `appSettings` "Test" için ayarlanması öğe *Web.config* dağıtıldığı dosya.

Bu dönüşüm, henüz oluşturmadıysanız yayımlama profili dönüştürme dosyalarında, aittir. Oluşturacak ve bu değişiklik, test, hazırlık ve üretim ortamları için yayımlama profilleri oluştururken uygulayan dönüşüm dosyasını güncelleştirme. Bu gerçekleştirirsiniz [IIS'ye dağıtma](deploying-to-iis.md) ve [üretime dağıtma](deploying-to-production.md) öğreticiler.

> [!NOTE]
> Bu ayar olduğundan `<appSettings>` öğesi, sahip olduğunuz Azure App Service bakın. Web Apps'e dağıtım yapılırken dönüşümü belirtmek için başka bir alternatif [belirten Web.config ayarları azure'da](#watransforms) önceki Bu konuda.


## <a name="setting-connection-strings"></a>Bağlantı dizelerini ayarlama

Varsayılan dönüştürme dosyasını bir bağlantı dizesi güncelleştirme gösteren bir örnek içerse de, bağlantı dizeleri yayımlama profilinde belirtebildiğinizden çoğu durumda, bağlantı dizesi dönüştürmeleri ayarlamak gerekmez. Bu gerçekleştirirsiniz [IIS'ye dağıtma](deploying-to-iis.md) ve [üretime dağıtma](deploying-to-production.md) öğreticiler.

## <a name="summary"></a>Özet

İle mümkün olduğunca artık yaptığınız *Web.config* yayımlama profilleri oluşturabilir ve dağıtılmış Web.config dosyasında ne olacağını önizlemesini gördüğünüz önce dönüşümler.

![Konum dönüştürme önizlemesi](web-config-transformations/_static/image8.png)

Aşağıdaki öğreticide, proje özelliklerini ayarlama gerektiren dağıtım kurulum görevlerini ilgileniriz.

## <a name="more-information"></a>Daha fazla bilgi

Bu öğretici kapsamında konular hakkında daha fazla bilgi için bkz. [dağıtım sırasında hedef Web.config veya app.config dosyası ayarlarını değiştirmek için kullanarak Web.config dönüşümlerini](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) için Web dağıtımı içerik haritası'nda Visual Studio ve ASP.NET.

> [!div class="step-by-step"]
> [Önceki](preparing-databases.md)
> [İleri](project-properties.md)
