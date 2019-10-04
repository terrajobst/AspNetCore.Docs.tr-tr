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
# <a name="grpc-services-with-c"></a><span data-ttu-id="80c31-103">C ile gRPC Hizmetleri\#</span><span class="sxs-lookup"><span data-stu-id="80c31-103">gRPC services with C\#</span></span>

<span data-ttu-id="80c31-104">Bu belgede, ' de C# [GRPC](https://grpc.io/docs/guides/) uygulamaları yazmak için gereken kavramlar özetlenmektedir.</span><span class="sxs-lookup"><span data-stu-id="80c31-104">This document outlines the concepts needed to write [gRPC](https://grpc.io/docs/guides/) apps in C#.</span></span> <span data-ttu-id="80c31-105">Burada ele alınan konular hem [C Core](https://grpc.io/blog/grpc-stacks)tabanlı hem de ASP.NET Core tabanlı GRPC uygulamaları için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="80c31-105">The topics covered here apply to both [C-core](https://grpc.io/blog/grpc-stacks)-based and ASP.NET Core-based gRPC apps.</span></span>

## <a name="proto-file"></a><span data-ttu-id="80c31-106">Proto dosyası</span><span class="sxs-lookup"><span data-stu-id="80c31-106">proto file</span></span>

<span data-ttu-id="80c31-107">gRPC, API geliştirmesi için bir sözleşmenin ilk yaklaşımını kullanır.</span><span class="sxs-lookup"><span data-stu-id="80c31-107">gRPC uses a contract-first approach to API development.</span></span> <span data-ttu-id="80c31-108">Protokol arabellekleri (protobellek) varsayılan olarak arabirim tasarım dili (IDL) olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="80c31-108">Protocol buffers (protobuf) are used as the Interface Design Language (IDL) by default.</span></span> <span data-ttu-id="80c31-109">*@No__t -1. proto* dosyası şunları içerir:</span><span class="sxs-lookup"><span data-stu-id="80c31-109">The *\*.proto* file contains:</span></span>

* <span data-ttu-id="80c31-110">GRPC hizmetinin tanımı.</span><span class="sxs-lookup"><span data-stu-id="80c31-110">The definition of the gRPC service.</span></span>
* <span data-ttu-id="80c31-111">İstemciler ve sunucular arasında gönderilen iletiler.</span><span class="sxs-lookup"><span data-stu-id="80c31-111">The messages sent between clients and servers.</span></span>

