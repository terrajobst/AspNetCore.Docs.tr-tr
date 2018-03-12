---
title: "Swashbuckle ile çalışmaya başlama"
author: zuckerthoben
description: "Bu öğretici Swashbuckle Swagger kullanıcı arabirimini tümleştirmek için projenize ekleme bir kılavuz sağlar."
keywords: "ASP.NET Core, Swagger, Swashbuckle, sayfalar, Web API Yardım"
ms.author: scaddie
manager: wpickett
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: 705fbe7c3aa65fed89f5fda02af3cb8dd757b71c
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/12/2018
---
# <a name="get-started-with-swashbuckle"></a><span data-ttu-id="ce05b-104">Swashbuckle ile çalışmaya başlama</span><span class="sxs-lookup"><span data-stu-id="ce05b-104">Get started with Swashbuckle</span></span>

<span data-ttu-id="ce05b-105">Tarafından [Shayne Boyer](https://twitter.com/spboyer) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="ce05b-105">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="ce05b-106">Swashbuckle için üç ana bileşeni vardır:</span><span class="sxs-lookup"><span data-stu-id="ce05b-106">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="ce05b-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): Swagger nesne modeli ve kullanıma sunmak için ara yazılım `SwaggerDocument` nesneleri JSON uç noktalar olarak.</span><span class="sxs-lookup"><span data-stu-id="ce05b-107">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="ce05b-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): derlemeleri bir Swagger oluşturucuyu `SwaggerDocument` nesneleri doğrudan rotalara, denetleyicilere ve modeller.</span><span class="sxs-lookup"><span data-stu-id="ce05b-108">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="ce05b-109">Bu, genellikle otomatik olarak Swagger JSON kullanıma sunmak için Swagger uç nokta ara yazılımı ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="ce05b-109">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="ce05b-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): Web API işlevleri açıklamak için zengin, özelleştirilebilir bir deneyim oluşturmak için Swagger JSON yorumlar Swagger kullanıcı Arabirimi aracını katıştırılmış bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="ce05b-110">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="ce05b-111">Genel yöntemler için yerleşik test harnesses içerir.</span><span class="sxs-lookup"><span data-stu-id="ce05b-111">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="ce05b-112">Paket yükleme</span><span class="sxs-lookup"><span data-stu-id="ce05b-112">Package installation</span></span>

<span data-ttu-id="ce05b-113">Swashbuckle ile aşağıdaki yaklaşımlardan eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="ce05b-113">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="ce05b-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ce05b-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ce05b-115">Gelen **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="ce05b-115">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="ce05b-116">Gelen **NuGet paketlerini Yönet** iletişim:</span><span class="sxs-lookup"><span data-stu-id="ce05b-116">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="ce05b-117">Projenize sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="ce05b-117">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="ce05b-118">Ayarlama **paket kaynağı** "nuget.org" için</span><span class="sxs-lookup"><span data-stu-id="ce05b-118">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="ce05b-119">Arama kutusuna "Swashbuckle.AspNetCore" girin</span><span class="sxs-lookup"><span data-stu-id="ce05b-119">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="ce05b-120">"Swashbuckle.AspNetCore" paketten seçin **Gözat** sekmesinde **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="ce05b-120">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="ce05b-121">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ce05b-121">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ce05b-122">Sağ *paketleri* klasöründe **çözüm paneli** > **paketleri Ekle...**</span><span class="sxs-lookup"><span data-stu-id="ce05b-122">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="ce05b-123">Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" için açılır</span><span class="sxs-lookup"><span data-stu-id="ce05b-123">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="ce05b-124">Arama kutusuna Swashbuckle.AspNetCore girin</span><span class="sxs-lookup"><span data-stu-id="ce05b-124">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="ce05b-125">Sonuçlar bölmesinde Swashbuckle.AspNetCore paketi seçin ve **Paketi Ekle**</span><span class="sxs-lookup"><span data-stu-id="ce05b-125">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="ce05b-126">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ce05b-126">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="ce05b-127">Aşağıdaki komutu çalıştırın **tümleşik Terminal**:</span><span class="sxs-lookup"><span data-stu-id="ce05b-127">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.Swashbuckle.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="ce05b-128">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="ce05b-128">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="ce05b-129">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="ce05b-129">Run the following command:</span></span>

```console
dotnet add TodoApi.Swashbuckle.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="ce05b-130">Ekleme ve Swagger ara yazılımını yapılandırma</span><span class="sxs-lookup"><span data-stu-id="ce05b-130">Add and configure Swagger middleware</span></span>

<span data-ttu-id="ce05b-131">Hizmetleri koleksiyonunu Swagger oluşturucusunu eklemek `Startup.ConfigureServices` yöntemi:</span><span class="sxs-lookup"><span data-stu-id="ce05b-131">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="ce05b-132">Aşağıdaki ad alanlarında alma `Info` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ce05b-132">Import the following namespaces in the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="ce05b-133">İçinde `Startup.Configure` yöntemi, oluşturulan JSON belgesini ve Swagger kullanıcı arabirimini hizmet veren ara yazılımı etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="ce05b-133">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="ce05b-134">Uygulamayı başlatın ve gidin `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="ce05b-134">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="ce05b-135">Uç noktaları açıklayan oluşturulan belge gösterildiği gibi görünüyor [Swagger belirtimi (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span><span class="sxs-lookup"><span data-stu-id="ce05b-135">The generated document describing the endpoints appears as shown in [Swagger specification (swagger.json)](xref:tutorials/web-api-help-pages-using-swagger#swagger-specification-swaggerjson).</span></span>

<span data-ttu-id="ce05b-136">Swagger kullanıcı arabirimini bulunabilir `http://localhost:<random_port>/swagger`.</span><span class="sxs-lookup"><span data-stu-id="ce05b-136">The Swagger UI can be found at `http://localhost:<random_port>/swagger`.</span></span> <span data-ttu-id="ce05b-137">Şimdi API Swagger kullanıcı arabirimini keşfetme ve diğer programları içerecek.</span><span class="sxs-lookup"><span data-stu-id="ce05b-137">Now you can explore the API via Swagger UI and incorporate it in other programs.</span></span>

## <a name="customize--extend"></a><span data-ttu-id="ce05b-138">Özelleştirme & Genişlet</span><span class="sxs-lookup"><span data-stu-id="ce05b-138">Customize & extend</span></span>

<span data-ttu-id="ce05b-139">Swagger tema eşleştirmek için nesne modeli belgeleme ve kullanıcı arabirimini özelleştirmek için seçenekler sağlar.</span><span class="sxs-lookup"><span data-stu-id="ce05b-139">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="ce05b-140">API bilgileri ve açıklaması</span><span class="sxs-lookup"><span data-stu-id="ce05b-140">API info and description</span></span>

<span data-ttu-id="ce05b-141">Yapılandırma eylemi geçirilen `AddSwaggerGen` yöntemi, yazar, lisans ve açıklaması gibi bilgileri eklemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="ce05b-141">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?range=20-30,36)]

<span data-ttu-id="ce05b-142">Aşağıdaki resimde Swagger sürüm bilgilerini görüntüleme UI gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="ce05b-142">The following image depicts the Swagger UI displaying the version information:</span></span>

![Sürüm bilgileri ile swagger kullanıcı Arabirimi: açıklama, yazar ve daha fazla bağlantıya bakın](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="ce05b-144">XML açıklamaları</span><span class="sxs-lookup"><span data-stu-id="ce05b-144">XML comments</span></span>

<span data-ttu-id="ce05b-145">XML açıklamaları aşağıdaki yaklaşımlardan ile etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="ce05b-145">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio-xml"></a>[<span data-ttu-id="ce05b-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ce05b-146">Visual Studio</span></span>](#tab/visual-studio-xml)

* <span data-ttu-id="ce05b-147">' Nde projeye sağ **Çözüm Gezgini** seçip **özellikleri**</span><span class="sxs-lookup"><span data-stu-id="ce05b-147">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="ce05b-148">Denetleyin **XML belge dosyası** altında kutusunda **çıkış** bölümünü **yapı** sekmesi:</span><span class="sxs-lookup"><span data-stu-id="ce05b-148">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Proje Özellikleri'nin sekmesi oluştur](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac-xml"></a>[<span data-ttu-id="ce05b-150">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ce05b-150">Visual Studio for Mac</span></span>](#tab/visual-studio-mac-xml)

* <span data-ttu-id="ce05b-151">Açık **proje seçenekleri** iletişim > **yapı** > **derleyici**</span><span class="sxs-lookup"><span data-stu-id="ce05b-151">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="ce05b-152">Denetleme **xml belgeleri oluşturmak** altında kutusunda **Genel Seçenekler** bölümü:</span><span class="sxs-lookup"><span data-stu-id="ce05b-152">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Proje seçenekleri Genel Seçenekler bölümünde](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code-xml"></a>[<span data-ttu-id="ce05b-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ce05b-154">Visual Studio Code</span></span>](#tab/visual-studio-code-xml)

<span data-ttu-id="ce05b-155">El ile eklemek için aşağıdaki kod parçacığını *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ce05b-155">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/TodoApiSwashbuckle.csproj?range=7-9)]

---

<span data-ttu-id="ce05b-156">Oluşturulan XML dosyasını kullanmak için Swagger yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="ce05b-156">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="ce05b-157">Linux veya Windows olmayan işletim sistemleri için dosya adlarını ve yollarını büyük küçük harfe duyarlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="ce05b-157">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="ce05b-158">Örneğin, bir *TodoApi.Swashbuckle.XML* Windows ancak değil CentOS dosyanın geçerli olduğu.</span><span class="sxs-lookup"><span data-stu-id="ce05b-158">For example, a *TodoApi.Swashbuckle.XML* file is valid on Windows but not CentOS.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="ce05b-159">Önceki kod `ApplicationBasePath` uygulamanın taban yolu alır.</span><span class="sxs-lookup"><span data-stu-id="ce05b-159">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="ce05b-160">Taban yol XML açıklamaları dosyayı bulmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="ce05b-160">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="ce05b-161">*TodoApi.Swashbuckle.xml* dosya uygulama adına dayalı oluşturulan XML adını yorumları Bu örnekte, yalnızca çalışır.</span><span class="sxs-lookup"><span data-stu-id="ce05b-161">*TodoApi.Swashbuckle.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="ce05b-162">Yöntem Üçlü eğik çizgi açıklamaları ekleme, Swagger kullanıcı arabirimini bölüm başlığı açıklama ekleyerek geliştirir:</span><span class="sxs-lookup"><span data-stu-id="ce05b-162">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Swagger kullanıcı Arabirimi 'Belirli bir Todoıtem siler.' XML açıklama gösterme](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="ce05b-165">Kullanıcı arabirimini de bu açıklamalar içeren oluşturulan JSON dosya tarafından yönetilir:</span><span class="sxs-lookup"><span data-stu-id="ce05b-165">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

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

<span data-ttu-id="ce05b-166">Ekleme bir [ \<açıklamalar >](/dotnet/csharp/programming-guide/xmldoc/remarks) etiketini `Create` eylem yöntemi belgeleri.</span><span class="sxs-lookup"><span data-stu-id="ce05b-166">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="ce05b-167">Belirtilen bilgilerini tamamlayan [ \<Özet >](/dotnet/csharp/programming-guide/xmldoc/summary) etiketi ve daha güçlü bir Swagger kullanıcı Arabirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="ce05b-167">It supplements information specified in the [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="ce05b-168">`<remarks>` Metin, JSON veya XML etiket içeriği oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="ce05b-168">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="ce05b-169">Bu ek açıklamalar UI geliştirmeleriyle dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="ce05b-169">Notice the UI enhancements with these additional comments.</span></span>

![Swagger kullanıcı Arabirimi ile gösterilen ek açıklamaları](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="ce05b-171">Veri açıklamaları</span><span class="sxs-lookup"><span data-stu-id="ce05b-171">Data annotations</span></span>

<span data-ttu-id="ce05b-172">Modelin bulunan özniteliklerle tasarlamanız [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) Swagger kullanıcı Arabirimi bileşenlerini sürücü yardımcı olması için ad alanı.</span><span class="sxs-lookup"><span data-stu-id="ce05b-172">Decorate the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="ce05b-173">Ekleme `[Required]` özniteliğini `Name` özelliği `TodoItem` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="ce05b-173">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="ce05b-174">Bu öznitelik varlığını UI davranışını değiştiren ve temel JSON şeması değiştirir:</span><span class="sxs-lookup"><span data-stu-id="ce05b-174">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="ce05b-175">Ekleme `[Produces("application/json")]` özniteliği API denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="ce05b-175">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="ce05b-176">Amacı denetleyicinin Eylemler bir yanıt içerik türü desteği bildirmektir *uygulama/json*:</span><span class="sxs-lookup"><span data-stu-id="ce05b-176">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="ce05b-177">**Yanıt içerik türü** açılan denetleyicinin GET eylemler için varsayılan olarak bu içerik türü seçer:</span><span class="sxs-lookup"><span data-stu-id="ce05b-177">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Swagger kullanıcı Arabirimi ile varsayılan yanıt içerik türü](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="ce05b-179">Web API içinde veri ek açıklamaları kullanımını arttıkça, kullanıcı Arabirimi ve API daha açıklayıcı ve kullanışlı hale sayfaları yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="ce05b-179">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="ce05b-180">Yanıt türleri açıklanmaktadır</span><span class="sxs-lookup"><span data-stu-id="ce05b-180">Describe response types</span></span>

<span data-ttu-id="ce05b-181">Süren geliştiricilerin ne döndürülen ile en ilgili&mdash;özellikle yanıt türleri ve hata kodları (değil standart değilse).</span><span class="sxs-lookup"><span data-stu-id="ce05b-181">Consuming developers are most concerned with what is returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="ce05b-182">Bunlar XML açıklamaları ve veri ek açıklamaları işlenir.</span><span class="sxs-lookup"><span data-stu-id="ce05b-182">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="ce05b-183">`Create` Eylem döndürür `201 Created` başarı veya `400 Bad Request` gönderilen istek gövdesi olduğunda null.</span><span class="sxs-lookup"><span data-stu-id="ce05b-183">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="ce05b-184">Swagger kullanıcı arabirimini uygun belgelerinde tüketici bu beklenen sonuçları bilgisi eksik.</span><span class="sxs-lookup"><span data-stu-id="ce05b-184">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="ce05b-185">Aşağıdaki örnekte vurgulanmış satırlarını ekleyerek bu sorun düzeltilene:</span><span class="sxs-lookup"><span data-stu-id="ce05b-185">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="ce05b-186">Swagger kullanıcı Arabirimi şimdi beklenen HTTP yanıt kodları açıkça belgeler:</span><span class="sxs-lookup"><span data-stu-id="ce05b-186">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Swagger kullanıcı Arabirimi 'yeni oluşturulan Yapılacaklar öğesi döndürür' POST yanıt sınıf tanımına gösteren ve '400 - öğe null ise' durum kodunu ve yanıt iletileri altında nedeni](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customize-the-ui"></a><span data-ttu-id="ce05b-188">Kullanıcı arabirimini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="ce05b-188">Customize the UI</span></span>

<span data-ttu-id="ce05b-189">UI stock işlevsel ve presentable; Ancak, belge sayfalarının API'nize oluştururken, marka veya temayı temsil etmek için istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="ce05b-189">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="ce05b-190">Swashbuckle bileşenleri ile bu görevi gerçekleştirmeye statik dosyaları işleme için kaynakları ekleme ve bu dosyaları barındırmak için klasör yapısı oluşturulmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="ce05b-190">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="ce05b-191">.NET Framework veya .NET hedefleme varsa 1.x çekirdek, ekleme [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet paketini projeye:</span><span class="sxs-lookup"><span data-stu-id="ce05b-191">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="ce05b-192">Önceki NuGet paketi zaten .NET Core hedefleme yüklü olsa 2.x ve kullanarak [metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="ce05b-192">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="ce05b-193">Statik dosya ara yazılımlarını etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="ce05b-193">Enable the static files middleware:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="ce05b-194">İçeriği alma *dağ* klasöründen [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="ce05b-194">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="ce05b-195">Bu klasör Swagger kullanıcı Arabirimi sayfası için gerekli varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="ce05b-195">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="ce05b-196">Oluşturma bir *wwwroot/swagger/UI* klasörünü ve içeriğini buraya kopyalayın *dağ* klasör.</span><span class="sxs-lookup"><span data-stu-id="ce05b-196">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="ce05b-197">Oluşturma bir *wwwroot/swagger/ui/css/custom.css* sayfa üstbilgisi özelleştirmek için aşağıdaki CSS dosyasıyla:</span><span class="sxs-lookup"><span data-stu-id="ce05b-197">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="ce05b-198">Başvuru *custom.css* içinde *index.html* dosyası:</span><span class="sxs-lookup"><span data-stu-id="ce05b-198">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi.Swashbuckle/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="ce05b-199">Gözat *index.html* adresindeki sayfasında `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="ce05b-199">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="ce05b-200">Girin `http://localhost:<random_port>/swagger/v1/swagger.json` üst bilginin textbox ve tıklatın **Araştır** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="ce05b-200">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="ce05b-201">Sonuçta elde edilen sayfanın şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="ce05b-201">The resulting page looks as follows:</span></span>

![Özel üstbilgi başlık swagger kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="ce05b-203">Çok daha fazlasını sayfasıyla yapabilirsiniz yoktur.</span><span class="sxs-lookup"><span data-stu-id="ce05b-203">There's much more you can do with the page.</span></span> <span data-ttu-id="ce05b-204">UI kaynakları tam özelliklerine bakın [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="ce05b-204">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
