> [!WARNING]
> Güvenlik nedenleriyle, `GET` istek verilerini sayfa modeli özelliklerine bağlamayı tercih etmeniz gerekir. Özelliklerle eşleştirmadan önce Kullanıcı girişini doğrulayın. `GET` bağlamaya dönüştürmek, sorgu dizesine veya rota değerlerine dayanan senaryoları adreslemekte yararlıdır.
>
> `GET` isteklerindeki bir özelliği bağlamak için, [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) özniteliğinin `SupportsGet` özelliğini `true`olarak ayarlayın:
>
> ```csharp
> [BindProperty(SupportsGet = true)]
> ```
>
> Daha fazla bilgi için bkz. [ASP.NET Core topluluk alışması: Get tartışmasına bağlama (YouTube)](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s).
