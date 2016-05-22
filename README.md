#1 Linq to Sitecore
#2 About
This is a small library to help a developer to map sitecore items to the code-model.

#2 Examples
#3 Code-First Approach
First of all you need to prepare your classes and reflect them in the sitecore items.

Lets create our base object class:

```C#
using LinqToSitecore;
 public class TemplateObject
    {
        [SitecoreSystemProperty(SitecoreSystemPropertyType.Id)]
        public ID Id { get; set; }

        [SitecoreSystemProperty(SitecoreSystemPropertyType.Name)]
        public string Name { get; set; }
    }
```
Base class must define Properties Id and Name which are sitecore base item properties.

Next, create your class:

```C#
using LinqToSitecore;

namespace MyTestClasses
{
    [SitecoreTemplate("{17CF7553-53AD-4260-B1F8-DCD0D9BE363C}")]
    public class MySitecoreItem: TemplateObject
    {
        public string CategoryName { get; set; }
        public string Description { get; set; }
        public string Combination { get; set; }
        public int SumNumber { get; set; }

        [SitecoreField("Custom Field")]
        public bool IsCustomField { get; set; }
    }
}
```
Use class attribute SitecoreTemplate to define Sitecore template which reflects your class. 
Also use SitecoreField property attribute to define a custom sitecore field name (in order if you want to have different names in your code and the sitecore template).

Next, you could call linq-to-sitecore extended methods:
```C#
using LinqToSitecore;
 public class MySitecoreController: MyBaseController
    {
        [HttpGet]
        public ActionResult Output()
        {

            var allPossibleObjects = Sitecore.Context.Database.OfType<MySitecoreItem>();
            var allPossibleObjectsFromSpecificLocation = Sitecore.Context.Database.OfType<MySitecoreItem>("sitecore/content/myitems");
            
            var queriable = Sitecore.Context.Database.Where<MySitecoreItem>(s => s.IsCustomField == true || CategoryName == "BestCategory").ToList();

            var snigleItem  = Sitecore.Context.Database.FirstOrDefault<MySitecoreItem>();
            
            var oneItemToClass = Sitecore.Context.Database.GetItem("some_path_to_item").ReflectTo<MySitecoreItem>();
            
            var itemsToClass = Sitecore.Context.Database.GetItem("some_path_to_item").Children.ToList<MySitecoreItem>();
            
            var SelectItemsToClass = Sitecore.Context.Database.SelectItems("some_path_to_item").ToList<MySitecoreItem>();
            

            return Json(queriable);

```