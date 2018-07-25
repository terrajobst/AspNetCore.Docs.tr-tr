# <a name="cookie-sharing-sample-app"></a>Tanımlama bilgisi örnek uygulama paylaşımı

Örnek paylaşımı tanımlama bilgisi kimlik doğrulamasını kullanan üç uygulamalar arasında tanımlama bilgisi gösterilmektedir:

| Proje                             | Açıklama |
| ----------------------------------- | ----------- |
| CookieAuth.Core                     | ASP.NET Core Razor sayfalar uygulamasını kullanarak ASP.NET Core kimliği olmadan |
| CookieAuthWithIdentity.Core         | ASP.NET Core kimliği ile ASP.NET Core MVC uygulaması |
| CookieAuthWithIdentity.NETFramework | ASP.NET Identity ile ASP.NET Framework MVC uygulaması |

Yönergeler:

1. CookieAuth.Core uygulamayı çalıştırın. Bir kullanıcı kaydı. Kullanıcı kaydedildiğinde uygulamayı kullanıcının kimliğini doğrular. Kullanıcı oturumu kapatın.
1. Aynı tarayıcı oturumunda CookieAuthWithIdentity.Core uygulamayı çalıştırın. Aynı kullanıcı olarak kullanılan Core uygulaması ile kaydedin. Kullanıcı kaydedildiğinde uygulamayı kullanıcının kimliğini doğrular. Kullanıcı oturumu kapatın.
1. Aynı tarayıcı oturumunda CookieAuthWithIdentity.NETFramework uygulamayı çalıştırın. Kullanılan diğer uygulamalarla aynı kullanıcı kaydedin. Kullanıcı kaydedildiğinde uygulamayı kullanıcının kimliğini doğrular. Kullanıcı oturumu kapatın.
1. Üç uygulamalardan herhangi birini için kullanıcının oturum açmasını. Kimlik doğrulama tanımlama, uygulamalar arasında paylaşılır. Kullanıcı diğer iki uygulamalara otomatik olarak işaretli olduğunu unutmayın.
1. Uygulamaların herhangi birinin kullanıcının oturum açın. Kullanıcı diğer iki uygulama dışında otomatik olarak işaretli olduğunu unutmayın.

Bu örnek, açıklanan özellikleri gösterir. [uygulamalar arasında tanımlama bilgilerini paylaşma](https://docs.microsoft.com/aspnet/core/security/cookie-sharing) konu.
