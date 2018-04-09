---
title: Swashbuckle ve ASP.NET Core kullanmaya başlama
author: zuckerthoben
description: Swagger kullanıcı arabirimini tümleştirmek için ASP.NET Core projenize Swashbuckle eklemeyi öğrenin.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: e90339f2884dd9b20cf135f879c9cab6110efecf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="09583-103">Swashbuckle ve ASP.NET Core kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="09583-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="09583-104">Tarafından [Shayne Boyer](https://twitter.com/spboyer) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="09583-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="09583-105">Swashbuckle için üç ana bileşeni vardır:</span><span class="sxs-lookup"><span data-stu-id="09583-105">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="09583-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): Swagger nesne modeli ve kullanıma sunmak için ara yazılım `SwaggerDocument` nesneleri JSON uç noktalar olarak.</span><span class="sxs-lookup"><span data-stu-id="09583-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="09583-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): derlemeleri bir Swagger oluşturucuyu `SwaggerDocument` nesneleri doğrudan rotalara, denetleyicilere ve modeller.</span><span class="sxs-lookup"><span data-stu-id="09583-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="09583-108">Bu, genellikle otomatik olarak Swagger JSON kullanıma sunmak için Swagger uç nokta ara yazılımı ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="09583-108">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="09583-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): Swagger kullanıcı Arabirimi aracını katıştırılmış bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="09583-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="09583-110">Web API işlevleri açıklamak için zengin, özelleştirilebilir bir deneyim oluşturmak için Swagger JSON yorumlar.</span><span class="sxs-lookup"><span data-stu-id="09583-110">It interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="09583-111">Genel yöntemler için yerleşik test harnesses içerir.</span><span class="sxs-lookup"><span data-stu-id="09583-111">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="09583-112">Paket yükleme</span><span class="sxs-lookup"><span data-stu-id="09583-112">Package installation</span></span>

<span data-ttu-id="09583-113">Swashbuckle ile aşağıdaki yaklaşımlardan eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="09583-113">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="09583-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09583-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="09583-115">Gelen **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="09583-115">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="09583-116">Gelen **NuGet paketlerini Yönet** iletişim:</span><span class="sxs-lookup"><span data-stu-id="09583-116">From the **Manage NuGet Packages** dialog:</span></span>

  * <span data-ttu-id="09583-117">Projenize sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="09583-117">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="09583-118">Ayarlama **paket kaynağı** "nuget.org" için</span><span class="sxs-lookup"><span data-stu-id="09583-118">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="09583-119">Arama kutusuna "Swashbuckle.AspNetCore" girin</span><span class="sxs-lookup"><span data-stu-id="09583-119">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="09583-120">"Swashbuckle.AspNetCore" paketten seçin **Gözat** sekmesinde **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="09583-120">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="09583-121">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09583-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="09583-122">Sağ *paketleri* klasöründe **çözüm paneli** > **paketleri Ekle...**</span><span class="sxs-lookup"><span data-stu-id="09583-122">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="09583-123">Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" için açılır</span><span class="sxs-lookup"><span data-stu-id="09583-123">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="09583-124">Arama kutusuna Swashbuckle.AspNetCore girin</span><span class="sxs-lookup"><span data-stu-id="09583-124">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="09583-125">Sonuçlar bölmesinde Swashbuckle.AspNetCore paketi seçin ve **Paketi Ekle**</span><span class="sxs-lookup"><span data-stu-id="09583-125">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="09583-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="09583-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="09583-127">Aşağıdaki komutu çalıştırın **tümleşik Terminal**:</span><span class="sxs-lookup"><span data-stu-id="09583-127">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="09583-128">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="09583-128">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="09583-129">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="09583-129">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="09583-130">Ekleme ve Swagger ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="09583-130">Add and configure Swagger middleware</span></span>

