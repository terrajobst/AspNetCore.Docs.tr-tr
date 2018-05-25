---
title: Güvenli Depolama Uygulama sırrı ASP.NET Core geliştirme
author: rick-anderson
description: Bir ASP.NET Core uygulama geliştirme sırasında uygulama sırrı olarak hassas bilgilerini depolamak ve almak öğrenin.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: ece2bf541df2b4acac60a88767cc57ede473bd49
ms.sourcegitcommit: 1b94305cc79843e2b0866dae811dab61c21980ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/24/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="62563-103">Güvenli Depolama Uygulama sırrı ASP.NET Core geliştirme</span><span class="sxs-lookup"><span data-stu-id="62563-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="62563-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), ve [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="62563-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="62563-105">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="62563-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="62563-106">Bu belge, depolamak ve almak için kullanılan hassas verileri ASP.NET Core uygulama geliştirme sırasında teknikleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="62563-106">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="62563-107">Kaynak kodunda parolalar ve diğer hassas verileri asla saklamalısınız ve geliştirme üretim gizlilikleri kullanın veya modu test döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="62563-107">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="62563-108">Depolama ve Azure test ve üretim parolaları ile korumak [Azure anahtar kasası yapılandırma sağlayıcısı](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="62563-108">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="62563-109">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="62563-109">Environment variables</span></span>

<span data-ttu-id="62563-110">Ortam değişkenleri, uygulama gizli kod veya yerel yapılandırma dosyalarını depolanmasını önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="62563-110">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="62563-111">Ortam değişkenleri tüm daha önce belirtilen yapılandırma kaynakları için yapılandırma değerleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="62563-111">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="62563-112">Ortam değişkeni değerlerini okuma çağırarak yapılandırma [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) içinde `Startup` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="62563-112">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="62563-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="62563-113">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="62563-114">Bir ASP.NET Core web uygulaması, göz önünde bulundurun **tek tek kullanıcı hesaplarını** güvenlik etkindir.</span><span class="sxs-lookup"><span data-stu-id="62563-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="62563-115">Bir varsayılan veritabanı bağlantı dizesi projesinin dahil *appsettings.json* anahtar dosyasıyla `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="62563-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="62563-116">Varsayılan bağlantı dizesini kullanıcı modunda çalışır ve bir parola gerektirmez LocalDB ' dir.</span><span class="sxs-lookup"><span data-stu-id="62563-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="62563-117">Uygulama dağıtımı sırasında `DefaultConnection` anahtar değeri ile bir ortam değişkeninin değeri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="62563-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="62563-118">Ortam değişkeni hassas kimlik bilgileriyle tam bağlantı dizesi depolayabilir.</span><span class="sxs-lookup"><span data-stu-id="62563-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="62563-119">Ortam değişkenleri genellikle düz ve şifresiz metin olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="62563-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="62563-120">Makine ya da işlem aşılırsa, ortam değişkenleri Güvenilmeyen taraflar tarafından erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="62563-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="62563-121">Kullanıcı parolaları açığa çıkmasını önlemek için ek önlemler gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="62563-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="62563-122">Parola Yöneticisi</span><span class="sxs-lookup"><span data-stu-id="62563-122">Secret Manager</span></span>

<span data-ttu-id="62563-123">Gizli Yöneticisi aracını hassas verileri ASP.NET Core projesinde geliştirme sırasında depolar.</span><span class="sxs-lookup"><span data-stu-id="62563-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="62563-124">Bu bağlamda bir gizli verilerin bir uygulama gizli anahtarı parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="62563-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="62563-125">Uygulama parolaları proje ağacından ayrı bir konumda depolanır.</span><span class="sxs-lookup"><span data-stu-id="62563-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="62563-126">Uygulama parolaları belirli bir projeyle ilişkili veya birkaç projeler arasında paylaşılan.</span><span class="sxs-lookup"><span data-stu-id="62563-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="62563-127">Uygulama parolaları kaynak denetimine iade değil.</span><span class="sxs-lookup"><span data-stu-id="62563-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="62563-128">Gizli Yöneticisi aracını depolanan parolaları şifrelemek değil ve bir güvenilen deposu olarak değerlendirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="62563-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="62563-129">Yalnızca geliştirme amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="62563-129">It's for development purposes only.</span></span> <span data-ttu-id="62563-130">Anahtarları ve değerleri, kullanıcı profili dizini bir JSON yapılandırma dosyasında depolanır.</span><span class="sxs-lookup"><span data-stu-id="62563-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="62563-131">Parola Yöneticisi aracını nasıl çalışır</span><span class="sxs-lookup"><span data-stu-id="62563-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="62563-132">Parola Yöneticisi aracını hemen nerede ve nasıl değerleri saklanır gibi uygulama ayrıntılarını soyutlar.</span><span class="sxs-lookup"><span data-stu-id="62563-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="62563-133">Bu uygulama ayrıntılarını bilmeden aracını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="62563-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="62563-134">Değerleri bir sistem korumalı kullanıcı profili klasörü JSON yapılandırma dosyasında yerel makine üzerinde saklanır:</span><span class="sxs-lookup"><span data-stu-id="62563-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="62563-135">Windows</span><span class="sxs-lookup"><span data-stu-id="62563-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="62563-136">Dosya sistemi yolu:</span><span class="sxs-lookup"><span data-stu-id="62563-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[<span data-ttu-id="62563-137">macOS</span><span class="sxs-lookup"><span data-stu-id="62563-137">macOS</span></span>](#tab/macos)

<span data-ttu-id="62563-138">Dosya sistemi yolu:</span><span class="sxs-lookup"><span data-stu-id="62563-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[<span data-ttu-id="62563-139">Linux</span><span class="sxs-lookup"><span data-stu-id="62563-139">Linux</span></span>](#tab/linux)

<span data-ttu-id="62563-140">Dosya sistemi yolu:</span><span class="sxs-lookup"><span data-stu-id="62563-140">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="62563-141">Önceki dosya yolları ve Değiştir `<user_secrets_id>` ile `UserSecretsId` belirtilen değer *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="62563-141">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="62563-142">Konum veya gizli anahtarı Yöneticisi aracıyla kaydedilen verilerin biçimi bağlıdır kod yazmayın.</span><span class="sxs-lookup"><span data-stu-id="62563-142">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="62563-143">Bu uygulama ayrıntılarını değişebilir.</span><span class="sxs-lookup"><span data-stu-id="62563-143">These implementation details may change.</span></span> <span data-ttu-id="62563-144">Örneğin, gizli değerleri şifreli değildir, ancak gelecekte olabilir.</span><span class="sxs-lookup"><span data-stu-id="62563-144">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="62563-145">Parola Yöneticisi aracını yükleyin</span><span class="sxs-lookup"><span data-stu-id="62563-145">Install the Secret Manager tool</span></span>

<span data-ttu-id="62563-146">.NET Core SDK 2.1.300 itibariyle .NET Core CLI ile gizli Yöneticisi aracını paketlenebilir.</span><span class="sxs-lookup"><span data-stu-id="62563-146">The Secret Manager tool is bundled with the .NET Core CLI as of .NET Core SDK 2.1.300.</span></span> <span data-ttu-id="62563-147">Aracı yükleme 2.1.300 önce .NET Core SDK sürümleri için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="62563-147">For .NET Core SDK versions before 2.1.300, tool installation is necessary.</span></span>

> [!TIP]
> <span data-ttu-id="62563-148">Çalıştırma `dotnet --version` yüklü .NET Core SDK sürüm numarasını görmek için bir komut kabuğu'ndan.</span><span class="sxs-lookup"><span data-stu-id="62563-148">Run `dotnet --version` from a command shell to see the installed .NET Core SDK version number.</span></span>

<span data-ttu-id="62563-149">.NET Core kullanılan SDK aracı içeriyorsa, bir uyarı görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="62563-149">A warning is displayed if the .NET Core SDK being used includes the tool:</span></span>

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

<span data-ttu-id="62563-150">Yükleme [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core projenizdeki NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="62563-150">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project.</span></span> <span data-ttu-id="62563-151">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="62563-151">For example:</span></span>

<span data-ttu-id="62563-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="62563-152">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="62563-153">Aracı yüklemesini doğrulamak için bir komut kabuğuna şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="62563-153">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="62563-154">Gizli Yöneticisi aracını örnek kullanım, seçenekleri ve komut Yardımı görüntüler:</span><span class="sxs-lookup"><span data-stu-id="62563-154">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="62563-155">Aynı dizinde olmalıdır *.csproj* tanımlanan araçları çalıştırmak için dosya *.csproj* dosyanın `DotNetCliToolReference` öğeleri.</span><span class="sxs-lookup"><span data-stu-id="62563-155">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="62563-156">Bir parola ayarlama</span><span class="sxs-lookup"><span data-stu-id="62563-156">Set a secret</span></span>

<span data-ttu-id="62563-157">Projeye özgü yapılandırma ayarlarını kullanıcı profilinizde depolanan gizli Manager aracı çalışır.</span><span class="sxs-lookup"><span data-stu-id="62563-157">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="62563-158">Kullanıcı parolaları kullanmak üzere tanımladığınız bir `UserSecretsId` öğesi içinde bir `PropertyGroup` , *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="62563-158">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="62563-159">Değeri `UserSecretsId` isteğe bağlıdır, ancak projeye benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="62563-159">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="62563-160">Geliştiriciler genellikle oluşturmak için bir GUID `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="62563-160">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="62563-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="62563-161">[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="62563-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="62563-162">[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="62563-163">Visual Studio'da, Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **kullanıcı parolaları yönetme** ve bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="62563-163">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="62563-164">Bu hareketi ekler bir `UserSecretsId` öğesi, bir GUID ile çok doldurulmuş *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="62563-164">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="62563-165">Visual Studio açılır bir *secrets.json* dosyasını bir metin düzenleyicisinde.</span><span class="sxs-lookup"><span data-stu-id="62563-165">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="62563-166">Değiştir *secrets.json* depolanması için anahtar-değer çiftleri ile.</span><span class="sxs-lookup"><span data-stu-id="62563-166">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="62563-167">Örneğin: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="62563-167">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="62563-168">Bir anahtarı ve değeri oluşan bir uygulama gizli anahtarı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="62563-168">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="62563-169">Gizli projenin ilişkili olmadığından `UserSecretsId` değeri.</span><span class="sxs-lookup"><span data-stu-id="62563-169">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="62563-170">Örneğin, hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası bulunmaktadır:</span><span class="sxs-lookup"><span data-stu-id="62563-170">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="62563-171">Önceki örnekte, iki nokta üst üste gösterir `Movies` bir nesne hazır olan bir `ServiceApiKey` özelliği.</span><span class="sxs-lookup"><span data-stu-id="62563-171">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="62563-172">Gizli Manager aracı diğer dizinlerden çok kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="62563-172">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="62563-173">Kullanım `--project` seçeneği, dosya sistemi yolu sağlamak için *.csproj* dosya yok.</span><span class="sxs-lookup"><span data-stu-id="62563-173">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="62563-174">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="62563-174">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="62563-175">Birden çok gizli ayarlayın</span><span class="sxs-lookup"><span data-stu-id="62563-175">Set multiple secrets</span></span>

<span data-ttu-id="62563-176">JSON olarak cmdlet'ine yönelterek gizli toplu ayarlanabilir `set` komutu.</span><span class="sxs-lookup"><span data-stu-id="62563-176">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="62563-177">Aşağıdaki örnekte, *input.json* dosyanın içeriğini yöneltilen için `set` komutu.</span><span class="sxs-lookup"><span data-stu-id="62563-177">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="62563-178">Windows</span><span class="sxs-lookup"><span data-stu-id="62563-178">Windows</span></span>](#tab/windows)

<span data-ttu-id="62563-179">Bir komut kabuğu'nu açın ve aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="62563-179">Open a command shell, and execute the following command:</span></span>

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[<span data-ttu-id="62563-180">macOS</span><span class="sxs-lookup"><span data-stu-id="62563-180">macOS</span></span>](#tab/macos)

<span data-ttu-id="62563-181">Bir komut kabuğu'nu açın ve aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="62563-181">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[<span data-ttu-id="62563-182">Linux</span><span class="sxs-lookup"><span data-stu-id="62563-182">Linux</span></span>](#tab/linux)

<span data-ttu-id="62563-183">Bir komut kabuğu'nu açın ve aşağıdaki komutu yürütün:</span><span class="sxs-lookup"><span data-stu-id="62563-183">Open a command shell, and execute the following command:</span></span>

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="62563-184">Bir gizli anahtar erişimi</span><span class="sxs-lookup"><span data-stu-id="62563-184">Access a secret</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="62563-185">[ASP.NET çekirdek yapılandırma API'si](xref:fundamentals/configuration/index) gizli Yöneticisi gizli erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="62563-185">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="62563-186">Yükleme [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="62563-186">Install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="62563-187">Kullanıcı parolaları yapılandırma kaynağı çağrısıyla eklemek [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) içinde `Startup` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="62563-187">Add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="62563-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="62563-188">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="62563-189">[ASP.NET çekirdek yapılandırma API'si](xref:fundamentals/configuration/index) gizli Yöneticisi gizli erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="62563-189">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="62563-190">Projeniz .NET Framework hedefliyorsa, yükleme [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="62563-190">If your project targets the .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="62563-191">Proje çağırdığında ASP.NET Core 2.0 veya sonraki sürümlerde, kullanıcı parolaları yapılandırma kaynağı otomatik olarak geliştirme modunda eklenir [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) önceden yapılandırılmış varsayılan ana bilgisayar yeni bir örneğini başlatılamadı.</span><span class="sxs-lookup"><span data-stu-id="62563-191">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="62563-192">`CreateDefaultBuilder` çağrıları [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) zaman [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) olan [geliştirme](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span><span class="sxs-lookup"><span data-stu-id="62563-192">`CreateDefaultBuilder` calls [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) when the [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) is [Development](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):</span></span>

<span data-ttu-id="62563-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span><span class="sxs-lookup"><span data-stu-id="62563-193">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]</span></span>

<span data-ttu-id="62563-194">Zaman `CreateDefaultBuilder` değil ana bilgisayar oluşturma sırasında çağrılır, kullanıcı parolaları yapılandırma kaynağı çağrısıyla eklemek [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) içinde `Startup` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="62563-194">When `CreateDefaultBuilder` isn't called during host construction, add the user secrets configuration source with a call to [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) in the `Startup` constructor:</span></span>

<span data-ttu-id="62563-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="62563-195">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="62563-196">Kullanıcı parolaları, aracılığıyla alınabilir `Configuration` API'si:</span><span class="sxs-lookup"><span data-stu-id="62563-196">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="62563-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="62563-197">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="62563-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="62563-198">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="62563-199">Gizli anahtarlarla dize değiştirme</span><span class="sxs-lookup"><span data-stu-id="62563-199">String replacement with secrets</span></span>

<span data-ttu-id="62563-200">Parolaları düz metin olarak depolanması risklidir.</span><span class="sxs-lookup"><span data-stu-id="62563-200">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="62563-201">Örneğin, bir veritabanı bağlantı dizesi içinde depolanan *appsettings.json* belirtilen kullanıcı için bir parola içerebilir:</span><span class="sxs-lookup"><span data-stu-id="62563-201">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="62563-202">Parolayı gizlilik olarak depolamak daha güvenli bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="62563-202">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="62563-203">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="62563-203">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="62563-204">Parolayı Değiştir *appsettings.json* bir yer tutucu.</span><span class="sxs-lookup"><span data-stu-id="62563-204">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="62563-205">Aşağıdaki örnekte, `{0}` form yer tutucu olarak kullanılan bir [bileşik biçim dizesi](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span><span class="sxs-lookup"><span data-stu-id="62563-205">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="62563-206">Parolanın değeri bağlantı dizesini tamamlamak için yer tutucu yerleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="62563-206">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="62563-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="62563-207">[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="62563-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="62563-208">[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="62563-209">Gizli listesi</span><span class="sxs-lookup"><span data-stu-id="62563-209">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="62563-210">Hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası bulunmaktadır:</span><span class="sxs-lookup"><span data-stu-id="62563-210">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="62563-211">Şu çıktı görünür:</span><span class="sxs-lookup"><span data-stu-id="62563-211">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="62563-212">Önceki örnekte, iki nokta üst üste anahtar adlarını nesne hiyerarşi içinde gösterir *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="62563-212">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="62563-213">Tek bir gizlilik Kaldır</span><span class="sxs-lookup"><span data-stu-id="62563-213">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="62563-214">Hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası bulunmaktadır:</span><span class="sxs-lookup"><span data-stu-id="62563-214">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="62563-215">Uygulamanın *secrets.json* dosyası ile ilişkili anahtar-değer çifti kaldırmak için değiştirildi `MoviesConnectionString` anahtarı:</span><span class="sxs-lookup"><span data-stu-id="62563-215">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="62563-216">Çalışan `dotnet user-secrets list` aşağıdaki ileti görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="62563-216">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="62563-217">Tüm gizli Kaldır</span><span class="sxs-lookup"><span data-stu-id="62563-217">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="62563-218">Hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası bulunmaktadır:</span><span class="sxs-lookup"><span data-stu-id="62563-218">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="62563-219">Uygulama için tüm kullanıcı parolaları gelen silinmiş *secrets.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="62563-219">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="62563-220">Çalışan `dotnet user-secrets list` aşağıdaki ileti görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="62563-220">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="62563-221">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="62563-221">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
