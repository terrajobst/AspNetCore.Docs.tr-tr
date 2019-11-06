<a name="codegenerator"></a>Aşağıdaki tabloda ASP.NET Core kod Oluşturucu parametrelerinin ayrıntıları verilmiştir:

| Parametre               | Açıklama|
| ----------------- | ------------ |
| -a  | Modelin adı. |
| -DC  | Kullanılacak `DbContext` sınıfı. |
| -UDL | Varsayılan düzeni kullanın. |
| -outDir | Görünümleri oluşturmak için göreli çıkış klasörü yolu. |
| --referenceScriptLibraries | Sayfaları düzenlemek ve oluşturmak için `_ValidationScriptsPartial` ekler |

`aspnet-codegenerator razorpage` komutu hakkında yardım almak için `h` anahtarını kullanın:

```dotnetcli
dotnet aspnet-codegenerator razorpage -h
```

Daha fazla bilgi için bkz. [DotNet ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator).
