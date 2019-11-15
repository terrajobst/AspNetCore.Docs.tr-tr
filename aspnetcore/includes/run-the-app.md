# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="76459-101">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="76459-101">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="76459-102">Hata ayıklayıcı olmadan çalıştırmak için CTRL + F5 tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="76459-102">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="76459-103">Visual Studio [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) başlar ve uygulamayı çalıştırır.</span><span class="sxs-lookup"><span data-stu-id="76459-103">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="76459-104">Adres çubuğunda `localhost:port#` ve `example.com` gibi bir şey görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="76459-104">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="76459-105">Bunun nedeni, `localhost` yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="76459-105">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="76459-106">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="76459-106">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="76459-107">Visual Studio bir Web projesi oluşturduğunda, Web sunucusu için rastgele bir bağlantı noktası kullanılır.</span><span class="sxs-lookup"><span data-stu-id="76459-107">When Visual Studio creates a web project, a random port is used for the web server.</span></span>
 
# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="76459-108">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="76459-108">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="76459-109">Hata ayıklayıcı olmadan çalıştırmak için **CTRL-F5** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="76459-109">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="76459-110">Visual Studio Code, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve `http://localhost:5001`gider.</span><span class="sxs-lookup"><span data-stu-id="76459-110">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="76459-111">Adres çubuğunda `localhost:port#` ve `example.com` gibi bir şey görüntülenir.</span><span class="sxs-lookup"><span data-stu-id="76459-111">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="76459-112">Bunun nedeni, `localhost` yerel bilgisayar için Standart ana bilgisayar adıdır.</span><span class="sxs-lookup"><span data-stu-id="76459-112">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="76459-113">Localhost yalnızca yerel bilgisayardan Web isteklerine hizmet verir.</span><span class="sxs-lookup"><span data-stu-id="76459-113">Localhost only serves web requests from the local computer.</span></span>

  
# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="76459-114">Mac için Visual Studio</span><span class="sxs-lookup"><span data-stu-id="76459-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="76459-115">Visual Studio 'dan, hata ayıklayıcı olmadan çalıştırmak için **alt-cmd-ENTER** tuşlarına basın.</span><span class="sxs-lookup"><span data-stu-id="76459-115">From Visual Studio, press **Alt-Cmd-Enter** to run without the debugger.</span></span> <span data-ttu-id="76459-116">Alternatif olarak, menü çubuğuna gidin ve **hata ayıklama olmadan başlat > Çalıştır**' a gidin.</span><span class="sxs-lookup"><span data-stu-id="76459-116">Alternatively, navigate to the menu bar and go to **Run>Start Without Debugging**.</span></span>

  <span data-ttu-id="76459-117">Visual Studio, [Kestrel](xref:fundamentals/servers/kestrel)başlatır, bir tarayıcı başlatır ve `http://localhost:5001`gider.</span><span class="sxs-lookup"><span data-stu-id="76459-117">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

<!-- End of VS tabs -->

---