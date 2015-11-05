Načtení obrázku skupiny v O365
===

```csharp
async public static Task<BitmapImage> LoadGroupImage(string groupId)
{
	// https://graph.microsoft.com/beta/contoso.onmicrosoft.com/groups/01d360b6-2487-43f2-b968-180754ef4c7e/GroupPhoto/$value

	HttpClient hc = await GetHttpClient();
	var resp = await hc.GetAsync($"[tenant].onmicrosoft.com/groups/{groupId}/GroupPhoto/$value");

	var imageArray = await resp.Content.ReadAsByteArrayAsync();
	using (InMemoryRandomAccessStream stream = new InMemoryRandomAccessStream())
	{
		await stream.WriteAsync(imageArray.AsBuffer(0, imageArray.Length));
		stream.Seek(0);

		BitmapImage image = new BitmapImage();

		await image.SetSourceAsync(stream);

		return image;
	}
}

async public static Task<HttpClient> GetHttpClient()
{
	var token = await AuthenticationHelper.GetTokenAsync();

	HttpClient hc = new HttpClient();
	hc.BaseAddress = new Uri("https://graph.microsoft.com/beta/");
	hc.DefaultRequestHeaders.Add("Authorization", "Bearer " + token);

	return hc;
}
```