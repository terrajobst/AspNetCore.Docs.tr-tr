---
title: Windows için ASP.NET Core ve Visual Studio ile Web API oluşturma
author: rick-anderson
description: ASP.NET Core MVC ve Windows için Visual Studio ile web API'si oluşturma
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api
ms.openlocfilehash: 1680d5e0be0f4844c904d923af30634c53bd1b81
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/15/2018
ms.locfileid: "34306679"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-windows"></a>Windows için ASP.NET Core ve Visual Studio ile Web API oluşturma

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT) ve [CAN Wasson](https://github.com/mikewasson)

Bu öğretici, "Yapılacaklar" öğeleri listesini yönetmek için bir web API oluşturur. Bir kullanıcı arabirimi (UI) oluşturulmaz.

Bu öğretici için üç sürümü vardır:

* Windows: Web API ile Windows için Visual Studio (Bu öğretici)
* macOS: [Mac için Visual Studio ile Web API](xref:tutorials/first-web-api-mac)
* macOS, Linux, Windows: [Visual Studio Code ile Web API](xref:tutorials/web-api-vsc)

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[intro to web API](../includes/webApi/intro.md)]

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE[](~/includes/net-core-prereqs-windows.md)]

## <a name="create-the-project"></a>Projeyi oluşturma

Visual Studio'da aşağıdaki adımları izleyin:

* Gelen **dosya** menüsünde, select **yeni** > **proje**.
* Seçin **ASP.NET çekirdek Web uygulaması** şablonu. Proje adı *TodoApi* tıklatıp **Tamam**.
* İçinde **yeni ASP.NET çekirdek Web uygulaması - TodoApi** iletişim kutusunda, ASP.NET Core sürümünü seçin. Seçin **API** şablonu ve tıklatın **Tamam**. Yapmak **değil** seçin **Docker desteğini etkinleştir**.

### <a name="launch-the-app"></a>Uygulamayı başlatın

Visual Studio'da uygulamayı başlatmak için CTRL + F5 tuşuna basın. Visual Studio bir tarayıcı ile başlatarak `http://localhost:<port>/api/values`, burada `<port>` rastgele seçilen bağlantı noktası numarasıdır. Chrome, Microsoft Edge ve Firefox aşağıdaki çıkış görüntüler:

```json
["value1","value2"]
```

Internet Explorer kullanıyorsanız, kaydetmeye istenir bir *values.json* dosya.

### <a name="add-a-model-class"></a>Bir model sınıfı ekleme

Uygulama verileri temsil eden bir nesne modelidir. Bu durumda, yalnızca bir Yapılacaklar öğesi modelidir.

Çözüm Gezgini'nde projeye sağ tıklayın. Seçin **ekleme** > **yeni klasör**. Klasör adı *modelleri*.

> [!NOTE]
> Model sınıfları herhangi bir yere projede gidebilirsiniz. *Modelleri* klasörü kural tarafından model sınıfları için kullanılır.

Çözüm Gezgini'nde sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**. Sınıf adını *Todoıtem* tıklatıp **Ekle**.

Güncelleştirme `TodoItem` aşağıdaki kodla sınıfı:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

Veritabanı oluşturur `Id` zaman bir `TodoItem` oluşturulur.

### <a name="create-the-database-context"></a>Veritabanı bağlamı oluşturma

*Veritabanı bağlamı* verilen veri modeli için Entity Framework işlevselliği koordinatları ana sınıftır. Bu sınıf türetme tarafından oluşturulan `Microsoft.EntityFrameworkCore.DbContext` sınıfı.

Çözüm Gezgini'nde sağ *modelleri* klasörü ve select **Ekle** > **sınıfı**. Sınıf adını *TodoContext* tıklatıp **Ekle**.

Sınıfı aşağıdaki kodla değiştirin:

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

### <a name="add-a-controller"></a>Denetleyici ekleme

Çözüm Gezgini'nde sağ *denetleyicileri* klasör. Seçin **ekleme** > **yeni öğe**. İçinde **Yeni Öğe Ekle** iletişim kutusunda **API denetleyicisi sınıfı** şablonu. Sınıf adını *TodoController*, tıklatıp **Ekle**.

![Yeni öğe iletişim denetleyicisiyle seçili arama kutusu ve web API denetleyicisi ekleyin](first-web-api/_static/new_controller.png)

Sınıfı aşağıdaki kodla değiştirin:

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a>Uygulamayı başlatın

Visual Studio'da uygulamayı başlatmak için CTRL + F5 tuşuna basın. Visual Studio bir tarayıcı ile başlatarak `http://localhost:<port>/api/values`, burada `<port>` rastgele seçilen bağlantı noktası numarasıdır. Gidin `Todo` denetleyicisinde `http://localhost:<port>/api/todo`.

[!INCLUDE[last part of web API](../includes/webApi/end.md)]

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
