---
title: "ASP.NET Core Web API Yardım Swagger kullanarak sayfaları"
author: spboyer
description: "Bu öğretici belgeleri oluşturmak ve bir Web API uygulaması için sayfa yardımcı olmak için Swagger ekleme bir kılavuz sağlar."
manager: wpickett
ms.author: spboyer
ms.date: 09/01/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 911504d9472ae78a0d1d002f1feb57f3a160d5bf
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-core-web-api-help-pages-using-swagger"></a><span data-ttu-id="de515-103">ASP.NET Core Web API Yardım Swagger kullanarak sayfaları</span><span class="sxs-lookup"><span data-stu-id="de515-103">ASP.NET Core Web API Help Pages using Swagger</span></span>

<a name="web-api-help-pages-using-swagger"></a>

<span data-ttu-id="de515-104">Tarafından [Shayne Boyer](https://twitter.com/spboyer) ve [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="de515-104">By [Shayne Boyer](https://twitter.com/spboyer) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="de515-105">Bir API çeşitli yöntemleri anlama kullanıcı bir uygulama oluştururken geliştiriciler için bir sınama olabilir.</span><span class="sxs-lookup"><span data-stu-id="de515-105">Understanding the various methods of an API can be a challenge for a developer when building a consuming application.</span></span>

<span data-ttu-id="de515-106">Web API için iyi belgeleri ve Yardım sayfaları oluşturma, kullanarak [Swagger](https://swagger.io/) .NET Core uygulamasıyla [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), birkaç NuGet paketleri ekleme ve değiştirme olarak kadar kolaydır *haline*.</span><span class="sxs-lookup"><span data-stu-id="de515-106">Generating good documentation and help pages for your Web API, using [Swagger](https://swagger.io/) with the .NET Core implementation [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore), is as easy as adding a couple of NuGet packages and modifying the *Startup.cs*.</span></span>

* <span data-ttu-id="de515-107">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) ASP.NET çekirdek Web API için Swagger belgeleri oluşturmak için bir açık kaynaklı proje.</span><span class="sxs-lookup"><span data-stu-id="de515-107">[Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="de515-108">[Swagger](https://swagger.io/) etkileşimli belgeler, istemci SDK oluşturma ve bulunabilirlik için destek sağlayan bir RESTful API'si makine tarafından okunabilir bir gösterimidir.</span><span class="sxs-lookup"><span data-stu-id="de515-108">[Swagger](https://swagger.io/) is a machine-readable representation of a RESTful API that enables support for interactive documentation, client SDK generation, and discoverability.</span></span>

<span data-ttu-id="de515-109">Bu öğretici örneğe bağlı derlemeler [yapı bilgisayarınızı ilk Web API ile ASP.NET Core MVC ve Visual Studio](xref:tutorials/first-web-api).</span><span class="sxs-lookup"><span data-stu-id="de515-109">This tutorial builds on the sample on [Building Your First Web API with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="de515-110">Örnek: izlemek istiyorsanız, indirme [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span><span class="sxs-lookup"><span data-stu-id="de515-110">If you'd like to follow along, download the sample at [https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample).</span></span>

## <a name="getting-started"></a><span data-ttu-id="de515-111">Başlarken</span><span class="sxs-lookup"><span data-stu-id="de515-111">Getting Started</span></span>

<span data-ttu-id="de515-112">Swashbuckle için üç ana bileşeni vardır:</span><span class="sxs-lookup"><span data-stu-id="de515-112">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="de515-113">`Swashbuckle.AspNetCore.Swagger`: bir Swagger nesne modeli ve kullanıma sunmak için ara yazılım `SwaggerDocument` nesneleri JSON uç noktalar olarak.</span><span class="sxs-lookup"><span data-stu-id="de515-113">`Swashbuckle.AspNetCore.Swagger`: a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="de515-114">`Swashbuckle.AspNetCore.SwaggerGen`: derlemeleri bir Swagger oluşturucuyu `SwaggerDocument` nesneleri doğrudan rotalara, denetleyicilere ve modeller.</span><span class="sxs-lookup"><span data-stu-id="de515-114">`Swashbuckle.AspNetCore.SwaggerGen`: a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="de515-115">Bu, genellikle otomatik olarak Swagger JSON kullanıma sunmak için Swagger uç nokta ara yazılımı ile birleştirilir.</span><span class="sxs-lookup"><span data-stu-id="de515-115">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="de515-116">`Swashbuckle.AspNetCore.SwaggerUI`: Web API işlevleri açıklamak için zengin, özelleştirilebilir bir deneyim oluşturmak için Swagger JSON yorumlar Swagger kullanıcı Arabirimi aracını katıştırılmış bir sürümü.</span><span class="sxs-lookup"><span data-stu-id="de515-116">`Swashbuckle.AspNetCore.SwaggerUI`: an embedded version of the Swagger UI tool which interprets Swagger JSON to build a rich, customizable experience for describing the Web API functionality.</span></span> <span data-ttu-id="de515-117">Genel yöntemler için yerleşik test harnesses içerir.</span><span class="sxs-lookup"><span data-stu-id="de515-117">It includes built-in test harnesses for the public methods.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="de515-118">NuGet paketleri</span><span class="sxs-lookup"><span data-stu-id="de515-118">NuGet Packages</span></span>

<span data-ttu-id="de515-119">Swashbuckle ile aşağıdaki yaklaşımlardan eklenebilir:</span><span class="sxs-lookup"><span data-stu-id="de515-119">Swashbuckle can be added with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de515-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de515-120">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de515-121">Gelen **Paket Yöneticisi Konsolu** penceresi:</span><span class="sxs-lookup"><span data-stu-id="de515-121">From the **Package Manager Console** window:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore
    ```

* <span data-ttu-id="de515-122">Gelen **NuGet paketlerini Yönet** iletişim:</span><span class="sxs-lookup"><span data-stu-id="de515-122">From the **Manage NuGet Packages** dialog:</span></span>

     * <span data-ttu-id="de515-123">Projenize sağ **Çözüm Gezgini** > **NuGet paketlerini Yönet**</span><span class="sxs-lookup"><span data-stu-id="de515-123">Right-click your project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
     * <span data-ttu-id="de515-124">Ayarlama **paket kaynağı** "nuget.org" için</span><span class="sxs-lookup"><span data-stu-id="de515-124">Set the **Package source** to "nuget.org"</span></span>
     * <span data-ttu-id="de515-125">Arama kutusuna "Swashbuckle.AspNetCore" girin</span><span class="sxs-lookup"><span data-stu-id="de515-125">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
     * <span data-ttu-id="de515-126">"Swashbuckle.AspNetCore" paketten seçin **Gözat** sekmesinde **yükleyin**</span><span class="sxs-lookup"><span data-stu-id="de515-126">Select the "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="de515-127">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de515-127">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="de515-128">Sağ *paketleri* klasöründe **çözüm paneli** > **paketleri Ekle...**</span><span class="sxs-lookup"><span data-stu-id="de515-128">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="de515-129">Ayarlama **paketleri Ekle** pencerenin **kaynak** "nuget.org" için açılır</span><span class="sxs-lookup"><span data-stu-id="de515-129">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="de515-130">Arama kutusuna Swashbuckle.AspNetCore girin</span><span class="sxs-lookup"><span data-stu-id="de515-130">Enter Swashbuckle.AspNetCore in the search box</span></span>
* <span data-ttu-id="de515-131">Sonuçlar bölmesinde Swashbuckle.AspNetCore paketi seçin ve **Paketi Ekle**</span><span class="sxs-lookup"><span data-stu-id="de515-131">Select the Swashbuckle.AspNetCore package from the results pane and click **Add Package**</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de515-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de515-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="de515-133">Aşağıdaki komutu çalıştırın **tümleşik Terminal**:</span><span class="sxs-lookup"><span data-stu-id="de515-133">Run the following command from the **Integrated Terminal**:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="de515-134">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="de515-134">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="de515-135">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="de515-135">Run the following command:</span></span>

```console
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore
```

---

## <a name="add-and-configure-swagger-to-the-middleware"></a><span data-ttu-id="de515-136">Ekleme ve Swagger ara yazılım için yapılandırma</span><span class="sxs-lookup"><span data-stu-id="de515-136">Add and configure Swagger to the middleware</span></span>

<span data-ttu-id="de515-137">Hizmetleri koleksiyonunu Swagger oluşturucusunu eklemek `ConfigureServices` yöntemi *haline*:</span><span class="sxs-lookup"><span data-stu-id="de515-137">Add the Swagger generator to the services collection in the `ConfigureServices` method of *Startup.cs*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_ConfigureServices&highlight=7-10)]

<span data-ttu-id="de515-138">Aşağıdaki ekleme deyimini kullanarak `Info` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="de515-138">Add the following using statement for the `Info` class:</span></span>

```csharp
using Swashbuckle.AspNetCore.Swagger;
```

<span data-ttu-id="de515-139">İçinde `Configure` yöntemi *haline*, oluşturulan JSON belgesini ve SwaggerUI hizmet veren ara yazılımı etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="de515-139">In the `Configure` method of *Startup.cs*, enable the middleware for serving the generated JSON document and the SwaggerUI:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup2.cs?name=snippet_Configure&highlight=4,7-10)]

<span data-ttu-id="de515-140">Uygulamayı başlatın ve gidin `http://localhost:<random_port>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="de515-140">Launch the app, and navigate to `http://localhost:<random_port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="de515-141">Uç noktaları açıklayan oluşturulan belge görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="de515-141">The generated document describing the endpoints appears.</span></span>

<span data-ttu-id="de515-142">**Not:** Microsoft Edge, Google Chrome, Firefox görünen ve JSON belgelerini yerel olarak.</span><span class="sxs-lookup"><span data-stu-id="de515-142">**Note:** Microsoft Edge, Google Chrome, and Firefox display JSON documents natively.</span></span> <span data-ttu-id="de515-143">Chrome için daha kolay okumak için belgenin biçimlendirme uzantıları vardır.</span><span class="sxs-lookup"><span data-stu-id="de515-143">There are extensions for Chrome that format the document for easier reading.</span></span> <span data-ttu-id="de515-144">*Aşağıdaki örnek okumanızdır azalır.*</span><span class="sxs-lookup"><span data-stu-id="de515-144">*The following example is reduced for brevity.*</span></span>

```json
{
   "swagger": "2.0",
   "info": {
       "version": "v1",
       "title": "API V1"
   },
   "basePath": "/",
   "paths": {
       "/api/Todo": {
           "get": {
               "tags": [
                   "Todo"
               ],
               "operationId": "ApiTodoGet",
               "consumes": [],
               "produces": [
                   "text/plain",
                   "application/json",
                   "text/json"
               ],
               "responses": {
                   "200": {
                       "description": "Success",
                       "schema": {
                           "type": "array",
                           "items": {
                               "$ref": "#/definitions/TodoItem"
                           }
                       }
                   }
                }
           },
           "post": {
               ...
           }
       },
       "/api/Todo/{id}": {
           "get": {
               ...
           },
           "put": {
               ...
           },
           "delete": {
               ...
   },
   "definitions": {
       "TodoItem": {
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
   "securityDefinitions": {}
}
```

<span data-ttu-id="de515-145">Bu belge Swagger giderek görüntülenebilen kullanıcı Arabirimi, sürücüler `http://localhost:<random_port>/swagger`:</span><span class="sxs-lookup"><span data-stu-id="de515-145">This document drives the Swagger UI, which can be viewed by navigating to `http://localhost:<random_port>/swagger`:</span></span>

![Swagger kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="de515-147">Her ortak eylem yönteminde `TodoController` kullanıcı Arabiriminden test edilebilir.</span><span class="sxs-lookup"><span data-stu-id="de515-147">Each public action method in `TodoController` can be tested from the UI.</span></span> <span data-ttu-id="de515-148">Bir yöntem adı bölümü genişletmek için tıklatın.</span><span class="sxs-lookup"><span data-stu-id="de515-148">Click a method name to expand the section.</span></span> <span data-ttu-id="de515-149">Tüm gerekli parametreleri ekleyin ve "deneyin!"'i tıklatın.</span><span class="sxs-lookup"><span data-stu-id="de515-149">Add any necessary parameters, and click "Try it out!".</span></span>

![Örnek Swagger alma testi](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

## <a name="customization--extensibility"></a><span data-ttu-id="de515-151">Özelleştirme ve genişletilebilirlik</span><span class="sxs-lookup"><span data-stu-id="de515-151">Customization & Extensibility</span></span>

<span data-ttu-id="de515-152">Swagger tema eşleştirmek için nesne modeli belgeleme ve kullanıcı arabirimini özelleştirmek için seçenekler sağlar.</span><span class="sxs-lookup"><span data-stu-id="de515-152">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

### <a name="api-info-and-description"></a><span data-ttu-id="de515-153">API bilgileri ve açıklaması</span><span class="sxs-lookup"><span data-stu-id="de515-153">API Info and Description</span></span>

<span data-ttu-id="de515-154">Yapılandırma eylemi geçirilen `AddSwaggerGen` yöntemi, yazar, lisans ve açıklaması gibi bilgileri eklemek için kullanılabilir:</span><span class="sxs-lookup"><span data-stu-id="de515-154">The configuration action passed to the `AddSwaggerGen` method can be used to add information such as the author, license, and description:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?range=20-30,36)]

<span data-ttu-id="de515-155">Aşağıdaki resimde Swagger sürüm bilgilerini görüntüleme UI gösterilmektedir:</span><span class="sxs-lookup"><span data-stu-id="de515-155">The following image depicts the Swagger UI displaying the version information:</span></span>

![Sürüm bilgileri ile swagger kullanıcı Arabirimi: açıklama, yazar ve daha fazla bağlantıya bakın](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="de515-157">XML açıklamaları</span><span class="sxs-lookup"><span data-stu-id="de515-157">XML Comments</span></span>

<span data-ttu-id="de515-158">XML açıklamaları aşağıdaki yaklaşımlardan ile etkinleştirilebilir.</span><span class="sxs-lookup"><span data-stu-id="de515-158">XML comments can be enabled with the following approaches:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="de515-159">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de515-159">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="de515-160">' Nde projeye sağ **Çözüm Gezgini** seçip **özellikleri**</span><span class="sxs-lookup"><span data-stu-id="de515-160">Right-click the project in **Solution Explorer** and select **Properties**</span></span>
* <span data-ttu-id="de515-161">Denetleyin **XML belge dosyası** altında kutusunda **çıkış** bölümünü **yapı** sekmesi:</span><span class="sxs-lookup"><span data-stu-id="de515-161">Check the **XML documentation file** box under the **Output** section of the **Build** tab:</span></span>

![Proje Özellikleri'nin sekmesi oluştur](web-api-help-pages-using-swagger/_static/swagger-xml-comments.png)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="de515-163">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="de515-163">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="de515-164">Açık **proje seçenekleri** iletişim > **yapı** > **derleyici**</span><span class="sxs-lookup"><span data-stu-id="de515-164">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="de515-165">Denetleme **xml belgeleri oluşturmak** altında kutusunda **Genel Seçenekler** bölümü:</span><span class="sxs-lookup"><span data-stu-id="de515-165">Check the **Generate xml documentation** box under the **General Options** section:</span></span>

![Proje seçenekleri Genel Seçenekler bölümünde](web-api-help-pages-using-swagger/_static/swagger-xml-comments-mac.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="de515-167">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="de515-167">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="de515-168">El ile eklemek için aşağıdaki kod parçacığını *.csproj* dosyası:</span><span class="sxs-lookup"><span data-stu-id="de515-168">Manually add the following snippet to the *.csproj* file:</span></span>

[!code-xml[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/TodoApi.csproj?range=7-9)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="de515-169">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="de515-169">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="de515-170">See Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="de515-170">See Visual Studio Code.</span></span>

---

<span data-ttu-id="de515-171">XML açıklamaları etkinleştirme belgelenmemiş genel türleri ve üyeleri için hata ayıklama bilgileri sağlar.</span><span class="sxs-lookup"><span data-stu-id="de515-171">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="de515-172">Belgelenmemiş türleri ve üyeleri tarafından uyarı iletisi gösterilir: *herkese görünür tür veya üye için eksik XML yorum*.</span><span class="sxs-lookup"><span data-stu-id="de515-172">Undocumented types and members are indicated by the warning message: *Missing XML comment for publicly visible type or member*.</span></span>

<span data-ttu-id="de515-173">Oluşturulan XML dosyasını kullanmak için Swagger yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="de515-173">Configure Swagger to use the generated XML file.</span></span> <span data-ttu-id="de515-174">Linux veya Windows olmayan işletim sistemleri için dosya adlarını ve yollarını büyük küçük harfe duyarlı olabilir.</span><span class="sxs-lookup"><span data-stu-id="de515-174">For Linux or non-Windows operating systems, file names and paths can be case sensitive.</span></span> <span data-ttu-id="de515-175">Örneğin, bir *ToDoApi.XML* Windows ancak değil CentOS dosya'nin bulunabilir.</span><span class="sxs-lookup"><span data-stu-id="de515-175">For example, a *ToDoApi.XML* file would be found on Windows but not CentOS.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_ConfigureServices&highlight=20-22)]

<span data-ttu-id="de515-176">Önceki kod `ApplicationBasePath` uygulamanın taban yolu alır.</span><span class="sxs-lookup"><span data-stu-id="de515-176">In the preceding code, `ApplicationBasePath` gets the base path of the app.</span></span> <span data-ttu-id="de515-177">Taban yol XML açıklamaları dosyayı bulmak için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="de515-177">The base path is used to locate the XML comments file.</span></span> <span data-ttu-id="de515-178">*TodoApi.xml* dosya uygulama adına dayalı oluşturulan XML adını yorumları Bu örnekte, yalnızca çalışır.</span><span class="sxs-lookup"><span data-stu-id="de515-178">*TodoApi.xml* only works for this example, since the name of the generated XML comments file is based on the application name.</span></span>

<span data-ttu-id="de515-179">Yöntem Üçlü eğik çizgi açıklamaları ekleme, Swagger kullanıcı arabirimini bölüm başlığı açıklama ekleyerek geliştirir:</span><span class="sxs-lookup"><span data-stu-id="de515-179">Adding the triple-slash comments to the method enhances the Swagger UI by adding the description to the section header:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete&highlight=2)]

![Swagger kullanıcı Arabirimi 'Belirli bir Todoıtem siler.' XML açıklama gösterme](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="de515-182">Kullanıcı arabirimini de bu açıklamalar içeren oluşturulan JSON dosya tarafından yönetilir:</span><span class="sxs-lookup"><span data-stu-id="de515-182">The UI is driven by the generated JSON file, which also contains these comments:</span></span>

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

<span data-ttu-id="de515-183">Ekleme bir [ <remarks> ](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) etiketini `Create` eylem yöntemi belgeleri.</span><span class="sxs-lookup"><span data-stu-id="de515-183">Add a [<remarks>](https://docs.microsoft.com/dotnet/csharp/programming-guide/xmldoc/remarks) tag to the `Create` action method documentation.</span></span> <span data-ttu-id="de515-184">Belirtilen bilgilerini tamamlayan `<summary>` etiketi ve daha güçlü bir Swagger kullanıcı Arabirimi sağlar.</span><span class="sxs-lookup"><span data-stu-id="de515-184">It supplements information specified in the `<summary>` tag and provides a more robust Swagger UI.</span></span> <span data-ttu-id="de515-185">`<remarks>` Metin, JSON veya XML etiket içeriği oluşabilir.</span><span class="sxs-lookup"><span data-stu-id="de515-185">The `<remarks>` tag content can consist of text, JSON, or XML.</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

<span data-ttu-id="de515-186">Bu ek açıklamalar UI geliştirmeleriyle dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="de515-186">Notice the UI enhancements with these additional comments.</span></span>

![Swagger kullanıcı Arabirimi ile gösterilen ek açıklamaları](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="de515-188">Veri ek açıklamaları</span><span class="sxs-lookup"><span data-stu-id="de515-188">Data Annotations</span></span>

<span data-ttu-id="de515-189">Modelin bulunan özniteliklerle tasarlamanız `System.ComponentModel.DataAnnotations`, Swagger kullanıcı Arabirimi bileşenlerini sürücü yardımcı olacak.</span><span class="sxs-lookup"><span data-stu-id="de515-189">Decorate the model with attributes, found in `System.ComponentModel.DataAnnotations`, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="de515-190">Ekleme `[Required]` özniteliğini `Name` özelliği `TodoItem` sınıfı:</span><span class="sxs-lookup"><span data-stu-id="de515-190">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="de515-191">Bu öznitelik varlığını UI davranışını değiştiren ve temel JSON şeması değiştirir:</span><span class="sxs-lookup"><span data-stu-id="de515-191">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

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

<span data-ttu-id="de515-192">Ekleme `[Produces("application/json")]` özniteliği API denetleyicisi.</span><span class="sxs-lookup"><span data-stu-id="de515-192">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="de515-193">Amacı denetleyicinin Eylemler bir içerik türü bir dönüş desteği bildirmektir *uygulama/json*:</span><span class="sxs-lookup"><span data-stu-id="de515-193">Its purpose is to declare that the controller's actions support a return a content type of *application/json*:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_TodoController&highlight=3)]

<span data-ttu-id="de515-194">**Yanıt içerik türü** açılan denetleyicinin GET eylemler için varsayılan olarak bu içerik türü seçer:</span><span class="sxs-lookup"><span data-stu-id="de515-194">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Swagger kullanıcı Arabirimi ile varsayılan yanıt içerik türü](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="de515-196">Web API içinde veri ek açıklamaları kullanımını arttıkça, kullanıcı Arabirimi ve API daha açıklayıcı ve kullanışlı hale sayfaları yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="de515-196">As the usage of data annotations in the Web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describing-response-types"></a><span data-ttu-id="de515-197">Açıklayan yanıt türleri</span><span class="sxs-lookup"><span data-stu-id="de515-197">Describing Response Types</span></span>

<span data-ttu-id="de515-198">Süren geliştiricilerin ne döndürülen ile en ilgili &mdash; özellikle yanıt türleri ve hata kodları (değil standart değilse).</span><span class="sxs-lookup"><span data-stu-id="de515-198">Consuming developers are most concerned with what is returned &mdash; specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="de515-199">Bunlar XML açıklamaları ve veri ek açıklamaları işlenir.</span><span class="sxs-lookup"><span data-stu-id="de515-199">These are handled in the XML comments and data annotations.</span></span>

<span data-ttu-id="de515-200">`Create` Eylem döndürür `201 Created` başarı veya `400 Bad Request` gönderilen istek gövdesi olduğunda null.</span><span class="sxs-lookup"><span data-stu-id="de515-200">The `Create` action returns `201 Created` on success or `400 Bad Request` when the posted request body is null.</span></span> <span data-ttu-id="de515-201">Swagger kullanıcı arabirimini uygun belgelerinde tüketici bu beklenen sonuçları bilgisi eksik.</span><span class="sxs-lookup"><span data-stu-id="de515-201">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="de515-202">Aşağıdaki örnekte vurgulanmış satırlarını ekleyerek bu sorun düzeltilene:</span><span class="sxs-lookup"><span data-stu-id="de515-202">That problem is fixed by adding the highlighted lines in the following example:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

<span data-ttu-id="de515-203">Swagger kullanıcı Arabirimi şimdi beklenen HTTP yanıt kodları açıkça belgeler:</span><span class="sxs-lookup"><span data-stu-id="de515-203">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Swagger kullanıcı Arabirimi 'yeni oluşturulan Yapılacaklar öğesi döndürür' POST yanıt sınıf tanımına gösteren ve '400 - öğe null ise' durum kodunu ve yanıt iletileri altında nedeni](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

### <a name="customizing-the-ui"></a><span data-ttu-id="de515-205">Kullanıcı arabirimini özelleştirme</span><span class="sxs-lookup"><span data-stu-id="de515-205">Customizing the UI</span></span>

<span data-ttu-id="de515-206">UI stock işlevsel ve presentable; Ancak, belge sayfalarının API'nize oluştururken, marka veya temayı temsil etmek için istediğiniz.</span><span class="sxs-lookup"><span data-stu-id="de515-206">The stock UI is both functional and presentable; however, when building documentation pages for your API, you want it to represent your brand or theme.</span></span> <span data-ttu-id="de515-207">Swashbuckle bileşenleri ile bu görevi gerçekleştirmeye statik dosyaları işleme için kaynakları ekleme ve bu dosyaları barındırmak için klasör yapısı oluşturulmasını gerektirir.</span><span class="sxs-lookup"><span data-stu-id="de515-207">Accomplishing that task with the Swashbuckle components requires adding the resources to serve static files and then building the folder structure to host those files.</span></span>

<span data-ttu-id="de515-208">.NET Framework'ü hedefleme varsa ekleyin `Microsoft.AspNetCore.StaticFiles` projesi için NuGet paketi:</span><span class="sxs-lookup"><span data-stu-id="de515-208">If targeting .NET Framework, add the `Microsoft.AspNetCore.StaticFiles` NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="de515-209">Statik dosya ara yazılımlarını etkinleştir:</span><span class="sxs-lookup"><span data-stu-id="de515-209">Enable the static files middleware:</span></span>

[!code-csharp[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/Startup.cs?name=snippet_Configure&highlight=3)]

<span data-ttu-id="de515-210">İçeriği alma *dağ* klasöründen [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span><span class="sxs-lookup"><span data-stu-id="de515-210">Acquire the contents of the *dist* folder from the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui/tree/2.x/dist).</span></span> <span data-ttu-id="de515-211">Bu klasör Swagger kullanıcı Arabirimi sayfası için gerekli varlıkları içerir.</span><span class="sxs-lookup"><span data-stu-id="de515-211">This folder contains the necessary assets for the Swagger UI page.</span></span>

<span data-ttu-id="de515-212">Oluşturma bir *wwwroot/swagger/UI* klasörünü ve içeriğini buraya kopyalayın *dağ* klasör.</span><span class="sxs-lookup"><span data-stu-id="de515-212">Create a *wwwroot/swagger/ui* folder, and copy into it the contents of the *dist* folder.</span></span>

<span data-ttu-id="de515-213">Oluşturma bir *wwwroot/swagger/ui/css/custom.css* sayfa üstbilgisi özelleştirmek için aşağıdaki CSS dosyasıyla:</span><span class="sxs-lookup"><span data-stu-id="de515-213">Create a *wwwroot/swagger/ui/css/custom.css* file with the following CSS to customize the page header:</span></span>

[!code-css[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/css/custom.css)]

<span data-ttu-id="de515-214">Başvuru *custom.css* içinde *index.html* dosyası:</span><span class="sxs-lookup"><span data-stu-id="de515-214">Reference *custom.css* in the *index.html* file:</span></span>

[!code-html[Main](../tutorials/web-api-help-pages-using-swagger/sample/TodoApi/wwwroot/swagger/ui/index.html?range=14)]

<span data-ttu-id="de515-215">Gözat *index.html* adresindeki sayfasında `http://localhost:<random_port>/swagger/ui/index.html`.</span><span class="sxs-lookup"><span data-stu-id="de515-215">Browse to the *index.html* page at `http://localhost:<random_port>/swagger/ui/index.html`.</span></span> <span data-ttu-id="de515-216">Girin `http://localhost:<random_port>/swagger/v1/swagger.json` üst bilginin textbox ve tıklatın **Araştır** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="de515-216">Enter `http://localhost:<random_port>/swagger/v1/swagger.json` in the header's textbox, and click the **Explore** button.</span></span> <span data-ttu-id="de515-217">Sonuçta elde edilen sayfanın şu şekilde görünür:</span><span class="sxs-lookup"><span data-stu-id="de515-217">The resulting page looks as follows:</span></span>

![Özel üstbilgi başlık swagger kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/custom-header.png)

<span data-ttu-id="de515-219">Çok daha fazlasını sayfasıyla yapabilirsiniz yoktur.</span><span class="sxs-lookup"><span data-stu-id="de515-219">There's much more you can do with the page.</span></span> <span data-ttu-id="de515-220">UI kaynakları tam özelliklerine bakın [Swagger kullanıcı Arabirimi GitHub deposunu](https://github.com/swagger-api/swagger-ui).</span><span class="sxs-lookup"><span data-stu-id="de515-220">See the full capabilities for the UI resources at the [Swagger UI GitHub repository](https://github.com/swagger-api/swagger-ui).</span></span>
