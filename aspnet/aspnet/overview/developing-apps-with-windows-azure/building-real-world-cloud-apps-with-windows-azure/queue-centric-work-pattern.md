---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: "Kuyruk merkezli çalışma deseni (Azure ile gerçek bulut uygulamaları derleme) | Microsoft Docs"
author: MikeWasson
description: "Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: ccfbaa26cbf610f847811e6f3c612458277046ed
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Kuyruk merkezli çalışma deseni (Azure ile gerçek bulut uygulamaları derleme)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. E-kitap hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


Birden çok hizmetlerini kullanarak uygulamanın etkili SLA olduğu bir "bileşik" SLA'da sonuçlanabilir daha önce gördüğümüz *ürün* tek tek SLA'ları. Örneğin, Web siteleri, depolama ve SQL veritabanı Düzelt uygulama kullanır. Bu hizmetlerden herhangi biri başarısız olursa, uygulama kullanıcıya bir hata döndürür.

Önbelleğe alma, salt okunur içerik için geçici hataları işlemek için iyi bir yoludur. Ancak, uygulamanızın ne çalışması gerekir? Örneğin, kullanıcı yeni bir Düzelt görev gönderdiğinde, uygulama görevi yalnızca önbelleğe yerleştirin. Uygulama işleme alınacak şekilde Düzelt görev kalıcı veri deposuna yazması gerekir.

Kuyruk merkezli çalışma deseni nereden geldiğini olmasıdır. Bu desen web katmanı ve arka uç hizmeti arasındaki bu sıkı bağ sağlar.

İşte düzeni nasıl çalışır. Uygulama bir istek aldığında, bir iş öğesi bir sıraya koyar ve hemen yanıtı döndürür. Daha sonra ayrı arka uç işlemi sıranın iş öğelerinden çeker ve çalışır.

Kuyruk merkezli çalışma deseni için yararlıdır:

