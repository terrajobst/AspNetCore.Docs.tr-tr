::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="26c7d-101">Identity scaffolder öğesini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="26c7d-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="26c7d-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="26c7d-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="26c7d-103">**Çözüm Gezgini**, projeye sağ tıklayıp **Yeni > Iskli öğe** **ekleyin** >.</span><span class="sxs-lookup"><span data-stu-id="26c7d-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="26c7d-104">**Yapı Iskelesi Ekle** iletişim kutusunun sol bölmesinde **kimlik** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="26c7d-105">**Kimlik Ekle** iletişim kutusunda istediğiniz seçenekleri belirleyin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="26c7d-106">Var olan düzen sayfanızı seçin veya Düzen dosyanızın üzerine yanlış biçimlendirme uygulanır.</span><span class="sxs-lookup"><span data-stu-id="26c7d-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="26c7d-107">Varolan bir *\_Layout. cshtml* dosyası **seçildiğinde, üzerine yazılmaz.**</span><span class="sxs-lookup"><span data-stu-id="26c7d-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="26c7d-108">Örneğin: MVC projeleri için Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`</span><span class="sxs-lookup"><span data-stu-id="26c7d-108">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="26c7d-109">Mevcut veri bağlamınızı kullanmak için, geçersiz kılmak üzere en az bir dosya seçin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="26c7d-110">Veri bağlamınızı eklemek için en az bir dosya seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="26c7d-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="26c7d-111">Veri bağlamı sınıfınızı seçin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-111">Select your data context class.</span></span>
  * <span data-ttu-id="26c7d-112">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-112">Select **Add**.</span></span>
* <span data-ttu-id="26c7d-113">Yeni bir kullanıcı bağlamı oluşturmak ve muhtemelen kimlik için özel bir Kullanıcı sınıfı oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="26c7d-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="26c7d-114">Seçin **+** yeni bir düğme **veri bağlamı sınıfının**.</span><span class="sxs-lookup"><span data-stu-id="26c7d-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="26c7d-115">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-115">Select **Add**.</span></span>

<span data-ttu-id="26c7d-116">Note: yeni bir kullanıcı bağlamı oluşturuyorsanız, geçersiz kılmak için bir dosya seçmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="26c7d-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="26c7d-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="26c7d-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="26c7d-118">ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="26c7d-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="26c7d-119">Gerekli NuGet paket başvurularını proje (\*. csproj) dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-119">Add required NuGet package references to the project (\*.csproj) file.</span></span> <span data-ttu-id="26c7d-120">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="26c7d-120">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="26c7d-121">Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="26c7d-121">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="26c7d-122">Proje klasöründe, kimlik desteği ' ı istediğiniz seçeneklerle çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="26c7d-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="26c7d-123">Örneğin, kimliği varsayılan UI ve minimum dosya sayısı ile ayarlamak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="26c7d-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="26c7d-124">DB içeriğiniz için doğru tam adı kullanın:</span><span class="sxs-lookup"><span data-stu-id="26c7d-124">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="26c7d-125">PowerShell, bir komut ayırıcısı olarak noktalı virgül kullanır.</span><span class="sxs-lookup"><span data-stu-id="26c7d-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="26c7d-126">PowerShell kullanırken, dosya listesinde noktalı virgül karakterini kaçış veya dosya listesini çift tırnak içine koyun.</span><span class="sxs-lookup"><span data-stu-id="26c7d-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="26c7d-127">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="26c7d-127">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="26c7d-128">Identity desteği `--files` bayrağını veya `--useDefaultUI` bayrağını belirtmeden çalıştırırsanız, tüm kullanılabilir kimlik Kullanıcı arabirimi sayfaları projenizde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="26c7d-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="26c7d-129">Identity scaffolder öğesini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="26c7d-129">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="26c7d-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="26c7d-130">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="26c7d-131">**Çözüm Gezgini**, projeye sağ tıklayıp **Yeni > Iskli öğe** **ekleyin** >.</span><span class="sxs-lookup"><span data-stu-id="26c7d-131">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="26c7d-132">**Yapı Iskelesi Ekle** iletişim kutusunun sol bölmesinde **kimlik** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-132">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="26c7d-133">**Kimlik Ekle** iletişim kutusunda istediğiniz seçenekleri belirleyin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-133">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="26c7d-134">Var olan düzen sayfanızı seçin veya Düzen dosyanızın üzerine yanlış biçimlendirme uygulanır.</span><span class="sxs-lookup"><span data-stu-id="26c7d-134">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="26c7d-135">Varolan bir *\_Layout. cshtml* dosyası **seçildiğinde, üzerine yazılmaz.**</span><span class="sxs-lookup"><span data-stu-id="26c7d-135">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="26c7d-136">Örneğin: MVC projeleri için Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`</span><span class="sxs-lookup"><span data-stu-id="26c7d-136">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="26c7d-137">Mevcut veri bağlamınızı kullanmak için, geçersiz kılmak üzere en az bir dosya seçin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-137">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="26c7d-138">Veri bağlamınızı eklemek için en az bir dosya seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="26c7d-138">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="26c7d-139">Veri bağlamı sınıfınızı seçin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-139">Select your data context class.</span></span>
  * <span data-ttu-id="26c7d-140">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-140">Select **Add**.</span></span>
* <span data-ttu-id="26c7d-141">Yeni bir kullanıcı bağlamı oluşturmak ve muhtemelen kimlik için özel bir Kullanıcı sınıfı oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="26c7d-141">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="26c7d-142">Seçin **+** yeni bir düğme **veri bağlamı sınıfının**.</span><span class="sxs-lookup"><span data-stu-id="26c7d-142">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="26c7d-143">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-143">Select **Add**.</span></span>

<span data-ttu-id="26c7d-144">Note: yeni bir kullanıcı bağlamı oluşturuyorsanız, geçersiz kılmak için bir dosya seçmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="26c7d-144">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="26c7d-145">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="26c7d-145">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="26c7d-146">ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="26c7d-146">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="26c7d-147">Proje (\*. csproj) dosyasına [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="26c7d-147">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="26c7d-148">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="26c7d-148">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="26c7d-149">Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="26c7d-149">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="26c7d-150">Proje klasöründe, kimlik desteği ' ı istediğiniz seçeneklerle çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="26c7d-150">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="26c7d-151">Örneğin, kimliği varsayılan UI ve minimum dosya sayısı ile ayarlamak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="26c7d-151">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="26c7d-152">DB içeriğiniz için doğru tam adı kullanın:</span><span class="sxs-lookup"><span data-stu-id="26c7d-152">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="26c7d-153">PowerShell, bir komut ayırıcısı olarak noktalı virgül kullanır.</span><span class="sxs-lookup"><span data-stu-id="26c7d-153">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="26c7d-154">PowerShell kullanırken, dosya listesinde noktalı virgül karakterini kaçış veya dosya listesini çift tırnak içine koyun.</span><span class="sxs-lookup"><span data-stu-id="26c7d-154">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="26c7d-155">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="26c7d-155">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="26c7d-156">Identity desteği `--files` bayrağını veya `--useDefaultUI` bayrağını belirtmeden çalıştırırsanız, tüm kullanılabilir kimlik Kullanıcı arabirimi sayfaları projenizde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="26c7d-156">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end