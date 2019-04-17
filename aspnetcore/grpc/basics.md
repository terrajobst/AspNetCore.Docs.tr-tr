---
title: C# içeren gRPC hizmetleri
author: juntaoluo
description: GRPC hizmetleriyle yazarken temel kavramları öğrenin C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/basics
ms.openlocfilehash: 7c5ecf21124414b21f5c36b76e90bde67ac1f958
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672677"
---
# <a name="grpc-services-with-c"></a>C ile gRPC Hizmetleri\#

Bu belgede yazmak için gereken kavramlar açıklanır [gRPC](https://grpc.io/docs/guides/) uygulamalarında C#. Burada ele alınan konulara için her ikisinin de geçerli [C çekirdek](https://grpc.io/blog/grpc-stacks)-tabanlı ve ASP.NET Core tabanlı gRPC uygulamalar.

## <a name="proto-file"></a>proto dosyası

gRPC bir sözleşme öncelikli yaklaşım API geliştirmesini kullanır. Protokol arabellekleri (protobuf) arabirimi tasarım dili (IDL) olarak varsayılan olarak kullanılır. *.Proto* dosyası içerir:

* GRPC hizmet tanımı.
* İstemciler ve sunucular arasında gönderilen iletiler.

Protobuf dosyaları söz dizimi hakkında daha fazla bilgi için bkz. [resmi belgelerine (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).

Örneğin, düşünün *greet.proto* kullanılan dosya [gRPC hizmeti ile çalışmaya başlama](xref:tutorials/grpc/grpc-start):

* Tanımlayan bir `Greeter` hizmeti.
* `Greeter` Hizmeti tanımlayan bir `SayHello` çağırın.
* `SayHello` gönderen bir `HelloRequest` alır ve ileti bir `HelloResponse` ileti:

[!code-proto[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>.Proto dosya eklemek için bir C\# uygulama

*.Proto* dosya ekleyerek bir projede bulunur `<Protobuf>` öğesi grubu:

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

## <a name="c-tooling-support-for-proto-files"></a>C#.Proto dosyaları için araç desteği

Araçlar paketi [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) oluşturmak için gereken C# varlıklarından *.proto* dosyaları. Oluşturulan varlıklar (dosyalar):

* Projenin her zaman bir gerektiğinde başına üretilir.
* Projeye eklenen veya kaynak denetimine iade.
* Bulunan bir derleme yapıtı olan *obj* dizin.

Bu paket, hem sunucu hem de istemci projeleri tarafından gereklidir. `Grpc.Tools` Visual Studio'da Paket Yöneticisi'ni kullanarak veya ekleme eklenebilir bir `<PackageReference>` proje dosyasına:

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=17)]

Bağımlılık ile işaretlenmiş şekilde araçları paket çalışma zamanında gerekli değil `PrivateAssets="All"`.

## <a name="generated-c-assets"></a>Oluşturulan C# varlıklar

Araçlar paketi oluşturur C# iletileri dahil tanımlı temsil eden türleri *.proto* dosyaları.

Sunucu tarafı varlıklar için soyut hizmeti temel türü oluşturulur. Temel tür tanımları tüm gRPC çağrıları kapsanıyorsa içerir *.proto* dosya. Bu temel türünden türetilen ve gRPC çağrıları mantığını uygulayan bir somut hizmet uygulaması oluşturun. İçin `greet.proto`, örnek bir soyut daha önce açıklanan `GreeterBase` içeren bir sanal tür `SayHello` yöntemi oluşturulur. Somut bir uygulama `GreeterService` metodu geçersiz kılar ve gRPC çağrı işleme mantığını uygular.

[!code-csharp[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

İstemci tarafı varlıklar için bir somut istemci türü oluşturulur. GRPC çağrıları *.proto* dosya çevirisi yöntemlerde çağrılabilir somut tür. İçin `greet.proto`, örnek bir somut daha önce açıklanan `GreeterClient` türü oluşturulur. Çağrı `GreeterClient.SayHello` sunucu gRPC çağrısı başlatmak için.

[!code-csharp[](~/tutorials/grpc/grpc-start/samples/GrpcGreeterClient/Program.cs?highlight=5-8&name=snippet)]

Varsayılan olarak, her biri için sunucu ve istemci varlıklar oluşturulan *.proto* dahil dosya `<Protobuf>` öğesi grubu. Yalnızca sunucu varlıkları bir sunucu projesinde oluşturulan emin olmak için `GrpcServices` özniteliği `Server`.

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

Benzer şekilde, öznitelik ayarlanmış `Client` istemci projelerinde.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
