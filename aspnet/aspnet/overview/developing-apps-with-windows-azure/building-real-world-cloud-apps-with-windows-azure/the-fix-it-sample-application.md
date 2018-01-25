---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: "Ek: Düzeltme bu örnek uygulama (Azure ile gerçek bulut uygulamaları derleme) | Microsoft Docs"
author: MikeWasson
description: "Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: c98e79bf8e9a1fe0899ed6d952c3e411ca472f7e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Ek: Düzeltme bu örnek uygulama (Azure ile gerçek bulut uygulamaları derleme)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[Bu proje düzeltmeyi karşıdan yükleyin](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. E-kitap hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


Bu ekte yapı gerçek dünya ile bulut uygulamaları Azure e-kitap için karşıdan yükleyebileceğiniz Düzelt örnek uygulama hakkında ek bilgiler sağlayan aşağıdaki bölümleri içerir:

- [Bilinen sorunlar](#knownissues)
- [En iyi uygulamalar](#bestpractices)
- [Uygulama, yerel bilgisayarınızda Visual Studio'dan çalıştırma](#run-in-vs)
- [Windows PowerShell komut dosyalarını kullanarak Azure App Service Web Apps için temel uygulama dağıtma](#deploybase)
- [Windows PowerShell betik sorunlarını giderme](#troubleshooting)
- [Azure App Service Web Apps ve Azure bulut hizmeti için işleme sırası ile uygulama dağıtma](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Bilinen sorunlar

Düzelt uygulama desenleri bazıları bu e-kitap sunulan olabildiğince basit bir şekilde göstermek için ilk olarak geliştirilmiştir. E-kitap gerçek uygulamaları oluşturma hakkında olduğundan, ancak biz Düzelt kodu sınama işlemi ne biz yayımlanan yazılımı yapacağınız benzer ve gözden geçirmek üzere tabi. Birkaç sorun bulduk ve tüm gerçek uygulamada olduğu gibi bazıları biz sabit ve bazıları biz daha sonraki bir sürüme ertelenmiş.

Aşağıdaki listede, bir üretim uygulamasında, ancak nedenlerinden biri ya da başka bir Düzelt örnek uygulamayı ilk sürümünde adresine değil karar ele alınması gereken sorunları içerir.

### <a name="security"></a>Güvenlik

- Mevcut olmayan sahibi için bir görev atayamazsınız emin olun.
- Yalnızca emin görüntüleyin ve oluşturduğunuz ya da size atanan görevlerin değiştirin.
- HTTPS oturum açma sayfaları ve kimlik doğrulaması tanımlama bilgileri için kullanın.
- Kimlik doğrulaması tanımlama bilgileri için bir zaman sınırı belirtin.

### <a name="input-validation"></a>Giriş doğrulaması

Genel olarak, bir üretim uygulaması yaptığınız Düzelt uygulama'den daha fazla giriş doğrulaması. Örneğin, görüntü boyutu / resim yükleme sınırlandırılmalıdır için izin verilen dosya boyutu.

### <a name="administrator-functionality"></a>Yönetici işlevi

Yönetici sahipliği varolan görevleri değiştirebilmesi. Örneğin, bir görevin Oluşturucusu hiç kimse yönetimsel erişim etkinleştirilmediği sürece Görev korumak yetkilisiyle bırakarak şirket bırakabilir.

### <a name="queue-message-processing"></a>Sıra ileti işleme

Kuyruk iletisi Düzelt uygulamada işleme en düşük miktarda kod kuyruk merkezli çalışma deseni göstermek için basit olmak üzere tasarlanmıştır. Bu basit kod gerçek üretim uygulaması için yeterli olmaz.

- Kodu her kuyruk iletisi en fazla bir kez işlenir garanti etmez. Kuyruktan bir ileti aldığınızda, iletinin diğer sıra dinleyicileri görünmeyen bir zaman aşımı süresi yoktur. İleti silinmeden önce zaman aşımı süresi dolarsa, ileti yeniden görünür hale gelir. İleti işlenirken uzun süre çalışan rol örneği harcadığı varsa, bu nedenle, yinelenen bir görevi veritabanında kaynaklanan aynı ileti iki kez işlenen için teorik olarak mümkündür. Bu sorun hakkında daha fazla bilgi için bkz: [kullanarak Azure depolama sıraları](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- Sıra yoklama mantığı ileti alma toplu işleme göre daha uygun maliyetli olabilir. Çağırmanız her zaman [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), bir maliyet yoktur. Bunun yerine, çağırabilirsiniz [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (çoğul 's Not '), tek bir işlemde birden çok iletiyi alır. Azure depolama kuyrukları ilişkin işlem maliyetlerini çok düşük olduğundan maliyetleri üzerindeki etkiyi Çoğu senaryoda önemli değildir.
- Sıra ileti işleme kodu sıkı bir döngüde çok çekirdekli sanal makineleri verimli kullanmaz CPU benzeşimi neden olur. Daha iyi bir tasarım görev paralelliği paralel olarak birden fazla zaman uyumsuz görevleri çalıştırmak için kullanabilirsiniz.
- Sıra ileti işleme yalnızca ilkel özel durum işleme sahiptir. Örneğin, kod işlemiyor [zehirli ileti](https://msdn.microsoft.com/library/ms789028.aspx). (Bir özel durum iletisi işleme neden olduğunda için hata günlüğüne ve iletiyi silmek için olması veya çalışan rolü yeniden işlemek dener ve döngü süresiz olarak devam eder.)

### <a name="sql-queries-are-unbounded"></a>SQL sorguları sınırsız

Geçerli Düzelt kodu sınır dizin sayfalarını sorgularında döndürebilir kaç satırlarda yerleştirir. Yüksek hacimli görevleri bir veritabanına girilir, alınan elde edilen listeleri boyutu performans sorunlarını neden olabilir. Disk belleği uygulamak için kullanılan çözümüdür. Bir örnek için bkz: [sıralama, filtreleme ve ASP.NET MVC uygulamasındaki Entity Framework disk belleği](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Önerilen modelleri görüntüle

Düzelt uygulama FixItTask varlık sınıfı denetleyici ve görünüm arasında bilgi aktarmak için kullanır. Görünüm modelleri kullanmak iyi bir uygulamadır. Etki alanı modeli (örn., FixItTask varlık sınıfı), bir görünüm modeli veri sunumu için tasarlanmış olabilir ancak veri kalıcılığını gerekenleri tasarlanmıştır. Daha fazla bilgi için bkz: [12 ASP.NET MVC en iyi uygulamaları](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Önerilen güvenli görüntü blob'u

Düzelt uygulama mağazaları URL bulur herkes görüntüleri erişebilmesini anlamına görüntü genel olarak karşıya yüklendi. Görüntüleri ortak yerine güvenli.

### <a name="no-powershell-automation-scripts-for-queues"></a>Hiçbir PowerShell Otomasyon betikleri sıralar

Örnek PowerShell Otomasyon komutlar, yalnızca temel sürümü düzeltme tamamen Azure App Service Web Apps içinde çalışan için yazılmıştır. Biz betikleri ayarlama ve web uygulaması ve kuyruk işleme için gereken bulut hizmeti ortamını dağıtma için sağlanan henüz.

### <a name="special-handling-for-html-codes-in-user-input"></a>Kullanıcı girişi HTML kodları için özel işleme

ASP.NET, kullanıcı giriş metin kutularında betiği girerek siteler arası komut dosyası saldırıları kötü niyetli kullanıcılar deneyebilir birçok yolu otomatik olarak önler. Ve MVC `DisplayFor` görev görüntülemek için kullanılan yardımcı başlıkları ve otomatik olarak tarayıcıya gönderir HTML olarak kodlar değerlerini not. Ancak, bir üretim uygulamasında ek önlemler almak isteyebilirsiniz. Daha fazla bilgi için bkz: [ASP.NET istek doğrulama](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Önerilen uygulamalar

Kod gözden geçirme içinde bulunan ve Düzelt uygulama özgün sürümü sınama kaldıktan sonra düzeltilen bazı sorunlar aşağıda verilmiştir. Bazı bazılarını yalnızca belirli bir en iyi uygulama farkına değil özgün Kodlayıcı tarafından kaynaklanan çünkü kodu hızlı bir şekilde yazılmıştır ve yayımlanan yazılım için hedeflenen değildi. Durumunda şey Biz bu gözden geçirme sürecinden öğrenilen sorunları burada listeleme ve sınama Ayrıca web uygulamaları geliştirme kişilere faydalı olabilir.

### <a name="dispose-the-database-repository"></a>Veritabanı depo dispose

`FixItTaskRepository` Sınıfı Entity Framework dispose gerekir `DbContext` örneği. Bu uygulama tarafından yaptığımız `IDisposable` içinde `FixItTaskRepository` sınıfı:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

AutoFac otomatik olarak dispose Not `FixItTaskRepository` örnek böylece biz açıkça dispose gerek kalmaz.

Kaldırmak için başka bir seçenektir `DbContext` üye değişkeninden `FixItTaskRepository`ve bunun yerine yerel bir oluşturma `DbContext` her depo yöntemi içinde değişken içinde bir `using` deyimi. Örneğin:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Teklileri DI, bu nedenle kaydetme

Yalnızca bir örneği itibaren `PhotoService` sınıfı ve `Logger` sınıfı gerekirse, bu sınıfları olmalıdır [bağımlılık ekleme için tek örnek olarak kayıtlı](https://code.google.com/p/autofac/wiki/InstanceScope) içinde *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Güvenlik: kullanıcılar için hata ayrıntılarını gösterme

Özgün Düzelt uygulama kaydetmedi genel hata sayfasının olması ve veritabanı bağlantı hataları gibi bazı özel durumlar tarayıcıda görüntülenen tam yığın izlemesi sonuçlanabilir şekilde UI kadar tüm özel durumları Kabarcık yalnızca izin verin. Ayrıntılı hata bilgileri bazen kötü niyetli kullanıcıların saldırılarına kolaylaştırabilir. Özel durum ayrıntıları oturum ve hata ayrıntıları içermeyen kullanıcıya hata sayfasını görüntülemek için çözümüdür. Düzelt uygulama zaten günlüğü ve hata sayfasını görüntülemek için eklediğimiz `<customErrors mode=On>` Web.config dosyasında.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Varsayılan olarak bu neden *Views\Shared\Error.cshtml* hatalar için görüntülenecek. Özelleştirebileceğiniz *Error.cshtml* veya kendi hata sayfası görünümü oluşturma ve ekleme bir `defaultRedirect` özniteliği. Belirli hataları için farklı hata sayfaları de belirtebilirsiniz.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Güvenlik: oluşturucusu tarafından düzenlenmesi için bir görev yalnızca izin ver

Pano dizin sayfası yalnızca oturum açmış kullanıcı tarafından oluşturulan görevler gösterir, ancak kötü niyetli bir kullanıcı başka bir kullanıcının görev için bir Kimliğe sahip bir URL oluşturabilirsiniz. Kodda eklediğimiz *DashboardController.cs* bir 404 bu durumda döndürmek için:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Özel durumlar swallow yok

Özgün Düzelt uygulama yalnızca bir SQL sorgusu sonuçlanan bir özel durum oturum sonra null döndürdü:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Bu sorgu başarılı oldu, ancak yalnızca herhangi bir satır döndürmedi gibi kullanıcıya görünmesini sağlamak. Durum yakalama ve günlüğe kaydetme sonra yeniden oluşturması için çözüm şöyledir:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Çalışan rollerinde tüm özel durumlarını yakalama

Çalışan rolü içinde işlenmeyen özel durumlar dönüştürülmesi VM neden olur, her şeyi kaydırmak istediğiniz için bir try-catch bloğu içinde yapın ve tüm özel durumları işleme.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Varlık sınıflarda dize özellikleri için uzunluğu belirtin

Basit kod görüntülemek için Düzelt uygulamasının özgün sürümü alanları uzunluklarını FixItTask varlık belirtin alamadık ve sonuç olarak, veritabanındaki varchar(max) olarak tanımlanmıştır. Sonuç olarak, kullanıcı arabirimini giriş neredeyse herhangi bir sayıdaki kabul eder. Web sayfası ve veritabanındaki sütun boyutu hem de kullanıcı uygulamak belirten uzunlukları kümeleri sınırları giriş:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Değiştirmek için beklenen olmayan yüklerken özel üyelerin salt okunur olarak işaretle

Örneğin, `DashboardController` örneği sınıf `FixItTaskRepository` oluşturulur ve biz olarak tanımlanan şekilde değiştirmek için beklenen değil [salt okunur](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Listeyi kullanın. Liste yerine any(). Count() &gt; 0

Önem verdiğiniz tüm, ise olup listesini bir veya daha fazla öğe belirtilen ölçütlere uyan kullanın [herhangi](https://msdn.microsoft.com/library/bb534972.aspx) yöntemi, ölçüt sığdırma bir öğe bulundu hemen, ancak döndürdüğünden `Count` yöntemi her zaman sahip yinelemek her öğe. Pano *Index.cshtml* dosya ilk olarak bu kodu vardı:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Biz, bunun için değiştirilmiştir:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>MVC Yardımcıları kullanarak MVC görünümlerde URL üretmek

İçin **bir düzeltme oluşturulduğu** düğmesi giriş sayfasındaki Düzelt uygulama sabit kodlanmış bağlayıcı bir öğe:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Bu gibi görünüm/eylem bağlantıları için bunu kullanmak en iyisidir [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) HTML Yardımcısı, örneğin:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Task.Delay Thread.Sleep yerine çalışan rolünde kullanın.

Yeni Proje şablonu koyar `Thread.Sleep` örnekte ek gereksiz iş parçacığı oluşturma iş parçacığı havuzu bir çalışan rolü ancak uyku moduna iş parçacığı neden kodu neden olabilir. Kullanarak, önleyebilirsiniz [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) yerine.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Zaman uyumsuz void kaçının

Async yöntemi bir değer döndürmesi gerekmez, dönüş bir `Task` türü yerine `void`.

Bu örnek kaynaklı `FixItQueueManager` sınıfı: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Kullanmanız gereken `async void` yalnızca üst düzey olay işleyicileri için. Bir yöntem olarak tanımlarsanız `async void`, çağıran olamaz **await** yöntemi veya yöntem oluşturulur tüm özel durumları yakalamak. Daha fazla bilgi için bkz: [zaman uyumsuz programlama en iyi yöntemleri](https://msdn.microsoft.com/magazine/jj991977.aspx). 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Çalışan rolü döngüden ayırmak için bir iptal belirteci kullanın

Genellikle, **çalıştırmak** çalışan rolü yöntemini sonsuz bir döngüde içerir. Çalışan rolü durdururken [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) yöntemi çağrılır. İçinde yapılan iş iptal etmek için bu yöntemi kullanmalısınız **çalıştırmak** yöntemi ve çıkış düzgün biçimde. Aksi takdirde işlem ortasında bir işlemi sona erdirildi.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Otomatik MIME algılaması yordamı dışında iptal et

Bazı durumlarda, Internet Explorer web sunucusu tarafından belirtilen türünden farklı bir MIME türü bildirir. Örneğin, Internet Explorer HTML HTTP yanıt üstbilgisi Content-Type teslim dosyasında içerik bulursa: metin/düz, Internet Explorer içerik HTML olarak işleneceğini belirler. Ne yazık ki, bu "MIME-algılaması" de güvenilmeyen içeriği barındıran sunucular için güvenlik sorunlarına neden olabilir. Bu sorunu gidermek için Internet Explorer 8 değişiklik sayısı MIME türünü belirleme kodu yaptı'ı ve uygulama geliştiricileri için verir [MIME algılaması dışında kabul](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). Aşağıdaki kod eklendi *Web.config* dosya.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Paketleme ve küçültme etkinleştir

Visual Studio yeni bir web projesi oluştururken, paketleme ve küçültme JavaScript dosyaların değil varsayılan olarak etkindir. İçinde BundleConfig.cs bir kod satırı eklediğimiz:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Kimlik doğrulaması tanımlama bilgileri için bir sona erme zaman aşımını ayarlama

Varsayılan olarak, kimlik doğrulama tanımlama bilgisi iki hafta içinde süresi dolacak. Daha kısa bir süre daha güvenlidir. Bu ayarı değiştirebilirsiniz *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Uygulama, yerel bilgisayarınızda Visual Studio'dan çalıştırma

Düzelt uygulamayı çalıştırmak için iki yolu vardır:

- Yeni görevler doğrudan SQL veritabanına yazar temel uygulamayı çalıştırın.
- Görevleri oluşturmak üzere bir kuyruk artı bir arka uç hizmetini kullanarak uygulamayı çalıştırın. Sıra düzeni bölümde açıklanan [kuyruk merkezli çalışma deseni](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Temel uygulamayı çalıştırın

1. Yükleme [Visual Studio 2013 veya Web için Visual Studio 2013 Express](https://www.visualstudio.com/downloads).
2. Yükleme [Visual Studio 2013 için .NET için Azure SDK.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. .zip dosyasını indirin [MSDN kod Galerisi'nden](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. Dosya Gezgini'nde, .zip dosyasını sağ tıklatın ve Özellikler'i tıklatın, sonra Özellikler penceresinde Engellemeyi Kaldır'ı tıklatın.
5. Dosyanın sıkıştırmasını açın.
6. Visual Studio başlatmak için .sln dosyasını çift tıklatın.
7. Araçlar menüsünden kitaplık Paket Yöneticisi sonra Paket Yöneticisi konsolu'ı tıklatın.
8. Paket Yöneticisi Konsolu (PMC)'da, geri yükleme'yi tıklatın.
9. Visual Studio'dan çıkın.
10. Başlat [Azure storage öykünücüsü](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx).
11. Önceki adımda kapattığınız çözüm dosyasını açarak Visual Studio'yu yeniden başlatın.
12. Düzelt proje başlangıç projesi olarak ayarlandığından emin olun ve sonra projeyi çalıştırmak için CTRL + F5 tuşuna basın.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Sıranın işleme uygulamayı çalıştırın

1. İçin yönergeleri izleyin [temel uygulamayı çalıştırın](#runbase), tarayıcıyı kapatın ve Visual Studio'yu kapatın.
2. Visual Studio'yu yönetici ayrıcalıklarıyla çalıştırın. (Azure işlem öykünücüsü kullanarak ve yönetici ayrıcalıkları gerektirir.)
3. Uygulamadaki *Web.config* dosyasını *MyFixIt* proje (web projesi), değerini değiştirme `appSettings/UseQueues` "true": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Varsa [Azure storage öykünücüsü](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) hala çalışıyor, değil yeniden başlatın.
5. Düzelt web projesi ve MyFixItCloudService proje aynı anda çalıştırmak.

    Visual Studio 2013 kullanarak:

    1. Düzelt projeyi çalıştırmak için F5 tuşuna basın.
    2. İçinde **Çözüm Gezgini**MyFixItCloudService projesine sağ tıklayın ve ardından **hata ayıklama** -- **Başlat yeni örnek**.

    Web için Visual Studio 2013 Express kullanarak:

    1. Çözüm Gezgini'nde Düzelt çözüme sağ tıklayın ve seçin **özellikleri**.
    2. Seçin **birden fazla başlangıç projesi**...
    3. İçinde **eylem** MyFixIt ve MyFixItCloudService, altındaki açılır listede seçin **Başlat**.
    4. **Tamam**'ı tıklatın.
    5. Her iki projelerini çalıştırmak için F5 tuşuna basın.

    MyFixItCloudService proje çalıştırdığınızda, Visual Studio Azure işlem öykünücüsü başlatır. Güvenlik Duvarı yapılandırmanıza bağlı olarak, güvenlik duvarı aracılığıyla öykünücüsü izin gerekebilir.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Windows PowerShell komut dosyalarını kullanarak Azure App Service Web Apps için temel uygulama dağıtma

Göstermeye [her şeyi otomatikleştirmek](automate-everything.md) düzeni, Düzelt uygulama Azure ortamında ayarlama ve projeyi yeni ortama dağıtan kodlarıyla sağlanmaktadır. Aşağıdaki yönergeler, komut dosyalarının nasıl kullanılacağı açıklanmaktadır.

Azure'da sıraları kullanmadan çalıştırmak ve kuyruklarla yerel olarak çalıştırmak için değişiklikler yaptı istiyorsanız aşağıdaki yönergelere devam etmeden önce false dön UseQueues appSetting değerini ayarlayın emin olun.

Bu yönergeleri zaten indirmiş ve Düzelt çözümü yerel olarak çalıştırın ve Azure sahip hesabı veya bir Azure aboneliğine sahip varsayılır Yönetme yetkisine sahip.

1. Yükleme **Azure PowerShell** konsol. Yönergeler için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Bu özelleştirilmiş konsol Azure aboneliğiniz ile çalışmak üzere yapılandırıldı. Azure Modülü yüklü *Program Files* directory ve Azure PowerShell konsolunda her kullanımını otomatik olarak alınır.

    Windows PowerShell ISE gibi farklı bir konak programında çalışmayı tercih ederseniz kullandığınızdan emin olun [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) cmdlet'ini Azure modülünü içeri aktarın veya modül, otomatik içeri aktarma tetiklemek için Azure modülünde bir komutunu kullanın.
2. Azure PowerShell ile başlangıç **yönetici olarak çalıştır** seçeneği.
3. Çalıştırma [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) Azure PowerShell yürütme ilkesini ayarlamak için cmdlet `RemoteSigned`. Girin **Y** (Evet için) ilke değişikliğini tamamlamak için.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Bu ayarı dijital olarak imzalı olmayan yerel komut dosyaları çalıştırmanıza olanak sağlar. (De yürütme ilkesi ayarlayabilirsiniz `Unrestricted`, hangi Engellemeyi Kaldır adım daha sonra gereksinimini ortadan, ancak bu güvenlik nedenleriyle önerilmez.)
4. Çalıştırma `Add-AzureAccount` hesabınız için kimlik bilgilerine sahip PowerShell ayarlamak için cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Bu kimlik bilgileri bir süre sonra süresi dolacak ve yeniden çalıştırmak zorunda `Add-AzureAccount` cmdlet'i. Bu e-kitap yazılmış olarak 12 saat zaman kimlik bilgilerinin süresi sona ermeden önce sınırı yoktur.
5. Birden çok aboneliğiniz varsa, test ortamında oluşturmak istediğiniz aboneliği belirtmek için Select-AzureSubscription cmdlet'ini kullanın.
6. Kullanarak aynı Azure aboneliği için yönetim sertifikası içeri `Get-AzurePublishSettingsFile` ve `Import-AzurePublishSettingsFile` cmdlet'leri. Bu cmdlet'ler ilk bir sertifika dosyası indirir ve ikinci bir almak için bu dosyanın konumunu belirtin. > [!IMPORTANT]
 > İndirilen Dosya güvenli bir yerde saklayın veya onunla tamamladığınızda Azure hizmetlerinizi yönetmek için kullanılan bir sertifika içerdiğinden silin.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Sertifikanın kullanıldığı SQL veritabanı sunucusunda bir güvenlik duvarı kuralı ayarlamak için geliştirme makinenin IP adresini algılanan REST API çağrısı için.
7. Çalıştırma [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (diğer adlar `cd`, `chdir`, ve `sl`) komut dosyaları içeren dizine gidin. (Bulunan *Otomasyon* klasöründe Düzelt çözüm.) Dizin adlarının herhangi biri boşluk içeriyorsa, yolu tırnak işaretleri içine alın. Örneğin, gitmek için `c:\Sample Apps\FixIt\Automation` dizinini aşağıdaki komutu yazabilirsiniz:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Bu komut dosyalarını çalıştırmak Windows PowerShell izin vermek için kullanın [Engellemeyi Kaldır dosya](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet'i. (Internet'ten yüklenen olduğundan komut dosyalarını engellenir.)

    > [!WARNING]
    > Güvenlik - çalıştırmadan önce `Unblock-File` her betik veya yürütülebilir dosya, dosyasını Not Defteri'nde açın, komutları inceleyin ve herhangi bir kötü amaçlı kod içermeyen doğrulayın.

    Örneğin, aşağıdaki çalışmalarını komut `Unblock-File` cmdlet'ini geçerli dizindeki tüm betikler.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Tabanı (sıraları işleme) için web uygulaması oluşturmak için uygulama düzeltme, ortam oluşturma komut dosyasını çalıştırın.

    Gerekli `Name` parametresi veritabanı adını belirtir ve komut dosyası oluşturur depolama hesabı için de kullanılır. Ad azurewebsites.net etki alanı içinde genel olarak benzersiz olmalıdır. Düzelt veya Test gibi (veya hatta fixitdemo örneğinde olduğu gibi), benzersiz bir ad belirtirseniz, `New-AzureWebsite` cmdlet'i bir iç bir çakışma raporları hata ile başarısız olur. Betik adı web uygulamaları, depolama hesapları ve veritabanları için ad gereksinimlerine uymak için tamamen küçük dönüştürür.

    Gerekli `SqlDatabasePassword` parametresi, SQL veritabanı için oluşturulan yönetici hesabının parolasını belirtir. Özel XML karakterlerinden Parolada eklemeyin (&amp; &lt; &gt; ;). Bu komut dosyalarını yazılan bir sınırlama değil Azure yolu kısıtlamasıdır.

    Örneğin, "fixitdemo" adlı bir web uygulaması oluşturma ve "Passw0rd1" bir SQL Server Yönetici parolasını kullanmak istiyorsanız, aşağıdaki komutu girin:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Ad azurewebsites.net etki alanında benzersiz olmalıdır ve parola, parola karmaşıklığını SQL veritabanı gereksinimlerini karşılaması gerekir. (Bu örnek Passw0rd1 gereksinimlerini karşılamıyor.)

    Komutu ile başlayan Not ". \". Kötü amaçlı komut dosyalarını yürütülmesini önlemeye yardımcı olmak için Windows PowerShell komut dosyası çalıştırdığınızda komut dosyasının tam yolunu sağlaması gerekir. Geçerli dizin belirtmek için bir nokta kullanabilirsiniz (".\") ya da tam nitelenmiş bir yol sağlayın:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Komut dosyası hakkında daha fazla bilgi için kullanmak `Get-Help` cmdlet'i.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Kullanabileceğiniz `Detailed`, `Full`, `Parameters`, ve `Examples` döndürülen Yardım filtrelemek için Get-Help cmdlet'i parametreleri.

    Komut başarısız olursa veya "Yeni AzureWebsite: çağrı Set-AzureSubscription ve Select-AzureSubscription ilk olarak," gibi hatalar oluşturur, Azure PowerShell yapılandırması tamamlanmamış.

    Komut dosyası tamamlandıktan sonra oluşturulan, kaynakları gösterildiği gibi görmek için Azure Yönetim Portalı kullanabilirsiniz [her şeyi otomatikleştirmek](automate-everything.md) bölüm.
10. Yeni Azure ortamına Düzelt projeyi dağıtmak için kullandığınız *AzureWebsite.ps1* komut dosyası. Örneğin:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Dağıtım tamamlandığında, onunla düzeltme Azure'da çalışan tarayıcı penceresinde açar.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Windows PowerShell betik sorunlarını giderme

Bu komut dosyalarını çalışırken karşılaşılan en yaygın hataları izinleri ilişkilidir. Olduğundan emin olun `Add-AzureAccount` ve `Import-AzurePublishSettingsFile` başarılı ve aynı Azure aboneliği için kullanılır. Olsa bile `Add-AzureAccount` edildi başarılı, yeniden çalıştırmak sahip olabilir. Tarafından eklenen izinleri `Add-AzureAccount` 12 saat içinde süresi dolacak.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Nesne başvurusu bir nesnenin örneğine ayarlı değil.

"Windows PowerShell (Bu, bir null başvuru özel durumu) işlemine bir nesne bulunamıyor anlamına gelir, bir nesne örneğine nesne başvurusu ayarlanmadı" çalıştırmak gibi komut dosyası hatası döndürürse `Add-AzureAccount` cmdlet'i ve komut dosyası yeniden deneyin.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: Sunucu bir iç hatayla karşılaştı.

`New-AzureWebsite` Ad azurewebsites.net etki alanında benzersiz değilse cmdlet bir iç hata döndürür. Hatayı gidermek için ad parametre adı için farklı bir değer kullanın *yeni AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Komut dosyası yeniden başlatma

Yeniden başlatmanız gerekiyorsa *yeni AzureWebsiteEnv.ps1* komut dosyası "komut dosyası tamamlandı" iletisi yazdırılan önce başarısız olduğundan, daha önce oluşturduğunuz betiğin durduruldu kaynakları silmek isteyebilirsiniz. Örneğin, komut dosyası ContosoFixItDemo web app ve komut dosyasını aynı adla yeniden çalıştırmadan zaten oluşturduysanız, ad kullanımda olduğundan komut başarısız olur.

Hangi kaynaklara durdurulmadan önce oluşturulan komut dosyası belirlemek için aşağıdaki cmdlet'leri kullanın:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Bu cmdlet'i çalıştırmak için veritabanı sunucusu adı için kanal `Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Bu kaynakları silmek için aşağıdaki komutları kullanın. Veritabanı sunucusu silerseniz, otomatik olarak sunucuyla ilişkili veritabanlarını sildiğini unutmayın.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Azure App Service Web Apps ve Azure bulut hizmeti için işleme sırası ile uygulama dağıtma

Kuyruklar etkinleştirmek için aşağıdaki MyFixIt\Web.config dosyasında değişiklik yapılabilir. Altında `appSettings`, değerini değiştirme `UseQueues` "true": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Azure App Service'te bir web uygulaması MVC uygulamasına açıklandığı şekilde dağıtırsınız [önceki](#deploybase).

Ardından, yeni bir Azure bulut hizmeti oluşturun. Düzelt uygulamasına dahil betikleri oluşturmayın veya bunun için Azure portalını kullanmanız gerekir böylece bulut hizmeti dağıtın. Portalı'nda tıklatın **yeni** -- **işlem** – **bulut hizmeti** -- **hızlı Oluştur**ve ardından bir URL girin ve veri merkezi konum. Web uygulaması dağıtıldığı aynı veri merkezinde kullanın.

![](the-fix-it-sample-application/_static/image1.png)

Bulut hizmeti dağıtmadan önce bazı yapılandırma dosyaları güncelleştirmeniz gerekir.

MyFixIt.WorkerRoler\app.config içinde altında `connectionStrings`, değerini `appdb` gerçek bağlantı dizesiyle SQL veritabanı bağlantı dizesi. Bağlantı dizesi Portalı'ndan elde edebilirsiniz. Portalı'nda tıklatın **SQL veritabanları** - **appdb** - **ADO .net, ODBC, PHP ve JDBC için Görünüm SQL veritabanı bağlantı dizelerini**. ADO.NET bağlantı dizesini kopyalayın ve değeri app.config dosyasına yapıştırın. Değiştir "{,\_parola\_burada}" veritabanı parolanızla. (MVC uygulamasını dağıtmak için komut dosyalarını kullandığınız varsayıldığında, veritabanı parolasını belirtilen `SqlDatabasePassword` parametresi komut dosyası.)

Sonuç aşağıdaki gibi görünmelidir:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

Aynı MyFixIt.WorkerRoler\app.config dosyasındaki altında `appSettings`, Azure depolama hesabı için iki yer tutucu değerlerini değiştirin.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Erişim anahtarı Portalı'ndan elde edebilirsiniz. Bkz: [depolama hesaplarını yönetmek nasıl](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

In MyFixItCloudService\ServiceConfiguration.Cloud.cscfg, replace the same two placeholders values for the Azure storage account.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Şimdi bulut hizmeti dağıtmak hazırsınız. Çözüm keşfetmek, MyFixItCloudService projesine sağ tıklayın ve seçin **Yayımla**. Daha fazla bilgi için "[uygulamayı Azure'a dağıtmak](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", parçası 2 olduğu [Bu öğretici](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

>[!div class="step-by-step"]
[Önceki](more-patterns-and-guidance.md)
