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
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>ASP.NET Core sürümünde geliştirme sırasında uygulama gizli dizileri güvenli depolama

By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27)ve [Scott Ade](https://github.com/scottaddie)

[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))

Bu belgede, bir ASP.NET Core uygulamasının geliştirilmesi sırasında hassas verilerin depolanması ve alınması için teknikler açıklanmaktadır. Kaynak kodunda parolaları veya diğer hassas verileri hiçbir şekilde depolamayin. Üretim gizli dizileri geliştirme veya test için kullanılmamalıdır. Gizli dizileri uygulamayla birlikte dağıtılmamalıdır. Bunun yerine, ortamlar, ortam değişkenleri, Azure Key Vault vb. gibi denetimli bir yöntemle üretim ortamında kullanılabilir hale gelmelidir. [Azure Key Vault yapılandırma sağlayıcısıyla](xref:security/key-vault-configuration)Azure test ve üretim gizli dizilerini saklayabilir ve koruyabilirsiniz.

## <a name="environment-variables"></a>Ortam değişkenleri

Ortam değişkenleri, kodda veya yerel yapılandırma dosyalarında uygulama gizli dizileri depolanmasını önlemek için kullanılır. Ortam değişkenleri, daha önce belirtilen tüm yapılandırma kaynakları için yapılandırma değerlerini geçersiz kılar.

::: moniker range="<= aspnetcore-1.1"

`Startup` oluşturucusunda <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> çağırarak ortam değişkeni değerlerinin okunmasını yapılandırın:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=8)]

::: moniker-end

**Bireysel kullanıcı hesapları** güvenliğinin etkinleştirildiği bir ASP.NET Core Web uygulaması düşünün. Varsayılan bir veritabanı bağlantı dizesi, projenin *appSettings. JSON* dosyasına anahtar `DefaultConnection`dahildir. Varsayılan bağlantı dizesi, kullanıcı modunda çalışan ve parola gerektirmeyen LocalDB içindir. Uygulama dağıtımı sırasında, `DefaultConnection` anahtar değeri bir ortam değişkeninin değeri ile geçersiz kılınabilir. Ortam değişkeni, tüm bağlantı dizesini hassas kimlik bilgileriyle saklayabilir.

> [!WARNING]
> Ortam değişkenleri genellikle düz, şifresiz metin olarak depolanır. Makinenin veya işlemin güvenliği tehlikeye atılırsa, ortam değişkenlerine güvenilmeyen taraflar tarafından erişilebilir. Kullanıcı gizliliklerinin açıklanmasını önlemeye yönelik ek ölçüler gerekebilir.

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a>Gizli dizi Yöneticisi

Gizli verileri bir ASP.NET Core projesi geliştirme sırasında depolar. Bu bağlamda, önemli bir veri parçası bir uygulama gizli dizisi. Uygulama gizli dizileri, proje ağacından ayrı bir konumda depolanır. Uygulama gizli dizileri belirli bir projeyle ilişkilendirilir veya çeşitli projeler arasında paylaşılır. Uygulama gizli dizileri kaynak denetimine iade edilmedi.

> [!WARNING]
> Gizli dizi Yöneticisi aracı depolanan gizli dizileri şifrelemez ve güvenilir bir depo olarak değerlendirilmemelidir. Yalnızca geliştirme amaçlıdır. Anahtarlar ve değerler, Kullanıcı profili dizinindeki bir JSON yapılandırma dosyasında depolanır.

## <a name="how-the-secret-manager-tool-works"></a>Gizli dizi Yöneticisi aracı nasıl kullanılır?

Gizli dizi Yöneticisi Aracı, değerlerin nerede ve nasıl depolandığı gibi uygulama ayrıntılarını soyutlar. Bu uygulama ayrıntılarını bilmeden aracı kullanabilirsiniz. Değerler, yerel makinedeki sistem korumalı bir kullanıcı profili klasöründe bir JSON yapılandırma dosyasında depolanır:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Dosya sistemi yolu:

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macostablinuxmacos"></a>[Linux/macOS](#tab/linux+macos)

Dosya sistemi yolu:

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

Yukarıdaki dosya yollarında `<user_secrets_id>`, *. csproj* dosyasında belirtilen `UserSecretsId` değeri ile değiştirin.

Gizli dizi yöneticisi aracıyla kaydedilen verilerin konumuna veya biçimine bağlı olarak kod yazma. Bu uygulama ayrıntıları değişebilir. Örneğin, gizli değerler şifrelenmez, ancak gelecekte olabilir.

::: moniker range="<= aspnetcore-2.0"

## <a name="install-the-secret-manager-tool"></a>Gizli dizi Yöneticisi aracını yükler

