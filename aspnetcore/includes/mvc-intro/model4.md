Aşağıdaki tabloda ASP.NET Core kod Oluşturucu parametrelerinin ayrıntıları verilmiştir:

| Parametre               | Açıklama|
| ----------------- | ------------ |
| -a  | Modelin adı. |
| -dc  | Veri bağlamı. |
| -UDL | Varsayılan düzeni kullanın. |
| --relativeFolderPath | Dosyaları oluşturmak için göreli çıkış klasörü yolu. |
| --useDefaultLayout | Görünümler için varsayılan düzen kullanılmalıdır. |
| --referenceScriptLibraries | Sayfaları düzenlemek ve oluşturmak için `_ValidationScriptsPartial` ekler |

`aspnet-codegenerator controller` komutu hakkında yardım almak için `h` anahtarını kullanın:

```dotnetcli
dotnet aspnet-codegenerator controller -h
```

Daha fazla bilgi için bkz. [DotNet ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator)
