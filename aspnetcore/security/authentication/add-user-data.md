---
title: Ekleme, indirmek ve bir ASP.NET Core projesi kimliği için kullanıcı verilerini silme
author: rick-anderson
description: Bir ASP.NET Core projesi içinde kimlik için özel kullanıcı veri eklemeyi öğrenin. GDPR başına verileri silin.
ms.author: riande
ms.date: 01/28/2020
ms.custom: mvc, seodec18
uid: security/authentication/add-user-data
ms.openlocfilehash: 7a67f55da0e685ed3fd5badb30e8be683411a5ae
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78662689"
---
# <a name="add-download-and-delete-custom-user-data-to-identity-in-an-aspnet-core-project"></a><span data-ttu-id="b2c0a-104">Ekleme, indirmek ve kimlik için bir ASP.NET Core projesi özel kullanıcı verilerini silme</span><span class="sxs-lookup"><span data-stu-id="b2c0a-104">Add, download, and delete custom user data to Identity in an ASP.NET Core project</span></span>

<span data-ttu-id="b2c0a-105">Gönderen [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b2c0a-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b2c0a-106">Bu makale nasıl yapılır:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-106">This article shows how to:</span></span>

* <span data-ttu-id="b2c0a-107">ASP.NET Core web uygulaması için özel kullanıcı verilerini ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-107">Add custom user data to an ASP.NET Core web app.</span></span>
* <span data-ttu-id="b2c0a-108">Özel Kullanıcı veri modelini <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> özniteliğiyle işaretleyin, bu nedenle otomatik olarak indirilebilir ve silinirler.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-108">Mark the custom user data model with the <xref:Microsoft.AspNetCore.Identity.PersonalDataAttribute> attribute so it's automatically available for download and deletion.</span></span> <span data-ttu-id="b2c0a-109">Verilerin indirilip silinebilmesini sağlamak, [GDPR](xref:security/gdpr) gereksinimlerini karşılamaya yardımcı olur.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-109">Making the data able to be downloaded and deleted helps meet [GDPR](xref:security/gdpr) requirements.</span></span>

<span data-ttu-id="b2c0a-110">Bir Razor sayfaları web uygulaması proje örneği oluşturulur, ancak yönergeleri ASP.NET Core MVC web uygulaması için benzerdir.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-110">The project sample is created from a Razor Pages web app, but the instructions are similar for a ASP.NET Core MVC web app.</span></span>

<span data-ttu-id="b2c0a-111">[Örnek kodu görüntüleme veya indirme](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([nasıl indirileceği](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b2c0a-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/add-user-data) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b2c0a-112">Önkoşullar</span><span class="sxs-lookup"><span data-stu-id="b2c0a-112">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [](~/includes/3.0-SDK.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [](~/includes/2.2-SDK.md)]

::: moniker-end

## <a name="create-a-razor-web-app"></a><span data-ttu-id="b2c0a-113">Razor web uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="b2c0a-113">Create a Razor web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b2c0a-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2c0a-114">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="b2c0a-115">Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="b2c0a-116">[İndirme örnek](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) kodunun ad alanıyla eşleştirmek Istiyorsanız projeyi **WebApp1** olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-116">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="b2c0a-117">**ASP.NET Core Web uygulaması** > **Tamam 'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="b2c0a-117">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="b2c0a-118">Açılan listede **ASP.NET Core 3,0** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="b2c0a-118">Select **ASP.NET Core 3.0** in the dropdown</span></span>
* <span data-ttu-id="b2c0a-119">**Web uygulaması** > **Tamam 'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="b2c0a-119">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="b2c0a-120">Derleme ve projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-120">Build and run the project.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="b2c0a-121">Visual Studio **Dosya** menüsünden **Yeni** > **projesi**' ni seçin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-121">From the Visual Studio **File** menu, select **New** > **Project**.</span></span> <span data-ttu-id="b2c0a-122">[İndirme örnek](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) kodunun ad alanıyla eşleştirmek Istiyorsanız projeyi **WebApp1** olarak adlandırın.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-122">Name the project **WebApp1** if you want to it match the namespace of the [download sample](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/authentication/add-user-data) code.</span></span>
* <span data-ttu-id="b2c0a-123">**ASP.NET Core Web uygulaması** > **Tamam 'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="b2c0a-123">Select **ASP.NET Core Web Application** > **OK**</span></span>
* <span data-ttu-id="b2c0a-124">Açılan listede **ASP.NET Core 2,2** ' i seçin</span><span class="sxs-lookup"><span data-stu-id="b2c0a-124">Select **ASP.NET Core 2.2** in the dropdown</span></span>
* <span data-ttu-id="b2c0a-125">**Web uygulaması** > **Tamam 'ı** seçin</span><span class="sxs-lookup"><span data-stu-id="b2c0a-125">Select **Web Application** > **OK**</span></span>
* <span data-ttu-id="b2c0a-126">Derleme ve projeyi çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-126">Build and run the project.</span></span>

::: moniker-end


# <a name="net-core-cli"></a>[<span data-ttu-id="b2c0a-127">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b2c0a-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet new webapp -o WebApp1
```

---

## <a name="run-the-identity-scaffolder"></a><span data-ttu-id="b2c0a-128">Kimlik iskele kurucu çalıştırın</span><span class="sxs-lookup"><span data-stu-id="b2c0a-128">Run the Identity scaffolder</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b2c0a-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2c0a-129">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b2c0a-130">**Çözüm Gezgini**, projeye sağ tıklayıp **Yeni > Iskli öğe** **ekleyin** >.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-130">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="b2c0a-131">**Yapı Iskelesi Ekle** iletişim kutusunun sol bölmesinde **kimlik** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-131">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="b2c0a-132">**Kimlik Ekle** iletişim kutusunda aşağıdaki seçenekler:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-132">In the **Add Identity** dialog, the following options:</span></span>
  * <span data-ttu-id="b2c0a-133">Var olan düzen dosyasını seçin *~/Pages/Shared/_Layout. cshtml*</span><span class="sxs-lookup"><span data-stu-id="b2c0a-133">Select the existing layout  file  *~/Pages/Shared/_Layout.cshtml*</span></span>
  * <span data-ttu-id="b2c0a-134">Aşağıdaki dosyalar geçersiz kılmak için seçin:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-134">Select the following files to override:</span></span>
    * <span data-ttu-id="b2c0a-135">**Hesap/kayıt**</span><span class="sxs-lookup"><span data-stu-id="b2c0a-135">**Account/Register**</span></span>
    * <span data-ttu-id="b2c0a-136">**Hesap/yönet/Dizin**</span><span class="sxs-lookup"><span data-stu-id="b2c0a-136">**Account/Manage/Index**</span></span>
  * <span data-ttu-id="b2c0a-137">Yeni bir **veri bağlamı sınıfı**oluşturmak için **+** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-137">Select the **+** button to create a new **Data context class**.</span></span> <span data-ttu-id="b2c0a-138">Proje **WebApp1**olarak adlandırılmışsa türü (**WebApp1. modeller. WebApp1Context** ) kabul edin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-138">Accept the type (**WebApp1.Models.WebApp1Context** if the project is named **WebApp1**).</span></span>
  * <span data-ttu-id="b2c0a-139">Yeni bir **Kullanıcı sınıfı**oluşturmak için **+** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-139">Select the **+** button to create a new **User class**.</span></span> <span data-ttu-id="b2c0a-140">**Ekle**> ( **projenin adı** **WebApp1User** ) öğesini kabul edin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-140">Accept the type (**WebApp1User** if the project is named **WebApp1**) > **Add**.</span></span>
* <span data-ttu-id="b2c0a-141">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-141">Select **Add**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="b2c0a-142">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b2c0a-142">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="b2c0a-143">ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-143">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="b2c0a-144">Proje (. csproj) dosyasına [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-144">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (.csproj) file.</span></span> <span data-ttu-id="b2c0a-145">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-145">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="b2c0a-146">Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-146">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="b2c0a-147">Proje klasöründe kimlik iskele kurucu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-147">In the project folder, run the Identity scaffolder:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -u WebApp1User -fi Account.Register;Account.Manage.Index
```

---

<span data-ttu-id="b2c0a-148">Aşağıdaki adımları gerçekleştirmek için [geçişler, UseAuthentication ve düzen](xref:security/authentication/scaffold-identity#efm) bölümündeki yönergeleri izleyin:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-148">Follow the instruction in [Migrations, UseAuthentication, and layout](xref:security/authentication/scaffold-identity#efm) to perform the following steps:</span></span>

* <span data-ttu-id="b2c0a-149">Bir geçiş oluşturmak ve veritabanı güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-149">Create a migration and update the database.</span></span>
* <span data-ttu-id="b2c0a-150">Add `UseAuthentication` to `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-150">Add `UseAuthentication` to `Startup.Configure`.</span></span>
* <span data-ttu-id="b2c0a-151">Düzen dosyasına `<partial name="_LoginPartial" />` ekleyin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-151">Add `<partial name="_LoginPartial" />` to the layout file.</span></span>
* <span data-ttu-id="b2c0a-152">Uygulamayı test edin:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-152">Test the app:</span></span>
  * <span data-ttu-id="b2c0a-153">Bir kullanıcı kaydı</span><span class="sxs-lookup"><span data-stu-id="b2c0a-153">Register a user</span></span>
  * <span data-ttu-id="b2c0a-154">Yeni Kullanıcı adını ( **oturum kapatma** bağlantısının yanında) seçin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-154">Select the new user name (next to the **Logout** link).</span></span> <span data-ttu-id="b2c0a-155">Penceresini genişletin veya kullanıcı adı ve diğer bağlantıları göstermek için gezinti çubuğu simgesini gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-155">You might need to expand the window or select the navigation bar icon to show the user name and other links.</span></span>
  * <span data-ttu-id="b2c0a-156">**Kişisel veri** sekmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-156">Select the **Personal Data** tab.</span></span>
  * <span data-ttu-id="b2c0a-157">**İndir** düğmesini seçin ve *persondata. JSON* dosyasını inceledi.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-157">Select the **Download** button and examined the *PersonalData.json* file.</span></span>
  * <span data-ttu-id="b2c0a-158">Oturum açan kullanıcıyı silen **Sil** düğmesini test edin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-158">Test the **Delete** button, which deletes the logged on user.</span></span>

## <a name="add-custom-user-data-to-the-identity-db"></a><span data-ttu-id="b2c0a-159">Özel kullanıcı verilerini kimlik Veritabanına Ekle</span><span class="sxs-lookup"><span data-stu-id="b2c0a-159">Add custom user data to the Identity DB</span></span>

<span data-ttu-id="b2c0a-160">`IdentityUser` türetilmiş sınıfı özel özelliklerle güncelleştirin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-160">Update the `IdentityUser` derived class with custom properties.</span></span> <span data-ttu-id="b2c0a-161">Projeyi WebApp1 olarak adlandırdıysanız, dosya *Areas/Identity/Data/WebApp1User. cs*olarak adlandırılır.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-161">If you named the project WebApp1, the file is named *Areas/Identity/Data/WebApp1User.cs*.</span></span> <span data-ttu-id="b2c0a-162">Dosya, aşağıdaki kod ile güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-162">Update the file with the following code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Data/WebApp1User.cs)]

::: moniker-end

<span data-ttu-id="b2c0a-163">[Personal Data](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) özniteliğiyle birlikte bulunan özellikler şunlardır:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-163">Properties with the [PersonalData](/dotnet/api/microsoft.aspnetcore.identity.personaldataattribute) attribute are:</span></span>

* <span data-ttu-id="b2c0a-164">*Areas/Identity/Pages/Account/Manage/Deletepersonal Data. cshtml* Razor sayfası `UserManager.Delete`çağırdığında silinir.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-164">Deleted when the *Areas/Identity/Pages/Account/Manage/DeletePersonalData.cshtml* Razor Page calls `UserManager.Delete`.</span></span>
* <span data-ttu-id="b2c0a-165">*Alan/kimlik/sayfa/hesap/Yönet/Downloadpersonal Data. cshtml* Razor sayfasında indirilen verilere dahildir.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-165">Included in the downloaded data by the *Areas/Identity/Pages/Account/Manage/DownloadPersonalData.cshtml* Razor Page.</span></span>

### <a name="update-the-accountmanageindexcshtml-page"></a><span data-ttu-id="b2c0a-166">Account/Manage/Index.cshtml sayfası</span><span class="sxs-lookup"><span data-stu-id="b2c0a-166">Update the Account/Manage/Index.cshtml page</span></span>

<span data-ttu-id="b2c0a-167">*Areas/kimlik/sayfa/hesap/Manage/Index. cshtml. cs* içindeki `InputModel` şu vurgulanmış kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-167">Update the `InputModel` in *Areas/Identity/Pages/Account/Manage/Index.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=24-32,48-49,96-104,106)]

<span data-ttu-id="b2c0a-168">*Bölgeleri/kimliği/sayfaları/hesabı/Yönet/Index. cshtml* 'yi aşağıdaki vurgulanmış işaretlerle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-168">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=18-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml.cs?name=snippet&highlight=28-36,63-64,98-106,119)]

<span data-ttu-id="b2c0a-169">*Bölgeleri/kimliği/sayfaları/hesabı/Yönet/Index. cshtml* 'yi aşağıdaki vurgulanmış işaretlerle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-169">Update the *Areas/Identity/Pages/Account/Manage/Index.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Manage/Index.cshtml?highlight=35-42)]

::: moniker-end

### <a name="update-the-accountregistercshtml-page"></a><span data-ttu-id="b2c0a-170">Account/Register.cshtml sayfası</span><span class="sxs-lookup"><span data-stu-id="b2c0a-170">Update the Account/Register.cshtml page</span></span>

<span data-ttu-id="b2c0a-171">*Areas/kimlik/sayfa/hesap/kayıt. cshtml. cs* ' deki `InputModel` aşağıdaki vurgulanmış kodla güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-171">Update the `InputModel` in *Areas/Identity/Pages/Account/Register.cshtml.cs* with the following highlighted code:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=30-38,70-71)]

<span data-ttu-id="b2c0a-172">*Areas/Identity/Pages/Account/Register. cshtml* ' i aşağıdaki vurgulanmış işaretlerle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-172">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-cshtml[](add-user-data/samples/3.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml.cs?name=snippet&highlight=28-36,67,66)]

<span data-ttu-id="b2c0a-173">*Areas/Identity/Pages/Account/Register. cshtml* ' i aşağıdaki vurgulanmış işaretlerle güncelleştirin:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-173">Update the *Areas/Identity/Pages/Account/Register.cshtml* with the following highlighted markup:</span></span>

[!code-chtml[](add-user-data/samples/2.x/SampleApp/Areas/Identity/Pages/Account/Register.cshtml?highlight=16-25)]

::: moniker-end


<span data-ttu-id="b2c0a-174">Projeyi oluşturun.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-174">Build the project.</span></span>

### <a name="add-a-migration-for-the-custom-user-data"></a><span data-ttu-id="b2c0a-175">Özel kullanıcı verileri için bir geçiş ekleyin</span><span class="sxs-lookup"><span data-stu-id="b2c0a-175">Add a migration for the custom user data</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b2c0a-176">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b2c0a-176">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="b2c0a-177">Visual Studio **Paket Yöneticisi konsolunda**:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-177">In the Visual Studio **Package Manager Console**:</span></span>

```powershell
Add-Migration CustomUserData
Update-Database
```

# <a name="net-core-cli"></a>[<span data-ttu-id="b2c0a-178">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b2c0a-178">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet ef migrations add CustomUserData
dotnet ef database update
```

---

## <a name="test-create-view-download-delete-custom-user-data"></a><span data-ttu-id="b2c0a-179">Test oluşturun, görüntüleyin, indirin, özel kullanıcı verilerini sil</span><span class="sxs-lookup"><span data-stu-id="b2c0a-179">Test create, view, download, delete custom user data</span></span>

<span data-ttu-id="b2c0a-180">Uygulamayı test edin:</span><span class="sxs-lookup"><span data-stu-id="b2c0a-180">Test the app:</span></span>

* <span data-ttu-id="b2c0a-181">Yeni bir kullanıcı kaydedin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-181">Register a new user.</span></span>
* <span data-ttu-id="b2c0a-182">`/Identity/Account/Manage` sayfasındaki özel kullanıcı verilerini görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-182">View the custom user data on the `/Identity/Account/Manage` page.</span></span>
* <span data-ttu-id="b2c0a-183">Kullanıcıların kişisel verilerini `/Identity/Account/Manage/PersonalData` sayfasından indirip görüntüleyin.</span><span class="sxs-lookup"><span data-stu-id="b2c0a-183">Download and view the users personal data from the `/Identity/Account/Manage/PersonalData` page.</span></span>
