> [!WARNING]
> Güvenlik nedenleriyle, istek verilerini sayfa modeli özelliklerine bağlamayı `GET` kabul etmeniz gerekir. Özelliklerle eşleştirmadan önce Kullanıcı girişini doğrulayın. `GET` Bağlama sırasında, sorgu dizesine veya rota değerlerine dayanan senaryolara yönelik adresleme yararlı olur.
>
> `GET` İsteklere bir özelliği bağlamak için [[BindProperty]](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) özniteliğinin özelliğini şu `SupportsGet` şekilde `true`ayarlayın:
>
> ```csharp
> [BindProperty(SupportsGet = true)]
> ```
>
> Daha fazla bilgi için bkz [. asp.NET Core topluluk bilgisi: GET tartışmasına bağlayın (YouTube)](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s).
