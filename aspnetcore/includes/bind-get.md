> [!WARNING]
> <span data-ttu-id="3ce78-101">Güvenlik nedenleriyle, `GET` istek verilerini sayfa modeli özelliklerine bağlamayı tercih etmeniz gerekir.</span><span class="sxs-lookup"><span data-stu-id="3ce78-101">For security reasons, you must opt in to binding `GET` request data to page model properties.</span></span> <span data-ttu-id="3ce78-102">Özelliklerle eşleştirmadan önce Kullanıcı girişini doğrulayın.</span><span class="sxs-lookup"><span data-stu-id="3ce78-102">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="3ce78-103">`GET` bağlamaya dönüştürmek, sorgu dizesine veya rota değerlerine dayanan senaryoları adreslemekte yararlıdır.</span><span class="sxs-lookup"><span data-stu-id="3ce78-103">Opting into `GET` binding is useful when addressing scenarios that rely on query string or route values.</span></span>
>
> <span data-ttu-id="3ce78-104">`GET` isteklerindeki bir özelliği bağlamak için, [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) özniteliğinin `SupportsGet` özelliğini `true`olarak ayarlayın:</span><span class="sxs-lookup"><span data-stu-id="3ce78-104">To bind a property on `GET` requests, set the [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute's `SupportsGet` property to `true`:</span></span>
>
> ```csharp
> [BindProperty(SupportsGet = true)]
> ```
>
> <span data-ttu-id="3ce78-105">Daha fazla bilgi için bkz. [ASP.NET Core topluluk alışması: Get tartışmasına bağlama (YouTube)](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s).</span><span class="sxs-lookup"><span data-stu-id="3ce78-105">For more information, see [ASP.NET Core Community Standup: Bind on GET discussion (YouTube)](https://www.youtube.com/watch?v=p7iHB9V-KVU&feature=youtu.be&t=54m27s).</span></span>
