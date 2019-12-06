---
title: ASP.NET Core sürümünde geliştirme sırasında uygulama gizli dizileri güvenli depolama
author: rick-anderson
description: ASP.NET Core uygulamasının geliştirilmesi sırasında gizli bilgileri uygulama gizli dizileri olarak depolamayı ve almayı öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: security/app-secrets
ms.openlocfilehash: ef5cb120c15d349be744c401bd518e026ddf11e9
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880779"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="bcd08-103">ASP.NET Core sürümünde geliştirme sırasında uygulama gizli dizileri güvenli depolama</span><span class="sxs-lookup"><span data-stu-id="bcd08-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="bcd08-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27)ve [Scott Ade](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="bcd08-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="bcd08-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bcd08-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="bcd08-106">Bu belgede, bir ASP.NET Core uygulamasının geliştirilmesi sırasında hassas verilerin depolanması ve alınması için teknikler açıklanmaktadır.</span><span class="sxs-lookup"><span data-stu-id="bcd08-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="bcd08-107">Kaynak kodunda parolaları veya diğer hassas verileri hiçbir şekilde depolamayin.</span><span class="sxs-lookup"><span data-stu-id="bcd08-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="bcd08-108">Üretim gizli dizileri geliştirme veya test için kullanılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="bcd08-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="bcd08-109">Gizli dizileri uygulamayla birlikte dağıtılmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="bcd08-109">Secrets shouldn't be deployed with the app.</span></span> <span data-ttu-id="bcd08-110">Bunun yerine, ortamlar, ortam değişkenleri, Azure Key Vault vb. gibi denetimli bir yöntemle üretim ortamında kullanılabilir hale gelmelidir. [Azure Key Vault yapılandırma sağlayıcısıyla](xref:security/key-vault-configuration)Azure test ve üretim gizli dizilerini saklayabilir ve koruyabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bcd08-110">Instead, secrets should be made available in the production environment through a controlled means like environment variables, Azure Key Vault, etc. You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="bcd08-111">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="bcd08-111">Environment variables</span></span>

<span data-ttu-id="bcd08-112">Ortam değişkenleri, kodda veya yerel yapılandırma dosyalarında uygulama gizli dizileri depolanmasını önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="bcd08-112">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="bcd08-113">Ortam değişkenleri, daha önce belirtilen tüm yapılandırma kaynakları için yapılandırma değerlerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="bcd08-113">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="bcd08-114">`Startup` oluşturucusunda <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> çağırarak ortam değişkeni değerlerinin okunmasını yapılandırın:</span><span class="sxs-lookup"><span data-stu-id="bcd08-114">Configure the reading of environment variable values by calling <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="bcd08-115">**Bireysel kullanıcı hesapları** güvenliğinin etkinleştirildiği bir ASP.NET Core Web uygulaması düşünün.</span><span class="sxs-lookup"><span data-stu-id="bcd08-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="bcd08-116">Varsayılan bir veritabanı bağlantı dizesi, projenin *appSettings. JSON* dosyasına anahtar `DefaultConnection`dahildir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="bcd08-117">Varsayılan bağlantı dizesi, kullanıcı modunda çalışan ve parola gerektirmeyen LocalDB içindir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="bcd08-118">Uygulama dağıtımı sırasında, `DefaultConnection` anahtar değeri bir ortam değişkeninin değeri ile geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="bcd08-119">Ortam değişkeni, tüm bağlantı dizesini hassas kimlik bilgileriyle saklayabilir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="bcd08-120">Ortam değişkenleri genellikle düz, şifresiz metin olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="bcd08-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="bcd08-121">Makinenin veya işlemin güvenliği tehlikeye atılırsa, ortam değişkenlerine güvenilmeyen taraflar tarafından erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="bcd08-122">Kullanıcı gizliliklerinin açıklanmasını önlemeye yönelik ek ölçüler gerekebilir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="bcd08-123">Gizli dizi Yöneticisi</span><span class="sxs-lookup"><span data-stu-id="bcd08-123">Secret Manager</span></span>

