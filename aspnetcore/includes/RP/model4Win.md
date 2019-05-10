<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a>Film modeli iskelesini

* Komut satırından aşağıdaki komutu çalıştırın (içeren proje dizininde *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları):

  ```console
  dotnet restore
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Hatası alırsanız:

  ```
  No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Önceki hatayı yanlış dizine olduğunda gerçekleşir. Proje dizini için bir komut kabuğunu açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları), ve ardından yukarıdaki komutu çalıştırın.

Hatası alırsanız:

  ```
  The process cannot access the file
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll'
  because it is being used by another process.
  ```

Visual Studio çıkın ve komutu yeniden çalıştırın.
