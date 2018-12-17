# Training

Sitecore PowerShell Extensions module has so much to offer, but sometimes those new to the module may find it difficult to know where to start. The following book should provide you with enough information to use and be productive with SPE.

Don't worry, you will be able to use it without having to write any code.

## Training Material

We have a video series available to help walk you through the module [here](https://www.youtube.com/playlist?list=PLph7ZchYd_nCypVZSNkudGwPFRqf1na0b).

We also maintain a comprehensive list of links to [blogs and videos](https://blog.najmanowicz.com/sitecore-powershell-console/) to help you on your journey to SPE awesomeness. Happy coding!

## Syntax Comparison

| Microsoft .Net C\# | Windows PowerShell |
| :--- | :--- |
| `// Assign data to a new variable   var name = "Michael";` | `# Assign data to a new variable  $name = "Michael"` |
| `// Perform simple math   var total = 1 + 1;` | `# Perform simple math  $total = 1 + 1` |
| `// Create a new list of strings  var names = new List();  names.Add("Michael");  names.Add("Adam");` | `# Create a new list of strings  $names = @()  $names += "Michael"  $names += "Adam"` |
| `// Create a hashtable of data  var table = new Hashtable(); table["Name"] = "Michael"; table["Age"] = 33;` | `# Create a new hashtable of data  $table = @{} $table["Name"] = "Michael" $table["Age"] = 33` |
| `// Check if the string is null or empty using a static method  if(string.IsNullOrEmpty(name)) { … }` | `# Check if the string is null or empty using a static method  if([string]::IsNullOrEmpty($name)) { … }` |
| `/* Create a comment block */` | `<# Create a comment block #>` |
| `// Loop through a list of strings  foreach(var name in names) { … }` | `# Loop through a list of strings  foreach($name in $names) { … }` |
| `// Compare values  name == "Michael" total <= 3 names.Count() > 2 && name[0] != "Adam"` | `# Compare values  $name -eq "Michael" # case-insensitive $total -le 3 $names.Count() -gt 2 –and $name[0] -ne "Adam"` |
| `// Negate value  var isTrue = !false;` | `# Negate value $isTrue = !$false $isTrue = -not $false` |
| `// String interpolation  var message = $"Hello, {name}";` | `# String interpolation  $message = "Hello, $($name)"` |
| `// Access instance property  var today = DateTime.Today;` | `# Access instance property  $today = [datetime]::Today` |

As you can see in the table above, the language syntaxes are not all that different. Within a few minutes you might even be able to translate code from your library classes into SPE scripts.

