---
title: ASP.NET Core sürümünde geliştirme sırasında uygulama gizli dizileri güvenli depolama
author: rick-anderson
description: ASP.NET Core uygulamasının geliştirilmesi sırasında gizli bilgileri uygulama gizli dizileri olarak depolamayı ve almayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 0203a5737caf1af809b739d9e266a6971cd1523b
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080711"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="abe07-103">ASP.NET Core sürümünde geliştirme sırasında uygulama gizli dizileri güvenli depolama</span><span class="sxs-lookup"><span data-stu-id="abe07-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="abe07-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27)ve [Scott Ade](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="abe07-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="abe07-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="abe07-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="abe07-106">Bu belgede, bir ASP.NET Core uygulamasının geliştirilmesi sırasında hassas verilerin depolanması ve alınması için teknikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="abe07-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="abe07-107">Kaynak kodunda parolaları veya diğer hassas verileri hiçbir şekilde depolamayin.</span><span class="sxs-lookup"><span data-stu-id="abe07-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="abe07-108">Üretim gizli dizileri geliştirme veya test için kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="abe07-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="abe07-109">[Azure Key Vault yapılandırma sağlayıcısıyla](xref:security/key-vault-configuration)Azure test ve üretim gizli dizilerini saklayabilir ve koruyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abe07-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="abe07-110">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="abe07-110">Environment variables</span></span>

<span data-ttu-id="abe07-111">Ortam değişkenleri, kodda veya yerel yapılandırma dosyalarında uygulama gizli dizileri depolanmasını önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="abe07-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="abe07-112">Ortam değişkenleri, daha önce belirtilen tüm yapılandırma kaynakları için yapılandırma değerlerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="abe07-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="abe07-113">Oluşturucuyu`Startup` çağırarak <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> ortam değişkeni değerlerinin okunmasını yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="abe07-113">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="abe07-114">**Bireysel kullanıcı hesapları** güvenliğinin etkinleştirildiği bir ASP.NET Core Web uygulaması düşünün.</span><span class="sxs-lookup"><span data-stu-id="abe07-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="abe07-115">Varsayılan bir veritabanı bağlantı dizesi, anahtarına `DefaultConnection`sahip projenin *appSettings. JSON* dosyasına dahildir.</span><span class="sxs-lookup"><span data-stu-id="abe07-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="abe07-116">Varsayılan bağlantı dizesi, kullanıcı modunda çalışan ve parola gerektirmeyen LocalDB içindir.</span><span class="sxs-lookup"><span data-stu-id="abe07-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="abe07-117">Uygulama dağıtımı sırasında, `DefaultConnection` anahtar değeri bir ortam değişkeninin değeri ile geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="abe07-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="abe07-118">Ortam değişkeni, tüm bağlantı dizesini hassas kimlik bilgileriyle saklayabilir.</span><span class="sxs-lookup"><span data-stu-id="abe07-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="abe07-119">Ortam değişkenleri genellikle düz, şifresiz metin olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="abe07-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="abe07-120">Makinenin veya işlemin güvenliği tehlikeye atılırsa, ortam değişkenlerine güvenilmeyen taraflar tarafından erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="abe07-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="abe07-121">Kullanıcı gizliliklerinin açıklanmasını önlemeye yönelik ek ölçüler gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="abe07-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="abe07-122">Gizli dizi Yöneticisi</span><span class="sxs-lookup"><span data-stu-id="abe07-122">Secret Manager</span></span>

