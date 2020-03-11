<span data-ttu-id="74aaa-101">Identity scaffolder öğesini çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="74aaa-101">Run the Identity scaffolder:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="74aaa-102">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="74aaa-102">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="74aaa-103">**Çözüm Gezgini**, projeye sağ tıklayıp **Yeni > Iskli öğe** **ekleyin** >.</span><span class="sxs-lookup"><span data-stu-id="74aaa-103">From **Solution Explorer**, right-click on the project > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="74aaa-104">**Yapı Iskelesi Ekle** iletişim kutusunun sol bölmesinde **kimlik** > **Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="74aaa-104">From the left pane of the **Add Scaffold** dialog, select **Identity** > **ADD**.</span></span>
* <span data-ttu-id="74aaa-105">**Kimlik Ekle** iletişim kutusunda istediğiniz seçenekleri belirleyin.</span><span class="sxs-lookup"><span data-stu-id="74aaa-105">In the **ADD Identity** dialog, select the options you want.</span></span>
  * <span data-ttu-id="74aaa-106">Var olan düzen sayfanızı seçin veya Düzen dosyanızın üzerine yanlış biçimlendirme uygulanır.</span><span class="sxs-lookup"><span data-stu-id="74aaa-106">Select your existing layout page, or your layout file will be overwritten with incorrect markup.</span></span> <span data-ttu-id="74aaa-107">Örneğin, MVC projeleri için Razor Pages `~/Views/Shared/_Layout.cshtml` `~/Pages/Shared/_Layout.cshtml`</span><span class="sxs-lookup"><span data-stu-id="74aaa-107">For example `~/Pages/Shared/_Layout.cshtml` for Razor Pages `~/Views/Shared/_Layout.cshtml` for MVC projects</span></span>
  * <span data-ttu-id="74aaa-108">Yeni bir **veri bağlamı sınıfı**oluşturmak için **+** düğmesini seçin.</span><span class="sxs-lookup"><span data-stu-id="74aaa-108">Select the **+** button to create a new **Data context class**.</span></span>
* <span data-ttu-id="74aaa-109">**Ekle**' yi seçin.</span><span class="sxs-lookup"><span data-stu-id="74aaa-109">Select **ADD**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="74aaa-110">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="74aaa-110">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="74aaa-111">ASP.NET Core iskele kurucu daha önce yüklemediyseniz şimdi yükleyin:</span><span class="sxs-lookup"><span data-stu-id="74aaa-111">If you have not previously installed the ASP.NET Core scaffolder, install it now:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="74aaa-112">Gerekli NuGet paket başvurularını proje (\*. csproj) dosyasına ekleyin.</span><span class="sxs-lookup"><span data-stu-id="74aaa-112">Add required NuGet package references to the project (\*.csproj) file.</span></span> <span data-ttu-id="74aaa-113">Proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="74aaa-113">Run the following command in the project directory:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.AspNetCore.Identity.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Identity.UI
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="74aaa-114">Kimlik destek seçeneklerini listelemek için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="74aaa-114">Run the following command to list the Identity scaffolder options:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity -h
```

[!INCLUDE[](~/includes/scaffoldTFM.md)]

<span data-ttu-id="74aaa-115">Proje klasöründe, kimlik desteği ' ı istediğiniz seçeneklerle çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="74aaa-115">In the project folder, run the Identity scaffolder with the options you want.</span></span> <span data-ttu-id="74aaa-116">Örneğin, kimliği varsayılan UI ve minimum dosya sayısı ile ayarlamak için aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="74aaa-116">For example, to setup identity with the default UI and the minimum number of files, run the following command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator identity --useDefaultUI
```

---
