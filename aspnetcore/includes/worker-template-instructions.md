# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="cf369-101">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cf369-101">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="cf369-102">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cf369-102">Create a new project.</span></span>
1. <span data-ttu-id="cf369-103">**Çalışan hizmeti**seçin.</span><span class="sxs-lookup"><span data-stu-id="cf369-103">Select **Worker Service**.</span></span> <span data-ttu-id="cf369-104">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cf369-104">Select **Next**.</span></span>
1. <span data-ttu-id="cf369-105">**Proje adı** alanında bir proje adı girin veya varsayılan proje adını kabul edin.</span><span class="sxs-lookup"><span data-stu-id="cf369-105">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="cf369-106">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="cf369-106">Select **Create**.</span></span>
1. <span data-ttu-id="cf369-107">**Yeni çalışan hizmeti oluştur** Iletişim kutusunda **Oluştur**' u seçin.</span><span class="sxs-lookup"><span data-stu-id="cf369-107">In the **Create a new Worker service** dialog, select **Create**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="cf369-108">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cf369-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="cf369-109">Yeni bir proje oluşturun.</span><span class="sxs-lookup"><span data-stu-id="cf369-109">Create a new project.</span></span>
1. <span data-ttu-id="cf369-110">Kenar çubuğunda **.NET Core** altında **uygulama** ' yı seçin.</span><span class="sxs-lookup"><span data-stu-id="cf369-110">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="cf369-111">**ASP.NET Core**altında **çalışan** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="cf369-111">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="cf369-112">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cf369-112">Select **Next**.</span></span>
1. <span data-ttu-id="cf369-113">**Hedef çerçeve**Için **.NET Core 3,0** ' ı seçin.</span><span class="sxs-lookup"><span data-stu-id="cf369-113">Select **.NET Core 3.0** for the **Target Framework**.</span></span> <span data-ttu-id="cf369-114">**İleri**’yi seçin.</span><span class="sxs-lookup"><span data-stu-id="cf369-114">Select **Next**.</span></span>
1. <span data-ttu-id="cf369-115">**Proje adı** alanına bir ad girin.</span><span class="sxs-lookup"><span data-stu-id="cf369-115">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="cf369-116">**Oluştur**’u seçin.</span><span class="sxs-lookup"><span data-stu-id="cf369-116">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="cf369-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="cf369-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="cf369-118">Çalışan hizmeti (`worker`) şablonunu bir komut kabuğundan [DotNet New](/dotnet/core/tools/dotnet-new) komutuyla birlikte kullanın.</span><span class="sxs-lookup"><span data-stu-id="cf369-118">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="cf369-119">Aşağıdaki örnekte, adlı `ContosoWorker`bir çalışan hizmeti uygulaması oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cf369-119">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="cf369-120">Komut yürütüldüğünde, `ContosoWorker` uygulamanın bir klasörü otomatik olarak oluşturulur.</span><span class="sxs-lookup"><span data-stu-id="cf369-120">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```
