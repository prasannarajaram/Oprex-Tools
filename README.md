# Oprex-Tools
Some tools to make life easier with Oprex forms
1. [Expand all sections in an Oprex page](https://github.com/prasannarajaram/Oprex-Tools/blob/main/README.md#expand-all-sections-in-an-oprex-page)
2. [Select all checkboxes](https://github.com/prasannarajaram/Oprex-Tools/blob/main/README.md#select-all-checkboxes)

## Expand all sections in an Oprex page
Many a times it is required to search the filled in Oprex form or a specific Oprex question.  
The search (`Cntl+F`) does not return results of the particular section if is not visible (or expanded)  
So, the user is forced to click the expand buttons multiple times to reveal all text to make a simple search.  
To mitage this problem, we have the JS function below, which will expand all the sections of an Oprex page.  
To create this button, go to bookmarks (favorites) in the browser and create a new one.  
Instead of providing a url, copy paste the below JS function into URL box. Save.  
```javascript
javascript:(function() {    const buttons = document.querySelectorAll('span[id^="expand-arrow"]');    buttons.forEach(btn => btn.click());})();
```
### Here is short video
https://github.com/user-attachments/assets/b1da3ab9-2b06-446d-8efb-2853d0027647

## Select all checkboxes
When creating an "Audit view" (read as exporting Oprex review to PDF), there are four check boxes that the user must select to export the complete Oprex form (with review) as PDF.  
It is possible that one of the checkboxes could be missed and the resulting PDF may not have the complete export of the Oprex for review.  
To make this easier, we have the JS function below which will "check" all the checkboxes in given Oprex form.  
To create this button, go to bookmarks (favorites) in the browser and create a new one.    
Instead of providing a url, copy paste the below JS function into URL box. Save.  
```javascript
javascript:(function() {    document.querySelectorAll('input[type="checkbox"]').forEach(cb => cb.click());    })();
```
