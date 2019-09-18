<a name="codegenerator"></a>Aşağıdaki tabloda ASP.NET Core kod Oluşturucu parametrelerinin ayrıntıları verilmiştir:

| Parametre               | Açıklama|
| ----------------- | ------------ |
| -a  | Modelin adı. |
| -dc  | Kullanılacak `DbContext` sınıf. |
| -UDL | Varsayılan düzeni kullanın. |
| -outDir | Görünümleri oluşturmak için göreli çıkış klasörü yolu. |
| --referenceScriptLibraries | Sayfaları `_ValidationScriptsPartial` Düzenle ve oluştur 'a ekler |

Komutuyla ilgili yardım almak için anahtarıkullanın:`h` `aspnet-codegenerator razorpage`

```dotnetcli
dotnet aspnet-codegenerator razorpage -h
```

Daha fazla bilgi için bkz. [DotNet ASPNET-CodeGenerator](xref:fundamentals/tools/dotnet-aspnet-codegenerator) 