<span data-ttu-id="abe07-123">Gizli verileri bir ASP.NET Core projesi geliştirme sırasında depolar.</span><span class="sxs-lookup"><span data-stu-id="abe07-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="abe07-124">Bu bağlamda, önemli bir veri parçası bir uygulama gizli dizisi.</span><span class="sxs-lookup"><span data-stu-id="abe07-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="abe07-125">Uygulama gizli dizileri, proje ağacından ayrı bir konumda depolanır.</span><span class="sxs-lookup"><span data-stu-id="abe07-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="abe07-126">Uygulama gizli dizileri belirli bir projeyle ilişkilendirilir veya çeşitli projeler arasında paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="abe07-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="abe07-127">Uygulama gizli dizileri kaynak denetimine iade edilmedi.</span><span class="sxs-lookup"><span data-stu-id="abe07-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="abe07-128">Gizli dizi Yöneticisi aracı depolanan gizli dizileri şifrelemez ve güvenilir bir depo olarak değerlendirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="abe07-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="abe07-129">Yalnızca geliştirme amaçlıdır.</span><span class="sxs-lookup"><span data-stu-id="abe07-129">It's for development purposes only.</span></span> <span data-ttu-id="abe07-130">Anahtarlar ve değerler, Kullanıcı profili dizinindeki bir JSON yapılandırma dosyasında depolanır.</span><span class="sxs-lookup"><span data-stu-id="abe07-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="abe07-131">Gizli dizi Yöneticisi aracı nasıl kullanılır?</span><span class="sxs-lookup"><span data-stu-id="abe07-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="abe07-132">Gizli dizi Yöneticisi Aracı, değerlerin nerede ve nasıl depolandığı gibi uygulama ayrıntılarını soyutlar.</span><span class="sxs-lookup"><span data-stu-id="abe07-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="abe07-133">Bu uygulama ayrıntılarını bilmeden aracı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="abe07-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="abe07-134">Değerler, yerel makinedeki sistem korumalı bir kullanıcı profili klasöründe bir JSON yapılandırma dosyasında depolanır:</span><span class="sxs-lookup"><span data-stu-id="abe07-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="abe07-135">Windows</span><span class="sxs-lookup"><span data-stu-id="abe07-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="abe07-136">Dosya sistemi yolu:</span><span class="sxs-lookup"><span data-stu-id="abe07-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="abe07-137">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="abe07-137">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="abe07-138">Dosya sistemi yolu:</span><span class="sxs-lookup"><span data-stu-id="abe07-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="abe07-139">Yukarıdaki dosya yollarında, *. csproj* dosyasında `<user_secrets_id>` belirtilen `UserSecretsId` değerle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="abe07-139">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="abe07-140">Gizli dizi yöneticisi aracıyla kaydedilen verilerin konumuna veya biçimine bağlı olarak kod yazma.</span><span class="sxs-lookup"><span data-stu-id="abe07-140">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="abe07-141">Bu uygulama ayrıntıları değişebilir.</span><span class="sxs-lookup"><span data-stu-id="abe07-141">These implementation details may change.</span></span> <span data-ttu-id="abe07-142">Örneğin, gizli değerler şifrelenmez, ancak gelecekte olabilir.</span><span class="sxs-lookup"><span data-stu-id="abe07-142">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="abe07-143">Gizli dizi Yöneticisi aracını yükler</span><span class="sxs-lookup"><span data-stu-id="abe07-143">Install the Secret Manager tool</span></span>

<span data-ttu-id="abe07-144">Gizli dizi Yöneticisi Aracı, .NET Core SDK 2.1.300 veya sonraki sürümlerde .NET Core CLI paketlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="abe07-144">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="abe07-145">2\.1.300 önceki .NET Core SDK sürümler için araç yüklemesi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="abe07-145">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="abe07-146">Yüklü `dotnet --version` .NET Core SDK sürüm numarasını görmek için bir komut kabuğu 'ndan çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="abe07-146">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="abe07-147">Kullanılan .NET Core SDK araç içeriyorsa bir uyarı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="abe07-147">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="abe07-148">ASP.NET Core projenize [Microsoft. Extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet paketini yükle.</span><span class="sxs-lookup"><span data-stu-id="abe07-148">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="abe07-149">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="abe07-149">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="abe07-150">Araç yüklemesini doğrulamak için bir komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="abe07-150">Execute the following command in a command shell to validate the tool installation:</span></span>

```dotnetcli
dotnet user-secrets -h
```

<span data-ttu-id="abe07-151">Gizli dizi Yöneticisi Aracı, örnek kullanım, Seçenekler ve komut yardımını görüntüler:</span><span class="sxs-lookup"><span data-stu-id="abe07-151">The Secret Manager tool displays sample usage, options, and command help:</span></span>

