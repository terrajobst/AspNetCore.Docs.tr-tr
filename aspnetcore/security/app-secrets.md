---
title: Güvenli Depolama Uygulama sırrı ASP.NET Core geliştirme
author: rick-anderson
description: Bir ASP.NET Core uygulama geliştirme sırasında uygulama sırrı olarak hassas bilgilerini depolamak ve almak öğrenin.
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: 4db09d3d41b705597f93d05af91077f2b9236b7e
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="acbf7-103">Güvenli Depolama Uygulama sırrı ASP.NET Core geliştirme</span><span class="sxs-lookup"><span data-stu-id="acbf7-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

<span data-ttu-id="acbf7-104">Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), ve [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="acbf7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="acbf7-105">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="acbf7-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="acbf7-106">[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="acbf7-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
::: moniker-end

<span data-ttu-id="acbf7-107">Bu belge, depolamak ve almak için kullanılan hassas verileri ASP.NET Core uygulama geliştirme sırasında teknikleri açıklar.</span><span class="sxs-lookup"><span data-stu-id="acbf7-107">This document explains techniques for storing and retrieving sensitive data during the development of an ASP.NET Core app.</span></span> <span data-ttu-id="acbf7-108">Kaynak kodunda parolalar ve diğer hassas verileri asla saklamalısınız ve geliştirme üretim gizlilikleri kullanın veya modu test döndürmemelidir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-108">You should never store passwords or other sensitive data in source code, and you shouldn't use production secrets in development or test mode.</span></span> <span data-ttu-id="acbf7-109">Depolama ve Azure test ve üretim parolaları ile korumak [Azure anahtar kasası yapılandırma sağlayıcısı](xref:security/key-vault-configuration).</span><span class="sxs-lookup"><span data-stu-id="acbf7-109">You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="acbf7-110">Ortam değişkenleri</span><span class="sxs-lookup"><span data-stu-id="acbf7-110">Environment variables</span></span>

<span data-ttu-id="acbf7-111">Ortam değişkenleri, uygulama gizli kod veya yerel yapılandırma dosyalarını depolanmasını önlemek için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="acbf7-111">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="acbf7-112">Ortam değişkenleri tüm daha önce belirtilen yapılandırma kaynakları için yapılandırma değerleri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="acbf7-112">Environment variables override configuration values for all previously specified configuration sources.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="acbf7-113">Ortam değişkeni değerlerini okuma çağırarak yapılandırma [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) içinde `Startup` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="acbf7-113">Configure the reading of environment variable values by calling [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) in the `Startup` constructor:</span></span>

<span data-ttu-id="acbf7-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="acbf7-114">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]</span></span>
::: moniker-end

<span data-ttu-id="acbf7-115">Bir ASP.NET Core web uygulaması, göz önünde bulundurun **tek tek kullanıcı hesaplarını** güvenlik etkindir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-115">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="acbf7-116">Bir varsayılan veritabanı bağlantı dizesi projesinin dahil *appsettings.json* anahtar dosyasıyla `DefaultConnection`.</span><span class="sxs-lookup"><span data-stu-id="acbf7-116">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="acbf7-117">Varsayılan bağlantı dizesini kullanıcı modunda çalışır ve bir parola gerektirmez LocalDB ' dir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-117">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="acbf7-118">Uygulama dağıtımı sırasında `DefaultConnection` anahtar değeri ile bir ortam değişkeninin değeri geçersiz.</span><span class="sxs-lookup"><span data-stu-id="acbf7-118">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="acbf7-119">Ortam değişkeni hassas kimlik bilgileriyle tam bağlantı dizesi depolayabilir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-119">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="acbf7-120">Ortam değişkenleri genellikle düz ve şifresiz metin olarak depolanır.</span><span class="sxs-lookup"><span data-stu-id="acbf7-120">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="acbf7-121">Makine ya da işlem aşılırsa, ortam değişkenleri Güvenilmeyen taraflar tarafından erişilebilir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-121">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="acbf7-122">Kullanıcı parolaları açığa çıkmasını önlemek için ek önlemler gerekli olabilir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-122">Additional measures to prevent disclosure of user secrets may be required.</span></span>

## <a name="secret-manager"></a><span data-ttu-id="acbf7-123">Parola Yöneticisi</span><span class="sxs-lookup"><span data-stu-id="acbf7-123">Secret Manager</span></span>

<span data-ttu-id="acbf7-124">Gizli Yöneticisi aracını hassas verileri ASP.NET Core projesinde geliştirme sırasında depolar.</span><span class="sxs-lookup"><span data-stu-id="acbf7-124">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="acbf7-125">Bu bağlamda bir gizli verilerin bir uygulama gizli anahtarı parçasıdır.</span><span class="sxs-lookup"><span data-stu-id="acbf7-125">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="acbf7-126">Uygulama parolaları proje ağacından ayrı bir konumda depolanır.</span><span class="sxs-lookup"><span data-stu-id="acbf7-126">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="acbf7-127">Uygulama parolaları belirli bir projeyle ilişkili veya birkaç projeler arasında paylaşılan.</span><span class="sxs-lookup"><span data-stu-id="acbf7-127">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="acbf7-128">Uygulama parolaları kaynak denetimine iade değil.</span><span class="sxs-lookup"><span data-stu-id="acbf7-128">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="acbf7-129">Gizli Yöneticisi aracını depolanan parolaları şifrelemek değil ve bir güvenilen deposu olarak değerlendirilmesi gerekir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-129">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="acbf7-130">Yalnızca geliştirme amacıyla kullanılır.</span><span class="sxs-lookup"><span data-stu-id="acbf7-130">It's for development purposes only.</span></span> <span data-ttu-id="acbf7-131">Anahtarları ve değerleri, kullanıcı profili dizini bir JSON yapılandırma dosyasında depolanır.</span><span class="sxs-lookup"><span data-stu-id="acbf7-131">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="acbf7-132">Parola Yöneticisi aracını nasıl çalışır</span><span class="sxs-lookup"><span data-stu-id="acbf7-132">How the Secret Manager tool works</span></span>

<span data-ttu-id="acbf7-133">Parola Yöneticisi aracını hemen nerede ve nasıl değerleri saklanır gibi uygulama ayrıntılarını soyutlar.</span><span class="sxs-lookup"><span data-stu-id="acbf7-133">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="acbf7-134">Bu uygulama ayrıntılarını bilmeden aracını kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="acbf7-134">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="acbf7-135">İçinde depolanan değerlerden bir [JSON](https://json.org/) yerel makinede bir sistem korumalı kullanıcı profili klasöründeki yapılandırma dosyası:</span><span class="sxs-lookup"><span data-stu-id="acbf7-135">The values are stored in a [JSON](https://json.org/) configuration file in a system-protected user profile folder on the local machine:</span></span>

* <span data-ttu-id="acbf7-136">Windows: `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`</span><span class="sxs-lookup"><span data-stu-id="acbf7-136">Windows: `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`</span></span>
* <span data-ttu-id="acbf7-137">Linux & macOS: `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`</span><span class="sxs-lookup"><span data-stu-id="acbf7-137">Linux & macOS: `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`</span></span>

<span data-ttu-id="acbf7-138">Önceki dosya yolları ve Değiştir `<user_secrets_id>` ile `UserSecretsId` belirtilen değer *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="acbf7-138">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="acbf7-139">Konum veya gizli anahtarı Yöneticisi aracıyla kaydedilen verilerin biçimi bağlıdır kod yazmayın.</span><span class="sxs-lookup"><span data-stu-id="acbf7-139">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="acbf7-140">Bu uygulama ayrıntılarını değişebilir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-140">These implementation details may change.</span></span> <span data-ttu-id="acbf7-141">Örneğin, gizli değerleri şifreli değildir, ancak gelecekte olabilir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-141">For example, the secret values aren't encrypted, but could be in the future.</span></span>

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a><span data-ttu-id="acbf7-142">Parola Yöneticisi aracını yükleyin</span><span class="sxs-lookup"><span data-stu-id="acbf7-142">Install the Secret Manager tool</span></span>

<span data-ttu-id="acbf7-143">.NET Core SDK 2.1 içinde .NET Core CLI ile gizli Yöneticisi aracını paketlenebilir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-143">The Secret Manager tool is bundled with the .NET Core CLI in .NET Core SDK 2.1.</span></span> <span data-ttu-id="acbf7-144">Aracı yükleme ve önceki sürümleri, .NET Core SDK 2.0 için gereklidir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-144">For .NET Core SDK 2.0 and earlier, tool installation is necessary.</span></span>

<span data-ttu-id="acbf7-145">Yükleme [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core projenizdeki NuGet paketi:</span><span class="sxs-lookup"><span data-stu-id="acbf7-145">Install the [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet package in your ASP.NET Core project:</span></span>

<span data-ttu-id="acbf7-146">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="acbf7-146">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]</span></span>

<span data-ttu-id="acbf7-147">Aracı yüklemesini doğrulamak için bir komut kabuğuna şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="acbf7-147">Execute the following command in a command shell to validate the tool installation:</span></span>

```console
dotnet user-secrets -h
```

<span data-ttu-id="acbf7-148">Gizli Yöneticisi aracını örnek kullanım, seçenekleri ve komut Yardımı görüntüler:</span><span class="sxs-lookup"><span data-stu-id="acbf7-148">The Secret Manager tool displays sample usage, options, and command help:</span></span>

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
> <span data-ttu-id="acbf7-149">Aynı dizinde olmalıdır *.csproj* tanımlanan araçları çalıştırmak için dosya *.csproj* dosyanın `DotNetCliToolReference` öğeleri.</span><span class="sxs-lookup"><span data-stu-id="acbf7-149">You must be in the same directory as the *.csproj* file to run tools defined in the *.csproj* file's `DotNetCliToolReference` elements.</span></span>
::: moniker-end

## <a name="set-a-secret"></a><span data-ttu-id="acbf7-150">Bir parola ayarlama</span><span class="sxs-lookup"><span data-stu-id="acbf7-150">Set a secret</span></span>

<span data-ttu-id="acbf7-151">Projeye özgü yapılandırma ayarlarını kullanıcı profilinizde depolanan gizli Manager aracı çalışır.</span><span class="sxs-lookup"><span data-stu-id="acbf7-151">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span> <span data-ttu-id="acbf7-152">Kullanıcı parolaları kullanmak üzere tanımladığınız bir `UserSecretsId` öğesi içinde bir `PropertyGroup` , *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="acbf7-152">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="acbf7-153">Değeri `UserSecretsId` isteğe bağlıdır, ancak projeye benzersizdir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-153">The value of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="acbf7-154">Geliştiriciler genellikle oluşturmak için bir GUID `UserSecretsId`.</span><span class="sxs-lookup"><span data-stu-id="acbf7-154">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="acbf7-155">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="acbf7-155">[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="acbf7-156">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span><span class="sxs-lookup"><span data-stu-id="acbf7-156">[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]</span></span>
::: moniker-end

> [!TIP]
> <span data-ttu-id="acbf7-157">Visual Studio'da, Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **kullanıcı parolaları yönetme** ve bağlam menüsünden.</span><span class="sxs-lookup"><span data-stu-id="acbf7-157">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="acbf7-158">Bu hareketi ekler bir `UserSecretsId` öğesi, bir GUID ile çok doldurulmuş *.csproj* dosya.</span><span class="sxs-lookup"><span data-stu-id="acbf7-158">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span> <span data-ttu-id="acbf7-159">Visual Studio açılır bir *secrets.json* dosyasını bir metin düzenleyicisinde.</span><span class="sxs-lookup"><span data-stu-id="acbf7-159">Visual Studio opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="acbf7-160">Değiştir *secrets.json* depolanması için anahtar-değer çiftleri ile.</span><span class="sxs-lookup"><span data-stu-id="acbf7-160">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="acbf7-161">Örneğin: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span><span class="sxs-lookup"><span data-stu-id="acbf7-161">For example: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]</span></span>

<span data-ttu-id="acbf7-162">Bir anahtarı ve değeri oluşan bir uygulama gizli anahtarı tanımlayın.</span><span class="sxs-lookup"><span data-stu-id="acbf7-162">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="acbf7-163">Gizli projenin ilişkili olmadığından `UserSecretsId` değeri.</span><span class="sxs-lookup"><span data-stu-id="acbf7-163">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="acbf7-164">Örneğin, hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası bulunmaktadır:</span><span class="sxs-lookup"><span data-stu-id="acbf7-164">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="acbf7-165">Önceki örnekte, iki nokta üst üste gösterir `Movies` bir nesne hazır olan bir `ServiceApiKey` özelliği.</span><span class="sxs-lookup"><span data-stu-id="acbf7-165">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="acbf7-166">Gizli Manager aracı diğer dizinlerden çok kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-166">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="acbf7-167">Kullanım `--project` seçeneği, dosya sistemi yolu sağlamak için *.csproj* dosya yok.</span><span class="sxs-lookup"><span data-stu-id="acbf7-167">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="acbf7-168">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="acbf7-168">For example:</span></span>

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="acbf7-169">Birden çok gizli ayarlayın</span><span class="sxs-lookup"><span data-stu-id="acbf7-169">Set multiple secrets</span></span>

<span data-ttu-id="acbf7-170">JSON olarak cmdlet'ine yönelterek gizli toplu ayarlanabilir `set` komutu.</span><span class="sxs-lookup"><span data-stu-id="acbf7-170">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="acbf7-171">Aşağıdaki örnekte, *input.json* dosyanın içeriğini yöneltilen için `set` Windows komutunu:</span><span class="sxs-lookup"><span data-stu-id="acbf7-171">In the following example, the *input.json* file's contents are piped to the `set` command on Windows:</span></span>

```console
type .\input.json | dotnet user-secrets set
```

<span data-ttu-id="acbf7-172">MacOS ve Linux üzerinde aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="acbf7-172">Use the following command on macOS and Linux:</span></span>

```console
cat ./input.json | dotnet user-secrets set
```

## <a name="access-a-secret"></a><span data-ttu-id="acbf7-173">Bir gizli anahtar erişimi</span><span class="sxs-lookup"><span data-stu-id="acbf7-173">Access a secret</span></span>

<span data-ttu-id="acbf7-174">[ASP.NET çekirdek yapılandırma API'si](xref:fundamentals/configuration/index) gizli Yöneticisi gizli erişim sağlar.</span><span class="sxs-lookup"><span data-stu-id="acbf7-174">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span> <span data-ttu-id="acbf7-175">.NET Core hedefleme 1.x veya .NET Framework yüklerseniz [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketi.</span><span class="sxs-lookup"><span data-stu-id="acbf7-175">If targeting .NET Core 1.x or .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="acbf7-176">Kullanıcı parolaları yapılandırma kaynağı'na ekleyin `Startup` Oluşturucusu:</span><span class="sxs-lookup"><span data-stu-id="acbf7-176">Add the user secrets configuration source to the `Startup` constructor:</span></span>

<span data-ttu-id="acbf7-177">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span><span class="sxs-lookup"><span data-stu-id="acbf7-177">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]</span></span>
::: moniker-end

<span data-ttu-id="acbf7-178">Kullanıcı parolaları, aracılığıyla alınabilir `Configuration` API'si:</span><span class="sxs-lookup"><span data-stu-id="acbf7-178">User secrets can be retrieved via the `Configuration` API:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="acbf7-179">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span><span class="sxs-lookup"><span data-stu-id="acbf7-179">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="acbf7-180">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span><span class="sxs-lookup"><span data-stu-id="acbf7-180">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]</span></span>
::: moniker-end

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="acbf7-181">Gizli anahtarlarla dize değiştirme</span><span class="sxs-lookup"><span data-stu-id="acbf7-181">String replacement with secrets</span></span>

<span data-ttu-id="acbf7-182">Parolaları düz metin olarak depolanması risklidir.</span><span class="sxs-lookup"><span data-stu-id="acbf7-182">Storing passwords in plain text is risky.</span></span> <span data-ttu-id="acbf7-183">Örneğin, bir veritabanı bağlantı dizesi içinde depolanan *appsettings.json* belirtilen kullanıcı için bir parola içerebilir:</span><span class="sxs-lookup"><span data-stu-id="acbf7-183">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="acbf7-184">Parolayı gizlilik olarak depolamak daha güvenli bir yaklaşımdır.</span><span class="sxs-lookup"><span data-stu-id="acbf7-184">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="acbf7-185">Örneğin:</span><span class="sxs-lookup"><span data-stu-id="acbf7-185">For example:</span></span>

```console
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="acbf7-186">Parolayı Değiştir *appsettings.json* bir yer tutucu.</span><span class="sxs-lookup"><span data-stu-id="acbf7-186">Replace the password in *appsettings.json* with a placeholder.</span></span> <span data-ttu-id="acbf7-187">Aşağıdaki örnekte, `{0}` form yer tutucu olarak kullanılan bir [bileşik biçim dizesi](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span><span class="sxs-lookup"><span data-stu-id="acbf7-187">In the following example, `{0}` is used as the placeholder to form a [Composite Format String](/dotnet/standard/base-types/composite-formatting#composite-format-string).</span></span>

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="acbf7-188">Parolanın değeri bağlantı dizesini tamamlamak için yer tutucu yerleştirilebilir:</span><span class="sxs-lookup"><span data-stu-id="acbf7-188">The secret's value can be injected into the placeholder to complete the connection string:</span></span>

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="acbf7-189">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span><span class="sxs-lookup"><span data-stu-id="acbf7-189">[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
<span data-ttu-id="acbf7-190">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span><span class="sxs-lookup"><span data-stu-id="acbf7-190">[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]</span></span>
::: moniker-end

## <a name="list-the-secrets"></a><span data-ttu-id="acbf7-191">Gizli listesi</span><span class="sxs-lookup"><span data-stu-id="acbf7-191">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="acbf7-192">Hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası bulunmaktadır:</span><span class="sxs-lookup"><span data-stu-id="acbf7-192">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets list
```

<span data-ttu-id="acbf7-193">Şu çıktı görünür:</span><span class="sxs-lookup"><span data-stu-id="acbf7-193">The following output appears:</span></span>

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

<span data-ttu-id="acbf7-194">Önceki örnekte, iki nokta üst üste anahtar adlarını nesne hiyerarşi içinde gösterir *secrets.json*.</span><span class="sxs-lookup"><span data-stu-id="acbf7-194">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="acbf7-195">Tek bir gizlilik Kaldır</span><span class="sxs-lookup"><span data-stu-id="acbf7-195">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="acbf7-196">Hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası bulunmaktadır:</span><span class="sxs-lookup"><span data-stu-id="acbf7-196">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="acbf7-197">Uygulamanın *secrets.json* dosyası ile ilişkili anahtar-değer çifti kaldırmak için değiştirildi `MoviesConnectionString` anahtarı:</span><span class="sxs-lookup"><span data-stu-id="acbf7-197">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="acbf7-198">Çalışan `dotnet user-secrets list` aşağıdaki ileti görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="acbf7-198">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="acbf7-199">Tüm gizli Kaldır</span><span class="sxs-lookup"><span data-stu-id="acbf7-199">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="acbf7-200">Hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası bulunmaktadır:</span><span class="sxs-lookup"><span data-stu-id="acbf7-200">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```console
dotnet user-secrets clear
```

<span data-ttu-id="acbf7-201">Uygulama için tüm kullanıcı parolaları gelen silinmiş *secrets.json* dosyası:</span><span class="sxs-lookup"><span data-stu-id="acbf7-201">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="acbf7-202">Çalışan `dotnet user-secrets list` aşağıdaki ileti görüntülenir:</span><span class="sxs-lookup"><span data-stu-id="acbf7-202">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="acbf7-203">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="acbf7-203">Additional resources</span></span>

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>