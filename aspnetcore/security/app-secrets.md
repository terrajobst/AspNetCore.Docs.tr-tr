---
title: ASP.NET core'da geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması
author: rick-anderson
description: ASP.NET Core uygulaması geliştirme sırasında uygulama gizli diziler olarak hassas bilgilerini depolamak ve almak öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 08/16/2018
uid: security/app-secrets
ms.openlocfilehash: 35c316230c19aa69a0dac26ec25a6e017f102237
ms.sourcegitcommit: 1cf65c25ed16495e27f35ded98b3952a30c68f36
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "41754196"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="2058f-103">ASP.NET core'da geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması</span><span class="sxs-lookup"><span data-stu-id="2058f-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="2058f-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), ve [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="2058f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="2058f-105">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2058f-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="2058f-106">Bu belge, depolamak ve ASP.NET Core uygulaması geliştirme sırasında hassas verileri almak için teknikleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="2058f-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="2058f-107">Kaynak kodunda hiçbir zaman parolaları ve diğer hassas verileri saklamalısınız ve geliştirmede üretim gizli anahtarları kullanma veya test modu olmamalıdır.</span><span class="sxs-lookup"><span data-stu-id="2058f-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="2058f-108">Depolama ve Azure test ve üretim parolalarını ile korumak [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="2058f-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="2058f-109">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="2058f-109">Environment variables</span></span>

<span data-ttu-id="2058f-110">Ortam değişkenleri, uygulama gizli anahtarlarının kod veya yerel yapılandırma dosyaları depolama önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="2058f-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="2058f-111">Ortam değişkenleri tüm daha önce belirtilen yapılandırma kaynakları için yapılandırma değerlerini geçersiz kılar.</span><span class="sxs-lookup"><span data-stu-id="2058f-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="2058f-112">Ortam değişkeni değerlerini okunmasını çağırarak yapılandırma [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) içinde `Startup` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="2058f-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

