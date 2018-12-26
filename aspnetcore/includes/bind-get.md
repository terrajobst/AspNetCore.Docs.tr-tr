> [!WARNING]
> Güvenlik nedenleriyle, bağlama kabul gerekir `GET` sayfa modeli özellikleri veri isteği. Özellikleri için eşleme önce kullanıcı girişi doğrulayın. İçin kabul `GET` bağlama, sorgu dizesi veya rota değerlerini senaryolarda belirtirken yararlıdır.
>
> Bir özelliği bağlamak `GET` istekleri, ayarlar [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) özniteliğin `SupportsGet` özelliğini `true`: `[BindProperty(SupportsGet = true)]`
