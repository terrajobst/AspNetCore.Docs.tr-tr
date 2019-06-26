# <a name="blazor-client-side-sample-app"></a>(İstemci-tarafı) Blazor örnek uygulaması

Bu örnek Blazor belgelerinde açıklanan Blazor senaryoları kullanımını gösterir.

## <a name="call-web-api-example"></a>Web API'si örneğinde çağırın

Web API'si örneğinde çalışan bir web API'si için örnek uygulamayı temel gerektirir <a href="https://docs.microsoft.com/aspnet/core/tutorials/first-web-api">Öğreticisi: ASP.NET Core MVC ile bir web API'si oluşturma</a> konu. Örnek uygulaması web API'si için isteği yapan `https://localhost:10000/api/todo`. Farklı bir web API adresi kullanılırsa, güncelleştirme `ServiceEndpoint` Razor bileşenin sabit değerinde `@functions` blok.</p>

Örnek uygulamayı yapar bir <a href="https://docs.microsoft.com/aspnet/core/security/cors">çıkış noktaları arası kaynak paylaşımı (CORS)</a> yapılan istekte `http://localhost:5000` veya `https://localhost:5001` Web API'sine. Kimlik bilgilerini (yetkilendirme tanımlama/üst bilgiler) izin verilir. Web API'SİNİN aşağıdaki CORS ara yazılım yapılandırma ekleme `Startup.Configure` yöntemi çağırmadan önce `UseMvc`:</p>

```csharp
app.UseCors(policy => 
    policy.WithOrigins("http://localhost:5000", "https://localhost:5001")
    .AllowAnyMethod()
    .WithHeaders(HeaderNames.ContentType, HeaderNames.Authorization)
    .AllowCredentials());
```

Etki alanları ve bağlantı noktalarını ayarlamak `WithOrigins` Blazor uygulama için gerektiği şekilde.

Web API CORS yetkilendirme tanımlama/headers ve istekleri istemci kodundan izin vermek için yapılandırılmış, ancak bu öğretici tarafından oluşturulan API web isteklerini gerçekten yetkilendirmek değil. Bkz: <a href="https://docs.microsoft.com/aspnet/core/security/">ASP.NET Core güvenlik ve kimlik makaleleri</a> Uygulama Kılavuzu için.
