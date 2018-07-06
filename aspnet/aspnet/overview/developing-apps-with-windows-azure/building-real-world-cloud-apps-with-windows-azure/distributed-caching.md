---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: Dağıtılmış önbelleğe alma (Azure'la gerçek hayatta kullanılan bulut uygulamaları oluşturma) | Microsoft Docs
author: MikeWasson
description: Gerçek dünya ile bulut uygulamaları oluşturma Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. Bu, 13 desenler ve kendisi için uygulamalar açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2015
ms.topic: article
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: f92d9a00ce3cc723643f4f8077bb4308f5d8089e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365652"
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Dağıtılmış önbelleğe alma (oluşturma gerçek hayatta kullanılan bulut uygulamaları oluşturma)
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitabı indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünyaya yönelik bulut uygulamaları Azure ile** e-kitap, Scott Guthrie tarafından geliştirilen bir sunuma dayalıdır. 13 desenleri açıklar ve web uygulamaları bulut için geliştirme başarılı yardımcı olabilecek uygulamalar. E-kitabı hakkında daha fazla bilgi için bkz. [ilk bölüm](introduction.md).


Önceki bölümde geçici hata işleme sırasında aranan ve belirtilen bir devre kesici stratejisi önbelleğe alma. Bu bölümde, ne zaman, onu kullanmak için ortak desenler kullanacağınız dahil olmak üzere önbellek hakkında daha fazla arka plan sağlar ve Azure'da nasıl.

## <a name="what-is-distributed-caching"></a>Neler dağıtılmış önbelleğe alma

Bir önbellek bellekte veri depolayarak yüksek aktarım hızı, sık erişilen uygulama verilerine düşük gecikmeli erişim sağlar. Bulut uygulaması için en kullanışlı önbellek tek tek web server'ın bellek ancak diğer bulut kaynaklarını veri depolanmaz ve önbelleğe alınan verilerin tüm uygulamanın web sunucuları için kullanılabilir hale getirileceğini dağıtılmış önbellek türüdür (veya bu ar diğer bulut Vm'leri e) uygulama tarafından kullanılır.

![birden çok web sunucuları aynı önbellek sunucularına erişmediğini gösteren diyagram](distributed-caching/_static/image1.png)

Uygulama ekleyerek veya kaldırarak sunucuları ölçeklendirildiğinde ya da sunucuları yükseltme ya da hatalar nedeniyle değiştirildiğinde, önbelleğe alınan verilerin uygulama çalıştıran her sunucu için erişilebilir kalır.

Kalıcı bir veri deposuna yüksek gecikme veri erişim önleyerek, önbelleğe alma uygulama yanıt hızını artırabilirsiniz. Örneğin, verileri önbellekten ilişkisel bir veritabanındaki almaktan daha hızlıdır.

Önbelleğe alma tarafı avantaj sınırlı veri çıkışı olduğunda, düşük maliyetlerden de neden olabilir kalıcı bir veri deposuna trafik ücretleri için kalıcı veri deposu.

## <a name="when-to-use-distributed-caching"></a>Ne zaman kullanılacağı dağıtılmış önbelleğe alma

En iyi, daha fazla okuma verilerin yazılmasını daha yapın ve ne zaman önbelleğindeki verileri depolama ve alma için kullandığınız anahtar/değer kuruluş veri modelini destekler uygulama iş yükleri için önbelleğe alma çalışır. Uygulama kullanıcılarına ortak verilerin çok paylaştığınızda de daha kullanışlıdır; Örneğin, her kullanıcı genellikle bu kullanıcı için benzersiz bir veri alıyorsa önbellek gibi birçok avantaj sağlamaz. Örneği burada önbelleğe alma çok yararlı olabilir bir ürün kataloğu, verileri sık değişmeyen ve tüm müşterilerine aynı verilere baktığımızda olmasıdır.

Önbelleğe alma avantajı daha fazla uygulama ölçeklenen, aktarım hızı sınırlarını ve gecikme süresi gecikmeler kalıcı veri deposu daha genel uygulama performansını sınırlı hale geldikçe giderek ölçülebilir olur. Ancak, başka bir nedenle performans de daha önbelleğe alma uygulayabilir. Kalıcı veri deposu yanıt vermiyor veya kullanılabilir olduğunda kullanıcıya gösterilen mükemmel güncel olması gerekmez, veriler için önbellek erişim için bir devre kesici görebilir.

