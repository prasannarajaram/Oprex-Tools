# Oprex-Tools
Some tools to make life easier with Oprex forms

## Expand all sections in an Oprex page

https://github.com/user-attachments/assets/b1da3ab9-2b06-446d-8efb-2853d0027647

To create this button, go to bookmarks (favorites) in the browser and create a new one.
Instead of providing a url, copy paste the below JS function into URL box. Save. Enjoy.

`javascript:(function() {    const buttons = document.querySelectorAll('span[id^="expand-arrow"]');    buttons.forEach(btn => btn.click());})();`


