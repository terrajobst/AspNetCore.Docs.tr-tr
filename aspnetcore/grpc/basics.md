---
title: gRPC hizmetleriyleC#
author: juntaoluo
description: GRPC hizmetleriyle yazarken temel kavramları öğrenin C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/basics
ms.openlocfilehash: d2ef9316c9bd8c551889c817403f7eb31f48eec6
ms.sourcegitcommit: a467828b5e4eaae291d961ffe2279a571900de23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2019
ms.locfileid: "58142632"
---
# <a name="grpc-services-with-c"></a><span data-ttu-id="b2813-103">gRPC hizmetleriyleC#</span><span class="sxs-lookup"><span data-stu-id="b2813-103">gRPC services with C#</span></span>

<span data-ttu-id="b2813-104">Bu belge yazma için gereken temel kavramları açıklar [gRPC](https://grpc.io/docs/guides/) uygulamalarında C#.</span><span class="sxs-lookup"><span data-stu-id="b2813-104">This document outlines the basic concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="b2813-105">Burada ele alınan konulara için her ikisinin de geçerli [C çekirdek](https://grpc.io/blog/grpc-stacks) ve ASP.NET Core dayalı gRPC uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="b2813-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks) based and ASP.NET Core based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="b2813-106">proto dosyası</span><span class="sxs-lookup"><span data-stu-id="b2813-106">proto file</span></span>

<span data-ttu-id="b2813-107">gRPC bir sözleşme öncelikli yaklaşım API geliştirmesini kullanır.</span><span class="sxs-lookup"><span data-stu-id="b2813-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="b2813-108">Protokol arabellekleri (protobuf) arabirimi tasarım dili (IDL) olarak varsayılan olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b2813-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="b2813-109">*.Proto* dosyası içerir:</span><span class="sxs-lookup"><span data-stu-id="b2813-109">The *.proto* file contains:</span></span>

* <span data-ttu-id="b2813-110">GRPC hizmet tanımı.</span><span class="sxs-lookup"><span data-stu-id="b2813-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="b2813-111">İstemciler ve sunucular arasında gönderilen iletiler.</span><span class="sxs-lookup"><span data-stu-id="b2813-111">The  messages sent between clients and servers.</span></span>

<span data-ttu-id="b2813-112">Protobuf dosyaları söz dizimi hakkında daha fazla bilgi için bkz. [resmi belgelerine (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span><span class="sxs-lookup"><span data-stu-id="b2813-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="b2813-113">Örneğin, düşünün *greet.proto* kullanılan dosya [gRPC hizmeti ile çalışmaya başlama](xref:tutorials/grpc/grpc-start):</span><span class="sxs-lookup"><span data-stu-id="b2813-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="b2813-114">Tanımlayan bir `Greeter` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="b2813-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="b2813-115">`Greeter` Hizmeti tanımlayan bir `SayHello` çağırın.</span><span class="sxs-lookup"><span data-stu-id="b2813-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="b2813-116">`SayHello` gönderen bir `HelloRequest` alır ve ileti bir `HelloResponse` ileti:</span><span class="sxs-lookup"><span data-stu-id="b2813-116">`SayHello` sends a `HelloRequest` message and receives a `HelloResponse` message:</span></span>

[!code-proto[](~/tutorials/grpc/grpc-start/samples/GrpcStart/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="b2813-117">.Proto dosyasına bir C# uygulama</span><span class="sxs-lookup"><span data-stu-id="b2813-117">Add a .proto file to a C# app</span></span>

<span data-ttu-id="b2813-118">*.Proto* dosya ekleyerek bir projede bulunur `<Protobuf>` öğesi grubu:</span><span class="sxs-lookup"><span data-stu-id="b2813-118">The *.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=2&range=7-10)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="b2813-119">C#.Proto dosyaları için araç desteği</span><span class="sxs-lookup"><span data-stu-id="b2813-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="b2813-120">Araçlar paketi [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) oluşturmak için gereken C# varlıklarından *.proto* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="b2813-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *.proto* files.</span></span> <span data-ttu-id="b2813-121">Oluşturulan varlıklar (dosyalar):</span><span class="sxs-lookup"><span data-stu-id="b2813-121">The generated assets (files):</span></span>

* <span data-ttu-id="b2813-122">Projenin her zaman bir gerektiğinde başına üretilir.</span><span class="sxs-lookup"><span data-stu-id="b2813-122">Are generated on a as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="b2813-123">Projeye eklenmediği veya kaynak denetimine iade.</span><span class="sxs-lookup"><span data-stu-id="b2813-123">Are not added to the project or checked into source control.</span></span>
* <span data-ttu-id="b2813-124">Bulunan bir derleme yapıtı olan *obj* dizin.</span><span class="sxs-lookup"><span data-stu-id="b2813-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="b2813-125">Bu paket, hem sunucu hem de istemci projeleri tarafından gereklidir.</span><span class="sxs-lookup"><span data-stu-id="b2813-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="b2813-126">`Grpc.Tools` Visual Studio'da Paket Yöneticisi'ni kullanarak veya ekleme eklenebilir bir `<PackageReference>` proje dosyasına:</span><span class="sxs-lookup"><span data-stu-id="b2813-126">`Grpc.Tools` can be added by using the Package Manager in Visual Studio or adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=1&range=16)]

<span data-ttu-id="b2813-127">Araçlar paketi çalışma zamanında gerekli değildir, bu nedenle, bağımlılık ile işaretlenmelidir `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="b2813-127">The tooling package is not required at runtime, therefore, the dependency should be marked with `PrivateAssets="All"`.</span></span>

## <a name="generated-c-assets"></a><span data-ttu-id="b2813-128">Oluşturulan C# varlıklar</span><span class="sxs-lookup"><span data-stu-id="b2813-128">Generated C# assets</span></span>

<span data-ttu-id="b2813-129">Araçlar paketi oluşturacak C# iletileri dahil tanımlı temsil eden türleri *.proto* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="b2813-129">The tooling package will generate the C# types representing the messages defined in the included *.proto* files.</span></span>

<span data-ttu-id="b2813-130">Sunucu tarafı varlıkları için bir soyut hizmet temel türü oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b2813-130">For server side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="b2813-131">Temel tür tanımları tüm gRPC çağrıları kapsanıyorsa içerir *.proto* dosya.</span><span class="sxs-lookup"><span data-stu-id="b2813-131">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="b2813-132">Ardından bir somut oluşturduğunuz hizmet uygulaması bu temel türünden türetilen ve gRPC çağrıları mantığını uygular.</span><span class="sxs-lookup"><span data-stu-id="b2813-132">You then create a concrete service implementation derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="b2813-133">İçin `greet.proto` örnek daha önce açıklanan bir soyut `GreeterBase` içeren bir sanal tür `SayHello` yöntemi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b2813-133">For the `greet.proto` example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="b2813-134">Somut bir uygulama `GreeterService` metodu geçersiz kılar ve gRPC çağrı işleme mantığını uygular.</span><span class="sxs-lookup"><span data-stu-id="b2813-134">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="b2813-135">İstemci tarafı varlıklar için bir somut istemci türü oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b2813-135">For client side assets, a concrete client type is generated.</span></span> <span data-ttu-id="b2813-136">GRPC çağrıları *.proto* dosya çevirisi yöntemlere çağrılabilen somut tür.</span><span class="sxs-lookup"><span data-stu-id="b2813-136">The gRPC calls in the *.proto* file are translated to methods on the concrete type which can be called.</span></span> <span data-ttu-id="b2813-137">İçin `greet.proto` örnek daha önce açıklanan bir somut `GreeterClient` türü oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="b2813-137">For the `greet.proto` example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="b2813-138">`GreeterClient` Türünü içeren bir `SayHello` sunucu gRPC çağrısı başlatmak için çağrılabilir yöntemi.</span><span class="sxs-lookup"><span data-stu-id="b2813-138">The `GreeterClient` type contains a `SayHello` method that can be called to initiate a gRPC call to the server.</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Client/Program.cs?highlight=9-11&name=snippet)]

<span data-ttu-id="b2813-139">Varsayılan olarak, her biri için hem sunucu hem de istemci varlıklar oluşturulan *.proto* dahil dosya `<Protobuf>` öğesi grubu.</span><span class="sxs-lookup"><span data-stu-id="b2813-139">By default, both server and client assets are generated for each *.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="b2813-140">Yalnızca sunucu varlıkları bir sunucu projesinde oluşturulan emin olmak için `GrpcServices` özniteliği `Server`.</span><span class="sxs-lookup"><span data-stu-id="b2813-140">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/samples/GrpcStart/GrpcGreeter.Server/GrpcGreeter.Server.csproj?highlight=2&range=7-10)]

<span data-ttu-id="b2813-141">Benzer şekilde, öznitelik ayarlanmış `Client` istemci projelerinde.</span><span class="sxs-lookup"><span data-stu-id="b2813-141">Similarly, the attribute is set to `Client` in client projects.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b2813-142">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="b2813-142">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
