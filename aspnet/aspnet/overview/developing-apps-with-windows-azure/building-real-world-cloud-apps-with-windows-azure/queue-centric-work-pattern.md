---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Kuyruk merkezli çalışma deseni (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: f9916e4ecbe6234ee12bcb56519e7e2c0e490972
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840194"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Kuyruk merkezli çalışma deseni (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. E-kitabı hakkında daha fazla bilgi için bkz. [ilk bölüm](introduction.md).


Daha önce birden çok hizmeti kullanarak etkin SLA'sı uygulamanın bulunduğu bir "bileşik" SLA'da sonuçlanabilir gördüğünüz *ürün* ayrı ayrı Sla'lardan biri. Örneğin, düzeltme uygulama, Web siteleri, depolama ve SQL veritabanı kullanır. Bu hizmetlerden herhangi biri başarısız olursa, uygulama kullanıcıya bir hata döndürür.

Önbelleğe alma, salt okunur içerik için geçici hataları işlemek için iyi bir yoludur. Ancak uygulamanızın çalışması gereken ne olur? Kullanıcı yeni bir Düzelt görev gönderdiğinde, örneğin, uygulamayı yalnızca görev önbelleğine konulamaz. Uygulamanın, işleyebileceğiniz şekilde Düzelt görev bir kalıcı bir veri deposuna yazma gerekir.

Bu, kuyruk merkezli çalışma deseni burada devreye girer. Bu düzen, bir web katmanı ve arka uç hizmeti arasındaki bu sıkı bağ sağlar.

İşte deseni nasıl çalışır. Uygulamaya bir istek aldığında, bir iş öğesi bir sıraya koyar ve hemen yanıtı döndürür. Ardından ayrı arka uç işlemi, sıranın iş öğelerini çeker ve çalışır.

Kuyruk merkezli çalışma deseni için yararlıdır:

- Bu iş zaman alıcı (yüksek gecikme süresi).
- Her zaman kullanılabilir olmayabilir bir dış hizmet gerektiren iş.
- Yani çalışma kaynak kullanımı yoğun (yüksek CPU).
- Bu iş oranı (yük ani artışları tabidir) Dengeleme yararlı.

## <a name="reduced-latency"></a>Düşük gecikme süresi

Kuyruklar, zaman harcayan iş yaptığı her zaman yararlıdır. Bir görevi birkaç saniye veya daha uzun sürerse, bunun yerine son kullanıcı engelleme iş öğesini kuyruğa yerleştirin. Kullanıcı "Üzerinde çalışıyoruz" söyleyin ve arka plan görevi işlemek için bir kuyruk dinleyicisini kullanın.

Örneğin, bir çevrimiçi satış şirketi, bir şey satın aldığınızda, web sitesinin hemen siparişinizi onaylar. Ancak bu öğelerinizi zaten teslim edilen bir kamyon içinde olduğu anlamına gelmez. Bunlar bir görev bir kuyruğa koymak ve arka planda bunlar öğelerinizi dağıtımı için hazırlama kredi kontrolü yapmak ve VS.

Kısa bir gecikme süresi senaryoları için toplam uçtan uca zaman zaman uyumlu olarak görev yapan ile karşılaştırıldığında, bir kuyruk kullanma uzun olabilir. Ancak diğer avantajları bu olumsuz bile daha fazla olabilir.

## <a name="increased-reliability"></a>Daha fazla güvenilirlik

Sürümü düzeltin, biz, şimdiye arıyorsunuz içinde web ön ucu SQL veritabanı arka ucu ile sıkı şekilde bağlı. SQL veritabanı hizmeti kullanılamıyorsa, kullanıcı hata alır. Yeniden deneme işe yaramazsa (diğer bir deyişle, hatanın geçici'den fazla), gerçekleştirebilirsiniz yalnızca hata Göster ve daha sonra yeniden denemek için kullanıcıya sor şeydir.

![Diyagram gösteren web ön uç SQL veritabanı uç başarısız olduğunda başarısız oluyor](queue-centric-work-pattern/_static/image1.png)

Kuyruklar, bir kullanıcı bir Düzelt görev gönderdiğinde kullanarak uygulamayı kuyruğa bir ileti yazar. İleti yükü bir [JSON](http://json.org/) Görev temsili. İleti sıraya yazılan hemen sonra uygulama döndürür ve hemen kullanıcıya bir başarı iletisi gösterilir.

Herhangi bir arka uç Hizmetleri – gibi SQL veritabanı veya kuyruk dinleyici--çevrimdışı ise kullanıcıların Düzelt yeni görevler yine de gönderebilirsiniz. Arka uç hizmetleri yeniden kullanılabilir kadar iletileri yalnızca kuyruğa ekler. Bu noktada, arka uç Hizmetleri biriktirme listesi üzerinde Kaçırdığınız.

![Web ön uç bir SQL veritabanı hatası olduğunda çalışmaya devam etmesini gösteren diyagram](queue-centric-work-pattern/_static/image2.png)

Üstelik, artık daha fazla arka uç mantığınızı ön uç dayanıklılığı hakkında endişelenmeden ekleyebilirsiniz. Örneğin, her bir yeni düzeltme atandıktan sahibine bir e-posta veya SMS mesajı göndermek isteyebilirsiniz. E-posta veya SMS hizmeti kullanılamaz duruma gelirse, diğer her şey işleyebilir ve ardından e-posta/SMS mesajları göndermek için ayrı bir kuyruğa bir ileti yerleştirin.

Daha önce Web uygulamaları etkili SLA'mız olan &times; depolama &times; SQL veritabanı %99.7 =. (Bkz [hatalara karşı tasarım](design-to-survive-failures.md).)

Bir sıra kullanmak için uygulamayı değiştirdiğimizde, web ön uç Web uygulamaları ve depolama, yalnızca bir bileşik SLA'sı % 99,8 bağlıdır. (Blob depolama aynı SLA'ya dahil edilir şekilde sıralar Azure depolama hizmetinin bir parçası olduğunu unutmayın.)

Daha da iyi % 99,8 gerekiyorsa, iki farklı bölgelerde iki kuyruk oluşturabilirsiniz. Birincil ve diğer olarak ikincil olarak belirleyin. Birincil kuyruk kullanılabilir değilse, uygulamanızda ikincil kuyruğa yük devretme. Her iki olma kullanılamaz aynı anda fırsat son derece düşüktür.

## <a name="rate-leveling-and-independent-scaling"></a>Dengeleme oranı ve bağımsız olarak ölçeklendirme

Kuyruk adı verilen bir şeyler için kullanışlı ayrıca *oranı Dengeleme* veya *Yük Dengeleme*.

Web apps, trafik ani artışları için genellikle açıktır. Artan web trafiğini işlemek için web sunucuları otomatik olarak eklemek için otomatik ölçeklendirme kullanabilmenize karşın, otomatik ölçeklendirme ani artış yükü işlemek için yeterince hızlı tepki vermek mümkün olmayabilir. Web sunucuları, bir kuyruğa bir ileti yazarak yapmak için sahip oldukları işinin bir kısmını boşaltabilirsiniz, daha fazla trafik işlemek. Arka uç hizmeti kuyruktan iletileri okumak ve bunları işleyin. Kuyruğun derinliği büyütür veya gelen yük değiştikçe Daralt.

Büyük bir arka uç hizmetine off-loaded zaman harcayan iş ile web katmanı ve daha kolay ani artışlar yanıt verebilir. Ve herhangi bir verilen trafik miktarını daha az web sunucuları tarafından işlenebilen çünkü tasarruf edin.

Web katmanı ve arka uç hizmeti bağımsız olarak ölçeklendirebilirsiniz. Örneğin, üç web sunucuları, ancak yalnızca bir sunucu Kuyruk iletilerini işleme gerekebilir. Veya arka planda yoğun işlem gücü kullanımlı görev çalıştırma, daha fazla arka uç sunucularına ihtiyacınız olabilir.

![](queue-centric-work-pattern/_static/image3.png)

Otomatik ölçeklendirme, arka uç Hizmetleri ile yanı sıra web katmanı ile çalışır. Ölçeği artırma veya görevleri arka uç VM'lerin CPU kullanımına göre sıradan işleme VM sayısının ölçeğini. Ya da kuyrukta ne kadar öğe olmasına göre otomatik ölçeklendirme yapabilirsiniz. Örneğin sırada en fazla 10 öğe tutmak denemek için otomatik ölçeklendirme söyleyebilirsiniz. Sıranın 10'dan fazla öğe varsa, otomatik ölçeklendirme Vm'leri ekler. Otomatik ölçeklendirme, catch, ek VM'ler yıkılıp.

## <a name="adding-queues-to-the-fix-it-application"></a>Ekleme kuyruklar düzeltmesi, uygulama

Kuyruk düzeni uygulamak için biz Düzelt uygulama için iki değişiklik yapmanız gerekir.

- Bir kullanıcı yeni bir Düzelt görev gönderdiğinde, görev veritabanına yazmak yerine sıranın yerleştirin.
- Sıradaki iletileri işleyen bir arka uç hizmeti oluşturun.

Sıra için kullanacağız [Azure kuyruk depolama hizmeti](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Başka bir seçenek kullanmaktır [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Kullanılacak kuyruk hizmeti karar vermek üzere nasıl gönderin ve ileti kuyruğa almak uygulamanız gereken göz önünde bulundurun:

- Kurduğuna ilişkin üreticileri ve rakip tüketiciler varsa, Azure kuyruk depolama hizmetini kullanarak göz önünde bulundurun. "Cooperating üreticileri", birden çok işlem iletileri kuyruğa eklemekte olduğunuz anlamına gelir. Kuyruk iletilerini işlemek için birden çok işlem çeken, ancak belirli bir ileti yalnızca bir "tüketicisi. tarafından" işleme "Rakip tüketiciler" anlamına gelir İle tek bir kuyruk yapabileceğinizden daha fazla performans gerekiyorsa ek kuyruklar ve/veya ek depolama hesapları kullanın.
- Gerekirse bir [yayımlama/abone olma modeli](http://en.wikipedia.org/wiki/Publish/subscribe), Azure Service Bus kuyruklarını kullanmayı düşünün.

Düzeltme uygulama kurduğuna ilişkin üreticileri ve rakip tüketiciler modeli için en uygun.

Uygulama kullanılabilirliği başka bir husustur. Kuyruk depolama hizmetini kullanmaya SLA'mız etkisi yoktur, blob depolama için kullandığımız aynı hizmeti bir parçasıdır. Azure Service Bus, kendi SLA'sı ile farklı bir hizmettir. Hizmet veri yolu kuyrukları kullandık, biz ek bir SLA yüzdesi faktörü gerekirdi ve bileşik SLA'mız daha düşük olacaktır. Kuyruk hizmeti seçerken, seçtiğiniz uygulama kullanılabilirliği üzerindeki etkisini anladığınızdan emin olun. Daha fazla bilgi için [kaynakları](#resources) bölümü.

## <a name="creating-queue-messages"></a>Kuyruk iletileri oluşturuluyor

Düzelt göreve sıraya koymak için web ön ucu aşağıdaki adımları gerçekleştirir:

1. Oluşturma bir [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) örneği. `CloudQueueClient` Örneği, istekler kuyruk hizmeti yürütmek için kullanılır.
2. Henüz yoksa bir kuyruk oluşturun.
3. Düzeltme görev serileştirir.
4. Çağrı [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) kuyruğuna bir ileti yerleştirmek için.

Biz bu iş oluşturucuda gerçekleştirirsiniz ve `SendMessageAsync` yöntemi yeni bir `FixItQueueManager` sınıfı.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Burada kullandığımız [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Düzelt JSON biçiminde seri hale getirmek için kitaplığı. Tercih ettiğiniz hangi serileştirme yaklaşımı kullanabilirsiniz. JSON, XML daha az ayrıntılı olmanın yanı sıra okunabilir, olma avantajına sahiptir.

Üretim kalitesinde kod hata işleme mantığı ekleyin, veritabanı kullanılamaz duruma geldi, duraklatma, Kurtarma daha temiz bir şekilde işlemek, üzerinde uygulama başlatma kuyruk oluşturma ve yönetme "[zehirli" iletileri](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Zehirli ileti, herhangi bir nedenden dolayı işlenemeyen bir iletidir. Burada çalışan rolü sürekli olarak işlemeye, başarısız, yeniden deneyin, başarısız ve benzeri dener kuyrukta oturmak zehirli iletiler istemediğiniz.)

Ön uç MVC uygulamasında, size yeni bir görev oluşturan kodu güncelleştirmeniz gerekir. Görev depoya almak yerine çağrı `SendMessageAsync` yukarıda gösterilen yöntemi.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Kuyruk iletilerini işleme

Sıradaki iletileri işlemek için arka uç hizmeti oluşturacağız. Arka uç hizmeti, aşağıdaki adımları gerçekleştirir sonsuz bir döngüye çalıştırılır:

1. Sonraki iletiyi sıradan alın.
2. İletiyi Düzelt göreve seri durumdan çıkarır.
3. Düzeltme görev veritabanına yazın.

Arka uç hizmeti barındırmak için Azure bulut hizmeti içeren oluşturacağız bir *çalışan rolü*. Bir çalışan rolü, arka uç işleme yapmak için bir veya daha fazla sanal makinelerin oluşur. Bu Vm'lerde çalışan kodu iletileri kullanıma sunuldukça kuyruğundan çeker. Her ileti için şu JSON yükü seri durumdan ve düzeltin, görev varlığı örneği web katmanından daha önce kullandığımız aynı depoyu kullanarak veritabanına yazma.

Aşağıdaki adımları ekleme bir çalışan rolü projesi standart bir web projesine sahip bir çözüme gösterir. Bu adımları indirebileceğiniz Düzelt projede zaten yaptınız.

İlk bulut hizmeti projesi için Visual Studio çözümünü ekleyin. Çözüme sağ tıklayıp **Ekle**, ardından **yeni proje**. Sol bölmede genişletin **Visual C#** seçip **bulut**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

İçinde **yeni Azure bulut hizmeti** iletişim kutusunda Genişlet **Visual C#** sol bölmedeki düğüm. Seçin **çalışan rolü** ve sağ ok simgesine tıklayın.

![](queue-centric-work-pattern/_static/image6.png)

(De ekleyebilirsiniz bildirim bir *web rolü*. Düzeltme yerine bir Azure Web sitesinde çalışan aynı bulut hizmetinde ön uç çalıştıralım. Bu, ön uç ve arka uç arasındaki bağlantıları koordine kolaylaştırma içinde bazı avantajları vardır. Ancak bu tanıtımda basit tutmak için biz bir Azure App Service Web uygulamasında ön uç tutma ve yalnızca arka uç bulut hizmetinde çalışan.)

Varsayılan bir ad çalışan rolüne atanır. Adını değiştirmek için sağ taraftaki bölmede çalışan rolü fareyi üzerine gelin ve Kalem simgesine tıklayın.

![](queue-centric-work-pattern/_static/image7.png)

Tıklayın **Tamam** iletişim tamamlanması. Bu, Visual Studio çözümü iki proje ekler.

- bir Azure projesi yapılandırma bilgilerini de dahil olmak üzere bulut hizmetini tanımlar.
- Çalışan rolü tanımlayan bir çalışan rolü projesi.

![](queue-centric-work-pattern/_static/image8.png)

Daha fazla bilgi için [Visual Studio ile bir Azure projesi oluşturma.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

Çalışan rolü içinde biz iletileri çağırarak yoklama `ProcessMessageAsync` yöntemi `FixItQueueManager` daha önce gördüğümüz sınıfı.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

`ProcessMessagesAsync` Yöntemi, bir ileti bekletme olup olmadığını denetler. Varsa, iletisine çıkarır. bir `FixItTask` varlığı ve varlığın veritabanına kaydeder. Sıranın boş olduğundan kadar döngüde kalır.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Küçük bir işlem doğurur kuyruk iletileri için yoklama ücretlendirme, işlenmeyi bekleyen herhangi bir ileti olduğunda bunu çalışan rolün `RunAsync` yöntemi bekler ikinci bir yoklama önce tekrar çağırarak `Task.Delay(1000)`.

IIS sınırlı iş parçacığı havuzu yönettiğinden web projesinde, zaman uyumsuz kod ekleyerek otomatik olarak performansı artırabilir. Bir çalışan rolü projesi durumda değil. Çalışan rolünün ölçeklenebilirliği geliştirmek için çok iş parçacıklı kod yazın veya zaman uyumsuz kod uygulamak için kullanma [paralel programlama](https://msdn.microsoft.com/library/ff963553.aspx). Örnek paralel programlama uygulamaz ancak paralel programlama uygulayabilmesi kodu zaman uyumsuz nasıl yapılacağını gösterir.

## <a name="summary"></a>Özet

Bu bölümde kuyruk merkezli çalışma deseni uygulayarak uygulama yanıt hızını, güvenilirliğini ve ölçeklenebilirliğini artırmak öğrendiniz.

Bu e-kitap, kapsamdaki 13 desenlerinin son budur ancak Elbette diğer birçok desen vardır ve yardımcı olabilecek uygulamalar başarılı bulut uygulamaları oluşturun. [Son bölüm](more-patterns-and-guidance.md) 13 bu desenleri kapsamdaki henüz konular için kaynaklara bağlantılar sağlanmaktadır.

<a id="resources"></a>
## <a name="resources"></a>Kaynaklar

Kuyruklar hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

Belgeler:

- [Microsoft Azure depolama kuyrukları 1. Bölüm: Başlarken](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Roman Schacherl makalesiyle.
- [Arka plan görevleri yürütme](https://msdn.microsoft.com/library/ff803365.aspx), bölüm 5 [uygulamaları bulutta, 3 taşıma](https://msdn.microsoft.com/library/ff728592.aspx) Microsoft Patterns ve yöntemler. (Özellikle, bölüm ["Kullanarak Azure depolama kuyrukları"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Ölçeklenebilirlik ve maliyet verimliliğini azure'da kuyruk tabanlı Mesajlaşma çözümleri, en üst düzeye en iyi uygulamalar](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Valery'nin Mizonov tarafından teknik incelemesinde yanıtlanmıştır.
- [Azure kuyrukları ve Service Bus kuyrukları karşılaştırma](https://msdn.microsoft.com/magazine/jj159884.aspx). MSDN Magazine makalesini kullanılacak kuyruk hizmeti seçmenize yardımcı olabilecek ek bilgiler sağlar. Bu makalede hizmet veri yolu ACS kullanılamadığında SB Kuyruklarınızı kullanılabilir olacağı anlamına gelir kimlik doğrulaması için ACS üzerinde bağlı olduğundan bahseder. Makale yazılmış olduğundan, ancak SB kullanmanızı sağlamak üzere değiştirildi [SAS belirteçlerini](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) ACS alternatif olarak.
- [Microsoft desenler ve uygulamalar - Azure Kılavuzu](https://msdn.microsoft.com/library/dn568099.aspx). Zaman uyumsuz Mesajlaşma temel bilgileri, Kanallar ve filtreler düzeni, telafi işlemi düzeni, rakip tüketiciler düzeni, CQRS düzeni bakın.
- [CQRS yolculuğu](https://msdn.microsoft.com/library/jj554200). E-kitabı Microsoft desenler ve uygulamalar tarafından CQRS hakkında.

Video:

- [Hatasız: Ölçeklenebilir, dayanıklı bulut hizmetleri oluşturmaya](https://channel9.msdn.com/Series/FailSafe). Dokuz bölümden Ulrich Homann, Marc Mercuri ve Mark Simms'in video serisi. Üst düzey kavramlarını ve mimari ilkeleri gerçek müşterilerle Microsoft Müşteri danışma ekibi (CAT) deneyiminden çizilmiş hikayeleri çok erişilebilir ve ilgi çekici bir biçimde sunar. Bölüm 5 35:13 başlayan kuyrukları ve Azure depolama hizmeti için bir giriş için bkz.

> [!div class="step-by-step"]
> [Önceki](distributed-caching.md)
> [İleri](more-patterns-and-guidance.md)
