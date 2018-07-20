---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Önbelleğe alma | Microsoft Docs
author: microsoft
description: İyi performans gösteren bir ASP.NET uygulaması için önbelleğe alma anlamak önemlidir. ASP.NET önbelleğe alma için; üç farklı seçenek 1.x sunulan çıktı, önbelleği...
ms.author: aspnetcontent
ms.date: 02/20/2005
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 0c38092c47060e6d02791f9672df6703852f4b5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829492"
---
<a name="caching"></a>Önbelleğe alma
====================
tarafından [Microsoft](https://github.com/microsoft)

> İyi performans gösteren bir ASP.NET uygulaması için önbelleğe alma anlamak önemlidir. ASP.NET önbelleğe alma için; üç farklı seçenek 1.x sunulan Çıktı önbelleğe alma, parça ve API önbellek.


İyi performans gösteren bir ASP.NET uygulaması için önbelleğe alma anlamak önemlidir. ASP.NET önbelleğe alma için; üç farklı seçenek 1.x sunulan Çıktı önbelleğe alma, parça ve API önbellek. Bu yöntemlerin üç ASP.NET 2.0 sunar ancak bazı önemli ek özelliklerini ekler. Geliştiriciler artık özel önbellek bağımlılıklar oluşturma seçeneğiniz vardır ve birkaç yeni önbellek bağımlılıkları vardır. Önbelleğe alma yapılandırmasını, ASP.NET 2. 0'da bir önemli ölçüde geliştirildi.

## <a name="new-features"></a>Yeni Özellikler

## <a name="cache-profiles"></a>Önbellek profilleri

Önbellek profilleri belirli sayfalara uygulanabilen özel önbellek ayarları tanımlamak geliştiricilerin sağlar. Örneğin, önbellekten 12 saat sonra süresi dolmuş olmamalıdır bazı sayfaları varsa, bu sayfalara uygulanabilir bir önbellek profili kolayca oluşturabilirsiniz. Yeni bir önbellek profili eklemek için &lt;outputCacheSettings&gt; yapılandırma dosyasının bir bölümünde. Örneğin, adında bir önbellek profili yapılandırması aşağıda verilmiştir *twoday* , 12 saatlik bir önbellek süresi yapılandırır.

[!code-xml[Main](caching/samples/sample1.xml)]

Belirli bir sayfada bu önbellek profili uygulamak için @ OutputCache yönergesi CacheProfile öznitelik aşağıda gösterildiği gibi kullanın:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Özel önbellek bağımlılıkları

ASP.NET 1.x geliştiriciler için özel bir önbellek bağımlılıklarını cried. ASP.NET'te 1.x, CacheDependency sınıfı önlenmiş geliştiriciler kendi sınıf türetmeniz gelen korumalı. ASP.NET 2. 0'da, bu sınırlama kaldırılır ve geliştiricilerin kendi özel önbellek bağımlılıklarını geliştirmek ücretsiz olarak kullanabilirsiniz. CacheDependency sınıf dosyaları, dizinleri veya önbellek anahtarları dayalı özel önbellek bağımlılığı oluşturulmasını sağlar.

Örneğin, aşağıdaki kod, Web uygulamasının kök dizininde bulunan stuff.xml adlı bir dosyaya dayalı yeni bir özel önbellek bağımlılığı oluşturur:

[!code-csharp[Main](caching/samples/sample3.cs)]

Bu senaryoda, stuff.xml dosyası değiştiğinde, önbelleğe alınan öğe geçersiz kılınır.

Önbellek anahtarları kullanarak özel bir önbellek bağımlılık oluşturmak mümkündür. Bu yöntemi kullanarak, önbellek anahtarını kaldırılmasını önbelleğe alınmış veriyi geçersiz kılar. Aşağıdaki örnek bunu göstermektedir:

[!code-csharp[Main](caching/samples/sample4.cs)]

Yukarıda eklenen öğeyi geçersiz kılmak için basitçe önbellek anahtarı davranacak şekilde önbelleğe eklenen öğeyi kaldırın.

[!code-csharp[Main](caching/samples/sample5.cs)]

Önbellek anahtarı davranan öğenin anahtarı önbellek anahtarları diziye eklenen değer ile aynı olması gerektiğini unutmayın.

## <a name="polling-based-sql-cache-dependenciesemalso-called-table-based-dependenciesem"></a>SQL önbellek bağımlılıklarını yoklama tabanlı<em>(tablo tabanlı bağımlılıkları olarak da bilinir)</em>

SQL Server 7 ve 2000 yoklama tabanlı bir modeli SQL önbellek bağımlılıklarını için kullanın. Yoklama tabanlı modeli tablosundaki verileri değiştirdiğinizde tetikleyen bir veritabanı tablosunda bir tetikleyici kullanır. Güncelleştirmeleri tetiklemek bir **Changeıd** bildirim tablosundaki ASP.NET düzenli olarak denetler. Varsa **Changeıd** alan güncelleştirildi, ASP.NET bilir verileri değiştirildi ve önbelleğe alınmış verileri çıkarır.

> [!NOTE]
> SQL Server 2005 yoklama tabanlı modeli de kullanabilirsiniz, ancak yoklama tabanlı modeli en verimli modeli olmadığından, bu (daha sonra açıklanmıştır) ve sorgu tabanlı bir modeli SQL Server 2005 ile kullanmanız önerilir.


Düzgün çalışması için yoklama tabanlı modeli kullanarak bir SQL önbellek bağımlılığı, tabloları bildirimleri etkinleştirilmiş olması gerekir. Bu program aracılığıyla SqlCacheDependencyAdmin sınıfı kullanarak gerçekleştirilebilir veya ASP.NET kullanarak\_regsql.exe yardımcı programı.

Aşağıdaki komut satırını adlı bir SQL Server örneğinde bulunan Northwind veritabanındaki Ürünler tablosuna kaydeder *dbase* SQL bağımlılık önbellek.

[!code-console[Main](caching/samples/sample6.cmd)]

Yukarıdaki komutta kullanılan komut satırı anahtarları bir açıklaması verilmiştir:

| **Komut satırı anahtarı** | **Amaç** |
| --- | --- |
| -S *sunucusu* | Sunucu adını belirtir. |
| -ed | SQL önbellek bağımlılık için veritabanı etkin olması gerektiğini belirtir. |
| -d *veritabanı\_adı* | SQL önbellek bağımlılık için etkinleştirilmesi gereken veritabanı adını belirtir. |
| -E | Bu aspnet belirtir\_regsql veritabanına bağlanırken Windows kimlik doğrulaması kullanmalıdır. |
| -et | SQL önbellek bağımlılığı bir veritabanı tablosu etkinleştirdiğini belirtir. |
| -t *tablo\_adı* | SQL önbellek bağımlılığında etkinleştirmek için veritabanı tablosunun adını belirtir. |

> [!NOTE]
> ASP.NET için kullanılabilir diğer anahtarlar vardır\_regsql.exe. Tam listesi için ASP.NET çalıştıran\_regsql.exe-? komut satırından.


Bu komut çalıştırıldığında aşağıdaki değişiklikler SQL Server veritabanına yapılan:

- Bir **AspNet\_SqlCacheTablesForChangeNotification** tablo eklenir. Bu tablo, bir SQL önbellek bağımlılık etkin veritabanındaki her tablo için bir satır içerir.
- Aşağıdaki saklı yordamlara içinde bir veritabanı oluşturulur:


| AspNet\_SqlCachePollingStoredProcedure | ASP.NET sorgular\_SqlCacheTablesForChangeNotification tablo ve SQL önbellek bağımlılığı ve her tablo için Changeıd değeri için etkinleştirilen tüm tabloları döndürür. Bu saklı yordam için yoklama veri değiştiğini belirlemek üzere kullanılır. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Tüm ASP.NET sorgulayarak SQL önbellek bağımlılık için etkinleştirilmiş tablolar döndürür\_bağımlılık önbelleğe SqlCacheTablesForChangeNotification tablo ve tüm tabloları SQL için etkin döndürür. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | SQL önbellek bağımlılığı tablosunu bildirim tablosunda gerekli giriş ekleyerek kaydeder ve tetikleyici ekler. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | SQL önbellek bağımlılığı bir tablo girişi bildirim tablosunda kaldırarak kaydını siler ve tetikleyici kaldırır. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Bildirim tablosu değişen tablosu için Changeıd artırılarak güncelleştirir. ASP.NET veri değiştiğini belirlemek üzere bu değeri kullanır. Aşağıda gösterildiği gibi bu saklı yordam tablosu etkinleştirildiğinde oluşturulmuş tetikleyicisi tarafından yürütülür. |


- Bir SQL Server tetikleyici adlı ***tablo\_adı *\_AspNet\_SqlCacheNotification\_tetikleyici** tablo için oluşturulur. Bu tetikleyiciyi yürütür AspNet\_tablosunda bir INSERT, UPDATE veya DELETE işlemi yapıldığında SqlCacheUpdateChangeIdStoredProcedure.
- Bir SQL Server rolü adlı **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** veritabanına eklenir.

**Aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** SQL Server rolü için ASP.NET EXEC izinlere sahip\_SqlCachePollingStoredProcedure. Düzgün çalışması yoklama modeli için sırayla aspnet için işlem hesabınızı eklemeniz gerekir\_ChangeNotification\_ReceiveNotificationsOnlyAccess rol. ASP.NET\_regsql.exe aracı değil bunu sizin için.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Yoklama tabanlı SQL önbellek bağımlılıklarını yapılandırma

SQL önbellek bağımlılıklarını yoklama tabanlı yapılandırma için gerekli olan birkaç adım vardır. İlk adım, veritabanı ve tablonun yukarıda açıklanan şekilde etkinleştirmektir. Bu adım tamamlandıktan sonra yapılandırmasını geri kalanını aşağıdaki gibidir:

- ASP.NET yapılandırma dosyası yapılandırma.
- SqlCacheDependency yapılandırma

### <a name="configuring-the-aspnet-configuration-file"></a>ASP.NET yapılandırma dosyasını yapılandırma

Önceki bir modülde açıklandığı gibi bir bağlantı dizesi eklemenin yanı sıra yapılandırmanız da gerekir bir &lt;önbellek&gt; öğesi ile bir &lt;sqlCacheDependency&gt; aşağıda gösterildiği gibi bir öğe:

[!code-xml[Main](caching/samples/sample7.xml)]

Bu yapılandırma üzerinde bir SQL önbellek bağımlılık sağlar. *pubs* veritabanı. PollTime özniteliği Not &lt;sqlCacheDependency&gt; öğesi varsayılan olarak 60000 milisaniye veya 1 dakika. (Bu değer 500'den az milisaniye olamaz.) Bu örnekte, &lt;ekleme&gt; öğe yeni bir veritabanı ekler ve 9000000 milisaniye ayarı pollTime geçersiz kılar.

#### <a name="configuring-the-sqlcachedependency"></a>SqlCacheDependency yapılandırma

Sonraki adım, SqlCacheDependency yapılandırmaktır. SqlDependency özniteliğinin değeri @ Outcache yönergesinde gibi belirtmek için bunu yapmaya yönelik en kolay yolu verilmiştir:

[!code-aspx[Main](caching/samples/sample8.aspx)]

SQL önbellek bağımlılık yapılandırıldı yukarıdaki @ OutputCache yönergesinde *yazarlar* tablosundaki *pubs* veritabanı. Noktalı virgülle ayrılarak birden çok bağımlılıkları yapılandırılabilir şu şekilde:

[!code-aspx[Main](caching/samples/sample9.aspx)]

SqlCacheDependency yapılandırma başka bir program aracılığıyla Bunu yapmak için yöntemdir. Aşağıdaki kod, üzerinde yeni bir SQL önbellek bağımlılık oluşturur. *yazarlar* tablosundaki *pubs* veritabanı.

[!code-csharp[Main](caching/samples/sample10.cs)]

SQL önbellek bağımlılık program aracılığıyla tanımlama avantajları oluşabilecek özel durumları işleyebilir biridir. Örneğin, bir bildirim için etkinleştirilmemiş bir veritabanı için SQL önbellek bağımlılık tanımlama denerseniz bir **DatabaseNotEnabledForNotificationException** özel durumu oluşturulur. Bu durumda, veritabanını bildirimleri çağırarak etkinleştirmek üzere deneyebilirsiniz **SqlCacheDependencyAdmin.EnableNotifications** yöntemi ve veritabanı adını geçirerek.

Benzer şekilde, bildirim için etkinleştirilmemiş bir tablo için bir SQL önbellek bağımlılık tanımlama denerseniz bir **TableNotEnabledForNotificationException** oluşturulur. Ardından çağırabilirsiniz **SqlCacheDependencyAdmin.EnableTableForNotifications** yöntemi tablo adını ve veritabanı adını geçirerek.

Aşağıdaki kod örneği, özel durum işleme, önbellek SQL bağımlılığı yapılandırırken düzgün şekilde yapılandırmak nasıl gösterir.

[!code-csharp[Main](caching/samples/sample11.cs)]

Daha fazla bilgi: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Sorgu tabanlı SQL önbellek bağımlılıklarını (yalnızca SQL Server 2005)

SQL Server 2005, SQL önbellek bağımlılığında kullanırken, yoklama tabanlı modeli gerekli değildir. SQL Server 2005 ile kullanıldığında, SQL önbellek bağımlılıklarını (başka bir yapılandırma gereklidir) SQL Server örneğine SQL bağlantı üzerinden doğrudan iletişim kurmasına kullanarak SQL Server 2005 sorgu bildirimleri.

Böylece bildirimli olarak ayarlayarak yapmak için sorgu tabanlı bildirimini etkinleştirmek için en kolay yolu olan **SqlCacheDependency** özniteliği için veri kaynağı nesnesinin **CommandNotification** ve ayarını**EnableCaching** özniteliğini **true**. Bu yöntemi kullanarak, kod gereklidir. Bir komutun sonucuna ilişkin kaynak değişiklikleri veri karşı yürütülen önbellek verileri geçersiz kılar.

Aşağıdaki örnek, bir veri kaynağı denetimi SQL önbellek bağımlılığı yapılandırır:

[!code-aspx[Main](caching/samples/sample12.aspx)]

Belirtilen sorgu, bu durumda **SelectCommand** daha farklı bir sonuç vermedi ilk değerini döndürür, önbelleğe alınan sonuçları geçersiz.

Tüm veri kaynaklarınız ayarlayarak SQL önbellek bağımlılıklarını için etkinleştirilmesi belirtebilirsiniz **SqlDependency** özniteliği **@ OutputCache** yönergesini **CommandNotification** . Aşağıdaki örnek bunu göstermektedir.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> SQL Server 2005'te sorgu bildirimleri hakkında daha fazla bilgi için SQL Server Books Online'a bakın.


SQL önbellek sorgu tabanlı bağımlılık yapılandırma başka bir yöntem Bunu yapmak için program aracılığıyla SqlCacheDependency sınıfı kullanmaktır. Aşağıdaki kod örneği, nasıl gerçekleştirilir gösterilmektedir.

[!code-csharp[Main](caching/samples/sample14.cs)]

Daha fazla bilgi: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Sonrası önbellek değiştirme

Bir sayfanın önbelleğe alma, bir Web uygulamasının performansını önemli ölçüde artırabilir. Ancak, bazı durumlarda önbelleğe alınması için sayfanın en fazla ve bazı parçaları dinamik olarak sayfada gerekir. Örneğin, belirlenen sürelerle tamamen statik bir sayfa haberleri, oluşturursanız, önbelleğe alınacak sayfanın tamamını ayarlayabilirsiniz. Her sayfa isteğinde değiştirilmiş bir döndürme ad başlık eklemek istiyorsanız, tanıtım içeren sayfasının parçası, dinamik olması gerekir. Bir sayfanın önbelleğe ancak bazı içerikleri dinamik yedek olanak tanımak için ASP.NET sonrası önbellek değişim kullanabilirsiniz. Sonrası önbellek değiştirme ile tüm çıktı önbelleği önbellek dışında tut olarak işaretlenmiş belirli bölümlerle sayfasıdır. Ad başlıklar örnekte AdRotator denetimi reklamlar her sayfa yenileme ve her kullanıcı için dinamik olarak oluşturulan sonrası önbellek değiştirme yararlanmak sağlar.

Sonrası önbellek değiştirme uygulamak için üç yolu vardır:

- Bildirimli olarak, değişim denetimi kullanma.
- Değişim Denetimi API'si programlı olarak kullanma.
- Örtülü olarak AdRotator denetim kullanma.

### <a name="substitution-control"></a>Değişim Denetimi

ASP.NET Değişim Denetimi dinamik olarak oluşturulan yerine önbelleğe önbelleğe alınmış bir sayfaya bir bölümünü belirtir. Değişim Denetimi sayfasında, dinamik içerik görünmesini istediğiniz konuma yerleştirin. Çalışma zamanında, değişim denetimi MethodName özellik belirttiğiniz bir yöntemi çağırır. Yöntemi daha sonra Değişim Denetimi içeriğini değiştirir bir dize döndürmelidir. Yöntemi, bir statik yöntem içeren sayfa ya da UserControl denetimi olması gerekir. Böylece sayfa istemcide önbelleğe alınmamış değişim denetimi kullanmak için sunucu önbelleğe alınabilirliğini, değiştirilecek, istemci tarafı önbelleğe alınabilirliğini neden olur. Bu, gelecek istekleri sayfasına dinamik içeriği yeniden oluşturmak için bir yöntemi çağırmanızı sağlar.

### <a name="substitution-api"></a>API değiştirme

Önbelleğe alınmış bir sayfaya dinamik içerik programlı olarak oluşturmak için çağırabilirsiniz [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) yöntemi, sayfa kodunuzda bir parametre olarak bir yöntemin adını geçirerek. Dinamik içerik oluşturulmasını işleyen yöntem tek bir alan [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) parametresi bir dize döndürür. Dönüş dizesi belirtilen konumda değiştirilecektir içeriktir. Değişim Denetimi bildirimli olarak kullanmak yerine WriteSubstitution yöntemini çağıran bir avantajı, sayfayı ya da UserControl nesnesinin statik bir yöntemi çağırmak yerine rastgele herhangi bir nesne bir yöntem çağırabilirsiniz olmasıdır.

Böylece sayfa istemcide önbelleğe alınmamış WriteSubstitution yöntemini çağırmak için sunucu önbelleğe alınabilirliğini, değiştirilecek, istemci tarafı önbelleğe alınabilirliğini neden olur. Bu, gelecek istekleri sayfasına dinamik içeriği yeniden oluşturmak için bir yöntemi çağırmanızı sağlar.

### <a name="adrotator-control"></a>AdRotator denetimi

Sunucu denetimi uygular AdRotator sonrası önbellek değişim için dahili olarak destekler. Bir AdRotator denetimini sayfanızda yerleştirirseniz, benzersiz olup olmadığını üst sayfası önbelleğe alınır bağımsız olarak her isteğin reklamları işlenir. Sonuç olarak, bir AdRotator denetimini içeren bir yalnızca önbelleğe alınmış sunucu tarafı sayfasıdır.

## <a name="controlcachepolicy-class"></a>ControlCachePolicy sınıfı

Kullanıcı denetimleri kullanarak önbelleğe alma parça programlı denetim için ControlCachePolicy sınıfı sağlar. ASP.NET katıştırır kullanıcı denetimleri içinde bir [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) örneği. BasePartialCachingControl sınıfı, önbelleğe alma etkin çıkış bir kullanıcı denetimi temsil eder.

Eriştiğinizde [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) özelliği bir [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) denetimi her zaman geçerli bir ControlCachePolicy nesnesi elde edersiniz. Ancak, erişirseniz [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) özelliği bir [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) denetimi, geçerli bir ControlCachePolicy nesnesi yalnızca kullanıcı denetimi zaten tarafından Sarmalanan olmadığını alır bir BasePartialCachingControl denetimi. Sarmalanmamış, ilişkili bir BasePartialCachingControl olmadığından işlemeden denediğinizde özelliği tarafından döndürülen ControlCachePolicy nesne özel durumlar. Özel durum oluşmadan önbelleğe alma bir UserControl örneği destekleyip desteklemediğini belirlemek için inceleyin [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) özelliği.

ControlCachePolicy sınıfını kullanarak çıktı önbelleği etkinleştirebilirsiniz çeşitli yollardan biridir. Aşağıdaki listede, çıkış önbelleğe almayı etkinleştirmek için kullanabileceğiniz yöntemler açıklanmaktadır:

- Kullanım [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) yönergesi etkinleştirmek için çıktı bildirim temelli bir senaryoda önbelleğe alma.
- Kullanım [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) bir arka plan kod dosyasında bir kullanıcı denetimi için önbelleğe almayı etkinleştirmek için özniteliği.
- Önceki yöntemlerden birini kullanarak önbelleği etkin ve kullanarakdinamikolarakyüklenenBasePartialCachingControlörnekleriyleiçindeçalıştığınızprogramlamasenaryolarındaönbellekayarlarınıbelirtmekiçinControlCachePolicysınıfıkullanın[System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) yöntemi.

ControlCachePolicy örneği başarıyla yalnızca Init ve PreRender aşamalarını denetim yaşam döngüsü arasında değişebilir. PreRender aşamadan sonra ControlCachePolicy nesne değiştirirseniz, ASP.NET denetimi oluşturulduğunda yapılan değişiklikler önbellek ayarları (Denetim işleme aşamasında önbelleğe alınır) gerçekte etkilemez çünkü özel durum oluşturur. Aslında işlendiğinde son olarak, bir kullanıcı denetimi örneği (ve bu nedenle ControlCachePolicy nesne) yalnızca programsal olarak için kullanılabilir.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Önbelleğe alma yapılandırmasını - değişiklikleri &lt;önbelleğe alma&gt; öğesi

Önbelleğe alma yapılandırmasını ASP.NET 2.0 için bazı değişiklikler vardır. &lt;Önbelleğe alma&gt; öğesi ASP.NET 2.0 sürümünde yenidir ve yapılandırma dosyasında önbelleğe alma yapılandırma değişiklikleri yapmanızı sağlar. Aşağıdaki öznitelikler kullanılabilir.

| **Öğesi** | **Açıklama** |
| --- | --- |
| **Önbellek** | İsteğe bağlı öğe. Genel Uygulama önbellek ayarlarını tanımlar. |
| **outputCache** | İsteğe bağlı öğe. Birçok farklı uygulama çıktı önbellek ayarlarını belirtir. |
| **outputCacheSettings** | İsteğe bağlı öğe. Uygulama sayfaları uygulanabilir çıktı önbellek ayarlarını belirtir. |
| **sqlCacheDependency** | İsteğe bağlı öğe. SQL önbellek bağımlılıklarını ASP.NET uygulaması için yapılandırır. |

### <a name="the-ltcachegt-element"></a>&lt;Önbellek&gt; öğesi

Aşağıdaki öznitelikler kullanılabilir &lt;önbellek&gt; öğesi:

| **Özniteliği** | **Açıklama** |
| --- | --- |
| **disableMemoryCollection** | İsteğe bağlı **Boole** özniteliği. Alır veya makine bellek baskısı altında olduğunda oluşan önbelleği koleksiyonu devre dışı bırakılıp bırakılmadığını belirten bir değer ayarlar. |
| **disableExpiration** | İsteğe bağlı **Boole** özniteliği. Alır veya önbellek sona erme devre dışı bırakılıp bırakılmadığını belirten bir değer ayarlar. Devre dışı bırakıldığında, önbelleğe alınmış öğeleri süresinin sona ermediğinden ve süresi dolmuş önbellek öğelerin arka plan atma oluşmaz. |
| **privateBytesLimit** | İsteğe bağlı **Int64** özniteliği. Alır veya ayarlar belirten öğeleri temizlemeye önbellek başlamadan önce bir uygulamanın özel baytlar en büyük boyutunu süresi ve belleği geri kazanmak çalışan bir değer. Bu sınır, hem önbelleği tarafından kullanılan bellek, hem de çalışan uygulama normal bellek ek yükünü içerir. Sıfır ayarı, ASP.NET başlangıç belleği geri kazanmak ne zaman belirlemek için buluşsal yöntemler, kendi kullanacağını gösterir. |
| **percentagePhysicalMemoryUsedLimit** | İsteğe bağlı **Int32** özniteliği. Öğeleri temizlemeye önbellek başlamadan önce bir uygulama tarafından tüketilebilecek bir makinenin fiziksel bellek en yüksek yüzdesi süresi belirten ve önbelleği tarafından kullanılan her iki bellek bu bellek kullanımını içeren belleği geri kazanmak çalışan bir değeri alır veya ayarlar çalışan uygulamayı normal bellek kullanımı. Sıfır ayarı, ASP.NET başlangıç belleği geri kazanmak ne zaman belirlemek için buluşsal yöntemler, kendi kullanacağını gösterir. |
| **privateBytesPollTime** | İsteğe bağlı **TimeSpan** özniteliği. Alır veya ayarlar uygulamanın özel bayt bellek kullanımı için yoklama zaman aralığını belirten bir değer. |

### <a name="the-ltoutputcachegt-element"></a>&lt;OutputCache&gt; öğesi

Aşağıdaki öznitelikler için kullanılabilir &lt;outputCache&gt; öğesi.


|       <strong>Özniteliği</strong>        |                                                                                                                                                                                                                                                       <strong>Açıklama</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          İsteğe bağlı <strong>Boole</strong> özniteliği. Sayfa çıktı önbelleğini etkinleştirir/devre dışı bırakır. Devre dışı bırakılırsa, sayfa Program erişimini ya da bildirim temelli ayarlarından bağımsız olarak önbelleğe alınır. Varsayılan değer <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                İsteğe bağlı <strong>Boole</strong> özniteliği. Uygulama parçası önbelleğini etkinleştirir/devre dışı bırakır. Devre dışı bırakılmışsa, sayfa yok bağımsız olarak, önbelleğe alınmaz [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) yönergesi veya kullanılan profilini önbelleğe alma. Yukarı Akış proxy sunucularının yanı sıra, tarayıcı istemcileri için sayfa çıktısını önbelleğe çalışmaması gerektiğini gösteren bir cache-control üst bilgisi içerir. Varsayılan değer <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      İsteğe bağlı <strong>Boole</strong> özniteliği. Belirten bir değeri alır veya ayarlar olmadığını <strong>önbellek-denetimi: özel</strong> üst bilgisi, varsayılan olarak çıktı önbelleği modülü tarafından gönderilir. Varsayılan değer <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | İsteğe bağlı <strong>Boole</strong> özniteliği. Bir Http gönderme etkinleştirir/devre dışı bırakır "<strong>ayırmayı: \</ strong ><em>" yanıt üst bilgisi. Varsayılan ayar FALSE ile bir "</em>* ayırmayı: \* <strong>" üst bilgisi, çıktı önbelleği sayfaları için gönderilir. Vary üstbilgisi gönderildiğinde için farklı izin önbelleğe alınacak sürümleri temel noktalarda üstbilgisinde belirtilen bağlı. Örneğin, <em>ayırmayı: kullanıcı-aracıları</em> Talep veren kullanıcı aracısı dayalı bir sayfa farklı sürümlerini depolar. Varsayılan değer: ** false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>&lt;OutputCacheSettings&gt; öğesi

&lt;OutputCacheSettings&gt; öğesi için daha önce açıklandığı gibi önbellek profillerinin oluşturulmasını sağlar. Yalnızca bir alt öğe için &lt;outputCacheSettings&gt; öğesi &lt;outputCacheProfiles&gt; önbellek profilleri yapılandırma öğesi.

### <a name="the-ltsqlcachedependencygt-element"></a>&lt;SqlCacheDependency&gt; öğesi

Aşağıdaki öznitelikler için kullanılabilir &lt;sqlCacheDependency&gt; öğesi.

| **Özniteliği** | **Açıklama** |
| --- | --- |
| **Etkin** | Gerekli **Boole** özniteliği. Değişiklikler için yoklanıyor olup olmadığını gösterir. |
| **pollTime** | İsteğe bağlı **Int32** özniteliği. SqlCacheDependency veritabanı tablosu değişiklikleri için yoklama sıklığı ayarlar. Bu değer, birbirini izleyen yoklamalar arasındaki milisaniye sayısını karşılık gelir. 500'den az milisaniye ayarlanamaz. Varsayılan değer 1 dakikadır. |

### <a name="more-information"></a>Daha fazla bilgi

Önbelleği yapılandırma ile ilgili farkında olmanız gereken bazı ek bilgiler yok.

- Çalışan işlem özel baytlar sınırı ayarlanmazsa, önbellek aşağıdaki sınırlar birini kullanır: 

    - x86 2 GB: 800 MB veya fiziksel RAM, 60 oranında küçüktür
    - x86 3 GB: 1800 MB veya fiziksel RAM, 60 oranında küçüktür
    - x 64: 1 terabayt ya da fiziksel RAM'in %60 küçüktür
- Her iki çalışan özel işlemi, bayt kısıtlamak ve &lt;privateBytesLimit önbellek /&gt; , önbellek, en az iki kullanacağı ayarlanmıştır.
- Tıpkı 1.x, biz önbellek girişlerinin bırakın ve GC çağırın. İki nedenden dolayı Topla: 

    - Özel bayt sınırına yakın çok duyuyoruz
    - Yakın veya % 10'dan kullanılabilir bellek
- Etkili bir şekilde kesim devre dışı bırakın ve önbellek düşük bellek koşullarını ayarlayarak &lt;percentagePhysicalMemoryUseLimit önbellek /&gt; 100.
- 1.x, 2.0 kırpma ve toplama çağrıları, askıya alırız son GC. Toplama veya özel bayt (önbellek) bellek sınırının % 1'den fazla tarafından yönetilen yığınlar boyutunu azaltın değil.

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Özel önbellek bağımlılıklarını

1. Yeni bir Web sitesi oluşturun.
2. Cache.xml adlı yeni bir XML dosyası ekleyin ve Web uygulaması kök dizinine kaydedin.
3. Aşağıdaki kod sayfasına ekleme\_yük default.aspx plan kod yöntemi: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Kaynak Görünümü'nde default.aspx en üstüne aşağıdakileri ekleyin: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Default.aspx göz atın. Ne zaman söylüyor?
6. Tarayıcıyı yenileyin. Ne zaman söylüyor?
7. Cache.XML açın ve aşağıdaki kodu ekleyin: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Cache.XML kaydedin.
9. Tarayıcınızı yenileyin. Ne zaman söylüyor?
10. Daha önce önbelleğe alınmış değerler yerine zaman neden güncelleştirilmiş açıklar:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Laboratuvar 2: Yoklama temelli önbellek bağımlılıklarını kullanma

Bu Laboratuvar GridView ve DetailsView denetimi aracılığıyla Northwind veritabanındaki verilerin düzenleme için izin veren önceki modülde oluşturduğunuz proje kullanır.

1. Visual Studio 2005'te projeyi açın.
2. ASP.NET çalıştırma\_veritabanı ve ürünler tablosu etkinleştirmek için Northwind veritabanı karşı regsql yardımcı programı. Gelen bir Visual Studio komut istemi aşağıdaki komutu kullanın: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Web.config dosyasına aşağıdakileri ekleyin: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. ShowData.aspx adlı yeni bir Web formu ekleyin.
5. Aşağıdaki @ outputcache yönergesi showdata.aspx sayfasına ekleyin: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Aşağıdaki kod sayfasına ekleme\_showdata.aspx yükü: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Showdata.aspx için yeni bir SqlDataSource denetimi ekleyin ve Northwind veritabanı bağlantısı kullanacak şekilde yapılandırın. İleri'ye tıklayın.
8. ProductName ve ProductID onay kutularını seçin ve İleri'ye tıklayın.
9. Son'a tıklayın.
10. Yeni GridView showdata.aspx sayfasına ekleyin.
11. SqlDataSource1 açılan listeden seçin.
12. Kaydet ve showdata.aspx göz atın. Görüntülenen süreyi not edin.
