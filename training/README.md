# Training

Sitecore PowerShell Extensions module has so much to offer, but sometimes those new to the module may find it difficult to know where to start. The following book should provide you with enough information to use and be productive with SPE. 

Don't worry, you will be able to use it without having to write any code.

## Training Material

We have a video series available to help walk you through the module [here](https://www.youtube.com/playlist?list=PLph7ZchYd_nCypVZSNkudGwPFRqf1na0b).

We also maintain a comprehensive list of links to [blogs and videos](https://blog.najmanowicz.com/sitecore-powershell-console/) to help you on your journey to SPE awesomeness. Happy coding!

## Syntax Comparison

| Microsoft .Net C# | Windows PowerShell |
| :--- | :--- |
| // Assign data to a new variable <br/> var name = "Michael"; | # Assign data to a new variable <br/>$name = "Michael" |
| // Perform simple math <br /> var total = 1 + 1; | # Perform simple math <br/>$total = 1 + 1 |
| // Create a new list of strings <br/>var names = new List<string>(); <br/>names.Add("Michael"); <br/>names.Add("Adam"); | # Create a new list of strings <br/>$names = @() <br/>$names += "Michael" <br/>$names += "Adam" |
| // Create a hashtable of data <br/>var table = new Hashtable();<br/>table["Name"] = "Michael";<br/>table["Age"] = 33; | # Create a new hashtable of data <br/>$table = @{}<br/>$table["Name"] = "Michael"<br/>$table["Age"] = 33 |
| // Check if the string is null or empty using a static method <br/>if(string.IsNullOrEmpty(name)) { … }<br/> | # Check if the string is null or empty using a static method <br/>if([string]::IsNullOrEmpty($name)) { … } |
| /* <br/>&nbsp;&nbsp;&nbsp;&nbsp;Create a comment block<br/>*/ | <# <br/>&nbsp;&nbsp;&nbsp;&nbsp;Create a comment block<br/>#> |
| // Loop through a list of strings <br/>foreach(var name in names) { … } | # Loop through a list of strings <br/>foreach($name in $names) { … } |
| // Compare values <br/>name == "Michael"<br/>total <= 3<br/>names.Count() > 2 && name[0] != "Adam" | # Compare values <br/>$name -eq "Michael" # case-insensitive<br/>$total -le 3<br/>$names.Count() -gt 2 –and $name[0] -ne "Adam" |
| // Negate value <br/>var isTrue = !false; | # Negate value<br/>$isTrue = !$false<br/>$isTrue = -not $false |
| // String interpolation <br/>var message = $"Hello, {name}"; | # String interpolation <br/>$message = "Hello, $($name)" |
| // Access instance property <br/>var today = DateTime.Today; | # Access instance property <br/>$today = [datetime]::Today |

As you can see in the table above, the language syntaxes are not all that different. Within a few minutes 
you might even be able to translate code from your library classes into SPE scripts.
