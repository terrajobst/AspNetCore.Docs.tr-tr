---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Web API çağırma bir Windows Phone 8 uygulama (C#) | Microsoft Docs
author: rmcmurray
description: Bir Windows Phone 8 uygulaması defterlerine kataloğunu sağlayan bir ASP.NET Web API uygulaması oluşan eksiksiz bir uçtan uca senaryo oluşturun.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 7d0486b4cab85ffe77fda87d4b34dd3ec0a9e8fe
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "30874232"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="4d845-103">Bir Windows Phone 8 uygulamasından (C#) Web API'si çağırma</span><span class="sxs-lookup"><span data-stu-id="4d845-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="4d845-104">tarafından [Robert McMurray '](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="4d845-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="4d845-105">Bu öğreticide, bir Windows Phone 8 uygulaması defterlerine kataloğunu sağlayan bir ASP.NET Web API uygulaması oluşan eksiksiz bir uçtan uca senaryo oluşturmak öğreneceksiniz.</span><span class="sxs-lookup"><span data-stu-id="4d845-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="4d845-106">Genel Bakış</span><span class="sxs-lookup"><span data-stu-id="4d845-106">Overview</span></span>

<span data-ttu-id="4d845-107">ASP.NET Web API gibi rESTful hizmetlerini, sunucu tarafı ve istemci tarafı uygulamalar mimarisi özetleyen tarafından geliştiriciler için HTTP tabanlı uygulamaların oluşturulmasını basitleştirir.</span><span class="sxs-lookup"><span data-stu-id="4d845-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="4d845-108">İletişim için bir özel yuva tabanlı Protokolü oluşturmak yerine, Web API geliştiricilerin kendi uygulama için gerekli HTTP yöntemleri yayımlamak yeterlidir (örneğin: GET, POST, PUT, DELETE), ve istemci uygulaması geliştiricileri yeterlidir kullanmak Kullanıcıların uygulama için gerekli HTTP yöntemleri.</span><span class="sxs-lookup"><span data-stu-id="4d845-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="4d845-109">Bu uçtan uca öğretici kapsamında, Web API aşağıdaki projeleri oluşturmak için nasıl kullanılacağını öğreneceksiniz:</span><span class="sxs-lookup"><span data-stu-id="4d845-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="4d845-110">İçinde [Bu öğreticinin ilk bölümünü](#STEP1), tüm kitap kataloğunun yönetmek için oluşturma, okuma, güncelleştirme ve Sil (CRUD) işlemlerini destekleyen bir ASP.NET Web API uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="4d845-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="4d845-111">Bu uygulamayı kullanmak [örnek XML dosyası (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) MSDN'den.</span><span class="sxs-lookup"><span data-stu-id="4d845-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="4d845-112">İçinde [ikinci bölümü bu öğreticinin](#STEP2), Web API uygulamanızı verileri alır etkileşimli bir Windows Phone 8 uygulaması oluşturacaksınız.</span><span class="sxs-lookup"><span data-stu-id="4d845-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="4d845-113">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="4d845-113">Prerequisites</span></span>

- <span data-ttu-id="4d845-114">Windows Phone 8 SDK ile Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="4d845-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="4d845-115">64-bit sistem yüklü Hyper-V ile Windows 8 veya sonraki bir sürümü</span><span class="sxs-lookup"><span data-stu-id="4d845-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="4d845-116">Ek gereksinimler listesi için bkz *sistem gereksinimleri* bölümünde [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) sayfa indirin.</span><span class="sxs-lookup"><span data-stu-id="4d845-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="4d845-117">Web API ve Windows Phone 8 projeleri yerel sisteminizde arasındaki bağlantıyı sınamak için kullanacaksanız,'ndaki yönergeleri izleyin gerekir *[yerel bir Web API uygulamalarını Windows Phone 8 öykünücüsü bağlanma Bilgisayar](https://go.microsoft.com/fwlink/?LinkId=324014)* makale test ortamınızı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4d845-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="4d845-118">1. adım: Web API kitaplığı projesi oluşturma</span><span class="sxs-lookup"><span data-stu-id="4d845-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="4d845-119">Bu uçtan uca öğreticinin ilk adımı, tüm CRUD işlemleri destekleyen bir Web API projesi oluşturmaktır; Bu çözüm için Windows Phone Uygulama projesi ekleyeceğini unutmayın [2. adım](#STEP2) Bu öğreticinin.</span><span class="sxs-lookup"><span data-stu-id="4d845-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="4d845-120">Açık **Visual Studio 2013'ün**.</span><span class="sxs-lookup"><span data-stu-id="4d845-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="4d845-121">Tıklatın **dosya**, ardından **yeni**ve ardından **proje**.</span><span class="sxs-lookup"><span data-stu-id="4d845-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="4d845-122">Zaman **yeni proje** iletişim kutusu görüntülenir, genişletin **yüklü**, ardından **şablonları**, ardından **Visual C#** ve ardından **Web**.</span><span class="sxs-lookup"><span data-stu-id="4d845-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="4d845-123">Genişletmek için tıklayın</span><span class="sxs-lookup"><span data-stu-id="4d845-123">Click image to expand</span></span>                                                                |


4. <span data-ttu-id="4d845-124">Vurgula **ASP.NET Web uygulaması**, girin **kitap** proje adı ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="4d845-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="4d845-125">Zaman **yeni ASP.NET projesi** iletişim kutusu görüntülenir, seçin **Web API** şablonu ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="4d845-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="4d845-126">Genişletmek için tıklayın</span><span class="sxs-lookup"><span data-stu-id="4d845-126">Click image to expand</span></span>                                                                |


6. <span data-ttu-id="4d845-127">Web API projesi açıldığında, örnek denetleyicisi projeden kaldırın:</span><span class="sxs-lookup"><span data-stu-id="4d845-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="4d845-128">Genişletme **denetleyicileri** Çözüm Gezgininde klasör.</span><span class="sxs-lookup"><span data-stu-id="4d845-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="4d845-129">Sağ **ValuesController.cs** dosya ve ardından **silmek**.</span><span class="sxs-lookup"><span data-stu-id="4d845-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="4d845-130">Tıklatın **Tamam** silme işlemini onaylamanız istendiğinde.</span><span class="sxs-lookup"><span data-stu-id="4d845-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="4d845-131">XML veri dosyası, Web API projesine ekleyin; Bu dosya kitap katalog içeriğinin içerir:</span><span class="sxs-lookup"><span data-stu-id="4d845-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="4d845-132">Sağ **uygulama\_veri** Çözüm Gezgininde klasör ardından **Ekle**ve ardından **yeni öğe**.</span><span class="sxs-lookup"><span data-stu-id="4d845-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="4d845-133">Zaman **Yeni Öğe Ekle** iletişim kutusu görüntülenir, vurgulayın **XML dosyası** şablonu.</span><span class="sxs-lookup"><span data-stu-id="4d845-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="4d845-134">Dosya adı **Books.xml**ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="4d845-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="4d845-135">Zaman **Books.xml** dosya açıldığında, örnekten XML dosyasındaki kodu yerine **books.xml** MSDN'de dosyası:</span><span class="sxs-lookup"><span data-stu-id="4d845-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="4d845-136">XML dosyasını kaydedip kapatın.</span><span class="sxs-lookup"><span data-stu-id="4d845-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="4d845-137">Kitap modelin Web API projesine ekleyin; Bu model Kitaplığı'nı uygulama oluşturma, okuma, güncelleştirme ve Sil (CRUD) mantığını içerir:</span><span class="sxs-lookup"><span data-stu-id="4d845-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="4d845-138">Sağ **modelleri** Çözüm Gezgininde klasör ardından **Ekle**ve ardından **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="4d845-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="4d845-139">Zaman **Yeni Öğe Ekle** iletişim kutusu görüntülenir, sınıf dosya adı **BookDetails.cs**ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="4d845-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="4d845-140">Zaman **BookDetails.cs** dosya açıldığında, dosyasındaki kodu aşağıdaki ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4d845-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="4d845-141">Kaydet ve Kapat **BookDetails.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="4d845-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="4d845-142">Kitap denetleyicisi için Web API projesi ekleyin:</span><span class="sxs-lookup"><span data-stu-id="4d845-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="4d845-143">Sağ **denetleyicileri** Çözüm Gezgininde klasör ardından **Ekle**ve ardından **denetleyicisi**.</span><span class="sxs-lookup"><span data-stu-id="4d845-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="4d845-144">Zaman **İskele Ekle** iletişim kutusu görüntülenir, vurgulayın **Web API 2 denetleyici - boş**ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="4d845-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="4d845-145">Zaman **denetleyici Ekle** iletişim kutusu görüntülenir, denetleyici adı **BooksController**ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="4d845-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="4d845-146">Zaman **BooksController.cs** dosya açıldığında, dosyasındaki kodu aşağıdaki ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4d845-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="4d845-147">Kaydet ve Kapat **BooksController.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="4d845-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="4d845-148">Hataları denetlemek için Web API uygulaması oluşturma.</span><span class="sxs-lookup"><span data-stu-id="4d845-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="4d845-149">2. adım: Windows Phone 8 kitap katalog projenize ekleme</span><span class="sxs-lookup"><span data-stu-id="4d845-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="4d845-150">Bu uçtan uca senaryoyu bir sonraki adım, Windows Phone 8 için katalog uygulama oluşturmaktır.</span><span class="sxs-lookup"><span data-stu-id="4d845-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="4d845-151">Bu uygulamayı kullanmak *Windows Phone veriye bağlı uygulama* şablonu varsayılan kullanıcı arabirimi ve onu için oluşturduğunuz Web API uygulama kullanacağınız [1. adım](#STEP1) veri kaynağı olarak bu öğreticinin.</span><span class="sxs-lookup"><span data-stu-id="4d845-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="4d845-152">Sağ **kitap** çözümde Çözüm Gezgini'nde ardından **Ekle**ve ardından **yeni proje**.</span><span class="sxs-lookup"><span data-stu-id="4d845-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="4d845-153">Zaman **yeni proje** iletişim kutusu görüntülenir, genişletin **yüklü**, ardından **Visual C#** ve ardından **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="4d845-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="4d845-154">Vurgula **Windows Phone veriye bağlı uygulama**, girin **BookCatalog** adı ve ardından **Tamam**.</span><span class="sxs-lookup"><span data-stu-id="4d845-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="4d845-155">Json.NET NuGet paketi ekleme **BookCatalog** proje:</span><span class="sxs-lookup"><span data-stu-id="4d845-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="4d845-156">Sağ **başvuruları** için **BookCatalog** Çözüm Gezgini'nde proje ve ardından **NuGet paketlerini Yönet**.</span><span class="sxs-lookup"><span data-stu-id="4d845-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="4d845-157">Zaman **NuGet paketlerini Yönet** iletişim kutusu görüntülenir, genişletin **çevrimiçi** bölümünde ve vurgulayın **nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="4d845-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="4d845-158">Girin **Json.NET** arama alan ve arama simgesine tıklayın.</span><span class="sxs-lookup"><span data-stu-id="4d845-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="4d845-159">Vurgula **Json.NET** arama sonuçları ve ardından **yükleme**.</span><span class="sxs-lookup"><span data-stu-id="4d845-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="4d845-160">Yükleme tamamlandığında, tıklayın **Kapat**.</span><span class="sxs-lookup"><span data-stu-id="4d845-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="4d845-161">Ekleme **BookDetails** için model **BookCatalog** proje; bu kitap sınıfının genel bir modeli içerir:</span><span class="sxs-lookup"><span data-stu-id="4d845-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="4d845-162">Sağ **BookCatalog** Çözüm Gezgini'nde proje ve ardından **Ekle**ve ardından **yeni klasör**.</span><span class="sxs-lookup"><span data-stu-id="4d845-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="4d845-163">Yeni bir klasör adı **modelleri**.</span><span class="sxs-lookup"><span data-stu-id="4d845-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="4d845-164">Sağ **modelleri** Çözüm Gezgininde klasör ardından **Ekle**ve ardından **sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="4d845-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="4d845-165">Zaman **Yeni Öğe Ekle** iletişim kutusu görüntülenir, sınıf dosya adı **BookDetails.cs**ve ardından **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="4d845-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="4d845-166">Zaman **BookDetails.cs** dosya açıldığında, dosyasındaki kodu aşağıdaki ile değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4d845-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="4d845-167">Kaydet ve Kapat **BookDetails.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="4d845-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="4d845-168">Güncelleştirme **MainViewModel.cs** sınıf kitaplığı Web API uygulamayla iletişim kurmak için işlevselliğini içerir:</span><span class="sxs-lookup"><span data-stu-id="4d845-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="4d845-169">Genişletme **ViewModels** Çözüm Gezgini ve çift tıklatarak klasöründe **MainViewModel.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="4d845-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="4d845-170">Zaman **MainViewModel.cs** dosya açıldığında, dosyasındaki kodu aşağıdaki ile değiştirin; değerini güncelleştirmeniz gerekecektir Not `apiUrl` Web API gerçek URL'si ile sabit:</span><span class="sxs-lookup"><span data-stu-id="4d845-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="4d845-171">Kaydet ve Kapat **MainViewModel.cs** dosya.</span><span class="sxs-lookup"><span data-stu-id="4d845-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="4d845-172">Güncelleştirme **MainPage.xaml** uygulama adı özelleştirmek için dosya:</span><span class="sxs-lookup"><span data-stu-id="4d845-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="4d845-173">Çift **MainPage.xaml** Çözüm Gezgini'nde dosya.</span><span class="sxs-lookup"><span data-stu-id="4d845-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="4d845-174">Zaman **MainPage.xaml** dosya açıldığında, aşağıdaki kod satırlarını bulun:</span><span class="sxs-lookup"><span data-stu-id="4d845-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="4d845-175">Bu satırlar aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4d845-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="4d845-176">Kaydet ve Kapat **MainPage.xaml** dosya.</span><span class="sxs-lookup"><span data-stu-id="4d845-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="4d845-177">Güncelleştirme **DetailsPage.xaml** görüntülenen öğelerini özelleştirmek için dosya:</span><span class="sxs-lookup"><span data-stu-id="4d845-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="4d845-178">Çift **DetailsPage.xaml** Çözüm Gezgini'nde dosya.</span><span class="sxs-lookup"><span data-stu-id="4d845-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="4d845-179">Zaman **DetailsPage.xaml** dosya açıldığında, aşağıdaki kod satırlarını bulun:</span><span class="sxs-lookup"><span data-stu-id="4d845-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="4d845-180">Bu satırlar aşağıdakiyle değiştirin:</span><span class="sxs-lookup"><span data-stu-id="4d845-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="4d845-181">Kaydet ve Kapat **DetailsPage.xaml** dosya.</span><span class="sxs-lookup"><span data-stu-id="4d845-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="4d845-182">Hataları denetlemek için Windows Phone Uygulama oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4d845-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="4d845-183">3. adım: uçtan uca çözüm test ediliyor</span><span class="sxs-lookup"><span data-stu-id="4d845-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="4d845-184">Bölümünde belirtildiği gibi *Önkoşullar* bölüm Web API ve Windows Phone 8 arasındaki bağlantıyı sınarken Bu öğreticinin yerel sisteminizde projeleri,'ndaki yönergeleri izleyin gerekecek *[ Yerel bir bilgisayarda Web API uygulamalarını Windows Phone 8 öykünücüsü bağlanma](https://go.microsoft.com/fwlink/?LinkId=324014)* makale test ortamınızı ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="4d845-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="4d845-185">Yapılandırılmış test ortamını olduktan sonra Windows Phone Uygulama başlangıç projesi olarak ayarlamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="4d845-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="4d845-186">Bunu yapmak için vurgulayın **BookCatalog** Çözüm Gezgini ve ardından uygulama **başlangıç projesi olarak ayarla**:</span><span class="sxs-lookup"><span data-stu-id="4d845-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="4d845-187">Genişletmek için tıklayın</span><span class="sxs-lookup"><span data-stu-id="4d845-187">Click image to expand</span></span> |

<span data-ttu-id="4d845-188">F5 tuşuna bastığınızda, hem Windows Phone hangi görüntüler öykünücüsü, Visual Studio başlayacak bir &quot;bekleyin&quot; uygulama verileri, Web API öğesinden alındığı sırada ileti:</span><span class="sxs-lookup"><span data-stu-id="4d845-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="4d845-189">Genişletmek için tıklayın</span><span class="sxs-lookup"><span data-stu-id="4d845-189">Click image to expand</span></span> |

<span data-ttu-id="4d845-190">Her şeyi başarılı olursa, görüntülenen katalog görmeniz gerekir:</span><span class="sxs-lookup"><span data-stu-id="4d845-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="4d845-191">Genişletmek için tıklayın</span><span class="sxs-lookup"><span data-stu-id="4d845-191">Click image to expand</span></span> |

<span data-ttu-id="4d845-192">Tüm kitap başlığı dokunursanız, uygulama rehberi açıklama görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="4d845-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="4d845-193">Genişletmek için tıklayın</span><span class="sxs-lookup"><span data-stu-id="4d845-193">Click image to expand</span></span> |

<span data-ttu-id="4d845-194">Uygulama, Web API'si ile iletişim kuramıyor, bir hata iletisi görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="4d845-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="4d845-195">Genişletmek için tıklayın</span><span class="sxs-lookup"><span data-stu-id="4d845-195">Click image to expand</span></span> |

<span data-ttu-id="4d845-196">Hata iletisinde dokunursanız, hatayla ilgili ek ayrıntılar görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="4d845-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="4d845-197">Genişletmek için tıklayın</span><span class="sxs-lookup"><span data-stu-id="4d845-197">Click image to expand</span></span>                                                                 |

