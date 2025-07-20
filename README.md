# Oprex-Tools
Some tools to make life easier with Oprex forms

## Expand all sections in an Oprex page
Many a times it is required to search the filled in Oprex form or a specific Oprex question.  
The search (`Cntl+F`) does not return results of the particular section if is not visible (or expanded)  
So, the user is forced to click the expand buttons multiple times to reveal all text to make a simple search.  
To mitage this problem, we have the JS function below, which will expand all the sections of an Oprex page.  
To create this button, go to bookmarks (favorites) in the browser and create a new one.  
Instead of providing a url, copy paste the below JS function into URL box. Save.  
`javascript:(function() {    const buttons = document.querySelectorAll('span[id^="expand-arrow"]');    buttons.forEach(btn => btn.click());})();`

### Here is short video
https://github.com/user-attachments/assets/b1da3ab9-2b06-446d-8efb-2853d0027647
