<span data-ttu-id="6d551-101">Kimlik iskele kurucu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6d551-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="6d551-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d551-102">Visual Studio</span></span>](#tab/visual-studio) 

* <span data-ttu-id="6d551-103">Gelen **Çözüm Gezgini**, projeye sağ tıklayın > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="6d551-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="6d551-104">Sol bölmesinden **İskele Ekle** iletişim kutusunda **kimlik** > **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="6d551-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="6d551-105">İçinde **Ekle kimlik** iletişim kutusunda, kullanmak istediğiniz seçenekleri seçin.</span><span class="sxs-lookup"><span data-stu-id="6d551-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="6d551-106">Var olan düzen sayfası seçin veya düzeni dosyanızı olan hatalı biçimlendirme üzerine yazılır.</span><span class="sxs-lookup"><span data-stu-id="6d551-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="6d551-107">Örneğin `~/Pages/Shared/_Layout.cshtml` Razor sayfalarının `~/Views/Shared/_Layout.cshtml` MVC projeleri için</span><span class="sxs-lookup"><span data-stu-id="6d551-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span> 
  * <span data-ttu-id="6d551-108">Seçin **+** yeni düğmesi **veri bağlamı sınıfı**.</span><span class="sxs-lookup"><span data-stu-id="6d551-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="6d551-109">Seçin **eklemek**.</span><span class="sxs-lookup"><span data-stu-id="6d551-109">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="6d551-110">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="6d551-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="6d551-111">ASP.NET iskele kurucu daha önce yüklemediyseniz şimdi yükle:</span><span class="sxs-lookup"><span data-stu-id="6d551-111">If you have not previously installed the ASP.NET scaffolder, install it now:</span></span>

```cli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="6d551-112">Paket için bir başvuru ekleyin [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) projeye (\*.csproj) dosyası.</span><span class="sxs-lookup"><span data-stu-id="6d551-112">Add a package reference to [Microsoft.VisualStudio.Web.CodeGeneration.Design](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.CodeGeneration.Design/) to the project (\*.csproj) file.</span></span> <span data-ttu-id="6d551-113">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6d551-113">Run the following command in the project directory:</span></span>

```cli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet restore
```

<span data-ttu-id="6d551-114">Kimlik iskele kurucu seçeneklerinden listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6d551-114">Run the following command to list the Identity scaffolder options:</span></span>


```cli
dotnet aspnet-codegenerator identity -h
```

<span data-ttu-id="6d551-115">Proje klasöründe kimlik iskele kurucu istediğiniz seçenekleriyle çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="6d551-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="6d551-116">Örneğin, varsayılan UI kimlikle ve dosyaları az sayıda kurulumu için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="6d551-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```cli
dotnet aspnet-codegenerator identity --useDefaultUI
```
-------------