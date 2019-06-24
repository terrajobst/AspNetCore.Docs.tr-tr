## <a name="app-startup-errors"></a>Uygulama başlatma hataları

::: moniker range=">= aspnetcore-2.2"

Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma. A *502.5 - işlem hatası* veya *500.30 - başlangıç hatası* hata ayıklama troubleshooted öneriler bu konudaki kullanarak yerel olarak olabilir olduğunda gerçekleşir.

::: moniker-end

::: moniker range="< aspnetcore-2.2"

Visual Studio'da varsayılan olarak bir ASP.NET Core projesi [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) hata ayıklama sırasında barındırma. A *502.5 işlem hatası* hata ayıklama troubleshooted öneriler bu konudaki kullanarak yerel olarak olabilir olduğunda gerçekleşir.

::: moniker-end

### <a name="500-internal-server-error"></a>500 İç sunucu hatası

Uygulamayı başlatır, ancak bir hata sunucu isteği yerine getirmesini önler.

Bu hata, başlatma sırasında veya bir yanıt oluşturulurken uygulamanın kod içinde oluşur. Yanıtın içerik içerebilir veya yanıt olarak görünebilir bir *500 İç sunucu hatası* tarayıcıda. Uygulama olay günlüğü, genellikle uygulama normal şekilde çalışmaya belirtir. Sunucunun açısından bakıldığında, doğru olmasıdır. Uygulama başladı, ancak geçerli bir yanıt oluşturulamıyor. Uygulama sunucuda bir komut isteminde çalıştırın veya [ASP.NET Core modülü stdout günlüğünü etkinleştir](#aspnet-core-module-stdout-log) sorunu gidermek için.

::: moniker range="= aspnetcore-2.2"

### <a name="5000-in-process-handler-load-failure"></a>500.0 işlem içi işleyici yükleme hatası

Çalışan işlemi başarısız olur. Uygulama başlamaz.

[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) .NET Core CLR bulup işlemde istek işleyicisi bulma başarısız oluyor (*aspnetcorev2_inprocess.dll*). Kontrol edin:

* Uygulamayı ya da hedeflediğinden [Microsoft.AspNetCore.Server.IIS](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IIS) NuGet paketini veya [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).
* ASP.NET Core paylaşılan framework'ün hedefliyorsa hedef makinede yüklü sürümü.

### <a name="5000-out-of-process-handler-load-failure"></a>500.0 giden işlem işleyicisi yükleme hatası

Çalışan işlemi başarısız olur. Uygulama başlamaz.

[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) işlem dışı barındırma istek işleyicisi bulmak başarısız olur. Emin *aspnetcorev2_outofprocess.dll* yanında bir alt klasöründe yoksa *aspnetcorev2.dll*.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="5000-in-process-handler-load-failure"></a>500.0 işlem içi işleyici yükleme hatası

Çalışan işlemi başarısız olur. Uygulama başlamaz.

Yüklenirken bilinmeyen bir hata oluştu [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) bileşenleri. Aşağıdaki eylemlerden birini gerçekleştirin:

* İlgili kişi [Microsoft Support](https://support.microsoft.com/oas/default.aspx?prid=15832) (seçin **Geliştirici Araçları** ardından **ASP.NET Core**).
* Stack Overflow sitesinde bir soru sorun.
* Bir sorun üzerinde dosya bizim [GitHub deposu](https://github.com/aspnet/AspNetCore).

### <a name="50030-in-process-startup-failure"></a>500.30 işlemdeki başlatma hatası

Çalışan işlemi başarısız olur. Uygulama başlamaz.

[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) işlemdeki .NET Core CLR, ancak başlatma girişimleri başarısız başlatmak. Bir işlem başlatma hatanın nedeni genellikle içinde girişlerinden belirlenebilir [uygulama olay günlüğüne](#application-event-log) ve [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log).

Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir. Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin.

### <a name="50031-ancm-failed-to-find-native-dependencies"></a>500.31 yerel bağımlılıkları bulmak ANCM başarısız oldu

Çalışan işlemi başarısız olur. Uygulama başlamaz.

[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) başlatmak .NET Core çalışma zamanı işlemdeki, ancak başlatma girişimleri başarısız. En yaygın nedeni bu başlatma başarısız olduğunda `Microsoft.NETCore.App` veya `Microsoft.AspNetCore.App` çalışma zamanı yüklü değil. ASP.NET Core 3.0 hedefine uygulamanın dağıtıldığı ve bu sürüm makinede mevcut değil, bu hata oluşur. Bir örnek hata iletisi aşağıdaki gibidir:

```
The specified framework 'Microsoft.NETCore.App', version '3.0.0' was not found.
  - The following frameworks were found:
      2.2.1 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview5-27626-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27713-13 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27714-15 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
      3.0.0-preview6-27723-08 at [C:\Program Files\dotnet\x64\shared\Microsoft.NETCore.App]
```

Hata iletisi, tüm yüklü .NET Core sürümleri ve uygulama tarafından istenen sürümü listelenir. Ya da bu hatayı düzeltmek için:

* Makine üzerinde .NET Core uygun sürümünü yükleyin.
* Uygulama makinede mevcut olan .NET Core sürümünü hedefleyecek şekilde değiştirin.
* Uygulama olarak Yayımla bir [müstakil dağıtım](/dotnet/core/deploying/#self-contained-deployments-scd).

Geliştirme çalıştırırken ( `ASPNETCORE_ENVIRONMENT` ortam değişkeni ayarlandığında `Development`), HTTP yanıtı belirli bir hata yazılır. Bir işlem başlatma hatanın nedenini ayrıca şurada bulunur [uygulama olay günlüğüne](#application-event-log).

### <a name="50032-ancm-failed-to-load-dll"></a>500.32 yük dll ANCM başarısız oldu

Çalışan işlemi başarısız olur. Uygulama başlamaz.

Bu hatanın en yaygın nedeni, uygulama için bir uyumsuz işlemciye mimari yayımlanır. Çalışan işlemin bir 32 bit uygulama olarak çalıştığı ve uygulama 64-bit hedefine yayımlandı, bu hata oluşur.

Ya da bu hatayı düzeltmek için:

* Çalışan işlemi olarak aynı işlemci mimarisi için uygulamayı yeniden yayımlayın.
* Uygulama olarak Yayımla bir [framework bağımlı dağıtım](/dotnet/core/deploying/#framework-dependent-executables-fde).

### <a name="50033-ancm-request-handler-load-failure"></a>500.33 ANCM istek işleyicisi yükleme hatası

Çalışan işlemi başarısız olur. Uygulama başlamaz.

Uygulamaya başvurmak yaramadı `Microsoft.AspNetCore.App` framework. Yalnızca hedefleyen uygulamaları `Microsoft.AspNetCore.App` framework tarafından barındırılabilir [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).

Bu hatayı düzeltmek için uygulamayı hedeflediği onaylayın `Microsoft.AspNetCore.App` framework. Denetleme `.runtimeconfig.json` uygulama tarafından hedeflenen framework doğrulayın.

### <a name="50034-ancm-mixed-hosting-models-not-supported"></a>500.34 desteklenmeyen barındırma modelleri ANCM karma

Çalışan işlemi, hem bir işlem içi uygulamayı hem de işlem dışı uygulama aynı işlem içinde çalıştırılamaz.

Bu hatayı düzeltmek için ayrı IIS uygulama havuzları, uygulamaları çalıştırma.

### <a name="50035-ancm-multiple-in-process-applications-in-same-process"></a>500.35 aynı işlemde birden çok işlem içi uygulama ANCM

Çalışan işlemi, hem bir işlem içi uygulamayı hem de işlem dışı uygulama aynı işlem içinde çalıştırılamaz.

Bu hatayı düzeltmek için ayrı IIS uygulama havuzları, uygulamaları çalıştırma.

### <a name="50036-ancm-out-of-process-handler-load-failure"></a>500.36 ANCM giden işlem işleyicisi yükleme hatası

İşlem dışı istek işleyicisi *aspnetcorev2_outofprocess.dll*, yanındaki değil *aspnetcorev2.dll* dosya. Bu, bozuk bir yüklemeyi gösterir [ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module).

Bu hatayı düzeltmek için yüklemesini onarmak [.NET Core barındırma paket](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle) (IIS için) ya da Visual Studio (için IIS Express).

### <a name="50037-ancm-failed-to-start-within-startup-time-limit"></a>500.37 ANCM başlangıç süre sınırı içinde başlatılamadı

ANCM bulunduğunda başlangıç süre sınırı içinde başlatılamadı. Varsayılan olarak, zaman aşımı süresi, 120 saniyedir.

Çok sayıda uygulamalar aynı makinede başlatırken bu hata oluşabilir. Sunucuda CPU/bellek ani başlatma sırasında kontrol edin. Birden fazla uygulama başlatma işlemi basamaklandırmak gerekebilir.

::: moniker-end

### <a name="5025-process-failure"></a>502.5 işlem hatası

Çalışan işlemi başarısız olur. Uygulama başlamaz.

[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) başlatmak çalışan işlemi ancak başlatma girişimleri başarısız. Bir işlem başlatma hatanın nedeni genellikle içinde girişlerinden belirlenebilir [uygulama olay günlüğüne](#application-event-log) ve [ASP.NET Core modülü stdout günlük](#aspnet-core-module-stdout-log).

Ortak bir hata durumu, uygulamanın mevcut olmayan ASP.NET Core paylaşılan framework sürümü hedefleme nedeniyle yanlış yapılandırılmış ' dir. Hangi sürümlerinin bir ASP.NET Core paylaşılan çerçeve hedef makinede yüklü olduğunu denetleyin. *Paylaşılan çerçeve* derlemeleri kümesidir ( *.dll* dosyaları) bu makinede yüklü ve gibi bir metapackage tarafından başvurulan `Microsoft.AspNetCore.App`. Gerekli en düşük sürüm metapackage başvuru belirtebilirsiniz. Daha fazla bilgi için [paylaşılan çerçeve](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).

*502.5 işlem hatası* hata sayfası, barındırma veya uygulama yanlış yapılandırma çalışan işlemin başarısız olmasına neden olduğunda döndürülür:

### <a name="failed-to-start-application-errorcode-0x800700c1"></a>Uygulama (hata kodu: '0x800700c1') başlatılamadı.

```
EventID: 1010
Source: IIS AspNetCore Module V2
Failed to start application '/LM/W3SVC/6/ROOT/', ErrorCode '0x800700c1'.
```

Uygulama başlatılamadı uygulamanın derleme ( *.dll*) yüklenmesi tamamlanamadı.

W3wp/ıısexpress işlemi ile yayımlanan uygulama arasındaki bir bit genişliği uyuşmazlığı olduğunda bu hata oluşur.

Uygulama havuzunun 32-bit ayarının doğru olduğundan emin olun:

1. IIS Yöneticisi'nin uygulama havuzunu seçin **uygulama havuzları**.
1. Seçin **Gelişmiş ayarlar** altında **uygulama havuzunu Düzenle** içinde **eylemleri** paneli.
1. Ayarlama **32-Bit uygulamaları etkinleştir**:
   * Bir 32-bit (x86) dağıtma, uygulama ayarlarsanız değer `True`.
   * Bir 64-bit (x64) dağıtma, uygulama ayarlarsanız değer `False`.

### <a name="connection-reset"></a>Bağlantı sıfırlama

Üst bilgileri gönderildiğinde sonra bir hata oluşursa, sunucunun göndermek çok geç bir **500 İç sunucu hatası** bir hata oluştuğunda. Bu durum, genellikle bir yanıt için karmaşık nesne serileştirme sırasında bir hata oluştuğunda gerçekleşir. Bu tür olarak görünür bir *bağlantı sıfırlama* istemci üzerinde hata. [Uygulama günlüğü](xref:fundamentals/logging/index) bu tür hataları gidermeye yardımcı olabilir.

## <a name="default-startup-limits"></a>Varsayılan başlangıç sınırları

[ASP.NET Core Modülü](xref:host-and-deploy/aspnet-core-module) varsayılan ile yapılandırılmış *startupTimeLimit* 120 saniye. Varsayılan değer olarak sol uygulama modülü bir işlem hatası oturum önce başlatmak için iki dakika sürebilir. Modül yapılandırma hakkında daha fazla bilgi için bkz. [aspNetCore öğenin öznitelikleri](xref:host-and-deploy/aspnet-core-module#attributes-of-the-aspnetcore-element).