<span data-ttu-id="2058f-113">ASP.NET Core web uygulaması, göz önünde bulundurun **bireysel kullanıcı hesapları** güvenlik etkin.</span><span class="sxs-lookup"><span data-stu-id="2058f-113">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="2058f-114">Varsayılan veritabanı bağlantı dizesi projesinin dahil *appsettings.json* anahtar dosyasıyla `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="2058f-114">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="2058f-115">Varsayılan bağlantı dizesini kullanıcı modunda çalışır ve bir parola gerektirmez LocalDB ' dir.</span><span class="sxs-lookup"><span data-stu-id="2058f-115">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="2058f-116">Uygulama dağıtımı sırasında `DefaultConnection` anahtar değeri ile bir ortam değişken değerini geçersiz kılınabilir.</span><span class="sxs-lookup"><span data-stu-id="2058f-116">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="2058f-117">Ortam değişkeni hassas kimlik bilgileriyle tam bağlantı dizesi depolayabilir.</span><span class="sxs-lookup"><span data-stu-id="2058f-117">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="2058f-118">Ortam değişkenleri genellikle düz ve şifresiz metin olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="2058f-118">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="2058f-119">İşlem ve makine tehlikedeyse, ortam değişkenleri Güvenilmeyen taraflar tarafından erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="2058f-119">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="2058f-120">Kullanıcı gizli dizilerinin açığa çıkmasını önlemek amacıyla ek ölçüler gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="2058f-120">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="2058f-121">Gizli dizi Yöneticisi</span><span class="sxs-lookup"><span data-stu-id="2058f-121">Secret Manager</span></span>

<span data-ttu-id="2058f-122">Gizli dizi Yöneticisi Aracı, bir ASP.NET Core projesi geliştirilmesi sırasında hassas verileri depolar.</span><span class="sxs-lookup"><span data-stu-id="2058f-122">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="2058f-123">Bu bağlamda bir hassas verileri uygulama gizli anahtarı bir parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="2058f-123">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="2058f-124">Uygulama gizli anahtarlarının proje ağacı ayrı bir konumda depolanır.</span><span class="sxs-lookup"><span data-stu-id="2058f-124">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="2058f-125">Uygulama gizli dizileri belirli bir projeyle ilişkili veya birkaç projede paylaşılan.</span><span class="sxs-lookup"><span data-stu-id="2058f-125">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="2058f-126">Uygulama gizli anahtarlarının kaynak denetimine denetlenmez.</span><span class="sxs-lookup"><span data-stu-id="2058f-126">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="2058f-127">Gizli dizi Yöneticisi aracını depolanan gizli dizileri şifrelemez ve güvenilir bir deposu olarak değerlendirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="2058f-127">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="2058f-128">Bu, yalnızca geliştirme amaçları içindir.</span><span class="sxs-lookup"><span data-stu-id="2058f-128">It's for development purposes only.</span></span> <span data-ttu-id="2058f-129">Anahtarları ve değerleri, kullanıcı profili dizini bir JSON yapılandırma dosyasında depolanır.</span><span class="sxs-lookup"><span data-stu-id="2058f-129">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="2058f-130">Gizli dizi Yöneticisi aracını nasıl çalışır?</span><span class="sxs-lookup"><span data-stu-id="2058f-130">How the Secret Manager tool works</span></span>

<span data-ttu-id="2058f-131">Gizli dizi Yöneticisi aracını nerede ve nasıl depolanacağını ve gibi uygulama ayrıntılarını dengelediği.</span><span class="sxs-lookup"><span data-stu-id="2058f-131">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="2058f-132">Bu uygulama ayrıntılarını bilmeden aracını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2058f-132">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="2058f-133">Değerleri, yerel makinede bulunan JSON yapılandırma dosyası bir sistem korumalı kullanıcı profili klasöründe depolanır:</span><span class="sxs-lookup"><span data-stu-id="2058f-133">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2058f-134">Windows</span><span class="sxs-lookup"><span data-stu-id="2058f-134">Windows</span></span>](#tab/windows)

<span data-ttu-id="2058f-135">Dosya sistemi yolu:</span><span class="sxs-lookup"><span data-stu-id="2058f-135">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="2058f-136">macOS</span><span class="sxs-lookup"><span data-stu-id="2058f-136">macOS</span></span>](#tab/macos)

<span data-ttu-id="2058f-137">Dosya sistemi yolu:</span><span class="sxs-lookup"><span data-stu-id="2058f-137">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="2058f-138">Linux</span><span class="sxs-lookup"><span data-stu-id="2058f-138">Linux</span></span>](#tab/linux)

<span data-ttu-id="2058f-139">Dosya sistemi yolu:</span><span class="sxs-lookup"><span data-stu-id="2058f-139">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="2058f-140">Önceki dosya yolları ve yerine `<user_secrets_id>` ile `UserSecretsId` belirtilen değeri *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="2058f-140">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="2058f-141">Konum veya gizli dizi Yöneticisi Aracı ile kaydedilen verilerin biçimi bağımlı kod yazmayın.</span><span class="sxs-lookup"><span data-stu-id="2058f-141">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="2058f-142">Bu uygulama ayrıntılarını değişebilir.</span><span class="sxs-lookup"><span data-stu-id="2058f-142">These implementation details may change.</span></span> <span data-ttu-id="2058f-143">Örneğin, gizli değerleri şifreli değildir, ancak gelecekte olabilir.</span><span class="sxs-lookup"><span data-stu-id="2058f-143">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="2058f-144">Gizli dizi Yöneticisi aracını yükleyin</span><span class="sxs-lookup"><span data-stu-id="2058f-144">Install the Secret Manager tool</span></span>

<span data-ttu-id="2058f-145">Gizli dizi Yöneticisi Aracı ile .NET Core SDK'sı 2.1.300 .NET Core CLI ile birlikte gelen veya üzeri.</span><span class="sxs-lookup"><span data-stu-id="2058f-145">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.300 or later.</span></span> <span data-ttu-id="2058f-146">Aracı yükleme 2.1.300 önce .NET Core SDK sürümleri için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="2058f-146">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="2058f-147">Çalıştırma `dotnet --version` yüklü .NET Core SDK'sı sürüm numarasını görmek için bir komut kabuğu'ndan.</span><span class="sxs-lookup"><span data-stu-id="2058f-147">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="2058f-148">.NET Core SDK kullanılan araç içeriyorsa, bir uyarı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="2058f-148">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="2058f-149">Yükleme [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core projenizdeki NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="2058f-149">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="2058f-150">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2058f-150">For example:</span></span>

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

<span data-ttu-id="2058f-151">Aracı yüklemesini doğrulamak için bir komut kabuğuna şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="2058f-151">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="2058f-152">Gizli dizi Yöneticisi aracını örnek kullanımı, Seçenekler ve komut Yardımı görüntüler:</span><span class="sxs-lookup"><span data-stu-id="2058f-152">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="2058f-153">Aynı dizinde olmalıdır *.csproj* tanımlanan araçları çalıştırmak için dosya *.csproj* dosyanın `DotNetCliToolReference` öğeleri.</span><span class="sxs-lookup"><span data-stu-id="2058f-153">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>

::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="2058f-154">Bir gizli dizisi ayarlayın</span><span class="sxs-lookup"><span data-stu-id="2058f-154">Set a secret</span></span>

<span data-ttu-id="2058f-155">Projeye özgü yapılandırma ayarlarını, kullanıcı profilinizin depolanan gizli dizi Yöneticisi aracı çalışır.</span><span class="sxs-lookup"><span data-stu-id="2058f-155">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="2058f-156">Kullanıcı parolalarını kullanmak için tanımlamak bir `UserSecretsId` öğesi içinde bir `PropertyGroup` , *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="2058f-156">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="2058f-157">Değerini `UserSecretsId` isteğe bağlıdır, ancak projeye benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="2058f-157">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="2058f-158">Geliştiriciler genellikle oluşturmak için bir GUID `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="2058f-158">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> <span data-ttu-id="2058f-159">Visual Studio'da, Çözüm Gezgini'nde projeye sağ tıklayıp seçin **nıcı parolalarını Yönet** bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="2058f-159">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="2058f-160">Bu hareket ekler bir `UserSecretsId` öğesi için bir GUID ile doldurulmuş *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="2058f-160">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="2058f-161">Visual Studio açılır bir *secrets.json* dosyasını metin düzenleyicisinde.</span><span class="sxs-lookup"><span data-stu-id="2058f-161">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="2058f-162">Öğesinin içeriğini değiştirin *secrets.json* depolanacak anahtar-değer çiftleri ile.</span><span class="sxs-lookup"><span data-stu-id="2058f-162">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="2058f-163">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2058f-163">For example:</span></span>
> ```json
> {
>   "Movies": {
>     "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
>     "ServiceApiKey": "12345"
>   }
> }
> ```
> <span data-ttu-id="2058f-164">JSON yapısı değişiklikleri sonra düzleştirilmiş `dotnet user-secrets remove` veya `dotnet user-secrets set`.</span><span class="sxs-lookup"><span data-stu-id="2058f-164">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="2058f-165">Örneğin, çalışan `dotnet user-secrets remove "Movies:ConnectionString"` daraltır `Movies` nesne sabit değeri.</span><span class="sxs-lookup"><span data-stu-id="2058f-165">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="2058f-166">Değiştirilen dosya şuna benzer:</span><span class="sxs-lookup"><span data-stu-id="2058f-166">The modified file resembles the following:</span></span>
> ```json
> {
>   "Movies:ServiceApiKey": "12345"
> }
> ```

