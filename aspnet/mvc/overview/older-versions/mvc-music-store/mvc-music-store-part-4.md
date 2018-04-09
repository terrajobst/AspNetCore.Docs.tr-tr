---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: '4. Kısım: Modelleri ve veri erişimi | Microsoft Docs'
author: jongalloway
description: Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir. Bölüm 4 modelleri ve veri erişimi kapsar.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="ffa5b-104">4. Kısım: Modelleri ve veri erişimi</span><span class="sxs-lookup"><span data-stu-id="ffa5b-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="ffa5b-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="ffa5b-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="ffa5b-106">MVC müzik deposu tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağı hakkında adım adım anlatan öğretici bir uygulamadır.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="ffa5b-107">MVC müzik deposu çevrimiçi müzik albümlerini sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliği uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="ffa5b-108">Bu öğretici seri ASP.NET MVC müzik deposu örnek uygulaması oluşturmak için geçen tüm adımları ayrıntılarını verir.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="ffa5b-109">Bölüm 4 modelleri ve veri erişimi kapsar.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="ffa5b-110">"Şu ana kadar biz yalnızca kukla veri" bizim denetleyicilerinden görünüm şablonlarımız geçirme.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="ffa5b-111">Şimdi biz kanca gerçek bir veritabanını yedeklemek hazırsınız.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="ffa5b-112">Bu öğreticide şu SQL Server Compact (SQL CE olarak da adlandırılır) Edition kullanmayı bizim veritabanı altyapısı olarak kapsayan.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="ffa5b-113">SQL CE herhangi bir yükleme veya yerel geliştirme için gerçekten kolay hale getirir yapılandırma gerektirmeyen bir ücretsiz, katıştırılmış, dosya tabanlı veritabanıdır.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="ffa5b-114">Veritabanı erişimi ile Entity Framework kod-ilk</span><span class="sxs-lookup"><span data-stu-id="ffa5b-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="ffa5b-115">Sorgulamak ve veritabanını güncelleştirmek için ASP.NET MVC 3 projelerinde dahil Entity Framework (EF) destek kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="ffa5b-116">EF bir esnek (ORM) veri nesne yönelimli bir yolla bir veritabanında depolanan verileri sorgulamak veya güncelleştirme geliştiricilerinin API eşleme ilişkisel nesnesidir.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="ffa5b-117">Entity Framework sürüm 4 kod ilk olarak adlandırılan bir geliştirme standardı destekler.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="ffa5b-118">Kod ilk basit sınıfları (POCO "düz-eski" CLR nesnelerinden olarak da bilinir) yazarak model nesnesi oluşturmanıza olanak tanır ve veritabanı, sınıflardan kolay bir şekilde bile oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="ffa5b-119">Bizim modeli sınıfları yapılan değişiklikler</span><span class="sxs-lookup"><span data-stu-id="ffa5b-119">Changes to our Model Classes</span></span>

<span data-ttu-id="ffa5b-120">Biz Entity Framework veritabanı oluşturma özelliği Bu öğreticide yararlanarak.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="ffa5b-121">Bunu önce yine de birkaç küçük değişiklikler biz daha sonra kullanacağız bazı şeyleri eklemek için bizim model sınıflarına olalım.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="ffa5b-122">Sanatçı Model sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="ffa5b-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="ffa5b-123">Sanatçı açıklamak için basit model sınıfı ekleyeceğiz şekilde bizim albümleri Sanatçılar ile ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="ffa5b-124">Aşağıda gösterilen kodu kullanarak Artist.cs adlı modeller klasörü için yeni bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="ffa5b-125">Bizim modeli sınıfları güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="ffa5b-125">Updating our Model Classes</span></span>