## <a name="popular-cache-population-strategies"></a>Popüler önbellek popülasyonu stratejileri

Verileri önbellekten oluşturabilmek, ilk var. depolamak zorunda. İhtiyacınız olan verilerin bir önbelleğe alma için birçok strateji vardır:

- İsteğe bağlı olarak / edilgen önbellek

    Önbellekten veri almak uygulama çalışır ve önbellek verilerini (bir "miss") sahip olmadığında, sonraki kullanılabilir olacaktır, böylece uygulama verileri önbellekte depolar. Uygulama aynı verileri almaya çalıştığında ne, önbellekte ("isabet") tarafından bakılıyor bulur. Veritabanı üzerinde değişmiş önbelleğe alınmış verileri getirme önlemek için veri deposuna değişiklikler yaparken önbelleği geçersiz.
- Arka plan veri gönderimi

    Arka plan Hizmetleri verileri önbelleğe düzenli bir zamanlamaya ve uygulama her zaman çeken önbellekten gönderin. Bu yaklaşım çalıştığı harika her zaman gerektirmeyen yüksek gecikme veri kaynaklarıyla en son verileri döndürür.
- Devre kesici

    Uygulamayı, normalde kalıcı veri deposuyla doğrudan iletişim kurar, ancak kalıcı veri deposu kullanılabilirlik sorunları olduğunda, uygulama önbellekten veri alır. Veri önbelleği kenara veya arka plan veri itme stratejisi kullanarak önbellekte konmuş olabilir. Bir performans geliştirme stratejisi yerine stratejisi işleme bir hata budur.

Önbellekte verileri güncel tutmak için uygulamanızı oluşturur, güncelleştirmeleri veya verileri sildiğinde ilgili önbellek girişi silebilirsiniz. Bazı durumlarda biraz güncel olmayan verileri almak uygulamanız için bunu tamamdır ise, verilerin ne kadar eski önbellek sınırlamak için yapılandırılabilir sona erme zamanı güvenebilirsiniz.

Mutlak zaman aşımı (önbellek öğesinin oluşturulduğu zaman miktarı) veya kayan zaman aşımı (bir önbellek öğeye erişildiği son kez daraltılmasından miktarı) yapılandırabilirsiniz. Mutlak zaman aşımı, verileri çok eski duruma gelmesini önlemek için Önbellek süre sonu mekanizması bağlı olduğunda kullanılır. Uygulamada Düzelt, biz eski önbellek öğeleri el ile Tahliye ve olmaadığını önbellekte en güncel verileri tutmak için kullanacağız. Seçtiğiniz süre sonu ilkesi bağımsız olarak, önbellek önbelleğin bellek sınırına ulaşıldığında en eski (en son kullanılan veya LRU) öğeleri otomatik olarak çıkarırsınız.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Düzelt uygulama için örnek edilgen önbellek kod

Aşağıdaki örnek kodda, biz önbellek ilk Düzelt görev alınırken denetleyin. Görev önbellekte bulunamazsa, biz onu döndürür; bulunamazsa, veritabanından alın ve önbellekte depolar. Değişiklikleri yaptığınız için önbelleğe alma ekleme `FindTaskByIdAsync` yöntemi vurgulanır.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Güncelleştirme ya da Düzelt görev silme (Kaldır) önbelleğe alınan görev geçersiz gerekir. Aksi takdirde, gelecekte bu görev eski verileri önbelleğe almak devam edecek okumaya çalışır.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Basit önbelleğe alma kodu göstermek için örnekler şunlardır; önbelleğe alma, indirilebilir Düzelt projede uygulanmadı.

## <a name="azure-caching-services"></a>Azure Hizmetleri için önbelleğe alma

