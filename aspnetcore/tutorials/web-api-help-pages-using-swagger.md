---
title: ASP.NET Core Web API Yardım sayfaları ile Swagger / Openapı
author: RicoSuter
description: Bu öğretici, belgeler oluşturmak ve Yardım sayfaları için bir Web API uygulaması için Swagger ekleme bir kılavuz sağlar.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/07/2019
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 4408e02996b958bf009903aa1e4eeda9ad4f457c
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658475"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--openapi"></a><span data-ttu-id="c24a3-103">ASP.NET Core web API Yardım sayfaları ile Swagger / Openapı</span><span class="sxs-lookup"><span data-stu-id="c24a3-103">ASP.NET Core web API help pages with Swagger / OpenAPI</span></span>

<span data-ttu-id="c24a3-104">Sağlayan- [Christoph Nienaber](https://twitter.com/zuckerthoben) ve [Riko Suter](https://blog.rsuter.com/)</span><span class="sxs-lookup"><span data-stu-id="c24a3-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](https://blog.rsuter.com/)</span></span>

<span data-ttu-id="c24a3-105">Bir Web API'si kullanılırken, çeşitli metotlarını anlama bir geliştirici için zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="c24a3-105">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="c24a3-106">[Openapı](https://www.openapis.org/)olarak da bilinen [Swagger](https://swagger.io/), Web API 'leri için faydalı belge ve yardım sayfaları oluşturma sorununu çözer.</span><span class="sxs-lookup"><span data-stu-id="c24a3-106">[Swagger](https://swagger.io/), also known as [OpenAPI](https://www.openapis.org/), solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="c24a3-107">Bu, etkileşimli belgeleri, istemci SDK oluşturma ve API keşfedilebilirliğini gibi avantajlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="c24a3-107">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="c24a3-108">Bu makalede, [swashbuckle. AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) ve [nswag](https://github.com/RicoSuter/NSwag) .net Swagger uygulamaları gösterilir:</span><span class="sxs-lookup"><span data-stu-id="c24a3-108">In this article, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) and [NSwag](https://github.com/RicoSuter/NSwag) .NET Swagger implementations are showcased:</span></span>

* <span data-ttu-id="c24a3-109">**Swashbuckle. AspNetCore** , ASP.NET Core Web API 'Leri için Swagger belgelerini oluşturmaya yönelik açık kaynak bir projem.</span><span class="sxs-lookup"><span data-stu-id="c24a3-109">**Swashbuckle.AspNetCore** is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="c24a3-110">**Nswag** , Swagger belgelerini oluşturmaya ve [Swagger Kullanıcı arabirimini](https://swagger.io/swagger-ui/) veya [yeniden belgeyi](https://github.com/Rebilly/ReDoc) ASP.NET Core Web API 'lerine tümleştirmede başka bir açık kaynak projem.</span><span class="sxs-lookup"><span data-stu-id="c24a3-110">**NSwag** is another open source project for generating Swagger documents and integrating [Swagger UI](https://swagger.io/swagger-ui/) or [ReDoc](https://github.com/Rebilly/ReDoc) into ASP.NET Core web APIs.</span></span> <span data-ttu-id="c24a3-111">Ayrıca, NSwag API'niz için C# ve TypeScript istemci kodu oluşturmak için bir yaklaşım sunar.</span><span class="sxs-lookup"><span data-stu-id="c24a3-111">Additionally, NSwag offers approaches to generate C# and TypeScript client code for your API.</span></span>

## <a name="what-is-swagger--openapi"></a><span data-ttu-id="c24a3-112">Swagger nedir / Openapı?</span><span class="sxs-lookup"><span data-stu-id="c24a3-112">What is Swagger / OpenAPI?</span></span>

<span data-ttu-id="c24a3-113">Swagger, [rest](https://en.wikipedia.org/wiki/Representational_state_transfer) API 'leri açıklamak için dilden bağımsız bir belirtimdir.</span><span class="sxs-lookup"><span data-stu-id="c24a3-113">Swagger is a language-agnostic specification for describing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs.</span></span> <span data-ttu-id="c24a3-114">Swagger projesi, openapı [Initiative](https://www.openapis.org/)'e bağlılmıştı ve burada openapı olarak adlandırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="c24a3-114">The Swagger project was donated to the [OpenAPI Initiative](https://www.openapis.org/), where it's now referred to as OpenAPI.</span></span> <span data-ttu-id="c24a3-115">Her iki birbirinin yerine kullanılır; Ancak, Openapı tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="c24a3-115">Both names are used interchangeably; however, OpenAPI is preferred.</span></span> <span data-ttu-id="c24a3-116">Bu hem bilgisayarları hem de bir hizmet uygulaması (kaynak kodu, ağ erişimi, belgeleri) doğrudan erişim olmadan yeteneklerini anlamalarına insanlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="c24a3-116">It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation).</span></span> <span data-ttu-id="c24a3-117">İlişkisi kaldırılan Hizmetleri bağlanmak için gerekli çalışma miktarını en aza bir hedeftir.</span><span class="sxs-lookup"><span data-stu-id="c24a3-117">One goal is to minimize the amount of work needed to connect disassociated services.</span></span> <span data-ttu-id="c24a3-118">Başka bir hedefe doğru bir şekilde bir hizmet belgesi için gereken süreyi azaltmaktır.</span><span class="sxs-lookup"><span data-stu-id="c24a3-118">Another goal is to reduce the amount of time needed to accurately document a service.</span></span>

## <a name="swagger-specification-swaggerjson"></a><span data-ttu-id="c24a3-119">Swagger belirtimi (swagger.json)</span><span class="sxs-lookup"><span data-stu-id="c24a3-119">Swagger specification (swagger.json)</span></span>

<span data-ttu-id="c24a3-120">Swagger akışının çekirdeği, varsayılan olarak *Swagger. JSON*adlı bir belge olan swagger belirtimidir&mdash;.</span><span class="sxs-lookup"><span data-stu-id="c24a3-120">The core to the Swagger flow is the Swagger specification&mdash;by default, a document named *swagger.json*.</span></span> <span data-ttu-id="c24a3-121">Hizmet tabanlı Swagger araç zinciri (veya bu üçüncü taraf uygulamaları) tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="c24a3-121">It's generated by the Swagger tool chain (or third-party implementations of it) based on your service.</span></span> <span data-ttu-id="c24a3-122">Bu, API'nizi ve HTTP ile erişmek nasıl yeteneklerini açıklar.</span><span class="sxs-lookup"><span data-stu-id="c24a3-122">It describes the capabilities of your API and how to access it with HTTP.</span></span> <span data-ttu-id="c24a3-123">Swagger kullanıcı arabirimini yönlendirir ve araç zinciri tarafından bulma ve istemci kodu oluşturmayı etkinleştirme için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="c24a3-123">It drives the Swagger UI and is used by the tool chain to enable discovery and client code generation.</span></span> <span data-ttu-id="c24a3-124">Konuyu uzatmamak amacıyla sınırlı bir Swagger belirtimi bir örnek aşağıda verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="c24a3-124">Here's an example of a Swagger specification, reduced for brevity:</span></span>

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

## <a name="swagger-ui"></a><span data-ttu-id="c24a3-125">Swagger kullanıcı Arabirimi</span><span class="sxs-lookup"><span data-stu-id="c24a3-125">Swagger UI</span></span>

<span data-ttu-id="c24a3-126">[Swagger Kullanıcı arabirimi](https://swagger.io/swagger-ui/) , oluşturulan Swagger belirtimini kullanarak hizmet hakkında bilgi sağlayan Web tabanlı bir kullanıcı arabirimi sunar.</span><span class="sxs-lookup"><span data-stu-id="c24a3-126">[Swagger UI](https://swagger.io/swagger-ui/) offers a web-based UI that provides information about the service, using the generated Swagger specification.</span></span> <span data-ttu-id="c24a3-127">ASP.NET Core uygulamanızı bir ara yazılım kayıt çağrısı kullanarak barındırılan Swashbuckle hem NSwag Swagger kullanıcı arabirimini, katıştırılmış bir sürümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="c24a3-127">Both Swashbuckle and NSwag include an embedded version of Swagger UI, so that it can be hosted in your ASP.NET Core app using a middleware registration call.</span></span> <span data-ttu-id="c24a3-128">Web kullanıcı Arabirimi şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="c24a3-128">The web UI looks like this:</span></span>

![Swagger kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="c24a3-130">Kullanıcı Arabiriminden denetleyicilerinizi her genel bir eylem yöntemi test edilebilir.</span><span class="sxs-lookup"><span data-stu-id="c24a3-130">Each public action method in your controllers can be tested from the UI.</span></span> <span data-ttu-id="c24a3-131">Bir yöntem adı bölümü genişletmek için tıklayın.</span><span class="sxs-lookup"><span data-stu-id="c24a3-131">Click a method name to expand the section.</span></span> <span data-ttu-id="c24a3-132">Gerekli parametreleri ekleyin ve **deneyin!** ' e tıklayın.</span><span class="sxs-lookup"><span data-stu-id="c24a3-132">Add any necessary parameters, and click **Try it out!**.</span></span>

![Örnek Swagger alma test](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> <span data-ttu-id="c24a3-134">Ekran görüntüleri için kullanılan Swagger kullanıcı arabirimini sürüm sürüm 2 ' dir.</span><span class="sxs-lookup"><span data-stu-id="c24a3-134">The Swagger UI version used for the screenshots is version 2.</span></span> <span data-ttu-id="c24a3-135">Sürüm 3 örneği için bkz. [petstore örneği](https://petstore.swagger.io/).</span><span class="sxs-lookup"><span data-stu-id="c24a3-135">For a version 3 example, see [Petstore example](https://petstore.swagger.io/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c24a3-136">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="c24a3-136">Next steps</span></span>

* [<span data-ttu-id="c24a3-137">Swashbuckle kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="c24a3-137">Get started with Swashbuckle</span></span>](xref:tutorials/get-started-with-swashbuckle)
* [<span data-ttu-id="c24a3-138">NSwag Kullanmaya Başlama</span><span class="sxs-lookup"><span data-stu-id="c24a3-138">Get started with NSwag</span></span>](xref:tutorials/get-started-with-nswag)
