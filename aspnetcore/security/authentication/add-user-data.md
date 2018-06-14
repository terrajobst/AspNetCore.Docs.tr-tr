---
title: Ekle, indirin ve ASP.NET Core projesinde kimliğine özel kullanıcı verilerini sil
author: rick-anderson
description: Özel kullanıcı verilerini kimliğine ASP.NET Core projesinde eklemeyi öğrenin. Veri GDPR başına silin.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 6/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/add-user-data
ms.openlocfilehash: 3ebb709cc40f6c2477ac57325d035b9b461e2eaf
ms.sourcegitcommit: 0d6f151e69c159d776ed0142773279e645edbc0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2018
ms.locfileid: "35415000"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="72685-104">Ekle, indirin ve ASP.NET Core projesinde kimliğine özel kullanıcı verilerini sil</span><span class="sxs-lookup"><span data-stu-id="72685-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="72685-105">tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="72685-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="72685-106">Bu makalede gösterilmektedir nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="72685-106">This article shows how to:</span></span>

* <span data-ttu-id="72685-107">Özel kullanıcı verilerini bir ASP.NET Core web uygulaması'na ekleyin.</span><span class="sxs-lookup"><span data-stu-id="72685-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="72685-108">Özel kullanıcı veri modeli ile işaretleme [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) indirme ve silme işlemini otomatik olarak kullanılabilir olacak şekilde özniteliği.</span><span class="sxs-lookup"><span data-stu-id="72685-108">Decorate the custom user data model with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="72685-109">İndirilen ve silinmiş verilerin sağlama yardımcı karşılayan [GDPR](xref:security/gdpr) gereksinimleri.</span><span class="sxs-lookup"><span data-stu-id="72685-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="72685-110">Proje örnek bir Razor sayfalarının web uygulamasından oluşturuldu, ancak bir ASP.NET Core MVC web uygulaması için benzer bir yönergelerdir.</span><span class="sxs-lookup"><span data-stu-id="72685-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="72685-111">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="72685-111">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="72685-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="72685-112">Prerequisites</span></span>

