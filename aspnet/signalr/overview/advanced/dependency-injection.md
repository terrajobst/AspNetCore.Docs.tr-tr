---
uid: signalr/overview/advanced/dependency-injection
title: Signalr'da bağımlılık ekleme | Microsoft Docs
author: MikeWasson
description: Yazılım sürümleri, sürüm 2 önceki sürümleri bu konunun önceki sürümleri hakkında bilgi için bu konu Visual Studio 2013 .NET 4.5 SignalR kullanılan...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a14121ae-02cf-4024-8af0-9dd0dc810690
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/advanced/dependency-injection
msc.type: authoredcontent
ms.openlocfilehash: 0b4276d25c999c2a78864a856f7f3de233c9ac87
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397766"
---
<a name="dependency-injection-in-signalr"></a>Signalr'da bağımlılık ekleme
====================
tarafından [Mike Wasson](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Bu konu başlığında kullanılan yazılım sürümleri
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
> SignalR eski sürümleri hakkında daha fazla bilgi için bkz: [SignalR eski sürümleri](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Sorularınız ve yorumlarınız
> 
> Lütfen bu öğreticide sevmediğinizi nasıl ve ne sayfanın alt kısmındaki açıklamalarda geliştirebileceğimiz hakkında geri bildirim bırakın. Öğretici için doğrudan ilgili olmayan sorularınız varsa, bunları gönderebilir [ASP.NET SignalR Forumu](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) veya [StackOverflow.com](http://stackoverflow.com/).


Bağımlılık ekleme (sahte nesneler kullanılarak) test etmek için ya da bir nesnenin bağımlılıkları değiştirin veya çalışma zamanı davranışını değiştirmek için daha kolay hale getirme nesneleri arasındaki sabit kodlu bağımlılıkları kaldırmak için bir yoldur. Bu öğreticide SignalR hub'larında bağımlılık ekleme yapmak nasıl gösterir. Ayrıca SignalR ile IOC kapsayıcıları kullanmayı gösterir. IOC kapsayıcı için bağımlılık ekleme genel bir çerçevedir.

## <a name="what-is-dependency-injection"></a>Bağımlılık ekleme nedir?

Zaten bağımlılık ekleme hakkında bilginiz varsa, bu bölümü atlayın.

*Bağımlılık ekleme* (dı) olan bir desen burada nesneler kendi bağımlılıklar oluşturmaktan sorumlu değildir. DI becerisiyle basit bir örnek aşağıda verilmiştir. İletilerini günlüğe kaydetmek için gereken bir nesne olduğunu varsayalım. Günlüğe kaydetme arabirimi tanımlayabilir:

[!code-csharp[Main](dependency-injection/samples/sample1.cs)]

Nesnesi içinde oluşturduğunuz bir `ILogger` iletilerini günlüğe kaydetmek için:

[!code-csharp[Main](dependency-injection/samples/sample2.cs)]

Bu çalışır, ancak en iyi tasarım değil. Değiştirmek istiyorsanız `FileLogger` diğeriyle `ILogger` uygulama, değiştirme gerekir `SomeComponent`. Diğer birçok kullanım nesnelerini kabul ederek şehirler `FileLogger`, bunların tümünün değiştirmeniz gerekir. Veya yapmaya karar verirseniz `FileLogger` tek Ayrıca uygulama boyunca değişiklik yapmanız gerekir.

Daha iyi bir yaklaşım "" bulunmaktır bir `ILogger` nesnesine — Örneğin, bir oluşturucu bağımsız değişkenini kullanarak tarafından:

[!code-csharp[Main](dependency-injection/samples/sample3.cs)]

Nesne seçmek için sorumlu değildir. Şimdi `ILogger` kullanılacak. Swich yapabilecekleriniz `ILogger` bağımlı nesneleri değiştirmeden uygulamaları.

[!code-csharp[Main](dependency-injection/samples/sample4.cs)]

Bu düzeni olarak adlandırılır [Oluşturucu ekleme](http://www.martinfowler.com/articles/injection.html#FormsOfDependencyInjection). Başka bir ayarlayıcı eklemesi, ayarlayıcı yöntemi veya özelliği aracılığıyla bağımlılık ayarladığınız modelidir.

## <a name="simple-dependency-injection-in-signalr"></a>Signalr'da basit bağımlılık ekleme

Sohbet uygulaması Öğreticisi göz önünde bulundurun [SignalR ile çalışmaya başlama](../getting-started/tutorial-getting-started-with-signalr.md). Bu uygulama hub sınıfı şöyledir:

[!code-csharp[Main](dependency-injection/samples/sample5.cs)]

Sohbet iletileri göndermeden önce sunucuda depolamak istediğinizi varsayalım. Bu işlev soyutlayan bir arabirim tanımlayın ve arabirimine eklemesine DI kullanmak `ChatHub` sınıfı.

[!code-csharp[Main](dependency-injection/samples/sample6.cs)]

Bir SignalR uygulama doğrudan hub'ları oluşturmaz yalnızca sorunudur; SignalR bunları sizin için oluşturur. Varsayılan olarak, bir hub sınıfı, parametresiz bir oluşturucu sağlamak için SignalR bekliyor. Ancak, kolayca hub örnekleri oluşturmak için bir işlev kaydetmek ve DI gerçekleştirmek için bu işlevi kullanın. İşlevini çağırarak kaydetmek **GlobalHost.DependencyResolver.Register**.

[!code-csharp[Main](dependency-injection/samples/sample7.cs)]

SignalR oluşturmanız gerektiğinde bu anonim işlev çağıracağı artık bir `ChatHub` örneği.

## <a name="ioc-containers"></a>IOC kapsayıcıları

Önceki kod, basit durumlar için uygundur. Ancak yine de bu yazmanız gerekirdi:

[!code-csharp[Main](dependency-injection/samples/sample8.cs)]

Birçok bağımlılıkları olan karmaşık bir uygulamada bu "bağlantı" kod yazmanız gerekebilir. Bu kod, özellikle bağımlılıkları iç içe geçmişse sağlamak zor olabilir. Ayrıca, birim testi de zordur.

Tek bir çözüm IOC kapsayıcı kullanmaktır. IOC kapsayıcı bağımlılıkları yönetmekten sorumlu olan bir yazılım bileşenidir. Türleri ile kapsayıcı kayıt ve sonra da kapsayıcı nesneleri oluşturmak için kullanın. Kapsayıcı bağımlılık ilişkileri otomatik olarak belirler. Çok sayıda IOC kapsayıcı nesne yaşam süresi ve kapsamı gibi denetlemenize izin.

> [!NOTE]
> "IoC" anlamına gelir "tersine çevirme denetimi için", burada bir çerçeve koduna çağrı genel düzen olduğu. IOC kapsayıcı nesnelerinizi sizin için "Normal denetim akışını tersine çevirir" oluşturur.


## <a name="using-ioc-containers-in-signalr"></a>SignalR öğesinde IOC kapsayıcıları kullanma

Sohbet uygulaması büyük olasılıkla bir IOC kapsayıcısından yararlanmak basit bir işlemdir. Bunun yerine, göz atalım [StockTicker](http://nuget.org/packages/microsoft.aspnet.signalr.sample) örnek.

StockTicker örnek iki ana sınıf tanımlar:

- `StockTickerHub`: İstemci bağlantıları yönetir hub sınıfı.
- `StockTicker`: Hisse senedi fiyatlarına tutar ve bunları düzenli aralıklarla güncelleştiren bir tekli.

`StockTickerHub` bir başvuru tutan `StockTicker` tekil, ancak `StockTicker` bir başvuru tutan **IHubConnectionContext** için `StockTickerHub`. Bu arabirim ile iletişim kurmak için kullandığı `StockTickerHub` örnekleri. (Daha fazla bilgi için [ASP.NET SignalR ile sunucu yayını](../getting-started/tutorial-server-broadcast-with-signalr.md).)

Bu bağımlılıklar biraz ayrıştırmaya IOC kapsayıcı kullanabiliriz. İlk olarak, şimdi basitleştirmek `StockTickerHub` ve `StockTicker` sınıfları. Aşağıdaki kodda, ihtiyaç duymayacağımız olduğunu miyim bölümlerini açıklamalı.

Parametresiz oluşturucusu öğesinden kaldırmak `StockTickerHub`. Bunun yerine, her zaman DI hub'ı oluşturmak için kullanacağız.

[!code-csharp[Main](dependency-injection/samples/sample9.cs)]

StockTicker için tekil örneğini kaldırın. Daha sonra IOC StockTicker ömrünü denetlemek için kullanacağız. Ayrıca, ortak oluşturucu olun.

[!code-csharp[Main](dependency-injection/samples/sample10.cs?highlight=7)]

Ardından, biz kodu için bir arabirim oluşturarak yeniden düzenleyebilirsiniz `StockTicker`. Bu arabirim ayrıştırmak için kullanacağız `StockTickerHub` gelen `StockTicker` sınıfı.

Visual Studio bu kolay tür yeniden düzenleme sağlar. StockTicker.cs dosyasını açın, sağ `StockTicker` sınıf bildiriminin ve seçin **yeniden düzenleme** ... **Ayıklama arabirimi**.

![](dependency-injection/_static/image1.png)

İçinde **Arabirimi Ayıkla** iletişim kutusunda, tıklayın **Tümünü Seç**. Diğer varsayılan değerleri bırakın. **Tamam**'ı tıklatın.

![](dependency-injection/_static/image2.png)

Visual Studio adlı yeni bir arabirim oluşturur `IStockTicker`ve ayrıca değiştirir `StockTicker` türetmek için `IStockTicker`.

IStockTicker.cs dosyasını açın ve değiştirmek için arabirimi **genel**.

[!code-csharp[Main](dependency-injection/samples/sample11.cs?highlight=1)]

İçinde `StockTickerHub` sınıfı, iki örneğinden birini değiştirmek `StockTicker` için `IStockTicker`:

[!code-csharp[Main](dependency-injection/samples/sample12.cs?highlight=4,6)]

Oluşturma bir `IStockTicker` arabirimi kesinlikle gerekli değildir, ancak DI uygulamanızın bileşenleri arasında eşleştirme yapmaktan azaltmak için nasıl yardımcı olabileceğini gösteren istedi.

## <a name="add-the-ninject-library"></a>Ninject kitaplığı Ekle

.NET için çok sayıda açık kaynaklı IOC kapsayıcı vardır. Bu öğretici için kullanacağım [Ninject](http://www.ninject.org/). (Diğer popüler kitaplıkları içerir [Castle Windsor](http://www.castleproject.org/), [Spring.Net](http://www.springframework.net/), [Autofac](https://code.google.com/p/autofac/), [Unity](https://github.com/unitycontainer/unity), ve [StructureMap ](http://docs.structuremap.net).)

Yüklemek için NuGet Paket Yöneticisi'ni kullanın [Ninject Kitaplığı](https://nuget.org/packages/Ninject/3.0.1.10). Visual Studio'da gelen **Araçları** menüsünü seçin **kitaplık Paket Yöneticisi** | **Paket Yöneticisi Konsolu**. Paket Yöneticisi konsolu penceresinde, aşağıdaki komutu girin:

[!code-powershell[Main](dependency-injection/samples/sample13.ps1)]

## <a name="replace-the-signalr-dependency-resolver"></a>SignalR bağımlılık çözümleyiciyi değiştirin

SignalR içinde Ninject kullanmak için türetilen bir sınıf oluşturma **DefaultDependencyResolver**.

[!code-csharp[Main](dependency-injection/samples/sample14.cs)]

Bu sınıf geçersiz kılmalar **GetService** ve **GetServices** yöntemlerinin **DefaultDependencyResolver**. SignalR hub örneklerinin yanı sıra, SignalR tarafından dahili olarak kullanılan çeşitli Hizmetleri dahil olmak üzere çalışma zamanında, çeşitli nesneleri oluşturmak için bu yöntemleri çağırır.

- **GetService** yöntemi, bir türün tek bir örneğini oluşturur. Bu yöntemin Ninject çekirdek 's çağırmaya **TryGet** yöntemi. Bu yöntem null değeri döndürür, varsayılan çözümleyici için geri döner.
- **GetServices** yöntemi, belirtilen bir türün nesnelerinin bir koleksiyonunu oluşturur. Ninject sonuçlardan varsayılan çözümleyici sonuçları ile birleştirmek için bu yöntemi yok sayın.

## <a name="configure-ninject-bindings"></a>Ninject bağlamaları yapılandırma

Artık türü bağlamaları bildirmek için Ninject kullanacağız.

Uygulamanızın Startup.cs sınıfını açın (ya da el ile paket içindeki yönergeler doğrultusunda oluşturduğunuz `readme.txt`, veya oluşturulmuş projenize kimlik doğrulaması ekleyerek). İçinde `Startup.Configuration` yöntemi çağıran Ninject Ninject kapsayıcı oluşturma *çekirdek*.

[!code-csharp[Main](dependency-injection/samples/sample15.cs)]

Bizim özel bağımlılık Çözümleyicisi örneği oluşturun:

[!code-csharp[Main](dependency-injection/samples/sample16.cs)]

İçin bir bağlama oluşturun `IStockTicker` gibi:

[!code-csharp[Main](dependency-injection/samples/sample17.cs)]

Bu kod, iki şey diyor. İlki, uygulamayı gerektiğinde bir `IStockTicker`, çekirdek örneğini oluşturmalısınız `StockTicker`. İkinci olarak, `StockTicker` sınıfı tekil nesnesi olarak oluşturulmuş olmalıdır. Ninject nesnesinin bir kopyasını oluşturmak ve her istek için aynı örnek döndürülecektir.

İçin bir bağlama oluşturun **IHubConnectionContext** gibi:

[!code-csharp[Main](dependency-injection/samples/sample18.cs)]

Bu kod creatres döndüren bir anonim işlev bir **IHubConnection**. **WhenInjectedInto** yöntemi yalnızca oluştururken bu işlevi kullanmak için Ninject söyler `IStockTicker` örnekleri. SignalR oluşturduğu nedeni **IHubConnectionContext** dahili olarak, örnekler ve SignalR bunları nasıl oluşturduğunu geçersiz kılmak istemiyorsanız. Bu işlevi yalnızca uygular bizim `StockTicker` sınıfı.

Bağımlılık çözümleyiciyi içine geçirmek **MapSignalR** bir hub yapılandırmasını ekleyerek yöntemi:

[!code-csharp[Main](dependency-injection/samples/sample19.cs)]

Örnek kullanıcının başlangıç sınıfındaki Startup.ConfigureSignalR yöntemi yeni bir parametre ile güncelleştirin:

[!code-csharp[Main](dependency-injection/samples/sample20.cs)]

SignalR belirtilen çözümleyici kullanacağı artık **MapSignalR**, yerine varsayılan çözümleyici.

İçin tam kodu işte `Startup.Configuration`.

[!code-csharp[Main](dependency-injection/samples/sample21.cs)]

Visual Studio'da StockTicker uygulamayı çalıştırmak için F5 tuşuna basın. Tarayıcı penceresinde gidin `http://localhost:*port*/SignalR.Sample/StockTicker.html`.

![](dependency-injection/_static/image3.png)

Uygulamayı önce olarak tam olarak aynı işlevlere sahiptir. (Bir açıklaması için bkz. [ASP.NET SignalR ile sunucu yayını](../getting-started/tutorial-server-broadcast-with-signalr.md).) Davranışı değiştirdik henüz; yalnızca kod sınamak için korumak ve geliştirmek daha kolay.
