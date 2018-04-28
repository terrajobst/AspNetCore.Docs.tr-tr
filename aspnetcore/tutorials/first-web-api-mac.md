---
title: Mac için ASP.NET Core ve Visual Studio ile Web API oluşturma
author: rick-anderson
description: Mac için ASP.NET Core MVC ve Visual Studio ile Web API oluşturma
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/27/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 9fca43e809720c22d87b963925bc009dc6c51831
ms.sourcegitcommit: 2ab550f8c46e1a8a5d45e58be44d151c676af256
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>Mac için ASP.NET Core ve Visual Studio ile Web API oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)

Bu öğreticide, "Yapılacaklar" öğeleri listesini yönetmek için bir web API'si oluşturma. Kullanıcı arabirimini oluşturulan değil.

Bu öğretici için üç sürümü vardır:

* macOS: Web API'si Mac (Bu öğretici) için Visual Studio ile
* Windows: [Web API'si Visual Studio ile Windows için](xref:tutorials/first-web-api)
* macOS, Linux, Windows: [Visual Studio Code ile Web API](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

Bkz: [macOS ASP.NET Core MVC ya da Linux giriş](xref:tutorials/first-mvc-app-xplat/index) kalıcı bir veritabanı kullanan bir örnek.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>Projeyi oluşturma

Visual Studio'dan seçin **dosya** > **yeni çözüm**.

![macOS yeni çözüm](first-web-api-mac/_static/sln.png)

Seçin **.NET Core uygulaması** > **ASP.NET Core Web API** > **sonraki**.

![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)

Girin *TodoApi* için **proje adı**ve ardından **oluşturma**.

![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Uygulamayı başlatın

Visual Studio'da seçin **çalıştırmak** > **Başlat ile hata ayıklama** uygulamayı başlatmak için. Visual Studio bir tarayıcı ile başlatarak `http://localhost:5000`. Bir HTTP 404 (bulunamadı) hata alıyorsunuz. URL'ye değiştirin `http://localhost:<port>/api/values`. `ValuesController` Veri görüntülenir:

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Entity Framework Çekirdek desteği ekleme

Yükleme [Entity Framework Çekirdek Inmemory](/ef/core/providers/in-memory/) veritabanı sağlayıcısı. Bu veritabanı sağlayıcısı bir bellek içi veritabanı ile kullanılacak Entity Framework Çekirdek sağlar.

* Gelen **proje** menüsünde, select **NuGet paketleri Ekle**.

  * Alternatif olarak, sağ tıklayarak **bağımlılıkları**ve ardından **paketleri Ekle**.

* Girin `EntityFrameworkCore.InMemory` arama kutusuna.
* Seçin `Microsoft.EntityFrameworkCore.InMemory`ve ardından **Paketi Ekle**.

### <a name="add-a-model-class"></a>Bir model sınıfı ekleme

Uygulamanızdaki verileri temsil eden bir nesne modelidir. Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.

Çözüm Gezgini'nde projeye sağ tıklayın. Seçin **ekleme** > **yeni klasör**. Klasör adı *modelleri*.

![Yeni klasör](first-web-api-mac/_static/folder.png)

> [!NOTE]
> Model sınıfları projenizde, herhangi bir yere koyabilirsiniz ancak *modelleri* klasörü, kural tarafından kullanılır.

Sağ *modelleri* klasörü ve select **Ekle** > **yeni dosya** > **genel**  >  **Boş sınıfı**. Sınıf adını *Todoıtem*ve ardından **yeni**.

Oluşturulan kod ile değiştirin:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.

### <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturma

*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır. Bu sınıf türetme tarafından oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.

Ekleme bir `TodoContext` sınıfının *modelleri* klasör.

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Denetleyici ekleme

Çözüm Gezgini'nde, içinde *denetleyicileri* klasörünü sınıfı ekleme `TodoController`.

Oluşturulan kod aşağıdakiyle değiştirin:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uygulamayı başlatın

Visual Studio'da seçin **çalıştırmak** > **Başlat ile hata ayıklama** uygulamayı başlatmak için. Visual Studio bir tarayıcı ile başlatarak `http://localhost:<port>`, burada `<port>` rastgele seçilen bağlantı noktası numarasıdır. Bir HTTP 404 (bulunamadı) hata alıyorsunuz. URL'ye değiştirin `http://localhost:<port>/api/values`. `ValuesController` Veri görüntülenir:

```json
["value1","value2"]
```

Gidin `Todo` denetleyicisinde `http://localhost:<port>/api/todo`:

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Diğer CRUD işlemleri uygulama

Ekleyeceğiz `Create`, `Update`, ve `Delete` denetleyiciye yöntemleri. Bu yöntemler ı yalnızca kodu gösterir ve temel farklar vurgulayın bir tema çeşidi olduğundan. Ekleme veya kod değiştirdikten sonra projeyi oluşturun.

### <a name="create"></a>Create

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Yukarıdaki yöntem tarafından belirtildiği gibi bir HTTP POST yanıt [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. [[FromBody]](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği HTTP isteği gövdesinden Yapılacaklar öğesi değerini almak için MVC söyler.
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Yukarıdaki yöntem tarafından belirtildiği gibi bir HTTP POST yanıt [[HttpPost]](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. MVC HTTP isteği gövdesinden Yapılacaklar öğesi değerini alır.
::: moniker-end

`CreatedAtRoute` Yöntemi 201 bir yanıt döndürür. Yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıt değil. `CreatedAtRoute` Ayrıca bir konum üstbilgisi yanıta ekler. Konum üstbilgisi yeni oluşturulan Yapılacaklar öğesi URI'sini belirtir. Bkz: [10.2.2 oluşturulan 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Postman oluşturma isteği göndermek için kullanın

* Uygulamayı başlatın (**çalıştırmak** > **Başlat hata ayıklamaya**).
* Postman açın.

![Postman konsol](first-web-api/_static/pmc.png)

* Localhost URL'sini bağlantı noktası numarasını güncelleştirin.
* HTTP yöntem kümesine *POST*.
* Tıklatın **gövde** sekmesi.
* Seçin **ham** radyo düğmesi.
* Türü kümesine *JSON (uygulama/json)*.
* Bir istek gövdesi aşağıdaki JSON benzeyen bir Yapılacaklar öğesi girin:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Tıklatın **Gönder** düğmesi.

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> Yanıt tıkladıktan sonra görüntülerse **Gönder**, devre dışı **SSL sertifika doğrulaması** seçeneği. Bu altında bulunan **dosya** > **ayarları**. Tıklatın **Gönder** daha sonra tekrar ayarı devre dışı bırakma düğmesi.
::: moniker-end

Tıklatın **üstbilgileri** sekmesinde **yanıt** bölmesinde ve kopyalama **konumu** üstbilgi değeri:

![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmc2.png)

Konum üstbilgisi URI oluşturduğunuz kaynağa erişmek için kullanabilirsiniz. `Create` Yöntemi döndürür [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_). Geçirilen ilk parametre `CreatedAtRoute` URL'yi oluşturmak için kullanılacak adlı rotayı temsil eder. Sözcüğünün `GetById` oluşturulan yöntemi `"GetTodo"` rota adlı:

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a>Güncelleştirme

::: moniker range="<= aspnetcore-2.0"
[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]
::: moniker-end

`Update` benzer `Create`, ancak HTTP PUT kullanır. Yanıt [204 (No içerik)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). HTTP spec göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca farkları göndermek istemci gerektirir. Kısmi güncelleştirmeler desteklemek için HTTP PATCH kullanın.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![204 (No içerik) yanıt gösteren postman konsol](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Sil

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Yanıt [204 (No içerik)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![204 (No içerik) yanıt gösteren postman konsol](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