Gizli dizi Yöneticisi Aracı, .NET Core SDK 2.1.300 veya sonraki sürümlerde .NET Core CLI paketlenmiştir. 2\.1.300 önceki .NET Core SDK sürümler için araç yüklemesi gereklidir.

> [!TIP]
> Yüklü .NET Core SDK sürüm numarasını görmek için bir komut kabuğundan `dotnet --version` çalıştırın.

Kullanılan .NET Core SDK araç içeriyorsa bir uyarı görüntülenir:

```console
The tool 'Microsoft.Extensions.SecretManager.Tools' is now included in the .NET Core SDK. Information on resolving this warning is available at (https://aka.ms/dotnetclitools-in-box).
```

ASP.NET Core projenize [Microsoft. Extensions. SecretManager. Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) NuGet paketini yükle. Örneğin:

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=15-16)]

Araç yüklemesini doğrulamak için bir komut kabuğu 'nda aşağıdaki komutu yürütün:

```dotnetcli
dotnet user-secrets -h
```

Gizli dizi Yöneticisi Aracı, örnek kullanım, Seçenekler ve komut yardımını görüntüler:

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
> *. Csproj* dosyasının `DotNetCliToolReference` öğelerinde tanımlanan araçları çalıştırmak için *. csproj* dosyasıyla aynı dizinde olmanız gerekir.

::: moniker-end

## <a name="enable-secret-storage"></a>Gizli depolamayı etkinleştir

Gizli dizi Yöneticisi Aracı, Kullanıcı profilinizde depolanan projeye özgü yapılandırma ayarları üzerinde çalışır.

::: moniker range=">= aspnetcore-3.0"

