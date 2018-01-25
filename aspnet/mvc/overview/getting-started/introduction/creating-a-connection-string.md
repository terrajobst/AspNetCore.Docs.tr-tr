---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: "Bağlantı dizesi oluşturma ve SQL Server yerel veritabanı ile çalışma | Microsoft Docs"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 25d1c1c9954baaca9ef91eff3dd3c853930a5893
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Bağlantı dizesi oluşturma ve SQL Server yerel veritabanı ile çalışma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Bağlantı dizesi oluşturma ve SQL Server yerel veritabanı ile çalışma

`MovieDBContext` Sınıfı, oluşturduğunuz görev veritabanına bağlanırken ve eşleme işler `Movie` veritabanı kayıtlarını nesnelere. Bir soru sorabilirsiniz. yine de bu bağlanacağı veritabanını belirtmek şeklidir. Gerçekten de kullanmak için veritabanını belirtmek zorunda kalmadan, Entity Framework varsayılan olarak kullanmaya [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). Bu bölümde açıkça bir bağlantı dizesinde ekleyeceğiz *Web.config* uygulamanın dosya.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[Yerel veritabanı](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) basit bir sürümü SQL Server Express Veritabanı Altyapısı'nın, isteğe bağlı olarak başlatılır ve kullanıcı modunda çalışır. Özel yürütme modunu veritabanları ile çalışmanıza olanak tanır SQL Server Express LocalDB çalışan *.mdf* dosyaları. Genellikle, yerel veritabanı veritabanı dosyaları saklanmaz *uygulama\_veri* bir web projesi klasörü.

SQL Server Express, üretim web uygulamalarında kullanım için önerilmez. IIS ile çalışmak üzere tasarlanmadı çünkü LocalDB özellikle bir web uygulaması ile üretim için kullanılmamalıdır. Ancak, bir LocalDB veritabanına kolayca SQL Server veya SQL Azure geçirilebilir.

Visual Studio 2017 içinde LocalDB Visual Studio ile varsayılan olarak yüklenir.

Varsayılan olarak, Entity Framework nesne bağlamı sınıf ile aynı adlı bir bağlantı dizesi arar (`MovieDBContext` bu proje için). Daha fazla bilgi için bkz: [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/library/jj653752.aspx).

Uygulama kök açmak *Web.config* aşağıda gösterilen dosya. (Değil *Web.config* dosyasını *görünümleri* klasörü.)

![](creating-a-connection-string/_static/image1.png)

Bul `<connectionStrings>` öğe:

![](creating-a-connection-string/_static/image2.png)

Aşağıdaki bağlantı dizesine eklemek `<connectionStrings>` öğesinde *Web.config* dosya.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

Aşağıdaki örnek, bir kısmı gösterir *Web.config* dosyasıyla eklenen yeni bağlantı dizesi:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

İki bağlantı dizesini çok benzer. İlk bağlantı dizesi adlı `DefaultConnection` ve üyelik veritabanı uygulama kimlerin erişebileceğini denetlemek için kullanılır. Adlı bir yerel veritabanı veritabanı eklediğiniz bağlantı dizesini belirtir *Movie.mdf* bulunan *uygulama\_veri* klasör. Biz olmaz üyelik veritabanının üyeliği, kimlik doğrulaması ve güvenlik hakkında daha fazla bilgi için Bu öğreticide, my öğretici bkz [auth ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Bağlantı dizesinin adını adı eşleşmelidir [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) sınıfı.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Gerçekten de eklemenize gerek yoktur `MovieDBContext` bağlantı dizesi. Bir bağlantı dizesi belirtmezseniz, Entity Framework LocalDB veritabanına tam adı ile kullanıcılar dizinde oluşturacak [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) sınıfı (Bu durumda `MvcMovie.Models.MovieDBContext`). Bunu olduğu sürece, veritabanı istediğiniz adı verebilirsiniz *. MDF* soneki. Örneğin, biz veritabanı adlandırabilirsiniz *MyFilms.mdf*.

Ardından, yeni oluşturacağınız `MoviesController` film verileri görüntülemek ve kullanıcıların yeni film listeleri oluşturmasına izin vermek için kullanabileceğiniz sınıfı.

>[!div class="step-by-step"]
[Önceki](adding-a-model.md)
[sonraki](accessing-your-models-data-from-a-controller.md)
