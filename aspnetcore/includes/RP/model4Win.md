<a name="scaffold"></a>
### <a name="scaffold-the-movie-model"></a><span data-ttu-id="f4ce5-101">İskele film modeli</span><span class="sxs-lookup"><span data-stu-id="f4ce5-101">Scaffold the Movie model</span></span>

* <span data-ttu-id="f4ce5-102">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="f4ce5-102">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f4ce5-103">Şu komutu çalıştırın:</span><span class="sxs-lookup"><span data-stu-id="f4ce5-103">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```
  
<span data-ttu-id="f4ce5-104">Hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="f4ce5-104">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="f4ce5-105">Proje dizininde bir komut penceresi açın (içeren dizine *Program.cs*, *haline*, ve *.csproj* dosyaları).</span><span class="sxs-lookup"><span data-stu-id="f4ce5-105">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

<span data-ttu-id="f4ce5-106">Hatayı alırsanız:</span><span class="sxs-lookup"><span data-stu-id="f4ce5-106">If you get the error:</span></span>
  ```
  The process cannot access the file 
 'RazorPagesMovie/bin/Debug/netcoreapp2.0/RazorPagesMovie.dll' 
  because it is being used by another process.
  ```

<span data-ttu-id="f4ce5-107">Visual Studio çıkın ve komutu yeniden çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="f4ce5-107">Exit Visual Studio and run the command again.</span></span>