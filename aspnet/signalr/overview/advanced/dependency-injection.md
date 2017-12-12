---
uid: signalr/overview/advanced/dependency-injection
title: "SignalR öğesindeki bağımlılık ekleme | Microsoft Docs"
author: MikeWasson
description: "Yazılım sürümleri bu konuda Visual Studio 2013 .NET 4.5 SignalR önceki sürümleri hakkında bilgi için bu konuda sürüm 2 önceki sürümlerinde kullanılan..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 3732b5d0ea6de841a6c402bfd5ef4dfb7b7a9162
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="dependency-injection-in-signalr"></a>SignalR öğesindeki bağımlılık ekleme
====================
tarafından [CAN Wasson](https://github.com/MikeWasson), [CAN Fletcher'dan](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Bu konuda kullanılan yazılım sürümleri
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR sürüm 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Bu konunun önceki sürümleri
> 
> SignalR daha önceki sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen Bu öğretici beğendiğinizi nasıl ve ne biz sayfanın sonundaki açıklamalarında artabileceğini görüşlerinizi. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları nakledebilirsiniz [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


Bağımlılık ekleme, nesneler, nesnenin bağımlılıkları, (sahte nesneler kullanılarak) sınama ya da değiştirmek için ya da çalışma zamanı davranışını değiştirmek için daha kolay hale arasındaki sabit kodlu bağımlılıklar kaldırmak için bir yoldur. Bu öğretici SignalR hub'larında bağımlılık ekleme gerçekleştirme gösterir. Ayrıca SignalR ile IOC kapsayıcıları kullanmayı gösterir. IOC kapsayıcı bağımlılık ekleme için genel bir çerçevedir.

## <a name="what-is-dependency-injection"></a>Bağımlılık ekleme nedir?

Bağımlılık ekleme ile bilginiz varsa bu bölüm atlayın.

*Bağımlılık ekleme* (dı) olan bir desen burada nesneler kendi bağımlılıkları oluşturmaktan sorumlu değildir. DI becerisiyle basit bir örnek aşağıda verilmiştir. İletilerini günlüğe kaydetmek için gereken bir nesne olduğunu varsayalım. Günlük arabirim tanımlayabilir:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Nesnenizin içinde oluşturduğunuz bir `ILogger` iletilerini günlüğe kaydetmek için:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Bu çalışır, ancak en iyi tasarım değil. Değiştirmek istiyorsanız `FileLogger` diğeriyle `ILogger` uygulaması, olacaktır değiştirmek `SomeComponent`. Diğer birçok kullanım nesneleri kabul `FileLogger`, bunların tümünün değiştirmeniz gerekir. Veya yapmaya karar verirseniz `FileLogger` bir singleton Ayrıca uygulama boyunca değişiklik sağlamanız gerekir.

Daha iyi bir yaklaşım "eklemesine" olan bir `ILogger` nesnesine — oluşturucu bağımsız değişkeni kullanarak örneğin:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Nesne seçmek için sorumlu değildir artık `ILogger` kullanmak için. Swich yapabilecekleriniz `ILogger` bağımlı nesneleri değiştirmeden uygulamaları.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Bu desen adlı [Oluşturucu ekleme](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Başka bir düzen ayarlayıcı yöntemi veya özelliği aracılığıyla bağımlılık ayarladığınız ayarlayıcı ekleme yöneliktir.

## <a name="simple-dependency-injection-in-signalr"></a>SignalR öğesindeki basit bağımlılık ekleme

Öğretici sohbet uygulamadan göz önünde bulundurun [SignalR ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr.md). Bu uygulama hub sınıfından şöyledir:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Sohbet iletileri göndermeden önce sunucuda saklamak istediğinizi varsayalım. Bu işlev soyutlar bir arabirim tanımlayın ve arabirimine eklemesine dı kullanmak `ChatHub` sınıfı.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Bir SignalR uygulaması doğrudan hub'ları oluşturmaz yalnızca sorunudur; SignalR bunları sizin için oluşturur. Varsayılan olarak, bir parametresiz oluşturucuya sahip olması için bir hub sınıf SignalR bekliyor. Ancak, kolayca hub örnekleri oluşturmak için bir işlev kaydetmek ve DI gerçekleştirmek için bu işlevi kullanın. İşlevini çağırarak kaydetmek **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

Oluşturması için gereken her SignalR bu anonim işlevi çağırma artık bir `ChatHub` örneği.

## <a name="ioc-containers"></a>IOC kapsayıcıları

Önceki kod basit durumlar için uygundur. Ancak hala bu yazmak zorunda kaldı:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

Birçok bağımlılıkları olan karmaşık bir uygulamada bu "kablolama" kod çok yazma gerekebilir. Bu kod, özellikle bağımlılıkları iç içe geçmişse korumak zor olabilir. Ayrıca, birim testi için de zordur.

Bir çözüm IOC kapsayıcı kullanmaktır. Bağımlılıklar yönetmekten sorumlu bir yazılım bileşeni bir IOC kapsayıcıdır. Kapsayıcıyla türleri kaydetmek ve nesneleri oluşturmak için kapsayıcı kullanın. Kapsayıcı otomatik olarak bağımlılık ilişkileri rakamlar. Birçok IOC kapsayıcıları nesne ömrü ve kapsam gibi denetlemenize izin.

> [!NOTE]
> "IoC" anlamına gelir "tersine çevirme denetimi için", olduğu genel bir desen olduğu bir çerçeve uygulama kodu çağırır. IOC kapsayıcı nesnelerinizi sizin için hangi "Normal akış denetimi tersine çevirir" oluşturur.


## <a name="using-ioc-containers-in-signalr"></a>SignalR öğesinde IOC kapsayıcıları kullanma

Sohbet uygulaması büyük olasılıkla bir IOC kapsayıcıdan yararlanmak basit bir işlemdir. Bunun yerine, bakalım [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) örnek.

StockTicker örnek iki ana sınıf tanımlar:

- `StockTickerHub`: İstemci bağlantıları yönetir hub sınıfı.
- `StockTicker`: Hisse senedi fiyatları tutar ve bunları düzenli aralıklarla güncelleştirir bir singleton.

`StockTickerHub`bir başvuru tutan `StockTicker` tek sırada `StockTicker` başvuru tutan **IHubConnectionContext** için `StockTickerHub`. Bu arabirim ile iletişim kurmak için kullandığı `StockTickerHub` örnekleri. (Daha fazla bilgi için bkz: [Server yayını ASP.NET SignalR ile](../getting-started/tutorial-server-broadcast-with-signalr.md).)

Bu bağımlılıklar biraz untangle için size bir IOC kapsayıcısı kullanabilirsiniz. İlk olarak, şirketinizdeki basitleştirmek `StockTickerHub` ve `StockTicker` sınıfları. Aşağıdaki kodda gerekmez, ı bölümleri açıklamalı.

Parametresiz bir kurucusu öğesinden Kaldır `StockTickerHub`. Bunun yerine, her zaman dı hub oluşturmak için kullanacağız.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

StockTicker için tek örnek kaldırın. IOC kapsayıcı StockTicker ömrünü denetlemek için daha sonra kullanacağız. Ayrıca, Oluşturucusu ortak olun.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Ardından, biz kodu için bir arabirimi oluşturarak yeniden düzenlemeniz `StockTicker`. Bu arabirim ile aynı şekilde kullanacağız `StockTickerHub` gelen `StockTicker` sınıfı.

Visual Studio bu kolay tür düzenleme yapar. StockTicker.cs dosyasını açın, sağ tıklayın `StockTicker` sınıf bildiriminin ve seçin **yeniden düzenlemeniz** ... **Ayıklama arabirimi**.

![](dependency-injection/_static/image1.png)

İçinde **arayüz** iletişim kutusunda, tıklatın **Tümünü Seç**. Diğer Varsayılanları bırakın. **Tamam**'ı tıklatın.

![](dependency-injection/_static/image2.png)

Visual Studio adlı yeni bir arabirim oluşturur `IStockTicker`ve ayrıca değiştirir `StockTicker` türetmek `IStockTicker`.

IStockTicker.cs dosyasını açın ve arabirimine değiştirme **ortak**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

İçinde `StockTickerHub` sınıfı, iki örneğini değiştirmek `StockTicker` için `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Oluşturma bir `IStockTicker` arabirimi kesinlikle gerekli değildir, ancak dı uygulamanızda bileşenleri arasındaki ilişki azaltmak için nasıl yardımcı olabileceğini gösteren istedik.

## <a name="add-the-ninject-library"></a>Ninject kitaplığı Ekle

.NET için birçok açık kaynak IOC kapsayıcı vardır. Bu öğretici için kullanmam [Ninject](http://www.ninject.org/). (Diğer popüler kitaplıkları dahil [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), ve [StructureMap ](http://docs.structuremap.net).)

Yüklemek için NuGet Paket Yöneticisi'ni kullanın [Ninject Kitaplığı](https://nuget.org/packages/Ninject/3.0.1.10). Visual Studio'da gelen **Araçları** menüsünü seçin **kitaplık Paket Yöneticisi** | **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde aşağıdaki komutu girin:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>SignalR bağımlılık çözümleyiciyi değiştirin

SignalR içinde Ninject kullanmak için türeyen bir sınıf oluşturun **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Bu sınıf geçersiz kılmaları **GetService** ve **GetServices** yöntemlerinin **DefaultDependencyResolver**. SignalR, SignalR tarafından dahili olarak kullanılan çeşitli hizmetlerin yanı sıra, hub örnekleri dahil olmak üzere çalışma zamanında çeşitli nesneleri oluşturmak için bu yöntem çağırır.

- **GetService** yöntemi, bir türü tek bir örneğini oluşturur. Ninject çekirdek 's çağırmak için bu yöntemi geçersiz kılın **TryGet** yöntemi. Bu yöntem null değeri döndürülürse, varsayılan çözümleyici geri.
- **GetServices** yöntemi, belirtilen türdeki nesneleri koleksiyonu oluşturur. Varsayılan çözümleyici alınan sonuçlarla Ninject sonuçlarından birleştirmek için bu yöntemi geçersiz kılın.

## <a name="configure-ninject-bindings"></a>Ninject bağlamaları yapılandırma

Şimdi türü bağlamaları bildirmek için Ninject kullanacağız.

Uygulamanızın haline sınıfı açın (ya da el ile paket yönergeler doğrultusunda oluşturduğunuz `readme.txt`, veya kimlik doğrulama projenize ekleme tarafından oluşturulan). İçinde `Startup.Configuration` yöntemi, Ninject çağıran Ninject kapsayıcı oluşturmak *çekirdek*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Bizim özel bağımlılık Çözümleyicisi örneği oluşturun:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

İçin bir bağlama oluşturun `IStockTicker` gibi:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Bu kod iki şey söyleyen. Uygulamanın ihtiyacı olduğunda ilk bir `IStockTicker`, çekirdek örneği oluşturmalısınız `StockTicker`. İkinci, `StockTicker` sınıfı singleton nesnesi olarak oluşturulmuş olabilir. Ninject nesnesinin bir örneğini oluşturun ve her istek için aynı örnek döndürülecektir.

İçin bir bağlama oluşturun **IHubConnectionContext** gibi:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Bu kod creatres döndüren anonim bir işlev bir **IHubConnection**. **WhenInjectedInto** yöntemi yalnızca oluşturulurken bu işlevi kullanmak için Ninject söyler `IStockTicker` örnekleri. SignalR oluşturduğu nedeni **IHubConnectionContext** dahili olarak, örnekler ve nasıl SignalR kendilerini oluşturan geçersiz kılmak istemiyorsanız. Bu işlev yalnızca uygular bizim `StockTicker` sınıfı.

Bağımlılık çözümleyici uygulamasına geçirmek **MapSignalR** bir hub yapılandırmasını ekleyerek yöntemi:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Örnek 's başlangıç sınıfı Startup.ConfigureSignalR yönteminde yeni parametresiyle güncelleştirin:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

SignalR belirtilen çözümleyici kullanacağı artık **MapSignalR**, yerine varsayılan çözümleyici.

İçin tam kod işte `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Visual Studio'da StockTicker uygulamayı çalıştırmak için F5 tuşuna basın. Tarayıcı penceresine gidin `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

Uygulamanın tam olarak aynı işlevselliği önce vardır. (Bir açıklaması için bkz [Server yayını ASP.NET SignalR ile](../getting-started/tutorial-server-broadcast-with-signalr.md).) Biz davranışı değiştirmezsiniz; Yeni kod sınamak için korumak ve gelişmesi daha kolay uyguladı.
