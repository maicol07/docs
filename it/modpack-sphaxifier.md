---
title: Modpack Sphaxifier
description: A tool to create a resource pack for Minecraft modpacks. Designed principally for Sphax mod patches.
published: true
date: 2023-01-20T09:24:54.770Z
tags: minecraft, modpack-sphaxifier
editor: markdown
dateCreated: 2020-09-14T14:57:28.095Z
---

# Caratteristiche

- Package multiple patches together
- Multiversion and multiresolution support!
- Credits generation with editor tool (for publishing a pack)
- Load previously created packs
- Customization: choose your font and theme and make Modpack Sphaxifier styled like you!

# Requirements

<img src="https://raw.githubusercontent.com/maicol07/modpack_sphaxifier/master/resources/img/logo.png?token=ACIGKZUE2LEJTALD3HWGHRS7NBYEO" alt="Logo" width="100" align="right">

- Windows 10 x64

# Installazione

You can download the installer from this link:

- [Download](https://maicol07.it/modpack_sphaxifier)
  {.links-list}

# Upgrade

The only way to update is to download the new version installer and rerun installation. You can always check the changelog [here](#changelog)

> The next minor update will enable self-updating.
> {.is-info}

# Prices

### Free demo

Currently, there is a free demo version that allows to use all the features, with the limitation of maximum 10 patches in a pack.

### Paid

If you want to bypass the free demo limits, you can go ahead and buy a license from one of these gateway:

| Gateway name | PayPal                                                        | Stripe                                                                                     |
| ------------ | ------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Price        | 9,99 €                                                        | 9,99 €                                                                                     |
| Commissions  | 10%                                                           | 5%                                                                                         |
| Total price  | 10,99 €                                                       | 10,49 €                                                                                    |
| Purchase     | <div id="paypal-button-container-P-02G36609M7987194TL5S2XJY"> | <a class="stripe-btn" href="https://buy.stripe.com/eVa4iz18c3vKbG8aEE">Pay with Stripe</a> |

> If you want to cancel the subscription you can do it via your PayPal account if you paid with PayPal. Otherwise, check if there is an unsubscribe link in your stripe subscription confirmation email or contact the developer from his [website](https://maicol07.it) (chat in the bottom-right corner or contact form)
> {.is-warning}

# How to use

## First run

When you first open Modpack Sphaxifier you will be prompted to complete a wizard. This helps you to setup the settings.

1. If you have a license, purchased from the buttons above, add its key/id in the first page of the wizard. Otherwise, you can use all the features of the software, but with a **limit of 10 patches** in a pack. When you have finished, click Next;
   ![Wizard - Page 1](/modpack_sphaxifier/wizard-1.png)
2. The final screen of the wizard will ask you to enter the main settings of Modpack Sphaxifier. When you have to select a folder, you can enter the path in the text field or click the folder button to browse your computer. Check [here](#available-settings) for help on individual settings. Click Finish when you're done.
   ![Wizard - Page 2](/modpack_sphaxifier/wizard-2.png)

## Main page

Here is where you can create the pack. Easily, select all the values in the first row, add the name of the pack, select the patches you want to include in the zip and click Start. You can also load a previously created pack from the last select.

<img class="right" src="/modpack_sphaxifier/main.png" alt="Main window">

The pack creation flow is like this:

1. Your selections are saved in a json file. You might want to load that in the future from the **Load previous modpack** select
2. Files are copied in a temporary directory.
3. The temporary directory is being zipped.
4. If credits has been added in the [Credits editor](#credits-editor), a txt file will be created in the folder you selected in settings.
5. The temporary directory is being deleted

If you want, you can reload the patches list. For example, you add a patch when you are selecting other patches in the list. This button adds that new patch in the list without restarting the software.

### Toolbar actions

To access other windows, you have to use the toolbar on the top. Currently, you can activate the following actions:

- **File > Exit**: Exit the app
- **Edit > Settings**: Opens the [settings](#settings) window
- **Tools > Credits Editor**: Opens the [Credits Editor](#credits-editor) window
- **Help > Check updates**: Check updates. Will be available in a future version.
- **Help > Support**: Opens this documentation page in the default system browser
- **Help > About Qt**: Opens the About Qt window
- **Help > About**: Opens the About window

## Settings

Edit your settings as you want. Check below for a list of available settings and what they do.

<img class="right" src="/modpack_sphaxifier/settings.png" alt="Settings window">

### Buttons

- **Check**: Checks the license. If valid, it will be activated.
- **Reset**: Resets all settings. You have to click on Save to apply the changes.
- **Save**: Saves the settings
- **Discard**: Cancel the changes and closes the settings window

### Available settings

- **Font**: Change the font in the app. Defaults to MS Shell Dlg 2;
- **Theme**: Change the theme of the app. Defaults to `windowsvista`;
- **Mods patches folder**: Select the folder that contains the mods patches. Check the section below to know what directory you have to choose;
- **Credits file**: Folder where a credits file will be generated;
- **Author**: Author of the pack;
- **License key**: If you have a license enter it here and press Check. You have to save the settings to save the license

### Mods patches directory structure

You need to follow a particular folder structure if you want that Modpack Sphaxifier recognizes your patches. Here is explained the folder tree:

```
root	<--- Root folder. You need to choose this folder in settings or wizard
├───1.12.2	<--- Version folder
│   └───64x	<--- Resolution folder
│       ├───abyssalcraft	<-- Patch folder
...
```

## Credits editor

<img class="right" src="/modpack_sphaxifier/credits-editor.png" alt="Credits editor window">
This is a small tool to manage patches credits. There's no much to explain, but here are the main elements:

### Buttons

- **Add**: Add a row in the table
- **Remove**: Remove the selected rows in the table
- **OK**: Save the credits and closes the window
- **Cancel**: Cancel the changes and close the window

### Table columns

- **ID**: ID of the patch. It's the patch folder name;
- **Credits**: The text to show in credits (generally, the list of authors of that patch)

# Troubleshooting

### I can't see any patch/version or resolution

Check if you have setup your patches folder as explained [here](#settings).

# Roadmap

## 1.1

- Check updates and automatic updates
- Select changelog file instead of directory (removes the setting)
- TBD

# Changelog

## 1.0.1

First public release

## 1.0 - Unpublished

First version

# Crediti

Icons created by:

- [Freepik](https://www.flaticon.com/authors/freepik "Freepik")
- [Pixel Buddha](https://www.flaticon.com/authors/pixel-buddha "Pixel Buddha")
- [Smashicons](https://www.flaticon.com/authors/smashicons "Smashicons")
- [Good Ware](https://www.flaticon.com/free-icon/spaceship_2927887?term=start\&page=1\&position=11 "Good Ware")

of [Flaticon](https://flaticon.com) and [Papirus Development Team](https://icon-icons.com/it/icona/qt/94938) .

Check out also these links:

- [BDCraft](https://bdcraft.net)
- [Author Website](https://maicol07.it)
