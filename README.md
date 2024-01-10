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

If you have made it this far, you can use my other tool to view and export a simplified version of docs from the XML files directly: https://github.com/InfernalWAVE/gdscript-xml-docviewer

For interactive docs, we will need to do a bit more.

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

## 6. Install, Configure, and Initialize Sphinx

Install sphinx according to their installation instructions: https://www.sphinx-doc.org/en/master/usage/installation.html

Since we know python is ready to use in the command prompt, we can use pip to install it with ```python -m pip install sphinx```

We will also need an extension for sphinx that the godot documentation expects, called sphinx-tabs: https://sphinx-tabs.readthedocs.io/en/latest/

We can install sphinx-tabs with ```python -m pip install sphinx-tabs```

This isn't mandatory, but we will install the sphinx theme that godot uses as well by ReadTheDocs: https://sphinx-themes.org/sample-sites/sphinx-rtd-theme/

We can install the ReadTheDocs theme with ```python -m pip install sphinx-rtd-theme```

Once everything is installed, we need to make a directory for sphinx to work in. For this example it will be ```Documents\docs_test```:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/9f2f735f-1d3f-40dd-b14f-5ee179b2819a)

From the command prompt, navigate to this folder:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/95a9149a-2064-4242-9f7d-33c602942c05)

Run the ```sphinx-quickstart``` command in the command prompt to initialize the documentation project in the current folder, follow the docs here: https://www.sphinx-doc.org/en/master/usage/quickstart.html

When you run the command, it will automatically select the folder you are currently in as the root folder, and begin asking you setup questions:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/58bf62b7-5089-4945-b0bb-821ac361d671)

I choose yes to separate "source" and "build", this will create two separate folders for us to use for input and output in the root. Then it will ask ask for a project name, author name, and release version:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/5cc66d6e-52f6-43d0-adce-b29d1b724a0f)

I chose "gdscript-doc-tutorial", my name, and 0.0.1. Then it asks about a language:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/5003183f-9223-415d-a024-1f79719923b9)

Since I am writing this in English, and it defaults to English, I just hit enter. This finally initializes the project and creates some files in the ```docs_test``` folder:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/240f6de0-24f4-46a9-b421-825bdd116c16)

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/1e8bd3d2-f473-4d07-a73b-fe00b0d47114)

Before building our documentation, we need to enable the extension and theme that we installed earlier. To do this, we need to edit a file in ```docs_test\source```:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/73f38ac1-1d84-45c8-b3e5-b41443eca81e)

```conf.py``` defines configuration information for the documentation when sphinx goes to build it. So open ```conf.py``` in a text editor:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/e870e2a8-27d7-488e-83fe-53b105098a69)

When the project is initialized, it creates some basic templates for us to use in ```conf.py```. We will add the sphinx-tabs extension to the ```extensions``` array and we will add the theme in the ```html-theme``` variable. The updated file should look like the following:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/56790505-fb0d-416c-85a1-ce90142478c5)

The lines exactly are:

```extensions = ['sphinx_tabs.tabs']```

```html_theme = 'sphinx_rtd_theme'```

Save the updated ```conf.py``` file.

Now we need to add all of the RST files we generated earlier to the ```docs_test\source``` folder:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/a8df1692-787a-470f-8dd0-2eb64ae8cc16)

When you add the files, windows will warn you that ```index.rst``` already exists in the destination, just replace it. The make_rst.py tool created the index.rst we want earlier.

Now that ```conf.py``` has been configured, and the source RST files have been added, we are ready to build the documentation!

## 7. Build Documentation
From the command prompt, navigate to the ```docs_test``` folder:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/71e591e9-b673-497d-9655-84df24327530)

The command we are going to run is ```sphinx-build -M html <path1> <path2>``` where
- ```sphinx-build -M html``` uses the sphinx-build tool to create HTML documentation using "make mode"
- ```<path1>``` is the location sphinx-build will use as the input source
- ```<path2>``` is the location sphinx-build will save the HTML documentation to

So the final command is ```sphinx-build -M html source build```:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/754e4ac6-1fa1-41ee-940e-acc47fc0d532)

Don't worry about the warning(s)! I think they are related to the "unable to resolve type" errors earlier when we were generating the RST files, and it just depends on how many class references from the engine you want to include!

If it was successful, you should have an HTML project in your ```docs_test\build\html``` folder:

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/b372c4da-a47f-4478-ae83-204e50f43bbb)

You should be able to open index.html in your browser, and browse your docs! That's it!

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/9be17747-1653-42e1-866f-528f007eeccb)

![image](https://github.com/InfernalWAVE/gdscript-sphinx-docs-tutorial/assets/48569884/c14c3063-8578-498a-bcd3-8b6244bd3e44)

