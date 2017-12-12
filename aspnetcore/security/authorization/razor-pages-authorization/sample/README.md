# <a name="aspnet-core-authorization-sample"></a>ASP.NET Core yetkilendirme örneği

Bu örnek kuralları tarafından Razor sayfalarının yetkilendirme kullanımını göstermektedir. Bu örnek açıklanan özelliklerini gösterir [Razor sayfalarının yetkilendirme kuralları](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) konu.

Bu örnekteki kullanıcı yetkilendirme özellikleri açıklanan tanımlama bilgisi kimlik doğrulamasını kullanan [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulaması kullanarak](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) konu. Konular, ASP.NET Core kimliği kullanma hakkında daha fazla bilgi için bkz [kimlik doğrulaması](https://docs.microsoft.com/aspnet/core/security/authentication/index) belgelere bölümü.

Örnek çalıştırırken, e-posta adresini kullanmak  **maria.rodriguez@contoso.com**  kullanıcının kimliği doğrulanamıyor.

## <a name="examples-in-this-sample"></a>Bu örnekte örnekleri

| Özellik | Açıklama |
| ------- | ----------- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Ekler bir [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) sayfasına belirtilen yola sahip. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Ekler bir [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yola sahip bir klasördeki tüm sayfalar için. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Ekler bir [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yol içeren bir sayfa için. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Ekler bir [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yola sahip bir klasördeki tüm sayfalar için. |
