# <a name="cookie-sharing-sample-app"></a>Tanımlama bilgisi paylaşımı örnek uygulaması

Tanımlama bilgisi, tanımlama bilgisi kimlik doğrulamasını kullanan üç uygulamalar arasında paylaşımı örnek gösterilmektedir:

| Proje                             | Açıklama |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | ASP.NET Core kimliği kullanmadan ASP.NET Core 2.0 Razor sayfalarının uygulama |
| CookieAuthWithIdentity.Core         | ASP.NET Core kimlikle ASP.NET Core 2.0 MVC uygulama |
| CookieAuthWithIdentity.NETFramework | ASP.NET kimliği ile ASP.NET Framework 4.6.1 MVC uygulama |

Yönergeler:

1. CookieAuth.Core uygulamayı çalıştırın. Bir kullanıcı kaydı. Kullanıcı kaydedildikten sonra uygulama kullanıcının kimliğini doğrular. Kullanıcı oturumu kapatın.
1. Aynı tarayıcı oturumunda CookieAuthWithIdentity.Core uygulamayı çalıştırın. Çekirdek uygulamayla kullanılanlarla aynı kullanıcı kaydedin. Kullanıcı kaydedildikten sonra uygulama kullanıcının kimliğini doğrular. Kullanıcı oturumu kapatın.
1. Aynı tarayıcı oturumunda CookieAuthWithIdentity.NETFramework uygulamayı çalıştırın. Diğer uygulamalarla kullanılanlarla aynı kullanıcı kaydedin. Kullanıcı kaydedildikten sonra uygulama kullanıcının kimliğini doğrular. Kullanıcı oturumu kapatın.
1. Oturum açma kullanıcı herhangi üç uygulamalar için. Kimlik doğrulama tanımlama bilgisi, uygulamalar arasında paylaşılır. Kullanıcı otomatik olarak diğer iki uygulamalarda oturum unutmayın.
1. Herhangi bir uygulama kullanıcıdan çıkışı oturum açın. Kullanıcının diğer iki uygulamalarının oturumunu otomatik olarak imzalanır unutmayın.

Bu örnek açıklanan özelliklerini gösterir [uygulamalar arasında tanımlama bilgilerini paylaşımı](https://docs.microsoft.com/aspnet/core/security/data-protection/compatibility/cookie-sharing) konu.
