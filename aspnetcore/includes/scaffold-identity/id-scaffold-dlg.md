<span data-ttu-id="c4a15-101">Identity scaffolder öğesini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c4a15-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c4a15-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c4a15-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="c4a15-103">Gelen **Çözüm Gezgini**, projeye sağ tıklayın > **Ekle** > **yeni iskele kurulmuş öğe**.</span><span class="sxs-lookup"><span data-stu-id="c4a15-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="c4a15-104">Sol bölmeden **İskele Ekle** iletişim kutusunda **kimlik** > **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="c4a15-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="c4a15-105">**Kimlik Ekle** iletişim kutusunda istediğiniz seçenekleri belirleyin.</span><span class="sxs-lookup"><span data-stu-id="c4a15-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="c4a15-106">Var olan düzen sayfanızı seçin veya Düzen dosyanızın üzerine yanlış biçimlendirme uygulanır.</span><span class="sxs-lookup"><span data-stu-id="c4a15-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="c4a15-107">Örneğin, MVC projeleri için Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`</span><span class="sxs-lookup"><span data-stu-id="c4a15-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="c4a15-108">Seçin **+** yeni bir düğme **veri bağlamı sınıfının**.</span><span class="sxs-lookup"><span data-stu-id="c4a15-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="c4a15-109">Seçin **ekleme**.</span><span class="sxs-lookup"><span data-stu-id="c4a15-109">Select **ADD**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c4a15-110">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="c4a15-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="c4a15-111">ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="c4a15-111">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="c4a15-112">Gerekli NuGet paket başvurularını proje (\*. csproj) dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="c4a15-112">Add required NuGet package references to the project (\*.csproj) file.</span></span> <span data-ttu-id="c4a15-113">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c4a15-113">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="c4a15-114">Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c4a15-114">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="c4a15-115">Proje klasöründe, kimlik desteği ' ı istediğiniz seçeneklerle çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="c4a15-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="c4a15-116">Örneğin, kimliği varsayılan UI ve minimum dosya sayısı ile ayarlamak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="c4a15-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
