---
title: Creating Layout Presets
taxonomy:
  category: docs
  tag: [gantry5]
gravui:
  enabled: true
  tabs: true
process:
  twig: true
---

Layout Presets are the basic building blocks the Layout Manager uses as a starting point for you to customize the layout of your site. Each preset gives you a new set of sections, each linked to styling, making up the look and feel of the site. You can then place Particles and Positions into these sections, add rows, and arrange the page the way you would like it to look.

If you're not finding a preset that meets your needs, you can create a new one. 

It is very easy for a skilled developer to create a **Layout Preset**. This is a great way to take an existing Layout Preset and add or remove **Sections** to it, or to build an entirely new Layout Preset from scratch, if you are so inclined.

Unlike adding rows in a section, creating an entirely new section (either stand-alone or as a sidebar) gives you the ability to create custom CSS styling affecting the new area of the page.

It's important to note that this is an advanced action, and Gantry 5 themes generally do not include built-in styling support for new sections. You will have to add the styling yourself, either linking it to an existing section or creating it from scratch in your `custom.scss` file located in `THEME_DIR/custom/scss`. 

## Preset Images

![Preset](outlines.png) {.border .shadow}

{% set tab1 %}

There is a section of the YAML files for the layout presets that deals with the preset image. This is an important part of the file as it creates the preview image you see when you are browsing the **Outlines** administrative interface. It can serve as a quick reference to the outline, giving you an at-a-glance look at what the layout looks like without having to visit the layout manager.

Gantry includes a set of these images that you can choose from. They are located in the `SITE_ROOT/administrator/components/com_gantry5/images/layouts/` directory and can be easily referenced with a stream link. For example, if you wanted to link to the `2-col-left.png` file in that folder, you would use the URL: `gantry-admin://images/layouts/2-col-left.png` as the preset image.

You can also add custom images. For example, let's say we have `example.png` and we want to use it as the preset image. We would place it in `/templates/TEMPLATE_DIR/custom/images/admin/layouts` and reference it in the YAML file as `gantry-media://images/admin/layouts/example.png`.

{% endset %}
{% set tab2 %}

There is a section of the YAML files for the layout presets that deals with the preset image. This is an important part of the file as it creates the preview image you see when you are browsing the **Outlines** administrative interface. It can serve as a quick reference to the outline, giving you an at-a-glance look at what the layout looks like without having to visit the layout manager.

Gantry includes a set of these images that you can choose from. They are located in the `SITE_ROOT/wp-content/plugins/gantry5/admin/images/layouts/` directory and can be easily referenced with a stream link. For example, if you wanted to link to the `2-col-left.png` file in that folder, you would use the URL: `gantry-admin://images/layouts/2-col-left.png` as the preset image.

You can also add custom images. For example, let's say we have `example.png` and we want to use it as the preset image. We would place it in `THEME_DIR/custom/images/admin/layouts` and reference it in the YAML file as `gantry-media://images/admin/layouts/example.png`.

{% endset %}
{{ gravui_tabs({'Joomla':tab1, 'WordPress':tab2}) }}

## Creating a New Layout Preset

![Preset](sections_3.png) {.border .shadow}

Creating a new **Layout Preset** is pretty simple. The first thing you will need to do is create a new YAML file in `THEME_DIR/custom/layouts`. For our example, we will name this file `example1.yaml`.

Here is the example code that will be in our new YAML file:

```yaml
version: 2

preset:
  image: gantry-media://images/admin/layouts/default.png

layout:
  /header/:
    - menu

  /main/:
    - position-breadcrumbs
    - system-messages
    - system-content

  /mainbottom/:
    - position-mainbottom

  /footer/:
    - position-footer
    - [copyright 40, spacer 30, branding 30]

  offcanvas:
    - mobile-menu
``` 

![Preset](sections_2.png) {.border .shadow}

This is a basic layout preset, featuring three sections included in the theme's original styling (`header`, `main`, and `footer`) with one additional section being added (`mainbottom`) that is not included with the original theme. We don't recommend adding new sections if you don't have to, but doing so can be done by adding it in a custom Layout Preset.

Once you have added a new section, it will display without any styling beyond the template's defaults. To add your own styling, you will want to do so via the `custom.scss` file located in `THEME_DIR/custom/scss`. For example, if you wanted the H1 tag to output a red font, you would add the line `#g-mainbottom h1 {color: red;}`. 

>>> Gantry sections appear in scss under the tag g-(section name). The `main` section would be `g-main`, as an example. This is done to separate your section names from other potentially conflicting third-party styling assignments.

## How to Create a Sidebar Section

![Sidebar](sections_1.png) {.border .shadow}

