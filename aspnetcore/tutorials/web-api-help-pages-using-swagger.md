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
# <a name="aspnet-core-web-api-help-pages-with-swagger--openapi"></a>ASP.NET Core web API Yardım sayfaları ile Swagger / Openapı

Sağlayan- [Christoph Nienaber](https://twitter.com/zuckerthoben) ve [Riko Suter](https://blog.rsuter.com/)

Bir Web API'si kullanılırken, çeşitli metotlarını anlama bir geliştirici için zor olabilir. [Openapı](https://www.openapis.org/)olarak da bilinen [Swagger](https://swagger.io/), Web API 'leri için faydalı belge ve yardım sayfaları oluşturma sorununu çözer. Bu, etkileşimli belgeleri, istemci SDK oluşturma ve API keşfedilebilirliğini gibi avantajlar sağlar.

Bu makalede, [swashbuckle. AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore) ve [nswag](https://github.com/RicoSuter/NSwag) .net Swagger uygulamaları gösterilir:

* **Swashbuckle. AspNetCore** , ASP.NET Core Web API 'Leri için Swagger belgelerini oluşturmaya yönelik açık kaynak bir projem.

* **Nswag** , Swagger belgelerini oluşturmaya ve [Swagger Kullanıcı arabirimini](https://swagger.io/swagger-ui/) veya [yeniden belgeyi](https://github.com/Rebilly/ReDoc) ASP.NET Core Web API 'lerine tümleştirmede başka bir açık kaynak projem. Ayrıca, NSwag API'niz için C# ve TypeScript istemci kodu oluşturmak için bir yaklaşım sunar.

## <a name="what-is-swagger--openapi"></a>Swagger nedir / Openapı?

Swagger, [rest](https://en.wikipedia.org/wiki/Representational_state_transfer) API 'leri açıklamak için dilden bağımsız bir belirtimdir. Swagger projesi, openapı [Initiative](https://www.openapis.org/)'e bağlılmıştı ve burada openapı olarak adlandırılmıştır. Her iki birbirinin yerine kullanılır; Ancak, Openapı tercih edilir. Bu hem bilgisayarları hem de bir hizmet uygulaması (kaynak kodu, ağ erişimi, belgeleri) doğrudan erişim olmadan yeteneklerini anlamalarına insanlar sağlar. İlişkisi kaldırılan Hizmetleri bağlanmak için gerekli çalışma miktarını en aza bir hedeftir. Başka bir hedefe doğru bir şekilde bir hizmet belgesi için gereken süreyi azaltmaktır.

## <a name="swagger-specification-swaggerjson"></a>Swagger belirtimi (swagger.json)

Swagger akışının çekirdeği, varsayılan olarak *Swagger. JSON*adlı bir belge olan swagger belirtimidir&mdash;. Hizmet tabanlı Swagger araç zinciri (veya bu üçüncü taraf uygulamaları) tarafından oluşturulur. Bu, API'nizi ve HTTP ile erişmek nasıl yeteneklerini açıklar. Swagger kullanıcı arabirimini yönlendirir ve araç zinciri tarafından bulma ve istemci kodu oluşturmayı etkinleştirme için kullanılır. Konuyu uzatmamak amacıyla sınırlı bir Swagger belirtimi bir örnek aşağıda verilmiştir:

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

[Swagger Kullanıcı arabirimi](https://swagger.io/swagger-ui/) , oluşturulan Swagger belirtimini kullanarak hizmet hakkında bilgi sağlayan Web tabanlı bir kullanıcı arabirimi sunar. ASP.NET Core uygulamanızı bir ara yazılım kayıt çağrısı kullanarak barındırılan Swashbuckle hem NSwag Swagger kullanıcı arabirimini, katıştırılmış bir sürümünü içerir. Web kullanıcı Arabirimi şöyle görünür:

![Swagger kullanıcı Arabirimi](web-api-help-pages-using-swagger/_static/swagger-ui.png)

Kullanıcı Arabiriminden denetleyicilerinizi her genel bir eylem yöntemi test edilebilir. Bir yöntem adı bölümü genişletmek için tıklayın. Gerekli parametreleri ekleyin ve **deneyin!** ' e tıklayın.

![Örnek Swagger alma test](web-api-help-pages-using-swagger/_static/get-try-it-out.png)

> [!NOTE]
> Ekran görüntüleri için kullanılan Swagger kullanıcı arabirimini sürüm sürüm 2 ' dir. Sürüm 3 örneği için bkz. [petstore örneği](https://petstore.swagger.io/).

## <a name="next-steps"></a>Sonraki adımlar

* [Swashbuckle kullanmaya başlama](xref:tutorials/get-started-with-swashbuckle)
* [NSwag Kullanmaya Başlama](xref:tutorials/get-started-with-nswag)
