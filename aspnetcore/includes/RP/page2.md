`<form method="post">` Öğesi bir [Form etiketi yardımcı](xref:mvc/views/working-with-forms#the-form-tag-helper). Form etiketi yardımcı otomatik olarak içeren bir [antiforgery belirteci](xref:security/anti-request-forgery).

Yapı iskelesi altyapısı her bir alan için Razor biçimlendirme (ID dışında) modelindeki aşağıdakine benzer oluşturur:

[!code-cshtml[](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=15-20)]

[Doğrulama etiket Yardımcıları](xref:mvc/views/working-with-forms#the-validation-tag-helpers) (`<div asp-validation-summary` ve ` <span asp-validation-for`) doğrulama hataları görüntüler. Doğrulama bu seri ilerleyen bölümlerinde daha ayrıntılı ele alınmıştır.

[Etiket etiket Yardımcısı](xref:mvc/views/working-with-forms#the-label-tag-helper) (`<label asp-for="Movie.Title" class="control-label"></label>`) etiket resim yazısı oluşturur ve `for` için öznitelik `Title` özelliği.

[Giriş etiketi yardımcı](xref:mvc/views/working-with-forms) (`<input asp-for="Movie.Title" class="form-control" />`) kullanan [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) öznitelikleri ve istemci tarafında jQuery doğrulama için gereken HTML özniteliklerini üretir.