<span data-ttu-id="ffa5b-126">Albüm sınıfı, aşağıda gösterildiği gibi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="ffa5b-127">Ardından, aşağıdaki güncelleştirmeleri Tarz sınıfına olun.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="ffa5b-128">Uygulama ekleme\_veri klasörü</span><span class="sxs-lookup"><span data-stu-id="ffa5b-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="ffa5b-129">Bir uygulama ekleyeceğiz\_Projemizin bizim SQL Server Express veritabanı dosyalarını tutmak için veri dizinine.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="ffa5b-130">Uygulama\_zaten veritabanı erişimi için doğru güvenlik erişim izinlerine sahip ASP.NET özel bir dizinde verilerdir.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="ffa5b-131">ASP.NET klasör ekleyin ve ardından uygulama proje menüsünden seçin\_veri.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="ffa5b-132">Web.config dosyasında bir bağlantı dizesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="ffa5b-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="ffa5b-133">Entity Framework bizim veritabanına bağlanmak bildiği böylece Web sitesinin yapılandırma dosyası için birkaç satır ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="ffa5b-134">Proje kök dizininde bulunan Web.config dosyasını çift tıklatın.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="ffa5b-135">Bu dosyanın sonuna kaydırın ve ekleme bir &lt;connectionStrings&gt; aşağıda gösterildiği gibi doğrudan son satırında bölüm.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="ffa5b-136">Bağlam sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="ffa5b-136">Adding a Context Class</span></span>

<span data-ttu-id="ffa5b-137">Modeller klasörü sağ tıklatın ve MusicStoreEntities.cs adlı yeni bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="ffa5b-138">Bu sınıf Entity Framework veritabanı bağlamı temsil eder ve bizim oluşturma işlemek, okuma, güncelleştirme ve silme işlemleri için bize.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="ffa5b-139">Bu sınıf için kod aşağıda verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="ffa5b-140">İşte bu kadar - hiçbir diğer yapılandırma, özel arabirimleri, vb. vardır. DbContext temel sınıf genişleterek, bizim MusicStoreEntities bizim veritabanı işlemleri için bize işleyebilen sınıftır.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="ffa5b-141">Biz, sayfaya olduğuna göre birkaç daha fazla özellik bazı ek bilgiler bizim veritabanında yararlanmak için bizim model sınıflarına ekleyelim.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="ffa5b-142">Bizim mağazası Kataloğu veri ekleme</span><span class="sxs-lookup"><span data-stu-id="ffa5b-142">Adding our store catalog data</span></span>

<span data-ttu-id="ffa5b-143">Biz bir özellik için yeni oluşturulan bir veritabanı "seed" veri ekleyen Entity Framework içinde yararlanır.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="ffa5b-144">Bu bizim mağazası kataloğu ve türler, sanatçılar, Albümler listesiyle önceden doldurur.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="ffa5b-145">-, Bu öğreticide daha önce kullanılan bizim site tasarımı dosyaları dahil - MvcMusicStore Assets.zip indirme kodu adlı bir klasörde bulunan bu çekirdek verilerle bir sınıf dosyası vardır.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="ffa5b-146">Kod içinde / modeller klasörü SampleData.cs dosyasını bulun ve aşağıda gösterildiği gibi bizim proje modelleri klasörüne bırakın.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="ffa5b-147">Şimdi biz bir Entity Framework o SampleData sınıfı hakkında söylemeniz kod satırı eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="ffa5b-148">Dosyayı açın ve aşağıdaki uygulama en çok satır eklemek için projenin kök Global.asax dosyasına çift tıklayın\_Başlat yöntemi.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="ffa5b-149">Bu noktada, sizi Projemizin için Entity Framework yapılandırmak için gerekli iş tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="ffa5b-150">Veritabanı sorgulama</span><span class="sxs-lookup"><span data-stu-id="ffa5b-150">Querying the Database</span></span>