In this section, we will explain how to create a new Layout Preset with double sidebar. Each sidebar section represents a **block** within the **grid** container that makes up the **Main**, **Sidebar 1**, and **Sidebar 2** sections. You can find more information about how sidebars work in the [Sidebar Blocks and Grids section of the Layout Manager guide](../../configure/layout-manager#sidebar-blocks-and-grids).

The first thing you will need to do is create a new YAML file in `THEME_DIR/custom/layouts`. For our example, we will name this file `example2.yaml`.

Here is the example code that will be in our new YAML file:

```yaml
version: 2

preset:
  image: gantry-media://images/admin/layouts/3-col-right.png

layout:
  /header/:
    - menu

  /container-main/:
    -
      - main 60:
        - position-breadcrumbs
        - system-messages
        - system-content

      - sidebar1 20:
        - social
        - position-sidebar
        - position-right

      - sidebar2 20:
        - social
        - position-sidebar
        - position-right

  /footer/:
    - position-footer
    - [copyright 40, spacer 30, branding 30]

  offcanvas:
    - mobile-menu

structure:
  sidebar1:
    subtype: aside
    block:
      fixed: 1
  sidebar2:
    subtype: aside
    block:
      fixed: 1
```

This is a very simple Layout Preset, giving the user **Header**, **Main**, and **Footer** sections in addition to two independent **Sidebar** sections. Each section gets its own base styling that provides the base by which added **Particles** and **Positions** are placed.

>>> Each horizontal row needs to equal 100% width. In the example above, you will notice that `main` has a width set of 60%, followed by two sidebars each at 20% width. It's also very important to remember that YAML files only support spaces divisible by 2, and not tabs.

## Spanning a Sidebar Across Multiple Sections

In this section, we will demonstrate two YAML files that create one and two sidebar sections that span across multiple sections of the site. This is useful in cases where you want to have additional sections, such as your header and footer, share horizontal space with the sidebar.

```yaml
version: 2

preset:
  image: gantry-admin://images/layouts/2-col-right.png

layout:
  /container-main/:
    -
      -
        header:
          - [position-header]
        navigation:
          - [menu]
        messages:
          - system-messages
        main:
          - system-content
        footer:
          - [copyright 40, spacer 30, branding 30]
      -
        aside:
          - position-aside

  offcanvas:
    - mobile-menu

structure:
  aside:
    block:
      fixed: 1
```

As you can see in the example above, we have nested multiple sections including the **header**, **navigation**, **messages**, and **footer** in the same horizontal space as the **aside** section, which acts as a sidebar.

In the example below, you will see a two-sidebar layout preset YAML with a sidebar to the left and an aside section to the right of multiple sections.

```yaml
version: 2

preset:
  image: gantry-admin://images/layouts/3-col.png

layout:
  /container-main/:
    -                              # main row
      -                               # main column 1
        sidebar:                        # section
          - position-sidebar              # content row

      -                             # main column 2
        header:                       # section
          - position-header             # content row
        navigation:                   # section
          - menu                        # content row
        main:                         # section
          - system-messages             # content row
          - system-content              # content row
        footer:                       # section
          - copyright                   # content row

      -                             # main column 3
        aside:                        # section
          - position-aside              # content row

  offcanvas:
    - mobile-menu

structure:
  aside:
    block:
      fixed: 1
```

## Syntax Guide

There are four main rules to keep in mind when creating a layout preset.

1. tiered content is ordered as `row - column - row - column - row - column`. See the earlier examples.
2. `foo:` creates a section. 
3. Adding slashes (example: `/foo/:`) creates a container that enables you to take advantage of section layout settings for improved styling flexibility.
3. Multiple particles in a row are put within `[ ]` brackets. Example: `- [logo, menu]`
4. You don't need brackets for single-particle rows. Example: `- menu` is the shorthand of `- [menu]`

## Common YAML Items

| Item              | Description                                                                                                            |
| :-----            | :-----                                                                                                                 |
| `system-messages` | Inserts a **System Messages** position which displays system messages on the front end.                                |
| `system-content`  | This line displays content on the page provided by the CMS. It is the content body.                                    |
| `position-`       | Followed directly by a position name (example: `position-header`) it creates a position and assigns it the given name. |
| `version`         | This indicates the Gantry YAML version being used. Version 2, for example, was introduced in Gantry 5.2.               |

## YAML Versions

As Gantry 5 evolved, we wanted to make its layout YAML files easier to read and use. By simplifying the syntax, removing particle tags, and simplifying the way sections are managed, we were able to create a better user experience. These changes were implemented in Gantry 5.2 and later versions.

Because this update would break YAML files created with the original YAML style, we opted to add a `version` designation that lets Gantry know which YAML style is being used in a given file.

In short, any YAML preset file created after Gantry 5.2 using the new (current) YAML style should have `version: 2` as the first line. Otherwise, Gantry will default to the original YAML syntax.
