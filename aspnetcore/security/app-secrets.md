---
title: ASP.NET core'da geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması
author: rick-anderson
description: ASP.NET Core uygulaması geliştirme sırasında uygulama gizli diziler olarak hassas bilgilerini depolamak ve almak öğrenin.
ms.author: scaddie
ms.custom: mvc
ms.date: 03/13/2019
uid: security/app-secrets
ms.openlocfilehash: 18313f8284e81d196cbe786f494a607ee97a299f
ms.sourcegitcommit: 3e9e1f6d572947e15347e818f769e27dea56b648
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2019
ms.locfileid: "58750981"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>ASP.NET core'da geliştirmede uygulama gizli anahtarlarının güvenli bir şekilde depolanması

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), ve [Scott Addie](https://github.com/scottaddie)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Bu belge, depolamak ve ASP.NET Core uygulaması geliştirme sırasında hassas verileri almak için teknikleri açıklar. Asla kaynak kodunda parola ya da diğer hassas verileri depolayın. Üretim gizli dizileri olmamalıdır kullanılabilir geliştirme veya test için. Depolama ve Azure test ve üretim parolalarını ile korumak [Azure Key Vault yapılandırma sağlayıcısı](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Ortam değişkenleri

Ortam değişkenleri, uygulama gizli anahtarlarının kod veya yerel yapılandırma dosyaları depolama önlemek için kullanılır. Ortam değişkenleri tüm daha önce belirtilen yapılandırma kaynakları için yapılandırma değerlerini geçersiz kılar.

::: moniker range="<= aspnetcore-1.1"

Ortam değişkeni değerlerini okunmasını çağırarak yapılandırma <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> içinde `Startup` Oluşturucusu:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

ASP.NET Core web uygulaması, göz önünde bulundurun **bireysel kullanıcı hesapları** güvenlik etkin. Varsayılan veritabanı bağlantı dizesi projesinin dahil *appsettings.json* anahtar dosyasıyla `DefaultConnection`. Varsayılan bağlantı dizesini kullanıcı modunda çalışır ve bir parola gerektirmez LocalDB ' dir. Uygulama dağıtımı sırasında `DefaultConnection` anahtar değeri ile bir ortam değişken değerini geçersiz kılınabilir. Ortam değişkeni hassas kimlik bilgileriyle tam bağlantı dizesi depolayabilir.

> [!WARNING]
> Ortam değişkenleri genellikle düz ve şifresiz metin olarak depolanır. İşlem ve makine tehlikedeyse, ortam değişkenleri Güvenilmeyen taraflar tarafından erişilebilir. Kullanıcı gizli dizilerinin açığa çıkmasını önlemek amacıyla ek ölçüler gerekli olabilir.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>Gizli dizi Yöneticisi

Gizli dizi Yöneticisi Aracı, bir ASP.NET Core projesi geliştirilmesi sırasında hassas verileri depolar. Bu bağlamda bir hassas verileri uygulama gizli anahtarı bir parçasıdır. Uygulama gizli anahtarlarının proje ağacı ayrı bir konumda depolanır. Uygulama gizli dizileri belirli bir projeyle ilişkili veya birkaç projede paylaşılan. Uygulama gizli anahtarlarının kaynak denetimine denetlenmez.

> [!WARNING]
> Gizli dizi Yöneticisi aracını depolanan gizli dizileri şifrelemez ve güvenilir bir deposu olarak değerlendirilmesi gerekir. Bu, yalnızca geliştirme amaçları içindir. Anahtarları ve değerleri, kullanıcı profili dizini bir JSON yapılandırma dosyasında depolanır.

## <a name="how-the-secret-manager-tool-works"></a>Gizli dizi Yöneticisi aracını nasıl çalışır?

Gizli dizi Yöneticisi aracını nerede ve nasıl depolanacağını ve gibi uygulama ayrıntılarını dengelediği. Bu uygulama ayrıntılarını bilmeden aracını kullanabilirsiniz. Değerleri, yerel makinede bulunan JSON yapılandırma dosyası bir sistem korumalı kullanıcı profili klasöründe depolanır:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Dosya sistemi yolu:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[Linux / macOS](#tab/linux+macos)

Dosya sistemi yolu:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

Önceki dosya yolları ve yerine `<user_secrets_id>` ile `UserSecretsId` belirtilen değeri *.csproj* dosya.

Konum veya gizli dizi Yöneticisi Aracı ile kaydedilen verilerin biçimi bağımlı kod yazmayın. Bu uygulama ayrıntılarını değişebilir. Örneğin, gizli değerleri şifreli değildir, ancak gelecekte olabilir.

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a>Gizli dizi Yöneticisi aracını yükleyin

Gizli dizi Yöneticisi Aracı ile .NET Core SDK'sı 2.1.300 .NET Core CLI ile birlikte gelen veya üzeri. Aracı yükleme 2.1.300 önce .NET Core SDK sürümleri için gereklidir.

> [!TIP]
> Çalıştırma `dotnet --version` yüklü .NET Core SDK'sı sürüm numarasını görmek için bir komut kabuğu'ndan.

.NET Core SDK kullanılan araç içeriyorsa, bir uyarı görüntülenir:

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

Yükleme [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core projenizdeki NuGet paketi. Örneğin:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

Aracı yüklemesini doğrulamak için bir komut kabuğuna şu komutu çalıştırın:

```console
dotnet user-secrets -h
```

Gizli dizi Yöneticisi aracını örnek kullanımı, Seçenekler ve komut Yardımı görüntüler:

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

## <a name="enable-secret-storage"></a>Gizli depolama etkinleştir

Projeye özgü yapılandırma ayarlarını, kullanıcı profilinizin depolanan gizli dizi Yöneticisi aracı çalışır.

::: moniker range=">= aspnetcore-3.0"

Gizli dizi Yöneticisi aracını içeren bir `init` .NET Core SDK'sı 3.0.100 komutunu veya üzeri. Kullanıcı parolalarını kullanmak için proje dizininde aşağıdaki komutu çalıştırın:

```console
dotnet user-secrets init
```

Önceki komutta ekler bir `UserSecretsId` öğesi içinde bir `PropertyGroup` , *.csproj* dosya. Varsayılan olarak, iç metni `UserSecretsId` bir GUID'dir. İç metni isteğe bağlıdır, ancak projeye benzersizdir.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Kullanıcı parolalarını kullanmak için tanımlamak bir `UserSecretsId` öğesi içinde bir `PropertyGroup` , *.csproj* dosya. İç metni `UserSecretsId` isteğe bağlıdır, ancak projeye benzersizdir. Geliştiriciler genellikle oluşturmak için bir GUID `UserSecretsId`.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> Visual Studio'da, Çözüm Gezgini'nde projeye sağ tıklayıp seçin **nıcı parolalarını Yönet** bağlam menüsünden. Bu hareket ekler bir `UserSecretsId` öğesi için bir GUID ile doldurulmuş *.csproj* dosya.

## <a name="set-a-secret"></a>Bir gizli dizisi ayarlayın

Bir anahtarı ve değeri içeren bir uygulama gizli anahtarı tanımlayın. Proje gizliliği ilişkilendirilen `UserSecretsId` değeri. Örneğin, hangi dizininden aşağıdaki komutu çalıştırın *.csproj* dosyası var:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

Önceki örnekte, iki nokta üst üste gösterir `Movies` bir nesne ile sabitidir bir `ServiceApiKey` özelliği.

Gizli dizi Yöneticisi Aracı diğer dizinlerden çok kullanılabilir. Kullanım `--project` seçeneği, dosya sistemi yolu sağlamak *.csproj* dosya yok. Örneğin:

```console
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>Visual Studio'da düzleştirme JSON yapısı

Visual Studio'nun **nıcı parolalarını Yönet** hareket açılır bir *secrets.json* dosyasını metin düzenleyicisinde. Öğesinin içeriğini değiştirin *secrets.json* depolanacak anahtar-değer çiftleri ile. Örneğin:

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

JSON yapısı değişiklikleri sonra düzleştirilmiş `dotnet user-secrets remove` veya `dotnet user-secrets set`. Örneğin, çalışan `dotnet user-secrets remove "Movies:ConnectionString"` daraltır `Movies` nesne sabit değeri. Değiştirilen dosya şuna benzer:

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>Birden çok gizli dizileri ayarlayın

Gizli dizi için JSON yönelterek ayarlanabilir `set` komutu. Aşağıdaki örnekte, *söz konusu input.json* dosyanın içeriğini yöneltilen için `set` komutu.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Bir komut kabuğunu açın ve aşağıdaki komutu yürütün:

  ```console
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[Linux / macOS](#tab/linux+macos)

Bir komut kabuğunu açın ve aşağıdaki komutu yürütün:

  ```console
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Gizli dizi erişimi

[ASP.NET Core yapılandırma API'si](xref:fundamentals/configuration/index) gizli dizi Yöneticisi gizli dizilere erişim sağlar.

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

Projeniz .NET Framework hedefliyorsa, yükleme [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketi.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Proje çağırdığında, ASP.NET Core 2.0 veya sonraki sürümlerde, kullanıcı parolaları yapılandırma kaynağı otomatik olarak geliştirme modunda eklenir <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> önceden yapılandırılmış varsayılan ana bilgisayar yeni bir örneğini başlatmak için. `CreateDefaultBuilder` çağrıları <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> olduğunda <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> olduğu <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

Zaman `CreateDefaultBuilder` değilse çağrılır, kullanıcı parolaları yapılandırma kaynağı açıkça çağrılarak ekleme <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> içinde `Startup` Oluşturucusu. Çağrı `AddUserSecrets` yalnızca, uygulama geliştirme ortamında, aşağıdaki örnekte gösterildiği gibi çalışır:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

Yükleme [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketi.

Kullanıcı parolaları yapılandırma kaynağı çağrısıyla ekleme <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets*> içinde `Startup` Oluşturucusu:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

Kullanıcı parolaları aracılığıyla alınabilir `Configuration` API:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a>Gizli diziler için bir POCO eşleme

Tüm nesne sabit değeri bir POCO (basit bir .NET sınıf özelliklere sahip) eşlemeye ilgili özellikleri toplamak için yararlı olur.

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Önceki gizli dizileri için bir POCO eşleyin `Configuration` API'nin [Nesne grafiği bağlama](xref:fundamentals/configuration/index#bind-to-an-object-graph) özelliği. Aşağıdaki kod bir özel bağlar `MovieSettings` POCO ve erişimleri `ServiceApiKey` özellik değeri:

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

`Movies:ConnectionString` Ve `Movies:ServiceApiKey` gizli dizileri ilgili özellikler eşleştirilmiş `MovieSettings`:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>Gizli dizesini değiştirme

Parolaları düz metin halinde depolanmasını güvenli değil. Örneğin, bir veritabanı bağlantı dizesi, içinde depolanan *appsettings.json* belirtilen kullanıcı için bir parola içerebilir:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Bir gizli dizi parolayı depolamak daha güvenli bir yaklaşımdır. Örneğin:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Kaldırma `Password` bağlantı dizesinde anahtar-değer çiftinden *appsettings.json*. Örneğin:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

Parolanın değer ayarlanabilir bir <xref:System.Data.SqlClient.SqlConnectionStringBuilder> nesnenin <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password*> özelliği bağlantı dizesini tamamlamak için:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a>Gizli dizilerini listeleme

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Hangi dizininden aşağıdaki komutu çalıştırarak *.csproj* dosyası var:

```console
dotnet user-secrets list
```

Aşağıdaki çıktı görünür:

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

Önceki örnekte, iki nokta üst üste anahtar adlarının nesne hiyerarşisi içinde gösterir. *secrets.json*.

## <a name="remove-a-single-secret"></a>Tek bir gizli dizi Kaldır

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Hangi dizininden aşağıdaki komutu çalıştırarak *.csproj* dosyası var:

```console
dotnet user-secrets remove "Movies:ConnectionString"
```

Uygulamanın *secrets.json* dosyası ile ilişkili anahtar-değer çiftini kaldırmak için değiştirildiği `MoviesConnectionString` anahtarı:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

Çalışan `dotnet user-secrets list` aşağıdaki iletiyi görüntüler:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Tüm gizli dizileri kaldırın

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Hangi dizininden aşağıdaki komutu çalıştırarak *.csproj* dosyası var:

```console
dotnet user-secrets clear
```

Uygulama için tüm kullanıcı gizli dizilerini gelen silinmiş *secrets.json* dosyası:

```json
{}
```

Çalışan `dotnet user-secrets list` aşağıdaki iletiyi görüntüler:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