<span data-ttu-id="ffa5b-151">Şimdi şimdi "veri kukla" kullanmak yerine, bunun yerine, bilgilerin tümünü sorgulamak için Veritabanımıza çağırır bizim StoreController güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="ffa5b-152">Üzerinde bir alan bildirerek başlayacağız **StoreController** storeDB adlı MusicStoreEntities sınıfının bir örneği tutmak için:</span><span class="sxs-lookup"><span data-stu-id="ffa5b-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="ffa5b-153">Veritabanını sorgulamak için depolama dizini güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="ffa5b-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="ffa5b-154">MusicStoreEntities sınıf Entity Framework tarafından korunur ve Veritabanımıza her tablo için bir koleksiyon özelliği sunar.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="ffa5b-155">Şimdi, veritabanındaki tüm türler almak için bizim StoreController'ın dizin eylem güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="ffa5b-156">Daha önce bu kodlama sabit dize verilerini tarafından yaptığımız.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="ffa5b-157">Şimdi biz yalnızca Entity Framework bağlamına Generes koleksiyonu kullanabilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="ffa5b-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="ffa5b-158">Biz yine biz yalnızca dinamik bizim veritabanından şimdi veriyor önce - biz döndürülen aynı StoreIndexViewModel döndürme bu yana bizim görünüm şablonu olmasını hiçbir değişiklik gerekir.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="ffa5b-159">Projeyi tekrar çalıştırın ve "/ depolamak" URL'yi ziyaret şimdi tüm türler listesini bizim veritabanında göreceğiz:</span><span class="sxs-lookup"><span data-stu-id="ffa5b-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="ffa5b-160">Deposu gözatmak ve ayrıntıları canlı verileri kullanmak için güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="ffa5b-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="ffa5b-161">/ Deposu/Gözat ile? Tarz =*[Tarz bazı]* eylem yöntemi, biz aramakta olduğunuz bir tarzını için ada göre.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="ffa5b-162">Yalnızca bir sonuç, biz şimdiye kadar aynı Tarz ad için iki giriş olmamalıdır beri ve biz kullanabilmeniz için bekliyoruz. LINQ Sorgu şöyle uygun türe nesne için Single() uzantısı (Bu henüz yazmayın):</span><span class="sxs-lookup"><span data-stu-id="ffa5b-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="ffa5b-163">Tek yöntem bir Lambda ifadesi sağlayacak şekilde, adı biz tanımladığınız değeri ile eşleşen tek bir tarzını nesne istediğimizi belirten bir parametre olarak alır.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="ffa5b-164">Yukarıdaki durumda biz DISCO eşleşen bir ad değeri ile tek bir tarzını nesne yüklüyorsunuz.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="ffa5b-165">Biz, bize Tarz nesnesi alınırken de yüklenen istiyoruz ilgili diğer varlıklar belirtmek izin veren bir Entity Framework özelliğinin avantajlarından yararlanmak.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="ffa5b-166">Bu özellik, sorgu sonucu şekillendirme olarak adlandırılır ve bize tüm ihtiyacımız bilgileri almak için veritabanına erişmek için ihtiyacımız sayısını azaltmak sağlar.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="ffa5b-167">İlgili Albümler istiyoruz belirtmek için Genres.Include("Albums") içerecek şekilde sorgumuzu güncelleştiriyoruz böylece alıyoruz, tarz albümleri önceden getirme istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="ffa5b-168">Tek veritabanı isteği bizim tarz ve albüm verileri alacak beri bu daha verimli olur.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="ffa5b-169">Açıklamaları ile göz önünden, İşte bizim güncelleştirilmiş Gözat denetleyici eylemi nasıl göründüğünü:</span><span class="sxs-lookup"><span data-stu-id="ffa5b-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="ffa5b-170">Biz her bir tarzını kullanılabilir olan albümleri görüntülemek için deposu Gözat görünüm şimdi güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="ffa5b-171">Görünüm şablonu (bulunan /Views/Store/Browse.cshtml) açın ve madde işaretli albümleri listesini aşağıda gösterildiği gibi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="ffa5b-172">Uygulamamız çalıştırma ve tarama/deposu/için Gözat? Tarz = bizim sonuçları şimdi tüm albümleri bizim seçili Tarz görüntüleme veritabanından alınır Jazz gösterir.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="ffa5b-173">Bizim/Store/Ayrıntılar / [kimlik] URL'yi değiştirin ve sahte verilerimizi Kimliğine parametre değeri ile eşleşen bir albümü yükleyen bir veritabanı sorgusu ile Değiştir'de aynı vermiyoruz.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="ffa5b-174">Uygulamamızı çalıştırmak ve /Store/Details/1 için tarama sonuçları veritabanından şimdi çekilen olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="ffa5b-175">Albüm albüm Kimliğine göre görüntülemek için deposu ayrıntıları sayfamızı ayarlayın, şimdi güncelleştirme **Gözat** Ayrıntılar görünümü bağlamak için görünümü.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="ffa5b-176">Tam olarak deposu dizinden deposu gözatmak için önceki bölümde sonunda bağlamak için yaptığımız gibi Html.ActionLink, kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="ffa5b-177">Gözat görünüm için tam kaynak aşağıda yer almaktadır.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="ffa5b-178">Biz şimdi deposu sayfamızı kullanılabilir albümleri listeleyen bir tarzını sayfasına göz ve bir albümü tıklayarak Biz bu Albüm ayrıntılarını görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="ffa5b-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="ffa5b-179">[Önceki](mvc-music-store-part-3.md)
> [sonraki](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="ffa5b-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