<span data-ttu-id="80c31-112">Prototipsiz dosyaların sözdizimi hakkında daha fazla bilgi için, [resmi belgelere (protoarabellek)](https://developers.google.com/protocol-buffers/docs/proto3)bakın.</span><span class="sxs-lookup"><span data-stu-id="80c31-112">For more information on the syntax of protobuf files, see the [official documentation (protobuf)](https://developers.google.com/protocol-buffers/docs/proto3).</span></span>

<span data-ttu-id="80c31-113">Örneğin, [gRPC hizmetini kullanmaya başlama](xref:tutorials/grpc/grpc-start)bölümünde kullanılan *Greet. proto* dosyasını düşünün:</span><span class="sxs-lookup"><span data-stu-id="80c31-113">For example, consider the *greet.proto* file used in [Get started with gRPC service](xref:tutorials/grpc/grpc-start):</span></span>

* <span data-ttu-id="80c31-114">Bir `Greeter` hizmeti tanımlar.</span><span class="sxs-lookup"><span data-stu-id="80c31-114">Defines a `Greeter` service.</span></span>
* <span data-ttu-id="80c31-115">`Greeter` Hizmet bir`SayHello` çağrıyı tanımlar.</span><span class="sxs-lookup"><span data-stu-id="80c31-115">The `Greeter` service defines a `SayHello` call.</span></span>
* <span data-ttu-id="80c31-116">`SayHello`bir `HelloRequest` ileti gönderir ve bir `HelloReply` ileti alır:</span><span class="sxs-lookup"><span data-stu-id="80c31-116">`SayHello` sends a `HelloRequest` message and receives a `HelloReply` message:</span></span>

[!code-protobuf[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Protos/greet.proto)]

## <a name="add-a-proto-file-to-a-c-app"></a><span data-ttu-id="80c31-117">C\# uygulamasına bir. proto dosyası ekleyin</span><span class="sxs-lookup"><span data-stu-id="80c31-117">Add a .proto file to a C\# app</span></span>

<span data-ttu-id="80c31-118">*@No__t -1. proto* dosyası, `<Protobuf>` öğe grubuna eklenerek bir projeye dahil edilmiştir:</span><span class="sxs-lookup"><span data-stu-id="80c31-118">The *\*.proto* file is included in a project by adding it to the `<Protobuf>` item group:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

## <a name="c-tooling-support-for-proto-files"></a><span data-ttu-id="80c31-119">C#. Proto dosyaları için araç desteği</span><span class="sxs-lookup"><span data-stu-id="80c31-119">C# Tooling support for .proto files</span></span>

<span data-ttu-id="80c31-120">Araç Paketi [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) , C# *@no__t -3. proto* dosyalarından varlıkları oluşturmak için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="80c31-120">The tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) is required to generate the C# assets from *\*.proto* files.</span></span> <span data-ttu-id="80c31-121">Oluşturulan varlıklar (dosyalar):</span><span class="sxs-lookup"><span data-stu-id="80c31-121">The generated assets (files):</span></span>

* <span data-ttu-id="80c31-122">, Projenin oluşturulduğu her seferinde gerekli olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="80c31-122">Are generated on an as-needed basis each time the project is built.</span></span>
* <span data-ttu-id="80c31-123">Projeye eklenmez veya kaynak denetimine iade edilmedi.</span><span class="sxs-lookup"><span data-stu-id="80c31-123">Aren't added to the project or checked into source control.</span></span>
* <span data-ttu-id="80c31-124">*Obj* dizininde bulunan bir yapı yapıtı.</span><span class="sxs-lookup"><span data-stu-id="80c31-124">Are a build artifact contained in the *obj* directory.</span></span>

<span data-ttu-id="80c31-125">Bu paket hem sunucu hem de istemci projeleri için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="80c31-125">This package is required by both the server and client projects.</span></span> <span data-ttu-id="80c31-126">Metapackage öğesine `Grpc.Tools`bir başvuru içerir. `Grpc.AspNetCore`</span><span class="sxs-lookup"><span data-stu-id="80c31-126">The `Grpc.AspNetCore` metapackage includes a reference to `Grpc.Tools`.</span></span> <span data-ttu-id="80c31-127">Sunucu projeleri, Visual `Grpc.AspNetCore` Studio 'da Paket Yöneticisi 'ni kullanarak veya bir proje dosyasına bir `<PackageReference>` ekleyerek ekleyebilir:</span><span class="sxs-lookup"><span data-stu-id="80c31-127">Server projects can add `Grpc.AspNetCore` using the Package Manager in Visual Studio or by adding a `<PackageReference>` to the project file:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=1&range=12)]

<span data-ttu-id="80c31-128">İstemci projeleri, gRPC istemcisini kullanmak için gereken diğer paketlerle birlikte `Grpc.Tools` ' a doğrudan başvurmalıdır.</span><span class="sxs-lookup"><span data-stu-id="80c31-128">Client projects should directly reference `Grpc.Tools` alongside the other packages required to use the gRPC client.</span></span> <span data-ttu-id="80c31-129">Çalışma zamanında araç paketi gerekli değildir, bu nedenle bağımlılık şu şekilde işaretlenir `PrivateAssets="All"`:</span><span class="sxs-lookup"><span data-stu-id="80c31-129">The tooling package isn't required at runtime, so the dependency is marked with `PrivateAssets="All"`:</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/GrpcGreeterClient.csproj?highlight=3&range=9-11)]

## <a name="generated-c-assets"></a><span data-ttu-id="80c31-130">Oluşturulan C# varlıklar</span><span class="sxs-lookup"><span data-stu-id="80c31-130">Generated C# assets</span></span>

