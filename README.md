# FigJam-Darkboard (archived)
# This repository is not longer maintaned, because on this place will be a better project. Cya soon : )

## Overview
FigJam-Darkboard is a simple script that enables dark mode in the FigJam whiteboard interface. It modifies the styles of key elements to provide a darker, more eye-friendly experience.

## Features
- Changes background color to black for `html`, `body`, and key elements.
- Adjusts the opacity of the canvas.
- Darkens toolbar and GPU view content.
- Removes unnecessary button borders for a cleaner UI.

## Installation & Usage
1. Open the FigJam whiteboard in your browser.
2. Open the browser developer console (`F12` or `Ctrl + Shift + I`).
3. Copy and paste the following script into the console and press `Enter`:

[main.js](./main.js)
```javascript
   class DarkMode {
      constructor(_MAX_TRIES = 10) {
         this.MAX_TRIES = _MAX_TRIES;
         this.ACTIVATED = false;
      }

      fetchElements() {
         this.HTML = document.querySelector("html");
         this.BODY = document.querySelector("body");
         this.CANVAS = document.querySelector("canvas");
         this.GPU_VIEW_CONTENT = document.querySelector("div.gpu-view-content");
         this.TOOLBAR = document.querySelector("div#delightful-toolbar");
      }

      applyStyle(element, styles) {
         if (!element)
            throw new Error(`[applyStyle] -> Element: ${element} not found.`);

         Object.assign(element.style, styles);
      }

      applyAttribute(element, attribute, value) {
         if (!element)
            throw new Error(`[applyAttribute] -> Element: ${element} not found.`);

         element.setAttribute(attribute, value);
      }

      make_html_darker() {
         this.applyStyle(this.HTML, { background: "black" });
      }

      make_body_darker() {
         this.applyAttribute(this.BODY, "data-preferred-theme", "dark");
         this.applyStyle(this.BODY, { colorScheme: "dark", background: "black" });
      }

      make_canvas_darker() {
         this.applyStyle(this.CANVAS, { opacity: "0.5", background: "black" });
      }

      make_gpu_view_content_darker() {
         this.applyStyle(this.GPU_VIEW_CONTENT, { background: "black" });
      }

      make_toolbar_darker() {
         this.applyStyle(this.TOOLBAR, { background: "#2f2f2f" });
      }

      make_other_elements_darker() {
         const button = document.querySelector("button.meeting_tools_toolbar--toolbarItemButton--VBNmh.meeting_tools_toolbar--withHover--r7Sks");
         this.applyStyle(button, { border: "none" });
      }

      activate() {
         console.log("Activating dark mode!");
         this.fetchElements(); // Fetch all elements.

         let tries = 0;
         let intervalId = setInterval(() => {
            tries += 1;

            if (this.ACTIVATED) {
               console.log("Dark mode was activated successfully.");
               return clearInterval(intervalId);
            }

            if (tries > this.MAX_TRIES) {
               console.log("Something went wrong while activating darkmode to some elements.");
               return clearInterval(intervalId);
            }

            try {
               this.make_html_darker();
               this.make_body_darker();
               this.make_canvas_darker();
               this.make_gpu_view_content_darker();
               this.make_toolbar_darker();
               this.make_other_elements_darker();

               this.ACTIVATED = !this.ACTIVATED;
            } catch(e) {
               console.log(e);
               this.fetchElements(); // Fetch all elements again.
            }

         }, 1000);
      }
   }

   let darkMode = new DarkMode(15);
   darkMode.activate(); // Activate the darkMode.
```

4. The FigJam whiteboard should now be in dark mode.


## Alternative Method: Tampermonkey Integration
You can also integrate this script with **Tampermonkey** for automatic execution every time you visit the FigJam whiteboard.

1. Install the [Tampermonkey extension](https://www.tampermonkey.net/) for your browser.
2. Create a new script in Tampermonkey.
3. Paste the following script into the editor:

[main.js](./main.js)
```javascript
   // ==UserScript==
   // @name         FigJam Dark Mode
   // @namespace    http://tampermonkey.net/
   // @version      0.1
   // @description  Enable Dark Mode for FigJam Whiteboard
   // @author       You
   // @match        https://www.figma.com/board/*
   // @grant        none
   // ==/UserScript==
   
   (function() {
      'use strict';
   
      class DarkMode {
         constructor(_MAX_TRIES = 10) {
            this.MAX_TRIES = _MAX_TRIES;
            this.ACTIVATED = false;
         }
      
         fetchElements() {
            this.HTML = document.querySelector("html");
            this.BODY = document.querySelector("body");
            this.CANVAS = document.querySelector("canvas");
            this.GPU_VIEW_CONTENT = document.querySelector("div.gpu-view-content");
            this.TOOLBAR = document.querySelector("div#delightful-toolbar");
         }
      
         applyStyle(element, styles) {
            if (!element)
               throw new Error(`[applyStyle] -> Element: ${element} not found.`);
      
            Object.assign(element.style, styles);
         }
      
         applyAttribute(element, attribute, value) {
            if (!element)
               throw new Error(`[applyAttribute] -> Element: ${element} not found.`);
      
            element.setAttribute(attribute, value);
         }
      
         make_html_darker() {
            this.applyStyle(this.HTML, { background: "black" });
         }
      
         make_body_darker() {
            this.applyAttribute(this.BODY, "data-preferred-theme", "dark");
            this.applyStyle(this.BODY, { colorScheme: "dark", background: "black" });
         }
      
         make_canvas_darker() {
            this.applyStyle(this.CANVAS, { opacity: "0.5", background: "black" });
         }
      
         make_gpu_view_content_darker() {
            this.applyStyle(this.GPU_VIEW_CONTENT, { background: "black" });
         }
      
         make_toolbar_darker() {
            this.applyStyle(this.TOOLBAR, { background: "#2f2f2f" });
         }
      
         make_other_elements_darker() {
            const button = document.querySelector("button.meeting_tools_toolbar--toolbarItemButton--VBNmh.meeting_tools_toolbar--withHover--r7Sks");
            this.applyStyle(button, { border: "none" });
         }
      
         activate() {
            console.log("Activating dark mode!");
            this.fetchElements(); // Fetch all elements.
      
            let tries = 0;
            let intervalId = setInterval(() => {
               tries += 1;
      
               if (this.ACTIVATED) {
                  console.log("Dark mode was activated successfully.");
                  return clearInterval(intervalId);
               }
      
               if (tries > this.MAX_TRIES) {
                  console.log("Something went wrong while activating darkmode to some elements.");
                  return clearInterval(intervalId);
               }
      
               try {
                  this.make_html_darker();
                  this.make_body_darker();
                  this.make_canvas_darker();
                  this.make_gpu_view_content_darker();
                  this.make_toolbar_darker();
                  this.make_other_elements_darker();
      
                  this.ACTIVATED = !this.ACTIVATED;
               } catch(e) {
                  console.log(e);
                  this.fetchElements(); // Fetch all elements again.
               }
      
            }, 1000);
         }
      }
      
      let darkMode = new DarkMode(15);
      darkMode.activate(); // Activate the darkMode.
   })();
```

4. Save the script and refresh your FigJam whiteboard. The dark mode should now be applied automatically every time you load the page.

## License
This project is open-source under the **GNU General Public License v3.0**.