```console
Usage: dotnet user-secrets [options] [command]

Options:
  -?|-h|--help                        Show help information
  --version                           Show version information
  -v|--verbose                        Show verbose output
  -p|--project <PROJECT>              Path to project. Defaults to searching the current directory.
  -c|--configuration <CONFIGURATION>  The project configuration to use. Defaults to 'Debug'.
  --id                                The user secret ID to use.

Commands:
  clear   Deletes all the application secrets
  list    Lists all the application secrets
  remove  Removes the specified user secret
  set     Sets the user secret to the specified value

Use "dotnet user-secrets [command] --help" for more information about a command.
```

> [!NOTE]
> <span data-ttu-id="abe07-152">*. Csproj* dosyasının `DotNetCliToolReference` öğelerinde tanımlanan araçları çalıştırmak için *. csproj* dosyasıyla aynı dizinde olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="abe07-152">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="abe07-153">Gizli depolamayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="abe07-153">Enable secret storage</span></span>

<span data-ttu-id="abe07-154">Gizli dizi Yöneticisi Aracı, Kullanıcı profilinizde depolanan projeye özgü yapılandırma ayarları üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="abe07-154">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="abe07-155">Gizli dizi Yöneticisi Aracı, .NET Core SDK `init` 3.0.100 veya sonraki sürümlerde bir komut içerir.</span><span class="sxs-lookup"><span data-stu-id="abe07-155">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="abe07-156">Kullanıcı gizli dizilerini kullanmak için, proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="abe07-156">To use user secrets, run the following command in the project directory:</span></span>

```dotnetcli
dotnet user-secrets init
```

<span data-ttu-id="abe07-157">Önceki komut, `UserSecretsId` *. csproj* dosyasının içindeki bir `PropertyGroup` öğesi ekler.</span><span class="sxs-lookup"><span data-stu-id="abe07-157">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="abe07-158">Varsayılan olarak, iç metni `UserSecretsId` bir GUID 'dir.</span><span class="sxs-lookup"><span data-stu-id="abe07-158">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="abe07-159">İç metin rastgele, ancak proje için benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="abe07-159">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="abe07-160">Kullanıcı gizli dizilerini kullanmak için `UserSecretsId` *. csproj* dosyasının `PropertyGroup` içindeki bir öğesi tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="abe07-160">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="abe07-161">İç metni `UserSecretsId` rastgele, ancak proje için benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="abe07-161">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="abe07-162">Geliştiriciler, `UserSecretsId`genellıkle için bir GUID oluşturur.</span><span class="sxs-lookup"><span data-stu-id="abe07-162">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="abe07-163">Visual Studio 'da Çözüm Gezgini projeye sağ tıklayın ve bağlam menüsünden **Kullanıcı gizli dizilerini Yönet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="abe07-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="abe07-164">Bu hareket, `UserSecretsId` *. csproj* dosyasına bir GUID ile doldurulmuş bir öğe ekler.</span><span class="sxs-lookup"><span data-stu-id="abe07-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="abe07-165">Gizli dizi ayarla</span><span class="sxs-lookup"><span data-stu-id="abe07-165">Set a secret</span></span>

