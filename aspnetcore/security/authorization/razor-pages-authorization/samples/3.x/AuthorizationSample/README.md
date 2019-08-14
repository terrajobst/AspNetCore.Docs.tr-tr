# <a name="aspnet-core-authorization-sample"></a>ASP.NET Core yetkilendirme örneği

Bu örnek, kurallara göre Razor Pages yetkilendirmesinin kullanımını gösterir. Bu örnek, [Razor Pages yetkilendirme kuralları](https://docs.microsoft.com/aspnet/core/security/authorization/razor-pages-authorization) konusunda açıklanan özellikleri gösterir.

Bu örnekteki kullanıcı yetkilendirmesi [ASP.NET Core kimliği olmadan tanımlama bilgisi kimlik doğrulaması kullanma](https://docs.microsoft.com/aspnet/core/security/authentication/cookie) konusunda açıklanan tanımlama bilgisi kimlik doğrulama özelliklerini kullanır. Bu konu başlığında gösterilen kavramlar ve örnekler ASP.NET Core kimliği kullanan uygulamalar için eşit oranda geçerlidir. ASP.NET Core kimliği kullanımı hakkında bilgi için bkz. [ASP.NET Core kimliğe giriş](https://docs.microsoft.com/aspnet/core/security/authentication/identity).

Kullanıcının kimliğini herhangi bir **maria.rodriguez@contoso.com** parolayla doğrulamak için e-posta adresini kullanın. Kullanıcının kimliği, `AuthenticateUser` *Sayfalar/Account/Login. cshtml. cs* dosyasındaki yönteminde doğrulanır. Gerçek dünyada bir örnekte, kullanıcının kimliği bir veritabanında doğrulanır.

## <a name="examples-in-this-sample"></a>Bu örnekteki örnekler

| Özellik | Açıklama |
| --- | --- |
| [AuthorizePage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) | Sayfaya belirtilen yola sahip bir [Authorizefilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) ekler. |
| [AuthorizeFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) | Belirtilen yola sahip bir klasördeki tüm sayfalara bir [Authorizefilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) ekler. |
| [AllowAnonymousToPage](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) | Belirtilen yola sahip bir sayfaya [Allowanonymousfilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) ekler. |
| [AllowAnonymousToFolder](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) | Belirtilen yola sahip bir klasördeki tüm sayfalara [Allowanonymousfilter](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) ekler. |
