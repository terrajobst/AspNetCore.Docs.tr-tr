<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>İskele film modeli

* Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).
* Şu komutu çalıştırın:

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```
  
Hatayı alırsanız:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).