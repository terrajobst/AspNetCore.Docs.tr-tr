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
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a>Güvenli Depolama Uygulama sırrı ASP.NET Core geliştirme

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), ve [Scott Addie](https://github.com/scottaddie)

::: moniker range="<= aspnetcore-1.1"
[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/1.1) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/app-secrets/samples/2.1) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))
::: moniker-end

Bu belge, depolamak ve almak için kullanılan hassas verileri ASP.NET Core uygulama geliştirme sırasında teknikleri açıklar. Kaynak kodunda parolalar ve diğer hassas verileri asla saklamalısınız ve geliştirme üretim gizlilikleri kullanın veya modu test döndürmemelidir. Depolama ve Azure test ve üretim parolaları ile korumak [Azure anahtar kasası yapılandırma sağlayıcısı](xref:security/key-vault-configuration).

## <a name="environment-variables"></a>Ortam değişkenleri

Ortam değişkenleri, uygulama gizli kod veya yerel yapılandırma dosyalarını depolanmasını önlemek için kullanılır. Ortam değişkenleri tüm daha önce belirtilen yapılandırma kaynakları için yapılandırma değerleri geçersiz.

::: moniker range="<= aspnetcore-1.1"
Ortam değişkeni değerlerini okuma çağırarak yapılandırma [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) içinde `Startup` Oluşturucusu:

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=10)]
::: moniker-end

Bir ASP.NET Core web uygulaması, göz önünde bulundurun **tek tek kullanıcı hesaplarını** güvenlik etkindir. Bir varsayılan veritabanı bağlantı dizesi projesinin dahil *appsettings.json* anahtar dosyasıyla `DefaultConnection`. Varsayılan bağlantı dizesini kullanıcı modunda çalışır ve bir parola gerektirmez LocalDB ' dir. Uygulama dağıtımı sırasında `DefaultConnection` anahtar değeri ile bir ortam değişkeninin değeri geçersiz. Ortam değişkeni hassas kimlik bilgileriyle tam bağlantı dizesi depolayabilir.

> [!WARNING]
> Ortam değişkenleri genellikle düz ve şifresiz metin olarak depolanır. Makine ya da işlem aşılırsa, ortam değişkenleri Güvenilmeyen taraflar tarafından erişilebilir. Kullanıcı parolaları açığa çıkmasını önlemek için ek önlemler gerekli olabilir.

## <a name="secret-manager"></a>Parola Yöneticisi

Gizli Yöneticisi aracını hassas verileri ASP.NET Core projesinde geliştirme sırasında depolar. Bu bağlamda bir gizli verilerin bir uygulama gizli anahtarı parçasıdır. Uygulama parolaları proje ağacından ayrı bir konumda depolanır. Uygulama parolaları belirli bir projeyle ilişkili veya birkaç projeler arasında paylaşılan. Uygulama parolaları kaynak denetimine iade değil.

> [!WARNING]
> Gizli Yöneticisi aracını depolanan parolaları şifrelemek değil ve bir güvenilen deposu olarak değerlendirilmesi gerekir. Yalnızca geliştirme amacıyla kullanılır. Anahtarları ve değerleri, kullanıcı profili dizini bir JSON yapılandırma dosyasında depolanır.

## <a name="how-the-secret-manager-tool-works"></a>Parola Yöneticisi aracını nasıl çalışır

Parola Yöneticisi aracını hemen nerede ve nasıl değerleri saklanır gibi uygulama ayrıntılarını soyutlar. Bu uygulama ayrıntılarını bilmeden aracını kullanabilirsiniz. İçinde depolanan değerlerden bir [JSON](https://json.org/) yerel makinede bir sistem korumalı kullanıcı profili klasöründeki yapılandırma dosyası:

* Windows: `%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`
* Linux & macOS: `~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

Önceki dosya yolları ve Değiştir `<user_secrets_id>` ile `UserSecretsId` belirtilen değer *.csproj* dosya.

Konum veya gizli anahtarı Yöneticisi aracıyla kaydedilen verilerin biçimi bağlıdır kod yazmayın. Bu uygulama ayrıntılarını değişebilir. Örneğin, gizli değerleri şifreli değildir, ancak gelecekte olabilir.

::: moniker range="<= aspnetcore-2.0"
## <a name="install-the-secret-manager-tool"></a>Parola Yöneticisi aracını yükleyin

.NET Core SDK 2.1 içinde .NET Core CLI ile gizli Yöneticisi aracını paketlenebilir. Aracı yükleme ve önceki sürümleri, .NET Core SDK 2.0 için gereklidir.

Yükleme [Microsoft.Extensions.SecretManager.Tools](https://www.nuget.org/packages/Microsoft.Extensions.SecretManager.Tools/) ASP.NET Core projenizdeki NuGet paketi:

[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_CsprojFile&highlight=13-14)]

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
[!code-xml[](app-secrets/samples/1.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-xml[](app-secrets/samples/2.1/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]
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

JSON olarak cmdlet'ine yönelterek gizli toplu ayarlanabilir `set` komutu. Aşağıdaki örnekte, *input.json* dosyanın içeriğini yöneltilen için `set` Windows komutunu:

```console
type .\input.json | dotnet user-secrets set
```

MacOS ve Linux üzerinde aşağıdaki komutu kullanın:

```console
cat ./input.json | dotnet user-secrets set
```

## <a name="access-a-secret"></a>Bir gizli anahtar erişimi

[ASP.NET çekirdek yapılandırma API'si](xref:fundamentals/configuration/index) gizli Yöneticisi gizli erişim sağlar. .NET Core hedefleme 1.x veya .NET Framework yüklerseniz [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet paketi.

::: moniker range="<= aspnetcore-1.1"
Kullanıcı parolaları yapılandırma kaynağı'na ekleyin `Startup` Oluşturucusu:

[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupConstructor&highlight=5-8)]
::: moniker-end

Kullanıcı parolaları, aracılığıyla alınabilir `Configuration` API'si:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=23)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]
::: moniker-end

## <a name="string-replacement-with-secrets"></a>Gizli anahtarlarla dize değiştirme

Parolaları düz metin olarak depolanması risklidir. Örneğin, bir veritabanı bağlantı dizesi içinde depolanan *appsettings.json* belirtilen kullanıcı için bir parola içerebilir:

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings-unsecure.json?highlight=3)]

Parolayı gizlilik olarak depolamak daha güvenli bir yaklaşımdır. Örneğin:

```console
dotnet user-secrets set "DbPassword" "pass123"
```

Parolayı Değiştir *appsettings.json* bir yer tutucu. Aşağıdaki örnekte, `{0}` form yer tutucu olarak kullanılan bir [bileşik biçim dizesi](/dotnet/standard/base-types/composite-formatting#composite-format-string).

[!code-json[](app-secrets/samples/2.1/UserSecrets/appsettings.json?highlight=3)]

Parolanın değeri bağlantı dizesini tamamlamak için yer tutucu yerleştirilebilir:

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](app-secrets/samples/1.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=23-25)]
::: moniker-end
::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](app-secrets/samples/2.1/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-16)]
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