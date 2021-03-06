# Volibri
Version Control for Kolibri UI/UX design


**The problems:**

It appears to us that a version control solution is needed to accommodate the collaborative design work we've done for Kolibri.
There are 3 major issues we want to address with this version control solution:

1. Be able to easily trace back the design decision making history. Following describes a typical issue we are facing more and more often. After many iterations for an UI element, it becomes unclear to us how we arrived at the current approach, and start bringing arguments up that later we realized were fully discussed before.

2. Be able to keep design modifications in sync across the team. Our design team has collectively experienced this pain that if one of us updates the design of a shared UI element, all of us have to find the files that uses this shared element and manually update it.

3. Be able to asynchronously comment on and reply to new designs. We usually post designs in our online chat room to receive feedback. This can be an issue if strings of comments were unseen and pushed up in the chatroom. It can be very inconvenient and difficult to access the feedback again. It makes a lot sense to asynchronously comment and reply via Pull Request.

**The solution:**

Terminology: `link-import` means when using `Sketch-Linked-SVG` plugin, you are importing an external SVG file into a Sketch file. When the imported external SVG file is modified, we can use the `Update All Linked SVGs` method to update all link-imported SVG layers in the Sketch file.

Issue 1 and 3 stated above are addressed by managing the project on github. 
For issue 2, we rely on the Sketch plugin `Sketch-Linked-SVG` to creat linkages between external SVG files and multiple Sketch files. If the external SVG files get modified, all Sketch files that `link-import` the SVG files will get updated automatically. The external SVG file can also be created by Sketch.

The reason we choose SVG format is because it's a text based format. It is impossible to merge binary files such as bitmap images(PNG, JPG) and `.sketch` format, PSD. By using the SVG format, we might be able to have a mergeable solution that enables multiple designers to work on the same file at the same time. There remains further testing to know how mergeable this SVG based solution is. 

There is an example how to build a list UI element using this approach.

1. We create 2 Sketch files, `container_element.sketch` and `list_item_element.sketch`.

2. We use the `Make Exportable` feature of Sketch to generate 2 SVG files, `container.svg` and `list-item.svg`. (Note: because we installed `svgo-compressor` plugin, all exported SVGs will be automatically compressed.)

3. We create another Sketch file `list.sketch` that will `link-import` the `container.svg` and `list-item.svg`. Place the imported list-item inside the imported container, copy paste(the duplications will keep the linkage to the external SVG file) the list-item couple times to make it looks like a proper list UI element.

Say later you want to change the background color of the list-item: 

1. Open the `list_item_element.sketch` and change the background color. 

2. Export again to update the `list-item.svg` with your modification. (it is important to keep the file name and location unchanged!). 

3. Open your `list.sketch`, click `Plugins/Place Linked Bitmap/Update All Bitmap`, you will see that all the list-items' background color get changed to the new color.

It gets more interesting when you apply this pattern recursively. 

1. Go ahead and select all elements that consists the list inside `list.sketch` and create a `Symbol` called `list`. 

2. Export this symbol you get `list.svg`. 

3. Now you can `link-import` this `list.svg` in other Sketch files, such as your `Learn_page.sketch` and `Explore_page.sketch`, and they will get auto updated if you changed anything in `list.svg`.

**The dependencies:**

You need to install the following plugins.

1. [Sketch-Linked-SVG](https://github.com/66eli77/Sketch-Linked-SVG) (Sketch plugin that lets you import external SVGs and update the imported SVGs).

**Let's get started:**

1. `fork` this Repo.

2. `git clone` your forking repo.

3. Create some SVG format UI elements using Sketch and `link-import` them in other Sketch file to create more complicated UI elements.

4. Commit both your source file(the Sketch files) and your SVG files(the Exported SVG files), so that other people can `link-import` your SVGs in their mockup. Future modifications can be done by editing your source files and re-export the SVG files.

5. Open a PR with proper descriptions stating the design consideration(a PR template is here to help you with).

6. Everyone can give feedbacks by commenting your PR.

7. After all concerns addressed, another designer with merge privilege can merge your PR to make your new design officially available to everyone in the organization. :shipit:

Notice: If you like to try a couple different design, you should create branches for each design.
