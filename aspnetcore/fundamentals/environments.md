---
title: "Birden çok ASP.NET Core ortamlarda ile çalışma"
author: ardalis
description: "Birden çok ortamlar genelinde uygulamanızın davranışını denetlemek için ASP.NET Core destek nasıl sağladığını öğrenin."
keywords: "ASP.NET Core, ortam ayarları, ASPNETCORE_ENVIRONMENT"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b5bba985-be12-4464-9a01-df3599b2a6f1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/environments
ms.openlocfilehash: 9127c3d7180422c0e3dbd813340dd485bf360c81
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="working-with-multiple-environments"></a>Birden çok ortamı ile çalışma

Tarafından [Steve Smith](https://ardalis.com/)

ASP.NET Core, geliştirme, hazırlama ve üretim gibi birden çok ortamlar üzerinde uygulamanızın davranışını denetlemek için destek sağlar. Ortam değişkenleri, uygulamanın bu ortam için yapılandırılması izin verme çalışma zamanı ortamı belirtmek için kullanılır.

[Görüntülemek veya karşıdan örnek kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/environments/sample) ([nasıl indirileceğini](xref:tutorials/index#how-to-download-a-sample))

## <a name="development-staging-production"></a>Hazırlama, geliştirme, üretim

ASP.NET Core belirli ortam değişkeni başvuruyor `ASPNETCORE_ENVIRONMENT` uygulamayı çalıştırmayı şu anda ortamı açıklamak için. Bu değişkeni ayarlayabilirsiniz istediğiniz herhangi bir değer, ancak üç değerler, kural tarafından kullanılır: `Development`, `Staging`, ve `Production`. Bu örnekleri kullanılan değerler ve ASP.NET Core ile sağlanan şablonları bulabilirsiniz.

Geçerli ortam ayarı program aracılığıyla uygulamanızdaki algılanabilir. Ayrıca, ortamı kullanabilirsiniz [yardımcı etiketi](../mvc/views/tag-helpers/index.md) belirli bölümlerde dahil etmek için [Görünüm](../mvc/views/index.md) geçerli uygulama ortamına bağlı.

Not: Windows ve macOS, belirtilen ortam ad büyük küçük harfe duyarlı değil. Değişkeni ayarlayın olup olmadığını `Development` veya `development` veya `DEVELOPMENT` sonuçları aynı olacaktır. Ancak, Linux olan bir **büyük küçük harfe duyarlı** varsayılan işletim sistemi. Ortam değişkenleri, dosya adları ve ayarları büyük küçük harfe duyarlılığın gerektirir.

### <a name="development"></a>Geliştirme

Bu, bir uygulama geliştirirken, kullanılan ortamı olmalıdır. Genellikle uygulama üretimde gibi çalıştığında kullanılabilir olmasını istediğiniz olmayacaktır özelliklerini etkinleştirmek için kullanılır [Geliştirici özel durum sayfasında](xref:fundamentals/error-handling#the-developer-exception-page).

Visual Studio kullanıyorsanız, ortam, projenizin hata ayıklama profillerinde yapılandırılabilir. Profilleri hata ayıklama belirtin [server](xref:fundamentals/servers/index) ayarlanması için uygulama ve tüm ortam değişkenlerini başlatılırken kullanılacak. Projeniz farklı ortam değişkenlerini ayarlama birden çok hata ayıklama profili var. Bu profiller kullanarak yönetmek **hata ayıklama** , web uygulaması projenizin sekmesinde **özellikleri** menüsü. Proje Özellikleri'nde ayarlanan değerlerle kalıcı yapılan *launchSettings.json* dosya ve de yapılandırabilirsiniz profilleri bu dosyayı doğrudan düzenleyerek.

IIS Express için profil burada gösterilir:

![Proje Özellikleri ayarını ortam değişkenleri](environments/_static/project-properties-debug.png)

Burada bir `launchSettings.json` profillerini içeren bir dosya `Development` ve `Staging`:

[!code-json[Main](../fundamentals/environments/sample/src/Environments/Properties/launchSettings.json?highlight=15,22)]

Proje profillere yapılan değişiklikler etkili olmaz kullanılan web sunucusu yeniden başlatılıncaya kadar (kendi ortama yapılan değişiklikleri algılar önce özellikle Kestrel yeniden başlatılması gerekir).

>[!WARNING]
> Ortam değişkenleri depolanır *launchSettings.json* herhangi bir şekilde güvenli değildir ve birini kullanıyorsanız, projenizi kaynak kodu deposu parçası olacak. **Hiçbir zaman kimlik bilgileri veya diğer gizli veriler bu dosyada depolar.** Bu tür veri depolamak için bir yer gerekiyorsa kullanın *gizli Yöneticisi* aracı açıklanan [geliştirme sırasında uygulama sırrı güvenli depolama](xref:security/app-secrets).

### <a name="staging"></a>Hazırlama

Kural tarafından bir `Staging` son üretime dağıtmadan önce sınamak için kullanılan bir üretim öncesi ortamı bir ortamdır. İdeal olarak, burada bunlar kullanıcılara etkisi olmadan çözülebilir hazırlama ortamında üretimde ortaya çıkabilecek sorunları önce gerçekleşmesi fiziksel özelliklerini, üretim, yansıtma.

### <a name="production"></a>Üretim

`Production` Ortam uygulamanın çalıştığı Canlı olduğunda ortam olduğu ve son kullanıcılar tarafından kullanılıyor. Bu ortamda, güvenlik, performans ve uygulama sağlamlık en üst düzeye çıkarmak için yapılandırılmalıdır. Geliştirme farklı bir üretim ortamında olabilecek bazı genel ayarları şunlardır:

* Önbelleğe almayı etkinleştirmek

* Tüm istemci-tarafı kaynaklar toplanmış, küçültülmüş ve potansiyel olarak bir CDN sunulan emin olun

* Tanılama ErrorPages devre dışı bırakma

* Kolay hata sayfalarında Aç

* Günlüğe kaydetme ve izleme üretim etkinleştir (örneğin, [Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-asp-net-five/))

Bu halinde tam bir liste olması amaçlanmıştır. Ortam denetimleri, uygulamanızın birçok bölümlerindeki Saçılma önlemek en iyisidir. Bunun yerine, uygulama içinde bu tür denetimleri gerçekleştirmek için önerilen yaklaşım olduğu `Startup` sınıfları mümkün olduğunda

## <a name="setting-the-environment"></a>Ortamını ayarlama

Ortam ayarı yöntemi işletim sistemine bağlıdır.

### <a name="windows"></a>Windows
Ayarlamak için `ASPNETCORE_ENVIRONMENT` uygulamayı kullanmaya başladıysanız geçerli oturum için `dotnet run`, aşağıdaki komutları kullanılır

**Komut satırı**
```
set ASPNETCORE_ENVIRONMENT=Development
```
**PowerShell**
```
$Env:ASPNETCORE_ENVIRONMENT = "Development"
```

Bu komutlar, yalnızca geçerli pencereyi için etkili olur. Pencere kapatıldığında ASPNETCORE_ENVIRONMENT ayarı varsayılan ayar veya bir makine değere geri döner. Windows Aç değeri genel olarak ayarlamak için **Denetim Masası** > **sistem** > **Gelişmiş Sistem ayarları** ve ekleme veya düzenleme`ASPNETCORE_ENVIRONMENT` değeri.

![Sistem Gelişmiş Özellikler](environments/_static/systemsetting_environment.png)

![ASP.NET Core ortam değişkeni](environments/_static/windows_aspnetcore_environment.png) 

**Web.config**

Bkz: *ortam değişkenlerini ayarlama* bölümünü [ASP.NET Core modül yapılandırma başvurusu](xref:host-and-deploy/aspnet-core-module#setting-environment-variables) konu.

**Her IIS uygulama havuzu**

Yalıtılmış (IIS 10.0 + desteklenir)'nde uygulama havuzları, çalışan tek tek uygulamalar için ortam değişkenlerini ayarlama gerekiyorsa bkz *AppCmd.exe komutunu* bölümünü [ortam değişkenleri \< environmentVariables >](/iis/configuration/system.applicationHost/applicationPools/add/environmentVariables/#appcmdexe) IIS konudaki başvuru belgelerini.

### <a name="macos"></a>MacOS
Geçerli ortamı macOS için satır içi uygulama çalışırken yapılabilir;

```bash
ASPNETCORE_ENVIRONMENT=Development dotnet run
```
veya kullanarak `export` uygulama çalıştırılmadan önce ayarlamak için.

```bash
export ASPNETCORE_ENVIRONMENT=Development
``` 
Makine düzeyi ortam değişkenleri ayarlanır *.bashrc* veya *.bash_profile* dosya. Herhangi bir metin düzenleyicisi kullanarak dosyasını düzenleyin ve aşağıdaki ifadeyi ekleyin.

```
export ASPNETCORE_ENVIRONMENT=Development
```  

### <a name="linux"></a>Linux
Linux distro'lar için kullanmak `export` değişkeni ayarları oturum tabanlı için komut satırında komut ve *bash_profile* makine düzeyi ortam ayarları dosyası.

## <a name="determining-the-environment-at-runtime"></a>Çalışma zamanında ortam belirleme

`IHostingEnvironment` Hizmeti ortamlarıyla çalışmak için çekirdek özeti sağlar. ASP.NET tarafından barındırma katman başlangıç mantığınızı içine yerleştirilebilir sağlanır ve bu hizmetidir [bağımlılık ekleme](dependency-injection.md). Visual Studio'da ASP.NET Core web sitesi şablonu ortama özgü yapılandırma dosyalarını (varsa) yüklemek ve uygulamanın hata ayarları işlemesini özelleştirmek için bu yaklaşımı kullanır. Şu anda belirtilen ortama çağırarak bakarak bu davranış her iki durumda da, elde edilen `EnvironmentName` veya `IsEnvironment` örneği üzerinde `IHostingEnvironment` uygun yönteme geçirilen.

> [!NOTE]
> Uygulamanın belirli bir ortamda, kullanım çalışıp çalışmadığını denetlemek gereksinim duyarsanız `env.IsEnvironment("environmentname")` düzgün çalışması yoksayacak bu yana (durumunda denetimi yerine `env.EnvironmentName == "Development"` örneğin).

Örneğin, ortam belirli hata işleme kurulumu için yapılandırma yönteminize aşağıdaki kodu kullanabilirsiniz:

[!code-csharp[Main](environments/sample/src/Environments/Startup.cs?range=19-30)]

Uygulama çalışıyorsa bir `Development` ortamı, ardından Visual Studio, (genellikle üretim çalıştırılmamalıdır) geliştirme özel hata sayfaları ve özel veritabanı hatası "BrowserLink" özelliğini kullanmak için gerekli çalışma zamanı desteği sağlar (geçişler uygulamak için bir yol sağlar ve bu nedenle yalnızca geliştirme kullanılmalıdır) sayfaları. Aksi takdirde, uygulama geliştirme ortamında çalışmıyorsa yanıt işlenmeyen özel durumlar olarak görüntülenecek bir standart hata işleme sayfasında yapılandırılır.

Çalışma zamanında, geçerli ortamı bağlı olarak istemciye gönderilecek hangi içerik belirlemeniz gerekebilir. Örneğin, geliştirme ortamında, genellikle olmayan simge durumuna küçültülmüş komut dosyaları ve daha kolay hata ayıklama yapar stil sayfaları işlevi görür. Üretim ve test ortamları küçültülmüş sürümleri hizmet ve genellikle bir CDN. Ortamı kullanarak bunu yapabilirsiniz [yardımcı etiketi](../mvc/views/tag-helpers/intro.md). Geçerli ortamı kullanarak belirtilen ortamları biriyle eşleşiyorsa ortam etiketi yardımcı yalnızca içeriğinin işleme `names` özniteliği.

[!code-html[Main](environments/sample/src/Environments/Views/Shared/_Layout.cshtml?range=13-22)]

Etiket Yardımcıları, uygulama bakın kullanmaya başlamak için [etiket Yardımcıları giriş](../mvc/views/tag-helpers/intro.md).

## <a name="startup-conventions"></a>Başlangıç kuralları

ASP.NET Core geçerli ortamda tabanlı bir uygulamanın başlangıç yapılandırma için bir kurala dayalı yaklaşım destekler. Hangi ortamına onu, oluşturmak ve kendi kuralları yönetmek sağlayarak göre yapılır uygulamanızı nasıl davranacağını program aracılığıyla da denetleyebilirsiniz.

Bir ASP.NET Core uygulama başladığında `Startup` sınıfı uygulama bootstrap, yükleme, yapılandırma ayarları, vb. için kullanılır ([ASP.NET başlangıç hakkında daha fazla bilgi](startup.md)). Ancak, bir sınıf varsa adlı `Startup{EnvironmentName}` (örneğin `StartupDevelopment`) ve `ASPNETCORE_ENVIRONMENT` ortam değişkeni ile eşleşen ada sonra `Startup` sınıfı yerine kullanılır. Bu nedenle, yapılandırabilirsiniz `Startup` geliştirme için ayrı bir ancak sahip `StartupProduction` , tanesi kullanılacak uygulama üretimde çalıştırıldığında. (Veya tersi).

> [!NOTE]
> Çağırma `WebHostBuilder.UseStartup<TStartup>()` yapılandırma bölümlerinin geçersiz kılar.

Tamamen ayrı kullanmanın yanı sıra `Startup` geçerli ortamda temel sınıfı da uygulamanın içinde nasıl yapılandırıldığı için ayarlamalar yapabilirsiniz bir `Startup` sınıfı. `Configure()` Ve `ConfigureServices()` yöntemlerini desteklemek ortama özgü sürümleri benzer `Startup` sınıfının kendisini, formun `Configure{EnvironmentName}()` ve `Configure{EnvironmentName}Services()`. Bir yöntem tanımlarsanız `ConfigureDevelopment()` yerine çağrılacağı `Configure()` geliştirme ortamı ayarlandığında. Benzer şekilde, `ConfigureDevelopmentServices()` yerine çağrılması `ConfigureServices()` aynı ortamda.

## <a name="summary"></a>Özet

ASP.NET Core birçok özellik ve geliştiricilerin uygulamalarını farklı ortamlarda nasıl davranacağını kolayca denetlemenize izin kuralları sağlar. Bir uygulamanın geliştirme hazırlama üretime yayımlarken ayarlanan ortam değişkenlerine uygun şekilde ortam için izin uygulama uygun olarak hata ayıklama, test ya da üretim kullanımı için en iyi duruma getirilmesi için.

## <a name="additional-resources"></a>Ek Kaynaklar

* [Yapılandırma](xref:fundamentals/configuration/index)

* [Etiket Yardımcıları giriş](../mvc/views/tag-helpers/intro.md)
