<span data-ttu-id="d8272-101">Kimlik iskele kurucu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d8272-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d8272-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d8272-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="d8272-103">Gelen **Çözüm Gezgini**, projeye sağ tıklayın > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="d8272-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="d8272-104">Sol bölmeden **İskele Ekle** iletişim kutusunda **kimlik** > **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="d8272-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="d8272-105">İçinde **ADD kimliğini** iletişim kutusunda, istediğiniz seçenekleri seçin.</span><span class="sxs-lookup"><span data-stu-id="d8272-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="d8272-106">Varolan bir düzen sayfası seçin veya hatalı biçimlendirme Düzen dosyanızın üzerine yazılacak.</span><span class="sxs-lookup"><span data-stu-id="d8272-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="d8272-107">Varolan _Layout.cshtml dosyasını seçildiğinde olduğu **değil** üzerine.</span><span class="sxs-lookup"><span data-stu-id="d8272-107">When an existing _Layout.cshtml file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="d8272-108">Örneğin `~/Pages/Shared/_Layout.cshtml` Razor sayfaları için `~/Views/Shared/_Layout.cshtml` MVC projeleri</span><span class="sxs-lookup"><span data-stu-id="d8272-108">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
* <span data-ttu-id="d8272-109">Mevcut veri Bağlamınızı kullanmak için geçersiz kılmak için en az bir dosya seçin.</span><span class="sxs-lookup"><span data-stu-id="d8272-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="d8272-110">Veri Bağlamınızı eklemek için en az bir dosya seçmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="d8272-110">You must select at least one file to add your data context.</span></span>
  * <span data-ttu-id="d8272-111">Veri bağlamı sınıfı seçin.</span><span class="sxs-lookup"><span data-stu-id="d8272-111">Select your data context class.</span></span>
  * <span data-ttu-id="d8272-112">Seçin **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="d8272-112">Select **ADD**.</span></span>
* <span data-ttu-id="d8272-113">Yeni bir kullanıcı bağlam oluşturur ve muhtemelen kimlik için bir özel kullanıcı sınıfı oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="d8272-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="d8272-114">Seçin **+** yeni bir düğme **veri bağlamı sınıfının**.</span><span class="sxs-lookup"><span data-stu-id="d8272-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="d8272-115">Seçin **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="d8272-115">Select **ADD**.</span></span>

<span data-ttu-id="d8272-116">Not: yeni bir kullanıcı bağlamı oluşturuyorsanız, geçersiz kılmak için bir dosya seçmek gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d8272-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d8272-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d8272-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d8272-118">ASP.NET iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="d8272-118">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="d8272-119">Paket başvurusu ekleme [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) projesine (\*.csproj) dosyası.</span><span class="sxs-lookup"><span data-stu-id="d8272-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="d8272-120">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d8272-120">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="d8272-121">Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d8272-121">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="d8272-122">Proje klasöründe kimlik iskele kurucu istediğiniz seçeneği ile çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d8272-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="d8272-123">Örneğin, kimliği ' % s'varsayılan kullanıcı Arabirimi ile ve en az sayıda dosya kurmak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d8272-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="d8272-124">DB Bağlamınızı doğru tam adını kullanın:</span><span class="sxs-lookup"><span data-stu-id="d8272-124">Use the correct fully qualified name for your DB context:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```

-------------
