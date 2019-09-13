# <a name="blazor-webassembly-sample-app"></a>Blazor WebAssembly örnek uygulaması

Bu örnek, Blazor belgelerinde açıklanan Blazor senaryolarının kullanımını gösterir.

## <a name="call-web-api-example"></a>Web API 'SI çağırma örneği

Web API örneği, <a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">öğretici için örnek uygulamayı temel alan çalışan bir Web API 'si gerektirir: ASP.NET Core MVC</a> konusunda bir Web API 'si oluşturun. Örnek uygulama, ' de `https://localhost:10000/api/todo`Web API 'sine istek yapar. Farklı bir Web API adresi kullanılırsa, Razor `ServiceEndpoint` `@functions` bileşeni bloğundaki sabit değeri güncelleştirin.</p>

Örnek uygulama, veya `https://localhost:5001` Web API 'sinden `http://localhost:5000` bir çıkış noktaları <a href="https://docs.microsoft.com/aspnet/core/security/cors">arası kaynak paylaşımı (CORS)</a> isteği oluşturur. Kimlik bilgilerine (yetkilendirme tanımlama bilgilerine/üstbilgilere) izin verilir. Aşağıdaki CORS ara yazılım yapılandırmasını, çağrı `Startup.Configure` `UseMvc`yapmadan önce Web API 'sinin yöntemine ekleyin:</p>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization)
    .AllowCredentials());
```

Blazor uygulaması için gerektiği `WithOrigins` gibi etki alanlarını ve bağlantı noktalarını ayarlayın.

Web API 'SI, istemci kodundan yetkilendirme tanımlama bilgilerine/üstbilgilere ve isteklere izin vermek üzere CORS için yapılandırılmıştır, ancak öğretici tarafından oluşturulan Web API 'SI istekleri yetkilendirmez. Uygulama Kılavuzu için bkz. <a href="https://docs.microsoft.com/aspnet/core/security/">güvenlik ve kimlik makaleleri ASP.NET Core</a> .
