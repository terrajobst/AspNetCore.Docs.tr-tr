---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: "Ek: Düzelt örnek uygulaması (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) | Microsoft Docs"
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: baf46a87155e6368d9a81c5c5b777a491117d7b6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754938"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Ek: Düzelt örnek uygulaması (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Bu proje düzeltmeyi indirin](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. E-kitabı hakkında daha fazla bilgi için bkz. [ilk bölüm](introduction.md).


Bu ekte gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap için indirebileceğiniz Düzelt örnek uygulaması hakkında ek bilgi sağlayan aşağıdaki bölümleri içerir:

- [Bilinen sorunlar](#knownissues)
- [En iyi uygulamalar](#bestpractices)
- [Uygulamayı yerel bilgisayarınızda Visual Studio'dan çalıştırma](#run-in-vs)
- [Windows PowerShell komut dosyalarını kullanarak Azure App Service Web Apps için temel uygulama dağıtma](#deploybase)
- [Windows PowerShell komut dosyaları sorunlarını giderme](#troubleshooting)
- [Azure App Service Web Apps ve Azure bulut hizmeti için işleme sırası ile uygulama dağıtma](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Bilinen sorunlar

Düzeltme uygulama desenleri bazıları bu e-kitapta tanıtılan olabildiğince basit bir şekilde göstermek için ilk olarak geliştirilmiştir. E-kitap, gerçek uygulamaları oluşturma hakkında olduğundan, ancak biz Düzelt kod gözden geçirme ve test işlemi ne için yayımlanan yazılım yapmamız için benzer tabi. Birkaç sorun bulduk ve tüm gerçek uygulamada olduğu gibi bunların bazılarını düzelttik ve bunların bazılarını daha sonraki bir sürüme biz ertelenmiş.

Aşağıdaki listede, bir üretim uygulaması, ancak bir sebeple veya başka Düzelt örnek uygulamanın ilk sürümde adresine değil verdik ele alınması gereken sorunları içerir.

### <a name="security"></a>Güvenlik

- Bir görev var olmayan bir kullanıcıya atanamaz emin olun.
- Yalnızca yapabilecekleriniz olun görüntüleyin ve oluşturduğunuz veya size atanan görevlerin değiştirin.
- Oturum açma sayfaları ve kimlik doğrulaması tanımlama bilgileri için HTTPS kullanır.
- Kimlik doğrulama tanımlama bilgisi için bir süre belirtin.

### <a name="input-validation"></a>Giriş doğrulaması

Genel olarak, bir üretim uygulaması yaptığınız Düzelt uygulama daha fazla giriş doğrulama. Örneğin, görüntü boyutu / görüntü karşıya yükleme sınırlandırılmalıdır için izin verilen maksimum dosya boyutu.

### <a name="administrator-functionality"></a>Yönetici işlevi

Yöneticinin mevcut görevlerde sahipliğini değiştirmek görebilmeniz gerekir. Örneğin, bir görev oluşturan hiç yönetici erişimi etkinleştirilmediği sürece Görev korumak yetkilisiyle bırakarak şirket bırakabilir.

### <a name="queue-message-processing"></a>Kuyruk ileti işleme

Kuyruk iletisi Düzelt uygulamada işleme minimum miktarda kod kuyruk merkezli çalışma deseni göstermek için basit olacak şekilde tasarlanmıştır. Bu basit kod bir gerçek bir üretim uygulaması için yeterli olmaz.

- Kod, her bir kuyruk iletisi en fazla bir kez işlenir garanti etmez. Kuyruktan bir ileti aldığınızda iletiyi diğer kuyruk dinleyicilere görünmez bir zaman aşımı süresi yoktur. İleti silinmeden önce zaman aşımı süresi dolarsa, ileti yeniden görünür hale gelir. Bir çalışan rolü örneği bir ileti işlenirken zaman harcıyorsa, bu nedenle, yinelenen bir görevi veritabanında kaynaklanan aynı ileti iki kez işlenemedi teorik olarak mümkündür. Bu sorun hakkında daha fazla bilgi için bkz. [kullanarak Azure depolama kuyrukları](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- Kuyruk yoklama mantığı ileti alma toplu işleme göre daha uygun maliyetli olabilir. Çağırmanızı her zaman [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), işlem maliyeti yoktur. Bunun yerine, çağırabilirsiniz [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (çoğul'ın unutmayın '), tek bir işlemde birden çok ileti alır. Azure depolama kuyruklarına ilişkin işlem maliyetlerini çok düşük olduğundan maliyetleri üzerindeki etkiyi Çoğu senaryoda önemli değildir.
- Kuyruk ileti işleme kodunu sıkı bir döngüde çok çekirdekli sanal makineleri verimli bir şekilde kullanmaz CPU benzeşimi, neden olur. Daha iyi bir tasarım, birden çok zaman uyumsuz görevleri paralel olarak çalıştırmak için görev paralelliği kullanmanız gerekir.
- Kuyruk ileti işleme yalnızca ilkel bir özel durum işleme yok. Örneğin, kod işlemiyor [zehirli iletiler](https://msdn.microsoft.com/library/ms789028.aspx). (Bir özel durum iletisi işleme neden olduğunda için hata günlüğüne ve iletiyi silmek için sahip olduğunuz veya çalışan rolü yeniden dener ve döngü süresiz olarak devam eder.)

### <a name="sql-queries-are-unbounded"></a>SQL sorguları sınırsız

Düzelt geçerli kod dizin sayfalarını sorgularında döndürebilir kaç satır sınır yerleştirir. Büyük miktarlarda görev veritabanına girilir, alınan sonuç listesi boyutu performans sorunlarına neden olabilecek. Çözüm, disk belleği uygulamaktır. Bir örnek için bkz. [sıralama, filtreleme ve bir ASP.NET MVC uygulamasında Entity Framework ile sayfalama](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Önerilen modelleri görüntüle

Düzeltme uygulama FixItTask varlık sınıfı denetleyici ve görünüm arasında bilgi geçirmek için kullanır. Görünüm modelleri kullanmak iyi bir uygulamadır. Etki alanı modeli (örneğin, FixItTask varlık sınıfı) bir görünüm modeli verilerini sunumu için tasarlanabilir veri kalıcılığı için gerekli geçici olarak tasarlanmıştır. Daha fazla bilgi için [12 ASP.NET MVC en iyi uygulamaları](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Önerilen güvenli görüntü blob'u

Düzelt uygulama mağazaları genel URL bulur herkes görüntüleri erişebileceği anlamına gelir görüntüleri karşıya yüklendi. Görüntüleri yerine genel güvenli.

### <a name="no-powershell-automation-scripts-for-queues"></a>Kuyruklar için hiçbir PowerShell Otomasyon betikleri

Örnek PowerShell Otomasyon betikleri, yalnızca temel sürümü düzeltme tamamen Azure App Service Web Apps'te çalışan için yazılmıştır. Biz ayarlama ve web uygulama ve kuyruk işlenmesi için gereken bulut hizmeti ortamı dağıtmak için betikler sağlamıyordu.

### <a name="special-handling-for-html-codes-in-user-input"></a>Kullanıcı girişini HTML kodları için özel işleme

ASP.NET, kullanıcı giriş metin kutularına betiği girerek siteler arası betik saldırıları kötü niyetli kullanıcılar çalışabilir birçok yolu otomatik olarak engeller. Ve MVC `DisplayFor` görev görüntülemek için kullanılan yardımcı başlıklar ve tarayıcıya gönderir ve HTML olarak kodlar değerleri otomatik olarak notlar. Ancak bir üretim uygulamasında ek önlemler almak isteyebilirsiniz. Daha fazla bilgi için [ASP.NET'te doğrulama isteği](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Önerilen uygulamalar

Kod incelemesi içinde bulunan ve düzeltme uygulamanın orijinal sürümünü test kaldıktan sonra düzeltilen bazı sorunlar aşağıda verilmiştir. Bazı bazılarını yalnızca belirli bir en iyi uygulama farkına varmadan değil özgün Kodlayıcı tarafından kaynaklanan kod hızlı bir şekilde yazılmış ve yayınlanan yazılım için hedeflenen değildi. Durumunda bu gözden geçirme sürecinden öğrendiğimiz bir şey sorunları burada listeleniyor ve sınama Ayrıca web uygulamaları geliştiren kişilere yararlı olabilir.

### <a name="dispose-the-database-repository"></a>Veritabanı havuzu dispose

`FixItTaskRepository` Sınıfı, varlık çerçevesi dispose gerekir `DbContext` örneği. Bu uygulama tarafından yaptığımız `IDisposable` içinde `FixItTaskRepository` sınıfı:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

AutoFac otomatik olarak dispose Not `FixItTaskRepository` örnek biz açıkça elden yapmak zorunda kalmazsınız.

Kaldırmak için başka bir seçenektir `DbContext` üye değişkeninin `FixItTaskRepository`ve bunun yerine yerel bir oluşturma `DbContext` her depo yöntemi içinde değişken içine bir `using` deyimi. Örneğin:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Teklileri DI, bu nedenle kaydedin.

Yalnızca bir örneğini beri `PhotoService` sınıfı ve `Logger` sınıfı gerekiyor, bu sınıflar olmalıdır [tek örnekleri için bağımlılık ekleme olarak kayıtlı](https://code.google.com/p/autofac/wiki/InstanceScope) içinde *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Güvenlik: kullanıcıları için hata ayrıntılarını gösterme

Düzelt özgün uygulama genel hata sayfası ve veritabanı bağlantı hataları gibi bazı özel durumlar tarayıcıda görüntülenen bir tam yığın izlemesi neden olabilir, böylece yalnızca kullanıcı Arabirimi kadar tüm özel durumları Kabarcık izin vermedi. Ayrıntılı hata bilgileri kötü amaçlı kullanıcılar tarafından saldırıları bazen kolaylaştırabilir. Özel durum ayrıntılarını ve hata ayrıntılarını içermez kullanıcıya bir hata sayfası görüntüleme çözümüdür. Düzeltme uygulama zaten günlüğe kaydetme ve bir hata sayfası görüntüleme için ekledik `<customErrors mode=On>` Web.config dosyasında.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Varsayılan olarak bu neden *Views\Shared\Error.cshtml* hataları görüntülenecek. Özelleştirebileceğiniz *Error.cshtml* veya kendi hata sayfası görünümü oluşturma ve ekleme bir `defaultRedirect` özniteliği. Ayrıca, belirli hataları için farklı hata sayfaları belirtebilirsiniz.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Güvenlik: yalnızca bir görev oluşturucusu tarafından düzenlenecek izin ver

Pano dizini sayfasında, yalnızca oturum açmış kullanıcı tarafından oluşturulan görevler gösterilmektedir, ancak kötü niyetli bir kullanıcı başka bir kullanıcının Görev Kimliğine sahip bir URL oluşturabilirsiniz. Kod olarak eklediğimiz *DashboardController.cs* bu durumda bir 404 döndürmek için:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Özel durumlar swallow yok

Düzelt özgün uygulamanın yalnızca bir SQL sorgudan sonuçlanan bir özel durum günlüğe kaydetme sonra null döndürdü:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Bu, kullanıcıya sorgu başarılı oldu, ancak hiçbir satır döndürmedi. yalnızca gibi görünmesini hale getirir. Yakalama ve günlük sonra özel durumu yeniden harekete geçirileceğini çözümüdür:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Çalışan rolleri tüm özel durumları Yakala

İşlenmemiş özel durumlar bir çalışan rolünde geri dönüştürülmesi VM neden olur, her şeyi sarmalamak istediğiniz şekilde bir try-catch bloğu içinde yapın ve tüm özel durumları işleme.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Varlık sınıflarında dize özellikleri için uzunluğu belirtin

Basit kod görüntülemek için düzeltme uygulama orijinal sürümünü alanlar için uzunluk FixItTask varlık belirtmediniz ve sonuç olarak, veritabanındaki varchar(max) olarak tanımlanan. Sonuç olarak, kullanıcı Arabirimi, neredeyse her miktarda giriş kabul eder. Web sayfası ve sütun boyutu veritabanındaki kullanıcı için her ikisinin de geçerli belirten uzunlukları kümeleri sınırları girin:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Değiştirmek için beklenen olmayan yüklerken özel üyeler readonly olarak işaretle

Örneğin, `DashboardController` sınıfının bir örneğini `FixItTaskRepository` oluşturulur ve olarak tanımladığımız şekilde değiştirmek için beklenen değil [salt okunur](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Listeyi kullanın. Any() yerine listesi. Count() işlevi &gt; 0

Önem verdiğiniz tüm, ister bir listedeki öğeler bir veya daha fazla belirtilen ölçütlere uyan kullanın [herhangi](https://msdn.microsoft.com/library/bb534972.aspx) yöntemi ölçütleri sığdırma bir öğe bulundu hemen sonra oysa döndürüldüğünden `Count` yöntemi her zaman sahip yinelemek her öğe arasında. Pano *Index.cshtml* dosya ilk olarak bu kodu sahipti:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Bunun için bunu değiştirdik:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>MVC görünümleri MVC Yardımcıları kullanarak URL üretmek

İçin **bir düzeltme oluşturmak** düğmesi giriş sayfasındaki Düzelt uygulama sabit kodlanmış bağlayıcı bir öğe:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Bunun gibi görünüm/eylem bağlantıları için bunu kullanmak en iyisidir [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) HTML Yardımcısı, örneğin:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Task.Delay yerine net_thread_sleep çalışan rolünde kullanın.

Yeni Proje şablonu koyar `Thread.Sleep` örnekte gereksiz ek iş parçacığı üretme iş parçacığı havuzu bir çalışan rolü, ancak iş parçacığının Uyku neden kodu neden olabilir. Bunu kullanarak önlemek [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) yerine.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Zaman uyumsuz void kaçının

Zaman uyumsuz bir yöntem bir değer döndürmesi gerekmez, iade bir `Task` türü yerine `void`.

Bu örnekte dandır `FixItQueueManager` sınıfı: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Kullanmanız gereken `async void` yalnızca üst düzey olay işleyicileri için. Bir yöntem olarak tanımlarsanız `async void`, çağırana olamaz **await** yöntemi veya yöntemin oluşturduğu özel durumları yakalama. Daha fazla bilgi için [iyi zaman uyumsuz programlama](https://msdn.microsoft.com/magazine/jj991977.aspx). 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Çalışan rolü döngüden ayırmak için bir iptal belirteci kullanma

Genellikle, **çalıştırma** bir çalışan rolü yöntemi sonsuz bir döngü içeriyor. Çalışan rolü durdururken, [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) yöntemi çağrılır. İçinde yapılan işi iptal etmek için bu yöntem kullanmalısınız **çalıştırın** yöntemi ve çıkış gerçekleşecektir. Aksi takdirde, işlemin ortasında bir işlem sonlandırılacak.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Otomatik MIME algılaması yordamı dışında iyileştirilmiş

Bazı durumlarda, Internet Explorer web sunucusu tarafından belirtilen türü farklı bir MIME türü bildirir. Örneğin, Internet Explorer HTML HTTP yanıt üst bilgisi ile Content-Type teslim dosyasındaki içerik bulursa: metin/düz, Internet Explorer içeriğini HTML olarak işleneceğini belirler. Ne yazık ki bu "MIME-algılaması" da güvenilmeyen içerik barındıran sunucular için güvenlik sorunlarına neden olabilir. Bu sorunu gidermek için Internet Explorer 8 MIME türünü belirleme kodu bir dizi değişiklik getirmiştir ve uygulama geliştiricilerinin sağlayan [MIME algılaması dışında iyileştirilmiş](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). Aşağıdaki kod eklendi *Web.config* dosya.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Paketleme ve küçültme etkinleştir

Visual Studio, yeni bir web projesi oluşturduğunda, paketleme ve küçültme JavaScript dosyalarının değil varsayılan olarak etkindir. Bir kod satırı BundleConfig.cs ekledik:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Kimlik doğrulama tanımlama bilgilerinin bir sona erme zaman aşımını ayarlama

Varsayılan olarak, kimlik doğrulama tanımlama bilgisi iki hafta içinde süresi dolar. Daha kısa bir süre daha güvenlidir. Bu ayarı değiştirebilirsiniz *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Uygulamayı yerel bilgisayarınızda Visual Studio'dan çalıştırma

Düzelt uygulamayı çalıştırmak için iki yolu vardır:

- Yeni görevleri doğrudan SQL veritabanına yazar temel uygulamayı çalıştırın.
- Görevleri oluşturmak üzere bir kuyruk yanı sıra bir arka uç hizmetini kullanarak uygulamayı çalıştırın. Kuyruk düzeni bölümde açıklanan [kuyruk merkezli çalışma deseni](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Temel uygulamayı çalıştırın

1. Yükleme [Visual Studio 2013 veya Web için Visual Studio 2013 Express](https://www.visualstudio.com/downloads).
2. Yükleme [Visual Studio 2013 için .NET için Azure SDK.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. .Zip dosyasından indirme [MSDN Kod Galerisi](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. Dosya Gezgini'nde, .zip dosyasını sağ tıklayın ve Özellikler'e tıklayın, sonra Özellikler penceresinde engelini Kaldır'a tıklayın.
5. Dosyanın sıkıştırmasını açın.
6. Visual Studio'yu başlatmak .sln dosyasına çift tıklayın.
7. Araçlar menüsünden kitaplık Paket Yöneticisi'ni ve ardından Paket Yöneticisi konsolu tıklayın.
8. Paket Yöneticisi Konsolu (PMC'de), geri yükleme'yi tıklatın.
9. Visual Studio'dan çıkın.
10. Başlangıç [Azure storage öykünücüsü](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx).
11. Visual Studio'yu yeniden başlatın, çözüm dosyasını açmadan önceki adımda kapalı.
12. Düzelt projeyi başlangıç projesi olarak ayarlandığından emin olun ve ardından projeyi çalıştırmak için CTRL + F5 tuşuna basın.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Kuyruk işleme ile uygulamayı çalıştırın

1. İçin yönergeleri izleyin [temel uygulamayı çalıştırmak](#runbase), tarayıcıyı kapatın ve ardından Visual Studio'yu kapatın.
2. Visual Studio'yu yönetici ayrıcalıklarıyla başlatın. (Azure işlem öykünücüsü kullanarak ve yönetici ayrıcalıkları gerektirir.)
3. Uygulamada *Web.config* dosyası *MyFixIt* değerini değiştirebilir, proje (web projesi) `appSettings/UseQueues` "true": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Varsa [Azure storage öykünücüsü](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) hala çalışıyor, olmadığından yeniden başlatın.
5. Aynı anda Düzelt web projesini ve MyFixItCloudService projesini çalıştırın.

    Visual Studio 2013'ü kullanarak:

   1. Düzelt projeyi çalıştırmak için F5 tuşuna basın.
   2. İçinde **Çözüm Gezgini**MyFixItCloudService projeye sağ tıklayın ve ardından **hata ayıklama** -- **yeni örnek Başlat**.

      Web için Visual Studio 2013 Express kullanarak:

   3. Çözüm Gezgini'nde Düzelt çözüme sağ tıklayıp seçin **özellikleri**.
   4. Seçin **birden fazla başlangıç projesi**...
   5. İçinde **eylem** MyFixIt ve MyFixItCloudService, altındaki açılır listede seçin **Başlat**.
   6. **Tamam**'ı tıklatın.
   7. İki projeyi de çalıştırmak için F5 tuşuna basın.

      MyFixItCloudService Projeyi çalıştırdığınızda, Visual Studio Azure işlem öykünücüsü başlatır. Güvenlik Duvarı'nı yapılandırmanıza bağlı olarak, güvenlik duvarı üzerinden öykünücü izin gerekebilir.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Windows PowerShell komut dosyalarını kullanarak Azure App Service Web Apps için temel uygulama dağıtma

Göstermek için [her şeyi otomatikleştirin](automate-everything.md) deseni, düzeltme uygulama, azure'daki bir ortamı ayarlayın ve projeyi yeni ortama dağıtmak betiklerle sağlanmaktadır. Aşağıdaki yönergeler, komut dosyalarının nasıl kullanılacağı açıklanmaktadır.

Kuyrukları kullanarak olmadan Azure'da çalıştırmak istediğiniz ve kuyrukları ile yerel olarak çalıştırmak için değişiklikler, aşağıdaki yönergelere geçmeden önce false dön UseQueues appSetting değeri ayarlamak emin olun.

Bu yönergeler zaten indirmiş ve Düzelt çözümü yerel olarak çalıştırın ve sahip olduğunuz bir Azure hesabı veya bir Azure aboneliğine sahip varsayar Yönetme yetkisine sahip.

1. Yükleme **Azure PowerShell** Konsolu. Yönergeler için [Azure PowerShell'i yükleme ve yapılandırma işlemini](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Bu özelleştirilmiş konsol, Azure aboneliğinizle çalışacak şekilde yapılandırılmıştır. Azure Modülü yüklü *Program dosyaları* directory ve Azure PowerShell konsolunu her kullanımı hakkında otomatik olarak içeri aktarılır.

    Windows PowerShell ISE'yi gibi farklı bir konak programında çalışmayı tercih ederseniz kullandığınızdan emin olun [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) cmdlet'ini Azure modülünü içeri aktarın veya otomatik modül içeri tetiklemek için Azure modülünde bir komut kullanın.
2. Azure PowerShell ile başlatma **yönetici olarak çalıştır** seçeneği.
3. Çalıştırma [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) Azure PowerShell yürütme ilkesini ayarlamak için cmdlet `RemoteSigned`. Girin **Y** (Evet için) ilke değişikliğinin tamamlanması.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Bu ayar, dijital olarak imzalı olmayan yerel betikleri çalıştırmanıza olanak sağlar. (De yürütme ilkesi ayarlayabilirsiniz `Unrestricted`hangi engellemeyi kaldırma adımı daha sonra gereksinimini ortadan, ancak güvenlik nedeniyle önerilmez.)
4. Çalıştırma `Add-AzureAccount` hesabınızın kimlik bilgileriyle PowerShell cmdlet'ini.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Bu kimlik bilgilerinin bir süre sonra süresi dolar ve yeniden çalıştırmak sahip olduğunuz `Add-AzureAccount` cmdlet'i. Bu e-kitap yazılmış gibi kimlik bilgilerinin süresi sona ermeden önce zaman sınırını 12 saati geçmez.
5. Birden fazla aboneliğiniz varsa, test ortamında oluşturmak istediğiniz aboneliği belirtmek için Select-AzureSubscription cmdlet'ini kullanın.
6. Aynı Azure aboneliği için yönetim sertifikası kullanarak içeri `Get-AzurePublishSettingsFile` ve `Import-AzurePublishSettingsFile` cmdlet'leri. Bir sertifika dosyası birinci bu cmdlet'lerin indirir ve ikinci bir almak için bu dosyanın konumunu belirtin. > [!IMPORTANT]
   > İndirilen dosyayı güvenli bir yerde saklayın veya bu ile işiniz bittiğinde, Azure hizmetlerinizi yönetmek için kullanılan bir sertifika içerdiğinden silin.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Sertifika kullanılır SQL veritabanı sunucusunda bir güvenlik duvarı kuralını ayarlamak için geliştirme makinenin IP adresi algılar bir REST API çağrısı.
7. Çalıştırma [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (diğer adlar `cd`, `chdir`, ve `sl`) komut dosyaları içeren dizine gidin. (Bulunan *Otomasyon* Düzelt Çözüm klasörü klasöründe.) Herhangi bir dizin adları boşluk içeriyorsa, yolu tırnak işaretleri içine alın. Örneğin, gitmek için `c:\Sample Apps\FixIt\Automation` dizin şu komutu yazabilirsiniz:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Bu komut dosyalarını çalıştırmak Windows PowerShell izin vermek üzere [Engellemeyi Kaldır-dosya](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet'i. (Internet'ten karşıdan yüklendiği için betikler engellenir.)

    > [!WARNING]
    > Güvenlik - çalıştırmadan önce `Unblock-File` her betik veya yürütülebilir dosya, dosyasını Not Defteri'nde açın, komutları inceleyin ve bunlar tüm kötü amaçlı kod içerip içermediğini doğrulayın.

    Örneğin, aşağıdaki çalıştırmalar komut `Unblock-File` cmdlet'ini geçerli dizindeki tüm betikler.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Web uygulaması için temel (kuyruklar işleme) oluşturmak için uygulama düzeltmek için ortam oluşturma betiği çalıştırın.

    Gerekli `Name` parametresi veritabanı adını belirtir ve betiğin oluşturduğu depolama hesabı için de kullanılır. Ad azurewebsites.net etki alanı içinde genel olarak benzersiz olmalıdır. Düzelt veya Test gibi (veya hatta fixitdemo örnekte olduğu gibi), benzersiz olmayan bir ad belirtirseniz `New-AzureWebsite` cmdlet'i bir iç bir çakışma raporların hata ile başarısız olur. Betik adı tamamen küçük harflerden oluşan web uygulamaları, depolama hesapları ve veritabanları için ad gereksinimleri ile uyum sağlamak için dönüştürür.

    Gerekli `SqlDatabasePassword` parametresi, SQL veritabanı için oluşturulacak yönetici hesabının parolasını belirtir. Özel XML karakterlerinden parola eklemeyin (&amp; &lt; &gt; ;). Bu komut dosyalarının yazıldığı, bir kısıtlaması değil Azure yolu kısıtlamasıdır.

    Örneğin, "fixitdemo" adlı bir web uygulaması oluşturma ve bir SQL Server Yönetici parolasını "Passw0rd1" kullanmak istiyorsanız, aşağıdaki komutu girebilirsiniz:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Bu ad azurewebsites.net etki alanında benzersiz olması gerekir ve parola, parola karmaşıklığını SQL veritabanı gereksinimlerini karşılaması gerekir. (Örnek Passw0rd1 gereksinimlerini karşılamıyor.)

    Komutu ile başlayıp başlamadığını Not ". \". Kötü amaçlı betikler yürütülmesini önlemeye yardımcı olmak için bir komut çalıştırdığınızda betik dosyasının tam yolunu sağlamanız Windows PowerShell gerektirir. Bir nokta geçerli dizini belirtmek için kullanabilirsiniz (".\") veya tam yolunu sağlayın:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Komut dosyası hakkında daha fazla bilgi için `Get-Help` cmdlet'i.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Kullanabileceğiniz `Detailed`, `Full`, `Parameters`, ve `Examples` döndürülen Yardım filtrelemek için Get-Help cmdlet parametreleri.

    Betik başarısız olursa veya, "New-AzureWebsite: çağrı Set-AzureSubscription ve Select-AzureSubscription ilk olarak," gibi hatalar oluşturur Azure PowerShell yapılandırması tamamlanmamış.

    Betik bittikten sonra Azure Yönetim Portalı gösterildiği gibi oluşturulmuş olan kaynakları görmek için kullanabileceğiniz [her şeyi otomatikleştirin](automate-everything.md) bölüm.
10. Düzelt proje için yeni Azure ortamına dağıtmak için kullanın *AzureWebsite.ps1* betiği. Örneğin:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Dağıtım tamamlandığında, onunla düzeltme Azure'da çalıştırılan tarayıcı açılır.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Windows PowerShell komut dosyaları sorunlarını giderme

Bu betikler çalışırken karşılaşılan en yaygın hataları izinleri ilgilidir. Emin olun `Add-AzureAccount` ve `Import-AzurePublishSettingsFile` başarılı ve aynı Azure aboneliği için kullanılır. Bile `Add-AzureAccount` olan başarılı, yeniden çalıştırmak sahip olabilir. Tarafından eklenen izinleri `Add-AzureAccount` 12 saat içinde süresi dolacak.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Nesne başvurusu bir nesnenin örneğine ayarlı değil.

Betiği Çalıştır "Windows PowerShell bir nesneye (Bu, bir null başvurusu özel durumu) işlem bulunamıyor anlamına gelir, bir nesnenin örneğine ayarlı değil nesne başvurusu" gibi hatalar, döndürürse `Add-AzureAccount` cmdlet'i ve komut dosyasını yeniden deneyin.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>Internalerror: Sunucu bir iç hatayla karşılaştı.

`New-AzureWebsite` Ad azurewebsites.net etki alanında benzersiz değilse cmdlet bir iç hata döndürür. Hatayı gidermek için Name parametresinde adı için farklı bir değer kullanın *yeni AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Betiği yeniden başlatılıyor

Yeniden başlatmanız gerekirse *yeni AzureWebsiteEnv.ps1* betik "Betik tamamlandı" iletisi yazdırmadan önce başarısız olduğundan, daha önce oluşturduğunuz betiğin durduruldu kaynaklarını silmek isteyebilirsiniz. Örneğin, ContosoFixItDemo web uygulaması ve komut dosyasını aynı adla yeniden çalıştırın. komut dosyası zaten oluşturduysanız, betik adının kullanımda olduğu için başarısız olur.

Hangi kaynakların durdurulmadan önce oluşturulan betik belirlemek için aşağıdaki cmdlet'leri kullanın:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Bu cmdlet'i çalıştırmak için veritabanı sunucusu adını kanal oluşturarak `Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Bu kaynakları silmek için aşağıdaki komutları kullanın. Veritabanı sunucusu silerseniz, otomatik olarak sunucuyla ilişkili veritabanlarını silmeniz gerektiğini unutmayın.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Azure App Service Web Apps ve Azure bulut hizmeti için işleme sırası ile uygulama dağıtma

Kuyruklar etkinleştirmek için MyFixIt\Web.config dosyasına aşağıdaki değişikliği yapın. Altında `appSettings`, değiştirin `UseQueues` "true": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Daha sonra açıklandığı MVC uygulamasını Azure App Service'te bir web uygulamasına dağıtma [önceki](#deploybase).

Ardından, yeni bir Azure bulut hizmeti oluşturun. Düzelt uygulamayla dahil betikleri oluşturamadı veya bunun için Azure portalını kullanmanız gerekir böylece bulut hizmeti dağıtın. Portalında **yeni** -- **işlem** – **bulut hizmeti** -- **hızlı Oluştur**ve ardından bir URL girin ve veri merkezi konumu. Web uygulaması dağıtıldığı aynı veri merkezinde kullanın.

![](the-fix-it-sample-application/_static/image1.png)

Bulut hizmeti dağıtmadan önce bazı yapılandırma dosyalarını güncelleştirmeniz gerekir.

MyFixIt.WorkerRoler\app.config içinde altında `connectionStrings`, değiştirin `appdb` SQL veritabanı için gerçek bağlantı dizesiyle bağlantı dizesi. Bağlantı dizesini portaldan alabilirsiniz. Portalında **SQL veritabanları** - **appdb** - **görünümü SQL veritabanı bağlantı dizelerini ADO .net, ODBC, PHP ve JDBC**. ADO.NET bağlantı dizesini kopyalayın ve değeri app.config dosyasına yapıştırın. Değiştir "{,\_parola\_burada}" veritabanı parolanızla. (MVC uygulaması dağıtmak için betikler kullandığınız varsayılarak, veritabanı Parolada belirtilen `SqlDatabasePassword` parametresi komut dosyası.)

Sonuç aşağıdaki gibi görünmelidir:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

Aynı MyFixIt.WorkerRoler\app.config dosyada altında `appSettings`, Azure depolama hesabı için iki yer tutucu değerlerini değiştirin.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Erişim anahtarı portaldan alabilirsiniz. Bkz: [depolama hesaplarını yönetme](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

MyFixItCloudService\ServiceConfiguration.Cloud.cscfg Azure depolama hesabı için aynı iki yer tutucu değeri değiştirin.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Bulut hizmeti dağıtmak hazırsınız. Çözümü keşfedin, MyFixItCloudService projeye sağ tıklayın ve seçin **Yayımla**. Daha fazla bilgi için "[uygulamasını azure'a dağıtma](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", parçası 2 olduğu [Bu öğreticide](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Önceki](more-patterns-and-guidance.md)
