# what does this tutorial explain?
- how to generate HTML documentation using sphinx for gdscipt files
    - write documentation comments
    - export XML class references using --doctool --gdscript-docs
    - generate RST files using make_rst.py
        - modify script
    - generate HTML documentation using sphinx
        - install extension
        - install theme



# tutorial

## 1. Add Documentation Comments
The first step is to document your scripts according to this doc page: https://docs.godotengine.org/en/stable/tutorials/scripting/gdscript/gdscript_documentation_comments.html

GDScript documentation comments are generally put after "##" immediately preceding the element you are documenting

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/aa5cc0fc-a986-438d-a7db-5cc83fc81bed)

## 2. Ensure Godot CLI is Configured
In order to export the class references based on the documentation comments created in step 1, we need to be able to use the Godot command line tool. This doc page covers command line use for Godot: https://docs.godotengine.org/en/stable/tutorials/editor/command_line_tutorial.html

If you didn't install Godot with scoop, and you want to make your existing installation work with the command line, this is how you do it:

First, locate where your Godot executable is (for me this is C:\Program Files\Godot):

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/4276c863-2f83-44d8-b580-77c4c3154910)

Next, add this directory to your PATH environment variable:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/bd01b88b-3a93-4f54-a886-2dfb8a803cee)

Finally, rename the godot executable to "godot.exe". This allows you to access command line tools by just typing "godot" from any directory.

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/dc26a3e5-4336-4f33-be74-82f4f6476c27)

You can verify this worked by running ```godot --version``` from the command line

## 3. Export XML Class References
Now that that godot works over the command line, we will use the ```--doctool``` command to generate the XML files. ```--doctool``` is what is used to create the engine documentation, and by default will output an XML file for every class in the engine. However, if you use the ```--gdscript-docs``` command with it, it will output XML files only for gdscript files found at the path you provide.

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/c1e1b568-c7a4-4ce6-8591-e8897b79f68f)

The full command we will run is ```godot --doctool <path1> --gdscript-docs <path2>```
- ```godot``` accesses the godot command line tool
- ```--doctool <path1>``` tells doctool to output the XML files to the location at ```<path1>```
- ```--gdscript-docs <path2>``` tells doctool to only generate XML files for scripts found in the location ```<path2>```

<path1> and <path2> can be whatever you want them to be. but I will explain how I do it, to minimize confusing paths

First, navigate to whatever folder contains the scripts that you want to generate documentation for in your file explorer. For this example, I have all of the scripts in the project folder Documents\Games\Godot\TestGrounds:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/d212e760-d6fd-4207-802b-c8eeb5551fca)

Next, create a folder the XML files will be saved to. Here I made the folder "doc_export":

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/f5a32f6b-f4ed-4d8c-8e07-ec406caaa90b)

Next, open the command prompt and navigate to the folder that contains your scripts (Documents\Games\Godot\TestGrounds):

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/cf61435f-098b-4ad9-829c-40da345752d8)

Finally, we can run our command,```godot --doctool <path1> --gdscript-docs <path2>``` where ```<path1>``` is "doc_export" and ```<path2>``` is the current directory, or "."
That means the final command, for the example, is ```godot --doctool doc_export --gdscript-docs .```

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/ce7acd6a-a240-42c0-b3be-0d3b86024b58)

When it runs, it will create a headless session of godot (i think) so just give it a few seconds, and then you can exit using ctrl+C in the command line.

If it was successful, you should have an XML file for each script in your doc_export folder:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/b3815bb2-b973-4e66-97ac-52e1818ada0b)

## 4. Generate RST Files
The XML files we generated in step 3 are "class references" for each script that we documented. We can use these class references to generate reStructuredText (RST) files using the make_rst python tool included in the engine source. The file itself can be found here: https://github.com/godotengine/godot/blob/master/doc/tools/make_rst.py

However, in order to run the make_rst.py tool, you need to clone the whole godot repo. This is because make_rst.py depends on other parts of the repo, specifically the "import version".

Additionally, we will need to make 2 adjustments to the make_rst.py script once the repo is downloaded. The adjusted file is also available in this repo if you would rather just replace it.

