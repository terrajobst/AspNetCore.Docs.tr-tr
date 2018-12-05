# <a name="aspnet-core-health-check-sample"></a>ASP.NET Core sistem durumu denetimi örneği

Bu örnek, sistem durumu denetleme ara yazılım ve hizmetler kullanımını gösterir. Bu örnek, açıklanan senaryoyu gösterir [durum denetimleri ASP.NET Core](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks) konu.

Konu başlığı altında açıklanan senaryo için örnek uygulama çalıştırmak için kullandığınız [çalıştırma dotnet](https://docs.microsoft.com/dotnet/core/tools/dotnet-run) proje klasöründeki bir komut kabuğu komut. Araştırırken senaryosu için bir anahtar geçirin. Uygulama varsayılanları `basic` geçiş için sağlanan değil yapılandırma `dotnet run`.

| Senaryo                                               | Örnek uygulama komutu               | Açıklama |
| ------------------------------------------------------ | -------------------------------- | ----------- |
| Temel durum araştırması (varsayılan)                           | `dotnet run --scenario basic`    | Uygulamanın HTTP isteklerini işleyebilir onaylar. |
| Veritabanı yoklama                                         | `dotnet run --scenario db`       | Bir SQL sunucusu veritabanı bağlantısını denetler. Bkz: [veritabanı araştırma](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks#database-probe) konudaki yönergeler için. |
| Hazırlık/canlılık araştırmaları                              | `dotnet run --scenario liveness` | İçin Canlı uygulama durumu denetimleri gerçekleştirir (*canlılık*) Canlı olmaya hazırlanma uygulama karşılaştırması (*hazırlık*). |
| Ölçüm tabanlı araştırma (bellek) /<br>özel bir yanıt yazıcı | `dotnet run --scenario writer`   | Bellek kullanımını karşı denetler ve sistem durumu uç nokta işaretlendiğinde özel JSON yazar. |
| Bağlantı noktası göre filtrele                                         | `dotnet run --scenario port`     | Belirli bir bağlantı noktası için durum denetimleri filtreler. Bkz: [bağlantı noktasına göre filtre](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks#filter-by-port) konudaki yönergeler için. |

Veritabanı araştırma ve bağlantı noktası filtresi senaryoları, ek yapılandırma gerektirir. Bkz: [durum denetimleri](https://docs.microsoft.com/aspnet/core/host-and-deploy/health-checks) Ayrıntılar için konu.