Azure, aşağıdaki önbelleğe alma hizmetleri sunar: [Azure Redis Cache](https://msdn.microsoft.com/library/dn690523.aspx) ve [Azure yönetilen önbellek](https://msdn.microsoft.com/library/dn386094.aspx). Azure Redis cache popüler üzerinde temel [açık kaynak Redis önbelleği](http://redis.io/) ve çoğu için gereken ilk seçim önbelleğe alma senaryoları.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>Bir önbelleği sağlayıcısı kullanarak ASP.NET oturum durumu

Belirtildiği gibi [web geliştirme en iyi yöntemler Bölüm](web-development-best-practices.md), oturum durumu kullanmaktan kaçınmak için en iyi uygulamadır. Uygulamanız oturum durumu gerektiriyorsa, sonraki en iyi uygulama, ölçeği genişletme (birden çok web sunucusu örneğini) sağlamaz çünkü varsayılan bellek içi sağlayıcısı kaçınmaktır. Oturum durumu kullanma birden çok web sunucusu çalıştıran bir siteyi SQL Server ASP.NET oturum durumu sağlayıcısı sağlar, ancak bir bellek içi sağlayıcısına kıyasla yüksek gecikme maliyeti doğurur. Oturum durumu kullanma varsa en iyi çözüm bir önbelleği sağlayıcısı gibi kullanmaktır [oturum durumu sağlayıcısını Azure Cache için](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Özet

Düzeltme uygulama yanıt süresi ve ölçeklenebilirliğini geliştirmek için ve veritabanı kullanılamadığında okuma işlemleri için esnek almaya devam etmek uygulamayı etkinleştirmek için önbelleğe alma nasıl uygulayabileceğine gördünüz. İçinde [sonraki bölümde](queue-centric-work-pattern.md) ölçeklenebilirliği geliştirmek ve yazma işlemleri için hızlı yanıt verdiğini devam uygulama oluşturmak nasıl yapacağınızı göstereceğiz.

## <a name="resources"></a>Kaynaklar

Önbelleğe alma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

Belgeler

- [Azure önbellek](https://msdn.microsoft.com/library/gg278356.aspx). Azure'da önbelleğe alma resmi MSDN belgeleri.
- [Microsoft desenler ve uygulamalar - Azure Kılavuzu](https://msdn.microsoft.com/library/dn568099.aspx). Önbelleğe alma Kılavuzu ve edilgen önbellek düzeni bakın.
- [Hatasız: Dayanıklı bulut mimarileri Kılavuzu](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Andrew Townhill Marc Mercuri ve Ulrich Homann tarafından teknik incelemesinde yanıtlanmıştır. Üzerinde önbelleğe alma bölümüne bakın.
- [Azure bulut Hizmetleri'nde büyük ölçekli hizmetler tasarlamak için en iyi uygulamalar](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. Mark Simms'in ve Michael Thomassy teknik incelemesinde yanıtlanmıştır. Dağıtılmış önbelleğe alma bölümüne bakın.
- [Dağıtılmış önbelleğe alma ölçeklenebilirlik yolunda](https://msdn.microsoft.com/magazine/dd942840.aspx). Eski bir (2009) MSDN dergisi makalesi, ancak genel olarak dağıtılmış önbelleğe alma açıkça yazılmış bir giriş. daha ayrıntılı hatasız ve en iyi yöntemler teknik incelemeler önbelleğe alma bölümlerinin daha geçerlidir.

Videolar

- [Hatasız: Ölçeklenebilir, dayanıklı bulut hizmetleri oluşturmaya](https://channel9.msdn.com/Series/FailSafe). Dokuz bölümden oluşan Ulrich Homann, Marc Mercuri ve Mark Simms'in. Bulut uygulamaları oluşturmak nasıl bir 400 düzeyi görünümünü sunar. Bu seri, teorik üzerinde odaklanır ve neden neden; nasıl yapılır daha fazla ayrıntı için yapı büyük Mark Simms'in seriyi bakın. 3. Bölüm 1:24:14 başlayarak, önbelleğe alma tartışmalara bakın.
- [Yapı büyük: Azure müşterilerinin - bölüm ı dersler](https://channel9.msdn.com/Events/Build/2012/3-029). Simon Davies dağıtılmış önbelleğe alma sırasında 46:00 başlangıç açıklanır. Benzer şekilde hatasız serisi ancak daha fazla nasıl yapılır ayrıntıya gider. Önbelleğe alma hizmeti, 2013'te sunulan Azure App Service'in Web Apps kapsamaz şekilde sunu 31 Ekim 2012 verildi.

Kod örneği

- [Bulut hizmeti temel bilgileri Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Dağıtılmış önbelleğe alma işlemini uygulayan örnek uygulaması. Eşlik eden blog gönderisini inceleyin [bulut hizmeti temelleri – temel bilgileri önbelleğe alma](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Önceki](transient-fault-handling.md)
> [İleri](queue-centric-work-pattern.md)