[!INCLUDE [](~/includes/2.1-SDK.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="72685-113">Bir Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="72685-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="72685-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="72685-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="72685-115">Visual Studio'dan **dosya** menüsünde, select **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="72685-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="72685-116">Proje adı **WebApp1** kendisine istiyorsanız, ad alanı eşleşen [örnek indirme](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) kodu.</span><span class="sxs-lookup"><span data-stu-id="72685-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/authentication/add-user-data/sample) code.</span></span>
* <span data-ttu-id="72685-117">Seçin **ASP.NET Core Web uygulaması** > **Tamam**</span><span class="sxs-lookup"><span data-stu-id="72685-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="72685-118">Seçin **ASP.NET Core 2.1** açılır</span><span class="sxs-lookup"><span data-stu-id="72685-118">Select **ASP.NET Core 2.1** in the dropdown</span></span>
* <span data-ttu-id="72685-119">Seçin **Web uygulaması**  > **Tamam**</span><span class="sxs-lookup"><span data-stu-id="72685-119">Select **Web Application**  > **OK**</span></span>
* <span data-ttu-id="72685-120">Oluşturun ve projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="72685-120">Build and run the project.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="72685-121">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="72685-121">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet new webapp -o WebApp1
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="72685-122">Kimlik iskele kurucu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="72685-122">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="72685-123">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="72685-123">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="72685-124">Gelen **Çözüm Gezgini**, projeye sağ tıklayın > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="72685-124">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="72685-125">Sol bölmesinden **İskele Ekle** iletişim kutusunda **kimlik** > **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="72685-125">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="72685-126">İçinde **Ekle kimlik** iletişim kutusunda aşağıdaki seçenekleri:</span><span class="sxs-lookup"><span data-stu-id="72685-126">In the **ADD Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="72685-127">Varolan düzeni dosyanızı seçmek *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="72685-127">Select your existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="72685-128">Geçersiz kılmak için aşağıdaki dosyaları seçin:</span><span class="sxs-lookup"><span data-stu-id="72685-128">Select the following files to override:</span></span>
    * <span data-ttu-id="72685-129">**Hesap/kaydı**</span><span class="sxs-lookup"><span data-stu-id="72685-129">**Account/Register**</span></span>
    * <span data-ttu-id="72685-130">**Hesap / / dizini Yönet**</span><span class="sxs-lookup"><span data-stu-id="72685-130">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="72685-131">Seçin **+** yeni düğmesi **veri bağlamı sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="72685-131">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="72685-132">Türünü kabul (**WebApp1.Models.WebApp1Context** proje adlandırırsanız **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="72685-132">Accept the type (**WebApp1.Models.WebApp1Context** if you named the project **WebApp1**).</span></span>
  * <span data-ttu-id="72685-133">Seçin **+** yeni düğmesi **kullanıcı sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="72685-133">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="72685-134">Türünü kabul (**WebApp1User** proje adlandırırsanız **WebApp1**) > **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="72685-134">Accept the type (**WebApp1User** if you named the project **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="72685-135">Seçin **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="72685-135">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="72685-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="72685-136">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="72685-137">ASP.NET iskele kurucu daha önce yüklemediyseniz şimdi yükle:</span><span class="sxs-lookup"><span data-stu-id="72685-137">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="72685-138">Paket için bir başvuru ekleyin [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) proje (.csproj) dosyasına.</span><span class="sxs-lookup"><span data-stu-id="72685-138">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="72685-139">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="72685-139">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="72685-140">Kimlik iskele kurucu seçeneklerinden listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="72685-140">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="72685-141">Proje klasöründe kimlik iskele kurucu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="72685-141">In the project folder, run the Identity scaffolder:</span></span>

```cli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

-------------

<span data-ttu-id="72685-142">İçindeki yönergeleri izleyin [geçişler, UseAuthentication ve düzeni](xref:security/authentication/scaffold-identity#efm) aşağıdaki adımları gerçekleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="72685-142">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="72685-143">Bir geçiş oluşturun ve veritabanı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="72685-143">Create a migration and update the database.</span></span>
* <span data-ttu-id="72685-144">Add `UseAuthentication` to `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="72685-144">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="72685-145">Ekleme `<partial name="_LoginPartial" />` düzeni dosyasına kayıt yapar.</span><span class="sxs-lookup"><span data-stu-id="72685-145">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="72685-146">Uygulamayı test etme:</span><span class="sxs-lookup"><span data-stu-id="72685-146">Test the app:</span></span>
  * <span data-ttu-id="72685-147">Bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="72685-147">Register a user</span></span>
  * <span data-ttu-id="72685-148">Yeni bir kullanıcı adı seçin (yanına **oturum kapatma** bağlantı).</span><span class="sxs-lookup"><span data-stu-id="72685-148">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="72685-149">Pencereyi genişletin veya kullanıcı adı ve diğer bağlantıları göstermek için gezinti çubuğu simgesini seçin gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="72685-149">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="72685-150">Seçin **kişisel veriler** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="72685-150">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="72685-151">Seçin **karşıdan** düğmesine tıklayın ve incelenmesi *PersonalData.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="72685-151">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="72685-152">Test **silmek** oturum açan kullanıcı siler düğmesi.</span><span class="sxs-lookup"><span data-stu-id="72685-152">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="72685-153">Özel kullanıcı verilerini kimlik DB ekleme</span><span class="sxs-lookup"><span data-stu-id="72685-153">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="72685-154">Güncelleştirme `IdentityUser` türetilmiş sınıf özel özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="72685-154">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="72685-155">Projeniz WebApp1 adlı, dosya adında *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="72685-155">If you named your project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="72685-156">Dosya, aşağıdaki kod ile güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="72685-156">Update the file with the following code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Data/WebApp1User.cs)]

<span data-ttu-id="72685-157">Özellikler donatılmış ile [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) özniteliği şunlardır:</span><span class="sxs-lookup"><span data-stu-id="72685-157">Properties decorated with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute?view=aspnetcore-2.1) attribute are:</span></span>

* <span data-ttu-id="72685-158">Ne zaman silinmiş *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor sayfasını çağırır `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="72685-158">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="72685-159">Tarafından yüklenen veriler dahil *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor sayfası.</span><span class="sxs-lookup"><span data-stu-id="72685-159">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="72685-160">Güncelleştirme Account/Manage/Index.cshtml sayfası</span><span class="sxs-lookup"><span data-stu-id="72685-160">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="72685-161">Güncelleştirme `InputModel` içinde *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="72685-161">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,87-95,120)]

<span data-ttu-id="72685-162">Güncelleştirme *Areas/Identity/Pages/Account/Manage/Index.cshtml* aşağıdaki vurgulanmış biçimlendirmeyi ile:</span><span class="sxs-lookup"><span data-stu-id="72685-162">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=34-41)]

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="72685-163">Güncelleştirme Account/Register.cshtml sayfası</span><span class="sxs-lookup"><span data-stu-id="72685-163">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="72685-164">Güncelleştirme `InputModel` içinde *Areas/Identity/Pages/Account/Register.cshtml.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="72685-164">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

[!code-csharp[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=8-16,43,44)]

<span data-ttu-id="72685-165">Güncelleştirme *Areas/Identity/Pages/Account/Register.cshtml* aşağıdaki vurgulanmış biçimlendirmeyi ile:</span><span class="sxs-lookup"><span data-stu-id="72685-165">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-html[Main](add-user-data/sample/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

<span data-ttu-id="72685-166">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="72685-166">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="72685-167">Özel kullanıcı verileri için bir geçiş ekleme</span><span class="sxs-lookup"><span data-stu-id="72685-167">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="72685-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="72685-168">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="72685-169">Visual Studio'da **Paket Yöneticisi Konsolu**:</span><span class="sxs-lookup"><span data-stu-id="72685-169">In the Visual Studio **Package Manager Console**:</span></span>

```PMC
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="72685-170">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="72685-170">.NET Core CLI</span></span>](#tab/netcore-cli)

```cli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

------

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="72685-171">Test oluşturun, görüntüleyin, indirin, özel kullanıcı verilerini sil</span><span class="sxs-lookup"><span data-stu-id="72685-171">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="72685-172">Uygulamayı test etme:</span><span class="sxs-lookup"><span data-stu-id="72685-172">Test the app:</span></span>

* <span data-ttu-id="72685-173">Yeni bir kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="72685-173">Register a new user.</span></span>
* <span data-ttu-id="72685-174">Özel kullanıcı verilerini görüntülemek `/Identity/Account/Manage` sayfası.</span><span class="sxs-lookup"><span data-stu-id="72685-174">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="72685-175">Karşıdan yükle ve kullanıcıların kişisel verileri görüntülemek `/Identity/Account/Manage/PersonalData` sayfası.</span><span class="sxs-lookup"><span data-stu-id="72685-175">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
