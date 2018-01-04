# Serilog
Passa a passo para implementação do serilog em WebAPIs com .NetCore 2.0

## Setup

Adicionar as dependências no projeto onde se encontra o WebAPI

Serilog
Serilog.Extensions.Logging
Serilog.Sinks.RollingFile
Serilog.Sinks.File

Adicionar o metodo abaixo na Startup class
```
private void ConfigurarLog(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{

   var appname = this.GetType().FullName.Replace($".{this.GetType().Name}", "");

   Log.Logger = new LoggerConfiguration()
     .MinimumLevel.Debug()
     .WriteTo.RollingFile($"logs/{appname}-{{Hour}}.txt")
     .CreateLogger();

   Serilog.Debugging.SelfLog.Enable(Console.Error);
   loggerFactory.AddSerilog();

   if (env.IsDevelopment())
   {
      app.UseDeveloperExceptionPage();
      loggerFactory.AddConsole();
      loggerFactory.AddDebug();
   }
}
```
Add o trecho de código abaixo no metodo Configure da Startup Class
```
ConfigurarLog(app, env, loggerFactory);
```
Add o parametro abaixo no metodo configure da Startup Class
```
ILoggerFactory loggerFactory
```
