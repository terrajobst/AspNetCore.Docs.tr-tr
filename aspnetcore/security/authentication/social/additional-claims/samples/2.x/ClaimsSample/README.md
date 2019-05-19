# <a name="additional-claims-sample-app"></a>Ek talep örnek uygulaması

Örnek uygulamayı gösterir nasıl yapılır:

* Kullanıcının verilen adı ve Soyadı Google'dan edinin ve Google tarafından sağlanan değerlerle ad talepleri depolar.
* Google erişim belirtecini kullanıcının Store `AuthenticationProperties`.

Örnek uygulamayı kullanmak için:

1. Uygulamayı kaydetme ve geçerli istemci Kimliğini ve Google kimlik doğrulaması için istemci parolası alın. Daha fazla bilgi için [Google dış oturum açma Kurulumu](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins).
1. İstemci Kimliğini ve istemci gizli anahtarı'nda sağlamak [GoogleOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) , `Startup.ConfigureServices`.
1. Uygulamayı çalıştırın ve My talep sayfası isteme. Kullanıcı oturum açmadı, uygulama için Google yönlendirir. Google ile oturum aç. Google kullanıcı uygulamaya geri yönlendirir (`/MyClaims`). Kullanıcının kimliği doğrulanır ve My talep sayfası yüklenir. Belirtilen ad ve Soyadı talep altında **kullanıcı taleplerini** Google tarafından sağlanan değerlerle. Erişim belirteci altında görüntülenen **kimlik doğrulaması özelliklerini**.
