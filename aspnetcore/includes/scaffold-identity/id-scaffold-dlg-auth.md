<span data-ttu-id="d9f0e-101">Kimlik iskele kurucu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d9f0e-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="d9f0e-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d9f0e-102">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="d9f0e-103">Gelen **Çözüm Gezgini**, projeye sağ tıklayın > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="d9f0e-104">Sol bölmesinden **İskele Ekle** iletişim kutusunda **kimlik** > **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="d9f0e-105">İçinde **Ekle kimlik** iletişim kutusunda, kullanmak istediğiniz seçenekleri seçin.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="d9f0e-106">Var olan düzen sayfası seçin veya düzeni dosyanızı olan hatalı biçimlendirme üzerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="d9f0e-107">Var olan bir _Layout.cshtml dosyasını seçildiğinde olduğu **değil** üzerine.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-107">When an existing _Layout.cshtml file is selected, it is **not** overwritten.</span></span>

 <span data-ttu-id="d9f0e-108">Örneğin `~/Pages/Shared/_Layout.cshtml` Razor sayfalarının `~/Views/Shared/_Layout.cshtml` MVC projeleri için</span><span class="sxs-lookup"><span data-stu-id="d9f0e-108">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span> 
* <span data-ttu-id="d9f0e-109">Mevcut veri içeriğiniz kullanmak için geçersiz kılmak için en az bir dosya seçin.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-109">To use your existing data context, select at least one file to override.</span></span> <span data-ttu-id="d9f0e-110">Veri bağlamı eklemek için en az bir dosya seçmelisiniz.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-110">You must select at least one file to add your data context.</span></span> 
  * <span data-ttu-id="d9f0e-111">Veri bağlamı sınıfı seçin.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-111">Select your data context class.</span></span>
  * <span data-ttu-id="d9f0e-112">Seçin **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-112">Select **ADD**.</span></span>
* <span data-ttu-id="d9f0e-113">Yeni kullanıcı bağlamı oluşturmak ve büyük olasılıkla bir özel kullanıcı sınıfı için kimlik oluşturmak için:</span><span class="sxs-lookup"><span data-stu-id="d9f0e-113">To create a new user context and possibly create a custom user class for Identity:</span></span>
  * <span data-ttu-id="d9f0e-114">Seçin **+** yeni düğmesi **veri bağlamı sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-114">Select the **+** button to create a new **Data context class**.</span></span>
  * <span data-ttu-id="d9f0e-115">Seçin **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-115">Select **ADD**.</span></span>
  
<span data-ttu-id="d9f0e-116">Not: yeni bir kullanıcı bağlamı oluşturuyorsanız, geçersiz kılmak için bir dosya seçin gerekmez.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-116">Note: If you're creating a new user context, you don't have to select a file to override.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="d9f0e-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d9f0e-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="d9f0e-118">ASP.NET iskele kurucu daha önce yüklemediyseniz şimdi yükle:</span><span class="sxs-lookup"><span data-stu-id="d9f0e-118">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="d9f0e-119">Paket için bir başvuru ekleyin [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) projeye (\*.csproj) dosyası.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-119">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="d9f0e-120">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d9f0e-120">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet restore
```

<span data-ttu-id="d9f0e-121">Kimlik iskele kurucu seçeneklerinden listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="d9f0e-121">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="d9f0e-122">Proje klasöründe kimlik iskele kurucu istediğiniz seçenekleriyle çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-122">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="d9f0e-123">Örneğin, varsayılan UI kimlikle ve dosyaları minimum sayısını ayarlamak için aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="d9f0e-123">For example, to setup identity with the default UI and the minimum number of files, run the following command.</span></span> <span data-ttu-id="d9f0e-124">DB bağlamı için doğru tam ad kullanın:</span><span class="sxs-lookup"><span data-stu-id="d9f0e-124">Use the correct fully qualified name for your DB context:</span></span>

```cli
dotnet aspnet-codegenerator identity -dc MyWeb.Data.ApplicationDbContext --files Account.Register
```
-------------
