`FetchData` bileşen şunları gösterir:

* Erişim belirteci sağlayın.
* *Sunucu* uygulamasında korumalı bir kaynak API 'si çağırmak için erişim belirtecini kullanın.

`@attribute [Authorize]` yönergesi, kullanıcının bu bileşeni ziyaret edebilmek için yetkilendirilmiş olması gereken Blazor WebAssembly yetkilendirme sistemine işaret ediyor. *İstemci* uygulamasındaki özniteliğin varlığı, sunucudaki API 'nin doğru kimlik bilgileri olmadan çağrılmasına engel olmaz. *Sunucu* uygulamasının aynı zamanda uygun uç noktalar üzerinde `[Authorize]` kullanması gerekir ve bunları doğru şekilde korur.

`AuthenticationService.RequestAccessToken();`, API 'yi çağırmak için isteğe eklenebilen bir erişim belirteci isteme işlemini gerçekleştirir. Belirteç önbelleğe alınmışsa veya hizmet Kullanıcı etkileşimi olmadan yeni bir erişim belirteci sağlayabiliyor ise, belirteç isteği başarılı olur. Aksi takdirde, belirteç isteği başarısız olur.

İsteğe dahil edilecek gerçek belirteci almak için, uygulamanın `tokenResult.TryGetToken(out var token)`çağırarak isteğin başarılı olduğunu denetlemesi gerekir. 

İstek başarılı olduysa, belirteç değişkeni erişim belirteciyle doldurulur. Belirtecin `Value` özelliği, `Authorization` istek başlığına dahil etmek için sabit dizeyi gösterir.

Belirteç Kullanıcı etkileşimi olmadan sağlanamadığından istek başarısız olduysa, belirteç sonucu bir yeniden yönlendirme URL 'SI içerir. Bu URL 'ye gidildiğinde, Kullanıcı oturum açma sayfasına geçer ve başarılı bir kimlik doğrulamasından sonra geçerli sayfaya geri dönün.

```razor
@page "/fetchdata"
...
@attribute [Authorize]

...

@code {
    private WeatherForecast[] forecasts;

    protected override async Task OnInitializedAsync()
    {
        var httpClient = new HttpClient();
        httpClient.BaseAddress = new Uri(Navigation.BaseUri);

        var tokenResult = await AuthenticationService.RequestAccessToken();

        if (tokenResult.TryGetToken(out var token))
        {
            httpClient.DefaultRequestHeaders.Add("Authorization", 
                $"Bearer {token.Value}");
            forecasts = await httpClient.GetJsonAsync<WeatherForecast[]>(
                "WeatherForecast");
        }
        else
        {
            Navigation.NavigateTo(tokenResult.RedirectUrl);
        }

    }
}
```

Daha fazla bilgi için bkz. [bir kimlik doğrulama işleminden önce uygulama durumunu kaydetme](xref:security/blazor/webassembly/additional-scenarios#save-app-state-before-an-authentication-operation).
