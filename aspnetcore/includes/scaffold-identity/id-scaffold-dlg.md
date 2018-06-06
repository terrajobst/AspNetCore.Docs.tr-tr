<span data-ttu-id="6f41d-101">Kimlik iskele kurucu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6f41d-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6f41d-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6f41d-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6f41d-103">Gelen **Çözüm Gezgini**, projeye sağ tıklayın > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="6f41d-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="6f41d-104">Sol bölmesinden **İskele Ekle** iletişim kutusunda **kimlik** > **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="6f41d-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="6f41d-105">İçinde **Ekle kimlik** iletişim kutusunda, kullanmak istediğiniz seçenekleri seçin.</span><span class="sxs-lookup"><span data-stu-id="6f41d-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="6f41d-106">Var olan düzen sayfası seçin veya düzeni dosyanızı olan hatalı biçimlendirme üzerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="6f41d-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="6f41d-107">Örneğin `~/Pages/Shared/_Layout.cshtml` Razor sayfalarının `~/Views/Shared/_Layout.cshtml` MVC projeleri için</span><span class="sxs-lookup"><span data-stu-id="6f41d-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="6f41d-108">Seçin **+** yeni düğmesi **veri bağlamı sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="6f41d-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="6f41d-109">Seçin **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="6f41d-109">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6f41d-110">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6f41d-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6f41d-111">ASP.NET iskele kurucu daha önce yüklemediyseniz şimdi yükle:</span><span class="sxs-lookup"><span data-stu-id="6f41d-111">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="6f41d-112">Paket için bir başvuru ekleyin [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) projeye (\*.csproj) dosyası.</span><span class="sxs-lookup"><span data-stu-id="6f41d-112">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="6f41d-113">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6f41d-113">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="6f41d-114">Kimlik iskele kurucu seçeneklerinden listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6f41d-114">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="6f41d-115">Proje klasöründe kimlik iskele kurucu istediğiniz seçenekleriyle çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6f41d-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="6f41d-116">Örneğin, varsayılan UI kimlikle ve dosyaları az sayıda kurulumu için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6f41d-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```cli
dotnet aspnet-codegenerator identity --useDefaultUI
```

-------------