# <a name="aspnet-core-authorization-sample"></a>ASP.NET Core yetkilendirme örneği

Bu örnek tarafından kuralları Razor sayfaları yetkilendirmesi kullanımı gösterilmektedir. Bu örnek, açıklanan özellikleri gösterir. [Razor sayfaları yetkilendirmesi kuralları](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) konu.

Bu örnekteki kullanıcı yetkilendirme özellikleri açıklanan tanımlama bilgisi kimlik doğrulamasını kullanan [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulamasını kullan](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) konu. Bu konuda gösterilen örnekler ve kavramlar eşit olarak ASP.NET Core kimliği kullanan uygulamalar için geçerlidir. ASP.NET Core kimliği kullanma hakkında daha fazla bilgi için bkz. [ASP.NET Core kimliği giriş](https://docs.microsoft.com/aspnet/core/security/authentication/identity).

E-posta adresi **maria.rodriguez@contoso.com** herhangi bir parolayla bir kullanıcı kimlik doğrulaması. Kullanıcının kimliğinin `AuthenticateUser` yönteminde *Pages/Account/Login.cshtml.cs* dosya. Gerçek hayatta kullanılan örnekte, bir veritabanında kullanıcı kimlik doğrulaması.

## <a name="examples-in-this-sample"></a>Bu örnekte örnekleri

| Özellik | Açıklama |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Ekler bir [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) sayfasına belirtilen yola sahip. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Ekler bir [AuthorizeFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) belirtilen yola sahip bir klasördeki tüm sayfalar için. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Ekler bir [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) için belirtilen yola sahip bir sayfa. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Ekler bir [AllowAnonymousFilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) belirtilen yola sahip bir klasördeki tüm sayfalar için. |
