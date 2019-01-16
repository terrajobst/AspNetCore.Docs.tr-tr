---
uid: web-api/overview/advanced/dependency-injection
title: ASP.NET Web API 2'de bağımlılık ekleme | Microsoft Docs
author: MikeWasson
description: Bu öğreticide, ASP.NET Web API denetleyicinizde bağımlılıkları ekleme işlemi gösterilmektedir. Eğitmen Web API 2 Unity uygulama bloğunda kullanılan yazılım sürümleri...
ms.author: riande
ms.date: 01/20/2014
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 318a2f1c587feb360212a390bb5de7bdc127513d
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341832"
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>ASP.NET Web API 2'de bağımlılık ekleme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson)

[Projeyi yükle](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Bu öğreticide, ASP.NET Web API denetleyicinizde bağımlılıkları ekleme işlemi gösterilmektedir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Bu öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 2
> - [Unity uygulama bloğu](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (sürüm 5 de kullanılabilir)


## <a name="what-is-dependency-injection"></a>Bağımlılık ekleme nedir?

A *bağımlılık* olan başka bir nesneye gerektiren herhangi bir nesne. Örneğin, tanımlamak için ortak bir [depo](http://martinfowler.com/eaaCatalog/repository.html) işleyen veri erişimi. Şimdi içeren bir örnek gösterilmektedir. İlk olarak, bir etki alanı modeli tanımlarız:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Öğeleri Entity Framework kullanarak bir veritabanında depolar. bir basit bir depo sınıfına aşağıda verilmiştir.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Artık GET isteklerini destekleyen bir Web API denetleyicisi tanımlayalım `Product` varlıklar. (Ben POST ve kolaylık sağlaması için diğer yöntemleri bırakarak.) İlk denemesi şu şekildedir:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Denetleyici sınıfı bağımlı bildirimi `ProductRepository`, ve biz denetleyici oluşturma izni vermiş olursunuz `ProductRepository` örneği. Ancak, çeşitli nedenlerle sabit koda bağımlılık bu şekilde, hatalı bir fikir olabilir.

- Değiştirmek istiyorsanız `ProductRepository` farklı bir uygulama ile aynı zamanda denetleyici sınıfı değiştirmeniz gerekir.
- Varsa `ProductRepository` bağımlılıklara sahiptir, bu denetleyici içinde yapılandırmanız gerekir. İçin büyük bir projenin birden fazla denetleyicileriyle yapılandırma kodunuzu projenizi arasında dağılmış olur.
- Denetleyici veritabanı sorgulamak için sabit kodlanmış olduğundan birim testine zordur. Birim testi için currect tasarım ile mümkün olmayan bir saplama sahte veya depo kullanmalıdır.

Bu sorunları ele *ekleme* denetleyici depoya. İlk olarak, yeniden düzenleme `ProductRepository` bir arabirim sınıfına:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Ardından sağlayın `IProductRepository` Oluşturucu parametresi olarak:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Bu örnekte [Oluşturucu ekleme](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Ayrıca *ayarlayıcı ekleme*, ayarlayıcı yöntemi veya özelliği aracılığıyla bağımlılık ayarladığınız yerdir.

Ancak artık bir sorunu, uygulamanızı denetleyici doğrudan çünkü. Web API denetleyici oluşturur isteği yönlendirir ve Web API'si değil bilmeniz herhangi bir şey hakkında `IProductRepository`. Bu, Web API'si bağımlılık çözümleyiciyi nerede devreye girer.

## <a name="the-web-api-dependency-resolver"></a>Web API bağımlılık çözümleyici

Web API tanımlar **Idependencyresolver** bağımlılıkları çözümlemek için arabirim. Arabirim tanımı aşağıda verilmiştir:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

**IDependencyScope** arabirimi iki yöntem vardır:

- **GetService** bir türün bir örneğini oluşturur.
- **GetServices** belirtilen bir türün nesnelerinin bir koleksiyonunu oluşturur.

**Idependencyresolver** yöntemi devralan **IDependencyScope** ve ekler **BeginScope** yöntemi. Kapsamlar hakkında bu öğreticinin sonraki bölümlerinde ele alacağız.

Web API denetleyici örneği oluşturduğunda, ilk çağrı **IDependencyResolver.GetService**, geçen denetleyici türü. Bu genişletilebilirlik kanca tüm bağımlılıkları çözümleniyor denetleyicisi oluşturmak için kullanabilirsiniz. Varsa **GetService** null döndürür, Web API denetleyici sınıfı üzerinde parametresiz bir oluşturucu arar.

## <a name="dependency-resolution-with-the-unity-container"></a>Unity kapsayıcısı ile bağımlılık çözümlemesi

Eksiksiz bir yazabilirsiniz rağmen **Idependencyresolver** sıfırdan arabirimi uygulaması Web API'si ve mevcut IOC kapsayıcılar arasında köprü olarak davranmak üzere gerçekten tasarlanmıştır.

IOC kapsayıcı bağımlılıkları yönetmekten sorumlu olan bir yazılım bileşenidir. Türleri ile kapsayıcı kayıt ve sonra da kapsayıcı nesneleri oluşturmak için kullanın. Kapsayıcı bağımlılık ilişkileri otomatik olarak belirler. Çok sayıda IOC kapsayıcı nesne yaşam süresi ve kapsamı gibi denetlemenize izin.

> [!NOTE]
> "IoC" anlamına gelir "tersine çevirme denetimi için", burada bir çerçeve koduna çağrı genel düzen olduğu. IOC kapsayıcı nesnelerinizi sizin için "Normal denetim akışını tersine çevirir" oluşturur.


Bu öğreticide, kullanacağız [Unity](https://msdn.microsoft.com/library/ff647202.aspx) Microsoft Patterns gelen &amp; yöntemler. (Diğer popüler kitaplıkları içerir [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), ve [StructureMap ](http://structuremap.github.io/documentation/).) Instalovat Unity için NuGet Paket Yöneticisi'ni kullanabilirsiniz. Gelen **Araçları** Visual Studio'da seçim menüsünde **NuGet Paket Yöneticisi**, ardından **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu yazın:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Uygulanışı işte **Idependencyresolver** Unity kapsayıcı sonuna geldik.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Varsa **GetService** yöntemi bir türünü çözümleyemiyor, döndürme zorunluluğu **null**. Varsa **GetServices** yöntemi, bir tür çözümleyemiyor, boş bir koleksiyon nesnesi döndürmelidir. Bilinmeyen türleri için özel durumlar yok.


## <a name="configuring-the-dependency-resolver"></a>Bağımlılık çözümleyiciyi yapılandırma

Bağımlılık çözümleyiciyi ayarlamak **DependencyResolver** genel özellik **HttpConfiguration** nesne.

Aşağıdaki kod kayıtları `IProductRepository` Unity ile arabirim ve ardından oluşturan bir `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Bağımlılık kapsamı ve denetleyici ömrü

Denetleyicileri, istek başına oluşturulur. Nesne ömrü yönetmek için **Idependencyresolver** kavramını kullanır bir *kapsam*.

Bağımlılık çözümleyiciyi bağlı **HttpConfiguration** nesnenin genel kapsam vardır. Web API denetleyici oluşturduğunda, çağrı **BeginScope**. Bu yöntem döndürür bir **IDependencyScope** temsil eden bir alt kapsamı.

Ardından Web API çağrılarının **GetService** denetleyicisi oluşturmak için alt kapsamda. İstek tamamlandıktan sonra Web API çağrılarının **Dispose** alt kapsamda. Kullanım **Dispose** denetleyicinin bağımlılıklarını atmayı yöntemi.

Nasıl uygulayacağınıza **BeginScope** IOC kapsayıcıdaki bağlıdır. Unity için kapsam için bir alt kapsayıcı karşılık gelmektedir:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

Çoğu IOC kapsayıcıları benzer eşdeğerleri vardır.
