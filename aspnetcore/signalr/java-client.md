---
title: ASP.NET Core SignalR Java istemci
author: mikaelm12
description: ASP.NET Core SignalR Java istemcisi kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 03/14/2019
uid: signalr/java-client
ms.openlocfilehash: 09e5ce23ddcc250d212a8cdf1176f39531a9c0ba
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978496"
---
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR Java istemci

Tarafından [Mikael Mengistu](https://twitter.com/MikaelM_12)

Android uygulamaları dahil olmak üzere Java koddan bir ASP.NET Core SignalR sunucusuna bağlanan Java istemci sağlar. Gibi [JavaScript istemci](xref:signalr/javascript-client) ve [.NET istemci](xref:signalr/dotnet-client), almak ve gerçek zamanlı bir hub'ına ileti göndermek Java istemcisi sağlar. Java istemci, ASP.NET Core 2.2 bulunan ve üzerinde desteklenir.

Bu makalede bahsedilen örnek Java konsol uygulaması SignalR Java istemcisi kullanır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>SignalR Java istemci paketini yükle

*Signalr 1.0.0* JAR dosyasını istemcilerin SignalR hub'ları bağlanmasına izin verir. JAR dosyası en son sürüm numarasını bulmak için bkz: [Maven arama sonuçları](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Gradle kullanıyorsanız, aşağıdaki satırı ekleyin `dependencies` bölümünü, *build.gradle* dosyası:

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

Maven kullanarak eklerseniz içinde aşağıdaki satırları `<dependencies>` öğesinin, *pom.xml* dosyası:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Bir hub'ına bağlama

Kurmak için bir `HubConnection`, `HubConnectionBuilder` kullanılmalıdır. Hub'ı URL'si ve günlük düzeyinde bir bağlantı oluşturulurken yapılandırılabilir. Gerekli tüm seçenekler herhangi birini çağırarak yapılandırma `HubConnectionBuilder` yöntemleri önce `build`. Bağlantıyı başlatmak `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>İstemciden hub yöntemlerini çağırma

Bir çağrı `send` bir hub yöntemini çağırır. Hub yönteminin adını ve hub yöntemi için tanımlanan herhangi bir bağımsız değişken geçirme `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> Azure SignalR hizmeti kullanıyorsanız, *sunucusuz modu*, bir istemciden hub yöntemlerini çağıramazsınız. Daha fazla bilgi için [SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>İstemci hub'ından yöntemleri çağırma

Kullanım `hubConnection.on` hub çağıran istemciye yöntemleri tanımlamak için. Yapı sonra ancak bağlantı başlatmadan önce yöntemleri tanımlar.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>Günlük kaydı ekleme

SignalR Java istemcinin kullandığı [SLF4J](https://www.slf4j.org/) günlüğe kaydetme için kitaplığı. Seçtiğiniz kendi özel günlük uygulama özel günlük bağımlılık olarak getirerek kullanıcıların Kitaplığı'nın izin veren bir üst düzey günlüğe kaydetme API'si var. Aşağıdaki kod parçacığını nasıl kullanılacağını gösterir `java.util.logging` SignalR Java istemcisi ile.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Bağımlılıklarınızı içinde günlüğü yapılandırmazsanız, aşağıdaki uyarı iletisi ile bir varsayılan yok-işlem günlükçü SLF4J yükler:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Bu güvenle yoksayılabilir.

## <a name="android-development-notes"></a>Android geliştirme notları

SignalR istemcisi özellikleri için Android SDK'sı uyumluluk bakımından, hedef Android SDK sürümünü belirtmek için aşağıdakileri göz önünde bulundurun:

* SignalR Java istemci Android API düzeyi 16 ve daha sonra çalışır.
* Azure SignalR hizmeti aracılığıyla bağlanma Android API düzeyi 20 ve sonraki sürümleri gerektirir çünkü [Azure SignalR hizmeti](/azure/azure-signalr/signalr-overview) TLS 1.2 gerektirir ve SHA-1 tabanlı şifre paketleri desteklemez. Android [destek SHA-256 (ve üstü) şifre paketleri eklenen](https://developer.android.com/reference/javax/net/ssl/SSLSocket) API düzeyi 20 içinde.

## <a name="configure-bearer-token-authentication"></a>Taşıyıcı belirteci kimlik doğrulamasını yapılandırma

SignalR Java istemci, bir "erişim belirteci fabrikası" sağlayarak kimlik doğrulaması için kullanılacak bir taşıyıcı belirteç için yapılandırabilirsiniz [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Kullanım [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) sağlamak için bir [RxJava](https://github.com/ReactiveX/RxJava) [tek<String>](http://reactivex.io/documentation/single.html). Çağrısıyla [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), istemciniz için erişim belirteci üretmek için mantığı yazabilirsiniz.

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a>Bilinen sınırlamalar

* Yalnızca JSON Protokolü desteklenir.
* WebSockets taşıma desteklenmiyor.
* Akış henüz desteklenmiyor.

## <a name="additional-resources"></a>Ek kaynaklar

* [Java API'si başvurusu](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* [Sunucusuz Azure SignalR hizmeti belgeleri](/azure/azure-signalr/signalr-concept-serverless-development-config)