<span data-ttu-id="abe07-166">Anahtar ve değerini içeren bir uygulama gizli anahtarı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="abe07-166">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="abe07-167">Gizli dizi, projenin `UserSecretsId` değeri ile ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="abe07-167">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="abe07-168">Örneğin, *. csproj* dosyasının bulunduğu dizinden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="abe07-168">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="abe07-169">Yukarıdaki örnekte, iki nokta üst üste bir `Movies` `ServiceApiKey` özelliği olan bir nesne sabit değeri olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="abe07-169">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="abe07-170">Gizli dizi Yöneticisi Aracı diğer dizinlerden de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="abe07-170">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="abe07-171">*. Csproj* dosyasının bulunduğu dosya sistemi yolunu sağlamak için seçeneğinikullanın.`--project`</span><span class="sxs-lookup"><span data-stu-id="abe07-171">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="abe07-172">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="abe07-172">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="abe07-173">Visual Studio 'da JSON yapısı düzleştirme</span><span class="sxs-lookup"><span data-stu-id="abe07-173">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="abe07-174">Visual Studio 'nun **Kullanıcı gizli dizilerini Yönet** hareketi metin düzenleyicisinde bir *gizli dizi. JSON* dosyası açar.</span><span class="sxs-lookup"><span data-stu-id="abe07-174">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="abe07-175">*Gizlilikler. JSON* içeriğini depolanacak anahtar-değer çiftleriyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="abe07-175">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="abe07-176">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="abe07-176">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="abe07-177">JSON yapısı, veya `dotnet user-secrets remove` `dotnet user-secrets set`ile yapılan değişikliklerden sonra düzleştirilir.</span><span class="sxs-lookup"><span data-stu-id="abe07-177">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="abe07-178">Örneğin, çalışıyor `dotnet user-secrets remove "Movies:ConnectionString"` `Movies` nesne değişmez değerini daraltır.</span><span class="sxs-lookup"><span data-stu-id="abe07-178">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="abe07-179">Değiştirilen dosya şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="abe07-179">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="abe07-180">Birden çok gizli dizi ayarla</span><span class="sxs-lookup"><span data-stu-id="abe07-180">Set multiple secrets</span></span>

