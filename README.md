![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/be1892ac-50ca-4a61-9775-36641c561b47)# what does this tutorial explain?
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

First, navigate to whatever folder contains the scripts that you want to generate documentation for in your file explorer. For this example, I have all of the scripts in the project folder ```Documents\Games\Godot\TestGrounds```:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/d212e760-d6fd-4207-802b-c8eeb5551fca)

Next, create a folder the XML files will be saved to. Here I made the folder ```doc_export```:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/f5a32f6b-f4ed-4d8c-8e07-ec406caaa90b)

Next, open the command prompt and navigate to the folder that contains your scripts (```Documents\Games\Godot\TestGrounds```):

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/cf61435f-098b-4ad9-829c-40da345752d8)

Finally, we can run our command,```godot --doctool <path1> --gdscript-docs <path2>``` where ```<path1>``` is ```doc_export``` and ```<path2>``` is the current directory, or ```.```
That means the final command, for the example, is ```godot --doctool doc_export --gdscript-docs .```

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/ce7acd6a-a240-42c0-b3be-0d3b86024b58)

When it runs, it will create a headless session of godot (i think) so just give it a few seconds, and then you can exit using ctrl+C in the command line.

If it was successful, you should have an XML file for each script in your doc_export folder:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/b3815bb2-b973-4e66-97ac-52e1818ada0b)

## 4. Update make_rst.py Tool
The XML files we generated in step 3 are "class references" for each script that we documented. We can use these class references to generate reStructuredText (RST) files using the make_rst python tool included in the engine source. The file itself can be found here: https://github.com/godotengine/godot/blob/master/doc/tools/make_rst.py

However, in order to run the make_rst.py tool, you need to clone the whole godot repo. This is because make_rst.py depends on other parts of the repo, specifically the "import version".

Additionally, we will need to make 2 adjustments to the make_rst.py script once the repo is downloaded. The adjusted file is also available in this repo if you would rather just replace it.

Additionally, additionally, you will need python installed and added to your path. There are several tutorials for how to accomplish this online. You can verify your python is ready to use by running ```python --version``` from your command prompt:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/f15d513b-d1ce-447a-bfad-5048f5dca952)

Now that we have verified python is ready to use, we need to download the godot repo. You can do this using git, or you can go to the following link and click Code > Download Zip.
https://github.com/godotengine/godot/tree/master

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/dee491e3-1523-4ba3-9179-346290ce2310)

If you downloaded the .zip, extract the folder to a convenient location. For this example I will be using ```Documents\godot-source```

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/db75d826-6c16-42f8-8d30-e935cb67a722)

Once the ```godot``` folder is extracted and ready, go to ```godot/doc/tools``` this is where the make_rst.py tool is that we need to modify. 

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/072e92a4-2d0f-4068-8aea-e51ee0f68b13)

When the doctool generates the XML files for gdscript files, the names (and inherits from extended gdscripts) end up with quotation marks and relative filepath artifacts:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/d145b67c-f813-4ff6-a43c-296c55957106)

These weird strings cause errors when we try to run make_rst.py. So you can manually remove the characters, or we can modify the make_rst.py tool to clean the strings automatically as it parses the XML.

Either replace make_rst.py with the file in this repo, or make the following changes to the file. The changes are to the ```parse_class``` method in the ```State``` class. :

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/10833132-6336-4bff-a25a-054a2413f3af)

These changes just strip the illegal characters from the ```name``` and ```inherits``` attributes as it reads them from the XML. NOTE: THERE MAY BE A BETTER WAY TO DO THIS, IDK

Either way, once the changes have been made, we are almost ready to generate the RST files!

## 5. Generate RST Files
Before we begin, we need to set up folders for our inputs to and outputs from the make_rst.py tool. So navigate to the folder that contains make_rst.py:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/960d6433-1914-4722-8a4e-a1502ec483ba)

Create two folders in this location, one for input files and one for output files. For this example, I will be naming them ```doc_input``` and ```doc_output```:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/04c983f3-3576-4267-946e-ed4751241e9d)

Copy the XML class references we generated earlier into the ```doc_input``` folder

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/2bce3b2f-20fb-4953-89d5-fb5d7580a543)

Okay, now we are finally ready to run this make_rst.py script!

From the command prompt, navigate to the folder that contains make_rst.py:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/fb435e85-ca8b-46d2-87a9-b24abf87ba5e)

The command we are going to run is ```python make_rst.py <path1> --output <path2>```
- ```python make_rst.py``` accesses python from the command line and executes the script
- ```<path1``` tells the script where to scan for XML files as input
- ```--output <path2>``` tells the script where to output the RST files it generates

So for this example, the final command is ```python make_rst.py doc_input --output doc_output```:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/df217cb7-ca96-40f8-9b06-dcf43863a27d)

Don't panic about the errors! What is happening is that the script is trying to link to class references for things like ```Variant``` or ```float```, since those are types that were used in the scripts. If you want to, you can include class references for those items by grabbing the XML files from https://github.com/godotengine/godot/tree/master/doc/classes

However, as you add class references from the engine, you will get more and more of these unresolved type errors. This is because the references you add make their own references to even more classes, and it is a kind of cascading effect. The only way to get the RST files to generate without error is to include literally all of the class references for the engine (which I did just to check, it works). The doc/classes section of the repo also does not contain all of the class references for the engine. Some of them are spread out, in their own doc_classes folder (like GridMap for example: https://github.com/godotengine/godot/blob/master/modules/gridmap/doc_classes/GridMap.xml).

Anyways, if it was successful, you should have RST files in your ```doc_output``` folder:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/8c41e43e-e043-4d95-b82f-db29e8d8468f)

Congratulations if you made it this far! Now we just need to run these RST files through sphinx to get our documentation.

# Install and Configure Sphinx
