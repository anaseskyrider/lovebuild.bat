# lovebuild.bat
Batch script that automates the basic process of creating a LOVE2D executable file from your game folder.

When I was learning how to automate the build process for my current game project, I was annoyed with how many steps were involved with all of the solutions everyone else provides. Stuff like LuaRocks and these compilers and other programs. I straight up don't understand how to install and use them.

So I learned batch and made a batch script in half a day instead. It automates the process of building your game into an executable. No packages, no programs, no other complicated stuff, just a single script. It even copies the .dlls you need. All you have to do is set the variables with the correct formatting listed in the file, make sure it works, and package your game folder however you like. If you're like me and don't know much batch syntax, look at the examples.

## lovebuild.bat
This script WILL overwrite the files in your output build folder, so if you want to archive your builds, backup the contents.

	@echo off
	REM User settings
	REM If you want paths relative to the .bat script's location, use %CD% or %~dp0, or use the full file paths. However you do it, you MUST include a `\` at the end. %~dp0 appends a `\` at the end, %CD% does not.
	REM Do not add quotation marks around your folder or file names.

	set src=<directory that contains main.lua>
	set love=<directory that contains love.exe>
	set build=<directory to output your .exe to>
	set name=<name of your game>

	REM Execute program
	set output=%love%%name%
	powershell Compress-Archive -Path '%src%*' -DestinationPath '%output%.zip' -Force
	move "%output%.zip" "%output%.love"
	copy /b "%love%love.exe"+"%output%.love" "%build%%name%.exe"
	xcopy "%love%love.dll" "%build%" /r /q
	xcopy "%love%SDL2.dll" "%build%" /r /q
	xcopy "%love%lua51.dll" "%build%" /r /q
	xcopy "%love%OpenAL32.dll" "%build%" /r /q
	xcopy "%love%mpg123.dll" "%build%" /r /q
	del "%output%.love"
[lovebuild.bat](https://github.com/anaseskyrider/lovebuild.bat/blob/main/lovebuild.bat)

## Example
Folder Structure:

	C:\Users\Admin\Desktop\Game Project\
	+-- Build
	+-- Love
	|   +-- love.exe
	+-- Source
	|   +-- libs
	|   +-- main.lua
	+-- lovebuild.bat

### lovebuild.bat (relative paths)
A relative path ensures that the location of your project folder is irrelevant to the functioning of this build-automation, so long as the rest of your project folder structure remains the same. It is the setup I recommend. If you happen to place your script elsewhere and use a relative path, use ..\ to go up one level.

	set src=%~dp0Source\
	set love=%~dp0Love\
	set build=%~dp0Build\
	set name=Giga Galactic Giants

### lovebuild.bat (absolute paths)

	set src=C:\Users\Admin\Desktop\Game Project\Source\
	set love=C:\Users\Admin\Desktop\Game Project\Love\
	set build=C:\Users\Admin\Desktop\Game Project\Build\
	set name=Giga Galactic Giants

When you run this script, inside **Game Project\Build\** will be your game executable **Giga Galactic Giants.exe** along with all the .dll files you need to run the game.
