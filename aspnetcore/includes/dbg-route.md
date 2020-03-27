## <a name="debug-diagnostics"></a>Hata ayıklama tanılaması

Ayrıntılı yönlendirme tanılama çıktısı için `Logging:LogLevel:Microsoft` `Debug`olarak ayarlayın. Örneğin, geliştirme ortamında appSettings ' i ayarlayın *. Development. JSON*:

```JSON
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Debug",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}