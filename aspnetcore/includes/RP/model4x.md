<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="7aa9c-101">İskele film modeli</span><span class="sxs-lookup"><span data-stu-id="7aa9c-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="7aa9c-102">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="7aa9c-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7aa9c-103">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="7aa9c-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```
  
<span data-ttu-id="7aa9c-104">Hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="7aa9c-104">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="7aa9c-105">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="7aa9c-105">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>