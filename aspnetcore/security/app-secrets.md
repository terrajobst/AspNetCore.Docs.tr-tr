---
title: Güvenli Depolama Uygulama sırrı ASP.NET Core geliştirme
author: rick-anderson
description: Bir ASP.NET Core uygulama geliştirme sırasında uygulama sırrı olarak hassas bilgilerini depolamak ve almak öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/21/2018
uid: security/app-secrets
ms.openlocfilehash: d3b2de1a17012986ef8dea7aaf8636dd35d10fa1
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314181"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Güvenli Depolama Uygulama sırrı ASP.NET Core geliştirme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), ve [Scott Addie](https://github.com/scottaddie)

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

Bu belge, depolamak ve almak için kullanılan hassas verileri ASP.NET Core uygulama geliştirme sırasında teknikleri açıklar. Kaynak kodunda parolalar ve diğer hassas verileri asla saklamalısınız ve geliştirme üretim gizlilikleri kullanın veya modu test döndürmemelidir. Depolama ve Azure test ve üretim parolaları ile korumak [Azure anahtar kasası yapılandırma sağlayıcısı](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Ortam değişkenleri

Ortam değişkenleri, uygulama gizli kod veya yerel yapılandırma dosyalarını depolanmasını önlemek için kullanılır. Ortam değişkenleri tüm daha önce belirtilen yapılandırma kaynakları için yapılandırma değerleri geçersiz.

::: moniker range="<= aspnetcore-1.1"
Ortam değişkeni değerlerini okuma çağırarak yapılandırma [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) içinde `Startup` Oluşturucusu:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

Bir ASP.NET Core web uygulaması, göz önünde bulundurun **tek tek kullanıcı hesaplarını** güvenlik etkindir. Bir varsayılan veritabanı bağlantı dizesi projesinin dahil *appsettings.json* anahtar dosyasıyla `DefaultConnection`. Varsayılan bağlantı dizesini kullanıcı modunda çalışır ve bir parola gerektirmez LocalDB ' dir. Uygulama dağıtımı sırasında `DefaultConnection` anahtar değeri ile bir ortam değişkeninin değeri geçersiz. Ortam değişkeni hassas kimlik bilgileriyle tam bağlantı dizesi depolayabilir.

> [!WARNING]
> Ortam değişkenleri genellikle düz ve şifresiz metin olarak depolanır. Makine ya da işlem aşılırsa, ortam değişkenleri Güvenilmeyen taraflar tarafından erişilebilir. Kullanıcı parolaları açığa çıkmasını önlemek için ek önlemler gerekli olabilir.

## <a name="secret-manager"></a>Parola Yöneticisi

Gizli Yöneticisi aracını hassas verileri ASP.NET Core projesinde geliştirme sırasında depolar. Bu bağlamda bir gizli verilerin bir uygulama gizli anahtarı parçasıdır. Uygulama parolaları proje ağacından ayrı bir konumda depolanır. Uygulama parolaları belirli bir projeyle ilişkili veya birkaç projeler arasında paylaşılan. Uygulama parolaları kaynak denetimine iade değil.

> [!WARNING]
> Gizli Yöneticisi aracını depolanan parolaları şifrelemek değil ve bir güvenilen deposu olarak değerlendirilmesi gerekir. Yalnızca geliştirme amacıyla kullanılır. Anahtarları ve değerleri, kullanıcı profili dizini bir JSON yapılandırma dosyasında depolanır.

## <a name="how-the-secret-manager-tool-works"></a>Parola Yöneticisi aracını nasıl çalışır

Parola Yöneticisi aracını hemen nerede ve nasıl değerleri saklanır gibi uygulama ayrıntılarını soyutlar. Bu uygulama ayrıntılarını bilmeden aracını kullanabilirsiniz. Değerleri bir sistem korumalı kullanıcı profili klasörü JSON yapılandırma dosyasında yerel makine üzerinde saklanır:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Dosya sistemi yolu:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Dosya sistemi yolu:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Dosya sistemi yolu:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

Önceki dosya yolları ve Değiştir `<user_secrets_id>` ile `UserSecretsId` belirtilen değer *.csproj* dosya.

Konum veya gizli anahtarı Yöneticisi aracıyla kaydedilen verilerin biçimi bağlıdır kod yazmayın. Bu uygulama ayrıntılarını değişebilir. Örneğin, gizli değerleri şifreli değildir, ancak gelecekte olabilir.

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>Parola Yöneticisi aracını yükleyin

.NET Core SDK 2.1.300 itibariyle .NET Core CLI ile gizli Yöneticisi aracını paketlenebilir. Aracı yükleme 2.1.300 önce .NET Core SDK sürümleri için gereklidir.

> [!TIP]
> Çalıştırma `dotnet --version` yüklü .NET Core SDK sürüm numarasını görmek için bir komut kabuğu'ndan.

.NET Core kullanılan SDK aracı içeriyorsa, bir uyarı görüntülenir:

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Yükleme [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core projenizdeki NuGet paketi. Örneğin:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

Aracı yüklemesini doğrulamak için bir komut kabuğuna şu komutu çalıştırın:

```console
dotnet user-secrets -h
```

Gizli Yöneticisi aracını örnek kullanım, seçenekleri ve komut Yardımı görüntüler:

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
> Aynı dizinde olmalıdır *.csproj* tanımlanan araçları çalıştırmak için dosya *.csproj* dosyanın `DotNetCliToolReference` öğeleri.
::: moniker-end

## <a name="set-a-secret"></a>Bir parola ayarlama

Projeye özgü yapılandırma ayarlarını kullanıcı profilinizde depolanan gizli Manager aracı çalışır. Kullanıcı parolaları kullanmak üzere tanımladığınız bir `UserSecretsId` öğesi içinde bir `PropertyGroup` , *.csproj* dosya. Değeri `UserSecretsId` isteğe bağlıdır, ancak projeye benzersizdir. Geliştiriciler genellikle oluşturmak için bir GUID `UserSecretsId`.

::: moniker range="<= aspnetcore-1.1"
[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end

> [!TIP]
> Visual Studio'da, Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **kullanıcı parolaları yönetme** ve bağlam menüsünden. Bu hareketi ekler bir `UserSecretsId` öğesi, bir GUID ile çok doldurulmuş *.csproj* dosya. Visual Studio açılır bir *secrets.json* dosyasını bir metin düzenleyicisinde. Değiştir *secrets.json* depolanması için anahtar-değer çiftleri ile. Örneğin: [!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file.md)]

Bir anahtarı ve değeri oluşan bir uygulama gizli anahtarı tanımlayın. Gizli projenin ilişkili olmadığından `UserSecretsId` değeri. Örneğin, hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası bulunmaktadır:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

Önceki örnekte, iki nokta üst üste gösterir `Movies` bir nesne hazır olan bir `ServiceApiKey` özelliği.

Gizli Manager aracı diğer dizinlerden çok kullanılabilir. Kullanım `--project` seçeneği, dosya sistemi yolu sağlamak için *.csproj* dosya yok. Örneğin:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

## <a name="set-multiple-secrets"></a>Birden çok gizli ayarlayın

JSON olarak cmdlet'ine yönelterek gizli toplu ayarlanabilir `set` komutu. Aşağıdaki örnekte, *input.json* dosyanın içeriğini yöneltilen için `set` komutu.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Bir komut kabuğu'nu açın ve aşağıdaki komutu yürütün:

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Bir komut kabuğu'nu açın ve aşağıdaki komutu yürütün:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Bir komut kabuğu'nu açın ve aşağıdaki komutu yürütün:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Bir gizli anahtar erişimi

::: moniker range="<= aspnetcore-1.1"
[ASP.NET çekirdek yapılandırma API'si](xref:fundamentals/configuration/index) gizli Yöneticisi gizli erişim sağlar. Yükleme [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketi.

Kullanıcı parolaları yapılandırma kaynağı çağrısıyla eklemek [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) içinde `Startup` Oluşturucusu:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[ASP.NET çekirdek yapılandırma API'si](xref:fundamentals/configuration/index) gizli Yöneticisi gizli erişim sağlar. Projeniz .NET Framework hedefliyorsa, yükleme [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketi.

Proje çağırdığında ASP.NET Core 2.0 veya sonraki sürümlerde, kullanıcı parolaları yapılandırma kaynağı otomatik olarak geliştirme modunda eklenir [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) önceden yapılandırılmış varsayılan ana bilgisayar yeni bir örneğini başlatılamadı. `CreateDefaultBuilder` çağrıları [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) zaman [EnvironmentName](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.environmentname) olan [geliştirme](/dotnet/api/microsoft.aspnetcore.hosting.environmentname.development):

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

Zaman `CreateDefaultBuilder` değil ana bilgisayar oluşturma sırasında çağrılır, kullanıcı parolaları yapılandırma kaynağı çağrısıyla eklemek [AddUserSecrets](/dotnet/api/microsoft.extensions.configuration.usersecretsconfigurationextensions.addusersecrets) içinde `Startup` Oluşturucusu:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

Kullanıcı parolaları, aracılığıyla alınabilir `Configuration` API'si:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>Gizli anahtarlarla dize değiştirme

Parolaları düz metin olarak depolanması güvenli değil. Örneğin, bir veritabanı bağlantı dizesi içinde depolanan *appsettings.json* belirtilen kullanıcı için bir parola içerebilir:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Parolayı gizlilik olarak depolamak daha güvenli bir yaklaşımdır. Örneğin:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Kaldırma `Password` anahtar-değer çifti bağlantı dizesinden *appsettings.json*. Örneğin:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

Parolanın değer ayarlanabilir bir [SqlConnectionStringBuilder](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder) nesnenin [parola](/dotnet/api/system.data.sqlclient.sqlconnectionstringbuilder.password) bağlantı dizesini tamamlamak için özellik:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]
::: moniker-end

## <a name="list-the-secrets"></a>Gizli listesi

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası bulunmaktadır:

```console
dotnet user-secrets list
```

Şu çıktı görünür:

```console
Movies:ServiceApiKey = 12345
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
```

Önceki örnekte, iki nokta üst üste anahtar adlarını nesne hiyerarşi içinde gösterir *secrets.json*.

## <a name="remove-a-single-secret"></a>Tek bir gizlilik Kaldır

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası bulunmaktadır:

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

Uygulamanın *secrets.json* dosyası ile ilişkili anahtar-değer çifti kaldırmak için değiştirildi `MoviesConnectionString` anahtarı:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

Çalışan `dotnet user-secrets list` aşağıdaki ileti görüntülenir:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Tüm gizli Kaldır

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası bulunmaktadır:

```console
dotnet user-secrets clear
```

Uygulama için tüm kullanıcı parolaları gelen silinmiş *secrets.json* dosyası:

```json
{}
```

Çalışan `dotnet user-secrets list` aşağıdaki ileti görüntülenir:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
