::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d3a48-101">Identity scaffolder öğesini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d3a48-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d3a48-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3a48-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d3a48-103">**Çözüm Gezgini**, projeye sağ tıklayıp **Yeni > Iskli öğe** **ekleyin** >.</span><span class="sxs-lookup"><span data-stu-id="d3a48-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="d3a48-104">**Yapı Iskelesi Ekle** iletişim kutusunun sol bölmesinde **kimlik** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="d3a48-105">**Kimlik Ekle** iletişim kutusunda istediğiniz seçenekleri belirleyin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="d3a48-106">Var olan düzen sayfanızı seçin veya Düzen dosyanızın üzerine yanlış biçimlendirme uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d3a48-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="d3a48-107">Varolan bir *\_Layout. cshtml* dosyası **seçildiğinde, üzerine yazılmaz.**</span><span class="sxs-lookup"><span data-stu-id="d3a48-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="d3a48-108">Örneğin: MVC projeleri için Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`</span><span class="sxs-lookup"><span data-stu-id="d3a48-108">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="d3a48-109">Mevcut veri bağlamınızı kullanmak için, geçersiz kılmak üzere en az bir dosya seçin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="d3a48-110">Veri bağlamınızı eklemek için en az bir dosya seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d3a48-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="d3a48-111">Veri bağlamı sınıfınızı seçin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-111">Select your data context class.</span></span>
  * <span data-ttu-id="d3a48-112">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-112">Select **Add**.</span></span>
* <span data-ttu-id="d3a48-113">Yeni bir kullanıcı bağlamı oluşturmak ve muhtemelen kimlik için özel bir Kullanıcı sınıfı oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="d3a48-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="d3a48-114">Yeni bir **veri bağlamı sınıfı**oluşturmak için **+** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="d3a48-115">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-115">Select **Add**.</span></span>

<span data-ttu-id="d3a48-116">Note: yeni bir kullanıcı bağlamı oluşturuyorsanız, geçersiz kılmak için bir dosya seçmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d3a48-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="d3a48-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d3a48-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d3a48-118">ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="d3a48-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="d3a48-119">Gerekli NuGet paket başvurularını proje (\*. csproj) dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-119">Add required NuGet package references to the project (\*.csproj) file.</span></span> <span data-ttu-id="d3a48-120">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d3a48-120">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="d3a48-121">Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d3a48-121">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="d3a48-122">Proje klasöründe, kimlik desteği ' ı istediğiniz seçeneklerle çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d3a48-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="d3a48-123">Örneğin, kimliği varsayılan UI ve minimum dosya sayısı ile ayarlamak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d3a48-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="d3a48-124">DB içeriğiniz için doğru tam adı kullanın:</span><span class="sxs-lookup"><span data-stu-id="d3a48-124">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="d3a48-125">PowerShell, bir komut ayırıcısı olarak noktalı virgül kullanır.</span><span class="sxs-lookup"><span data-stu-id="d3a48-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="d3a48-126">PowerShell kullanırken, dosya listesinde noktalı virgül karakterini kaçış veya dosya listesini çift tırnak içine koyun.</span><span class="sxs-lookup"><span data-stu-id="d3a48-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="d3a48-127">Örnek:</span><span class="sxs-lookup"><span data-stu-id="d3a48-127">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="d3a48-128">Identity desteği `--files` bayrağını veya `--useDefaultUI` bayrağını belirtmeden çalıştırırsanız, tüm kullanılabilir kimlik Kullanıcı arabirimi sayfaları projenizde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d3a48-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d3a48-129">Identity scaffolder öğesini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d3a48-129">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="d3a48-130">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d3a48-130">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d3a48-131">**Çözüm Gezgini**, projeye sağ tıklayıp **Yeni > Iskli öğe** **ekleyin** >.</span><span class="sxs-lookup"><span data-stu-id="d3a48-131">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="d3a48-132">**Yapı Iskelesi Ekle** iletişim kutusunun sol bölmesinde **kimlik** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-132">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="d3a48-133">**Kimlik Ekle** iletişim kutusunda istediğiniz seçenekleri belirleyin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-133">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="d3a48-134">Var olan düzen sayfanızı seçin veya Düzen dosyanızın üzerine yanlış biçimlendirme uygulanır.</span><span class="sxs-lookup"><span data-stu-id="d3a48-134">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="d3a48-135">Varolan bir *\_Layout. cshtml* dosyası **seçildiğinde, üzerine yazılmaz.**</span><span class="sxs-lookup"><span data-stu-id="d3a48-135">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="d3a48-136">Örneğin: MVC projeleri için Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`</span><span class="sxs-lookup"><span data-stu-id="d3a48-136">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="d3a48-137">Mevcut veri bağlamınızı kullanmak için, geçersiz kılmak üzere en az bir dosya seçin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-137">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="d3a48-138">Veri bağlamınızı eklemek için en az bir dosya seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="d3a48-138">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="d3a48-139">Veri bağlamı sınıfınızı seçin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-139">Select your data context class.</span></span>
  * <span data-ttu-id="d3a48-140">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-140">Select **Add**.</span></span>
* <span data-ttu-id="d3a48-141">Yeni bir kullanıcı bağlamı oluşturmak ve muhtemelen kimlik için özel bir Kullanıcı sınıfı oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="d3a48-141">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="d3a48-142">Yeni bir **veri bağlamı sınıfı**oluşturmak için **+** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-142">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="d3a48-143">**Add (Ekle)** seçeneğini belirleyin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-143">Select **Add**.</span></span>

<span data-ttu-id="d3a48-144">Note: yeni bir kullanıcı bağlamı oluşturuyorsanız, geçersiz kılmak için bir dosya seçmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d3a48-144">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="d3a48-145">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d3a48-145">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d3a48-146">ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="d3a48-146">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="d3a48-147">Proje (\*. csproj) dosyasına [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="d3a48-147">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="d3a48-148">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d3a48-148">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="d3a48-149">Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d3a48-149">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="d3a48-150">Proje klasöründe, kimlik desteği ' ı istediğiniz seçeneklerle çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d3a48-150">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="d3a48-151">Örneğin, kimliği varsayılan UI ve minimum dosya sayısı ile ayarlamak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d3a48-151">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="d3a48-152">DB içeriğiniz için doğru tam adı kullanın:</span><span class="sxs-lookup"><span data-stu-id="d3a48-152">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="d3a48-153">PowerShell, bir komut ayırıcısı olarak noktalı virgül kullanır.</span><span class="sxs-lookup"><span data-stu-id="d3a48-153">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="d3a48-154">PowerShell kullanırken, dosya listesinde noktalı virgül karakterini kaçış veya dosya listesini çift tırnak içine koyun.</span><span class="sxs-lookup"><span data-stu-id="d3a48-154">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="d3a48-155">Örnek:</span><span class="sxs-lookup"><span data-stu-id="d3a48-155">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="d3a48-156">Identity desteği `--files` bayrağını veya `--useDefaultUI` bayrağını belirtmeden çalıştırırsanız, tüm kullanılabilir kimlik Kullanıcı arabirimi sayfaları projenizde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="d3a48-156">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---

::: moniker-end