<span data-ttu-id="09583-131">Hizmetleri koleksiyonunu Swagger oluşturucusunu eklemek `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="09583-131">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="09583-132">Kullanmak için aşağıdaki ad alanı içe `Info` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="09583-132">Import the following namespace to use the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="09583-133">İçinde `Startup.Configure` yöntemi, oluşturulan JSON belgesini ve Swagger kullanıcı arabirimini hizmet veren ara yazılımı etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="09583-133">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="09583-134">Uygulamayı başlatın ve gidin `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="09583-134">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="09583-135">Uç noktaları açıklayan oluşturulan belge gösterildiği gibi görünüyor [Swagger belirtimi (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="09583-135">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="09583-136">Swagger kullanıcı arabirimini bulunabilir `http://localhost:<random_port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="09583-136">The Swagger UI can be found at `http://localhost:<random_port>/swagger`.</span></span> <span data-ttu-id="09583-137">Swagger kullanıcı Arabirimi API inceleyin ve diğer programları içerecek.</span><span class="sxs-lookup"><span data-stu-id="09583-137">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="09583-138">Swagger kullanıcı arabirimini uygulamanızın kök dizininde hizmet (`http://localhost:<random_port>/`) ayarlayın `RoutePrefix` boş bir dize özelliği:</span><span class="sxs-lookup"><span data-stu-id="09583-138">To serve the Swagger UI at the app's root (`http://localhost:<random_port>/`), set the `RoutePrefix` property to an empty string:</span></span>
> 
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

## <a name="customize--extend"></a><span data-ttu-id="09583-139">Özelleştirme & Genişlet</span><span class="sxs-lookup"><span data-stu-id="09583-139">Customize & extend</span></span>

<span data-ttu-id="09583-140">Swagger tema eşleştirmek için nesne modeli belgeleme ve kullanıcı arabirimini özelleştirmek için seçenekler sağlar.</span><span class="sxs-lookup"><span data-stu-id="09583-140">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="09583-141">API bilgileri ve açıklaması</span><span class="sxs-lookup"><span data-stu-id="09583-141">API info and description</span></span>

<span data-ttu-id="09583-142">Yapılandırma eylemi geçirilen `AddSwaggerGen` yöntemi yazar, lisans ve açıklaması gibi bilgileri ekler:</span><span class="sxs-lookup"><span data-stu-id="09583-142">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?range=21-40,46)]

<span data-ttu-id="09583-143">Swagger kullanıcı arabirimini sürüme ait bilgileri görüntüler:</span><span class="sxs-lookup"><span data-stu-id="09583-143">The Swagger UI displays the version's information:</span></span>

![Sürüm bilgileri ile swagger kullanıcı Arabirimi: açıklama, yazar ve daha fazla bağlantıya bakın](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="09583-145">XML açıklamaları</span><span class="sxs-lookup"><span data-stu-id="09583-145">XML comments</span></span>

<span data-ttu-id="09583-146">XML açıklamaları aşağıdaki yaklaşımlardan ile etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="09583-146">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="09583-147">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09583-147">Visual Studio</span></span>](#tab/visual-studio-xml/)
* <span data-ttu-id="09583-148">' Nde projeye sağ **Çözüm Gezgini** seçip **özellikleri**</span><span class="sxs-lookup"><span data-stu-id="09583-148">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="09583-149">Denetleyin **XML belge dosyası** altında kutusunda **çıkış** bölümünü **yapı** sekmesi:</span><span class="sxs-lookup"><span data-stu-id="09583-149">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Proje Özellikleri'nin sekmesi oluştur](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

#### <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="09583-151">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="09583-151">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml/)
* <span data-ttu-id="09583-152">Açık **proje seçenekleri** iletişim > **yapı** > **derleyici**</span><span class="sxs-lookup"><span data-stu-id="09583-152">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="09583-153">Denetleme **xml belgeleri oluşturmak** altında kutusunda **Genel Seçenekler** bölümü:</span><span class="sxs-lookup"><span data-stu-id="09583-153">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Proje seçenekleri Genel Seçenekler bölümünde](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

#### <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="09583-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="09583-155">Visual Studio Code</span></span>](#tab/visual-studio-code-xml/)
<span data-ttu-id="09583-156">El ile eklemek için aşağıdaki kod parçacığını *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="09583-156">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=2)]

* * *
<span data-ttu-id="09583-157">XML açıklamaları etkinleştirme belgelenmemiş genel türleri ve üyeleri için hata ayıklama bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="09583-157">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="09583-158">Belgelenmemiş türleri ve üyeleri tarafından uyarı iletisi gösterilir.</span><span class="sxs-lookup"><span data-stu-id="09583-158">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="09583-159">Örneğin, aşağıdaki ileti uyarı kod 1591 ihlal gösterir:</span><span class="sxs-lookup"><span data-stu-id="09583-159">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="09583-160">Noktalı virgülle ayrılmış olarak yoksaymak için uyarı kodlarının listesini tanımlayarak uyarıları bastırma *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="09583-160">Suppress warnings by defining a semicolon-delimited list of warning codes to ignore in the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

