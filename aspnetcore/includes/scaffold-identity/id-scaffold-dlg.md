<span data-ttu-id="a38e3-101">Kimlik iskele kurucu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a38e3-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a38e3-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a38e3-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="a38e3-103">Gelen **Çözüm Gezgini**, projeye sağ tıklayın > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="a38e3-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="a38e3-104">Sol bölmeden **İskele Ekle** iletişim kutusunda **kimlik** > **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="a38e3-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="a38e3-105">İçinde **ADD kimliğini** iletişim kutusunda, istediğiniz seçenekleri seçin.</span><span class="sxs-lookup"><span data-stu-id="a38e3-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="a38e3-106">Varolan bir düzen sayfası seçin veya hatalı biçimlendirme Düzen dosyanızın üzerine yazılacak.</span><span class="sxs-lookup"><span data-stu-id="a38e3-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="a38e3-107">Örneğin `~/Pages/Shared/_Layout.cshtml` Razor sayfaları için `~/Views/Shared/_Layout.cshtml` MVC projeleri</span><span class="sxs-lookup"><span data-stu-id="a38e3-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="a38e3-108">Seçin **+** yeni bir düğme **veri bağlamı sınıfının**.</span><span class="sxs-lookup"><span data-stu-id="a38e3-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="a38e3-109">Seçin **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="a38e3-109">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a38e3-110">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="a38e3-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a38e3-111">ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="a38e3-111">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="a38e3-112">Paket başvurusu ekleme [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) projesine (\*.csproj) dosyası.</span><span class="sxs-lookup"><span data-stu-id="a38e3-112">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="a38e3-113">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a38e3-113">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet restore
```

<span data-ttu-id="a38e3-114">Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a38e3-114">Run the following command to list the Identity scaffolder options:</span></span>

```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="a38e3-115">Proje klasöründe kimlik iskele kurucu istediğiniz seçeneği ile çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="a38e3-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="a38e3-116">Örneğin, varsayılan UI kimliği ve en az sayıda dosya Kurulumu için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="a38e3-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```cli
dotnet aspnet-codegenerator identity --useDefaultUI
```

-------------
