---
title: Günlüğe kaydetme ve ASP.NET Core signalr'da tanılama
author: anurse
description: ASP.NET Core SignalR uygulamanızdan tanılama toplama hakkında bilgi edinin.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 06/19/2019
uid: signalr/diagnostics
ms.openlocfilehash: 69dbd057b3dcadeb3ca5d94ede1234530fb447db
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313704"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a>Günlüğe kaydetme ve ASP.NET Core signalr'da tanılama

Tarafından [Andrew Stanton-Nurse](https://twitter.com/anurse)

Bu makalede tanılama sorunlarını gidermenize yardımcı olması için ASP.NET Core SignalR uygulamanızdan toplamak için yönergeler sağlar.

## <a name="server-side-logging"></a>Sunucu tarafı günlüğe kaydetme

> [!WARNING]
> Sunucu tarafı günlüklerini uygulamanızdan hassas bilgiler içerebilir. **Hiçbir zaman** GitHub gibi genel forumları için üretim uygulamalardan ham günlükleri gönderin.

SignalR ASP.NET Core parçası olduğundan, sistem günlüğü ASP.NET Core kullanır. Varsayılan yapılandırmasında, çok az bilgi SignalR kaydeder, ancak bu yapılandırılmış. İlgili belgelere bakın [ASP.NET Core günlüğü](xref:fundamentals/logging/index#configuration) ASP.NET Core günlüğü yapılandırma hakkında ayrıntılar için.

SignalR iki Günlükçü kategoriler kullanır:

* `Microsoft.AspNetCore.SignalR` &ndash; Hub protokollerini ilgili günlükler için yöntemleri ve diğer Hub ilgili etkinlikleri çağırma, hub'ı etkinleştirme.
* `Microsoft.AspNetCore.Http.Connections` &ndash; WebSockets, uzun yoklama ve Server-Sent olayları ve alt düzey SignalR altyapı gibi taşımalar için günlükleri ilgili.

SignalR öğesinden alınan ayrıntılı günlükleri etkinleştirmek için hem de önceki ön eklerin yapılandırmanız `Debug` düzeyde, *appsettings.json* dosyası aşağıdaki öğelere ekleyerek `LogLevel` alt konusundaki `Logging`:

[!code-json[](diagnostics/logging-config.json?highlight=7-8)]

Ayrıca bu kodunda yapılandırabilirsiniz, `CreateWebHostBuilder` yöntemi:

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5-6)]

JSON tabanlı yapılandırma kullanmıyorsanız, yapılandırma sisteminizde aşağıdaki yapılandırma değerleri ayarlayın:

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

İç içe geçmiş yapılandırma değerlerini belirtmek nasıl belirlemek yapılandırma sistemi için belgelere bakın. Örneğin, ortam değişkenlerini kullanarak iki `_` yerine kullanılan karakterler `:` (örneğin, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).

Kullanmanızı öneririz `Debug` düzeyinde daha ayrıntılı tanılama için uygulamanızı toplanırken. `Trace` Düzeyi çok düşük düzeyli tanılama üretir ve uygulamanızdaki sorunları tanılamak için nadiren gereklidir.

## <a name="access-server-side-logs"></a>Sunucu tarafı günlüklerine erişme

Sunucu tarafı günlüklerini nasıl erişmenizi, çalıştırmakta olduğunuz ortamınıza bağlıdır.

### <a name="as-a-console-app-outside-iis"></a>Bir konsol uygulaması IIS dışında

