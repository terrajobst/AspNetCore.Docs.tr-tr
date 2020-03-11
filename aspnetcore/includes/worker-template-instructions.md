# <a name="visual-studio"></a>[<span data-ttu-id="cb32e-101">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cb32e-101">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="cb32e-102">Yeni bir proje oluşturma.</span><span class="sxs-lookup"><span data-stu-id="cb32e-102">Create a new project.</span></span>
1. <span data-ttu-id="cb32e-103">**Çalışan hizmeti**seçin.</span><span class="sxs-lookup"><span data-stu-id="cb32e-103">Select **Worker Service**.</span></span> <span data-ttu-id="cb32e-104">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cb32e-104">Select **Next**.</span></span>
1. <span data-ttu-id="cb32e-105">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="cb32e-105">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="cb32e-106">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="cb32e-106">Select **Create**.</span></span>
1. <span data-ttu-id="cb32e-107">**Yeni çalışan hizmeti oluştur** Iletişim kutusunda **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="cb32e-107">In the **Create a new Worker service** dialog, select **Create**.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="cb32e-108">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cb32e-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="cb32e-109">Yeni bir proje oluşturma.</span><span class="sxs-lookup"><span data-stu-id="cb32e-109">Create a new project.</span></span>
1. <span data-ttu-id="cb32e-110">Kenar çubuğunda **.NET Core** altında **uygulama** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="cb32e-110">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="cb32e-111">**ASP.NET Core**altında **çalışan** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="cb32e-111">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="cb32e-112">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cb32e-112">Select **Next**.</span></span>
1. <span data-ttu-id="cb32e-113">**Hedef çerçeve**Için **.NET Core 3,0** veya üzerini seçin.</span><span class="sxs-lookup"><span data-stu-id="cb32e-113">Select **.NET Core 3.0** or later for the **Target Framework**.</span></span> <span data-ttu-id="cb32e-114">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cb32e-114">Select **Next**.</span></span>
1. <span data-ttu-id="cb32e-115">**Proje adı** alanına bir ad girin.</span><span class="sxs-lookup"><span data-stu-id="cb32e-115">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="cb32e-116">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="cb32e-116">Select **Create**.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="cb32e-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="cb32e-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="cb32e-118">Bir komut kabuğundan [DotNet New](/dotnet/core/tools/dotnet-new) komutuyla çalışan hizmeti (`worker`) şablonunu kullanın.</span><span class="sxs-lookup"><span data-stu-id="cb32e-118">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="cb32e-119">Aşağıdaki örnekte, `ContosoWorker`adlı bir çalışan hizmeti uygulaması oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cb32e-119">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="cb32e-120">Komut yürütüldüğünde `ContosoWorker` uygulamasının bir klasörü otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cb32e-120">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---
