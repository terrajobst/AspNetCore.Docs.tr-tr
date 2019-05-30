---
title: C# içeren gRPC hizmetleri
author: juntaoluo
description: GRPC hizmetleriyle yazarken temel kavramları öğrenin C#.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/basics
ms.openlocfilehash: 5a88bd0e9f789058b3606691c5ebd9a74325ac9b
ms.sourcegitcommit: 4d05e30567279072f1b070618afe58ae1bcefd5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376338"
---
# <a name="grpc-services-with-c"></a><span data-ttu-id="7ca08-103">C ile gRPC Hizmetleri\#</span><span class="sxs-lookup"><span data-stu-id="7ca08-103">gRPC services with C\#</span></span>

<span data-ttu-id="7ca08-104">Bu belgede yazmak için gereken kavramlar açıklanır [gRPC](https://grpc.io/docs/guides/) uygulamalarında C#.</span><span class="sxs-lookup"><span data-stu-id="7ca08-104">This document outlines the concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="7ca08-105">Burada ele alınan konulara için her ikisinin de geçerli [C çekirdek](https://grpc.io/blog/grpc-stacks)-tabanlı ve ASP.NET Core tabanlı gRPC uygulamalar.</span><span class="sxs-lookup"><span data-stu-id="7ca08-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks)-based and ASP.NET Core-based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="7ca08-106">proto dosyası</span><span class="sxs-lookup"><span data-stu-id="7ca08-106">proto file</span></span>

<span data-ttu-id="7ca08-107">gRPC bir sözleşme öncelikli yaklaşım API geliştirmesini kullanır.</span><span class="sxs-lookup"><span data-stu-id="7ca08-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="7ca08-108">Protokol arabellekleri (protobuf) arabirimi tasarım dili (IDL) olarak varsayılan olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="7ca08-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="7ca08-109">*.Proto* dosyası içerir:</span><span class="sxs-lookup"><span data-stu-id="7ca08-109">The *.proto* file contains:</span></span>

* <span data-ttu-id="7ca08-110">GRPC hizmet tanımı.</span><span class="sxs-lookup"><span data-stu-id="7ca08-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="7ca08-111">İstemciler ve sunucular arasında gönderilen iletiler.</span><span class="sxs-lookup"><span data-stu-id="7ca08-111">The messages sent between clients and servers.</span></span>

<span data-ttu-id="7ca08-112">Protobuf dosyaları söz dizimi hakkında daha fazla bilgi için bkz. [resmi belgelerine (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span><span class="sxs-lookup"><span data-stu-id="7ca08-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="7ca08-113">Örneğin, düşünün *greet.proto* kullanılan dosya [gRPC hizmeti ile çalışmaya başlama](xref:tutorials/grpc/grpc-start):</span><span class="sxs-lookup"><span data-stu-id="7ca08-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="7ca08-114">Tanımlayan bir `Greeter` hizmeti.</span><span class="sxs-lookup"><span data-stu-id="7ca08-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="7ca08-115">`Greeter` Hizmeti tanımlayan bir `SayHello` çağırın.</span><span class="sxs-lookup"><span data-stu-id="7ca08-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="7ca08-116">`SayHello` gönderen bir `HelloRequest` alır ve ileti bir `HelloResponse` ileti:</span><span class="sxs-lookup"><span data-stu-id="7ca08-116">`SayHello` sends a `HelloRequest` message and receives a `HelloResponse` message:</span></span>

[!code-proto[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="7ca08-117">.Proto dosya eklemek için bir C\# uygulama</span><span class="sxs-lookup"><span data-stu-id="7ca08-117">Add a .proto file to a C\# app</span></span>

<span data-ttu-id="7ca08-118">*.Proto* dosya ekleyerek bir projede bulunur `<Protobuf>` öğesi grubu:</span><span class="sxs-lookup"><span data-stu-id="7ca08-118">The *.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="7ca08-119">C#.Proto dosyaları için araç desteği</span><span class="sxs-lookup"><span data-stu-id="7ca08-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="7ca08-120">Araçlar paketi [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) oluşturmak için gereken C# varlıklarından *.proto* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="7ca08-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *.proto* files.</span></span> <span data-ttu-id="7ca08-121">Oluşturulan varlıklar (dosyalar):</span><span class="sxs-lookup"><span data-stu-id="7ca08-121">The generated assets (files):</span></span>

* <span data-ttu-id="7ca08-122">Projenin her zaman bir gerektiğinde başına üretilir.</span><span class="sxs-lookup"><span data-stu-id="7ca08-122">Are generated on an as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="7ca08-123">Projeye eklenen veya kaynak denetimine iade.</span><span class="sxs-lookup"><span data-stu-id="7ca08-123">Aren't added to the project or checked into source control.</span></span>
* <span data-ttu-id="7ca08-124">Bulunan bir derleme yapıtı olan *obj* dizin.</span><span class="sxs-lookup"><span data-stu-id="7ca08-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="7ca08-125">Bu paket, hem sunucu hem de istemci projeleri tarafından gereklidir.</span><span class="sxs-lookup"><span data-stu-id="7ca08-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="7ca08-126">`Grpc.Tools` Visual Studio'da Paket Yöneticisi'ni kullanarak veya ekleme eklenebilir bir `<PackageReference>` proje dosyasına:</span><span class="sxs-lookup"><span data-stu-id="7ca08-126">`Grpc.Tools` can be added by using the Package Manager in Visual Studio or adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=17)]

<span data-ttu-id="7ca08-127">Bağımlılık ile işaretlenmiş şekilde araçları paket çalışma zamanında gerekli değil `PrivateAssets="All"`.</span><span class="sxs-lookup"><span data-stu-id="7ca08-127">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`.</span></span>

## <a name="generated-c-assets"></a><span data-ttu-id="7ca08-128">Oluşturulan C# varlıklar</span><span class="sxs-lookup"><span data-stu-id="7ca08-128">Generated C# assets</span></span>

<span data-ttu-id="7ca08-129">Araçlar paketi oluşturur C# iletileri dahil tanımlı temsil eden türleri *.proto* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="7ca08-129">The tooling package generates the C# types representing the messages defined in the included *.proto* files.</span></span>

<span data-ttu-id="7ca08-130">Sunucu tarafı varlıklar için soyut hizmeti temel türü oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7ca08-130">For server-side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="7ca08-131">Temel tür tanımları tüm gRPC çağrıları kapsanıyorsa içerir *.proto* dosya.</span><span class="sxs-lookup"><span data-stu-id="7ca08-131">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="7ca08-132">Bu temel türünden türetilen ve gRPC çağrıları mantığını uygulayan bir somut hizmet uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="7ca08-132">Create a concrete service implementation that derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="7ca08-133">İçin `greet.proto`, örnek bir soyut daha önce açıklanan `GreeterBase` içeren bir sanal tür `SayHello` yöntemi oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7ca08-133">For the `greet.proto`, the example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="7ca08-134">Somut bir uygulama `GreeterService` metodu geçersiz kılar ve gRPC çağrı işleme mantığını uygular.</span><span class="sxs-lookup"><span data-stu-id="7ca08-134">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="7ca08-135">İstemci tarafı varlıklar için bir somut istemci türü oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7ca08-135">For client-side assets, a concrete client type is generated.</span></span> <span data-ttu-id="7ca08-136">GRPC çağrıları *.proto* dosya çevirisi yöntemlerde çağrılabilir somut tür.</span><span class="sxs-lookup"><span data-stu-id="7ca08-136">The gRPC calls in the *.proto* file are translated into methods on the concrete type, which can be called.</span></span> <span data-ttu-id="7ca08-137">İçin `greet.proto`, örnek bir somut daha önce açıklanan `GreeterClient` türü oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="7ca08-137">For the `greet.proto`, the example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="7ca08-138">Çağrı `GreeterClient.SayHello` sunucu gRPC çağrısı başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="7ca08-138">Call `GreeterClient.SayHello` to initiate a gRPC call to the server.</span></span>

[!code-csharp[](~/tutorials//grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?highlight=5-8&name=snippet)]

<span data-ttu-id="7ca08-139">Varsayılan olarak, her biri için sunucu ve istemci varlıklar oluşturulan *.proto* dahil dosya `<Protobuf>` öğesi grubu.</span><span class="sxs-lookup"><span data-stu-id="7ca08-139">By default, server and client assets are generated for each *.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="7ca08-140">Yalnızca sunucu varlıkları bir sunucu projesinde oluşturulan emin olmak için `GrpcServices` özniteliği `Server`.</span><span class="sxs-lookup"><span data-stu-id="7ca08-140">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials//grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-11)]

<span data-ttu-id="7ca08-141">Benzer şekilde, öznitelik ayarlanmış `Client` istemci projelerinde.</span><span class="sxs-lookup"><span data-stu-id="7ca08-141">Similarly, the attribute is set to `Client` in client projects.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7ca08-142">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="7ca08-142">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>
