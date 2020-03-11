# <a name="additional-claims-sample-app"></a>Ek talep örnek uygulaması

Örnek uygulama şunları gösterir:

* Google 'dan kullanıcının verilen adını ve soyadını alın ve ad taleplerini Google tarafından sağlanan değerlerle depolayın.
* Google erişim belirtecini kullanıcının `AuthenticationProperties`depolayın.

Örnek uygulamayı kullanmak için:

1. Uygulamayı kaydedin ve Google kimlik doğrulaması için geçerli bir istemci KIMLIĞI ve istemci parolası alın. Daha fazla bilgi için bkz. [Google dış oturum açma kurulumu](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins).
1. [GoogleOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) of `Startup.ConfigureServices`, uygulamaya istemci kimliğini ve istemci gizli anahtarını sağlayın.
1. Uygulamayı çalıştırın ve talepler sayfamı isteyin. Kullanıcı oturum açmamışsa, uygulama Google 'a yeniden yönlendirir. Google ile oturum açın. Google, kullanıcıyı uygulamaya geri yönlendirir (`/MyClaims`). Kullanıcının kimliği doğrulanır ve talepler sayfası yüklenir. Verilen ad ve soyadı talepleri, Google tarafından sağlanan değerlerle **Kullanıcı talepleri** altında bulunur. Erişim belirteci, **kimlik doğrulama özellikleri**altında görüntülenir.