<span data-ttu-id="2058f-167">Bir anahtarı ve değeri içeren bir uygulama gizli anahtarı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="2058f-167">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="2058f-168">Proje gizliliği ilişkilendirilen `UserSecretsId` değeri.</span><span class="sxs-lookup"><span data-stu-id="2058f-168">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="2058f-169">Örneğin, hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası var:</span><span class="sxs-lookup"><span data-stu-id="2058f-169">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="2058f-170">Önceki örnekte, iki nokta üst üste gösterir `Movies` bir nesne ile sabitidir bir `ServiceApiKey` özelliği.</span><span class="sxs-lookup"><span data-stu-id="2058f-170">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="2058f-171">Gizli dizi Yöneticisi Aracı diğer dizinlerden çok kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2058f-171">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="2058f-172">Kullanım `--project` seçeneği, dosya sistemi yolu sağlamak *.csproj* dosya yok.</span><span class="sxs-lookup"><span data-stu-id="2058f-172">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="2058f-173">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2058f-173">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="2058f-174">Birden çok gizli dizileri ayarlayın</span><span class="sxs-lookup"><span data-stu-id="2058f-174">Set multiple secrets</span></span>

<span data-ttu-id="2058f-175">Gizli dizi için JSON yönelterek ayarlanabilir `set` komutu.</span><span class="sxs-lookup"><span data-stu-id="2058f-175">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="2058f-176">Aşağıdaki örnekte, *söz konusu input.json* dosyanın içeriğini yöneltilen için `set` komutu.</span><span class="sxs-lookup"><span data-stu-id="2058f-176">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2058f-177">Windows</span><span class="sxs-lookup"><span data-stu-id="2058f-177">Windows</span></span>](#tab/windows)

