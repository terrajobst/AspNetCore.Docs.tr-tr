---
title: ASP.NET Core SignalR Java istemcisi
author: mikaelm12
description: ASP.NET Core SignalR Java istemcisinin nasıl kullanılacağını öğrenin.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/java-client
ms.openlocfilehash: 6919eabf454f16887e012161a454a4848c45002b
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660519"
---
# <a name="aspnet-core-opno-locsignalr-java-client"></a>ASP.NET Core SignalR Java istemcisi

X [MIKAEL Mengistu](https://twitter.com/MikaelM_12) tarafından

Java istemcisi, Android uygulamaları dahil olmak üzere Java kodundan bir ASP.NET Core SignalR sunucusuna bağlanmasını sağlar. [JavaScript istemcisi](xref:signalr/javascript-client) ve [.NET istemcisi](xref:signalr/dotnet-client)gibi Java istemcisi, bir hub 'a gerçek zamanlı iletiler alıp göndermenizi sağlar. Java istemcisi ASP.NET Core 2,2 ve üzeri sürümlerde kullanılabilir.

Bu makalede başvurulan örnek Java konsol uygulaması SignalR Java istemcisini kullanır.

[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/java-client/sample) ([nasıl indirileceği](xref:index#how-to-download-a-sample))

## <a name="install-the-opno-locsignalr-java-client-package"></a>SignalR Java istemci paketini yükler

*SignalR-1.0.0* jar dosyası istemcilerin SignalR hub 'lara bağlanmasına izin verir. En son JAR dosyası sürüm numarasını bulmak için bkz. [Maven arama sonuçları](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Gradle kullanıyorsanız, *Build. Gradle* dosyanızın `dependencies` bölümüne aşağıdaki satırı ekleyin:

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0'
```

Maven kullanıyorsanız, *Pok. xml* dosyanızın `<dependencies>` öğesinin içine aşağıdaki satırları ekleyin:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Bir hub'ına bağlama

`HubConnection`oluşturmak için `HubConnectionBuilder` kullanılmalıdır. Hub URL 'SI ve günlük düzeyi bir bağlantı derlenirken yapılandırılabilir. `build`önce `HubConnectionBuilder` yöntemlerinden herhangi birini çağırarak gerekli seçenekleri yapılandırın. Bağlantıyı `start`başlatın.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>İstemciden hub yöntemlerini çağırma

Bir `send` çağrısı, hub yöntemini çağırır. Hub yöntemi adını ve hub yönteminde tanımlanan tüm bağımsız değişkenleri `send`geçirin.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

> [!NOTE]
> Azure SignalR hizmetini *sunucusuz modda*kullanıyorsanız, bir istemciden hub yöntemlerini çağıramezsiniz. Daha fazla bilgi için [SignalR hizmeti belgelerine](/azure/azure-signalr/signalr-concept-serverless-development-config)bakın.

## <a name="call-client-methods-from-hub"></a>İstemci hub'ından yöntemleri çağırma

İstemcide hub 'ın çağırakullanabileceği yöntemleri tanımlamak için `hubConnection.on` kullanın. Bağlantıyı başlatmadan önce, oluşturma işleminden sonra yöntemleri tanımlayın.

[!code-java[Define client methods](java-client/sample/src/main/java/Chat.java?range=19-21)]

## <a name="add-logging"></a>Günlüğe kaydetme ekleme

SignalR Java istemcisi, günlük kaydı için [dolayısıyla slf4j](https://www.slf4j.org/) kitaplığını kullanır. Bu, kitaplık kullanıcılarının belirli bir günlüğe kaydetme bağımlılığı vererek kendi belirli günlük uygulamasını seçmesine olanak sağlayan, üst düzey bir günlüğe kaydetme API 'sidir. Aşağıdaki kod parçacığı, SignalR Java istemcisiyle `java.util.logging` nasıl kullanacağınızı gösterir.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Bağımlılıklarınız için günlük kaydını yapılandırmazsanız, DOLAYıSıYLA SLF4J aşağıdaki uyarı iletisiyle varsayılan işlem olmayan bir günlükçü yükler:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Bu, güvenle yoksayılabilir.

## <a name="android-development-notes"></a>Android geliştirme notları

SignalR istemci özelliklerine Android SDK uyumlulukla ilgili olarak, hedef Android SDK sürümünüzü belirtirken aşağıdaki öğeleri göz önünde bulundurun:

* SignalR Java Istemcisi, Android API düzeyi 16 ve sonrasında çalışır.
* Azure [SignalR HIZMETI](/azure/azure-signalr/signalr-overview) TLS 1,2 GEREKTIRDIĞINDEN ve SHA-1 tabanlı şifre paketlerini desteklemediğinden Azure SignalR hizmeti üzerinden bağlanmak IÇIN Android API düzeyi 20 ve üzeri bir sürüm gerekir. Android, API düzeyi 20 ' de [SHA-256 (ve üzeri) şifre paketleri için destek ekledi](https://developer.android.com/reference/javax/net/ssl/SSLSocket) .

## <a name="configure-bearer-token-authentication"></a>Taşıyıcı belirteç kimlik doğrulamasını yapılandırma

Java istemcisinde SignalR, [Httphubconnectionbuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java)'a "erişim belirteci fabrikası" sağlayarak kimlik doğrulaması için kullanılacak bir taşıyıcı belirteç yapılandırabilirsiniz. [Rxjava](https://github.com/ReactiveX/RxJava) [tek bir\<dize >](https://reactivex.io/documentation/single.html)sağlamak Için [withaccesstokenfactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) kullanın. [Tek. ertele](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)çağrısıyla, istemciniz için erişim belirteçleri oluşturmak üzere mantık yazabilirsiniz.

```java
HubConnection hubConnection = HubConnectionBuilder.create("YOUR HUB URL HERE")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

## <a name="known-limitations"></a>Bilinen sınırlamalar

::: moniker range=">= aspnetcore-3.0"

* Yalnızca JSON Protokolü destekleniyor.
* Taşıma geri dönüşü ve sunucu gönderme olayları aktarımı desteklenmez.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* Yalnızca JSON Protokolü destekleniyor.
* Yalnızca WebSockets taşıması desteklenir.
* Akış henüz desteklenmiyor.

::: moniker-end

## <a name="additional-resources"></a>Ek kaynaklar

* [Java API'si başvurusu](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
* [Azure SignalR hizmeti sunucusuz belgeler](/azure/azure-signalr/signalr-concept-serverless-development-config)
