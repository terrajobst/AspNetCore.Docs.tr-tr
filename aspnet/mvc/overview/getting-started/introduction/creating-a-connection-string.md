---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Bağlantı dizesi oluşturma ve SQL Server LocalDB ile çalışma | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 73a3149c8c789221baa2e3804ebf15ed5a509ddf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756533"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="b0275-102">Bağlantı dizesi oluşturma ve SQL Server LocalDB ile çalışma</span><span class="sxs-lookup"><span data-stu-id="b0275-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="b0275-103">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="b0275-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="b0275-104">Bağlantı dizesi oluşturma ve SQL Server LocalDB ile çalışma</span><span class="sxs-lookup"><span data-stu-id="b0275-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="b0275-105">`MovieDBContext` Sınıfı, oluşturduğunuz veritabanına bağlanma ve eşleme görevi işler `Movie` veritabanı kayıtlarını nesneleri.</span><span class="sxs-lookup"><span data-stu-id="b0275-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="b0275-106">Bir soru sorabilirsiniz. ancak, listede bağlantı kurulacak veritabanını belirtmek şeklidir.</span><span class="sxs-lookup"><span data-stu-id="b0275-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="b0275-107">Gerçekten de kullanmak için veritabanını belirtmek zorunda kalmadan, Entity Framework kullanarak varsayılan [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="b0275-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="b0275-108">Bu bölümde açıkça bir bağlantı dizesinde ekleyeceğiz *Web.config* uygulamanın dosya.</span><span class="sxs-lookup"><span data-stu-id="b0275-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="b0275-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="b0275-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="b0275-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) SQL Server Express Veritabanı Altyapısı'nın, isteğe bağlı olarak başlar ve kullanıcı modunda çalışan basit bir sürümüdür.</span><span class="sxs-lookup"><span data-stu-id="b0275-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="b0275-111">Bir özel yürütme modu veritabanları ile çalışmanıza olanak tanır SQL Server Express LocalDB çalışan *.mdf* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="b0275-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="b0275-112">Genellikle, LocalDB veritabanı dosyaları saklanmaz *uygulama\_veri* web projesinin klasörüne.</span><span class="sxs-lookup"><span data-stu-id="b0275-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="b0275-113">SQL Server Express, üretim web uygulamalarında kullanım için önerilmez.</span><span class="sxs-lookup"><span data-stu-id="b0275-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="b0275-114">IIS ile çalışmak üzere tasarlanmamıştır, çünkü LocalDB özellikle bir web uygulaması ile üretim için kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="b0275-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="b0275-115">Ancak, bir LocalDB veritabanına kolayca SQL Server veya SQL Azure geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="b0275-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="b0275-116">Visual Studio 2017'de LocalDB, Visual Studio ile varsayılan olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="b0275-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="b0275-117">Varsayılan olarak Entity Framework nesne bağlamı sınıfı ile aynı adlı bir bağlantı dizesi arar. (`MovieDBContext` bu proje için).</span><span class="sxs-lookup"><span data-stu-id="b0275-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="b0275-118">Daha fazla bilgi için [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0275-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="b0275-119">Uygulama kökü açın *Web.config* aşağıda gösterilen dosya.</span><span class="sxs-lookup"><span data-stu-id="b0275-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="b0275-120">(Değil *Web.config* dosyası *görünümleri* klasör.)</span><span class="sxs-lookup"><span data-stu-id="b0275-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="b0275-121">Bulma `<connectionStrings>` öğesi:</span><span class="sxs-lookup"><span data-stu-id="b0275-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="b0275-122">Aşağıdaki bağlantı dizesi Ekle `<connectionStrings>` öğesinde *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="b0275-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="b0275-123">Aşağıdaki örnek bir bölümü gösterilmektedir *Web.config* dosyasıyla eklenen yeni bağlantı dizesi:</span><span class="sxs-lookup"><span data-stu-id="b0275-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="b0275-124">İki bağlantı dizelerine çok benzer.</span><span class="sxs-lookup"><span data-stu-id="b0275-124">The two connection strings are very similar.</span></span> <span data-ttu-id="b0275-125">İlk bağlantı dizesi adlı `DefaultConnection` ve uygulamaya erişebilmesi için Denetim üyelik veritabanına için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="b0275-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="b0275-126">Adlı bir LocalDB veritabanına eklediğiniz bağlantı dizesini belirtir *Movie.mdf* bulunan *uygulama\_veri* klasör.</span><span class="sxs-lookup"><span data-stu-id="b0275-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="b0275-127">Biz üyelik veritabanının üyelik, kimlik doğrulama ve güvenlik hakkında daha fazla bilgi için bu öğreticideki kullanın, my öğretici bkz olmaz [kimlik denetimi ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="b0275-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="b0275-128">Bağlantı dizesinin adını adıyla eşleşmelidir [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b0275-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="b0275-129">Gerçekten de eklemenize gerek yoktur `MovieDBContext` bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="b0275-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="b0275-130">Bir bağlantı dizesi belirtmezseniz, Entity Framework bir LocalDB veritabanına tam ada sahip kullanıcılar dizine oluşturacaktır [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) sınıfı (Bu durumda `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="b0275-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="b0275-131">Sahip olduğu sürece, veritabanı, istediğiniz şekilde adlandırabilirsiniz *. MDF* soneki.</span><span class="sxs-lookup"><span data-stu-id="b0275-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="b0275-132">Örneğin, biz veritabanı adlandırabilirsiniz *MyFilms.mdf*.</span><span class="sxs-lookup"><span data-stu-id="b0275-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="b0275-133">Ardından, yeni bir oluşturacaksınız `MoviesController` film verileri görüntülemek ve kullanıcıların yeni film listeleri oluşturmak için kullanabileceğiniz sınıfı.</span><span class="sxs-lookup"><span data-stu-id="b0275-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b0275-134">[Önceki](adding-a-model.md)
> [İleri](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="b0275-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
