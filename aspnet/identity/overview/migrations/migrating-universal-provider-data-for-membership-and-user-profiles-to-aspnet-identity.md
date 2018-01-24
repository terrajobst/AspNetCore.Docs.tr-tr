---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: "Üyelik ve ASP.NET Identity (C#) için kullanıcı profilleri için evrensel sağlayıcı veri geçirme | Microsoft Docs"
author: rustd
description: "Bu öğretici, kullanıcı ve rol verileri ve mevcut uygulama Evrensel sağlayıcıları kullanılarak oluşturulan kullanıcı profili verilerini geçirmek için gerekli olan adımları açıklanmıştır..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/13/2013
ms.topic: article
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: e00bcfc111425d5dd26c7ff341eaf87fd969e089
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
<a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a><span data-ttu-id="38698-103">Üyelik ve ASP.NET Identity (C#) için kullanıcı profilleri için evrensel sağlayıcısı verileri geçirme</span><span class="sxs-lookup"><span data-stu-id="38698-103">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity (C#)</span></span>
====================
<span data-ttu-id="38698-104">tarafından [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="38698-104">by [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="38698-105">Bu öğretici, kullanıcı ve rol verileri ve Evrensel Sağlayıcılar, ASP.NET Identity modeli varolan bir uygulamanın kullanılarak oluşturulan kullanıcı profili verilerini geçirmek gereken adımları açıklar.</span><span class="sxs-lookup"><span data-stu-id="38698-105">This tutorial describes the steps that are necessary to migrate user and role data and user profile data created using Universal Providers of an existing application to the ASP.NET Identity model.</span></span> <span data-ttu-id="38698-106">Bir yaklaşım, kullanıcı profili verilerini geçirmek için SQL üyeliğe sahip bir uygulamada kullanılabilir burada sözü edilen.</span><span class="sxs-lookup"><span data-stu-id="38698-106">The approach mentioned here to migrate user profile data can be used in an application with SQL membership as well.</span></span>


<span data-ttu-id="38698-107">Visual Studio 2013 sürümüyle, yeni bir ASP.NET Identity sistemi ASP.NET takım sunulan ve daha fazla bilgiyi bu sürüm hakkında [burada](../../index.md).</span><span class="sxs-lookup"><span data-stu-id="38698-107">With the release of Visual Studio 2013, the ASP.NET team introduced a new ASP.NET Identity system, and you can read more about that release [here](../../index.md).</span></span> <span data-ttu-id="38698-108">Web uygulamalarından geçirmek için makale üzerinde takip [yeni kimlik sistemi SQL üyelik](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), bu makalede kullanıcı ve rol yönetimi için sağlayıcı modelini izler mevcut uygulamalarını taşımak için adımları gösterir Yeni kimlik modeline.</span><span class="sxs-lookup"><span data-stu-id="38698-108">Following up on the article to migrate web applications from [SQL Membership to the new Identity system](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), this article illustrates the steps to migrate existing applications that follow the Providers model for user and role management to the new Identity model.</span></span> <span data-ttu-id="38698-109">Bu öğreticinin odak öncelikle sorunsuz bir şekilde yeni sisteme bağlanmanıza kullanıcı profili verilerini geçirme olacaktır.</span><span class="sxs-lookup"><span data-stu-id="38698-109">The focus of this tutorial will be primarily on migrating the user profile data to seamlessly hook it into the new system.</span></span> <span data-ttu-id="38698-110">Kullanıcı ve rol bilgilerini geçirme için SQL üyeliği benzer.</span><span class="sxs-lookup"><span data-stu-id="38698-110">Migrating user and role information is similar for SQL membership.</span></span> <span data-ttu-id="38698-111">Profil verilerini geçirmek ve ardından yaklaşım SQL üyeliğe sahip bir uygulamada kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="38698-111">The approach followed to migrate profile data can be used in an application with SQL membership as well.</span></span>

<span data-ttu-id="38698-112">Örnek olarak, biz sağlayıcıları modeli kullanan Visual Studio 2012 kullanılarak oluşturulan bir web uygulaması ile başlar.</span><span class="sxs-lookup"><span data-stu-id="38698-112">As an example, we will start with a web app created using Visual Studio 2012 which uses the Providers model.</span></span> <span data-ttu-id="38698-113">Biz sonra profil yönetimi için kodu ekleyin, bir kullanıcı kaydı, kullanıcılar için profil verileri eklemek, veritabanı şemasını geçirmek ve sonra uygulamayı kimlik sistemi için kullanıcı ve rol yönetimi kullanacak şekilde değiştirin.</span><span class="sxs-lookup"><span data-stu-id="38698-113">We'll then add code for profile management, register a user, add profile data for the users, migrate the database schema, and then change the application to use the Identity system for user and role management.</span></span> <span data-ttu-id="38698-114">Bir test geçiş olarak Evrensel sağlayıcıları kullanılarak oluşturulan kullanıcılar oturum açamaz ve yeni kullanıcılar kaydetmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="38698-114">As a test of migration, users created using Universal Providers should be able to log in and new users should be able to register.</span></span>

> [!NOTE]
> <span data-ttu-id="38698-115">Tam örneğini şurada bulabilirsiniz [https://github.com/suhasj/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations).</span><span class="sxs-lookup"><span data-stu-id="38698-115">You can find the complete sample at [https://github.com/suhasj/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations).</span></span>


## <a name="profile-data-migration-summary"></a><span data-ttu-id="38698-116">Profil verileri geçiş özeti</span><span class="sxs-lookup"><span data-stu-id="38698-116">Profile data migration summary</span></span>

<span data-ttu-id="38698-117">İle geçişlerin başlamadan önce bize sağlayıcıları modelinde profil verileri depolamak deneyimi bakın.</span><span class="sxs-lookup"><span data-stu-id="38698-117">Before starting with the migrations, let us look at the experience of storing profile data in the Providers model.</span></span> <span data-ttu-id="38698-118">Kullanıcıların birden çok yolla depolanabilir uygulama için profil verileri, aralarındaki en yaygın Evrensel sağlayıcıları ile birlikte gönderilen yerleşik Profil sağlayıcıları kullanarak.</span><span class="sxs-lookup"><span data-stu-id="38698-118">Profile data for application users can be stored in multiple ways, the most common among them being using the inbuilt profile providers shipped along with the Universal Providers.</span></span> <span data-ttu-id="38698-119">Adımları içerir.</span><span class="sxs-lookup"><span data-stu-id="38698-119">The steps would include</span></span>

1. <span data-ttu-id="38698-120">Profil verilerini depolamak için kullanılan özelliklere sahip bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="38698-120">Add a class that has properties used to store Profile data.</span></span>
2. <span data-ttu-id="38698-121">'ProfileBase' genişleten ve kullanıcı için yukarıdaki profil verileri almak için yöntemleri uygulayan bir sınıf ekleyin.</span><span class="sxs-lookup"><span data-stu-id="38698-121">Add a class that extends 'ProfileBase' and implements methods to get the above profile data for the user.</span></span>
3. <span data-ttu-id="38698-122">Varsayılan Profil sağlayıcıları kullanarak etkinleştirin *web.config* dosya ve profil bilgilerini erişirken kullanılacak #2. adımda bildirilen sınıfı tanımlayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38698-122">Enable using default profile providers in the *web.config* file and define the class declared in step #2 to be used in accessing profile information.</span></span>

<span data-ttu-id="38698-123">Profil bilgilerini serileştirilmiş xml ve veritabanı 'Profilleri' tablosunda ikili veri olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="38698-123">The profile information is stored as serialized xml and binary data in the 'Profiles' table in the database.</span></span>

<span data-ttu-id="38698-124">Yeni ASP.NET kimlik sistemi kullanmak için uygulamayı taşıdıktan sonra profil bilgileri serisi ve kullanıcı sınıfındaki özellikleri olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="38698-124">After migrating the application to use the new ASP.NET Identity system, the profile information is deserialized and stored as properties on the user class.</span></span> <span data-ttu-id="38698-125">Her bir özellik, kullanıcı tablodaki sütunlar üzerine sonra eşlenebilir.</span><span class="sxs-lookup"><span data-stu-id="38698-125">Each property can then be mapped onto columns in the user table.</span></span> <span data-ttu-id="38698-126">Avantajı burada özellikleri serileştirmek/veri bilgilerini seri durumdan zorunluluğunu yanı sıra kullanıcı sınıfı kullanarak doğrudan çalıştığından emin olup bu erişirken zaman.</span><span class="sxs-lookup"><span data-stu-id="38698-126">The advantage here is that the properties can be worked on directly using the user class in addition to not having to serialize/deserialize data information each time when accessing it.</span></span>

## <a name="getting-started"></a><span data-ttu-id="38698-127">Başlarken</span><span class="sxs-lookup"><span data-stu-id="38698-127">Getting Started</span></span>

1. <span data-ttu-id="38698-128">Visual Studio 2012'de yeni bir ASP.NET 4.5 Web formları uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="38698-128">Create a new ASP.NET 4.5 Web Forms application in Visual Studio 2012.</span></span> <span data-ttu-id="38698-129">Geçerli örnek Web Forms şablonunu kullanır, ancak MVC uygulaması de kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38698-129">The current sample uses the Web Forms template, but you could use MVC Application as well.</span></span>  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. <span data-ttu-id="38698-130">Profil bilgilerini depolamak için ' modeller' yeni bir klasör oluşturun</span><span class="sxs-lookup"><span data-stu-id="38698-130">Create a new folder 'Models' to store profile information</span></span>  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. <span data-ttu-id="38698-131">Örnek olarak, doğum tarihi, şehir, yükseklik ve Ağırlık kullanıcının profilinde bize depolar.</span><span class="sxs-lookup"><span data-stu-id="38698-131">As an example, let us store the date of birth, city, height and weight of the user in profile.</span></span> <span data-ttu-id="38698-132">Yükseklik ve Ağırlık 'PersonalStats' adlı özel bir sınıf depolanır.</span><span class="sxs-lookup"><span data-stu-id="38698-132">The height and weight are stored as a custom class called 'PersonalStats'.</span></span> <span data-ttu-id="38698-133">Depolamak ve profil almak için 'ProfileBase' genişleten bir sınıfı gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="38698-133">To store and retrieve the profile, we need a class that extends 'ProfileBase'.</span></span> <span data-ttu-id="38698-134">Yeni bir sınıf profil bilgilerini depolamak ve almak için ' AppProfile' oluşturalım.</span><span class="sxs-lookup"><span data-stu-id="38698-134">Let's create a new class 'AppProfile' to get and store profile information.</span></span>

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. <span data-ttu-id="38698-135">Profilinde etkinleştirmek *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="38698-135">Enable profile in the *web.config* file.</span></span> <span data-ttu-id="38698-136">Depolama / #3. adımda oluşturduğunuz kullanıcı bilgilerini almak için kullanılan sınıf adı girin.</span><span class="sxs-lookup"><span data-stu-id="38698-136">Enter the class name to be used to store/retrieve user information created in step #3.</span></span>

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. <span data-ttu-id="38698-137">Web forms sayfası kullanıcı profil verileri almak ve depolamak için 'Account' klasöründe ekleyin.</span><span class="sxs-lookup"><span data-stu-id="38698-137">Add a web forms page in 'Account' folder to get the profile data from the user and store it.</span></span> <span data-ttu-id="38698-138">Projeye sağ tıklayın ve 'Yeni Öğe Ekle' seçin.</span><span class="sxs-lookup"><span data-stu-id="38698-138">Right click on project and select 'Add new Item'.</span></span> <span data-ttu-id="38698-139">Ana Sayfa 'AddProfileData.aspx' ile yeni bir webforms sayfa ekleyin.</span><span class="sxs-lookup"><span data-stu-id="38698-139">Add a new webforms page with master page 'AddProfileData.aspx'.</span></span> <span data-ttu-id="38698-140">Aşağıdaki 'MainContent' bölümünde kopyalayın:</span><span class="sxs-lookup"><span data-stu-id="38698-140">Copy the following in the 'MainContent' section:</span></span>

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

 <span data-ttu-id="38698-141">Arkasındaki kodda aşağıdaki kodu ekleyin:</span><span class="sxs-lookup"><span data-stu-id="38698-141">Add the following code in the code behind:</span></span>

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

 <span data-ttu-id="38698-142">Hangi AppProfile altında sınıfı derleme hataları kaldırmak için tanımlanmış bir ad ekleyin.</span><span class="sxs-lookup"><span data-stu-id="38698-142">Add the namespace under which AppProfile class is defined to remove the compilation errors.</span></span>
6. <span data-ttu-id="38698-143">Uygulamayı çalıştırın ve kullanıcı adı olan yeni bir kullanıcı oluşturmak '**olduser'.**</span><span class="sxs-lookup"><span data-stu-id="38698-143">Run the app and create a new user with username '**olduser'.**</span></span> <span data-ttu-id="38698-144">'AddProfileData' sayfasına gidin ve kullanıcı için profil bilgilerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="38698-144">Navigate to the 'AddProfileData' page and add profile information for the user.</span></span>  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

<span data-ttu-id="38698-145">Veriler Sunucu Gezgini penceresini kullanarak 'Profilleri' tablosundaki serileştirilmiş xml olarak depolanır doğrulayabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38698-145">You can verify that the data is stored as serialized xml in the 'Profiles' table using the Server Explorer window.</span></span> <span data-ttu-id="38698-146">Visual Studio'da 'Görüntüle' menüsünden 'Sunucu Gezgini' seçin.</span><span class="sxs-lookup"><span data-stu-id="38698-146">In Visual Studio, from 'View' menu, choose 'Server Explorer'.</span></span> <span data-ttu-id="38698-147">Bir veri bağlantısı için tanımlanan veritabanı olmalıdır *web.config* dosya.</span><span class="sxs-lookup"><span data-stu-id="38698-147">There should be a data connection for the database defined in the *web.config* file.</span></span> <span data-ttu-id="38698-148">Veri bağlantısı tıklayarak farklı alt kategorileri gösterir.</span><span class="sxs-lookup"><span data-stu-id="38698-148">Clicking on the data connection shows different sub categories.</span></span> <span data-ttu-id="38698-149">Veritabanınızda, farklı tablolara Göster sonra 'Profilleri' sağ tıklatın ve 'Show Table profilleri tablosunda depolanan profil verilerini görüntülemek için Data' seçin ' tabloları genişletin.</span><span class="sxs-lookup"><span data-stu-id="38698-149">Expand 'Tables' to show the different tables in your database, then right click on 'Profiles' and choose 'Show Table Data' to view the profile data stored in the Profiles table.</span></span>

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a><span data-ttu-id="38698-150">Geçirme veritabanı şeması</span><span class="sxs-lookup"><span data-stu-id="38698-150">Migrating database schema</span></span>

<span data-ttu-id="38698-151">Kimlik sistemi ile çalışmak için varolan veritabanı yapmak için şu kimlik veritabanını özgün veritabanına eklediğimiz alanlarını destekleyecek biçimde şemada güncelleştirmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="38698-151">To make the existing database work with the Identity system, we need to update the schema in the Identity database to support the fields we added to the original database.</span></span> <span data-ttu-id="38698-152">Bu yapılabilir SQL komut dosyalarını kullanarak yeni tablolar oluşturma ve varolan bilgileri kopyalayın.</span><span class="sxs-lookup"><span data-stu-id="38698-152">This can be done using SQL scripts to create new tables and copy the existing information.</span></span> <span data-ttu-id="38698-153">'Sunucu Gezgini' penceresinde 'tablolarını görüntülemek için DefaultConnection' genişletin.</span><span class="sxs-lookup"><span data-stu-id="38698-153">In the 'Server Explorer' window, expand the 'DefaultConnection' to display the tables.</span></span> <span data-ttu-id="38698-154">Tabloları sağ tıklayın ve 'Yeni sorgu' yi seçin</span><span class="sxs-lookup"><span data-stu-id="38698-154">Right click Tables and select 'New Query'</span></span>

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

<span data-ttu-id="38698-155">SQL komut dosyasından Yapıştır [https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) ve çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="38698-155">Paste the SQL script from [https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) and run it.</span></span> <span data-ttu-id="38698-156">'DefaultConnection' yenilenirse, yeni tablolar eklenen görebiliriz.</span><span class="sxs-lookup"><span data-stu-id="38698-156">If the 'DefaultConnection' is refreshed, we can see that the new tables are added.</span></span> <span data-ttu-id="38698-157">İçindeki bilgileri geçirilmiş görmek için tablo verileri kontrol edebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38698-157">You can check the data inside the tables to see that the information has been migrated.</span></span>

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a><span data-ttu-id="38698-158">ASP.NET kimlik kullanmak üzere uygulamayı geçirme</span><span class="sxs-lookup"><span data-stu-id="38698-158">Migrating the application to use ASP.NET Identity</span></span>

1. <span data-ttu-id="38698-159">ASP.NET kimliği için gereken Nuget paketlerini yükleyin:</span><span class="sxs-lookup"><span data-stu-id="38698-159">Install the Nuget packages needed for ASP.NET Identity:</span></span>

    - <span data-ttu-id="38698-160">Microsoft.ASPNET.Identity.entityframework</span><span class="sxs-lookup"><span data-stu-id="38698-160">Microsoft.AspNet.Identity.EntityFramework</span></span>
    - <span data-ttu-id="38698-161">Microsoft.ASPNET.Identity.owin</span><span class="sxs-lookup"><span data-stu-id="38698-161">Microsoft.AspNet.Identity.Owin</span></span>
    - <span data-ttu-id="38698-162">Microsoft.Owin.Host.SystemWeb</span><span class="sxs-lookup"><span data-stu-id="38698-162">Microsoft.Owin.Host.SystemWeb</span></span>
    - <span data-ttu-id="38698-163">Microsoft.Owin.Security.Facebook</span><span class="sxs-lookup"><span data-stu-id="38698-163">Microsoft.Owin.Security.Facebook</span></span>
    - <span data-ttu-id="38698-164">Microsoft.Owin.Security.Google</span><span class="sxs-lookup"><span data-stu-id="38698-164">Microsoft.Owin.Security.Google</span></span>
    - <span data-ttu-id="38698-165">Microsoft.Owin.Security.MicrosoftAccount</span><span class="sxs-lookup"><span data-stu-id="38698-165">Microsoft.Owin.Security.MicrosoftAccount</span></span>
    - <span data-ttu-id="38698-166">Microsoft.Owin.Security.Twitter</span><span class="sxs-lookup"><span data-stu-id="38698-166">Microsoft.Owin.Security.Twitter</span></span>

 <span data-ttu-id="38698-167">Nuget paketlerini yönetme hakkında daha fazla bilgi bulunabilir [burada](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)</span><span class="sxs-lookup"><span data-stu-id="38698-167">More information on managing Nuget packages can be found [here](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)</span></span>
2. <span data-ttu-id="38698-168">Tabloda varolan veriler çalışmak için size geri tablolara harita ve kimlik sistemi takma modeli sınıflarını oluşturmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="38698-168">To work with existing data in the table, we need to create model classes which map back to the tables and hook them up in the Identity system.</span></span> <span data-ttu-id="38698-169">Kimlik sözleşmesinin bir parçası olarak modeli sınıfları Identity.Core DLL'de tanımlanan arabirimleri ya da uygulamanız gerekir veya bu arabirimleri Microsoft.ASPNET.Identity.entityframework içinde kullanılabilir varolan uyarlamasını genişletebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38698-169">As part of the Identity contract, the model classes should either implement the interfaces defined in the Identity.Core dll or can extend the existing implementation of these interfaces available in Microsoft.AspNet.Identity.EntityFramework.</span></span> <span data-ttu-id="38698-170">Biz mevcut sınıflarını rolü, kullanıcı oturumu ve kullanıcı talepleri için kullanır.</span><span class="sxs-lookup"><span data-stu-id="38698-170">We will be using the existing classes for role, user logins and user claims.</span></span> <span data-ttu-id="38698-171">Bizim örnek için özel bir kullanıcı kullanmamız gerekiyor.</span><span class="sxs-lookup"><span data-stu-id="38698-171">We need to use a custom user for our sample.</span></span> <span data-ttu-id="38698-172">Projeye sağ tıklayın ve 'IdentityModels' yeni bir klasör oluşturun.</span><span class="sxs-lookup"><span data-stu-id="38698-172">Right click on the project and create new folder 'IdentityModels'.</span></span> <span data-ttu-id="38698-173">Yeni bir 'User' sınıf aşağıda gösterildiği gibi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="38698-173">Add a new 'User' class as shown below:</span></span>

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

 <span data-ttu-id="38698-174">'ProfileInfo' artık kullanıcı sınıfı bir özellik olduğuna dikkat edin.</span><span class="sxs-lookup"><span data-stu-id="38698-174">Notice that the 'ProfileInfo' is now a property on the user class.</span></span> <span data-ttu-id="38698-175">Bu nedenle biz doğrudan profili verilerle çalışmak için kullanıcı sınıfını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="38698-175">Hence we can use the user class to directly work with profile data.</span></span>

<span data-ttu-id="38698-176">Dosyaları kopyalama **IdentityModels** ve **IdentityAccount** klasörleri yükleme kaynağından ( [https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/ UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ).</span><span class="sxs-lookup"><span data-stu-id="38698-176">Copy the files in the **IdentityModels** and **IdentityAccount** folders from the download source ( [https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ).</span></span> <span data-ttu-id="38698-177">Bunlar, kalan modeli sınıfları ve kullanıcı ve rol yönetimi ASP.NET Identity API'lerini kullanarak için gereken yeni sayfalar içerir.</span><span class="sxs-lookup"><span data-stu-id="38698-177">These have the remaining model classes and the new pages needed for user and role management using the ASP.NET Identity APIs.</span></span> <span data-ttu-id="38698-178">Kullanılan yaklaşımı SQL üyelik benzer ve ayrıntılı açıklama bulunabilir [burada](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).</span><span class="sxs-lookup"><span data-stu-id="38698-178">The approach used is similar to the SQL Membership and the detailed explanation can be found [here](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).</span></span>

## <a name="copying-profile-data-to-the-new-tables"></a><span data-ttu-id="38698-179">Yeni tablolar için profil verileri kopyalama</span><span class="sxs-lookup"><span data-stu-id="38698-179">Copying Profile data to the new tables</span></span>

<span data-ttu-id="38698-180">Daha önce belirtildiği gibi biz profilleri tablolarındaki xml verileri seri durumdan ve AspNetUsers tablosunun sütunları depolamak gerekir.</span><span class="sxs-lookup"><span data-stu-id="38698-180">As mentioned earlier, we need to deserialize the xml data in the Profiles tables and store it in the columns of the AspNetUsers table.</span></span> <span data-ttu-id="38698-181">Bu sütunları gerekli verileri ile doldurmak için kalan tüm olacak şekilde yeni sütunlar önceki adımda kullanıcılar tablosundaki oluşturuldu.</span><span class="sxs-lookup"><span data-stu-id="38698-181">The new columns were created in the users table in the previous step so all that is left is to populate those columns with the necessary data.</span></span> <span data-ttu-id="38698-182">Bunu yapmak için kullanıcılar tablosunun yeni oluşturulan sütunları doldurmak için bir kez çalışan bir konsol uygulaması kullanacağız.</span><span class="sxs-lookup"><span data-stu-id="38698-182">To do this, we will use a console application which is run once to populate the newly created columns in the users table.</span></span>

1. <span data-ttu-id="38698-183">Mevcut çözüme yeni bir konsol uygulaması oluşturun.</span><span class="sxs-lookup"><span data-stu-id="38698-183">Create a new console application in the exiting solution.</span></span>  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. <span data-ttu-id="38698-184">Entity Framework paketinin en son sürümünü yükleyin.</span><span class="sxs-lookup"><span data-stu-id="38698-184">Install the latest version of the Entity Framework package.</span></span>
3. <span data-ttu-id="38698-185">Konsol uygulaması bir başvuru olarak yukarıda oluşturduğunuz web uygulaması ekleyin.</span><span class="sxs-lookup"><span data-stu-id="38698-185">Add the web application created above as a reference to the console application.</span></span> <span data-ttu-id="38698-186">Yapmak için bu sağ projede ve ardından 'Başvuru Ekle', daha sonra çözüm, ardından projeye tıklayın ve Tamam'ı tıklatın.</span><span class="sxs-lookup"><span data-stu-id="38698-186">To do this right click on Project, then 'Add References', then Solution, then click on the project and click OK.</span></span>
4. <span data-ttu-id="38698-187">Kopya kodu Program.cs sınıfında aşağıda.</span><span class="sxs-lookup"><span data-stu-id="38698-187">Copy the below code in the Program.cs class.</span></span> <span data-ttu-id="38698-188">Bu mantık her kullanıcı için profil verileri okur, 'ProfileInfo' nesnesi olarak serileştirir ve veritabanına depolar.</span><span class="sxs-lookup"><span data-stu-id="38698-188">This logic reads profile data for each user, serializes it as 'ProfileInfo' object and stores it back to the database.</span></span>

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

 <span data-ttu-id="38698-189">Karşılık gelen ad alanları içermelidir şekilde kullanılan modellerin bazıları web uygulama projesi 'IdentityModels' klasöründe tanımlanır.</span><span class="sxs-lookup"><span data-stu-id="38698-189">Some of the models used are defined in the 'IdentityModels' folder of the web application project, so you must include the corresponding namespaces.</span></span>
5. <span data-ttu-id="38698-190">Yukarıdaki kod uygulama veritabanı dosyasında çalışır\_web uygulaması projesi veri klasörü önceki adımlarda oluşturulacak.</span><span class="sxs-lookup"><span data-stu-id="38698-190">The above code works on the database file in the App\_Data folder of the web application project created in the previous steps.</span></span> <span data-ttu-id="38698-191">Başvuran için web uygulamasının web.config dosyasında bağlantı dizesiyle konsol uygulamasındaki app.config dosyasında bağlantı dizesini güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="38698-191">To reference that, update the connection string in the app.config file of the console application with the connection string in the web.config of the web application.</span></span> <span data-ttu-id="38698-192">Ayrıca 'AttachDbFilename' özelliğindeki tam fiziksel yolunu belirtin.</span><span class="sxs-lookup"><span data-stu-id="38698-192">Also provide the complete physical path in the 'AttachDbFilename' property.</span></span>
6. <span data-ttu-id="38698-193">Bir komut istemi açın ve yukarıdaki konsol uygulaması bin klasörüne gidin.</span><span class="sxs-lookup"><span data-stu-id="38698-193">Open a command prompt and navigate to the bin folder of the above console application.</span></span> <span data-ttu-id="38698-194">Yürütülebilir dosyayı çalıştırmak ve aşağıdaki görüntüde gösterildiği gibi günlük çıkışı gözden geçirin.</span><span class="sxs-lookup"><span data-stu-id="38698-194">Run the executable and review the log output as shown in the following image.</span></span>  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. <span data-ttu-id="38698-195">Server Explorer'da 'AspNetUsers' tablo açın ve yeni sütunlardaki özellikleri tutmak verileri doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="38698-195">Open the 'AspNetUsers' table in the Server Explorer and verify the data in the new columns that hold the properties.</span></span> <span data-ttu-id="38698-196">İle ilgili özellik değerlerini güncelleştirilmelidir.</span><span class="sxs-lookup"><span data-stu-id="38698-196">They should be updated with the corresponding property values.</span></span>

## <a name="verify-functionality"></a><span data-ttu-id="38698-197">İşlevselliğini doğrulayın</span><span class="sxs-lookup"><span data-stu-id="38698-197">Verify functionality</span></span>

<span data-ttu-id="38698-198">Eski veritabanından bir kullanıcı oturum açma ASP.NET Identity kullanılarak uygulanan yeni eklenen üyelik sayfalarını kullanın.</span><span class="sxs-lookup"><span data-stu-id="38698-198">Use the newly added membership pages that are implemented using ASP.NET Identity to login a user from the old database.</span></span> <span data-ttu-id="38698-199">Kullanıcı kimlik bilgilerini kullanarak oturum açması görebilmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="38698-199">The user should be able to log in using the same credentials.</span></span> <span data-ttu-id="38698-200">Rol, ekleme, parola değiştirme, yeni bir kullanıcı oluşturma OAuth ekleme gibi diğer işlevlerini deneyin kullanıcıları ekleyin rolleri, vb. için.</span><span class="sxs-lookup"><span data-stu-id="38698-200">Try the other functionalities like adding OAuth, creating a new user, changing a password, adding roles, add users to roles, etc.</span></span>

<span data-ttu-id="38698-201">Eski kullanıcı ve yeni kullanıcılar için profil verileri alınır ve kullanıcıların tablosunda depolanır.</span><span class="sxs-lookup"><span data-stu-id="38698-201">The Profile data for the old user and the new users should be retrieved and stored in the users table.</span></span> <span data-ttu-id="38698-202">Eski tablo artık başvurulmayan.</span><span class="sxs-lookup"><span data-stu-id="38698-202">The old table should no longer be referenced.</span></span>

## <a name="conclusion"></a><span data-ttu-id="38698-203">Sonuç</span><span class="sxs-lookup"><span data-stu-id="38698-203">Conclusion</span></span>

<span data-ttu-id="38698-204">Makaleyi sağlayıcı modeline üyelik ASP.NET kimliği için kullanılan web uygulamalarını geçirme işlemi açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="38698-204">The article described the process of migrating web applications that used the provider model for membership to ASP.NET Identity.</span></span> <span data-ttu-id="38698-205">Makale ayrıca kullanıcıların kimlik sisteme sayfaya profil verileri geçirme özetlenen.</span><span class="sxs-lookup"><span data-stu-id="38698-205">The article additionally outlined migrating profile data for users to be hooked into the Identity system.</span></span> <span data-ttu-id="38698-206">Sorular ve uygulamanızı geçirirken karşılaşılan sorunları için aşağıda yorum bırakın.</span><span class="sxs-lookup"><span data-stu-id="38698-206">Please leave comments below for questions and issues encountered when you migrate your app.</span></span>

<span data-ttu-id="38698-207">*Rick Anderson ve Robert McMurray'nın makaleyi gözden geçirme için teşekkür ederiz.*</span><span class="sxs-lookup"><span data-stu-id="38698-207">*Thanks to Rick Anderson and Robert McMurray for reviewing the article.*</span></span>