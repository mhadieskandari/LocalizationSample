How to work with Localization in asp.net core mvc

first : you must be write some configuration in startup.cs
some code like this in ConfigureServices method:

    public void ConfigureServices(IServiceCollection services)
        {
            //Dependency Injection for localization
            services.AddLocalization(options => options.ResourcesPath = "Resources"); 
            services.AddMvc().AddViewLocalization(LanguageViewLocationExpanderFormat.Suffix)
            .AddDataAnnotationsLocalization(options => {
             options.DataAnnotationLocalizerProvider = (type, factory) => factory.Create(typeof(SharedResource));
            });
        }

   and some code like this in Configure method :

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
        {
            //add culture that you want
            var supportedCultures = new[]
            {
                new CultureInfo("en-US"),
                new CultureInfo("fa-IR")
            };
            //set DefalutCulture
            app.UseRequestLocalization(new RequestLocalizationOptions
            {
                DefaultRequestCulture = new RequestCulture("en-US"),
                // Formatting numbers, dates, etc.
                SupportedCultures = supportedCultures,
                // UI strings that we have localized.
                SupportedUICultures = supportedCultures
            }); 
        }


you must be create a directory in root of project . in this case i create directory with "Resources" name and create dictionary with controllers name for every Culture that you want  like this:

//this name is that path you config in startup.cs
 
/Resources/Controllers.HomeController.en-US.resx
/Resources/Controllers.HomeController.fa-IR.resx

in this case you must be create resource for every controller for every Culture you want.