<span data-ttu-id="80c31-131">Araç paketi, eklenen C# *@no__t -2. proto* dosyalarında tanımlanan iletileri temsil eden türleri oluşturur.</span><span class="sxs-lookup"><span data-stu-id="80c31-131">The tooling package generates the C# types representing the messages defined in the included *\*.proto* files.</span></span>

<span data-ttu-id="80c31-132">Sunucu tarafı varlıklar için, soyut bir hizmet temel türü oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="80c31-132">For server-side assets, an abstract service base type is generated.</span></span> <span data-ttu-id="80c31-133">Temel tür, *. proto* dosyasında bulunan tüm GRPC çağrılarının tanımlarını içerir.</span><span class="sxs-lookup"><span data-stu-id="80c31-133">The base type contains the definitions of all the gRPC calls contained in the *.proto* file.</span></span> <span data-ttu-id="80c31-134">Bu temel türden türetilen somut bir hizmet uygulamasını oluşturun ve gRPC çağrılarının mantığını uygular.</span><span class="sxs-lookup"><span data-stu-id="80c31-134">Create a concrete service implementation that derives from this base type and implements the logic for the gRPC calls.</span></span> <span data-ttu-id="80c31-135">Daha önce açıklanan örnek için, sanal `SayHello` bir yöntemi içeren `GreeterBase` bir soyut tür oluşturulur. `greet.proto`</span><span class="sxs-lookup"><span data-stu-id="80c31-135">For the `greet.proto`, the example described previously, an abstract `GreeterBase` type that contains a virtual `SayHello` method is generated.</span></span> <span data-ttu-id="80c31-136">Somut bir uygulama `GreeterService` , yöntemini geçersiz kılar ve GRPC çağrısını işleme mantığını uygular.</span><span class="sxs-lookup"><span data-stu-id="80c31-136">A concrete implementation `GreeterService` overrides the method and implements the logic handling the gRPC call.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

<span data-ttu-id="80c31-137">İstemci tarafı varlıklar için somut bir istemci türü oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="80c31-137">For client-side assets, a concrete client type is generated.</span></span> <span data-ttu-id="80c31-138">*. Proto* dosyasındaki GRPC çağrıları, çağrılabilecek somut türdeki yöntemlere çevrilir.</span><span class="sxs-lookup"><span data-stu-id="80c31-138">The gRPC calls in the *.proto* file are translated into methods on the concrete type, which can be called.</span></span> <span data-ttu-id="80c31-139">Daha önce açıklanan örnek için somut `GreeterClient` bir tür oluşturulur. `greet.proto`</span><span class="sxs-lookup"><span data-stu-id="80c31-139">For the `greet.proto`, the example described previously, a concrete `GreeterClient` type is generated.</span></span> <span data-ttu-id="80c31-140">Sunucuya `GreeterClient.SayHelloAsync` bir GRPC çağrısı başlatmak için çağrısı yapın.</span><span class="sxs-lookup"><span data-stu-id="80c31-140">Call `GreeterClient.SayHelloAsync` to initiate a gRPC call to the server.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeterClient/Program.cs?name=snippet)]

<span data-ttu-id="80c31-141">Varsayılan olarak, `<Protobuf>` öğe grubuna eklenen her bir *@no__t -1. proto* dosyası için sunucu ve istemci varlıkları oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="80c31-141">By default, server and client assets are generated for each *\*.proto* file included in the `<Protobuf>` item group.</span></span> <span data-ttu-id="80c31-142">Sunucu projesinde `GrpcServices` yalnızca sunucu varlıklarının oluşturulmasını sağlamak için özniteliği olarak `Server`ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="80c31-142">To ensure only the server assets are generated in a server project, the `GrpcServices` attribute is set to `Server`.</span></span>

[!code-xml[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/GrpcGreeter.csproj?highlight=2&range=7-9)]

<span data-ttu-id="80c31-143">Benzer şekilde, özniteliği istemci projelerinde olarak `Client` ayarlanır.</span><span class="sxs-lookup"><span data-stu-id="80c31-143">Similarly, the attribute is set to `Client` in client projects.</span></span>

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a><span data-ttu-id="80c31-144">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="80c31-144">Additional resources</span></span>

* <xref:grpc/index>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/client>
