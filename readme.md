Asp.net MVC rendering engine using ClearScript with popular javascript rendering engines

using (var engine = new V8ScriptEngine())
{
    engine.Execute("var window = {}"); // easiest way I found to keep the nunjucks reference somewhere
 
    using (var client = new WebClient())
    {
        // get the nunjucks js source
        var nunjucks = client.DownloadString("http://jlongster.github.io/nunjucks/files/nunjucks.js");
        // run it
        engine.Evaluate(nunjucks);
    }
 
    // use the Script method that dynamically hosts the js objects (rather slow ~200ms on my machine):
    var renderIt1 = engine.Script.window.nunjucks.renderString("hello {{username}}", new { username = "James" });
    
    // use a plain evaluate method for best performance (fast ~1 ms on my machine):
    var renderIt2 = engine.Evaluate("window.nunjucks.renderString('hello {{username}}',  { username : 'James' })");
    
}