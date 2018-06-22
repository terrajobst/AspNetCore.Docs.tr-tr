---
title: ASP.NET Core Web API Yardım sayfası Swagger / açık API
author: rsuter
description: Bu öğretici belgeleri oluşturmak ve bir Web API uygulaması için sayfa yardımcı olmak için Swagger ekleme bir kılavuz sağlar.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/09/2018
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: 56e146337ad9e94298f72abf5ede009eea65fb46
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272257"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--open-api"></a><span data-ttu-id="3f3df-103">ASP.NET Core Web API Yardım sayfası Swagger / açık API</span><span class="sxs-lookup"><span data-stu-id="3f3df-103">ASP.NET Core Web API help pages with Swagger / Open API</span></span>

<span data-ttu-id="3f3df-104">Tarafından [Christoph Nienaber](https://twitter.com/zuckerthoben) ve [Riko Suter](http://rsuter.com)</span><span class="sxs-lookup"><span data-stu-id="3f3df-104">By [Christoph Nienaber](https://twitter.com/zuckerthoben) and [Rico Suter](http://rsuter.com)</span></span>

<span data-ttu-id="3f3df-105">Bir Web API kullanırken, çeşitli yöntemlerini anlamak için bir geliştirici zor olabilir.</span><span class="sxs-lookup"><span data-stu-id="3f3df-105">When consuming a Web API, understanding its various methods can be challenging for a developer.</span></span> <span data-ttu-id="3f3df-106">[Swagger](https://swagger.io/), olarak da bilinen açık API, Web API'leri için yararlı belgeler ve Yardım sayfaları oluşturma sorunu çözer.</span><span class="sxs-lookup"><span data-stu-id="3f3df-106">[Swagger](https://swagger.io/), also known as Open API, solves the problem of generating useful documentation and help pages for Web APIs.</span></span> <span data-ttu-id="3f3df-107">Etkileşimli belgeler, istemci SDK oluşturma ve API bulunabilirliği gibi fayda sağlar.</span><span class="sxs-lookup"><span data-stu-id="3f3df-107">It provides benefits such as interactive documentation, client SDK generation, and API discoverability.</span></span>

<span data-ttu-id="3f3df-108">Bu makalede, [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) ve [NSwag](https://github.com/RSuter/NSwag) uygulamaları .NET Swagger showcased:</span><span class="sxs-lookup"><span data-stu-id="3f3df-108">In this article, the [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) and [NSwag](https://github.com/RSuter/NSwag) .NET Swagger implementations are showcased:</span></span>

* <span data-ttu-id="3f3df-109">**Swashbuckle.AspNetCore** ASP.NET çekirdek Web API için Swagger belgeleri oluşturmak için bir açık kaynaklı proje.</span><span class="sxs-lookup"><span data-stu-id="3f3df-109">**Swashbuckle.AspNetCore** is an open source project for generating Swagger documents for ASP.NET Core Web APIs.</span></span>

* <span data-ttu-id="3f3df-110">**NSwag** tümleştirmek için başka bir açık kaynaklı proje [Swagger kullanıcı arabirimini](https://swagger.io/swagger-ui/) veya [ReDoc](https://github.com/Rebilly/ReDoc) ASP.NET çekirdek Web API uygulamasına.</span><span class="sxs-lookup"><span data-stu-id="3f3df-110">**NSwag** is another open source project for integrating [Swagger UI](https://swagger.io/swagger-ui/) or [ReDoc](https://github.com/Rebilly/ReDoc) into ASP.NET Core Web APIs.</span></span> <span data-ttu-id="3f3df-111">API için C# ve TypeScript istemci kodu oluşturmak için yaklaşım sunar.</span><span class="sxs-lookup"><span data-stu-id="3f3df-111">It offers approaches to generate C# and TypeScript client code for your API.</span></span>

## <a name="what-is-swagger--open-api"></a><span data-ttu-id="3f3df-112">Swagger / açmak nedir API?</span><span class="sxs-lookup"><span data-stu-id="3f3df-112">What is Swagger / Open API?</span></span>

<span data-ttu-id="3f3df-113">Swagger belirtimidir açıklayan bir dilden bağımsız [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API'leri.</span><span class="sxs-lookup"><span data-stu-id="3f3df-113">Swagger is a language-agnostic specification for describing [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) APIs.</span></span> <span data-ttu-id="3f3df-114">Swagger proje bağışlanır [OpenAPI Initiative](https://www.openapis.org/), burada bunu şimdi için açık API adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="3f3df-114">The Swagger project was donated to the [OpenAPI Initiative](https://www.openapis.org/), where it's now referred to as Open API.</span></span> <span data-ttu-id="3f3df-115">Her iki birbirinin yerine kullanıldığı; Ancak, açık API tercih edilir.</span><span class="sxs-lookup"><span data-stu-id="3f3df-115">Both names are used interchangeably; however, Open API is preferred.</span></span> <span data-ttu-id="3f3df-116">Bu, hem bilgisayarları hem de bir hizmet uygulaması (kaynak kodu, ağ erişimi, belgeleri) doğrudan erişim olmadan özelliklerini anlamak için insanlar sağlar.</span><span class="sxs-lookup"><span data-stu-id="3f3df-116">It allows both computers and humans to understand the capabilities of a service without any direct access to the implementation (source code, network access, documentation).</span></span> <span data-ttu-id="3f3df-117">Bir iş ilişkilendirilmemiş Hizmetleri bağlanmak için gereken en aza indirmek için hedeftir.</span><span class="sxs-lookup"><span data-stu-id="3f3df-117">One goal is to minimize the amount of work needed to connect disassociated services.</span></span> <span data-ttu-id="3f3df-118">Başka bir hizmet doğru bir şekilde belge için gereken süreyi azaltmak amacıyla belirtilir.</span><span class="sxs-lookup"><span data-stu-id="3f3df-118">Another goal is to reduce the amount of time needed to accurately document a service.</span></span>

## <a name="swagger-specification-swaggerjson"></a><span data-ttu-id="3f3df-119">Swagger belirtimi (swagger.json)</span><span class="sxs-lookup"><span data-stu-id="3f3df-119">Swagger specification (swagger.json)</span></span>

<span data-ttu-id="3f3df-120">Swagger akışı için çekirdek Swagger belirtimidir&mdash;varsayılan olarak, bir belge adlı *swagger.json*.</span><span class="sxs-lookup"><span data-stu-id="3f3df-120">The core to the Swagger flow is the Swagger specification&mdash;by default, a document named *swagger.json*.</span></span> <span data-ttu-id="3f3df-121">Hizmet tabanlı Swagger aracını zinciri (veya üçüncü taraf uygulamaları bunu) tarafından oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="3f3df-121">It's generated by the Swagger tool chain (or third-party implementations of it) based on your service.</span></span> <span data-ttu-id="3f3df-122">API'nizi ve HTTP ile erişim özelliklerini açıklar.</span><span class="sxs-lookup"><span data-stu-id="3f3df-122">It describes the capabilities of your API and how to access it with HTTP.</span></span> <span data-ttu-id="3f3df-123">Swagger kullanıcı arabirimini yönlendirir ve aracı zincir bulma ve istemci kodu oluşturma etkinleştirmek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="3f3df-123">It drives the Swagger UI and is used by the tool chain to enable discovery and client code generation.</span></span> <span data-ttu-id="3f3df-124">Konuyu uzatmamak amacıyla sınırlı bir Swagger belirtimi bir örneği burada verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="3f3df-124">Here's an example of a Swagger specification, reduced for brevity:</span></span>

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

## <a name="swagger-ui"></a><span data-ttu-id="3f3df-125">Swagger kullanıcı Arabirimi</span><span class="sxs-lookup"><span data-stu-id="3f3df-125">Swagger UI</span></span>

<span data-ttu-id="3f3df-126">[Swagger kullanıcı Arabirimi](https://swagger.io/swagger-ui/) oluşturulan Swagger belirtimi kullanarak hizmeti hakkında bilgi sağlayan bir web tabanlı bir kullanıcı Arabirimi sunar.</span><span class="sxs-lookup"><span data-stu-id="3f3df-126">[Swagger UI](https://swagger.io/swagger-ui/) offers a web-based UI that provides information about the service, using the generated Swagger specification.</span></span> <span data-ttu-id="3f3df-127">Bir ara yazılım kayıt çağrısı kullanarak ASP.NET Core uygulamanızda barındırılan Swashbuckle ve NSwag Swagger kullanıcı Arabirimi, katıştırılmış bir sürümünü içerir.</span><span class="sxs-lookup"><span data-stu-id="3f3df-127">Both Swashbuckle and NSwag include an embedded version of Swagger UI, so that it can be hosted in your ASP.NET Core app using a middleware registration call.</span></span> <span data-ttu-id="3f3df-128">Web kullanıcı Arabirimi şöyle görünür:</span><span class="sxs-lookup"><span data-stu-id="3f3df-128">The web UI looks like this:</span></span>

![Swagger kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/swagger-ui.png)

<span data-ttu-id="3f3df-130">Her denetleyicilerinizi ortak eylem yönteminde kullanıcı Arabiriminden sınanabilir.</span><span class="sxs-lookup"><span data-stu-id="3f3df-130">Each public action method in your controllers can be tested from the UI.</span></span> <span data-ttu-id="3f3df-131">Bir yöntem adı bölümü genişletmek için tıklatın.</span><span class="sxs-lookup"><span data-stu-id="3f3df-131">Click a method name to expand the section.</span></span> <span data-ttu-id="3f3df-132">Tüm gerekli parametreleri ekleyin ve **deneyin!**.</span><span class="sxs-lookup"><span data-stu-id="3f3df-132">Add any necessary parameters, and click **Try it out!**.</span></span>

![Örnek Swagger alma testi](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> <span data-ttu-id="3f3df-134">Ekran görüntüleri için kullanılan Swagger kullanıcı arabirimini sürüm 2 sürümdür.</span><span class="sxs-lookup"><span data-stu-id="3f3df-134">The Swagger UI version used for the screenshots is version 2.</span></span> <span data-ttu-id="3f3df-135">Sürüm 3 örnek için bkz: [Petstore örnek](http://petstore.swagger.io/).</span><span class="sxs-lookup"><span data-stu-id="3f3df-135">For a version 3 example, see [Petstore example](http://petstore.swagger.io/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f3df-136">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="3f3df-136">Next steps</span></span>

* [<span data-ttu-id="3f3df-137">Swashbuckle kullanmaya başlama</span><span class="sxs-lookup"><span data-stu-id="3f3df-137">Get started with Swashbuckle</span></span>](xref:tutorials/get-started-with-swashbuckle)
* [<span data-ttu-id="3f3df-138">NSwag Kullanmaya Başlama</span><span class="sxs-lookup"><span data-stu-id="3f3df-138">Get started with NSwag</span></span>](xref:tutorials/get-started-with-nswag)