<span data-ttu-id="bcd08-124">Gizli verileri bir ASP.NET Core projesi geliştirme sırasında depolar.</span><span class="sxs-lookup"><span data-stu-id="bcd08-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="bcd08-125">Bu bağlamda, önemli bir veri parçası bir uygulama gizli dizisi.</span><span class="sxs-lookup"><span data-stu-id="bcd08-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="bcd08-126">Uygulama gizli dizileri, proje ağacından ayrı bir konumda depolanır.</span><span class="sxs-lookup"><span data-stu-id="bcd08-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="bcd08-127">Uygulama gizli dizileri belirli bir projeyle ilişkilendirilir veya çeşitli projeler arasında paylaşılır.</span><span class="sxs-lookup"><span data-stu-id="bcd08-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="bcd08-128">Uygulama gizli dizileri kaynak denetimine iade edilmedi.</span><span class="sxs-lookup"><span data-stu-id="bcd08-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="bcd08-129">Gizli dizi Yöneticisi aracı depolanan gizli dizileri şifrelemez ve güvenilir bir depo olarak değerlendirilmemelidir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="bcd08-130">Yalnızca geliştirme amaçlıdır.</span><span class="sxs-lookup"><span data-stu-id="bcd08-130">It's for development purposes only.</span></span> <span data-ttu-id="bcd08-131">Anahtarlar ve değerler, Kullanıcı profili dizinindeki bir JSON yapılandırma dosyasında depolanır.</span><span class="sxs-lookup"><span data-stu-id="bcd08-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="bcd08-132">Gizli dizi Yöneticisi aracı nasıl kullanılır?</span><span class="sxs-lookup"><span data-stu-id="bcd08-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="bcd08-133">Gizli dizi Yöneticisi Aracı, değerlerin nerede ve nasıl depolandığı gibi uygulama ayrıntılarını soyutlar.</span><span class="sxs-lookup"><span data-stu-id="bcd08-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="bcd08-134">Bu uygulama ayrıntılarını bilmeden aracı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="bcd08-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="bcd08-135">Değerler, yerel makinedeki sistem korumalı bir kullanıcı profili klasöründe bir JSON yapılandırma dosyasında depolanır:</span><span class="sxs-lookup"><span data-stu-id="bcd08-135">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="bcd08-136">Windows</span><span class="sxs-lookup"><span data-stu-id="bcd08-136">Windows</span></span>](#tab/windows)

