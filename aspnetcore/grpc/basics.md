---
title: C# içeren gRPC hizmetleri
author: juntaoluo
description: GRPC hizmetleriyle yazarken temel kavramları öğrenin C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/basics
ms.openlocfilehash: 936561a3ad04183aff4c3ba1c9b0e8ab20dcbe12
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264772"
---
# <a name="grpc-services-with-c"></a>C ile gRPC Hizmetleri\#

Bu belge yazma için gereken temel kavramları açıklar [gRPC](https://grpc.io/docs/guides/) uygulamalarında C#. Burada ele alınan konulara için her ikisinin de geçerli [C çekirdek](https://grpc.io/blog/grpc-stacks) ve ASP.NET Core dayalı gRPC uygulamalar.

## <a name="proto-file"></a>proto dosyası

gRPC bir sözleşme öncelikli yaklaşım API geliştirmesini kullanır. Protokol arabellekleri (protobuf) arabirimi tasarım dili (IDL) olarak varsayılan olarak kullanılır. *.Proto* dosyası içerir:

* GRPC hizmet tanımı.
* İstemciler ve sunucular arasında gönderilen iletiler.

Protobuf dosyaları söz dizimi hakkında daha fazla bilgi için bkz. [resmi belgelerine (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).

Örneğin, düşünün *greet.proto* kullanılan dosya [gRPC hizmeti ile çalışmaya başlama](xref:tutorials/grpc/grpc-start):

* Tanımlayan bir `Greeter` hizmeti.
* `Greeter` Hizmeti tanımlayan bir `SayHello` çağırın.
* `SayHello` gönderen bir `HelloRequest` alır ve ileti bir `HelloResponse` ileti:

[!code-proto[](~/tutorials/grpc/grpc-start/samples/GrpcStart/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a>.Proto dosyasına bir C# uygulama

*.Proto* dosya ekleyerek bir projede bulunur `<Protobuf>` öğesi grubu:

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=2&range=7-10)]

## <a name="c-tooling-support-for-proto-files"></a>C#.Proto dosyaları için araç desteği

Araçlar paketi [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) oluşturmak için gereken C# varlıklarından *.proto* dosyaları. Oluşturulan varlıklar (dosyalar):

* Projenin her zaman bir gerektiğinde başına üretilir.
* Projeye eklenmediği veya kaynak denetimine iade.
* Bulunan bir derleme yapıtı olan *obj* dizin.

Bu paket, hem sunucu hem de istemci projeleri tarafından gereklidir. `Grpc.Tools` Visual Studio'da Paket Yöneticisi'ni kullanarak veya ekleme eklenebilir bir `<PackageReference>` proje dosyasına:

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=1&range=16)]

Araçlar paketi çalışma zamanında gerekli değildir, bu nedenle, bağımlılık ile işaretlenmelidir `PrivateAssets="All"`.

## <a name="generated-c-assets"></a>Oluşturulan C# varlıklar

Araçlar paketi oluşturacak C# iletileri dahil tanımlı temsil eden türleri *.proto* dosyaları.

Sunucu tarafı varlıkları için bir soyut hizmet temel türü oluşturulur. Temel tür tanımları tüm gRPC çağrıları kapsanıyorsa içerir *.proto* dosya. Ardından bir somut oluşturduğunuz hizmet uygulaması bu temel türünden türetilen ve gRPC çağrıları mantığını uygular. İçin `greet.proto` örnek daha önce açıklanan bir soyut `GreeterBase` içeren bir sanal tür `SayHello` yöntemi oluşturulur. Somut bir uygulama `GreeterService` metodu geçersiz kılar ve gRPC çağrı işleme mantığını uygular.

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Services/GreeterService.cs?name=snippet)]

İstemci tarafı varlıklar için bir somut istemci türü oluşturulur. GRPC çağrıları *.proto* dosya çevirisi yöntemlere çağrılabilen somut tür. İçin `greet.proto` örnek daha önce açıklanan bir somut `GreeterClient` türü oluşturulur. `GreeterClient` Türünü içeren bir `SayHello` sunucu gRPC çağrısı başlatmak için çağrılabilir yöntemi.

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Client/Program.cs?highlight=9-11&name=snippet)]

Varsayılan olarak, her biri için hem sunucu hem de istemci varlıklar oluşturulan *.proto* dahil dosya `<Protobuf>` öğesi grubu. Yalnızca sunucu varlıkları bir sunucu projesinde oluşturulan emin olmak için `GrpcServices` özniteliği `Server`.

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=2&range=7-10)]

Benzer şekilde, öznitelik ayarlanmış `Client` istemci projelerinde.

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
