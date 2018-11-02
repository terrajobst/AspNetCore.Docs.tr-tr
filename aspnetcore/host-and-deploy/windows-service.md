---
title: ASP.NET Core bir Windows hizmetinde barındırma
author: guardrex
description: ASP.NET Core uygulaması bir Windows hizmetinde barındırmayı öğrenin.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758199"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="4204f-103">ASP.NET Core bir Windows hizmetinde barındırma</span><span class="sxs-lookup"><span data-stu-id="4204f-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="4204f-104">Tarafından [Luke Latham](https://github.com/guardrex) ve [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="4204f-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="4204f-105">ASP.NET Core uygulaması olarak IIS kullanmadan Windows üzerinde barındırılabilen bir [Windows hizmeti](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="4204f-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="4204f-106">Bir Windows hizmeti olarak barındırıldığında, uygulama yeniden başlatma sonrasında otomatik olarak başlar.</span><span class="sxs-lookup"><span data-stu-id="4204f-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="4204f-107">[Görüntüleme veya indirme örnek kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([nasıl indirileceğini](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4204f-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="4204f-108">Bir Windows hizmetinde bir projeyi dönüştürmeye</span><span class="sxs-lookup"><span data-stu-id="4204f-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="4204f-109">Mevcut bir ASP.NET Core projesini ayarlarsınız bir hizmet olarak çalışacak şekilde ayarlamak için aşağıdaki en düşük değişiklikleri gerekir:</span><span class="sxs-lookup"><span data-stu-id="4204f-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="4204f-110">Proje dosyasında:</span><span class="sxs-lookup"><span data-stu-id="4204f-110">In the project file:</span></span>

   * <span data-ttu-id="4204f-111">Bir Windows varlığını onaylamak [çalışma zamanı tanımlayıcı (RID)](/dotnet/core/rid-catalog) veya eklemek `<PropertyGroup>` , hedef Framework'ü içerir:</span><span class="sxs-lookup"><span data-stu-id="4204f-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      <span data-ttu-id="4204f-112">İçin birden fazla RID yayımlamak için:</span><span class="sxs-lookup"><span data-stu-id="4204f-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="4204f-113">RID noktalı virgülle ayrılmış bir liste sağlar.</span><span class="sxs-lookup"><span data-stu-id="4204f-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="4204f-114">Özellik adını kullanan `<RuntimeIdentifiers>` (çoğul).</span><span class="sxs-lookup"><span data-stu-id="4204f-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="4204f-115">Daha fazla bilgi için [.NET Core RID Kataloğu](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="4204f-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="4204f-116">İçin bir paket başvurusu ekleme [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="4204f-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="4204f-117">Aşağıdaki değişiklikleri yapın `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="4204f-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="4204f-118">Çağrı [ana bilgisayar. RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) yerine `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="4204f-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="4204f-119">Çağrı [UseContentRoot](xref:fundamentals/host/web-host#content-root) ve uygulama için bir yol yerine konumu yayımlanan `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="4204f-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. <span data-ttu-id="4204f-120">Kullanarak uygulama yayımlamayı [dotnet yayımlama](/dotnet/articles/core/tools/dotnet-publish), [Visual Studio yayımlama profilini](xref:host-and-deploy/visual-studio-publish-profiles), veya Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4204f-120">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="4204f-121">Visual Studio kullanırken **FolderProfile** ve yapılandırma **hedef konum** seçmeden önce **Yayımla** düğmesi.</span><span class="sxs-lookup"><span data-stu-id="4204f-121">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

   <span data-ttu-id="4204f-122">Komut satırı arabirimi (CLI) araçlarını kullanarak örnek uygulamayı yayımlamak için çalıştırma [dotnet yayımlama](/dotnet/core/tools/dotnet-publish) proje klasöründeki bir komut isteminde komutu.</span><span class="sxs-lookup"><span data-stu-id="4204f-122">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="4204f-123">RID belirtilmelidir `<RuntimeIdenfifier>` (veya `<RuntimeIdentifiers>`) özelliği proje dosyasının.</span><span class="sxs-lookup"><span data-stu-id="4204f-123">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="4204f-124">Aşağıdaki örnekte, uygulama için sürüm yapılandırmasında yayımlanır `win7-x64` çalışma zamanı sırasında oluşturulan bir klasöre *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="4204f-124">In the following example, the app is published in Release configuration for the `win7-x64` runtime to a folder created at *c:\\svc*:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. <span data-ttu-id="4204f-125">Hizmet kullanımı için bir kullanıcı hesabı oluşturma `net user` komutu:</span><span class="sxs-lookup"><span data-stu-id="4204f-125">Create a user account for the service using the `net user` command:</span></span>

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   <span data-ttu-id="4204f-126">Örnek uygulama için bir kullanıcı hesabı adı ile oluşturun. `ServiceUser` ve parola.</span><span class="sxs-lookup"><span data-stu-id="4204f-126">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="4204f-127">Aşağıdaki komutta `{PASSWORD}` ile bir [güçlü parola](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="4204f-127">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   <span data-ttu-id="4204f-128">Bir gruba kullanıcı eklemeniz gerekiyorsa, kullanın `net localgroup` komutu, burada `{GROUP}` grubunun adıdır:</span><span class="sxs-lookup"><span data-stu-id="4204f-128">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   <span data-ttu-id="4204f-129">Daha fazla bilgi için [hizmeti kullanıcı hesaplarını](/windows/desktop/services/service-user-accounts).</span><span class="sxs-lookup"><span data-stu-id="4204f-129">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

1. <span data-ttu-id="4204f-130">Uygulamanın klasörüne yazma/okuma/yürütme erişimi vermek kullanarak [icacls](/windows-server/administration/windows-commands/icacls) komutu:</span><span class="sxs-lookup"><span data-stu-id="4204f-130">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * <span data-ttu-id="4204f-131">`{PATH}` &ndash; Uygulamanın klasörün yolu.</span><span class="sxs-lookup"><span data-stu-id="4204f-131">`{PATH}` &ndash; Path to the app's folder.</span></span>
   * <span data-ttu-id="4204f-132">`{USER ACCOUNT}` &ndash; Kullanıcı hesabı (SID).</span><span class="sxs-lookup"><span data-stu-id="4204f-132">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
   * <span data-ttu-id="4204f-133">`(OI)` &ndash; Nesne devral bayrağı dosyaları alt izinleri yayar.</span><span class="sxs-lookup"><span data-stu-id="4204f-133">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
   * <span data-ttu-id="4204f-134">`(CI)` &ndash; Kapsayıcı devral bayrağı, alt klasörler için izinleri yayar.</span><span class="sxs-lookup"><span data-stu-id="4204f-134">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
   * <span data-ttu-id="4204f-135">`{PERMISSION FLAGS}` &ndash; Uygulamanın erişim izinlerini ayarlar.</span><span class="sxs-lookup"><span data-stu-id="4204f-135">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
     * <span data-ttu-id="4204f-136">Yazma (`W`)</span><span class="sxs-lookup"><span data-stu-id="4204f-136">Write (`W`)</span></span>
     * <span data-ttu-id="4204f-137">Okuma (`R`)</span><span class="sxs-lookup"><span data-stu-id="4204f-137">Read (`R`)</span></span>
     * <span data-ttu-id="4204f-138">Yürütme (`X`)</span><span class="sxs-lookup"><span data-stu-id="4204f-138">Execute (`X`)</span></span>
     * <span data-ttu-id="4204f-139">Tam (`F`)</span><span class="sxs-lookup"><span data-stu-id="4204f-139">Full (`F`)</span></span>
     * <span data-ttu-id="4204f-140">Değiştir (`M`)</span><span class="sxs-lookup"><span data-stu-id="4204f-140">Modify (`M`)</span></span>
   * <span data-ttu-id="4204f-141">`/t` &ndash; Yinelemeli olarak mevcut alt klasörler ve dosyalar için geçerlidir.</span><span class="sxs-lookup"><span data-stu-id="4204f-141">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

   <span data-ttu-id="4204f-142">Örnek uygulamayı yayımlanan *c:\\svc* klasörü ve `ServiceUser` hesap yazma/okuma/Yürütme izinleri, aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="4204f-142">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   <span data-ttu-id="4204f-143">Daha fazla bilgi için [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="4204f-143">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

1. <span data-ttu-id="4204f-144">Kullanım [sc.exe](https://technet.microsoft.com/library/bb490995) hizmeti oluşturmak için komut satırı aracı.</span><span class="sxs-lookup"><span data-stu-id="4204f-144">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="4204f-145">`binPath` Değerdir yürütülebilir dosya adını içeren uygulamanın yürütülebilir dosyanın yolu.</span><span class="sxs-lookup"><span data-stu-id="4204f-145">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="4204f-146">**Eşittir işareti ve tırnak karakteri her bir parametre ve değer arasında gerekli bir alandır.**</span><span class="sxs-lookup"><span data-stu-id="4204f-146">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * <span data-ttu-id="4204f-147">`{SERVICE NAME}` &ndash; Hizmete atanacak ad [Hizmet Denetimi Yöneticisi](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="4204f-147">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
   * <span data-ttu-id="4204f-148">`{PATH}` &ndash; Hizmet yürütülebilir dosya yolu.</span><span class="sxs-lookup"><span data-stu-id="4204f-148">`{PATH}` &ndash; The path to the service executable.</span></span>
   * <span data-ttu-id="4204f-149">`{DOMAIN}` (veya makinenin etki alanı yoksa katılmış yerel makine adını) ve `{USER ACCOUNT}` &ndash; etki alanı (veya yerel makine adı) ve hizmetin altında çalıştığı kullanıcı hesabı.</span><span class="sxs-lookup"><span data-stu-id="4204f-149">`{DOMAIN}` (or if the machine isn't domain joined, the local machine name) and `{USER ACCOUNT}` &ndash; The domain (or local machine name) and user account under which the service runs.</span></span> <span data-ttu-id="4204f-150">Yapmak **değil** atlamak `obj` parametresi.</span><span class="sxs-lookup"><span data-stu-id="4204f-150">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="4204f-151">İçin varsayılan değer `obj` olduğu [LocalSystem hesabı](/windows/desktop/services/localsystem-account) hesabı.</span><span class="sxs-lookup"><span data-stu-id="4204f-151">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="4204f-152">Altında bir hizmeti çalıştıran `LocalSystem` hesabı önemli bir güvenlik riski sunar.</span><span class="sxs-lookup"><span data-stu-id="4204f-152">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="4204f-153">Her zaman bir hizmeti bir kullanıcı hesabı altında sunucu üzerinde sınırlı ayrıcalıklarla çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="4204f-153">Always run a service under a user account with restricted privileges on the server.</span></span>
   * <span data-ttu-id="4204f-154">`{PASSWORD}` &ndash; Kullanıcı hesabı parolası.</span><span class="sxs-lookup"><span data-stu-id="4204f-154">`{PASSWORD}` &ndash; The user account password.</span></span>

   <span data-ttu-id="4204f-155">Aşağıdaki örnekte:</span><span class="sxs-lookup"><span data-stu-id="4204f-155">In the following example:</span></span>

   * <span data-ttu-id="4204f-156">Adlı hizmetin **MyService**.</span><span class="sxs-lookup"><span data-stu-id="4204f-156">The service is named **MyService**.</span></span>
   * <span data-ttu-id="4204f-157">Yayınlanan hizmet bulunan *c:\\svc* klasör.</span><span class="sxs-lookup"><span data-stu-id="4204f-157">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="4204f-158">Uygulama yürütülebilir dosyası adlı *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="4204f-158">The app executable is named *AspNetCoreService.exe*.</span></span> <span data-ttu-id="4204f-159">`binPath` Değerine düz tırnak işaretleri (") içine alınır.</span><span class="sxs-lookup"><span data-stu-id="4204f-159">The `binPath` value is enclosed in straight quotation marks (").</span></span>
   * <span data-ttu-id="4204f-160">Altında çalışacağı `ServiceUser` hesabı.</span><span class="sxs-lookup"><span data-stu-id="4204f-160">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="4204f-161">Değiştirin `{DOMAIN}` kullanıcı hesabının etki alanı veya yerel makine adı.</span><span class="sxs-lookup"><span data-stu-id="4204f-161">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="4204f-162">İçine `obj` düz tırnak (") değeri.</span><span class="sxs-lookup"><span data-stu-id="4204f-162">Enclose the `obj` value in straight quotation marks (").</span></span> <span data-ttu-id="4204f-163">Örnek: barındıran sistemde adlı bir yerel makineye ise `MairaPC`ayarlayın `obj` için `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="4204f-163">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
   * <span data-ttu-id="4204f-164">Değiştirin `{PASSWORD}` ile kullanıcı hesabının parolası.</span><span class="sxs-lookup"><span data-stu-id="4204f-164">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="4204f-165">`password` Değerine düz tırnak işaretleri (") içine alınır.</span><span class="sxs-lookup"><span data-stu-id="4204f-165">The `password` value is enclosed in straight quotation marks (").</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="4204f-166">Parametreleri eşittir işareti ve parametrelerin değerleri arasında boşluk bulunmadığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="4204f-166">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

1. <span data-ttu-id="4204f-167">Hizmetle başlar `sc start {SERVICE NAME}` komutu.</span><span class="sxs-lookup"><span data-stu-id="4204f-167">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="4204f-168">Örnek uygulama hizmeti başlatmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="4204f-168">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="4204f-169">Komut hizmeti başlatmak için birkaç saniye sürer.</span><span class="sxs-lookup"><span data-stu-id="4204f-169">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="4204f-170">Hizmet durumunu denetlemek için kullanmak `sc query {SERVICE NAME}` komutu.</span><span class="sxs-lookup"><span data-stu-id="4204f-170">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="4204f-171">Durumu aşağıdaki değerlerden biri olarak bildirilir:</span><span class="sxs-lookup"><span data-stu-id="4204f-171">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="4204f-172">Örnek uygulama hizmeti durumunu denetlemek için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="4204f-172">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="4204f-173">Hizmet olduğunda `RUNNING` durum ve hizmeti bir web uygulaması ise, uygulama, bir yola göz atın (varsayılan olarak, `http://localhost:5000`, hangi yönlendiren `https://localhost:5001` kullanırken [HTTPS yeniden yönlendirmesi ara yazılım](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="4204f-173">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="4204f-174">Örnek app service için uygulamaya Gözat `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="4204f-174">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="4204f-175">Hizmetle Durdur `sc stop {SERVICE NAME}` komutu.</span><span class="sxs-lookup"><span data-stu-id="4204f-175">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="4204f-176">Aşağıdaki komut örnek uygulama hizmetini durdurur:</span><span class="sxs-lookup"><span data-stu-id="4204f-176">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="4204f-177">Hizmeti ile bir hizmeti durdurmak için bir kısa bir gecikmeyle kaldırmanız `sc delete {SERVICE NAME}` komutu.</span><span class="sxs-lookup"><span data-stu-id="4204f-177">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="4204f-178">Örnek uygulama hizmeti durumunu kontrol edin:</span><span class="sxs-lookup"><span data-stu-id="4204f-178">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="4204f-179">Örnek uygulama hizmeti olduğunda `STOPPED` durum, örnek uygulama hizmeti kaldırmak için aşağıdaki komutu kullanın:</span><span class="sxs-lookup"><span data-stu-id="4204f-179">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="4204f-180">Uygulama dışında bir hizmet çalıştırma</span><span class="sxs-lookup"><span data-stu-id="4204f-180">Run the app outside of a service</span></span>

<span data-ttu-id="4204f-181">Çağıran kod eklemek için her zamanki şekilde bir hizmet dışında çalışırken hata ayıklama ve test etmek daha kolay `RunAsService` yalnızca belirli koşullar altında.</span><span class="sxs-lookup"><span data-stu-id="4204f-181">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="4204f-182">Örneğin, bir konsol uygulaması olarak uygulama çalıştırabilirsiniz bir `--console` komut satırı bağımsız değişkeni veya hata ayıklayıcı eklenir:</span><span class="sxs-lookup"><span data-stu-id="4204f-182">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="4204f-183">ASP.NET Core yapılandırma komut satırı bağımsız değişkenleri için ad-değer çiftleri gerektirdiğinden `--console` anahtar için bağımsız değişkenler geçirilmeden önce kaldırılır [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="4204f-183">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="4204f-184">`isService` gelen geçirilen değil `Main` içine `CreateWebHostBuilder` çünkü imzası `CreateWebHostBuilder` olmalıdır `CreateWebHostBuilder(string[])` sırayla [tümleştirme testi](xref:test/integration-tests) düzgün çalışması için.</span><span class="sxs-lookup"><span data-stu-id="4204f-184">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="4204f-185">Durdurma ve başlatma olaylarını işleme</span><span class="sxs-lookup"><span data-stu-id="4204f-185">Handle stopping and starting events</span></span>

<span data-ttu-id="4204f-186">İşlenecek [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), ve [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) olayları, aşağıdaki ek değişiklikleri yapın:</span><span class="sxs-lookup"><span data-stu-id="4204f-186">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="4204f-187">Türetilen bir sınıf oluşturmanız [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="4204f-187">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="4204f-188">Bir genişletme yöntemi için oluşturma [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) özel Geçiren `WebHostService` için [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="4204f-188">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="4204f-189">İçinde `Program.Main`, yeni bir uzantı metodu çağırma `RunAsCustomService`, yerine [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="4204f-189">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="4204f-190">`isService` gelen geçirilen değil `Main` içine `CreateWebHostBuilder` çünkü imzası `CreateWebHostBuilder` olmalıdır `CreateWebHostBuilder(string[])` sırayla [tümleştirme testi](xref:test/integration-tests) düzgün çalışması için.</span><span class="sxs-lookup"><span data-stu-id="4204f-190">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

<span data-ttu-id="4204f-191">Varsa özel `WebHostService` kod bağımlılık ekleme (örneğin, bir Günlükçü) hizmetinden gerektirir,'ndan elde [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) özelliği:</span><span class="sxs-lookup"><span data-stu-id="4204f-191">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="4204f-192">Ara sunucu ve yük dengeleyici senaryoları</span><span class="sxs-lookup"><span data-stu-id="4204f-192">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="4204f-193">Internet'ten veya kurumsal ağ istekleri etkileşim ve bir proxy'nin arkasındaysa veya yük dengeleyici Hizmetleri ek yapılandırma gerektirebilir.</span><span class="sxs-lookup"><span data-stu-id="4204f-193">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="4204f-194">Daha fazla bilgi için bkz. <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="4204f-194">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="4204f-195">HTTPS yapılandırma</span><span class="sxs-lookup"><span data-stu-id="4204f-195">Configure HTTPS</span></span>

<span data-ttu-id="4204f-196">Hizmet güvenli bir uç nokta ile yapılandırmak için:</span><span class="sxs-lookup"><span data-stu-id="4204f-196">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="4204f-197">Barındırma system, platformun sertifika edinme ve dağıtım mekanizmalarını kullanarak bir X.509 sertifikası oluşturun.</span><span class="sxs-lookup"><span data-stu-id="4204f-197">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>
1. <span data-ttu-id="4204f-198">Belirtin bir [Kestrel sunucu HTTPS uç noktası yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) sertifikayı kullanmak için.</span><span class="sxs-lookup"><span data-stu-id="4204f-198">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="4204f-199">Hizmet uç noktası güvenli hale getirmek için ASP.NET Core HTTPS geliştirme sertifikası kullanılması desteklenmiyor.</span><span class="sxs-lookup"><span data-stu-id="4204f-199">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="4204f-200">Geçerli dizin ve içerik kök</span><span class="sxs-lookup"><span data-stu-id="4204f-200">Current directory and content root</span></span>

<span data-ttu-id="4204f-201">Geçerli çalışma dizini çağırarak döndürülen `Directory.GetCurrentDirectory()` bir Windows hizmeti için *C:\\WINDOWS\\system32* klasör.</span><span class="sxs-lookup"><span data-stu-id="4204f-201">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="4204f-202">*System32* klasör değil bir hizmetin dosyaları (örneğin, ayarları) depolamak için uygun bir konum.</span><span class="sxs-lookup"><span data-stu-id="4204f-202">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="4204f-203">Korumak ve bir hizmetin varlıklar ve ayar dosyaları ile erişmek için aşağıdaki yaklaşımlardan birini kullanın [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) kullanırken bir [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="4204f-203">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="4204f-204">İçerik kök yolu kullanın.</span><span class="sxs-lookup"><span data-stu-id="4204f-204">Use the content root path.</span></span> <span data-ttu-id="4204f-205">`IHostingEnvironment.ContentRootPath` Sağlanan aynı yol `binPath` hizmeti oluşturulduğunda bağımsız değişken.</span><span class="sxs-lookup"><span data-stu-id="4204f-205">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="4204f-206">Yerine `Directory.GetCurrentDirectory()` ayarları dosyalara olan yolları oluşturmak için içerik kök yolu kullanın ve uygulamanın içerik kökünde dosyaların bakımını yapar.</span><span class="sxs-lookup"><span data-stu-id="4204f-206">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="4204f-207">Disk üzerinde uygun bir konumda dosya Store.</span><span class="sxs-lookup"><span data-stu-id="4204f-207">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="4204f-208">Mutlak bir yol belirtin `SetBasePath` dosyaları içeren klasör.</span><span class="sxs-lookup"><span data-stu-id="4204f-208">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4204f-209">Ek kaynaklar</span><span class="sxs-lookup"><span data-stu-id="4204f-209">Additional resources</span></span>

* <span data-ttu-id="4204f-210">[Kestrel'i uç nokta Yapılandırması](xref:fundamentals/servers/kestrel#endpoint-configuration) (HTTPS yapılandırma ve SNI desteği içerir)</span><span class="sxs-lookup"><span data-stu-id="4204f-210">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
