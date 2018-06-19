---
title: ASP.NET Core Web API Yardım sayfası Swagger / açık API
author: rsuter
description: Bu öğretici belgeleri oluşturmak ve bir Web API uygulaması için sayfa yardımcı olmak için Swagger ekleme bir kılavuz sağlar.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 03/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: e44b491fd5265e12646efa42f12eb0662e287f04
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077342"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--open-api"></a>ASP.NET Core Web API Yardım sayfası Swagger / açık API

Tarafından [Christoph Nienaber](https://twitter.com/zuckerthoben) ve [Riko Suter](http://rsuter.com)

Bir Web API kullanırken, çeşitli yöntemlerini anlamak için bir geliştirici zor olabilir. [Swagger](https://swagger.io/), olarak da bilinen açık API, Web API'leri için yararlı belgeler ve Yardım sayfaları oluşturma sorunu çözer. Etkileşimli belgeler, istemci SDK oluşturma ve API bulunabilirliği gibi fayda sağlar.

Bu makalede, [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) ve [NSwag](https://github.com/RSuter/NSwag) uygulamaları .NET Swagger showcased:

* **Swashbuckle.AspNetCore** ASP.NET çekirdek Web API için Swagger belgeleri oluşturmak için bir açık kaynaklı proje.

* **NSwag** tümleştirmek için başka bir açık kaynaklı proje [Swagger kullanıcı arabirimini](https://swagger.io/swagger-ui/) veya [ReDoc](https://github.com/Rebilly/ReDoc) ASP.NET çekirdek Web API uygulamasına. API için C# ve TypeScript istemci kodu oluşturmak için yaklaşım sunar.

## <a name="what-is-swagger--open-api"></a>Swagger / açmak nedir API?

Swagger belirtimidir açıklayan bir dilden bağımsız [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API'leri. Swagger proje bağışlanır [OpenAPI Initiative](https://www.openapis.org/), burada bunu şimdi için açık API adlandırılır. Her iki birbirinin yerine kullanıldığı; Ancak, açık API tercih edilir. Bu, hem bilgisayarları hem de bir hizmet uygulaması (kaynak kodu, ağ erişimi, belgeleri) doğrudan erişim olmadan özelliklerini anlamak için insanlar sağlar. Bir iş ilişkilendirilmemiş Hizmetleri bağlanmak için gereken en aza indirmek için hedeftir. Başka bir hizmet doğru bir şekilde belge için gereken süreyi azaltmak amacıyla belirtilir.

## <a name="swagger-specification-swaggerjson"></a>Swagger specification (swagger.json)

Swagger akışı için çekirdek Swagger belirtimidir&mdash;varsayılan olarak, bir belge adlı *swagger.json*. Hizmet tabanlı Swagger aracını zinciri (veya üçüncü taraf uygulamaları bunu) tarafından oluşturulur. API'nizi ve HTTP ile erişim özelliklerini açıklar. Swagger kullanıcı arabirimini yönlendirir ve aracı zincir bulma ve istemci kodu oluşturma etkinleştirmek için kullanılır. Konuyu uzatmamak amacıyla sınırlı bir Swagger belirtimi bir örneği burada verilmiştir:

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

## <a name="swagger-ui"></a>Swagger kullanıcı Arabirimi

[Swagger kullanıcı Arabirimi](https://swagger.io/swagger-ui/) oluşturulan Swagger belirtimi kullanarak hizmeti hakkında bilgi sağlayan bir web tabanlı bir kullanıcı Arabirimi sunar. Bir ara yazılım kayıt çağrısı kullanarak ASP.NET Core uygulamanızda barındırılan Swashbuckle ve NSwag Swagger kullanıcı Arabirimi, katıştırılmış bir sürümünü içerir. Web kullanıcı Arabirimi şöyle görünür:

![Swagger kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Her denetleyicilerinizi ortak eylem yönteminde kullanıcı Arabiriminden sınanabilir. Bir yöntem adı bölümü genişletmek için tıklatın. Tüm gerekli parametreleri ekleyin ve **deneyin!**.

![Örnek Swagger alma testi](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> Ekran görüntüleri için kullanılan Swagger kullanıcı arabirimini sürüm 2 sürümdür. Sürüm 3 örnek için bkz: [Petstore örnek](http://petstore.swagger.io/).

## <a name="next-steps"></a>Sonraki adımlar

* [Swashbuckle ile çalışmaya başlama](xref:tutorials/get-started-with-swashbuckle)
* [NSwag ile çalışmaya başlama](xref:tutorials/get-started-with-nswag)
