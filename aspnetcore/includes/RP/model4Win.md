<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a>İskele film modeli

* Aşağıdaki komut satırından çalıştırma (içeren proje dizininde *Program.cs*, *haline*, ve *.csproj* dosyaları):

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

Hatayı alırsanız:
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

Yanlış dizinindedir önce gelen hata olur. Proje dizini için bir komut kabuğunu açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları), ve ardından önceki komutu çalıştırın.

Hatayı alırsanız:
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

Visual Studio çıkın ve komutu yeniden çalıştırın.
