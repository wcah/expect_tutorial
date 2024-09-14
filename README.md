# expect_tutorial
Tutorial for Expect, for automating interactive processes.
From the Expect manual:

Expect is a program that "talks" to other interactive programs according to a script. Following the script, Expect knows what can be expected from a program and what the correct response should be. An interpreted language provides branching and high-level control structures to direct the dialogue. In addition, the user can take control and interact directly when desired, afterward returning control to the script.


# Autoexpect
Autoexpect will generate an expect script for you with minimal effort.

`$ autoexpect`

After you run this command, Autoexpect will start logging all of your inputs. 
You can end keylogging by entering `CTRL+D`.
Autoexpect will log *exactly* all of the information that comes to and from the command line.
If you want a less-verbose file, run `$ autoexpect -p` to only log the prompts that you are responding to.
Autoexpect generates a file called script.exp in your current working directory.

# your .exp file

script.exp will likely need some tweaks to do exactly what you want.
In this example, Expect is automatically logging in to a remote server.
However, `CTRL+D` will log you out of the remote server, so your expect script will include a logout.
You'll need to edit your script.exp to remove the logout commands and instead add an `interact` line.
`interact` tells Expect that you want to interact with the terminal. 
Without this command, anything that you enter will be passed to the expect script instead.
In our example ssh scenario, this means that you will be logged in to the server but Expect will be blocking all of your inputs. 
`input` tells Expect to get out of your way so that you can actually use the server.

# Expect

Now that you have a script.exp, you can run

`$ expect script.exp`

Expect will now recreate everything that you did while Autoexpect was watching.
Running your script in this manner is the best way to debug and make sure that it's doing everything you want.

# Advanced
## Alias
Aliases are user-defined commands in BASH.
The command `alias` enables you to use shorthand for a command that you use a lot.

`$ alias jump='ssh user@jump.server'`

The above command sets `jump` as a command for you.
When you enter `jump`, you will automatically ssh into the target server.
However, the `alias` command only sets an alias for your session, i.e. your alias will be gone when you close the terminal.
To set a permanent alias, you need to edit your ~/.bashrc file.
Luckily, the necessary syntax is exactly the same as the `alias` command:

`$ echo "alias jump='ssh user@jump.server'" >> ~/.bashrc`

Alternatively:

`$ nano ~/.bashrc`

and add the alias line manually.
After adding an alias, you need to run

`$ source ~/.bashrc`

so that your shell knows where to find it.

## Housekeeping
Combining Expect and aliasing allows you to keep your Expect scripts out of the way.
For example, I have a directory `utility_scripts` in my home directory where I store all of my expect scripts.
My aliases point to `/home/chase/utility_scripts/` so that I can use Expect scripts from anywhere in my system.
