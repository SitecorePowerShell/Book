# Task Scheduler

The task scheduler is a great way to run scripts in a periodic fashion. You may find the need to automatically archive log files into a compressed file. Perhaps send an email with a generated report based on stale site content. To help make the setup simple, we've provided a *Task Command*.

![PowerShell Script Command](images/screenshots/tasks-powershellscriptcommand.png)

The command shown above is simply a type exposed as a public method in the Cognifide.PowerShell assembly. There exists an update method which accepts one or more items and executes the associated script.