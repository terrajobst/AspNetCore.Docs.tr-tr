---
title: "ASP.NET Core geliştirme sırasında uygulama sırrı güvenli depolama"
author: rick-anderson
description: "Gizli geliştirme sırasında güvenli bir şekilde depolamak nasıl gösterir"
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/app-secrets
ms.openlocfilehash: a23c9dc9ee1e20c0e0551a372e1cd706bb82070e
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="safe-storage-of-app-secrets-during-development-in-aspnet-core"></a>ASP.NET Core geliştirme sırasında uygulama sırrı güvenli depolama

Tarafından [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), ve [Scott Addie](https://scottaddie.com) 

Bu belge, gizli Yöneticisi aracını geliştirme gizli kodunuzu dışında tutmak için nasıl kullanabileceğinizi gösterir. En önemli kaynak kodunda parolalar ve diğer hassas verileri asla saklamalısınız ve geliştirme ve test modunda üretim gizli kullanmamanız noktasıdır. Bunun yerine kullanabileceğiniz [yapılandırma](xref:fundamentals/configuration/index) ortam değişkenlerinin bu değerleri okumak veya gizli Yöneticisi'ni kullanarak depolanan değerlerden aracı sistem. Gizli Manager aracı kaynak denetimine iade hassas verileri önlemeye yardımcı olur. [Yapılandırma](xref:fundamentals/configuration/index) sistem, bu makalede açıklanan gizli Yöneticisi aracıyla depolanan gizli okuyabilir.

Parola Yöneticisi aracını yalnızca geliştirme kullanılır. Azure test ve üretim parolaları ile koruma [Microsoft Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) yapılandırma sağlayıcısı. Bkz: [Azure anahtar kasası yapılandırma sağlayıcısı](https://docs.microsoft.com/aspnet/core/security/key-vault-configuration) daha fazla bilgi için.

## <a name="environment-variables"></a>Ortam değişkenleri

Uygulama gizli kod veya yerel yapılandırma dosyalarını depolamak önlemek için ortam değişkenleri gizli depolar. Kurulum [yapılandırma](xref:fundamentals/configuration/index) çağırarak ortam değişkenlerinin değerlerini okumasını framework `AddEnvironmentVariables`. Ardından, tüm daha önce belirtilen yapılandırma kaynakları için yapılandırma değerleri geçersiz kılmak için ortam değişkenlerini kullanabilirsiniz.

Örneğin, tek tek kullanıcı hesaplarını yeni bir ASP.NET Core web uygulaması oluşturursanız, bu varsayılan bağlantı dizesi ekleyecek *appsettings.json* anahtarla proje dosyasında `DefaultConnection`. Varsayılan bağlantı dizesini, kullanıcı modunda çalışır ve bir parola gerektirmez, LocalDB kullanmak üzere kurulur. Uygulamanızı test veya üretim sunucusunu dağıttığınızda, geçersiz kılabilirsiniz `DefaultConnection` anahtar değerini ayarıyla test veya üretim veritabanı için (potansiyel olarak harfe duyarlı kimlik bilgileri) bağlantı dizesini içeren bir ortam değişkeni Sunucu.

>[!WARNING]
> Ortam değişkenleri genellikle düz metin olarak depolanır ve şifrelenmez. Makine ya da işlem aşılırsa, ortam değişkenleri Güvenilmeyen taraflar tarafından erişilebilir. Kullanıcı parolaları açığa çıkmasını önlemek için ek önlemler gerekli olabilir.

## <a name="secret-manager"></a>Parola Yöneticisi

Parola Yöneticisi aracını proje ağacı dışında geliştirme çalışması için hassas verileri depolar. Parola Yöneticisi Aracı için parolaları depolamak için kullanılan bir proje araçtır bir [.NET Core](https://www.microsoft.com/net/core) proje geliştirme sırasında. Gizli Yöneticisi aracıyla uygulama sırrı belirli bir proje ile ilişkilendirmek ve birden çok projeler arasında paylaşın.

>[!WARNING]
> Gizli Yöneticisi aracını depolanan parolaları şifrelemek değil ve bir güvenilen deposu olarak değerlendirilmesi gerekir. Yalnızca geliştirme amacıyla kullanılır. Anahtarları ve değerleri, kullanıcı profili dizini bir JSON yapılandırma dosyasında depolanır.

## <a name="installing-the-secret-manager-tool"></a>Parola Yöneticisi aracını yükleme

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Çözüm Gezgini'nde projeye sağ tıklayın ve seçin **Düzenle \<project_name\>.csproj** ve bağlam menüsünden. Vurgulanan satırı ekleyin *.csproj* dosya ve ilişkili NuGet paket geri yüklemek için kaydedin:

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

Çözüm Gezgini'nde projeye tekrar sağ tıklayın ve seçin **kullanıcı parolaları yönetme** ve bağlam menüsünden. Bu hareketi yeni ekler `UserSecretsId` düğümde bir `PropertyGroup` , *.csproj* dosyası, aşağıdaki örnekte vurgulanmış olarak:

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

Değiştirilen kaydetme *.csproj* dosya de açılır bir `secrets.json` dosyasını bir metin düzenleyicisinde. Değiştir `secrets.json` aşağıdaki kod ile dosya:

```json
{
    "MySecret": "ValueOfMySecret"
}
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Ekleme `Microsoft.Extensions.SecretManager.Tools` için *.csproj* dosya ve çalıştırma [dotnet geri yükleme](/dotnet/core/tools/dotnet-restore). Parola Yöneticisi için komut satırını kullanarak aracı yüklemek için aynı adımları kullanabilirsiniz.

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-before.csproj?highlight=10)]

Parola Yöneticisi aracını aşağıdaki komutu çalıştırarak test edin:

```console
dotnet user-secrets -h
```

Parola Yöneticisi aracını kullanımı, seçenekleri ve komut Yardımı görüntüler.

> [!NOTE]
> Aynı dizinde olmalıdır *.csproj* tanımlanan araçları çalıştırmak için dosya *.csproj* dosyanın `DotNetCliToolReference` düğümleri.

Kullanıcı profilinizde depolanan projeye özgü yapılandırma ayarlarını gizli Yöneticisi aracını çalıştırır. Kullanıcı parolaları kullanmak için proje belirtmeniz gerekir bir `UserSecretsId` değeri kendi *.csproj* dosya. Değeri `UserSecretsId` isteğe bağlıdır, ancak projeye genellikle benzersizdir. Geliştiriciler genellikle oluşturmak için bir GUID `UserSecretsId`.

Ekleme bir `UserSecretsId` projenizde için *.csproj* dosyası:

[!code-xml[](app-secrets/sample/UserSecrets/UserSecrets-after.csproj?highlight=4)]

Bir parolayı ayarlamak için gizli Yöneticisi aracını kullanın. Örneğin, proje dizinden bir komut penceresinde aşağıdakileri girin:

```console
dotnet user-secrets set MySecret ValueOfMySecret
```

Diğer dizinlerden gizli Yöneticisi aracını çalıştırabilirsiniz ancak kullanmalısınız `--project` yolunu geçirmek için seçeneği *.csproj* dosyası:
 
```console
dotnet user-secrets set MySecret ValueOfMySecret --project c:\work\WebApp1\src\webapp1
```

Gizli Yöneticisi aracını, listesinde, kaldırmak ve uygulama temizlemek için de kullanabilirsiniz.

-----

## <a name="accessing-user-secrets-via-configuration"></a>Kullanıcı parolaları yapılandırması aracılığıyla erişme

Gizli Yöneticisi gizli yapılandırma sistemi aracılığıyla erişebilir. Ekleme `Microsoft.Extensions.Configuration.UserSecrets` paketini ve çalıştırma [dotnet geri yükleme](/dotnet/core/tools/dotnet-restore).

Kullanıcı parolaları yapılandırma kaynağı'na ekleyin `Startup` yöntemi:

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=16-19)]

Kullanıcı parolaları yapılandırma API'si aracılığıyla erişebilirsiniz:

[!code-csharp[](app-secrets/sample/UserSecrets/Startup.cs?highlight=26-29)]

## <a name="how-the-secret-manager-tool-works"></a>Parola Yöneticisi aracını nasıl çalışır

Parola Yöneticisi aracını hemen nerede ve nasıl değerleri saklanır gibi uygulama ayrıntılarını soyutlar. Bu uygulama ayrıntılarını bilmeden aracını kullanabilirsiniz. İçinde depolanan geçerli sürümde değerlerden bir [JSON](http://json.org/) kullanıcı profili dizini yapılandırma dosyasında:

* Windows: `%APPDATA%\microsoft\UserSecrets\<userSecretsId>\secrets.json`

* Linux: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

* macOS: `~/.microsoft/usersecrets/<userSecretsId>/secrets.json`

Değeri `userSecretsId` içinde belirtilen değerle geldiği *.csproj* dosya.

Bu uygulama ayrıntılarını değişebilir gibi konumu veya gizli anahtarı Yöneticisi aracıyla kaydedilen verilerin biçimi bağlıdır kodu yazmaya döndürmemelidir. Örneğin, gizli şu anda değerlerdir *değil* bugün şifrelenir, ancak gün olabilir.

## <a name="additional-resources"></a>Ek kaynaklar

* [Yapılandırma](xref:fundamentals/configuration/index)
