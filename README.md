# PS_Https

Information passed between client and server is encrypted with SSL / TLS

HTTPS: Secure Protocol Over Http

SSL: Seure Socket Layer

TLS: Transport Level Security(Successor of SSL)(Recommended)

SSL and TLS are the security protocols do the encryption for security

![image](https://user-images.githubusercontent.com/20635839/126878220-870e4b83-2c48-4711-af79-a0bfb74d9dbf.png)


To enable Https and force redirection in .Net framework, Canges neeed to be done in the filter.config
```C#
  public class FilterConfig
    {
        public static void RegisterGlobalFilters(GlobalFilterCollection filters)
        {
            filters.Add(new RequireHttpsAttribute());
            filters.Add(new HandleErrorAttribute());
        }
    }
```

```C#
  public void ConfigureServices(IServiceCollection services)
        {
            services.AddControllersWithViews();
            services.AddHttpsRedirection((httpOptions)=>
            {
                httpOptions.RedirectStatusCode=340;
                httpOptions.HttpsPort = 443;
            });
        }
```

```C#
   public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Home/Error");
                // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
                //app.UseHsts();
            }
            app.UseHttpsRedirection();
            app.UseStaticFiles();

            app.UseRouting();

            app.UseAuthorization();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllerRoute(
                    name: "default",
                    pattern: "{controller=Home}/{action=Index}/{id?}");
            });
        }
        
```
Handling first request to HTTP
HSTS handles such issue, to ensure that the site will be only served under HTTPS
Even when the http bookmark is http, thr browser internally converts it to HTTPS before sending the request.

![image](https://user-images.githubusercontent.com/20635839/126879556-9971565b-f349-42e9-b820-de86aab10f56.png)

For ASP.Net Core App
```C#
  public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            if (env.IsDevelopment())
            {
                app.UseDeveloperExceptionPage();
            }
            else
            {
                app.UseExceptionHandler("/Home/Error");
                // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
                app.UseHsts();
            }
         }
```

```C#
      services.AddHsts(hSTSOptions =>
            {
                hSTSOptions.IncludeSubDomains = true;
                hSTSOptions.MaxAge = TimeSpan.FromDays(30);
                hSTSOptions.Preload = true;
                //hSTSOptions.ExcludedHosts.Clear();// only Demo
            });
``` 

https://hstspreload.org/

