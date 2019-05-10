---
title: ASP.NET Core Web API Yardım sayfaları ile Swagger / Openapı
author: rsuter
description: Bu öğretici, belgeler oluşturmak ve Yardım sayfaları için bir Web API uygulaması için Swagger ekleme bir kılavuz sağlar.
ms.author: scaddie
ms.custom: mvc
ms.date: 09/20/2018
uid: tutorials/web-api-help-pages-using-swagger
ms.openlocfilehash: d7a6ed158dcb464bb80c83773ed7d455b25ce44b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2019
ms.locfileid: "64901025"
---
# <a name="aspnet-core-web-api-help-pages-with-swagger--openapi"></a>ASP.NET Core web API Yardım sayfaları ile Swagger / Openapı

Tarafından [Christoph Nienaber](https://twitter.com/zuckerthoben) ve [Riko Suter](http://rsuter.com)

Bir Web API'si kullanılırken, çeşitli metotlarını anlama bir geliştirici için zor olabilir. [Swagger](https://swagger.io/)olarak da bilinen [Openapı](https://www.openapis.org/), Web API'leri için kullanışlı belgeler ve Yardım sayfaları oluşturma sorununu çözer. Bu, etkileşimli belgeleri, istemci SDK oluşturma ve API keşfedilebilirliğini gibi avantajlar sağlar.

Bu makalede, [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) ve [NSwag](https://github.com/RSuter/NSwag) uygulamaları .NET Swagger büyütmüş:

* **Swashbuckle.AspNetCore** Swagger belgeler için ASP.NET Core Web API'leri oluşturmak için açık kaynak bir projedir.

* **NSwag** Swagger belgeler oluşturmak ve tümleştirme için başka bir açık kaynaklı proje [Swagger kullanıcı arabirimini](https://swagger.io/swagger-ui/) veya [ReDoc](https://github.com/Rebilly/ReDoc) içinde ASP.NET Core web API'leri. Ayrıca, NSwag API'niz için C# ve TypeScript istemci kodu oluşturmak için bir yaklaşım sunar.

## <a name="what-is-swagger--openapi"></a>Swagger nedir / Openapı?

Swagger belirtimidir tanımlamak için bir dilden [REST](https://en.wikipedia.org/wiki/Representational_state_transfer) API'leri. Swagger proje için üzerinde Bağış [Openapı girişim](https://www.openapis.org/), burada, artık Openapı adlandırılır. Her iki birbirinin yerine kullanılır; Ancak, Openapı tercih edilir. Bu hem bilgisayarları hem de bir hizmet uygulaması (kaynak kodu, ağ erişimi, belgeleri) doğrudan erişim olmadan yeteneklerini anlamalarına insanlar sağlar. İlişkisi kaldırılan Hizmetleri bağlanmak için gerekli çalışma miktarını en aza bir hedeftir. Başka bir hedefe doğru bir şekilde bir hizmet belgesi için gereken süreyi azaltmaktır.

## <a name="swagger-specification-swaggerjson"></a>Swagger belirtimi (swagger.json)

Swagger akışa çekirdek Swagger belirtimidir&mdash;varsayılan olarak, bir belge adlı *swagger.json*. Hizmet tabanlı Swagger araç zinciri (veya bu üçüncü taraf uygulamaları) tarafından oluşturulur. Bu, API'nizi ve HTTP ile erişmek nasıl yeteneklerini açıklar. Swagger kullanıcı arabirimini yönlendirir ve araç zinciri tarafından bulma ve istemci kodu oluşturmayı etkinleştirme için kullanılır. Konuyu uzatmamak amacıyla sınırlı bir Swagger belirtimi bir örnek aşağıda verilmiştir:

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

[Swagger kullanıcı Arabirimi](https://swagger.io/swagger-ui/) oluşturulan Swagger belirtimi kullanarak hizmeti hakkında bilgi sağlayan bir web tabanlı bir kullanıcı Arabirimi sunar. ASP.NET Core uygulamanızı bir ara yazılım kayıt çağrısı kullanarak barındırılan Swashbuckle hem NSwag Swagger kullanıcı arabirimini, katıştırılmış bir sürümünü içerir. Web kullanıcı Arabirimi şöyle görünür:

![Swagger kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Kullanıcı Arabiriminden denetleyicilerinizi her genel bir eylem yöntemi test edilebilir. Bir yöntem adı bölümü genişletmek için tıklayın. Gerekli tüm parametreleri ekleyin ve **deneyin!**.

![Örnek Swagger alma test](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> Ekran görüntüleri için kullanılan Swagger kullanıcı arabirimini sürüm sürüm 2 ' dir. Sürüm 3 örnek için bkz: [Petstore örnek](http://petstore.swagger.io/).

## <a name="next-steps"></a>Sonraki adımlar

* [Swashbuckle kullanmaya başlama](xref:tutorials/get-started-with-swashbuckle)
* [NSwag Kullanmaya Başlama](xref:tutorials/get-started-with-nswag)
