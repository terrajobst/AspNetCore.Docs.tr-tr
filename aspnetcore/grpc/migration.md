---
title: Geçirme gRPC hizmetlerden C çekirdekli ASP.NET Core
author: juntaoluo
description: ASP.NET Core yığının en üstünde çalıştırmak için mevcut bir C çekirdek tabanlı gRPC uygulamayı taşımayı öğreneceksiniz.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/migration
ms.openlocfilehash: 520318cfe87708cf5c453c88a5eb54027350db4b
ms.sourcegitcommit: a467828b5e4eaae291d961ffe2279a571900de23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58142626"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>Geçirme gRPC hizmetlerden C çekirdekli ASP.NET Core

Tarafından [John Luo](https://github.com/juntaoluo)

Temel alınan yığın uygulanması nedeniyle, tüm özellikler arasında aynı şekilde çalışır. [C-çekirdek tabanlı gRPC](https://grpc.io/blog/grpc-stacks) uygulamaları ve ASP.NET Core tabanlı uygulamalar. Bu belge iki yığınları arasında geçiş yaparken dikkat edilecek önemli farklar vurgulanmaktadır.

## <a name="grpc-service-implementation-lifetime"></a>gRPC hizmet uygulama ömrü

ASP.NET Core yığınında gRPC Hizmetleri, varsayılan olarak, ile oluşturulur bir [kapsamındaki ömrü](xref:fundamentals/dependency-injection). Buna karşılık, varsayılan olarak gRPC C çekirdekli bir Singleton yaşam süresi ile bir hizmeti bağlar.

Kapsamındaki ömrü kapsamındaki ömürleriyle diğer hizmetlerin çözümlenmesi hizmet uygulaması sağlar. Örneğin kapsamındaki yaşam süresi de çözebilirsiniz `DBContext`, oluşturucu ekleme yoluyla DI kapsayıcısından. Kapsamındaki ömrü kullanarak:

* Hizmet uygulaması yeni bir örneğini her istek için oluşturulur.
* Uygulama türü üzerindeki örnek üyelerinden keşfi arasında durum paylaşma mümkün değildir.
* Paylaşılan durumlar DI kapsayıcıdaki tek bir hizmet depolamak için kullanılan beklenir. Depolanan paylaşılan durumlar gRPC hizmet uygulamasının oluşturucuda çözümlenir. 

Kapsamındaki ve tekil ömrü hakkında daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection>.

### <a name="add-a-singleton-service"></a>Tek bir hizmet ekleyin

GRPC C çekirdek uygulamadan ASP.NET Core geçişi kolaylaştırmak için hizmet uygulamasının hizmet ömrü kapsamındaki tekliye değiştirmek mümkündür. Bu hizmet uygulaması örneğini DI kapsayıcıya ekleme içerir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

Ancak, tekil ömrü ile hizmet uygulaması artık kapsamındaki Hizmetleri Oluşturucu ekleme yoluyla çözmek mümkün olacaktır.

## <a name="configure-grpc-services-options"></a>GRPC Hizmetleri seçeneklerini yapılandırma

C-çekirdek tabanlı uygulamalar, ayarlar gibi `grpc.max_receive_message_length` ve `grpc.max_send_message_length` ile yapılandırılmış `ChannelOption` olduğunda [oluşturmak `Server` örneği](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).

ASP.NET core'da `GrpcServiceOptions` bu ayarları yapılandırmak için bir yol sağlar. Ayarları, genel olarak tüm gRPC hizmetlere ya da bir bireysel hizmet uygulama türü için uygulanabilir. Bireysel hizmet uygulaması türleri için belirtilen seçenekler yapılandırıldığında genel ayarları geçersiz kılar.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddGrpc(globalOptions =>
        {
            // Global settings
            globalOptions.SendMaxMessageSize = 4096
            globalOptions.ReceiveMaxMessageSize = 4096
        })
        .AddServiceOptions<GreeterService>(greeterOptions =>
        {
            // GreeterService settings. These will override global settings
            globalOptions.SendMaxMessageSize = 2048
            globalOptions.ReceiveMaxMessageSize = 2048
        })
}
```

## <a name="logging"></a>Günlüğe Kaydetme

C / çekirdek tabanlı uygulamalarınızı Bel `GrpcEnvironment` için [günlükçüden yapılandırma](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) hata ayıklama amacıyla. ASP.NET Core yığını aracılığıyla bu işlevselliği sağlar [günlüğe kaydetme API'si](xref:fundamentals/logging/index). Örneğin bir Günlükçü Oluşturucu ekleme yoluyla gRPC hizmeti eklenebilir:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

C / çekirdek tabanlı uygulamalarınızı yapılandırma HTTPS üzerinden [ `Server.Ports` özelliği](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports). Benzer bir kavram, ASP.NET Core sunucularını yapılandırmak için kullanılır. Örneğin, Kestrel kullanır [uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) bu işlevselliği.

## <a name="interceptors-and-middlewares"></a>Kesiciler ve Middlewares

ASP.NET Core [middlewares](xref:fundamentals/middleware/index) tabanlı gRPC uygulamaları C core'da dinleyicileri karşılaştırdığınızda benzer işlevler sunar. GRPC isteğini işleyen bir pipleline oluşturmak için kullanılan her ikisi de olarak Middlewares ve dinleyicileri kavramsal olarak aynıdır. Her ikisi de, önce veya sonra ardışık düzende sonraki bileşene gerçekleştirilecek işin izin verin. ASP.NET Core middlewares dinleyicileri soyutlama kullanmanın gRPC katmanda çalışır ancak temel alınan HTTP/2 iletileri ancak çalışması [ `ServerCallContext` ](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
