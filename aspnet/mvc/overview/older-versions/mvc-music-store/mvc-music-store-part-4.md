---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: '4. Bölüm: Modeller ve veri erişimi | Microsoft Docs'
author: jongalloway
description: Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır. 4. Bölüm modeller ve veri erişimi kapsar.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 6a07bf6c8a3fb926ae25fe1f6c9359e64cd7a290
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751940"
---
<a name="part-4-models-and-data-access"></a><span data-ttu-id="4c7b1-104">4. Bölüm: Modeller ve veri erişimi</span><span class="sxs-lookup"><span data-stu-id="4c7b1-104">Part 4: Models and Data Access</span></span>
====================
<span data-ttu-id="4c7b1-105">tarafından [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="4c7b1-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="4c7b1-106">MVC müzik Store tanıtır ve ASP.NET MVC ve Visual Studio web geliştirme için nasıl kullanılacağını adım adım anlatan bir öğretici uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="4c7b1-107">MVC müzik Store müzik albümleri çevrimiçi sattığı ve temel site yönetimi, kullanıcı oturum açma ve alışveriş sepeti işlevselliğini uygulayan bir Basit örnek deposu uygulamasıdır.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>
> 
> <span data-ttu-id="4c7b1-108">Bu öğretici serisinde ASP.NET MVC müzik Store örnek uygulamayı oluşturmak için gerçekleştirilen tüm adımları ayrıntılı olarak açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="4c7b1-109">4. Bölüm modeller ve veri erişimi kapsar.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-109">Part 4 covers Models and Data Access.</span></span>


<span data-ttu-id="4c7b1-110">"Şu ana kadar biz yalnızca işlevsiz veri" bizim denetleyicilerinden görünümü şablonlarımızı geçirme.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-110">So far, we've just been passing "dummy data" from our Controllers to our View templates.</span></span> <span data-ttu-id="4c7b1-111">Şimdi gerçek bir veritabanını kanca hazırız.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-111">Now we're ready to hook up a real database.</span></span> <span data-ttu-id="4c7b1-112">Bu öğreticide size SQL Server Compact (SQL CE olarak da adlandırılır) Edition kullanmayı veritabanı altyapımız kapsar.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-112">In this tutorial we'll be covering how to use SQL Server Compact Edition (often called SQL CE) as our database engine.</span></span> <span data-ttu-id="4c7b1-113">SQL CE herhangi bir yükleme veya yerel geliştirme için gerçekten kullanışlı kılan yapılandırma gerektirmeyen bir ücretsiz, katıştırılmış, dosya tabanlı veritabanıdır.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-113">SQL CE is a free, embedded, file based database that doesn't require any installation or configuration, which makes it really convenient for local development.</span></span>

## <a name="database-access-with-entity-framework-code-first"></a><span data-ttu-id="4c7b1-114">Veritabanı erişimi ile Entity Framework Code-First</span><span class="sxs-lookup"><span data-stu-id="4c7b1-114">Database access with Entity Framework Code-First</span></span>

<span data-ttu-id="4c7b1-115">Sorgulamak ve veritabanını güncellemek için ASP.NET MVC 3 projeleri dahil Entity Framework (EF) destek kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-115">We'll use the Entity Framework (EF) support that is included in ASP.NET MVC 3 projects to query and update the database.</span></span> <span data-ttu-id="4c7b1-116">EF bir esnek nesne ilişkisel eşleme (ORM) veri geliştiricilerinin nesne yönelimli bir şekilde bir veritabanında depolanan verileri sorgu ve güncelleştirme API ' dir.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-116">EF is a flexible object relational mapping (ORM) data API that enables developers to query and update data stored in a database in an object-oriented way.</span></span>

<span data-ttu-id="4c7b1-117">Entity Framework sürüm 4 kod öncelikli olarak adlandırılan bir geliştirme paradigma destekler.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-117">Entity Framework version 4 supports a development paradigm called code-first.</span></span> <span data-ttu-id="4c7b1-118">İlk kod model nesnesi basit sınıfları (POCO "düz eski" CLR nesne olarak da bilinir) yazarak oluşturmanıza olanak sağlar ve veritabanı, sınıflardan hareket halindeyken bile oluşturabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-118">Code-first allows you to create model object by writing simple classes (also known as POCO from "plain-old" CLR objects), and can even create the database on the fly from your classes.</span></span>

### <a name="changes-to-our-model-classes"></a><span data-ttu-id="4c7b1-119">Bizim Model sınıfları değişiklikler</span><span class="sxs-lookup"><span data-stu-id="4c7b1-119">Changes to our Model Classes</span></span>

<span data-ttu-id="4c7b1-120">Biz Entity Framework veritabanı oluşturma özelliği Bu öğreticide yararlanarak.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-120">We will be leveraging the database creation feature in Entity Framework in this tutorial.</span></span> <span data-ttu-id="4c7b1-121">Bunu yapmadan önce birkaç küçük değişiklikler daha sonra kullanacağız bazı şeyler eklemek için sunduğumuz model sınıflarına olalım.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-121">Before we do that, though, let's make a few minor changes to our model classes to add in some things we'll be using later on.</span></span>

#### <a name="adding-the-artist-model-classes"></a><span data-ttu-id="4c7b1-122">Sanatçının Model sınıfları ekleme</span><span class="sxs-lookup"><span data-stu-id="4c7b1-122">Adding the Artist Model Classes</span></span>

<span data-ttu-id="4c7b1-123">Bir Sanatçıdan açıklamak için basit bir model sınıfı ekleyeceğiz. Bu nedenle bizim albümleri Sanatçılar ile ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-123">Our Albums will be associated with Artists, so we'll add a simple model class to describe an Artist.</span></span> <span data-ttu-id="4c7b1-124">Aşağıda gösterilen kodu kullanarak Artist.cs adlı modelleri klasörüne yeni bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-124">Add a new class to the Models folder named Artist.cs using the code shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a><span data-ttu-id="4c7b1-125">Bizim Model sınıfları güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="4c7b1-125">Updating our Model Classes</span></span>

<span data-ttu-id="4c7b1-126">Albüm sınıfı, aşağıda gösterildiği gibi güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-126">Update the Album class as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

<span data-ttu-id="4c7b1-127">Ardından, tarz sınıfına aşağıdaki güncelleştirmeleri yapın.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-127">Next, make the following updates to the Genre class.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a><span data-ttu-id="4c7b1-128">Uygulama ekleme\_veri klasörü</span><span class="sxs-lookup"><span data-stu-id="4c7b1-128">Adding the App\_Data folder</span></span>

<span data-ttu-id="4c7b1-129">Bir uygulama ekleyeceğiz\_Projemizin bizim SQL Server Express veritabanı dosyaları için veri dizini.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-129">We'll add an App\_Data directory to our project to hold our SQL Server Express database files.</span></span> <span data-ttu-id="4c7b1-130">Uygulama\_veritabanı erişimi için doğru güvenlik erişim izinleri olan ASP.NET özel bir dizinde verilerdir.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-130">App\_Data is a special directory in ASP.NET which already has the correct security access permissions for database access.</span></span> <span data-ttu-id="4c7b1-131">ASP.NET klasör ekleyin ve ardından uygulama proje menüsünden seçin\_veri.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-131">From the Project menu, select Add ASP.NET Folder, then App\_Data.</span></span>

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a><span data-ttu-id="4c7b1-132">Web.config dosyasında bağlantı dizesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="4c7b1-132">Creating a Connection String in the web.config file</span></span>

<span data-ttu-id="4c7b1-133">Entity Framework bizim veritabanına bağlanmak nasıl bilebilmesi Web sitesinin yapılandırma dosyası için birkaç satır kod ekleyeceğiz.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-133">We will add a few lines to the website's configuration file so that Entity Framework knows how to connect to our database.</span></span> <span data-ttu-id="4c7b1-134">Proje kök dizininde bulunan Web.config dosyasını çift tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-134">Double-click on the Web.config file located in the root of the project.</span></span>

![](mvc-music-store-part-4/_static/image2.png)

<span data-ttu-id="4c7b1-135">Bu dosyanın alt kısma kaydırın ve ekleme bir &lt;connectionStrings&gt; aşağıda gösterildiği gibi doğrudan son satırında bölümü.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-135">Scroll to the bottom of this file and add a &lt;connectionStrings&gt; section directly above the last line, as shown below.</span></span>

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a><span data-ttu-id="4c7b1-136">Bağlam sınıfı ekleme</span><span class="sxs-lookup"><span data-stu-id="4c7b1-136">Adding a Context Class</span></span>

<span data-ttu-id="4c7b1-137">Modeller klasörü sağ tıklatın ve MusicStoreEntities.cs adlı yeni bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-137">Right-click the Models folder and add a new class named MusicStoreEntities.cs.</span></span>

![](mvc-music-store-part-4/_static/image3.png)

<span data-ttu-id="4c7b1-138">Bu sınıf Entity Framework veritabanı bağlamı temsil eder ve bizim Oluştur işlemek, okuma, güncelleştirme ve silme işlemleri bizim için.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-138">This class will represent the Entity Framework database context, and will handle our create, read, update, and delete operations for us.</span></span> <span data-ttu-id="4c7b1-139">Bu sınıfın kodu, aşağıda gösterilmiştir.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-139">The code for this class is shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

<span data-ttu-id="4c7b1-140">İşte bu kadar - hiçbir diğer yapılandırma, özel arabirimler vb. yoktur. DbContext temel sınıfını genişleterek, bizim MusicStoreEntities sınıfı bizim için veritabanı işlemlerimiz işleyebilir.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-140">That's it - there's no other configuration, special interfaces, etc. By extending the DbContext base class, our MusicStoreEntities class is able to handle our database operations for us.</span></span> <span data-ttu-id="4c7b1-141">Şimdi, ölçekledikçe ayarladığımıza göre bizim model sınıflarına veritabanımızda yer bazı ek bilgiler avantajlarından yararlanmak için birkaç daha fazla özellik ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-141">Now that we've got that hooked up, let's add a few more properties to our model classes to take advantage of some of the additional information in our database.</span></span>

### <a name="adding-our-store-catalog-data"></a><span data-ttu-id="4c7b1-142">Mağaza Kataloğu verilerimizi ekleme</span><span class="sxs-lookup"><span data-stu-id="4c7b1-142">Adding our store catalog data</span></span>

<span data-ttu-id="4c7b1-143">Biz Entity Framework, yeni oluşturulan veritabanına "seed" veri ekleyen bir özelliğin avantajlarından sürer.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-143">We will take advantage of a feature in Entity Framework which adds "seed" data to a newly created database.</span></span> <span data-ttu-id="4c7b1-144">Bu depolama kataloğumuzu türleri, sanatçıların ve albümleri listesiyle önceden doldurulur.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-144">This will pre-populate our store catalog with a list of Genres, Artists, and Albums.</span></span> <span data-ttu-id="4c7b1-145">-Bu öğreticide daha önce kullanılan bizim site tasarımı dosyaları dahil - bir MvcMusicStore Assets.zip indirme kodu adlı bir klasörde bulunan bu çekirdek verilerle bir sınıf dosyası vardır.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-145">The MvcMusicStore-Assets.zip download - which included our site design files used earlier in this tutorial - has a class file with this seed data, located in a folder named Code.</span></span>

<span data-ttu-id="4c7b1-146">Kod içinde / modeller klasörü SampleData.cs dosyasını bulun ve aşağıda gösterildiği gibi proje modelleri klasörüne bırakın.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-146">Within the Code / Models folder, locate the SampleData.cs file and drop it into the Models folder in our project, as shown below.</span></span>

![](mvc-music-store-part-4/_static/image4.png)

<span data-ttu-id="4c7b1-147">Şimdi biz bir Entity Framework, SampleData sınıfı hakkında bilgi için kod satırı eklemeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-147">Now we need to add one line of code to tell Entity Framework about that SampleData class.</span></span> <span data-ttu-id="4c7b1-148">Global.asax dosyası açın ve aşağıdaki uygulama üstüne satır eklemek için proje kökündeki çift\_yöntemi başlatın.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-148">Double-click on the Global.asax file in the root of the project to open it and add the following line to the top the Application\_Start method.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

<span data-ttu-id="4c7b1-149">Bu noktada, biz Projemizin için Entity Framework yapılandırmak gerekli iş tamamladınız.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-149">At this point, we've completed the work necessary to configure Entity Framework for our project.</span></span>

## <a name="querying-the-database"></a><span data-ttu-id="4c7b1-150">Veritabanını sorgulama</span><span class="sxs-lookup"><span data-stu-id="4c7b1-150">Querying the Database</span></span>

<span data-ttu-id="4c7b1-151">Artık "veri kukla" kullanmak yerine bunun yerine, bilgilerin tümünü sorgulamak için sunduğumuz veritabanına çağırır, böylece müşterilerimize StoreController güncelleştirelim.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-151">Now let's update our StoreController so that instead of using "dummy data" it instead calls into our database to query all of its information.</span></span> <span data-ttu-id="4c7b1-152">Bir alan üzerinde bildirerek başlayacağız **StoreController** storeDB adlı MusicStoreEntities sınıfının bir örneğini tutmak için:</span><span class="sxs-lookup"><span data-stu-id="4c7b1-152">We'll start by declaring a field on the **StoreController** to hold an instance of the MusicStoreEntities class, named storeDB:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a><span data-ttu-id="4c7b1-153">Veritabanını sorgulamak için Store dizini güncelleştiriliyor</span><span class="sxs-lookup"><span data-stu-id="4c7b1-153">Updating the Store Index to query the database</span></span>

<span data-ttu-id="4c7b1-154">MusicStoreEntities sınıf, Entity Framework tarafından korunur ve veritabanımızdaki her tablo için bir koleksiyon özelliği sunar.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-154">The MusicStoreEntities class is maintained by the Entity Framework and exposes a collection property for each table in our database.</span></span> <span data-ttu-id="4c7b1-155">Tüm türleri veritabanımızdaki almak için dizin eylem bizim StoreController'ın güncelleştirelim.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-155">Let's update our StoreController's Index action to retrieve all Genres in our database.</span></span> <span data-ttu-id="4c7b1-156">Daha önce bu kodlama sabit dize verileri tarafından yaptık.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-156">Previously we did this by hard-coding string data.</span></span> <span data-ttu-id="4c7b1-157">Şimdi bunun yerine yalnızca Entity Framework bağlamına Generes koleksiyon kullanabiliriz:</span><span class="sxs-lookup"><span data-stu-id="4c7b1-157">Now we can instead just use the Entity Framework context Generes collection:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

<span data-ttu-id="4c7b1-158">Hiçbir değişiklik biz yine de biz yalnızca canlı verileri bizim veritabanından artık döndürürken önce - biz döndürülen aynı StoreIndexViewModel döndürürken beri bizim şablonu görüntüle gerçekleşmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-158">No changes need to happen to our View template since we're still returning the same StoreIndexViewModel we returned before - we're just returning live data from our database now.</span></span>

<span data-ttu-id="4c7b1-159">Projeyi yeniden çalıştırın ve "/ Store" URL'sini ziyaret edin, artık tüm türleri listesini veritabanımızda yer görüyoruz:</span><span class="sxs-lookup"><span data-stu-id="4c7b1-159">When we run the project again and visit the "/Store" URL, we'll now see a list of all Genres in our database:</span></span>

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a><span data-ttu-id="4c7b1-160">Store göz atın ve ayrıntıları canlı verileri güncelleştirme</span><span class="sxs-lookup"><span data-stu-id="4c7b1-160">Updating Store Browse and Details to use live data</span></span>

<span data-ttu-id="4c7b1-161">İle/Store/göz atma? Tarz =*[Tarz bazı]* eylem yöntemi, biz aramakta olduğunuz bir türe için ada göre.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-161">With the /Store/Browse?genre=*[some-genre]* action method, we're searching for a Genre by name.</span></span> <span data-ttu-id="4c7b1-162">Yalnızca tek bir sonuç şu iki giriş aynı türe adı için hiç olmadığı kadar sahip olmamalıdır ve kullanabiliriz şekilde bekliyoruz. LINQ Sorgu için şunun gibi uygun türe nesne Single() uzantısı (Bu henüz yazmayın):</span><span class="sxs-lookup"><span data-stu-id="4c7b1-162">We only expect one result, since we shouldn't ever have two entries for the same Genre name, and so we can use the .Single() extension in LINQ to query for the appropriate Genre object like this (don't type this yet):</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

<span data-ttu-id="4c7b1-163">Tek bir yöntem sağlayacak şekilde tanımlamış olduğunuz değer adı ile eşleşen tek bir türe nesne istediğimizi belirten bir parametre olarak bir Lambda ifadesi alır.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-163">The Single method takes a Lambda expression as a parameter, which specifies that we want a single Genre object such that its name matches the value we've defined.</span></span> <span data-ttu-id="4c7b1-164">Yukarıdaki durumda biz DISCO eşleşen bir ad değer ile tek bir türe nesne yükleniyor.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-164">In the case above, we are loading a single Genre object with a Name value matching Disco.</span></span>

<span data-ttu-id="4c7b1-165">Biz, tarz nesne alındığında de yüklenen istiyoruz diğer ilgili varlıkları belirtmek olanak sağlayan bir Entity Framework özelliğin avantajlarından yararlanmak.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-165">We'll take advantage of an Entity Framework feature that allows us to indicate other related entities we want loaded as well when the Genre object is retrieved.</span></span> <span data-ttu-id="4c7b1-166">Bu özellik, sorgu sonucu biçimlendirmeye ek olarak adlandırılır ve ihtiyacımız olan tüm bilgileri almak için veritabanına erişmek için ihtiyacımız sayısını azaltmak sağlıyor.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-166">This feature is called Query Result Shaping, and enables us to reduce the number of times we need to access the database to retrieve all of the information we need.</span></span> <span data-ttu-id="4c7b1-167">Sorgumuzu ilgili Albümler istediğimizi belirten Genres.Include("Albums") içerecek şekilde güncelleştireceğiz. böylece albümleri alıyoruz, tarz için önceden getirme istiyoruz.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-167">We want to pre-fetch the Albums for Genre we retrieve, so we'll update our query to include from Genres.Include("Albums") to indicate that we want related albums as well.</span></span> <span data-ttu-id="4c7b1-168">Tek veritabanı isteği bizim tarz ve albüm verileri alacak olduğundan daha verimli budur.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-168">This is more efficient, since it will retrieve both our Genre and Album data in a single database request.</span></span>

<span data-ttu-id="4c7b1-169">Açıklamalar ile ortada, işte güncelleştirilmiş Gözat denetleyicisi eylemimiz nasıl göründüğünü:</span><span class="sxs-lookup"><span data-stu-id="4c7b1-169">With the explanations out of the way, here's how our updated Browse controller action looks:</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

<span data-ttu-id="4c7b1-170">Biz, her bir tür içinde kullanılabilir olan albümleri görüntülenecek Store Gözat görünümü şimdi güncelleştirebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-170">We can now update the Store Browse View to display the albums which are available in each Genre.</span></span> <span data-ttu-id="4c7b1-171">Görünüm şablonu (öğe içinde bulunan /Views/Store/Browse.cshtml) açın ve bir madde işaretli liste albümleri, aşağıda gösterildiği gibi ekleyin.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-171">Open the view template (found in /Views/Store/Browse.cshtml) and add a bulleted list of Albums as shown below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

<span data-ttu-id="4c7b1-172">Uygulamamızı çalıştırma ve tarama/Store/dizinine göz atma? Tarz = ettiğimiz sonuçların artık tüm Albümler bizim seçili tarzında görüntüleme veritabanından alınır Caz gösterir.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-172">Running our application and browsing to /Store/Browse?genre=Jazz shows that our results are now being pulled from the database, displaying all albums in our selected Genre.</span></span>

![](mvc-music-store-part-4/_static/image2.jpg)

<span data-ttu-id="4c7b1-173">Aynı bizim/Store/Ayrıntılar / [ID] URL'sini değiştirin ve sahte verilerimizi albüm kimliği parametre değeri ile eşleşen yükleyen bir veritabanı sorguyla değiştirin oluşturacağız.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-173">We'll make the same change to our /Store/Details/[id] URL, and replace our dummy data with a database query which loads an Album whose ID matches the parameter value.</span></span>

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

<span data-ttu-id="4c7b1-174">Uygulamamızı çalıştırmak ve /Store/Details/1 için tarama sonuçları veritabanından artık çekilen olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-174">Running our application and browsing to /Store/Details/1 shows that our results are now being pulled from the database.</span></span>

![](mvc-music-store-part-4/_static/image5.png)

<span data-ttu-id="4c7b1-175">Albüm albüm Kimliğine göre görüntülemek için Store ayrıntıları sayfamıza ayarlayın, güncelleştirelim **Gözat** ayrıntıları görünüme bağlantılandırmak için Görünüm.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-175">Now that our Store Details page is set up to display an album by the Album ID, let's update the **Browse** view to link to the Details view.</span></span> <span data-ttu-id="4c7b1-176">Tam olarak Store dizinden Store gözatmak için önceki bölümde sonunda bağlamak için yaptığımız gibi Html.ActionLink kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-176">We will use Html.ActionLink, exactly as we did to link from Store Index to Store Browse at the end of the previous section.</span></span> <span data-ttu-id="4c7b1-177">Göz atma görünümü için tam kaynak altında görünür.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-177">The complete source for the Browse view appears below.</span></span>

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

<span data-ttu-id="4c7b1-178">Artık Store sayfamızı kullanılabilir albümleri listeleyen bir türe sayfasına göz atın sunabiliyoruz ve bir albümü tıklayarak Biz bu Albüm ayrıntılarını görüntüleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="4c7b1-178">We're now able to browse from our Store page to a Genre page, which lists the available albums, and by clicking on an album we can view details for that album.</span></span>

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> <span data-ttu-id="4c7b1-179">[Önceki](mvc-music-store-part-3.md)
> [İleri](mvc-music-store-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="4c7b1-179">[Previous](mvc-music-store-part-3.md)
[Next](mvc-music-store-part-5.md)</span></span>