Gizli dizi Yöneticisi Aracı, .NET Core SDK 3.0.100 veya sonraki sürümlerde bir `init` komutu içerir. Kullanıcı gizli dizilerini kullanmak için, proje dizininde aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet user-secrets init
```

Önceki komut, *. csproj* dosyasının `PropertyGroup` içinde `UserSecretsId` öğesi ekler. Varsayılan olarak, `UserSecretsId` iç metni bir GUID 'dir. İç metin rastgele, ancak proje için benzersizdir.

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Kullanıcı gizli dizilerini kullanmak için, *. csproj* dosyasının bir `PropertyGroup` içinde `UserSecretsId` öğesi tanımlayın. `UserSecretsId` iç metni rastgele, ancak proje için benzersizdir. Geliştiriciler genellikle `UserSecretsId`için bir GUID oluşturur.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](app-secrets/samples/1.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

::: moniker-end

> [!TIP]
> Visual Studio 'da Çözüm Gezgini projeye sağ tıklayın ve bağlam menüsünden **Kullanıcı gizli dizilerini Yönet** ' i seçin. Bu hareket, *. csproj* DOSYASıNA bir GUID ile doldurulmuş bir `UserSecretsId` öğesi ekler.

## <a name="set-a-secret"></a>Gizli dizi ayarla

Anahtar ve değerini içeren bir uygulama gizli anahtarı tanımlayın. Gizli dizi, projenin `UserSecretsId` değeriyle ilişkilendirilir. Örneğin, *. csproj* dosyasının bulunduğu dizinden aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

Yukarıdaki örnekte, iki nokta üst üste `Movies` bir `ServiceApiKey` özelliğine sahip bir nesne sabit değeri olduğunu gösterir.

Gizli dizi Yöneticisi Aracı diğer dizinlerden de kullanılabilir. *. Csproj* dosyasının bulunduğu dosya sistemi yolunu sağlamak için `--project` seçeneğini kullanın. Örneğin:

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a>Visual Studio 'da JSON yapısı düzleştirme

Visual Studio 'nun **Kullanıcı gizli dizilerini Yönet** hareketi metin düzenleyicisinde bir *gizli dizi. JSON* dosyası açar. *Gizlilikler. JSON* içeriğini depolanacak anahtar-değer çiftleriyle değiştirin. Örneğin:

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

JSON yapısı, `dotnet user-secrets remove` veya `dotnet user-secrets set`aracılığıyla yapılan değişikliklerden sonra düzleştirilir. Örneğin, `dotnet user-secrets remove "Movies:ConnectionString"` çalıştırmak `Movies` nesnesinin değişmez değerini daraltır. Değiştirilen dosya şuna benzer:

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a>Birden çok gizli dizi ayarla

Bir toplu işlem, JSON tarafından `set` komutuna boru tarafından ayarlanabilir. Aşağıdaki örnekte, *input. JSON* dosyasının içeriği `set` komutuna yöneldir.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Bir komut kabuğu açın ve şu komutu yürütün:

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macostablinuxmacos"></a>[Linux/macOS](#tab/linux+macos)

Bir komut kabuğu açın ve şu komutu yürütün:

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a>Gizli dizi erişimi

[ASP.NET Core Configuration API 'si](xref:fundamentals/configuration/index) , gizli yönetici sırları için erişim sağlar.

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

Projeniz .NET Framework hedefliyorsa [Microsoft. Extensions. Configuration. Usergizlilikler](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketini yükler.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core 2,0 veya sonraki sürümlerde Kullanıcı gizli dizileri yapılandırma kaynağı, önceden yapılandırılmış varsayılanlar ile konağın yeni bir örneğini başlatmak için <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> çağırdığında geliştirme moduna otomatik olarak eklenir. `CreateDefaultBuilder`, <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>olduğunda <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> çağırır:

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Program.cs?name=snippet_CreateHostBuilder&highlight=2)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

`CreateDefaultBuilder` çağrılmıyorsa, `Startup` oluşturucusunda <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> çağırarak Kullanıcı gizli dizi yapılandırma kaynağını açıkça ekleyin. Aşağıdaki örnekte gösterildiği gibi, uygulama geliştirme ortamında çalıştırıldığında `AddUserSecrets` çağırın:

::: moniker-end

::: moniker range=">= aspnetcore-2.0 <= aspnetcore-2.2"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup2.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[Microsoft. Extensions. Configuration. Usergizlilikler](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketini yükler.

`Startup` oluşturucusunda <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> çağrısıyla Kullanıcı gizli dizi yapılandırma kaynağını ekleyin:

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=12)]

::: moniker-end

Kullanıcı gizli dizileri `Configuration` API 'SI aracılığıyla alınabilir:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=26)]

::: moniker-end

## <a name="map-secrets-to-a-poco"></a>Gizli dizileri bir POCO ile eşleme

Bir nesne sabit değerinin tamamını bir POCO 'ya eşleme (özelliklerle basit bir .NET sınıfı) ilgili özellikleri toplamak için faydalıdır.

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

Önceki gizli dizileri bir POCO 'ya eşlemek için `Configuration` API 'sinin [nesne grafiği bağlama](xref:fundamentals/configuration/index#bind-to-an-object-graph) özelliğini kullanın. Aşağıdaki kod, özel bir `MovieSettings` POCO 'a bağlanır ve `ServiceApiKey` özellik değerine erişir:

::: moniker range=">= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

::: moniker range="= aspnetcore-1.0"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

::: moniker-end

`Movies:ConnectionString` ve `Movies:ServiceApiKey` gizli dizileri `MovieSettings`ilgili özelliklerle eşlenir:

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a>Gizli dizileri olan dize değiştirme

Parolaların düz metin olarak depolanması güvenli değildir. Örneğin, *appSettings. JSON* içinde depolanan bir veritabanı bağlantı dizesi, belirtilen kullanıcı için bir parola içerebilir:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

Daha güvenli bir yaklaşım, parolayı gizli olarak depolanmalıdır. Örneğin:

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

`Password` anahtar-değer çiftini *appSettings. JSON*içindeki bağlantı dizesinden kaldırın. Örneğin:

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

Gizli dizi değeri, bağlantı dizesinin tamamlanabilmesi için <xref:System.Data.SqlClient.SqlConnectionStringBuilder> nesnenin <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> özelliğinde ayarlanabilir:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](app-secrets/samples/1.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=26-29)]

::: moniker-end

## <a name="list-the-secrets"></a>Gizli dizileri listeleyin

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

*. Csproj* dosyasının bulunduğu dizinden aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet user-secrets list
```

Şu çıktı görünür:

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

Yukarıdaki örnekte, anahtar adlarındaki bir iki nokta üst üste *gizli dizi. JSON*içindeki nesne hiyerarşisini gösterir.

## <a name="remove-a-single-secret"></a>Tek bir parolayı kaldır

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

*. Csproj* dosyasının bulunduğu dizinden aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

Uygulamanın *gizlilikler. JSON* dosyası, `MoviesConnectionString` anahtarıyla ilişkili anahtar-değer çiftini kaldırmak için değiştirilmiştir:

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

`dotnet user-secrets list` çalıştırmak aşağıdaki iletiyi görüntüler:

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a>Tüm gizli dizileri kaldır

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

*. Csproj* dosyasının bulunduğu dizinden aşağıdaki komutu çalıştırın:

```dotnetcli
dotnet user-secrets clear
```

Uygulamanın tüm Kullanıcı gizli dizileri, *gizlilikler. JSON* dosyasından silindi:

```json
{}
```

`dotnet user-secrets list` çalıştırmak aşağıdaki iletiyi görüntüler:

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>
