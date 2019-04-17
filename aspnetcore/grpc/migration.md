---
title: Geçirme gRPC hizmetlerden C çekirdekli ASP.NET Core
author: juntaoluo
description: ASP.NET Core yığının en üstünde çalıştırmak için mevcut bir C çekirdek tabanlı gRPC uygulamayı taşımayı öğreneceksiniz.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 4d489b5aecf2e15fbbe3ac472b991a4365cd47c1
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672625"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>Geçirme gRPC hizmetlerden C çekirdekli ASP.NET Core

Tarafından [John Luo](https://github.com/juntaoluo)

Temel alınan yığın uygulanması nedeniyle, tüm özellikler arasında aynı şekilde çalışır. [C-çekirdek tabanlı gRPC](https://grpc.io/blog/grpc-stacks) uygulamaları ve ASP.NET Core tabanlı uygulamalar. Bu belge, iki yığın arasında geçirmek için temel farklılıklar vurgular.

## <a name="grpc-service-implementation-lifetime"></a>gRPC hizmet uygulama ömrü

ASP.NET Core yığınında gRPC Hizmetleri, varsayılan olarak, ile oluşturulan bir [kapsamlı ömrü](xref:fundamentals/dependency-injection#service-lifetimes). Buna karşılık, varsayılan olarak gRPC C çekirdekli bir hizmetle bağlar bir [tekil ömrü](xref:fundamentals/dependency-injection#service-lifetimes).

Kapsamı belirlenmiş bir yaşam süresi, kapsamlı ömürleriyle diğer hizmetlerin çözümlenmesi hizmet uygulaması sağlar. Örneğin, kapsamlı bir yaşam süresi de çözebilirsiniz `DBContext` Oluşturucu ekleme yoluyla DI kapsayıcısından. Kapsamı belirlenmiş bir yaşam süresi'ı kullanma:

* Hizmet uygulaması yeni bir örneğini her istek için oluşturulur.
* Uygulama türü üzerindeki örnek üyelerinden keşfi arasında durum paylaşma mümkün değildir.
* Paylaşılan durumlar DI kapsayıcıdaki tek bir hizmet depolamak için kullanılan beklenir. Depolanan paylaşılan durumlar gRPC hizmet uygulamasının oluşturucuda çözümlenir.

Hizmet ömrü hakkında daha fazla bilgi için bkz. <xref:fundamentals/dependency-injection#service-lifetimes>.

### <a name="add-a-singleton-service"></a>Tek bir hizmet ekleyin

GRPC C çekirdek uygulamadan ASP.NET Core geçişi kolaylaştırmak için tekliye kapsamlı hizmet uygulamasından hizmet ömrü değiştirmek mümkündür. Bu hizmet uygulaması örneğini DI kapsayıcıya ekleme içerir:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

Ancak, bir singleton yaşam süresi ile bir hizmet uygulaması artık Oluşturucu ekleme kapsamlı hizmetleriyle çözümlemeleri değil.

## <a name="configure-grpc-services-options"></a>GRPC Hizmetleri seçeneklerini yapılandırma

Ayarları gibi C-çekirdek tabanlı uygulamalarda `grpc.max_receive_message_length` ve `grpc.max_send_message_length` ile yapılandırılmış `ChannelOption` olduğunda [sunucu örneği oluşturmak](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).

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

C-çekirdek tabanlı uygulamaları Bel `GrpcEnvironment` için [günlükçüden yapılandırma](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) hata ayıklama amacıyla. ASP.NET Core yığını aracılığıyla bu işlevselliği sağlar [API'si günlük kaydını](xref:fundamentals/logging/index). Örneğin, bir Günlükçü Oluşturucu ekleme yoluyla gRPC hizmeti eklenebilir:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

C-çekirdek tabanlı uygulamalarını yapılandırma HTTPS üzerinden [Server.Ports özelliği](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports). Benzer bir kavram, ASP.NET Core sunucularını yapılandırmak için kullanılır. Örneğin, Kestrel kullanır [uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) bu işlevselliği.

## <a name="interceptors-and-middleware"></a>Kesiciler ve ara yazılım

ASP.NET Core [ara yazılım](xref:fundamentals/middleware/index) gRPC C-çekirdek tabanlı uygulamalarda dinleyicileri ile karşılaştırıldığında benzer işlevler sunar. GRPC isteğini işleyen bir işlem hattı oluşturmak için kullanılan her ikisi de olarak ara yazılım ve dinleyicileri kavramsal olarak aynıdır. Her ikisi de, önce veya sonra ardışık düzende sonraki bileşene gerçekleştirilecek işin izin verin. ASP.NET Core ara yazılım dinleyicileri soyutlama kullanmanın gRPC katmanda çalışması sırasında temel alınan HTTP/2 iletilerde ancak çalışır [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
