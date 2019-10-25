<span data-ttu-id="e73e5-101">Identity scaffolder öğesini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e73e5-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e73e5-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e73e5-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e73e5-103">**Çözüm Gezgini**, projeye sağ tıklayıp **Yeni > Iskli öğe** **ekleyin** >.</span><span class="sxs-lookup"><span data-stu-id="e73e5-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="e73e5-104">**Yapı Iskelesi Ekle** iletişim kutusunun sol bölmesinde **kimlik** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="e73e5-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **Add**.</span></span>
* <span data-ttu-id="e73e5-105">**Kimlik Ekle** iletişim kutusunda istediğiniz seçenekleri belirleyin.</span><span class="sxs-lookup"><span data-stu-id="e73e5-105">In the **Add Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="e73e5-106">Var olan düzen sayfanızı seçin veya Düzen dosyanızın üzerine yanlış biçimlendirme uygulanır.</span><span class="sxs-lookup"><span data-stu-id="e73e5-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="e73e5-107">Varolan bir *\_Layout. cshtml* dosyası **seçildiğinde, üzerine yazılmaz.**</span><span class="sxs-lookup"><span data-stu-id="e73e5-107">When an existing *\_Layout.cshtml* file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="e73e5-108">Örneğin: MVC projeleri için Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`</span><span class="sxs-lookup"><span data-stu-id="e73e5-108">For example: `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="e73e5-109">Mevcut veri bağlamınızı kullanmak için, geçersiz kılmak üzere en az bir dosya seçin.</span><span class="sxs-lookup"><span data-stu-id="e73e5-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="e73e5-110">Veri bağlamınızı eklemek için en az bir dosya seçmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="e73e5-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="e73e5-111">Veri bağlamı sınıfınızı seçin.</span><span class="sxs-lookup"><span data-stu-id="e73e5-111">Select your data context class.</span></span>
  * <span data-ttu-id="e73e5-112">**Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="e73e5-112">Select **Add**.</span></span>
* <span data-ttu-id="e73e5-113">Yeni bir kullanıcı bağlamı oluşturmak ve muhtemelen kimlik için özel bir Kullanıcı sınıfı oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="e73e5-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="e73e5-114">Yeni bir **veri bağlamı sınıfı**oluşturmak için **+** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="e73e5-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="e73e5-115">**Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="e73e5-115">Select **Add**.</span></span>

<span data-ttu-id="e73e5-116">Note: yeni bir kullanıcı bağlamı oluşturuyorsanız, geçersiz kılmak için bir dosya seçmeniz gerekmez.</span><span class="sxs-lookup"><span data-stu-id="e73e5-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e73e5-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="e73e5-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="e73e5-118">ASP.NET Core scaffolder ' ı daha önce yüklemediyseniz, şimdi yükleyebilirsiniz:</span><span class="sxs-lookup"><span data-stu-id="e73e5-118">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="e73e5-119">Proje (\*. csproj) dosyasına [Microsoft. VisualStudio. Web. CodeGeneration. Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) öğesine bir paket başvurusu ekleyin.</span><span class="sxs-lookup"><span data-stu-id="e73e5-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="e73e5-120">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e73e5-120">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="e73e5-121">Identity desteği seçeneklerini listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="e73e5-121">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="e73e5-122">Proje klasöründe, kimlik desteği ' ı istediğiniz seçeneklerle çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e73e5-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="e73e5-123">Örneğin, kimliği varsayılan UI ve minimum dosya sayısı ile ayarlamak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e73e5-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="e73e5-124">DB içeriğiniz için doğru tam adı kullanın:</span><span class="sxs-lookup"><span data-stu-id="e73e5-124">Use the correct fully qualified name for your DB context:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login"
```

<span data-ttu-id="e73e5-125">PowerShell, bir komut ayırıcısı olarak noktalı virgül kullanır.</span><span class="sxs-lookup"><span data-stu-id="e73e5-125">PowerShell uses semicolon as a command separator.</span></span> <span data-ttu-id="e73e5-126">PowerShell kullanırken, dosya listesinde noktalı virgül karakterini kaçış veya dosya listesini çift tırnak içine koyun.</span><span class="sxs-lookup"><span data-stu-id="e73e5-126">When using PowerShell, escape the semi-colons in the file list or put the file list in double quotes.</span></span> <span data-ttu-id="e73e5-127">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="e73e5-127">For example:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files "Account.Register;Account.Login;Account.Logout"
```

<span data-ttu-id="e73e5-128">Identity desteği `--files` bayrağını veya `--useDefaultUI` bayrağını belirtmeden çalıştırırsanız, tüm kullanılabilir kimlik Kullanıcı arabirimi sayfaları projenizde oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="e73e5-128">If you run the Identity scaffolder without specifying the `--files` flag or the `--useDefaultUI` flag, all the available Identity UI pages will be created in your project.</span></span>

---
