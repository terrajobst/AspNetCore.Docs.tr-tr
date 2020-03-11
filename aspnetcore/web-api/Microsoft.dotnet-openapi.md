---
title: Openapı kullanarak ASP.NET Core uygulamaları geliştirme
author: ryanbrandenburg
description: Openapı dosyalarına başvurular eklemek için ' Microsoft. DotNet-openapı ' aracının nasıl kullanılacağını gösterir.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: 079e36511b63c186ffa7726bdb1e3c3bcbda9d34
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655269"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a><span data-ttu-id="93610-103">Openapı araçlarını kullanarak ASP.NET Core uygulamaları geliştirme</span><span class="sxs-lookup"><span data-stu-id="93610-103">Develop ASP.NET Core apps using OpenAPI tools</span></span>

<span data-ttu-id="93610-104">İle Ryan Brandenburg</span><span class="sxs-lookup"><span data-stu-id="93610-104">By Ryan Brandenburg</span></span>

<span data-ttu-id="93610-105">[Microsoft. DotNet-openapı](https://www.nuget.org/packages/Microsoft.dotnet-openapi) , bir proje Içindeki [openapı](https://github.com/OAI/OpenAPI-Specification) başvurularını yönetmek için [.NET Core küresel bir araçtır](/dotnet/core/tools/global-tools) .</span><span class="sxs-lookup"><span data-stu-id="93610-105">[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) is a [.NET Core Global Tool](/dotnet/core/tools/global-tools) for managing [OpenAPI](https://github.com/OAI/OpenAPI-Specification) references within a project.</span></span>

## <a name="installation"></a><span data-ttu-id="93610-106">Yükleme</span><span class="sxs-lookup"><span data-stu-id="93610-106">Installation</span></span>

<span data-ttu-id="93610-107">`Microsoft.dotnet-openapi`yüklemek için şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="93610-107">To install `Microsoft.dotnet-openapi`, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a><span data-ttu-id="93610-108">Ekle</span><span class="sxs-lookup"><span data-stu-id="93610-108">Add</span></span>

<span data-ttu-id="93610-109">Bu sayfadaki komutlardan herhangi birini kullanarak bir Openapı başvurusu eklemek, *. csproj* dosyasına aşağıdakine benzer bir `<OpenApiReference />` öğesi ekler:</span><span class="sxs-lookup"><span data-stu-id="93610-109">Adding an OpenAPI reference using any of the commands on this page adds an `<OpenApiReference />` element similar to the following to the *.csproj* file:</span></span>

```xml
<OpenApiReference Include="openapi.json" />
```

<span data-ttu-id="93610-110">Uygulamanın oluşturulan istemci kodunu çağırması için önceki başvuru gereklidir.</span><span class="sxs-lookup"><span data-stu-id="93610-110">The preceding reference is required for the app to call the generated client code.</span></span>

<!-- TODO: Restore after https://github.com/dotnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a><span data-ttu-id="93610-111">Dosya Ekle</span><span class="sxs-lookup"><span data-stu-id="93610-111">Add File</span></span>

#### <a name="options"></a><span data-ttu-id="93610-112">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="93610-112">Options</span></span>

| <span data-ttu-id="93610-113">Short seçeneği</span><span class="sxs-lookup"><span data-stu-id="93610-113">Short option</span></span>| <span data-ttu-id="93610-114">Long seçeneği</span><span class="sxs-lookup"><span data-stu-id="93610-114">Long option</span></span>| <span data-ttu-id="93610-115">Açıklama</span><span class="sxs-lookup"><span data-stu-id="93610-115">Description</span></span> | <span data-ttu-id="93610-116">Örnek</span><span class="sxs-lookup"><span data-stu-id="93610-116">Example</span></span> |
|-------|------|-------|---------|
| <span data-ttu-id="93610-117">-p</span><span class="sxs-lookup"><span data-stu-id="93610-117">-p</span></span>|<span data-ttu-id="93610-118">--updateProject</span><span class="sxs-lookup"><span data-stu-id="93610-118">--updateProject</span></span> | <span data-ttu-id="93610-119">Üzerinde çalışılacak proje.</span><span class="sxs-lookup"><span data-stu-id="93610-119">The project to operate on.</span></span> |<span data-ttu-id="93610-120">DotNet openapı dosya Ekle *--updateproject .\Ref.exe* .\Openapi.exe</span><span class="sxs-lookup"><span data-stu-id="93610-120">dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="93610-121">-c</span><span class="sxs-lookup"><span data-stu-id="93610-121">-c</span></span>|<span data-ttu-id="93610-122">--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="93610-122">--code-generator</span></span>| <span data-ttu-id="93610-123">Başvuruya uygulanacak kod Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="93610-123">The code generator to apply to the reference.</span></span> <span data-ttu-id="93610-124">Seçenekler `NSwagCSharp` ve `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="93610-124">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> <span data-ttu-id="93610-125">`--code-generator` belirtilmemişse araç varsayılan olarak `NSwagCSharp`.</span><span class="sxs-lookup"><span data-stu-id="93610-125">If `--code-generator` is not specified the tooling defaults to `NSwagCSharp`.</span></span>|<span data-ttu-id="93610-126">DotNet openapı Add dosyası .\Openapi.exe JSON--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="93610-126">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="93610-127">-h</span><span class="sxs-lookup"><span data-stu-id="93610-127">-h</span></span>|<span data-ttu-id="93610-128">--yardım</span><span class="sxs-lookup"><span data-stu-id="93610-128">--help</span></span>|<span data-ttu-id="93610-129">Yardım bilgilerini göster</span><span class="sxs-lookup"><span data-stu-id="93610-129">Show help information</span></span>|<span data-ttu-id="93610-130">DotNet openapı dosya Ekle--yardım</span><span class="sxs-lookup"><span data-stu-id="93610-130">dotnet openapi add file --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="93610-131">Bağımsız Değişkenler</span><span class="sxs-lookup"><span data-stu-id="93610-131">Arguments</span></span>

|  <span data-ttu-id="93610-132">Bağımsız Değişken</span><span class="sxs-lookup"><span data-stu-id="93610-132">Argument</span></span>  | <span data-ttu-id="93610-133">Açıklama</span><span class="sxs-lookup"><span data-stu-id="93610-133">Description</span></span> | <span data-ttu-id="93610-134">Örnek</span><span class="sxs-lookup"><span data-stu-id="93610-134">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="93610-135">Kaynak dosya</span><span class="sxs-lookup"><span data-stu-id="93610-135">source-file</span></span> | <span data-ttu-id="93610-136">Başvuru oluşturulacak kaynak.</span><span class="sxs-lookup"><span data-stu-id="93610-136">The source to create a reference from.</span></span> <span data-ttu-id="93610-137">Bir Openapı dosyası olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="93610-137">Must be an OpenAPI file.</span></span> |<span data-ttu-id="93610-138">DotNet openapı dosya ekleme *.\Openapi.exe*</span><span class="sxs-lookup"><span data-stu-id="93610-138">dotnet openapi add file *.\OpenAPI.json*</span></span> |

### <a name="add-url"></a><span data-ttu-id="93610-139">URL ekle</span><span class="sxs-lookup"><span data-stu-id="93610-139">Add URL</span></span>

#### <a name="options"></a><span data-ttu-id="93610-140">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="93610-140">Options</span></span>

| <span data-ttu-id="93610-141">Short seçeneği</span><span class="sxs-lookup"><span data-stu-id="93610-141">Short option</span></span>| <span data-ttu-id="93610-142">Long seçeneği</span><span class="sxs-lookup"><span data-stu-id="93610-142">Long option</span></span>| <span data-ttu-id="93610-143">Açıklama</span><span class="sxs-lookup"><span data-stu-id="93610-143">Description</span></span> | <span data-ttu-id="93610-144">Örnek</span><span class="sxs-lookup"><span data-stu-id="93610-144">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="93610-145">-p</span><span class="sxs-lookup"><span data-stu-id="93610-145">-p</span></span>|<span data-ttu-id="93610-146">--updateProject</span><span class="sxs-lookup"><span data-stu-id="93610-146">--updateProject</span></span> | <span data-ttu-id="93610-147">Üzerinde çalışılacak proje.</span><span class="sxs-lookup"><span data-stu-id="93610-147">The project to operate on.</span></span> |<span data-ttu-id="93610-148">DotNet openapı Add URL *--updateproject .\Ref.exe* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="93610-148">dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="93610-149">-o</span><span class="sxs-lookup"><span data-stu-id="93610-149">-o</span></span>|<span data-ttu-id="93610-150">--Çıkış-dosya</span><span class="sxs-lookup"><span data-stu-id="93610-150">--output-file</span></span> | <span data-ttu-id="93610-151">Openapı dosyasının yerel kopyasının yerleştirileceği yer.</span><span class="sxs-lookup"><span data-stu-id="93610-151">Where to place the local copy of the OpenAPI file.</span></span> |<span data-ttu-id="93610-152">DotNet openapı URL ekleme `https://contoso.com/openapi.json` *--çıktı-dosya MyClient. JSON*</span><span class="sxs-lookup"><span data-stu-id="93610-152">dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json*</span></span> |
| <span data-ttu-id="93610-153">-c</span><span class="sxs-lookup"><span data-stu-id="93610-153">-c</span></span>|<span data-ttu-id="93610-154">--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="93610-154">--code-generator</span></span>| <span data-ttu-id="93610-155">Başvuruya uygulanacak kod Oluşturucu.</span><span class="sxs-lookup"><span data-stu-id="93610-155">The code generator to apply to the reference.</span></span> <span data-ttu-id="93610-156">Seçenekler `NSwagCSharp` ve `NSwagTypeScript`.</span><span class="sxs-lookup"><span data-stu-id="93610-156">Options are `NSwagCSharp` and `NSwagTypeScript`.</span></span> |<span data-ttu-id="93610-157">DotNet openapı Add dosyası .\Openapi.exe JSON--Code-Generator</span><span class="sxs-lookup"><span data-stu-id="93610-157">dotnet openapi add file .\OpenApi.json --code-generator</span></span>
| <span data-ttu-id="93610-158">-h</span><span class="sxs-lookup"><span data-stu-id="93610-158">-h</span></span>|<span data-ttu-id="93610-159">--yardım</span><span class="sxs-lookup"><span data-stu-id="93610-159">--help</span></span>|<span data-ttu-id="93610-160">Yardım bilgilerini göster</span><span class="sxs-lookup"><span data-stu-id="93610-160">Show help information</span></span>|<span data-ttu-id="93610-161">DotNet openapı URL ekleme--Yardım</span><span class="sxs-lookup"><span data-stu-id="93610-161">dotnet openapi add url --help</span></span>|

#### <a name="arguments"></a><span data-ttu-id="93610-162">Bağımsız Değişkenler</span><span class="sxs-lookup"><span data-stu-id="93610-162">Arguments</span></span>

|  <span data-ttu-id="93610-163">Bağımsız Değişken</span><span class="sxs-lookup"><span data-stu-id="93610-163">Argument</span></span>  | <span data-ttu-id="93610-164">Açıklama</span><span class="sxs-lookup"><span data-stu-id="93610-164">Description</span></span> | <span data-ttu-id="93610-165">Örnek</span><span class="sxs-lookup"><span data-stu-id="93610-165">Example</span></span> |
|-------------|-------------|---------|
| <span data-ttu-id="93610-166">Kaynak-URL</span><span class="sxs-lookup"><span data-stu-id="93610-166">source-URL</span></span> | <span data-ttu-id="93610-167">Başvuru oluşturulacak kaynak.</span><span class="sxs-lookup"><span data-stu-id="93610-167">The source to create a reference from.</span></span> <span data-ttu-id="93610-168">Bir URL olmalıdır.</span><span class="sxs-lookup"><span data-stu-id="93610-168">Must be a URL.</span></span> |<span data-ttu-id="93610-169">DotNet openapı URL ekleme `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="93610-169">dotnet openapi add url `https://contoso.com/openapi.json`</span></span> |

## <a name="remove"></a><span data-ttu-id="93610-170">Kaldır</span><span class="sxs-lookup"><span data-stu-id="93610-170">Remove</span></span>

<span data-ttu-id="93610-171">Verilen dosya adıyla eşleşen Openapı başvurusunu *. csproj* dosyasından kaldırır.</span><span class="sxs-lookup"><span data-stu-id="93610-171">Removes the OpenAPI reference matching the given filename from the *.csproj* file.</span></span> <span data-ttu-id="93610-172">Openapı başvurusu kaldırıldığında, istemciler oluşturulmaz.</span><span class="sxs-lookup"><span data-stu-id="93610-172">When the OpenAPI reference is removed, clients won't be generated.</span></span> <span data-ttu-id="93610-173">Local *. JSON* ve *. YAML* dosyaları silinir.</span><span class="sxs-lookup"><span data-stu-id="93610-173">Local *.json* and *.yaml* files are deleted.</span></span>

### <a name="options"></a><span data-ttu-id="93610-174">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="93610-174">Options</span></span>

| <span data-ttu-id="93610-175">Short seçeneği</span><span class="sxs-lookup"><span data-stu-id="93610-175">Short option</span></span>| <span data-ttu-id="93610-176">Long seçeneği</span><span class="sxs-lookup"><span data-stu-id="93610-176">Long option</span></span>| <span data-ttu-id="93610-177">Açıklama</span><span class="sxs-lookup"><span data-stu-id="93610-177">Description</span></span>| <span data-ttu-id="93610-178">Örnek</span><span class="sxs-lookup"><span data-stu-id="93610-178">Example</span></span> |
|-------|------|------------|---------|
| <span data-ttu-id="93610-179">-p</span><span class="sxs-lookup"><span data-stu-id="93610-179">-p</span></span>|<span data-ttu-id="93610-180">--updateProject</span><span class="sxs-lookup"><span data-stu-id="93610-180">--updateProject</span></span> | <span data-ttu-id="93610-181">Üzerinde çalışılacak proje.</span><span class="sxs-lookup"><span data-stu-id="93610-181">The project to operate on.</span></span> |<span data-ttu-id="93610-182">DotNet openapı Remove *--updateproject .\ref. csproj* .\openapi.exe</span><span class="sxs-lookup"><span data-stu-id="93610-182">dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json</span></span> |
| <span data-ttu-id="93610-183">-h</span><span class="sxs-lookup"><span data-stu-id="93610-183">-h</span></span>|<span data-ttu-id="93610-184">--yardım</span><span class="sxs-lookup"><span data-stu-id="93610-184">--help</span></span>|<span data-ttu-id="93610-185">Yardım bilgilerini göster</span><span class="sxs-lookup"><span data-stu-id="93610-185">Show help information</span></span>|<span data-ttu-id="93610-186">DotNet openapı Remove--Help</span><span class="sxs-lookup"><span data-stu-id="93610-186">dotnet openapi remove --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="93610-187">Bağımsız Değişkenler</span><span class="sxs-lookup"><span data-stu-id="93610-187">Arguments</span></span>

|  <span data-ttu-id="93610-188">Bağımsız Değişken</span><span class="sxs-lookup"><span data-stu-id="93610-188">Argument</span></span>  | <span data-ttu-id="93610-189">Açıklama</span><span class="sxs-lookup"><span data-stu-id="93610-189">Description</span></span>| <span data-ttu-id="93610-190">Örnek</span><span class="sxs-lookup"><span data-stu-id="93610-190">Example</span></span> |
| ------------|------------|---------|
| <span data-ttu-id="93610-191">Kaynak dosya</span><span class="sxs-lookup"><span data-stu-id="93610-191">source-file</span></span> | <span data-ttu-id="93610-192">Başvurunun kaldırılacağı kaynak.</span><span class="sxs-lookup"><span data-stu-id="93610-192">The source to remove the reference to.</span></span> |<span data-ttu-id="93610-193">DotNet openapı kaldırma *.\Openapi.exe*</span><span class="sxs-lookup"><span data-stu-id="93610-193">dotnet openapi remove *.\OpenAPI.json*</span></span> |

## <a name="refresh"></a><span data-ttu-id="93610-194">Yenile</span><span class="sxs-lookup"><span data-stu-id="93610-194">Refresh</span></span>

<span data-ttu-id="93610-195">İndirme URL 'sindeki en son içerik kullanılarak indirilen bir dosyanın yerel sürümünü yeniler.</span><span class="sxs-lookup"><span data-stu-id="93610-195">Refreshes the local version of a file that was downloaded using the latest content from the download URL.</span></span>

### <a name="options"></a><span data-ttu-id="93610-196">Seçenekler</span><span class="sxs-lookup"><span data-stu-id="93610-196">Options</span></span>

| <span data-ttu-id="93610-197">Short seçeneği</span><span class="sxs-lookup"><span data-stu-id="93610-197">Short option</span></span>| <span data-ttu-id="93610-198">Long seçeneği</span><span class="sxs-lookup"><span data-stu-id="93610-198">Long option</span></span>| <span data-ttu-id="93610-199">Açıklama</span><span class="sxs-lookup"><span data-stu-id="93610-199">Description</span></span> | <span data-ttu-id="93610-200">Örnek</span><span class="sxs-lookup"><span data-stu-id="93610-200">Example</span></span> |
|-------|------|-------------|---------|
| <span data-ttu-id="93610-201">-p</span><span class="sxs-lookup"><span data-stu-id="93610-201">-p</span></span>|<span data-ttu-id="93610-202">--updateProject</span><span class="sxs-lookup"><span data-stu-id="93610-202">--updateProject</span></span> | <span data-ttu-id="93610-203">Üzerinde çalışılacak proje.</span><span class="sxs-lookup"><span data-stu-id="93610-203">The project to operate on.</span></span> | <span data-ttu-id="93610-204">DotNet openapı yenilemesi *--updateproject .\Ref.exe* `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="93610-204">dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json`</span></span> |
| <span data-ttu-id="93610-205">-h</span><span class="sxs-lookup"><span data-stu-id="93610-205">-h</span></span>|<span data-ttu-id="93610-206">--yardım</span><span class="sxs-lookup"><span data-stu-id="93610-206">--help</span></span>|<span data-ttu-id="93610-207">Yardım bilgilerini göster</span><span class="sxs-lookup"><span data-stu-id="93610-207">Show help information</span></span>|<span data-ttu-id="93610-208">DotNet openapı yenilemesi--yardım</span><span class="sxs-lookup"><span data-stu-id="93610-208">dotnet openapi refresh --help</span></span>|

### <a name="arguments"></a><span data-ttu-id="93610-209">Bağımsız Değişkenler</span><span class="sxs-lookup"><span data-stu-id="93610-209">Arguments</span></span>

|  <span data-ttu-id="93610-210">Bağımsız Değişken</span><span class="sxs-lookup"><span data-stu-id="93610-210">Argument</span></span>  | <span data-ttu-id="93610-211">Açıklama</span><span class="sxs-lookup"><span data-stu-id="93610-211">Description</span></span> | <span data-ttu-id="93610-212">Örnek</span><span class="sxs-lookup"><span data-stu-id="93610-212">Example</span></span> |
| ------------|-------------|---------|
| <span data-ttu-id="93610-213">Kaynak-URL</span><span class="sxs-lookup"><span data-stu-id="93610-213">source-URL</span></span> | <span data-ttu-id="93610-214">Başvurunun yenileneceği URL.</span><span class="sxs-lookup"><span data-stu-id="93610-214">The URL to refresh the reference from.</span></span> | <span data-ttu-id="93610-215">DotNet openapı yenileme `https://contoso.com/openapi.json`</span><span class="sxs-lookup"><span data-stu-id="93610-215">dotnet openapi refresh `https://contoso.com/openapi.json`</span></span> |
