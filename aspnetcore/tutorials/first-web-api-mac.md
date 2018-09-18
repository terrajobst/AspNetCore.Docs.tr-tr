---
title: Mac için Visual Studio ile ASP.NET Core ile Web API'si oluşturma
author: rick-anderson
description: Mac için ASP.NET Core MVC ve Visual Studio ile Web API'si oluşturma
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 40f9bd9c57b97826edfddeb00cb4fb38a026d46e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011631"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a>Mac için Visual Studio ile ASP.NET Core ile Web API'si oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [Mike Wasson](https://github.com/mikewasson)

Bu öğreticide, bir "Yapılacaklar" öğelerinin listesi yönetmek için bir web API'si oluşturma. Kullanıcı Arabirimi oluşturulur değil.

Bu öğretici üç sürümü vardır:

* macOS: (Bu öğreticide) Mac için Visual Studio ile Web API'si
* Windows: [Windows için Visual Studio ile API Web](xref:tutorials/first-web-api)
* macOS, Linux, Windows: [Visual Studio Code ile Web API](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

Bkz: [ASP.NET Core MVC MacOS veya Linux giriş](xref:tutorials/first-mvc-app-xplat/index) kalıcı bir veritabanı kullanan bir örnek.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a>Projeyi oluşturma

Visual Studio'dan seçin **dosya** > **yeni çözüm**.

![Yeni çözüm macOS](first-web-api-mac/_static/sln.png)

Seçin **.NET Core uygulaması** > **ASP.NET Core Web API'sini** > **sonraki**.

![macOS yeni proje iletişim kutusu](first-web-api-mac/_static/1.png)

Girin *TodoApi* için **proje adı**ve ardından **Oluştur**.

![Yapılandırma iletişim kutusu](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a>Uygulamayı başlatın

Visual Studio'da **çalıştırmak** > **ile Start Debugging** uygulamayı başlatın. Visual Studio bir tarayıcı ile başlatarak `http://localhost:5000`. HTTP 404 (bulunamadı) hatası alırsınız. URL'yi `http://localhost:<port>/api/values`. `ValuesController` Veri görüntülenir:

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a>Entity Framework Core desteği eklendi

Yükleme [Entity Framework Core Inmemory](/ef/core/providers/in-memory/) veritabanı sağlayıcısı. Bu veritabanı sağlayıcısı, bir bellek içi veritabanı ile kullanılacak Entity Framework Core sağlar.

* Gelen **proje** menüsünde **NuGet paketleri Ekle**.

  * Alternatif olarak, sağ **bağımlılıkları**ve ardından **paketleri Ekle**.

* Girin `EntityFrameworkCore.InMemory` arama kutusuna.
* Seçin `Microsoft.EntityFrameworkCore.InMemory`ve ardından **Paketi Ekle**.

### <a name="add-a-model-class"></a>Bir model sınıfı ekleme

Uygulamanızda veri temsil eden bir nesne modelidir. Bu durumda, bir yapılacak iş öğesi tek model budur.

Çözüm Gezgini'nde projeye sağ tıklayın. Seçin **ekleme** > **yeni klasör**. Klasör adı *modelleri*.

![Yeni klasör](first-web-api-mac/_static/folder.png)

> [!NOTE]
> Model sınıfları, projenizdeki herhangi bir yere koyabilirsiniz ancak *modelleri* klasörü, kural olarak kullanılır.

Sağ *modelleri* klasörü ve select **Ekle** > **yeni dosya** > **genel**  >  **Boş sınıf**. Sınıf adı *Todoıtem*ve ardından **yeni**.

Oluşturulan kod ile değiştirin:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Veritabanı oluşturur `Id` olduğunda bir `TodoItem` oluşturulur.

### <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturur

*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordine eden ana sınıftır. Türeterek Bu sınıf oluşturduğunuz `Microsoft.EntityFrameworkCore.DbContext` sınıfı.

Ekleme bir `TodoContext` sınıfının *modelleri* klasör.

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a>Denetleyici ekleme

Çözüm Gezgini içinde *denetleyicileri* klasöründe sınıfı Ekle `TodoController`.

Oluşturulan kodu aşağıdakiyle değiştirin:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uygulamayı başlatın

Visual Studio'da **çalıştırmak** > **ile Start Debugging** uygulamayı başlatın. Visual Studio bir tarayıcı ile başlatarak `http://localhost:<port>`burada `<port>` bir rastgele seçilen bağlantı noktası numarasıdır. HTTP 404 (bulunamadı) hatası alırsınız. URL'yi `http://localhost:<port>/api/values`. `ValuesController` Veri görüntülenir:

```json
["value1","value2"]
```

Gidin `Todo` denetleyicisinde `http://localhost:<port>/api/todo`. Aşağıdaki JSON döndürülür:

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a>Bir CRUD işlemleri uygulama

Ekleyeceğiz `Create`, `Update`, ve `Delete` denetleyiciye yöntemleri. Ben yalnızca kodu gösterir ve temel farklar vurgulayın bu yöntemler bir tema çeşidi olduğundan. Ekleme veya değiştirme kod sonra projeyi derleyin.

### <a name="create"></a>Create

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Önceki yöntem tarafından belirtildiği gibi bir HTTP POST için yanıt [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) özniteliği HTTP isteği gövdesinden yapılacak iş öğesi değeri almak için MVC söyler.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

Önceki yöntem tarafından belirtildiği gibi bir HTTP POST için yanıt [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) özniteliği. MVC, HTTP isteği gövdesinden Yapılacaklar öğenin değerini alır.

::: moniker-end

`CreatedAtRoute` 201 yanıtında yöntemi döndürür. Yeni bir kaynak sunucuda oluşturan bir HTTP POST yöntemi için standart yanıttır. `CreatedAtRoute` Ayrıca bir konum üst bilgisi yanıta ekler. Location üst bilgisini, yeni oluşturulan yapılacak iş öğesi URI'sini belirtir. Bkz: [10.2.2 oluşturulan 201](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).

### <a name="use-postman-to-send-a-create-request"></a>Bir oluşturma isteği göndermek için Postman'ı kullanma

* Uygulamayı başlatın (**çalıştırma** > **Başlat hatalarını ayıklamaya**).
* Postman'i açın.

![Postman Konsolu](first-web-api/_static/pmc.png)

* Localhost URL'sini kullanılan bağlantı noktası numarasını güncelleştirin.
* HTTP yöntemi kümesine *POST*.
* Tıklayın **gövdesi** sekmesi.
* Seçin **ham** radyo düğmesi.
* Tür kümesine *JSON (application/json)*.
* İstek gövdesi aşağıdaki JSON benzeyen bir yapılacak iş öğesi ile girin:

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* Tıklayın **Gönder** düğmesi.

::: moniker range=">= aspnetcore-2.1"

> [!TIP]
> Yanıt tıklandıktan sonra görüntülerse **Gönder**, devre dışı **SSL sertifika doğrulama** seçeneği. Bunun altında bulunan **dosya** > **ayarları**. Tıklayın **Gönder** ayarı devre dışı bırakma sonra yeniden düğmesi.

::: moniker-end

Tıklayın **üstbilgileri** sekmesinde **yanıt** bölmesi ve kopyalama **konumu** üst bilgi değeri:

![Postman konsolunun üst bilgiler sekmesi](first-web-api/_static/pmc2.png)

URI konum üst bilgisi, oluşturduğunuz kaynağa erişmek için kullanabilirsiniz. `Create` Yöntemi döndürür [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_). Geçirilen ilk parametre `CreatedAtRoute` URL'yi oluşturmak için adlandırılmış bir rotayı temsil eder. Bu geri çağırma `GetById` oluşturulan yöntemi `"GetTodo"` rota adı:

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

`Update` benzer `Create`, ancak HTTP PUT kullanır. Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html). HTTP spec göre bir PUT İsteği tüm güncelleştirilmiş varlık yalnızca deltaları göndermek istemci gerektirir. Kısmi güncelleştirmeleri desteklemek için HTTP PATCH kullanın.

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/pmcput.png)

### <a name="delete"></a>Sil

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

Yanıt [204 (içerik yok)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).

![204 (içerik yok) yanıtı gösteren postman konsol](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
