## <a name="debug-diagnostics"></a><span data-ttu-id="c6d19-101">Hata ayıklama tanılaması</span><span class="sxs-lookup"><span data-stu-id="c6d19-101">Debug diagnostics</span></span>

<span data-ttu-id="c6d19-102">Ayrıntılı yönlendirme tanılama çıktısı için `Logging:LogLevel:Microsoft` `Debug`olarak ayarlayın.</span><span class="sxs-lookup"><span data-stu-id="c6d19-102">For detailed routing diagnostic output, set `Logging:LogLevel:Microsoft` to `Debug`.</span></span> <span data-ttu-id="c6d19-103">Örneğin, geliştirme ortamında appSettings ' i ayarlayın *. Development. JSON*:</span><span class="sxs-lookup"><span data-stu-id="c6d19-103">For example, in the development environment, set *appsettings.Development.json*:</span></span>

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