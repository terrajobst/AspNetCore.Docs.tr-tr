<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="371a7-101">Film modeli iskelesini</span><span class="sxs-lookup"><span data-stu-id="371a7-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="371a7-102">Komut satırından aşağıdaki komutu çalıştırın (içeren proje dizininde *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları):</span><span class="sxs-lookup"><span data-stu-id="371a7-102">Run the following from the command line (in the project directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files):</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

<span data-ttu-id="371a7-103">Hatası alırsanız:</span><span class="sxs-lookup"><span data-stu-id="371a7-103">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="371a7-104">Proje dizini için bir komut kabuğunu açın (içeren dizine *Program.cs*, *Startup.cs*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="371a7-104">Open a command shell to the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