<span data-ttu-id="abe07-181">Bir dizi gizli dizi, `set` JSON ile komutuna ayırarak ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="abe07-181">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="abe07-182">Aşağıdaki örnekte, *input. JSON* dosyasının içeriği `set` komutuna yöneldir.</span><span class="sxs-lookup"><span data-stu-id="abe07-182">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="abe07-183">Windows</span><span class="sxs-lookup"><span data-stu-id="abe07-183">Windows</span></span>](#tab/windows)

<span data-ttu-id="abe07-184">Bir komut kabuğu açın ve şu komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="abe07-184">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="abe07-185">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="abe07-185">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="abe07-186">Bir komut kabuğu açın ve şu komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="abe07-186">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="abe07-187">Gizli dizi erişimi</span><span class="sxs-lookup"><span data-stu-id="abe07-187">Access a secret</span></span>

<span data-ttu-id="abe07-188">[ASP.NET Core Configuration API 'si](xref:fundamentals/configuration/index) , gizli yönetici sırları için erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="abe07-188">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="abe07-189">Projeniz .NET Framework hedefliyorsa [Microsoft. Extensions. Configuration. Usergizlilikler](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="abe07-189">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="abe07-190">ASP.NET Core 2,0 veya sonraki bir sürümde, proje önceden yapılandırılmış varsayılanlar ile konağın yeni bir örneğini başlatmak için çağırdığında <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> , Kullanıcı gizli dizileri yapılandırma kaynağı geliştirme moduna otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="abe07-190">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="abe07-191">`CreateDefaultBuilder`Şu<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> olduğundaçağırır<xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span><span class="sxs-lookup"><span data-stu-id="abe07-191">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="abe07-192">Çağrılmıyorsa, `Startup` oluşturucuda çağırarak <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> Kullanıcı gizli dizi yapılandırma kaynağını açıkça ekleyin. `CreateDefaultBuilder`</span><span class="sxs-lookup"><span data-stu-id="abe07-192">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor.</span></span> <span data-ttu-id="abe07-193">Aşağıdaki `AddUserSecrets` örnekte gösterildiği gibi, yalnızca uygulama geliştirme ortamında çalıştırıldığında çağırın:</span><span class="sxs-lookup"><span data-stu-id="abe07-193">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="abe07-194">[Microsoft. Extensions. Configuration. Usergizlilikler](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="abe07-194">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="abe07-195">Kullanıcı gizli dizileri yapılandırma kaynağını <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> `Startup` oluşturucuya bir çağrısıyla ekleyin:</span><span class="sxs-lookup"><span data-stu-id="abe07-195">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="abe07-196">Kullanıcı gizli dizileri `Configuration` API aracılığıyla alınabilir:</span><span class="sxs-lookup"><span data-stu-id="abe07-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="abe07-197">Gizli dizileri bir POCO ile eşleme</span><span class="sxs-lookup"><span data-stu-id="abe07-197">Map secrets to a POCO</span></span>

<span data-ttu-id="abe07-198">Bir nesne sabit değerinin tamamını bir POCO 'ya eşleme (özelliklerle basit bir .NET sınıfı) ilgili özellikleri toplamak için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="abe07-198">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="abe07-199">Önceki gizli dizileri bir poco 'ya eşlemek için, `Configuration` API 'nin [nesne grafiği bağlama](xref:fundamentals/configuration/index#bind-to-an-object-graph) özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="abe07-199">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="abe07-200">Aşağıdaki kod, özel `MovieSettings` bir poco 'a bağlanır ve `ServiceApiKey` özellik değerine erişir:</span><span class="sxs-lookup"><span data-stu-id="abe07-200">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="abe07-201">`MovieSettings`Ve `Movies:ConnectionString` gizlidizileri,içindekiilgiliözelliklerleeşlenir`Movies:ServiceApiKey` :</span><span class="sxs-lookup"><span data-stu-id="abe07-201">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="abe07-202">Gizli dizileri olan dize değiştirme</span><span class="sxs-lookup"><span data-stu-id="abe07-202">String replacement with secrets</span></span>

<span data-ttu-id="abe07-203">Parolaların düz metin olarak depolanması güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="abe07-203">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="abe07-204">Örneğin, *appSettings. JSON* içinde depolanan bir veritabanı bağlantı dizesi, belirtilen kullanıcı için bir parola içerebilir:</span><span class="sxs-lookup"><span data-stu-id="abe07-204">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="abe07-205">Daha güvenli bir yaklaşım, parolayı gizli olarak depolanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="abe07-205">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="abe07-206">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="abe07-206">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="abe07-207">Anahtar-değer çiftini *appSettings. JSON*içindeki bağlantı dizesinden kaldırın. `Password`</span><span class="sxs-lookup"><span data-stu-id="abe07-207">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="abe07-208">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="abe07-208">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="abe07-209">Gizli dizi değeri, bağlantı dizesinin tamamlanabilmesi için bir <xref:System.Data.SqlClient.SqlConnectionStringBuilder> <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> nesnenin özelliğinde ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="abe07-209">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="abe07-210">Gizli dizileri listeleyin</span><span class="sxs-lookup"><span data-stu-id="abe07-210">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="abe07-211">*. Csproj* dosyasının bulunduğu dizinden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="abe07-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="abe07-212">Aşağıdaki çıktı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="abe07-212">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="abe07-213">Yukarıdaki örnekte, anahtar adlarındaki bir iki nokta üst üste *gizli dizi. JSON*içindeki nesne hiyerarşisini gösterir.</span><span class="sxs-lookup"><span data-stu-id="abe07-213">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="abe07-214">Tek bir parolayı kaldır</span><span class="sxs-lookup"><span data-stu-id="abe07-214">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="abe07-215">*. Csproj* dosyasının bulunduğu dizinden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="abe07-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="abe07-216">Bu `MoviesConnectionString` anahtarla ilişkili anahtar-değer çiftini kaldırmak için uygulamanın *gizli dizi. JSON* dosyası değiştirildi:</span><span class="sxs-lookup"><span data-stu-id="abe07-216">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="abe07-217">Çalıştıran `dotnet user-secrets list` aşağıdaki iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="abe07-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="abe07-218">Tüm gizli dizileri kaldır</span><span class="sxs-lookup"><span data-stu-id="abe07-218">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="abe07-219">*. Csproj* dosyasının bulunduğu dizinden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="abe07-219">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="abe07-220">Uygulamanın tüm Kullanıcı gizli dizileri, *gizlilikler. JSON* dosyasından silindi:</span><span class="sxs-lookup"><span data-stu-id="abe07-220">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="abe07-221">Çalıştıran `dotnet user-secrets list` aşağıdaki iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="abe07-221">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="abe07-222">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="abe07-222">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
