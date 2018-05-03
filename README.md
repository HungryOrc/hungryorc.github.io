# Orc's Tech Notes

* URL of these notes: https://hungryorc.github.io/

* This website implemented [Jekyll Documentation Theme](http://idratherbewriting.com/documentation-theme-jekyll/), thanks to the author of this theme.

* Notice
  
  * Any change saved on this git repo will be reflected on the URL listed above after a few seconds.

## How to add a new page

### 1. Add a new folder/item in the side bar

The default side bar is configured in this file: `/_data/sidebars/mydoc_sidebar.yml`.

If this new page does not require a new folder/item in the side bar, then ignore this step.

### 2. Add a .md file in the `/pages/` folder

Find/create the sub folder to place the .md file.

### 3. Add the tag(s) for the new page in the tag list at `/_data/tags.yml`

If the tag(s) already exist in the tag list, then ignore this step. 

### 4. Create a new `tag_xxx.md` file for the new tag

All the .md files for tags reside in folder `/pages/tags/`, take a look at any file in this folder, then you'll see how to create a new one, very simple and short files.

Again, if this new page doesn't require a new tag, ignore this step.
