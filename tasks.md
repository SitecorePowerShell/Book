# Task Scheduler

The task scheduler is a great way to run scripts in a periodic fashion. You may find the need to automatically archive log files into a compressed file. Perhaps send an email with a generated report based on stale site content. To help make the setup simple, we've provided a *Task Command*.

![PowerShell Script Command](images/screenshots/tasks-powershellscriptcommand.png)

The command shown above is simply a type exposed as a public method in the *Cognifide.PowerShell* assembly. There exists an update method which accepts one or more items and executes the associated script.

Beneath *Schedules* you can create as many tasks as Sitecore will allow. Configure the *Command* and *Items* fields like that shown below.

![PowerShell Script Task](images/screenshots/tasks-archiveschedule.png)

The *Items* field contains the path to a script in the *Script Library*. 

Below are some of the scripts found out-of-the-box with SPE.

| Module | Script |
| ------ | ---- |
| System Maintenance | Archive Sitecore logs, Clean up deleted items older than 30 days |
| User Session Management | Remove idle user sessions |

#### Create and Manage Tasks

We've added a context menu item to provide you with a shortcut to the Task Editor.