Bir konsol uygulamasında çalıştırıyorsanız [konsol günlüğe](xref:fundamentals/logging/index#console-provider) varsayılan olarak etkinleştirilmesi gerekir. SignalR günlükleri konsolunda görünür.

### <a name="within-iis-express-from-visual-studio"></a>Visual Studio'dan IIS Express içinde

Visual Studio içinde günlük çıktısını görüntüler **çıkış** penceresi. Seçin **ASP.NET Core Web sunucusu** seçeneği bırakın.

### <a name="azure-app-service"></a>Azure uygulama hizmeti

Etkinleştirme **uygulama günlüğü (dosya sistemi)** seçeneğini **tanılama günlükleri** Azure App Service portalının bölümüne ve yapılandırma **düzeyi** için `Verbose`. Günlükleri kullanılabilir **günlük akışını** hizmetini ve App Service dosya sistemindeki günlüklerde. Daha fazla bilgi için [Azure günlük akışını](xref:fundamentals/logging/index#azure-log-streaming).

### <a name="other-environments"></a>Diğer ortamlarda

Başka bir ortama (örneğin, Docker, Kubernetes veya Windows hizmeti) uygulamanın dağıtıldığı olup <xref:fundamentals/logging/index> günlük sağlayıcıları ortam için uygun yapılandırma hakkında daha fazla bilgi için.

## <a name="javascript-client-logging"></a>JavaScript istemci günlüğe kaydetme

> [!WARNING]
> İstemci tarafı günlüklerini uygulamanızdan hassas bilgiler içerebilir. **Hiçbir zaman** GitHub gibi genel forumları için üretim uygulamalardan ham günlükleri gönderin.

JavaScript istemcisi kullanılırken kullanarak günlüğe kaydetme seçeneklerini yapılandırabilirsiniz `configureLogging` metodunda `HubConnectionBuilder`:

[!code-javascript[](diagnostics/logging-config-js.js?highlight=3)]

Tamamen günlüğünü devre dışı bırakmanız belirtin `signalR.LogLevel.None` içinde `configureLogging` yöntemi.

Aşağıdaki tabloda kullanılabilir günlük düzeyleri için JavaScript istemci gösterir. Günlük düzeyi şu değerlerden birini ayarlamak, günlüğe kaydetme düzeyi ve üzerindeki tüm düzeylerinde tabloda sağlar.

| Düzey | Açıklama |
| ----- | ----------- |
| `None` | Günlüğe ileti kaydedilmedi. |
| `Critical` | Bir uygulamanın tamamında hata iletileri. |
| `Error` | Geçerli işlem bir hata iletileri. |
| `Warning` | Önemli olmayan bir sorunu işaret eden iletileri. |
| `Information` | Bilgilendirme iletileri. |
| `Debug` | Tanılama iletileri hata ayıklama için yararlıdır. |
| `Trace` | Belirli sorunları tanılamak için tasarlanan çok ayrıntılı tanılama iletileri. |

Ayrıntı yapılandırdıktan sonra tarayıcı konsolunu (veya bir NodeJS uygulamasında standart çıktı) günlüklere yazılır.

Özel günlük sisteme günlükleri göndermek istiyorsanız, bir JavaScript nesnesi uygulayan sağlayabilir `ILogger` arabirimi. Uygulanması gereken tek yöntemdir `log`, olay düzeyi alır ve ileti olay ile ilişkili. Örneğin:

[!code-typescript[](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a>.NET istemci günlüğe kaydetme

> [!WARNING]
> İstemci tarafı günlüklerini uygulamanızdan hassas bilgiler içerebilir. **Hiçbir zaman** GitHub gibi genel forumları için üretim uygulamalardan ham günlükleri gönderin.

.NET İstemci'den günlükleri almak için kullanabileceğiniz `ConfigureLogging` metodunda `HubConnectionBuilder`. Bu aynı şekilde çalışır `ConfigureLogging` metodunda `WebHostBuilder` ve `HostBuilder`. ASP.NET Core içinde kullandığınız aynı günlük sağlayıcıları yapılandırabilirsiniz. Ancak, el ile yükleyin ve tek tek günlük sağlayıcıları için NuGet paketlerini etkinleştirme gerekir.

### <a name="console-logging"></a>Konsol günlüğü

Konsol günlüğü etkinleştirmek için ekleme [Microsoft.Extensions.Logging.Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) paket. Ardından, `AddConsole` yöntemi konsol günlüğe yapılandırmak için:

[!code-csharp[](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a>Hata ayıklama çıkış penceresinde günlüğü

Gitmek için günlükleri de yapılandırabilirsiniz **çıkış** Visual Studio'daki. Yükleme [Microsoft.Extensions.Logging.Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) kullanın ve paket `AddDebug` yöntemi:

[!code-csharp[](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a>Diğer günlük sağlayıcıları

SignalR Serilog, Seq, NLog veya tümleşik şekilde çalışarak, herhangi bir günlük sisteminin gibi diğer günlük sağlayıcılarını destekler `Microsoft.Extensions.Logging`. Günlüğe kaydetme sisteminizi sağlıyorsa bir `ILoggerProvider`, ile kaydedebilirsiniz `AddProvider`:

[!code-csharp[](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a>Denetim ayrıntı düzeyi

Diğer yerlerden uygulamanızda oturum açıyorsanız, varsayılan düzeyini değiştirme `Debug` çok ayrıntılı olabilir. SignalR günlükleri için günlüğe kaydetme düzeyini yapılandırmak için bir filtre kullanabilirsiniz. Bu kodda, çok sunucuda aynı şekilde gerçekleştirebilirsiniz:

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a>Ağ izleme

> [!WARNING]
> Ağ izleme, uygulamanız tarafından gönderilen her ileti tam içeriğini içerir. **Hiçbir zaman** ham ağ izlerini GitHub gibi genel forumları üretim uygulamalardan postalayabilir.

Bir sorunla karşılaşırsanız, ağ izleme bazen birçok yararlı bilgi sağlayabilir. Bir sorun bizim sorun İzleyicisi'ni kullanarak dosyaya kullanacaksanız bu özellikle yararlıdır.

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a>Fiddler'ı (tercih edilen seçenek) ile bir ağ izleme Topla

Bu yöntem, tüm uygulamalar için çalışır.

Fiddler HTTP izlemeleri toplamak için çok güçlü bir araçtır. Buradan yükleyin [telerik.com/fiddler](https://www.telerik.com/fiddler), başlatın ve ardından uygulamanızı çalıştırın ve sorunu yeniden oluşturun. Windows için fiddler kullanılabilir ve macOS ve Linux için beta sürümleri vardır.

HTTPS kullanarak bağlanırsa Fiddler HTTPS trafiği şifresini çözebilir emin olmak için bazı ek adımlar vardır. Daha fazla ayrıntı için [Fiddler belgeleri](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).

İzleme derledik sonra seçerek izleme verebilirsiniz **dosya** > **Kaydet** > **tüm oturumları** menü çubuğundan.

![Tüm oturumlar Fiddler ' dışarı aktarma](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a>Bir ağ izleme tcpdump (macOS ve yalnızca Linux) ile toplama

Bu yöntem, tüm uygulamalar için çalışır.

Bir komut kabuğu'ndan aşağıdaki komutu çalıştırarak tcpdump kullanarak ham TCP izlemeleri toplayabilirsiniz. Olması gerekebilir `root` veya komutu ile önek `sudo` izin hatası alırsanız:

```console
tcpdump -i [interface] -w trace.pcap
```

Değiştirin `[interface]` üzerinde yakalamak istediğiniz ağ arabirimine sahip. Genellikle, bu gibi bir şeydir `/dev/eth0` (için standart, Ethernet arabirimi) ya da `/dev/lo0` (için localhost trafik). Daha fazla bilgi için `tcpdump` ana bilgisayar sisteminizin man sayfasında.

## <a name="collect-a-network-trace-in-the-browser"></a>Bir ağ izleme tarayıcıda Topla

Bu yöntem, yalnızca tarayıcı tabanlı uygulamalar için çalışır.

Çoğu tarayıcı geliştirici araçları, tarayıcı ve sunucu arasındaki ağ etkinliği yakalamanıza olanak tanıyan bir "Ağ" sekmesi vardır. Ancak, bu izlemelerin WebSocket ve Server-Sent olay iletileri dahil değildir. Taşımaları kullanıyorsanız, Fiddler veya TcpDump (aşağıda açıklanmıştır) gibi bir araç kullanılması daha iyi bir yaklaşımdır.

### <a name="microsoft-edge-and-internet-explorer"></a>Microsoft Edge ve Internet Explorer

(Yönergeleri için hem uç hem de Internet Explorer aynıdır)

1. Geliştirme Araçları'nı açmak için F12 tuşuna basın
2. Ağ sekmesine tıklayın
3. (Gerekirse) sayfayı yeniler ve sorunu yeniden oluşturun
4. İzleme "HAR" dosyası olarak dışarı aktarmak için araç çubuğundaki Kaydet simgesine tıklayın:

![Kaydet simgesine Microsoft Edge geliştirici araçlarını Ağ sekmesi](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a>Google Chrome

1. Geliştirme Araçları'nı açmak için F12 tuşuna basın
2. Ağ sekmesine tıklayın
3. (Gerekirse) sayfayı yeniler ve sorunu yeniden oluşturun
4. Sağ istekleri listede herhangi bir yere tıklayın ve seçin "İçerikle HAR olarak kaydetme":

![Google Chrome geliştirme araçları ağı sekmesini "HAR içeriğe sahip farklı kaydet" seçeneği](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a>Mozilla Firefox

1. Geliştirme Araçları'nı açmak için F12 tuşuna basın
2. Ağ sekmesine tıklayın
3. (Gerekirse) sayfayı yeniler ve sorunu yeniden oluşturun
4. Sağ istekleri listede herhangi bir yere tıklayın ve "Kaydet tüm olarak HAR" seçin

![Mozilla Firefox geliştirme araçları ağı sekmesini "Kaydetme tümünü HAR olarak" seçeneği](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a>GitHub sorunları tanılama dosyaları Ekle

GitHub sorunları için sahip oldukları için yeniden adlandırarak Tanılama dosyalarını ekleyebilirsiniz bir `.txt` uzantısı ve ardından sürükleyip bunları sorun açın.

> [!NOTE]
> Lütfen günlük dosyalarını veya ağ izlerini içeriğini bir GitHub sorunu yapıştırın yok. Bu günlükler ve izlemeler oldukça büyük olabilir ve GitHub genellikle bunları keser.

![Bir GitHub sorunu açın günlük dosyaları sürükleme](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
