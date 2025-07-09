# Creating a custom MSE Hub

Follow these instructions to set up your own MSE Hub.

## Prerequisites

### Install Github Desktop

This isn't strictly necessary, but promise me, it'll make your life a lot easier. You can get GH Desktop [here](https://desktop.github.com/download/).

### Install Python

This one *is* strictly necessary, as the script that builds the entire site is written in Python. You can get it [here](https://www.python.org/downloads/).

## Step 1: Fork me

If you're reading this, you're probably already here, but in case you aren't, navigate to https://github.com/magictheegg/mse-hub/. At the top, click the "Fork" button to start creating your own fork of the code.

![Fork](https://github.com/magictheegg/mse-hub-readme/blob/main/fork.png?raw=true)

On the next page, **change the "Repository name" field** to \<your-github-username\>.github.io. This is **critical** for making your code actually deploy to Github sites. You can add a Description if you want. Keep "Copy the `main` branch only" checked. Once you've renamed the repository, click "Create fork".

![Fork part 2](https://github.com/magictheegg/mse-hub-readme/blob/main/fork-part-two.png?raw=true)

Once the fork is created, you'll see the code in Github. Now it's time to move to Github desktop.

Once you've logged into Github Desktop, click File => Clone repository ... and you should see your forked repository in the repo list. Click it, then choose clone. When the "How are you planning to use this fork?" modal pops up, select "For my own purposes," then Continue.

![How are you planning to use this fork?](https://github.com/magictheegg/mse-hub-readme/blob/main/how-fork.png?raw=true)

Finally, in the bar along the top, select "Fetch origin" to pull the origin into your forked repo.

## Step 2: Exporting set files

> :memo: **Note:** This exporter uses the "Title" and "Set code" entered in the "Set info" tab of your MSE set. If those aren't set, the exporter will exhibit strange behavior.

In the resources folder of your cloned repo, you'll find the "magic-egg-allinone" exporter. Copy that into the "data" folder of MSE, then open the program. Open a set you'd like to export, then click File => Export => HTML ... and select Egg's All-in-One. This will export all of your site files.

The options you can select are as follows:
- **Export images**: Defaults to "Yes". Only change this if for some reason you don't need images with your export.
- **Image type**: Defaults to "Png". Github Pages sites can only host 1 GB of data before they start to error out, so if you're doing large exports, consider exporting some or all sets as "Jpg".
- **Symbol rarity**: Defaults to "Rare". This is the rarity color that your symbol icons will export as.
- **Draft structure**: Defaults to "Play booster". If your set is following a different booster structure, set it here. This is editable with a custom JSON while building the site.
- **Formats**: This is a text field where you can enter any custom formats your set is a part of, which can be queried in your site's search.
- **V mana replacement**: If you're using a custom V mana symbol, insert it here.

Once each of these options is filled out, click OK and save the set file as "\<set_code>.txt". (This should match the "Set code" in your "Set info" tab.) This will take a second as the application exports all your images, and the end result is two outputs:
- **\<code>.txt**, which is irrelevant.
- **\<code>-files**, a directory containing all the files necessary to publish your set onto your hub.

## Step 3: Generating the site

Surprisingly, you're almost done! Copy the "\<code>-files" folder (the entire directory) into the "sets" folder of your GitHub checkout. Open Github Desktop, and you should see that directory in the "Changes" sidebar. Click Repository => Open in Terminal (or Command Prompt for Windows machines).

In the opened terminal, execute the following command:

```
git config --global http.postBuffer 157286400
```

This updates your buffer so you can upload all of your images with no timeout issues. Otherwise, git sometimes gets tired and quits somewhat arbitrarily. Afterwards, execute:

```
python3 -m pip install pillow
python3 -m pip install markdown
python3 scripts/build_site.py
```

> :memo: **Note:** If at this point `python3` prompts you to install through the Windows Store, do so. It's the path of least resistance.

> :memo: **Note:** If `python3` can't be found, try running the same command with `py` or `python` instead.

This will spit out a bunch of confirmation lines for different site elements being built. The first time it runs, it will take a few minutes to process through each image. Subsequent runs will be much quicker, unless you update the images with new files. After the command finishes, navigate back to Github desktop and you should see plenty of new artifacts in the "Changes" sidebar. In the bottom left, type a title for your change (this is for versioning), then click "Commit to main". Once you've done so, a big "Push origin" button will appear in the middle of the window. Click that, wait for the push to finish, and voila! Your site is deployed.

To track the process of your site deployment, navigate to https://github.com/USERNAME/USERNAME.github.io/actions, replacing USERNAME with your Github username. Each time you push to origin, a deployment action will trigger, and once that's complete your site will be visible at https://\<username>.github.io.

## Future MSE Set Hub Updates

To get updates to the scripts or resources, from Github Desktop, select "Fetch origin" in the bar along the top and wait for that process to complete. Once it's done, select "Current branch" in the same bar, then at the bottom of the opened menu click "Choose a branch to merge into **main**". On the next modal, select "upstream/main", then click "Create a merge commit". This will bring all new code in the main repo into your forked repo, and it will be ready to push the next time you push the contents of your site to main.

If it's indicated that the new change comes with a change to the exporter, you can find the updated exporter in the `resources` directory. Make sure to copy it into your MSE's `data` folder.

## Appendix

### Updating your custom site

To update a set, re-export it using Egg's All-in-One exporter, then replace the "\<code>-files" directory in your "sets" folder with the newly generated folder. To add or delete a set, simply add or delete that set's "-files" folder from the "sets" folder. After updating the "sets" folder, rerun `python3 scripts/build_site.py` and deploy your new changes.

### Custom assets

If you want to replace default or generated assets, You can use the "custom" folder in your Github checkout. Put any files you'd like to replace in similarly named directories within "custom". For instance, to replace a set's logo, create "sets/\<code>-files" within your "custom" folder, then put a new "logo.png" within it. Any assets replaced this way will be brought over once you run `build_site.py`.

This is specifically useful for backgrounds on preview pages, which are by default blank. If you want a background, add "bg.png" to that set's "custom/sets/\<code>-files" directory and rebuild the site.

### Custom tags

The search page supports custom tags, which can be queried using "tag:\<foo>". In your MSE file, add "!tag \<foo>" to the card notes. The exporter and site builder will do the rest.

### Custom preview sorting

If you want cards to sort to the same row, add "!sort <foo>" to their notes. If you want cards to sort to the end of the preview gallery, add "!last" to their notes.

### Set splash pages

Sets now support splash pages written in markdown! If you want to add one, add `splash.md` to your "custom/sets/\<code>-files" directory. It will automatically show up as the default view on the set's page. You can shortcut adding set logos/icons and card images by putting their name between `%` symbols. It'll look something like this:
```
![alt text](%Card Name Goes Here% "hover-over text")
```

### Designer Notes

To add designer notes to a card, add `card-notes/<cardname>.md` to "custom/sets/\<code>-files" to your custom directory. This markdown file will automatically append to the card's page when you build your site.

### Changing the homepage's background gradient

There are 34 gradients you can choose from on the homepage, which are stored in `resources/gradients.json`. You can always switch between them using the select in the top left of the homepage. If you'd like to set a new gradient as the default, simply edit `gradients.json` and move your favorite gradient's entry to the top of the text file.

### HTML addenda

If you want to insert your own custom HTML into a set's preview gallery (for instance, to add images of a Masterpiece series to the end), create your custom HTML file in "custom/\<code>-files/addenda/\<code>-addendum.html". This will be injected at the end of that set's preview gallery once you run `build_site.py`.