<span data-ttu-id="2058f-178">Bir komut kabuğunu açın ve aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="2058f-178">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="2058f-179">macOS</span><span class="sxs-lookup"><span data-stu-id="2058f-179">macOS</span></span>](#tab/macos)

<span data-ttu-id="2058f-180">Bir komut kabuğunu açın ve aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="2058f-180">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2058f-181">Linux</span><span class="sxs-lookup"><span data-stu-id="2058f-181">Linux</span></span>](#tab/linux)

<span data-ttu-id="2058f-182">Bir komut kabuğunu açın ve aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="2058f-182">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="2058f-183">Gizli dizi erişimi</span><span class="sxs-lookup"><span data-stu-id="2058f-183">Access a secret</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="2058f-184">[ASP.NET Core yapılandırma API'si](xref:fundamentals/configuration/index) gizli dizi Yöneticisi gizli dizilere erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="2058f-184">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="2058f-185">Projeniz .NET Framework hedefliyorsa, yükleme [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="2058f-185">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="2058f-186">Proje çağırdığında, ASP.NET Core 2.0 veya sonraki sürümlerde, kullanıcı parolaları yapılandırma kaynağı otomatik olarak geliştirme modunda eklenir [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) önceden yapılandırılmış varsayılan ana bilgisayar yeni bir örneğini başlatmak için.</span><span class="sxs-lookup"><span data-stu-id="2058f-186">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="2058f-187">`CreateDefaultBuilder` çağrıları [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) olduğunda [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) olduğu [geliştirme](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="2058f-187">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="2058f-188">Zaman `CreateDefaultBuilder` olmadığından konak yapım sırasında çağrılır, kullanıcı parolaları yapılandırma kaynağı çağrısıyla Ekle [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) içinde `Startup` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="2058f-188">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="2058f-189">[ASP.NET Core yapılandırma API'si](xref:fundamentals/configuration/index) gizli dizi Yöneticisi gizli dizilere erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="2058f-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="2058f-190">Yükleme [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="2058f-190">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="2058f-191">Kullanıcı parolaları yapılandırma kaynağı çağrısıyla ekleme [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) içinde `Startup` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="2058f-191">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

<span data-ttu-id="2058f-192">Kullanıcı parolaları aracılığıyla alınabilir `Configuration` API:</span><span class="sxs-lookup"><span data-stu-id="2058f-192">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="2058f-193">Gizli diziler için bir POCO eşleme</span><span class="sxs-lookup"><span data-stu-id="2058f-193">Map secrets to a POCO</span></span>

<span data-ttu-id="2058f-194">Tüm nesne sabit değeri bir POCO (basit bir .NET sınıf özelliklere sahip) eşlemeye ilgili özellikleri toplamak için yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="2058f-194">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="2058f-195">Önceki gizli dizileri için bir POCO eşleyin `Configuration` API'nin [Nesne grafiği bağlama](xref:fundamentals/configuration/index#bind-to-an-object-graph) özelliği.</span><span class="sxs-lookup"><span data-stu-id="2058f-195">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="2058f-196">Aşağıdaki kod bir özel bağlar `MovieSettings` POCO ve erişimleri `ServiceApiKey` özellik değeri:</span><span class="sxs-lookup"><span data-stu-id="2058f-196">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

<span data-ttu-id="2058f-197">`Movies:ConnectionString` Ve `Movies:ServiceApiKey` gizli dizileri ilgili özellikler eşleştirilmiş `MovieSettings`:</span><span class="sxs-lookup"><span data-stu-id="2058f-197">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="2058f-198">Gizli dizesini değiştirme</span><span class="sxs-lookup"><span data-stu-id="2058f-198">String replacement with secrets</span></span>

<span data-ttu-id="2058f-199">Parolaları düz metin halinde depolanmasını güvenli değil.</span><span class="sxs-lookup"><span data-stu-id="2058f-199">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="2058f-200">Örneğin, bir veritabanı bağlantı dizesi, içinde depolanan *appsettings.json* belirtilen kullanıcı için bir parola içerebilir:</span><span class="sxs-lookup"><span data-stu-id="2058f-200">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="2058f-201">Bir gizli dizi parolayı depolamak daha güvenli bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="2058f-201">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="2058f-202">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2058f-202">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="2058f-203">Kaldırma `Password` bağlantı dizesinde anahtar-değer çiftinden *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="2058f-203">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="2058f-204">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="2058f-204">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="2058f-205">Parolanın değer ayarlanabilir bir [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) nesnenin [parola](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) özelliği bağlantı dizesini tamamlamak için:</span><span class="sxs-lookup"><span data-stu-id="2058f-205">The secret's value can be set on a [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) object's [Password](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) property to complete the connection string:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="2058f-206">Gizli dizilerini listeleme</span><span class="sxs-lookup"><span data-stu-id="2058f-206">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="2058f-207">Hangi dizininden aşağıdaki komutu çalıştırarak *.csproj* dosyası var:</span><span class="sxs-lookup"><span data-stu-id="2058f-207">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="2058f-208">Aşağıdaki çıktı görünür:</span><span class="sxs-lookup"><span data-stu-id="2058f-208">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="2058f-209">Önceki örnekte, iki nokta üst üste anahtar adlarının nesne hiyerarşisi içinde gösterir. *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="2058f-209">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="2058f-210">Tek bir gizli dizi Kaldır</span><span class="sxs-lookup"><span data-stu-id="2058f-210">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="2058f-211">Hangi dizininden aşağıdaki komutu çalıştırarak *.csproj* dosyası var:</span><span class="sxs-lookup"><span data-stu-id="2058f-211">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="2058f-212">Uygulamanın *secrets.json* dosyası ile ilişkili anahtar-değer çiftini kaldırmak için değiştirildiği `MoviesConnectionString` anahtarı:</span><span class="sxs-lookup"><span data-stu-id="2058f-212">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="2058f-213">Çalışan `dotnet user-secrets list` aşağıdaki iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="2058f-213">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="2058f-214">Tüm gizli dizileri kaldırın</span><span class="sxs-lookup"><span data-stu-id="2058f-214">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="2058f-215">Hangi dizininden aşağıdaki komutu çalıştırarak *.csproj* dosyası var:</span><span class="sxs-lookup"><span data-stu-id="2058f-215">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="2058f-216">Uygulama için tüm kullanıcı gizli dizilerini gelen silinmiş *secrets.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="2058f-216">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="2058f-217">Çalışan `dotnet user-secrets list` aşağıdaki iletiyi görüntüler:</span><span class="sxs-lookup"><span data-stu-id="2058f-217">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="2058f-218">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="2058f-218">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
