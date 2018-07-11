---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Bağlantı dizesi oluşturma ve SQL Server LocalDB ile çalışma | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 8c8db3fc121b2ff263ab033404211e167e19cd71
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38168382"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Bağlantı dizesi oluşturma ve SQL Server LocalDB ile çalışma
====================
Tarafından [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Bağlantı dizesi oluşturma ve SQL Server LocalDB ile çalışma

`MovieDBContext` Sınıfı, oluşturduğunuz veritabanına bağlanma ve eşleme görevi işler `Movie` veritabanı kayıtlarını nesneleri. Bir soru sorabilirsiniz. ancak, listede bağlantı kurulacak veritabanını belirtmek şeklidir. Gerçekten de kullanmak için veritabanını belirtmek zorunda kalmadan, Entity Framework kullanarak varsayılan [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). Bu bölümde açıkça bir bağlantı dizesinde ekleyeceğiz *Web.config* uygulamanın dosya.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) SQL Server Express Veritabanı Altyapısı'nın, isteğe bağlı olarak başlar ve kullanıcı modunda çalışan basit bir sürümüdür. Bir özel yürütme modu veritabanları ile çalışmanıza olanak tanır SQL Server Express LocalDB çalışan *.mdf* dosyaları. Genellikle, LocalDB veritabanı dosyaları saklanmaz *uygulama\_veri* web projesinin klasörüne.

SQL Server Express, üretim web uygulamalarında kullanım için önerilmez. IIS ile çalışmak üzere tasarlanmamıştır, çünkü LocalDB özellikle bir web uygulaması ile üretim için kullanılmamalıdır. Ancak, bir LocalDB veritabanına kolayca SQL Server veya SQL Azure geçirilebilir.

Visual Studio 2017'de LocalDB, Visual Studio ile varsayılan olarak yüklenir.

Varsayılan olarak Entity Framework nesne bağlamı sınıfı ile aynı adlı bir bağlantı dizesi arar. (`MovieDBContext` bu proje için). Daha fazla bilgi için [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/library/jj653752.aspx).

Uygulama kökü açın *Web.config* aşağıda gösterilen dosya. (Değil *Web.config* dosyası *görünümleri* klasör.)

![](creating-a-connection-string/_static/image1.png)

Bulma `<connectionStrings>` öğesi:

![](creating-a-connection-string/_static/image2.png)

Aşağıdaki bağlantı dizesi Ekle `<connectionStrings>` öğesinde *Web.config* dosya.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

Aşağıdaki örnek bir bölümü gösterilmektedir *Web.config* dosyasıyla eklenen yeni bağlantı dizesi:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

İki bağlantı dizelerine çok benzer. İlk bağlantı dizesi adlı `DefaultConnection` ve uygulamaya erişebilmesi için Denetim üyelik veritabanına için kullanılır. Adlı bir LocalDB veritabanına eklediğiniz bağlantı dizesini belirtir *Movie.mdf* bulunan *uygulama\_veri* klasör. Biz üyelik veritabanının üyelik, kimlik doğrulama ve güvenlik hakkında daha fazla bilgi için bu öğreticideki kullanın, my öğretici bkz olmaz [kimlik denetimi ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

Bağlantı dizesinin adını adıyla eşleşmelidir [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) sınıfı.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Gerçekten de eklemenize gerek yoktur `MovieDBContext` bağlantı dizesi. Bir bağlantı dizesi belirtmezseniz, Entity Framework bir LocalDB veritabanına tam ada sahip kullanıcılar dizine oluşturacaktır [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) sınıfı (Bu durumda `MvcMovie.Models.MovieDBContext`). Sahip olduğu sürece, veritabanı, istediğiniz şekilde adlandırabilirsiniz *. MDF* soneki. Örneğin, biz veritabanı adlandırabilirsiniz *MyFilms.mdf*.

Ardından, yeni bir oluşturacaksınız `MoviesController` film verileri görüntülemek ve kullanıcıların yeni film listeleri oluşturmak için kullanabileceğiniz sınıfı.

> [!div class="step-by-step"]
> [Önceki](adding-a-model.md)
> [İleri](accessing-your-models-data-from-a-controller.md)
