---
title: ASP.NET Core SignalR Java istemci
author: mikaelm12
description: ASP.NET Core SignalR Java istemcisi kullanmayı öğrenin.
monikerRange: '>= aspnetcore-2.2'
ms.author: mimengis
ms.custom: mvc
ms.date: 10/18/2018
uid: signalr/java-client
ms.openlocfilehash: 3d2cfe5f58cc41741512c68cebbbc90e8f714cba
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148791"
---
# <a name="aspnet-core-signalr-java-client"></a>ASP.NET Core SignalR Java istemci

Tarafından [Mikael Mengistu](https://twitter.com/MikaelM_12)

Android uygulamaları dahil olmak üzere Java koddan bir ASP.NET Core SignalR sunucusuna bağlanan Java istemci sağlar. Gibi [JavaScript istemci](xref:signalr/javascript-client) ve [.NET istemci](xref:signalr/dotnet-client), almak ve gerçek zamanlı bir hub'ına ileti göndermek Java istemcisi sağlar. Java istemci, ASP.NET Core 2.2 bulunan ve üzerinde desteklenir.

Bu makalede bahsedilen örnek Java konsol uygulaması SignalR Java istemcisi kullanır.

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/java-client/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="install-the-signalr-java-client-package"></a>SignalR Java istemci paketini yükle

*1.0.0 preview3 35501 signalr* JAR dosyasını istemcilerin SignalR hub'ları bağlanmasına izin verir. JAR dosyası en son sürüm numarasını bulmak için bkz: [Maven arama sonuçları](https://search.maven.org/search?q=g:com.microsoft.signalr%20AND%20a:signalr).

Gradle kullanıyorsanız, aşağıdaki satırı ekleyin `dependencies` bölümünü, *build.gradle* dosyası:

```gradle
implementation 'com.microsoft.signalr:signalr:1.0.0-preview3-35501'
implementation 'io.reactivex.rxjava2:rxjava:2.2.2'
```

Maven kullanarak eklerseniz içinde aşağıdaki satırları `<dependencies>` öğesinin, *pom.xml* dosyası:

[!code-xml[pom.xml dependency element](java-client/sample/pom.xml?name=snippet_dependencyElement)]

## <a name="connect-to-a-hub"></a>Bir hub'ına bağlama

Kurmak için bir `HubConnection`, `HubConnectionBuilder` kullanılmalıdır. Hub'ı URL'si ve günlük düzeyinde bir bağlantı oluşturulurken yapılandırılabilir. Gerekli tüm seçenekler herhangi birini çağırarak yapılandırma `HubConnectionBuilder` yöntemleri önce `build`. Bağlantıyı başlatmak `start`.

[!code-java[Build hub connection](java-client/sample/src/main/java/Chat.java?range=16-17)]

## <a name="call-hub-methods-from-client"></a>İstemciden hub yöntemlerini çağırma

Bir çağrı `send` bir hub yöntemini çağırır. Hub yönteminin adını ve hub yöntemi için tanımlanan herhangi bir bağımsız değişken geçirme `send`.

[!code-java[send method](java-client/sample/src/main/java/Chat.java?range=28)]

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

## <a name="known-limitations"></a>Bilinen sınırlamalar

Bu bir Java istemci Önizleme sürümüdür. Bazı özellikler desteklenmez:

* Yalnızca JSON Protokolü desteklenir.
* WebSockets taşıma desteklenmiyor.
* Akış henüz desteklenmiyor.

## <a name="additional-resources"></a>Ek kaynaklar

* [Java API'si başvurusu](/java/api/com.microsoft.signalr?view=aspnet-signalr-java)
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/publish-to-azure-web-app>
