# <a name="aspnet-core-distributed-cache-sample"></a>ASP.NET Core dağıtılmış önbellek örneği

Bu örnek, bir dağıtılmış önbellek kullanımını gösterir. Bu örnek, açıklanan senaryoyu gösterir [ASP.NET Core dağıtılmış bir önbellekte çalışın](https://docs.microsoft.com/aspnet/core/performance/caching/distributed) konu.

Üretim ortamında, örnek uygulama, dağıtılmış bir SQL Server Önbelleği kullanmak üzere yapılandırılır. Dağıtılmış bir Redis önbelleği kullanmak üzere uygulamayı yapılandırmak için önişlemci yönergesi en üstündeki değiştirme *Startup.cs* Redis kullanılacak dosyasını (`#define Redis // SQLServer`). Daha fazla bilgi için [önişlemci yönergeleri örnek kodda](https://docs.microsoft.com/aspnet/core/#preprocessor-directives-in-sample-code).
