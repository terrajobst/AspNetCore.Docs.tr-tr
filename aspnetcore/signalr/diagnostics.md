---
title: ASP.NET Core SignalR günlüğe kaydetme ve tanılama
author: anurse
description: ASP.NET Core SignalR uygulamanızdan tanılamayı nasıl toplayacağınızı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: signalr
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/diagnostics
ms.openlocfilehash: c5bd2ac27f8ca486b0d75aed8439747f72448625
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660974"
---
# <a name="logging-and-diagnostics-in-aspnet-core-signalr"></a>ASP.NET Core SignalR 'de günlüğe kaydetme ve tanılama

, [Andrew Stanton-nurte](https://twitter.com/anurse)

Bu makalede, sorunları gidermeye yardımcı olmak üzere ASP.NET Core SignalR uygulamanızdan tanılama toplamaya yönelik rehberlik sunulmaktadır.

## <a name="server-side-logging"></a>Sunucu tarafında günlüğe kaydetme

> [!WARNING]
> Sunucu tarafı günlükleri, uygulamanızdan önemli bilgiler içerebilir. Ham günlükleri **hiçbir** şekilde üretim uygulamalarından GitHub gibi genel forumlara nakletmeyin.

SignalR ASP.NET Core bir parçası olduğundan, ASP.NET Core günlük sistemini kullanır. Varsayılan yapılandırmada, SignalR çok az bilgiyi günlüğe kaydeder, ancak bu yapılandırılabilir. ASP.NET Core günlüğü yapılandırma hakkında ayrıntılar için [ASP.NET Core günlüğe kaydetme](xref:fundamentals/logging/index#configuration) hakkındaki belgelere bakın.

SignalR iki günlükçü kategorisi kullanır:

* Merkez protokolleriyle ilgili Günlükler için `Microsoft.AspNetCore.SignalR`, hub 'Ları etkinleştirme, yöntemleri çağırma ve hub ile ilgili diğer etkinlikler için &ndash;.
* WebSockets, uzun yoklama ve sunucu tarafından gönderilen olaylar ve alt düzey SignalR altyapısı gibi aktarımlarıyla ilgili Günlükler için `Microsoft.AspNetCore.Http.Connections` &ndash;.

SignalR 'den ayrıntılı günlükleri etkinleştirmek için, aşağıdaki öğeleri `Logging``LogLevel` alt bölümüne ekleyerek, yukarıdaki ön ekleri *appSettings. JSON* dosyanızdaki `Debug` düzeyine yapılandırın:

[!code-json[](diagnostics/logging-config.json?highlight=7-8)]

Ayrıca, `CreateWebHostBuilder` yöntemdeki kodda de yapılandırabilirsiniz:

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5-6)]

JSON tabanlı yapılandırma kullanmıyorsanız, yapılandırma sisteminizde aşağıdaki yapılandırma değerlerini ayarlayın:

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

İç içe yapılandırma değerlerinin nasıl belirleneceğini belirlemek için yapılandırma sisteminizin belgelerini denetleyin. Örneğin, ortam değişkenlerini kullanırken, `:` yerine iki `_` karakter kullanılır (örneğin, `Logging__LogLevel__Microsoft.AspNetCore.SignalR`).

Uygulamanız için daha ayrıntılı tanılama toplanırken `Debug` düzeyinin kullanılması önerilir. `Trace` düzeyi çok düşük düzey Tanılamalar üretir ve uygulamanızdaki sorunları tanılamak için nadiren gereklidir.

## <a name="access-server-side-logs"></a>Sunucu tarafı günlüklerine erişin

Sunucu tarafı günlüklerine erişme, çalıştırdığınız ortama bağlıdır.

### <a name="as-a-console-app-outside-iis"></a>IIS dışında bir konsol uygulaması olarak