- Uzun süren (yüksek gecikme süresi) çalışma.
- Her zaman kullanılamayabilir bir dış hizmet gerektirir çalışın.
- Yani iş yoğun kaynak (yüksek CPU).
- (Tabi ani yük WINS'e) Dengeleme kurundan yararlanabileceğini çalışın.

## <a name="reduced-latency"></a>Düşük gecikme süresi

Kuyruklar, uzun süren iş yapmakta olduğunuz dilediğiniz zaman yararlıdır. Bir görev birkaç saniye sürer veya artık, son kullanıcı engelleme yerine iş öğesi bir kuyruğa koymak varsa. Kullanıcı "Üzerinde çalışıyoruz" söyleyin ve sonra görev arka planda işlemek için bir kuyruk dinleyicisini kullanın.

Örneğin, bir çevrimiçi satıcısı konumundaki bir şey satın aldığınızda, web sitesini hemen siparişinizi onaylar. Ancak, öğelerinizi zaten teslim edilen bir kamyonu olduğu anlamına gelmez. Bir görev sıraya koyarlar ve arka planda öğelerinizi dağıtımı için hazırlama kredi kontrolü işleminden olduğundan ve benzeri.

Kısa bir gecikme süresi ile senaryoları için toplam uçtan uca süre görevi eşzamanlı olarak yapılması ile karşılaştırıldığında, bir sıra kullanarak daha uzun olabilir. Ancak daha sonra diğer ağır bu dezavantajı basıyor.

## <a name="increased-reliability"></a>Daha fazla güvenilirlik

Sürümünde düzeltin, biz kadarki bakarak bu, ön uç web SQL veritabanı uç ile sıkı şekilde bağlı. SQL veritabanı hizmeti kullanılamıyorsa, kullanıcı hata alır. Yeniden deneme işe yaramazsa (diğer bir deyişle, birden fazla geçici hatasıdır), bir hata gösterebilir ve daha sonra yeniden denemek için kullanıcıya sor yapabileceğiniz tek şey.

![Diyagram gösteren web SQL veritabanı arka uç başarısız olduğunda başarısız olan ön uç](queue-centric-work-pattern/_static/image1.png)

Bir kullanıcı bir Düzelt görev gönderdiğinde kuyrukları kullanma, uygulama kuyruğa bir ileti yazar. İleti yükü bir [JSON](http://json.org/) görev gösterimi. İleti sıraya yazılan hemen uygulama döndürür ve hemen kullanıcıya bir başarı iletisi gösterilir.

Arka uç hizmetlerini – gibi SQL veritabanı veya sıra dinleyicisi--çevrimdışı kalırsa, kullanıcı hala Düzelt yeni görevler gönderebilirsiniz. Arka uç hizmetlerini yeniden kullanılabilir kadar iletileri yalnızca sıraya koyar. Bu noktada, arka uç hizmetlerini biriktirme listesi üzerinde Yakala.

![Web ön uç bir SQL veritabanı hatası olduğunda çalışmaya devam eden gösteren diyagram](queue-centric-work-pattern/_static/image2.png)

Üstelik artık daha fazla arka uç mantığı ön ucu dayanıklılık hakkında endişelenmeden ekleyebilirsiniz. Örneğin, bir yeni düzeltin, kendine atanan her sahibine bir e-posta veya SMS mesajı göndermek isteyebilirsiniz. E-posta veya SMS hizmeti kullanılamaz duruma gelirse, şey işlemek ve ardından e-posta/SMS iletileri göndermek için ayrı bir sıraya bir ileti yerleştirin.

Daha önce Web uygulamaları bizim etkili SLA edildi &times; depolama &times; SQL veritabanı = %99.7. (Bkz [hatalarına karşı korur tasarım](design-to-survive-failures.md).)

Biz bir sıra kullanmak üzere uygulamayı değiştirdiğinizde, web ön uç, yalnızca Web uygulamaları ve depolama, bir bileşik %99.8 SLA bağlıdır. (Blob depolama aynı SLA dahil edilir şekilde sıralar Azure depolama hizmetinin bir parçası olduğunu unutmayın.)

%99.8 daha da iyi gerekiyorsa, iki farklı bölgelerde iki sıralar da oluşturabilirsiniz. Birincil ve diğeri de bir ikincil olarak belirleyin. Birincil kuyruk kullanılabilir değilse, uygulamanızda ikincil kuyruğuna yük devri. Her iki olma kullanılamaz aynı anda fırsat çok küçük.

## <a name="rate-leveling-and-independent-scaling"></a>Oranı Dengeleme ve bağımsız ölçeklendirme

Kuyruklar adlı bir şey için yararlı de *oranı Dengeleme* veya *Yük Dengeleme*.

Web uygulamaları genellikle trafiğinin ani WINS'e maruz kalabilir. Artan web trafiğini işlemek için web sunucuları otomatik olarak eklemek için otomatik ölçeklendirmeyi kullanabilirsiniz, ancak otomatik ölçeklendirmeyi ani artışlarını yük için yeterince hızlı tepki mümkün olmayabilir. Web sunucuları bir sıraya bir ileti yazarak yapmak için sahip oldukları iş bazıları boşaltabileceği varsa, bunlar daha fazla trafiği işleyebilir. Bir arka uç hizmeti sıradaki iletileri okumak ve bunları işlemek. Sıra Derinliği büyütür veya gelen Yük hacmi değiştikçe küçültür.

Bir arka uç hizmetine off-loaded zaman çalışmasını çoğunu ile web katmanı daha kolay trafiğinin ani ani yanıt verebilir. Ve daha az web sunucuları tarafından verilen tüm trafik miktarı işlenebilir çünkü paradan tasarruf.

Web katmanı ve arka uç hizmeti bağımsız olarak ölçeklendirebilirsiniz. Örneğin, yalnızca bir sunucu işleme sırası iletiler ancak üç web sunucuları gerekebilir. Veya arka planda işlem yoğunluklu görev çalıştırıyorsanız, daha fazla arka uç sunucularına gerekebilir.

![](queue-centric-work-pattern/_static/image3.png)

Otomatik ölçeklendirmeyi arka uç hizmetleriyle ve aynı zamanda web katmanı ile çalışır. Ölçeği artırma veya arka uç VM'ler CPU kullanımına dayalı sırasındaki görevlerin işleme VM'ler ölçeğini. Veya, bir kuyruktaki öğe sayısını göre otomatik ölçekleme yapabilirsiniz. Örneğin, en fazla 10 öğe sırada tutmaya çalışın için otomatik ölçeklendirme anlayabilirsiniz. Sıra birden fazla 10 öğe varsa, otomatik ölçeklendirme sanal makineleri ekleyin. Catch olduğunda otomatik ölçeklendirme fazladan VM'ler kesmeden.

## <a name="adding-queues-to-the-fix-it-application"></a>Ekleme kuyruklar düzeltmesi, uygulama

Sıra desen uygulamak için biz Düzelt uygulama için iki değişiklikler yapmanız gerekir.

- Bir kullanıcı yeni bir Düzelt görev gönderdiğinde, görev sırasındaki veritabanına yazmak yerine, yerleştirin.
- Sıradaki iletileri işleyen bir arka uç hizmeti oluşturun.

Sıra için kullanacağız [Azure kuyruk depolama hizmeti](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Başka bir seçenek kullanmaktır [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Hangi kuyruk hizmetini kullanacak biçimde karar vermek için nasıl uygulamanızın ileti gönderme ve sıraya alma gerektiğini göz önünde bulundurun:

- Birlikte çalışan üreticileri ve rakip tüketiciye varsa, Azure kuyruk depolama hizmeti kullanmayı düşünün. "Cooperating üreticileri" birden çok işlem iletileri kuyruğa eklemekte olduğunuz anlamına gelir. Birden çok işlem iletileri işlemek için sıra dışı çekme, ancak belirli bir ileti yalnızca "tüketici tarafından." işlenebilir "Rekabet tüketicileri" anlamına gelir İle tek bir sıraya yapabileceğinizden daha fazla verimlilik gerekiyorsa, ek kuyruklar ve/veya ek depolama hesapları kullanın.
- Gerekirse bir [Yayınla/Abone ol modeli](http://en.wikipedia.org/wiki/Publish/subscribe), Azure Service Bus kuyruklarını kullanmayı düşünün.

Düzelt uygulama için birlikte çalışan üreticileri ve rakip tüketiciye modeli uygun.

Uygulama kullanılabilirliği başka bir konudur. Kuyruk depolama hizmeti kullanmaya bizim SLA üzerinde hiçbir etkisi yoktur, blob depolama için kullanmakta olduğunuz aynı hizmeti bir parçasıdır. Azure hizmet veri yolu, kendi SLA ile ayrı bir hizmettir. Hizmet veri yolu kuyrukları kullansaydık, biz ek bir SLA yüzdesi faktörü gerekirdi ve bizim bileşik SLA daha düşük olacaktır. Sıra Hizmeti seçerken uygulama kullanılabilirliği tercih ettiğiniz etkilerini anladığınızdan emin olun. Daha fazla bilgi için bkz: [kaynakları](#resources) bölümü.

## <a name="creating-queue-messages"></a>İletileri kuyruğa oluşturma

Düzelt görev sırasına koymak için web ön uç aşağıdaki adımları gerçekleştirir:

1. Oluşturma bir [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) örneği. `CloudQueueClient` Örneği sıra hizmeti isteklerini yürütmek için kullanılır.
2. Henüz yoksa kuyruk oluşturun.
3. Düzelt görev serileştirir.
4. Çağrı [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) ileti sıranın yerleştirilecek.

Biz bu iş oluşturucuda gerçekleştirirsiniz ve `SendMessageAsync` yöntemi yeni bir `FixItQueueManager` sınıfı.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Burada kullanıyoruz [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Düzelt JSON biçiminde seri hale getirmek için kitaplık. Tercih ettiğiniz herhangi bir seri hale getirme yaklaşım kullanabilirsiniz. JSON XML daha az ayrıntılı devam ederken okunabilir, olma avantajına sahiptir.

Üretim kaliteli kod hata işleme mantığı ekleme, veritabanı kullanılamaz hale geldiyse duraklatmak, kurtarmayı daha düzgün bir şekilde işlemek, sıra üzerinde uygulama başlatma oluşturmak ve yönetmek "[zarar" iletileri](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Zararlı bir ileti herhangi bir nedenden dolayı işlenemeyen bir iletidir. Kuyrukta, burada çalışan rolü sürekli işlenecekleri, başarısız, yeniden deneyin, başarısız ve benzeri dener sit zarar iletileri istemediğiniz.)

Ön uç MVC uygulamasındaki size yeni bir görev oluşturur kodu güncelleştirmeniz gerekir. Görev depoya koyma yerine çağrı `SendMessageAsync` yukarıda gösterilen yöntemi.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>İletileri işleme

Sıradaki iletileri işlemek için bir arka uç hizmeti oluşturacağız. Arka uç hizmetine aşağıdaki adımları gerçekleştirir sonsuz bir döngüde çalışır:

1. Sonraki iletiyi sıradan alın.
2. Bir Düzelt görevi seri durumdan çıkarır.
3. Düzelt görev veritabanına yazar.

Arka uç hizmetine barındırmak için Azure bulut hizmeti içeren oluşturacağız bir *çalışan rolü*. Çalışan rolü, arka uç işleme yapmak için bir veya daha fazla VM oluşur. Bu Vm'lerde çalışan bir kod kullanılabilir olduklarında sırasından ileti çeker. Her ileti için şu JSON yükü seri durumdan ve daha önce web katmanında kullandık aynı deponun kullanarak düzeltme bu görev varlık örneği veritabanına yazma.

Aşağıdaki adımlar, standart web projesi bulunduğu bir çözümü rolü projesine bir çalışan ekleme gösterir. Bu adımları yükleyebileceğiniz Düzelt projede zaten yapılmış.

Önce bir bulut hizmeti projesini Visual Studio çözümü ekleyin. Çözüme sağ tıklayın ve seçin **Ekle**, ardından **yeni proje**. Sol bölmede **Visual C#** seçip **bulut**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

İçinde **yeni Azure bulut hizmeti** iletişim kutusunda, genişletin **Visual C#** sol bölmedeki düğüm. Seçin **çalışan rolü** ve sağ ok simgesine tıklayın.

![](queue-centric-work-pattern/_static/image6.png)

(Ayrıca ekleyebileceğiniz bildirimi bir *web rolü*. Biz düzeltin, bir Azure Web sitesini çalıştırmak yerine aynı bulut hizmetinde ön uç çalıştırabilir. Ön uç ve arka uç arasındaki bağlantıları koordine kolaylaştırma içinde bazı avantajları vardır. Ancak, bu demo basit tutmak için biz ön uç bir Azure App Service Web uygulaması tutulması ve yalnızca arka uç bulut hizmetinde çalışan.)

Varsayılan ad çalışan rolü atanır. Adını değiştirmek için fareyi sağ bölmede çalışan rolü üzerine gelin ve Kalem simgesine tıklayın.

![](queue-centric-work-pattern/_static/image7.png)

Tıklatın **Tamam** iletişim tamamlamak için. Bu iki proje için Visual Studio çözümü ekler.

- bir Azure projesi yapılandırma bilgilerini de dahil olmak üzere bulut hizmetini tanımlar.
- Çalışan rolü tanımlayan çalışan rolü projesi.

![](queue-centric-work-pattern/_static/image8.png)

Daha fazla bilgi için bkz: [Visual Studio ile bir Azure projesi oluşturma.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

Çalışan rolü biz iletileri çağırarak yoklamak `ProcessMessageAsync` yöntemi `FixItQueueManager` daha önce gördüğümüz sınıfı.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

`ProcessMessagesAsync` Yöntemi bir ileti bekleme olup olmadığını denetler. Varsa, iletisine çıkarır bir `FixItTask` varlık ve varlık veritabanına kaydeder. Sıranın boş olmadığı sürece döngüye girer.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

İletileri kuyruğa doğurur için küçük bir işlem yoklama gider, bunu işlenmek üzere bekleyen bir ileti olduğunda çalışan rolün `RunAsync` yöntemi bekler yoklama önce ikinci bir yeniden çağırarak `Task.Delay(1000)`.

IIS sınırlı iş parçacığı havuzu yönetir çünkü bir web projesi zaman uyumsuz kod ekleme otomatik olarak performansı artırabilir. Çalışan rolü projesi durumda değil. Çalışan rolü ölçeklenebilirliği artırmak için çok iş parçacıklı kod yazmak veya uygulamak için zaman uyumsuz kodu kullanın [paralel programlama](https://msdn.microsoft.com/library/ff963553.aspx). Örnek paralel programlama uygulamaz ancak paralel programlama uygulamak için kod zaman uyumsuz nasıl yapılacağını gösterir.

## <a name="summary"></a>Özet

Bu bölümde kuyruk merkezli çalışma deseni uygulayarak uygulama yanıt hızını, güvenilirliğini ve ölçeklenebilirliğini artırmak öğrendiniz.

Bu e-kitap içinde kapsanan 13 desenleri son budur ancak yardımcı olacak yöntemler başarılı bulut uygulamaları derleme ve diğer birçok desenleri Elbette vardır. [Son bölüm](more-patterns-and-guidance.md) bu 13 düzenleri kapsamdaki henüz konular için kaynaklara bağlantılar sağlanmaktadır.

<a id="resources"></a>
## <a name="resources"></a>Kaynaklar

Kuyruklar hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

Belgeler:

- [Microsoft Azure depolama kuyrukları bölüm 1: Başlarken](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Latin Schacherl makalesiyle.
- [Arka plan görevleri çalıştırma](https://msdn.microsoft.com/library/ff803365.aspx), bölüm 5 [3 bulut uygulamalarını taşıma](https://msdn.microsoft.com/library/ff728592.aspx) Microsoft Patterns ve yöntemleri. (Özellikle, bölüm ["Azure depolama kuyruklarını kullanarak"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Ölçeklenebilirlik ve sıra tabanlı Mesajlaşma çözümü Azure üzerinde maliyet verimliliğini en üst düzeye çıkarma için en iyi uygulamalar](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Teknik incelemesi Valery Mizonov tarafından.
- [Azure kuyruklar ve hizmet veri yolu kuyrukları karşılaştırma](https://msdn.microsoft.com/magazine/jj159884.aspx). MSDN dergisi makale kullanmak için hangi sıra hizmeti seçmenize yardımcı olabilecek ek bilgiler sağlar. Makale, Service Bus ACS kullanılamadığında SB sıraları kullanılamaz durumda olurdu anlamına gelir kimlik doğrulaması için üzerinde ACS bağımlı olduğunu tanımlamıştır. Makalenin yazıldığı olduğundan, ancak SB kullanmanızı sağlamak üzere değiştirilmiştir [SAS belirteci](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) alternatif ACS'nin olarak.
- [Microsoft Patterns and Practices - Azure Kılavuzu](https://msdn.microsoft.com/library/dn568099.aspx). Zaman uyumsuz Mesajlaşma primer, Kanallar ve filtreleri düzeni, karşılayan işlem düzeni, rekabet tüketicileri düzeni, CQRS düzeni bakın.
- [CQRS Journey](https://msdn.microsoft.com/library/jj554200). E-kitap Microsoft desenleri ve uygulamalar tarafından CQRS hakkında.

Video:

- [Hatasız: Ölçeklenebilir ve esnek bulut Hizmetleri derleme](https://channel9.msdn.com/Series/FailSafe). Ulrich Homann, Marc Mercuri ve işareti SIMM'lerinin video serisi dokuz bölümü. Üst düzey kavramlarını ve mimari ilkeler çok erişilebilir ve ilginç yolla, Microsoft Müşteri danışma ekibi (CAT) deneyiminde gerçek müşterilerle çizilmiş hikayeleri sunar. Kuyruklar ve Azure depolama hizmeti için bir giriş için bkz: Bölüm 5 35:13 başlatma.

>[!div class="step-by-step"]
[Önceki](distributed-caching.md)
[sonraki](more-patterns-and-guidance.md)
