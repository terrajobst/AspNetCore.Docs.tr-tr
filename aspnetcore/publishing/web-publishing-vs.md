---
title: "Oluşturmak için Visual Studio ve MSBuild yayımlama profilleri"
author: rick-anderson
description: "Visual Studio Web yayımlama açıklanır."
keywords: "ASP.NET Core web yayımlama, yayınlama, msbuild, web dağıtımı, dotnet yayımlama, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: f010f9d90165ce4d6718fe1440e600985f21a01d
ms.sourcegitcommit: f33fb9d648a611bb7b2b96291dd2176b230a9a43
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/29/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a>Oluşturma yayımlama profillerini Visual Studio ve MSBuild, ASP.NET Core uygulamaları dağıtmak

Tarafından [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) ve [Rick Anderson](https://twitter.com/RickAndMSFT)

Bu makalede oluşturmak için Visual Studio 2017 kullanmaya odaklanır yayımlama profilleri. Visual Studio ile oluşturulan yayımlama profilleri MSBuild ve Visual Studio 2017 çalıştırabilirsiniz. Makale yayımlama işleminin ayrıntıları sağlar. Bkz: [Visual Studio kullanarak Azure App Service için ASP.NET Core web uygulama yayımlama](xref:tutorials/publish-to-azure-webapp-using-vs) yayımlama için yönergeler için.

Aşağıdaki *.csproj* dosyası komutuyla oluşturuldu `dotnet new mvc`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

`Sdk` Özniteliğini `<Project>` öğesi (ilk satırı) yukarıdaki biçimlendirme aşağıdakileri yapar:

* İçeri aktarmalar `props` dosya *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* başında.
* Hedef dosyadan içeri aktarır *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* sonunda.

Varsayılan konumu `MSBuildSDKsPath` (Visual Studio 2017 kuruluş ile) *% programfiles (x86) %\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* klasör.

`Microsoft.NET.Sdk.Web`aşağıdakilere bağlıdır:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Aşağıdaki neden olan `props` ve `targets` içeri aktarılacak:

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

Hedefleri alma hedefleri doğru yetenek kümesini kullanılan Yayımla yöntemine göre yeniden yayımlayın.

MSBuild veya Visual Studio Proje yüklediğinde, aşağıdaki yüksek düzey eylemleri gerçekleştirilir:

* Proje derleme
* Yayımlanacak dosyaları işlem
* Dosyaları hedefe yayımlama

### <a name="compute-project-items"></a>Proje öğeleri işlem

Proje yüklendiğinde, proje öğeleri (dosyaları) hesaplanır. `item type` Özniteliği dosya nasıl işleneceğini belirler. Varsayılan olarak, *.cs* dosyaları dahil edilmiştir `Compile` öğe listesi. Dosyalar `Compile` öğe listesi derlenir.

`Content` Öğesi listesinin yanı sıra yapı çıkışları yayımlanan dosyaları içerir. Varsayılan olarak, dosyaları düzeni wwwroot eşleşen / ** dahil edilecek `Content` öğesi. [wwwroot / ** genelleme deseni](https://gruntjs.com/configuring-tasks#globbing-patterns) tüm dosyalarda belirtir *wwwroot* klasörü **ve** alt klasörler. Açıkça Yayımla listesine bir dosya eklemek için doğrudan dosyasına ekleyin *.csproj* dosya gösterildiği gibi [dosyaları dahil](#including-files).

Seçtiğinizde, **Yayımla** Visual Studio veya komut satırından yayımladığınızda düğmesini:

- Özellikler/öğeleri hesaplanır (oluşturmak için gereken dosyaları).
- Yalnızca Visual Studio: NuGet paketleri geri yüklenir.  (Geri yükleme CLI kullanıcı tarafından açık olması gerekir.)
- Proje oluşturur.
- Yayımla öğeleri hesaplanır (yayımlama için gerekli olan dosyalar).
- Proje yayımlanır. (Hesaplanan dosyalar Yayımla hedefe kopyalanır.)

## <a name="basic-command-line-publishing"></a>Temel komut satırı yayımlama

Bu bölümde, tüm .NET Core desteklenen platformlarda çalışır ve Visual Studio gerektirmez. Aşağıdaki örnekte `dotnet publish` komutu proje dizinden çalıştırın (içeren *.csproj* dosyası). Proje klasöründe değilseniz, proje dosya yolunda açıkça geçirebilirsiniz. Örneğin:

```console
dotnet publish  c:/webs/web1
```

Oluşturma ve bir web uygulaması yayımlama için aşağıdaki komutları çalıştırın:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET 2.x çekirdek](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET 1.x çekirdek](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

--------------

`dotnet publish` Aşağıdakine benzer bir çıktı üretir:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

Varsayılan klasör yayımlama olduğu `bin\$(Configuration)\netcoreapp<version>\publish`. İçin varsayılan `$(Configuration)` Hata Ayıkla'dır. Yukarıdaki örnekteki `<TargetFramework>` olan `netcoreapp2.0`.

`dotnet publish -h`bilgi yayımlama için yardımı görüntüler.

Aşağıdaki komut belirtir bir `Release` oluşturma ve yayımlama dizini:

```console
dotnet publish -c Release -o C:/MyWebs/test
```

`dotnet publish` Komutu çağrıları `MSBuild` hangi çağırır `Publish` hedef. Herhangi bir parametre geçirilen `dotnet publish` geçirilen `MSBuild`. `-c` Parametresi eşlendiğini `Configuration` MSBuild özelliği. `-o` Parametresi eşlendiğini `OutputPath`.

MSBuild özellikleri aşağıdaki biçimlerden birini kullanarak geçirebilirsiniz:

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

Aşağıdaki komutu yayımlayan bir `Release` bir ağ paylaşımına oluşturun:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Ağ paylaşımı eğik çizgi ile belirtilir (*//r8/*) ve tüm desteklenen .NET Core platformlarında çalışır.

Yayımlanan uygulama dağıtımı için çalışmayan onaylayın. Dosyalar *yayımlama* uygulama çalışırken klasörü kilitlenir. Dağıtım kilitli olduğundan dosyaları kopyalanamaz gerçekleşemez.

## <a name="publish-profiles"></a>Yayımlama profilleri

Bu bölümde Visual Studio 2017 ve daha yüksek yayımlama profillerini oluşturmak için kullanır. Oluşturduktan sonra Visual Studio veya komut satırından yayımlayabilirsiniz.

Yayımlama profilleri yayımlama işlemini basitleştirir. Yayımlama profilleri birden fazla olabilir. Visual Studio'da bir yayımlama profili oluşturmak için çözüm keşfedin projeye sağ tıklayın ve seçin **Yayımla**. Alternatif olarak, seçebileceğiniz **Yayımla \<proje adı >** yapı menüsünde. **Yayımla** uygulama kapasiteleri sayfasında görüntülenir. Proje bir yayımlama profili içermiyorsa, aşağıdaki sayfayı görüntülenir:

![** Yayımla ** sekmesinde Azure, IIS, FTB, seçili Azure ile klasör gösteren uygulama kapasiteleri sayfası. Ayrıca gösterir yeni oluşturabilir ve Exiting radyo düğmeleri seçin](web-publishing-vs/_static/az.png)

Zaman **klasörü** seçili **Yayımla** düğmesi, bir klasör oluşturur yayımlama profili ve yayımlar.

![** Gösteren Azure, IIS, FTB, klasör uygulama kapasiteleri sayfasının sekmesinde Yayımla **](web-publishing-vs/_static/pub1.png)

Bir yayımlama profili oluşturulduktan sonra **Yayımla** sekme değişiklikleri ve seçin **yeni profil oluşturmak** yeni bir profil oluşturmak için.

![** FolderProfile gösteren uygulama kapasiteleri sayfasının sekmesinde Yayımla **-oluşturulan son adımı ve yeni profil bağlantısını oluşturma](web-publishing-vs/_static/create_new.png)

Yayımlama Sihirbazı'nı aşağıdaki Yayımla hedefleri destekler:

- Microsoft Azure uygulama hizmeti
- IIS, FTP, vb. (için herhangi bir web sunucusu)
- Klasör
- Profil alma (içeri aktarılacak bir profili sağlar).
- Microsoft Azure sanal makineler

Bkz: [hangi yayımlama seçeneklerini benim için en uygun?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) daha fazla bilgi için.

Visual Studio ile bir yayımlama profili oluşturduğunuzda, bir *özellikleri/PublishProfiles/\<Yayımla adı > .pubxml* MSBuild dosyası oluşturulur. Bu *.pubxml* dosyası MSBuild dosyadır ve içeren yapılandırma ayarlarını yayımlayın. Yapı özelleştirme ve yayımlama işlemi için bu dosyayı değiştirebilirsiniz. Bu dosya yayımlama işlemi tarafından okunur. `<LastUsedBuildConfiguration>`Genel bir özellik olduğundan ve yapı alınan herhangi bir dosya olmamalıdır özeldir. Bkz: [MSBuild: Yapılandırma özelliğinin nasıl ayarlandığını](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) daha fazla bilgi için.
*.Pubxml* dosyası değil işaretlenmelidir kaynak denetimine bağımlı olduğundan dolayı *.kullanıcı* dosya. *.Kullanıcı* dosya hiç işaretlenmelidir kaynak denetimine hassas bilgileri içerebileceği ve yalnızca bir kullanıcı ve makine için geçerli olur.

(Yayımla parola gibi) hassas bilgileri şifrelenir bir kullanıcı/makine düzeyi ve depolanan başına *özellikleri/PublishProfiles/\<Yayımla adı >. pubxml.user* dosya. Bu dosya hassas bilgileri içerdiğinden gereken **değil** kaynak denetimine iade edilmelidir.

ASP.NET Core üzerinde bir web uygulaması yayımlama genel bakış için bkz: [yayımlama ve dağıtım](index.md). [Yayımlama ve dağıtım](index.md) https://github.com/aspnet/websdk adresindeki bir açık kaynaklı proje.

 `dotnet publish`klasörü, Msdeploy, kullanabilirsiniz ve [KUDU](https://github.com/projectkudu/kudu/wiki) yayımlama profilleri:
 
Klasör (platformlar arası çalışır)`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`

Msdeploy (şu anda bu yalnızca çalışır msdeploy platformlar arası olmadığından Windows'ta):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`

Msdeploy paketi (şu anda bu yalnızca çalışır msdeploy platformlar arası olmadığından Windows'ta):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`

Önceki örnekte **yok** geçirmek `deployonbuild` için `dotnet publish`.

Daha fazla bilgi için bkz: [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)

`dotnet publish`herhangi bir platform için Azure yayımlama için KUDU API'lerini destekler. Visual Studio KUDU API'leri ancak desteklenen websdk tarafından çapraz plat yayımlama için Azure'a mu destek yayımlayın.

Bir yayımlama profili Ekle *özellikleri/PublishProfiles* klasöründe aşağıdaki içeriğe sahip:

```xml
<Project>
<PropertyGroup>
                <PublishProtocol>Kudu</PublishProtocol>
                <PublishSiteName>nodewebapp</PublishSiteName>
                <UserName>username</UserName>
                <Password>password</Password>
</PropertyGroup>
</Project>
```

Aşağıdaki komutu çalıştırarak Yayımla içerikleri zip ve KUDU API'lerini kullanarak Azure yayımlayın.

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

Bir yayımlama profili kullanırken aşağıdaki MSBuild özellikleri ayarlayın:

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

Örneğin, adlandırılmış bir profille yayımlarken *FolderProfile* aşağıdaki komutlardan herhangi birini çalıştırabilirsiniz.

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Ne zaman çağırmayı `dotnet build` çağırır `msbuild` yapı çalıştırın ve işlem yayınlamak için. Çağırma `dotnet build` veya `msbuild` bir klasör profilinde geçirdiğinizde temelde eşdeğerdir. MSBuild doğrudan Windows çağrılırken MSBuild .NET Framework sürümünü alır.  MSDeploy, Windows makineler yayımlama için şu anda sınırlıdır. Çağırma `dotnet build` klasörde olmayan profili MSBuild çağırır ve MSBuild klasörü olmayan profilleri MSDeploy kullanır. Çağırma `dotnet build` bir klasör olmayan profilindeki (MSDeploy kullanarak) MSBuild çağırır ve (bile Windows platformu üzerinde çalışırken) bir hatayla sonuçlanır. Bir klasör olmayan profili ile yayımlamak için MSBuild doğrudan çağırabilir.

Aşağıdaki klasörü yayımlama profili Visual Studio ile oluşturulmuş ve bir ağ paylaşımına yayımlar:

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

Not `<LastUsedBuildConfiguration>` ayarlanır `Release`.  Visual Studio'dan yayımlarken `<LastUsedBuildConfiguration>` yapılandırma özellik değeri, yayımlama işlemi başlatıldığında değeri kullanılarak ayarlanır. `<LastUsedBuildConfiguration>` Yapılandırma özelliği özeldir ve içeri aktarılan bir MSBuild dosyasında geçersiz kılınmış döndürmemelidir. Bu özellik komut satırından kılabilirsiniz. Örneğin:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

MSBuild kullanma:

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>MSDeploy uç noktasına komut satırından yayımlama

Daha önce belirtildiği gibi kullanarak yayımlayabilirsiniz `dotnet publish` veya `msbuild` komutu. `dotnet publish`.NET Core bağlamında çalışır. `msbuild`.NET framework gerektirir ve bu nedenle Windows ortamları için sınırlı olur.

En kolay yolu MSDeploy ile yayımlamak için önce bir yayımlama profili Visual Studio 2017 içinde oluşturmak ve komut satırından profil kullanmaktır.

Aşağıdaki örnekte, bir ASP.NET Core web uygulaması oluşturuldu (kullanarak `dotnet new mvc`) ve Azure yayımlama profili Visual Studio ile eklenir.

Çalıştırmadan `msbuild` gelen bir **VS 2017 için geliştirici komut istemi**. Geliştirici komut istemi doğru olacaktır *msbuild.exe* yolundaki ve bazı MSBuild değişkenleri ayarlayın.

MSBuild aşağıdaki söz dizimini kullanır:

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

Alma `Password` gelen  *\<Yayımla adı >. PublishSettings* dosya. İndirebilirsiniz *. PublishSettings* dosya:

- Çözüm Gezgini: Web uygulamasında sağ tıklayın ve **yayımlama profili indirme**.
- Azure Yönetim Portalı: Seçin **Get yayımlama profili** Web uygulaması dikey penceresinden.

`Username`Yayımlama profili bulunabilir.

Aşağıdaki örnek kullanır "Web11112 – Web Dağıtımı" yayımlama profili:

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>Dosyaları dışlama

ASP.NET Core web uygulamaları, derleme yapıları ve içeriğini yayımlarken *wwwroot* klasörü dahil. `msbuild`destekleyen [genelleme desenleri](https://gruntjs.com/configuring-tasks#globbing-patterns). Örneğin, aşağıdaki `<Content>` öğesi biçimlendirme hariç tutmak tüm metni (*.txt*) dosyaları buradan *wwwroot/içerik* klasör ve tüm alt klasörlerini.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Yukarıdaki biçimlendirme bir yayımlama profili eklenebilir veya *.csproj* dosya. Eklendiğinde *.csproj* dosya, kural eklenmiş olup projedeki tüm yayımlama profilleri.

Aşağıdaki `<MsDeploySkipRules>` öğesi biçimlendirme içerenleri tüm dosyaları *wwwroot/içerik* klasörü:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>`değil silecek *atla* dağıtım sitesinden hedefler. `<Content>`hedef dosyalar ve klasörler dağıtım site veritabanından silinir. Örneğin, bir web uygulaması şu dosyaları dağıttığınız varsayın:

- *Views/Home/About1.cshtml*
- *Views/Home/About2.cshtml*
- *Views/Home/About3.cshtml*

Aşağıdaki eklediyseniz `<MsDeploySkipRules>` biçimlendirme, bu dosyaları dağıtım sitesinde silinmez.

``` xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` Yukarıda gösterilen biçimlendirme engeller *atlandı* dosyaları ancak bunlar dağıtıldığında dosyaları silmeyin depoyed engeller.

Aşağıdaki `<Content>` biçimlendirme dağıtım sitede hedeflenen dosyaları silin:

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Komut satırı dağıtımı ile kullanarak `<Content>` biçimlendirme yukarıdaki aşağıdakine benzer bir çıktı neden:

``` console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="including-files"></a>Dosyaları dahil olmak üzere

Aşağıdaki biçimlendirmede dahil etmek için kullanılan bir *görüntüleri* klasörünün dışında proje dizinine *wwwroot/görüntüleri* Yayımla sitesinin klasörüne.

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

İşaretleme eklenebilir *.csproj* dosya veya yayımlama profili. İçin eklediyseniz *.csproj* dosyası, projenin her yayımlama profilinde eklenecektir.

Aşağıdaki biçimlendirme gösterir nasıl vurgulanmış için:

* Projeye dışında dosyasından kopyalama *wwwroot* klasör.
* Dışlama *wwwroot\Content* klasör.
* Dışlama *Views\Home\About2.cshtml*.

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

Bkz: [WebSDK Benioku](https://github.com/aspnet/websdk) daha fazla dağıtım örnekleri için.

### <a name="run-a-target-before-or-after-publishing"></a>Bir hedef önce veya sonra yayımlama çalıştırın

Yerleşik `BeforePublish` ve `AfterPublish` hedefleri, bir hedef önce veya sonra yayımlama hedefi yürütmek için kullanılabilir. Aşağıdaki biçimlendirmede önce ve sonra yayımlama konsol çıktısı iletilerini günlüğe kaydetmek için yayımlama profili eklenebilir:

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>Kudu hizmeti

Azure Web uygulamanızın dosyalarını görüntülemek için kullanın [kudu hizmet](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Append `scm` adı ya da Web uygulamanızı belirteci. Örneğin:

| URL               | Sonuç|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | Web uygulaması |
| `http://mysite.scm.azurewebsites.net/` | Kudu hizmeti |

Seçin [hata ayıklama Konsolu'nda](https://github.com/projectkudu/kudu/wiki/Kudu-console) dosyaları görünüm/düzenleme/silme/Ekle menü öğesine.

## <a name="additional-resources"></a>Ek kaynaklar

- [Web dağıtımı](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) IIS sunucuları için Web uygulamaları ve Web siteleri dağıtımını basitleştirir.

- [https://github.com/ASPNET/websdk](https://github.com/aspnet/websdk/issues): dosya sorunları ve dağıtım için özelliklerini isteyin.
