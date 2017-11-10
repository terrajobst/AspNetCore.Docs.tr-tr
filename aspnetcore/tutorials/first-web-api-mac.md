---
title: "Mac için ASP.NET Core ve Visual Studio ile Web API oluşturma"
description: "Mac için ASP.NET Core MVC ve Visual Studio ile Web API oluşturma"
author: rick-anderson
ms.author: riande
ms.date: 09/15/2017
ms.topic: get-started-article
ms.prod: asp.net-core
uid: tutorials/first-web-api-mac
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
ms.technology: aspnet
keywords: "ASP.NET Core, Webapı, Web API'si, REST, mac, macOS, HTTP, hizmeti, HTTP hizmeti"
manager: wpickett
ms.openlocfilehash: 6bbd5e332e395928d8f79888ecf190f7f59a4bbc
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/01/2017
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a>Mac için ASP.NET Core MVC ve Visual Studio ile Web API oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)

Bu öğreticide, "Yapılacaklar" öğeleri listesini yönetmek için bir web API'si oluşturma. Bir kullanıcı Arabirimi yapı olmaz.

Bu öğretici 3 sürümü vardır:

* macOS: Web API'si Mac (Bu öğretici) için Visual Studio ile
* Windows: [Web API'si Visual Studio ile Windows için](xref:tutorials/first-web-api)
* macOS, Linux, Windows: [Visual Studio Code ile Web API](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* Bkz: [ASP.NET Core MVC Mac veya Linux giriş](xref:tutorials/first-mvc-app-xplat/index) kalıcı bir veritabanı kullanan bir örnek.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki yükleyin:

- [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) veya daha yenisi
- [Mac için Visual Studio](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a>Projeyi oluşturma

Visual Studio'dan seçin **Dosya > Yeni bir çözüm**.

![macOS yeni çözüm](first-web-api-mac/_static/sln.png)

Seçin **.NET Core uygulaması > ASP.NET Core Web API > sonraki**.

![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)

Girin **TodoApi** için **proje adı**ve Oluştur'u seçin.

![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Uygulamayı başlatın

Visual Studio'da seçin **Çalıştır > Başlat ile hata ayıklama** uygulamayı başlatmak için. Visual Studio bir tarayıcı ile başlatarak `http://localhost:5000`. Bir HTTP 404 (bulunamadı) hata alıyorsunuz.  URL'ye değiştirin `http://localhost:port/api/values`. `ValuesController` Veri görüntülenir:

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Entity Framework Çekirdek desteği ekleme

Yükleme [Entity Framework Çekirdek Inmemory](https://docs.microsoft.com/ef/core/providers/in-memory/) veritabanı sağlayıcısı. Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.

* Gelen **proje** menüsünde, select **NuGet paketleri Ekle**. 

  *  Alternatif olarak, sağ tıklayarak **bağımlılıkları**ve ardından **paketleri Ekle**.

* Girin `EntityFrameworkCore.InMemory` arama kutusuna.
* Seçin `Microsoft.EntityFrameworkCore.InMemory`ve ardından **Paketi Ekle**.

### <a name="add-a-model-class"></a>Bir model sınıfı ekleme

Uygulamanızdaki verileri temsil eden bir nesne modelidir. Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.

Adlı bir klasör ekleme *modelleri*. Çözüm Gezgini'nde projeye sağ tıklayın. Seçin **ekleme** > **yeni klasör**. Klasör adı *modelleri*.

![Yeni klasör](first-web-api-mac/_static/folder.png)

Not:, Modeli sınıfları herhangi bir yere, projenizdeki koyabilirsiniz ancak *modelleri* klasörü, kural tarafından kullanılır.

Ekleme bir `TodoItem` sınıfı. Sağ *modelleri* klasörü ve select **Ekle > Yeni Dosya > Genel > boş sınıfı**. Sınıf adını `TodoItem`ve ardından **yeni**.

Oluşturulan kod ile değiştirin:

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.

### <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturma

*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır. Bu sınıf türetme tarafından oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.

Ekleme bir `TodoContext` sınıfının *modelleri* klasör.

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Denetleyici ekleme

Çözüm Gezgini'nde, içinde *denetleyicileri* klasörünü sınıfı ekleme `TodoController`.

Oluşturulan kod aşağıdakiyle değiştirin (ve kapanış köşeli parantez ekleyin):

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uygulamayı başlatın

Visual Studio'da seçin **Çalıştır > Başlat ile hata ayıklama** uygulamayı başlatmak için. Visual Studio bir tarayıcı ile başlatarak `http://localhost:port`, burada *bağlantı noktası* rastgele seçilen bağlantı noktası numarasıdır. Bir HTTP 404 (bulunamadı) hata alıyorsunuz.  URL'ye değiştirin `http://localhost:port/api/values`. `ValuesController` Veri görüntülenir:

```
["value1","value2"]
```

Gidin `Todo` denetleyicisinde`http://localhost:port/api/todo`:

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Diğer CRUD işlemleri uygulama

Ekleyeceğiz `Create`, `Update`, ve `Delete` denetleyiciye yöntemleri. I yalnızca kodu gösterir ve temel farklar vurgulayın bunlar bir tema çeşidi olduğundan. Ekleme veya kod değiştirdikten sonra projeyi oluşturun.

### <a name="create"></a>Create

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Bu belirttiği bir HTTP POST yöntemi olup [ `[HttpPost]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. [ `[FromBody]` ](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) Özniteliği HTTP isteği gövdesinden Yapılacaklar öğesi değerini almak için MVC söyler.

`CreatedAtRoute` Yöntemi yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıt 201 bir yanıt döndürür. `CreatedAtRoute`Ayrıca bir konum üstbilgisi yanıta ekler. Konum üstbilgisi yeni oluşturulan Yapılacaklar öğesi URI'sini belirtir. Bkz: [10.2.2 oluşturulan 201](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Postman oluşturma isteği göndermek için kullanın

* Uygulamayı başlatın (**Çalıştır > Başlat hata ayıklamaya**).
* Postman başlatın.

![Postman konsol](first-web-api/_static/pmc.png)

* HTTP yöntemini ayarlayın`POST`
* Seçin **gövde** radyo düğmesi
* Seçin **ham** radyo düğmesi
* Türü için JSON ayarlayın
* Anahtar-değer Düzenleyicisi'nde bir Yapılacaklar öğesi gibi girin

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* Seçin **Gönder**

* Alt bölme ve kopyalama Üstbilgileri sekmesini seçin **konumu** üstbilgisi:

![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmget.png)

Yeni oluşturduğunuz kaynağa erişmek için konum üstbilgisi URI kullanabilirsiniz. Geri çağırma `GetById` oluşturulan yöntemi `"GetTodo"` rota adlı:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a>Güncelleştirme

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

`Update`benzer `Create`, ancak HTTP PUT kullanır. Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). HTTP spec göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca farkları göndermek istemci gerektirir. Kısmi güncelleştirmeler desteklemek için HTTP PATCH kullanın.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![204 (No içerik) yanıt gösteren postman konsol](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Sil

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Yanıt [204 (No içerik)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![204 (No içerik) yanıt gösteren postman konsol](first-web-api/_static/pmd.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Denetleyici eylemleri için yönlendirme](xref:mvc/controllers/routing)
* API'nizi dağıtma hakkında daha fazla bilgi için bkz: [yayımlama ve dağıtım](../publishing/index.md).
* [Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))
* [Postman](https://www.getpostman.com/)
* [Fiddler](https://www.telerik.com/download/fiddler)
