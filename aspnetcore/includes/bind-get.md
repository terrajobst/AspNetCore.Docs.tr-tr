> [!WARNING]
> <span data-ttu-id="48930-101">Güvenlik nedenleriyle, istek verilerini sayfa modeli özelliklerine bağlamayı `GET` kabul etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="48930-101">For security reasons, you must opt in to binding `GET` request data to page model properties.</span></span> <span data-ttu-id="48930-102">Özelliklerle eşleştirmadan önce Kullanıcı girişini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="48930-102">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="48930-103">`GET` Bağlama sırasında, sorgu dizesine veya rota değerlerine dayanan senaryolara yönelik adresleme yararlı olur.</span><span class="sxs-lookup"><span data-stu-id="48930-103">Opting into `GET` binding is useful when addressing scenarios that rely on query string or route values.</span></span>
>
> <span data-ttu-id="48930-104">`GET` İsteklere bir özelliği bağlamak için [[BindProperty]](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) özniteliğinin özelliğini şu `SupportsGet` şekilde `true`ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="48930-104">To bind a property on `GET` requests, set the [[BindProperty]](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute's `SupportsGet` property to `true`:</span></span>
>
> ```csharp
> [BindProperty(SupportsGet = true)]
> ```
>
> <span data-ttu-id="48930-105">Daha fazla bilgi için bkz [. asp.NET Core topluluk bilgisi: GET tartışmasına bağlayın (YouTube)](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s).</span><span class="sxs-lookup"><span data-stu-id="48930-105">For more information, see [ASP.NET Core Community Standup: Bind on GET discussion (YouTube)](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s).</span></span>
