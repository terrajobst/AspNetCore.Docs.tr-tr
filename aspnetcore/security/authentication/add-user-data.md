---
title: Ekleme, indirmek ve bir ASP.NET Core projesi kimliği için kullanıcı verilerini silme
author: rick-anderson
description: Bir ASP.NET Core projesi içinde kimlik için özel kullanıcı veri eklemeyi öğrenin. GDPR başına verileri silin.
ms.author: riande
ms.date: 01/28/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: e08c02e2e5d4a429aae10c59e7ae3ea48c975067
ms.sourcegitcommit: c81ef12a1b6e6ac838e5e07042717cf492e6635b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76885552"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="22fd2-104">Ekleme, indirmek ve kimlik için bir ASP.NET Core projesi özel kullanıcı verilerini silme</span><span class="sxs-lookup"><span data-stu-id="22fd2-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="22fd2-105">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="22fd2-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="22fd2-106">Bu makale nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="22fd2-106">This article shows how to:</span></span>

* <span data-ttu-id="22fd2-107">ASP.NET Core web uygulaması için özel kullanıcı verilerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="22fd2-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="22fd2-108">Özel Kullanıcı veri modelini <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> özniteliğiyle işaretleyin, bu nedenle otomatik olarak indirilebilir ve silinirler.</span><span class="sxs-lookup"><span data-stu-id="22fd2-108">Mark the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="22fd2-109">Verileri silinir ve indirilen mümkün hale yardımcı karşılamak [GDPR](xref:security/gdpr) gereksinimleri.</span><span class="sxs-lookup"><span data-stu-id="22fd2-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="22fd2-110">Bir Razor sayfaları web uygulaması proje örneği oluşturulur, ancak yönergeleri ASP.NET Core MVC web uygulaması için benzerdir.</span><span class="sxs-lookup"><span data-stu-id="22fd2-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="22fd2-111">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="22fd2-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="22fd2-112">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="22fd2-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="22fd2-113">Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="22fd2-113">Create a Razor web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22fd2-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22fd2-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="22fd2-115">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="22fd2-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="22fd2-116">Projeyi adlandırın **WebApp1** için istiyorsanız ad alanı eşleşen [örneği indirin](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) kod.</span><span class="sxs-lookup"><span data-stu-id="22fd2-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="22fd2-117">**ASP.NET Core Web uygulaması** > **Tamam 'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="22fd2-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="22fd2-118">Açılan listede **ASP.NET Core 3,0** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="22fd2-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="22fd2-119">**Web uygulaması** > **Tamam 'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="22fd2-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="22fd2-120">Derleme ve projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="22fd2-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="22fd2-121">Visual Studio'dan **dosya** menüsünde **yeni** > **proje**.</span><span class="sxs-lookup"><span data-stu-id="22fd2-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="22fd2-122">Projeyi adlandırın **WebApp1** için istiyorsanız ad alanı eşleşen [örneği indirin](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) kod.</span><span class="sxs-lookup"><span data-stu-id="22fd2-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="22fd2-123">**ASP.NET Core Web uygulaması** > **Tamam 'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="22fd2-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="22fd2-124">Açılan listede **ASP.NET Core 2,2** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="22fd2-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="22fd2-125">**Web uygulaması** > **Tamam 'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="22fd2-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="22fd2-126">Derleme ve projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="22fd2-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="22fd2-127">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="22fd2-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="22fd2-128">Kimlik iskele kurucu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="22fd2-128">Run the Identity scaffolder</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22fd2-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22fd2-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="22fd2-130">Gelen **Çözüm Gezgini**, projeye sağ tıklayın > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="22fd2-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="22fd2-131">**Yapı Iskelesi Ekle** iletişim kutusunun sol bölmesinde **kimlik** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="22fd2-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="22fd2-132">**Kimlik Ekle** iletişim kutusunda aşağıdaki seçenekler:</span><span class="sxs-lookup"><span data-stu-id="22fd2-132">In the **Add Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="22fd2-133">Varolan bir düzen dosyası seçin *~/Pages/Shared/_Layout.cshtml*</span><span class="sxs-lookup"><span data-stu-id="22fd2-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="22fd2-134">Aşağıdaki dosyalar geçersiz kılmak için seçin:</span><span class="sxs-lookup"><span data-stu-id="22fd2-134">Select the following files to override:</span></span>
    * <span data-ttu-id="22fd2-135">**Hesabı/kaydı**</span><span class="sxs-lookup"><span data-stu-id="22fd2-135">**Account/Register**</span></span>
    * <span data-ttu-id="22fd2-136">**Hesabı/yönetmek/dizin**</span><span class="sxs-lookup"><span data-stu-id="22fd2-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="22fd2-137">Seçin **+** yeni bir düğme **veri bağlamı sınıfının**.</span><span class="sxs-lookup"><span data-stu-id="22fd2-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="22fd2-138">Tür kabul et (**WebApp1.Models.WebApp1Context** proje adlandırılmışsa **WebApp1**).</span><span class="sxs-lookup"><span data-stu-id="22fd2-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="22fd2-139">Seçin **+** yeni bir düğme **kullanıcı sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="22fd2-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="22fd2-140">Tür kabul et (**WebApp1User** proje adlandırılmışsa **WebApp1**) > **Ekle**.</span><span class="sxs-lookup"><span data-stu-id="22fd2-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="22fd2-141">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="22fd2-141">Select **Add**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="22fd2-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="22fd2-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="22fd2-143">ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="22fd2-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="22fd2-144">Paket başvurusu ekleme [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) proje (.csproj) dosyasına.</span><span class="sxs-lookup"><span data-stu-id="22fd2-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="22fd2-145">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="22fd2-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="22fd2-146">Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="22fd2-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="22fd2-147">Proje klasöründe kimlik iskele kurucu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="22fd2-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="22fd2-148">Verilen yönergeleri izleyin [düzeni geçişleri ve UseAuthentication](xref:security/authentication/scaffold-identity#efm) aşağıdaki adımları gerçekleştirmek için:</span><span class="sxs-lookup"><span data-stu-id="22fd2-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="22fd2-149">Bir geçiş oluşturmak ve veritabanı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="22fd2-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="22fd2-150">Add `UseAuthentication` to `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="22fd2-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="22fd2-151">Ekleme `<partial name="_LoginPartial" />` Düzen dosyası için.</span><span class="sxs-lookup"><span data-stu-id="22fd2-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="22fd2-152">Uygulamayı test edin:</span><span class="sxs-lookup"><span data-stu-id="22fd2-152">Test the app:</span></span>
  * <span data-ttu-id="22fd2-153">Bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="22fd2-153">Register a user</span></span>
  * <span data-ttu-id="22fd2-154">Yeni kullanıcı adını seçin (yanındaki **oturum kapatma** bağlantı).</span><span class="sxs-lookup"><span data-stu-id="22fd2-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="22fd2-155">Penceresini genişletin veya kullanıcı adı ve diğer bağlantıları göstermek için gezinti çubuğu simgesini gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="22fd2-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="22fd2-156">Seçin **kişisel verileri** sekmesi.</span><span class="sxs-lookup"><span data-stu-id="22fd2-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="22fd2-157">Seçin **indirme** düğmesine tıklayın ve denetlenen *PersonalData.json* dosya.</span><span class="sxs-lookup"><span data-stu-id="22fd2-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="22fd2-158">Test **Sil** düğmesi, oturum açmış kullanıcıyı siler.</span><span class="sxs-lookup"><span data-stu-id="22fd2-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="22fd2-159">Özel kullanıcı verilerini kimlik Veritabanına Ekle</span><span class="sxs-lookup"><span data-stu-id="22fd2-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="22fd2-160">Güncelleştirme `IdentityUser` türetilmiş sınıf özel özelliklere sahip.</span><span class="sxs-lookup"><span data-stu-id="22fd2-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="22fd2-161">' % S'proje WebApp1 adlı, dosyanın nasıl adlandırıldığı *Areas/Identity/Data/WebApp1User.cs*.</span><span class="sxs-lookup"><span data-stu-id="22fd2-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="22fd2-162">Dosya, aşağıdaki kod ile güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="22fd2-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="22fd2-163">[Personal Data](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) özniteliğiyle birlikte bulunan özellikler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="22fd2-163">Properties with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="22fd2-164">Ne zaman silinmiş *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor sayfası çağırır `UserManager.Delete`.</span><span class="sxs-lookup"><span data-stu-id="22fd2-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="22fd2-165">Tarafından yüklenen veriler dahil *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor sayfası.</span><span class="sxs-lookup"><span data-stu-id="22fd2-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="22fd2-166">Account/Manage/Index.cshtml sayfası</span><span class="sxs-lookup"><span data-stu-id="22fd2-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="22fd2-167">Güncelleştirme `InputModel` içinde *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="22fd2-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="22fd2-168">Güncelleştirme *Areas/Identity/Pages/Account/Manage/Index.cshtml* aşağıdaki vurgulanmış biçimlendirmeye sahip:</span><span class="sxs-lookup"><span data-stu-id="22fd2-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="22fd2-169">Güncelleştirme *Areas/Identity/Pages/Account/Manage/Index.cshtml* aşağıdaki vurgulanmış biçimlendirmeye sahip:</span><span class="sxs-lookup"><span data-stu-id="22fd2-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="22fd2-170">Account/Register.cshtml sayfası</span><span class="sxs-lookup"><span data-stu-id="22fd2-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="22fd2-171">Güncelleştirme `InputModel` içinde *Areas/Identity/Pages/Account/Register.cshtml.cs* aşağıdaki vurgulanmış kodu:</span><span class="sxs-lookup"><span data-stu-id="22fd2-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="22fd2-172">Güncelleştirme *Areas/Identity/Pages/Account/Register.cshtml* aşağıdaki vurgulanmış biçimlendirmeye sahip:</span><span class="sxs-lookup"><span data-stu-id="22fd2-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="22fd2-173">Güncelleştirme *Areas/Identity/Pages/Account/Register.cshtml* aşağıdaki vurgulanmış biçimlendirmeye sahip:</span><span class="sxs-lookup"><span data-stu-id="22fd2-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="22fd2-174">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="22fd2-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="22fd2-175">Özel kullanıcı verileri için bir geçiş ekleyin</span><span class="sxs-lookup"><span data-stu-id="22fd2-175">Add a migration for the custom user data</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="22fd2-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="22fd2-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="22fd2-177">Visual Studio **Paket Yöneticisi Konsolu**:</span><span class="sxs-lookup"><span data-stu-id="22fd2-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="22fd2-178">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="22fd2-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="22fd2-179">Test oluşturun, görüntüleyin, indirin, özel kullanıcı verilerini sil</span><span class="sxs-lookup"><span data-stu-id="22fd2-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="22fd2-180">Uygulamayı test edin:</span><span class="sxs-lookup"><span data-stu-id="22fd2-180">Test the app:</span></span>

* <span data-ttu-id="22fd2-181">Yeni bir kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="22fd2-181">Register a new user.</span></span>
* <span data-ttu-id="22fd2-182">Özel kullanıcı veri görünümünde `/Identity/Account/Manage` sayfası.</span><span class="sxs-lookup"><span data-stu-id="22fd2-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="22fd2-183">İndirme ve görüntüleme kullanıcıların kişisel verilerden `/Identity/Account/Manage/PersonalData` sayfası.</span><span class="sxs-lookup"><span data-stu-id="22fd2-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
