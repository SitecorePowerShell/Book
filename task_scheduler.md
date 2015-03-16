# Task Scheduler

The task scheduler is a great way to run scripts in a period fashion. One example is to have a task that automatically archive log files into a compressed file. Another example may be to email a generated report based on stale site content. We've provided a Task Command for creating new scheduled tasks.

![PowerShell Script Command](TaskScheduler.png)

The command shown above is simply a type exposed as a public method in the Cognifide.PowerShell assembly. There exists an update method which accepts one or more items and executes the associated script.