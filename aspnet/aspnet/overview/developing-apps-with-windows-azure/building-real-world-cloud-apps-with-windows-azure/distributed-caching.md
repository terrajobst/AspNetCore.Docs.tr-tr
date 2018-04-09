---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
title: (Azure ile gerçek bulut uygulamaları derleme) önbelleğe alma dağıtılmış | Microsoft Docs
author: MikeWasson
description: Yapı gerçek dünya ile bulut uygulamaları Azure e-kitap Scott Guthrie tarafından geliştirilen bir sunu temel alır. 13 desenleri ve kendisi için yöntemler açıklanmaktadır...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2015
ms.topic: article
ms.assetid: 406518e9-3817-49ce-8b90-e82bc461e2c0
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/distributed-caching
msc.type: authoredcontent
ms.openlocfilehash: 3600200f9bb705ccf66c859547668bdf8e89d97a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="distributed-caching-building-real-world-cloud-apps-with-azure"></a>Dağıtılmış önbelleğe alma (yapı gerçek bulut uygulamaları Azure ile)
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [zel Dykstra](https://github.com/tdykstra)

[İndirme proje düzelt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) veya [E-kitap indirin](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> **Yapı gerçek dünya bulut uygulamalarını Azure ile** e-kitap Scott Guthrie tarafından geliştirilen bir sunu dayanır. 13 desenleri açıklar ve yardımcı olacak yöntemler bulutu için web uygulamaları geliştirme başarılı. E-kitap hakkında daha fazla bilgi için bkz: [ilk bölüm](introduction.md).


Önceki bölümde geçici hata işleme sırasında arama ve devre kesici stratejisi önbelleğe alma belirtiliyor. Bu bölümde, bunu kullanmak için ortak desenler kullanmak ne zaman dahil olmak üzere önbelleğe alma hakkında daha fazla arka plan verir ve Azure'da uygulama.

## <a name="what-is-distributed-caching"></a>Ne önbelleğe alma dağıtılır

Bir önbellek bellekte veri depolayarak yüksek verimlilik, sık erişilen uygulama verileri için düşük gecikmeli erişim sağlar. Bir bulut uygulaması için en kullanışlı türü önbellek verilerini tek tek web sunucunuzun bellek ancak diğer bulut kaynakları depolanmaz ve önbelleğe alınan veriler tüm bir uygulamanın web sunucuları için kullanılabilir hale getirileceğini anlamına gelir dağıtılmış önbellek (veya bu ar diğer bulut VM'ler e) uygulama tarafından kullanılır.

![birden çok web sunucuları aynı önbellek sunucularına erişilirken gösteren diyagram](distributed-caching/_static/image1.png)

Uygulama ekleyerek veya kaldırarak sunucuları ölçeklendirir veya sunucuları yükseltme veya hataları nedeniyle değiştirilir önbelleğe alınmış verileri uygulamayı çalıştıran her sunucuya erişilebilir kalır.

Kalıcı veri deposunun yüksek gecikme veri erişimi kaçınarak önbelleğe alma önemli ölçüde uygulama yanıt hızını artırabilir. Örneğin, önbellekten veri almaya ilişkisel bir veritabanından alma daha hızlıdır.

Önbelleğe alma tarafı avantajı azaltılmış veri çıkışı olduğunda alt maliyetlerini sonuçlanabilir kalıcı veri depolama alanı trafiğini ücretlendirilen için kalıcı veri deposu.

## <a name="when-to-use-distributed-caching"></a>Ne zaman kullanılacağı dağıtılmış önbelleğe alma

Daha fazla okuma yazma verilerin daha yapın ve ne zaman veri modelini depolamak ve veri önbelleğindeki almak için kullandığınız anahtar/değer kuruluşunuzun desteklediği uygulama iş yükleri için en iyi önbelleğe alma çalışır. Uygulama kullanıcıları çok ortak veri paylaştığınızda de daha kullanışlıdır; Her kullanıcı genellikle o kullanıcı için benzersiz verileri alır, örneğin, önbellek gibi birçok avantaj sağlar değil. Burada önbelleğe alma çok yararlı olabilir bir ürün kataloğu, veri sık değişmeyen ve tüm müşterilerin aynı veri arıyorsanız çünkü örneğidir.

Önbelleğe alma avantajı daha fazla uygulama ölçekler, işleme sınırları ve gecikme gecikmeler kalıcı veri deposunun daha genel uygulama performansına sınırının hale geldikçe giderek ölçülebilir olur. Ancak, diğer nedenlerle da performans daha önbelleğe alma uygulayabilir. Kalıcı veri deposu yanıt vermeyen ya da kullanılabilir olduğunda, kullanıcıya gösterilen mükemmel güncel olması gerekmez veriler için bir devre Kesici olarak önbellek erişim hizmet verebilir.

## <a name="popular-cache-population-strategies"></a>Popüler önbellek popülasyonu stratejileri

Verileri önbellekten oluşturabilmek, önce var. depolamak sahip. Gereksinim duyduğunuz verileri bir önbelleğe almak için birkaç strateji vardır:

- İsteğe bağlı / önbelleğe ayırın

    Uygulama verileri önbellekten dener ve önbellek verilerini (bir "isabetsiz") sahip olmadığında sonraki kullanılabilir olmayacaktır uygulama verileri önbellekte depolar. Uygulama aynı veri almaya çalıştığında ne bunu ("isabet") önbellekte aradığı bulur. Veritabanında değiştirildi önbelleğe alınmış veri getirme önlemek için önbelleğe veri deposuna değişiklik yaparken geçersiz.
- Arka plan veri gönderimi

    Arka plan Hizmetleri veri önbelleğine düzenli ve uygulama her zaman çeken önbellekten iletin. Bu yaklaşım works mükemmel her zaman gerektirmeyen yüksek gecikme veri kaynakları ile en son verileri döndürür.
- Devre kesici

    Uygulamayı, normalde doğrudan kalıcı veri deposuyla iletişim kurar, ancak kalıcı veri deposu kullanılabilirlik sorunları olduğunda, uygulama önbellekten veri alır. Veri önbelleği kenara veya arka plan veri gönderme stratejisini kullanarak önbelleğinde put. Bir performans geliştirerek stratejisi yerine stratejisi işleme bir hata budur.

Veri önbellekte güncel tutmak için uygulamanızın, güncelleştirmeler, oluşturduğunda veya verileri sildiğinde ilgili önbellek girişlerinin silebilirsiniz. Bazen biraz güncel verileri almak, uygulamanız için sorun olması durumunda, ne kadar eski önbellek veriler olabilir sınırlamak için yapılandırılabilir sona erme zamanı güvenebilirsiniz.

Mutlak zaman aşımı (süreyi önbellek öğesi oluşturulmasından bu yana) veya kayan bitiş tarihi (bir önbellek öğesi erişilen en son ne zaman bu yana geçen süre miktarı) yapılandırabilirsiniz. Mutlak zaman aşımı, verileri çok eski haline gelmesini engellemek için Önbellek süre sonu mekanizmasını bağlı olarak kullanılır. Uygulamasındaki Düzelt biz eski önbellek öğeleri el ile Tahliye ve kayan zaman aşımı önbelleğindeki en güncel verileri tutmak için kullanacağız. Seçtiğiniz süre sonu ilkesi bağımsız olarak, önbellek Önbelleği'nin bellek sınırına ulaşıldığında en eski (en az kısa süre önce kullanılan veya LRU) öğelerini otomatik olarak çıkarırsınız.

## <a name="sample-cache-aside-code-for-fix-it-app"></a>Örnek edilgen önbellek kod Düzelt uygulaması

Aşağıdaki örnek kodda biz önbellek ilk Düzelt görev alınırken denetleyin. Görev önbelleğinde bulunan olursa biz döndürür; bulunamazsa, veritabanından almak ve önbelleğine depolayabilirsiniz. Değişiklikler, yapmak için önbelleğe alma ekleme `FindTaskByIdAsync` yöntemi vurgulanır.

[!code-csharp[Main](distributed-caching/samples/sample1.cs?highlight=5,9-11,13-15,19)]

Güncelleştirme veya silme bir Düzelt görevi (kaldırma) önbelleğe alınan görev geçersiz kılmak vardır. Aksi takdirde, gelecekte bu görev eski verileri önbellekten almaya devam edersiniz okumaya çalışır.

[!code-csharp[Main](distributed-caching/samples/sample2.cs?highlight=7)]

Basit önbelleğe alma kodu göstermek için örnekler şunlardır; önbelleğe alma, indirilebilir Düzelt projede uygulanmadı.

## <a name="azure-caching-services"></a>Azure Hizmetleri önbelleğe alma

Azure aşağıdaki önbelleğe alma hizmetleri sunar: [Azure Redis önbelleği](https://msdn.microsoft.com/library/dn690523.aspx) ve [Azure yönetilen önbellek](https://msdn.microsoft.com/library/dn386094.aspx). Azure Redis önbelleği popüler üzerinde temel [açık kaynaklıdır Redis önbelleği](http://redis.io/) ve ilk seçenek çoğu için önbelleğe alma senaryoları.

<a id="sessionstate"></a>
## <a name="aspnet-session-state-using-a-cache-provider"></a>Bir önbellek sağlayıcısı kullanarak ASP.NET oturum durumu

Bölümünde belirtildiği gibi [web geliştirme en iyi yöntemler Bölüm](web-development-best-practices.md), oturum durumu kullanmaktan kaçınmak için en iyi uygulamadır. Uygulamanız oturum durumu gerektiriyorsa, sonraki en iyi uygulama olarak (birden çok web sunucusu örneğini) ölçeklendirme sağlamaz çünkü varsayılan bellek içi sağlayıcısı kaçınmaktır. Oturum durumunu kullanmak için birden çok web sunucusu çalıştıran bir siteye ASP.NET SQL Server oturum durumu sağlayıcısı sağlar, ancak bir bellek içi sağlayıcısına göre bir gecikme süresi yüksek maliyet oluşturur. Oturum durumu kullanmanız gerekiyorsa en iyi çözüm önbelleği sağlayıcısı gibi kullanmaktır [Azure önbelleği için oturum durumu sağlayıcısı](https://msdn.microsoft.com/library/windowsazure/gg185668.aspx).

## <a name="summary"></a>Özet

Düzelt uygulama yanıt süresini ve ölçeklenebilirliğini geliştirmek için ve veritabanı kullanılamadığında okuma işlemleri için esnek olmaya devam sağlamak için önbelleğe alma nasıl uygulamak gördünüz. İçinde [sonraki bölümde](queue-centric-work-pattern.md) daha fazla ölçeklenebilirlik geliştirmek ve yazma işlemleri için esnek olmaya devam uygulama yapmak nasıl göstereceğiz.

## <a name="resources"></a>Kaynaklar

Önbelleğe alma hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın.

Belgeler

- [Azure önbelleği](https://msdn.microsoft.com/library/gg278356.aspx). Azure'da önbelleğe alma üzerinde resmi MSDN belge.
- [Microsoft Patterns and Practices - Azure Kılavuzu](https://msdn.microsoft.com/library/dn568099.aspx). Önbelleğe alma yönerge ve edilgen önbellek düzeni bakın.
- [Hatasız: Dayanıklı bulut mimarileri Kılavuzu](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Teknik incelemesi Marc Mercuri, Ulrich Homann ve Barış Townhill tarafından. Önbelleğe alma üzerinde bölümüne bakın.
- [En iyi uygulamalar için Azure bulut hizmetleri üzerinde büyük ölçekli hizmetler tasarımını](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). W. Teknik incelemesi işareti SIMM'lerinin ve Michael Thomassy tarafından. Dağıtılmış önbelleğe alma bölümüne bakın.
- [Ölçeklenebilirlik yolunda önbelleğe alma dağıtılmış](https://msdn.microsoft.com/magazine/dd942840.aspx). Eski (2009) MSDN dergisi makale, ancak genel olarak dağıtılmış önbelleğe alma açıkça belirtilmiş bir giriş; önbelleğe alma hatasız ve en iyi yöntemler teknik incelemeler kısımları'den daha kapsamlı girmeyeceğini.

Videolar

- [Hatasız: Ölçeklenebilir ve esnek bulut Hizmetleri derleme](https://channel9.msdn.com/Series/FailSafe). Dokuz parçalı seriyi Ulrich Homann, Marc Mercuri ve işareti SIMM'lerinin. Bulut uygulamalarını mimari nasıl 400 düzeyi görünümünü sunar. Bu seri teorisine odaklanır ve neden nedenleri; nasıl yapılır daha fazla ayrıntı için yapı büyük seri işareti SIMM'lerinin tarafından bakın. Bölüm 3 1:24:14 başlatırken önbelleğe alma tartışma bakın.
- [Yapı büyük: Azure müşterilerden - bölümü ı dersleri öğrenilen](https://channel9.msdn.com/Events/Build/2012/3-029). Dağıtılmış önbellek 46:00 başlangıç Simon Davies açıklanır. Hatasız serisi ancak yapılır Ayrıntılar gerçekleştirip gerçekleştirmeyeceğini benzer. Web uygulamaları Azure App Service'te 2013'te tanıtılan önbelleğe alma hizmeti kapsamaz şekilde sunu 31 Ekim 2012 verildi.

kod örneği

- [Bulut hizmeti temel bilgileri Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Dağıtılmış önbelleğe alma uygulayan örnek uygulama. Eşlik eden blog gönderisine bakın [bulut hizmeti temel bilgileri – temel bilgileri önbelleğe alma](https://blogs.msdn.com/b/windowsazure/archive/2013/10/03/cloud-service-fundamentals-caching-basics.aspx).

> [!div class="step-by-step"]
> [Önceki](transient-fault-handling.md)
> [sonraki](queue-centric-work-pattern.md)
