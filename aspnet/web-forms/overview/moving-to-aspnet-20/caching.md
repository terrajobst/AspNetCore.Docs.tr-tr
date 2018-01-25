---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: "Önbelleğe alma | Microsoft Docs"
author: microsoft
description: "İyi gerçekleştiren bir ASP.NET uygulaması için önbelleğe alma anlaşılması önemlidir. ASP.NET 1.x sunulan önbelleğe alma için; üç farklı seçenekler önbelleğe alma çıkışını..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 9b229de60e09b94189f62a6bb6fa61a9973d637b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="caching"></a>Önbelleğe alma
====================
tarafından [Microsoft](https://github.com/microsoft)

> İyi gerçekleştiren bir ASP.NET uygulaması için önbelleğe alma anlaşılması önemlidir. ASP.NET 1.x sunulan önbelleğe alma için; üç farklı seçenekler çıktı önbelleği, parça ve önbellek API.


İyi gerçekleştiren bir ASP.NET uygulaması için önbelleğe alma anlaşılması önemlidir. ASP.NET 1.x sunulan önbelleğe alma için; üç farklı seçenekler çıktı önbelleği, parça ve önbellek API. ASP.NET 2.0 üçünü de bu yöntemleri sunar, ancak bazı önemli ek özellikler ekler. Geliştiriciler artık özel önbellek bağımlılıkları da oluşturma seçeneğiniz vardır ve birkaç yeni önbellek bağımlılıkları vardır. Önbelleğe alma yapılandırması, ASP.NET 2. 0'da bir önemli ölçüde iyileştirilmiştir.

## <a name="new-features"></a>Yeni Özellikler

## <a name="cache-profiles"></a>Önbellek profilleri

Önbellek profilleri, geliştiricilerin belirli sayfalara uygulanabilir belirli önbellek ayarlarını tanımlamak olanak sağlar. Örneğin, önbellekten 12 saat sonra süresi bazı sayfaları varsa, bu sayfalara uygulanabilir bir önbellek profili kolayca oluşturabilirsiniz. Yeni bir önbellek profili eklemek için kullanın &lt;outputCacheSettings&gt; yapılandırma dosyasındaki bölüm. Örneğin, adında bir önbellek profili yapılandırma aşağıdadır *twoday* 12 saatlik bir önbellek süresi yapılandırır.

[!code-xml[Main](caching/samples/sample1.xml)]

Bu önbellek profili için belirli bir sayfa uygulamak için aşağıda gösterildiği gibi @ OutputCache yönergesinin CacheProfile özniteliğini kullanın:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Özel önbellek bağımlılıkları

ASP.NET 1.x geliştiriciler için özel önbellek bağımlılıkları cried. ASP.NET 1.x, CacheDependency sınıfı ondan kendi sınıflardan türetme gelen önlenmiş geliştiricilerin korumalı. ASP.NET 2. 0'da, bu sınırlama kaldırılır ve geliştiricilerin kendi özel önbellek bağımlılıkları geliştirmek boş. CacheDependency sınıfı dosyaları, dizinleri veya önbellek anahtarlarını göre özel önbellek bağımlılığı oluşturulmasını sağlar.

Örneğin, aşağıdaki kod, Web uygulaması kök dizininde bulunan stuff.xml adlı bir dosya dayalı yeni bir özel önbellek bağımlılığı oluşturur:

[!code-csharp[Main](caching/samples/sample3.cs)]

Bu senaryoda, stuff.xml dosyası değiştiğinde, önbelleğe alınan öğe geçersiz kılınır.

Önbellek anahtarlarını kullanan bir özel önbellek bağımlılığı oluşturmak mümkündür. Bu yöntemi kullanarak, önbellek anahtarını kaldırılmasını önbelleğe alınan veri geçersiz kılar. Aşağıdaki örnekte bu gösterilmektedir:

[!code-csharp[Main](caching/samples/sample4.cs)]

Yukarıdaki eklenmiş öğesi geçersiz kılmak için basitçe önbellek anahtarı olarak davranacak şekilde önbelleğine eklenmiş öğeyi kaldırın.

[!code-csharp[Main](caching/samples/sample5.cs)]

Önbellek anahtarı olarak davranan öğenin anahtarı önbelleği anahtarları diziye eklenen değer ile aynı olması gerektiğini unutmayın.

## <a name="polling-based-sql-cache-dependenciesalso-called-table-based-dependencies"></a>Yoklama tabanlı SQL önbellek bağımlılıkları*(tablo tabanlı bağımlılıklar olarak da bilinir)*

SQL Server 7 ve 2000 yoklama tabanlı modelini SQL önbellek bağımlılıkları için kullanın. Yoklama tabanlı modeli tetikleyici tablodaki verileri değiştirdiğinizde tetikleyen bir veritabanı tablosu kullanır. Güncelleştirmeleri tetikleyen bir **changeId** bildirim tablosundaki ASP.NET düzenli olarak denetler. Varsa **changeId** alan güncelleştirildi, ASP.NET bilir veri değişmiş ve önbelleğe alınan veri geçersiz kılar.

> [!NOTE]
> SQL Server 2005 yoklama tabanlı modeli de kullanabilirsiniz, ancak yoklama tabanlı modeli en verimli modeli olmadığından, SQL Server 2005'te (daha sonra açıklanan) bir sorgu tabanlı modelini kullanmak için önerilir.


Doğru çalışması için yoklama tabanlı modeli kullanarak bir SQL önbellek bağımlılığı tabloları bildirimleri etkinleştirilmiş olmalıdır. Bu program aracılığıyla SqlCacheDependencyAdmin sınıfı kullanılarak gerçekleştirilebilir veya aspnet kullanarak\_regsql.exe yardımcı programı.

Aşağıdaki komut satırını Ürünler tablosuna adlı bir SQL Server örneği üzerinde bulunan Northwind veritabanı kaydeder *dbase* SQL için önbellek bağımlılığı.

[!code-console[Main](caching/samples/sample6.cmd)]

Yukarıdaki komutta kullanılan komut satırı anahtarları bir açıklaması verilmiştir:

| **Komut satırı anahtarı** | **Amaç** |
| --- | --- |
| -S *sunucu* | Sunucu adını belirtir. |
| -ed | Veritabanı için SQL önbellek bağımlılığı etkin olması gerektiğini belirtir. |
| -d *veritabanı\_adı* | SQL önbellek bağımlılığı için etkinleştirilmelidir veritabanı adını belirtir. |
| -E | Bu aspnet belirtir\_regsql veritabanına bağlanırken Windows kimlik doğrulaması kullanmanız. |
| -et | Bir veritabanı tablosu SQL önbellek bağımlılığı etkinleştiriyorsanız belirtir. |
| -t *tablo\_adı* | SQL önbellek bağımlılığı için etkinleştirmek için veritabanı tablosunun adını belirtir. |

> [!NOTE]
> Diğer anahtarlar için aspnet kullanılabilir\_regsql.exe. Tam bir listesi için ASP.NET çalıştıran\_regsql.exe-? bir komut satırından.


Bu komut çalıştırıldığında aşağıdaki değişiklikler SQL Server veritabanına yapılır:

- An **AspNet\_SqlCacheTablesForChangeNotification** table is added. Bu tablo bir SQL önbellek bağımlılığı etkin veritabanındaki her tablo için bir satır içerir.
- Aşağıdaki saklı yordamlar veritabanı içinde oluşturulur:


| AspNet\_SqlCachePollingStoredProcedure | AspNet sorgular\_SqlCacheTablesForChangeNotification tablo ve SQL önbellek bağımlılığı ve her tablo için changeId değeri için etkinleştirilen tüm tabloları döndürür. Bu saklı yordam, veri değişip değişmediğini için yoklama için kullanılır. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Tüm AspNet sorgulayarak SQL önbellek bağımlılığı için etkinleştirilmiş tablolar döndürür\_SqlCacheTablesForChangeNotification tablo ve tüm tablolar için SQL etkin döndürür Önbelleğe bağımlılığı. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Bildirim tablosunda gerekli girişini ekleyerek SQL önbellek bağımlılığı tablosunu kaydeder ve tetikleyici ekler. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Bildirim tablosunda giriş kaldırarak SQL önbellek bağımlılığı tablosunu kaydını siler ve tetikleyici kaldırır. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Bildirim tablosu değişen tablo changeId artırılarak güncelleştirir. ASP.NET veri değişip değişmediğini için bu değeri kullanır. Aşağıda gösterildiği gibi bu saklı yordam tablo etkinleştirildiğinde oluşturulan tetik tarafından yürütülür. |


- SQL Server tetikleyici adlı ***tablo\_adı *\_AspNet\_SqlCacheNotification\_tetikleyici** tablo için oluşturulur. Bu tetikleyici AspNet yürütür\_tablosunda bir INSERT, UPDATE veya DELETE gerçekleştirildiğinde SqlCacheUpdateChangeIdStoredProcedure.
- SQL Server rolü adlı **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** veritabanına eklenir.

**Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** SQL Server rolü AspNet yürütme izinlerine sahip\_SqlCachePollingStoredProcedure. Doğru çalışması yoklama modeli için sırayla aspnet işlem hesabınızı eklemelisiniz\_ChangeNotification\_ReceiveNotificationsOnlyAccess rol. Aspnet\_regsql.exe aracı değil bunu sizin için.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Yoklama tabanlı SQL önbellek bağımlılıkları yapılandırma

Yoklama tabanlı SQL önbellek bağımlılıkları yapılandırmak için gereken birkaç adım vardır. İlk adım, veritabanı ve tablo yukarıda açıklandığı gibi sağlamaktır. Bu adım tamamlandıktan sonra yapılandırmasını geri kalanı aşağıdaki gibidir:

- ASP.NET yapılandırma dosyası yapılandırma.
- SqlCacheDependency yapılandırma

### <a name="configuring-the-aspnet-configuration-file"></a>ASP.NET yapılandırma dosyasını yapılandırma

Bir önceki modüldeki anlatıldığı gibi bir bağlantı dizesi ekleme yanı sıra da yapılandırmalısınız bir &lt;önbellek&gt; öğesi ile bir &lt;sqlCacheDependency&gt; aşağıda gösterildiği gibi öğe:

[!code-xml[Main](caching/samples/sample7.xml)]

Bu yapılandırma üzerinde bir SQL önbellek bağımlılığı etkinleştirir *pubs* veritabanı. PollTime özniteliği Not &lt;sqlCacheDependency&gt; öğesi varsayılan olarak 60000 milisaniye veya 1 dakika. (Bu değer 500'den az milisaniye olamaz.) Bu örnekte, &lt;ekleme&gt; öğeyi yeni bir veritabanı ekler ve 9000000 milisaniye ayarı pollTime geçersiz kılar.

#### <a name="configuring-the-sqlcachedependency"></a>SqlCacheDependency yapılandırma

Sonraki adım SqlCacheDependency yapılandırmaktır. SqlDependency özniteliğinin değeri @ Outcache yönergesinde gibi belirtmek için bunu yapmaya yönelik en kolay yolu şöyledir:

[!code-aspx[Main](caching/samples/sample8.aspx)]

Yukarıdaki @ OutputCache yönergesinde SQL önbellek bağımlılığı için yapılandırılmış *yazarlar* tablosundaki *pubs* veritabanı. Noktalı virgülle ayrılarak birden çok bağımlılıkları yapılandırılabilir sözlüğüdür:

[!code-aspx[Main](caching/samples/sample9.aspx)]

SqlCacheDependency yapılandırma başka bir program aracılığıyla bunu yöntemdir. Aşağıdaki kod yeni bir SQL önbellek bağımlılık oluşturur *yazarlar* tablosundaki *pubs* veritabanı.

[!code-csharp[Main](caching/samples/sample10.cs)]

Program aracılığıyla SQL önbellek bağımlılığı tanımlama avantajlarını oluşabilecek özel durumlar işleyebilir biridir. Bildirim için etkinleştirilmemiş bir veritabanı için SQL önbellek bağımlılık tanımlamak çalışırsanız, örneğin, bir **DatabaseNotEnabledForNotificationException** özel durum. Bu durumda, çağırarak veritabanı bildirimleri için etkinleştirmeyi deneyebilirsiniz **SqlCacheDependencyAdmin.EnableNotifications** yöntemi ve veritabanı adını geçirme.

Benzer şekilde, bildirim için etkinleştirilmemiş bir tablo için bir SQL önbellek bağımlılığı tanımlamak çalışırsanız bir **TableNotEnabledForNotificationException** oluşturulur. Ardından çağırabilirsiniz **SqlCacheDependencyAdmin.EnableTableForNotifications** tablo adını ve veritabanı adını geçirme yöntemi.

Aşağıdaki kod örneği düzgün özel durum işleme SQL önbellek bağımlılığı yapılandırırken nasıl yapılandırılacağı gösterilmektedir.

[!code-csharp[Main](caching/samples/sample11.cs)]

Daha fazla bilgi: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Sorgu tabanlı SQL önbellek bağımlılıkları (yalnızca SQL Server 2005)

SQL Server 2005 SQL önbellek bağımlılığı için kullanılırken, yoklama tabanlı modeli gerekli değildir. SQL Server 2005 ile kullanıldığında, SQL önbellek bağımlılıkları (başka bir yapılandırma gereklidir) SQL Server örneğine SQL bağlantıları üzerinden doğrudan iletişim kurmasına SQL Server 2005 sorgu bildirimleri kullanarak.

Bu nedenle bildirimli olarak ayarlayarak yapmak için sorgu tabanlı bildirim etkinleştirmek için en basit yolu olan **SqlCacheDependency** özniteliği için veri kaynağı nesnesinin **CommandNotification** ve ayarını**EnableCaching** özniteliğini **doğru**. Bu yöntemi kullanarak, kod gereklidir. Bir komut sonucunu karşı veri kaynağı değişiklikleri yürütülen önbellek verilerini geçersiz kılacak.

Aşağıdaki örnek, bir veri kaynağı denetimi SQL önbellek bağımlılığı yapılandırır:

[!code-aspx[Main](caching/samples/sample12.aspx)]

Sorgu içinde belirtilmişse bu durumda, **SelectCommand** daha farklı bir sonuç vermedi başlangıçta döndürür, önbelleğe alınan sonuçları geçersiz.

Tüm veri kaynaklarınız için SQL önbellek bağımlılıkları ayarlayarak etkinleştirilmiş olması da belirtebilirsiniz **SqlDependency** özniteliği **@ OutputCache** için yönerge **CommandNotification** . Aşağıdaki örnekte bu gösterilmektedir.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> SQL Server 2005'te sorgu bildirimleri hakkında daha fazla bilgi için SQL Server Books Online'a bakın.


Sorgu tabanlı SQL önbellek bağımlılığı yapılandırma başka bir yöntem Bunu yapmak için program aracılığıyla SqlCacheDependency sınıfı kullanmaktır. Aşağıdaki kod örneği, nasıl yapıldığını gösterir.

[!code-csharp[Main](caching/samples/sample14.cs)]

Daha fazla bilgi: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Sonrası önbellek değiştirme

Bir sayfanın önbelleğe alma, bir Web uygulaması performansını önemli ölçüde artırabilir. Ancak, bazı durumlarda önbelleğe alınacak sayfa çoğu ve bazı parçaları dinamik olarak sayfada gerekir. Örneğin, ayarlanmış süreyle tamamen statik haberleri sayfasında oluşturursanız, önbelleğe alınacak sayfanın tamamını ayarlayabilirsiniz. Her sayfa isteğinde değiştirilen dönen bir ad başlık eklemek istiyorsanız, tanıtım içeren sayfanın parçası dinamik olması gerekir. Bir sayfayı önbelleğe ancak bazı içerikler dinamik yedek olanak tanımak için ASP.NET sonrası önbellek değişim kullanabilirsiniz. Sonrası önbellek değiştirme ile tüm çıktı önbelleği muaf olarak işaretlenmiş belirli bölümlerle önbelleği sayfasıdır. Ad başlıkları örnekte AdRotator denetimi, böylece ads dinamik olarak oluşturulan her kullanıcı için ve her sayfa yenileme sonrası önbellek değiştirme yararlanmak sağlar.

Sonrası önbellek değiştirme uygulamak için üç yolu vardır:

- Bildirimli olarak, değişim denetimi kullanma.
- Program aracılığıyla, değişim denetimi API kullanma.
- Örtük olarak, AdRotator denetim kullanma.

### <a name="substitution-control"></a>Değişim Denetimi

ASP.NET değişim denetimi, dinamik olarak oluşturulan yerine önbelleğe alınmış önbelleğe alınan bir sayfanın bölümünü belirtir. Değişim Denetimi sayfasında görünmesi dinamik içerik istediğiniz konuma yerleştirin. Çalışma zamanında, değişim denetimi MethodName özelliğiyle belirttiğiniz bir yöntemi çağırır. Yöntemi, değişim denetimi içeriğini değiştiren bir dize döndürmesi gerekir. Yöntem statik yöntem içeren sayfa veya UserControl denetiminin üzerinde olmalıdır. Böylece sayfa istemcide önbelleğe alınmamış değişim denetimi kullanarak sunucu önbelleğe alınabilirliğini için değiştirilecek istemci tarafı önbelleğe alınabilirliğini neden olur. Bu sayfaya gelecekteki isteklerin dinamik içeriği yeniden oluşturmak için yöntemi çağırın sağlar.

### <a name="substitution-api"></a>Değiştirme API

Dinamik içerik önbelleğe alınmış bir sayfa için programlı olarak oluşturmak için çağırabilirsiniz [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) bir yöntemin adını parametre olarak geçirme sayfa kodunuzdaki yöntemi. Tek bir dinamik içerik oluşturulmasını işleyen yöntem alır [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) parametre ve bir dize döndürür. Dönüş dizesi, belirli bir konumda değiştirilecektir içeriktir. Değişim Denetimi bildirimli olarak kullanmak yerine WriteSubstitution yöntemini çağıran bir avantajı, sayfa veya UserControl nesnesinin statik bir yöntemi çağırmak yerine herhangi bir rastgele nesne yöntemi çağırabilirsiniz olmasıdır.

Böylece sayfa istemcide önbelleğe alınmamış WriteSubstitution yöntemini çağıran sunucu önbelleğe alınabilirliğini için değiştirilecek istemci tarafı önbelleğe alınabilirliğini neden olur. Bu sayfaya gelecekteki isteklerin dinamik içeriği yeniden oluşturmak için yöntemi çağırın sağlar.

### <a name="adrotator-control"></a>AdRotator denetimi

Sunucu denetimi uygulayan AdRotator sonrası önbellek değiştirme için dahili olarak destekler. Sayfanızda bir AdRotator kontrolü yerleştirirseniz, ana sayfa olup önbelleğe bağımsız olarak her isteği benzersiz reklamları işlemez. Sonuç olarak, bir AdRotator denetimini içeren bir yalnızca önbelleğe alınmış sunucu tarafı sayfasıdır.

## <a name="controlcachepolicy-class"></a>ControlCachePolicy sınıfı

Kullanıcı denetimleri kullanarak önbelleğe alma parça programsal denetim için ControlCachePolicy sınıfı sağlar. ASP.NET katıştırır kullanıcı denetimleri içinde bir [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) örneği. BasePartialCachingControl sınıfı önbelleğe alma etkin çıktısı bir kullanıcı denetimi temsil eder.

Eriştiğinizde [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) özelliği bir [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) denetim, geçerli bir ControlCachePolicy nesnesi her zaman alacak. Ancak, erişim [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) özelliği bir [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) denetimi, alırsanız geçerli bir ControlCachePolicy nesnesi yalnızca kullanıcı denetimi zaten tarafından kaydırılan bir BasePartialCachingControl denetim. Bunu sarmalanmamış ilişkili BasePartialCachingControl olmadığından işlemek çalıştığınızda özellik tarafından döndürülen ControlCachePolicy nesne bu özel durumlar atar. UserControl örneği özel durumlar oluşturmadan önbelleğe alma destekleyip desteklemediğini belirlemek için incelemek [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) özelliği.

ControlCachePolicy sınıfını kullanarak çıktı önbelleğe almayı etkinleştirmek çeşitli yollardan biri. Aşağıdaki liste, çıktı önbelleğe almayı etkinleştirmek için kullanabileceğiniz yöntemleri açıklar:

- Kullanım [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) etkinleştirmek için yönergesi çıktı bildirim temelli senaryolarda önbelleğe alma.
- Kullanım [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) bir arka plan kod dosyasına bir kullanıcı denetimi için önbelleğe almayı etkinleştirmek için öznitelik.
- Önceki yöntemlerden birini kullanarak önbelleği etkin ve dinamik olarak kullanılarakyüklenenBasePartialCachingControlörnekleriyleiçindeçalıştığınızıprogramlısenaryolardaönbellekayarlarınıbelirtmekiçinControlCachePolicysınıfınıkullanmak[System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) yöntemi.

Bir ControlCachePolicy örneği başarıyla yalnızca Init ve PreRender aşamalarını denetim yaşam döngüsü arasında değişebilir. Sonra PreRender aşama ControlCachePolicy nesne değiştirirseniz, Denetim oluşturulduğunda yapılan değişiklikler gerçekte (Denetim işleme aşamasında önbelleğe alınır) önbellek ayarları olamaz etkilediğinden ASP.NET bir özel durum oluşturur. Gerçekte işlendiğinde son olarak, bir kullanıcı denetimi örneği (ve bu nedenle, ControlCachePolicy nesnesi) yalnızca programlı değişikliği yapmak için kullanılabilir.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Değişiklikleri önbelleğe alma yapılandırması için - &lt;önbelleğe alma&gt; öğesi

ASP.NET 2.0 önbelleğe alma yapılandırması için bazı değişiklikler vardır. &lt;Önbelleğe alma&gt; öğesi ASP.NET 2. 0 ' yenidir ve yapılandırma dosyasında önbelleğe alma yapılandırma değişiklikleri yapmasını sağlar. Aşağıdaki öznitelikler kullanılabilir.

| **Öğesi** | **Açıklama** |
| --- | --- |
| **Önbellek** | İsteğe bağlı öğe. Genel Uygulama önbellek ayarlarını tanımlar. |
| **outputCache** | İsteğe bağlı öğe. Uygulama çapında çıkış önbelleği ayarlarını belirtir. |
| **outputCacheSettings** | İsteğe bağlı öğe. Uygulama sayfalarına uygulanan çıkış önbelleği ayarlarını belirtir. |
| **sqlCacheDependency** | İsteğe bağlı öğe. Bir ASP.NET uygulaması için SQL önbellek bağımlılıkları yapılandırır. |

### <a name="the-ltcachegt-element"></a>&lt;Önbellek&gt; öğesi

Aşağıdaki öznitelikler kullanılabilir &lt;önbellek&gt; öğe:

| **Özniteliği** | **Açıklama** |
| --- | --- |
| **disableMemoryCollection** | İsteğe bağlı **Boolean** özniteliği. Alır veya makine bellek baskısı altında olduğunda oluşan önbellek koleksiyonu devre dışı olup olmadığını belirten bir değer ayarlar. |
| **disableExpiration** | İsteğe bağlı **Boolean** özniteliği. Alır veya önbellek süre sonu devre dışı olup olmadığını belirten bir değer ayarlar. Devre dışı bırakıldığında, önbelleğe alınmış öğeleri son kullanma tarihi ve önbellek süresi dolan öğelerinin arka plan atma meydana gelmez. |
| **privateBytesLimit** | İsteğe bağlı **Int64** özniteliği. Alır veya öğeleri temizlemeye önbellek başlamadan önce bir uygulamanın özel bayt en büyük boyutunu süresi belirten ve bellek boşaltma denemesinde bir değer ayarlar. Bu sınır, hem önbelleği tarafından kullanılan bellek, hem de çalışan uygulama normal bellek ek yükünü içerir. Sıfır ayarı ASP.NET bellek geri kazanma başlatmak ne zaman belirlemek için kendi buluşsal yöntemler kullanacağını gösterir. |
| **percentagePhysicalMemoryUsedLimit** | İsteğe bağlı **Int32** özniteliği. Maksimum önbellek temizlemeye başlamadan önce bir uygulama tarafından kullanılabilecek bir makinenin fiziksel bellek yüzdesi öğeleri süresi belirten ve önbellek tarafından kullanılan her iki bellek bu bellek kullanımını içeren belleği geri çalışılırken bir değeri alır veya ayarlar çalışan uygulama normal bellek kullanımı. Sıfır ayarı ASP.NET bellek geri kazanma başlatmak ne zaman belirlemek için kendi buluşsal yöntemler kullanacağını gösterir. |
| **privateBytesPollTime** | İsteğe bağlı **TimeSpan** özniteliği. Alır veya uygulamanın özel bayt bellek kullanımı için yoklama zaman aralığı belirten bir değer ayarlar. |

### <a name="the-ltoutputcachegt-element"></a>&lt;OutputCache&gt; öğesi

Aşağıdaki öznitelikler kullanılabilir &lt;outputCache&gt; öğesi.

| **Özniteliği** | **Açıklama** |
| --- | --- |
| **enableOutputCache** | İsteğe bağlı **Boolean** özniteliği. Sayfa çıktısı önbelleğini etkinleştirir/devre dışı bırakır. Devre dışı bırakılırsa, hiç sayfa Program erişimini ya da bildirim temelli ayarlarından bağımsız olarak önbelleğe alınır. Varsayılan değer **doğru**. |
| **enableFragmentCache** | İsteğe bağlı **Boolean** özniteliği. Uygulama parçası önbelleğini etkinleştirir/devre dışı bırakır. Devre dışı bırakılırsa, hiç sayfa bağımsız olarak, önbelleğe alınan [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) yönergesi veya kullanılan profili önbelleğe alma. Tarayıcı istemcilerinin yanı sıra Yukarı Akış proxy sunucuları sayfa çıktısını önbelleğe almaya çalışmamalısınız belirten bir önbellek-control üstbilgisi içerir. Varsayılan değer **false**. |
| **sendCacheControlHeader** | İsteğe bağlı **Boolean** özniteliği. Belirten değeri alır veya ayarlar olup olmadığını **önbellek-denetimi: özel** üstbilgisi varsayılan olarak çıktı önbelleği modülü tarafından gönderilir. Varsayılan değer **false**. |
| **omitVaryStar** | İsteğe bağlı **Boolean** özniteliği. Bir Http gönderme etkinleştirir/devre dışı bırakır "**ayırmayı: \*** " yanıt üstbilgisi. False, varsayılan ayarı olan bir "**ayırmayı: \*** " üstbilgisi çıktı önbelleği sayfaları için gönderilir. Ayırmayı üstbilgi gönderildiğinde için farklı verir önbelleğe alınacak sürümleri temel ayırmayı üstbilgisinde belirtilen bağlıdır. Örneğin, *ayırmayı: kullanıcı-aracıları* isteği vermeden kullanıcı aracısı temel bir sayfanın farklı sürümlerini depolar. Varsayılan değer **false**. |

### <a name="the-ltoutputcachesettingsgt-element"></a>&lt;OutputCacheSettings&gt; öğesi

&lt;OutputCacheSettings&gt; öğesi için daha önce açıklandığı gibi önbellek profilleri oluşturulmasını sağlar. Tek alt öğesi için &lt;outputCacheSettings&gt; öğesi &lt;outputCacheProfiles&gt; önbellek profilleri yapılandırma öğesi.

### <a name="the-ltsqlcachedependencygt-element"></a>&lt;SqlCacheDependency&gt; öğesi

Aşağıdaki öznitelikler kullanılabilir &lt;sqlCacheDependency&gt; öğesi.

| **Özniteliği** | **Açıklama** |
| --- | --- |
| **Etkin** | Gerekli **Boolean** özniteliği. Değişiklikler için yoklanıyor olup olmadığını gösterir. |
| **pollTime** | İsteğe bağlı **Int32** özniteliği. SqlCacheDependency değişiklikleri için veritabanı tablosunu yokladığında frekansı ayarlar. Bu değer, birbirini izleyen yoklamalar arasındaki milisaniye sayısı karşılık gelir. 500'den az milisaniye olarak ayarlanamaz. Varsayılan değer 1 dakikadır. |

### <a name="more-information"></a>Daha fazla bilgi

Önbellek yapılandırma ile ilgili farkında olmanız gereken bazı ek bilgi yok.

- Çalışan işlem özel bayt sayısı sınırı ayarlanmamışsa önbellek aşağıdaki sınırları birini kullanın: 

    - x86 2 GB: 800 MB ya da fiziksel RAM, %60 küçüktür
    - x86 3 GB: 1800 MB ya da fiziksel RAM, %60 küçüktür
    - x 64: 1 terabayttan küçük ya da fiziksel RAM, %60 küçüktür
- Her iki alt özel işlemi, bayt sınırlamak ve &lt;privateBytesLimit önbelleğe /&gt; , önbellek, en az iki kullanacağınız ayarlanmıştır.
- Gibi 1.x, biz önbellek girişlerinin bırakın ve GC çağırın. İki nedenden dolayı Topla: 

    - Özel bayt sınıra yakın çok duyuyoruz
    - Yakın veya % 10'dan kullanılabilir bellek
- Etkili bir şekilde kırpma devre dışı bırakın ve önbellek için yetersiz bellek koşulları ayarlayarak &lt;percentagePhysicalMemoryUseLimit önbelleğe /&gt; 100.
- 1.x 2.0 kırpma ve toplama çağrıları, askıya alınır son GC. Toplama özel bayt veya (önbellek) bellek sınırı % 1'den fazla tarafından yönetilen yığınların boyutunu azaltın değil.

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Özel önbellek bağımlılıkları

1. Yeni bir Web sitesi oluşturun.
2. Cache.xml adlı yeni bir XML dosyası ekleyin ve Web uygulaması kök dizinine kaydedin.
3. Aşağıdaki kod sayfasına ekleme\_yük default.aspx plan kod yöntemi: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Kaynak görünümünde default.aspx üstüne aşağıdakileri ekleyin: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Browse Default.aspx. Ne zaman dediği?
6. Tarayıcıyı yenileyin. Ne zaman dediği?
7. Cache.XML açın ve aşağıdaki kodu ekleyin: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Cache.XML kaydedin.
9. Tarayıcınızı yenileyin. Ne zaman dediği?
10. Önceden önbelleğe alınan değer yerine zaman neden güncelleştirilmiş açıklamaktadır:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Laboratuvar 2: Yoklama tabanlı önbelleği bağımlılıklarını kullanma

Bu Laboratuvar GridView ve DetailsView denetimi aracılığıyla Northwind veritabanında veri düzenleme için izin veren önceki modüldeki oluşturulan proje kullanır.

1. Projeyi Visual Studio 2005'te açın.
2. Aspnet çalıştırmak\_veritabanı ve Ürünler tablosuna etkinleştirmek için Northwind veritabanı karşı regsql yardımcı programı. Gelen bir Visual Studio komut istemi aşağıdaki komutu kullanın: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Aşağıdaki web.config dosyanıza ekleyin: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. ShowData.aspx adlı yeni bir webform ekleyin.
5. Aşağıdaki @ outputcache yönergesindeki showdata.aspx sayfasına ekleyin: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Aşağıdaki kod sayfasına ekleme\_showdata.aspx yükünü: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Yeni bir SqlDataSource denetimi için showdata.aspx ekleyin ve Northwind veritabanı bağlantısı kullanacak şekilde yapılandırın. İleri'yi tıklatın.
8. ProductName ve ProductID onay kutularını seçin ve İleri'yi tıklatın.
9. Son'u tıklatın.
10. Yeni GridView showdata.aspx sayfasına ekleyin.
11. SqlDataSource1 aşağı açılır listeden seçin.
12. Kaydet ve showdata.aspx göz atın. Görüntülenen süreyi not edin.