<span data-ttu-id="09583-161">Oluşturulan XML dosyasını kullanmak için Swagger yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="09583-161">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="09583-162">Linux veya Windows olmayan işletim sistemleri için dosya adlarını ve yollarını büyük küçük harfe duyarlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="09583-162">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="09583-163">Örneğin, bir *TodoApi.XML* Windows ancak değil CentOS dosyanın geçerli olduğu.</span><span class="sxs-lookup"><span data-stu-id="09583-163">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=29-31)]

<span data-ttu-id="09583-164">Önceki kod [yansıma](/dotnet/csharp/programming-guide/concepts/reflection) , Web API projesi eşleşen bir XML dosya adı oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="09583-164">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the Web API project.</span></span> <span data-ttu-id="09583-165">Bu yaklaşım, oluşturulan XML dosya adı proje adının eşleşmesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="09583-165">This approach ensures that the generated XML file name matches the project name.</span></span> <span data-ttu-id="09583-166">[AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) özelliği XML dosyasının yolunu oluşturmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="09583-166">The [AppContext.BaseDirectory](/dotnet/api/system.appcontext.basedirectory#System_AppContext_BaseDirectory) property is used to construct a path to the XML file.</span></span>

<span data-ttu-id="09583-167">Bir eylem Üçlü eğik çizgi açıklama ekleme, Swagger kullanıcı arabirimini bölüm başlığı açıklama ekleyerek geliştirir.</span><span class="sxs-lookup"><span data-stu-id="09583-167">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="09583-168">Ekleme bir [ \<Özet >](/dotnet/csharp/programming-guide/xmldoc/summary) öğesi yukarıdaki `Delete` eylem:</span><span class="sxs-lookup"><span data-stu-id="09583-168">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="09583-169">Önceki kodun iç metni Swagger kullanıcı arabirimini görüntüler `<summary>` öğe:</span><span class="sxs-lookup"><span data-stu-id="09583-169">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Swagger kullanıcı Arabirimi 'Belirli bir Todoıtem siler.' XML açıklama gösterme](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="09583-172">Kullanıcı Arabirimi tarafından oluşturulan JSON şeması yönetilir:</span><span class="sxs-lookup"><span data-stu-id="09583-172">The UI is driven by the generated JSON schema:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="09583-173">Ekleme bir [ \<açıklamalar >](/dotnet/csharp/programming-guide/xmldoc/remarks) öğesine `Create` eylem yöntemi belgeleri.</span><span class="sxs-lookup"><span data-stu-id="09583-173">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="09583-174">Belirtilen bilgilerini tamamlayan `<summary>` öğesi ve daha güçlü bir Swagger kullanıcı Arabirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="09583-174">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="09583-175">`<remarks>` Metin, JSON veya XML öğesi içeriği oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="09583-175">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="09583-176">Bu ek açıklamalar UI geliştirmeleriyle dikkat edin:</span><span class="sxs-lookup"><span data-stu-id="09583-176">Notice the UI enhancements with these additional comments:</span></span>

![Swagger kullanıcı Arabirimi ile gösterilen ek açıklamaları](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="09583-178">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="09583-178">Data annotations</span></span>

<span data-ttu-id="09583-179">Modelin bulunan özniteliklerle tasarlamanız [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) Swagger kullanıcı Arabirimi bileşenlerini sürücü yardımcı olması için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="09583-179">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="09583-180">Ekleme `[Required]` özniteliğini `Name` özelliği `TodoItem` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="09583-180">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="09583-181">Bu öznitelik varlığını UI davranışını değiştiren ve temel JSON şeması değiştirir:</span><span class="sxs-lookup"><span data-stu-id="09583-181">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="09583-182">Ekleme `[Produces("application/json")]` özniteliği API denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="09583-182">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="09583-183">Amacı denetleyicinin Eylemler bir yanıt içerik türü desteği bildirmektir *uygulama/json*:</span><span class="sxs-lookup"><span data-stu-id="09583-183">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="09583-184">**Yanıt içerik türü** açılan denetleyicinin GET eylemler için varsayılan olarak bu içerik türü seçer:</span><span class="sxs-lookup"><span data-stu-id="09583-184">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Swagger kullanıcı Arabirimi ile varsayılan yanıt içerik türü](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="09583-186">Web API içinde veri ek açıklamaları kullanımını arttıkça, kullanıcı Arabirimi ve API daha açıklayıcı ve kullanışlı hale sayfaları yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="09583-186">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="09583-187">Yanıt türleri açıklanmaktadır</span><span class="sxs-lookup"><span data-stu-id="09583-187">Describe response types</span></span>

<span data-ttu-id="09583-188">Süren geliştiricilerin ne döndürülen ile en ilgili&mdash;özellikle yanıt türleri ve hata kodları (değil standart değilse).</span><span class="sxs-lookup"><span data-stu-id="09583-188">Consuming developers are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="09583-189">XML açıklamaları ve veri ek açıklamaları içinde yanıt türleri ve hata kodları gösterilir.</span><span class="sxs-lookup"><span data-stu-id="09583-189">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="09583-190">`Create` Eylem başarı bir HTTP 201 durum kodunu döndürür.</span><span class="sxs-lookup"><span data-stu-id="09583-190">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="09583-191">Gönderilen istek gövdesi null olduğunda bir HTTP 400 durum kodu döndürülür.</span><span class="sxs-lookup"><span data-stu-id="09583-191">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="09583-192">Swagger kullanıcı arabirimini uygun belgelerinde tüketici bu beklenen sonuçları bilgisi eksik.</span><span class="sxs-lookup"><span data-stu-id="09583-192">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="09583-193">Aşağıdaki örnekte vurgulanmış satırlarını ekleyerek bu sorunu düzeltin:</span><span class="sxs-lookup"><span data-stu-id="09583-193">Fix that problem by adding the highlighted lines in the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="09583-194">Swagger kullanıcı Arabirimi şimdi beklenen HTTP yanıt kodları açıkça belgeler:</span><span class="sxs-lookup"><span data-stu-id="09583-194">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Swagger kullanıcı Arabirimi 'yeni oluşturulan Yapılacaklar öğesi döndürür' POST yanıt sınıf tanımına gösteren ve '400 - öğe null ise' durum kodunu ve yanıt iletileri altında nedeni](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="09583-196">Kullanıcı arabirimini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="09583-196">Customize the UI</span></span>

<span data-ttu-id="09583-197">UI stock işlevsel ve presentable ' dir.</span><span class="sxs-lookup"><span data-stu-id="09583-197">The stock UI is both functional and presentable.</span></span> <span data-ttu-id="09583-198">Ancak, API belgelerine sayfaları marka veya temayı temsil etmelidir.</span><span class="sxs-lookup"><span data-stu-id="09583-198">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="09583-199">Swashbuckle bileşenleri markalama statik dosyaları işleme için kaynakları ekleme ve bu dosyaları barındırmak için klasör yapısı oluşturulmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="09583-199">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="09583-200">.NET Framework veya .NET hedefleme varsa 1.x çekirdek, ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet paketini projeye:</span><span class="sxs-lookup"><span data-stu-id="09583-200">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="09583-201">Önceki NuGet paketi zaten .NET Core hedefleme yüklü olsa 2.x ve kullanarak [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="09583-201">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="09583-202">Statik dosya ara yazılımlarını etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="09583-202">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="09583-203">İçeriği alma *dağ* klasöründen [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span><span class="sxs-lookup"><span data-stu-id="09583-203">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/master/dist).</span></span> <span data-ttu-id="09583-204">Bu klasör Swagger kullanıcı Arabirimi sayfası için gerekli varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="09583-204">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="09583-205">Oluşturma bir *wwwroot/swagger/UI* klasörünü ve içeriğini buraya kopyalayın *dağ* klasör.</span><span class="sxs-lookup"><span data-stu-id="09583-205">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="09583-206">Oluşturma bir *custom.css* dosyasında *wwwroot/swagger/UI*, sayfa üstbilgisi özelleştirmek için aşağıdaki CSS ile:</span><span class="sxs-lookup"><span data-stu-id="09583-206">Create a *custom.css* file, in *wwwroot/swagger/ui*, with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/custom.css)]

<span data-ttu-id="09583-207">Başvuru *custom.css* içinde *index.html* dosya, diğer bir CSS dosyaları sonra:</span><span class="sxs-lookup"><span data-stu-id="09583-207">Reference *custom.css* in the *index.html* file, after any other CSS files:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/samples/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?name=snippet_SwaggerUiCss&highlight=3)]

<span data-ttu-id="09583-208">Gözat *index.html* adresindeki sayfasında `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="09583-208">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="09583-209">Girin `http://localhost:<random_port>/swagger/v1/swagger.json` üst bilginin textbox ve tıklatın **Araştır** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="09583-209">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="09583-210">Sonuçta elde edilen sayfanın şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="09583-210">The resulting page looks as follows:</span></span>

![Özel üstbilgi başlık swagger kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="09583-212">Çok daha fazlasını sayfasıyla yapabilirsiniz yoktur.</span><span class="sxs-lookup"><span data-stu-id="09583-212">There's much more you can do with the page.</span></span> <span data-ttu-id="09583-213">UI kaynakları tam özelliklerine bakın [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="09583-213">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
