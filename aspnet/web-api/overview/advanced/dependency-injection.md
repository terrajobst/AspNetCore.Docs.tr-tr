---
uid: web-api/overview/advanced/dependency-injection
title: "ASP.NET Web API 2 bağımlılık ekleme | Microsoft Docs"
author: MikeWasson
description: "Bu öğreticide, ASP.NET Web API denetleyicinizde bağımlılıkları eklemesine gösterilmiştir. Eğitmen Web API 2 Unity uygulama bloğunda kullanılan yazılım sürümleri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: e3d3e7ba-87f0-4032-bdd3-31f3c1aa9d9c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: b4cf39c59ed257b0014dbdbecef3eb7bc48f410d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-aspnet-web-api-2"></a>ASP.NET Web API 2 bağımlılık ekleme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson)

[Tamamlanan projenizi indirin](http://code.msdn.microsoft.com/ASP-NET-Web-API-Tutorial-468ee148)

> Bu öğreticide, ASP.NET Web API denetleyicinizde bağımlılıkları eklemesine gösterilmiştir.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Öğreticide kullanılan yazılım sürümleri
> 
> 
> - Web API 2
> - [Unity uygulama bloğu](https://www.nuget.org/packages/Unity/)
> - Entity Framework 6 (sürüm 5 de çalışır)


## <a name="what-is-dependency-injection"></a>Bağımlılık ekleme nedir?

A *bağımlılık* , başka bir nesnenin gerektiren herhangi bir nesne. Örneğin, tanımlamak için ortak bir [depo](http://martinfowler.com/eaaCatalog/repository.html) veri erişimi işler. Şimdi ile bir örnek gösterilmektedir. Öncelikle, biz etki alanı modeli tanımlamanız:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Öğeleri Entity Framework kullanarak bir veritabanında depolayan bir basit havuz sınıf İşte.

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Şimdi şimdi GET isteklerini destekleyen bir Web API denetleyicisi tanımlamak `Product` varlıklar. (T POST ve Basitlik için diğer yöntemleri bırakmak.) İlk denemesi şöyledir:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Denetleyici sınıfı bağlıdır bildirimi `ProductRepository`, ve biz oluşturmak denetleyicisi izin vererek `ProductRepository` örneği. Ancak, çeşitli nedenlerle sabit koda bu şekilde bağımlılık hatalı bir fikir olabilir.

- Değiştirmek istiyorsanız `ProductRepository` farklı bir uygulama ile aynı zamanda denetleyici sınıfı değiştirmeniz gerekir.
- Varsa `ProductRepository` bağımlılıklara sahiptir, bu denetleyici içinde yapılandırmanız gerekir. Birden çok denetleyicileriyle büyük projesi için yapılandırma kodunuzu projeniz dağılmış haline gelir.
- Denetleyici sorgu veritabanı sabit kodlanmış olduğundan birim testi için zordur. Birim testi için currect tasarım ile mümkün olmayan bir mock veya saplama deposu kullanmanız gerekir.

Bu sorunları ele alabileceği *injecting* denetleyicisi depoya. İlk olarak, yeniden düzenlemeniz `ProductRepository` bir arabirim sınıfına:

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Ardından sağlayın `IProductRepository` Oluşturucusu parametre olarak:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Bu örnekte [Oluşturucu ekleme](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Aynı zamanda *ayarlayıcı ekleme*, bağımlılık ayarlayıcı yöntemi veya özelliği aracılığıyla ayarladığınız.

Ancak artık bir sorun olduğundan, uygulamanızın denetleyicisi doğrudan oluşturmaz olduğundan. Web API denetleyici istek yönlendirir ve Web API değil bildiğiniz bir şey hakkında oluşturur `IProductRepository`. Web API bağımlılık çözümleyiciyi nereden geldiğini budur.

## <a name="the-web-api-dependency-resolver"></a>Web API bağımlılık Çözümleyicisi

Web API tanımlar **Idependencyresolver** bağımlılıkları çözümleniyor için arabirim. Arabirim tanımı aşağıda verilmiştir:

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

**IDependencyScope** arabirimi iki yöntem vardır:

- **GetService** bir türünün bir örneğini oluşturur.
- **GetServices** belirtilen türdeki nesneleri koleksiyonu oluşturur.

**Idependencyresolver** yöntemi devralır **IDependencyScope** ve ekler **BeginScope** yöntemi. Daha sonra Bu öğreticide kapsamları hakkında konuşun.

Web API denetleyici örneği oluşturduğunda, önce çağırır **IDependencyResolver.GetService**, geçen denetleyici türü. Bu genişletilebilirlik kanca tüm bağımlılıkları çözümleniyor denetleyicisi oluşturmak için kullanabilirsiniz. Varsa **GetService** null değerini döndürür, Web API denetleyicisi sınıfı parametresiz bir kurucusu arar.

## <a name="dependency-resolution-with-the-unity-container"></a>Unity kapsayıcısı ile bağımlılık çözümlemesi

Bir tam yazabilirsiniz rağmen **Idependencyresolver** sıfırdan arabirimi uygulaması Web API ve varolan IOC kapsayıcıları arasında köprü olarak davranmak üzere tasarlanmış gerçekten.

Bağımlılıklar yönetmekten sorumlu bir yazılım bileşeni bir IOC kapsayıcıdır. Kapsayıcıyla türleri kaydetmek ve nesneleri oluşturmak için kapsayıcı kullanın. Kapsayıcı otomatik olarak bağımlılık ilişkileri rakamlar. Birçok IOC kapsayıcıları nesne ömrü ve kapsam gibi denetlemenize izin.

> [!NOTE]
> "IoC" anlamına gelir "tersine çevirme denetimi için", olduğu genel bir desen olduğu bir çerçeve uygulama kodu çağırır. IOC kapsayıcı nesnelerinizi sizin için hangi "Normal akış denetimi tersine çevirir" oluşturur.


Bu öğretici için kullanacağız [Unity](https://msdn.microsoft.com/en-us/library/ff647202.aspx) Microsoft Patterns gelen &amp; yöntemler. (Diğer popüler kitaplıkları dahil [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Ninject](http://www.ninject.org/), ve [StructureMap ](http://docs.structuremap.net/).) Unity yüklemek için NuGet Paket Yöneticisi'ni kullanabilirsiniz. Gelen **Araçları** menü Visual Studio'da seçin **kitaplık Paket Yöneticisi**seçeneğini belirleyip **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu yazın:

[!code-console[Main](dependency-injection/samples/sample7.cmd)]

Uygulaması işte **Idependencyresolver** Unity kapsayıcı sarmalar.

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

> [!NOTE]
> Varsa **GetService** yöntemi bir türü çözmek olamaz, döndürme zorunluluğu **null**. Varsa **GetServices** yöntemi bir türü çözmek olamaz, boş koleksiyon nesneyi döndürmelidir. Bilinmeyen türleri için özel durumlar durum yok.


## <a name="configuring-the-dependency-resolver"></a>Bağımlılık çözümleyiciyi yapılandırma

Bağımlılık çözümleyiciyi Ayarla **DependencyResolver** genel özelliğinin **HttpConfiguration** nesnesi.

Aşağıdaki kod yazmaçlar `IProductRepository` arabirim ile Unity ve ardından oluşturan bir `UnityResolver`.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

## <a name="dependency-scope-and-controller-lifetime"></a>Bağımlılık kapsamı ve denetleyici yaşam süresi

Denetleyicileri istek başına oluşturulur. Nesne yaşam süresi, yönetmek için **Idependencyresolver** kavramını kullanan bir *kapsam*.

Bağımlılık çözümleyiciyi bağlı **HttpConfiguration** nesne genel kapsama sahip. Web API bir denetleyici oluşturduğunda, çağıran **BeginScope**. Bu yöntem bir **IDependencyScope** temsil eden bir alt kapsamı.

Daha sonra Web API çağrılarının **GetService** denetleyicisi oluşturmak için alt kapsamında. İstek tamamlandıktan sonra Web API çağrılarının **Dispose** alt kapsamında. Kullanım **Dispose** denetleyicinin bağımlılıklarını dispose yöntemi.

Nasıl uygulayacağınıza **BeginScope** IOC kapsayıcısında bağlıdır. Unity için kapsam için bir alt kapsayıcıdaki karşılık gelir:

[!code-csharp[Main](dependency-injection/samples/sample10.cs)]

Çoğu IOC kapsayıcıları benzer eşdeğerleri.