Konsol uygulamasında çalıştırıyorsanız, [konsol günlükçüsü](xref:fundamentals/logging/index#console-provider) varsayılan olarak etkinleştirilmelidir. SignalR günlükleri konsolunda görünür.

### <a name="within-iis-express-from-visual-studio"></a>Visual Studio 'dan IIS Express içinde

Visual Studio **çıktı** penceresinde günlük çıktısını görüntüler. **ASP.NET Core Web sunucusu** açılır seçeneğini belirleyin.

### <a name="azure-app-service"></a>Azure uygulama hizmeti

Azure App Service portalının **tanılama günlükleri** bölümünde **uygulama günlüğü (dosya sistemi)** seçeneğini etkinleştirin ve **düzeyi** `Verbose`olarak yapılandırın. Günlükler **günlük akış** hizmetinden ve App Service dosya sistemindeki günlüklerde kullanılabilir olmalıdır. Daha fazla bilgi için bkz. [Azure günlük akışı](xref:fundamentals/logging/index#azure-log-streaming).

### <a name="other-environments"></a>Diğer ortamlar

Uygulama başka bir ortama (örneğin, Docker, Kubernetes veya Windows hizmeti) dağıtılırsa, ortama uygun günlük sağlayıcılarının nasıl yapılandırılacağı hakkında daha fazla bilgi için bkz. <xref:fundamentals/logging/index>.

## <a name="javascript-client-logging"></a>JavaScript istemci günlüğü

> [!WARNING]
> İstemci tarafı günlükleri, uygulamanızdan önemli bilgiler içerebilir. Ham günlükleri **hiçbir** şekilde üretim uygulamalarından GitHub gibi genel forumlara nakletmeyin.

JavaScript istemcisini kullanırken, `HubConnectionBuilder``configureLogging` yöntemi kullanarak günlüğe kaydetme seçeneklerini yapılandırabilirsiniz:

[!code-javascript[](diagnostics/logging-config-js.js?highlight=3)]

Günlüğe kaydetmeyi tamamen devre dışı bırakmak için `configureLogging` yönteminde `signalR.LogLevel.None` belirtin.

Aşağıdaki tabloda JavaScript istemcisi için kullanılabilir olan günlük düzeyleri gösterilmektedir. Günlük düzeyinin bu değerlerden birine ayarlanması, bu düzeyde ve tabloda üzerindeki tüm düzeylerde günlüğe kaydetmeyi sağlar.

| Düzey | Açıklama |
| ----- | ----------- |
| `None` | Hiçbir ileti günlüğe kaydedilmez. |
| `Critical` | Uygulamanın tamamında bir hata olduğunu gösteren mesajlar. |
| `Error` | Geçerli işlemdeki bir hatayı gösteren mesajlar. |
| `Warning` | Önemli olmayan bir sorunu belirten mesajlar. |
| `Information` | Bilgi iletileri. |
| `Debug` | Tanılama iletileri hata ayıklama için yararlıdır. |
| `Trace` | Belirli sorunları tanılamak için tasarlanan çok ayrıntılı tanılama iletileri. |

Ayrıntı düzeyini yapılandırdıktan sonra, Günlükler tarayıcı konsoluna yazılır (veya bir NodeJS uygulamasında standart çıkış).

Günlükleri özel bir günlüğe kaydetme sistemine göndermek istiyorsanız, `ILogger` arabirimini uygulayan bir JavaScript nesnesi sağlayabilirsiniz. Uygulanması gereken tek yöntem, olayın düzeyini ve olayla ilişkili iletiyi alan `log`. Örnek:

[!code-typescript[](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a>.NET istemci günlüğü

> [!WARNING]
> İstemci tarafı günlükleri, uygulamanızdan önemli bilgiler içerebilir. Ham günlükleri **hiçbir** şekilde üretim uygulamalarından GitHub gibi genel forumlara nakletmeyin.

.NET istemcisinden günlükleri almak için `HubConnectionBuilder``ConfigureLogging` yöntemi kullanabilirsiniz. Bu, `WebHostBuilder` ve `HostBuilder``ConfigureLogging` yöntemiyle aynı şekilde çalışmaktadır. ASP.NET Core ' de kullandığınız günlük sağlayıcılarını yapılandırabilirsiniz. Ancak, bireysel günlük sağlayıcıları için NuGet paketlerini el ile yükleyip etkinleştirmeniz gerekir.

### <a name="console-logging"></a>Konsol günlüğü

Konsol günlüğünü etkinleştirmek için [Microsoft. Extensions. Logging. Console](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) paketini ekleyin. Ardından, konsol günlükçüsü 'yi yapılandırmak için `AddConsole` yöntemini kullanın:

[!code-csharp[](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a>Çıkış penceresi günlüğüne hata ayıkla

Günlükleri, Visual Studio 'daki **Çıkış** penceresine gitmek için de yapılandırabilirsiniz. [Microsoft. Extensions. Logging. Debug](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) paketini yükleyip `AddDebug` yöntemi kullanın:

[!code-csharp[](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a>Diğer günlüğe kaydetme sağlayıcıları

SignalR, Serilog, seq, NLog veya `Microsoft.Extensions.Logging`ile tümleştirilen herhangi bir günlük sistemi gibi diğer günlük sağlayıcılarını destekler. Günlüğe kaydetme sisteminiz bir `ILoggerProvider`sağlıyorsa, bu dosyayı `AddProvider`kaydedebilirsiniz:

[!code-csharp[](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a>Denetim ayrıntı düzeyi

Uygulamanızdaki diğer yerlerden oturum açıyorsanız, varsayılan düzeyin `Debug` olarak değiştirilmesi çok ayrıntılı olabilir. Kayıt düzeyini SignalR Günlükler için yapılandırmak üzere bir filtre kullanabilirsiniz. Bu, sunucuda olduğu şekilde kodda yapılabilir:

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a>Ağ izlemeleri

> [!WARNING]
> Bir ağ izlemesi, uygulamanız tarafından gönderilen her iletinin tam içeriğini içerir. Ham ağ izlemelerini, üretim uygulamalarından GitHub gibi genel forumlara **hiçbir** şekilde yayımlayamazsınız.

Bir sorunla karşılaşırsanız, ağ izleme bazen yararlı olabilecek çok sayıda bilgi sağlayabilir. Sorun izleyicimizde bir sorun oluşturacaksanız bu özellikle yararlı olur.

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a>Fiddler ile ağ izleme toplama (tercih edilen seçenek)

Bu yöntem tüm uygulamalar için geçerlidir.

Fiddler, HTTP izlemelerinin toplanması için çok güçlü bir araçtır. [Telerik.com/Fiddler](https://www.telerik.com/fiddler)adresinden yükleyip uygulamayı çalıştırın ve sorunu yeniden oluşturun. Fiddler Windows için kullanılabilir ve macOS ve Linux için beta sürümleri mevcuttur.

HTTPS kullanarak bağlanıyorsanız, Fiddler 'ın HTTPS trafiğinin şifresini çözebilmesini sağlamaya yönelik bazı ek adımlar vardır. Daha ayrıntılı bilgi için bkz. [Fiddler belgeleri](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS).

İzlemeyi topladıktan sonra, **dosya** > seçerek izlemeyi dışarı aktarabilirsiniz. bu işlemi, menü çubuğundan **tüm oturumları** > **Kaydet** ' i seçin.

![Fiddler 'tan tüm oturumlar dışarı aktarılıyor](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a>Tcpdump ile bir ağ izlemesi toplayın (yalnızca macOS ve Linux)

Bu yöntem tüm uygulamalar için geçerlidir.

Komut kabuğundan aşağıdaki komutu çalıştırarak, tcpdump kullanarak ham TCP izlemeleri toplayabilirsiniz. Bir izin hatası alırsanız, `root` olması veya komutun `sudo` öneki olması gerekebilir:

```console
tcpdump -i [interface] -w trace.pcap
```

`[interface]`, yakalamak istediğiniz ağ arabirimiyle değiştirin. Genellikle bu, `/dev/eth0` (Standart Ethernet arabiriminiz için) veya `/dev/lo0` (localhost trafiği için) gibi bir şeydir. Daha fazla bilgi için, ana bilgisayar sisteminizdeki `tcpdump` Man sayfasına bakın.

## <a name="collect-a-network-trace-in-the-browser"></a>Tarayıcıda bir ağ izlemesi toplayın

Bu yöntem yalnızca tarayıcı tabanlı uygulamalar için geçerlidir.

Çoğu tarayıcı Geliştirici Araçları, tarayıcı ve sunucu arasında ağ etkinliğini yakalamanızı sağlayan bir "ağ" sekmesi vardır. Ancak, bu izlemeler WebSocket ve sunucu tarafından gönderilen olay iletilerini içermez. Bu taşımaları kullanıyorsanız, Fiddler veya TcpDump (aşağıda açıklanmıştır) gibi bir araç kullanmak daha iyi bir yaklaşımdır.

### <a name="microsoft-edge-and-internet-explorer"></a>Microsoft Edge ve Internet Explorer

(Yönergeler hem Edge hem de Internet Explorer için aynıdır)

1. Geliştirici araçlarını açmak için F12 tuşuna basın
2. Ağ sekmesine tıklayın
3. Sayfayı (gerekirse) yenileyin ve sorunu yeniden oluşturun
4. İzlemeyi "HAR" dosyası olarak dışarı aktarmak için araç çubuğundaki Kaydet simgesine tıklayın:

![Microsoft Edge geliştirme araçları Ağ sekmesinde Kaydet simgesi](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a>Google Chrome

1. Geliştirici araçlarını açmak için F12 tuşuna basın
2. Ağ sekmesine tıklayın
3. Sayfayı (gerekirse) yenileyin ve sorunu yeniden oluşturun
4. İstek listesinde herhangi bir yere sağ tıklayın ve "İçerikle HAR olarak Kaydet" i seçin:

![Google Chrome geliştirme araçları Ağ sekmesinde "Içerik ile HAR olarak Kaydet" seçeneği](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a>Mozilla Firefox

1. Geliştirici araçlarını açmak için F12 tuşuna basın
2. Ağ sekmesine tıklayın
3. Sayfayı (gerekirse) yenileyin ve sorunu yeniden oluşturun
4. İstek listesinde herhangi bir yere sağ tıklayın ve "tümünü HAR olarak Kaydet" i seçin

![Mozilla Firefox geliştirme araçları Ağ sekmesinde "tümünü HAR olarak Kaydet" seçeneği](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a>GitHub sorunlarına tanılama dosyaları iliştirme

Tanılama dosyalarını, `.txt` uzantısına sahip olacak şekilde yeniden adlandırarak ve sonra sorunu üzerine sürükleyip bırakarak GitHub sorunlarına iliştirebilirsiniz.

> [!NOTE]
> Lütfen günlük dosyalarının veya ağ izlemelerinin içeriğini bir GitHub sorununa yapıştırmayın. Bu Günlükler ve izlemeler oldukça büyük olabilir ve GitHub genellikle bunları keser.

![Günlük dosyalarını bir GitHub sorununa sürükleme](diagnostics/attaching-diagnostics-files.png)

## <a name="additional-resources"></a>Ek kaynaklar

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
