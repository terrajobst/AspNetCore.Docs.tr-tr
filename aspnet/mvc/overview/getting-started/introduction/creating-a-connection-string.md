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
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="2cd81-102">Bağlantı dizesi oluşturma ve SQL Server yerel veritabanı ile çalışma</span><span class="sxs-lookup"><span data-stu-id="2cd81-102">Creating a Connection String and Working with SQL Server LocalDB</span></span>
====================
<span data-ttu-id="2cd81-103">Tarafından [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="2cd81-103">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="2cd81-104">Bağlantı dizesi oluşturma ve SQL Server yerel veritabanı ile çalışma</span><span class="sxs-lookup"><span data-stu-id="2cd81-104">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="2cd81-105">`MovieDBContext` Sınıfı, oluşturduğunuz görev veritabanına bağlanırken ve eşleme işler `Movie` veritabanı kayıtlarını nesnelere.</span><span class="sxs-lookup"><span data-stu-id="2cd81-105">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="2cd81-106">Bir soru sorabilirsiniz. yine de bu bağlanacağı veritabanını belirtmek şeklidir.</span><span class="sxs-lookup"><span data-stu-id="2cd81-106">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="2cd81-107">Gerçekten de kullanmak için veritabanını belirtmek zorunda kalmadan, Entity Framework varsayılan olarak kullanmaya [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="2cd81-107">You don't actually have to specify which database to use, Entity Framework will default to using [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="2cd81-108">Bu bölümde açıkça bir bağlantı dizesinde ekleyeceğiz *Web.config* uygulamanın dosya.</span><span class="sxs-lookup"><span data-stu-id="2cd81-108">In this section we'll explicitly add a connection string in the *Web.config* file of the application.</span></span>

## <a name="sql-server-express-localdb"></a><span data-ttu-id="2cd81-109">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="2cd81-109">SQL Server Express LocalDB</span></span>

<span data-ttu-id="2cd81-110">[Yerel veritabanı](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) basit bir sürümü SQL Server Express Veritabanı Altyapısı'nın, isteğe bağlı olarak başlatılır ve kullanıcı modunda çalışır.</span><span class="sxs-lookup"><span data-stu-id="2cd81-110">[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) is a lightweight version of the SQL Server Express Database Engine that starts on demand and runs in user mode.</span></span> <span data-ttu-id="2cd81-111">Özel yürütme modunu veritabanları ile çalışmanıza olanak tanır SQL Server Express LocalDB çalışan *.mdf* dosyaları.</span><span class="sxs-lookup"><span data-stu-id="2cd81-111">LocalDB runs in a special execution mode of SQL Server Express that enables you to work with databases as *.mdf* files.</span></span> <span data-ttu-id="2cd81-112">Genellikle, yerel veritabanı veritabanı dosyaları saklanmaz *uygulama\_veri* bir web projesi klasörü.</span><span class="sxs-lookup"><span data-stu-id="2cd81-112">Typically, LocalDB database files are kept in the *App\_Data* folder of a web project.</span></span>

<span data-ttu-id="2cd81-113">SQL Server Express, üretim web uygulamalarında kullanım için önerilmez.</span><span class="sxs-lookup"><span data-stu-id="2cd81-113">SQL Server Express is not recommended for use in production web applications.</span></span> <span data-ttu-id="2cd81-114">IIS ile çalışmak üzere tasarlanmadı çünkü LocalDB özellikle bir web uygulaması ile üretim için kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2cd81-114">LocalDB in particular should not be used for production with a web application because it is not designed to work with IIS.</span></span> <span data-ttu-id="2cd81-115">Ancak, bir LocalDB veritabanına kolayca SQL Server veya SQL Azure geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="2cd81-115">However, a LocalDB database can be easily migrated to SQL Server or SQL Azure.</span></span>

<span data-ttu-id="2cd81-116">Visual Studio 2017 içinde LocalDB Visual Studio ile varsayılan olarak yüklenir.</span><span class="sxs-lookup"><span data-stu-id="2cd81-116">In Visual Studio 2017, LocalDB is installed by default with Visual Studio.</span></span>

<span data-ttu-id="2cd81-117">Varsayılan olarak, Entity Framework nesne bağlamı sınıf ile aynı adlı bir bağlantı dizesi arar (`MovieDBContext` bu proje için).</span><span class="sxs-lookup"><span data-stu-id="2cd81-117">By default, the Entity Framework looks for a connection string named the same as the object context class (`MovieDBContext` for this project).</span></span> <span data-ttu-id="2cd81-118">Daha fazla bilgi için bkz: [ASP.NET Web uygulamaları için SQL Server bağlantı dizelerini](https://msdn.microsoft.com/library/jj653752.aspx).</span><span class="sxs-lookup"><span data-stu-id="2cd81-118">For more information see [SQL Server Connection Strings for ASP.NET Web Applications](https://msdn.microsoft.com/library/jj653752.aspx).</span></span>

<span data-ttu-id="2cd81-119">Uygulama kök açmak *Web.config* aşağıda gösterilen dosya.</span><span class="sxs-lookup"><span data-stu-id="2cd81-119">Open the application root *Web.config* file shown below.</span></span> <span data-ttu-id="2cd81-120">(Değil *Web.config* dosyasını *görünümleri* klasörü.)</span><span class="sxs-lookup"><span data-stu-id="2cd81-120">(Not the *Web.config* file in the *Views* folder.)</span></span>

![](creating-a-connection-string/_static/image1.png)

<span data-ttu-id="2cd81-121">Bul `<connectionStrings>` öğe:</span><span class="sxs-lookup"><span data-stu-id="2cd81-121">Find the `<connectionStrings>` element:</span></span>

![](creating-a-connection-string/_static/image2.png)

<span data-ttu-id="2cd81-122">Aşağıdaki bağlantı dizesine eklemek `<connectionStrings>` öğesinde *Web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="2cd81-122">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

<span data-ttu-id="2cd81-123">Aşağıdaki örnek, bir kısmı gösterir *Web.config* dosyasıyla eklenen yeni bağlantı dizesi:</span><span class="sxs-lookup"><span data-stu-id="2cd81-123">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

<span data-ttu-id="2cd81-124">İki bağlantı dizesini çok benzer.</span><span class="sxs-lookup"><span data-stu-id="2cd81-124">The two connection strings are very similar.</span></span> <span data-ttu-id="2cd81-125">İlk bağlantı dizesi adlı `DefaultConnection` ve üyelik veritabanı uygulama kimlerin erişebileceğini denetlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2cd81-125">The first connection string is named `DefaultConnection` and is used for the membership database to control who can access the application.</span></span> <span data-ttu-id="2cd81-126">Adlı bir yerel veritabanı veritabanı eklediğiniz bağlantı dizesini belirtir *Movie.mdf* bulunan *uygulama\_veri* klasör.</span><span class="sxs-lookup"><span data-stu-id="2cd81-126">The connection string you've added specifies a LocalDB database named *Movie.mdf* located in the *App\_Data* folder.</span></span> <span data-ttu-id="2cd81-127">Biz olmaz üyelik veritabanının üyeliği, kimlik doğrulaması ve güvenlik hakkında daha fazla bilgi için Bu öğreticide, my öğretici bkz [auth ve SQL DB ile bir ASP.NET MVC uygulaması oluşturma ve Azure App Service'e dağıtma](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="2cd81-127">We won't use the membership database in this tutorial, for more information on membership, authentication and security, see my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span>

<span data-ttu-id="2cd81-128">Bağlantı dizesinin adını adı eşleşmelidir [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2cd81-128">The name of the connection string must match the name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class.</span></span>

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

<span data-ttu-id="2cd81-129">Gerçekten de eklemenize gerek yoktur `MovieDBContext` bağlantı dizesi.</span><span class="sxs-lookup"><span data-stu-id="2cd81-129">You don't actually need to add the `MovieDBContext` connection string.</span></span> <span data-ttu-id="2cd81-130">Bir bağlantı dizesi belirtmezseniz, Entity Framework LocalDB veritabanına tam adı ile kullanıcılar dizinde oluşturacak [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) sınıfı (Bu durumda `MvcMovie.Models.MovieDBContext`).</span><span class="sxs-lookup"><span data-stu-id="2cd81-130">If you don't specify a connection string, Entity Framework will create a LocalDB database in the users directory with the fully qualified name of the [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) class (in this case `MvcMovie.Models.MovieDBContext`).</span></span> <span data-ttu-id="2cd81-131">Bunu olduğu sürece, veritabanı istediğiniz adı verebilirsiniz *. MDF* soneki.</span><span class="sxs-lookup"><span data-stu-id="2cd81-131">You can name the database anything you like, as long as it has the *.MDF* suffix.</span></span> <span data-ttu-id="2cd81-132">Örneğin, biz veritabanı adlandırabilirsiniz *MyFilms.mdf*.</span><span class="sxs-lookup"><span data-stu-id="2cd81-132">For example, we could name the database *MyFilms.mdf*.</span></span>

<span data-ttu-id="2cd81-133">Ardından, yeni oluşturacağınız `MoviesController` film verileri görüntülemek ve kullanıcıların yeni film listeleri oluşturmasına izin vermek için kullanabileceğiniz sınıfı.</span><span class="sxs-lookup"><span data-stu-id="2cd81-133">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2cd81-134">[Önceki](adding-a-model.md)
[sonraki](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="2cd81-134">[Previous](adding-a-model.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