<span data-ttu-id="bcd08-137">Dosya sistemi yolu:</span><span class="sxs-lookup"><span data-stu-id="bcd08-137">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="bcd08-138">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="bcd08-138">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="bcd08-139">Dosya sistemi yolu:</span><span class="sxs-lookup"><span data-stu-id="bcd08-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="bcd08-140">Yukarıdaki dosya yollarında `<user_secrets_id>`, *. csproj* dosyasında belirtilen `UserSecretsId` değeri ile değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bcd08-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="bcd08-141">Gizli dizi yöneticisi aracıyla kaydedilen verilerin konumuna veya biçimine bağlı olarak kod yazma.</span><span class="sxs-lookup"><span data-stu-id="bcd08-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="bcd08-142">Bu uygulama ayrıntıları değişebilir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-142">These implementation details may change.</span></span> <span data-ttu-id="bcd08-143">Örneğin, gizli değerler şifrelenmez, ancak gelecekte olabilir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="bcd08-144">Gizli dizi Yöneticisi aracını yükler</span><span class="sxs-lookup"><span data-stu-id="bcd08-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="bcd08-145">Gizli dizi Yöneticisi Aracı, .NET Core SDK 2.1.300 veya sonraki sürümlerde .NET Core CLI paketlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-145">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="bcd08-146">2\.1.300 önceki .NET Core SDK sürümler için araç yüklemesi gereklidir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="bcd08-147">Yüklü .NET Core SDK sürüm numarasını görmek için bir komut kabuğundan `dotnet --version` çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="bcd08-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="bcd08-148">Kullanılan .NET Core SDK araç içeriyorsa bir uyarı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="bcd08-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="bcd08-149">ASP.NET Core projenize [Microsoft. Extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet paketini yükle.</span><span class="sxs-lookup"><span data-stu-id="bcd08-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="bcd08-150">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bcd08-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="bcd08-151">Araç yüklemesini doğrulamak için bir komut kabuğu 'nda aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="bcd08-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```dotnetcli
dotnet user-secrets -h
```

<span data-ttu-id="bcd08-152">Gizli dizi Yöneticisi Aracı, örnek kullanım, Seçenekler ve komut yardımını görüntüler:</span><span class="sxs-lookup"><span data-stu-id="bcd08-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="bcd08-153">*. Csproj* dosyasının `DotNetCliToolReference` öğelerinde tanımlanan araçları çalıştırmak için *. csproj* dosyasıyla aynı dizinde olmanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="enable-secret-storage"></a><span data-ttu-id="bcd08-154">Gizli depolamayı etkinleştir</span><span class="sxs-lookup"><span data-stu-id="bcd08-154">Enable secret storage</span></span>

<span data-ttu-id="bcd08-155">Gizli dizi Yöneticisi Aracı, Kullanıcı profilinizde depolanan projeye özgü yapılandırma ayarları üzerinde çalışır.</span><span class="sxs-lookup"><span data-stu-id="bcd08-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bcd08-156">Gizli dizi Yöneticisi Aracı, .NET Core SDK 3.0.100 veya sonraki sürümlerde bir `init` komutu içerir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-156">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="bcd08-157">Kullanıcı gizli dizilerini kullanmak için, proje dizininde aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bcd08-157">To use user secrets, run the following command in the project directory:</span></span>

```dotnetcli
dotnet user-secrets init
```

<span data-ttu-id="bcd08-158">Önceki komut, *. csproj* dosyasının `PropertyGroup` içinde `UserSecretsId` öğesi ekler.</span><span class="sxs-lookup"><span data-stu-id="bcd08-158">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="bcd08-159">Varsayılan olarak, `UserSecretsId` iç metni bir GUID 'dir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-159">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="bcd08-160">İç metin rastgele, ancak proje için benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-160">The inner text is arbitrary, but is unique to the project.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="bcd08-161">Kullanıcı gizli dizilerini kullanmak için, *. csproj* dosyasının bir `PropertyGroup` içinde `UserSecretsId` öğesi tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="bcd08-161">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="bcd08-162">`UserSecretsId` iç metni rastgele, ancak proje için benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-162">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="bcd08-163">Geliştiriciler genellikle `UserSecretsId`için bir GUID oluşturur.</span><span class="sxs-lookup"><span data-stu-id="bcd08-163">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="bcd08-164">Visual Studio 'da Çözüm Gezgini projeye sağ tıklayın ve bağlam menüsünden **Kullanıcı gizli dizilerini Yönet** ' i seçin.</span><span class="sxs-lookup"><span data-stu-id="bcd08-164">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="bcd08-165">Bu hareket, *. csproj* DOSYASıNA bir GUID ile doldurulmuş bir `UserSecretsId` öğesi ekler.</span><span class="sxs-lookup"><span data-stu-id="bcd08-165">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="bcd08-166">Gizli dizi ayarla</span><span class="sxs-lookup"><span data-stu-id="bcd08-166">Set a secret</span></span>

<span data-ttu-id="bcd08-167">Anahtar ve değerini içeren bir uygulama gizli anahtarı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="bcd08-167">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="bcd08-168">Gizli dizi, projenin `UserSecretsId` değeriyle ilişkilendirilir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-168">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="bcd08-169">Örneğin, *. csproj* dosyasının bulunduğu dizinden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bcd08-169">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="bcd08-170">Yukarıdaki örnekte, iki nokta üst üste `Movies` bir `ServiceApiKey` özelliğine sahip bir nesne sabit değeri olduğunu gösterir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-170">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="bcd08-171">Gizli dizi Yöneticisi Aracı diğer dizinlerden de kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-171">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="bcd08-172">*. Csproj* dosyasının bulunduğu dosya sistemi yolunu sağlamak için `--project` seçeneğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="bcd08-172">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="bcd08-173">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bcd08-173">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="bcd08-174">Visual Studio 'da JSON yapısı düzleştirme</span><span class="sxs-lookup"><span data-stu-id="bcd08-174">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="bcd08-175">Visual Studio 'nun **Kullanıcı gizli dizilerini Yönet** hareketi metin düzenleyicisinde bir *gizli dizi. JSON* dosyası açar.</span><span class="sxs-lookup"><span data-stu-id="bcd08-175">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="bcd08-176">*Gizlilikler. JSON* içeriğini depolanacak anahtar-değer çiftleriyle değiştirin.</span><span class="sxs-lookup"><span data-stu-id="bcd08-176">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="bcd08-177">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bcd08-177">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="bcd08-178">JSON yapısı, `dotnet user-secrets remove` veya `dotnet user-secrets set`aracılığıyla yapılan değişikliklerden sonra düzleştirilir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-178">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="bcd08-179">Örneğin, `dotnet user-secrets remove "Movies:ConnectionString"` çalıştırmak `Movies` nesnesinin değişmez değerini daraltır.</span><span class="sxs-lookup"><span data-stu-id="bcd08-179">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="bcd08-180">Değiştirilen dosya şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="bcd08-180">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="bcd08-181">Birden çok gizli dizi ayarla</span><span class="sxs-lookup"><span data-stu-id="bcd08-181">Set multiple secrets</span></span>

<span data-ttu-id="bcd08-182">Bir toplu işlem, JSON tarafından `set` komutuna boru tarafından ayarlanabilir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-182">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="bcd08-183">Aşağıdaki örnekte, *input. JSON* dosyasının içeriği `set` komutuna yöneldir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-183">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="bcd08-184">Windows</span><span class="sxs-lookup"><span data-stu-id="bcd08-184">Windows</span></span>](#tab/windows)

<span data-ttu-id="bcd08-185">Bir komut kabuğu açın ve şu komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="bcd08-185">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[<span data-ttu-id="bcd08-186">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="bcd08-186">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="bcd08-187">Bir komut kabuğu açın ve şu komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="bcd08-187">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="bcd08-188">Gizli dizi erişimi</span><span class="sxs-lookup"><span data-stu-id="bcd08-188">Access a secret</span></span>

<span data-ttu-id="bcd08-189">[ASP.NET Core Configuration API 'si](xref:fundamentals/configuration/index) , gizli yönetici sırları için erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="bcd08-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

<span data-ttu-id="bcd08-190">Projeniz .NET Framework hedefliyorsa [Microsoft. Extensions. Configuration. Usergizlilikler](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="bcd08-190">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bcd08-191">ASP.NET Core 2,0 veya sonraki sürümlerde Kullanıcı gizli dizileri yapılandırma kaynağı, önceden yapılandırılmış varsayılanlar ile konağın yeni bir örneğini başlatmak için <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> çağırdığında geliştirme moduna otomatik olarak eklenir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="bcd08-192">`CreateDefaultBuilder`, <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>olduğunda <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> çağırır:</span><span class="sxs-lookup"><span data-stu-id="bcd08-192">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Program.cs?name=snippet_CreateHostBuilder&highlight=2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="bcd08-193">`CreateDefaultBuilder` çağrılmıyorsa, `Startup` oluşturucusunda <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> çağırarak Kullanıcı gizli dizi yapılandırma kaynağını açıkça ekleyin.</span><span class="sxs-lookup"><span data-stu-id="bcd08-193">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> in the `Startup` constructor.</span></span> <span data-ttu-id="bcd08-194">Aşağıdaki örnekte gösterildiği gibi, uygulama geliştirme ortamında çalıştırıldığında `AddUserSecrets` çağırın:</span><span class="sxs-lookup"><span data-stu-id="bcd08-194">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup2.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="bcd08-195">[Microsoft. Extensions. Configuration. Usergizlilikler](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketini yükler.</span><span class="sxs-lookup"><span data-stu-id="bcd08-195">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="bcd08-196">`Startup` oluşturucusunda <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> çağrısıyla Kullanıcı gizli dizi yapılandırma kaynağını ekleyin:</span><span class="sxs-lookup"><span data-stu-id="bcd08-196">Add the user secrets configuration source with a call to <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="bcd08-197">Kullanıcı gizli dizileri `Configuration` API 'SI aracılığıyla alınabilir:</span><span class="sxs-lookup"><span data-stu-id="bcd08-197">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="bcd08-198">Gizli dizileri bir POCO ile eşleme</span><span class="sxs-lookup"><span data-stu-id="bcd08-198">Map secrets to a POCO</span></span>

<span data-ttu-id="bcd08-199">Bir nesne sabit değerinin tamamını bir POCO 'ya eşleme (özelliklerle basit bir .NET sınıfı) ilgili özellikleri toplamak için faydalıdır.</span><span class="sxs-lookup"><span data-stu-id="bcd08-199">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="bcd08-200">Önceki gizli dizileri bir POCO 'ya eşlemek için `Configuration` API 'sinin [nesne grafiği bağlama](xref:fundamentals/configuration/index#bind-to-an-object-graph) özelliğini kullanın.</span><span class="sxs-lookup"><span data-stu-id="bcd08-200">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="bcd08-201">Aşağıdaki kod, özel bir `MovieSettings` POCO 'a bağlanır ve `ServiceApiKey` özellik değerine erişir:</span><span class="sxs-lookup"><span data-stu-id="bcd08-201">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="bcd08-202">`Movies:ConnectionString` ve `Movies:ServiceApiKey` gizli dizileri `MovieSettings`ilgili özelliklerle eşlenir:</span><span class="sxs-lookup"><span data-stu-id="bcd08-202">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="bcd08-203">Gizli dizileri olan dize değiştirme</span><span class="sxs-lookup"><span data-stu-id="bcd08-203">String replacement with secrets</span></span>

<span data-ttu-id="bcd08-204">Parolaların düz metin olarak depolanması güvenli değildir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-204">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="bcd08-205">Örneğin, *appSettings. JSON* içinde depolanan bir veritabanı bağlantı dizesi, belirtilen kullanıcı için bir parola içerebilir:</span><span class="sxs-lookup"><span data-stu-id="bcd08-205">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="bcd08-206">Daha güvenli bir yaklaşım, parolayı gizli olarak depolanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="bcd08-206">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="bcd08-207">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bcd08-207">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="bcd08-208">`Password` anahtar-değer çiftini *appSettings. JSON*içindeki bağlantı dizesinden kaldırın.</span><span class="sxs-lookup"><span data-stu-id="bcd08-208">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="bcd08-209">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="bcd08-209">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="bcd08-210">Gizli dizi değeri, bağlantı dizesinin tamamlanabilmesi için <xref:System.Data.SqlClient.SqlConnectionStringBuilder> nesnenin <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> özelliğinde ayarlanabilir:</span><span class="sxs-lookup"><span data-stu-id="bcd08-210">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="bcd08-211">Gizli dizileri listeleyin</span><span class="sxs-lookup"><span data-stu-id="bcd08-211">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="bcd08-212">*. Csproj* dosyasının bulunduğu dizinden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bcd08-212">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="bcd08-213">Şu çıktı görünür:</span><span class="sxs-lookup"><span data-stu-id="bcd08-213">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="bcd08-214">Yukarıdaki örnekte, anahtar adlarındaki bir iki nokta üst üste *gizli dizi. JSON*içindeki nesne hiyerarşisini gösterir.</span><span class="sxs-lookup"><span data-stu-id="bcd08-214">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="bcd08-215">Tek bir parolayı kaldır</span><span class="sxs-lookup"><span data-stu-id="bcd08-215">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="bcd08-216">*. Csproj* dosyasının bulunduğu dizinden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bcd08-216">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="bcd08-217">Uygulamanın *gizlilikler. JSON* dosyası, `MoviesConnectionString` anahtarıyla ilişkili anahtar-değer çiftini kaldırmak için değiştirilmiştir:</span><span class="sxs-lookup"><span data-stu-id="bcd08-217">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="bcd08-218">`dotnet user-secrets list` çalıştırmak aşağıdaki iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="bcd08-218">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="bcd08-219">Tüm gizli dizileri kaldır</span><span class="sxs-lookup"><span data-stu-id="bcd08-219">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="bcd08-220">*. Csproj* dosyasının bulunduğu dizinden aşağıdaki komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="bcd08-220">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="bcd08-221">Uygulamanın tüm Kullanıcı gizli dizileri, *gizlilikler. JSON* dosyasından silindi:</span><span class="sxs-lookup"><span data-stu-id="bcd08-221">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="bcd08-222">`dotnet user-secrets list` çalıştırmak aşağıdaki iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="bcd08-222">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="bcd08-223">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="bcd08-223">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
