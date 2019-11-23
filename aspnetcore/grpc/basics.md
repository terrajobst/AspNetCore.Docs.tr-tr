---
title: C# içeren gRPC hizmetleri
author: juntaoluo
description: İle C#GRPC hizmetlerini yazarken temel kavramları öğrenin.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 07/03/2019
uid: grpc/basics
ms.openlocfilehash: 8d99d036fd4b00fc4568e67ea5225dc006dea4b1
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925181"
---
# <a name="grpc-services-with-c"></a>C\# ile gRPC Hizmetleri

Bu belgede, ' de C# [GRPC](https://grpc.io/docs/guides/) uygulamaları yazmak için gereken kavramlar özetlenmektedir. Burada ele alınan konular hem [C Core](https://grpc.io/blog/grpc-stacks)tabanlı hem de ASP.NET Core tabanlı GRPC uygulamaları için geçerlidir.

## <a name="proto-file"></a>Proto dosyası

gRPC, API geliştirmesi için bir sözleşmenin ilk yaklaşımını kullanır. Protokol arabellekleri (protobellek) varsayılan olarak arabirim tasarım dili (IDL) olarak kullanılır. *\*. proto* dosyası şunları içerir:

* GRPC hizmetinin tanımı.
* İstemciler ve sunucular arasında gönderilen iletiler.

Prototipsiz dosyaların sözdizimi hakkında daha fazla bilgi için, [resmi belgelere (protoarabellek)](https://developers.google.com/protocol-buffers/docs/proto3)bakın.

Örneğin, [gRPC hizmetini kullanmaya başlama](xref:tutorials/grpc/grpc-start)bölümünde kullanılan *Greet. proto* dosyasını düşünün:

* `Greeter` bir hizmeti tanımlar.
* `Greeter` hizmeti `SayHello` çağrısını tanımlar.
* `SayHello` bir `HelloRequest` iletisi gönderir ve bir `HelloReply` iletisi alır:

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>C\# uygulamasına. proto dosyası ekleme

*\*. proto* dosyası bir projeye `<Protobuf>` öğesi grubuna eklenerek eklenir:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a>C#. Proto dosyaları için araç desteği

Araç Paketi [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) , C# *\*. proto* dosyalarından varlıkları oluşturmak için gereklidir. Oluşturulan varlıklar (dosyalar):

* , Projenin oluşturulduğu her seferinde gerekli olarak oluşturulur.
* Projeye eklenmez veya kaynak denetimine iade edilmedi.
* *Obj* dizininde bulunan bir yapı yapıtı.

Bu paket hem sunucu hem de istemci projeleri için gereklidir. `Grpc.AspNetCore` metapackage `Grpc.Tools`bir başvuru içerir. Sunucu projeleri, Visual Studio 'da Paket Yöneticisi 'Ni kullanarak veya proje dosyasına bir `<PackageReference>` ekleyerek `Grpc.AspNetCore` ekleyebilir:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

İstemci projeleri, gRPC istemcisini kullanmak için gereken diğer paketlerle birlikte `Grpc.Tools` doğrudan başvurmalıdır. Araç, çalışma zamanında gerekli değildir, bu nedenle bağımlılık `PrivateAssets="All"`olarak işaretlenir:

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=3&range=9-11)]

## <a name="generated-c-assets"></a>Oluşturulan C# varlıklar

Araç paketi, eklenen C# *\*. proto* dosyalarında tanımlanan iletileri temsil eden türleri oluşturur.

Sunucu tarafı varlıklar için, soyut bir hizmet temel türü oluşturulur. Temel tür, *. proto* dosyasında bulunan tüm GRPC çağrılarının tanımlarını içerir. Bu temel türden türetilen somut bir hizmet uygulamasını oluşturun ve gRPC çağrılarının mantığını uygular. `greet.proto`için, daha önce açıklanan örnek, bir sanal `SayHello` yöntemi içeren soyut bir `GreeterBase` türü oluşturulur. Somut bir uygulama `GreeterService` yöntemi geçersiz kılar ve gRPC çağrısını idare eden mantığı uygular.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

İstemci tarafı varlıklar için somut bir istemci türü oluşturulur. *. Proto* dosyasındaki GRPC çağrıları, çağrılabilecek somut türdeki yöntemlere çevrilir. Daha önce açıklanan örnek `greet.proto`için somut bir `GreeterClient` türü oluşturulur. Sunucuya bir gRPC çağrısı başlatmak için `GreeterClient.SayHelloAsync` çağırın.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet)]

Varsayılan olarak, sunucu ve istemci varlıkları `<Protobuf>` öğesi grubuna eklenen her bir *\*. proto* dosyası için oluşturulur. Sunucu projesinde yalnızca sunucu varlıklarının oluşturulmasını sağlamak için, `GrpcServices` özniteliği `Server`olarak ayarlanır.

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

Benzer şekilde, özniteliği istemci projelerinde `Client` olarak ayarlanır.

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
